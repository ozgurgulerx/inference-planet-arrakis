# T2.23 — Advanced MoE

Status: **Preparation outline**. This page defines coverage to develop; it does not record completed experiments or demonstrated mastery.

[All subjects](../SUBJECTS.md) · [Coverage and priorities](../COVERAGE.md) · [Research context](../RESEARCH_CONTEXT.md)

## Scope

- Wide expert parallelism.
- expert skew and rebalancing.
- low-bit expert kernels.
- emerging attention–FFN disaggregation.

## Changed-case question

What evidence justifies splitting attention and experts into separate resource pools? Explain the mechanism, state your assumptions, and name a measurement that could disprove your explanation.

## Connections

[T1.01 — LLM Inference Lifecycle](../T1.01%20LLM%20Inference%20Lifecycle/README.md), [T1.09 — Workload Characterization](../T1.09%20Workload%20Characterization/README.md), [T2.33 — Benchmark Methodology](../T2.33%20Benchmark%20Methodology%20/README.md), [T1.43 — Bottleneck Analysis](../T1.43%20Bottleneck%20Analysis/README.md). Use the coverage matrix to locate workload-specific extensions.

## Source starting point

[vLLM expert parallel deployment](https://docs.vllm.ai/en/latest/serving/expert_parallel_deployment/). This is an entry point, not evidence that every item above is implemented by every engine. Check model, hardware, backend and version support before making implementation claims.
