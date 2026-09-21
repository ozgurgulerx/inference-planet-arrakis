# T1.01-L04 — Instrumented generation loop

**CORE · Intro · G1**
Dependencies: [L01](../lab-01-rendered-prompt/README.md), [L03](../lab-03-toy-bpe/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

Where is the first output selected and when does state grow?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Implement model.forward-based prefill and incremental decode without generate(). Trace input IDs, cache length, last-position logits, selected token, EOS/length checks and elapsed CUDA time where meaningful.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Use the same model/revision and short prompt. Record prefill separately from subsequent forwards; keep sampling simple and deterministic for the accounting experiment.
4. **Analyze and defend, 15–45 active minutes.** Per-step trace and a working generation loop; identify selection versus processing into state.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Additional coverage check

Explain why causal masking permits prompt positions to be processed together while layer dependencies remain. Demonstrate that future appends preserve historical K/V under unchanged prefix/weights/positions, and explain why historical Q is not the cached input needed by a new query.

## Completion evidence

Per-step trace and a working generation loop; identify selection versus processing into state.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Introduce early EOS and show which remaining forwards disappear.

Source traces in the reference loop do not establish identical engine scheduling or performance.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Lifecycle extension boundary

Map multimodal preprocessing/extra positions, hybrid recurrent state, reasoning and tool pauses onto the lifecycle as an analytical extension. The text-only Qwen3-4B baseline cannot demonstrate those other architectures. Select a verified checkpoint and its owning subject before claiming a runnable extension; preserve the distinction in the coverage map.

## Sources and navigation

[config](https://huggingface.co/Qwen/Qwen3-4B/blob/3101254bbe4169895668a0e7653c3fd1f313576e/config.json) · [model](https://huggingface.co/Qwen/Qwen3-4B/tree/3101254bbe4169895668a0e7653c3fd1f313576e)

[Lab index](../README.md) · [Subject](../../README.md)
