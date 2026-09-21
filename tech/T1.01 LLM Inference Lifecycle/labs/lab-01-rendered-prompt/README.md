# T1.01-L01 — Request to rendered prompt

**CORE · Intro · CPU**
Dependencies: [L00](../lab-00-environment/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

How much work is hidden outside the visible user turn?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Load only the pinned tokenizer. Build a request with system text, user text and tool schemas; render once with the required template and thinking disabled. Compare raw-text tokens, rendered tokens and duplicate-special-token mistakes.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Hold content constant; vary system/tools/template representation. Check input plus requested output against the explicit serving limit. Exercise an invalid budget without sending an unbounded request.
4. **Analyze and defend, 15–45 active minutes.** Rendered prompts, token IDs and budget tests; account for all special/schema tokens.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Additional coverage check

Check embedding lookup compatibility: IDs index learned rows, not token positions or embeddings themselves. Equal vocabulary sizes do not establish compatible token meanings. Keep normalization/template/weights/tokenizer coupled.

## Completion evidence

Rendered prompts, token IDs and budget tests; account for all special/schema tokens.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Move a large tool schema into a short request and predict whether context admission changes.

Character counts do not establish token budgets; physical KV capacity is a different constraint.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Lifecycle extension boundary

Map multimodal preprocessing/extra positions, hybrid recurrent state, reasoning and tool pauses onto the lifecycle as an analytical extension. The text-only Qwen3-4B baseline cannot demonstrate those other architectures. Select a verified checkpoint and its owning subject before claiming a runnable extension; preserve the distinction in the coverage map.

## Sources and navigation

[model](https://huggingface.co/Qwen/Qwen3-4B/tree/3101254bbe4169895668a0e7653c3fd1f313576e)

[Lab index](../README.md) · [Subject](../../README.md)
