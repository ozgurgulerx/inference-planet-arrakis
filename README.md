![The tokens must flow — Dune-inspired inference pixel art](assets/dune-tokens-must-flow.png)

My name is Özgür Güler.

This repo is my journey into inference engineering: building, scaling, and optimizing LLM inference stacks in the real world.

## Learning goal

Build inference-engineering knowledge that transfers across models, runtimes, workloads, and hardware:

- **Mechanics:** Understand how model architecture, request state, serving runtimes, and accelerators interact.
- **Systems mindset:** Identify bottlenecks, reason about scaling, and optimize for quality-constrained latency, throughput, reliability, and cost.
- **Engineering intuition:** Predict how architecture, precision, caching, batching, scheduling, and parallelism choices will behave under a changed model, workload, or hardware configuration.

A subject is complete only when I can explain the mechanism, predict its behavior, measure it, diagnose the evidence, and defend the conclusion on a changed case.

## Daily journal

The journal is intentionally idle until the core theory is complete and the hands-on model work begins.

[Journal memos](days/)

- Day 001
- Day 002
- Day 003

## T1 — Core inference engineering

[All technical subjects, including T2 and T3](tech/SUBJECTS.md)

- [T1.01 — LLM Inference Lifecycle](tech/T1.01%20LLM%20Inference%20Lifecycle/README.md)
- [T1.02 — Transformer Inference Architecture](tech/T1.02%20Transformer%20Inference%20Architecture/README.md)
- [T1.03 — KV Cache](tech/T1.03%20KV%20Cache%20/README.md)
- [T1.04 — Paged Attention - KV Memory Management](tech/T1.04%20Paged%20Attention%20-%20KV%20Memory%20Management%20/README.md)
- [T1.05 — Prefill vs Decode](tech/T1.05%20Prefill%20vs%20Decode%20/README.md)
- [T1.06 — Continuous Batching](tech/T1.06%20Continous%20Batching%20/README.md)
- [T1.07 — Chunked Prefill](tech/T1.07%20Chunked%20Prefill%20/README.md)
- [T1.08 — Serving metrics](tech/T1.08%20Serving%20metrics/README.md)
- [T1.09 — Workload Characterization](tech/T1.09%20Workload%20Characterization/README.md)
- [T1.10 — GPU Architecture Basics](tech/T1.10%20GPU%20Architecture%20Basics/README.md)
- [T1.11 — GPU memory hierarchy](tech/T1.11%20GPU%20memory%20hierarchy/README.md)
- [T1.12 — Roofline Reasoning](tech/T1.12%20Roofline%20Reasoning%20/README.md)
- [T1.13 — CUDA Fundamentals](tech/T1.13%20CUDA%20Fundamentals%20/README.md)
- [T1.14 — CUDA streams - events](tech/T1.14%20CUDA%20streams%20-%20events/README.md)
- [T1.15 — Tensor parallelism](tech/T1.15%20Tensor%20parallelism/README.md)
- [T1.16 — Pipeline parallelism](tech/T1.16%20Pipeline%20paralellism/README.md)
- [T1.17 — Data parallelism](tech/T1.17%20Data%20parallelism/README.md)
- [T1.18 — Expert parallelism](tech/T1.18%20Expert%20parallelism/README.md)
- [T1.19 — NCCL](tech/T1.19%20NCCL%20/README.md)
- [T1.20 — Collectives](tech/T1.20%20Collectives/README.md)
- [T1.21 — PCIe](tech/T1.21%20PCIe/README.md)
- [T1.22 — NVLink NVSwitch](tech/T1.22%20NVLink%20NVSwitch/README.md)
- [T1.23 — NUMA](tech/T1.23%20NUMA/README.md)
- [T1.24 — RDMA Fundamentals](tech/T1.24%20RDMA%20Fundamentals%20/README.md)
- [T1.25 — Memory Registration](tech/T1.25%20Memory%20Registration%20/README.md)
- [T1.26 — InfiniBand](tech/T1.26%20InfiniBand/README.md)
- [T1.27 — RoCE](tech/T1.27%20RoCE/README.md)
- [T1.28 — GPUDirect RDMA](tech/T1.28%20GPUDirect%20RDMA%20/README.md)
- [T1.29 — KV transfer](tech/T1.29%20KV%20transfer/README.md)
- [T1.30 — NIXL](tech/T1.30%20NIXL%20/README.md)
- [T1.31 — Prefill/decode disaggregation](tech/T1.31%20prefill%20/README.md)
- [T1.32 — Quantization](tech/T1.32%20Quantization/README.md)
- [T1.33 — GQA - MQA - MLA](tech/T1.33%20GQA%20-%20MQA%20-%20MLA%20/README.md)
- [T1.34 — Flash Attention - Optimised Attention](tech/T1.34%20Flash%20Attention%20-%20Optimised%20Attention/README.md)
- [T1.35 — MoE Inference](tech/T1.35%20MoE%20Inference/README.md)
- [T1.36 — Speculative Decoding](tech/T1.36%20Speculative%20Decoding%20/README.md)
- [T1.37 — vLLM Internals](tech/T1.37%20vLLM%20Internals%20/README.md)
- [T1.38 — SGLang Architecture](tech/T1.38%20SGLang%20Architecture/README.md)
- [T1.39 — Tensor-RT LLM](tech/T1.39%20Tensor-RT%20LLM%20/README.md)
- [T1.40 — NVIDIA Dynamo](tech/T1.40%20NVIDIA%20Dynamo%20/README.md)
- [T1.41 — Profiling](tech/T1.41%20Profiling/README.md)
- [T1.42 — GPU Observability](tech/T1.42%20GPU%20Observability/README.md)
- [T1.43 — Bottleneck Analysis](tech/T1.43%20Bottleneck%20Analysis/README.md)
- [T1.44 — Capacity Planning](tech/T1.44%20Capacity%20Planning/README.md)
- [T1.45 — Inference Economics](tech/T1.45%20Inference%20Economics/README.md)
