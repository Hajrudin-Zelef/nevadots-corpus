---
id: collect-261121-veille-ia/claude-anthropic/claude-haiku-5-5-price-release-date-specs-2026-pandromeda
title: "Claude Haiku 5.5 Price, Release Date & Specs [2026] | Pandromeda"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-07"]
keywords: ["claude", "haiku", "anthropic", "model", "tokens", "price", "sonnet", "release"]
source: https://pandromeda.com/ai/claude-haiku-55-explained-price-speed-and-release-date
source_anchor: ""
source_lines: [1, 132]
section: "Claude_Anthropic"
---
# Claude Haiku 5.5 Explained: Price, Speed and Release Date

Anthropic's newest small model trades peak intelligence for speed and a much lower price. Here's what Claude Haiku 5.5 costs, what it's for, and how it stacks up.

Claude Haiku 5.5 is Anthropic's fastest and cheapest model in the Claude 5.5 lineup, launched October 7, 2026 alongside a lower cache price for Claude Sonnet 5.5. It starts at $0.10 per million input tokens and $0.50 per million output tokens, runs on a 1-million-token context window, and is built for high-volume jobs like classification, summarization, and routing, plus speed-sensitive work like live chat and browser automation, rather than for the hardest reasoning tasks.

**Claude Haiku 5.5 at a glance**

- **Released:** October 7, 2026
- **Model ID:**`claude-haiku-5-5`
- **Price:** $0.10 / $0.50 per MTok (input/output) for prompts up to 100K tokens; $0.50 / $2.50 above that
- **Context window:** 1M tokens; max output 128K tokens
- **Positioning:** fastest, lowest-cost model in the Claude 5.5 family; first Haiku with an adjustable effort setting
- **Available on:** Claude API, Claude apps, Claude Code, Amazon Bedrock, Google Cloud, Microsoft Foundry (Azure)

## What is Claude Haiku 5.5?

Claude Haiku 5.5 is the small, fast model in Anthropic's Claude 5.5 generation, sitting below Claude Sonnet 5.5 and Claude Opus 5.5 in the lineup. Anthropic's own comparison table describes it as built "for high-volume, latency-sensitive tasks such as classification, extraction, and routing." It is also the first Haiku-class model to ship with an adjustable **effort** setting, a control that lets a developer trade cost and latency against answer quality on a per-request basis, something earlier Haiku models did not offer.

Anthropic's announcement frames three main uses for the model: high-volume text work (summaries, compaction, classification, database queries), subagent work alongside Sonnet 5.5 or Opus 5.5 on coding tasks, and speed-sensitive, real-time work such as live customer support, voice agents, and browser or desktop automation.

Like Opus 5.5 and Sonnet 5.5, Haiku 5.5 uses a dateless Claude API model ID (`claude-haiku-5-5`) rather than the snapshot-dated IDs Anthropic used for earlier generations, such as `claude-haiku-4-5-20251001`. Anthropic's documentation describes every model ID from the 4.6 generation onward as itself a pinned snapshot, so the dateless name won't silently point to a different model later — a new version gets a new name instead.

## Release date

Anthropic released Claude Haiku 5.5 on October 7, 2026, its first update to the Haiku line since Claude Haiku 4.5 shipped in October 2025. The launch also came with a separate pricing change for Sonnet 5.5: Anthropic halved Sonnet 5.5's prompt-cache read price, from $0.20 to $0.10 per million tokens, making it roughly 20% cheaper on most agentic workloads according to Anthropic's own figures.

## Pricing

Haiku 5.5 is priced by prompt length rather than a single flat rate. Requests with a prompt of up to 100,000 tokens get the lower rate; once a request's prompt (including any cached tokens) passes that threshold, the whole request bills at the higher rate. Anthropic puts the average savings over Haiku 4.5 at about 75%, with up to 90% savings on cached, sub-100K-token requests and 50% off on the Message Batches API.

| Model | Input (per MTok) | Output (per MTok) | Cache reads (per MTok) | 
|---|---|---|---|
| Claude Haiku 5.5 (prompts ≤100K tokens) | $0.10 | $0.50 | $0.01 | 
| Claude Haiku 5.5 (prompts >100K tokens) | $0.50 | $2.50 | $0.05 | 
| Claude Haiku 4.5 | $1.00 | $5.00 | $0.10 | 
| Claude Sonnet 5.5 | $2.00 | $10.00 | $0.10 | 
| Claude Opus 5.5 | $4.00 | $20.00 | $0.20 | 

Those are Claude API list prices, in USD per million tokens, from Anthropic's published pricing page. Batch API requests get a further 50% off, and the same prompt-length split applies to batch pricing. To put the base rate in concrete terms: processing roughly 1.4 million words of input text (about 2 million tokens, at the under-100K-token rate) through Haiku 5.5 costs around $0.20, before any output tokens or prompt-caching discount are applied — the kind of math that makes always-on classification or summarization across a large document set economically realistic in a way it wasn't on earlier, pricier models.

One pricing detail worth flagging: unlike most of Anthropic's current lineup, Haiku 5.5 does *not* get flat-rate pricing across its full context window. Claude 4.6-and-later models, Haiku 5.5 excepted, bill a 900,000-token request at the same per-token rate as a 9,000-token one. Haiku 5.5 instead steps up to the higher "over 100K tokens" rate for any request whose prompt crosses that line, including cached tokens — so very long-context use on Haiku 5.5 costs proportionally more than the same workload would on Sonnet 5.5 or Opus 5.5.

## Context window and other specs

Claude Haiku 5.5 ships with a 1-million-token context window and a 128,000-token output limit on the standard Messages API; that output limit rises to 300,000 tokens on the Message Batches API with a beta header enabled. Anthropic's documentation lists Haiku 5.5's reliable knowledge cutoff as June 2026. Anthropic has committed to keep the model available on its own platform no sooner than October 7, 2027, though partner clouds set their own retirement schedules.

## Where Claude Haiku 5.5 is available

Claude Haiku 5.5 is available through the `claude-haiku-5-5` model ID on the Claude Developer Platform (the first-party API), and in Claude's consumer and team apps — web, iOS, and Android — across the Free, Pro, Max, Team, and Enterprise tiers. It also runs in Claude Code for coding-agent work.

Outside Anthropic's own platform, Haiku 5.5 is available on:

- **Amazon Bedrock** , via the`anthropic.claude-haiku-5-5` model ID, served through US, EU, AU, JP, and Global cross-region inference profiles, plus AWS GovCloud (US).
- **Google Cloud** , through both of Anthropic's Google Cloud offerings (the Google-managed integration on Vertex AI's Agent Platform and the Anthropic-managed integration), with the same 1-million-token context window as the first-party API.
- **Microsoft Foundry** , Microsoft's Azure-hosted model catalog, using the same`claude-haiku-5-5` identifier.

## The effort setting, explained

Effort is new to the Haiku line with this release. Instead of a single fixed response quality, Haiku 5.5 can run at five published levels — Low, Medium (the API default), High, Xhigh, and Max — and both accuracy and per-task cost climb together as effort increases. Anthropic's own benchmark charts make the trade-off explicit: on OSWorld 2.1, a computer-use benchmark, Haiku 5.5 scores 42.0% at Low effort for roughly $0.07 a task, rising to 72.4% at Max effort for about $0.61 a task. On GDPval-AA, Anthropic's knowledge-work benchmark, Haiku 5.5 moves from a score of 1125 at Low effort (about $0.01 a task) to 1620 at Max effort (about $0.87 a task). On Terminal-Bench 4.0, an agentic coding benchmark, the same pattern holds: 12.7% at Low effort for about $0.42 a task, up to 39.2% at Max effort for about $2.64 a task.

That range is the point of the feature: a developer can turn effort down for cheap, high-volume classification work and turn it up for a harder subagent task, without switching models. It's also a reminder that any single "Haiku 5.5 scored X%" headline depends heavily on which effort level was used to produce it — the gap between Low and Max effort on these benchmarks is often 30 points or more.

## Benchmark and speed claims

Anthropic's launch benchmarks compare Haiku 5.5 against its own predecessor and against Sonnet 5.5, generally at Max effort unless noted:

| Benchmark | Haiku 5.5 (Max effort) | Haiku 4.5 | Sonnet 5.5 (Max effort) | 
|---|---|---|---|
| OSWorld 2.1 (computer use) | 72.4% | 15.7% | 83.9% | 
| GDPval-AA (knowledge work) | 1620 | 735 | 1840 | 
| Humanity's Last Exam, with tools | 45.9% | 10.2% | 56.9% | 
| Terminal-Bench 4.0 (agentic coding) | 39.2% | 0.0% | 70.6% | 

Two things stand out. First, Haiku 5.5 is a large jump over Haiku 4.5 on every benchmark Anthropic published, including tasks (like Terminal-Bench) where the previous Haiku essentially scored zero. Second, Sonnet 5.5 still leads Haiku 5.5 on every one of these benchmarks even at Haiku's maximum effort setting, which is consistent with Anthropic's own guidance to use Sonnet 5.5 or Opus 5.5, not Haiku, for complex agentic coding.

On speed, Anthropic calls Haiku 5.5 "the fastest and most efficient model in the Claude 5.5 family" at each model's standard (non–Fast Mode) speed; Fast Mode itself is reserved for Opus-line models. Anthropic also published customer-reported figures rather than its own lab benchmarks for latency and quality. Asana said it saw more than a 30% reduction in task-completion latency and up to 2.5x faster inference per agent turn after switching to Haiku 5.5. Box said the model scored 11 points higher than Haiku 4.5 on an internal evaluation at roughly half the latency. HubSpot reported a 92.8% average score, over three runs, on an internal CRM evaluation suite, and said Haiku 5.5 was the fastest model it tested on a stale-record audit task, with the highest hit rate and lowest false-positive rate of the models compared. Financial-research firm AlphaSense reported a jump from 0.76 to 0.84 on an internal document-QA benchmark across 400 queries, which it called a statistically significant improvement over Haiku 4.5. All of these are the companies' own reported results, published by Anthropic as customer testimonials rather than independently verified third-party benchmarks.

## How it compares to Sonnet 5.5 and Opus 5.5

Anthropic doesn't publish a direct Haiku-5.5-vs-Opus-5.5 benchmark chart, but its own positioning is consistent: Claude Opus 5.5 is the "daily driver" for complex, long-running agentic coding and knowledge work; Sonnet 5.5 is for well-scoped production tasks that need more intelligence than Haiku can reliably deliver; and Haiku 5.5 is for speed and volume — including acting as a cheap subagent that a larger model like Opus 5.5 delegates narrower steps to.

On price alone, Haiku 5.5 is 20 times cheaper than Opus 5.5 on input tokens ($0.10 vs. $4 per MTok) and 40 times cheaper on output tokens ($0.50 vs. $20 per MTok) at the base rate, and it's an order of magnitude cheaper than Sonnet 5.5 too. The benchmark gap narrows as Haiku's effort setting rises, but on every published benchmark, Sonnet 5.5 at any effort level still outperforms Haiku 5.5 at its own highest effort level — so the three models are best read as a cost/capability ladder rather than interchangeable options.

## Who Claude Haiku 5.5 is for

Based on Anthropic's own use-case guidance, Haiku 5.5 fits best for: classification and routing at scale; summarization, extraction, and other high-volume text processing; real-time, latency-sensitive products like chat and voice agents; browser and desktop automation such as form-filling and data entry; and narrow, well-defined coding edits or subagent tasks delegated by a larger model. It is not positioned as a replacement for Sonnet 5.5 or Opus 5.5 on open-ended reasoning or large, complex coding work.

## Safety and alignment notes

Anthropic's own materials say Haiku 5.5's alignment is "substantially improved" over Haiku 4.5, with fewer misaligned behaviors and less willingness to assist with misuse, and that its cybersecurity safeguards are stricter than Haiku 4.5's — allowing more defensive security work while still blocking penetration-testing and attacker-style techniques. Anthropic says the model's biology-related safeguards match those already in place for Sonnet 5, Sonnet 5.5, and Opus 5; full detail is in Anthropic's Claude Haiku 5.5 system card rather than the launch page itself.

## What to watch next

A few open questions are worth tracking as Haiku 5.5 rolls out. Anthropic hasn't published a direct head-to-head benchmark against Opus 5.5, so how the two compare on identical tasks is still something third-party evals will need to fill in. Pricing and availability on partner clouds can also shift independently of Anthropic's own platform — Amazon, Google, and Microsoft each set their own regional rollout and retirement timelines. And as with past model updates, including the run-up to the Claude Sonnet 4.5 retirement, expect Anthropic to eventually set a firm retirement date for Haiku 4.5 once Haiku 5.5 adoption climbs — Anthropic's current commitment is only that Haiku 5.5 itself stays available on its own platform no sooner than October 7, 2027.

## Frequently asked questions

### When was Claude Haiku 5.5 released?

Anthropic released Claude Haiku 5.5 on October 7, 2026, its first update to the Haiku line since Claude Haiku 4.5 shipped in October 2025.

### How much does Claude Haiku 5.5 cost?

On the Claude API, Haiku 5.5 costs $0.10 per million input tokens and $0.50 per million output tokens for prompts up to 100,000 tokens, rising to $0.50 and $2.50 per million tokens for longer prompts. Message Batches API requests get a further 50% discount.

### What is the effort setting on Claude Haiku 5.5?

Effort is a new control, unique to Haiku 5.5 among Haiku-class models, that lets developers pick from five levels — Low, Medium, High, Xhigh, and Max — trading cost and latency for accuracy on a per-request basis.

### How is Claude Haiku 5.5 different from Claude Sonnet 5.5 and Claude Opus 5.5?

Haiku 5.5 is the fastest and cheapest model in the Claude 5.5 family, built for high-volume and latency-sensitive tasks. Sonnet 5.5 and Opus 5.5 cost more but outperform Haiku 5.5 on every benchmark Anthropic has published, even at Haiku's highest effort setting, and remain Anthropic's recommendation for complex agentic coding.

### Is Claude Haiku 5.5 available on Amazon Bedrock and Google Cloud?

Yes. Haiku 5.5 is available on Amazon Bedrock (as anthropic.claude-haiku-5-5), on both of Anthropic's Google Cloud integrations, and on Microsoft Foundry, in addition to Anthropic's own Claude API and Claude apps.

### What is Claude Haiku 5.5's context window?

Haiku 5.5 has a 1-million-token context window and a 128,000-token maximum output on the standard Messages API, rising to 300,000 output tokens on the Message Batches API with a beta header enabled.

## Sources

- Anthropic — Introducing Claude Haiku 5.5anthropic.com
- Anthropic — Claude Haiku 5.5 model pageanthropic.com
- docs.claude.com — Pricingdocs.claude.com
- docs.claude.com — Models overviewdocs.claude.com
- docs.claude.com — Claude Models on Google Cloud (Google managed)docs.claude.com
- AWS — Introducing Claude Haiku 5.5 on AWSaws.amazon.com

Theo Park runs the AI desk at Pandromeda. He follows model launches from the frontier labs and the open-weight community, tracks the assistants and developer tools built on them, and explains what each release changes on pricing, capability and safety. His reporting leans on primary sources: model cards, technical reports, API documentation and the companies' own announcements.
