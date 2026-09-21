# T1.29 — KV transfer

Status: **Preparation outline**. This page defines coverage to develop; it does not record completed experiments or demonstrated mastery.

[All subjects](../SUBJECTS.md) · [Coverage and priorities](../COVERAGE.md) · [Research context](../RESEARCH_CONTEXT.md)

## Scope

- KV layout and compatibility.
- ownership handoff.
- transfer/recompute break-even.
- full versus selective state transfer.

## Changed-case question

When is recomputing a prefix cheaper than transferring its cached state? Explain the mechanism, state your assumptions, and name a measurement that could disprove your explanation.

## Connections

[T1.01 — LLM Inference Lifecycle](../T1.01%20LLM%20Inference%20Lifecycle/README.md), [T1.09 — Workload Characterization](../T1.09%20Workload%20Characterization/README.md), [T2.33 — Benchmark Methodology](../T2.33%20Benchmark%20Methodology%20/README.md). Use the coverage matrix to locate workload-specific extensions.

## Existing notes

T1.29
KV transfer
why moving KV becomes a distributed-systems problem; GPU→GPU/CPU/storage paths
vLLM KV connector experiment
vLLM KV transfer docs

## Source starting point

[Dynamo compatibility reference](https://docs.nvidia.com/dynamo/latest/reference/compatibility). This is an entry point, not evidence that every item above is implemented by every engine. Check model, hardware, backend and version support before making implementation claims.

## Practical labs

[Runpod lab pack](labs/README.md) — planned experiments and evidence gates; no execution is implied.
