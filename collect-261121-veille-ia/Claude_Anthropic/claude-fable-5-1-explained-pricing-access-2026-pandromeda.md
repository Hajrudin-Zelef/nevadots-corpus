---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-1-explained-pricing-access-2026-pandromeda
title: "Claude Fable 5.1 Explained: Pricing & Access [2026] | Pandromeda"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "fable", "model", "anthropic", "opus", "agentic", "tokens", "demanding"]
source: https://pandromeda.com/ai/what-is-claude-fable-51-anthropics-model-explained
source_anchor: ""
source_lines: [1, 161]
section: "Claude_Anthropic"
---
# What Is Claude Fable 5.1? Anthropic's Model, Explained

Anthropic's model for demanding reasoning and long-horizon agentic work, alongside Opus 5.5, Sonnet 5.5, and Haiku 5.5 — what it costs, who gets it, and what it's for.

Claude Fable 5.1 is Anthropic's model for demanding reasoning and long-horizon agentic work — the slowest, most expensive, and most capable model in the current Claude 5 lineup, sitting above Opus 5.5, Sonnet 5.5, and Haiku 5.5. Anthropic's own documentation describes it plainly: start with Opus 5.5 for most workloads, and reach for Fable 5.1 only for demanding reasoning and long-horizon agentic tasks, or when Opus 5.5 at higher effort still falls short. It is available now through the Claude API as `claude-fable-5-1`, through Amazon Bedrock, Google Cloud, and Microsoft Foundry, and through the Claude apps on paid plans.

**Claude Fable 5.1 at a glance**

- **What it is:** Anthropic's top-tier model for demanding reasoning and long-horizon agentic work
- **API model ID:**`claude-fable-5-1`
- **Released:** September 1, 2026
- **API pricing:** $10 per million input tokens / $50 per million output tokens
- **Context window:** 1 million tokens; max output 128K tokens
- **Claude app access:** Not on the Free plan; usage-credit access on Pro; included (capped at half the weekly limit) on Max, Ultra, and Team Premium seats; included on Enterprise

## What is Claude Fable 5.1?

Claude Fable 5.1 is the latest version of Anthropic's "Fable" model line, released September 1, 2026 as a direct successor to Claude Fable 5. Anthropic's model documentation defines it in a single line: a model "for demanding reasoning and long-horizon agentic work." In the official models comparison table, it sits alongside Claude Sonnet 5.5, Claude Opus 5.5, and Claude Haiku 5.5 as the fourth member of the current Claude 5 generation — but it is priced, and positioned, differently from the other three.

Unlike Opus, Sonnet, and Haiku, which get a new ".5" release roughly once a quarter, Fable is a separate naming track that Anthropic introduced in June 2026 with the original Claude Fable 5, alongside a restricted sibling called Claude Mythos. Fable 5.1 is the first point release in that line, and it remains Anthropic's most expensive and most capable publicly available model, even after Opus 5.5 shipped three weeks later.

## Where Fable 5.1 sits in the Claude 5 lineup

Anthropic publishes a single comparison table for its current model lineup. Here is what it says about all four models, reproduced from the official models overview:

| Model | Description | Price per MTok (in/out) | Context window | Latency | Default effort | 
|---|---|---|---|---|---|
| Claude Fable 5.1 | For demanding reasoning and long-horizon agentic work | $10 / $50 | 1M tokens | Slower | High | 
| Claude Opus 5.5 | For long-running agentic coding and knowledge work | $4 / $20 | 1M tokens | Moderate | Medium | 
| Claude Sonnet 5.5 | The best combination of speed and intelligence | $2 / $10 | 1M tokens | Fast | High | 
| Claude Haiku 5.5 | For high-volume, latency-sensitive tasks such as classification, extraction, and routing | From $0.10 / $0.50 | 1M tokens | Fastest | Medium | 

All four models share the same 1 million token context window, 128K token maximum output, and a June 2026 reliable knowledge cutoff. Where they differ is price, comparative latency, and how much "thinking" they do by default. Fable 5.1 is the only one of the four with a "Slower" latency rating and the only one priced above Opus 5.5, which is itself the second most expensive model in the family. For a closer side-by-side on when each one actually wins, see this site's comparison of Claude Opus 5.5 and Fable 5.1.

## What Fable 5.1 is designed for

Anthropic's "what's new" documentation for Fable 5.1 lists six areas where the model's gains over Fable 5 concentrate, and they double as a description of its intended use:

- **Agentic coding over long sessions** — multifile features, large refactors, migrations, and debugging across sessions that run for hours
- **Knowledge work with documents, spreadsheets, and slides** — taking an analysis from a first question to a finished document, live-formula spreadsheet, or slide deck
- **Research and search** — higher accuracy on multistep web research and deep-research tasks that follow up on what they find
- **Vision** — reading dense charts, filings, and tables nested in PDFs
- **Long-context work** — reasoning over and connecting details across the full 1M-token window
- **Computer use** — operating a browser and desktop applications more reliably, and recovering from failed steps

Despite the name, Anthropic's own documentation does not position Fable 5.1 as a dedicated fiction or storytelling model. Its model page and announcement both describe it as a successor built for "long-running agentic coding, knowledge work, and research" — the same territory as Opus 5.5, just priced and tuned for cases where Opus 5.5 at its highest effort setting still isn't enough. Anthropic explicitly tells developers to default to Opus 5.5 and only move up to Fable 5.1 when their own evaluations show it's necessary.

## Pricing: what Fable 5.1 costs

On the Claude API, Fable 5.1 is priced at $10 per million input tokens and $50 per million output tokens — identical to Fable 5's list price. The change in 5.1 is on the caching side: cache reads dropped to $0.25 per million tokens, a quarter of Fable 5's old cache-read rate and well below the 10% of base-input-price ratio most other Claude models use. Anthropic frames this as the headline cost story for 5.1, estimating roughly 25% lower costs on typical workloads and up to about 45% lower costs on highly agentic ones, driven mostly by that cheaper cache reuse.

The full rate card:

| Item | Price per million tokens | 
|---|---|
| Input (base) | $10 | 
| Output | $50 | 
| 5-minute cache write | $12.50 | 
| 1-hour cache write | $20 | 
| Cache read | $0.25 | 
| Batch API (input/output) | $5 / $25 (50% off) | 

For context, that input/output rate is more than double Opus 5.5's ($4/$20) and five times Sonnet 5.5's ($2/$10). Anthropic's full, current rate card — including regional and tool-use pricing — is published on its Claude API pricing page.

## How to get it: API and model ID

Developers can call Fable 5.1 directly through the Claude API using the model ID `claude-fable-5-1`. It's also available through:

- **Amazon Bedrock** , as`anthropic.claude-fable-5-1`
- **Claude Platform on AWS** , as`claude-fable-5-1`
- **Google Cloud** (both Anthropic-managed and Google-managed deployments), as`claude-fable-5-1`
- **Microsoft Foundry** , as`claude-fable-5-1`

Every model ID on the Claude API is a pinned snapshot rather than a moving alias, per Anthropic's documentation — so `claude-fable-5-1` will keep pointing at this exact model rather than silently updating later. Anthropic's support pages note that Fable 5.1 and its restricted sibling Mythos 5.1 carry 30-day data retention by default and are not available under zero data retention unless Anthropic expressly authorizes it for a given account, though an enterprise option called Enterprise Frontier Safeguards — letting customers keep data in their own cloud — began rolling out in phases starting this past fall.

## Claude app and plan access

Outside the API, access to Fable 5.1 depends on which Claude.ai subscription plan you're on. Per Anthropic's current pricing page:

| Plan | Price | Fable 5.1 access | 
|---|---|---|
| Free | $0 | Not included | 
| Pro | $20/month ($17/month billed annually) | Usage-credit access only | 
| Max | $100/month | Included, capped at half the weekly usage limit | 
| Ultra | $200/month | Included, capped at half the weekly usage limit | 
| Team (Standard seat) | $20–25/seat/month | Not included | 
| Team (Premium seat) | $100–125/seat/month | Included, capped at half the weekly limit | 
| Enterprise | From $20/seat/month (20-seat minimum) plus usage | Included | 

In other words, Fable 5.1 is not a model every Claude subscriber gets by default. Free and Pro users can reach it only by purchasing API-style usage credits inside the Claude app; Max, Ultra, and Team Premium seats get it bundled in, but at a reduced weekly allowance compared with the rest of their usage; Enterprise customers get it as part of their seat. By default, Anthropic's apps set Fable 5.1 to Medium effort on Claude.ai and High effort in Claude Code. If you're deciding between paying for Fable 5.1 access or sticking with a cheaper model in the same app, this site's breakdown of Claude Sonnet 5.5's pricing and availability is a useful companion read.

## Release date and context

Claude Fable 5.1 was released on September 1, 2026, with Anthropic committing not to retire it from its own platforms sooner than September 1, 2027. It followed the original Claude Fable 5, which Anthropic launched in June 2026 alongside a restricted model called Claude Mythos 5 — the two models share the same underlying capabilities, but Mythos is reserved for vetted organizations with a need for fewer safety restrictions, such as verified cybersecurity and life-sciences researchers.

Fable 5.1 arrived a few weeks before the rest of the current Claude 5.5 refresh: Opus 5.5 followed on September 22, 2026, Sonnet 5.5 on September 28, and Haiku 5.5 on October 7. That makes Fable 5.1, despite its name suggesting a minor update, the oldest model by release date in what Anthropic now presents as a single current lineup — and the only one still using the "Fable" naming track rather than the "5.5" numbering Anthropic gave the Opus, Sonnet, and Haiku refreshes. Anthropic's full announcement for the release is published at anthropic.com/claude-fable-and-mythos-5-1.

## Fable 5.1 vs. Mythos 5.1

Alongside Fable 5.1, Anthropic also released Claude Mythos 5.1. According to Anthropic's own documentation, the two share identical specifications and pricing — context window, output limits, thinking behavior, and the rate card above are the same for both. The difference is access and safety configuration: Mythos 5.1 is restricted to organizations verified through Anthropic's formal verification programs, such as its Cyber Verification Program, and is aimed at vetted cybersecurity and life-sciences use cases where some of Fable 5.1's standard safeguards would otherwise block legitimate work. Anthropic says interested organizations should apply through the relevant verification program, such as its Cyber Verification Program, or contact their Anthropic, AWS, or Google Cloud account team; as of this model's release, that access was limited to US-based organizations.

## What changed from Claude Fable 5

Anthropic's migration documentation for Fable 5.1 lists several concrete, confirmed changes for anyone already building on Fable 5:

- **Cheaper cache reads** — down to $0.25 per million tokens from Fable 5's rate, the single biggest driver of Anthropic's quoted cost savings
- **Content provenance** — all text Fable 5.1 and Mythos 5.1 generate now carries Anthropic's statistical text watermark by default, and supported generated images, video, and audio carry signed C2PA Content Credentials when retrieved through the Files API
- **Mid-conversation effort changes (beta)** — developers can raise or lower the model's thinking depth partway through a conversation without invalidating the prompt cache
- **Forced tool use is no longer supported** — a breaking change for integrations that relied on`tool_choice` set to a specific tool or "any," since Fable 5.1 always runs its adaptive thinking first
- **Fewer, more variable parallel tool calls and sparser in-chat progress narration** — both documented as intentional behavior changes rather than regressions, with prompting workarounds in Anthropic's developer docs

Anthropic also reports performance gains concentrated at higher effort settings, widest in long agentic coding sessions, multistep research, and document/spreadsheet/slide generation, though it does not publish every benchmark figure on the model's own overview page — for broader, named benchmark comparisons against other labs' models, see this site's dedicated Fable 5.1 benchmark comparison. The full list of changes, including code samples, is in Anthropic's "What's new in Claude Fable 5.1" documentation.

## What's not officially confirmed

A few things circulating about Fable 5.1 are not stated on Anthropic's own model pages, pricing documentation, or announcement, and this piece deliberately leaves them out rather than guess:

- Anthropic's own materials do not name specific competing models (such as any particular GPT or Gemini release) when describing Fable 5.1's performance, so head-to-head claims against other labs should be sourced separately and treated as third-party testing rather than Anthropic's own benchmark claims.
- Anthropic has not published a breakdown of exactly which Claude app plans get how many Fable 5.1 messages per week in absolute terms — only that access on Max, Ultra, and Team Premium seats is "capped at half" the account's regular weekly limit.
- The company has not given a specific completion date for the phased rollout of Enterprise Frontier Safeguards, its in-your-own-cloud data retention option for Fable 5.1 and Mythos 5.1.

## What's next

Anthropic ships new Claude models on a rolling basis, and both the Fable and the Opus/Sonnet/Haiku tracks are likely to keep getting incremental updates on separate schedules — Fable 5.1 is already the second release in its naming line in under three months. Anyone building on the API should watch Anthropic's model deprecations page for retirement dates and its migration guides before switching model IDs in production. For everyday Claude.ai users, the practical question is usually simpler: whether a Max, Ultra, or Team Premium seat is worth it for occasional access to Fable 5.1's weekly-capped allowance, or whether Opus 5.5 — cheaper, faster, and the model Anthropic itself recommends for most workloads — already covers the job. Readers weighing that trade-off in more depth can see this site's full Opus 5.5 vs. Fable 5.1 comparison as pricing, access tiers, and benchmarks continue to shift.

## Frequently asked questions

### What is Claude Fable 5.1?

Claude Fable 5.1 is Anthropic's model for demanding reasoning and long-horizon agentic work, released September 1, 2026. It's the top-tier model in the current Claude 5 lineup, alongside Claude Opus 5.5, Claude Sonnet 5.5, and Claude Haiku 5.5, and is available via the Claude API as claude-fable-5-1.

### Is Claude Fable 5.1 better than Claude Opus 5.5?

Anthropic doesn't rank them that simply. Its own documentation recommends starting with Opus 5.5 for most workloads and reaching for Fable 5.1 only for demanding reasoning and long-horizon agentic tasks where Opus 5.5 at higher effort still falls short. Fable 5.1 is also more than double Opus 5.5's price.

### How much does Claude Fable 5.1 cost on the API?

Fable 5.1 is priced at $10 per million input tokens and $50 per million output tokens, the same list price as Fable 5. Cache reads are cheaper in 5.1, at $0.25 per million tokens.

### How do I get Claude Fable 5.1 in the Claude app?

It isn't included on the Free plan and requires usage credits on Pro. It's bundled in, capped at half your weekly usage limit, on Max, Ultra, and Team Premium seats, and included on Enterprise plans.

### What's the difference between Claude Fable 5.1 and Claude Mythos 5.1?

Anthropic says the two share identical specifications and pricing. Mythos 5.1 is restricted to organizations verified through programs such as Anthropic's Cyber Verification Program, for cybersecurity and life-sciences use cases where some of Fable 5.1's standard safeguards would otherwise block legitimate work.

### What happened to Claude Fable 5, the previous version?

Claude Fable 5 remains listed as a legacy model still available on the Claude API alongside Fable 5.1. Fable 5.1 is the current, recommended successor, with the same input/output pricing and a much cheaper cache-read rate.

## Sources

- Claude models overview — platform.claude.complatform.claude.com
- Claude Fable 5.1 model page — platform.claude.complatform.claude.com
- What's new in Claude Fable 5.1 — platform.claude.complatform.claude.com
- Claude Fable 5.1 and Mythos 5.1 announcement — Anthropicanthropic.com
- Claude pricing — claude.comclaude.com
- Claude API pricing — platform.claude.complatform.claude.com

Theo Park runs the AI desk at Pandromeda. He follows model launches from the frontier labs and the open-weight community, tracks the assistants and developer tools built on them, and explains what each release changes on pricing, capability and safety. His reporting leans on primary sources: model cards, technical reports, API documentation and the companies' own announcements.
