---
id: collect-261121-veille-ia/qwen/qwen4-max-by-alibaba-qwen-specs-benchmarks-pricing
title: "Qwen4-Max by Alibaba (Qwen): Specs, Benchmarks & Pricing"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["qwen4-max", "tokens", "alibaba", "output", "model", "qwen", "thinking", "pricing"]
source: https://www.thelatentdaily.com/models/qwen4-max/
source_anchor: ""
source_lines: [1, 29]
section: "Qwen"
---
## Overview

Qwen4-Max is the first model of Alibabaâs Qwen4 generation and the new flagship of the Qwen family. Alibaba Cloud made it generally available on its Model Studio platform on November 16, 2026, after a preview that opened on November 11 for subscribers to its Token Plan. Alibaba describes it as a multimodal model that combines text generation, image and video understanding and deep reasoning, built for coding, office productivity, front-end design and complex agent tasks. Unlike several earlier Qwen releases, it is a hosted model only: Alibaba has not published its weights or its parameter count. Our news report on the Qwen4-Max release covers the launch in detail.

## Specifications

- **Model ID:**`qwen4-max` on Model Studio;`alibaba/qwen4-max` on Vercelâs AI Gateway.
- **Context window:** 1,000,000 tokens.
- **Output:** up to 131,072 tokens without thinking; thinking and the final answer share a combined 262,144-token limit.
- **Input and output:** text, images and video in; text out.
- **Reasoning:** thinking is on by default at`high` effort, with`low` and`xhigh` as alternatives and a default thinking budget of 131,072 tokens.
- **APIs:** OpenAI Chat Completions, OpenAI Responses, Anthropic Messages and DashScope.
- **Features:** streaming, web search, function and parallel tool calling, prefix completion, structured JSON output, explicit and implicit caching.
- **Regions:** Beijing, Singapore, Frankfurt, Virginia, Tokyo and Hong Kong; batch inference only in Beijing; no fine-tuning or custom deployment.

## Pricing

In the Singapore region, Qwen4-Max lists at $1.50 per million input tokens and $4.50 per million output tokens, 25% below Qwen3.8-Max ($2 and $6). Cache reads cost $0.075 per million tokens, while explicit cache writes cost $1.875 (five-minute lifetime) or $3 (one hour). New Singapore users get a free quota of 1 million tokens for 90 days. Beijing and Hong Kong list $1.413 and $4.24.

## Typical uses

Coding agents, long-document and multi-file analysis, office documents and reports, and agent workflows that call many tools, especially for teams that want OpenAI- or Anthropic-compatible endpoints at lower output prices.

## Things to consider

- No open weights and no fine-tuning; for self-hosting, look at the open Qwen3.8 family instead.
- Alibabaâs quality claims were not yet confirmed by independent leaderboards at launch.
- Reasoning tokens count toward the output limit and are billed as output, so long thinking raises costs.
- The Token Plan subscription is limited to interactive tools and bans automation scripts and application backends.
