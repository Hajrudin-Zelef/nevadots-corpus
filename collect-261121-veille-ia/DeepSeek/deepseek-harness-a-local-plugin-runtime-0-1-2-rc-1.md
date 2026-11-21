---
id: collect-261121-veille-ia/deepseek/deepseek-harness-a-local-plugin-runtime-0-1-2-rc-1
title: "DeepSeek Harness â local plugin runtime, 0.1.2-rc.1"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-08-13"]
keywords: ["plugins", "runtime", "deepseek", "harness", "session", "preview", "local", "tools"]
source: https://www.harness-deepseek.org/
source_anchor: ""
source_lines: [1, 177]
section: "DeepSeek"
---
Released

## 13 Aug 2026

First public preview. npm next is 0.1.2-rc.1, published 3 September 2026.

DeepSeek Harness is a plugin-first local agent runtime. Released as a developer preview, it provides plugins for models, tools, skills, sessions, sandboxes, storage, loops, scheduling, and a web interface.

`npx @deepseek-ai/dsh web`
              
            MIT on 0.1.x. Developer preview. Node.js ^22.19.0 or >=24. Web UI on 127.0.0.1:3080.

License

0.1.x packages. Inspect, modify, and self-host under the MIT License.

Status

Compatibility-breaking changes. Not security-audited. Pin the version you run.

Runtime plugin directory

The official Settings list shows 159 installed rows. Filter by family, search by name, and open any row on the Features page.

9 plugins

Models

Model plugins, including llm. Swap the model without editing DeepSeek Harness source.

Settings â Plugins

Tools

File editing, shell, file and web search. Standard mode ships the full tool surface.

Standard mode

Skills

Skill plugins the agent can load. Missing tools are often load order or a denied Service.

Cordis plugins

Sessions

Session plugin and the append-only log. Trajectory resume, fork, search, and replay read the same stream.

Trajectory

Sandboxes

File-effect sandbox. Windows uses an ACL restricted-token backend. Do not treat prompts as containment.

windows-acl

Storage

Storage plugins. Optional SQLite session backend was removed in 0.1.2; export from an older build if you still need it.

0.1.2-rc.1

Loops

Agent loops keep the model working outside a chat box. Code / PTC can combine multi-step tool work in one TypeScript program.

Code / PTC

Scheduling

Subagent scheduling is written into the session log. Resume and replay use those records.

Session log

Interface

Local Web UI on 127.0.0.1:3080, including api-gateway. Do not expose the port.

127.0.0.1:3080

A plugin-first local runtime around models. Developer preview. MIT on 0.1.x. The Web UI binds to 127.0.0.1:3080.

Release channel

0.1.2-rc.1 on npm next, published 3 September 2026. Compatibility-breaking changes. Pin the version you run.

Install path

Node.js ^22.19.0 or >=24. npx @deepseek-ai/dsh web or clone the published tree. Unversioned npx follows latest, not next.

Architecture

Models, tools, skills, sessions, sandboxes, storage, loops, scheduling, and the UI are Cordis plugins. Compose them in configuration.

Official developer preview on npm and the published source tree. Commands on this site are shown as text, not outbound links.

First public preview 13 August 2026. npm next is 0.1.2-rc.1. npm latest is still 0.1.1-rc.2 (21 Aug 2026).

0.1.x packages. Inspect, modify, and self-host. Early 0.0.1-rc packages used BSD-3-Clause.

Cordis installs and resolves plugin dependencies. You do not edit DeepSeek Harness source to change the stack.

Runtime authors, operators, eval teams, and preset builders run dsh because capabilities live in plugins, and every run leaves a Trajectory record.

The agent reads the workspace you point at. Keep it disposable. workspace-write plus ask is the usual pin.

Standard mode ships file editing, shell, file and web search, skills, planning, goals, subagents, and workflows.

Append-only session log: prompts, reasoning, tool calls, subagent scheduling. Resume, fork, search, and replay.

Standard, Code / PTC, Minimal, and Creator. One runtime. Full table on the Features page.

DeepSeek Harness keeps the model in the environment: workspace, tools, session log, then plugins you can recombine.

01

Point dsh at a disposable workspace. Plugins load models, tools, and session state before the model proposes changes.

02

Standard mode searches, reads, patches, and runs a shell. Code / PTC can combine multi-step tool work in one TypeScript program.

03

Trajectory records prompts, reasoning, tool calls and results. Resume, fork, search, and replay use the same stream.

04

Creator mode inspects the live runtime and loads Cordis plugins in memory before you write a Profile.

0.1.2-rc.1 is still a developer preview. The useful part is a readable plugin runtime you can pin, inspect, and self-host.

A model card is not a runtime. Plugins cover tools, sessions, sandboxes, and the local Web UI.

Trajectory keeps an append-only record. That is how you resume a turn and see the exact tool payload.

Swap model, tool, or sandbox plugins in configuration. You review what you install.

Pin @deepseek-ai/[email protected]. Unversioned npx follows latest (0.1.1-rc.2), not next.

Web UI on 127.0.0.1:3080. Port 3080 is often reserved by Hyper-V, WSL2, or Docker.

The README says contracts can move before a stable. Community topic tags are discovery, not endorsement.

Official release materials on this site. Commands and repository paths are shown as text.

Download

0.1.2-rc.1 packages, integrity notes, and platform buttons.

Source tree

Published repository path. Clone is shown as text on this page.

npm

CLI package. latest 0.1.1-rc.2. next 0.1.2-rc.1. shasums on the download page.

Features

Cordis plugins, Trajectory, four modes, and sandbox policy.

Guides

Windows ACL, port 3080, pnpm load order, and upgrade titles.

Trust

Legal usage policy. Not a security audit. You review plugins you load.

No. The developer preview is MIT-licensed. You can read, change, and host it yourself. Early 0.0.1-rc packages used BSD-3-Clause; current 0.1.x packages are MIT.

Yes, with Node.js ^22.19.0 or >=24.0.0. The Windows sandbox uses an ACL restricted-token backend. Known issues include C:\Windows\Temp ACL lockouts, workspaces owned by BUILTIN\Administrators, and port 3080 reserved by Hyper-V, WSL2, or Docker.

No. It is the runtime around models: plugins for tools, sessions, sandboxes, storage, loops, scheduling, and the Web UI. Model IDs and API prices ship separately.

dsh is an npm CLI, not a Windows installer. Heuristic scanners sometimes flag Node processes that spawn shells. Check the published shasum for the version you pinned, such as fef213043313affc36ca2226d2637ad483b5e3f6 for 0.1.2-rc.1.
