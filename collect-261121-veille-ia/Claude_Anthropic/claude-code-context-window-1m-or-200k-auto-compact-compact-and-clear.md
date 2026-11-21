---
id: collect-261121-veille-ia/claude-anthropic/claude-code-context-window-1m-or-200k-auto-compact-compact-and-clear
title: "Claude Code context window: 1M or 200K, auto-compact, /compact and /clear"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-21"]
keywords: ["window", "context", "claude", "code", "auto-compact", "compact", "model", "every"]
source: https://caprock.dev/guides/claude-code-context-window/
source_anchor: ""
source_lines: [1, 171]
section: "Claude_Anthropic"
---
# Claude Code context window: 1M or 200K, auto-compact, /compact and /clear

Claude Code's context window per model, how to see usage, when auto-compact fires and how to change or turn it off, /compact vs /clear, what context costs.

Claude Code's context window is 1M tokens on current models: Opus 4.7 and later, Sonnet 5 and later, Haiku 5.5 and the Fable models run with it on every plan, including Pro. Sonnet 4.5, Haiku 4.5 and other older models have 200K. When the conversation nears the limit, auto-compact replaces it with a summary; `/compact` does the same on demand, and `/clear` throws the conversation away. 

Caprock keeps the record of every coding agent you run. Free and open source. Install Caprock ↓

## Claude Code context window size: 1M or 200K

| Context window by model in Claude Code, from Anthropic's docs, read 21 October 2026. |  |  | 
|---|---|---|
| Model | Window | How you get 1M | 
|---|---|---|
| Opus 4.7 and later | 1M | default, every plan | 
| Sonnet 5 and later | 1M | default, every plan; no `[1m]` variant | 
| Fable 5.1, Fable 5 | 1M | default, every plan | 
| Haiku 5.5 | 1M | default, every plan | 
| Opus 4.6 | 200K or 1M | `claude-opus-4-6[1m]` : included on Max, Ultra, Team, Enterprise; usage credits on Pro | 
| Sonnet 4.6 | 200K or 1M | `claude-sonnet-4-6[1m]` : usage credits on every plan | 
| Sonnet 4.5, Haiku 4.5 | 200K | — | 

`[1m]`. Fable usage itself can bill to usage credits on some plans. On the Anthropic API the 1M window is billed at standard prices, with no premium past 200K, except on Haiku 5.5, whose prompts over 100K tokens cost more per token. `CLAUDE_CODE_DISABLE_1M_CONTEXT=1` holds every model to 200K and removes the 1M variants from `/model`. 

## Show context usage

- **/context** draws current usage as a coloured grid, by category, with suggestions for heavy tools and memory files. In fullscreen mode the per-item list is collapsed;`/context all` expands it.
- **The status line** gets`context_window.used_percentage` ,`remaining_percentage` ,`context_window_size` (200000 or 1000000) and`total_input_tokens` on every update. The percentage counts input tokens only and can be`null` early in a session. Setup is in the status line guide.

`used_percentage` always measures against the model's full window, not the auto-compact window. Set a 500K window on a 1M model and compaction comes before the line reads 50%. 

## What /compact and auto-compact do

Compaction replaces the conversation with a structured summary. Older tool output is cleared first, then the history is summarized. Your requests and key code survive; detailed instructions from early on may not.

After a compaction Claude Code reloads from disk the project-root `CLAUDE.md`, auto memory and the plan, re-reads up to five recently modified files, and re-injects invoked skills (up to 5,000 tokens each). Path-scoped rules and nested `CLAUDE.md` files are summarized away until a matching file is read again. 

**Auto-compact** is the same pass, run for you as the window fills. To steer what any summary keeps: 

1. 1 Compact now, with a focus
2. 2 Summarize part of the conversation Pick a message, then Summarize from here or up to here.

Or add a standing section to the project's `CLAUDE.md`: 

# Compact instructions
When you are using compact, please focus on test output and code changes

If one file or tool output refills the window right after each summary, Claude Code stops with `Autocompact is thrashing` instead of looping. Read the file in smaller ranges. 

## When auto-compact fires

With no setting, a model with a native 1M window compacts at about 967K tokens, by Anthropic's docs; Opus 4.6 and Sonnet 4.6 without `[1m]` compact at 200K. You can move that point, the *auto-compact window*, anywhere from 100K to 1M: 

1. 1 For the current model, saved to user settings /autocompact auto returns to the default.
2. 2 For one launch
3. 3 In scripts and cloud environments A token count; 500k also works from v2.1.304. Overrides the command, the flag and the setting, unless Claude Code ignores a value it can't read as a size.

For every model, put `"autoCompactWindow": 500000` in a settings file. `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` (1–100) compacts at a share of that window; it can lower the threshold, not raise it. 

Claude Code also compacts a conversation you leave idle: in a terminal session on the one-hour prompt cache, once the context passes 200,000 tokens (the default threshold) and about 54 minutes have gone by, it compacts shortly before the cache expires. `"idleCompaction": false`, or **Idle compaction** in `/config`, turns that off. 

On my machine (Claude Code, main thread only, models with a 1M window, 18 July to 5 October 2026, read from `caprock export`) I ran without a setting until 9 September, then added `"autoCompactWindow": 500000`: 

| Where auto-compact fired on one machine, before and after the setting. |  |  | 
|---|---|---|
|  | Before 9 September | From 9 September | 
|---|---|---|
| Compaction fired at | about 998K | 464K–468K | 
| Next call carried | e.g. 71,981 | 63K–75K | 
| Calls above 500K | 46.8% | 0.9% | 
| Largest context | 999,803 | 532K | 

## Turn off auto-compact

- **/config** →**Auto-compact** , which writes`"autoCompactEnabled": false` to your user settings.
- **One session:**`DISABLE_AUTO_COMPACT=1` , which also stops idle compaction. Whichever of the two turns it off, the other cannot turn it back on.
- **No compaction at all:**`DISABLE_COMPACT=1` also disables`/compact` .

With auto-compact off, a full window makes requests fail with `Context limit reached · /compact or /clear to continue`. If you turned it off in `/config`, the line adds `· auto-compact is off · /config to turn it on`. 

## /compact vs /clear

- **/compact** keeps the same conversation as a summary. It reads the whole context to write it, so on a large context it is itself a large request; cheap while the cache is warm, expensive after a break.
- **/clear** starts a new conversation with empty context and keeps project memory. It costs nothing.`/clear auth-work` names the old one so`/resume` finds it. Aliases:`/reset` ,`/new` .

Compact when the next step needs what came before. Clear when you switch to unrelated work: Anthropic's cost page says stale context "wastes tokens on every subsequent message".

## Why context size drives cost

Claude Code sends the whole conversation with every request, and each tool result sends it again. With prompt caching that history is read back at the cache-read rate, a tenth of fresh input or less, but it is read on every call.

How full my main threads ran:

- 426K
- median context of 45,919 main-thread calls
- 851K
- 90th percentile
- 65K
- median subagent call

Of 71 sessions with main-thread calls, 39 peaked above 200K and 19 above 500K. And what it cost, at list prices, across all $14,460 of Claude Code calls:

- 78.1%
- cache reads: re-reading context
- 16.9%
- cache writes
- 5.0%
- output

Fresh input rounded to 0.0%. A smaller auto-compact window caps what each call re-reads. That 78.1% covers 18 July to 5 October overall, in line with the 78–83% per month in what Claude Code costs a month; which tool calls the turns made is in where the money goes.

## What Caprock adds

`/context` and the status line show this session, now. Caprock keeps the record across sessions: 

- **Status line:**`caprock statusline` shows`ctx 41%` from Claude Code's`used_percentage` , beside cost, turns and plan windows.
- **Price per call:** each session card's Context stat shows the fill against the model's full window, the token count and a price such as`$0.07/call` : what the next call costs at that fill, priced as a cache read.
- **Compactions and clears:** the session timeline marks each`context compaction (auto)` or`(manual)` and each`context cleared (/clear)` . It recorded 28 compactions and 3 clears on my machine in this period.
- **One conversation after a clear:**`/clear` gives the session a new ID, and Caprock links it to the one before, so the conversation stays one card with its earlier parts a click away. In a Caprock tab the chat shows the earlier part above a*Context cleared — Claude no longer remembers the part above* line. That record is for you: Claude gets no summary of what came before.
- **Context tax:** the All time panel on Caprock's Home screen (its live view) has a Context tax row with the dollars spent re-sending the conversation and their share of the total.
- **Your own numbers:**`caprock export` writes every call's`cache_read` and`cache_write` and the`context.compact` and`context.clear` events, which is how the figures above were read.

```
$ caprock export --agent claude --since 90d --out events.tsv
```
Caprock records every session on your machine, whether or not it started it, and keeps that record past Claude Code's cleanup. Free, open source; nothing leaves your machine. Install Caprock.

## Questions people ask

- What is auto-compact in Claude Code?
- When the conversation nears the auto-compact window, Claude Code clears older tool output and replaces the history with a summary so the session can continue. It is the same pass as /compact, run automatically.
- How big is Claude Code's context window?
- 1M tokens on Opus 4.7 and later, Sonnet 5 and later, Haiku 5.5 and the Fable models, on every plan. Opus 4.6 and Sonnet 4.6 reach 1M as claude-opus-4-6[1m] and claude-sonnet-4-6[1m]; Sonnet 4.5 and Haiku 4.5 have 200K.
- When does Claude Code auto-compact?
- On a model with a native 1M window, at about 967K tokens by default, per Anthropic's docs. /autocompact, --autocompact, the autoCompactWindow setting or CLAUDE_CODE_AUTO_COMPACT_WINDOW move it anywhere from 100K to 1M.
- How do I disable auto-compact in Claude Code?
- Turn off Auto-compact in /config, set "autoCompactEnabled": false in settings, or set DISABLE_AUTO_COMPACT=1. /compact still works; DISABLE_COMPACT=1 turns off both.
- What is the difference between /compact and /clear?
- /compact keeps the conversation as a summary and reads the whole context to write it. /clear starts a new conversation with empty context, keeps project memory and costs nothing.

## Sources

Try it

The record of every coding agent you run, the sessions it did not start included. Free, open source; nothing leaves your machine.

Windows 10 22H2 or later, x64. Windows may ask once whether to run it: click More info, then Run anyway.

Paste it into Claude Code, Codex or any coding agent.

## See what it says ▾

Install the Caprock desktop app for me. It is open source (Apache-2.0), a local
app for my coding agents: https://github.com/dspv/caprock
What the install does and how to remove it:
https://github.com/dspv/caprock/blob/master/docs/install-agent.md
Run the install script for my system: https://caprock.dev/mac.sh, /win.ps1 or
/linux.sh. Read it before you run it if you like. Leave my Claude Code settings
alone; the app asks me itself. Tell me when it's open.

Or run it yourself:

Paste in Terminal: it installs Caprock and opens it.Or with Homebrew: `brew install --cask dspv/tap/caprock-app` 

Or run it yourself:

Paste in PowerShell: it installs Caprock for you and opens it.

Or run it yourself:

Paste in a terminal: it installs Caprock for you, no sudo. Or download the AppImage
