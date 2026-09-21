# T1.01-L00 — Environment fingerprint

**CORE · Intro · CPU then G1**
Dependencies: None
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

Which differences invalidate a supposedly repeated experiment?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Record GPU/VRAM, host RAM, driver, runtime CUDA, Python/Torch/vLLM, image digest, effective launch flags and the currently served model. Inspect existing listeners before starting a server; preserve unfamiliar processes.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Run the inventory twice and diff stable identity/configuration fields; separate volatile utilization from identity. Save CLI help and /v1/models after an authorized startup.
4. **Analyze and defend, 15–45 active minutes.** manifest.json, CLI help and identity diff; explain driver versus runtime and model-name versus revision.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Completion evidence

manifest.json, CLI help and identity diff; explain driver versus runtime and model-name versus revision.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Rebuild on a new Pod from your recorded environment, then identify an intentional version mismatch.

GPU absence in a local manifest does not validate a remote GPU environment.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[release](https://github.com/vllm-project/vllm/releases/tag/v0.29.0)

[Lab index](../README.md) · [Subject](../../README.md)
