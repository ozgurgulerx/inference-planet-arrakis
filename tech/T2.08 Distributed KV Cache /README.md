# T2.08 — Distributed KV Cache

Status: **Preparation outline**. This page defines coverage to develop; it does not record completed experiments or demonstrated mastery.

[All subjects](../SUBJECTS.md) · [Coverage and priorities](../COVERAGE.md) · [Research context](../RESEARCH_CONTEXT.md)

## Scope

- Distributed state directories.
- replication and eviction.
- consistency and lifetime.
- GPU/host/shared-storage tier boundaries.

## Changed-case question

What happens when metadata advertises a block that was evicted or belongs to an older model version? Explain the mechanism, state your assumptions, and name a measurement that could disprove your explanation.

## Connections

[T1.01 — LLM Inference Lifecycle](../T1.01%20LLM%20Inference%20Lifecycle/README.md), [T1.09 — Workload Characterization](../T1.09%20Workload%20Characterization/README.md), [T2.33 — Benchmark Methodology](../T2.33%20Benchmark%20Methodology%20/README.md), [T1.43 — Bottleneck Analysis](../T1.43%20Bottleneck%20Analysis/README.md). Use the coverage matrix to locate workload-specific extensions.

## Source starting point

[SGLang HiCache design](https://docs.sglang.io/docs/advanced_features/hicache_design). This is an entry point, not evidence that every item above is implemented by every engine. Check model, hardware, backend and version support before making implementation claims.
