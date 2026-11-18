# VOLET 1 — Vague 1 (EN)
## Frontier AI Models — February → September 2026
*Consolidated research fiche for RAG ingestion. Compiled 2026-09-22.*
*All benchmark figures are vendor-reported unless marked as independently measured.*

---
# VOLET 1 — Wave 1 Research Fiche (EN)
## Frontier AI Models: February 2026 Release Wave
**Claude Sonnet 5 "Fennec" · GPT-5.3-Codex · Gemini 3.1 Pro**

---

## 1. Claude Sonnet 5 "Fennec" (Anthropic)

### Release & announcement
- **Official launch: February 3, 2026.** Anthropic released Claude Sonnet 5 under the internal codename "Fennec" (the fox known for speed/agility) with model identifier `claude-sonnet-5-20260203`. The launch was timed around Super Bowl week 2026 for maximum visibility.
- The community first spotted the model via a version string `claude-sonnet-5@20260203` leaked in Google Vertex AI error logs before the official announcement.
- **Historical note / discrepancy for the RAG:** a July 2026 retrospective (Medium/SSNTPL) claims the February checkpoint actually became Sonnet 4.6 and that the "genuine" Sonnet 5 shipped **June 30, 2026** with SWE-bench Pro 63.2% and OSWorld-Verified 81.2%. Most Feb–Apr 2026 sources (WaveSpeed, Vertu, AI First Founders) treat Feb 3, 2026 as the Sonnet 5 launch with 82.1% SWE-Bench Verified. Both narratives are documented; the Feb-2026 narrative is the primary one for this wave.

### Architecture & technical specs
- **Architecture:** not officially disclosed by Anthropic (transformer-based frontier LLM). Third-party analyses describe it as using "distilled reasoning" — compressing flagship-class intelligence into an efficient inference engine to solve the latency–intelligence paradox — plus "zero-latency background reasoning" (reasoning without visible thinking blocks).
- **Context window:** 1,000,000 tokens (5× larger than Opus 4.5; via API). Max output ~128,000 tokens.
- **Infrastructure:** first Anthropic model optimized for Google's "Antigravity" layer on TPUv6 architecture, co-developed with Google — speculative decoding (10–20 tokens in parallel), massive throughput (1M context at the latency Sonnet 3.5 needed for 200K), "warm" contextual persistence across days, and ~20–30% faster inference than prior generations.
- **Modalities:** text, images, PDF, code (reported).
- **Security-aware generation:** "Refusal with Explanation" logic — when it detects a security vulnerability being introduced (SQL injection, XSS, etc.), it identifies the vulnerability, explains the risk, and suggests a secure alternative, proceeding only on explicit user confirmation.

### Key benchmarks (reported values, Feb–Apr 2026 sources)
| Benchmark | Score |
|---|---|
| **SWE-Bench Verified** | **82.1%** — first model ever above 80% |
| SWE-Bench vs Opus 4.5 | 82.1% vs 80.9% (Opus 4.5) vs ~78% (GPT-5 est.) |
| OSWorld (computer use) | >85% (leaked); 81.2% (June-build reports) |
| Terminal-Bench 2.1 | 80.4% (June-build reports) |
| SWE-bench Pro | 63.2% (June-build reports) |
| GDPval-AA v2 | 1,618 (edges Opus 4.8's 1,615) |

### API pricing & availability
- **$3.00 / 1M input tokens, $15.00 / 1M output tokens** (~50–80% cheaper than Opus 4.5 depending on the report).
- Intro pricing on the June build: $2/$10 per 1M through Aug 31, 2026, then $3/$15.
- Included in Claude Pro ($20/month); default model on Free and Pro plans (June build).
- Channels: Anthropic API, Amazon Bedrock, Google Vertex AI, Microsoft Foundry, Claude Code.
- Model ID: `claude-sonnet-5-20260203` / `claude-sonnet-5@20260203`.

### Agentic / coding use cases
- **"Dev Team" mode** in the Claude Code CLI: Sonnet 5 acts as a multi-agent orchestrator — a Manager agent decomposes a high-level goal, spawns specialized sub-agents (Backend, QA, Infrastructure/Researcher), they work in parallel on different files (e.g., Backend writes an API route while QA generates unit tests), and the Manager reconciles conflicts before presenting the final PR.
- Self-correcting code execution with built-in terminal; full-feature builds from briefs autonomously; bug-report → write → test → verify patch loops on first try ("human parity" milestone for junior-to-mid-level dev work).
- Solo-founder/agency use: entire feature pipelines (architecture, implementation, debugging) built in AI-first loops.

### What distinguished it in 2026
- The **"Opus-killer"**: flagship-class coding (beating Opus 4.5) at mid-tier price — the best price/performance ratio in frontier models at launch, and the catalyst for the February 2026 agentic-coding arms race (OpenAI launched GPT-5.3-Codex minutes later).

---

## 2. GPT-5.3-Codex (OpenAI)

### Release & announcement
- **Released February 5, 2026** — GA of the code-specialized GPT-5.3 family. OpenAI described it as "the most capable agentic coding model to date," combining GPT-5.2-Codex's coding frontier with GPT-5.2's reasoning/professional knowledge.
- Launched **minutes after Anthropic's own agentic coding announcement** (both were planned for 10 a.m. PST; Anthropic moved theirs up 15 minutes) — the defining Feb 2026 "arms race" moment.
- **March 2026:** designated **LTS (Long-Term Support)** — the first time OpenAI/GitHub guaranteed a model version remains available for a fixed period, **through February 4, 2027**, giving enterprises deployment stability.
- Self-referential milestone: early versions of GPT-5.3-Codex were used during its own training, deployment, and test diagnosis — the first OpenAI model instrumental in creating itself (recursive self-improvement loop).

### Architecture & technical specs
- **Architecture:** not officially disclosed; analyses point to advanced Mixture-of-Experts / sparse activation optimized for programming logic.
- **Context window:** 400,000 tokens; max output 128,000 tokens.
- **Speed:** **25% faster** than GPT-5.2-Codex on agentic tasks, with fewer tokens used, thanks to inference-stack improvements.
- Knowledge work hybrid: coding specialization + reasoning/professional knowledge of GPT-5.2.

### Key benchmarks (reported values)
| Benchmark | GPT-5.3-Codex | Predecessors |
|---|---|---|
| **SWE-Bench Pro (Public)** | **56.8%** (SOTA) | 56.4% (5.2-Codex), 55.6% (5.2) |
| **Terminal-Bench 2.0** | **77.3%** | 64.0% (5.2-Codex), 62.2% (5.2) |
| **OSWorld-Verified** (agentic computer use) | **64.7%** | 38.2% (5.2-Codex) |
| SWE-Lancer (IC-Diamond) | 81.4% | — |
| Cybersecurity CTFs | 77.6% | — |
| LiveBench | 72.8% | — |
- Contamination-resistant design: SWE-Bench Pro spans 4 programming languages.

### API pricing & availability
- **$1.75 / 1M input tokens, $14.00 / 1M output tokens** (confirmed via llm-stats.com / official pricing pages, Aug 2026 recheck).
- Note: early Feb 2026 coverage said "pricing not separately announced from the GPT-5.3 family"; $1.75/$14 figures come from later official pricing pages.
- **Access (at launch):** ChatGPT paid tiers (Plus, Pro, Business, Enterprise, Edu) via Codex app, Codex CLI (`npm i -g @openai/codex`), IDE extensions, web; **API access delayed** because the model was flagged **"High Capability" in cybersecurity** under OpenAI's Preparedness Framework (dual-use exploit-discovery concern) — a precautionary access control.
- GitHub Copilot: GA February 2026, 25% faster on agentic tasks.
- Three products share the "Codex" name: v1 = the 2021 API model (deprecated 2023), v2 = the CLI agent (Apr 2025–), v3 = the cloud agent (May 2025–).

### Agentic / coding use cases
- Interactive agentic coding paradigm: **real-time steering** — developers intervene mid-execution (e.g., switching protocol requirements) without restarting; frequent progress updates; plan → tool-use → iterate loops.
- Beyond code: debugging, test creation, documentation, refactoring, deployment support, monitoring, PRD writing, user research — near every professional computing task.
- Long-horizon projects: builds functional complex games/apps from scratch over multi-day runs.

### What distinguished it in 2026
- **Terminal-first agentic specialist**: it dominated terminal-based workflows (Terminal-Bench 2.0 77.3% vs rivals) at 2.9× cheaper input pricing than Opus-class models — the price-efficiency leader for terminal-heavy engineering, with enterprise-grade LTS stability.

---

## 3. Gemini 3.1 Pro (Google DeepMind)

### Release & announcement
- **Released February 19, 2026 — in Preview** (`gemini-3.1-pro-preview`), ~3 months after Gemini 3 Pro (Nov 2025) — a fast turnaround by frontier standards.
- Preview → staged rollout: developer launch in Feb; consumer rollout accelerated May 4, 2026 in the Gemini app (expanded limits for AI Pro/Ultra subscribers) and NotebookLM for paid users. Gemini 3 Pro was discontinued on Vertex AI on March 26, 2026, pushing enterprise customers onto the 3.1 track.

### Architecture & technical specs
- **Architecture:** Mixture-of-Experts multimodal reasoning model with extended chain-of-thought ("thinking" / reasoning levels: Minimal, Low, Medium, High — configurable reasoning compute per request).
- **Context:** 1,048,576 input tokens (~1M; ≈1,500 A4 pages) → suitable for whole-codebase/long-context RAG. **Output:** 65,536 tokens per response.
- **Multimodality (native):** text, images, video, audio, PDF, code repositories as first-class inputs (e.g., ~45 min video with audio, 10 videos per prompt); text output only (no image/audio generation in preview). Live API not supported.
- Knowledge cutoff: January 2025 (API docs).
- Output speed: ~104–116 tokens/sec (well above peer median of ~72 t/s).
- **Dedicated agentic endpoint:** `gemini-3.1-pro-preview-customtools` — optimized for custom tool calls (view_file, search_code, bash) and multi-step agentic pipelines.

### Key benchmarks (reported values, Feb 2026)
| Benchmark | Score |
|---|---|
| **ARC-AGI-2** | **77.1%** — more than double Gemini 3 Pro (31.1%) |
| **SWE-Bench Verified** | **80.6%** (Google's figure; ~75% on third-party BenchLM standardization) |
| GPQA Diamond | 94.3% |
| MMMLU | 92.6% |
| MMMU / MMMU-Pro | ~62–64% / ~80–84% (independent trackers) |
| APEX-Agents | 33.5% (vs Claude Opus 4.6's 29.8%) |
- Headline claim: **1st place on 13 of 16** evaluated benchmarks (Google).

### API pricing & availability
- **$2.00 / 1M input, $12.00 / 1M output** for prompts ≤200K tokens; **$4.00 / $18.00** above 200K — **unchanged from Gemini 3 Pro**, making it a "free performance upgrade." Context caching supported (up to ~75% cost reduction); cached input ~$0.20/1M.
- Consumer: bundled in Google AI Pro and AI Ultra subscriptions (regional pricing varies).
- Channels: Gemini app, AI Studio, Gemini API, Vertex AI, Gemini CLI, Google Antigravity platform, Android Studio, NotebookLM, GitHub Copilot / VS Code extensions.

### Agentic / coding use cases
- **Creative & agentic coding:** code-based SVG animations, 3D visualizations, live aerospace dashboards, interactive design prototypes from text prompts alone.
- **Long-context RAG workflows:** entire codebases, contracts, research corpora in a single pass.
- Enterprise: Batch API, context caching, function calling, code execution, search grounding, structured outputs, URL context.

### What distinguished it in 2026
- **The reasoning-leap release at flat pricing**: generational jump (ARC-AGI-2 doubled, SWE-Bench 80.6%) with **zero price increase** — and the multimodal/reasoning flagship anchoring Google's entire 2026 ecosystem (Antigravity, AI Studio, NotebookLM, Android Studio), with a purpose-built agentic tool-calling endpoint.

---

## Comparison summary (February 2026 wave)

| Dimension | Claude Sonnet 5 "Fennec" | GPT-5.3-Codex | Gemini 3.1 Pro |
|---|---|---|---|
| **Release** | Feb 3, 2026 | Feb 5, 2026 | Feb 19, 2026 (Preview) |
| **Context** | 1M tokens | 400K tokens | 1M tokens |
| **SWE-Bench Verified** | **82.1%** | 56.8% (SWE-Bench Pro) | 80.6% |
| **Terminal-Bench 2.0** | 80.4% (2.1) | **77.3%** | — |
| **Headline reasoning** | Dev Team orchestration | Self-recursive training | ARC-AGI-2 **77.1%** |
| **Input / Output $ per 1M** | $3 / $15 | $1.75 / $14 | $2 / $12 |
| **Standout** | Agentic autonomy, best price/perf | Terminal workflows + LTS stability | Multimodal reasoning at flat pricing |
# VOLET 1 — Vague 1: Frontier AI Models (February → September 2026)
## Research fiche: Grok 4.20 · DeepSeek V4 / V4.1 Flash · GLM-5.2 / GLM-5.3 / GLM-5.3-Flash

*Research conducted 2026-09-22. All benchmark figures are vendor-reported unless marked as independently measured. Vendor-published numbers should be treated as upper bounds, not final verdicts.*

---

# 1. Grok 4.20 (xAI)

## 1.1 Release & status
- **Public beta launched 17 February 2026.** Elon Musk announced it in a post (no formal launch window — "it was live"); users had to manually select "Grok 4.2" in the model menu.
- **Rapid-learning architecture:** first Grok that improves continuously after deployment — capabilities updated weekly based on user feedback, with release notes published alongside each update. Musk stated Grok 4.20 would be "an order of magnitude smarter and faster" than Grok 4 by the time the beta concludes.
- **Still in beta as of mid-2026**, accessible to SuperGrok subscribers; by September 2026 the API lists dated snapshot variants (`grok-4.20-multi-agent-0309`, `grok-4.20-0309-reasoning`, `grok-4.20-0309-non-reasoning`), indicating the line is in active production use while successor `grok-4.3` rolls out in stages.
- Prior cadence: Grok 4 (9 Jul 2025, native tool use + real-time search, RL at pretraining scale on 200K GPUs), Grok 4 Heavy (9 Jul 2025), Grok 4.1 (17 Nov 2025, emotional-intelligence upgrade). Grok 4.20 arrived ~3 months after Grok 4.1.

## 1.2 Architecture: four-agent system (multi-agent)
- **Not one monolithic model — four specialized agents** that think in parallel and debate in real time before answering:
  - **Grok** — lead orchestration
  - **Harper** — research and fact-checking
  - **Benjamin** — logical verification
  - **Lucas** — creative synthesis
- Agents cross-check outputs and reach **"adversarial consensus"** before the lead agent synthesizes the final response.
- Headline safety metric: **hallucination rate dropped from ~12% to ~4.2% (−65%)**.
- **"Heavy" mode scales to 16 agents** for demanding tasks, drawing on xAI's 200,000-GPU Colossus supercluster.
- **Context:** 256K window, up to 2M (per third-party analysis).
- **Native multimodal:** text + image + video; video generation and revamped image generation added to the platform in early 2026; Grok Imagine 1.5 (image/video engine) as of June 2026.
- **Training:** reinforcement learning at pretraining scale on the 200K-GPU Colossus supercluster.
- **New capability highlights at beta:** medical document analysis via photo upload; improved engineering reasoning; financial reasoning (strong showing in live trading simulations, e.g. Alpha Arena: +34.59% returns in one report, ~12.11% average / 47% max in a stealth "mystery model" debut per crypto press).
- **Musk roadmap note:** Grok 5 (next flagship, aimed at AGI-level breakthroughs, reportedly ~6T parameters — 2× Grok 4) was expected "in a few months" from early 2026.

## 1.3 Benchmarks
- **LMArena Elo: estimated 1505–1535 (provisional).** Grok 4.1 Thinking already sat at 1483; the multi-agent council + extra inference-time compute was projected to add 20–60 Elo, potentially #1 overall. No official provisional ranking at beta time (internal/beta status).
- **ForecastBench: #2.**
- **Safety Overfit tests: strong (per NextBigFuture).**
- **Live trading competitions (Alpha Arena): +34.59% returns while competitors posted losses** (vendor-adjacent claim; treat with caution).
- **Comparative press (mid-2026):** OpenAI GPT-5.4 "wins on reliability and reasoning; Grok 4.20 wins on personality and speed" — both described as the most human-feeling assistants either company had shipped.

## 1.4 License & availability
- **Closed/proprietary.** No open weights.
- **Consumer access:** X Premium+ and SuperGrok subscribers (manual model selection at beta); SuperGrok Heavy ($300/mo) unlocks Grok 4 Heavy multi-agent (16-agent) mode. Free tier limited; X Premium bundles include Grok access.
- **Developer API (api.x.ai / console.x.ai):** dated model IDs — `grok-4.20-multi-agent-0309`, `grok-4.20-0309-reasoning`, `grok-4.20-0309-non-reasoning`. Dated IDs are the only route to a guaranteed version (consumer apps don't disclose which variant answered).
- **API pricing (2026, per 1M tokens):** input $1.25 / cached input $0.20 / output $2.50 — same as `grok-4.3`; 1M context. (Dedicated coding model `grok-build-0.1`: $1.00 in / $2.00 out, 256K context.) xAI changes API prices with little warning.
- **Subscription vs API billing are separate products:** API usage is pay-per-token and does NOT draw on SuperGrok subscription quotas.

## 1.5 Position vs competitors (Feb 2026)
- Multi-agent "council" approach differentiated it from monolithic frontier models (Claude, GPT, Gemini); hallucination-reduction narrative targeted enterprise reliability concerns.
- Weaknesses at launch: limited official benchmark suite; beta-only availability; personality-forward brand that critics saw as a liability.

---

# 2. DeepSeek V4 & DeepSeek V4.1 Flash

## 2.1 DeepSeek V4 — release
- **Planned/announced for mid-February 2026, ~17 February (Lunar New Year)** — reported by The Information citing people with direct knowledge; DeepSeek declined to comment. Strategy mirrored the R1 launch (Jan 20, 2025, one week before Lunar New Year; R1 had triggered a ~$1T tech selloff).
- **DeepSeek's first flagship built specifically for coding** — successor to V3.2 (Dec 2025).
- Two serving tiers: **V4-Pro** (flagship) and **V4-Flash** (efficient).

## 2.2 DeepSeek V4 — architecture
- **Hybrid MoE architecture** optimized for multi-file refactoring, full-stack scaffolding, and whole-repo ingestion in a single prompt.
- **Engram Conditional Memory** (paper: DeepSeek + Peking University, 12–13 Jan 2026, arXiv:2601.07372) — conditional memory as a complementary sparsity axis to MoE; O(1) lookups for static patterns, freeing GPU for reasoning.
- **mHC (Manifold-Constrained Hyperconnections)** (1 Jan 2026) — training-stability fix enabling larger models on the same hardware.
- **Muon optimizer** for training stability.
- **Hybrid attention:** Compressed Sparse Attention + Heavily Compressed Attention.
- **Pre-training: 32T+ tokens.**
- **V4-Pro:** 1.6T total / 49B active; **V4-Flash:** 284B total / 13B active.
- **Context:** 1M tokens (up from 128K on V3.1).
- **API features:** OpenAI ChatCompletions + Anthropic Messages formats, JSON mode, tool/function calling, up to 128 parallel function calls, chat-prefix completion (beta), FIM completion (beta, non-thinking), dual Thinking/Non-Thinking modes, automatic KV-cache reuse.
- **Integrations:** Claude Code, OpenCode, OpenClaw, CodeBuddy (pre-tuned adapters on Pro).

## 2.3 DeepSeek V4 — benchmarks (vendor-reported)
| Benchmark | V4-Pro | V4-Flash | Reference points |
|---|---|---|---|
| SWE-bench Verified | **80.6%** | ~77% | Claude Opus 4.6: 80.8% — near tie |
| SWE-bench Pro | ~55–58% | ~53% | Claude Opus 4.7: 64.3% (gap real) |
| Terminal-Bench 2.0 | **67.9%** | ~58% | Opus 4.7: 69.4%; GPT-5.5: 82.7% |
| LiveCodeBench | **93.5%** | ~89% | best-in-class reported |
| Codeforces | **3206** | — | GPT-5.4: 3168 |
| GPQA Diamond | ~90–92% | — | Opus 4.7: 94.2% |
| MMLU-Pro | ~90% | — | Opus 4.5: 89.5% |
| AIME 2026 | ~95% | — | Opus 4.5: 95.1% |
| MCPAtlas (agentic) | **73.6** (Pro-Max) | — | tied Opus 4.6; leads open models |
| Toolathlon | 51.8 (Pro-Max) | — | — |
| SkillsBench Avg5 | ~50% | ~45% | — |

Note: Anthropic publishes SWE-bench **Pro** (81.2 for Fable 5.1), OpenAI quotes **DeepSWE** (74.1 for GPT-6 Astra) — three vendors, three benchmarks, no like-for-like comparison. DeepSeek itself acknowledges V4 "falls marginally short of GPT-5.4 and Gemini 3.1 Pro" on world knowledge by ~3–6 months.

## 2.4 DeepSeek V4 — license, price, availability
- **Open weights, MIT license** (both tiers) — Hugging Face / ModelScope.
- **API pricing (per 1M):** V4-Pro $1.74 in / $3.48 out ($0.145 cached in); V4-Flash $0.14 in / $0.28 out ($0.028 cached in). V4-Pro output ~87% cheaper than GPT-5.4 ($15) and ~91% cheaper than Claude Opus 4.6 ($25).
- Typical 10-step agent workflow: <$0.05 with optimized Flash/Pro routing vs $0.50+ with Opus 4.7/GPT-5.5.

---

## 2.5 DeepSeek V4.1 Flash — release (10 September 2026)
- **Released 10 September 2026** — not a leak, not a beta. A two-day API test (model ID `deepseek-v4.1-flash-expires-on-0910`, from Sept 8) ended on schedule; release landed across web app, mobile app, first-party API, with **weights + technical report on Hugging Face** (`deepseek-ai/DeepSeek-V4.1-Flash`).
- API model ID: **`deepseek-flash`** (single name). Legacy IDs `deepseek-v4-flash` / `deepseek-v4-flash-vision-exp` now route to V4.1 Flash at Flash pricing (retired).
- **DeepSeek is retiring the ~4× more expensive V4 Pro on 14 September 2026**, stating "V4.1 Flash has comprehensively surpassed V4 Pro across all key metrics."
- Independent analysis: "not a point release — a generation that declined to name itself."

## 2.6 DeepSeek V4.1 Flash — architecture (detailed)
- **Causal Encoder-Decoder (CED):** 40-layer Transformer split into a **20-layer causal encoder + 20-layer decoder**. The decoder's global KV cache is projected from the encoder's final hidden states instead of rebuilt layer-by-layer.
- **Parameters:** 552B backbone — **8B active per token during prefill, 16B during decode** (the "smallest model in the new architecture family").
- **+196B Engram conditional-memory parameters** (sparse, token-based lookup) → ~763B stored parameters in the HF repo.
- **MoE layout:** each MoE layer has 1 shared expert + 384 routed experts, 6 routed experts activated per token.
- **Attention/cache efficiency:** Compressed Sparse Attention 2 (CSA2) + **FP4 KV caching** (E2M1) + shared attention indices → **~890 bytes/token global KV cache** (~¼ of V4-Flash, ~1/437th of DeepSeek V1).
- **SWA Bounded Replay:** cuts persistent KV-cache footprint to ~⅛ of V4-Flash.
- **Training:** from scratch on **45T mixed text+image tokens**; context extended to 1M at the 34T mark.
- **Native multimodal visual understanding** (text + image in, text out).
- **Context:** 1M tokens; **max output 384K tokens**; concurrency limit 2,500 (vs 500 on V4-Pro — 5× throughput headroom).
- **Reasoning effort dial 1–100** (continuously controllable; thinking on by default with low/high/max presets) — trade accuracy vs cost per request without switching models.
- **Post-training:** SFT + RL + on-policy distillation; agentic gains credited to "large-scale automated synthesis of agent tasks and environments."

## 2.7 DeepSeek V4.1 Flash — benchmarks
| Benchmark | V4.1 Flash | Reference |
|---|---|---|
| Terminal-Bench 2.1 | **90.6** | ahead of Claude Opus 5 (vendor claim) |
| DeepSWE v1.1 | **74.2** (up from 54.4 on prior Flash) | — |
| GPQA Diamond | **90.9** (vs 92.4 prior flagship) | "98% of flagship score, 45% of price" |
| (Flash-0731 preview row, vendor) | TB 2.1: 82.7; Toolathlon-V: 70.3; Agents' Last Exam: 25.2; AutomationBench: 25.1 | Opus 4.8 beats Flash on all nine rows (DeepSeek published both) |

## 2.8 DeepSeek V4.1 Flash — license, price, availability
- **MIT license, weights + repo on Hugging Face and ModelScope** — permissive end of the open-weights spectrum (no revenue threshold, no field-of-use clause); self-host, fine-tune, serve commercially, royalty-free. Inference folder + separate encoding module in repo; community building inference support from day one.
- **Official API pricing (per 1M, off-peak):** $0.15 input / $0.60 output / $0.003 cached input. **Peak** (Mon–Fri 01:00–04:00 & 06:00–10:00 UTC): doubled — $0.30 / $1.20 / $0.006. Weekends always off-peak.
- **Speed (observed):** ~190–427 tok/s community-reported; 215 tok/s p50 in one routing analysis; p50 TTFT 842 ms, p95 2.40 s, ~1.2% error rate.
- **Distribution:** DeepSeek app (rolling out), official API, **OpenRouter** (`deepseek/deepseek-v4.1-flash` — 1,048,576-token context, reasoning default `high`, tools, JSON, streaming; upstreams include DeepInfra, Novita, Venice; 99.92% 3-day availability); partners WorkBuddy, CodeBuddy, OpenCode Go, ClinePass.
- **Positioning:** killed its own premium tier — "a lab deciding the premium tier was no longer worth charging for." Best value in its class on price/perf/agent metrics per multiple independent analyses.

---

# 3. GLM-5.2 / GLM-5.3 / GLM-5.3-Flash (Zhipu AI / Z.ai)

## 3.1 GLM-5.2 — release
- **Released 13 June 2026** on the GLM Coding Plan; **API, chatbot, and MIT open weights followed 16–17 June 2026** (weights ~2–4 days after launch; Hugging Face + ModelScope; official FP8 build day one; community GGUF quantisations — Unsloth — shortly after).
- Unusual launch: **zero benchmark numbers published at release** (no SWE-bench, no Terminal-Bench) — full benchmark suite published with the open weights on June 17.
- Succeeds GLM-5.1 in the GLM-5 family; Z.ai (formerly Zhipu AI) monetises via hosted API + flat-rate GLM Coding Plan, not by withholding weights.
- **"New DeepSeek moment"**: as of 16 June 2026, Artificial Analysis ranked GLM-5.2 the **leading open-weights model on its Intelligence Index v4.1 (score 51)**, ahead of DeepSeek V4-Pro and Kimi K2.6 — a reversal of the GLM-5.1 mid-pack picture.

## 3.2 GLM-5.2 — architecture
- **MoE: 744B total / ~40B active per token** (Z.ai's own spec: "744B-A40B"; some write-ups say 753B — the HF safetensors count includes embedding/output-head weights outside the MoE budget; same model).
- **Context: 1,048,576 tokens (1M)** — 5× GLM-5.1's ~200K; max output 128K–262K (sources vary: 131,072 in model card; 262,144 per AgentRiot).
- **IndexShare sparse-attention technique:** groups of four transformer layers share one lightweight indexer — keeps 1M-context inference costs under control.
- **Dual reasoning modes: High (fast) and Max (deep reasoning)**; also non-thinking mode.
- **Text only — no vision.**
- Agent-first design; **Anthropic-compatible endpoint** → works with Claude Code, Cline, OpenCode, OpenClaw (one base-URL change); exposed in Claude Code as model ID `glm-5.2[1m]`.
- **ZCode:** official Claude-Code-style agent harness shipped alongside, built to drive GLM-5.2.

## 3.3 GLM-5.2 — benchmarks
| Benchmark | GLM-5.2 | Reference |
|---|---|---|
| AA Intelligence Index v4.1 | **51 — #1 open-weights** | ahead of DeepSeek V4-Pro, Kimi K2.6 |
| Terminal-Bench 2.1 | **81.0** | Kimi K3: 88.3; Claude Opus 4.8: 85.0; DeepSeek V4-Flash-0731: 82.7 |
| SWE-bench Pro | **62.1** | — |
| DeepSWE v1.1 | 46.2 | — |
| Agents' Last Exam | 23.8 | — |
| AutomationBench Public | 12.9 | — |
| DSBench-FullStack/Hard (internal) | 68.7 / 59.6 | — |
| (Z.ai vendor table, via VentureBeat) | edges past OpenAI GPT-5.5 on several multi-step engineering benchmarks | vendor's own table, not independent |

- **Positioning:** first open-weights model to close the gap with closed frontier models on *long-horizon* coding — within single-digit points of Claude Opus 4.8 on long-horizon coding tasks; hardest general-reasoning crown still held by closed frontier.

## 3.4 GLM-5.2 — license, price, availability
- **MIT license — fully permissive open weights** (no regional restrictions).
- **API pricing (per 1M, unchanged from GLM-5.1):** $1.40 input / $4.40 output / $0.26 cached input.
- **Speed:** ~141 tok/s.
- Available: GLM Coding Plan, Z.ai API/chatbot, Claude Code, third-party platforms; self-host via SGLang/vLLM-class + GGUF.

---

## 3.5 GLM-5.3 — release (14 August 2026)
- **Released 14 August 2026** (announcement thesis: "Scaling post-training is all we did for GLM-5.3").
- **Same ~743–744B-parameter MoE base as GLM-5.2** — no new base model, no bigger architecture; **all claimed gains from post-training alone** (statement about where the frontier is moving: next jump from training smarter, not bigger).
- Available at launch via **GLM Coding Plan**, working with **ZCode, Claude Code, OpenCode**.
- **Weights staged:** not downloadable at launch — open release planned ~2 weeks later, pending security reviews. Unlike GLM-5.2, API access + weights staged behind safety review, not shipped day one.
- Crowded launch week: Google shipped Gemini 3.7 Flash (Aug 13); competing coding updates from DeepSeek (V4-Pro) and Alibaba (Qwen).

## 3.6 GLM-5.3 — architecture & the cyber angle
- Same MoE base as 5.2 (~743B total / ~40B active class).
- **Post-training focused on coding + cybersecurity:** trained with data and environments built specifically to find software vulnerabilities → **"emergent cyber capability"** that arrived faster than the team expected.
- **2,436 vulnerabilities uncovered across 269 projects during testing** (some flaws up to 40 years old), per Zhipu.

## 3.7 GLM-5.3 — benchmarks (all vendor-published — treat as claims, not independent verification)
- **+50% coding capability vs GLM-5.2** (Zhipu's in-house coding benchmark).
- **Terminal-Bench 3.0: 28.3**; **SWE-Marathon: 42.5** — claimed open-source SOTA.
- **Agents' Last Exam (CLI): open-source SOTA** (claimed).
- **CyberGym: 84.5** — tops the vulnerability-discovery benchmark, edging Anthropic's Mythos 5 in Zhipu's headline comparison; **>2× GLM-5.2 on exploitation benchmarks**.
- Zhipu calls it **"the strongest open-weights coding model available today"** (own benchmarks).
- Direct competition: DeepSeek V4-Pro, Alibaba Qwen coding updates.

---

## 3.8 GLM-5.3-Flash ("FlashX") — release (26 August 2026)
- **Launched 26–27 August 2026** — first **natively multimodal** model in the GLM-5 series (image + video inputs from the architecture level up, not bolted on).
- **Stealth preview as "Ox Alpha":** anonymised model briefly **topped OpenRouter's usage charts in mid-August** before attribution; community forensics (tokenizer match, Z.AI error codes, Java stack trace, Aug 21–22) pointed to Zhipu before Z.ai/Bloomberg/Caixin/CNBC confirmed.
- **Runs entirely on ~100,000 domestically manufactured Chinese AI accelerators** — no Nvidia hardware; architected for deployment across domestic Chinese GPU families. Pointed rebuttal to the narrative that SOTA Chinese AI depends on Nvidia; major signal on semiconductor self-sufficiency under US export controls.
- **Z.ai shares gained ~8% on the announcement.**

## 3.9 GLM-5.3-Flash — architecture
- **MoE: 320B total / 18B active per token.**
- **Hybrid sparse + linear attention** — design choice cutting compute significantly.
- **Context: 1M tokens.**
- **Natively multimodal:** native image + video input; text output.
- **MIT open-weights license** — weights at `huggingface.co/zai-org/GLM-5.3-Flash`; local inference via SGLang, vLLM, TokenSpeed.
- Post-trained on the 743B-parameter base (per explainx summary of launch materials).

## 3.10 GLM-5.3-Flash — benchmarks (vendor-reported)
| Benchmark | GLM-5.3-Flash | Reference |
|---|---|---|
| DeepSWE v1.1 | **63.4** | vs 46.2 for GLM-5.2 |
| AA Intelligence Index | **57** | — |
| GDPVal-AA v2 | **1773** | Claude Opus 4.8: 1582 |
| AutomationBench | **48.8** | Opus 4.8: 41.0 |
| Terminal-Bench 2.1 | 84.3 | Opus 4.8: 85.0 |
| HLE w/ Tools | 55.3 | Opus 4.8: 57.9 |
| Z.ai Code Bench v1.0 (max effort) | 29.0 | Opus 4.8: 29.5 — near parity |

## 3.11 GLM-5.3-Flash — license, price, availability
- **MIT license, open weights day one** (contrast with GLM-5.3 non-Flash, staged behind safety review).
- **API pricing (per 1M):** $0.15 input / $0.50 output / $0.03 cached input — ~33× cheaper input and ~50× cheaper output than Anthropic Opus 5 ($5/$25); ~$0.045/task on AA Intelligence Index v4.1.1 with discounts (~10× cheaper than prior frontier-tier intelligence per Z.ai).
- **OpenRouter:** live (formerly Ox Alpha); Z.ai API; GLM Coding Plan; ZCode.
- Gap flagged for wave 2: the brief's note (SWE-bench Verified 70–75%+ "per snapshots" for the GLM-5.3-FlashX/MiniMax class) could not be confirmed in web sources — no SWE-bench Verified figure published for GLM-5.3-Flash to date.

---

# 4. Cross-cutting comparison (vague 1 models)

| Dimension | Grok 4.20 | DeepSeek V4-Pro | DeepSeek V4.1 Flash | GLM-5.2 | GLM-5.3 | GLM-5.3-Flash |
|---|---|---|---|---|---|---|
| Release | 17 Feb 2026 (beta) | ~17 Feb 2026 | 10 Sep 2026 | 13–16 Jun 2026 | 14 Aug 2026 | 26 Aug 2026 |
| Weights | Closed | Open (MIT) | Open (MIT) | Open (MIT) | Open (MIT, staged) | Open (MIT) |
| Params (total/active) | n.d. | 1.6T / 49B | 552B / 8B–16B (+196B Engram) | 744B / 40B | ~743B / ~40B | 320B / 18B |
| Context | 256K (→2M) | 1M | 1M (384K out) | 1M | 1M | 1M |
| Multimodal | text+image+video | vision (V4.1) | native vision | text only | text (coding) | native image+video |
| Signature tech | 4-agent council, adversarial consensus | Engram memory, mHC, Muon | Causal Encoder-Decoder, FP4 KV, CSA2 | IndexShare attention | post-training scaling, cyber training | sparse+linear attention, CN-chip serving |
| SWE-bench Verified | n.d. | 80.6% | — | — (Pro: 62.1%) | — | — |
| Terminal-Bench | n.d. | 67.9% (2.0) | 90.6 (2.1) | 81.0 (2.1) | 28.3 (3.0) | 84.3 (2.1) |
| API in/out ($/1M) | 1.25 / 2.50 | 1.74 / 3.48 | 0.15 / 0.60 (off-peak) | 1.40 / 4.40 | n.d. (Coding Plan) | 0.15 / 0.50 |

# 5. Global trend (Feb–Sep 2026)
- **Chinese open-weight models (DeepSeek, Qwen, Kimi, GLM/Z.ai, MiniMax, MiMo) dominate the open-source top tier** in coding, agentic work, and price/performance. The pattern repeats: frontier-class scores at 5–10% of Western API prices, MIT-licensed weights, 1M-token contexts as standard.
- **Closed models (Claude Opus/Fable 5.x, GPT-5.x/6, Gemini 3.x, Grok 4.x) retain leads** on the hardest closed benchmarks and enterprise agentic tooling — but the gap on *long-horizon coding* has narrowed to single-digit points (GLM-5.2 vs Opus 4.8), and DeepSeek V4.1 Flash outright retired its own premium tier.
- **Architectural convergence on inference efficiency:** sparse MoE everywhere, KV-cache compression (FP4, CSA2, IndexShare), conditional memory (Engram), reasoning-effort dials, agent-task synthetic post-training.
- **Hardware sovereignty subplot:** GLM-5.3-Flash serving production traffic on ~100K domestic Chinese accelerators — first credible demonstration that SOTA open models can be trained/served without Nvidia.
- **Benchmark fragmentation:** vendors increasingly report on *different* benchmarks (SWE-bench Verified vs Pro vs DeepSWE vs Terminal-Bench), making like-for-like comparison unreliable — a structural caveat for the RAG.

# 6. Caveats & methodology notes
- All benchmark numbers above are **vendor-published** unless noted; independent evaluations (Artificial Analysis, community harnesses) exist for some models and generally show slightly lower figures.
- An agentic benchmark scores a *system* (model + harness + tools + retries), not a model alone — cross-harness comparisons are not apples-to-apples.
- GLM-5.3 (non-Flash) weights were still pending (~2 weeks post-launch) at last check — verify before citing as "open."
# RAG Research Brief — VOLET1 / Wave 1 (Part 3/3)
## Frontier AI Models Feb–Sept 2026: Chinese Open-Weight Dominance
**Compiled: 2026-09-22 — Research coverage for RAG ingestion**

---

## 1. XIAOMI MiMo-V2.6 (Pro / Flash / Pro-UltraSpeed)

**Release date:** Weights and announcement September 21–22, 2026 (Artificial Analysis lists Sept 21; Xiaomi announcement post dated Sept 22). Preceded by an unusual public "Live RL" training dashboard (mimo.xiaomi.com/rl) streaming the final RL run.

**Variants:**
- **MiMo-V2.6-Pro** — flagship, native omnimodal (text + image + audio + video in), 1M-token context window, sparse MoE: ~1.02T total parameters / ~42B active per token.
- **MiMo-V2.6-Flash** — efficiency tier: 309B total / ~15B active per token.
- **MiMo-V2.6-Pro-UltraSpeed** — inference tier delivering up to **20× faster output at the same quality** for latency-sensitive workloads; priced 10× the Pro tier.

**Architecture & training (key RAG facts):**
- Natively omnimodal, trained with scaled reinforcement learning on verifiable complex tasks (coding, general agents, visual, cybersecurity) in a single run.
- Production RL run: <6 days, 30 RL steps each for Flash and Pro, ~750,000 trajectories, 1,568 samples per update, 16 async rollouts per prompt, 3.5–3.7B tokens per step, contexts up to 1M tokens.
- Cost: ~$850K (Flash) and ~$2.62M (Pro) — comparatively modest frontier-class post-training bill.
- Anti-reward-hacking measures: frozen router, adversarial evaluation, anomaly detection, verifier cross-checks; graders that rank successful trajectories by quality, not just pass/fail.
- DeepSWE v1.1 (held-out) rose during the run: Flash 48.8 → 65.68; Pro 58.4 → 72.57 (final vendor table reports Pro 71.9).
- Predecessor MiMo-V2.5 (April 2026) scored only 19.0 on DeepSWE v1.1 — a ~50-point generational jump.

**Benchmarks (vendor-reported, Sept 2026):**
- **Artificial Analysis Intelligence Index v4.3: 46.32 → 46** — highest open-weight score on the leaderboard; ahead of GLM-5.3 (45), Kimi K3 (44), Qwen3.8 Max (45 on 0902 snapshot); tied with closed Grok 4.7 (46). Ahead of Grok 4.6 (44) and Gemini 3.8 Flash (41) on the same index.
- DeepSWE v1.1: Pro 71.9 vs Claude Opus 5 74.0, GPT-5.6 Sol ~73, Kimi K3 69.0, DeepSeek V4.1 Flash 74.2.
- AutomationBench v1.0.6: Pro 53.1 — beats Claude Opus 5 (50.3), GPT-5.6 Sol (45.8).
- Terminal-Bench 2.1: 89.9 (best in Xiaomi's table); Terminal-Bench 4.0: 34.9 (trails Opus 5 at 49.0 — hardest terminal agentic work still closed-lab territory).
- OSWorld-Verified: 82.0; Toolathlon-Verified: 76.9; MiMo Code Bench: 63.2; GDPval-AA 2.1 Elo: 1673 (vs Opus 5 1708); Agents' Last Exam: 31.6 (ties Opus 5).
- Visual: MiMo VisualCoding 72.3 — ahead of Opus 5 (70.0).
- Cyber: CyberGym 94.0; but ExploitBench 47.9 vs GPT-5.6 Sol 78.5; SEC Bench Pro 66.3 vs Sol 79.1 — offensive security remains a gap.
- Competitive coding gap: ProgramBench 26.5 vs Opus 5 37.0.

**License & price:** MIT license on Hugging Face + ModelScope. API: Flash $0.14/M input / $0.28/M output; Pro $0.435/M / $0.87/M (unchanged from V2.5); UltraSpeed 10× Pro. Artificial Analysis: $0.13 per Intelligence Index task; ~134 tok/s output. Xiaomi claims 1/20–1/60 the cost of overseas models at the same intelligence level. Available via OpenRouter.

**Positioning:** "Strongest open-source model to date" per Xiaomi; on par with Claude Opus 5 and GPT-5.6 Sol on most agent benchmarks per the launch post; still trails the top closed models (Claude Fable 5.1, GPT-6 Astra) on the hardest evals. Controversy note: some commentary alleges training-data borrowing from Anthropic's Claude — unverified, but worth flagging in RAG as disputed.

---

## 2. KIMI K3 / K2.x (Moonshot AI)

**Release timeline:**
- Kimi K2 (1.04T / 32.6B active, 128K ctx): July 11, 2025. K2-Instruct-0905 (256K): Sept 2025. K2 Thinking: Nov 2025. K2.5 (multimodal MoE, 1T/32B, 256K): Jan 27, 2026. K2.6: Apr 20, 2026. K2.7 Code: June 12, 2026.
- **Kimi K3: hosted service July 16, 2026; downloadable weights late July 2026 (~July 27); Amazon Bedrock GA September 18, 2026.**

**Architecture:** 2.8T total parameters — "first open model to reach 2.8T" per Moonshot. MoE "Stable LatentMoE": 16 of 896 experts active (~50B active). Innovations: Kimi Delta Attention + Attention Residuals (long-sequence/depth information flow); quantization-aware training from SFT onward (MXFP4 weights, MXFP8 activations). ~2.5× scaling-efficiency gain over K2. Native vision; 1M-token context.

**Benchmarks:**
- Artificial Analysis Intelligence Index: K3 (max) ~44 (v4.3.2) in Sept 2026 snapshots; earlier v4.1.1 readings ~59.7–60. Index-version changes make cross-version comparison invalid — note methodology version when citing.
- BenchAlign ~74.4 (per user brief; Chinese coding/agent composite where K3 frequently leads Chinese open models).
- Vendor claim: frontier-level across Moonshot's suite, consistently beating other tested open models; still trails proprietary Claude Fable 5 and GPT-5.6 Sol overall.
- K2.6: 80.2% SWE-bench Verified, 58.6% SWE-bench Pro (vendor).
- Known limitations (Moonshot): trained in "preserved thinking history" mode — unstable if harness drops thinking history; long-horizon bias can cause unexpected autonomous decisions on ambiguous instructions.

**License & price:** K2 series: modified MIT. K3: new "open weight" license — large MaaS businesses (>$20M/yr revenue) must sign a separate agreement; NOT traditional open source. Weights 1.56 TB on Hugging Face. Commercial access via OpenRouter, Moonshot API, and now AWS Bedrock (first open-weight model on Bedrock with explicit prompt caching; runs in Bedrock security/compliance boundary).

**Positioning:** Moonshot is the most open-weight-forward Chinese frontier lab and the only one repeatedly setting the open-model size ceiling (K2 → K2.5 → K3). K3 competes as the top Chinese open model on several composites but was overtaken by MiMo-V2.6-Pro (46) and Qwen3.8-Max-0902 (45) on AA Index v4.3 in Sept 2026.

---

## 3. QWEN3.8 MAX (Alibaba) + Qwen3.x family

**Release timeline:** Qwen3-Max (est. 1T): Sept 2025. Qwen3.6-Max-Preview (closed): Apr 2026. Qwen3.7-Max (closed): May 2026. **Qwen3.8-Max-Preview: July 19, 2026 (WAIC, no benchmark table). Qwen3.8-Max GA: August 3, 2026. Snapshot Qwen3.8-Max-0902: September 2, 2026** (post-training focused on coding/long-horizon agents).

**Architecture:** 2.4T total / ~95B active sparse MoE (Qwen3.5 architecture lineage). Alibaba's first multimodal model above 1T params. Native: 1M-token context, 131K max output, up to 262K reasoning tokens; text/image/video in, text out. Reasoning effort low/medium/xhigh (default xhigh). RL-environment scaling (~4,000 internal envs cited as generational-jump mechanism).

**Benchmarks:**
- AA Intelligence Index v4.3: 40 (Aug 3 GA) → **45 (0902 snapshot)** — reclaimed #1 Chinese-model spot mid-Sept, ahead of GLM-5.3 (44.9) and Kimi K3 (43.8), before MiMo-V2.6-Pro hit 46.
- Earlier v4.1.1: 58.1 (hosted) / 57.7 (open 2.4T-A95B weights) — effectively tied; vs Claude Opus 5 max 63.1, Fable 5 62.1, GPT-5.6 Sol 60.9, Kimi K3 max 59.7, GLM-5.3 max 59.5 on same version.
- Code Arena WebDev: 0902 snapshot ranked 1st (1,691), ahead of Claude Opus 5 Max.
- Caveat: 0902's intelligence gain came with verbosity inflation — reasoning tokens/task 44K → 71K, output 63K → 108K tokens; cost/task $2.67 → $5.41 at unchanged $2/$6 per M pricing.

**License & price:** API $2.00/M input / $6.00/M output; cache read $0.25 (10%), cache write $2.50; flat rate across full 1M context (no tiered long-context pricing, unlike Gemini 3.1 Pro). Open weights: PROMISED ("week of Aug 10" incl. Qwen3.8-27B) but not shipped as of research date; AA classifies the model proprietary. Note: do NOT confuse Qwen3.8-Max (2.4T flagship) with Qwen3-8B (8.2B dense Apache-2.0 model, Apr 2025) — search engines conflate them.

**Positioning:** Alibaba's flagship; strongest on web-dev/coding-agent tasks among Chinese models in Sept 2026; priced well above MiMo/DeepSeek Flash tiers — the "premium Chinese open-weight" play vs Xiaomi/Moonshot cost leadership.

---

## 4. MINIMAX M2.5 / M3

**MiniMax M3 — release June 1, 2026** (weights on Hugging Face by ~June 7–13; tech report arXiv June 11).
- Architecture: 428B total / ~22–23B active MoE, 128 experts (4 active); MiniMax Sparse Attention (MSA): GQA + sparse attention cutting per-token compute at 1M context to ~1/20 of prior gen; ~15.6× faster decoding, ~9.7× faster prefill at 1M. Native multimodal (text/image/video from step 0, ~100T interleaved training tokens) — structural edge over text-only DeepSeek V4, GLM-5.2, Qwen3.6. 1M context.
- Benchmarks: **SWE-bench Verified 80.5%**; SWE-bench Pro 59.0% (vendor — edged GPT-5.5's 58.6% at launch, first open-weight model to do so); Terminal-Bench 2.1 66.0%; MCP Atlas 74.2%; OSWorld-Verified 70.06%; LiveCodeBench 82.2; BrowseComp 83.5 (beats Claude Opus 4.7's 79.3). AA caveat: low attempt rate on AA-Omniscience (30.9% — low hallucination, low accuracy).
- Price: official $0.60/M in / $2.40/M out (promo $0.30/$1.20); Morph serving $0.255/$1.02 (256K). Among cheapest models above 80% on SWE-bench Verified.
- License: announced open-weight; license terms flagged as potentially restrictive for commercial use (confirm before self-host reliance).

**MiniMax M2.5:** prior generation; per user brief, M2.5/M3 class holds 70–75%+ SWE-bench Verified across snapshots; M2.5 established MiniMax in the agentic-coding tier before M3's June leap. (M2.5-specific independent figures are sparse in Sept-2026 coverage — treat the 70–75% band as snapshot-dependent, vendor/aggregator-reported.)

**Positioning:** M3 was "the first open-weight model to combine frontier coding + true 1M context + native multimodality." Directly collapses the cost/control case for closed APIs: ~1/30th the cost of Claude Fable 5-class, ~1/10th of Claude Opus 4.8 on coding workflows.

---

## 5. GLOBAL TREND — Chinese open-weights vs closed frontier (Sept 2026)

1. **Chinese labs own the open-weight coding/agent leaderboard.** Sept 2026 AA open rankings: MiMo-V2.6-Pro (46) > GLM-5.3 (45) > Qwen3.8-Max-0902 (45) > Kimi K3 (44) — all Chinese. SWE-bench Verified 78–80.6% cluster: DeepSeek V4 Pro 80.6%, MiniMax M3 80.5%, Kimi K2.6 80.2%, GLM-5.1 ~78%, MiMo-V2.5 Pro 78.0%.
2. **The efficiency frontier moved to Flash-tier models:** DeepSeek V4.1 Flash (Sept 10, 2026): 552B MoE (8B/16B active), 1M ctx, MIT, $0.14/$0.28 — near-frontier coding at the cheapest frontier-class price. MiMo-V2.6-Flash and GLM-5.3-FlashX play the same game.
3. **Closed models still lead on the hardest evals:** Claude Opus 5.x / Fable 5.1, GPT-5.x–6 (Sol/Astra), Gemini 3.x, Grok 4.x lead on Terminal-Bench 4.0 (Opus 5: 49.0 vs MiMo 34.9), ProgramBench competitive programming (37.0 vs 26.5), exploit-generation (ExploitBench 78.5 vs 47.9), and top AA Index absolute scores (Opus 5 max 63.1 vs best open ~58–60 on v4.1.1 scale).
4. **Cost asymmetry is the story:** open-weight Chinese models deliver ~90–95% of closed-frontier agentic coding at 1/10 to 1/60 the price (MiMo-Pro $0.13/task vs dollars-per-task closed equivalents; M3 at 1/30th of Fable 5-class).
5. **Transparency as strategy:** Xiaomi's live-streamed RL training (public dashboard, published costs, open-sourced RL tooling/environments) contrasts with US labs' closed training — reproducibility as competitive positioning. NVIDIA's Nemotron 3 Ultra (June 2026, 550B/55B, 71.9% Verified, free on OpenRouter) is the first Western open model partially breaking the Chinese monopoly in this tier.
6. **Caveats for RAG:** (a) AA Intelligence Index versions (v4.1.1 vs v4.3/v4.3.2) are NOT comparable across versions — always record the version; (b) vendor-reported SWE/DeepSWE figures are provisional until independently reproduced; (c) "open-weight" ≠ "open-source" — K3's MaaS revenue clause, Qwen3.8-Max's unshipped weights, and MiniMax's license terms all restrict the label.

---

## 6. Quick-reference table (Sept 2026 snapshots)

| Model | Release | Params (total/active) | Context | AA Index (v4.3) | SWE-bench Verified | License | API $/M (in/out) |
|---|---|---|---|---|---|---|---|
| MiMo-V2.6-Pro (Xiaomi) | 2026-09-21 | 1.02T / 42B | 1M, omni | 46 | n/r (DeepSWE 71.9) | MIT | 0.435 / 0.87 |
| MiMo-V2.6-Flash | 2026-09-21 | 309B / 15B | 1M, omni | — | n/r (DeepSWE 67.9) | MIT | 0.14 / 0.28 |
| Kimi K3 (Moonshot) | 2026-07-16 (wt late Jul) | 2.8T / ~50B | 1M | 44 | — (K2.6: 80.2%) | Open-weight* | via Bedrock/API |
| Qwen3.8-Max-0902 (Alibaba) | 2026-09-02 (GA Aug 3) | 2.4T / 95B | 1M | 45 | — | Proprietary (wt promised) | 2.00 / 6.00 |
| MiniMax M3 | 2026-06-01 | 428B / 23B | 1M, native multi | — | 80.5% | Open-weight‡ | 0.60 / 2.40 |
| DeepSeek V4.1 Flash | 2026-09-10 | 552B / 8–16B | 1M | 39 | 79.0% | MIT | 0.14 / 0.28 |
| GLM-5.3 (Z.ai) | 2026 (mid) | — | 1M (5.2+) | 45 | ~78% (5.1) | MIT | — / 3.20 |

\* K3: "open weight" license with >$20M/yr MaaS separate-agreement clause. ‡ M3: confirm commercial terms.
