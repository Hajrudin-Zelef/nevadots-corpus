---
id: collect-261121-veille-ia/claude-anthropic/how-to-use-claude-sonnet-5-5-in-claude-code-and-when-to-keep-opus-5-5
title: "How to Use Claude Sonnet 5.5 in Claude Code (and When to Keep Opus 5.5)"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-28"]
keywords: ["sonnet", "claude", "model", "code", "effort", "opus", "anthropic", "default"]
source: https://apidog.com/blog/claude-sonnet-5-5-claude-code/
source_anchor: ""
source_lines: [1, 153]
section: "Claude_Anthropic"
---
To use Claude Sonnet 5.5 in Claude Code, update to v2.1.284 or later, then launch with `claude --model claude-sonnet-5-5` or pick it with `/model`. You need a paid Claude plan (Pro, Max, Team or Enterprise) or an Anthropic API key; the Free plan doesn’t include Claude Code. The default model stays Opus 5.5 on all of them, so Sonnet 5.5 runs only when you select it. At API rates it costs half as much: $2/$10 per million input/output tokens against $4/$20 for Opus 5.5.

Below: setup, the `sonnet` alias trap, effort, habits to steer, cost, and when Opus 5.5 still wins. For the model itself, see what Claude Sonnet 5.5 is. If your code calls APIs, Apidog holds the test scenarios Claude Code can run against your changes.

## Claude Sonnet 5.5 in Claude Code at a glance

| Setting | Sonnet 5.5 | 
|---|---|
| Minimum version | v2.1.284 | 
| Plans | Pro, Max, Team, Enterprise, API key; not Free | 
| Default model | Opus 5.5, so select Sonnet 5.5 yourself | 
| `sonnet` alias | Sonnet 5.5 on the Anthropic API only | 
| Context | 1M native | 
| Default effort | `medium` ; thinking can’t be turned off | 
| Fast mode | Not available | 

Sources: Claude Code’s model configuration docs and Anthropic’s launch post.

## Step 1: update Claude Code and check your plan

Sonnet 5.5 arrived in Claude Code v2.1.284 on September 28, 2026, and older builds can’t run it:

```
claude update
```
Per claude.com/pricing, Pro costs $17 a month billed annually or $20 monthly, and Max starts at $100. Free users can chat with Sonnet 5.5 on Claude.ai, but not in Claude Code; our free-access guide covers what works.

## Step 2: switch the model to Sonnet 5.5

```
claude --model claude-sonnet-5-5
```
Inside a session, `/model claude-sonnet-5-5` switches and saves it as your default. Run `/model` alone to open the picker, where `s` switches for this session only. `ANTHROPIC_MODEL` or a `"model"` key in a settings file work too, and `/status` shows what’s active.

A `/model` switch also reaches subagents that inherit the main model, and a resumed session keeps the model it was saved with. If you pinned `claude-sonnet-5` for our Sonnet 5 Claude Code guide, update it: a full ID doesn’t move when the alias does.

### The `sonnet` alias doesn’t mean Sonnet 5.5 everywhere

`sonnet` points to Sonnet 5.5 on the Anthropic API only:

| Provider | `sonnet` resolves to | 
|---|---|
| Anthropic API | Sonnet 5.5 | 
| Claude Platform on AWS | Sonnet 4.6 | 
| Amazon Bedrock, Google Cloud’s Agent Platform | Sonnet 4.5 | 
| Microsoft Foundry | Sonnet 4.5 | 

On a cloud provider, set `ANTHROPIC_DEFAULT_SONNET_MODEL` to your provider’s Sonnet 5.5 ID: `claude-sonnet-5-5` on Google Cloud and Claude Platform on AWS, your deployment name on Foundry, your inference profile on Bedrock. The same variable sets the model `opusplan` executes with.

Skip `sonnet[1m]`: the docs say it has no effect once `sonnet` is Sonnet 5.5, whose 1M window is native.

## Step 3: pick an effort level (thinking stays on)

Sonnet 5.5 starts at `medium` in Claude Code, one level below its `high` default on the Claude Platform API. Levels run `low`, `medium`, `high`, `xhigh`, `max`:

```
claude --model claude-sonnet-5-5 --effort high
```
In a session, `/effort high` saves a level for the current model, `/effort` opens a slider, and `/effort auto` clears it. `CLAUDE_CODE_EFFORT_LEVEL` sets it from the environment. Changing effort mid-conversation can invalidate the prompt cache, so Claude Code may warn you first.

You can’t turn thinking off here. The `Option+T` (`Alt+T`) toggle, `alwaysThinkingEnabled` and `MAX_THINKING_TOKENS=0` have no effect on Sonnet 5.5. Anthropic’s prompting guide adds that from `medium` up it thinks before almost every reply, and asking it to “think less” doesn’t work reliably. Lower the effort instead, or add `ultrathink` to one prompt for a single deeper turn.

### What each level buys on CursorBench

Cursor measured these CursorBench 4.0 scores in its own harness; the per-task costs come from Anthropic’s launch charts.

| Effort | Sonnet 5.5 | Opus 5.5 | 
|---|---|---|
| `low` | 35.8% at $0.50 | 43.7% at $1.17 | 
| `medium` | 39.2% at $0.70 | 52.5% at $2.91 | 
| `high` | 47.8% at $1.67 | 56.0% at $3.97 | 
| `xhigh` | 53.1% at $3.88 | 56.0% at $6.98 | 
| `max` | 55.5% at $9.67 | 57.8% at $13.43 | 

From `low` to `high`, Sonnet 5.5 costs less than half of Opus 5.5 at the same level. Look across levels, though: Opus 5.5 at `medium` ($2.91) outscores Sonnet at `high` and lands within a point of Sonnet at `xhigh`, which costs $3.88. If you’re pushing Sonnet 5.5 to `xhigh` or `max`, try Opus 5.5 at `medium` first.

## Steer the habits that show up in a coding session

Anthropic’s Sonnet 5.5 prompting guide flags behaviors that matter in Claude Code:

- **It checks in early at `low` and `medium`** on long agentic tasks.
- **It adds tests, docs and small files you didn’t request** , at every effort level.
- **At `xhigh` and `max` it starts extra review rounds or reviewer subagents.** Anthropic ties this to a lower FrontierCode score at`max` (46.2%) than at`xhigh` (52.1%).
- **At `low` it can skip verification** and report code as done without running tests.

The guide has Anthropic’s own suggested prompt paragraphs; one cut session cost by about a third at `max` in Anthropic’s testing. Here’s a shorter set in our own words to adapt for your `CLAUDE.md`:

```
## How to work in this repo
- Keep working until the task is done. Stop to ask only if you are blocked or about to delete data.
- Change only what the task needs. No new tests, docs or files unless I ask.
- Before saying a task is done, run the relevant tests and show the command and result.
- Don't start reviewer subagents unless I ask for a review.
```
## Fast mode, opusplan and mixing models

Fast mode is Opus-only: Opus 5.5, Opus 5 and Opus 4.8, per the fast mode docs. Running `/fast` on Sonnet 5.5 switches the session to Opus. Sonnet’s speed comes from the model, which Anthropic says runs “30%+ faster” than Sonnet 5.

`claude --model opusplan` uses Opus in plan mode and Sonnet for execution. It fits work where the design is hard and the edits are routine. Creator’s Kevin Ngo, quoted on Anthropic’s launch page, said he’d let Sonnet 5.5 build a game architecture that Opus 5.5 set. You can also keep Opus as the main model and set `CLAUDE_CODE_SUBAGENT_MODEL=claude-sonnet-5-5` for subagents. Our Claude Code workflows guide has more patterns.

## When to keep Opus 5.5

Anthropic says Opus 5.5 “remains clearly stronger at complex, open-ended work,” with Sonnet 5.5 best at well-scoped tasks and bug fixes. The system card (Anthropic-run) agrees: SWE-Bench Pro 81.3 vs 89.9 at max effort, and ProgramBench, a long-context test, 79.7 vs 91.2.

Keep Opus 5.5 for ambiguous bugs, architecture calls, large-codebase sessions and fast mode. Pick Sonnet 5.5 for scoped fixes, specced features, tests, docs and subagent jobs. Sonnet does lead on Terminal-Bench 4.0 (70.6% at `max` vs 66.4% at `xhigh`), but Anthropic ran it, and fallback touched 10% of Opus trials against 1.5% of Sonnet’s. Details: Sonnet 5.5 vs Opus 5.5.

## What it costs in a session

With an API key, Claude Code bills list price: $2/$10 per MTok for Sonnet 5.5, $4/$20 for Opus 5.5, $0.20 cache reads on both. Anthropic also claims “up to 30% less per task” than Sonnet 5.

On Pro, Max, Team or Enterprise, sessions draw on your plan’s usage limits instead, and `/usage` shows where you stand. We found no published figure for how each model weighs against those limits, so measure on your own tasks.

Watch `max`: Simon Willison reported on Hacker News that his pelican test at `max` burned 128,000 thinking tokens in about 15 minutes and ran out before producing the SVG. More math: Sonnet 5.5 pricing.

## Migrate an API project from inside Claude Code

If your app calls Sonnet 5 through the API, Claude Code can migrate it:

```
/claude-api migrate this project to claude-sonnet-5-5
```
Expect real breaking changes: `thinking: {"type": "disabled"}` returns a 400 and becomes `between_tools`, forced `tool_choice` gives way to `auto` plus `strict: true` tools, and non-default `temperature`, `top_p` or `top_k` return a 400. Sonnet 5.5 vs Sonnet 5 covers each fix.

Then test against the live API. Save the old and new request as a pair in Apidog, keep `ANTHROPIC_API_KEY` in an environment variable, and assert on `stop_reason` and `usage`, so a refusal or a jump in output tokens shows up as a failed test.

## Run your API tests with the Apidog CLI

Since Sonnet 5.5 at `low` can skip testing, give it a test to run. The Apidog CLI runs your saved Apidog test scenarios from the terminal, so Claude Code can start a run, read the results, fix the handler and rerun. Add one rule to `CLAUDE.md`:

```
- After changing an API endpoint, run its Apidog test scenario with the Apidog CLI and report pass and fail counts.
```
Setup is in Apidog CLI in Claude Code, and the Copilot version is Apidog CLI in GitHub Copilot. Download Apidog to build the scenarios.

## Sonnet 5.5 in GitHub Copilot and Cursor

**GitHub Copilot.** Per GitHub’s changelog, Sonnet 5.5 went GA on September 28 for Copilot Pro, Pro+, Max, Business and Enterprise, billed at provider list pricing. It’s in VS Code, JetBrains, Copilot CLI, the coding agent and more, with a gradual rollout. Copilot Free and Student don’t get it; those plans are limited to auto model selection, which doesn’t include Sonnet 5.5.

**Cursor.** Cursor’s model page lists `claude-sonnet-5-5` with 200k default and 1M maximum context, billed from the Other Models pool on Pro and above at the same $2/$10 rates. US-only endpoints cost 10% more. Cursor hasn’t posted a changelog entry for it yet.

## FAQ

**Is Sonnet 5.5 the default in Claude Code?** No. `default` is Opus 5.5 on Pro, Max, Team, Enterprise and the Anthropic API. Select Sonnet 5.5 with `--model` or `/model`.

**Can I use it in Claude Code on the Free plan?** No, Claude Code is paid-only. Free users can chat with it on Claude.ai; see how to use Sonnet 5.5 for free.

**Why does `/model sonnet` give me an older model?** You’re on a cloud provider (Bedrock, Google Cloud, Foundry, Claude Platform on AWS) or a version before v2.1.284. Pin the full ID.

**How much cheaper is it than Opus 5.5?** Half at API list prices. See the pricing breakdown.

## Your next step

Run `claude update`, start your next scoped task with `claude --model claude-sonnet-5-5`, and leave effort at `medium` for a few days. Add the `CLAUDE.md` rules, send design-heavy work through `opusplan`, and keep Opus 5.5 one `/model` away. If the work touches APIs, wire in the Apidog CLI so every “done” has a test run behind it. Apidog is free to start.
