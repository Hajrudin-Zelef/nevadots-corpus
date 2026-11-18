# ÉTAPE 1 — Open / Local AI Models (EN)
## Open-weight models + local inference — 2026 landscape
*Consolidated research fiche for RAG ingestion. Compiled 2026-09-22.*
*All benchmark figures are vendor-reported unless marked as independently measured. Priority sources: Artificial Analysis, BenchLM, OpenRouter, Hugging Face, CheapestInference, official lab blogs.*

---
# STEP 1 — Open / Local AI Models (Chinese track) — Research Fiche
**Compiled: 2026-09-22 · Output: English · For RAG ingestion**
*All benchmark figures are vendor-reported unless marked as independently measured (Artificial Analysis, BenchLM, independent harnesses). Always store the provenance tag with the number.*

---

## 0. LANDSCAPE OVERVIEW — the open-weight frontier (September 2026)

- The open-weight frontier is **owned by Chinese labs**: DeepSeek, Moonshot AI (Kimi), Z.ai (GLM), Alibaba (Qwen), Xiaomi (MiMo), MiniMax, Tencent (Hy3), StepFun (Step 5 Preview). **Meta has left the open frontier** — Llama 5 has not shipped (forecast 2027); Meta pivoted to its first closed frontier model, Muse Spark (April 2026).
- **Artificial Analysis Intelligence Index v4.3 (Sept 2026) snapshot** — closed leaders: Fable 5.1 = GPT-6 Astra = 53, Opus 5 = 51, Muse Spark 1.3 = 48, GPT-5.6 Sol = 47. Open-weight leaders: **MiMo-V2.6-Pro = 46** (tied with closed Grok 4.7), **Qwen3.8 Max 0902 = 45, GLM-5.3 = 45, Kimi K3 = 44** (level with Grok 4.6), **DeepSeek V4.1 Flash ≈ 39–40, DeepSeek V4.1 Pro = 36**.
  - *Methodology warning: AA Index v4.1/v4.1.1 numbers (e.g., Kimi K3 57.1, GLM-5.3-Flash 57, Qwen3.6-Max-Preview 52, Qwen3.7-Max 56.6) are NOT directly comparable to v4.3 numbers (K3 44). Record the index version with every figure.*
- **OpenRouter**: Chinese open models went from ~2% of usage (2025) to >50% (mid-2026); US frontier models fell from 70% to 30%. Crossover week: Feb 9–15, 2026.
- **The "open" bifurcation**: leading open models are trillion-parameter datacenter-scale MoEs (GLM-5.3 753B, Kimi K3 2.8T, Qwen3.8 Max 2.4T, MiMo-V2.6-Pro 1.02T) needing 8–64+ accelerators; genuinely small models (Qwen3.6-27B, Qwen3.8-27B) run on one GPU, with little in between.
- **License tightening arc**: every Chinese family moved from MIT/Apache permissive terms toward custom licenses with revenue-gated MaaS clauses (Kimi K3: $20M; MiniMax M3: $20M; GLM-5.3: $10B security review; MiniMax H3: territorial restriction). GLM-5.3-Flash (MIT) is the notable counter-example.
- **SWE-bench milestones**: DeepSeek V4 Pro 80.6% Verified; MiniMax M3 80.5% Verified; GLM-5.2 62.1% Pro (vs GPT-5.5 58.6%); MiMo-V2.6-Pro DeepSWE v1.1 71.9.

---

## PART A — QWEN (Alibaba / Tongyi Qianwen)

**Naming convention (2026):** Qwen3.5 / 3.6 / 3.7 / 3.8 are successive flagship generations, not point releases. "Max" = top tier of each generation (proprietary until 3.8).

### A1. Qwen3.5-397B-A17B — released February 16, 2026
- **Architecture:** Sparse MoE — **397B total, 17B active/token**; 512 routed experts + 1 shared, 10 routed + 1 shared activated. **Hybrid attention:** 60 layers as 15 repeating groups of [3× Gated DeltaNet → MoE, 1× Gated Attention → MoE]. Multi-Token Prediction (MTP), MRoPE, vocab 248,320.
- **Context:** 262,144 native, extensible to 1M+ via YaRN. Extremely small KV cache (~3.9 GB at 128K ctx).
- **Multimodal:** Native vision-language via early-fusion training (text + image + video in); 201 languages/dialects.
- **Reasoning:** Thinking mode on by default, toggleable via API.
- **License:** **Apache 2.0**.
- **Hugging Face:** `Qwen/Qwen3.5-397B-A17B` (BF16), `Qwen/Qwen3.5-397B-A17B-FP8`; community quants (Unsloth, QuantTrio AWQ). Smaller sibling: Qwen3.5-27B.
- **Benchmarks (vendor):** SWE-bench Verified 76.2 · Pro 50.9 · Multilingual 69.3 · Terminal-Bench 2.0 52.5 · SkillsBench 30.0 · MMLU-Pro 87.8 · SuperGPQA 70.4 · GPQA Diamond 88.4 · HLE 28.7 · LiveCodeBench v6 83.6 · AIME26 93.3 · IFBench 76.5 (best in class) · BrowseComp 78.6 (best in class).
- **Distinguishing:** First open-weight Max-scale generation; Alibaba claimed 8.6–19× decoding throughput vs Qwen3-Max.

### A2. Qwen3.6 family (April 2026)
- **Qwen3.6-Plus** (Apr 2, 2026): 1M context, always-on reasoning, agentic coding + visual coding; integrates OpenClaw, Claude Code, Cline. Bailian pricing ~¥2/M in, ¥12/M out.
- **Qwen3.6-Max-Preview** (Apr 20, 2026): **closed weights**; 35B total / 3B active MoE; 256K context; #1 at launch on six benchmarks (SWE-bench Pro, Terminal-Bench 2.0, SkillsBench, SciCode, QwenClawBench, QwenWebBench). AA Index v4.1.1: **52**.
- **Qwen3.6-27B** (Apr 22, 2026): first **dense** open-weight Qwen3.6; **Apache 2.0**; HF `Qwen/Qwen3.6-27B`; 256K context; multimodal; runs on 1× H100/H200. SWE-bench Verified **77.2** (beats Qwen3.5-397B), Pro 53.5, Terminal-Bench 2.0 59.3.
- **Qwen3.6-35B-A3B:** MoE 35B/3B active; SWE-bench Verified 73.4.

### A3. Qwen3.7-Max — unveiled May 26, 2026 (API GA May 21–23)
- **Architecture:** Sparse MoE, **>1T parameters**, 1M context, 65,536 max output. Tailored for agentic workflows (35-hour autonomous runs claimed).
- **License:** **Proprietary / closed weights** (API only).
- **Benchmarks:** AA Index v4.1.1 **56.6** — global #5, #1 Chinese model (May 2026); IFBench 79.1 (record).
- **Pricing:** Alibaba Cloud ¥12/M in / ¥36/M out; OpenRouter **$2.50/$7.50**. Supports OpenAI- and Anthropic-Messages API formats.

### A4. Qwen3.8-Max — Preview July 19–20, 2026; GA August 3, 2026
- **Architecture:** Sparse MoE + hybrid attention — **2.4T total, ~95B active/token** (~4% activation); 92 layers, hidden 8192, **512 experts (10 routed + 1 shared)**; vocab 248,320.
- **Context:** 1M tokens (~991K max input; 131K max output; 262K reasoning budget). Multimodal in (text/image/video), text out — API only.
- **Pricing (API):** **$2/M input, $6/M output**. Updated checkpoint **Qwen3.8-Max-0902** (Sept 2): #1 Code Arena.
- **Benchmarks (vendor):** Terminal-Bench 2.1 86.6 · PaperBench 93.0 · SWE-bench Pro 67.7 · FrontierSWE 73.5 · DeepSWE 1.1 56.6 · GPQA Diamond 92.6. AA Index v4.3: **45** (AA-Briefcase Elo 1622, GDPval 1689, cost/task **$5.41** — the expensive end of the tier).
- **OPEN WEIGHTS — VERIFIED SHIPPED:** Published **August 12, 2026** on HF as **`Qwen/Qwen3.8-2.4T-A95B`** and **`-FP8`** (also ModelScope; Unsloth GGUF quants). First-ever open weights for a Max-tier Qwen flagship.
  - **License catch:** NOT Apache 2.0 — custom **"qwen3.8-max" license**: attribution above 100M MAU or $20M monthly revenue; MaaS/AI-assistant businesses above $50M trailing revenue need a separate commercial license.
  - **Capability gap vs API:** open checkpoint is **text-only, thinking-mode only** — no vision, no native 1M context (262,144 native, extensible to ~1,010,000).
  - **Hardware:** BF16 ≈ 4.89 TB, FP8 ≈ half; cheapest usable quant ≈ 450 GB combined RAM+VRAM; NVIDIA reference = GB300 NVL72 rack.
- **Companion:** **Qwen3.8-27B** — dense 27.8B, **Apache 2.0**, Terminal-Bench 73.0, ~14–16 GB VRAM at 4-bit (RTX 4090 friendly); HF `Qwen/Qwen3.8-27B`.

---

## PART B — DEEPSEEK

### B0. R-series (reasoning lineage)
- **DeepSeek-R1** — Jan 20, 2025 (updated R1-0528, May 2025). MoE 671B/37B active. 64K → 164K context. AIME 2024 79.8% at ~1/18 the per-token price of o3. **MIT**; HF `deepseek-ai/DeepSeek-R1`. First-party API retired July 24, 2026 (weights remain).
- **DeepSeek-R2 — NOT RELEASED (as of Sept 2026).** All specs online (685B–1.2T, $0.14/M) are unconfirmed leaks. Documented delays: CEO Liang Wenfeng dissatisfied with performance; export controls; failed training run on Huawei Ascend 910C (stability/interconnect/software); pivot back to Nvidia for training, Ascend for inference. No launch timeline.

### B1. DeepSeek V4 family — V4 Preview April 24, 2026
- **DeepSeek V4-Pro:** MoE **1.6T total / 49B active**. **Hybrid CSA + HCA attention + mHC hyper-connections** (replaces V3's MLA); **Muon optimizer**; **mixed FP4+FP8 precision**; 32T+ training tokens. 1M context, 384K max output. At 1M tokens: 27% of V3.2's FLOPs, 10% of KV cache. Text-only. **MIT**; HF `deepseek-ai/DeepSeek-V4-Pro-0813`. LiveCodeBench 93.5, Codeforces 3206, GPQA Diamond 90.1. AA Index 36. Off-peak pricing **$0.66/M in / $1.98/M out**; cache hit $0.022. Self-host: 8× H200/B200 (SGLang NVFP4) or 4× GB300 (vLLM).
- **DeepSeek V4-Flash:** MoE **284B total / 13B active**; same CSA/HCA architecture; 1M context. **MIT**; HF `deepseek-ai/DeepSeek-V4-Flash-0731`. Current off-peak **$0.22/$0.66**. AA Index 35.
- **Engram note:** "Engram conditional memory" was rumored pre-V4 but absent from V4; it shipped in V4.1 Flash.

### B2. DeepSeek V4.1 Flash ⭐ — released September 10, 2026
- **Architecture:** First model on **Causal Encoder-Decoder (CED)** — 40 layers (20 encoder + 20 decoder). **552B backbone, 8B active on input / 16B on output**; plus **196B Engram conditional-memory params** (~763B total). CSA-2 + FP4 KV cache → 890 bytes/token (~¼ of V4 Flash). Trained on **45T tokens**. Native vision. 1M context, 384K max output. Observed 214 tok/s, TTFT ~1.1s.
- **License:** **MIT**. Weights: `deepseek-ai/DeepSeek-V4.1-Flash` on HF (FP8 + technical report). Checkpoint ≈ 510 GB.
- **Pricing (two tiers):** Off-peak — cache-hit input **$0.003/M**, cache-miss **$0.15/M**, output **$0.60/M**. Peak (Mon–Fri 01:00–04:00 and 06:00–10:00 UTC) = double. Weekends always off-peak.
- **Product-line event:** V4-Pro API traffic was to auto-route to V4.1 Flash on Sept 14; **reversed** — V4-Pro continues, no date for V4.1-Pro.
- **Benchmarks (vendor):** Terminal-Bench 2.1 **90.6** (vs Opus 5.0 89.1), DeepSWE v1.1 **74.2** (vs Opus 5.0 74.0, V4-Pro 62.7), CyberGym 88.1, AutomationBench 54.8, GPQA Diamond 90.9. Beats V4-Pro on 12/14 rows at ~⅓ backbone, ~⅙ active params.
- **Independent (Artificial Analysis):** Index **≈39–40**; AutomationBench-AA 69%, cost/task **$0.27** (Pareto frontier). Flagged as "very verbose" (250M output tokens vs 140M median).
- **Distinguishing:** Cheapest intelligent open-weight model per task; first shipped Engram memory; 890 B/token KV cache makes 1M context economically viable.

---

## PART C — KIMI (Moonshot AI, Beijing, est. 2023)

**License trajectory:** K2 (MIT) → K2.5/K2.6/K2.7 Code (modified MIT, attribution trigger ~100M MAU/$20M-mo) → K3 (fully custom license, $20M/12-mo MaaS separate-agreement trigger).

### Kimi K2 — July 11, 2025
MoE **1T total / 32B active**; MLA; 128K → 256K context. **MIT**. Agentic-task optimized. OpenRouter ~$0.57/$2.30. First 1T open-weight agentic model.

### Kimi K2.5 — January 26–27, 2026
Same 1T/32B/384-expert MoE (8+1 shared active, 61 layers); vision-native (MoonViT, ~15T visual+text tokens); native INT4. 256K context. Modified MIT. Benchmarks: HLE **50.2** (vs GPT-5.2 45.5), BrowseComp 74.9, SWE-bench Verified 76.8 (scaffold; independent mini-SWE-agent 70.80%), AIME 2025 96.1. Pricing $0.60/$3.00, cached $0.10/M. **Agent Swarm** primitive (up to 100 parallel sub-agents). Deprecated May 20, 2026.

### Kimi K2.6 — April 20, 2026 (GA Apr 21)
1T/32B/384 experts, 61 layers, 160K vocab, MLA+SwiGLU, MoonViT 400M vision; native INT4; multimodal. 256K context. Modified MIT (`moonshotai/Kimi-K2.6`). Benchmarks: SWE-bench Pro **58.6** (tied GPT-5.5), Verified **80.2**, Terminal-Bench 2.0 66.7, AIME 2026 96.4, HLE-Full 54.0, AA Index **54 — #1 open-weight at launch**. Pricing $0.95/$4.00, cache-hit $0.16/M. Agent Swarm scaled to **300 sub-agents, 12–13h autonomous sessions**.

### Kimi K2.7 Code — June 12, 2026 (HighSpeed June 15)
Coding/agents specialization of K2.6; thinking mode mandatory (temp hard-pinned 1.0). Modified MIT. Benchmarks (vendor-only): Kimi Code Bench v2 62.0 (+21.8% vs K2.6), MCP Mark Verified 81.1 (vs Opus 4.8 76.4). **~30% fewer thinking tokens** than K2.6; HighSpeed mode up to 6× throughput. Pricing $0.95/$4.00. Self-host footprint ~630–640GB at INT4.

### Kimi K3 ⭐ — API July 16, 2026; open weights July 26–27, 2026
- **Architecture:** **2.8T total**, sparse MoE — **896 experts, 16 active/token**; 61 layers. **Kimi Delta Attention (KDA)** (up to 6.3× faster decoding at 1M ctx); **Attention Residuals** (~25% training efficiency at <2% extra params). Native multimodal. Weights shipped as **MXFP4 weights / MXFP8 activations** — download footprint **~1.56 TB**, largest open-weight checkpoint ever. Variants: K3 Max, K3 Swarm Max; always-on reasoning.
- **Context:** 1,048,576 tokens.
- **License:** **NOT MIT.** Custom **Kimi K3 License**: (1) **MaaS clause — operators/affiliates with >$20M aggregate revenue over any 12 consecutive months must sign a separate commercial agreement**; (2) attribution ("Kimi K3") above 100M MAU or $20M/month revenue.
- **Benchmarks:** AA Index v4.1 **57.1** — 4th of all models (behind Fable 5 59.9, GPT-5.6 Sol 58.9); v4.3 rebase: **44**. Vendor: Terminal-Bench 2.1 88.3; topped Claude Fable 5 on Frontier Code Arena (human-judged); launch stunts: autonomous 48h chip design, MiniTriton GPU compiler built from scratch.
- **Pricing:** **$3/M in, $15/M out**, cached ~$0.30/M — ~3× K2.6 ("the end of super-cheap Chinese AI"); AA-measured ~$0.94/task; 21% fewer output tokens than K2.6.
- **HF:** `moonshotai/Kimi-K3` + ModelScope; vLLM/SGLang/TokenSpeed recipes.
- **Market impact:** launch triggered AI/semiconductor stock sell-off ("DeepSeek moment" parallels).

---

## PART D — GLM (Zhipu AI / Z.ai, Tsinghua spin-out 2019)

### GLM-5.2 — Coding Plan June 13, 2026; API + weights June 16, 2026
Sparse MoE **753B total / ~40B active**; 384 experts; Dynamic Sparse Attention; trained on **28.5T tokens**. **1M context** (from 200K), 128K–131K max output; text-only. **MIT** (`zai-org/GLM-5.2`). Benchmarks: SWE-bench Pro **62.1** (vs GPT-5.5 58.6), Terminal-Bench 2.1 **81.0** (first open-weight >80), AIME 2026 99.2, GPQA Diamond 91.2, AA Index **51 — #1 open-weight at launch**. Pricing $1.40/$4.40, cached $0.26/M (~⅙ of GPT-5.5). Released one day after US Commerce forced Anthropic to disable Fable 5/Mythos 5.

### GLM-5.3 — API August 14, 2026; open weights August 28, 2026
- **Architecture:** 753B/~40B active MoE (78 layers); **same base as GLM-5.2 — gains from post-training only**; text-only; 1M context.
- **Staged release:** weights **withheld ~2 weeks for "safety evaluation and hardening"** (emergent offensive-cyber capability, CyberGym 84.5); shipped on HF (`zai-org/GLM-5.3`, `-BF16`) **Aug 28, 2026**, exactly the promised 14 days; safety work reportedly found 2,436 vulns across 269 OSS projects. 756GB native FP8; fits 8×H200, not 8×80GB H100.
- **License:** **NOT MIT.** Custom **GLM-5.3 License**: MIT-style permissions **plus security-review condition for MaaS operators with >$10B trailing-12-month revenue** (~500× looser than Kimi K3's $20M trigger).
- **Benchmarks:** DeepSWE **66.9** (vs 5.2's 46.2), CyberGym 84.5, AutomationBench 48.8 (vs Opus 4.8 41.0), AA v4.3 **45**. AA measures ~$2.15/M blended.
- **Distinguishing:** first explicit **staged open-weight release (pause → harden → ship)**; cyber-capability framing drove both the delay and the license change.

### GLM-5.3-Flash — August 26, 2026 (stealth-tested as "Ox Alpha")
- **Architecture:** **320B total / 18B active** MoE (45 layers); hybrid **KDA linear + NoPE sparse MLA** (~3× less attention compute, 4.4× smaller KV via IndexPool); **natively multimodal** (first in GLM-5 family); **1M context**; native FP8 (~306 GiB).
- **License:** **MIT**. HF: `zai-org/GLM-5.3-Flash` + `-BF16` (321B params).
- **Benchmarks:** DeepSWE v1.1 **63.4** (vs Opus 4.8 58.0), AutomationBench 48.8 (vs Opus 4.8 41.0), Terminal-Bench 2.1 **84.3** (vs Opus 4.8 85.0 — near-parity), AA Index v4.1.1 **57** at ~$0.045/task. Vision is the weak flank.
- **Pricing:** **$0.15/M in, $0.50/M out, $0.03/M cached**; launch half-price promo through Sept 9 (~1/33 in, ~1/50 out of Opus 4.8's $5/$25).
- **The 100,000-chip story:** stealth-tested as **"Ox Alpha"** on OpenRouter/OpenCode — became **most-used model on both, >62T tokens in 6 days (31% of OpenRouter weekly traffic)**. All inference on **>100,000 domestic Chinese accelerators** (Cambricon, Huawei, Moore Threads) with custom SGLang-based disaggregated engine; **Infra Agent powered by GLM-5.3 did most optimization** — first boot on domestic silicon to all production traffic in **~13 days, 3.2× throughput vs baseline**. Serving via SGLang, vLLM, TokenSpeed, KTransformers.

### GLM-5.3-FlashX — September 18, 2026
**Identical weights to GLM-5.3-Flash** (320B/18B, 1M) — a pure **inference-configuration** variant for the domestic-chip fleet. Output **200 tok/s** (~3.2–5× faster than Flash depending on metric); license/pricing presumed same as Flash.

---

## PART E — MIMO (Xiaomi; MiMo AI team led by Luo Fuli, ex-DeepSeek)

**Context:** First model MiMo-7B (MIT, Apr 30, 2025). CEO Lei Jun announced ≥**$8.7B** AI investment over 3 years (Mar 2026). MiMo Code (open-source terminal coding agent) and HarnessX shipped June 2026. V2.6's RL reportedly ran in part on **Chinese chips rather than Nvidia GPUs**.

### MiMo-V2.5 / V2.5-Pro — April 22, 2026
V2.5 = **310B total**; V2.5-Pro = **1.02T total / 42B active** MoE. 1M context; May 2025 cutoff. **MIT** (V2.5 made the Pro tier open; predecessor March-2026 MiMo-V2-Pro was proprietary). Agentic coding / long-horizon SE; "harness awareness". Benchmarks (vendor): SWE-bench Pro 57.2 with 40–60% fewer tokens than Opus 4.6; DeepSWE v1.1 19.0 (Pro). Launch pricing (overseas): Pro $1.00/$3.00 (≤256K), doubling for 256K–1M; cache reads $0.20–$0.40/M; base V2.5 $0.40/$2.00. (Pricing later revised downward before V2.6.)

### MiMo-V2.6-Flash — September 21, 2026
**309B total / 15B active** MoE. **1M context**, 128K max output; natively omnimodal (text/image/audio/video in, text out). **MIT**; HF `XiaomiMiMo/MiMo-V2.6-Flash-RL` (65 shards, ungated) + ModelScope. API: **$0.14/M in, $0.28/M out**, cached $0.0028/M; 50% batch discount. Benchmarks (vendor): DeepSWE v1.1 **65.68–67.9**, AutomationBench **52.3–52.7** (Flash *outscores* Pro here), Terminal Bench 2.1 87.6, CyberGym 95.1, Toolathlon-Verified 73.6.

### MiMo-V2.6-Pro ⭐ — September 21, 2026 (top open-weight model)
- **Architecture:** **1.02T total / 42B active**; **frozen-router MoE**; **hybrid attention**; **5-layer MTP speculative decoder**; RL via **fully asynchronous GRPO** — 1,568 prompts × 16 rollouts/step, 3.5–3.7B tokens/step, 750k trajectories, 30 RL steps in <6 days.
- **Context:** **1M tokens**, 128K max output; native multimodal in.
- **License:** **MIT**; HF `XiaomiMiMo/MiMo-V2.6-Pro-RL` (ungated) + ModelScope. Training cost (Xiaomi-reported): ~**$2.62M** Pro RL / $0.85M Flash; training livestreamed publicly at mimo.xiaomi.com/rl.
- **Benchmarks (vendor):** AA Index v4.3/v4.3.2: **46.32 (→46)** — best open-weight; ahead of Kimi K3, Qwen3.8 Max; tied with closed Grok 4.7 (46); above Grok 4.6 (44), Gemini 3.8 Flash (41), DeepSeek V4.1 Flash (39). AA measures **$0.13/task**, ~134 tok/s. DeepSWE v1.1 **71.9** (from 58.4 pre-RL; vs Opus 5 74.0, Sol ~73.0). AutomationBench **53.1** (beats all listed closed rivals). Terminal Bench 2.1 **89.9**. GDPval-AA 2.1 Elo 1673 (vs Opus 5 1708). MiMo Visual Coding 72.3 (vs Opus 5 70.0). Weak spots (vendor's own table): ProgramBench 26.5 (vs Opus 5 37.0); Terminal Bench 4.0 34.9 (vs 49.0); ExploitBench 47.9 (vs Sol 78.5).
- **Pricing:** **$0.435/M uncached in, $0.87/M out**; cached $0.0036/M; 50% batch discount. Xiaomi: "1/20 to 1/60 of overseas models at the same intelligence level."
- **Why it leads:** (1) scaled RL on verifiable long-horizon agentic tasks (30 steps / ~750k trajectories / 6 days); (2) frozen-router MoE + hybrid attention + MTP at 42B/15B active params; (3) MIT + ungated HF + released RL environments (7,000+ task envs); (4) V2.5-era pricing kept with higher intelligence → cost Pareto record ($0.13/task).
- **Pricing note:** the brief's "$0.20/$0.70" could NOT be matched to any MiMo tier — likely confusion with V2.5-era cache-read pricing ($0.20/M) or Grok 4.1 Fast. Treat as unverified for MiMo.

### MiMo-V2.6-Pro-UltraSpeed (serving tier, not separate weights)
Accelerated inference tier: up to **20× faster output at the same quality**; 10× price → **$4.35/$8.70** (cached $0.036/M). MiMo API / MiMo Desktop. No Flash UltraSpeed tier documented.

---

## PART F — MINIMAX (Shanghai, est. 2021; also Hailuo video, MiniMax Agent)

**License trajectory (tightest of the families):** M2 (MIT) → M2.1 (modified MIT) → M2.5 (frontmatter "modified-mit" but file is "MINIMAX MODEL LICENSE" — trust the file) → M2.7 (non-commercial) → M3 (Community License, non-commercial default; <$20M notice, >$20M prior written authorization) → H3 (community license + **"Applicable Territory" geographic restriction**).

### MiniMax M2.5 — February 12, 2026
MoE **229B total / ~10B active**; RL in "hundreds of thousands of real-world environments" (Forge framework). 196K context (AWS Bedrock card). Variants: Standard (50 tok/s), **Lightning (100 tok/s)**. License: MINIMAX MODEL LICENSE. Benchmarks: SWE-bench Verified **80.2** (Droid scaffold 79.7 vs Opus 4.6 78.9), BrowseComp 76.3. Pricing: $0.30/$1.20, cache read $0.03/M; "intelligence too cheap to meter" (~$1/hr).

### MiniMax M2.7 — March 18, 2026
**230B / 10B active** MoE; ~200K context; text-only. **Non-commercial** license (commercial needs prior written authorization + "Built with MiniMax M2.7" display). Benchmarks: SWE-Pro **56.22%** (matching GPT-5.3-Codex), Terminal Bench 2 ~57.0, AA Index **50** (+8 over M2.5 in <1 month). Pricing $0.30/$1.20. **First model to participate in its own development** — 100+ rounds autonomous scaffold self-optimization, +30% internal gain, zero human intervention. Caveat: per-task verbosity can run ~3× headline rate (~87M vs 26M median tokens).

### MiniMax M3 — June 1, 2026
- **Architecture:** MoE **~428B total / ~23B active**; **MiniMax Sparse Attention (MSA)** — blockwise sparse attention on GQA: **9× prefill / 15× decode speedups vs M2 at 1M ctx, ~1/20 per-token compute** (arXiv:2606.13392); native multimodal (text+image+video from step one); three thinking modes; native MXFP4 + MXFP8.
- **Context:** **1M tokens** (guaranteed min 512K under load); 131K max output.
- **HF:** `MiniMaxAI/MiniMax-M3` (+ `-MXFP8`, 443.8GB serving checkpoint; BF16 854.2GB); MSA kernel open-sourced (`MiniMax-AI/MSA`); NVIDIA build.nvidia.com; OpenRouter `minimax/minimax-m3`; Anthropic-compatible endpoint.
- **License:** **"MINIMAX COMMUNITY LICENSE"** — non-commercial by default; commercial requires "Built with MiniMax M3" display + notice to api@minimax.io (≤$20M/yr) or **prior written authorization (>$20M/yr)**; Prohibited Uses appendix.
- **Benchmarks (vendor):** **SWE-bench Verified 80.5** (vs V4-Pro 80.6, K2.6 80.2, GPT-5.5 82.9, Opus 4.7 87.6); **SWE-bench Pro 59.0** (beats GPT-5.5 58.6); Terminal-Bench 2.1 66.0; BrowseComp 83.5; Video-MME v2 85.4; SVG-Bench 63.7 (beats Opus 4.7 62.3). One independent harness reproduced ~80.5 territory.
- **Pricing:** Standard ≤512K: **$0.30/M in / $1.20/M out** (cache read $0.06; "permanent 50% off" applied); >512K: $0.60/$2.40; Token Plans $20/$50/$120 per month.

### MiniMax H3 — announced July 31, 2026 (weights ~Aug 2026) ⚠ NOT an LLM
**H3-Omni-Transformer, 33B dense single-stream transformer for native joint video+audio generation** (4–15s clips, 24 FPS, **native 32 kHz stereo audio in the same forward pass**, 768p native / 2K via H3-Regenerate-2K, 11 dialogue languages). Two open checkpoints (FL2VA, Ref2VA); H3-Context-IR and H3-Regenerate-2K closed. **First open model to top an AI video ranking** (AA: #1 Video Editing, #2 Text-to-Video, #3 Image-to-Video). **License: "MiniMax-H3 Community License" with "Applicable Territory" geographic restriction** (reported to effectively lock out US/EU — procurement risk). Commercial only under $20M revenue. HF `MiniMaxAI/MiniMax-H3`; fits single RTX 5090-class GPU.

---

## PART G — "JEV": identity resolved ⚠ NOT a Chinese open model

- **What:** Jev is a **proprietary American AI model** by **TypeSafe AI** (San Francisco, founded 2024 by **Diogo Almeida** — ex-OpenAI: RLHF, InstructGPT, ChatGPT, GPT-4 — Erik Gafni, Sasha Sheng).
- **Release:** **September 15, 2026**, limited early access (waitlisted), ~2-year stealth exit; **$40M seed led by DCVC** (~$200M valuation, per Forbes).
- **Category:** first of a new class of **"System One models"** (Kahneman's fast/intuitive System 1 vs deliberative System 2 LLMs).
- **Key difference:** **not a generative LLM** — generates **no natural-language text**. Takes program state + typed questions, returns **typed values with probability/confidence** for direct software consumption. Three primitives: **Choice** (distribution over options), **Score** (probability-weighted rubric score), **Noul** (yes/no probability). All questions answered in one parallel pass.
- **Specs:** 70–500 ms latency; 64K context (32K state + longest question); text-only at launch. Trained with **RLCD (Reinforcement Learning for Calibrated Decisions)** — distinct from RLHF/RLVR.
- **License:** **proprietary / closed** — managed API only, **no open weights**. Routes: `jev-latest`, `jev-preview`, `jev-1.13.0`. Python + JS/TS SDKs.
- **Pricing:** **$0.042 per 1M input tokens, output free**.
- **Open-source clone:** **Laya** (independent researcher, Sept 2026) — open weights, claimed 6–8× faster, benchmarked against Jev.
- **RAG filing:** does NOT belong in the open-weight Chinese track. File under a separate "new model categories / System One" track. Own Wikipedia page: https://en.wikipedia.org/wiki/Jev_(AI_model).

---

## CROSS-FAMILY COMPARISON (Sept 2026)

| Model | Release | Params (total/active) | Context | License | API $/M in/out | SWE-bench Verified | AA Index |
|---|---|---|---|---|---|---|---|
| Qwen3.5-397B-A17B | Feb 2026 | 397B / 17B | 262K→1M+ | Apache 2.0 | — | 76.2 | — |
| Qwen3.6-27B | Apr 2026 | 27B dense | 256K | Apache 2.0 | — | 77.2 | — |
| Qwen3.6-Max-Preview | Apr 2026 | 35B / 3B | 260K | Proprietary | — | — | 52 (v4.1.1) |
| Qwen3.7-Max | May 2026 | >1T | 1M | Proprietary | $2.50/$7.50 | — | 56.6 (v4.1.1) |
| Qwen3.8-Max | Aug 2026 | 2.4T / 95B | 1M | Custom (open) | $2 / $6 | — (Pro 67.7) | 45 (v4.3) |
| DeepSeek V4-Pro | Apr 2026 | 1.6T / 49B | 1M | MIT | $0.66/$1.98 | — | 36 |
| DeepSeek V4-Flash | Apr 2026 | 284B / 13B | 1M | MIT | $0.22/$0.66 | — | 35 |
| DeepSeek V4.1 Flash | Sep 2026 | 552B+196B / 8–16B | 1M | MIT | $0.15/$0.60 | — | ~39–40 |
| Kimi K2.6 | Apr 2026 | 1T / 32B | 256K | Modified MIT | $0.95/$4.00 | 80.2 | 54 |
| Kimi K3 | Jul 2026 | 2.8T / ~16×896e | 1M | Custom ($20M MaaS gate) | $3 / $15 | — | 44 (v4.3) |
| GLM-5.2 | Jun 2026 | 753B / 40B | 1M | MIT | $1.40/$4.40 | — (Pro 62.1) | 51 |
| GLM-5.3 | Aug 2026 | 753B / 40B | 1M | Custom ($10B review) | ~$2.15 blended | — | 45 (v4.3) |
| GLM-5.3-Flash | Aug 2026 | 320B / 18B | 1M | MIT | $0.15/$0.50 | — | 57 (v4.1.1) |
| MiMo-V2.5-Pro | Apr 2026 | 1.02T / 42B | 1M | MIT | $1.00/$3.00* | — (Pro 57.2) | — |
| MiMo-V2.6-Pro | Sep 2026 | 1.02T / 42B | 1M | MIT | $0.435/$0.87 | — (DeepSWE 71.9) | 46 (v4.3) |
| MiMo-V2.6-Flash | Sep 2026 | 309B / 15B | 1M | MIT | $0.14/$0.28 | — | — |
| MiniMax M2.5 | Feb 2026 | 229B / 10B | 196K | MINIMAX license | $0.30/$1.20 | 80.2 | — |
| MiniMax M2.7 | Mar 2026 | 230B / 10B | 200K | Non-commercial | $0.30/$1.20 | — (Pro 56.22) | 50 |
| MiniMax M3 | Jun 2026 | 428B / 23B | 1M | Community ($20M gate) | $0.30/$1.20 | 80.5 | — |
| Jev (TypeSafe) | Sep 2026 | n/a (decision model) | 64K | Proprietary | $0.042/in, out free | n/a | n/a |

\*V2.5 launch-era pricing; later revised downward.

## STRUCTURAL FINDINGS (for RAG metadata)
1. **License tightening is universal** across Chinese open-weight families: MIT/Apache → custom licenses with revenue-gated MaaS clauses ($20M: Kimi K3, MiniMax M3; $10B security review: GLM-5.3; territorial: MiniMax H3). Counter-example: GLM-5.3-Flash stayed plain MIT.
2. **Sovereign-hardware narrative**: GLM-5.3-Flash serves entirely on ~100K domestic Chinese accelerators (Cambricon/Huawei/Moore Threads) — built in ~13 days with Infra-Agent automation; MiMo-V2.6's RL ran partly on Chinese chips.
3. **Staged open-weight release**: GLM-5.3's 2-week safety hold is the first explicit pause→harden→ship precedent.
4. **1M context is table stakes** for flagships (K3, GLM-5.x, M3, MiMo-V2.x, Qwen Max, DeepSeek V4.x); Kimi K2.x (256K) and MiniMax M2.x (~200K) are the exceptions.
5. **"Open" now means datacenter-scale**: flagship open models are trillion-parameter MoEs; small dense models (Qwen3.6/3.8-27B) serve the single-GPU tier.
6. **Architectural trend**: off plain attention — Gated DeltaNet hybrids (Alibaba), compressed-sparse attention + FP4 KV (DeepSeek), KDA linear attention (Moonshot, Z.ai), MiniMax Sparse Attention, encoder-decoder (DeepSeek V4.1 Flash), Engram conditional memory (DeepSeek).
7. **Meta out**: Llama 5 delayed to 2027, Meta pivoted to closed Muse Spark — Chinese labs own the open frontier.

## CAVEATS
- All vendor-reported benchmarks are self-reported unless labeled Artificial Analysis / independent harness. MiMo V2.6 figures were <24h old at research time.
- AA Index v4.1.x vs v4.3/v4.3.2 numbers are NOT comparable across versions — always record the version.
- Qwen3.8-Max open weights: weights confirmed shipped Aug 12, 2026 (`Qwen/Qwen3.8-2.4T-A95B`) but under custom license, text-only, reduced context.
- DeepSeek R2: unreleased/vaporware as of Sept 2026.
- MiniMax M2.5 license: HF frontmatter ("modified-mit") disagrees with the repo license file ("MINIMAX MODEL LICENSE") — trust the file.
- MiMo "$0.20/$0.70" pricing from the brief could not be verified — likely confusion with cache-read pricing or Grok 4.1 Fast.
- Kimi K3 "~104B active/token" appears in one source only — unverified; 896 experts / 16 active is multiply confirmed.
- Jev is American and proprietary — filed separately from the Chinese open track.

## KEY SOURCES
- Artificial Analysis: https://artificialanalysis.ai
- BenchLM.ai: https://benchlm.ai
- OpenRouter: https://openrouter.ai
- CheapestInference (State of Open Weights) · ChinaAPI / China AI Index
- https://venturebeat.com/technology/better-than-deepseek-xiaomis-mimo-v2-6-pro-debuts-as-the-top-open-weights-model-in-the-world-alongside-cheaper-v2-6-flash
- https://theoutpost.ai/news-story/xiaomi-mi-mo-v2-6-pro-debuts-as-top-open-source-ai-model-scoring-46-on-intelligence-index-31152/
- https://officechai.com/ai/xiaomi-mimo-v-2-6-pro-benchmarks/
- https://www.intelligentliving.co/qwen3-8-max-update-smarter/
- https://www.intelligentliving.co/xiaomi-mimo-v2-6-pro-cheapest-frontier/
- https://github.com/sampraszheng/yxz/blob/HEAD/wiki/synthesis/open-weight-llm-agent-stack-six-region.md
- https://www.swfte.com/ai/leaderboard
- https://en.wikipedia.org/wiki/Xiaomi_MiMo
- https://en.wikipedia.org/wiki/Jev_(AI_model)
- https://jev-agent.com/
- https://www.alibabacloud.com/en/press-room/alibaba-unveils-qwen3-8-max
- https://www.alizila.com/alibaba-unveils-qwen3-8-max-most-capable-flagship-model-to-date/
- https://explainx.ai/blog/qwen3-8-max-open-weights-live-hugging-face-august-2026
- https://www.siliconreport.com/alibaba-launches-qwen3-6-27b-and-says-its-dense-coding-model-tops-qwen3-5-397b-a17b-d15d82b7347c62a3
- https://www.morphllm.com/deepseek-v4
- https://www.orcarouter.ai/blog/deepseek-v4-1-flash-openrouter
- https://dataconomy.com/2026/09/11/deepseek-v4-1-flash-ultralow-token-pricing/
- https://theplanettools.ai/blog/deepseek-r2-never-shipped-what-deepseek-released-instead-2026
- https://openrouter.ai/moonshotai/kimi-k2
- https://www.marktechpost.com/2026/06/12/moonshot-ai-releases-kimi-k2-7-code-a-coding-model-reporting-21-8-on-kimi-code-bench-v2-over-k2-6/
- https://venturebeat.com/technology/kimi-k3s-full-weights-are-here-but-theyre-open-with-a-caveat-what-enterprises-should-know
- https://www.marktechpost.com/2026/08/26/z-ai-releases-glm-5-3-flash-a-320b-a18b-natively-multimodal-moe-with-a-1m-token-context/
- https://www.unite.ai/z-ai-details-glm-5-3-flash-inference-build-on-100-000-chinese-chips/
- https://cryptobriefing.com/zhipu-glm-53-flash-chinese-ai-chips/
- https://huggingface.co/MiniMaxAI/MiniMax-M3
- https://platform.minimax.io/docs/guides/pricing-paygo
- https://the-decoder.com/chinas-minimax-h3-is-the-first-open-model-to-top-an-ai-video-ranking/
- https://www.techtimes.com/articles/318622/20260618/minimax-m3-takes-open-weight-ai-lead-sparse-attention-architecture-now-verified.htm
- https://felloai.com/glm-5-2/
- https://felloai.com/minimax-m2-5-model/
- https://forum.devtalk.com/t/laya-the-os-version-of-jev-created-a-year-ago/249901
# STEP 1 — Open / Local AI Models — WESTERN TRACK (EN)

**Consolidated research fiche for RAG ingestion. Compiled 2026-09-22.**
*Coverage: Meta Llama · Meta Muse Spark · Poolside · Mistral AI · NVIDIA Nemotron · Microsoft Phi · Google Gemma · other notable Western open-weight releases through September 2026. Benchmark figures are vendor-reported unless marked independent.*

---

# PART 1 — META LLAMA FAMILY

## 1.1 Timeline overview (through September 2026)

| Date | Event |
|---|---|
| **2025-04-05** | Meta releases **Llama 4 Scout** and **Llama 4 Maverick** — first natively-multimodal, open-weight MoE models in the Llama family. Llama 4 **Behemoth** (288B-active teacher) announced as still training. |
| 2025-04 | **Benchmark controversy**: the LMArena entry `Llama-4-Maverick-03-26-Experimental` turns out to be an unreleased variant scoring far above the public weights; later publicly acknowledged. |
| 2025-06 | Meta forms **Meta Superintelligence Labs (MSL)** under Chief AI Officer **Alexandr Wang**, following the ~$14B Scale AI investment. |
| **2026-01** | Departing chief AI scientist Yann LeCun publicly acknowledges the Llama 4 benchmark manipulation. |
| **2026-04-08** | **Muse Spark** launches — first model from MSL, **closed-weight/proprietary** (Meta's first ever proprietary model), replacing the Llama brand. Internally codenamed **"Avocado"**. |
| 2026-07-09 | **Meta Model API** goes public and paid — Meta's first metered inference API. |
| 2026-07 | **Muse Spark 1.1** ships. |
| **2026-08-05** | **Muse Spark 1.2** + **Muse Code** (terminal coding agent, macOS/Linux beta). |
| **2026-08-10** | **Muse Glimmer 30B** — open weights under **Apache 2.0**; Meta announces **Muse Spark 1.2's weights will also be open-sourced**. Zuckerberg publishes an essay framing open weights as a safeguard against AI concentration. |
| 2026-09-02 | **Muse Spark 1.3** ships (xhigh public, max partner preview); paid API opened Sept 3. |
| 2026-09-08 | **"Muse" consumer app** launches — personal agent app on per-user Meta-hosted VMs. |
| Sept 2026 | Current frontier: **muse-spark-1.3**. **No Llama 5 exists** — the newest Meta model on OpenRouter is still `meta-llama/llama-4-maverick` (Apr 2025). Behemoth was never released. |

## 1.2 Llama 4 Scout

**Release date:** April 5, 2025

| Attribute | Value |
|---|---|
| Architecture | Mixture-of-Experts, natively multimodal (early fusion; text+image in) |
| Parameters | **109B total / 17B active per token, 16 experts** |
| Context window | **10M tokens** — largest open-weight context at release |
| Training data | ~40T tokens, text-focused corpus |
| Hardware | Fits on a single NVIDIA H100 (FP8) |
| License | **Llama 4 Community License** (gated; accept on Hugging Face) |
| HF weights | `meta-llama/Llama-4-Scout-17B-16E-Instruct` (+ `-Instruct-Original` native format, base `-17B-16E`); NVIDIA FP8/FP4 builds `nvidia/Llama-4-Scout-17B-16E-Instruct-FP8` |

**Benchmarks (Meta-reported, April 2025):** MMLU **79.6%**, MMLU Pro **74.3%**, GPQA Diamond **57.2%**, MATH **50.3%**, LiveCodeBench **32.8%**, MMMU **69.4%**, MGSM **90.6%**.
**Benchmarks (Artificial Analysis, independent):** Intelligence Index v4.3 **6.5** (earlier AA estimate **8**); GPQA Diamond **58.7%**, SciCode **21.3%**, Terminal-Bench 2.1 **3.7%**, AIME 2025 **14%**. Output speed **100.1 tok/s** median, TTFT **0.83s**.
**API pricing:** AA median across providers **$0.19 / $0.68** per 1M in/out tokens; OpenRouter-range ≈ **$0.15–$0.20 in / $0.50–$0.80 out**.
**What distinguishes it:** the 10M-token context window in an open-weight model (∼78× GPT-4o's 128K), single-H100 deployability, and best-in-class multimodal quality at its size tier (beat Gemma 3, Gemini 2.0 Flash-Lite, Mistral 3.1 per Meta).

## 1.3 Llama 4 Maverick

**Release date:** April 5, 2025

| Attribute | Value |
|---|---|
| Architecture | MoE, natively multimodal (early fusion); distilled partly from Behemoth |
| Parameters | **~400B total / 17B active per token, 128 experts** |
| Context window | **1M tokens** |
| Training data | ~22T tokens, multimodal incl. Meta content |
| License | **Llama 4 Community License** |
| HF weights | `meta-llama/Llama-4-Maverick-17B-128E-Instruct` (+ `-Original` native); Ollama `llama4:maverick` |

**Benchmarks (Meta-reported, April 2025):** MMLU **85.5%**, MMLU Pro **80.5%**, GPQA Diamond **69.8%**, MATH **61.2%**, LiveCodeBench **43.4%**, MMMU **73.4%**; MBPP pass@1 **77.6%**; HumanEval **62.0** (third-party AWS Bedrock table).
**Benchmarks (independent):** AA Intelligence Index v4.3 **9.3** (AA estimate **10**); AA GPQA Diamond **67.1%**, SciCode **31.7%**, Terminal-Bench 2.1 **7.9%**, AIME 2025 **19.3%**. **SWE-bench Verified 21.0%**; **SWE-bench Pro 5.24%** — a weak coding showing. LMArena Elo **1,417** — *but see the benchmark controversy below*. Output speed **111.8 tok/s**, TTFT **0.90s**.
**API pricing:** AA median **$0.26 / $0.91** per 1M in/out; OpenRouter **`meta-llama/llama-4-maverick` $0.20 / $0.80**; cheapest providers from **$0.15 / $0.60**.
**What distinguishes it:** the strongest open-weight multimodal model of its generation (Meta claimed it beat GPT-4o and Gemini 2.0 Flash on broad benchmarks; comparable to DeepSeek V3 on reasoning/coding at <½ the active parameters), 1M context, 200+ languages. Coding was its weak spot vs. Chinese peers — Qwen3.5-122B-A10B beats it by +16.8 pts on GPQA Diamond and +34.6 on LiveCodeBench.

## 1.4 Llama 4 Behemoth — never released

Announced April 5, 2025 as still training: **288B active parameters / 16 experts, ~2T total**, intended as the teacher for Scout/Maverick and Meta's answer to GPT-4.5/Claude 3.7/Gemini 2.0 Pro on STEM. As of September 2026 it has **never been publicly released and was effectively shelved** — mid-training MoE-routing/attention issues at 2T scale; Kimi K2.6 took the "best open-weight flagship on SWE-bench" slot Behemoth was designed for.

## 1.5 License: the Llama Community License (through 2026)

- **Terms:** free for research and commercial use, but with restrictions that make it non-open-source by OSI standards: an **Acceptable Use Policy**, attribution requirements, a **"Llama" naming/trademark** clause, and — crucially — a clause requiring a **separate license from Meta for entities with >700M monthly active users**. The same MAU carve-out has applied since Llama 2.
- Weights are **gated on Hugging Face**: users must accept the license before download. NVIDIA's FP8/FP4 redistributions inherit the same license (no separate HF auth needed).
- **No "Community License 2.0" was issued** — instead, Meta leapfrogged it: **Muse Glimmer (Aug 2026) shipped under plain Apache 2.0** with no MAU carve-out and no AUP, and Meta announced Muse Spark 1.2's weights will follow. This is the real 2026 licensing shift: from Llama Community License to genuine OSI-approved licensing.

## 1.6 Llama 5 — does not exist (as of Sept 2026)

Multiple independent sources confirm **no Llama 5 has been released**: "the newest Meta model in the [OpenRouter] catalog, since there is no Llama 5" (catalog read 2026-07-31); "Llama 5 (600B+, April 8 2026) does not exist… HF meta-llama holds no Llama-5 weights at all"; "As of August 2026, Meta has not released Llama 5". Notably, the internal codename **"Avocado" became Muse Spark, not Llama 5**. Some trackers forecast a possible 2027 Llama 5; consensus put a 2026 ship below 20%.

**The benchmark controversy (relevant context):** Meta submitted an unreleased experimental variant (`Llama-4-Maverick-03-26-Experimental`) to LMArena that scored far above the public model; in January 2026 Yann LeCun publicly acknowledged the manipulation. This damaged Meta's open-weight credibility and is widely cited as a driver of the 2026 strategy pivot.

## 1.7 Meta's 2026 open-weight strategy shifts — summary

1. **April 2026: Llama brand retired** — Meta pivots from open weights to the closed, API-monetized **Muse Spark** line (first proprietary Meta model ever). "Meta's last open release was Llama 4, and Llama 4's adoption was 'disappointing' by Meta's own admission."
2. **July 2026: Meta Model API goes public/paid** — Meta joins the metered-API revenue model it once undercut, with an aggressive Contributor tier ($0.10/$0.20) that trades price for training-data rights.
3. **August 10, 2026: the reversal** — **Muse Glimmer 30B under Apache 2.0** (genuinely OSI-approved, no MAU carve-out) plus a promise to open-source **Muse Spark 1.2's** weights. The strategy is now **two-track: closed API for enterprise revenue, open weights for ecosystem leverage** — and Zuckerberg's accompanying essay reframes open weights as a US-competitiveness/national-security argument.
4. **Infrastructure scale-up underpins it:** ~$14B Scale AI investment, up to **$60B AMD chip deal** (Feb 2026), **$115–135B AI capex** reported around earnings (single weak source; treat as indicative).

---

# PART 2 — META MUSE SPARK FAMILY

*(Merged from two independent research passes; discrepancies across outlets are flagged inline rather than smoothed over.)*

## 2.1 What it is

**Muse Spark** is Meta's proprietary frontier reasoning-model family and the **first product of Meta Superintelligence Labs (MSL)**, the division led by Chief AI Officer **Alexandr Wang** (joined Meta from Scale AI in 2025; Meta invested ~$14–14.3B in Scale AI to bring him over). Debuted **April 8, 2026**. Internally codenamed **"Avocado"**.

It is a **natively multimodal reasoning model**: text, image, video, PDF, audio in; text out. Built around long-horizon agentic work: tool use, computer use, MCP servers, custom skills, multi-agent orchestration (main agent delegating to parallel subagents, or running as a subagent). Meta says it was built after a **9-month rebuild of its AI stack "from scratch"** (new infrastructure, new architecture, new data pipelines).

## 2.2 Release timeline (through Sept 2026)

| Version | Date | Notable |
|---|---|---|
| **Muse Spark 1.0** | 2026-04-08 | First MSL model; Meta AI app/meta.ai free tier; API invite-only |
| **Muse Spark 1.1** | 2026-07-09 | First **paid Meta Model API** (public preview, US developers at launch); 1M context; Thinking mode in Meta AI app |
| **Muse Spark 1.2** | 2026-08-05 | Co-released with **Muse Code** terminal coding agent (co-trained with it); global public-preview API expansion; OpenRouter listing; API out of beta 2026-08-31 (subscription option added) |
| **Muse Glimmer 30B** (sibling, open weights) | ~2026-08-10 | 30B dense local agentic model, Apache 2.0, HF: `meta-models/Muse-Glimmer-30B` |
| **Muse Spark 1.3** | 2026-09-02 | Coding/long-context flagship; two variants — `muse-spark-1.3` (xhigh, public), `muse-spark-1.3-max` (limited partner preview); reasoning_effort up to "max" (live 2026-09-04); paid API opened Sept 3 |
| **"Muse" consumer app** | 2026-09-08 | Personal agent app running Muse Spark 1.3 on per-user Meta-hosted VMs; free / $20/mo / $100/mo tiers |

**Meta Model API** (Meta's first metered inference API) public + paid since **July 9, 2026**.

## 2.3 Specs

- **Parameters:** Not disclosed by Meta for any Muse Spark version (Artificial Analysis FAQ: "Meta has not disclosed the model size or parameter count"). Every figure online is inference or error.
- **Architecture:** Described by third parties as a dense multimodal foundation model / hybrid reasoning transformer.
- **Context window:** **1M tokens** for 1.1/1.2/1.3 (standard figure). *Discrepancies:* Artificial Analysis lists the original Muse Spark at **260K tokens**; one dev.to comparison table lists 1.2 at 128K (131,072) — minority report, likely error.
- **Reasoning control:** `reasoning_effort` parameter from `minimal` to `xhigh` (1.3 adds `max`); reasoning tokens billed as output. Consumer app modes: **Instant, Thinking, Contemplating** (Contemplating = multi-agent parallel reasoning).
- **License:** **Proprietary, closed weights, API-only** — Meta's first paid model API, a sharp break from the open-weight Llama tradition. *Discrepancies:* (a) one French consultant PDF claims 1.1 is open-weights under a "Llama 5 Community License" with HF deployment via vLLM/Ollama — not corroborated by any major outlet; (b) Memeburn reported Meta "will open the weights for Muse Spark 1.2," again not corroborated by Meta's own blog. Treat "open weights" claims for the Spark line as **unverified**.

## 2.4 Availability

- **Consumer surfaces:** Meta AI app and meta.ai (free); rolled out across **WhatsApp, Instagram, Facebook, Messenger**, and Ray-Ban Meta AI glasses.
- **Developer API:** Meta Model API, base URL `api.meta.ai/v1`, supports **OpenAI Chat Completions and Anthropic Messages formats**; model names `muse-spark-1.1`, `muse-spark-1.2`, `muse-spark-1.2-contributor`, `muse-spark-1.3`. Built-in web-search grounding (`{"type": "web_search"}` tool). New accounts get a **$20 one-time credit**. Zero-data-retention arrangements available on request. OpenRouter listing from 1.2. Early partners: Replit, Cline, Box.
- **Muse Code** (terminal coding agent, macOS/Linux beta, launched Aug 5, 2026): parallel subagents in isolated git worktrees; replay-safe local event log; default skills `/plan`, `/grill`, `/goal`. Competes with Claude Code/Codex; Wang frames the strategy as differentiating "by price rather than capabilities".
- **Hugging Face:** Muse Spark weights are **not** on HF (proprietary). Sibling **Muse Glimmer 30B** is open on HF under Apache 2.0 (see §2.7).

## 2.5 API pricing

| Item | Price |
|---|---|
| Input | **$1.25 / 1M tokens** |
| Output | **$4.25 / 1M tokens** (reasoning tokens billed as output) |
| Cached input | **$0.15 / 1M tokens** |
| Blended rate (7:2:1 cache/input/output) | ~$0.78 / 1M tokens (Artificial Analysis) |
| **Contributor tier** (`muse-spark-1.2-contributor` / `muse-spark-1.3-contributor`) | ~**$0.10 in / $0.20 out per 1M** ($0.002 cached input) — in exchange for Meta training rights on prompts/completions; rate-limited on rolling 5-hour token window |

Competitive position: undercuts GPT-5.6 Sol ($5/$30) and Claude Sonnet 5 ($2/$10), slightly above GPT-5.6 Luna ($1/$6) and Claude Haiku 4.5. Output runs ~30%+ faster than Claude Opus 5-class models; aggressive pricing aimed at high-volume agent workloads.

## 2.6 Benchmarks

**Artificial Analysis Intelligence Index (independent):**
- Muse Spark 1.0: **52** (top-5 globally at launch, behind GPT-5.4 57, Gemini 3.1 Pro 57, Claude Opus 4.6 53). *Inconsistencies flagged:* TechTimes later cited 43 for 1.0; AA's own FAQ page estimates 31.
- Muse Spark 1.1: **53**; cost/task ~$0.29.
- Muse Spark 1.2: **57** (v4.1.1).
- Muse Spark 1.3 (xhigh): **61**; 1.3 (max): **62** — behind only Claude Fable 5.1 (max, 66) and Claude Opus 5 (max, 63); tied with GPT-5.6 Sol (max) and Grok 4.6 (high). 1.3 gains come from agentic work + science. 1.3 (max): Tau3-Bench Banking **52%** (vs 47% xhigh), GDPval-AA v2 **1,754 Elo** (vs 1,709). Output speed 232.6 tok/s, TTFT 23.48s.

**1.0 launch-window results:** **HealthBench Hard 42.8%** — best of any frontier model at the time (vs GPT-5.4 40.1, Gemini 3.1 Pro 20.6); CharXiv Reasoning **86.4**; MMMU-Pro **80.5%**; Humanity's Last Exam **58%** (Contemplating mode) / 39.9% (third-party); weak spots: Terminal-Bench 2.0 **59.0** (vs GPT-5.4's 75.1), ARC-AGI-2 **42.5%** (vs Gemini 3.1 Pro 76.5%), GDPval-AA Elo **1427–1444** (vs GPT-5.4 1672). **SWE-bench Pro 55** — tied #1 with GPT-5.4 xHigh on the Scale leaderboard. Token-efficient: 58M output tokens on the AA index vs 157M (Claude Opus 4.6).

**1.2:** Terminal-Bench 2.1 **82.9%** (2nd to Claude Opus 5's 86.7); GDPval-AA v2 **1631 Elo** (#5 of all models tested, ahead of Claude Opus 4.8); SWE-bench Verified ~**54.2%** (third-party table); cost/task ~**$0.40** (among most cost-efficient at its level).

**1.3 (Meta-reported):** DeepSWE v1.1 **75.4%** (113 real-world tasks, 91 repos, 5 languages); Terminal-Bench 2.1 **88.8%** (tied with GPT-5.6 Sol); SWEAtlas CodeBase QnA **59.4%**; MRCR long-context retrieval **98.5%** (256K–512K) / **98.1%** (512K–1M); DeepSearchQA **89.4%** (vs GPT-5.6 93.0%); Agentic IF Index **57.8** (vs GPT-5.6 60.5). Efficiency: ~20% fewer tool calls, ~25% fewer tokens than 1.2. Trails Claude Opus 5 on JobBench, OSWorld 2.0, AutomationBench, GDPval-AA v2 in Meta's own table.

## 2.7 Muse Glimmer 30B — Meta returns to open weights (Apache 2.0)

**Release:** August 10, 2026.

| Attribute | Value |
|---|---|
| Architecture | **Dense 30B** (~29.6B), distilled from Muse Spark 1.2 via logit distillation; dedicated perception encoder; natively multimodal (text+image) |
| Context window | **120K+ tokens** (131K per one source) |
| License | **Apache 2.0** — commercial use, modification, redistribution; no MAU cutoff, no AUP |
| HF weights | `meta-models/Muse-Glimmer-30B` (separate `meta-models` org; +GGUF, -assistant, ExecuTorch-PTE variants) |
| Design goal | Agentic local model: fits **<20 GB** quantized on a single 24GB consumer GPU or Apple Silicon Mac; runs offline; **no first-party Meta API** |

**Benchmarks (Meta vendor-reported):** SWE-Bench Pro **51.2%**, AIME **94.7** (2026), GPQA **83.5**, SWE-Bench Verified **76%**, WildCodeBench **47.6%**; MCP Atlas **75.5–75.8** (sources vary slightly) vs Gemma4-31B 54.2 vs Qwen3.6-27B 62.5; OSWorld-Verified **65.9** vs Qwen3.6-27B 75.6. Bundled **DFlash** speculative drafter: RTX 5090 **74.9 → 233.4 tok/s** (3.1×); M5 Max 26.6 → 50.2 tok/s. vs Qwen3.6-27B: Qwen is the stronger pure coder (77.2% SWE-bench Verified); Glimmer wins on native multimodality and agent tooling.

## 2.8 Relationship to Llama

- Muse Spark is **not** an open-weights release and has **no migration path from Llama** ("fundamentally different deployment models"). Meta's stated rationale: Llama 4 (Apr 2025) failed commercially and its launch was marred by the LMArena benchmark scandal (acknowledged by Yann LeCun after his departure, Jan 2026).
- Alexandr Wang has promised to **open-source future (bigger) versions**; existing Llama models remain available but are in maintenance mode while frontier investment flows to Muse.
- One French source claims Muse Spark = instruction-tuned/RLHF version of a "Llama 5" base under a Llama 5 Community License — treat as unverified/single-source.

## 2.9 What distinguishes Muse Spark

- First paid Meta model API — strategic pivot from open weights to monetization under Wang/MSL; fast release cadence (1.0→1.3 in 5 months).
- Agentic-first design (context compaction over 1M tokens, multi-agent orchestration, computer use) with aggressive pricing aimed at high-volume agent workloads.
- Co-training of model + agent harness (Muse Spark 1.2 ↔ Muse Code).
- Strongest on: health/vision (1.0), long-horizon coding + long-context retrieval (1.3). Weakest on: abstract reasoning (ARC-AGI-2), open-ended agentic search vs OpenAI/Anthropic flagships.

---

# PART 3 — POOLSIDE — CODE-MODEL COMPANY

## 3.1 Company & business model

- **Founded 2023** by **Jason Warner** (ex-GitHub CTO) and **Eiso Kant** (ex-Athenian, acquired by Linux Foundation 2023). Paris-based (moved HQ from US to Paris ~Aug 2024).
- **Business model:** enterprises pay to run powerful coding models **on their own hardware** (self-hosted/VPC, inside the customer's security boundary, "no information leaves") rather than sending prompts to closed APIs. Core customers: government, defense, highly regulated industries; pitch framed around Western enterprises/governments avoiding data transfers to overseas vendors.
- **Model Factory:** internal platform automating architecture search + **RL from code execution feedback (RLCEF)** — models "improve by completing millions of tasks in tens of thousands of real-world software projects." Enables ~5-week release cadence and ~60-day train-to-release cycles.

## 3.2 Funding timeline

- ~2024: $126M initial raise (BCV-led, with Redpoint, Felicis et al.) around Paris HQ move; June 2024 reports of a $400M raise at a $2B valuation (TechCrunch via SiliconANGLE).
- **Oct 2024: $500M Series B at $3B valuation** (Nvidia, eBay participated).
- **Apr 2026: planned $2B Series C at $14B valuation fell through** after CoreWeave withdrew from a joint Texas datacenter project.
- **2026-08-21: NVIDIA deal — $6B non-exclusive license for the Model Factory + $1B investment at $12B pre-money valuation; 109 employees (most of technical staff; "fewer than 115 people built the model" per Kant) hired by NVIDIA; founders stay; described as neither acquisition nor acquihire.** Poolside's investor letter: missed a 6-week window to raise $2B for a 40,000-GB300 cluster, lost the cluster; frontier training now constrained by datacenter space and contracted compute. Infra arm (PIC) spun out Jan 2026, building a 1.2GW Texas datacenter toward 7GW.

## 3.3 Model lineup (2026: the Laguna family; Malibu = legacy)

**Legacy: Malibu / Point (pre-2026, enterprise-only, now superseded)**
- **Malibu** (as of May 2025): flagship model for deep reasoning via chat — code generation, test writing, refactoring, documentation; **>1M token context**; built from scratch, fine-tuned on each customer's environment via RLCEF, deployed inside customer security boundaries.
- **Point**: real-time code completion model, ~100K context.
- **Malibu 2.2** (referenced in partner materials): dense architecture, 128K context, enterprise license, for low-latency IDE completion/inline editing.
- Status in 2026: no Malibu family on poolside.ai/models or the blog — treated as **legacy/superseded by Laguna**.

**Laguna XS.2** — released **2026-04-28**. 33.4B total / 3B active (per one guide; others say undisclosed), 256K context, Apache 2.0, single-GPU/Apple Silicon friendly. SWE-Bench Verified **64%** (vendor). Superseded by XS 2.1 (retired from API/OpenRouter 2026-07-09).

**Laguna M.1** — released **2026-04-28**. 225B total / 23B active MoE, 262,144 (256K) context, enterprise availability. Benchmarks: SWE-Bench Verified **65.4%** (vendor; one blog claims 72.5% for M.1 — flagged), SWE-Bench Multilingual **63.1%**, SWE-Bench Pro (public) **49.2%**, Terminal-Bench 2.0 **44.9%**.

**Laguna XS 2.1** — released **2026-07-02**. 33B total / 3B active MoE, 256K context, **OpenMDW-1.1** license, free on HF + free OpenRouter tier; runs on a single GPU. SWE-Bench Multilingual **63.1%** (+5.4pp vs XS.2), Terminal-Bench 2.0 **37.5%**.

**Laguna S 2.1** — released **2026-07-21**. Flagship; "the West's most capable open-weight model" (GlobeNewswire release).
- **Params/architecture:** 118B total / **8B active** MoE — 256 routed experts + 1 shared expert, grouped-query attention, interleaved sliding-window layers (per HF model card). Trained via Model Factory on 4,000 Nvidia H200 GPUs in <4 weeks; training began 2026-05-22 → 60-day cycle.
- **Context:** 1,048,576 (1M) tokens. Inference scales with the 8B active params; runs on a single **Nvidia DGX Spark** (desktop AI machine); official 4-bit Apple MLX weights (71.9GB download — real target: 128GB Mac); 4-bit GGUF ~75GB.
- **License:** **OpenMDW-1.1** — a new permissive license for model weights backed by the Linux Foundation; commercial use, modification, redistribution allowed.
- **Hugging Face:** `huggingface.co/poolside/Laguna-S-2.1` — open weights, day one. Also official Apple MLX weights.
- **Benchmarks (vendor, run with Poolside's `pool` agent harness, reproducible):** Terminal-Bench 2.1 **70.2%**; SWE-Bench Multilingual **78.5%**; SWE-Bench Pro (public) **59.4%**; DeepSWE v1.1 **40.4%**; Toolathlon Verified **49.7%**. Comparables: Tencent Hy3 71.7% TB2.1; Nemotron 3 Ultra 56.4%; Claude Sonnet 5 80.4% / 63.2% Pro; GPT-5.6 Luna Max 82.5% TB.
- **Availability & pricing:** HF download; **OpenRouter** `poolside/laguna-s-2.1` (paid, full 1M context) at **$0.10 / 1M input, $0.20 / 1M output** — an order of magnitude below frontier pricing; free tier `poolside/laguna-s-2.1:free` (capped 262,144 context), plus free XS 2.1 / M.1 routes; Baseten model library; Vercel AI Gateway; vLLM/SGLang/Ollama/llama.cpp support; free demo chat at chat.poolside.ai (no login); platform.poolside.ai API.

## 3.4 What distinguishes Poolside

- Only Western lab shipping frontier-adjacent **open-weight agentic coding models** at this cadence (positioned explicitly against Chinese open-weight dominance and US closed APIs).
- "Token economics" thesis: sparse MoE means inference cost follows active params; on hardest benchmarks the model burns a mean ~249K completion tokens/trajectory in thinking mode — self-hosting, not per-token API, is the economic answer for enterprise agents.
- Behavioral training emphasis ("working habits": verification, persistence) over raw scale.
- Open-ended future post-NVIDIA deal: founders pivoting the remaining company toward an undisclosed direction; the thesis that intelligence-bound problems commoditize while experiment-bound problems hold value ("the world's most valuable scientific discovery engine").

## 3.5 Side-by-side (Sept 2026 snapshot)

| | Muse Spark 1.3 (Meta) | Laguna S 2.1 (Poolside) |
|---|---|---|
| Release | 2026-09-02 | 2026-07-21 |
| Params | Undisclosed (proprietary) | 118B total / 8B active MoE |
| Context | 1M | 1M |
| Weights/license | Proprietary, API-only | Open weights, OpenMDW-1.1 (commercial OK) |
| HF | No | Yes (`poolside/Laguna-S-2.1`) |
| AA Intelligence Index | 61 (xhigh) / 62 (max) | n/a (coding-specialist) |
| Terminal-Bench 2.x | 88.8% (2.1) | 70.2% (2.1) |
| DeepSWE v1.1 | 75.4% | 40.4% |
| API price in/out per 1M | $1.25 / $4.25 | $0.10 / $0.20 (OpenRouter paid) |
| Channel | Meta AI app, WhatsApp/IG/FB, Meta Model API, Muse Code, OpenRouter | Self-host, HF, OpenRouter (free+paid), Baseten, Vercel AI Gateway |
| Audience | Consumers + developers/enterprise | Developers, enterprises, governments (self-host) |

---

# PART 4 — MISTRAL AI (OPEN MODELS)

**Vendor context:** French AI startup; ~$400M ARR (Jan 2026), ~$23B valuation (June 2026 round). Strategy is "open weights, proprietary platform": Apache 2.0 / MIT-licensed weights distributed via Hugging Face, revenue from La Plateforme API, Forge training platform, and Mistral Compute (18,000 NVIDIA Blackwell GPUs). All recent open models train on NVIDIA hardware and are vLLM/SGLang-compatible.

## 4.1 Mistral Large 3 — flagship open-weight MoE
- **Release:** December 2, 2025. 10-model "Mistral 3" release.
- **Parameters/architecture:** 675B total / 41B active per token, sparse MoE. Trained from scratch on ~3,000 NVIDIA H200 GPUs.
- **Context window:** 256K–262K tokens. Multimodal (text + vision), multilingual.
- **License:** Apache 2.0.
- **HF weights:** `mistralai/Mistral-Large-3-675B-Instruct-2512`. API id `mistral-large-latest` / `mistral-large-2512`. Runs on a single 8×A100/H100 node with vLLM.
- **Benchmarks:** LMArena debut at **#2 open-source non-reasoning model**. MMLU ≈ 85.5% (8-language); MMLU-Pro 73.11%; MATH-500 93.60%; HumanEval ≈ 92% pass@1; GPQA Diamond ≈ 43.9% (non-reasoning model; strong on broad knowledge, weaker on hard reasoning).
- **API pricing (La Plateforme, May 2026):** $0.50 / $1.50 per 1M input/output tokens.
- **Distinguisher:** largest Apache 2.0 open-weight model in the world at release; frontier non-reasoning quality at ~½ the API cost of closed frontier models; sovereign-European, enterprise-focused positioning.

## 4.2 Mistral Medium 3.5 — flagship "merged" model
- **Release:** April 29/30, 2026.
- **Parameters/architecture:** 128B **dense** transformer + vision encoder trained from scratch (variable image sizes/aspect ratios). Self-hostable on 4 GPUs.
- **Context window:** 256K tokens.
- **License:** **Modified MIT** (a license change worth flagging — several commentators noted Mistral was "loose with the word open source" here; not Apache 2.0).
- **HF weights:** open weights published; API + Le Chat/Vibe. Powers Mistral's Vibe CLI coding agent and Le Chat Work mode.
- **Benchmarks (vendor-reported):** SWE-Bench Verified **77.6%** (ahead of its own Devstral 2 ~72% and Qwen 3.5 397B-A17B ~75%); τ³-Telecom 91.4%; τ³-Airline 72.0; τ³-Retail 76.1.
- **Distinguisher:** Mistral's "first flagship merged model" — it **replaces/retires three lines**: Devstral 2 (coding), Magistral (reasoning), and Medium 3.1. One weight set with configurable per-request `reasoning_effort` (`none` for instant chat → `high` for deep agentic reasoning with `[THINK]` traces).

## 4.3 Mistral Small 4 — efficient unified MoE
- **Release:** March 16, 2026.
- **Parameters/architecture:** 119B total / **~6B active** per token (8B incl. embeddings/output) sparse MoE. Inference stack co-developed with NVIDIA (day-0 NIM, vLLM, SGLang).
- **Context window:** 262K tokens. Multimodal.
- **License:** Apache 2.0.
- **Benchmarks (vendor-reported):** "close to the level of Mistral Medium 3.1 and Mistral Large 3, particularly in MMLU Pro"; Artificial Analysis **LCR (Live Code Reasoning) 0.72** — beats OpenAI's GPT-OSS 120B; Qwen 3.5 122B and Qwen 3-next 80B still beat it on LiveCodeBench. Key claim: shortest instruct-mode outputs of any model tested (2.1K chars vs 14.2K Claude Haiku, 23.6K GPT-OSS 120B) → ~3× throughput vs Mistral Small 3, lower inference cost.
- **Distinguisher:** unifies instruct + reasoning + vision + coding in one open-weight model; accuracy-per-token efficiency play for the 80–120B class.

## 4.4 Devstral 2 — agentic coding model (retired)
- **Release:** December 9–10, 2025. Co-developed with All Hands AI (OpenHands).
- **Parameters:** 123B (plus Devstral Small 2: 24B). Context 262K. License Apache 2.0.
- **Benchmarks:** SWE-Bench ≈ 46.8% (per Red Hat intel doc; another third-party summary cites ~72% SWE-Bench Verified — figures conflict across harnesses, treat as directional).
- **Status:** retired/replaced by Mistral Medium 3.5 (Apr 2026) in the Vibe CLI.

## 4.5 Magistral — reasoning models (retired)
- **Magistral Small (24B):** June 10, 2025; Apache 2.0; open on HF. **Magistral Medium:** enterprise, API/premier access (not open weights).
- **Benchmarks (vendor-reported, June 2025):** Magistral Medium AIME-24 73.6% (90% with majority voting); underperformed Gemini 2.5 Pro and Claude Opus 4 on GPQA Diamond / AIME / LiveCodeBench; 10× faster responses claimed in Le Chat; strong multilingual coverage.
- **API pricing:** Magistral Medium $2/$5 per 1M in/out (2025); catalogs list magistral-medium $2.00/$8.00, magistral-small $0.50/$1.50.
- **Status:** retired into Mistral Medium 3.5 (Apr 2026) for Le Chat reasoning.

## 4.6 Ministral 3 — edge dense models
- **Release:** December 2, 2025. Sizes 3B / 8B / 14B dense; base, chat, and reasoning variants. Context 131K–262K. Apache 2.0, HF open.
- **Benchmarks:** 14B reasoning variant hits **85% on AIME 2025** with far fewer tokens than comparable models.

## 4.7 Other 2026 Mistral open releases (brief)
- **Voxtral TTS** (Mar 23, 2026): open-weight TTS on Ministral 3B backbone; 9 languages, zero-shot voice cloning; **CC BY-NC 4.0** (non-commercial — not permissive). Voxtral Mini Transcribe / Small for speech-to-text, mostly open.
- **Robostral Navigate** (8B, 2026): embodied navigation VLM, single RGB camera, **76.6% on R2R-CE**; sim-trained on 400K trajectories; Apache 2.0 — first Mistral robotics model, part of the Physical AI push (with Emmi AI acquisition, May 2026).
- **Leanstral** (Mar 16, 2026): Mistral Labs release for formal proof engineering (Lean 4).

---

# PART 5 — NVIDIA NEMOTRON (OPEN RELEASES)

**Vendor context:** NVIDIA's own model family, positioned as the open standard for **agentic AI workloads**. Differentiator vs Llama (and vs most open releases): NVIDIA publishes **weights + training data + RL environments + post-training recipes + fine-tuning code** — "fully open, not open in the marketing sense." Note NVIDIA previously built on Meta's weights (Llama-3.1-Nemotron 70B, Llama Nemotron reasoning family Apr/May 2025: Nano 4B / Super 49B / Ultra 253B); the Nemotron 3 generation is original NVIDIA architecture, optimized for NVIDIA GPUs (NVFP4 on Blackwell).

## 5.1 Nemotron 3 family (Nano / Super / Ultra)

- **Releases:** Nano — Dec 15, 2025; Super — Mar 11, 2026 (GTC); Ultra — Jun 4, 2026 (Computex/GTC Taipei keynote).
- **Architecture:** hybrid **Mamba-Transformer MoE** (Nemotron-H / latent MoE). Learned MLP router: Nano activates 6 of 128 experts per forward pass (~10% active).
- **Sizes:** Nano 31.6B total / ~3.2–3.6B active; Super 120B total / ~12B active; Ultra **550B total / 55B active** (~90% sparse; 108 layers, 8192 dim, 512 experts/layer, top-22 routing). Ultra pretrained on 20T tokens (Warmup-Stable-Decay); post-training SFT → RLVR → MOPD → MTP Boosting.
- **Context window:** **1M tokens** (Ultra NVFP4; BF16 HF checkpoint ships 262K); Super 1M (256K HF); Nano 1M (262K HF).
- **License:** NVIDIA Open Model License (Nano family); Ultra governed by **OpenMDW-1.1** (per NVIDIA API docs) — both permit commercial use and redistribution.
- **HF weights:** `nvidia/NVIDIA-Nemotron-3-Ultra-550B-A55B-BF16` (+ NVFP4 and FP8 variants), Nano/Super equivalents under `nvidia/`. Also on AWS Bedrock, Google Cloud, NVIDIA NIM, OpenRouter, Together, Fireworks, DeepInfra, SageMaker JumpStart.
- **Features:** reasoning ON/OFF control with configurable thinking-token budget; built-in MTP head for speculative decoding; reasoning-optimized agent scaffolds.
- **Benchmarks — Nemotron 3 Ultra** (HF model card + NVIDIA API docs):
  - Artificial Analysis **Intelligence Index 48** — highest of any **US-origin open-weight model** (vs Gemma 4 31B: 39, Nemotron 3 Super: 36, gpt-oss-120b: 33; China's Kimi K2.6: 54, DeepSeek V4 Pro: ~56)
  - SWE-Bench Verified **70.7** (HF card; NVIDIA docs: 71.9 BF16 / 69.7 NVFP4); range 65–70.4% across five agent harnesses (Pi, OpenHands, Hermes, OpenCode, Mini SWE Agent)
  - SWE-Bench Multilingual 67.7; Terminal-Bench 2.1 56.4; GPQA (no tools) 87.0; LiveCodeBench v6 89.0; IOI 2025 570.0; RULER @1M 94.7; IFBench 81.7; τ-bench V3 avg 70.9; PinchBench 90.0; BrowseComp 44.4; HLE 26.7
  - **Throughput:** 300+ tok/s (DeepInfra pre-release, BF16); NVFP4 on GB200: 5.9× throughput vs GLM-5.1-754B, 4.8× vs Kimi K2.6 at 8K/64K; up to 30% lower cost-to-completion than peers
- **Benchmarks — Nemotron 3 Nano:** 3.3× throughput vs Qwen3-30B-A3B and 2.2× vs GPT-OSS-20B (8K in/16K out, single H200); beats both on RULER; 4× throughput of Nemotron 2 Nano; 60% fewer reasoning tokens; Artificial Analysis ranks Nemotron 3 most efficient among same-size open models. 13 early enterprise adopters (Accenture, CrowdStrike, Palantir, Perplexity).
- **API pricing (June–July 2026):** Ultra — OpenRouter $0.50/$2.50 (+$0.15 cached), DeepInfra $0.50/$2.20, Fireworks NVFP4 $0.60/$2.40 ($0.12 cached), Together $0.60/$3.60. Blended AA rate ≈ $0.52/1M.
- **Positioning vs Llama:** NVIDIA is building a Llama-independent open stack — original architectures, open data/recipes, GPU-co-designed efficiency — aimed at enterprise agentic workloads (coding agents, RAG, orchestration) where cost-per-task and long context matter more than chat benchmarks. Meta's Llama line was in maintenance mode by mid-2026, leaving NVIDIA as the leading US open-weight lab on the Intelligence Index.

## 5.2 Other 2026 Nemotron releases (brief)

- **Nemotron 3 Nano Omni** (Apr 28, 2026): 30B-A3B multimodal (text+image+audio+video), NVIDIA Open Model License.
- **Nemotron-Cascade-2-30B-A3B** (Mar 19, 2026): 30B-A3B, cascade RL, thinking+instruct unified.
- **Nemotron 3.5 Content Safety** (Jun 4, 2026): open-weight safety model, 128K context.
- **Nemotron Nano 2 12B** (Dec 2, 2025): hybrid Mamba-Transformer, 131K context. (Nano 2 9B: Aug 2025.)

---

# PART 6 — MICROSOFT PHI

**Status: incremental in 2026 — no Phi-5.** Two independent 2026 surveys confirm "Phi-5 unreleased" as of mid-2026.

- **Phi-4-reasoning-vision-15B** (Mar 2026) — current latest. Selective reasoning (model decides when to think), ~200B multimodal tokens; fits 12GB VRAM. MIT license.
- **Phi-4** (Dec 12, 2024, 14B dense, MIT); **Phi-4-mini** (3.8B); **Phi-4-multimodal** (5.6B, unified text+speech+vision). All MIT-licensed, HF + Ollama (`ollama pull phi-4`) + LM Studio.
- **Positioning as small local models:** Microsoft's SLM line is the reference "runs anywhere" family. **Foundry Local** reached GA (announced at Build 2026, milestone Apr 9): a ~20MB embeddable runtime (Windows/macOS Apple Silicon/Linux x64, no cloud, no per-token cost, OpenAI-compatible API, ONNX Runtime backend claiming 3.9× throughput over llama.cpp). Its curated GA catalog ships **Phi**, Qwen, DeepSeek, Mistral, Whisper — Phi is the first-party small-model default for on-device Windows/agent scenarios.
- **Note:** Phi-4-class models are now outclassed by newer small open models (e.g., Qwen3.5-9B, Gemma 4 E4B) on benchmarks; their value is MIT licensing + Microsoft distribution, not leadership.

---

# PART 7 — GOOGLE GEMMA

## 7.1 Gemma 4 (April 2, 2026) — license watershed

- **The headline change:** Gemma 1/2/3 used the restrictive **Gemma Terms of Use** (commercial MAU thresholds, redistribution limits). **Gemma 4 ships under Apache 2.0** — unlimited commercial use, full redistribution. This is Google's first direct entry into the open-weight enterprise market against Llama.
- **Lineup (4 sizes, base + instruction-tuned on HF):**
  - **E2B:** 2.3B effective (5.1B w/ embeddings), 128K, 35 layers — text/image/audio
  - **E4B:** 4.5B effective (8B w/ embeddings), 128K, 42 layers — text/image/audio
  - **26B-A4B (MoE):** 25.2B total / 3.8B active (128 experts, 8 routed + 1 shared per token), 256K, 30 layers — text/image/video
  - **31B (dense):** 30.7B, 256K (vals.ai: 262K), 60 layers — text/image/video
  - 262K vocabulary; 140+ languages (up from 35+ in Gemma 3); built-in thinking/reasoning mode; native function calling; system-prompt support; video understanding up to 60s @1fps (a Gemma first).
- **Architecture notes:** E-series uses **Per-Layer Embeddings (PLE)** — a secondary embedding signal fed into every decoder layer — so 2.3B-active behaves like 5.1B params and fits <1.5GB at 2-bit quant (verified on Raspberry Pi 5). Ships **MTP (Multi-Token Prediction) draft models** for speculative decoding: up to 2× speedup with identical output quality.
- **HF weights:** `google/gemma-4-31B-it`, `google/gemma-4-26B-A4B-it`, `google/gemma-4-E4B-it`, `google/gemma-4-E2B-it`; Kaggle; Ollama (`ollama pull gemma4`); LM Studio; vLLM; MLX (Apple Silicon); llama.cpp; LiteRT-LM for on-device. **Free API via Google AI Studio** (31B/26B).
- **Benchmarks — Gemma 4 31B (vendor-reported, instruction-tuned):** MMLU Pro **85.2**; AIME 2026 **89.2** (no tools; vs 20.8 for Gemma 3 27B); LiveCodeBench v6 **80.0**; Codeforces ELO 2150 (vs 110 for Gemma 3); GPQA Diamond 84.3; Vision MMMU Pro 76.9; MATH-Vision 85.6; agentic **τ²-bench 86.4%** (vs 6.6% for Gemma 3 — a categorical change for agent use); vals.ai: #1 on SAGE (55.03%), 53.92% on SWE-bench Verified subset, 38.94% Vals Index; AA Intelligence Index **39**.
- **Benchmarks — 26B-A4B MoE:** MMLU Pro 82.6; AIME 2026 88.3; LiveCodeBench v6 77.1; Codeforces 1718 — ≈97% of dense-31B MMLU-Pro quality at ~12% of the FLOPs; runs on a 24GB consumer GPU.
- **Positioning:** derived from the Gemini 3 research lineage; the 31B ranks **#3 among all open models** on the Arena AI text leaderboard; E2B/E4B are the only Western edge models with native audio at that size class (no Llama 4/Qwen 3.5 equivalent); powers Gemini Nano 4 on Android.
- **Pricing:** free via Google AI Studio; third-party hosted pricing is low/near-zero; self-host on a single H100 (31B) or consumer GPU (26B MoE).

---

# PART 8 — OTHER NOTABLE WESTERN OPEN-WEIGHT RELEASES

## 8.1 Allen Institute for AI — OLMo 3 (Nov 20, 2025)

- **Sizes/variants:** 7B & 32B; Olmo 3-Base (7B, 32B), Olmo 3-Think (7B, 32B — first Ai2 reasoning release), Olmo 3-Instruct (7B). Context 65K (doubled in pre/mid-training; sliding-window attention on 3 of 4 layer types).
- **License:** Apache 2.0. **Fully open** — the key differentiator: Dolma 3 training data (5.9T tokens for 32B), Dolci post-training stack, OlmoRL (SFT→DPO→RLVR), checkpoints, all public. Marketed as "best American-made open source model at this scale" and "best 7B Western instruct/thinking model."
- **Benchmarks:** Olmo 3-Think 32B — MATH ≈ 96.1%; HumanEval+ ≈ 91.4% (marginally ahead of Qwen 3 32B); AIME-style low-to-mid 70s; MMLU mid-80s; competitive with Qwen 3-32B-Thinking on ~1/6 the training tokens. Olmo 3-Think 7B: AIME 2025 70.7% (AA).
- **Positioning:** the transparency-first lab's answer to open-weight models; reproducibility and research use over raw scale.

## 8.2 IBM — Granite 4.0 family (Oct 2025 → 2026)

- **Architecture:** hybrid **Mamba-2 state-space + transformer** ("H" variants) plus traditional dense transformer fallbacks; linear compute scaling, constant memory with context length, >70% RAM reduction claimed on long-context workloads.
- **Releases:** Granite 4.0 core family (Oct 2025): H-Micro/Micro 3B, Small; **Granite 4.0 Nano** (Oct 29, 2025): 350M / 1B (plus transformer-only alternates) — run in-browser via Transformers.js, llama.cpp/vLLM/MLX compatible; **Granite-4.0-3B-Vision** (Mar 27, 2026): document/chart/table extraction VLM (`<chart2csv>`, `<tables_json>` task tags); **Granite 4.0 1B Speech** (Mar 2026): compact multilingual STT/translation (EN/FR/DE/ES/PT/JA + EN↔IT/ZH), two-pass design.
- **License:** Apache 2.0 throughout. First open model family with **ISO/IEC 42001** AI-management certification; cryptographically signed checkpoints; HackerOne bug bounty.
- **Benchmarks (IBM-reported):** Granite 4.0 H 1B — IFEval **78.5** (vs Qwen3 1.7B 73.1, Gemma 3 1B 59.3); Berkeley Function Calling Leaderboard v3 **54.8** (vs Qwen3 52.2, Gemma 3 16.3); Granite 4.0 1B Speech **#1 on OpenASR leaderboard** (avg WER 5.52, RTFx 280; LibriSpeech Clean 1.42). Strong on Stanford HELM.
- **Positioning:** enterprise trust + efficiency play (EY, Lockheed Martin testing); available on watsonx.ai, HF (`ibm-granite`), Docker Hub, Kaggle, NVIDIA NIM, Dell Pro AI Studio.

## 8.3 Nous Research — Hermes 4 (2026 release cycle)

- **What:** community open-weight lab's "hybrid reasoning" family — neutrally aligned (minimal refusals), toggleable `<think>`-tag reasoning, trained on ~5M samples / 19B tokens of newly synthesized reasoning + instruction data (Distilabel pipeline) + RLHF, per the 40-page technical report (arXiv 2508.18255).
- **Sizes/licensing:** reported sizes vary across coverage (8B/14B/35B/70B; base architecture consistent with the Llama 3.x generation); weights + GGUF/AWQ quants on HF under `NousResearch` (e.g., the `hermes-4-collection`); no API gate, no registration.
- **Benchmarks (mixed sourcing — treat as directional):** Hermes 4 35B ≈ 83 on GPQA Diamond per a Delphi Digital industry comparison (vs Qwen 3.5 88.4, Kimi K2.5 87.6); 405B-class variant reported at 96.3% MATH-500 / 81.9% AIME'24 in reasoning mode; top score on Nous's own RefusalBench (57.1% vs GPT-4o 17.67%).
- **Adjacent:** Nous shipped **Hermes Agent** (open-source MIT autonomous CLI agent, persistent memory, GEPA self-improving skill loop — ICLR 2026 oral) — evidence of the lab's agent-first strategy.

## 8.4 OpenAI — GPT-OSS (Aug 2025; relevant baseline)

- **gpt-oss-20b / gpt-oss-120b:** OpenAI's first open-weight release since GPT-2; Apache 2.0; hybrid reasoning (configurable effort); text-only.
- **Benchmarks:** AA Intelligence Index **33** for the 120B (June 2026); beaten by Mistral Small 4 on the AA LCR long-context reasoning benchmark; Nemotron 3 Nano beats it on RULER and throughput (2.2×).
- **Why included:** the US-lab open-weight baseline that 2026 releases are measured against.

## 8.5 Gaps / negative findings

- **Snowflake Arctic:** no 2026 successor found; Arctic 2 (Dec 2024) remains the latest — excluded.
- **Databricks DBRX successors:** nothing found in 2026 — excluded.
- **xAI:** no open-weight release in 2026 found; Grok line remains closed — excluded.
- **Apertus (ETH Zurich/EPF Lausanne, Sep 2025, 70B, Apache 2.0)** — first EU-AI-Act-compliant LLM, fully open; Swiss/European but worth a cross-reference for the "sovereign open" theme.

---

# PART 9 — MASTER COMPARISON TABLE (Sept 2026 snapshot)

| Model | Vendor | Release | Params | Context | License | HF weights | AA Intelligence Index | SWE-bench Verified | API in/out per 1M |
|---|---|---|---|---|---|---|---|---|---|
| Llama 4 Maverick | Meta | 2025-04-05 | 400B / 17B act | 1M | Community License | ✅ | 9.3 | 21.0% | $0.20 / $0.80 |
| Llama 4 Scout | Meta | 2025-04-05 | 109B / 17B act | 10M | Community License | ✅ | 6.5 | — | $0.19 / $0.68 |
| Muse Spark 1.3 | Meta | 2026-09-02 | undisclosed | 1M | Proprietary (API-only) | ❌ | 61 (xhigh) / 62 (max) | — | $1.25 / $4.25 |
| Muse Glimmer 30B | Meta | 2026-08-10 | 30B dense | 120K+ | Apache 2.0 | ✅ | — | 76% (vendor) | n/a (local) |
| Laguna S 2.1 | Poolside | 2026-07-21 | 118B / 8B act | 1M | OpenMDW-1.1 | ✅ | — (coding) | 59.4% Pro (vendor) | $0.10 / $0.20 |
| Mistral Large 3 | Mistral | 2025-12-02 | 675B / 41B act | 256K | Apache 2.0 | ✅ | — | — | $0.50 / $1.50 |
| Mistral Medium 3.5 | Mistral | 2026-04-29 | 128B dense | 256K | Modified MIT | ✅ | — | 77.6% (vendor) | via La Plateforme |
| Mistral Small 4 | Mistral | 2026-03-16 | 119B / 6B act | 262K | Apache 2.0 | ✅ | — | — | via La Plateforme |
| Nemotron 3 Ultra | NVIDIA | 2026-06-04 | 550B / 55B act | 1M | OpenMDW-1.1 | ✅ | **48** (top US open) | 70.7 (vendor) | $0.50 / $2.50 |
| Nemotron 3 Super | NVIDIA | 2026-03-11 | 120B / 12B act | 1M | NVIDIA Open Model License | ✅ | 36 | — | via NIM |
| Nemotron 3 Nano | NVIDIA | 2025-12-15 | 31.6B / 3.2B act | 1M | NVIDIA Open Model License | ✅ | — | — | via NIM |
| Gemma 4 31B | Google | 2026-04-02 | 30.7B dense | 256K | Apache 2.0 | ✅ | 39 | ~54% (vals.ai subset) | Free (AI Studio) |
| Gemma 4 26B-A4B | Google | 2026-04-02 | 25.2B / 3.8B act | 256K | Apache 2.0 | ✅ | — | — | Free (AI Studio) |
| OLMo 3 Think 32B | Ai2 | 2025-11-20 | 32B | 65K | Apache 2.0 | ✅ | — | — | self-host |
| Granite 4.0 H 1B | IBM | 2025-10 | 1B | long | Apache 2.0 | ✅ | — | — | self-host |
| Phi-4 | Microsoft | 2024-12-12 | 14B dense | 16K | MIT | ✅ | — | — | self-host |
| gpt-oss-120b | OpenAI | 2025-08 | 120B | 128K | Apache 2.0 | ✅ | 33 | — | self-host |

*AA Intelligence Index values are from AA v4.x snapshots (methodology-version dependent); vendor benchmarks are marked where relevant.*

---

# PART 10 — CROSS-CUTTING OBSERVATIONS (for the RAG)

## 10.1 Meta vs Chinese open-weights (2026)

The open-weight frontier in 2026 is **Chinese-led**, and Meta is no longer in the flagship race:

- **Top open-weights on AA Index:** MiMo-V2.6-Pro (Xiaomi, **46**), GLM-5.3 (Z.ai, **45–60** depending on index version), Qwen3.8 Max (Alibaba, **45**), Kimi K3 (Moonshot, **44–60**) — all shipping **MIT-licensed** (fully permissive) weights. MiMo-V2.6-Pro: 1.02T MoE / 42B active, 1M context, $0.43/$0.87. DeepSeek V4.1 Flash: 552B MoE, 1M context, MIT, **$0.15/$0.60 off-peak**.
- **Chinese open flagships match closed models:** Kimi K3 scores 60 on the AA Index — 3 points off the top closed model, "the narrowest open-vs-closed gap since February."
- **Meta's position:** Llama 4 (Apr 2025) is its last large open release — a generation behind. Meta's 2026 open contribution is **Glimmer (30B local-agent tier)**, not a flagship; its frontier effort is the **closed Muse Spark line**, which competes credibly on health/visual reasoning and price ($1.25/$4.25 undercuts Opus/GPT tiers) but trails Claude Opus 5, GPT-5.x, and Gemini 3.x on coding and raw intelligence.
- **Licensing contrast:** Chinese labs now ship MIT (Xiaomi, DeepSeek, Z.ai) while Meta's historical position was the restrictive Community License; Glimmer's Apache 2.0 is Meta catching up to the permissive norm.

## 10.2 License liberalization is the 2026 Western story

Gemma moved Terms-of-Use → Apache 2.0 (Apr 2026); NVIDIA's Nemotron 3 ships weights + data + recipes; Poolside's Laguna S 2.1 introduced OpenMDW-1.1 (Linux Foundation-backed); Meta's Glimmer went Apache 2.0. Counter-notes: Mistral introduced a *Modified MIT* for Medium 3.5 (watch this); Mistral's Voxtral TTS is CC BY-NC 4.0 (non-commercial).

## 10.3 Architecture convergence

Hybrid Mamba-Transformer MoE (Nemotron 3, Granite 4.0 H), latent/sparse MoE with ~10% active params (Mistral Large 3, Nemotron 3, Gemma 4 26B-A4B, Poolside Laguna S 2.1), MTP/speculative-decoding heads (Gemma 4, Nemotron 3), 1M-token context as the agentic standard (Nemotron 3, Muse Spark, Laguna S 2.1), 256K+ elsewhere.

## 10.4 US open-weight leadership (AA Intelligence Index, June–Sept 2026)

Nemotron 3 Ultra 48 > Gemma 4 31B 39 > Nemotron 3 Super 36 > gpt-oss-120b 33 — still trailing China's Kimi K2.6 (54) and DeepSeek V4 Pro (~56).

## 10.5 Agentic benchmarks replaced chat benchmarks

SWE-Bench Verified, Terminal-Bench 2.x, τ-bench V3, PinchBench, τ²/τ³-bench, RULER, IFBench dominate vendor reporting in 2026.

## 10.6 Business-model patterns

- **Open weights + paid platform** (Mistral: La Plateforme/Forge/Compute; Poolside pre-NVIDIA: self-hosted enterprise).
- **Proprietary API pivot** (Meta Muse Spark: first paid Meta Model API, Contributor tier trading price for training rights).
- **GPU-vendor open stack** (NVIDIA Nemotron: weights+data+recipes as an on-ramp to NIM/Blackwell inference).
- **Execuhire / licensing deals** as the 2026 exit path (NVIDIA × Poolside: $6B Model Factory license + $1B investment, staff hired).
- **Sovereignty framing** (Mistral's European positioning; Zuckerberg's open-weights-as-US-competitiveness essay; Apertus EU-AI-Act compliance).

---

# PART 11 — CAVEATS & METHODOLOGY

- Benchmark figures are vendor-reported unless marked as independently measured (Artificial Analysis Intelligence Index, vals.ai, LMArena, third-party tables).
- Harness variance is large: Nemotron 3 Ultra SWE-Bench spans 65–70.4% across five harnesses; Devstral 2 figures conflict across sources (46.8% vs ~72%); Muse Spark 1.0's AA Index is cited as 52 / 43 / 31 across outlets — record methodology version.
- API prices move frequently; all pricing figures are June–September 2026 snapshots from the cited aggregators (llm-stats.com, Artificial Analysis, vendor docs), not live quotes.
- Muse Spark parameter counts and architecture internals are undisclosed by Meta — every figure online is inference or error. "Open weights" claims for the Spark line (Memeburn on 1.2; a French PDF on 1.1) are uncorroborated — treat as unverified.
- The Llama topic attracts fabricated "2026" aggregator content (e.g., a nonexistent "Llama 5" model page). **No Llama 5 exists as of Sept 2026.**
- Poolside's post-NVIDIA pivot direction is undisclosed as of Sept 2026; no August/September 2026 model releases found — Laguna S 2.1 (July 21) remains the current flagship.
- Meta's $115–135B AI capex figure rests on a single weak source; treat as indicative.
- Nemotron 3 Ultra's documented knowledge cutoff is 2025-09-30; exact Mistral Large 3 knowledge cutoff is undisclosed.

---

# PART 12 — KEY SOURCES

**Benchmarks & rankings:** Artificial Analysis — [Llama 4 Scout](https://artificialanalysis.ai/models/llama-4-scout) · [Llama 4 Maverick](https://artificialanalysis.ai/models/llama-4-maverick) · [Muse Spark](https://artificialanalysis.ai/models/muse-spark) · [Muse Spark 1.3](https://artificialanalysis.ai/models/muse-spark-1-3) · [Muse Spark 1.3 article](https://artificialanalysis.ai/articles/muse-spark-1-3) · Scale [SWE-bench Pro leaderboard](https://labs.scale.com) · [OpenTools (Maverick)](https://opentools.ai/llms/llama-4-maverick) · [OpenTools (Scout)](https://opentools.ai/llms/llama-4-scout) · [aiwiki Llama 4](https://aiwiki.ai/wiki/llama_4_scout_maverick) · vals.ai [Gemma 4 31B](https://www.vals.ai/models/google_gemma-4-31b-it)
**Weights (Hugging Face):** `meta-llama/Llama-4-Scout-17B-16E-Instruct` · `meta-llama/Llama-4-Maverick-17B-128E-Instruct` · `meta-models/Muse-Glimmer-30B` · `poolside/Laguna-S-2.1` · `mistralai/Mistral-Large-3-675B-Instruct-2512` · `nvidia/NVIDIA-Nemotron-3-Ultra-550B-A55B-BF16` · `google/gemma-4-31B-it` (+ 26B-A4B, E4B, E2B) · `ibm-granite/*` · `NousResearch/*`
**Pricing:** [OpenRouter catalog (2026-07-31)](https://github.com/deluair/econ-eval) · [GrowthMethod 2026 model table](https://growthmethod.com/ai-models-for-marketers/) · [llm-stats.com](https://llm-stats.com/models/compare/kimi-k2.7-code-vs-nemotron-3-ultra-550b-a55b) · [NVIDIA API docs (Nemotron 3 Ultra)](https://docs.api.nvidia.com/nim/reference/nvidia-nemotron-3-ultra-550b-a55b) · [HF model card (Nemotron 3 Ultra)](https://huggingface.co/nvidia/NVIDIA-Nemotron-3-Ultra-550B-A55B-BF16)
**Open-weights landscape:** [CheapestInference — State of Open Weights](https://cheapestinference.com/reports/state-of-open-weights/) · [buyer's guide](https://cheapestinference.com/reports/which-llm/) · [CheapestInference FAQ](https://cheapestinference.com/faq/)
**Meta:** [Llama 4 announcement](https://ai.meta.com/blog/llama-4-multimodal-intelligence/) · [meta-llama/llama-models](https://github.com/meta-llama/llama-models) · [mungomash Llama versions](https://mungomash.com/ai/llama/versions/) · [9to5Mac — Muse Spark launch](https://9to5mac.com/2026/04/08/goodbye-llama-meta-unveils-muse-spark-ai-with-new-contemplating-mode/) · [VentureBeat — goodbye Llama](https://venturebeat.com/technology/goodbye-llama-meta-launches-new-proprietary-ai-model-muse-spark-first-since) · [nerdleveltech](https://nerdleveltech.com/meta-muse-spark-proprietary-ai-model-benchmarks) · [felloai](https://felloai.com/meta-muse-spark/) · [uristocrat](https://uristocrat.com/meta-launches-muse-spark-first-reasoning-model-from-alexandr-wangs-superintelligence-labs/) · [neuralcoretech](https://neuralcoretech.com/meta-muse-spark-review-benchmarks-2026/) · [flowtivity.ai — Spark 1.3](https://flowtivity.ai/blog/meta-muse-spark-1-3-benchmarks-ai-agents/) · [pulse2 — Spark 1.2 + Muse Code](https://pulse2.com/meta-launches-muse-spark-1-2-and-muse-code-coding-agent-with-1-million-token-context-window/) · [technology.org](https://www.technology.org/2026/08/07/meta-muse-code-muse-spark-1-2-coding-agent/) · [mlq.ai — Spark 1.1 API](https://mlq.ai/news/meta-launches-muse-spark-11-api-its-first-paid-ai-model-to-challenge-anthropic-and-openai/) · [pondero.ai — Spark 1.1](https://pondero.ai/news/2026-07-12-meta-muse-spark-1-1-launch-model-api/) · [memeburn — open weights claim](https://memeburn.com/meta-will-open-the-weights-for-muse-spark-1-2/) · [techtimes — Spark 1.3](https://www.techtimes.com/articles/326417/20260903/muse-spark-13-jumps-16-points-deepswe-how-meta-training-loop-closed-gap.htm) · [tech-insider.org — Spark vs Astra vs Fable](https://tech-insider.org/muse-spark-vs-gpt-6-astra-vs-claude-fable-2026/) · [miraflow.ai](https://miraflow.ai/blog/meta-ended-llama-built-muse-spark-changes-everything-2026) · [webpronews](https://www.webpronews.com/metas-bold-ai-pivot-ditching-llamas-open-weights-for-muse-sparks-closed-power-play/) · [tbreak — Muse Code](https://tbreak.com/meta-muse-code-muse-spark-1-2/) · [ecorpit — Spark 1.1 coding](https://ecorpit.com/meta-muse-spark-1-1-coding-agent-model-dev-teams-2026/) · [cryptonomist — Muse Code](https://en.cryptonomist.ch/2026/08/06/meta-muse-code-agent/) · [agentbuildershq — Muse Code](https://agentbuildershq.com/blog/what-is-meta-muse-code)
**Muse Glimmer:** [tech-insider.org](https://tech-insider.org/meta-muse-glimmer-open-weight-ai-model-2026/) · [singularitybyte](https://singularitybyte.com/models/meta-muse-glimmer-30b-local-agent-model.html) · [opensourceforu](https://www.opensourceforu.com/2026/08/meta-open-sources-muse-glimmer/) · [temperature2](https://temperature2.com/p/2026-08-10-meta-muse-glimmer-open-weight-model/) · [DecodingAI/Medium](https://medium.com/@aerator_rolls.27/metas-muse-glimmer-puts-a-30b-ai-agent-on-one-consumer-gpu-90830bf14283) · [llmcheck](https://llmcheck.net/blog/llama-5-405b-review/) · [clawfit research watch](https://github.com/hongsw/clawfit/blob/HEAD/docs/research-watch/2026-08-10-meta-muse-glimmer-30b-open-agentic-model.md)
**Meta strategy commentary:** [codersera Llama 4 guide (2026)](https://codersera.com/blog/llama-4-complete-guide-2026/) · [balakumar.dev](https://blog.balakumar.dev/2026/07/09/meta-just-killed-llama-and-nobody-is-saying-it-out-loud/) · [tokenmix (Behemoth)](https://tokenmix.ai/blog/llama-4-behemoth-still-training-2026)
**Poolside:** [VentureBeat — Laguna S 2.1](http://venturebeat.com/infrastructure/poolside-drops-laguna-s-2-1-an-open-weight-coding-model-that-beats-rivals-10x-its-size) · [GlobeNewswire release](https://markets.financialcontent.com/1discountbrokerage/article/gnwcq-2026-7-21-poolside-releases-laguna-s-21-the-wests-most-capable-open-weight-model) · [the-decoder](https://the-decoder.com/poolsides-laguna-s-2-1-is-a-small-open-weight-coding-model-that-punches-well-above-its-size/) · [pymnts — NVIDIA deal](https://www.pymnts.com/news/artificial-intelligence/2026/nvidia-pays-6-billion-to-license-ai-model-development-software/) · [ai-roundup — NVIDIA/Poolside](https://github.com/rinti/ai-roundup/blob/HEAD/roundups/2026-08-21-poolside-flips-the-execuhire-att-goes-open-wayfinder-maps-the-fog.md) · [zeronoise — NVIDIA deal](https://zeronoise.ai/posts/nvidias-reported-poolside-deal-puts-the-model-factory-at-the-center-of-ai-competition-gfkjmwplop/download/pdf) · [codetodeploy/Medium — Laguna local](https://medium.com/codetodeploy/this-open-weight-coding-model-makes-local-agents-real-on-a-128gb-mac-2adc24c6a5c9) · [9cv9 guide](https://blog.9cv9.com/the-complete-guide-to-the-laguna-models-by-poolside-ai/) · [Techtimes — XS 2.1](https://www.Techtimes.Com/articles/319676/20260704/poolside-releases-free-open-weight-coding-model-july-9-upgrade-deadline.htm) · [contrary research](http://research.contrary.com/company/poolside) · [wed oany](http://en.wedoany.com/shortnews/420681.html) · [siliconangle — raise](https://siliconangle.com/2024/06/20/ai-coding-assistant-startup-poolside-reportedly-raising-400m/)
**Mistral:** [dev.to — Large 3 guide](https://dev.to/jangwook_kim_e31e7291ad98/mistral-large-3-the-675b-open-weight-moe-model-developer-guide-250a) · [IntuitionLabs](https://intuitionlabs.ai/articles/mistral-large-3-moe-llm-explained) · [airevolution — 10-model release](https://airevolution.poltextlab.com/mistral-launches-its-new-10-model-release/) · [LLM-Model-Comparison-2026](https://github.com/salttechno/LLM-Model-Comparison-2026) · [letsdatascience — Medium 3.5](https://letsdatascience.com/blog/mistral-medium-3-5-128b-open-weight-merged-model) · [nestfrontier — Medium 3.5](https://nestfrontier.com/mistral-medium-35-128b-one-model-to-rule-coding-reasoning-and-chat) · [firethering — Medium 3.5](https://firethering.com/mistral-medium-3-5-unified-model/) · [VentureBeat — Small 4](https://venturebeat.com/technology/mistrals-small-4-consolidates-reasoning-vision-and-coding-at) · [news.saerio — Small 4](https://news.saerio.com/mistrals-small-4-consolidates-reasoning-vision-and-coding-into-one-model-at-a-fraction-of-the-inference-cost/) · [mysocial81/Medium — Small 4](https://medium.com/@mysocial81/how-mistral-small-4-unifies-reasoning-vision-and-agentic-coding-776d0cb876f3) · [aizolo — Mistral models 2026](https://aizolo.com/blog/mistral-ai-models-2026/) · [redhat-et — Mistral intel](https://github.com/redhat-et/physical-ai-platform-intel/blob/HEAD/deliverables/intel/companies/mistral-ai.md) · [redhat-et — Mistral deep-dive](https://github.com/redhat-et/physical-ai-platform-intel/blob/HEAD/deliverables/intel/companies/mistral-ai-deep-dive.md) · [deeplearning.ai — Ministral 3](https://www.deeplearning.ai/the-batch/mistral-3-update-adds-four-new-open-models) · [varindia — Magistral](https://www.varindia.com/news/mistral-takes-on-openai-deepseek-with-new-magistral-models) · [cenchs — Magistral](https://cenchs.com/mistral-releases-a-pair-of-ai-reasoning-models/) · [aibase — Magistral](https://www.aibase.com/news/18801)
**NVIDIA Nemotron:** [memeburn — Nemotron 3 Ultra](https://memeburn.com/nvidia-nemotron-3-ultra-americas-best-open-ai-model-2026/) · [aiweekly — Nemotron 3](https://aiweekly.co/alerts/nvidia-nemotron-3-launches-500b-param-open-ultra-model) · [awesomeagents.ai — Nemotron 3 Ultra](https://awesomeagents.ai/models/nvidia-nemotron-3-ultra-550b/) · [shadowfetch — Nano](https://shadowfetch.com) · [smfworks journal](https://github.com/smfworks/smfworks-site/blob/HEAD/content/jeffs-journal/nvidia-nemotron-3-ultra-open-550b-agent-model-million-token-work.md) · [dockerbuildfiles — Nemotron catalog](https://github.com/99sono/dockerbuildfiles/blob/HEAD/inference-containers/ai-labs/nvidia-nemotron.md)
**Microsoft Phi:** [chronohorn small-models-2026-survey](https://github.com/asuramaya/chronohorn/blob/HEAD/paper/small-models-2026-survey.md) · [tt-model-bringup — home LLM landscape 2026](https://github.com/aweditya/tt-model-bringup/blob/HEAD/research/archive/home_llm_landscape_2026.md) · [aiclearinghouse — Build 2026](https://github.com/smfworks/aiclearinghouse-site/blob/HEAD/content/blog/build-2026-day-one-everything-microsoft-announced.md) · [Wikipedia — Phi (LLM)](https://en.wikipedia.org/wiki/Phi_(LLM)) · [Microsoft Tech Community — Phi-4](https://techcommunity.microsoft.com/blog/educatordeveloperblog/phi-4-small-language-models-that-pack-a-punch/4464167)
**Google Gemma:** [tpsreport.news — Gemma 4](https://tpsreport.news/news/google-gemma-4-31b-mtp-assistant-release) · [happycapyguide — Gemma 4](https://happycapyguide.com/blog/google-gemma-4-apache-open-weights-agentic-ai-2026) · [techglimmer.io — Gemma 4](https://techglimmer.io/google-gemma-4-open-source-ai-models-2026/) · [medium/@shubhamnv2 — Gemma 4](https://medium.com/@shubhamnv2/google-gemma-4-the-open-source-ai-model-that-just-ranked-3-in-the-world-and-runs-on-your-phone-a8f160e5cc83) · [wavespeed.ai — Gemma 4](https://wavespeed.ai/blog/posts/what-is-google-gemma-4/)
**Others:** [The New Stack — OLMo 3](https://thenewstack.io/ai2s-olmo-3-pushes-the-envelope-for-open-source-llm-performance/) · [MarkTechPost — OLMo 3](https://www.marktechpost.com/2025/11/20/allen-institute-for-ai-ai2-introduces-olmo-3-an-open-source-7b-and-32b-llm-family-built-on-the-dolma-3-and-dolci-stack/) · [medium/@leucopsis — OLMo 3](https://medium.com/@leucopsis/allenais-olmo-3-32b-think-the-best-fully-open-source-reasoning-model-b1d04a6a0010) · [dataconomy — OLMo 3](https://dataconomy.com/ai-models/olmo-3-7b-think/) · [SiliconANGLE — Granite 4.0 Nano](https://siliconangle.com/2025/10/29/ibm-releases-small-open-source-granite-4-models-mobile-devices-browsers/) · [VentureBeat — Granite 4.0 Nano](https://venturebeat.com/ai/ibms-open-source-granite-4-0-nano-ai-models-are-small-enough-to-run-locally) · [MarkTechPost — Granite Speech](https://www.marktechpost.com/2026/03/15/ibm-ai-releases-granite-4-0-1b-speech-as-a-compact-multilingual-speech-model-for-edge-ai-and-translation-pipelines/) · [HF ibm-granite/granite-4.0-3b-vision](https://huggingface.co/ibm-granite/granite-4.0-3b-vision) · [c-sharpcorner — Granite 4.0](https://www.c-sharpcorner.com/news/ibm-unveils-granite-40-hyperefficient-hybrid-ai-models-for-enterprise) · [udit.co — Hermes 4](https://udit.co/blog/nous-research-hermes-4-open-source-outperforms-chatgpt) · [arXiv 2508.18255 — Hermes 4](https://arxiv.org/pdf/2508.18255) · [ml-debug — Hermes 4 report](https://github.com/wassname/ml-debug/blob/HEAD/docs/evidence/reports/hermes4_technical_report.md)

---

*End of STEP 1 — Western track fiche. Individual research parts preserved at `~/workspace/rag_collect/etape1_ouest_part1_llama.md`, `etape1_ouest_part2_muse_poolside.md`; the third agent's source fiche (Mistral/Nemotron/Phi/Gemma/others) is fully incorporated above.*
# STEP 1 — Open / Local AI Models: Local Inference Practical Track (EN)
**Research date: 2026-09-22 — for RAG ingestion**
*All figures are third-party/community-reported unless marked vendor-reported or official. GPU rental prices move fast; treat as directional.*

---

# 1. LOCAL INFERENCE ENGINES — 2026 STATE

## 1.1 Ollama

**What it is:** MIT-licensed local LLM daemon (REST API on port 11434, model management, minimal terminal chat). The default local backend for the ecosystem — powers Open WebUI, Continue.dev, AnythingLLM, Cline/Roo Code local setups, and most IDE/agent integrations. Model library at ollama.com/library has 100+ models; `ollama pull` downloads the recommended quantization for your hardware automatically (no manual Q4_K_M vs Q5_K_S choice).

**2026 developments:**
- **v0.19 (preview, March 30, 2026): MLX backend for Apple Silicon.** Rebuilt the Mac inference stack on Apple's MLX framework. Official Ollama benchmark on M5 Max running Qwen3.5–35B-A3B (NVFP4): prefill 1,154 → 1,810 tok/s (+57%), decode 58 → 112 tok/s (+93%); int4 pushes to 1,851 prefill / 134 decode tok/s. Community reports 2–3× faster decode vs the old Metal-via-llama.cpp path, TTFT roughly halved. llama.cpp remains the backend on Linux/Windows.
- **v0.24.0 (May 2026):** latest stable cited in 2026 guides; MLX backend now standard on Apple Silicon.
- **OpenAI-compatible `/v1/chat/completions` endpoint** — any OpenAI-API-speaking tool works unmodified. Also added **Anthropic Messages API compatibility**, so Claude-format tools work too.
- **GitHub Copilot integration (March 2026):** every code suggestion, chat prompt, and agentic workflow can route to local models on localhost:11434 — no API keys, no telemetry, no per-token charges. Structural shift for regulated industries (code never leaves the network perimeter).
- Time to first inference: ~5 min macOS/Linux, ~8 min Windows.

**Positioning 2026:** the API-first headless local runner — best for terminal users, daemons, REST APIs, Docker workflows, repeatable model pulls, local agents/automation. "If you want a headless local model server for scripts, agents, Docker, CI jobs, or production-like APIs, choose Ollama."

Sources: https://medium.com/@tentenco/ollama-0-19-ships-mlx-backend-for-apple-silicon-local-ai-inference-gets-a-real-speed-bump-878b4928f680 · https://aifoss.dev/blog/llamafile-vs-ollama-vs-lm-studio-2026/ · https://medium.com/@_lukasz_/local-coding-models-in-2026-why-mlx-beat-ollama-on-apple-silicon-68118d806c40 · https://pooyagolchian.com/blog/github-copilot-ollama-agentic-local-llm-2026/ · https://localclaw.io/blog/ollama-vs-lm-studio-2026

## 1.2 llama.cpp

**What it is:** the C/C++ inference engine under the whole local-LLM world (Ollama, LM Studio's llama engine, and dozens of wrappers). GGML library parses GGUF weights and builds the compute graph. Supports Apple Silicon, x86, RISC-V, NVIDIA/AMD GPUs.

**2026 developments:**
- **Versioning:** builds b7931 → b10369 (Aug 2026); major version bump to **llama.cpp 0.2.0** with enforced semantic versioning (Aug 2026).
- **Backends:** CUDA, Metal (consolidated binary kernels, Feb 2026), **Vulkan** (full cross-vendor GPU path), **ROCm 6.4.4** (AMD — Qwen3-235B-A22B hit 1,132 tok/s on Radeon 8060S via ROCm, 3.47× over ROCm 7.0.1 in one reported test), SYCL, OpenCL, WebGPU, Hexagon.
- **Quantization:** 1.5-bit to 8-bit; **GGUF** is the standard checkpoint format; **IQ2/IQ3** quants with importance-matrix (`--imatrix`) calibration for quality retention at extreme compression.
- **MCP integration (finalized early 2026, PR #19546):** llama.cpp became an MCP host — local agents with tool use can run fully offline. Elevates it from inference engine to modular local AI platform.
- **Multimodal:** vision support via libmtmd in llama-server; `--mmproj-device` places the vision projector on a specific GPU.
- **Speculative decoding:** MTP (multi-token prediction) draft-model auto-discovery via `--models-dir`; DSpark speculative decoder accepts SpecForge-exported drafts.
- **Native pocket-TTS support (b10369, Aug 2026):** SEANet decoder rewrite (matmul + col2im), −80% per-frame generation time on CUDA, output correlation 0.999994 vs prior — sample-identical speedup.
- **Architecture support cadence:** rapid 2026 additions — Kimi-K3 (hybrid KDA linear + MLA attention, 1024-expert latent MoE), ByteDance BailingMoE3, IBM Granite SWA/MoE-SWA, Qwen3.5 dense + MoE, Step3.5-Flash.

**Key flags for VRAM control:** `--n-gpu-layers` (fine-grained GPU offload), `--no-mmap`, `--flash-attn`.

**Positioning 2026:** maximum flexibility — Apple Silicon native, embedded/edge, one-off CLI experiments, and the engine inside Ollama/LM Studio. Best single-stream latency (one benchmark: llama.cpp ROCm + MTP 39 tok/s single-stream vs vLLM 19.5). Not for multi-user production.

Sources: https://github.com/licjon/cl-llama-cpp/blob/HEAD/docs/upstream-digest.md · https://dasroot.net/posts/2026/01/running-local-llms-python-ollama-llama-cpp-transformers/ · https://aihaberleri.org/en/news/llamacpp-integrates-mcp-protocol-to-expand-local-ai-capabilities · https://dasroot.net/posts/2026/01/local-llm-deployment-ollama-llama-cpp/

## 1.3 LM Studio

**What it is:** desktop app (Mac/Windows/Linux) — the easiest GUI for discovering, downloading (built-in Hugging Face model browser), configuring, and chatting with GGUF models. One-click OpenAI-compatible local server (port 1234). Hardware auto-detection (CUDA/MPS), visible controls for context, GPU offload, sampling.

**2026 developments:**
- **LM Studio Bionic (launched July 16, 2026):** separate agentic app for Mac/Windows — an AI agent that accesses local files and autonomously does coding, document creation, research. Built-in open-model library; falls back to **LM Studio Secure Cloud** (US servers, Zero Data Retention) for heavy tasks — billed LM Studio account. Bionic added Kimi K3, then **GLM-5.3-Flash** (Aug 26, 2026) as cloud models. Ships a voice keyboard with local transcription (Mistral Voxtral).
- **MTP speculative decoding stable in v0.4.14 (May 22, 2026)** — materially faster inference on supported models; current stable **v0.4.17**.
- **MCP host since v0.3.17** (June 2025); v0.4.10 (April 2026) added OAuth 2.1 browser auth; ephemeral per-request MCPs (SSRF-guarded); fine-grained `allowed_tools` gating.
- **Locally acquisition (April 8, 2026):** extended the device story to iPhone/iPad — shipped as the Locally mobile app + **LM Link**, an end-to-end-encrypted bridge letting a phone drive large models on the desktop (v0.4.16, June 2026).
- **mlx-engine v1.8.5 (June 2026):** disk-backed KV-cache reuse + continuous batching for serious local agentic workloads.
- **Model Search by hardware:** tells you which models fit your Mac's specs.

**Positioning 2026:** "LM Studio is an app" (easiest desktop experience, HF GGUF browsing, non-technical users) vs "Ollama is a runtime" (headless, scriptable, Docker/CI). Note: Ollama now also offers optional cloud models — "using Ollama" no longer automatically means every request is local.

Sources: https://9to5mac.com/2026/07/16/lm-studio-expands-beyond-chat-with-bionic-a-new-ai-agent-app-for-open-models/ · https://9to5mac.com/2026/08/26/lm-studio-adds-glm-5-3-flash-to-bionic-with-image-support-and-1m-token-context/ · https://media.patentllm.org/news/local-ai/lm-studio-adds-mtp-speculative-decoding-qwen-3-6-gguf-quants-20260520 · https://localclaw.io/blog/ollama-vs-lm-studio-2026

---

# 2. HARDWARE REQUIREMENTS FOR POPULAR OPEN MODELS

## 2.1 The VRAM math

```
VRAM ≈ Parameters (B) × Bytes per parameter + KV-cache + runtime overhead
```

| Quantization | Bytes/param | 7B | 13B | 32B/34B | 70B | Quality vs FP16* |
|---|---|---|---|---|---|---|
| FP16/BF16 | 2.00 | 14 GB | 26 GB | 64–68 GB | 140 GB | baseline |
| Q8_0 (8-bit) | ~1.06 | 7.5 GB | ~14 GB | 34–36 GB | 74 GB | ~99% (near-identical) |
| Q6_K | ~0.81 | 5.7 GB | — | — | 57 GB | ~97% (excellent) |
| Q5_K_M | ~0.69 | 4.8 GB | — | — | 48 GB | ~95% (good) |
| **Q4_K_M** | **~0.56** | **3.9 GB** | **~8 GB** | **18–20 GB** | **39 GB** | **~90% (acceptable)** |
| Q3_K_M | ~0.44 | 3.1 GB | — | — | 31 GB | ~80% (noticeable loss) |
| Q2_K | ~0.31 | 2.2 GB | — | — | 22 GB | ~70% (significant loss) |
| FP8 (E4M3) | 1.0 | 7 GB | — | — | 70 GB | ~100% (near-lossless) |

*\*Approximate, task-dependent. Chat is forgiving; coding/reasoning less so. Community rule: "Before dropping to Q3 to fit a model, ask: would a smaller model at Q5 give better results? The answer is usually yes."*

**KV cache — the underestimated factor:** grows linearly with context and batch. For a 70B GQA model: ~10 GB extra at 32K context (FP16 KV); ~24.6 GB at 75K tokens. Doubling context doubles KV cache. FP8 KV cache is the cheapest 2× concurrency lever.

**CPU offload fallback:** if weights exceed VRAM, layers spill to system RAM — works but 5–20× slower for offloaded layers. Partial offload (10–20%) is often acceptable.

Sources: https://willitrunai.com/blog/vram-requirements-for-ai-models · https://willitrunai.com/blog/quantization-q4-q8-fp16-explained · https://github.com/casteldazur/awesome-local-ai/blob/HEAD/guides/vram-requirements.md · https://www.alekseialeinikov.com/en/blog/topics/ai/quantization-explained-run-70b-model-consumer-hardware-2026 · https://github.com/drajb/gekro/blob/HEAD/apps/web/src/content/apps/gpu-vram-calculator.md

## 2.2 What fits where (2026 hardware classes)

### Consumer GPUs (24 GB — RTX 4090 class)
- Comfortable: up to **~32–34B at Q4_K_M** (18–20 GB).
- 70B at Q4_K_M (39 GB) needs significant CPU offload on 24 GB — usable but slow.
- 8B models at Q4 (~5 GB): ~95 tok/s reported on RTX 4090-class (llama.cpp eval).
- Note: during decode, **memory bandwidth** (not just capacity) dominates speed once the model fits.

### Prosumer single-GPU (32–48 GB — RTX 5090 32 GB, RTX 4090 Pro 48 GB)
- RTX 5090 (32 GB): 70B Q4 is tight; 27B-class models (e.g., Qwen3.8-27B) run comfortably; community verdict — "on today's best models a 5090 nearly matches the Pro 6000 for a fraction of the price" for dense models up to ~32B.
- RTX 4090 Pro (48 GB): 70B Q4_K_M (39 GB) fits with headroom; 70B Q5_K_M (48 GB) tight.

### Workstation flagship (96 GB — RTX PRO 6000 Blackwell, ~$10,000–11,000)
- **Llama 3.3 70B at Q8 (~74 GB):** fits with context headroom; community reports **35–50 tok/s** sustained.
- **70B at BF16:** full-quality, no quantization needed.
- 120B-class at FP8, 180B+ at INT4/FP4 on a single card.
- Qwen3.5-122B-A10B MoE (10B active) fits 96 GB.
- 2× cards (192 GB) reach larger frontier MoEs; 4× (384 GB) covers virtually every open-weight model released to date.
- Specs: 24,064 CUDA cores, 96 GB GDDR7 ECC, 1.79 TB/s bandwidth, 600W.

### Unified-memory desktops (DGX Spark / Strix Halo / Mac)
- **NVIDIA DGX Spark (GB10, 128 GB unified LPDDR5x, ~$4,699):** runs 128B-class models (e.g., Mistral Medium 128B) locally for far less than a Pro 6000 build.
- **AMD Strix Halo (128 GB, ~$2,799 desktop):** same class — 128B local inference at consumer-ish pricing.
- **Apple Silicon:**
  - M4 Max 128 GB: 70B Q4_K_M at ~10–15 tok/s; unified pool shared CPU/GPU, no PCIe copy.
  - M5 Max 128 GB (up to 614 GB/s): 70B Q4 comfortable; Llama 3.3 70B Q6 (~55 GB) leaves room for context + OS.
  - M5 Ultra up to **512 GB** (top config, ships late Oct 2026, expected >$10,000): first single Mac that could theoretically hold GLM-5.3 (744B MoE, ~239 GB at 2-bit) in unified memory — unbenchmarked as of Sept 2026.
  - Mac Studio M5 Max from $2,499 (128 GB); M5 Ultra from $5,499 (96 GB base).
  - **MLX is the 2026 unlock on Mac:** Ollama 0.19+ / LM Studio mlx-engine give 2–3× decode vs llama.cpp-Metal; Qwen3.6-35B-A3B MoE at 60–70+ tok/s on M5 Max-class hardware.

### Multi-GPU / node-scale (the 400B+ MoE tier)
Critical MoE fact: **only ~8–42B params are active per token, but ALL weights must be resident** — the router can select any expert, so nothing can be left off-card.

| Model | Checkpoint | VRAM floor | Practical config |
|---|---|---|---|
| **DeepSeek V4.1 Flash** (552B MoE; community analysis argues 748B incl. 196B Engram tables — disputed) | ~510 GB (FP8 weights + FP4 routed experts, 48 shards) | **~614 GB** (vLLM recipe) | 8× H200 (1,128 GB) recommended; GB200 NVL4 tray validated; 8× H100 80 GB (640 GB) fits with ~130 GB headroom — tight for production |
| **DeepSeek V4 Flash** (older, 284B) | 158 GB native (FP4+FP8) | ~170–175 GB | 2× RTX PRO 6000 (192 GB) min for native quality; Q4_K_M (~86–96 GB) fits a single 96 GB card — tight, 45–60 tok/s, ~5% quality degradation vs native |
| **GLM-5.3 Flash** (320B / 18B active) | ~306 GiB | 8-GPU node class | more headroom on 8× H100 than V4.1 Flash |
| **GLM-5.1** (744B / 40B active) | FP8 ≈ **800 GB** | — | 8× H200/H20 node |
| **GLM-5.3** (744B MoE) | ~239 GB at 2-bit | — | 512 GB Mac Studio M5 Ultra (theoretical); 4× RTX 3090/4090 documented community path |

- DeepSeek V4.1 Flash KV cache: ~890 bytes/token (FP4 cache) — 1M context ≈ 890 MB... note: at 1M tokens the cache is material (~0.9 GB) but was never the binding constraint; weights are.
- No GGUF/Ollama desktop conversion existed for V4.1 Flash at research time (checkpoint already ships FP8/FP4; little room left for further quantization).
- Engines: vLLM ≥ 0.30, SGLang day-zero build, NVIDIA Dynamo — Hopper/Blackwell and AMD MI350X/MI355X.

Sources: https://www.yottalabs.ai/post/deepseek-v4-1-flash-hardware-requirements-gpu-memory-2026 · https://github.com/dbirks/home-k8s/blob/HEAD/deepseek-v4-flash-hosting-notes.md · https://www.orcarouter.ai/blog/deepseek-v4-1-flash-local-deployment · https://www.orcarouter.ai/blog/deepseek-v4-1-flash-inference-stack · https://www.yottalabs.ai/post/deepseek-v4-1-flash-pricing-specs-v4-pro-routing-2026 · https://the-decoder.com/new-deepseek-model-v4-1-flash-cuts-memory-needs-for-ai-agents/ · https://www.promptquorum.com/local-llms/local-llm-hardware-guide-2026 · https://awesomeagents.ai/news/apple-m5-pro-max-70b-models-portable/ · https://www.newegg.com/insider/nvidia-rtx-pro-6000-blackwell-workstation-96gb-gddr7-for-serious-local-ai/ · https://mwgamers.com/blog/nvidia-rtx-pro-6000-monolith-future-gaming/

---

# 3. QUANTIZATION LANDSCAPE 2026

## 3.1 Format cheat sheet (practical bytes/param and quality story)

| Format | Practical bytes/param | Quality story | Ecosystem |
|---|---|---|---|
| BF16 | 2.0 | training + high-quality baseline | everything |
| FP8 (E4M3) | 1.0 | often ~identical to BF16 (~0.0 ppl drop) | server serving default (vLLM/SGLang) |
| **NVFP4** | ~0.56 | close to FP8 in NVIDIA examples; ~0.3–0.5 ppl drop; competitive with best INT4 | **Blackwell-native**; 16-elem blocks, UE4M3 scales |
| MXFP4 | ~0.53 | useful 4-bit microscaling; ~0.5–1.0 ppl drop; slightly behind NVFP4 | OCP MX standard; 32-elem blocks |
| **GGUF Q4_K_M** | ~0.56–0.65 | strong local-chat default | llama.cpp / Ollama / LM Studio |
| INT4 GPTQ/AWQ | ~0.5 + metadata | calibration-dependent; ~0.3–1.5 ppl drop | legacy/common 4-bit path — "only if that is all you have" |
| GGUF Q8_0 | ~1.06 | ~99% of FP16 | near-lossless local |
| GGUF IQ3/IQ2 (+imatrix) | ~0.35 / ~0.22 | quality drops fast below 3-bit | extreme compression |

*Rule of thumb from the 2026 doctrine: low-batch/single-user → weight-only INT4-class; high-batch serving → W8A8 FP8 (INT4 dequant makes it slower at high batch); KV pressure → FP8 KV cache first (cheapest 2× concurrency).*

## 3.2 What the community actually uses

- **Desktop/local chat:** GGUF **Q4_K_M** remains the default sweet spot — Ollama auto-selects it by VRAM. Q8_0 when VRAM allows. IQ4_XS approaches Q4_K_M quality at slightly less VRAM.
- **Server self-hosting:** **FP8** for <400B models (near-lossless, mature kernels); **NVFP4** where the artifact ships that way (DeepSeek V4.1 Flash ships FP8+FP4 natively; SGLang shipped day-zero GLM-5.2 support including NVIDIA's NVFP4 checkpoint for Blackwell).
- **AWQ/GPTQ:** declining — kept for older model checkpoints that only exist in those formats.

## 3.3 The Blackwell FP4 caveat (important for 2026 buyers)

- NVFP4 dequantization is **free on Blackwell Tensor Cores** (native format); INT4 schemes need a separate dequant kernel that costs throughput.
- **But:** as of 2026, **no off-the-shelf stack delivers NVFP4 weights + NVFP4 KV cache on consumer Blackwell (SM120/RTX 5090/PRO 6000/DGX Spark):** vLLM's NVFP4 path falls back to Marlin (40–50% perf loss); SGLang's attention backends fail for MoE on SM120; TensorRT-LLM lacks NVFP4 KV-cache support. Only custom CUTLASS kernels close the gap. SM100 (datacenter B200) and SM120 (consumer Blackwell) need different kernels — code compiled for `sm_100a` traps on consumer Blackwell.
- Independent test (QuTLASS/MR-GPTQ MXFP4 on RTX 5090): genuine **~4× GEMM speedup** at batch ≥128, but **end-to-end decode lost to BF16 outright** at batch 1–32 (20.4 vs 78.6 tok/s at batch 1) — FP4 wins at GEMM level, loses at memory-bound decode on consumer cards.

**Bottom line for RAG:** recommend NVFP4 artifacts on datacenter Blackwell/Hopper; on consumer Blackwell and Apple Silicon, GGUF Q4_K_M (llama.cpp/Ollama) or MLX 4-bit remain the practical defaults. The FP4 software story on consumer Blackwell is unfinished.

Sources: https://github.com/chsasank/blackwell · https://github.com/0xsero/blackwell-gpu-wiki/blob/HEAD/docs/blackwell/nvfp4-deep-dive.md · https://github.com/kubesimplify/website/blob/HEAD/content/blog/day-4-quantization-demystified-bf16-fp8-nvfp4-mxfp4-int4-gguf-and-why-it-all-matters.md · https://github.com/codehalwell/fable-skills/blob/HEAD/skills/llm-inference-optimization/SKILL.md · https://github.com/notwitcheer/llm-bench-rig/blob/HEAD/reports/fp4-consumer-blackwell.md

---

# 4. vLLM vs SGLang FOR SELF-HOSTING — 2026 RECAP

## 4.1 The two-contender market

- **TGI is out:** Hugging Face's Text Generation Inference accepts bug fixes only since Dec 2025 — no new features. HF Inference Endpoints now default to **vLLM**, with SGLang as the alternative. Two real contenders remain.

| Dimension | vLLM | SGLang |
|---|---|---|
| Origin / core trick | UC Berkeley — **PagedAttention** | LMSYS — **RadixAttention** (prefix tree) |
| Best for | Production multi-tenant serving; high-concurrency API; predictable latency | Structured-output workloads; multi-turn agents; shared-prefix workloads |
| 2026 benchmarks (H100) | Llama 3.3 70B FP8: 120→2,400 tok/s (conc. 1→100); Llama 3.1 8B: ~12,500 tok/s | Llama 3.3 70B FP8: 125→2,460 tok/s; Llama 3.1 8B: **~16,200 tok/s (+29%)** |
| Structured output | Noticeable overhead at high batch | **4.7× multi-turn speedup**, 99.8% JSON validity, overlapped mask gen |
| Memory (agent workloads) | baseline | **~47% less** (40 GB vs 75 GB) |
| Single-user structured | baseline | **6.6× faster** than vLLM |
| Hardware | NVIDIA, AMD, Intel, AWS Trainium, TPU | NVIDIA, AMD (400K+ GPUs deployed) |
| Day-0 model support 2026 | NVIDIA Nemotron 3 Ultra (Jun 2026); GLM-5.2 recipe targets vLLM 0.23.0 (stable) | **DeepSeek-V4 infer+RL (Apr 2026)**; GLM-5.2 (incl. NVFP4 Blackwell ckpt); Kimi-K2.7-Code; MiMo; Nemotron-H; LFM2.5 |
| Maturity | Very high; mature docs, Helm charts | High; Docker-first |

**2026 decision rule (community consensus):** vLLM for high-concurrency API serving where predictable latency matters; SGLang for agent loops, multi-turn conversations, and structured generation where prefix caching pays. "Ollama vs vLLM is easy; vLLM vs SGLang depends on your workload" — vLLM beats Ollama 16–29× in aggregate throughput at concurrency >10 (Ollama times out at ~20 concurrent).

## 4.2 VRAM examples in production serving

- **GLM-5.1 FP8 ≈ 800 GB** → 8× H200/H20 node (vLLM or SGLang, tensor-parallel).
- **DeepSeek V4.1 Flash:** 614 GB floor → 8× H200 (1,128 GB) or GB200 NVL4; vLLM ≥ 0.30 / SGLang day-zero / NVIDIA Dynamo.
- **Llama 3.3 70B FP8 on H100:** single GPU serves 100 concurrent at ~2,400–2,460 tok/s either engine.

Sources: https://techsy.io/en/blog/vllm-vs-sglang · https://privocto.com/blog/vllm-sglang · https://lushbinary.com/blog/glm-5-2-self-hosting-open-weights-vllm-guide/ · https://github.com/chipi/agentic-ai-homelab/blob/HEAD/docs/reading/self-hosting-llms.md · https://github.com/adilshamim8/genai-roadmap-with-notes-and-projects/blob/HEAD/fine-tuning-and-self-hosting/03-vllm-sglang-tgi.md

---

# 5. COST COMPARISON: SELF-HOSTING vs API

## 5.1 The three-way break-even (2026)

Self-hosting beats a **frontier API** (GPT-5-class, Claude Sonnet 5/Opus, Gemini Pro) at roughly **5–8M tokens/day (~160–256M/month)** at 60–70% GPU utilization. Against a **budget open-weight API** ($0.14–$0.50/M tokens), it **rarely wins on cost at all**. "Self-hosting beats an expensive model long before it beats a cheap one."

| What you'd rent | $/month (Runpod Community Cloud, 720h) | vs API | Break-even volume |
|---|---|---|---|
| RTX 4090 24 GB | $244.80 | Together Llama 3 8B Lite ($0.14/M) | **1.75B tokens/month** (675 tok/s sustained) |
| RTX 4090 24 GB | $244.80 | gpt-4o-mini ($0.375/M) | 653M tokens/month |
| A100 80 GB PCIe | $856.80 | gpt-5.6-terra ($7.00/M blended) | 122M tokens/month |
| H100 80 GB PCIe | $1,432.80 | gpt-6-astra ($30/M blended) | **48M tokens/month** |

*(Rates read Sept 4, 2026; blended = even input/output split.)*

**DeepSeek V4 worked example:** 8× H100 node ≈ **$18,000/month** rented. vs V4-Flash API ($0.28/M output): break-even at **~65B output tokens/month** — a card kept near full load continuously. At 50M tokens/month the API costs ~$14 vs $18,000 self-hosted. "Stay on the API until you are spending well above $15,000–$20,000/month, then evaluate self-hosting."

**European bare-metal (Sept 2026):** from ~€234/mo (20 GB Hetzner GPU server, ≤14B models) → €1,199/mo (96 GB GEX131, 27–70B class) → $2,100–3,100/mo (rented H100). 27–70B self-host breaks even vs Claude Sonnet 5 ($3/$15) at ~10–15M tokens/day; vs Haiku 4.5 ($1/$5) at ~25–45M/day. Add 10–20% of a senior engineer for ops.

**Real deployment case study:** €38,500 hardware + €1,030/mo ops vs €10,900/mo equivalent frontier API → **break-even 3.9 months**, 81% cheaper over 3 years. Below ~€2,000/mo API spend, self-hosting rarely pays on cost alone.

## 5.2 Self-hosted per-token costs (mid-2026)

| Option | Input $/M | Output $/M | Notes |
|---|---|---|---|
| Open ~70B via Groq/Together (managed) | ~$0.60 | ~$0.80 | managed, fast |
| Open ~70B self-hosted (A100) | ~$0.20–0.40 | ~$0.60–1.20 | needs DevOps |
| Open ~8B self-hosted (L4) | ~$0.03–0.05 | ~$0.10–0.20 | high-volume simple tasks |
| vLLM self-hosted (general) | $0.50–1.00 blended | | per privocto |

**API reference (open models, Sept 2026):** DeepSeek V4.1 Flash $0.15/$0.60 off-peak ($0.30/$1.20 peak, $0.003 cache hits); GLM-5.3-Flash $0.15/$0.50; MiMo-V2.6-Pro $0.435/$0.87; MiMo-V2.6-Flash $0.14/$0.28; GLM-5.3 $4.40/M output; Kimi K3 $15/M output; Qwen3.8 Max $6/M output; MiniMax M3 $1.20/M output.

**Flat-rate disruptor — CheapestInference:** Core pool from **$15.29–17.99/mo** unlimited (DeepSeek V4.1 Flash + MiMo v2.5); Frontier pool **$60.35/mo** (GLM-5.3 + MiniMax M3); Flagship pool **$199/mo** per 8h daily block (Kimi K3 + Qwen3.8 Max). "No token caps during your reserved hours."

## 5.3 When local makes sense (decision framework)

**Self-host when:** sustained volume > break-even for your tier; GPU utilization >50–60%; data residency/compliance requires it; you need latency control (local TTFT 20–80ms vs 200–500ms cloud); you need any fine-tune/quantization/engine control; flat predictable cost beats per-token.

**Stay on API when:** volume < ~€2,000/mo spend equivalent; spiky/bursty traffic (can't scale to zero locally); no ops capacity (the real cost is the serving stack + on-call, not the GPUs); you need frontier quality (expect ~1pp quality give-up vs frontier on open models).

**Hidden costs of self-hosting:** idle GPU cost, model updates/drift, monitoring, on-call engineering, networking/storage, power (~$3.50/day for a 400W card), hardware amortization (~$20.55/day for a $25K A100 server over 3 years — "the anchor most 'local is cheaper' calculations ignore").

Sources: http://cloudzy.com/blog/self-hosting-open-weight-llm-gpu-vps-cost/ · https://aireviewrating.com/articles/self-hosting-llm-break-even · https://renezander.com/guides/self-hosted-llm-vs-api/ · https://ecorpit.com/deepseek-v4-self-hosted-vs-api-gpu-cost-break-even-2026/ · https://vallettasoftware.com/blog/post/self-hosted-llm-cost · https://promptcost.org/en/blog/local-llms-total-cost-ownership-2026/ · https://cheapestinference.com/ · https://cheapestinference.com/pools/core/ · https://cheapestinference.com/faq/ · https://cheapestinference.com/reports/state-of-open-weights/

---

# 6. OPENROUTER DATA — OPEN MODELS DOMINATE 2026

## 6.1 Platform scale

- **~9,000× weekly token growth since Jan 2024:** from ~10B/week to **90T+/week by mid-2026** (25T in May 2026 → 55T+ Aug 2026; ~100T/month). 400+ LLMs, **8M+ users**.
- **Business:** ~$140M annualized revenue (Jul 2026), 29% MoM growth; **$113M Series B (May 2026)** at ~$1.3B valuation (CapitalG-led).
- **Menlo Ventures:** 2.5M → 8M+ developers in ~1 year; ~**1.5 quadrillion tokens/year** run rate (May 2026).

## 6.2 Agents eat the platform

- Agentic workloads: **14× growth Feb–Aug 2026** (0.51T → 7.3T tokens/day); **~71% of all consumption** by early Aug 2026 (human usage only 2.8× in the same window).
- Agents consume **~5× more tokens per task** than humans (a16z/Carta analysis; one analysis cites ~15×); **85%+ of agentic burn is cached prompts**.
- Coding/agent task share of platform usage: 11% → **>50%** since early 2025.

## 6.3 The Chinese open-model flip

| Metric | 2025 | Mid-2026 |
|---|---|---|
| Chinese open models share | ~2% | **>50%** (Bloomberg: 48% vs US 20% late June 2026 — 18T vs 5.5T tokens/week) |
| US frontier models share | ~70% | **~30%** |
| Crossover | — | **Feb 9–15, 2026** (one analysis dates the overtake to early June 2026 — snapshot/methodology differences) |

- **DeepSeek: 16.3–18%** — the single largest provider on the platform (ahead of Anthropic at 13.3%, down from 29.1%); DeepSeek rose 9% → 18% Jan–Jun 2026; V4 Flash became 70% of DeepSeek's agentic flow by late May.
- **Xiaomi MiMo: ~21%** of routed tokens in one mid-2026 analysis (22% of coding traffic); MiniMax M2.5 ~4.55T tokens/month; Kimi K2.6 ~4.02T/month; six of the top-10 models by usage were Chinese open-source (June 2026).
- Open-model token totals (one study): DeepSeek 14.37T, Qwen 5.59T, Meta Llama 3.96T, Mistral 2.92T, MiniMax 1.26T, Z.ai 1.18T, Moonshot 0.92T.
- **Drivers:** Chinese API prices 10–20× cheaper than US models (DeepSeek V4 Flash up to 150× cheaper than GPT-5.5 on output tokens); agent workloads moved to cheapest capable endpoints; a16z: **~80% of startups on open-source stacks run Chinese models**.
- OpenRouter doubles as a **stealth-launch venue** (GLM-5.3-Flash topped charts anonymously as "Ox Alpha" mid-Aug 2026 before attribution) and the industry's adoption benchmark of record.

Sources: https://cryptobriefing.com/openrouter-9000x-token-usage-growth/ · https://tech-insider.org/au/chinese-ai-models-openrouter-2026/ · http://groundtruth.day/news/chinese-open-models-passed-us-models-in-openrouter-token-share.html · https://www.ajupress.com/view/20260707151270574 · https://officechai.com/ai/share-of-us-models-being-used-on-openrouter-has-collapsed-from-70-to-30-over-the-past-year/ · https://cheapestinference.com/blog/openrouter-alternative/

---

# 7. METHODOLOGY NOTES & GAPS

- **Snapshot sensitivity:** OpenRouter share figures differ across analyses (crossover dated Feb vs June 2026; Chinese share 48% vs >50% vs 61%) — different weeks, different token-counting methods (input vs output, cached vs uncached). Record the source date with every figure.
- **Benchmark versions:** Artificial Analysis moved its Intelligence Index to v4.3.2 in Sept 2026 — every score fell 12–19 points at unchanged prices (methodology change, not model regression). Never compare across index versions.
- **Vendor-reported vs independent:** per-model launch benchmarks are vendor-reported; Artificial Analysis / BenchLM provide the independent check and usually show smaller deltas.
- **GPU rental prices** (Runpod, Hetzner) move monthly — break-even math must be recomputed, not quoted as fixed.
- **Unverified:** 512 GB Mac Studio M5 Ultra real-world inference benchmarks (ships late Oct 2026); V4.1 Flash 4× H200 community configs (unbenchmarked); Stripe/OpenRouter acquisition narrative (ignore — unverified).
- **Verify later:** DeepSeek V4.1 Flash Artificial Analysis score (not scored at research time); MiMo-V2.6-Flash independent benchmarks.

---

*End of Step 1 fiche — Open / local AI models (local inference practical track).*
