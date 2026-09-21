# T1.42-L02 — Missing-signal diagnosis

Status: planned, not executed · Scope: later · Level: expert · Hardware: CPU analysis using G1 artifacts · Dependencies: T1.42-L01

Read the [environment and measurement contract](../environment.md) before using paid hardware.

## Engineering question

What conclusion remains defensible when a metric is absent, aggregated or virtualized?

## Before hints

Write your prediction, two competing mechanisms, and one observation that would falsify each. Freeze correctness/quality and any latency SLO before seeing results.

## Build and run

1. Create `results/T1.42-L02/` and save the exact environment, command, version/digest and workload manifest there.
2. Hide or remove one signal in analysis and require an independent corroborating observation.
3. Add deterministic correctness assertions and a `--dry-run` or smallest-shape smoke path before the measured run.
4. Warm the exact condition separately, then run at least three interleaved trials. Bound requests, duration, inflight work and distributed timeouts.
5. Preserve raw observations; generate a tidy CSV/JSON summary without deleting failures or unsupported conditions.

## Controlled experiment

- Independent conditions: full telemetry versus one missing/ambiguous field.
- Hold constant where applicable: model/tokenizer revision, effective precision, runtime/container, GPU assignment, endpoint semantics, workload seeds and correctness criteria unless the named variable requires a change.
- Measure: changed confidence, alternative measurement and unresolved gap.
- Record actual rather than requested work (including token counts for serving labs), cache/compile state, trial order, N and variation. Network and profiler conclusions require the relevant capability/permission evidence.

## Evidence and gate

Submit the manifest, exact commands, raw logs, summary table/plot and correctness output. Explain the causal mechanism, account for the critical path, reject at least one competing explanation, and defend the result on one changed case. Offline calculations or simulations remain labeled and cannot satisfy a G1/G2/N2 gate.

## Failure boundary and rebuild

Falsifier: **Utilization alone is promoted to a bottleneck diagnosis.**

An unsupported feature, missing counter, absent topology, failed correctness check or uncontrolled workload is a useful blocked/negative result, not a speed conclusion. Rebuild the smallest failed invariant, then rerun only the bounded condition needed to discriminate mechanisms.

## Sources

- [Live DCGM documentation (checked 2026-09-21)](https://docs.nvidia.com/datacenter/dcgm/latest/)
- [Runpod Pod constraints](https://docs.runpod.io/pods/overview)
- [Subject outline](../../README.md)
- [T1 lab route and evidence rules](../../../T1-LABS.md)
