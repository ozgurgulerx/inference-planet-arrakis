# T1.01-L27 — Preemption and overload

**LATER · Expert · G1**
Dependencies: [L13](../lab-13-paged-kv/README.md), [L15](../lab-15-arrival-process/README.md), [L26](../lab-26-clock-reconciliation/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

Is work waiting, preempted or failing due to capacity?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Inspect the pinned scheduler’s actual preemption path. Increase state demand gradually with hard duration/inflight/request limits and a recoverable baseline.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Observe waiting/running/preemption/error counters and state pressure. Stop before destructive or unbounded OOM loops.
4. **Analyze and defend, 15–45 active minutes.** Classification backed by engine evidence and a recovery/retest.


## Deferred execution contract

This is a complete experiment specification, not a GPU-tested recipe. Before execution, select the exact supported runtime/backend/checkpoint/connector combination, freeze its command and image digest, and inventory required permissions/topology. Start with the bounded analytical or reduced single-device case above. A simulation or missing hardware does not satisfy the real-system gate. Preserve this ID when adding a verified executable implementation.

## Completion evidence

Classification backed by engine evidence and a recovery/retest.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Contrast overloaded admission with preemption of already-running state.

Deferred; do not deliberately disrupt shared resources or call every latency spike OOM.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[scheduler](https://github.com/vllm-project/vllm/blob/v0.29.0/vllm/v1/core/sched/scheduler.py)

[Lab index](../README.md) · [Subject](../../README.md)
