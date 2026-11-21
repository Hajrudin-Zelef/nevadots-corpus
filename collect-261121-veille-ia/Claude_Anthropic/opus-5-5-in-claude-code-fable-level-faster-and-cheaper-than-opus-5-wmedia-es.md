---
id: collect-261121-veille-ia/claude-anthropic/opus-5-5-in-claude-code-fable-level-faster-and-cheaper-than-opus-5-wmedia-es
title: "Opus 5.5 in Claude Code: Fable-level, faster, and cheaper than Opus 5 | wmedia.es"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["opus", "claude", "faster", "code", "model", "fable", "less", "sonnet"]
source: https://wmedia.es/en/tips/claude-code-opus-5-5-not-showing-up
source_anchor: ""
source_lines: [1, 142]
section: "Claude_Anthropic"
---
**TL;DR** Opus 5.5 beats Fable 5.1 on all nine benchmarks Anthropic published (66.4% against 55.8% on Terminal-Bench 4.0), generates output over 30% faster than Opus 5, and costs $4/$20 per MTok against $5/$25. It's already the Default on every plan, Pro included. Two conditions to get all of it: it needs **Claude Code v2.1.280**, and it starts at **`medium`** even if your `settings.json` says `high`. One `claude-opus-5-5` entry under `modelSettings` fixes that.


Opus 5.5 shipped today, September 22, 2026, the first model in the Claude 5.5 family. Anthropic's one-line pitch: it performs at Fable 5.1's level on most work and costs 40% less than Opus 5. In Claude Code it becomes the default model.

## What it brings

### Fable-level coding

| Benchmark | Opus 5.5 | Fable 5.1 | Opus 5 | 
|---|---|---|---|
| Terminal-Bench 4.0 | **66.4%** | 55.8% | 52.3% | 
| CursorBench 4.0 | **57.8%** | 51.8% | 46.6% | 
| FrontierCode v1.1 | **54.4%** | 50.3% | 48.0% | 
| OSWorld 2.0 (computer use) | **81.8%** | 80.7% | 74.0% | 

These are Anthropic's numbers, not mine. In their table it beats Fable 5.1 on all nine benchmarks, and on the three agentic coding ones by a clear margin.

### Faster

It generates output more than 30% faster than Opus 5. And it needs fewer steps to get there: the launch note cites more terminal tasks solved than Opus 5 in less than half the steps. Fewer steps means less time waiting and fewer output tokens.

### Cheaper than Opus 5

| Per MTok | Opus 5.5 | Opus 5 | Fable 5.1 | 
|---|---|---|---|
| Input | **$4** | $5 | $10 | 
| Output | **$20** | $25 | $50 | 
| Cache write (1 h) | **$8** | $10 | $20 | 
| Cache read | **$0.20** | $0.50 | $0.25 | 

Per token it's 20% less than Opus 5, but the row that decides is the last one: cache reads are the line that weighs most in a long session (I measured it on Fable 5.1), and there Opus 5.5 is the cheapest of the three. With the same tokens, my three longest sessions this week would have cost 44% to 46% less than on Opus 5. And on tasks it spends fewer tokens: given the same four tasks it finished in half the turns Opus 5 needed, and rate and turns together left the bill at a third.

On a subscription you never see the per-token price. What you notice is the speed and each task using less. Anthropic is also raising the five-hour limits on Pro, Max, Team, and seat-based Enterprise.

My first impression, after half an hour and with all the caution that deserves: it answers faster and sounds more natural, which Anthropic highlights too (it leads with what matters and follows the style rules you give it).

## It's your Default now, Pro included

With Opus 5, the Default on Pro and Team Standard stayed on Sonnet 5. Version 2.1.280 changes that: `default` now resolves to Opus 5.5 on Pro, Max, Team, Enterprise, and the API.

If you're on Pro and preferred Sonnet to make your five-hour limit last, you now have to ask for it: `/model sonnet` in the session, or `"model": "sonnet"` in your `settings.json`. Since v2.1.284 that `sonnet` is Sonnet 5.5, and on well-scoped tasks it does the same work as Opus 5.5 at half the cost.

## Before you rely on it, two checks

Two things decide whether you're really running it, and both fail silently.

### 1. If it isn't showing up, it's your version

Opus 5.5 **requires Claude Code v2.1.280**. Earlier today I was on 2.1.278, and the `opus` alias kept answering with Opus 5 without a word:

```
claude --model opus -p "Reply with just: ok" --output-format json
# "modelUsage": { "claude-opus-5": { ... } }
```
Ask for it by full name and at least you get told:

```
$ claude --model claude-opus-5-5 -p "Reply with just: ok"
API Error: 400 Claude Code 2.1.278 does not support this model; version 2.1.280 or newer is required. Run 'claude update', or update the Claude desktop app, then try again.
```
The fix is `claude update`. Afterwards the same command returns `claude-opus-5-5`. Two setups where that isn't enough today:

- **Homebrew.** The`claude-code` cask tracks the stable channel and sits at 2.1.267;`claude-code@latest` sits at 2.1.278. Neither has reached 2.1.280 yet, and Homebrew doesn't auto-update. If that's how you installed, you wait for the cask to move or switch to the native installer.
- **Stable channel.** With`"autoUpdatesChannel": "stable"` you get releases roughly a week late. That channel is on 2.1.267 today.

The Claude desktop app was already running 2.1.280 on my Mac.

### 2. It starts at medium, and your global effort doesn't reach it

Opus 5 defaults to `high` effort. Opus 5.5 defaults to `medium`.

That wouldn't matter if your own setting won, but it doesn't. The top-level `effortLevel` key in `settings.json` is the old form, the one `/effort` wrote before Claude Code started saving a level per model. It still applies to Opus 5, Fable 5.1, and older models. Not to Opus 5.5, and not to anything released after it. This only affects the key in your user `settings.json`: a top-level `effortLevel` in project, local, or managed settings still applies to every model.

Part of my `settings.json`:

```
{
  "effortLevel": "high",
  "modelSettings": {
    "claude-opus-5": { "effortLevel": "high" },
    "claude-fable-5-1": { "effortLevel": "high" }
  }
}
```
To read the real level I use the six-line skill from the effort chain, saved as `.claude/skills/whicheffort/SKILL.md`:

```
---
name: whicheffort
description: Report the active effort level
---
Reply with exactly this line and nothing else:
EFFORT=${CLAUDE_EFFORT}
```
On 2.1.280:

```
$ claude -p "/whicheffort"
EFFORT=medium
$ claude --model claude-opus-5 -p "/whicheffort"
EFFORT=high
```
The first one is the Default, which means Opus 5.5. No error, no warning. My `high` still works on Opus 5 and simply doesn't exist on Opus 5.5.

The fix is one more entry under `modelSettings`:

```
"claude-opus-5-5": { "effortLevel": "high" }
```
With it, the same probe returns `EFFORT=high` for `default`, for `opus`, and for the full name. You can also pick the level with `/effort` while on Opus 5.5, or from the `/model` picker: per the docs, that sets its level the same way the manual entry does.

Do you need to raise it? Not necessarily. Anthropic set it to `medium`, and at the same level Opus 5.5 thinks more per turn than Opus 5, especially at `xhigh` and `max`. Its `medium` isn't Opus 5's `medium`. And on repo work, `high` and `xhigh` matched `medium` and cost more. What matters is that it's your call: if you set `high` for a reason, that reason no longer reaches it.

## What changes without you touching anything

- **Thinking can't be turned off.**`alwaysThinkingEnabled` and`MAX_THINKING_TOKENS=0` do nothing on Opus 5.5; effort is the only control left. The other knobs that aren't wired up anymore are in Claude Code thinking.
- **1M context on every plan** , Pro included, with no`[1m]` suffix and no usage credits.
- **`/fast` moves to Opus 5.5** , now the fast mode default, at $8/$40 per MTok (it was $10/$50 on Opus 5).

## Reference

| Opus 5.5 | Detail | 
|---|---|
| Minimum version | Claude Code v2.1.280 | 
| `opus` and`default` | Opus 5.5 on Pro, Max, Team, Enterprise, and API | 
| Price | $4 / $20 per MTok (Opus 5: $5 / $25) | 
| Cache read | $0.20 per MTok (Opus 5: $0.50) | 
| Fast mode | $8 / $40 per MTok (Opus 5: $10 / $50) | 
| Default effort | `medium` (Opus 5:`high` ) | 
| Global `effortLevel` (user settings) | Ignored: use `modelSettings` | 
| Thinking | Always on | 

Official docs: Model configuration · Introducing Claude Opus 5.5 · What's new in Claude Opus 5.5


Coming from Opus 5? What its launch brought, and why effort did carry over back then, is in Opus 5 in Claude Code. And if you're never sure which model to reach for, my actual routing is in Sonnet 5, Opus 5 and Fable 5.


## Requirements

- Claude Code **v2.1.280 or later** (`claude update` ). Every output above comes from that version or from 2.1.278. The Pro Default change comes from the 2.1.280 changelog; I haven't checked it on a Pro account.
