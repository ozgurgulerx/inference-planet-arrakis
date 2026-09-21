# T1.25 — Memory Registration

Status: **Preparation outline**. This page defines coverage to develop; it does not record completed experiments or demonstrated mastery.

[All subjects](../../SUBJECTS.md) · [Coverage and priorities](../../COVERAGE.md) · [Research context](../../RESEARCH_CONTEXT.md)

## Scope

- Pinned memory and registration.
- keys and access lifetime.
- registration caches.
- deregistration safety.

## Changed-case question

Why can frequent small transfers spend more time registering buffers than moving data? Explain the mechanism, state your assumptions, and name a measurement that could disprove your explanation.

## Connections

[T1.01 — LLM Inference Lifecycle](../T1.01%20LLM%20Inference%20Lifecycle/README.md), [T1.09 — Workload Characterization](../T1.09%20Workload%20Characterization/README.md), [T2.33 — Benchmark Methodology](../../T2.X%20-%20Inference%20Engineering%20Extended/T2.33%20Benchmark%20Methodology%20/README.md). Use the coverage matrix to locate workload-specific extensions.

## Source starting point

[GPUDirect RDMA documentation](https://docs.nvidia.com/cuda/gpudirect-rdma/). This is an entry point, not evidence that every item above is implemented by every engine. Check model, hardware, backend and version support before making implementation claims.

## Practical labs

[Runpod lab pack](labs/README.md) — planned experiments and evidence gates; no execution is implied.
