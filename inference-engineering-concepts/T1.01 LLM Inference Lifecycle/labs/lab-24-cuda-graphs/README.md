# T1.01-L24 — CUDA graphs and launch overhead

**LATER · Expert · G1 profiler**
Dependencies: [L20](../lab-20-cold-starts/README.md), [L21](../lab-21-projection-roofline/README.md), [L25](../lab-25-profiling-ladder/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

When does replay reduce launch gaps, and what does it cost?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Compare explicit supported graph mode against NONE in the pinned backend; preserve compilation settings as separately controlled variables. Capture startup and new-shape behavior.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Measure representative timeline windows separately from throughput runs; record graph coverage, memory and collection overhead.
4. **Analyze and defend, 15–45 active minutes.** Launch-gap traces and warm/shape latency comparison.


## Deferred execution contract

This is a complete experiment specification, not a GPU-tested recipe. Before execution, select the exact supported runtime/backend/checkpoint/connector combination, freeze its command and image digest, and inventory required permissions/topology. Start with the bounded analytical or reduced single-device case above. A simulation or missing hardware does not satisfy the real-system gate. Preserve this ID when adding a verified executable implementation.

## Completion evidence

Launch-gap traces and warm/shape latency comparison.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Introduce a shape outside the capture path and diagnose the behavior.

Deferred; enforce-eager can change more than graph replay and is not an isolated control by itself.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[release](https://github.com/vllm-project/vllm/releases/tag/v0.29.0) · [nsight](https://docs.nvidia.com/nsight-systems/UserGuide/)

[Lab index](../README.md) · [Subject](../../README.md)
