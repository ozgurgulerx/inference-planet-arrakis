# T1.13 — CUDA Fundamentals — Runpod labs

Three experiments cover the chapter's foundation, controlled system behavior and a changed-case boundary. These are learning specifications: **no GPU/network run is marked complete or validated by generating them**.

Read [environment, cost and measurement rules](environment.md) first. The default route is Intro then Medium; Expert remains visible and deferred until chosen. Use 15–45 minute design and analysis slices; implementation, dependency setup and GPU runtime are separate and may take longer. Some T1.01 labs already provide scaffolds; these new chapter labs are implementation-ready specifications rather than ready-to-run bundles. Reuse the shared Pod and artifacts across adjacent chapters.

## Existing T1.01 lineage

Semantic owner links: None. This chapter fills a gap not owned by an existing T1.01 experiment.

The original T1.01 IDs and files remain stable for manual/provenance continuity. This chapter owns deeper interpretation; it does not copy prior evidence or imply those experiments were run.

## Catalog

| ID | Experiment | Level / scope | Hardware |
|---|---|---|---|
| T1.13-L00 | [Threads, warps and correctness](lab-00-foundation/README.md) | intro / core | G1 |
| T1.13-L01 | [Coalescing and launch overhead](lab-01-controlled-sweep/README.md) | medium / core | G1 |
| T1.13-L02 | [Synchronization failure case](lab-02-boundary-case/README.md) | expert / later | G1; sanitizer availability conditional |

## Intro — A: study/do tasks

- [ ] **T1.13-L00-A** — [Threads, warps and correctness](lab-00-foundation/README.md): predict, build, run the bounded conditions and retain artifacts. Hardware: G1; dependencies: none.

## Intro — B: completion evidence

- [ ] **T1.13-L00-B** — Pass the guide's correctness and evidence checks, explain the mechanism, reject a competing explanation, and defend one changed case. An offline result does not satisfy a GPU/network gate.
## Medium — A: study/do tasks

- [ ] **T1.13-L01-A** — [Coalescing and launch overhead](lab-01-controlled-sweep/README.md): predict, build, run the bounded conditions and retain artifacts. Hardware: G1; dependencies: T1.13-L00.

## Medium — B: completion evidence

- [ ] **T1.13-L01-B** — Pass the guide's correctness and evidence checks, explain the mechanism, reject a competing explanation, and defend one changed case. An offline result does not satisfy a GPU/network gate.
## Expert — A: study/do tasks (deferred until chosen)

- [ ] **T1.13-L02-A** — [Synchronization failure case](lab-02-boundary-case/README.md): predict, build, run the bounded conditions and retain artifacts. Hardware: G1; sanitizer availability conditional; dependencies: T1.13-L01.

## Expert — B: completion evidence (deferred until chosen)

- [ ] **T1.13-L02-B** — Pass the guide's correctness and evidence checks, explain the mechanism, reject a competing explanation, and defend one changed case. An offline result does not satisfy a GPU/network gate.

## Evidence and move-on

Retain a prediction, environment manifest, exact commands, raw observations, correctness result, summary with uncertainty, falsifier, intervention and changed-case defense for each experiment. Negative results can pass when the test is controlled and the mechanism is explained. Missing capability stays blocked. Results and evaluator notes remain private unless explicitly selected for publication.

[Back to subject](../README.md) · [All T1 labs](../../T1-LABS.md)
