# T1.01-L17 — Prefix cache perturbation

**CORE · Medium · G1**
Dependencies: [L13](../lab-13-paged-kv/README.md), [L16](../lab-16-chunked-prefill/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

Which small prompt changes destroy useful prefix reuse?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Compare cold prefix, exact repeat, first-token mutation, suffix mutation, stable system/dynamic user and timestamps at the front/end. Check token-level common prefixes.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Measure query/hit units, executed prompt work, queueing and latency. Record effective block alignment and final-logit recomputation behavior from pinned source.
4. **Analyze and defend, 15–45 active minutes.** Perturbation matrix with token identity and cache/latency evidence.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Additional coverage check

Audit cache identity beyond text equality: compatible weights, adapters, position conventions, modality and representation. Do not assume semantic similarity or a matching suffix permits reuse.

## Completion evidence

Perturbation matrix with token identity and cache/latency evidence.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Design a prompt layout preserving reuse without changing required application semantics.

Cache reuse saves computation; it does not expand logical context or guarantee lower latency.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[kv](https://github.com/vllm-project/vllm/blob/v0.29.0/vllm/v1/core/kv_cache_manager.py)

[Lab index](../README.md) · [Subject](../../README.md)
