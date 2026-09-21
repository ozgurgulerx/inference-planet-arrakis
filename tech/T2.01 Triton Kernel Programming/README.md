# T2.01 — Triton Kernel Programming

Status: **Preparation outline**. This page defines coverage to develop; it does not record completed experiments or demonstrated mastery.

[All subjects](../SUBJECTS.md) · [Coverage and priorities](../COVERAGE.md) · [Research context](../RESEARCH_CONTEXT.md)

## Scope

- Launch grids and program instances.
- vectorized memory operations.
- reductions.
- correctness and performance comparison.

## Changed-case question

When does increasing tile size reduce rather than improve throughput? Explain the mechanism, state your assumptions, and name a measurement that could disprove your explanation.

## Connections

[T1.01 — LLM Inference Lifecycle](../T1.01%20LLM%20Inference%20Lifecycle/README.md), [T1.09 — Workload Characterization](../T1.09%20Workload%20Characterization/README.md), [T2.33 — Benchmark Methodology](../T2.33%20Benchmark%20Methodology%20/README.md), [T1.43 — Bottleneck Analysis](../T1.43%20Bottleneck%20Analysis/README.md). Use the coverage matrix to locate workload-specific extensions.

## Existing notes

understand launch grids, blocks, memory access fusion

## Source starting point

[Triton tutorials](https://triton-lang.org/main/getting-started/tutorials/index.html). This is an entry point, not evidence that every item above is implemented by every engine. Check model, hardware, backend and version support before making implementation claims.
