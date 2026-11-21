---
id: collect-261121-veille-ia/claude-anthropic/what-is-claude-sonnet-5-5-features-benchmarks-pricing
title: "What Is Claude Sonnet 5.5? Features, Benchmarks, Pricing"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["sonnet", "claude", "opus", "anthropic", "tokens", "model", "work", "effort"]
source: https://emergent.sh/learn/what-is-claude-sonnet-5-5
source_anchor: ""
source_lines: [1, 197]
section: "Claude_Anthropic"
---
#### TL;DR

- Claude Sonnet 5.5 is Anthropic's mid-tier AI model, released on September 28, 2026, as the faster, cheaper partner to Claude Opus 5.5.
- It scores within a few points of Opus 5.5 on most of Anthropic's published benchmarks and edges it on Terminal-Bench 4.0 (70.6% vs 66.4%).
- API pricing is $2 per million input tokens and $10 per million output tokens, half of Opus 5.5, with cache reads cut to $0.10 on October 7, 2026.
- It shines at well-defined work at Low to High effort, but at Max effort it burns so many tokens that Opus 5.5 is often the better buy.


Claude Sonnet 5.5 beat Opus 5.5 on Terminal-Bench 4.0 at half the per-token price. Push it to its highest effort setting, though, and independent tests show it costing more per task than Opus.

That tension points to one rule. Run Sonnet 5.5 at Medium or High for well-defined work, and when a task needs more, switch to Opus instead of turning Sonnet up.

Below are its specs, upgrades over Sonnet 5, benchmarks, pricing, effort settings, limits, and access, plus what it means if you build apps without code.

## What is Claude Sonnet 5.5?

Claude Sonnet 5.5 is Anthropic's mid-tier large language model and the second release in the Claude 5.5 family. Released on September 28, 2026, it carries the API model ID claude-sonnet-5-5. Anthropic positions it for well-scoped everyday work, such as fixing bugs and producing documents, slides, and spreadsheets, at half the per-token price of Opus 5.5.

The Claude 5.5 family now has three tiers. Claude Opus 5.5 arrived first, on September 22, for complex work that needs careful judgment. Sonnet 5.5 followed six days later. Claude Haiku 5.5 joined on October 7 for high-volume, cost-sensitive jobs.

In Anthropic's launch announcement, the company is unusually direct about the split. Benchmark scores put Sonnet 5.5 close to Opus 5.5, but Anthropic still says Opus is clearly stronger at complex, open-ended work that needs sustained judgment. Treat Sonnet 5.5 as the model you run most of the time, and Opus as the one you escalate to.

For a deeper look at every score, see our full benchmark breakdown.

## Claude Sonnet 5.5 specs at a glance

Claude Sonnet 5.5 has a 1M-token context window, writes up to 128K tokens per reply, and reads text and images. The full spec sheet from the Claude Platform docs is below.

| Spec | Claude Sonnet 5.5 | 
|---|---|
| Release date | September 28, 2026 | 
| API model ID | claude-sonnet-5-5 (anthropic.claude-sonnet-5-5 on Amazon Bedrock) | 
| Context window | 1M tokens | 
| Max output | 128K tokens (300K on the Batch API with a beta header) | 
| Input and output | Text and images in, text out | 
| Thinking | Adaptive, on by default | 
| Effort levels | Low, Medium, High, Xhigh, Max | 
| Default effort | High on the Claude Platform; Medium in the Claude apps and Claude Code | 
| Knowledge cutoff | June 2026 | 
| Platforms | Claude API, Amazon Bedrock, Google Cloud, Microsoft Foundry, Claude Platform on AWS | 
| Retirement | Not sooner than September 28, 2027 | 

*Claude Sonnet 5.5 specifications, as listed in Anthropic's platform docs (as of October 2026)*

The context window is the amount of text the model can hold in view at once. At 1M tokens, Sonnet 5.5 can read a long contract, a full codebase, or a year of meeting notes in a single request.

## What's new in Claude Sonnet 5.5 compared to Sonnet 5

Sonnet 5.5 is a large upgrade over Claude Sonnet 5 at the same $2/$10 token price. Anthropic reports four headline changes.

- **Faster output:** Output arrives more than 30% faster than on Sonnet 5, making it Anthropic's fastest Sonnet model so far.
- **Fewer tokens per task:** The same work takes far fewer tokens and tool calls, so Anthropic puts the cost per task up to 30% lower.
- **Much stronger agentic work:** On Terminal-Bench 4.0, which tests multi-step tasks in a command line, it jumps from 10.3% to 70.6%.
- **New safeguards:** Sonnet 5.5 is the first Sonnet model to ship with the cybersecurity safeguards Anthropic built for its top models, plus classifiers that block attempts to copy its reasoning.

The token savings show up in customer tests Anthropic published. Balyasny Asset Management ran 2,441 finance tasks and saw Sonnet 5.5 use about 121K tokens per answer, against 497K for Sonnet 5, while scoring higher. Slack saw better results on almost all of its Slackbot evaluations with about 14% fewer output tokens and no prompt changes.

Its reliable knowledge cutoff is June 2026, so it knows about more recent events and tools than earlier Sonnet models. If you run Sonnet 5 today, our guide to the upgrade from Sonnet 5 covers the switch in detail.

## Claude Sonnet 5.5 benchmarks

Claude Sonnet 5.5 lands within one to three points of Opus 5.5 on most of Anthropic's published benchmarks and beats Sonnet 5 on every one. All scores in the table below are vendor-reported from Anthropic's launch post.

| Benchmark | What it tests | Sonnet 5.5 | Sonnet 5 | Opus 5.5 | 
|---|---|---|---|---|
| Terminal-Bench 4.0 | Multi-step tasks in a command line | 70.6% | 10.3% | 66.4% | 
| FrontierCode 1.1 | Code changes that could merge without edits | 52.1% (Xhigh) | 42.4% | 54.4% | 
| CursorBench 4.0 | Tasks from real Cursor coding sessions | 55.5% | 34.1% | 57.8% | 
| GDPval-AA v2.1 | Real work across 44 occupations (Elo) | 1844 | 1449 | 1846 | 
| AA-Briefcase v1.1 | Long, multi-hour knowledge work (Elo) | 1811 | 1359 | 1822 | 
| Humanity's Last Exam | Expert questions across subjects, with tools | 64.5% | 54.9% | 67.7% | 
| OSWorld 2.1 | Operating a computer from screenshots | 80.1% | 57.0% | 81.8% | 
| Chartography | Reading data from charts, no tools | 61.6% | 15.6% | 64.4% | 

*Claude Sonnet 5.5 benchmark scores vs Sonnet 5 and Opus 5.5, vendor-reported by Anthropic (September 2026)*

### 1. Coding and agentic work

Coding is where the jump over Sonnet 5 is largest. Sonnet 5.5 sits within about two points of Opus 5.5 on CursorBench, and it leads Opus on Terminal-Bench 4.0.

One quirk is worth knowing. On FrontierCode, Sonnet 5.5 scored lower at Max effort (46.2%) than at Xhigh (52.1%). Anthropic traced the drop to the model making extra edits beyond the task at Max, which the benchmark penalizes. More effort does not always mean better results.

### 2. Knowledge work and documents

On GDPval-AA, which grades real tasks from 44 occupations, Sonnet 5.5 finishes two Elo points behind Opus 5.5 and about 400 ahead of Sonnet 5. In one internal test, Anthropic gave it a public company's earnings materials and a slide template, then asked for a 10-slide operating review. Two experts judged the first draft ready to send. That is one example chosen by the vendor, but it matches the model's pitch for documents and decks.

### 3. Computer use and chart reading

Sonnet 5.5 operates a computer from screenshots almost as well as Opus 5.5, at 80.1% vs 81.8% on OSWorld 2.1. Chart reading improved the most of any category, from 15.6% to 61.6%. That matters for any workflow that pulls numbers out of reports, dashboards, or PDFs.

### 4. What independent testing shows

Independent testing broadly backs Anthropic's story, with one sharp caveat on cost. Artificial Analysis ran Sonnet 5.5 at Max effort on its own harness on launch day.

| Artificial Analysis measure | Sonnet 5.5 | Opus 5.5 | Sonnet 5 | 
|---|---|---|---|
| Intelligence Index | 56 | 58 | 38 | 
| Terminal-Bench 4.0 (own harness) | 64% | 60% | 14% | 
| AA-Omniscience accuracy (factual recall) | 54% | 66% | 40% | 

*Independent results from Artificial Analysis, all models at Max effort (September 2026)*

The Intelligence Index score of 56 places Sonnet 5.5 second only to Opus 5.5. Its terminal lead over Opus holds up outside Anthropic's own tests. Factual recall is the clear weak spot, which we cover in the limitations section below. Artificial Analysis also notes it tested a pre-release build with a since-fixed bug, so these numbers may shift slightly.

## How much does Claude Sonnet 5.5 cost?

Claude Sonnet 5.5 costs $2 per million input tokens and $10 per million output tokens on the API, exactly half of Opus 5.5. Those are the same token rates Sonnet 5 had.

The one price that changed since launch is cache reads. When Anthropic launched Haiku 5.5 on October 7, 2026, it halved Sonnet 5.5's cache-read price from $0.20 to $0.10 per million tokens. Cache reads are the cheaper rate you pay when the model re-reads text it has already seen, like a long system prompt or earlier turns in an agent loop. Anthropic estimates the cut makes Sonnet 5.5 about 20% cheaper on most agentic work.

| Price per 1M tokens | Claude Sonnet 5.5 | Claude Opus 5.5 | Claude Haiku 5.5 (prompts up to 100K) | 
|---|---|---|---|
| Input | $2 | $4 | $0.10 | 
| Output | $10 | $20 | $0.50 | 
| Cache reads | $0.10 | $0.20 | $0.01 | 
| Cache writes (5 minutes) | $2.50 | $5 | $0.125 | 
| Batch API | 50% off input and output | 50% off input and output | 50% off input and output | 

*Claude 5.5 family API pricing, pricing as of October 2026*

The Claude apps are a separate story. Chatting with Sonnet 5.5 on claude.ai or in Claude Code runs on your subscription plan, not per-token billing. For plan-by-plan details and worked cost examples, see our Sonnet 5.5 pricing guide.

## Effort levels and the real cost per task

The effort setting decides whether Sonnet 5.5 is cheap or expensive, more than the token price does. Effort controls how long the model thinks and checks its work before answering. Higher effort spends more tokens and time, and usually returns a better answer.

At lower settings, the savings are large. Anthropic reports that on several benchmarks, Sonnet 5.5 at Low or Medium effort beats Sonnet 5's best score for about a tenth of the cost per task. On FrontierCode at High effort, it scores 10 points above Sonnet 5 at the same setting for about one-fifteenth of the cost.

At Max effort, the picture flips. Artificial Analysis measured Sonnet 5.5 using about 193K output tokens per task at Max, the highest it had recorded for any model and roughly 60% more than Opus 5.5 at Max. That worked out to $7.60 per task, measured before the October cache-read cut, which put it above Opus 5.5's own cost per task at Max.

| Effort level | Good fit for | What to expect | 
|---|---|---|
| Low | Sorting, tagging, short rewrites, quick lookups | Fastest and cheapest; already beats Sonnet 5's best on some coding tests | 
| Medium | Everyday drafting, well-defined coding, routine agent steps | Default in the Claude apps; strong value for most work | 
| High | Multi-part analysis, reports, multi-file changes | Default on the API; the setting Anthropic recommends starting with for general use | 
| Xhigh | Long agent sessions, hard debugging | Best FrontierCode score; compare against Opus 5.5 at High | 
| Max | The hardest single tasks | Very token-hungry; Opus 5.5 at a lower setting is often cheaper for the same result | 

*How to choose a Claude Sonnet 5.5 effort level*

*Rule of thumb:**Run Sonnet 5.5 at Medium or High. If a task keeps failing there, try Opus 5.5 at Medium or High before you push Sonnet 5.5 to Max.*

Anthropic's own guidance points the same way: Sonnet 5.5 complements Opus 5.5 best at lower effort, where it costs less per task. At the top settings, the two models perform comparably at a similar cost.

## Claude Sonnet 5.5 vs Opus 5.5: which should you use?

Use Sonnet 5.5 when you know what done looks like, and Opus 5.5 when the task needs judgment calls along the way. That is the split Anthropic and AWS both describe, and the benchmarks support it.

| If your task is... | Better pick | Why | 
|---|---|---|
| A bug fix or a clearly specified feature | Sonnet 5.5 | Near-Opus coding scores at half the token price | 
| Documents, slides, or spreadsheets | Sonnet 5.5 | Anthropic's stated strength; two Elo points behind Opus on GDPval-AA | 
| High-volume support or chat | Sonnet 5.5 at Low or Medium | Zendesk reported tickets processed 20% faster than with its current Claude models | 
| A terminal-heavy coding agent | Sonnet 5.5 | Level with or ahead of Opus in both Anthropic's and independent tests | 
| Open-ended work with many decisions | Opus 5.5 | Anthropic says Opus remains clearly stronger here | 
| Answering factual questions from memory | Opus 5.5 | 66% vs 54% factual accuracy in independent testing | 
| Anything that needs Max effort | Opus 5.5 at a lower setting | Often cheaper per task for the same score | 

*Claude Sonnet 5.5 vs Opus 5.5 by task type*

A practical pattern from early testers is to let Opus 5.5 plan the architecture of a big build and hand the implementation to Sonnet 5.5. You pay Opus rates only for the decisions that need them. Our Sonnet 5.5 vs Opus 5.5 comparison goes deeper on scores and cost. We also put it up against GPT-6.1 Sol, OpenAI's closest rival.

## Where Claude Sonnet 5.5 falls short

Sonnet 5.5 is a strong default, but it has four real limits worth planning around.

- **Weaker factual recall:** Sonnet 5.5 answers more questions correctly than Sonnet 5, but in Artificial Analysis's knowledge test it scored 54% against 66% for Opus 5.5. If your app answers questions from the model's own memory instead of your documents, give it a source to read or use Opus.
- **Expensive at Max effort:** At its highest setting it uses more tokens per task than any model Artificial Analysis has measured. Cheap per token does not mean cheap per job.
- **A thin lead on its best benchmark:** The 4.2-point Terminal-Bench lead over Opus 5.5 is real in both Anthropic's and independent runs, but it is one benchmark. On most other tests, Opus 5.5 still scores higher.
- **Cyber requests fall back:** Higher-risk cybersecurity requests visibly hand off to Sonnet 5 instead of running on Sonnet 5.5. Routine bug fixing is not affected, but security teams will notice the switch.

Developers moving code from Sonnet 5 also face five breaking API changes, documented in Anthropic's migration guide. Thinking can no longer be switched off fully, and forced tool use now returns an error. None of this affects people using Sonnet 5.5 through the Claude apps or a no-code builder.

If these limits rule it out for your use case, our list of Sonnet 5.5 alternatives compares the other options.

## How to access Claude Sonnet 5.5

Claude Sonnet 5.5 is available in the Claude apps, in Claude Code, and on every major cloud platform. Where you use it decides the default effort and how you pay.

- **Claude apps and Claude Code:** Pick Sonnet 5.5 from the model menu. Effort defaults to Medium, and usage counts against your Claude plan.
- **Claude Platform (API):** Call the model ID claude-sonnet-5-5. Effort defaults to High, and you pay per token.
- **Amazon Bedrock:** Use anthropic.claude-sonnet-5-5, as covered in the AWS launch post. Your data stays inside AWS, and usage appears on your AWS bill.
- **Google Cloud and Microsoft Foundry:** Both list it under the same claude-sonnet-5-5 ID.

Zero data retention is available, as it is for Opus 5.5. For teams handling customer or financial data, that option matters more than any benchmark score.

## Pick Sonnet 5.5 for scoped work and Opus 5.5 for judgment calls

Claude Sonnet 5.5 is Anthropic's mid-tier option for well-scoped work. The model comes within a few points of Opus 5.5 on coding, documents, and computer use, runs over 30% faster than Sonnet 5, and costs $2/$10 per million tokens, with cache reads now at $0.10.

The catch is effort. Keep it at Medium or High and it delivers near-Opus quality for far less per task. Push it to Max and Opus 5.5 is often cheaper, so escalate the model instead of the setting. For factual questions answered from memory, Opus is the safer pick.

That split matters most if you build apps without code. The AI features a business app needs, like a CRM that summarizes call notes or a client portal that drafts weekly status reports, are well-scoped jobs where speed and cost per request beat deep reasoning. Run those on Sonnet, send rare judgment-heavy work like contract review to Opus, and leave bulk tagging or routing to Haiku.

Emergent, which publishes this guide, is built around the same split. Most AI app builders make it easy to generate something that looks like an app; Emergent is built to produce something that runs like a business, with a real backend and code you own.

Claude Sonnet 5.5 is now live on Emergent, so you can select it for everyday builds when you start a project. Inside your app, the Universal LLM Key gives each AI feature access to Claude, GPT, and Gemini models with one credential, so routine features on Sonnet and judgment calls on Opus need no extra setup.
