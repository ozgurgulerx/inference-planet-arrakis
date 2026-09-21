# T1.18-L01 — Dispatch/combine trace

Status: planned, not executed · Scope: core · Level: medium · Hardware: G1 supported MoE · Dependencies: T1.18-L00

Read the [environment and measurement contract](../environment.md) before using paid hardware.

## Engineering question

What compute and movement lie on the MoE critical path before and after each expert?

## Before hints

Write your prediction, two competing mechanisms, and one observation that would falsify each. Freeze correctness/quality and any latency SLO before seeing results.

## Build and run

1. Create `results/T1.18-L01/` and save the exact environment, command, version/digest and workload manifest there.
2. Instrument a supported small-MoE forward pass with router, dispatch, expert and combine ranges.
3. Add deterministic correctness assertions and a `--dry-run` or smallest-shape smoke path before the measured run.
4. Warm the exact condition separately, then run at least three interleaved trials. Bound requests, duration, inflight work and distributed timeouts.
5. Preserve raw observations; generate a tidy CSV/JSON summary without deleting failures or unsupported conditions.

## Controlled experiment

- Independent conditions: batch/token sweep with fixed top-k.
- Hold constant where applicable: model/tokenizer revision, effective precision, runtime/container, GPU assignment, endpoint semantics, workload seeds and correctness criteria unless the named variable requires a change.
- Measure: phase times, active/total parameters, memory and routing skew.
- Record actual rather than requested work (including token counts for serving labs), cache/compile state, trial order, N and variation. Network and profiler conclusions require the relevant capability/permission evidence.

## Evidence and gate

Submit the manifest, exact commands, raw logs, summary table/plot and correctness output. Explain the causal mechanism, account for the critical path, reject at least one competing explanation, and defend the result on one changed case. Offline calculations or simulations remain labeled and cannot satisfy a G1/G2/N2 gate.

## Failure boundary and rebuild

Falsifier: **The trace cannot distinguish expert compute from dispatch/combine.**

An unsupported feature, missing counter, absent topology, failed correctness check or uncontrolled workload is a useful blocked/negative result, not a speed conclusion. Rebuild the smallest failed invariant, then rerun only the bounded condition needed to discriminate mechanisms.

## Sources

- [Primary release or dated documentation baseline](https://github.com/vllm-project/vllm/releases/tag/v0.29.0)
- [Runpod Pod constraints](https://docs.runpod.io/pods/overview)
- [Subject outline](../../README.md)
- [T1 lab route and evidence rules](../../../T1-LABS.md)
