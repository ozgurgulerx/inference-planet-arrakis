# T1.01-L06 — Sampling microscope

**CORE · Intro · CPU and G1**
Dependencies: [L04](../lab-04-reference-generation/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

Can a sampling change secretly change benchmark shape?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Implement greedy and temperature/top-k/top-p filtering on fixed logits; then inspect supported min-p, seed and penalty behavior in the pinned runtime. Keep policies and renormalization explicit.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Compare fixed-logit distributions first. Then hold prompts constant while changing one policy; retain actual output lengths and finish reasons.
4. **Analyze and defend, 15–45 active minutes.** Distribution checks and output-length report; explain why a seed is not universal bitwise reproducibility.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Additional coverage check

Check filtering order, renormalization and invalid all-masked distributions; penalties/constraints do not remove transformer or vocabulary-projection work. Compare actual length and quality before declaring any sampling policy faster.

## Completion evidence

Distribution checks and output-length report; explain why a seed is not universal bitwise reproducibility.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Change temperature or a stop condition and redesign the workload control.

A token-distribution difference alone proves neither task quality nor server speed.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[model](https://huggingface.co/Qwen/Qwen3-4B/tree/3101254bbe4169895668a0e7653c3fd1f313576e)

[Lab index](../README.md) · [Subject](../../README.md)
