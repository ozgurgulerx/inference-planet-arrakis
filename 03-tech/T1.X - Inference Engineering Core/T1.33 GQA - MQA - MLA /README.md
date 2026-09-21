# T1.33 — GQA - MQA - MLA

Status: **Preparation outline**. This page defines coverage to develop; it does not record completed experiments or demonstrated mastery.

[All subjects](../../SUBJECTS.md) · [Coverage and priorities](../../COVERAGE.md) · [Research context](../../RESEARCH_CONTEXT.md)

## Scope

- MHA, GQA and MQA state scaling.
- MLA latent state.
- model-specific layouts.
- kernel and transfer implications.

## Changed-case question

Why must the KV memory estimator change for MLA rather than just reduce head count? Explain the mechanism, state your assumptions, and name a measurement that could disprove your explanation.

## Connections

[T1.01 — LLM Inference Lifecycle](../T1.01%20LLM%20Inference%20Lifecycle/README.md), [T1.09 — Workload Characterization](../T1.09%20Workload%20Characterization/README.md), [T2.33 — Benchmark Methodology](../../T2.X%20-%20Inference%20Engineering%20Extended/T2.33%20Benchmark%20Methodology%20/README.md). Use the coverage matrix to locate workload-specific extensions.

## Source starting point

[Lifecycle research and measurement context](../../RESEARCH_CONTEXT.md). This is an entry point, not evidence that every item above is implemented by every engine. Check model, hardware, backend and version support before making implementation claims.

## Practical labs

[Runpod lab pack](labs/README.md) — planned experiments and evidence gates; no execution is implied.
