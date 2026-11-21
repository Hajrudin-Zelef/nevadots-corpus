---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-mai-base-2-2-8t-parameters-what-we-know
title: "Microsoft MAI-Base-2: 2.8T Parameters, What We Know"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "model", "mai-base-2", "parameters", "data", "trained", "says", "models"]
source: https://cellcog.ai/blog/microsoft-mai-base-2/
source_anchor: ""
source_lines: [1, 84]
section: "Copilot_Microsoft"
---
**Microsoft said on October 15, 2026 that it has finished pre-training MAI-Base-2, a mixture-of-experts model with 2.8 trillion total parameters and 80 billion active per token, trained on 16,000 GB200 GPUs over 38 days.** In its lab notes, posted at 16:09 UTC, Microsoft AI writes that “MAI-Base-2 outperforms every open-weight model we tested on our held-out evaluations.”

It is a base model, not a product. Nobody can use it yet: Microsoft says it is still in post-training, gives no release date, and publishes no API price. What it does publish is unusually specific about how the model was trained, plus one strategic line about who will get the weights.

## On this page · 7 sectionsOpen

1. Microsoft finished pre-training MAI-Base-2 on October 15, 2026: a mixture-of-experts model with 2.8 trillion total parameters and 80 billion active per token.
2. It trained on 16,000 GB200 GPUs over 38 days in August and September 2026, at 21% average utilization and 89% goodput.
3. Microsoft estimates its recipe is 4.2 to 12.3 times more compute-efficient than DeepSeek-V4-Pro-Base; these are its own numbers, not replicated.
4. Microsoft says the model was trained on organic data with no distillation from other models, so it can license the weights to trusted partners through Foundry.
5. It is still in post-training, with no release date, no benchmarks for agents and no price.

## Â§ 01What Microsoft disclosed

| Item | What Microsoft says | 
|---|---|
| Architecture | Mixture of experts, 2.8 trillion total parameters, 80 billion active | 
| Hardware and time | 16,000 GB200 GPUs, 38 days, August and September 2026 | 
| Utilization | Average model FLOPs utilization 21%, goodput 89% | 
| Data | Organic data, no distillation from other models; vision data added | 
| Status | In post-training; no release date, no API price | 
| Distribution | Weights to be licensed to trusted partners through Foundry | 

Its predecessor, MAI-Base-1, underpins MAI-Thinking-1, which Microsoft announced in June. Microsoft says MAI-Base-2 is âa larger, sparser model than MAI-Base-1, trained on more data.â

## Â§ 02The efficiency claim, explained

Microsoft does not compare MAI-Base-2 with closed models. It compares bits-per-byte on held-out text, a measure of how well a model predicts unseen writing where lower is better, against open models with at least one trillion parameters: DeepSeek-V4-Pro-Base, Kimi K3, Qwen3.8-2.4T-A95B and MiMo-V2.6-Pro-RL. Only DeepSeekâs is a true base checkpoint; the other three are post-trained, a caveat Microsoft states itself.

The headline number is efficiency, not size. Microsoft fits scaling curves to estimate how much training compute its recipe would need to match DeepSeek-V4-Pro-Base, then compares that with what DeepSeek actually used.

| Compared with | Low estimate | High estimate | 
|---|---|---|
| DeepSeek-V4-Pro-Base | 4.2x | 12.3x | 
| MAI-Base-1 (June 2026) | 6.3x | 12.7x | 

In plain terms: Microsoft says its method gets a given quality for a fraction of the training compute, and it spent that saving on a bigger model. These are Microsoftâs own estimates on Microsoftâs own evaluation sets, with code drawn from its own codebase and STEM, math and question sets commissioned from vendors to limit contamination. No outside group has replicated them.

## Â§ 03How they got there

The lab notes name three things. Scaling ladders: families of smaller models trained with a fixed recipe, plus a technique Microsoft calls source scaling that repeats each dataset the same number of times at every size, so small experiments do not mislead the big run. Model and infrastructure co-design: better mixture-of-experts and Muon optimizer implementations and activation gradient clipping. And more data, including vision data.

Microsoft also publishes the fitted formulas it used to set learning rate, weight decay and dropout, a level of training detail frontier labs rarely release.

## Â§ 04The Foundry line

The strategic sentence is about provenance. âOur pre-training is built on organic data which does not involve distillation from other models,â Microsoft writes, adding: âThis will enable us to license our model weights to trusted partners through Foundry.â

That matters because the open models Microsoft compares against are often trained partly on outputs of other labsâ models, which complicates licensing for cautious enterprises. Microsoft is positioning a model it owns end to end, alongside the OpenAI and Anthropic models it already sells through Foundry.

## Â§ 05What it means if you run agents

A base model is raw capability; what you can delegate depends on the post-trained model built on it, and that has not shipped. The numbers to watch are the ones Microsoft has not published yet: agent benchmarks, price per token and which customers get the weights. Until then, MAI-Base-2 is a signal that one more company intends to own a frontier model rather than rent one.

## Â§ 06What we are watching

- The post-trained model built on MAI-Base-2, its benchmarks and its Foundry price.
- Which partners Microsoft licenses the weights to, and on what terms.
- Any independent test of the efficiency estimates.

## Â§ 07Sources

- Microsoft AI, Frontier Pre-training at Microsoft AI, October 15, 2026.
- Hugging Face model pages for DeepSeek-V4-Pro-Base, Kimi K3, Qwen3.8-2.4T-A95B and MiMo-V2.6-Pro-RL, read October 15, 2026.

### Q1How big is MAI-Base-2?

2.8 trillion total parameters with 80 billion active per token, in a mixture-of-experts design, per Microsoft AI’s October 15, 2026 lab notes.

### Q2How was it trained?

On 16,000 GB200 GPUs for 38 days in August and September 2026, with average model FLOPs utilization of 21% and goodput of 89%.

### Q3What does 4 to 12 times more efficient mean?

Microsoft estimates its recipe would need 4.2 to 12.3 times less training compute than DeepSeek used to match DeepSeek-V4-Pro-Base on its held-out text tests.

### Q4When will it be available?

Microsoft has not said. The model is in post-training, and Microsoft says it will license weights to trusted partners through Foundry.

### Q5Does CellCog use MAI models?

No. CellCog runs on Anthropic’s Claude models (Claude Opus 5.5 on most tiers; Claude Fable 5.2 on Agent Max and Creative Max since November 11, 2026). This post reads Microsoft’s lab notes as published.
