---
id: collect-261121-veille-ia/claude-anthropic/what-s-new-in-claude-code-november-2026-updates-havoptic
title: "What's New in Claude Code — November 2026 Updates | Havoptic"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-20"]
keywords: ["claude", "code", "mode", "default", "sandbox", "november", "added", "haiku"]
source: https://www.havoptic.com/whats-new/claude-code
source_anchor: ""
source_lines: [1, 46]
section: "Claude_Anthropic"
---
# What's new in Claude Code

The latest Claude Code release is v2.1.330, released Nov 20, 2026. 18 releases shipped in the last 30 days.

## November 2026: Claude Code adds Code Mode, MCP background tasks, cloud auto mode and Fable 5.2

So far in November 2026, Claude Code has added an experimental Code Mode, background handling for MCP tasks, and a broader auto mode. Ten releases have shipped so far, from v2.1.317 to v2.1.328.

Code Mode (v2.1.323), an opt-in where Claude writes JavaScript to call its tools, now also works with agent teams (v2.1.326). MCP tasks are moved to the background automatically (v2.1.319), and a new permissions.denyInsteadOfPrompting setting denies calls that would wait on a prompt. Auto mode now starts by default for interactive sessions on Opus 4.6 and Sonnet 4.6 on Bedrock, Google Cloud's Agent Platform, Foundry and Claude apps gateway (v2.1.328). Claude Fable 5.2 became the default Fable model (v2.1.321), and fast mode arrived on Vertex (v2.1.322).

Security and permission fixes landed throughout, including MCP OAuth secrets redirected to another host, a read-only Bash command that ran outside the sandbox, and zstd response decoding. /status also now lists broken files and hooks.

- Code Mode: experimental opt-in where Claude writes JavaScript to call its tools; works with agent teams from v2.1.326 (v2.1.323)
- Auto mode starts by default for interactive sessions on Opus 4.6 and Sonnet 4.6 on Bedrock, Vertex, Foundry and gateways (v2.1.328)
- MCP tasks now move to the background; permissions.denyInsteadOfPrompting denies calls that would wait on a prompt (v2.1.319)
- Claude Fable 5.2 (claude-fable-5-2) is the default Fable model; fast mode added on Vertex (v2.1.321, v2.1.322)
- Security fixes: MCP OAuth secrets sent to a redirect host, Bash sandbox and permission gaps, zstd decoding (v2.1.321, v2.1.325)
- /status lists broken agent, skill and hook files; claude mcp add-json <name> - reads JSON from stdin (v2.1.327)

## October 2026: Claude Code adds Claude Mods, idle compaction, Haiku 5.5 and tighter sandbox checks

Claude Code's main changes in October 2026 were Claude Mods, which let plugins modify deeper behavior, plus idle compaction, a new default Haiku model and a long run of permission and sandbox fixes. Twenty-six releases shipped, from v2.1.287 to v2.1.316.

v2.1.293 added Claude Haiku 5.5 (claude-haiku-5-5) as the default Haiku model on the Anthropic API. Idle compaction in the terminal (v2.1.299) compacts a 200K+ token conversation left idle before its prompt cache expires, with an "idleCompaction": false setting to turn it off, and v2.1.314 extended it to VS Code and Claude Desktop. Hooks gained onFailure: "block" (v2.1.295), /code-review gained --max-findings (v2.1.288), and v2.1.314 added permissions.blockWritesOutsideWorkingDirectories. v2.1.309 closed several sandbox and Bash permission gaps. Later releases added many Claude apps gateway options for admins. v2.1.316 notes that Opus 4.8 fast mode retires before November 30, 2026.

- Claude Mods let plugins modify deeper behavior, with many mod hooks added through the month (v2.1.287)
- Claude Haiku 5.5 (claude-haiku-5-5) becomes the default Haiku model on the Anthropic API (v2.1.293)
- Idle compaction compacts 200K+ token conversations before the prompt cache expires; extended to VS Code and Desktop (v2.1.299, v2.1.314)
- onFailure: "block" makes command and HTTP hooks block the action when they fail (v2.1.295)
- permissions.blockWritesOutsideWorkingDirectories stops Write, Edit and NotebookEdit outside working directories (v2.1.314)
- Sandbox and Bash permission check fixes, including startup files running outside the sandbox on macOS and Linux (v2.1.309)

## Latest Claude Code releases

- v2.1.330 (Nov 20, 2026) — Claude Code 2.1.330 is here: sign in to your org's Enterprise Admin Console, get automatic charts via the new easel tool, and point self-hosted runners at a specific folder. Plus a long list of fixes
- v2.1.329 (Nov 19, 2026) — Claude Code 2.1.329 is here: /tasks now shows scheduled tasks and forks, /status flags marketplaces without auto-updates, and a long list of fixes covers sign-outs, /teleport, and file handling. Curio
- v2.1.328 (Nov 18, 2026) — Claude Code 2.1.328 is here: auto mode now starts by default for interactive sessions on Opus 4.6 and Sonnet 4.6 across major cloud platforms. Plus, hook failures can now block actions. Curious what e
- v2.1.327 (Nov 17, 2026) — Claude Code 2.1.327 is here: safer MCP config handling, a stdin option so tokens stay off the command line, and /status now flags broken files and hooks. Plus a long list of crash and resume fixes. Cu
- v2.1.326 (Nov 16, 2026) — Claude Code 2.1.326 is here: Code Mode now works with agent teams, startup warnings catch malformed tool rules, and a long list of fixes tightens permissions, /rewind, and background sessions. Update
- v2.1.325 (Nov 13, 2026) — Claude Code 2.1.325 is here: a security fix for zstd decoding, smarter subagent model handling, compaction that finally works on third-party gateways, and a long list of auth and MCP fixes. Curious wh
- v2.1.323 (Nov 12, 2026) — Claude Code 2.1.323 is here with experimental Code Mode, where Claude writes JavaScript to call its tools. Plugins now honor version ranges, and validate catches TypeScript hook issues. Plus a long li
- v2.1.322 (Nov 10, 2026) — Claude Code 2.1.322 brings fast mode to Vertex in global, us, and eu regions. If Vertex declines it, you automatically drop back to standard speed. Curious how much faster your sessions can run?
- v2.1.321 (Nov 9, 2026) — Claude Code 2.1.321 is here: Fable 5.2 is now the default, remote-control takes a settings file, and a batch of permission and security gaps got patched. Curious what else slipped through the cracks?
- v2.1.319 (Nov 6, 2026) — Claude Code 2.1.319 is out: MCP tasks now run in the background, auto mode behaves differently behind gateways, and a new setting can deny prompts instead of waiting. Want the details?

Upgrading from v2.1.319? See every change since.
