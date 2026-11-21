---
id: collect-261121-veille-ia/muse/muse-wonder-meta-s-frontier-model-pricing-and-benchmarks
title: "Muse Wonder: Meta's Frontier Model, Pricing, and Benchmarks"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: ["2026-10-12"]
keywords: ["muse", "wonder", "meta", "model", "full", "tokens", "models", "million"]
source: https://www.lindy.ai/blog/muse-wonder
source_anchor: ""
source_lines: [1, 227]
section: "Muse"
---
*Meta released Muse Wonder on October 12, and for the first time in a while, a Meta model landed near the top of the leaderboards. It also comes with a catch: Muse Wonder is two models, and the stronger one isn't on the API at all.*

Muse Wonder is the one developers can call for $4 per million input tokens. Muse Wonder Full, the version behind Meta's best benchmark numbers, is only in Muse Code's $50 and $200 monthly plans.

I went through Meta's newsroom post, its developer blog and benchmark table, the Muse Code plans page, the Meta Model API pricing, and Artificial Analysis's current scores on November 3. Here's what Muse Wonder is, how the two versions differ, what each costs, how they score once you separate Meta's numbers from independent ones, and how it relates to Meta's open-weight Muse Spark Open.

**TL;DR:**

- **What it is:** Meta's most capable model series, released October 12, 2026, built for coding, long-running agents, computer use, and expert work in law, finance, and health.
- **Two models:** Muse Wonder is on the Meta Model API, OpenRouter, Cursor, and every Muse Code plan. Muse Wonder Full is only in Muse Code's Power Usage ($50 a month) and Maximum Usage ($200 a month) plans.
- **API price:** $4 per million input tokens, $0.20 per million cached input tokens, and $20 per million output tokens, for Muse Wonder only.
- **Headline features:** active memory, which carries context across conversations, and active focus, which keeps an agent working through blockers and checks in before important actions.
- **How good it is:** on Artificial Analysis's current Intelligence Index v5, Muse Wonder Full scores 50 and Muse Wonder scores 43, behind Gemini 4 Carbon (59), Claude Opus 5.5 (58), and GPT-6 Astra (57). Meta's own table shows Wonder Full leading on legal work and long coding jobs.
- **Open source?** No. Both Wonder models are proprietary. Meta's open-weight model is Muse Spark Open, a separate release that scores well below both on Artificial Analysis.

## What is Muse Wonder?

**Muse Wonder is Meta's newest frontier AI model series,** announced on October 12, 2026, and made up of two models: Muse Wonder and Muse Wonder Full. Meta's newsroom post calls them "our most capable models to date," built on the groundwork of Muse Spark, Meta's earlier model.

Meta aims them at "personal and professional agentic tasks." That covers expert domains like law, finance, and health, plus coding for anything from a game to an app for a new business.

Meta says Muse Wonder Full beats Claude Fable 5.1 and ranks just below Claude Opus 5.5 on the Artificial Analysis score it reported at launch, though both Wonder models score lower on the index's newer version (more on that below).

**The specs, from Meta's** **developer blog****, the Meta Model API, and** **Artificial Analysis****:**

| 🔧 Spec | 📋 Muse Wonder | 📋 Muse Wonder Full | 
|---|---|---|
| Made by | Meta | Meta | 
| Released | October 12, 2026 | October 12, 2026 | 
| Model ID | muse-wonder-1.0 | muse-wonder-full-1.0 | 
| Where you can use it | Meta Model API and all Muse Code plans | Muse Code Power and Maximum Usage plans only | 
| API price per 1M tokens | $4 input, $0.20 cached, $20 output | Not sold by the token | 
| Context window | 1 million tokens (Cursor defaults to 300K) | 1 million tokens | 
| Inputs and output | Text, images, video, and audio in, text out; Meta says to confirm audio support on each surface | Same | 
| Open weights | No | No | 

## Muse Wonder vs. Muse Wonder Full

**Muse Wonder Full is the stronger model, and Muse Wonder is the one you can build on.** Meta says Muse Wonder "brings comparable performance to Muse Wonder Full," and its own benchmark table shows the gap is small on some tests and wide on others.

How big the gap is depends on the test. In Meta's table, Wonder Full beats Wonder by about 5 points on Meta's internal Personal Superintelligence Index, 6 points on AutomationBench-AA, and 9 points on ProgramBench, which tests writing programs from scratch. On memory and long-horizon coding (MemoryBench and DeepSWE), the two score within a fraction of a point of each other.

Access is the other big difference:

- **Muse Wonder** is on the Meta Model API at $4 and $20 per million tokens, and it's included in every Muse Code plan, starting at $5 a month.
- **Muse Wonder Full** has no API price. Meta says it's "available exclusively by subscription in Muse Code," on the Power Usage and Maximum Usage plans.

As of November 3, Muse Wonder is the only one of the two you can build a product on, and Muse Code's Power or Maximum Usage plan is the only way to get Wonder Full for your own coding work.

## How to get Muse Wonder

**You can use Muse Wonder through the Meta Model API, OpenRouter, Cursor, or Muse Code, and Muse Wonder Full only through Muse Code's two top plans.** Here's where each one is available as of November 3, with Muse Code prices from Meta's Muse Code page:

| 📍 Where | 🤖 Muse Wonder | 🤖 Muse Wonder Full | 💵 Price | 
|---|---|---|---|
| Meta Model API | Yes | No | $4 input / $20 output per 1M tokens | 
| OpenRouter | Yes | No | $4 input / $20 output per 1M tokens | 
| Cursor | Yes | No | $4 input / $20 output per 1M tokens, billed outside Cursor's included usage | 
| Muse Code Everyday Usage | Yes | No | $5 a month | 
| Muse Code High Usage | Yes | No | $15 a month | 
| Muse Code Power Usage | Yes | Yes | $50 a month | 
| Muse Code Maximum Usage | Yes | Yes | $200 a month | 
| Muse assistant app | Not yet | Not yet | Coming "soon," subscribers first | 

**The Muse Code plans differ mainly in how much you can use.** Everyday Usage allows 10 to 50 prompts every five hours. High Usage gives you 5 times that, Power Usage 20 times, and Maximum Usage 80 times, with usage resetting every five hours and every week.

**Muse Code itself is Meta's coding agent,** a command-line tool that runs several agents in parallel on a task, with reviewers working in the background. Meta says it also handles computer use across browsers, desktop, and mobile apps.

**Muse Wonder isn't in the Muse assistant yet.** Meta's newsroom post says it "will soon power more of Muse, rolling out first to subscribers," and Meta hadn't updated that as of November 3.

### API pricing and what a task costs

**Muse Wonder costs $4 per million input tokens, $0.20 per million cached input tokens, and $20 per million output tokens on the Meta Model API.** That's the same list price as Claude Opus 5.5, and the price Google says Gemini 4 Carbon will move to after its $2 and $10 introductory period.

The API is OpenAI-compatible. Meta says you can point an existing OpenAI client at Model API, swap the base URL, and set the model to muse-wonder-1.0.

Artificial Analysis puts Muse Wonder at $6.95 per Intelligence Index task, against $7.76 for Gemini 4 Carbon and $10.04 for Claude Opus 5.5. It's also fast: at about 196 output tokens per second, Artificial Analysis calls it "notably fast."

For coding agents, Artificial Analysis measured Muse Wonder in Muse Code at $8.05 per task.

## Active memory and active focus, explained

**Active memory and active focus are the two features Meta built into both Muse Wonder models** to make long-running agents more reliable.

**Active memory** is the model's ability to "carry what matters to you across conversations and bring back the details that are relevant to the task at hand," in Meta's words. It keeps goals, constraints, and earlier decisions available as the work goes on.

Here's how that could look in practice (this example is ours): you tell Muse Wonder early in a project that your app has to run on a specific database version, and days later, in a new conversation, it applies that constraint without you repeating it.

**Active focus** is the ability to "keep working toward your goal when a task gets hard, try other approaches, recover from errors, and follow through." Meta also trained it to ask for clarification when needed and check in before important actions.

Another hypothetical: if a build fails halfway through a migration, active focus is meant to have the agent try a different fix and keep going, then check with you before something like deleting a table.

Meta hasn't published how either feature works under the hood, and independent tests of them are limited so far. Meta's own MemoryBench, which tests remembering context, has both Wonder models at about 85.5%, ahead of Opus 5.5 (81.5%) and GPT-6 Astra (81.4%).

## Muse Wonder benchmarks: Meta's numbers vs. independent tests

**Muse Wonder Full leads several of Meta's own benchmarks, and independent scores put both models a step behind the current leaders.**

### Meta's own numbers

Meta's developer blog compares both Wonder models with Claude Fable 5.1, GPT-6 Astra, and Claude Opus 5.5, all at maximum effort. Here are the headline results:

| 📊 Benchmark | 🧪 What it tests | 🤖 Wonder Full | 🤖 Wonder | 📉 Best rival | 
|---|---|---|---|---|
| Harvey LAB-AA | Legal agent work | 13.1% | 10.0% | GPT-6 Astra, 8.6% | 
| DeepSWE 1.1 | Long-horizon coding | 79.0% | 78.8% | Opus 5.5, 74.2% | 
| SWE-Marathon | Very long engineering tasks | 57.5% | 53.1% | Opus 5.5, 46.3% | 
| AutomationBench-AA | Business workflows | 45.4% | 39.1% | GPT-6 Astra, 41.6% | 
| MemoryBench | Remembering context | 85.5% | 85.6% | Opus 5.5, 81.5% | 
| Vals Finance Agent V2 | Financial research | 59.1% | 56.6% | Fable 5.1, 58.9% | 
| Terminal-Bench 4.0 (AA) | Terminal coding | 57.1% | 52.0% | Opus 5.5, 59.6% | 
| ProgramBench | Programming from scratch | 59.0% | 50.0% | Opus 5.5, 70.5% | 
| OSWorld V2.1 (partial) | Computer use | 78.4% | 72.6% | Opus 5.5, 81.8% | 

Meta says Wonder Full "tops coding benchmark leaderboards, including DeepSWE and SWE-Marathon in independent third-party runs." Meta's model card notes that Wonder Full's SWE-Marathon score used the "xhigh" reasoning setting, one step below the max setting used for the rest of the table, and that the row isn't a same-harness comparison.

The same table shows where rivals still lead. Opus 5.5 wins HealthBench Pro, JobBench, Terminal-Bench, ProgramBench, and OSWorld's partial score. Claude Fable 5.1 wins OSWorld's pass-or-fail score, and GPT-6 Astra wins GDP.pdf, a test of professional document work.



### What Artificial Analysis found

**On Artificial Analysis's current Intelligence Index v5, Muse Wonder Full scores 50 and Muse Wonder scores 43.** That puts Wonder Full seventh, behind Gemini 4 Carbon (59), Claude Opus 5.5 (58), and GPT-6 Astra (57) at the top, and level with Gemini 4 Argon (50).

Meta's table shows higher scores (56 and 52) because they come from the older version of the index at launch. Artificial Analysis switched to v5, a harder mix of nine tests, on October 27, so the two sets of numbers can't be compared directly.

**A few independent results stand out:**

- **Office work:** on AA-Briefcase, which tests spreadsheets, slides, and documents, Wonder Full's 1,729 Elo ranks third, behind Claude Sonnet 5.5 and Claude Opus 5.5.
- **Coding agents:** running in Muse Code, Wonder Full scores 58.9 on Artificial Analysis's Coding Agent Index, close to Gemini 4 Carbon in Google's Antigravity CLI (59.4) and behind Claude Code with Opus 5.5 (61.7). Muse Wonder scores 52.0.
- **Hallucinations:** Wonder Full's hallucination rate on AA-Omniscience is 14%, against 51% for Opus 5.5.
- **Professional tasks:** on SemiAnalysis's GDPx benchmark, Wonder Full scores 9.0%, behind Opus 5.5 (12.0%), Gemini 4 Carbon (10.9%), and GPT-6 Astra (9.6%).

### Where Muse Wonder trails

**Muse Wonder trails the leaders in a few areas,** and both Meta's and independent numbers show where:

- **General intelligence:** 8 to 9 points behind the top two on Artificial Analysis for Wonder Full, and 15 to 16 points behind for Muse Wonder.
- **Knowledge:** Wonder Full scores 31 on AA-Omniscience, against 54 for Gemini 4 Carbon and 48 for Opus 5.5.
- **Writing code from scratch:** Opus 5.5 leads ProgramBench by more than 11 points in Meta's own table.
- **Health questions:** both Opus 5.5 and GPT-6 Astra beat Wonder Full on HealthBench Pro.

## What Muse Wonder is good at, and what it can't do yet

**Meta built Muse Wonder for long, multi-step jobs,** and its developer blog lists five areas it targets:

- **Multi-step reasoning:** keeping a coherent plan across long, complex workflows
- **End-to-end agent work:** planning, calling tools in sequence, recovering from failures, and finishing multi-turn requests
- **Coding agents:** multi-file refactors, debugging, and long-running tasks across large repositories
- **Tool use:** diagnosing errors and recovering when a tool call fails
- **Multimodal reasoning:** reading screenshots, charts, documents, images, and video alongside the conversation

The benchmarks back up the coding and legal claims best. Wonder Full leads Meta's long-horizon coding tests and Harvey's legal agent benchmark, and its Coding Agent Index score is within three points of the top setup.

**What it can't do yet:**

- **Audio isn't guaranteed.** Meta says "audio-dependent use cases require confirmation of audio support on the release surface."
- **Wonder Full isn't on the API.** As of November 3, you can't build a product on Meta's strongest model.
- **It isn't in the Muse assistant yet.** Meta says that's coming soon, for subscribers first.
- **Meta hasn't published its full safety report yet.** Its model card includes risk assessments and some safety results, but the newsroom post still says a fuller report "will be available soon."

## Muse Wonder vs. Muse Spark Open

**Muse Wonder and Muse Wonder Full are proprietary, and Muse Spark Open is Meta's open-weight model.** The names are easy to mix up because Meta released them days apart.

Muse Spark Open came out on October 15, three days after Muse Wonder, according to its license. You can download it from Hugging Face after Meta approves your access request. It has about 816 billion total parameters with 42 billion active per token.

Spark Open sits well below both Wonder models on capability. Artificial Analysis scores Muse Spark Open at 25 on its Intelligence Index v5, against 43 for Muse Wonder and 50 for Wonder Full.

Spark Open's Muse Model License allows commercial use, fine-tuning, and redistribution. Products built on it must display "Built with Muse Spark Open," and companies that sell model access as a service with more than $20 million in yearly revenue need a separate agreement with Meta.

If you need open weights, our guides to Kimi K3 and Mistral Large 4 cover two other large open-weight models.



## Should you use Muse Wonder?

I also tested the Muse agent against Lindy on real errands and customer work in this Meta Muse vs Lindy write-up.

**Muse Wonder is worth testing for long coding jobs, legal and finance work, and agents that need to remember context, and easy to skip if you need the strongest general model.**

**Try Muse Wonder if you:**

- Run long, multi-file coding jobs, where Wonder Full leads Meta's DeepSWE and SWE-Marathon tests
- Do legal or financial research, where Wonder Full tops Harvey's legal benchmark and edges out Claude Fable 5.1 on Vals Finance Agent in Meta's table
- Build agents that need to carry context across sessions
- Already pay for Muse Code, since every plan includes Muse Wonder
- Want a fast API model at $4 input and $20 output per million tokens, with about 196 output tokens per second

**Skip it for now if you:**

- Want the highest-scoring model overall (Gemini 4 Carbon, Claude Opus 5.5, and GPT-6 Astra all score higher on Artificial Analysis)
- Need Meta's best model in your own product, since Wonder Full isn't on the API
- Need open weights, since neither Wonder model has them
- Need audio input, since Meta says audio support has to be confirmed on each surface

For a closer look at the current leaders, see our Gemini 4 Carbon vs. Claude Opus 5.5 comparison, or our GPT-6.1 Terra guide for OpenAI's latest.

If you'd rather not track which model wins each week, Lindy, an AI teammate that works in Slack, lets you pick your model for each task from most major models, so you can switch when a new one pulls ahead.

## FAQ

### Is Muse Wonder free?

**No, Muse Wonder isn't free.** On the Meta Model API it costs $4 per million input tokens and $20 per million output tokens, and the cheapest way to use it in Muse Code is the Everyday Usage plan at $5 a month. Meta says it will come to the Muse assistant, starting with subscribers.

### What is Muse Wonder Full?

**Muse Wonder Full is the more powerful of Meta's two Muse Wonder models,** released alongside Muse Wonder on October 12, 2026. It's only available in Muse Code's Power Usage ($50 a month) and Maximum Usage ($200 a month) plans, and it isn't sold through the API.

### Is Muse Wonder open source?

**No, Muse Wonder and Muse Wonder Full are both proprietary models.** Meta's open-weight model is Muse Spark Open, released on October 15 under Meta's Muse Model License. It scores well below the Wonder models on Artificial Analysis.

### Is Muse Wonder better than Claude Opus 5.5?

**Not overall.** Claude Opus 5.5 scores 58 on Artificial Analysis's Intelligence Index v5, against 50 for Muse Wonder Full and 43 for Muse Wonder. Wonder Full does beat Opus 5.5 on several of Meta's own benchmarks, including legal agent work, long-horizon coding, and memory, and Muse Wonder costs less per task at the same $4 and $20 list price.

### How do I use Muse Wonder?

**You can use Muse Wonder through the Meta Model API, OpenRouter, Cursor, or Muse Code.** For the API, generate a key at Meta's developer site, point an OpenAI-compatible client at Model API, and set the model to muse-wonder-1.0. For Muse Code, install the command-line tool, pick a plan, and choose Power Usage or Maximum Usage if you want Muse Wonder Full.

## About the editorial team

**Lindy Drope**Founding GTM at Lindy

Lindy leads GTM at Lindy and is the team’s most prolific automation builder. She publishes weekly educational videos and articles on building AI assistants – And yes, she’s a real person!
