# T4.02 — Tokenization for Pretraining

Status: **Preparation outline — deferred**. This page maps future study; it does not record completed study, experiments or mastery.

[All subjects](../../SUBJECTS.md) · [Coverage and priorities](../../COVERAGE.md) · [Research context](../../RESEARCH_CONTEXT.md)

## Scope

- Byte, character, word and subword representations; BPE, unigram tokenization and byte fallback.
- Vocabulary construction, normalization, reserved tokens, multilingual coverage and deterministic encode/decode behavior.
- How fertility, sequence length and domain coverage affect training compute, context use and downstream behavior.

## Training/inference boundary

The tokenizer is fitted or selected before training and becomes part of the model contract. Inference must reproduce the same vocabulary and normalization exactly, but tokenizer serving speed is distinct from the statistical effects of vocabulary design during pretraining.

## Related existing subjects

[T1.02 — Transformer Inference Architecture](../../T1.X%20-%20Inference%20Engineering%20Core/T1.02%20Transformer%20Inference%20Architecture/README.md), [T1.05 — Prefill vs Decode](../../T1.X%20-%20Inference%20Engineering%20Core/T1.05%20Prefill%20vs%20Decode%20/README.md), and [T1.09 — Workload Characterization](../../T1.X%20-%20Inference%20Engineering%20Core/T1.09%20Workload%20Characterization/README.md).

## Source starting points

- [Stanford CS336 — Language Modeling from Scratch](https://cs336.stanford.edu/).
- [SentencePiece: A simple and language independent subword tokenizer and detokenizer for Neural Text Processing](https://arxiv.org/abs/1808.06226).
- [Neural Machine Translation of Rare Words with Subword Units](https://arxiv.org/abs/1508.07909).

These are starting points; evaluate the selected tokenizer against the actual languages and domains rather than assuming universal behavior.
