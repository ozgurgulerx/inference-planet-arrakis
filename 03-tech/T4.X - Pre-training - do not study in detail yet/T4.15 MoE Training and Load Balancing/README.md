# T4.15 — MoE Training and Load Balancing

Status: **Preparation outline — deferred**. This page maps future study; it does not record completed study, experiments or mastery.

[All subjects](../../SUBJECTS.md) · [Coverage and priorities](../../COVERAGE.md) · [Research context](../../RESEARCH_CONTEXT.md)

## Scope

- Sparse expert layers, router scores, top-k dispatch, expert capacity, token dropping and expert-parallel communication.
- Expert specialization, routing collapse and load imbalance across tokens, devices and training steps.
- Auxiliary-loss balancing and auxiliary-loss-free bias-based balancing, including their quality and systems tradeoffs.

## Training/inference boundary

Training must learn both expert weights and routing behavior while keeping gradients and token dispatch workable. Inference reuses the learned router and experts but faces request-dependent skew, capacity and latency without optimizer updates.

## Related existing subjects

[T1.18 — Expert parallelism](../../T1.X%20-%20Inference%20Engineering%20Core/T1.18%20Expert%20parallelism/README.md), [T1.35 — MoE Inference](../../T1.X%20-%20Inference%20Engineering%20Core/T1.35%20MoE%20Inference/README.md), and [T2.23 — Advanced MoE](../../T2.X%20-%20Inference%20Engineering%20Extended/T2.23%20Advanced%20MoE/README.md).

## Source starting points

- [Switch Transformers: Scaling to Trillion Parameter Models with Simple and Efficient Sparsity](https://arxiv.org/abs/2101.03961).
- [GShard: Scaling Giant Models with Conditional Computation and Automatic Sharding](https://arxiv.org/abs/2006.16668).
- [DeepSeek-V3 Technical Report](https://arxiv.org/abs/2412.19437).

These are starting points; distinguish training-time balancing mechanisms from observed inference-time expert traffic.
