# T1.01-L11 — Decode output/history scaling

**CORE · Intro · G1**
Dependencies: [L09](../lab-09-harness-validation/README.md), [L10](../lab-10-prefill-scaling/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

Does decode slow because there is more output or more history?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Sweep OSL at fixed ISL, then independently sweep history at fixed active batch/output length. Record actual outputs, stop reasons and engine generation intervals.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Use OSL1,16,64,128,256,512 within bounded capacity; do not substitute max_tokens for actual OSL. Keep sampling and cache conditions stable.
4. **Analyze and defend, 15–45 active minutes.** E2E/generation versus output length and per-token behavior versus history; handle single-output denominators correctly.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Completion evidence

E2E/generation versus output length and per-token behavior versus history; handle single-output denominators correctly.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Explain a case with unchanged TTFT but higher generation latency.

E2E/OSL is not automatically TPOT; mean intervals cannot establish a tail.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[bench](https://github.com/vllm-project/vllm/blob/v0.29.0/vllm/benchmarks/serve.py) · [metrics](https://github.com/vllm-project/vllm/blob/v0.29.0/docs/features/per_request_metrics.md)

[Lab index](../README.md) · [Subject](../../README.md)
