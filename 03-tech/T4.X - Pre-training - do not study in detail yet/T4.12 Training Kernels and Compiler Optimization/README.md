# T4.12 — Training Kernels and Compiler Optimization

Status: **Preparation outline — deferred**. This page maps future study; it does not record completed study, experiments or mastery.

[All subjects](../../SUBJECTS.md) · [Coverage and priorities](../../COVERAGE.md) · [Research context](../../RESEARCH_CONTEXT.md)

## Scope

- Forward and backward kernels for matrix multiplication, attention, normalization, activation functions and optimizer updates.
- Fusion, tiling, recomputation and memory-layout choices; identifying launch-, compute- and bandwidth-bound regions.
- Compiler capture, graph breaks, shape specialization, generated kernels and correctness checks against reference implementations.

## Training/inference boundary

Some forward kernels are shared, but training also requires backward kernels, saved activations and gradient accumulation. An inference speedup does not establish training throughput or gradient correctness.

## Related existing subjects

[T1.13 — CUDA Fundamentals](../../T1.X%20-%20Inference%20Engineering%20Core/T1.13%20CUDA%20Fundamentals%20/README.md), [T1.34 — Flash Attention](../../T1.X%20-%20Inference%20Engineering%20Core/T1.34%20Flash%20Attention%20-%20Optimised%20Attention/README.md), [T2.01 — Triton Kernel Programming](../../T2.X%20-%20Inference%20Engineering%20Extended/T2.01%20Triton%20Kernel%20Programming/README.md), and [T2.03 — torch.compile](../../T2.X%20-%20Inference%20Engineering%20Extended/T2.03%20torch.compile%20/README.md).

## Source starting points

- [Stanford CS336 — Language Modeling from Scratch](https://cs336.stanford.edu/).
- [Triton language and compiler documentation](https://triton-lang.org/main/index.html).
- [PyTorch `torch.compile` programming model](https://docs.pytorch.org/docs/stable/torch.compiler_programming_model.html).

These are starting points; preserve numerical and gradient checks while measuring the exact shapes and hardware of interest.
