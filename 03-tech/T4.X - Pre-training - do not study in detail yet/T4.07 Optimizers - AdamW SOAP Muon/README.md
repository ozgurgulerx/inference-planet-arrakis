# T4.07 — Optimizers: AdamW, SOAP and Muon

Status: **Preparation outline — deferred**. This page maps future study; it does not record completed study, experiments or mastery.

[All subjects](../../SUBJECTS.md) · [Coverage and priorities](../../COVERAGE.md) · [Research context](../../RESEARCH_CONTEXT.md)

## Scope

- Adam and AdamW update rules, moments, bias correction, decoupled weight decay and optimizer-state memory.
- SOAP's Shampoo-style preconditioning in an Adam-like basis and the systems cost of matrix statistics and eigendecompositions.
- Muon's orthogonalized matrix updates, parameter grouping, scaling conventions and comparison conditions against AdamW or SOAP.

## Training/inference boundary

Optimizers update parameters and maintain state only during training. Their effects survive in the learned weights, but optimizer state and per-step computation are not part of ordinary inference.

## Related existing subjects

[T1.11 — GPU memory hierarchy](../../T1.X%20-%20Inference%20Engineering%20Core/T1.11%20GPU%20memory%20hierarchy/README.md), [T1.12 — Roofline Reasoning](../../T1.X%20-%20Inference%20Engineering%20Core/T1.12%20Roofline%20Reasoning%20/README.md), and [T2.33 — Benchmark Methodology](../../T2.X%20-%20Inference%20Engineering%20Extended/T2.33%20Benchmark%20Methodology%20/README.md).

## Source starting points

- [Decoupled Weight Decay Regularization](https://arxiv.org/abs/1711.05101).
- [SOAP: Improving and Stabilizing Shampoo using Adam](https://arxiv.org/abs/2409.11321).
- [Muon: An optimizer for hidden layers in neural networks](https://kellerjordan.github.io/posts/muon/).

These are starting points; compare optimizers under matched budgets, parameter groups and clearly stated scaling conventions.
