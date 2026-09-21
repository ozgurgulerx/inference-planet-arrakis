# T1.05 — Prefill vs Decode

Status: **Preparation outline**. This page defines coverage to develop; it does not record completed experiments or demonstrated mastery.

[All subjects](../SUBJECTS.md) · [Coverage and priorities](../COVERAGE.md) · [Research context](../RESEARCH_CONTEXT.md)

## Scope

- Parallel prompt processing versus serial generation.
- first-token production.
- phase interference.
- compute and bandwidth regimes.

## Changed-case question

How does a larger decode batch change the claim that decode is memory-bound? Explain the mechanism, state your assumptions, and name a measurement that could disprove your explanation.

## Connections

[T1.01 — LLM Inference Lifecycle](../T1.01%20LLM%20Inference%20Lifecycle/README.md), [T1.09 — Workload Characterization](../T1.09%20Workload%20Characterization/README.md), [T2.33 — Benchmark Methodology](../T2.33%20Benchmark%20Methodology%20/README.md). Use the coverage matrix to locate workload-specific extensions.

## Existing notes

arithmetic intensity, compute-bound vs memory-bound behavior, interference between phases

arithmetic intensity, compute-bound vs memory-bound behavior, interference between phases

## Source starting point

[Lifecycle research and measurement context](../RESEARCH_CONTEXT.md). This is an entry point, not evidence that every item above is implemented by every engine. Check model, hardware, backend and version support before making implementation claims.

## Practical labs

[Runpod lab pack](labs/README.md) — planned experiments and evidence gates; no execution is implied.
