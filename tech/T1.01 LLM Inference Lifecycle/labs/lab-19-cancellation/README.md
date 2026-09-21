# T1.01-L19 — Backpressure and cancellation

**CORE · Medium · G1**
Dependencies: [L07](../lab-07-streaming/README.md), [L08](../lab-08-request-autopsy/README.md), [L14](../lab-14-continuous-batching/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

Does abandoned delivery stop scheduling and release ownership?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Use slow reading, disconnect after first event and explicit cancellation where supported. Observe request IDs through queue/running/finish and KV ownership.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Compare normal versus slow/abandoned clients with bounded timeouts; distinguish cancellation acknowledgement from observed engine cleanup.
4. **Analyze and defend, 15–45 active minutes.** Client/server traces showing work ceases or persists; pool reservation recorded separately.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Completion evidence

Client/server traces showing work ceases or persists; pool reservation recorded separately.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Cancel while queued and while running, then compare cleanup paths.

Closing an HTTP connection alone is not proof that GPU work stopped.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[scheduler](https://github.com/vllm-project/vllm/blob/v0.29.0/vllm/v1/core/sched/scheduler.py)

[Lab index](../README.md) · [Subject](../../README.md)
