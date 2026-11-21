---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-builds-2-8t-parameter-model-to-rival-open-weights-fourweekmba
title: "Microsoft Builds 2.8T-Parameter Model to Rival Open Weights - FourWeekMBA"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-10-15"]
keywords: ["microsoft", "says", "model", "post", "mai-base-2", "training", "data", "weights"]
source: https://fourweekmba.com/ai-microsoft-builds-2-8t-parameter-model-to-rival-open-weights/
source_anchor: ""
source_lines: [1, 69]
section: "Copilot_Microsoft"
---
Microsoft AI said on 15 October 2026 that it has completed pre-training MAI-Base-2, a mixture-of-experts foundation model with 2.8 trillion total parameters and 80 billion active, according to a post on its website. The model is now undergoing post-training, the post says.

Microsoft says MAI-Base-2 outperforms every open-weight model it tested on its held-out evaluations, and that it plans to license its model weights to trusted partners through Foundry.

Business Pill · PRETRAINING

A one-minute explainer of pretraining: how an AI model learns from a large body of data before it is taught a specific task. It teaches the general idea only and says nothing about any company in this story.

According to the post, MAI-Base-2 is “an MoE foundation model with 80 billion active and 2.8 trillion total parameters.” Microsoft describes it as larger and sparser than MAI-Base-1, the base model behind MAI-Thinking-1, which it announced in June.

Microsoft says it trained MAI-Base-2 on 16k GB200 GPUs over 38 days in August and September 2026. Over the run, the post says, average model FLOPs utilization was 21% and goodput, the fraction of time training progressed, was 89%.

The training mixture was 50T tokens, the post says. Unlike MAI-Base-1, the model was pre-trained natively with vision, and Microsoft says vision-language data accounted for 18% of that mixture.

The post says the corpus grew by roughly 36% from MAI-Base-1 to MAI-Base-2, measured in unique training tokens, with a March 2026 knowledge cutoff.

Microsoft says MAI-Base-2 is 4-12x more pre-training FLOPs-efficient than DeepSeek-V4-Pro-Base, which it calls the strongest open base model, and that “we have put that saving into training a larger model.”

The post gives the range as 4.2–12.3x, estimated by fitting scaling laws on four held-out evaluations. Against its own MAI-Base-1, Microsoft puts the improvement at 6.3–12.7x since June.

Microsoft says it compared MAI-Base-2 with DeepSeek-V4-Pro-Base, Kimi K3, Qwen3.8-2.4T-A95B and MiMo-V2.6-Pro-RL. It notes that only post-trained checkpoints were available for the last three, and that post-training may affect these evaluations.

The comparisons use bits-per-byte on held-out text, the post says: code from Microsoft AI’s own codebase, and STEM, math and Q&A evaluations commissioned from vendors to reduce the risk of training-set contamination.

Microsoft says its pre-training “is built on organic data which does not involve distillation from other models,” and that it processed every training token in-house.

The post links that data provenance to a business plan: “This will enable us to license our model weights to trusted partners through Foundry, which provides enterprises with a trusted source of world-class intelligence that serves their needs.”

Owning the full stack, the post says, lets Microsoft tune its models and infrastructure together for the best performance at the lowest cost, first in its own products and then for Foundry customers.

Microsoft says its proprietary web crawler respects robots.txt and related controls, and that it excludes sources on the US Trade Representative’s Notorious Markets list.

The post credits three initiatives: scaling ladders that account for repeated training data, model-infrastructure co-design, and scaling data, including vision data.

Microsoft says fixes to its mixture-of-experts implementation raised the utilization of its MoE layers from 24% to 35%, which let it use MoE in every layer. It says the Muon optimizer took 2.1% (165 ms) of the average training iteration time.

Microsoft has said its Maia 300 chip gets 40% more tokens per megawatt, as we reported on 12 October; the MAI-Base-2 post describes training on GB200 GPUs.

Nvidia released open Nemotron 3.5 models the same day, as we reported.

As we read it, the efficiency figure is where Microsoft spent its edge: the post says the recipe is 4-12x more pre-training FLOPs-efficient than DeepSeek-V4-Pro-Base and that Microsoft put that saving into training a larger model.

As we read it, the comparison set is open-weight models: Microsoft says MAI-Base-2 outperforms every open-weight model it tested, and three of the four it names were measured as post-trained checkpoints.

As we read it, the full-stack claim runs from data to infrastructure: the post says Microsoft processed every training token in-house and tunes models and infrastructure together, first in its own products and then for Foundry customers.

**FOUNDRY CUSTOMERS** Microsoft says data provenance will enable it to license model weights to trusted partners through Foundry.

**OPEN-WEIGHT LABS** The post names DeepSeek-V4-Pro-Base, Kimi K3, Qwen3.8-2.4T-A95B and MiMo-V2.6-Pro-RL as the models MAI-Base-2 was compared with.

**MICROSOFT AI’S ROADMAP** The post says MAI-Base-2 is undergoing post-training and that preparations for MAI-Base-3 are already underway.

This story maps onto the Business Engineer framework The Four Intelligence Moats.

The framework names four moats, one per data paradigm, beginning with “The Corpus Moat — built by pretraining, eroding as the public web is exhausted and synthetic data closes gaps.”

As we read it, Microsoft’s post is a bet on that first moat: it says MAI-Base-2 was pre-trained on organic data processed in-house, without distillation, and that its corpus grew by roughly 36% in unique tokens. The framework does not discuss this model.

The post we read gives no release date for a post-trained model built on MAI-Base-2, no Foundry licensing price or terms, and no dollar cost for the training run.

The efficiency and performance figures are Microsoft’s own, from its own evaluations, and three of the comparison models were measured as post-trained checkpoints.

Microsoft AI says it has completed pre-training MAI-Base-2, a 2.8-trillion-parameter mixture-of-experts model with 80 billion active parameters, trained on 16k GB200 GPUs over 38 days.

Microsoft says the model outperforms every open-weight model it tested, was trained on organic data without distillation, and that it plans to license model weights to trusted partners through Foundry. The model is in post-training, and the post says preparations for MAI-Base-3 are underway.

95,000+ executives read Business Engineer for the AI strategy frameworks cited by ChatGPT, Claude, and Perplexity.

*Sources: Microsoft AI: Frontier Pre-training at Microsoft AI (15 Oct 2026)*
