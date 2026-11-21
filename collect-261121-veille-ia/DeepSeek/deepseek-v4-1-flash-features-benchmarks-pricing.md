---
id: collect-261121-veille-ia/deepseek/deepseek-v4-1-flash-features-benchmarks-pricing
title: "DeepSeek V4.1 Flash: Features, Benchmarks, Pricing"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-09-14"]
keywords: ["flash", "deepseek", "model", "input", "pricing", "features", "benchmarks", "weights"]
source: https://www.datacamp.com/blog/deepseek-v4-1-flash
source_anchor: ""
source_lines: [1, 372]
section: "DeepSeek"
---
DeepSeek V4.1 Flash: Features, Benchmarks, and Pricing

DeepSeek's new sparse MoE model ships MIT-licensed weights, native image input, and peak pricing of $0.30/$1.20 per 1M tokens, undercutting frontier rivals by a

The frontier model race in late 2026 has mostly been an arms race on price rather than raw capability. In September alone, we've seen OpenAI's GPT-6 Astra and Anthropic's Claude Fable 5.1 both sit at $10 per 1M input tokens and $50 per 1M output tokens, and neither is available on free tiers.

DeepSeek's answer is to go the other direction entirely.

DeepSeek V4.1 Flash is a 552B-parameter mixture-of-experts model that activates roughly 8B parameters for input and 16B for output, ships with MIT-licensed open weights, and costs $0.30/$1.20 per 1M tokens at peak, dropping to $0.15/$0.60 off-peak, beating other similar Flash models, such as Gemini 3.8 Flash and GLM-5.3-Flash.

In this article, I'll cover everything new with DeepSeek V4.1 Flash, looking at the new features and exploring the benchmarks. If you want to see the model in action, read our DeepSeek V4.1 API tutorial.

V4.1 Flash is DeepSeek's new open-weight flagship Flash model, phasing out V4 Pro 0813 on cost, speed, and performance.

MIT-licensed weights plus native image input make it the most capable open model in this price band right now.

Peak pricing of $0.30/$1.20 per 1M tokens sits roughly 30x below GPT-6 Astra and Claude Fable 5.1.

Off-peak pricing halves that again, but you need job scheduling to actually capture the discount.

Worth switching to if you run high-volume coding or reasoning workloads and can either self-host or tolerate peak/off-peak billing.

What Is DeepSeek V4.1 Flash?

DeepSeek V4.1 Flash is the newest model in DeepSeek's Flash line, positioned as the efficient, high-throughput tier rather than a research-grade heavyweight.

It is a sparse mixture-of-experts (MoE) architecture with 552B total parameters, of which only about 8B are active per token on input and 16B on output. That sparse activation is what keeps serving costs low while still reaching frontier-level scores on several benchmarks.

The headline change versus the previous generation is efficiency.

DeepSeek shrank the KV cache to roughly a quarter of the previous V4 Flash's size, which reduces the memory footprint and lets the model serve longer contexts more cheaply. The company also states that V4.1 Flash surpasses the older V4 Pro 0813 on performance, cost, speed, and total time, and V4 Pro is being phased out as a result.

On the benchmark front, the number that stands out is Terminal-Bench 2.1 at 90.6. When we tested the earlier DeepSeek-V4-Flash-0731, it scored 82.7 on the same benchmark, so V4.1 Flash is a real step up on agentic terminal work within the same family.

DeepSeek V4.1 Flash Key Features

V4.1 Flash's feature set is built around two ideas: run cheap, and stay open. Here are the capabilities that matter most for a working data scientist or ML engineer.

Open weights you can self-host

V4.1 Flash ships with MIT-licensed weights, which is the most permissive common license and allows commercial use, redistribution, and fine-tuning with no revenue thresholds.

For teams with data-privacy constraints, this means you can run the model entirely on your own hardware rather than sending requests to the DeepSeek API.

The trade-off is that the MIT license shifts responsibility for safety, fine-tuning, and inference optimization onto you.

When we ran the earlier V4-Flash-0731 locally, it needed three NVIDIA A100 80GB GPUs at Q8 quantization, so self-hosting is only viable when you have real GPU memory to spare.

Native image input

V4.1 Flash accepts images directly as input, without a separate vision adapter or preprocessing step. This lets you feed screenshots, charts, or document scans into the same request alongside text, which is useful for document-heavy pipelines and UI-agent workflows.

Native multimodal input at this price point is unusual. On the ZeroBench-main evaluation with tools, V4.1 Flash scores 49.0, which gives you a rough signal of how it handles reasoning over visual inputs.

Reduced KV cache for cheaper long context

The KV cache in V4.1 Flash is roughly a quarter the size of the previous V4 Flash, which lowers the memory cost of holding long contexts in a serving session.

For anyone running always-on inference, a smaller KV cache means either more concurrent sessions per GPU or lower per-request memory overhead.

Combined with cache-hit input priced at $0.006 per 1M tokens at peak, this makes context reuse extremely cheap. If you repeatedly send the same system prompt or document across requests, the cached portion costs almost nothing.

Peak and off-peak pricing modes

DeepSeek runs a two-tier pricing scheme in which off-peak prices are set at half of peak prices, with the stated intent of pushing batch and scheduled workloads into off-peak windows. That turns $0.30/$1.20 into $0.15/$0.60 per 1M tokens if you can time your jobs.

The caveat is operational. Always-on workloads become harder to cost-predict, and capturing the discount requires job-scheduling logic that you have to build and maintain yourself.

How Does DeepSeek V4.1 Flash Perform on the Benchmarks?

V4.1 Flash's strongest showing is on agentic and reasoning benchmarks, where it reaches frontier-adjacent scores at a fraction of the token cost.

Its scores drop sharply on the hardest agentic evaluations, which is worth knowing before you point it at long-horizon tasks.

Coding and agentic workflows

V4.1 Flash scores 90.6 on Terminal-Bench 2.1, which measures how well a model completes multi-step tasks in a terminal environment.

For scale, DeepSeek-V4-Flash-0731 scored 82.7 on the same benchmark in our testing, and GPT-5.6 Terra reached 87.4 in our Terra vs. Sonnet 5 comparison.

The picture changes on harder agentic evaluations.

V4.1 Flash drops to 30.0 on Terminal-Bench 3.0, so the model handles standard terminal tasks well but struggles with the more demanding agentic suite. Its Codeforces rating of 3471 signals strong competitive-programming ability.

Reasoning and knowledge tasks

On GPQA Diamond, a set of graduate-level science questions designed to be hard to answer from memory alone, V4.1 Flash scores 90.9.

That is a strong result for a model in this price band and puts it in the conversation with far more expensive frontier models.

On Humanity's Last Exam (HLE), a deliberately brutal expert-level benchmark, V4.1 Flash scores 36.8, or 39.1 with the starred configuration.

For comparison, agentic-science evaluations like Terminal-Bench-Science 0.1 show Claude Fable 5.1 at 52.6% against tougher rivals, so V4.1 Flash is not topping the hardest expert evals, but it is not embarrassing itself either.

Mathematical reasoning

V4.1 Flash scores 65.6 on MathArena Apex, a competition-math benchmark that rewards multi-step symbolic reasoning.

This is one of the model's stronger results and suggests it is a reasonable pick for quantitative and math-heavy workloads.

A score in the mid-60s here matters for anyone building tools that need reliable arithmetic and algebraic reasoning, since many cheaper models collapse on exactly these tasks.

Which Tier Should You Use?

The decision here is less about model variants and more about how you deploy V4.1 Flash, since the same weights run three ways. Your choice comes down to cost predictability, data privacy, and whether you can schedule jobs.

DeepSeek is also running a migration period. From 12:00 Beijing Time on September 14, 2026, until the future release of V4.1 Pro, requests to legacy endpoints are redirected, which pushes existing users toward the V4.1 family. If you are still on V4 Pro 0813, that model is being phased out, so plan the move to V4.1 Flash now.

Use case

Recommended path

Why

High-volume, always-on coding agent

Official API, peak pricing

$0.30/$1.20 per 1M is already far below frontier rivals; no scheduling overhead

Large batch reasoning or math jobs

Official API, off-peak

Off-peak halves the rate to $0.15/$0.60 per 1M if you can schedule the run

Sensitive or proprietary data

Self-host MIT weights

Keeps inference on your own hardware; requires real GPU memory

Heavy context reuse

API with prompt caching

Cache-hit input at $0.006 per 1M makes repeated context almost free

DeepSeek V4.1 Flash Pricing and Availability

V4.1 Flash is one of the cheapest capable models available, especially off-peak. Here is how the token pricing breaks down.

Rate

Peak

Off-peak

Input per 1M tokens

$0.30

$0.15

Output per 1M tokens

$1.20

$0.60

Cache-hit input per 1M tokens

$0.006

Not published

Off-peak prices are set to half of peak, and the scheme is designed to push batch and scheduled workloads into off-peak windows.

To put the peak rate in context, GPT-6 Astra and Claude Fable 5.1 both charge $10 per 1M input and $50 per 1M output, so V4.1 Flash is roughly 30x cheaper on input and 40x cheaper on output at peak.

The model being phased out, V4 Pro 0813, is priced at $0.435 per 1M input and $0.87 per 1M output with a 1M token context.

Because DeepSeek claims V4.1 Flash surpasses V4 Pro on performance, cost, and speed, there is little reason to stay on the older model.

V4.1 Flash is available through the official DeepSeek API, and its MIT-licensed weights are published for self-hosting.

From September 14, 2026, legacy endpoint requests are redirected toward the V4.1 family until V4.1 Pro ships.

How to Get Access to DeepSeek V4.1 Flash?

V4.1 Flash is both an API model and an open-weight release, so you can reach it in two ways.

On the API, the model ID is commonly listed as deepseek-flash, and the endpoint is OpenAI-compatible, which means most existing OpenAI client code works with a base URL change.

from openai import OpenAI
client = OpenAI(
api_key="YOUR_DEEPSEEK_KEY",
base_url="https://api.deepseek.com",)
response = client.chat.completions.create(
model="deepseek-flash",
messages=[{"role":"user","content":"Refactor this function..."}],)print(response.choices.message.content)Powered by

To self-host, the MIT-licensed weights allow commercial use, redistribution, and fine-tuning with no revenue thresholds.

When we ran the earlier V4-Flash-0731 locally with Unsloth Studio, it needed three NVIDIA A100 80GB GPUs at Q8 quantization, so budget similar hardware for the full-precision setup.

V4.1 Flash is DeepSeek making a statement on price and openness, while the rest of the frontier climbs toward $10/$50 per 1M tokens and gates access behind paid tiers.

A 552B MoE model with MIT weights, native image input, 90.9 on GPQA Diamond, and peak pricing 30x below GPT-6 Astra is a serious offer for high-volume workloads.

I would switch to it for coding agents and batch reasoning jobs, especially if you can schedule off-peak.

The negatives come when you look at a score of 30.0 on Terminal-Bench 3.0, which shows it wobbles on the hardest agentic tasks, and peak/off-peak billing adds scheduling work you have to own.

How does DeepSeek V4.1 Flash compare to V4 Pro 0813?⌄

DeepSeek states that V4.1 Flash surpasses V4 Pro 0813 on performance, cost, speed, and total time, and V4 Pro is being phased out as a result. V4.1 Flash costs $0.30/$1.20 per 1M tokens at peak versus V4 Pro's $0.435/$0.87, and from September 14, 2026, legacy endpoint requests are redirected toward the V4.1 family.

Where can I access DeepSeek V4.1 Flash?⌄

You can access V4.1 Flash through the official DeepSeek API using the model ID commonly listed as deepseek-flash, which runs on an OpenAI-compatible endpoint. Because the weights are MIT-licensed, you can also self-host the model on your own hardware.

How much does DeepSeek V4.1 Flash cost?⌄

Peak pricing is $0.30 per 1M input tokens and $1.20 per 1M output tokens, with cache-hit input at $0.006 per 1M. Off-peak pricing halves the standard rates to $0.15 input and $0.60 output per 1M tokens, so scheduling batch jobs off-peak roughly halves your bill.

Is DeepSeek V4.1 Flash open source?⌄

V4.1 Flash ships with MIT-licensed open weights, which permits commercial use, redistribution, and fine-tuning with no revenue thresholds. The trade-off is that you take on responsibility for safety, fine-tuning, and inference optimization when you self-host.

What is DeepSeek V4.1 Flash best used for?⌄

It is well suited to high-volume coding agents, batch reasoning, and math-heavy workloads, scoring 90.6 on Terminal-Bench 2.1, 90.9 on GPQA Diamond, and 65.6 on MathArena Apex. It is weaker on the hardest agentic tasks, dropping to 30.0 on Terminal-Bench 3.0, so it is less suited to long-horizon autonomous agent work.

Discover DeepSeek V4 features, pricing, and 1M context efficiency. We compare V4 Pro and Flash benchmarks against frontier models like GPT-5.5 and Opus 4.7.

DeepSeek's open-weight V4.1 Flash matches Gemini 3.8 Flash on agentic coding benchmarks at a third of the price. We compare specs, costs, and a hands-on build test.

DeepSeek V4 costs 98% less than GPT-5.5 Pro, but can it compete? We compare both models on agentic coding, long-context reasoning, and pricing to help you choos

Learn about DeepSeek-V3.2-Exp, its new sparse attention mechanism, how it reduces API costs and improves long-context handling, and how to use it in your own projects.

Learn how to use the DeepSeek API, including model selection, parameter management, cost optimization, context caching, and chain-of-thought optimization.

The frontier model race in late 2026 has mostly been an arms race on price rather than raw capability. In September alone, we've seen OpenAI's GPT-6 Astra and Anthropic's Claude Fable 5.1 both sit at $10 per 1M input tokens and $50 per 1M output tokens, and neither is available on free tiers.

DeepSeek's answer is to go the other direction entirely.

DeepSeek V4.1 Flash is a 552B-parameter mixture-of-experts model that activates roughly 8B parameters for input and 16B for output, ships with MIT-licensed open weights, and costs $0.30/$1.20 per 1M tokens at peak, dropping to $0.15/$0.60 off-peak, beating other similar Flash models, such as Gemini 3.8 Flash and GLM-5.3-Flash.

In this article, I'll cover everything new with DeepSeek V4.1 Flash, looking at the new features and exploring the benchmarks. If you want to see the model in action, read our DeepSeek V4.1 API tutorial.

To learn more about the DeepSeek family, check out our guide to running DeepSeek-V4-Flash-0731 with Unsloth Studio and OpenCode, and our comparison of DeepSeek v4.1 Flash vs Gemini 3.8 Flash.

## TL;DR

## What Is DeepSeek V4.1 Flash?

DeepSeek V4.1 Flash is the newest model in DeepSeek's Flash line, positioned as the efficient, high-throughput tier rather than a research-grade heavyweight.

It is a sparse mixture-of-experts (MoE) architecture with 552B total parameters, of which only about 8B are active per token on input and 16B on output. That sparse activation is what keeps serving costs low while still reaching frontier-level scores on several benchmarks.

The headline change versus the previous generation is efficiency.

DeepSeek shrank the KV cache to roughly a quarter of the previous V4 Flash's size, which reduces the memory footprint and lets the model serve longer contexts more cheaply. The company also states that V4.1 Flash surpasses the older V4 Pro 0813 on performance, cost, speed, and total time, and V4 Pro is being phased out as a result.

On the benchmark front, the number that stands out is Terminal-Bench 2.1 at 90.6. When we tested the earlier DeepSeek-V4-Flash-0731, it scored 82.7 on the same benchmark, so V4.1 Flash is a real step up on agentic terminal work within the same family.

## DeepSeek V4.1 Flash Key Features

V4.1 Flash's feature set is built around two ideas: run cheap, and stay open. Here are the capabilities that matter most for a working data scientist or ML engineer.

## Open weights you can self-host

V4.1 Flash ships with MIT-licensed weights, which is the most permissive common license and allows commercial use, redistribution, and fine-tuning with no revenue thresholds.

For teams with data-privacy constraints, this means you can run the model entirely on your own hardware rather than sending requests to the DeepSeek API.

The trade-off is that the MIT license shifts responsibility for safety, fine-tuning, and inference optimization onto you.

When we ran the earlier V4-Flash-0731 locally, it needed three NVIDIA A100 80GB GPUs at Q8 quantization, so self-hosting is only viable when you have real GPU memory to spare.

## Native image input

V4.1 Flash accepts images directly as input, without a separate vision adapter or preprocessing step. This lets you feed screenshots, charts, or document scans into the same request alongside text, which is useful for document-heavy pipelines and UI-agent workflows.

Native multimodal input at this price point is unusual. On the ZeroBench-main evaluation with tools, V4.1 Flash scores 49.0, which gives you a rough signal of how it handles reasoning over visual inputs.

## Reduced KV cache for cheaper long context

The KV cache in V4.1 Flash is roughly a quarter the size of the previous V4 Flash, which lowers the memory cost of holding long contexts in a serving session.

For anyone running always-on inference, a smaller KV cache means either more concurrent sessions per GPU or lower per-request memory overhead.

Combined with cache-hit input priced at $0.006 per 1M tokens at peak, this makes context reuse extremely cheap. If you repeatedly send the same system prompt or document across requests, the cached portion costs almost nothing.

## Peak and off-peak pricing modes

DeepSeek runs a two-tier pricing scheme in which off-peak prices are set at half of peak prices, with the stated intent of pushing batch and scheduled workloads into off-peak windows. That turns $0.30/$1.20 into $0.15/$0.60 per 1M tokens if you can time your jobs.

The caveat is operational. Always-on workloads become harder to cost-predict, and capturing the discount requires job-scheduling logic that you have to build and maintain yourself.

## How Does DeepSeek V4.1 Flash Perform on the Benchmarks?

V4.1 Flash's strongest showing is on agentic and reasoning benchmarks, where it reaches frontier-adjacent scores at a fraction of the token cost.

Its scores drop sharply on the hardest agentic evaluations, which is worth knowing before you point it at long-horizon tasks.

## Coding and agentic workflows

V4.1 Flash scores 90.6 on Terminal-Bench 2.1, which measures how well a model completes multi-step tasks in a terminal environment.

For scale, DeepSeek-V4-Flash-0731 scored 82.7 on the same benchmark in our testing, and GPT-5.6 Terra reached 87.4 in our Terra vs. Sonnet 5 comparison.

The picture changes on harder agentic evaluations.

V4.1 Flash drops to 30.0 on Terminal-Bench 3.0, so the model handles standard terminal tasks well but struggles with the more demanding agentic suite. Its Codeforces rating of 3471 signals strong competitive-programming ability.

## Reasoning and knowledge tasks

On GPQA Diamond, a set of graduate-level science questions designed to be hard to answer from memory alone, V4.1 Flash scores 90.9.

That is a strong result for a model in this price band and puts it in the conversation with far more expensive frontier models.

On Humanity's Last Exam (HLE), a deliberately brutal expert-level benchmark, V4.1 Flash scores 36.8, or 39.1 with the starred configuration.

For comparison, agentic-science evaluations like Terminal-Bench-Science 0.1 show Claude Fable 5.1 at 52.6% against tougher rivals, so V4.1 Flash is not topping the hardest expert evals, but it is not embarrassing itself either.

## Mathematical reasoning

V4.1 Flash scores 65.6 on MathArena Apex, a competition-math benchmark that rewards multi-step symbolic reasoning.

This is one of the model's stronger results and suggests it is a reasonable pick for quantitative and math-heavy workloads.

A score in the mid-60s here matters for anyone building tools that need reliable arithmetic and algebraic reasoning, since many cheaper models collapse on exactly these tasks.

## Which Tier Should You Use?

The decision here is less about model variants and more about how you deploy V4.1 Flash, since the same weights run three ways. Your choice comes down to cost predictability, data privacy, and whether you can schedule jobs.

DeepSeek is also running a migration period. From 12:00 Beijing Time on September 14, 2026, until the future release of V4.1 Pro, requests to legacy endpoints are redirected, which pushes existing users toward the V4.1 family. If you are still on V4 Pro 0813, that model is being phased out, so plan the move to V4.1 Flash now.

## DeepSeek V4.1 Flash Pricing and Availability

V4.1 Flash is one of the cheapest capable models available, especially off-peak. Here is how the token pricing breaks down.

Off-peak prices are set to half of peak, and the scheme is designed to push batch and scheduled workloads into off-peak windows.

To put the peak rate in context, GPT-6 Astra and Claude Fable 5.1 both charge $10 per 1M input and $50 per 1M output, so V4.1 Flash is roughly 30x cheaper on input and 40x cheaper on output at peak.

The model being phased out, V4 Pro 0813, is priced at $0.435 per 1M input and $0.87 per 1M output with a 1M token context.

Because DeepSeek claims V4.1 Flash surpasses V4 Pro on performance, cost, and speed, there is little reason to stay on the older model.

V4.1 Flash is available through the official DeepSeek API, and its MIT-licensed weights are published for self-hosting.

From September 14, 2026, legacy endpoint requests are redirected toward the V4.1 family until V4.1 Pro ships.

## How to Get Access to DeepSeek V4.1 Flash?

V4.1 Flash is both an API model and an open-weight release, so you can reach it in two ways.

On the API, the model ID is commonly listed as

`deepseek-flash`, and the endpoint is OpenAI-compatible, which means most existing OpenAI client code works with a base URL change.
To self-host, the MIT-licensed weights allow commercial use, redistribution, and fine-tuning with no revenue thresholds.

When we ran the earlier V4-Flash-0731 locally with Unsloth Studio, it needed three NVIDIA A100 80GB GPUs at Q8 quantization, so budget similar hardware for the full-precision setup.

For a full local walkthrough, see our tutorial on running DeepSeek-V4-Flash with Unsloth Studio and OpenCode, which covers the GPU distribution and inference setup in detail.

## Final Thoughts

V4.1 Flash is DeepSeek making a statement on price and openness, while the rest of the frontier climbs toward $10/$50 per 1M tokens and gates access behind paid tiers.

A 552B MoE model with MIT weights, native image input, 90.9 on GPQA Diamond, and peak pricing 30x below GPT-6 Astra is a serious offer for high-volume workloads.

I would switch to it for coding agents and batch reasoning jobs, especially if you can schedule off-peak.

The negatives come when you look at a score of 30.0 on Terminal-Bench 3.0, which shows it wobbles on the hardest agentic tasks, and peak/off-peak billing adds scheduling work you have to own.

If you want to build agent workflows around models like this, our AI Agent Fundamentals skill track is a good place to start.
