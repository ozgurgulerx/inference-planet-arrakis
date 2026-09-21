# T1.01-L13 — Paged KV and block rounding

**CORE · Medium · G1**
Dependencies: [L12](../lab-12-kv-math/README.md), [L08](../lab-08-request-autopsy/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

Where do logical positions diverge from physical allocation?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Read the effective runtime block configuration. Probe lengths immediately below/at/above boundaries; trace active references, reusable blocks and release behavior.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Change only sequence length, then number of active sequences. Separate allocator reservation, physical occupancy and shared prefixes.
4. **Analyze and defend, 15–45 active minutes.** Allocation step plot and state-ownership explanation with observed block size.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Additional coverage check

Trace logical-block→physical-block mapping, references, partial tails, eviction and reusable-but-unowned blocks. Paging is allocation, not compression, added context length or reduced historical attention work.

## Completion evidence

Allocation step plot and state-ownership explanation with observed block size.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Complete one of two sharing requests and explain which references and blocks can remain.

Do not hard-code a historical block size or infer a leak from a retained pool.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[kv](https://github.com/vllm-project/vllm/blob/v0.29.0/vllm/v1/core/kv_cache_manager.py)

[Lab index](../README.md) · [Subject](../../README.md)
