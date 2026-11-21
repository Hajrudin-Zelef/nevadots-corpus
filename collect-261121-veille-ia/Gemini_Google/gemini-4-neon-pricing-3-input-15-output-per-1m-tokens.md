---
id: collect-261121-veille-ia/gemini-google/gemini-4-neon-pricing-3-input-15-output-per-1m-tokens
title: "Gemini 4 Neon Pricing: $3 Input, $15 Output per 1M Tokens"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "neon", "tokens", "input", "million", "output", "tier", "rate"]
source: https://www.layer3labs.io/guides/gemini-4-neon-pricing
source_anchor: ""
source_lines: [1, 104]
section: "Gemini_Google"
---
# Gemini 4 Neon Pricing

Google prices Gemini 4 Neon at $3.00 per million input tokens and $15.00 per million output tokens on the Standard API tier with no free tier.

Gemini 4 Neon costs $3.00 per 1 million input tokens and $15.00 per 1 million output tokens on the Standard Application Programming Interface (API) tier. At Layer3Labs, we build custom AI agents and workflow automations for small and mid-sized businesses, so model price and routing decide which model each workflow runs on. Neon's rate is more than Carbon's and Argon's through November 30, 2026 and less than both from December 1, 2026. Every token is billed with no free tier, and cached input costs $0.15 per 1 million tokens, a 95 percent discount.

Google released Gemini 4 Neon on November 18, 2026, under the model identifier gemini-4-neon with a unified 1 million token context window. In its announcement blog post, Google positions Neon between Gemini 4 Carbon and Gemini 4 Hydrogen for complex coding, analytical reasoning and enterprise workflows. Neon costs exactly twice Hydrogen on both dates. Its position against Carbon and Argon flips when their introductory pricing expires on December 1, 2026.

Production budgets depend on the tier. The rate card, the same-token cost math and the break-even conditions follow.

## Gemini 4 Neon Rate Card

Gemini 4 Neon costs $3.00 per 1 million input tokens and $15.00 per 1 million output tokens on the Standard paid API tier, as listed on Google's Gemini API pricing page. Cached input costs $0.15 per 1 million tokens, a 95 percent discount off the input rate. Neon supports implicit caching only, so explicit caching is not available.

Unlike Hydrogen, Gemini 4 Neon has no free tier on the Gemini API. Every token is counted and billed, including thinking tokens, internal tool system instructions and function declarations. Developers who want a free tier can look at Gemini 4 Hydrogen pricing. Google's pricing page says paid-tier data is not used to improve its products.

| Service Tier | Input Rate per 1M Tokens | Output Rate per 1M Tokens | Cached Input per 1M Tokens | 
|---|---|---|---|
| Standard | $3.00 | $15.00 | $0.15 | 
| Batch | $1.50 | $7.50 | $0.075 | 
| Flex | $1.50 | $7.50 | $0.075 | 
| Priority | $5.40 | $27.00 | $0.27 | 

Grounding adds separate fees. On the Standard and Priority tiers, Grounding with Google Search includes 5,000 free requests a month, shared across all Gemini 3 and newer models. After that Google bills $14 per 1,000 requests, up to 1,500 requests per day. Grounding with Google Maps offers 5,000 free shared queries each month, then $14 per 1,000 search queries.

Neon has four thinking levels. High is the default, and xhigh adds multi-step reasoning for hard scientific and engineering work. Thinking tokens are billed at the output rate of $15.00 per 1 million tokens. On per-token rates, Neon costs more than Carbon and Argon through November 30, 2026. It costs less than both from December 1, 2026, when their introductory rates expire, and exactly twice as much as Hydrogen on both dates.

## Neon vs Carbon, Argon and Hydrogen on Same-Token Price Math

In our internal cost modeling, we priced identical token volumes on Gemini 4 Neon, Carbon, Argon and Hydrogen at Google's Standard rates as published November 18, 2026. Neon goes from 50 percent more expensive than Carbon and Argon to 25 percent cheaper when the December 1, 2026 adjustment takes effect. The formula multiplies billable input millions by the effective input rate, adds output millions times the output rate, and applies an optional tokens-per-task multiplier. For other providers, see our every model per-token price chart.

| Model | Rate Card Status | Cost for 1M In and 1M Out (Through Nov 30, 2026) | Cost for 1M In and 1M Out (From Dec 1, 2026) | 
|---|---|---|---|
| Gemini 4 Neon | $3.00 in / $15.00 out (No Free Tier) | $18.00 | $18.00 | 
| Gemini 4 Carbon | $2.00 in / $10.00 out then $4.00 in / $20.00 out | $12.00 | $24.00 | 
| Gemini 4 Argon | $2.00 in / $10.00 out then $4.00 in / $20.00 out | $12.00 | $24.00 | 
| Gemini 4 Hydrogen | $1.50 in / $7.50 out (Free Tier Available) | $9.00 | $9.00 | 

Rates as published November 18, 2026. Every figure in the table is same-token price math, not a per-task result. At 200 million input tokens and 40 million output tokens a month with no caching, Neon costs $1,200.00. Carbon or Argon cost $800.00 through November 30, 2026 and $1,600.00 from December 1, 2026. Hydrogen costs $600.00.

Take 100 million input tokens and 20 million output tokens without caching. Neon costs $600.00. Carbon and Argon cost $400.00 through November 30, 2026 and $800.00 from December 1, 2026. Cache 90 percent of the input and Neon drops to $343.50, against $229.00 then $458.00 on Carbon and Argon and $171.75 on Hydrogen on every date.

Adjust the token volumes, cache share and tokens-per-task multipliers in the calculator below to model your own workload.

**Same-token price math, not a per-task result.** Rates as published November 18, 2026.

| Monthly cost at Through November 30, 2026, lowest first |  |  |  | 
|---|---|---|---|
| Rank | Model | Monthly cost | Difference from Neon | 
|---|---|---|---|
| 1 | Gemini 4 Hydrogen | $600.00 | -$600.00 | 
| 2 | Gemini 4 Carbon | $800.00 | -$400.00 | 
| 3 | Gemini 4 Argon | $800.00 | -$400.00 | 
| 4 | Gemini 4 Neon | $1,200.00 | Baseline | 

Gemini 4 Neon ranks 4 of 4 on these inputs.

## When Neon Is Cheaper

Neon is more expensive per token than Carbon and Argon through November 30, 2026, and cheaper from December 1, 2026. Through November 30, Neon's rate is 1.5 times theirs ($3.00/$15.00 versus $2.00/$10.00). On December 1, 2026, Carbon and Argon rise to $4.00 input and $20.00 output per 1 million tokens. Neon's rate then falls to 0.75 times theirs. Detailed line-item comparisons are in our guide to Gemini 4 Argon pricing.

Token pricing is one part of the bill. Token efficiency decides what a job costs. Through November 30, 2026, Carbon must use more than 1.5 times Neon's tokens on the same job for Neon to cost less per task. From December 1, 2026, Carbon needs more than 0.75 times Neon's tokens for Neon to win.

Here is a worked case. A task takes 1 million input and 1 million output tokens on Neon, which costs $18.00. If Carbon uses 1.6 times as many tokens for the same task, it costs $19.20 through November 30, 2026, so Neon is cheaper. After December 1, 2026, if Carbon uses only 0.7 times Neon's tokens, Carbon costs $16.80 against $18.00 and Carbon is cheaper.

Google's launch post reports that at default high thinking Neon delivers a 32 percent lower cost per task than Carbon. It also reports an Artificial Analysis Intelligence Index score of 61 at xhigh thinking, third overall, and roughly 50 percent to 80 percent less cost per task than top competing frontier models at the high setting. Google's launch post does not state the token or price basis for the 32 percent figure. Treat it as a vendor claim, not a modeling constant.

## What the Price Does Not Include

Gemini 4 Neon bills all thinking tokens at the output rate of $15.00 per 1 million tokens.

Token totals also include internal tool system instructions, schema definitions and function declarations sent during tool calls. Grounding with Google Search or Google Maps adds the $14 per 1,000 request fee after the 5,000 free monthly requests. The Priority tier carries an 80 percent premium, at $5.40 input and $27.00 output per 1 million tokens. Same-token price math ignores latency, accuracy and token-generation overhead.

## Who Should Not Rely on This Math

Teams whose tokens per task vary widely across models should not choose on per-token price alone. If another model solves a workflow in half as many reasoning steps, its higher rate can still give a smaller invoice. Measure real token use on a sample set before you commit to a routing design.

Developers who need a free tier should not choose Neon, because Google offers none on the Gemini API. For free testing or low-volume prototyping, review Gemini 4 Hydrogen pricing to use its free tier. Teams that need explicit context caching must look elsewhere too, since Neon supports implicit caching only.

## What Would Change Our Answer

Our cost comparison hinges on the price step on December 1, 2026. On that date Carbon and Argon rise from $2.00 input and $10.00 output to $4.00 and $20.00 per 1 million tokens. Neon then becomes the cheaper option per token.

Any price revision by Google on Neon, Carbon, Argon or Hydrogen would change these break-even ratios. So would a change to the 95 percent implicit caching discount, or an extension of Carbon's introductory pricing past November 30, 2026. Rates here are as published November 18, 2026, and we review them on December 1, 2026.

## Our Takeaway

In our internal cost modeling from published rates at identical token counts, Gemini 4 Neon costs more per token than Carbon and Argon through November 30, 2026, and less from December 1, 2026. Hydrogen costs exactly half of Neon on every date. Actual spend depends on how many thinking and output tokens each model uses per task.

Use the calculator in the price math section above with your own token mix before routing production traffic. It prices the Standard tier only.

## Frequently Asked Questions

- No. Gemini 4 Neon is not available on the Gemini API free tier. Every token is billed on the paid tiers (Standard, Batch, Flex or Priority). Standard pricing is $3.00 per 1 million input tokens and $15.00 per 1 million output tokens.
- Cached input on Gemini 4 Neon costs $0.15 per 1 million tokens on the Standard tier, a 95 percent discount off input pricing. The discount applies through implicit caching, since explicit caching is not supported.
- Neon is more expensive per token than Carbon through November 30, 2026 and cheaper from December 1, 2026. Through November 30, Neon costs $3.00 input and $15.00 output per 1M tokens against Carbon's introductory $2.00 and $10.00. On December 1, Carbon rises to $4.00 and $20.00, so Neon is 25 percent cheaper per token at identical volumes.
- On the Batch tier Gemini 4 Neon costs $1.50 per 1 million input tokens, $7.50 per 1 million output tokens and $0.075 per 1 million cached input tokens. That is 50 percent off Standard pricing.
- No. Neon's published base rates do not change on December 1, 2026. Introductory pricing ends for Carbon and Argon that day, which doubles their rates and makes Neon cheaper than both per token.

### Optimize Your Gemini 4 Neon Token Spend

Choosing the right tier, caching what repeats and routing each task to the cheapest model that handles it all lower the bill. Book a consultation with Layer3Labs to audit your model pipeline and lower production API bills.

Book a Consultation
