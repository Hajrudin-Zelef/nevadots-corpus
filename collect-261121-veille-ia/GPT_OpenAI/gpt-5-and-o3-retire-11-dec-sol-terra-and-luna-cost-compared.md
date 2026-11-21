---
id: collect-261121-veille-ia/gpt-openai/gpt-5-and-o3-retire-11-dec-sol-terra-and-luna-cost-compared
title: "GPT-5 and o3 retire 11 Dec: Sol, Terra and Luna cost compared"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2025-08-07"]
keywords: ["gpt-6", "gpt-5", "model", "terra", "openai", "deprecations", "luna", "page"]
source: https://ecorpit.com/gpt-5-o3-retirement-gpt-6-1-sol-terra-luna-cost-comparison/
source_anchor: ""
source_lines: [1, 188]
section: "GPT_OpenAI"
---
**Summary.** OpenAI's deprecations page, checked again on 20 November 2026, lists 11 December 2026 as the shutdown date for gpt-5, gpt-5-mini, gpt-5-nano, gpt-5-pro, o3 and o3-pro, 21 days from today. The named replacements are gpt-6.1-sol for gpt-5 and o3, gpt-6.1-sol in pro reasoning mode for both Pro models, gpt-6.1-terra for gpt-5-mini and gpt-6-luna for gpt-5-nano. GPT-6.1 Sol costs $2 input and $10 output per million tokens, GPT-6.1 Terra $1 and $5, and GPT-6 Luna $0.10 and $0.50. Set against today's rates, the move is cheap for some workloads and expensive for others. In our model, a gpt-5 chat workload with a cached prefix rises about 9% to 15% on Sol, and o3 analysis rises about 15%. gpt-5-mini traffic costs 3.3 times as much on Terra, yet a third as much on Luna. Pro workloads fall to a fraction of their old per-token rate, but pro mode uses more tokens by an amount OpenAI does not publish. Our verdict: do not swap model strings one for one. Price each workload against two targets, and finish the rollout by 4 December.

**About this article.** For engineering leads, platform teams and finance owners in India, the UK, the EU and the US who run OpenAI API workloads on any of the six retiring models. On 18 November 2026 we checked every date, price, rate limit and parameter against OpenAI's deprecations page, pricing page, eleven model pages and its reasoning, GPT-6, prompt caching, Batch and Flex guides, plus Microsoft's Foundry retirement schedule and the ECB reference rates. Dates and replacements were re-checked on 20 November 2026. The cost models are our own illustrations at list prices; your token counts will differ, and vendors change prices often. This is not financial advice, and no vendor paid for inclusion. Researched with AI assistance and checked against the primary sources listed in the references.

## What stops working on 11 December 2026

All six models sit in the upcoming deprecations table with a notice date of 11 June 2026 and a shutdown date of 11 December 2026, and fine-tuned versions follow their base model's schedule. Each alias resolves to one dated snapshot, so `gpt-5` and `gpt-5-2025-08-07` fail on the same day.

| Retiring model | Snapshot | Price per 1M tokens (input, cached, output) | Named replacement | 
|---|---|---|---|
| gpt-5 | gpt-5-2025-08-07 | $1.25, $0.125, $10 | gpt-6.1-sol | 
| gpt-5-mini | gpt-5-mini-2025-08-07 | $0.25, $0.025, $2 | gpt-6.1-terra | 
| gpt-5-nano | gpt-5-nano-2025-08-07 | $0.05, $0.005, $0.40 | gpt-6-luna | 
| gpt-5-pro | gpt-5-pro-2025-10-06 | $15, none, $120 | gpt-6.1-sol, pro mode, Responses API | 
| o3 | o3-2025-04-16 | $2, $0.50, $8 | gpt-6.1-sol | 
| o3-pro | o3-pro-2025-06-10 | $20, none, $80 | gpt-6.1-sol, pro mode, Responses API | 

Snapshots and replacements come from the deprecations table, and prices from the model pages. One inconsistency is worth knowing. The gpt-5 model page recommends GPT-6.2 Astra, a far more expensive model at $8 and $40, while the deprecations table names GPT-6.1 Sol. The gpt-5-mini and gpt-5-nano pages agree with the table and point to GPT-6.1 Terra and GPT-6 Luna. We treat the deprecations table as the default target.

OpenAI's policy promises at least six months' notice for generally available models, and this notice gave exactly six. The same page says some developers may be able to provision dedicated capacity after shutdown through sales; treat that as an exception path, not a plan. Our earlier GPT-5 and o3 shutdown migration guide covers the behaviour changes in depth. This article concentrates on the money.

## The replacement price sheet

In the four standard-mode moves OpenAI names, every target costs more per input token than the model it replaces, except Sol against o3, which matches it. Output rates move less.

| Move | Input rate change | Cached read change | Output rate change | 
|---|---|---|---|
| gpt-5 to gpt-6.1-sol | $1.25 to $2 (1.6x) | $0.125 to $0.10 (0.8x) | $10 to $10 (same) | 
| o3 to gpt-6.1-sol | $2 to $2 (same) | $0.50 to $0.10 (0.2x) | $8 to $10 (1.25x) | 
| gpt-5-mini to gpt-6.1-terra | $0.25 to $1 (4x) | $0.025 to $0.05 (2x) | $2 to $5 (2.5x) | 
| gpt-5-mini to gpt-6-luna | $0.25 to $0.10 (0.4x) | $0.025 to $0.01 (0.4x) | $2 to $0.50 (0.25x) | 
| gpt-5-nano to gpt-6-luna | $0.05 to $0.10 (2x) | $0.005 to $0.01 (2x) | $0.40 to $0.50 (1.25x) | 
| gpt-5-pro to gpt-6.1-sol pro | $15 to $2 | none to $0.10 | $120 to $10 | 
| o3-pro to gpt-6.1-sol pro | $20 to $2 | none to $0.10 | $80 to $10 | 

All rates above are Standard, short-context list prices from OpenAI's pricing page and model pages. Three billing rules on the new models change the arithmetic.

**Cache writes now cost money.** On GPT-5.6 and later models, writing a prompt prefix to cache costs 1.25 times the uncached input rate for the default 30-minute lifetime, and 2.5 times for a 12-hour lifetime in the Responses API. The retiring models carry no extra cache-write charge. GPT-6.1 Sol writes at $2.50 and GPT-6.1 Terra at $1.25 per million tokens on the 30-minute setting. Reads are cheap: 5% of the input rate on Sol and Terra and 10% on Luna.

**Long prompts cost double.** Requests above 272,000 input tokens are billed at twice the input and cache rates and 1.5 times the output rate for the whole request. This does not touch a straight migration, because gpt-5, mini and nano accept at most 272,000 input tokens and o3 has a 200,000-token window. It bites only if you use the new 1,050,000-token window (922,000 input tokens) to send more context after moving.

**Pro mode bills more tokens at lower rates.** The reasoning guide says pro mode adds up the model work behind the final answer, bills it at the selected model's standard token rates, and does more work than standard mode. The per-token price drops sharply; the token count rises by an amount you have to measure.

Two other options sit beside the named targets. GPT-6.2 Sol lists at the same $2, $0.10 and $10 as GPT-6.1 Sol and also rejects `none` reasoning effort. GPT-6 Sol costs $2 and $10 too, but its cached read is $0.20 and it accepts `none` effort. Neither is the named replacement, so test them only if the named target falls short.

## Worked cost comparison by workload

These illustrations hold token counts constant across models and use Standard list prices. That is the honest baseline, not the likely outcome: newer models use different numbers of reasoning tokens for the same task, so measure your own.

### Workload A: gpt-5 support assistant

300,000 requests a month. Each request sends a 3,000-token cached system prompt and tool list, 1,000 new input tokens and 600 output tokens including reasoning.

- gpt-5 today: 3,000 Ã $0.125 + 1,000 Ã $1.25 + 600 Ã $10, divided by one million, is $0.007625 a request, or $2,287.50 a month.

- gpt-6.1-sol, if the 1,000 new tokens are billed as plain input: 3,000 Ã $0.10 + 1,000 Ã $2 + 600 Ã $10 gives $0.0083 a request, or $2,490 a month.

- gpt-6.1-sol, if all 1,000 new tokens are written to cache at $2.50: $0.0088 a request, or $2,640 a month.

The rise is 8.9% to 15.4%. In explicit caching mode, content after the last breakpoint is billed as plain input with no write charge, which pushes you towards the low end. Our guide to OpenAI cache-write costs shows how to read the usage numbers.

### Workload B: gpt-5-mini document extraction

2 million requests a month, 2,500 input tokens and 300 output tokens each, no caching.

| Target | Arithmetic per million requests | Monthly cost | Against today | 
|---|---|---|---|
| gpt-5-mini today | 2,500 Ã $0.25 + 300 Ã $2 = $1,225 | $2,450 | Baseline | 
| gpt-6.1-terra, named | 2,500 Ã $1 + 300 Ã $5 = $4,000 | $8,000 | 3.3x | 
| gpt-6.1-terra on Batch | half of $8,000 | $4,000 | 1.6x | 
| gpt-6-luna | 2,500 Ã $0.10 + 300 Ã $0.50 = $400 | $800 | 0.33x | 
| gpt-6-luna on Batch | half of $800 | $400 | 0.16x | 

The Terra and Luna model pages put Batch at half the Standard rate. The pricing page also lists discounted background completion windows for Terra and Luna, 30% below Standard for a 2-hour window and 40% below for 8 hours, in public beta. This is the workload where the named target hurts most. Terra is pitched at visual reasoning, computer use and natural writing, while Luna is described as OpenAI's most efficient model for focused, high-volume tasks. For field extraction and classification, Luna at `none` effort is the first thing to test.

### Workload C: gpt-5-nano classifier

5 million requests a month, 800 input tokens and 20 output tokens each.

- gpt-5-nano today: 800 Ã $0.05 + 20 Ã $0.40 = $48 per million requests, or $240 a month.

- gpt-6-luna Standard: 800 Ã $0.10 + 20 Ã $0.50 = $90 per million requests, or $450 a month, 1.9 times today.

- gpt-6-luna on Batch or Flex at half price: $225 a month, slightly below today.

Batch completes within 24 hours. Flex bills Batch rates on ordinary requests in exchange for slower responses and occasional 429 resource-unavailable errors, which are not charged. Any classifier that does not need an instant answer belongs on one of the two.

### Workload D: o3 analysis jobs

20,000 requests a month, 10,000 input tokens and 4,000 output tokens each, no caching.

- o3 today: 10,000 Ã $2 + 4,000 Ã $8 = $52,000 per million requests, or $1,040 a month.

- gpt-6.1-sol: 10,000 Ã $2 + 4,000 Ã $10 = $60,000 per million requests, or $1,200 a month, 15.4% more.

Input costs are identical, so the whole gap is output. Sol breaks even if it finishes the same job with 3,200 output tokens instead of 4,000, 20% fewer. Measure that on your own prompts before you budget the increase.

### Pro workloads: the break-even multiplier

Take 1,000 hard requests a month at 20,000 input and 15,000 output tokens each.

| Model | Arithmetic per request | Monthly cost | 
|---|---|---|
| o3-pro today | 20,000 Ã $20 + 15,000 Ã $80 = $1.60 | $1,600 | 
| gpt-5-pro today | 20,000 Ã $15 + 15,000 Ã $120 = $2.10 | $2,100 | 
| gpt-6.1-sol pro, same tokens | 20,000 Ã $2 + 15,000 Ã $10 = $0.19 | $190 | 

The table uses the model pages for o3-pro and gpt-5-pro and the GPT-6.1 Sol rates, which the reasoning guide says pro mode bills at. If pro mode multiplies every token by the same factor, o3-pro users break even at 8.4 times the tokens and gpt-5-pro users at 11.1 times. OpenAI publishes no multiplier, so treat these as ceilings to test against, not forecasts.

## A portfolio view in dollars and rupees

Most teams run several of these workloads at once. Adding up A to D shows how much the choice of target matters. Rupee figures use the ECB reference rates of 17 November 2026 (US$1.0989 and â¹102.72 per euro), crossed to â¹93.48 per US dollar and rounded to two decimals.

| Plan | Monthly cost in USD | Monthly cost in INR | Against today | 
|---|---|---|---|
| Today on the retiring models | $6,017.50 | â¹5,62,516 | Baseline | 
| Named replacements, one for one | $12,140 to $12,290 | â¹11,34,847 to â¹11,48,869 | 2.0x | 
| Sol for A and D, Luna for B, Luna Batch for C | $4,715 to $4,865 | â¹4,40,758 to â¹4,54,780 | 0.78x to 0.81x | 

The gap between the two migration plans is roughly $7,400 a month at identical token counts, almost all of it from Workload B. The figures are built from the workload arithmetic above and exclude taxes. If you call OpenAI through a regional processing endpoint, add 10% for the GPT-6 targets: the pricing page applies that uplift to models released on or after 5 March 2026, and each new model page lists the 10% regional premium.

For routing that sends easy requests to Luna and escalates the rest, see our hybrid routing framework for LLM API spend.

## Rate limits: the hidden cost of Terra

Price is not the only ceiling. Tokens-per-minute limits by usage tier differ between old and new models.

| Model | Build tier TPM | Launch tier TPM | Grow tier TPM | 
|---|---|---|---|
| gpt-5-mini | 2,000,000 | 10,000,000 | 180,000,000 | 
| gpt-6.1-terra | 1,000,000 | 4,000,000 | 40,000,000 | 
| gpt-6-luna | 2,000,000 | 10,000,000 | 180,000,000 | 
| gpt-5 | 1,000,000 | 4,000,000 | 40,000,000 | 
| o3 | 450,000 | 2,000,000 | 30,000,000 | 
| gpt-6.1-sol | 1,000,000 | 4,000,000 | 40,000,000 | 

Limits come from each model page. A high-volume gpt-5-mini pipeline on the Grow tier loses more than three quarters of its per-minute headroom on Terra, so a job that ran cleanly can start returning 429 errors with no code change. Luna keeps the same limits as mini and nano. o3 users gain headroom on Sol. Batch jobs draw on a separate queued-token limit, which is another reason to move bulk work there.

## Request changes that affect cost and correctness

Changing the model string is the start, not the end. OpenAI's GPT-6 guide lists the checks.

- **Reasoning effort.** GPT-6.1 Sol does not accept`none` ; Terra and Luna do. If you use`minimal` on gpt-5 today, start at`low` and compare. GPT-6.1 Sol, GPT-6 Sol and GPT-6 Luna default to`medium` if you set nothing, and higher effort spends more output tokens.

- **Tool calls.** GPT-6.1 Sol and GPT-6.1 Terra need the Responses API for tool calling. GPT-6 Luna supports function calling in Chat Completions only at`none` effort.

- **Removed parameters.** When effort is above`none` , drop`temperature` ,`top_p` and`top_logprobs` , and`logprobs` in Chat Completions.

- **Caching parameters.** Replace`prompt_cache_retention` with`prompt_cache_options.ttl` set to`30m` . The minimum cacheable prefix on the new models is 1,024 visible input tokens.

- **Instructions in files.** The guide says GPT-6 Astra can be more sensitive to instructions in skills and files such as`AGENTS.md` , and more likely to stop and ask for clarification, and asks you to evaluate its prompts on whichever GPT-6 model you choose. In an unattended pipeline, a paused job is a failed job.

A minimal gpt-5 to GPT-6.1 Sol change in Python:

For gpt-5-pro and o3-pro, set `reasoning.mode` to `pro` and keep the Responses API, with background mode for long jobs.

To stop the next notice catching you out, the List models and Retrieve model endpoints return an optional `shutdown_date` field for each model. A nightly check is a few lines:

## A 21-day plan to 11 December

From 20 November there are 21 days left. Keep the final week free for rollback and stragglers.

| Dates | Work | Done when | 
|---|---|---|
| 20 to 21 Nov | Find every call to the six IDs and their snapshots in code, config, prompt files, CI and gateway routes; match against the usage dashboard by project | Owner and two candidate targets per workload | 
| 22 to 26 Nov | Run 100 to 300 real inputs per workload on the retiring model and both candidates; record quality, tokens split four ways, latency and cost | One target per workload, with a costed monthly forecast | 
| 27 Nov to 1 Dec | Fix prompts, schemas and tool definitions; shadow live traffic through your gateway; load-test against the target's TPM limit | No regression on shadow traffic | 
| 2 to 4 Dec | Canary at 5%, 25%, then 100%, behind a feature flag | All traffic on new targets | 
| 5 to 10 Dec | Remove old IDs, add a CI check that blocks them, add the shutdown_date alert, watch 429s and cache hit rates | No calls to retiring IDs in logs | 
| 11 Dec | Shutdown | Requests to the six models fail | 

The four token buckets to log are uncached input, cached input, cache writes and output including reasoning. The judging number is cost per successful task: a cheaper model that fails more often can cost more.

## Azure and other platforms

Azure runs on its own calendar. Microsoft's Foundry retirement schedule shows o3 and o3-pro retired on 19 November 2026, with gpt-5.6-sol as the Azure replacement. It lists gpt-5 and gpt-5-nano on 9 February 2027, gpt-5-pro on 7 April 2027 and gpt-5-mini on 13 July 2027. Teams that fail over between OpenAI and Azure need both dates in their plan; our Azure OpenAI GPT-5 retirement checklist covers the Azure side. OpenAI's next text-model removals after this one are GPT-4o, GPT-4o mini, GPT-4.1 and GPT-4.1 mini on 14 January 2027, all mapped to GPT-6 Luna.

## India-specific considerations

For Indian teams the conversion matters as much as the dollar rate. At â¹93.48 per dollar, Workload B alone moves from about â¹2.29 lakh a month on gpt-5-mini to â¹7.48 lakh on Terra, or â¹0.75 lakh on Luna. Budget owners will want the forecast before the invoice. Card-billed API usage is also exposed to currency movement, so build a margin into monthly forecasts.

If prompts carry personal data, the migration is a good moment to update your processing records under the Digital Personal Data Protection Act. Note which model, which endpoint and which region now handle the data, and whether the regional processing uplift applies. Re-run any redaction step against the new model's outputs, because output style changes between generations.

## How eCorpIT can help

eCorpIT is a Gurugram-based technology organisation, founded in 2021, assessed at CMMI Level 5 and MSME certified, with senior-led engineering teams working across AWS, Microsoft and Google platforms. Our LLM migration and cost optimisation service can inventory your OpenAI calls, test Sol, Terra and Luna on your own prompts, and route each workload to the cheapest target that passes. Talk to us at /contact-us/.

Last updated: 20 November 2026.
