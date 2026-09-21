# T1.34-L02 — Fast-path and sparsity boundary

Status: planned, not executed · Scope: later · Level: expert · Hardware: G1; sparse backend conditional · Dependencies: T1.34-L01

Read the [environment and measurement contract](../environment.md) before using paid hardware.

## Engineering question

Which mask, head dimension, dtype or ragged shape forces a slower path, and when does an exact or approximate sparse method change the validity/performance trade-off?

## Before hints

Write your prediction, two competing mechanisms, and one observation that would falsify each. Freeze correctness/quality and any latency SLO before seeing results.

## Build and run

1. Create `results/T1.34-L02/` and save the exact environment, command, version/digest and workload manifest there.
2. Commit a dispatch prediction, run a boundary matrix and inspect profiler/kernel logs. Add one version-supported sparse-attention condition only after freezing its sparsity pattern and task-validity check; distinguish exact sparse computation from an approximate method. If no supported backend exists, keep that branch explicitly deferred.
3. Add deterministic correctness assertions and a `--dry-run` or smallest-shape smoke path before the measured run.
4. Warm the exact condition separately, then run at least three interleaved trials. Bound requests, duration, inflight work and distributed timeouts.
5. Preserve raw observations; generate a tidy CSV/JSON summary without deleting failures or unsupported conditions.

## Controlled experiment

- Independent conditions: one supported and one suspected fallback per dimension; prefill-like versus decode-like shape; dense exact control versus one supported exact/approximate sparse condition.
- Hold constant where applicable: model/tokenizer revision, effective precision, runtime/container, GPU assignment, endpoint semantics, workload seeds and correctness criteria unless the named variable requires a change.
- Measure: selected backend/kernel, latency, memory, numerical correctness for exact paths and frozen task validity for approximate paths.
- Record actual rather than requested work (including token counts for serving labs), cache/compile state, trial order, N and variation. Network and profiler conclusions require the relevant capability/permission evidence.

## Evidence and gate

Submit the manifest, exact commands, raw logs, summary table/plot and correctness output. Explain the causal mechanism, account for the critical path, reject at least one competing explanation, and defend the result on one changed case. Offline calculations or simulations remain labeled and cannot satisfy a G1/G2/N2 gate.

## Failure boundary and rebuild

Falsifier: **The conclusion rests on latency alone without dispatch evidence, or sparse work is called equivalent without an exactness/task-validity result.**

An unsupported feature, missing counter, absent topology, failed correctness check or uncontrolled workload is a useful blocked/negative result, not a speed conclusion. Rebuild the smallest failed invariant, then rerun only the bounded condition needed to discriminate mechanisms.

## Sources

- [Primary release or dated documentation baseline](https://github.com/pytorch/pytorch/releases/tag/v2.14.0)
- [FlashAttention-2 paper](https://arxiv.org/abs/2307.08691)
- [Runpod Pod constraints](https://docs.runpod.io/pods/overview)
- [Subject outline](../../README.md)
- [T1 lab route and evidence rules](../../../T1-LABS.md)
