# T3.04 — Image Diffusion Inference

Status: **Preparation outline — Tier 3 extension**. This defines scope to develop; it does not record completed study or experiments.

[All subjects](../SUBJECTS.md) · [Coverage and order](../COVERAGE.md) · [Research context](../RESEARCH_CONTEXT.md)

## Scope

- Conditioning encoders, latent initialization, repeated denoiser calls and image decoding; distinguish diffusion formulations from related flow-based sampling.
- UNet and DiT backbones: activations, spatial or patch dimensions, attention and convolution costs.
- Noise schedules, numerical sampling algorithms, number of model evaluations and guidance; a diffusion scheduler is distinct from a request scheduler.
- Precision, batching, resolution, offload and intermediate reuse; separate exact reuse from approximations that change output quality.
- End-to-end latency, peak memory, images per second and quality under fixed resolution, sampling configuration and seed policy.

## Prerequisites and connections

[T1.10](../T1.10%20GPU%20Architecture%20Basics/README.md), [T1.11](../T1.11%20GPU%20memory%20hierarchy/README.md), [T1.12](../T1.12%20Roofline%20Reasoning%20/README.md), [T1.32](../T1.32%20Quantization/README.md), [T1.41](../T1.41%20Profiling/README.md), [T2.33](../T2.33%20Benchmark%20Methodology%20/README.md). Revisit measurement and controlled comparisons in [T2.33](../T2.33%20Benchmark%20Methodology%20/README.md).

## Changed-case question

Double width and height while holding sampling steps fixed. Predict latent size and likely memory/compute growth, explicitly stating whether attention is global, local or absent. State assumptions, explain the mechanism and identify evidence that could falsify your prediction.

## Source starting points

- [Diffusers scheduler overview](https://huggingface.co/docs/diffusers/api/schedulers/overview).
- [Scalable Diffusion Models with Transformers (DiT)](https://arxiv.org/abs/2212.09748).
- [Supplied research context](../RESEARCH_CONTEXT.md): lifecycle foundations and the September report’s hybrid/diffusion/omni discussion. The user’s Tier 3 extension supplies this dedicated subject scope.

These are starting points, not claims that every implementation supports the same features. Check the selected model, runtime and version before making deployment or performance claims.
