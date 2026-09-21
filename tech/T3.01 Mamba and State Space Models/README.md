# T3.01 — Mamba and State Space Models

Status: **Preparation outline — Tier 3 extension**. This defines scope to develop; it does not record completed study or experiments.

[All subjects](../SUBJECTS.md) · [Coverage and order](../COVERAGE.md) · [Research context](../RESEARCH_CONTEXT.md)

## Scope

- SSM mathematics: continuous state dynamics, discretization, recurrence, stability and the distinction between time-invariant and input-selective systems.
- Mamba selective state updates, parallel scan for sequences and recurrent execution during generation; convolution state and implementation-specific layouts.
- State bytes per sequence and layer, batch scaling, precision and state transfer; distinguish fixed-size recurrent state from a token-indexed attention KV history.
- Prefix checkpointing, branching, rollback and cancellation: retaining historical recurrent snapshots consumes additional memory.
- Quality, long-context recall and hardware efficiency; bounded recurrent state is not proof of unlimited effective memory or universal speed superiority.

## Prerequisites and connections

[T1.02](../T1.02%20Transformer%20Inference%20Architecture/README.md), [T1.03](../T1.03%20KV%20Cache%20/README.md), [T1.05](../T1.05%20Prefill%20vs%20Decode%20/README.md), [T1.11](../T1.11%20GPU%20memory%20hierarchy/README.md), [T1.12](../T1.12%20Roofline%20Reasoning%20/README.md). Revisit measurement and controlled comparisons in [T2.33](../T2.33%20Benchmark%20Methodology%20/README.md).

## Changed-case question

With the architecture and batch fixed, double the processed context length. Predict recurrent-state storage and prompt-processing work separately, then explain what changes if arbitrary prefix rollback must be supported. State assumptions, explain the mechanism and identify evidence that could falsify your prediction.

## Source starting points

- [Mamba: Linear-Time Sequence Modeling with Selective State Spaces](https://arxiv.org/abs/2312.00752).
- [Supplied research context](../RESEARCH_CONTEXT.md): lifecycle foundations and the September report’s hybrid/diffusion/omni discussion. The user’s Tier 3 extension supplies this dedicated subject scope.

These are starting points, not claims that every implementation supports the same features. Check the selected model, runtime and version before making deployment or performance claims.
