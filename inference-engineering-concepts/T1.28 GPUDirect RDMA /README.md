# T1.28 — GPUDirect RDMA

Status: **Preparation outline**. This page defines coverage to develop; it does not record completed experiments or demonstrated mastery.

[All subjects](../SUBJECTS.md) · [Coverage and priorities](../COVERAGE.md) · [Research context](../RESEARCH_CONTEXT.md)

## Scope

- GPU/NIC direct data paths.
- PCIe and NUMA constraints.
- registration and synchronization.
- bounce-buffer alternatives.

## Changed-case question

What measurement proves host staging was removed rather than merely hidden? Explain the mechanism, state your assumptions, and name a measurement that could disprove your explanation.

## Connections

[T1.01 — LLM Inference Lifecycle](../T1.01%20LLM%20Inference%20Lifecycle/README.md), [T1.09 — Workload Characterization](../T1.09%20Workload%20Characterization/README.md), [T2.33 — Benchmark Methodology](../T2.33%20Benchmark%20Methodology%20/README.md). Use the coverage matrix to locate workload-specific extensions.

## Existing notes

T1.28
GPUDirect RDMA
NIC DMA directly to GPU memory; avoiding CPU bounce buffers; topology constraints
GPU↔GPU RDMA experiment when hardware permits
NVIDIA documentation

## Source starting point

[GPUDirect RDMA documentation](https://docs.nvidia.com/cuda/gpudirect-rdma/). This is an entry point, not evidence that every item above is implemented by every engine. Check model, hardware, backend and version support before making implementation claims.
