---
id: collect-261121-veille-ia/gemini-google/google-s-jules-shuts-down-december-31-7-days-left-to-export-your-coding-agent-s-memories-and-setup
title: "Google's Jules shuts down December 31: 7 days left to export your coding agent's memories and setup"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["jules", "agent", "export", "google", "coding", "prompt", "down", "december"]
source: https://dev.to/piekwerk/googles-jules-shuts-down-december-31-7-days-left-to-export-your-coding-agents-memories-and-setup-5oi
source_anchor: ""
source_lines: [1, 56]
section: "Gemini_Google"
---
Google is shutting down Jules, its asynchronous coding agent, on December 31, 2026. The official notice landed on October 29, and it carries a tighter deadline that most coverage buried: you have until November 11 to finish running sessions. After that date, Jules turns read-only until the end of the year, and then it is gone. If you ever pointed Jules at a real repository, the next seven days are the window to move its state out.

I went through the turndown notice and the export tooling. The mechanics matter beyond Jules, because the export list is a clean inventory of what a hosted coding agent actually holds about your work, and what it holds nothing of.

The first date is November 11, 2026. Until then, Jules works normally and you can finish sessions in progress. On November 11 the service goes read-only. You can still look at things, but nothing new runs.

The second date is December 31, 2026, when the service winds down entirely. Google's stated reason is consolidation: the notice describes the move "as part of our work to unify our agentic coding efforts," with the learnings "shaping products across Google, including our agent-first platform, Google Antigravity." The Jules homepage now opens with the same two dates and a pointer to Antigravity, and Google confirmed the direction in an X post covered by TechRadar.

What does not exist yet is a feature-by-feature migration plan from Jules to Antigravity. The announcement describes a direction, not a replacement. If you are waiting for one, the read-only date does not wait with you.

The export is more thorough than I expected. Per the notice, it covers everything Jules knows about your work:

- account settings
- environment variables
- scheduled tasks
- connected repositories
- memories
- setup scripts
- every session, including its prompt, plan, full activity log, and code patches

Read that list with a config engineer's eye. Settings, environment variables, scheduled tasks, and setup scripts are the agent's operating configuration. Memories are its accumulated context about your project. Sessions are the history. That is the complete stack of state a coding agent keeps about you, and every item on the list lives inside Google's infrastructure, not yours.

One thing survives without any export at all: your code, your branches, and your pull requests. They are already in GitHub and they stay there. The shutdown splits your work into two layers. The repository layer is portable by construction. The agent layer is the part that dies with the vendor, and it is the only part that needs a migration path.

Google ships the export through Jules Tools, a CLI package. From the changelog:


```
npm install -g @google/jules
jules login
```
That part is ordinary. What follows is not. The official instruction is to open a CLI coding agent, "such as Claude Code," from your home directory and paste the export prompt that Google publishes in the same changelog entry. The prompt tells your local agent to check whether the Jules CLI is installed, install it if not, run the export, and tell you what it saved.

Two practical notes. Run it from your home directory as instructed, so the output lands where the prompt expects. And read the export prompt before you paste it. It is a plain prompt, visible in the changelog, and you should know what your agent is about to do on your behalf.

Sit with that design choice for a second. Google did not build a one-button dashboard export for the turndown. It wrote a prompt, and delegated the work to whatever coding agent you run locally.

There is something honest in it. The export task is exactly the kind of multi-step choreography agents are decent at now: check a dependency, walk an API, write files, summarize. But it also means the final copy of your Jules state gets produced by an agent reading a prompt, so treat the output the way you treat any agent output. Spot-check that the session folders contain what the summary claims, and keep the export folder somewhere versioned or backed up before you consider it safe.

Jules ran for under two years as an experiment in asynchronous coding agents, the kind that work through your backlog while you are away. Consolidating it into Antigravity is a rational end for an experiment, and the export tooling shows Google did try to not strand people.

The cost pattern is consistent though. The code outlives the agent. The context does not. When a hosted agent retires, you lose the layer that made it useful on your repository: the memory of what you asked before, the scheduled tasks, the environment setup. That layer is invisible while it works, and it is exactly the layer with an expiry date. We keep relearning this in different forms; when we looked at deprecated MCP servers recently, agents were still choosing them in 98% of tracked sessions long after the deprecation, because the knowledge of what changed lived outside the repo 1.

Run the Jules export this week regardless. The stronger move is making the next one trivial. Three habits that hold up across harnesses:

**Keep agent instructions in the repository, as files.** An `AGENTS.md` at the root, a `CLAUDE.md` for Claude Code, rules files for Cursor. Any harness can read a file inside the repo, and the repo survives every vendor. When Cursor shipped its Agent SDK, the agent definition itself was a folder of Markdown and TypeScript you keep in version control 2.

**Keep scheduled work in your own scheduler.** A cron entry or a GitHub Actions workflow that calls an agent CLI is yours. A scheduled task that exists only in a vendor dashboard has the vendor's lifetime. Export it to a file now, rebuild it in your own tooling, and the next shutdown becomes a non-event.

**Keep secrets in your own secret store, referenced by name.** Environment variables configured inside a hosted agent's dashboard are the easiest thing to forget during a migration and the worst thing to lose track of afterward.

The common thread: anything that lives only inside a hosted tool should have a copy somewhere you control. Your repository already does this for code. Doing it for agent state is the same instinct applied one layer up.

November 11: last day Jules runs. December 31: the service is gone. Between now and then, `npm install -g @google/jules`, `jules login`, paste the export prompt into your local agent, and check the output. Then look at where your current agent's instructions, schedules, and variables live, and ask which of those would need an export if the vendor posted its own turndown notice tomorrow.

## Top comments (0)
