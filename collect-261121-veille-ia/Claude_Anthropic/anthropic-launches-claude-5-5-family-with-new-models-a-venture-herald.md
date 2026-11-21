---
id: collect-261121-veille-ia/claude-anthropic/anthropic-launches-claude-5-5-family-with-new-models-a-venture-herald
title: "Anthropic launches Claude 5.5 family with new models â Venture Herald"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-07"]
keywords: ["claude", "anthropic", "opus", "sonnet", "model", "said", "haiku", "company"]
source: https://ventureherald.com/news/anthropic-launches-claude-5-5
source_anchor: ""
source_lines: [1, 58]
section: "Claude_Anthropic"
---
Image: AI-generated editorial illustration

Anthropic has rolled out its Claude 5.5 family in a three-step launch that began with Opus 5.5 on September 22, continued with Sonnet 5.5 on September 28 and added Haiku 5.5 on October 7, 2026. The company says the new lineup spans long-running agentic work, everyday productivity tasks and high-volume jobs, while cutting latency and costs across the product line.

Anthropic launched its Claude 5.5 family across September and October 2026, introducing Claude Opus 5.5 on September 22, Claude Sonnet 5.5 on September 28 and Claude Haiku 5.5 on October 7. The company says the three models are aimed at different workloads, from long-running agentic coding and knowledge work to everyday office tasks and high-volume, latency-sensitive use cases.

Anthropicâs newsroom says Opus 5.5 is the first model in the Claude 5.5 family and that it is intended for complex work requiring careful judgment. Sonnet 5.5 is positioned as a faster, lower-cost complement for well-scoped everyday tasks, bug fixing, and creating polished documents, slides and spreadsheets. Haiku 5.5 is described as the fastest and most capable small model Anthropic has released, tuned for summarization, compactions, database queries, classification, browser use and as a coding subagent.

## What changed with Claude Opus 5.5?

Claude Opus 5.5 is Anthropicâs top-end model in the 5.5 lineup, and the company says it is built for long-running agentic coding and knowledge-work tasks. In its announcement, Anthropic said the model performs at the level of Claude Fable 5.1 on most work and costs 40% less to run than Opus 5 on typical workloads.

Anthropic also published pricing and performance details for Opus 5.5. The model is priced at $4 per million input tokens and $20 per million output tokens, down from $5 and $25 for Opus 5. Anthropic said fast mode is available in Claude Code and the Claude Platform with up to 2.5x speed, priced at $8 per million input tokens and $40 per million output tokens. The company said the model was tested before release by external evaluators including Frontier Design and METR.

The announcement also said vetted organizations can apply immediately for the Life Sciences Verification Program, while Cyber Verification Program access is set to expand in the coming weeks. Anthropic said Opus 5.5 is deployed with safeguards similar to Claude Fable 5.1.

## How does Sonnet 5.5 fit into the lineup?

Claude Sonnet 5.5 sits between Opus and Haiku as a general-purpose model for everyday work. Anthropic said it is a clear upgrade over Sonnet 5, runs more than 30% faster, and costs up to 30% less for most work than its predecessor.

The company is pitching Sonnet 5.5 to founders, operators and individual builders who want help with coding, analysis and document creation without needing the heavier Opus model. Anthropicâs launch post says it is designed for well-scoped everyday tasks, bug fixing and producing polished documents, slides and spreadsheets.

Anthropic reported benchmark results for Sonnet 5.5 including 70.6% on Terminal-Bench 4.0, 46.2% Max on FrontierCode 1.1 Main, 55.5% on CursorBench 4.0, 1844 on GDPval-AA v2.1, 1811 on AA-Briefcase v1.1, 64.5% with tools on Humanityâs Last Exam and 80.1% partial on OSWorld 2.1. The company also said Sonnet 5.5 is the first Sonnet model to launch with cyber safeguards and fallbacks like those used for its most capable models.

Anthropic said Sonnet 5.5 is available with zero data retention and is now available across its API and major cloud platforms, including Amazon Web Services, Google Cloud and Microsoft Azure.

## Why is Haiku 5.5 important for developers and agents?

Claude Haiku 5.5 is Anthropicâs smallest new model, but the company is positioning it as a workhorse for high-volume production tasks. Anthropic says it is its fastest model to date and especially suited for live customer support and browser use.

In the dedicated Haiku 5.5 announcement, Anthropic said the model is designed for high-volume, cost-sensitive tasks including summaries, compactions, database queries and classification, and can act as a coding subagent alongside Opus 5.5 and Sonnet 5.5. The company said Haiku 5.5 costs around 75% less to run on average than Haiku 4.5.

Anthropic also disclosed usage and pricing specifics. For prompts up to 100,000 tokens, Haiku 5.5 costs $0.10 per million input tokens and $0.50 per million output tokens; above that threshold, pricing rises to $0.50 and $2.50. Anthropic said prompts up to 100,000 tokens account for around 90% of requests to its previous Haiku model, and said Haiku 5.5 is its first Haiku-class model with an adjustable effort setting for trading off cost and intelligence.

The company reported benchmark scores of 72.4% on the OSWorld 2.1 offline subset, 39.2% on Terminal-Bench 4.0, 46.4% on FrontierCode 1.1 Main, 45.9% on Humanityâs Last Exam without tools and 57.4% with tools. Anthropic said developers can access Haiku 5.5 through model ID claude-haiku-5-5 on AWS, Google Cloud and Microsoft Azure.

## What do the platform and API updates mean for customers?

Anthropic paired the model launches with platform changes that affect how developers deploy and bill workloads. On October 7, the company said Max and Team plans began including monthly API credits for running apps and agents on the Claude Platform, with Anthropic specifying $100 per month for Max users, $200 for Ultra users and up to $500 pooled across Team subscribers.

Anthropicâs API release notes also list broader availability for the new models across the Claude API, Amazon Bedrock, Claude Platform on AWS, Google-managed Claude Models on Google Cloud and Microsoft Foundry. For Haiku 5.5, the release notes add a 1-million-token context window, 128,000 maximum output tokens and adaptive thinking with an effort parameter. For Opus 5.5, the API notes say the model has a 1-million-token context window by default, 128,000 maximum output tokens and always-on adaptive thinking.

The same release notes also flag developer changes and compatibility issues. Anthropic said code written for Haiku 4.5 can break on Haiku 5.5, and that the new model may reject manual extended thinking, sampling parameters and assistant message prefill with 400 errors. It said adaptive thinking is on by default in Haiku 5.5 and that thinking text is omitted unless users set display to summarized. The platform notes also say the Haiku tokenizer produces more tokens for the same text.

For Sonnet 5.5, Anthropic said code written for Sonnet 5 can break in five ways, forced tool use returns a 400 error, and thinking blocks produced by Sonnet 5.5 only work in the account that produced them or a linked account. Anthropic also said prompt cache reads for Sonnet 5.5 were cut from $0.20 to $0.10 per million tokens on October 7.

Separately, Anthropic said it launched version two of its Code Execution Tool in beta, replacing the Python-only tool with Bash command execution and direct file manipulation, including the ability to write code in other languages. That update broadens the kinds of tasks Claude can handle inside agentic workflows, especially for developers building tools that need to execute commands or manipulate files directly.

## Sources

- Introducing Claude Haiku 5.5anthropic.comÂ· Anthropic
- Claude Sonnet 5.5anthropic.comÂ· Anthropic
- Claude Platform release notesdocs.anthropic.comÂ· Anthropic
- Claude Opus 5.5anthropic.comÂ· Anthropic
- Newsroom | Anthropicanthropic.comÂ· Anthropic
- Release notessupport.claude.comÂ· Claude Help Center
- Release notesplatform.claude.comÂ· Anthropic
- Release notessupport.claude.comÂ· Claude Help Center
