---
id: collect-261121-veille-ia/claude-anthropic/claude-sonnet-5-5-pricing-0-10-cache-reads-benchmarks-api
title: "Claude Sonnet 5.5: Pricing, $0.10 Cache Reads, Benchmarks, API"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-26"]
keywords: ["claude", "sonnet", "opus", "anthropic", "model", "october", "haiku", "cache"]
source: https://codersera.com/blog/claude-sonnet-5-5-complete-guide-2026/
source_anchor: ""
source_lines: [1, 298]
section: "Claude_Anthropic"
---
**Quick answer.**Claude Sonnet 5.5 is Anthropic's mid-tier model, released September 28, 2026, with API ID

`claude-sonnet-5-5`, a 1M-token context window and 128K max output. As of October 2026 it costs $2 per million input tokens and $10 per million output tokens, and since October 7 its cache reads cost $0.10 per million, half of Opus 5.5's $0.20. That makes every Sonnet 5.5 rate exactly half of the Opus 5.5 rate, for a model that scores within about 2 points of Opus 5.5 on Artificial Analysis's Intelligence Index.
*Updated 2026-10-26: We fixed the cache-read price. Anthropic cut Sonnet 5.5 cache reads from $0.20 to $0.10 per million tokens on October 7, so they are no longer the same as Opus 5.5's. This update also recomputes the cached-agent cost examples, refreshes the Artificial Analysis cost figures, replaces Haiku 4.5 with Haiku 5.5 as the small-tier recommendation, and adds API version 2026-10-21 and the November 30 retirement of Sonnet 4.5.*

Anthropic released Claude Sonnet 5.5 on September 28, 2026, six days after Claude Opus 5.5 opened the Claude 5.5 family. The launch kept Claude Sonnet 5's $2 input and $10 output per million tokens, and the benchmarks jumped: Anthropic reports Terminal-Bench 4.0 rising from 10.3% to 70.6%, and on several agentic and office-work evals Sonnet 5.5 now lands close to Opus 5.5.

Three changes since launch affect what you pay and how you call the model. On September 30, Anthropic set Claude Sonnet 4.5's retirement for November 30, with Sonnet 5.5 as the named replacement. On October 7, it halved the cache-read price and shipped Claude Haiku 5.5. On October 21, it released API version `2026-10-21`, which removes `temperature`, `top_p` and `top_k` for every model.

This guide covers what Sonnet 5.5 costs now, with worked cache math, how it compares with Opus 5.5 and GPT-6.1 Sol on both vendor and independent benchmarks, a working Python quickstart, the breaking changes to plan for, and when to use Sonnet 5.5, Opus 5.5 or Haiku 5.5.

## What is Claude Sonnet 5.5?

Claude Sonnet 5.5 is the Sonnet-tier model in the Claude 5.5 family. Anthropic's model docs describe it as "the best combination of speed and intelligence" in the current lineup. It sits between Claude Opus 5.5 ($4/$20) and Claude Haiku 5.5 (from $0.10/$0.50), and Anthropic rates its latency "Fast", against "Moderate" for Opus 5.5 and "Fastest" for Haiku 5.5.

Anthropic's product page aims it at "well-scoped everyday tasks, fixing bugs, and creating polished documents, slides, and spreadsheets," and also calls out long-horizon work, image understanding and agentic coding. The system card says Sonnet 5.5 significantly outperforms Sonnet 5 across many domains and "in a few areas, it rivals or exceeds Claude Opus 5.5." It also says Sonnet 5.5 is "broadly less capable than Opus 5.5 across domains" and does not cross any new Responsible Scaling Policy thresholds.

### Key facts at a glance

- **Release date:** September 28, 2026
- **Family:** second Claude 5.5 model, after Opus 5.5 (September 22). Haiku 5.5 followed on October 7.
- **Knowledge cutoff:** June 2026 (both reliable-knowledge and training-data cutoff)
- **Thinking:** adaptive thinking, on by default, controlled by the`effort` parameter (default`high` on the API)
- **Cache reads:** $0.10 per million tokens since October 7, 2026 (previously $0.20)
- **Retirement:** not sooner than September 28, 2027

## Claude Sonnet 5.5 specs

All values below come from Anthropic's model pages and models overview, as of October 2026.

| Spec | Claude Sonnet 5.5 | Claude Opus 5.5 | Claude Haiku 5.5 | 
|---|---|---|---|
| Claude API ID | `claude-sonnet-5-5` | `claude-opus-5-5` | `claude-haiku-5-5` | 
| Amazon Bedrock ID | `anthropic.claude-sonnet-5-5` | `anthropic.claude-opus-5-5` | `anthropic.claude-haiku-5-5` | 
| Google Cloud (both variants), Microsoft Foundry, Claude Platform on AWS | `claude-sonnet-5-5` | `claude-opus-5-5` | `claude-haiku-5-5` | 
| Context window | 1M tokens | 1M tokens | 1M tokens | 
| Max output (sync) | 128K tokens | 128K tokens | 128K tokens | 
| Max output (Batch API, beta) | 300K tokens | 300K tokens | 300K tokens | 
| Input / output price per 1M | $2 / $10 | $4 / $20 | $0.10 / $0.50 (prompts up to 100K); $0.50 / $2.50 (over 100K) | 
| Thinking | Adaptive (lowest setting `between_tools` ) | Adaptive (always on) | Adaptive | 
| Default effort | `high` | `medium` | `medium` | 
| Comparative latency | Fast | Moderate | Fastest | 
| Input / output modalities | Text and images in, text out | Text and images in, text out | Text and images in, text out | 
| Knowledge cutoff | Jun 2026 | Jun 2026 | Jun 2026 | 

A few details matter in practice. The 300K batch output requires the `output-300k-2026-03-24` beta header. Sonnet 5.5's minimum cacheable prompt is 512 tokens, down from 1,024 on Sonnet 5, so caching now pays off on shorter system prompts. Sonnet 5.5 uses the same tokenizer as Sonnet 5, so the same text produces the same token count. Haiku 5.5 is different: Anthropic says the same text comes out to roughly 30% more tokens than on Haiku 4.5.

## How much does Claude Sonnet 5.5 cost?

Sonnet 5.5 matches Sonnet 5 on every price except cache reads. Anthropic's pricing page, as of October 2026:

| Price per 1M tokens | Sonnet 5.5 | Sonnet 5 | Opus 5.5 | 
|---|---|---|---|
| Base input | $2.00 | $2.00 | $4.00 | 
| Output | $10.00 | $10.00 | $20.00 | 
| 5-minute cache write | $2.50 | $2.50 | $5.00 | 
| 1-hour cache write | $4.00 | $4.00 | $8.00 | 
| Cache read (hit) | **$0.10** (was $0.20 before Oct 7) | $0.20 | $0.20 | 
| Batch input / output | $1 / $5 | $1 / $5 | $2 / $10 | 

The October 7 release notes describe the cut as $0.20 to $0.10 per million tokens, which is "0.05x the base input price instead of 0.1x", with cache writes and all other prices unchanged. Opus 5.5 also uses the 0.05x multiplier, but because its base input is $4, its cache reads stay at $0.20. In its Haiku 5.5 announcement the same day, Anthropic said that because cache reads make up a large share of token consumption, the cut "reduces the cost of Sonnet 5.5 on most agentic tasks by around 20%." Our two scenarios below land at 14% and 36%, depending on how much of each turn is cache hits.

An earlier version of this guide said cache reads cost the same on both models. That is no longer true. Every Sonnet 5.5 rate is now exactly half the matching Opus 5.5 rate: input, output, both cache-write tiers, cache reads and batch. Before the cut, a cache-heavy agent loop narrowed the gap between the two models, because the largest line item cost the same on both. Now the 2x gap holds on every line of the bill.

Other pricing rules still apply. The full 1M context is billed at the standard rate, so a 900K-token request costs the same per token as a 9K-token request. Setting `inference_geo: "us"` applies a 1.1x multiplier to all token categories, cache reads included. Regional and multi-region endpoints on Bedrock and Google Cloud (Google managed) cost 10% more than global endpoints. Batch, caching and data-residency modifiers stack.

### Worked example: what the cache cut saves on an agent loop

Take a coding agent where each turn re-reads a cached prefix, writes the newest context to a 5-minute cache, and produces a short output. We price two shapes at Claude API list prices, global routing, no batch.

**Scenario A: moderate context.** Each turn has 90,000 cache-read tokens, 10,000 cache-write tokens (5-minute) and 2,000 output tokens, over 50 turns per task.

| Line item (per turn) | Sonnet 5.5 now | Sonnet 5.5 before Oct 7 | Opus 5.5 | 
|---|---|---|---|
| Cache reads (90K) | 90,000 × $0.10 / 1M = $0.009 | 90,000 × $0.20 / 1M = $0.018 | 90,000 × $0.20 / 1M = $0.018 | 
| 5m cache writes (10K) | 10,000 × $2.50 / 1M = $0.025 | $0.025 | 10,000 × $5 / 1M = $0.050 | 
| Output (2K) | 2,000 × $10 / 1M = $0.020 | $0.020 | 2,000 × $20 / 1M = $0.040 | 
| **Per turn** | **$0.054** | $0.063 | $0.108 | 
| **Per 50-turn task** | **$2.70** | $3.15 | $5.40 | 

The cut saves $0.45 per task in this scenario, or 14% ($0.009 ÷ $0.063 per turn). Before the cut, Sonnet 5.5 cost 58% of Opus 5.5 here ($3.15 ÷ $5.40). Now it costs exactly 50%.

**Scenario B: long, cache-dominated context.** Each turn has 295,000 cache-read tokens, 5,000 cache-write tokens and 1,000 output tokens.

- **Sonnet 5.5 now:** 295,000 × $0.10 / 1M = $0.0295, plus 5,000 × $2.50 / 1M = $0.0125, plus 1,000 × $10 / 1M = $0.010, for**$0.052 per turn** .
- **Sonnet 5.5 before Oct 7:** $0.059 + $0.0125 + $0.010 =**$0.0815 per turn** .
- **Opus 5.5:** $0.059 + $0.025 + $0.020 =**$0.104 per turn** .

Here the cut saves 36% per turn ($0.0295 ÷ $0.0815), and Sonnet 5.5 moves from 78% of Opus 5.5's cost to 50%. The more of your input is cache hits, the more the cut saves. Long-running agents with large, stable prefixes gain the most. Remember that thinking tokens bill as output, so effort settings can outweigh any cache savings. The next section covers that.

### Is Sonnet 5.5 actually cheaper per task than Opus 5.5?

Per token, yes, by exactly half. Per finished task, it depends on effort, because Sonnet 5.5 can produce a lot of tokens.

- **Anthropic** says Sonnet 5.5 "costs up to 30% less per task than its predecessor" because "it typically needs far fewer tokens to do the same work," and that it generates outputs "30%+ faster than Sonnet 5." Separately, Anthropic says that in its head-to-head coding runs Sonnet 5.5 batched tool calls together more than Sonnet 5, leading to fewer steps. These are vendor claims made against Sonnet 5, not against Opus.
- **Artificial Analysis (AA, independent)** measured about 193K output tokens per Intelligence Index task for Sonnet 5.5 at max effort. Its model pages list 420M output tokens across the index for Sonnet 5.5 (Max) against 260M for Opus 5.5 (Max), so Sonnet produces about 60% more output.
- As of October 26, 2026, AA's model pages list **$5.46 per Index task for Sonnet 5.5 (Max)** against**$5.98 for Opus 5.5 (Max)** . At launch, AA's analysis put Sonnet 5.5 (max) at about $7.60 per task, roughly 27% above Opus 5.5's $5.98. AA does not say whether the drop comes from the new cache price, a re-run (its launch article said some evaluations would be re-run after a pre-release structured-outputs bug), or both.

So at `max` effort, Sonnet 5.5 is now about 9% cheaper per index task than Opus 5.5 ($5.46 ÷ $5.98 ≈ 0.91) and scores about 2 points lower. The price gap that is 50% per token shrinks to about 9% per task at max effort because of verbosity. Lower effort widens it again: as of October 26, AA's leaderboard lists Sonnet 5.5 at `high` at $0.88 per task for 46.75 points, against Opus 5.5 at `high` at $1.82 for 53.58, so Sonnet is about 52% cheaper per task there ($0.88 ÷ $1.82 ≈ 0.48). Either way, set effort deliberately. Anthropic's guidance is to start at `medium` for well-specified agentic coding and at `medium` or `low` for chat.

## Claude Sonnet 5.5 benchmarks

The first table is Anthropic's launch table, from the Sonnet 5.5 product page and system card (Table 8.1.A). These numbers are **vendor-reported**. The system card says the Claude results use adaptive thinking at max effort, averaged over 5 trials, and that competitor figures come from those developers' own system cards or leaderboards.

| Benchmark (vendor-reported by Anthropic) | Sonnet 5.5 | Sonnet 5 | Opus 5.5 | GPT-6 Sol | 
|---|---|---|---|---|
| Terminal-Bench 4.0 | **70.6%** | 10.3% | 66.4%* | n/a | 
| SWE-Bench Pro (system card) | 81.3% | 63.2% | **89.9%** | n/a | 
| SWE-Bench Multilingual (system card) | 90.3% | 78.3% | **93.9%** | n/a | 
| SWE-Bench Multimodal (system card) | 54.3% | 28.1% | **61.4%** | n/a | 
| FrontierCode 1.1 (Main) | 46.2% | 42.4% | **54.4%** | 49.3% | 
| CursorBench 4.0 | 55.5% | 34.1% | **57.8%** | n/a | 
| OSWorld 2.1 | 80.1% | 57.0% | **81.8%** | n/a | 
| Humanity's Last Exam (with tools) | 64.5% | 54.9% | **67.7%** | n/a | 
| Humanity's Last Exam (no tools, system card) | 56.9% | 43.1% | **64.4%** | n/a | 
| GDPval-AA v2.1 (Elo) | 1844 | 1449 | **1846** | 1487 | 
| AA-Briefcase v1.1 (Elo) | 1811 | 1359 | **1822** | 1483 | 
| Chartography (no tools) | 61.6% | 15.6% | **64.4%** | 53.6% | 
| HealthBench Professional (system card) | **69.2%** | 57.8% | 65.6% | n/a | 
| AutomationBench (system card) | **44.7%** | 10.7% | 42.5% | 32.0% | 

**Anthropic footnotes the Opus 5.5 Terminal-Bench score as its best result, at xhigh effort. Anthropic also notes that Sonnet 5.5's FrontierCode score is at max effort and is lower than its xhigh score (52.1%). "n/a" means the vendor did not publish a comparable number.*

The 81.3% SWE-Bench Pro score is not on the product page. It appears in Anthropic's system card, both in the capability summary table and in section 8.2, so it is a primary-source number that Anthropic simply didn't headline.

### What independent testing says (Artificial Analysis)

| Metric (independent, Artificial Analysis) | Sonnet 5.5 | Opus 5.5 | 
|---|---|---|
| Intelligence Index v4.3.2 (max effort) | **56.00** , #2 on the board | **57.62** , #1 on the board | 
| Terminal-Bench 4.0 (AA's own run) | 64% | 60% | 
| Terminal-Bench-Science | 53% | n/a | 
| AutomationBench-AA (AA launch analysis) | 71% | 70% | 
| AA-Omniscience accuracy | 54% | 66% | 
| Hallucination rate (lower is better) | 47% | 59% | 
| Output tokens per Index task (max effort) | ~193K | ~119K | 
| Cost per Index task (max effort, as of Oct 26) | $5.46 (about $7.60 in AA's launch analysis) | $5.98 | 
| Cost per Index task (high effort, as of Oct 26) | $0.88 for 46.75 points | $1.82 for 53.58 points | 
| Output speed (max effort; fluctuates daily) | ~140 tok/s | ~98 tok/s | 

The index scores are Intelligence Index **v4.3.2** figures. AA's model pages display them rounded, as 56 and 58. Don't compare them with scores from earlier index versions in older coverage, because AA changes the benchmark basket between versions.

AA's results largely support Anthropic's. Sonnet 5.5 is about 18 Index points ahead of Sonnet 5 (56.00 vs 38, both at max effort) and 1.6 points behind Opus 5.5. At max effort, AA lists Sonnet 5 at $5.09 per task, so Sonnet 5.5's 18 extra points cost about 7% more per task at that setting ($5.46 ÷ $5.09 ≈ 1.07). Anthropic's "up to 30% less" figure comes from its own testing, not from AA's max-effort index runs. AA's own Terminal-Bench run scores it at 64%, below Anthropic's 70.6%, but still ahead of Opus 5.5 on that test. The trade-off is factual recall. Sonnet 5.5 answers fewer knowledge questions correctly than Opus 5.5 (54% vs 66%), but it hallucinates less often when it doesn't know (47% vs 59%). AA's full effort ladder for Sonnet 5.5, as of October 26, is 56.00 at max ($5.46 per task), 51.90 at xhigh ($2.01), 46.75 at high ($0.88), 40.84 at medium ($0.48) and 35.87 at low ($0.35).

## Claude Sonnet 5.5 vs Opus 5.5: which should you use?

On agentic and office-work evals, Sonnet 5.5 is within about 2 points of Opus 5.5 on OSWorld, GDPval-AA and CursorBench, and ahead on Terminal-Bench, at half the price on every billing line. On harder software engineering (SWE-Bench Pro 81.3% vs 89.9%; FrontierCode 46.2% at max or 52.1% at xhigh, vs 54.4%) and on raw knowledge (HLE without tools 56.9% vs 64.4%), Opus 5.5 is clearly ahead. On AA's current figures, Sonnet 5.5 is also cheaper per task than Opus 5.5 even at max effort (see the cost section above), so the case for running Opus has to rest on quality, not cost. Our Claude Sonnet 5.5 vs Opus 5.5 comparison goes deeper, and our Opus 5 vs Sonnet 5 comparison covers the previous generation.

### And vs GPT-6.1 Sol?

Anthropic's launch table compares against GPT-6 Sol on four evals. Sonnet 5.5 leads on GDPval-AA (1844 vs 1487), AA-Briefcase (1811 vs 1483) and Chartography (61.6% vs 53.6%), and trails on FrontierCode (46.2% vs 49.3%). These are a vendor's numbers about a competitor, and Anthropic footnotes that an OpenAI bug fix may not be reflected in some GPT-6 Sol scores.

OpenAI released GPT-6.1 Sol on September 29, according to AA. On AA's Intelligence Index v4.3.2, GPT-6.1 Sol (Max) scores 52 against Sonnet 5.5 (Max)'s 56, at $0.72 per Index task against Sonnet's current $5.46. That is about 93% of the score for about 13% of the measured cost. AA lists GPT-6.1 Sol at the same $2/$10 list price, so the gap comes from token volume, not rates. Our Sonnet 5.5 vs GPT-6.1 Sol comparison breaks this down, and our GPT-6.1 Sol guide covers OpenAI's side.

## Where does Haiku 5.5 fit?

Haiku 5.5 launched October 7, 2026, and replaces Haiku 4.5 as the small model to pair with Sonnet 5.5. Anthropic positions it "for high-volume, latency-sensitive tasks such as classification, extraction, and routing," along with subagent work. As of October 2026 it costs $0.10 input and $0.50 output per million tokens for prompts up to 100,000 tokens, and $0.50/$2.50 for prompts over that, with cache reads at $0.01 (or $0.05 over the threshold). Unlike Sonnet 5.5, its price is set per request by prompt length, and cache reads and writes count toward the 100K threshold.

Two integration details matter when you pair the models. On the Claude API and on Google Cloud (Google managed), Sonnet 5.5 can read Haiku 5.5's thinking blocks, so a conversation that starts on Haiku 5.5 and escalates to Sonnet 5.5 keeps its reasoning. Haiku 5.5 cannot act as an advisor for a Sonnet 5.5 executor in the advisor tool. That pairing returns a 400. Our Claude Haiku 5.5 guide covers it in full.

## Where can you use Claude Sonnet 5.5?

- **Claude apps (claude.ai, desktop, mobile):** available since launch day. The product page says the apps default to`medium` effort.
- **Claude API:** all customers, model`claude-sonnet-5-5` .
- **Cloud platforms:** Amazon Bedrock (`anthropic.claude-sonnet-5-5` ), Claude Platform on AWS, Microsoft Foundry, and both Google Cloud variants (all`claude-sonnet-5-5` ). Claude Models on Google Cloud (Anthropic managed), which bills through Google Cloud Marketplace, launched October 22.
- **Claude Code:** the 2.1.284 changelog (September 28) made Sonnet 5.5 the default Sonnet model.
- **GitHub Copilot:** generally available on Copilot Pro, Pro+, Max, Business and Enterprise, according to GitHub's September 28 changelog.
- **Cursor:** Cursor announced support for Sonnet 5.5 in a post on X.

## How to use the Claude Sonnet 5.5 API (Python quickstart)

Install the SDK and set your key. Python SDK 2.0.0 is the first version that sends API version `2026-10-21` by default:

```
pip install "anthropic>=2.0.0"
export ANTHROPIC_API_KEY="sk-ant-..."
```
Here is a basic Messages API call with effort set explicitly. The `output_config.effort` field follows Anthropic's effort docs:

```
import anthropic
client = anthropic.Anthropic()  # reads ANTHROPIC_API_KEY
response = client.messages.create(
    model="claude-sonnet-5-5",
    max_tokens=16000,
    messages=[
        {"role": "user", "content": "Review this function for edge cases: def div(a, b): return a / b"}
    ],
    output_config={"effort": "medium"},  # low | medium | high (default) | xhigh | max
)
for block in response.content:
    if block.type == "text":
        print(block.text)
```
Thinking counts toward `max_tokens` even when the thinking text isn't returned, so leave headroom. For agentic coding, Anthropic recommends `max_tokens=128000` with streaming. Leave out `temperature`, `top_p` and `top_k` entirely (see below).

To turn off up-front thinking for fast, simple calls, use `between_tools`. Sonnet 5.5 rejects `disabled`, and `between_tools` works only at `low`, `medium` or `high` effort:

```
response = client.messages.create(
    model="claude-sonnet-5-5",
    max_tokens=4096,
    thinking={"type": "between_tools"},
    output_config={"effort": "low"},
    messages=[{"role": "user", "content": "Classify this ticket: 'Login page 500s on Safari'"}],
)
```
## API version 2026-10-21: what changes for Sonnet 5.5 users

Anthropic released API version `2026-10-21` on October 21. New major SDK versions default to it: Python 2.0.0, TypeScript 1.0.0, C# 13.0.0, Go 2.0.0, Java 3.0.0, PHP 1.0.0, Ruby 2.0.0, and the `ant` CLI 2.0.0. They are now licensed under Apache 2.0 instead of MIT. The changes that matter most:

- **Sampling parameters are gone.** The migration guide says that in`2026-10-21` "a request that sends`temperature` ,`top_p` , or`top_k` returns a 400 error, even at the parameter's default value." On the older`2023-06-01` version, Sonnet 5.5 already rejected non-default values. The new version rejects the parameters themselves, on every model. The latest SDK majors no longer include them at all.
- **Document blocks in assistant messages** return a 400. Send documents in user messages.
- **A non-empty `citations` array on text blocks in user messages** returns a 400. Citations on assistant text blocks, and the`"citations": {"enabled": true}` setting on documents and search results, are still accepted.
- **SDK-level changes** , separate from the API version. For example, Python's`MessageParam` becomes a union of per-role types (dict literals need no change), and API keys are sent as`Authorization: Bearer` . If a proxy or gateway reads`X-Api-Key` , check it before you upgrade.

If a workload depends on a non-default `temperature` on an older model such as Sonnet 4.6, Anthropic says you can keep that request on `2023-06-01`. For Sonnet 5.5, nothing about sampling behavior changes, because it already sampled only at defaults. See our API 2026-10-21 and SDK migration guide for the full checklist.

## What breaks when you migrate from Sonnet 5 to Sonnet 5.5?

Changing `claude-sonnet-5` to `claude-sonnet-5-5` is the easy part. Anthropic's "What's new" page lists five breaking changes:

1. **`thinking: {"type": "disabled"}` returns a 400.** Use`{"type": "between_tools"}` instead, at`low` ,`medium` or`high` effort.
2. **Forced tool use is gone.** A`tool_choice` of`any` or`tool` returns a 400. Use`auto` with`strict: true` tool definitions, or structured outputs.
3. **Thinking blocks are tied to the model and the conversation.** For accounts created on or after August 31, 2026, replaying a Sonnet 5.5 thinking block after editing the system prompt, tools or earlier messages returns a 400. Keep history append-only.
4. **Computer use needs the new toolset** (`computer_toolset_20260801` ) on the Claude API and Google Cloud (Google managed). Those platforms reject`computer_20251124` . Bedrock still accepts it.
5. **Advisor tool pairings:** Opus 4.8, Opus 4.7, Sonnet 5 and Haiku 5.5 can't serve as advisors for a Sonnet 5.5 executor.

One change is silent. Text longer than a sentence or two between tool calls now comes back in `thinking` blocks, and at the default `display: "omitted"` those blocks are empty. A UI that streams those notes will go quiet with no error. Anthropic also recalibrated effort levels, so re-run your effort sweep instead of carrying over the Sonnet 5 setting. The pattern is similar to our Opus 5.5 migration guide.

### Still on Sonnet 4.5? You have until November 30

Anthropic deprecated `claude-sonnet-4-5-20250929` on September 30, 2026. It retires on **November 30, 2026** on Anthropic-operated platforms (the Claude API, Claude Platform on AWS, Claude Models on Google Cloud (Anthropic managed) and Microsoft Foundry), and Anthropic names `claude-sonnet-5-5` as the replacement. After that date, requests to the old ID fail. Amazon Bedrock and Google Cloud (Google managed) set their own retirement schedules, so check those separately. Moving from Sonnet 4.5 is a bigger jump than moving from Sonnet 5. You pick up all five breaking changes above, the sampling-parameter rules, and adaptive thinking in place of manual `budget_tokens`, which Sonnet 5.5 rejects. To find remaining traffic, export your usage CSV from the Console's Usage page. Our Sonnet 4.5 retirement and migration guide walks through it.

## Who should use Claude Sonnet 5.5?

- **Most production agents and coding assistants.** It tracks Opus 5.5 closely on the agentic evals above, costs half as much on every rate and, on AA's current figures, is cheaper per task even at max effort. Start here and move up only when your evals say so.
- **Teams on Sonnet 5:** upgrade. Rates are the same except cache reads, which are now half price. Sonnet 5.5 improved on every benchmark Anthropic published, and AA independently scores it about 18 Index points higher. Budget an afternoon for the breaking changes, especially`tool_choice` and`disabled` thinking.
- **Teams on Sonnet 4.5:** migrate before November 30.
- **Long-context, cache-heavy agents:** these gain the most from the October 7 cut. In our Scenario B above, per-turn cost fell 36%.
- **Hard, multi-file software engineering:** test Opus 5.5 as well. The SWE-Bench Pro gap (81.3% vs 89.9%) is real. The FrontierCode gap is 8.2 points at Sonnet's max effort, but only 2.3 at its xhigh score.
- **Knowledge-heavy Q&A without retrieval:** Opus 5.5 recalls more facts (AA-Omniscience 66% vs 54%). With RAG this matters less, and Sonnet's lower hallucination rate helps.
- **High-volume, simple tasks:** use Haiku 5.5 at $0.10/$0.50 for prompts under 100K tokens, or Sonnet 5.5 at`low` effort with`between_tools` when you need more capability.
- **Cost-sensitive workloads:** avoid`max` effort unless you've measured a gain. At about 193K output tokens per index task, output and thinking tokens, not cache reads, dominate the bill.

## FAQ

### When was Claude Sonnet 5.5 released?

September 28, 2026, according to Anthropic's release notes, model docs and system card. It came six days after Claude Opus 5.5 (September 22, 2026).

### How much does Claude Sonnet 5.5 cost?

As of October 2026: $2 per million input tokens and $10 per million output tokens. Cache reads are $0.10, 5-minute cache writes $2.50 and 1-hour cache writes $4. The Batch API halves input and output to $1/$5.

### What is the Sonnet 5.5 cache read price?

$0.10 per million tokens since October 7, 2026, down from $0.20. That is 0.05x the $2 base input price. Opus 5.5 uses the same multiplier on a $4 base, so its cache reads stay at $0.20, twice Sonnet 5.5's.

### What is the Claude Sonnet 5.5 API model ID?

`claude-sonnet-5-5` on the Claude API, Google Cloud, Microsoft Foundry and Claude Platform on AWS. On Amazon Bedrock it is `anthropic.claude-sonnet-5-5`.

### What is Claude Sonnet 5.5's context window?

1M input tokens, with up to 128K output tokens on the synchronous Messages API and up to 300K on the Batch API with the `output-300k-2026-03-24` beta header.

### Is Claude Sonnet 5.5 better than Opus 5.5?

Not overall. Artificial Analysis scores it 56.00 on Intelligence Index v4.3.2 at max effort, behind Opus 5.5 at 57.62, and Opus leads on SWE-Bench Pro, FrontierCode and HLE. In Anthropic's numbers, Sonnet 5.5 beats Opus 5.5 on Terminal-Bench 4.0, AutomationBench and HealthBench Professional, and every rate is half of Opus 5.5's.

### Is Sonnet 5.5 cheaper per task than Opus 5.5?

On AA's current figures, yes, even at max effort: $5.46 per Index task against $5.98. The margin is much smaller than the 2x per-token gap because Sonnet 5.5 produces about 60% more output tokens. AA's launch analysis had Sonnet 5.5 (max) at about $7.60, more than Opus. At lower effort the gap is larger: at `high`, AA lists $0.88 against $1.82.

### What does Claude Sonnet 5.5 score on the Artificial Analysis Intelligence Index?

56.00 at max effort on Intelligence Index v4.3.2, second only to Claude Opus 5.5 at 57.62, as of October 26, 2026. The ladder runs 51.90 at xhigh, 46.75 at high, 40.84 at medium and 35.87 at low.

### Does API version 2026-10-21 affect Sonnet 5.5?

Yes, if your code still sends `temperature`, `top_p` or `top_k`. Under `2026-10-21`, sending any of them returns a 400, even at default values. Python SDK 2.0.0 and the other new SDK majors use the new version by default.

### When does Claude Sonnet 4.5 retire?

`claude-sonnet-4-5-20250929` retires on November 30, 2026 on the Claude API and the other Anthropic-operated platforms. Amazon Bedrock and Google Cloud (Google managed) set their own dates. Anthropic's recommended replacement is `claude-sonnet-5-5`.

### Can I turn off thinking on Sonnet 5.5?

Not with `disabled`, which returns a 400 error. The lowest setting is `thinking: {"type": "between_tools"}`, which turns off up-front thinking and works at low, medium or high effort.

### Is Sonnet 5.5 available in Claude Code, Copilot and Cursor?

Yes. Claude Code 2.1.284 made it the default Sonnet model, GitHub Copilot made it generally available on paid plans, and Cursor announced support.

## Sources

If your team is building agents or coding tools on Sonnet 5.5 and needs engineers who can tune effort, caching and migrations, Codersera can help you hire vetted remote AI developers.
