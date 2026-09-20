# T1.03 — KV Cache

Status: **Preparation outline**. This page defines coverage to develop; it does not record completed experiments or demonstrated mastery.

[All subjects](../SUBJECTS.md) · [Coverage and priorities](../COVERAGE.md) · [Research context](../RESEARCH_CONTEXT.md)

## Scope

- KV bytes by layer, KV head, dtype and resident token.
- allocation versus logical length.
- model-specific recurrent state.
- ownership and lifetime.

## Changed-case question

When does the standard KV formula fail for sliding-window, MLA or hybrid models? Explain the mechanism, state your assumptions, and name a measurement that could disprove your explanation.

## Connections

[T1.01 — LLM Inference Lifecycle](../T1.01%20LLM%20Inference%20Lifecycle/README.md), [T1.09 — Workload Characterization](../T1.09%20Workload%20Characterization/README.md), [T2.33 — Benchmark Methodology](../T2.33%20Benchmark%20Methodology%20/README.md). Use the coverage matrix to locate workload-specific extensions.

## Existing notes

size calculation, layers × heads × head dim × tokens × dtype; GQA/MQA implications; lifecycle

size calculation, layers × heads × head dim × tokens × dtype; GQA/MQA implications; lifecycle

## Source starting point

[Lifecycle research and measurement context](../RESEARCH_CONTEXT.md). This is an entry point, not evidence that every item above is implemented by every engine. Check model, hardware, backend and version support before making implementation claims.
