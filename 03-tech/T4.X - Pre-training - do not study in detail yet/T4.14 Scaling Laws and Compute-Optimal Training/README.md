# T4.14 — Scaling Laws and Compute-Optimal Training

Status: **Preparation outline — deferred**. This page maps future study; it does not record completed study, experiments or mastery.

[All subjects](../../SUBJECTS.md) · [Coverage and priorities](../../COVERAGE.md) · [Research context](../../RESEARCH_CONTEXT.md)

## Scope

- Empirical relationships among loss, parameter count, training tokens and compute budget.
- Compute-optimal allocation, under- versus over-training, data limits and the assumptions behind fitted power laws.
- Pilot runs, curve fitting, uncertainty, extrapolation risk and the difference between validation loss and downstream utility.

## Training/inference boundary

Scaling laws guide pretraining resource allocation and checkpoint selection. They do not directly choose an inference deployment, whose cost and quality also depend on architecture, quantization, workload and service constraints.

## Related existing subjects

[T1.44 — Capacity Planning](../../T1.X%20-%20Inference%20Engineering%20Core/T1.44%20Capacity%20Planning/README.md), [T1.45 — Inference Economics](../../T1.X%20-%20Inference%20Engineering%20Core/T1.45%20Inference%20Economics/README.md), and [T2.33 — Benchmark Methodology](../../T2.X%20-%20Inference%20Engineering%20Extended/T2.33%20Benchmark%20Methodology%20/README.md).

## Source starting points

- [Scaling Laws for Neural Language Models](https://arxiv.org/abs/2001.08361).
- [Training Compute-Optimal Large Language Models](https://arxiv.org/abs/2203.15556).
- [Stanford CS336 — Language Modeling from Scratch](https://cs336.stanford.edu/).

These are starting points; preserve the fitted regime, uncertainty and data assumptions when applying a scaling result.
