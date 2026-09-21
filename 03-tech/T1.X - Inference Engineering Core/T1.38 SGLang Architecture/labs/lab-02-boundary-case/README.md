# T1.38-L02 — Hierarchical-cache and distributed boundary

Status: planned, not executed · Scope: later · Level: expert · Hardware: G1+host RAM; G2/N2 extension conditional · Dependencies: T1.38-L01

Read the [environment and measurement contract](../environment.md) before using paid hardware.

## Engineering question

When does a lower cache tier or second worker save more recompute than it adds in lookup, transfer, routing and queueing?

## Before hints

Write your prediction, two competing mechanisms, and one observation that would falsify each. Freeze correctness/quality and any latency SLO before seeing results.

## Build and run

1. Create `results/T1.38-L02/` and save the exact environment, command, version/digest and workload manifest there.
2. Choose one version-supported SGLang hierarchical-cache path and record its exact tier/connector semantics. Replay a fixed prefix trace through cold, local Radix-hit and lower-tier-hit conditions. Add a second-worker/distributed condition only when topology and connector support are observed; otherwise preserve it as blocked.
3. Add deterministic correctness assertions and a `--dry-run` or smallest-shape smoke path before the measured run.
4. Warm the exact condition separately, then run at least three interleaved trials. Bound requests, duration, inflight work and distributed timeouts.
5. Preserve raw observations; generate a tidy CSV/JSON summary without deleting failures or unsupported conditions.

## Controlled experiment

- Independent conditions: cold, local Radix hit and one supported lower-tier hit; single worker versus a supported second-worker route as a deferred extension.
- Hold constant where applicable: model/tokenizer revision, effective precision, runtime/container, GPU assignment, endpoint semantics, workload seeds and correctness criteria unless the named variable requires a change.
- Measure: hit tier and reused tokens, lookup/transfer bytes and time, route/queue time, TTFT, SLO goodput and failures.
- Record actual rather than requested work (including token counts for serving labs), cache/compile state, trial order, N and variation. Network and profiler conclusions require the relevant capability/permission evidence.

## Evidence and gate

Submit the manifest, exact commands, raw logs, summary table/plot and correctness output. Explain the causal mechanism, account for the critical path, reject at least one competing explanation, and defend the result on one changed case. Offline calculations or simulations remain labeled and cannot satisfy a G1/G2/N2 gate.

## Failure boundary and rebuild

Falsifier: **A reported lower-tier hit lacks avoided-work evidence, or distributed results change endpoint/workload semantics.**

An unsupported feature, missing counter, absent topology, failed correctness check or uncontrolled workload is a useful blocked/negative result, not a speed conclusion. Rebuild the smallest failed invariant, then rerun only the bounded condition needed to discriminate mechanisms.

## Sources

- [Primary release or dated documentation baseline](https://github.com/sgl-project/sglang/releases/tag/v0.5.20)
- [Runpod Pod constraints](https://docs.runpod.io/pods/overview)
- [Subject outline](../../README.md)
- [T1 lab route and evidence rules](../../../../T1-LABS.md)
