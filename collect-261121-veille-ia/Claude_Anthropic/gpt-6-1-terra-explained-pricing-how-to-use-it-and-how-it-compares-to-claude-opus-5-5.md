---
id: collect-261121-veille-ia/claude-anthropic/gpt-6-1-terra-explained-pricing-how-to-use-it-and-how-it-compares-to-claude-opus-5-5
title: "GPT-6.1 Terra Explained: Pricing, How to Use It, and How It Compares to Claude Opus 5.5"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["gpt-6", "terra", "input", "model", "output", "how", "astra", "openai"]
source: https://dev.to/weiyi/gpt-61-terra-explained-pricing-how-to-use-it-and-how-it-compares-to-claude-opus-55-1b8
source_anchor: ""
source_lines: [1, 200]
section: "Claude_Anthropic"
---
On October 26, OpenAI released GPT-6.1 Terra, with the API name `gpt-6.1-terra`.

When I wrote about GPT-6.1 Sol in early October, Sol was the only model in the 6.1 series. Now Terra has joined it, and it costs exactly half as much as Sol.

This post covers Terra end to end: where it sits in the lineup, specs and pricing, where you can use it, how to call it from the API, the gotchas to watch for, and finally a comparison with Claude Opus 5.5 and how to choose between them.

OpenAI now splits its models into four size tiers, from cheapest to most capable:

| Tier | Current model | Positioning | 
|---|---|---|
| Luna | GPT-6 Luna | Cheapest, good for extraction, classification and batch jobs | 
| **Terra** | **GPT-6.1 Terra** | Second cheapest, fast, strong at vision, computer use and writing | 
| Sol | GPT-6.1 Sol | The everyday workhorse | 
| Astra | GPT-6 Astra | Most capable, most expensive | 

Terra replaces GPT-5.6 Terra. There was never a GPT-6.0 Terra; it jumped straight from 5.6 to 6.1.

OpenAI's one-line pitch for it: **a fast, efficient model that excels at visual reasoning, precise computer use, and clear, natural writing.** The system card describes it as "built for high-volume professional work."

The typical use cases listed in the official guide are quite concrete:

- matching product photos against a product catalog;
- spotting the differences between two technical drawings;
- clicking buttons and filling in forms in an app UI;
- drafting and editing copy from provided materials;
- small, well-scoped code fixes.

At the same time, OpenAI is blunt about it: **for the most complex coding tasks, GPT-6.1 Sol or GPT-6 Astra are still the recommended choices.** Terra isn't here to replace Sol. Its job is to make high-volume, well-defined, verifiable work fast and cheap.

| Item | GPT-6.1 Terra | 
|---|---|
| Context | 1,050,000 tokens (up to 922K input, up to 128K output) | 
| Input / output | Text and image input, text-only output | 
| Knowledge cutoff | June 1, 2026 | 
| Reasoning effort | none, low, medium (default), high, xhigh, max | 
| Endpoints | Responses, Chat Completions, Batch | 
| Tools | Function calling, structured outputs, web search, file search, code interpreter, computer use, MCP and more | 
| Not supported | Fine-tuning, Predicted Outputs, audio and video | 

One detail that's easy to miss: **Terra supports `none`, meaning no reasoning at all**, which neither GPT-6.1 Sol nor GPT-6 Astra offers. For tasks that only need quick rewriting or extraction, turning reasoning off saves another chunk of time and money.

USD per million tokens:

| Model | Input | Cached input | Output | 
|---|---|---|---|
| GPT-6 Luna | 0.10 | 0.01 | 0.50 | 
| **GPT-6.1 Terra** | **1.00** | **0.05** | **5.00** | 
| GPT-6.1 Sol | 2.00 | 0.10 | 10.00 | 
| GPT-6 Astra | 10.00 | 1.00 | 50.00 | 
| GPT-5.6 Terra (previous generation) | 2.00 | 0.20 | 12.00 | 

A few billing rules to keep in mind:

- 
**Long-context surcharge** : if a single request's input exceeds 272K tokens, the entire request is billed at 2x for input and 1.5x for output, which puts Terra at 2.00 / 0.10 / 7.50.
- 
**Service tiers** : Batch and Flex are half price, Fast (formerly called Priority) is double. Terra has no Ultrafast tier.
- 
**Cache writes** : $1.25 for a 30-minute retention, $2.50 for 12 hours.
- 
**Web search** is billed separately at $10 per thousand calls.

Compared with the previous GPT-5.6 Terra, input is half the price and output drops from $12 to $5, nearly 60% cheaper. A lot of coverage says "50% cheaper," but that only refers to input.

**API**: model name `gpt-6.1-terra`, with no dated snapshot versions.

**ChatGPT**: it only shows up in ChatGPT Work and Codex, **not in regular chat**. Plus, Pro, Business, Enterprise and Edu all have access; you pick it manually from the model list, and overall usage limits are unchanged. For Enterprise and Edu members, an admin has to enable both the model and Work / Codex.

**Codex**: requires Codex CLI 0.159.0 or later:


```
codex -m gpt-6.1-terra
```
You can also switch mid-session with `/model`. In the Codex desktop app and the Work web app, pick it from the model list under "Advanced."

**Other platforms**: GitHub Copilot and Microsoft Foundry both shipped it on launch day. According to GitHub, Terra "solves significantly more terminal and C# coding tasks than GPT-5.6 Terra." Some third-party reports also say it went live on Amazon Bedrock at the same time, but I couldn't confirm that on any official page.

Here's what OpenAI claims in the announcement:

- At medium reasoning effort, it scores **15 percentage points higher than GPT-5.6 Terra on computer-use tasks, at roughly a third of the cost** ;
- It can match GPT-6 Astra's level at one-tenth of Astra's per-token price;
- On average it completes tasks **more than 3x faster** than Astra and 6.1 Sol.

I wasn't able to open the official page to verify the exact numbers in the announcement charts, so the figures below come from media transcriptions and should be taken as indicative only:

| Benchmark (reasoning effort) | GPT-6.1 Terra | GPT-5.6 Terra | GPT-6.1 Sol | GPT-6 Astra | 
|---|---|---|---|---|
| Agents' Last Exam (max) | 60.0% | 50.7% | 58.6% | 59.3% | 
| Terminal-Bench 4.0 (max) | 50.3% | 21.5% | 55.9% | 58.2% | 
| OSWorld 2.1 (max, offline subset) | 74.2% | 57.1% | ~75.9% | ~75.8% | 
| MMMU-Pro | 85.5% | — | — | — | 

Two things worth noting:

- 
**Terminal-Bench 4.0 depends heavily on reasoning effort** : Terra scores just 24.8% at low and 35.8% at medium, and only reaches 50.3% at max. If you're using Terra for coding, don't set reasoning effort too low.
- 
**The "15 percentage points" doesn't add up** : going by the chart numbers, Terra at medium scores 57.6% and GPT-5.6 Terra at its highest effort scores 50.7%, a gap of only 6.9 points. OpenAI's 15 points may come from a same-effort comparison, but there's no way to verify that right now.

On the composite Intelligence Index from independent evaluator Artificial Analysis, Terra scores 51 at max effort (6.1 Sol 55, Astra 57, Claude Opus 5.5 58). Another source reports 48, so treat this as a rough reference too.

The simplest possible call:


```
from openai import OpenAI
client = OpenAI()
resp = client.responses.create(
    model="gpt-6.1-terra",
    reasoning={"effort": "medium"},
    input="Based on the meeting notes below, write a follow-up email of no more than 200 words: ...",
)
print(resp.output_text)
```
If you're migrating from an older model, these are the spots where things most often break:

**1. With reasoning on, you can't pass `temperature` or `top_p`**

Whenever reasoning effort is anything other than `none`, you need to drop `temperature`, `top_p` and `top_logprobs`. In Chat Completions you also need to remove `logprobs`, and in Responses you need to remove `message.output_text.logprobs` from `include`. Developers who've tested it report that leaving these parameters in returns an HTTP 400 straight away.

**2. `minimal` is gone**

If you used to use `minimal`, switch to `low`. Passing `minimal` also returns a 400.

**3. Tool calls must go through the Responses API**

Chat Completions handles plain conversations, but tools like function calling, web search and computer use only work in the Responses API. Older projects that call tools through Chat Completions will need to migrate.

**4. To change reasoning effort mid-conversation, don't just change the request parameter**

OpenAI recommends adjusting it with a `configuration_update` input item and keeping the request-level `reasoning.effort` unchanged, so you don't break the prompt cache.

**5. The cache parameter has been renamed**

When migrating from GPT-5.5 or earlier, replace `prompt_cache_retention` with `prompt_cache_options.ttl`, for example `"30m"`.

OpenAI's advice: start at `medium` and only go higher when it's genuinely worth it. Before migrating, take a representative set of images and workflows and run both Terra and your current model on them at the same reasoning effort to compare.

Computer use is Terra's headline feature, but a few figures in its system card deserve a careful look:

- It has fewer severe issues than 6.1 Sol and Astra: across roughly 49,000 simulated Codex tasks, Terra had 22 flags at severity level 3 or above, versus 24 for 6.1 Sol and 26 for Astra;
- But it has more low-severity issues: its flag rates for concealing uncertainty, circumventing restrictions, data exfiltration and destructive actions are all higher than 6.1 Sol and Astra;
- 
**In 29.6% of tests, it completed the task in a way that violated the intent of the constraints** ;
- In 13.75% of tests, it followed instructions from what appeared to be a "peer AI" and performed unauthorized actions, compared with just 1.25% for 6.1 Sol.

So my advice is simple: letting Terra click buttons and fill in forms is fine, but **for irreversible actions like deleting, paying, sending or submitting, always add a human confirmation step**.

Let me get one thing straight first: **these two are not in the same tier.** Terra is OpenAI's second-cheapest tier, while Claude Opus 5.5 is Anthropic's general-purpose flagship. For a like-for-like comparison, Opus 5.5 lines up with GPT-6.1 Sol or GPT-6 Astra; the Claude model priced closest to Terra is Claude Sonnet 5.5 ($2 input, $10 output).

That said, plenty of people do compare these two, so here are the key differences:

|  | GPT-6.1 Terra | Claude Opus 5.5 | 
|---|---|---|
| Released | 2026-10-26 | 2026-09-22 | 
| Positioning | Fast and cheap; vision, computer use, writing | General-purpose flagship for long-running coding and knowledge work | 
| Price (per million tokens) | $1 input, $5 output | $4 input, $20 output | 
| Cached input | $0.05 | $0.20 | 
| Long context | Whole request surcharged above 272K | Flat pricing up to 1M, no surcharge | 
| Context / output | ~1M / 128K | 1M / 128K | 
| Reasoning | Can be fully turned off (none) | Always on, medium by default | 
| Knowledge cutoff | June 2026 | June 2026 | 
| Where to use it | API, ChatGPT Work, Codex, Copilot | API, all Claude subscriptions, Claude Code, Bedrock, Vertex and more | 

Benchmarks where both companies have published results:

| Benchmark | GPT-6.1 Terra | Claude Opus 5.5 | 
|---|---|---|
| Terminal-Bench 4.0 | 50.3% (max) | 66.4% (xhigh) | 
| Terminal-Bench-Science 0.1 | 58.3% (max) | 58.7% | 
| OSWorld 2.1 | 74.2% (offline subset) | 81.8% (subset of tasks) | 
| Agents' Last Exam | 60.0% | 63.2% (figure from OpenAI's chart) | 

Read this table with caution: the reasoning efforts and task subsets differ between the two, and the Terra numbers are transcribed, so it isn't a strict comparison. On top of that, the two companies sometimes publish different numbers for the same thing. For Opus 5.5 on Terminal-Bench-Science, for example, Anthropic's own figure is 58.7%, while OpenAI's chart shows 63.3% (labeled "with fallback").

The rough takeaway: **Opus 5.5 is clearly stronger at terminal-based coding tasks; the gap on scientific computing and computer use is small; and Terra costs only a quarter as much.**

| Scenario | Pick | Why | 
|---|---|---|
| High-volume drafting, editing, and summarizing from source material | Terra | Cheap, fast, and reasoning can be turned off | 
| Image understanding, comparing drawings, reading screenshots | Terra | A headline capability, at a low price | 
| Well-defined computer use with verifiable results | Terra | Its core use case, but irreversible actions need human confirmation | 
| Complex coding, long refactors, multi-step debugging | Opus 5.5 or GPT-6.1 Sol | The Terminal-Bench gap is clear, and OpenAI itself recommends this | 
| Feeding very long context (over 272K) | Opus 5.5 | No surcharge up to 1M; Terra surcharges the whole request above 272K | 
| Tight budget but you want Claude | Sonnet 5.5 | The same-tier rival priced close to Terra | 

Following that logic, the most cost-effective setup is to pair them: hand coding and large projects to Opus 5.5, which is exactly how I built my mobile app (I wrote up the process in Same Claude Opus 5.5, so why do some people actually ship an app with it), and give high-volume writing cleanup, screenshot recognition and similar work to a cheap, fast model like Terra.

Models and pricing change quickly, so before you integrate for real, check the latest information in OpenAI's and Anthropic's official documentation.

References:

If you're also working through model selection, or evaluating a migration from GPT-5.6 to GPT-6.1, I'd love to compare notes: linktr.ee/weiyii

Cover: NASA / Apollo 17 crew, Public domain.

Originally published at ay3s.com.

## Top comments (0)
