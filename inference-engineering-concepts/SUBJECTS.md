# Inference engineering — subject index

All **85 subjects** are visible here: **T1.01–T1.45**, **T2.01–T2.33** and **T3.01–T3.07**. Existing folder names and subject IDs are retained. T1.01 contains expanded notes; the other pages are preparation outlines, with existing seed notes preserved where present.

[Coverage, priorities and workload contexts](COVERAGE.md) · [Research sources and evidence boundaries](RESEARCH_CONTEXT.md)

Start with the lifecycle and measurement spine, then select a workload path in the coverage guide. Numeric order is a catalog, not a prerequisite sequence.

Tier meanings: **T1** = core production inference engineering; **T2** = deeper kernel/runtime/optimization specialization and supporting systems; **T3** = emerging model architectures and non-standard inference systems. T3 IDs are assigned in the user-supplied order; existing T1/T2 IDs are unchanged. Tier membership is not a maturity rating.

| Subject | Topic | Material |
|---|---|---|
| [T1.01](T1.01%20LLM%20Inference%20Lifecycle/README.md) | LLM Inference Lifecycle | Expanded notes |
| [T1.02](T1.02%20Transformer%20Inference%20Architecture/README.md) | Transformer Inference Architecture | Preparation outline |
| [T1.03](T1.03%20KV%20Cache%20/README.md) | KV Cache | Preparation outline |
| [T1.04](T1.04%20Paged%20Attention%20-%20KV%20Memory%20Management%20/README.md) | Paged Attention - KV Memory Management | Preparation outline |
| [T1.05](T1.05%20Prefill%20vs%20Decode%20/README.md) | Prefill vs Decode | Preparation outline |
| [T1.06](T1.06%20Continous%20Batching%20/README.md) | Continuous Batching | Preparation outline |
| [T1.07](T1.07%20Chunked%20Prefill%20/README.md) | Chunked Prefill | Preparation outline |
| [T1.08](T1.08%20Serving%20metrics/README.md) | Serving metrics | Preparation outline |
| [T1.09](T1.09%20Workload%20Characterization/README.md) | Workload Characterization | Preparation outline |
| [T1.10](T1.10%20GPU%20Architecture%20Basics/README.md) | GPU Architecture Basics | Preparation outline |
| [T1.11](T1.11%20GPU%20memory%20hierarchy/README.md) | GPU memory hierarchy | Preparation outline |
| [T1.12](T1.12%20Roofline%20Reasoning%20/README.md) | Roofline Reasoning | Preparation outline |
| [T1.13](T1.13%20CUDA%20Fundamentals%20/README.md) | CUDA Fundamentals | Preparation outline |
| [T1.14](T1.14%20CUDA%20streams%20-%20events/README.md) | CUDA streams - events | Preparation outline |
| [T1.15](T1.15%20Tensor%20parallelism/README.md) | Tensor parallelism | Preparation outline |
| [T1.16](T1.16%20Pipeline%20paralellism/README.md) | Pipeline parallelism | Preparation outline |
| [T1.17](T1.17%20Data%20parallelism/README.md) | Data parallelism | Preparation outline |
| [T1.18](T1.18%20Expert%20parallelism/README.md) | Expert parallelism | Preparation outline |
| [T1.19](T1.19%20NCCL%20/README.md) | NCCL | Preparation outline |
| [T1.20](T1.20%20Collectives/README.md) | Collectives | Preparation outline |
| [T1.21](T1.21%20PCIe/README.md) | PCIe | Preparation outline |
| [T1.22](T1.22%20NVLink%20NVSwitch/README.md) | NVLink NVSwitch | Preparation outline |
| [T1.23](T1.23%20NUMA/README.md) | NUMA | Preparation outline |
| [T1.24](T1.24%20RDMA%20Fundamentals%20/README.md) | RDMA Fundamentals | Preparation outline |
| [T1.25](T1.25%20Memory%20Registration%20/README.md) | Memory Registration | Preparation outline |
| [T1.26](T1.26%20InfiniBand/README.md) | InfiniBand | Preparation outline |
| [T1.27](T1.27%20RoCE/README.md) | RoCE | Preparation outline |
| [T1.28](T1.28%20GPUDirect%20RDMA%20/README.md) | GPUDirect RDMA | Preparation outline |
| [T1.29](T1.29%20KV%20transfer/README.md) | KV transfer | Preparation outline |
| [T1.30](T1.30%20NIXL%20/README.md) | NIXL | Preparation outline |
| [T1.31](T1.31%20prefill%20/README.md) | Prefill/decode disaggregation | Preparation outline |
| [T1.32](T1.32%20Quantization/README.md) | Quantization | Preparation outline |
| [T1.33](T1.33%20GQA%20-%20MQA%20-%20MLA%20/README.md) | GQA - MQA - MLA | Preparation outline |
| [T1.34](T1.34%20Flash%20Attention%20-%20Optimised%20Attention/README.md) | Flash Attention - Optimised Attention | Preparation outline |
| [T1.35](T1.35%20MoE%20Inference/README.md) | MoE Inference | Preparation outline |
| [T1.36](T1.36%20Speculative%20Decoding%20/README.md) | Speculative Decoding | Preparation outline |
| [T1.37](T1.37%20vLLM%20Internals%20/README.md) | vLLM Internals | Preparation outline |
| [T1.38](T1.38%20SGLang%20Architecture/README.md) | SGLang Architecture | Preparation outline |
| [T1.39](T1.39%20Tensor-RT%20LLM%20/README.md) | Tensor-RT LLM | Preparation outline |
| [T1.40](T1.40%20NVIDIA%20Dynamo%20/README.md) | NVIDIA Dynamo | Preparation outline |
| [T1.41](T1.41%20Profiling/README.md) | Profiling | Preparation outline |
| [T1.42](T1.42%20GPU%20Observability/README.md) | GPU Observability | Preparation outline |
| [T1.43](T1.43%20Bottleneck%20Analysis/README.md) | Bottleneck Analysis | Preparation outline |
| [T1.44](T1.44%20Capacity%20Planning/README.md) | Capacity Planning | Preparation outline |
| [T1.45](T1.45%20Inference%20Economics/README.md) | Inference Economics | Preparation outline |
| [T2.01](T2.01%20Triton%20Kernel%20Programming/README.md) | Triton Kernel Programming | Preparation outline |
| [T2.02](T2.02%20CUTLASS%20-%20CuTe/README.md) | CUTLASS - CuTe | Preparation outline |
| [T2.03](T2.03%20torch.compile%20/README.md) | torch.compile / CUDA Graphs | Preparation outline |
| [T2.04](T2.04%20Kernel%20Fusion%20/README.md) | Kernel Fusion | Preparation outline |
| [T2.05](T2.05%20Advanced%20Quantization/README.md) | Advanced Quantization | Preparation outline |
| [T2.06](T2.06%20KVCache%20Quantization/README.md) | KVCache Quantization | Preparation outline |
| [T2.07](T2.07%20Prefix%20Caching/README.md) | Prefix Caching | Preparation outline |
| [T2.08](T2.08%20Distributed%20KV%20Cache%20/README.md) | Distributed KV Cache | Preparation outline |
| [T2.09](T2.09%20KV%20Offloading%20/README.md) | KV Offloading | Preparation outline |
| [T2.10](T2.10%20NVMe%20-%20storage%20tiers%20/README.md) | NVMe - storage tiers | Preparation outline |
| [T2.11](T2.11%20GPUDirect%20Storage%20/README.md) | GPUDirect Storage | Preparation outline |
| [T2.12](T2.12%20Advanced%20Routing/README.md) | Advanced Routing | Preparation outline |
| [T2.13](T2.13%20Admission%20Control/README.md) | Admission Control | Preparation outline |
| [T2.14](T2.14%20Backpressure/README.md) | Backpressure | Preparation outline |
| [T2.15](T2.15%20Autoscaling%20GPU%20Inference/README.md) | Autoscaling GPU Inference | Preparation outline |
| [T2.16](T2.16%20Kubernetes%20GPU%20Scheduling%20/README.md) | Kubernetes GPU Scheduling | Preparation outline |
| [T2.17](T2.17%20Multi-Node%20serving/README.md) | Multi-Node serving | Preparation outline |
| [T2.18](T2.18%20Fault%20Tolerance/README.md) | Fault Tolerance | Preparation outline |
| [T2.19](T2.19%20Model%20Loading/README.md) | Model Loading | Preparation outline |
| [T2.20](T2.20%20Multi-LoRA%20serving/README.md) | Multi-LoRA serving | Preparation outline |
| [T2.21](T2.21%20Long-context%20inference/README.md) | Long-context inference | Preparation outline |
| [T2.22](T2.22%20Context%20parallelism/README.md) | Context parallelism | Preparation outline |
| [T2.23](T2.23%20Advanced%20MoE/README.md) | Advanced MoE | Preparation outline |
| [T2.24](T2.24%20Network%20Congestion/README.md) | Network Congestion | Preparation outline |
| [T2.25](T2.25%20Topology-aware%20placement/README.md) | Topology-aware placement | Preparation outline |
| [T2.26](T2.26%20Post-training%20systems%20/README.md) | Post-training systems | Preparation outline |
| [T2.27](T2.27%20SFT/README.md) | SFT | Preparation outline |
| [T2.28](T2.28%20GRPO%20PPO%20RLHF/README.md) | GRPO PPO RLHF | Preparation outline |
| [T2.29](T2.29%20Rollout%20Serving/README.md) | Rollout Serving | Preparation outline |
| [T2.30](T2.30%20Weight%20Synchronization/README.md) | Weight Synchronization | Preparation outline |
| [T2.31](T2.31%20Colocated%20vs%20disaggregated%20RL/README.md) | Colocated vs disaggregated RL | Preparation outline |
| [T2.32](T2.32%20AMD%20ROCm%20inference/README.md) | AMD ROCm inference | Preparation outline |
| [T2.33](T2.33%20Benchmark%20Methodology%20/README.md) | Benchmark Methodology | Preparation outline |

## Tier 3 — architecture and serving extensions

| Subject | Topic | Material |
|---|---|---|
| [T3.01](T3.01%20Mamba%20and%20State%20Space%20Models/README.md) | Mamba and State Space Models | Preparation outline |
| [T3.02](T3.02%20Hybrid%20Mamba%20and%20Attention%20Architectures/README.md) | Hybrid Mamba and Attention Architectures | Preparation outline |
| [T3.03](T3.03%20Diffusion%20Language%20Models/README.md) | Diffusion Language Models | Preparation outline |
| [T3.04](T3.04%20Image%20Diffusion%20Inference/README.md) | Image Diffusion Inference | Preparation outline |
| [T3.05](T3.05%20Video%20Generation%20Inference/README.md) | Video Generation Inference | Preparation outline |
| [T3.06](T3.06%20Multimodal%20Inference/README.md) | Multimodal Inference | Preparation outline |
| [T3.07](T3.07%20Speech%20Inference/README.md) | Speech Inference | Preparation outline |
