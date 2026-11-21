---
id: collect-261121-veille-ia/deepseek/deepseek-harness
title: "DeepSeek Harness"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-08-13"]
keywords: ["deepseek", "harness", "agent", "model", "agents", "tool", "sandbox", "plugins"]
source: https://primo.ai/wiki/DeepSeek_Harness
source_anchor: ""
source_lines: [1, 89]
section: "DeepSeek"
---
# DeepSeek Harness

- DeepSeek Harness | DeepSeek ... DeepSeek Tool Calls
- AI Agent Advisor ... Ready-to-Use AI Agents ... AI Workflows ... Agent Builders ... Agent Development Frameworks ... AI App Builders ... AI Coding Agents ... AI Software Engineering ... Agent Catalog
- Agents/Assistants ... Personal Companions ... Negotiation ... Productivity ... Email ... Web Automation ... Computer Use ... Model Context Protocol (MCP) ... Robotic Process Automation (RPA) ... LangChain
- DeepSeek Harness | DeepSeek AI - GitHub ... Official repository, README, architecture docs, and release tags. 
- DeepSeek Harness Documentation | DeepSeek AI ... Official documentation site.
- @deepseek-ai/dsh | DeepSeek AI - npm ... The official npm package.
- Safety Notice | DeepSeek AI - GitHub ... Project safety notice on sandboxing and approval limits.
- DeepSeek Harness launches as open source rival to Claude Code, alongside V4-Pro on API with higher prices | VentureBeat
- DeepSeek Open-Sources the Missing Layer Between AI Models and Agents | Ali Azhar - BigDATAwire
- DeepSeek Harness, Explained: The Agent Runtime Behind "Model + Harness = Agent" | Alistair Wren - OrcaRouter
- DeepSeek Harness Flaw Let AI Agents Disable Their Own File Sandbox Without Approval | Swati Khandelwal - The Hacker News
- DeepSeek Harness for Desktop Is Here | DeepSeek Harness - X ... v0.2 preview announcement.


DeepSeek Harness (dsh) is DeepSeek AI's open-source agent harness: the runtime layer that turns a language model into a working agent that can read and edit files, run shell commands, search the web, keep a plan, and delegate work to other agents. DeepSeek released it on August 13, 2026 as an MIT-licensed developer preview, on the same day DeepSeek-V4-Pro-0813 reached general availability. The whole design follows one rule: everything is a plugin. The model adapter, tool registry, agent loop, session storage, sandbox, scheduling, and even the Web UI are all plugins running on the Cordis framework, so developers can swap any part without patching a privileged core. DeepSeek's own shorthand for the idea is "Model + Harness = Agent": the model reasons, and the harness does everything else.

*DeepSeek Harness moves DeepSeek beyond the model itself: an open, swappable agent runtime that runs DeepSeek V4 by default, accepts rival models just as readily, and doubles as the reference environment for DeepSeek's own agentic benchmarks.*



- **The Mechanism** : Cordis acts as a small kernel that loads and unloads plugins and manages the dependencies between them. Every registration a plugin makes is an effect that unwinds when the plugin unloads. Plugins add capabilities, and presets decide which of those capabilities a given agent can see. The monorepo ships 49 packages (including core, llm, mcp, sandbox, context, plan, and goal), and launch coverage counted more than 100 first-party plugins. Every run is recorded by Trajectory, an append-only session log stored as zstd-compressed JSONL. It captures system prompts, reasoning, tool calls and results, and subagent scheduling, and you can inspect, resume, fork, search, and replay any run. Cordis's design is described in the Peking University and DeepSeek paper "A Programming Paradigm for Spatiotemporal Composability," and the framework grew out of the Koishi bot ecosystem.
- **The Physics** : Every harness runs under three hard constraints:
  - **Tokens:** Scaffolding costs tokens before any real work begins. In one hands-on test, a trivial "reply PONG" task consumed about 13,467 input tokens on its first request, because default system prompts, tool schemas, injected repository rules, and skill summaries all ride along.
  - **Isolation:** The default sandbox keeps writes inside the workspace and temporary folders, but reads and network access are not confined.
  - **Footprint:** One review measured an npm install of 531 packages and 306 MB on macOS.
  - **Pricing:** Model costs compound all three, since DeepSeek moved V4 Flash and V4 Pro to peak and off-peak billing starting August 16, 2026.
- **Why This Matters** : Harness gives developers an open, MIT-licensed alternative to closed coding agents like Claude Code and Codex, without locking them into DeepSeek's models.
  - **Model-agnostic:** Its providers guide also covers Anthropic, OpenAI, Moonshot AI, Z.ai, and any OpenAI-compatible gateway, and it can even spawn Claude Code or Codex as subagents.
  - **Checkable benchmarks:** DeepSeek measured its own models inside this harness. The V4 Flash Terminal-Bench 2.1 score of 82.7 came from Minimal mode, so anyone can now try to reproduce or challenge it.
  - **Business model:** Releasing a free harness on the same day as an API price increase signals a "free orchestration, paid inference" strategy.

### Agent Presets

Harness ships with four presets, each loading a different set of plugins:

| Preset | What It Loads | Best Used For | 
|---|---|---|
| Standard | Full coding agent: file editing, shell, file and web search, skills, planning, goals, subagents, and workflows. | The default for most work. | 
| Code (PTC, "programmatic tool calling") | Everything in Standard, plus a Code Mode SDK that lets the model write TypeScript programs chaining multi-step tool calls. | Complex orchestration. It can also multiply side effects and tool calls. | 
| Minimal | Persistent bash plus `str_replace_editor` only. | Reproducing benchmark runs. DeepSeek used it for the V4 Flash Terminal-Bench 2.1 result. | 
| Creator (preset directory named `cordis` ) | Standard plus runtime inspection, in-memory plugin experiments, and preset authoring. The agent can install, replace, or remove plugins mid-conversation. | Building and testing plugins. Not a production approval layer. | 

### Getting Started

- **Requirements:** Node.js 22.19 or newer in the 22.x line, or Node.js 24 and later.
- **Launch:** Run`npx @deepseek-ai/dsh web` . The Web UI serves at`http://127.0.0.1:3080` by default.
- **Configure:** Open Settings, then Models, paste a DeepSeek API key, and choose a workspace. The default model is DeepSeek V4 Flash, and the default permission policy is workspace-write with approval prompts for anything broader.
- **Configuration home:** Settings live in`~/.dsh` unless you set`DSH_HOME` .
- **Other entry points:**  - headless one-shot runs (`dsh --profile headless "task"` )
  - an official Python SDK (`pip install deepseek-harness-sdk` ) that drives the harness over JSON-RPC stdio
  - `--dump-config` , which prints the assembled plugin tree
- headless one-shot runs (
- **Plugins:** Add one with`dsh plugin --profile web add "github:owner/repo#ref"` . Community authors tag their repositories with the`dsh-plugin` GitHub topic.
- **From source:**`git clone https://github.com/deepseek-ai/deepseek-harness.git` , then`pnpm install` ,`pnpm run build` , and`pnpm dsh web` .
- **Name collision:** An unrelated Python package called`deepseek-harness` has been on PyPI since May 2026 and also installs a`dsh` command. The official harness is the npm package under the`@deepseek-ai` scope.

### Known Limitations

- The README warns, in capitals, that there will be compatibility-breaking changes while the project stays in developer preview.
- The project's safety notice states the software has not had a security audit and that sandboxing and approval prompts do not guarantee isolation.
- GitHub Issues is disabled; feedback and bug reports go through GitHub Discussions.
- Early reviews reported rough edges, including:
  - silent failures under MSYS2 on Windows
  - hot-reload cache staleness
  - a roughly 200 ms persistence delay that lags the UI's progress display

### Recent Developments

- **May 2026:** DeepSeek began forming its internal Harness team.
- **August 1, 2026:** Cui Tianyi, head of DeepSeek's Harness team, publicly recruited beta testers worldwide, prioritizing developers with open-source agent harness experience.
- **August 13, 2026:** DeepSeek released Harness v0.1 as an MIT-licensed developer preview, alongside the general-availability release of DeepSeek-V4-Pro-0813.
  - DeepSeek reported V4-Pro-0813 at 87.9 on Terminal-Bench 2.1 and 62.7 on DeepSWE (up from 12.8 in preview), measured in the new harness. These are vendor-reported results.
  - The same day, DeepSeek announced restructured API pricing with peak and off-peak tiers.
- **Launch adoption:** Press coverage reported roughly 50,000 GitHub stars in the first 12 hours. The repository passed 125,000 stars by August 16 and 216,000 by September 9.
  - Developers created 288 community plugin repositories within 24 hours.
  - Within days, the `dsh-plugin` topic auto-catalog held more than 2,600 repositories.
- **August 16, 2026:** The new peak and off-peak pricing took effect. Peak hours run from 9 a.m. to noon and 2 to 6 p.m. Beijing time. V4 Pro peak output rose to $3.96 per million tokens, up from $0.87.
- **August 27 to September 8, 2026:** DeepSeek fixed a sandbox-escape flaw, later tracked as CVE-2026-82533 and rated 9.4 by VulnCheck.
  - **How it worked:** The local web interface trusted the request's Host header and required no authentication. An agent steered by attacker-supplied text could call that interface to switch its own session to a "danger-full-access" mode, disabling the sandbox and approval prompts.
  - **The fix:** The interface now requires a one-time startup token, which the browser exchanges for a signed cookie.
  - **Patched versions:** The first patched npm release is 0.1.2-alpha.2 (August 30), followed by 0.1.2-rc.1 (September 3).
- **September 25, 2026:** Official desktop preview builds (Electron, version 0.1.7-rc.1) for Windows and macOS appeared on download.deepseek.com. The Mac build is notarized under Hangzhou DeepSeek Artificial Intelligence Co., Ltd.
- **September 29, 2026:** DeepSeek released the Harness v0.2 preview with desktop apps for macOS and Windows. The release adds:
  - a plugin manager that installs plugins by package name and can disable or uninstall them and show their descriptions and sources
  - improved file display, content previews, and code-change views
  - an Automation Task plugin for scheduling recurring work
  - an experimental creator mode that builds or configures plugins from a plain-language description
