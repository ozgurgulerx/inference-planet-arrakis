# T4.13 — Distributed Pretraining

Status: **Preparation outline — deferred**. This page maps future study; it does not record completed study, experiments or mastery.

[All subjects](../../SUBJECTS.md) · [Coverage and priorities](../../COVERAGE.md) · [Research context](../../RESEARCH_CONTEXT.md)

## Scope

- Data, tensor, pipeline, sequence/context and expert parallelism for forward and backward computation.
- Sharded parameters, gradients and optimizer states; activation checkpointing and memory/communication tradeoffs.
- Collective ordering, topology mapping, overlap, stragglers, failure recovery and distributed correctness.

## Training/inference boundary

Training parallelism must propagate activations and gradients and keep optimizer state consistent. Inference can use related partitions and collectives, but it has different latency, request-state and fault-recovery requirements.

## Related existing subjects

[T1.15 — Tensor parallelism](../../T1.X%20-%20Inference%20Engineering%20Core/T1.15%20Tensor%20parallelism/README.md), [T1.16 — Pipeline parallelism](../../T1.X%20-%20Inference%20Engineering%20Core/T1.16%20Pipeline%20paralellism/README.md), [T1.17 — Data parallelism](../../T1.X%20-%20Inference%20Engineering%20Core/T1.17%20Data%20parallelism/README.md), and [T1.19 — NCCL](../../T1.X%20-%20Inference%20Engineering%20Core/T1.19%20NCCL%20/README.md).

## Source starting points

- [Megatron-LM: Training Multi-Billion Parameter Language Models Using Model Parallelism](https://arxiv.org/abs/1909.08053).
- [ZeRO: Memory Optimizations Toward Training Trillion Parameter Models](https://arxiv.org/abs/1910.02054).
- [PyTorch Fully Sharded Data Parallel documentation](https://docs.pytorch.org/docs/stable/fsdp.html).

These are starting points; topology, framework version and exact sharding configuration must accompany any performance result.
