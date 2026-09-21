# T1.01 — LLM Inference Lifecycle

For a warmed, ordinary autoregressive request, the serving path is:

**validate and render → tokenize and budget → queue and schedule → prefill uncached positions → select the first output → repeat decode and selection → finish and release active state.**

Prefill is model execution, and KV state is written during that execution. Sampling is part of the generation loop, including selection of the first output from prefill logits. Prefill often has greater compute intensity; small-batch decode often faces memory-bandwidth pressure. These are workload-dependent tendencies, not universal bottleneck labels.

The goal is to trace one vLLM request and measure preprocessing, waiting, prefill, decode and delivery separately. The model is assumed loaded and ready; checkpoint loading, compilation and warm-up are separate startup costs. Transformer internals belong in T1.02.

An interactive technology atlas is maintained alongside these notes in the local workspace. The existing tokenizer and BPE explanations follow the lifecycle sections below.

## Contents

- [The inference cycle](#the-inference-cycle)
- [Request processing and context budgets](#request-processing-and-context-budgets)
- [Admission, scheduling and continuous batching](#admission-scheduling-and-continuous-batching)
- [Prefill, the first output and decode](#prefill-the-first-output-and-decode)
- [KV state, memory and resource ownership](#kv-state-memory-and-resource-ownership)
- [Selection, streaming and termination](#selection-streaming-and-termination)
- [Performance, measurement and diagnosis](#performance-measurement-and-diagnosis)
- [Modern extensions](#modern-extensions-which-lifecycle-assumption-changes)
- [Questions to explain, calculate and defend](#questions-to-explain-calculate-and-defend)
- [Tokenizer foundations and BPE](#tokenizer-foundations-and-bpe--existing-detailed-notes)

## The inference cycle

```mermaid
flowchart LR
  A[Validate request and render template] --> B[Tokenize and check budgets]
  B --> C[Queue and schedule]
  C --> D[Prefill uncached positions]
  D --> K[KV state]
  D --> E[Select next token]
  E --> F[Detokenize and stream eligible text]
  E --> G{Continue?}
  F --> G
  G -->|yes| H[Decode selected token]
  K --> H
  H --> K
  H --> E
  G -->|no| I[Finish and release active ownership]
```

This is a dependency diagram, not a guarantee that streaming and GPU execution run serially. EOS and token-count checks may stop generation before text delivery; text stop strings require output processing. Implementations can overlap these activities.

## Request processing and context budgets

A request includes more than its visible prompt: model identity, messages, tool definitions, generation settings and limits all influence the work. Validate the request, render the model's expected chat template, and tokenize the rendered input. Avoid adding special tokens twice if the template has already supplied them. [Hugging Face chat templates](https://huggingface.co/docs/transformers/chat_templating)

For an ordinary decoder-only request, distinguish three limits:

- **Context limit:** supported logical input plus generated positions.
- **Output limit:** the maximum requested generation, not a promise that this many tokens will be produced.
- **Physical state capacity:** how much concurrent state the runtime can hold after weights and other allocations.

For an illustrative 32,768-position context and a rendered input of 24,768 tokens, at most 8,000 output positions remain under a simple input-plus-output policy. Requesting 8,192 exceeds it by 192. Include system text, role markers and tool schemas in the input count. Multimodal models may add positions or separate state that a text tokenizer cannot count on its own.

A request can fit the context limit and still wait because the server lacks free capacity. Whether a runtime reserves the maximum future state at admission or allocates incrementally is an implementation policy; do not assume `max_tokens` immediately allocates that much KV. Generation and stopping controls are documented in [vLLM sampling parameters](https://docs.vllm.ai/en/latest/api/vllm/sampling_params/).

## Admission, scheduling and continuous batching

The waiting queue holds submitted work that is not currently scheduled. The scheduler chooses runnable work under sequence, token and state budgets. Keep these quantities separate:

| Quantity | What it counts |
|---|---|
| Client concurrency | Outstanding requests, including waiting requests |
| Active sequences | Sequences admitted to ongoing execution |
| Iteration token budget | Token positions scheduled in one iteration, potentially mixing prompt and decode work |
| KV capacity | Physical state that can be retained for active or reusable sequences |

Continuous batching revisits the batch at execution boundaries: finished work leaves, and new work can enter. The batch is not necessarily fixed until the longest request completes. More concurrent decode work can amortize weight movement and improve throughput, while longer iterations worsen per-request token latency. Allocation and sharing are central to efficient continuous serving. [PagedAttention paper](https://arxiv.org/abs/2309.06180)

Long prefills can interfere with ongoing generation. **Chunked prefill** limits the prompt work placed into an iteration and allows it to coexist with decode work. Smaller chunks may protect inter-token latency but delay prompt completion. The cited vLLM V1 tuning documentation describes a decode-prioritizing policy; pin the runtime version before assuming exact defaults. [vLLM optimization, v0.25.1](https://docs.vllm.ai/en/v0.25.1/configuration/optimization/)

Admission also needs an overload policy: unbounded waiting does not create capacity. Bound queueing, honor deadlines and cancel abandoned work. A retry is another request unless the service explicitly deduplicates it; retries can amplify overload.

## Prefill, the first output and decode

**Prefill** processes uncached prompt positions, computes their reusable state, and produces the logits from which the first output is selected. A large prompt may be processed in chunks; a compatible cached prefix may avoid some prompt computation.

**Decode** then consumes the previously selected output token with retained state, appends that token's KV, and produces the distribution for the next output. Selection can be greedy or use a configured sampling strategy. The next token depends on preceding selections, so ordinary single-sequence generation cannot freely parallelize all future output positions. [Generation strategies](https://huggingface.co/docs/transformers/generation_strategies)

### A worked trace: four prompt tokens, three outputs

Assume one warmed sequence, no prefix reuse, full-history attention, no speculative decoding and an output limit of three. Token labels are illustrative.

| Event | What is processed | KV positions computed | Output tokens selected |
|---|---|---:|---:|
| Prefill | `p1 p2 p3 p4` | 4 | 0 |
| First selection | Logits from the last prompt position select `y1` | 4 | 1 |
| First decode and selection | Process `y1`, then select `y2` | 5 | 2 |
| Second decode and selection | Process `y2`, then select `y3` | 6 | 3 |
| Termination | Output limit reached; no `y3` forward pass required | 6 before reclamation | 3 |

The distinction is between **selecting a token** and **processing that token into state**. Under these assumptions, N output selections require one prefill and N−1 later decode passes. A one-token answer can finish after prefill and selection. Speculative execution, multiple branches and extra implementation work change invocation accounting.

## KV state, memory and resource ownership

The cache retains prior attention keys and values so later steps can use them without recomputing their source positions. It is not a cache of final answers. Prompt execution writes initial state; subsequent execution extends it. Full-history, sliding-window and hybrid models retain different amounts and types of state. [Transformers cache strategies](https://huggingface.co/docs/transformers/kv_cache)

For a conventional full-attention cache with uniform dimensions, the raw state size is:

```text
KV bytes per retained token
= 2 × number of layers × KV heads per layer × head dimension × bytes per element
```

The factor two accounts for K and V. For an illustrative 32 layers, 8 KV heads, head dimension 128 and two-byte elements:

```text
2 × 32 × 8 × 128 × 2 = 131,072 bytes = 128 KiB per token
8,192 retained positions × 128 KiB = 1 GiB
```

This excludes block rounding, metadata, scales and allocator overhead. It assumes every layer retains every position. Per-device memory depends on sharding or replication. Query-head count must not be substituted for KV-head count when they differ.

The service also needs memory for weights, activations/workspace, graph captures, communication and runtime overhead. “The weights fit” therefore does not establish safe concurrency. Cached prefixes may share physical blocks, so summing each request's logical length can overcount shared physical state.

**Paged allocation** maps logical positions to physical blocks; it reduces allocation waste and supports sharing. It does not automatically reduce the amount of attention history read. When capacity is exhausted, the runtime may delay admission or preempt work. The cited vLLM V1 policy uses recomputation by default; do not assume every runtime swaps state to host memory. Watch preemption counts alongside latency. [PagedAttention](https://arxiv.org/abs/2309.06180), [vLLM preemption](https://docs.vllm.ai/en/v0.25.1/configuration/optimization/)

For a versioned vLLM example, the frontend handles input/output processing, the engine core schedules and manages KV, and workers execute the model. Correlate observations across these owners instead of treating the server as one opaque GPU call. [vLLM V1 architecture, v0.16.0](https://docs.vllm.ai/en/v0.16.0/design/arch_overview/)

## Selection, streaming and termination

The model supplies logits; the generation policy selects an ID. Greedy choice, temperature, top-p and permitted-token constraints are different policies over candidate outputs. Sampling does not create KV for the token it has just selected; that happens if the token is subsequently processed. [Generation strategies](https://huggingface.co/docs/transformers/generation_strategies)

Detokenization converts IDs into text. A token may represent only part of a Unicode character or text fragment, so one token need not equal one visible stream event. Network buffering and output-processing policy can further combine deliveries. Measure both engine token progress and client-visible arrival before interpreting a bursty stream. Tokenization can also normalize input, so exact byte-for-byte round trips depend on configuration. [Tokenization pipeline](https://huggingface.co/docs/tokenizers/pipeline)

Termination can result from EOS, a stop string, an output/context limit, cancellation, a deadline or an error. Record the reason. The server should stop scheduling finished requests and release their active references; reusable prefix blocks can remain cached until eviction. Logical completion, reference release and physical deallocation are different events.

A client disconnect should propagate cancellation to the serving engine. Check this path explicitly: a closed socket alone does not prove that GPU work stopped. Slow consumers also require bounded output buffering and an explicit backpressure or cancellation policy.

For tool use, a generation segment can end with a tool call while the application task remains unfinished. The tool result becomes input to a later segment. Distinguish segment latency, generated tokens and end-to-end task latency.

## Performance, measurement and diagnosis

Define timestamp boundaries before comparing metrics. Client measurements can include transport and buffering absent from an engine interval. TTFT is not simply “prefill duration.”

| Metric | Interpretation |
|---|---|
| Queue delay | Waiting between the chosen admission/engine-arrival boundary and scheduling |
| TTFT | Time from the stated request-start boundary to first output |
| ITL | Gaps between consecutive output observations; event/token definitions must be explicit |
| TPOT | Average post-first-output time per additional output token |
| E2E latency | Time from request start to the chosen completion boundary |
| Throughput | Requests or tokens completed per unit time, with input/output counting specified |
| SLO goodput | Useful work meeting the declared latency objectives; also check quality and errors |

With matched boundaries and N>1, `TPOT = (E2E − TTFT)/(N−1)`. If TTFT is 1 s, E2E is 3 s and N is 101, TPOT is 20 ms. This mean can hide a long individual stall. One-output requests have no inter-output interval; identify them separately. Metric definitions and collection boundaries are implementation-specific. [vLLM metrics](https://docs.vllm.ai/en/latest/design/metrics/)

Record model/tokenizer/runtime revisions, hardware and topology, precision, rendered input lengths, actual output lengths, arrival process, concurrency, cache state, warm-up, errors and task quality. Compare p50/p95/p99 latency alongside throughput. A closed-loop client submits replacement work only as requests finish; it can reduce offered load as latency rises, masking overload that a fixed arrival process would expose.

Prefill's many input positions often provide useful parallelism, while small-batch decode repeatedly moves weights and retained state. Nevertheless, long context, batch size, communication, CPU work and launch overhead can shift the bottleneck. Start from a hypothesis and use discriminating measurements. [DistServe](https://arxiv.org/abs/2401.09670)

| Observation | Plausible hypotheses | Evidence that distinguishes them |
|---|---|---|
| Client TTFT rises; engine prefill stays stable | Frontend work, queue growth, network/output delay | Correlated timestamps before and after each boundary |
| ITL spikes when long prompts arrive | Prefill interference, changed batches, preemption | Per-iteration token mix, execution duration and preemption events |
| GPU utilization is low while requests wait | CPU bottleneck, admission limit, communication wait | CPU timelines, runnable queue, capacity counters and device trace |
| More concurrency raises tokens/s but misses latency targets | Larger batches amortize work but lengthen iterations | Latency distributions and SLO goodput at the same request mix |
| High prefix hit rate gives little E2E improvement | Decode dominates, or saved work is small | Cached token counts, prefill savings and output-time share |

### Trace one request through vLLM

This is a proposed investigation, not a record of a completed experiment.

1. Pin model, tokenizer/template and runtime versions. Record hardware, configuration and whether the model is warm.
2. Send a bounded request with known rendered input length and an explicit output limit; carry a request ID through logging.
3. Capture frontend processing, engine waiting, prefill completion, output selections/delivery and finish reason. State which timestamps the runtime actually exposes; do not infer missing spans from unrelated averages.
4. Inspect execution/state observations alongside client timing. GPU profiling is asynchronous: use appropriate device timing or traces rather than interpreting host submission time as kernel duration.
5. Change one condition: input length, output length, offered load or prefix reuse. Predict which interval changes, then compare evidence and errors.
6. Repeat with a changed case that could overturn the explanation. For example, hold output length fixed while increasing the prompt, then hold the prompt fixed while increasing output.

## Modern extensions: which lifecycle assumption changes?

| Extension | Change to the baseline | Boundary to defend |
|---|---|---|
| Prefix caching | Reuse compatible prefix state instead of repeating its prefill | Does not directly remove later decode iterations; identity and prefix matching matter |
| Speculative decoding | Draft candidates and verify with the target; potentially accept several per target invocation | Draft overhead and low acceptance can eliminate gains; exactness depends on the verification algorithm |
| Prefill/decode disaggregation | Execute phases on different resources and transfer state | Transfer, coordination and queueing can exceed the interference avoided |
| Structured output | Restrict permitted choices using a supported grammar/schema | Valid syntax does not establish factual correctness or safe tool execution |
| Multimodal requests | Add media processing, encoder work and model-specific representations | Text length alone cannot describe processing/state demand |
| Hybrid/windowed models | Retain different state by layer/type | The uniform full-history cache formula no longer applies unchanged |
| Reasoning and tool turns | Extend or repeat generation segments within a task | Visible answer length can differ from total generated work; budget and measure the whole task |

Sources: [prefix caching](https://docs.vllm.ai/en/latest/features/automatic_prefix_caching/), [speculative decoding](https://arxiv.org/abs/2211.17192), [DistServe](https://arxiv.org/abs/2401.09670), [structured outputs](https://docs.vllm.ai/en/latest/features/structured_outputs/), [hybrid state management](https://docs.vllm.ai/en/latest/design/hybrid_kv_cache_manager/). These are optional branches, not prerequisites for every deployment.

## Questions to explain, calculate and defend

- A one-token response finishes after prefill. What state exists, and why is no subsequent decode pass required?
- A prompt fits the context window but remains queued. Which other capacities could prevent admission?
- Under the worked cache assumptions, what happens to raw state when retained positions double? What changes if blocks are shared?
- TTFT improves after prefix caching, but E2E barely changes. What workload would explain this?
- Lowering the iteration token budget improves ITL but worsens TTFT. Trace the scheduling trade-off.
- A stream stalls while device execution remains regular. Which measurements could falsify a GPU explanation?
- Under what workload and topology would you reject prefill/decode disaggregation?

The calculations and traces above are illustrative, not measured production results. Primary sources were reviewed on 20 September 2026; versioned examples should not be treated as universal defaults, and rolling documentation can change.

## Tokenizer foundations and BPE — existing detailed notes

The following notes retain the original detailed explanation. Token IDs, segmentations and comparative corpus counts are illustrative unless a tokenizer/checkpoint is explicitly specified. Vocabulary size alone does not guarantee shorter encodings across unrelated tokenizers; corpus, algorithm and normalization matter.

There are three different kinds of decisions for the training, and separating them clears up most of the confusion:

- Tokenizer design/training decides what the discrete symbols are.
- Model architecture design decides things like hidden size / embedding width.
- LLM training learns the actual numerical values of the embeddings and transformer weights.

Tokenizer training learns which discrete pieces of text deserve vocabulary entries. Model architecture determines how wide each token's internal representation will be. LLM training then learns what those representations should contain.



### Tokenizer

An LLM cannot directly operate on text. Neural networks perform mathematical operations on numbers—vectors, matrices, tensors—so before a prompt can enter the model, the text has to be converted into a numerical representation. We use tokenisers to map text into token IDs. The model then performs an embedding lookup to obtain vectors.

Each model comes with a vocabulary.

A tokenizer segments text into vocabulary pieces and returns their integer IDs.

(*A tokenizers' encoder segments a string into vocabulary entries and returns integer IDs; tokenizers' decoder maps those IDs back to byte or text pieces so it is two-way.*)

The whole flow is something like this...

```text
text -> tokenizer -> tokens -> token IDs -> embedding lookup -> vectors -> transformer
```

(*The token embedding lookup is static, but the representation entering and moving through the transformer can depend on position and rapidly becomes context-dependent.*)

A pretrained tokenizer and a pretrained model checkpoint should be thought of as one coupled system. The vocabulary, token IDs, embedding matrix, and model weights must agree about what each token ID means.
This also explains why simply replacing a model's tokenizer with a “better” tokenizer usually does not work. Even if both tokenizers contain exactly 100,000 tokens, the ID-to-token mapping may be completely different.

For inference, the safe mental model is therefore:

```text
Tokenizer + vocabulary + model weights form a versioned interface.
Changing one of them can change the meaning of the numerical input the model receives.
```

#### Token Identity is not Token Position
The embedding lookup tells the model **what token this is**, but the model also needs information about **where that token occurs in the sequence**.

Consider:

```text
dog bites man
```

and:

```text
man bites dog
```

These sentences contain the same three words, but their order completely changes the meaning.

The token embedding for `"dog"` is initially the same whenever that token ID is looked up. Something else therefore has to tell the transformer whether `"dog"` appears at position 1, position 3, or position 5,000.

Transformers use positional mechanisms for this purpose.

Different model architectures implement this differently. Some architectures historically added positional embeddings directly to token embeddings. Many modern LLMs instead use mechanisms such as rotary positional embeddings, or **RoPE**, inside the attention computation.

We do not need to understand the details yet.

For now, the important distinction is:

```text
token embedding
→ tells the model which token this is

position mechanism
→ tells the model where the token is
```

The transformer then combines token identity, position, and surrounding context to create contextual representations.


#### Creating a tokenizer

Let's start with how to create a tokenizer...
When designing a tokenizer, one of the most important decisions is how large the vocabulary should be. This is closely related to how much text each token is allowed to represent. At one extreme, tokens can be very small—individual characters, bytes, or short character sequences. At the other extreme, tokens can be much larger—whole words, common phrases, or long recurring subword units. Neither extreme is ideal, so tokenizer design is fundamentally about finding a useful compromise between vocabulary size and sequence length.

It is also important to mention not every token represents ordinary user-visible text. LLM vocabularies usually also contain special tokens that help structure the conversation.

Examples can include tokens representing concepts such as:

```text
<BOS>        beginning of sequence
<EOS>        end of sequence
<system>     system message
<user>       user message
<assistant>  assistant message
<tool>       tool call or tool result
```

The exact tokens differ between model families.
This matters because when we send a chat request such as:

> Hello, how are you?

the model may not actually receive only those words.
A chat template may transform the conversation into something conceptually like:

```text
<system>
You are a helpful assistant.
<user>
Hello, how are you?
<assistant>
```

The tokenizer then tokenizes this entire formatted sequence, including the special markers.
So the real pipeline is closer to:

```text
user messages
↓
chat template
↓
formatted text + special tokens
↓
tokenizer
↓
token IDs
↓
embedding lookup
```

The chat template and tokenizer therefore work together to define the actual sequence presented to the model.

This becomes especially important when serving instruction-tuned models. Two models may understand the same human language but expect completely different conversation formats. Using the wrong chat template can significantly damage model behaviour even when the underlying model weights are correct.

From an inference perspective, special tokens also consume token positions. The user may see a short prompt, while the actual model input includes system instructions, role markers, tool descriptions, and other formatting added before tokenization.

#### Using Small Tokens

If we use very small tokens, the vocabulary itself can remain small. For example, if a system worked mostly at the byte level, it would only need a few hundred fundamental symbols to represent arbitrary text. This is attractive because the model does not need a huge embedding table or a huge output vocabulary. If the hidden dimension is 4,096, then a vocabulary of 32,000 tokens requires roughly 32,000 × 4,096 embedding parameters, while a vocabulary of 200,000 tokens requires far more. A small vocabulary therefore reduces the amount of model capacity spent simply on representing possible tokens.

Small tokens also have excellent coverage. The tokenizer rarely encounters something it fundamentally cannot represent, because unusual words, names, code, URLs, spelling mistakes, or new terminology can simply be decomposed into smaller units. In that sense, small-token vocabularies are very robust.

The downside is that small tokens produce long token sequences. Suppose the word internationalization can be represented as one or two large tokens by one tokenizer, but requires twenty character-like tokens in another. The second tokenizer has converted exactly the same text into a much longer sequence.

That matters directly for inference. If a prompt becomes 30% longer in token space, the model has more tokens to process during prefill. More prompt tokens also produce more KV-cache entries, consume more of the context window, and leave fewer KV-cache slots available for other concurrent requests. Even though the vocabulary itself is smaller, the serving workload becomes larger.

So the small-token extreme can be summarized as:

```text
Small tokens
    ↓
Small vocabulary
    ↓
Smaller embedding/output vocabulary structures
    ↓
BUT
    ↓
More tokens per piece of text
    ↓
Longer sequences
    ↓
More prefill work + more KV-cache usage
```

#### Using Long Tokens

At the opposite extreme, we could make tokens very large. Imagine creating one token for every common word, technical term, name, URL fragment, programming construct, and perhaps even frequent phrases. Text would then compress into relatively few tokens. A sentence that requires twelve subword tokens under one tokenizer might require only six large tokens under another.

This is attractive from the serving perspective because shorter sequences generally mean less work. The model sees fewer prompt tokens, prefill requires fewer token positions, less KV-cache memory is needed, and a fixed context window can contain more original text.

However, this comes at the cost of a much larger vocabulary. Every vocabulary entry needs an associated token ID, and the model needs to be able to represent that token internally. With a conventional embedding matrix, the number of embedding parameters is approximately:

vocabulary_size × hidden_size

This counts one conventional embedding table. If the input embedding and output projection share weights, do not count them twice when estimating parameter memory.

If the hidden dimension is 4,096, increasing the vocabulary from 32,000 to 128,000 increases the embedding table from roughly 131 million parameters to roughly 524 million parameters. Moving to a 256,000-token vocabulary pushes it beyond one billion embedding parameters.

There is often also an output-side cost. At the end of the transformer, the model produces a hidden state and must calculate scores for possible next tokens. If the vocabulary contains 150,000 possible tokens, the output layer must ultimately produce around 150,000 logits. A larger vocabulary therefore increases memory footprint and can increase output-projection and sampling-related work.

Importantly, this cost does not arise because the token IDs themselves become numerically larger. Token ID 180000 is not intrinsically more difficult for the model than token ID 12. Both are just indexes. The cost comes from having 180,000 possible vocabulary entries rather than, say, 32,000. The model needs parameters and output capacity corresponding to that larger set.

Very large vocabularies also risk wasting capacity on rare tokens. If a tokenizer creates a dedicated token for an unusual word that appears only a few hundred times in a massive training corpus, the model has allocated an entire embedding vector to something it has very little opportunity to learn well. A smaller subword vocabulary could instead compose the same word from reusable pieces that occur in many different contexts.

The large-token extreme can therefore be summarized as:

```text
Large tokens
    ↓
Large vocabulary
    ↓
Fewer tokens per piece of text
    ↓
Shorter sequences
    ↓
Less prefill + less KV-cache usage
    ↓
BUT
    ↓
Larger embedding/output structures
    ↓
More parameters + more vocabulary-side computation
```

Modern LLM tokenizers try to sit between these two extremes. They typically use **subword units**. Common words or frequently recurring fragments may become individual tokens, while unusual words are decomposed into smaller pieces.

For example, a tokenizer might represent a common word such as:

`"computer"`

as one token, while representing a less common word such as:

`"computerization"`

as:

`["computer", "ization"]`

An even rarer string might be split further. This allows the tokenizer to obtain much of the sequence-length benefit of large tokens without requiring a unique vocabulary entry for every possible word.

This is why tokenizer training can be thought of partly as a compression problem. Given a fixed vocabulary budget, the tokenizer tries to discover recurring pieces of text that are valuable enough to deserve their own token.

The designers might decide to evaluate candidate vocabularies of 32K, 64K, 128K, and 256K tokens. They then train candidate tokenizers on a representative corpus and measure how efficiently each tokenizer represents the languages and domains the model is intended to handle.

Suppose the same dataset produces:

| Vocabulary | Tokens required |
|---|---:|
| 32K | 1,000,000 |
| 64K | 850,000 |
| 128K | 770,000 |
| 256K | 745,000 |

The first increases in vocabulary size provide substantial compression. Moving from 32K to 64K saves 150,000 tokens. Moving from 64K to 128K saves another 80,000.

But moving from 128K to 256K saves only another 25,000 tokens while doubling the vocabulary size.
That is the point where the designer may conclude that the additional vocabulary is no longer worth its cost.

The decision therefore looks less like:
“128K feels like a good vocabulary size.” and more like:

“Increasing vocabulary from 64K to 128K materially improves sequence compression across our important languages, while increasing it from 128K to 256K produces only marginal improvement but substantially increases vocabulary-dependent model parameters.”

Language coverage makes this even more important. An English-heavy tokenizer may represent English very efficiently while breaking Turkish, Finnish, Arabic, Chinese, or code into unnecessarily small fragments. A multilingual model therefore has to evaluate tokenizer efficiency independently across its important languages.

For example, consider:

`çalıştırabileceklerimizden`

One tokenizer might split this into many small fragments, while another tokenizer trained with substantial Turkish data might have learned useful recurring stems and suffixes. If the second tokenizer represents Turkish text using 25% fewer tokens, that translates directly into less prefill work, lower KV-cache consumption, and more useful text fitting into the context window.

This gives tokenizer designers several competing objectives. They want good compression, but they do not want an enormous vocabulary. They want strong coverage of many languages and domains, but they do not want millions of extremely rare tokens. They want shorter sequences, but they also want the embedding and output layers to remain manageable.

The practical objective is therefore not simply to minimize vocabulary size or minimize token count independently. It is to find a vocabulary size for which the marginal benefit of adding more tokens to the vocabulary begins to fall below the marginal model and serving cost.

A useful mental model is:

```text
              Smaller vocabulary
                     ↑
                     │
      cheaper vocabulary structures
                     │
                     │
                     │
longer sequences ────┼──── shorter sequences
                     │
                     │
                     │
       better text compression
                     ↓
               Larger vocabulary
```

The tokenizer designer is trying to find a useful position in the middle.

From an inference-engineering perspective, the most important consequence is that tokenizer design changes the effective workload presented to the GPU. Two models can receive exactly the same human-readable prompt but process very different numbers of tokens because their tokenizers differ.

If one tokenizer produces 2,000 tokens and another produces 2,600 tokens for the same document, the second model receives a 30% longer prompt in token space. That affects prefill latency, KV-cache memory, batching capacity, context utilization, and often API economics.

So vocabulary size is not merely an NLP preprocessing detail. It is one of the architectural choices that connects the training corpus directly to the eventual serving characteristics of the model.

The core trade-off is:

- Small vocabulary: cheaper vocabulary representation, but longer sequences.
- Large vocabulary: shorter sequences, but more expensive vocabulary representation.
- Modern subword tokenization exists largely to find the useful middle ground between those two extremes.




The tokenizer performs the first part of this conversion.

At a simplified level:

**text → tokens → token IDs**

Suppose we write:

> `The cat is sleeping`

A tokenizer might split this into something conceptually like:

> `["The", " cat", " is", " sleeping"]`

Each token exists in a fixed **vocabulary** associated with that model. We can think of this vocabulary as a dictionary mapping token strings to integer IDs:

```text
"The"       → 791
" cat"      → 8415
" is"       → 374
" sleeping" → 16732
```

The prompt therefore becomes a sequence of integers:

```text
[791, 8415, 374, 16732]
```

These integers are **token IDs**. They are essentially indexes into the model's vocabulary.

There is an important distinction here: the token IDs are **not yet the vectors that the transformer works with**. The next stage performs an embedding lookup. Each token ID selects a learned vector from the model's embedding matrix.

Conceptually:

```text
text
  ↓
tokenizer
  ↓
tokens
  ↓
token IDs
  ↓
embedding lookup
  ↓
vectors
  ↓
transformer
```

So tokenization answers a fundamental question:

**How should arbitrary human text be divided into a finite set of symbols that a neural network can efficiently represent?**

This turns out to involve an important engineering trade-off.

### Extreme 1 — Very small tokens

At one extreme, we could make tokens extremely small. For example, we could tokenize individual characters:

```text
"cat"

→ ["c", "a", "t"]
```

Or, going even further, represent text using bytes.

The advantage is that the vocabulary can be very small. Instead of needing tens or hundreds of thousands of vocabulary entries, we might need only hundreds.

A small vocabulary means smaller vocabulary-dependent model structures, particularly the **embedding matrix** and usually the final **LM head** that converts hidden states back into vocabulary logits.

But there is a major downside.

Small tokens mean that the same piece of text requires **many more tokens**.

For example:

```text
"internationalization"
```

might require roughly twenty character tokens instead of perhaps two or three larger subword tokens.

That makes the sequence longer.

And sequence length is extremely important for inference.

More tokens mean:

```text
more prompt tokens
→ more prefill computation
→ more KV-cache entries
→ larger KV-cache memory consumption
→ longer contexts for attention
→ potentially higher latency and lower throughput
```

So although the vocabulary is cheap, the **sequence becomes expensive**.

This gives us one side of the tokenizer trade-off:

> **Smaller vocabulary → smaller tokens → longer sequences.**

### Extreme 2 — Very large tokens

Now consider the opposite extreme.

Suppose we created one token for every possible word:

```text
"cat"             → token
"dog"             → token
"international"   → token
"internationally" → token
"internationalization" → token
...
```

Whole sentences or common phrases could theoretically become tokens too.

The advantage would be extremely short sequences.

For example:

```text
"The cat is sleeping"
```

could theoretically become four tokens rather than perhaps six or eight smaller subword tokens.

Shorter sequences are attractive for inference because fewer tokens generally mean less transformer work and less KV-cache usage.

But now we have created the opposite problem: the **vocabulary explodes**.

Human language can construct enormous numbers of words, word forms, names, numbers, URLs, code fragments, spelling variations and combinations.

If every one required its own token, we might need millions or even billions of vocabulary entries.

The problem here is not that the resulting token IDs are numerically larger.

For example:

```text
token ID = 42
```

versus

```text
token ID = 428,392
```

makes essentially no difference by itself. Both are simply integer indexes.

The real problem is that a larger vocabulary means the model needs parameters corresponding to more possible tokens.

Consider the input embedding matrix.

If:

```text
V = vocabulary size
D = model hidden dimension
```

then the embedding matrix is approximately:

```text
V × D
```

If the vocabulary contains 32,000 tokens and the hidden dimension is 4096:

```text
32,000 × 4,096
≈ 131 million parameters
```

If we instead used a vocabulary of 1,000,000 tokens:

```text
1,000,000 × 4,096
≈ 4.1 billion parameters
```

We have potentially added billions of parameters simply to represent the vocabulary.

There is a similar issue at the other end of the transformer. The model produces a hidden state and must turn it into scores—**logits**—for possible next tokens.

Conceptually:

```text
hidden state
     ↓
LM head
     ↓
one score for every vocabulary token
```

With a 150,000-token vocabulary, the model ultimately needs to consider approximately 150,000 possible token outputs.

So increasing vocabulary size has real memory, parameter and compute consequences.

There is also a statistical problem: extremely specific tokens may occur too rarely in the training corpus for the model to learn good representations for them.

So the other side of the trade-off becomes:

> **Larger vocabulary → larger tokens → shorter sequences, but larger vocabulary-dependent model structures and potentially poorer statistical efficiency.**

### The tokenizer therefore has to find a compromise

We would ideally like to avoid both extremes.

We do not want:

```text
tiny vocabulary
+
tiny tokens
+
very long sequences
```

but we also do not want:

```text
enormous vocabulary
+
very large tokens
+
huge embedding/output matrices
```

Modern LLM tokenizers therefore tend to use **subword units**.

A tokenizer might know common words as individual tokens:

```text
"the"
"computer"
"London"
```

while breaking less common words into reusable pieces:

```text
"tokenization"

→ ["token", "ization"]
```

and unusual strings into even smaller pieces when necessary.

This gives the tokenizer a reusable vocabulary capable of representing essentially arbitrary text without requiring every possible word to exist independently in the vocabulary.

We can think of this as a compression problem:

> **Find a vocabulary that represents the training distribution using reasonably few tokens without making the vocabulary itself excessively large.**

That trade-off matters directly to inference engineering.

### Why inference engineers care about tokenization

Imagine two tokenizers processing exactly the same 10 KB document.

Tokenizer A produces:

```text
2,000 tokens
```

Tokenizer B produces:

```text
3,000 tokens
```

Even though the user supplied exactly the same text, the model running Tokenizer B now sees a prompt that is **50% longer in token space**.

That can affect:

* prefill latency,
* prefill FLOPs,
* KV-cache consumption,
* maximum effective context length,
* batching capacity,
* throughput,
* and API cost when pricing is token-based.

This is why characters, words, bytes and tokens must not be treated as interchangeable units.

A model's context window is not normally:

```text
128,000 words
```

or:

```text
128,000 characters
```

It is:

```text
128,000 tokens
```

How much actual text fits inside that context therefore depends partly on the tokenizer.

### A useful mental model

The tokenizer sits at the boundary between the human representation of language and the model's discrete representation of language:

```text
HUMAN WORLD

"The cat is sleeping."

        ↓ tokenization

DISCRETE MODEL WORLD

[791, 8415, 374, 16732, 13]

        ↓ embedding lookup

CONTINUOUS VECTOR WORLD

[
  vector(token 791),
  vector(token 8415),
  vector(token 374),
  ...
]

        ↓

Transformer
```

The tokenizer itself does **not understand the semantic meaning** of the text in the way the transformer eventually learns to represent it.

Its primary job is to efficiently map arbitrary input text onto a finite vocabulary.

The key optimization problem is therefore:

> **How large should the vocabulary be, and how much text should each token represent?**

Too small a vocabulary produces too many tokens.

Too large a vocabulary produces expensive vocabulary structures and many overly specific or rarely observed tokens.

Modern tokenizers occupy the middle ground by learning useful recurring subword units from data.

Exactly **how** they determine those units—BPE, byte-level BPE, unigram tokenization and related approaches (with toolkits such as SentencePiece supporting multiple algorithms)—is the next layer of the topic. Before going there, the important inference-engineering intuition is:

**Vocabulary size trades off against sequence length.**

And sequence length is not merely an NLP detail. It propagates directly into the economics and performance characteristics of LLM inference.




Tokenizer's encoder segments a string into vocabulary entries and returns integer IDs; its decoder maps those IDs back to byte or text pieces. The transformer never sees words or characters directly. It receives token IDs, plus masks and positions, and uses every ID as an index into the model's learned token embedding matrix. That matrix has one row per vocabulary item and one column per hidden dimension. Looking up an ID returns a dense vector, after which positional information and transformer blocks create contextual representations.

This links tokenization directly to the embeddings concept: changing the ID-to-piece mapping changes which embedding row is selected. A pretrained model and tokenizer are therefore a coupled artifact; swapping only the tokenizer makes familiar IDs mean different things and breaks the model even if both vocabularies have the same size. A useful tokenizer cannot simply store every word. Whole-word vocabularies fail on names, code, spelling variants, and new terms, while character-only vocabularies make sequences unnecessarily long.

BPE-style subword units choose the middle ground: frequent reusable chunks receive their own IDs, while rare strings decompose into smaller known pieces. Byte-level bases preserve open-vocabulary behavior because every input can fall back to bytes instead of an unknown token. The systems consequence is measurable. Token count consumes the context window, determines how much prefill and decode work the model performs, grows the KV cache, and is commonly the billing unit.

Before selecting a model for multilingual or code-heavy traffic, I would benchmark tokens per one thousand characters, p95 tokenization time, prompt fit, and output length on representative requests. That connects the tokenizer to the context-window and decoding concepts, not merely to preprocessing. The decision rule is simple: treat the tokenizer as a frozen model interface. Version it with the weights and chat template, test encode/decode round trips and special-token behavior, and compare fertility across the languages and domains that matter. If a tokenizer inflates representative traffic, the deployment pays in cost, context capacity, and latency even when model quality is unchanged.


Further reading: [vLLM V1 serving architecture](https://docs.vllm.ai/en/v0.16.0/design/arch_overview/).

During prefill, the model processes uncached prompt positions and writes their KV state; chunked prefill can spread this work across iterations. It has comparatively high parallelism across tokens and often behaves more compute-intensively.

During decode, each sequence usually produces one new token per iteration and repeatedly reads prior KV state. That changes the arithmetic intensity and commonly makes memory bandwidth much more important.

Under what conditions does a 10k-token prompt affect TTFT more than TPOT, and when might its later KV/attention cost make that intuition fail?

Why can many concurrent decoders improve GPU utilization while also worsening individual token latency?

From those, you should understand:

- queue delay
- TTFT
- inter-token latency / TPOT
- end-to-end latency
- generation throughput

### Subword tokenization
Subword tokenization also gives the tokenizer a useful escape route when it encounters unusual text.

Imagine the vocabulary contains common tokens such as:

```text
"computer"
"ization"
"HTTP"
"function"
```

If the tokenizer sees a familiar word, it may represent it using one or two large tokens.

But real input contains many things that could never all be stored individually in the vocabulary:

```text
new product names
usernames
random identifiers
URLs
spelling mistakes
Unicode symbols
source code
serial numbers
new words
```

The tokenizer therefore needs a way to represent strings it has never seen before.

One solution is to keep breaking the text into smaller pieces until known units are found.

Conceptually:

```text
very common string
→ one large token

less common string
→ several subword tokens

very unusual string
→ smaller character-like pieces

if necessary
→ byte-level pieces
```

Byte-level fallback, when supported and configured, is particularly powerful because arbitrary digital text can ultimately be represented as bytes. Not every subword tokenizer provides byte coverage; some use an unknown-token mechanism instead. The tokenizer therefore does not need a dedicated vocabulary entry for every possible word or Unicode string.

This is sometimes described as **open-vocabulary behaviour**. The tokenizer has a fixed vocabulary, but it can still represent text that was never explicitly stored as one vocabulary item.

The trade-off is that unusual text may require many more tokens than familiar text. A strange identifier or uncommon language fragment might therefore be much more expensive in token space than a common English word.


#### Measuring Tokeniser Efficiency

Because different tokenizers can turn the same text into very different numbers of tokens, tokenizer efficiency can be measured.

One common idea is **token fertility**: roughly speaking, how many tokens are required to represent some amount of text.

For example, we could compare two tokenizers on the same Turkish corpus:

```text
Tokenizer A
1,000 words → 1,450 tokens

Tokenizer B
1,000 words → 1,950 tokens
```

Tokenizer B has higher token fertility: it needs more tokens to represent the same content.

We can also measure the same idea using metrics such as:

```text
tokens per 1,000 characters
characters per token
bytes per token
tokens per document
```

There is no single perfect metric because languages behave differently. Chinese characters, Turkish morphology, English words, Python source code, JSON, and URLs all have very different structures.

This is why a tokenizer should be evaluated on the actual traffic the model is expected to serve.

For an inference system, we might benchmark:

```text
Turkish prompts
English prompts
code
JSON
RAG documents
tool-call payloads
long system prompts
```

and measure how many tokens each tokenizer produces.

This matters because tokenizer efficiency becomes serving efficiency.

If one tokenizer needs 30% more tokens to represent the same workload, that can mean more prefill computation, more KV-cache memory, less usable context capacity, and potentially greater serving cost.

So when comparing models for a real deployment, it is useful to ask not only:

**How good is the model?**

but also:

**How efficiently does its tokenizer represent our actual traffic?**

### BPE - Byte Pair Encoding

So far we have said that a tokenizer needs to find a useful middle ground between two extremes.

If tokens are too small, the vocabulary can remain small, but text turns into long token sequences. If tokens are too large, text compresses into fewer tokens, but the vocabulary becomes very large.

The obvious next question is:

**How does the tokenizer decide which pieces of text deserve to become tokens?**

One common answer is **Byte Pair Encoding**, or **BPE**.

BPE is essentially a **corpus-driven compression algorithm**. It examines a large body of training text and learns which sequences occur frequently enough that it is useful to represent them as single tokens.

Importantly, **training the tokenizer is completely separate from training the language model**.

There is no neural network involved in learning the BPE vocabulary. There is no gradient descent, loss function, backpropagation, or next-token prediction.

BPE simply examines text and counts patterns.

---

#### Starting from very small units

In **byte-level BPE**, the tokenizer begins with the 256 possible byte values.

This gives the tokenizer an extremely useful property: arbitrary digital text can already be represented before any larger tokens have been learned.

Conceptually, before learning any merges:

```text
text
↓
UTF-8 bytes
↓
individual byte-level symbols
```

This means the tokenizer does not need a special vocabulary entry for every possible word, name, language, emoji, identifier, or spelling variation.

Everything can ultimately fall back to bytes.

But representing everything as bytes would produce very long sequences.

BPE therefore tries to discover frequently occurring combinations of those smaller units and turn them into larger reusable tokens.

---

#### Learning frequent combinations

Imagine a tiny training corpus containing:

```text
low
lower
lowest
```

Initially, these words are represented using small symbols.

Very conceptually:

```text
l o w

l o w e r

l o w e s t
```

BPE now counts how frequently adjacent pairs occur.

For example:

```text
l + o
o + w
w + e
e + r
e + s
s + t
```

Because `l` followed by `o` occurs in all three words, it might be one of the most frequent pairs.

BPE can therefore create a new symbol:

```text
l + o
↓
lo
```

The corpus is then rewritten using that new symbol:

```text
lo w

lo w e r

lo w e s t
```

BPE counts adjacent pairs again.

Now:

```text
lo + w
```

appears repeatedly.

So another merge may be created:

```text
lo + w
↓
low
```

The corpus becomes:

```text
low

low e r

low e s t
```

The process continues.

---

#### Repeating the process builds the vocabulary

BPE repeatedly performs approximately the same operation:

```text
count adjacent symbol pairs
↓
find a very frequent pair
↓
merge that pair
↓
add the merged piece to the vocabulary
↓
rewrite the corpus
↓
count again
↓
repeat
```

Each merge creates a larger reusable symbol.

Over a huge training corpus, useful fragments begin to emerge naturally.

The resulting vocabulary might eventually contain pieces such as:

```text
"ing"
"tion"
"computer"
"HTTP"
"function"
"stanbul"
"ization"
```

These pieces were not manually selected by a linguist.

They became tokens because the training data made them useful.

This is what we mean when we say that a tokenizer is **trained on a corpus**.

It is learning which text fragments deserve some of the limited vocabulary budget.

---

#### Vocabulary size determines when we stop

Before training the tokenizer, the designers normally choose a target vocabulary size.

For example:

```text
128,000 tokens
```

The tokenizer begins with its base symbols and repeatedly adds learned merges until approximately that vocabulary budget has been reached.

So the simplified relationship is:

```text
base symbols
+
learned BPE merges
+
special tokens
≈
target vocabulary size
```

A larger vocabulary allows more frequent strings to become individual tokens.

A smaller vocabulary forces more strings to remain combinations of smaller pieces.

This connects directly to the vocabulary-size trade-off discussed earlier.

---

#### Frequent strings become large tokens

Suppose the training corpus contains the word:

```text
computer
```

millions of times.

The character or byte sequences that form "computer" are repeatedly seen together.
Over enough merge operations, BPE may eventually create:

`"computer"` as a single token.

Now consider something much rarer:

`computerizationally`

There may never be enough evidence to justify creating one token for the whole string.
Instead, the tokenizer might encode it as:

`["computer", "ization", "ally"]`

And an extremely unusual identifier might be divided even further:

`["AB", "91", "x", "7", ...]`

This gives BPE its useful middle-ground behaviour:

```text
very common text
→ larger tokens

less common text
→ subword tokens

very unusual text
→ small pieces / byte-level fallback
```

#### Training BPE produces two important artifacts

A useful simplified way to think about BPE training is that it produces two related artifacts.

The first is the vocabulary.

The vocabulary maps token pieces to IDs:

```text
"low"   → 4201
"er"    → 198
"est"   → 871
```

The second is the merge information describing which combinations were learned and their priority.

Conceptually:

```text
l + o   → lo
lo + w  → low
e + r   → er
e + s   → es
...
```

This ordering matters because later merged tokens can depend on earlier merges.
For example, we cannot create:

lo + w → low

unless:

l + o → lo

already exists.

The learned merge order therefore describes how increasingly large pieces were constructed.
Once tokenizer training finishes, these artifacts are frozen.
Encoding does not train anything.

This distinction is extremely important.

When a production request arrives, BPE does not examine the production traffic and start counting frequencies again.

The learning has already happened.

With ordinary deterministic encoding configured, tokenization applies fixed tokenizer artifacts. Optional sampled segmentation or BPE-dropout changes that assumption.

Conceptually:

```text
incoming text
↓
convert to base units
↓
apply learned BPE merges
↓
remaining pieces become tokens
↓
map tokens to token IDs
```

The tokenizer follows the merge rules learned during tokenizer training.

It does not invent new vocabulary entries.

It does not modify the merge table.

It does not learn from the user's request.

#### Merge priority matters

Suppose the tokenizer learned these merges:

1. l + o  → lo

2. lo + w → low

3. e + r  → er

When encoding text, multiple possible merges might sometimes exist.

BPE uses the learned merge ranking to determine which valid merge has priority.

This is why the tokenizer cannot simply know the vocabulary and independently guess how a string should be divided.

The merge rules are part of the tokenizer's behaviour.

Given the same tokenizer version, text and deterministic encoding settings, encoding should produce the same tokenization.

#### Decoding goes in the opposite direction

Encoding converts text into IDs:

```text
text
↓
BPE
↓
token pieces
↓
token IDs
```

Decoding reverses this:

```text
token IDs
↓
vocabulary entries
↓
byte/text pieces
↓
text
```

So the tokenizer participates at both ends of inference.

Before the transformer:

`text → IDs`

After generation:

`IDs → text`

#### BPE training is not LLM training

This is worth making completely explicit.

Tokenizer training might look like:

```text
training corpus
↓
count frequent symbol combinations
↓
learn BPE merges
↓
build vocabulary
↓
freeze tokenizer
```

There is:

- no gradient descent
- no backpropagation
- no neural network loss

Language-model pretraining begins afterwards.

The already-trained tokenizer converts the training corpus into sequences of token IDs:

```text
raw corpus
↓
frozen tokenizer
↓
token IDs
↓
LLM
↓
next-token prediction
↓
loss
↓
backpropagation
```

Now gradient descent updates:

- token embedding matrix
- attention weights
- MLP weights
- normalization parameters
- output weights

but the BPE vocabulary and merge rules normally remain fixed.

So these are two completely different types of learning:

Tokenizer training learns how text should be divided into discrete symbols.

LLM training learns how those symbols behave and relate to one another.

#### BPE connects directly to the embedding matrix

Suppose BPE produces a vocabulary containing:

`128,000 tokens`

and the model designers choose:

`hidden_size = 4096`

The model then needs an embedding table with approximately:

`128,000 × 4,096`

entries.

Each BPE vocabulary token owns one row in that embedding matrix.

For example:

```text
"computer"
↓
token ID 19482
↓
embedding_matrix[19482]
↓
4096-dimensional learned vector
```

This is another reason the tokenizer cannot casually be changed after model training.
The transformer has learned that embedding row 19482 corresponds to the token that the tokenizer assigned to ID 19482.
Changing that relationship changes what the model receives.

#### BPE also connects directly to inference efficiency

Tokenizer quality affects how many tokens a piece of text becomes.
Consider two tokenizers operating on the same document:

```text
Tokenizer A → 2,000 tokens
Tokenizer B → 2,800 tokens
```

The second tokenizer has created a 40% longer input sequence.
That means more prompt tokens need to be processed during prefill.

It also means:

- larger initial KV cache
- more context-window consumption
- less room for additional tokens
- more KV state for later decode steps to attend over

So how well BPE compresses the production workload eventually becomes a systems property.

#### BPE can favour some languages more than others
There is another important consequence of the way BPE learns its vocabulary.
BPE spends its limited merge budget on patterns that occur frequently in its training corpus.
Suppose the tokenizer-training corpus contains enormous amounts of English but relatively little Turkish.
English fragments such as:

```text
"the"
"ing"
"tion"
"computer"
```

may occur frequently enough to earn large reusable tokens.
Turkish fragments may occur less frequently simply because Turkish made up a smaller portion of the tokenizer-training data.
The tokenizer may therefore need more pieces to represent equivalent Turkish text.
For example, two sentences conveying roughly similar amounts of information might become:

```text
English
→ 14 tokens

Turkish
→ 20 tokens
```

This does not mean the Turkish sentence contains 43% more meaning.
It means the tokenizer represents that language less compactly.

#### UTF-8 can amplify this effect
Byte-level tokenizers ultimately operate over bytes.
Many common ASCII characters use one UTF-8 byte.
Other scripts and characters may require multiple bytes.
If useful multi-byte sequences have been learned as larger BPE tokens, this is not necessarily a problem.
But if a language or script was underrepresented during tokenizer training, it may fall back to smaller byte-level pieces more frequently.

This can further increase token counts.

#### Morphologically rich languages can introduce another challenge

Turkish, for example, can construct many surface forms from a common root:

```text
çalış
çalışmak
çalışıyor
çalıştırmak
çalıştırabilecek
çalıştırabileceklerimizden
```

A good tokenizer can discover reusable stems and suffix-like fragments.
A poorly matched tokenizer may split these forms much less efficiently.

#### This creates fertility asymmetry
We can describe this using token fertility.
If equivalent content requires substantially more tokens in one language than another, the tokenizer has different fertility across those languages.

For example:

```text
Language A
1,000 words → 1,300 tokens

Language B
1,000 words → 2,100 tokens
```

Language B now effectively pays more of the model's token budget to express comparable text.
That translates into real serving consequences:

- more context consumption
- more prefill computation
- larger KV-cache requirements
- potentially higher API cost

For generated output, inefficient tokenization can also mean that expressing the same amount of text requires more generated tokens, producing more serial decode iterations.

That last distinction is important: input token inflation primarily increases prefill and KV-related costs, while output token inflation can directly increase the number of decode iterations.

#### How I would evaluate a tokenizer

For a real production workload, I would not evaluate the tokenizer only on aggregate English benchmarks.
I would build representative held-out datasets for each important language and domain.

For example:

- Turkish conversation
- English conversation
- source code
- JSON
- RAG documents
- legal documents
- tool calls
- URLs and identifiers

Then measure metrics such as:

- tokens per 1,000 characters
- tokens per word
- bytes per token
- median token count
- p95 token count

The comparison should be performed separately by language and workload.

This lets us answer a much more useful question than simply asking how large the vocabulary is:

How efficiently does this tokenizer represent the traffic we actually expect to serve?

#### There is no universally optimal vocabulary

It would be tempting to optimize only for the lowest possible token count.

But that would push us towards an enormous vocabulary.

That creates its own costs:

- larger embedding matrix
- larger output vocabulary
- more model parameters
- more rarely trained vocabulary entries

So tokenizer design remains an optimization problem.

We are balancing:

```text
language/domain coverage

        ↕
text compression efficiency

        ↕
vocabulary size

        ↕
embedding/output memory and compute
```

The objective is not to make every language produce exactly the same number of tokens.

The objective is to obtain sensible compression across the workloads we care about without paying an unreasonable vocabulary cost.

#### The final mental model

The entire relationship can now be summarized as:

```text
TOKENIZER TRAINING

representative corpus
↓
start from small base units
↓
count frequent adjacent pairs
↓
learn BPE merges
↓
build fixed vocabulary
↓
freeze tokenizer


LLM TRAINING

raw training corpus
↓
frozen tokenizer
↓
token IDs
↓
embedding lookup
↓
transformer
↓
next-token prediction
↓
gradient descent
↓
learn embeddings + transformer weights


INFERENCE

user text
↓
same frozen tokenizer
↓
token IDs
↓
same learned embedding rows
↓
transformer
↓
generated token IDs
↓
same tokenizer decodes IDs
↓
text
```

The key idea is:

BPE learns the alphabet the model will speak in. LLM pretraining then learns what that alphabet means and how its symbols relate to one another.

Once LLM pretraining begins, that alphabet is effectively part of the model architecture.

## Practical labs

[Runpod lifecycle lab pack — L00–L35](labs/README.md): build, instrument, perturb and diagnose the request path, with a pinned environment and independent rebuild gates. Continue with the [chapter-by-chapter T1 Runpod route](../T1-LABS.md). Lab specifications are not completed experiments.
