# T1.01-L35 — Turkish enterprise inference capstone

**CORE · Medium · G1; scaling branches deferred**
Dependencies: [L18](../lab-18-structured-output/README.md), [L19](../lab-19-cancellation/README.md), [L20](../lab-20-cold-starts/README.md), [L26](../lab-26-clock-reconciliation/README.md), [L33](../lab-33-capacity-goodput/README.md), [L34](../lab-34-diagnosis-gauntlet/README.md)
Execution status: proposed; GPU behavior unverified. Source check: 2026-09-20.

## Engineering question

What is the simplest architecture meeting this workload’s quality and latency needs?

## Environment and controls

Use the [pinned environment and measurement contract](../environment.md). GPU labs use the exact Qwen3-4B/model-tokenizer revision and vLLM0.29 baseline unless this lab explicitly selects a documented extension. Freeze prompt/token fixtures, seed, precision, actual context limit, sampling, cache state and arrival process. Record effective settings, not assumed defaults. Reference methods/expected interpretations stay separate until after an attempt.

## Study and experiment

1. **Prepare, 15–30 active minutes.** Build synthetic Turkish/English system-policy,8–20 tool-schema, RAG-like and identifier fixtures. Mock tool execution. Measure actual rendered lengths and stratify output lengths.
2. **Predict, before hints.** Write two competing explanations and the observation that would falsify each. Record actual expected quantities or direction before running.
3. **Run, bounded session.** Use short/normal/long output targets55/35/10 percent, steady/Poisson/burst arrivals and frozen quality checks. Test prompt layout, caching, chunking and admission independently before proposing more infrastructure.
4. **Analyze and defend, 15–45 active minutes.** Evidence-backed recommendation, capacity/SLO envelope and cost per accepted task; retain failed targets and alternatives.


## Local starter and check

From this lab directory, inspect `starter/task.py` and implement `analyze(evidence)`; run:

```bash
python3 checks/check.py starter/task.py
```

The starter deliberately raises `NotImplementedError`; its check should fail until you implement it. The small offline fixture checks one reasoning invariant, not this lab's GPU/source-code gate. Complete the build task above as a separate implementation. Retain your code and raw evidence in this local ignored workspace. Shared infrastructure is available from the `labs/` directory via `python3 -m _shared.lab_cli --help`.

## Additional coverage check

Classify each chosen component as engine, kernel library or orchestrator. Map emerging adaptive admission/phase/precision/storage controls to the work they save and their coordination costs, without adopting unverified maturity labels.

## Completion evidence

Evidence-backed recommendation, capacity/SLO envelope and cost per accepted task; retain failed targets and alternatives.

Save your own implementation, environment manifest, prediction, commands, raw records, failures, comparison/plot, uncertainty and scope limits. Use the local evidence template. Explain the mechanism, apply it to the changed case and defend what the measurements establish. A passing offline fixture or reading the manual is not completion of a GPU gate.

## Failure boundary and independent rebuild

Rebuild from a blank workspace, change traffic/context mix and defend why the architecture should or should not change.

Synthetic exercise is not production experience; unmet targets may justify a negative feasibility conclusion.

Repeat later with a different workload and without the lab walkthrough. Documentation remains allowed; copying a reference solution is assisted practice. Record only actual observations, never prefill findings or performance numbers.

## Frozen exercise targets

Initial targets: p95 client first-visible-content latency <1500ms; p95 **engine token ITL**, when genuinely observable, <60ms; HTTP failure rate <1%; structured validity >=99%; task acceptance >=95%. These are exercise requirements, not industry standards or promised Qwen performance. If token ITL is unavailable, mark that target unevaluable rather than substituting SSE intervals. Freeze semantic validators and sample populations before testing. Retain a failure or negative-feasibility conclusion rather than tuning the target after seeing results.

Start from measured safe arrival rates; the manual's 0.5/2 RPS and4×10s burst are candidate experiments, not guaranteed-safe starting capacity. Enforce hard request/inflight/duration caps. Larger architecture branches require separate scope/budget.

## Lifecycle extension boundary

Map multimodal preprocessing/extra positions, hybrid recurrent state, reasoning and tool pauses onto the lifecycle as an analytical extension. The text-only Qwen3-4B baseline cannot demonstrate those other architectures. Select a verified checkpoint and its owning subject before claiming a runnable extension; preserve the distinction in the coverage map.

## Sources and navigation

[model](https://huggingface.co/Qwen/Qwen3-4B/tree/3101254bbe4169895668a0e7653c3fd1f313576e) · [metrics](https://github.com/vllm-project/vllm/blob/v0.29.0/docs/features/per_request_metrics.md)

[Lab index](../README.md) · [Subject](../../README.md)
