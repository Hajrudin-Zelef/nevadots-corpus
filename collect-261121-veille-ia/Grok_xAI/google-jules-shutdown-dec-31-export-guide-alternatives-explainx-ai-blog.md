---
id: collect-261121-veille-ia/grok-xai/google-jules-shutdown-dec-31-export-guide-alternatives-explainx-ai-blog
title: "Google Jules Shutdown Dec 31: Export Guide + Alternatives | explainx.ai Blog"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-10-29"]
keywords: ["jules", "google", "export", "november", "antigravity", "changelog", "december", "shutdown"]
source: https://explainx.ai/blog/google-jules-shutdown-december-31-antigravity-export-guide-2026
source_anchor: ""
source_lines: [1, 95]
section: "Grok_xAI"
---
Google is ending Jules, its asynchronous coding agent. A changelog entry titled "Jules Turndown", dated October 29, 2026 on Google's Jules docs, sets the schedule: you can finish active sessions until November 11, 2026, Jules becomes read-only after that, and the product winds down on December 31, 2026. If you use Jules for background bug fixes or test writing, you have about six weeks to export what you care about and pick a replacement.

This post covers what Google has confirmed, what it has not, how the export works, and how to think about the move to Antigravity. It is based on the changelog itself, the official Jules account's post, and our earlier coverage of Google's coding stack.

## Quick reference: the Jules shutdown at a glance

| Question | Answer (source: Jules changelog, Oct 29, 2026) | 
|---|---|
| When can I still start and finish work? | Until November 11, 2026 to continue finishing sessions | 
| What happens after November 11? | Jules is read-only until the end of the year | 
| When is it fully gone? | December 31, 2026 | 
| Is my code lost? | No. Code, branches and pull requests stay in GitHub | 
| What can I export? | Account settings, environment variables, scheduled tasks, connected repositories, memories, setup scripts, and every session (prompt, plan, activity log, code patches) | 
| Where does the export go? | `~/jules-export` on your machine | 
| What replaces it? | Google points to Antigravity, its agent-first platform | 
| Is there a date for Jules-style background tasks in Antigravity? | Not in the notice | 

## What Google actually said

Google frames Jules as an experiment. The changelog says Jules was a project to learn how asynchronous coding agents could help developers ship faster, and that the lessons now inform several Google products, including Antigravity. The official Jules account on X put the direction in one line: the team is "bringing our learnings from Jules into @antigravity so you get the best of both worlds: autonomous remote background task execution and an agent-first development environment."

Two things are worth separating. The shutdown dates are specific and confirmed by Google's own docs. The Antigravity plan is a stated intention: the notice does not give a launch date for background-task features in Antigravity, a user count for Jules, or any statement about how Google AI Pro and Ultra task allowances for Jules will be treated. Those gaps are real, and if you pay for a Google plan partly for Jules, ask Google support what changes rather than assuming.

## The three dates, and why November 11 matters most

Most people will read "December 31" and relax. The more important date is **November 11**. After it, Jules stops accepting new work and only lets you read what is there. If you run scheduled tasks, such as nightly dependency bumps or weekly test-coverage jobs, those stop being useful on that date, not on New Year's Eve.

A practical timeline:

1. **Now to November 11:** finish or abandon open sessions, merge or close pull requests Jules opened, and run the export.
2. **November 11 to December 31:** read-only. You can still look at old sessions and plans, which is a reasonable window to double-check the export is complete.
3. **December 31:** gone. Assume anything not exported or already in GitHub is unrecoverable.

## How to export your Jules data

The changelog describes an agent-assisted export. The steps, as Google lays them out:

1. Install Jules Tools: `npm install -g @google/jules`
2. Authenticate: `jules login`
3. Open a CLI coding agent, such as Claude Code, from your home directory and paste the export prompt provided in the changelog entry.
4. The agent checks your setup, runs the export and reports what it saved.

Everything lands in `~/jules-export`. That includes account settings, environment variables, scheduled tasks, connected repositories, memories, setup scripts and each session's prompt, plan, activity log and code patches.

Two cautions from the notice itself. First, repository code, branches and pull requests are not part of the export because they live in GitHub; if you want repository data included, connect your GitHub account to Jules before exporting. Second, `env-variables.json` contains your real variable values. Keep the export folder private, do not commit it, and consider rotating any secret you stored there once you have migrated.

That second point connects to a pattern we have covered repeatedly: tools that hold your secrets are an attack surface the day you stop trusting or using them. Our write-ups of the LiteLLM PyPI compromise and the backdoored Qwen model that read .env files both come back to the same habit: keep secrets scoped, and treat exports as sensitive files.

## What Jules was, and why folding it into Antigravity makes sense

Jules ran tasks asynchronously in a cloud environment: you described a bug fix or a test to write, Jules planned it, worked in a remote workspace and opened a pull request. That is a different shape from an interactive terminal or IDE agent, where you watch and steer.

Antigravity is Google's agent-first development environment, which also has a command-line interface with sandboxing, plugins and subagents. We broke down the CLI in our Antigravity CLI guide, and covered the earlier consolidation of Google's terminal agent in the Gemini CLI deprecation and Antigravity migration guide. Jules is now the second Google coding product pointed at Antigravity. The pattern is consistent: Google is reducing the number of separate coding surfaces and concentrating on one.

For users, the trade is familiar. A single platform is easier to learn and to budget for, but you lose the dedicated product you chose. The risk is mostly timing: if Antigravity does not ship background execution before Jules goes read-only, there is a gap in which Google has no first-party async agent.

## Where can I go instead?

Blocks crossing a bridge between two trays, showing a Google Jules shutdown migration to a new coding agent

None of the following is a Google-confirmed replacement; they are options that cover similar ground, so pick by how you worked with Jules.

| If you used Jules for... | Consider | 
|---|---|
| Fire-and-forget tasks that return a pull request | A cloud or background mode in another agent harness; see our agent harness guide for what to look for | 
| Interactive fixes from the terminal | Antigravity CLI, Claude Code or another CLI agent; our Claude Code vs Codex vs Gemini CLI comparison lists trade-offs | 
| Scheduled maintenance jobs | A scheduler plus any CLI agent, or a loop pattern as described in our loop engineering guide | 
| Open-source, self-hosted control | Open-source agents such as OpenCode or Kilo Code | 

Whatever you choose, port the portable parts first: setup scripts, the environment variables you actually need, and your standing instructions or memories. Plain text instructions move between agents easily; session transcripts mostly do not.

## What this means for teams that built workflows around Jules

If Jules was one person's side tool, the shutdown is an afternoon of work. If a team wired it into its process, the cost is larger and mostly hidden. Jules was often the thing that turned a ticket into a first-draft pull request, so reviewers got used to a particular style of change: a plan, an activity log and a patch. Replacing the tool also replaces that review experience, and the new agent's pull requests may be bigger, smaller or structured differently.

Three questions help scope the move. Which repositories did Jules touch, and does each have a setup script that another agent can reuse? Which jobs ran on a schedule, and who owns re-creating them? And which environment variables were stored only in Jules, so that nobody else has a copy? The last one catches people out: a secret that exists in exactly one product's settings page is a migration risk and, after December 31, a recovery problem.

It also pays to run the replacement in parallel for a couple of weeks before November 11. Give both agents the same small task, compare the pull requests, and only then retire the old workflow. That is cheap insurance, and it gives you real evidence about whether a successor, Antigravity or otherwise, handles your codebase as well as Jules did.

## A short migration checklist

- Merge, close or hand off every pull request Jules opened, so none are orphaned.
- Run the export before November 11 and open `~/jules-export` to confirm sessions and setup scripts are there.
- Move the export folder somewhere private and off any Git repository.
- Rotate secrets that lived in Jules environment variables once your new tool is configured.
- Re-create scheduled tasks in your replacement tool before Jules stops accepting new work.
- Check your Google plan terms if Jules was part of why you subscribed.

## What is confirmed and what is still open

Confirmed by Google's docs: the three dates, the export contents, the GitHub carve-out, the warning about environment variables, and Antigravity as the stated home for what Google learned. Confirmed by the official Jules account: the intent to bring autonomous remote background execution into Antigravity.

Not confirmed: when that capability arrives, whether it matches Jules task for task, how many people used Jules, and what happens to plan allowances. We will add an update section here if Google publishes those details.

*Details are accurate as of October 30, 2026 and may change; check the Jules changelog for the current notice.*
