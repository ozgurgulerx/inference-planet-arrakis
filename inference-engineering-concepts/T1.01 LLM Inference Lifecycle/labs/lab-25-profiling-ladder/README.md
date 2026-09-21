# T1.01-L25 — Profiling ladder

**CORE · Medium · G1; profiler permissions conditional**
Dependencies: [L08](../lab-08-request-autopsy/README.md), [L09](../lab-09-harness-validation/README.md), [L21](../lab-21-projection-roofline/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

What can each observation tool establish?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Collect nvidia-smi, engine/Prometheus metrics and a short supported PyTorch trace. Inventory DCGM/Nsight availability before attempting a selected deeper collection.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Use the same bounded A/B/C shapes. Profile separately, record overhead and compare tool time resolution and scope.
4. **Analyze and defend, 15–45 active minutes.** Capability matrix and available trace; mark inaccessible counter conclusions blocked. Distinguish utilization from causation.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Completion evidence

Capability matrix and available trace; mark inaccessible counter conclusions blocked. Distinguish utilization from causation.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Choose the cheapest observation that separates a CPU-launch explanation from long kernels.

No requirement to install six tools blindly; utilization alone cannot prove compute or bandwidth limitation.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[profiler](https://docs.pytorch.org/docs/stable/profiler.html) · [nsight](https://docs.nvidia.com/nsight-systems/UserGuide/)

[Lab index](../README.md) · [Subject](../../README.md)
