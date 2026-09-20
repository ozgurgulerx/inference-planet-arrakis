# T3.07 — Speech Inference

Status: **Preparation outline — Tier 3 extension**. This defines scope to develop; it does not record completed study or experiments.

[All subjects](../SUBJECTS.md) · [Coverage and order](../COVERAGE.md) · [Research context](../RESEARCH_CONTEXT.md)

## Scope

- ASR, text-to-speech and speech-to-speech pipelines; distinguish cascaded components from joint speech/text models.
- Audio frames, sample rates, codecs, chunk sizes and lookahead; model latency versus buffering, transport and playback latency.
- Streaming state, endpointing and turn-taking, interruption and full-duplex operation; cancel obsolete output while preserving session consistency.
- Concurrent-stream admission, jitter and backpressure; real-time factor, first-audio latency, transcript stability and output gaps.
- Accuracy, intelligibility and conversational quality alongside latency; average real-time factor below one does not rule out playback underruns.

## Prerequisites and connections

[T3.06](../T3.06%20Multimodal%20Inference/README.md), [T1.08](../T1.08%20Serving%20metrics/README.md), [T1.09](../T1.09%20Workload%20Characterization/README.md), [T2.13](../T2.13%20Admission%20Control/README.md), [T2.14](../T2.14%20Backpressure/README.md), [T2.18](../T2.18%20Fault%20Tolerance/README.md). Revisit measurement and controlled comparisons in [T2.33](../T2.33%20Benchmark%20Methodology%20/README.md).

## Changed-case question

Average synthesis is faster than real time, yet playback repeatedly stalls. Which chunk-latency, buffering and transport measurements distinguish the causes, and what happens when the user interrupts? State assumptions, explain the mechanism and identify evidence that could falsify your prediction.

## Source starting points

- [Moshi: a speech-text foundation model for real-time dialogue](https://arxiv.org/abs/2410.00037).
- [Supplied research context](../RESEARCH_CONTEXT.md): lifecycle foundations and the September report’s hybrid/diffusion/omni discussion. The user’s Tier 3 extension supplies this dedicated subject scope.

These are starting points, not claims that every implementation supports the same features. Check the selected model, runtime and version before making deployment or performance claims.
