# Step 4 — Track B: Local Inference Stack (llama.cpp + Ollama + LM Studio)
## Final research report (English) — collected September 22, 2026

**Project:** RAG data-collection, Step 4 (Infra inference/training), Track B
**Coverage window:** January 1, 2026 → September 22, 2026
**Subjects:** llama.cpp, Ollama, LM Studio, local inference hardware landscape, GGUF quantization
**Status:** Research snapshot. Prices are dated snapshots; re-verify against official pages before use.

### Provenance legend
- **[official]** — project's own GitHub releases, docs, changelog, or company blog.
- **[vendor-reported]** — figure claimed by the vendor without independent audit.
- **[independent]** — reputable third-party press (ServeTheHome, Tom's Hardware, Bloomberg, Reuters, CNBC, The Register, SiliconANGLE) or independent measurement.
- **[secondary]** — lower-tier press, blogs, AI-generated news mirrors of official releases; useful but unverified.
- **[unverified]** — single-source or conflicting claims; treat as uncertain.

### Contents
- Part A — llama.cpp deep dive
- Part B — Ollama deep dive
- Part C — LM Studio + local inference hardware + GGUF quantization landscape
- Master open-verification log (track-level uncertainties carried forward)

---

## Part A — llama.cpp

**Research date:** 22 September 2026 · **Coverage window:** February → September 2026
**Provenance tags:** [official] = ggml-org GitHub releases/docs · [vendor-reported] = NVIDIA/vendor-published numbers · [independent] = third-party benchmarks/press · [secondary] = community docs/wikis · [unverified] = not corroborated.

---

## 1. Release train: versions, dates, scheme

llama.cpp adopted formal semantic versioning in August 2026; before that it used rolling `b[NNNN]` nightly-style tags on nearly every master commit. Both schemes coexist:

- `vX.Y.Z` — "stable", slower cadence, recommended for downstream/distribution [official] (https://github.com/ggml-org/llama.cpp/releases/tag/v0.2.0)
- `b[NUM]` — bleeding edge/nightly, recommended for developers/technical users [official] (https://github.com/ggml-org/ggml/discussions/1579)

Verified stable releases (dates from GitHub API `published_at`, UTC):

| Tag | Published | Nightly build embedded | Source |
|---|---|---|---|
| v0.1.2 (pre-release) | 2026-08-18 | b10485 | [official] https://api.github.com/repos/ggml-org/llama.cpp/releases/tags/v0.1.2 |
| v0.2.0 | 2026-08-21 | b10566 | [official] https://api.github.com/repos/ggml-org/llama.cpp/releases/tags/v0.2.0 |
| v0.3.0 | 2026-08-25 | b10621 | [official] https://api.github.com/repos/ggml-org/llama.cpp/releases/tags/v0.3.0 |
| v0.4.0 | 2026-09-04 | b10809 | [official] https://api.github.com/repos/ggml-org/llama.cpp/releases/tags/v0.4.0 |

Latest nightly observed at research time: **b11001** (published ~2026-09-16, "last updated 6 days ago" on 2026-09-22 crawl), with b10997, b10994, b10796, b10776 trailing in the days before [official] (https://github.com/ggml-org/llama.cpp/releases/tag/b11001).

Release assets shipped per nightly (b11001 example): macOS arm64/x64 (KleidiAI variant listed DISABLED for macOS arm64), iOS XCFramework, Ubuntu x64/arm64/s390x CPU, Ubuntu x64/arm64 **Vulkan**, Ubuntu x64 **CUDA 12** (12.8 libs) / **CUDA 13** (13.4 libs), Ubuntu arm64 CUDA 13, Ubuntu x64 **ROCm 10.0**, Ubuntu x64 OpenVINO, Ubuntu x64 **SYCL FP32/FP16**, Linux arm64 **Snapdragon (CPU, Adreno GPU, Hexagon NPU)** with setup guide, Android arm64 CPU + Snapdragon, Windows x64/arm64 CPU, Windows arm64 **OpenCL Adreno**, Windows x64 CUDA 12.4/13.4 (+CUDA 13.4 arm64 preview), Vulkan, OpenVINO, SYCL, ROCm 10.0; openEuler x86/aarch64 builds listed as DISABLED (linked to PR #23705); plus a separate UI tarball and `nightly-tag.txt` [official] (https://github.com/ggml-org/llama.cpp/releases).

New official web presence: **https://llama.app** is now the project's site [official] (llama.cpp releases page). Download-and-run shorthand now exists: `llama cli -hf <user>/<repo>` and `llama serve -hf <user>/<repo>` (OpenAI-compatible server) [secondary] (https://selfhost.directory/project/llamacpp).

ggml companion releases: ggml **v0.20.2** (ggml/1589, via v0.1.2 changelog), **v0.21.0** (ggml/1597), **v0.22.0** (ggml/1607), **v0.23.0** (ggml/1579-discussed line) [official] (release pages v0.1.2–v0.4.0).

### v0.4.0 (2026-09-04) — headline [official] (https://github.com/ggml-org/llama.cpp/releases/tag/v0.4.0)
- New models: initial **Qwen3.8-Flash-Next** (`qwen4exp` arch; optimization still pending), **NVIDIA Nemotron-3-Puzzle-75B-A9B** (`NemotronHPuzzle`), **DSpark for Nemotron 3.5**, **nanbeige4.2-3B**.
- Core: lazy/on-demand tensor reading (`llama_lazy_mode`, `--lazy-mode`/`-lzm`; auto-on for tensors > 4 GiB) — reduces RAM peaking at load; `--n-cpu-ffn`; per-layer expert routing/FFN (`n_expert_used_max`); KV-cell token tracking (session/state version bump); n-gram history lookup; optimized KV restore; quantizer RAM cap + row-slab streaming; autoscaled YaRN training context; **DFlash2 support**; fused DFlash encoder into KV injection; **sparse flash attention for DeepSeek-V4/GLM and Qwen4exp**; **Apple RDMA as an RPC transport**; RPC event/async backend APIs; synthetic spec-acceptance options.
- Multimodal: video params (`--video-*`), `mtmd_tokenize_from_parts()`, DeepSeek-V4-Flash-Vision-Exp, Gemma-4 vision fixes, WebP via ffmpeg, Idefics3 fix.
- Server: **per-slot context limit**, `data:` URLs for media, `preserve_reasoning` enabled by default, reject prefilled assistant tool calls, synthetic spec acceptance options.
- ggml v0.23.0: sparse-attention ops (`ggml_flash_attn_ext_set_n_kv_max`), RPC event/async, Apple RDMA.

### v0.3.0 (2026-08-25) — headline [official] (https://github.com/ggml-org/llama.cpp/releases/tag/v0.3.0)
- New model: **dots3-note** multimodal with a new **DSA-ISWA KV cache** type.
- **MTP (multi-token prediction) support for GLM-4.5-Air**; tensor-split mode for DeepSeek 4 (`-sm tensor`, meta-backend tensor-split with split-state propagation fixes); DSpark for bailingmoe3; mamba2 GEMM-over-GEMV dispatch.
- `fit` logic moved from server to common, now accounting for `n_streams`.
- ggml v0.22.0: tensor-split in meta backend, Metal per-op kernels with parallel compilation, non-in-place `ggml_clamp`, new ops (`POOL_1D`, `PAD_REFLECT_1D`), Q2_K SYCL kernels, OpenCL MoE bias fusion.

### v0.2.0 (2026-08-21) — headline [official] (https://github.com/ggml-org/llama.cpp/releases/tag/v0.2.0)
- Marks the **start of consistent semantic versioning**.
- Highlights: ggml v0.21.0; KleidiAI SME2 F32 GEMV kernel; SYCL Q2_K/Q5_K kernels; tensor-split for LFM2/LFM2MoE; DSpark for LFM2; Metal quantized-KV dequantization for large batches; server router lazy-load `startup_models`; RPC `use_count` fusion; Vulkan Q8_0 KV dequant; Windows ARM64 CUDA support in CI.

### v0.1.2 (2026-08-18, pre-release) — headline [official] (API)
- Notable: CUDA MMVQ `nwarps=8` for bs=1 dense models on **DGX Spark** (#26843) — early GB10 tuning; docs for **MCP stdio servers** and CORS defaults (#26847); vocab support for **integer tokenizer scores** (#27260); ggml v0.20.2.

### Earlier 2026 (Feb–Aug, from `b` tags) — selected milestones [secondary]/[official]
- **b7964** (Feb 2026): Step3.5-Flash support (#19283). **b7973**: Qwen3.5 dense and MoE support (community-driven, PR #19435; author notes "Llama.cpp missing all the zero-day releases", built on the common-delta-net PR #19125) [secondary] (https://pypi.org/project/llama-cpp-pydist/0.31.0/).
- **b8018** (2026-02-13): pydist sync point with 44 upstream commits [secondary] (same).
- Community build records (Apr 2026, Strix Halo): b8119 (kyuz0 custom), b8299 official (+40% prompt speed over b8119), b8461 (kyuz0 Vulkan RADV) [secondary] (https://github.com/valentijnvenus/strix-halo-setup).
- llama-cpp-python fork (JamePeng) synced to upstream commit `9723942` on 2026-08-31 with `load_mode` + Qwen3.8-Flash-Next + DFlash2 NVFP4 fix [secondary] (https://github.com/tao71-ai/llama-cpp-python-jamepeng/blob/HEAD/CHANGELOG.md).

---
## 2. GGUF format updates (2026)

### 2.1 Mainline llama.cpp: NO new weight-quantization types in 2026
The authoritative evidence is the current-master server README (September 2026): allowed KV-cache types are still exactly `f32, f16, bf16, q8_0, q4_0, q4_1, iq4_nl, q5_0, q5_1` [official] (https://github.com/ggml-org/llama.cpp/blob/master/tools/server/README.md). The weight-quant family remains the established three tiers:

| Family | Examples | Calibrated? | Quality ranking (per a 2026 community taxonomy) |
|---|---|---|---|
| Legacy | Q4_0, Q4_1, Q5_0, Q5_1, Q8_0 | No | Worst-per-bit, but Q8_0 near-lossless [secondary] |
| K-quants | Q2_K … Q6_K (Q4_K_M still the mainstream default) | No (data-free) | Middle [secondary] |
| I-quants | IQ1_S … IQ4_XS/IQ4_NL (non-linear lattice codebooks + importance matrix) | Yes (imatrix) | Better per-bit, especially < 4 bpw [secondary] |

(https://github.com/noonghunna/club-3090/blob/HEAD/docs/QUANTIZATION.md)

The only 2026 "format-adjacent" additions in mainline are:
- **NVFP4 scales** in the DFlash2 speculative-decoding path (dflash: pass missing NVFP4 scales to attention ops, #28000, v0.4.0 window) [official] (v0.4.0 changelog).
- **TQ1_0/TQ2_0** (ternary BitNet quants) exist in mainline but pre-date 2026 (v0.2.0 changelog marks `tq2_0` as not supported on SYCL — i.e., it exists, just unsupported there) [official] (v0.2.0 changelog).
- **DFlash2/NVFP4 draft metadata**: new GGUF metadata keys for the DFlash2 drafter (`conv_kernel_size`, `conv_group_size`, `selector_rank`, `selector_top_k`) [secondary] (https://github.com/wszhoho/llama-cpp-turboquant-dflash2 — describes the format; upstream merged DFlash2 via #27342).
- Vocab handling: **integer tokenizer scores** supported (v0.1.2, #27260); DFlash draft conversion reuses the *target's* vocab via `--target-model-dir` and adds `_vocab_hparams()` so special-token ids follow the target config (fixes HunYuan pad/eod ids) [official] (releases page, PR #28890 description).

### 2.2 New-in-2026 quantization types are FORK-EXCLUSIVE (verified)
- **IQK quants** (`IQ4_KS`, `IQ5_KS`, `IQ4_K`, `IQ2_K`, `IQ2_KS`, `IQ3_KT`, `IQ4_KT`, `IQ1_S`) — ik_llama.cpp only. Refined grids + imatrix + co-designed CUDA/CPU kernels; widely regarded as the best quality-per-bit in the GGUF world [secondary] (https://github.com/noonghunna/club-3090/blob/HEAD/docs/QUANTIZATION.md; https://github.com/ikawrakow/ik_llama.cpp). Note: ik's README warns **not** to use Unsloth `_XL` models containing f16 tensors, and that K-quants lack CUDA row-interleaved implementations so `-rtr` hurts hybrid GPU/CPU for them [secondary] (ik README).
- **TurboQuant** (`TURBO2_0`=enum 43, `TURBO3_0`=44, `TURBO4_0`=45; WHT rotation + PolarQuant, based on arXiv 2504.19874/ICLR 2026; fused into FA kernels as turbo KV types; InnerQ channel equalization) — **wszhoho/llama-cpp-turboquant-dflash2** fork only [secondary] (https://github.com/wszhoho/llama-cpp-turboquant-dflash2). (Strix Halo lab notes confirm turbo KV types were CPU-only upstream-incompatible and the setup later retired the fork for upstream Vulkan [secondary] https://github.com/bogdan-d/strix-halo-setup.)
- **ROCmFPX** (GGML types 100–107, "GGML_TYPE_COUNT=108" in that build) — **artomyuan/llama.cpp-rocm** fork only; fixes "invalid type 101" validation in `llama-quantize` (2026-09-14 changelog) [secondary] (https://github.com/artomyuan/llama.cpp-rocm/blob/HEAD/CHANGELOG.en.md).
- **APEX I-Compact** quants appear in community benchmarks (Qwen3.6 35B MoE "APEX I-Compact") but no verified upstream/FHF format spec was located — treat as [unverified] in the GGUF-format sense [secondary] (https://github.com/luksamuk/ai-dotfiles/blob/HEAD/llama-swap/docs/BINARIES.md).

### 2.3 Quality-per-bit evidence (2026)
- Unified evaluation on Llama-3.1-8B-Instruct (arXiv 2601.14277, 2026): PPL increase vs FP16 — Q8_0 +0.1%, Q6_K +0.4%, Q5_K_M +1.1%, Q4_K_M +3.3%, Q4_K_S +4.1%, Q3_K_M +8.7%, Q3_K_S +22.4% [independent] (https://arxiv.org/pdf/2601.14277v1). Note: 2026-trained models lose measurably more per quant step than 2023-era models [secondary] (https://github.com/pradeepgudipati/gguf-switchboard/blob/HEAD/docs/QUANT_SCORING.md).
- Quesma benchmark (Aug 2026) of Qwen3.8-27B: **Q4_K_M matches BF16** on GPQA Diamond, IFBench, Terminal-Bench 2.1; Q2 drops; IQ1 collapses to chance [secondary] (https://github.com/chriscorbell/llm-server/blob/HEAD/docs/research/2026-09-07-b70-and-qwen38-27b.md).

---
## 3. Backends: status in 2026

| Backend | Status / 2026 highlights |
|---|---|
| **CUDA** | First-class. XOR-swizzle flash-attn K/V smem fp16 tiles (#25635); tuned MMVQ→MMQ decode crossover per HW/quant (#26079, #28285 for SM87); fused MoE weighted expert reduction extended to specdec + topk-router fusion (#27621, #25952); fast `mm_ids_helper` for any `n_expert_used` (#27978); **concurrent streams per split for multi-GPU** (#28198); CUDA 12 (12.8 libs) and CUDA 13 (13.4 libs) official builds; **Windows arm64 CUDA 13.4 (preview)** added [official] (release pages + v0.4.0 changelog). |
| **Metal** | First-class. Massive 2026 per-device **fa-vec tunings**: M1, M2, M2 Pro, M2 Max, M1 Ultra, M3, M3 Pro, M3 Max, M3 Ultra, **A18 Pro (MacBook Neo)**, M5+ — plus **Metal 4.0 tensor API enabled on M5+/A19+** (#27461); sparse FA (#28098); per-op source split + parallel compilation (v0.3.0/ggml 0.22); top-k radix; quantized-KV dequant to F16 before FA for large batches; bin-kernel consolidation (#19390, Feb 2026) [official] (v0.4.0, v0.2.0 changelogs; pydist changelog). |
| **Vulkan** | Strong community/mainline backend. IQ3_S mat-vec for batch>4 (#27449); bfloat16 extension only if supported (#28155); Q8_0 KV dequant in coopmat1 (#25494); tiled transpose; **Intel Xe FA optimization kernels (Xe-LPG Plus / Xe2 / Xe3, #24406)**; mat-vec row tuning for **Strix Halo** batched inference (#27909); top_k radix for k≥1024 (Qwen 3.8 Flash Next, #28032); Vulkan FA MMQ fp32 Q-quant [official] (releases page, v0.2.0/v0.4.0). Reported ~4–5% uplift from Mesa 26.0.1→26.0.2 on Strix Halo [secondary] (https://github.com/getnyrex/strix-halo-guide/blob/HEAD/RESEARCH.md). |
| **ROCm/HIP** | Official builds at **ROCm 10.0** (updated in v0.4.0 window, #27803); radix TOP_K for long rows (#27466); RDNA3 MMQ config tuning; Q2_0 dot-product path for gfx1201 (#26753). Known pain: ROCm 7.2 regression — 73% perf loss with `GGML_HIP_ROCWMMA_FATTN=ON` on upstream (Feb 28, 2026); Qwen3.5 hang on gfx1151 during load_tensors (open, 2026-03-18; workaround `--flash-attn off --n-gpu-layers 1`); kernel 6.19.4 misdetects gfx1151 as gfx1100 [secondary] (https://github.com/getnyrex/strix-halo-guide/blob/HEAD/RESEARCH.md). |
| **SYCL** | Intel GPUs. Fused rms_norm+mul+add / add+add chains (#27610); Q2_K reordered MMVQ + ESIMD kernels; Q5_K ESIMD; peer-to-peer copy API (#27550); MKL FA refactor; host-pinned 2 GiB cap (#27559); FP32/FP16 official builds. `tq2_0` marked unsupported [official] (v0.2.0/v0.3.0/v0.4.0 changelogs). |
| **OpenCL** | Adreno-focused (+Intel Xe-LP tuning). Quant lm_head / decode GEMV and medium-batch GEMM for spec-decoding/MTP (#26477); MoE expert-bias folding into epilogue for gpt-oss (#26431); fused SSM scan (Mamba-2) (#26439); deterministic MoE expert scatter (#26464); Xe-LP TG/PP tuning (#26438); Adreno A6x/A7x compiler workarounds [official] (v0.2.0/v0.3.0/v0.4.0). |
| **OpenVINO** | Still **[In Progress]** in the supported-backend matrix; official Windows/Linux builds ship with OpenVINO 2026.3/2026.3.1; CI had failing OpenVINO tests disabled in the v0.4.0 window (#28347) [official] (README backend table via selfhost.directory; v0.4.0 changelog). |
| **Hexagon (Snapdragon)** | New mobile-tier backend with dedicated release builds (Linux arm64 + Android arm64: CPU/Adreno/Hexagon NPU) and setup guides. 2026 work: MUL_MAT/MUL_MAT_ID fusion, F16 unary ops, device discovery + on-demand sessions, FA HMX queue fixes, CPY fence fix; added to `docs/backend/ops.md` (#28263) [official] (releases page, v0.4.0/v0.2.0). |
| **WebGPU** | All-targets; misc fixes (ARGSORT/TOP_K inf, tensor_get offset, rope offset support, overlapping-src mulmat) [official] (v0.2.0/v0.3.0/v0.4.0). |
| **CPU (x86/ARM/RISC-V)** | AVX/AVX2/AVX512/AMX; ARM NEON + KleidiAI (SME2 F32 GEMV added; KleidiAI macOS build listed DISABLED in latest assets — status unclear, flag as [unverified]); RVV/ZVFH/ZICBOP/ZIHINTPAUSE; **AVX2: large-batch PP speedup for IQ models** (#27402); SpacemiT IME kernels (#27961); s390x fixes [official] (v0.2.0/v0.4.0). Zen4 PP speedups for IQ2_KS/IQ4_KS/IQ5_KS are ik-fork-only (#428) [secondary] (ik README). |
| **RPC / distributed** | **Apple RDMA** added as RPC transport (#26421); RPC event/async backend APIs (#18626); buffer serialization fixes (#26500); `use_count` fusion (v0.2.0) [official] (v0.4.0/v0.2.0). Note: Pi-cluster RPC measurements show ~25× regression vs single-node on 1 GbE [independent] (https://github.com/hellomatik-org/distributed-llama/raw/cfbdcd4c16659a7f61edc0413b05e09b37f74060/paper/main_en.pdf). |
| Others listed | BLAS, BLIS, **CANN** (Ascend NPU), **MUSA** (Moore Threads GPU), IBM zDNN, ZenDNN, VirtGPU [official] (backend table). |

### 3.1 Model-architecture support added in 2026 (notable)
Qwen3.5 dense+MoE (b7973, Feb 2026) → Qwen3-Next/Qwen3.5-MoE fused delta-net (ik, Mar 2026) → Qwen3.6-27B → **Qwen3.8-Flash-Next** (`qwen4exp`, v0.4.0, upstream) → DeepSeek-V4 (sparse FA, DSpark, MLA), GLM-4.5-Air MTP, GLM-5/5.2, Nemotron-3-Puzzle / Nemotron 3.5 DSpark, Gemma 4 (+vision fixes), **Step3.5-Flash** (b7964), **Step 3.7**, bailingmoe3 (DSpark), LFM2/LFM2-MoE (+DSpark), dots3-note (DSA-ISWA KV), MiniMax-01, HunyuanOCR 1.5/v1.0 (+DFlash support #28890), GraniteSWA/GraniteMoeSWA, nanbeige4.2-3B, Qwen3-TTS-0.6B fix, DeepSeek-V4-Flash-Vision-Exp [official] (release pages v0.1.2–v0.4.0; pydist changelog; ik README). **Known issue:** Qwen3.8-27B emits silent EOS past ~130K prompt tokens on CUDA/CPU — open issue #27756 (Aug 26, 2026), suspected GDN recurrent-state error [secondary] (https://github.com/chriscorbell/llm-server/blob/HEAD/docs/research/2026-09-07-b70-and-qwen38-27b.md).

---
## 4. Performance: flash attention, speculative decoding, measured pp/tg

### 4.1 Flash attention & core kernels (2026)
- **Sparse flash attention** landed in mainline: CUDA + ggml sparse-FA for DeepSeek-V4/GLM (#27970), Metal sparse FA (#28098), `ggml_flash_attn_ext_set_n_kv_max` scheduling API (ggml v0.23.0) [official] (v0.4.0).
- Dense-FA tuning: CUDA XOR-swizzle of K/V smem fp16 tiles; per-device (Q, NE) fa-vec tunings across the whole Apple line; MoE-weighted expert reduction fusion extended to speculative decoding; Metal dequantized quantized KV→F16 before FA for large batches; Vulkan dequant-Q8_0-KV-once in coopmat1 [official] (v0.2.0/v0.4.0).
- KV-cache machinery: n-gram history lookup in the sequence position index; early sequence-scan termination; non-contiguous restore optimization; per-layer expert routing; `--n-cpu-ffn`; `--fit` auto layer distribution (moved from server to common, stream-aware) [official] (v0.3.0/v0.4.0).
- **DFlash2** (block-diffusion speculative decoding, inco.ai): 2B Apache-2.0 draft released **2026-08-18**; llama.cpp engine support merged **2026-08-27** via PR #27342 (Jian Chen, per community doc); converter fix for NVFP4 scales (#28000); fused DFlash encoder into KV-cache injection [official+secondary] (v0.4.0; https://runaihome.com/blog/qwen38-27b-dflash2-speculative-decoding-guide-2026/; https://github.com/nathanw1014/strix-halo-llamacpp/blob/HEAD/docs/dflash2-strix.md).

### 4.2 Speculative decoding modes in 2026
| Mode | Status |
|---|---|
| Draft-model (EAGLE-style) | Long-standing `--spec-type` support |
| **draft-mtp** (multi-token prediction heads) | First-class; native flag `--spec-type draft-mtp`; upstream added MTP for GLM-4.5-Air (v0.3.0); ik supports MTP for GLM-4.x, Qwen 3.5/3.6, Gemma 4, GLM 5, Step 3.7, Qwen 3.8 [official/secondary] |
| **DFlash/DFlash2** | Block-diffusion: drafter predicts a whole block (block_size=16) in one pass from extracted target hidden states; DFlash2 adds conv+selector for higher acceptance; replaces MTP [official/secondary] |
| **DSpark** | DFlash + semi-autoregressive Markov head + confidence head for DeepSeek-V4-class models; upstream supports DSpark for Nemotron 3.5, bailingmoe3, LFM2 [official] (v0.2.0/v0.3.0/v0.4.0) |
| ik self-spec | n-gram and suffix self-speculative decoding [secondary] (ik README) |

### 4.3 Published pp/tg numbers (as reported — do not compare across setups)
- **NVIDIA gpt-oss-20b on llama.cpp** (NVIDIA testing via llama.cpp): RTX 5090 **282 tok/s**, Mac M3 Ultra **116 tok/s**, AMD 7900 XTX **102 tok/s** [vendor-reported via independent] (https://www.techradar.com/ai-platforms-assistants/gpt-oss-20b-performance-faster-pc-rtx-nvidia).
- **DFlash2, Qwen3.8-27B** (Sept 7, 2026): 3.43× claimed on SGLang/vLLM at C=1; llama.cpp implementation measures **1.81×**; RTX 3090 on vLLM ≈ 2.6×. If MTP already runs, DFlash2 adds only a few percent [secondary] (https://runaihome.com/blog/qwen38-27b-dflash2-speculative-decoding-guide-2026/).
- **DFlash2 on Strix Halo (Vulkan/RADV, 2026-08-19)**: 2.23× at d0, 1.89× at 8k, **2.00× at 32k**; v1 collapses to 1.02× by 32k; decode base 11.81→10.54 t/s [secondary] (https://github.com/nathanw1014/strix-halo-llamacpp/blob/HEAD/docs/dflash2-strix.md).
- **ik_llama.cpp vs upstream** (RTX 3050 6GB hybrid, Qwen3.6 35B MoE APEX I-Compact + MTP): ik **91.2 pp / 40.4 tg** vs upstream 60.8–67.1 pp / 30.5 tg (+41% prompt); dense GPT-OSS 20B Q4_K_M upstream wins (66.8–104.6 pp / 31.8 tg vs ik 42.1–53.9 / 22.4). Rule of thumb: ik for MoE, upstream for dense [secondary] (https://github.com/luksamuk/ai-dotfiles/blob/HEAD/llama-swap/docs/BINARIES.md).
- **Strix Halo (Qwen3-30B-A3B, 130k ctx)**: ROCm pp512 41 t/s tg128 5 t/s; rocWMMA-tuned pp512 51 / tg128 13 t/s; Vulkan RADV pp512 17 / tg128 13 t/s. DGX Spark vs Strix Halo @120B tg32: **56 vs 47–53 t/s** [secondary] (https://github.com/getnyrex/strix-halo-guide/blob/HEAD/RESEARCH.md).
- **Strix Halo (gfx1151, 64GB, Vulkan, Apr 2026)**: kyuz0 RADV build — **351 pp / 19 tg**; official b8299 — **393 pp / 22 tg**; turboquant fork 8793 — 393 pp / 22 tg [secondary] (https://github.com/valentijnvenus/strix-halo-setup).
- **DGX Spark GB10 (llama.cpp, INT4)**: Llama 3.3 70B ≈ 3.8 t/s decode; GPT-OSS 120B (4-bit MoE) ≈ 32–33 t/s; general guidance <20B = 15–40 t/s [secondary] (https://github.com/dylancouzon/aie-talk/blob/HEAD/research/V-nvidia-arm.md).
- **Dual Xeon (2× QYFS/8480+, ik_llama.cpp, DeepSeek-R1 Q4_K_M)**: pp512 109.69 t/s, pp1000 105.46 t/s, tg128 8.74 t/s [secondary] (https://forums.servethehome.com/index.php?threads/es-xeon-discussion.5031/page-205).
- **Apple Silicon study (M2 Ultra, early 2026 arxiv 2511.05502)**: llama.cpp ≈150 t/s short-context only; MLX ≈230, MLC-LLM ≈190, Ollama 20–40 [independent] (https://arxiv.org/pdf/2511.05502v1.pdf).
- **llama.cpp vs Ollama**: 3–12% raw throughput advantage claimed for llama.cpp (video analysis, 2026-08) [secondary] (https://www.youtube.com/watch?v=MAkd1qmOuAI); vs vLLM on single 24 GB card: llama.cpp **120 t/s vs vLLM 19 t/s** (6–7×, CUDA-graphs memory argument) [secondary] (https://medium.com/data-science-collective/what-is-the-best-hardware-for-running-local-llms-in-2026-mac-vs-5090-vs-cloud-ff023b660442).
- Qwen3.8-Flash-Next ≈125B-A6B MoE + 51B "engram" table on Strix Halo 128GB (EngramHalo fork, UD-IQ4_XS): ≈28 t/s warm [secondary] (https://github.com/sypherin/strix-halo-setup/blob/HEAD/docs/qwen3.8-flash-next-engramhalo-stability.md).

---
## 5. ik_llama.cpp fork — status & headline features

**Repo:** https://github.com/ikawrakow/ik_llama.cpp (Iwan Kawrakow). Active in September 2026 (README references PRs up to ~#2369; changelog entries from 2026-03 merged `--fit` etc.) [secondary].

**What it is:** performance-focused fork of llama.cpp with better CPU and hybrid GPU/CPU performance, new SOTA quantization types, first-class BitNet support, and MLA/FlashMLA/fused-MoE improvements [secondary] (https://github.com/ikawrakow/ik_llama.cpp/blob/HEAD/README.md).

**Headline 2025→2026 features (from its own changelog):**
- **IQK quants**: IQ2_KS, IQ3_KT, IQ4_KT, IQ4_KS, IQ5_KS, IQ1_S — kernels co-designed with the grids; AVX-VNNI optimizations; Zen4 PP speedups for IQ2_KS/IQ4_KS/IQ5_KS (#428); faster IQ3_KT/IQ4_KT (#453); ~2% CUDA tg gain for iq2_ks (#468).
- **New split mode "graph"** for multi-GPU (PR #1022) — tensor parallelism at the GGML graph level; reported **3–4× speedup on 4× Tesla T4** vs layer/row splitting [secondary] (https://medium.com/@jagusztinl/llama-cpp-performance-breakthrough-for-multi-gpu-setups-04c83a66feb2).
- Fused delta-net for Qwen3-Next and Qwen3.5-MoE (#1315/#1333/#1362/#1373); Hadamard transforms for K/V cache (#1033/#1034/#1527); auto-fit offloaded tensors to VRAM for MoE+dense (#1501/#1504, merged Mar 2026, per ik fork docs), per-GPU fit margin (#1872).
- **MTP decoding**: GLM-4.x MoE (#1270), Qwen 3.5/3.6 (#1698/#1745), Gemma 4 (#1744), GLM 5 (#1890), Step 3.7 (#2250), Qwen 3.8 (#2369), incl. standalone MTP heads via `-md`.
- Self-speculative decoding (n-gram #1261, suffix #1646); DFlash initial support (#1970); DSpark initial support (#2280).
- Server extras: **OpenAI `/v1/responses` endpoint** (#1184), function-call support (#628), jinja template support (#677), expiring logit bias, string-ban, Adaptive-P sampler, multimodal vision in llama-mtmd-cli and llama-server, mikupad alt WebUI, MCP support (#1904), dynamic control-vector endpoints, on-demand tensor reload.
- Checkpoints for recurrent models (#1310/#1398); GLM-DSA indexer cache; GLM-5.2 vision hack (#2283).

**Why it matters:** the go-to engine for **CPU and hybrid CPU/GPU MoE inference** (DeepSeek-class models on consumer hardware); origin of IQK quants widely considered the best GGUF quality-per-bit; source of several ideas later re-adopted upstream (auto-fit/`--fit` logic). **Scope limits (explicit):** only CPU (AVX2+/NEON+) and CUDA (Turing+) are fully supported backends — ROCm/Vulkan/Metal issues are not handled [secondary] (ik README). Only use `-rtr` (row-interleaved repack) deliberately for k-quants, which lack CUDA row-interleaved implementations [secondary] (ik README).

## 6. Other notable forks (2026) — names verified, not guessed

| Fork | Focus | Verification |
|---|---|---|
| **artomyuan/llama.cpp-rocm** | ROCm-first fork: ROCmFPX quants (GGML types 100–107), TurboQuant, mtmd-grounders multimodal vision (LocateAnything + DeepSeek4V dual projector); synced to upstream 97e4ca735 on 2026-09-14; claims pp512 +105% / pp2048 +109% (measured 2026-09-15) | [secondary] https://github.com/artomyuan/llama.cpp-rocm/blob/HEAD/CHANGELOG.en.md |
| **wszhoho/llama-cpp-turboquant-dflash2** | TurboQuant (TURBO2_0/3_0/4_0 = enum 43/44/45) + DFlash/DFlash2/DSpark drafts + DeepSeek-V4 ecosystem; tracks upstream closely (Qwen3.5, Gemma4, Kimi-K3 support) | [secondary] https://github.com/wszhoho/llama-cpp-turboquant-dflash2 |
| **prismml-eng/llama.cpp** | OpenVINO-oriented fork (docs/backend/OPENVINO.md with Ubuntu install scripts for OpenVINO 2026.3) | [secondary] https://github.com/prismml-eng/llama.cpp/blob/HEAD/docs/backend/OPENVINO.md |
| **kyuz0/amd-strix-halo-toolboxes** | ROCm toolboxes + llama.cpp builds for Strix Halo (gfx1151), incl. **EngramHalo** variant for Qwen3.8-Flash-Next's 51B engram table; rocm-10.0 images (Aug 2026) | [secondary] https://github.com/sypherin/strix-halo-setup/blob/HEAD/docs/qwen3.8-flash-next-engramhalo-stability.md; https://github.com/ashebanow/nix-config/commit/7f93e6e59ffc6fb2bd7d8eab53e5960d7670bd0e |
| **nathanw1014/strix-halo-llamacpp** | Strix Halo branch: DFlash2 validation on gfx1151/Vulkan (Aug 2026) | [secondary] https://github.com/nathanw1014/strix-halo-llamacpp/blob/HEAD/docs/dflash2-strix.md |
| **pugant/strix-halo-llamacpp-lab**, **pugant/strix-nebulosa** | ROCmFP4-STRIX_LEAN preset origin; ships fork source tree `rocmfpx/` | [secondary] https://github.com/pugant/strix-halo-llamacpp-lab |
| **mrlordcat/llama.cpp-rdna-lab** | RDNA/Strix research docs (spec-decoding status, DFlash via `--spec-draft-device ROCm1`) | [secondary] https://github.com/mrlordcat/llama.cpp-rdna-lab/blob/HEAD/docs/research/SPEC_DECODING_STATUS.md |
| **Anbeeld/beellama.cpp** | Source of the DFlash port into llama.cpp (`c6dfa39e3`) | [secondary] (referenced by mrlordcat docs) |
| **poisonxa16/pxq_llama.cpp** | Documents itself as a fork tracking ik_llama.cpp (DELTA-SINCE-IK.md); mirrors ik README | [secondary] https://github.com/poisonxa16/pxq_llama.cpp/blob/HEAD/docs/README-upstream-ik_llama.md |
| **black6spdz/ik_llama.cpp** | Stale mirror of ik (last updated ~212 days ago) | [secondary] https://github.com/black6spdz/ik_llama.cpp |
| **tao71-ai/llama-cpp-python-jamepeng** | llama-cpp-python fork, syncs upstream frequently (2026-08-31: load_mode + Qwen3.8-Flash-Next + DFlash2 NVFP4 patch) | [secondary] https://github.com/tao71-ai/llama-cpp-python-jamepeng/blob/HEAD/CHANGELOG.md |
| **dushyant30suthar/opencode-llama.cpp** | Experiment notes repo (DFlash vs MTP measurements, exllamav3 cross-checks) | [secondary] https://github.com/dushyant30suthar/opencode-llama.cpp/blob/HEAD/docs/HANDOVER-2026-08-01.md |

Note: Tom's/ServeTheHome-level press does not cover these forks individually; the list above is verified by repo existence + 2026-dated content. Anything else (e.g., hypothetical "llama.cpp-openvino") was NOT verified and is excluded per the no-guessing rule.

---
## 7. Server mode (llama-server) — features as of September 2026

Current-master README (tools/server/README.md) lists [official] (https://github.com/ggml-org/llama.cpp/blob/master/tools/server/README.md):

- **OpenAI-compatible API**: chat completions, **responses** (`/v1/responses`), and embeddings routes; multimodal (vision/audio/video) via the OpenAI-compatible API; accepts `data:` URLs for media input (added in v0.4.0 window, #27735).
- **Anthropic Messages API-compatible** chat completions; **reranking endpoint** (#9510); assistant-message prefilling à la Claude API; `preserve_reasoning` **on by default** (v0.4.0, #28174).
- **Function calling / tool use for ~any model**; prefilled assistant tool calls now rejected (v0.4.0, #27626); **MCP**: web UI replaced "MCP overrides" with a **tool policy** (v0.4.0, #27745); server README documents **MCP stdio servers** and CORS defaults (#26847, v0.1.2 window).
- **Parallel decoding with multi-user support, continuous batching, speculative decoding** (incl. DFlash2/synthetic spec-acceptance options).
- Serving ops: per-slot context limits (v0.4.0, #24124); `/metrics` accessible even during sleep (#27376); model endpoints can be made private when auth is enabled (#26347); `dedup-cache-models` preset (#27346); router mode lazy-loads `startup_models` (#27424); docker-repo (`--docker-repo`, e.g. `gemma3`) and **Hugging Face shorthand** `--hf-repo <user>/<model>[:quant]` (default Q4_K_M; mmproj auto-download) — `llama serve -hf ggml-org/Qwen3.5-0.8B-GGUF`.
- CLI-serving flags of note: `--lazy-mode/-lzm` (on-demand tensor read), `--load-mode` (auto/mmap/mlock/mmap+mlock/dio), `--fit/--fit-target/--fit-ctx` (auto-fit to device memory), `-ncmoe`/`-ncffn` (keep first N MoE/FFN layers on CPU), `-sm {none,layer,row,tensor}`, `--spec-draft-type-k/v` KV types, `--video-*` params.
- Web UI: redesigned (June 2025, legacy available via `--path ./examples/server/public_legacy`); tabbed chat navigation (v0.3.0); chat form actions UX, agentic-response grouping/copy (v0.4.0); pytest-xdist server tests (v0.4.0).

## 8. Community & press coverage (2026)

### Tom's Hardware
- **"Nvidia DGX Spark review: the GB10 Superchip … beats out AMD's Ryzen AI Max+ 395"** (~2026-01-27, "238 days ago" at crawl): uses **llama.cpp as the standard LLM inference benchmark platform** ("broadly compatible … well-documented and tunable benchmarking interface") vs Corsair AI Workstation 300 (Ryzen AI Max+ 395). Tested prompt-processing/prefill and token generation on llama-3.1-8B, Gemma 3 12B/27B, plus a token-generation **power-efficiency test: 4096 tokens from GPT-OSS 120b measuring joules/token**, with GPT-OSS 20B on both platforms for comparison [independent] (https://www.tomshardware.com/pc-components/gpus/nvidia-dgx-spark-review/3).
- Also: OpenClaw on Beelink SER10 MAX (2026-08) used llama.cpp at **10.64 tok/s** for a general-knowledge query [independent] (https://www.tomshardware.com/tech-industry/artificial-intelligence/setting-up-openclaw-isnt-as-straightforward-as-the-internet-wants-you-to-think-running-local-ai-on-humble-hardware).

### ServeTheHome
- **No dedicated llama.cpp benchmark article was found on servethehome.com in 2026.** Community coverage lives in the **STH Forums**: ES Xeon Discussion thread (May 2026 posts — dual-Xeon + ik_llama.cpp DeepSeek-R1 numbers cited above), ASUS Pro WS W790E-SAGE / Xeon SPR-SP thread (DeepSeek-R1 on W790 + RTX 4090 via llama.cpp). STH main site adjacent: MLPerf Inference v5.0 results (June 4, 2026) and a Lenovo ThinkPad X1 Carbon Gen 14 review using **MLPerf Client 1.6.1** (OpenVINO path, not llama.cpp) [independent/secondary] (https://forums.servethehome.com/index.php?threads/es-xeon-discussion.5031/page-205; https://www.servethehome.com/lenovo-thinkpad-x1-carbon-gen-14-review/).

### Other press/independent numbers
- **TechRadar** (gpt-oss launch, Aug 2025 window): NVIDIA RTX AI PCs + llama.cpp — RTX 5090 **282 tok/s** on gpt-oss-20b vs M3 Ultra 116 vs 7900 XTX 102 [independent/vendor-reported] (https://www.techradar.com/ai-platforms-assistants/gpt-oss-20b-performance-faster-pc-rtx-nvidia).
- **RunAIHome** (2026-09-07): DFlash2 guide with honest 1.8–2.7× guidance [secondary].
- Community hardware guides (vucense, Medium data-science-collective, mayhemcode) routinely use llama.cpp numbers as the reference engine; key framing: VRAM capacity is the first constraint; Strix Halo is a capacity play (128 GB unified, ~256 GB/s → ~6 t/s ceiling on 70B Q4_K_M) [secondary] (https://vucense.com/tech-reviews/compute-chips/local-llm-hardware-2026-strix-halo-m5-ultra-rtx-5090-70b-models/).
- arXiv 2601.14277 (Jan 2026): unified GGUF quantization evaluation on Llama-3.1-8B-Instruct (quality table above) [independent] (https://arxiv.org/pdf/2601.14277v1).
- arXiv 2511.05502 (Nov 2025): Apple Silicon engine comparison incl. llama.cpp ≈150 t/s on M2 Ultra [independent] (https://arxiv.org/pdf/2511.05502v1.pdf).

## 9b. Quantization tooling & operational notes (2026)
- **llama-quantize** (2026): RAM cap via new `max_buf_size` quantize param (#27795); row-slab streaming to avoid thread starvation (#27830); memory optimization by evicting weights after each layer (#22877, v0.2.0 window); combined with `--lazy-mode`/`-lzm` and the "prevent RAM peaking at load" fix (#27483) — 2026 is the year llama.cpp fixed its RAM spikes on both load and quantize paths [official] (v0.4.0, v0.2.0 changelogs).
- **KV-cache quantization**: allowed types unchanged (f32/f16/bf16/q8_0/q4_0/q4_1/iq4_nl/q5_0/q5_1); new per-model draft KV-cache variants in code (`llama-kv-cache-dsv4.cpp` draft ring, `llama-kv-cache-iswa.cpp`, `-dsa.cpp`, `-msa.cpp`) track new attention types (DSpark, DSA-ISWA, MSA) rather than new quant types [official/secondary] (v0.3.0 dots3-note; wszhoho fork README).
- **Multimodal (mtmd/libmtmd)**: vision/audio in llama-server; video params `--video-*`; `mtmd_tokenize_from_parts()`; DeepSeek-V4-Flash-Vision-Exp; dots3-note vision+audio; WebP via ffmpeg; Pillow-accurate resize; `--mmproj-device`; SAM deepseek-ocr conv2d with F32 im2col [official] (v0.3.0/v0.4.0 changelogs).
- **Finetune tool**: fixed "no KV cache" bug (#27199, v0.4.0 window) [official].
- **CI/release infra**: signed release artifacts + attestations (v0.2.0); release.sh + nightly-tag.txt; BoringSSL 0.20260903.0 vendored (v0.4.0); cpp-httplib 0.54.0 [official].

## 9. Uncertainties & gaps (flagged explicitly)
1. **Feb–Jul 2026 b-tag milestones**: only sampled (b7964/b7973/b8018/b8119/b8299/b8461/b10485). A complete commit-level timeline was out of scope; numbers are from changelogs/build records, not direct tag inspection.
2. **KleidiAI macOS build** is listed DISABLED in current release assets — reason not established [unverified].
3. **GGUF spec version**: no GGUF *container* version bump was found in 2026 changelogs; tokenizer/vocab metadata additions (integer scores, DFlash2 keys) documented above are the confirmed changes. Absence of a version bump is an inference from changelog silence — treat as [unverified].
4. **Tom's Hardware DGX Spark benchmark numbers** (charts) could not be extracted (page renders charts as images); the methodology/model list is confirmed, numeric pp/tg values are not.
5. **APEX I-Compact** quant format: seen only in one community benchmark table; no format spec verified.
6. **Qwen3.8-Flash-Next "engram"** (51B per-layer embedding table, SSD lazy-read) is described by a community fork (EngramHalo), not by upstream docs — details are [secondary].
7. Fork performance claims (e.g., artomyuan pp512 +105%) are fork-author-reported; not independently reproduced here.

## 10. Key source URLs
- https://github.com/ggml-org/llama.cpp/releases/tag/v0.4.0
- https://github.com/ggml-org/llama.cpp/releases/tag/v0.3.0
- https://github.com/ggml-org/llama.cpp/releases/tag/v0.2.0
- https://github.com/ggml-org/llama.cpp/releases/tag/b11001
- https://github.com/ggml-org/llama.cpp/blob/master/tools/server/README.md
- https://github.com/ggml-org/ggml/discussions/1579 (release versioning policy)
- https://github.com/ikawrakow/ik_llama.cpp
- https://github.com/artomyuan/llama.cpp-rocm/blob/HEAD/CHANGELOG.en.md
- https://github.com/wszhoho/llama-cpp-turboquant-dflash2
- https://www.tomshardware.com/pc-components/gpus/nvidia-dgx-spark-review/3
- https://www.techradar.com/ai-platforms-assistants/gpt-oss-20b-performance-faster-pc-rtx-nvidia
- https://runaihome.com/blog/qwen38-27b-dflash2-speculative-decoding-guide-2026/
- https://github.com/nathanw1014/strix-halo-llamacpp/blob/HEAD/docs/dflash2-strix.md
- https://arxiv.org/pdf/2601.14277v1 (quant evaluation)
- https://llama.app

---
*End of report. File: ~/workspace/rag_collect/etape4_draft_llamacpp.md*


---

## Part B — Ollama


> RAG data-collection project, Step 4, track B, part 2. Research cut-off: 22 September 2026.
> Provenance tags: `[official]` = Ollama/official docs & GitHub releases; `[vendor-reported]` = Ollama statements via press; `[independent]` = reputable press; `[secondary]` = third-party summaries/docs/repos quoting official material; `[unverified]` = single-source, low-confidence.
> **Rule followed: no version numbers or model names were guessed; everything below is sourced from tool output with URLs.**

---

## 1. Executive summary

- Ollama remains the dominant local-LLM runner in 2026: #1 most-starred conversational-AI open-source project (178,535 stars, Aug 2026 [secondary]), ~8.9M monthly developers, 85% of Fortune 500 [vendor-reported via TechCrunch, 9 Jul 2026], ~1M installs/week [vendor-reported].
- Funding: **$65M Series B led by Theory Ventures announced 9 July 2026**; $88M raised total (prior $15M Series A, Benchmark) [independent].
- 2026 release cadence: v0.21 (Apr) → v0.30 (Jun, engine rebuild on llama.cpp + MLX) → v0.32 (Jul–Aug, Muse Glimmer, coding-agent "launch" integrations) → v0.33 (late Aug) → **v0.34.2 (15 Sep 2026, latest at cut-off)** [official/secondary].
- Strategic pivot of 2026: from pure local runner to **local + cloud hybrid** — Ollama Cloud (`:cloud` models, per-token pricing, off-peak discounts), **coding-agent integrations** (`ollama launch claude/codex/pi/openclaw/hermes/dsh/muse`), and first-party **desktop apps** wired into ChatGPT Desktop and Claude Desktop.
- Pricing shifted during 2026 from GPU-time billing (still reported Jul 2026) to **transparent per-token pricing with usage credits**: Free $0 / Pro $20/mo ($60 usage) / Max $100/mo ($300 usage) / Team $500/mo ($1,000 shared) [secondary, Sep 2026]; Team pricing data conflicts with a seat-based figure (see §7, flagged).
- Competition: LM Studio (GUI/desktop lane, proprietary, paid enterprise) and vLLM (server-throughput lane, up to 2.6x throughput claims) are the two named challengers; 2026 coverage consistently ranks Ollama #1 for developer API use but notes weak concurrency and no built-in auth.

---

## 2. Company, team and funding

- **Founders:** Jeff Morgan (CEO) and Michael Chiang. Previously built Kitematic, acquired by Docker; then built Docker Desktop. Ollama launched 2023 [independent: TechCrunch, 9 Jul 2026]. URL: https://techcrunch.com/2026/07/09/popular-open-source-ai-developer-tool-ollama-raises-65m-grows-to-nearly-9m-users/
- **Series B:** $65M, announced **9 July 2026**, led by Theory Ventures [independent: TechCrunch].
- **Total raised:** $88M. Prior round: $15M Series A led by Benchmark's Peter Fenton (Fenton sits on the board) [independent: TechCrunch]. Morgan and Fenton declined to discuss revenue or valuation [independent].
- **Team size:** 14 employees (stated Jul 2026) [vendor-reported via TechCrunch].
- **Business thesis (2026):** the proving point for monetization came around January 2026 when OpenClaw became popular — "larger open models suddenly became able to do these agentic tasks, like coding" — pushing deep-pocketed enterprises and AI app-layer startups toward open models for daily workloads while reserving closed models for as-needed use [vendor-reported: Morgan via TechCrunch]. Benchmark's Fenton frames every high-inference-spend company as having a "vital existential project" to move to open-weight models [independent].
- **Cloud model:** Ollama hosts larger models on its "neocloud" via subscription tiers (free to $100/month at Jul 2026); usage then tracked by GPU time, not tokens [independent: TechCrunch, 9 Jul 2026]. *Note: this billing model was superseded by per-token billing by Sep 2026 — see §6.*
- **Founder position on cloud vs. open source:** Morgan calls cloud an evolution of the mission ("too big to run on your own computer. So we said, 'Hey, let's help find the compute for that'"); Fenton: "Nothing has changed for the core product that's free on the desktop" [independent: TechCrunch].
- **Community tension:** ~a year before Jul 2026, blog/social posts complained the cloud business was drawing attention from the free project, citing Ollama as an example of the "enshittification" of dev tools [independent: TechCrunch, referencing 2025 HN/Reddit/blog posts].
- **Competitive funding context (Jul 2026):** open-source inference providers also raising — Inferact (maker of vLLM) and RadixArk (maker of SGLang) named alongside Ollama, OpenClaw/NanoClaw, and Arcee [independent: TechCrunch].

---

## 3. Release timeline 2026 (latest first)

> Ollama ships "multiple/month" [secondary]. Version numbers below are all from release notes; RC tags noted where the source only confirmed an RC.

### v0.34.2 — 15 September 2026 [secondary: dev.to summary of official changelog]
- First-run setup when running `ollama`: option to sign in or continue locally; state shared with the desktop app on macOS and Windows [secondary].
- `ollama://apps` deep link opens the Apps page of the desktop app directly (macOS, Windows) [secondary].
- Fix: "Fixed excessive memory growth during long generations with MLX speculative decoding" [secondary].
- llama.cpp update (no version detail in changelog) [secondary].
- Source: http://dev.to/jtorchia/ollama-v0342-vs-v0341-a-targeted-mlx-fix-not-the-same-regression-mkh (summarizes https://github.com/ollama/ollama/releases/tag/v0.34.2)

### v0.34.1 — 14 September 2026 [official via newreleases.io mirror of GitHub release]
- MLX safetensors `ollama create` is **no longer experimental**; creating GGUF models now requires llama.cpp tooling for safetensor conversion and quantization [official].
- "Improved MLX memory handling on Apple Silicon" [official].
- Runaway repeat-token detection now requires **100 repeat tokens** before triggering (fewer false positives, e.g. OCR) [official].
- `/api/tags` much faster on large model libraries: **3.1 s → 294 ms cold** in testing; model capabilities reported consistently [official].
- Deprecated `typical_p`: can no longer be set on new models; existing GGUF models retain support [official].
- MLX and llama.cpp updates [official].
- Source: https://newreleases.io/project/github/ollama/ollama/release/v0.34.1 (mirror of https://github.com/ollama/ollama/releases/tag/v0.34.1)

### v0.34.0 — 8 September 2026 [official: GitHub release]
- **Ollama models can now be used directly in ChatGPT Desktop** — "keep your existing workflow while running open models"; setup available from the Ollama app on macOS [official].
- Improved structured-output performance on Apple Silicon [official].
- Support for OpenAI-compatible client **tool search** and **response compaction** [official].
- Source: https://github.com/ollama/ollama/releases/tag/v0.34.0

### v0.33.3 — early September 2026 [official: GitHub release]
- `gemma4` now supports **images and audio on the MLX engine** [official].
- Report cached prompt tokens [official].
- Honor GGUF model-defined default parameters [official].
- MLX, MLX-C, llama.cpp updates [official].
- Source: https://github.com/ollama/ollama/releases/tag/v0.33.3

### v0.33.0 — 26 August 2026 [secondary: bokiko/localist news]
- **Claude Desktop**: developers can configure Claude Desktop to work with Ollama as a third-party gateway provider [secondary].
- Source: https://github.com/bokiko/localist/blob/HEAD/news/2026-08.md (references https://github.com/ollama/ollama/releases/tag/v0.33.0)

### v0.32.15 — 21 August 2026 [secondary: bokiko/localist news]
- New desktop onboarding flow on first launch [secondary].
- Caches resolved model metadata between requests; time-to-first-token roughly halved (TTFT ~995 ms → ~524 ms in benchmarks) [secondary].
- Source: https://github.com/bokiko/localist/blob/HEAD/news/2026-08.md (references https://github.com/ollama/ollama/releases/tag/v0.32.15)

### v0.32.11 — 18 August 2026 [secondary: boardwire-ai article of official release]
- `ollama launch dsh` supports **DeepSeek Harness** (DeepSeek's open-source agent harness) [secondary].
- `ollama launch muse` supports **Muse Code** (Meta's agentic coding CLI) [secondary].
- The **OpenAI-compatible Responses API now supports web search** [secondary].
- Muse Glimmer template updates [secondary].
- Source: https://github.com/jonas-is-coding/boardwire-ai/blob/HEAD/articles/2026-08-18-ollama-v0-32-11-b60cd68a161b.md (source: https://github.com/ollama/ollama/releases/tag/v0.32.11)

### v0.32.10-rc0 — 13 August 2026 [secondary: boardwire-ai article of official RC]
- Kernel-fusion optimization for ModelOpt checkpoints applying float32 global scale: on an M5 Max, `qwen3.6:27b` prefill **703 → 769 t/s (+7.9%)**, `muse-glimmer:30b` prefill **790 → 843 t/s (+6.7%)**; greedy outputs byte-identical; speculative decode unchanged within noise [secondary].
- Source: https://github.com/jonas-is-coding/boardwire-ai/blob/HEAD/articles/2026-08-13-ollama-v0-32-10-rc0-a8ebf902d5a6.md

### v0.32.8 — 12 August 2026 [secondary: boardwire-ai article of official release]
- **Muse Glimmer on all platforms**: NVIDIA, AMD and additional platforms (previously Apple Silicon only) [secondary].
- Source: https://github.com/jonas-is-coding/boardwire-ai/blob/HEAD/articles/2026-08-12-ollama-v0-32-8-05c0630e94fc.md

### v0.32.7 — 10 August 2026 [secondary: boardwire-ai article of official release]
- **Muse Glimmer**: Meta's newest open model, the first released by **Meta Superintelligence Labs**; 30B multimodal model purpose-built for agent workloads that run locally [secondary].
- Initial support via Ollama's **MLX engine on Apple Silicon** with **DFlash** speculative decoding and image input [secondary].
- Usage: `ollama run muse-glimmer` / `ollama run muse-glimmer:30b-mlx`; powers Claude Code, Codex, Pi, OpenClaw, Hermes via `ollama launch <agent> --model muse-glimmer` [secondary].
- Source: https://github.com/jonas-is-coding/boardwire-ai/blob/HEAD/articles/2026-08-10-ollama-v0-32-7-21fdc04ea90a.md

### v0.32.5 — 28 July 2026 [secondary: bokiko/localist news]
- Fixed an MLX Metal bug that could reduce output quality for **NVFP4** models, particularly Laguna [secondary].
- Source: https://github.com/bokiko/localist/blob/HEAD/news/2026-07.md (references https://github.com/ollama/ollama/releases/tag/v0.32.5)

### v0.32.4 — 26 July 2026 [secondary: bokiko/localist news]
- Support **Laguna on Apple GPUs via the MLX engine** [secondary].
- Quantize draft-model output heads at the requested type when creating speculative-decoding drafts [secondary].
- Source: https://github.com/bokiko/localist/blob/HEAD/news/2026-07.md (references https://github.com/ollama/ollama/releases/tag/v0.32.4)

### v0.32.3 — 24 July 2026 [secondary: bokiko/localist news]
- Fixed model downloads that stall before sending data [secondary].
- Integration fixes: restored Claude Code Channels, fixed Anthropic thinking streams, Hermes Desktop respects `--force-build` [secondary].
- Source: https://github.com/bokiko/localist/blob/HEAD/news/2026-07.md (references https://github.com/ollama/ollama/releases/tag/v0.32.3)

### v0.32.0 — 13 July 2026 [secondary: boardwire-ai article of official release]
- `create`: select the **qwen3.5 parser and renderer** for Qwen3.5/Next (PR #17078) [secondary].
- `launch`: warn before old agent models (PR #17063); new agent UI in cmd (PR #17017) [secondary].
- Source: https://github.com/jonas-is-coding/boardwire-ai/blob/HEAD/articles/2026-07-13-ollama-v0-32-0-720f6826f66d.md

### v0.30.11 — June 2026 [official: GitHub release v0.30.11-rc0]
- `launch`: thinking-capability detection for opencode; **auto-install Claude Code / opencode when missing** [official].
- `launch/codex`: detect model drift when Codex App UI switches [official].
- Hardware: fix inverted iGPU/dGPU Vulkan classification on Windows hybrid graphics; add sm_86 to cuda_v13_windows preset; Jetson CC 87 for CUDA v13; use host Vulkan loader on Windows; CUDA JIT packaging fix for MLX [official].
- Engine: unify/tune speculative decoding in mlxrunner; llama.cpp version update [official].
- Docs: redesign docs landing and integrations overview; document max think level [official].
- Source: https://github.com/ollama/ollama/releases/tag/v0.30.11-rc0

### v0.30.9 — 17 June 2026 [secondary: boardwire-ai article of official release]
- **Cohere2Moe architecture support** [secondary].
- Fixed LFM2 parser/render for cases where thinking was not emitted [secondary].
- Fixed bug where `ollama launch claude` and other coding-agent/assistant use cases would only output one token [secondary].
- Ollama returns an error if a single message exceeds the current context window [secondary].
- Source: https://github.com/jonas-is-coding/boardwire-ai/blob/HEAD/articles/2026-06-17-ollama-v0-30-9-de557a3a864d.md

### v0.30.0 — June 2026 (RCs from late May 2026) [secondary: boardwire-ai articles of official RCs]
- **Architecture change**: directly supports **llama.cpp** instead of building on top of GGML; enables GGUF file-format compatibility [secondary].
- **MLX** used to accelerate model inference on Apple Silicon [secondary].
- Known issues at RC: `laguna-xs.2` not yet supported on Windows/Linux; `llama3.2-vision` not yet supported; `nomic-embed-text` now lowercases inputs per model card (behavior change vs. prior versions) [secondary].
- Install pins: `OLLAMA_VERSION=0.30.0-rc31` (Mac/Linux install.sh, Windows install.ps1) [secondary].
- Sources: https://github.com/jonas-is-coding/boardwire-ai/blob/HEAD/articles/2026-06-01-ollama-v0-30-0-977511cd989c.md ; https://github.com/jonas-is-coding/boardwire-ai/blob/HEAD/articles/2026-05-29-ollama-v0-30-0-b1fd508aac93.md

### v0.21.2-rc0 — 23 April 2026 [secondary: AppSelfHost]
- **Bundled OpenClaw web search at launch** — web search available out of the box at Ollama start, no extra config or sidecar [secondary].
- Expanded docs covering **structured outputs** and **Ollama Cloud** [secondary].
- GitHub stars at the time: **169,752** [secondary].
- Source: https://appselfhost.com/ollama-v0-21-2-rc0-released-bundled-web-search-and-structured-outputs-docs-land-in-new-rc/

### Flagged uncertainty — v0.31.x
- A third-party research doc lists Ollama "v0.31.2 / v0.31.3 (Apr 22 2026)" [secondary: geeks-accelerator/ollama-herd research], which conflicts with version ordering (v0.21.x in late April, v0.30.x in June, v0.32.0 on 13 Jul 2026). The date is likely a snapshot error; v0.31.x presumably shipped late June / early July 2026, but **this is [unverified]** — no release-note content for v0.31.x was recovered.
- Source: https://github.com/geeks-accelerator/ollama-herd/blob/HEAD/docs/research/mlx-vs-ollama-adoption-2026.md

---

## 4. Model library — notable 2026 additions

> Names verified via official `ollama.com/library/*` pages or official release notes — none guessed. Cloud models carry a `:cloud` suffix in the CLI/app (`gemma4:cloud`) and are suffix-less via the direct Cloud API (`gemma4:31b`) [official: docs.ollama.com/cloud].

### Cloud library (verified names, Sep 2026 snapshot)
Per community-maintained snapshots of the official catalog [secondary: grikomsn/ollama-cloud-copilot-chat (snapshot 2026-09-06), kirklasalle/prism]:

| Model (cloud name) | Params | Notes |
|---|---|---|
| `gpt-oss:120b`, `gpt-oss:20b` | 120B / 20B | OpenAI open-weight models; 131K context; thinking Low/Med/High [secondary] |
| `deepseek-v3.1:671b` | 671B | Reasoning, hybrid thinking [secondary] |
| `deepseek-v4-flash`, `deepseek-v4-pro`, `deepseek-v4.1-flash` | — | **Official library page verified**: `deepseek-v4.1-flash` is a multimodal MoE, **552B backbone, up to 1M-token context**, Causal Encoder-Decoder design, 8B active params/token prefill, 16B decode, FP4 main KV cache (890 bytes/token) [official: https://ollama.com/library/deepseek-v4.1-flash] |
| `kimi-k2:1t`, `kimi-k2-thinking` | 1T | Moonshot; agentic/coding [secondary] |
| `kimi-k3` | — | 1.049M context, images, tools; **requires Pro/Max subscription + extra usage credits** [secondary: official library https://ollama.com/library/kimi-k3 cited in third-party doc] |
| `kimi-k2.6`, `kimi-k2.7-code` | — | 262K context, images, tools [secondary] |
| `qwen3-coder:480b` | 480B | Agentic coding [secondary] |
| `qwen3.5:397b` (aka Qwen 3.5 397B) | 397B | 262K context, images, tools [secondary] |
| `glm-5.1`, `glm-5.2`, `glm-5.3`, `glm-5.3-flash` | — | Zhipu; 1.049M context (Flash adds images) [secondary] |
| `minimax-m2.7`, `minimax-m3` | — | 197K/512K context [secondary] |
| `gemma4:31b` | 31B | Google; 262K, images, tools; images+audio on MLX since v0.33.3 [official/secondary] |
| `mistral-large-3:675b` | 675B | 262K, images, tools [secondary] |
| `nemotron-3` (nano 30B / super / ultra) | — | NVIDIA; 262K [secondary] |

### Local library — 2026 additions
- **muse-glimmer** (30B, Meta Superintelligence Labs) — first model from Meta's new Superintelligence Labs; multimodal, agent-purpose-built; MLX engine with DFlash + image input; tags include `muse-glimmer:30b-mlx` [secondary: v0.32.7/v0.32.8 release notes].
- **gpt-oss:20b** — recommended 2026 local reasoning option, OpenAI-style [secondary: Medium Sep 2026 roundup].
- **qwen3-coder:30b** — best 2026 local coding model per roundups; 30B MoE, ~19 GB package, 256K context, needs ~24 GB VRAM [secondary].
- **qwen3.6:27b**, **qwen3.6:35b-a3b**, **qwen3:4b-instruct** [secondary].
- **devstral-small-2:24b** (Mistral coding-agent alternative) [secondary].
- **qwen3-vl:235b** (vision-language), **glm-4.6:14b** (agentic), **minimax-m2** (efficient coding) — listed in Jan 2026 official library pulse [secondary].
- **LFM2 / LFM2.5** (Liquid AI) — parser/render support added v0.30.9; community MLX ports note DFlash support [secondary].
- **Laguna** family (`laguna-xs.2` referenced in v0.30.0 known issues; NVFP4 fixes v0.32.4/v0.32.5) [secondary]. *Provenance of the Laguna model family itself (vendor) was not verified in this research — flagged [unverified].*
- **Cohere2Moe** architecture support added v0.30.9 [secondary].
- **deepseek-r1:14b** — compact reasoning pick in Sep 2026 roundups [secondary].

### Retirements (cloud)
- **Kimi K2.5 and MiniMax M2.5 retired 2026-07-31**; untagged `deepseek-v4-pro` / `deepseek-v4-flash` aliases removed from `/api/tags`, only dated `0731`/`0813` tags kept [secondary: grikomsn doc, citing official https://docs.ollama.com/cloud#retirements].
- Official docs confirm usage settings show upcoming retirements; downloaded local models are never affected [official: https://docs.ollama.com/cloud].

---

## 5. API features

### Native API (localhost:11434)
- `POST /api/generate`, `POST /api/chat`, `POST /api/pull`, `POST /api/create`, `GET /api/tags`, `DELETE /api/delete`, `POST /api/show`, `POST /api/embed`, `GET /api/ps` [secondary: whisperjav research doc].
- **`/api/tags` performance**: 3.1 s → 294 ms cold on large model libraries (v0.34.1) [official].
- **Web search (cloud)**: `POST https://ollama.com/api/web_search` with `query` (required), `max_results` (default 5, max 10); returns title/url/content snippets; API-key auth [secondary: uwuclxdy docs].
- **Bundled web search at launch**: OpenClaw web search integrated into Ollama startup since v0.21.2-rc0 (Apr 2026) [secondary].
- **Responses API**: OpenAI-compatible `/v1/responses` gained **web search support** in v0.32.11 (Aug 2026) [secondary].

### OpenAI compatibility
- Endpoints: `POST /v1/chat/completions`, `GET /v1/models`, `POST /v1/embeddings` [secondary]; any OpenAI-SDK code works with a base-URL change (dummy API key accepted) [secondary: Medium practical guide, Apr 2026].
- **Structured outputs** (JSON-schema enforcement), **tool calling / function calling**, streaming supported via the OpenAI-compatible surface [secondary: Medium guide].
- **Tool search** and **response compaction** for OpenAI-compatible clients added in v0.34.0 (Sep 2026) [official].
- Improved **structured-output performance on Apple Silicon** in v0.34.0 [official].

### Tool calling / agents
- `ollama launch` integrations (auto-install when missing, since v0.30.11): **claude** (Claude Code), **codex**, **pi**, **openclaw**, **hermes**, **dsh** (DeepSeek Harness, v0.32.11), **muse** (Muse Code, v0.32.11) [official/secondary].
- v0.32.0 added an **agent UI** (`cmd`) and warnings before old agent models [secondary].
- **Thinking/reasoning**: "max think level" documented (v0.30.11); Anthropic thinking streams fixed (v0.32.3); thinking-capability detection for opencode (v0.30.11); models expose thinking modes (e.g. Kimi/DeepSeek/GPT-OSS Low/High/Max) [official/secondary].

### Vision & audio & embeddings
- Vision: image input supported (e.g. gemma4 images+audio on MLX since v0.33.3 [official]; Muse Glimmer image input on MLX [secondary]; qwen3-vl, kimi-k3, deepseek-v4.1-flash multimodal [official/secondary]).
- Embeddings: `/v1/embeddings` + native embedding support; `nomic-embed-text` (lowercase behavior change in v0.30.0) commonly used [secondary].
- Direct Cloud API: point any client at `https://ollama.com` with `Authorization: Bearer <API key>`; supports OpenAI and Anthropic client shapes (subsets) [official: docs.ollama.com/cloud].

### Cloud access modes (two)
1. **Cloud-local hybrid**: `ollama signin` locally → pull `-cloud` models → local Ollama proxies to cloud [secondary].
2. **Direct Cloud API**: client pointed at `https://ollama.com` with API key; cloud model names omit the `-cloud` suffix (e.g. `gpt-oss:120b`) [secondary].
- Compute hosted primarily in the **US**, routable to **Europe and Singapore** for capacity [secondary: mindstone doc citing ollama.com/cloud, 9 Sep 2026].
- Privacy: "Ollama processes cloud prompts and responses to answer your requests. **We do not use them to train models**" [official: docs.ollama.com/cloud]; zero data retention claimed for cloud tiers [secondary].

---

## 6. Cloud / Turbo offerings and pricing history

> The task asked about "Ollama Cloud / Turbo". Research found **"Ollama Cloud"** as the current product name with `:cloud` models. A distinct "Turbo" branded tier was **not confirmed in sources** — [unverified]; earlier (pre-2026) reporting referenced cloud access more loosely. Flagging rather than asserting.

### Pricing eras
1. **GPU-time era (through at least Jul 2026):** cloud tiers from free to $100/month; "tracks usage based on GPU time, not token limits" [independent: TechCrunch, 9 Jul 2026].
2. **Transparent per-token era (by Sep 2026):** "transparent pricing update replaces vague cloud access with a metered model: paid plans include monthly usage credits… The unit of cost is no longer the seat or the session, it is the actual prompt and output volume" [secondary: Tech To Heart, ~6 Sep 2026].

### Current plans (per-token era) [secondary: mindstone-agent doc, verified against ollama.com/pricing 9 Sep 2026; corroborated by Tech To Heart]
| Plan | Price | Included | Concurrency |
|---|---|---|---|
| Free | $0 | starter usage credits + starter models | 1 concurrent request |
| Pro | $20/mo | $60 monthly usage credits, larger models | 3 concurrent requests (50x free usage) |
| Max | $100/mo | $300 monthly usage credits | 10 concurrent requests (250x free) |
| Team | $500/mo | $1,000 shared usage | shared billing |
- Credits can be added to any plan for pay-as-you-go access to all models [secondary].
- Local model usage is unlimited on all plans [secondary].
- Sources: https://github.com/mindstone-agent/mindstone-agent/blob/HEAD/OLLAMA_CLOUD_QWEN3.5.md ; https://techtoheart.com/ollama-clouds-off-peak-pricing-turns-scheduling-into-a-cost-lever/

### Per-model per-token rates (USD per 1M tokens; verified 8 Sep 2026 against ollama.com/pricing) [secondary: wobblebot implementation doc]
| Model | Input | Cached input | Output |
|---|---|---|---|
| `gpt-oss:120b` | 0.15 | 0.014 | 0.60 |
| `gpt-oss:20b` | 0.07 | 0.035 | 0.30 |
| `qwen3.5:397b` | 0.60 | full input rate | 3.60 |
| `gemma4:31b` | 0.14 | 0.05 | 0.40 |
- Source: https://github.com/carldog/wobblebot/blob/HEAD/docs/implementation/ollama-cloud.md

### Off-peak pricing (DeepSeek V4) [secondary: Tech To Heart, ~6 Sep 2026]
- Off-peak: **DeepSeek V4 Flash** $0.22 input / $0.007 cached / $0.66 output per 1M; **DeepSeek V4 Pro** $0.66 / $0.022 / $1.98.
- Peak (12:00–18:00 UTC, Mon–Fri): **exactly 2x** off-peak rates.
- Off-peak applies all day Saturday and Sunday.
- Source: https://techtoheart.com/ollama-clouds-off-peak-pricing-turns-scheduling-into-a-cost-lever/

### Usage mechanics (as reported by third-party operators, Sep 2026)
- Four usage levels (light→extra-heavy models); usage measured by weighted input/cached/output tokens against plan allowance [secondary: coding-plan skill doc].
- Session limits reset every 5 hours; weekly limits every 7 days (no published numbers) [secondary].
- Extra-usage regime: after included quota, billing draws from prepaid balance at per-token rates; API returns 200 in both regimes with no quota headers — only signal is 429 at full exhaustion [secondary: merchant-routing-engine doc]. *Treat operator observations as [secondary], not official.*

---

## 7. Enterprise features

- **Team plan**: conflicting reports —
  - (a) **$500/mo with $1,000 shared usage**, shared billing [secondary: Sep 2026 docs above];
  - (b) **$25/seat/mo, 5-seat minimum ($125/mo min)**, usage included per seat, overage from shared team balance billed pay-as-you-go, US + Europe model access, zero data retention, shared billing/admin, priority support [secondary: coding-plan skill doc, sign-up state observed **28 Aug 2026**].
  - **Flag:** the two figures likely reflect different page states/dates (Aug 28 vs Sep 9, 2026) or different plan presentations. Both are [secondary]; neither confirmed from the live pricing page by this research.
- **Enterprise (custom)**: custom terms and support for larger orgs; everything in Team + volume pricing, security/procurement support, deployment planning with Ollama; no published price [secondary].
- **SSO / access controls / MDM**: listed as **"Coming soon: SSO, model access controls, MDM installer for Windows and macOS"** (as of 28 Aug 2026) — i.e. **not shipped** at cut-off [secondary].
- **Team sign-up state**: "Join waitlist", not an open purchase button (28 Aug 2026) [secondary].
- The self-hosted core remains free/open-source (MIT); local inference has no usage costs beyond hardware/electricity [independent/secondary].
- Positioning gap noted by third parties: Ollama has **no built-in auth, RBAC, audit logging, or multi-tenant controls** on the local server — "175,000+ Ollama instances are exposed without auth" [secondary: solon GTM doc]; production multi-user setups pair it with gateways (LiteLLM etc.) [secondary: markaicode, Aug 2026].

---

## 8. Adoption figures

| Metric | Value | Date | Provenance |
|---|---|---|---|
| GitHub stars | **178,535** (#1 in conversational-AI tracked set) | Aug 2026 | [secondary: dappros/state-of-conversational-ai 2026-Q3] |
| GitHub stars | ~177,000 | 28 Jul 2026 | [secondary: agents-radar AI OSS trends] |
| GitHub stars / forks | **176,000 / ~17,000** | 9 Jul 2026 | [independent: TechCrunch] |
| GitHub stars | 169,752 | 23 Apr 2026 | [secondary: AppSelfHost] |
| GitHub stars | ~164,300 | 7 Mar 2026 | [secondary: github-topstar daily] |
| Monthly developers | **~8.9M** | Jul 2026 | [vendor-reported via TechCrunch] |
| Fortune 500 penetration | **85%** | Jul 2026 | [vendor-reported via TechCrunch] |
| Installs | **~1M/week** | Jul 2026 | [vendor-reported via ollama-herd research citing TechCrunch/TFN] |
| Monthly downloads | 100K (Q1 2023) → **52M (Q1 2026)**, 520x | Mar 2026 | [unverified: blackroados research notes — single source, methodology unclear] |
| Rank vs peers (stars, Aug 2026) | ollama 178,535 > langflow 153,239 > dify 152,436 > open-webui 148,767 > langchain 144,246 > llama.cpp 123,903 > vLLM 89,044 | Aug 2026 | [secondary: dappros report] |
| GGUF models on Hugging Face | 135,000 | Mar 2026 | [unverified: blackroados notes] |

- Sources: https://techcrunch.com/2026/07/09/popular-open-source-ai-developer-tool-ollama-raises-65m-grows-to-nearly-9m-users/ ; https://github.com/dappros/state-of-conversational-ai/blob/HEAD/2026-q3/report/report.md ; https://github.com/geeks-accelerator/ollama-herd/blob/HEAD/docs/research/mlx-vs-ollama-adoption-2026.md

---

## 9. Desktop app, engine, quantization, hardware backends

### Desktop app
- Native apps for **macOS and Windows** (download at ollama.com/download); an **Apps page** connects third-party apps — sign-in handled by Ollama [official: docs.ollama.com/cloud].
- **New desktop onboarding flow** on first launch (v0.32.15, Aug 2026) [secondary].
- **First-run setup** in CLI (`ollama`) offering sign-in or local-only; state shared with desktop app (v0.34.2, Sep 2026) [secondary].
- `ollama://apps` deep links open the desktop Apps page (v0.34.2) [secondary].
- **ChatGPT Desktop integration** (v0.34.0, Sep 2026): use Ollama models inside ChatGPT Desktop, configured from the Ollama macOS app [official].
- **Claude Desktop** configurable with Ollama as third-party gateway provider (v0.33.0, Aug 2026) [secondary].
- Cached resolved model metadata → TTFT ~995 ms → ~524 ms (v0.32.15 benchmarks) [secondary].

### Engines
- **llama.cpp**: direct integration since v0.30.0 (Jun 2026) — no longer via GGML layer; GGUF-native [secondary].
- **MLX runner** (Apple Silicon): DFlash speculative decoding, NVFP4 support, MLX safetensors model creation GA in v0.34.1 [official/secondary].
- **MLX-C** referenced in v0.33.3 engine updates [official].

### Hardware backends & GPU support [official: docs/windows.mdx unless noted]
- **Windows** (10 22H2+, native app, no admin required): NVIDIA CUDA (551.61+ drivers), AMD **ROCm v7 / HIP7** driver stack, or **Vulkan** for AMD Radeon; Vulkan is enabled by default and the recommended fallback where ROCm v7 isn't exposed (e.g. RDNA2/RX 6000) [official: https://github.com/ollama/ollama/blob/HEAD/docs/windows.mdx].
- **macOS Apple Silicon**: Metal (native); **macOS Intel**: CPU only [secondary: whisperjav research].
- **Linux x64**: CUDA, ROCm, Vulkan (experimental); **Docker**: CUDA, ROCm [secondary].
- **NVIDIA Jetson**: CC 87 / CUDA v13 support (v0.30.11) [official].
- **Windows on Arm**: Ollama ships Windows-on-Arm builds (noted working on NVIDIA RTX Spark / N1X at launch, Jun 2026) [secondary: awesomeagents.ai].
- GPU discovery: two-phase bootstrap, multi-backend detection (CUDA/ROCm/Metal/Vulkan) with dedup, auto VRAM-based layer offloading, CPU fallback [secondary].
- Mixed iGPU/dGPU control: `GGML_VK_VISIBLE_DEVICES` (Vulkan indices); fixed inverted iGPU/dGPU Vulkan classification on Windows hybrid graphics (v0.30.11) [official].
- Known 2026 pain points (community): Vulkan backend numerically wrong outputs on RDNA 4 (gfx1201, e.g. RX 9070 XT) — community workarounds use standalone llama.cpp ROCm 7 builds [secondary: tokenpal doc, Apr 2026 debug session].

### Quantization & model creation
- Primary format **GGUF**; Q4_K_M the community sweet spot [secondary].
- **`ollama create` from MLX safetensors no longer experimental** (v0.34.1); **GGUF creation now requires llama.cpp tooling** for conversion/quantization [official].
- **NVFP4** models supported on MLX (Metal quality bug fixed v0.32.5) [secondary].
- **ModelOpt** checkpoints with float32 global scale: fused kernel → +7.9% prefill on qwen3.6:27b (M5 Max) in v0.32.10-rc0 [secondary].
- Speculative decoding: unified/tuned in mlxrunner (v0.30.11); draft-model output heads quantized at requested type (v0.32.4); memory-growth fix for MLX speculative decoding (v0.34.2) [official/secondary].
- Hugging Face direct: `ollama run hf.co/user/repo:Q4_K_M` [secondary].

---

## 10. Pricing — consolidated

| Offering | Price | Since / observed | Provenance |
|---|---|---|---|
| Ollama local (self-hosted) | **Free** (MIT open source) | standing | [independent] |
| Cloud Free | $0 (starter credits, starter models, 1 concurrent) | Sep 2026 | [secondary] |
| Cloud Pro | **$20/mo** ($60 usage, 3 concurrent, 50x free) | Sep 2026 (also $20/mo in Jul 2026) | [secondary] |
| Cloud Max | **$100/mo** ($300 usage, 10 concurrent, 250x free) | Sep 2026 | [secondary] |
| Cloud Team | **$500/mo** ($1,000 shared) — conflicts with $25/seat/mo (5-seat min) observed 28 Aug 2026 | Aug–Sep 2026 | [secondary, conflict flagged] |
| Extra usage credits | pay-as-you-go at per-token model rates | Sep 2026 | [secondary] |
| Enterprise | custom | 2026 | [secondary] |
| Off-peak discount | 50% off DeepSeek V4 (peak = 2x, 12:00–18:00 UTC Mon–Fri) | Sep 2026 | [secondary] |

---

## 11. Competitive positioning in 2026 coverage

### The fractured market (2026 consensus)
- "The one-size-fits-all approach to offline AI no longer works" — **LM Studio** owns the desktop/GUI lane, **vLLM** owns server throughput; Ollama remains the developer default [secondary: blazetrends, ~Jul 2026]. Source: https://blazetrends.com/lm-studio-and-vllm-challenge-ollama-for-local-ai-dominance-the-best-alternatives/
- Ranked comparison (Aug 2026, editorial scores): **#1 Ollama** (dev API + broad library; MIT; ~1-min setup), **#2 vLLM** (production concurrency, NVIDIA), **#3 LM Studio** (GUI discovery), #4 llama.cpp (engine-level control), #5 LocalAI [secondary: markaicode]. Source: https://markaicode.com/best/best-local-llm-inference-tools-production-2026/

### Ollama's documented weaknesses
- **Concurrency**: processes requests sequentially by default (llama.cpp heritage) — queues rather than batches; gap vs vLLM "small at one user and grows sharply past ~4–8 concurrent requests" [secondary: markaicode].
- **Throughput claim**: "vLLM pushes up to 2.6x the throughput of Ollama on the same hardware" [secondary: blazetrends — single source, treat as [unverified] magnitude].
- **No built-in auth / multi-tenancy**: "reasonable default for small production workloads… provided you put a gateway in front for rate limiting and auth, since Ollama has no built-in multi-tenant controls" [secondary: markaicode].
- **Memory**: "LM Studio vs Ollama 2026: 5x memory gap [tested]" headline (Sep 2026) claims LM Studio wins on memory efficiency for desktop use [secondary: tech-insider.org — headline-level, methodology not reviewed here].

### LM Studio
- Proprietary; **free for personal and commercial use since July 2025**; paid **enterprise tier with SSO/admin features** [secondary: tech-insider.org, Sep 2026].
- Versions: 0.4.20 (Jul 2026), 1.0.9 (Aug 2026) [secondary: localist news].
- Strengths: polished GUI model browser, chat UI, visual parameter controls, local OpenAI-compatible server; best for non-CLI teams and CI-adjacent desktop testing [secondary].
- Source: https://tech-insider.org/lm-studio-vs-ollama-2026/

### vLLM
- Production serving standard: batching, multi-user throughput, GPU servers; v0.26.0 released 26 Jul 2026 (411 commits, 212 contributors) [secondary: localist news].
- Maker **Inferact** raising as an open-source inference company alongside Ollama's round [independent: TechCrunch].
- Positioning: "Choose vLLM when local experimentation becomes real serving" [secondary: sabbirz].

### Ecosystem notes
- **llama.cpp** is Ollama's engine (direct since v0.30.0) and simultaneously its low-level competitor; 123,903 stars (Aug 2026) [secondary].
- **Open WebUI** (148,767 stars) is the standard chat frontend over Ollama [secondary].
- **OpenClaw** (Jan 2026 breakout) was Ollama's business proving point; bundled web search (OpenClaw provider) shipped in Ollama itself Apr 2026 [vendor-reported/secondary].
- 2026 hardware tailwinds for local AI cited with Ollama: AMD Strix Halo mini PCs (128 GB unified), Mac Mini M4, NVIDIA RTX Spark Windows-on-Arm builds [secondary].

---

## 12. Explicit uncertainties and gaps

1. **"Turbo" branding** — could not confirm a product literally named "Ollama Turbo" in 2026 sources; the cloud product is consistently "Ollama Cloud" with `:cloud` models. Possibly a legacy/coloquial name. [unverified — do not assert].
2. **Team pricing conflict** — $500/mo ($1,000 shared) vs $25/seat/mo (5-seat min), observed ~12 days apart (28 Aug vs 9 Sep 2026). Unresolved; likely a page/packaging change mid-transition to per-token billing. [flagged].
3. **Per-token transition date** — the GPU-time→per-token switch is bracketed (Jul 2026 → Sep 2026) but the exact announcement date was not recovered from an official source. [unverified].
4. **v0.31.x** — versions mentioned with a conflicting date (Apr 22 2026); no release-note content recovered. [unverified].
5. **52M monthly downloads (Q1 2026)** and 135K GGUF models on HF — single-source research notes, methodology unclear. [unverified].
6. **Laguna model provenance** — appears across official release notes (support added/fixed), but the model family's vendor/line was not verified. [unverified].
7. **Desktop app versioning** — no separate desktop-app version numbers found; app updates ship with engine releases. [gap].
8. **vLLM 2.6x throughput claim** — single secondary source, no methodology reviewed. [unverified magnitude].
9. **"5x memory gap" LM Studio vs Ollama** — headline only; methodology not reviewed. [unverified].
10. Off-peak pricing and per-model rates are from third-party docs verified against ollama.com/pricing on 8–9 Sep 2026 — accurate as of then, but the pricing page changes frequently (one source explicitly advises rechecking). [secondary, time-sensitive].

---

## 13. Key sources

- Official releases: https://github.com/ollama/ollama/releases/tag/v0.34.0 ; https://github.com/ollama/ollama/releases/tag/v0.33.3 ; https://github.com/ollama/ollama/releases/tag/v0.34.1 ; https://github.com/ollama/ollama/releases/tag/v0.30.11-rc0 ; https://github.com/ollama/ollama/releases/tag/v0.34.2
- Official docs: https://docs.ollama.com/cloud ; https://github.com/ollama/ollama/blob/HEAD/docs/windows.mdx
- Official library (sample verified page): https://ollama.com/library/deepseek-v4.1-flash
- TechCrunch (funding/adoption): https://techcrunch.com/2026/07/09/popular-open-source-ai-developer-tool-ollama-raises-65m-grows-to-nearly-9m-users/
- Adoption ranking: https://github.com/dappros/state-of-conversational-ai/blob/HEAD/2026-q3/report/report.md
- Pricing (secondary, Sep 2026): https://techtoheart.com/ollama-clouds-off-peak-pricing-turns-scheduling-into-a-cost-lever/ ; https://github.com/mindstone-agent/mindstone-agent/blob/HEAD/OLLAMA_CLOUD_QWEN3.5.md ; https://github.com/carldog/wobblebot/blob/HEAD/docs/implementation/ollama-cloud.md
- Enterprise/team (secondary): https://github.com/rizquuula/coding-plan/blob/HEAD/.claude/skills/provider-ollama/references/plans.md
- Comparisons (secondary): https://markaicode.com/best/best-local-llm-inference-tools-production-2026/ ; https://blazetrends.com/lm-studio-and-vllm-challenge-ollama-for-local-ai-dominance-the-best-alternatives/ ; https://tech-insider.org/lm-studio-vs-ollama-2026/ ; https://www.sabbirz.com/blog/ollama-vs-lm-studio-vs-llama.cpp-vs-vllm
- Release coverage (secondary): https://appselfhost.com/ollama-v0-21-2-rc0-released-bundled-web-search-and-structured-outputs-docs-land-in-new-rc/ ; https://github.com/bokiko/localist/blob/HEAD/news/2026-07.md ; https://github.com/bokiko/localist/blob/HEAD/news/2026-08.md ; http://dev.to/jtorchia/ollama-v0342-vs-v0341-a-targeted-mlx-fix-not-the-same-regression-mkh
- Model roundups (secondary): https://medium.com/@mmoufakkir2/the-best-local-ollama-models-in-2026-without-pretending-one-model-wins-everything-d1d2645737d6 ; https://github.com/grikomsn/ollama-cloud-copilot-chat/blob/HEAD/docs/models.md


---

## Part C — LM Studio + local inference hardware + GGUF quantization

**Research date:** September 22, 2026 (cutoff)
**All deliverables in English per project rule.** Provenance tags used throughout:
`[official]` = the vendor/project's own site, docs, blog, changelog · `[vendor-reported]` = numbers stated by the vendor about its own product · `[independent]` = independent review/benchmark/guide · `[secondary]` = aggregator, blog, or derived summary · `[unverified]` = could not be cross-confirmed; do not treat as fact.

---

## PART 1 — LM STUDIO (Element Labs)

### 1.1 Latest version & release history (2026)

- **Latest release: LM Studio 0.4.24 — September 9, 2026 (Build 1)** [official] (https://lmstudio.ai/changelog/lmstudio/lmstudio-v0.4.24)
  - Release notes: new LM Studio Engine Protocol with advanced llama.cpp argument overrides for GGUF model loading; improved DSpark and DFlash assistant-drafter compatibility heuristics; fixed Loaded Instances UI context-length display; fixed `/api/v1/chat` splitting text and image inputs into separate user messages; improved sign-in reliability. [official]
- Previous: **0.4.22 — August 28, 2026** [official] (https://lmstudio.ai/changelog/lmstudio/lmstudio-v0.4.22)
  - Support for DFlash, DSpark, and MTP assistant drafters (speculative decoding; requires llama.cpp engine ≥ 2.29.1); images returned by tools in OpenAI-compatible `/v1/responses`, `/v1/chat/completions`, and Anthropic-compatible `/v1/messages`; Linux fixes (AppImage without FUSE 2, Chromium sandbox, `lms` wake-up, .deb icons, in-app updates, Wayland keyboard shortcuts). [official]
- **0.4.11** added support for updated Gemma 4 chat template; **0.4.6 (Feb 27, 2026)** introduced LM Link (remote instance access via end-to-end encryption in partnership with Tailscale) and Qwen3.5 RAG jinja rendering fix. [official] (https://lmstudio.ai/changelog/lmstudio/lmstudio-v0.4.6)
- **0.4.16 — June 4, 2026**: shipped with "Locally", a native iPhone/iPad app paired with LM Link — a remote-access layer letting a phone use models running on the user's Mac over an end-to-end-encrypted Tailscale mesh; chat history stays on-device; only a device discovery list touches LM Studio servers; iPhone/iPad only at launch; free during preview. [secondary] (https://www.digitalapplied.com/blog/lm-studio-locally-lm-link-iphone-local-llm-2026)
- **0.4.0 (Jan 28, 2026)** — major re-architecture [official]: (https://lmstudio.ai/blog/0.4.0)
  - GUI separated from core: the core ("llmster" daemon) can run standalone on Linux boxes, cloud servers, GPU rigs, even Google Colab — i.e., a server-native, headless deployment of LM Studio.
  - Parallel requests with continuous batching (high-throughput serving instead of queueing).
  - New stateful native REST API (`/api/v1/*`), including MCP via API, stateful chats, API-token auth, model download/load/unload endpoints.
  - Refreshed UI: chat export, split view, developer mode, in-app docs.
  - Headless install: `curl -fsSL https://lmstudio.ai/install.sh | bash` / Windows `irm https://lmstudio.ai/install.ps1 | iex`; commands `lms daemon up`, `lms get <model>`, `lms server start`, `lms chat`, `lms runtime update llama.cpp` (and `lms runtime update mlx` on macOS). [official]
  - Self-hosted server mode via `lms server` — positioned against Ollama as the server option. [secondary] (http://www.howtogeek.com/you-can-make-a-self-hosted-ai-server-with-lm-studio-040/)

### 1.2 LM Studio Bionic (AI agent app)

- **LM Studio Bionic** — separate standalone desktop app (Mac, Windows, later Linux x64/ARM64 in 1.1.2), announced July 16, 2026 on the company blog [official/secondary]: (https://www.webpronews.com/lm-studios-bionic-agent-puts-open-models-to-work-on-your-desktop/), (https://9to5mac.com/2026/07/16/lm-studio-expands-beyond-chat-with-bionic-a-new-ai-agent-app-for-open-models/)
- An agentic app that works on code projects, documents, research with file access; uses local models or **LM Studio Secure Cloud** for heavier tasks (Zero Data Retention policy; US-based servers); cloud use requires an LM Studio account with billing configured. [secondary]
- **Bionic 1.1.5 — September 19, 2026** (latest at cutoff) [official]: (https://lmstudio.ai/changelog)
  - "[Mac only] Ultra fast Qwen3.8 inference using a new engine: Splash (by Inco AI)" — a new inference engine for Apple Silicon from Inco AI.
  - Prior versions: 1.1.4 (Sep 17, 2026) — agent introspection, llama.cpp 2.41.0 extension packs, tool-call argument streaming; 1.1.3 (Sep 15, 2026) — Linux voice transcription, MTP speculative decoding for more models, per-layer GGUF expert counts, MCP organization management; 1.1.0 (Aug 27, 2026) — "2–2.75× faster prompt processing on M5 Macs", Muse Glimmer tool calling for MLX models; 1.1.1/1.1.2 (Aug 31 / Sep 8, 2026) — Linux builds, `lms load --auto`, llama.cpp 2.31.x packs. [official]
- Bionic cloud models: GLM 5.2, Kimi K2.7 Code (code projects), Kimi K3, Z.ai GLM-5.3-Flash (320B MoE, 18B active, 1M context; served from US servers with ZDR; reported 9–10× cheaper than GLM-5.2) [secondary]: (https://9to5mac.com/2026/08/26/lm-studio-adds-glm-5-3-flash-to-bionic-with-image-support-and-1m-token-context/)
- Offline voice: ships with Mistral Voxtral model for multilingual offline transcription; a "voice keyboard" works across apps. [secondary]

### 1.3 SDKs & developer surfaces

- **lmstudio-js** (TypeScript): `npm install @lmstudio/sdk`; `import { LMStudioClient } from "@lmstudio/sdk"`. Source: https://github.com/lmstudio-ai/lmstudio-js. The app itself uses the same public APIs. Works from Node and (with CORS) the browser. [official] (https://lmstudio.ai/blog/introducing-lmstudio-sdk)
- **lmstudio-python**: `pip install lmstudio`; `import lmstudio as lms`. Sync convenience API + session-based async API. [official]
- Core SDK APIs: `.respond()` chat, `.act()` agentic tool use, structured output (Pydantic/zod/JSON schema), image input, speculative decoding (MLX and llama.cpp), `.complete()`, `.embed()`, model load/unload in memory, low-level config (GPU, context length). Auto engine selection (llama.cpp vs MLX) with per-hardware parameters chosen automatically. [official]
- REST: OpenAI-compatible `/v1/chat/completions`, `/v1/responses` (with `reasoning.effort` for `openai/gpt-oss-20b`), `/v1/embeddings`; Anthropic-compatible `/v1/messages` (added in 0.4.1); native stateful API `/api/v1/chat` with MCP support and auth tokens (added in 0.4.0). API changelog: [official] (https://lmstudio.ai/docs/developer/api-changelog)
- CLI `lms`: `lms load --estimate-only <model>` (GPU + total memory estimates, honoring `--context-length`/`--gpu`), `lms chat` (Ctrl+C interrupt), `lms ps --json`, `lms ls --variants`. [official]

### 1.4 Model catalog (notable additions)

- In-app model search wired to Hugging Face with memory-requirement estimates before download; supports GGUF (llama.cpp) and MLX formats. [secondary] (https://medium.com/@nishilbhave/lm-studio-in-2026-download-models-run-local-llms-vs-ollama-b30567df17f8)
- 2026 model landmarks referenced in official materials: `openai/gpt-oss-20b` (reasoning effort support in `/v1/responses`), Gemma 4 (0.4.11 chat-template support), Qwen3.5 (RAG fix in 0.4.6), Qwen3.8 (Splash engine in Bionic 1.1.5), NVIDIA Nemotron-Nano-v2 (0.3.25). [official]
- Company history context: launched May 2023 by Element Labs; old commercial license scrapped — free at home and at work since ~July 2025. [secondary] (https://www.aixploria.com/en/lm-studio-ai/), (https://medium.com/@nishilbhave/lm-studio-in-2026-download-models-run-local-llms-vs-ollama-b30567df17f8)
- One secondary source reports "LM Studio crossed millions of downloads worldwide" as of 2026 [secondary — no vendor-published user count found at cutoff; flag as unverified for exact figures]: (https://medium.com/@nishilbhave/lm-studio-in-2026-download-models-run-local-llms-vs-ollama-b30567df17f8)

### 1.5 Hardware acceleration

- Inference engines: **llama.cpp** (primary, CPU/GPU, all platforms) and **Apple MLX** (Apple Silicon M1–M5); both are independently downloadable runtimes — engine updates ship separately from app updates (`lms runtime update`). [secondary cross-checked with official 0.4.0 install docs] (https://github.com/aterrylu/autonomos/blob/HEAD/docs/research/desktop-shells/lm-studio.md)
- Backend coverage on desktop: CUDA (NVIDIA), Metal (Apple), Vulkan (cross-vendor, incl. AMD/Intel); changelog entries also reference ROCm builds and multi-GPU selection bugs on CUDA 12 / ROCm / Vulkan. [official] (https://lmstudio.ai/changelog/lmstudio/lmstudio-v0.4.22)
- NPU: LM Studio runs on the user's GPU or NPU per third-party coverage; NPU-specific offload reported in community NPU laptop tests (see §2.4). Flag: exact NPU backends in LM Studio are not enumerated in official docs found at cutoff [secondary/unverified]. [secondary] (http://www.howtogeek.com/you-can-make-a-self-hosted-ai-server-with-lm-studio-040/)

### 1.6 Pricing

- **Desktop app and local use: free** (home and work). [secondary — widely reported, incl. LM Studio comparison coverage] (https://www.aixploria.com/en/lm-studio-ai/), (https://www.kunalganglani.com/blog/lm-studio-vs-ollama)
- One comparison site reports an **Enterprise tier** for teams needing LM Link multi-device workload routing and priority support — but this is not confirmed on LM Studio's public pricing pages at cutoff; treat as [unverified]. (https://www.kunalganglani.com/blog/lm-studio-vs-ollama)
- **LM Studio Secure Cloud**: pay-as-you-go via Cloud Credits; exact pricing "not published" as of July 2026 per a Bionic developer guide; "Bionic Pass" subscription announced as "coming soon". LM Link free tier covers up to 5 devices. [secondary] (https://learning.christiandrapatz.de/lmstudio-en.pdf)
- LM Studio acquisition of Locally AI (mobile, Adrien Grondin) — strategic push to iPhone/iPad on-device AI; iOS local models realistically 1B–3B at practical speeds. [secondary] (https://evermx.com/case/lm-studio-acquires-locally-ai-mobile-on-device-ai)

---

## PART 2 — LOCAL INFERENCE HARDWARE LANDSCAPE (2026)

### 2.1 Recommended GPUs / VRAM for 2026 flagship open models

Community-maintained VRAM math (llama.cpp/GGUF ecosystem) — the standard estimator is `VRAM ≈ params × bytes-per-weight × ~1.1–1.2` (KV cache + overhead):

| Model / quant | Approx weights | Practical VRAM incl. context | Source |
|---|---|---|---|
| 7–8B, Q4_K_M | ~5 GB | ~6 GB (entry: 8–12 GB card) | [independent] (https://github.com/high-altitude-ai/local-llm-rig) |
| 13–14B, Q4_K_M | ~8.7–9.1 GB | ~10 GB | [independent] same + (https://github.com/casteldazur/awesome-local-ai/blob/HEAD/guides/vram-requirements.md) |
| 32–34B, Q4_K_M | ~20 GB | ~22 GB | [independent] (https://github.com/high-altitude-ai/local-llm-rig) |
| 70B, Q4_K_M | ~40–43 GB file | ~45–48 GB loaded | [independent] (https://github.com/high-altitude-ai/local-llm-rig), (https://github.com/joyroy9454/aryvora/blob/HEAD/content/posts/run-ai-locally-ollama-llama-cpp-guide.md) |
| 70B, Q8_0 | ~78 GB | ~85 GB+ | [independent] (same) |
| 120B (gpt-oss-120B), MXFP4/NVFP4 | — | runs on DGX Spark 128GB at ~38 tok/s single user; ~505 tok/s batched on 2-unit cluster | [secondary] (https://emarque.co/collections/nvidia-dgx-spark) |

Key community takeaways [independent] (https://github.com/high-altitude-ai/local-llm-rig):
- Q4_K_M is the default sweet spot, not a compromise (small quality delta vs FP16, 4× VRAM saving).
- The painful gap is 24 GB → 48 GB: 24 GB comfortably runs 32B-class models; 70B at usable quality needs ~48 GB (2× cards or unified-memory silicon).
- Long context eats VRAM: 32k context on 8B adds gigabytes of KV cache; budget +20–30% for long-doc use.
- A 70B Q4 model "wants roughly 40 GB of memory", so a 24 GB GPU or a 64 GB+ Mac is the practical entry point for big models. [independent] (https://medium.com/@nishilbhave/local-llms-in-2026-which-runtime-to-run-and-the-hardware-you-need-a88450dece2e)

**GPU ladder for local inference in 2026 (community consensus):**
- 16 GB (RTX 5060 Ti 16GB, RX 9070 XT, Arc B580): up to ~13–14B at Q4, 20B MoE; 16 GB "enough for most use cases" but 30B+ models hit a ceiling. [independent] (https://www.compute-market.com/blog/rx-9070-xt-vs-rtx-5060-ti-local-ai-2026), (https://localaimaster.com/blog/rx-9070-xt-local-ai)
- 24 GB (RTX 4090, RTX 3090 used, RX 7900 XTX, Arc Pro B60): 32B-class comfortably; used RTX 3090 at $699–$999 is the community value pick for 24 GB. [secondary] (same)
- 32 GB (RTX 5090): 27B dense, 35B MoE comfortably; 70B only with aggressive quant/split offload. [independent] (https://github.com/sipeed/llmdev.guide/blob/HEAD/devices/nvidia-rtx-5090-32gb.md)
- 96 GB (RTX PRO 6000 Blackwell Workstation Edition): runs $8,500+; GPT-OSS-120B-class locally on one card. [secondary] (http://gpusmith.com/articles/en/pdfs/nvidia-dgx-spark-review-specs-performance.pdf)
- 128 GB unified (DGX Spark, Strix Halo systems, Mac Studio): the "run 70B+ without compromise" tier; 200B-class at FP4 on DGX Spark. [independent/secondary] (https://emarque.co/collections/nvidia-dgx-spark)

### 2.2 NVIDIA

**GeForce RTX 5090** (Blackwell GB202; released Jan 30, 2025, announced at CES Jan 6, 2025):
- 32 GB GDDR7, 512-bit bus, 1,792 GB/s (~1.79 TB/s) bandwidth; 21,760 CUDA cores; 680 5th-gen Tensor Cores; 104.8 TFLOPS FP32; 575 W TDP; MSRP $1,999 (Founders Edition). [independent] (https://www.runpod.io/articles/guides/nvidia-rtx-5090), (https://www.pcgamer.com/hardware/graphics-cards/nvidia-announces-the-rtx-50-series-led-by-the-usd1-999-rtx-5090-with-twice-the-performance-of-the-4090/)
- Street price reality: ~$2,500–$3,200 secondary market; one benchmark guide reports street ~$3,799 early 2026 due to GDDR7 constraints. Supply tight much of 2025–2026. [secondary] (https://github.com/sipeed/llmdev.guide/blob/HEAD/devices/nvidia-rtx-5090-32gb.md)
- Inference: highest decode throughput among consumer GPUs (bandwidth-bound decode); community numbers: Llama 7B Q4_0 ≈ 300 tok/s decode / ~15k tok/s prefill; Qwen3.5-35B-A3B int4 ≈ 194 tok/s (llama.cpp CUDA + Flash Attention). [independent] (same)
- NVFP4 hardware (dual-level scaling) is consumer-first on Blackwell; FP4 supported in TensorRT-LLM and being integrated into llama.cpp's CUDA backend as of July 2026 — an NVIDIA-exclusive advantage, no AMD consumer GPU supports FP4. [secondary] (https://www.compute-market.com/blog/rx-9070-xt-vs-rtx-5060-ti-local-ai-2026)

**RTX 5080**: 10,752 CUDA cores, 16 GB GDDR7, 960 GB/s, 360 W, MSRP $999. [independent] (https://www.pcgamer.com/hardware/graphics-cards/nvidia-announces-the-rtx-50-series-led-by-the-usd1-999-rtx-5090-with-twice-the-performance-of-the-4090/)
**RTX 5070 Ti / 5070**: 16 GB / 12 GB GDDR7; $749 / $549. [independent] (same)

**DGX Spark** (GB10 Grace Blackwell Superchip; formerly "Project Digits"):
- Status: launched Oct 15, 2025 (global availability); announced at CES Jan 2025 as Project Digits, renamed at GTC March 2025. [secondary] (https://www.techedt.com/nvidia-launches-dgx-spark-personal-ai-supercomputer-on-15-october), (https://www.aitooldiscovery.com/ai-infra/nvidia-dgx-spark-explained)
- Specs: 20-core Arm CPU (10× Cortex-X925 + 10× Cortex-A725), Blackwell GPU w/ 6,144 CUDA cores, 5th-gen Tensor Cores, 1 PFLOP FP4 (sparse), **128 GB LPDDR5x unified memory, 273 GB/s**, 4 TB NVMe, 150×150×50.5 mm, 1.2 kg, DGX OS (Ubuntu 24.04-based). [independent] (https://www.tomshardware.com/pc-components/gpus/nvidia-dgx-spark-review/2), (https://peterfalkingham.com/2026/09/18/academic-tech-gigabyte-ai-top-nvidia-dgx-spark-review/)
- Pricing: $3,999 MSRP at launch (Oct 15, 2025); **raised to $4,699 on Feb 23, 2026** (+$700, ~18%) citing LPDDR5x supply constraints; no hardware change. [secondary, NVIDIA forum statement relayed] (http://gpusmith.com/articles/en/pdfs/nvidia-dgx-spark-review-specs-performance.pdf)
- Performance (independent): LMSYS — GPT-OSS 20B in Ollama: ~2,053 tok/s prefill, 49.7 tok/s decode (~1/4 of RTX Pro 6000, slower than a single RTX 5090 at 8,519/205); 70B models ~35–45 tok/s; GPT-OSS-120B ~38 tok/s single-user; batched Llama 3.1 8B at batch 32 ≈ 368 tok/s decode. **The 273 GB/s bandwidth, not the 1-PFLOP headline, determines throughput** — consistent reviewer consensus. [secondary summarizing LMSYS/The Register/Raschka] (same)
- 2026 software progress: CES 2026 enterprise update claims up to 2.5× on key workloads vs launch (NVIDIA cites 2.6× on Qwen-235B across 2 units with NVFP4 + speculative decoding); multi-node clustering up to 4 units via Cluster Assistant (June 2026 release); ~200B params FP4 per unit, ~405B with two units over ConnectX-7 (200 GbE). [secondary] (https://emarque.co/collections/nvidia-dgx-spark)
- Caveats reported: thermal/power-delivery issues on early units (John Carmack publicly reported power capping ~100W of rated 240W and reboots under load; NVIDIA forums confirmed a known platform issue); consumer-class Blackwell GPU causes software-compat friction vs datacenter cards. [secondary] (http://gpusmith.com/articles/en/pdfs/nvidia-dgx-spark-review-specs-performance.pdf)
- Also announced: **RTX Spark** family (Windows AI laptops/desktops on "RTX Spark Superchip", up to 1 PFLOP FP4, up to 128 GB unified, from ASUS/Dell/HP/Lenovo/Microsoft/MSI — Fall 2026 target; distinct from DGX Spark). [secondary] (https://emarque.co/collections/nvidia-dgx-spark)
- **DGX Station** (GB300 Grace Blackwell Ultra Desktop Superchip): 20 PFLOP, 784 GB unified memory; 1T+ param models on one unit — the step above Spark. [secondary] (https://www.aitooldiscovery.com/ai-infra/nvidia-dgx-spark-explained)
- **RTX PRO 6000 Blackwell Workstation Edition**: 96 GB GDDR7, $8,500+ (HotHardware cited at ~$10,000 before host workstation). [secondary] (http://gpusmith.com/articles/en/pdfs/nvidia-dgx-spark-review-specs-performance.pdf)
- ServeTheHome coverage exists (Jetson Thor / DGX Spark ecosystem); STH's Thor coverage noted preliminary specs and bandwidth-sensitive benchmarks. [independent] (http://aiwiki.ai/wiki/jetson_thor)

### 2.3 AMD

**Radeon RX 9070 XT** (RDNA4, gfx1201):
- 16 GB GDDR6, 640 GB/s bandwidth, MSRP $599; street from ~$740 (early Aug 2026). Board power 304 W. [independent] (https://localaimaster.com/blog/rx-9070-xt-local-ai)
- Local inference: ROCm backend on Linux (gfx1150/gfx1201 target, ROCm 6.3+; ROCm 7 in newer coverage) delivers 35–42 tok/s on 8B models; Vulkan backend works cross-platform (~15% slower); llama.cpp's official ROCm Docker image natively supports RDNA4 (gfx1201). Community run: Qwen3.6-27B IQ3_M at 46 tok/s decode with MTP speculative decoding (llama.cpp + ROCm Docker, 16k context, 8-bit KV). [independent] (https://www.compute-market.com/blog/rx-9070-xt-vs-rtx-5060-ti-local-ai-2026), (https://midwest.social/post/48528597)
- vs RTX 5060 Ti 16GB ($429 MSRP): 9070 XT's 43% bandwidth advantage shows up ~linearly in tok/s (~52 vs ~33 tok/s on 14B Q4); the 5060 Ti buys you CUDA (ExLlamaV2, TensorRT-LLM, mainstream fine-tuning) and 124 W lower power. Verdict from independent coverage: choose 9070 XT for Linux/fastest-16GB-inference or gaming; choose 5060 Ti for the CUDA ecosystem. [independent] (https://localaimaster.com/blog/rx-9070-xt-local-ai)
- No FP4 support on any AMD consumer GPU (NVIDIA-exclusive). [secondary] (https://www.compute-market.com/blog/rx-9070-xt-vs-rtx-5060-ti-local-ai-2026)
- Easiest path for AMD users: LM Studio with Vulkan backend (Windows & Linux) — no ROCm setup; ROCm only to squeeze the last bit of speed. [independent] (https://github.com/bokiko/localist/blob/HEAD/guides/amd-gpu.md)
- 24 GB alternative: RX 7900 XTX — runs 32B-class models a 16 GB card cannot hold; the community "spend more, get 24 GB" option. [secondary] (https://localaimaster.com/blog/rx-9070-xt-local-ai)

**Ryzen AI Max+ 395 "Strix Halo"** (16× Zen 5, 40-CU Radeon 8060S iGPU / gfx1151, up to 128 GB LPDDR5x-8000 unified):
- The Windows answer to Apple Silicon for local LLM: 128 GB unified memory with ~256 GB/s theoretical (~215 GB/s measured) bus. Massive capacity, modest bandwidth — token generation is bandwidth-bound, so expect single-digit tok/s on dense 70B, but huge models fit: MoE models fly (Qwen3 30B MoE ~66–72 tok/s; LFM2-24B-A2B ~109 tok/s; ~100 tok/s on 30B MoE per setup guides). 70B (Q4) ~15–20 tok/s (ROCm/rocWMMA, fits entirely in memory). [independent] (https://github.com/nelisw/framework-strix-halo-llm-setup), (https://codersera.com/blog/amd-strix-halo-ryzen-ai-max-local-llm-setup-2026/), (https://digitalarchitects.hr/insights/amd-ryzen-ai-max-395-local-llm/)
- Practical setup (2026): allocate ~96 GB to iGPU via AMD Adrenalin (Windows) or GTT kernel parameter + small BIOS framebuffer (Linux, kernel ≥ 6.16.9/6.18); run LM Studio / Ollama / llama.cpp on Vulkan backend (most reliable as of mid-2026; ROCm HIP path improving but had stability issues; community benchmarks show Vulkan outperforming HIP). [independent] (https://codersera.com/blog/amd-strix-halo-ryzen-ai-max-local-llm-setup-2026/), (https://digitalarchitects.hr/insights/amd-ryzen-ai-max-395-local-llm/)
- Price points: Framework Desktop ~$1,999 (128 GB), GMKtec mini PCs ~$1,499–$2,500, AMD Ryzen AI Halo dev platform $3,999; ASRock Industrial AI BOX-A395 announced Mar 17, 2026 (200×100×232 mm; pricing unannounced). [secondary] (https://digitalarchitects.hr/insights/amd-ryzen-ai-max-395-local-llm/), (https://videocardz.com/newz/asrock-industrial-launches-ai-box-a395-with-ryzen-ai-max-395-and-128gb-lpddr5x)
- Price delta vs DGX Spark: Strix Halo platforms generally undercut Spark by $1,000–$2,000 at comparable 128 GB. [secondary] (http://gpusmith.com/articles/en/pdfs/nvidia-dgx-spark-review-specs-performance.pdf)

**ROCm for local inference (2026 status):** supported on RDNA4 (gfx1200/gfx1201) and Strix Halo (gfx1151); official llama.cpp ROCm Docker images; LM Studio ships ROCm builds. Maturing but still behind CUDA in ecosystem (no Unsloth, no TensorRT-LLM equivalent); fine-tuning on ROCm via PyTorch works but slower than CUDA. [independent] (https://www.compute-market.com/blog/rx-9070-xt-vs-rtx-5060-ti-local-ai-2026), (https://localaimaster.com/blog/rx-9070-xt-local-ai)

### 2.4 Intel

**Arc B580** (Battlemage Xe2, 12 GB, $249 MSRP):
- 12 GB at $249 — but with a "software tax". Three practical paths [independent] (https://runaihome.com/blog/intel-arc-b580-local-ai-2026/):
  1. llama.cpp **Vulkan backend** — no Intel-specific tooling, works on Windows/Linux with stock builds: Llama 3.1 8B Q4_K_M ~40–42 tok/s (parity with RTX 3060 12 GB).
  2. **IPEX-LLM Portable ZIP** (Windows, zero-install) — SYCL binary via patched Ollama fork; counterintuitively *slower* than Vulkan (IPEX-LLM issue #12991); Qwen2.5 14B Q4_K_M ~15–20 tok/s.
  3. **IPEX-LLM native install on Linux** (full oneAPI stack) — Qwen2.5 14B Q4_K_M 32–38 tok/s.
- Flag: IPEX-LLM was **archived Jan 28, 2026** for "known security issues" (read-only) per an engine-status research doc — the recommended path is shifting to upstream llama.cpp SYCL and Intel's llm-scaler-vllm fork. [secondary/unverified — from a community research doc; treat with caution] (https://github.com/chriscorbell/llm-server/blob/HEAD/docs/research/2026-09-07-engines-on-battlemage.md)
- Community benchmarks on Battlemage (Aug–Sep 2026): llama.cpp SYCL is the fastest llama.cpp path for dense models on B70/B580-class hardware; Vulkan the most robust; vLLM-XPU (Intel fork) best measured numbers with patches; SGLang-XPU "not ready". [independent/community] (same)

**Intel Core Ultra NPUs:** OpenVINO is the primary local path (Intel CPUs, GPUs, NPUs); llama.cpp has an OpenVINO backend "in progress". NPU LLM inference remains marginal vs iGPU/CPU paths. See §2.6.

### 2.5 Apple Silicon

**Unified memory advantage:** the entire model + KV cache lives in one pool (CPU/GPU/NPU coherent), so 64–128 GB+ Macs run 70B-class models that no single consumer dGPU can hold. This is the same "capacity over bandwidth" trade as DGX Spark/Strix Halo — and the reason Apple machines remain the default local-LLM developer machine. [independent] (https://medium.com/@nishilbhave/local-llms-in-2026-which-runtime-to-run-and-the-hardware-you-need-a88450dece2e)

**M4 family (2026 status):** M4 Max 128 GB — community MLX benchmarks (vllm-mlx project): Qwen3-0.6B-8bit 417.9 tok/s, Llama-3.2-3B-4bit 205.6 tok/s, Qwen3-30B-A3B-4bit 127.7 tok/s (~18 GB) — single-stream greedy decode. [independent] (https://github.com/anisoptera/vllm-mlx-upstream)

**M5 family (announced Oct 2025 per coverage; shipping in MacBook Pro/iPad Pro/Vision Pro):**
- Specs per press coverage: 3rd-gen 3 nm; 10-core GPU with a dedicated Neural Accelerator per core (>4× peak GPU AI compute vs M4); 16-core Neural Engine; unified memory bandwidth 153 GB/s (base M5), up to 32 GB config; M5 Max: up to 128 GB, 614 GB/s, 40 GPU cores / 40 Neural Accelerators; M5 Pro: 64 GB, 307 GB/s. [secondary] (https://tecknexus.com/apple-m5-the-next-leap-in-on‑device-ai/), (https://www.banandre.com/blog/apple-m5-max-neural-engine-local-llm-inference-game-changer)
- Apple claims (Machine Learning Research blog, MLX benchmarks): up to 4× LLM prompt-processing on M5 Max vs M4 Max; M5 Pro 3.9× vs M4 Pro; up to ~6.9× vs M1 Pro. [vendor-reported] (same)
- LM Studio/Bionic 1.1.0 notes "2–2.75× faster prompt processing on M5 Macs"; Bionic 1.1.5 adds "ultra fast Qwen3.8 inference" via **Splash**, a new Apple-Silicon inference engine by Inco AI. [official]
- **MLX ecosystem (2026):** Apple's MLX framework is the native path (Metal kernels, unified memory, no model conversion); vLLM-for-MLX projects add continuous batching, multimodal, MCP tool calling, Claude Code support; `lms runtime update mlx` keeps LM Studio's MLX engine current; LM Studio also exposes MLX prompt disk-cache controls. [independent/official]

### 2.6 NPU / edge (real status 2026)

The honest 2026 picture: NPUs matter for Copilot+ OS features (Studio Effects, Recall-type workloads) and small on-device assistants — **not** for serious local LLM inference, where GPU/iGPU/CPU still dominate:

- **Qualcomm Snapdragon X Elite (Hexagon NPU, 45 TOPS):** a community benchmark (LM Studio 0.3 + Ollama/DirectML, Win 11 24H2, Llama 3.1 8B Q4_K_M) measured ~26–31 tok/s on Snapdragon X Elite (QNN) vs 19–24 tok/s on Ryzen AI 9 HX 375 (DirectML, partial NPU) vs 13–17 tok/s on Intel Core Ultra 7 258V (OpenVINO, partial) — but TOPS ≠ usable throughput; driver/runtime maturity decides. [secondary — single-blogger benchmark, treat as indicative] (https://medium.com/@neilandrews1983/best-npu-laptops-2026-llama-mistral-benchmark-rankings-for-uk-buyers-c041a1804219)
- **AMD Ryzen AI NPUs (XDNA 2, up to 50 TOPS):** Linux XDNA driver stack matured enough in 2026 to run LLMs on the NPU directly (Phoronix/Michael Larabel testing) via ONNX Runtime + Vitis AI — a milestone, but the `ryzenai` backend in community libraries was still "awaiting hardware validation" as of mid-2026. [secondary] (https://www.webpronews.com/amds-ryzen-ai-npus-can-now-run-llms-locally-on-linux-heres-what-that-means/), (https://github.com/quintindk/crier)
- **Intel Core Ultra NPU:** OpenVINO path; llama.cpp OpenVINO backend still "in progress". [independent] (https://github.com/lunncing/llama.cpp-adaptive-kv-streaming — backend table, via search result)
- **Apple Neural Engine:** used via Core ML / MLX; Apple's on-device AI story is GPU+Neural-Engine combined, not NPU-only. [secondary]
- **Snapdragon X2 generation:** one AI-laptop comparison reports a "Snapdragon X2 Elite Extreme" with 80 TOPS NPU (highest NPU score in its table) but weaker memory bandwidth/GPU for memory-intensive inference. [secondary — single source, unverified] (https://medium.com/@raulasla/the-ai-laptop-chip-guide-nobody-wrote-how-to-actually-compare-processors-in-2026-d63b4a9476f2)
- Bottom line: NPUs in 2026 are a complement for always-on/assistant workloads; local LLM inference still runs on GPU (CUDA/Metal/Vulkan), with the NPU contributing only where the runtime explicitly supports it (QNN/OpenVINO/Vitis AI). Flag any vendor TOPS figure as marketing input, not inference throughput.

### 2.7 Jetson / edge (2026)

**Jetson AGX Thor (T5000)** — NVIDIA's "physical AI" flagship (robotics/humanoids), not a desktop LLM box:
- Specs: Blackwell GPU, 2,070 TFLOPS FP4 sparse, 128 GB LPDDR5X, 40–130 W configurable, 14-core Arm Neoverse-V3AE CPU, Multi-Instance GPU; ~7.5× AI compute and ~3.5× energy efficiency claims vs AGX Orin (note: mixes FP4-sparse vs INT8-sparse in the comparison). **Dev kit $3,499**; bulk module $2,999. [independent] (http://aiwiki.ai/wiki/jetson_thor), (https://www.techradar.com/pro/nvidia-quietly-unveiled-its-fastest-mini-pc-ever-capable-of-topping-2070-tflops-and-if-you-squint-enough-you-might-even-think-it-looks-like-an-rtx-5090)
- Developer kit released Aug 25, 2025 (shipments from Nov 20, 2025 per pre-order coverage); runs JetPack 7 (Ubuntu 24.04, kernel 6.8); JetPack 7.1 current production release. [independent] (https://github.com/liunix61/awesome-embedded-ai-stack/blob/HEAD/hardware-landscape/jetson.md), (https://twowintech.com/nvidia-jetson-agx-thor-full-analysis/)
- July 2026: NVIDIA announced lower **T3000 / T2000** modules (smaller memory/form factor, Q1 2027 availability; pricing unannounced; CNX/ServeTheHome flagged specs as preliminary). [independent] (http://aiwiki.ai/wiki/jetson_thor)
- LLM angle: 128 GB lets Thor run Llama-70B-class models locally, but ~273 GB/s bandwidth (like DGX Spark) limits token throughput; community consensus: relevant for 70B+ edge inference and VLA/robotics workloads, overkill for simple robotics (Orin Nano class suffices). [independent] (https://github.com/alpininsight/capi-provider-ssh/blob/HEAD/docs/roadmap/nvidia-jetson-edge-devices.md)
- Hands-on: HotHardware/ServeTheHome demos of GR00T N1 on Thor; wayin.ai tested local LLMs/VLMs on the dev kit (128 GB, 273 GB/s bandwidth noted as "slow for GPU memory, fast for CPU/RAM"). [independent] (http://aiwiki.ai/wiki/jetson_thor), (https://wayin.ai/wayinvideo/s/chshare0uMzeE0gKLh/)

---

## PART 3 — GGUF QUANTIZATION BENCHMARKS (2026)

### 3.1 Quality vs size: the standard numbers

**Measured perplexity/KL data (ikawrakow / Artefact2 measurements on Mistral-7B-class models; bits-per-weight, KL-divergence median, ln(PPL(Q)/PPL(base)))** [independent — community measurements, widely cited as the reference] (https://gist.github.com/Artefact2/b5f810600771265fc1e39442288e8ec9):

| Quant | Bits/weight | KL-div median | ln(PPL(Q)/PPL(base)) | Notes |
|---|---|---|---|---|
| Q8_0 | ~8.5 | 0.0043 | 0.0005 | ~lossless |
| Q6_K | 6.57 | 0.0032 | −0.0008 | ~lossless |
| Q5_K_M | 5.67 | 0.0043 | 0.0005 | near-lossless |
| Q5_K_S | 5.52 | 0.0045 | 0.0005 | — |
| **Q4_K_M** | 4.83 | 0.0075 | 0.0060 | community default |
| Q4_K_S | 4.57 | 0.0083 | 0.0081 | — |
| **IQ4_NL** | 4.56 | 0.0085 | 0.0074 | — |
| **IQ4_XS** | 4.32 | 0.0088 | 0.0079 | ≈Q4_K_M quality, fewer bits |
| Q3_K_L | 4.22 | 0.0152 | 0.0205 | — |
| IQ3_M | 3.63 | 0.0186 | 0.0268 | — |
| Q3_K_M | 3.89 | 0.0171 | 0.0258 | — |
| IQ3_XS | 3.32 | 0.0296 | 0.0458 | — |
| Q3_K_S | 3.50 | 0.0304 | 0.0511 | cliff: avoid |
| Q2_K | 3.00 | 0.0588 | 0.1103 | — |
| IQ2_M | 2.76 | 0.0702 | 0.1223 | — |

**PPL-increase summary table (gguf-switchboard, based on Llama-3.1-8B; measured except i-quants which are extrapolated)** [independent] (https://github.com/pradeepgudipati/gguf-switchboard/blob/HEAD/docs/QUANT_SCORING.md):

| Quant | PPL increase vs FP16 | Quality score | Confidence |
|---|---|---|---|
| Q8_0 | 0.1% | 99.9 | Measured |
| Q6_K | 0.4% | 99.6 | Measured |
| Q5_K_M | 1.1% | 98.9 | Measured |
| Q4_K_M | 3.3% | 96.7 | Measured |
| Q4_K_S | 4.1% | 95.9 | Measured |
| Q3_K_L | 6.7% | 93.3 | Measured |
| Q3_K_M | 8.7% | 91.3 | Measured |
| Q3_K_S | 22.4% | 77.6 | Measured |
| Q2_K | ~35% | ~65 | Extrapolated |
| IQ4_NL/IQ4_XS, IQ3_*, IQ2_* | various | various | Extrapolated — treat as order-of-magnitude only |

Caveat (stated in the source itself): this is a generic table; quantization loss varies by architecture and training; per-model measured data (imatrix runs, quantizer model cards) beats the table for that model. [independent]

### 3.2 2026 consensus: K-quants vs i-quants, imatrix

- **Q4_K_M remains the default/recommended 4-bit quant** ("fast, recommended", "community sweet spot: minimal quality loss, major VRAM savings"). Q4_K_S is the size/speed/quality optimum one step down. [independent] (https://github.com/casteldazur/awesome-local-ai/blob/HEAD/guides/vram-requirements.md), (https://huggingface.co/mradermacher/UNAversal-8x7B-v1beta-GGUF)
- **i-quants (imatrix-calibrated) are the 2026 standard for squeezing more quality per bit.** Quantizer guidance (mradermacher, a major GGUF publisher) repeated across model cards: "IQ-quants are often preferable over similar sized non-IQ quants" — specifically: IQ3_S beats Q3_K*; IQ3_M beats Q3_K_L; IQ4_XS approaches Q4_K_M quality at 4.32 vs 4.83 bits/weight. [independent] (https://huggingface.co/mradermacher/Llama3-8B-ScaleQuest-i1-GGUF), (https://huggingface.co/mradermacher/Qwen2.5-14B-Instruct-abliterated-i1-GGUF)
- **imatrix calibration** = importance-matrix computed from a calibration corpus; weights that matter more get more bits. The "i1-" prefix on mradermacher repos denotes imatrix quants. Quality varies with calibration data and the compute used to produce it (mradermacher credits access to a private supercomputer for higher-quality imatrix runs). [independent] (same)
- Practical ladder at cutoff (per major quantizer model cards):
  - Desperate/low VRAM: IQ2_M / IQ3_XS ("IQ3_XXS probably better than Q2_K")
  - Mid: IQ3_S/M, IQ4_XS
  - Default: Q4_K_S / Q4_K_M ("fast, recommended")
  - High quality: Q5_K_M, Q6_K ("very good quality"), Q8_0 ("best quality", ~lossless) [independent] (https://huggingface.co/mradermacher/UNAversal-8x7B-v1beta-GGUF)
- **ikawrakow's quant-PPL graph** (nethype.de) is the community's canonical visual for lower-quality quant comparison (lower = better), linked from most quantizer model cards. [independent] (https://www.nethype.de/huggingface_embed/quantpplgraph.png)
- FP4 (NVFP4) is emerging as a hardware format on Blackwell (DGX Spark, RTX 50) via TensorRT-LLM / vLLM and incoming llama.cpp CUDA support — separate from GGUF integer quants; expect format convergence work through 2026. [secondary] (https://www.compute-market.com/blog/rx-9070-xt-vs-rtx-5060-ti-local-ai-2026)

### 3.3 Notes on sources & limitations

- Most quantization PPL data is community-measured (ikawrakow, Artefact2) on a handful of architectures; i-quant entries in summary tables are often extrapolated, not measured — flagged above.
- r/LocalLLaMA was not directly mined for this report; community claims above come from benchmark repos, quantizer model cards, and review sites.
- No new GGUF quant *types* beyond the K/i-quant families surfaced in 2026 coverage at cutoff; the movement is in imatrix quality, speculative-decoding drafters (MTP/DFlash/DSpark in LM Studio 0.4.22+), and hardware FP4.

---

## OPEN QUESTIONS / UNCERTAINTIES (flagged for follow-up)

1. Exact LM Studio download/user counts — no vendor-published figure found; "millions of downloads" is secondary only. [unverified]
2. LM Studio Enterprise tier and Secure Cloud pricing — not published at cutoff. [unverified]
3. DFlash / DSpark assistant drafters in LM Studio 0.4.22 — changelog-confirmed names only; architectural details not verified. [unverified]
4. IPEX-LLM archival (Jan 28, 2026) — from a community research doc; not independently confirmed against Intel's repo at cutoff. [unverified]
5. Snapdragon X2 Elite / X2 Elite Extreme specs (80 TOPS claim) — single secondary source. [unverified]
6. DGX Spark "2.5×" post-launch software speedup and GPT-OSS-120B 58.8 tok/s (vLLM/MXFP4) figures — vendor/community-reported, not independently reproduced here. [vendor-reported/secondary]
7. Street prices (RTX 5090 ~$2,500–$3,799; RX 9070 XT ~$740) move with GDDR7/memory supply; figures are dated snapshots (Aug–Sep 2026). [secondary]
8. AMD's listed price for the Ryzen AI Halo developer platform ($3,999) and ASRock AI BOX-A395 pricing (unannounced) — from secondary coverage. [secondary]

---

## APPENDIX A — 2026 TIMELINE (LM Studio / local hardware / GGUF-relevant)

| Date | Event | Provenance |
|---|---|---|
| Jan 28, 2026 | LM Studio 0.4.0: GUI/core split (llmster daemon), continuous batching, native REST API `/api/v1/*`, UI refresh | [official] (https://lmstudio.ai/blog/0.4.0) |
| Jan 28, 2026 (reported) | Intel IPEX-LLM archived (read-only) for "known security issues" | [unverified] (https://github.com/chriscorbell/llm-server/blob/HEAD/docs/research/2026-09-07-engines-on-battlemage.md) |
| Feb 23, 2026 | NVIDIA raises DGX Spark Founders Edition MSRP $3,999 → $4,699 (+18%), citing LPDDR5x supply constraints | [secondary] (http://gpusmith.com/articles/en/pdfs/nvidia-dgx-spark-review-specs-performance.pdf) |
| Feb 27, 2026 | LM Studio 0.4.6: LM Link (Tailscale partnership) introduced | [official] (https://lmstudio.ai/changelog/lmstudio/lmstudio-v0.4.6) |
| Mar 17, 2026 | ASRock Industrial announces AI BOX-A395 (Ryzen AI Max+ 395, 128 GB LPDDR5x-8000) | [secondary] (https://videocardz.com/newz/asrock-industrial-launches-ai-box-a395-with-ryzen-ai-max-395-and-128gb-lpddr5x) |
| ~Mar–Apr 2026 | llama.cpp Q8_0 reorder fix merged for SYCL (Apr 7); Intel llm-scaler-vllm B70 support official since 0.14.0-b8.2 | [secondary] (https://github.com/chriscorbell/llm-server/blob/HEAD/docs/research/2026-09-07-engines-on-battlemage.md) |
| Jun 4, 2026 | LM Studio 0.4.16 ships with Locally iPhone/iPad app + LM Link remote access | [secondary] (https://www.digitalapplied.com/blog/lm-studio-locally-lm-link-iphone-local-llm-2026) |
| Jun 2026 | DGX Spark multi-node clustering (up to 4 units) via Cluster Assistant; software claims up to 2.5× vs launch on key workloads | [secondary] (https://emarque.co/collections/nvidia-dgx-spark) |
| Jul 16, 2026 | LM Studio Bionic announced (agentic app; local + Secure Cloud execution) | [secondary] (https://9to5mac.com/2026/07/16/lm-studio-expands-beyond-chat-with-bionic-a-new-ai-agent-app-for-open-models/) |
| Jul 2026 (announced) | Jetson T3000 / T2000 modules announced; Q1 2027 availability | [independent] (http://aiwiki.ai/wiki/jetson_thor) |
| ~Jul 2026 | FP4 inference "being integrated into llama.cpp's CUDA backend" (NVIDIA Blackwell NVFP4) | [secondary] (https://www.compute-market.com/blog/rx-9070-xt-vs-rtx-5060-ti-local-ai-2026) |
| Aug 26, 2026 | GLM-5.3-Flash (320B MoE, 18B active, 1M context) added to Bionic Secure Cloud | [secondary] (https://9to5mac.com/2026/08/26/lm-studio-adds-glm-5-3-flash-to-bionic-with-image-support-and-1m-token-context/) |
| Aug 27, 2026 | Bionic 1.1.0: 2–2.75× faster prompt processing on M5 Macs; Muse Glimmer tool calling for MLX | [official] (https://lmstudio.ai/changelog) |
| Aug 28, 2026 | LM Studio 0.4.22: DFlash/DSpark/MTP assistant drafters (speculative decoding) | [official] (https://lmstudio.ai/changelog/lmstudio/lmstudio-v0.4.22) |
| Sep 8–19, 2026 | Bionic 1.1.2 (Linux builds) → 1.1.4 → 1.1.5 (Splash engine by Inco AI for Qwen3.8 on Mac) | [official] (https://lmstudio.ai/changelog) |
| Sep 9, 2026 | **LM Studio 0.4.24 (latest at cutoff)**: advanced llama.cpp GGUF-load overrides, drafter heuristics, `/api/v1/chat` image fix | [official] (https://lmstudio.ai/changelog/lmstudio/lmstudio-v0.4.24) |

## APPENDIX B — ADDITIONAL HARDWARE NOTES

- **Mac Studio (M3 Ultra)** remains the bandwidth king of unified-memory machines in 2026 coverage: up to 512 GB unified memory at up to ~800–819 GB/s — vs DGX Spark's 273 GB/s; frequently cited as Spark's main capacity-class rival alongside Strix Halo. [secondary] (http://gpusmith.com/articles/en/pdfs/nvidia-dgx-spark-review-specs-performance.pdf)
- **Jetson Orin family** runs JetPack 6.x (Ubuntu 22.04, aarch64) with CUDA/cuDNN/TensorRT at no extra software cost; Jetson Nano (Maxwell) discontinued; Xavier NX/AGX Xavier approaching EOL. [independent] (https://github.com/alpininsight/capi-provider-ssh/blob/HEAD/docs/roadmap/nvidia-jetson-edge-devices.md)
- **RTX 5060 Ti 16 GB** ($429 MSRP, 16 GB GDDR7, 448 GB/s, 180 W): 5th-gen Tensor Cores with FP4; the CUDA-ecosystem budget pick at 16 GB. [independent] (https://www.compute-market.com/blog/rx-9070-xt-vs-rtx-5060-ti-local-ai-2026), (https://localaimaster.com/blog/rx-9070-xt-local-ai)
- **Used market:** RTX 3090 (24 GB) at $699–$999 is the community value buy for 24 GB VRAM (fine-tuning with Unsloth, 32B inference). [secondary] (https://www.compute-market.com/blog/rx-9070-xt-vs-rtx-5060-ti-local-ai-2026)
- **MoE efficiency note (2026):** on bandwidth-limited unified-memory systems, MoE models (gpt-oss-20B, Qwen3-30B-A3B, LFM2-24B-A2B) are dramatically faster than dense models of similar total size — e.g., gpt-oss:20b at ~92 tok/s on a 16 GB RX 9070 XT, where a dense 27B Q4 would be unusable. [independent] (https://github.com/hirokuze/local-llm-benchmark-rx9070xt), (https://localaimaster.com/blog/rx-9070-xt-local-ai)
- **KV-cache quantization** is now standard practice for fitting bigger contexts: 8-bit KV (`--cache-type-k q8_0 --cache-type-v q8_0`) saves ~50% VRAM; vllm-mlx documents 4-bit/8-bit/fp16 KV options. [independent] (https://midwest.social/post/48528597), (https://github.com/anisoptera/vllm-mlx-upstream)
- **Multi-token prediction (MTP) speculative decoding** is the 2026 speed lever: community reports 46 tok/s on Qwen3.6-27B IQ3_M (RX 9070 XT, 62.7% draft acceptance); Google MTP drafters for Gemma; LM Studio 0.4.22+ ships DFlash/DSpark/MTP assistant-drafter support; Bionic enables MTP for more models. [independent/official] (https://midwest.social/post/48528597), (https://lmstudio.ai/changelog/lmstudio/lmstudio-v0.4.22)
- **oMLX** (18.8k GitHub stars, Aug 2026) — a macOS-native multi-model inference server with tiered KV cache (hot blocks in unified RAM, cold spilled to SSD) — illustrates the Apple-Silicon power-user niche between llama.cpp and GUI tools like LM Studio. [independent] (https://github.com/hongsw/clawfit/blob/HEAD/docs/research-watch/2026-08-16-omlx-apple-silicon-llm-inference-server-ssd-kv-cache.md)

## APPENDIX C — LM STUDIO MODEL-CATALOG & ECOSYSTEM NOTES

- The in-app catalog is the product's signature flow: search → Hugging Face pull → memory-fit estimate → download → chat; supports GGUF and MLX quants; no terminal required. [secondary] (https://medium.com/@nishilbhave/lm-studio-in-2026-download-models-run-local-llms-vs-ollama-b30567df17f8)
- 2026 catalog landmarks trace the open-model frontier: `openai/gpt-oss-20b` (120B sibling runs on 128 GB unified machines), Kimi K2.7 Code / K3 (Moonshot), GLM 5.2/5.3-Flash (Z.ai), Qwen3.5/3.6/3.8 (Alibaba), Gemma 4 (Google), DeepSeek V4 Pro (cloud), NVIDIA Nemotron-Nano-v2. [official/secondary]
- LM Link free tier: up to 5 devices; paid/enterprise details unpublished at cutoff. [secondary] (https://learning.christiandrapatz.de/lmstudio-en.pdf)
- Community position vs Ollama (2026): LM Studio = GUI-first, Ollama = CLI-first; both llama.cpp-based so raw tok/s nearly identical; Ollama added paid cloud tiers in 2026 (Pro $20/mo, Max $100/mo) while local stays free; LM Studio remains free locally with paid Secure Cloud credits. [secondary] (https://www.kunalganglani.com/blog/lm-studio-vs-ollama)
- vLLM vs llama.cpp at scale (Red Hat 2026 benchmark, cited in independent coverage): at 64 concurrent users vLLM generated ~44× more tokens/s than llama.cpp — i.e., llama.cpp/LM Studio is the single-user king, vLLM the serving king. [secondary] (https://medium.com/@nishilbhave/local-llms-in-2026-which-runtime-to-run-and-the-hardware-you-need-a88450dece2e)
- Menlo Ventures (2025, cited 2026): open-source models hold ~11% of enterprise LLM usage, down from 19% — local inference remains a niche vs hosted APIs, which frames LM Studio's audience. [secondary] (same)

---

*End of report. Research cutoff: September 22, 2026.*


---


## Master open-verification log (track B, carried forward)

1. **Ollama "Turbo" branding** — could not be confirmed; flagged [unverified] (Ollama draft §12).
2. **Ollama Team pricing discrepancy** — $500/mo ($1,000 shared, Sep 9, 2026) vs $25/seat/mo 5-seat minimum (Aug 28, 2026); both kept, flagged; needs official-page verification.
3. **Ollama per-token billing transition date** — not pinned to an official announcement.
4. **Ollama v0.31.x** — versions unaccounted for (conflicting third-party date); minor timeline gap.
5. **Ollama Enterprise/SSO/MDM** — listed "coming soon" (Aug 2026), not shipped; Enterprise custom plan exists with no published price.
6. **LM Studio user/download figures** — no vendor-published number; "millions of downloads" is [secondary]-only.
7. **LM Studio Secure Cloud / Enterprise pricing** — unpublished.
8. **LM Studio DFlash/DSpark drafter details** — [unverified].
9. **IPEX-LLM archival status** — [unverified].
10. **Snapdragon X2 (2026)** — unconfirmed details.
11. **DGX Spark speedup claims** — vendor-adjacent claims not independently verified; thermal issues community-reported.
12. **AI BOX pricing** — [unverified].
13. **llama.cpp Feb–Jul 2026 b-tag history** — only sampled, not exhaustive.
14. **Tom's Hardware DGX Spark chart numbers** — not extractable from accessible sources.
15. **ServeTheHome 2026 llama.cpp benchmark article** — no dedicated article found; coverage is forum-based.
16. **GGUF container version-bump absence (2026)** — inference from unchanged server README cache-type list, not a positive statement.
17. **KleidiAI macOS build DISABLED** — reason unknown.
18. **GGUF quant community benchmark numbers** — Artefact2/ikawrakow and gguf-switchboard PPL tables are [secondary] community measurements; do not mix with vendor-reported figures.
19. **Street prices for GPUs** — dated September 2026 snapshots, move weekly.

## Collection metadata
- Sources used: ggml-org/llama.cpp GitHub releases, ollama/ollama GitHub releases, lmstudio.ai changelog, Ollama docs, ServeTheHome, Tom's Hardware, community benchmarks (Artefact2/ikawrakow, gguf-switchboard), LM Studio SDK docs.
- Method: web research, read-only; no live-browser visits; no external sends.
- Working drafts retained: etape4_draft_llamacpp.md, etape4_draft_ollama.md, etape4_draft_lmstudio_hw.md.
