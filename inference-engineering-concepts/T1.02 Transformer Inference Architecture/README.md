# T1.02 — Transformer Inference Architecture

A decoder-only Transformer is best understood as a **stateful numerical program executed repeatedly on an accelerator**:

```text
architecture
    → tensor shapes
    → kernels and collectives
    → weight, state and temporary bytes moved
    → prefill and decode behavior
    → latency, throughput, concurrency and cost
```

This chapter uses Qwen3-4B as a concrete reference, then generalizes to MoE, MLA, sparse attention and hybrid recurrent/attention models. The calculations are derived examples, not measurements. The linked labs remain planned until their evidence gates are satisfied; this page does not claim completed experiments or demonstrated mastery.

[All subjects](../SUBJECTS.md) · [Coverage and priorities](../COVERAGE.md) · [Research context](../RESEARCH_CONTEXT.md) · [Practical labs](labs/README.md)

## Contents

- [The executable mental model](#the-executable-mental-model)
- [Read a model configuration as a serving contract](#read-a-model-configuration-as-a-serving-contract)
- [A manual Qwen3-4B parameter and compute ledger](#a-manual-qwen3-4b-parameter-and-compute-ledger)
- [Attention is persistent state plus a read operation](#attention-is-persistent-state-plus-a-read-operation)
- [Prefill and decode are different workloads](#prefill-and-decode-are-different-workloads)
- [Position, normalization, MLPs and residuals](#position-normalization-mlps-and-residuals)
- [From model graph to GPU execution](#from-model-graph-to-gpu-execution)
- [Dense, MoE and distributed execution](#dense-moe-and-distributed-execution)
- [The architecture frontier is a state-design problem](#the-architecture-frontier-is-a-state-design-problem)
- [Changed case: equal parameters, unequal serving cost](#changed-case-equal-parameters-unequal-serving-cost)
- [How to analyze an unfamiliar architecture](#how-to-analyze-an-unfamiliar-architecture)
- [Questions and planned investigations](#questions-and-planned-investigations)
- [Practical labs and connections](#practical-labs-and-connections)
- [Primary sources](#primary-sources)

## The executable mental model

For inference, the more useful abstraction is not only \(y=f(x)\), but:

\[
(y_t,S_{t+1})=F(x_t,S_t;\theta),
\]

where \(\theta\) is the fixed model and \(S_t\) is request state: KV or recurrent tensors, positions, cache-block mappings and related metadata.

For one modern pre-norm decoder layer, the logical path is:

```text
residual x [T,H]
   │
   ├─ RMSNorm
   │    └─ packed QKV projection
   │         ├─ Q [T,Nq,D]
   │         ├─ K [T,Nkv,D] ─┐
   │         └─ V [T,Nkv,D] ─┴─ persistent state
   │              │
   │         Q/K norm + RoPE
   │              │
   │             GQA
   │              │
   │         output projection
   │              │
   └──────────── add
                  │
             RMSNorm
                  │
          gate and up projections
                  │
             SiLU(gate) × up
                  │
            down projection
                  │
   └──────────── add → next layer [T,H]
```

The residual stream is the fixed-width data bus between layers. Attention performs content-addressable reads from sequence state; the MLP applies parameterized per-token computation; both write updates back to the residual stream.

The diagram is a semantic specification, not a promise of separate kernels. A serving engine may pack projections, fuse normalization and residual work, write K/V directly into paged storage, and shard weights across devices.

## Read a model configuration as a serving contract

The released [Qwen3-4B configuration at revision `3101254`](https://huggingface.co/Qwen/Qwen3-4B/blob/3101254bbe4169895668a0e7653c3fd1f313576e/config.json) defines:

```text
H    hidden size          = 2,560
I    MLP width            = 9,728
L    layers               = 36
Nq   query heads          = 32
Nkv  key/value heads      = 8
D    head dimension       = 128
V    vocabulary           = 151,936
Smax declared positions   = 40,960
```

One important correction follows immediately:

\[
H=2560 \neq N_qD=32\times128=4096.
\]

`hidden_size = heads × head_dim` is common, not universal. Here the residual vector is projected into a wider query space, then the output projection maps attention width 4,096 back to residual width 2,560.

| Configuration field | Direct consequence | Serving question |
|---|---|---|
| \(H\) | residual width and major GEMM dimensions | How many weight bytes and FLOPs does each block require? |
| \(I\) | MLP matrix sizes | Does MLP work dominate attention projections? |
| \(L\) | sequential depth and state multiplier | How many launches, synchronization points and per-token state writes occur? |
| \(N_q\) | query width and attention work | How many independent query heads execute? |
| \(N_{kv}\) | K/V width | How many bytes does each retained token add? |
| \(D\) | per-head dot-product width | What are projection, attention and state dimensions? |
| \(V\) | embedding and LM-head width | What does full-vocabulary scoring cost? |
| position scheme | supported positional regime | Is a declared long context trained and implemented correctly? |
| layer types | state and kernel type per layer | Which layers actually grow KV state? |
| expert fields | resident versus active weights | What must fit, execute and cross the network? |

A declared context limit is not a capacity guarantee. Runtime memory, kernel support and quality at that length are separate questions.

## A manual Qwen3-4B parameter and compute ledger

For flattened active tokens \(T=B\times S\), the main Qwen3-4B projection shapes are:

```text
QKV:      [T,2560] × [2560,6144]
O:        [T,4096] × [4096,2560]
gate/up:  [T,2560] × [2560,9728] twice
down:     [T,9728] × [9728,2560]
LM head:  [U,2560] × [2560,151936]
```

Here \(U\) is the number of positions for which the runtime requests logits. In ordinary serving, prefill usually scores only the last prompt position per sequence; requesting prompt log-probabilities can make \(U\) much larger.

The packed QKV width is:

\[
(N_q+2N_{kv})D=(32+16)128=6144.
\]

Per layer:

\[
P_{QKV}=H(N_q+2N_{kv})D=15{,}728{,}640,
\]

\[
P_O=(N_qD)H=10{,}485{,}760,
\]

\[
P_{\text{SwiGLU}}=3HI=74{,}711{,}040.
\]

Therefore:

```text
attention projections/layer   26.214 M
SwiGLU projections/layer      74.711 M
large weights/layer          100.925 M
36 layers                  3,633.316 M
tied embedding/LM head       388.956 M
norm weights                   0.196 M
total                       4,022.468 M parameters
```

The MLP contains about 2.85 times as many large projection parameters as the attention projections in this block. Attention can dominate at long context, but it should not be assumed to dominate every layer's parameters or FLOPs.

At two bytes per BF16 parameter, raw weight storage is approximately:

\[
4.022468\text{B}\times2\approx8.045\text{ GB}=7.49\text{ GiB}.
\]

Qwen ties its input embedding and output LM-head matrix. Tying avoids a second 389M-parameter matrix; it does **not** remove the output projection.

For a matrix multiplication \([M,K][K,N]\), counting multiply-add as two operations:

\[
F_{\text{GEMM}}=2MKN.
\]

The Transformer blocks contribute about **7.27 GFLOPs per processed model token**. Each position sent through the full-vocabulary head adds about **0.778 GFLOP**. One ordinary decode step therefore approaches 8.04 GFLOPs per active sequence before context-dependent attention and small elementwise work. The familiar \(2P\) estimate is close in that decode case, but overstates ordinary prefill if logits are computed only at selected positions; it also fails for sparse experts, speculative verification and context-dependent attention.

### Do not call all allocated bytes “model memory”

| Memory class | Lifetime | Examples | Main scaling variable |
|---|---|---|---|
| Parameters | model lifetime | projections, embeddings, norm scales | parameter count and precision |
| Persistent request state | request/cache lifetime | K/V, latent KV, recurrent state | context and concurrency |
| Temporary activations | layer or iteration | Q, attention accumulators, MLP intermediates | active tokens |
| Runtime metadata | request/iteration | positions, block tables, slot maps | requests and cache blocks |
| Workspaces and graph pools | backend/capture lifetime | GEMM scratch, captured buffers | kernels and shape buckets |
| Communication buffers | collective lifetime | reductions, expert dispatch | parallelism and batch |

“The weights fit” does not imply that a useful serving workload fits.

## Attention is persistent state plus a read operation

For hidden states \(X\in\mathbb{R}^{T\times H}\):

\[
Q=XW_Q,\qquad K=XW_K,\qquad V=XW_V.
\]

For one head, causal attention is:

\[
\operatorname{Attn}(Q,K,V)=
\operatorname{softmax}\left(\frac{QK^\top}{\sqrt D}+M\right)V,
\]

where \(M_{ij}=0\) when key position \(j\le i\), and \(-\infty\) for future positions. Efficient kernels usually enforce that condition without materializing a full mask tensor.

### MHA, MQA and GQA change state width

| Mechanism | KV heads | Mapping |
|---|---:|---|
| Multi-head attention (MHA) | \(N_{kv}=N_q\) | each query head has its own K/V head |
| Multi-query attention (MQA) | \(N_{kv}=1\) | all query heads share one K/V head |
| Grouped-query attention (GQA) | \(1<N_{kv}<N_q\) | groups of query heads share K/V |

Qwen3-4B maps four query heads to each KV head. An efficient GQA kernel performs this mapping logically; it does not duplicate every K/V tensor four times.

For a uniform full-history cache, raw KV bytes per request are:

\[
\boxed{M_{KV}=2LSN_{kv}Db},
\]

where the leading 2 is K plus V and \(b\) is bytes per element. For \(C\) independent requests, multiply by \(C\) before adding page rounding, scale metadata, allocator overhead and any replicated state.

For Qwen3-4B BF16:

\[
2(36)(8)(128)(2)=147{,}456\text{ bytes}=144\text{ KiB}
\]

per retained token per request.

| Retained positions | Raw BF16 KV per request |
|---:|---:|
| 256 | 36 MiB |
| 2,048 | 288 MiB |
| 8,192 | 1.125 GiB |
| 16,384 | 2.25 GiB |
| 32,768 | 4.5 GiB |
| 40,960 | 5.625 GiB |

At 8K context, concurrency 8 implies **9 GiB of raw KV** before weights and runtime overhead. Prefix sharing or sharding can change physical per-device allocation; logical sequence lengths alone do not reveal that.

### Why K and V are cached but old Q is not

At decode step \(t\), the new token asks a new question with \(Q_t\):

\[
Q_t[K_0,\ldots,K_t]^\top
\quad\rightarrow\quad
P_t[V_0,\ldots,V_t].
\]

Earlier K/V projections are reusable because their layer inputs and model weights no longer change. Earlier queries answered earlier positions and are not needed for the new output. The cache stores per-layer projected attention state, not final answers or residual vectors.

### Allocation, reuse and computation are separate concerns

- **Paged KV** decides where logical state blocks live physically and limits fragmentation.
- **Prefix/radix caching** decides whether another request can reuse already materialized state.
- **An attention kernel** decides how Q reads state and computes the result.
- **FlashAttention** tiles exact attention to reduce HBM traffic; it does not make dense attention arithmetically linear.

Conflating these mechanisms produces incorrect memory and performance explanations.

## Prefill and decode are different workloads

| Property | Prefill | Ordinary decode |
|---|---|---|
| New positions per request | many known prompt tokens | one selected token |
| Projection shape | large \([BS,H][H,N]\) GEMMs | \([B,H][H,N]\), GEMV-like at small \(B\) |
| Attention | all prompt queries | one query scans retained state |
| State action | materialize prompt state | read history and append new state |
| Frequent pressure | compute and attention IO | weight/KV bandwidth, launches, communication |
| Primary user metric | TTFT and input throughput | TPOT/ITL and output throughput |

For dense attention, a full-square prefill bound is:

\[
F_{\text{prefill-attn}}\approx4BN_qS^2D,
\]

before causal-triangle savings. One decode token at history length \(t\) costs approximately:

\[
F_{\text{decode-attn}}\approx4BN_qtD.
\]

For Qwen3-4B at \(B=1,S=8192\), the full-square attention bound is about 39.6 TFLOPs across 36 layers; the causally useful triangle is roughly half. Dense block projections and MLPs across the same prompt are about 59.53 TFLOPs; scoring only the final prompt position adds about 0.778 GFLOP, while scoring every position would add about 6.37 TFLOPs. At this context, attention is important but does not automatically dominate all prefill work.

For one decode token at 8K history, attention adds about 4.83 GFLOPs across the layers and may inspect roughly 1.125 GiB of historical BF16 KV. The output is sequential even though each iteration contains fewer new FLOPs.

Treat “prefill is compute-bound” and “decode is memory-bound” as starting hypotheses. Short prompts, large decode batches, long contexts, quantization, launch overhead and distributed collectives can move either phase into a different regime.

## Position, normalization, MLPs and residuals

### RoPE: position changes the query-key geometry

RoPE rotates pairs of Q and K coordinates by position. Its useful property is:

\[
(R_pq)^\top(R_kk)=q^\top R_{k-p}k,
\]

so a query-key dot product depends naturally on relative displacement. V is not rotated. Changing a configuration's maximum position alone does not validate longer context: positional scaling, training distribution, attention cost and state capacity remain separate constraints. See [RoFormer](https://arxiv.org/abs/2104.09864).

### RMSNorm: a small operation that can still cost latency

\[
\operatorname{RMSNorm}(x)=
\gamma\odot
\frac{x}{\sqrt{\frac1H\sum_i x_i^2+\epsilon}}.
\]

RMSNorm omits LayerNorm's mean subtraction. Its FLOP count is small, but during decode a separate reduction and launch repeated twice per layer can matter. Production runtimes commonly fuse residual addition and RMS normalization.

Qwen3 also normalizes Q and K per head before RoPE. The exact trained operation order is part of model correctness, not an optional optimization.

### SwiGLU: three matrices, not one activation swap

\[
\operatorname{SwiGLU}(x)=
\left(\operatorname{SiLU}(xW_g)\odot xW_u\right)W_d.
\]

The gate and up projections can be packed into one wider GEMM; SiLU and multiplication can be fused. This is why a clean architecture diagram and a profiler trace can show different operation counts while computing the same model.

## From model graph to GPU execution

### Arithmetic intensity explains the phase change

\[
AI=\frac{\text{FLOPs}}{\text{bytes moved}},
\qquad
R_{\text{achievable}}\le\min(R_{\text{peak}},AI\times BW).
\]

For a batch-one BF16 matrix-vector multiply \([1,K][K,N]\), an idealized weight-dominated estimate is:

\[
AI\approx\frac{2KN\text{ FLOPs}}{2KN\text{ weight bytes}}=1\text{ FLOP/byte}.
\]

With \(M\) decode rows sharing the same fetched weights, the estimate approaches \(M\) FLOP/byte until activation traffic, caches and implementation overhead matter. Continuous batching therefore raises throughput by reusing weights, but waiting to form more work can hurt individual latency.

A useful first-order Qwen3-4B batch-one decode model at 8K context is:

```text
~8.0 GB model-weight bytes
+ ~1.125 GiB historical KV bytes
+ smaller activations, outputs and new-KV writes
```

This is not a DRAM measurement. Quantization, L2 reuse, sharding, fusion and layout can change the traffic materially.

### FLOPs are only one currency

For one output token, reason about:

\[
(F, B_w, B_s, B_c, N_k, N_{sync}),
\]

where \(F\) is arithmetic, \(B_w\) weight bytes, \(B_s\) persistent-state bytes, \(B_c\) communication bytes, \(N_k\) kernel launches and \(N_{sync}\) synchronization points.

- Quantized weights reduce \(B_w\), but may add dequantization work.
- GQA, MLA and KV quantization reduce \(B_s\) in different ways.
- MoE can reduce active \(F\) while increasing \(B_c\).
- Fusion reduces both intermediate traffic and \(N_k\).
- CUDA Graphs reduce repeated host launch work but require stable capture shapes and memory pools.

### Checkpoint graph is not execution graph

The [vLLM Qwen3 implementation at commit `f0c14b4`](https://github.com/vllm-project/vllm/blob/f0c14b4f776bb976e654b430442067539fbdb2ea/vllm/model_executor/models/qwen3.py) exposes several production transformations: packed tensor-parallel QKV, separate query/KV widths, per-head Q/K norm, RoPE, an attention backend and a row-parallel output projection.

Always distinguish:

```text
checkpoint tensor names
    ≠ local sharded weight layout
    ≠ runtime kernel graph
    ≠ physical cache layout
```

FlashAttention is another example. It computes exact attention with tiled Q/K/V loads and online softmax, avoiding full \(S\times S\) score and probability tensors in HBM. It reduces IO, not the standard attention arithmetic class. Decode needs a different geometry: one query, long paged/ragged K/V, grouped heads and partial reductions. See [FlashAttention](https://arxiv.org/abs/2205.14135).

## Dense, MoE and distributed execution

A dense MLP executes all of its weights for every token. An MoE router selects a small subset:

\[
y=\sum_{e\in\operatorname{TopK}(xW_r,k)}p_e\operatorname{MLP}_e(x).
\]

“Model size” is therefore insufficient. Track at least:

\[
(P_{\text{resident}},P_{\text{active}},P_{\text{shared}})
\]

plus expert placement. Total parameters govern storage; active parameters approximate arithmetic; locality, imbalance and routing determine weight traffic and communication.

| Parallel axis | Partitions | New systems cost |
|---|---|---|
| Tensor parallelism (TP) | heads or matrix dimensions | all-reduce/reduce-scatter; possible KV replication |
| Pipeline parallelism (PP) | layers | stage transfer, bubbles and added latency |
| Context parallelism (CP) | token history | exchange or circulate remote K/V/state |
| Expert parallelism (EP) | experts | token permutation and all-to-all dispatch/return |

An MoE can be compute-bound with large expert batches, HBM-bound with small expert batches, network-bound during dispatch, or imbalance-bound when routing is skewed. Sparse arithmetic does not imply simple serving.

## The architecture frontier is a state-design problem

The most durable comparison is not “Transformer versus non-Transformer.” Ask what state persists, how it grows and what each decode step reads.

| Architecture | Persistent sequence state | Decode access | Important boundary |
|---|---:|---:|---|
| MHA | \(O(LSN_qD)\) | full history | maximum KV width |
| GQA/MQA | \(O(LSN_{kv}D)\) | full but narrower history | shared K/V may constrain TP layout |
| MLA | \(O(LSd_c)\) plus position state | compressed history | extra projections and specialized kernels |
| Sliding window | \(O(LWN_{kv}D)\) if old state is evictable | bounded recent history | global/sink layers may preserve older state |
| Sparse attention | often \(O(LSN_{kv}D)\) stored | \(O(LKN_{kv}D)\) state read for \(K\ll S\) | selection must not cost a full scan |
| Linear attention / SSM | often \(O(1)\) in sequence length | recurrent state | state can still be large in feature dimensions |
| Hybrid | mixed | architecture-specific | every layer type needs its own state contract |

These asymptotic state expressions omit constant K/V and byte-width factors. Their purpose is to expose which architecture dimensions grow.

MLA asks whether full K and V must be cached at all. A simplified form stores:

\[
c_t^{KV}=W^{DKV}h_t,
\qquad
M_{\text{MLA/token/layer}}\approx d_cb+\text{position state},
\]

then reconstructs or algebraically absorbs later projections. Real MLA must handle the position-dependent RoPE component carefully. [DeepSeek-V2](https://arxiv.org/abs/2405.04434) reported a 93.3% KV-cache reduction in its evaluated comparison; that result is architecture- and baseline-specific, not a universal MLA factor.

Sparse attention changes the **number of historical entries read** rather than only their width. It wins only when index selection, metadata, irregular loads and sparse-kernel imbalance cost less than the avoided scan.

Linear/recurrent attention replaces individual historical entries with an update such as:

\[
R_t=f(R_{t-1},k_t,v_t),
\qquad
o_t=g(q_t,R_t).
\]

Its state can be constant with sequence length, but recurrence compresses history and creates different kernels and quality trade-offs.

For a hybrid where only fraction \(f\) of \(L\) layers use conventional KV attention:

\[
M_{state}\approx2(fL)SN_{kv}Db+M_{recurrent}.
\]

The official [Qwen3-Coder-Next model card at revision `a7fbcb5`](https://huggingface.co/Qwen/Qwen3-Coder-Next/blob/a7fbcb5c0e12d62a448eaa0e260346bf5dcc0feb/README.md) gives a concrete case: 48 layers repeat three Gated DeltaNet layers followed by one gated-attention layer, while sparse MoE activates about 3B of 80B total parameters. The system lesson is more general than the model: layer count no longer reveals how many layers grow KV, and total parameters no longer reveal per-token work.

[Kimi Linear](https://arxiv.org/abs/2510.26692) combines recurrent Kimi Delta Attention with periodic MLA. Its reported reductions—up to 75% less KV use and up to 6× decode throughput at 1M context in the authors' evaluated setting—are evidence for architecture/kernel co-design, not a portable speedup promise.

The likely direction is not simply that attention disappears. Exact global attention can become one expensive tier in a hierarchy of recurrent, local, compressed and sparse memories.

## Changed case: equal parameters, unequal serving cost

Consider two 36-layer models with Qwen-like \(N_q=32,D=128\), BF16 state and equal total parameter counts:

- Model G uses GQA with \(N_{kv}=8\).
- Model M uses MHA with \(N_{kv}=32\).
- Model G allocates the parameters saved in K/V projections to a wider MLP so both models retain the same total \(P\).

At 8,192 retained positions:

\[
M_{KV,G}=2(36)(8192)(8)(128)(2)=1.125\text{ GiB/request},
\]

\[
M_{KV,M}=2(36)(8192)(32)(128)(2)=4.5\text{ GiB/request}.
\]

At concurrency 8, the raw difference is 9 GiB versus 36 GiB. Equal parameter counts did not equalize request state because parameter count describes model-lifetime weights, while \(N_{kv}\) controls bytes per retained token.

Communication can also differ. MHA's 32 KV heads partition naturally across more TP ranks; GQA with fewer KV heads may require head replication when the TP degree exceeds \(N_{kv}\). Conversely, replacing dense MLP parameters with routed experts could preserve active arithmetic while introducing all-to-all traffic.

**Assumptions:** every layer retains full history; no prefix sharing, windowing, KV quantization or offload; cache is not replicated except where the parallel layout requires it.

**Falsifying evidence:** measured allocated cache bytes fail to scale with \(N_{kv}\), context and concurrency as predicted after accounting for block rounding and sharding; or profiler/NCCL traces show another allocation or collective dominates the expected difference. A lower latency alone does not prove the mechanism.

## How to analyze an unfamiliar architecture

1. **Pin the artifact.** Record the exact checkpoint revision, runtime/backend version, precision and hardware topology.
2. **Trace one layer.** Write every tensor shape from residual input to residual output. Do not assume \(H=N_qD\).
3. **Inventory persistent state.** For every layer type, state what survives an iteration, its precision and its growth law in \(S\) and concurrency.
4. **Separate resident and active weights.** For MoE, include shared experts, routing and placement.
5. **Derive phase work.** Estimate projection FLOPs, attention/state bytes and temporary allocations for prefill and decode separately.
6. **Draw communication edges.** Identify reductions, all-to-all, stage transfers and remote-state reads.
7. **Predict the limiting resource.** Choose among compute, HBM, interconnect, network, launch latency, synchronization or capacity.
8. **Name a falsifier, then measure.** Use profiler traffic, kernel timings, collective bytes, allocation telemetry and client-visible latency under controlled workloads.

The mastery target is to derive the state-growth law, active parameter path, matrix operations, data movement and communication topology before running the model—then revise that prediction from evidence.

## Questions and planned investigations

### Explain, calculate and defend

- Why can an MLP own more parameters than attention while long-context decode is still dominated by attention state?
- Why does GQA reduce state width but not remove the linear full-history decode scan?
- Why does FlashAttention reduce memory traffic without making exact dense-attention arithmetic \(O(S)\)?
- Under what batch size and context would the Qwen3-4B first-order decode model stop being weight-dominated?
- When can a sparse-attention method read \(K\ll S\) positions yet fail to reduce latency?
- Given a hybrid model, which state must move from a prefill worker to a decode worker?
- How would TP degree greater than KV-head count change placement, memory and communication?

Todo:
Three useful things to test or read
- Add context depth to your current L40S experiment. With Qwen3-4B and a pinned vLLM version, test input lengths such as 256, 2K, 8K and 16K at concurrency 1, 4 and 8. Record TTFT, TPOT, p95 latency, KV utilization and completed requests. This establishes the baseline needed before studying sparse attention.
- Separate microbenchmark throughput from API latency. Run a phase-specific benchmark and an OpenAI-compatible serving benchmark against the same model, precision and input/output lengths. Explain why prompt tokens/s does not directly predict TTFT once queuing, tokenization and scheduling are included.
- Read the Qwen sparse-attention PR as an inference case study. Identify the KV blocks read, block-selection overhead and expected compute/bandwidth crossover. There is no reason yet to rent a multi-GPU system for this exercise.

## Practical labs and connections

[Runpod lab pack](labs/README.md) — one-layer tracing, a phase/context-depth matrix and an architecture boundary change. No execution is implied by the lab specifications.

Continue with [T1.01 — LLM Inference Lifecycle](../T1.01%20LLM%20Inference%20Lifecycle/README.md), [T1.03 — KV Cache](../T1.03%20KV%20Cache%20/README.md), [T1.05 — Prefill vs Decode](../T1.05%20Prefill%20vs%20Decode%20/README.md), [T1.09 — Workload Characterization](../T1.09%20Workload%20Characterization/README.md), [T1.33 — GQA, MQA and MLA](../T1.33%20GQA%20-%20MQA%20-%20MLA%20/README.md), [T1.34 — FlashAttention](../T1.34%20Flash%20Attention%20-%20Optimised%20Attention/README.md), [T1.35 — MoE Inference](../T1.35%20MoE%20Inference/README.md) and [T2.33 — Benchmark Methodology](../T2.33%20Benchmark%20Methodology%20/README.md).

## Primary sources

- [Attention Is All You Need](https://arxiv.org/abs/1706.03762)
- [Root Mean Square Layer Normalization](https://arxiv.org/abs/1910.07467)
- [GLU Variants Improve Transformer](https://arxiv.org/abs/2002.05202)
- [RoFormer](https://arxiv.org/abs/2104.09864)
- [GQA: Training Generalized Multi-Query Transformer Models from Multi-Head Checkpoints](https://arxiv.org/abs/2305.13245)
- [FlashAttention](https://arxiv.org/abs/2205.14135)
- [PagedAttention / vLLM](https://arxiv.org/abs/2309.06180)
- [DeepSeek-V2: MLA and sparse MoE](https://arxiv.org/abs/2405.04434)
- [Qwen3-4B configuration, revision `3101254`](https://huggingface.co/Qwen/Qwen3-4B/blob/3101254bbe4169895668a0e7653c3fd1f313576e/config.json) and [vLLM Qwen3 implementation, commit `f0c14b4`](https://github.com/vllm-project/vllm/blob/f0c14b4f776bb976e654b430442067539fbdb2ea/vllm/model_executor/models/qwen3.py)
- [Qwen3-Coder-Next model card, revision `a7fbcb5`](https://huggingface.co/Qwen/Qwen3-Coder-Next/blob/a7fbcb5c0e12d62a448eaa0e260346bf5dcc0feb/README.md)
- [Kimi Linear](https://arxiv.org/abs/2510.26692)

Implementation behavior is version-, backend- and hardware-dependent. Paper speedups and compression ratios apply to their stated baselines and workloads. Verify a deployment claim against the pinned model, runtime source and measured workload before treating it as general.
