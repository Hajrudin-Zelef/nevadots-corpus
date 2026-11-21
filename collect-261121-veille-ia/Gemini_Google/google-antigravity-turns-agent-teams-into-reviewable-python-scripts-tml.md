---
id: collect-261121-veille-ia/gemini-google/google-antigravity-turns-agent-teams-into-reviewable-python-scripts-tml
title: "Google Antigravity Turns Agent Teams Into Reviewable Python Scripts — TML"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["antigravity", "agent", "google", "script", "python", "developer", "turns", "teams"]
source: https://tml.news/stories/antigravity-dynamic-workflows-python-scripts/
source_anchor: ""
source_lines: [1, 33]
section: "Gemini_Google"
---
# Google Antigravity Turns Agent Teams Into Reviewable Python Scripts

Google's Antigravity shipped Dynamic Workflows on November 13, 2026, a feature that turns a single request into a Python script coordinating a team of coding agents. A developer types /run-workflow, the agent writes the script with parallel and sequential steps, and the developer can read it before it runs. The feature is available on Google AI Pro and Google AI Ultra plans, in the Antigravity app and its command-line tool.

Edited and fact-checked by Stephen Sprague

Antigravity's bet is that a team of agents should answer to a script a person can read. The workflow is code first and conversation second, and that's the opposite of how agent tools have worked so far.

Google's Antigravity platform shipped Dynamic Workflows on November 13, 2026, and the feature changes who holds the plan. Instead of one agent working through a long conversation, a developer types /run-workflow and the agent writes a short Python script that coordinates a team of agents, with named phases, parallel and sequential steps, and structured results passed between them.

The script is the point. A workflow expresses its multi-agent steps in code a developer can read before anything runs, approve, save and run again later. Subagents return their work in JSON Schema form, so one agent's output arrives in a shape the next step can check instead of a paragraph it has to interpret. Background progress shows phases, logs and agent states while the script runs.

That design answers a known failure of multi-agent coding. In a shared conversation, instructions drift, agents repeat each other's work, and nobody can say afterward which step produced a bad change. A script fixes the control flow in advance. The coordination stops being a negotiation and becomes a program, with the trade that a badly written script fails in a structured, repeatable way.

Dynamic Workflows sit next to two features Antigravity shipped the same week, and Google lists them separately. Orchestrator, rolled out November 12, switches models inside one session. The /swarm mode spreads work across agents on a shared board. Dynamic Workflows are neither: they put the coordination into an explicit script the developer owns, which makes them the option for work that has to run the same way twice.

Access runs through Google's paid tiers. Dynamic Workflows are available with Google AI Pro and Google AI Ultra plans, in both the Antigravity app and the Antigravity command-line tool, and a workflow consumes the plan's existing model credits rather than a separate charge. Antigravity's lead, Varun Mohan, said the team is resetting everyone's Gemini quota on the platform to mark the week of shipping, with no duration stated.

The timing matters because every major coding agent is racing to solve the same coordination problem. Anthropic shipped a Claude Code update the same day, November 13, and the week's releases share a theme: the tools are moving from one assistant answering prompts to systems that plan, delegate and report. Google's version is the one that hands the developer the plan as a file.

Whether developers want to read agent plans as Python is the open question. A script is auditable and reusable, and it's also one more artifact to maintain when the underlying models change monthly. Antigravity has made its choice plain: the workflow is code, the code is reviewable, and the review is where the human stays in charge.

The documentation tells the same story as the launch posts. Antigravity's docs describe /run-workflow as a command that produces a script with explicit parallel and sequential steps, and the tutorial material runs the pattern end to end: a parallel review of changed files, a review-fix-verify loop where one reviewer keeps its notes across phases, and a parallel migration where each agent works in its own isolated Git worktree. Those three shapes cover most of what teams actually ask agent systems to do, which suggests Google designed the feature from observed use rather than from a demo.

### Sources

## The conversation.

Loading comments…

## A little more perspective.

Reading is free. Verified members can comment and reply for $2 a month.
