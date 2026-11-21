---
id: collect-261121-veille-ia/xiaomi-mimo/xiaomi-mimo-v2-6-open-source-pro-flash-9b-models
title: "Xiaomi MiMo-V2.6 Open Source: Pro, Flash, 9B Models"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: []
keywords: ["xiaomi", "mimo-v2", "model", "flash", "models", "release", "checkpoint", "smaller"]
source: https://www.eweek.com/news/xiaomi-mimo-v26-open-source-rl-reproduction/
source_anchor: ""
source_lines: [1, 41]
section: "Xiaomi_MiMo"
---
Xiaomi has released MiMo-V2.6 with downloadable Pro and Flash model weights, a smaller 9-billion-parameter research model, and reinforcement-learning resources aimed at developers studying agentic AI training.

The release is broader than a typical model checkpoint drop. Xiaomi is publishing model weights alongside more than 7,000 RL task environments and an end-to-end training framework, while reporting large performance gains from the reinforcement-learning system behind the models.

According to Xiaomi's MiMo-V2.6 release, MiMo-V2.6-Pro-RL and MiMo-V2.6-Flash-RL are the two primary multimodal models. Xiaomi also released MiMo-V2.6-Distill-Qwen-9B, a smaller checkpoint created by fine-tuning Alibaba's Qwen3.5-9B on MiMo-generated data.

The Pro, Flash, and 9B Hugging Face repositories list MIT licenses. That adds MiMo-V2.6 to a growing field of Chinese open-weight AI models competing on cost, deployment control, and agentic capabilities.

## What Xiaomi released with MiMo-V2.6

The MiMo-V2.6-Pro-RL repository describes Pro as the flagship checkpoint, with text, image, video, and audio inputs and a context window of up to 1 million tokens. Flash targets a more efficiency-focused position within the same family.

The 9B release serves a different purpose. Its model card describes MiMo-V2.6-Distill-Qwen-9B as a supervised fine-tuned checkpoint intended as a starting point for agentic reinforcement-learning research, rather than a smaller equivalent of Pro or Flash.

That distinction matters because enterprises evaluating the release are looking at three different assets: two large RL-trained models that can be deployed directly, plus a smaller checkpoint designed to support further experimentation.

Xiaomi says its open release also includes more than 7,000 RL environments covering software engineering, vulnerability reproduction, knowledge-intensive tasks, and web development, together with a framework for environment interaction, trajectory collection, reward evaluation, and policy optimization.

The company reports that its larger training runs used 1,568 prompts and 16 rollouts per training step, generating billions of tokens per update. Xiaomi says the RL phase cost about $2.62 million for Pro and $850,000 for Flash, excluding pretraining and other development work.

Those figures come from Xiaomi. Independent testing does provide some outside evidence about the finished Pro model: Artificial Analysis currently scores MiMo-V2.6-Pro at 46 on its Intelligence Index and lists it among the leading open-weight models it has tested.

That result evaluates the released model's capabilities. It does not independently validate Xiaomi's claims about which parts of its RL system produced the gains.

## What eWeek found: what enterprises can verify today

eWeek's review of Xiaomi's release materials, public model repositories, and independent benchmark assessments points to several distinctions enterprises should keep in view:

- **The model releases are publicly verifiable.** Pro-RL, Flash-RL, and MiMo-V2.6-Distill-Qwen-9B have public Hugging Face repositories, and each currently lists an MIT license.
- **The 9B model is not a mini version of Pro.** Xiaomi identifies it as a supervised fine-tuned Qwen3.5-9B checkpoint for RL research. Teams evaluating it should not assume that Xiaomi's Pro or Flash performance transfers to the smaller model.
- **Independent testing supports Pro's competitiveness, not Xiaomi's entire training narrative.** Artificial Analysis has tested MiMo-V2.6-Pro independently, providing evidence beyond Xiaomi's own benchmark tables. That testing does not establish that another organization would obtain the same gains from Xiaomi's RL framework.
- **Some headline coding benchmarks need qualification.** Xiaomi reports results on SWE-bench Verified and DeepSWE v1.1. Epoch AI currently classifies SWE-bench Verified and DeepSWE v1.1 as flawed because of scoring, contamination, or task-quality problems. Those assessments do not invalidate MiMo-V2.6, but they limit what the scores alone can establish.
- **Cybersecurity evidence remains narrower.** Xiaomi reports improvements on its own MiMo Cyber benchmark, while independent security testing specific to MiMo-V2.6 remains limited. Previous eWeek analysis of open-weight cybersecurity models found that external testing can produce a much narrower picture than vendor benchmark claims.

For enterprise buyers, the immediate evaluation question is therefore not whether MiMo-V2.6 exists or whether its weights can be downloaded. Those points are clear.

The remaining work is workload-specific: test the exact model being considered, verify infrastructure requirements, assess security and data-handling controls, and compare Xiaomi's vendor-reported benchmark gains with independent results as more testing becomes available.

Because the 9B checkpoint is built on Qwen3.5, teams should also review the exact dependency and license chain rather than treating "MiMo-V2.6" as a single software package. Recent Qwen licensing changes illustrate how terms can vary between models in the same broader ecosystem.

**Also read:** **Nvidia Cosmos 3** **offers another example of an open-weight AI release where model availability is only one part of the enterprise deployment decision.**
