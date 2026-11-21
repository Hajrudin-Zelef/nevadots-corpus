---
id: collect-261121-veille-ia/claude-anthropic/claude-haiku-5-5-costs-a-tenth-of-its-predecessor-with-a-catch-in-the-fine-print
title: "Claude Haiku 5.5 Costs a Tenth of Its Predecessor, With a Catch in the Fine Print"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-07"]
keywords: ["haiku", "percent", "tokens", "claude", "anthropic", "more", "model", "costs"]
source: https://www.newscast.tech/claude-haiku-5-5-price-cut
source_anchor: ""
source_lines: [1, 35]
section: "Claude_Anthropic"
---
# Claude Haiku 5.5 Costs a Tenth of Its Predecessor, With a Catch in the Fine Print

Anthropic cut Haiku pricing by 90 percent for short prompts, but long prompts cost five times more and the new tokenizer bills more tokens per task.

## The Haiku 5.5 price cut in one paragraph

Anthropic released Claude Haiku 5.5 on October 7, 2026, and the headline number is hard to ignore: for prompts under 100,000 tokens, input costs $0.10 per million tokens and output costs $0.50, down from $1 and $5 on Haiku 4.5. That is the Haiku 5.5 price cut in its simplest form, a 90 percent reduction that lands exactly on OpenAI's GPT-6 Luna rate card. Anthropic says typical workloads will cost about 75 percent less overall, not 90 percent, and the gap between those two numbers is where the real story lives.

Haiku 5.5 is Anthropic's small, fast model, built for high-volume work like classification, routing, summarization, and subagent tasks. It is also the third Claude 5.5 model in about a month, following Opus 5.5 and Sonnet 5.5. The release matters beyond Anthropic's customer list because small-model pricing sets the floor for what AI features cost to run at scale. When the floor drops this far, product teams rethink which features are worth building.

## What actually got cheaper, and what did not

The pricing has two tiers, and the tier boundary matters more than the headline. Under 100,000 tokens per request, you pay the new low rates. Over that line, input jumps to $0.50 and output to $2.50 per million tokens, five times the short-prompt rate. Those long-prompt rates are still half of Haiku 4.5, so nothing got more expensive in absolute terms. But teams whose traffic sits mostly above the line will see a 50 percent saving, not 90 percent, and they need to budget for that.

Then there is the tokenizer. Haiku 5.5 counts roughly 30 percent more tokens for the same text than its predecessor, according to independent analyses of the launch. A token is the chunk of text a model bills by, so a job that used to cost 1,000 tokens may now bill around 1,300. Anthropic's 75 percent average already accounts for this, but it means you cannot just swap the model string and divide your bill by ten. Run your own traffic through the new rate card before you promise savings to anyone.

There are two more changes worth noting. The context window grew from 200,000 tokens to 1 million, with output up to 128,000 tokens, and Haiku 5.5 is the first small Claude with an adjustable effort setting that lets you choose how hard the model thinks before answering. Cache reads on Sonnet 5.5 were also halved to $0.10 per million tokens, which cuts agentic workloads that reuse prompt text. The model is available on the Claude API, Amazon Bedrock, Google Cloud, and Microsoft Foundry.

## The benchmarks are big, but read them carefully

Anthropic's benchmark table claims dramatic gains over Haiku 4.5. On the offline subset of OSWorld 2.1, a computer-use test, Haiku 5.5 scores 72.4 percent against 15.7 percent for its predecessor. On GDPval-AA, a knowledge-work evaluation, it scores 1620 Elo against 735. On Humanity's Last Exam, an expert-level reasoning test, it reaches 45.9 percent without tools and 57.4 percent with tools, up from 10.2 and 18.7 percent.

Those are vendor-published numbers, not independent results, and they deserve the usual caution. What makes them more interesting than a typical launch chart is the customer evidence attached to them. Asana reported over 30 percent lower latency and up to 2.5 times faster inference. Box said Haiku 5.5 scored 11 points higher than Haiku 4.5 at about half the latency. AlphaSense saw document question-answering improve from 0.76 to 0.84. None of these prove the model will be great for your workload, but they suggest the gains are not purely a benchmark artifact.

If you are comparing small models, the honest frame is this: Haiku 5.5 now matches Luna on sticker price for short prompts, while Kimi K3.1 Pro is pushing frontier capability toward open weights at $2 per million input tokens. The small-model tier is no longer the place you go when you cannot afford a real model. It is a tier with real capability, and the vendors know it.

## How to switch without a surprise bill

First, price a real job, not a token count. Take a week of your production traffic, split it at the 100,000-token line, apply the new tokenizer's higher counts, and compare the result against your current bill. If most of your traffic is short, the savings will be close to the headline. If it is long, budget for the 50 percent tier instead.

Second, check the breaking changes before you migrate. Haiku 5.5 drops several request shapes that worked on Haiku 4.5, including some sampling parameters and assistant prefill patterns, and those requests now return errors instead of falling back. Grep your codebase for the old parameters, update your parser assumptions (thinking now lives inside max_tokens, and blocks are typed), and give refusals a handled path. There is no server-side fallback waiting to catch you.

Third, use the effort setting deliberately. Low effort suits routing and classification, where speed matters more than depth. Higher effort suits the agentic work where Haiku 5.5's benchmark gains actually show up. The cheapest model in the world is still expensive if you run every request at maximum effort for no reason.

The broader point is that small-model pricing is now a competitive weapon, not a rounding error. Anthropic cut three Claude prices in three weeks between late September and early October, and OpenAI answered in kind on its own small tier. For builders, that is good news, as long as you read the rate card instead of the headline. The GPT-6 rollout showed the same pattern: the announcement number is the start of the analysis, not the end of it.
