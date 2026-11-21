---
id: collect-261121-veille-ia/qwen/qwen3-8-max-open-weights-are-live-august-2026-explainx-ai-blog
title: "Qwen3.8-Max Open Weights Are Live (August 2026) | explainx.ai Blog"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: ["2026-08-25"]
keywords: ["qwen3", "weights", "august", "open", "qwen", "model", "live", "blog"]
source: https://explainx.ai/blog/qwen3-8-max-open-weights-live-hugging-face-august-2026
source_anchor: ""
source_lines: [1, 145]
section: "Qwen"
---
**Update — August 25, 2026:** A ModelScope leak describes **Qwen3.8-Flash-Next** — a **125B-A6B MoE** on teased Qwen4 architecture, expected Aug 26 — as the locally oriented MoE tier below this 2.4T flagship.

**Update — August 15, 2026:** The missing half of this story shipped. Qwen3.8-27B — the smaller, locally-runnable companion this post says was "still not on the official Qwen org" — is now live and hit **#1 on Hacker News with 893 points**. Full coverage: Qwen3.8-27B: the local model Hacker News put at #1.

**Alibaba's open weights for Qwen3.8-Max are live.** Not "soon," not "week of August 10" — actually published, actually downloadable, and confirmed by an unlikely source: NVIDIA's own deployment engineering blog, dated **August 12, 2026**, which states plainly that "Alibaba released the open weights" while walking through how to serve the model on a GB300 NVL72 rack.

That closes a loop we flagged twice. Our July 19 preview coverage noted weights were promised "soon." Our August 3 coding-and-cowork post put "Open Weights Still Missing" in the headline because the product video shipped with no repo attached. Nine days later, the repo exists — but it is not the same model the API sells.

## TL;DR — what changed on August 12

| Question | Direct answer | 
|---|---|
| **Are weights really live?** | **Yes.**`Qwen/Qwen3.8-2.4T-A95B` and an`-FP8` variant on Hugging Face; NVIDIA's Aug 12 blog confirms it | 
| **Same model as the API?** | **No.** Open weights are**text-only** , thinking-mode only — no vision, no native 1M context | 
| **License?** | Custom **`qwen3.8-max`** license, not Apache 2.0 — reporting says a**revenue-share** clause applies to large commercial users | 
| **Size?** | **2.4T total / 95B active** (fine-grained MoE, ~4% activation), full + linear attention | 
| **Can I run it locally?** | Only on serious hardware — BF16 is **~4.89 TB** ; Unsloth GGUF quants run**397 GB–1.31 TB** even aggressively compressed | 
| **Reference deployment?** | NVIDIA's **GB300 NVL72** (72 Blackwell Ultra GPUs),**4K+ tok/s per GPU** in FP8 | 
| **Did Qwen3.8-27B ship too?** | **No.** The smaller companion Alibaba promised alongside it is still not on the official Qwen org | 
| **Community reaction?** | Mixed to negative — a Hugging Face discussion opened within hours calling the stripped feature set a "huge disappointment" | 

## What actually landed on Hugging Face

Two official repositories under the `Qwen` organization:

- **`Qwen/Qwen3.8-2.4T-A95B`** — BF16 safetensors, Hugging Face Transformers format, documented as compatible with vLLM, SGLang, and TokenSpeed
- **`Qwen/Qwen3.8-2.4T-A95B-FP8`** — fine-grained FP8 quantization (block size 128) for lower-memory serving

Unsloth followed within the same window with **`unsloth/Qwen3.8-2.4T-A95B-GGUF`**, covering quant levels from 1-bit through BF16. As of this snapshot the official repo shows modest but real traction — under a thousand downloads in the first period and roughly 575 likes — early numbers for a model this large, consistent with a fresh drop rather than a long-established release.

```
# Pattern: serve the FP8 checkpoint with vLLM (adjust tensor-parallel size to your node)
vllm serve Qwen/Qwen3.8-2.4T-A95B-FP8 \
  --tensor-parallel-size 8 \
  --max-model-len 32768
```
The model card explicitly frames this as a **post-trained** checkpoint aimed at `vLLM`/`SGLang`/`TokenSpeed` deployment — Alibaba is not pretending this is a laptop model, and neither should you.

## The catch: this is not the Qwen3.8-Max you tried in Qoder

The single most important fact in this release is what it **leaves out**. Compared to the hosted Qwen3.8-Max our coding-and-cowork coverage walked through:

| Capability | Qwen3.8-Max API (Token Plan / Qoder) | Open weights ( `Qwen3.8-2.4T-A95B` ) | 
|---|---|---|
| **Vision input** | Yes | **No** — text-only | 
| **Context window** | Up to **1M tokens** | Native window well below that | 
| **Output length** | Up to 128K | Documented, but tied to the smaller context | 
| **Thinking mode** | Optional | **Required** for all interactions | 
| **Tools / built-in agent features** | Full Qoder/QoderWork surface | Base checkpoint only | 
| **License** | Hosted terms | Custom `qwen3.8-max` license, revenue-share reported | 

A Hugging Face discussion thread on the repo — opened within hours of the drop and still active — captures the community reaction directly: commenters argue the release "removes half its core value" by stripping vision, call the gap between the `qwen3.8-max`-labeled license and the reduced feature set inconsistent, and compare the pattern to game-industry DLC paywalling, with at least one saying it cost Alibaba "all good faith" built up from prior Qwen generations. That reaction is worth reading before you plan a migration around this checkpoint — see our broader framework for choosing open weight vs. closed models when the open release does not match the flagship's marketing.

## The license: read it before you deploy

Earlier Qwen generations — including the models covered in our top open-weight models for a laptop roundup — shipped under permissive Apache 2.0 terms. This release does not. The Hugging Face repo lists a bespoke **`qwen3.8-max`** license, and reporting around the launch describes a **revenue-sharing requirement** for large commercial users, with the exact threshold and percentage still being finalized. That mirrors a trend other Chinese labs — Moonshot AI among them — have moved toward for their biggest open releases.

Practical takeaway: do not assume this is a drop-in Apache 2.0 replacement for Qwen 3.7-Max or Qwen 3.6 27B in your compliance paperwork. Pull the actual license text from the repository and have it reviewed before any commercial fine-tune, redistribution, or high-volume inference deployment — especially if your usage could cross whatever revenue threshold Alibaba eventually publishes.

## Hardware: still a data-center model, not a desktop one

The scale that made Qwen3.8-Max a flagship also makes the open weights impractical for most local setups:

| Format | Approx. size | Realistic hardware | 
|---|---|---|
| **BF16** (full) | ~4.89 TB | Multi-node GPU cluster | 
| **FP8** (official) | roughly half of BF16 | NVIDIA's own reference: **GB300 NVL72** , 72 Blackwell Ultra GPUs | 
| **Q8_0** (GGUF) | ~2.6 TB | High-end multi-GPU workstation cluster | 
| **UD-IQ4_XS** (4-bit) | ~1.31 TB | Still requires serious multi-GPU or heavy NVMe offload | 
| **UD-Q1_0 / IQ1_S / IQ1_M** (1-bit) | ~397–564 GB | The floor — aggressive quality loss expected at this bit depth | 

NVIDIA's deployment blog, published the same day as the wider confirmation, reports **over 4,000 tokens/sec per GPU** and **over 350 tokens/sec per user** serving the FP8 checkpoint on GB300 NVL72 — genuinely fast, but on hardware most teams do not own. If you want something that runs on a single card today, stay on Qwen 3.6 27B with llama.cpp or explore the broader locally-runnable open-weight roster — Qwen3.8-2.4T-A95B is not that model, even quantized to the floor.

## The missing half: Qwen3.8-27B

Alibaba's August 3 announcement paired two promises: the 2.4T Max-class flagship, and a **smaller Qwen3.8-27B** aimed at exactly the self-hosting, single-GPU audience the table above rules out for the big model. Only the flagship has shipped. As of this writing there is **no official `Qwen3.8-27B` repository** on the Qwen Hugging Face organization — only unofficial community forks quantizing a leaked or early checkpoint, which is not the same as an Alibaba-published release with a real license attached.

That matters more than it sounds. The 2.4T weights are a research and infrastructure release — genuinely open, genuinely usable by teams with GB300-class hardware, but out of reach for the local-first audience that runs open models on their own machines. The 27B was supposed to be the accessible half of the promise. It is still the missing half.

## How this fits the open-weight news cycle

This is the third Qwen3.8-Max post on explainx.ai, and each stage told a different story:

| Date | Stage | What shipped | 
|---|---|---|
| **July 19** | Preview | Qwen3.8-Max-Preview via API only; weights "coming soon" | 
| **August 3** | Product pitch | Coding-and-cowork positioning video; still API-only | 
| **August 12** | Open weights | 2.4T checkpoint on Hugging Face; text-only, new license, 27B still missing | 

Set against the wider China vs. closed-model debate, this is a mixed signal for the "China ships more open weights" narrative: Alibaba did open-source its first Max-tier flagship ever, which is genuinely new — but it did so with a narrower feature set and a license that pulls back toward commercial control, not further from it. Compare that to the more straightforwardly open DeepSeek Flash volume story or the broader China AI playbook of cheap, wide access — Qwen3.8-Max's open weights look more like a research and infrastructure showcase than a community handoff.

## Builder checklist

| Do | Don't | 
|---|---|
| Pull the actual `qwen3.8-max` license text before any commercial deployment | Assume it inherited Apache 2.0 terms from earlier Qwen generations | 
| Plan for GB300-class or heavily quantized multi-GPU serving | Expect this to run on a single consumer GPU, even at 1-bit | 
| Keep using the hosted API if you need vision or 1M context | Migrate agent workloads expecting parity with Qwen3.8-Max-Preview | 
| Watch for an official Qwen3.8-27B repo before betting on a local drop | Trust unofficial community forks as a substitute for Alibaba's own release | 
| Read the Hugging Face discussion threads before you commit to a migration | Treat "open weights are live" as "open weights are equivalent" | 

## Bottom line

**Qwen3.8-Max's open weights shipped on August 12, 2026** — a real, confirmable release, not another teaser. But "open weights" undersold what actually landed: a text-only checkpoint under a new revenue-share license, missing the vision and 1M-context features that made the hosted product notable, with the smaller, more locally-friendly Qwen3.8-27B still nowhere in sight. If your team was waiting to self-host the exact model you tested in Qoder, keep waiting — what you can download today is a different, narrower release wearing the same name.

## Related on explainx.ai

- 
Nous Research Arete-27B: a Qwen3.8-27B fine-tune, claimed vs verified
- 
GLM-5.3 ties Kimi K3 at 60 on the AA Intelligence Index (Qwen3.8-Max scores 58, just behind)
- 
Roboflow's vision benchmark: Roboflow's author calls Qwen3.8-Max "totally insane" among open models, though it's API-only via Alibaba Cloud
- 
Qwen hits 3 billion downloads (Aug 17) — the most downloaded model family, and what the number does and doesn't measure
- 
Qwen3.8-Flash-Next — leaked 125B-A6B MoE, expected Aug 26 (Aug 25)
- 
Qwen3.8-27B: the local model Hacker News put at #1 (Aug 15) — the missing companion model from this post, now shipped
- 
Fable 5 vs Grok 4.6 vs GPT-5.6 Sol vs Qwen3.8-Max: full comparison — how the hosted Qwen3.8-Max stacks up on cost and speed against Grok 4.6, Fable 5, and GPT-5.6 Sol
- 
Qwen3.8-Max coding & cowork video (Aug 3) — "Open Weights Still Missing"
- 
Qwen 3.8-Max Preview — Token Plan setup (July 19)
- 
Qwen 3.7-Max agent frontier
- 
Qwen 3.6 27B local with llama.cpp / OpenCode
- 
Top 10 open-weight models for a laptop
- 
Choosing open weight vs. closed AI models
- 
How to run open-source models locally in OpenCode
- 
American closed AI vs China open weights
- 
DeepSeek Flash 8T tokens/day
- 
China AI playbook — cheap compute & free models

**Primary sources:** NVIDIA — Serve Qwen3.8-2.4T-A95B on GB300 NVL72 (Aug 12, 2026) · Qwen/Qwen3.8-2.4T-A95B on Hugging Face · Qwen/Qwen3.8-2.4T-A95B-FP8 · unsloth/Qwen3.8-2.4T-A95B-GGUF

*Status as of August 13, 2026. Download links, license text, and quantization file sizes reflect Hugging Face listings as of publication — verify the current license and file manifest on the repository before any production or commercial deployment. Follow @explainx_ai for updates, including if Qwen3.8-27B ships.*
