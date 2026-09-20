# T1.26 — InfiniBand

Status: **Preparation outline**. This page defines coverage to develop; it does not record completed experiments or demonstrated mastery.

[All subjects](../SUBJECTS.md) · [Coverage and priorities](../COVERAGE.md) · [Research context](../RESEARCH_CONTEXT.md)

## Scope

- HCAs, switches and fabric topology.
- link utilization.
- congestion.
- bandwidth and latency microbenchmarks.

## Changed-case question

What would make a healthy point-to-point test coexist with poor all-to-all performance? Explain the mechanism, state your assumptions, and name a measurement that could disprove your explanation.

## Connections

[T1.01 — LLM Inference Lifecycle](../T1.01%20LLM%20Inference%20Lifecycle/README.md), [T1.09 — Workload Characterization](../T1.09%20Workload%20Characterization/README.md), [T2.33 — Benchmark Methodology](../T2.33%20Benchmark%20Methodology%20/README.md). Use the coverage matrix to locate workload-specific extensions.

## Existing notes

HCAs, switches, fabric, link speeds, congestion concepts
ib_write_bw, ib_read_bw
NVIDIA networking docs

## Source starting point

[GPUDirect RDMA documentation](https://docs.nvidia.com/cuda/gpudirect-rdma/). This is an entry point, not evidence that every item above is implemented by every engine. Check model, hardware, backend and version support before making implementation claims.
