---
id: collect-261121-veille-ia/muse/meta-launches-muse-ember-its-smallest-model-at-0-10-per-1m-tokens
title: "Meta launches Muse Ember, its smallest model, at $0.10 per 1M tokens"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: ["2026-11-09"]
keywords: ["muse", "model", "meta", "tokens", "million", "ember", "claude", "launches"]
source: https://codingislove.com/meta-muse-ember/
source_anchor: ""
source_lines: [1, 44]
section: "Muse"
---
# Meta launches Muse Ember, its smallest model, at $0.10 per 1M tokens

Meta released Muse Ember on November 9, 2026, calling it its smallest and most efficient model. It is built for agent work such as tool calls, coding subtasks, routing, extraction and long-document parsing, and it is available now through Muse Code and the Meta Model API. Pricing starts at $0.10 per million input tokens and $0.50 per million output tokens for prompts up to 200K tokens.

## What’s new

According to Meta’s announcement, the model ID is `muse-ember-1.0`. Key facts from the page:

- **Context:** a 1M-token context window and up to 512K output tokens.
- **Modalities:** text, images, audio and video natively.
- **Training:** the first model trained using Muse Wonder Full.
- **Size:** Meta gives no parameter count, and the page does not state a license.
- **API:** it supports the Responses API.
- **Cloud:** it is also available on Google Cloud and Oracle Cloud, alongside Muse Spark and Muse Wonder.

### Pricing

| Prompt length | Input | Cached input | Output | 
|---|---|---|---|
| Up to 200K tokens | $0.10 per million | $0.004 per million | $0.50 per million | 
| Longer prompts | $0.30 per million | $0.012 per million | $1.50 per million | 

## Benchmarks Meta published

These are Meta’s own figures at maximum reasoning, taken from the announcement. They have not been independently verified here.

- **Artificial Analysis Intelligence Index:** 36, against 37 for Muse Spark 1.3. Meta says the cost per Artificial Analysis task is about $0.14, versus $2.39 for Muse Spark 1.3.
- **Terminal-Bench 4.0:** 40.9%, against 39.2% for Claude Haiku 5.5 and 16.4% for GPT-6 Luna.
- **Coding Agent Index-AA:** 46, against 37 for Claude Haiku 5.5 and 31 for GPT-6 Luna.
- **AutomationBench-AA:** 37.8%, against 20.2% for GPT-6 Luna and 13.9% for Claude Haiku 5.5.
- **FrontierCode 2.0-AA:** 27.2%, where Claude Haiku 5.5 scores higher at 34.9% and GPT-6 Luna scores 25.2%.

Meta also quotes partners. Cloudflare’s CTO Dane Knecht said Muse Ember “ran up to 30% faster at the tail than the comparable model we tested”. These are vendor-selected results, so test on your own tasks.

## What it means for developers

- **Cheap agent steps:** at $0.10 per million input tokens, Muse Ember is meant for the high-volume steps in an agent loop: routing, extraction and tool calls, where a larger model is overkill.
- **Long documents:** the 1M-token window and low cached-input price suit repeated queries over the same large document or codebase.
- **Integration:** because it supports the Responses API, trying it can be a base-URL and model-name change if your code already uses that format. Check Meta’s API docs for exact endpoints and rate limits, which the announcement does not list.
- **Unknowns:** no stated license and no parameter count, so do not assume it can be self-hosted.

## How it compares

On Meta’s numbers, Muse Ember trades a little accuracy for a large drop in cost: one point below Muse Spark 1.3 on the Intelligence Index at roughly 6% of the per-task cost. Against Claude Haiku 5.5 it leads on most agent benchmarks listed but trails on FrontierCode 2.0-AA. For other recent model launches, see our posts on Claude Fable 5.2 and Mythos 5.2 and Gemini Omni 1.2 Flash.
