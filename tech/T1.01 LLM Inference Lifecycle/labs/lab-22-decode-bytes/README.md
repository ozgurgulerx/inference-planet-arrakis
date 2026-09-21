# T1.01-L22 — Which bytes dominate decode?

**LATER · Expert · G1 profiling**
Dependencies: [L11](../lab-11-decode-scaling/README.md), [L21](../lab-21-projection-roofline/README.md), [L25](../lab-25-profiling-ladder/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

How can weight traffic, historical KV and launch gaps be distinguished?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Design separate batch, context, weight-byte, KV-byte and launch-mode sweeps. Use targeted attention/projection microbenchmarks where the engine hides the mechanism.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Pin compatible FlashInfer/runtime/kernel tools; compare competing slopes, correctness and observed traffic rather than assuming decode is bandwidth-bound.
4. **Analyze and defend, 15–45 active minutes.** Discriminating-sweep table and selected profiler evidence.


## Deferred execution contract

This is a complete experiment specification, not a GPU-tested recipe. Before execution, select the exact supported runtime/backend/checkpoint/connector combination, freeze its command and image digest, and inventory required permissions/topology. Start with the bounded analytical or reduced single-device case above. A simulation or missing hardware does not satisfy the real-system gate. Preserve this ID when adding a verified executable implementation.

## Completion evidence

Discriminating-sweep table and selected profiler evidence.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Find a workload where the initial bottleneck explanation fails.

Advanced deferred lab; required hardware counters may be unavailable.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[flashinfer](https://github.com/flashinfer-ai/flashinfer)

[Lab index](../README.md) · [Subject](../../README.md)
