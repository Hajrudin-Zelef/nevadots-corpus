---
id: collect-261121-veille-ia/glm-zhipu/glm-5-3-flash-launch-ox-alpha-was-zhipu-mit-explainx-ai-blog
title: "GLM-5.3-Flash Launch — Ox Alpha Was Zhipu (MIT) | explainx.ai Blog"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: []
keywords: ["glm-5", "flash", "alpha", "weights", "launch", "august", "stealth", "update"]
source: https://www.explainx.ai/blog/glm-5-3-flash-ox-alpha-official-launch-august-2026?ref=hackernoon.com
source_anchor: ""
source_lines: [1, 172]
section: "GLM_Zhipu"
---
**Update — August 28, 2026:** GLM-5.3-Flash's day-one MIT weights are now a useful contrast point: the separate, larger **GLM-5.3** (non-Flash, cyber-defense line) missed its own staged-release target. Z.ai's Hugging Face placeholder listed August 28 for GLM-5.3's open weights, and that date passed with nothing published. See GLM-5.3 open weights delayed — Z.ai misses its own Aug 28 target for the full timeline. Those open weights also let Unsloth ship a Dynamic 3-bit GGUF sized to run on 128GB of RAM.

**Update — August 30, 2026:** Open-weight **GLM-5.3 placed third on Terminal-Bench 4.0**, ahead of GPT-5.6 Sol on the terminal-agent leaderboard — GLM-5.3 Terminal-Bench 4.0 third place.

**Update — August 30, 2026:** The MIT license also made GLM-5.3-Flash a target for third-party refusal removal — OrcaRouter shipped an uncensored build with the edit baked directly into the model's native block-FP8 weight shards, not a LoRA adapter or a prompt trick: OrcaRouter's uncensored GLM-5.3-Flash, explained.

**Update — August 27, 2026:** The launch date matters for a second story — three Google Gemini team posts from **August 22** were retroactively read as trolling this launch, four days before it happened. See the corrected Ox Alpha timing timeline, which also puts GLM-5.3-Flash and Gemini 3.7 Flash side by side on price, license, and vendor benchmarks.

**Update — September 4, 2026:** GLM-5.3-Flash is running a launch promo through September 9, 2026 at $0.075/M input and $0.25/M output — about **18x cheaper** than flagship GLM-5.3's $1.40/$4.40 list price. After the promo ends it reverts to $0.15/$0.50 list price, roughly a 9x discount. Full pricing breakdown, plus a fact-check of a separate viral claim about Claude Fable 5.1 and SimpleBench, in GLM-5.3 Flash's price cut and the Fable 5.1 SimpleBench claim, checked.

On **August 26, 2026 at 7:42 PM**, **Z.ai** (@Zai_org) posted the reveal the community spent six days chasing:

**Introducing GLM-5.3-Flash** — Previously previewed as **Ox Alpha**, running entirely on **Chinese AI chips**.


The tweet hit **740K+ views** within an hour. Weights landed on **Hugging Face** under **MIT License**. API docs went live at **docs.z.ai/guides/llm/glm-5.3-flash**. The stealth codename `stealth/ox-alpha` on OpenRouter was always a preview skin — this is the named product.

If you followed the Ox Alpha forensics timeline, the ending is almost anticlimactic: **stack traces, error code 1214, and 30/30 tokenizer matches to GLM-5.3 were right**. Bloomberg confirmed Zhipu that morning; Z.ai confirmed the SKU and architecture that evening.

## TL;DR — GLM-5.3-Flash at a glance

| Question | Answer | 
|---|---|
| **Former codename** | **Ox Alpha** (`stealth/ox-alpha` on OpenRouter) | 
| **Architecture** | **320B total / 18B active** (MoE-style sparse activation) | 
| **Context** | **1M tokens** , natively**multimodal** (text, image, video) | 
| **License** | **MIT** — weights on Hugging Face | 
| **API price** | **$0.15/M in · $0.50/M out · $0.03/M cached** | 
| **Stealth infra** | Entire preview served on **Chinese AI chips** , per Z.ai | 
| **Inference stacks** | **SGLang, vLLM, TokenSpeed** (local); ZCode, Chat, AutoClaw | 
| **vs GLM-5.2** | Large gains on **DeepSWE** (+17.2 pts) and**AutomationBench** (+22.6 pts) | 
| **vs Opus 4.8** | Leads **GDPVal-AA v2** ; trails**Terminal Bench 2.1** slightly | 
| **Coding Plan** | **3× usable quota** vs GLM-5.3 on same subscription tier | 

## From Ox Alpha stealth to named model

The launch closes a six-day arc explainx.ai tracked in three acts:

| Date | Event | 
|---|---|
| **Aug 20** | Ox Alpha appears on OpenRouter — free, 1M context, anonymous Stealth provider | 
| **Aug 21–22** | Community forensics tie it to **Z.AI / GLM-5.3** (full evidence) | 
| **Aug 26 AM** | **Bloomberg** : Zhipu confirms GLM lineage; weights promised that night | 
| **Aug 26 PM** | **Z.ai** : names**GLM-5.3-Flash** , ships MIT weights, publishes benchmarks | 

Z.ai's launch blog is explicit about the strategy: test anonymously on OpenCode and OpenRouter, become **"the most popular model of the week,"** then ship under the GLM brand with open weights. Hunter Alpha and Healer Alpha → MiMo was the same playbook from a different lab.

**What changes for builders now:**

- Route to **`z-ai/glm-5.3-flash`** (or Hugging Face locally) instead of guessing about`stealth/ox-alpha` longevity
- **MIT license** — self-host, fine-tune, strip guardrails (with the usual responsibility caveats)
- **Multimodal native** — not a text-only model with bolt-on vision; Z.ai frames vision as part of the coding loop (render → verify → refine)

## Specs and architecture

From Z.ai's technical post:

| Spec | GLM-5.3-Flash | GLM-5.2 (prior) | 
|---|---|---|
| **Total params** | 320B | (prior gen larger activated count) | 
| **Active params** | **18B** | 32B (GLM-4.5 series comparison in blog) | 
| **Layers** | **45** | 92 (GLM-4.5 series) | 
| **Context** | **1M tokens** | Shorter in prior GLM-5.2 API tier | 
| **Attention** | **Hybrid linear + sparse** + IndexPool for long context | Prior architecture | 
| **Pre-training** | **30T-token multimodal corpus** | — | 

Architectural headline: **Manifold-Constrained Hyper-Connections (mHC)** plus hybrid attention cut long-context serving cost — Z.ai claims **3.0× less attention compute** and **4.4× smaller KV cache** vs GLM-5.3 (non-Flash) on per-token metrics.

## Benchmarks — coding and agentic

Z.ai's published evaluation table (August 26, 2026):

| Benchmark | **GLM-5.3-Flash** | GLM-5.2 | DeepSeek-V4-Vision-Exp | Claude Opus 4.8 | GPT-5.6 Terra | Gemini 3.7 Flash | 
|---|---|---|---|---|---|---|
| **Terminal Bench 2.1** | 84.3 | 81.0 | 83.9 | **85.0** | **87.4** | 85.8 | 
| **DeepSWE v1.1** | **63.4** | 46.2 | 59.3 | 58.0 | **69.6** | 65.3 | 
| **AutomationBench v1.0.6** | 48.8 | 26.2 | 38.8 | 41.0 | 37.2 | **52.3** | 
| **Agents' Last Exam** | 26.3 | 20.4 | **27.3** | 27.0 | **28.0** | — | 
| **HLE w/ Tools** | 55.3 | 54.7 | 55.1 | **57.9** | — | — | 
| **GDPVal-AA v2** | **1773** | 1504 | 1675 | 1582 | 1571 | 1527 | 
| **Toolathlon Verified** | **78.4** | 59.9 | 75.9 | 76.2 | 74.9 | — | 

**explainx.ai read:**

- **Biggest jump vs GLM-5.2:** AutomationBench (48.8 vs 26.2) and DeepSWE (63.4 vs 46.2) — aligns with Ox Alpha's agent traffic on OpenRouter (Claude Code, Hermes Agent)
- **GDPVal-AA v2 leadership** is the headline for enterprise agentic workflows — beats Opus, GPT-5.6 Terra, and Gemini 3.7 Flash
- **Not a blanket frontier win:** GPT-5.6 Terra still leads DeepSWE (69.6) and Terminal Bench (87.4); Gemini 3.7 Flash leads AutomationBench (52.3)
- **Independent Ox Alpha DeepSWE claim (80%)** from Ben Davis during stealth may reflect harness differences vs Z.ai's official 63.4 — always run your own eval

On **Z.ai Code Bench v1.0** (Claude Code 2.1.207 harness), Flash **beats GLM-5.2 at every effort level** and at max effort scores **29.0 vs Opus 4.8's 29.5** — functionally tied on Z.ai's in-house coding eval.

## Pricing and where to run it

### API (standard)

| Tier | Price per 1M tokens | 
|---|---|
| **Input** | $0.15 | 
| **Output** | $0.50 | 
| **Cached input** | $0.03 | 

Compare to Claude / GPT frontier pricing — Flash-tier MoE at sub-dollar output is the China playbook in action (free preview → named open model).

### Platforms (all live Aug 26)

| Surface | URL | 
|---|---|
| **Weights** | huggingface.co/zai-org/GLM-5.3-Flash | 
| **API** | docs.z.ai/guides/llm/glm-5.3-flash | 
| **Coding Plan** | z.ai/subscribe — **3× quota** vs GLM-5.3 | 
| **ZCode** | zcode.z.ai — Browser Use + Computer Use | 
| **Chat** | chat.z.ai | 
| **AutoClaw** | autoclaw.z.ai | 
| **OpenRouter** | Likely `z-ai/glm-5.3-flash` soon if not already — watch model page | 

### Local inference

```
# Example — verify current flags against Z.ai docs before production
# SGLang / vLLM support announced at launch; TokenSpeed also listed
```
Z.ai notes **SGLang, vLLM, and TokenSpeed** at launch; more frameworks incoming. For agent harness setup patterns, see how to run GLM-5.2 on coding agents — routing is the same, swap model ID.

## Chinese AI chips — the infra story

The line that will age into geopolitics textbooks:

*"All of this traffic served on Chinese AI chips."*


Z.ai's blog describes:

- **Large-scale cluster** of domestic accelerators with high-bandwidth interconnect
- Custom **SGLang inference engine** — partially built with**GLM-5.3 infrastructure agents** (model optimizing its own serving stack)
- **EPD disaggregation** — Encode / Prefill / Decode worker pools at cluster scale
- **3× end-to-end serving improvement** vs baseline on same hardware,**comparable per-token cost to NVIDIA GPUs**

For builders outside China, the practical takeaway is not nationalism — it is **proof that a MoE Flash model can serve at OpenRouter scale without H100 clusters**, which matters for inference cost deflation forecasts.

## What people are asking

### "Should I stop using stealth/ox-alpha on OpenRouter?"

**Migrate to the named model when OpenRouter lists it** — stealth previews can disappear or reprice without notice. Until the slug updates, `stealth/ox-alpha` may still work but treat it as deprecated.

### "Is this the same as GLM-5.3 from August 14?"

**No — different SKUs.** GLM-5.3 (Aug 14) is the **743B-base cyber-defense** line with **staged** open weights. **GLM-5.3-Flash** is a **320B-A18B multimodal Flash** variant with **immediate MIT release**. Same family name, different products.

### "MIT license — can I use it commercially?"

**Yes**, MIT is permissive. Standard caveats: verify the Hugging Face repo license file, run your own safety evals for regulated domains, and do not assume Z.ai's hosted API has the same data-retention terms as self-hosted weights.

### "Does this change the US vs China model routing map?"

**Yes, incrementally.** A free stealth week proved demand; MIT weights plus **$0.15/$0.50 API** gives teams a tier-2 agentic route that does not depend on OpenRouter's anonymous provider. Pair with US frontier models for regulated tiers — same architecture AT&T documented.

## Bottom line

**GLM-5.3-Flash is Ox Alpha with a name, a license, and a price list.** The mystery was fun; the product is what matters: **320B-A18B, 1M multimodal context, MIT weights, agent benchmarks competitive with Opus on GDPVal and DeepSWE, API at flash-tier pricing, and a claim of full stealth-week serving on Chinese silicon.**

Run your evals. The stealth free lunch is over; the open-weight MoE era for agent harnesses is not.

**Update — September 17, 2026:** Z.ai published how this model's own inference stack was built: a GLM-5.3-powered Infra Agent took it from first run to production in 13 days, ending at 3.22x baseline throughput on 100,000+ Chinese-made accelerators. See GLM-5.3 built its own inference stack.

## Related on explainx.ai

**Official:** Z.ai GLM-5.3-Flash blog · Hugging Face weights · @Zai_org launch post

*Benchmarks, pricing, and architecture claims reflect Z.ai's August 26, 2026 launch materials. Run independent evals on your workloads before production routing. OpenRouter slug migration may lag the official announcement — verify live model IDs.*

**Update — October 8, 2026:** New comparison — Haiku 5.5 vs GPT-6 Luna vs DeepSeek V4.1 Flash vs Gemini 3.7 Flash vs GLM-5.3-Flash.
