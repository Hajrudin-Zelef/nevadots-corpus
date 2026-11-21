---
id: collect-261121-veille-ia/kimi/kimi-k3-1-pro-benchmarks-pricing-and-how-to-access-it
title: "Kimi K3.1 Pro: Benchmarks, Pricing, and How to Access It"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["kimi", "moonshot", "model", "coding", "benchmarks", "open-weight", "pricing", "rather"]
source: https://www.datacamp.com/blog/kimi-k3-1-pro
source_anchor: ""
source_lines: [1, 291]
section: "Kimi"
---
Open-weight labs have spent 2026 closing the gap to Anthropic and OpenAI on coding, and Moonshot AI just narrowed it again. Last Friday, Moonshot released Kimi K3.1 Pro, which it calls its most capable 3T-class open-weight model to date, and puts it within a point of Claude Opus 5.5 on agentic coding.

K3.1 Pro is a heavily post-trained version of Kimi K3 with a 1-million-token context window. It ships alongside a faster variant, K3.1 Pro Highspeed, and is live today in the Kimi apps, Kimi Code, and the Kimi API. The full weights follow by November 4, 2026.

In this article, I'll cover everything new with Kimi K3.1 Pro, looking at the new features, the benchmarks, and how the pricing compares with Kimi K3. To catch up on the model it builds on, check out our guides to Kimi K3 and our Kimi K3 hands-on tutorial.

## TL;DR

- **Kimi K3.1 Pro is Moonshot's new flagship** , a post-trained upgrade of Kimi K3 with the same 1M-token context.
- **Agentic coding is the headline** : it trades blows with Claude Opus 5.5 and beats GPT-6 Astra on terminal work.
- **Pure reasoning and knowledge benchmarks still trail** the closed frontier models by a clear margin.
- **Moonshot stripped cyber-related training data** before the open-weight release, on purpose.
- **The API costs less than Kimi K3 did** , so there is no price reason to stay on the old model.
- **Use standard K3.1 Pro by default** ; pay for Highspeed only when latency matters more than cost.

## What Is Kimi K3.1 Pro?

Kimi K3.1 Pro is Moonshot AI's flagship large language model (LLM) and the first model in its new K3.1 series. It keeps the base of Kimi K3, a 2.8T-parameter model with a 1M-token context window, and adds far more post-training compute on top.

The change is in the training, rather than in the architecture: Moonshot ran joint reinforcement learning (RL) across what it calls "hundreds of downstream domains", and rebuilt its RL framework, sandbox infrastructure, and rollout scheduling so that extra compute produced steady gains. The result, according to Moonshot, is improvement in every domain it measured, with coding, 3D creation, and financial research singled out.

The headline claim is agentic coding parity with the best proprietary models. Moonshot's own table puts K3.1 Pro within a point of Claude Opus 5.5 on terminal-based coding tasks, while charging a fraction of Anthropic's price. The model is open-weight, but the weights aren't public yet.

## Kimi K3.1 Pro Key Features

K3.1 Pro is a broad upgrade rather than a single new trick. The following four capabilities are the ones I'd actually change a workflow for.

### Run long, multi-step coding jobs

K3.1 Pro can stay on a long, multi-step coding task, diagnosing its own failures rather than just retrying. Moonshot's showcase is a human walking simulator built on the open-source human-atlas anatomical model, with 296 skeletal and 402 muscle components.

Across 167 missions and 420 independent reviews, the model took the simulated human from collapsing within seconds to walking with alternating strides for 11.2 seconds. The interesting part came when reward shaping stopped helping the arm motion: K3.1 Pro ran open-loop probes of the simulator, found 36 errors in the shoulder-muscle moment-arm calculations, fixed them, and retrained.

That is one internal demo, so treat it as an illustration of the intended use case rather than evidence. Still, it shows what Moonshot is optimizing for: agents that investigate and reiterate.

### Build games, front ends, and 3D assets with more polish

Moonshot says K3.1 Pro produces front-end pages, Three.js scenes, and 3D assets with "stronger composition, typography, animation, and polish." It works with Unity, Unreal Engine, and Godot, and can combine them with web search and 3D generation to assemble a complete project.

On the modeling side, K3.1 Pro writes and runs scripts for Blender and CAD software directly, without MCP connectors, to build parametric models such as mechanical parts, architectural models, and configurable furniture. Moonshot also launched 3D Make on kimi.ai and the Kimi app, where you can preview, edit, and download models ready for 3D printing.

The visual-quality claim is also the hardest one to check against a benchmark table, so treat Moonshot's demos as the vendor's best examples rather than typical output.

### Do financial modeling and research with fewer made-up facts

K3.1 Pro can carry a financial workflow from screening to a finished workbook, and Moonshot made targeted fixes to factual accuracy for this kind of work. On its internal finance benchmarks, K3.1 Pro improves on Kimi K3 by 7.2 points on Financial Modeling and 11.6 points on Financial Research, and reaches about 4.2× K3's score on Business Operation, a long-horizon simulated business task.

The accuracy work shows up in Moonshot's hallucination stress test. On Kimi Hallu Bench Hard, the claim-level hallucination rate fell from 23.4% for K3 to 8.3% for K3.1 Pro. Moonshot is careful to say the test is built to provoke errors, so those rates are not everyday error rates.

Kimi K3.1 Pro is competitive with Claude Opus 5.5 and GPT-6 Astra on agentic coding and tool-heavy work, but clearly behind them on pure reasoning. All figures below come from Moonshot's announcement, with K3.1 Pro at `max` effort.

### Agentic coding

Agentic coding is where K3.1 Pro looks strongest. On Terminal-Bench 4.0, which grades coding agents on realistic, resource-constrained terminal tasks, K3.1 Pro scores 65.7%. That's only very narrowly behind Claude Opus 5.5 at 66.4% and well ahead of GPT-6 Astra at 58.2%.

The rest of the coding results are more mixed than that headline suggests. All scores are vendor-reported, with the best score in each row in bold:

| Benchmark | What it measures | Kimi K3.1 Pro | Claude Opus 5.5 | GPT-6 Astra | DeepSeek-V4.1-Flash-1017 | 
|---|---|---|---|---|---|
| Terminal-Bench 4.0 | Coding agents on realistic terminal tasks | 65.7% | **66.4%** | 58.2% | 42.3% | 
| SWE-Marathon v1.1 | 20 multi-hour engineering tasks, no internet | **51.7%** | 46.3% | 42.5% | 40.0% | 
| DeepSWE v1.1 | 113 long-horizon tasks, graded on behavior | 75.5% | 74.2% | 74.1% | **77.0%** | 
| Kimi Code Bench 3.0 Long-horizon (Moonshot internal) | Multi-step execution and tool use | 61.2% | 58.5% | **66.5%** | 51.5% | 
| ProgramBench | Rebuild a program from its binary and spec (tasks passing at least 95% of tests) | 31.5% | **65.0%** | 50.0% | 29.1% | 
| Terminal-Bench Science 0.1 | Agents on scientific research workflows | 38.1% | 63.3% | **68.1%** | 15.7% | 

K3.1 Pro is level with the best closed models on long engineering tasks and leads Moonshot's SWE-Marathon comparison. On DeepSWE, every lab ran its own setup, so a 3-point spread is a tie rather than a win. It falls well behind when it has to rebuild a program purely from its observed behavior, or work through a scientific research workflow.

Against its predecessor, the clearest gain is on Moonshot's own long-horizon benchmark, where Kimi K3 scored 55.95%.

On tool-heavy agent work, K3.1 Pro leads or sits within a couple of points of Opus 5.5:

| Benchmark | What it measures | Kimi K3.1 Pro | Claude Opus 5.5 | GPT-6 Astra | DeepSeek-V4.1-Flash-1017 | 
|---|---|---|---|---|---|
| HLE-Full with tools | 2,500 hard knowledge and reasoning questions, tools allowed | **70.0%** | 67.7% | 57.2% | 58.8% | 
| BrowseComp | Finding hard-to-locate information on the web | **92.1%** | 90.8% | 91.5% | n/a | 
| OfficeQA Pro v2 | Numerical analysis across real financial PDFs | **64.4** | 62.2 | 54.4 | 40.4 | 
| Frontier Finance | Investment workflows from screening to profitability analysis | **58.6** | 56.7 | 45.0 | 54.5 | 
| OSWorld V2.1 | Operating a desktop computer to finish tasks | 79.6% | **81.8%** | 75.8% | 69.3% | 

The one clear exception in Moonshot's table is computer use, where Opus 5.5 stays ahead. On Frontier Finance, Claude Fable 5.1 (60.8) is the overall leader, ahead of all four models shown here.

### Reasoning and knowledge

Without tools, K3.1 Pro falls clearly behind the closed models:

| Benchmark | What it measures | Kimi K3.1 Pro | Claude Opus 5.5 | GPT-6 Astra | DeepSeek-V4.1-Flash-1017 | 
|---|---|---|---|---|---|
| HLE-Full | 2,500 hard knowledge and reasoning questions | 51.1% | **59.8%** | 53.6% | 42.6% | 
| HLE-Diamond | 1,000-question hardest subset of HLE | 39.4% | 60.9% | **65.8%** | 21.6% | 
| GPQA-Diamond | Graduate-level science questions | 94.2% | 93.9% | **96.1%** | 92.2% | 
| MMMU-Pro | Questions combining images and text | 84.1% | **87.7%** | 86.9% | 80.6% | 
| Kimi Hallu Bench (Moonshot internal, lower is better) | Share of factual claims that are wrong | 5.4% | 6.8% | **4.2%** | 13.1% | 

The gap is widest on HLE-Diamond, while GPQA-Diamond is close to saturated, so its 2-point spread says little. If your work is hard closed-book reasoning, this isn't the model to switch to.

### Cybersecurity, by design

Moonshot did something unusual for an open-weight release: it published how much capability it deliberately removed. It found that cyber-related content in its mid-training data and RL environments was steadily raising vulnerability research skill, so it removed that data from both stages for the open-weight release.

On xdev-bench-hard, Moonshot's internal exploit-development benchmark, a checkpoint without the data removal solved 90.5% of problems (pass@3). The shipped K3.1 Pro solves 81.0%, which is still far above Kimi K3's 33.3%. On public cyber benchmarks it sits below every closed model Moonshot compared, scoring 74.7 on ExploitBench against Opus 5.5's 91.0.

Moonshot's stated principle is to avoid releasing open weights that push the absolute frontier of offensive cyber capability, while raising the open-weight bar gradually. Whether a 9.5-point haircut is enough is a judgment call, but publishing the number is more than many other labs do.

## Which Tier Should You Use?

Standard K3.1 Pro is the right default for almost everyone. The Highspeed variant only earns its higher price when a person is sitting in front of the output, waiting.

Moonshot sells Highspeed purely on speed: it runs 2–3× faster, costs twice as much per input and output token, and has no separate benchmark results.

Both variants always think before answering, and you can't switch that off. You set how hard with three reasoning levels: `max` for the hardest agent work, `high` as a sensible middle, and `low` when the default thinking time is more than the task needs. The API defaults to `max`, the setting behind every score in Moonshot's benchmark table, so drop it to `high` or `low` unless the task needs full depth.

| Use case | Variant | Reasoning level | Why | 
|---|---|---|---|
| Long agent runs, hard debugging | K3.1 Pro | `max` | The setting behind Moonshot's coding scores; speed matters less over hours | 
| Everyday coding in Kimi Code | K3.1 Pro | `high` | Most of the quality at a lower token bill | 
| Interactive chat and quick edits | K3.1 Pro Highspeed | `low` | Latency is the bottleneck, and short prompts keep the 2× price small | 
| High-volume batch jobs | K3.1 Pro | `low` | Nobody is waiting, so pay the lower rate and cap the thinking | 

**One important practical note from the Kimi Code docs:** switching reasoning level mid-session invalidates the context cache, so pick a level per session rather than flipping back and forth.

## Kimi K3.1 Pro Pricing and Availability

Kimi K3.1 Pro costs $2.00 per million input tokens and $10.00 per million output tokens on the Kimi API, which is less than Kimi K3. Pricing is flat across the full 1M-token context, with no long-context surcharge.

| Model | Input | Output | Cached input | 5-minute cache write | 1-hour cache write | 
|---|---|---|---|---|---|
| Kimi K3.1 Pro | $2.00 | $10.00 | $0.20 | $2.00 | $4.00 | 
| Kimi K3.1 Pro Highspeed | $4.00 | $20.00 | $0.20 | $4.00 | $8.00 | 
| Kimi K3 (previous generation) | $3.00 | $15.00 | $0.30 | $3.00 | $6.00 | 

Prices are per million tokens and exclude tax. Caching is automatic with a 5-minute default, and you can request the 1-hour tier per call.

A few access details are worth knowing before you plan around it:

- **API gating:** K3.1 Pro unlocks after a minimum $1 top-up, and your cumulative top-up sets your rate limits.
- **Data retention:** Zero Data Retention is available for API customers.
- **Apps:** it is rolling out on Kimi.ai, the iOS, Android, and HarmonyOS apps, and the Kimi Work desktop app from version 3.1.0.
- **Enterprise:** Kimi Enterprise administrators have to switch K3.1 Pro on for their organization; it is off by default at launch.

If you're still on K2-series models, Moonshot's docs say they'll be unavailable to new users from October 31, 2026, and retired by the end of December 2026.

## How to Get Access to Kimi K3.1 Pro?

You can use Kimi K3.1 Pro today through the Kimi API, OpenRouter, and Kimi's own apps, but not as downloadable weights yet. Moonshot says the full weights will be released by November 4, 2026, and its announcement doesn't mention license terms, so check the model card when the weights land.

- **Kimi API:** the model IDs are`kimi-k3.1-pro` and`kimi-k3.1-pro-highspeed` , on an OpenAI-compatible endpoint.
- **OpenRouter:** listed as`moonshotai/kimi-k3.1-pro` .
- **Coding agents:** in Kimi Code, select it with the`/model` command. Moonshot's docs also cover connecting it to Codex through the Kimi Responses API.
- **Not listed yet:** as of October 26, 2026, it isn't listed in the GitHub Copilot, Azure AI Foundry, or Ollama docs, and the only Hugging Face hits are community uploads, not Moonshot's weights.

A minimal call with the OpenAI Python client looks like this:

```
from openai import OpenAI
client = OpenAI(api_key="YOUR_MOONSHOT_API_KEY", base_url="https://api.moonshot.ai/v1")
response = client.chat.completions.create(
    model="kimi-k3.1-pro",
    reasoning_effort="high",
    messages=[{"role": "user", "content": "Find the bug in this function..."}],
)
print(response.choices[0].message.content)
```

Once the weights land, our guide on how to run LLMs locally covers the serving options, and our explainer on OpenCode shows how to plug a model like this into an open-source coding agent.

## Final Thoughts

Kimi K3.1 Pro is the strongest case yet that open-weight models can match the closed frontier on agentic coding, at a price below its own predecessor. It isn't a general reasoning leader, but for long coding and tool-heavy agent work it sits right next to Claude Opus 5.5 and ahead of GPT-6 Astra on several tasks.

Moonshot is also betting that publishing its cyber trade-offs will buy goodwill for its open releases. If you code with agents, I'd try it now through the API and revisit self-hosting once the weights and model card land in November.

If you're keen to work with open models like this one, I recommend checking out our Hugging Face Fundamentals skill track.

Open-weight labs have spent 2026 closing the gap to Anthropic and OpenAI on coding, and Moonshot AI just narrowed it again. Last Friday, Moonshot released Kimi K3.1 Pro, which it calls its most capable 3T-class open-weight model to date, and puts it within a point of Claude Opus 5.5 on agentic coding.

K3.1 Pro is a heavily post-trained version of Kimi K3 with a 1-million-token context window. It ships alongside a faster variant, K3.1 Pro Highspeed, and is live today in the Kimi apps, Kimi Code, and the Kimi API. The full weights follow by November 4, 2026.

In this article, I'll cover everything new with Kimi K3.1 Pro, looking at the new features, the benchmarks, and how the pricing compares with Kimi K3. To catch up on the model it builds on, check out our guides to Kimi K3 and our Kimi K3 hands-on tutorial.

## TL;DR

Kimi K3.1 Pro is Moonshot's new flagship, a post-trained upgrade of Kimi K3 with the same 1M-token context.Agentic coding is the headline: it trades blows with Claude Opus 5.5 and beats GPT-6 Astra on terminal work.Pure reasoning and knowledge benchmarks still trailthe closed frontier models by a clear margin.Moonshot stripped cyber-related training databefore the open-weight release, on purpose.The API costs less than Kimi K3 did, so there is no price reason to stay on the old model.Use standard K3.1 Pro by default; pay for Highspeed only when latency matters more than cost.
## What Is Kimi K3.1 Pro?

Kimi K3.1 Pro is Moonshot AI's flagship large language model (LLM) and the first model in its new K3.1 series. It keeps the base of Kimi K3, a 2.8T-parameter model with a 1M-token context window, and adds far more post-training compute on top.

The change is in the training, rather than in the architecture: Moonshot ran joint reinforcement learning (RL) across what it calls "hundreds of downstream domains", and rebuilt its RL framework, sandbox infrastructure, and rollout scheduling so that extra compute produced steady gains. The result, according to Moonshot, is improvement in every domain it measured, with coding, 3D creation, and financial research singled out.

The headline claim is agentic coding parity with the best proprietary models. Moonshot's own table puts K3.1 Pro within a point of Claude Opus 5.5 on terminal-based coding tasks, while charging a fraction of Anthropic's price. The model is open-weight, but the weights aren't public yet.

## Kimi K3.1 Pro Key Features

K3.1 Pro is a broad upgrade rather than a single new trick. The following four capabilities are the ones I'd actually change a workflow for.

## Run long, multi-step coding jobs

K3.1 Pro can stay on a long, multi-step coding task, diagnosing its own failures rather than just retrying. Moonshot's showcase is a human walking simulator built on the open-source human-atlas anatomical model, with 296 skeletal and 402 muscle components.

Across 167 missions and 420 independent reviews, the model took the simulated human from collapsing within seconds to walking with alternating strides for 11.2 seconds. The interesting part came when reward shaping stopped helping the arm motion: K3.1 Pro ran open-loop probes of the simulator, found 36 errors in the shoulder-muscle moment-arm calculations, fixed them, and retrained.

That is one internal demo, so treat it as an illustration of the intended use case rather than evidence. Still, it shows what Moonshot is optimizing for: agents that investigate and reiterate.

## Build games, front ends, and 3D assets with more polish

Moonshot says K3.1 Pro produces front-end pages, Three.js scenes, and 3D assets with "stronger composition, typography, animation, and polish." It works with Unity, Unreal Engine, and Godot, and can combine them with web search and 3D generation to assemble a complete project.

On the modeling side, K3.1 Pro writes and runs scripts for Blender and CAD software directly, without MCP connectors, to build parametric models such as mechanical parts, architectural models, and configurable furniture. Moonshot also launched 3D Make on kimi.ai and the Kimi app, where you can preview, edit, and download models ready for 3D printing.

The visual-quality claim is also the hardest one to check against a benchmark table, so treat Moonshot's demos as the vendor's best examples rather than typical output.

## Do financial modeling and research with fewer made-up facts

K3.1 Pro can carry a financial workflow from screening to a finished workbook, and Moonshot made targeted fixes to factual accuracy for this kind of work. On its internal finance benchmarks, K3.1 Pro improves on Kimi K3 by 7.2 points on Financial Modeling and 11.6 points on Financial Research, and reaches about 4.2× K3's score on Business Operation, a long-horizon simulated business task.

The accuracy work shows up in Moonshot's hallucination stress test. On Kimi Hallu Bench Hard, the claim-level hallucination rate fell from 23.4% for K3 to 8.3% for K3.1 Pro. Moonshot is careful to say the test is built to provoke errors, so those rates are not everyday error rates.

## How Does Kimi K3.1 Pro Perform on the Benchmarks?

Kimi K3.1 Pro is competitive with Claude Opus 5.5 and GPT-6 Astra on agentic coding and tool-heavy work, but clearly behind them on pure reasoning. All figures below come from Moonshot's announcement, with K3.1 Pro at

`max`effort.
## Agentic coding

Agentic coding is where K3.1 Pro looks strongest. On Terminal-Bench 4.0, which grades coding agents on realistic, resource-constrained terminal tasks, K3.1 Pro scores 65.7%. That's only very narrowly behind Claude Opus 5.5 at 66.4% and well ahead of GPT-6 Astra at 58.2%.

The rest of the coding results are more mixed than that headline suggests. All scores are vendor-reported, with the best score in each row in bold:

66.4%51.7%77.0%66.5%65.0%68.1%
K3.1 Pro is level with the best closed models on long engineering tasks and leads Moonshot's SWE-Marathon comparison. On DeepSWE, every lab ran its own setup, so a 3-point spread is a tie rather than a win. It falls well behind when it has to rebuild a program purely from its observed behavior, or work through a scientific research workflow.

Against its predecessor, the clearest gain is on Moonshot's own long-horizon benchmark, where Kimi K3 scored 55.95%.

## Tool use, office work, and browsing

On tool-heavy agent work, K3.1 Pro leads or sits within a couple of points of Opus 5.5:

70.0%92.1%64.458.681.8%
The one clear exception in Moonshot's table is computer use, where Opus 5.5 stays ahead. On Frontier Finance, Claude Fable 5.1 (60.8) is the overall leader, ahead of all four models shown here.

## Reasoning and knowledge

Without tools, K3.1 Pro falls clearly behind the closed models:

59.8%65.8%96.1%87.7%4.2%
The gap is widest on HLE-Diamond, while GPQA-Diamond is close to saturated, so its 2-point spread says little. If your work is hard closed-book reasoning, this isn't the model to switch to.

## Cybersecurity, by design

Moonshot did something unusual for an open-weight release: it published how much capability it deliberately removed. It found that cyber-related content in its mid-training data and RL environments was steadily raising vulnerability research skill, so it removed that data from both stages for the open-weight release.

On xdev-bench-hard, Moonshot's internal exploit-development benchmark, a checkpoint without the data removal solved 90.5% of problems (pass@3). The shipped K3.1 Pro solves 81.0%, which is still far above Kimi K3's 33.3%. On public cyber benchmarks it sits below every closed model Moonshot compared, scoring 74.7 on ExploitBench against Opus 5.5's 91.0.

Moonshot's stated principle is to avoid releasing open weights that push the absolute frontier of offensive cyber capability, while raising the open-weight bar gradually. Whether a 9.5-point haircut is enough is a judgment call, but publishing the number is more than many other labs do.

## Which Tier Should You Use?

Standard K3.1 Pro is the right default for almost everyone. The Highspeed variant only earns its higher price when a person is sitting in front of the output, waiting.

Moonshot sells Highspeed purely on speed: it runs 2–3× faster, costs twice as much per input and output token, and has no separate benchmark results.

Both variants always think before answering, and you can't switch that off. You set how hard with three reasoning levels:

`max`for the hardest agent work,`high`as a sensible middle, and`low`when the default thinking time is more than the task needs. The API defaults to`max`, the setting behind every score in Moonshot's benchmark table, so drop it to`high`or`low`unless the task needs full depth.`max``high``low``low`One important practical note from the Kimi Code docs:switching reasoning level mid-session invalidates the context cache, so pick a level per session rather than flipping back and forth.
## Kimi K3.1 Pro Pricing and Availability

Kimi K3.1 Pro costs $2.00 per million input tokens and $10.00 per million output tokens on the Kimi API, which is less than Kimi K3. Pricing is flat across the full 1M-token context, with no long-context surcharge.

Prices are per million tokens and exclude tax. Caching is automatic with a 5-minute default, and you can request the 1-hour tier per call.

A few access details are worth knowing before you plan around it:

API gating:K3.1 Pro unlocks after a minimum $1 top-up, and your cumulative top-up sets your rate limits.Data retention:Zero Data Retention is available for API customers.Apps:it is rolling out on Kimi.ai, the iOS, Android, and HarmonyOS apps, and the Kimi Work desktop app from version 3.1.0.Enterprise:Kimi Enterprise administrators have to switch K3.1 Pro on for their organization; it is off by default at launch.
If you're still on K2-series models, Moonshot's docs say they'll be unavailable to new users from October 31, 2026, and retired by the end of December 2026.

## How to Get Access to Kimi K3.1 Pro?

You can use Kimi K3.1 Pro today through the Kimi API, OpenRouter, and Kimi's own apps, but not as downloadable weights yet. Moonshot says the full weights will be released by November 4, 2026, and its announcement doesn't mention license terms, so check the model card when the weights land.

Kimi API:the model IDs are`kimi-k3.1-pro`and`kimi-k3.1-pro-highspeed`, on an OpenAI-compatible endpoint.OpenRouter:listed as`moonshotai/kimi-k3.1-pro`.Coding agents:in Kimi Code, select it with the`/model`command. Moonshot's docs also cover connecting it to Codex through the Kimi Responses API.Not listed yet:as of October 26, 2026, it isn't listed in the GitHub Copilot, Azure AI Foundry, or Ollama docs, and the only Hugging Face hits are community uploads, not Moonshot's weights.
A minimal call with the OpenAI Python client looks like this:

Once the weights land, our guide on how to run LLMs locally covers the serving options, and our explainer on OpenCode shows how to plug a model like this into an open-source coding agent.

## Final Thoughts

Kimi K3.1 Pro is the strongest case yet that open-weight models can match the closed frontier on agentic coding, at a price below its own predecessor. It isn't a general reasoning leader, but for long coding and tool-heavy agent work it sits right next to Claude Opus 5.5 and ahead of GPT-6 Astra on several tasks.

Moonshot is also betting that publishing its cyber trade-offs will buy goodwill for its open releases. If you code with agents, I'd try it now through the API and revisit self-hosting once the weights and model card land in November.

If you're keen to work with open models like this one, I recommend checking out our Hugging Face Fundamentals skill track.
