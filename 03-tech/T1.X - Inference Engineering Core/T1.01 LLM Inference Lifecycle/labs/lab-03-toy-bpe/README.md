# T1.01-L03 — BPE from scratch

**CORE · Intro · CPU**
Dependencies: [L02](../lab-02-tokenizer-autopsy/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

How do learned merge ranks determine a frozen encoding?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Implement byte-level pair counts, deterministic tie-breaking, merge training, rank-ordered encoding and byte decoding. Keep vocabulary/merge training separate from transformer training.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Use a tiny weighted corpus. Print each learned merge, encode an unseen UTF-8 string using frozen ranks, and round-trip its bytes; include empty input and a tie.
4. **Analyze and defend, 15–45 active minutes.** Your BPE implementation, merge trace and round-trip tests; compare one trace with the production tokenizer without expecting identical segmentation.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Completion evidence

Your BPE implementation, merge trace and round-trip tests; compare one trace with the production tokenizer without expecting identical segmentation.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Change pair frequencies and explain how the next merge changes; demonstrate that serving does not retrain merges.

This toy ignores production normalization/pre-tokenization; it is not the Qwen tokenizer.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[model](https://huggingface.co/Qwen/Qwen3-4B/tree/3101254bbe4169895668a0e7653c3fd1f313576e)

[Lab index](../README.md) · [Subject](../../README.md)
