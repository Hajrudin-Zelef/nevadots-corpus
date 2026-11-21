---
id: collect-261121-veille-ia/deepseek/deepseek-v4-pro-launches-with-major-agent-upgrades-and-open-source-harness-memeburn
title: "DeepSeek V4 Pro Launches With Major Agent Upgrades and Open-Source Harness - Memeburn"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "agent", "model", "preview", "benchmark", "developers", "april", "harness"]
source: https://memeburn.com/deepseek-v4-pro-launches-with-major-agent-upgrades-and-open-source-harness/
source_anchor: ""
source_lines: [1, 106]
section: "DeepSeek"
---
TL;DR

- DeepSeek V4 Pro 0813 is now generally available across the web app, mobile app, and API, replacing the April preview build.
- Agent benchmark scores jumped dramatically: Terminal-Bench 2.1 rose from 72.1 to 87.9, CyberGym from 52.7 to 83.3, and DeepSWE from 12.8 to 62.7.
- The model now natively supports the OpenAI Responses API format with Codex integration out of the box.
- DeepSeek also open-sourced Harness, a plugin-based agent framework that hit over 23,000 GitHub stars within hours.
- API pricing increases take effect on August 16, with peak-hour output tokens jumping from $0.87 to $3.96 per million for V4 Pro.

DeepSeek has officially launched DeepSeek V4 Pro 0813, moving its 1.6-trillion-parameter flagship model out of preview and into general availability. The release closes a testing period that started in April and arrives alongside a new open-source agent framework and a significant API price increase.

The model is now available in Expert Mode through the DeepSeek app and website, as well as through the API using the existing model names. DeepSeek has released the model weights under the MIT License and published updated API documentation for developers migrating from the preview build.

This launch comes at a critical moment. DeepSeek’s cheaper V4-Flash model, which we covered earlier this month, unexpectedly outperformed the April preview of V4 Pro on several agent benchmarks. That was an awkward outcome for a model positioned as the company’s premium offering. The official V4 Pro release is designed to fix that gap.

## **Agent Benchmark Scores Show Massive Improvements**

The headline story is agentic performance. DeepSeek published benchmark results showing dramatic improvements over the April preview build across every agent-focused evaluation.

Here are the key numbers:

- **Terminal-Bench 2.1:** 72.1 → 87.9 (up 15.8 points)
- **CyberGym:** 52.7 → 83.3 (up 30.6 points)
- **DeepSWE:** 12.8 → 62.7 (up 49.9 points)
- **AutomationBench (Public):** 12.8 → 31.8
- **DSBench-Hard:** 31.1 → 67.2
- **NL2Repo:** 31.2 → 61.5
- **Toolathlon-Verified:** 56.9 → 74.1

The DeepSWE improvement deserves special attention. A score jump from 12.8 to 62.7 represents a 49.9 percentage-point gain, a massive leap for a software engineering benchmark. DeepSeek says this came entirely from improved post-training. The model architecture is unchanged from the preview.

These numbers put V4 Pro within striking distance of Anthropic’s frontier models on several tests. On Terminal-Bench 2.1, V4 Pro 0813 sits at 87.9 versus Claude Fable 5 at 88.0, a gap of just 0.1 points. On CyberGym, V4 Pro scored 83.3, edging past Opus 4.8’s 78.3.

**Important caveat:** these benchmarks are vendor-reported. As of publication, no independent third-party evaluator has replicated the scores. The benchmark tracker benchable.ai showed an empty section for third-party V4 Pro 0813 results. Treat these numbers as evidence of competitiveness, not confirmed rankings.

## **What V4 Pro 0813 Actually Changes for Developers**

Beyond raw benchmark numbers, the official release introduces several practical upgrades.

**Native Responses API support.** V4 Pro now works with the OpenAI Responses API format out of the box and includes built-in Codex integration. Developers can complete the Codex configuration with a one-click setup script, lowering the barrier for anyone already working within the OpenAI ecosystem.

**Flexible reasoning effort.** Both V4 Pro and V4 Flash now support three thinking effort levels: low, high, and max. This lets developers match computation intensity to task complexity. Use low for simple queries, high for daily agent workflows, and max for complex coding and reasoning tasks.

**1M-token context window** with a maximum output length of 384K tokens. The model supports both thinking and non-thinking modes, giving developers control over latency and cost.

**Tool integration.** V4 Pro already works inside popular coding tools including OpenCode Go, Cline, and Kilo Code. The OpenAI-compatible and Anthropic-compatible API endpoints mean developers can swap V4 Pro into existing agent setups with minimal configuration changes.

## **DeepSeek Harness: Open-Source Agent Framework Hits 23K+ Stars**

Alongside the V4 Pro launch, DeepSeek released Harness, an open-source agent framework built on a plugin-first architecture. The project accumulated over 23,000 GitHub stars within hours of its release.

Harness is not a model. It is the engineering layer that sits between a language model and a working developer environment: planning, tool use, file reading and writing, terminal execution, and test feedback. This is the same tool Memeburn reported on when it was still unreleased and known informally as “DeepSeek Code.”

The framework’s core idea is that “everything is a plugin.” The model adapter, tool registry, session log, and agent loop itself are all swappable plugins. It runs on Node.js via npx @deepseek-ai/dsh web, launching a web UI at localhost. The project is currently labeled a developer preview, with DeepSeek warning that compatibility-breaking changes should be expected.

Harness is powered by the Cordis meta-framework, which DeepSeek describes in a research paper on “spatiotemporal composability.” The practical takeaway: different deployers can replace models, swap storage, change security policies, add tools, and even replace the agent loop entirely without modifying the core framework.

This release positions DeepSeek directly against Anthropic’s Claude Code and OpenAI’s Codex, which are both closed-source. The open-source approach under the MIT License means any developer can inspect, modify, and self-host the entire framework.

## **API Pricing: The Cheap Era Is Ending**

The launch comes with a major pricing change. Starting August 16 at 16:00 UTC, DeepSeek is introducing peak and off-peak billing across the V4 model family.

For V4 Pro:

- **Output tokens (peak):** $3.96 per million (up from $0.87)
- **Output tokens (off-peak):** $1.98 per million
- **Input tokens, cache miss (peak):** $1.32 per million (up from $0.435)
- **Input tokens, cache miss (off-peak):** $0.66 per million

Peak hours are defined as 01:00 to 04:00 and 06:00 to 10:00 UTC. Off-peak rates are half the peak rates.

This represents a roughly 4.5x increase on output tokens during peak hours. The move ends the era of ultra-cheap pricing that made DeepSeek the disruptive force in the AI cost war throughout the first half of 2026.

Even at the new peak rates, DeepSeek remains significantly cheaper than Western competitors. Anthropic’s Fable 5 charges $50 per million output tokens, and Moonshot’s Kimi K3 costs $15. But the pricing gap has narrowed considerably. Smart developers who schedule heavy workloads during off-peak windows can still capture substantial savings.

## **Where V4 Pro Stands in the Broader AI Race**

The official V4 Pro launch arrives during one of the most competitive stretches in the AI model market. DeepSeek’s R1 model went viral in early 2025, but competitors have closed the gap. Moonshot AI’s Kimi K3, Alibaba’s Qwen3.8-Max, and ByteDance have all released competitive models in recent months.

The Vals AI leaderboard ranked V4 Pro second among open-source models, just behind Kimi K3. On the Artificial Analysis Intelligence Index, V4 Pro scored 53, compared to V4 Flash’s 40 and K3’s 57.

DeepSeek has backed its ambitions with serious capital. The company raised roughly $7.4 billion in its first round of outside funding in June, and Reuters reported additional discussions that would value it at approximately $74 billion. The company is also building a gigawatt-scale data center in Inner Mongolia and developing its own AI inference chip.

The combination of V4 Pro’s agent-level performance, Harness as an open-source alternative to Claude Code, and competitive pricing (even after the increase) gives DeepSeek a multi-layered strategy. It is no longer just a cheap model provider. It is building the full stack.

## **FAQs**

### **What is DeepSeek V4 Pro 0813?**

DeepSeek V4 Pro 0813 is the general availability release of DeepSeek’s 1.6-trillion-parameter flagship AI model. It replaces the April 2026 preview build and includes major improvements in agent capabilities, coding performance, and tool integration.

### **How much does DeepSeek V4 Pro cost?**

Before August 16, V4 Pro costs $0.435 per million input tokens and $0.87 per million output tokens. After August 16, peak-hour output pricing increases to $3.96 per million tokens, with off-peak rates at $1.98.

### **Is DeepSeek V4 Pro better than Claude or GPT models?**

On some agent benchmarks, DeepSeek V4 Pro 0813 approaches or matches Anthropic’s frontier models. It scored 87.9 on Terminal-Bench 2.1, just 0.1 points behind Fable 5. However, these are vendor-reported scores and have not been independently verified. On the Artificial Analysis Intelligence Index, V4 Pro scores lower than both Kimi K3 and Anthropic’s top models.

### **What is DeepSeek Harness?**

DeepSeek Harness is an open-source agent framework released under the MIT License alongside V4 Pro. It uses a plugin-first architecture where the model adapter, tool registry, and agent loop are all swappable components. It is designed to compete with Claude Code and OpenAI Codex.

### **Does DeepSeek V4 Pro support vision or image input?**

No. DeepSeek V4 Pro 0813 does not include vision support. This is a text-only model focused on reasoning, coding, and agentic tasks.
