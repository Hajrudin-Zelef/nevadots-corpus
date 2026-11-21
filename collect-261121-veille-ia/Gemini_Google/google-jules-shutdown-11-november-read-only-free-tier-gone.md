---
id: collect-261121-veille-ia/gemini-google/google-jules-shutdown-11-november-read-only-free-tier-gone
title: "Google Jules Shutdown: 11 November Read-Only, Free Tier Gone"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-11"]
keywords: ["jules", "google", "free", "november", "tier", "tasks", "read-only", "shutdown"]
source: https://efreedirectory.com/jules-shutdown-free-tier/
source_anchor: ""
source_lines: [1, 103]
section: "Gemini_Google"
---
**Google will wind down Jules on 31 December 2026. The announcement on its changelog, dated 29 October, gives users until 11 November 2026 to finish active sessions, after which Jules becomes read-only until the end of the year. The free tier that allowed 15 tasks a day and 3 concurrent tasks has no announced replacement, and Google's notice does not say what happens to the higher task limits it sold through Google AI Pro and Ultra.**<sup>1</sup>

This is the rarest kind of plan change: not a tier being squeezed, but a whole product being closed. For anyone who built a workflow around the free allowance, the practical deadline is not December. It is 11 November, thirteen days after the announcement.<sup>2</sup>

## The shutdown, in dates

| Date | What happens | What you can still do | 
|---|---|---|
| 29 October 2026 | Shutdown announced on the Jules changelog | Everything works normally | 
| 11 November 2026 | Jules becomes read-only | View past sessions, no new tasks | 
| 31 December 2026 | Jules shuts down | Nothing, access ends | 

**Prices measured on 27 October 2026.** Vendor plan terms, allowances and prices change without notice, so treat every figure on this page as a dated snapshot rather than a permanent fact. Where a number here differs from the vendor's own pricing page, the vendor's page is correct and the figure above is simply older than it.

Google's wording is that this is "part of our work to unify our agentic coding efforts", and that what users built with Jules "taught us where agentic development is headed", with those learnings shaping products "including our agent-first platform, Google Antigravity".

Read that carefully. It promises that lessons carry over. It does not promise that features carry over, that limits carry over, or that anything arrives on a schedule.

## What the free tier actually was

Jules ran on three plans and the differences were entirely about volume. Every tier used the same models; only the task counts changed.

| Plan | Daily tasks | Concurrent tasks | Cost | 
|---|---|---|---|
| Jules (free) | 15 | 3 | $0 | 
| Jules in Pro | 100 | 15 | Included with Google AI Pro | 
| Jules in Ultra | 300 | 60 | Included with Google AI Ultra | 

Fifteen tasks a day is not a demo allowance. It is enough to clear a backlog item, open a pull request, review it and fix the review comments, every working day, on a free account. That is why its removal matters more than the closure of a typical experimental product.

Two other details from Google's own limits page are worth carrying forward. The daily limit is a rolling 24-hour window, not a midnight reset, so the clock starts from each task you run. And when you hit the limit the new-task button disables, but your history stays readable, which is the same posture the read-only mode will take from 11 November.

## The export, and the detail that raised eyebrows

Google has published an export path, and it covers more than most shutdown notices bother with: account settings, environment variables, scheduled tasks, connected repositories, memories, setup scripts, and every session including its prompt, plan, activity log and code patches.

Your code is not at risk. Branches, commits and pull requests live in GitHub and stay there. What the export preserves is the context that only existed inside Jules.

To run it you install the Jules command line tool and authenticate:

- npm install -g @google/jules
- jules login

Then, in Google's own instructions, you "open a CLI coding agent, such as Claude Code, from your home directory and paste the export prompt below".

That is Google telling its users to finish the migration using a competitor's agent. The notice names Claude Code as its example and recommends no Google product for the job. It is an honest instruction and a strange one to read from the company that also runs Antigravity.

Two warnings about the export. It writes your environment variables to a file called env-variables.json, so keep the folder out of any repository. And if you have never connected GitHub to Jules, the export will simply contain no repository data, which is easy to discover too late.

## What replaces it, and what does not

Google points users to Antigravity and says it is working on bringing Jules's autonomous background execution into that platform. It has not published a date, a limit, or a price for that work.

The free tier is the specific gap. Antigravity has a free individual tier, and its model access differs sharply by plan, but nothing in Google's announcement commits to a free allowance of daily autonomous coding tasks. Fifteen a day with three in parallel was a real product decision, and its absence from the Antigravity documentation so far is the thing to watch.

The same silence covers paying customers. Jules in Pro and Jules in Ultra were sold as benefits of Google AI Pro and Ultra, and those subscriptions continue. The notice does not say what replaces the Jules allowance inside them. Anyone paying $19.99 or more a month partly for Jules task limits has been told the product ends and nothing about the value that replaces it.

There is better news on the same day: Antigravity launched a plugin marketplace, which turns manual connector setup into one-click installs. It is a real improvement to the platform Google is moving everyone toward. It is also not a free coding agent with a daily task allowance.

## What to do before 11 November

Finish anything in flight. Read-only means the agent stops working, so a half-finished task on 12 November is finished by hand.

Run the export before the cutoff rather than after. Google says the tooling works during read-only, but the access token expires hourly and the endpoints it uses are undocumented, so there are more ways for it to fail on the last evening than on a Tuesday morning.

Then decide where the work goes. If you were on the free tier, the honest options are a different free agent, or accepting a paid plan somewhere. GitHub Copilot's free plan remains the most generous zero-cost coding allowance on the market, and Cursor's free tier covers light use.

The temptation is to assume Antigravity will absorb Jules's free allowance because Google says the learnings are moving there. It might. But a shutdown notice is a statement about what is ending, and this one is unusually clear about the ending and completely silent about the beginning that follows.

## Frequently asked questions

**When does Jules shut down?**

31 December 2026. It becomes read-only on 11 November 2026 and stops accepting new tasks from that date.

**Is Jules free?**

It had a free tier of 15 tasks per rolling 24 hours with 3 concurrent tasks. Higher limits came with Google AI Pro and Ultra.

**Will I lose my code?**

No. Code, branches and pull requests stay in GitHub. Only Jules-specific data needs exporting.

**How do I export my Jules data?**

Install @google/jules with npm, run jules login, then follow Google's published export prompt using any CLI coding agent.

**Does Antigravity replace Jules?**

Google says Jules's lessons are shaping Antigravity and that it is working on background task execution there. No date, limit or price has been published.

**What happens to my Google AI Pro subscription?**

It continues. Google has not said what replaces the Jules task allowance included in it.

*Shutdown dates, the read-only cutoff, export contents and the Claude Code instruction verified on the Jules changelog on 31 October 2026. Free, Pro and Ultra task limits verified on Jules's own Limits and Plans page the same day. The Antigravity plugin marketplace and the absence of a published Jules replacement are as reported on 30 October.*

## Sources

1. Jules changelog: Jules Turndown: jules.google/docs/changelog. Used for: The 29 October turndown notice, read-only and shutdown dates, export contents and the Claude Code export instruction.
2. Jules: Limits and Plans: jules.google/docs/usage-limits. Used for: Free, Pro and Ultra daily task limits, concurrency and the rolling 24-hour window.

*All sources are the vendor's own plan or documentation pages, checked on 27 October 2026. Plan terms change without notice, so confirm current limits before you rely on them.*
