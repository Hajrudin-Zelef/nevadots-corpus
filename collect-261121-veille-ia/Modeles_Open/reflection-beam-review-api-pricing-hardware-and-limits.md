---
id: collect-261121-veille-ia/modeles-open/reflection-beam-review-api-pricing-hardware-and-limits
title: "Reflection Beam Review: API Pricing, Hardware, and Limits"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-10-16"]
keywords: ["beam", "reflection", "model", "tokens", "listed", "output", "provider", "hardware"]
source: https://aireiter.com/blog/reflection-beam-review-api-pricing
source_anchor: ""
source_lines: [1, 93]
section: "Modeles_Open"
---
If you are evaluating Reflection Beam for a coding agent, the important question is no longer whether the model exists. Reflection released the weights on October 16, 2026. The harder question is whether the cheap API route is responsive enough for production, because provider telemetry ranges from 4 to 364 output tokens per second and from 0.3 to 37.8 seconds to first token.

## Is Reflection Beam actually available now?

Reflection Beam is publicly available as an open-weight model, not just an early-access announcement. Reflection’s release post says the weights, tokenizer, reference implementation, safety report, and partner integrations are available; the Hugging Face model card lists the public repository as `reflection/Beam-1.0-501B-A23B`.

The current model card lists public BF16, FP8, and NVFP4 files under an Apache 2.0 license. Public weights solve the availability question, but not the cost of running a 501B checkpoint on your own hardware.

## The practical Beam profile: what matters beyond 501B

The Reflection Beam model card describes a sparse mixture-of-experts model with about 501.3 billion total parameters and roughly 23.4 billion active parameters per token. The active count helps explain the efficiency pitch, but it does not erase the memory needed to store the full checkpoint.

| Specification | Current value | 
|---|---|
| Total parameters | 501.3B | 
| Active parameters per token | 23.4B | 
| Context window | 1,048,576 tokens | 
| Maximum completion listed by OpenRouter | 131,072 tokens | 
| Input/output | Text only | 
| Tool calling | Supported by listed API route | 
| Structured outputs | Listed by OpenRouter | 
| License | Apache 2.0 | 
| Release | October 16, 2026 | 

Its million-token context is listed by providers, but the cited independent tracker reports no quality score for Beam yet.

Beam is text-only in the current model listing. A workflow that needs screenshots or diagrams must add OCR or another vision model before Beam sees the content.

## Beam API pricing is provider-dependent

The lowest currently listed OpenRouter route is $0.25 per million input tokens and $1.00 per million output tokens through Nebius. That headline is useful for batch evaluation, but it hides a large responsiveness trade-off.

| Provider | Input / 1M | Output / 1M | Output speed | Time to first token | 
|---|---|---|---|---|
| Nebius | $0.25 | $1.00 | 4 tok/s | 37.8s | 
| Together | $0.40 | $1.20 | 9 tok/s | 12.6s | 
| CoreWeave | $1.00 | $3.00 | 112 tok/s | 0.7s | 
| Baseten | $1.30 | $4.30 | 364 tok/s | 0.3s | 

Prices and telemetry are from the OpenRouter Beam page and The Known Good’s provider snapshot. The measurements are time-sensitive, not a guarantee for every prompt or region.

For 1,000 requests averaging 500 input and 300 output tokens, the listed cost is about $0.43 on Nebius versus $1.94 on Baseten. Confirm how your provider bills reasoning tokens before extrapolating this example. Use Nebius for asynchronous experiments if its 37.8-second startup is acceptable; use CoreWeave or Baseten for a user-facing agent, then measure your own tool-call latency.

OpenRouter reports automatic provider routing and failover, but tool calls are supported on only three of the four listed providers, so pinning a route can change behavior.

## Can you run Beam yourself?

The practical answer is “yes, with data-center-scale hardware,” not “yes, on a normal workstation.” Hugging Face’s verified catalog recipe specifies eight Nvidia H200 GPUs, 1,128 GB of GPU memory, 208 vCPUs, 2,880 GB of system memory, vLLM, and a displayed price of $40 per hour per running replica.

The Hugging Face deployment page preconfigures tensor parallelism across eight GPUs, FP8 KV cache, a 90% GPU-memory utilization target, a maximum model length of 524,288, prefix caching, chunked prefill, and Beam-specific tool and reasoning parsers. Scale-to-zero replicas are not billed, but the page warns that restarting from zero takes time.

Community reports also flag early looping and put the NVFP4 files at roughly 273 GB, well beyond a typical single-GPU workstation. The looping report appears in this post-release r/LocalLLaMA discussion; the 273 GB observation comes from @jimmycheng722 on X. Neither is an official minimum requirement.

The 23B active count does not make Beam a 23B-sized deployment: stored weights, runtime overhead, and KV cache still require multi-GPU hardware. For most teams, a hosted API is the sensible first test. Self-hosting makes sense when data residency, predictable capacity, or repeated high-volume traffic justifies a multi-GPU budget.

## What the evidence says about quality so far

Beam’s release claims emphasize coding, reasoning, tool use, and token efficiency. Those are plausible target workloads, but the evidence is still uneven. The Known Good currently gives Beam no composite quality score and no published result in its tracked GPQA, math, coding, or arena datasets. That does not prove poor quality; it means a buyer should not present vendor claims as independent validation.

The early community signal is more nuanced than “501B means frontier.” @wikiwayne reported a seven-for-seven result in a personal game-generation test, with 171,946 aggregate output tokens across the test, including 123,463 reasoning tokens, and said the generated HTML was smaller than Argon’s. It is not a benchmark, but it illustrates Beam’s possible trade-off: high reasoning-token use for a compact final artifact.

“Beam cleared the bar. 7/7 games. First try. $0. ... Honest tradeoff: Beam's HTML came in smaller than Argon's.” — @wikiwayne, X.


Treat Reflection’s coding scores and efficiency claims as vendor-reported until reproducible third-party evaluations cover the same prompts, tool settings, and reasoning configuration. The current information is enough to test Beam; it is not enough to declare it the best open model for coding.

## Which deployment should you choose?

| Your workload | Best starting point | Why | 
|---|---|---|
| Cheap batch experiments | Nebius through OpenRouter | Lowest listed token rates, if long startup is acceptable | 
| Interactive coding agent | CoreWeave or Baseten | Much lower first-token delay and higher throughput | 
| Long documents or repositories | Hosted route with a verified context limit | Avoid buying multi-GPU hardware before validating quality | 
| Strict data-control requirement | Hugging Face Endpoint or self-managed vLLM | Evaluate the verified deployment path against your network requirements | 
| High-volume predictable traffic | Cost model first, then self-host | $40/hour is a real baseline for the displayed H200 replica | 

Start with a fixed evaluation set: three coding tasks, one tool-calling task, one long-context retrieval task, and one task designed to catch looping. Record input tokens, reasoning tokens, time to first token, total latency, and final artifact quality.

## Reflection Beam FAQ

### Is Beam open source or open weight?

Beam is distributed as open weights under Apache 2.0. That permits broad use and modification of the released weights.

### What is the cheapest Beam API?

OpenRouter currently lists Nebius at $0.25 per million input tokens and $1.00 per million output tokens, but its current provider snapshot is also the slowest.

### Can Beam run on one GPU?

The verified Hugging Face recipe uses eight H200 GPUs; a single-GPU deployment should not be assumed.

Choose based on token cost, latency, and infrastructure control: Beam is testable now, but independent quality evidence remains limited.
