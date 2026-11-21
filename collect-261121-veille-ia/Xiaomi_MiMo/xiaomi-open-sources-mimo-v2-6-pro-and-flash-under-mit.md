---
id: collect-261121-veille-ia/xiaomi-mimo/xiaomi-open-sources-mimo-v2-6-pro-and-flash-under-mit
title: "Xiaomi open-sources MiMo-V2.6-Pro and Flash under MIT"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: []
keywords: ["mimo-v2", "xiaomi", "model", "flash", "weights", "under", "same", "hugging"]
source: https://www.deai.org/news/xiaomi-mimo-v2-6-open-weights-mit-rl-training-code
source_anchor: ""
source_lines: [1, 61]
section: "Xiaomi_MiMo"
---
Xiaomi open-sourced its MiMo-V2.6 model series on September 22, putting MIT-licensed weights for a 524B-parameter flagship and a 159B smaller sibling on Hugging Face — and, unusually, publishing the reinforcement learning code, task environments, and technical report behind them. The release is verifiable; the benchmark claims attached to it are not yet.

## Key facts

- MiMo-V2.6-Pro-RL is live on Hugging Face at 524B parameters, tagged license:mit, with the technical report PDF in the repo.
- MiMo-V2.6-Flash-RL (159B) and a MiMo-V2.6-Distill-Qwen-9B checkpoint round out the MiMo-V2.6 collection.
- Xiaomi says the two models completed 30 RL steps in under six days at reported costs of about $2.62M (Pro) and $0.85M (Flash) — vendor figures, not audited ones.
- Published API pricing: $0.435 input (cache miss) / $0.87 output per million tokens for Pro; $0.14 / $0.28 for Flash, unchanged from V2.5.
- Xiaomi claims MiMo-V2.6-Pro scores 46.32 on the Artificial Analysis Intelligence Index, which would make it the highest-scoring open-weights model — a claim Artificial Analysis' own post corroborates as an index placement, not a settled fact.

## What happened

The release page describes the series as two natively omnimodal models — text, image, video, and audio input in one model — plus a Pro-UltraSpeed mode the company says serves the same outputs up to 20× faster. The model card specifies a 1M-token context window and a mixture-of-experts architecture trained with asynchronous GRPO at 1,568 prompts × 16 rollouts per step.

The part that is genuinely new is not the parameter count. Xiaomi streamed the production RL run live and has now shipped everything around it: the training environments, the RL code, and the full technical report, alongside the weights. The run covered coding, general agents, visual tasks, and cybersecurity in one mixed batch rather than separate per-domain runs, and the company built an explicit defense stack against reward hacking — reward design, adversarial evaluation, anomaly detection, and cross-checking between verifiers. Previous large open-weight releases have shipped the model and sometimes the data; shipping the working RL environments is rarer, and it is the piece a research team can reuse regardless of which model they run.

That transparency discipline is why this leads as TODAY rather than BREAKING: the event is confirmed by the vendor's own page, the Hugging Face repos, and third-party coverage (VentureBeat called MiMo-V2.6-Pro "the top open-weights model in the world" on the same numbers Xiaomi published), but nothing here has been independently reproduced.

## Why it matters

For builders deciding where to run models, the license is the load-bearing fact. MIT means the weights can be self-hosted, fine-tuned on proprietary data, and redistributed without negotiation — the same terms as K2 Horizon's Apache 2.0 release earlier this month. The practical self-host target is MiMo-V2.6-Flash-RL at 159B parameters; the 524B Pro needs a multi-GPU serving setup that puts it out of hobbyist range, though quantized community builds will shrink that quickly.

The pricing floor matters just as much. At $0.14 per million input tokens for Flash, Xiaomi keeps its position as one of the cheapest frontier-adjacent API options, and holding V2.5's prices while shipping a stronger model is a direct challenge to StepFun's Step 5 Preview at $2.70 per million output tokens — a model that still has no downloadable weights. Whether MiMo's token efficiency holds up on real workloads, not vendor-selected benchmarks, decides how much that gap is worth; our earlier coverage of self-hosting versus API economics breaks down that calculation.

There is also a competitive wrinkle worth naming. Xiaomi's own marketing ties MiMo-V2.6's gains to Moonshot AI's Kimi K3 as a post-training base — the same Moonshot model Bloomberg reported this week that Harvey, Abridge, and Ramp post-trained in-house to escape negative margins on frontier-lab APIs. Open weights are becoming the substrate other vendors build on, and that compounding is the real story of the month.

## Background

This is Xiaomi's third large open-weight drop and the most complete. The MiMo-V2.5 series shipped in April with a 1.02T-parameter Pro model, MIT-licensed, but no training code. MiMo-V2.6 adds the reinforcement learning infrastructure, the environments, and a live-training dashboard that exposed trainer logs — including crash and restart incidents — during the run itself.

The release also lands in the middle of an open-weights supply surge: StepFun's Step 5 Preview promises weights on October 15, the Institute of Foundation Models shipped K2 Horizon with a 512K context window, and Jared Palmer's Kev decision-model family showed that even frontier-lab product *shapes* are now reproducible at hobbyist cost. What distinguishes MiMo-V2.6 in that crowd is license plus training-code transparency, not headline benchmark position — every model in this cohort claims to lead some index.

## What's next

Watch three things. First, independent reproductions: the technical report and RL environments are public, so third-party benchmark runs — particularly on DeepSWE v1.1 and Automation Bench — are possible within weeks and are the only thing that converts Xiaomi's claims into verified facts. Second, community quantizations of the 524B Pro, which will determine whether it becomes practically hostable outside large clusters. Third, whether competitors match the training-code transparency rather than just the weights; StepFun's October 15 weight drop will be an immediate test of whether the MIT-plus-code bar becomes the new default for frontier open releases.

Questions

- Are the MiMo-V2.6 weights actually downloadable?
- Yes. The XiaomiMiMo Hugging Face organization lists MiMo-V2.6-Pro-RL (524B parameters), MiMo-V2.6-Flash-RL (159B) and a 9B Qwen distill in its MiMo-V2.6 collection, with the Pro and Flash checkpoints tagged license:mit. The full technical report PDF ships inside the Pro repository.
- What can Xiaomi's MiMo-V2.6 benchmark claims be trusted for?
- The release itself (weights, license, RL code, environments) is verifiable on Hugging Face and mimo.xiaomi.com. Benchmark scores, the Artificial Analysis Index placement of 46.32, training costs, and token-efficiency improvements are Xiaomi's own numbers until someone reproduces them.
- Which MiMo-V2.6 model should a self-hoster start with?
- MiMo-V2.6-Flash-RL at 159B parameters is the lighter checkpoint; the 524B Pro needs multi-GPU serving. Both are MIT, so commercial use and fine-tuning carry no license negotiation. Model size, not license, is the constraint.
- How much does the MiMo-V2.6 API cost?
- Xiaomi's published prices are $0.435 per million input tokens (cache miss) and $0.87 per million output for Pro, and $0.14 / $0.28 for Flash — unchanged from V2.5, per the release page. UltraSpeed mode for Pro lists at $4.35 / $8.70.

Sources

1. Introducing MiMo-V2.6 series — Xiaomi MiMo release page — Xiaomi
2. XiaomiMiMo/MiMo-V2.6-Pro-RL — weights and technical report — Hugging Face
3. MiMo-V2.6 collection — XiaomiMiMo on Hugging Face — Hugging Face
4. Better than DeepSeek? Xiaomi's MiMo-V2.6 Pro debuts as the top open-weights model, alongside cheaper V2.6 Flash — VentureBeat

About DeAI

DeAI is an independent publication covering open-weight AI models, private inference, and decentralized infrastructure — the tools for running AI you actually control. We test providers on price, privacy, and refusal behavior and publish the numbers, not the vibes. DeAI is powered by Morpheus (mor.org), a decentralized inference marketplace, and covers it on the same terms as every other provider.

Powered by Morpheus and StrandCMS

Morpheus is a decentralized inference marketplace, covered on the same terms as every other provider — we rank it wherever the data lands. StrandCMS is the open-source, agent-first framework this site is built on.
