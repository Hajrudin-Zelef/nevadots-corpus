---
id: collect-261121-veille-ia/gemini-google/gemini-3-8-flash-guide-3-7-flash-deprecated-2027-pricing
title: "Gemini 3.8 Flash Guide: 3.7 Flash Deprecated, 2027 Pricing"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-10-26"]
keywords: ["flash", "gemini-3", "gemini", "google", "model", "deprecated", "october", "guide"]
source: https://codersera.com/blog/gemini-3-8-flash-complete-guide-2026/
source_anchor: ""
source_lines: [1, 276]
section: "Gemini_Google"
---
**Quick answer.**Gemini 3.8 Flash (

`gemini-3.8-flash`) is Google's current Flash model and, since October 8, 2026, the only Flash you get if you call `gemini-3.7-flash`: Google deprecated 3.7 Flash and now routes its API requests to 3.8 Flash (and routes `gemini-3.5-flash` to `gemini-3.6-flash`). It costs $0.75 input / $3.75 output per 1M tokens through December 31, 2026, then $1.50 / $7.50 from January 1, 2027. Thinking tokens are billed at the output rate. Artificial Analysis scores it 41 on Intelligence Index v4.3.2.
*Updated 2026-10-26: An earlier version of this guide said Gemini 3.7 Flash was not deprecated. That is no longer true. Since October 8, 3.7 Flash and 3.5 Flash requests are rerouted to newer models, so we rewrote the migration section, added a re-test checklist, a Gemini 4 Argon comparison and the January 1 price change, and refreshed all Artificial Analysis figures.*

**Jump to:** What changed Oct 8 · Re-test checklist · Specs · Pricing · Benchmarks · Argon vs Flash · Code · FAQ

Google shipped Gemini 3.8 Flash on September 2, 2026, less than three weeks after Gemini 3.7 Flash (August 13). When this guide first ran, the main question was whether to upgrade. Google has since answered it for you. On October 8, the Gemini API changelog deprecated `gemini-3.7-flash` and began routing every API request for it to `gemini-3.8-flash`. The same entry deprecated `gemini-3.5-flash` and routes it to `gemini-3.6-flash`. Neither old model can be selected in Google AI Studio any more.

The endpoints still answer, which is exactly why this is easy to miss. Your code keeps sending the old model string, gets a 200 back, and is now billed for and served by a different model. If your team pinned a Flash ID for stability, that pin stopped doing its job on October 8.

This guide covers what the reroutes change in practice (cost, latency and behavior), a checklist for re-testing pinned workloads, current pricing and what happens on January 1, how Gemini 3.8 Flash compares with Gemini 4 Argon, and the specs, benchmarks and code you need to run it. Google's own claims are kept separate from independent measurements, and every Artificial Analysis figure carries its index version. Pricing and availability are as of October 2026.

## What changed on October 8, 2026?

Two Flash model IDs became aliases. Google's deprecations page lists both with a shutdown date of "N/A": the endpoints are not being turned off, they forward to a newer model.

| Model ID you send | Released | Status since Oct 8, 2026 | Model that actually serves it | AI Studio | 
|---|---|---|---|---|
| `gemini-3.7-flash` | Aug 13, 2026 | Deprecated, routed | `gemini-3.8-flash` | No longer selectable | 
| `gemini-3.5-flash` | May 19, 2026 | Deprecated, routed | `gemini-3.6-flash` (Google also names 3.8 Flash as a replacement) | No longer selectable | 
| `gemini-3.8-flash` | Sep 2, 2026 | Current, stable | Itself | Available | 
| `gemini-3.6-flash` | Jul 21, 2026 | Stable, previous generation; no shutdown announced | Itself | Available | 

An earlier access change landed on September 18. That day, Google limited the Gemini 2.5 models (2.5 Pro, 2.5 Flash, 2.5 Flash-Lite) to "users who have actively used them in the past," without defining a time window. They are not deprecated and have no shutdown date. Google says they stay available "until further notice," and the changelog tells new projects to use `gemini-3.5-flash-lite` or `gemini-3.8-flash`. If you were planning to start a new project on 2.5 Flash to save money, you may not be able to.

## What a rerouted model ID silently changes

A reroute is not a rename. The model behind the ID changed, and three things change with it.

### 1. Cost

**3.7 Flash → 3.8 Flash.** The list price is the same: 3.7 Flash also launched at $0.75/$3.75 with an introductory price through December 31, 2026. The per-token rate on your invoice does not move. The per-task cost can. Artificial Analysis reports that Gemini 3.8 Flash (high) generated 170M tokens while running its Intelligence Index, against 100M for Gemini 3.7 Flash (high) and a median of 81M for models in its price tier. Because thinking tokens bill at the output rate (see pricing), a model that reasons longer costs more per request at the same rate card. Artificial Analysis no longer reports cost per task for 3.7 Flash, so compare your own `total_thought_tokens` from before and after October 8 instead of relying on a published delta.

**3.5 Flash → 3.6 Flash.** This one moves the rate card itself. Google's pricing page no longer lists 3.5 Flash. Artificial Analysis still lists its price at $1.50 input / $9.00 output per 1M tokens. 3.6 Flash is $0.75 / $3.75 through December 31, then $1.50 / $7.50. Using a sample call of 30,000 input tokens and 6,800 output-plus-thinking tokens, and assuming the same token counts on both models (which your traffic won't match exactly):

On 3.5 Flash at the Artificial Analysis-listed price, that call costs 30,000 × $1.50/1M = $0.0450, plus 6,800 × $9.00/1M = $0.0612, for a total of **$0.1062**. Because 3.6 Flash has the same rate card as 3.8 Flash, the same call costs **$0.0480** through December 31 and **$0.0960** from January 1, 2027 (math in the worked example below). Artificial Analysis also measured 3.6 Flash (high) using fewer tokens than 3.5 Flash (high) on its index, 90M vs 120M.

So 3.5 Flash users are probably paying less right now. Don't build your 2027 budget on that number.

### 2. Latency

As measured by Artificial Analysis on October 26, 2026, on Google's API: Gemini 3.8 Flash (high) runs at **133.7 output tokens per second** with **28.78 seconds to first token**. Gemini 3.6 Flash (high) runs at 198.2 tokens per second with 18.09 seconds, and Gemini 3.5 Flash (high) at 194.0 tokens per second with 14.58 seconds. These live figures move from week to week. The time-to-first-token numbers include a full reasoning run at the high setting, so treat them as worst-case reference points, not your p50.

Direction is what matters. On the 3.5 → 3.6 path, output speed is about the same, but time to first token is roughly 3.5 seconds longer (18.09 − 14.58 = 3.51 s). Expect higher latency, not lower. On the 3.7 → 3.8 path, Artificial Analysis no longer publishes speed for 3.7 Flash, so there is no current like-for-like comparison. 3.8 Flash also generates more tokens per task (above), which adds to total response time. If your timeouts, streaming UX or SLOs were tuned on 3.7 Flash, they were tuned on a different model. Measure again.

### 3. Behavior

- **Thinking levels differ.** Google's thinking docs list`minimal` ,`low` ,`medium` and`high` for 3.6 Flash, but only`low` ,`medium` and`high` for 3.8 Flash. The 3.8 Flash model page says`minimal` is not supported and returns an error. Any code path that sends`minimal` to an ID routed to 3.8 Flash needs checking.
- **Grounding on 3.6 Flash.** According to the pricing page, Search grounding works on 3.6 Flash's paid tiers (5,000 free requests a month shared across Gemini 3 and newer models, then $14 per 1,000). It is not available on the free tier, where it can only be tested in AI Studio. If your 3.5 Flash workload ran grounding on a free-tier key, that won't carry over.
- **Output style and tool-calling drift.** Any new model changes answer length, formatting, refusal edges and how readily it calls tools. Prompts tuned for 3.7 Flash, and parsers written against its output, are the usual places this breaks.

## Re-test checklist for pinned Flash workloads

1. **Find every pinned ID.** Search code, config, environment variables, prompt registries and third-party tools for`gemini-3.7-flash` and`gemini-3.5-flash` . Swap them for the explicit target (`gemini-3.8-flash` or`gemini-3.6-flash` ) so your config shows which model is actually running.
2. **Check thinking settings.** Remove`minimal` from anything now served by 3.8 Flash. Decide on purpose between`low` ,`medium` (the default for both models) and`high` .
3. **Re-run your eval set.** Use the same golden prompts and the same scoring, and compare against results you saved before October 8. Pay attention to structured-output validity, tool-call accuracy and refusal rate.
4. **Measure tokens per request.** Log input, output and thought tokens on a representative traffic sample, then work out cost per completed task, not per token.
5. **Measure latency.** Check time to first token and total duration at p50 and p95 against your timeouts and retry budgets.
6. **Verify tools and grounding.** Check function calling, Search grounding, URL context and code execution. Confirm grounding requests on 3.6 Flash run on a paid-tier key, because the free tier doesn't include it.
7. **Re-price for 2027.** Model costs at both the current rate and the January 1 rate (table below), with and without Batch and caching.
8. **Set an alert.** Watch the Gemini API changelog and deprecations page. Google says shutdown dates are the "earliest possible dates" and that exact dates are communicated in advance.

## What is Gemini 3.8 Flash?

Gemini 3.8 Flash is the current stable model in Google's Flash line, the tier between the cheap Flash-Lite models and the flagship Gemini 4 Argon. Google's model page calls it the most intelligent Flash model, aimed at long-horizon software engineering and autonomous agents. Thinking is on by default at medium. The lowest supported level is low, and `minimal` returns an error.

| Property | Gemini 3.8 Flash | 
|---|---|
| Model ID | `gemini-3.8-flash` (stable) | 
| General availability | September 2, 2026 | 
| Input modalities | Text, image, video, audio, PDF | 
| Output modality | Text only | 
| Input token limit | 1,048,576 | 
| Output token limit | 65,536 | 
| Thinking | `low` ,`medium` ,`high` ; default`medium` ;`minimal` returns an error | 
| Function calling / structured outputs | Supported | 
| Context caching | Supported | 
| Batch API / Flex / Priority inference | All supported | 
| Search grounding, Maps grounding, code execution, URL context, file search | Supported | 
| Computer use | Supported (Preview) | 
| Live API | Not supported (use `gemini-3.8-live` ) | 
| Image / audio generation | Not supported | 

Two gaps to note. There is no Live API support on this model ID. For realtime voice, Google shipped the separate `gemini-3.8-live` model on September 15. And the output is text only. Speech output is handled by `gemini-3.8-flash-tts`, which became GA on September 22. Google also does not publish a knowledge cutoff on the model page, so use Search grounding or URL context when recent facts matter.

## Gemini 3.8 Flash pricing: now and from January 1, 2027

The current prices are introductory. Google's pricing page lists every tier doubling on January 1, 2027, and Gemini 3.6 Flash has exactly the same prices on both dates.

| Tier (per 1M tokens) | Through Dec 31, 2026 | From Jan 1, 2027 | 
|---|---|---|
| Standard input / output | $0.75 / $3.75 | $1.50 / $7.50 | 
| Batch API input / output | $0.375 / $1.875 | $0.75 / $3.75 | 
| Flex input / output | $0.375 / $1.875 | $0.75 / $3.75 | 
| Priority input / output | $1.35 / $6.75 | $2.70 / $13.50 | 
| Cached input read (standard) | $0.075 | $0.15 | 
| Cache storage (per 1M tokens per hour) | $0.50 | $1.00 | 

**Thinking tokens are billed at the output rate.** The pricing page says output prices include thinking tokens, and the thinking docs put it this way: "response pricing is the sum of output tokens and thinking tokens." You are billed for the full thought tokens the model generates, even though the API returns only summaries. Search grounding on 3.8 Flash gets 5,000 free requests a month, shared across Gemini 3+ models, then costs $14 per 1,000. 3.8 Flash is also on the free tier in AI Studio, with per-model rate limits shown inside AI Studio, not in the public docs.

### Worked example, with thinking tokens and the January price

Take an agentic call with 30,000 input tokens of context, 800 tokens of visible answer and 6,000 thinking tokens at the default medium level.

| Scenario | Through Dec 31, 2026 | From Jan 1, 2027 | 
|---|---|---|
| Input: 30,000 tokens | 30,000 × $0.75/1M = $0.0225 | 30,000 × $1.50/1M = $0.0450 | 
| Output + thinking: 6,800 tokens | 6,800 × $3.75/1M = $0.0255 | 6,800 × $7.50/1M = $0.0510 | 
| **Standard, per call** | **$0.0480** | **$0.0960** | 
| Standard, 1,000 calls/day × 30 days | $1,440 / month | $2,880 / month | 
| Batch API, per call (half price) | $0.0240 | $0.0480 | 
| 25,000 of the input tokens cached | 25,000 × $0.075/1M + 5,000 × $0.75/1M + $0.0255 = $0.0311 | 25,000 × $0.15/1M + 5,000 × $1.50/1M + $0.0510 = $0.0623 | 

A naive estimate that counts only the 800 visible output tokens comes to 30,000 × $0.75/1M + 800 × $3.75/1M = $0.0225 + $0.0030 = $0.0255 per call. The real bill is about 1.9× that. At 1,000 calls a day, the gap is $0.0225 × 1,000 = $22.50 a day, about $675 a month at today's rates and about $1,350 a month after January 1.

Three levers move the number:

- **Drop `thinking_level` to `low`** on tasks that don't need deliberation. It cuts the most expensive token class directly.
- **Use the Batch API** for anything not user-facing. It is half price on both input and output, before and after January 1.
- **Cache the stable prefix.** Caching saves $0.75 − $0.075 = $0.675 per 1M input tokens read. Storing a 25,000-token prefix costs 25,000 × $0.50/1M = $0.0125 per hour, and each read saves 25,000 × $0.675/1M ≈ $0.0169. One read an hour roughly covers the storage. Both figures double in January, so the break-even stays the same.

For a broader view of the price bracket, see our comparison of the cheapest fast LLM APIs and our explainer on how prompt caching reduces AI cost.

## How good is Gemini 3.8 Flash?

**Google's numbers** (vendor-reported, from the DeepMind Flash page):

| Benchmark (Google-reported) | Gemini 3.8 Flash | Gemini 3.7 Flash | Claude Opus 5 | GPT-5.6 Sol | GPT-5.6 Terra | Claude Sonnet 5 | 
|---|---|---|---|---|---|---|
| HLE-Verified | **54.9%** | 53.6% | 54.4% | 54.5% | 51.1% | 31.0% | 
| Vals Finance Agent v2 | **61.4%** | 59.0% | 58.6% | 53.8% | 54.4% | 53.9% | 
| Harvey Legal Agent Benchmark | **10.0%** | 8.8% | 6.7% | 2.5% | 0.8% | 5.0% | 

On DeepSWE v1.1, Google gives no exact score in text. Its chart shows 3.8 Flash at over 70% success at a low cost per task. Treat that as a floor Google is willing to show, not a measured figure you can quote. The page also quotes Glean's Thai Tran saying 3.8 Flash completed "more than three times as many tasks" as 3.7 Flash in Glean's evaluations. That is one company's document-heavy workload on an undisclosed harness.

**Independent numbers** (from Artificial Analysis, all on **Intelligence Index v4.3.2**, as measured on October 26, 2026; speed and cost figures are live and change often):

| Model (Artificial Analysis, v4.3.2) | Intelligence Index | Rank | Output speed | Time to first token | Cost per Index task | List price in / out | 
|---|---|---|---|---|---|---|
| Gemini 4 Argon (high) | 53 | #9 of 221 | 79.7 tok/s | 48.23 s | $1.62 | $2.00 / $10.00 | 
| GLM-5.3-Flash (open-weights model) | 42 | #6 of 121 | 74.7 tok/s | 2.83 s | $0.28 | $0.15 / $0.50 | 
| **Gemini 3.8 Flash (high)** | **41** | #56 of 221 | 133.7 tok/s | 28.78 s | $3.49 | $0.75 / $3.75 | 
| Gemini 3.7 Flash (high, deprecated) | 39 | #66 of 221 | n/a | n/a | n/a | $0.75 / $3.75 | 
| Gemini 3.6 Flash (high) | 34 | #83 of 221 | 198.2 tok/s | 18.09 s | $1.60 | $0.75 / $3.75 | 
| Gemini 3.5 Flash (high, deprecated) | 33 | #92 of 221 | 194.0 tok/s | 14.58 s | $2.98 | $1.50 / $9.00 | 

Four points from that table.

**Always check the index version.** Artificial Analysis rebased its Intelligence Index to v4.3.2. The first version of this page quoted 59 for 3.8 Flash under the earlier v4.1.1 index. The two scales are not comparable, and no conversion factor exists. A score without a version is unusable.

**Both reroutes are upgrades on measured intelligence.** 3.7 → 3.8 Flash is +2 points (39 → 41). 3.5 → 3.6 Flash is +1 point (33 → 34), and cost per Index task falls from $2.98 to $1.60. If you were on 3.5 Flash, the forced move is mostly good news until January.

**Cost per task is volatile, so treat it with care.** On October 26, Artificial Analysis listed 3.8 Flash at $3.49 per Index task, above Argon's $1.62, even though Flash's per-token prices are 37.5% of Argon's. These cost figures moved noticeably between our checks this month. Use them as a sign that 3.8 Flash is verbose (170M tokens on the index), not as a per-task price you can budget against. Measure cost per completed task on your own traffic.

**Open weights still win on value.** GLM-5.3-Flash scores one point higher at $0.28 per Index task, against $3.49 for Gemini 3.8 Flash, which is about 12× cheaper per task ($3.49 / $0.28 ≈ 12.5). It is also about 5× cheaper per input token and 7.5× cheaper per output token on list price. Gemini streams about 1.8× faster (133.7 vs 74.7 tok/s) and keeps the bigger input surface: a 1M-token context with native video and audio. For batch queues, price out GLM. For multimodal input or Google Cloud integration, Gemini is the safer default. Our GLM-5.3-Flash guide covers the alternative.

## Gemini 4 Argon vs Gemini 3.8 Flash: which one?

Gemini 4 Argon (`gemini-4-argon`) went GA on October 13 as Google's flagship reasoning model. It takes the same five input modalities as 3.8 Flash. Its 1,048,576-token context window is a unified limit shared by input and output. Output defaults to 65,536 tokens and can reach 1,048,576 with long decode, all within that same window. It costs $2 / $10 per 1M tokens through November 30, 2026, then $4 / $20 from December 1. It supports only implicit caching, and Google's pricing page lists no free tier for it.

**Decision box: Argon or 3.8 Flash?**

- **Choose Gemini 3.8 Flash** for high-volume extraction, classification, summarization, RAG answers, multimodal ingestion (audio, video, PDF) and most tool-calling agents. Its per-token prices are 37.5% of Argon's ($0.75/$3.75 vs $2/$10), and it streams faster (133.7 vs 79.7 tok/s on Artificial Analysis). It is also on the free tier and supports explicit context caching. One caveat: it is verbose, and Artificial Analysis currently lists a higher cost per Index task for it than for Argon. Check cost per completed task on your own workload.
- **Choose Gemini 4 Argon** for hard multi-step coding, math and science, long-horizon agents where a failed run costs more than the tokens, or jobs that need very long outputs. It scores 53 vs 41 on the same index.
- **Use both** with a router: 3.8 Flash by default, escalating to Argon when Flash fails validation or a task is flagged as hard.
- **Watch the dates.** Argon's price doubles on December 1, 2026. 3.8 Flash's doubles on January 1, 2027. Price your 2027 plans at $4/$20 vs $1.50/$7.50.

Using the same 30,000-in / 6,800-out call and assuming identical token counts (Argon will usually think more): Argon costs 30,000 × $2/1M + 6,800 × $10/1M = $0.060 + $0.068 = $0.128 through November 30, and $0.256 from December 1. 3.8 Flash costs $0.048 now and $0.096 in January (see the worked example). Google's Gemini 4 Argon model page lists its full capabilities and limits.

## What can Gemini 3.8 Flash take as input?

This is where the Flash tier stands apart from price-matched rivals.

| Modality | Limit | Token cost | 
|---|---|---|
| Text | 1,048,576-token context | Standard input rate | 
| Video (low media resolution, default) | Up to 3 hours | ~100 tokens per second | 
| Video (high media resolution) | Up to 1 hour | ~300 tokens per second | 
| Audio | Up to 9.5 hours per prompt | 32 tokens/sec (1,920 per minute) | 
| Images, PDF | Supported | Standard input rate | 

All three media caps are roughly the 1M context in other units. Each one works out slightly above 1,048,576 tokens, so the practical limit is a little lower. 3 hours × 3,600 s × 100 = 1,080,000 tokens. 1 hour × 3,600 s × 300 = 1,080,000 tokens. 9.5 hours × 3,600 s × 32 = 1,094,400 tokens. In practice:

- **One hour of audio** is 3,600 × 32 = 115,200 tokens. That is 115,200 × $0.75/1M ≈ $0.086 of input today, or ≈ $0.17 from January 1.
- **One hour of low-resolution video** is 3,600 × 100 = 360,000 tokens. That is ≈ $0.27 of input today, or ≈ $0.54 from January 1.

Per Google's audio docs, audio is downsampled to 16 Kbps and multi-channel audio is merged to mono, so don't expect speaker separation from the raw signal. Thirteen MIME types are accepted, and the model recognizes non-speech sounds. The video docs list nine formats and accept public YouTube URLs: up to 8 hours a day on the free tier, with no length-based limit on paid tiers. 3.8 Flash also supports agentic video understanding, where the model navigates the timeline and pulls only what it needs. Google says this uses up to 88% fewer tokens on long-form content. It is opt-in: you set `"processing": "agentic"` on the video part, and static frame sampling stays the default. Google also warns that on short clips (under 5 minutes) or broad queries it can raise time to first token and total token usage.

## How do you call Gemini 3.8 Flash?

Use the explicit model string `gemini-3.8-flash`, not the rerouted `gemini-3.7-flash`. Google's current docs use the Interactions API. A minimal Python call with thinking set to low, following the thinking docs:

```
from google import genai
client = genai.Client()  # reads GEMINI_API_KEY
interaction = client.interactions.create(
    model="gemini-3.8-flash",
    input="Extract every version number from this changelog as JSON.",
    generation_config={
        "thinking_level": "low"
    }
)
print(interaction.output_text)
print(interaction.usage)  # check total_thought_tokens before you scale up
```
The same request over REST:

```
curl -X POST "https://generativelanguage.googleapis.com/v1beta/interactions" \
  -H "x-goog-api-key: $GEMINI_API_KEY" \
  -H 'Content-Type: application/json' \
  -d '{
    "model": "gemini-3.8-flash",
    "input": "Summarize this changelog into five bullets."
  }'
```
Check the thought-token count on a representative sample of real traffic before you commit to a budget. It drives your bill, and it varies far more by task than input length does.

**Where you can run it:**

- **Google AI Studio and the Gemini API** , including the free tier. Batch enqueued-token limits are 3M (Tier 1), 400M (Tier 2) and 1B (Tier 3) per the rate-limits page. RPM and TPM are shown in AI Studio.
- **Vertex AI** , for enterprise controls and regional deployment.
- **OpenRouter** , as`google/gemini-3.8-flash` . It accepts`reasoning_effort` ,`response_format` ,`tools` ,`tool_choice` ,`seed` ,`temperature` and`top_p` , and is not currently offered through EU in-region routing.

For the history behind this release, see our Gemini 3.8 Flash vs 3.7 Flash comparison, the Gemini 3.7 Flash launch guide and the Gemini 3.5 Flash guide. All three cover models that are now rerouted.

## FAQ

### Is Gemini 3.7 Flash deprecated?

Yes. Since October 8, 2026, `gemini-3.7-flash` is deprecated, can't be selected in Google AI Studio, and Google routes its API requests to `gemini-3.8-flash` automatically. The endpoint is not shutting down. It now serves a different model, so re-test anything pinned to it.

### Is Gemini 3.5 Flash deprecated?

Yes, also as of October 8, 2026. Requests to `gemini-3.5-flash` are routed to `gemini-3.6-flash`, and the model can no longer be selected in AI Studio. Google names 3.8 Flash as an alternative replacement.

### How much does Gemini 3.8 Flash cost?

As of October 2026: $0.75 per 1M input tokens and $3.75 per 1M output tokens (thinking included) on the standard tier, through December 31, 2026. From January 1, 2027 it is $1.50 / $7.50. Batch and Flex are half the standard rate on both dates. Cached input reads are $0.075 now and $0.15 from January.

### Do Gemini 3.6 Flash prices also double?

Yes. Google's pricing page lists 3.6 Flash at the same prices as 3.8 Flash on every tier: $0.75 / $3.75 through December 31, 2026, then $1.50 / $7.50.

### Is Gemini 3.8 Flash better than 3.7 Flash?

On measured intelligence, slightly: 41 vs 39 on Artificial Analysis Intelligence Index v4.3.2, and it beats 3.7 Flash on all three agent benchmarks Google published. It also generates more tokens per task. Either way, calls to 3.7 Flash are now served by 3.8 Flash. For the detailed comparison, see our Gemini 3.8 Flash vs 3.7 Flash post.

### What is Gemini 3.8 Flash's context window?

The input limit is 1,048,576 tokens and the output limit is 65,536 tokens, per Google's model page. If you need longer outputs, Gemini 4 Argon supports up to 1,048,576 output tokens with long decode, within its unified input-plus-output window.

### Does Gemini 3.8 Flash support audio and video?

Yes, as input. It accepts text, images, video, audio and PDF, and outputs text only. Video costs about 100 tokens per second at the default low media resolution (up to about 3 hours per prompt), and audio costs 32 tokens per second (up to 9.5 hours). For speech output, use `gemini-3.8-flash-tts`. For realtime voice, use `gemini-3.8-live`.

### Should I use Gemini 4 Argon or Gemini 3.8 Flash?

Default to 3.8 Flash for volume, multimodal ingestion and most agents. Escalate to Argon ($2 / $10 through November 30, then $4 / $20) for hard coding, math and science, or long-horizon tasks where reliability matters more than cost. Artificial Analysis scores Argon 53 and 3.8 Flash 41 on v4.3.2.

### Can I still use Gemini 2.5 Flash?

Only if you have an active usage history with it. Since September 18, 2026, Google limits the Gemini 2.5 models to "users who have actively used them in the past" and gives no time window, so a long-lapsed account may not qualify. They are not deprecated and have no shutdown date. New projects are pointed to `gemini-3.5-flash-lite` or `gemini-3.8-flash`.

### Do thinking tokens cost extra?

They are billed at the output rate ($3.75 per 1M now, $7.50 from January 1), and you pay for all thought tokens generated even though the API returns only summaries. On 3.8 Flash, thinking is on by default at medium. The lowest supported level is `low`, and `minimal` returns an error.

### Is Gemini 3.8 Flash free?

It is available on the Gemini API free tier in AI Studio, with rate limits shown in AI Studio and an 8-hour-per-day cap on YouTube video ingestion. Production traffic runs on the paid tiers.

## Sources

If your team needs to audit pinned model IDs, rebuild eval suites and re-price LLM workloads before January 1, Codersera can help you hire vetted remote AI developers to extend your engineering team, with a risk-free trial.
