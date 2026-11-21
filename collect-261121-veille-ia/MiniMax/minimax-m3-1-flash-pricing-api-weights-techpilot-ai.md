---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-pricing-api-weights-techpilot-ai
title: "MiniMax M3.1 Flash: Pricing, API & Weights | TechPilot.ai"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: ["2026-10-22"]
keywords: ["minimax", "flash", "free", "tokens", "openrouter", "model", "pricing", "input"]
source: https://techpilot.ai/tools/minimax-m3-1-flash/
source_anchor: ""
source_lines: [1, 142]
section: "MiniMax"
---
MiniMax M3.1 Flash gives you near-frontier agent performance at $0.15 and $0.60 per million tokens, with no peak pricing.

## The AI news that actually mattered this week.

One short email every Friday: the biggest AI stories, what they mean for you, and the tools worth trying. A 3-minute read, no hype.

Free. Unsubscribe anytime.

 **You're in.**
Watch for the brief on Friday. Free. Unsubscribe anytime.

## Summary

## Pros and cons

### What we like

-  **Low price** $0.15 input and $0.60 output per 1M tokens.
-  **Flat rates** No peak-hour surcharge.
-  **Built for agents** Strong office automation, computer use and security scores.
-  **Big context** 1M tokens, with up to 524K output tokens on OpenRouter.
-  **Easy access** MiniMax API, OpenRouter and MiniMax Code.

### What could be better

-  **No free tier** The OpenRouter free model ends October 22, 2026.
-  **No weights yet** Open weights are promised but not released.
-  **Self-reported benchmarks** Most scores come from MiniMax.
-  **Not the best coder** DeepSeek's newest Flash leads on several coding tests.

## Key features

- Agent workflows: Multi-step automation across SaaS tools and documents.
- Coding: Understands real codebases, fixes bugs and builds features.
- Computer use: Operates apps and completes cross-application tasks.
- Security: Code auditing and vulnerability discovery.
- 1M context: Reads very large codebases and document sets.
- Fast output: MiniMax reports 170+ tokens a second at the median.

## Screenshots

## Full review

Independent and reader-supported. Some links may earn us a commission; it never changes our scores. Editorial policy

MiniMax M3.1 Flash is MiniMax’s low-cost agent and coding model at $0.15 and $0.60 per 1M tokens. See API pricing, OpenRouter setup, benchmarks, weights.

MiniMax released M3.1 Flash on October 19, 2026, and revealed that “Space Bunny”, the anonymous model that topped OpenRouter’s free charts in September, was an early version of it. It’s built for agents: multi-step office automation, coding in real codebases, computer use and security work. MiniMax lists it at $0.15 per million input tokens and $0.60 per million output tokens, and OpenRouter serves it at the same rates. OpenRouter’s free launch variant ends on October 22, 2026 at 12:00 UTC (19:00 UTC+7). Prices on this page were checked on the MiniMax launch post and OpenRouter on October 22, 2026.

## Is MiniMax M3.1 Flash free?

Not for long. OpenRouter ran a free variant (`minimax/minimax-m3.1-flash:free`) for 72 hours after launch, and that free period **ends Thursday, October 22, 2026 at 12:00 UTC**, which is 19:00 in Vietnam (UTC+7). Don’t plan on free access after that. After that the `:free` model is retired, and you need to switch to the paid slug `minimax/minimax-m3.1-flash` or your requests will fail.

From then on, both routes are paid: OpenRouter bills MiniMax’s list price, and MiniMax’s own API has no free tier. If any of your code still calls the `:free` slug, change it now.

## MiniMax M3.1 Flash Pricing (2026)

MiniMax charges one flat rate at every hour of the day:

| Token type | Price per 1M tokens | 
|---|---|
| **Input** | $0.15 | 
| **Output** | $0.60 | 
| **Cached input** | $0.015 | 

MiniMax says the overall cost of agent tasks is about 70% lower than with its previous model. For a real-world reference, Plotly ran its data analysis benchmark on the model and priced the whole 43-question exam at $4.09 at list prices, about 10 cents a question, because 98% of the input was cached.

Those are the same headline rates as DeepSeek V4.1 Flash off-peak. The difference is that DeepSeek doubles its price during weekday peak hours, while MiniMax doesn’t, and DeepSeek’s cached input is cheaper. We compare the two in detail in MiniMax M3.1 Flash vs DeepSeek V4.1 Flash.

## What is MiniMax M3.1 Flash good at?

MiniMax pitches it as a cheap model for long agent runs. According to its launch post:

- **Office automation:** 76.2 on BankerToolBench and a top result on AutomationBench, which tests end-to-end workflows across SaaS tools.
- **Coding:** 73.7% on DeepSWE v1.1, which tests building features and fixing bugs in real repositories.
- **Computer use:** 61.7 on OSWorld 2.0 and 29.6 on Agents’ Last Exam.
- **Security:** 82.2 on CyberGym for finding and validating vulnerabilities.
- **Speed:** MiniMax says output reaches 200+ tokens a second, with a median above 170.

These are MiniMax’s numbers. Its own table shows the newer DeepSeek V4.1 Flash (1017) ahead on DeepSWE, Terminal Bench 4.0 and repository-level coding, so M3.1 Flash isn’t the best at everything. Its strongest case is agent workflows at a predictable price.

## Specs

- **Context window:** 1M tokens; OpenRouter allows up to 524,288 output tokens.
- **Input:** multimodal, including images, according to OpenRouter.
- **Where to use it:** the MiniMax API (platform.minimax.io), MiniMax’s M Plan, the MiniMax Code desktop app, and OpenRouter.
- **Open weights:** promised, not released. MiniMax says the weights “will be released later”. They weren’t on MiniMax’s Hugging Face page when we checked on October 22, and the date reported for them is October 23. No licence has been published yet, so don’t plan on self-hosting until it is. Until then, see our best local AI models for models you can run today.

## How to use MiniMax M3.1 Flash

1. **Get an OpenRouter API key** and point any OpenAI-compatible client at https://openrouter.ai/api/v1.
2. **Set the model** to`minimax/minimax-m3.1-flash` , the paid slug, and add OpenRouter credit.
3. **Retire the free slug:**`minimax/minimax-m3.1-flash:free` stops working after October 22, 12:00 UTC (19:00 UTC+7).
4. **Or go direct:** create a key on platform.minimax.io to pay MiniMax’s rates without a middleman.
5. **For coding,** use it inside an agent tool that supports OpenRouter or custom OpenAI endpoints.

## Limitations

- **No lasting free tier:** the OpenRouter free variant ends on October 22 at 12:00 UTC.
- **Weights aren’t out:** open weights are promised, with no licence yet.
- **Self-reported scores:** most benchmark results come from MiniMax.
- **Not the top coder:** DeepSeek’s latest Flash beats it on several coding tests in MiniMax’s own table.

## Verdict

MiniMax M3.1 Flash is one of the cheapest strong agent models you can call today, with flat pricing that is easy to budget. Test it on your own prompts through OpenRouter or the MiniMax API, compare it with DeepSeek V4.1 Flash, which is free in OpenCode, and wait for the weights and licence before you plan to run it yourself.

## TechPilot.ai’s Verdict on MiniMax M3.1 Flash

MiniMax M3.1 Flash gives you near-frontier agent performance at $0.15 and $0.60 per million tokens, with no peak pricing.

Test it on OpenRouter or the MiniMax API, but wait for the promised weights and licence before you plan to self-host.

## MiniMax M3.1 Flash FAQ

## Is MiniMax M3.1 Flash free?

Not after October 22, 2026. OpenRouter’s free variant ends that day at 12:00 UTC (19:00 UTC+7) and is retired. After that you pay $0.15 and $0.60 per 1M tokens on OpenRouter or the MiniMax API.

•

## How much does MiniMax M3.1 Flash cost?

MiniMax charges $0.15 per 1M input tokens, $0.60 per 1M output tokens and $0.015 per 1M cached input tokens, at every hour. OpenRouter lists the same rates for the paid model.

•

## Are MiniMax M3.1 Flash weights open source?

Not yet. MiniMax says the weights will be released later, and October 23 has been reported as the date. No licence has been published, so check it before you use the weights commercially.

•

## What is the MiniMax M3.1 Flash context window?

It has a 1M-token context window. On OpenRouter it supports up to 524,288 output tokens.

•

## What are the best MiniMax M3.1 Flash alternatives?

DeepSeek V4.1 Flash is the closest alternative, with the same off-peak prices, open MIT weights and stronger coding scores in MiniMax’s own table. Muse Wonder and other frontier models cost more but score higher on the hardest tests.
