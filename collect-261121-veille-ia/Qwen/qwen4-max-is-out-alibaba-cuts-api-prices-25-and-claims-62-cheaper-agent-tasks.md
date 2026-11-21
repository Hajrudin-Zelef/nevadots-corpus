---
id: collect-261121-veille-ia/qwen/qwen4-max-is-out-alibaba-cuts-api-prices-25-and-claims-62-cheaper-agent-tasks
title: "Qwen4-Max Is Out: Alibaba Cuts API Prices 25% and Claims 62% Cheaper Agent Tasks"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: ["2026-11-15"]
keywords: ["qwen4-max", "alibaba", "output", "alibabas", "code", "tokens", "qwen", "model"]
source: https://foundermag.co/launches/qwen4-max/
source_anchor: ""
source_lines: [1, 47]
section: "Qwen"
---
## What launched

Alibaba’s Qwen team has fully released Qwen4-Max, the first model in its Qwen 4 series. Vercel’s AI Gateway changelog, dated November 15, 2026, calls it “a reasoning model with native vision and a 1M-token context window”, aimed at coding, professional work and long-running agent tasks. Qwen describes it as built on a new mixture-of-experts architecture. The full release comes after a preview that opened on November 11 through Alibaba’s subscription-only Token Plan, as `qwen4-max-preview`.

Where you can use it:

- **Alibaba’s own products:** the QwenCloud API, Qwen Studio, QwenWork and the Qoder coding tool. Token Plan subscriptions still work.
- **Third-party gateways:** Vercel AI Gateway as`alibaba/qwen4-max` , which passes through Alibaba’s pricing with no markup and can route to Claude Code, Codex and Cursor. OpenRouter lists it as`qwen/qwen4-max` .
- **Weights:** not released. Unlike Qwen3.8-Max, which Alibaba promised to open-weight, Qwen4-Max is hosted-only for now. Alibaba says smaller Qwen4 models will be open-sourced but hasn’t given a date. It is open-sourcing a “Live Delivery” plugin, which ships an early working version of a long task and then keeps improving it.

**Pricing:** OpenRouter lists Alibaba Cloud International at **$1.50 per million input tokens and $4.50 per million output tokens**. As AlphaSignal and Startup Fortune report, that’s 25% below Qwen3.8-Max’s $2/$6, and implicit-cache input drops to $0.075 per million tokens. During the preview, output was capped at 393,216 tokens, reasoning included.

## What Alibaba claims

Everything below comes from Alibaba’s own evaluations. Alibaba published no head-to-head scores against GPT, Claude or Gemini, and we found no independent results yet:

- **Work360** , Alibaba’s own benchmark covering 721 professional fields: 52.69, up from 47.74 for Qwen3.8-Max.
- **Cost per task:** the average Work360 task fell from $4.43 to $1.67, about 62% cheaper. Lower prices explain part of that. The rest comes from token efficiency: Alibaba says Qwen4-Max uses up to about 50% fewer tokens than Qwen3.8-Max, about 24% fewer on Work360 at its highest reasoning setting and roughly 48% fewer on Terminal-Bench 4.0.
- **Safer code:** Vercel’s changelog says it writes code with fewer injection, memory-safety and access-control flaws by default. Coverage of Qwen’s post reports the function-level pass rate on Last Secure Code Bench, which counts both working and secure code, rising from 22.0% to 31.8%. That still means most generated code fails the security check.
- **Agent demos:** building 3D scenes in Blender and playable games in Unity, Unreal and Godot using visual feedback, and finance, legal and healthcare tasks that call dozens of MCP tools and produce spreadsheets, slide decks and cited reports.

A chart giving Qwen4-Max a score of 53 on the Artificial Analysis index has circulated online. AlphaSignal notes Artificial Analysis hadn’t listed the model when it published, so treat that number as unconfirmed.

## Why it matters

The headline is price, not benchmarks. Here are list prices per million input/output tokens from each vendor:

| Model | Input | Output | 
|---|---|---|
| Qwen4-Max | $1.50 | $4.50 | 
| Gemini 4 Hydrogen | $1.50 | $7.50 | 
| GPT-6.1 Sol / Claude Sonnet 5.5 | $2 | $10 | 
| Gemini 4 Argon / Carbon | $2 ($4 from Dec 1) | $10 ($20 from Dec 1) | 
| Claude Opus 5.5 | $4 | $20 | 
| GPT-6 Astra | $10 | $50 | 

Qwen4-Max has the lowest output price on this list. Output is usually the bigger part of the bill for reasoning-heavy agents. Its output costs less than half of Sol’s or Sonnet 5.5’s, and less than a quarter of Opus 5.5’s.

What’s missing is evidence that it matches those models. Alibaba only compared Qwen4-Max to its own previous model, on its own benchmark. For now, the safe claim is that it’s a cheaper, more token-efficient Qwen3.8-Max. Whether it competes with the Western frontier is still unknown.

Two practical points for teams deciding whether to test it. First, it’s easy to try: Vercel AI Gateway and OpenRouter let you swap it into an existing OpenAI-compatible or coding-agent setup by changing a model ID. Second, closed weights change the picture for some teams. Self-hosting was a big part of Qwen-Max’s appeal last time. If you need on-prem or data-residency control, you’re waiting on the smaller open Qwen4 models, which have no release date yet.

**Who should try it now:** teams running high-volume agent or coding workloads on $2/$10-class models who are comfortable sending data to Alibaba Cloud. Run your own evals and compare cost per finished task. **Who can wait:** anyone who needs open weights, or independent benchmarks before switching a production workload.

## Where to try it

Call `qwen4-max` through QwenCloud, use it in Qwen Studio, QwenWork or Qoder, or route `alibaba/qwen4-max` via Vercel AI Gateway or `qwen/qwen4-max` via OpenRouter.
