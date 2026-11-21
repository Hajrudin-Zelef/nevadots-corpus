---
id: collect-261121-veille-ia/claude-anthropic/claude-opus-5-5-api-pricing-4-00-20-00-per-1m-optimtoken
title: "Claude Opus 5.5 API pricing: $4.00/$20.00 per 1M | OptimToken"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-20"]
keywords: ["price", "claude", "tokens", "quote", "opus", "model", "arena", "tier"]
source: https://optimtoken.optimnow.io/models/anthropic/claude-opus-5-5
source_anchor: ""
source_lines: [1, 116]
section: "Claude_Anthropic"
---
# Claude Opus 5.5 API pricing

Anthropic · Premium price tier · Frontier quality · Proprietary · released 2026-09. Prices in USD per 1 million tokens. Data snapshot: 2026-11-20.

A support ticket workload of 100K requests a month costs $1600.00 on Claude Opus 5.5; the next cheaper model in the Premium price tier for this workload, Claude Sonnet 4.5, does it for $1200.00. Claude Opus 5.5 has held the same list price since 2026-09-23 and is the 9th cheapest of 33 models in the Premium price tier. None of its 5 hosts undercuts the price we quote. It is a model in the Premium price tier released within the last 3 months, the period in which versions and prices change most.

- Input: $4.00 per 1M tokens
- Output: $20.00 per 1M tokens
- Batch API (in/out): $2.00 / $10.00 per 1M tokens
- Prompt-cache read: $0.20 per 1M tokens
- Context window: 1M tokens
- Arena ELO: 1514 as of 2026-11-11

## Available on

Direct API, AWS Bedrock, Azure AI Foundry, Google Vertex, OpenRouter

AWS Bedrock regions: ap-northeast-1, ap-northeast-3, ap-southeast-1, eu-central-1, eu-north-1, eu-south-1, eu-south-2, eu-west-1, eu-west-3, us-east-1, us-east-2, us-west-2

Curated by hand; check with the provider before committing.

## Market position

On blended price ($15.20 per 1M tokens at 30% input, 70% output) it is cheaper than 66% of models in the Premium price tier: 9th cheapest of 33, tied with 3 others. Across every price tier it is cheaper than 7% of priced text models in the catalogue: 283rd cheapest of 307, tied with 3 others. Its Arena ELO of 1514 ranks 2nd of the 157 models that carry one, above 99% of them. With an Arena score of 1514, Claude Opus 5.5 clears OptimNow's Frontier-grade, Strong and Standard working quality thresholds: Frontier-grade (Arena 1504+), used for coding task and agent workflow; Strong (Arena 1469+), used for support ticket, knowledge Q&A and marketing content; Standard (Arena 1419+), used for meeting summary, invoice processing and call summary. Each threshold is a distance below the best ranked model, which scores 1529 on Arena scores as of 11 Nov 2026.

## Licence, size and capabilities

Claude Opus 5.5 is recorded here as proprietary: access is through an API, priced by the token. Its parameter count is not published. Listed capabilities: Text, Vision, Reasoning, Agents. Filling its 1M context window once costs $4.00 in input tokens at list price.

## Pricing by host

A host is a company that runs the model and sells it by the token: the model maker, a cloud platform like Amazon Bedrock, Azure or Vertex, or an independent host. One model, several hosts, and often several prices. This model is sold by 5 different hosts. The price we quote elsewhere on this page is marked. We quote one host's rate so that models stay comparable; it is not a recommendation to buy there.

**The price we quote is already the cheapest of the 5 hosts: Anthropic at $4.00 input / $20.00 output per 1M tokens. All of them charge within 10% of one another.**

| Host | Input $/1M | Output $/1M | 
|---|---|---|
| Anthropic (model maker, price we quote) | $4.00 | $20.00 | 
| Amazon Bedrock (price we quote) | $4.00 | $20.00 | 
| Azure (price we quote) | $4.00 | $20.00 | 
| Google (price we quote) | $4.00 | $20.00 | 
| Claude Platform on AWS (price we quote) | $4.00 | $20.00 | 

List prices as published on 2026-11-20, via OpenRouter. Negotiated rates, committed-use discounts and provisioned capacity are not shown.

## Monthly cost at 100K requests, by business use case

From the after-discounts figure (prompt caching on the cached input share, batch API for async workloads, only where Anthropic publishes those rates), which assumes you implement both, to the list price. One figure means no discount is published.

| Use case | Tokens (in/out) | Monthly (after discounts – list) | 
|---|---|---|
| Support Ticket | 1,500 / 500 | $1.3K – $1.6K | 
| Knowledge Q&A | 2,000 / 800 | $1.9K – $2.4K | 
| Meeting Summary | 10,000 / 1,200 | $3.1K – $6.4K | 
| Marketing Content | 2,500 / 1,800 | $4.4K – $4.6K | 
| Coding Task | 3,000 / 2,000 | $4.7K – $5.2K | 
| Invoice Processing | 1,500 / 600 | $846 – $1.8K | 
| Call Summary | 2,000 / 700 | $1.1K – $2.2K | 
| Agent Workflow | 6,000 / 3,000 | $6.9K – $8.4K | 

## Price history

Claude Opus 5.5 entered our daily price archive on 2026-09-23; tracking began on 2026-08-15. It listed at $4.00 input / $20.00 output per 1M tokens that day. No list price change since 2026-09-23. Last checked against the 2026-11-20 snapshot.

## Alternatives to Claude Opus 5.5

None of the 4 comparable models below costs less than Claude Opus 5.5 on the blended price.

- Next cheaper model in the Premium price tier: Claude Sonnet 4.5 (Anthropic) at $11.40 blended, 25% less.
- Next more expensive: Claude Opus 4.5 (Anthropic) at $19.00 blended, 25% more.

The nearest models in the Premium price tier on Arena ELO, each with its blended price and score difference against Claude Opus 5.5.

- Claude Fable 5.1 (Anthropic): $10.00 in / $50.00 out per 1M, Arena ELO 1512 (150% more expensive, 2 ELO points lower)
- Muse Wonder (meta): $4.00 in / $20.00 out per 1M, Arena ELO 1503 (same blended price, 11 ELO points lower)
- Claude Opus 5 (Anthropic): $5.00 in / $25.00 out per 1M, Arena ELO 1502 (25% more expensive, 12 ELO points lower)
- Claude Opus 4.6 (Anthropic): $5.00 in / $25.00 out per 1M, Arena ELO 1497 (25% more expensive, 17 ELO points lower)

- Cheapest within 15 Arena points: Gemini 3.8 Flash (Google), 81% cheaper, 14 ELO points lower, out of 6 models in that band.
- Highest Arena ELO within 20% of this price: Claude Opus 5.5 itself has the highest score of the 3 models priced within 20% of it.

## Other models from Anthropic

The 8 nearest to Claude Opus 5.5 in price, of 15 other Anthropic models in the catalogue. Prices per 1M tokens, input then output.

- Claude Sonnet 5: $2.00 / $10.00
- Claude Sonnet 5.5: $2.00 / $10.00
- Claude Sonnet 4.5: $3.00 / $15.00
- Claude Sonnet 4.6: $3.00 / $15.00
- Claude Opus 4.5: $5.00 / $25.00
- Claude Opus 4.6: $5.00 / $25.00
- Claude Opus 4.7: $5.00 / $25.00
- Claude Opus 4.8: $5.00 / $25.00

## Common questions about Claude Opus 5.5 pricing

- How much does Claude Opus 5.5 cost?
- Claude Opus 5.5 costs $4.00 per 1M input tokens and $20.00 per 1M output tokens. That is the model maker's own rate (Anthropic). Prices as listed on 2026-11-20.
- Which host is cheapest for Claude Opus 5.5?
- The price we quote is already the cheapest of the 5 hosts: Anthropic at $4.00 input / $20.00 output per 1M tokens. All of them charge within 10% of one another. Host rates as listed on 2026-11-20.
- Does Claude Opus 5.5 offer batch or cached pricing?
- Yes, both. Batch API pricing is $2.00 per 1M input tokens and $10.00 per 1M output tokens, and prompt-cache reads cost $0.20 per 1M tokens, as listed on 2026-11-20.
- What would Claude Opus 5.5 cost per month?
- With the Support Ticket profile (1,500 input and 500 output tokens per request), 100,000 requests a month cost about $1.6K on Claude Opus 5.5 at the list price quoted on 2026-11-20.
- Can Claude Opus 5.5 be self-hosted?
- No, as far as this site records. Claude Opus 5.5 is recorded here as proprietary: access is through an API, priced by the token.
- Has the price of Claude Opus 5.5 changed?
- Claude Opus 5.5 entered our daily price archive on 2026-09-23; tracking began on 2026-08-15. It listed at $4.00 input / $20.00 output per 1M tokens that day. No list price change since 2026-09-23. Last checked against the 2026-11-20 snapshot.

It publishes a batch rate, and that discount is conditional: when the batch discount applies.

**These are list prices.** Your real cost also depends on caching and batching, negotiated discounts, marketplace agreements, and the contracts you already have. OptimNow audits exactly that.

Build the business case · Get a FinOps review

LLM pricing · Models by maker · Cloud compute pricing · Compare models · Price barometer · FinOps guides · API and MCP server
