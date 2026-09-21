# T1.01-L28 — Speculative decoding

**LATER · Expert · G1 supported method**
Dependencies: [L05](../lab-05-k-minus-one/README.md), [L06](../lab-06-sampling/README.md), [L11](../lab-11-decode-scaling/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

Does extra draft/verification work reduce time per committed output?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Choose a supported simple n-gram/suffix method before adding another checkpoint. Verify exact v0.29 method flags and instrument accepted versus committed tokens.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Hold actual workload/quality fixed; compare draft/verify cost, target rounds, memory and generation interval.
4. **Analyze and defend, 15–45 active minutes.** Time per committed token and acceptance/cost report with correctness checks.


## Deferred execution contract

This is a complete experiment specification, not a GPU-tested recipe. Before execution, select the exact supported runtime/backend/checkpoint/connector combination, freeze its command and image digest, and inventory required permissions/topology. Start with the bounded analytical or reduced single-device case above. A simulation or missing hardware does not satisfy the real-system gate. Preserve this ID when adding a verified executable implementation.

## Completion evidence

Time per committed token and acceptance/cost report with correctness checks.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Find a high-acceptance workload where total latency fails to improve.

Deferred; acceptance alone is not a speedup and speculative rounds invalidate ordinary K-1 accounting.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[release](https://github.com/vllm-project/vllm/releases/tag/v0.29.0)

[Lab index](../README.md) · [Subject](../../README.md)
