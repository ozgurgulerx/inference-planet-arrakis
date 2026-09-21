# T3.03 — Diffusion Language Models

Status: **Preparation outline — Tier 3 extension**. This defines scope to develop; it does not record completed study or experiments.

[All subjects](../SUBJECTS.md) · [Coverage and order](../COVERAGE.md) · [Research context](../RESEARCH_CONTEXT.md)

## Scope

- Discrete corruption or masking and iterative reverse generation; model objective, prediction parameterization and conditioning.
- Full-sequence versus blockwise generation, remasking and stopping; distinguish non-autoregressive refinement from an autoregressive outer loop.
- Attention visibility and state validity across refinement steps; do not assume ordinary autoregressive KV reuse remains correct.
- Scheduler budgets for iterations and sequence blocks, variable output lengths, cancellation and when output is stable enough to stream.
- Quality versus denoising budget, total model evaluations and time to a usable answer; parallel token updates do not imply one-pass generation.

## Prerequisites and connections

[T1.01](../T1.01%20LLM%20Inference%20Lifecycle/README.md), [T1.05](../T1.05%20Prefill%20vs%20Decode%20/README.md), [T1.08](../T1.08%20Serving%20metrics/README.md), [T1.34](../T1.34%20Flash%20Attention%20-%20Optimised%20Attention/README.md), [T2.33](../T2.33%20Benchmark%20Methodology%20/README.md). Revisit measurement and controlled comparisons in [T2.33](../T2.33%20Benchmark%20Methodology%20/README.md).

## Changed-case question

Halve the refinement steps while holding output length fixed. What quality and latency evidence would establish a useful improvement, and which cached representations may become stale? State assumptions, explain the mechanism and identify evidence that could falsify your prediction.

## Source starting points

- [Large Language Diffusion Models (LLaDA)](https://arxiv.org/abs/2502.09992).
- [Supplied research context](../RESEARCH_CONTEXT.md): lifecycle foundations and the September report’s hybrid/diffusion/omni discussion. The user’s Tier 3 extension supplies this dedicated subject scope.

These are starting points, not claims that every implementation supports the same features. Check the selected model, runtime and version before making deployment or performance claims.
