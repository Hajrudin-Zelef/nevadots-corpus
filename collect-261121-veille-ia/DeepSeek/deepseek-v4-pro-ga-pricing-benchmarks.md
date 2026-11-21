---
id: collect-261121-veille-ia/deepseek/deepseek-v4-pro-ga-pricing-benchmarks
title: "DeepSeek V4 Pro: GA, Pricing & Benchmarks"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "context", "peak", "pricing", "off-peak", "flash", "tokens", "rates"]
source: https://www.deepseekv4guide.org/guides/v4-pro
source_anchor: ""
source_lines: [1, 159]
section: "DeepSeek"
---
# DeepSeek V4 Pro: GA Release, Pricing & Benchmarks (0813)

DeepSeek V4 Pro is now GA (Aug 13, 2026, 0813 build): 1.6T-param MoE, 1M context, new peak/off-peak pricing from Aug 16, and agent-first upgrades.

Retiring: from Sept 14, 2026, all deepseek-v4-pro requests route to V4.1 Flash at Flash rates. See why V4-Pro was retired and the V4.1 Flash guide.

## What Is DeepSeek V4 Pro?

DeepSeek V4 Pro is the flagship of the DeepSeek V4 family, now generally available. It is a Mixture-of-Experts (MoE) model with 1.6T total parameters and 49B active per token, a 1M-token context window, and 384K max output. Text-only, MIT-licensed<sup>[4]</sup>.

It launched as a preview on April 24, 2026 alongside DeepSeek V4 Flash, and moved to general availability on August 13, 2026 with the 0813 checkpoint — the build now served by the API under the unchanged model id deepseek-v4-pro<sup>[1]</sup><sup>[2]</sup>.

DeepSeek positions it as its answer to top closed-source models — 'performance rivaling the world's top closed-source models,' with agentic coding as open-source state of the art. The GA release centers on agent upgrades: flexible reasoning effort, native OpenAI Responses API support, and an Expert Mode in the app<sup>[1]</sup>.

| Spec | DeepSeek V4 Pro | DeepSeek V4 Flash | 
|---|---|---|
| Total parameters | 1.6T | 284B | 
| Active parameters (per token) | 49B | 13B | 
| Context window | 1M tokens | 1M tokens | 
| Max output | 384K tokens | 384K tokens | 
| Modality | Text only | Text only | 
| License | MIT | MIT | 
| Precision (inference) | FP4 + FP8 mixed | FP4 + FP8 mixed | 
| Released | Apr 24, 2026 (preview); Aug 13, 2026 (GA, 0813) | Apr 24, 2026 (preview); Jul 31, 2026 (0731 beta) | 

## V4 Pro Architecture & Long-Context Engineering

V4 Pro is engineered to make million-token context practical: at 1M context, per-token inference FLOPs are only 27% of DeepSeek V3.2, and KV cache is just 10% (vendor-reported).

Under the hood, V4 Pro pairs two attention mechanisms — Compressed Sparse Attention (CSA) and Heavily Compressed Attention (HCA) — with Manifold-Constrained Hyper-Connections (mHC) and the Muon optimizer. It was pretrained on more than 32T tokens. The technical report, 'DeepSeek-V4: Towards Highly Efficient Million-Token Context Intelligence,' is on arXiv as 2606.19348 (April 26, 2026).

For inference, the Pro build uses FP4 for MoE expert parameters and FP8 for everything else — which is why the full weights come to a third-party estimate of roughly 862GB of VRAM. The hosted API remains the practical route for most teams; DeepSeek publishes an official vLLM recipe if you do self-host.

- 1M context is native and the official default across DeepSeek services — no extra charge.
- Max output of 384K tokens; Think Max workloads recommend a context of at least 384K.
- Recommended sampling defaults: temperature 1.0, top_p 1.0.
- Self-hosting starting point: 8-GPU vLLM setup (official recipe); 4x80GB GPUs are a realistic minimum per third-party estimates.

The 27% FLOPs / 10% KV cache figures and the 32T pretraining count are vendor-reported from the technical report.

## V4 Pro Pricing: Peak / Off-Peak Rates (Aug 16)

On August 16, 2026 (16:00 UTC) DeepSeek switched the whole V4 lineup to peak / off-peak pricing. Off-peak rates are 50% lower than the new peak rates; peak hours are 01:00-04:00 and 06:00-10:00 UTC<sup>[1]</sup><sup>[3]</sup>.

Compared with the pre-Aug-16 flat rates (Pro $0.435 input / $0.87 output; Flash $0.14 / $0.28), this is a real price increase: Pro output rises +127.6% off-peak and +355% at peak; the cache-hit input price rises +507% off-peak and +1114% at peak<sup>[3]</sup>.

That +1114% figure is where the 'DeepSeek V4 Pro 1,100% price hike' headlines come from — it is the worst-case corner (cache-hit input during peak hours), not the headline rate. Normalize your workloads to off-peak (roughly 12 of every 24 hours) to cut the bill roughly in half<sup>[3]</sup>.

Concurrency limits are unchanged: Pro 500 concurrent requests, Flash 2,500. Cache-hit pricing applies automatically via context caching — no SDK changes required<sup>[3]</sup>.

| Per 1M tokens (USD) | Pro OFF-PEAK | Pro PEAK | Flash OFF-PEAK | Flash PEAK | 
|---|---|---|---|---|
| Input (cache hit) | $0.022 | $0.044 | $0.007 | $0.014 | 
| Input (cache miss) | $0.66 | $1.32 | $0.22 | $0.44 | 
| Output | $1.98 | $3.96 | $0.66 | $1.32 | 

All prices per 1M tokens, verified on DeepSeek's official pricing page August 16, 2026<sup>[3]</sup>. Thinking-mode reasoning tokens bill at the output price. See the V4 Pro pricing guide for the full breakdown and cost strategy.

## V4 Pro Benchmarks: 0813 GA Results

The 0813 GA build closes the gap to the closed frontier and takes first place on two agent benchmarks. All official agent scores were measured with the DeepSeek Harness minimal mode at max reasoning effort<sup>[4]</sup>.

Headlines from the table: Terminal-Bench 2.1 jumps from 72.1 (preview) to 87.9 (+15.8), just behind Opus-4.8's 88.3; Cybergym 83.3 and AutomationBench 31.8 are the highest published scores in the comparison set<sup>[4]</sup>. DeepSWE 62.7 is a 390% jump over the preview's 12.8.

An independent 8-task coding/reasoning evaluation by mindstudio scored 0813 at 61/80 (76.25%), tied with Muse Spark 1.2 and slightly below Kimi K3 and Opus 5 — versus just 24.8% for the preview build. Their field notes: it over-thinks simple problems and over-rewrites code, but is strong at front-end generation, task planning, and clarifying questions<sup>[7]</sup>.

One caveat in both sources: every official number is a Harness + model result (minimal mode, max effort). Independent reproduction with a different harness can diverge — the harness choice is part of the score<sup>[4]</sup><sup>[8]</sup>.

| Benchmark | V4 Pro 0813 | Flash-0731 | V4 Pro Preview | Opus-4.8 | 
|---|---|---|---|---|
| Terminal-Bench 2.1 | 87.9 | 82.7 | 72.1 | 88.3 | 
| Cybergym | 83.3 (1st) | 76.7 | 52.7 | 80.0 | 
| AutomationBench (Public) | 31.8 (1st) | 25.1 | 12.8 | 30.8 | 
| DeepSWE | 62.7 | 54.4 | 12.8 | 67.5 | 
| NL2Repo | 61.5 | 54.2 | 38.5 | — | 
| Toolathlon (verified) | 74.1 | 70.3 | 55.9 | 76.5 | 
| Agents' Last Exam | 25.7 | 25.2 | 16.5 | 27.6 | 

## V4 Pro GA: Reasoning Effort, Responses API & Expert Mode

The GA release is a production upgrade, not a version bump. Three changes matter for developers<sup>[1]</sup>.

The reasoning_effort knob is the practical headline: earlier preview builds were effectively max-only, which made simple calls slow and expensive. With low available, cheap routing (low for simple, high for daily agents, max only for hard problems) is now possible on both Pro and Flash<sup>[1]</sup><sup>[5]</sup>.

The Responses API support is the other big one — before GA (Aug 6) only Flash supported Responses/Codex; Pro errored. Since 0813, Pro is natively supported through the same endpoint<sup>[5]</sup>.

- Flexible reasoning effort: low (simple tasks) / high (daily agent workflows, the default) / max (complex tasks). Parameter: reasoning_effort, with thinking enabled by default on Pro<sup>[1]</sup><sup>[5]</sup> .
- Native OpenAI Responses API support, optimized for Codex with one-click setup — all Codex clients (CLI, ChatGPT desktop, VS Code extension) share one config file<sup>[1]</sup><sup>[5]</sup> .
- Expert Mode in app/web: select V4 Pro in the desktop model menu to run it with the heavier agent-style reasoning loop<sup>[1]</sup> .

In thinking mode, temperature, top_p, presence_penalty, and frequency_penalty are silently ignored. FIM and Chat Prefix Completion work only in non-thinking mode<sup>[6]</sup>.

```
# One-click Codex setup (macOS / Linux) — backs up ~/.codex/config.toml,
# writes models.json (flash + pro metadata) and configures model_providers.deepseek
bash <(curl -fsSL https://cdn.deepseek.com/api-docs/codex-deepseek-setup-en.sh)
# Windows (PowerShell)
irm https://cdn.deepseek.com/api-docs/codex-deepseek-setup-en.ps1 | iex
```
## Flash-0731 vs Pro-0813: Which to Use

The practical routing question: Flash now costs roughly one-third of Pro at off-peak, and Flash-0731's agent numbers already beat Pro-Preview. The GA closes part of that gap on agent work — but Pro still leads on raw reasoning and world knowledge<sup>[1]</sup><sup>[3]</sup><sup>[4]</sup>.

A common pattern after GA: default to Flash, escalate to Pro for hard reasoning and complex agentic tasks, and schedule non-urgent Pro jobs into off-peak hours to halve the bill. DeepSeek's own 22-benchmark aggregation puts Flash at roughly 83% of Pro's quality at a third of the price<sup>[3]</sup>.

Both support the same reasoning_effort levels and the Responses API, so the switch is a model string, not a migration<sup>[1]</sup><sup>[5]</sup>.

| Dimension | V4 Pro 0813 | V4 Flash 0731 | 
|---|---|---|
| Off-peak output price / 1M | $1.98 | $0.66 | 
| Terminal-Bench 2.1 (official) | 87.9 | 82.7 | 
| DeepSWE (official) | 62.7 | 54.4 | 
| Concurrency | 500 | 2,500 | 
| Best for | Hard reasoning, complex agents, world knowledge | High-volume daily coding at low cost | 

All benchmark numbers are official (Harness minimal mode) unless noted. Independent reproduction with other harnesses can diverge.

## Calling the DeepSeek V4 Pro API

The GA build is call-compatible: same base URL, same key, same model id deepseek-v4-pro. Add reasoning_effort and the optional thinking block<sup>[1]</sup><sup>[6]</sup>.

For a full setup walkthrough (OpenAI format, Anthropic format, tool calls, and the Responses API), see the V4 Pro API guide and the Responses API + Codex guide.

- OpenAI Responses API is natively supported — Codex can talk to DeepSeek directly (one-click setup script in Step 5)<sup>[5]</sup> .
- Anthropic-format calls use base_url https://api.deepseek.com/anthropic with a reasoning block (effort: none/low/high/max)<sup>[6]</sup> .
- In thinking mode, temperature/top_p/presence/frequency penalties are ignored; multi-turn tool calls must return reasoning_content or you get a 400<sup>[6]</sup> .
- Legacy aliases deepseek-chat / deepseek-reasoner stopped resolving on July 24, 2026 — use deepseek-v4-pro or deepseek-v4-flash.

V4 Pro now supports Chat Completions, the Anthropic-compatible endpoint, tool calls, JSON output, and — since GA — the Responses API<sup>[1]</sup><sup>[5]</sup>.

```
from openai import OpenAI
client = OpenAI(
    api_key="<DeepSeek API Key>",
    base_url="https://api.deepseek.com",   # Anthropic format: https://api.deepseek.com/anthropic
)
response = client.chat.completions.create(
    model="deepseek-v4-pro",
    messages=[{"role": "user", "content": "Refactor this function to be async."}],
    reasoning_effort="high",              # low | high | max
    extra_body={"thinking": {"type": "enabled"}},
)
# Chain of thought is returned in reasoning_content; the answer in content.
```
## Should You Build on DeepSeek V4 Pro Now?

Short answer: yes for agentic workloads that need the flagship, with an off-peak scheduling strategy. The GA removes the 'preview' risk; the pricing change removes the 'flat rate' assumption.

The GA verdict from independent testing is a qualified yes: 0813 is 'one of the largest single-version jumps' (24.8% → 76.25% on the mindstudio suite), though reviewers note over-thinking on simple problems and code over-rewriting — tune reasoning_effort low for simple tasks<sup>[7]</sup>.

If you pair it with the official DeepSeek Harness, you get the exact stack DeepSeek used for its agent benchmarks (minimal mode, max effort)<sup>[4]</sup>. For routing guidance between the family, see V4 Pro vs Flash and V4 Pro vs GPT-5.5.

- Cost: off-peak Pro output is $1.98/1M vs $3.96 at peak — schedule batch and background agent jobs into off-peak hours (about 12h/day)<sup>[3]</sup> .
- Reasoning effort: route simple calls to low to avoid paying max-depth token cost on trivial tasks<sup>[1]</sup><sup>[5]</sup> .
- Cache: context caching is automatic; cache-hit input is $0.022 off-peak — reuse prefixes to cut input cost<sup>[3]</sup> .
- Hybrid: Flash-first default with Pro escalation stays the value pattern; Pro leads on world knowledge and hard reasoning<sup>[3]</sup> .
- Self-host: 862GB VRAM estimate (third-party) — the hosted API is the realistic default; official vLLM recipe exists if you insist.

Every figure in this guide comes from DeepSeek's official docs, the model card, or the cited independent evaluations, all as of August 16, 2026. Official agent scores are vendor-reported and measured on the Harness.
