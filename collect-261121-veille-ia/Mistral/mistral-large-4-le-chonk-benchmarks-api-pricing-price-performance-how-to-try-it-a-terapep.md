---
id: collect-261121-veille-ia/mistral/mistral-large-4-le-chonk-benchmarks-api-pricing-price-performance-how-to-try-it-a-terapep
title: "Mistral Large 4 (Le Chonk): Benchmarks, API Pricing, Price-Performance & How to Try It Â· Terapep"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: ["2026-10-30"]
keywords: ["mistral", "large", "benchmarks", "pricing", "active", "hugging", "face", "preview"]
source: https://www.terapep.com/blog/mistral-large-4-benchmarks-pricing-how-to-use-2026/
source_anchor: ""
source_lines: [1, 197]
section: "Mistral"
---
# Mistral Large 4 (Le Chonk): Benchmarks, API Pricing, Price-Performance & How to Try It

Mistral Large 4 open weights: 1.05T MoE, 52B active, 1M context, $0.70/$2.10 per 1M. Vendor vs independent benchmarks, price-performance, and how to try it.

**TL;DR**: Mistral put the weights for **Mistral Large 4 ("Le Chonk")** on Hugging Face on **Oct 29, 2026** (Oct 29â30 KST), three weeks after the API preview on Oct 6. It's a **1.05T-parameter MoE with 52B active**, a **1M-token context**, and text + image input. API pricing is **$0.70 in / $2.10 out per 1M tokens**, 40% above Mistral Large 3. Mistral's own charts put it **first among open models on Terminal-Bench 4.0 (41.4%) and FrontierCode 2.0 (19.7%)**. On the independent **Artificial Analysis Intelligence Index v5.0 it scores 26**, behind DeepSeek V4.1 Flash and GLM-5.3 (both 32) and far behind closed models (57â59). Where it stands out is speed and token efficiency: **161 tok/s**, and **$2.06 per index task** versus $5.54 for the preview.


*Labels used below: **[Vendor]** = numbers published by Mistral. **[Independent]** = Artificial Analysis (AA) or LMArena measurements. Prices are list prices as of Oct 30, 2026.*

## What was released, and when

| Date | Event | Source | 
|---|---|---|
| Oct 6, 2026 | Mistral Large 4 public preview on the Mistral API ( `mistral-large-4` ), with 50% off for 2 weeks at launch | Mistral news, docs changelog | 
| Oct 29, 2026 | Open weights on Hugging Face plus an improved (further RL-trained) checkpoint; API price set to $0.70 / $2.10 | Mistral news, Hugging Face blog | 
| Oct 29â30, 2026 | AA scores the final model; Intelligence Index v5.0 launches the same week | Artificial Analysis, The Decoder | 

## Specs at a glance

| Item | Mistral Large 4 | Mistral Large 3 (Dec 2025) | 
|---|---|---|
| Architecture | Granular MoE: 72 layers (1 dense + 71 MoE), 128 experts, top-4 + 1 shared active | MoE | 
| Total / active params | **1.05T / 52B** (Mistral docs) | 675B / 41B | 
| Attention | MLA + DeepSeek-style sparse attention indexer (â¤2,048 keys per query) | n/a | 
| Context | **1M tokens** | 256K tokens | 
| Modalities | Text + image in (1.6B Pixtral-style vision encoder), text out | Text + image in, text out | 
| Reasoning | Low / medium / high effort (default high) | n/a | 
| Languages | 160+ (Mistral) | n/a | 
| Training | From scratch on ~3,800 NVIDIA Grace Blackwell GPUs in Mistral's European datacenters | n/a | 
| Weights | `mistralai/Mistral-Large-4-1T-A52B` on Hugging Face; FP8 and NVFP4 checkpoints; community GGUFs | Open weights | 
| License | Mistral Ecosystem Open-Weight License (modified MIT) | Apache 2.0 (as listed on LMArena) | 
| API price (in / cached / out per 1M) | **$0.70 / $0.07 / $2.10** | $0.50 / $0.05 / $1.50 | 

**License catch:** you can use, modify and ship Large 4 commercially for free. But if you sell it as a hosted **Model-as-a-Service**, you need a separate commercial license from Mistral.

**Parameter-count discrepancy:** Mistral's docs and the Hugging Face repo name say **52B active**. The Hugging Face launch blog says "about 1T total, 49B active", and AA's preview entry lists 1T / 49B with a 524K context. This post uses Mistral's docs figures (1.05T / 52B / 1M).

*Source: Mistral docs model cards and pricing.*

**Is there an official Large 3 vs Large 4 benchmark table?** No. Mistral's launch posts compare Large 4 with other open models, not with Large 3, and AA has no v5.0 index score for Large 3. The only independent head-to-head we found is LMArena Text (Oct 27 snapshot): **Large 4 scores 1432 Â± 6 (#113)** and **Large 3 scores 1414 Â± 3 (#146)**, an **+18 Elo** gap. LMArena's Large 4 entry still shows preview pricing ($1.36 / $4.18), so it most likely reflects the preview checkpoint.

## Benchmarks vs open-weight rivals [Vendor]

All of these numbers come from the charts in Mistral's Oct 29 open-weights post (we read them from the chart data embedded in the page). Mistral picks the benchmarks and the comparison set.

| Benchmark | Mistral Large 4 | GLM-5.3 | DeepSeek V4.1 Flash (1017) | Kimi K3 | 
|---|---|---|---|---|
| Terminal-Bench 4.0 (%) | **41.4** | 40 | 31 | 21 | 
| FrontierCode 2.0 (Cognition, private) | **19.7** | 17.8 | 19.3 | 17.3 | 
| AA Cyber Index | 50.9 | 36 | **57** | 41 | 
| Finch / FinWorkBench (%) | 73.3 | 65.12 | n/a (V4 Pro 0813: 67.44) | **77.33** | 
| BrowseComp (%) | 84.5 | 82.5 | 83.4 | **91.2** | 
| Surge Code human preference: Large 4 win rate vs this model | â | 64% | 56% | 64% | 

*Source: Mistral AI, Oct 29, 2026 open-weights announcement (vendor-reported).*

What the table shows:

- **Coding is the real win.** Large 4 leads both coding agent benchmarks, but only narrowly: 41.4 vs 40 on Terminal-Bench, 19.7 vs 19.3 on FrontierCode. For scale, The Decoder reports**Claude Opus 5.5 at 46% on FrontierCode 2.0** , and in Surge's blind coding comparison**Large 4 beat Opus 5.5 in only 22% of matchups** .
- **"Leads defensive security" is overstated.** On AA's Cyber Index, Mistral's own chart puts**DeepSeek V4.1 Flash (57) and MiMo-V2.6-Pro (56) above Large 4 (50.9)** . That makes it third among open models, not first.
- **Office and search work is good, not best.** Kimi K3 beats it on both Finch (77.33 vs 73.3) and BrowseComp (91.2 vs 84.5).
- **Safety is strong.** Large 4 refuses**96.6%** of harmful cyber requests (Kimi K3 93.6, GLM-5.3 87.1, DeepSeek-V4-Pro 50.7) and scores**97.1%** on B3 attack resistance.
- **Missing rival:** Kimi K3.1 Pro isn't in Mistral's charts because its weights aren't public yet (Moonshot says they will be by Nov 4). The Decoder reports it at**67% on Terminal-Bench 4.0** .

## Independent ranking: Artificial Analysis Intelligence Index v5.0 [Independent]

| Model | AA Intelligence Index v5.0 | Weights | 
|---|---|---|
| Gemini 4 Carbon (high) | 59 | Closed (limited access) | 
| Claude Opus 5.5 (max) | 58 | Closed | 
| GPT-6 Astra (max) | 57 | Closed | 
| Kimi K3.1 Pro | 46 | Weights not yet released | 
| DeepSeek V4.1 Flash 1017 (max) | 32 | Open | 
| GLM-5.3 (max) | 32 | Open | 
| MiMo-V2.6-Pro | 30 | Open | 
| **Mistral Large 4 (high)** | **26** | Open | 
| Mistral Large 4 Preview | 25 | â | 
| Mistral Large 4 (medium) | 23 | Open | 

*Source: Artificial Analysis leaderboard and model pages, read Oct 30, 2026.*

AA notes that Large 4 (high) beats the **median of 23** for open models in its size class, ranking #9 of 21 in that class. The Hugging Face blog calls it the "fifth best open weights model" on AA as of Oct 29. On the current list several open models sit between 27 and 32, so read that rank as approximate. The bottom line is the same either way: Large 4 is a solid mid-pack open model and **6 points behind the best Chinese open models**.

## API price vs rivals

| Model | Input $/1M | Output $/1M | Notes | 
|---|---|---|---|
| DeepSeek V4.1 Flash | 0.30 | 1.20 | AA | 
| Mistral Large 3 | 0.50 | 1.50 | Mistral docs | 
| **Mistral Large 4** | **0.70** | **2.10** | Mistral docs; cached input $0.07 | 
| GLM-5.3 | 1.40 | 4.40 | AA | 
| Kimi K3.1 Pro | 2.00 | 10.00 | The Decoder | 
| Claude Opus 5.5 | 4.00 | 20.00 | Anthropic | 
| Gemini 4 Carbon | 4.00 | 20.00 | List price; $2 / $10 during launch promo | 
| GPT-6 Astra | 10.00 | 50.00 | OpenAI | 

*Sources: Mistral, Anthropic and OpenAI pricing pages; Artificial Analysis; The Decoder.*

The preview's list price was **$1.36 / $4.18**, halved to $0.68 / $2.09 for launch. The final $0.70 / $2.10 roughly locks in that launch discount. Compared with Large 3, both input and output are **+40%**. AA's blended price (7:2:1 cache-hit/input/output mix) is **$0.40 per 1M**.

## Price-performance: what a task actually costs [Independent]

Per-token price doesn't capture how many tokens a model burns. AA's **cost per Intelligence Index task** does.

| Model | Cost per task | AA index | Index points per $ (our calc) | 
|---|---|---|---|
| MiMo-V2.6-Pro | $0.26 | 30 | 115.4 | 
| DeepSeek V4.1 Flash 1017 | $0.60 | 32 | 53.3 | 
| **Mistral Large 4 (high)** | **$2.06** | **26** | **12.6** | 
| Kimi K3.1 Pro | $3.68 | 46 | 12.5 | 
| GLM-5.3 (max) | $5.01 | 32 | 6.4 | 
| GPT-6 Astra (max) | $6.24 | 57 | 9.1 | 
| Gemini 4 Carbon (high) | $7.76 | 59 | 7.6 | 
| Claude Opus 5.5 (max) | $10.04 | 58 | 5.8 | 

*Source: Artificial Analysis (Intelligence Index v5.0 cost per task), read Oct 30, 2026. "Points per $" is our calculation.*

- Large 4 costs **less than half of GLM-5.3 per task** while scoring 6 points lower.
- It is **3â8Ã more expensive per task than DeepSeek V4.1 Flash and MiMo-V2.6-Pro** , which also score higher. On pure cost-efficiency, the Chinese open models win clearly.
- **Token efficiency [Vendor]:** on AA's Coding Agent Index, Mistral says Large 4 (high) reaches**48 points using 8.5M tokens per task** , versus GLM-5.3 at 49 with 15.9M. That's the "**47% fewer tokens** " claim. DeepSeek V4.1 Flash uses 16.7M tokens to reach 46.**[Independent]:** AA counted**110M output tokens** across the full index, down from 150M for the preview but slightly above the class median of 100M.
- **Speed [Independent]:****161.3 tok/s** output and**1.42 s** time to first token on AA, one of the fastest models in its class.

## Three weeks of extra training: preview vs final

| Metric | Preview (Oct 6) | Final (Oct 29) | Change | Type | 
|---|---|---|---|---|
| Terminal-Bench 4.0 | 28% | 41.4% | +13.4 pts | Vendor | 
| Finch | 67.4% | 73.3% | +5.9 pts | Vendor | 
| B3 attack resistance | 93.3% | 97.1% | +3.8 pts | Vendor | 
| Harmful-cyber refusal | 95.3% | 96.6% | +1.3 pts | Vendor | 
| AA Intelligence Index v5.0 | 25 | 26 | +1 | Independent | 
| AA Cyber Index | ~50 | 51 | +1 | Independent (via The Decoder) | 
| AA output speed | 106 tok/s | 161 tok/s | +52% | Independent | 
| AA cost per index task | $5.54 | $2.06 | â63% | Independent | 

*Sources: Mistral (first four metrics, vendor); Artificial Analysis (index, speed, cost; independent).*

The vendor numbers moved a lot on coding. The independent general-intelligence score barely moved (+1). The biggest independently confirmed gains are **speed (+52%) and cost per task (â63%)**.

## How to try it

**1. In the browser (no code).** Sign in to **Mistral Studio** (formerly La Plateforme) and test the model in the playground. Mistral also lists Large 4 in **Vibe Code**, its coding agent. Mistral hasn't clearly said whether the consumer chat app (Vibe, formerly Le Chat) offers it in the model picker, so check there before you count on it.

**2. Through the API.** Create a key in Mistral Studio, then:

```
export MISTRAL_API_KEY="your-key"
curl https://api.mistral.ai/v1/chat/completions \
  -H "Authorization: Bearer $MISTRAL_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "mistral-large-4",
    "messages": [{"role": "user", "content": "Review this function for edge cases: ..."}]
  }'
```
- The model ID is `mistral-large-4` . The docs list structured outputs, function calling, document QnA, batching, agents and built-in tools as supported.
- Reasoning effort is adjustable (low / medium / high; default high). Lower settings cut both tokens and cost: AA measured **$1.41 per task at medium** vs $2.06 at high, for 23 vs 26 points.
- Cached input costs **$0.07 per 1M** , so reuse long system prompts and documents.
- Large 4 is also available on **OpenRouter, Microsoft Foundry, NVIDIA NIM, Hugging Face Inference Endpoints and the Dell Enterprise Hub** .

**3. Self-hosting the open weights.** The repo is `mistralai/Mistral-Large-4-1T-A52B`, with FP8 and NVFP4 checkpoints and support in vLLM, SGLang, transformers and llama.cpp. Expect to need serious hardware:

| Setup | Reported result | Source | 
|---|---|---|
| 8Ã H200, FP8 | ~65 tok/s single request; 120â138 tok/s with speculative decoding | Hugging Face blog | 
| 8Ã B300 (standard) / 4Ã GB300 (compressed) | Supported configurations | SGLang docs via The Decoder | 
| Mac Studio M3 Ultra 512GB, Q2_K GGUF | ~15 tok/s | Hugging Face blog | 

Use the launch flags from the model card for your engine. Long-context and sparse-attention support differs between versions.

## Who should use it

- **Good fit:** teams that need a**European-trained, self-hostable** frontier-scale model (data residency, "sovereign AI" procurement), and**agentic coding** pipelines where its Terminal-Bench lead and lower token use matter. It also suits defensive security work that needs a model that**refuses harmful requests reliably** , and**long-document and multimodal** workloads that use the 1M context.
- **Look elsewhere if:** your main goal is the**lowest cost per correct answer** (DeepSeek V4.1 Flash and MiMo-V2.6-Pro score higher on AA for a fraction of the price), you need**top-tier reasoning** (closed models lead by about 30 index points), or you plan to**resell it as a hosted API** without a commercial license from Mistral.
- **Upgrading from Large 3:** you pay 40% more per token for 4Ã the context, a reasoning mode, and +18 LMArena Elo (preview checkpoint). There's no official benchmark table comparing the two, so run your own evals before switching.

## Open questions and caveats

- **Most headline numbers are vendor-reported.** FrontierCode 2.0 is private, and Surge's preference study was commissioned for the launch.
- **Active parameters:** 52B (Mistral docs, repo name) vs 49B (Hugging Face blog, AA preview entry).
- **Coding Agent Index:** the preview post claimed 49.8%, while the final post shows 48. These are probably different index versions, so don't read it as a regression or a gain.
- **No LMArena score for the final checkpoint yet.** The listed entry still shows preview pricing.
- **Rival prices shift often.** Gemini 4 Carbon is on promo pricing and limited access, and Kimi K3 has been superseded by K3.1 Pro.

## Sources

- Mistral AI: Mistral Large 4 (preview announcement, Oct 6) Â· Mistral Large 4 open weights (Oct 29)
- Mistral docs: Mistral Large 4 model card Â· Mistral Large 3 model card Â· Models overview Â· Changelog
- Hugging Face: Welcome Le Chonk: Mistral Large 4 on Hugging Face Â· mistralai/Mistral-Large-4-1T-A52B
- Artificial Analysis: Leaderboard / Intelligence Index v5.0
- LMArena: Text leaderboard
- Coverage: The Decoder Â· VentureBeat
- Pricing: Anthropic pricing Â· OpenAI API pricing

*Caution: benchmark scores and prices change fast. Check the linked pages before you make a purchasing or deployment decision.*

â Back to all posts
