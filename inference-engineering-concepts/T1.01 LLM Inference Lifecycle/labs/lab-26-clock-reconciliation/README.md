# T1.01-L26 — Four-clock observation reconciliation

**CORE · Medium · G1**
Dependencies: [L07](../lab-07-streaming/README.md), [L08](../lab-08-request-autopsy/README.md), [L25](../lab-25-profiling-ladder/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

Why can honest tools disagree about request latency?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Correlate raw client records, optional engine request metrics, aggregate scrapes and GPU timelines by request/batch identity and event semantics.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Record every clock domain and boundary. Compare intervals within one domain; measure per-request-metrics enabled versus disabled overhead.
4. **Analyze and defend, 15–45 active minutes.** Boundary dictionary and explanation of discrepancies; missing events remain null, not zero.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Additional coverage check

Compare request-weighted mean TPOT with gap-weighted TPOT on unequal output lengths, and explain why a percentile of request means differs from a percentile of individual gaps. Never add component p95 values to obtain E2E p95.

## Completion evidence

Boundary dictionary and explanation of discrepancies; missing events remain null, not zero.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Reconcile first metadata event with first visible content and first engine output.

Prometheus aggregates are not request traces; wall-clock and monotonic timestamps cannot be cross-subtracted.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[metrics](https://github.com/vllm-project/vllm/blob/v0.29.0/docs/features/per_request_metrics.md)

[Lab index](../README.md) · [Subject](../../README.md)
