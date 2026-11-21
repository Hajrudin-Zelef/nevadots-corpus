---
id: collect-261121-veille-ia/claude-anthropic/claude-code-pricing-plans-api-openrouter-bedrock-costs
title: "Claude Code Pricing: Plans, API, OpenRouter, Bedrock Costs"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-15"]
keywords: ["claude", "code", "opus", "every", "month", "haiku", "sonnet", "billed"]
source: https://www.turboai.dev/blog/claude-code-pricing-guide
source_anchor: ""
source_lines: [1, 125]
section: "Claude_Anthropic"
---
# How Much Does Claude Code Cost?#

Published by Alex Sorokoletov

Claude Code costs $20 a month on Claude Pro ($17 a month billed annually), the cheapest plan that includes it. Max is $100 and Ultra $200 a month. With an API key you pay per token instead: from $0.10 input and $0.50 output per million tokens on Claude Haiku 5.5 to $4 and $20 on Claude Opus 5.5, as of 2026-11-15.<sup>[1][2]</sup>

## Plans that include Claude Code#

The Free plan does not include Claude Code. Every paid plan does, and Claude Code draws from the same usage pool as Claude on the web, desktop and mobile.<sup>[1]</sup> Anthropic states limits as multiples of Pro, not as token or message counts: "How much you can do depends on the length and complexity of your conversations, the model you choose, and the features you use, so there's no fixed message count."<sup>[1]</sup>

| Claude plans with Claude Code, as of 2026-11-15. Source: claude.com/pricing |  |  |  |  | 
|---|---|---|---|---|
| Plan | Price | Usage limit | Models in Claude Code | Monthly API credits | 
|---|---|---|---|---|
| Free | $0 | Limited, every 5 hours and every week | No Claude Code | None | 
| Pro | $20/month, or $17/month billed annually ($200 up front) | 1x, every 5 hours and every week | Opus, Sonnet, Haiku; Fable paid with usage credits | None | 
| Max | $100/month, billed monthly | 5x Pro, every 5 hours and every week | Opus, Sonnet, Haiku; Fable up to 50% of the weekly limit | $100 | 
| Ultra | $200/month, billed monthly | 10x Pro every week; no 5-hour limit for verified accounts | Opus, Sonnet, Haiku; Fable up to 50% of the weekly limit | $200 | 
| Team, Standard seat | $25/seat/month, or $20 billed annually | More than Pro | Opus, Sonnet, Haiku; Fable not listed for Standard seats | Up to $500 pooled per team | 
| Team, Premium seat | $125/seat/month, or $100 billed annually | 5x a Standard seat | Opus, Sonnet, Haiku; Fable up to 50% of the weekly limit |  | 
| Enterprise | $20/seat/month billed annually, 20-seat minimum, plus usage at API rates | Usage is billed at API rates | Fable, Opus, Sonnet, Haiku | None | 

Team is for 2 to 150 people. On every plan, the default model in Claude Code is Opus 5.5.<sup>[3]</sup> When you reach a limit, you wait for the reset, move to a higher plan, or turn on usage credits, which bill at standard API rates.<sup>[1]</sup> In Claude Code, `/usage-credits` opens the page that manages them; it does not work with API key authentication.<sup>[4]</sup>

## API pricing for Claude Code with an API key#

With `ANTHROPIC_API_KEY` set, Claude Code bills every token at the Claude API list price. Opus 5.5 and Sonnet 5.5 charge the same rate across their full 1M-token context window; Haiku 5.5 charges more for any request whose prompt is over 100,000 tokens, cache reads and writes included.<sup>[2]</sup>

| Claude API prices in USD per million tokens, as of 2026-11-15. Source: Claude API pricing |  |  |  |  |  |  | 
|---|---|---|---|---|---|---|
| Model | Input | Output | 5-min cache write | 1-hour cache write | Cache read | Batch input / output | 
|---|---|---|---|---|---|---|
| Opus 5.5 ( `claude-opus-5-5` ) | $4 | $20 | $5 | $8 | $0.20 | $2 / $10 | 
| Sonnet 5.5 ( `claude-sonnet-5-5` ) | $2 | $10 | $2.50 | $4 | $0.10 | $1 / $5 | 
| Haiku 5.5 ( `claude-haiku-5-5` ), prompt up to 100K | $0.10 | $0.50 | $0.125 | $0.20 | $0.01 | $0.05 / $0.25 | 
| Haiku 5.5, prompt over 100K | $0.50 | $2.50 | $0.625 | $1 | $0.05 | $0.25 / $1.25 | 

Three modifiers change these numbers. Cache reads on Opus 5.5 and Sonnet 5.5 cost 0.05x the input price, half the 0.1x that most models charge. US-only inference (`inference_geo: "us"`) multiplies every price by 1.1. Fast mode on Opus 5.5 costs $8 input and $40 output, twice the standard rate, and does not combine with the Batch API.<sup>[2]</sup> An API key session uses the 5-minute cache unless you set `"promptCacheTtl": "1h"`, so the 5-minute write price is the one most sessions pay (see the prompt cache guide).

## OpenRouter and Amazon Bedrock prices#

Claude Code runs through OpenRouter with `ANTHROPIC_BASE_URL="https://openrouter.ai/api"` and an OpenRouter key in `ANTHROPIC_AUTH_TOKEN`; OpenRouter guarantees it only with Anthropic as the provider.<sup>[5]</sup> It runs on Bedrock with `CLAUDE_CODE_USE_BEDROCK=1`. Bedrock lists two endpoint types: global cross-region inference, and geo or in-region inference, which costs 10% more.<sup>[2][6]</sup> The Bedrock prices below are for us-east-1 (US East, N. Virginia).

| Opus 5.5, USD per million tokens, as of 2026-11-15 |  |  |  |  |  |  | 
|---|---|---|---|---|---|---|
| Provider | Input | Output | 5-min cache write | 1-hour cache write | Cache read | Batch input / output | 
|---|---|---|---|---|---|---|
| Claude API | $4 | $20 | $5 | $8 | $0.20 | $2 / $10 | 
| OpenRouter | $4 | $20 | $5 | $8 | $0.20 | $2 / $10 | 
| Bedrock us-east-1, global | $4 | $20 | $5 | $8 | $0.20 | Not offered | 
| Bedrock us-east-1, in-region | $4.40 | $22 | $5.50 | $8.80 | $0.22 | Not offered | 

| Sonnet 5.5, USD per million tokens, as of 2026-11-15 |  |  |  |  |  |  | 
|---|---|---|---|---|---|---|
| Provider | Input | Output | 5-min cache write | 1-hour cache write | Cache read | Batch input / output | 
|---|---|---|---|---|---|---|
| Claude API | $2 | $10 | $2.50 | $4 | $0.10 | $1 / $5 | 
| OpenRouter | $2 | $10 | $2.50 | $4 | $0.10 | $1 / $5 | 
| Bedrock us-east-1, global | $2 | $10 | $2.50 | $4 | $0.10 | Not offered | 
| Bedrock us-east-1, in-region | $2.20 | $11 | $2.75 | $4.40 | $0.11 | Not offered | 

| Haiku 5.5, USD per million tokens, as of 2026-11-15 (second figure: prompt over 100K tokens) |  |  |  |  |  |  | 
|---|---|---|---|---|---|---|
| Provider | Input | Output | 5-min cache write | 1-hour cache write | Cache read | Batch input / output | 
|---|---|---|---|---|---|---|
| Claude API | $0.10 / $0.50 | $0.50 / $2.50 | $0.125 / $0.625 | $0.20 / $1 | $0.01 / $0.05 | $0.05 / $0.25 (over 100K: $0.25 / $1.25) | 
| OpenRouter | $0.10 / $0.50 | $0.50 / $2.50 | $0.125 / $0.625 | $0.20 / $1 | $0.01 / $0.05 | $0.05 / $0.25 (over 100K: $0.25 / $1.25) | 
| Bedrock us-east-1, global | $0.10 / $0.50 | $0.50 / $2.50 | $0.125 / $0.625 | $0.20 / $1 | $0.01 / $0.05 | Not offered | 
| Bedrock us-east-1, in-region | $0.11 / $0.55 | $0.55 / $2.75 | $0.1375 / $0.6875 | $0.22 / $1.10 | $0.011 / $0.055 | Not offered | 

OpenRouter passes through the provider's token prices and charges a 5.5% fee ($0.80 minimum) when you buy credits, 5% for crypto.<sup>[5]</sup> Bedrock lists "N/A" for batch on all three models and notes that Sonnet 5.5 cache reads dropped 50% on October 7, 2026.<sup>[6]</sup>

## Which is cheaper for you#

Take one API key session of 200K uncached input tokens, 500K tokens written to the 5-minute cache, 10M cache-read tokens and 150K output tokens. Agentic sessions read far more from the cache than they send fresh, because every turn resends the conversation. Each cost below is tokens times the price from the tables above.

| Cost of the example session, as of 2026-11-15 |  |  |  |  |  | 
|---|---|---|---|---|---|
| Model and provider | Uncached input | Cache write | Cache read | Output | Total | 
|---|---|---|---|---|---|
| Opus 5.5, Claude API or Bedrock global | $0.80 | $2.50 | $2.00 | $3.00 | $8.30 | 
| Opus 5.5, Bedrock in-region | $0.88 | $2.75 | $2.20 | $3.30 | $9.13 | 
| Opus 5.5, OpenRouter with the 5.5% credit fee | $8.30 x 1.055 |  |  |  | $8.76 | 
| Sonnet 5.5, Claude API or Bedrock global | $0.40 | $1.25 | $1.00 | $1.50 | $4.15 | 
| Sonnet 5.5, Bedrock in-region | $0.44 | $1.375 | $1.10 | $1.65 | $4.57 | 
| Haiku 5.5, every prompt under 100K | $0.02 | $0.0625 | $0.10 | $0.075 | $0.26 | 
| Haiku 5.5, every prompt over 100K | $0.10 | $0.3125 | $0.50 | $0.375 | $1.29 | 

At $8.30 a session, Opus 5.5 on an API key passes the $20 Pro price after three sessions a month and the $100 Max price after thirteen. Anthropic does not publish how many tokens a plan's limits hold, so the plan is cheaper only while its limits cover your work; past them, usage credits bill at the same API rates. Anthropic's own figure for enterprise deployments is about $13 per developer per active day and $150 to $250 per developer per month, with 90% of users under $30 per active day.<sup>[4]</sup> That range is above the $100 Max price and spans the $200 Ultra price. For occasional use or scripted runs, the API costs less. Bedrock global and OpenRouter cost the same per token as the Claude API; pick them for billing, region or provider-routing reasons, not for price. On Bedrock, the in-region endpoint adds 10% to every line.

Check your own numbers in Claude Code with `/usage`. On an API key, its Session block shows the session's tokens and a `Total cost` estimate at list price; on a plan, it shows the 5-hour and weekly usage bars and what used them. `/cost` and `/stats` merged into `/usage` in v2.1.118 and still open its tabs.<sup>[4][7]</sup> To read the breakdown, see the /usage breakdown guide.

## FAQ#

**Q: How much does Claude Code cost per month?**

A: $20 a month on Claude Pro, or $17 a month billed annually. Max is $100 a month and Ultra $200. Team seats are $25 or $125 a month ($20 or $100 billed annually). As of 2026-11-15.

**Q: Is Claude Code free?**

A: No. The Free plan does not include Claude Code. Pro, Max, Ultra, Team and Enterprise do, or you can pay per token with an API key.

**Q: What are the Claude Code usage limits on Pro and Max?**

A: Pro has a limit that resets every 5 hours and a weekly limit. Max gives 5x Pro on both. Ultra gives 10x Pro's weekly limit with no 5-hour limit for verified accounts. Anthropic does not publish token or message counts for these limits.

**Q: How much does Claude Code cost with an API key?**

A: Per million tokens: Opus 5.5 $4 input and $20 output, Sonnet 5.5 $2 and $10, Haiku 5.5 $0.10 and $0.50 (prompts up to 100K tokens). Cache reads cost $0.20, $0.10 and $0.01.

**Q: Is Claude cheaper on OpenRouter or Bedrock than on the Claude API?**

A: No. OpenRouter and Bedrock global endpoints list the same token prices as the Claude API. OpenRouter adds a 5.5% fee when you buy credits, and Bedrock in-region endpoints cost 10% more.

**Q: Can Pro users use Opus in Claude Code?**

A: Yes. Opus is available on Pro, and Opus 5.5 is the default model in Claude Code on every plan. Fable on Pro is paid with usage credits.

**Q: How do I see what a Claude Code session cost?**

A: Run `/usage`. With an API key, its Session block shows the tokens used and an estimated total cost at list price. `/cost` and `/stats` merged into `/usage` in v2.1.118.

*Sources (all read 2026-11-15): [1] Claude plans and pricing | [2] Claude API pricing | [3] Claude Code model configuration | [4] Claude Code: manage costs | [5] OpenRouter models API, Opus 5.5, Sonnet 5.5, Haiku 5.5, fees, Claude Code setup | [6] Amazon Bedrock pricing and the AWS Price List API for us-east-1 | [7] Claude Code changelog, v2.1.118*

*Related: Install and update Claude Code | Claude Code env-var glossary | CLAUDE_CODE_USE_BEDROCK in the glossary | CLAUDE_CODE_ATTRIBUTION_HEADER: Bedrock 400 fix | Prompt cache TTL guide | /usage breakdown guide*
