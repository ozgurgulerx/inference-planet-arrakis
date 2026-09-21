---
id: day-001
title: "Inference: Qwen3.8-27B"
day: 1
date: "2025-09-12"
items: []
---


Ok now we covered what to study how...
Now the focus is on qwen-3.8-27b model and its iference.

BF16 (16-bit floating point) each parameter uses 2 bytes.
Unlike 4-bit, 8-bit quantisations (INT4, INT8, FP4, FP8- choice between FP or INT quantization variants depends on the hw / runtime support) these are raw model weights.
27b x 2 bytes is close to 54GB storage for weights.

Ok, now lets say we have a 54GB GPU to store the BF16 weights of qwen-3.8-27b...Can it hold the model successfuly to serve a chatbot? No - because the gpu will also need to store the request state and additional temporary working memory to store intermediate results and buffers used during computation.

For qwen the request state has two parts...
- KV cache (stores key/value vectors from the previous tokens, not attention weights. Attention weights are calculated using the current query and those keys) in its full attention layers.
- Recurrent state in its DeltaNet layers.

So for qwen3.8-27b our memory budget becomes,
- weights
- request state = KV cache in full attention layers + Recurrent state (fixed size state) in DeltaNet layers
- working memory engine overhead - depends on batching and runtime allocation

Ok, now what happens if we serve multiple conversations...
The request state memory will increase as sim conversations increases.

### What is Gated DeltaNet?

Recurrent linear attention, using a matrix memory updated, using a matrix memory updated through gating and a delta rule.
It is close to mamba-style approaches, but has a different update mechanism. (Gated DeltaNet paper).


[GATED DELTA NETWORKS: IMPROVING MAMBA2 WITH DELTA RULE](https://arxiv.org/pdf/2412.06464)
Linear Transformers

Ok, so qwen3.8-27b has a "state space", mamba like component in it.
The DeltaNet paper talks about linear attention, not quiet the state space model but something similar. e.g. currently I dont know what may be special in terms of inference for serving SSM's.
