---
id: collect-261121-veille-ia/deepseek/deepseek-harness-review-everything-is-a-plugin-dsh-andrew-ooo
title: "DeepSeek Harness Review: Everything Is a Plugin (dsh) — andrew.ooo"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-08-13"]
keywords: ["plugin", "harness", "deepseek", "agent", "model", "stars", "core", "august"]
source: https://andrew.ooo/posts/deepseek-harness-everything-is-a-plugin-review/
source_anchor: ""
source_lines: [1, 207]
section: "DeepSeek"
---
## TL;DR

**DeepSeek Harness** (CLI name `dsh`) is an MIT-licensed open-source agent runtime DeepSeek released on **August 13, 2026**. It is less a coding agent than the machinery a coding agent is assembled from — and its central claim is that *everything is a plugin*: the model adapter, the tool registry, the session log, the sandbox, the UI, and even the agent loop itself.

- **Absurd launch velocity** — ~50,000 GitHub stars in 12 hours, ~92,000 by hour 28, and**186,000+ stars with 20,600+ forks** as of August 23, 2026.
- **Loop-as-plugin** —`packages/core/agent-loop` is an ordinary package you swap from config. Changing Codex CLI’s loop means editing its Rust core; changing dsh’s means mounting a different plugin.
- **Built on Cordis** , a plugin framework with four years of production use inside the Koishi chatbot project.
- **Append-only session logs** with a runtime-enforced invariant:*model-visible means logged* .
- **Model-agnostic** — ~40 providers, and subagents can be delegated to a competitor’s agent.
- **Real sandboxing** — bwrap + Landlock on Linux, Seatbelt on macOS, restricted ACL tokens on Windows, fail-closed.
- **Explicitly a developer preview.** The README shouts: “THERE WILL BE COMPATIBILITY-BREAKING CHANGES.”

The honest verdict: if you want a coding agent to write code with today, this is not it. If you build agent infrastructure, the architecture here has no other working reference implementation.

## Quick Reference

| **Repository** | github.com/deepseek-ai/deepseek-harness | 
| **License** | MIT | 
| **Language** | TypeScript monorepo (~500K LOC) + ~300 lines C11 | 
| **npm package** | `@deepseek-ai/dsh` | 
| **Install** | `npx @deepseek-ai/dsh web` | 
| **Released** | August 13, 2026 | 
| **Latest release** | `dsh-v0.1.1-rc.2` (August 21, 2026) | 
| **Stars / forks** | 186,383 / 20,661 (Aug 23, 2026) | 
| **Foundation** | Cordis plugin framework | 
| **Website** | deepseek.com/harness | 

## What DeepSeek Harness Actually Is

Most coding agents share a shape: a core agent loop — take input, call the model, run tools, repeat — written as privileged code, with a ring of extension points around it (MCP servers, skills, hooks). The core is framework-private; extensions observe rather than participate.

`dsh` inverts that. Underneath sits **Cordis**, a plugin framework where plugins contribute services, typed events, and *reversible effects* to a shared context; registrations unwind when their plugin unloads. The architecture doc puts it bluntly:

There is no privileged core to patch: you extend dsh by mounting a plugin beside the others.


The minimal plugin is a file exporting an `apply(ctx)` function:

```
import type { Context } from '@deepseek-ai/cordis'
export const name = 'hello'
export function apply(ctx: Context) {
  console.log('hello from my first plugin')
}
```
Mount it with one line of YAML and it sits at the same privilege level as the model adapter or the tool registry. That is the whole thesis.

Useful backstory: when DeepSeek previously published agentic benchmark numbers, it drew criticism for vendor-reported, unreproducible scores, and promised to open-source the harness used for evaluation. Multiple signals point at `dsh` being that promise kept. That explains why the repo landed so mature on day one, with 22 contributors and top committers already at thousands of commits each.

## Why It’s Trending Now

OpenClaw, the previous velocity record holder, took 84 days to reach 200,000 stars. `dsh` did ~92,000 in 28 hours and is past 186,000 inside two weeks.

But stars are a hype metric, not adoption. What makes this significant is *who* shipped it. A frontier model lab open-sourcing its entire harness layer under MIT — decision records included — argues that the harness should not be a paid product at all. That is a shot at the business model, not at Claude Code’s feature list.

It arrived alongside the GA release of **DeepSeek-V4-Pro-0813** (1.6T parameters, 49B activated per token, 1M context), which added native OpenAI Responses API support and Codex integration. DeepSeek simultaneously moved its API from flat pricing to peak/off-peak rates — a genuine increase. Some negative sentiment around the launch is actually about that price change, not the code.

## The Architecture: Profiles, Bundles, and the Cordis Tree

A running `dsh` is a **plugin tree composed at boot from ordered layers**. A **profile** lists the bundles it stacks and keeps your own `cordis.patch.yml` (`web` and `headless` ship as templates). A **bundle** distributes Cordis config rows plus the code they mount, so whatever it inserts stays patchable from above.

`dsh-base` is the first layer of every profile — model adapters, tools, persistence, sandbox and approval policy, credentials, telemetry. `dsh-web-app` adds the browser app; `dsh-headless` a one-shot runner with no server. To dump the tree your machine actually boots:

`dsh --profile web --dump-config`
Every row it prints can be replaced by a patch of your own. The core packages contributing to that tree:

| Package | Owns | `ctx` key | 
|---|---|---|
| `core/session` | Append-only `SessionEvent` log and in-memory store | `ctx.sessions` | 
| `core/system-prompt` | Prompt-section and tool-schema assembly | `ctx.systemPrompt` | 
| `core/tools` | Scoped tool registry and guarded execution pipeline | `ctx.tools` | 
| `core/agent` | `Agent` interface, live registry,`agent/*` events | `ctx.agents` | 
| `core/agent-loop` | The default driver implementing that interface | `ctx.agentLoop` | 
| `llm/llm` | Message/stream vocabulary plus the adapter seam | `ctx.llm` | 

Note that `core/agent` (the interface) and `core/agent-loop` (the default driver) are separate packages. That separation is what makes “swap the loop” a config change rather than a fork.

## The Turn/Step Event Loop

`dsh` runs a standard ReAct pattern, but instead of a hard-coded `while` loop it decomposes into interceptable events:

```
turn/start → agent/pre-step → step/start
  → system-prompt/assemble      (prompt sections + tool schemas)
  → agent/request → llm/stream → assistant/message
  → tools/pre-execute → tools/execute → tools/post-execute
  → step/end → agent/turn-stopping → turn/end
```
A **step** is one model request plus the tools it calls. A **turn** is zero or more steps: it opens before its first input is claimed and closes once nothing is owed.

Nearly every stage is interceptable. A plugin can rewrite messages or refuse execution at `agent/pre-step`, or substitute a tool’s result at `tools/post-execute`. The loop is a public protocol, not framework-private code. Events split into three domains: **session events** (durable facts appended to the log), **agent events** (`agent/*`, carrying a live `Agent`), and **capability events** (attaching policy to a seam like `fs/*` or `tools/*` without importing the loop).

## Append-Only Session Logs

This is the least controversial strength in the whole project.

`dsh` enforces a runtime invariant: **anything that goes into a model request must be reconstructible from the session log.** The log is an append-only event stream; the history the model sees is *projected* from it. Resume, fork, search, and replay fall out of that design rather than being bolted on. Context compaction is an independent plugin too — over-budget tool results get trimmed first, then a summary node replaces a chunk of history, bracketed by three log events so a crash mid-compaction stays reconstructible.

At a moment when several vendors are encrypting reasoning traces and making agent behavior hard to audit, making full traceability an *architectural guarantee* is a real differentiator. If you need auditable agent execution records for compliance or research, this is currently unmatched in open source.

## Code Mode and Cross-Vendor Subagents

Two capabilities here genuinely do not exist elsewhere.

**Code mode (`run_code`).** Instead of emitting a dozen tool calls that each round-trip through the context window, the model writes TypeScript that batch-calls tools:

```
const files = await tools.glob({ pattern: 'src/**/*.ts' })
const hits = []
for (const f of files.slice(0, 50)) {
  const text = await tools.read({ path: f })
  if (text.includes('TODO')) hits.push(f)
}
return hits
```
Only what is printed or returned goes back to the model — a dozen round-trips collapse into one execution, directly attacking the “scattered tool calls burn the context window” problem.

**Subagents delegable to a competitor.** `ctx.subagents` supports multiple backends, and one can be a rival vendor’s agent — you can run `dsh` as the orchestration layer and delegate a subtask to Claude Code or Codex. Very few vendors would ship that.

A set of `cordis_*` tools also lets the agent write and mount plugins for itself at runtime — the seed of a self-evolving agent. Two caveats: they are **off by default** in every official preset, and a plugin written on the fly lives only in memory. It is gone on restart, with no persistence path yet.

## Getting Started

The fastest path is npm:

`npx @deepseek-ai/dsh web`
That starts the Web UI at `http://127.0.0.1:3080` and opens your browser (`--no-open` skips it; over SSH it only prints the URL). From source: clone the repo, then `pnpm install && pnpm run build && pnpm dsh web`.

In the Web UI: **Settings → Models**, enter an API key (DeepSeek’s own, or any of ~40 providers), and save — the route works immediately with no restart. Click **Choose workspace** and add your project directory; the session composer stays disabled until one is selected.

Four preset run modes ship officially: **standard** (full coding agent), **code** (TypeScript SDK orchestration), **minimal** (shell and file editor only, for benchmarks), and **creative** (runtime inspection and plugin experimentation). As one beta tester put it, `dsh` is a Lego kit and these presets are just the assembly printed on the box.

## Sandboxing and the Trust Gap

The security work is not sloppy. Linux uses bwrap + Landlock behind a custom fail-closed C launcher; macOS uses Seatbelt; Windows uses restricted ACL tokens. The approval model is a closed enumeration — anything anomalous is rejected as “unavailable” rather than silently allowed. It even distinguishes honestly between *full* and *partial* enforcement, reporting Landlock on older kernels as partial rather than overclaiming.

But there is a layering gap worth flagging loudly: **the sandbox governs tool execution, while plugins themselves run inside the harness process.** Any third-party plugin can reach the shell and filesystem directly. With a plugin repository that crossed 2,000+ submissions in two days, the trust model for community plugins amounts to good faith. Treat a `dsh` plugin like an npm postinstall script, not like an MCP server.

## Honest Limitations

This is a preview build and it shows.

**Token usage runs high.** One developer’s preliminary comparison found uncached input around **47.6K tokens for dsh versus ~4.5K for Pi** on the same model — an order of magnitude, and roughly 3x versus other frameworks. The tester flagged their own confounders, but the direction is consistent: full pluggability shows up on the bill.

**A confirmed context-duplication bug.** `dsh` reads both `CLAUDE.md` and `AGENTS.md`. Many repos keep those files identical for cross-tool compatibility — in which case the instruction set is injected **twice**, doubling the system prompt. No official fix at the time of writing.

**Third-party compatibility is thin.** The official list shows **41 compatible integrations against 219 flagged** as needing attention. One hands-on test found all five third-party tools it tried failed outright.

**The docs are inverted.** Internal architecture docs run to ~170,000 lines with CI checks preventing drift against the source, while user-facing onboarding is thin. Documentation written for agents and documentation for newcomers are different artifacts, and the latter is missing.

**Benchmark transparency questions.** V4-Pro’s SWE-bench Verified score circulates in two versions: 80.6% self-reported versus 96.4% from third-party evaluator Vals — a 16-point gap with no authoritative explanation. Official agent benchmarks ran in `dsh`’s minimal mode, raising a fair question about model capability versus framework boost. To be precise: **nobody has publicly reproduced and disproven any official number.** The skepticism is about transparency, not fabrication — and an open-source harness is what makes independent reproduction possible for the first time.

**Over-engineering for everyday work.** Hot-swapping components without stopping the runtime is a capability very few users need, while the complexity and token overhead are paid by everyone. One beta tester observed that DeepSeek’s own models frequently could not work out how to use a plugin and just edited their own code instead — faster, and about as effective.

## Comparison With Alternatives

Star counts approximate, August 2026:

| Project | Approach | License | Stars | One-liner | 
|---|---|---|---|---|
| **DeepSeek Harness** | Runtime-pluggable | MIT | ~186k | Everything is a plugin; even the loop swaps | 
| **Claude Code** | Layered extensible ecosystem | Source-available | ~141k | Skills/hooks/subagents/MCP — fullest ecosystem, heaviest | 
| **OpenAI Codex CLI** | Rust + kernel-level sandbox | Apache 2.0 | ~106k | Safety leader; declarative extensions (a folder is a plugin) | 
| **Pi** | Minimalist | MIT | ~90k | Four default tools, sub-1K-token system prompt | 
| **OpenCode** | Open ecosystem | Open source | ~197k | Broad provider support, mature TUI | 

The most illuminating debate is **dsh versus Codex**. Codex is *declarative*: a plugin is a folder on disk — a Markdown skill, an MCP config, a shell script — that never enters the harness process, so reload takes 2-3 seconds and the barrier to entry is near zero. `dsh` is *imperative*: plugins carry state, run in-process, and register into each other. Hot-swapping one at runtime means handling dangling references, terminating background tasks, and rolling back cleanly from a crash — exactly why it needed a runtime as heavy as Cordis.

The renovation analogy is apt: Codex hands you a finished apartment with a pegboard wall. `dsh` hands you a house where you can rework a load-bearing wall without cutting the water or power. The question is how often you need to rework a load-bearing wall.

Against **Pi**, the two are opposite extremes — and yet `dsh`’s adapter layer uses Pi’s own `@earendil-works/pi-ai` library. In a real sense `dsh` is a plugin skyscraper built on Pi’s model layer.

## Who Should Use This

**Use it if** you build agent infrastructure, multi-agent systems, or self-evolving agent research. The capability-surface design and event-driven loop have no other working reference implementation. Spend a weekend in the source and the `.agents/` directory — 1,386 architectural decision records classified as implemented, rejected, archived, or proposed, plus published post-mortems. As an artifact of AI-native engineering practice, that alone is worth the read.

**Watch it closely if** you need auditable, replayable agent execution records.

**Skip it for now if** you just want a coding agent to write code with. Claude Code (ecosystem), Codex CLI (safety and stability), and Pi (minimalism and cost) are all more mature today. Even a developer with a month of pre-launch access said flatly that as a daily coding agent, the experience is not as polished as Claude Code or Codex. If you’re on the fence, give it three to six months — the ecosystem will shake out and someone will reproduce the benchmarks.

## FAQ

**Is DeepSeek Harness a replacement for Claude Code?**
No — not today. It is an open, model-agnostic alternative to the *agent infrastructure* underneath Claude Code and Codex, not their full developer experience. It can inspect repos, edit files, run shell commands, search, plan, invoke skills, delegate to subagents, and enforce approval policies. It lacks hosted background agents, a finished GitHub-native PR workflow, and the IDE/desktop/mobile surface area of a mature commercial product.

**Does DeepSeek Harness only work with DeepSeek models?**
No. The adapter layer supports ~40 providers including OpenAI, Anthropic, Google, Kimi, and any OpenAI-compatible endpoint. Wiring one up is a few lines of YAML — the model is just another plugin.

**How do I install DeepSeek Harness?**
Install Node.js, then run `npx @deepseek-ai/dsh web`. The Web UI starts at `http://127.0.0.1:3080`. For a source checkout: clone the repo, `pnpm install`, `pnpm run build`, `pnpm dsh web`.

**Is it safe to run on a real codebase?**
The tool sandbox is well built — bwrap + Landlock, Seatbelt, or restricted ACL tokens by platform, fail-closed — and the Web UI asks before operations requiring approval. But plugins run inside the harness process and are *not* sandboxed, so a malicious third-party plugin has shell and filesystem access. Vet plugins as you would arbitrary npm code.

**Why is it using so many tokens?**
Structurally, the pluggable runtime assembles a much larger system prompt than minimalist harnesses. Specifically, a confirmed bug injects your instruction set twice if `CLAUDE.md` and `AGENTS.md` have identical content. Preliminary tests put uncached input around 10x Pi’s.

**Is it production-ready?**
No, and the maintainers say so in capital letters. It is a developer preview at `v0.1.1-rc.2` with promised compatibility-breaking changes.

## Sources

- deepseek-ai/deepseek-harness — repository, README, and architecture documentation
- DeepSeek Harness developer preview — official announcement page
- DeepSeek Harness launches as open source rival to Claude Code — VentureBeat, August 2026
- DeepSeek Harness In Depth: 90K Stars in Two Days — line-by-line source analysis and community claim-checking
- Cordis — the plugin framework underneath dsh
