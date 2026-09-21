# T4.10 — Mixed Precision and Numerical Stability

Status: **Preparation outline — deferred**. This page maps future study; it does not record completed study, experiments or mastery.

[All subjects](../../SUBJECTS.md) · [Coverage and priorities](../../COVERAGE.md) · [Research context](../../RESEARCH_CONTEXT.md)

## Scope

- FP32, FP16, BF16 and lower-precision formats: range, precision, accumulation and conversion behavior.
- Autocast, master weights, loss scaling, reduced-precision collectives and precision choices for sensitive operations.
- Detecting and localizing overflow, underflow, NaNs, loss spikes and silent numerical divergence.

## Training/inference boundary

Training needs stable forward, backward and optimizer computations; inference normally runs only the forward path. A precision that is acceptable for serving quality and latency may still be unstable for gradients or optimizer state.

## Related existing subjects

[T1.10 — GPU Architecture Basics](../../T1.X%20-%20Inference%20Engineering%20Core/T1.10%20GPU%20Architecture%20Basics/README.md), [T1.32 — Quantization](../../T1.X%20-%20Inference%20Engineering%20Core/T1.32%20Quantization/README.md), and [T1.41 — Profiling](../../T1.X%20-%20Inference%20Engineering%20Core/T1.41%20Profiling/README.md).

## Source starting points

- [Mixed Precision Training](https://arxiv.org/abs/1710.03740).
- [PyTorch Automatic Mixed Precision examples](https://docs.pytorch.org/docs/stable/notes/amp_examples.html).
- [NVIDIA Transformer Engine documentation](https://docs.nvidia.com/deeplearning/transformer-engine/).

These are starting points; validate numerical behavior for the selected hardware, software versions and model rather than inferring it from format names.
