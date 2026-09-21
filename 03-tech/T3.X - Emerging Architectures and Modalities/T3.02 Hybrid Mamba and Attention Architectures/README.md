# T3.02 — Hybrid Mamba and Attention Architectures

Status: **Preparation outline — Tier 3 extension**. This defines scope to develop; it does not record completed study or experiments.

[All subjects](../../SUBJECTS.md) · [Coverage and order](../../COVERAGE.md) · [Research context](../../RESEARCH_CONTEXT.md)

## Scope

- Layer composition and model-specific roles of recurrent/state-space, convolution, attention and optional expert components.
- Memory accounting for recurrent state plus attention KV; distinguish full attention, windowed attention and saved recurrent checkpoints.
- Prefill, decode, prefix reuse and speculative rollback across heterogeneous state representations.
- Allocator, scheduler and prefill/decode handoff contracts: transfer a consistent state at the same sequence position across every layer.
- Compare attention fraction, context length, precision and quality; fewer attention layers do not eliminate KV growth in remaining full-attention layers.

## Prerequisites and connections

[T3.01](../T3.01%20Mamba%20and%20State%20Space%20Models/README.md), [T1.03](../../T1.X%20-%20Inference%20Engineering%20Core/T1.03%20KV%20Cache%20/README.md), [T1.29](../../T1.X%20-%20Inference%20Engineering%20Core/T1.29%20KV%20transfer/README.md), [T1.31](../../T1.X%20-%20Inference%20Engineering%20Core/T1.31%20prefill%20/README.md), [T2.07](../../T2.X%20-%20Inference%20Engineering%20Extended/T2.07%20Prefix%20Caching/README.md), [T2.18](../../T2.X%20-%20Inference%20Engineering%20Extended/T2.18%20Fault%20Tolerance/README.md). Revisit measurement and controlled comparisons in [T2.33](../../T2.X%20-%20Inference%20Engineering%20Extended/T2.33%20Benchmark%20Methodology%20/README.md).

## Changed-case question

A hybrid replaces most attention layers with SSM layers. Which memory terms still grow with context, and what must a worker handoff preserve beyond KV tensors? State assumptions, explain the mechanism and identify evidence that could falsify your prediction.

## Source starting points

- [Jamba: A Hybrid Transformer-Mamba Language Model](https://arxiv.org/abs/2403.19887).
- [Supplied research context](../../RESEARCH_CONTEXT.md): lifecycle foundations and the September report’s hybrid/diffusion/omni discussion. The user’s Tier 3 extension supplies this dedicated subject scope.

These are starting points, not claims that every implementation supports the same features. Check the selected model, runtime and version before making deployment or performance claims.
