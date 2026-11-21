---
id: collect-261121-veille-ia/xiaomi-mimo/mimo-v2-6-pro-vs-mimo-v2-6-flash
title: "MiMo V2.6 Pro vs MiMo V2.6 Flash"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: ["2026-09-22"]
keywords: ["mimo", "tokens", "output", "input", "flash", "xiaomi", "mimo-v2", "cache"]
source: https://www.thequery.in/comparisons/mimo-v2-6-pro-vs-mimo-v2-6-flash
source_anchor: ""
source_lines: [1, 92]
section: "Xiaomi_MiMo"
---
# MiMo V2.6 Pro vs MiMo V2.6 Flash

Updated September 30, 2026

| Specifications |  |  | 
|---|---|---|
| Developer | Xiaomi MiMo | Xiaomi MiMo | 
| Release date | 2026-09-22 | 2026-09-22 | 
| API model ID | mimo-v2.6-flash | mimo-v2.6-pro | 
| Total / active parameters | 309B / 15B | 1.02T / 42B | 
| Architecture | Sparse MoE, 48 layers (39 sliding-window, 9 global), 256 routed experts | Sparse MoE, 70 layers (60 sliding-window, 10 global), 384 routed experts, 8 activated per token | 
| Context window | 1M tokens | 1M tokens | 
| Max output | 128K tokens | 128K tokens | 
| Reasoning / effort | Deep thinking, on by default | Deep thinking, on by default | 
| Vision encoder | Not separately documented | 681M MiMo ViT, 28 layers, hidden 1280 | 
| Audio encoder | Not separately documented | 308M AudioTokenizer plus a 127M audio patch encoder | 
| Speculative decoding | Not separately documented | 5-layer sliding-window multi-token-prediction drafter, 7 tokens per pass | 
| Weights / license | Open source. XiaomiMiMo/MiMo-V2.6-Flash-RL, MIT License; RL datasets and environments Apache-2.0 | Open source. XiaomiMiMo/MiMo-V2.6-Pro-RL, MIT License; RL datasets and environments Apache-2.0 | 
| Rate limits | 100 RPM, 10M TPM | 100 RPM, 10M TPM | 
| Pricing |  |  | 
| Input / 1M tokens (cache miss) | **$0.14** | $0.435 | 
| Cached input / 1M | **$0.0028** | $0.0036 | 
| Cache miss-to-hit discount | ~50x | ~120x | 
| Output / 1M tokens | **$0.28** | $0.87 | 
| Batch API input, cache miss | **$0.07** | $0.2175 | 
| Batch API output | **$0.14** | $0.435 | 
| China pricing, cache miss / output | ¥1 / ¥2 | ¥3 / ¥6 | 
| Cache write | Free for a limited time | Free for a limited time | 
| Price change vs V2.5 series | None | None | 
| Capabilities & access |  |  | 
| Text input | Yes | Yes | 
| Image / vision input | Yes, native | Yes, native | 
| Audio input | No. Artificial Analysis lists Flash as text and image only | Yes, native. Artificial Analysis lists text, image, speech and video | 
| Video input | No, on the same reading | Yes, native. Artificial Analysis lists text, image, speech and video | 
| Text output | Yes | Yes | 
| Tool / function calling | Yes, with streaming, structured output and web search | Yes, with streaming, structured output and web search | 
| Context caching | Yes | Yes | 
| Batch API | Yes, at half the real-time rate | Yes, at half the real-time rate | 
| Token Plan coverage | Yes | Yes | 
| MiMo-V2.6-Pro-UltraSpeed variant | None | Yes, at up to 20x output speed for 10x the price. It should be treated as not production-ready: Xiaomi leaves its capability column blank, it has no Batch API, no published rate limits, no Token Plan coverage, and Artificial Analysis has no evaluation page for it, so nothing about it is independently verified | 
| API access | https://api.xiaomimimo.com/v1, both OpenAI and Anthropic protocols | https://api.xiaomimimo.com/v1, both OpenAI and Anthropic protocols | 
| Product access | Xiaomi AI Studio, MiMo Code, MiMo Desktop, the MiMo API platform, and OpenRouter | Xiaomi AI Studio, MiMo Code, MiMo Desktop, the MiMo API platform, and OpenRouter | 
| Weights / license | Open source, unmodified MIT | Open source, unmodified MIT | 
| Model behavior |  |  | 
| Primary focus | High-frequency calls and large-scale professional office tasks; the best balance in the series | The most capable model Xiaomi has shipped, for complex projects, long-horizon work, cybersecurity and research | 
| RL steps / wall clock | 30 steps in under 6 days | 30 steps in under 6 days | 
| Reported RL training cost | ~$0.85M | ~$2.62M | 
| Trajectories | ~750,000 across the series | ~750,000 across the series | 
| DeepSWE training curve (step 1 to 30) | 48.8 to 65.7, about 17 points | 58.4 to 72.6, about 14 points. The post's prose reports 48.8 to 65.68 and 58.4 to 72.57 against 48.7 and 65.7 in its own step table | 
| Training config | 1,568 samples per update, 1M context, 3.5 to 3.7B tokens per step, fully asynchronous | Same configuration and same groupwise reward synthesis approach | 
| Artificial Analysis Intelligence Index v4.3.2 | **38** , at $0.06 per index task | **46** , at $0.13 per index task and the highest open-weights result, ahead of GLM-5.3 at 45 and Kimi K3 at 44. Xiaomi cites 46.32; the gap is rounding | 
| Generation profile | Described by Xiaomi as the token-efficient end of the series, but Artificial Analysis measures it as the more verbose of the two: 240M output tokens across its index run against a 140M median. It is the faster of the two at 55 tokens per second | Artificial Analysis describes it as slow and verbose, generating 140M tokens across its index run at 41.1 tokens per second and a 4.24-second time to first token | 
| Pass-rate improvement over training | The post reports 25% and 12% across the two models without mapping either figure unambiguously to one of them | Same | 
| Safety / approvals | No safety evaluation published. CyberGym 95.1, ExploitBench 25.3, SEC Bench Pro 47.5 | No safety evaluation published. CyberGym 94.0, ExploitBench 47.9, SEC Bench Pro 66.3 | 
| Coding |  |  | 
| DeepSWE v1.1 | 67.9% | **71.9%** | 
| ProgramBench | 26.0% | **26.5%** | 
| MiMo Code Bench (in-house, unreproducible) | 61.2% | **63.2%** | 
| Terminal-Bench 4.0 | 28.8% | **34.9%** | 
| Knowledge |  |  | 
| GDPval 2.1 (AA, Elo) | Not reported | **1673 Elo** | 
| Agentic & computer use |  |  | 
| Toolathlon-verified | 73.6% | **76.9%** | 
| AutomationBench v1.0.6 | 52.3% | **53.1%** | 
| Agents' Last Exam | 27.6% | **31.6%** | 
| OSWorld-Verified | 80.8% | **82.0%** | 
| Professional |  |  | 
| JobBench | 61.2% | **62.0%** | 
| Multimodal |  |  | 
| MiMo Visual Coding (in-house, unreproducible) | 71.5% | **72.3%** | 
| Cyber capability |  |  | 
| CyberGym | **95.1%** | 94.0% | 
| ExploitGym | 6.0% | **17.8%** | 
| ExploitBench | 25.3% | **47.9%** | 
| SEC Bench Pro | 47.5% | **66.3%** | 
| MiMo Cyber Bench (in-house, unreproducible) | 77.2% | **81.7%** | 

## What changes when you switch

Switching Flash to Pro is a model-string change, not a migration. Both speak the OpenAI and Anthropic protocols from the same base URL, both are 1M context and 128K output, both cap at 100 RPM and 10M TPM, and both are covered by the same Token Plan. Neither is deprecated by the other. `mimo-v2.5-pro` and `mimo-v2.5` are deprecated at 10:00 Beijing time on October 21, 2026, so the real migration question in the series is leaving V2.5 rather than choosing within V2.6.

What does change is the bill and the latency profile. Pro costs roughly 3.1x Flash per million input and output tokens on a cache miss, and the batch path halves both, so batch inference is the cheapest way to buy Pro's scores. The cached-input rates behave differently: Pro's $0.0036 against a $0.435 miss is about a 120x discount, while Flash's $0.0028 against $0.14 is about 50x. An agent loop that re-sends the same repository or document context every turn leans on that discount, and at high cache-hit rates the effective gap between the two narrows well below 3x. Low-reuse workloads pay the full 3x.

One caution on the comparison table itself. Both models are MIT licensed with the reinforcement learning code, training environments and serving recipes published alongside them, which is unusual and genuinely useful. But no safety evaluation accompanied the release for either. On downloadable weights, Flash scores higher than Pro on CyberGym and far lower on ExploitBench, a reversal of more than 20 points, so these narrow vulnerability benchmarks do not describe one shared cyber level. Treat them as properties of the weights rather than of the hosted product, and apply whatever controls your own deployment needs.

## Bottom line

MiMo V2.6 Pro and MiMo V2.6 Flash are the same reinforcement learning recipe at two compute budgets, which is the most useful thing to know about the choice. Pro costs about 3x as much and leads on most shared benchmarks, but usually by a few points: DeepSWE 71.9 against 67.9, Toolathlon 76.9 against 73.6, OSWorld 82.0 against 80.8. JobBench is under a point apart, and Flash leads on CyberGym.

For most production workloads Flash is the right default. It clears the bar for high-frequency office and document work at a third of the price, and Xiaomi states its advantage plainly as token efficiency. Buy Pro where its specific strengths matter rather than as a general upgrade: long-horizon coding agents in real repositories, vulnerability discovery and security research, multimodal analysis where audio and video input alongside a 1M window matters together, and the workloads Artificial Analysis measured at 140M output tokens, where Flash's efficiency may or may not survive contact with the harder tasks. The honest answer for most teams is to run Flash in production and evaluate Pro on the specific tasks where its benchmarks actually separate.

Two caveats carry across both. Three of the shared benchmark rows are Xiaomi in-house results that no third party can reproduce, and two of those three are among the closest rows on the page, so the narrow margins there are the least trustworthy numbers in the table. And no safety evaluation was published for either model, which matters more than it would for a closed one because both are downloadable under an unmodified MIT license.
