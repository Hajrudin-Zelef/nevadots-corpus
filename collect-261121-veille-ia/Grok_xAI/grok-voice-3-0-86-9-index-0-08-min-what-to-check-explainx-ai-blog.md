---
id: collect-261121-veille-ia/grok-xai/grok-voice-3-0-86-9-index-0-08-min-what-to-check-explainx-ai-blog
title: "Grok Voice 3.0: 86.9 Index, $0.08/min, What to Check | explainx.ai Blog"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["voice", "grok", "speech-to-speech", "index", "artificial", "analysis", "model", "gpt-live-1"]
source: https://explainx.ai/blog/grok-voice-3-0-speech-to-speech-86-9-index-starlink-70-percent-2026
source_anchor: ""
source_lines: [1, 105]
section: "Grok_xAI"
---
**Update — October 20, 2026:** New coverage — Eleven Realtime v1 Mini and Max, ElevenLabs' speech-to-speech launch with an OpenAI-compatible API.

SpaceXAI has released Grok Voice 3.0, which it calls "the world's most intelligent speech-to-speech model." In xAI's announcement, the headline is an independent number: Artificial Analysis ranks it first on its Speech-to-Speech Index with a score of 86.9. The price stays at $0.08 per minute, and agents on `grok-voice-latest` have already been switched over.

That is a strong launch, and most of it holds up as reported. Two parts need care: the one benchmark where a rival scores higher, and the Starlink statistics, which are the company's own. This post lays out the numbers and what to test before you move traffic.

## TL;DR

| Question | Answer | 
|---|---|
| What is it? | Speech-to-speech model in xAI's Voice Agent API | 
| Price | $0.08/min ($4.80/hr), same as Grok Voice 2.0 | 
| Headline result | #1 on Artificial Analysis Speech-to-Speech Index: 86.9 | 
| Where it is not claimed as first | tau-Voice customer-support test: 71.6%, which xAI calls "comparable" with GPT-Live-1 (Astra) | 
| Latency | Under one second; about 40% lower than GPT-Live-1 on tau-Voice, per xAI | 
| Switch needed? | No for `grok-voice-latest` users | 
| Verified independently? | Index and cost figures come from Artificial Analysis via xAI; Starlink figures are xAI's | 

## The benchmark table

The comparison below is xAI's table, which cites Artificial Analysis testing dated October 13, 2026.

| Benchmark | Grok Voice 3.0 | Grok Voice 2.0 | GPT-Live-1 (Astra, Medium) | Gemini 3.8 Live (Ext. Thinking, High) | 
|---|---|---|---|---|
| Speech-to-Speech Index (overall) | 86.9 | 81.3 | 83.2 | 82.6 | 
| Big Bench Audio (speech reasoning) | 98.5% | 97.2% | 90.1% | 97.7% | 
| Speech Agent Arena Elo (human preference) | 1070 | 1011 | 1048 | 990 | 
| Task success rate | 94.8% | 94.6% | 87.4% | 89.1% | 
| Full Duplex Bench (turn-taking) | 97.0% | 95.1% | 94.9% | 91.9% | 

Artificial Analysis defines the index as an equal-weighted score over four parts: speech reasoning, agentic performance on tau-Voice, arena preference, and task success. A model must post all four to be ranked. The biggest jump over the predecessor is in preference Elo (1011 to 1070) and the index overall (81.3 to 86.9).

### Where xAI's wording is softer

On tau-Voice, a simulated customer-support benchmark with accents, background noise and interruptions, xAI reports 71.6% for Grok Voice 3.0 and calls it "comparable with GPT-Live-1 (Astra), but at 40% lower latency." That is not a claim of first place: "comparable" leaves room for the rival to be slightly ahead, and xAI's text does not give the rival's score. The exact chart values are in an image we could not read, so check Artificial Analysis for them. Whether the trade is right depends on your calls: for a phone line, response speed is a large part of perceived quality, but a wrong refund is costlier than a slow one.

For the OpenAI side of this comparison, see our write-up of GPT-Live-1 reaching the API.

### Cost per task

xAI says Artificial Analysis measured average cost per task on its Big Bench Audio run and that Grok Voice 3.0 comes in under both GPT-Live-1 (Astra, Medium) and GPT-Realtime-2.1; the dollar values are in a chart we could not read. That is one benchmark's cost, not your invoice. Real calls are longer, use tools and carry telephony fees.

## Why speech-to-speech matters for voice agents

Two speech bubbles passing a bead, showing turn-taking in a Grok Voice 3.0 speech-to-speech conversation

Most voice stacks chain three services: speech-to-text, a language model, then text-to-speech. xAI's builder page makes the case against that design: each hop adds cost, latency and a new failure mode, and the middle model never hears tone, hesitation or interruption. A speech-to-speech model takes audio in and produces audio out in one path, which is why metrics like Full Duplex Bench, which measures pauses, turn-taking and interruptions, exist at all. Grok Voice 3.0's 97.0% there, versus 94.9% for GPT-Live-1 and 91.9% for Gemini 3.8 Live in xAI's table, is the number that most directly describes how a call feels.

The trade-off is control. In a three-stage pipeline you can swap the language model, log the text and apply text filters. In a single speech-to-speech path you rely more on the vendor's guardrails and on the recordings and transcripts the platform gives you. xAI says every call is recorded and transcribed, with playback and tool-use logs.

## Which model for which job

This table is our reading of the published numbers, not a ranking from a third party.

| If your priority is | Look first at | Why | 
|---|---|---|
| Natural conversation, interruptions | Grok Voice 3.0 | Highest Full Duplex Bench and arena Elo in xAI's table | 
| Support-task accuracy on tau-Voice | Test both | xAI calls its 71.6% "comparable" with GPT-Live-1, at 40% lower latency | 
| Lowest cost per benchmark task | Grok Voice 3.0 | xAI says it is below GPT-Live-1 and GPT-Realtime-2.1 on Big Bench Audio | 
| Existing Google stack | Gemini 3.8 Live | 82.6 on the index; strong on Big Bench Audio at 97.7% | 
| Zero code changes from Grok Voice 2.0 | Grok Voice 3.0 | `grok-voice-latest` already switched | 

Call-center headset with sound waves, representing voice support agents in the Grok Voice 3.0 era

## ServiceNow's EVA-Bench

xAI also cites ServiceNow's open-source EVA-Bench, which scores voice agents on accuracy and conversational experience across 200+ enterprise scenarios. Grok Voice 3.0 gets 74% accuracy, the highest reported, and is best when accuracy and experience are scored together. The quote supporting this is from Nitin Aggarwal of ServiceNow, published with xAI's announcement: the model "sets a new Pareto frontier on EVA-Bench." As with any vendor-hosted quote, the statement is welcome but the benchmark is open source, so you can run it yourself.

## The Starlink numbers: claimed, not audited

xAI says Starlink's call centers use Grok Voice to answer 20,000 customer calls a day across multiple languages, that roughly 70% are resolved with no human in the loop, and that one in four sales calls ends in a purchase before the caller hangs up. A single agent reportedly verifies customers, troubleshoots hardware, issues credits and ships replacement equipment.

What is missing: a definition of "resolved" (no human transfer? no repeat call?), the time window, and any outside audit. Starlink is a sister company under the same owner, so this is also a first-party deployment. That makes the 70% a useful signal that the system works at scale, not a benchmark you can compare against your own deployment. Our earlier pieces cover Grok Voice Think Fast 2.0 and the no-code Voice Agent Builder.

## In cars as well

Per xAI, the model handles millions of conversations a day in Tesla vehicles. Drivers use Connectors for requests such as checking a calendar or answering an email, and Grok Bot for bigger tasks like ordering coffee or booking dinner, hands-free. It also powers Grok Bot's voice mode. For how Grok Bot compares with other agents, see OpenAI Dots vs Grok Bot vs Meta Muse.

## Pricing and what the bill really looks like

The API rate is $0.08 per minute. The earlier Voice Agent Builder launch explains how builder agents are billed: the API rate with voices included, no separate platform fee, and $0.01 per minute for telephony on a free provisioned number. Some third-party pages still quote $0.05 per minute, which was the rate at the builder's beta launch and is out of date. Tool calls such as web search may be billed separately, so check the docs for your setup.

A rough calculation, using only those published rates: a five-minute call on a provisioned number is about $0.45 ($0.08 times five, plus $0.01 times five). One thousand such calls a day is about $450 before tools. Compare that with your current human cost per call and with the other stack options you are weighing.

## What people are asking

**Is "world's most intelligent voice model" a fair claim?** It is xAI's wording. The independent part is the Artificial Analysis index ranking, where Grok Voice 3.0 leads by 3.7 points over the next model in the table. On individual components the picture is mixed, as the tau-Voice gap shows, so "best overall index score" is the defensible version.

**Does the unchanged price mean a better deal?** Yes in the narrow sense that capability rose at the same rate card. Whether it is cheaper than a rival depends on the whole stack, including telephony, tools and any minimums, which vary by vendor.

**Can I use my own voice?** xAI's builder page says agents can use built-in voices or a clone of your brand's voice made from about two minutes of audio. Cloning carries consent and impersonation obligations; only use voices you have rights to.

**What about languages?** The builder page mentions 25+ languages and the Starlink deployment is described as multilingual, but we saw no per-language results for 3.0, so test your languages directly.

## Should you switch? A test plan

1. **Replay your hardest calls.** Benchmarks use simulated callers. Run recordings of your real difficult cases: noise, accents, interruptions.
2. **Measure containment honestly.** Define resolved as no transfer and no repeat contact within seven days, then compare against your baseline.
3. **Test guardrails.** xAI's builder page describes guardrails for things like reading back card numbers; verify they hold under adversarial callers.
4. **Check latency from your region and telephony path.** "Under one second" is a model-side figure.
5. **Keep a fallback.** A second provider behind a feature flag costs little and protects you from outages.

## Related reading

*Figures are from xAI's October 14, 2026 announcement and Artificial Analysis testing dated October 13, 2026; check the docs for current pricing.*
