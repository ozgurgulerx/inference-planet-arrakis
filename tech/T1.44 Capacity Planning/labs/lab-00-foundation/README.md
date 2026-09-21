# T1.44-L00 — Capacity ledger

Status: planned, not executed · Scope: core · Level: intro · Hardware: CPU then G1 validation · Dependencies: none

Read the [environment and measurement contract](../environment.md) before using paid hardware.

## Engineering question

How many concurrent requests fit after weights, runtime reserve, KV/state and failure headroom?

## Before hints

Write your prediction, two competing mechanisms, and one observation that would falsify each. Freeze correctness/quality and any latency SLO before seeing results.

## Build and run

1. Create `results/T1.44-L00/` and save the exact environment, command, version/digest and workload manifest there.
2. Build a byte-and-service-demand worksheet from the observed model/runtime rather than advertised VRAM.
3. Add deterministic correctness assertions and a `--dry-run` or smallest-shape smoke path before the measured run.
4. Warm the exact condition separately, then run at least three interleaved trials. Bound requests, duration, inflight work and distributed timeouts.
5. Preserve raw observations; generate a tidy CSV/JSON summary without deleting failures or unsupported conditions.

## Controlled experiment

- Independent conditions: three request shapes and explicit reserve.
- Hold constant where applicable: model/tokenizer revision, effective precision, runtime/container, GPU assignment, endpoint semantics, workload seeds and correctness criteria unless the named variable requires a change.
- Measure: memory envelope, tokens/sequence, concurrency bound and assumptions.
- Record actual rather than requested work (including token counts for serving labs), cache/compile state, trial order, N and variation. Network and profiler conclusions require the relevant capability/permission evidence.

## Evidence and gate

Submit the manifest, exact commands, raw logs, summary table/plot and correctness output. Explain the causal mechanism, account for the critical path, reject at least one competing explanation, and defend the result on one changed case. Offline calculations or simulations remain labeled and cannot satisfy a G1/G2/N2 gate.

## Failure boundary and rebuild

Falsifier: **The bound ignores block rounding, output headroom or non-KV memory.**

An unsupported feature, missing counter, absent topology, failed correctness check or uncontrolled workload is a useful blocked/negative result, not a speed conclusion. Rebuild the smallest failed invariant, then rerun only the bounded condition needed to discriminate mechanisms.

## Sources

- [Primary release or dated documentation baseline](https://github.com/vllm-project/vllm/releases/tag/v0.29.0)
- [Runpod Pod constraints](https://docs.runpod.io/pods/overview)
- [Subject outline](../../README.md)
- [T1 lab route and evidence rules](../../../T1-LABS.md)
