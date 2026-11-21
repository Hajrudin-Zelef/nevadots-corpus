---
id: collect-261121-veille-ia/deepseek/deepseek-v4-1-flash-1017-what-deepseek-says-changed-and-swapping-it-in-without-breaking-anyone-s-sav
title: "DeepSeek V4.1 Flash 1017: what DeepSeek says changed, and swapping it in without breaking anyone's saved model"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-10-17"]
keywords: ["flash", "deepseek", "model", "catalog", "tools", "saved", "cloud", "says"]
source: https://dev.to/purpledoubled/deepseek-v41-flash-1017-what-deepseek-says-changed-and-swapping-it-in-without-breaking-anyones-18ci
source_anchor: ""
source_lines: [1, 44]
section: "DeepSeek"
---
Locally Uncensored is an open source desktop app (AGPL-3.0) that runs AI models on your own machine. It also has an optional cloud mode, LU Cloud, for models too big for a desk. DeepSeek V4.1 Flash is one of those: the checkpoint is 510.3 GB.

On 17 October 2026 DeepSeek published V4.1 Flash 1017. Since 18 October, 21:54 UTC, it is the model behind our V4.1 Flash entry. This post covers what DeepSeek claims for it and how we moved people over.

DeepSeek calls 1017 a minor update to V4.1 Flash. The release note says it scores higher on benchmarks, finishes tasks faster and writes shorter outputs. The model card has a table comparing it with V4.1 Flash. These are DeepSeek's numbers, run by DeepSeek:

| Benchmark | V4.1 Flash | V4.1 Flash 1017 | 
|---|---|---|
| HLE without tools | 36.8 | 42.6 | 
| Terminal-Bench 4.0 | 31.2 | 42.3 | 
| DeepSWE v1.1 | 74.2 | 77.0 | 
| ProgramBench | 20.3 | 29.1 | 
| AutomationBench | 54.8 | 60.5 | 
| Agents' Last Exam | 31.8 | 35.4 | 
| Apex Bench v1.1 | 64.6 | 72.1 | 
| Chartography, with tools | 78.9 | 83.0 | 
| BabyVision, with tools | 89.6 | 92.5 | 
| ZeroBench, with tools | 49.0 | 51.0 | 

The biggest jumps are on the long agent tests, Terminal-Bench 4.0 and ProgramBench. The image tests move by two to four points. I have not measured output length myself yet.

The checkpoint is the same size as before, under the same MIT licence. If you call it yourself, the card now recommends `temperature` 1.0 and `top_p` 1.0.

Sources: the release note and the model card.

People had V4.1 Flash saved as a default, in old chats and in API scripts that send `deepseek-ai/DeepSeek-V4.1-Flash`. Renaming the entry would have broken all of them.

So the catalog entry now carries the new id, `deepseek-ai/DeepSeek-V4.1-Flash-1017`, and the old id sits in a small table of retired ids that points to its replacement. Every request goes through that lookup before it reaches the model, and the meter prices the replacement. Old chats keep working, API scripts keep working, and nobody has to touch a setting.

Before the switch I ran both versions side by side: a native tool call, an image to describe, and the same reasoning prompts at different effort levels. They behaved the same way, so the flags in the catalog (image input, tools, reasoning always on) carried over unchanged.

The catalog price went down with the switch. 1017 draws 20,000 credits per million input tokens and 60,000 per million output tokens. Before the switch the catalog listed V4.1 Flash at 30,000 and 120,000. Every plan and every credit pack carries the whole chat catalog.

In the desktop app, flip the Cloud switch and pick DeepSeek V4.1 Flash 1017 in Chat. The app pulls the catalog from the server, so no update is needed. From code, any OpenAI-compatible client works against `https://lu-labs.ai/api/inference/v1` with the model id `deepseek-ai/DeepSeek-V4.1-Flash-1017` and a personal API key from account settings.

- 1017 replaced V4.1 Flash in LU Cloud on 18 October; DeepSeek calls it a minor update with higher benchmark scores and shorter outputs.
- The old model id still works and is routed to 1017, so saved picks, old chats and API scripts keep running.
- It got cheaper: 20,000 credits per million input tokens and 60,000 per million output tokens.

The app itself is on GitHub. The full post with the local hardware math links is on locallyuncensored.com.

*AI-assisted: I wrote this with help from an AI model and checked the facts against the release note, the model card and our catalog.*

## Top comments (0)
