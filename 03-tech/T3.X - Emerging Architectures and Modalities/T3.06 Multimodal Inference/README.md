# T3.06 — Multimodal Inference

Status: **Preparation outline — Tier 3 extension**. This defines scope to develop; it does not record completed study or experiments.

[All subjects](../../SUBJECTS.md) · [Coverage and order](../../COVERAGE.md) · [Research context](../../RESEARCH_CONTEXT.md)

## Scope

- Media preprocessing, encoders, projectors, language generation and output decoders as a heterogeneous execution graph.
- Vision-token counts, crops, patches, variable image sizes and video frames; text token count alone does not specify prefill work.
- Encoder/LLM disaggregation and stage overlap: placement, independently sized pools, intermediate-tensor transfer and queueing.
- Caching encoded media and prefixes with model, processor and input identity; ownership, backpressure and cancellation across stages.
- Stage-level latency and quality, modality-specific batching and admission limits; separate understanding inputs from generating media outputs.

## Prerequisites and connections

[T1.01](../../T1.X%20-%20Inference%20Engineering%20Core/T1.01%20LLM%20Inference%20Lifecycle/README.md), [T1.09](../../T1.X%20-%20Inference%20Engineering%20Core/T1.09%20Workload%20Characterization/README.md), [T1.16](../../T1.X%20-%20Inference%20Engineering%20Core/T1.16%20Pipeline%20paralellism/README.md), [T1.29](../../T1.X%20-%20Inference%20Engineering%20Core/T1.29%20KV%20transfer/README.md), [T1.31](../../T1.X%20-%20Inference%20Engineering%20Core/T1.31%20prefill%20/README.md), [T2.12](../../T2.X%20-%20Inference%20Engineering%20Extended/T2.12%20Advanced%20Routing/README.md), [T2.14](../../T2.X%20-%20Inference%20Engineering%20Extended/T2.14%20Backpressure/README.md). Revisit measurement and controlled comparisons in [T2.33](../../T2.X%20-%20Inference%20Engineering%20Extended/T2.33%20Benchmark%20Methodology%20/README.md).

## Changed-case question

Two requests have identical text lengths but different image resolutions and crop counts. Which stage measurements distinguish encoder saturation, transfer delay and LLM prefill pressure? State assumptions, explain the mechanism and identify evidence that could falsify your prediction.

## Source starting points

- [vLLM-Omni v0.22.0](https://docs.vllm.ai/projects/vllm-omni/en/v0.22.0/).
- [Supplied research context](../../RESEARCH_CONTEXT.md): lifecycle foundations and the September report’s hybrid/diffusion/omni discussion. The user’s Tier 3 extension supplies this dedicated subject scope.

These are starting points, not claims that every implementation supports the same features. Check the selected model, runtime and version before making deployment or performance claims.
