# Research context and evidence boundaries

[Subject index](SUBJECTS.md) · [Coverage plan](COVERAGE.md)

The coverage revision uses all three user-supplied artifacts as standing context:

| Supplied artifact | Role in this revision |
|---|---|
| `T1.01 deepresearch .md` | Full lifecycle foundation: front end, workload shape, scheduling, prefill/decode, state, generation, performance, diagnosis and experiments. |
| `What Is New and Getting Traction in LLM Inference — September 2026.pdf` | 25-page emerging-concepts report. Technical themes on pp. 5–16; integrated architecture and adoption discussion on pp. 19–21. |
| `deep-research-report-10.md` | Prose version of the same September report; read alongside the PDF. It is not independent corroboration. |

The original research files remain local. This public synthesis excludes private career and readiness material. Source titles and section/page references establish provenance; assertions in a supplied report still require primary evidence. Original generated citation markers are not usable public references.

## Primary technical starting points

These sources support mechanism and implementation checks. A feature in one version, backend or model is not evidence of universal support. Recheck current compatibility before deployment.

- [Dynamo compatibility reference](https://docs.nvidia.com/dynamo/latest/reference/compatibility): backend-dependent serving features.
- [Dynamo KV-aware routing](https://docs.dynamo.nvidia.com/dynamo/dev/knowledge-base/concepts/system-architecture/kv-aware-routing): locality and projected routing cost.
- [SGLang HiCache design](https://docs.sglang.io/docs/advanced_features/hicache_design): GPU, host and storage tiers. Host memory is per instance; shared storage depends on configuration.
- [Mooncake](https://github.com/kvcache-ai/Mooncake): distributed KV and transfer infrastructure.
- [llm-d](https://llm-d.ai/): distributed inference orchestration.
- [vLLM expert parallel deployment](https://docs.vllm.ai/en/latest/serving/expert_parallel_deployment/): EP configuration and load balancing.
- [vLLM speculative decoding, v0.28.0](https://docs.vllm.ai/en/v0.28.0/features/speculative_decoding/): supported draft methods and workload caveats.
- [TensorRT-LLM quantization](https://github.com/NVIDIA/TensorRT-LLM/blob/main/docs/source/features/quantization.md): formats and compatibility distinctions.
- [vLLM hybrid KV cache manager](https://docs.vllm.ai/en/latest/design/hybrid_kv_cache_manager/): model-dependent state allocation.
- [vLLM-Omni, v0.22.0](https://docs.vllm.ai/projects/vllm-omni/en/v0.22.0/): heterogeneous multimodal serving stages.
- [CUDA programming guide](https://docs.nvidia.com/cuda/cuda-programming-guide/contents.html), [NCCL guide](https://docs.nvidia.com/deeplearning/nccl/user-guide/docs/overview.html), and [GPUDirect RDMA](https://docs.nvidia.com/cuda/gpudirect-rdma/): execution, communication and memory-lifetime contracts.
- [Triton tutorials](https://triton-lang.org/main/getting-started/tutorials/index.html), [CUTLASS overview](https://docs.nvidia.com/cutlass/latest/overview.html), and [PyTorch compiler](https://docs.pytorch.org/docs/2.14/torch.compiler.html): kernel and compilation branches.
- [verl engine workers](https://verl.readthedocs.io/en/latest/workers/engine_workers.html) and [delta weight synchronization](https://verl.readthedocs.io/en/latest/advance/delta_weight_sync.html): post-training execution and weight freshness.
- [ROCm documentation](https://rocm.docs.amd.com/en/latest/): alternate-platform compatibility and performance work.

## How to extend the material

Read the three supplied sources before revising scope; preserve stable subject IDs and existing authored notes. Map new concepts to both a subject and a workload context. Distinguish established mechanisms, version-specific implementations and research proposals. For quantitative claims, retain hardware, model, precision, concurrency, input/output distributions, cache state, quality constraints and measurement boundary. Never copy a paper's peak gain into a general expectation.

For each advanced addition include a causal mechanism, a boundary or counterexample, an observable consequence and a decision that changes under explicit constraints. Future full lessons should add direct section-level citations and checked calculations; the present outlines are topic inventories, not comprehensive evidence-backed golden answers.
