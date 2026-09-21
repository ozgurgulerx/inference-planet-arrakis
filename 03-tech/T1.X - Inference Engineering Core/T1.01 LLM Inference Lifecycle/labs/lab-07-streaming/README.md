# T1.01-L07 — Token versus text versus SSE

**CORE · Intro · CPU then G1**
Dependencies: [L04](../lab-04-reference-generation/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

What does the client actually receive when a model emits tokens?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Capture raw SSE frames, timestamps, content/reasoning/metadata and final usage. Feed parser fixtures split within UTF-8 characters and across event boundaries. Compare protocol events with tokenizer IDs.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Use Turkish text, empty metadata deltas and stop strings crossing output fragments. Record first event separately from first visible content; include missing usage.
4. **Analyze and defend, 15–45 active minutes.** Raw stream plus event/content/usage counts; no inferred output token count from chunk count.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Completion evidence

Raw stream plus event/content/usage counts; no inferred output token count from chunk count.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Throttle reads and explain which observed gaps belong to delivery rather than token generation.

Client inter-chunk gaps are not engine token ITL.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[metrics](https://github.com/vllm-project/vllm/blob/v0.29.0/docs/features/per_request_metrics.md)

[Lab index](../README.md) · [Subject](../../README.md)
