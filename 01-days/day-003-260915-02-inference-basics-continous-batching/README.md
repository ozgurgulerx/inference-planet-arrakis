---
id: day-003
title: "Inference basics — Continuous batching"
day: 3
date: "2026-09-15"
items: []
---
### Batching
First, the batch can be thought of as N logical sequences/slots, each with its own token history and processed together in one model execution step:
A: [a1 a2 a3 ...]
B: [b1 b2 ...]
C: [c1 c2 c3 c4 ...]
D: [d1 ...]

For LLMs, instead of sending one sequence through the GPU:
Request A
"Explain TCP congestion control"
you process several sequences together:
Request A: "Explain TCP congestion control"
Request B: "Summarize this paragraph"
Request C: "Write Python code for..."
Request D: "What is KV cache?"
The GPU sees them as one larger tensor operation rather than four completely separate tiny operations.

batch
→ batch dimension
→ attention mask
→ causal attention
→ KV cache per sequence
→ decode step
→ continuous batching

#### Batch Size
Batch size = number of sequences being processed together

batch_size = 1
[A]

batch_size = 4
[A, B, C, D]

batch_size = 32
[A, B, C, ...]

We need to note that concurrency != batch size.
If you have 100 incoming requests, this does not mean batch size = 100.
It may as well be 100 requests in system but end up as...
30 currently scheduled
70 waiting

concurrency = number of requests currently in-flight / offered workload
scheduled sequences = sequences selected for this iteration


### Attention
They are processed together, but attention is constrained so A only attends to A, B only to B, etc.

        Keys/Values
        A   B   C   D
Query A ✓   ✗   ✗   ✗
Query B ✗   ✓   ✗   ✗
Query C ✗   ✗   ✓   ✗
Query D ✗   ✗   ✗   ✓

So for decode step 17 you can imagine:
A: KV cache for A1...A16  + query for A17
B: KV cache for B1...B16  + query for B17
C: KV cache for C1...C16  + query for C17
D: KV cache for D1...D16  + query for D17

Then the model produces one next token for each active sequence:
A -> next token
B -> next token
C -> next token
D -> next token


Now suppose C finishes:
A -> continues
B -> continues
C -> EOS, finished
D -> continues

In a naive fixed batch, you could conceptually have:
[A, B, empty, D]
and keep going until the others finish.
In continuous batching, the scheduler says:
C finished
↓
put request E into available capacity
↓
[A, B, E, D]

Batch = set of currently active independent sequences. Each sequence has its own growing KV-cache/history (We need to note here physically, modern engines often maintain a shared KV-cache memory pool and allocate blocks/pages to sequences). At every decode iteration, the GPU advances the active sequences, typically producing one token per sequence. Some sequences finish earlier, and continuous batching lets new work replace them.

### Continous Batching
Then continous batching is simply...
A live GPU batch dynamically inserts / removes requests as sequences finish.
Please note continuous batching thinks at the iteration/token level, not at the request level...

Step 1: [A, B, C]
Step 2: [A, B, C]
C finishes
Step 3: [A, B, D]   <- new request D immediately enters
A finishes
Step 4: [E, B, D]   <- E enters

Continuous batching is also called iteration-level or in-flight batching.
The scheduler can reconsider the batch at every decoding iteration rather
than waiting for the entire batch to finish.

#### Why Continous Batching
Without continuous batching, you get poor GPU utilization because requests have very different output lengths. A short request can finish while a long request keeps the batch occupied.

#### How much can you increase the batch size?

The practical max batch size is constrained by:
- GPU memory — especially KV cache memory.
- Model size — larger models leave less VRAM for active sequences.
- Context length — longer sequences need much larger KV caches.
- Precision / quantization — FP16/BF16 vs FP8/INT8 changes memory pressure.
- Serving engine — vLLM, TensorRT-LLM, SGLang, etc. manage memory differently.
- Latency target — even if batch size 256 fits, it may hurt per-user latency.

So the real question is not:
“What is the maximum batch size?”
but:
“How many active sequences can I keep concurrently while meeting my latency/SLO target?”
A simplified memory picture is:

GPU VRAM = model weights + KV caches for active sequences + activations / workspace + runtime overhead

### Chunked prefill
When a large prefill request arrives, the scheduler may use chunked prefill so that the entire prompt does not monopolize one scheduling iteration. Instead, the prompt is processed in chunks while decode work for existing requests continues. This helps balance TTFT for new requests against TPOT / inter-token latency for requests already decoding.



#### How is it configured in practice?

- **max_num_seqs** = maximum number of active sequences/requests the scheduler can process in one scheduling cycle.
- **max_num_batched_tokens** = maximum total number of tokens the scheduler can include in that cycle.
- **KV cache capacity** = how much sequence history can physically fit in memory.
- **concurrency** = how many requests are active/in-flight overall.

So imagine:
max_num_seqs = 16
max_num_batched_tokens = 2048
You might have:
16 requests × 1 decode token each = 16 batched tokens
during decode.
But during prefill:
Request A = 1000 prompt tokens
Request B = 800 prompt tokens
Request C = 600 prompt tokens
That totals 2400 tokens, so the scheduler cannot process all three prefills together if:
max_num_batched_tokens = 2048
It has to split/chunk/schedule them across iterations.
So modern serving is less like:
batch_size = 16 forever
and more like:
Every iteration:
  choose some active sequences
  subject to:
    sequence-count limit
    token-count limit
    KV-cache availability
    scheduling policy
That is the key shift.
And yes, if you change these parameters, you can absolutely change TTFT, throughput, TPOT, and memory usage.
For example:
higher max_num_seqs
→ potentially more concurrency
→ potentially higher throughput
→ potentially worse latency
→ more KV cache pressure
and:
higher max_num_batched_tokens
→ larger prefill/decode work per iteration
→ better GPU utilization up to a point
→ possibly longer scheduling steps / worse latency
So the "batch" in modern inference is really dynamic and scheduler-defined, not just a fixed N like classic ML batching.


#### Inference Tuning
So inference tuning is basically: **how much useful work can I keep on the GPU before latency or memory becomes unacceptable**?


### References:

[Anyscale - Continous Batching](https://www.anyscale.com/blog/continuous-batching-llm-inference)
