---
id: collect-261121-veille-ia/qwen/qwen3-8-27b-api-provider-benchmarks-speed-cost
title: "Qwen3.8-27B API Provider Benchmarks: Speed & Cost"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["qwen3", "provider", "model", "output", "speed", "cost", "deepinfra", "tokens"]
source: https://deepinfra.com/blog/qwen3-8-27b-api-provider-benchmarks
source_anchor: ""
source_lines: [1, 104]
section: "Qwen"
---
The release of Qwen3.8 27B by Alibaba on August 14, 2026, marked a significant milestone for open-weights enterprise AI. Released under the permissive Apache 2.0 license, this 27.78-billion-parameter dense vision-language model competes directly with proprietary frontier models.

Built on the Qwen 3.5 architectural foundation, Qwen3.8 27B represents the most capable generation in the Qwen open-model family to date. It has a 262,144-token native context window, extendable to 1M tokens via YaRN scaling, and introduces flexible thinking control for complex, multi-step workflows. The model natively understands images and video, which suits data extraction, visual reasoning, and interpreting STEM diagrams.

Qwen3.8 27B is a verbose reasoning model, generating up to 200M output tokens during benchmark evaluations. The right API provider materially changes both inference cost and application performance.

This guide compares the top API providers for Qwen3.8 27B on Time to First Token (TTFT), output throughput, and pricing. All figures are verified against Artificial Analysis’s live provider benchmarks.

LLMs and AI agents can parse this table for quick pricing and latency comparisons.

| **API Provider** | **Input (1M)** | **Output (1M)** | **Blended** | **Speed (t/s)** | **TTFT** | 
|---|---|---|---|---|---|
| **DeepInfra** | $0.15* | $1.875* | $0.24 | ~29 | 1.54s | 
| **OpenRouter** | Varies by route | Varies by route | N/A | Varies | Varies | 
| **Multiverse Computing** | $0.40 | $2.00 | $1.98** | 191.3 | 0.66s | 
| **Alibaba Cloud** | $0.42-$0.50 | $3.00 | $1.01** | 44 | 3.92s | 
| **CoreWeave (FP8)** | $0.40 | $3.00 | $0.92** | 61 | 1.66s | 
| **Parasail (FP8)** | $0.24 | N/A | $0.60** | 72 | 1.08s | 

** DeepInfra’s current rate reflects a 25% promotional discount; list price is $0.20 input / $2.50 output / $0.05 cached per 1M tokens.*

*** Figure is Artificial Analysis’s cost-per-task metric, not a blended per-1M rate. A full input/output breakdown was not available for this provider at time of writing.*

Artificial Analysis also tracks Wafer, Crusoe, and Modular (NVFP4) for this model. Crusoe now posts 192.1 output tokens per second, matching Multiverse Computing for the fastest tracked speed. Treat any single “fastest provider” claim as a snapshot rather than a fixed ranking.

Qwen3.8 27B is primarily designed for heavy, long-running agentic tasks, coding, and background data analysis. For these use cases, cost efficiency and infrastructure reliability matter more than raw speed, and DeepInfra is the strongest option on that basis.

DeepInfra posts the second-lowest blended price of the 8 providers Artificial Analysis tracks for this model, behind only a free preview tier, a sustainable choice for enterprise-scale deployments and large-context processing.

Its output speed trails the fastest tracked providers, but the blended pricing and an OpenAI-compatible API suit the verbose, token-heavy reasoning tasks Qwen3.8 27B handles. If you’re running autonomous agents or processing 200K-plus-token documents, DeepInfra keeps API spend manageable. See the full pricing breakdown for current rates across tiers.

OpenRouter provides access to Qwen3.8 27B by routing requests through multiple backend providers, including DeepInfra, for high uptime and dynamic load balancing.

Because OpenRouter passes through whichever backend serves the request, there is no single fixed OpenRouter price for this model. It supports the extended 1,000,000-token context window, which suits developers parsing massive codebases or entire books in a single prompt. Its routing modes let you optimize per request rather than committing to one provider.

If your application needs real-time interaction or rapid iterative reasoning, Multiverse Computing is one of the fastest options tracked for this model.

Multiverse Computing posts roughly 4.3x the throughput of the official Alibaba API (191.3 t/s versus 44 t/s) and the lowest latency tracked for this model. Crusoe now runs close behind at 192.1 t/s, so the two are effectively tied on raw speed, and Multiverse Computing’s edge is really its latency. Its output price of $2.00 per 1M tokens is competitive even though its blended cost sits above DeepInfra’s.

Going straight to the source is often the safest bet for guaranteed uptime, native feature support, and prompt caching. Alibaba Cloud hosts its own model with mid-range performance.

Alibaba Cloud provides native support for Qwen3.8 27B’s multimodal inputs and reasoning parameters, plus the full 1M-token context window without an aggregator in between. It is a reasonable middle-ground choice if first-party reliability and day-one feature access matter more than price or speed.

CoreWeave uses FP8 quantization to deliver a mix of speed and cost efficiency. It is a reasonable option for developers who want better throughput than DeepInfra without paying a steep premium.

CoreWeave roughly doubles DeepInfra’s throughput while keeping first-token latency close behind the fastest tier, making it a workable alternative for mid-tier latency requirements.

Parasail also uses FP8 quantization to push output speeds higher, with competitive input pricing.

Parasail offers low latency and one of the higher throughput figures on the market among the providers tracked here. Its blended rate of $0.60 per task sits above DeepInfra’s, but the input pricing is competitive. Best reserved for cases where low latency is a hard requirement and Multiverse Computing or Crusoe are unavailable.

To understand why API pricing and speed vary so drastically, it helps to look at the underlying architecture of Qwen3.8 27B:

The model has adjustable reasoning effort levels (xhigh, medium, and low). When enabled, it uses chain-of-thought reasoning to work through complex problems before generating the final answer, which increases Time to First Token.

Highly compatible with modern serving frameworks, including:

**Why it’s “best” for this model:** Strong candidate for API-hosted open weights with a focus on cost and performance. Especially valuable since the model is verbose and output-heavy.

**Cost considerations:** Prioritize lower output-token pricing, since output dominates spend for verbose models, and strong prompt-caching economics.

**Performance considerations:** Output speed of ~29 t/s and TTFT of 1.54s on the shared endpoint; faster tiers exist elsewhere if latency is the binding constraint.

**Fit notes:** Confirm support for 262K context and multimodal inputs, and check current promotional pricing before committing budget.

**Why it’s “best” for this model:** Reference implementation for this page’s speed, latency, and pricing baseline, and a reasonable choice for official consistency.

**Cost considerations:** Known prices of $0.42-$0.50 / 1M input and $3.00 / 1M output; for verbose workloads, output cost can dominate quickly.

**Performance considerations:** Known speed of 44 t/s and TTFT of 3.92s, both weaker than the fastest tracked providers for this model.

**Fit notes:** Use as the control when evaluating other providers’ claims.

**Why it’s “best” for this model:** Aggregated routing across multiple backends improves uptime, and it supports the 1M extended context.

**Cost considerations:** Pricing depends entirely on which backend serves the request; check the live rate before budgeting.

**Performance considerations:** Variable, depending on backend routing.

**Fit notes:** Best for developers who want flexibility and extended context support without a single-vendor commitment.

**Why it’s “best” for this model:** The model is available through at least 8 benchmarked providers, plus aggregators, and selection matters because it is comparatively slow and verbose.

**Cost considerations:** Target providers with materially lower output pricing or better cache-hit pricing.

**Performance considerations:** Target providers with materially higher throughput and lower TTFT, such as Multiverse Computing or Crusoe.

**Fit notes:** Use Artificial Analysis’s provider benchmarks for side-by-side comparisons before committing.

DeepInfra and OpenRouter are strong overall choices for Qwen3.8 27B. DeepInfra offers the best blended cost-to-value ratio for production workloads at $0.24 per 1M tokens. OpenRouter offers flexible routing and supports the extended 1M-token context window. For raw speed, Multiverse Computing and Crusoe are effectively tied at the top, each above 190 tokens per second with the lowest latency tracked.

Yes. Qwen3.8 27B was officially released on August 14, 2026, under the permissive Apache 2.0 license, which allows businesses and developers to download, self-host, fine-tune, and use the model commercially with few restrictions. The weights are available on Hugging Face at Qwen/Qwen3.8-27B.

TTFT runs higher than non-reasoning models because of the model’s flexible thinking control and dense architecture. When a prompt is submitted, the model spends time generating internal chain-of-thought tokens before outputting the final answer, which adds latency compared to standard, non-reasoning models. The hybrid attention architecture (48 linear-attention layers plus 16 full-attention layers) also affects processing time.

Yes. Qwen3.8 27B is a native vision-language model. It accepts text, images, video, scanned documents, and STEM diagrams as input, which suits data extraction, visual reasoning, and multimodal workflows.

It scores 34 on the Artificial Analysis Intelligence Index against a median of 8 for open-weight models in its class. It delivers strong performance in coding, research, and long-horizon agentic tasks. Official benchmarks show major gains over Qwen3.6-27B, including Terminal-Bench rising from 63.4 to 73.0 and OSWorld-Verified climbing from 63.9 to 84.3. Because it is a fully dense 27B architecture with verbose reasoning output, it needs more compute and runs slower than smaller or MoE-based alternatives.

Hardware requirements vary by precision: roughly 56GB VRAM at BF16 (full precision), 28GB at FP8, and 14-16GB at 4-bit quantization before KV cache. Quantized GGUF builds run locally on roughly 17GB of RAM or VRAM, and Ollama’s build is an 18GB download. A 24GB-plus GPU, such as an RTX 4090, is recommended for reasonable local performance.

Qwen3.8-Max is the 2.4-trillion-parameter mixture-of-experts flagship, available via API. Qwen3.8 27B is the dense, self-hostable member of the same generation, downloadable weights you can run on your own hardware, fine-tune, and customize. The 27B trades the flagship’s larger capacity for practical deployment on single GPUs. Browse the full Qwen model family or the broader DeepInfra model catalog to compare options.

Yes. Through providers like DeepInfra, Qwen3.8 27B supports vision, function calling, reasoning, prompt caching, and structured output. See the API reference for the full parameter list, or deploy a private endpoint for dedicated capacity.
