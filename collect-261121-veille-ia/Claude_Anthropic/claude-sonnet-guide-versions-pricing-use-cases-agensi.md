---
id: collect-261121-veille-ia/claude-anthropic/claude-sonnet-guide-versions-pricing-use-cases-agensi
title: "Claude Sonnet Guide: Versions, Pricing, Use Cases | Agensi"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["sonnet", "claude", "anthropic", "guide", "model", "version", "pricing", "versions"]
source: https://www.agensi.io/learn/claude-sonnet-guide
source_anchor: ""
source_lines: [1, 135]
section: "Claude_Anthropic"
---
1. Home
2. Learn
3. Deep Dives
4. Claude Sonnet Guide: Versions, Pricing and Best Use Cases

Published 16-11-2026. Last verified on 16-11-2026 against Anthropic's model overview, pricing and deprecation pages. We re-check this guide monthly and after every Sonnet release.

## Quick answer: which Claude Sonnet should you use?

This guide is for developers, marketers and operations leads who run skills and agents on Claude every day and need to know which Sonnet version to use, what a run costs and whether an older version in their scripts is about to stop working. Claude Sonnet is Anthropic's middle model tier and, in our view, the model most skills should run on. On 16-11-2026 the current version is Claude Sonnet 5.5, released on 28-09-2026, at $2 per million input tokens and $10 per million output tokens with a 1M-token context window. Sonnet 5 costs the same, Sonnet 4.6 costs $3 and $15 and Sonnet 4.5 is deprecated with a tentative retirement date of 30-11-2026. Claude 3.5 Sonnet, Claude 3.7 Sonnet and Sonnet 4 no longer run at all.

## How we put this guide together

Our method in three lines: dates, prices and status come from Anthropic's own pages as we read them on 16-11-2026; benchmark numbers are copied from Anthropic's launch posts and labelled vendor-reported; and every cost example is our calculation on list prices, not a measurement.

Our gate: a Sonnet version is in the main table only if it can still be called through the API or a Claude plan on that date, according to Anthropic's deprecation page. Retired versions get a dated note instead. The gate removed Claude 3.5 Sonnet (retired 28-10-2025), Claude 3.7 Sonnet (retired 19-02-2026) and Sonnet 4 (retired 15-06-2026) from the main table, even though "claude 3.5 sonnet" is still searched more than any other Sonnet term. It also removed the Sonnet 5 "introductory price" framing that several articles still use: Anthropic made the $2 and $10 price permanent on 10-08-2026.

We had no model access in this production run, so runtime and cost per skill run are not tested by us. Where a measured number would help, we say so.

## What is Claude Sonnet?

Sonnet is the middle of Anthropic's three main tiers: Haiku for speed and volume, Sonnet for daily work and Opus for the hardest jobs. Anthropic's models overview describes Sonnet 5.5 as "the best combination of speed and intelligence" and lists its latency as "Fast", between Opus 5.5 ("Moderate") and Haiku 5.5 ("Fastest").

That position is why we treat Sonnet as the workhorse. Most skills do work that is too complex for a small model but repeats too often to justify Opus: writing a first draft, reviewing a pull request, cleaning a spreadsheet, summarising a meeting. On Claude.ai, Sonnet has been the default model for Free and Pro plans since Sonnet 4.6, and Anthropic's Sonnet 5 announcement on 30-06-2026 kept it that way. For skills that speed up that everyday office work, the productivity collection is where we would start.

All three top-ranking pages for "claude sonnet" describe Sonnet the same way: an all-round model for code, writing and analysis, with coding as its strongest use. What none of them gives is a version table, so that is what follows.

## Every Claude Sonnet version

The main table lists every Sonnet you can still call on 16-11-2026. Prices are API list prices per million tokens from the pricing page, vendor-reported.

| Version | API ID | Released | Context | Max output | Input | Output | Status | 
|---|---|---|---|---|---|---|---|
| Sonnet 5.5 | claude-sonnet-5-5 | 28-09-2026 | 1M | 128K | $2 | $10 | Active, until at least 28-09-2027 | 
| Sonnet 5 | claude-sonnet-5 | 30-06-2026 | 1M | 128K | $2 | $10 | Active, until at least 30-06-2027 | 
| Sonnet 4.6 | claude-sonnet-4-6 | 17-02-2026 | 1M | 128K | $3 | $15 | Active, until at least 17-02-2027 | 
| Sonnet 4.5 | claude-sonnet-4-5-20250929 | 29-09-2025 | 200K | 64K | $3 | $15 | Deprecated, tentative retirement 30-11-2026 | 

Status and dates come from Anthropic's model deprecations page. The replacement Anthropic names for Sonnet 4.5 is Sonnet 5.5. If any of your scripts, skills or automations still call claude-sonnet-4-5-20250929, we would switch them this week: after retirement those calls fail.

Retired and no longer callable: Sonnet 4 (claude-sonnet-4-20250514, released 22-05-2025, retired 15-06-2026), Claude 3.7 Sonnet (claude-3-7-sonnet-20250219, retired 19-02-2026) and Claude 3.5 Sonnet (two snapshots, retired 28-10-2025).

## What changed from Sonnet 4.5 to Sonnet 5.5?

Four releases in just over a year, each with one change that matters for people who run skills. In release order:

1. **Sonnet 4.5 (29-09-2025).** First in time and the version many older guides still describe. At launch Anthropic called it the best model in the world for agents, coding and computer use. 200K context, $3 and $15.
2. **Sonnet 4.6 (17-02-2026).** Second because it brought the 1M-token context window to Sonnet, in beta at launch and listed without a beta label today. Anthropic reported that Claude Code users preferred it over Sonnet 4.5 roughly 70% of the time, and gave a SWE-bench Verified score of 80.2% with a prompt modification.
3. **Sonnet 5 (30-06-2026).** Third because it cut the price to $2 and $10 per million tokens. Anthropic wrote that its performance is "close to that of Opus 4.8, but at lower prices".
4. **Sonnet 5.5 (28-09-2026).** Newest: same price as Sonnet 5, outputs "30%+ faster than Sonnet 5" per Anthropic and cache reads at $0.10 per million tokens, half of Sonnet 5. In Claude Code and the apps the default effort is medium; on the API it is high.

The step that matters most for skill users is from 4.6 to 5: a third off both input and output prices with no quality loss that Anthropic reports. If a pull request or a release goes wrong after a model switch, the code quality and review collection has skills for catching it before it ships.

## What happened to Claude 3.5 and 3.7 Sonnet?

Both are retired and cannot be called anymore. Claude 3.5 Sonnet had two snapshots, from June and October 2024; Anthropic announced their deprecation on 13-08-2025 and retired them on 28-10-2025. Claude 3.7 Sonnet was deprecated on 28-10-2025 and retired on 19-02-2026. The replacement Anthropic names for all of them today is Sonnet 5.5.

We mention them because "claude 3.5 sonnet" still draws about 14,000 searches a month in our planning data, more than any current Sonnet term. Most of those searchers are looking at an old tutorial, an old integration or an old benchmark. Our advice: treat any guide that recommends 3.5 or 3.7 Sonnet as out of date on prices, limits and model IDs too, and check the IDs in your own code against the table above.

## How much does Claude Sonnet cost?

On the API, Sonnet 5.5 and Sonnet 5 cost $2 per million input tokens and $10 per million output tokens. Sonnet 4.6 and Sonnet 4.5 cost $3 input and $15 output, which answers the frequent search for Sonnet 4.5 input and output pricing. The discounts matter more than the version:

| Option, per million tokens | Sonnet 5.5 | Sonnet 5 | Sonnet 4.6 and 4.5 | 
|---|---|---|---|
| Cache hit | $0.10 | $0.20 | $0.30 | 
| Cache write, 5 minutes | $2.50 | $2.50 | $3.75 | 
| Batch input and output | $1 and $5 | $1 and $5 | $1.50 and $7.50 | 

All from the pricing page on 16-11-2026. Sonnet 5.5 reads cached input at 0.05 times the base price, against 0.1 times for most other models, which is why we think a skill with a long fixed prompt is cheapest on Sonnet 5.5. Requests with US-only inference on Claude 4.6 and later carry a 1.1 times multiplier.

On Claude plans you do not pay per token; Sonnet runs within your plan's usage limits, and our guide to Claude Code usage limits explains how those caps work. For the price list across all models, see Claude API pricing explained.

## What does one skill run cost on Sonnet?

Our worked example: a typical skill run that reads about 40,000 input tokens (the skill instructions, your files and the conversation) and writes about 4,000 output tokens. These are calculations on list prices, counted in each model's own tokens, not measurements:

| Model | Cost per run | 100 runs a month | 
|---|---|---|
| Sonnet 5.5 or Sonnet 5 | $0.12 | $12.00 | 
| Sonnet 4.6 or 4.5 | $0.18 | $18.00 | 
| Opus 5.5 ($4 and $20) | $0.24 | $24.00 | 
| Haiku 5.5 (under 100K tokens) | $0.006 | $0.60 | 

The jump from Sonnet 4.6 to Sonnet 5.5 saves a third with nothing else changed. Prompt caching on a repeated skill prompt saves more. Anthropic's pricing page also notes that Claude 4.7 and later models use a tokenizer that produces about 30% more tokens for the same text; we have not confirmed how that applies to Sonnet 5 and 5.5, so treat the first row as a lower bound. To see what your own runs cost, Agent Cost Scan, free, reads your agent logs and reports cost per run. Our guide to reducing Claude Code token usage and the LLM ops collection cover the rest.

## Sonnet 5.5 benchmarks

Anthropic's Sonnet 5.5 launch post of 28-09-2026 publishes a comparison table in text. Vendor-reported, none measured by us:

| Benchmark | Sonnet 5.5 | Sonnet 5 | Opus 5.5 | 
|---|---|---|---|
| CursorBench 4.0 | 55.5% | 34.1% | 57.8% | 
| OSWorld 2.1 (partial) | 80.1% | 57.0% | 81.8% | 
| Humanity's Last Exam, with tools | 64.5% | 54.9% | 67.7% | 
| GDPval-AA v2.1 (Elo) | 1844 | 1449 | 1846 | 

Two caveats we would keep in mind. Anthropic's Haiku 5.5 post of 07-10-2026 reports slightly different Sonnet 5.5 numbers on some of the same benchmarks, so always quote the source with the number. And the Sonnet 5 and Sonnet 4.6 launch tables are images, so we do not reproduce those scores here.

For independent data from your own agent, Artificial Analysis through Agensi Tools returns model details from $0.013 per call and model cost per task from $0.13 per call, and LMArena returns the best coding models from $0.013 per call (starting prices read on 16-11-2026). No account or API key at either provider is needed, you see the price before a call runs and failed calls are free. More analysis skills sit in the research and analysis collection.

## When does Sonnet beat Opus, and when Haiku?

Our rule: Sonnet by default, Opus when a wrong answer costs a person real time to find, Haiku when the task is short and repeats thousands of times. Sonnet wins on everyday coding, writing, summaries and data clean-up, because on Anthropic's own table it is within a few points of Opus 5.5 at half the price. Opus wins on long, unattended agent runs and multi-file changes. Haiku 5.5, at $0.10 and $0.50 per million tokens for prompts up to 100,000 tokens, wins on classification, extraction and routing, which is what Anthropic recommends it for.

We have not run the same task set on all three for this guide, so these are rules of thumb backed by vendor numbers, not our measurements. Our article on the best Claude model for coding goes deeper on the developer side.

## How do you switch Sonnet versions in your skills and scripts?

You change one model string. Where that string lives depends on how you run Claude, and in our view the hard part is finding every place it is set, not changing it.

1. **API scripts and agents.** First because this is where retired IDs break things. Replace the model value in each request, for example claude-sonnet-4-5-20250929 with claude-sonnet-5-5. The newer IDs carry no date suffix, so an upgrade within a version line does not need a code change, but a jump to a new version does.
2. **Claude Code.** Second: use /model during a session, the --model flag for one session or the model key in settings.json for a default. The Claude Code settings docs say an ANTHROPIC_MODEL environment variable overrides the model key, so check your shell profile if a switch does not stick.
3. **Team-wide limits.** Third, for leads: the availableModels key in a shared or managed settings file restricts which models people can pick. We would use it to stop anyone from pinning a deprecated Sonnet after its retirement date.
4. **Claude.ai and the desktop app.** Last because it needs no action: the model picker shows only versions you can still use.

After a switch, we would run each skill once on a known input and compare the output with the previous version. Model upgrades rarely break a skill, but a changed default effort level, such as medium in Claude Code for Sonnet 5.5, can change how much the model writes. Prompts that rely on a specific output length are the first place we would look.

## Which skills run best on Sonnet?

Skills that do one clear job per run, with a moderate amount of input, are the sweet spot: Sonnet's quality is enough and the price stays low. Our picks, grouped by the work they take off your hands:

- **Debugging and code history.** ai bug fixer, free, reads a stack trace and source files and returns the root cause, a diff and a regression test. commit message explainer, free, turns a git log into a readable summary. First because developers run these several times a day, which is where the Sonnet price pays off. More in the testing and debugging collection.
- **Writing.** Research-Backed Article Writer, $15, runs a cited long-form pipeline with checks and a WordPress draft. Humanize Writing, $7, rewrites stiff AI text into natural language while keeping the facts. Second because writing is the other big Sonnet workload; the writing and content collection has more.
- **Operations.** meeting outcomes action tracker, $9, turns a transcript into actions with owner and deadline. Clean, Deduplicate and Reformat Messy Excel Files, $12, fixes messy exports locally. Third because these jobs are lighter and could often run on Haiku too; we would test both.

For a broader list, our best Claude Code skills of 2026 is the next stop.

We sell skills and pay-per-call tools that your agent uses through one MCP connection, and we name ours above. No vendor paid to be listed.

## Your next step

Search your scripts and skill configurations for claude-sonnet-4-5 and switch them to claude-sonnet-5-5 before 30-11-2026, then turn on prompt caching for any skill with a long fixed prompt. To compare models with live benchmark data from inside your agent, connect it once through Agensi MCP.

Written by Samuel Rose, Founder of Agensi. Next scheduled review: 16-12-2026.
