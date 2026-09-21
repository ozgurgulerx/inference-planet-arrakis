# T1.01-L18 — Structured decoding

**CORE · Medium · G1**
Dependencies: [L06](../lab-06-sampling/README.md), [L07](../lab-07-streaming/README.md), [L09](../lab-09-harness-validation/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

Does syntactically valid output answer the task correctly?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Compare free generation with a supported explicit JSON-schema/grammar backend. Build a small known-answer fixture set with deterministic validators for syntax and semantics.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Freeze prompts and scoring criteria; record backend compilation/cold state, actual length, validity, accepted tasks and timing.
4. **Analyze and defend, 15–45 active minutes.** Separate syntax-valid and task-accepted rates with failure examples.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Additional coverage check

Explain token-mask/grammar-state advancement and distinguish token healing at a tokenizer-sensitive prompt boundary from structured validation or JSON repair.

## Completion evidence

Separate syntax-valid and task-accepted rates with failure examples.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Construct a syntactically valid but semantically wrong response and catch it.

A grammar does not prove factual correctness or safe tool execution; tools are mocked.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[release](https://github.com/vllm-project/vllm/releases/tag/v0.29.0)

[Lab index](../README.md) · [Subject](../../README.md)
