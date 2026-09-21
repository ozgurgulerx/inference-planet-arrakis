# T1.01-L14 — Continuous batching movie

**CORE · Medium · G1**
Dependencies: [L08](../lab-08-request-autopsy/README.md), [L09](../lab-09-harness-validation/README.md), [L13](../lab-13-paged-kv/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

How does active batch membership change while requests remain alive?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Submit staggered short/long requests. Produce an iteration table with request IDs, prompt/decode work, running/waiting state, scheduled tokens and KV occupancy.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Sweep concurrency1,2,4,8,16 only while within bounded capacity. Hold shapes and token budgets constant; then vary one scheduler budget.
4. **Analyze and defend, 15–45 active minutes.** Scheduler heatmap plus latency/throughput curve showing entries and exits.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Completion evidence

Scheduler heatmap plus latency/throughput curve showing entries and exits.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Predict membership after one request terminates early and another arrives.

Concurrency is not active sequences, and aggregate throughput alone does not reveal batching behavior.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[scheduler](https://github.com/vllm-project/vllm/blob/v0.29.0/vllm/v1/core/sched/scheduler.py)

[Lab index](../README.md) · [Subject](../../README.md)
