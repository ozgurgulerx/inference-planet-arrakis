# T1.01-L30 — Prefill/decode disaggregation

**LATER · Expert · G2; source-verified connector**
Dependencies: [L16](../lab-16-chunked-prefill/README.md), [L12](../lab-12-kv-math/README.md), [L33](../lab-33-capacity-goodput/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

When does separating phases repay handoff overhead?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** First calculate KV-transfer lower bounds. Then select a pinned supported1P1D recipe and verify actual transport before comparing with aggregated serving.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Measure queueing, bytes, exposed handoff, phase interference and correctness separately. Record bandwidth measurement rather than line-rate marketing numbers.
4. **Analyze and defend, 15–45 active minutes.** Transfer model and real1P1D trace, or explicitly analytical evidence if hardware absent.


## Deferred execution contract

This is a complete experiment specification, not a GPU-tested recipe. Before execution, select the exact supported runtime/backend/checkpoint/connector combination, freeze its command and image digest, and inventory required permissions/topology. Start with the bounded analytical or reduced single-device case above. A simulation or missing hardware does not satisfy the real-system gate. Preserve this ID when adding a verified executable implementation.

## Completion evidence

Transfer model and real1P1D trace, or explicitly analytical evidence if hardware absent.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Find the workload where handoff cancels the interference saving.

Deferred; routing and phase separation are distinct mechanisms. Simulation is not a distributed run.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[dynamo](https://github.com/ai-dynamo/dynamo)

[Lab index](../README.md) · [Subject](../../README.md)
