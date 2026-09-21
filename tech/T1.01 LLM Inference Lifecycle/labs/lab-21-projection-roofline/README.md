# T1.01-L21 — Projection arithmetic intensity

**CORE · Medium · CPU and G1**
Dependencies: [L12](../lab-12-kv-math/README.md), [L11](../lab-11-decode-scaling/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

How does increasing rows amortize projection weight traffic?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Derive Q projection dimensions from query heads times head_dim, not hidden size alone. Implement an analytical traffic calculator and a bounded warmed matrix microbenchmark.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** For Qwen use [M,2560]×[2560,4096], M1,8,128,2048. Report2MKO FLOPs and the assumed minimum traffic; measure with CUDA events when authorized.
4. **Analyze and defend, 15–45 active minutes.** Intensity curve and measured elapsed time; state lower-bound traffic assumptions and hardware roof source.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Completion evidence

Intensity curve and measured elapsed time; state lower-bound traffic assumptions and hardware roof source.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Change batch rows and explain which term stops being negligible.

Analytical intensity is not measured HBM traffic. Kernel-counter confirmation is deferred if permissions are absent.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[config](https://huggingface.co/Qwen/Qwen3-4B/blob/3101254bbe4169895668a0e7653c3fd1f313576e/config.json)

[Lab index](../README.md) · [Subject](../../README.md)
