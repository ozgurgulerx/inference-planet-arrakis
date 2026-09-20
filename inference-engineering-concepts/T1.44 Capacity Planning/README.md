# T1.44 — Capacity Planning

Status: **Preparation outline**. This page defines coverage to develop; it does not record completed experiments or demonstrated mastery.

[All subjects](../SUBJECTS.md) · [Coverage and priorities](../COVERAGE.md) · [Research context](../RESEARCH_CONTEXT.md)

## Scope

- SLO-constrained capacity.
- workload mix and bursts.
- memory headroom.
- scaling and failure reserve.

## Changed-case question

Why is peak tokens/s an unsafe estimate of supported arrival rate? Explain the mechanism, state your assumptions, and name a measurement that could disprove your explanation.

## Connections

[T1.01 — LLM Inference Lifecycle](../T1.01%20LLM%20Inference%20Lifecycle/README.md), [T1.09 — Workload Characterization](../T1.09%20Workload%20Characterization/README.md), [T2.33 — Benchmark Methodology](../T2.33%20Benchmark%20Methodology%20/README.md). Use the coverage matrix to locate workload-specific extensions.

## Source starting point

[Lifecycle research and measurement context](../RESEARCH_CONTEXT.md). This is an entry point, not evidence that every item above is implemented by every engine. Check model, hardware, backend and version support before making implementation claims.
