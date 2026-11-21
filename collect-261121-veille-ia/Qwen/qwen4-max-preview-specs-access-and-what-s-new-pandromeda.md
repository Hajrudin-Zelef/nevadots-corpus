---
id: collect-261121-veille-ia/qwen/qwen4-max-preview-specs-access-and-what-s-new-pandromeda
title: "Qwen4-Max Preview: Specs, Access and What's New | Pandromeda"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["alibaba", "tokens", "preview", "qwen4-max", "model", "output", "context", "window"]
source: https://pandromeda.com/ai/qwen4-max-preview-what-it-is-how-to-access-it-and-whats-new
source_anchor: ""
source_lines: [1, 139]
section: "Qwen"
---
# Qwen4-Max Preview: What It Is, How to Access It and What's New

Alibaba Cloud has opened a time-limited preview of Qwen4-Max, its next flagship model, with a 1-million-token context window and Token Plan-only access through November 25, 2026.

**Qwen4-Max-Preview is a time-limited preview of Alibaba's next flagship AI model, opened through Alibaba Cloud's Model Studio on November 11, 2026.** According to Alibaba Cloud's own documentation, it reads a 1-million-token context window, can output up to 393,216 tokens split between its internal reasoning and its final answer, and accepts text, image and video input. There is a catch: it is not sold through Model Studio's normal pay-as-you-go API. The preview is reachable only with a Token Plan subscription, and Alibaba says it will retire the preview entirely at 24:00 Beijing time (UTC+8) on November 25, 2026.

**Qwen4-Max-Preview at a glance**

- **Context window:** 1,000,000 tokens
- **Max output:** 393,216 tokens (shared by thinking and the answer)
- **Input types:** text, images, video
- **Access:** Token Plan subscription only — no pay-per-token API
- **Preview window:** November 11-25, 2026 (UTC+8), then retired
- **Published benchmarks or parameter count:** none, as of this writing

## What is Qwen4-Max-Preview?

Qwen4-Max-Preview is the early-access version of Qwen4-Max, the latest entry in Alibaba's Qwen line of hosted flagship models. It sits inside Alibaba Cloud Model Studio, the company's managed platform for serving Qwen and other models through an API. Alibaba's own documentation describes it as a "next-generation flagship" multimodal model built for text generation, deep reasoning, image understanding and video understanding, with thinking enabled by default and a model ID of `qwen4-max-preview`.

What makes this confusing for anyone searching "Qwen4-Max" right now is that Alibaba actually has two things live at once. There's a standard, already-shipping Qwen4-Max that's billed per token through the regular Model Studio API. And there's this preview, Qwen4-Max-Preview, which is a separate, higher-limit build that Alibaba is only renting out through a subscription, for two weeks, before switching it off. Alibaba's own documentation for the preview page states plainly: "The production version, Qwen4-Max, is available and recommended for use" — in other words, Alibaba itself is pointing production users toward the non-preview model and treating this preview as a look at something newer that hasn't fully replaced it yet.

## Context window, output limit and input types

The specs that matter most for developers are the context window and the output cap, and Alibaba's documentation is specific about both. Qwen4-Max-Preview reads up to 1,000,000 tokens of context. Its maximum output is 393,216 tokens, but — and this is a detail easy to miss — that ceiling is *shared* between the model's internal "thinking" tokens and the final answer it returns. A request that burns through a lot of reasoning tokens before answering can get truncated if the combined total crosses that line, so Alibaba's docs warn that an output limit sized only for the expected answer length may cut off a response. Input is multimodal: text, images and video, which Alibaba markets for screenshot analysis, document understanding and video analysis. Output is text only.

That 393,216-token output ceiling is also where the preview pulls ahead of the already-shipping Qwen4-Max. The non-preview model caps combined thinking-plus-answer output at 262,144 tokens, and its context window, while still nominally 1,000,000 tokens, has a slightly smaller usable input of 983,616 tokens in thinking mode. The table below lines up what Alibaba's documentation states for each.

| Spec | Qwen4-Max-Preview | Qwen4-Max (standard) | 
|---|---|---|
| Context window | 1,000,000 tokens | 1,000,000 tokens | 
| Max output (thinking + answer) | 393,216 tokens | 262,144 tokens | 
| Max output, non-thinking mode | Not separately listed | 131,072 tokens | 
| Input types | Text, image, video | Text, image, video | 
| Access | Token Plan subscription only | Pay-as-you-go API (plus other options) | 
| Pricing (per 1M tokens, Singapore region) | Not published — bundled into subscription | $1.50 input / $4.50 output | 

Those standard-model prices, and the free 90-day quota of 1 million tokens that comes with it, are published on Alibaba Cloud's own Qwen4-Max documentation page. Alibaba has not published a comparable per-token price for the preview, because the preview isn't sold per token at all — more on that below.

## How to access Qwen4-Max-Preview

This is the part that trips people up: Qwen4-Max-Preview cannot be called with a normal Model Studio API key and a per-token bill. Alibaba's documentation is explicit that pay-as-you-go access is not supported for this model. The only route in is a Token Plan subscription — either the Personal or the Team tier — each of which gets its own dedicated API key and a separate base URL (`https://token-plan.ap-southeast-1.maas.aliyuncs.com`) rather than the standard Model Studio endpoint.

Once you're on a Token Plan, the model behaves like other recent Qwen releases on Model Studio: it supports tool calling (including function calling, web search and built-in tools), structured output in JSON Object and JSON Schema formats, and API compatibility with the OpenAI Chat Completions and Responses formats as well as the Anthropic Messages format. Developers can tune how hard it thinks with a `reasoning_effort` parameter set to low, medium or xhigh (xhigh is the default), though that can't be combined with a separate thinking-budget parameter. One quirk worth flagging for anyone porting code from OpenAI's Chat Completions API: on Qwen4-Max-Preview, `max_tokens` and `max_completion_tokens` are treated as the same value, and both cap the combined thinking-plus-answer total — not just the visible reply.

According to Alibaba Cloud's documentation, multi-turn conversations in Chat Completions mode preserve thinking by default, and historical reasoning is meant to come back in a separate `reasoning_content` field rather than getting folded into the visible `content` of past turns.

## The preview window: November 11-25, then retirement

Alibaba has put a hard clock on this one. The Model Studio documentation dates the preview from 10:00 on November 11, 2026 to 24:00 on November 25, 2026, both Beijing time (UTC+8) — a run of just under two weeks. Alibaba's own language is that the model "will be retired after the preview ends," which, going by how Alibaba has handled earlier preview models, typically means the model ID stops responding and its documentation page disappears or redirects. When this desk checked Alibaba Cloud's help page for the prior preview model, Qwen3-Max-Preview, it had already gone to a 404 — consistent with Alibaba's pattern of pulling preview docs once a model is retired.

That timeline matters for anyone building against the preview: code written against `qwen4-max-preview` and the Token Plan base URL is not meant to run in production past November 25, 2026. Alibaba's guidance points toward the standard, non-preview Qwen4-Max for anything that needs to keep working after that date.

## How it compares with Qwen3-Max-Preview

Qwen4-Max-Preview is the successor to Qwen3-Max-Preview, the flagship preview Alibaba ran in 2025. Qwen3-Max was a closed-weight, API-only model reported at over one trillion parameters using a mixture-of-experts design, with most reporting pointing to a 262,144-token context window split into roughly 258,048 input tokens and 32,768 output tokens. Alibaba itself never published an official parameter count for Qwen3-Max either — that trillion-plus figure comes from outside reporting, not an Alibaba spec sheet, and the same caution applies to Qwen4-Max-Preview now.

What is directly comparable, because Alibaba states it for both generations, is the jump in headline context and output: Qwen3-Max-Preview's roughly 258K-token input ceiling and 32,768-token output cap are dwarfed by Qwen4-Max-Preview's 1-million-token context and 393,216-token combined output budget. Alibaba has not published a model card, benchmark table or open weights for either the Qwen4-Max-Preview or the standard Qwen4-Max, so there is no official head-to-head score to cite — only the published limits above.

It's also worth separating this closed, hosted "Max" line from Alibaba's open-weight Qwen releases. According to Wikipedia's entry on Qwen, Alibaba's most recent open-weight flagship is Qwen3.8, a 2.4-trillion-parameter model released with open weights (with revenue-sharing terms for large commercial users and a more permissive Apache license for a smaller, distilled 27B variant). Qwen4-Max and Qwen4-Max-Preview are not part of that open-weight line — they are closed, API- and subscription-only models, the same way Qwen3-Max-Preview was.

## Where the preview shows up besides the API

Alibaba isn't only offering Qwen4-Max-Preview to developers calling an endpoint. According to secondary reporting on the launch, Alibaba has also rolled early access into its own consumer and productivity apps — including the Qwen app, the coding tool Qoder, the enterprise suite QwenWork, and QwenStudio. Alibaba separately runs an enterprise office product called Qwen Office, built from several of its agent products and aimed at workplace tasks, though it isn't clear from available reporting whether that product and the "QwenWork" integration described in the coverage above are the same thing or two separate efforts under similar names. Readers using any of these apps should expect the model switch to be temporary, tracking the same November 25 retirement date as the API preview.

Secondary reporting has also described limited-time discounts on Token Plan pricing during the preview window, with figures that vary from outlet to outlet and that this desk could not confirm on Alibaba's own pricing pages. Given the inconsistency, treat any specific discount percentage circulating online as unconfirmed until Alibaba's own Token Plan documentation states it directly.

## What Alibaba hasn't said

For a model this size, the list of unpublished details is almost as notable as the published specs. As of this writing, Alibaba has not released, for either Qwen4-Max-Preview or the standard Qwen4-Max:

- A parameter count
- A benchmark table or model card
- Open weights or a license for self-hosting
- A per-token price for the preview specifically (only the standard model is priced per token)

Any number you see online for Qwen4-Max's parameter count or benchmark scores is, at best, someone's estimate or a leak — not a figure Alibaba has put its own name to. Readers comparing it against other frontier models, including Alibaba's own prior Qwen3-Max-Preview, should treat those comparisons as informed guesswork until Alibaba publishes its own technical report.

## Background: Alibaba's Qwen family

Qwen (also known as Tongyi Qianwen) is Alibaba Group's family of AI models, spanning everything from small open-weight models released on Alibaba's QwenLM GitHub organization to the large, closed "Max" models served only through Model Studio's API. The open side of that family has released more than 100 model weights that have collectively been downloaded tens of millions of times, per Wikipedia's entry on the project. The "Max" line, including Qwen3-Max-Preview and now Qwen4-Max-Preview, is Alibaba's separate, closed-weight flagship track — bigger, API-only, and generally the first place Alibaba's newest reasoning and multimodal capabilities show up before (if ever) a smaller version is released with open weights. For more on how one of Alibaba's recent open releases compares with a rival Chinese lab's model, see Pandromeda's look at DeepSeek V4.1 Flash versus Qwen3.8-Max.

## Frequently asked questions

### Is Qwen4-Max-Preview available through the normal Model Studio API?

No. Alibaba's documentation states that pay-as-you-go access is not supported for the preview; it is reachable only through a Token Plan subscription, using a separate API key and base URL.

### When does the Qwen4-Max-Preview window close?

Alibaba's documentation sets the preview from 10:00 on November 11, 2026 to 24:00 on November 25, 2026, Beijing time (UTC+8), after which the model is retired.

### What's the difference between Qwen4-Max-Preview and the standard Qwen4-Max?

Both read a 1-million-token context window, per Alibaba's documentation. The preview can output up to 393,216 tokens of combined thinking and answer; the standard model caps that combined total at 262,144 tokens (131,072 in non-thinking mode). The standard model is billed per token through the regular API; the preview is subscription-only.

### Has Alibaba published benchmarks or a parameter count for Qwen4-Max?

No, not as of this writing, for either the preview or the standard version. Any benchmark scores or parameter figures circulating online are not Alibaba's own published numbers.

### Will Qwen4-Max-Preview get open weights?

Alibaba has not said. Its closed "Max" line, including the prior Qwen3-Max-Preview, has historically stayed closed-weight and API-only, separate from Alibaba's open-weight Qwen releases such as Qwen3.8.

## What to watch next

The clearest next milestone is the November 25, 2026 cutoff itself: watch whether Alibaba quietly folds the preview's higher 393,216-token output limit into the standard, pay-as-you-go Qwen4-Max once the preview retires, the way it has partially done with past preview-to-production transitions. Also worth tracking is whether Alibaba ever publishes a technical report, benchmark table or parameter count for the Qwen4-Max line — something it still hasn't done for Qwen3-Max-Preview over a year after that model shipped. Developers currently testing against the preview's Token Plan endpoint should treat any production code as temporary and plan a path to the standard Qwen4-Max API before the retirement date. For context on where Alibaba's open-weight models stand relative to rivals, Pandromeda's comparison of DeepSeek V4.1 Flash and Qwen3.8-Max and its coverage of Alibaba's Qwen-Audio price cuts are good next reads.

## Frequently asked questions

### Is Qwen4-Max-Preview available through the normal Model Studio API?

No. Alibaba's documentation states that pay-as-you-go access is not supported for the preview; it is reachable only through a Token Plan subscription, using a separate API key and base URL.

### When does the Qwen4-Max-Preview window close?

Alibaba's documentation sets the preview from 10:00 on November 11, 2026 to 24:00 on November 25, 2026, Beijing time (UTC+8), after which the model is retired.

### What's the difference between Qwen4-Max-Preview and the standard Qwen4-Max?

Both read a 1-million-token context window, per Alibaba's documentation. The preview can output up to 393,216 tokens of combined thinking and answer; the standard model caps that combined total at 262,144 tokens (131,072 in non-thinking mode). The standard model is billed per token through the regular API; the preview is subscription-only.

### Has Alibaba published benchmarks or a parameter count for Qwen4-Max?

No, not as of this writing, for either the preview or the standard version. Any benchmark scores or parameter figures circulating online are not Alibaba's own published numbers.

### Will Qwen4-Max-Preview get open weights?

Alibaba has not said. Its closed 'Max' line, including the prior Qwen3-Max-Preview, has historically stayed closed-weight and API-only, separate from Alibaba's open-weight Qwen releases such as Qwen3.8.

## Sources

- Alibaba Cloud Model Studio: Qwen4-Max-Preview documentationalibabacloud.com
- Alibaba Cloud Model Studio: Qwen4-Max documentationalibabacloud.com
- Alibaba Cloud: Model Studio product pagealibabacloud.com
- Wikipedia: Qwenen.wikipedia.org
- Wikipedia: Alibaba Groupen.wikipedia.org
- GitHub: QwenLM organizationgithub.com

Theo Park runs the AI desk at Pandromeda. He follows model launches from the frontier labs and the open-weight community, tracks the assistants and developer tools built on them, and explains what each release changes on pricing, capability and safety. His reporting leans on primary sources: model cards, technical reports, API documentation and the companies' own announcements.
