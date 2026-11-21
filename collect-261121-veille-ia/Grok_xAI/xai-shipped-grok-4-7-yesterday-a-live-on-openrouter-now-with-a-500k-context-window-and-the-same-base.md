---
id: collect-261121-veille-ia/grok-xai/xai-shipped-grok-4-7-yesterday-a-live-on-openrouter-now-with-a-500k-context-window-and-the-same-base
title: "xAI Shipped Grok 4.7 Yesterday â Live on OpenRouter Now With a 500k Context Window and the Same Base Pricing as Grok 4.6"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-09-22"]
keywords: ["grok", "openrouter", "context", "window", "same", "base", "pricing", "shipped"]
source: https://beta.cloudcodetree.com/ai-news/2026-09-22-04-grok-4-7-openrouter-api/
source_anchor: ""
source_lines: [1, 19]
section: "Grok_xAI"
---
# xAI Shipped Grok 4.7 Yesterday â Live on OpenRouter Now With a 500k Context Window and the Same Base Pricing as Grok 4.6

Chris Harper

2 min read

Sep 22, 2026 Â· 12:03 UTC

**xAI released Grok 4.7 on September 21. It is on OpenRouter as `x-ai/grok-4.7` at $1.60/$4.80 per million tokens, with a 500k context window â one string change from Grok 4.6 in any existing routing config.**

Per xAI's release notes, Grok 4.7 uses a larger base model and a longer reinforcement learning run focused on difficult tasks involving self-verification and long-context management. xAI reports improvements on software engineering benchmarks where the task runs for minutes â long agentic loops, not single-turn completions.

Grok 4.7 routes through the same platforms as Grok 4.6: OpenRouter (at the prices above), Cloudflare Workers AI, and Vercel. If you use OpenRouter for dynamic model selection in a coding agent or long-context task, `x-ai/grok-4.7` is now a valid option to add to your routing config.

**One limit:** xAI's benchmark improvements are measured on multi-step software engineering tasks. For quick, single-turn completions, the performance delta over Grok 4.6 is likely small relative to any routing overhead. No independent benchmark replication is available yet.

**Why it matters:** Another 500k-context model at sub-$2 input pricing gives model routers one more competitive option for the "need long context, budget-sensitive" slot.

**Sources:** Grok 4.7 docs â xAI Â· Grok 4.7 on OpenRouter Â· xAI releases Grok 4.7 â MarkTechPost
