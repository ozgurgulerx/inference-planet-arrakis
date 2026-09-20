# Coverage, priorities and workload contexts

[All 78 subjects](SUBJECTS.md) · [Source context](RESEARCH_CONTEXT.md)

This is a coverage plan, not a record of completed preparation. T1.01 has expanded notes; the remaining subjects have explicit scope outlines and changed-case questions. Subject coverage does not imply production experience or a professional percentile.

## Preparation order

1. **Lifecycle and measurement spine:** T1.01, T1.03–T1.09, T1.41–T1.45, T2.33. Explain state ownership and measure queueing, execution and client-visible behavior before choosing optimizations.
2. **Hardware and movement:** T1.10–T1.14, T1.21–T1.28; then T1.15–T1.20 and T1.29–T1.31. Relate memory capacity, bandwidth, synchronization and topology to a concrete workload.
3. **Model and runtime choices:** T1.02, T1.32–T1.40; T2.05–T2.08 and T2.19–T2.22. Compare quality, compatibility, state footprint and useful throughput.
4. **Production control:** T2.12–T2.18 and T2.24–T2.25. Include cancellation, tenant isolation, bounded queues, cold starts, failures and rollout behavior.
5. **Choose specialist branches:** kernels/compiler work T2.01–T2.04; storage T2.09–T2.11; advanced MoE T2.23; post-training/rollouts T2.26–T2.31; AMD portability T2.32. These are valuable when the workload requires them, not prerequisites for every deployment.

For each subject, progress from explaining the mechanism to a calculation or prediction, then a diagnosis and a decision under changed constraints. Record assumptions and falsifying evidence. Do not substitute a memorized answer for an independently reasoned changed case.

## Research-driven additions within existing subjects

The September report supplies a hypothesis and topic inventory. Priority below is an engineering judgment about when to investigate, not an independently measured adoption ranking. See the source context for primary references and version caveats.

| Theme | Subjects | Consequential boundary / preparation objective |
|---|---|---|
| Prefill/decode disaggregation and phase specialization | T1.05, T1.29–T1.31, T1.40; T2.13–T2.18, T2.24–T2.25 | Compare independent phase sizing against state-transfer time, pool queues and network cost. Colocation can win at low load or expensive handoff. |
| Distributed context memory and tiered KV | T1.03–T1.04, T1.29–T1.30, T1.38; T2.07–T2.11, T2.18 | Calculate lookup + transfer versus recompute. Distinguish per-instance GPU/host caches from genuinely shared storage; include invalidation, model identity and lost state. |
| Wide expert parallelism and attention–FFN disaggregation | T1.18–T1.20, T1.35; T2.23–T2.25 | Measure expert skew, all-to-all and dispatch/combine. Treat sub-layer disaggregation as an emerging design to evaluate, not a universal MoE requirement. |
| Learned, MTP and adaptive speculation | T1.36; T2.04, T2.21, T2.23, T2.33 | Measure draft cost, accepted tokens per verification, verification cost and resulting quality/correctness semantics. Acceptance alone does not prove a speedup. |
| FP8, FP4 and mixed precision across weights, activations and KV | T1.10–T1.12, T1.32, T1.39; T2.02, T2.05–T2.06 | Separate weight-only and activation/KV formats; check native kernels and model support. Validate task quality and long-context behavior as well as capacity. |
| Stateful routing and session-aware control | T1.06, T1.09, T1.40; T2.07–T2.08, T2.12–T2.18 | Balance cache affinity against queue length, topology and fairness. Include tool pauses, adapter/model identity and client cancellation. |
| GPU-resident control and persistent kernels | T1.13–T1.14, T1.41–T1.43; T2.01–T2.04 | Establish that host/launch overhead is material first. Compare flexibility, occupancy, supported shapes and operational debuggability. Full CPU-bypass designs remain a research branch. |
| Hybrid state, diffusion and multimodal stage graphs | T1.01–T1.03, T1.16, T1.29–T1.31; T2.12, T2.17–T2.18, T2.21–T2.22 | Identify each stage's state and scheduling contract. Recurrent state is not ordinary KV; diffusion execution cannot be assumed to follow a token-by-token autoregressive loop. |
| Sparse attention and adaptive KV compression | T1.03–T1.04, T1.34; T2.05–T2.06, T2.21, T2.33 | Separate exact IO optimization from approximate selection/compression. Test quality and selection overhead; paper gains do not establish portable production gains. |

All themes also require T1.08–T1.09 and T2.33: representative distributions, SLO goodput, quality and controlled comparisons.

## Coverage across workload contexts

| Context | Primary subjects | Changed-case pressure test |
|---|---|---|
| Short interactive chat | T1.05–T1.09, T1.36, T2.12–T2.14 | Does speculation still help when queueing or draft overhead dominates? |
| Long prompts / RAG | T1.03–T1.07, T2.07–T2.09, T2.21–T2.22 | Do cache reuse and chunking still help with cold, unique prefixes? |
| Long output / reasoning | T1.05, T1.08, T1.36, T1.44 | How do output-length tails change admission, state residence and capacity? |
| Repeated prefixes and multi-turn sessions | T2.07–T2.08, T2.12–T2.15 | When does affinity overload a worker; what invalidates cached state? |
| Agents and tool pauses | T1.01, T1.09, T2.08–T2.09, T2.12–T2.14 | Should state stay resident while a tool runs, and who owns cancellation? |
| Constrained / structured generation | T1.01, T1.09, T1.37–T1.39, T2.33 | How do constraint-processing overhead and invalid outputs affect useful goodput? |
| Multimodal and hybrid models | T1.01–T1.03, T1.16, T2.17–T2.22 | Which preprocessing, encoder, decoder and non-KV state costs were omitted? |
| High-throughput offline work | T1.06, T1.09, T1.44–T1.45, T2.33 | Does relaxing latency change the preferred batch, parallelism or precision? |
| Bursty multi-tenant serving | T1.06–T1.09, T2.12–T2.18, T2.20 | Who gets delayed or rejected, and are isolation and fairness preserved? |
| Multi-node MoE | T1.18–T1.20, T1.24–T1.28, T1.35, T2.23–T2.25 | Does skew or congestion dominate after increasing the expert-parallel group? |
| Cold starts, updates and failures | T2.15–T2.20, T2.30 | What happens to partially streamed requests and state from an old weight version? |
| RL rollout generation | T2.26–T2.31 | Does faster generation increase policy staleness or interfere with training? |
| Alternate GPU platform | T2.32, T1.41–T1.45, T2.33 | Are quality, workload, precision and software support equivalent in the comparison? |

## Remaining coverage boundaries

Security and correctness cut across the catalog: request limits and validation (T1.01/T2.13), tenant-safe cache identity (T2.07–T2.08), adapter isolation (T2.20), cancellation/backpressure (T2.14), and versioned state/weights (T2.18/T2.30). These are preparation requirements, not assertions that a particular implementation provides them.

The catalog does not yet contain full lessons or validated labs for 77 outline subjects. Multimodal, hybrid and diffusion serving are cross-cutting extensions rather than dedicated curricula. Compiler, networking, storage, distributed training and security are included to explain inference consequences; complete specialist curricula would require separate scope. Source-suggested frontier techniques need individual primary-paper review before detailed implementation or benchmark claims are added.
