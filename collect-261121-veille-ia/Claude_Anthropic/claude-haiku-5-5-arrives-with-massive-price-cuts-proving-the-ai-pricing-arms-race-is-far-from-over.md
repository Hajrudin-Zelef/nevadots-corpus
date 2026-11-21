---
id: collect-261121-veille-ia/claude-anthropic/claude-haiku-5-5-arrives-with-massive-price-cuts-proving-the-ai-pricing-arms-race-is-far-from-over
title: "Claude Haiku 5.5 arrives with massive price cuts proving the AI pricing arms race is far from over"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-08"]
keywords: ["haiku", "percent", "tokens", "model", "claude", "anthropic", "gpt-6", "luna"]
source: https://the-decoder.com/claude-haiku-5-5-arrives-with-massive-price-cuts-proving-the-ai-pricing-arms-race-is-far-from-over/
source_anchor: ""
source_lines: [1, 77]
section: "Claude_Anthropic"
---
# Claude Haiku 5.5 arrives with massive price cuts proving the AI pricing arms race is far from over

## Key Points

- Anthropic has released Claude Haiku 5.5, its fastest and most affordable small model, built for high-volume tasks like data queries and customer support.
- Haiku 5.5 shows major benchmark gains over Haiku 4.5 at up to 90 percent lower token prices. It also beats OpenAI's budget model GPT-6 Luna in areas like agentic coding.
- Haiku 5.5 is available now on AWS, Google Cloud, and Azure. Anthropic is also cutting Sonnet 5.5 cache read costs in half and rolling out monthly API credits for subscribers.

**Update, October 8, 2026:**

Benchmarking platform Artificial Analysis ranks Haiku 5.5 as the leading small-class model with a score of 43 on its Intelligence Index, just ahead of GLM-5.3 Flash (42), Gemini 3.8 Flash (41), and GPT-6 Luna (38). A 13-point gap to Claude Sonnet 5.5 remains.

Artificial Analysis flags a much higher token consumption, though. At the highest effort level, Haiku 5.5 uses about 162,000 output tokens per task on the Intelligence Index, roughly three times the 50,000 tokens GPT-6 Luna needs. Even at comparable performance levels the gap persists: on the "high" effort setting, Haiku 5.5 scores 38 with about 55,000 tokens per task, while GPT-6 Luna hits the same score with around 50,000.

In terms of Pareto efficiency, the ratio of performance to resource use, OpenAI's model comes out ahead. Jumping from "xhigh" to "max" adds just two points while increasing token consumption by 1.8x.

Artificial Analysis also notes Haiku 5.5's low hallucination rate: it hallucinates in 40 percent of cases compared to 77 percent for GPT-6 Luna. The model's weaker factual knowledge (36 percent accuracy on the AA-Omniscience test versus 44 percent for GPT-6 Luna) is partly offset by Haiku 5.5 being more likely to admit when it doesn't know something. The context window also grows from 200,000 tokens on Haiku 4.5 to one million.

**Original article, October 7, 2026:**

**Anthropic has released Claude Haiku 5.5, the company's fastest and most affordable small model to date. Benchmark results show a major performance jump over its predecessor, and Anthropic is also cutting prices for Sonnet 5.5.**

Haiku 5.5 is designed for high-volume, cost-sensitive tasks like summarization, database queries, classification, and live customer support, according to Anthropic. On average, the model costs about 75 percent less than Haiku 4.5. For requests with prompts up to 100,000 tokens, which Anthropic says account for roughly 90 percent of all previous Haiku requests, prices drop by up to 90 percent. Prompts longer than 100,000 tokens cost five times as much.

| Price per 1 million tokens | Haiku 5.5 (prompts up to / over 100k) | Haiku 4.5 | Sonnet 5.5 | 
|---|---|---|---|
| Cache Reads | $0.01 / $0.05 | $0.10 | $0.10 | 
| Cache Writes | $0.125 / $0.625 | $1.25 | $2.50 | 
| Input Tokens | $0.10 / $0.50 | $1.00 | $2.00 | 
| Output Tokens | $0.50 / $2.50 | $5.00 | $10.00 | 

Anthropic points out that Haiku 5.5 uses an updated tokenizer that consumes slightly more tokens per task than its predecessor. The same thing happened with the Opus 4.x models, where token usage jumped about 30 percent from the tokenizer change alone. Real-world savings are likely smaller than the per-token prices suggest.

## Benchmarks show a big leap over Haiku 4.5

Haiku 5.5 scores 1,620 on the knowledge benchmark GDPval-AA v2.1, more than double the 735 its predecessor managed. On Humanity's Last Exam, it hits 45.9 percent without tools and 57.4 percent with tools, up from 10.2 and 18.7 percent.

The biggest jump is in computer use, where the model operates a computer on its own. Since computer use burns through large amounts of tokens, a cheap, fast model like Haiku 5.5 is a natural fit. It scores 72.4 percent on OSWorld-2.1, up from 15.7 percent. On the agentic coding benchmark Terminal-Bench 4.0, it reaches 39.2 percent while Haiku 4.5 scored zero.

Anthropic also lists OpenAI's budget model GPT-6 Luna as a comparison, and Haiku 5.5 leads across every tested category. Sonnet 5.5 reference scores show, however, that Haiku 5.5 still falls well behind Anthropic's larger model.

|  | Haiku 5.5 | Haiku 4.5 | GPT-6 Luna | Sonnet 5.5 | 
|---|---|---|---|---|
| Knowledge work / GDPval-AA v2.1 | 1620 | 735 | 1437 | 1840 | 
| Knowledge Work / AA-Briefcase v1.1 | 1578 | 614 | 1336 | 1824 | 
| Computer Use / OSWorld 2.1 | 72.4% (offline subset) | 15.7% (offline subset) | 48.9% (offline subset) | 83.9% (offline subset) | 
| Multidisciplinary reasoning / Humanity's Last Exam | 45.9% (no tools) | 10.2% (no tools) | — | 56.9% (no tools) | 
|  | 57.4% (with tools) | 18.7% (with tools) | — | 64.5% (with tools) | 
| Agentic coding / Terminal-Bench 4.0 | 39.2% | 0.0% | 16.4% | 70.6% | 
| Agentic coding / FrontierCode 1.1 (Main) | 46.4% | — | 42.4% | 52.1% (Xhigh) | 
| Visual reasoning / Chartography | 46.4% (no tools) | 6.4% (no tools) | 29.1% (no tools) | 61.6% (no tools) | 

## First Haiku model lets users trade cost for performance

Haiku 5.5 is the first Haiku-class model with adjustable reasoning levels, letting users balance cost against quality. Anthropic says the model works best for narrowly scoped tasks like compaction, summarization, or sub-agent work. For complex agentic coding, Sonnet 5.5 and Opus 5.5 remain the better picks.

Cybersecurity safeguards are tighter than on the predecessor but allow a broader range of defensive tasks than Sonnet 5.5, partly because the model is less capable overall. Penetration testing stays blocked. Organizations with broader needs can apply for Anthropic's verification programs for life sciences and cybersecurity.

Haiku 5.5 is available now across all platforms, including Amazon Web Services, Google Cloud, and Microsoft Azure.

## Sonnet 5.5 gets cheaper too, and Anthropic hands out API credits

Alongside the Haiku launch, Anthropic is cutting cache read costs for Sonnet 5.5 by 50 percent, from $0.20 to $0.10 per million tokens. The company says this should reduce costs for most agentic tasks by about 20 percent. The move is almost certainly a reaction to OpenAI's new GPT-6.1 series, showing that AI price wars are being fought harder than ever.

Anthropic is also rolling out monthly API credits. Max-5x subscribers get $100, Max-20x subscribers get $200, and Team subscribers receive up to $500 per month. Users can spend the credits to experiment with tools, apps, and agents through the API.

The company is updating its Python and TypeScript SDKs as well, adding beta support for computer use and browser use.

THE DECODER+

## AI News Without the Hype – Curated by Humans

- Every Plus article and the full archive
- No ad banners
- Frontier Radar: 6 deep dives a year

Already a subscriber? Sign in
