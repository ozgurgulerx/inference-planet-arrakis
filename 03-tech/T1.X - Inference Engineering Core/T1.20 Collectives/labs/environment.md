# T1.20 — Collectives — Runpod environment contract

Status: specification only. Generating this pack did not provision a Pod or validate a GPU result. Source check: 2026-09-21.

## Resource gate

Highest requested tier: **G2: one Runpod multi-GPU Pod with two compatible GPUs and verified topology/P2P**.

- CPU steps establish arithmetic, harness correctness or a blocked-capability record; they do not satisfy a GPU/network gate.
- G1 is one GPU. For the shared Qwen baseline, prefer a 24–48 GB GPU and use the pinned T1.01 environment where compatible.
- G2 is two compatible GPUs in one Pod. Record `nvidia-smi topo -m`, peer-access support and the exact GPU pair before running.
- N2 is two authorized endpoints with provider-exposed networking. Inventory NICs, link layer, RDMA devices, routes, permissions and counters before choosing a transport. Ordinary Internet-connected Pods do not prove InfiniBand, RoCE, GPUDirect RDMA or NVSwitch.

Use the smallest tier that can answer the question. The planning ceiling is **$2/GPU-hour and $30 for this subject**, including storage and reruns; it is a stop/replan limit, not spending authority. Check the current Runpod quote at rental time. Reuse one bounded Pod session across related chapters rather than renting separately for every page.

## Freeze before measurement

Record the cited source release/commit and exact relevant documentation section, image/release digest, package lock, model and tokenizer revisions, driver, CUDA reported by the framework, GPU UUID/VRAM, host CPU/RAM, topology, clocks/power state, profiler permissions, disk and all launch flags. Use the pinned `Qwen/Qwen3-4B` + vLLM `0.29.0` T1.01 baseline only where the feature supports it. For SGLang, TensorRT-LLM, Dynamo, NIXL or networking tools, choose one official compatible release and record the compatibility decision; do not combine unverified latest components.

Before a paid run:

```bash
mkdir -p results/environment
nvidia-smi --query-gpu=index,name,uuid,memory.total,driver_version --format=csv > results/environment/gpus.csv
nvidia-smi topo -m > results/environment/topology.txt
python3 -m pip freeze > results/environment/python-freeze.txt
```

If a command or device is unavailable, preserve the error and mark the hardware gate blocked. Do not install privileged drivers, change shared fabric controls, expose an unauthenticated inference endpoint or stop an unknown process.

## Measurement contract

State a prediction and two competing mechanisms before running. Change one independent variable, retain actual input/output counts and all failures, separate warmup from trials, and use monotonic durations within one clock domain. A profiler run has an unprofiled control. A distributed run has rank-local logs, hard timeouts and owned-process cleanup. A speed result must retain correctness/quality and the declared SLO population.

Store raw logs and private billing evidence outside the public guides. Complete a gate only after explaining, applying and defending observed evidence on a changed case.

## Primary entry points

- [Primary release or dated documentation baseline](https://github.com/NVIDIA/nccl/releases/tag/v2.32.3-1)
- [Runpod Pods](https://docs.runpod.io/pods/overview)
- [Shared T1 lab route](../../../T1-LABS.md)
