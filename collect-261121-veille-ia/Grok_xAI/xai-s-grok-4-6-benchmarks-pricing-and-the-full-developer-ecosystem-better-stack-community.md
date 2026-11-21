---
id: collect-261121-veille-ia/grok-xai/xai-s-grok-4-6-benchmarks-pricing-and-the-full-developer-ecosystem-better-stack-community
title: "xAI's Grok 4.6: Benchmarks, Pricing, and the Full Developer Ecosystem | Better Stack Community"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["grok", "model", "more", "reasoning", "pricing", "benchmark", "coding", "tokens"]
source: https://betterstack.com/community/guides/ai/xai-grok-46/
source_anchor: ""
source_lines: [1, 77]
section: "Grok_xAI"
---
xAI (now operating as SpaceXAI after SpaceX's February 2026 acquisition) released Grok 4.6 on August 12, 2026, 35 days after Grok 4.5. **The model is pitched as a refinement of the same base with more aggressive post-training focused on long-running agents and coding**. Pricing is unchanged at $2 per million input tokens and $6 per million output tokens. At that rate, it's the cheapest model currently competing at the intelligence frontier.

This article covers the model's benchmark position, where its numbers hold up and where they need reading carefully, and the developer tooling around it: Grok Build, Grok Bot, Cursor, and the Voice Agent Builder.

## Grok 4.6's position in the field

On the Artificial Analysis Intelligence Index, a third-party composite of nine benchmarks covering reasoning, coding, tool use, and knowledge, Grok 4.6 scores 61. That ties it with GPT-5.6 Sol Max and places it sixth overall, one point behind Claude Fable 5 (62) and two behind Claude Opus 5 (63).

The composite score is an average, and averages smooth over the interesting parts. Reading xAI's own ten-row eval table more carefully: Claude Fable 5 Max wins the most individual rows, Grok 4.6 wins knowledge work and legal reasoning, and it loses Terminal-Bench v3.0 by roughly 8.6 points to GPT-5.6 Sol, a weakness the launch announcement didn't emphasize. The model beats Grok 4.5 on every row, often by wide margins, so the generational improvement is real. Against the current frontier, it wins four rows, ties one, and loses five.

xAI has no published architecture details for 4.6, no parameter count, no MoE disclosure, and no system card. Reports that it uses the same 1.5-trillion-parameter base as 4.5 with longer post-training come from third-party analysis, not vendor statements. The confirmed improvements are in post-training: upgraded supervised fine-tuning and reinforcement learning using xAI's own Grok Build coding harness.

The new `xhigh` reasoning effort level was added in 4.6. Grok 4.5 silently ignored this setting; 4.6 acts on it. The previous `high` setting remains the default. There is no "Grok 4.6 High" model as a separate SKU, though you'll see that label on some benchmark sites. It refers to the reasoning effort setting.

Context window is 500,000 tokens. A fast variant is available at twice the base price. Grok 4.7 (2.1 trillion parameters) is reportedly expected within weeks of the 4.6 release, with Grok 5 targeted before end of 2026.

## Real-world task performance

On the GDPval-AA v2 benchmark, which evaluates model performance on complex, multi-step real-world tasks rather than abstract reasoning:

Grok 4.6 jumps from 12th place (Grok 4.5) to second. On Terminal-Bench v2.1, the benchmark measuring terminal-based software engineering tasks:

Second place again. Note the version distinction: xAI publishes 26% on Terminal-Bench v3.0 (a substantially harder benchmark), while Artificial Analysis reports 88.39% on v2.1. These are not comparable numbers, and v3.0 is the more current test.

Artificial Analysis also measured a result xAI didn't publish: a 65.7% non-hallucination rate. For customer-facing applications, that matters more than most coding scores. On efficiency, Grok 4.6 completed long agentic tasks in roughly 53 turns at $0.84 per task, compared to Claude Opus 5's 103 turns for comparable work.

## Intelligence vs. cost

The price-to-intelligence argument is where Grok 4.6's case is strongest.

At $2/$6 per million tokens, Grok 4.6 is roughly 60% cheaper than GPT-5.6 Sol ($5/$30) at equivalent intelligence scores. The scatter plot positions it in the high-intelligence, low-cost quadrant. The per-task cost is slightly higher than Grok 4.5 despite identical token pricing, which indicates that more reasoning steps are being consumed per task to achieve the higher output quality. That's a reasonable trade-off: more capable answers at a modest increase in effective cost, still well below the alternatives.

One note: the "same API price as 4.5" claim applies to the base token rate. Grok 4.6's tendency to use more internal thinking steps means some workloads will see a higher invoice even without a rate change.

## Grok Imagine: image and video generation

The Grok Imagine API offers both image generation and video generation. According to xAI's own benchmarks, it leads on quality, speed, and price among current video generation APIs.

Video generation is priced at $0.07 per second of generated video with latency around 46 seconds. These are vendor-reported figures; the competitive positioning against Veo and Sora should be verified against current benchmarks as the space moves fast.

The more distinctive feature is granular in-image editing. After generating an image, Grok Imagine breaks it into labeled segments and lets you select and modify individual elements without regenerating the full image.

You click an element in the image or in the segments panel, type a modification prompt for just that element, and the model regenerates only that portion while keeping the rest intact. For iterative creative work where you want to adjust one component at a time, this removes the trial-and-error cycle of regenerating the full image for each change.

## The developer tooling stack

xAI currently offers three main channels for agentic development work:

**Grok Build** is a CLI tool for terminal-based scripting, system tasks, and quick code generation. It's the closest analog to Claude Code or early Codex CLI.

**Grok Bot** has broader system access: it can execute commands, browse the web, and interact with applications. It's designed for automating multi-step workflows that span multiple tools.

**Cursor** is xAI's most significant developer distribution channel after the acquisition. It's a fork of VS Code with deep AI integration, and Grok 4.6 is now the primary model in the Cursor lineup.

### Cursor and the model routing

Within Cursor, two usage pools determine billing:

The Cursor Models pool includes significantly more included usage for Grok 4.6, Grok 4.5, and Cursor's own Composer 2.5 fine-tuned model. Third-party models are billed from the Other Models pool at standard API rates. This structure incentivizes using Grok models within Cursor without blocking access to alternatives.

The Cursor Router in Auto mode analyzes requests and routes to the most appropriate model: lighter models for simpler tasks, Grok 4.6 for complex or frontier-level requests. CursorBench 3.2 scores: Grok 4.6 at 69.9%, up from Grok 4.5's 66.7%, just behind Fable 5 Max at 70.5% and slightly ahead of GPT-5.6 Sol at 67.2%.

## Grok Voice Agent Builder

The Voice Agent Builder lets you create real-time conversational voice agents with tool access, low latency, and full interruptibility.

Key capabilities: connect agents to external tools and APIs (web search, Gmail, custom databases); deploy within an application or connected to a phone number for direct user calls; configure persona, objective, and conversation flow through a structured UI.

The configuration screen covers role and persona definition, objective statement, and conversation flow instructions. Pre-built templates include Customer Support and Appointment Scheduler. The "Try it live" button provides a text and voice test interface before deployment.

## What to keep in mind

The source article's description of Grok 4.6 as "fourth place" on the Intelligence Index is overstated. The current verified position is sixth, tied with GPT-5.6 Sol. The benchmark improvements are real and significant over Grok 4.5, but the launch framing emphasized wins and underemphasized losses, particularly on Terminal-Bench v3.0.

The model is available through the xAI API, Grok Build, Cursor, OpenRouter, Vercel, and Cloudflare. With Grok 4.7 reportedly close and Grok 5 targeting end of 2026, developers building infrastructure around a specific model version should use the API endpoint rather than pinning to a model ID that may be superseded quickly.

For developers already in the Cursor ecosystem, the integration with Grok 4.6 is the most direct path to evaluating the model in a real workflow. The price-to-intelligence positioning makes it worth testing for high-volume agentic tasks where the cost difference over Claude Opus 5 or GPT-5.6 Sol adds up materially.
