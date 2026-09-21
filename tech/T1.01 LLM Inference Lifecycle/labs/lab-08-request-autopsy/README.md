# T1.01-L08 — Autopsy one vLLM request

**CORE · Intro · G1**
Dependencies: [L00](../lab-00-environment/README.md), [L04](../lab-04-reference-generation/README.md), [L07](../lab-07-streaming/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

How do textbook phases map onto actual engine control flow?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Inspect the pinned frontend/input processor, V1 scheduler, KV manager, model runner and output processor. Add request-correlated observation points in a separate source checkout; retain a patch and keep behavior unchanged.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** First use one quiet warm request. Capture receive/validate/enqueue/schedule/runner/output/finish events with clock domains. In a batched run record batch membership rather than charging the whole GPU batch independently to each request.
4. **Analyze and defend, 15–45 active minutes.** Source-path map, instrumentation patch and event trace; distinguish unavailable events from inferred ones.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Completion evidence

Source-path map, instrumentation patch and event trace; distinguish unavailable events from inferred ones.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Trace an early cancellation and compare its ownership transitions.

A unified token scheduler need not expose literal prefill/decode branches; CPU call duration is not GPU kernel time.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[scheduler](https://github.com/vllm-project/vllm/blob/v0.29.0/vllm/v1/core/sched/scheduler.py) · [kv](https://github.com/vllm-project/vllm/blob/v0.29.0/vllm/v1/core/kv_cache_manager.py)

[Lab index](../README.md) · [Subject](../../README.md)
