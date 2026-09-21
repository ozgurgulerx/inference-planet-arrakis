# T4.08 — Learning Rate Schedules and Regularization

Status: **Preparation outline — deferred**. This page maps future study; it does not record completed study, experiments or mastery.

[All subjects](../../SUBJECTS.md) · [Coverage and priorities](../../COVERAGE.md) · [Research context](../../RESEARCH_CONTEXT.md)

## Scope

- Warmup, constant, cosine and warmup-stable-decay schedules; step-, token- and sample-based schedule definitions.
- Coupling among peak learning rate, optimizer, batch size, training horizon and restart or continual-training policy.
- Weight decay, dropout and other regularization choices, including when large-scale pretraining omits or reduces them.

## Training/inference boundary

Schedules and stochastic regularizers control weight updates during training; dropout is normally disabled for deterministic inference. Their quality effects persist in the checkpoint, but serving-time warmup and cache warming are unrelated concepts.

## Related existing subjects

[T1.08 — Serving metrics](../../T1.X%20-%20Inference%20Engineering%20Core/T1.08%20Serving%20metrics/README.md), [T1.41 — Profiling](../../T1.X%20-%20Inference%20Engineering%20Core/T1.41%20Profiling/README.md), and [T2.33 — Benchmark Methodology](../../T2.X%20-%20Inference%20Engineering%20Extended/T2.33%20Benchmark%20Methodology%20/README.md).

## Source starting points

- [SGDR: Stochastic Gradient Descent with Warm Restarts](https://arxiv.org/abs/1608.03983).
- [Dropout: A Simple Way to Prevent Neural Networks from Overfitting](https://jmlr.org/papers/v15/srivastava14a.html).
- [MiniCPM: Unveiling the Potential of Small Language Models with Scalable Training Strategies](https://arxiv.org/abs/2404.06395).

These are starting points; schedule names are insufficient without exact step boundaries, units and optimizer settings.
