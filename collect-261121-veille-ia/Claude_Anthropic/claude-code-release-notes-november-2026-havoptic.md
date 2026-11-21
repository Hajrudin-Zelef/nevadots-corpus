---
id: collect-261121-veille-ia/claude-anthropic/claude-code-release-notes-november-2026-havoptic
title: "Claude Code Release Notes — November 2026 | Havoptic"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "code", "november", "mode", "here", "tasks", "auto", "fable"]
source: https://www.havoptic.com/releases/claude-code/2026/11
source_anchor: ""
source_lines: [1, 35]
section: "Claude_Anthropic"
---
# Claude Code — November 2026 Releases

12 releases in November 2026.

## What's new in Claude Code, November 2026

**Claude Code adds Code Mode, MCP background tasks, cloud auto mode and Fable 5.2**

So far in November 2026, Claude Code has added an experimental Code Mode, background handling for MCP tasks, and a broader auto mode. Ten releases have shipped so far, from v2.1.317 to v2.1.328.

Code Mode (v2.1.323), an opt-in where Claude writes JavaScript to call its tools, now also works with agent teams (v2.1.326). MCP tasks are moved to the background automatically (v2.1.319), and a new permissions.denyInsteadOfPrompting setting denies calls that would wait on a prompt. Auto mode now starts by default for interactive sessions on Opus 4.6 and Sonnet 4.6 on Bedrock, Google Cloud's Agent Platform, Foundry and Claude apps gateway (v2.1.328). Claude Fable 5.2 became the default Fable model (v2.1.321), and fast mode arrived on Vertex (v2.1.322).

Security and permission fixes landed throughout, including MCP OAuth secrets redirected to another host, a read-only Bash command that ran outside the sandbox, and zstd response decoding. /status also now lists broken files and hooks.

- Code Mode: experimental opt-in where Claude writes JavaScript to call its tools; works with agent teams from v2.1.326 (v2.1.323)
- Auto mode starts by default for interactive sessions on Opus 4.6 and Sonnet 4.6 on Bedrock, Vertex, Foundry and gateways (v2.1.328)
- MCP tasks now move to the background; permissions.denyInsteadOfPrompting denies calls that would wait on a prompt (v2.1.319)
- Claude Fable 5.2 (claude-fable-5-2) is the default Fable model; fast mode added on Vertex (v2.1.321, v2.1.322)
- Security fixes: MCP OAuth secrets sent to a redirect host, Bash sandbox and permission gaps, zstd decoding (v2.1.321, v2.1.325)
- /status lists broken agent, skill and hook files; claude mcp add-json <name> - reads JSON from stdin (v2.1.327)

Upgrading from v2.1.316? See every change through v2.1.330.

- v2.1.330 (November 20, 2026) — Claude Code 2.1.330 is here: sign in to your org's Enterprise Admin Console, get automatic charts via the new easel tool, and point self-hosted runner
- v2.1.329 (November 19, 2026) — Claude Code 2.1.329 is here: /tasks now shows scheduled tasks and forks, /status flags marketplaces without auto-updates, and a long list of fixes cov
- v2.1.328 (November 18, 2026) — Claude Code 2.1.328 is here: auto mode now starts by default for interactive sessions on Opus 4.6 and Sonnet 4.6 across major cloud platforms. Plus, h
- v2.1.327 (November 17, 2026) — Claude Code 2.1.327 is here: safer MCP config handling, a stdin option so tokens stay off the command line, and /status now flags broken files and hoo
- v2.1.326 (November 16, 2026) — Claude Code 2.1.326 is here: Code Mode now works with agent teams, startup warnings catch malformed tool rules, and a long list of fixes tightens perm
- v2.1.325 (November 13, 2026) — Claude Code 2.1.325 is here: a security fix for zstd decoding, smarter subagent model handling, compaction that finally works on third-party gateways,
- v2.1.323 (November 12, 2026) — Claude Code 2.1.323 is here with experimental Code Mode, where Claude writes JavaScript to call its tools. Plugins now honor version ranges, and valid
- v2.1.322 (November 10, 2026) — Claude Code 2.1.322 brings fast mode to Vertex in global, us, and eu regions. If Vertex declines it, you automatically drop back to standard speed. Cu
- v2.1.321 (November 9, 2026) — Claude Code 2.1.321 is here: Fable 5.2 is now the default, remote-control takes a settings file, and a batch of permission and security gaps got patch
- v2.1.319 (November 6, 2026) — Claude Code 2.1.319 is out: MCP tasks now run in the background, auto mode behaves differently behind gateways, and a new setting can deny prompts ins
- v2.1.318 (November 3, 2026) — Claude Code 2.1.318 is here: earlier warnings before your claude.ai sign-in expires, quieter config writes, and a long list of fixes for sessions, MCP
- v2.1.317 (November 2, 2026) — Claude Code 2.1.317 is out: new cost reports for gateway admins, safer Agent SDK hooks that can block on failure, and a long list of fixes. Curious wh
