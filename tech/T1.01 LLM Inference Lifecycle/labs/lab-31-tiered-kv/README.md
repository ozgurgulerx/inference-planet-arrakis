# T1.01-L31 — Tiered and distributed KV

**LATER · Expert · G1+CPU; G2/N2 extension**
Dependencies: [L17](../lab-17-prefix-perturbation/README.md), [L12](../lab-12-kv-math/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

When is fetching state cheaper than recomputing it?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Start with a pinned LMCache CPU-offload integration on one GPU; compare cold recomputation with lookup/copy/restore. Remote Mooncake/store work follows only when selected.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Sweep prefix length and reuse distance; record transfer/restore, capacity, eviction and quality/correctness.
4. **Analyze and defend, 15–45 active minutes.** Measured fetch-versus-recompute break-even with actual cache residency.


## Deferred execution contract

This is a complete experiment specification, not a GPU-tested recipe. Before execution, select the exact supported runtime/backend/checkpoint/connector combination, freeze its command and image digest, and inventory required permissions/topology. Start with the bounded analytical or reduced single-device case above. A simulation or missing hardware does not satisfy the real-system gate. Preserve this ID when adding a verified executable implementation.

## Additional coverage check

Specify ownership/index freshness, eviction, partial transfer failure and recovery. Separate cold-tier compression from hot-cache quantization; test tail latency under realistic reuse distance.

## Completion evidence

Measured fetch-versus-recompute break-even with actual cache residency.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Change reuse distance and predict when the benefit disappears.

Deferred; CPU offload does not establish remote/RDMA capability or end-to-end superiority.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[lmcache](https://github.com/LMCache/LMCache)

[Lab index](../README.md) · [Subject](../../README.md)
