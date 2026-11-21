---
id: collect-261121-veille-ia/deepseek/deepseek-harness-autonomous-agent-tokenstead
title: "DeepSeek Harness - Autonomous agent | Tokenstead"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["harness", "deepseek", "agent", "plugin", "plugins", "github", "models", "autonomous"]
source: https://www.tokenstead.ai/autonomous-agents/deepseek-harness
source_anchor: ""
source_lines: [1, 65]
section: "DeepSeek"
---
Autonomous agents

# DeepSeek Harness

DeepSeek's open-source agent harness where everything is a plugin

DeepSeek Harness (dsh) is DeepSeek's open-source agent harness, released in developer preview under MIT and built on the Cordis meta-framework. Models, tools, skills, sessions, sandboxes, filesystems, loops, orchestration, and UI are all implemented as plugins, so a harness is composed rather than configured. `npx @deepseek-ai/dsh web` starts a local Web UI. Developer preview: expect compatibility-breaking changes.

GitHub stars

284,361

        daily snapshots

2026-09-10 to 2026-11-20
      

## What it is

DeepSeek Harness (`dsh`) is DeepSeek’s own agent harness, released into developer preview on August 13, 2026 under MIT. It crossed 50,000 GitHub stars in twelve hours and 92,000 within about a day, the fastest open-source adoption curve on record (OpenClaw needed 84 days to reach 200,000), and sits above 239,000 now. The story behind the maturity: DeepSeek had drawn criticism for vendor-reported benchmark numbers and promised to open-source the harness it evaluates its models with, and dsh is that promise kept - the codebase (TypeScript, pnpm workspace, roughly 500K lines across 57 package groups) had clearly been iterating internally for a long time before publication. Expect compatibility-breaking changes: the README says so in caps, and the release train runs weekly (v0.2.0-rc.2 landed September 29 with a desktop app that manages dsh and plugins from the menu bar without Node).

## Everything is a plugin

The design bet is the Cordis meta-framework, described in the paper “A Programming Paradigm for Spatiotemporal Composability” (arXiv 2608.25512) and battle-tested for four years inside the Koishi chatbot framework. Cordis gets specific: every registration carries a runtime-held inverse (revertible effects, so a plugin leaves no residue when it unloads) and every context change is checked against a component’s declared dependencies to trigger activation and deactivation (reactive coeffects). Models, tools, skills, sessions, sandboxes, filesystems, agent loops, orchestration, and the UI are each implemented as plugins. Nothing is baked in: a harness is composed from parts rather than configured through options, and the agent loop itself is an ordinary package that config can swap. Plugins carry the `dsh-plugin` GitHub topic, so the ecosystem is browsable on GitHub the way an app store is browsable. Creator mode is the party trick: write a plugin request in chat and the harness builds, installs, and verifies it inside the running app (the demo is a floating Pomodoro timer, five minutes end to end).

The practical consequence for model choice: the model layer is a plugin, not a constant. The adapter layer covers about forty providers, DeepSeek models are first-class, and local OpenAI-compatible endpoints (Ollama, LM Studio, vLLM) are just another provider plugin - which is the whole local-AI pitch. Prerelease builds even ship optional subagent provider bundles that run Claude Code and Codex as plugins.

## Install and run

`npx @deepseek-ai/dsh web` (Node.js) starts a Web UI at `127.0.0.1:3080` and opens it in your browser - use `--no-open` to skip the launch, and note the SSH-session case only prints the host URL since port forwarding is handled client-side. From source: clone the repo, `pnpm install && pnpm run build`, then `pnpm dsh web`. Documentation lives at deepseek-harness.github.io, with a user guide for the Web UI under `docs/user/guide`, a SAFETY.md notice, and GitHub Discussions plus a Discord for feedback.

## Where it fits for local AI

A major open-weight lab shipping its own harness is the missing piece of the local-agent stack falling into place: open models (DeepSeek V4.1 Flash, GLM 5.x, Qwen) plus a plugin-composable harness plus your own hardware is a complete, vendor-independent agent setup. The sandbox, session, and filesystem layers being plugins also means the trust boundary is inspectable - you can read what the agent’s loop and tools actually do before you let it near a machine. Treat both preview labels honestly: the README caps warning plus a real security history. CVE-2026-82533 (CVSS 9.4, disclosed September 8 by OX Security researchers via VulnCheck) let a sandboxed agent disable its own confinement with one shell command on default settings, by trusting a client-supplied HTTP Host header; DeepSeek shipped a fix in 0.1.2-alpha.1 on August 27 with token-and-cookie authentication. Their own SAFETY.md says sandboxing “does not guarantee isolation or prevent damage” - run it least-privilege in a throwaway VM or container. This is a watch-and-try project today, not a production dependency.

Sources: GitHub repo · announcement thread · docs · Cordis paper · CSA sandbox-escape research note

In use

At a glance

Primary use case

Running coding agents and everyday work on your own machine with every layer swappable: model, tools, sandbox, session, even the UI. Creator mode builds new plugins inside the running app.

Ecosystem

Cordis meta-framework (four years in production inside the Koishi chatbot framework), the dsh-plugin GitHub topic as the plugin directory, an awesome-deepseek-harness list, GitHub Discussions, and a Discord. Shipped plugins: agent teams, scheduled tasks, voice input, terminal, subagents (several experimental).

Extends

Runs web, headless, SDK, and ACP profiles on the dsh-base core; prerelease builds include optional subagent provider bundles for Claude Code and Codex. Anything OpenAI-compatible runs as the model provider, so open-weight models on your own hardware are first-class.

Who's behind it

DeepSeek AI

- License
- MIT
- Platforms
- web, desktop
- Install
- `npx @deepseek-ai/dsh web`
- Model support
- Model, tool, and UI layers are all plugins - DeepSeek, OpenAI, Anthropic, Google, Kimi, and 40+ providers via adapter plugins, including local OpenAI-compatible endpoints such as Ollama, LM Studio, and vLLM
