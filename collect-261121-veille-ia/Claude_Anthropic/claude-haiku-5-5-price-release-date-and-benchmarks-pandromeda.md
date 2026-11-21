---
id: collect-261121-veille-ia/claude-anthropic/claude-haiku-5-5-price-release-date-and-benchmarks-pandromeda
title: "Claude Haiku 5.5: Price, Release Date and Benchmarks | Pandromeda"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-07"]
keywords: ["haiku", "claude", "anthropic", "model", "tokens", "price", "release", "date"]
source: https://pandromeda.com/ai/claude-haiku-55-release-date-pricing-and-benchmarks
source_anchor: ""
source_lines: [1, 134]
section: "Claude_Anthropic"
---
# Claude Haiku 5.5: Release Date, Pricing and Benchmarks

Anthropic's new small model cuts API pricing by up to 90% over Haiku 4.5 while posting big benchmark gains, with a 1M-token context window.

Anthropic launched Claude Haiku 5.5 on October 7, 2026, calling it the cheapest, fastest and most capable small model the company has shipped. It slots in below Claude Sonnet 5.5 and Claude Opus 5.5 in Anthropic's lineup, and it's aimed squarely at high-volume, cost-sensitive work — think classification, extraction, routing, summarization and live customer support — rather than the hardest agentic coding tasks. The model is available now through the Claude API as `claude-haiku-5-5`, through Amazon Bedrock, Google Cloud Vertex AI and Microsoft Foundry, and on Anthropic's consumer plans.

**Claude Haiku 5.5 at a glance**

- **Announced:** October 7, 2026
- **Model ID:**`claude-haiku-5-5`
- **API price:** $0.10 / $0.50 per million input/output tokens (prompts up to 100K tokens); $0.50 / $2.50 above 100K tokens
- **Context window:** 1 million tokens; max output 128K tokens
- **Availability:** Claude API, Amazon Bedrock, Google Cloud, Microsoft Foundry, and Anthropic's consumer plans including Free

## What is Claude Haiku 5.5?

Claude Haiku 5.5 is the newest entry in Anthropic's "small model" tier, sitting underneath Claude Sonnet 5.5 and Claude Opus 5.5. Anthropic's own description, taken from its announcement page, is that it's "the cheapest, fastest, and most capable small model we've ever released," built for high-volume, cost-sensitive tasks such as summaries, compactions, database queries and classification.

Anthropic also pitches it as the junior half of a two-model workflow: Opus 5.5 or Sonnet 5.5 handles the planning and the hard reasoning, while Haiku 5.5 runs underneath as a subagent for speed-sensitive chores like live customer support and browser use. That pairing is a theme across Anthropic's current lineup — a larger "lead" model delegating narrow, repetitive work to a smaller, cheaper one — and Haiku 5.5 is built specifically to be the delegate.

One genuinely new feature for the Haiku line: this is the first Haiku-class model with an adjustable effort setting. Developers can choose Low, Medium, High, Xhigh or Max effort depending on how much reasoning a given call needs, trading latency and cost against output quality on a per-request basis.

## Release date and what changed from Haiku 4.5

Claude Haiku 5.5 went live on October 7, 2026, replacing Claude Haiku 4.5 as Anthropic's current small model (Haiku 4.5 remains listed as a legacy option in Anthropic's model documentation). Anthropic's retirement commitment for Haiku 5.5 on its own platform is "not sooner than October 7, 2027," a year out from launch, matching the pattern it uses for other current-generation models.

The headline change from Haiku 4.5 is price. Anthropic says Haiku 5.5 is priced 90% lower than Haiku 4.5 for requests up to 100,000 tokens, and 50% lower for requests above that, which the company summarizes as "around 75% less to run" on average. Capability moved in the same direction: Anthropic's own benchmark table (more on that below) shows sizeable gains over Haiku 4.5 across reasoning, coding and computer-use evaluations.

## Pricing: API and consumer plans

On the Claude API, Haiku 5.5 is billed per million tokens, with a lower rate for shorter prompts and a higher rate once a request crosses 100,000 tokens of context. Here's how Anthropic's pricing page lays it out, next to the model it replaces and the mid-tier Sonnet 5.5 for comparison:

| Price per million tokens | Haiku 5.5 (≤100K tokens) | Haiku 5.5 (>100K tokens) | Haiku 4.5 | Sonnet 5.5 | 
|---|---|---|---|---|
| Input | $0.10 | $0.50 | $1.00 | $2.00 | 
| Output | $0.50 | $2.50 | $5.00 | $10.00 | 
| Prompt cache read | $0.01 | $0.05 | $0.10 | $0.10 | 
| Prompt cache write | $0.125 | $0.625 | $1.25 | $2.50 | 

A few extra notes straight from the pricing page: batch processing cuts the above rates by 50%, prompt cache pricing reflects a five-minute cache TTL, and Anthropic offers US-only inference at a 1.1x multiplier on input and output tokens for customers who need data to stay within US regions.

On the consumer side, Anthropic doesn't publish a separate price for "using Haiku" — it's bundled into the regular Claude app plans. According to Anthropic's pricing page, the Free plan (which includes Claude Sonnet and Claude Haiku) costs nothing; Pro is $17/month billed annually (or $200 up front) or $20/month billed monthly, and adds Claude Opus; and the Max plans build on Pro with 5x or 20x higher usage limits, starting from $100/month. Max 5x subscribers get $100 in monthly API credits, Max 20x subscribers get $200, and Team plan subscribers get up to $500 pooled across their users — credits that can be spent on any model, including Haiku 5.5, through the API.

For a side-by-side on which plan or model actually makes sense for a given budget, Pandromeda's Haiku 5.5 vs. Sonnet 5.5 vs. Opus 5.5 price and speed comparison goes deeper than this article needs to.

## Context window, output limits and effort control

Per Anthropic's model documentation, Claude Haiku 5.5 ships with the same 1-million-token context window as the rest of the current Claude lineup (Opus 5.5, Sonnet 5.5 and Fable 5.1), and a maximum synchronous output of 128,000 tokens. On the Message Batches API, that output ceiling rises to 300,000 tokens using the `output-300k-2026-03-24` beta header. Anthropic's documentation notes that, on the tokenizer introduced with Opus 4.7, 1 million tokens works out to roughly 555,000 words.

Anthropic lists Haiku 5.5's "reliable knowledge cutoff" — the date through which its training data is most extensive and reliable — as June 2026, the same cutoff as Opus 5.5, Sonnet 5.5 and Fable 5.1. Thinking is "adaptive," meaning the model decides how much internal reasoning to do on its own, steered by the effort parameter; Haiku 5.5's default effort level on the API is Medium, with Low, High, Xhigh and Max available for developers who want to tune the latency-versus-quality tradeoff per request.

## Benchmarks: what Anthropic claims

Anthropic published its own benchmark comparison for Haiku 5.5 against Haiku 4.5, Sonnet 5.5, and an unaffiliated model the company refers to as "GPT-6 Luna." As with any vendor-published benchmark, these are Anthropic's own test conditions and numbers, not independently verified, and we're reporting them as Anthropic's claims rather than confirmed facts:

| Benchmark | Haiku 5.5 | Haiku 4.5 | GPT-6 Luna | Sonnet 5.5 | 
|---|---|---|---|---|
| GDPval-AA v2.1 (Elo) | 1620 | 735 | 1437 | 1840 | 
| AA-Briefcase v1.1 | 1578 | 614 | 1336 | 1824 | 
| OSWorld 2.1 (offline subset) | 72.4% | 15.7% | 48.9% | 83.9% | 
| Humanity's Last Exam (no tools) | 45.9% | 10.2% | — | 56.9% | 
| Humanity's Last Exam (with tools) | 57.4% | 18.7% | — | 64.5% | 
| Terminal-Bench 4.0 | 39.2% | 0.0% | 16.4% | 70.6% | 
| FrontierCode 1.1 (Main, Xhigh) | 46.4% | — | 42.4% | 52.1% | 
| Chartography (no tools) | 46.4% | 6.4% | 29.1% | 61.6% | 

The pattern is consistent: Haiku 5.5 beats Haiku 4.5 and GPT-6 Luna by wide margins on nearly every benchmark Anthropic lists, while Sonnet 5.5 stays ahead of Haiku 5.5 on every one of them — sometimes by a little (FrontierCode), sometimes by a lot (Terminal-Bench 4.0, where Sonnet 5.5 scores almost double). That's by design: Anthropic is explicit that "Sonnet 5.5 and Opus 5.5 remain better choices for complex agentic coding tasks," and positions Haiku 5.5 as "best suited to more narrowly scoped tasks."

Anthropic's write-up also cites a handful of customer-reported results rather than its own benchmarks: Asana reported "over a 30% reduction in latency for task completions and up to 2.5x faster inference per agent turn" after switching to Haiku 5.5; HubSpot measured 92.8% accuracy averaged over three runs on its CRM evaluation suite; AlphaSense scored 0.84 versus 0.76 for Haiku 4.5 across 400 queries; Box reported results "11 points higher than Haiku 4.5 at about half the latency"; and Cognition said its Fusion tool hit a FrontierCode score of 66.2 when using Haiku 5.5 as the supporting "sidekick" model. These are company-supplied figures from Anthropic's own customers, not third-party audits.

## How Haiku 5.5 fits next to Sonnet 5.5 and Opus 5.5

Anthropic's current three-model structure is unchanged in spirit, even as all three generations moved to 5.5: Opus 5.5 is the model for long-running agentic coding and knowledge work, Sonnet 5.5 is positioned as "the best combination of speed and intelligence," and Haiku 5.5 is the fast, cheap option for high-volume, latency-sensitive tasks like classification, extraction and routing. On list price, Haiku 5.5 starts at $0.10 per million input tokens against $2 for Sonnet 5.5 and $4 for Opus 5.5 — roughly a 20x and 40x gap, respectively, before accounting for Haiku's shorter, cheaper typical outputs.

In practice, that makes Haiku 5.5 the model most teams will reach for as the workhorse underneath an agent built on Claude Sonnet 5.5 or Claude Opus 5.5, rather than a replacement for either. Anthropic's own benchmark table backs that framing up: Haiku 5.5 trails Sonnet 5.5 on every listed evaluation, sometimes by a wide margin on harder agentic-coding tests like Terminal-Bench 4.0. If your workload is single-turn classification, short summarization, high-volume extraction or a chat widget that needs to answer fast and cheap, Haiku 5.5 is the one to evaluate first; if it's multi-step coding, long-horizon planning or anything where accuracy matters more than cost per call, Anthropic's own data points toward Sonnet 5.5 or Opus 5.5 instead.

We're not re-running the full buying breakdown here — Pandromeda's dedicated Haiku 5.5 vs. Sonnet 5.5 vs. Opus 5.5 comparison covers price, speed and use-case fit across all three models in more depth than a launch news story needs.

## Availability: where you can use it today

Anthropic says Claude Haiku 5.5 is "available now on all platforms, including Amazon Web Services, Google Cloud, and Microsoft Azure," alongside the first-party Claude API. Developers call it with the model ID `claude-haiku-5-5` on Anthropic's own API, or the equivalent IDs on Bedrock (`anthropic.claude-haiku-5-5`), Vertex AI and Microsoft Foundry.

On the consumer side, Anthropic's pricing page lists Haiku as one of the models included on the Free plan, alongside Sonnet, with Opus added on Pro and the Max tiers. That means Haiku 5.5 is reachable inside the Claude apps at every subscription level, including the free tier, once Anthropic finishes rolling the new snapshot into the model picker.

## Where Anthropic expects Haiku 5.5 to get used

The use cases Anthropic highlights all share a theme: lots of requests, modest reasoning depth per request, and latency that users actually notice. Summarization and context compaction for long agent sessions, database and CRM queries, document classification, and live customer support are the examples called out by name. Browser-use agents — the kind that click through a web page step by step — get a specific mention too, since every step is a separate model call and shaving milliseconds off each one compounds quickly across a session.

The subagent pattern is probably the most consequential use case for teams building with Claude today: rather than running every step of a complex workflow through Opus 5.5 or Sonnet 5.5, a lead model can hand off narrow, well-defined subtasks to Haiku 5.5 and only escalate back up when a step needs deeper reasoning. Anthropic's pricing math makes that pattern meaningfully cheaper than it was with Haiku 4.5, which is likely why the company leads its announcement with price rather than any single benchmark score.

## Safety and alignment notes

Anthropic's announcement includes a short safety section alongside the performance claims. The company says its alignment evaluations show "major improvements across almost all" categories relative to Haiku 4.5. On cybersecurity, Haiku 5.5's safeguards are described as more restrictive than Haiku 4.5's but less restrictive than Sonnet 5.5's, with protections that still block penetration-testing-style requests and similar attacker-oriented techniques. Biology-related safeguards, Anthropic says, match the level already applied to Sonnet 5, Sonnet 5.5 and Opus 5.5.

## What to watch next

A few things worth tracking as Haiku 5.5 rolls out more broadly: how quickly Anthropic's cloud partners (AWS, Google Cloud, Microsoft Foundry) light up regional and global endpoints for the new model, since each sets its own availability and lifecycle schedule independently of Anthropic's own API; whether the effort-setting feature — new to the Haiku line with this release — gets adopted by other developer tools as a standard latency/cost dial; and how the "lead model plus Haiku subagent" pattern that Anthropic is promoting shows up in agent frameworks and products built on Claude.

For now, the headline is straightforward: Claude Haiku 5.5 is a meaningfully cheaper, faster small model than its predecessor, with real benchmark gains to go with the price cut, released the same week Anthropic pushed Sonnet 5.5's cache pricing down too. Whether it changes anyone's model choice depends entirely on whether the workload in question needs Sonnet- or Opus-level reasoning, or was always better served by something fast and cheap underneath.

## Frequently asked questions

### When was Claude Haiku 5.5 released?

Anthropic announced Claude Haiku 5.5 on October 7, 2026, and made it available immediately through the Claude API, major cloud platforms and Anthropic's consumer plans.

### How much does Claude Haiku 5.5 cost on the API?

It costs $0.10 per million input tokens and $0.50 per million output tokens for prompts up to 100,000 tokens, rising to $0.50 and $2.50 per million tokens above that length, according to Anthropic's pricing page.

### What is Claude Haiku 5.5's context window?

Per Anthropic's documentation, Haiku 5.5 has a 1-million-token context window and a maximum output of 128,000 tokens, which rises to 300,000 tokens on the Message Batches API with a beta header.

### Is Claude Haiku 5.5 available on the free Claude plan?

Yes. Anthropic's pricing page lists Haiku as one of the models included on its Free plan, alongside Claude Sonnet.

### How does Claude Haiku 5.5 compare to Claude Sonnet 5.5?

Anthropic's own benchmark figures show Sonnet 5.5 scoring higher than Haiku 5.5 on every listed evaluation. Anthropic positions Haiku 5.5 for high-volume, narrowly scoped tasks and recommends Sonnet 5.5 or Opus 5.5 for complex agentic coding.

### What is Claude Haiku 5.5's knowledge cutoff?

Anthropic lists a reliable knowledge cutoff of June 2026 for Claude Haiku 5.5, the same cutoff it gives for Opus 5.5 and Sonnet 5.5.

## Sources

- Anthropic - Introducing Claude Haiku 5.5anthropic.com
- Anthropic - Claude Pricingclaude.com
- Anthropic Docs - Models overviewplatform.claude.com
- Wikipedia - Claude (language model)en.wikipedia.org

Theo Park runs the AI desk at Pandromeda. He follows model launches from the frontier labs and the open-weight community, tracks the assistants and developer tools built on them, and explains what each release changes on pricing, capability and safety. His reporting leans on primary sources: model cards, technical reports, API documentation and the companies' own announcements.
