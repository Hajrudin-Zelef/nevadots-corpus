---
id: collect-261121-veille-ia/glm-zhipu/glm-5-3-flash-vs-opus-price-benchmarks-hardware
title: "GLM-5.3-Flash vs Opus: Price, Benchmarks, Hardware"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: ["2026-08-26"]
keywords: ["flash", "glm-5", "model", "input", "tokens", "opus", "output", "price"]
source: https://blog.invidelabs.com/glm-5-3-flash/
source_anchor: ""
source_lines: [1, 152]
section: "GLM_Zhipu"
---
Z.ai released GLM-5.3-Flash on August 26, 2026: a 320-billion-parameter mixture-of-experts model with 18B active per token, a 1,048,576-token context window, text, image and video input in Z.ai's API, and weights published under the MIT license as `zai-org/GLM-5.3-Flash`.

The pricing is what gets attention. Z.ai lists the model at $0.15 per million input tokens and $0.50 per million output tokens. Claude Opus 4.8 lists at $5 and $25, as does Opus 5. GLM-5.3-Flash is roughly 33 times cheaper on input and 50 times cheaper on output, and Z.ai's launch post positions it as "approaching Claude Opus 4.8 on coding and agentic benchmarks."

Three weeks of independent evidence now let us ask whether the discount survives real work. An ExploitBench run favoured GLM under a token budget rather than the benchmark's usual turn cap, while Polyform found it viable for regular work but not for its hardest tier.

There is also a catch for anyone planning to skip the API. Despite the 18B active parameters, the native FP8 checkpoint needs about 306 GiB of weight memory before runtime and KV-cache overhead. "Flash" describes the serving economics. The download is another matter.

## The model arrived with a week of traffic behind it

Z.ai ran the model anonymously on OpenCode and OpenRouter as Ox Alpha before release, and says it "quickly became the most popular model of the week." The listing surfaced on Hacker News on August 20, and developers spent days fingerprinting its behaviour against the GLM family before Bloomberg reported Z.ai's confirmation on the 26th in a story discussed on Hacker News.

The South China Morning Post reported the next day that, according to Zhipu, the Ox Alpha run served 62 trillion tokens across both platforms on a cluster of 100,000 domestically produced chips, and that Zhipu's Hong Kong shares closed up more than 12 percent. Those figures are the company's and are not independently audited. Z.ai's own post says only that the traffic ran on Chinese AI chips.

## GLM-5.3-Flash pricing: low, but not uniquely low

Public API rates checked September 17:

| Model | Input / 1M tokens | Output / 1M tokens | 
|---|---|---|
| GLM-5.3-Flash | $0.15 | $0.50 | 
| Qwen3.8 Flash (International, Singapore) | $0.15 | $0.47 | 
| DeepSeek V4.1 Flash (off-peak) | $0.15 | $0.60 | 
| GPT-5.6 Terra | $2.00 | $12.00 | 
| Claude Opus 4.8 | $5.00 | $25.00 | 

Alibaba Model Studio's international Singapore rate puts Qwen3.8 Flash level with GLM on input and slightly lower on output, while DeepSeek V4.1 Flash sits in the same bracket at its off-peak rate. DeepSeek doubles both figures during its weekday peak windows of 01:00-04:00 and 06:00-10:00 UTC. The gap only becomes dramatic against frontier pricing. Z.ai also charges $0.03 per million cached input tokens, with cached-input storage currently free for a limited period.

The route matters more than the list price suggests. OpenRouter lists close to 30 provider endpoints for this model, spanning a six-fold spread on input when checked in mid-September: DeepInfra at the bottom at $0.075 and $0.25 under a 50%-off promotion, most of the field including Z.ai's own row at the $0.15 and $0.50 list rate, Modal at the top at $0.45 and $1.50. Maximum output ranges from 128K to roughly 1.18M tokens, and Z.ai's own row caps it at 131,072. OpenRouter's Balanced mode considers both price and speed, so the real exposure is pinning a provider for throughput or context and absorbing six times the input cost.

The chat template adds another cost lever. The vLLM recipe notes that effort resolves to `max` unless `reasoning_effort` is explicitly set to `low` or `high`, and max is the highest token-cost mode. A deployment that never passes the parameter pays for the most expensive reasoning setting by default.

None of those numbers is the one that decides an agent's bill, though. That is the cache line, and it sits below the fold on most pricing pages.

## GLM-5.3-Flash prefix caching: where agent costs are decided

An agent resends most of the same prompt every turn. The system prompt, the tool definitions, the repository context, the conversation so far. Only the tail changes.

Without caching, the server recomputes attention over that entire prefix every turn, and you pay full input rate for all of it. Prefix caching stores the computed state for a prefix it has already seen, so the next request starting with the same tokens skips straight to the new part. That cuts the input line and shortens time to first token, because the work was already done.

Providers price the cached path very differently.

| Route for GLM-5.3-Flash | Input / 1M | Cache write / 1M | Cache read / 1M | 
|---|---|---|---|
| Z.ai direct | $0.15 | not charged separately | $0.03 | 
| DeepInfra (50% promo) | ~~$0.15~~ $0.075 | not charged separately | ~~$0.03~~ $0.015 | 
| CoralBricks | $0.15 | $0.23 | Free | 

CoralBricks prices caching the other way around. Cached reads cost nothing on every model it serves, paid for by charging 1.5x the input rate to put a prefix into cache in the first place. The company is upfront that this is a trade: you pay more once, then read it back for free, and it only pays off if you reuse the prefix. It claims a 98% cache hit rate on its pricing page.

Writing a prefix costs $0.23 against a $0.15 input rate, so the premium is $0.08 per million tokens. Against Z.ai's $0.03 cached read that pays off after about three reads of the same prefix. With DeepInfra's current promotional rate the same math lands around ten reads. A coding agent that replays its context every turn clears either bar within one session. A service answering one-shot queries with no shared prefix never clears it and should stay on a cheaper input rate.

CoralBricks serves an NVFP4 build and DeepInfra an FP4 one, while Z.ai serves the native FP8 checkpoint.

## GLM-5.3-Flash benchmarks put it near Opus, unevenly

Z.ai's published table compares against GLM-5.2, DeepSeek-V4-Vision-Exp, Opus 4.8, GPT-5.6 Terra and Gemini 3.7 Flash.

Across the eight coding and agentic rows, GLM-5.3-Flash leads Opus 4.8 on DeepSWE v1.1 (63.4 against 58.0), AutomationBench v1.0.6 (48.8 against 41.0), Toolathlon Verified (78.4 against 76.2) and GDPval-AA v2 (1773 against 1582). It trails on Terminal-Bench 2.1 (84.3 against 85.0), HLE with tools (55.3 against 57.9) and Agents' Last Exam (26.3 against 27.0), all narrowly, and on NL2Repo it trails badly at 56.3 against 69.7. Z.ai publishes that last row without comment. On its in-house Z.ai Code Bench v1.0 the model reaches 29.0 against Opus 4.8's 29.5 at max effort.

Each row carries its own harness, context limit and sampling settings, documented in the post's footnotes. Four wins and four losses, one of them by thirteen points, is a different claim from general parity.

Artificial Analysis scores the model 42 on its Intelligence Index. Its September 25 Z.ai API measurement is about 43 output tokens per second, a live figure that can change. The site's direct flagship comparison lists $280 to run the full index.

Z.ai's launch post claims a different number: 57, at $0.045 per task on discounted pricing. That figure is Z.ai's own, measured against Intelligence Index v4.1.1, and the current 42 comes from v4.3. Articles quoting 57 are repeating the company's claim under an older methodology.

The model generated 181M output tokens across the Artificial Analysis index. That output contributes to task cost alongside its per-token price.

## An independent test that favours the cheap model

Generality published an ExploitBench evaluation by James Mann pitting GLM-5.3-Flash against Claude Mythos Preview. The page is dated only "September 2026" and reached Hacker News on the 14th.

The two models were held to different limits. GLM-5.3-Flash was given a budget of one billion tokens per vulnerability. The comparison runs came from the original ExploitBench implementation, which caps turns rather than tokens, at 300 turns. Mann argues tokens are the more comparable limiting factor, which is defensible and also a change to the protocol.

Under that budget GLM-5.3-Flash achieved arbitrary code execution on 13 of 41 samples, while Claude Mythos Preview averaged 10 across three runs, scoring 9, 10 and 11. Four of the 41 GLM samples terminated on errors.

The cost comparison is a derived observation, downstream of the experiment's design. Using Flash's discounted output rate against Mythos Preview's historic $125 per million output tokens, a roughly 500x price difference, Mann puts parity at about 6 percent of the cost. The result is still useful. A cheap model allowed to spend far more tokens reached the same place for a fraction of the money.

Mann is explicit about the limits. A single GLM run, possible contamination, and no basis for a broad claim about cyber capability.

## What happened when a team shipped it

Polyform published a migration report on September 3 after moving parts of a Codex-backed product to GLM-5.3-Flash. Two gates matter, and they are different tests, each with its own bar.

On regular inference, 20 cases across 60 runs, GLM scored 96.7 percent expected behaviour at 100 percent safety. On Deep Work Hard, 30 runs, it reached 56.7 percent on safety and behaviour and did not advance. Their verdict was bounded: viable for the regular work, not yet viable for their Codex-based Deep Work.

The migration itself touched 150 files when Polyform unified providers behind OpenRouter, and the rollback to native providers touched 174. Their closing line is that benchmarks show you the price and capability of the engine, not the cost of making the whole machine run.

Polyform never reports that its cost per accepted outcome rose. Its narrower claim is that per-token price is the wrong unit. Cost per accepted task is what a team should track, and the evidence so far says the answer depends on which tier of work you point the model at.

## GLM-5.3-Flash hardware requirements for self-hosting

Open weights offer a way around API pricing. Everything is on Hugging Face, and there are three sources worth knowing:

```
# native FP8 checkpoint
hf download zai-org/GLM-5.3-Flash --local-dir GLM-5.3-Flash
# one rung of the GGUF ladder for llama.cpp; add mmproj for image input
hf download unsloth/GLM-5.3-Flash-GGUF \
  --include "UD-Q4_K_XL/*" "mmproj-F16.gguf" --local-dir GLM-5.3-Flash-GGUF
# NVFP4 four-bit build
hf download RedHatAI/GLM-5.3-Flash-NVFP4 --local-dir GLM-5.3-Flash-NVFP4
```
The MIT license covers commercial use, modification and redistribution of the weights and the code in the repository. The hosted API is a separate paid service. Hugging Face reported about 2.4 million downloads over the preceding 30 days when checked on September 17.

Memory is the obstacle. The 18B active-parameter figure describes how much of the mixture-of-experts network fires per token, not how much you store. The vLLM recipe puts the native FP8 checkpoint at roughly 306 GiB before runtime and KV-cache overhead, with BF16 needing about twice that. The NVFP4 variant, `RedHatAI/GLM-5.3-Flash-NVFP4`, requires Blackwell GPUs, and Hopper cannot use FP8 KV cache with this model at all and must run BF16 KV.

The recipe is also less settled than its version badge implies. Prerequisites tell you to use Docker until the integration lands in the public vLLM repo, and ask for FlashInfer 0.6.17 or newer, while the troubleshooting section asks you to verify 0.6.18 or newer.

The GLM-5.3-Flash GGUF builds published by Unsloth bring the floor down considerably, from about 93 GB at the most aggressive 1-bit rung to 200 GB for UD-Q4_K_XL and 642 GB at BF16. Those are file sizes, not complete runtime requirements, the low rungs trade real fidelity for the smaller footprint, and running them currently needs Unsloth's llama.cpp PR #27754, still open at the time of writing, or their desktop app instead of a stock build.

That build comes with conditions. It needs `-fa off`, plus `NVIDIA_TF32_OVERRIDE=0` on CUDA, for correct output, both documented in the PR. On Apple Silicon, testers also hit a long-context failure where generation collapses into one repeated token past a depth that shrinks as the configured `-c` grows: coherent to 96K at `-c 131072`, but only to about 60K at `-c 393216`. It was traced to an integer overflow in Metal's `mul_mm` kernel. A fix was proposed in PR #28210 but closed without merging on September 2, so `-ub 128` remains the workaround. On a Mac, a 1M-token window on paper is not yet a 1M-token window in llama.cpp.

Alex Ellis published a working four-node recipe showing what four DGX Spark systems can deliver on a switchless ring with a 262K context, a patched NCCL 2.30.7 and a third-party NVFP4 checkpoint.

Read its numbers carefully, because the benchmark and the recipe are not the same deployment. The September 5 baseline recorded 75.2 tokens per second decoding code, 29.8 on prose and 2,276 on a cold 64K prefill, using Red Hat NVFP4 weights, BF16 KV and `reasoning_effort=low`, inside a local experimental container the repository does not distribute. The published recipe differs on nearly every axis: LibertAI weights, FP8 E4M3 KV, `reasoning_effort` set to max, and a public image. Ellis says plainly that the speed table should not be read as a result from the public launcher.

Ellis's daily-driver log is less controlled but closer to normal use. Across 476 requests and roughly 17.5 million tokens of real agentic coding traffic, it reports around 45 tokens per second typical and up to 100 warm.

Much of the launch discussion on Hacker News turned into hardware economics. Commenters weighed DGX Spark clusters and 128 GB desktop builds against current DDR5 and NVMe prices, and one estimate put break-even against API pricing near ten years. Several people who use frontier models daily reported the quality gap in real work remains large whatever the benchmarks say.

## GLM-5.3-Flash specifications

| Specification | GLM-5.3-Flash | 
|---|---|
| Developer | Z.ai | 
| Release date | August 26, 2026 | 
| Model ID | `zai-org/GLM-5.3-Flash` | 
| Total parameters | ~320B | 
| Active parameters | 18B per token | 
| Layers | 45 | 
| Architecture | Mixture of experts, hybrid sparse and linear attention with IndexPool | 
| Context window | 1,048,576 tokens | 
| Inputs | Text, image, video | 
| Output | Text | 
| Native weights | FP8 | 
| License | MIT | 
| Z.ai API input | $0.15 / 1M tokens | 
| Z.ai API output | $0.50 / 1M tokens | 
| Native FP8 weight memory | ~306 GiB before runtime and KV overhead | 

OpenRouter exposes a `context_length` of 1,310,720, the maximum across all its routed providers, while most individual providers report 1,048,576. The checkpoint itself declares a maximum context length of 1,048,576 tokens, so treat that as the model's native window.

## Before you move traffic to GLM-5.3-Flash

Settled: Z.ai's pricing, the specifications, the 306 GiB weight figure and the GGUF ladder. The model beats Opus 4.8 on several of Z.ai's own agentic benchmarks and loses to it clearly on NL2Repo.

Open: whether the discount converts into cheaper finished work. ExploitBench shows it doing so at roughly 6 percent of the cost, under a protocol its author changed to make that comparison. Polyform found it fine for bounded work and not ready for their hardest tier. Neither result has been reproduced, and the model's above-median verbosity quietly works against the headline price.

Route a slice of real traffic to it, pass `reasoning_effort` explicitly, pin a cheap provider, then measure acceptance rate, retries and token consumption against what you pay now.

## Member discussion
