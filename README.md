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

- Day 001
- Day 002
- Day 003

## T1 — Core inference engineering

- [T1.01 — LLM Inference Lifecycle](inference-engineering-concepts/T1.01%20LLM%20Inference%20Lifecycle/README.md)
- [T1.02 — Transformer Inference Architecture](inference-engineering-concepts/T1.02%20Transformer%20Inference%20Architecture/README.md)
- [T1.03 — KV Cache](inference-engineering-concepts/T1.03%20KV%20Cache%20/README.md)
- [T1.04 — Paged Attention - KV Memory Management](inference-engineering-concepts/T1.04%20Paged%20Attention%20-%20KV%20Memory%20Management%20/README.md)
- [T1.05 — Prefill vs Decode](inference-engineering-concepts/T1.05%20Prefill%20vs%20Decode%20/README.md)
- [T1.06 — Continuous Batching](inference-engineering-concepts/T1.06%20Continous%20Batching%20/README.md)
- [T1.07 — Chunked Prefill](inference-engineering-concepts/T1.07%20Chunked%20Prefill%20/README.md)
- [T1.08 — Serving metrics](inference-engineering-concepts/T1.08%20Serving%20metrics/README.md)
- [T1.09 — Workload Characterization](inference-engineering-concepts/T1.09%20Workload%20Characterization/README.md)
- [T1.10 — GPU Architecture Basics](inference-engineering-concepts/T1.10%20GPU%20Architecture%20Basics/README.md)
- [T1.11 — GPU memory hierarchy](inference-engineering-concepts/T1.11%20GPU%20memory%20hierarchy/README.md)
- [T1.12 — Roofline Reasoning](inference-engineering-concepts/T1.12%20Roofline%20Reasoning%20/README.md)
- [T1.13 — CUDA Fundamentals](inference-engineering-concepts/T1.13%20CUDA%20Fundamentals%20/README.md)
- [T1.14 — CUDA streams - events](inference-engineering-concepts/T1.14%20CUDA%20streams%20-%20events/README.md)
- [T1.15 — Tensor parallelism](inference-engineering-concepts/T1.15%20Tensor%20parallelism/README.md)
- [T1.16 — Pipeline parallelism](inference-engineering-concepts/T1.16%20Pipeline%20paralellism/README.md)
- [T1.17 — Data parallelism](inference-engineering-concepts/T1.17%20Data%20parallelism/README.md)
- [T1.18 — Expert parallelism](inference-engineering-concepts/T1.18%20Expert%20parallelism/README.md)
- [T1.19 — NCCL](inference-engineering-concepts/T1.19%20NCCL%20/README.md)
- [T1.20 — Collectives](inference-engineering-concepts/T1.20%20Collectives/README.md)
- [T1.21 — PCIe](inference-engineering-concepts/T1.21%20PCIe/README.md)
- [T1.22 — NVLink NVSwitch](inference-engineering-concepts/T1.22%20NVLink%20NVSwitch/README.md)
- [T1.23 — NUMA](inference-engineering-concepts/T1.23%20NUMA/README.md)
- [T1.24 — RDMA Fundamentals](inference-engineering-concepts/T1.24%20RDMA%20Fundamentals%20/README.md)
- [T1.25 — Memory Registration](inference-engineering-concepts/T1.25%20Memory%20Registration%20/README.md)
- [T1.26 — InfiniBand](inference-engineering-concepts/T1.26%20InfiniBand/README.md)
- [T1.27 — RoCE](inference-engineering-concepts/T1.27%20RoCE/README.md)
- [T1.28 — GPUDirect RDMA](inference-engineering-concepts/T1.28%20GPUDirect%20RDMA%20/README.md)
- [T1.29 — KV transfer](inference-engineering-concepts/T1.29%20KV%20transfer/README.md)
- [T1.30 — NIXL](inference-engineering-concepts/T1.30%20NIXL%20/README.md)
- [T1.31 — Prefill/decode disaggregation](inference-engineering-concepts/T1.31%20prefill%20/README.md)
- [T1.32 — Quantization](inference-engineering-concepts/T1.32%20Quantization/README.md)
- [T1.33 — GQA - MQA - MLA](inference-engineering-concepts/T1.33%20GQA%20-%20MQA%20-%20MLA%20/README.md)
- [T1.34 — Flash Attention - Optimised Attention](inference-engineering-concepts/T1.34%20Flash%20Attention%20-%20Optimised%20Attention/README.md)
- [T1.35 — MoE Inference](inference-engineering-concepts/T1.35%20MoE%20Inference/README.md)
- [T1.36 — Speculative Decoding](inference-engineering-concepts/T1.36%20Speculative%20Decoding%20/README.md)
- [T1.37 — vLLM Internals](inference-engineering-concepts/T1.37%20vLLM%20Internals%20/README.md)
- [T1.38 — SGLang Architecture](inference-engineering-concepts/T1.38%20SGLang%20Architecture/README.md)
- [T1.39 — Tensor-RT LLM](inference-engineering-concepts/T1.39%20Tensor-RT%20LLM%20/README.md)
- [T1.40 — NVIDIA Dynamo](inference-engineering-concepts/T1.40%20NVIDIA%20Dynamo%20/README.md)
- [T1.41 — Profiling](inference-engineering-concepts/T1.41%20Profiling/README.md)
- [T1.42 — GPU Observability](inference-engineering-concepts/T1.42%20GPU%20Observability/README.md)
- [T1.43 — Bottleneck Analysis](inference-engineering-concepts/T1.43%20Bottleneck%20Analysis/README.md)
- [T1.44 — Capacity Planning](inference-engineering-concepts/T1.44%20Capacity%20Planning/README.md)
- [T1.45 — Inference Economics](inference-engineering-concepts/T1.45%20Inference%20Economics/README.md)
