---
id: collect-261121-veille-ia/modeles-open/ling-3-1-flash-1019-a-560b-mit-licensed-moe-drops-its-weights-sized-for-8h200-gpujournal
title: "Ling-3.1-flash-1019: a 560B MIT-licensed MoE drops its weights — sized for 8×H200 | GPUJournal"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-10-27"]
keywords: ["weights", "ling-3", "flash-1019", "mit-licensed", "h200", "model", "card", "drops"]
source: https://gpujournal.ai/story/ling-3-1-flash-1019-weights
source_anchor: ""
source_lines: [1, 19]
section: "Modeles_Open"
---
NEWS → · Models · Ant Group · inclusionAI

# Ling-3.1-flash-1019: a 560B MIT-licensed MoE drops its weights — sized for 8×H200

Oct 27, 2026

Released by Ant Group’s inclusionAI on Hugging Face on Oct 27, 2026: the weights for Ling-3.1-flash-1019, a 560-billion-parameter mixture-of-experts model with 25B active parameters and a 1M-token context window, under the MIT licence. Until today the upgraded model was API-only; now it can be self-hosted — on a full node.

Image: inclusionAI

What shipped, from the model’s own Hugging Face card and repository, read Oct 27, 2026: a hybrid-linear mixture-of-experts model (63 KDA layers + 9 Gated MLA; 512 routed experts, 8 active per token), 560B total and 25B active parameters, a 1M-token context window, the MIT licence in the repo, public and ungated. The shipped weights total about 574GB. It is a post-training upgrade of Ling-3.1-flash, aimed at reasoning and agentic work.

The gains are as reported on the card, which cites Artificial Analysis: Intelligence Index 44, up from 41 for the predecessor; AA-Briefcase up from roughly 1,400 to 1,524 Elo; AutomationBench-AA 62% to 68%; CritPt 18% to 26%; SciCode 54% to 57%. Artificial Analysis is an independent benchmarker, but these figures are inclusionAI’s card quoting it — treat them as card-reported until checked against the underlying listing.

The serving facts are unusually concrete, from the same card: day-0 SGLang support with a published recipe for 8×H200 (tensor and expert parallelism 8) or 4×GB300, a vLLM route at tensor parallelism 8, and verified recipes also listed for H800, H100 and B200. By our published method the memory maths agree with the maker’s sizing: about 1,120GB at FP16, roughly 560GB at 8-bit — the shipped build’s class — and about 280GB at 4-bit. This is a node rental, priced like one; nobody runs it on a single card.

### What it means if you rent GPUs

For builders, an MIT-licensed 560B with no revenue gate and a same-day serving recipe is the useful kind of weights drop: the licence question and the hardware question are both answered on day one. The honest limits: the benchmark figures are card-reported, the model ships with custom modelling code, and the entry ticket is an 8-GPU node. We have added it to the model catalogue with the memory maths.
