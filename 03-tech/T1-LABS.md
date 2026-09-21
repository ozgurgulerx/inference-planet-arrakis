# T1 Runpod lab route

Status: planned curriculum. The repository now contains **168 lab specifications**: the preserved 36-experiment T1.01 lifecycle pack plus three chapter-owned experiments for every subject from T1.02 through T1.45. Generating these guides did not provision hardware, execute a GPU experiment or demonstrate completion. T1.01 retains its existing starter/check infrastructure where present; the 132 new labs are implementation-ready specifications, not prebuilt executable bundles.

[All subjects](SUBJECTS.md) · [Coverage and priorities](COVERAGE.md) · [Research context](RESEARCH_CONTEXT.md)

## Recommended route

The practical first checkpoint is **after T1.09**. Study T1.01–T1.09, make the CPU-side predictions and harness checks as you go, then reuse one bounded Runpod session for the representative lifecycle experiments. This gives the first coherent request→scheduler→prefill/decode→KV→metrics→workload evidence packet without renting a new Pod per chapter.

| Wave | Chapters | Practical outcome | Default resource |
|---|---|---|---|
| A — lifecycle baseline | T1.01–T1.09 | One request traced end to end; KV, batching, phase timings, metrics and a mixed-workload saturation knee | CPU preparation, then G1 |
| B — GPU mechanisms | T1.10–T1.14 | Architecture, memory paths, roofline, CUDA execution and stream/event correctness | G1 |
| C — parallel execution | T1.15–T1.20 | TP/PP/DP/EP decisions plus NCCL and collective evidence | CPU simulation, then verified G2 |
| D — movement and state | T1.21–T1.31 | PCIe/topology, conditional network evidence, KV transfer, NIXL and P/D disaggregation | G1/G2; N2 only when exposed |
| E — model/runtime choices | T1.32–T1.40 | Precision, attention, MoE, speculation and three serving-runtime paths | G1; selected G2 extensions |
| F — operations and decision | T1.41–T1.45 | Profiling, telemetry, diagnosis, capacity and quality-adjusted economics | Reuse G1/G2 artifacts |

Do Intro and Medium by default. Expert labs are explicit changed-case boundaries and stay deferred until chosen. A later subject may reuse an earlier raw artifact, but it must answer its own engineering question and cannot inherit completion automatically.

## Hardware vocabulary

- **CPU** — arithmetic, simulation, harness checks and analysis. It cannot satisfy a GPU or network evidence gate.
- **G1** — one Runpod NVIDIA GPU. For the shared Qwen baseline, use the smallest supported 24–48 GB option and freeze the exact image, model/tokenizer revisions and runtime.
- **G2** — two compatible GPUs in one Pod, after recording topology and peer-access support. Two visible GPUs do not prove NVLink or an efficient peer path.
- **N2** — two authorized endpoints with the required provider-exposed fabric, NICs, routes, permissions and counters. T1.22–T1.28 deliberately remain blocked when Runpod does not expose the named NVSwitch, NUMA, InfiniBand, RoCE or GPUDirect capability. A simulator is preparation, not hardware proof.

The planning ceiling remains $2/GPU-hour and $30 per subject, including storage and reruns. It is not spending authority. Recheck price and availability immediately before rental, batch adjacent work into one Pod session, export evidence, and stop only resources you own.

## Chapter lab packs

| Chapter | Lab pack | Chapter | Lab pack |
|---|---|---|---|
| T1.01 | [Lifecycle](T1.X%20-%20Inference%20Engineering%20Core/T1.01%20LLM%20Inference%20Lifecycle/labs/README.md) | T1.24 | [RDMA fundamentals](T1.X%20-%20Inference%20Engineering%20Core/T1.24%20RDMA%20Fundamentals%20/labs/README.md) |
| T1.02 | [Transformer architecture](T1.X%20-%20Inference%20Engineering%20Core/T1.02%20Transformer%20Inference%20Architecture/labs/README.md) | T1.25 | [Memory registration](T1.X%20-%20Inference%20Engineering%20Core/T1.25%20Memory%20Registration%20/labs/README.md) |
| T1.03 | [KV cache](T1.X%20-%20Inference%20Engineering%20Core/T1.03%20KV%20Cache%20/labs/README.md) | T1.26 | [InfiniBand](T1.X%20-%20Inference%20Engineering%20Core/T1.26%20InfiniBand/labs/README.md) |
| T1.04 | [Paged attention](T1.X%20-%20Inference%20Engineering%20Core/T1.04%20Paged%20Attention%20-%20KV%20Memory%20Management%20/labs/README.md) | T1.27 | [RoCE](T1.X%20-%20Inference%20Engineering%20Core/T1.27%20RoCE/labs/README.md) |
| T1.05 | [Prefill versus decode](T1.X%20-%20Inference%20Engineering%20Core/T1.05%20Prefill%20vs%20Decode%20/labs/README.md) | T1.28 | [GPUDirect RDMA](T1.X%20-%20Inference%20Engineering%20Core/T1.28%20GPUDirect%20RDMA%20/labs/README.md) |
| T1.06 | [Continuous batching](T1.X%20-%20Inference%20Engineering%20Core/T1.06%20Continous%20Batching%20/labs/README.md) | T1.29 | [KV transfer](T1.X%20-%20Inference%20Engineering%20Core/T1.29%20KV%20transfer/labs/README.md) |
| T1.07 | [Chunked prefill](T1.X%20-%20Inference%20Engineering%20Core/T1.07%20Chunked%20Prefill%20/labs/README.md) | T1.30 | [NIXL](T1.X%20-%20Inference%20Engineering%20Core/T1.30%20NIXL%20/labs/README.md) |
| T1.08 | [Serving metrics](T1.X%20-%20Inference%20Engineering%20Core/T1.08%20Serving%20metrics/labs/README.md) | T1.31 | [P/D disaggregation](T1.X%20-%20Inference%20Engineering%20Core/T1.31%20prefill%20/labs/README.md) |
| T1.09 | [Workload characterization](T1.X%20-%20Inference%20Engineering%20Core/T1.09%20Workload%20Characterization/labs/README.md) | T1.32 | [Quantization](T1.X%20-%20Inference%20Engineering%20Core/T1.32%20Quantization/labs/README.md) |
| T1.10 | [GPU architecture](T1.X%20-%20Inference%20Engineering%20Core/T1.10%20GPU%20Architecture%20Basics/labs/README.md) | T1.33 | [GQA/MQA/MLA](T1.X%20-%20Inference%20Engineering%20Core/T1.33%20GQA%20-%20MQA%20-%20MLA%20/labs/README.md) |
| T1.11 | [GPU memory hierarchy](T1.X%20-%20Inference%20Engineering%20Core/T1.11%20GPU%20memory%20hierarchy/labs/README.md) | T1.34 | [Flash attention](T1.X%20-%20Inference%20Engineering%20Core/T1.34%20Flash%20Attention%20-%20Optimised%20Attention/labs/README.md) |
| T1.12 | [Roofline reasoning](T1.X%20-%20Inference%20Engineering%20Core/T1.12%20Roofline%20Reasoning%20/labs/README.md) | T1.35 | [MoE inference](T1.X%20-%20Inference%20Engineering%20Core/T1.35%20MoE%20Inference/labs/README.md) |
| T1.13 | [CUDA fundamentals](T1.X%20-%20Inference%20Engineering%20Core/T1.13%20CUDA%20Fundamentals%20/labs/README.md) | T1.36 | [Speculative decoding](T1.X%20-%20Inference%20Engineering%20Core/T1.36%20Speculative%20Decoding%20/labs/README.md) |
| T1.14 | [CUDA streams/events](T1.X%20-%20Inference%20Engineering%20Core/T1.14%20CUDA%20streams%20-%20events/labs/README.md) | T1.37 | [vLLM internals](T1.X%20-%20Inference%20Engineering%20Core/T1.37%20vLLM%20Internals%20/labs/README.md) |
| T1.15 | [Tensor parallelism](T1.X%20-%20Inference%20Engineering%20Core/T1.15%20Tensor%20parallelism/labs/README.md) | T1.38 | [SGLang](T1.X%20-%20Inference%20Engineering%20Core/T1.38%20SGLang%20Architecture/labs/README.md) |
| T1.16 | [Pipeline parallelism](T1.X%20-%20Inference%20Engineering%20Core/T1.16%20Pipeline%20paralellism/labs/README.md) | T1.39 | [TensorRT-LLM](T1.X%20-%20Inference%20Engineering%20Core/T1.39%20Tensor-RT%20LLM%20/labs/README.md) |
| T1.17 | [Data parallelism](T1.X%20-%20Inference%20Engineering%20Core/T1.17%20Data%20parallelism/labs/README.md) | T1.40 | [NVIDIA Dynamo](T1.X%20-%20Inference%20Engineering%20Core/T1.40%20NVIDIA%20Dynamo%20/labs/README.md) |
| T1.18 | [Expert parallelism](T1.X%20-%20Inference%20Engineering%20Core/T1.18%20Expert%20parallelism/labs/README.md) | T1.41 | [Profiling](T1.X%20-%20Inference%20Engineering%20Core/T1.41%20Profiling/labs/README.md) |
| T1.19 | [NCCL](T1.X%20-%20Inference%20Engineering%20Core/T1.19%20NCCL%20/labs/README.md) | T1.42 | [GPU observability](T1.X%20-%20Inference%20Engineering%20Core/T1.42%20GPU%20Observability/labs/README.md) |
| T1.20 | [Collectives](T1.X%20-%20Inference%20Engineering%20Core/T1.20%20Collectives/labs/README.md) | T1.43 | [Bottleneck analysis](T1.X%20-%20Inference%20Engineering%20Core/T1.43%20Bottleneck%20Analysis/labs/README.md) |
| T1.21 | [PCIe](T1.X%20-%20Inference%20Engineering%20Core/T1.21%20PCIe/labs/README.md) | T1.44 | [Capacity planning](T1.X%20-%20Inference%20Engineering%20Core/T1.44%20Capacity%20Planning/labs/README.md) |
| T1.22 | [NVLink/NVSwitch](T1.X%20-%20Inference%20Engineering%20Core/T1.22%20NVLink%20NVSwitch/labs/README.md) | T1.45 | [Inference economics](T1.X%20-%20Inference%20Engineering%20Core/T1.45%20Inference%20Economics/labs/README.md) |
| T1.23 | [NUMA](T1.X%20-%20Inference%20Engineering%20Core/T1.23%20NUMA/labs/README.md) |  |  |

## T1.01 semantic ownership

The original T1.01 lab IDs and paths are preserved so the supplied L00–L35 manual, links and future evidence do not break. “Moved” here means the narrower chapter becomes the semantic owner and links back to the one canonical T1.01 experiment; files and results are not copied.

| Owner | Existing T1.01 labs |
|---|---|
| T1.01 lifecycle boundaries | L00, L01, L02, L03, L04, L06, L07, L18, L19, L35 |
| T1.03 KV cache | L12 |
| T1.04 paged attention | L13, L17 |
| T1.05 prefill/decode | L05, L10, L11 |
| T1.06 continuous batching | L14, L27 |
| T1.07 chunked prefill | L16 |
| T1.08 serving metrics | L09, L26 |
| T1.09 workload characterization | L15 |
| T1.12 roofline | L21, L22 |
| T1.15 tensor parallelism | L29 |
| T1.29 KV transfer | L31 |
| T1.31 P/D disaggregation | L30 |
| T1.32 quantization | L23 |
| T1.36 speculative decoding | L28 |
| T1.37 vLLM internals | L08, L20, L24 |
| T1.40 Dynamo/routing | L32 |
| T1.41 profiling | L25 |
| T1.43 bottleneck analysis | L34 |
| T1.44 capacity planning | L33 |

This assigns every one of the 36 existing IDs exactly once. Cross-cutting chapters may reference an experiment without becoming a second owner—for example, T1.17 can compare against L29, and the T2 prefix-cache, backpressure, distributed-KV and benchmark subjects can reuse the relevant T1.01 artifacts.

## Evidence contract

Every chapter pack contains exactly two default experiments and one deferred Expert boundary. Each lab requires a prediction before hints, two competing mechanisms, a controlled variable, correctness or task-validity evidence, raw artifacts, a falsifier, and a changed-case defense. A negative result can pass when it is controlled and explained. Missing source support, unavailable counters, absent fabric capabilities or unexecuted GPU work remain visible gaps.

Use one evidence packet per lab: environment and exact command; workload and actual token counts; warmup/repetition/cache state; raw artifact paths; result and uncertainty; failures and excluded population; intervention/retest; cost; independent explanation/application/defense. Keep raw results, billing records and evaluations private unless explicitly selected for publication.

## Primary operational references

- [Runpod Pods](https://docs.runpod.io/pods/overview)
- [Runpod pricing](https://www.runpod.io/pricing) — live page; recheck immediately before rental
- [vLLM v0.29.0 release](https://github.com/vllm-project/vllm/releases/tag/v0.29.0)
- [NCCL v2.32.3-1 release](https://github.com/NVIDIA/nccl/releases/tag/v2.32.3-1)
- [CUDA 13.1 GPUDirect RDMA guide](https://docs.nvidia.com/cuda/archive/13.1.0/gpudirect-rdma/index.html)
- [NVIDIA Dynamo v1.4.2 release](https://github.com/ai-dynamo/dynamo/releases/tag/v1.4.2)
- [Nsight Systems guide](https://docs.nvidia.com/nsight-systems/UserGuide/) — live documentation checked 2026-09-21
