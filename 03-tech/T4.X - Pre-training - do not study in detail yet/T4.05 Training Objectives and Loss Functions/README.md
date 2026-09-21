# T4.05 — Training Objectives and Loss Functions

Status: **Preparation outline — deferred**. This page maps future study; it does not record completed study, experiments or mastery.

[All subjects](../../SUBJECTS.md) · [Coverage and priorities](../../COVERAGE.md) · [Research context](../../RESEARCH_CONTEXT.md)

## Scope

- Autoregressive next-token prediction, cross-entropy, masking, label construction and token- versus sequence-level aggregation.
- Multi-token prediction as an auxiliary objective: multiple future-token heads, weighting and interaction with the shared trunk.
- Objective design tradeoffs, loss accounting and the distinction between lower training loss and useful downstream quality.

## Training/inference boundary

The objective supplies the learning signal that changes weights. Inference consumes the learned distribution; an objective may affect inference quality or enable techniques such as speculative prediction, but inference decoding is not itself pretraining.

## Related existing subjects

[T1.02 — Transformer Inference Architecture](../../T1.X%20-%20Inference%20Engineering%20Core/T1.02%20Transformer%20Inference%20Architecture/README.md), [T1.36 — Speculative Decoding](../../T1.X%20-%20Inference%20Engineering%20Core/T1.36%20Speculative%20Decoding%20/README.md), and [T2.33 — Benchmark Methodology](../../T2.X%20-%20Inference%20Engineering%20Extended/T2.33%20Benchmark%20Methodology%20/README.md).

## Source starting points

- [Better & Faster Large Language Models via Multi-token Prediction](https://arxiv.org/abs/2404.19737).
- [DeepSeek-V3 Technical Report](https://arxiv.org/abs/2412.19437).
- [Stanford CS336 — Language Modeling from Scratch](https://cs336.stanford.edu/).

These are starting points; separate reported paper results from behavior reproduced for a chosen model, data mixture and training scale.
