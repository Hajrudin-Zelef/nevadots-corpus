---
id: collect-261121-veille-ia/qwen/what-is-qwen-3-8-max
title: "What Is Qwen 3.8-Max?"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["qwen", "model", "august", "tokens", "early", "weights", "input", "thinking"]
source: https://apidog.com/blog/what-is-qwen-3-8/
source_anchor: ""
source_lines: [1, 132]
section: "Qwen"
---
Alibaba officially released Qwen 3.8-Max in early August 2026, and it lands with an unusual combination: a 2.4 trillion parameter Mixture-of-Experts model that only activates 95B parameters per token, a flat $2/$6 per million tokens price across a full 1M context, and a promise that the weights go public on Hugging Face and ModelScope within a week. That last part matters. No Qwen-Max-class model has ever shipped open weights before.

The official announcement calls it the most capable Qwen to date, and the vendor benchmark table backs that with some genuinely strong rows and a few honest losses. This guide breaks down what Qwen 3.8-Max actually is, what the numbers say, where it sits against Kimi K3, Fable 5, and GPT-5.6 Sol, and every route you have to run it today. If you plan to build against the API, the model ships with both OpenAI-compatible and Anthropic-compatible endpoints, which makes a protocol-aware client like Apidog useful for testing both shapes against the same key.

## Qwen 3.8-Max at a glance

| Spec | Qwen 3.8-Max | 
|---|---|
| Developer | Alibaba (Qwen team) | 
| Release | Early August 2026 (GA on Model Studio, August 3) | 
| Architecture | MoE, built on the Qwen 3.5 foundation | 
| Total parameters | 2.4T | 
| Active parameters | 95B (~4% activation) | 
| Context window | 1,000,000 tokens | 
| Max output | 65,536 tokens | 
| Modalities | Text + image input, text output | 
| Reasoning controls | `reasoning_effort` : xhigh (default), medium, low | 
| API model ID | `qwen3.8-max` | 
| Pricing | $2 input / $6 output per 1M tokens, flat across the full context | 
| Open weights | Promised for next week (~August 10); not yet downloadable as of early August 2026 | 
| API protocols | OpenAI-compatible and Anthropic-compatible | 

Everything in that table comes from Alibaba’s own release materials and the Model Studio pricing page. Now for the detail.

## What Qwen 3.8-Max is

Qwen 3.8-Max is the new flagship of Alibaba’s Qwen family, built on the Qwen 3.5 architectural foundation. It replaces Qwen3.7-Max at the top of the lineup, and the vendor table shows meaningful jumps over its predecessor: Terminal Bench 2.1 goes from 74.5 to 86.6, and PaperBench goes from 64.8 to 93.0. If you’re deciding whether to move off the older model, the deltas are laid out in our Qwen 3.8 vs Qwen 3.7 Max comparison.

The headline spec is the parameter split. 2.4T total parameters sounds enormous, and it is, but the MoE design activates only 95B per forward pass. That roughly 4% activation ratio is why Alibaba can serve a model this large at $2/$6 per million tokens. For context, Kimi K3 runs 2.8T total with 104B active, so Qwen 3.8-Max is the smaller model on both counts.

On the input side, the model accepts text and images through the API. Alibaba’s blog also demonstrates long-document understanding (200+ page PDFs) and 100-hour video comprehension via what it calls memory graphs. Treat those as blog-demonstrated capabilities rather than separate API input types; the published API configs list text and image as the input modalities.

Reasoning is controlled through a `reasoning_effort` parameter with three levels: xhigh (the default), medium, and low. There’s also `enable_thinking` and `preserve_thinking`, with thinking preserved by default. One pricing detail worth knowing early: thinking and non-thinking modes bill at the same rate, but thinking tokens count as output, so real bills at the xhigh default run higher than a naive sticker calculation.

## The first open-weight Max-class Qwen

This is the release’s biggest strategic move. Alibaba has open-sourced plenty of Qwen models over the years, but never the Max tier. Qwen 3.8-Max breaks that pattern: the company says weights land on Hugging Face and ModelScope “next week,” which works out to around August 10.

Two caveats, stated plainly:

1. **The weights are not downloadable yet.** As of early August 2026, the promise is a promise. Kimi K3 set a recent precedent here: its weights arrived 11 days after launch, so a short slip wouldn’t be surprising.
2. **Self-hosting a 2.4T model is a multi-node project.** Even aggressively quantized, this is not something you run on a workstation. For most teams, “open weights” will mean cheaper hosted access through third-party providers rather than a local deployment.

If your interest in Qwen 3.8-Max is mostly about not paying list price, the current realistic routes are Qwen Chat and the Model Studio free quota, both covered in our guide to using Qwen 3.8 for free.

## What the benchmarks show, including the losses

Alibaba published a full benchmark table comparing Qwen 3.8-Max against Claude Opus 4.8, Fable 5, GPT-5.6 Sol, and Qwen3.7-Max. Two disclosures before any numbers: these are vendor-run results, and most of the coding rows were executed using the Claude Code harness for all models. Several benchmarks (QwenSWEBench, QwenQoderBench, CoWorkBench, RecreationBench) are Alibaba’s own in-house creations. No independent numbers from outlets like Artificial Analysis existed at the time of writing.

With that framing, the standout rows per Alibaba’s table:

**Where it wins:**

- **PaperBench: 93.0** , its single best flagship result, ahead of GPT-5.6 Sol (90.5), Fable 5 (88.8), and Opus 4.8 (80.3)
- **IFBench: 82.8** , the top score in the row, ahead of Sol’s 72.7
- **Terminal Bench 2.1: 86.6** , beating Opus 4.8 and Fable 5 (both 84.6), trailing only Sol (88.8)
- **Multimodal is its strongest suit** : MathVision 95.2, LogicVista 91.9, OSWorld-Verified 86.1, and OCR rows that lead nearly everything in the table

**Where it loses:**

- **SWE-bench Pro: 67.7** , well behind Fable 5’s 80.0 and slightly behind Opus 4.8’s 69.2 (it does beat Sol’s 64.6)
- **HLE: 43.6** , the weakest flagship score in the row; Fable 5 posts 53.3, Sol 47.2, and Opus 4.8 45.7

The honest summary: per Alibaba’s own numbers, Qwen 3.8-Max beats Opus 4.8 on several agentic rows, trails Fable 5 on most core coding work, and wins big on multimodal and document intelligence. On GPQA Diamond it sits at 92.6, tied with Fable 5 and just behind Sol’s 94.1, so raw science reasoning is competitive. For the full table walkthrough, including the harness fine print and what independent verification to watch for, see our Qwen 3.8 benchmarks breakdown.

Alibaba also published a set of showcase runs: a 16-day autonomous coding session on a public repo (265 commits, 127 pull requests), a paper-reproduction run that beat the original paper’s AIME24 result, and a Tianchi contest entry that outperformed 458 of 526 human teams in 24 hours. These are the vendor’s own demos, so read them as capability advertisements rather than controlled evaluations. The coding story, including how to reproduce the harness setups, gets a dedicated treatment in Qwen 3.8 for coding.

## Where it sits against Kimi K3, Fable 5, and GPT-5.6 Sol

**Vs Kimi K3.** This is the natural rivalry: two Chinese labs, two open-weight giant MoE models, released weeks apart. K3 is bigger (2.8T total, 104B active vs 2.4T/95B), text-only where Qwen is multimodal, and its weights are already live while Qwen’s are still a promise. Pricing diverges sharply too: K3 charges $3 input/$15 output with $0.30 cache hits, against Qwen’s flat $2/$6. No direct head-to-head evaluation exists yet; each vendor published its own table. We compare them row by row in Qwen 3.8 vs Kimi K3, and if you’re new to Moonshot’s model, start with what Kimi K3 is.

**Vs Fable 5.** Anthropic’s flagship remains the coding benchmark to beat, and Qwen 3.8-Max doesn’t beat it: SWE-bench Pro (67.7 vs 80.0) and HLE (43.6 vs 53.3) are clear gaps, per Alibaba’s own table. What Qwen offers instead is price (a fraction of frontier rates), a 1M context, open weights on the way, and stronger multimodal rows.

**Vs GPT-5.6 Sol.** Sol takes Terminal Bench (88.8 vs 86.6), GPQA (94.1 vs 92.6), and HLE (47.2 vs 43.6), while Qwen 3.8-Max wins PaperBench (93.0 vs 90.5) and IFBench (82.8 vs 72.7). On price, Qwen’s $2/$6 undercuts GPT-5.6 Terra’s $2/$12 on output, and Sol-tier access costs more still.

One more anchor: Claude Opus 5 lists at $5/$25. Whatever you make of vendor tables, Alibaba is pricing its flagship at a level that changes the math for high-volume workloads.

## Pricing: $2/$6, flat across 1M tokens

The pricing is simple enough to state in one sentence: $2 per million input tokens, $6 per million output tokens, one flat tier from token zero to the 1M context limit, thinking mode or not.

The flat tier is the unusual part. Most long-context models step prices up as your prompt grows; Qwen 3.8-Max doesn’t. It also launches cheaper than Qwen3.7-Max’s list price of $2.5/$7.5, although that older model currently runs a 50% promotion ($1.25/$3.75), which keeps it relevant as a value play.

Context caching sweetens repeated-prefix workloads: cache hits bill at 10% of the input rate, and explicit cache creation costs 125%. There’s also a free quota (1M tokens, Singapore region only, valid 90 days) for kicking the tires. The full cost breakdown, with worked per-task examples and the thinking-token gotcha quantified, lives in our Qwen 3.8 pricing guide.

## How to access Qwen 3.8-Max

The access matrix is broader than a typical launch. Five routes:

**1. Qwen Chat.** The consumer app, no API key needed. Fastest way to form an opinion.

**2. The API on Alibaba Cloud Model Studio (DashScope).** Get a key from home.qwencloud.com, set `DASHSCOPE_API_KEY`, and call model ID `qwen3.8-max` through OpenAI-compatible chat-completions or responses endpoints. Three regional base URLs are live: Beijing (`dashscope.aliyuncs.com/compatible-mode/v1`), Singapore (`dashscope-intl.aliyuncs.com/compatible-mode/v1`), and US-Virginia (`dashscope-us.aliyuncs.com/compatible-mode/v1`). The Model Studio models page lists it at the top of the recommended stack.

**3. The Anthropic-compatible endpoint.** This is the genuinely unusual one: `https://dashscope-intl.aliyuncs.com/apps/anthropic` speaks the Anthropic Messages protocol, so tools built for Claude can point at Qwen with two environment variables.

**4. Coding harnesses.** Alibaba published official configs for five agents: Claude Code (via `ANTHROPIC_BASE_URL` plus `ANTHROPIC_MODEL=qwen3.8-max`), Codex, Qoder, Qwen Code, and OpenClaw. It’s worth noting that Alibaba ran most of its coding benchmarks inside the Claude Code harness, so the config it published is also the config behind its own numbers.

**5. Open weights, soon.** Hugging Face and ModelScope, promised for next week, not yet downloadable as of early August 2026.

Setup details for routes 2 through 4, with copy-paste Python and cURL, are in our Qwen 3.8 API guide.

### Testing the dual-protocol API

The same model answering on two different protocol shapes creates a practical testing question: does your request behave identically on both? This is where an API client earns its keep. In Apidog, you can save the three regional base URLs as environments and switch regions without editing requests, send the same prompt through the OpenAI-compatible endpoint and the Anthropic-protocol endpoint side by side, and inspect the SSE stream to watch `reasoning_content` deltas arrive before the final answer. That last part is useful for verifying how much thinking the xhigh default actually generates, since those tokens hit your bill as output. Download Apidog for free if you want to follow along when you wire up the endpoints; the same workspace also makes it easy to A/B `qwen3.8-max` against `qwen3.7-max` or `kimi-k3` on identical prompts before you commit a workload.

## Where Qwen 3.8-Max fits in the lineup

If you’re mapping the broader family, Qwen 3.8-Max now sits above Qwen3.7-Max and the plus-tier models, and our guide to the best Qwen models covers which tier fits which workload. The short version: 3.8-Max is the pick when you need the multimodal and agentic ceiling, 3.7-Max at 50% off is the value flagship while the promotion lasts, and the plus models remain the cheap workhorses for routine tasks.

## FAQ

**Is Qwen 3.8 open source?**

Not yet, but it’s close. Alibaba promised the weights for Hugging Face and ModelScope “next week” as of the early August 2026 launch, which would make it the first open-weight Max-class Qwen. Nothing is downloadable at the time of writing. Until then, access runs through the API or Qwen Chat; see how to use Qwen 3.8 for free for the no-cost routes.

**How much does Qwen 3.8-Max cost?**

$2 per million input tokens and $6 per million output tokens, on a single flat tier across the full 1M context. Cache hits bill at 10% of input. Thinking tokens bill as output, and the default reasoning effort is xhigh, so budget above the sticker rate for reasoning-heavy work.

**Is Qwen 3.8-Max better than Kimi K3?**

There’s no direct head-to-head evaluation yet; both vendors published their own tables. On paper, Qwen 3.8-Max is smaller (2.4T/95B active vs 2.8T/104B), multimodal where K3 is text-only, and cheaper on output ($6 vs $15). K3’s advantage today is that its weights are already public.

**Can I use Qwen 3.8-Max in Claude Code?**

Yes. Alibaba published an official config: point `ANTHROPIC_BASE_URL` at the Anthropic-compatible DashScope endpoint and set `ANTHROPIC_MODEL=qwen3.8-max`. Codex, Qoder, Qwen Code, and OpenClaw have official configs too.

## What to do next

Qwen 3.8-Max is a real flagship release, not a press preview: GA on three API regions, published pricing, a full (vendor-run) benchmark table with wins and losses, and open weights days away. The honest read is that it doesn’t dethrone Fable 5 on core coding, but it beats Opus 4.8 on several agentic rows, leads on document and multimodal work, and costs $2/$6 while doing it.

The sensible move is to test it against your own workload rather than anyone’s table. Grab the free quota, wire up both protocol endpoints, and compare outputs on prompts you actually run. Start with the API setup guide, and check back around August 10 to see whether the weights landed on schedule.
