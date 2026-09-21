# T1.34-L00 — Reference versus optimized attention

Status: planned, not executed · Scope: core · Level: intro · Hardware: G1 · Dependencies: none

Read the [environment and measurement contract](../environment.md) before using paid hardware.

## Engineering question

Does optimized attention match a trusted reference across masks and ragged lengths?

## Before hints

Write your prediction, two competing mechanisms, and one observation that would falsify each. Freeze correctness/quality and any latency SLO before seeing results.

## Build and run

1. Create `results/T1.34-L00/` and save the exact environment, command, version/digest and workload manifest there.
2. Implement a tiny tiled online-softmax attention reference for small deterministic tensors, compare it with the ordinary materialized-score reference, then compare PyTorch SDPA and an available flash backend. Record the selected backend instead of inferring it from the API name.
3. Add deterministic correctness assertions and a `--dry-run` or smallest-shape smoke path before the measured run.
4. Warm the exact condition separately, then run at least three interleaved trials. Bound requests, duration, inflight work and distributed timeouts.
5. Preserve raw observations; generate a tidy CSV/JSON summary without deleting failures or unsupported conditions.

## Controlled experiment

- Independent conditions: materialized versus tiled-online reference; causal/noncausal; lengths around tile boundaries; supported dtypes.
- Hold constant where applicable: model/tokenizer revision, effective precision, runtime/container, GPU assignment, endpoint semantics, workload seeds and correctness criteria unless the named variable requires a change.
- Measure: max/mean error, score/intermediate memory, peak runtime memory and backend selected.
- Record actual rather than requested work (including token counts for serving labs), cache/compile state, trial order, N and variation. Network and profiler conclusions require the relevant capability/permission evidence.

## Evidence and gate

Submit the manifest, exact commands, raw logs, summary table/plot and correctness output. Explain the causal mechanism, account for the critical path, reject at least one competing explanation, and defend the result on one changed case. Offline calculations or simulations remain labeled and cannot satisfy a G1/G2/N2 gate.

## Failure boundary and rebuild

Falsifier: **Backend dispatch is assumed from API name without verification.**

An unsupported feature, missing counter, absent topology, failed correctness check or uncontrolled workload is a useful blocked/negative result, not a speed conclusion. Rebuild the smallest failed invariant, then rerun only the bounded condition needed to discriminate mechanisms.

## Sources

- [Primary release or dated documentation baseline](https://github.com/pytorch/pytorch/releases/tag/v2.14.0)
- [FlashAttention-2 paper](https://arxiv.org/abs/2307.08691)
- [Runpod Pod constraints](https://docs.runpod.io/pods/overview)
- [Subject outline](../../README.md)
- [T1 lab route and evidence rules](../../../T1-LABS.md)
