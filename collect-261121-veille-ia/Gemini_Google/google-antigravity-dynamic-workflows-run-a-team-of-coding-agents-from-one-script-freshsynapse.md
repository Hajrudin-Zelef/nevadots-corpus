---
id: collect-261121-veille-ia/gemini-google/google-antigravity-dynamic-workflows-run-a-team-of-coding-agents-from-one-script-freshsynapse
title: "Google Antigravity Dynamic Workflows: Run a Team of Coding Agents From One Script | FreshSynapse"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-16"]
keywords: ["antigravity", "workflows", "agents", "freshsynapse", "google", "dynamic", "script", "agent"]
source: https://freshsynapse.com/articles/antigravity-dynamic-workflows-coding-agents-2026-11-16/
source_anchor: ""
source_lines: [1, 16]
section: "Gemini_Google"
---
*Photo: Hippopx (public domain). Illustrative image.*

**What happened.** Google shipped Dynamic Workflows in Antigravity v2.32.0 on 13 November 2026, a feature that turns multi-agent coding from a conversation into a script. The centrepiece is the `/run-workflow` command: instead of asking one agent to do a big job, you describe the job and the agent writes a short Python script that coordinates many agents — running some in parallel, others in sequence — and you approve that script before anything runs.

**How it works.** The script uses `parallel()` and `pipeline()` phases to organise the work: independent tasks run side by side, dependent ones wait their turn. Each agent returns a JSON-Schema-structured result, so the outputs are machine-readable and the workflow can check them, pass them on, or branch on them. The key design choice is the approval step. Multi-agent systems are powerful and unpredictable in equal measure; making the coordination plan a readable script that a human signs off on is Google’s answer to the control problem — the plan is inspectable before the agents start spending tokens and touching files.

**What it costs and who gets it.** Dynamic Workflows is available in the Antigravity App and CLI, on Google AI Pro and Ultra plans only. There is no separate charge: workflows consume the plan’s existing model credits. It runs on Gemini 4 Carbon, Argon and Hydrogen. It is distinct from two neighbouring features: Orchestrator, which arrived on 12 November and switches models mid-session, and `/swarm`, which puts agents on a shared board. Dynamic Workflows is the scripted, repeatable one — closer to a build pipeline than a brainstorm. Antigravity lead Varun Mohan also announced a Gemini quota reset the same week, though its duration was not stated.

**Why it matters.** Coding agents are splitting into two philosophies: the single agent you chat with, and the team of agents you orchestrate. Dynamic Workflows is Google’s bet that the second model wins for real projects — but only if the orchestration itself is transparent and reviewable. The same week, GitHub announced it will retire six Copilot models on 14 December, a reminder that the model underneath any of these tools changes constantly. Our three-way comparison of Antigravity vs GitHub Copilot vs Claude Code weighs the three against each other, and What Is an AI Agent? explains the category in plain English.

## Sources

- Google — “Dynamic Workflows”, Antigravity documentation: https://antigravity.google/docs/dynamic-workflows/
- FreshSynapse — “GitHub Copilot Is Retiring 6 AI Models on December 14”: https://freshsynapse.com/articles/github-copilot-retires-six-models-december-14-2026-11-16/
- FreshSynapse — “Google Antigravity vs GitHub Copilot vs Claude Code: Which AI Coding Agent Should You Pick?”: https://freshsynapse.com/articles/antigravity-vs-copilot-vs-claude-code-2026-11-16/
- FreshSynapse — “What Is an AI Agent? A Plain-English Guide to How AI Agents Work”: https://freshsynapse.com/articles/what-is-an-ai-agent/
