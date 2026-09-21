# T2.07 — Prefix Caching

Status: **Preparation outline**. This page defines coverage to develop; it does not record completed experiments or demonstrated mastery.

[All subjects](../../SUBJECTS.md) · [Coverage and priorities](../../COVERAGE.md) · [Research context](../../RESEARCH_CONTEXT.md)

## Scope

- Prefix identity and hashing.
- cache granularity.
- tenant isolation.
- hit rate versus saved work.

## Changed-case question

Why does a high prefix hit rate not necessarily imply a large latency benefit? Explain the mechanism, state your assumptions, and name a measurement that could disprove your explanation.

## Connections

[T1.01 — LLM Inference Lifecycle](../../T1.X%20-%20Inference%20Engineering%20Core/T1.01%20LLM%20Inference%20Lifecycle/README.md), [T1.09 — Workload Characterization](../../T1.X%20-%20Inference%20Engineering%20Core/T1.09%20Workload%20Characterization/README.md), [T2.33 — Benchmark Methodology](../T2.33%20Benchmark%20Methodology%20/README.md), [T1.43 — Bottleneck Analysis](../../T1.X%20-%20Inference%20Engineering%20Core/T1.43%20Bottleneck%20Analysis/README.md). Use the coverage matrix to locate workload-specific extensions.

## Source starting point

[SGLang HiCache design](https://docs.sglang.io/docs/advanced_features/hicache_design). This is an entry point, not evidence that every item above is implemented by every engine. Check model, hardware, backend and version support before making implementation claims.
