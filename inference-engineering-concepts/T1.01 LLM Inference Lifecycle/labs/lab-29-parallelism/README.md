# T1.01-L29 — Tensor and data parallelism

**LATER · Expert · G2 topology verified**
Dependencies: [L12](../lab-12-kv-math/README.md), [L15](../lab-15-arrival-process/README.md), [L33](../lab-33-capacity-goodput/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

Does a second GPU improve fit, capacity or latency?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Inventory links/P2P/topology. Compare one GPU with tensor parallel2, then two independent replicas where supported; record per-GPU weights/state and communication.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Freeze checkpoint, precision, workload and SLO. Report total GPU-seconds and host constraints.
4. **Analyze and defend, 15–45 active minutes.** Fit/latency/goodput/cost comparison with actual topology.


## Deferred execution contract

This is a complete experiment specification, not a GPU-tested recipe. Before execution, select the exact supported runtime/backend/checkpoint/connector combination, freeze its command and image digest, and inventory required permissions/topology. Start with the bounded analytical or reduced single-device case above. A simulation or missing hardware does not satisfy the real-system gate. Preserve this ID when adding a verified executable implementation.

## Additional coverage check

Include an analytical pipeline-parallel critical path and bubble comparison alongside actual TP/DP experiments. Replicas do not pool one request’s context automatically; compare equal total GPU budgets.

## Completion evidence

Fit/latency/goodput/cost comparison with actual topology.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Predict a case where communication makes single-user latency worse.

Deferred; GPU count alone does not imply NVLink, RDMA or useful scaling.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[release](https://github.com/vllm-project/vllm/releases/tag/v0.29.0)

[Lab index](../README.md) · [Subject](../../README.md)
