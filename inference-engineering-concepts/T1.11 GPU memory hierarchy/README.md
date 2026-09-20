# T1.11 — GPU memory hierarchy

Status: **Preparation outline**. This page defines coverage to develop; it does not record completed experiments or demonstrated mastery.

[All subjects](../SUBJECTS.md) · [Coverage and priorities](../COVERAGE.md) · [Research context](../RESEARCH_CONTEXT.md)

## Scope

- HBM, caches, shared memory and registers.
- bandwidth and latency.
- spills and working sets.
- host/device movement.

## Changed-case question

When does reducing bytes fail to reduce end-to-end latency? Explain the mechanism, state your assumptions, and name a measurement that could disprove your explanation.

## Connections

[T1.01 — LLM Inference Lifecycle](../T1.01%20LLM%20Inference%20Lifecycle/README.md), [T1.09 — Workload Characterization](../T1.09%20Workload%20Characterization/README.md), [T2.33 — Benchmark Methodology](../T2.33%20Benchmark%20Methodology%20/README.md). Use the coverage matrix to locate workload-specific extensions.

## Source starting point

[CUDA programming guide](https://docs.nvidia.com/cuda/cuda-programming-guide/contents.html). This is an entry point, not evidence that every item above is implemented by every engine. Check model, hardware, backend and version support before making implementation claims.
