# T4.03 — Pretraining Data Pipelines

Status: **Preparation outline — deferred**. This page maps future study; it does not record completed study, experiments or mastery.

[All subjects](../../SUBJECTS.md) · [Coverage and priorities](../../COVERAGE.md) · [Research context](../../RESEARCH_CONTEXT.md)

## Scope

- Acquisition and provenance of raw corpora, document parsing, language and quality filtering, safety and policy constraints.
- Exact and near-duplicate removal, benchmark-contamination controls, dataset versioning and auditable lineage.
- Domain mixing, sampling weights, packing and shuffling from curated documents to deterministic training batches.

## Training/inference boundary

This subject governs the examples and distribution that shape the weights. Inference observes consequences of those choices but cannot repair missing provenance, contamination or duplicated training data at serving time.

## Related existing subjects

[T1.09 — Workload Characterization](../../T1.X%20-%20Inference%20Engineering%20Core/T1.09%20Workload%20Characterization/README.md), [T1.44 — Capacity Planning](../../T1.X%20-%20Inference%20Engineering%20Core/T1.44%20Capacity%20Planning/README.md), and [T2.33 — Benchmark Methodology](../../T2.X%20-%20Inference%20Engineering%20Extended/T2.33%20Benchmark%20Methodology%20/README.md).

## Source starting points

- [Stanford CS336 — Language Modeling from Scratch](https://cs336.stanford.edu/).
- [Common Crawl](https://commoncrawl.org/).
- [DataComp-LM: In search of the next generation of language model pretraining datasets](https://arxiv.org/abs/2406.11794).

These are starting points; dataset legality, licenses, policies and provenance require review for the intended use.
