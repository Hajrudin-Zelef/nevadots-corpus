---
id: collect-261121-veille-ia/deepseek/deepseek-harness-v0-1-run-the-plugin-first-stack-explainx-ai-blog
title: "DeepSeek Harness v0.1: Run the Plugin-First Stack | explainx.ai Blog"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "harness", "repository", "explainx", "official", "august", "github", "stack"]
source: https://www.explainx.ai/blog/deepseek-harness-v0-1-plugin-first-agent-stack-august-2026
source_anchor: ""
source_lines: [1, 221]
section: "DeepSeek"
---
**Update — October 2, 2026:** DeepSeek opened a worldwide public preview with an installable desktop app (macOS advertised; Windows named on Hacker News) plus the same Web UI. This August post remains the v0.1 architecture and setup note — for whether you should switch, see DeepSeek Harness desktop: macOS/Windows 2026.

**Update — August 17, 2026:** DeepSeek Harness's star count has become a story of its own. The live GitHub API reported **135,042 stars and 13,592 forks** on the `deepseek-ai/deepseek-harness` repository four days after it was created, and aggregator headlines circulating this week cite **"122K stars in 3 days"** as proof of the "fastest launch in GitHub history." The volume is real and unusually fast for an agent-runtime project — but "fastest in GitHub history" is not a claim GitHub or any primary source has verified; treat it as hype layered on top of a genuinely notable number. See how fast did it actually grow and the harness comparison table below for the corrected numbers and a grounded comparison against the tools explainx.ai readers already use.

**DeepSeek released DeepSeek Harness v0.1 in developer preview on August 13, 2026, giving agent builders a runnable open-source stack instead of another model announcement.** The official repository's central claim is unusually broad: **“Everything is a plugin.”** Models, tools, session state, filesystems, sandboxes, the agent loop, orchestration, and the Web experience all sit behind composable Cordis services.

That makes this release more consequential than a branded wrapper around DeepSeek V4 Pro. The model can change. The execution layer can change. Even the loop that decides what happens next can change. It is a concrete implementation of the separation explainx.ai covers in what an agent harness actually does.

For this release-day review, we inspected DeepSeek's official README, package metadata, CLI reference, Web UI guide, provider guide, Cordis primer, architecture document, and plugin tutorial. We did **not** execute the `npx` command in the explainx.ai repository because it would download and run a remote package.

## TL;DR: what developers are asking

| Question | Direct answer | 
|---|---|
| **Can I run it now?** | Yes. `npx @deepseek-ai/dsh web` starts a local Web UI at`http://127.0.0.1:3080` by default. | 
| **Is it only for DeepSeek models?** | No. Official docs cover DeepSeek, Anthropic, OpenAI, and custom OpenAI-compatible providers. | 
| **What is actually open source?** | The harness repository is MIT-licensed, including the runtime, UI, profiles, bundles, tools, persistence, and Cordis integration. | 
| **What version is this?** | DeepSeek calls it v0.1; the repository package metadata reported `0.1.0-rc.5` on August 13. | 
| **Is it stable?** | No. DeepSeek promises compatibility-breaking changes during the developer preview. | 
| **Why Cordis?** | It lets services declare dependencies, communicate through typed events, and cleanly undo registrations when plugins unload. | 
| **Is it a Claude Code replacement?** | It is better treated as a hackable harness substrate today, not a stable drop-in replacement for a mature coding product. | 

## How to run DeepSeek Harness today

The shortest official path needs Node.js and one command:

```
npx @deepseek-ai/dsh web
```
DeepSeek's root package currently declares Node `^22.19.0 || >=24.0.0`. The command boots the `web` profile and serves the interface at:

```
http://127.0.0.1:3080
```
Then complete three steps in the browser:

1. Open **Settings → Models** .
2. Save a DeepSeek key, add a catalog provider, or configure a custom OpenAI-compatible endpoint.
3. Choose the workspace directory before starting a session.

The invoking directory becomes the default filesystem location, but the fresh interface does not silently select it as a workspace. Once selected, the official quickstart suggests a repository-summary task. A more useful first prompt for builders is:

```
Map this repository's packages, identify the test commands, and propose one
small read-only verification task. Do not edit files or install dependencies.
```
That keeps the first run bounded while you inspect the approval flow. DeepSeek says the agent can read and edit files, run commands, delegate work, and maintain a plan, with prompts for operations covered by the active permission policy.

### Run from source instead

DeepSeek also documents a checkout path:

```
git clone https://github.com/deepseek-ai/deepseek-harness.git
cd deepseek-harness
pnpm install
pnpm run build
pnpm dsh web
```
This is the better route if your goal is to read, patch, or author plugins. It is a large TypeScript and Python monorepo, not a single-file agent loop. The repository also ships a headless profile for one-shot persisted sessions:

```
dsh --profile headless "run the tests and summarize failures"
```
Use the Web profile to inspect sessions and approvals. Use headless mode when embedding the harness in automation after you have pinned the version and permission policy.

## What “everything is a plugin” means in practice

The phrase could have been marketing shorthand. The official architecture document makes it concrete: DeepSeek Harness has **no privileged core that plugin authors must patch**. A running instance is a tree of services mounted through Cordis.

| Layer | What DeepSeek Harness makes replaceable | 
|---|---|
| **Inference** | Model adapters and streaming vocabulary through `ctx.llm` | 
| **Tools** | Scoped tool registry, schemas, pre-execution policy, and results | 
| **State** | Append-only session events, persistence, replay, fork, and resume | 
| **Control** | Agent registry, loop driver, goals, turns, steps, and continuation | 
| **Execution** | Filesystem, shell, subprocess, terminal, and sandbox providers | 
| **Composition** | Profiles, ordered bundles, patches, and runtime overlays | 
| **Experience** | Web application, conversation nodes, settings, and editor integrations | 

This extends the usual loop engineering idea. Many tools let developers add a prompt, skill, or MCP server. DeepSeek Harness lets a plugin replace the service that assembles the prompt, stores the session, executes a tool, or drives the loop.

### Cordis in five useful ideas

DeepSeek vendors Cordis as the meta-framework underneath `dsh`. Its official primer reduces the model to five parts:

1. A plugin is a function, object, or service class mounted into a context.
2. The context is a service repository with stable keys such as `ctx.tools` ,`ctx.llm` , and`ctx.sessions` .
3. Plugins declare required services through `inject` , so dependencies determine activation.
4. Typed events handle observation, wrapping, parallel work, and ordered decisions.
5. Registrations are reversible effects, so unloading a plugin removes its listeners, tools, prompt sections, and providers predictably.

The fifth point matters. Hot-swappable architecture becomes dangerous when old listeners or timers survive an unload. Cordis treats cleanup as part of registration rather than leaving every extension author to invent it.

### Profiles and bundles are the deployment unit

A **bundle** is an npm package that contributes a configuration patch. A **profile** is an ordered stack of bundles plus user overrides. DeepSeek ships `web` and `headless` templates, while `dsh-base` contributes models, tools, credentials, telemetry, persistence, filesystem access, sandboxing, and approval policy.

Inspect the effective tree without booting the app:

```
dsh --profile web --dump-config
```
That command is one of the most useful architectural decisions in the preview. “Composable” is testable only if developers can see the final composition after all bundle and patch layers have applied.

## What the agent loop records

DeepSeek's loop distinguishes a **step** from a **turn**. One step is one model request plus its tool calls. One turn can contain multiple steps until no follow-up work is owed.

The session log stores durable events for the turn, model messages, tool calls, and tool results. Live extension events can intercept the request or execution pipeline, but the architecture enforces an important invariant: anything shown to the model must be reconstructable from the log.

That is stronger than treating chat history as a UI transcript. Resume, fork, telemetry, persistence, and prompt derivation all consume the same event stream. It also connects neatly to DAG planner, worker, and critic harness patterns, where reliability depends on knowing which state is durable and which coordination event exists only in flight.

## Is DeepSeek Harness only for DeepSeek models?

No. The first-party DeepSeek card is the fastest configuration path, but the official provider guide also documents catalog entries such as Anthropic and OpenAI, native authentication for Bedrock, Vertex, Azure, and Codex, plus custom OpenAI-compatible endpoints.

That makes the name slightly misleading. DeepSeek Harness is an agent runtime from DeepSeek, not a harness locked to DeepSeek inference. You can keep the same session, tool, approval, and UI layers while replacing the model adapter.

For builders choosing the model separately from the loop, compare the release with explainx.ai's DeepSeek V4 Pro launch analysis and the broader open versus closed agent-harness roundup.

## How fast did DeepSeek Harness actually grow?

The growth curve is the most-cited fact in this story, and most of it checks out — the exact "122K in 3 days" figure just doesn't match the live API. Here is what multiple independent trackers and the GitHub API itself reported as the repository aged:

| Time since creation (Aug 13, 2026) | Reported stars | Source | 
|---|---|---|
| ~1.5 hours | ~22,000 | Aggregator flash coverage | 
| ~12 hours | ~50,000 | Multiple outlets | 
| ~28 hours | ~92,000 | Independent review roundup | 
| ~2 days | 95,386 stars, 8,826 forks | GitHub API snapshot cited by reviewers | 
| ~3 days (widely quoted headline) | "122K" | Aggregator digest — imprecise | 
| 4 days (Aug 17, 2026, checked live) | **135,042 stars, 13,592 forks** | GitHub API, `deepseek-ai/deepseek-harness` | 

Two things are true at once. First, the underlying velocity is real and exceptional for an agent-runtime project — for comparison, OpenClaw, the prior fast-growth reference point in this category, took 84 days to reach 200,000 stars, a pace DeepSeek Harness is on track to beat by a wide margin. Second, **"fastest launch in GitHub history" is not a sourced claim.** GitHub has not published an official star-velocity record, and no primary source — not DeepSeek, not GitHub — has made that claim. It appears to originate from aggregator headline inflation layered on top of a genuinely remarkable, but category-specific, growth rate. explainx.ai's read: cite the verified 135K-and-climbing figure, not the unverified superlative.

### Why the adoption happened this fast

Three factors compound here rather than any single cause:

1. **Zero-cost distribution on a trusted brand.** The harness itself is MIT-licensed and free to run; DeepSeek's reputation for shipping unusually cheap, capable models (see DeepSeek V4 Pro's launch pricing) gave the harness an audience that was already primed to try anything DeepSeek shipped.
2. **A genuinely distinct architecture pitch.** "Everything is a plugin" — including the loop, session state, and sandbox, not just tools — is a sharper claim than most harness READMEs make, and it landed at a moment when loop engineering was already a live conversation among the same builders.
3. **A one-command try-it path.**`npx @deepseek-ai/dsh web` removes the setup friction that slows adoption of most self-hosted agent tooling — evaluation cost is close to zero, which matters enormously for a GitHub star, which itself is a low-commitment action.

None of that requires the "fastest ever" framing to be a legitimate adoption story — the honest version is more interesting than the inflated one.

## DeepSeek Harness vs Claude Code, Codex, Cursor, and OpenCode

These tools overlap in job-to-be-done — an agent that reads, edits, and runs code in your repository — but differ sharply in ownership model, pricing, and how much of the internals you can touch.

| Harness | Pricing model | Default model backend | Key differentiator | Main caution | 
|---|---|---|---|---|
| **DeepSeek Harness** | Free, MIT-licensed; you pay only for whichever model API you connect | DeepSeek (first-party default); Anthropic, OpenAI, Bedrock, Vertex, Azure, and custom OpenAI-compatible endpoints all supported | Entire runtime — loop, session state, sandbox, UI — composed as swappable Cordis plugins | Developer preview with explicit breaking-change warnings; days old, thin community/security track record | 
| **Claude Code** | Included in Claude subscription tiers or pay-as-you-go API usage | Anthropic Claude models only | Low-friction, mature defaults; deep integration with Anthropic's own model roadmap | Closed orchestration core; no first-party path to a non-Anthropic model | 
| **Codex** | Included in ChatGPT plans or OpenAI API usage | OpenAI models only | Tight coupling to OpenAI's own coding-model releases and platform | Closed core; OpenAI-only model path, similar to Claude Code's Anthropic lock-in | 
| **OpenCode** | Free, open-source; bring your own model API key | Broad multi-provider support out of the box | Polished, mature open-source product across terminal, desktop, and IDE surfaces | More product-shaped than a from-scratch harness laboratory — fewer internals exposed for plugin authors | 
| **Pi** | Free, open-source; bring your own model API key | Provider-agnostic, minimal core | Deliberately small terminal core plus SDK for extension | Richer product behavior and verification are intentionally left to you | 

DeepSeek Harness is the most radical about composition and the only one of these five that treats the agent loop and session substrate themselves as replaceable plugins rather than fixed internals. Claude Code and Codex trade that flexibility for vendor-integrated polish and a single, well-tested model path. OpenCode sits in between: open-source and multi-provider, but a finished product rather than a harness laboratory. Pi is the clearest minimal core if you want to own the loop yourself without DeepSeek Harness's plugin ceremony. If you are learning the space, start with the six components of an agent harness before deciding which philosophy fits.

### Trying it today: requirements and caveats

To actually run DeepSeek Harness right now, per the official docs: Node.js `^22.19.0 || >=24.0.0`, then `npx @deepseek-ai/dsh web` (see how to run it above). A DeepSeek API key is **not required** — the provider guide supports Anthropic, OpenAI, Bedrock, Vertex, Azure, and custom OpenAI-compatible endpoints alongside DeepSeek's own models, so you can point the harness at a model you already pay for. What you should not expect four days in: a stable plugin API, a deep Stack Overflow or Discord answer base, or a security review from a third party — this is early-adoption software with real momentum, not a hardened production dependency yet.

This launch also changes how to read the older third-party DeepSeek-TUI guide. DeepSeek-TUI is a Rust terminal agent optimized around DeepSeek models. DeepSeek Harness is DeepSeek AI's official, provider-extensible Web and headless runtime. Similar name, different project and ownership.

## What people should be cautious about

### 1. The compatibility warning is explicit

The README does not bury the status: **“There will be compatibility-breaking changes.”** Do not build a production platform against unpinned preview packages and assume config schemas, service keys, or plugin interfaces will stay fixed.

Practical response:

- Pin the exact release candidate or commit.
- Keep plugins small and behind your own interfaces.
- Save the output of `--dump-config` with experiments.
- Re-run session replay and approval tests on every upgrade.

### 2. `npx` is convenient because it downloads and executes code

The one-command path is excellent for evaluation, but it is still remote package execution. Review the package and use a disposable workspace if your organization requires supply-chain controls.

The official plugin packaging guide contains an even sharper warning for Git-hosted plugins: a dependency's `prepare` script can execute package code **at install time and outside the agent sandbox** once explicitly allowlisted. Pin a commit and audit the source before granting that permission.

### 3. Agent sandboxing does not make every integration safe

The Web guide says approval prompts depend on the active permission policy. Custom model endpoints still receive the context you send them. Model credentials are stored as write-only secrets in `$DSH_HOME/.credentials.yaml`, but provider retention, workspace access, network policy, and plugin behavior remain deployment decisions.

For a wider explanation of tool boundaries, see MCP and agent capability design and browse reusable instruction packages in the explainx.ai skills directory.

### 4. Developer preview means incomplete expectations, not just bugs

There is no stable-release entry in the official GitHub Releases feed at publication time. The root package reports `0.1.0-rc.5`, the docs are changing rapidly, and the repository directs bug reports to Discussions rather than Issues. Treat current commands and provider fields as a snapshot.

## Should you try it?

Try DeepSeek Harness now if you build harness infrastructure, need an inspectable Web agent, want to experiment with swappable execution providers, or are designing plugins that should survive model changes. The combination of a visible configuration tree, durable event log, provider seams, and reversible plugin effects is technically substantive.

Wait for a stable release if you need a supported team standard, a fixed plugin API, or predictable upgrade work. Developers who mainly want an open coding agent today may still get to useful work faster with OpenCode; developers learning the smallest possible loop may prefer Pi.

The release's real contribution is not a new chat interface. It is a strong claim about agent software architecture: the model, context, tools, loop, permissions, state, and UI should be independently replaceable. DeepSeek has now published a working codebase that developers can inspect, run, and challenge on that premise.

## Related on explainx.ai

- DeepSeek Harness 1.54M DAU, Creator Mode, and marketplace plans (November 2026) — official-API user scale, plugin counts, and Creator Mode
- DeepSeek Harness desktop public preview (October 2026) — dock app, Web UI, and whether to switch from Pi, OpenCode, or Claude Code
- Cordis and spatiotemporal composability, explained — a deeper dive into the meta-framework underneath this harness's plugin architecture
- DeepSeek V4 Pro launch: Terminal-Bench, Cline, and the 0813 API
- What is an agent harness? The complete scaffolding guide
- Top 10 closed-source and open-source agent harnesses
- OpenCode: open-source terminal, desktop, and IDE agent
- Pi: Mario Zechner's minimal agent harness
- Codex vs Claude Code: agent comparison
- From ReAct to production harness: planner, worker, critic
- What is loop engineering?
- DeepSeek-TUI: the separate third-party Rust terminal agent

## Official sources

*Commands, package metadata, and developer-preview status were checked against the official DeepSeek Harness repository on August 13, 2026. Star and fork counts, growth-timeline figures, and the "fastest launch" claim were re-checked against the live GitHub API and independent coverage on August 17, 2026 — the project explicitly expects breaking changes, and the star count will have moved further by the time you read this.*
