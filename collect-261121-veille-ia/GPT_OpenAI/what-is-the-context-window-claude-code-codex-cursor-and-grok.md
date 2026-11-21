---
id: collect-261121-veille-ia/gpt-openai/what-is-the-context-window-claude-code-codex-cursor-and-grok
title: "What is the context window? Claude Code, Codex, Cursor and Grok"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["claude", "context", "window", "code", "compact", "codex", "model", "prompt"]
source: https://flaviocopes.pages.dev/ai-glossary/context-window/
source_anchor: ""
source_lines: [1, 109]
section: "GPT_OpenAI"
---
The **context window** is everything a model can look at in one request. For a coding agent, that’s the system prompt, your instruction files, the conversation so far, and every file it read and command output it saw, all counted in tokens up to a fixed limit.

## How the context window works

A model doesn’t remember your session between requests. Each time the agent calls it, the agent sends the conversation again. The Claude Code docs say it directly: Claude Code sends your full conversation with every request.

So the context grows as you work. Some of it loads before you type anything: the agent’s system prompt, files like `AGENTS.md` or `CLAUDE.md`, the descriptions of your skills, and the tools your MCP servers expose. The rest piles up during the session, one message, file read and test run at a time.

When the window gets close to full, the agent compacts the conversation. It replaces older turns with a summary and keeps going. The summary keeps the main points but loses detail, which is why a long session can forget something you said early on.

Window sizes depend on the model and change with every release. In October 2026, recent Claude models in Claude Code run with a 1M-token window by default, and GPT-6.1 Sol uses a 500,000-token window by default in Codex. A bigger window gives you room, but the agent still sends all of it with each request, so a long session gets more expensive per turn. My post on prompt caching, compression and compaction explains how caching softens that cost.

## The context window in each tool

All four tools let you check usage, compact by hand and start fresh. They differ in how you see the numbers and when auto-compaction kicks in.

|  | Claude Code | Codex | Cursor | Grok Build | 
|---|---|---|---|---|
| Check usage | `/context` | `/status` | The context ring next to the prompt | `/context` | 
| Compact by hand | `/compact` | `/compact` | `/summarize` in the Cursor CLI | `/compact` | 
| Start fresh | `/clear` | `/clear` or`/new` | `/clear` in the Cursor CLI | `/new` | 
| Auto-compaction | Before the window fills, adjustable with `/autocompact` | Set with `model_auto_compact_token_limit` | When the window gets close to full | At 85% by default | 

### Claude Code

At session start, Claude Code loads your `CLAUDE.md` files, its auto memory, MCP tool names and skill descriptions. An `AGENTS.md` loads instead only when you have no `CLAUDE.md`. Full skill content loads only when a skill is used, and MCP tool definitions load on demand.

`/context` draws your current usage as a colored grid, with suggestions for what takes too much space. `/compact` summarizes the conversation, and you can tell it what to keep, like `/compact keep the API changes and the test commands`. `/clear` starts over with an empty context, and `/btw` asks a quick question without adding it to the history.

After compaction, Claude Code re-reads your project’s `CLAUDE.md` from disk, so instructions kept there survive. Details you only typed early in the chat may not. The docs recommend keeping each `CLAUDE.md` under 200 lines, because longer files cost more context and get followed less reliably.

To change when auto-compaction happens, run `/autocompact` with a value, like `/autocompact 500k`. See the context window in the Claude Code docs.

### OpenAI Codex

Codex reads your `AGENTS.md` files before doing any work, concatenating them from the repository root down. It stops adding files once they reach `project_doc_max_bytes`, which is 32 KiB by default.

`/status` shows the active model, your approval policy and how much context is left. `/compact` replaces earlier turns with a short summary. `/clear` resets the visible chat and its context, and `/new` starts a fresh chat without leaving the CLI.

In `config.toml`, `model_auto_compact_token_limit` sets the token count that triggers automatic compaction, and `compact_prompt` replaces the prompt Codex uses to write the summary. See the Codex slash commands and the AGENTS.md docs.

### Cursor

In Cursor, every chat shares a fixed context window with the model. The ring next to your prompt shows how full it is. Click it to see the tokens split by category: system prompt, tools, rules, skills, MCP, subagents, summarized conversation and the conversation itself.

When the window gets close to full, Cursor compresses older parts of the conversation into a summary. In the Cursor CLI, `/summarize` does it by hand.

Rules marked Always Apply load into every chat, so keep them short. The docs suggest under 500 lines per rule. Files matched by `.gitignore` or a `.cursorignore` file stay out of the agent’s context. See Cursor’s context docs.

### Grok Build

Grok Build loads your project rules into context for every session: `AGENTS.md`, `CLAUDE.md`, files in `.grok/rules/` and a few other names. Files are loaded in full, with no size cap. `grok inspect` lists each rules file with its approximate token count.

`/context` shows current usage. `/compact` compresses the conversation and takes optional instructions about what to keep. Grok also compacts on its own when usage reaches `auto_compact_threshold_percent` in the `[session]` settings, 85% by default. `/new` starts a new session.

The status line can show context usage as a percentage, but it’s off by default. See sessions in the Grok Build docs.

Details checked against each vendor’s documentation in October 2026.

## A real example

You ask Claude Code to fix a failing test. To find the cause, it reads a dozen files and runs the test suite four times, each run printing a long stack trace. The fix works, but your context is now full of output you don’t need.

First, look at what’s taking the space:

`/context`
Then compact, telling Claude what to keep:

`/compact keep the failing test name, the root cause and the files we changed`
If the next task has nothing to do with the test, skip the summary and start clean with `/clear`. The Claude Code docs recommend it between unrelated tasks, because it costs nothing and the next task stops paying for the previous one’s history.

The same session in Grok Build works the same way, with `/context` and `/compact` plus your instructions. In Codex, check with `/status`, then `/compact`. In Cursor, watch the ring, and start a new chat when the task changes.

If the root cause taught you something about the codebase, also write it down in `AGENTS.md` or `CLAUDE.md`, or in a decisions file. Files on disk load again in every new session, while a chat summary stays in that one chat.

## When the context window matters

It matters most in long sessions, where early instructions get compacted away, and in big codebases, where exploring reads a lot of files. It also matters when you connect many MCP servers or install many skills, because their descriptions load before your first message.

To keep it small, start a new chat for each unrelated task. Send research that reads a lot to a subagent, so only its summary lands in your context. Use plan mode for big changes, then implement from the plan. Keep instruction files short too, and move details into files the agent reads only when it needs them.

## Pitfalls

Compaction loses detail. In Claude Code, the summary keeps your requests and key code snippets, but the docs warn that detailed instructions from early in the conversation may be lost. Put anything that must survive in an instruction file.

Instruction files cost context in every session. Codex stops reading `AGENTS.md` files past 32 KiB by default, so the last part of a long file may never reach the model. Grok Build has no cap, and its docs note that short, specific instructions are followed more reliably than long ones.

Skills and MCP servers aren’t free. Their names and descriptions load at session start, even the ones you never use in that session.

Cursor’s ignore files keep files away from the agent, but the docs warn that terminal commands and MCP tools run outside those controls and may still read ignored files.

Parallel subagents don’t share one window. Each has its own context, which keeps yours clean, but every one of them uses tokens.

## Related on flaviocopes.com

- Prompt caching, compression, and compaction in coding agents explains what compaction does to your costs and cache hits.
- What is an AGENTS.md file shows how to write the instruction file that loads at the start of every session.
- How to make AI harnesses talk less cuts the agent’s own output, which also fills the window.
- AI coding agent context: how to document project decisions keeps decisions in files instead of in a chat that gets compacted.
- Skill Cabinet is my Mac app that lists the skills each of your agents loads and flags the ones no chat used. Every installed skill adds its description at session start, so removing unused ones trims your context.

## Related glossary entries

- Subagents: A subagent is a helper your coding agent starts with its own context window. How subagents work in Claude Code, Codex, Cursor and Grok Build.
- Plan mode: Plan mode makes a coding agent research and propose a plan before it edits code. How it works in Claude Code, Codex, Cursor and Grok Build, and when to use it.
- Hooks: Hooks run your own scripts when a coding agent calls a tool or stops. How hooks work in Claude Code, Codex, Cursor and Grok Build, with examples.
- Status line: A status line is the row under a coding session that shows the model, context, or a script you wrote. How Claude Code, Codex, Cursor, and Grok Build set it.

Want me to talk about your product? You can sponsor this site.
