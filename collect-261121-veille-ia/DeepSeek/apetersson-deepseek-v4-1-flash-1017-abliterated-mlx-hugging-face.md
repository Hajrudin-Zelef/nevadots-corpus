---
id: collect-261121-veille-ia/deepseek/apetersson-deepseek-v4-1-flash-1017-abliterated-mlx-hugging-face
title: "apetersson/DeepSeek-V4.1-Flash-1017-Abliterated-MLX · Hugging Face"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-11-01"]
keywords: ["prefill", "native", "engram", "memory", "omlx", "model", "context", "cache"]
source: https://huggingface.co/apetersson/DeepSeek-V4.1-Flash-1017-Abliterated-MLX?local-app=mlx-lm
source_anchor: ""
source_lines: [1, 231]
section: "DeepSeek"
---
A **baked, mixed-precision oQ2.5e checkpoint for Apple Silicon**, served with **oMLX**.
The writer modifications are already in the weights: **no runtime steering adapter is needed**.

**Best configuration: oMLX 0.7.3 (the `v0.7.3` release; A8 prefill + CED decoder skip) on a 256 GiB Mac, with Engram tables on a fast local SSD.** This card documents that configuration end to end.

Thanks to u/lgrimani for the optimized oMLX prefill recipe (memory headroom + CED) that this configuration builds on.

| Item | This release | 
|---|---|
| Download / variant | oQ2.5e-mtp (payload at the repository root) — all 51 model shards plus the two native Engram files (layer1.safetensors, layer14.safetensors) are required | 
| Package size | Approximately **422.6 GB / 393.5 GiB** , not its runtime RAM requirement | 
| Resident model weights with Engram offloaded | Approximately **204.7 GiB** | 
| SSD-backed Engram tensors | **202.8 GB / 188.8 GiB** of full-quality native FP8/E8M0 tables; already included | 
| Tested machine class | **256 GiB unified-memory Apple Silicon** , local PCIe SSD | 
| Model layout | Native MLX / mixed affine and MXFP formats, not GGUF and not uniform 2.5-bit | 
| Context | **1M native** ; this recipe uses**256K as a speed/memory optimisation** — see below | 
| Runtime steering | Not required; do not apply the same writer patch again | 

The two native Engram tables are **202.8 GB / 188.8 GiB** in full-quality FP8/E8M0 form, with no table requantization. For background on why they live on SSD, see DeepSeek Engram memory on SSD.

**Repository layout:** the payload, `config.json`, `tokenizer*` and `SHA256SUMS` live at the repository root (the MLX standard layout), not in a subfolder.

| Layer | Recommended value | 
|---|---|
| Runtime | oMLX **0.7.3** (`v0.7.3` , commit**`9de79bc39a01c1f3fec1e849903aaf0dd5478d7a`** ); includes PR #4946; native custom kernels are required | 
| Engine pins | MLX **0.32.4** ; mlx-vlm**0.7.11** ; mlx-lm**0.32.3.dev8** (git`0d4d3d04` ); mlx-audio**0.6.3** ; transformers 5.14–5.20 | 
| Model settings | `deepseek_v41_ced_prefill_enabled: true` ;`a8_prefill_enabled: true` ;`a8_prefill_min_tokens: 128` ;`deepseek_v41_engram_ssd_offload: true` ; MTP on, depth 1 | 
| Context / output | `max_context_window: 262144` (speed-optimised;**1M native available** ); default`max_tokens: 2048` | 
| OS Metal cap | `iogpu.wired_limit_mb=237568` (**232 GiB** ) — required for 228 GiB MLX wiring | 
| MLX limits | **228 GiB** reference startup request; oMLX can adjust allocator/wired settings at runtime (see below) | 
| oMLX guard | `--memory-guard-gb 232` | 
| Residency | `MLX_METAL_RESIDENCY_REFRESH_INTERVAL_MS=1000` | 
| Scheduler | 3 active requests, chunked prefill, `prefill_priority: context` , decode fairness on | 
| Cache | 100 GiB paged SSD cache, no RAM hot cache | 
| Spotlight | Never-index the model and cache directories | 

DeepSeek-V4.1-Flash-1017 has a **1,048,576-token native window**, and this checkpoint inherits it. The **262,144-token** setting in this recipe is a serving optimisation: it reduces KV/cache memory and prefill time, and shorter contexts are generally more accurate. Any accuracy falloff at very long context is a general DeepSeek-V4.1-Flash characteristic, **not** an effect of this abliteration or the oQ2.5e quantization.

To use the full window, set `max_context_window` to `1048576` and size the cache/memory accordingly. Expect higher memory use and slower prefill. The 256K recommendation is what this card qualifies for speed and stability.

Apple Silicon uses unified memory, not a separate VRAM pool. Values below use **GiB = 2³⁰ bytes**, **MiB = 2²⁰ bytes**, and a **256 GiB** host; 256K / 1M mean 262,144 / 1,048,576 tokens. Tensor sizes are summed from 51 model shards plus the two native Engram files; they are storage payloads, not a prediction of exact process RSS or peak Metal allocation.

| Component / limit | Calculation | Size | 
|---|---|---|
| Full checkpoint tensor payload | Before SSD offload | 393.56 GiB | 
| SSD-backed Engram embedding tables | Native FP8/E8M0 tables (202,758,032,400 bytes) | 188.80 GiB | 
| Non-offloaded tensor payload | 219,828,499,272 bytes after SSD offload | **204.73 GiB** | 
| ↳ Vision tower + image projector | Included above: 0.767 + 0.137 GiB | 0.90 GiB | 
| ↳ MTP / DSpark tensors | Included above; entire retained draft payload | 7.49 GiB | 
| Growing packed-KV estimate, one request | 890 bytes/token × 256K / 1M | **222.50 / 890.00 MiB** | 
| Tensor payload + growing KV only | At 256K / 1M | 204.95 / 205.60 GiB | 
| Example MLX wired request | `mx.set_wired_limit(228 * 2**30)` ; not a permanent allocation cap | 228 GiB | 
| OS GPU wired cap / oMLX guard target | `237568 / 1024` ; guard configured separately | 232 GiB each | 
| Nominal memory outside those budgets | `256 − 228` /`256 − 232` | 28 / 24 GiB | 

The **890 bytes/token** figure is oMLX's logical growing packed-KV/index estimate, **not 890 bf16 elements**: `(512/2 + 512/16 + 128/2 + 128/32) × (1/2 + 1/2 + 1/2 + 1) = 890`. Do not multiply it by two. It is **not total cache or runtime memory**: decode growth can retain spare buffers with capacity slack; rolling windows, compressor/Engram state, MTP state, CED replay, allocator caches and prefill/image-processing workspaces add memory. Each active request adds its own state. Consequently the subtotals do not prove a safe 1M peak or a concurrency guarantee.

**What the limits mean:** `mx.set_wired_limit` sets the budget MLX can keep resident/non-pageable; it does not allocate that amount, cap all process memory, or guarantee every allocation stays wired if usage exceeds it. MLX rejects wiring above the device's reported system limit (about **222.7 GiB** by default on this host). Apply `sudo sysctl iogpu.wired_limit_mb=237568` first, then check `mx.device_info()["max_recommended_working_set_size"]` before requesting 228 GiB. `mx.set_memory_limit(228 * 2**30)` is an allocator **guideline**, not a hard RSS cap. Both MLX settings are process-local and must be applied inside the serving process; setting them in a separate Python command does not configure a later `omlx serve` process.

The oMLX guard is a separate admission/memory-pressure mechanism. In the pinned build, when the sysctl is explicitly raised, oMLX can mirror its guard ceiling into the MLX wired setting at startup (232 GiB here), overriding an earlier 228 GiB request. It also adjusts allocator backpressure during prefill. A startup API call therefore does not establish a permanently fixed 228 GiB limit. **232 − 228 = 4 GiB is only a difference between configured thresholds, not a reserved transient pool.** Likewise 28/24 GiB above is nominal arithmetic, not measured free memory: macOS, other applications, CPU buffers and file-cache activity also consume RAM.

**A8 / CED / vision / MTP:** A8 uses the existing quantized weights with request-time INT8 activation/workspace buffers; it does not require a second full INT8 weight copy. CED reduces decoder prefill work to the tail, but still needs replay and other runtime state. Neither has a fixed zero-overhead memory guarantee. The vision and MTP figures are included in the payload subtotal, not extra additions; runtime loading and extra state depend on the selected serving path. Keep headroom and use measured peak allocation under the intended context, images and concurrency rather than treating payload + KV as the full budget.

- **CED prefill.** DeepSeek V4.1 is a causal encoder–decoder model with a 128-token decoder window. oMLX can forward only the decoder tail during prefill and project decoder global KV from the encoder-final hidden state. This is the`deepseek_v41_ced_prefill_enabled` setting; the model layout supports it (40 layers, midpoint 20, decoder ratios all 1,`sliding_window=128` ).
- **A8 prefill.** On M5 tensor units, routed experts and dense projections can use INT8-activation prefill kernels. This is opt-in via`a8_prefill_enabled` and requires oMLX**0.7.3** or later (PR #4946 is included in`v0.7.3` ). On this checkpoint it is applied to 299 dense projections plus the routed A8 gather path.
- **Memory headroom.** MLX's default recommended working set on this 256 GiB host is about**222.7 GiB** , so`set_wired_limit(228 GiB)` is rejected unless the OS Metal cap is raised first. Set`iogpu.wired_limit_mb=237568` (232 GiB) before requesting 228 GiB via MLX. With the 232 GiB oMLX guard, the runtime may subsequently raise its wired request to 232 GiB. The nominal remainder is 28 GiB outside a 228 GiB wired budget (24 GiB relative to the 232 GiB guard), not a measured free-memory guarantee; see the budget table above.
- **Residency refresh.** The MLX Metal residency refresh keeps GPU allocations resident while the model is idle, avoiding cold first-token latency after quiet periods.
- **Engram on SSD.** The full package does not fit in 256 GiB of unified memory. Keep the Engram tables on the local SSD; do not attempt a full resident load.
- **Spotlight off.** Indexing a 422.6 GB model directory competes with weight reads. Add`.metadata_never_index` to the model and cache directories.

Use oMLX **v0.7.3** (or a later build containing PR #4946). Building from source with native kernels needs full Xcode, the Metal Toolchain component, CMake and Python 3.12:

```
sudo xcodebuild -downloadComponent MetalToolchain
sudo xcodebuild -runFirstLaunch
xcrun --kill-cache
xcrun metal --version   # must print a version, not "missing Metal Toolchain"
git clone https://github.com/jundot/omlx.git
cd omlx
git checkout v0.7.3
python3.12 -m venv .venv
. .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install "setuptools>=61" wheel "cmake>=3.27" "nanobind==3.1.0" "mlx==0.32.4"
OMLX_WITH_CUSTOM_KERNEL=1 python -m pip install --no-build-isolation .
python -c "from omlx.custom_kernels import native_kernel_status; print(native_kernel_status())"
```
All four kernel groups must report `available: True`. Align the engine dependencies with the runtime pins:

```
python -m pip install --no-deps --force-reinstall \
  "mlx-vlm==0.7.11" "mlx-audio==0.6.3" \
  "mlx-lm @ git+https://github.com/ml-explore/mlx-lm@0d4d3d04f8ab7ce4a51e7209e8615755a48ba94d"
```
If you use the packaged **v0.7.3** release, it contains PR #4946 and the native kernel artifacts.

Allow space for the **422.6 GB package**, plus your chosen SSD prefix-cache budget. Downloading only the small config or only the non-Engram shards is not sufficient.

```
MODEL_ROOT="$HOME/models/DeepSeek-V4.1-Flash-1017-Abliterated-MLX"
hf download apetersson/DeepSeek-V4.1-Flash-1017-Abliterated-MLX \
  --local-dir "$MODEL_ROOT"
# Optional full integrity check: reads all 422.6 GB once.
(cd "$MODEL_ROOT" && shasum -a 256 -c SHA256SUMS)
```
For reproducibility, add `--revision <commit-sha>` and record the chosen commit. SHA256SUMS binds the exact published files.

```
sudo sysctl iogpu.wired_limit_mb=237568
sysctl -n iogpu.wired_limit_mb          # 237568
```
Apply the cap before starting oMLX. If the OS resets it (reboot, driver reload), reapply it before the next oMLX start.

Disable Spotlight indexing for the model and cache trees:

```
CACHE_ROOT="$HOME/Library/Caches/omlx/ds41"
mkdir -p "$CACHE_ROOT"
touch "$MODEL_ROOT/.metadata_never_index"
touch "$CACHE_ROOT/.metadata_never_index"
```
Create the oMLX base layout. `BASE` is the directory passed as `--base-path`; the model is exposed under a short alias:

```
STATE_ROOT="$HOME/.omlx-ds41"
BASE="$STATE_ROOT/omlx"
mkdir -p "$BASE/models" "$CACHE_ROOT"
ln -sfn "$MODEL_ROOT" "$BASE/models/deepseek-v41"
```
Create `$BASE/model_settings.json`:

```
{
  "version": 1,
  "models": {
    "deepseek-v41": {
      "deepseek_v41_engram_ssd_offload": true,
      "deepseek_v41_ced_prefill_enabled": true,
      "a8_prefill_enabled": true,
      "a8_prefill_min_tokens": 128,
      "moe_expert_offload_enabled": false,
      "mtp_enabled": true,
      "mtp_draft_depth": 1,
      "vlm_mtp_enabled": false,
      "enable_thinking": true,
      "chat_template_kwargs": {"enable_thinking": true},
      "max_context_window": 262144,
      "max_tokens": 2048
    }
  }
}
```
Create `$BASE/settings.json`:

```
{
  "version": "1.0",
  "scheduler": {
    "max_concurrent_requests": 3,
    "chunked_prefill": true,
    "prefill_priority": "context",
    "decode_fairness": true
  }
}
```
Start the server with the matching flags and residency environment:

```
export MLX_METAL_RESIDENCY_REFRESH_INTERVAL_MS=1000
omlx serve \
  --model-dir "$BASE/models" \
  --base-path "$BASE" \
  --host 127.0.0.1 --port 8000 \
  --max-concurrent-requests 3 \
  --memory-guard-gb 232 \
  --paged-ssd-cache-dir "$CACHE_ROOT" \
  --paged-ssd-cache-max-size 100GB \
  --hot-cache-max-size 0 \
  --no-hf-cache --headless --log-level info
```
Keep the server on loopback. These flags configure the **232 GiB oMLX process guard**, not independently pinned 228 GiB MLX limits. oMLX can adjust wired residency at startup and allocator backpressure during prefill; check effective runtime settings rather than assuming a separate 4 GiB reserve.

**KL (first-token, 100 short prompts, thinking off):** native ablation lineage **0.047639 nats**; this native-Engram package ⇄ the previous affine4 release **0.030479 nats symmetric**; distance to native abliteration **0.525236** symmetric versus **0.491497** for affine4; distance to public oQ2.5e **0.083334** versus **0.081928**. The native-Engram distance to native abliteration did not become smaller in this test; the public-oQ2.5e difference is a difference, not a ranking. See KL_DIVERGENCE.md. These are distribution distances, not capability scores; the upstream Heretic 0.136 uses another protocol.

**Multi-token follow-up (oMLX 0.7.3, 2026-11-01):** the same 100 prompts, scored over up to 128 frozen native-abliterated answer tokens (11,583 positions) with identical teacher-forced prefixes, give mean KL to the native-abliterated reference **0.114011 nats** with affine4 Engram tables and **0.115734 nats** with the native Engrams; the paired difference **+0.001722** has a 95% interval **[−0.000123, +0.003850]** that includes zero. No clear winner, and no equivalence claim. These values use a different runtime and scoring path than the first-token numbers above: full-prefix prefill changed first-position logits noticeably relative to the prompt-only runs, so they are neither a dilution of the earlier heading-token effect nor evidence about incremental decoding. Details in KL_DIVERGENCE.md.

**Refusal:** no release-level refusal benchmark exists for this exact checkpoint. Strongest local V4.1 evidence: GLP-39 α=0.5 4/32 → 24/32 comply (refusal32) and 5/32 → 31/32 (cyber32); W9 native hybrids 6/6 substantive harmful completions but rejected as bake candidates for incompleteness. Upstream claims (not reproduced here): pqhaz α=2.5 98/100 → 0/100 and 97/100 → 1/100; dealignai FP8 100 % HarmBench-320.

Detailed: KL_DIVERGENCE.md · REFUSAL_EVALUATION.md.

Host: Apple M5 Ultra, 256 GiB unified memory, macOS 27, local PCIe SSD. oMLX v0.7.3 release, MLX 0.32.4, CED + A8 + MTP, 228 GiB initial MLX request / 232 GiB guard (runtime limits may be adjusted by oMLX), one serial client.

| Workload | Prefill | First token | Decode | 
|---|---|---|---|
| 35.1K tokens, natural Python source | **2077.1 tok/s** | 16.9 s | — | 
| 34.7K tokens, unique synthetic code | 889 tok/s | 39.0 s | — | 
| 69.2K tokens, unique synthetic code | 936 tok/s | 73.9 s | 68–80 tok/s | 
| Model load | — | 36 s | — | 

The synthetic workload is deliberately uncompressible (random identifiers), so it measures the worst case for Engram and prefix reuse. The natural-source value matches the published M5 Ultra reference for this checkpoint. These are host- and workload-specific observations, not guaranteed rates.

Quality smoke checks on this configuration: `17*23` returned 391, a unique 16-hex marker placed at 12.6K tokens was retrieved correctly, and a requested `add(a, b)` function was emitted correctly.

- **Context:** the model supports 1M tokens natively. This recipe sets 256K as a speed/memory optimisation; raise`max_context_window` only with the memory margin above and after your own long-context checks.
- **Cache:** the 100 GiB paged SSD prefix cache is reusable across turns. Do not run with`--no-cache` for a growing chat: every turn would reprocess the whole history. Keep the RAM hot cache at zero on a 256 GiB host. oMLX's observed block size is 8,192 tokens; not every common text prefix is reusable.
- **Concurrency:** three active oMLX slots share one model instance; chunked prefill and decode fairness keep a long prompt from starving an active decode. Three concurrent full-window requests have been exercised, but the memory margin is tight; do not expect 3× single-stream speed.
- **Memory pressure:** keep the guard enabled. If`Prefill above max_bytes` appears, reduce context or concurrency; do not disable the guard or overcommit the host.
- **Cold start:** the first request loads the weights. Separate load latency from decode speed when comparing runs.

- Base: deepseek-ai/DeepSeek-V4.1-Flash-1017, revision `1bae0e1088993b9b82de08f59de5be5a34e42170` .
- Writer patch: pqhaz/DeepSeek-V4.1-Flash-1017-heretic, revision `12d58ad0878b5008e80d720251adb969a0069412` . Credit to DeepSeek, pqhaz and the Heretic method; attribution is not endorsement.
- **38 writer tensors were replaced prior to final quantization.** This is a real baked checkpoint, not a base-model alias or runtime adapter.
- Fresh calibration processed **1,024 × 512-token samples** using an explicitly approximate calibration-only proxy. Final weights were read from the native baked master, not exported from the proxy.
- **9 of 46,080 expert/projection positions were unvisited** at the stock calibration maximum; source precision was retained for them. This is not complete calibration coverage.
- Quantization is mixed: affine 2/3-bit and 8-bit group-64 tensors, native MXFP4/MXFP8 group-32 tensors, and full-quality native FP8/E8M0 Engram tables with no additional table requantization. `oQ2.5e` is a recipe name, not a claim of uniform bits per parameter.
- The package accounts for **763,205,315,794 logical tensor parameters** , including retained vision/MTP and large embedding tables. This is not the number of dense parameters evaluated for each token.

See source provenance, build provenance and quantization report. Public metadata copies replace private calibration-machine paths with provenance labels; model weights, tokenizer and runtime geometry are unchanged. The public checksum manifest reflects those metadata-only changes.

The exact checkpoint has served real text requests on the recommended configuration (oMLX 0.7.3 release, CED + A8, MTP, Engram SSD offload). Arithmetic, short code and long-context marker retrieval passed; these are **not broad capability-preservation tests**. CED and A8 change numerics relative to a full-precision forward pass; broad quality parity has not been established.

Multi-token Engram fidelity was measured only as teacher-forced distribution distance under one matched full-prefix prefill path on a fixed 100-prompt corpus. It is not an answer-correctness or capability test, and the measured runtime-path sensitivity means those numbers do not transfer automatically to incremental decoding or other workloads.

The abliteration label describes the patch lineage. There is no comprehensive refusal-rate, safety or general-quality benchmark in this release, and no proof of bit-identical MTP-on/off output. It can generate inaccurate, harmful or unsafe content. Treat outputs accordingly. Retained vision/MTP tensors do not imply arbitrary client support or feature parity across engines.

The MIT notice is preserved in LICENSE. The native Engram tables are embedded in this package; no separate overlay is required. No smaller-memory tier, GGUF conversion, universal client RAM badge or full two-user 1M guarantee is claimed.

- Downloads last month
- 14,502

`# Install MLX LM uv tool install mlx-lm``# Generate some text mlx_lm.generate --model "apetersson/DeepSeek-V4.1-Flash-1017-Abliterated-MLX" --prompt "Once upon a time"`
