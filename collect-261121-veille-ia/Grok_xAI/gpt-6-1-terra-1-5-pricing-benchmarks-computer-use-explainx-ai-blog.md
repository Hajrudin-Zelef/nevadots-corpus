---
id: collect-261121-veille-ia/grok-xai/gpt-6-1-terra-1-5-pricing-benchmarks-computer-use-explainx-ai-blog
title: "GPT-6.1 Terra: $1/$5 Pricing, Benchmarks, Computer Use | explainx.ai Blog"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["terra", "gpt-6", "openai", "model", "computer", "price", "october", "says"]
source: https://explainx.ai/blog/gpt-6-1-terra-launch-1-5-dollar-pricing-computer-use-benchmarks-2026
source_anchor: ""
source_lines: [1, 200]
section: "Grok_xAI"
---
OpenAI released **GPT-6.1 Terra** on October 26, 2026, a fast mid-tier model built around "fast and capable computer use at scale," per the official announcement. The headline: **$1 per million input tokens and $5 per million output tokens**, which OpenAI says is 50% below both GPT-5.6 Terra and GPT-6.1 Sol. It is live in ChatGPT Work and Codex for paid plans, and in the API as `gpt-6.1-terra`.

This is the model that The Information's October report hinted at when it mentioned "a potential new Terra model." That rumor is now confirmed. Below is what OpenAI actually published, which claims are its own, and how to decide whether to switch.

## Update (October 27, 2026): an OpenAI engineer explains where Terra fits

In a Hacker News comment under the Terra launch thread, OpenAI's Ted Sanders (posting as tedsanders and identifying as an OpenAI employee) laid out the company's own rule of thumb for choosing a model. The Decoder picked it up the same morning. This is one employee's guidance, not a formal OpenAI policy document.

- **Ordering:** the models run Luna, then Terra, then Sol, then Astra, from smallest to largest. Sanders says bigger models are generally more reliable, cost more per token and recall more obscure facts.
- **Starting point:** "If you care about quality only, pick Astra. If you're a bit price sensitive, pick Sol. If you're very price sensitive, pick Terra or Luna."
- **Terra specifically:** in his words, Terra is probably better than Astra and Sol at technical communication, similar on vision and computer use but much faster, and a bit worse on most other things. For API developers he calls it a midrange option, cheaper than Sol and much smarter than Luna. For subscribers he says it is "not a bombshell release," mainly useful if you hit usage limits or want clearer writing and faster computer use.
- **Why the names stay complicated:** model size, reasoning effort and multi-agent count are separate dimensions, and a single "IQ" label would hide details users need.

This lines up with OpenAI's own claim above that Terra matches Astra on its computer-use test at a fraction of the price, and with the independent Artificial Analysis score below. For the other tiers, see our GPT-6.1 Sol launch and GPT-6 Astra launch posts.

## Update (October 26, 2026): first independent score from Artificial Analysis

Artificial Analysis has listed the max-reasoning version of the model on its GPT-6.1 Terra page with a score of 48 on its Intelligence Index, ranked 25th of 221 models, against a median of 28 for its class. That is the first third-party number we have seen, and it is a different kind of measurement from the OpenAI benchmarks above.

What else the page lists, as of this writing:

- **Price:** $1.00 input and $5.00 output per million tokens, matching OpenAI's model docs and below the listed medians of $2.00 and $10.00.
- **Cost to run the index:** about $0.40 per task, ranked 32nd of 120, despite the low token price. The model is verbose: it generated 130M output tokens on the index versus a median of 81M. A cheap per-token price does not guarantee a cheap task.
- **Speed:** listed as unavailable, so the "fast" part of OpenAI's pitch is not yet independently measured there.
- **Specs:** text and image input, text output, 1M-token context, one API provider.

The score was also posted to Hacker News, where it had 24 points at the time of writing. Treat the ranking as a snapshot: Artificial Analysis notes a non-reasoning variant may exist, and its numbers can change as providers and settings are added.

### Also on October 26: GPT-6.1 Terra is generally available in GitHub Copilot

GitHub's changelog entry says GPT-6.1 Terra is now generally available in Copilot, aimed at everyday agentic coding and terminal work at a lower cost than GPT-6.1 Sol. In early testing GitHub says it solved noticeably more terminal and C# tasks than GPT-5.6 Terra and works in focused, consolidated steps on well-scoped tasks.

- **Plans:** Copilot Pro, Pro+, Max, Business and Enterprise, with a gradual rollout.
- **Where:** the model picker in VS Code, Visual Studio, Copilot CLI, the cloud agent, the Copilot app, github.com, GitHub Mobile, JetBrains IDEs, Xcode and Eclipse.
- **Billing:** provider list pricing under usage-based billing, so the $1 and $5 per million tokens above is the reference.
- **Admins:** Business and Enterprise admins can control access through the model policy; under default enablement new models switch on automatically unless the global default or this model is turned off.

## TL;DR: GPT-6.1 Terra at a glance

| Question | Answer | 
|---|---|
| What is it? | A fast, efficient model for computer use, visual reasoning and writing; successor to GPT-5.6 Terra | 
| API name | `gpt-6.1-terra` | 
| Price (standard) | $1 input, $0.05 cached input, $5 output per million tokens | 
| Versus GPT-5.6 Terra and GPT-6.1 Sol | 50% lower standard API prices, per OpenAI | 
| Where it ships | ChatGPT Work and Codex (Plus, Pro, Enterprise, Edu) and the API | 
| Computer use | Agents' Last Exam: +15 points over GPT-5.6 Terra at about one-third the cost (OpenAI) | 
| Coding | Terminal-Bench 4.0: 50.3% at max effort vs 21.5% for GPT-5.6 Terra (OpenAI) | 
| Errors | Error rate on error-inducing prompts: 53.6% to 16.7% at low effort; 4.3% at max effort (OpenAI) | 
| Where it loses | OpenAI says GPT-6.1 Sol and GPT-6 Astra remain better for the most complex coding | 
| Independent numbers? | Not yet | 

## What is GPT-6.1 Terra, and where does it sit?

OpenAI's lineup has been reshuffled several times this year. Terra started as the middle tier of the GPT-5.6 family, between Sol and Luna, as covered in our GPT-5.6 Sol, Terra and Luna preview and the later GPT-5.6 Terra and Luna price cuts. When the GPT-6 Sol and Luna launch happened in September, no GPT-6 Terra shipped. The GPT-6 Astra launch then put a premium flagship on top, and GPT-6.1 Sol replaced the first Sol after about a week.

GPT-6.1 Terra fills the gap. OpenAI frames it as continuing to "advance the cost-intelligence frontier across our model families, providing a new balance of speed, capability, and cost." In plain terms: it is not the smartest model OpenAI sells, it is the one meant for work you run thousands of times.

The examples OpenAI gives are deliberately workaday: drafting proposals, interpreting technical drawings, filling out forms, editing videos, and "reading incoming invoices and entering their details into accounting software." That last one is the archetype of a high-volume computer-use job, where per-task cost matters more than peak reasoning.

## How much does GPT-6.1 Terra cost?

A price tag with a downward arrow showing the GPT-6.1 Terra API price falling by half

OpenAI lists the standard API prices as:

| Token type | Price per million tokens | 
|---|---|
| Input | $1.00 | 
| Cached input | $0.05 | 
| Output | $5.00 | 

OpenAI says these are 50% lower than GPT-5.6 Terra and GPT-6.1 Sol. For context, our earlier coverage documented GPT-6.1 Sol's list price at $2 input and $10 output per million tokens (see the GPT-6.1 Sol cost-efficiency fact check and OpenAI's GPT-6.1 Sol model page), so the math is consistent: Terra is exactly half of Sol on both input and output.

The cached-input price is the quiet detail. At $0.05 per million, a cached prompt prefix costs one-twentieth of fresh input. Computer-use agents resend a growing history of screenshots, tool calls and instructions on every step, so cache hits will dominate real bills. If you have not tuned for caching yet, our token price history and timeline explains why prices keep falling, and why the cheapest token is still the one you never resend.

One more pricing point from the announcement concerns subscriptions, not the API: in ChatGPT Work and Codex, OpenAI says "your plan goes further with GPT-6.1 Terra." The same usage allowance covers more tasks than with GPT-5.6 Terra, and "substantially more" than with GPT-6.1 Sol or GPT-6 Astra. OpenAI gave no exact multipliers in the announcement, so treat that as direction, not a number. If you are watching plan limits, see our note on Pro usage changes.

## What does OpenAI claim about computer use?

A hand dragging a tile into a tray, illustrating OpenAI computer use agents moving through desktop tasks

The central claim is about computer use, measured on **Agents' Last Exam**, a benchmark of professional tasks. OpenAI's examples are turning architectural drawings into a 3D model and reviewing an animated film shot by shot to produce a time-coded log for a restoration team. Its figures:

- At **medium reasoning effort** , GPT-6.1 Terra scores**15 percentage points higher** than GPT-5.6 Terra at about**one-third the cost** .
- It "performs on par with GPT-6 Astra at only one-tenth its price per token."
- At **max effort** , Terra is "over three times as token efficient as Claude Opus 5.5, and one-fourth its price per token, resulting in a ~9x lower cost per task."
- It completes tasks "over three times faster" than both GPT-6 Astra and GPT-6.1 Sol.

Read the comparison carefully. The 9x figure is cost per task against Opus 5.5 at max effort, on OpenAI's own run of OpenAI's chosen benchmark. The arithmetic hangs together (one-fourth the price times roughly one-third the tokens is about one-twelfth, so 9x is plausible once other factors are included), but it is a vendor figure. For how the rival model performs on independent indices, see our post on Opus 5.5 topping the Epoch Capabilities Index.

OpenAI also reports gains on two other tests:

- **MMMU-Pro** (visual reasoning): Terra beats GPT-5.6 Terra at every reasoning setting and "nearly matches" GPT-6 Astra and GPT-6.1 Sol at much lower cost.
- **OSWorld 2.1** (multistep desktop tasks such as building a 3D model from a technical drawing): at medium effort, Terra surpasses GPT-5.6 Terra's best score at a fraction of the cost per task.

No raw OSWorld or MMMU-Pro scores were given in the text of the announcement, only relative statements, so we cannot put numbers in a table for those two.

## Coding: Terminal-Bench 4.0 doubles, with a caveat

On **Terminal-Bench 4.0**, which tests coding agents on command-line problems like fixing a webpage whose layout shifts as it loads, OpenAI says GPT-6.1 Terra solves **50.3%** of tasks at maximum effort. GPT-5.6 Terra's published score was **21.5%**, so this is more than double.

OpenAI is candid about the limit: "For the most complex coding tasks, GPT-6.1 Sol and GPT-6 Astra remain the better choice." That is a useful signal. Terra is pitched as the volume worker, not the model you give a hard repository-scale refactor.

A caution on the benchmark itself. Terminal-Bench 4.0 has been under scrutiny: we covered an audit claiming a large share of its tasks were flawed, and the benchmark's reliability is still being debated. A score on a contested benchmark is a data point, not a verdict, and the 21.5% to 50.3% jump is worth re-checking on your own repos.

OpenAI adds that on **Terminal-Bench-Science 0.1**, where agents use code to run scientific tasks such as tracking cell divisions in a time-lapse video, Terra at max reasoning surpasses GPT-6.1 Sol at lower cost, and comes "within five points of Opus 5.5 at less than one-fifth the estimated cost." Note the word "estimated."

## Fewer wrong answers: the factuality claim

OpenAI says it has "invested heavily in reducing hallucinations and factual errors" this year. On its internal benchmark of error-inducing prompts:

| Reasoning effort | GPT-5.6 Terra error rate | GPT-6.1 Terra error rate | 
|---|---|---|
| Low | 53.6% | 16.7% | 
| Max | about 22.3% (implied) | 4.3% | 

The low-effort figures are stated directly, a drop of nearly 37 points. For max effort OpenAI says the rate "fell by 18 percentage points to 4.3%," so the previous figure is implied rather than printed; we show it as implied for that reason. OpenAI says the 4.3% nearly matches GPT-6 Astra and GPT-6.1 Sol.

This is an internal benchmark of prompts designed to induce errors, so it says little about your domain. But the direction matters for the use case Terra targets: an agent that fills forms or enters invoice data has to be right, and a low-effort setting is where you save money.

## Safety, cybersecurity and prompt injection

OpenAI states that GPT-6.1 Terra "meets the Critical threshold in cybersecurity under our Preparedness Framework, and maintains the protections developed for GPT-6 Astra." That wording matters: a small, cheap model now sits at the framework's top cyber category, which means broad access comes with safeguards rather than an assumption of low risk.

On alignment, the company says Terra is more likely than GPT-5.6 Terra to stay within scope and to honestly disclose limitations and blockers. It also reports "strong resistance" to malicious instructions in its prompt-injection tests. Prompt injection is the central risk for computer-use agents, since a model that clicks around a browser will read untrusted pages. Treat the claim as a starting point: run your own injection tests before giving an agent credentials, and put guardrails around destructive actions. If you deploy agents with real access, AgentBeam is the agent security platform from the explainx.ai team, built to stop AI agents before they take dangerous actions.

## Terra vs Sol vs Astra: which model for which job?

This table uses only what OpenAI said. Where it gave no number, we say so.

| Job | Likely pick | Why | 
|---|---|---|
| High-volume form filling, invoice entry, screen-driven workflows | GPT-6.1 Terra | Half Sol's price, over 3x faster, computer-use strength (OpenAI) | 
| Visual reasoning over documents and drawings | GPT-6.1 Terra | MMMU-Pro near Astra and Sol at lower cost (OpenAI) | 
| Everyday drafting and writing | GPT-6.1 Terra | OpenAI cites clearer, more natural writing | 
| Hardest coding and repository-scale work | GPT-6.1 Sol or GPT-6 Astra | OpenAI says they remain the better choice | 
| Maximum reliability when cost does not matter | GPT-6 Astra | Premium flagship; no new Astra numbers in this launch | 
| Cheapest possible bulk text | GPT-6 Luna or a small model | Terra is mid-tier; see our small models comparison | 

## How to try GPT-6.1 Terra today

**In ChatGPT Work or Codex:** if you are on Plus, Pro, Enterprise or Edu, select GPT-6.1 Terra from the model picker. OpenAI says it is available starting today.

**In the API:** call the model by its identifier.

```
curl https://api.openai.com/v1/responses \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "gpt-6.1-terra", "input": "Extract the vendor, date and total from this invoice."}'
```
A sensible migration plan for an existing GPT-5.6 Terra or Sol workload:

1. Pick 50 to 100 real tasks from your logs, including the ugly ones.
2. Run them on Terra at low, medium and max effort and record accuracy, latency and cost per completed task, not per token.
3. Turn on prompt caching and measure the cached share of input.
4. Keep Sol or Astra as an escalation path for tasks Terra fails.
5. Add injection tests for any step where the agent reads untrusted content.

Cost per completed task is the number that matters. A cheaper model that needs two attempts can still lose to a pricier one that succeeds the first time, which is the same lesson from our GPT-6.1 Sol versus Astra cost analysis.

## What is still unconfirmed

- **No independent evaluations yet.** Every benchmark number above is OpenAI's. Independent indices from groups such as Artificial Analysis typically follow within days; we will add an update section when they appear.
- **Exact plan allowances.** OpenAI says Terra stretches ChatGPT Work and Codex limits further, but did not give a multiplier in the announcement.
- **Raw scores for OSWorld 2.1 and MMMU-Pro.** Only relative claims were given in the text.
- **Whether GPT-5.6 Terra is retired.** The announcement does not say.
- **Cost per task versus Opus 5.5.** The 9x figure depends on OpenAI's chosen benchmark and settings.

## Why it matters

Three things stand out. First, the price: a model that OpenAI says matches its flagship on a computer-use test at one-tenth the per-token price, if it holds up, changes the economics of agents that operate software. Second, the positioning is honest about limits, since OpenAI itself sends the hardest coding back to Sol and Astra. Third, it continues a pattern covered in our token price timeline: capability that cost flagship money a quarter ago is moving to the mid-tier.

The responsible reading is "promising, vendor-reported." Test it on your own tasks before you rewire a production pipeline.

## Related reading

- 
Tooling: LLM 0.37 adds gpt-6.1-terra to the command line
- 
How OpenAI's Ted Sanders explains the tier names and version numbers: Luna, Terra, Sol, Astra explained.
- 
GPT-5.6 Sol, Terra and Luna preview
- 
GPT-5.6 Luna and Terra price cuts
- 
GPT-6 Sol and Luna launch and pricing
- 
GPT-6 Astra launch, benchmarks and pricing
- 
GPT-6.1 Sol launch, pricing and benchmarks
- 
GPT-6.1 Sol and Astra cost efficiency, fact checked
- 
The Information's Terra report: what we knew
- 
Terminal-Bench 4.0 audit: claimed vs verified

*Model names, prices and benchmark figures are accurate as of October 26, 2026, and come from OpenAI's launch announcement. Check OpenAI's pricing page before budgeting.*
