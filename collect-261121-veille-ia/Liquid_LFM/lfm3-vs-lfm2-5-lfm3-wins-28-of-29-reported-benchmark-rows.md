---
id: collect-261121-veille-ia/liquid-lfm/lfm3-vs-lfm2-5-lfm3-wins-28-of-29-reported-benchmark-rows
title: "LFM3 vs LFM2.5: LFM3 Wins 28 of 29 Reported Benchmark Rows"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: ["2026-10-27"]
keywords: ["lfm2", "liquid", "lfm3-1", "lfm3", "benchmark", "rows", "tokens", "published"]
source: https://www.layer3labs.io/comparisons/lfm3-vs-lfm25
source_anchor: ""
source_lines: [1, 112]
section: "Liquid_LFM"
---
# LFM3 vs LFM2.5

LFM3-1.7B fits agents and long documents, while LFM2.5-2.6B fits teams with existing fine-tunes or an OpenRouter rate.

LFM3-1.7B scores higher than LFM2.5-2.6B on 28 of the 29 text benchmark rows Liquid AI publishes, with 35 percent fewer parameters and a 256K-token context window. At Layer3Labs, we build custom agents and workflow automation for business operations.

Liquid AI introduced LFM3, a family of six Liquid Foundation Models (LFMs), on October 27, 2026. LFM2.5 is a family of many models. Liquid AI benchmarks LFM3-1.7B against LFM2.5-2.6B, so those two are the pair compared here.

The single published benchmark where LFM2.5-2.6B recorded a higher score is AA-Omniscience accuracy.

## LFM3 vs. LFM2.5: Side-by-Side

| Dimension | LFM3 | LFM2.5 | 
|---|---|---|
| Parameters | 1.73B on Hugging Face card (nominal 1.7B) | 2.69B on Hugging Face card (nominal 2.6B) | 
| Context Window | 256K tokens (LFM3-1.7B model card) | 131,072 tokens on the model card (128,000 tokens listed on OpenRouter) | 
| Layers and Design | 24 layers: 13 gated short convolutions, 5 sliding-window attention, 6 global attention | 30 layers: 22 double-gated short convolutions, 8 grouped-query attention blocks | 
| Training Token Volume | 22 trillion tokens pre-training for LFM3-1.7B-Base (Liquid AI) | 34 trillion tokens training budget (Liquid AI) | 
| Text Benchmark Rows Won | 28 of 29 rows reported by Liquid AI | 1 of 29 rows reported by Liquid AI | 
| Benchmark Table Average | 49.37 average score across Liquid AI text table | 31.55 average score across Liquid AI text table | 
| Memory Footprint | About 1 GB at 4-bit precision (Liquid AI) | Under 2.5 GB of memory (Liquid AI) | 
| License Designation | LFM Open License v1.0 (Hugging Face license name lfm1.0) | LFM Open License (Hugging Face license name lfm1.0) | 
| Release Date | October 27, 2026 announcement | July 28, 2026 repository creation | 
| Hosted Application Programming Interface (API) Pricing | Not published as of launch | $0.35 input and $0.50 output per million tokens on OpenRouter | 

Are you one of these vendors? Update your listing

## Is LFM3 Better Than LFM2.5

LFM3-1.7B is better than LFM2.5-2.6B for autonomous agent loops, tool calling, instruction following, and long-context processing, according to evaluation numbers published by Liquid AI. Liquid AI publishes a 29-row text benchmark table in its launch announcement. In our internal analysis, we compared LFM3-1.7B and LFM2.5-2.6B row by row and counted LFM3 ahead on 28. Liquid AI's own average row reads 49.37 against 31.55.

The single recorded benchmark where LFM2.5-2.6B scored higher is the public AA-Omniscience factual accuracy test, where LFM2.5-2.6B scored 8.75 against 7.07 for LFM3-1.7B. However, on AA-Omniscience non-hallucination, LFM3-1.7B scored 92.93 compared to 53.69 for LFM2.5-2.6B. Liquid AI ran every benchmark row in its published launch report, and no independent third party has published comparative replication figures as of late October 2026.

If you are choosing a small open-weight model, LFM3-1.7B fits agents that call tools, work in browsers and business apps, and read long documents. LFM2.5-2.6B fits you better if you depend on its existing fine-tunes or on a published OpenRouter rate.

## Architectural and Practical Model Differences

LFM3-1.7B has fewer parameters (1.73 billion against 2.69 billion), a different layer mix and a longer context window (256K against 131,072 tokens). The LFM3-1.7B Hugging Face model card lists 1.73 billion total parameters across 24 layers, whereas the LFM2.5-2.6B Hugging Face model card specifies 2.69 billion parameters across 30 layers.

Layer distribution also differs between the two generations. LFM2.5-2.6B combines 22 double-gated short convolution blocks with 8 grouped-query attention blocks. In contrast, LFM3-1.7B introduces a tri-hybrid architecture containing 13 gated short convolutions with kernel size 7, 5 sliding-window attention layers with a 256-token window, and 6 global attention layers. Liquid AI says swapping some convolutions for sliding-window attention gave the best quality and speed tradeoff. Liquid AI adds head-wise gated attention with a sigmoid gate scaling each head output, which Liquid AI says can help suppress activation outliers that hurt quantization.

LFM3-1.7B supports a context length of 256K tokens for inputs and outputs combined, as documented on the Liquid AI LFM3-1.7B documentation page. LFM2.5-2.6B lists a context limit of 131,072 tokens on its model card, while the hosted listing on OpenRouter lists 128,000 tokens. That longer window fits longer reference documents and tool lists.

The Hugging Face cards for both models show the license name lfm1.0. The threshold below comes from the LFM3-1.7B license text. Liquid AI has not published a comparison with the LFM2.5 license text. The LFM Open License v1.0 grants commercial usage rights conditioned on the legal entity not reaching an annual revenue threshold of 10 million United States dollars. For entities with annual revenue of $10,000,000 or more, commercial use requires a separate agreement from Liquid AI. Qualified 501(c)(3) non-profit organizations remain exempt from this commercial revenue threshold for research and non-commercial purposes.

Liquid AI's model cards also list training budgets. LFM3-1.7B-Base was pre-trained on 22 trillion tokens, and the LFM2.5-2.6B card lists a 34 trillion token training budget. The two figures come from different documents and may not count the same stages, so they do not show which model saw more data.

## Text Benchmark Performance Comparison

In our internal analysis, we compared the 29 text benchmark rows Liquid AI reports for LFM3-1.7B and LFM2.5-2.6B row by row and found LFM3 ahead on 28. Across the 29 evaluated tasks, the median performance gain for LFM3-1.7B over LFM2.5-2.6B is 12.89 points.

The table groups the 29 rows by Liquid AI's own categories and averages each group. Long context shows the widest gap.

| Category | Rows | LFM3-1.7B mean | LFM2.5-2.6B mean | 
|---|---|---|---|
| Agents | 7 | 44.36 | 21.87 | 
| Tool use | 4 | 63.08 | 51.79 | 
| Agentic coding | 3 | 26.44 | 8.67 | 
| Coding | 3 | 44.56 | 32.02 | 
| Instruction following | 4 | 88.13 | 80.71 | 
| Knowledge | 6 | 30.82 | 17.09 | 
| Long context | 2 | 59.20 | 3.58 | 

The five largest gains are RULER 128K (+64.41), AA-LCR (+46.83), AutomationBench (+40.35), AA-Omniscience non-hallucination (+39.24) and WebArena-Verified (+35.40). The five smallest are IFEval (+1.27), Humanity's Last Exam without tools (+1.29), ToolSandbox (+2.32), Multi-IF (+4.30) and Humanity's Last Exam with tools (+4.30). Long-context and browser-task rows move most. Instruction following and general knowledge move least.

Liquid AI ran every score itself. Liquid AI reports 1.15 on RULER 128K for LFM2.5-2.6B and 65.56 for LFM3-1.7B and gives no reason for the gap, so test long documents from your own files before you rely on it. Readers can inspect the primary data directly in the launch documentation.

## Inference Speed and Memory Footprint

LFM3-1.7B takes about 1 GB of memory at 4-bit precision, and a full 128K-token context needs 1.6 GB of Key-Value (KV) cache. Liquid AI says it fits on any phone or laptop.

For LFM2.5-2.6B, Liquid AI's model card reports that the model operates in under 2.5 GB of system memory. Liquid AI claims generation speeds of 220 tokens per second on an Apple M5 Max chip and 113 tokens per second on an AMD Ryzen Central Processing Unit (CPU). However, Liquid AI did not publish a direct, like-for-like generation speed benchmark comparing LFM3-1.7B against LFM2.5-2.6B under identical hardware setups.

Liquid AI's blog compares LFM3-1.7B with LFM2-1.2B: per parameter it matches that model on short prompts and runs about 25 percent faster at 8K tokens of context. Because these published metrics evaluate different devices and baseline models, you should not compare the raw speed figures of LFM3-1.7B and LFM2.5-2.6B directly without local profiling.

## When LFM2.5 Is the Better Fit

Stay on LFM2.5-2.6B in four cases: - You need factual accuracy: LFM2.5-2.6B scores 8.75 on AA-Omniscience accuracy against 7.07 for LFM3-1.7B, and the LFM3-1.7B model card says to use retrieval or search for knowledge-heavy tasks.

- You rely on LFM2.5 fine-tunes: the Liquid AI migration guide lists LoRA module names for LFM2 and LFM2.5 only, so re-running the fine-tune is the cautious plan.

- You call models through a managed endpoint: OpenRouter lists LFM2.5-2.6B at $0.35 input and $0.50 output per million tokens and listed no LFM3 model on October 27, 2026.

- You need a larger model: LFM2.5-24B-A2B, LFM2.5-8B-A1B and the 1.2B and smaller models were not part of this LFM3 release.

## Upgrade Recommendations and Trigger Criteria

Upgrade to LFM3-1.7B if your workload is local agents, tool calls, structured outputs or documents up to 256K tokens. Liquid AI's table puts LFM3-1.7B ahead of LFM2.5-2.6B on all seven agent rows and all four tool-use rows, for example BFCL V4 at 71.39 against 55.89. If you plan to fine-tune, the open-weights fine-tuning guide covers the data you need.

Do not upgrade if you depend on managed API access through OpenRouter. Do not upgrade either if you rely on LFM2.5 LoRA checkpoints you cannot re-train, or need the 8B or 24B tiers. If one of those applies to you, stay on LFM2.5-2.6B or a larger LFM2.5 model. The LFM3-1.7B model card does not recommend the model for agentic coding or complex multi-step reasoning, so use a larger model for those jobs.

Our answer changes if independent evaluations disagree with Liquid AI's table, if Liquid AI publishes guidance on moving LFM2.5 fine-tunes to LFM3, or if an LFM3 API price appears. Run 20 of your own real tasks on both models, starting from the LFM3-1.7B and LFM2.5-2.6B model cards. The LFM3 pricing guide covers license terms and the LFM3 model guide covers the other five models.

## The Verdict

Based on our row-by-row count of Liquid AI's table, LFM3-1.7B is the better base for agents, tool calls and long documents: it is ahead on 28 of 29 rows with 35 percent fewer parameters. Stay on LFM2.5-2.6B if you call it through OpenRouter, rely on LFM2.5 fine-tunes you cannot re-train, or need the higher AA-Omniscience accuracy.

Researched from primary vendor documentation and public regulator sources. Pricing and availability are accurate as of Oct 27, 2026 and can change. Confirm current terms with each vendor before you buy.

## Frequently Asked Questions

- In our row-by-row count of the 29 text benchmark rows Liquid AI publishes, LFM3-1.7B scores higher on 28. It scores higher on every agent and tool-use row and on both long-context rows while using roughly 35 percent fewer parameters. The only published row where LFM2.5-2.6B scored higher was AA-Omniscience accuracy, 8.75 against 7.07, while LFM3-1.7B scores 92.93 against 53.69 on AA-Omniscience non-hallucination.
- The LFM3-1.7B model card lists 1.73 billion parameters across 24 layers. In comparison, the LFM2.5-2.6B model card lists 2.69 billion parameters across 30 layers. Liquid AI describes this change as an approximate 35 percent reduction in total parameter count.
- Liquid AI has not published guidance on whether existing LFM2.5 fine-tuned adapters transfer directly to LFM3. The Liquid AI migration guide lists LoRA module names for LFM2 and LFM2.5 architectures but omits LFM3, so re-running the fine-tune on LFM3 is the cautious plan.
- Liquid AI has not published a comparison of the two license texts. Both Hugging Face cards show the license name lfm1.0, and the LFM3-1.7B license text sets a threshold of $10,000,000 in annual revenue: below it commercial use is free, at or above it a separate agreement with Liquid AI is required.
- Liquid AI did not publish direct, head-to-head inference speed comparisons between LFM3-1.7B and LFM2.5-2.6B on identical hardware. For LFM2.5-2.6B, Liquid AI reported 220 tokens per second on an Apple M5 Max and 113 on an AMD Ryzen CPU. For LFM3-1.7B, Liquid AI reported that per parameter, it matches the older LFM2-1.2B on short prompts and runs about 25 percent faster at 8K tokens of context.

### Evaluate Small Open-Weight Models for Your Automation Workflows

Choose LFM3 for agents and long documents, and keep LFM2.5 where its fine-tunes or API access matter. Book a free audit with Layer3Labs to run your own tasks on both models.

Book a Free Audit
