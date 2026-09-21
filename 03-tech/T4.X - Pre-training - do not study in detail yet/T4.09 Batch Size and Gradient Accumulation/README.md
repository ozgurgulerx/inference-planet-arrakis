# T4.09 — Batch Size and Gradient Accumulation

Status: **Preparation outline — deferred**. This page maps future study; it does not record completed study, experiments or mastery.

[All subjects](../../SUBJECTS.md) · [Coverage and priorities](../../COVERAGE.md) · [Research context](../../RESEARCH_CONTEXT.md)

## Scope

- Microbatch, data-parallel batch and global batch definitions in sequences and tokens; gradient accumulation and loss normalization.
- Memory, utilization, communication and optimizer-step tradeoffs as microbatch count or sequence length changes.
- Gradient-noise scale and critical batch size: where added parallel batch yields diminishing time-to-quality returns.

## Training/inference boundary

Training batches combine examples to estimate a gradient, whereas inference batching combines independent requests for hardware efficiency under latency constraints. Their optimal sizes and correctness conditions are different.

## Related existing subjects

[T1.06 — Continuous Batching](../../T1.X%20-%20Inference%20Engineering%20Core/T1.06%20Continous%20Batching%20/README.md), [T1.17 — Data parallelism](../../T1.X%20-%20Inference%20Engineering%20Core/T1.17%20Data%20parallelism/README.md), and [T1.43 — Bottleneck Analysis](../../T1.X%20-%20Inference%20Engineering%20Core/T1.43%20Bottleneck%20Analysis/README.md).

## Source starting points

- [An Empirical Model of Large-Batch Training](https://arxiv.org/abs/1812.06162).
- [Accurate, Large Minibatch SGD: Training ImageNet in 1 Hour](https://arxiv.org/abs/1706.02677).
- [Stanford CS336 — Language Modeling from Scratch](https://cs336.stanford.edu/).

These are starting points; report batch units, accumulation, sequence-length distribution and optimizer-step count explicitly.
