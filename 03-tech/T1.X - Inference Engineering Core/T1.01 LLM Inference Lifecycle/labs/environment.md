# T1.01 environment, cost and measurement contract

Status: local CPU helper checks are separate from GPU validation. **No Runpod experiment has been executed by creating this pack.** Source check: 2026-09-20.

## 1. Choose the resource for the question

Reference: one L40S48GB with Qwen/Qwen3-4B BF16. Budget alternatives: A40 48GB for compatible BF16 work; RTX4090 24GB for smaller active-state conditions. The current price references are $1.09/$0.49/$0.74 per GPU-hour respectively, not reserved offers. Recheck selected tier and availability at rental. A40 is not a replacement for native-FP8 hardware. Additional GPUs require a topology/P2P/fabric inventory and separately scoped cost.

The current planning cap is $2/GPU-hour and $30 for this subject including storage/retries. Initial allocation:20 billed L40S hours=$21.80 at the reference rate; the remaining$8.20 covers storage/contingency. This does not promise completion within20 hours. Stop new runs as the cap approaches; keep unfinished gates visible. The full manual's30 L40S hours would already cost$32.70 before storage. No provisioning or spending is authorized merely by generating this pack.

Start with60GB disk for core work; select80–120GB only for chosen profiler/additional-checkpoint work. Track whether storage continues billing after the Pod stops. Export evidence before stopping; delete only explicitly selected disposable resources. Never kill an unrelated service or delete persistent data to meet a budget.

## 2. Freeze and verify the stack

| Component | Required baseline |
|---|---|
| Model/tokenizer | `Qwen/Qwen3-4B` |
| Revision for both | `3101254bbe4169895668a0e7653c3fd1f313576e` |
| Runtime | vLLM `0.29.0`; full release commit and image digest recorded at deployment |
| Precision | BF16 weights; record effective KV dtype explicitly |
| Initial serving limit |8192 positions, including input and output allowance |
| Generation | thinking disabled in each chat request; record all sampling parameters |
| Access | SSH/VS Code and localhost tunnel; no unauthenticated public API |

Use a release container such as `vllm/vllm-openai:v0.29.0` after checking its registry digest and driver compatibility; record the exact digest, not just the mutable tag. Runpod launches the container. Ordinary nested Docker/Compose is not assumed. A plain pip version pin alone does not reproduce Torch/CUDA dependencies.

The pinned configuration gives36 layers,8 KV heads, head dimension128. BF16 raw KV is144KiB/position. `max_position_embeddings=40960` is a configuration fact, not a quality guarantee or the active endpoint limit. Increase the explicit limit only for selected long-context conditions; ensure input plus requested output fits. A8192-input prompt plus any output exceeds the initial8192 serving limit.

From the local lab root:

```bash
python3 -m _shared.lab_cli record-env --out results/environment.json
python3 -m _shared.lab_cli --help
```

The local inventory can report no GPU. On an authorized Pod, record GPU model/UUID/VRAM, host RAM/CPU, driver, installed runtime, existing listeners/model/processes, clocks/power settings where exposed, disk and profiler permissions. The CUDA compatibility shown by nvidia-smi is not the installed Torch CUDA runtime.

Before any model operation verify the exact local snapshot/revision. If the checkpoint must be downloaded, do that only as part of an explicitly requested GPU/setup session and record the download/revision; the helpers do not download it automatically. Inspect the local config/tokenizer:

```bash
python3 -m _shared.lab_cli inspect-model --config /absolute/pinned-snapshot/config.json
python3 -m _shared.lab_cli inspect-model --config /absolute/pinned-snapshot/config.json --tokenizer /absolute/pinned-snapshot --load-tokenizer
```

These path placeholders must be replaced with your actual local snapshot; optional tokenizer loading requires the container's compatible Transformers installation. A supplied revision label is not proof of file provenance; retain the snapshot origin and file hashes.

## 3. Start one bounded server when execution is requested

Check `vllm --version`, save `vllm serve --help` and inspect whether port8000 is occupied. Confirm `/v1/models` before submitting traffic; do not stop an unknown template-managed model. From the Pod, use the installed0.29 CLI with the approved pinned local model/tokenizer. The local `setup/serve_baseline.sh /absolute/pinned-snapshot` records a command and starts a foreground server only when explicitly invoked. It refuses an occupied port and does not download weights. Copy the local lab tooling to the Pod only during requested setup; do not upload private source exports, solutions or assessment files.

The explicit baseline is BF16, localhost:8000, max model length8192, max sequences16, iteration token budget2048, GPU memory utilization0.85, chunked prefill enabled and prefix reuse disabled. Change one setting for an experiment and keep its new command. If any flag is unsupported by the observed build, stop and reconcile versions; do not silently drop it.

On your Mac use an SSH tunnel with the Pod's actual user/host/port rather than exposing the inference port. Validate one short request before any load; reference client usage:

```bash
python3 -m _shared.lab_cli stream --base-url http://127.0.0.1:8000 --prompt 'Explain KV cache in one sentence.' --timeout-s 30 --jsonl results/one-request.jsonl
python3 -m _shared.lab_cli load --base-url http://127.0.0.1:8000 --count 8 --concurrency 1 --shape A --out-dir results/baseline-A
```

These commands send inference traffic to the supplied endpoint; do not run them against a paid/shared endpoint without the requested execution scope. Shape fixtures are approximate unless a pinned tokenizer verifies the rendered prompt. Consult `--help` for prompt-file and controlled-workload options. Keep exact-ID/verified-token experiments separate from templated natural text.

For CPU-only client inspection, run `python3 -m _shared.lab_cli serve-mock` in another terminal and use the loopback URL it prints. Synthetic fixture observations cannot establish model, GPU or SLO evidence.

## 4. Workload and measurement contract

| Shape | Requested ISL | Output target | Question |
|---|---:|---:|---|
| A |128|128|Short interactive baseline|
| B |2048|128|Medium prefill|
| C |6144|128|Long prefill|
| D |2048|512|Decode-heavy|
| E |2048|1|Prompt to first output|

`max_tokens` is a ceiling. Store actual usage, finish reason, errors and shape validity; stratify mismatched outputs instead of claiming the requested OSL occurred. Missing usage stays null. Response usage may count engine tokens unseen in text; generated text re-tokenization is only a separately labeled estimate. Thinking/template settings are part of the workload.

Keep separate client-send, first SSE event, first visible content, stream completion and engine token events. A stream fragment is not a token; do not publish inter-chunk gaps as ITL. Preserve raw records and metric definitions. Mean engine TPOT for K>1 uses(first-token→last-token)/(K-1); for K<=1 it is undefined. Mean request TPOT and token-gap-weighted TPOT differ; component percentiles cannot be summed into an E2E percentile.

All client intervals use one monotonic clock. Correlate engine/request/batch identities, but never subtract a client timestamp from a server timestamp or mix wall and monotonic timestamps. Prometheus is aggregate: isolated histogram deltas are attributable only with matching labels, no reset and exactly one observation; ordinary multi-request scrapes are not request traces.

vLLM0.29 per-request metrics use scheduling→first generated token for server TTFT and expose queue time separately. Streaming requires final usage inclusion; n>1 suppresses these metrics. Treat missing metrics as unavailable. Compare metric collection enabled/disabled because instrumentation can alter CPU cost. Profile short representative windows separately from benchmark measurements.

Open-loop offered arrivals are independent of completions; closed-loop clients self-throttle. Save planned/actual send time, lag, rejected/skipped work, submitted/completed rates and all failures. Use hard count/time/inflight limits. If safety bounds reject work, report it rather than silently measuring a lower offered rate. Verify generator capacity before blaming the server.

Use warmups separate from trials, interleave comparable A/B conditions, report N/population/variation and avoid unsupported p99 claims. Freeze quality/SLO criteria and preserve failures in success/goodput denominators. No speedup is required if the falsifying experiment and explanation are sound.

## 5. Profiling and session closure

Start with nvidia-smi and runtime state, then PyTorch or Nsight Systems timelines; use DCGM and Nsight Compute only with verified access. Missing hardware-counter permission is a blocked counter conclusion, not permission to claim a bottleneck from utilization. CUDA dispatch is asynchronous; use appropriate events/synchronization and account for overlap rather than summing kernels blindly.

After a bounded session: save manifests, exact commands, raw results and uncertainty; export them; stop only the server/Pod you own and inspect remaining billed storage. Analyze locally. Add observed results to the appropriate canonical experiment, never prefill successful evidence.

## Primary sources

- [vLLM0.29 release](https://github.com/vllm-project/vllm/releases/tag/v0.29.0)
- [Pinned Qwen config](https://huggingface.co/Qwen/Qwen3-4B/blob/3101254bbe4169895668a0e7653c3fd1f313576e/config.json)
- [Versioned per-request metric semantics](https://github.com/vllm-project/vllm/blob/v0.29.0/docs/features/per_request_metrics.md)
- [Runpod Pod constraints](https://docs.runpod.io/pods/overview) and [current pricing](https://www.runpod.io/pricing)

[Lab index](README.md)
