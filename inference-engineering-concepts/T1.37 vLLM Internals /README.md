# T1.37 — vLLM Internals

Status: **Preparation outline**. This page defines coverage to develop; it does not record completed experiments or demonstrated mastery.

[All subjects](../SUBJECTS.md) · [Coverage and priorities](../COVERAGE.md) · [Research context](../RESEARCH_CONTEXT.md)

## Scope

- Scheduler and execution boundary.
- paged state management.
- workers and model runner.
- KV connectors and feature compatibility.

## Changed-case question

Which trace separates scheduler delay from model execution and connector transfer? Explain the mechanism, state your assumptions, and name a measurement that could disprove your explanation.

## Connections

[T1.01 — LLM Inference Lifecycle](../T1.01%20LLM%20Inference%20Lifecycle/README.md), [T1.09 — Workload Characterization](../T1.09%20Workload%20Characterization/README.md), [T2.33 — Benchmark Methodology](../T2.33%20Benchmark%20Methodology%20/README.md). Use the coverage matrix to locate workload-specific extensions.

## Existing notes

The original seed below describes KV transfer (T1.29), not vLLM internals. It is preserved as historical context; the scope above defines this subject.

T1.29
KV transfer
why moving KV becomes a distributed-systems problem; GPU→GPU/CPU/storage paths
vLLM KV connector experiment
vLLM KV transfer docs

## Source starting point

[Lifecycle research and measurement context](../RESEARCH_CONTEXT.md). This is an entry point, not evidence that every item above is implemented by every engine. Check model, hardware, backend and version support before making implementation claims.
