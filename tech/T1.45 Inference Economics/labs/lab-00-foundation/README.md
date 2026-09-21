# T1.45-L00 — Inference cost ledger

Status: planned, not executed · Scope: core · Level: intro · Hardware: CPU using G1 billing/results · Dependencies: none

Read the [environment and measurement contract](../environment.md) before using paid hardware.

## Engineering question

What is the cost per accepted request, output token and useful task for this exact run?

## Before hints

Write your prediction, two competing mechanisms, and one observation that would falsify each. Freeze correctness/quality and any latency SLO before seeing results.

## Build and run

1. Create `results/T1.45-L00/` and save the exact environment, command, version/digest and workload manifest there.
2. Combine billed GPU time, storage/idle time and measured valid completions; keep advertised and realized utilization separate.
3. Add deterministic correctness assertions and a `--dry-run` or smallest-shape smoke path before the measured run.
4. Warm the exact condition separately, then run at least three interleaved trials. Bound requests, duration, inflight work and distributed timeouts.
5. Preserve raw observations; generate a tidy CSV/JSON summary without deleting failures or unsupported conditions.

## Controlled experiment

- Independent conditions: one baseline run with all failures and warmup labeled.
- Hold constant where applicable: model/tokenizer revision, effective precision, runtime/container, GPU assignment, endpoint semantics, workload seeds and correctness criteria unless the named variable requires a change.
- Measure: GPU-seconds, billed cost, cost/request/token/useful task and uncertainty.
- Record actual rather than requested work (including token counts for serving labs), cache/compile state, trial order, N and variation. Network and profiler conclusions require the relevant capability/permission evidence.

## Evidence and gate

Submit the manifest, exact commands, raw logs, summary table/plot and correctness output. Explain the causal mechanism, account for the critical path, reject at least one competing explanation, and defend the result on one changed case. Offline calculations or simulations remain labeled and cannot satisfy a G1/G2/N2 gate.

## Failure boundary and rebuild

Falsifier: **Failed or quality-invalid tasks disappear from cost denominators.**

An unsupported feature, missing counter, absent topology, failed correctness check or uncontrolled workload is a useful blocked/negative result, not a speed conclusion. Rebuild the smallest failed invariant, then rerun only the bounded condition needed to discriminate mechanisms.

## Sources

- [Live Runpod pricing (checked 2026-09-21)](https://www.runpod.io/pricing)
- [Runpod Pod constraints](https://docs.runpod.io/pods/overview)
- [Subject outline](../../README.md)
- [T1 lab route and evidence rules](../../../T1-LABS.md)
