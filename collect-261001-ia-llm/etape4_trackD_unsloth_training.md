# Step 4 — Track D: Unsloth + Training / Fine-Tuning Tooling (2026)

**Coverage window:** January 1 → September 22, 2026
**Report date:** September 22, 2026
**Status:** research snapshot; prices/versions are September-2026 snapshots. Re-verify against official repos before use.

## Provenance legend
- `[official]` — project's own repo, docs, changelog, or release notes.
- `[vendor-reported]` — claim made by the vendor/team without independent audit.
- `[independent]` — third-party measurement, reputable press, or peer analysis.
- `[secondary]` — blogs, aggregators, community docs (GitHub knowledge bases, AI China News); useful but unverified.
- `[unverified]` — single-source or conflicting; treat as uncertain.

---

## 1. UNSLOTH AI

### 1.1 Company and funding (as of September 2026)

- Unsloth AI (unsloth.ai), founded by brothers **Daniel Han** and **Michael Han** (Daniel — ex-NVIDIA background; LinkedIn `danielhanchen`) — Australia-based founders, corporate office **720 Clipper Street, San Francisco, CA** (per PitchBook; office is real-estate HQ/listing, team largely remote) [secondary — PitchBook company profile, Crunchbase].
- **Confirmed funding: ~$500K pre-seed** (Y Combinator pre-seed; Redpoint Ventures scout; Samsung NEXT; Pioneer Fund). An investor scorecard (Zoo Capital EIR, March 23, 2026) lists "~$500K seed (Redpoint Ventures scout, Samsung NEXT)" with 57K stars and 150M+ downloads at that date [secondary — https://github.com/lucy-cxy/agentvc-index/blob/main/cases/2026-03-23_unsloth.md]. Crunchbase lists 8 investors incl. **Pioneer Fund and Samsung NEXT**, headcount 11–50 [secondary]. PitchBook lists 12 investors: **Lightspeed Venture Partners, Transpose Platform Management, Y Combinator, Brightwing Capital** and others [secondary].
- **No confirmed large seed/Series A announced** as of Sep 22, 2026. Some older dev.to commentary claimed "no seed round yet" (that piece is from ~Feb 2025; stale). Treat any "raised $X" figure beyond the ~$500K pre-seed as **[unverified]** — Crunchbase/PitchBook figures are paywall-obfuscated in public view.
- Notable: an unusually small raise relative to adoption (~500K→investor ratio is widely commented on). ⚠️ "Official fine-tuning partner" claims (Google/OpenAI/Meta/NVIDIA) appear in the investor scorecard but lack official partnership press releases — **[unverified]**.
- Community adoption snapshot: the skill-card survey (Sep 17, 2026) puts Unsloth at **34k+ GitHub stars** (repo `unslothai/unsloth`); March 2026 scorecard: 57K stars / 150M+ downloads — ⚠️ inconsistent counts across sources; star count is a moving target [secondary].

### 1.2 Product line (2026)

Unsloth now ships three surfaces [official — unsloth.ai docs + staging READMEs]:
- **Unsloth Core** — the Python library (`pip install unsloth`), code-first fine-tuning.
- **Unsloth Studio** — web UI: dataset building ("Data Recipes" from PDFs/CSVs/DOCX), export to GGUF/NVFP4/FP8, RL configs [official].
- **Unsloth Desktop** — local desktop app (Mac/Windows/Linux), 100% offline-capable, GGUF/Safetensors; one-line installer `curl -fsSL https://unsloth.ai/install.sh | sh` [official/vendor-reported].
- **Unsloth Start** (2026): connects coding agents to local models with one command: `unsloth start claude --model unsloth/Qwen3.8-27B-GGUF:UD-Q4_K_XL`; supports Claude Code, OpenAI Codex, DeepSeek Harness (`dsh`), Hermes Agent, OpenCode, OpenClaw [official — README, Sep 2026].
- Docker image: `hub.docker.com/r/unsloth/unsloth` [official].

### 1.3 Speed / VRAM claims (vendor vs independent)

Vendor claims (all `[vendor-reported]` unless noted):
- "2–5x faster training with 50–80% less VRAM, zero accuracy loss" via custom fused Triton kernels + manual backprop derivation [vendor-reported].
- **MoE LLMs: "12x faster with 35% less VRAM"** — DeepSeek, GLM, Qwen, gpt-oss [vendor-reported — https://unsloth.ai/docs/new/faster-moe].
- **New RoPE & MLP Triton kernels + padding-free packing: "3x faster training, 30% less VRAM"** [vendor-reported — https://unsloth.ai/docs/new/3x-faster-training-packing].
- **7x longer-context RL** vs other setups via new batching algorithms [vendor-reported — https://unsloth.ai/docs/new/grpo-long-context].
- **500K context**: fine-tuning a 20B model with >500K context on an 80GB GPU [vendor-reported — https://unsloth.ai/docs/blog/500k-context-length-fine-tuning].
- **Embedding models: ~1.8–3.3x faster embedding fine-tuning** [vendor-reported].
- Qwen3.6: MTP for 1.4–2.2x faster inference; NVFP4 quants for supported GPUs [vendor-reported].
- Dynamic 2.0 4-bit quantization: claims record 5-shot MMLU & Aider Polyglot scores vs other quants [vendor-reported, single-vendor benchmarks].
- Independent verification: **not found** in this research window — no published third-party benchmark reproduced Unsloth's 2–5x/70% figures against transformers+FA2 baselines. A community voice-fine-tuning study (Ryan Baumann, Aug 17, 2026) is building such a comparison (Gemma 4 26B-A4B on Unsloth vs `mlx_lm.lora` on Apple Silicon) but had not published final numbers at research time [independent-in-progress]. → **flag: speed/VRAM claims remain vendor-only; cite with caution in RAG.**

### 1.4 2026 feature releases (chronological, from official docs/news)

- **GRPO / RL stack**: rule-based GRPO (no separate critic), **Dr. GRPO, DAPO, BNPO, GSPO** variants; **FP8 reinforcement learning** on consumer GPUs; **Vision RL (VLM GRPO)**; long-context RL (7x claim) [official docs].
- **TTS**: text-to-speech fine-tuning incl. `sesame/csm-1b`, STT `openai/whisper-large-v3`; 2026 model support extended to MiniMax-Music3 / Higgs / MOSS audio models [official]. ⚠️ Per-community docs, Whisper support is for inference/adaptation recipes, not full TTS training — exact coverage not independently verified.
- **Vision fine-tuning**: Qwen3-VL, Gemma 3, Llama 3.2 Vision recipes; VLM GRPO [official].
- **500K-context blog** (2026): 20B model at >500K ctx on 80GB [official].
- **AMD ROCm**: formal AMD GPU support added **2026-07-20** [secondary — community SKILL.md; official docs page not directly fetched — verify].
- **DoRA** lightweight fine-tuning method support [secondary].
- **GGUF Dynamic v3.0** quants [secondary].
- **Auto compaction** of long chats in Desktop; remote/LAN access preview with QR; MCP support; Projects; Deep Research Mode; Parallel Chat [secondary — community skill cards; not all independently confirmed].
- **Model support added in 2026** (from official README news): Qwen3.5 (0.8B–112B-A10B), Qwen3.6 (+MTP, NVFP4), Qwen3.8-27B, Qwen3.8-Flash-Next (125B), DiffusionGemma, GLM-5.2, GLM-5.3-Flash, DeepSeek-V4-0731, Kimi K3 (2.8T total / 104B active MoE, 1M ctx), Gemma 4 12B (+QAT), Muse Glimmer (Meta), MiniMax audio models, gpt-oss RL + flex-attention long-context [official].
- **Apple Silicon / MLX**: one community card says "MLX (Apple Silicon) — not yet supported" while another (later, Chinese) lists "MLX optimized support" — **conflicting; [unverified]** whether native MLX fine-tuning shipped in 2026.

### 1.5 Licensing
- Core library: **Apache 2.0** (per investor scorecard) [secondary]. Unsloth's pre-quantized model uploads on Hugging Face carry per-model licenses; commercial products: **Unsloth Pro** (multi-GPU, enhanced speed) and **Unsloth Studio/Desktop** (desktop app) — pricing not published in fetched sources [secondary/unverified]. ⚠️ Confirm current license text in repo LICENSE before legal use.

---

## 2. AXOLOTL

### 2.1 v0.19.0 release — September 2026 (latest)
- Release: **axolotl-ai-cloud/axolotl v0.19.0**, ~12 days before Sep 22, 2026; **72 commits since v0.18.0 (July 17, 2026)** [official — https://github.com/axolotl-ai-cloud/axolotl/releases/tag/v0.19.0].
- Highlights:
  - **Declarative Model Support Profiles**: new-arch support via family templates (`VANILLA_CAUSAL_LM`, `IMAGE_TEXT_TO_TEXT`) instead of scattered loader edits; no config changes [official].
  - **Qwen3.8-Flash-Next**: fine-tune Qwen's 176.94B multimodal MoE on a single B300, as little as 120 GiB with `ple_cpu_offload: true`; QLoRA, vision QLoRA, NVFP4 MoE-LoRA configs ship [official].
  - **Ternary (BitNet b1.58) QAT**: `qat.weight_dtype: ternary` — weights restricted to {-1,0,1} natively, no extra dependency; Neutrino-1 two-stage recipe (ternary continued pretraining → instruction tuning) [official].
  - Seven new model families; two new optimizers; chat-template tokenization speedup; **PyTorch 2.13 support** [official].
- Community summary (README, Sep 2026): Unsloth 34k+ vs **Axolotl 17k+ GitHub stars**; positioning: Axolotl = production-grade distributed training [secondary].

### 2.2 2026 feature timeline (from README "Latest Updates")
- **2026/07**: NVFP4 (4-bit) MoE LoRA via **ScatterMoE (W4A16)** and **SonicMoE (W4A4)**, incl. lossless adapter merge back into NVFP4 checkpoint [official/secondary].
- **2026/06**: **Expert Parallelism (EP)** for distributed MoE via **DeepEP**; remote training through **Tinker-compatible APIs** (Thinking Machines' Tinker); **Context Parallelism for hybrid SSM models** (Nemotron-H, Falcon-H1, Bamba); **BitNet 1.58-bit fine-tuning**; multimodal assistant-only loss-masking fix [secondary].
- **2026/04**: model support for **Mistral Medium 3.5** and **Gemma 4**; **Async GRPO** (up to 58% faster steps); **Flash Attention 4**; **NeMo Gym** integration; **EBFT**; **uv-first** packaging; **SonicMoE fused LoRA** [secondary].
- **2026/03**: Mistral Small 4, Qwen3.5 (+MoE), GLM-4.7-Flash, GLM-4.6V, GLM-4.5-Air; **MoE expert quantization** (`quantize_moe_experts: true`, FSDP2-compatible) [secondary].
- **2026/02**: **ScatterMoE LoRA** (LoRA directly on MoE expert weights, custom Triton kernels); **SageAttention**; **GDPO** (Generalized DPO) [secondary].
- **2026/01**: **EAFT** (Entropy-Aware Focal Training); **Scalable Softmax** for long-context attention [secondary].
- **2025/12** (context): Kimi-Linear, Plano-Orchestrator, MiMo, InternVL 3.5, Olmo3, Trinity, Ministral3; **Distributed Muon Optimizer** for FSDP2 pretraining [secondary].
- Note: Axolotl positions against Unsloth as YAML-config-driven, production-grade, **FSDP2 multi-node** training [secondary].

### 2.3 Company status
- Maintained by **axolotl-ai-cloud** org (the company behind Axolotl AI Cloud). No funding announcement found in 2026 research — **[unverified/no data]**. Treat "Axolotl raised $X" as unconfirmed.

---

## 3. LLAMA-FACTORY (hiyouga/LLaMA-Factory)

- **v0.9.5 — June 17, 2026** (latest found): support for **Qwen 3.5/3.6**, Gemma 4, Phi-4-mini, **GLM-4.7-Flash**; Tencent Hunyuan/Youtu; **Huawei Ascend NPU** (Partial RoPE, Hybrid Attention); ROCm 7.2; VLMs (MiniCPM-V-4.6, Qwen3-VL, LiquidAI LFM 2.5-VL); **Transformers v5 compatibility**; **FSDP2**; Liger Kernels for Qwen 3.5/Qwen3-Next [secondary — AI China News].
- v0.9.3 (Mar 2026): Llama 4, DeepSeek-R1, Qwen3, InternVL3, Gemma 3 [secondary].
- v0.9.2 (early 2026): DeepSeek V3 (671B), Qwen2.5-VL, InternLM3; QLoRA on Ascend via CANN; APOLLO optimizer; Ray Trainer; vLLM batch inference; Ollama modelfile auto-generation [secondary].
- Role: "Swiss Army Knife" of the Chinese developer community; no-code **LlamaBoard** web UI; 30K+ GitHub stars (2025) [secondary].
- ⚠️ All LLaMA-Factory version details sourced from AI China News auto-generated posts; treat specific feature claims as [secondary].

---

## 4. TORCHTUNE (PyTorch/Meta)

- PyPI description (crawled Sep 2026; package page itself dated ~Dec 2025, v0.6.x era): hackable recipes for **SFT, knowledge distillation, DPO, PPO, GRPO, QAT**; native Llama/Gemma/Mistral/Phi/Qwen implementations; YAML configs [official — pypi.org/project/torchtune].
- Recipe coverage matrix (from docs): full/single/multi-node SFT; LoRA/QLoRA single-device & multi-device (not multi-node); DPO full (distributed) + LoRA; PPO full single-device; **GRPO marked 🚧 (in progress)**; QAT distributed [official].
- **No 2026 torchtune release notes found** in this research window — flag as **[gap]**: Meta's torchtune releases (0.7+) were not located; do not assume features. Known context: torchtune 0.6 (Dec 2025) added Qwen3, Gemma 3, Llama 4 Scout support and torchao QAT improvements — outside or at the edge of window [background knowledge, unverified in-window].

---

## 5. TRL (Hugging Face)

- **TRL v1.0** landed early 2026: unified post-training stack (SFT, reward modeling, DPO, GRPO) with one API [secondary — press]. Docs/community notes put the line at ~**v1.8** by September 2026 [secondary].
- **TRL v1.4** (announced by HF staff @qgallouedec, ~Sep 15, 2026): chunked NLL loss for SFT; first-class **agentic RL environments** (`trl.experimental.openreward`, `OpenRewardSpec("Eigent/SETA", num_tasks=64)` — one-string wiring of dataset/factory/reward; dynamic tool binding from JSON Schema); MFU helpers for dense + MoE; **GRPO support for Liger 0.8.0** (delta clipping + VESPO + KL bias correction); Tülu 3 length-normalized DPO loss; 4 new chat templates (Cohere, Cohere2, Gemma 3, Qwen3-2507); 5+ GB CUDA memory-leak fix in activation offloading [secondary — HF post].
- **As of August 2026**: `GRPOTrainer` `loss_type` defaults to **"dapo"**, not original GRPO (docstring: "Not recommended due to length bias"); GSPO via `importance_sampling_level="sequence"`; paper index covers DAPO, Dr. GRPO, GSPO, CISPO reproductions [independent — HF blog "How frontier models train on outcomes in 2026", Aug 2026].
- CLI: `trl sft|dpo|grpo|kto|rloo|reward|distillation`; `trl vllm-serve` **deprecated** in favor of `vllm serve` [official — trl docs].
- Trend note (HF blog, Aug 2026): frontier labs train **in environments** (sandboxed tool-use rollouts, outcome-based rewards combining LLM-judge rubrics + programmatic checks + safety gates); cited: LiquidAI LFM2.5-2.6B, Cursor Composer, Kimi K3, GLM-5, Nemotron 3 Ultra, MiniMax [independent].

---

## 6. PEFT / LoRA ECOSYSTEM (2026)

- **DoRA** supported in Unsloth [secondary]; **LoRA/QLoRA** remain the default adapter method; **NVFP4 LoRA** (ScatterMoE W4A16 / SonicMoE W4A4) landed in Axolotl Jul 2026 [official/secondary].
- **LoRA directly on MoE expert weights** via ScatterMoE custom Triton kernels (Axolotl, Feb 2026) [secondary].
- **Liger Kernels** integrated in LLaMA-Factory (Qwen 3.5/3-Next) and **Liger 0.8.0** supported by TRL v1.4 GRPO [secondary].
- **NeMo Gym** (NVIDIA) integration in Axolotl (Apr 2026) [secondary].
- **Merged-adapter export**: Axolotl Jul 2026 supports lossless merge of NVFP4 MoE LoRA back into plain NVFP4 checkpoints [secondary].

---

## 7. DISTRIBUTED TRAINING FRAMEWORKS

### 7.1 DeepSpeed
- **v0.19.4 patch release** (~Aug 6, 2026) [official — GitHub release]: highlights — **AutoTP: ZeRO stage 3 inference with tensor parallelism**; HF `colwise_gather_output` for `lm_head` replace; AutoEP rank splits derived from per-expert count exchange; Tutel support for shared-MoE k≠1; DeepCompile compiler-state scoping; NVMe write warning [official].
- **Release 0.19.5** docs dated **Aug 06, 2026** (readthedocs PDF) [official]: covers AutoEP (automatic expert parallelism), MoE, Transformer kernels, pipeline parallelism, optimizers.
- **ZeRO-Inference refresh**: up to **20X throughput speedup** via weight quantization + KV-cache offloading (DeepSpeed ≥ 0.10.3) [official — deepspeedexamples README].
- AutoTP/AutoEP direction: automatic parallelism-plan derivation (HF `tp_plan` support) — reduces manual sharding config [official].

### 7.2 FSDP2
- FSDP2 is the 2026 default distributed strategy in the PyTorch ecosystem: Axolotl ships **FSDP2** for MoE expert quantization and distributed Muon pretraining; LLaMA-Factory v0.9.5 adds **FSDP2** support; torchtune builds on latest PyTorch distributed APIs; Axolotl added **PyTorch 2.13** support (v0.19.0) [official/secondary].
- ⚠️ No standalone "FSDP2 2026 announcement" located; evidence is via downstream adoption — treat specific claims per-project.

### 7.3 Megatron-LM
- **No 2026 Megatron-LM release notes located** in this window. Context: NVIDIA's Megatron-LM repo development continued (TransformerEngine, Megatron-Bridge), and DeepSpeed docs still reference Megatron-LM model init patterns, but no dated 2026 milestone was verified. → **[gap — flag for re-research]**.

---

## 8. INFERENCE EXTRAS

### 8.1 TensorRT-LLM
- **v1.2.0** (~Mar 2026) [official]: beta support for K-EXAONE, Nemotron Nano V3, Qwen3-Next, Qwen3-VL; expanded Blackwell/Hopper/Ampere enablement (B300/GB200/GB300, SM120/SM121/SM103); FP8/NVFP4/MXFP4/INT4-AWQ; MTP>1 speculative decoding for DeepSeek v3.2; disaggregated serving (service discovery, request cancellation, NIXL-LibFabric, Mooncake); FlashInfer batched sampling; EPLB expert parallelism; **DGX Spark (GB10) beta support**.
- **v1.2.1** (2026-04-20): multi-arch container incl. **arm64 on NGC** (`nvcr.io/nvidia/tensorrt-llm/release:1.2.1`) [secondary].
- **v1.3.0 RC** (Sep 2026): "Enable FlashInfer GDN decoding kernel for Qwen3.5"; Mamba-hybrid support (from 0.19.0-era work); Qwen3.6-27B (GDN hybrid) **not yet** in the stable support matrix [secondary].
- Infra: TensorRT 10.10.0, CUDA 12.9.0, PyTorch 2.7.0 wheels, ModelOpt 0.29.0; XQA kernels open-sourced; **V100 support removed** (breaking change) [official/secondary].
- Notable fixes in window: LLaMA 4 CUDA-graph illegal memory access; Qwen3-MoE attention DP bug [official].

### 8.2 ONNX Runtime GenAI
- **No 2026 ONNX Runtime GenAI release notes located** in this window → **[gap — flag for re-research]**. (Last verified releases predate the window; Microsoft's focus in 2026 reporting has been on Foundry/TRT-LLM-class stacks.)

### 8.3 llama.cpp server (llama-server)
- No dedicated 2026 llama-server milestone captured in this window → **[gap]**. (llama.cpp itself continues active development; server-side OpenAI-compatible endpoint is stable. Not separately verified — flag.)

### 8.4 ExLlamaV3 + TabbyAPI
- **ExLlamaV3** (`turboderp-org/exllamav3`) is the live project; **ExLlamaV2 is archived** ("Development continues on ExLlamaV3") [official].
- **v1.4.5 (2026-08-31)**: win_amd64 wheels cp310–cp313, cu128/torch 2.10.0; **v1.4.6** in use by Sep 3, 2026 (community benchmark) [independent].
- Format: **EXL3** — streamlined variant of **QTIP** (Cornell RelaxML); one-step quantization (fused Viterbi kernel), minutes for small models on one RTX 4090; coherent at **1.6 bpw** (Llama-3.1-70B in <16 GB); Marlin-inspired GEMM ≈ memory-bound at 4 bpw on RTX 4090; v1.0.0 (2026-07-14) greatly improved Ampere GEMM/GEMV, removed flash-attn-2/xformers deps [independent — community doc citing GitHub API].
- Features: tensor-parallel + expert-parallel for consumer setups; 2–8-bit KV-cache quantization; speculative decoding; LoRA; multimodal; ROCm on the to-do list (not yet) [independent].
- Model support (Sep 2026): Llama 3/4, Qwen 2/2.5/3 (+MoE, Next, VL, 3.5), Mistral/Mixtral, Phi3/4, Gemma, GLM-5.3-Flash, DeepSeek family, Kimi, NanoChat, Nemotron-H/3, Olmo 3.1, Step 3.5/3.7, SmolLM, SolarOpen, Seed-OSS [independent].
- **TabbyAPI** (`theroyallab/tabbyAPI`, ~1.3k★, AGPL-3.0): active; security issue #448 (CORS allowlist) **closed 2026-09-07** by maintainer; startup script manages ExLlamaV3 prerequisites; OpenAI-compatible serving [independent].
- Community benchmark (Sep 3, 2026, 4× CMP 170HX, GLM-5.3-Flash EXL3 4.05bpw via TabbyAPI): **25.2 tok/s single-concurrency decode**, 44.6 tok/s aggregate at c=8 [independent — measured].

---

## 9. FUNDING / M&A IN THE TOOLING SPACE

- **Unsloth AI**: ~$500K pre-seed (YC, Redpoint scout, Samsung NEXT, Pioneer Fund); 12 investors incl. Lightspeed VP, Transpose, Brightwing per PitchBook. No confirmed larger round as of Sep 22, 2026 [secondary; see §1.1 caveats].
- **Axolotl AI Cloud**: no funding news located in window [gap].
- **Thinking Machines' Tinker** (managed fine-tuning API, Oct 2025 launch; Inkling model Jul 2026) is the managed-cloud alternative positioning against Unsloth Core/Axolotl self-hosted — relevant competitive context, covered in Step 3 Track C [cross-ref].
- **No 2026 M&A** found in the fine-tuning/inference-tooling space (Unsloth, Axolotl, LLaMA-Factory, TRL, torchtune, DeepSpeed, ExLlamaV3 all independent as of Sep 22, 2026) [secondary — absence of evidence; not proof].

---

## 10. UNCERTAINTY / VERIFICATION LOG

1. Unsloth speed/VRAM claims (2–5x, 70% less, 12x MoE, 7x long-ctx RL) are **vendor-only** — no independent reproduction located. [open]
2. Unsloth raise beyond ~$500K pre-seed — unconfirmed; Crunchbase/PitchBook details paywalled. [open]
3. Unsloth "official fine-tuning partner" of Google/OpenAI/Meta/NVIDIA — no official announcements located. [unverified]
4. Unsloth Apple-Silicon/MLX native support — conflicting community claims. [unverified]
5. Axolotl funding/company financials — no data. [gap]
6. torchtune 2026 releases — not located. [gap]
7. Megatron-LM 2026 milestones — not located. [gap]
8. ONNX Runtime GenAI 2026 releases — not located. [gap]
9. llama-server 2026 milestones — not located. [gap]
10. TRL version ~v1.8 claim — community doc only; latest directly confirmed is v1.4 (Sep 2026). [secondary]
11. LLaMA-Factory specifics — sourced from auto-generated AI China News posts only. [secondary]
12. Unsloth Desktop feature list (MCP, Deep Research Mode, etc.) — community skill cards; not independently confirmed. [secondary]

## 11. COLLECTION METADATA
- Method: web search (browser.search), 8 query batches, Sep 22, 2026.
- Priority sources hit: official GitHub release pages (Axolotl v0.19.0, DeepSpeed v0.19.4, TensorRT-LLM v1.2.0), official docs (TRL CLI, torchtune PyPI, Unsloth docs), HF blog (Aug 2026 agentic RL), community benchmarks (ExLlamaV3/TabbyAPI Sep 2026).
- No SiliconANGLE piece specific to this tooling niche was located in-window; inference-tooling coverage came from GitHub/primary sources instead.
- Nothing was sent externally; read-only research + local file write.
