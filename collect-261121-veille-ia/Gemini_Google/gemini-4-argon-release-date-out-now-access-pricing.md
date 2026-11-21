---
id: collect-261121-veille-ia/dev-langages/gemini-4-argon-release-date-out-now-access-pricing
title: "Gemini 4 Argon Release Date (Out Now): Access & Pricing"
domain: dev-langages
role: reference
task: article
actors: ["Google"]
dates: ["2026-10-13"]
keywords: ["gemini", "argon", "google", "ultra", "pricing", "model", "output", "release"]
source: https://techpilot.ai/gemini-4-argon-release-date-access/
source_anchor: ""
source_lines: [1, 123]
section: "Gemini_Google"
---
## On this page Gemini 4 Argon release date and status

Independent and reader-supported. Some links may earn us a commission; it never changes our rankings. Editorial policy

**Quick answer:** Gemini 4 Argon was released on **October 13, 2026**, and it is now generally available worldwide. You can use it in the Gemini app with a Google AI Pro or Ultra plan, in Google AI Studio and the Gemini API on the paid tier (model ID `gemini-4-argon`), and on Google Cloud. API pricing is $2 per 1M input tokens and $10 per 1M output tokens until November 30, 2026, then $4 and $20.

Google first showed Argon on September 30, but only vetted cyber defenders and early testers could use it for the first two weeks. That gated phase is over. Below is where you can use it today, what it costs, how to start, and the limits worth knowing before you build on it.

## Gemini 4 Argon release date and status

- **Announced:** September 30, 2026, as a limited preview for Fairwind Program cyber defenders and trusted testers.
- **General availability:** October 13, 2026, globally, per Google’s launch post.
- **Status today:** live for developers, enterprises and paying Gemini app users. It is not on the Gemini API free tier.
- **Security version:** an Argon build without cyber guardrails stays limited to Fairwind Program partners, which Google is now expanding to hundreds more organizations.

## Where to use Gemini 4 Argon

### Gemini app (Google AI Pro and Ultra)

Argon is in the Gemini app and in AI Mode in Search for Google AI Pro and Google AI Ultra subscribers. The free Gemini plan does not include it. Google does not publish a fixed number of Argon prompts for consumer plans: limits are compute-based, and higher plans get more headroom. Ultra is the plan to pick if you want Argon heavily, because it also unlocks Argon in Google Antigravity and AI Studio.

### Google AI Studio

AI Studio is the fastest way to try Argon in a browser. You need either a paid Gemini API tier or a Google AI Ultra subscription. Pick `gemini-4-argon` in the Playground model menu and set the thinking level before you run long prompts.

### Gemini API

Developers call it with the model ID `gemini-4-argon` through the Interactions API and the `google-genai` SDKs for Python and JavaScript. It also works as the model for Managed Agents, and through third-party tools such as OpenRouter, Vercel, LiteLLM and OpenCode. A billing-enabled project is required, since the free tier does not include Argon.

### Google Cloud: Gemini Enterprise Agent Platform (formerly Vertex AI)

Vertex AI is now called Gemini Enterprise Agent Platform, and existing Vertex AI code keeps working. Enterprise teams get Argon there with the same model ID, the same launch pricing, and Google Cloud’s data controls. Business users without an API can also pick it in Gemini for business on web, desktop and mobile, and Workspace admins can check plan limits: Google lists up to 200 Argon prompts a day on AI Expanded Access and up to 500 on AI Ultra Access.

### Google Antigravity

Google AI Ultra subscribers can select Argon as the active model in Antigravity, Google’s agent-first coding platform, for long planning runs and multi-agent jobs. If coding agents are your main use, compare it with the options in our best AI agents for coding roundup.

## Gemini 4 Argon pricing

These are the paid-tier API rates per 1 million tokens, as listed on Google’s Gemini API pricing page:

| Usage | Until Nov 30, 2026 | From Dec 1, 2026 | 
|---|---|---|
| Input | $2.00 | $4.00 | 
| Output (including thinking tokens) | $10.00 | $20.00 | 
| Cached input (implicit caching) | $0.10 | $0.20 | 
| Batch or Flex input | $1.00 | $2.00 | 
| Batch or Flex output | $5.00 | $10.00 | 
| Priority input / output | $3.60 / $18.00 | $7.20 / $36.00 | 

Three things move your bill more than the headline rate:

- **Thinking tokens count as output.** At the`high` thinking level, Argon can reason for a long time before it answers, and you pay output rates for all of it.
- **Long outputs add up fast.** A single response of 250,000 tokens costs $2.50 in output at the intro rate and $5 from December 1, before input.
- **Caching is automatic.** Repeated context is billed at 95% off, and explicit context caching is not supported, so keep stable instructions at the start of the prompt.

Search grounding comes with 5,000 free requests a month shared across Gemini 3 and newer models, then $14 per 1,000. If tokens are new to you, our guide to what tokens are in AI shows how to estimate them. On the consumer side, Argon is bundled into Google AI Pro and Ultra subscriptions at no extra cost.

## How to use Gemini 4 Argon now

1. **Pick your route.** For chat and documents, open the Gemini app on a Pro or Ultra plan and select Argon in the model picker. For building, go to Google AI Studio.
2. **Enable billing.** In AI Studio, attach a billing account to your project so the key moves to the paid tier, or use a Google AI Ultra plan.
3. **Choose the model.** Select`gemini-4-argon` in the Playground, or set it as the`model` in your SDK call.
4. **Set the thinking level.** Use`low` for fast extraction and routing,`medium` (the default) for most coding and agent work, and`high` for hard debugging, maths and security fixes.
5. **Plan for long runs.** Raise client timeouts to 15–20 minutes or run requests with`background=True` , and set`max_output_tokens` above 65,536 only when you need a long answer.
6. **Clean up old code when migrating.** Swap`thinking_budget` for`thinking_level` , and remove`temperature` ,`top_p` ,`top_k` and explicit`cached_content` : Argon ignores or rejects them.

A minimal Python call looks like this:

```
from google import genai
client = genai.Client()
interaction = client.interactions.create(
    model="gemini-4-argon",
    input="Summarize the trade-offs in this design doc and list open risks.",
    generation_config={"thinking_level": "medium"},
)
print(interaction.output_text)
```
## Limits and context window

- **Context window:** a unified 1M-token window in the Gemini API that input, thinking and output share.
- **Output length:** 65,536 tokens by default. With long decode, a response can run up to 1M tokens minus your input. Earlier Gemini models stopped at 64K.
- **Inputs:** text, code, images, audio, video and PDFs.
- **Free tier:** none in the API. Consumer access needs Google AI Pro or Ultra.
- **App limits:** compute-based for Pro and Ultra, with no fixed public prompt count. Workspace plans list 200 or 500 Argon prompts a day depending on the add-on.
- **Sampling controls:** custom`temperature` ,`top_p` and`top_k` values are ignored.
- **Cyber guardrails:** the public model blocks offensive tasks such as pentesting and exploit writing. Defensive work like finding and fixing vulnerabilities is allowed.
- **Latency:** hard prompts at`high` thinking can take several minutes, so it is not the model for snappy chat.

## Is Gemini 4 Argon worth switching to?

For long, multi-step work, yes. Google reports first place on the Vals Index for finance, legal, tax and coding work, and a tie for first on CWE-bench v1 at 68% for fixing security flaws. Independent rankings put it in the top group rather than alone at the top, but it costs less per task than its closest rivals. For quick chat, Gemini 3.8 Flash is cheaper and faster, and Google itself suggests pairing Argon as the planner with Flash as the worker.

For head-to-head comparisons, see Muse Spark Open vs Gemini 4 Argon, our best AI model ranking, and Claude API vs Gemini API if you are choosing a provider.

## Frequently asked questions

### When was Gemini 4 Argon released?

Google announced Gemini 4 Argon on September 30, 2026, as a limited preview, and made it generally available worldwide on October 13, 2026.

### Can I use Gemini 4 Argon for free?

Not in the API: Argon is not on the Gemini API free tier. In the Gemini app it is included with Google AI Pro and Google AI Ultra subscriptions, not the free plan.

### How much does Gemini 4 Argon cost?

On the paid Gemini API tier it costs $2 per 1M input tokens and $10 per 1M output tokens until November 30, 2026. From December 1 it is $4 and $20. Cached input is 95% off.

### What is the Gemini 4 Argon model ID?

Use gemini-4-argon in the Gemini API, Google AI Studio, Google Antigravity and on Google Cloud.

### What is Gemini 4 Argon’s context window?

Argon has a unified 1M-token window in the Gemini API that input, thinking and output share. Output defaults to 65,536 tokens, and long decode can extend a response up to 1M tokens minus the input.

### Is Gemini 4 Argon on Vertex AI?

Yes. Vertex AI is now called Gemini Enterprise Agent Platform on Google Cloud, and Argon is available there with the model ID gemini-4-argon.

 **Found this useful?** Share it with someone comparing AI tools.
