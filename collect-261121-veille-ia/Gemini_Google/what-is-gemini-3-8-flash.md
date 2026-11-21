---
id: collect-261121-veille-ia/gemini-google/what-is-gemini-3-8-flash
title: "What is Gemini 3.8 Flash?"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-09-02"]
keywords: ["flash", "gemini", "model", "tokens", "output", "google", "million", "input"]
source: https://apidog.com/blog/what-is-gemini-3-8-flash/
source_anchor: ""
source_lines: [1, 122]
section: "Gemini_Google"
---
Gemini 3.8 Flash is Google’s newest Flash-tier model, released on September 2, 2026, alongside a gated security twin called Gemini 3.8 Flash Cyber. It is the third Flash release in six weeks, following Gemini 3.6 Flash on July 21 and 3.7 Flash on August 13, at the same introductory price as 3.7 Flash: $0.75 per million input tokens and $3.75 per million output tokens through December 31, 2026. Google calls it “our most intelligent Flash model, engineered for long-horizon software engineering, autonomous agents, and complex enterprise workflows.”

The headline change is not a new price or a bigger context window. It is behavior. Google designed 3.8 Flash to “work harder” on difficult tasks: it takes smaller reasoning steps, verifies its work along the way, and calls tools iteratively. That buys measurable gains on agentic benchmarks, and it means the model spends more tokens per task, by design. Artificial Analysis measured about 30% more output tokens than 3.7 Flash. If you budget by the token, the price is flat. If you budget by the task, the bill went up.

This guide covers the whole launch: specs, the “works harder” trade-off, benchmarks, pricing, availability, the Cyber twin, and what the model cannot do. Every request here is plain HTTP with JSON, so you can build and inspect them in Apidog before they reach application code. Google’s launch post and the model page are the primary sources.

## Gemini 3.8 Flash at a glance

| Spec | Value | 
|---|---|
| API model ID | `gemini-3.8-flash` (stable, no preview suffix) | 
| Released | September 2, 2026 | 
| Based on | Gemini 3.7 Flash (per the DeepMind model card) | 
| Context window | 1,048,576 input tokens | 
| Max output | 65,536 tokens | 
| Input modalities | Text, image, video, audio, PDF | 
| Output modalities | Text only | 
| Thinking levels | `low` ,`medium` (default),`high` ;`minimal` returns an error | 
| Intro pricing (through Dec 31, 2026) | $0.75 input / $3.75 output per million tokens; thinking billed as output | 
| Standard pricing (from Jan 1, 2027) | $1.50 input / $7.50 output per million tokens | 
| Context caching | $0.075 per million cached tokens (intro), $0.15 standard | 
| Batch API | 50% off: $0.375 / $1.875 intro | 
| Knowledge cutoff | March 2026 | 
| Developer availability | Gemini API, Google AI Studio, Android Studio, Google Antigravity, Stitch, Gemini Enterprise | 
| Consumer availability | Gemini app (AI Pro and Ultra subscribers), AI Mode in Search, Gemini in Google Sheets | 
| 3.7 Flash status | Remains fully supported; no deprecation date | 

Three rows deserve a second look. The default thinking level is `medium`, not `high` as on Gemini 3 Pro, so a prompt tuned on Pro reasons less here unless you set the level. The `minimal` level returns a validation error rather than silently mapping to `low`; the thinking levels guide covers the fix. And the March 2026 cutoff carries a model-card caveat: in some domains, knowledge may be limited to January 2025.

## What Gemini 3.8 Flash is

Flash is Google’s workhorse tier: the model meant to run most production traffic while Pro handles the hardest problems. Google calls 3.8 Flash its “most intelligent workhorse model yet,” and the DeepMind model card describes it as based on 3.7 Flash rather than a new base model. So the honest framing is a refinement of a model released three weeks earlier, tuned for long-running software engineering and agent work.

The cadence is unusual. 3.6 Flash landed on July 21 at $1.50 / $7.50, 3.7 Flash on August 13 at the $0.75 / $3.75 intro rate, and 3.8 Flash on September 2 at the same rate. Google’s phrasing is “third Flash release in six weeks”; Artificial Analysis counts a fourth Flash model in under four months because their tally includes 3.5 Flash-Lite. For anyone maintaining a model config, the 3.7 Flash what’s-new post is three weeks old and already describes the previous generation. No Gemini 3.8 Pro, Gemini 4, or new Flash-Lite shipped with this release, and Google has not said when a Pro update is coming.

## The “works harder” design and what it costs you

Google’s description of the change is specific. On complex tasks, 3.8 Flash “executes extra reasoning steps, and calls tools iteratively.” It takes “smaller reasoning steps” and “verifies its work along the way.” Google is direct about the consequence: the model “can use more tokens on longer running and complex tasks, by design,” especially at higher effort levels.

Artificial Analysis measured that in their independent write-up. Running their Intelligence Index, 3.8 Flash at high reasoning averaged 48,000 output tokens per task, up 30% from 3.7 Flash. Per-token prices did not move, so the cost per task rose from $0.40 on 3.7 Flash high to $0.58 on 3.8 Flash high. Time per task rose too: 2.5 minutes versus 2.2 minutes.

The same report shows the lever you have. At `medium`, cost per task drops to $0.41, close to 3.7 Flash at high. At `low`, it drops to $0.24 with 0.8 minutes per task. So the thinking level is now a routing decision, not a global setting: latency-sensitive endpoints on `low`, agent loops that must finish the job on `medium` or `high`. The pricing breakdown works through 1,000 agent tasks per day at each level.

Speed is unchanged. Logan Kilpatrick of Google described 3.8 Flash as “same price as 3.7, is ~the same speed,” and Artificial Analysis measured 302.1 output tokens per second at high reasoning. Their 13.30-second time-to-first-token on that run is the model thinking before it answers, not a slow network path.

## What the benchmarks say

Google published three benchmark tables in text form on the DeepMind Flash page. These are Google-run numbers.

| Benchmark | 3.8 Flash | 3.7 Flash | Claude Opus 5 | GPT-5.6 Sol | GPT-5.6 Terra | Claude Sonnet 5 | 
|---|---|---|---|---|---|---|
| Vals Finance Agent v2 | 61.4% | 59.0% | 58.6% | 53.8% | 54.4% | 53.9% | 
| Harvey Legal Agent Benchmark | 10.0% | 8.8% | 6.7% | 2.5% | 0.8% | 5.0% | 
| HLE-Verified | 54.9% | 53.6% | 54.4% | 54.5% | 51.1% | 31.0% | 

The gains over 3.7 Flash are 2.4, 1.2, and 1.3 points. Modest, but the finance and legal rows put a Flash-priced model ahead of Opus 5 and GPT-5.6 Sol, which cost several times more per token. Claude Fable 5.1, which shipped the day before, does not appear in Google’s tables; the three-way comparison uses Anthropic’s Opus 5 and Sonnet 5 rows as the closest published entries.

Google makes three more claims without text-form numbers. On DeepSWE v1.1, 3.8 Flash “outperforms most larger frontier models” at “a fraction of the cost”; the percentage appears only in the model card’s image tables, so we are not printing one (3.7 Flash scored 65.3%). On document-heavy long-running workflows, an internal eval shows it “completes more than three times as many tasks as Gemini 3.7 Flash.” On Gray Swan prompt injection, Google reports a “significant leap” with no figure. SWE-Bench Pro, Terminal-bench, and OSWorld results are not published in text for 3.8 Flash.

The independent view lines up. Artificial Analysis scores 3.8 Flash (high) at 59 on their Intelligence Index, up from 56 for 3.7 Flash and 52 for 3.6 Flash. That ties GPT-5.6 Sol (xhigh) and Grok 4.6 (medium), and sits above GPT-5.6 Terra (max), Claude Fable 5.1 (medium), and Muse Spark 1.2 (xhigh), all at 57. On their τ³-Banking tool-use eval, 3.8 Flash scored 45%, 12 points above 3.7 Flash. The 3.8 vs 3.7 Flash comparison weighs those gains against the token cost by workload.

## Pricing: same per token, more per task

The pricing page lists 3.8 Flash on the same rows as 3.6 and 3.7 Flash, and all three double on January 1, 2027.

| Item | Through Dec 31, 2026 | From Jan 1, 2027 | 
|---|---|---|
| Input | $0.75 / 1M | $1.50 / 1M | 
| Output (includes thinking) | $3.75 / 1M | $7.50 / 1M | 
| Cached input | $0.075 / 1M | $0.15 / 1M | 
| Cache storage | $0.50 / 1M tokens / hour | $1.00 / 1M tokens / hour | 
| Batch input / output | $0.375 / $1.875 | $0.75 / $3.75 | 

Two details trip people. Thinking tokens are output tokens: billed at the output rate and reported separately in `usageMetadata.thoughtsTokenCount`, so a short answer at `high` can cost more than a long one at `low`. And Google Search grounding has its own meter: 5,000 free requests per month shared across all Gemini 3.x models, then $14 per 1,000 requests.

A rate-limited free tier exists in AI Studio and the API, with Google noting free-tier data is “used to improve our products.” Per-model rate limits are shown in AI Studio rather than in the docs. Tier 1 unlocks by linking a billing account, Tier 2 after $100 of spend and three days, Tier 3 after $1,000 and 30 days. The free-access guide covers what the free path does and does not get you, and the Batch API guide covers the 50% discount for jobs that can wait.

## How to call it

Google now treats the Interactions API as the primary path for Gemini 3.x. `generateContent` is “considered legacy” but “remains fully supported” with no sunset date, and most existing code still uses it. A minimal Interactions request:

```
curl -X POST "https://generativelanguage.googleapis.com/v1beta/interactions" \
  -H "x-goog-api-key: $GEMINI_API_KEY" \
  -H 'Content-Type: application/json' \
  -d '{"model":"gemini-3.8-flash","input":"Explain HTTP caching in 3 sentences.","generation_config":{"thinking_level":"medium"}}'
```
Multi-turn conversations pass `previous_interaction_id` and let the server keep state. Function calling declares tools with a JSON schema, receives a `function_call` step, and expects a `function_result` carrying both `call_id` and `name`, which are required on 3.8 Flash (legacy `generateContent` spells the field `id`). The API walkthrough shows both endpoints in REST and Python, and the function calling guide covers the iterative tool loops the “works harder” design produces and how to cap them.

If you are moving from 3.7 Flash, the changes are small but they error: `minimal` thinking is rejected, `thinking_budget` gives way to `thinking_level`, `candidate_count` is unsupported, and Google says to leave `temperature` at the default 1.0 because lowering it “may cause looping or degraded performance.” The migration guide turns Google’s what’s-new notes into a checklist with before-and-after JSON.

## What Gemini 3.8 Flash cannot do

The model page is explicit. Not supported: audio generation, the Live API, and image generation. Output is text only, even though input accepts text, image, video, audio, and PDF. Image segmentation is not supported on Gemini 3 models either. If your product needs real-time voice or image output, this model is the reasoning and tool-calling layer, not the whole stack. For cheap, high-throughput text without heavy reasoning, Gemini 3.5 Flash-Lite stays on the price list at $0.30 / $2.50. The rest of the checklist is supported, including function calling, structured outputs, context caching, code execution, Search and Maps grounding, Batch API, and Computer use in preview.

## Gemini 3.8 Flash Cyber: the gated twin

Gemini 3.8 Flash Cyber shipped the same day, and you cannot sign up for it. Access runs only through the new Fairwind Program, open to “trusted government authorities, critical infrastructure operators and software maintainers,” with background checks and requirements such as phishing-resistant MFA. There is no public API, no public pricing, and no self-hosting. It replaces the 3.5 Flash Cyber limited pilot.

Google’s claims are large: a real-world vulnerability discovery success rate above 70% across 20 programming languages, and a Chrome security team report of “2.6 times more correct patches to vulnerabilities in Chrome than the best commercial models that are much larger.” Both are Google-reported. The Cyber explainer covers eligibility, obligations, and what it means for the many teams who will never get access.

## Testing Gemini 3.8 Flash requests in Apidog

Because the cost story is per task rather than per token, the useful test is not “did the call succeed” but “how many tokens did it burn.” Apidog handles that as an ordinary API test. Store `GEMINI_API_KEY` as an environment variable, save the Interactions and `generateContent` requests as endpoints, and assert on status 200, the JSON fields your app reads, and a ceiling on `usageMetadata.thoughtsTokenCount`. Run the same prompt at `low`, `medium`, and `high` in one test scenario and you get the per-level token spread for your own prompts instead of Artificial Analysis’s averages.

Streaming responses render as SSE, which helps when debugging thought summaries with `includeThoughts: true`; the SSE testing guide walks through that. Schedule the scenario daily and a token regression fails an assertion before it lands on the invoice. Download Apidog to set it up on the free plan.

## FAQ

**Is Gemini 3.8 Flash a new model or an update to 3.7 Flash?** The DeepMind model card says it is based on Gemini 3.7 Flash. Treat it as a tuned successor: same price, speed, and context window, with more reasoning and tool-calling steps on hard tasks. 3.7 Flash remains fully supported, with no deprecation date.

**Why does Gemini 3.8 Flash use more tokens than 3.7 Flash?** By design. Google says it “can use more tokens on longer running and complex tasks,” and Artificial Analysis measured 30% more output tokens on their index. Lower `thinking_level` to `medium` or `low` on routes that do not need the extra reasoning; the thinking levels guide has the cost table per level.

**What is the Gemini 3.8 Flash context window?** 1,048,576 input tokens and 65,536 output tokens, with context caching at $0.075 per million cached tokens through December 31, 2026.

**Can I use Gemini 3.8 Flash in the free Gemini app?** Launch coverage lists the Gemini app for Google AI Pro and Ultra subscribers, not the free app tier. Developers get a rate-limited free tier in AI Studio and the API.

**How does Gemini 3.8 Flash compare to Claude Fable 5.1?** Artificial Analysis scores them at 59 and 57. On price, Claude Fable 5.1 costs $10 / $50 per million tokens, about 13x the intro rate of 3.8 Flash on both input and output. The gap narrows once you count tokens per task.

## Where to go next

Gemini 3.8 Flash is a workhorse that thinks longer, and the trade is explicit: a few points more on agentic benchmarks and 12 points on tool use, paid for with about 30% more output tokens at high reasoning. If your agents were hitting a wall on 3.7 Flash, the upgrade costs the same per token. If your traffic is latency-bound chat, set `low` or stay on 3.7 Flash, which is still supported. Start with the API walkthrough, send your first request from Apidog with a token-count assertion attached, and let the number, not the launch post, decide the thinking level for each route.
