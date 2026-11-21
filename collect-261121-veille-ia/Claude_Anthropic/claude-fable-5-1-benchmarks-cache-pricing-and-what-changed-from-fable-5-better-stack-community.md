---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-1-benchmarks-cache-pricing-and-what-changed-from-fable-5-better-stack-community
title: "Claude Fable 5.1: Benchmarks, Cache Pricing, and What Changed from Fable 5 | Better Stack Community"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "claude", "pricing", "effort", "cache", "better", "anthropic", "changed"]
source: https://betterstack.com/community/guides/ai/claude-fable-51/
source_anchor: ""
source_lines: [1, 53]
section: "Claude_Anthropic"
---
Anthropic released **Claude Fable 5.1 and Claude Mythos 5.1 on September 1, 2026**, twelve weeks after Fable 5 and Mythos 5 shipped and were briefly pulled from the market following a US government export control directive. The two new models **share the same underlying weights**: Fable 5.1 is the generally available version with Anthropic's production safeguards; Mythos 5.1 is the restricted-access version for vetted cybersecurity and life-sciences organizations with fewer constraints.

This article covers **what actually changed from Fable 5**, what the benchmark numbers mean and don't mean, how the pricing works in practice, and what else shipped alongside the models.

## What changed from Fable 5

The headline benchmark result is Terminal-Bench-Science 0.1, an agentic scientific research benchmark. Fable 5.1 scores 52.6% against 24.7% for Fable 5 under the same conditions. More than doubling on an agentic research benchmark is a significant jump.

On OSWorld 2.0 and AutomationBench, Fable 5.1 also leads its predecessors and GPT-5.6 Sol. One qualification from Anthropic's own methodology notes: where the production safeguards triggered on cybersecurity or biology tasks, those tasks scored zero. For cybersecurity tasks in those cases, Claude Opus 4.8 completed them; for biology tasks, Claude Opus 5 completed them. This likely reduces Fable 5.1's headline scores on those benchmarks relative to what the raw model would score without safeguards.

Mythos 5.1, the same model with fewer safeguard constraints, scored 60.9% on Terminal-Bench-Science 0.1 under its more permissive settings.

The second change is to Fable 5's biology safeguards. Anthropic updated them in a way that substantially reduces false positives, a direct response to complaints that the June iteration was triggering too aggressively on legitimate research tasks.

## Pricing: the cache read cut that actually matters

Standard token pricing is unchanged: $10 per million input tokens, $50 per million output tokens. What changed is the cost of cache reads.

Prompt caching lets you pre-process context once, save it, and reuse it across requests at a reduced rate. Agentic workloads rely on this heavily: they repeatedly revisit the same codebase, system instructions, tool definitions, documents, and accumulated conversation history. Cache read pricing dropped from $1.00 to $0.25 per million tokens, a 75% reduction.

Anthropic measured the real-world impact across four weeks of August 2026 usage across Claude Enterprise, Claude Code, and the API at default effort settings. The result: roughly 25% lower effective cost for typical workloads, and up to roughly 45% for highly agentic workloads where cached context accounts for a large fraction of each request's tokens.

That 45% figure is the more significant number for teams running Claude Code continuously, multi-step research pipelines, or agents that maintain large system prompts and tool definitions across many turns.

## The cost curve at different effort levels

One nuance the launch materials make explicit: Fable 5.1 achieves similar or better results to Fable 5 at low or medium effort, and substantially better results at high effort. This affects how the pricing lands in practice.

If you're running Fable 5.1 at low effort, you're getting Fable 5 quality or better at significantly lower effective cost thanks to cache reads. If you're running it at high effort, you're paying for more compute per request but getting meaningfully better results on complex tasks. The default on the Claude API is high effort.

For workloads that don't need frontier-level reasoning on every turn, using a lower effort setting can bring costs closer to Claude Opus 5 territory while still using the Fable 5.1 model.

## Invisible watermarking

Fable 5.1 and Mythos 5.1 are the first Claude models to include invisible watermarks in text and file outputs. Anthropic committed to adding watermarking to models released after August 2, 2026, in line with the EU AI Act.

The watermarks work by influencing word choice when several plausible options exist. They remain detectable after copying, pasting, or light editing, but are invisible to readers. Anthropic says they don't affect response quality or content, and include no information about the user, organization, or conversation.

A detection API is available to regulators, law enforcement, media organizations, fact-checkers, and independent researchers. A positive result confirms Claude produced or processed the content; a negative result doesn't prove human authorship.

## Claude Code agent-to-agent messaging

Alongside the model release, Anthropic shipped Claude Code 2.1.224 with a new feature: separate coding sessions can now message each other directly on the same machine.

Two new tools power it: `ListAgents`, which finds other active Claude Code sessions running on the machine, and `SendMessage`, which delivers a message to a specific session. This removes the need to manually copy context between terminal windows when running parallel agent workflows, and opens the door to structured multi-agent patterns where one session orchestrates and delegates to others.

## The enterprise context

Anthropic's run-rate revenue has **passed $30 billion, up from roughly $9 billion at the end of 2025**. The company's million-dollar enterprise accounts more than doubled in under two months. That growth rate is also why the cache read cut matters strategically: enterprise customers were raising concerns about unpredictable billing, with at least one company reportedly consuming its annual Anthropic budget ahead of schedule. The cache read reduction is **partly a capability improvement and partly a response to that pressure**.

Investment firm Millennium offered a concrete case study in the launch materials: Fable 5.1 **traced an extremely rare software crash to a bug inside an external vendor library after the problem had resisted explanation for four to five years**. That's the kind of long-horizon, high-context reasoning task the model is specifically built for.

Fable 5.1 is available through Anthropic's API, Amazon Bedrock, Google's Agent Platform, Microsoft Foundry, and the Claude desktop app. The new cache pricing is **rolling out in phases through fall 2026** across these platforms; enterprise customers not yet on the new pricing can contact Anthropic for early access.
