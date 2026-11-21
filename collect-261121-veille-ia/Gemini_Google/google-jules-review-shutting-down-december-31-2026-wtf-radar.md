---
id: collect-261121-veille-ia/gemini-google/google-jules-review-shutting-down-december-31-2026-wtf-radar
title: "Google Jules Review: Shutting Down December 31, 2026 [WTF Radar]"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["jules", "google", "down", "december", "tasks", "november", "agent", "plan"]
source: https://wetheflywheel.com/en/radar/google-jules-review/
source_anchor: ""
source_lines: [1, 75]
section: "Gemini_Google"
---
Google Jules is ending. Google announced on October 29, 2026 that the asynchronous coding agent winds down on December 31, 2026, and goes read-only after November 11. This page covers the dates, the export, and where Jules users can go.

#### Key Takeaways

-  **Jules is winding down.** â Google announced on October 29, 2026 that Jules shuts down on December 31, 2026. Sessions can be finished until November 11; after that Jules is read-only.
-  **Export before the deadline.** â The Jules Tools CLI exports settings, environment variables, scheduled tasks, memories, setup scripts and every session. Code and PRs already live in GitHub.
-  **Google points to Antigravity.** â Google says the lessons from Jules shape its other products, including Google Antigravity, its agent-first platform.

## What are the Jules shutdown dates?

From the Jules changelog entry of October 29, 2026:

| Date | What happens | 
|---|---|
| October 29, 2026 | Wind-down announced; export available | 
| November 11, 2026 | Last day to run and finish sessions | 
| November 12 to December 31, 2026 | Read-only | 
| December 31, 2026 | Jules shuts down | 

Google describes the move as unifying its agentic coding work and says the lessons feed Google Antigravity, its agent-first platform. Jules launched in May 2025 and left beta on August 6, 2025. It cloned a GitHub repository into a cloud VM, wrote a plan for approval, made the change and opened a pull request. Later updates added a CLI and API, scheduled and suggested tasks, MCP support, automatic CI-failure fixes and Gemini 3.1 Pro (March 2026).

## How does Google Jules score?

These scores record Jules as documented before the wind-down; they are not a recommendation to adopt it. WTF Radar scores four criteria from 1 to 5, using published vendor documentation rather than benchmarks or hands-on tests: how far the agent runs without a person (autonomy), what reviewers and admins can constrain (control), how many places it works (integration), and whether the price of real usage is published (pricing clarity). Total: **12/20**.

| Criterion | Score | Why | 
|---|---|---|
| Autonomy | **4/5** | Ran tasks asynchronously in a cloud VM, planned, opened PRs, fixed CI failures and ran scheduled tasks. | 
| Control | **3/5** | Plan approval before coding and a critic agent; little admin tooling, paid plans for personal Google accounts only. | 
| Integration | **1/5** | GitHub only, and read-only after November 11, 2026. | 
| Pricing clarity | **4/5** | Task limits per plan were published; paid tiers came through Google AI Pro and Ultra. | 

## What did Jules cost, and what happens to paid plans?

Per the Jules limits page: the free plan allowed 15 tasks a day and 3 concurrent tasks; Jules in Pro, included with Google AI Pro, allowed 100 and 15; Jules in Ultra, included with Google AI Ultra, allowed 300 and 60. Paid access only applied to personal Google accounts. Jules was a benefit of those plans rather than a separate subscription, so check your Google AI plan for any changes to its benefits.

## Who is Google Jules for?

- **Current users:** export before November 11, 2026, move scheduled tasks to another agent, and remove the Jules GitHub app once you no longer need it.
- **New evaluations:** do not adopt Jules. Shortlist one of the alternatives below.

## What are the Google Jules alternatives?

- Factory: background Droids with automations for recurring work.
- Amp: agents on their own cloud machines; can be assigned issues from Linear.
- Devin or Claude Code: the comparison for autonomous PR work.
- Gemini CLI, Codex or Claude Code: for teams staying close to Google models.

## Related questions

- Which coding agents keep working in the background?
- Codex, Claude Code or Gemini CLI?
- What does agent autonomy actually cost? (partner site CTAIO)
- Where do humans stay in the loop in an agentic software lifecycle? (partner site CTAIO)

## Sources

- Google, Jules changelog: Jules Turndown (Oct 29, 2026).
- Google, Jules limits and plans (fetched Nov 3, 2026).

### Is Google Jules shutting down?

Yes. Google announced on October 29, 2026 that Jules will wind down on December 31, 2026. Users can finish sessions until November 11, 2026; after that Jules becomes read-only until the end of the year.

### How do I export my Jules data?

Install the Jules Tools CLI (npm install -g @google/jules), run jules login, and use the export described in the Jules changelog. It saves account settings, environment variables, scheduled tasks, connected repositories, memories, setup scripts and every session's prompt, plan, activity log and patches to a local folder. Code, branches and pull requests are already in GitHub.

### What should Jules users switch to?

Google points to Google Antigravity, its agent-first platform. Other asynchronous agents that open pull requests from a cloud environment include Devin, Factory Droids, Amp, OpenAI Codex and Claude Code. Pick one that supports the GitHub workflow and scheduled tasks you relied on.

### What did Jules cost?

Jules had a free plan with 15 tasks a day and 3 concurrent tasks. Jules in Pro (100 tasks a day, 15 concurrent) came with Google AI Pro, and Jules in Ultra (300 tasks a day, 60 concurrent) with Google AI Ultra, for personal Google accounts.
