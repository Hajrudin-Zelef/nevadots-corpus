---
id: collect-261121-veille-ia/divers-tech/the-week-in-ai-tools-four-models-one-open-standard-and-a-watermark-361-network
title: "The week in AI tools: four models, one open standard, and a watermark — 361 Network"
domain: divers-tech
role: reference
task: article
actors: []
dates: ["2026-08-16"]
keywords: ["four", "week", "models", "copilot", "standard", "watermark", "agent", "plugins"]
source: https://www.361network.com/en/news/ai-tools-week-in-review-2026-08-16
source_anchor: ""
source_lines: [1, 48]
section: "Divers_Tech"
---
Last week was about controls. This week was about supply — four models added to Copilot in nine days, a cross-vendor plugin standard, and a step change in cloud agent startup time.

The single most consequential item is the one that is not a feature at all: Anthropic explaining, in detail, how Claude marks the text it writes.

## Models: four in nine days

The cadence is now roughly one new model every two days, and three of the four are small and fast.

- 6 Aug — Kimi K3, billed by token at $3 / $15 / $0.30 per million input, output and cached input
- 11 Aug — MAI-Code-1.1-Flash, Microsoft's small-tier model with native vision, 0.25× multiplier for annual subscribers, and a 73% lower list price than its predecessor, which retires on 10 September
- 13 Aug — Gemini 3.7 Flash, gated behind a policy whose name includes "Preview"
- 14 Aug — Grok 4.6, positioned for agentic coding and long multi-step runs

## Agent Plugins 1.0 was the structural change

On 12 August GitHub shipped support for Agent Plugins 1.0 in VS Code, the Copilot CLI, the Copilot SDK and the Copilot app. The standard was published on 6 August with AWS, Anysphere, Microsoft, OpenAI, Vercel and Google behind it.

A plugin bundles skills and MCP servers, with vendor-specific parts in namespaced directories so one package works across clients. Plugins install from the Awesome Copilot marketplace by default, and Business and Enterprise administrators govern them from managed-settings.json with enabledPlugins, extraKnownMarketplaces and strictKnownMarketplaces.

Read alongside the MCP allowlists that reached GA on 6 August, the sequencing is deliberate: the control shipped before the distribution channel opened.

## Anthropic explained the watermark

On 14 August Anthropic published how Claude's text watermarking works. It uses a key plus the preceding words to choose between equally valid word options, producing a statistical pattern that is invisible to readers, costs no extra tokens and does not affect quality.

The parts developers should take away: code is barely watermarked at all, because the mechanism needs interchangeable options and code does not offer them — though comments may carry it. Detection needs a reasonably sized passage. No watermark is not evidence of human authorship. And it carries no information identifying a user, an organisation or a conversation.

It is applied globally at launch for EU AI Act compliance, older models get it over the coming months, and a public detection tool is planned.

## Everything else that shipped

A busy week underneath the headlines:

- 11 Aug — Copilot for JetBrains gained memory across chat sessions and Ollama as a BYOK provider, which is the first configuration that satisfies "the code never leaves the building"
- 11 Aug — the usage report gained a per-model token breakdown: input, output, cache read and cache write with the credits each cost
- 12 Aug — organization-level rule insights entered public preview, reporting allowed, failed and bypassed runs across every repository
- 13 Aug — Cursor shipped Builds, pre-prepared dev environments that make cloud agents start three times faster, free and automatic for new environments
- 13 Aug — GitHub improved license detection, cutting missing licenses across 170 million packages from 45% to 24%
- 14 Aug — OAuth apps got 8-hour access tokens with 6-month refresh tokens and up to ten redirect URIs, with wildcard matching now visible on legacy single-URI apps

## What to actually do

Three of these have a deadline or a default attached, which puts them ahead of the rest.

- Check whether wildcard redirect matching is enabled on your OAuth apps, and turn it off where it is not needed — this is the cheapest security action on the list
- Enable MAI-Code-1.1-Flash before 10 September, when MAI-Code-1-Flash stops existing rather than degrading
- Decide a posture on agent plugins before people start installing them, because one-click adoption curves are steep
- Answer the four model policies sitting in your admin console, even if the answer to some is no
