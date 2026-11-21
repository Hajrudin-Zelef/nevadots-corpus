---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-s-ignite-model-drop-mai-cyber-2-flash-code-2-realtime-1-and-decision-1-1-aim-at-price-not-
title: "Microsoft's Ignite model drop: MAI Cyber 2 Flash, Code 2, Realtime 1 and Decision 1.1 aim at price, not the frontier | News"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "flash", "code", "model", "cyber", "gpt-6", "realtime", "cost"]
source: https://getaibook.com/news/microsoft-ignite-mai-cyber-2-flash-code-2-realtime-1-decision
source_anchor: ""
source_lines: [1, 63]
section: "Copilot_Microsoft"
---
Microsoft AI announced seven first-party models at Ignite on November 17, the largest batch of in-house models it has shipped at once. The umbrella post lists MAI Cyber 2 Flash, MAI Code 2, MAI Code 2 Flash, MAI Image 2.7, MAI Realtime 1, Dragon Copilot Transcribe and Decision 1.1, alongside Microsoft’s Maia 200 accelerator, which it says is now in production running its largest models at 1.7x more tokens per watt.

The common thread is cost. Almost every claim is framed as matching or narrowly beating an OpenAI or Anthropic model at a fraction of the price, and the models are built to slot into Microsoft products (GitHub Copilot, Microsoft 365, Dynamics, Dragon Copilot, Defender-adjacent security tools) where Microsoft currently pays OpenAI. Since MAI-Transcribe-1 and MAI-Image-2 in April, MAI has moved from a hedge to a lineup that covers most of the workloads Microsoft runs at scale.

## MAI Cyber 2 Flash: a cheap security model, with caveats

MAI Cyber 2 Flash is built for codebase-scale vulnerability discovery and fix recommendations. It has a 1M-token context window, four times its predecessor MAI Cyber 1.1 Flash, and output tokens are priced 86% lower than that model.

On CyberGym-E2E, Microsoft reports 92.3% task success against 91.5% for GPT-6 Astra while using half the reasoning budget; on WebExploitBench it scores 40.4% against 38.4% for GPT-5.6 Cyber. The list price used for the comparison is $0.10 per million input tokens, $0.01 cached and $0.50 output, against $10, $1 and $50 for GPT-6 Astra, which is how the keynote arrives at “under 2% of the cost.”

Read the footnotes before quoting those numbers. Microsoft found that 92 of CyberGym’s 905 tasks overlapped with its training data sources, so it excluded them and reported on 813 tasks. The prices are offline evaluation costs, not what customers pay. The comparison against OpenAI’s dedicated GPT-6 Cyber model is promised “in the coming weeks.” And the model is not a Foundry product: it is exclusive to Microsoft security products, including Codename MDASH, and replaces the existing model for eligible customers starting mid-December.

## MAI Code 2 and Code 2 Flash: aimed at everyday coding

MAI Code 2 targets production coding: debugging, multi-file changes, repo-wide reasoning and UI work. Averaged across DeepSWE 1.1, Terminal Bench 2.1, SWE-Atlas and FrontierCode v1.1, Microsoft says it reaches 98% of GPT-6.1 Sol’s score at 40% of the cost and 96% of Claude Opus 5.5’s at 5% of the cost. In a blind frontend evaluation with 258 annotators, reviewers preferred it over GPT-6 Sol 64% of the time. Microsoft is explicit that Code 2 is not tuned for scientific research or very long-horizon agentic work, which is where the frontier labs are now competing.

Code 2 Flash is the high-volume tier for explanations, targeted edits and routine fixes, roughly comparable to GPT-6 Luna and Claude Haiku 5.5 at 65% and 20% of their cost. Both are available in GitHub Copilot; Code 2 Flash is also in limited-access preview in Microsoft Foundry and is coming to OpenRouter, Vercel, Hermes, OpenCode and Kilo.

Two details matter for anyone comparing costs. All the code-model cost figures use a 50% promotional price, so the ratios will widen against Microsoft when the promotion ends. And a distilled variant, MAI Work Flash, now serves Microsoft 365 and Fabric IQ, where Microsoft says it matches GPT-6 Luna in Excel and PowerPoint and uses about 42% fewer input tokens per call than GPT-6.1 Sol in Word. That is the clearest sign of where Microsoft’s OpenAI spend is being displaced.

## Realtime 1, Image 2.7, Transcribe and Decision 1.1

- **MAI Realtime 1** is MAI’s first speech-to-speech model. It handles interruptions and background noise, connects to enterprise tools and is compatible with the OpenAI Realtime API, so existing voice agents can switch endpoints with little code change. Base pricing is $0.01 per minute, with reasoning billed separately at $1/$5 per million input/output tokens. Microsoft claims it beats GPT Realtime 2.1 on quality, speed and price on Big Bench Audio and τ-Voice but published no numbers. It is in limited-access preview on Foundry, unbilled until public preview, and is headed to Dynamics 365 Contact Center.
- **MAI Image 2.7** is in public preview on Foundry and through fal, Genspark, OpenRouter and Vercel, at about $35 per 1,000 images ($10 for the Flash variant). It ranked first on Artificial Analysis image editing as of November 16 with 1,215 Elo, 4 points ahead of GPT Image 2.6, a margin inside the noise of an arena ranking.
- **Dragon Copilot Transcribe** , built on MAI Transcribe 2, now handles 100% of doctor-patient transcription in Dragon Copilot, about 10 million encounters a month, and Microsoft says it halves multilingual error rates.
- **Decision 1.1** is the general-availability successor to October’s Microsoft-Decision-1, a routing and decision model that returns calibrated confidence so apps can defer when unsure. SiliconANGLE reports it is built on Google’s Gemma 4 26B A4 with multimodal support added; Microsoft claims it is 50x faster and 79x cheaper than GPT-6.1 Sol and leads the AA Decision Index, with 4,000 customers. It enters a category that OpenAI and AWS moved into in October.

## What it means for developers, and what to watch

Microsoft also announced MAI Licensed Weights (coming soon), which lets enterprises take a frontier MAI model, fine-tune it on their own data and run it inside their own Azure tenant. AT&T has already built a custom Code 2 Flash for 5G network analytics that Microsoft says beats GPT-6.1 Sol at 50x lower cost. On the platform side, Foundry is adding human annotation of agent traces (in preview), evaluation-driven optimization and reinforcement learning on organizational data, all aimed at agents that improve against a baseline over time.

For teams already on Azure, the practical change is that Microsoft now offers a credible cheaper default for coding assistance, voice agents and routing, with OpenAI-compatible interfaces where it counts. The open questions are the ones in the footnotes:

- **Post-promotion pricing** for Code 2 and Code 2 Flash, and whether the cost ratios still hold.
- **Independent benchmarks** , especially for Cyber 2 Flash after the contamination exclusions, and the promised GPT-6 Cyber comparison.
- **Realtime 1 numbers** once public preview starts.
- **How far Microsoft 365 moves to MAI Work Flash** , which would show how much of the OpenAI relationship Microsoft is ready to replace with its own models.

Get Insanely Good at AI

The book for developers who want to understand how AI actually works. LLMs, prompt engineering, RAG, AI agents, and production systems.

## Keep Reading

### How to Secure AI Agents With Google ADK

Learn how to secure your autonomous workflows and prevent unauthorized actions using Google ADK's hardware-backed tool binding and execution logs.

### GPT-6.2 Astra and Sol launch: 20% cheaper Astra, and OpenAI's first public model rated High for AI self-improvement

OpenAI released GPT-6.2 Astra and GPT-6.2 Sol on November 17, two months after shelving GPT-6.1 Astra. Astra drops to $8/$40 per million tokens, claims new highs on professional-work benchmarks, and is the first model OpenAI has shipped at its High threshold for AI self-improvement.

### Qwen4-Max: Alibaba's 2.4T closed model cuts prices and claims a 62% cheaper task, but trails US rivals on independent tests

Alibaba released Qwen4-Max on November 16 with a new Gated DeltaNet plus sparse-attention architecture, 71B active parameters, 1M context and $1.50/$4.50 pricing. Its own Work360 benchmark shows a 62% lower cost per task; Artificial Analysis puts it at 43, behind Kimi K3.1 Pro.

### Ruby2Shell: a one-byte overflow in Ruby's upcase gives pre-auth RCE on stock Rails, found three times over with AI help

Ruby 4.0.10, 3.4.13 and 3.3.14 fix seven memory-safety bugs, including a heap overflow triggered by uppercasing the German sharp s in legacy encodings. Two teams say it yields unauthenticated code execution on a default Rails app. Here is how it works, who found it and what to patch.

### Java4Shell: an AI-assisted audit found a pre-auth RCE in OpenJDK's Base64 decoder for under $200 in tokens

Oracle and OpenJDK patched CVE-2026-114938 on November 17, a heap overflow in HotSpot's AVX-512 Base64 intrinsic that Calif says it found with OpenAI Codex in about an hour and turned into remote code execution in Jira, Confluence, Keycloak and Jenkins. Here is how the bug works, who is affected and what to do.
