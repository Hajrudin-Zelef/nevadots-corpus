---
id: collect-261121-veille-ia/claude-anthropic/claude-haiku-vs-sonnet-5-5-is-20x-cheaper-good-enough
title: "Claude Haiku vs Sonnet (5.5): Is 20x Cheaper Good Enough?"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["sonnet", "haiku", "claude", "tokens", "anthropic", "effort", "model", "benchmark"]
source: https://www.respan.ai/blog/claude-3-5-sonnet-vs-claude-3-5-haiku
source_anchor: ""
source_lines: [1, 235]
section: "Claude_Anthropic"
---
Anthropic released Claude Haiku 5.5 on October 7, nine days after Sonnet 5.5. It's the first Haiku-class model with an adjustable effort setting, and it now matches Sonnet's 1M-token context window and 128K max output.

If you run high-volume calls through Claude, the choice between the two now moves your bill more than it used to, because Haiku 5.5 dropped its price sharply while Sonnet 5.5 kept its lead on quality.

Sonnet 5.5 is still the stronger model on every benchmark Anthropic published. For most high-volume work, though, Haiku 5.5's pricing makes it the model to start with, and Sonnet earns its cost on the hard coding and agent tasks where the quality gap shows.

So the Haiku vs Sonnet question comes down to whether the much cheaper model is good enough for the work you send it.

## TL;DR: Is Haiku better than Sonnet?

**Sonnet 5.5 is the better model on every benchmark, but Haiku 5.5 costs up to 20x less and is good enough for most high-volume work, so send hard coding and agent tasks to Sonnet and high-volume, speed-sensitive calls to Haiku.**

The differences that should drive the decision:

- **Price** - Haiku 5.5 costs $0.10 per million input tokens and $0.50 per million output tokens on prompts up to 100K tokens, a twentieth of Sonnet 5.5's $2 and $10. Above 100K, the gap narrows to 4x.
- **Quality** - Sonnet 5.5 leads on every benchmark in Anthropic's Haiku 5.5 announcement, by as much as 31 points on Terminal-Bench 4.0.
- **Speed** - Anthropic ranks Haiku 5.5 as its fastest model, and Artificial Analysis measures it at 258 tokens per second against 155 for Sonnet 5.5, both at max effort.
- **Defaults** - Haiku 5.5 runs at medium effort out of the box, while Sonnet 5.5 defaults to high.

Most of the remaining spec sheet is identical, which keeps the Sonnet vs Haiku decision focused on price and quality. To see which of your calls Haiku can handle, Respan's AI gateway puts both models behind one API and logs each request with its cost, so you can compare Haiku's outputs against Sonnet's on your own traffic before moving anything over.

## Comparison methodology

Benchmark scores, pricing, and speed rankings come from Anthropic's own announcement and documentation, and each figure is attributed in its section. Throughput comes from Artificial Analysis, which measures both models at max effort through Anthropic's API.

Results from Respan's earlier hands-on testing of the Claude 3.5 models appear in the older-models section for context.

## Basic comparison

|  | Claude Haiku 5.5 | Claude Sonnet 5.5 | 
|---|---|---|
| Input | $0.10 / 1M tokens ($0.50 over 100K) | $2.00 / 1M tokens | 
| Output | $0.50 / 1M tokens ($2.50 over 100K) | $10.00 / 1M tokens | 
| Context window | 1M | 1M | 
| Max output tokens | 128K | 128K | 
| Supported inputs | Text and images | Text and images | 
| Default effort | Medium | High | 
| Knowledge cutoff | June 2026 | June 2026 | 
| Release date | October 7, 2026 | September 28, 2026 | 

On paper the two models share almost every limit, so the comparison comes down to the pricing rows and what Sonnet's extra cost buys in output quality.

## Haiku 5.5 vs Sonnet 5.5: Benchmark comparison

Anthropic's Haiku 5.5 announcement reports scores for both models across eight benchmarks covering coding, reasoning, computer use, and knowledge work.

| Benchmark | Claude Haiku 5.5 | Claude Sonnet 5.5 | 
|---|---|---|
| Terminal-Bench 4.0 | 39.2% | 70.6% | 
| FrontierCode 1.1 (Main) | 46.4% | 52.1% (Xhigh effort) | 
| OSWorld 2.1 (offline subset) | 72.4% | 83.9% | 
| Humanity's Last Exam (no tools) | 45.9% | 56.9% | 
| Humanity's Last Exam (with tools) | 57.4% | 64.5% | 
| Chartography (no tools) | 46.4% | 61.6% | 
| GDPval-AA v2.1 | 1620 | 1840 | 
| AA-Briefcase v1.1 | 1578 | 1824 | 

Sonnet 5.5 leads on all eight. The widest gap is on Terminal-Bench 4.0, and the narrowest is on FrontierCode 1.1, although Anthropic ran Sonnet at its Xhigh effort level there, so that 5.7-point gap isn't a like-for-like comparison.

On OSWorld 2.1, a computer-use benchmark, Haiku 5.5 reaches 72.4%, which puts it within 12 points of Sonnet at a fraction of the price.

## Haiku 5.5 vs Sonnet 5.5: Speed comparison

The effort setting shapes speed on both models, because effort controls how much the model thinks before it answers. Haiku 5.5 and Sonnet 5.5 ship with different defaults, which tilts out-of-the-box speed toward Haiku.

### Generation time

No end-to-end generation time has been published for either model yet. What is documented is the starting effort level: Haiku 5.5 defaults to medium effort, and Sonnet 5.5 defaults to high. At lower effort the model thinks less and can skip thinking on simple requests, so Haiku should finish typical calls sooner without any tuning. The same default helps time to first token, the delay users notice first in chat and streaming responses, since a model that thinks less starts answering sooner.

**Haiku 5.5:** Medium effort by default

**Sonnet 5.5:** High effort by default

**Winner:** Haiku 5.5

### Speed (tokens per second)

Artificial Analysis measures Haiku 5.5 at 258 output tokens per second and Sonnet 5.5 at 155, both running at max effort through Anthropic's API. That makes Haiku about 1.7x faster at generating output, which lines up with Anthropic's model overview, where Haiku 5.5 is ranked "fastest" and Sonnet 5.5 "fast."

**Haiku 5.5:** 258 tokens per second

**Sonnet 5.5:** 155 tokens per second

**Winner:** Haiku 5.5

Haiku 5.5 wins every speed measure available so far. If you need Sonnet's quality with less wait, lowering Sonnet's effort level is worth testing before you switch models.

## Haiku 5.5 vs Sonnet 5.5: Performance comparison

### Coding

Sonnet 5.5 scores 70.6% on Terminal-Bench 4.0, which tests agentic coding in a terminal, against 39.2% for Haiku 5.5. Anthropic itself writes that Sonnet 5.5 and Opus 5.5 remain better choices for complex agentic coding. On FrontierCode 1.1 the gap shrinks to 46.4% versus 52.1%, with Sonnet running at Xhigh effort.

**Haiku 5.5:** 39.2% Terminal-Bench 4.0, 46.4% FrontierCode 1.1

**Sonnet 5.5:** 70.6% Terminal-Bench 4.0, 52.1% FrontierCode 1.1 (Xhigh effort)

**Winner:** Sonnet 5.5

### Logical reasoning

On Humanity's Last Exam without tools, Sonnet 5.5 scores 56.9% to Haiku's 45.9%. Giving both models tools narrows the gap from 11 points to about 7 (64.5% versus 57.4%), so a Haiku setup with search or code execution recovers part of the difference.

**Haiku 5.5:** 45.9% without tools, 57.4% with tools

**Sonnet 5.5:** 56.9% without tools, 64.5% with tools

**Winner:** Sonnet 5.5

### Agentic tasks

Computer use is a strong area for Haiku 5.5 relative to its price. It scores 72.4% on OSWorld 2.1 to Sonnet's 83.9%, and Anthropic positions it for browser use. Sonnet keeps clear leads on the knowledge-work benchmarks, scoring 1840 to 1620 on GDPval-AA v2.1 and 1824 to 1578 on AA-Briefcase v1.1.

**Haiku 5.5:** 72.4% OSWorld 2.1, 1620 GDPval-AA v2.1

**Sonnet 5.5:** 83.9% OSWorld 2.1, 1840 GDPval-AA v2.1

**Winner:** Sonnet 5.5

## Haiku 5.5 vs Sonnet 5.5: Price comparison

### Standard pricing (prompts up to 100K tokens)

On prompts up to 100K tokens, Haiku 5.5 costs $0.10 per million input tokens and $0.50 per million output tokens, according to Anthropic's pricing page. Sonnet 5.5 costs $2 and $10, so on Haiku vs Sonnet pricing for prompts in this range, Haiku comes in at a twentieth of the price on both input and output.

Haiku 5.5 is also cheaper than its own predecessor, since Anthropic says it costs around 75% less to run than Haiku 4.5 on average.

**Haiku 5.5:** $0.10 input / $0.50 output per 1M tokens

**Sonnet 5.5:** $2 input / $10 output per 1M tokens

**Winner:** Haiku 5.5, at a twentieth of the price

### Long-prompt pricing (prompts over 100K tokens)

Once a prompt crosses 100K tokens, Haiku 5.5 moves to $0.50 input and $2.50 output. Sonnet 5.5 has no long-prompt rate and charges $2 and $10 across its full 1M context, so the gap narrows from 20x to 4x.

That makes prompt length worth tracking if you run RAG pipelines with large retrieved context or agents with long transcripts, because a Haiku request that crosses the line costs five times as much per token as one just under it.

**Haiku 5.5:** $0.50 input / $2.50 output per 1M tokens

**Sonnet 5.5:** $2 input / $10 output per 1M tokens

**Winner:** Haiku 5.5, at a quarter of the price

### Prompt caching and batch pricing

Cache reads cost $0.01 per million tokens on Haiku 5.5 for prompts up to 100K, and $0.10 on Sonnet 5.5. Sonnet's cache-read rate is 0.05x its base input price while Haiku's is 0.1x, so the gap on cached tokens is 10x rather than 20x, which matters if your workload leans heavily on Claude prompt caching.

Five-minute cache writes cost $0.125 on Haiku and $2.50 on Sonnet. The Batch API takes 50% off input and output on both models, bringing Haiku to $0.05 and $0.25 and Sonnet to $1 and $5.

**Haiku 5.5:** $0.01 cache reads, $0.05 / $0.25 batch

**Sonnet 5.5:** $0.10 cache reads, $1 / $5 batch

**Winner:** Haiku 5.5

## Which to use: Haiku 5.5 or Sonnet 5.5?

### When to use Haiku 5.5

Anthropic built Haiku 5.5 for high-volume, cost-sensitive tasks such as summaries, context compaction, database queries, and classification. It also fits subagent roles alongside Sonnet or Opus in coding workflows, and its OSWorld score makes it a reasonable pick for browser automation.

Haiku is the model to start from when requests are short, frequent, and easy to check, since that's where the full 20x price gap applies and a weaker answer is cheap to catch.

### When to use Sonnet 5.5

Sonnet 5.5 earns its price on complex coding and multi-step agent work, where its lead on Terminal-Bench and Humanity's Last Exam is likely to mean fewer failed runs. It also makes more sense for long-context work than the headline price gap suggests, since past 100K tokens Haiku's advantage shrinks to 4x.

In practice, when to use Haiku vs Sonnet doesn't have to be one decision for the whole app. Splitting traffic by task lets you run Sonnet where quality matters and Haiku everywhere else.

## Comparing older Haiku and Sonnet models

Haiku and Sonnet are the two lower-cost tiers in Anthropic's lineup, with Opus and Fable above them, so the best Claude model for frontier reasoning work may be neither of these. Within the two lines, earlier versions compare on price like this:

| Model | Input (per 1M) | Output (per 1M) | Status | 
|---|---|---|---|
| Claude Haiku 5.5 | $0.10 ($0.50 over 100K) | $0.50 ($2.50 over 100K) | Current | 
| Claude Sonnet 5.5 | $2.00 | $10.00 | Current | 
| Claude Sonnet 5 | $2.00 | $10.00 | Available | 
| Claude Sonnet 4.6 | $3.00 | $15.00 | Available | 
| Claude Sonnet 4.5 | $3.00 | $15.00 | Deprecated, retirement planned for Nov 30, 2026 | 
| Claude Haiku 4.5 | $1.00 | $5.00 | Available | 
| Claude Haiku 3.5 | $0.80 | $4.00 | Retired Feb 19, 2026 | 

Per-token prices across generations aren't a clean comparison, because Haiku 5.5 uses the newer tokenizer shared by Claude 4.7 and later models. According to the Haiku 5.5 migration guide, the same text produces about 30% more tokens on Haiku 5.5 than on Haiku 4.5, so the savings from upgrading are smaller than the list prices suggest.

If you're still on Sonnet 4.5, a migration is coming either way, and Anthropic recommends Sonnet 5.5 as its replacement.

### What Respan's Claude 3.5 testing found

When Claude 3.5 Haiku and Claude 3.5 Sonnet were current, Respan tested both across multiple requests. The speed gap was small: Haiku averaged 13.98 seconds per request to Sonnet's 14.17, generated 52.54 tokens per second to Sonnet's 50.88, and returned its first token in 0.36 seconds to Sonnet's 0.64.

On quality, Sonnet came out ahead in coding and logical reasoning, and its writing read more naturally. Haiku handled simple frontend and backend tasks but tended to hallucinate on code longer than about 150 lines, and it was more likely to hallucinate on tricky questions or long inputs.

The 5.5 generation keeps the same quality ordering, with Sonnet ahead on every benchmark Anthropic published. Price is what moved: Claude 3.5 Haiku cost $0.80 per million input tokens, and Haiku 5.5 costs $0.10 for prompts up to 100K.

## Route between Haiku and Sonnet with Respan

Respan puts Haiku 5.5, Sonnet 5.5, and 1,000+ other models behind one API, so switching between them is a one-line model name change. Route, observe, and evaluate every LLM call from the same AI gateway:

- **Respan Router** - Set the model to Respan Router (beta) and Respan picks the model for each request based on the task, weighing cache cost before it switches so a cheaper model doesn't end up costing more.
- **Fallbacks** - List backup models, and Respan moves to the next one when a request errors or hits a rate limit.
- **Cost tracing** - Every request logs tokens, cost, and latency, so you can see how much of your Haiku traffic crosses the 100K pricing line.
- **Side-by-side evals** - Run Haiku 5.5 and Sonnet 5.5 against the same test set, built from your production logs, and compare score distributions in evaluations.
- **Production monitoring** - Evaluators score sampled live traffic to catch regressions, and alerts go to Slack, email, or a webhook when cost, latency, or errors cross a threshold you set.
- **Spend controls** - Set budgets and rate limits per key, per customer, or across the org.

Instead of reading logs after the fact, use Respan to run observability in production, know when production shifts, and act before it spreads.

## FAQ

### What's the difference between Claude Haiku and Sonnet?

Haiku is Anthropic's fastest and cheapest tier, and Sonnet is the mid tier that gives up some speed for stronger reasoning and coding. In the 5.5 generation the two share a 1M-token context window, 128K max output, and a June 2026 knowledge cutoff, so the differences come down to price, quality, and defaults: Haiku 5.5 costs up to 20x less, Sonnet 5.5 scores higher on every benchmark Anthropic published, and Haiku defaults to medium effort while Sonnet defaults to high.

### Is Haiku cheaper than Sonnet?

Yes. Haiku 5.5 costs $0.10 per million input tokens and $0.50 per million output tokens on prompts up to 100K, compared with $2 and $10 for Sonnet 5.5, which makes it 20x cheaper. Above 100K tokens, Haiku moves to $0.50 and $2.50, and the gap narrows to 4x.

### Is Sonnet better than Haiku?

On quality, yes. Sonnet 5.5 beats Haiku 5.5 on every benchmark in Anthropic's announcement, with the biggest lead in agentic coding. Whether it's better for your app depends on whether that gap shows up in your tasks, since Haiku costs up to 20x less.

### Is Haiku faster than Sonnet?

Yes. Anthropic ranks Haiku 5.5 as its fastest model and Sonnet 5.5 as fast, and Artificial Analysis measures Haiku 5.5 at 258 tokens per second against 155 for Sonnet 5.5, both at max effort. Haiku also defaults to medium effort while Sonnet defaults to high, so it starts answering sooner out of the box.

### Is Claude Haiku or Sonnet better for coding?

Sonnet 5.5, for anything complex. It scores 70.6% on Terminal-Bench 4.0 to Haiku's 39.2%, and Anthropic recommends Sonnet or Opus for complex agentic coding. Haiku 5.5 fits smaller coding jobs and subagent roles, where its speed and price matter more than peak capability.

### When should you use Haiku vs Sonnet vs Opus?

Use Haiku 5.5 for high-volume, simple calls, Sonnet 5.5 for most coding and agent work, and Opus 5.5 ($4 input, $20 output per million tokens) when a task needs more than Sonnet can give, which is the tradeoff at the center of any Claude Opus vs Sonnet decision. You don't have to pick one for the whole app, though. Through Respan, all three sit behind the same API, so you can send each request to the tier it needs by model name or let Respan Router pick the model per request.
