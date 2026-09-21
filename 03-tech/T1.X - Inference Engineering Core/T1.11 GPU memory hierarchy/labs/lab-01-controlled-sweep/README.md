# T1.11-L01 — Working-set staircase

Status: planned, not executed · Scope: core · Level: medium · Hardware: G1 · Dependencies: T1.11-L00

Read the [environment and measurement contract](../environment.md) before using paid hardware.

## Engineering question

Where do register, shared-memory, cache and HBM transitions or spills appear as a working set grows?

## Before hints

Write your prediction, two competing mechanisms, and one observation that would falsify each. Freeze correctness/quality and any latency SLO before seeing results.

## Build and run

1. Create `results/T1.11-L01/` and save the exact environment, command, version/digest and workload manifest there.
2. Build a small CUDA/Triton/PyTorch-extension microbenchmark with variants that keep the hot values in registers, stage a tile in shared memory, reuse a cache-sized global working set and stream an HBM-sized working set. Add one deliberate register-pressure variant and inspect spill/local-memory evidence when profiler permission exists.
3. Add deterministic correctness assertions and a `--dry-run` or smallest-shape smoke path before the measured run.
4. Warm the exact condition separately, then run at least three interleaved trials. Bound requests, duration, inflight work and distributed timeouts.
5. Preserve raw observations; generate a tidy CSV/JSON summary without deleting failures or unsupported conditions.

## Controlled experiment

- Independent conditions: register/shared/global path; sizes below/around/above documented cache capacities; sequential versus strided; normal versus deliberate register pressure.
- Hold constant where applicable: model/tokenizer revision, effective precision, runtime/container, GPU assignment, endpoint semantics, workload seeds and correctness criteria unless the named variable requires a change.
- Measure: bandwidth, latency, register count/spill evidence, shared-memory use, cache metrics if permitted and checksum.
- Record actual rather than requested work (including token counts for serving labs), cache/compile state, trial order, N and variation. Network and profiler conclusions require the relevant capability/permission evidence.

## Evidence and gate

Submit the manifest, exact commands, raw logs, summary table/plot and correctness output. Explain the causal mechanism, account for the critical path, reject at least one competing explanation, and defend the result on one changed case. Offline calculations or simulations remain labeled and cannot satisfy a G1/G2/N2 gate.

## Failure boundary and rebuild

Falsifier: **A knee moves with iteration count rather than working-set size.**

An unsupported feature, missing counter, absent topology, failed correctness check or uncontrolled workload is a useful blocked/negative result, not a speed conclusion. Rebuild the smallest failed invariant, then rerun only the bounded condition needed to discriminate mechanisms.

## Sources

- [Primary release or dated documentation baseline](https://docs.nvidia.com/cuda/archive/13.1.0/cuda-programming-guide/index.html)
- [Runpod Pod constraints](https://docs.runpod.io/pods/overview)
- [Subject outline](../../README.md)
- [T1 lab route and evidence rules](../../../../T1-LABS.md)
