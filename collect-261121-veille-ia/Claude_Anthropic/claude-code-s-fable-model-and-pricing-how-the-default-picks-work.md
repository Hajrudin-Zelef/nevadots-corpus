---
id: collect-261121-veille-ia/claude-anthropic/claude-code-s-fable-model-and-pricing-how-the-default-picks-work
title: "Claude Code's Fable Model and Pricing: How the Default Picks Work"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "claude", "code", "model", "default", "pricing", "november", "fast"]
source: https://www.pondero.ai/coding/guides/claude-code-default-model-pricing-guide/
source_anchor: ""
source_lines: [1, 74]
section: "Claude_Anthropic"
---
## Table of Contents

# Claude Code's Fable Model and Pricing: How the Default Picks Work

If you had Fable 5.1 saved as your model in Claude Code, it quietly became Fable 5.2 sometime after November 9, 2026, and the per-token price dropped. Not rose. Dropped, from $10/$50 per million tokens to $8/$40, per the Claude Code changelog. That is the opposite of how most model upgrades land, and it is worth stopping on before you decide whether to keep the new pick or roll it back.

Claude Code's own docs are blunt about scope here, so let's be precise too: "No Fable model is the account-type default on any plan or provider," per Claude Code's model configuration docs. Fable isn't replacing Sonnet or Opus as what a fresh install opens with. What changed in version 2.1.321 (shipped November 9, 2026) is narrower and still worth your attention: the `fable` alias now resolves to Fable 5.2 instead of Fable 5.1, and if you had explicitly picked Fable before, Claude Code moved your saved pick forward for you.

## What "default Fable model" actually means

Claude Code gives you five model aliases: `sonnet`, `opus`, `haiku`, `fable`, and `default`. `fable` is the one reserved for "your hardest and longest-running tasks," per the same docs, and it draws on usage credits rather than your normal subscription rate limit, meaning it costs extra on top of Pro or Max. Before v2.1.321, `fable` meant Fable 5.1. After it, `fable` means Fable 5.2.

The practical trigger is this: if you had `claude-fable-5-1` saved as your model, Claude Code moved it to the `fable` alias, which now resolves to 5.2. Two paths, both from the changelog and the model-config docs:

- **Pro, Max, or Ultra** : the move happened the first time you ran v2.1.321 or later. Your terminal's startup model line showed`(auto-updated)` once, then went quiet.
- **Team or Enterprise** : the move waits on your organization's own settings. It can land on a later startup, or never, depending on what an admin allows.

Run `/model` right now to see what you're actually on. If it says `fable` or `claude-fable-5-2`, you got moved. If it still says `claude-fable-5-1` explicitly (not the alias), you pinned a version and the switch skipped you, which is by design, not a bug.

## The price moved the right direction

Here's the table worth screenshotting for your own records, built from the Claude Code changelog and Anthropic's live model-pricing page:

| Model | Input /MTok | Output /MTok | Cache hits /MTok | Where it applies | Source | 
|---|---|---|---|---|---|
| Fable 5.1 (prior `fable` ) | $10 | $50 | $1 | Legacy pick, still usable by exact ID | Anthropic pricing | 
| Fable 5.2 (current `fable` ) | $8 | $40 | $0.20 | Default fable alias since v2.1.321 | Claude Code changelog | 
| Fable 5.2, fast mode on | $16 | $80 | n/a | Opt-in via `/fast` , usage credits only | fast-mode docs | 
| Opus 5.5 (for comparison) | $4 | $20 | $0.20 | Cheaper tier, no fable-style gating | Anthropic pricing | 

Both sources were checked November 12, 2026.

*Source: platform.claude.com, captured November 12, 2026.*

The cache-hit drop from $1 to $0.20 per MTok, per Anthropic's pricing page, is the part a flat input/output comparison misses. If you run Fable on a repo you keep reopening in the same session, most of your input tokens after the first turn are cache hits, not fresh reads. A 5x cheaper cache-hit rate matters more to your actual bill than the $2 drop on fresh input tokens.

*Source: code.claude.com, captured November 12, 2026.*

## Fast mode showed up on Vertex, and it is not free

Version 2.1.322 (November 10, 2026) added fast mode support for Fable 5.2 when you run Claude Code through Google Cloud's Agent Platform, the renamed Vertex AI integration, per the same changelog. Fast mode isn't new on the Anthropic API; what's new is that Vertex deployments can now use it on Fable 5.2 at all.

Mechanically, fast mode is "the same model with a different API configuration that prioritizes speed over cost efficiency," per Claude Code's fast-mode docs. Quality doesn't change. Speed does, up to 2.5x, and so does price: $16/$80 per MTok on Fable 5.2 with fast mode on, versus $8/$40 standard.

On Vertex, fast mode has a region requirement the Anthropic API version doesn't: it needs your Fable 5.2 region set to `global`, `us`, or `eu`. Set it before you start a session:

`export VERTEX_REGION_CLAUDE_FABLE_5_2=global`
Then toggle it on inside Claude Code with `/fast`, or pass it directly in a scripted run:

`claude -p "Summarize this project" --model claude-fable-5-2 --settings '{"fastMode": true}'`
In any other region, `/fast` reports `Fast mode needs Vertex region global, us or eu` and the session runs at standard speed instead of failing, per the fast-mode docs. That graceful fallback is worth knowing before you ship a script that assumes fast mode is active: check `/fast` status rather than trusting your config silently worked.

*Source: code.claude.com, captured November 12, 2026.*

## A smaller change worth one line: shared settings for remote-control servers

The same v2.1.321 release added a `--settings <file>` flag to `claude remote-control`, the command that runs Claude Code as a server for claude.ai or the Claude app to connect to. Per the changelog, it "applies a settings file to every session it starts; if the file changes, no new session starts until you restart `claude remote-control`." That's a narrow admin convenience: one settings file governs every session the server spins up, rather than configuring each session by hand. Most solo developers won't touch it; teams running shared remote-control servers for multiple contributors are the actual audience.

## Rolling back, and when you'd want to

Switching back is one command:

`/model claude-fable-5-1`
That pins the exact prior version rather than the alias, so a future Fable bump won't drag you forward again without you asking. Reasons to actually use it: you've validated a workflow against 5.1's specific output shape and don't want variance mid-project, or your org's Team/Enterprise policy hasn't rolled out 5.2 yet and you want consistency with teammates still on the old pick.

Reasons to stay on the new default: the price is lower on every dimension in the table above, and Fable's job (hard, long-running tasks) hasn't changed. There's no quality trade being asked of you here, which is unusual enough that it's the main reason this update is worth a guide instead of a changelog skim.

## The actual decision

Most Claude Code users never touch Fable at all, since it's not anyone's account default and it bills against usage credits on top of your subscription. If you're already a Fable user: keep the auto-upgrade, because 5.2 costs less than 5.1 did, and check `/model` once to confirm you landed on it. If you're on Team or Enterprise and haven't seen the switch yet, that's your org's rollout pace, not a bug on your end, ask whoever owns Claude Code admin settings when they're flipping it on. And if you're running Claude Code through Vertex AI for the first time this week, set `VERTEX_REGION_CLAUDE_FABLE_5_2` to `global`, `us`, or `eu` before you bother testing fast mode, or you'll spend ten minutes debugging a feature that was never going to activate in your region.

## What changed

- **2026-11-12** : Initial publish, covering Claude Code v2.1.321 (Fable 5.2 as the default`fable` alias,`--settings` on`claude remote-control` ) and v2.1.322 (Vertex fast mode for Fable 5.2 in the global, US, and EU regions).
