---
id: collect-261121-veille-ia/microsoft-actu/microsoft-mai-base-2-a-2-8-trillion-parameter-moe-and-the-efficiency-claim-behind-it
title: "Microsoft MAI-Base-2: A 2.8 Trillion-Parameter MoE and the Efficiency Claim Behind It"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "model", "mai-base-2", "efficiency", "claim", "pre-training", "mai-base-1", "models"]
source: https://www.metirai.com/blog/microsoft-mai-base-2-2-8-trillion-moe-pretraining-2026
source_anchor: ""
source_lines: [1, 40]
section: "Microsoft_Actu"
---
On October 15, 2026, Microsoft AI published "Frontier Pre-training at Microsoft AI," describing MAI-Base-2, a mixture-of-experts foundation model with 2.8 trillion total parameters and 80 billion active per token. The post is a technical account of how the model was trained and a set of efficiency claims against open-weight rivals. It is also notable for what Microsoft says about its own data and for what it does not yet offer: a release date. This analysis separates the specifications, the claims and the open questions.

## The specifications

MAI-Base-2 is larger and sparser than its predecessor, MAI-Base-1, with mixture-of-experts layers in every layer. Because only 80 billion of 2.8 trillion parameters are active for any token, the active-to-total ratio is roughly 2.9%, which is how a model of this size keeps per-token compute manageable. The model is pre-trained natively with vision using an encoder-free vision-language architecture; 4.5 trillion unique tokens of vision-language data make up 18% of the mixture with repetition. The mixture has about 36% more unique training tokens than MAI-Base-1.

Microsoft states that all tokens were processed in-house and that the pre-training data does not involve distillation from other models. That is a provenance claim relevant to a year in which distillation disputes between labs have been prominent, and it is a statement by the developer rather than an audited finding.

## The efficiency claim

The headline comparison is on compute. Microsoft estimates MAI-Base-2 delivers 4.2 to 12.3 times the FLOPs efficiency of DeepSeek-V4-Pro-Base and 6.3 to 12.7 times that of MAI-Base-1. The introduction rounds the first range to 4x to 12x. It also reports lower bits-per-byte on held-out evaluations than every open-weight model tested, including DeepSeek-V4-Pro-Base, Kimi K3, Qwen3.8-2.4T-A95B and MiMo-V2.6-Pro-RL.

### Microsoft's estimated FLOPs-efficiency advantage

Low and high ends of the range Microsoft estimates for MAI-Base-2, as a multiple (x). Company-reported, not independently verified.

How to read a range that wide? FLOPs efficiency here is an estimate of how much compute a model needs to reach a given loss, and the spread between 4.2x and 12.3x reflects estimation uncertainty and different reference points. Bits-per-byte is a pre-training metric: it measures how well a base model predicts held-out text, not how useful the finished assistant is. The post says specific scores appear only in figures, so independent readers cannot yet reproduce the numbers.

## Training efficiency

The run used 16,000 GB200 GPUs for 38 days in August and September 2026. Average model FLOPs utilization (MFU), normalized to a 2.5 PFLOPS BF16 peak, was 21%, with goodput of 89%. Microsoft says MFU for the mixture-of-experts layers improved from 24% to 35%. The Muon optimizer took 2.1% of average iteration time (165 ms) against a theoretical peak of about 55 ms, which leaves identifiable headroom. A 21% average MFU may look low, but large sparse models often run well below dense-model utilization because expert routing and communication add overhead; the more informative figure is the improvement trend.

## What is not settled

MAI-Base-2 is currently in post-training and has no release date. Microsoft plans to license weights to trusted partners through Foundry rather than release open weights, and preparations for MAI-Base-3 are underway. The earlier model in the family, MAI-Thinking-1, announced in June 2026, was post-trained on MAI-Base-1, so a finished assistant built on MAI-Base-2 would be a step beyond what customers can use today.

Three questions remain. First, whether pre-training advantages survive post-training. Second, whether independent evaluators can reproduce the efficiency estimates once weights reach partners. Third, how a trusted-partner licensing model compares with open weights for the developers who have been building on models like DeepSeek.

For teams choosing models, the practical lesson is that base-model metrics and product behavior are different layers. Platforms that let users compare several providers in one place, such as Metir, make that difference visible by testing the finished model rather than the pre-training chart.

The measured summary: Microsoft reports a very large, sparse, natively multimodal base model with an estimated 4x to 12x compute-efficiency edge over a leading open-weight base, trained on 16k GB200s in 38 days. The numbers are self-reported, the model is unreleased, and the evidence that matters will come after post-training and partner access.

## Sources:

- Frontier Pre-training at Microsoft AI (Microsoft AI, Oct 15, 2026)
- AI News Today, October 17 (AI Weekly)

## Image credits

Header and in-body image: Microsoft Redmond Campus redevelopment aerial view, Sept. 2021, by Atomic Taco, Wikimedia Commons, CC BY-SA 2.0 (source).
