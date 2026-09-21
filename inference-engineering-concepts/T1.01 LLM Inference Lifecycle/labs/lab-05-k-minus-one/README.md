# T1.01-L05 — Prove the K-minus-one relationship

**CORE · Intro · G1**
Dependencies: [L04](../lab-04-reference-generation/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

How many post-prefill forwards are necessary for K selected outputs?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Instrument your loop’s forward count for K=1,2,8,32. Stop before computing unused logits; count actual outputs if EOS intervenes.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Hold prompt and selected-output accounting constant. Compare forward counts and processed positions, explicitly stating ordinary non-speculative assumptions.
4. **Analyze and defend, 15–45 active minutes.** Assertion for actual K>=1: post-prefill forwards=K-1 and processed positions=P+K-1; include K=1.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Completion evidence

Assertion for actual K>=1: post-prefill forwards=K-1 and processed positions=P+K-1; include K=1.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Diagnose a loop that executes one unnecessary final forward.

Beam search, speculation and implementations with extra work need different accounting.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[config](https://huggingface.co/Qwen/Qwen3-4B/blob/3101254bbe4169895668a0e7653c3fd1f313576e/config.json)

[Lab index](../README.md) · [Subject](../../README.md)
