# Step 4 — Inference Serving Frameworks: vLLM + SGLang (February 1 → September 22, 2026)
## Research report (English) — collected September 22, 2026

**Project:** RAG data-collection, Step 4 (Infra inference / training), Track A
**Coverage window:** February 1, 2026 → September 22, 2026
**Engines:** vLLM (vllm-project) and SGLang (sgl-project)

This file combines two full research reports produced as independent passes:

- **Part 1 — vLLM** (`_draft_etape4_vllm.md`): 609 lines. Research date 2026-09-22.
- **Part 2 — SGLang** (`_draft_etape4_sglang.md`): 394 lines. Research date 2026-09-22.
- **Part 3 — Synthesis** (head-to-head comparison, 2026 combined timeline, shared ecosystem,
  unified uncertainty log) — added below, after Part 2.

### Provenance legend (used throughout both parts)
- **[official]** — the project itself: GitHub repo, GitHub releases API, official docs, project blog.
- **[vendor-reported]** — hardware/software vendor claims (NVIDIA, AMD, Intel, Red Hat, cloud providers).
- **[independent]** — reputable third-party measurements (SemiAnalysis InferenceX, dstack, community benchmarks).
- **[secondary]** — press, blogs, third-party summaries, aggregator repos.
- **[unverified]** — could not be confirmed from an authoritative source; treat as uncertain.

Prices, versions, and star counts are dated snapshots of 2026-09-22; re-verify before operational use.

---

# PART 1 — vLLM

# vLLM Inference-Serving Framework — Research Report
**Research date:** 2026-09-22
**Project:** RAG data collection, Step 4 (AI infra: inference/training)
**Language:** English

> Provenance tags used throughout: `[official]` (vLLM project itself: GitHub, docs, blog),
> `[vendor-reported]` (NVIDIA/AMD/Intel/Red Hat/vendor sources),
> `[independent]` (independent benchmarks/studies),
> `[secondary]` (press, blogs, third-party summaries),
> `[unverified]` (could not be confirmed from an authoritative source).
> Uncertainties are marked explicitly with ⚠️.

---

## 1. Identity & origins

- **What:** vLLM is an open-source, high-throughput LLM inference and serving engine built around
  **PagedAttention** (KV cache organized into fixed-size physical blocks addressed via per-sequence
  block tables, OS-virtual-memory-style) and **continuous (iteration-level) batching**. [official][secondary]
- **Origin:** Created at UC Berkeley's **Sky Computing Lab** (SOSP 2023 paper: "Efficient Memory Management
  for Large Language Model Serving with PagedAttention"); associated with the **LMSYS** team (Chatbot Arena).
  [official][secondary]
- **License:** Apache-2.0, no CLA requirement, no relicense history to date. [secondary]
- **GitHub:** https://github.com/vllm-project/vllm [official]
  - **Stars: 92,444 — Forks: 22,525 — Open issues: 8,335** (GitHub API, read 2026-09-22 19:4x UTC;
    pushed_at 2026-09-22T19:39:14Z; repo created 2023-02-09). [official]
- ⚠️ Ecosystem-scale claim: listed as the backend for many inference-as-a-service platforms and the de-facto
  open-source standard for LLM serving (widely repeated; directionally consistent with star/fork data, but a
  "de-facto standard" is a characterization, not a measurable fact). [secondary]

---

## 2. Release history 2026 (all dates from GitHub Releases API, 2026-09-22)

Release cadence in 2026 is roughly bi-weekly majors plus occasional patches. Full official list
(version — published date) [official]:

| Version | Published | Notes |
|---|---|---|
| **v0.30.0** | 2026-09-22 | Latest as of research date |
| v0.29.0 | 2026-09-09 | MRV2 default for all models |
| v0.28.0 | 2026-08-26 | Kimi-K3 perf push; DeepSeek V4 sparse MLA |
| v0.27.1 | 2026-08-11 | patch |
| v0.27.0 | 2026-08-10 | |
| v0.26.0 | 2026-07-27 | Inkling family; DeepSeek-V4 perf push |
| v0.25.1 | 2026-07-14 | patch |
| v0.25.0 | 2026-07-11 | |
| v0.24.0 | 2026-06-29 | |
| v0.23.0 | 2026-06-15 | |
| v0.22.1 | 2026-06-05 | |
| v0.22.0 | 2026-05-29 | |
| v0.21.0 | 2026-05-15 | |
| v0.20.2 | 2026-05-10 | patch |
| v0.20.1 | 2026-05-04 | patch |
| v0.20.0 | 2026-04-27 | |
| v0.19.1 | 2026-04-18 | patch |
| v0.19.0 | 2026-04-03 | |
| v0.18.1 | 2026-03-31 | patch |
| v0.18.0 | 2026-03-20 | |
| v0.17.1 | 2026-03-11 | patch |
| v0.17.0 | 2026-03-07 | |
| v0.16.0 | 2026-02-25 | |
| v0.15.1 | 2026-02-04 | patch |
| v0.15.0 | 2026-01-29 | |
| v0.14.1 | 2026-01-24 | patch |
| v0.14.0 | 2026-01-20 | First 2026 release |

Source: GitHub Releases API `https://api.github.com/repos/vllm-project/vllm/releases` (read 2026-09-22).
Release-notes pages: https://github.com/vllm-project/vllm/releases/tag/v0.30.0 etc. [official]

### 2.1 v0.30.0 — 2026-09-22 (762 commits, 315 contributors, 104 new) [official]

Headline features:
- **New models:** DeepSeek-V4.1-Flash (whole KV stored in MXFP8 via FlashMLA V4.1 on SM100; Mega-mHC from a
  DeepGEMM fork; async Engram prefetch with Engram DP sharding), DeepSeek-V4-Flash-Vision-Exp (+ROCm, +LoRA),
  GLM-5.3-Flash (with EPLB), K2-Horizon (reasoning + tool parsers), Cohere Compass, Bailing V3 VL,
  Nanbeige4.2 via the Transformers backend, DeepSeek-V4 CPU backend (AVX512/AMX sparse MLA, indexer, mHC,
  compressor kernels).
- **Fast Start:** persistent per-GPU weight-cache daemon holding post-quantized, TP-sharded weights in GPU
  memory; engine restarts map them over CUDA IPC with `--load-format ipc_cache` instead of reloading from
  disk; now covers FP4 checkpoints and multi-node TP.
- **Watermarking:** Gumbel-max watermarked generation and detection with a keyed PRF, per-request opt-out,
  example detection endpoint; dual-key Gumbel-max makes it compatible with speculative decoding; Rust
  frontend forwards per-request controls.
- **HiSparse:** host-resident tier for sparse-MLA decode — spills KV pages to pinned host memory under GPU
  pressure, serves top-k misses from a per-request GPU hot buffer (`HiSparseConnector`), with Prometheus
  counters and a host cache shared across TP ranks.
- **Model Runner V2:** dual-batch overlap (DBO) in eager mode and with FULL CUDA graphs for microbatched
  steps; MTP and EAGLE3/DFlash/DSpark speculative decoding under pipeline parallelism; adaptive verification
  for every draft-model speculator via online acceptance estimator; GC frozen during graph capture —
  capture 12s→2s, engine init 28.9s→8.2s on H200 (vendor-reported PR numbers).
- **Qwen3.8-Flash-Next performance:** separate prefill/decode QSA indexer kernels, fused PLE kernels, FP8
  indexer cache, padded-index skipping in sparse GQA, fused PLE residual + QSA output gate, UVA PLE offload
  and Engram TP via `--engram-config`; torch.compile removed from NVIDIA path so FP8 fits on a single GB300.
- **Kimi K3 performance:** native CUDA AttnRes default on SM100, KDA mixed-batch gather/scatter removed
  (5.2–7.7% E2E throughput), grouped FP8 MLA cache insertion (4–6× kernel speedup at small batch), DSV3
  low-latency GEMM on strided tensors (12–81% kernel speedup), overlapped TP8 KDA projections, FlashInfer
  KDA kernels, internal prefix checkpoints with partial prefix caching + speculative decoding, symmetric
  DCP disaggregation for hybrid Mamba models.
- **Large-scale serving:** PCP+DCP on sparse-MLA models; Elastic EP reusing CUDA graphs across
  reconfiguration; opt-in FlashInfer PCIe IPC all-reduce for NVLink-less boxes; DeepEP v2 async finalize
  overlapping shared experts with combine; Mooncake Store heterogeneous TP sharing; KVCR secondary-tier
  adapter; encoder-cache sharing over NIXL and Mooncake.
- **Quantization:** targeted online quantization via `quantization_config.targets`; W4A16 DSA with
  `nvfp4_fp8_ds_mla` KV cache; FlashInfer CuTeDSL NVFP4 W4A16 default over Marlin on SM100/103; NVFP4 in the
  torch linear backend; per-quantization linear backend overrides; AutoRound 2/3/5/6/7-bit on CUDA;
  DeepSelect top-k for the DSA sparse indexer.
- ⚠️ **Breaking changes:** scale-out endpoints opt-in on plain `vllm serve` via `--enable-scale-out`
  (replaces `VLLM_ENABLE_SCALE_OUT_ENDPOINTS`); GPTQ activation ordering (`g_idx`) removed; items
  deprecated for 0.29 removed (incl. `VLLM_PREFIX_CACHE_RETENTION_INTERVAL`, `VLLM_MM_HASHER_ALGORITHM` env
  vars); `all` Mamba cache mode deprecated; `python -m vllm.entrypoints.grpc_server` deprecated in favor
  of `vllm serve --grpc`; YaRN aligned with Transformers so vendor YaRN aliases no longer re-scale
  `max_model_len`.

### 2.2 v0.29.0 — 2026-09-09 (594 commits, 277 contributors, 91 new) [official]

- **Model Runner V2 now default for all models** (rollout began with pooling models); gained CUDA-graph
  memory profiling for KV cache auto-sizing, batch-sharded sampling (cuts per-step logits memory by 1/TP),
  prompt embeds, `extract_hidden_states` speculation, padded FULL cudagraph dispatch for uniform decode
  under spec decode, DP-sync skipping before EAGLE/MTP draft prefill. **MRV1 deprecated; targeting v0.32
  for removal.** Gaps to close (fall back to MRV1 if configured): sequence parallelism, dual-batch overlap,
  elastic expert parallelism, custom logits processors, some spec-decode methods.
- **New models:** Hy4-preview (Tencent 770B/49B-active MoE, Gated DeepSeek Sparse Attention, native MTP),
  Qwen3.8-Flash-Next (BF16/FP8/NVFP4 + MTP), GraniteSWA/GraniteMoeSWA, NemotronH_Omni_Reasoning_V3 (+MTP),
  Kimi K3 NVFP4 checkpoints, FP8 ModernBERT.
- **Kimi-K3 & DeepSeek V4 performance:** fused MXFP4 top-k finalization (~5% E2E latency), K3 Mamba metadata
  in one Triton launch (6.6–7.6× kernel speedup), tuned Hopper low-latency GEMM now on SM100, GEMM-RS→GEMM-AR,
  K3 DCP with DSpark + DCP partial prefix cache hits; DeepSeek V4 shared experts fused into MegaMoE,
  adaptive top-k width re-landed, opt-in FlashInfer `moe_ep` expert backend.
- **Speculative decoding:** per-request acceptance stats in OpenAI responses (`--per-request-spec-decode-metrics`),
  adaptive verification extended to logprobs, SM100 sparse MLA for GLM-5.2, DSv4 sparse MLA on SM90,
  Qwen3-Omni DSpark drafts, PLaMo3 EAGLE-3/DFlash, DFlash2 loading from speculators format.
- **RL weight sync:** new `sharded_rdt` P2P backend (each worker pulls only its TP/EP slice over NIXL or
  Ray Direct Transport), rank-local IPC weight updates, sparse checkpoint-coordinate updates, gpt-oss
  routed expert loading.
- **Mamba prefix caching:** internal prefill checkpoints → 9–25% TTFT improvement (reported PR numbers).
- **New defaults:** FlashInfer all-reduce enabled by default for TP CUDA groups
  (`VLLM_ALLREDUCE_USE_FLASHINFER=0` to opt out); prefix-cache `NONE_HASH` deterministic by default;
  `--max-num-queued-reqs` / `--max-num-queued-tokens` admission-control flags.
- ⚠️ **Breaking changes:** ten deprecated model architectures removed (Arctic, Chameleon, Cheers,
  Fairseq2Llama, FireRedLID, GritLM, HCXVision, MPT, `RWForCausalLM`/`StableLMEpochForCausalLM` aliases,
  PrithviGeoSpatialMAE → Terratorch); FlexOlmo, Olmo3, Hunyuan V1/VL migrated to the Transformers modeling
  backend; PyAV video decoder removed (use OpenCV/Torchcodec); `python -m vllm.entrypoints.openai.api_server`
  deprecated in favor of `vllm serve`; `VLLM_TEST_FORCE_FP8_MARLIN`, `VLLM_ROCM_USE_AITER_FP4_ASM_GEMM`,
  dead `--attention-config.use_prefill_decode_attention` removed.
- **Hardware:** NVIDIA (DeepSeek V3.2/GLM-5.2 DSA on CUDA path all GPUs; FA4 hdim256 on SM100; SM120 sparse
  MLA fixes; opt-in Rubin Docker builds for CUDA 13.4/13.5); AMD ROCm (dual-stream decode with hipgraphs;
  W4A4 preshuffled asm GEMM default — +15% throughput on Llama-3.3-70B MXFP4, reported; ROCr/CLR update,
  ~20% TPOT improvement reported; CPU offload on ROCm 7.13+; TheRock 7.14 preview docker); Intel XPU (INC
  int4 W4A8 linear backend; AutoRound MXFP8 MoE; EC connector KV offloading); CPU (AMX high-perf MLA backend
  for DeepSeek V2/V3/R1; Int8 MoE via zentorch on AMD Zen).
- **Disaggregation/context parallelism:** NIXL P/D DCP for MLA models; Mooncake Store decode KV saving
  (`save_decode_cache`), hybrid DCP prefix caching; externally transferable KV cache group IDs; EC offload
  connector; CPU→GPU loads ordered against compute stream.
- **API/frontend:** `/v1/messages/render` (Anthropic Messages API), `/cohere/v2/chat/render`, SSE keep-alive
  comments; structured-output fixes (XGrammar termination in batches, GPT-OSS Harmony strict grammar,
  MistralCommonBackend tokenizers); Rust frontend (gRPC audio/video, gRPC LoRA lifecycle control, pure-Rust
  `protox` replacing protoc).
- Dependencies: FlashInfer 0.6.18, huggingface-hub 1.28.0, tpu-inference v0.28.0, NIXL 1.3.2. [official]

### 2.3 v0.28.0 — 2026-08-26 (584 commits, 270 contributors) [official][secondary]

- **Kimi-K3 performance push:** Decode Context Parallel (DCP), fused FlashKDA decode/prefill kernels,
  SiTU activation for MegaMoE, GEMM-RS for sequence parallelism, combined all-gathers (1.5–3× kernel-level
  speedup reported), adaptive speculative token budget (~60% better DSpark TTFT reported), optional
  shared-expert sharding (~17 GiB saved per GPU reported); runs on ROCm with the V2 model runner.
- **DeepSeek V4:** sparse MLA works end-to-end for plain decode, MTP, and DSpark spec decode; AMD Quark
  NVFP4 support; reasoning-effort prompts; ROCm enablement on gfx11 and gfx950.
- **Tiered KV cache offloading:** KV cache offload to disk (object-store secondary tier, DP-replica-aware
  tiering, EC connectors incl. CPU offload); `max_num_batched_tokens` default raised 8192 → 16384.
- **New models:** Muse Glimmer, Ling 3.0 Flash (BF16/FP8/MXFP4), Dots3 NOTE.
- ⚠️ **Breaking changes:** bitsandbytes support migrated to an **out-of-tree plugin**; deprecated
  `calculate_kv_scales` runtime KV-scale calculation, `override_attention_dtype`, MoE legacy code removed;
  KV-offload tiering metrics renamed `kv_offload_tiering_block_{queries,hits}` → `..._chunk_...`;
  Transformers 5.15.0 upgrade (breaking environment change); `reasoning_content` output removal documented
  as a breaking client change.
- Spec decode: DFlash2 with local convolution + candidate selector, DSpark confidence-scheduled
  verification, async scheduling auto-enabled for draft models.
- Model Runner V2 maturation: E/P/D disaggregation, weight offloading, multi-layer MTP KV cache, encoder
  CUDA graphs, `thinking_token_budget` support.

### 2.4 v0.26.0 — 2026-07-27 (411 commits, 212 contributors, 61 new) [official]

- **New Inkling model family** with full support stack: base modeling, piecewise CUDA graph, Hopper FA4
  relative attention, MTP=1 speculative decoding, LoRA, standard ModelOpt NVFP4 quantization.
- **DeepSeek-V4 performance push:** specialized routing kernel (2.94% E2E TPOT, PR-reported), `fused_topk_bias`
  (1.5–2× kernel, PR-reported), redundant repeat/copy removal (1.8% E2E TPOT, PR-reported); ROCm two-stage
  compressor for HCA prefill; sparse decode/prefill optimizations; DSpark speculative decoding on AMD and XPU.
- **`head_dtype`**: fp32 `lm_head` for generation models (also extended to LoRA path, ROCm `torch.mm` fast
  path) — improves generation-head accuracy.
- **Flexible attention backends:** per-KV-cache-group attention backend selection; sliding-window support as
  explicit backend capability (hybrid-model support).
- **KV offloading & tiered secondary storage matured:** offloading metrics, tier-owned event handling,
  object-store secondary tier with workload identity, DP-replica-aware tiering, encoder-cache (EC)
  connectors incl. CPU offloading.
- **Rust frontend:** multimodal video + audio, Seed-OSS tool parser, native `vllm-bench` port.
- Transformers 5.13.0; more models migrated to Transformers backend (Olmo/Olmo2, MistralLarge3, HunyuanVL).
- Source: https://github.com/vllm-project/vllm/releases/tag/v0.26.0 [official]

### 2.5 Notable earlier-2026 items (from secondary summaries) [secondary][unverified for exact PR claims]

- v0.16.0 (2026-02-25): V1 is the only engine — V0 fully removed (as of v0.11+, V0 code fully removed). [secondary]
- 2026-09-04 weekly summary: Qwen3.8-Flash-Next support added (#53896), EC Connector P2P NIXL + CPU EC
  connector (#47941), "no breaking changes" that week. [secondary]
- Kimi K3 blog (vLLM blog, 2026-07-27): 2.8T-parameter multimodal MoE (16 of 896 experts/token, 1M-token
  context); vLLM serves it at 118 tok/s w/o spec decode, **370 tok/s (3.14×) with DSpark on 16× NVIDIA
  GB300 NVL72**; DSpark (block-diffusion spec decode, trained with vLLM + TorchSpec, open-sourced by
  **Inferact**); hybrid prefix caching redesigned over recurrent KDA state. [official]
  (Source: https://github.com/vllm-project/vllm-project.github.io/blob/HEAD/_posts/2026-07-27-k3.md)

---

## 3. Architecture (2026 state)

### 3.1 Core ideas (unchanged foundations)

- **PagedAttention** (SOSP 2023): KV cache split into fixed-size physical blocks (typical 16 tokens),
  per-request page table maps logical→physical blocks; custom CUDA kernels gather non-contiguous KV.
  [official][secondary]
- **Continuous batching / iteration-level scheduling:** requests join/leave between forward passes.
  [official]
- **Prefix caching (Automatic Prefix Caching, APC):** block-level hash-chained KV reuse; in V1,
  **always-on with near-zero overhead** (constant-time eviction). In v0.29: Mamba internal prefill
  checkpoints (+9–25% TTFT, PR-reported); `prefix_cache_retention_interval` CLI arg defaulting to 0;
  deterministic `NONE_HASH`. [official][secondary]

### 3.2 V1 engine → Model Runner V2 (2026)

- **V1** (only engine since V0 removal, v0.11+; confirmed as sole engine in v0.16.0, Feb 2026):
  isolated EngineCore process (scheduler + executor) separate from API server; async single-step
  scheduling with unified token budget `{request_id: num_tokens}`; persistent batch (cached tensors,
  incremental diffs); stateful workers (only diffs sent); chunked prefill **on by default**; APC
  **on by default**; piecewise CUDA graphs + torch.compile; async multimodal preprocessing with
  encoder cache. [official][secondary]
- **Model Runner V2** (MRV2): became **default for all models in v0.29.0 (2026-09-09)**; MRV1 deprecated,
  targeted removal in **v0.32**; fallback to MRV1 when sequence parallelism / dual-batch overlap /
  elastic EP / custom logits processors / certain spec-decode methods are configured. Features in
  v0.30: dual-batch overlap in eager + FULL CUDA graphs, PP speculative decoding, online acceptance
  estimator for adaptive verification, GC-frozen graph capture (12s→2s capture, 28.9s→8.2s init on H200,
  PR-reported). [official]
- **Tensor parallelism** (multi-GPU; ~7.2× throughput on 8× A100 per one secondary explainer),
  **pipeline parallelism**, cross-node PP via InfiniBand; **data parallelism**; **expert parallelism**
  incl. **Elastic EP** (reconfiguration reusing CUDA graphs; DeepEP v2, FlashInfer one-sided All2All).
  [official][secondary]
- **Disaggregated prefill/decode:** first-class support via **NIXL connector** and **Mooncake** connectors;
  v0.29 added NIXL P/D DCP for MLA models, Mooncake Store decode KV saving (`save_decode_cache`), hybrid
  DCP prefix caching, EC connectors (P2P NIXL + CPU). **Context parallelism:** DCP (decode) and PCP
  (prefill) for sparse-MLA models; FlashInfer native CP for MLA decode; FlashMLA sparse DCP on Hopper.
  [official]
- **KV-cache management 2026:** KV offloading & tiered secondary storage (object-store secondary tier with
  workload identity, DP-replica-aware tiering, EC connectors incl. CPU offload); disk offload for large
  contexts (v0.28); HiSparse host-resident tier for sparse-MLA decode (v0.30); FP8/C8 KV cache (incl. GQA,
  DeepSeek-V3.1 PD scenario in Ascend plugin); MXFP8/NVFP4 KV caches (`nvfp4_fp8_ds_mla`, `fp8_ds_mla`);
  SSD support for multiple DP ranks in Mooncake offload dirs (experimental, Ascend plugin). [official]
- **Speculative decoding (2026):** EAGLE3 (incl. for Sarvam MLA), DSpark (block-diffusion; 3.14× on Kimi K3,
  GB300 NVL72, per vLLM blog), DFlash2 (local convolution + candidate selector), MTP multi-token prediction
  (Qwen3.8-Flash-Next native MTP, Nemotron, PLaMo3 EAGLE-3/DFlash), P-Eagle and PARD (stable parallel spec
  methods per Ascend plugin notes), DSpark on AMD and XPU (v0.26), adaptive verification (online acceptance
  estimator, v0.30), per-request acceptance stats in OpenAI responses (`--per-request-spec-decode-metrics`,
  v0.29). [official][secondary]

---

## 4. Quantization support

Single dispatch point `vllm/model_executor/layers/quantization/__init__.py` (`--quantization` flag).
State as of Aug–Sep 2026 [secondary, verified against source tree by third-party researchers]:

| Format | Scheme / notes |
|---|---|
| FP8 | `fp8` W8A8 (per-tensor/per-channel; compressed-tensors FP8 — "Hopper default"); `modelopt` (ModelOpt-exported FP8); KV cache FP8 (`fp8_ds_mla`, `nvfp4_fp8_ds_mla`); FP8 MoE kernels (block-FP8 fused shared experts) |
| NVFP4 / MXFP4 | `modelopt_fp4` (native on SM100/103 Blackwell; emulated from SM75); `mxfp4` / `gpt_oss_mxfp4` (GPT-OSS ships MXFP4); Kimi-K3 NVFP4; FlashInfer CuTeDSL NVFP4 W4A16 default over Marlin on SM100/103 (v0.30); DeepSeek V4.1 whole KV in MXFP8 (v0.30) |
| MXFP8 | `mxfp8` online (v0.19+); `modelopt_mxfp8`; FlashInfer TRT-LLM MXFP8 linear backend (v0.29); Humming MoE MXFP4 weights + block-FP8 activations (v0.29) |
| AWQ / GPTQ | `awq_marlin`, `gptq_marlin` W4A16 (Marlin kernels; recommended); legacy `awq`/`gptq` unfused deprecated; ⚠️ GPTQ `g_idx` activation ordering **removed in v0.30** |
| INT8 | `int8_per_channel_weight_only`; Marlin INT4/INT8; `experts_int8` (use int8_per_channel_weight_only) |
| Online quantization | `online` + shorthands (`fp8_per_tensor`, `fp8_per_block`, `fp8_per_channel`, `mxfp8`, `int8_per_channel_weight_only`, `nvfp4_per_token`) — quantize from BF16 checkpoint at load; **targeted online quantization** via `quantization_config.targets` (v0.30) |
| compressed-tensors | neuralmagic/Red Hat llm-compressor output; SpinQuant + QuaRot integrated via llm-compressor (vLLM v0.11+) [secondary] |
| AMD | `quark` (ROCm path); AMD Quark NVFP4 (v0.28); W4A4 preshuffled asm GEMM default on ROCm (+15% Llama-3.3-70B MXFP4, PR-reported) |
| Intel | `inc` / AutoRound (Intel Neural Compressor; AutoRound 2/3/5/6/7-bit on CUDA, v0.30); XPU INC int4 W4A8 linear backend (v0.29) |
| Others | torchao, GGUF (plugin, experimental/under-optimized), ModelOpt mixed (`modelopt_mixed`), Humming (W4A8 FP4×FP8, Ant Group), CPU AWQ (`cpu_awq` legacy) |
| Deprecated/removed | `fbgemm_fp8`, `fp_quant`, `moe_wna16` (legacy); **bitsandbytes → out-of-tree plugin** (v0.28, breaking) |

Per-quantization **linear backend overrides** added in v0.30 (`--linear-backend` choices: auto, cutlass,
deep_gemm, flashinfer_cutlass, flashinfer_cudnn, marlin, machete, humming, torch, triton, emulation).
[secondary; release notes official]

---

## 5. 2026 feature surface (serving features)

### 5.1 Structured outputs / JSON mode [official][secondary]
- Guided decoding constrains generation to JSON schemas, regex, grammars, choices
  (`guided_json`, `guided_regex`, `guided_choice`, `guided_grammar`).
- Structured-outputs engine: default backend `auto` selects **xgrammar** or guidance/llguidance; <5% latency
  impact claimed by third-party guides [secondary].
- 2026-02-12: generic `structured_outputs` enabled for the **Responses API** (grammars/regexes/choices
  beyond JSON schema; #33709). [secondary]
- v0.30: XGrammar V4.1 schema constraints for strict tool parameters (DeepSeek-V4.1); XGrammar termination
  in batches fixed (v0.29); GPT-OSS Harmony strict grammar. [official]

### 5.2 Reasoning (multi-step) serving [official][secondary]
- Reasoning parsers (token-attributed text in Rust frontend, 2026-09; `reasoning_token_count` support),
  `reasoning_effort` prompts (DeepSeek V4, v0.28), `enable_thinking` defaults (Gemma4), `thinking_token_budget`
  (MRV2, v0.28), reasoning-end detection scoped to current turn, Kimi K3 reasoning/tool parsers (v0.30).
- ⚠️ `reasoning_content` output removal was a documented **breaking client change** in v0.28.

### 5.3 LoRA serving [official]
- Multi-LoRA serving: multiple adapters on one base model, per-request selection via `--lora-modules`
  / `--enable-lora` / client `model=name`; supports dense and MoE layers.
- v0.29: LoRA for DeepSeek V4, Qwen3-Omni multimodal LoRA, LLaVA-NeXT tower/connector LoRA, LFM2-VL,
  Qwen3.5 embedding modules, partial LoRA on Qwen3.5/3.6 GatedDeltaNet; fp32 `lm_head` via `head_dtype`
  extended to LoRA path (v0.26). v0.30: rsLoRA scaling in MoE expert packing, `--lora-modules name=path`
  with `=` in path. [official]

### 5.4 Multi-modal (vision/audio/video) [official]
- Images, **video** (torchcodec backend, MP4 edit-list-aware frame sampling, encoder CUDA graphs for
  MiniCPM-V 2.5/2.6/4.0, video embeds via Python frontend v0.29), **audio** (torchcodec audio decoding,
  torchaudio resampler, `audio_backend` selectable in `--media-io-kwargs`, `/v1/audio/transcriptions`,
  `/v1/realtime` WebSocket), encoder cache for chunked-prefill VLMs, encoder-cache sharing over NIXL and
  Mooncake (v0.30), Qwen3-VL-235B / MiniMax-M3 / Gemma-4 / Llama-4-era VL support, multimodal LoRA.
- Rust frontend gained multimodal video + audio (v0.26). [official]

### 5.5 Prefix caching
- APC always-on in V1; Mamba internal prefill checkpoints (9–25% TTFT, PR-reported, v0.29); hybrid prefix
  caching redesigned for Kimi K3's recurrent KDA state (benefits all hybrid linear models, per vLLM blog);
  K3 DCP partial prefix cache hits (v0.29); K3 internal prefix checkpoints with partial prefix caching +
  spec decode (v0.30); Mooncake hybrid DCP prefix caching. [official]

### 5.6 Other 2026 service features
- **Watermarking** (Gumbel-max, v0.30); **Fast Start** IPC weight cache (v0.30); **RL weight sync**
  (`sharded_rdt` P2P over NIXL/Ray, v0.29); **scale-out endpoints** opt-in via `--enable-scale-out` (v0.30);
  admission control flags; `/v1/messages/render` (Anthropic) + `/cohere/v2/chat/render`; pooling/embedding
  models (ModernBERT, BGE-M3, Qwen3-Embedding on TPU); tool calling (Seed-OSS parser, Gemma4/Harmony
  strict grammars). [official]

---

## 6. Benchmarks (throughput/latency)

### 6.1 Official / vendor-reported numbers
- **vLLM blog, 2026-07-27 (Kimi K3):** 118 tok/s without spec decode → **370 tok/s (3.14×) with DSpark**,
  16× NVIDIA GB300 NVL72. [official]
- **vLLM release notes (PR-reported, not independently verified):** Kimi K3 ~5% E2E latency (fused MXFP4
  top-k), 6.6–7.6× kernel speedup (Mamba metadata, one Triton launch), ~17 GiB/GPU saved (shared-expert
  sharding), ~60% better DSpark TTFT (adaptive speculative budget); DeepSeek-V4 routing kernel −2.94% E2E
  TPOT; ROCm W4A4 preshuffled asm GEMM +15% (Llama-3.3-70B MXFP4); B200 FP8 MoE tuning; Hopper low-latency
  GEMM also dispatched on SM100. [official — treat as vendor-reported]
- **NVIDIA GB300 / DeepSeek NVFP4** (third-party collected, unattributed vendor origin): DeepSeek-V3.2
  prefill-only 7,360 tok/GPU/s; DeepSeek-R1 (2× GPU) prefill 22,476 tok/GPU/s; "8–20× over Hopper with
  Blackwell + NVFP4". [secondary — vendor-reported, methodology not shown]
- **NVIDIA Blackwell RTX PRO 6000** (third-party collected): vLLM (NVFP4) 8,033 tok/s, TTFT 10.7 ms vs
  SGLang (GPTQ-INT4) 6,395 tok/s, 100% success at concurrency 128. [secondary]
- **Google Cloud, 2026-08-26 (vLLM TPU embedding):** Qwen3-Embedding-8B on TPU Ironwood (bf16, 16K+ seq,
  TP=4): 83,996 total tok/s, 5.13 req/s; cosine-similarity parity ≥0.999 text / ≥0.995 multimodal.
  [vendor-reported][secondary]

### 6.2 Independent measurements
- **SemiAnalysis InferenceX (Aug 2026)** [independent]:
  - DeepSeek V4 Pro 0813 (1.6T params / 49B active), agentic-coding replay (p50 88k in / p90 272k in tokens):
    MI355X **SGLang** matched B200 **vLLM** on perf/$ **until 2026-08-21**, when vLLM optimizations from
    Inferact and NVIDIA pushed B200 ahead — "a close race rather than a settled one". AMD published its own
    DeepSeek V4 vLLM optimization list (vllm-project/vllm#52911).
  - AMD MI355X Kimi-K2.5 MXFP4: one vLLM PR (#35850, merged 2026-03-06, shipped v0.18) moved 6.6 → **78.9
    tok/s/user** (8k/1k workload), 12.0× interactivity at low batch, 7.7× peak throughput, 15× at
    iso-throughput; peak **2,687 tok/s/GPU** (25 days from baseline to full effect).
  - Kimi K3: between 40–60 s E2E latency, MI355X **ATOM** (AMD vendor engine) beats even GB300 NVL72 vLLM on
    perf/$ — caveat: ATOM is a vendor engine, vLLM is the open comparison.
- **dstack DeepSeek-R1 benchmark (2026)** [independent]:
  - H200: TensorRT-LLM highest online throughput (4,176 tok/s); **vLLM led at concurrencies <128** in online
    throughput + E2E latency; offline: SGLang 6,311 tok/s.
  - MI300X: **vLLM outperformed SGLang in both online and offline throughput and E2E latency**; best online
    4,574 tok/s (vLLM); SGLang better only at concurrencies <32.
- **L40S head-to-head (Medium, Aug 2026)** [independent]: long-context workload — TensorRT-LLM ~2× throughput
  (75.9 vs ~40 tok/s) and 7.5× faster avg TTFT (8.6 s vs vLLM 63.8 s / SGLang 68.6 s); vLLM slight edge over
  SGLang in throughput (41.0 vs 38.2 tok/s) and TTFT; vLLM ITL 68.0 ms vs SGLang 72.6 ms. Quantization sweep
  on vLLM: FP8 vs AWQ vs GPTQ (figures only, no extracted numbers).
- **Single-L4 reproducibility study (inference-bench, GitHub)** [independent]:
  - Short regime (c=64, Qwen2.5-7B): vLLM AWQ **976 tok/s** > SGLang FP16 914 > vLLM FP16 831 > SGLang AWQ 506.
  - A100 sweep (vLLM v0.20.1 vs SGLang, May 2026, Qwen2.5-7B): vLLM FP16 3,102 tok/s @ c=64 vs SGLang 2,141;
    Marlin kernels +70% in v0.20.1 (177 vs 104 tok/s @ c=1 vs v0.8.5); SGLang Marlin collapsed at c=64
    (2,231 vs vLLM 4,762) — "opposite of the L4 pattern where SGLang wins at c=64".
- **ai-lab-benchmarks (Aug 2026, production model Qwen3.6-35B-A3B)** [independent]: NVFP4/vLLM vs GGUF/llama.cpp —
  4.7× throughput (47.1 vs 10.0 items/s), 3.4× faster prompt reading (13,916 vs 4,068 tok/s at 8k), generation
  201.5 vs 167 tok/s; translation quality identical (chrF++ 69.33 vs 69.34). Author noted SGLang setup friction
  (mem-fraction 0.80 vs vLLM 0.90; NVFP4 MoE runner backend flag) — "real setup cost".
- **Architecture-aware AMD study (arXiv 2603.10031, Feb 2026, MI325X ×8, vLLM v0.14.1)** [independent]:
  Llama-3.1-405B 15,944 tok/s vs DeepSeek V3.2 15,343 tok/s peak (text-only); Qwen3-VL-235B 47,873 tok/s
  (vision, incl. image tokens); Kimi-K2.5 7,327 tok/s; AITER required for competitive MLA (+3–5% at high
  concurrency on Llama-405B, MoE/MLA speedups larger).
- ⚠️ Legacy A100 numbers still circulating (17.12 req/s OPT-13B vs HF TGI 0.71 req/s) are from the
  original vLLM paper era — illustrative of PagedAttention's original gains, not 2026 hardware. [secondary]
- ⚠️ Viral "29% gap" claims (SGLang ~16,200 vs vLLM ~12,500 tok/s on H100 Llama-3.1-8B): the primary source
  (RunPod) attributes the gap almost entirely to **prefix-heavy** traffic (RadixAttention prefix reuse);
  on unique prompts the engines are "within a few percent". Do not quote the 29% without the workload
  qualifier. [secondary]

---

## 7. Deployment

### 7.1 Official artifacts [official]
- **PyPI:** `pip install vllm` (CUDA 13.0); `uv pip install vllm --torch-backend=auto`; ROCm wheels at
  `https://wheels.vllm.ai/rocm/<ver>/rocm723`; XPU via `https://wheels.vllm.ai/<ver>/xpu`; CPU wheels
  (x86_64, arm64, macOS); CUDA 12.9/13.0 wheels for x86_64 and arm64.
- **Docker (v0.30.0):** `vllm/vllm-openai:v0.30.0` (CUDA 13.0), `:v0.30.0-cu129`, `:v0.30.0-ubuntu2404`,
  `:v0.30.0-cu129-ubuntu2404`, **`vllm/vllm-openai-rocm:v0.30.0`**, **`vllm/vllm-openai-cpu:v0.30.0`**,
  **`vllm/vllm-openai-xpu:v0.30.0`**; opt-in Rubin builds for CUDA 13.4/13.5 (v0.29).
- **Hardware plugins (out-of-tree):** vllm-ascend (Huawei Ascend NPU; release line tracks upstream,
  e.g. v0.22.1rc1 2026-06-30 with Mooncake DeepSeek-V4/HCCL weight transfer/Ascend 950 W8A8),
  vllm-gaudi (Intel Gaudi 2/3; v0.26.0 on upstream v0.26.0 + Gaudi software v1.24.1),
  tpu-inference (Google TPU v5e/v6e/v7x; v0.28.0 pinned in vLLM v0.29 deps), XPU support in-tree-ish
  (official wheels + docker), CPU backend in-tree.

### 7.2 Kubernetes [official][secondary]
- **vLLM Production Stack** (https://github.com/vllm-project/production-stack; 2,627 stars / 507 forks
  as of 2026-09-22 [official]): Helm chart (`helm repo add vllm https://vllm-project.github.io/production-stack;
  helm install vllm vllm/vllm-stack -f values.yaml`) installing serving engine + KV-cache-aware **request
  router** behind ClusterIP; Prometheus/Grafana observability; autoscaling guidance via KEDA on
  `vllm:num_requests_waiting`; deploy guides incl. Oracle OKE and Nebius; **Kubernetes operator with
  `VLLMRuntime` and `VLLMRouter` CRDs** (production-stack.ai API group). [official][secondary]
- Ecosystem: **llm-d** (Red Hat/Google/IBM/NVIDIA founded; 4,624 stars; P/D disaggregation at scale, GKE
  well-lit paths, NIXL KV transfer; vLLM as default model server; AMD/Intel/TPU/Rebellions accelerator
  maintainers), **AIBrix** (4.7k stars; LoRA management, SLO-aware autoscaling), **KubeAI** (1.2k stars,
  lightweight, scale-from-zero), Ray Serve + vLLM (KubeRay) for multi-node/multi-model. [secondary]
- Docs: https://docs.vllm.ai (official); vLLM recipes tool: https://recipes.vllm.ai (hardware × model ×
  feature recipes, e.g. Kimi-K2.5 on MI355X with tool_calling/reasoning/encoder_parallel). [official]

### 7.3 Cloud integrations [official][vendor-reported][secondary]
- **Google Cloud:** vLLM on TPU via `tpu-inference` plugin (JAX + PyTorch unified runtime; TPU v5e/v6e/v7x,
  Trillium, Ironwood); GKE deployment guides; 2026-08-26 native TPU support for production embedding
  pipelines (see §6.1).
- **AWS:** Trainium support exists (named in comparison tables; no dedicated 2026 announcement found —
  ⚠️ status should be confirmed in docs). [unverified]
- **Broadcom/VMware:** VMware AI Factory (announced ~2026-09-01, [secondary]) ships a **vLLM-based
  inference runtime** for VCF private AI, >150 models runnable, 5 fully validated as managed services
  (NVIDIA Nemotron 3, Google DeepMind Gemma 4, NEC cotomi, Alibaba Qwen 3.8-27B, Z.ai GLM 5.2); AMD
  MI350 + ROCm validated path with zero-touch provisioning.
- **Red Hat OpenShift AI:** see §9.

---

## 8. Hardware backends (2026 status)

| Backend | Status | Evidence |
|---|---|---|
| NVIDIA CUDA | Primary; Hopper/Blackwell (+opt-in Rubin), Ada/Lovelace, GB10/GB300 | Release notes per-version kernel work; CUDA 13.0 default wheels [official] |
| AMD ROCm | First-class community path; AITER kernels; MI300X/MI325X/MI350X/MI355X (gfx90a/gfx942/gfx950); dual-stream decode w/ hipgraphs; W4A4 asm GEMM; TheRock 7.14 preview docker | v0.29/v0.30 release notes; official `vllm-openai-rocm` images [official] |
| Google TPU | Via `tpu-inference` plugin (vllm-project); v5e/v6e/v7x; native FP8 on v6e/Ironwood, INT8 on v5e | Google Cloud docs; dev.to/Google posts Aug 2026 [official][vendor-reported] |
| Intel HPU (Gaudi) | Out-of-tree plugin **vllm-gaudi** (now under vllm-project org), v0.26.0 tracking upstream v0.26.0 + Gaudi SW v1.24.1 + PT 2.11; Gaudi 2/3; INC/AWQ/GPTQ/ModelOpt/compressed-tensors; NIXL connector | vllm-gaudi release notes [official] |
| Intel XPU | Official wheels + `vllm-openai-xpu` docker; INC int4 W4A8 linear backend; AutoRound MXFP8 MoE; Data Center GPU Max 1550+ | v0.29/v0.30 notes [official] |
| CPU | In-tree; AVX512/AMX kernels (DeepSeek V2/V3/R1 MLA end-to-end on AMX); Int8 MoE via zentorch on AMD Zen; macOS/arm64 wheels | v0.29/v0.30 notes [official] |
| Huawei Ascend NPU | Out-of-tree **vllm-ascend** (vllm-project org), tracking upstream releases | vllm-ascend release notes [official] |
| Others (community) | Iluvatar BI-V150, MetaX C500X, Rebellions NPU via llm-d; AWS Neuron/Trainium referenced | llm-d accelerator docs [secondary] |

---

## 9. Ecosystem: repos, contributors, sponsors, forks

### 9.1 Repository scale (GitHub API, 2026-09-22) [official]
- vllm-project/vllm: **92,444 stars / 22,525 forks**
- sgl-project/sglang: 36,323 / 9,061 (SGLang comparison baseline)
- LMCache/LMCache: 11,892 / 1,938
- llm-d/llm-d: 4,624 / 786
- vllm-project/vllm-ascend: 2,873 / 2,347
- vllm-project/production-stack: 2,627 / 507
- vllm-project/tpu-inference: 438 / 322
- vllm-project/vllm-gaudi: 57 / 151
- vllm-project/dllm-plugin: 29 / 10 (diffusion-LLM plugin; LLaDA2.0 benchmarks May 2026)

### 9.2 Contributors & governance [official][secondary]
- Origins: UC Berkeley Sky Computing Lab + LMSYS; Woosuk Kwon initiated the project (scheduler/model
  runner). [official][secondary]
- V1 effort "mainly driven together" by **UC Berkeley, Neural Magic (now Red Hat), Anyscale, and Roblox**
  (vLLM blog, 2025-01-27). [official]
- Current contribution is heavily multi-vendor: NVIDIA, AMD, Intel, Red Hat, Google, Anyscale, Roblox
  engineers appear across release notes (kernel work, plugins, connectors); weekly merge volume in Sep
  2026 ≈ 358 PRs/week from 183 contributors (2026-09-04 week); v0.30.0: 762 commits / 315 contributors /
  104 new. [official][secondary]
- No Apache/CNCF/LF foundation umbrella; community-led open source governance (third-party assessment,
  Jul 2026). [secondary]
- UC Berkeley Sky Computing Lab and LMCache Lab (UChicago) are founding academic supporters of llm-d,
  described as "originators of vLLM" / "originators of LMCache". [secondary]

### 9.3 Sponsors (from the project's README sponsors section) [official]
- **Cash donations:** a16z, Dropbox, Sequoia Capital, Skywork AI, ZhenFund.
- **Compute resources:** Alibaba Cloud, AMD, Anyscale, AWS, Crusoe Cloud, Databricks, DeepInfra, Google
  Cloud, Intel, Lambda Lab, Nebius, Novita AI, NVIDIA, Replicate, Roblox, RunPod, Trainy, UC Berkeley,
  UC San Diego.
- **Slack sponsor:** Anyscale. Fundraising via OpenCollective (opencollective.com/vllm).
- ⚠️ Sponsor list read from README copies in GitHub forks (crawled Sep 2026); presumed current — the
  authoritative copy is https://github.com/vllm-project/vllm#sponsors. [official — verify before quoting verbatim]

### 9.4 Notable forks / related projects [official][secondary]
- **vllm-ascend** (Huawei Ascend NPU, vllm-project org) — out-of-tree plugin w/ own release line. [official]
- **vllm-gaudi** (Intel Gaudi, vllm-project org) — succeeded the deprecated HabanaAI/vllm-fork (EOL 2025-11). [official]
- **candle-vllm** (ericlbuehler) — Rust/Candle port with GPTQ/Marlin, FP8 KV, Metal/CPU, MCP support. [secondary]
- **vLLM SR / router ("Themis")** (vllm-project org) — serving-router with replay, observability, Redis/Valkey/Qdrant
  storage backends, long-context routing (v0.3 post, 2026-06-05). [official]
- **LMCache** (LMCache Lab, UChicago) — KV-cache reuse/offload layer integrated with vLLM. [secondary]
- **llm-d** (Red Hat/Google/IBM/NVIDIA) — Kubernetes-native distributed inference on top of vLLM/SGLang. [secondary]

---

## 10. Enterprise support & large deployments

### 10.1 Enterprise support options
- **Red Hat AI Inference Server** — enterprise-grade, commercially supported, security-hardened vLLM
  distribution (container image; standalone or in RHEL AI / OpenShift AI); built on vLLM + **Neural Magic**
  technologies (Red Hat acquired Neural Magic; LLM Compressor for quantization/pruning; pre-optimized model
  registry on Hugging Face; validated models; multi-accelerator: NVIDIA, AMD, Intel Gaudi, Google TPU);
  MLPerf inference results published with Supermicro on OpenShift AI + vLLM runtime (Red Hat blog).
  [vendor-reported][secondary]
- **nm-vllm** — Neural Magic's enterprise distribution of vLLM: stable builds with bug fixes and selected
  model backporting, **enterprise support with SLAs**, Kubernetes reference architectures, pre-optimized
  model registry (Red Hat Developer article; "nm-vllm" branding predates the acquisition — ⚠️ current
  naming under Red Hat AI should be verified). [vendor-reported]
- **VMware AI Factory (VCF)** — vLLM-based inference runtime as the managed model-serving layer for private
  AI on VMware Cloud Foundation (announced ~2026-09-01; techtimes). [secondary]
- ⚠️ No evidence found of a first-party "vLLM Enterprise" SKU from the vLLM project itself; enterprise
  support is delivered by vendors (Red Hat et al.). [unverified — absence of evidence]

### 10.2 Known production users (as reported by third parties; not officially confirmed by the companies)
- **Meta, LinkedIn, Mistral, Hugging Face** — named as running vLLM in production (third-party reading
  list, evaluated May 2026). [secondary — unverified by the companies]
- **Uber, LinkedIn** — named in a 2026 self-hosting guide as production users. [secondary — unverified]
- **Tesla** (ML Platform team) and **Cohere** (model serving + RL) — listed as llm-d users (2025/2026).
  [secondary]
- **JPMorgan Chase, Goldman Sachs** — cited as running AI workloads on Kubernetes with the vLLM-inclusive
  stack (third-party guide). [secondary — unverified]
- **Lambda, Mistral AI** — llm-d launch partners supporting the vLLM-based distributed-serving ecosystem.
  [secondary]

---

## 11. Competitive positioning vs SGLang (2026)

| Dimension | vLLM | SGLang | Notes |
|---|---|---|---|
| Core innovation | PagedAttention (block paging) | RadixAttention (radix tree) | Architectural differentiator |
| GitHub (2026-09-22) | 92,444★ / 22,525 forks | 36,323★ / 9,061 forks | vLLM ~2.5× stars [official] |
| Prefix-heavy throughput | ~12,500 tok/s | ~16,200 tok/s (H100, Llama-3.1-8B) | ~29% SGLang edge **only with shared prefixes**; unique prompts within a few % (RunPod, 2026) [secondary] |
| Prefix caching | Block-level hash (APC, always-on in V1) | Token-level radix tree (better for branching/agentic) | SGLang wins tree-structured reuse (MCTS, tool rollbacks) [secondary] |
| Structured output | xgrammar/guidance; noticeable overhead at high batch (per TECHSY) | Minimal overhead (overlapped mask gen) | SGLang edge on constrained decoding [secondary] |
| Hardware breadth | NVIDIA, AMD, Intel (XPU/Gaudi), TPU, CPU, Ascend | NVIDIA, AMD (narrower) | vLLM's main differentiator: breadth [secondary] |
| Model adoption speed | Fastest day-0-ish support (Kimi K3, DeepSeek V4.x, GLM-5.x) | Fast, smaller kernel team | [secondary] |
| Ease of setup | `pip install vllm` | More friction (mem-fraction, backend flags) | Independent benchmark author noted "real setup cost" for SGLang [independent] |
| Quantization | 29 `--quantization` values; Marlin W4A16; NVFP4/MXFP4; online quant | Comparable breadth; quark_int4fp8_moe, petit_nvfp | Rough parity mid-2026 [secondary] |
| Disaggregation | NIXL + Mooncake; DCP/PCP | Mooncake/NIXL backends | Both support [secondary] |
| Long-context TTFT | Weaker (L40S: 63.8 s vs TensorRT-LLM 8.6 s @ 8k ctx) | Similar to vLLM (68.6 s) | TensorRT-LLM leads this niche [independent] |

- Positioning summary (multiple 2026 sources converge): **vLLM = breadth** (models × hardware × deployment,
  largest ecosystem, fastest new-model support); **SGLang = depth on prefix-heavy/agentic workloads**
  (RadixAttention, structured generation); **TensorRT-LLM = NVIDIA peak throughput** at the cost of a
  compile step; LMDeploy leads quantized-model serving in some comparisons. [secondary][independent]
- ⚠️ TECHSY (Sep 2026) claims "Hugging Face put TGI into maintenance mode in December 2025 and now points
  teams toward vLLM or SGLang" — could not independently verify; treat as [unverified].
- ⚠️ Star-count comparisons in third-party articles are stale (e.g. "17k+ vs 15k+", "72.4k as of Mar 2026");
  current counts (Sep 2026) are in §9.1. [official]

---

## 12. Key uncertainties & gaps

1. ⚠️ **v0.30.0 released 2026-09-22 (today)** — release notes read the same day; adoption/bug reports not
   yet available. Flag early-adopter risk, esp. breaking changes (scale-out endpoints opt-in, `g_idx`
   removal, gRPC entrypoint deprecation). [official]
2. ⚠️ **MRV1 removal targeted at v0.32** — exact v0.32 date unknown; feature gaps (sequence parallelism,
   elastic EP, custom logits processors) still fall back to MRV1. [official]
3. ⚠️ **Production-user claims** (Meta/LinkedIn/Mistral/HF/Uber/JPMorgan) are third-party assertions; no
   company-issued confirmation found in this research. [unverified]
4. ⚠️ **AWS Trainium** support status not confirmed from an authoritative 2026 source. [unverified]
5. ⚠️ **TGI maintenance-mode claim** (TECHSY) unverified. [unverified]
6. ⚠️ **Sponsor list** copied from forked READMEs; verify against the canonical README before citation. [official]
7. ⚠️ PR-reported performance percentages in release notes (e.g. "−2.94% E2E TPOT", "+15% throughput")
   are micro-benchmark claims from the PR author, not audited results. [official — vendor-reported]
8. ⚠️ BitsAndBytes migration to out-of-tree plugin (v0.28) — plugin's maintenance/availability not verified
   in this research. [unverified]

---

## 13. Sources

- GitHub releases (official): https://github.com/vllm-project/vllm/releases (API + v0.26.0/v0.29.0/v0.30.0 notes)
- vLLM blog — Kimi K3 (2026-07-27): https://github.com/vllm-project/vllm-project.github.io/blob/HEAD/_posts/2026-07-27-k3.md
- vLLM V1 alpha post (2025-01-27): https://github.com/vllm-project/vllm-project.github.io/blob/HEAD/_posts/2025-01-27-v1-alpha-release.md
- vLLM Production Stack docs: https://github.com/vllm-project/production-stack
- vLLM Gaudi plugin release notes: https://github.com/vllm-project/vllm-gaudi/blob/HEAD/docs/release_notes_v0.26.0.md
- vLLM Ascend release notes (v0.22.1rc1, 2026-06-30; v0.18.0rc1, 2026-04-01)
- vLLM recipes: https://github.com/vllm-project/recipes ; TPU inference: https://github.com/vllm-project/tpu-inference
- SemiAnalysis InferenceX blogs (2026-08-25): DeepSeek-V4-Pro-AgentX MI355X vs B200; MI355X Kimi-K2.5 AITER 7× speedup; Kimi-K3 AgentX MI355X vs GB300 NVL72
- dstack DeepSeek-R1 benchmark (H200/MI300X): https://github.com/dstackai/dstack docs/blog
- inference-bench (L4/A100 vLLM vs SGLang, May 2026): https://github.com/ree2raz/inference-bench
- ai-lab-benchmarks engines-2026-08: https://github.com/marianvid/ai-lab-benchmarks
- arXiv 2603.10031 (Feb 2026): Architecture-Aware LLM Inference on AMD MI325X
- RunPod comparison (vLLM vs TensorRT-LLM vs SGLang, 2026): https://www.runpod.io/articles/comparison/vllm-vs-tensorrt-llm
- TECHSY vLLM vs SGLang (Sep 2026): https://techsy.io/en/blog/vllm-vs-sglang
- explore.n1n.ai — SGLang vs vLLM architecture (2026-09-20)
- Red Hat Developer — LLM Compressor / nm-vllm: https://developers.redhat.com/articles/2024/08/14/llm-compressor-here-faster-inference-vllm
- ISTA — Neural Magic acquired by Red Hat: https://ist.ac.at/en/news/neural-magic-ai-startup-with-ista-mit-roots-acquired-by-red-hat/
- Red Hat AI Inference Server: https://www.press.in.th/red-hat-unlocks-generative-ai/ ; digitalisationworld (2026)
- TechTimes — VMware AI Factory (2026-09-01): https://www.techtimes.com/articles/326160/20260901/enterprise-gpu-chaos-ends-vmware-ai-factory-ships-full-stack-bare-metal-model.htm
- Google Cloud TPU docs: https://docs.cloud.google.com/tpu/docs/tpu-inference ; vLLM TPU announcement (CloudSteak)
- premai.io — LLMs on Kubernetes guide (2026): https://www.premai.io/blog/deploying-llms-on-kubernetes-vllm-ray-serve-gpu-scheduling-guide-2026/
- Oracle — Deploy OpenAI vLLM Production Stack on OKE: http://docs.oracle.com/en/learn/deploy-vllm-production-stack-oke/
- Quantization catalog (third-party, source-verified): https://github.com/kriptoburak/air-gapped-skills vllm-quantization SKILL.md ; vllm-quant-deep-dive.md (allanschramm/local-model-autotuning)
- oss-atlas vLLM page (Jul 2026): https://github.com/zhenninglang/oss-atlas vllm.md
- vllm-daily digests (2026-02/2026-04): https://github.com/vllm-project/vllm-daily
- V1 architecture skill note: https://github.com/tylertitsworth/skills vllm/SKILL.md
- candle-vllm (ericlbuehler): https://github.com/ericlbuehler/candle-vllm


# PART 2 — SGLang

# SGLang — Inference-Serving Framework (LMSYS) — Research Report

> **Research date:** 2026-09-22 (UTC)
> **Project:** RAG data collection, Step 4 (AI infra — inference/training)
> **Scope:** SGLang news and state as of September 2026
> **Provenance legend:** `[official]` = sgl-project GitHub / docs.sglang.io / lmsys.org blog; `[vendor-reported]` = company marketing claims; `[independent]` = third-party measurements (SemiAnalysis InferenceX, dstack, community); `[secondary]` = press/blogs summarizing others; `[unverified]` = could not be confirmed.
> Every fact below carries its date. Uncertainties are marked explicitly.

---

## 1. Project identity & ecosystem snapshot

- **What it is:** SGLang is a high-performance serving framework for large language models and multimodal models, "designed to deliver low-latency and high-throughput inference across a wide range of setups, from a single GPU to large distributed clusters" [official, GitHub README, viewed 2026-09-22](https://github.com/sgl-project/sglang).
- **Repository:** `github.com/sgl-project/sglang`, Apache-2.0 license, default branch `main`, created 2024-01-08 [official, GitHub API, 2026-09-22](https://api.github.com/repos/sgl-project/sglang).
- **GitHub traction at research date:** **36,323 stars**, **9,061 forks**, 184 subscribers, 5,359 open issues [official, GitHub API, `updated_at` 2026-09-22T19:08:12Z](https://api.github.com/repos/sgl-project/sglang). Last push at research date: 2026-09-22T19:29:47Z.
- **Governance:** Hosted under the non-profit open-source organization **LMSYS** [official]. Originated in 2023 inside the UC Berkeley lab of Databricks co-founder Ion Stoica [secondary, TechCrunch 2026-01-21](https://techcrunch.com/2026/01/21/sources-project-sglang-spins-out-as-radixark-with-400m-valuation-as-inference-market-explodes/).
- **Scale claims (project-stated):** "deployed at large scale, generating trillions of tokens in production each day"; "deployments running on over 400,000 GPUs worldwide"; "the de facto industry standard" [official, README]. Treat exact token/GPU counts as **[unverified]** (self-reported, no independent audit found).
- **Design heritage:** README acknowledges reusing design/code from Guidance, vLLM, LightLLM, FlashInfer, Outlines, LMQL [official].

### Named adopters (README, 2026-09-22) [official]
xAI, NVIDIA, AMD, Intel, LinkedIn, Cursor, Oracle Cloud, Google Cloud, Microsoft Azure, AWS, Atlas Cloud, Voltage Park, Nebius, DataCrunch, Novita, RunPod, InnoMatrix, Modal, MIT, UCLA, University of Washington, Stanford, UC Berkeley, Tsinghua University, Baseten, Baidu, AntGroup, Alibaba, Tencent. (Note: an older README revision listed "Jam & Tea Studios" and fewer names; the current list is the one above.) Adopters are self-declared by the project — treat the *production* nature of each as [vendor-reported]/[unverified] unless independently confirmed.

### RL / post-training role
SGLang is a "proven rollout backend used for training many frontier models", with native RL integrations in **AReaL, Miles, slime, Tunix, verl** [official, README, 2026-09-22]. A ByteDance-iaas fork README (crawled 2026-09-18) mirrors the same list [secondary](https://github.com/bytedance-iaas/sglang/blob/HEAD/README.md). A Jan-2026 research note described SGLang as an "RL/post-training rollout backend: verl, AReaL, slime, Tunix, Miles" [secondary](https://github.com/allanschramm/local-model-autotuning/blob/HEAD/docs/discovery/sglang-inference-engine.md).

---

## 2. 2026 release timeline (official GitHub releases)

All dates below are `published_at` from the GitHub Releases API [official](https://api.github.com/repos/sgl-project/sglang/releases?per_page=20), fetched 2026-09-22.

| Version | Release date | Size | Headline |
|---|---|---|---|
| **v0.5.20** | **2026-09-18** | 713 PRs, 237 contributors | RL sampling masks, SGLang Simulator, CUDA 12 lane retired, XPU/MUSA release images |
| v0.5.19 | 2026-09-05 | 786 PRs, 214 contributors | Beam search, DeepEP v2, LayerNorm SP, unified radix tree by default, Lean attention on AMD |
| v0.5.18 | 2026-08-22 | 710 PRs, 212 contributors | (highlights not pulled in this pass — see uncertainties) |
| v0.5.17 | 2026-08-08 | 582 PRs, 194 contributors | **Kimi K3 day-0 support** |
| v0.5.16 | 2026-07-25 | 574 PRs, 169 contributors | DSpark confidence-driven speculative decoding |
| v0.5.15.post1 | 2026-07-14 | patch | GLM-5.2 / DSA launch fixes |
| v0.5.15 | 2026-07-10 | — | GLM-5.2 NVFP4 production tuning on Blackwell |
| v0.5.14 | 2026-06-26 | — | New models: GLM-5.2, LFM2.5, Kimi-K2.7-Code |
| v0.5.13 | 2026-06-13 | — | Nemotron 3 Ultra day-0 support |
| v0.5.12.post1 | 2026-05-26 | stability patch | 12 cherry-picked fixes, mostly DeepSeek-V4 |
| v0.5.12 | 2026-05-16 | — | **DeepSeek-V4 day-0 support** |
| v0.5.11 | 2026-05-05 | — | **CUDA 13 + Torch 2.11** become default; transformers upgrade path |
| v0.5.10.post1 | 2026-04-09 | patch | FlashInfer 0.6.7.post2 → 0.6.7.post3 (JIT cubin downloader fix) |
| v0.5.10 | 2026-04-06 | — | Piecewise CUDA graph default; Elastic NIXL-EP; HiSparse; FlashInfer MXFP8 kernels |
| v0.5.10rc0 | 2026-03-28 | prerelease | v0.5.10 release candidate |
| v0.5.9 | 2026-02-24 | — | LoRA weight-load/compute overlap (−78% TTFT) |
| v0.5.8 | 2026-01-23 | — | Diffusion speedups (up to 1.5× across major diffusion models) |
| v0.5.7 | 2026-01-01 | — | Day-0: Mimo-V2-Flash; other new models |
| gateway-v0.3.1 | ~2026-01-09 [secondary] | — | SGLang Model Gateway: 10–12× faster cache-aware routing, 99% less memory |

Notes:
- v0.5.7 was published 2026-01-01T10:01:57Z [official]. The v0.5.0–v0.5.6 series dates to late 2025 and is out of this report's scope; not pulled.
- Cadence in 2026 is roughly one minor release every 2–4 weeks, with `.post1` stability patches between.
- "gateway-v0.3.1" date (2026-01-09) comes from a [secondary] research note, not the releases API — the release notes themselves say "SMG v0.3.1 Released!" with "10-12x performance improvement and 99% memory reduction in cache-aware routing" [official](https://github.com/sgl-project/sglang/releases/tag/gateway-v0.3.1).

### Per-release detail

**v0.5.20 (2026-09-18)** [official](https://github.com/sgl-project/sglang/releases/tag/v0.5.20) — 713 PRs / 237 contributors.
- **Sampling masks for RL rollouts:** new `return_sampling_mask` — each decode step returns the exact token support and sampled-token log-prob so a trainer can replay rollouts without reconstructing top-k/top-p. Runs under overlap scheduling: +17% decode throughput at batch 1, +52% at batch 64 on Qwen3-8B vs previous implementation. Capacity via `--sampling-mask-max-tokens` (default 4096).
- **SGLang Simulator:** CPU-only simulator running the real scheduler, radix cache and hierarchical cache with a latency predictor instead of model forward. Predicts TTFT within ~6% on most traces (up to 10% on 32K–128K traces); prefix reuse within 0.05 pp. For cache/scheduling studies without GPUs.
- **Unified radix tree — branching-point caching for SWA:** keeps sliding-window state at fork points; on DeepSeek-V4-Flash with shared system prompt, token hit rate 43.8% → 60.8%, mean TTFT 1.57 s → 1.07 s. Opt-in external linker addressing a shared global memory pool via Mooncake or UMBP.
- **DSpark under PD + decode context parallelism:** DCP1 prefill can transfer DSpark draft KV to DCP-N decode; verified on 8×B300 over NIXL and Mooncake up to 256K input.
- **Responses API storage is opt-in** (`--enable-response-store`); without it retrieval/`previous_response_id`/background requests return 400; cannot be enabled in PD deployments.
- **Prefill context parallelism v1 removed** (breaking): strategy-based implementation is the only prefill CP path; CP on HIP/NPU/MUSA rejected until ported.
- **Faster model loading on ROCm:** staged pageable H2D copies; GLM-5.2 TP4 on 4×MI355X loads in 40.4 s vs 505.7 s before.
- **DeepSeek-V4 on Blackwell:** TRT-LLM attention kernels cover DeepSeek-V4 CSA/HCA layers on SM100/SM103 — ~1.2× prefill, ~1.45× decode vs FlashMLA at kernel level on B200. New `--moe-runner-backend flashinfer_megamoe` (+11.9% prefill throughput at 8192 tokens/rank on DeepSeek-V4-Flash NVFP4 TP4/DP4).
- **DeepSeek-V4 on RTX PRO 6000 (SM120):** sparse-MLA indexer on DeepGEMM paged-MQA kernel + DeepGEMM FP4 MoE backend replaces torch fallback; decode TPOT 36.1 → 10.5 ms at batch 1 on 4× RTX PRO 6000; TTFT −20% from 8K→128K input.
- **BREAKING / upgrade notes:** CUDA 12 lane retired (v0.5.19 last with `-cu12x` wheels/images); cutlass_mla, non-Marlin GPTQ, AWQ AOT kernel and Dual Chunk Flash Attention deleted in the quantization refactor; prefill CP v1 removed; `/v1/responses` storage opt-in. New images: ROCm 10 for MI30x/MI35x, gfx1151 (Strix Halo / Ryzen AI MAX+), **Moore Threads MUSA**. ROCm 7.0 CI/images/wheel retired. Intel XPU joins tagged release images (`lmsysorg/sglang:vX.Y.Z-xpu`).
- New models: GLM-5.3-Flash, Hy4-Preview, Qwen3.8-Flash-Next, K2 Horizon, Nanbeige4.2 (autoregressive); SenseNova-U1.5-8B-MoT, FastH3, VDN-H3 (diffusion).
- Dependencies: sglang-kernel 0.4.7, sgl-deep-gemm 0.2.0.

**v0.5.19 (2026-09-05)** [official](https://github.com/sgl-project/sglang/releases/tag/v0.5.19) — 786 PRs / 214 contributors.
- **Beam search** (`beam_width` param, returns n best sequences; does not yet mix with speculative decoding, disaggregation, DP attention, or HiCache).
- **DeepEP v2** (`--moe-a2a-backend deepep_v2`, ElasticBuffer; fixed-size buffers → decode under CUDA graphs even across nodes; for DeepSeek-V3/V4 and Qwen3-MoE FP8).
- **LayerNorm sequence parallelism** (`--enable-layernorm-sp`; −3.5% Qwen3-8B prefill on H100, −5.6% on B200; dense Qwen3 only).
- **W4A8 MoE on Hopper** (`--flashinfer-mxfp4-moe-precision fp8`; +12% output throughput on DeepSeek-V4-Flash, no GSM8K change; needs FlashInfer 0.6.18).
- **DCP on default Blackwell MLA backend** (`trtllm_mla`; at 128K input, plain TP stalls ~680 tok/s on 8×B200 while DCP keeps scaling with concurrency).
- **Faster speculative kernels:** DSA prefill top-k v2 kernel (1.3–1.8× on B200); KDA fused accept path (`SGLANG_OPT_KDA_FUSED_ACCEPT_STATE=1`, MTP verify-and-commit −45% to −63% on Kimi-Linear shapes, bit-identical); DFlash2 (3.43× over no-spec at batch 1, +24% over DFlash at concurrency 64).
- **Unified radix tree now default for every configuration** (breaking; `SGLANG_ENABLE_UNIFIED_RADIX_TREE` deprecated).
- **Lean attention on AMD:** persistent work-centric decode kernel; up to 1.52× throughput, 3.62× lower median ITL on MI355X; auto-enabled where it helps.
- **DSA models on ROCm:** GLM-5.2 PD decode TPOT 23.16 ms → 7.94 ms on 8×MI355X; DeepSeek-V4 top-k kernel up to 3×.
- Dependencies: FlashInfer 0.6.18, sgl-deep-ep 0.1.2, sgl-deep-gemm 0.1.7, mooncake 0.3.13; new CUDA 13.4 preview image for **Rubin**; ROCm 10 images for gfx942/gfx950/gfx1250.
- **BREAKING:** unified radix tree default; Spark3 → Spark2.5 rename (config `spark2_5`, tool-call parser `spark` → `spark25`); DeepSeek-V4 FP4 checkpoints default to FlashInfer MXFP4 MoE runner on SM90/SM100/SM120; W4A4 MegaMoE behind `--enable-w4a4-megamoe`; FlashInfer 0.6.18 required; `ServerArgs` construction no longer resolves (must call `resolve_once()`); requests limited to 32 stop strings / 32 stop regexes (HTTP 400 beyond); XPU `SGLANG_USE_SGL_XPU` flag removed; `/rerun-stage` removed, `/rerun-test` gated on commenter trust.
- Notable fix: `/v1/chat/completions` `usage.reasoning_tokens` was always 0 for thinking models — fixed in v0.5.10 cycle (PR #15562, merged 2026-04-04) [official release notes; confirmed fixed by downstream tracker](https://github.com/hitechcloud-vietnam/dgxarley/blob/HEAD/SGLANG_v0.5.10_VERSION_CHANGES.md).

**v0.5.18 (2026-08-22)** [official](https://github.com/sgl-project/sglang/releases/tag/v0.5.18) — 710 PRs / 212 contributors. Highlights body was not pulled in this research pass ([unverified] detail — see §12).
- Downstream note: v0.5.18 pinned by a community RTX-PRO-6000 build doc as the latest PyPI release in Aug 2026 (Python ≥ 3.10) [secondary](https://github.com/allanschramm/local-model-autotuning/blob/HEAD/docs/discovery/sglang-inference-engine.md).

**v0.5.17 (2026-08-08)** [official] — 582 PRs / 194 contributors. **Kimi K3 day-0 support**: described in release notes as "a 2.8T-parameter multimodal LatentMoE (896 experts, top-16, routed in a …" (release body truncated in API fetch; full detail [unverified]).

**v0.5.16 (2026-07-25)** [official] — 574 PRs / 169 contributors.
- **DSpark: confidence-driven speculative decoding** (new spec algorithm).
- NVFP4 MoE: buggy `cutlass` backend path removed; `triton` and `flashinfer_cutlass` remain the viable NVFP4 MoE backends [official; confirmed by downstream source review 2026-07-28](https://github.com/vroomfondel/dgxarley/blob/HEAD/SGLANG_TP_EP_MOE_UPSTREAM_BUG.md).

**v0.5.15 (2026-07-10)**: **GLM-5.2 NVFP4 tuned for production** on Blackwell [official].

**v0.5.14 (2026-06-26)**: new models GLM-5.2, LiquidAI LFM2.5, Kimi-K2.7-Code [official].

**v0.5.13 (2026-06-13)**: **Nemotron 3 Ultra day-0** (+ blog) [official].

**v0.5.12 (2026-05-16)**: **DeepSeek-V4 day-0 support** — full inference path incl. tensor/data/expert parallelism (#23882) [official]; v0.5.12.post1 (2026-05-26) cherry-picks 12 DeepSeek-V4 fixes [official].

**v0.5.11 (2026-05-05)**: default CUDA moves to 13.0 (from 12.x), PyTorch 2.9 → 2.11 across SGLang/sgl-kernel/Docker images [official].

**v0.5.10 (2026-04-06)** [official] (details via releases-page crawl):
- **Piecewise CUDA Graph enabled by default** (lower memory overhead, better throughput for complex control flow) — #16331.
- **Elastic EP (NIXL-EP) for partial failure tolerance** for DeepSeek MoE: a failed GPU's expert weights are redistributed, serving continues without full restart.
- **GPU staging buffer for PD disaggregation**: contiguous RDMA bulk transfer; ~1000× fewer RDMA requests on GQA models; ~5× TPS/GPU at large concurrency (Qwen3.5, prefill TP4 + decode DEP4).
- **HiSparse sparse-attention backend** for long context.
- **FlashInfer MXFP8 kernels** (GEMM + MoE) for mixed-precision FP8.
- **Transformers 4.57.1 → 5.3.0** major upgrade.
- SGLang-Diffusion updates: LTX-2, Hunyuan3D-2, Helios models; Qwen-Image/Z-Image +1.5×; macOS platform; Cache-DiT integration into diffusers backend.

**v0.5.9 (2026-02-24)**: **LoRA weight loading overlapped with computation** — TTFT −~78% when swapping LoRA adapters (#15363) [official].

**v0.5.8 (2026-01-23)**: diffusion models up to 1.5× faster (ties to lmsys.org/blog/2026-01-16-sglang-diffusion) [official].

**v0.5.7 (2026-01-01)**: day-0 Mimo-V2-Flash (#15207, blog 2025-12-16) and other new models [official].

---

## 3. Architecture

### 3.1 RadixAttention / prefix caching (the signature feature)
- KV cache organized as a **radix tree (trie) over token sequences**; prefixes shared across requests are computed once and reused **automatically** — no explicit prompt-caching API needed [official, lmsys.org/blog/2024-01-17-sglang; paper arXiv:2312.07104](https://github.com/sgl-project/sglang). Benefits: multi-turn agent sessions, shared system prompts, RAG context blocks, few-shot prefixes.
- Reported gains: **up to 6.4× throughput** and **3.7× lower latency** vs vLLM/LMQL baselines on prefix-heavy workloads; KV hit rates ~50–99% [secondary, MarkTechPost 2025-11-07](https://www.marktechpost.com/2025/11/07/comparing-the-top-6-inference-runtimes-for-llm-serving-in-2025/). Original 2024 claim: up to **5× faster inference** with RadixAttention [official, lmsys.org/blog/2024-01-17-sglang/].
- **Eviction:** LRU over the radix tree. `UnifiedRadixCache` variant adds session-aware eviction (`SGLANG_ENABLE_UNIFIED_RADIX_TREE=1`): KV registered to an active `session_id` is evicted only after unreferenced KV (soft references, not pins) [secondary, docs reference](https://github.com/allanschramm/local-model-autotuning/blob/HEAD/docs/discovery/sglang-inference-engine.md).
- **2026 evolution — unified radix tree:** became the default cache for *every* configuration in **v0.5.19 (2026-09-05)** [official]. In **v0.5.20 (2026-09-18)**: branching-point caching for the SWA component (DeepSeek-V4-Flash: token hit rate 43.8% → 60.8%, mean TTFT 1.57 s → 1.07 s with shared system prompt); opt-in external linker to a shared global memory pool via Mooncake or UMBP; PD decode workers reuse cached prefixes for SWA hybrid models (e.g. gpt-oss); runtime attach/detach of L3 storage; PP + HiCache L3 consistency across ranks [official].
- Cache-aware scheduling: "cache-aware sglang router" improvements shipped in v0.4.1 (2025); SMG v0.3.1 (Jan 2026) reworked cache-aware routing: 216,000 cache insertions/s (was 18,900), 4.6 µs/op (was 52.9 µs), ~99% memory reduction per tree node (~180 KB → ~1.4 KB) [official](https://github.com/sgl-project/sglang/releases/tag/gateway-v0.3.1).

### 3.2 HiCache — hierarchical KV cache
- 3-tier KV cache: GPU memory → host RAM → external storage (HF3FS, Mooncake). Flags: `--enable-hierarchical-cache --hicache-ratio 2 --hicache-size 100 --hicache-write-policy write_through` [secondary, docs reference](https://github.com/allanschramm/local-model-autotuning/blob/HEAD/docs/discovery/sglang-inference-engine.md).
- v0.5.19: buffer-only mode for host-memory layer; batch PP write/load completion sync; −13% allocation time (no double mmap populate); L3 prefetch retry [official].
- v0.5.20: v0.5.19's mxfp8 KV cache support in PD transfer + CPU offload [official].

### 3.3 Parallelism
- Tensor / pipeline / expert / data parallelism, plus **DP attention** and **context parallelism** (prefill CP v1 removed in v0.5.20; strategy-based prefill CP is now the only path; **decode context parallelism (DCP)** runs on `trtllm_mla` since v0.5.19) [official].
- **DeepEP / expert parallelism:** DeepEP classic backend; **DeepEP v2 (ElasticBuffer)** added v0.5.19 (`--moe-a2a-backend deepep_v2`) enabling decode under CUDA graphs across nodes; AMD **Mori-EP / MoRI** backend for MI300X/MI355X (Kimi-K3, DeepSeek-V4, GLM-5.2) [official]. **Elastic NIXL-EP** for partial failure tolerance (v0.5.10) [official]. Large-scale EP milestone (May 2025): 52.3K in-tok/s, 22.3K out-tok/s on 96 GPUs — 5× faster than vanilla TP [secondary](https://github.com/semianalysisai/inferencex-app) / [official blog 2025-05-05].
- **LayerNorm sequence parallelism** (`--enable-layernorm-sp`, v0.5.19): −3.5% prefill (H100) / −5.6% (B200) on Qwen3-8B; dense Qwen3 only [official].
- **W4A4 MegaMoE** server flag (`--enable-w4a4-megamoe`, v0.5.19); FlashInfer MegaMoE runner (`flashinfer_megamoe`, v0.5.20, +11.9% prefill throughput on DeepSeek-V4-Flash NVFP4) [official].

### 3.4 Disaggregated prefill/decode (PD) + Mooncake
- PD disaggregation is a first-class, production feature: DeepSeek's production prefill-decode disaggregation was reproduced in SGLang [secondary](https://particula.tech/blog/sglang-vs-vllm-inference-engine-comparison).
- v0.5.10: GPU staging buffer gathers scattered head slices into contiguous memory for bulk RDMA — ~1000× fewer RDMA requests on GQA, ~5× TPS/GPU at high concurrency [official].
- v0.5.19: PD transfer time −28% to −39% (Kimi-Linear-48B, 8×B300) via packed dest-contiguous RDMA blocks; deferred decode-side KV release for aborts mid-transfer (incl. NIXL backend); mxfp8 KV cache in PD transfer [official].
- v0.5.20: DCP1→DCP-N relayouts transfer **DSpark draft KV** (DSpark under PD with decode context parallelism, verified 8×B300 over NIXL and Mooncake to 256K input); new EPD refactor [official].
- **Mooncake:** dependency `mooncake==0.3.13` (v0.5.19) [official]; MooncakeStore as L3/external storage tier for HiCache; Mooncake MoE A2A backend; Mooncake backend for the unified-cache external linker (v0.5.20) [official]. A downstream doc describes HiCache's third tier as "HF3FS, Mooncake" [secondary].
- GB200 NVL72 blogs (official, lmsys.org): Part I (2025-06-16) 2.7× higher decoding throughput; Part II (2025-09-25) 3.8× prefill / 4.8× decode throughput with PD + large-scale EP [official]. GB300 NVL72 blog (2026-02-19): "Unlocking 25x Inference Performance with SGLang on NVIDIA GB300 NVL72" [official]. GB300 long-context work continued into 2026.

### 3.5 Speculative decoding
- Algorithms supported: **EAGLE** (incl. EAGLE3-generation work), **DFlash / DFlash2** ("next generation of speculative decoding", blog 2026-06), **DSpark** (confidence-driven, new in v0.5.16), **KDA**, **NEXTN**, **NGRAM**, **MTP** (multi-token prediction), **Standalone** draft models, adaptive speculative decoding [official].
- Reported wins: DeepSeek models via EAGLE MTP — 1.8× decode speedup at batch 1, 1.5× at batch 32 on H200 [secondary](https://particula.tech/blog/sglang-vs-vllm-inference-engine-comparison); DFlash2 3.43× over no-spec at batch 1, +24% over DFlash at concurrency 64 (v0.5.19) [official]; NEXTN 1.35–1.76× over no-spec on AMD after GQA packing fix (v0.5.19) [official]; LFM2/LFM2-MoE DSpark 1.05–2.42× faster decoding (1×H100) [official]; FR-Spec on Flash-Next (community, Sept 2026): 65,536-token draft vocabulary [secondary](https://github.com/jpezzulli/sglang-rtxpro6000/blob/HEAD/RESULTS.md).
- **Beam search** added v0.5.19 (`beam_width`) — distinct from speculative decoding, does not yet compose with it [official].
- Compatibility caveats [secondary, LocalAI docs 2026]: DFLASH and NGRAM incompatible with `enable_dp_attention`; DFLASH requires `pp_size == 1`; STANDALONE incompatible with `enable_dp_attention`; NGRAM is CUDA-only and disables the overlap scheduler.

### 3.6 CUDA graphs
- **Piecewise & breakable CUDA graphs** are the default execution mode since v0.5.10 [official]; v0.5.19 adds PP prefill CUDA graphs (up to 2.48× at 2K-token forwards on Qwen3.5-397B, GB300), DP-attention coordination, −18.9% median TTFT / +12.9% QPS from reduced idle DP work [official]; v0.5.20 sizes the graph pool from warmup measurements and reuses output storage across full prefill graphs [official].

### 3.7 Attention backends
- FlashAttention-3, **FlashInfer** (0.6.18 required since v0.5.19; CuTe DSL; `flashinfer_trtllm` mxfp8 GEMM), FlashMLA / CutlassMLA / Tokenspeed MLA, Triton, **HiSparse** (sparse attention, v0.5.10), **fmha_v2** for SM90/120 (~15% faster than FA3 at kernel level, v0.5.19), TRT-LLM MLA/MHA decode, DeepGEMM, AITER (AMD), TileLang-backed MLA path [official].
- MLA optimization and DP attention were in place before v0.4.1; SGLang claims 3.1× faster DeepSeek-V3 inference vs vLLM via optimized MLA backends [secondary](https://particula.tech/blog/sglang-vs-vllm-inference-engine-comparison) / [official v0.4.1 notes](https://github.com/sgl-project/sglang/releases/tag/v0.4.1).
- **DeepSeek Sparse Attention (DSA)**: first-class; DSA prefill top-k v2 kernel 1.3–1.8× on B200 (v0.5.19); Q8KV8 sparse MLA prefill runtime for DeepSeek-V4 (+4.4–7.5% prefill throughput, H20) [official].

### 3.8 Scheduler & runtime
- Zero-overhead batch scheduler (v0.4, Dec 2024) [official]; continuous batching, chunked prefill (mixed chunk prefill base, v0.5.19), paged attention, overlap scheduling, per-scheduler load published on a dedicated socket for load-aware routers (v0.5.19) [official].
- Rust server (`SGLANG_RUST_SERVER=1`): process-local in-memory KV indexer + Router integration, e2e latency metadata, configurable HTTP/2 window (v0.5.19) [official].
- Default serving port **30000** (vLLM: 8000); Prometheus metrics under `sglang:*` prefix (requires `--enable-metrics`) [secondary](https://github.com/fuzzifikation/vllm-copilot/blob/HEAD/docs/sglang-compat-plan.md).

---

## 4. Quantization, model formats & architectures

- **Supported (README):** FP4 / FP8 / INT4 / AWQ / GPTQ [official]. v0.5.19 release notes additionally exercise: **NVFP4, MXFP4, MXFP8, W4A8, W4A4 (MegaMoE), FP8 KV cache, mxfp8 KV cache, compressed-tensors** (quantized lm_head, kv_cache_scheme scales), torchao integration, ModelSlim (Kimi-K3) [official].
- **Marlin:** FP4 Marlin helpers were deduped in the v0.5.20 quantization refactor; the old **AWQ AOT kernel and non-Marlin GPTQ were deleted** in v0.5.20 — Marlin remains the path for GPTQ/AWQ-style INT4 [official]. Community config translation confirms `--quantization gptq_marlin` is auto-detected [secondary](https://github.com/randomchaos7800-hub/inference-research/blob/HEAD/tower/gdn-blackwell/sglang-vs-vllm-sm120.md).
- **NVFP4 MoE backends:** as of v0.5.16, the buggy `cutlass` FP4 path was deleted; `triton` and `flashinfer_cutlass` are the viable backends; FlashInfer CuTe DSL NVFP4 W4A16 mode added v0.5.19 (Qwen3-30B-A3B W4A16 GSM8K 0.965 → 0.980) [official].
- **FP8:** FP8 KV cache; FP8 DeepEP dispatch (Humming backend: TPOT −35%, TTFT −23% vs BF16 on H20, v0.5.19); FP8 block-scale MoE keeps fp32 routing weights (v0.5.20) [official].
- **Model coverage:** Llama, Qwen (incl. Qwen3.x, Qwen3.5/3.6/3.8, Qwen3-VL), DeepSeek (V3/R1/V4 + MLA/DSA/MTP), Kimi (K2/K3/Linear), GLM (4.5/4.6/5.x, incl. GLM-5.2/5.3-Flash NVFP4), GPT (gpt-oss day-0 Aug 2025), Gemma (incl. Gemma-4 FP8 MTP), Mistral (incl. Mistral Large 3, Ministral), Nemotron 3 (Nano/Super/Ultra/Lightning), LFM2/2.5, MiniMax (M2/H3/M3, incl. Music 3), Spark2.5, Granite 4.2, LongCat, Ling-3.0, dots3.note, MiniCPM (incl. MiniCPM5/SALA), Intern-S2-Mobius, Mamba-hybrid models (Qwen3.8-Next, Nemotron-H, Kimi-Linear), embedding models (e5-mistral, gte, mcdse), reward models (Skywork), diffusion models (WAN, Qwen-Image, FLUX.2, HunyuanVideo, LTX-2.5, SANA-Video, Cosmos3, MiniMax-H3, LongCat-Image, SenseNova-U1.5-8B-MoT) [official, across release notes + README].
- **Model formats:** Hugging Face checkpoints (safetensors), GGUF (diffusion transformer checkpoints, MiniMax-H3, v0.5.19), ComfyUI serialized checkpoints (NVFP4/W4A8/INT8 DiTs), pruned safetensors [official]. Transformers upgraded 4.57.1 → 5.3.0 in v0.5.10 [official].

---

## 5. New 2026 features (beyond §2)

- **Structured outputs / grammar-constrained generation:** XGrammar integration since v0.4 (Dec 2024, "Faster Structured Outputs") [official, lmsys.org/blog/2024-12-04-sglang-v0-4/]; compressed-FSM JSON decoding claimed 3× faster (Feb 2024 blog) [official]. v0.5.19: MistralCommon tokenizer support in the XGrammar backend [official]. Structured-generation frontend (`sgl.function`, `sgl.gen`, choices/regex/JSON-schema) is the project's original 2023–2024 DSL [official README "Frontend Tutorial"]. Agent-workload claim: <400 ms p50 for schema-constrained hops [secondary](https://particula.tech/blog/sglang-vs-vllm-inference-engine-comparison).
- **Reasoning / multi-step:** the SGLang **frontend language** includes a program interpreter, few-shot generation primitives, and tool/chain composition (the "Structured Generation Language" of its name) [official README]; `--reasoning-parser` emits `delta.reasoning_content` in chat completions [secondary]; `reasoning_tokens` usage accounting fixed v0.5.10 [official]. For RL: `return_sampling_mask` (v0.5.20) and `DisallowedTokensLogitsProcessor` support [official].
- **Multi-modal:** LLaVA-OneVision multi-image/video (v0.3), Pixtral multi-image, Qwen3-VL MoE, PaddleOCR-VL (1080p page TTFT 235 ms → 120 ms on H200, v0.5.19), ASR/audio/speech fixes, `input_audio` content part in chat completions (v0.5.19) [official]. **SGLang-Omni** (sister repo): audio/TTS/ASR serving framework — v0.1.6 on PyPI (Sept 2026), day-0 AuK/AuK-Flash and MiniMax Music 3, MOSS-TTS, Higgs Audio v3 [official, repo news](https://github.com/sgl-project/sglang-omni).
- **SGLang-Diffusion:** image/video generation serving (WAN, Qwen-Image, FLUX.2, HunyuanVideo, LTX-2.5, Cosmos3, MiniMax-H3, SANA-Video, LongCat); up to 2.5× faster than its Nov-2025 initial release by Jan 2026; up to 1.5× across major diffusion models in v0.5.8 [official, lmsys.org/blog/2026-01-16-sglang-diffusion/].
- **DeepEP / MoE expert parallelism:** see §3.3. SGLang was "first opensource implementation of DeepSeek V3/R1 expert parallelism with PD disaggregation" (May 2025) [secondary].
- **CUDA graphs:** see §3.6.
- **FlashInfer attention backend:** required 0.6.18 since v0.5.19; powers MXFP4/MXFP8 MoE, DSA top-k, NVFP4 W4A16, MegaMoE [official].
- **Multi-LoRA:** multi-adapter LoRA batching incl. with EAGLE/NEXTN/DFLASH/DSPARK speculative decoding (v0.5.19); LoRA weight-load overlap −78% TTFT (v0.5.9) [official].
- **Embeddings/rerank/reward models** supported (e5-mistral, gte, mcdse, Skywork) [official].

---

## 6. Performance benchmarks

### 6.1 Official (project-published) numbers
- **v0.2 blog (2024-07-25):** faster Llama-3 serving vs TensorRT-LLM and vLLM [official].
- **v0.3 blog (2024-09-04):** 7× faster DeepSeek MLA, 1.5× faster torch.compile [official].
- **v0.4 blog (2024-12-04):** zero-overhead scheduler, cache-aware load balancer, faster structured outputs [official].
- **Large-scale EP (2025-05-05):** 52.3K in-tok/s + 22.3K out-tok/s on 96 GPUs, 5× vs vanilla TP [official].
- **GB200 NVL72 Part I (2025-06-16):** 2.7× higher decoding throughput; **Part II (2025-09-25):** 3.8× prefill, 4.8× decode throughput (PD + large-scale EP) [official].
- **GB300 NVL72 (2026-02-19):** "Unlocking 25x Inference Performance with SGLang on NVIDIA GB300 NVL72" [official].
- **GLM-5.2 agentic workloads (2026-07 blog):** 500 TPS in two weeks on NVFP4 [official].
- Per-release kernel numbers are in §2 (e.g., v0.5.19: Cohere Command-A-Plus decode 2.28× at batch 1, up to 9,152 tok/s on 4×GB300; v0.5.20: DCP1→DCP-N verified 8×B300 to 256K).

### 6.2 Independent measurements
- **SemiAnalysis InferenceX — Qwen3.5-397B-A17B FP8 on MI355X** (published ~2026-05, runs 2026-02-20 → 2026-05-19): throughput/GPU on 8k/1k workload went **192 → 3,660 tok/s/GPU (19.0× at iso-interactivity 40 tok/s/user)** across three SGLang releases (v0.5.8.post1 → v0.5.10rc0 → v0.5.12) plus three AITER MoE kernel landings; another ~1.5× from the May image bump [independent](https://github.com/semianalysisai/inferencex-app/blob/HEAD/packages/app/content/blog/mi355x-qwen3-5-sglang-v0-5-12-up-to-17x.mdx). (Note: one related InferenceX post wrote "SGLang v0.12" for its GLM-5 run — likely a typo; treated as [unverified] version label.)
- **SemiAnalysis InferenceX — GLM-5 FP8 cost (2026-05-20 run):** MI355X+SGLang undercuts B200+SGLang on $/M tokens by up to **1.41× at 18 tok/s/user with MTP ($0.30/M vs $0.22/M — a ~40% reduction)** and 1.36× without MTP; B200 noses back ahead above ~90 tok/s/user [independent](https://github.com/semianalysisai/inferencex-app/blob/HEAD/packages/app/content/blog/mi355x-glm5-fp8-sglang-40-cheaper-than-b200.mdx).
- **dstack — DeepSeek-R1 on 8×H200 SXM5:** online (SGLang `bench_serving.py`, concurrencies 4–128, 3200-in/800-out) and offline inference benchmark comparing SGLang vs TensorRT-LLM; full results in their repo [independent](https://github.com/dstackai/dstack/blob/HEAD/docs/blog/posts/h200-mi300x-deepskeek-benchmark.md).

### 6.3 vLLM vs SGLang comparisons (all secondary/independent — methodology varies)
- **PremAI 2026 benchmark (H100 80GB, Llama 3.1 8B):** SGLang ~16,200 tok/s vs vLLM ~12,500 tok/s — **SGLang +29%**; LMDeploy ~16,100 (tie) [secondary](https://blog.premai.io/vllm-vs-sglang-vs-lmdeploy-fastest-llm-inference-engine-in-2026/) via [secondary summary](https://github.com/profsynapse/synaptic-tuner/blob/HEAD/docs/preparation/vllm-vs-sglang-inference-serving-research.md).
- **Spheron (Llama 3.3 70B FP8, H100):** SGLang +29% total throughput, **+117% output-token throughput** (894 vs 413 tok/s), TTFT 79 vs 103 ms (−23%), ITL 6.0 vs 7.1 ms (−15%) [secondary](https://particula.tech/blog/sglang-vs-vllm-inference-engine-comparison). Same source: with **unique prompts (no shared prefixes) the gap shrinks to near-zero** — concurrency 1→100 shows only +2–5%.
- **Community single-request test (old, ~2025-06):** vLLM 60.0 tok/s vs SGLang 52.7 tok/s on a single unique prompt — vLLM ~1.1× faster [secondary](https://github.com/brendanmckeag/sglang-vllm-benchmark/blob/HEAD/README.md). Dated; treat as historical.
- **DeepSeek-specific:** SGLang claimed **3.1× faster than vLLM on DeepSeek-V3** via optimized MLA backends (FA3/FlashInfer/FlashMLA/CutlassMLA) [secondary](https://particula.tech/blog/sglang-vs-vllm-inference-engine-comparison).
- **Consumer GPUs (RTX PRO 6000, Sept 2026, community):** Qwen3.8-27B NVFP4 + DSpark: 119.19 tok/s output, 1,072.69 tok/s total (8K-in/1K-out, 8 reqs), mean TTFT 729.82 ms, mean TPOT 7.68 ms, DSpark accept length 2.50 [secondary/independent](https://github.com/lEWFkRAD/qwen38-rtx-pro-6000).
- **Artificial Analysis:** no Artificial-Analysis-published SGLang-vs-vLLM engine benchmark was found in this research pass ([unverified]/not found). SemiAnalysis InferenceX (above) is the closest independent price/performance source.
- **ServeTheHome:** no STH SGLang benchmark article was surfaced in this pass ([unverified]/not found).
- **SiliconANGLE:** no SiliconANGLE SGLang article was surfaced in this pass ([unverified]/not found).

### 6.4 Caveats
- The +29% SGLang-vs-vLLM gap is a **high-concurrency batching** result; single-stream unique-prompt workloads show ~0–12% either way. RadixAttention's advantage concentrates in prefix-sharing workloads (agents, RAG, multi-turn chat) [secondary].
- One community SM120 report (Aug 2026) flagged garbage output with INT4 quantized models on SGLang (issue #21132) and recommended vLLM for that config; upstream fixed NVFP4 paths in v0.5.16+ and improved SM120 support in v0.5.19/0.5.20 — current status [unverified] [secondary](https://github.com/randomchaos7800-hub/inference-research/blob/HEAD/tower/gdn-blackwell/sglang-vs-vllm-sm120.md).

---

## 7. Deployment

- **Docker:** official images `lmsysorg/sglang` on Docker Hub [official, README install docs]. v0.5.20 image matrix: CUDA 13.x (CUDA 12 lane retired after v0.5.19), CUDA 13.4 preview for **Rubin**, ROCm 10 (gfx942/gfx950/gfx1250, MI30x/MI35x), Intel XPU (`lmsysorg/sglang:vX.Y.Z-xpu`), gfx1151 (Strix Halo / Ryzen AI MAX+), Moore Threads MUSA [official].
- **PyPI:** `pip install sglang` (PyPI release cadence tracks GitHub tags; v0.5.18 was latest PyPI release in Aug 2026 per downstream doc; Python ≥ 3.10) [secondary]. Jetson (l4t) builds tracked separately [secondary].
- **Kubernetes:** no SGLang-specific operator found in this pass; the standard route is the **SGLang Model Gateway** (Kubernetes service discovery, multi-model routing) [official docs ref via secondary](https://github.com/hygon-ai/sglang-das/blob/HEAD/docs/docs/advanced_features/sgl_model_gateway.mdx) and the **llm-d** project (Kubernetes-native distributed inference), whose guides ship SGLang inference-server configs "validated each release" on NVIDIA GPU and AMD GPU (GKE + base) [secondary](https://github.com/rohitg00/llm-d/blob/HEAD/guides/pd-disaggregation/README.md).
- **Cloud providers:** adopted/deployed on Oracle Cloud, Google Cloud, Microsoft Azure, AWS (per README adopter list) plus GPU clouds: Nebius, DataCrunch, Novita, RunPod, Voltage Park, Atlas Cloud, InnoMatrix, Modal, Baseten [official README; production depth per provider [unverified]].
- **Hardware backends (2026-09):**
  - **NVIDIA CUDA:** H100/H200/B200/B300/GB200/GB300 NVL72, A100, RTX 5090/PRO 6000, DGX Spark; SM90/100/103/107/120; CUDA 13 default since v0.5.11; Rubin preview (v0.5.19+) [official].
  - **AMD ROCm:** MI300X/MI355X (Lean attention, AITER, MoRI/Mori-EP, ROCm 10 images; MI355X reached CUDA-stack feature parity incl. MTP per InferenceX [independent]); ROCm 7.2.4 → 7.0 retired in v0.5.20 [official].
  - **Intel:** Xeon CPU; XPU (versioned Docker images since v0.5.20; INT4 AWQ/GPTQ dense linear, SYCL kernels for DeepSeek-V4 MHC) [official].
  - **Google TPU:** SGLang-Jax native backend (Oct 2025); **RadixArk + Google brought full SGLang features to TPUs** (blog, 2026-07) [official README news].
  - **Ascend NPU:** NPU backend (DeepSeek-V4, MXFP4-W4A4 MoE quant, MiniMax-H3 diffusion, Kimi-K3 on Ascend A3 cookbook) [official v0.5.19/v0.5.20].
  - **Moore Threads MUSA:** image added v0.5.20 [official].
  - **Apple MLX / macOS:** MLX backend (Torch 2.13/MLX 0.32+, v0.5.19); macOS added as SGLang-Diffusion platform (v0.5.10) [official].
  - **Intel Gaudi (HPU):** initial `--device hpu` support PRs from 2024 (#2121/#2357, offline batch inference on Gaudi2) [official PRs]; but a May-2026 industry guide states **"SGLang: Not supported on Gaudi 3; the RadixAttention scheduler has no HPU backend"** [secondary](https://www.spheron.network/blog/intel-gaudi-3-vs-nvidia-h200-b200-llm-inference-2026/) — treat HPU as experimental, not production-grade.
  - **Disaggregated deployment:** PD disaggregation via Mooncake/NIXL RDMA backends; MooncakeStore/HiCache L3 tiers [official].
- **APIs:** OpenAI-compatible (`/v1/chat/completions`, `/v1/completions`, `/v1/embeddings`, `/v1/audio/speech` in Omni), Anthropic-compatible bits, `/v1/responses` (storage opt-in since v0.5.20), MCP integration in Model Gateway, gRPC support [official/secondary].

---

## 8. Ecosystem

- **SGLang Router:** cache-aware load balancer/router (Rust `sgl-router` + newer Rust server integration); the original v0.4 (Dec 2024) shipped a "cache-aware load balancer" [official]. Note: a third-party compat doc flags the *classic* `sgl-router` as missing `max_model_len` in `/v1/models` and no `/server_info` route — the **SGLang Model Gateway** (`sgl-model-gateway`) proxies to workers and is the supported path [secondary](https://github.com/fuzzifikation/vllm-copilot/blob/HEAD/docs/sglang-compat-plan.md).
- **SGLang Model Gateway (SMG):** high-performance model-routing gateway — load balancing, PD disaggregation, multi-model routing, gRPC, MCP, Kubernetes service discovery, history-store/privacy controls [secondary docs ref](https://github.com/hygon-ai/sglang-das/blob/HEAD/docs/docs/advanced_features/sgl_model_gateway.mdx); gateway-v0.3.1 (Jan 2026) with 10–12× routing speedup [official].
- **Related repos (sgl-project org):** `sglang-omni` (audio/TTS/ASR, v0.1.6, Sept 2026) [official](https://github.com/sgl-project/sglang-omni); `sgl-learning-materials` (slides/adoption); diffusion serving inside the main repo (SGLang-Diffusion). Downstream/community: `bytedance-iaas/sglang` fork, `hygon-ai/sglang-das`, LocalAI SGLang backend, LMCache integration work [secondary].
- **Dependencies (v0.5.20):** FlashInfer 0.6.18, sglang-kernel 0.4.7 (split out as standalone `sgl-kernel` package since v0.4.1), sgl-deep-gemm 0.2.0, sgl-deep-ep 0.1.2, mooncake 0.3.13, TileLang 0.1.12, compressed-tensors 0.18.0, transformers 5.3.0, torch 2.11–2.13, CUDA 13.x [official].
- **Contributors:** v0.5.20: 713 PRs from 237 contributors; v0.5.19: 786 PRs / 214; v0.5.18: 710 / 212 [official]. Release captains seen in API: Qiaolin-Yu, Fridge003, Kangyan-Zhou [official].
- **Sponsors/grants:** a16z Open Source AI Grant, third batch (announced 2025-06, per README news) [official]; PyTorch Ecosystem member (Mar 2025) [official]. Commercial company: **RadixArk** (see §9).

---

## 9. Enterprise support & commercial backers

- **RadixArk — the commercial company behind SGLang** [secondary, TechCrunch 2026-01-21](https://techcrunch.com/2026/01/21/sources-project-sglang-spins-out-as-radixark-with-400m-valuation-as-inference-market-explodes/):
  - Spun out of the Berkeley SGLang team; announced August 2025.
  - Valued at **~$400M** in a round led by **Accel** (TechCrunch, citing two people familiar; round size unconfirmed by TechCrunch).
  - A later summary reports a **$100M seed at $400M post-money (May 2026), led by Accel and Spark Capital, with NVIDIA's NVentures and AMD participating** [secondary](https://www.beri.net/article/vllm-vs-tensorrt-llm-vs-sglang-inference-runtime-2026) — the $100M figure is single-sourced; treat as [unverified].
  - Founders: **Ying Sheng** (CEO; ex-xAI, ex-Databricks research scientist) and **Banghua Zhu** [secondary]. Earlier angel backing incl. Intel CEO Lip-Bu Tan [secondary].
  - Product direction: keep developing SGLang as open-source engine; **Miles** RL framework; **paid hosting services** launched [secondary](https://techfundingnews.com/radixark-sglang-spinoff-400m-valuation-ai-inference/).
  - **TPU partnership:** "RadixArk and Google bring full SGLang features to TPUs" (2026-07) [official README news].
- **Enterprise contact:** `sglang@lmsys.org` for "adopting or deploying SGLang at scale, including technical consulting, sponsorship opportunities, or partnership inquiries" [official README].
- **Known production users (project-stated):** xAI ("default inference engine for xAI" per a 2025 community deck [secondary]), Cursor, LinkedIn, plus the cloud/GPU providers in §1 [official README].
- **Vendor-built platforms on SGLang:** **Atlas Cloud "Atlas Inference"** — press release claims up to 2.1× throughput vs competitors on 12 nodes, sub-5 s TTFT, 100 ms ITL at 10,000+ concurrent sessions, via PD disaggregation + DeepEP + two-batch overlap; SGLang core developer Yineng Zhang quoted [vendor-reported](https://www.newscom.com/accesswire?p_p_id=listenersearchresults_WAR_searchportlet&p_p_lifecycle=0&p_p_state=normal&p_p_mode=view&p_p_col_pos=1&p_p_col_count=2&_listenersearchresults_WAR_searchportlet_pageNumber=1&_listenersearchresults_WAR_searchportlet_struts.portlet.action=%2Fview%2FshowDetail&_listenersearchresults_WAR_searchportlet_tagId=awire192676).
- **Security posture (enterprise-relevant caveat):** August-2026 CERT/CC-coordinated disclosure of **six SGLang vulnerabilities** (pickle-based IPC in expert-backup path — mitigation `SGLANG_USE_PICKLE_IPC=false`; opt-in dumper `DUMPER_SERVER_PORT`; unauthenticated `/server_info`; NCCL weight-broadcast exfiltration; `update_weights_from_disk`/`update_weights_from_huggingface` fetching; mitigations: API key, network isolation, HF repo allowlisting) [secondary](https://github.com/wrg-11/wrg-sigma-rules/blob/HEAD/docs/detection-notes/sglang-2026-08-disclosure-series-detection-2026-09-04.md). A Sept-2026 comparison notes SGLang's repo had **no SECURITY.md** and no documented triage/disclosure process, vs vLLM's PyTorch-Foundation governance [secondary](https://www.beri.net/article/vllm-vs-tensorrt-llm-vs-sglang-inference-runtime-2026). Status of upstream fixes as of 2026-09-22: [unverified] — confirm against current release notes before production sign-off.

---

## 10. Positioning vs vLLM

| Dimension | SGLang | vLLM |
|---|---|---|
| Core scheduling idea | **RadixAttention**: radix-tree prefix KV reuse across requests (automatic) | **PagedAttention**: block-level KV management; prefix caching opt-in (`--enable-prefix-caching`) |
| Sweet spot | Prefix-heavy: agents, multi-turn chat, RAG, few-shot evals (up to 6.4× claimed) | General production, broadest model coverage, most mature ecosystem |
| Raw throughput (H100, Llama 3.1 8B, high concurrency) | ~16,200 tok/s [secondary] | ~12,500 tok/s [secondary] |
| Single-stream unique prompts | Roughly parity to −12% [secondary] | Slightly ahead in one old test [secondary] |
| DeepSeek models | Day-0 support (V3/R1/V4), optimized MLA/DSA/MTP, claimed 3.1× on V3 [official/secondary] | Supported; SGLang is the "officially recommended engine" per one comparison [secondary] |
| Speculative decoding | EAGLE/DFlash2/DSpark/KDA/NEXTN/MTP + beam search (v0.5.19) | EAGLE3 etc. |
| PD disaggregation | First-class (Mooncake/NIXL, DCP) | Available; llm-d covers both |
| Hardware breadth | NVIDIA + AMD (strong) + Intel XPU/CPU + TPU + Ascend + MUSA; HPU experimental | NVIDIA + AMD + TPU + XPU + HPU (Habana fork, mature-ish) |
| Structured output | XGrammar, compressed-FSM JSON (3× claim), frontend DSL | Outlines/XGrammar guidance |
| Governance | LMSYS non-profit + RadixArk commercial spin-out ($400M) | PyTorch Foundation (neutral IP), formal security process; Red Hat sells supported vLLM/llm-d |
| GitHub traction | 36.3K stars / 9.1K forks (2026-09-22) | (not pulled in this pass) |
| Default port | 30000 | 8000 |

**Practitioner summary:** choose SGLang when prefix reuse dominates (agentic/RAG/multi-turn), when serving DeepSeek-family models (day-0 kernels), or when AMD MI300X/MI355X is the fleet (feature parity + strong independent cost numbers in 2026). Choose vLLM for the broadest ecosystem, formal enterprise support channels (e.g., Red Hat), and Gaudi/HPU fleets. The raw-throughput gap is real at high concurrency but collapses on unique-prompt workloads.

---

## 11. Source list

- https://github.com/sgl-project/sglang (README, releases)
- https://api.github.com/repos/sgl-project/sglang (repo stats)
- https://api.github.com/repos/sgl-project/sglang/releases?per_page=20 (release dates)
- https://github.com/sgl-project/sglang/releases/tag/v0.5.20
- https://github.com/sgl-project/sglang/releases/tag/v0.5.19
- https://github.com/sgl-project/sglang/releases/tag/v0.5.18
- https://github.com/sgl-project/sglang/releases/tag/gateway-v0.3.1
- https://github.com/sgl-project/sglang/releases/tag/v0.4.1
- https://github.com/sgl-project/sglang-omni
- https://github.com/sgl-project/sglang/pull/2357 and /pull/2121 (HPU)
- https://lmsys.org/blog/2024-01-17-sglang/ · https://lmsys.org/blog/2024-12-04-sglang-v0-4/ · https://lmsys.org/blog/2026-01-16-sglang-diffusion/ · https://lmsys.org/blog/2026-02-19-gb300-longctx/
- https://techcrunch.com/2026/01/21/sources-project-sglang-spins-out-as-radixark-with-400m-valuation-as-inference-market-explodes/
- https://techfundingnews.com/radixark-sglang-spinoff-400m-valuation-ai-inference/
- https://www.beri.net/article/vllm-vs-tensorrt-llm-vs-sglang-inference-runtime-2026
- https://particula.tech/blog/sglang-vs-vllm-inference-engine-comparison
- https://blog.premai.io/vllm-vs-sglang-vs-lmdeploy-fastest-llm-inference-engine-in-2026/
- https://github.com/semianalysisai/inferencex-app (InferenceX MI355X Qwen3.5 + GLM-5 posts)
- https://github.com/dstackai/dstack (DeepSeek-R1 H200 benchmark)
- https://www.marktechpost.com/2025/11/07/comparing-the-top-6-inference-runtimes-for-llm-serving-in-2025/
- https://www.spheron.network/blog/intel-gaudi-3-vs-nvidia-h200-b200-llm-inference-2026/
- https://github.com/jpezzulli/sglang-rtxpro6000/blob/HEAD/RESULTS.md
- https://github.com/lEWFkRAD/qwen38-rtx-pro-6000
- https://github.com/wrg-11/wrg-sigma-rules/blob/HEAD/docs/detection-notes/sglang-2026-08-disclosure-series-detection-2026-09-04.md
- https://github.com/rohitg00/llm-d/blob/HEAD/guides/pd-disaggregation/README.md
- https://github.com/hygon-ai/sglang-das/blob/HEAD/docs/docs/advanced_features/sgl_model_gateway.mdx
- https://github.com/fuzzifikation/vllm-copilot/blob/HEAD/docs/sglang-compat-plan.md
- https://github.com/vroomfondel/dgxarley/blob/HEAD/SGLANG_TP_EP_MOE_UPSTREAM_BUG.md
- https://github.com/hitechcloud-vietnam/dgxarley/blob/HEAD/SGLANG_v0.5.10_VERSION_CHANGES.md
- https://github.com/allanschramm/local-model-autotuning/blob/HEAD/docs/discovery/sglang-inference-engine.md
- https://github.com/mudler/localai/blob/HEAD/.agents/sglang-backend.md
- https://github.com/profsynapse/synaptic-tuner/blob/HEAD/docs/preparation/vllm-vs-sglang-inference-serving-research.md
- https://github.com/brendanmckeag/sglang-vllm-benchmark/blob/HEAD/README.md
- https://github.com/randomchaos7800-hub/inference-research/blob/HEAD/tower/gdn-blackwell/sglang-vs-vllm-sm120.md

## 12. Open questions / uncertainties

1. **v0.5.18 highlights** — release-notes body not pulled; only size (710 PRs / 212 contributors) confirmed [official].
2. **v0.5.17 Kimi-K3 details** — release body truncated in API fetch; "2.8T-parameter multimodal LatentMoE (896 experts, top-16…)" is a partial quote.
3. **Gateway-v0.3.1 date** (2026-01-09) is [secondary]-sourced only.
4. **RadixArk $100M seed figure** — single-sourced summary; only the ~$400M valuation is TechCrunch-sourced.
5. **Security fixes** — six Aug-2026 CERT/CC-coordinated issues; fix/landed-version status as of 2026-09-22 not verified.
6. **No Artificial Analysis, ServeTheHome, or SiliconANGLE** SGLang benchmarks/articles were found in this pass.
7. **Adoption/GPU/token counts** ("400,000 GPUs", "trillions of tokens/day") are project self-reports — no independent audit found.
8. **SM120 INT4 correctness issue (#21132)** — community report predates v0.5.16 NVFP4 fixes; current status not verified.
9. **vLLM star/fork counts** and **SGLang enterprise pricing/SLAs** were not pulled (out of scope for this pass).

---

## 13. 2026 news & blog timeline (official unless noted)

- **2026-01-01** — v0.5.7 released (day-0 Mimo-V2-Flash) [official].
- **2026-01-09** — SGLang Model Gateway v0.3.1: 10–12× faster cache-aware routing, 99% memory reduction [official; date [secondary]].
- **2026-01-16** — lmsys.org blog: "SGLang-Diffusion: Two Months In" — up to 2.5× faster than its Nov-2025 initial release [official].
- **2026-01-21** — TechCrunch: "Project SGLang spins out as RadixArk with $400M valuation" (round led by Accel; announced Aug 2025; founders Ying Sheng + Banghua Zhu) [secondary].
- **2026-01-23** — v0.5.8: diffusion speedups up to 1.5× [official].
- **2026-02-19** — lmsys.org blog: "Unlocking 25x Inference Performance with SGLang on NVIDIA GB300 NVL72" [official].
- **2026-02-24** — v0.5.9: LoRA weight-load/compute overlap (−78% TTFT) [official].
- **2026-03-28** — v0.5.10rc0 prerelease [official].
- **2026-04** — lmsys.org blog: "DeepSeek-V4 on Day 0: From Fast Inference to Verified RL with SGLang and Miles" [official].
- **2026-04-06** — v0.5.10: piecewise CUDA graphs default, Elastic NIXL-EP, HiSparse, FlashInfer MXFP8, transformers 5.3.0 [official].
- **2026-05-05** — v0.5.11: CUDA 13 + Torch 2.11 default [official].
- **2026-05-16** — v0.5.12: DeepSeek-V4 day-0 support [official].
- **2026-06** — lmsys.org blog: "The next generation of speculative decoding: DFlash and Spec V2" [official].
- **2026-06** — lmsys.org blog: day-0 support for Nemotron 3 Ultra / Nemotron 3 Super / Higgs Audio v3 TTS [official].
- **2026-06-13** — v0.5.13: Nemotron 3 Ultra day-0 [official].
- **2026-06-26** — v0.5.14: GLM-5.2, LFM2.5, Kimi-K2.7-Code [official].
- **2026-07** — lmsys.org blog: "Serving GLM5.2 NVFP4 agentic workloads with SGLang: Reaching 500 TPS in two weeks" [official].
- **2026-07** — lmsys.org blog: "SGLang and Miles add day-0 support for Kimi K3" [official].
- **2026-07** — lmsys.org blog: "RadixArk and Google bring full SGLang features to TPUs" [official].
- **2026-07-10** — v0.5.15: GLM-5.2 NVFP4 production tuning [official].
- **2026-07-25** — v0.5.16: DSpark speculative decoding [official].
- **2026-08** — SGLang provides day-0 support for OpenAI gpt-oss (noted 2025-08 in README news; gpt-oss PD-decode prefix reuse for SWA hybrids landed v0.5.19) [official].
- **2026-08** — CERT/CC-coordinated disclosure of six SGLang vulnerabilities; mitigations published by community (pickle IPC, dumper port, API key, network isolation) [secondary].
- **2026-08-08** — v0.5.17: Kimi K3 day-0 [official].
- **2026-08-22** — v0.5.18 [official].
- **2026-09-05** — v0.5.19: beam search, DeepEP v2, unified radix tree default [official].
- **2026-09-18** — v0.5.20: RL sampling masks, SGLang Simulator, CUDA 12 retired [official].
- **2026-09** — SGLang-Omni v0.1.6 on PyPI; day-0 AuK/AuK-Flash audio models [official].

## 14. Practical deployment notes (from docs & community, 2026)

- **Install:** `pip install "sglang[srt]"` (PyPI; `sglang` package; Python ≥ 3.10; CUDA 13.x driver needed for recent releases); Docker `lmsysorg/sglang`; from-source for unreleased features [official/secondary].
- **Launch:** `python -m sglang.launch_server --model <hf-id> --tp 2` or `sglang serve`; default port 30000; OpenAI-compatible endpoints [official/secondary].
- **Key server flags (2026):** `--enable-hierarchical-cache` (HiCache), `--moe-a2a-backend {deepep,deepep_v2,mooncake,mori}`, `--moe-runner-backend {triton,flashinfer_cutlass,flashinfer_trtllm,flashinfer_megamoe,cutlass}`, `--speculative-algorithm {EAGLE,DFLASH,DSPARK,NEXTN,NGRAM,STANDALONE}`, `--quantization {fp8,awq,gptq_marlin,...}`, `--kv-cache-dtype fp8_e4m3`, `--enable-layernorm-sp`, `--enable-lean-attention`, `--sampling-mask-max-tokens`, `--enable-response-store`, `--reasoning-parser`, `--tool-call-parser`, `--enable-metrics` (Prometheus `sglang:*`) [official/secondary].
- **Env knobs seen in the wild:** `SGLANG_ALLOW_OVERWRITE_LONGER_CONTEXT_LEN=1`, `SGLANG_DISABLE_CUDNN_CHECK=1`, `VLLM_WORKER_MULTIPROC_METHOD=spawn`, `SGLANG_USE_PICKLE_IPC=false` (security), `SGLANG_DISABLE_LEAN_ATTENTION=1`, `SGLANG_OPT_KDA_FUSED_ACCEPT_STATE=1` [secondary].
- **Multi-node:** PD disaggregation topologies (prefill TP4 + decode DEP4 cited with ~5× TPS/GPU on Qwen3.5 [official]); NIXL/Mooncake RDMA backends; per-scheduler load sockets feed the Model Gateway/router [official].
- **Observability:** `/server_info`, `/metrics` (Prometheus), per-token weight-version spans in generation metadata (v0.5.19), e2e latency metadata in Rust server [official/secondary].
- **Known operational gotchas (community, 2026):** TP model-loading barrier hardcodes a 480 s timeout (`UNBALANCED_MODEL_LOADING_TIMEOUT_S`) — large quantized checkpoints on slow storage can die mid-boot (community patch proposes `SGLANG_UNBALANCED_MODEL_LOADING_TIMEOUT_S` env override) [secondary](https://github.com/mattbucci/2x-3090-ga102-300-a1-sglang-inference/blob/HEAD/scripts/upstream-pr/049-load-timeout-env-PR.md); `/get_server_info` deprecated in favor of `/server_info` (v0.5.10) [official]; `reasoning_tokens` was zero before v0.5.10 [official].
- **Consumer-GPU recipes (cookbooks):** Qwen3.8-27B on RTX 5090 / RTX PRO 6000 / DGX Spark (re-measured v0.5.19); MiniMax-H3 on 24 GB GPUs; Ling-3.0-flash on DGX Spark; Qwen3.5 MXFP4 on MI355X with FP8 KV cache or HiCache host tier [official].

---

*End of report. Research conducted 2026-09-22. All dated claims carry provenance tags; see §12 for open questions.*


---

# PART 3 — SYNTHESIS: vLLM vs SGLang (2026 state)

**Synthesized 2026-09-22 from the two research passes above.**

## 3.1 Snapshot comparison (2026-09-22)

| Dimension | vLLM | SGLang |
|---|---|---|
| Repo | `vllm-project/vllm` (created 2023-02-09) | `sgl-project/sglang` (created 2024-01-08) |
| License | Apache-2.0, no CLA | Apache-2.0 |
| GitHub stars / forks | **92,444 / 22,525** [official] | **36,323 / 9,061** [official] |
| Latest 2026 release | **v0.30.0** (2026-09-22), bi-weekly cadence | **v0.5.20** (2026-09-18), ~2–4 week cadence |
| Core innovation | PagedAttention (block-paged KV) | RadixAttention (radix-tree prefix KV) |
| Engine architecture | V1 engine (V0 removed since v0.11); **Model Runner V2 default since v0.29.0** (MRV1 deprecated, removal target v0.32) [official] | Zero-overhead batch scheduler (v0.4); piecewise/breakable CUDA graphs default (v0.5.10); **unified radix tree default since v0.5.19** [official] |
| Origin | UC Berkeley Sky Computing Lab + LMSYS (SOSP 2023) | UC Berkeley (Ion Stoica's lab) 2023, hosted by LMSYS non-profit |
| Commercial backers | Red Hat AI Inference Server (vLLM + Neural Magic); nm-vllm with SLAs; VMware AI Factory runtime [vendor-reported] | **RadixArk** spin-out (Aug 2025; TechCrunch Jan 2026: ~$400M valuation, Accel-led; $100M seed May 2026 single-sourced [unverified]); paid hosting [secondary] |
| Sponsors | a16z, Dropbox, Sequoia, Skywork AI, ZhenFund (cash); compute from Alibaba Cloud, AMD, Anyscale, AWS, Crusoe, Databricks, DeepInfra, Google Cloud, Intel, Lambda, Nebius, NVIDIA, Replicate, Roblox, RunPod (README list — verify canonical) [official] | a16z Open Source AI Grant (2025), PyTorch Ecosystem member (Mar 2025) [official] |
| Governance | Community-led, no foundation umbrella (third-party assessment Jul 2026) [secondary]; vLLM Production Stack Helm/operator; llm-d (Red Hat/Google/IBM/NVIDIA) builds on vLLM [secondary] | LMSYS non-profit hosts the project; formal security process weaker — Aug-2026 CERT/CC disclosure of 6 vulns; no SECURITY.md at that time [secondary][unverified fix status] |
| Production users (project- or third-party-stated) | Meta, LinkedIn, Mistral, Hugging Face, Uber (third-party claims, unconfirmed) [unverified]; Tesla, Cohere via llm-d [secondary] | xAI (community deck), Cursor, LinkedIn, clouds/GPU providers (README list — production depth unverified) [unverified] |
| Scale claims (self-reported) | "De-facto open standard" characterization [secondary] | "Trillions of tokens/day", "400,000+ GPUs worldwide", "de facto industry standard" [official][unverified] |

## 3.2 Performance picture (independent + vendor, qualified)

- **Prefix-heavy high-concurrency (H100 80GB, Llama 3.1 8B, PremAI 2026):** SGLang ~16,200 tok/s vs
  vLLM ~12,500 tok/s — SGLang +29%, but RunPod's primary attribution is **prefix-heavy traffic only**;
  on unique prompts the engines are "within a few percent" [secondary].
- **Spheron (Llama 3.3 70B FP8, H100):** SGLang +29% total throughput, +117% output-token throughput,
  TTFT −23%, ITL −15%; gap shrinks to +2–5% with unique prompts [secondary].
- **Single unique prompt (community, ~2025-06):** vLLM 60.0 vs SGLang 52.7 tok/s (vLLM ~1.1×) [secondary, dated].
- **DeepSeek family:** SGLang claims 3.1× on DeepSeek-V3 via optimized MLA backends [secondary]; both
  engines had **day-0 support for DeepSeek-V4** (vLLM v0.28-ish stack; SGLang v0.5.12, 2026-05-16) [official].
- **SemiAnalysis InferenceX (Aug 2026) — DeepSeek V4 Pro 0813:** MI355X **SGLang** matched B200 **vLLM**
  on perf/$ until 2026-08-21, when vLLM optimizations (Inferact, NVIDIA) pushed B200 ahead — "a close
  race, not a settled one" [independent].
- **dstack DeepSeek-R1 (H200):** vLLM led at concurrency <128 (online throughput + E2E latency); SGLang
  6,311 tok/s offline. **MI300X:** vLLM beat SGLang in online and offline throughput and E2E latency [independent].
- **Long-context L40S:** TensorRT-LLM ~2× throughput and 7.5× faster TTFT than both; vLLM slight edge
  over SGLang (41.0 vs 38.2 tok/s) [independent].
- **Official GB300 claims:** vLLM blog — Kimi K3 118 → **370 tok/s (3.14×)** with DSpark on 16× GB300 NVL72
  [official]. SGLang lmsys blog (2026-02-19) — "Unlocking 25x Inference Performance on GB300 NVL72" [official].
- ⚠️ "29% gap" numbers must always be quoted **with the prefix-heavy workload qualifier**.

## 3.3 2026 combined timeline (both engines)

| Date | vLLM | SGLang |
|---|---|---|
| 2026-01-01 | — | v0.5.7 (day-0 Mimo-V2-Flash) [official] |
| 2026-01-09 | — | SGLang Model Gateway v0.3.1: 10–12× faster cache-aware routing [official; date secondary] |
| 2026-01-16 | — | lmsys blog: SGLang-Diffusion 2.5× faster than Nov-2025 release [official] |
| 2026-01-20 | **v0.14.0** — first 2026 release [official] | — |
| 2026-01-21 | — | TechCrunch: SGLang spins out as **RadixArk** (~$400M valuation, Accel) [secondary] |
| 2026-01-23 | — | v0.5.8 (diffusion speedups up to 1.5×) [official] |
| 2026-02-19 | — | lmsys blog: 25× inference perf on GB300 NVL72 [official] |
| 2026-02-24 | — | v0.5.9 (LoRA load/compute overlap, −78% TTFT) [official] |
| 2026-02-25 | v0.16.0 — V1 confirmed as only engine [secondary] | — |
| 2026-04-06 | — | v0.5.10 (piecewise CUDA graphs default, Elastic NIXL-EP, HiSparse) [official] |
| 2026-04-27 | v0.20.0 [official] | — |
| 2026-05-05 | — | v0.5.11 (CUDA 13 + Torch 2.11 default) [official] |
| 2026-05-16 | — | **v0.5.12 — DeepSeek-V4 day-0** [official] |
| 2026-06-13 | — | v0.5.13 (Nemotron 3 Ultra day-0) [official] |
| 2026-07-10 | v0.25.0 [official] | v0.5.15 (GLM-5.2 NVFP4 production tuning) [official] |
| 2026-07-25 | — | v0.5.16 (DSpark confidence-driven spec decode) [official] |
| 2026-07-27 | **v0.26.0** — Inkling family; DeepSeek-V4 perf push; vLLM Kimi-K3 blog: 370 tok/s w/ DSpark (3.14×), 16× GB300 NVL72 [official] | lmsys blog: RadixArk + Google bring full SGLang to TPUs [official] |
| 2026-08-08 | — | **v0.5.17 — Kimi K3 day-0** [official] |
| 2026-08-21 | vLLM optimizations push B200 ahead of MI355X-SGLang on DeepSeek V4 perf/$ (SemiAnalysis) [independent] | — |
| 2026-08-22 | — | v0.5.18 (710 PRs) [official; highlights not pulled] |
| 2026-08-26 | **v0.28.0** — Kimi-K3 perf push, DeepSeek V4 sparse MLA, KV disk offload, bitsandbytes→out-of-tree [breaking] [official] | — |
| 2026-08-26 | Google Cloud: vLLM TPU embedding pipelines GA on Ironwood [vendor-reported] | — |
| 2026-08 | — | CERT/CC-coordinated disclosure of 6 SGLang vulnerabilities [secondary] |
| 2026-09-01 | VMware AI Factory ships vLLM-based runtime [secondary] | — |
| 2026-09-05 | — | **v0.5.19** — beam search, DeepEP v2, unified radix tree default, AMD Lean attention [official] |
| 2026-09-09 | **v0.29.0** — Model Runner V2 default, 10 archs removed, RL weight sync [breaking] [official] | — |
| 2026-09-18 | — | **v0.5.20** — RL sampling masks, SGLang Simulator, CUDA 12 retired, XPU/MUSA images [official] |
| 2026-09-22 | **v0.30.0** — Fast Start IPC weight cache, Gumbel-max watermarking, HiSparse, DeepSeek-V4.1-Flash [breaking] [official] | — |

## 3.4 Shared ecosystem (used by both)

- **FlashInfer** (0.6.18): attention/kernel library required/used by both engines; MXFP4/MXFP8 MoE
  backends, TRT-LLM kernels, CuTe DSL [official].
- **NIXL**: NVIDIA's P/D-disaggregation KV-transfer library; vLLM NIXL connectors (v0.29 NIXL P/D DCP
  for MLA), SGLang Elastic NIXL-EP, NIXL RDMA backends [official].
- **Mooncake / MooncakeStore**: KV-cache transfer & L3 storage; vLLM Mooncake connectors (decode KV
  saving, encoder-cache sharing), SGLang MooncakeStore as HiCache L3 + MoE A2A backend; dependency
  `mooncake==0.3.13` (SGLang v0.5.19) [official].
- **DeepEP / DeepEP v2**: DeepSeek's expert-parallel all-to-all; both engines support (vLLM Elastic EP
  with DeepEP v2; SGLang `--moe-a2a-backend deepep_v2` in v0.5.19) [official].
- **Spec decode family**: EAGLE3 (both), DSpark (both, Inferact's open-source block-diffusion), DFlash2
  (both), MTP multi-token prediction [official].
- **llm-d** (4,624 stars): Kubernetes-native distributed inference founded by Red Hat/Google/IBM/NVIDIA;
  vLLM is the default model server, SGLang configs validated per release [secondary].
- **LMCache** (11,892 stars): KV-cache reuse/offload layer integrated with vLLM [secondary].
- **Hugging Face Transformers** 5.x (vLLM upgraded 5.15.0 in v0.28; SGLang 5.3.0 in v0.5.10) [official].
- **AMD AITER / Quark**: AMD kernel stack leveraged by both on ROCm (vLLM: Quark NVFP4; SGLang: Lean
  attention, MoRI/Mori-EP) [official].

## 3.5 Practitioner guidance (convergent across 2026 sources) [secondary][independent]

- **Choose SGLang** when prefix reuse dominates (agentic/RAG/multi-turn), for DeepSeek-family day-0
  kernels, or on AMD MI300X/MI355X fleets (strong independent cost numbers, e.g. up to ~40% $/M-token
  undercut vs B200 on GLM-5) [independent][secondary].
- **Choose vLLM** for the broadest ecosystem and hardware breadth (NVIDIA/AMD/Intel XPU+Gaudi/TPU/CPU/
  Ascend), fastest new-model support, and formal enterprise channels (Red Hat AI Inference Server,
  VMware AI Factory) [secondary].
- **TensorRT-LLM** leads NVIDIA-only peak long-context throughput at the cost of a compile step [independent].
- The headline "29% gap" collapses to ~0 on unique-prompt workloads; always qualify benchmark claims
  by workload shape [secondary][independent].

## 3.6 Unified uncertainty log

1. ⚠️ **vLLM v0.30.0 (released 2026-09-22, research day):** adoption/bug reports not yet available;
   breaking changes (scale-out opt-in, `g_idx` removal, gRPC entrypoint deprecation) carry early-adopter risk. [official]
2. ⚠️ **MRV1 removal targeted at vLLM v0.32** — date unknown; sequence parallelism / elastic EP /
   custom logits processors still fall back to MRV1. [official]
3. ⚠️ **SGLang security fixes** (6 Aug-2026 CERT/CC vulns): fix/landed-version status as of 2026-09-22
   unverified; repo lacked SECURITY.md at disclosure time. [secondary][unverified]
4. ⚠️ **Production-user claims** (vLLM: Meta/LinkedIn/Mistral/HF/Uber; SGLang: xAI/400k GPUs/trillions
   tokens) are third-party or self-reported; no independent audit. [unverified]
5. ⚠️ **AWS Trainium** support in vLLM 2026 — not confirmed from an authoritative source. [unverified]
6. ⚠️ **TGI maintenance-mode claim** (TECHSY, Dec 2025) — unverified.
7. ⚠️ **RadixArk $100M seed figure** — single-sourced; only the ~$400M valuation is TechCrunch-sourced. [unverified]
8. ⚠️ **vLLM README sponsor list** — read from forked README copies; verify against canonical before quoting. [official]
9. ⚠️ **SGLang v0.5.18 highlights** — release-notes body not pulled in the research pass. [official]
10. ⚠️ PR-reported micro-benchmark percentages in both engines' release notes (e.g. "−2.94% E2E TPOT",
    "3.43×") are PR-author claims, not audited results. [official — vendor-reported]
11. ⚠️ **SGLang SM120 INT4 correctness issue (#21132)** — community report predates v0.5.16 NVFP4 fixes;
    current status unverified. [secondary]
12. ⚠️ **SGLang HPU (Gaudi)** — experimental only; "not supported on Gaudi 3" per a May-2026 industry guide. [secondary]

---

*End of synthesis. Parts 1–2 above are the complete original research passes (kept verbatim); this Part 3
adds only the cross-engine comparison. All dated claims carry provenance tags; see §3.6 for open questions.*

