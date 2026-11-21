---
id: collect-261121-veille-ia/grok-xai/grok-4-7-benchmarks-pricing-and-specs-a-spacexai-2026-the-ai-rankings
title: "Grok 4.7: Benchmarks, Pricing and Specs â SpaceXAI (2026) | The AI Rankings"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-09-21"]
keywords: ["grok", "spacexai", "september", "xhigh", "price", "model", "high", "cursor"]
source: https://theairankings.com/xai/grok-4-7/
source_anchor: ""
source_lines: [1, 176]
section: "Grok_xAI"
---
# Grok 4.7

- Provider
- SpaceXSI
- Status
- Current
- Context
- 500,000 tok
- Price
- $2 / $6 /MTok
- Knowledge
- 2026-05

Grok 4.7 is xAIâs flagship model, released on 21 September 2026 on the SpaceXAI API, in Cursor and in Grok Build, with gateway availability on OpenRouter, Vercel and Cloudflare (SpaceXAI). It succeeds Grok 4.6 after six weeks. SpaceXAI describes it as built on âa new, larger base modelâ with a longer reinforcement learning run weighted toward tasks that take many hours to complete; Elon Musk put the base at about 2.1 trillion parameters on 11 September. The release had been expected around 12 September and was delayed on the 11th; on the 13th Musk announced a larger Grok 4.8, which has no release date.

Artificial Analysis scored Grok 4.7 at **46 on its Intelligence Index v4.3** at xhigh effort at launch, two points above Grok 4.6 (44) and one below GPT-5.6 Sol (47) at the time. The price per token is unchanged at **$2/$6**, but the model uses about 81,000 output tokens per index task, against 36,000 for Grok 4.6, so its cost per completed task is higher: **$2.73 at high effort and $3.74 at xhigh**, against Grok 4.6âs $1.86. SpaceXAI publishes no SWE-bench figure of any kind.

## Quick specs

| **Provider** | xAI / SpaceXAI | 
| **Released** | 21 September 2026 (API, Cursor, Grok Build, OpenRouter, Vercel, Cloudflare) | 
| **API model ID** | `grok-4.7` (OpenAI-compatible endpoint) | 
| **Context window** | 500,000 tokens | 
| **Knowledge cutoff** | May 2026 | 
| **Parameters** | ~2.1 trillion (Musk; not in SpaceXAIâs documentation) | 
| **Input price** | $2.00 / MTok (doubled above 200K input) | 
| **Output price** | $6.00 / MTok (doubled above 200K input) | 
| **Modalities** | Text and image in; text out | 
| **Reasoning effort** | Low, medium, high (default), xhigh | 
| **AA Intelligence Index** | 46 at launch (v4.3, xhigh) | 
| **Cost per AA index task** | $2.73 (high), $3.74 (xhigh) | 
| **Limitations** | No SWE-bench figure; higher token use than Grok 4.6; Fast variant limited to Cursor and Grok Build | 

## Whatâs new in Grok 4.7

Grok 4.6 was a post-training release on Grok 4.5âs foundation. Grok 4.7 changes the base model: SpaceXAI describes âa new, larger base modelâ trained with a longer reinforcement learning run on a harder mix of tasks, weighted toward problems that take many hours to complete. SpaceXAI says the model works longer on difficult tasks and checks its own work more carefully.

The context window (500K), modalities (text and image in, text out) and price ($2/$6) are the same as Grok 4.6. Reasoning effort now runs from low to xhigh, with high the default. On the Responses API, encrypted reasoning is always returned so multi-turn conversations keep the modelâs reasoning state. A Fast variant at twice the standard rates is available in Cursor and Grok Build only.

## Benchmark performance

### SpaceXAIâs launch table (21 September 2026)

| Benchmark (21 September 2026) | Grok 4.7 | Grok 4.6 | GPT-5.6 Sol | Claude Fable 5.1 | 
|---|---|---|---|---|
| **CursorBench 4.0** | 46.3% | 40.4% | 41.7% | 51.8% | 
| **DeepSWE v1.1** | 71.0% (high) | 65.2% | 72.7% | 70.0% | 
| **EEBench** | 64.0% | 53.0% | 39.4% | 56.4% | 
| **AA-Briefcase v1.1** | 1,657 | 1,546 | 1,487 | 1,678 | 
| **Terminal-Bench 4.0** | 38.0% | 20.3% | 37.3% | 57.9% | 
| **Harvey Legal Agent Benchmark** | 19.6% | 15.8% | 2.5% | 6.7% | 
| **HealthBench Professional** | 56.7% | 48.5% | 60.5% | 62.1% | 
| **GDPval** | 1,695 Elo | 1,605 Elo | â | 1,735 Elo | 

On SpaceXAIâs table, Grok 4.7 leads GPT-5.6 Sol on five of seven shared rows and trails it on DeepSWE v1.1 and HealthBench Professional. Claude Fable 5.1 leads it on CursorBench, Terminal-Bench 4.0, HealthBench, AA-Briefcase and GDPval; Grok 4.7 leads Fable 5.1 on DeepSWE, EEBench and Harvey. SpaceXAIâs Grok 4.6 figures in this table differ slightly from its August launch table (DeepSWE 65.2% here, 65.9% then). These are vendor numbers.

### The independent read (Artificial Analysis)

| At launch (21 Sep 2026) | Grok 4.7 | Change from Grok 4.6 | 
|---|---|---|
| **Intelligence Index v4.3** | 46 (xhigh) | +2 (44) | 
| **Coding Agent Index** (with Grok Build) | 56 | +9 | 
| **Terminal-Bench 4.0** | 33% | from 18% | 
| **GDPval-AA** | 1695 Elo | +90 | 
| **Hallucination rate** | 29% | from 34% | 
| **Output tokens per index task** | ~81,000 | from ~36,000 | 
| **Cost per index task** | $3.74 (xhigh), $2.73 (high) | from $1.86 | 
| **Output speed** | ~188 tokens/s on long prompts | â | 

Source: Artificial Analysis and VentureBeat. At launch, Artificial Analysis reported AA-Briefcase as the largest gain (+111 Elo), placing Grok 4.7 behind only Claude Opus 5 and Claude Fable 5.1 on that measure. No SWE-bench Pro or Verified figure exists for Grok 4.7 in any harness.

### Safety claims

SpaceXAI describes Grok 4.7 as âthe strongest model weâve tested on refusals and jailbreak resistanceâ, reports 62.4% on the LatchBio biosafety benchmark and a 3.3% refusal rate on risky dual-use prompts in HackerBench v0.3, and says it built âan entirely new safeguard stackâ. These are company-reported; no separate system card was published at launch.

## Pricing breakdown

| Mode | Input (per MTok) | Output (per MTok) | 
|---|---|---|
| **Standard (â¤200K input)** | $2.00 | $6.00 | 
| **Long context (>200K input)** | $4.00 | $12.00 | 
| **Grok 4.7 Fast** (Cursor, Grok Build only) | $4.00 | $12.00 | 
| **US regional endpoint** | +10% | +10% | 

### Cost per completed task

| Model | Price (in/out) | AA index (v4.3) | Cost per index task | 
|---|---|---|---|
| **Grok 4.7 (high)** | $2 / $6 | â | $2.73 | 
| **Grok 4.7 (xhigh)** | $2 / $6 | 46 | $3.74 | 
| Grok 4.6 | $2 / $6 | 44 | $1.86 | 
| GPT-5.6 Sol | $4 / $20 | 47 | $1.99 | 
| GLM-5.3 | $1.40 / $4.40 | 45 | $2.01 | 
| GPT-6 Astra | $10 / $50 | 53 | $3.26 | 

Per completed task, Grok 4.7 at xhigh costs more than GPT-6 Astra, and Grok 4.6 remains the cheaper Grok.

## How to access Grok 4.7

### Via API

Grok 4.7 is generally available as `grok-4.7` on SpaceXAIâs API (console.x.ai), with a US regional endpoint at a 10% premium, and through OpenRouter, Vercel and Cloudflare.

```
from openai import OpenAI
client = OpenAI(base_url="https://api.x.ai/v1", api_key="XAI_API_KEY")
resp = client.chat.completions.create(
    model="grok-4.7",
    messages=[{"role": "user", "content": "Your prompt here"}],
)
print(resp.choices[0].message.content)
```
### Via Cursor and Grok Build

Grok 4.7 launched in Cursor and Grok Build, including Grok Buildâs free tier (standard variant only). SpaceXAI did not state which Grok app subscription tiers include it at launch; see the Grok app page and the xAI provider page.

## How Grok 4.7 compares

### vs Grok 4.6

At launch, Grok 4.7 scored two points higher on the Artificial Analysis index (46 to 44), nine points higher on its Coding Agent Index, and led Grok 4.6 on every row of SpaceXAIâs table, at the same per-token price. It uses about 2.25 times as many output tokens per index task, so a completed task costs $2.73 at high effort against Grok 4.6âs $1.86.

### vs GPT-5.6 Sol

When Grok 4.7 launched, GPT-5.6 Sol was one point higher on the index (47 to 46), and it publishes a SWE-bench Pro figure (64.6%, vendor) where Grok 4.7 has none. On SpaceXAIâs table Grok 4.7 leads Sol on CursorBench 4.0, Terminal-Bench 4.0, EEBench, AA-Briefcase and Harvey, and trails on DeepSWE and HealthBench. Sol costs less per completed index task ($1.99) despite its higher per-token price.

### vs Claude Fable 5.1

When Grok 4.7 launched, Claude Fable 5.1 scored 53 on the index, and it leads Grok 4.7 on Terminal-Bench 4.0 by about 20 points on SpaceXAIâs own table (57.9% to 38.0%). Fable 5.1 costs $10/$50 per million tokens and $7.63 per index task.

## Known limitations

**No SWE-bench figure.** Neither SpaceXAI nor any standardised harness has published a SWE-bench Pro or Verified result.

**Higher token use.** About 81,000 output tokens per Artificial Analysis index task, 125% more than Grok 4.6 and 196% more than GPT-6 Astra, which raises cost per task despite the unchanged price.

**Long-context calls double the price.** Prompts above 200K input tokens are billed at $4/$12.

**Fast variant is not on the API.** It is limited to Cursor and Grok Build.

**Consumer and EU availability not stated.** SpaceXAIâs launch material does not list Grok app subscription tiers or EU availability.

## Version history

| Version | Released | Key changes | 
|---|---|---|
| **Grok 4.7** | 21 Sep 2026 | New, larger base model; longer RL run; AA index 46 (v4.3); xhigh effort; same $2/$6 price | 
| **Grok 4.6** | 12 Aug 2026 | Post-training release for long-running agents; AA index 61 at its August launch, 44 on v4.3 | 
| **Grok 4.5** | 8 Jul 2026 | Coding and agentic focus, jointly trained with Cursor, 500K context, $2/$6 | 
| **Grok 4.3** | Apr 2026 | Native video input, 1M context, $1.25/$2.50 | 

Musk announced Grok 4.8, a model of about 2.5 trillion parameters, on 13 September 2026; it has no release date.

## Frequently asked questions

### When was Grok 4.7 released?

Grok 4.7 was released on 21 September 2026 on the SpaceXAI API, in Cursor and in Grok Build, with gateway access through OpenRouter, Vercel and Cloudflare. It had been expected around 12 September; Musk delayed it on 11 September.

### How much does Grok 4.7 cost?

$2.00 per million input tokens and $6.00 per million output tokens, the same as Grok 4.6. Prompts above 200,000 input tokens are billed at doubled rates, the US regional endpoint costs 10% more, and the Grok 4.7 Fast variant, available only in Cursor and Grok Build, costs twice the standard rates. Artificial Analysis measured $2.73 per completed Intelligence Index task at high effort and $3.74 at xhigh.

### How does Grok 4.7 score on benchmarks?

At launch, Artificial Analysis scored it 46 on its Intelligence Index v4.3 at xhigh effort, against 44 for Grok 4.6, 47 for GPT-5.6 Sol and 53 for Claude Fable 5.1 and GPT-6 Astra at the time. It measured 56 on the Coding Agent Index with Grok Build and 33% on Terminal-Bench 4.0. SpaceXAIâs own table reports 71.0% on DeepSWE v1.1 and 38.0% on Terminal-Bench 4.0. No SWE-bench figure has been published.

### What is Grok 4.7âs context window?

500,000 tokens, with text and image input and text output, and a May 2026 knowledge cutoff.

### How is Grok 4.7 different from Grok 4.6?

Grok 4.7 uses a new, larger base model and a longer reinforcement learning run; Grok 4.6 was a post-training release on Grok 4.5âs base. Grok 4.7 scores higher on every published benchmark at the same per-token price, but uses about 2.25 times as many output tokens per task, so its cost per completed task is higher ($2.73 at high against $1.86).

*Written at launch, 22 September 2026. Artificial Analysis figures are independent; benchmark table rows are SpaceXAIâs. Pricing and availability reflect the 21 September 2026 launch documentation.*
