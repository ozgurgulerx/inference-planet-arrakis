# T4.06 — Initialization, Normalization and muP

Status: **Preparation outline — deferred**. This page maps future study; it does not record completed study, experiments or mastery.

[All subjects](../../SUBJECTS.md) · [Coverage and priorities](../../COVERAGE.md) · [Research context](../../RESEARCH_CONTEXT.md)

## Scope

- Variance-preserving initialization, residual-branch scaling and the propagation of activations and gradients with depth.
- LayerNorm and RMSNorm placement, pre-norm versus post-norm blocks and numerical considerations.
- Maximal update parameterization (muP) and which hyperparameters can transfer across model widths under its assumptions.

## Training/inference boundary

Initialization and parameterization primarily determine optimization dynamics, while normalization remains active in both training and inference. A stable inference forward pass does not prove that initialization or gradient flow was stable during training.

## Related existing subjects

[T1.02 — Transformer Inference Architecture](../../T1.X%20-%20Inference%20Engineering%20Core/T1.02%20Transformer%20Inference%20Architecture/README.md), [T1.10 — GPU Architecture Basics](../../T1.X%20-%20Inference%20Engineering%20Core/T1.10%20GPU%20Architecture%20Basics/README.md), and [T1.43 — Bottleneck Analysis](../../T1.X%20-%20Inference%20Engineering%20Core/T1.43%20Bottleneck%20Analysis/README.md).

## Source starting points

- [Understanding the difficulty of training deep feedforward neural networks](https://proceedings.mlr.press/v9/glorot10a.html).
- [Layer Normalization](https://arxiv.org/abs/1607.06450).
- [Tensor Programs V: Tuning Large Neural Networks via Zero-Shot Hyperparameter Transfer](https://arxiv.org/abs/2203.03466).

These are starting points; parameterization rules must be checked against the exact implementation before transferring hyperparameters.
