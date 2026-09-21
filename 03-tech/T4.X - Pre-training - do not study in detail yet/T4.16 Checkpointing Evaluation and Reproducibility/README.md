# T4.16 — Checkpointing, Evaluation and Reproducibility

Status: **Preparation outline — deferred**. This page maps future study; it does not record completed study, experiments or mastery.

[All subjects](../../SUBJECTS.md) · [Coverage and priorities](../../COVERAGE.md) · [Research context](../../RESEARCH_CONTEXT.md)

## Scope

- Full and sharded checkpoints for model, optimizer, scheduler, random-number and data-loader state; atomicity and recovery.
- Training and validation loss, capability and safety evaluations, contamination checks and checkpoint-selection criteria.
- Reproducible manifests for code, data, configuration, hardware and software, with tolerance for nondeterministic distributed execution.

## Training/inference boundary

Training checkpoints must support restart with optimizer and pipeline state; inference artifacts usually contain only what is needed for a forward pass. Pretraining evaluations select and characterize weights, while serving evaluations add runtime, workload and service-level behavior.

## Related existing subjects

[T1.39 — TensorRT-LLM](../../T1.X%20-%20Inference%20Engineering%20Core/T1.39%20Tensor-RT%20LLM%20/README.md), [T2.19 — Model Loading](../../T2.X%20-%20Inference%20Engineering%20Extended/T2.19%20Model%20Loading/README.md), and [T2.33 — Benchmark Methodology](../../T2.X%20-%20Inference%20Engineering%20Extended/T2.33%20Benchmark%20Methodology%20/README.md).

## Source starting points

- [PyTorch distributed checkpoint documentation](https://docs.pytorch.org/docs/stable/distributed.checkpoint.html).
- [Stanford CS336 — Language Modeling from Scratch](https://cs336.stanford.edu/).
- [Language Model Evaluation Harness](https://github.com/EleutherAI/lm-evaluation-harness).

These are starting points; preserve exact dataset, prompt, harness, checkpoint and dependency versions with every reported result.
