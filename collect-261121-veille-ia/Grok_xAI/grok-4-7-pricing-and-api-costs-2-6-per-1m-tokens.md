---
id: collect-261121-veille-ia/grok-xai/grok-4-7-pricing-and-api-costs-2-6-per-1m-tokens
title: "Grok 4.7 Pricing and API Costs: $2/$6 per 1M Tokens"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-09-21"]
keywords: ["tokens", "grok", "prompt", "output", "input", "rate", "cached", "grok-4"]
source: https://codersera.com/blog/grok-4-7-pricing-api-costs-2026/
source_anchor: ""
source_lines: [1, 222]
section: "Grok_xAI"
---
**Quick answer.**Grok 4.7 costs $2.00 per million input tokens and $6.00 per million output tokens, with cached input at $0.50. Any request whose prompt reaches 200,000 tokens is billed at double those rates — all tokens, not just the overflow. Server-side tools like web search bill separately at $5 per 1,000 calls.

xAI shipped Grok 4.7 on 21 September 2026 with a headline about speed, not price: "twice as fast, at half the price of comparable models." The per-token rate didn't move — 4.7 is served at exactly the same price as Grok 4.6. What makes the real bill hard to predict is everything *around* that rate: a hard long-context cliff at 200,000 tokens, per-call fees on server-side tools, reasoning tokens billed at the output rate, and no Batch API support.

Everything below was read off xAI's own pricing page and grok-4.7 model page on 5 October 2026. Where a third-party listing disagrees, xAI's page wins.

## What does Grok 4.7 cost per million tokens?

Two rate rows, split by prompt size. Verified 5 October 2026 against docs.x.ai.

| Meter | Prompt under 200K tokens | Prompt at or above 200K | 
|---|---|---|
| Input | $2.00 / 1M | $4.00 / 1M | 
| Cached input | $0.50 / 1M | $1.00 / 1M | 
| Output (incl. reasoning) | $6.00 / 1M | $12.00 / 1M | 

And the operational envelope:

| Spec | Grok 4.7 | 
|---|---|
| Model ID | `grok-4.7` | 
| Context window | 500,000 tokens | 
| Knowledge cutoff | May 2026 | 
| Modalities | Text + image in, text out | 
| Reasoning effort | `low` ,`medium` ,`high` ,`xhigh` — default`high` | 
| Rate limit | 150 requests/sec, 50,000,000 tokens/min | 
| Batch API | Not supported | 
| Priority Processing | 2x standard token rates | 
| US regional endpoint | 1.1x global rates (10% premium) | 

Two things worth flagging. xAI publishes no separate max-output-tokens figure for 4.7 — the model page lists context and rate limits but no completion ceiling. (OpenRouter advertises 450,000 for `x-ai/grok-4.7`, which is exactly 90% of context and looks derived rather than quoted.)

And the **Batch API discount does not apply to Grok 4.7**. xAI's 20%-off batch tier covers `grok-4.3`, the two `grok-4.20-0309` variants and `grok-4.20-multi-agent-0309`; the 4.7 page says "Not supported" outright. For async, cost-sensitive work that's a real argument for routing to 4.3 instead.

## What happens when your prompt crosses 200,000 tokens?

This is the detail almost nobody covers, and it's the single biggest cost variable on Grok 4.7. xAI's wording is unambiguous: "requests whose prompt reaches the listed token threshold are billed at the higher rate for *all* tokens in the request."

Not the overflow. All of them. Including the output tokens, which jump from $6 to $12 per million even though output volume has nothing to do with why you tripped the threshold.

Here's what that does to two nearly identical requests:

| Request | Input | Output | Input cost | Output cost | Total | 
|---|---|---|---|---|---|
| A — just under | 199,000 | 2,000 | $0.398 | $0.012 | **$0.410** | 
| B — just over | 201,000 | 2,000 | $0.804 | $0.024 | **$0.828** | 

Two thousand extra input tokens — a 1% increase — doubles the invoice. At 1,000 requests a day that's $12,300 a month versus $24,840 a month for functionally the same work.

So treat 200,000 as a **hard architectural ceiling**, not a soft target. A retrieval step that trims 5,000 tokens of marginal context off a 203K prompt is the highest-ROI optimisation available on this model, and it's worth a token-counting guard in your request path to enforce it.

Prompt caching helps, but only within a tier. On that 199,000-token prompt with a 180,000-token cached system block, you pay $0.090 cached plus $0.038 fresh plus $0.012 output — $0.140 instead of $0.410. Push it to 201,000 and cached reads reprice to $1.00/1M, so the cached version costs $0.288. Caching never rescues you from the cliff; it just makes both sides cheaper.

## Do server-side tools cost extra on top of tokens?

Yes, and this is where token-only pricing tables quietly mislead. xAI bills its server-side tools per invocation, on top of every token the tool's results consume in your context:

| Tool | Tool name | Cost per 1,000 calls | 
|---|---|---|
| Web Search | `web_search` | $5 | 
| X Search | `x_search` | $5 per 1k posts, $10 per 1k profiles | 
| Code Execution | `code_execution` ,`code_interpreter` | $5 | 
| File Attachments | `attachment_search` | $5 | 
| Collections Search (RAG) | `collections_search` ,`file_search` | $2.50 | 
| Image Understanding | `view_image` | Token-based | 
| X Video Understanding | `view_x_video` | Token-based | 
| Remote MCP tools | Set by MCP server | Token-based | 

Half a cent per search sounds like rounding error until you count how many an agent fires. Take a research task: 20,000 input tokens, 3,000 output tokens, six web searches.

- Tokens: $0.040 input + $0.018 output = **$0.058**
- Tool calls: 6 × $0.005 = **$0.030**
- Actual total: **$0.088**

A token-only estimate undershoots that task by 52%. Tool fees scale with agent *behaviour* rather than content size, which makes them much harder to forecast than tokens — so cap invocations per task explicitly if anything calls `web_search` or `x_search` in a loop. Storage is metered too: files at $0.025/GiB/day, collections at $0.10/GiB/day, downloads at $0.20/GiB.

For how the tool and connector surface actually works in practice, see our guide to Grok Build skills and connectors.

## Do Grok 4.7's reasoning tokens bill at the output rate?

Yes. xAI's reasoning guide states that "the reasoning tokens are billed as part of your total consumption," and the API's usage object nests `reasoning_tokens` inside the output-token breakdown. Thinking tokens are completion tokens — $6 per million under the threshold, $12 above it — even though you may never show them to a user.

That makes `reasoning_effort` the most important cost knob on the model, and it defaults to `high`. The `xhigh` level (new since 4.6; 4.5 silently downgrades it to `high`) is where bills go strange.

Artificial Analysis publishes the number that matters here — not price per token but cost to run its whole Intelligence Index, which captures how verbose a model actually is. All figures below are **Intelligence Index v4.3.2** (October 2026), with the reasoning-effort tier named on every row, because the tier moves the cost per task by 3x on identical per-token pricing:

| Model (effort tier) | Intelligence Index v4.3.2 | Cost per Intelligence Index task | 
|---|---|---|
| **Grok 4.7 (Xhigh)** | **46.45** | **$3.74** | 
| Grok 4.7 (High) | 46.33 | $2.73 | 
| Grok 4.7 (Low — AA's default entry) | 42.22 | $1.25 | 
| Grok 4.6 (Xhigh) | 44.20 | $2.32 | 
| Grok 4.6 (High) | 44.31 | $1.86 | 
| GPT-6 Sol (Max) | 47.63 | $1.04 | 
| Claude Sonnet 5.5 (Max) | 56.00 | $7.67 | 

That inverts the per-token story. Grok 4.7 and 4.6 cost *identical* amounts per token, yet 4.7 at `xhigh` runs $3.74 a task against $1.86 for 4.6 at `high` — twice the money for 2.1 index points, bought by thinking a lot more. Compared like-for-like at `xhigh`, the gap narrows to $3.74 against $2.32 for 2.25 points. And GPT-6 Sol (Max), nominally more expensive on output ($10 versus $6), comes in at under a third of Grok 4.7's cost per task while scoring 1.2 points higher (47.63 against 46.45), because it emits far fewer tokens to get there.

**The cheapest row in that table is the one most people miss.** Grok 4.7 at `high` scores 46.33 against `xhigh`'s 46.45 — a 0.12-point difference, far inside the noise of a ten-eval composite — for **$2.73 per task instead of $3.74**. Stepping down one effort notch is a 27% unit-cost cut for no measurable quality loss, and AA's default Grok 4.7 entry (the `low` tier) is $1.25 a task at 42.22. Same $2/$6 per-token rate across all three. The whole cost swing is reasoning tokens, which is why you compare cost per completed task on your own evals and never price per token. Grok 4.7 at `xhigh` measures 83.27 output tokens/sec with 43.17 seconds to first answer token; the `low` tier answers in 4.96 seconds at 73.59 tok/s, so the latency bill tracks the token bill.

## What does Grok 4.7 actually cost per month on real workloads?

Three worked examples at standard (non-priority, global-endpoint) rates, 30-day months.

### 1. Coding assistant for one developer

150 requests/day, 25,000 input tokens each with a 70% cache hit rate, 1,800 output tokens.

- Cached input: 17,500 × $0.50/1M = $0.00875
- Fresh input: 7,500 × $2.00/1M = $0.015
- Output: 1,800 × $6.00/1M = $0.0108
- Per request: $0.0346 → **≈$155/month**

### 2. Long-document analysis, staying under the cliff

20 documents/day at 180,000 input tokens each, 4,000 output, no caching (every document is unique).

- Per document: $0.360 + $0.024 = $0.384 → **≈$230/month**
- Same pipeline at 210,000 tokens per document: $0.840 + $0.048 = $0.888 → **≈$533/month**

17% more content, 2.3x the bill. That's the cliff doing all the work.

### 3. Production research agent with live search

2,000 tasks/day, 20,000 input tokens (50% cached), 3,000 output, six `web_search` calls per task.

- Tokens: $0.005 + $0.020 + $0.018 = $0.043
- Tool calls: $0.030
- Per task: $0.073 → **≈$4,380/month** , of which**$1,800 is tool fees**

Forty-one percent of that invoice never appears in a token calculator.

## How does Grok 4.7 compare with GPT-6, Claude, GLM and DeepSeek on price?

Standard-tier list rates per 1M tokens, cross-checked against the vendors' pages and OpenRouter's model API on 5 October 2026.

| Model | Input | Output | Context | Long-context repricing | 
|---|---|---|---|---|
| **Grok 4.7** | $2.00 | $6.00 | 500K | 2x at 200K | 
| Grok 4.6 | $2.00 | $6.00 | 500K | 2x at 200K | 
| GPT-6 Sol | $2.00 | $10.00 | 1.05M | $4 / $15 at 272K | 
| GPT-6 Luna | $0.10 | $0.50 | 1.05M | $0.20 / $0.75 at 272K | 
| GPT-6 Astra | $10.00 | $50.00 | 1.05M | $20 / $75 at 272K | 
| Claude Opus 5.5 | $4.00 | $20.00 | 1M | None listed | 
| Claude Sonnet 5.5 | $2.00 | $10.00 | 1M | None listed | 
| GLM-5.3 Prime | $2.80 | $8.80 | 1M | None listed | 
| DeepSeek V4.1 Flash | $0.30 | $1.20 | 1.05M | None listed | 

On output tokens, Grok 4.7 is the cheapest frontier-tier model in this set — $6 against $10 for both GPT-6 Sol and Claude Sonnet 5.5, $20 for Opus 5.5, and $50 for Astra. That's a genuine advantage for output-heavy work: long code generation, document drafting, bulk rewriting.

It's not cheapest overall. GPT-6 Luna at $0.10/$0.50 is 20x cheaper on input and 12x on output; DeepSeek V4.1 Flash at $0.30/$1.20 is in the same neighbourhood. For classification, extraction or routing, Grok 4.7 is the wrong tool at any price — our comparison of the cheapest fast LLM APIs covers that end of the market.

Two structural notes against it: its long-context threshold fires earlier than OpenAI's (200K versus 272K), and unlike the GPT-6 family and Claude it has no batch discount at all. Sonnet 5.5 at batch rates is $1/$5, which undercuts Grok 4.7's standard rate outright on async workloads.

## Is Grok 4.7 included in SuperGrok, or do you need the API?

xAI's consumer plans, as listed on x.ai/pricing on 5 October 2026:

| Plan | Price | What's listed | 
|---|---|---|
| Free | $0/month | Web and X search, voice mode, connectors, "generous limits" | 
| SuperGrok | $30/month | Grok 4.6 model, bot access, connectors, elevated rate limits, image/video generation | 
| SuperGrok Plus | $100/month | Everything in SuperGrok, 1080p video, much higher chat/imagine/voice/build usage, faster responses, priority peak-time access, early features | 
| Enterprise | Custom | Custom rate limits, SSO/SCIM, data residency, volume pricing | 

The honest answer is a caveat rather than a confirmation: **xAI's own plan comparison still names Grok 4.6 as the SuperGrok model, not 4.7.** The 21 September launch post lists availability as Cursor, Grok Build (with free access), the Grok API, third-party coding platforms and model routers — it does not mention SuperGrok or X Premium at all. We could not verify a consumer tier that explicitly ships Grok 4.7, and won't assume one. Check the live plan page before paying for a subscription specifically to get it.

The arithmetic if you're choosing: $30/month of API spend buys roughly 5M input plus 1M output tokens at standard rates. Heavy interactive chat will exceed that and the subscription wins. Anything programmatic can only run through the API anyway.

## How do you cut your Grok 4.7 bill?

In rough order of how much money each one actually saves:

1. **Stay under 200,000 prompt tokens.** Nothing else here is worth half as much. One token over doubles everything.
2. **Lower `reasoning_effort`.** It defaults to`high` and reasoning tokens bill at the output rate. xAI recommends`low` for simple tool-calling and latency-sensitive paths; reserve`xhigh` for problems where you've measured that it changes the answer.
3. **Structure prompts for cache hits.** Cached input is $0.50 against $2.00 — a 75% discount. Stable system prompt and reference material first, variable part last.
4. **Cap tool invocations per task.** At $5 per 1,000 calls, an unbudgeted loop is the easiest way to double an agent's unit cost without noticing.
5. **Skip Priority Processing and the US endpoint unless you need them.** 2x and 1.1x respectively — latency and residency purchases, not capability ones.
6. **Route async work elsewhere.**`grok-4.3` is $1.25/$2.50 with 20% off on batch; Sonnet 5.5's batch rate of $1/$5 beats Grok 4.7's standard rate.
7. **Clean up stored files and collections.** $0.10/GiB/day is $36 per GiB per year for data nobody is querying.

## So is Grok 4.7 worth it?

Grok 4.7 is the cheapest frontier-tier model on *output* tokens in this field, so it earns its place on output-heavy work — code generation, long drafting, bulk transformation — where you keep prompts comfortably under 200K and reasoning effort off the default.

It's the wrong choice in three cases. If your prompts routinely exceed 200K, the 2x repricing of the whole request makes a 1M-context model with no cliff cheaper in practice. If your workload is async, the missing batch tier is a 20–50% penalty. And if you want the strongest answer per dollar rather than per token, Artificial Analysis has GPT-6 Sol (Max) scoring higher on Intelligence Index v4.3.2 — 47.63 against Grok 4.7 (Xhigh)'s 46.45 — at under a third of the cost per task. Before switching vendors, though, try Grok 4.7 at `high` rather than `xhigh`: same score inside noise, 27% less per task.

Benchmarks and capability detail are out of scope here by design — our complete Grok 4.7 guide covers those, the Grok 4.7 vs Grok 4.6 comparison covers whether the upgrade is worth it, and our Grok 4.6 pricing breakdown is the reference point if you're still on the previous model.

## FAQ

### How much does Grok 4.7 cost?

$2.00 per million input tokens, $0.50 per million cached input tokens, and $6.00 per million output tokens, for any request whose prompt is under 200,000 tokens. At or above 200,000 prompt tokens, every token in the request reprices to $4.00 input, $1.00 cached, and $12.00 output. Server-side tools bill separately per call.

### Is Grok 4.7 more expensive than Grok 4.6?

Not per token — both are $2.00 input and $6.00 output per million, with the same 200K threshold and the same 500,000-token context. xAI's launch post explicitly says 4.7 is served "at the same price and speed as Grok 4.6." Per completed task it can be considerably more expensive: on Intelligence Index v4.3.2 Artificial Analysis measures Grok 4.7 at `xhigh` effort at $3.74 per Index task against $1.86 for Grok 4.6 at `high` and $2.32 for Grok 4.6 at `xhigh`, because 4.7 generates far more reasoning tokens. Grok 4.7 at `high` lands at $2.73 for a near-identical score (46.33 versus 46.45).

### Does Grok 4.7 charge extra for live search?

Yes. Web Search bills at $5 per 1,000 calls on top of the tokens its results consume in your context. X Search is $5 per 1,000 posts or $10 per 1,000 profiles, Code Execution and attachment search are $5 per 1,000 calls each, and collections search (RAG) is $2.50 per 1,000 queries. On a search-heavy agent these fees commonly reach a third or more of total spend, so any cost model built on tokens alone will understate the bill.

### Is Grok 4.7 cheaper than Claude Sonnet 5.5?

At standard rates, yes on output: both are $2.00 per million input, but Grok 4.7 is $6.00 per million output against Sonnet 5.5's $10.00. The picture flips on async work, where Sonnet 5.5's Batch API rate of $1.00/$5.00 undercuts Grok 4.7's standard rate and Grok 4.7 has no batch tier at all. Sonnet 5.5 also scores higher on Artificial Analysis's Intelligence Index v4.3.2 — 56.00 at `max` effort against 46.45 for Grok 4.7 at `xhigh` — and has a 1M-token context with no long-context repricing. That score costs $7.67 per Index task against Grok 4.7's $3.74, so the quality is real and so is the bill.

### What is Grok 4.7's context window?

500,000 tokens, same as Grok 4.6 and 4.5. The practical working limit is lower: at 200,000 prompt tokens the whole request reprices to double rates, so the top 300,000 tokens of that window cost twice as much to use. xAI does not publish a separate maximum-output-token figure for the model. Knowledge cutoff is May 2026.

### Is Grok 4.7 included in SuperGrok?

We could not confirm that it is. As of 5 October 2026, xAI's plan comparison page lists SuperGrok ($30/month) with "Grok 4.6 model," and the Grok 4.7 launch post lists availability through Cursor, Grok Build, the Grok API, and third-party routers without mentioning any consumer subscription tier. Check the live plan page before subscribing specifically for 4.7 access.

### Does Grok 4.7 support the Batch API?

No. The model page says "Not supported," and xAI's 20% batch discount applies only to `grok-4.3`, the two `grok-4.20-0309` variants, and `grok-4.20-multi-agent-0309`. For async or overnight workloads, `grok-4.3` at $1.25/$2.50 standard with a batch discount on top is substantially cheaper than Grok 4.7.

### Do reasoning tokens cost extra on Grok 4.7?

They are not a separate meter, but they are not free either — reasoning tokens are counted as output tokens and billed at the output rate ($6 or $12 per million depending on prompt size). Because `reasoning_effort` defaults to `high`, most users are paying for more thinking than their task needs. Setting it to `low` or `medium` on simple calls is the second-largest cost lever after keeping prompts under 200K.
