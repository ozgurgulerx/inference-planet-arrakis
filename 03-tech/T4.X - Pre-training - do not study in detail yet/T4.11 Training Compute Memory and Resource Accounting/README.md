# T4.11 — Training Compute, Memory and Resource Accounting

Status: **Preparation outline — deferred**. This page maps future study; it does not record completed study, experiments or mastery.

[All subjects](../../SUBJECTS.md) · [Coverage and priorities](../../COVERAGE.md) · [Research context](../../RESEARCH_CONTEXT.md)

## Scope

- FLOP estimates for forward and backward passes, tokens processed and model FLOP utilization.
- Memory ledgers for parameters, gradients, optimizer state, activations, temporary buffers and communication workspaces.
- Arithmetic intensity, bandwidth ceilings, recomputation and cost accounting across hardware and training duration.

## Training/inference boundary

Training includes backward-pass activations, gradients and optimizer state that ordinary inference does not. Inference memory is instead dominated by weights, runtime workspaces and request state such as the KV cache, so ledgers must not be reused unchanged.

## Related existing subjects

[T1.11 — GPU memory hierarchy](../../T1.X%20-%20Inference%20Engineering%20Core/T1.11%20GPU%20memory%20hierarchy/README.md), [T1.12 — Roofline Reasoning](../../T1.X%20-%20Inference%20Engineering%20Core/T1.12%20Roofline%20Reasoning%20/README.md), and [T1.45 — Inference Economics](../../T1.X%20-%20Inference%20Engineering%20Core/T1.45%20Inference%20Economics/README.md).

## Source starting points

- [Stanford CS336 — Language Modeling from Scratch](https://cs336.stanford.edu/).
- [Roofline: An Insightful Visual Performance Model for Multicore Architectures](https://dl.acm.org/doi/10.1145/1498765.1498785).
- [Efficient Large-Scale Language Model Training on GPU Clusters Using Megatron-LM](https://arxiv.org/abs/2104.04473).

These are starting points; state counting conventions, recomputation policy and achieved versus theoretical throughput.
