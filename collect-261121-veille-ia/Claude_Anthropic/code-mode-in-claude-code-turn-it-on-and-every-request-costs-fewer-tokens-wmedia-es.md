---
id: collect-261121-veille-ia/claude-anthropic/code-mode-in-claude-code-turn-it-on-and-every-request-costs-fewer-tokens-wmedia-es
title: "Code Mode in Claude Code: turn it on and every request costs fewer tokens | wmedia.es"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-20"]
keywords: ["code", "mode", "claude", "tokens", "request", "turn", "script", "every"]
source: https://wmedia.es/en/tips/claude-code-code-mode-turn-on
source_anchor: ""
source_lines: [1, 106]
section: "Claude_Anthropic"
---
**TL;DR** Start with `CLAUDE_CODE_EXPERIMENTAL_CODE_MODE=1 claude`, accept the notice and check that `/status` says `Code Mode: on`. On v2.1.323, with the same setup, every request went out with about **14,000 fewer tokens** of context. The trade: that session can only be resumed with Code Mode on, and it loses Remote Control, Chrome and the background.


**Update, November 20, 2026:** since v2.1.329, Code Mode starts even with telemetry, nonessential traffic or feature flags turned off, and also with OpenTelemetry events and metrics on; only tracing keeps it out. Tested on v2.1.329 with `DISABLE_TELEMETRY`, `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` and `CLAUDE_CODE_ENABLE_TELEMETRY` all set: `/status` reads `Code Mode: on`. The table at the end already reflects it.


**Update, November 18, 2026:** since v2.1.327, Code Mode no longer refuses to start when you have a plugin installed whose hooks check tool calls; only your organization's settings can keep it off. The table of what you lose now says so.


**Update, November 17, 2026:** since v2.1.326, Code Mode also turns on with agent teams, and the reasons it gives when it won't turn on say what to do. The table at the end has the new texts.


Code Mode is the experimental mode that shipped in v2.1.323 on November 12: **Claude stops calling `Read`, `Bash` or `Edit` one at a time and writes JavaScript that calls them**. The script runs locally, and what comes back into the conversation is what it returns or logs, not everything it read along the way. It ships off: either you turn it on, or it doesn't exist for you.

## What changes on a real task

The same request in two Opus 5.5 sessions at `medium`, identical except for Code Mode: an `orders/` folder with 40 daily JSON files, and the question of which three customers spent the most on paid orders. With Code Mode on, this is the script Claude used to get the totals:

```
const t = {};
for (const f of files) for (const o of JSON.parse(await file('orders/' + f).text()))
  if (o.status === 'paid') t[o.customer] = (t[o.customer] || 0) + o.amount;
Object.entries(t).map(([c, v]) => [c, Math.round(v * 100) / 100]).sort((a, b) => b[1] - a[1])
```
It read all 40 files inside the script, and the only thing that came back into the conversation was the list of eight customers with their totals. `files` came from an earlier script: **variables survive from one script to the next**.

|  | Without Code Mode | With Code Mode | 
|---|---|---|
| Requests to the model | 4 | 4 | 
| Input tokens in the first one | 36,404 | **22,233** | 
| Input tokens in total, cache included | 150,364 | **94,659** | 
| Output tokens | 1,172 | 1,213 | 
| Time | 15 s | 37 s | 
| Answer | initech, umbrella, acme | initech, umbrella, acme | 

**The saving didn't come from fewer turns**: without Code Mode, Claude wrote a script too, in Python through `Bash`. It came from the fixed context. In Code Mode the model gets two tools, `REPL` (runs the JavaScript) and `Poll` (collects results), and the rest reach it as JavaScript functions described inside `REPL`, in a shorter form. That difference rides along on every request: the more requests a task makes, the more tokens it adds up to, though after the first one they are mostly cache reads, which cost less. One run each is not a benchmark, and on this one Code Mode was slower.

You can check it in your own session with `/context` after the first reply (before it, the Code Mode figure is an underestimate):

```
Without Code Mode   ⛁ System tools: 24.6k tokens (2.5%)
With Code Mode      ⛁ System tools: 11.1k tokens (1.1%)
```
## How to turn it on

**1. Start with the variable**

```
CLAUDE_CODE_EXPERIMENTAL_CODE_MODE=1 claude
```
To keep it on, put it in the `env` block of `~/.claude/settings.json`. The project's `.claude/settings.json` won't do: Claude Code ignores it there without a word. Use `1` as the value.

**2. Accept the notice**

The first time, a dialog titled `Code Mode (experimental)` opens with the focus on `No, start without it`. Move down to `Yes, turn on Code Mode` and press Enter. What it warns you about, in short: it's an experiment that can change or disappear in any release, a session you start in Code Mode can only be resumed with Code Mode on (cloud sessions and the Claude apps can't continue it), and in auto mode the safety check reads each script once before it runs, not each tool the script then calls. If you work in auto mode, that's the change that matters. Say yes and it's saved, so it won't ask again; say no and it asks on every launch while the variable is set.

**3. Check that it's on**

This line shows at startup:

```
⚠ Code Mode (experimental) is on · this session can be resumed only with Code Mode on
```
And `/status` says so, with where it came from:

```
Code Mode:              on (experimental) · CLAUDE_CODE_EXPERIMENTAL_CODE_MODE in the shell environment
```
Inside the session, the screen summarizes the work as usual (`Read 40 files, listed 1 directory`). Press `ctrl+o` to see each script as a `Code(...)` entry with the tools it called underneath.

**4. To turn it off**, remove the variable from wherever you set it, or start with `CLAUDE_CODE_EXPERIMENTAL_CODE_MODE=0` in the shell, which wins over `settings.json`.

## What you give up while it's on

| Situation | What happens | 
|---|---|
| Resuming the session without Code Mode | It won't open: it gives you the command with the variable instead | 
| `claude -p` , claude.ai cloud sessions | `isn't available: for now it only works in the interactive terminal` , and it starts as usual | 
| Remote Control | The session doesn't connect: quit and start again with the variable set to `0` . If you launch with Remote Control already on, Code Mode is the one that stays off | 
| Claude in Chrome and computer use | Off for that session | 
| Sending it to the background, to Claude Desktop or to the cloud | It can't be moved | 
| A plugin with JavaScript hooks that check tool calls | Since v2.1.327 Code Mode starts anyway. If a hook changes one of the script's calls, auto mode doesn't run that code | 

This is what you get when you resume a Code Mode session without the variable:

```
This session was started in Code Mode (experimental), which is off, so it can't be resumed.
To resume it, run:
  CLAUDE_CODE_EXPERIMENTAL_CODE_MODE=1 claude --resume 4a4353fb-1b39-4123-bfa0-25ea4278401e
```
## When it won't turn on

At startup you get `Code Mode (experimental) isn't available:` plus the reason, and the session starts with the usual tools. The ones you're likely to hit:

| Reason on screen | What to do | 
|---|---|
| `for now it only works with Opus 5.5, Sonnet 5.5 and later models` | Start again with one of those models, for example `claude --model opus` | 
| `for now it only works directly with the Anthropic API` | No Code Mode through Bedrock, Vertex, Foundry, a gateway or another address in `ANTHROPIC_BASE_URL` | 
| `for now it doesn't work with an API key helper, ANTHROPIC_AUTH_TOKEN or custom auth headers` | Start without them | 
| `OpenTelemetry tracing is on` | Drop `CLAUDE_CODE_ENHANCED_TELEMETRY_BETA` (or`ENABLE_ENHANCED_TELEMETRY_BETA` ) and`ENABLE_BETA_TRACING_DETAILED` , from your settings'`env` too; if your organization sets them, it's out of your hands. OpenTelemetry events and metrics from`CLAUDE_CODE_ENABLE_TELEMETRY` no longer turn it off | 
| `it isn't open to your account in this version of Claude Code, or Claude Code couldn't check whether it is` | Update Claude Code or start it again; if your account doesn't have it yet, wait | 

Official docs: Changelog, v2.1.323. The page it links to, `code-mode`, still answered 404 on November 13.


Tested with Claude Code v2.1.323 on Linux, on November 13, 2026.
