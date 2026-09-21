# T1.01-L16 — Long-prompt interference

**CORE · Medium · G1**
Dependencies: [L10](../lab-10-prefill-scaling/README.md), [L11](../lab-11-decode-scaling/README.md), [L14](../lab-14-continuous-batching/README.md), [L15](../lab-15-arrival-process/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

Can prompt work disrupt existing interactive decodes?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Start a bounded group of decode-heavy requests, then inject one long prompt at a recorded timestamp. Capture existing-user output timing around the injection.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Compare supported chunked-prefill settings and explicit token budgets. Preserve shapes, arrival timing, sampling and cache state; raise max length explicitly for a16k prompt.
4. **Analyze and defend, 15–45 active minutes.** Interactive latency versus long-request TTFT versus throughput trade-off; retain launch configuration.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Additional coverage check

Explain why later prompt chunks attend to previously computed prefix state instead of acting as independent prompts; report fairness across request classes.

## Completion evidence

Interactive latency versus long-request TTFT versus throughput trade-off; retain launch configuration.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Find a setting that helps one objective but violates another and defend the choice.

No setting is a universal improvement; client buffering must be ruled out.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[scheduler](https://github.com/vllm-project/vllm/blob/v0.29.0/vllm/v1/core/sched/scheduler.py)

[Lab index](../README.md) · [Subject](../../README.md)
