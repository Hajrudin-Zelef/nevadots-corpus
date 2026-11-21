---
id: collect-261121-veille-ia/gemini-google/google-is-shutting-down-its-jules-coding-agent-and-folding-the-work-into-antigravity-aipopdb
title: "Google is shutting down its Jules coding agent and folding the work into Antigravity — AIPopDB"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["jules", "google", "coding", "agent", "work", "antigravity", "tasks", "shutting"]
source: https://aipopdb.com/news/google-jules-shutdown
source_anchor: ""
source_lines: [1, 23]
section: "Gemini_Google"
---
Google is shutting down Jules, its asynchronous AI coding agent, and folding the work into Antigravity, its agent-first development platform. The company announced the wind-down on 29 October 2026, in the Jules changelog and in a post on X. Sessions that are already running can be finished until 11 November 2026. After that date Jules turns read-only, and the service closes completely on 31 December 2026.

Google's announcement was brief. It calls Jules an experiment in how asynchronous coding agents can help developers ship faster, and says the lessons from it now feed several of its products. It thanks users for helping shape agentic coding. The stated plan is to bring those lessons into Antigravity, with the aim of offering autonomous remote background task execution inside an agent-first development environment, which it describes as the best of both worlds. Google's wording frames this as a move rather than an ending: the product goes away, but the idea of an agent that takes a task, works on it remotely and hands back the result is meant to live on inside Antigravity.

What Google has not said matters for anyone relying on Jules. There is no date for when Jules-style background tasks will reach Antigravity, and it is unconfirmed whether session histories, memories or scheduled tasks can move across directly. It is also unclear how the Jules task allowances that came with Google AI Pro and Ultra subscriptions will be treated once the service is gone. Anyone who built routines around Jules therefore faces two separate deadlines and an open question about what replaces it.

Jules did its work in the background on connected GitHub repositories, taking on chores such as bug fixes, tests and dependency updates so that a developer could hand a task over, look away and come back to review the outcome without an editor open. It was introduced through Google Labs and entered public beta in May 2025, running on Gemini 2.5 Pro. Google never disclosed how many people used it, saying only that thousands of developers tested it during the beta, so how large the affected group is remains unknown. TechRadar judged that it never looked especially popular, and it competed with OpenAI's Codex, Claude Code and GitHub Copilot.

Users who want to keep their work have to export it themselves before the shutdown. Finishing work in progress is possible until 11 November, which leaves a short window for anything started earlier. The export covers account settings, environment variables, scheduled tasks, connected repositories, memories and setup scripts, plus every session with its prompt, plan, activity log and code patches. It runs by installing Jules Tools through npm and pasting a prompt from the changelog into a command-line coding agent, and the output lands in a folder called jules-export in the user's home directory. In practice it is a two-part job: first the command-line tool, then the prompt that tells a coding agent what to collect, and the changelog spells out both.

A few practical points follow from the export route, all of them taken from the changelog instructions as the coverage describes them. Code, branches and pull requests already on GitHub stay there, so users should connect GitHub before exporting if they want the repository data included. The environment-variables file holds real values, so it should be kept private and secrets considered for rotation. Scheduled tasks such as nightly dependency bumps stop being useful on 11 November rather than 31 December, because Jules can no longer act once it is read-only. Anything a team still wants from its Jules sessions should therefore be pulled out well ahead of the final date, not on the last day of December.

Reaction has been mixed, and some of it comes from the small group of developers who relied on Jules daily. Some developers said they valued being able to run tasks without an open editor, and one user on X said there was now no cloud coding agent from Google and urged it to look at Claude Code on the web. The comment points at what some users will miss, which is a coding agent that runs in the cloud on its own schedule rather than inside a local editor. Another commenter joked that Google could have simply emailed the three people affected.

## About Jules

Jules is Google's asynchronous AI coding agent. It clones a connected repository into a secure Google Cloud virtual machine, then writes tests, fixes bugs and bumps dependencies in the background, returning a plan and a diff for the developer to review, so that the work can run unattended and be checked afterwards.

Google introduced it in Google Labs in December 2024 as an autonomous agent rather than a co-pilot or code-completion tool. Its public beta opened on 20 May 2025 to everyone, with no waitlist, wherever Gemini is available.

Antigravity itself is newer than Jules. It launched on 18 November 2025 alongside Gemini 3, and Antigravity 2.0, shown at Google I/O on 19 May 2026, added parallel agents and scheduled background tasks. The platform has moved quickly. Jules is not the first Google coding tool to move there: explainx's account of a Gemini CLI deprecation says the company's terminal agent was folded into Antigravity earlier.

## Comments
