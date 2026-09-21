# T1.01-L12 — KV bytes per token

**CORE · Intro · CPU then G1**
Dependencies: [L04](../lab-04-reference-generation/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

How much state does the architecture require per position?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Derive K/V bytes from config layers, KV heads, head dimension and element size. Keep query heads separate. Validate the pinned dimensions before calculating.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Compare analytical logical state with engine KV capacity/usage and process VRAM. State whether weights, graph/workspace and block slack are included.
4. **Analyze and defend, 15–45 active minutes.** Derivation and measured capacity reconciliation; Qwen baseline raw BF16 term=144KiB/token.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Completion evidence

Derivation and measured capacity reconciliation; Qwen baseline raw BF16 term=144KiB/token.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Repeat for a changed KV-head count or dtype and predict the direction before calculating.

Raw KV arithmetic is not an equality for nvidia-smi memory and does not guarantee admission.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[config](https://huggingface.co/Qwen/Qwen3-4B/blob/3101254bbe4169895668a0e7653c3fd1f313576e/config.json)

[Lab index](../README.md) · [Subject](../../README.md)
