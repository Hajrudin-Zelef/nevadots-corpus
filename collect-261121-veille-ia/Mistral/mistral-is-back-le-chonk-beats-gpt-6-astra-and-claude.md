---
id: collect-261121-veille-ia/mistral/mistral-is-back-le-chonk-beats-gpt-6-astra-and-claude
title: "Mistral Is Back: Le Chonk Beats GPT-6 Astra and Claude"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: ["2026-10-06"]
keywords: ["mistral", "large", "model", "cyber", "astra", "open-weight", "claude", "gpt-6"]
source: https://apidog.com/blog/mistral-large-4/
source_anchor: ""
source_lines: [1, 149]
section: "Mistral"
---
Mistral has been quiet at the top end for a while. Mistral Large 3 shipped in December 2025, and since then the open-weight frontier conversation has been about Kimi, DeepSeek, GLM and Qwen. On October 6, 2026, Mistral answered with Mistral Large 4, a 1-trillion-parameter model the team calls “Le Chonk.”

The headline number is a cyber benchmark where Large 4 scores 82% and Claude Opus 5.5 and GPT-6 Astra score near zero. That is true, it is on Mistral’s launch page, and it needs one sentence of context before you share it. This article gives you that context, the specs, the real price, and where Large 4 still loses.

## TL;DR

- **What it is:** a 1.05T-parameter mixture-of-experts model with 49B active parameters, a 1.6B vision encoder, text and image input, and a 1M-token context window.
- **Where to get it:** public preview on the Mistral API today as`mistral-large-4` . Weights “by the end of the month,” so around October 27.
- **Price:** $1.36 input / $4.18 output per million tokens at list, currently shown at**$0.68 / $2.09** during the preview. Cached input is $0.07.
- **The cyber win:** 82% on the AA Cyber Index vulnerability reproduction test. Opus 5.5 and Astra score near zero**because they refuse the task** , not because they fail it.
- **The real loss:** in human-rated coding, Claude Opus 5 scored 4.22 and Large 4 scored 3.74.

## Why “Mistral is back” is fair

For most of 2026, the strongest open-weight models came from China. Mistral kept shipping, with Medium 3.5, Devstral 2 and Small 4, but nothing that competed with frontier closed models on their home turf.

Large 4 changes the pitch. Mistral claims it “significantly outperforms any open-weight model developed in the US or Europe,” and it backs that with a training story built for the EU-sovereignty crowd. The model was trained from scratch on 3,800 NVIDIA Grace Blackwell GPUs in Mistral’s own European datacenters, covers more than 160 languages including every official EU language, and lands weeks after a €3 billion Series D that Mistral calls the largest equity round ever raised by a European tech company.

Timing matters too. Large 4 arrived right after Reflection unveiled its open-weight Beam model, so the US-vs-China open-model race now has a third serious entrant.

## The cyber headline, and the catch

Here is the result everyone is screenshotting. The Artificial Analysis Cyber Index includes a test that asks a model to reproduce a real vulnerability in open-source software and then patch it. Mistral says Large 4 scores **82%, the highest of any model**.

Then the catch, in Mistral’s own words: “Several leading closed models, including Claude Opus 5.5 and GPT-6 Astra, score near zero on the same test because they refuse to perform the task.”

So read it this way:

| Claim | What it actually means | 
|---|---|
| “Large 4 beats Astra and Opus 5.5 at cyber” | On this one test, the closed models decline to answer. Large 4 answers and usually succeeds. | 
| “Large 4 is the best hacking model” | Not established. A refusal is a policy choice, not a capability ceiling. | 
| “Large 4 is unsafe” | Also not established. Mistral reports its average refusal rate on cyber prompts from JailbreakBench, StrongREJECT and AgentHarm is higher than every other open-weight model. | 

There is real capability behind the headline. Large 4 also solves 93% of Cybench, a set of 40 capture-the-flag exercises, which Mistral calls one of the highest scores reported for an open-weight model. On the B3 Agent Security Benchmark it resists 93.3% of attacks.

For security teams, that combination is the actual story: a model you can self-host that will help reproduce and patch a CVE in your own code, while still refusing most clearly harmful requests. For API teams, that is a useful tool to have on your own hardware rather than behind someone else’s policy filter.

## Where Large 4 beats GPT-6 Astra outside cyber

The cyber result is the loud one. Two other wins against Astra are quieter and more meaningful, because both models actually attempted the task:

| Benchmark | Mistral Large 4 | GPT-6 Astra | Notes | 
|---|---|---|---|
| Dense 200 (visual grounding) | **42%** | 41% | A one-point lead. Real, but narrow. | 
| Finance Agent v2 (vals.ai) | Ahead | Behind | Mistral reports the ranking, not the scores. | 
| Harvey Legal Agent Benchmark | Best open model | Listed | No head-to-head number published. | 

Against other open models, the gap is wider:

| Benchmark | Mistral Large 4 | Who it beats | 
|---|---|---|
| AutomationBench (657 workflows) | 59.9% | Kimi K3, DeepSeek V4 Pro | 
| AA-Briefcase (knowledge work) | 1,393 Elo | DeepSeek V4 Pro | 
| DeepSWE v1.1 (coding) | 61.7% | Leads open-weight models | 
| SWE-Atlas-QnA | 59.4% |  | 
| Terminal-Bench 4.0 | 28.3% |  | 

All of these are Mistral-reported, preview-model numbers. No independent lab has re-run them yet, and Euronews reports that reinforcement learning was still wrapping up at launch. Treat them as a strong signal, not a verdict.

## Where it still loses

Mistral published one result where a closed model clearly wins. In a human evaluation of coding quality run by Surge AI across five models, Large 4 Preview ranked second:

| Model | Human coding score (out of 5) | 
|---|---|
| Claude Opus 5 | **4.22** | 
| Mistral Large 4 Preview | 3.74 | 
| GLM-5.3 | 3.60 | 
| Kimi K3 | 3.59 | 
| GLM-5.2 | 3.40 | 

That is a clean win over the best Chinese open models, and a half-point gap behind Claude. Mistral’s own phrasing is that Large 4 is “closing the gap” with frontier models in coding, which is an honest read.

Also missing: there is no comparison against Claude Fable 5.1 or GPT-6 Sol anywhere in the launch materials. If someone tells you Large 4 beats Fable, ask for the benchmark.

## Specs at a glance

| Spec | Mistral Large 4 | 
|---|---|
| API model ID | `mistral-large-4` (alias`mistral-large-4-0` ) | 
| Version | 26.10, public preview | 
| Architecture | Mixture-of-experts, hybrid instruct + reasoning | 
| Parameters | 1.05T total, 49B active, 1.6B vision encoder | 
| Context window | 1M tokens | 
| Input | Text, images | 
| Reasoning | `reasoning_effort` parameter (`"high"` or`"none"` ) | 
| Tools | Function calling, structured outputs, built-in agent tools | 
| Endpoints | `/v1/chat/completions` ,`/v1/conversations` ,`/v1/agents` ,`/v1/batch` | 
| Weights | Open, by end of October 2026; license not yet published | 

## Price: the cheapest frontier-class model you can call today

This is the part that should make API teams pay attention. Mistral’s pricing page lists Large 4 at half its list price during the preview:

| Model | Input / 1M | Output / 1M | Open weights | 
|---|---|---|---|
| **Mistral Large 4 (preview price)** | **$0.68** | **$2.09** | Yes, late October | 
| Mistral Large 4 (list price) | $1.36 | $4.18 | Yes, late October | 
| Claude Opus 5.5 | $4.00 | $20.00 | No | 
| GPT-6 Astra | $10.00 | $50.00 | No | 

At preview pricing, Large 4 output costs roughly a twenty-fourth of Astra’s. Even at list price it is about 12x cheaper on output. Cached input drops to $0.07 per million, which matters for agents that resend the same system prompt and tool definitions on every turn.

Mistral has not said when the preview discount ends, so budget against the list price.

## Should you switch?

**Try it now if:**

- You run agentic workflows or knowledge-work automation and pay frontier prices. AutomationBench and AA-Briefcase suggest Large 4 is competitive here at a fraction of the cost.
- You need multilingual output across EU languages or EU-hosted inference for compliance.
- You do defensive security work and keep hitting refusals on legitimate vulnerability reproduction.
- You want a frontier-class model you will be able to self-host in a few weeks.

**Wait if:**

- Code quality is your main metric. Claude still leads on human-rated coding.
- You need numbers you can defend in a review. Wait for independent benchmarks after the weights ship.
- You run Large 3 in production. The preview may still change, and Mistral has not published a stable versioned ID yet.

## How to test Large 4 against your own APIs in Apidog

Benchmarks tell you how a model does on someone else’s tasks. The faster way to decide is to run Large 4 on your own prompts and compare it with the model you pay for today. Apidog handles that without a line of glue code:

1. **Save the request once.** Create`POST https://api.mistral.ai/v1/chat/completions` , store your key as an environment variable, and set`Authorization: Bearer {{MISTRAL_API_KEY}}` .
2. **Clone it per model.** Duplicate the request, change only the`model` field (`mistral-large-4` vs. your current model), and send both. Apidog shows the response, status, latency and size side by side, and the`usage` block gives you token counts for a cost comparison.
3. **Add assertions.** Check for a`200` status, a non-empty answer, and a JSON Schema match if you ask for structured output. That turns a one-off experiment into a regression test.
4. **Rerun it on every model update.** Group the requests into a test scenario and rerun it when Mistral updates the preview or ships the weights. You will see whether quality drifted before your users do.

Two Large 4 strengths fit naturally into API work. Its security results make it a good second reviewer for API security testing, for example when you reproduce an auth bypass in your own staging API. Its function calling and structured outputs let you turn an OpenAPI spec designed in Apidog into tool definitions an agent can call.

For the full code walkthrough, including keys, reasoning chunks, images, function calling and cost math, see our Mistral Large 4 API guide. Coming from an older Mistral model? The Mistral AI API basics and Mistral Medium 3.5 API guide still apply: same base URL, same auth, new model ID.

## FAQ

**Is Mistral Large 4 open source?** It is open-weight. Mistral says the weights ship by the end of October 2026. The license has not been published yet, so do not assume it matches Large 3’s Apache 2.0.

**Does Mistral Large 4 really beat GPT-6 Astra?** On visual grounding (42% vs 41%) and Finance Agent v2, yes, by Mistral’s numbers. On the cyber vulnerability test, Astra scores near zero because it refuses, so that is not a like-for-like win.

**Does it beat Claude?** Not on coding. Claude Opus 5 leads the human coding evaluation 4.22 to 3.74. Claude Opus 5.5 also refuses the cyber reproduction task, which is why it scores near zero there.

**What does “Le Chonk” mean?** It is Mistral’s unofficial nickname for the model, a joke about its size. The official name is Mistral Large 4, or ML4.

**Can I use it for free?** Mistral’s free plan includes $10 a month in API credits and access to its latest models in Le Chat and Vibe. At preview pricing, $10 buys roughly 4.8 million output tokens on Large 4.

## Bottom line

Mistral is back in the frontier conversation. Large 4 is the strongest open-weight model from outside China by Mistral’s numbers, it costs a fraction of Astra or Opus 5.5, and it will run on your own hardware within weeks. The “beats Astra and Claude at cyber” line is real but comes from refusals, and the honest coding verdict is “second to Claude.” Test it on your own APIs in Apidog now while the preview price lasts, and hold your production decision until independent benchmarks arrive after October 27.
