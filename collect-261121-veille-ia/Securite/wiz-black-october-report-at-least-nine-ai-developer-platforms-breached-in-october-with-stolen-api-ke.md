---
id: collect-261121-veille-ia/securite/wiz-black-october-report-at-least-nine-ai-developer-platforms-breached-in-october-with-stolen-api-ke
title: "Wiz 'Black October' report: at least nine AI developer platforms breached in October, with stolen API keys feeding an LLMjacking resale market — AICoder"
domain: securite
role: reference
task: article
actors: []
dates: []
keywords: ["october", "keys", "platforms", "least", "stolen", "nine", "developer", "calls"]
source: https://aicoder.com/news/news-20261030-wiz-black-october-nine-ai-developer-platforms-breached-llmjacking-bedrock-keys
source_anchor: ""
source_lines: [1, 45]
section: "Securite"
---
Cloud security firm Wiz published research on October 30 saying at least nine AI developer platforms disclosed incidents this month: Fireworks AI, Andromeda, Zeabur, Lightning AI, OpenCode, Railway, Warp, CommandCode and Northflank. Attackers hunted stored environment variables, especially keys for paid AI models; Wiz traced two stolen enterprise keys to roughly 800,000 and 1.7 million Amazon Bedrock calls, which it estimates likely ran over $100k at list price in those two cases alone.

## Key Takeaways

- ✓Scale: between October 6 and 30, at least nine AI developer platforms disclosed incidents, and Zeabur is shutting down as a result (Wiz blog)
- ✓Methods: Fireworks, OpenCode and Warp reported signs of an "agent swarm" attack, and OpenCode and CommandCode attributed theirs to "distillation" attempts; the common thread is hunting stored environment variables that grant AI access
- ✓Case A: one stolen key drove about 800k Bedrock calls over 30 hours, about 90% successful, with Opus models making up three quarters (Wiz figures)
- ✓Case B: a single address made about 1.7 million calls in 13 hours, over a million successful; the two cases together likely exceeded $100k at list price
- ✓Resale chain: Wiz identified two sites selling discounted frontier-model access and saw both test the stolen keys

### Key Decision Metrics at a Glance

#### Turn your technical choice into a development budget

Compare 40 dev plans & simulate token costs vs $20/mo subscriptions

### Project Links & Resources

Direct Access
## In-Depth Technical Analysis

### Background and the problem

AI developer platforms keep many customer secrets in environment variables: model API keys, cloud credentials and deploy tokens. In October these platforms were hit one after another, but most coverage looked at each incident alone. In an October 30 report, Wiz researchers Benjamin Read and Rami McCarthy connect them as "Black October": at least nine platforms disclosed incidents within a month, and Zeabur is shutting down because of its breach.

### How the attacks work

According to Wiz, the root causes differ: Fireworks, Zeabur and Lightning point to credential access, Railway to privilege escalation in its build infrastructure, Warp to a third-party dependency and OpenCode to a legacy system. No single vulnerability links them; the common thread is attackers hunting environment variables that grant AI access. Wiz ties this to two trends: agentic attacks that make intrusions faster, and LLMjacking plus reseller brokers that make stolen keys easy to monetize, with distillation adding demand.

### Benchmarks

This is security research with no model benchmarks. All figures are Wiz's: two enterprise keys were first validated within 30 minutes of the vendor breach; in Case A about 800k Bedrock calls ran over 30 hours with about 90% succeeding, led by Opus 5 at 36% and Opus 5.5 at 22%; in Case B about 1.7 million calls ran over 13 hours with over a million succeeding; together the two cases likely exceeded $100k at list price.

### What developers can do

If you used any of these platforms, rotate every AI and cloud key stored there now and check Bedrock and similar bills for unusual cross-region traffic. In Wiz's Case B the key was limited to regional inference profiles, so the abuse ran into regional quotas and nearly all failed calls were throttled. Also give keys least privilege, usage alerts and short lifetimes, and avoid putting long-lived keys directly into a third-party platform's environment variables.

Benchmark side-by-side against alternatives, or calculate monthly token cost vs subscription break-even.

## Discussion & Comments

0
Sign in to join the discussion

Connect with AI developers to exchange benchmark insights.
