---
id: collect-261121-veille-ia/cursor-ide/cursor-opens-multiplayer-projects-beta
title: "Cursor Opens Multiplayer Projects Beta"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: []
keywords: ["cursor", "context", "projects", "agents", "shared", "work", "teammate", "multiplayer"]
source: https://aiincontext.com/articles/cursor-opens-multiplayer-projects-beta-for-human-and-agent-teams
source_anchor: ""
source_lines: [1, 81]
section: "Cursor_IDE"
---
## Why it matters

- November 10, 2026: Cursor began a multiplayer Projects beta for all Cursor users.
- September 10, 2026: Cursor introduced the underlying Projects architecture for features, migrations and full applications.
- Thousands of subagents: Cursor says the original system could delegate at scale while retaining context for months.
- One-to-one chats coexist with shared context, memory and documents at the Project level.
- Several internal Cursor teams use shared Projects for on-call rotations and direct handoffs.

**By Rebecca Stone**

Cursor announced a multiplayer upgrade to Projects on November 10, 2026, and began rolling out the beta to all Cursor users.

The release turns that system for directing fleets of coding agents into a shared team workspace. According to Cursor, teams can invite colleagues into the workspace, where people and agents coordinate tasks, context and handoffs. The release moves Projects beyond its original single-user setup.

The practical stakes appear in Cursor's own examples: one group of agents may add product reviews while another redesigns the same page, and a database feature may collide with a schema change. Cursor presents shared context as a way to identify those dependencies before they produce rework.

Cursor is targeting an operational problem that appears when several people use agents on the same codebase: work can overlap, ownership can blur and context can disappear between shifts. Its examples focus on those dependencies and handoffs rather than on another standalone chat feature.

## What does multiplayer Projects change?\n

Cursor's shared task board shows what each teammate and that teammate's agents are working on. It identifies the pull request behind each task, giving developers and agents a unified, current view of active work.

- **Individual chats:** Each teammate keeps a one-to-one chat with their coordinator and worker agents. Cursor says those interactions retain local context for that teammate.
- **Team context:** Cursor's team-wide layer holds requirements documents, diagrams, technical specifications, mockups and other material. The company says the layer expands as the Project's work develops.
- **Shared work:** The workspace combines shared context, memory and documents, extending collaboration beyond task assignments and private chats.

Cursor also makes decisions recorded in one person's chat available to the Project's full agent fleet. In Cursor's model, individual chats remain in place while knowledge relevant to the wider team stays maintained at the Project level.

Invited teammates receive preconfigured, up-to-date context, according to Cursor. That removes the need for every new member to build and maintain a separate agent setup before starting work, even as a long-running Project accumulates context.

Cursor says the personal chats remain active after onboarding. A teammate can continue a direct exchange with a coordinator or worker agent while project-level material gives other agents access to the decisions that affect their assignments.

In Cursor's design, private chats preserve a teammate's direct line to the agents, while the Project layer supplies the common state required for work to move between people and agents.

## How do handovers work?\n

Cursor reports that several internal teams now run on-call rotations in shared Projects. In a recent handover, the incoming engineer's coordinator checked in and resumed from the rotation's current state rather than beginning with an empty briefing.

According to Cursor, that coordinator could see work already in flight, unattended issues, incidents and notes requiring handoff. It could also identify the channels, Linear boards and threads it needed to monitor during the next shift.

Unfinished work can transfer directly to the incoming teammate's agent. Cursor presents the handoff as a continuation of shared state, including Project-maintained context.

The handover example covers both unfinished technical work and the operational channels around it. Cursor says an incoming coordinator receives the state of active issues, incidents, notes and monitoring points together, reducing the need to reconstruct those details from scattered conversations.

## How do agents coordinate across teammates?\n

Within a shared workspace, agents belonging to different teammates can communicate, plan around one another and unblock work in real time, according to Cursor. The feature extends coordination beyond scheduling tasks for one person's agents.

Cursor gives two hypothetical examples. In the first, coordinators for an engineer adding product-page reviews and a designer redesigning that page detect the overlap early, allowing the reviews to be incorporated directly into the new design.

In the second, agents supporting a feature that reads from a database and a teammate changing its schema agree on the order of work before starting. Cursor says that sequencing is meant to prevent a later merge conflict.

In both examples, the coordinator remains the directing layer while the shared record gives each side access to the same current project state. Cursor describes the arrangement as a way to resolve dependencies before agents duplicate work or encounter conflicting changes.

The cross-teammate channel gives agents a shared point of reference before they act. Cursor says common context can settle sequencing and reveal overlapping output, allowing human coordinators to redirect work while the shared Project record remains current.

## What runs behind the shared workspace?\n

Multiplayer extends the Projects architecture that Cursor introduced on September 10, 2026, for large bodies of work such as features, migrations and complete applications. Cursor says that earlier architecture already separated coordination from code production.

In Cursor's design, a coordinator directs coding agents rather than writing the code itself. The company says delegation keeps the coordinator responsive to user direction while the agents perform the assigned work.

Projects run on cloud computers by default, according to Cursor, so work can continue after a laptop closes. When something must be tested on the user's own machine, the coordinator can start a local agent for that task.

Project files synchronize across the cloud and local machines used by the agents. Cursor says this preserves research, artifacts and knowledge about both the codebase and the team's work preferences as execution moves between environments.

Subscriptions give a coordinator several recurring triggers. It can watch a Slack channel, run on a schedule or follow pull requests, including fixing continuous-integration failures and acting when a pull request opens or merges.

Those subscriptions make the coordinator an active participant in repository events. Cursor says the coordinator can respond when a pull request opens, when it merges or when continuous integration fails, without waiting for a new instruction for each event.

Cursor described the original system as able to maintain context over months, delegate to thousands of subagents and perform recurring work without repeated prompts. The multiplayer update places that persistent context in a workspace shared by invited teammates and their agents.

The distinction matters because Cursor positions Projects as a system for supervising long-running bodies of work. A coordinator can retain direction over agents that run for months, recur on schedules or continue after a user's device disconnects.

## How does a team start?\n

A user creates a Project from the left-hand navigation and invites teammates, according to Cursor. The beta concentrates onboarding, handovers and coordination of overlapping work inside the shared project record.

Cursor says the record must remain current as people and agents contribute. For teams evaluating the beta, that continuity will determine whether shared coordination reduces reconstruction work or simply adds another place to maintain project state.

Source: unite.ai
