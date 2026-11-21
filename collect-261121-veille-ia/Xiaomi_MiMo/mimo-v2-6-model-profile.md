---
id: collect-261121-veille-ia/xiaomi-mimo/mimo-v2-6-model-profile
title: "MiMo-V2.6 - Model Profile"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: []
keywords: ["mimo-v2", "model", "flash", "analysis", "artificial", "index", "billion", "faraday"]
source: https://www.faradaymachines.com/models/mimo-v2-6/
source_anchor: ""
source_lines: [1, 65]
section: "Xiaomi_MiMo"
---
# MiMo-V2.6

Xiaomi's MIT-licensed open-weight pair: MiMo-V2.6-Pro, the top open-weight model on the Artificial Analysis Intelligence Index v4.3.2, and MiMo-V2.6-Flash, a cost-efficient generalist that fits a single 512GB Mac Studio on Faraday's Scale tier.

## Model Story

Xiaomi released MiMo-V2.6 on September 21–22, 2026, under the MIT license. Xiaomi also published its training code and reinforcement learning environments, alongside a 9B distilled model. The release comes in two main sizes: MiMo-V2.6-Pro and MiMo-V2.6-Flash.

MiMo-V2.6-Pro is a sparse Mixture-of-Experts model with 1.02 trillion total parameters, 42 billion active, and a 1 million token context window. Artificial Analysis lists it as the #1 open-weight model on its Intelligence Index v4.3.2, with a score of 46 at roughly $0.13 per Intelligence Index task in Artificial Analysis's own harness. That puts it about 7 points behind the GPT-6 Astra, Claude Fable 5.1 and Gemini 4 Argon tier (53), which costs $1.99–$7.63 per task, and 12 points behind Claude Opus 5.5 at max effort (58).

MiMo-V2.6-Flash is the smaller sibling: 309 billion total parameters with 15 billion active, also MIT-licensed. Artificial Analysis scores it at 38 on the v4.3 index. Artificial Analysis also describes Flash as slower than average and very verbose, which is worth knowing before you choose it for latency-sensitive work.

Faraday Machines ships MiMo-V2.6-Flash as a cost-efficient generalist on the Scale tier. MiMo-V2.6-Pro is available on larger setups, by quote, for organizations that want the strongest open-weight model available.

## Key Specifications

| Developer | Xiaomi | 
|---|---|
| Release Date | September 21–22, 2026 | 
| Architecture | Sparse Mixture-of-Experts (MoE) | 
| Total Parameters | 1.02 trillion (Pro) / 309 billion (Flash) | 
| Active Parameters | 42 billion (Pro) / 15 billion (Flash) | 
| Context Window | 1,000,000 tokens (Pro) | 
| License | MIT (both models), with open training code and RL environments | 
| Size at 4-bit | Roughly 560 GB (Pro) / roughly 170 GB (Flash) | 
| Faraday Tier | Flash: Scale (1× Mac Studio M5 Ultra, 512GB); Pro: larger setups (talk to us) | 
| Also Released | 9B distilled model | 

## Pricing

Artificial Analysis measures MiMo-V2.6-Pro at roughly $0.13 per Intelligence Index task, a small fraction of the $1.99–$7.63 per task it measures for the closed models that score 53. The MIT license means no licensing fees and no usage caps, and on Faraday hardware there are no per-token costs; you pay only the monthly plan and electricity.

## Benchmarks

All scores above are independent Artificial Analysis results on the v4.3 index, which was rescaled in September 2026 with Terminal-Bench 4.0 and AutomationBench-AA. Every model scored lower than on the earlier v4.1 index, so these numbers should not be compared with v4.1 scores elsewhere on this site. On v4.3 the gap between the best open model and the very top closed model widened, while the cost advantage of open models grew.

## On-Premises Deployment

At 4-bit quantization MiMo-V2.6-Flash needs roughly 170 GB of memory, which fits on Faraday's Scale tier: one Mac Studio M5 Ultra with 512GB of unified memory. With only 15 billion active parameters per token, it is a cost-efficient generalist for drafting, analysis and everyday business tasks. Its verbosity matters less on on-premises hardware, where extra output tokens cost time rather than per-token fees.

MiMo-V2.6-Pro needs roughly 560 GB at 4-bit quantization, more than a single 512GB Scale machine holds, so it calls for a larger setup with more machines. Faraday Machines sizes larger setups by quote, so organizations that want the top open-weight model on the Artificial Analysis index can run it entirely on hardware they control.

Both models are MIT-licensed, so legal teams can sign off without the review cycles required for custom licenses like those on Kimi K3 or GLM-5.3.

## Frequently Asked Questions

## ### Can I run MiMo-V2.6 on-premises?


                            Yes. MiMo-V2.6-Flash needs roughly 170 GB at 4-bit quantization and runs on Faraday Machines' Scale tier: one Mac Studio M5 Ultra with 512GB of unified memory. MiMo-V2.6-Pro needs roughly 560 GB at 4-bit, more than one Mac Studio holds, so it calls for a larger setup, which Faraday Machines configures on request.

## ### Is MiMo-V2.6 open source?


                            Yes. Xiaomi released both MiMo-V2.6-Pro and MiMo-V2.6-Flash under the MIT license on September 21–22, 2026, and also published its training code, reinforcement learning environments and a 9B distilled model. The MIT license means no licensing fees or usage caps, and legal teams can approve it without the review cycles that custom licenses such as Kimi K3's or GLM-5.3's require.

## ### How does MiMo-V2.6-Pro compare to GPT-6 Astra and Claude Opus 5.5?


                            MiMo-V2.6-Pro trails both but costs far less per task. Artificial Analysis lists it as the #1 open-weight model on its Intelligence Index v4.3.2, scoring 46 at roughly $0.13 per task. That is about 7 points behind GPT-6 Astra, Claude Fable 5.1 and Gemini 4 Argon (53), which cost $1.99–$7.63 per task, and 12 points behind Claude Opus 5.5 at max effort (58).

## ### Is MiMo-V2.6-Flash fast?


                            Not especially. Artificial Analysis describes MiMo-V2.6-Flash as slower than average and very verbose, and scores it at 38 on its Intelligence Index v4.3. With 15 billion active parameters it suits drafting, analysis and everyday business tasks better than latency-sensitive work. On Faraday Machines hardware its verbosity costs time rather than per-token fees.
