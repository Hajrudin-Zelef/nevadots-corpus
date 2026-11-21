---
id: collect-261121-veille-ia/deepseek/deepseek-v4-pro-goes-live-and-runs-openai-s-own-coding-agent-8x-cheaper-alphasignal
title: "DeepSeek V4-Pro Goes Live and Runs OpenAI's Own Coding Agent 8x Cheaper | AlphaSignal"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["agent", "openai", "deepseek", "v4-pro", "model", "build", "reasoning", "v4-flash"]
source: https://alphasignal.ai/news/deepseek-v4-pro-goes-live-and-runs-openai-s-own-coding-agent-8x-cheaper
source_anchor: ""
source_lines: [1, 85]
section: "DeepSeek"
---
## Takeaways

- **GA release:** DeepSeek-V4-Pro exits preview as build`V4-Pro-0813` , available on web, mobile, and API now.
- **Agent upgrades:** Major post-training improvements for agentic workflows; flexible reasoning effort (low/high/max) across V4-Pro and V4-Flash.
- **Codex support:** Native OpenAI Responses API integration lets you run Codex on DeepSeek with one-click setup -- no bridging code needed.
- **Pricing:** V4-Pro at $0.435/M input and $0.87/M output; peak/off-peak rates (50% off-peak discount) take effect August 16, 2026.
- **Benchmarks:** 80.6% on SWE-bench Verified (near Claude Opus 4.6), 93.5% LiveCodeBench -- at ~1/7 the output price of frontier competitors.
- **Architecture:** 1.6T total / 49B active MoE, 1M-token context, 384K max output; MIT-licensed open weights (April preview build) on Hugging Face.

DeepSeek-V4-Pro has officially left preview. After nearly four months of staged rollout, the flagship model is now fully available on the web app, mobile app, and API under the build designation `V4-Pro-0813`, completing a two-part release strategy that started when DeepSeek shipped V4-Flash to general availability on July 31.

## What's Under the Hood

V4-Pro is a mixture-of-experts system with 1.6 trillion total parameters and 49 billion active per token. Mixture-of-experts means the model is divided into many specialized sub-networks, and only a small fraction of them activate for any given token, so you get the capacity of a 1.6T model at the compute cost of a much smaller one. Both V4-Pro and V4-Flash share a 1M-token context window.

The attention system combines Compressed Sparse Attention (CSA) and Heavily Compressed Attention (HCA), targeting serving cost rather than raw capability. These are DeepSeek's custom attention variants that dramatically reduce the memory required to process long contexts. The KV cache (the memory buffer that stores past tokens during inference) drops to 10% of what V3.2 needed at the million-token setting. V4-Flash totals 284 billion parameters with around 13 billion active per token.

## What Actually Changed in the GA Build

The architecture is the same one that shipped in April. Post-training is where the work went. When V4-Flash went official, DeepSeek published agent-benchmark results showing the re-post-trained Flash build outscoring the V4-Pro-Preview on its internal coding-agent suites, a deliberate move that made the small model the default for agent workloads while the flagship stayed in preview. The 0813 build is the flagship's answer to that.

The key upgrades in V4-Pro:

- **Major agent improvements.** Production-tuned post-training specifically targeting multi-step agentic workflows.
- **Flexible reasoning effort.** Three modes: low for simple tasks, high for daily agent loops, and max for the hardest reasoning problems.
- **Native OpenAI Responses API support.** One-click Codex setup, meaning you can now run OpenAI's own coding agent on a DeepSeek model.
- **Anthropic API compatibility.** The model also exposes an Anthropic-format endpoint, making it a drop-in for Claude Code integrations.

## The Responses API Integration Is the Real Story

OpenAI introduced the Responses API as the successor to Chat Completions: a single interface designed for agentic workloads, with first-class reasoning items, built-in tools, and semantic streaming events. It is the format OpenAI's agent stack, including Codex, speaks natively. DeepSeek now supports it directly.

A Chinese open-weight lab has implemented OpenAI's newest API format, the one OpenAI built for its own agent products, specifically so that OpenAI's own coding agent can run on a DeepSeek model. That is a meaningful shift. Previously, plugging DeepSeek into Codex required custom bridging code. Now it is a config swap.

Codex-style development workflows need structured input and output items, reasoning items, tool calls, file-change operations, streaming events, usage accounting, and integration with coding-agent clients. The native support handles all of that.

Here is what the Codex config looks like for V4-Pro, per the official agent docs:

```
models:
  deepseek-v4-pro:
    context_window: 1000000
    max_output_tokens: 384000
    default_reasoning_level: "high"
    supported_reasoning_levels:
      - effort: "high"
      - effort: "xhigh"  # maps to max reasoning
    supports_reasoning_summaries: true
```
## Benchmarks: Strong, But Vendor-Reported

The numbers DeepSeek is putting forward come from its own harness runs. No independent replication of the 0813 build exists yet. With that caveat, the model card reports at max reasoning effort:

- **SWE-bench Verified:** 80.6%, level with Gemini-3.1-Pro, just behind Claude Opus 4.6 at 80.8%
- **LiveCodeBench:** 93.5% pass@1, top of the comparison table
- **GPQA Diamond:** 90.1% pass@1
- **Codeforces rating:** 3,206
- **MRCR at 1M tokens:** 83.5 MMR

V4-Pro lands within 0.2 points of Claude Opus 4.6 on SWE-bench Verified at roughly one-seventh the output price. The model trails GPT-5.4 on Terminal Bench 2.0 (67.9 vs 75.1) and Gemini-3.1-Pro on Humanity's Last Exam (37.7 vs 44.4). Coding benchmarks are where it shines; general knowledge and long-horizon reasoning are less dominant.

## Pricing: Still the Cheapest Frontier-Adjacent API

DeepSeek-V4-Flash costs $0.14 per million input tokens on a cache miss and $0.28 per million output tokens. V4-Pro costs $0.435 per million input tokens and $0.87 per million output tokens. Cache hits drop dramatically: cache-hit input falls to $0.003625 for Pro and $0.0028 for Flash.

Even at peak pricing, V4-Pro output is 8.6x cheaper than Claude Opus 4.8 and GPT-5.6. The new pricing structure introduces peak and off-peak rates, with off-peak 50% lower than peak, taking effect August 16, 2026 at 16:00 UTC. Worth planning around if you run high-volume workloads.

| Model | Input (cache miss) | Input (cache hit) | Output | Context | 
|---|---|---|---|---|
| V4-Flash | $0.14/M | $0.0028/M | $0.28/M | 1M tokens | 
| V4-Pro | $0.435/M | $0.003625/M | $0.87/M | 1M tokens | 

## How to Access It

The API model name remains `deepseek-v4-pro`, with a 1M context window and a maximum output of 384K tokens. The endpoint is a drop-in, so no migration is required if you were already calling the preview. V4-Pro is also available via the web and mobile app under **Expert Mode**. The official API docs have the full setup details including the Responses API guide and Codex configuration.

Both V4-Pro and V4-Flash support three reasoning effort modes. In practice: use `low` for fast lookups and simple completions, `high` for agent loops and daily coding tasks, and `max` (labeled `xhigh` in the Responses API) when you need the model to push its reasoning ceiling on hard math or complex multi-file refactors.

## The Bigger Picture

Three months after launch, DeepSeek V4 has done what the leaks predicted: architectural efficiency beating brute-force scaling on cost-per-quality. With a million-token context window, the V4-Pro permanent price cut, and MIT-licensed open weights, V4 has forced every major API provider to revisit pricing.

The open item is weights. The Hugging Face repository still hosts the April preview builds. DeepSeek has not confirmed whether the 0813 GA build differs from preview beyond post-training, or when updated weights will be published. For teams self-hosting, that is the gap to watch.

One more thing to plan for: DeepSeek's pricing page now warns of a significant API price increase coming in the near future, with specifics to follow by official notice. Current rates are still at preview levels, and they will not stay there.
