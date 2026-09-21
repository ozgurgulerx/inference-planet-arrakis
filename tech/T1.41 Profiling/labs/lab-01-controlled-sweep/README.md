# T1.41-L01 — Systems-to-kernel handoff

Status: planned, not executed · Scope: core · Level: medium · Hardware: G1 profiler permissions conditional · Dependencies: T1.41-L00

Read the [environment and measurement contract](../environment.md) before using paid hardware.

## Engineering question

Which kernel or gap from the system timeline deserves a focused kernel profile?

## Before hints

Write your prediction, two competing mechanisms, and one observation that would falsify each. Freeze correctness/quality and any latency SLO before seeing results.

## Build and run

1. Create `results/T1.41-L01/` and save the exact environment, command, version/digest and workload manifest there.
2. Mark a representative NVTX window, capture Nsight Systems, then profile only the selected kernel with Nsight Compute if allowed.
3. Add deterministic correctness assertions and a `--dry-run` or smallest-shape smoke path before the measured run.
4. Warm the exact condition separately, then run at least three interleaved trials. Bound requests, duration, inflight work and distributed timeouts.
5. Preserve raw observations; generate a tidy CSV/JSON summary without deleting failures or unsupported conditions.

## Controlled experiment

- Independent conditions: one representative warm window.
- Hold constant where applicable: model/tokenizer revision, effective precision, runtime/container, GPU assignment, endpoint semantics, workload seeds and correctness criteria unless the named variable requires a change.
- Measure: CPU launch gaps, kernels/copies, selected kernel counters and rationale.
- Record actual rather than requested work (including token counts for serving labs), cache/compile state, trial order, N and variation. Network and profiler conclusions require the relevant capability/permission evidence.

## Evidence and gate

Submit the manifest, exact commands, raw logs, summary table/plot and correctness output. Explain the causal mechanism, account for the critical path, reject at least one competing explanation, and defend the result on one changed case. Offline calculations or simulations remain labeled and cannot satisfy a G1/G2/N2 gate.

## Failure boundary and rebuild

Falsifier: **Whole-workload averages replace the selected critical-path evidence.**

An unsupported feature, missing counter, absent topology, failed correctness check or uncontrolled workload is a useful blocked/negative result, not a speed conclusion. Rebuild the smallest failed invariant, then rerun only the bounded condition needed to discriminate mechanisms.

## Sources

- [Live Nsight Systems documentation (checked 2026-09-21)](https://docs.nvidia.com/nsight-systems/UserGuide/)
- [Runpod Pod constraints](https://docs.runpod.io/pods/overview)
- [Subject outline](../../README.md)
- [T1 lab route and evidence rules](../../../T1-LABS.md)
