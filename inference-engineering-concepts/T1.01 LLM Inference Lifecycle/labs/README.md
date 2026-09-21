# T1.01 — Runpod inference lifecycle labs

A 36-experiment route from request preparation to state release, with source tracing, controlled load, diagnosis and an independent capstone. **The pack is a learning specification: no GPU lab is marked completed or validated.**

Start with [L00](lab-00-environment/README.md), then the Intro dependencies. Before renting anything read [environment, cost and measurement rules](environment.md). Your existing Qwen3-4B/L40S notes remain historical evidence; this pack does not rewrite them.

The full catalog follows the supplied laboratory manual's L00–L35 IDs. Intro/Medium are the chosen core; advanced/distributed branches stay visible and deferred. The local workspace includes ignored Python infrastructure, starter checks and source metadata; these Markdown guides alone are not an installable public software release. Full reference solutions and personal evaluations remain private.

## Workflow

Predict → build → measure → explain → falsify → intervene → retest under a changed workload. Build before comparing with reference solutions. Each lab separates a small offline invariant check from its actual system evidence gate. Six primary task/gate lists follow; later reading and deferred experiments are not hidden graduation conditions.

## Catalog

| ID | Experiment | Scope | Hardware |
|---|---|---|---|
| L00 | [Environment fingerprint](lab-00-environment/README.md) | intro / core | CPU then G1 |
| L01 | [Request to rendered prompt](lab-01-rendered-prompt/README.md) | intro / core | CPU |
| L02 | [Qwen tokenizer autopsy](lab-02-tokenizer-autopsy/README.md) | intro / core | CPU |
| L03 | [BPE from scratch](lab-03-toy-bpe/README.md) | intro / core | CPU |
| L04 | [Instrumented generation loop](lab-04-reference-generation/README.md) | intro / core | G1 |
| L05 | [Prove the K-minus-one relationship](lab-05-k-minus-one/README.md) | intro / core | G1 |
| L06 | [Sampling microscope](lab-06-sampling/README.md) | intro / core | CPU and G1 |
| L07 | [Token versus text versus SSE](lab-07-streaming/README.md) | intro / core | CPU then G1 |
| L08 | [Autopsy one vLLM request](lab-08-request-autopsy/README.md) | intro / core | G1 |
| L09 | [Validate the measurement harness](lab-09-harness-validation/README.md) | intro / core | CPU then G1 |
| L10 | [Prefill and input-length scaling](lab-10-prefill-scaling/README.md) | intro / core | G1 |
| L11 | [Decode output/history scaling](lab-11-decode-scaling/README.md) | intro / core | G1 |
| L12 | [KV bytes per token](lab-12-kv-math/README.md) | intro / core | CPU then G1 |
| L13 | [Paged KV and block rounding](lab-13-paged-kv/README.md) | medium / core | G1 |
| L14 | [Continuous batching movie](lab-14-continuous-batching/README.md) | medium / core | G1 |
| L15 | [Open-loop versus closed-loop load](lab-15-arrival-process/README.md) | medium / core | G1 |
| L16 | [Long-prompt interference](lab-16-chunked-prefill/README.md) | medium / core | G1 |
| L17 | [Prefix cache perturbation](lab-17-prefix-perturbation/README.md) | medium / core | G1 |
| L18 | [Structured decoding](lab-18-structured-output/README.md) | medium / core | G1 |
| L19 | [Backpressure and cancellation](lab-19-cancellation/README.md) | medium / core | G1 |
| L20 | [Three kinds of cold](lab-20-cold-starts/README.md) | medium / core | G1 |
| L21 | [Projection arithmetic intensity](lab-21-projection-roofline/README.md) | medium / core | CPU and G1 |
| L22 | [Which bytes dominate decode?](lab-22-decode-bytes/README.md) | expert / later | G1 profiling |
| L23 | [Quantization decomposition](lab-23-quantization/README.md) | expert / later | G1 exact supported precision |
| L24 | [CUDA graphs and launch overhead](lab-24-cuda-graphs/README.md) | expert / later | G1 profiler |
| L25 | [Profiling ladder](lab-25-profiling-ladder/README.md) | medium / core | G1; profiler permissions conditional |
| L26 | [Four-clock observation reconciliation](lab-26-clock-reconciliation/README.md) | medium / core | G1 |
| L27 | [Preemption and overload](lab-27-preemption/README.md) | expert / later | G1 |
| L28 | [Speculative decoding](lab-28-speculation/README.md) | expert / later | G1 supported method |
| L29 | [Tensor and data parallelism](lab-29-parallelism/README.md) | expert / later | G2 topology verified |
| L30 | [Prefill/decode disaggregation](lab-30-disaggregation/README.md) | expert / later | G2; source-verified connector |
| L31 | [Tiered and distributed KV](lab-31-tiered-kv/README.md) | expert / later | G1+CPU; G2/N2 extension |
| L32 | [KV-aware routing](lab-32-kv-routing/README.md) | expert / later | CPU simulator then G2 |
| L33 | [Little’s Law and goodput](lab-33-capacity-goodput/README.md) | medium / core | G1 |
| L34 | [Diagnosis gauntlet](lab-34-diagnosis-gauntlet/README.md) | medium / core | CPU evidence then G1 |
| L35 | [Turkish enterprise inference capstone](lab-35-capstone/README.md) | medium / core | G1; scaling branches deferred |

## Intro — A: study/do tasks

- [ ] **T1.01-L00-A** — [Environment fingerprint](lab-00-environment/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: CPU then G1; dependencies: none.
- [ ] **T1.01-L01-A** — [Request to rendered prompt](lab-01-rendered-prompt/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: CPU; dependencies: T1.01-L00.
- [ ] **T1.01-L02-A** — [Qwen tokenizer autopsy](lab-02-tokenizer-autopsy/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: CPU; dependencies: T1.01-L01.
- [ ] **T1.01-L03-A** — [BPE from scratch](lab-03-toy-bpe/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: CPU; dependencies: T1.01-L02.
- [ ] **T1.01-L04-A** — [Instrumented generation loop](lab-04-reference-generation/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1; dependencies: T1.01-L01, T1.01-L03.
- [ ] **T1.01-L05-A** — [Prove the K-minus-one relationship](lab-05-k-minus-one/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1; dependencies: T1.01-L04.
- [ ] **T1.01-L06-A** — [Sampling microscope](lab-06-sampling/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: CPU and G1; dependencies: T1.01-L04.
- [ ] **T1.01-L07-A** — [Token versus text versus SSE](lab-07-streaming/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: CPU then G1; dependencies: T1.01-L04.
- [ ] **T1.01-L08-A** — [Autopsy one vLLM request](lab-08-request-autopsy/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1; dependencies: T1.01-L00, T1.01-L04, T1.01-L07.
- [ ] **T1.01-L09-A** — [Validate the measurement harness](lab-09-harness-validation/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: CPU then G1; dependencies: T1.01-L07, T1.01-L08.
- [ ] **T1.01-L10-A** — [Prefill and input-length scaling](lab-10-prefill-scaling/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1; dependencies: T1.01-L09.
- [ ] **T1.01-L11-A** — [Decode output/history scaling](lab-11-decode-scaling/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1; dependencies: T1.01-L09, T1.01-L10.
- [ ] **T1.01-L12-A** — [KV bytes per token](lab-12-kv-math/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: CPU then G1; dependencies: T1.01-L04.

## Intro — B: completion evidence

- [ ] **T1.01-L00-B** — Supply the artifacts and checks specified by T1.01-L00-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L01-B** — Supply the artifacts and checks specified by T1.01-L01-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L02-B** — Supply the artifacts and checks specified by T1.01-L02-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L03-B** — Supply the artifacts and checks specified by T1.01-L03-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L04-B** — Supply the artifacts and checks specified by T1.01-L04-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L05-B** — Supply the artifacts and checks specified by T1.01-L05-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L06-B** — Supply the artifacts and checks specified by T1.01-L06-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L07-B** — Supply the artifacts and checks specified by T1.01-L07-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L08-B** — Supply the artifacts and checks specified by T1.01-L08-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L09-B** — Supply the artifacts and checks specified by T1.01-L09-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L10-B** — Supply the artifacts and checks specified by T1.01-L10-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L11-B** — Supply the artifacts and checks specified by T1.01-L11-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L12-B** — Supply the artifacts and checks specified by T1.01-L12-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.

## Medium — A: study/do tasks

- [ ] **T1.01-L13-A** — [Paged KV and block rounding](lab-13-paged-kv/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1; dependencies: T1.01-L12, T1.01-L08.
- [ ] **T1.01-L14-A** — [Continuous batching movie](lab-14-continuous-batching/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1; dependencies: T1.01-L08, T1.01-L09, T1.01-L13.
- [ ] **T1.01-L15-A** — [Open-loop versus closed-loop load](lab-15-arrival-process/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1; dependencies: T1.01-L09, T1.01-L14.
- [ ] **T1.01-L16-A** — [Long-prompt interference](lab-16-chunked-prefill/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1; dependencies: T1.01-L10, T1.01-L11, T1.01-L14, T1.01-L15.
- [ ] **T1.01-L17-A** — [Prefix cache perturbation](lab-17-prefix-perturbation/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1; dependencies: T1.01-L13, T1.01-L16.
- [ ] **T1.01-L18-A** — [Structured decoding](lab-18-structured-output/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1; dependencies: T1.01-L06, T1.01-L07, T1.01-L09.
- [ ] **T1.01-L19-A** — [Backpressure and cancellation](lab-19-cancellation/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1; dependencies: T1.01-L07, T1.01-L08, T1.01-L14.
- [ ] **T1.01-L20-A** — [Three kinds of cold](lab-20-cold-starts/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1; dependencies: T1.01-L09, T1.01-L17.
- [ ] **T1.01-L21-A** — [Projection arithmetic intensity](lab-21-projection-roofline/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: CPU and G1; dependencies: T1.01-L12, T1.01-L11.
- [ ] **T1.01-L25-A** — [Profiling ladder](lab-25-profiling-ladder/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1; profiler permissions conditional; dependencies: T1.01-L08, T1.01-L09, T1.01-L21.
- [ ] **T1.01-L26-A** — [Four-clock observation reconciliation](lab-26-clock-reconciliation/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1; dependencies: T1.01-L07, T1.01-L08, T1.01-L25.
- [ ] **T1.01-L33-A** — [Little’s Law and goodput](lab-33-capacity-goodput/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1; dependencies: T1.01-L09, T1.01-L15, T1.01-L18.
- [ ] **T1.01-L34-A** — [Diagnosis gauntlet](lab-34-diagnosis-gauntlet/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: CPU evidence then G1; dependencies: T1.01-L16, T1.01-L17, T1.01-L19, T1.01-L20, T1.01-L25, T1.01-L26, T1.01-L33.
- [ ] **T1.01-L35-A** — [Turkish enterprise inference capstone](lab-35-capstone/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1; scaling branches deferred; dependencies: T1.01-L18, T1.01-L19, T1.01-L20, T1.01-L26, T1.01-L33, T1.01-L34.

## Medium — B: completion evidence

- [ ] **T1.01-L13-B** — Supply the artifacts and checks specified by T1.01-L13-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L14-B** — Supply the artifacts and checks specified by T1.01-L14-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L15-B** — Supply the artifacts and checks specified by T1.01-L15-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L16-B** — Supply the artifacts and checks specified by T1.01-L16-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L17-B** — Supply the artifacts and checks specified by T1.01-L17-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L18-B** — Supply the artifacts and checks specified by T1.01-L18-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L19-B** — Supply the artifacts and checks specified by T1.01-L19-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L20-B** — Supply the artifacts and checks specified by T1.01-L20-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L21-B** — Supply the artifacts and checks specified by T1.01-L21-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L25-B** — Supply the artifacts and checks specified by T1.01-L25-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L26-B** — Supply the artifacts and checks specified by T1.01-L26-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L33-B** — Supply the artifacts and checks specified by T1.01-L33-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L34-B** — Supply the artifacts and checks specified by T1.01-L34-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L35-B** — Supply the artifacts and checks specified by T1.01-L35-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.

## Expert — A: study/do tasks (deferred)

- [ ] **T1.01-L22-A** — [Which bytes dominate decode?](lab-22-decode-bytes/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1 profiling; dependencies: T1.01-L11, T1.01-L21, T1.01-L25.
- [ ] **T1.01-L23-A** — [Quantization decomposition](lab-23-quantization/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1 exact supported precision; dependencies: T1.01-L12, T1.01-L18, T1.01-L21.
- [ ] **T1.01-L24-A** — [CUDA graphs and launch overhead](lab-24-cuda-graphs/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1 profiler; dependencies: T1.01-L20, T1.01-L21, T1.01-L25.
- [ ] **T1.01-L27-A** — [Preemption and overload](lab-27-preemption/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1; dependencies: T1.01-L13, T1.01-L15, T1.01-L26.
- [ ] **T1.01-L28-A** — [Speculative decoding](lab-28-speculation/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1 supported method; dependencies: T1.01-L05, T1.01-L06, T1.01-L11.
- [ ] **T1.01-L29-A** — [Tensor and data parallelism](lab-29-parallelism/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G2 topology verified; dependencies: T1.01-L12, T1.01-L15, T1.01-L33.
- [ ] **T1.01-L30-A** — [Prefill/decode disaggregation](lab-30-disaggregation/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G2; source-verified connector; dependencies: T1.01-L16, T1.01-L12, T1.01-L33.
- [ ] **T1.01-L31-A** — [Tiered and distributed KV](lab-31-tiered-kv/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: G1+CPU; G2/N2 extension; dependencies: T1.01-L17, T1.01-L12.
- [ ] **T1.01-L32-A** — [KV-aware routing](lab-32-kv-routing/README.md): perform its bounded build/run/analysis steps and retain the required artifacts. Hardware: CPU simulator then G2; dependencies: T1.01-L17, T1.01-L15, T1.01-L33.

## Expert — B: completion evidence (deferred)

- [ ] **T1.01-L22-B** — Supply the artifacts and checks specified by T1.01-L22-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L23-B** — Supply the artifacts and checks specified by T1.01-L23-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L24-B** — Supply the artifacts and checks specified by T1.01-L24-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L27-B** — Supply the artifacts and checks specified by T1.01-L27-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L28-B** — Supply the artifacts and checks specified by T1.01-L28-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L29-B** — Supply the artifacts and checks specified by T1.01-L29-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L30-B** — Supply the artifacts and checks specified by T1.01-L30-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L31-B** — Supply the artifacts and checks specified by T1.01-L31-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.
- [ ] **T1.01-L32-B** — Supply the artifacts and checks specified by T1.01-L32-A, satisfy its actual hardware requirement, then explain, apply and defend the result on a changed case. Offline checks alone do not meet a GPU gate.

## Coverage and boundaries

Source audit maps 36 Anki card IDs, the subject's substantive README sections and all 36 manual experiments. The following map is a planning contract, not measured completion. Detailed private provenance includes source hashes and exact card/export locations; answers are not copied here.

| Source concept | Labs | Evidence route |
|---|---|---|
| Anki: T1.01-001 · The request is a state machine, not one model call | L01, L04, L08, L19 | executable |
| Anki: T1.01-002 · The tokenizer–model interface | L01, L02, L03 | executable |
| Anki: T1.01-003 · BPE: learned compression, frozen encoding | L03 | executable |
| Anki: T1.01-004 · Vocabulary size, Turkish fertility and useful-task cost | L02, L33, L35 | executable |
| Anki: T1.01-005 · Context composition and output headroom | L01, L12 | executable |
| Anki: T1.01-006 · Workload shape and offered load | L09, L15, L35 | executable |
| Anki: T1.01-007 · State lifetimes and three kinds of cold | L13, L19, L20 | executable |
| Anki: T1.01-008 · Prefill, causality and the first output | L04, L05, L10 | executable |
| Anki: T1.01-009 · Decode, the off-by-one, and latency arithmetic | L05, L11 | executable |
| Anki: T1.01-010 · Why cache keys and values—and when reuse is valid | L04, L12, L17 | executable |
| Anki: T1.01-011 · Paged KV: allocation, sharing, eviction and cleanup | L13, L17, L19 | executable |
| Anki: T1.01-012 · KV capacity: derive the number and its limits | L12, L13 | executable |
| Anki: T1.01-013 · Logits, sampling policy and hidden workload changes | L06 | executable |
| Anki: T1.01-014 · Structured decoding is a state machine, not truth checking | L18 | executable |
| Anki: T1.01-015 · Streaming, stop detection and cancellation | L07, L19 | executable |
| Anki: T1.01-016 · Admission, continuous batching and token budgets | L08, L14, L15 | executable |
| Anki: T1.01-017 · Chunked prefill and phase interference | L16 | executable |
| Anki: T1.01-018 · Prefix caching: identity, avoided work and honest hit rates | L17 | executable |
| Anki: T1.01-019 · Arithmetic intensity and the roofline | L21 | executable |
| Anki: T1.01-020 · Which bytes are limiting decode? | L22 | deferred |
| Anki: T1.01-021 · Quantization: four possible wins, not one | L23 | deferred |
| Anki: T1.01-022 · Speculation: spend compute to reduce target rounds | L28 | deferred |
| Anki: T1.01-023 · Latency metrics with an explicit event ledger | L07, L09, L26 | executable |
| Anki: T1.01-024 · Averages, tails and populations that quietly disagree | L09, L15, L26, L33 | executable |
| Anki: T1.01-025 · Trace vLLM and recover trustworthy phase timings | L08, L25, L26 | executable |
| Anki: T1.01-026 · Profiling, asynchronous execution and CUDA graphs | L24, L25, L26 | executable |
| Anki: T1.01-027 · A reproducible lifecycle lab and benchmark campaign | L00, L09, L35 | executable |
| Anki: T1.01-028 · Diagnose symptoms without prescribing the GPU first | L34 | executable |
| Anki: T1.01-029 · Multimodal, reasoning and tools change the useful endpoint | L01, L04, L35 | analytical |
| Anki: T1.01-030 · Capacity, Little’s law and cost per accepted task | L33, L35 | executable |
| Anki: T1.01-031 · Parallelism changes the critical path | L29 | deferred |
| Anki: T1.01-032 · Prefill/decode disaggregation and transfer break-even | L30 | deferred |
| Anki: T1.01-033 · Distributed and tiered KV as a cache hierarchy | L31 | deferred |
| Anki: T1.01-034 · KV-aware routing: locality competes with queueing | L32 | deferred |
| Anki: T1.01-035 · Engine, kernel library, orchestrator—and emerging controls | L08, L22, L30, L32 | executable |
| Anki: T1.01-036 · Capstone: defend a private-inference decision end to end | L35 | executable |
| README: The inference cycle | L04, L05, L08 | executable |
| README: Request processing and context budgets | L01, L02, L12 | executable |
| README: Admission, scheduling and continuous batching | L13, L14, L15, L16, L19 | executable |
| README: Prefill, the first output and decode | L04, L05, L10, L11 | executable |
| README: KV state, memory and resource ownership | L12, L13, L17, L19 | executable |
| README: Selection, streaming and termination | L06, L07, L18, L19 | executable |
| README: Performance, measurement and diagnosis | L09, L10, L11, L21, L25, L26, L33, L34 | executable |
| README: Trace one request through vLLM | L08 | executable |
| README: Modern extensions: which lifecycle assumption changes? | L01, L04, L28, L30, L31, L32, L35 | analytical |
| README: Questions to explain, calculate and defend | L34, L35 | executable |
| README: Tokenizer foundations and BPE — existing detailed notes | L01, L02, L03 | executable |

Multimodal, hybrid/recurrent state, reasoning and tool pauses have analytical extension tasks in L01/L04/L35. Qwen3-4B text measurements do not demonstrate other architectures. Advanced branches link to their owning subjects through the main subject navigation; source/runtime verification and required hardware precede execution. No arbitrary lab count is imposed on future subjects.

## Evidence, pacing and move-on

Each lab has preparation, bounded execution and analysis tasks, normally15–45 active minutes each. GPU runtime is separate and capped by the environment budget. A robust negative result can meet an experimental gate; unsupported configurations, missing traces or unexecuted GPU work remain explicit gaps. Moving on with a gap is allowed without calling it completed.

Use one technical evidence packet per experiment; model-specific results link to the canonical model experiment rather than being copied. Keep detailed assistance/evaluator records in existing private assessment storage. Review the work independently, rebuild from a blank workspace, then revisit a changed case in a later session. No numeric readiness or hiring claim is implied.

[Back to subject](../README.md)
