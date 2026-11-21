---
id: collect-261121-veille-ia/claude-anthropic/claude-haiku-5-5-effort-controls-1m-context-two-tier-pricing
title: "Claude Haiku 5.5: Effort Controls, 1M Context, Two-Tier Pricing"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-07"]
keywords: ["haiku", "effort", "anthropic", "medium", "tools", "osworld", "context", "output"]
source: https://lucenta.ai/blog/claude-haiku-5-5-first-credible-haiku-agent
source_anchor: ""
source_lines: [1, 107]
section: "Claude_Anthropic"
---
## What Shipped

Anthropic released `claude-haiku-5-5` on October 7, 2026. It succeeds `claude-haiku-4-5-20251001`. The API id carries no date suffix. On Amazon Bedrock the id is `anthropic.claude-haiku-5-5`; the same short id applies on Google Cloud Vertex AI and Microsoft Foundry.

- Context window: 1M tokens (was 200K), with 128K max output. Message Batches support 300K output with the `output-300k-2026-03-24` beta header.
- Effort controls: first Haiku with adjustable effort. Levels are `low` ,`medium` ,`high` ,`xhigh` ,`max` . Default is`medium` .
- Adaptive thinking: on by default. Disabling it is allowed at `high` or below; at`xhigh` or`max` it returns a 400.
- Computer and browser use tools: `computer_toolset_20260801` and`browser_toolset_20260801` on the Claude API and Google Cloud.
- Modalities: text and images in (PDF supported), text out. Knowledge cutoff: June 2026.

## Benchmarks

All scores below are self-reported by Anthropic. `claude-haiku-5-5` ran at max effort with adaptive thinking, default sampling, and five trials. These figures are not independently verified.

| Benchmark | Haiku 5.5 | Haiku 4.5 | GPT-6 Luna | Sonnet 5.5 | 
|---|---|---|---|---|
| GDPval-AA v2.1 (Elo) | 1620 | 735 | 1437 | 1840 | 
| AA-Briefcase v1.1 (Elo) | 1578 | 614 | 1336 | 1824 | 
| OSWorld 2.1 partial (%) | 72.4 | 15.7 | 48.9 | 83.9 | 
| OSWorld 2.1 strict (%) | 37.1 | - | - | 48.8 | 
| Terminal-Bench 4.0 (%) | 39.2 | 0.0 | 16.4 | 70.6 | 
| FrontierCode 1.1 Main (%) | 46.4 | - | 42.4 | 52.1 (xhigh) | 
| HLE no tools (%) | 45.9 | 10.2 | - | 56.9 | 
| HLE with tools (%) | 57.4 | 18.7 | - | 64.5 | 
| Chartography no tools (%) | 46.4 | 6.4 | 29.1 | 61.6 | 
| Chartography with tools (%) | 86.2 | 8.8 | - | 90.2 | 

OSWorld ran on 82 of 108 tasks, offline, 1080p, max 500 steps. GPT-6 Luna's OSWorld figure was run by Anthropic through OpenAI's API with OpenAI's context compaction. Terminal-Bench used 66 tasks, 10 trials each, Claude Code in bare mode at max effort, SE plus or minus 1.9. GPT-6 Luna's Terminal-Bench figure comes from the public leaderboard. FrontierCode was built and run by Cognition; the Sonnet 5.5 cite of 52.1% is at xhigh, not max. At matched max, Sonnet scored 46.2%, essentially tied with Haiku 5.5.

The jump that matters for Haiku buyers is the delta off Haiku 4.5. GDPval-AA rises 885 Elo points. OSWorld partial rises 56.7 points. Terminal-Bench goes from 0.0% to 39.2%. Sonnet 5.5 still leads every hard agent row.

## Effort vs. Cost

Anthropic published per-task cost estimates at each effort level. Default is `medium`. Most production traffic should start there and measure before stepping up.

| Effort | OSWorld partial | GDPval-AA (Elo) | HLE no tools | Terminal-Bench | 
|---|---|---|---|---|
| low | 42.0% | 1125 | 30.9% | 12.7% | 
| medium (default) | 53.3% | 1277 | 35.5% | 20.3% | 
| high | 61.3% | 1420 | 39.7% | 24.8% | 
| xhigh | 67.6% | 1513 | 44.1% | 31.5% | 
| max | 72.4% | 1620 | 45.9% | 39.2% | 

At `medium`, GDPval-AA reaches 1277 Elo using about a tenth of the output tokens consumed at max. AA-Briefcase at `medium` is 1372 Elo using under a quarter. Scores self-reported by Anthropic, not independently verified.

## Pricing

Two-tier pricing keyed to prompt length. A request pays the rate for that request's prompt length on the entire request.

| Rate | Haiku 5.5 (<=100K) | Haiku 5.5 (>100K) | Haiku 4.5 | Sonnet 5.5 | 
|---|---|---|---|---|
| Input / 1M | $0.10 | $0.50 | $1.00 | $2.00 | 
| Output / 1M | $0.50 | $2.50 | $5.00 | $10.00 | 
| 5-min cache write | $0.125 | $0.625 | $1.25 | $2.50 | 
| 1-hr cache write | $0.20 | $1.00 | $2.00 | $4.00 | 
| Cache read | $0.01 | $0.05 | $0.10 | $0.10 (was $0.20) | 
| Batch input / output | $0.05 / $0.25 | $0.25 / $1.25 | $0.50 / $2.50 | $1.00 / $5.00 | 

On average, it now costs around 75% less to run.

Sticker math from Anthropic: 90% lower than Haiku 4.5 for requests up to 100K, 50% lower above 100K. About 90% of Haiku 4.5 requests had prompts under 100K. The 75% average already accounts for the new tokenizer. The same day, Sonnet 5.5 cache reads were cut 50% from $0.20 to $0.10. A `us` inference geo adds a 1.1x multiplier. Bedrock and Vertex regional endpoints carry a 10% premium over global.

## The Tokenizer Trap

Haiku 5.5 ships a new tokenizer similar to the one in Sonnet 5.5 and Opus 5.5. The migration guide states the same input text produces approximately 30% more tokens than on Haiku 4.5. The exact increase is content-dependent.

- Example A: 10K in / 1K out. Haiku 5.5 $0.0015 vs Haiku 4.5 $0.0150. At 1.3x tokens: $0.00195, still 87% below Haiku 4.5.
- Example B: 150K in / 4K out. Haiku 5.5 $0.085 vs Haiku 4.5 $0.170. At 1.3x: $0.1105, 35% below.
- Example D (tier-crossing trap): 90K / 2K on Haiku 4.5 becomes roughly 117K / 2.6K on Haiku 5.5. Haiku 4.5 $0.100 vs Haiku 5.5 $0.065. Savings: 35%, not 90%.

A 100,000-token prompt costs $0.010 on input. A 100,001-token prompt costs about $0.050: five times the input rate on the entire prompt. Output jumps from $0.50 to $2.50 the same way. Recount prompts near 100K with `model: claude-haiku-5-5` before assuming the headline discount applies.

## API Constraints

Several parameters that worked on Haiku 4.5 return a 400 on Haiku 5.5:

- `temperature` ,`top_p` , and`top_k` must be omitted entirely.
- Assistant prefill is rejected with a 400, even with thinking disabled.
- Priority Tier is not supported.
- `thinking: { type: enabled, budget_tokens: N }` returns a 400. Use adaptive thinking instead.
- Disabling thinking at `xhigh` or`max` returns a 400.

Thinking blocks return empty by default (signature only). Setting `display: summarized` returns summaries. Thinking tokens bill as output and count toward `max_tokens`. The constraint list is longer than any prior Haiku release.

## When to Use Each Model

Prefer `claude-haiku-5-5` for high-volume classification, extraction, routing, chat and voice agents, live support, in-app assistants, and browser or desktop automation where Haiku 4.5 was too weak and Sonnet 5.5 is too expensive per turn. It also works as a subagent under Opus 5.5 or Sonnet 5.5 on coding pipelines: the stronger model plans, Haiku executes scoped subtasks.

Step up to Sonnet 5.5 when Terminal-Bench margins, OSWorld strict scores, or open-ended judgment dominate cost. Sonnet leads every hard agent row by a meaningful gap.

## Migration Checklist

1. Swap to `claude-haiku-5-5` . No date suffix, no alias.
2. Recount prompts near 100K with the new model id. Expect roughly 30% more tokens for the same text.
3. Drop non-default `temperature` ,`top_p` , and`top_k` .
4. Drop assistant prefill and Priority Tier.
5. Use adaptive thinking. Do not send `budget_tokens` .
6. Disable thinking only at `high` effort or below.
7. Benchmark at default `medium` before assuming you need`max` .

Haiku 4.5 remains active. No deprecation date has been announced.

## Availability

Claude API; Claude.ai on all plans (Free, Pro, Max, Team, Enterprise; web, iOS, Android); Claude Code; Amazon Bedrock (`anthropic.claude-haiku-5-5`); Claude Platform on AWS; Google Cloud Vertex AI; Microsoft Foundry. The id `claude-haiku-5-5` is consistent across Vertex, Foundry, and Claude Platform on AWS.

Safeguards are more restrictive than Haiku 4.5 but somewhat less restrictive than other recent models. Cybersecurity permits a wider range of defensive tasks than Sonnet 5.5 but still blocks penetration testing. Biology safeguards match Sonnet 5, Sonnet 5.5, and Opus 5. Safety classifiers can return `stop_reason: refusal`; there is no server-side fallback when a classifier triggers.
