# T4.04 — Transformer Architecture and Hyperparameters

Status: **Preparation outline — deferred**. This page maps future study; it does not record completed study, experiments or mastery.

[All subjects](../../SUBJECTS.md) · [Coverage and priorities](../../COVERAGE.md) · [Research context](../../RESEARCH_CONTEXT.md)

## Scope

- Decoder-only Transformer components: embeddings, attention, feed-forward blocks, normalization, residual paths and output heads.
- Width, depth, head count, head dimension, feed-forward expansion, context length and positional representation.
- How architectural choices interact with optimization, parameter count, activation memory and training compute.

## Training/inference boundary

Architecture and hyperparameters define what is optimized during pretraining and constrain the resulting checkpoint. The same choices shape inference memory and kernels, but a serving-efficient configuration is not automatically the best training configuration or quality tradeoff.

## Related existing subjects

[T1.02 — Transformer Inference Architecture](../../T1.X%20-%20Inference%20Engineering%20Core/T1.02%20Transformer%20Inference%20Architecture/README.md), [T1.33 — GQA, MQA and MLA](../../T1.X%20-%20Inference%20Engineering%20Core/T1.33%20GQA%20-%20MQA%20-%20MLA%20/README.md), and [T1.34 — Flash Attention](../../T1.X%20-%20Inference%20Engineering%20Core/T1.34%20Flash%20Attention%20-%20Optimised%20Attention/README.md).

## Source starting points

- [Attention Is All You Need](https://arxiv.org/abs/1706.03762).
- [Stanford CS336 — Language Modeling from Scratch](https://cs336.stanford.edu/).
- [LLaMA: Open and Efficient Foundation Language Models](https://arxiv.org/abs/2302.13971).

These are starting points; record the exact architecture and implementation before transferring conclusions between model families.
