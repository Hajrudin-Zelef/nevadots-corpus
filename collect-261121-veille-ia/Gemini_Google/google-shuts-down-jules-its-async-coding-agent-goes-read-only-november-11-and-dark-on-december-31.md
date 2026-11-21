---
id: collect-261121-veille-ia/gemini-google/google-shuts-down-jules-its-async-coding-agent-goes-read-only-november-11-and-dark-on-december-31
title: "Google Shuts Down Jules: Its Async Coding Agent Goes Read-Only November 11 and Dark on December 31"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "jules", "coding", "export", "agent", "december", "november", "down"]
source: https://www.aireport.net/Gn_c814d4116eff
source_anchor: ""
source_lines: [1, 18]
section: "Gemini_Google"
---
Google is shutting down Jules, the asynchronous coding agent it first introduced in Google Labs in December 2024. A changelog entry titled "Jules Turndown," posted on the product's official documentation on October 29, says Jules "will wind down on December 31, 2026" as part of Google's "work to unify our agentic coding efforts." Users have until November 11 to keep finishing sessions; after that, the product turns read-only until the end of the year.

The timeline gives Jules users roughly two distinct windows to plan around. Between now and November 11, developers can finish open work, merge or close the pull requests Jules opened, and export everything they care about. From November 11 to December 31, the agent still exists but accepts no new work — which matters most for anyone running scheduled tasks like nightly dependency bumps or weekly test-coverage jobs, since those silently stop being useful on November 11, not New Year's Eve. Anything not exported by the end of the year should be assumed unrecoverable.

Google is at least making the exit orderly. The changelog ships an export tool built around the Jules CLI: install it, sign in, then paste a provided export prompt into a command-line coding agent such as Claude Code, which checks the setup, runs the export script and reports what it saved. The export lands in a local folder covering account settings, environment variables, scheduled tasks, connected repositories, memories, setup scripts and every session — prompt, plan, full activity log and code patches included. Code itself needs no export, Google notes, because branches and pull requests already live in GitHub. One warning is spelled out: the export contains real secret values in its environment-variable file, so it should stay out of any repository.

Jules had a short but formative run. Google unveiled it in December 2024 as an experiment in asynchronous development: hand it a task like fixing a bug or writing tests, and it clones the repo into a Google Cloud virtual machine, edits the code and opens a PR with its plan, reasoning and a diff. It entered public beta in May 2025 and graduated in August of that year, by which point Google said thousands of developers had completed tens of thousands of tasks and publicly shared more than 140,000 code improvements. The shutdown notice now speaks of it in the past tense: "When we launched Jules, it was an experiment to learn how asynchronous coding agents could help you tackle your backlog and ship faster."

The stated destination is Antigravity, Google's agent-first development platform. The official Jules account said the team is bringing its learnings into Antigravity so users get "the best of both worlds: autonomous remote background task execution and an agent-first development environment." The same day, Antigravity launched a plugin marketplace — a directory of Google-built and third-party plugins bundling skills, MCP servers, rules, hooks and custom agents, with launch partners including Atlassian, Canva and CrowdStrike. What Google has not given is a date for Jules-style background tasks inside Antigravity, any promise of automatic migration for sessions and memories, or an answer on how AI Pro and Ultra task allowances will be treated.

The shutdown is a clean read on where the coding-agent market stands. Async background agents were 2024's experiment; agent-first platforms with plugin ecosystems are the 2026 product. Jules' learnings are not being discarded — they are being consolidated into a platform that can monetize them — but for developers, the lesson is the usual one for Google-adjacent tooling: the experiment phase is a subscription to nothing. Six weeks is enough time to export. It is also enough time to notice that every surviving coding agent now has a migration story pointed at itself.

## Comments (0)

Log in to join the discussion

Log In
No comments yet
