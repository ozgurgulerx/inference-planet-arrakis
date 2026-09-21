# T1.01-L33 — Little’s Law and goodput

**CORE · Medium · G1**
Dependencies: [L09](../lab-09-harness-validation/README.md), [L15](../lab-15-arrival-process/README.md), [L18](../lab-18-structured-output/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

How much useful work can this serving system sustain?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Define a request population and measurement boundary. In stable windows compare mean in-system requests with completed rate times residence; separate overload transients.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Freeze TTFT/quality/error criteria. Count failures in denominators and report cost per accepted task including idle time/storage where allocated.
4. **Analyze and defend, 15–45 active minutes.** Capacity envelope, steady-state check, goodput and accepted-task cost.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Completion evidence

Capacity envelope, steady-state check, goodput and accepted-task cost.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Show why a high token-throughput configuration can deliver fewer acceptable tasks.

Little’s Law needs consistent populations and steady-state assumptions; SLO thresholds are workload-specific.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[bench](https://github.com/vllm-project/vllm/blob/v0.29.0/vllm/benchmarks/serve.py)

[Lab index](../README.md) · [Subject](../../README.md)
