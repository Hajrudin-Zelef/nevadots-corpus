---
id: collect-261121-veille-ia/microsoft-actu/what-is-mai-base-2-microsoft-s-new-2-8t-parameter-ai-model
title: "What Is MAI-Base-2? Microsoft's New 2.8T-Parameter AI Model"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "model", "mai-base-2", "base", "post-training", "trillion", "parameters", "training"]
source: https://www.layer3labs.io/guides/mai-base-2-explained
source_anchor: ""
source_lines: [1, 109]
section: "Microsoft_Actu"
---
# What Is MAI-Base-2? Microsoft's 2.8-Trillion-Parameter Base Model

Technical specifications for Microsoft AI's 2.8-trillion-parameter base model and its Foundry partner licensing plans.

MAI-Base-2 is an in-house foundation model developed by Microsoft AI that features 2.8 trillion total parameters, 80 billion active parameters per token, and native vision processing. On October 15, 2026, Microsoft AI published details in a technical announcement confirming that pre-training finished after 38 days of computation across 16,000 NVIDIA GB200 graphics processing units (GPUs). Nobody outside Microsoft can use it yet. Post-training is still underway.

Microsoft pre-trained MAI-Base-2 on 50 trillion tokens with a knowledge cutoff of March 2026, incorporating an encoder-free Vision-Language Model (VLM) architecture. The design uses a dropless Mixture-of-Experts (MoE) configuration with Mixture-of-Experts in every layer. Unlike customer-facing chat assistants, base models do not follow conversational dialogue instructions before post-training completes.

As of October 2026, Microsoft plans to license the model weights to trusted partners through Foundry rather than distributing them openly. Organizations seeking immediate deployments must turn to existing open-weights alternatives or public commercial services while post-training continues. Microsoft AI products a business can use today are covered in the guide to Microsoft AI tools for business.

## What MAI-Base-2 Is (and Whether You Can Use It Yet)

MAI-Base-2 is an in-house base model developed by Microsoft AI, combining 2.8 trillion parameters with a sparse routing mechanism that activates 80 billion parameters per token. Microsoft announced on October 15, 2026 that pre-training is complete, but the model remains unavailable for public or enterprise use. The model is currently undergoing post-training.

Base models represent the initial stage of machine learning development, capturing statistical patterns across text and images without conversational alignment. Because MAI-Base-2 has not undergone instruction tuning or safety filtering, no public Application Programming Interface (API), chat interface, or weight download exists today. Microsoft intends to license the weights to trusted partners through Foundry, with no date announced.

- Maker: Microsoft AI, the dedicated organization building in-house foundation models for Microsoft.
- Current status: Pre-training complete, announced October 15, 2026, with post-training underway.
- Parameter scale: 2.8 trillion total parameters with 80 billion active parameters per token.
- Architecture: Dropless Mixture-of-Experts (MoE) in every layer.
- Native vision: Encoder-free Vision-Language Model (VLM) pre-trained directly on multi-modal data.
- Training dataset: 50 trillion total tokens, including 4.5 trillion unique vision-language tokens.
- Knowledge cutoff: March 2026.
- Training compute: 16,000 NVIDIA GB200 GPUs over 38 days in August and September 2026.
- Optimization algorithm: Muon optimizer, chosen for stable training dynamics over AdamW.
- Context window: Not published by Microsoft.
- Pricing: Not published because no commercial endpoint or service exists.
- Release date: Not published, with Microsoft stating only that additional results are coming soon.
- Availability: Unreleased, with planned distribution limited to trusted partners via Foundry.

## Base Models and the Role of Post-Training Alignment

A base model is a neural network trained exclusively on raw text and multi-modal data to predict upcoming tokens, lacking instruction-following capabilities. When a user submits a prompt to an unaligned base model, the system simply continues the text pattern rather than responding helpfully. Enterprise software products and conversational assistants require post-training before they can perform practical tasks.

The post-training stage adds instruction tuning, reasoning training and safety training to guide model outputs. Microsoft built MAI-Thinking-1, a general-purpose model with strong reasoning, on MAI-Base-1 and announced it in June 2026.

Because MAI-Base-2 is still in post-training, enterprise teams cannot integrate it into production pipelines or test its reasoning capabilities. Microsoft has not stated whether MAI-Base-2 will be used in Microsoft Copilot. Organizations evaluating immediate deployment options can review existing models in the guide to the best open-weights AI models.

- Pre-training objective: Pre-trained across a 50-trillion-token mixture before undergoing post-training alignment.
- Post-training requirements: Adds instruction tuning, reasoning, and safety training before serving practical applications.
- Deployment constraint: Deploying an unaligned base model produces unstructured completions instead of conversational answers or safe API responses.

## Architectural Differences Between MAI-Base-1 and MAI-Base-2

Microsoft described MAI-Base-1 as a strong model for its size, but acknowledged that it remained smaller than frontier open-weight and proprietary systems. Microsoft describes MAI-Base-2 as larger and sparser than MAI-Base-1, at 2.8 trillion total parameters.

MAI-Base-1 alternated between dense feed-forward layers and Mixture-of-Experts layers throughout its network depth. MAI-Base-2 implements a dropless Mixture-of-Experts design across every layer, with 80 billion of its 2.8 trillion parameters active per token.

MAI-Base-1 contained no native vision pre-training. MAI-Base-2 was pre-trained natively with vision, using an encoder-free vision-language architecture.

- Layer construction: MAI-Base-1 alternated dense and expert layers, whereas MAI-Base-2 uses Mixture-of-Experts in every layer.
- Vision integration: MAI-Base-1 lacked native vision, while MAI-Base-2 pre-trained natively on 4.5 trillion unique vision-language tokens.
- Token volume: MAI-Base-2 was trained on a 50-trillion-token mixture, and its unique tokens rose about 36 percent over MAI-Base-1.
- Efficiency improvement: Microsoft reported a self-measured 6.3x to 12.7x pre-training FLOPs-efficiency improvement over MAI-Base-1 (FLOPs here means floating-point operations, the total compute used in training).

## MAI-Base-2 Training Infrastructure and Data Pipeline

Microsoft AI completed the pre-training of MAI-Base-2 in 38 days across a cluster of 16,000 NVIDIA GB200 GPUs during August and September 2026. The engineering team achieved an average Model FLOPs Utilization (MFU) of 21 percent and an overall system goodput rate of 89 percent. MFU in the Mixture-of-Experts layers rose from 24 percent to 35 percent.

Microsoft trained MAI-Base-2 with the Muon optimizer instead of AdamW. Microsoft says Muon gave more stable training dynamics.

The data pipeline emphasized organic data collection without distillation from external artificial intelligence models. Microsoft processed all training tokens internally, and vision-language data made up 18 percent of the mixture, drawn from 4.5 trillion unique vision-language tokens. The knowledge cutoff date for this training mixture is March 2026.

- Compute hardware: 16,000 NVIDIA GB200 GPUs operating over a 38-day window in August and September 2026.
- Cluster efficiency: 21 percent average Model FLOPs Utilization, with MoE-layer utilization rising from 24 percent to 35 percent, and 89 percent goodput.
- Optimization: Muon optimizer selected over AdamW for more stable training dynamics.
- Organic curation: Zero synthetic distillation from other models, with all data processed through Microsoft's internal pipelines.

## Efficiency Claims and Evaluation Methodology

Microsoft self-reported that MAI-Base-2 delivers a 4.2x to 12.3x pre-training FLOPs-efficiency advantage over DeepSeek-V4-Pro-Base based on internal scaling-law fits. In its general technical disclosure, Microsoft summarized this margin as a 4x to 12x improvement in compute efficiency. The post also claimed a 6.3x to 12.7x efficiency gain over the earlier MAI-Base-1 architecture.

Performance evaluations were conducted entirely on held-out datasets rather than standard public leaderboards. Microsoft scored held-out text by bits-per-byte and held-out code from Microsoft AI's own codebase. Microsoft also used vendor-commissioned STEM, math and Q&A evaluations. Microsoft stated that all evaluation sets were decontaminated against the 50-trillion-token training mixture.

Despite claiming that MAI-Base-2 outperformed every tested open-weight model on these held-out evaluations, Microsoft published no individual benchmark numbers. No independent replication exists as of October 16, 2026, and outside researchers have no access to the weights. API prices for models you can use today are on the AI model pricing page.

## Comparison with DeepSeek-V4-Pro-Base

The primary external reference point for MAI-Base-2 in Microsoft's announcement is DeepSeek-V4-Pro-Base, an open-weight foundation model released by DeepSeek. DeepSeek-V4-Pro-Base contains 1.6 trillion total parameters with 49 billion active parameters, pre-trained on more than 32 trillion tokens. DeepSeek-V4-Pro-Base is publicly downloadable and DeepSeek-V4-Pro's weights are under the MIT license, while MAI-Base-2 remains an unreleased, closed base model.

DeepSeek published its own scores for DeepSeek-V4-Pro-Base on its DeepSeek-V4-Pro model card, including 90.1 on Massive Multitask Language Understanding (MMLU) 5-shot, 92.6 on Grade School Math 8K (GSM8K), and 76.8 on HumanEval pass@1. Microsoft published zero equivalent scores for MAI-Base-2, preventing any direct quantitative score comparison. For a detailed parameter and architectural analysis between the two systems, see the MAI-Base-2 vs DeepSeek-V4-Pro comparison.

The post-trained DeepSeek-V4-Pro model is live on the DeepSeek API. As of October 16, 2026, the DeepSeek API pricing page lists peak rates per million tokens of $0.044 for cache-hit input, $1.32 for cache-miss input and $3.96 for output. In contrast, MAI-Base-2 has no public API pricing, no context window specification, and no published output limit.

- DeepSeek-V4-Pro-Base architecture: 1.6 trillion total parameters, 49 billion active parameters, 1-million-token context window, and 8-bit Floating Point (FP8) mixed precision.
- Licensing model: DeepSeek-V4-Pro-Base is downloadable and DeepSeek-V4-Pro's weights are under the MIT license, whereas Microsoft plans to license MAI-Base-2 weights to trusted partners.
- Benchmark transparency: DeepSeek publishes nine per-benchmark scores for DeepSeek-V4-Pro-Base, while Microsoft publishes no MAI-Base-2 scores.

## Foundry Licensing Plans and the Future MAI Roadmap

Microsoft plans to license MAI-Base-2 weights to trusted partners through Foundry. The announcement outlined a partner-licensing model rather than general open-weight distribution or public consumer availability. Microsoft has not published qualification criteria, pricing schedules, or commercial availability dates for this program as of October 2026.

Engineering preparations for MAI-Base-3 are already underway at Microsoft AI. Microsoft did not share technical specifications, target parameter counts, or training timelines for MAI-Base-3 in its technical release. Microsoft stated that additional information regarding MAI-Base-2 post-training milestones will appear soon.

Enterprise teams planning AI infrastructure should treat MAI-Base-2 as an active research asset rather than an available development component. Organizations requiring immediate open-weight foundation models can read DeepSeek-V4 explained or DeepSeek's self-published scores in the DeepSeek-V4 benchmarks guide.

## Frequently Asked Questions

- No, MAI-Base-2 is not available to use as of October 2026. Microsoft announced on October 15, 2026 that pre-training is complete, and the model is now in post-training. Microsoft plans to license the model weights to trusted partners through Foundry at a future date, but no public API, chat application, or download exists today.
- MAI-Base-2 has 2.8 trillion total parameters and activates 80 billion parameters per token. It uses a dropless Mixture-of-Experts architecture in every layer, so only 80 billion of the 2.8 trillion parameters are active for each token.
- MAI-Base-2 introduces native vision pre-training, uses Mixture-of-Experts routing across every layer, and scales to 2.8 trillion total parameters. In contrast, MAI-Base-1 had no native vision pre-training and alternated between dense and expert layers. MAI-Base-2 also used about 36 percent more unique tokens.
- Microsoft has not published pricing for MAI-Base-2. Because the model is an unreleased base model undergoing post-training, there are no public API token rates or hosting fees. Microsoft plans to distribute weights to trusted partners through Foundry under terms that have not been disclosed.
- No, MAI-Base-2 is not open source or open weights. Microsoft stated that it intends to license model weights to trusted partners through Foundry. The model weights are not downloadable under open-source terms like the MIT license used by DeepSeek-V4-Pro.

### Evaluate Foundation Model Infrastructure and Strategy

Assess how emerging foundation models, sparse architectures, and enterprise licensing terms would affect an AI project's compute budget and roadmap. Book a consultation with Layer3Labs.

Book a Consultation
