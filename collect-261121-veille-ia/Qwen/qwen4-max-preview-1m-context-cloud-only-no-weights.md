---
id: collect-261121-veille-ia/qwen/qwen4-max-preview-1m-context-cloud-only-no-weights
title: "Qwen4-Max Preview: 1M Context, Cloud Only, No Weights"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["model", "alibaba", "weights", "preview", "qwen4-max", "qwen", "november", "cloud"]
source: https://modelfit.io/blog/qwen4-max-preview-cloud-only-no-weights/
source_anchor: ""
source_lines: [1, 105]
section: "Qwen"
---
Alibaba opened a preview of Qwen4-Max on 11 November 2026, the first model of its next-generation Qwen family. It reads text, images and video and takes up to 1M tokens of context (Alibaba Cloud Model Studio, 2026). Short answer for local users: you cannot run it. It is a cloud model, Alibaba has published no weights, and the preview itself ends on 25 November.

**Update, 16 November 2026:** Qwen4-Max is now generally available as `qwen4-max`, with a per-token price and a published size of 2.4T parameters. The weights are still closed. See Qwen4-Max general availability.

## What did Alibaba confirm?

The Model Studio page is the only official specification. It describes Qwen4-Max-Preview as "the next-generation flagship multimodal model in the Qwen family", built for coding, office work, frontend design and agentic tasks.

| Spec | Value (Alibaba Cloud) | 
|---|---|
| Model ID | `qwen4-max-preview` | 
| Input | Text, images and video | 
| Context length | 1M tokens | 
| Maximum output | 393,216 tokens, shared by thinking and the final answer | 
| Thinking | On by default, `reasoning_effort` set to`low` ,`medium` or`xhigh` (default) | 
| Tools | Function Calling, web search, built-in tools, JSON Object and JSON Schema outputs | 
| API compatibility | OpenAI Chat Completions, Responses, and Anthropic Messages | 
| Access | Token Plan only, pay-as-you-go not supported | 
| Preview window | 10:00 on 11 November 2026 to 24:00 on 25 November 2026 (UTC+8), then retired | 

## Get told when a better model fits your MacBook Air M5 16 GB

One email when a new open-weight model is a better fit for your machine, plus the Thursday weekly.

For: **MacBook Air M5 16 GB**

You'll also get the weekly ModelFit email. By signing up you agree to our Privacy Policy. Unsubscribe from either email anytime, in one click.

One API detail can break existing code. Starting with this model, `max_tokens` counts thinking and the answer together, so a limit sized for the answer alone may cut the output short.

Outside the API, TechNode reports early access in the Qwen app, Qoder, QwenWork and QwenStudio (TechNode, 2026).

## What is still missing?

The list is longer than the spec table:

- **No weights.** Nothing named Qwen4 exists in the Qwen organisation on Hugging Face as of 11 November 2026, per the Hugging Face API.
- **No parameter count.** Neither the total nor the activated count is published, so memory and speed cannot be estimated.
- **No API price.** Access runs through a Token Plan subscription; Alibaba lists no per-token rate for this model.
- **No benchmarks.** Alibaba has published no score table, and we found no independent result. ModelFit will not rank a model it cannot verify.
- **No license and no technical report.**

Reports about a wider launch date and about other Qwen4 sizes circulate. Alibaba has not confirmed them, so we leave them out until it does.

## Can a 512GB Mac Studio run Qwen4-Max?

No, and there is no distance to measure. A Mac Studio with 512GB is the largest single machine we track, but a model with no published weights and no published parameter count cannot be loaded or even sized. Qwen4-Max-Preview runs only on Alibaba's servers, and the preview is retired after 25 November.

For comparison, the previous flagship sits in the same place. Qwen3.8-Max was previewed in July as a 2.4 trillion parameter model with open weights promised and no date, and our catalog still lists it as cloud-only.

## What do we know about open-weight Qwen4?

Only two official statements, and neither is a release:

1. At its Apsara Conference in September, Alibaba said "its next-generation model, Qwen 4, is currently in training", with Qwen 4.5 and Qwen 5 "projected to scale up to 5 to 10 trillion parameters" (Alibaba Cloud, 2026). It gave no date, size or license for open weights.

2. The model card of Qwen3.8-Flash-Next calls it an "experimental preview of the architecture that will underpin Qwen4" (Qwen/Qwen3.8-Flash-Next, 2026). That one has open weights today.

So the closest thing to a local Qwen4 is Qwen3.8-Flash-Next, which runs fully in memory from a 128GB Mac, and with SSD streaming on smaller ones (Strata on a Mac). When Qwen4 weights land, the first lines to read are the total and activated parameter counts: the first sets your memory bill, the second your speed.

## What should you run instead today?

A Qwen model that fits your machine. Qwen3.8-27B is the dense open-weight flagship of the current generation and runs on 24GB-class hardware (how to run it). Our Qwen models page lists every size in GB and the best Qwen build for each amount of RAM. For a pick matched to your hardware, use the ModelFit wizard or `npx @modelfit/cli@latest`.

## FAQ

### Can you run Qwen4-Max locally?

No. Qwen4-Max-Preview is available only through Alibaba's Token Plan and Qwen apps. Alibaba has published no weights and no parameter count, so no Mac or GPU can run it.

### How long is the Qwen4-Max context window?

1M tokens, with a maximum output of 393,216 tokens shared by thinking and the final answer, per Alibaba Cloud's Model Studio page.

### How much does the Qwen4-Max API cost?

Alibaba has not published a per-token price. During the preview the model is available through Token Plan only, and pay-as-you-go is not supported.

### When does the Qwen4-Max preview end?

At 24:00 on 25 November 2026 (UTC+8). Alibaba says the model will not change during the preview and will be retired after it ends.

### Will Qwen4 have open weights?

Alibaba has not said. The only official signals are that Qwen 4 is in training and that Qwen3.8-Flash-Next previews the architecture Qwen4 will use. We will add Qwen4 to the open dataset when weights ship with support in llama.cpp or mlx-lm.

## Sources

- Alibaba Cloud Model Studio, Qwen4-Max-Preview: https://www.alibabacloud.com/help/en/model-studio/qwen4-max-preview
- TechNode, Alibaba opens Qwen4-Max preview with a million-token context window: https://technode.com/2026/11/11/alibaba-opens-qwen4-max-preview-with-a-million-token-context-window/
- Alibaba Cloud, 2026 Apsara Conference roadmap: https://www.alibabacloud.com/blog/alibaba-clouds-2026-apsara-conference-full-stack-ai-roadmap-along-with-global-market-expansion-plan_603598
- Qwen3.8-Flash-Next model card: https://huggingface.co/Qwen/Qwen3.8-Flash-Next
- Hugging Face API, Qwen organisation models, read 11 November 2026: https://huggingface.co/api/models?author=Qwen&search=Qwen4

## Don't miss the next one for your MacBook Air M5 16 GB

This article covers one moment. Get one email when a newer open-weight model fits your machine better, plus the Thursday weekly with what changed.

For: **MacBook Air M5 16 GB**

You'll also get the weekly ModelFit email. By signing up you agree to our Privacy Policy. Unsubscribe from either email anytime, in one click.

Match this model to a machine that can run it: by RAM tier for Apple Silicon, or by VRAM for an NVIDIA GPU.

Have questions? Reach out on X/Twitter
