---
id: collect-261121-veille-ia/claude-anthropic/fable-5-2-in-claude-code-up-to-30-faster-for-20-less-wmedia-es
title: "Fable 5.2 in Claude Code: up to 30% faster for 20% less | wmedia.es"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-17"]
keywords: ["fable", "claude", "model", "mtok", "code", "update", "november", "output"]
source: https://wmedia.es/en/tips/claude-code-fable-5-2-faster-for-less
source_anchor: ""
source_lines: [1, 93]
section: "Claude_Anthropic"
---
**TL;DR** Run `claude update` to **v2.1.321**, then `/model fable`. Fable 5.2 drops from $10/$50 to **$8/$40 per million tokens**. On Pro it draws on usage credits from the first request: the included launch week ended on November 16.


**Update, November 17, 2026:** the launch offer for Pro and standard Team seats ended on November 16 at 11:59 pm PT. The table of what each plan includes now shows it.


Fable 5.2 has been the model behind the `fable` alias in Claude Code since November 9, 2026, but only from **v2.1.321** on. Below that version, `/model fable` still gives you Fable 5.1 at the old price, with no warning. It is still the tier above Opus, with two changes you notice on day one: output is up to 30% faster, and every line of the price list drops 20%.

## What changes from Fable 5.1

|  | Fable 5.1 | Fable 5.2 | 
|---|---|---|
| Input | $10 / MTok | **$8 / MTok** | 
| Output | $50 / MTok | **$40 / MTok** | 
| Cache write (1 h) | $20 / MTok | **$16 / MTok** | 
| Cache read | $0.25 / MTok | **$0.20 / MTok** | 
| Output speed | as before | up to 30% faster | 
| Default effort in Claude Code | `high` | `medium` | 
| Fast mode ( `/fast` ) | not available | $16 / $80 per MTok | 

Cache reads stay at 2.5% of input, the cut that arrived with Fable 5.1. Because all four lines drop together, a long session costs the same 20% less as a short one.

On capability, Anthropic reports 49.1% on FrontierCode 2.0, against 32.4% for Fable 5.1 and 45.5% for Opus 5.5. The number that matters day to day is a different one: at `high`, Fable 5.2 matches Opus 5.5 at `max` (45.3% against 45.5%) for roughly half the cost per task.

## Check which Fable is answering

The alias is spelled the same on both versions, so the number from `claude --version` is what decides. To see it in a real response, ask for JSON and read which model billed it:

```
claude -p "Reply with the single word ok" --model fable --output-format json | jq -c '.modelUsage | keys'
```
Output on v2.1.319:

```
["claude-fable-5-1"]
```
Output on v2.1.321:

```
["claude-fable-5-2"]
```
That same minimal request cost **$0.184 on Fable 5.1 and $0.141 on Fable 5.2** at list price. If Fable draws on usage credits on your plan, with `-p` Claude Code bills it without asking, so for a check that costs nothing, the version number is enough.

## How to move to Fable 5.2

**1. Update Claude Code**

```
claude update
claude --version
```
You need `2.1.321` or later. If your release channel is `stable`, or you installed the `claude-code` Homebrew cask, today you land below it: that channel serves versions about a week old. Switch to `latest` from `/config`, or with `"autoUpdatesChannel": "latest"` in `~/.claude/settings.json`.

**2. Select the model**

```
/model fable
```
The choice is saved to your user settings, and later sessions start there. On Pro and Max, a saved `claude-fable-5-1` moves to the `fable` alias by itself on the first run of v2.1.321. To stay on the previous model, select it by ID: `/model claude-fable-5-1`.

**3. Leave effort at `medium` and raise it only for long work**

Fable 5.2 starts at `medium`, unless you had already set a level for it. Anthropic recommends that level for interactive coding and keeps `high` for hard work and long unattended runs (what each level costs, measured):

```
/effort high
```
If you switch models often, set each model's effort once.

**Can't update yet?** `/model claude-fable-5-2` also answers on v2.1.319, but Claude Code doesn't recognize the model: it logs `[claude-code:unrecognized_model]` and can't work out its cost. Fine as a stopgap, not as a setup.

## What your plan includes

| Plan | Fable | Launch offer | 
|---|---|---|
| Pro and standard Team seats | Usage credits from the first request | Ended November 16, 2026, 11:59 pm PT | 
| Max, Ultra and premium seats | Included for up to 50% of your weekly limit | One to three Fable limit resets, for under 6 months, 6 to 12, or more than 12 months subscribed. They expire March 8, 2027, 11:59 pm PT | 

From November 9 to 16, Pro had Fable included for up to 50% of its weekly limit; now it bills to usage credits, if you have them turned on. A reset restores what you used; it doesn't raise the limit. Here is where you see what you have left. Which sessions bill without asking, and how to cap the spend: Fable 5.2 on usage credits on Pro.

## What you will notice in use

- **A turn can end before the work is done.** On long tasks it sometimes closes with a list of what remains or an offer to continue, more often at`low` and`medium` . For long unattended work, go up to`high` (what else to do when it stops).
- **Telling it to think less no longer works.** A line such as "answer directly without deliberating" doesn't stop the thinking at`medium` and above. If easy requests take too long, lower the effort instead of adding instructions (what else your`CLAUDE.md` no longer needs).
- **Fewer reroutes to Opus.** Anthropic measures about 55% fewer blocks than on Fable 5.1 (60% fewer in cybersecurity, 40% fewer in biology). When one happens, the session continues on Opus until you switch back with`/model` . And if your organization has CVP Defense Access, Fable doesn't get it: do security work on Opus 5.5.
- **`/fast` has its own price.** Up to 2.5 times faster at $16/$80, always from usage credits even with plan usage left. Turn it on at the start of a session: the first time, you pay for the whole context uncached.

To decide when it pays off against Opus and Sonnet, this is how I split the models.

Official docs: Model configuration · Fable 5.2 announcement · Launch promotion


**Requirements:** Claude Code v2.1.321 or later and a paid plan (Pro, Max, Ultra, Team or Enterprise) or API access.
