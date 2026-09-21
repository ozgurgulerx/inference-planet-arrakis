# T1.01-L15 — Open-loop versus closed-loop load

**CORE · Medium · G1**
Dependencies: [L09](../lab-09-harness-validation/README.md), [L14](../lab-14-continuous-batching/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

Can self-throttling clients hide overload?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Implement fixed-concurrency closed-loop arrivals and independent seeded Poisson arrivals. Record scheduled and actual send time, generator lag and any skipped submissions.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Sweep offered rate across a bounded saturation region. Set hard request/duration/inflight limits; never convert an open-loop test silently into queued client-side closed-loop load.
4. **Analyze and defend, 15–45 active minutes.** Offered/submitted/completed rates, queue residence, errors and tail samples; reconcile counts.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Completion evidence

Offered/submitted/completed rates, queue residence, errors and tail samples; reconcile counts.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Repeat with a short burst and explain whether overload is server-side or in the generator.

Little’s Law and steady-state averages are not valid over arbitrary overload transients.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[bench](https://github.com/vllm-project/vllm/blob/v0.29.0/vllm/benchmarks/serve.py)

[Lab index](../README.md) · [Subject](../../README.md)
