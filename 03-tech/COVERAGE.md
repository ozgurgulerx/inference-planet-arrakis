# Coverage, priorities and workload contexts

[All 101 subjects](SUBJECTS.md) · [Source context](RESEARCH_CONTEXT.md)

This is a coverage plan, not a record of completed preparation. T1.01 and T1.02 have expanded notes; the remaining subjects have explicit scope outlines and changed-case questions. Subject coverage does not imply production experience or a professional percentile.

## Preparation order

1. **Lifecycle and measurement spine:** T1.01, T1.03–T1.09, T1.41–T1.45, T2.33. Explain state ownership and measure queueing, execution and client-visible behavior before choosing optimizations.
2. **Hardware and movement:** T1.10–T1.14, T1.21–T1.28; then T1.15–T1.20 and T1.29–T1.31. Relate memory capacity, bandwidth, synchronization and topology to a concrete workload.
3. **Model and runtime choices:** T1.02, T1.32–T1.40; T2.05–T2.08 and T2.19–T2.22. Compare quality, compatibility, state footprint and useful throughput.
4. **Production control:** T2.12–T2.18 and T2.24–T2.25. Include cancellation, tenant isolation, bounded queues, cold starts, failures and rollout behavior.
5. **Choose specialist branches:** kernels/compiler work T2.01–T2.04; storage T2.09–T2.11; advanced MoE T2.23; post-training/rollouts T2.26–T2.31; AMD portability T2.32. These are valuable when the workload requires them, not prerequisites for every deployment.

6. **Architecture and modality extensions:** T3.01 SSM/Mamba plus recurrent linear attention and Gated DeltaNet → T3.02 hybrid recurrent/attention models; T3.03 diffusion language models; T3.04 image diffusion → T3.05 video generation; T3.06 multimodal → T3.07 speech. These are branches after the relevant foundations, not a requirement to finish every T2 subject first.

7. **Deferred pretraining map:** T4.01–T4.16 cover the model-building lifecycle, data, objectives, optimization, scaling and training systems. Use them when an inference question depends on how weights were produced; they are not part of the current inference-first study sequence.

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
| Hybrid state, diffusion and multimodal stage graphs | T1.01–T1.03, T1.16, T1.29–T1.31; T2.12, T2.17–T2.18, T2.21–T2.22; T3.01–T3.07 | Identify each stage's state and scheduling contract. Recurrent state is not ordinary KV; diffusion execution cannot be assumed to follow a token-by-token autoregressive loop. |
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
| Multimodal and hybrid models | T1.01–T1.03, T1.16, T2.17–T2.22, T3.01–T3.02, T3.06 | Which preprocessing, encoder, decoder and non-KV state costs were omitted? |
| High-throughput offline work | T1.06, T1.09, T1.44–T1.45, T2.33 | Does relaxing latency change the preferred batch, parallelism or precision? |
| Bursty multi-tenant serving | T1.06–T1.09, T2.12–T2.18, T2.20 | Who gets delayed or rejected, and are isolation and fairness preserved? |
| Multi-node MoE | T1.18–T1.20, T1.24–T1.28, T1.35, T2.23–T2.25 | Does skew or congestion dominate after increasing the expert-parallel group? |
| Cold starts, updates and failures | T2.15–T2.20, T2.30 | What happens to partially streamed requests and state from an old weight version? |
| RL rollout generation | T2.26–T2.31 | Does faster generation increase policy staleness or interfere with training? |
| Pretraining-system diagnosis | T4.01–T4.16 | Which data, objective, optimizer, scaling or distributed-training assumption explains the observed checkpoint behavior, and what evidence would distinguish it from an inference problem? |
| Alternate GPU platform | T2.32, T1.41–T1.45, T2.33 | Are quality, workload, precision and software support equivalent in the comparison? |
| Iterative language generation | T3.03, T1.08, T2.33 | Do fewer iterations preserve useful quality, and when is output stable enough to stream? |
| Image and video generation | T3.04–T3.05, T2.22, T2.25, T2.33 | Which costs scale with resolution, duration, attention layout and model evaluations? |
| Real-time speech | T3.07, T2.13–T2.14, T2.18 | Can good average throughput hide audio gaps or failed interruption handling? |

## Remaining coverage boundaries

Security and correctness cut across the catalog: request limits and validation (T1.01/T2.13), tenant-safe cache identity (T2.07–T2.08), adapter isolation (T2.20), cancellation/backpressure (T2.14), and versioned state/weights (T2.18/T2.30). These are preparation requirements, not assertions that a particular implementation provides them.

The catalog does not yet contain full lessons or validated labs for 99 outline subjects. T3.01–T3.07 provide dedicated architecture and modality extension outlines connected to the T1/T2 serving foundations. T4.01–T4.16 provide a deliberately deferred pretraining map rather than a complete training course. Source-suggested frontier techniques need individual primary-paper review before detailed implementation or benchmark claims are added.
