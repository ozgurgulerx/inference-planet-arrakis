# T1.01-L32 — KV-aware routing

**LATER · Expert · CPU simulator then G2**
Dependencies: [L17](../lab-17-prefix-perturbation/README.md), [L15](../lab-15-arrival-process/README.md), [L33](../lab-33-capacity-goodput/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

Can a warm busy replica lose to a cold idle replica?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Implement a small latency-cost simulator, then optionally reproduce with two replicas and a pinned Dynamo/llm-d routing path. Use observed cache/load signals.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Vary queue depth and overlap independently; keep downstream service work comparable.
4. **Analyze and defend, 15–45 active minutes.** Decision boundary from queue+residual+prefill+decode terms, and separate real-replica evidence if run.


## Deferred execution contract

This is a complete experiment specification, not a GPU-tested recipe. Before execution, select the exact supported runtime/backend/checkpoint/connector combination, freeze its command and image digest, and inventory required permissions/topology. Start with the bounded analytical or reduced single-device case above. A simulation or missing hardware does not satisfy the real-system gate. Preserve this ID when adding a verified executable implementation.

## Additional coverage check

Include stale directory, eviction and load-prediction error cases; define a correctness-preserving fallback and count its latency.

## Completion evidence

Decision boundary from queue+residual+prefill+decode terms, and separate real-replica evidence if run.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Construct a case where maximizing hit rate worsens TTFT.

Deferred; simulator timings must not be reported as measured routing results.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[dynamo](https://github.com/ai-dynamo/dynamo) · [router](https://github.com/llm-d/llm-d-router)

[Lab index](../README.md) · [Subject](../../README.md)
