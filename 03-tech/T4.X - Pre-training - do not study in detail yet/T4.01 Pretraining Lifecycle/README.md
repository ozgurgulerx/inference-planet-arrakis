# T4.01 — Pretraining Lifecycle

Status: **Preparation outline — deferred**. This page maps future study; it does not record completed study, experiments or mastery.

[All subjects](../../SUBJECTS.md) · [Coverage and priorities](../../COVERAGE.md) · [Research context](../../RESEARCH_CONTEXT.md)

## Scope

- The path from a data specification through tokenization, training, evaluation and release of a base-model checkpoint.
- Training-run inputs and artifacts: datasets, code and configuration versions, optimizer state, checkpoints and evaluation reports.
- Stage gates for correctness, stability, quality and resource use, including recovery from interrupted or unstable runs.

## Training/inference boundary

Pretraining produces and validates model weights. Inference loads those weights and exposes different workloads, state and service-level constraints; serving measurements do not establish training stability or data quality.

## Related existing subjects

[T1.01 — LLM Inference Lifecycle](../../T1.X%20-%20Inference%20Engineering%20Core/T1.01%20LLM%20Inference%20Lifecycle/README.md), [T1.09 — Workload Characterization](../../T1.X%20-%20Inference%20Engineering%20Core/T1.09%20Workload%20Characterization/README.md), and [T2.26 — Post-training systems](../../T2.X%20-%20Inference%20Engineering%20Extended/T2.26%20Post-training%20systems%20/README.md).

## Source starting points

- [Stanford CS336 — Language Modeling from Scratch](https://cs336.stanford.edu/).
- [PyTorch distributed checkpoint documentation](https://docs.pytorch.org/docs/stable/distributed.checkpoint.html).

These are starting points, not evidence of completed work or guarantees about a particular training stack.
