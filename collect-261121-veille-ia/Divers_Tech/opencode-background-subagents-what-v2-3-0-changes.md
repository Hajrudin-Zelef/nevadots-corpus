---
id: collect-261121-veille-ia/divers-tech/opencode-background-subagents-what-v2-3-0-changes
title: "OpenCode Background Subagents: What v2.3.0 Changes"
domain: divers-tech
role: reference
task: article
actors: []
dates: ["2026-11-13"]
keywords: ["background", "subagents", "opencode", "verbosity", "session", "release", "parent", "shell"]
source: https://www.developersdigest.tech/blog/opencode-2-3-background-subagents-quieter-transcripts-2026
source_anchor: ""
source_lines: [1, 98]
section: "Divers_Tech"
---
TL;DR

OpenCode v2.3.0 makes every subagent run in the background and defaults transcripts to low verbosity. What changed, what to set, and where it bites.

**Last updated:** November 13, 2026

OpenCode v2.3.0 shipped on November 13 with one change that alters how you drive the agent: subagents now run in the background by default. If you search for how OpenCode background subagents behave today, this is the release that defines it. The parent gets a session ID back immediately, you keep typing, and the child reports when it goes idle.

## What the release says#

The v2.3.0 release notes list three headline items:

- **Background subagents by default.** Subagents return immediately, run in the background and report back when idle. Several can run while you work in the parent session.
- **Quieter transcripts.** Transcript verbosity defaults to`low` , which folds tool calls and thoughts into one collapsible activity row. Set Verbosity to`medium` or`high` , or set`session.verbosity` in the TUI config, to see every step. An existing`session.verbosity` is kept.
- **Jump to message** in the desktop app, on`Cmd+Alt+G` (`Ctrl+Alt+G` on Windows and Linux).

The details that matter for agent workflows sit under Core:

- A subagent's report now says when its own background work is still running, so a parent is not told "done" while a child shell is alive.
- Experimental `mcp_events` flag for MCP events, and an experimental`shell_timeout_background` flag that moves a shell command to the background when its timeout expires.
- Shell commands survive server handoffs and keep their output when interrupted.
- Provider mutual TLS through `settings.tls` with`cert` ,`key` and an optional`passphrase` , which accept`{file:}` and`{env:}` like`apiKey` .
- Tool output truncation keeps the start of the output and a usable preview if the full output cannot be saved.

This did not appear from nowhere. The release page history shows the groundwork: v2.2.1 let desktop users view and stop background shells from the activity pill, and v2.2.6 made `opencode run` wait for the background shell commands, subagents and image jobs its prompt started, with no time limit and SIGTERM cancelling the session and its background jobs. v2.3.0 continues that direction: the notes say subagents now return immediately and run in the background, which I read as background being the default path for them.

## The angle: the cost of "just run it in the background"#

Making background the only mode is a bet that the wait was the problem. It mostly was. A foreground subagent blocks the one thing you are good at, which is reading the diff the parent is building. The trade is that three things get harder.

**Review and undo.** A background child can change files while you edit the same files in the parent. The third-party opencode-background-agents plugin, now read-only and pointing users to V2's native version, documents the same hazard from the plugin side: its background sessions ran outside the session tree, so undo could not track their changes, and it limited delegation to read-only subagents for that reason. The native feature is a different implementation, so this is a pattern to test, not a claim about v2.3.0. Until you have tested it, keep write-capable subagents on separate files or a separate worktree - see Git worktrees for parallel agents and merge discipline for parallel coding agents.

**Cost.** Parallel children multiply token spend. Nothing in the release notes limits fan-out, so the guard is yours. Ask for a bounded number of children in the prompt, and pick a cheaper model for them where your provider config allows it. The cost shape is the same one covered in what parallel Claude agents actually cost.

**Silence.** Low verbosity plus background children means less on screen. That is pleasant until something goes wrong inside a collapsed row. The fix is one line, and I would set it before the first run on an unfamiliar repo:

```
{
  "session": { "verbosity": "medium" }
}
```
The release notes name `session.verbosity` in the TUI config but I did not verify the exact file it lives in, so check where your existing setting is and edit it in place rather than copying this block blindly.

## What to do Monday#

1. **Upgrade and read your permissions.** If you migrated from V1, the`task` permission was renamed`subagent` in V2, as covered in the OpenCode 2 migration guide. Confirm your rules still gate who can spawn children.
2. **Raise verbosity for one session.** Run a real task at`medium` , watch what the children do, then decide whether`low` is safe for you.
3. **Give each child its own files.** Name the directories each subagent may edit in the prompt. Read-only research children are the safe default.
4. **Keep headless runs bounded.** Because`opencode run` waits for the background work it started with no time limit, a long-running dev server started by a child keeps your CI job open until interrupted. Put a timeout around the job in your pipeline, as in the OpenCode cron guide.
5. **Leave the experimental flags off in production.**`mcp_events` and`shell_timeout_background` are flagged experimental in the notes.

If you also run local models, the same release changes discovery: local model discovery for Ollama, LM Studio and vLLM now waits 15 seconds per request instead of 1, and LM Studio models with a null context length are fixed. That helps slow-starting local servers; it also means a dead endpoint takes longer to fail.

## What people are actually saying#

I could not read Hacker News or Reddit from this run, so there is no fresh v2.3.0 thread to quote and I will not invent one. The closest verified signal is the one already covered in our OpenCode 2 migration post: a Hacker News commenter said V2 handles asynchronous subagents and background shells better than V1, while another said the v1/v2 split made them hesitate to switch. The counter-case is the plugin author's own caveat above: background work you cannot undo is a real cost, not a hypothetical one.

Treat the "agents now fan out by themselves" narrative with some caution. Release notes describe capability, not outcomes; the useful measurement is whether your review time dropped and your diff conflicts did not rise.

## Continue Reading#

- OpenCode 2 upgrade: migrate V1 config and plugins - the V1 to V2 migration this release builds on
- OpenCode cache warming in v2.2.7 - the previous release and its cost trade
- Claude Code subagents vs agent teams vs workflows - the same delegation question in Claude Code
- Parallel coding agents: merge discipline - keeping simultaneous agents from colliding
- OpenCode developer guide - setup and config reference
- OpenCode tool page - what OpenCode is, pricing and setup links
- Best AI Coding Tools (2026) - Claude Code, Codex, Cursor and more, tested and ranked

## Sources#

- OpenCode v2.3.0 release notes - primary
- OpenCode releases page, v2.2.1 to v2.2.7 - primary
- opencode-background-agents plugin README - third-party plugin, retired for V1
- Hacker News comment - community, via our earlier post

## Get the next deep dive like this in your inbox

One email a week on OpenCode and the rest of the AI dev stack. Free.

Read next on Claude Code

### OpenCode 2: How to Upgrade From v1 Without Breaking Your Setup

OpenCode 2 upgrade from v1: what the migration guide says breaks (plugins, server API, TUI config), what carries over, and a safe checklist before you switch.

7 min read
### OpenCode Cache Warming: What v2.2.7 Changed and When to Turn It On

OpenCode v2.2.7 makes cache warming time its pings from the model cache TTL. The config, the cost of idle pings, and when warming is a bad trade.

7 min read
### Subagents vs Agent Teams vs Workflows: Claude Code's Parallelism Primitives, Compared

Claude Code subagents vs agent teams vs workflows: who holds the plan, the hard limits from the docs, and which parallelism primitive fits your task.

9 min read
Technical content at the intersection of AI and development. Building with AI agents, Claude Code, and modern dev tools - then showing you exactly how it works.
