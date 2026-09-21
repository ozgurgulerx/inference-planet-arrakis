# T1.38 — SGLang Architecture

Status: **Preparation outline**. This page defines coverage to develop; it does not record completed experiments or demonstrated mastery.

[All subjects](../SUBJECTS.md) · [Coverage and priorities](../COVERAGE.md) · [Research context](../RESEARCH_CONTEXT.md)

## Scope

- Radix/prefix reuse.
- scheduler behavior.
- hierarchical cache.
- distributed serving boundaries.

## Changed-case question

Which HiCache tier is actually shared across instances in the chosen deployment? Explain the mechanism, state your assumptions, and name a measurement that could disprove your explanation.

## Connections

[T1.01 — LLM Inference Lifecycle](../T1.01%20LLM%20Inference%20Lifecycle/README.md), [T1.09 — Workload Characterization](../T1.09%20Workload%20Characterization/README.md), [T2.33 — Benchmark Methodology](../T2.33%20Benchmark%20Methodology%20/README.md). Use the coverage matrix to locate workload-specific extensions.

## Source starting point

[SGLang HiCache design](https://docs.sglang.io/docs/advanced_features/hicache_design). This is an entry point, not evidence that every item above is implemented by every engine. Check model, hardware, backend and version support before making implementation claims.

## Practical labs

[Runpod lab pack](labs/README.md) — planned experiments and evidence gates; no execution is implied.
