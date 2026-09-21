# T1.01-L34 — Diagnosis gauntlet

**CORE · Medium · CPU evidence then G1**
Dependencies: [L16](../lab-16-chunked-prefill/README.md), [L17](../lab-17-prefix-perturbation/README.md), [L19](../lab-19-cancellation/README.md), [L20](../lab-20-cold-starts/README.md), [L25](../lab-25-profiling-ladder/README.md), [L26](../lab-26-clock-reconciliation/README.md), [L33](../lab-33-capacity-goodput/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

Can you falsify a plausible story before changing flags?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Investigate eight cases: slow first response; periodic stream stalls; low-utilization queue; persistently full VRAM; high hit rate with little benefit; throughput up/latency worse; memory saving without TPOT gain; long-prompt interference.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** For each give two hypotheses with differing predictions, choose a discriminating measurement, inspect evidence, propose one intervention and retest. Keep expected diagnoses private.
4. **Analyze and defend, 15–45 active minutes.** Eight incident records with observations, falsifiers, rejected/remaining explanations and uncertainty; measurements must support the conclusion.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Completion evidence

Eight incident records with observations, falsifiers, rejected/remaining explanations and uncertainty; measurements must support the conclusion.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Receive an unseen case with changed workload and implement the diagnostic measurement without the walkthrough.

A plausible written answer without the relevant measurements does not satisfy the GPU diagnosis gate.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Sources and navigation

[scheduler](https://github.com/vllm-project/vllm/blob/v0.29.0/vllm/v1/core/sched/scheduler.py) · [metrics](https://github.com/vllm-project/vllm/blob/v0.29.0/docs/features/per_request_metrics.md)

[Lab index](../README.md) · [Subject](../../README.md)
