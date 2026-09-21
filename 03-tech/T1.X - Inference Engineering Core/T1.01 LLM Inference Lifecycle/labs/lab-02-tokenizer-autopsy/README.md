# T1.01-L02 — Qwen tokenizer autopsy

**CORE · Intro · CPU**
Dependencies: [L01](../lab-01-rendered-prompt/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

Which representations inflate this tokenizer’s workload?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Inspect tokenizer artifacts and special tokens. Build matched Turkish/English examples and code, URLs, UUIDs and formatted/minified JSON. Record codepoints, UTF-8 bytes, token IDs and reconstructed text.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Freeze tokenizer revision and semantic content where possible. Compare tokens/1000 codepoints, bytes/token and rendered inflation; label corpus and normalization.
4. **Analyze and defend, 15–45 active minutes.** Fertility CSV, histogram and tokenizer identity; distinguish semantic matching from equal character counts.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Additional coverage check

Compare a hand trace of BPE merge ranks with WordPiece longest-vocabulary matching and Unigram segmentation scores; SentencePiece is a toolkit, not a fourth interchangeable algorithm. Inspect normalization before claiming exact text round trips. Explain the embedding/LM-head cost versus sequence-length trade-off of vocabulary size.

## Completion evidence

Fertility CSV, histogram and tokenizer identity; distinguish semantic matching from equal character counts.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Add previously unseen identifiers and test whether the predicted ordering survives.

A few examples cannot establish a language-wide fertility or model-quality claim.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[model](https://huggingface.co/Qwen/Qwen3-4B/tree/3101254bbe4169895668a0e7653c3fd1f313576e)

[Lab index](../README.md) · [Subject](../../README.md)
