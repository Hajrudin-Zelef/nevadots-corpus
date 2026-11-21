---
id: collect-261121-veille-ia/divers-tech/ai-daily-october-18-2026
title: "AI Daily｜October 18, 2026"
domain: divers-tech
role: reference
task: article
actors: []
dates: ["2026-10-18"]
keywords: ["bottom", "line", "agent", "model", "data", "provider", "meta", "commercial"]
source: https://www.communeify.com/en/blog/ai-daily-2026-10-18/
source_anchor: ""
source_lines: [1, 91]
section: "Divers_Tech"
---
DeepSeek has launched version V4.1-Flash-1017, delivering substantial performance gains in agentic and coding benchmarks alongside faster execution and lower token usage.

This brief is compiled automatically by AI and may contain errors. Check the linked sources.

Model Releases & Updates

DeepSeek-V4.1-Flash-1017DDeepSeek

Bottom line：DeepSeek has launched version V4.1-Flash-1017, delivering substantial performance gains in agentic and coding benchmarks alongside faster execution and lower token usage.

Benchmark Performance：Achieved significant improvements on complex agent evaluations, raising Terminal-Bench 4.0 scores from 31.2 to 42.3.

Pareto Efficiency：Delivers faster task completion with shorter outputs, reducing token consumption and task costs without raising API pricing.

Open-Weights Availability：Released under an MIT license with checkpoints available on Hugging Face and ModelScope for self-hosting.

REKI Vision-Language-Action ModelKKyutai, Inria & École des Ponts

Bottom line：Kyutai and academic partners have released REKI, a 2.4B VLA model designed to provide efficient memory and control for robotic manipulation.

Architecture：Built with a compact 2.4 billion parameter Vision-Language-Action architecture.

Pre-training Data：Pre-trained exclusively on public data and released on Hugging Face under an open license.

Bottom line：Vercel has introduced session affinity in AI Gateway logs to automatically route sequential chat turns to the same provider for maximum prompt caching benefits.

Routing Logic：Directs consecutive turns to the provider that served the previous request, retaining cached prompt states.

Session Analytics：Aggregates requests into session views displaying total costs, models, provider pathways, and cache hit rates.

Bottom line：DeepSeek has opened its proprietary enterprise search and web fetching APIs to developers, enabling real-time grounding for agent harnesses.

Core Capabilities：Combines multi-language web search and URL text extraction to supply verified context to LLM workflows.

Pricing Structure：Priced per successful request starting November 1, with unsuccessful or timed-out queries incurring no charge.

OpenCode Security Breach and Distillation AttackOOpenCode

Bottom line：OpenCode disclosed a security breach affecting 1.4% of its Console users linked to a large-scale distillation attack, prompting urgent API key revocations.

Impact Scope：Impacted 1.4% of OpenCode Console users who had third-party provider keys (OpenAI, Anthropic, Google) stored in the platform.

Security Remediation：Affected users were notified via email and advised to immediately revoke compromised keys and update dependent systems.

Meta Removes Content Scraping Terms from Commercial PolicyMMeta

Bottom line：Meta updated its commercial terms to remove phrasing that legal experts previously interpreted as allowing publisher domain scraping for AI model training.

Policy Adjustment：Removed specific language suggesting commercial account data could be used to train Meta products and AI models.

Official Stance：Meta clarified that its web crawler practices remain unchanged and scraping terms apply strictly to ads and commercial products.

Anthropic Workforce Study Reveals Junior Hiring ShiftAAnthropic

Bottom line：An internal Anthropic report showed that junior technical hiring dropped significantly as internal agent work hours reached six times human operating levels.

Hiring Changes：Junior technical employee representation fell from 21% to 14%, and junior hiring share decreased from 27% to 9%.

Agent Workload：Telemetry data indicated that interactive AI agent work hours scaled to six times human operator levels by August 2026.

Training Reasoning Models for Diverse Research Idea GenerationSSalesforce & Researchers

Bottom line：Researchers demonstrated an 8B reasoning model trained to propose diverse and novel research ideas in a single generation by weighting conceptual distance.

Methodology：Trains a small reasoning model to generate sets of research ideas while penalizing similarity to previously generated concepts.

Evaluation：Outperformed larger frontier models like Claude Opus 4.8 and GPT-6 Astra on creative utility and diversity metrics.

Recursive Self-Improvement for Terminal AgentsTTencent

Bottom line：Tencent researchers proposed a recursive self-improvement framework using verifier-guided fine-tuning and PPO to dramatically elevate terminal coding agents.

Performance Gains：Elevated the four-suite Terminal-Bench average from 20.68% to 50.58%, scoring 86.52% on Terminal-Bench 2.1.

Training Architecture：Alternated between fine-tuning on verified successful trajectories and running PPO with an inherited critic across five stages.

Claude Solves 25-Year-Old ECDSA Cryptography MysteryAAnthropic

Bottom line：Anthropic’s Claude successfully reverse-engineered and cracked a 25-year-old NIST elliptic curve cryptography seed originating from the NSA in under 30 minutes.

Background：Addressed the NIST standardized ECDSA curve seed from 2000 whose unspecified NSA origins long prompted backdoor speculation.

Resolution Speed：Solved the complex cryptographic mystery in less than 30 minutes using advanced chain-of-thought reasoning.

Reforming Code Review When AI Writes 90% of CodeAAnthropic (Claude Code Team)

Bottom line：The Claude Code team published an engineering breakdown detailing how they overhauled code reviews as AI agents generate roughly 90% of their codebase.

Efficiency Metrics：Despite merged pull requests doubling between June and September, code revert and incident rates dropped by over 40%.

Multi-Layered Checks：Employs a Swiss-cheese verification model featuring self-checks, independent reviewer agent pipelines, and automated CI tests.
