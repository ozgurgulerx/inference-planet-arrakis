# T1.01-L09 — Validate the measurement harness

**CORE · Intro · CPU then G1**
Dependencies: [L07](../lab-07-streaming/README.md), [L08](../lab-08-request-autopsy/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

Could the benchmark client itself create the apparent bottleneck?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Validate parser, timeout, HTTP-error, missing-usage, malformed-stream and cancellation paths against the local mock service. Implement an independent timing/count cross-check before remote load.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Use manual shapes A=(128,128), B=(2048,128), C=(6144,128), D=(2048,512), E=(2048,1). Verify actual shapes with tokenizer/usage; approximate text is not exact ISL.
4. **Analyze and defend, 15–45 active minutes.** Raw JSONL, summaries, shape validation and mock tests; report all failures and incomplete streams.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Completion evidence

Raw JSONL, summaries, shape validation and mock tests; report all failures and incomplete streams.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Inject a buffering client and catch the misleading latency conclusion.

A passing mock validates client behavior only; it establishes no GPU or production result.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[bench](https://github.com/vllm-project/vllm/blob/v0.29.0/vllm/benchmarks/serve.py) · [metrics](https://github.com/vllm-project/vllm/blob/v0.29.0/docs/features/per_request_metrics.md)

[Lab index](../README.md) · [Subject](../../README.md)
