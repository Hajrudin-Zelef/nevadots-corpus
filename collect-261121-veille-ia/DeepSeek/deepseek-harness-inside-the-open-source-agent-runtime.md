---
id: collect-261121-veille-ia/deepseek/deepseek-harness-inside-the-open-source-agent-runtime
title: "DeepSeek Harness: Inside the Open-Source Agent Runtime"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-08-13"]
keywords: ["deepseek", "plugin", "agent", "runtime", "trajectory", "harness", "tool", "production"]
source: https://luby.co/blog/deepseek-harness-inside-the-open-source-agent-runtime/
source_anchor: ""
source_lines: [1, 39]
section: "DeepSeek"
---
On August 13, 2026, DeepSeek open-sourced DeepSeek Harness (dsh) as a developer preview under the MIT license, the same day its DeepSeek-V4-Pro (build 0813) model went generally available across DeepSeek’s app, web, and API (DeepSeek, TechTimes). Instead of a monolithic agent platform, dsh runs on a micro-kernel called Cordis, where the model adapter, tool registry, sandbox, agent loop, storage, and even the UI ship as swappable plugins. Every message, tool call, reasoning state, and sub-agent dispatch is written into one execution trajectory that engineers can inspect, replay, and fork. For engineering leaders weighing build-vs-buy on agent runtime and observability tooling, dsh is a concrete new data point in a larger shift: the runtime is unbundling from the model itself.

## What DeepSeek Harness Actually Is

dsh is built on Cordis, a plugin meta-framework documented in the paper “A Programming Paradigm for Spatiotemporal Composability.” The Cordis kernel handles only plugin mounting, unmounting, and dependency resolution: every functional unit of the runtime, including model adapters, the tool registry, the session/event log, the sandbox, the agent loop, storage, and scheduling, ships as an interchangeable plugin. DeepSeek’s own framing for the design is direct: “everything is a plugin” (GitHub, corroborated by InfoQ).

## A Unified Execution Trajectory Replaces Ad Hoc Logging

Every unit of runtime activity, including system prompts, reasoning states, tool calls and their results, sub-agent scheduling, and context injections, is written into an append-only execution trajectory. The web UI’s “Trajectory” view lets engineers inspect events by originating plugin, resume interrupted sessions, fork conversations, search across history, and replay exact event sequences, all against the same underlying event stream (DeepSeek). Third-party reporting additionally credits the log with capturing token-level metrics, though that detail isn’t explicit in DeepSeek’s own documentation as retrieved (InfoQ).

## Four Runtime-Mode Presets for Different Workflows

dsh v0.1 ships with four presets, each exposing a different slice of the plugin stack (DeepSeek, corroborated by InfoQ):

- **Standard** — the full toolset: file editing, shell, web/search, skills, and planning.
- **Code** — the same capabilities exposed as a TypeScript SDK for programmatic or model-orchestrated batch runs.
- **Minimal** — a persistent shell session paired with a text editor, nothing else.
- **Creator** — a diagnostic/inspection mode for testing and composing custom plugin configurations.

That range matters for evaluation work: Minimal is cheap to stand up for a narrow automation, while Creator gives a team a sandbox to stress-test plugin composition before committing to a production configuration.

## Adoption Is Fast — Production Readiness Is a Separate Question

Star-count figures vary by source and move quickly. Reports cite roughly 95,000 GitHub stars and about 8,800 forks within two days of the August 13 release, growing to an estimated 165,000 stars within a week (Flowtivity). A direct check of the repository on August 25, 2026 showed 193,700 stars (GitHub). That velocity is genuinely unusual for a developer-preview tool, but the same repository’s README labels the release a “developer preview” and warns that compatibility-breaking changes should be expected (GitHub), which is a different signal than production readiness.

## What This Means for Build-vs-Buy Decisions

The trajectory feature lands in a real gap. A LangChain-cited survey found that 89% of teams running production agents have implemented observability, but only 52% have implemented evals, a 37-point gap that O’Reilly Radar calls “where production quality dies” (O’Reilly Radar). Full trajectory capture, meaning replay, fork, and per-plugin inspection on one event stream, is aimed squarely at that gap between basic monitoring and actual debuggability.

That said, LinkedIn’s engineering team offered a general guideline for fast-moving infrastructure categories at QCon (Augment Code). A runtime that its own maintainer flags for breaking changes is a reasonable place to prototype, not necessarily to build production infrastructure on today.

“Always try to buy, don’t try to build. Only try to build if it’s simply not available, because the space is moving really fast.”


For teams still deciding, dsh’s four presets and Trajectory view offer a low-cost way to prototype observability and evaluation requirements. Spin up Minimal or Standard mode, generate real execution trajectories, and use that as the spec against which any commercial platform gets evaluated.

Independent analysis frames dsh as part of a broader move by frontier labs from pure model vendor to product/infrastructure company, the same path Anthropic took with Claude Code and OpenAI took with Codex, positioning the runtime layer as a distinct, increasingly open-sourced component separate from the underlying model (Eigent). Coverage so far is concentrated in developer- and technical-focused outlets rather than mainstream tech press, a sign this is still a developer-community story rather than a mainstream one.

The takeaway for engineering leaders isn’t “adopt dsh in production this quarter.” It’s that the runtime and the observability layer sitting underneath your agents are becoming a decision you make separately from your model choice. Use a tool like dsh to define what you actually need from that layer, then run the buy-vs-build call against real requirements instead of a vendor’s roadmap.
