---
id: collect-261121-veille-ia/deepseek/deepseek-v4-1-flash-1017-the-20-cent-coding-task-and-what-mit-open-weights-do-to-your-ai-saas-margin
title: "DeepSeek V4.1-Flash-1017: The 20-Cent Coding Task and What MIT Open Weights Do to Your AI SaaS Margins (2026)"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "october", "task", "model", "snapshot", "flash", "index", "same"]
source: https://saascity.io/blog/deepseek-v4-1-flash-1017-open-weights-2026
source_anchor: ""
source_lines: [1, 145]
section: "DeepSeek"
---
**Quick answer:** DeepSeek released V4.1-Flash-1017 on Saturday, October 17, 2026, a snapshot update to the Flash model it shipped in September. DeepSeek's own chart puts a finished DeepSWE coding task at just under $0.20, down from about $0.50. Artificial Analysis has since scored it 45 on its Intelligence Index, up from 39 for the September snapshot, at $0.26 per index task against $0.49. The weights are MIT-licensed on Hugging Face. API pricing is unchanged: $0.15/M input and $0.60/M output off-peak, doubling to $0.30/$1.20 at peak. DeepSeek also opened its Search and Fetch APIs at $0.75 per 1,000 requests.

## What has happened since October 17

In two days the story got an outside check. Artificial Analysis scored the 1017 snapshot 45 on its Intelligence Index, up from 39, at roughly half the cost per task. DeepSeek raised the Search API rate limit from 5 to 100 queries per second, and 14 providers on OpenRouter now serve the open weights.

- **October 19:** Artificial Analysis lists DeepSeek V4.1 Flash 1017 (Max) at 45 on its Intelligence Index v4.3.2, against 39 for the September V4.1 Flash. Cost per index task fell from $0.49 to $0.26, and output tokens to run the index from 250M to 210M. On launch day The Decoder noted that no independent benchmarks existed yet; this is the first one we've found, and it points the same way as DeepSeek's chart.
- **October 19:** DeepSeek's Search & Fetch guide lists rate limits of 100 queries per second for Search and 25 for Fetch, per account. The launch version archived on October 17 said 5 for each. The changelog has no entry for the change.
- **October 19:** OpenRouter lists 14 providers for the snapshot, including DeepSeek, Together, Fireworks, DeepInfra, Cloudflare, Alibaba and Baseten. OpenRouter says the model is not yet available through its EU in-region routing.
- **October 19:** The Hugging Face repo shows 10,233 downloads, 1,494 likes and 64 community quantizations. Unsloth's GGUF repo is still marked work in progress, and its 4-bit folder alone holds about 500 GB.
- **October 19:** There is still no V4.1-Pro. DeepSeek's September 10 release note referred to a coming V4.1-Pro without a date, and the pricing page still lists`deepseek-v4-pro` on the V4-Pro-0813 snapshot at $0.66 input and $1.98 output per million tokens off-peak.
- **October 18:** SiliconANGLE reported that the release followed a nearly five-hour API outage on October 15, after which DeepSeek apologized and issued credits.

## What actually shipped on October 17

DeepSeek's own changelog calls it a minor update, and structurally it is: same Flash tier, same API endpoint, same 1M context window. But two things changed under the hood that matter more than the "minor" label suggests.

First, routing. Per the DeepSeek API changelog, the model name `deepseek-flash` now serves V4.1-Flash-1017 by default. The pricing page adds that the legacy names `deepseek-v4-flash` and `deepseek-v4-flash-vision-exp` are retired, and any request still pointed at them is served by the new snapshot at the same Flash price. If your app has been calling the old model string, it already upgraded itself sometime over the weekend. Worth checking your logs if you run anything latency-sensitive, since output length shifted too (more on that below).

Second, the actual model behavior. The Hugging Face model card describes "substantial improvements across reasoning, coding, agentic tasks, and visual benchmarks," paired with a line that's easy to skim past and shouldn't be: the update "reduces task execution time and output length." Translation for anyone paying a per-token bill: the model does the same job in fewer billed tokens. That's a line item on your invoice getting smaller every time a task runs.

This is the fourth Flash release since April, counting the original V4 Flash. We covered the July snapshot, when the only change was post-training and it was still enough to beat DeepSeek's own bigger Pro model. The pattern holding here is that DeepSeek treats Flash as a model that ships often, not a model that ships once.

## The benchmarks, and where they actually lose

DeepSeek put out comparison numbers against Claude Opus 5, GPT-5.6 Sol, Kimi K3 and GLM-5.3. Every number below is DeepSeek's own test run (Terminal-Bench 4.0 on the Claude Code harness, DeepSWE v1.1 on Mini-SWE-V2, ZeroBench at Pass@5), not an independent lab's. Worth saying plainly, and worth saying before you build a procurement decision on it.

| Benchmark | V4.1-Flash-1017 | Prev. V4.1-Flash | Opus 5 | GPT-5.6 Sol | 
|---|---|---|---|---|
| DeepSWE v1.1 | **77.0** | 74.2 | 74.0 | 73.0 | 
| Terminal-Bench 4.0 | 42.3 | 31.2 | **51.8** | 39.9 | 
| Agents' Last Exam | **35.4** | 31.8 | 28.6 | 26.7 | 
| Automation-Bench v1.0.6 | **60.5** | 54.8 | 50.3 | 45.8 | 
| Apex Bench v1.1 | **72.1** | 64.6 | 65.8 | 54.3 | 
| ProgramBench | 29.1 | 20.3 | **37.0** | 23.0 | 
| NL2Repo-Bench | 68.2 | 65.4 | **75.3** | 56.8 | 
| Humanity's Last Exam (no tools) | 42.6 | 36.8 | **56.3** | 44.5 | 
| Chartography (with tools) | 83.0 | 78.9 | **84.0** | 79.9 | 
| BabyVision (with tools) | 92.5 | 89.6 | **94.1** | 88.9 | 
| ZeroBench-main (with tools) | 51.0 | 49.0 | 52.0 | **53.0** | 

So no, it isn't a clean sweep, and DeepSeek doesn't pretend it is. The cheap model wins four of the eleven rows against Opus 5: DeepSWE, Automation-Bench, Agents' Last Exam and Apex Bench. The first three are agentic, task-completion-style tests. Opus 5 leads the other seven. It wins comfortably on Terminal-Bench 4.0, ProgramBench, NL2Repo-Bench and Humanity's Last Exam, which lean harder on raw reasoning depth and single-shot code generation than on iterative agent loops, and narrowly on the three vision tests. If your product needs a model that reasons through a gnarly refactor in one pass, this release doesn't change your vendor choice. If your product runs an agent loop that completes many small tasks and gets graded on whether the task finished, the math below is the part that should change your choice.

## Cost per task, not cost per token

Here's the number that actually matters for a SaaS margin, and it's not in the benchmark table.

The Decoder's report on DeepSeek's Pass@1-vs-cost chart puts V4.1-Flash-1017 at roughly 77% on DeepSWE for just under $0.20 per task at top reasoning effort, and above 72% for about $0.10 at the low setting. The previous Flash snapshot scored 74.2% at around $0.50 per run. Closed-model competitors on the same chart sit between $2 and more than $10 per task. That's DeepSeek's chart and DeepSeek's harness. But the direction matches the model card's claim about shorter outputs, it matches the pattern from the July release, where post-training alone cut the hallucination rate without the per-token rate moving at all, and Artificial Analysis has since measured a similar drop on its own index.

If you're running an AI feature at scale, cost per token was never the number you should have been tracking. Cost per finished task is. A model that costs more per token but finishes in a third of the turns can still be cheaper, and a model that's "free" per token but needs five retries to pass a task is the expensive one in disguise. This is the same framing we used looking at Claude Haiku 5.5's margin math for SaaS teams: the sticker price on the API docs page is the least useful number in the decision.

## DeepSeek V4.1 Flash vs Claude Haiku 5.5: which costs less per task?

On Artificial Analysis's numbers as of October 19, 2026, DeepSeek V4.1-Flash-1017 scores a little higher and Claude Haiku 5.5 costs a little less per task. DeepSeek scores 45 to Haiku's 43 on the Intelligence Index, and Haiku runs the index at $0.21 per task to DeepSeek's $0.26. Scheduling and prompt length can tip that either way.

|  | DeepSeek V4.1-Flash-1017 | Claude Haiku 5.5 | 
|---|---|---|
| Artificial Analysis Intelligence Index (max effort) | 45 | 43 | 
| Cost per index task (Artificial Analysis) | $0.26 | $0.21 | 
| Output tokens to run the index | 210M | 440M | 
| Output speed (Artificial Analysis) | 230.4 tokens/s | 232.3 tokens/s | 
| Input / output per M tokens | $0.15 / $0.60 off-peak, $0.30 / $1.20 peak | $0.10 / $0.50 up to 100K-token prompts, $0.50 / $2.50 above | 
| Cache read per M tokens | $0.003 off-peak, $0.006 peak | $0.01 up to 100K, $0.05 above | 
| Context / max output | 1M / 384K | 1M / 128K | 
| Weights | MIT, on Hugging Face | Closed | 

Sources: Artificial Analysis on DeepSeek, Artificial Analysis on Haiku 5.5, DeepSeek's pricing page and Anthropic's Haiku 5.5 docs.

Haiku 5.5 uses about twice the output tokens to finish the same index (440M against 210M) and still comes out cheaper, because its rates are lower. One detail matters here: Artificial Analysis lists DeepSeek at $0.30 input and $1.20 output, which is DeepSeek's peak rate. If that is the rate behind its cost figure, the same work run off-peak halves every DeepSeek rate, which by our arithmetic puts it near $0.13 per task. That's our estimate, not a measured number.

Prompt length is the other lever. Anthropic charges five times the base rate once a Haiku 5.5 prompt passes 100,000 tokens. DeepSeek charges one rate across its whole 1M context, so for prompts over 100,000 tokens it is cheaper per token even at peak ($0.30/$1.20 against $0.50/$2.50). For short calls during the DeepSeek peak windows, Haiku's lower list price wins.

## The actual price sheet

Here's what you'll pay calling the API directly, per DeepSeek's own Models & Pricing page for `deepseek-flash` (model version DeepSeek-V4.1-Flash-1017), as of October 19, 2026:

| Rate | Input, cache miss ($/M) | Input, cache hit ($/M) | Output ($/M) | 
|---|---|---|---|
| Off-peak (default) | $0.15 | $0.003 | $0.60 | 
| Peak | $0.30 | $0.006 | $1.20 | 

Peak hours are 01:00-04:00 UTC and 06:00-10:00 UTC, Monday through Friday, excluding Chinese public holidays. Every other hour is off-peak, including weekends and Chinese public holidays in full. Resellers follow the same clock: Vercel's AI Gateway model list encodes the same two windows with a 2x peak multiplier, and at 08:47 UTC on Monday, October 19, inside a peak window, OpenRouter's DeepSeek endpoint for the snapshot showed $0.30 input and $1.20 output. If your workload is a batch job, a nightly re-indexing run, or anything that doesn't need to happen the instant a user clicks a button, scheduling it outside those windows is a lever that costs you nothing but a cron change and saves half the bill. Nobody talks about this because "schedule your batch jobs for off-peak" sounds too simple to be a real optimization. It is a real optimization. It's just boring.

The cache-hit rate deserves the same attention. At $0.003 per million tokens off-peak (doubling to $0.006 at peak), a cache hit costs about 2% of the off-peak input rate. Any agent loop that replays a large shared system prompt or shared context window across turns (which describes most production agent architectures) should be structuring prompts specifically to maximize cache hits, not treating caching as a nice-to-have the SDK handles for you.

## MIT weights: negotiating power even if you never self-host

The part that gets underrated in most coverage of this release: the weights are MIT-licensed, full stop. Self-host, fine-tune, redistribute, strip the branding. There's no usage restriction in the license text.

Most founders reading this will never run a self-hosted inference stack. That's fine. The practical value of MIT weights is a hard price ceiling you can hold over any vendor you're negotiating with. If your current provider raises rates, or changes terms, or has an outage during your busiest week, "we could move to the open-weights model on another host this weekend" is a real sentence, not a bluff, because the weights, the license and the inference recipes already exist, and OpenRouter already lists 14 providers serving this exact snapshot. That changes every renewal conversation you have, even if you renew on the same terms. This is the exact dynamic we walked through with Xiaomi's MiMo-V2.6 release and what open weights do to model economics: the option value of a credible exit is worth something whether or not you ever exercise it.

## The second story: a $0.75 search API

Buried in the same release notes is a product launch that got a fraction of the attention: DeepSeek opened its own Search and Fetch APIs, its web search engine and its page-reader tool, to every developer. Per the changelog, the engine already handles hundreds of millions of search requests a day and indexes hundreds of billions of web pages.

Pricing is $0.75 per 1,000 successful requests. Empty or failed calls cost nothing. At launch each endpoint was capped at 5 queries per second per account; as of October 19, 2026, the Search & Fetch guide lists 100 for Search and 25 for Fetch. And if you're already calling DeepSeek through the server-side tools baked into DeepSeek Harness, Claude Code, or Codex, those calls are free until billing kicks in on October 31, 2026 at 16:00 UTC (00:00 Beijing time Nov 1), about two weeks of a genuinely free on-ramp if you're already in that ecosystem.

For anyone building an agent that needs live web results (research assistants, competitive-monitoring tools, anything that needs to ground an answer in something newer than a training cutoff), this is a vendor that now covers both the model and the retrieval layer, from one account, at a published price. For comparison, as of October 19, 2026, Brave's Search API page lists $5 per 1,000 requests with $5 of monthly credit, and Anthropic's pricing page lists Claude API web search at $10 per 1,000 searches plus token costs. We covered a third option, Cloudflare's Web Search API and its three providers, earlier this month. List price is one input. Test result quality on your own queries and read the data terms before you switch.

## How do you use DeepSeek V4.1 Flash in Claude Code?

DeepSeek documents a direct Claude Code setup: point Claude Code at DeepSeek's Anthropic-compatible endpoint with a DeepSeek API key, and map every model slot to `deepseek-flash`. Claude Code's web searches then run on DeepSeek's Search API, which stays free for those calls until 00:00 Beijing time on November 1, 2026.

These are the variables from DeepSeek's Claude Code guide for Linux and Mac (Windows uses the same names with PowerShell's `$env:` syntax):

```
export ANTHROPIC_BASE_URL=https://api.deepseek.com/anthropic
export ANTHROPIC_AUTH_TOKEN=<your DeepSeek API Key>
export ANTHROPIC_MODEL=deepseek-flash[1m]
export ANTHROPIC_DEFAULT_OPUS_MODEL=deepseek-flash[1m]
export ANTHROPIC_DEFAULT_SONNET_MODEL=deepseek-flash[1m]
export ANTHROPIC_DEFAULT_HAIKU_MODEL=deepseek-flash
export CLAUDE_CODE_SUBAGENT_MODEL=deepseek-flash
export CLAUDE_CODE_EFFORT_LEVEL=max
export CLAUDE_CODE_AUTO_COMPACT_WINDOW=786432
```
Two cost details from the same guide. Model names starting with `claude-opus` map to `deepseek-v4-pro` and bill at the V4 Pro price, which is why the Opus slot is set to `deepseek-flash` explicitly. And each web search Claude Code runs adds model tokens to summarize the results, plus Search API billing once the free window closes. If you're weighing this against a Claude subscription, our Claude Code pricing breakdown has the plan math.

## Hacker News noticed the model first

The model card hit the front page of Hacker News as the #1 story. As of October 19, 2026, the discussion thread had 943 points and 706 comments. That's a big number for a model release; most snapshot updates don't clear the front page at all, let alone take the top slot.

The Search API pricing got its own thread, posted on October 18, which had 571 points and 269 comments by October 19, here. Commenters compared it mostly with Brave, Exa, Serper and self-hosted SearXNG, which tells you developers noticed the retrieval play even if the headlines didn't lead with it.

## The caveats worth keeping in your back pocket

None of this is a reason to switch blindly. A few things to hold onto before you move production traffic:

- **Every number in the benchmark table is DeepSeek's own test run.** Terminal-Bench 4.0 specifically was run on the Claude Code harness, not an independent evaluation. Treat the within-DeepSeek comparisons (this snapshot vs. the last one) as solid, and the cross-vendor comparisons as directional. Artificial Analysis's index is the better independent cross-check.
- **Opus 5 still leads on seven of the eleven benchmarks DeepSeek published** , including the ones that matter most for single-shot reasoning. If your workload looks like that, this release doesn't move your decision.
- **Data residency and compliance are unresolved for regulated buyers.** DeepSeek's privacy policy says it collects, processes and stores personal data in the People's Republic of China, and its Open Platform terms are governed by mainland Chinese law. If you're shipping to EU enterprise customers or US customers with contractual data-location requirements, a model served from DeepSeek's infrastructure needs its own legal review before it touches production. A third-party host of the open weights is the usual workaround, but OpenRouter does not offer this snapshot through EU in-region routing yet.
- **Reliability is a live question.** DeepSeek's API was down for nearly five hours on October 15, two days before this release, per SiliconANGLE. Keep a fallback provider configured.
- **Self-hosting MIT weights is still real infrastructure work.** Unsloth's 4-bit GGUF folder for this snapshot is about 500 GB on its own, which puts it in multi-GPU server territory. The negotiating power is real even if you never deploy it, but don't confuse "the weights are free" with "running them is free."

## What this means for your margins this week

Run the numbers on what you're actually paying per completed task right now, not per token. If a chunk of your AI feature is agentic coding, tool-calling loops, or anything where the task either finishes or it doesn't, a sub-$0.20 cost per task at this quality level is worth a real evaluation against your current spend. Not a full migration on day one: a side-by-side test on your own workload this week.

And if you're building the kind of tool that gets switched between providers on a weekend because the economics moved (which, per this release, they just did again), getting found by the people making that switch matters as much as the switch itself. SaaSCity is a gamified startup directory with a live city map and actual human editorial review, built for exactly this kind of fast-moving AI tooling. A SaaSCity listing is free with our badge on your homepage and an open launch week, or $19.99 one-time to go live within 24 hours. Every listing is reviewed by hand, and a paid listing also gets a building on the city map. A Premium listing ($39.99) adds a launch review on our blog: 3000+ words, 5 links, published within 3 days.

DeepSeek just moved the price of a shipped coding task down again, for the second time in three months, with the weights sitting right there under MIT if anyone wants to check the math themselves. The question worth asking isn't whether this model is "the best." It's whether the vendor line item you haven't questioned since you set it up still deserves the price you're paying.

## Get your SaaS in front of founders

List your product on SaaSCity for $19.99, one-time: a permanent listing and a building on the live city map.
