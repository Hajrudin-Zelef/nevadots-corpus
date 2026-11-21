---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-1-api-pricing-what-changed-and-what-you-ll-pay
title: "Claude Fable 5.1 API Pricing: What Changed and What You'll Pay"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "claude", "cache", "reads", "anthropic", "tokens", "output", "token"]
source: https://blog.laozhang.ai/en/posts/claude-fable-5-1-api-pricing
source_anchor: ""
source_lines: [1, 186]
section: "Claude_Anthropic"
---
## On this page

As of September 25, 2026, Claude Fable 5.1 (`claude-fable-5-1`) on the Claude API costs **$10 per million input tokens and $50 per million output tokens**, with cache reads at **$0.25 per million**. Cache reads are the only rate that moved from Fable 5, where they were $1. So Fable 5.1 is cheaper than Fable 5 only in proportion to how much of your old bill was cache reads: a workload that rarely hits the cache pays almost the same, and a long-running agent that re-reads a large context on every turn can save a lot. Per token, Fable 5.1 is not the cheapest current Claude model. Opus 5.5, released September 22, 2026, charges 40% of Fable's rate for input and output.

**Update, November 10, 2026:** Anthropic released Claude Fable 5.2 (`claude-fable-5-2`) on November 9 at $8 input and $40 output per million tokens; Fable 5.1 stays at $10 / $50. Cache reads on Fable 5.2 cost $0.20 per million tokens. A request with 2,000 input and 500 output tokens costs $0.036 on Fable 5.2, against $0.045 on Fable 5.1. This page stays about Fable 5.1, which is still available. For the new model, see Claude Fable 5.2 vs Opus 5.5: When Fable High Beats Opus Max and Claude Fable 5.2 vs GPT-6 Astra vs GPT-6.2 Astra: Cost per Task.

## Fable 5.1 rates on the Claude API

These are Anthropic's first-party prices for standard global inference, in USD per million tokens (MTok), before tax:

| Token type | Fable 5.1 standard | Fable 5.1 Batch | Fable 5 standard | 
|---|---|---|---|
| Base input (uncached) | $10 | $5 | $10 | 
| 5-minute cache write | $12.50 | $6.25 | $12.50 | 
| 1-hour cache write | $20 | $10 | $20 | 
| Cache hits and refreshes | $0.25 | $0.125 | $1 | 
| Output (including thinking) | $50 | $25 | $50 | 

Sources: Claude API pricing and the Fable 5.1 announcement. The Batch column applies the 50% Batch discount to every row, because Anthropic states that caching multipliers stack with the Batch discount.

A few details decide which row a token lands in:

- **Cache reads are 2.5% of base input on Fable 5.1.** Most Claude models charge 10% for a cache hit, and Opus 5.5 charges 5%. That special multiplier is the whole price change.
- **The full 1M-token context is billed at the same per-token rate.** A 900,000-token request costs the same per token as a 9,000-token one. Maximum output on the synchronous Messages API is 128K tokens.
- **Thinking is always on and billed as output.** Fable 5.1 uses adaptive thinking with default effort`high` , and thinking text is hidden by default, so the visible reply understates what you pay for.
- **Claude Mythos 5.1 has identical prices** but is available by invitation only.

Fable 5.1 is also sold through Amazon Bedrock, Google Cloud, Microsoft Foundry and Claude Platform on AWS. Bedrock and Google Cloud set their own prices and invoice you directly, so check their pricing pages rather than applying the table above.

## Is Fable 5.1 cheaper than Fable 5?

Three percentages are attached to the Fable 5.1 price change, and each measures something different:

- **75%** is the cut to a single rate: cache reads dropped from $1 to $0.25 per MTok. It is not a cut to your bill.
- **About 25%** is Anthropic's estimate for "typical workloads," based on four weeks of actual usage in August 2026 across Claude Enterprise, Claude Code and the API, at default effort and usage-based pricing.
- **Up to about 45%** is Anthropic's estimate for context-heavy, tool-heavy agentic work "where cache reads make up most of the cost."

Both estimates are averages over Anthropic's own traffic. You can check what applies to you with one number. If your token counts stay the same, only the cache-read line changes, and it shrinks by 75%:

| Cache reads as a share of your Fable 5 bill | Expected saving on Fable 5.1 | 
|---|---|
| 0% (no caching, or caching that never hits) | 0% | 
| 10% | 7.5% | 
| About one third | 25%, Anthropic's typical case | 
| 60% | 45%, Anthropic's agentic case | 
| 80% | 60% | 

To find your share, take last month's Fable 5 usage, multiply the cache-read tokens by $1 per million, and divide by the month's Fable 5 token spend. If the answer is 5%, Fable 5.1 will save you under 4%.

The rule has one dependency: equal token counts. Fable 5 and Fable 5.1 share the same tokenizer, so input counts carry over, but output does not have to. Thinking volume depends on effort and on the model, and every thinking token is billed at $50 per MTok. Finout, citing a third-party Intelligence Index run at max effort, reports Fable 5.1 costing about 20% more per task than Fable 5 ($3.76 versus $3.14). That is one benchmark at one setting, but it shows how extra output can outweigh the cheaper cache reads. Compare `output_tokens` on a sample of your own requests before you assume the saving. For a per-task comparison against Opus 5, see Claude Fable 5.1 vs Opus 5: Pricing and Cost per Task.

## Turn one response's usage into dollars

Every Messages API response carries a `usage` block. Here is a hypothetical agent turn with a large cached context:

Read the fields this way:

- `input_tokens` counts only the tokens**after your last cache breakpoint** , the part that was neither written to nor read from the cache. It is not your total input.
- `cache_creation_input_tokens` counts tokens written to the cache. The`cache_creation` object splits the same tokens into 5-minute and 1-hour writes, and its two values add up to`cache_creation_input_tokens` . Price the split, not both.
- `cache_read_input_tokens` counts tokens served from the cache.
- Total input = `cache_read_input_tokens` +`cache_creation_input_tokens` +`input_tokens` , which is 131,000 here.
- `output_tokens` includes thinking, even when the thinking text was not returned to you.

The formula, with raw token counts and Fable 5.1 standard rates:

For the turn above:

| Line | Tokens | Rate per MTok | Cost | 
|---|---|---|---|
| Uncached input | 3,000 | $10 | $0.030 | 
| 5-minute cache write | 8,000 | $12.50 | $0.100 | 
| Cache reads | 120,000 | $0.25 | $0.030 | 
| Output | 4,000 | $50 | $0.200 | 
| **Total on Fable 5.1** |  |  | **$0.360** | 

The same usage on Fable 5 costs $0.450, because the 120,000 cache-read tokens cost $0.120 at $1 per MTok instead of $0.030. That is a 20% saving, and the rule from the previous section predicts it: cache reads were $0.12 of $0.45, or 26.7% of the old bill, and 0.75 × 26.7% = 20%.

Two common mistakes change this answer a lot. Pricing all 131,000 input tokens at $10 gives $1.51 instead of $0.36. Adding `cache_creation_input_tokens` on top of the 5-minute and 1-hour breakdown bills the writes twice and gives $0.46.

If you want to run it over logged responses, this Python function applies the same formula:

The field names above are the Claude API's. Amazon Bedrock reports usage under its own names, so map them first if you run Fable 5.1 there.

## From one request to a monthly estimate

A monthly figure is the per-request cost times volume, as long as the sample reflects real traffic. If the hypothetical turn above runs 20,000 times a month, the token bill is:

| Model | Per turn | 20,000 turns per month | 
|---|---|---|
| Fable 5 | $0.450 | $9,000 | 
| Fable 5.1 | $0.360 | $7,200 | 
| Opus 5.5, same token counts | $0.156 | $3,120 | 

A more reliable method is to add up `usage` across a full day of production logs and price the totals, because cache hit rates swing with traffic gaps and TTL expiry.

Before you have traffic, the token counting endpoint is free (rate-limited) and counts a request under the tokenizer of the model you pass. It returns an estimate of total input and does not apply caching logic, so it tells you prompt size, not how many tokens will be reads. Two sizing notes:

- Fable 5.1 uses the tokenizer introduced with Opus 4.7. The same text produces roughly 30% more tokens than on Sonnet 4.6 and earlier models, depending on content. Don't reuse token counts from an older model; count with `"claude-fable-5-1"` .
- Anthropic's rough guide for the current tokenizer is that 1M tokens is about 555,000 English words.

## 5-minute or 1-hour cache: when each pays off

A cache write costs more than plain input, so it only pays once the prefix is read back. For a prefix of P tokens that is written once and then read H times:

With a 100,000-token prefix, the 5-minute cache is already cheaper at the second use ($1.275 against $2.00), and the 1-hour cache wins from the third use ($2.05 against $3.00). This matches the pricing page's own rule: one read pays for a 5-minute write, two reads pay for a 1-hour write. After that, each extra read adds only $0.025 per 100,000 tokens.

The choice between the two TTLs comes down to the gaps between your requests:

- **Requests arrive more often than every 5 minutes:** use the 5-minute cache. Every hit refreshes the lifetime at no extra write charge, so it can stay warm all day.
- **Gaps run longer than 5 minutes but under an hour** (a user who replies slowly, a side agent that works for 10 minutes): use the 1-hour cache. One 1-hour write costs $20 per MTok, while two 5-minute writes cost $25, so the 1-hour option is cheaper as soon as it saves you one rewrite.
- **The lifetime starts when the request starts,** not when the response ends. If a response takes 4 minutes to stream, a 5-minute cache leaves you about a minute to send the next request.

Some conditions quietly turn reads back into full-price writes on Fable 5.1:

- Prefixes shorter than 512 tokens are not cached, and the API returns no error. If both `cache_creation_input_tokens` and`cache_read_input_tokens` are 0, nothing was cached.
- Changing tool definitions invalidates the whole cache. Turning web search or citations on or off invalidates the system prompt and messages.
- Changing `output_config.effort` invalidates the cached message blocks. Setting effort explicitly to its default is treated the same as omitting it and does not invalidate.
- To add an instruction mid-conversation, append a `{"role": "system"}` message to`messages` instead of editing the top-level`system` field; the cached prefix stays intact.
- A cache entry becomes available only once the first response begins. Parallel requests sent before then miss the cache, so send one request first and fan out once it starts responding.

To trace cache misses inside Claude Code specifically, see Claude Code Cache TTL and Token Usage.

## Batch API: half price if you can wait

The Message Batches API takes 50% off every token category on Fable 5.1: $5 uncached input, $6.25 for 5-minute writes, $10 for 1-hour writes, $0.125 for cache reads and $25 for output per MTok. Those are already the discounted rates, so don't halve them again.

Batch suits work where nobody is waiting on the answer: evaluation runs, classification, document extraction, nightly reports and bulk rewrites. Most batches finish within an hour, but results are only guaranteed after all requests complete or 24 hours pass, and requests still unprocessed after 24 hours expire. Keep interactive traffic on the synchronous API.

What to know before you move a job:

- **Limits:** up to 100,000 requests or 256 MB per batch. Streaming and`max_tokens: 0` cache pre-warming are not allowed.
- **Caching still works, less predictably.** Requests run concurrently and in any order, so cache hits are best effort. Anthropic suggests the 1-hour TTL for batches with shared context, since a batch can outlast a 5-minute cache.
- **Billing follows results.** Requests that end as`errored` ,`canceled` or`expired` are not billed. A`succeeded` request is billed even if the output fails your own quality check.
- **Reconcile by `custom_id`.** Results can arrive in any order and stay available for 29 days.

For the turn priced earlier, Batch cuts $0.360 to $0.180, provided the cache hits still land.

## Charges that sit on top of the token rates

- **US-only inference:** setting`inference_geo: "us"` on the Claude API or Claude Platform on AWS multiplies every token category by 1.1, including cache writes and reads. Multipliers stack, so a Batch cache read with US-only inference is $0.125 × 1.1 = $0.1375 per MTok, and Batch output is $27.50. Global routing is the default and carries no multiplier.
- **Bedrock and Google Cloud:** these providers set their own Fable 5.1 prices. For Claude 4.5 and later models, their regional and multi-region endpoints cost 10% more than their global endpoints. Don't apply the Claude API's 1.1× to those invoices.
- **Tools:** tool definitions,`tool_use` blocks and`tool_result` blocks all count as input tokens. Web search adds $10 per 1,000 searches plus the tokens it brings in. Web fetch has no fee beyond tokens. Per-search fees are not token prices, so the Batch discount formula above does not cover them.

## Fable 5.1 vs Opus 5.5 per token

| Per MTok, standard | Fable 5.1 | Opus 5.5 | Opus 5.5 as % of Fable 5.1 | 
|---|---|---|---|
| Base input | $10 | $4 | 40% | 
| 5-minute cache write | $12.50 | $5 | 40% | 
| 1-hour cache write | $20 | $8 | 40% | 
| Cache reads | $0.25 | $0.20 | 80% | 
| Output | $50 | $20 | 40% | 
| Batch input / output | $5 / $25 | $2 / $10 | 40% | 

Both have a 1M-token context window and 128K maximum output. Opus 5.5 defaults to `medium` effort and is faster; Fable 5.1 defaults to `high` and is the slower model.

On identical token counts, Opus 5.5 costs 40% as much for everything except cache reads, where it costs 80%. The more a workload leans on cache reads, the smaller Fable's per-token disadvantage becomes. Real task costs also depend on how many thinking tokens each model spends and how many retries it needs, and those differ by model.

Anthropic's guidance is to start with Opus 5.5 for most work and use Fable 5.1 for demanding reasoning and long-horizon tasks, or when Opus 5.5 at higher effort still falls short. If Fable's quality is what you need, the premium buys that; if Opus 5.5 passes your evaluations, it is the cheaper default. Further down the range, Sonnet 5 is $2/$10 and Haiku 4.5 is $1/$5, the cheapest current model on the list. For the model decision itself, see Claude Fable 5.1 vs Opus 5.5: Which Should You Use?, and for Opus 5.5 bill math, Claude Opus 5.5 Pricing and Limit Reset.

## FAQ

### Does the price cut change my Claude Pro or Max subscription?

No. Claude subscriptions and the Claude API are separate products with separate bills, so the cheaper cache reads change only usage-based API charges. For plan limits and extra usage credits, see Claude Fable 5.1 Usage Limits: Pro, Max, and Paid Credits.

### Is there a free Fable 5.1 API?

Anthropic's price list has no free rate for Fable 5.1; every token category is billed. The free part is token counting, which lets you size prompts before spending anything. Resellers that advertise Fable at "below Anthropic" prices are third parties, and their prices are not Anthropic's. To set up your own paid access, see How to Buy Claude API Access Safely.

### Do cache writes cost extra on Fable 5.1?

Yes. A 5-minute write costs $12.50 per MTok and a 1-hour write $20, versus $10 for uncached input. Writes pay off after one read (5-minute) or two reads (1-hour). After that, each read costs $0.25 per MTok.

### Do Batch and prompt caching discounts stack?

Yes. The Batch discount halves every category, including cache writes and reads, so a Batch cache read on Fable 5.1 costs $0.125 per MTok. Cache hits inside a batch are best effort, so check `cache_read_input_tokens` in the results rather than assuming them.

## ## Sources4

External pages this guide links to, in the order they appear. Last updated Nov 10, 2026.


## Sources4

External pages this guide links to, in the order they appear. Last updated Nov 10, 2026.
