---
id: collect-261121-veille-ia/deepseek/deepseek-v4-pro-exits-preview-with-87-9-terminalbench-score-and-open-sources-harness-agent-framework
title: "DeepSeek V4-Pro exits preview with 87.9 TerminalBench score and open-sources Harness agent framework under MIT"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-08-14"]
keywords: ["deepseek", "agent", "preview", "harness", "terminalbench", "framework", "v4-pro", "under"]
source: https://pondero.ai/news/2026-08-14-deepseek-v4-pro-ga/
source_anchor: ""
source_lines: [1, 33]
section: "DeepSeek"
---
# DeepSeek V4-Pro exits preview with 87.9 TerminalBench score and open-sources Harness agent framework under MIT

DeepSeek moved V4-Pro-0813 from a months-long preview to general availability on August 13, 2026, pushing it live across the API, the app, and the web at the same time as releasing Deepseek Harness, its internal agent framework, under the MIT license.

## What happened

The `deepseek-v4-pro` API endpoint now serves the 0813 checkpoint. Architecture and parameter count are unchanged from the preview. Per DeepSeek, existing integrations require no code changes. The 1-million-token context window and up to 384,000 output tokens carry over, with three reasoning-effort levels available: low, high, and max.

On agent benchmarks the gains were significant. Per DeepSeek's own comparison table, TerminalBench 2.1 rose from 72.1 to 87.9 and DeepSWE from 12.8 to 62.7, as reported by The Decoder. On several agent benchmarks in the same table, V4-Pro-0813 placed above Claude Opus 4.8. For broader context, Artificial Analysis puts V4-Pro's Intelligence Index at 53 (up from 45 for the preview), behind Kimi K3 at 60 and Claude Opus 5 at 63.

Two capability additions arrived with the GA build: native support for the OpenAI Responses API and dedicated Codex integration. In the consumer app and on the web, the model runs under "Expert Mode."

Alongside the model, DeepSeek published Deepseek Harness v0.1 as a Developer Preview under the MIT license. Harness turns any language model into an autonomous agent through a modular plugin system built on DeepSeek's Cordis framework. A minimal mode strips the environment down to a shell and a file editor. The framework starts via `npx` and surfaces a local web interface. DeepSeek built V4-Flash-0731's published agent benchmarks using Harness minimal mode, so the scores for both models reflect the same framework baseline.

API prices for the V4 family rise starting August 16 at 16:00 UTC, per DeepSeek. V4-Pro input tokens price at $0.435 per 1M tokens (cache miss) and output tokens at $0.87 per 1M, per the DeepSeek pricing page. The update also introduces peak and off-peak billing, with off-peak rates at half the peak-hour price, per DeepSeek.

## A big TerminalBench jump with no integration work

The jump from 72.1 to 87.9 on TerminalBench 2.1 is the number developers building coding and shell-manipulation agents will look at first. Moving from the low 70s into the high 80s puts V4-Pro-0813 at or above what most published agent benchmarks showed for top-tier models earlier this year, though the comparison relies on DeepSeek's own table and independent third-party replication of TerminalBench 2.1 is still limited. For operators already running the preview, the capability upgrade requires no integration work.

The Harness release is a distinct story. In 2026 most production agent frameworks are either proprietary or built for one provider. An MIT-licensed framework from the team that built V4-Pro, compatible with any model through its plugin system, gives developers a credible starting point for multi-provider agent pipelines without licensing overhead. The `npx` entry point and local web UI mean setup time is measured in minutes, not hours of configuration.

Developers who have been staging V4-Pro-based workflows need to account for the August 16 price change before it takes effect. The new per-token rates and the shift to peak and off-peak billing will change cost projections for anyone running high-volume inference windows outside of Chinese business hours.

## What to watch next

Independent replication of the TerminalBench 2.1 result would clarify how V4-Pro-0813 compares on an open benchmark rather than DeepSeek's internal table. Deepseek Harness is in Developer Preview: star and fork counts on its repository over the next two weeks will indicate whether the agent developer community adopts it alongside or ahead of established frameworks.

## Sources

- DeepSeek API Change Log (primary, vendor changelog)
- DeepSeek Models and Pricing (primary, pricing verification)
- The Decoder: Deepseek ships improved V4 Pro, open-sources its agent software, and raises API prices (secondary, August 13, 2026)
