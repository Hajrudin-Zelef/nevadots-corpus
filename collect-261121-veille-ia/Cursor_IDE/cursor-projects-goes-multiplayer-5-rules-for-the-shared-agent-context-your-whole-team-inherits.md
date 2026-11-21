---
id: collect-261121-veille-ia/cursor-ide/cursor-projects-goes-multiplayer-5-rules-for-the-shared-agent-context-your-whole-team-inherits
title: "Cursor Projects goes multiplayer: 5 rules for the shared agent context your whole team inherits"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: ["2026-11-11"]
keywords: ["agent", "shared", "context", "team", "cursor", "agents", "every", "rules"]
source: http://dev.to/piekwerk/cursor-projects-goes-multiplayer-5-rules-for-the-shared-agent-context-your-whole-team-inherits-16o8
source_anchor: ""
source_lines: [1, 59]
section: "Cursor_IDE"
---
Cursor shipped a significant one yesterday. Projects, the September feature that let a single engineer direct a fleet of agents through one coordinator, is now multiplayer. Invite your teammates into a Project and you plan, build, and ship in one workspace, humans and agents together. It rolled out in beta to all Cursor users on November 10.

I spend my days on agent config files, so the part that caught my eye was not the multiplayer framing. It was this: a shared context system now sits on top of each person's private agent chats, and a decision made in one chat is accessible to every agent in the Project. That is a new config surface. It behaves like a rules file with a much bigger blast radius, and most teams will meet it unprepared.

Three things, per the announcement:

1. A shared workspace where each teammate keeps a one-to-one chat with their own coordinator and worker agents, with its own local context.
2. A shared context layer on top, holding requirements docs, diagrams, tech specs, and mockups. It is built once and grows with the team's work.
3. A shared task board showing who is doing what, what everyone's agents are working on, and the pull request behind each task.

Cursor's own example is an on-call rotation. An engineer took over a rotation from a teammate, and their coordinator picked up mid-flight after checking in first. It already knew the work in progress, the unattended issues, and which Linear boards and threads to watch. In-progress work can be handed over directly, because the new person's agent inherits the same context.

Everything painful about agent config files applies here, multiplied by headcount. When a rules file drifts in one repo, one repo misbehaves. When a shared context doc drifts, every teammate's agents act on stale information at the same time. A decision recorded casually in one chat becomes team law the moment it lands in the shared layer.

The flip side is real too. Today, every new team member rebuilds the same context by hand before their agent is useful. A shared layer that is actually maintained removes that tax. The question is who maintains it, and under what review. That is a config governance question, not a Cursor question.

The agent-to-agent communication story is the most interesting and least supported part. The post gives two worked examples: coordinators spotting a design overlap before code gets written, and agents agreeing on an order of work when one feature reads a database table that a teammate is refactoring, so nobody hits a merge conflict at the end.

Those are best-case scenarios, and Cursor publishes no numbers on how often coordinators actually catch the overlap. Coverage of the launch raised the same gap. Treat coordinator-to-coordinator negotiation as demo-grade until your own team has watched it work or fail on real work. The mechanism is plausible. The reliability is unmeasured.

**1. Version the shared layer like code.** If edits to shared docs can land without a review, you have built an unreviewed config channel into every agent on the team. Put shared-layer changes through pull requests, or at minimum a named reviewer, the same way you treat your AGENTS.md today.

**2. Keep personal rules personal.** The one-to-one agent chats still exist, and that boundary matters. Preferences, experiments, and half-formed ideas belong in your own chat, never in the shared layer. Shared means team truth only.

**3. Name an owner for every document.** Unowned docs rot. Shared-layer rot is worse, because every agent reads it. An owner and a last-reviewed date at the top of each doc makes rot visible instead of ambient.

**4. Let the task board be your audit trail.** Tasks link to PRs, which means the board is where you reconstruct what an agent did and why. When something odd ships, start there, not in the agent's chat history.

**5. Write down what agents may say to each other.** Agent-to-agent coordination is agent output. A short protocol in the shared layer (when to sync, when to wait, when to escalate to a human) beats letting two coordinators improvise.

Shape matters. This skeleton has kept team config readable for us across projects:


```
# Shared agent context
Owner: ana | Last reviewed: 2026-11-11 | Scope: this Project only
## Decisions
- 2026-11-10: sessions move to the platform session table (ADR-14)
## In flight
- Checkout rewrite, PR #482, owner ana
## Ground rules for agents
- Ask before editing anything under db/migrations
- Sync with the other coordinator before touching the schema
```
Short, dated, owned. If a section grows past a screen, it probably wants its own doc with its own owner.

Teams ask me a version of this weekly, usually as "CLAUDE.md or AGENTS.md for teams, and who owns which file?" The multiplayer shared layer adds a third home, and the split is clean when you draw it by blast radius:

- Repo config (AGENTS.md, CLAUDE.md, rules files): how agents work in this codebase. Lives in the repo, reviewed in PRs.
- Personal chat context: how you work. Dies with your preferences, harms nobody else.
- Shared layer: what the team decided and what is in flight. Propagates to every agent, so it needs the strictest review of the three.

Config drift between repos was already a solved-by-discipline problem. Multiplayer Projects adds drift between teammates, and the discipline is the same: one owner per file, dated decisions, and a review path for anything every agent will read. We cover the repo side of that in managing agent config across multiple repositories and the standardization story in team-standardizing AI coding agents.

Two limits worth holding onto. First, agreeing an order of work up front only helps when the coordinators actually spot the overlap, and that detection is exactly the unmeasured part. Second, a new teammate whose context arrives pre-configured inherits assumptions they never reviewed. That is a fast start and an unaudited one. Pair every Project invite with ten minutes of reading the shared layer end to end. The onboarding shortcut only stays safe if somebody actually reads what the agents now believe.

If you already version your agent configs, multiplayer Projects is a natural extension and the beta is worth trying on one rotation or one feature stream. If your team has never reviewed a rules file in its life, fix that first, because you are about to share it with everyone.

## Top comments (0)
