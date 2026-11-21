---
id: collect-261121-veille-ia/kimi/minimax-m3-1-flash-vs-deepseek-v4-1-flash-vs-kimi-k3-the-cheap-coding-model-war-is-on
title: "MiniMax M3.1 Flash vs DeepSeek V4.1 Flash vs Kimi K3 : The Cheap Coding Model War Is On"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["flash", "minimax", "deepseek", "kimi", "model", "tokens", "opus", "three"]
source: https://medium.com/data-science-in-your-pocket/minimax-m3-1-flash-vs-deepseek-v4-1-flash-vs-kimi-k3-the-cheap-coding-model-war-is-on-d7ab8ab17a3e
source_anchor: ""
source_lines: [1, 97]
section: "Kimi"
---
# MiniMax M3.1 Flash vs DeepSeek V4.1 Flash vs Kimi K3 : The Cheap Coding Model War Is On

*Three Chinese labs, three wildly different philosophies, and one uncomfortable question for Anthropic and OpenAI: why pay $10 per agent task when 20 cents gets you most of the way?*

## The Week the Flash Tier Exploded

The wait is over, and it happened fast. On October 17, DeepSeek quietly swapped V4.1-Flash-1017 into its API, apps, and web chat at the same price as before, calling it a Pareto improvement: better scores, faster completion, shorter outputs. Two days later, MiniMax dropped M3.1 Flash and revealed it was the anonymous “Space Bunny” model that had been topping OpenRouter’s usage charts, chewing through more than 200 trillion tokens before anyone knew whose model it was.

And looming over both of them is Kimi K3, Moonshot’s 2.8-trillion-parameter monster from July, still the largest open-weight model ever released. Three models, three price points, and one shared claim: near-frontier agentic performance at a fraction of frontier cost.

So which one deserves your API key? Let’s break it down.

## Meet the Contenders

Before the benchmarks, the specs. These three could not be more different in how they’re built and sold.

*MiniMax M3.1 Flash is the scrappy newcomer:*

- Parameters: 456B total, 45.9B active per token (MoE)
- Price: $0.15 input / $0.60 output per 1M tokens, with cached input at just $0.015
- Speed: 200+ tokens/sec in everyday use, P50 above 170
- Access: MiniMax API, M Plan, MiniMax Code. Weights promised, reportedly landing around October 23
- The catch: license, exact context length, and architecture details still undisclosed

*DeepSeek V4.1-Flash-1017 is the open-weight darling:*

- Architecture: 552B backbone plus 196B Engram memory, only 8B active in prefill and 16B in decode
- Context: 1M tokens, up to 384K output
- Price: $0.30 input / $1.20 output per 1M tokens, unchanged from the September checkpoint
- License: MIT, weights live on Hugging Face and ModelScope right now
- Bonus: new Search and Fetch APIs at $0.75 per 1,000 calls, free inside agent harnesses until November 1

*Kimi K3 is the heavyweight:*

- Parameters: 2.8T total, 104B active, 93-layer MoE routing 16 of 896 experts per token
- Context: 1,048,576 tokens
- Price: $3 input / $15 output per 1M tokens ($0.30 cached). Yes, 20x MiniMax’s input price
- License: open weights under the Kimi K3 License, but Moonshot recommends 64+ accelerators to self-host
- Party trick: debuted at #1 on the Frontend Code Arena with a 1,679 score, ahead of Claude Fable 5

Read that pricing again. K3 costs 20 times more per input token than M3.1 Flash. The entire question of this blog is whether the benchmarks justify that gap.

## Benchmarks: Agentic Work (CoWork)

This is where MiniMax’s launch table (the one in the header image) gets interesting. MiniMax ran all three models against DeepSeek’s 1017 checkpoint, GPT-5.6 Sol, and Claude Opus 5.

- Automation-Bench v1.0.6: M3.1 Flash 62.2, DeepSeek 1017 60.5, K3 46.7. MiniMax takes the crown, and even Opus 5 (50.3) loses here.
- BankerToolBench: a dead heat, M3.1 Flash and Opus 5 tied at 76.2, DeepSeek’s older Flash at 73.7, K3 at 69.1.
- Terminal Bench 4.0: DeepSeek 1017 leads the Chinese trio at 42.3, M3.1 Flash 38.1, K3 a rough 12.6. Opus 5 still dominates at 51.8.
- OSWorld V2 (computer use): M3.1 Flash 61.7 edges K3’s 58.3, but GPT-5.6 Sol (65.7) and Opus 5 (70.6) lead overall. MiniMax claims first among Chinese models here.
- Agents’ Last Exam: Opus 5 (32.2) and GPT-5.6 Sol (30.6) top the table, M3.1 Flash 29.6, K3 28.3.

The pattern: MiniMax wins business-workflow automation, DeepSeek wins terminal work, and K3, for all its trillion parameters, is oddly weak in this category.

## Benchmarks: Coding

This is DeepSeek’s home turf, and the 1017 checkpoint shows it.

DeepSWE v1.1: DeepSeek 1017 scores 77.0, beating even Opus 5 (74.0). M3.1 Flash posts a strong 73.7 at max effort, K3 trails at 67.5.

NL2Repo-Bench: Opus 5 leads at 75.3, DeepSeek 1017 at 68.2, M3.1 Flash 61.2, K3 58.0.

SWE Refactor Bench: Opus 5 (31.0) leads, M3.1 Flash 23.0, K3 19.5, older DeepSeek Flash 11.8.

PostTrain Bench v1.1: Opus 5 40.4, M3.1 Flash 39.3, K3 36.6.


But here’s the number that matters more than any single score: cost per task. On DeepSWE, V4.1-Flash-1017 hits 77.0% at roughly $0.20 per task. On MiniMax’s own cost chart, M3.1 Flash lands around a quarter of a dollar per task. Frontier models like GPT-6 Astra and Opus 5 sit between $2 and over $10 per task on the same chart. That’s not a discount, it’s a different economic category.

## Benchmarks: Cyber and General Capability

Two quick categories before the verdict.

- CyberGym: GLM-5.3 actually leads at 84.5, with M3.1 Flash at 82.2, K3 at 80.0. MiniMax backs this with internal evals in code auditing, pentesting, and reverse engineering.
- BabyVision (with tools): Opus 5 94.1, DeepSeek 1017 92.5, M3.1 Flash 91.0, K3 85.7.
- HLE without tools: the frontier gap shows. Opus 5 runs away at 55.6, GPT-5.6 Sol 49.5, while the three Chinese models cluster tightly: K3 43.5, M3.1 Flash 43.4, DeepSeek 1017 42.6.
- CharXiv RQ: GPT-5.6 Sol leads at 85.2, K3 84.8, M3.1 Flash 82.5.

Raw reasoning without tools is still where the expensive models earn their price. None of the three Chinese contenders touches Opus 5 on HLE.

## The Fine Print

Three caveats before you rip up your routing config.

First, nearly every number above is vendor-reported. MiniMax’s table favors MiniMax, DeepSeek’s card favors DeepSeek, and harness choices (Claude Code vs mini-SWE-agent) can swing scores by several points. Second, M3.1 Flash is days old: no license, no disclosed context length, no independent evals yet. The weights drop will tell us a lot. Third, K3’s benchmark showing here undersells it in one specific area: frontend code. Its Arena #1 ranking came from blind human preference, and that’s the one eval no vendor can cherry-pick.

## Which Should You Pick?

My honest take after digging through both launch tables:

- Pick MiniMax M3.1 Flash if you run high-volume agent pipelines: tool routing, form filling, business workflow automation, first-pass code edits. At $0.15/$0.60 with 200+ tokens/sec, it’s the cheapest credible workhorse, and its Automation-Bench lead is real (within vendor-table caveats).
- Pick DeepSeek V4.1-Flash-1017 if you want the best coding-per-dollar with zero strings attached. MIT license, weights on Hugging Face today, DeepSWE 77.0 at $0.20 a task, and a first-party Search API thrown in. For terminal-heavy and repo-scale agent work, this is the default choice right now.
- Pick Kimi K3 if you’re doing frontend development, visual iteration, or long research sessions where quality per call matters more than cost per million. It’s the premium option wearing an open-weight badge, and the 64-GPU self-hosting requirement means most of us will only ever meet it through an API.

The real story isn’t which of these three wins. It’s that the frontier labs’ moat in agentic coding, the thing that justified $15 output tokens, just got a lot narrower. When a $0.15 model ties Opus 5 on BankerToolBench and a $0.30 model beats it on DeepSWE, the pricing conversation in every engineering standup changes.

Run your own evals this week. All three have free or near-free routes to try them. Your workloads are the only benchmark that counts.
