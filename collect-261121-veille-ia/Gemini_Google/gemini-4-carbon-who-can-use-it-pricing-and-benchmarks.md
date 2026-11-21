---
id: collect-261121-veille-ia/gemini-google/gemini-4-carbon-who-can-use-it-pricing-and-benchmarks
title: "Gemini 4 Carbon: Who Can Use It, Pricing, and Benchmarks"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "google", "carbon", "artificial", "analysis", "model", "november", "input"]
source: https://www.lindy.ai/blog/gemini-4-carbon
source_anchor: ""
source_lines: [1, 226]
section: "Gemini_Google"
---
Google announced Gemini 4 Carbon on October 28, and the same day it took the #1 spot on Artificial Analysis's Intelligence Index (Claude Fable 5.2 has since passed it), but almost nobody can use it yet.

Carbon is Google's strongest model, built for long engineering jobs and cybersecurity, and it went to a small group of cyber defenders first while developers, businesses, and Gemini app users wait for a date.

I went through Google's launch post and its full benchmark table, Google's November 2 update, Artificial Analysis's results, the Fairwind Program page, the Gemini API docs, the Gemini app release notes, and Google's AI plans page on November 3. Here's what Gemini 4 Carbon is, who can use it right now, what it will cost, how its benchmarks hold up, and what changed from Gemini 4 Argon.

**TL;DR:**

- **What it is:** Google DeepMind's newest frontier model, announced October 28, 2026, and built for long-running coding, agentic work, and cyber defense.
- **How good it is:** #2 on the Artificial Analysis Intelligence Index v5.0.1 at 60, behind Claude Fable 5.2 (65) and ahead of Claude Opus 5.5 (59) and GPT-6 Astra (57).
- **Who can use it:** as of November 3, only U.S. government pre-release testers and a subset of cyber defenders in Google's Fairwind Program. More trusted partners come next, then developers, businesses, and consumers "as soon as possible," with no date.
- **Where it isn't yet:** the Gemini API, Google AI Studio, the Gemini app, Google Cloud, and every Google AI plan.
- **Price:** $2 per million input tokens and $10 per million output tokens at launch, rising to $4 and $20 after an introductory period Google hasn't dated. Cached input is 95% off.
- **What changed from Argon:** 10 points higher on Artificial Analysis, much stronger cyber scores, roughly half the unsupported claims, and 0.0% success for prompt-injection attacks in Gray Swan's test.

## What is Gemini 4 Carbon?

**Gemini 4 Carbon is Google's newest and strongest AI model,** announced on October 28, 2026, by Koray Kavukcuoglu, Google's Chief AI Architect. Google's launch post describes it as its "strongest model with state-of-the-art cybersecurity and long horizon capabilities."

It arrived 15 days after Gemini 4 Argon became generally available, which makes it the second Gemini 4 model in a month. Google says Carbon "illustrates the pace at which we're pushing the frontier forward." On November 18, Google followed with Gemini 4 Neon, a faster model adapted from Carbon, and our Gemini 4 Neon vs Gemini 4 Carbon comparison covers when each one makes sense.

Carbon is built for jobs that take a long time. Google says its teams have seen it "run for days," working through dead ends to root-cause and patch subtle bugs across large codebases and low-level systems.

**The specs, from Google's launch post and** **Artificial Analysis****:**

| 🔧 Spec | 📋 Gemini 4 Carbon | 
|---|---|
| Made by | Google DeepMind | 
| Announced | October 28, 2026 | 
| Who can use it | U.S. government testers and a subset of cyber defenders in the Fairwind Program (as of November 3, 2026) | 
| API model ID | Not published yet | 
| Launch price | $2 input, $10 output per 1M tokens | 
| Standard price | $4 input, $20 output per 1M tokens, after the introductory period | 
| Cached input | 95% off the input price | 
| Context window | 1 million tokens (per Artificial Analysis) | 
| Inputs and output | Text, images, video, and speech in, text out (per Artificial Analysis) | 
| Open weights | No | 
| Intelligence Index | 60 on Artificial Analysis's v5.0.1 index, behind only Claude Fable 5.2 (65) and ahead of Claude Opus 5.5 (59) and GPT-6 Astra (57), as of November 12, 2026 | 

Google hasn't published Carbon's model ID, output limit, or knowledge cutoff, so this table takes the context window and input types from Artificial Analysis.

## Who can use Gemini 4 Carbon right now?

**As of November 3, 2026, Gemini 4 Carbon is only available to U.S. government pre-release testers and a subset of cyber defenders in Google's Fairwind Program.** It isn't in the Gemini API, Google AI Studio, the Gemini app, or Google Cloud's Gemini Enterprise Agent Platform (formerly Vertex AI), and none of Google's AI plans list it yet.

Google's launch post describes the rollout this way:

1. **U.S. government pre-release testing.** Google says it's "actively engaged in the U.S. government's voluntary process for pre-release model testing."
2. **A subset of cyber defenders, starting the week of October 28.** Google's November 2 update says this group gets Carbon "via the Fairwind Program," which fits where Carbon's biggest gains are.
3. **More trusted partners through the Fairwind Program.** Google will "gradually phase access" to them.
4. **Developers, businesses, and consumers.** Google says Carbon will reach its "product surfaces as soon as possible," after it keeps gathering feedback and iterating on guardrails.

On November 3, I checked the places Carbon would show up first. The Gemini API's changelog has no Carbon entry (its latest update, on October 30, is about Computer Use), the API pricing page and models list still top out at Argon, the Gemini app's release notes don't mention Carbon, and Google's AI plans page lists Argon for Google AI Pro and Ultra with no mention of Carbon.

One company outside Google's cyber program has said it's coming. Perplexity CEO Aravind Srinivas posted on October 30 that Carbon is "coming soon to all Perplexity Pro and Max users inside Computer, once we have sufficient token capacity," without a date. Google hasn't confirmed that.

For comparison, Argon was announced on September 30 and opened to paid users on October 13, two weeks later. Google hasn't said Carbon will follow the same schedule.

### How the Fairwind Program works

**The Fairwind Program is Google DeepMind's early-access program for cyber defenders.** Its program page says it gives "high-priority defenders (like governments, healthcare providers, and telecommunications services) early access to advanced models," and that it works with more than 650 partners worldwide.

Google prioritizes three groups:

- **Governments and national cyber authorities**
- **Critical infrastructure operators** in healthcare, telecommunications, energy, and finance
- **Core technology platforms** whose software millions of people depend on

To apply, Google says to contact your Google Cloud account team. Applicants go through background checks, members can only use the models for defensive and research work, and they can't share or resell access. As of November 3, the program page still describes exclusive access to Gemini 4 Argon and doesn't mention Carbon yet.

### What to use while you wait

**Gemini 4 Argon is the closest thing you can use today.** It's in the Gemini API for paid developers, and in the Gemini app for Google AI Pro ($19.99 a month) and Ultra (from $99.99 a month) subscribers. Our Gemini pricing guide covers the plans, and our Argon explainer covers where it runs.

If you need a frontier model in production this week, Claude Opus 5.5 and GPT-6 Astra are both generally available and score within three points of Carbon on Artificial Analysis.

## Gemini 4 Carbon pricing

**Gemini 4 Carbon will launch at $2 per million input tokens and $10 per million output tokens,** the same introductory price Argon launched at, with cached input tokens priced 95% below the input rate. A footnote in Google's post adds that "the price of $4 per 1M input tokens and $20 per 1M output tokens will apply" once the introductory period ends.

| 💵 Price per 1M tokens | 📥 Input | 💾 Cached input | 📤 Output | 
|---|---|---|---|
| Gemini 4 Carbon, introductory | $2.00 | $0.10 | $10.00 | 
| Gemini 4 Carbon, standard | $4.00 | $0.20 | $20.00 | 
| Gemini 4 Argon, through Nov 30 | $2.00 | $0.10 | $10.00 | 
| Gemini 4 Argon, from Dec 1 | $4.00 | $0.20 | $20.00 | 

Carbon's prices come from Google's post and Artificial Analysis, and Argon's come from the Gemini API pricing page.

**Google hasn't said when Carbon's introductory price ends.** Artificial Analysis describes it as a 50% launch discount "with no confirmed end date." Argon's discount runs through November 30, so budget Carbon at $4 and $20 if you're planning past the introductory period.

### What it costs per task

**Carbon will cost less per job than Claude Opus 5.5, even though both list at $4 and $20 per million tokens at standard pricing.** Artificial Analysis puts Carbon at $7.76 per Intelligence Index task at standard pricing, about 23% less than Opus 5.5 ($10.04), because Carbon writes about 123,000 output tokens per task against Opus 5.5's 173,000.

GPT-6 Astra is cheaper still at $6.24 per task, and Argon costs $9.95 at the same $4 and $20 price while scoring 10 points lower. For comparison, Artificial Analysis puts Meta's cheapest model, Muse Ember, at about $0.14 per task.



## Gemini 4 Carbon benchmarks: Google's numbers vs. independent tests

**Gemini 4 Carbon leads most of the benchmarks Google published, and the independent results put it second overall, behind only Claude Fable 5.2, while showing a few areas where it trails.**

### Google's own numbers

Google's launch post includes a table of 17 benchmarks comparing Carbon with Argon, GPT-6 Astra, Claude Opus 5.5, and Claude Fable 5.1. Carbon has the top score on 15 of them. Here are the headline results:

| 📊 Benchmark | 🧪 What it tests | 🏆 Carbon | 📉 Next best | 
|---|---|---|---|
| SEC-bench Pro | Finding real vulnerabilities | 88.2% | GPT-6 Astra, 85.4% | 
| ExploitGym | Checking whether a flaw is exploitable | 54.8% | GPT-6 Astra, 42.4% | 
| CWE-bench | Patching the root cause | 71.0% | Argon and GPT-6 Astra, 68.0% | 
| Terminal-Bench 4.0 (AA) | Multi-step terminal work | 65.2% | Opus 5.5, 59.6% | 
| GraphWalks (256K to 1M) | Reasoning over very long inputs | 96.2% | Argon, 84.2% | 
| AutomationBench (Zapier) | Business workflows across apps | 53.9% | Argon, 51.3% | 
| Agents' Last Exam v1.1 | Computer use | 77.4% | GPT-6 Astra, 73.1% | 
| Vals Index v2.2 | Vals AI's overall index | 70.4% | Opus 5.5, 70.2% | 

"Next best" means among the models in Google's table. On Artificial Analysis's own Terminal-Bench 4.0 leaderboard, Kimi K3.1 Pro edges Carbon, 67.2% to 65.2%.

Google ran the three cyber benchmarks without guardrails. It also reports that Carbon jumped 89 Elo points to 1768 on WebDev Arena, which put it at #4 for building websites at launch.

Some of Google's strongest claims come from internal testing you can't check. Google says Carbon has more than twice Argon's success rate at generating secure code, and that Chrome's team found it more than 10 times better than "another lab's large frontier model" at writing high-quality patches.

### What Artificial Analysis found

**On** **Artificial Analysis****, Gemini 4 Carbon scores 60 on the Intelligence Index v5.0.1,** ahead of Claude Opus 5.5 at 59 and GPT-6 Astra at 57. Claude Fable 5.2, released November 9, now leads at 65. The index blends nine tests, and Carbon has the top score on four of them: HLE-Diamond, Terminal-Bench-Science, AA-Omniscience, and AutomationBench-AA.

Its knowledge results stand out. On AA-Omniscience, which penalizes confident wrong answers, Carbon scores 54 against 48 for Opus 5.5, and its hallucination rate is 11%, among the lowest Artificial Analysis has measured (only Argon and Carbon's own medium setting, both at 4%, are lower). Opus 5.5 and GPT-6 Astra both sit at 50%.

SemiAnalysis's GDPx benchmark, which grades 206 hard tasks across 11 fields, tells a value story. Carbon scores 10.9%, just behind Opus 5.5 at 12.0% and ahead of GPT-6 Astra at 9.6%, at $16.29 per task against $51.41 for Opus 5.5 and $53.99 for Astra. As 9to5Google noted, GPT-6 Astra moves to #1 if you drop the cybersecurity, biology, and alignment categories.

Artificial Analysis ran Carbon with high reasoning, while Opus 5.5 and Astra ran at max, so treat a one-point lead as close to a tie.

### Where Carbon doesn't lead

**Carbon trails in a few areas,** and both Google's table and Artificial Analysis show where:

- **Coding agents:** on Artificial Analysis's Coding Agent Index, Carbon in Google's Antigravity CLI scores 59, three points behind Claude Code running Opus 5.5 (62).
- **Computer use:** Opus 5.5 wins OSWorld-2.1 in Google's own table, 87.2% to Carbon's 81.5%.
- **Real-world software tasks:** GPT-6 Astra wins FrontierSWE 2, 65.5% to 61.2%.
- **Office work:** on AA-Briefcase, which tests spreadsheets, slides, and documents, Carbon's 1,610 Elo ranks eighth, well behind Claude Sonnet 5.5 (1,814) and Claude Opus 5.5 (1,801).
- **Hallucinations vs. Argon:** Carbon's hallucination rate rose from Argon's 4% to 11%, even as it answered 10 points more questions correctly.

## What changed from Gemini 4 Argon

**Carbon is a step up from Argon on almost every number Google and Artificial Analysis published,** two weeks after Argon reached paying users. Here's what changed:

- **Higher overall score:** 60 on Artificial Analysis, up 10 points from Argon's 50.
- **Longer jobs:** Google says Carbon is built for "sustained agentic workflows" and has run for days on hard engineering tasks.
- **Better cyber work:** more than 2x Argon's success rate at generating secure code in Google's internal tests, plus higher scores on all three cyber benchmarks (ExploitGym rose from 42.0% to 54.8%).
- **Fewer unsupported claims in Google's tests:** Google says Carbon cuts unsupported claims by about half compared with Argon, and it adds caveats to its work or lists what's still missing when it lacks evidence.
- **Better scope control:** Google says overreach is down 26% and under-service is down 29%, so it does more of what you asked and less of what you didn't.
- **Less cheating on tests:** successful cheating on Terminal-Bench 4.0 fell to 0.0%, with attempts at 0.4%, and high-severity misalignment alerts in Google's live coding environments dropped by almost 80%.
- **Stronger prompt-injection defense:** 0.0% attack success on Gray Swan's indirect prompt injection benchmark, down from Argon's 0.7%.
- **Better web output:** up 89 Elo points to 1768 on WebDev Arena.

Carbon's pricing mirrors Argon's. It launches at the same $2 and $10 introductory rates Argon had and moves to the same $4 and $20 standard price, so the upgrade costs about the same per task.

The one trade-off in the numbers is hallucination. Carbon knows more, but when it's wrong it's more often confidently wrong than Argon was.

## Gemini 4 Carbon vs. Claude Opus 5.5 and GPT-6 Astra

**Carbon leads overall by a point or two, Opus 5.5 still leads coding agents, OSWorld-2.1 computer use, and office work, and both rivals are available to use today.**

Claude Opus 5.5 has been on the Claude API and paid Claude plans since September 22, and GPT-6 Astra is generally available too. Carbon will cost less per task than Opus 5.5 at list price and more than Astra.

For the full head-to-head, including specs, costs on a real workload, and which to pick for each kind of job, see our Gemini 4 Carbon vs. Claude Opus 5.5 comparison. For how Argon stacks up against OpenAI's flagship, see Gemini 4 Argon vs. GPT-6 Astra, and for Google's decision model, which scores options rather than writing text, see Gemini Decision 1 vs GPT-6 Luna.



## Should you wait for Gemini 4 Carbon?

**Wait for Carbon if your work matches its strengths, and use what's available if you need a model this week.**

**Watch for Gemini 4 Carbon if you:**

- Do vulnerability research, patching, or other security work, where it posts the top scores on SEC-bench Pro, ExploitGym, and CWE-bench
- Need answers you'll quote, since it has the best AA-Omniscience score and a lower hallucination rate than Opus 5.5 and GPT-6 Astra
- Work with very long documents or codebases, where it scores 96.2% on GraphWalks at 256K to 1M tokens
- Run long, multi-step agent jobs that need to stay on task for hours
- Already build on Argon and want an upgrade at the same price

**Use something else for now if you:**

- Need a frontier model in production today (Claude Opus 5.5 and GPT-6 Astra are both available)
- Mostly run coding agents, where Claude Code running Opus 5.5 still leads Artificial Analysis's Coding Agent Index
- Want a Google model right away, in which case Argon is in the Gemini API and the Gemini app

If you'd rather not track which model wins each week, Lindy, an AI teammate that works in Slack, lets you pick your model for each task from most major models, so you can switch when a new one pulls ahead.

## FAQ

### When can I use Gemini 4 Carbon?

**Google hasn't given a date.** As of November 3, 2026, Carbon is limited to U.S. government pre-release testing and a subset of cyber defenders in the Fairwind Program, with more trusted partners being added gradually. Google says developers, businesses, and consumers will get it "as soon as possible." Argon reached paying users two weeks after it was announced, but Google hasn't said Carbon will follow the same timeline.

### How much does Gemini 4 Carbon cost?

**Gemini 4 Carbon will cost $2 per million input tokens and $10 per million output tokens at launch,** with cached input 95% off. After an introductory period Google hasn't dated, the price rises to $4 and $20, the same standard price as Gemini 4 Argon.

### Is Gemini 4 Carbon better than Gemini 4 Argon?

**Yes, on almost every published benchmark.** Carbon scores 60 on Artificial Analysis's Intelligence Index against Argon's 50, beats it on all three of Google's cyber benchmarks, and cuts unsupported claims by about half in Google's testing. Argon still has a lower hallucination rate (4% vs. 11%), and Argon is the one you can use today.

### Is Gemini 4 Carbon in the Gemini app?

**Not yet.** As of November 3, 2026, the Gemini app's release notes and Google's AI plans page don't mention Carbon. Google AI Pro and Ultra subscribers get Gemini 4 Argon in the app.

### Does Gemini 4 Carbon have an API?

**Not yet.** Google hasn't published a model ID, and the Gemini API changelog, models list, and pricing page don't include Carbon as of November 3. When it arrives, Google has already said it will cost $2 and $10 per million tokens at launch.

### What is the Fairwind Program?

**The Fairwind Program is Google DeepMind's early-access program for cyber defenders,** with more than 650 partners, including governments, critical infrastructure operators, and core technology platforms. Members get early access to advanced models, currently a cyber-defense version of Gemini 4 Argon, for defensive and research work. You apply through your Google Cloud account team, and Google vets every applicant.

### Is Gemini 4 Carbon the best AI model right now?

**Not anymore on Artificial Analysis's Intelligence Index.** Claude Fable 5.2 took the top spot on November 9 with 65, and Carbon is second at 60, just ahead of Claude Opus 5.5 (59) and GPT-6 Astra (57). Opus 5.5 still leads coding agents, OSWorld-2.1, and office work, and Carbon isn't publicly available yet, so the best model you can use today depends on your job.

## About the editorial team

**Michelle Liu**Senior Product Manager

Michelle is a Senior Product Manager at Lindy. She’s focused on making Lindy the most powerful yet easy-to-use AI workflow automation app.
