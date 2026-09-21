# T3.05 — Video Generation Inference

Status: **Preparation outline — Tier 3 extension**. This defines scope to develop; it does not record completed study or experiments.

[All subjects](../SUBJECTS.md) · [Coverage and order](../COVERAGE.md) · [Research context](../RESEARCH_CONTEXT.md)

## Scope

- Extend image generation to temporal/spatial latent representations, temporal conditioning and video encode/decode stages.
- Frame count, frame rate, resolution and temporal compression as separate workload dimensions; temporal coherence as a quality requirement.
- Spatiotemporal attention, intermediate activations and denoising iterations; distinguish global, factorized, windowed and chunked execution.
- Sequence/context parallelism, communication, offload and stage placement; measure communication and peak memory rather than extrapolating only from image size.
- Latency to a usable clip, generated video duration per wall-clock time, quality and continuity; diffusion is a major family, not the only possible video architecture.

## Prerequisites and connections

[T3.04](../T3.04%20Image%20Diffusion%20Inference/README.md), [T1.20](../T1.20%20Collectives/README.md), [T2.17](../T2.17%20Multi-Node%20serving/README.md), [T2.22](../T2.22%20Context%20parallelism/README.md), [T2.25](../T2.25%20Topology-aware%20placement/README.md), [T2.33](../T2.33%20Benchmark%20Methodology%20/README.md). Revisit measurement and controlled comparisons in [T2.33](../T2.33%20Benchmark%20Methodology%20/README.md).

## Changed-case question

Double clip duration at fixed resolution. When could attention cost grow faster than latent storage, and how would temporal chunking change memory, communication and continuity? State assumptions, explain the mechanism and identify evidence that could falsify your prediction.

## Source starting points

- [Wan: Open and Advanced Large-Scale Video Generative Models](https://arxiv.org/abs/2503.20314).
- [Supplied research context](../RESEARCH_CONTEXT.md): lifecycle foundations and the September report’s hybrid/diffusion/omni discussion. The user’s Tier 3 extension supplies this dedicated subject scope.

These are starting points, not claims that every implementation supports the same features. Check the selected model, runtime and version before making deployment or performance claims.
