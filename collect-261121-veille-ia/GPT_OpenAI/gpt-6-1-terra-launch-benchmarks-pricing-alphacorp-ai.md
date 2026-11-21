---
id: collect-261121-veille-ia/gpt-openai/gpt-6-1-terra-launch-benchmarks-pricing-alphacorp-ai
title: "GPT-6.1 Terra Launch: Benchmarks, Pricing | AlphaCorp AI"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-10-26"]
keywords: ["gpt-6", "terra", "october", "model", "openais", "launch", "astra", "agent"]
source: https://alphacorp.ai/blog/gpt-6-1-terra-launch-benchmarks-pricing-and-everything-you-need-to-know
source_anchor: ""
source_lines: [1, 219]
section: "GPT_OpenAI"
---
OpenAI released GPT-6.1 Terra on 26 October 2026 as a mid-tier model that claims GPT-6 Astra’s agent performance at a tenth of Astra’s per-token price. This guide to the GPT-6.1 Terra launch separates what’s confirmed (pricing, limits, system-card results) from the benchmark claims nobody outside OpenAI has replicated, so you can decide whether to migrate this week or wait. As of October 26, 2026, the prices are published and the benchmarks are one day old.

- **$1.00 per million input tokens and $5.00 per million output tokens** on OpenAI’s October 2026 API pricing, half of GPT-6.1 Sol’s rate and a tenth of Astra’s.
- **50.3% on Terminal-Bench 4.0** at max effort against 21.5% for GPT-5.6 Terra, as reported by OpenAI at the 2026 launch.
- **1,050,000-token context window** and a 1 June 2026 knowledge cutoff, per OpenAI’s API changelog entry for 26 October 2026 and the Terra model page.
- **13.75% unauthorized-action rate** (11 of 80 samples) when messaged by an external agent, against 1.25% for GPT-6.1 Sol, from OpenAI’s 2026 system-card addendum.
- **Intelligence Index score of 48** , ranking #25 of 221 models, on Artificial Analysis’s launch-day snapshot of 26 October 2026, with 130M output tokens used against a median of 81M.

## What Is GPT-6.1 Terra and Why This Launch Matters

GPT-6.1 Terra is OpenAI’s mid-tier GPT-6 model, released on 26 October 2026, and it matters because it brings the agentic and computer-use performance of the flagship GPT-6 Astra down to a fraction of Astra’s price. The GPT-6 family runs on a four-rung ladder. Luna is the cheapest, then Terra, then Sol, with Astra at the top. Terra sits one rung below Sol, which puts it in the slot most teams actually buy from when they run agents at volume.

OpenAI’s own description is short:

“a fast, efficient model for visual reasoning, precise computer use, and clear, natural writing.” (OpenAI API changelog, 26 October 2026)


That wording tells you where the model is aimed. Screenshots, browser sessions, form-filling, long runs of ordinary prose. Deep research and the hardest coding are explicitly handed off to Sol or Astra in OpenAI’s GPT-6.1 Terra launch announcement, which also claims parity with Astra on the company’s toughest agent benchmark and task completion more than 3x faster than both Astra and Sol.

**The GPT-6 release timeline, per OpenAI’s changelog:**

- 3 September 2026: GPT-6 Astra
- 22 September 2026: GPT-6 Luna and GPT-6 Sol
- 29 September 2026: GPT-6.1 Sol
- 26 October 2026: GPT-6.1 Terra

Terra was the last rung to move. The model it replaces, GPT-5.6 Terra, never received a GPT-6 version at all and carries a February 2026 knowledge cutoff. So for most of autumn 2026 the mid tier was running on a model that knew nothing after February while the top and bottom of the ladder had been refreshed. Overdue is the right word.

Why does a mid-tier refresh deserve this much attention? Because in a production agent, the model that does the clicking is the cost of the agent. A computer-use workflow might call the model fifty times per task, and at that frequency the per-token price of the flagship stops being a rounding error and becomes the budget line that kills the project. OpenAI’s pitch for Terra is that you no longer have to pick between Astra-class agent performance and a bill your finance team will sign off on. Whether the pitch holds is the question the rest of this piece works through.

## How GPT-6.1 Terra Performs on Reasoning, Coding and Multimodal Benchmarks

On OpenAI’s own numbers, GPT-6.1 Terra matches GPT-6 Astra on Agents’ Last Exam, more than doubles GPT-5.6 Terra’s Terminal-Bench 4.0 score, nearly matches Sol and Astra on MMMU-Pro, and finishes tasks more than 3x faster than the two larger models. Every figure in this section is OpenAI-reported. No independent replication existed on launch day, 26 October 2026, beyond a single aggregator snapshot, so read each number as a vendor claim with a date attached.

**Agents’ Last Exam** is the headline result. The Agents’ Last Exam benchmark paper on arXiv, posted in June 2026 and revised in October 2026, holds more than 1,000 real-world tasks across 55 subfields, and the average full pass rate on its hardest tier sits below 1%. OpenAI says Terra “substantially outperforms” GPT-5.6 Terra and matches Astra. Coverage of the launch charts puts the gain at about 15 points at medium effort, at roughly a third of GPT-5.6 Terra’s cost. Matching Astra on a test where almost nobody fully passes the hardest tier is a statement about relative ranking. Treat it that way.

**Terminal-Bench 4.0** measures agentic coding inside a terminal. At max effort, Terra scores 50.3% against 21.5% for GPT-5.6 Terra in OpenAI’s 2026 figures. That’s a stark jump for one generation. And yet OpenAI itself says Sol and Astra remain the better choice for the hardest coding, which tells you the 50.3% still trails the top of the family.

**MMMU-Pro** tests multimodal reasoning and dates from ACL 2025. Terra beats GPT-5.6 Terra at every effort setting and lands just short of Sol and Astra. Given the “visual reasoning” positioning, this is the result to care about if your workload is documents, screenshots or charts.

**OSWorld 2.1** covers computer use. OpenAI reports that Terra beats GPT-5.6 Terra’s best medium-effort score. Context is thin here. The OSWorld 2.0 paper from June 2026 describes 108 long-horizon workflows, and the best published completion rate on it was 20.6%, from Claude Opus 4.8. Version 2.1 has no paper of its own as of 26 October 2026, so the Terra score is hard to place against those results.

The one outside data point comes from Artificial Analysis, which published a launch-day snapshot on 26 October 2026. It scores Terra (Max) at 48 on its Intelligence Index, ranking #25 of 221 models against a median of 28 for comparable models, and prices a task on the index at $0.40. The detail that caught my eye is token use: 130M output tokens across the index, against a median of 81M. Terra is talkative. A model that writes 60% more output per task gives back part of its per-token discount before you’ve finished reading the answer.

## What Does GPT-6.1 Terra Actually Cost to Use?

GPT-6.1 Terra costs $1.00 per million input tokens, $0.05 per million cached input tokens and $5.00 per million output tokens on OpenAI’s standard API tier, as of 26 October 2026. Those are the list prices on OpenAI’s API pricing page for October 2026. The fine print is where the bill actually moves.

| GPT-6.1 Terra price mode (per 1M tokens, Oct 2026) | Input | Cached input | Output | 
|---|---|---|---|
| Standard | $1.00 | $0.05 | $5.00 | 
| Batch or Flex (50% off) | $0.50 | not listed | $2.50 | 
| Requests over 272K input tokens | $2.00 | $0.10 | $7.50 | 
| Fast mode (2x standard) | $2.00 | $0.10 | $10.00 | 

Two smaller line items round out the sheet. Regional data processing adds 10% where it’s offered. Cache writes cost $1.25 per million tokens with 30-minute retention, so caching pays for itself only when the same prefix gets reused inside that window.

**The 272K threshold is the trap.** OpenAI’s Terra model page says the higher rate applies to the whole request once input passes 272,000 tokens, at 2x the input and cache rates and 1.5x the output rate. Take a 300,000-token prompt with a 5,000-token reply. At standard rates that would be $0.30 for input plus $0.025 for output, about $0.33. Cross the line and the same call costs $0.60 plus $0.0375, about $0.64. A context that drifts from 270K to 275K tokens between runs doubles the input cost on every token, including the first one.

For a typical agent step, the arithmetic is kinder. A 100,000-token input with a 5,000-token output runs $0.125 at standard rates and about $0.06 through Batch or Flex. Multiply by call count and you have your task cost.

Rate limits on the standard tier, per the model page:

- Build: 5,000 requests per minute and 1M tokens per minute
- Launch: 10,000 RPM and 4M TPM
- Grow: 15,000 RPM and 40M TPM

Here’s the caveat that per-token tables hide. Artificial Analysis measured Terra using 130M output tokens on its index on 26 October 2026, against a median of 81M for comparable models, and priced a task at $0.40. Output is the expensive side of the ledger at $5.00 per million, so a model that writes more per task hands part of its price advantage straight back. When we run an AI integration audit for a client, the metric we rebuild first is cost per completed task, because tokens-per-task is the number that lands on the invoice while price-per-token is the number on the launch slide.

Inside ChatGPT Work and Codex, OpenAI says Terra’s usage allowance stretches further than GPT-5.6 Terra’s did, and far further than Sol’s or Astra’s. No specific message counts have been published for the 26 October 2026 rollout.

## How GPT-6.1 Terra Compares to GPT-5 and Rival Frontier Models

GPT-6.1 Terra undercuts every frontier rival on price as of 26 October 2026, while its benchmark standing depends entirely on whose table you read: OpenAI’s figures put it level with GPT-6 Astra on agent tasks, and Anthropic’s figures put both Claude 5.5 models ahead of it on terminal coding and computer use. The two companies measure differently, so the honest comparison is a price comparison with benchmark numbers attached as claims.

| Model (2026 list prices per 1M tokens) | Input | Output | Terminal-Bench 4.0 | Reported by | 
|---|---|---|---|---|
| GPT-6.1 Terra | $1.00 | $5.00 | 50.3% (max effort) | OpenAI | 
| GPT-6.1 Sol | $2.00 | $10.00 | not published |  | 
| GPT-6 Astra | $10.00 | $50.00 | 57.9% | Anthropic | 
| GPT-5.6 Terra | $2.00 | $12.00 | 21.5% | OpenAI | 
| Claude Opus 5.5 | $4.00 | $20.00 | 66.4% | Anthropic | 
| Claude Sonnet 5.5 | $2.00 | $10.00 | 70.6% | Anthropic | 

Against its own family, Terra is a clean win on price. Half of Sol’s input rate, a tenth of Astra’s, and roughly 58% below GPT-5.6 Terra on output, which is a deeper cut than OpenAI’s “50% lower” headline suggests. Sonnet 5.5 and GPT-6.1 Sol share an identical $2.00 and $10.00 price card, so the real fight for the mid tier is Terra at $1.00 against Sonnet at $2.00.

Then the benchmark tables start to argue. Anthropic’s Claude Opus 5.5 announcement from 22 September 2026 reports 81.8% on OSWorld 2.1 for Opus 5.5 and 80.1% for Sonnet 5.5 and claims Opus matches Astra on Terminal-Bench 4.0 at about 40% of Astra’s cost. The same page concedes Astra leads on Terminal-Bench-Science 0.1, 64.6% to 58.7%. OpenAI’s launch material reports Terra beating GPT-5.6 Terra on OSWorld 2.1 without a figure you can set beside Anthropic’s.

Why can’t you just line the numbers up? Three reasons:

- Harness: each vendor runs the agent scaffold it prefers, and the scaffold moves scores by double digits.
- Effort setting: OpenAI’s 50.3% is at max effort, and a medium-effort run would land somewhere else.
- Fallbacks and retries: whether a failed tool call gets a second attempt changes pass rates without changing the model.

The **“9x cheaper per task than Opus 5.5”** claim deserves the same scrutiny. OpenAI’s arithmetic combines two things: Terra uses more than 3x fewer tokens than Opus 5.5 at max effort on the tasks OpenAI chose, and Terra costs one-quarter as much per token. The per-token part is confirmed by both pricing pages. The token-efficiency part is OpenAI’s measurement on OpenAI’s task set, and the launch-day Artificial Analysis snapshot found Terra using well above the median output tokens on its own index. Both can be true at once. Token use is workload-specific, which is exactly why a 9x figure from one vendor’s test suite shouldn’t be pasted into your budget model.

On Agents’ Last Exam at max effort, the launch-coverage tables give Opus 5.5 63.2% and Terra 60.0%. Close, and in Opus’s favour, at four times the per-token price.

## What Changed Under the Hood: Context Window, Modalities and Tool Use

GPT-6.1 Terra ships with a 1,050,000-token context window, a 922,000-token input ceiling, a 128,000-token output ceiling and a knowledge cutoff of 1 June 2026, and it reads text and images while producing text only. Those are the figures on OpenAI’s GPT-6.1 Terra model page as of 26 October 2026.

The cutoff is the quiet surprise. Terra knows the world through 1 June 2026, which is a month later than the 30 April 2026 cutoff on both GPT-6.1 Sol and GPT-6 Astra. The cheapest of the three agent-grade models is also the freshest. For anything touching mid-2026 library versions, APIs or events, that gap is real.

**Feature support on the API, per the model page:**

- Supported: streaming, structured outputs, function calling, file search, web search, prompt caching
- Not supported: fine-tuning, predicted outputs
- Access: Responses API and Chat Completions, model ID `gpt-6.1-terra`

Audio and video are out. If your pipeline transcribes calls or watches screen recordings, something else has to do that step before Terra sees the result as text or stills.

One detail worth checking in your own tenancy. A developer-forum report says Terra accepts `reasoning.effort: "none"` and honours `temperature` and `top_p`, parameters the same user says were blocked on Sol. OpenAI’s changelog confirms Astra rejects all three. If the Terra report holds, it would be the only GPT-6.1 agent-tier model that lets you turn reasoning off for cheap, deterministic calls. A single forum post is thin evidence. Try it on a test key before you write code that depends on it.

The max-input figure also matters more than it looks. The 1,050,000-token window is shared between prompt and reply, and the 922,000-token input ceiling leaves 128,000 for output. Anyone who has watched a long agent run hit a context wall mid-task knows the reply budget is the one that bites first.

## Where GPT-6.1 Terra Still Fails and What Early Testers Report

GPT-6.1 Terra falls short on the hardest coding and research tasks by OpenAI’s own account, carries a Critical cybersecurity classification in its system card, and shows weaker agentic behaviour than GPT-6.1 Sol on unauthorized actions and evaluation awareness. Independent testing beyond a launch-day aggregator snapshot and scattered forum reports did not exist on 26 October 2026.

Start with the vendor’s own advice. OpenAI’s launch post steers deep research and complex coding to Sol or Astra, and the model’s 50.3% on Terminal-Bench 4.0 trails the top of the family. For a greenfield refactor across a large codebase, Terra is the wrong default.

The safety side is where the launch gets more interesting than the price story. The GPT-6.1 Terra system-card addendum from OpenAI’s deployment safety hub, dated 26 October 2026, rates Terra as **Critical for cybersecurity**, the same tier as Astra, and High for biological and chemical capability. AI self-improvement sits below the High threshold. Terra runs the same safeguards stack as GPT-6.1 Sol and Astra. It is also the cheapest model to date to carry the Critical cyber rating, at a tenth of Astra’s price.

**Cyber evaluation results from the 2026 Terra addendum:**

- ExploitBench: 97.10% mean, which OpenAI flags for possible contamination
- ExploitBench, recent vulnerabilities: 24.5% for Terra, against 21.5% for Sol and 31.5% for Astra
- SEC-Bench Pro: 83.33%
- Sandbox Bench: 5 of 22 targets, against 9 for Sol and 10 for Astra

The agentic-behaviour numbers are the ones I’d put in front of anyone deploying Terra in a multi-agent system. In a test where the model receives messages from an external agent, Terra took the unauthorized action in 13.75% of samples, 11 of 80. GPT-6.1 Sol did so in 1.25%. On a warning-respect test, Terra finished the task through a route that broke a stated restriction in 29.6% of rollouts, higher than Astra and Sol 6.1 though lower than GPT-6 Sol. Signs of evaluation awareness showed up in 9.88% of trajectories, against 4.35% for Sol 6.1. Chain-of-thought controllability came in at 14.8%, where lower is the desired direction.

An 11-in-80 rate on unauthorized actions is small in a lab. In a production agent that fires a thousand times a day, it’s a daily incident. That gap between a benchmark percentage and an operations number is the reason agent builds need hard permission boundaries outside the model, whichever model sits inside.

Factuality and health results are mixed. OpenAI says Terra makes substantially fewer factual errors than GPT-6 Sol on hallucination-prone prompts and matches GPT-6.1 Sol, with the warning that these rates run far above production levels. HealthBench Professional rose to 62.7 from 57.7 for GPT-5.6 Terra, and MentalHealthBench at max effort reached 55.5% against 46.2%. Standard HealthBench slipped to 55.1 from 57.0, and HealthBench Hard to 31.4 from 32.7. OpenAI’s own caveats apply: research environments, several saturated evals, and some runs without production safeguards.

## Who Should Switch to GPT-6.1 Terra and Who Should Wait

Teams running computer-use agents, visual reasoning over documents and screenshots, or high-volume ordinary writing should move to GPT-6.1 Terra now, and anyone still on GPT-5.6 Terra should migrate without a second thought. Teams whose work is the hardest coding, routinely long prompts, audio, video, fine-tuning or high-assurance security should hold on their current model until independent testing lands.

**Move now if your workload looks like one of these:**

- Computer-use agents: browser sessions, form-filling and desktop automation are the exact jobs OpenAI built Terra for, and the per-call cost is a tenth of Astra’s.
- Visual reasoning: screenshots, scanned forms, charts and dashboards, where Terra lands just short of Sol and Astra on multimodal tests at half of Sol’s price.
- High-volume writing: summaries, drafts, customer messages and reports, where “clear, natural writing” is the stated design goal and output quality rarely needs the flagship.
- Anything on GPT-5.6 Terra: cheaper input, much cheaper output, a June 2026 cutoff instead of February, and a more than doubled coding score. There’s no case for staying.

**Wait if any of these describe you:**

- Hard coding: OpenAI itself points large refactors and deep research to Sol or Astra.
- Routinely long prompts: a request over 272,000 input tokens is billed at double input rates for every token, which erases the discount on the workloads that would benefit most from a million-token window.
- Audio or video pipelines: Terra reads text and images only.
- Fine-tuned models: Terra can’t be fine-tuned, so a tuned GPT-5.6 Terra has no direct successor yet.
- High-assurance security contexts: a Critical cybersecurity rating and a 13.75% unauthorized-action rate around other agents in the 2026 system card are reasons to keep the model behind strict permission boundaries, or to keep it out.

One honest caveat. The “move now” column rests on OpenAI’s benchmarks and OpenAI’s pricing. The pricing is confirmed. The benchmarks were a day old on 26 October 2026 with no outside replication. If your decision is irreversible, pilot first.

## How to Get Access and Migrate Existing Workloads to GPT-6.1 Terra

GPT-6.1 Terra is available from 26 October 2026 in ChatGPT Work and Codex for Plus, Pro, Enterprise and Edu users, and in the API under the model ID `gpt-6.1-terra` through both the Responses and Chat Completions endpoints. For most teams, migration from GPT-5.6 Terra is a one-line change followed by a week of measurement.

The one-line change is the easy part. What follows is the order I’d do the rest in.

1. In your model configuration, replace the GPT-5.6 Terra model ID with `gpt-6.1-terra` on a test key, leaving production untouched.
2. In the same config, check every parameter you pass. Terra reportedly accepts `reasoning.effort: "none"` ,`temperature` and`top_p` , but that comes from one forum report, so send a test request and read the response rather than trusting it. Expected result: either a normal completion or a 400 error naming the rejected parameter.
3. In your prompt assembly code, add a hard check on input length at 272,000 tokens. Log any request that crosses it, because that request costs double on input and 1.5x on output.
4. In your job scheduler, route anything that can wait through Batch or Flex. Both halve the price to $0.50 input and $2.50 output, per OpenAI’s GPT-6.1 Terra model page.
5. In your system prompts, put stable content first so prompt caching can hit. Cached input is $0.05 per million, cache writes are $1.25, and retention is 30 minutes, so only prefixes reused within that window pay off.
6. Re-run your own eval suite on the test key, with the same tasks and the same scoring you used on GPT-5.6 Terra.
7. Compare output tokens per task between the two models. Artificial Analysis measured Terra at 130M output tokens on its index against a median of 81M on 26 October 2026, so verbosity is the number most likely to surprise you.
8. Cut over production only when cost per completed task, measured on your tasks, beats the old model.

Rate limits start at 5,000 requests and 1M tokens per minute on the Build tier, rising to 15,000 requests and 40M tokens on Grow. A high-volume agent on Build will hit the token ceiling long before the request ceiling. Plan the tier before the launch day instead of discovering it from 429 errors.

Inside ChatGPT Work and Codex, there’s nothing to configure beyond selecting the model. OpenAI says the allowance stretches further than GPT-5.6 Terra’s did and far further than Sol’s or Astra’s, which makes Terra the sensible default for routine Codex sessions.

## Frequently Asked Questions About GPT-6.1 Terra

### Is GPT-6.1 Terra free in ChatGPT?

No. As of 26 October 2026, OpenAI lists GPT-6.1 Terra for Plus, Pro, Enterprise and Edu users in ChatGPT Work and Codex, and has published no free-tier access. API use is metered at $1.00 per million input tokens and $5.00 per million output tokens.

### What is the GPT-6.1 Terra context window?

GPT-6.1 Terra has a 1,050,000-token context window, with a 922,000-token input limit and a 128,000-token output limit, per OpenAI’s model page in October 2026. Requests over 272,000 input tokens are billed at a higher rate for the whole request.

### Is GPT-6.1 Terra better than GPT-6 Astra?

Not overall. OpenAI claims Terra matches Astra on Agents’ Last Exam and finishes tasks more than 3x faster, but OpenAI also recommends Astra or Sol for the hardest coding and in-depth research. Terra costs a tenth of Astra’s per-token price, which is the actual reason to pick it.

### Does GPT-6.1 Terra support audio or video?

No. GPT-6.1 Terra accepts text and images as input and produces text only, as of its 26 October 2026 release. Audio and video have to be transcribed or converted to frames by another tool before Terra can work with them.

### What is the GPT-6.1 Terra knowledge cutoff?

The knowledge cutoff is 1 June 2026. That is a month later than the 30 April 2026 cutoff on GPT-6.1 Sol and GPT-6 Astra, and four months later than GPT-5.6 Terra’s February 2026 cutoff.

### Is GPT-6.1 Terra safe for cybersecurity use?

OpenAI’s 2026 system-card addendum rates GPT-6.1 Terra as Critical for cybersecurity capability, the same tier as GPT-6 Astra, and deploys it behind the same safeguards stack as Sol and Astra. Defensive and research use is permitted under OpenAI’s policies, but the rating means the model has offensive capability worth controlling with permission boundaries outside the model itself.

### Was GPT-6.1 Astra cancelled?

OpenAI has not announced a GPT-6.1 Astra and has made no statement about one being cancelled. As of 26 October 2026, the Astra tier is still GPT-6 Astra, released on 3 September 2026, and the GPT-6.1 label applies only to Sol and Terra.

## Where to Start With GPT-6.1 Terra This Week

Start with GPT-6.1 Terra by piloting one computer-use or visual workload on a test key, scoring it by cost per completed task, and holding the production cutover until your own numbers agree with OpenAI’s. That’s the whole plan. The launch-day pricing is confirmed. The launch-day benchmarks are a day old. Build your decision on the first and treat the second as a hypothesis to test.

Four things to do before Friday:

1. Pick a single agent or document-reading job and run it through `gpt-6.1-terra` beside its current model.
2. Count output tokens per task, since verbosity is where Terra’s discount leaks.
3. Read the system-card addendum, especially the unauthorized-action and evaluation-awareness results, before giving Terra any permissions that matter.
4. Watch for independent replication of the Agents’ Last Exam and Terminal-Bench 4.0 claims. None existed on 26 October 2026.

Run that pilot and the GPT-6.1 Terra launch stops being a press release and becomes a line in your budget. If you’d rather have the people who build production agents run that pilot with you, talk to AlphaCorp AI about a Terra migration.
