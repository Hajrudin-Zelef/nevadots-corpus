---
id: collect-261121-veille-ia/deepseek/deepseek-v4-release-date-features-benchmarks-2026
title: "DeepSeek V4 Release Date, Features & Benchmarks 2026"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "release", "v4-pro", "v4-flash", "april", "both", "released", "august"]
source: https://codersera.com/blog/deepseek-v4-release-date-features-benchmarks/
source_anchor: ""
source_lines: [1, 214]
section: "DeepSeek"
---
**Quick answer.** DeepSeek V4 is released, and it is no longer a preview. V4-Pro and V4-Flash shipped on April 24, 2026, and **V4-Pro reached general availability on August 13, 2026** with the V4-Pro-0813 checkpoint. Both are open-weight (MIT), text-only, and carry a 1,048,576-token context window. The legacy `deepseek-chat` and `deepseek-reasoner` aliases retired July 24, 2026.

DeepSeek V4 is officially released and live. On **April 24, 2026**, DeepSeek shipped two production-ready models — **DeepSeek V4-Pro** and **DeepSeek V4-Flash** — both available immediately via the DeepSeek API and as open weights under the MIT license. DeepSeek frames this as a **preview** release: stable, usable in production, but with a stable version expected later in 2026. This page is the canonical live-status reference — it covers the exact release timeline, the hard API-migration deadline, verified specifications, vendor-reported benchmarks, and current API pricing as of June 2026.

**Want the full picture?** Read our continuously-updated **DeepSeek V4 complete guide** — benchmarks, pricing, deployment patterns, and how it compares to GPT-5.5 and Claude Opus 4.7.

**Want the full picture?** Read our continuously-updated **DeepSeek V4 cost breakdown** — how V4-Flash ($0.14/$0.28) and V4-Pro compare on price and coding to Claude Opus 5, GPT-5.6, Kimi K3 and Gemini 3.5, with real monthly-bill math.

## Is DeepSeek V4 released yet? Status as of August 2026?

Yes. DeepSeek V4 is released and has been live since April 24, 2026. There is no waiting, no waitlist, and no "coming soon" — both models are callable via the API and downloadable from Hugging Face today. Here is the precise state of play as of August 2026:

- **Released:** April 24, 2026. Both V4-Pro and V4-Flash shipped simultaneously.
- **Release stage:****Generally available.** The April 24 launch was a preview; DeepSeek promoted V4-Pro to GA on**August 13, 2026** with the**V4-Pro-0813** checkpoint — architecturally near-identical to the preview plus DSpark speculative decoding. GA also added reasoning-effort levels (`low` /`high` /`max` ) on both models, a native OpenAI Responses API with one-click Codex setup, and an Expert Mode in the app and web client. Model IDs did not change. Both models remain**text-only** — the multimodal/vision modes rumoured from test interfaces did not ship. See the V4-Pro-0813 guide.
- **Access:** DeepSeek API (model IDs`deepseek-v4-pro` and`deepseek-v4-flash` ) and open weights on Hugging Face under MIT.
- **Migration deadline:** The legacy`deepseek-chat` and`deepseek-reasoner` aliases are**fully retired and inaccessible after July 24, 2026, 15:59 UTC** (per the official DeepSeek API change log). Until then they transparently route to V4-Flash.

If you landed here searching "is DeepSeek V4 out" or "DeepSeek V4 release status April/May 2026" — the answer is unambiguous: it shipped in April, it went GA on August 13, 2026, and the legacy alias deadline has already passed.

## What is the DeepSeek release timeline (V3 to V4)?

DeepSeek moved fast from V3 to V4. The timeline below covers the releases that matter for anyone deciding what to run today.

| Release | Date | What it was | Status (June 2026) | 
|---|---|---|---|
| DeepSeek-V3 | Dec 2024 | First large MoE generation; established the cost-efficient open-weight line | Superseded | 
| DeepSeek-V3.2 | Late 2025 | Incremental MoE upgrade; reachable via `deepseek-chat` /`deepseek-reasoner` aliases | Legacy aliases retire Jul 24, 2026 | 
| DeepSeek V4-Pro (GA) | Apr 24, 2026 | Flagship: 1.6T total / 49B active, 1M context, hybrid attention | Live (GA since Aug 13, 2026) | 
| DeepSeek V4-Flash | Apr 24, 2026 | Cost-optimized: 284B total / 13B active, 1M context | Live | 

V4-Pro and V4-Flash were a single dual-model launch on the same day — there was no staggered rollout. The only date you must act on is the legacy-alias retirement.

## What is the DeepSeek V4 API migration deadline?

Until the deadline, the legacy aliases keep working but are transparently routed to **V4-Flash** (non-thinking for `deepseek-chat`, thinking for `deepseek-reasoner`). The single most common migration mistake is assuming `deepseek-reasoner` mapped to the flagship — it did not. It routes to V4-Flash, so moving to `deepseek-v4-pro` is an *upgrade*, not a like-for-like swap, and you should re-test prompts before assuming identical behavior.

The migration itself is a one-line change per call: replace the legacy model string with `deepseek-v4-pro` or `deepseek-v4-flash`. The API surface remains OpenAI-compatible, so no SDK change is required.

## What is DeepSeek V4 (two models, one release)?

DeepSeek V4 is a dual-model release built on a **Mixture-of-Experts (MoE)** architecture. Both models support a **1 million token context window** with a maximum output of 384K tokens, and both are released under the MIT license — meaning free commercial use and full weights access on Hugging Face.

| Model | Total Parameters | Activated per Token | Context Window | Max Output | License | 
|---|---|---|---|---|---|
| DeepSeek V4-Pro | 1.6T | 49B | 1M tokens | 384K tokens | MIT | 
| DeepSeek V4-Flash | 284B | 13B | 1M tokens | 384K tokens | MIT | 

V4-Pro is the flagship model, targeting frontier-level reasoning, coding, and agentic workflows. V4-Flash is the cost-optimized variant — it trades some benchmark headroom for dramatically lower latency and API cost, making it the practical choice for high-volume production workloads. Parameter counts and context limits above are confirmed in DeepSeek's official API release notes. For a detailed comparison with the previous generation, see DeepSeek V4 vs DeepSeek V3.2: What Changed and What Developers Should Use.

## How does the DeepSeek V4 architecture work?

DeepSeek V4 introduces three architectural changes that separate it from V3.2. Understanding them matters because they explain why V4 can handle 1M-token contexts at a fraction of the inference cost of competing models.

### 1. CSA + HCA Hybrid Attention

The central innovation is a hybrid attention mechanism that interleaves **Compressed Sparse Attention (CSA)** and **Heavily Compressed Attention (HCA)** across Transformer layers.

CSA compresses the Key-Value cache of every m tokens into a single entry using a learned token-level compressor, then applies DeepSeek Sparse Attention (DSA) where each query token attends only to top-k selected compressed KV entries. HCA takes compression further for layers that tolerate greater approximation. The result: at 1M-token context, DeepSeek V4-Pro requires only **27% of the single-token inference FLOPs** and **10% of the KV cache** compared with DeepSeek-V3.2. That is not a rounding artifact — it is a structural efficiency gain that makes long-context inference economically practical.

### 2. Manifold-Constrained Hyper-Connections (mHC)

**Manifold-Constrained Hyper-Connections (mHC)** replace standard residual connections throughout the network. Standard residuals add the layer input directly to the layer output. mHC instead projects residual connections onto a learned manifold, strengthening signal propagation across deep layers while preserving expressivity. The practical outcome is more stable training at scale and reduced gradient degradation in very deep networks.

### 3. Muon Optimizer

DeepSeek V4 is trained using the **Muon optimizer**, which applies Newton-Schulz iterations to approximately orthogonalize the gradient update matrix before applying it as a weight update. Compared to AdamW, Muon produces faster convergence and greater training stability — particularly important when training a 1.6T parameter model where optimizer instability would be catastrophic.

Together, CSA+HCA, mHC, and Muon explain how DeepSeek V4 achieves near-frontier benchmark scores while remaining deployable at far lower cost than dense models of similar capability.

## What are the DeepSeek V4 benchmarks?

DeepSeek published benchmark results for both models. These are **vendor-reported** numbers — independent labs are still reproducing them, and the US Center for AI Standards and Innovation (CAISI) has published an evaluation that is more conservative than DeepSeek's self-reported figures. Treat the table below as DeepSeek's claimed best single-run performance (V4-Pro Max, extended inference compute), not yet third-party-verified.

### V4-Pro Max benchmarks (vendor-reported)?

| Benchmark | DeepSeek V4-Pro Max | What It Measures | 
|---|---|---|
| MMLU-Pro | 87.5 | Graduate-level knowledge across 14 domains | 
| GPQA Diamond | 90.1 | Expert-level science questions (PhD difficulty) | 
| LiveCodeBench | 93.5 | Competitive programming on unseen problems | 
| SWE Verified | 80.6 | Real GitHub issue resolution | 
| Codeforces Rating | 3206 | Competitive programming ELO (top 0.03% range) | 
| HMMT | 95.2 | Harvard-MIT Math Tournament problems | 
| BrowseComp | 83.4 | Multi-step web research and retrieval | 

### V4-Flash benchmarks (vendor-reported)?

| Benchmark | DeepSeek V4-Flash | 
|---|---|
| MMLU-Pro | 86.2 | 
| GPQA Diamond | 88.1 | 
| LiveCodeBench | 91.6 | 
| SWE Verified | 79.0 | 
| Codeforces Rating | 3052 | 

The gap between Flash and Pro is narrow — Flash gives up roughly 1-2 points across most benchmarks in exchange for a large reduction in API cost. For most production applications that do not require frontier-level reasoning, V4-Flash is the right default. These are vendor-reported scores, and independent harnesses tell a more mixed story: on Vals' neutral harness V4-Pro-0813 places 2nd on SWE-bench Verified at 96.40% at $0.022 per test, but scores 54.68% on the reference Terminus 2 Terminal-Bench harness against DeepSeek's claimed 87.9%, and lands near the hallucination floor on AA-Omniscience (0.83 vs Claude Opus 5's 37.07). Validate against your own task suite before standardizing on any of these numbers for procurement decisions.

## What is the DeepSeek V4 API pricing?

Both models are available immediately via the **DeepSeek API** using the model IDs `deepseek-v4-pro` and `deepseek-v4-flash`. Pricing follows the standard cache-hit / cache-miss structure. The figures below are DeepSeek's published API rates and hold until **16:00 UTC on August 16, 2026**, when a scheduled price change takes effect — see the table underneath.

| Model | Input (cache miss) | Input (cache hit) | Output | 
|---|---|---|---|
| deepseek-v4-pro | $0.435 / 1M tokens | $0.003625 / 1M tokens | $0.87 / 1M tokens | 
| deepseek-v4-flash | $0.14 / 1M tokens | $0.0028 / 1M tokens | $0.28 / 1M tokens | 

At cache-miss rates, V4-Pro is dramatically cheaper than comparable closed frontier models for equivalent throughput, and V4-Flash at $0.14/M input is competitive with the cheapest frontier-class models available anywhere. Both models support **thinking mode** and **non-thinking mode** via the API. Thinking mode adds chain-of-thought reasoning tokens before the final response — useful for math and code generation where reasoning quality matters more than latency.

**Pricing changes on August 16, 2026.** From 16:00 UTC that day DeepSeek splits both models into peak (01:00–04:00 and 06:00–10:00 UTC) and off-peak tiers. Off-peak is not a discount on today’s rate — it is half of a raised peak, and every tier bills above the current flat price: roughly **1.96x on input, 2.94x on output and 7.84x on cache reads**.

| Model / tier (from 2026-08-16) | Input (cache miss) | Input (cache hit) | Output | 
|---|---|---|---|
| `deepseek-v4-pro` — off-peak | $0.66 / 1M | $0.022 / 1M | $1.98 / 1M | 
| `deepseek-v4-pro` — peak | $1.32 / 1M | $0.044 / 1M | $3.96 / 1M | 
| `deepseek-v4-flash` — off-peak | $0.22 / 1M | $0.007 / 1M | $0.66 / 1M | 
| `deepseek-v4-flash` — peak | $0.44 / 1M | $0.014 / 1M | $1.32 / 1M | 

Full working in the August 2026 DeepSeek price change breakdown. Concurrency, not uptime, is the practical ceiling on both models: **500 concurrent requests on V4-Pro, 2,500 on V4-Flash**, against 99.99% measured uptime on DeepSeek’s own endpoints.

## How do you use DeepSeek V4 via the API?

The DeepSeek API is OpenAI-compatible. You can use it with any library that targets the OpenAI API format by swapping the base URL and model name.

```
from openai import OpenAI
client = OpenAI(
    api_key="your-deepseek-api-key",
    base_url="https://api.deepseek.com/v1"
)
response = client.chat.completions.create(
    model="deepseek-v4-pro",
    messages=[
        {"role": "user", "content": "Explain the CSA+HCA hybrid attention mechanism in DeepSeek V4."}
    ],
    max_tokens=2048
)
print(response.choices[0].message.content)
```
To use V4-Flash instead, replace `deepseek-v4-pro` with `deepseek-v4-flash`. No other changes are needed. If you are still on the legacy `deepseek-chat` / `deepseek-reasoner` aliases, this same one-line change is your migration before the July 24, 2026 deadline. For local deployment of V4-Flash, see the Run DeepSeek V4 Flash Locally: Full 2026 Setup Guide for hardware requirements and setup instructions.

### How do you enable thinking mode?

```
response = client.chat.completions.create(
    model="deepseek-v4-pro",
    messages=[
        {"role": "user", "content": "Solve this differential equation step by step..."}
    ],
    extra_body={"thinking": True},
    max_tokens=8192
)
```
Thinking mode is billed at the same per-token rate as standard output. Budget for 2-5x more output tokens when enabling it on complex tasks.

## Should you upgrade from DeepSeek V3.2 to V4?

If you are currently running DeepSeek V3.2 via the API, you do not have a choice about *whether* to migrate — the legacy aliases stop working July 24, 2026. The only question is which V4 model you move to.

The architectural changes matter most at long context. At 1M tokens, V4-Pro uses 10% of V3.2's KV cache. For applications with large system prompts, long chat histories, or document-grounded generation, V4 will be substantially cheaper and faster than V3.2 at the same context length.

For short-context workloads under 32K tokens, the per-token difference is smaller, but V4-Pro's benchmark improvements in code generation and STEM reasoning still make it the better default unless cost is the binding constraint — in which case V4-Flash provides nearly equivalent output quality at a fraction of the price. Re-test against the GA checkpoint: V4-Pro-0813 changed inference behaviour (speculative decoding, new reasoning-effort levels) without changing the model name.

## What does the MIT license mean for developers?

Both V4-Pro and V4-Flash are released under the MIT license — the most permissive open-source license available. You can:

- Download and run the weights for free, including commercial use
- Fine-tune on your own data without restriction
- Build and sell products on top of V4 without royalties
- Redistribute modified versions

V4-Flash weights are the practical self-hosting target. At 284B parameters with 13B activated per token, V4-Flash can run on a multi-GPU setup that most mid-size teams can afford. V4-Pro at 1.6T total parameters requires significant cluster capacity to serve at production latency — most teams will use the DeepSeek API for Pro and consider self-hosting only for Flash.

If you are evaluating alternatives for workloads where DeepSeek V4 is not the right fit, see DeepSeek V4 Alternatives: Qwen, Kimi, MiniMax, GPT, and Claude Compared (2026) for a structured comparison.

## Summary: should you switch to DeepSeek V4?

For most development teams, the answer is yes — and for legacy-alias users, it is mandatory before July 24, 2026. DeepSeek V4 delivers strong vendor-reported benchmark performance at a fraction of the cost of closed-source competitors, ships with open weights under a permissive license, and introduces real architectural advances in long-context efficiency that directly reduce API bills for production workloads.

- **For new projects:** Start with`deepseek-v4-flash` . Upgrade to Pro only if benchmarks reveal a quality gap on your specific task.
- **For existing V3.2 / legacy-alias users:** Migrate now. The API is compatible, and the July 24 deadline is firm.
- **For self-hosting:** V4-Flash is the practical target. V4-Pro requires cluster-scale hardware to serve at competitive latency.

## FAQ

### Is DeepSeek V4 released?

Yes. DeepSeek V4 was released on April 24, 2026, in two variants — V4-Pro and V4-Flash — both available via the DeepSeek API and as open weights on Hugging Face. The preview label is gone: V4-Pro reached general availability on **August 13, 2026** with the V4-Pro-0813 checkpoint.

### When do deepseek-chat and deepseek-reasoner stop working?

The legacy `deepseek-chat` and `deepseek-reasoner` model aliases are fully retired and inaccessible after July 24, 2026, 15:59 UTC, per DeepSeek's official API change log. Until then they route transparently to V4-Flash. There is no announced extension.

### What do the legacy aliases route to before the deadline?

During the grace period, `deepseek-chat` routes to V4-Flash in non-thinking mode and `deepseek-reasoner` routes to V4-Flash in thinking mode. Neither routes to V4-Pro, so moving to `deepseek-v4-pro` is an upgrade, not a like-for-like swap.

### What are the DeepSeek V4 model specs?

V4-Pro is a 1.6T-parameter MoE with 49B activated per token; V4-Flash is 284B total with 13B activated. Both support a 1M-token context window, up to 384K output tokens, dual thinking/non-thinking modes, and ship under the MIT license.

### How much does the DeepSeek V4 API cost?

As of June 2026, DeepSeek's official pricing for V4-Pro is **$0.435 per 1M input tokens** (cache miss), **$0.003625 per 1M cache hit**, and **$0.87 per 1M output tokens**. V4-Flash is **$0.14/M input**, **$0.0028/M cache hit**, **$0.28/M output**. The previous V4-Pro list rates of $1.74/$3.48 per 1M (in effect until late May 2026) were superseded on 2026-05-22 when DeepSeek made the 75% discount permanent — there is no expiry date. See the official pricing page; the change was also discussed on Hacker News. Those rates hold until **16:00 UTC on August 16, 2026**, when both models move to peak/off-peak tiers — V4-Pro to $0.66 / $0.022 / $1.98 off-peak and $1.32 / $0.044 / $3.96 at peak, V4-Flash to $0.22 / $0.007 / $0.66 and $0.44 / $0.014 / $1.32.

### Are the DeepSeek V4 benchmarks independently verified?

Not fully. The benchmark scores DeepSeek published (e.g., 80.6 SWE-bench Verified, 93.5 LiveCodeBench for V4-Pro Max) are vendor-reported. Independent reproduction is ongoing, and at least one government evaluation has been more conservative. Validate on your own task suite before relying on them.

### How do I migrate my code to DeepSeek V4?

Replace the legacy model string with `deepseek-v4-pro` or `deepseek-v4-flash`. The API stays OpenAI-compatible, so no SDK or base-URL change is required — it is a one-line change per call. Re-test prompts because V4-Pro is an upgrade over the old default.

If you are hiring vetted remote developers experienced with DeepSeek V4, open-weight LLM deployment, or migrating production systems off deprecated model aliases ahead of a hard cutover, Codersera connects you with engineers who have shipped exactly this kind of work. See codersera.com/hire to extend your engineering team with remote-ready talent.
