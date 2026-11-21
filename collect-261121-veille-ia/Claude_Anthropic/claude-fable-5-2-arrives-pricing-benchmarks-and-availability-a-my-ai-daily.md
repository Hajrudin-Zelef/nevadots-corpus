---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-arrives-pricing-benchmarks-and-availability-a-my-ai-daily
title: "Claude Fable 5.2 Arrives: Pricing, Benchmarks and Availability â my ai daily"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "fable", "cache", "output", "million", "tokens", "input", "cost"]
source: https://www.myaidaily.net/news/claude-fable-5-2-pricing-benchmarks/
source_anchor: ""
source_lines: [1, 35]
section: "Claude_Anthropic"
---
THE STORY IN THREE POINTS

- Anthropic released Claude Fable 5.2 on November 9, 2026, delivering 1M token context, 128K maximum output, and cloud availability across AWS, Google Cloud, and Azure under API identifier claude-fable-5-2.
- Pricing is set at $8 per million input tokens, $40 per million output tokens, and $0.20 per million cache reads; while cache reads match Opus 5.5, base input and output rates are exactly 2.0x the Opus rate.
- Independent Artificial Analysis evaluations record Claude Fable 5.2 scores of 62 (High effort) and 65 (Max effort), while Claude Mythos 5.2 shares foundation weights under the Cyber and Life Sciences Verification Program.

## Official rollout: Release date, cloud channels and system card context

On the official Claude Fable 5.2 release date of November 9, 2026, Anthropic announced general availability for its new frontier reasoning model. Operating under the API endpoint identifier claude-fable-5-2, the model deployed simultaneously across AWS Bedrock, Google Cloud Vertex AI, and Microsoft Azure AI Foundry, alongside Anthropicâs direct Console platform. Documented specifications in Anthropicâs models overview and the accompanying Claude Fable 5.2 and Claude Mythos 5.2 System Card define a 1,000,000-token input context window, a 128,000-token maximum output generation limit, and a knowledge cutoff of June 2026.

The release materials clarify the relationship between Claude Fable 5.2 and Claude Mythos 5.2. Both models share foundation model weights, but operate under distinct deployment frameworks: Claude Fable 5.2 is generally accessible for standard developer and enterprise workloads, whereas Claude Mythos 5.2 access is restricted to vetted organizations participating in Anthropicâs Cyber and Life Sciences Verification Program for defensive biosecurity and cybersecurity evaluations. In our tracking of pre-release speculation in Claude Fable 5.5 pricing claims, we noted that earlier leaked strings carried no official confirmation; Fable 5.2 represents the confirmed commercial milestone.

## Deconstructing the rate card: Base pricing, prompt caching and batch economics

Understanding the Claude Fable 5.2 price requires reviewing Anthropicâs rate schedule across standard, prompt-cached, and batch execution modes. Standard uncached API requests are billed at $8.00 per million input tokens and $40.00 per million output tokens. Prompt caching provides substantial cost reductions for repetitive contexts: 5-minute ephemeral cache writes cost $10.00 per million tokens, 1-hour extended cache writes cost $16.00 per million tokens, and subsequent cache reads cost $0.20 per million tokens. Asynchronous Batch API processing applies a 50% discount to $4.00 input and $20.00 output, while Fast execution scales to $16.00 input and $80.00 output.

Compared to the previous generation Fable 5.1 rate card ($10.00 input, $50.00 output, $12.50 for 5-minute cache writes, $20.00 for 1-hour cache writes, and $0.25 for cache reads), Claude Fable 5.2 reflects a 20% price reduction across all five billing dimensions, lowering the operational cost for recursive agent loops and automated workflows.

## Claude Fable 5.2 vs Opus 5.5: Pricing structure and token cost comparison

Engineering teams evaluating migration paths frequently compare Claude Fable 5.2 vs Opus 5.5. While initial community chatter suggested broad price parity, examining the Fable 5.2 vs Opus 5.5 cost reality shows that parity applies strictly to cache reads. Claude Opus 5.5 prices uncached inputs at $4.00, outputs at $20.00, and 5-minute cache writes at $5.00 per million tokens. Claude Fable 5.2 ($8.00 input / $40.00 output / $10.00 5-minute cache write) is priced at exactly 2.0x Opus 5.5 across input, output, and write tiers, with both models converging at $0.20 per million tokens only for cache reads.

To illustrate the cost distinction: a workload processing 1 million uncached input tokens and generating 100,000 output tokens costs $12.00 on Claude Fable 5.2 ($8.00 + $4.00) versus $6.00 on Opus 5.5 ($4.00 + $2.00) (excluding cache write and platform overhead; illustrative token billing, not measured session expense). If that same 1 million input tokens consists entirely of cached reads, the token charge drops to $4.20 on Fable 5.2 ($0.20 + $4.00) versus $2.20 on Opus 5.5 ($0.20 + $2.00).

## Auditing Claude Fable 5.2 benchmarks: FrontierCode, HLE and Artificial Analysis Index

Anthropicâs evaluation report presents vendor-reported figures across standardized Claude Fable 5.2 benchmarks. On the FrontierCode 2.0 software engineering benchmark, Anthropic reported Fable 5.2 at 49.1% in Extra-High effort mode (compared with Opus 5.5 at 45.5% and GPT-6 Astra at 42.8%), while in High effort mode Fable 5.2 reached 45.3%. On Humanityâs Last Exam (HLE) evaluated with tools, Anthropic reported Fable 5.2 at 74.2%, compared to 67.7% for Opus 5.5.

In Table 8.1.A of the published Claude Fable 5.2 and Claude Mythos 5.2 System Card, Anthropic expands pre-deployment agent evaluations furtherâreporting 93.0% on SWE-bench Pro (up from 81.2% on Fable 5.1 and 89.9% on Opus 5.5), 69.9% on long-horizon FrontierSWE v2, and 41.3% task completion on the Scale AI / CAIS Remote Labor Index (RLI) alongside AA-Briefcase and GDPval-AA knowledge-work evaluations. Independent testing on the Artificial Analysis Intelligence Index v5.0.1 aligns with that frontier lead: High effort attained 61.82 and Max effort reached 64.56 (often summarized as 62/65 at headline launch).

Sources: Claude Fable and Mythos 5.2 Announcementï½Anthropic â Claude Model Pricing Matrix & Active Tier Rate Cardï½Anthropic Docs â Models Overview & Technical Specificationsï½Anthropic Docs â Claude Fable 5.2 Evaluation & Cost Analysisï½Artificial Analysis â

THE BROADER CONTEXT Â· CLAUDE DOSSIER

Explore full product specifications, pricing analysis, and continuous coverage in the **Claude Desk Hub** and **Claude Models Dossier**. Independent benchmarks available on the **AI Intelligence Index** and **Coding Agents Leaderboard**.
