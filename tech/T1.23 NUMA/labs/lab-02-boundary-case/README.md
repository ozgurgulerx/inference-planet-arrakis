# T1.23-L02 — GPU/NIC affinity changed case

Status: planned, not executed · Scope: later · Level: expert · Hardware: N2/host access conditional · Dependencies: T1.23-L01

Read the [environment and measurement contract](../environment.md) before using paid hardware.

## Engineering question

Would a remote NIC path explain transfer tail latency?

## Before hints

Write your prediction, two competing mechanisms, and one observation that would falsify each. Freeze correctness/quality and any latency SLO before seeing results.

## Build and run

1. Create `results/T1.23-L02/` and save the exact environment, command, version/digest and workload manifest there.
2. Map NIC locality and run network+GPU transfer only if device and counters are exposed; otherwise retain an analytical prediction.
3. Add deterministic correctness assertions and a `--dry-run` or smallest-shape smoke path before the measured run.
4. Warm the exact condition separately, then run at least three interleaved trials. Bound requests, duration, inflight work and distributed timeouts.
5. Preserve raw observations; generate a tidy CSV/JSON summary without deleting failures or unsupported conditions.

## Controlled experiment

- Independent conditions: local/remote NIC-GPU pair if available.
- Hold constant where applicable: model/tokenizer revision, effective precision, runtime/container, GPU assignment, endpoint semantics, workload seeds and correctness criteria unless the named variable requires a change.
- Measure: latency, CPU/socket traffic, PCIe/NIC counters and topology.
- Record actual rather than requested work (including token counts for serving labs), cache/compile state, trial order, N and variation. Network and profiler conclusions require the relevant capability/permission evidence.

## Evidence and gate

Submit the manifest, exact commands, raw logs, summary table/plot and correctness output. Explain the causal mechanism, account for the critical path, reject at least one competing explanation, and defend the result on one changed case. Offline calculations or simulations remain labeled and cannot satisfy a G1/G2/N2 gate.

## Failure boundary and rebuild

Falsifier: **An analytical NUMA model is labeled as measured network evidence.**

An unsupported feature, missing counter, absent topology, failed correctness check or uncontrolled workload is a useful blocked/negative result, not a speed conclusion. Rebuild the smallest failed invariant, then rerun only the bounded condition needed to discriminate mechanisms.

## Sources

- [Primary release or dated documentation baseline](https://docs.nvidia.com/cuda/archive/13.1.0/cuda-programming-guide/index.html)
- [Runpod Pod constraints](https://docs.runpod.io/pods/overview)
- [Subject outline](../../README.md)
- [T1 lab route and evidence rules](../../../T1-LABS.md)
