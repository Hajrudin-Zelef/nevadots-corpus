---
id: collect-261121-veille-ia/cursor-ide/cursor-projects-goes-multiplayer-your-whole-team-and-all-its-agents-in-one-shared-workspace
title: "Cursor Projects Goes Multiplayer: Your Whole Team — and All Its Agents — in One Shared Workspace"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: []
keywords: ["agents", "team", "shared", "projects", "cursor", "work", "context", "multiplayer"]
source: https://dev.to/yehechuhai/cursor-projects-goes-multiplayer-your-whole-team-and-all-its-agents-in-one-shared-workspace-141b
source_anchor: ""
source_lines: [1, 42]
section: "Cursor_IDE"
---
In September, Cursor introduced Projects: a way for engineers to take on large-scale work by directing a fleet of agents through a single coordinator. One person, many agents, one big job.

On November 10, Projects got its biggest upgrade yet — it's now multiplayer. Invite your teammates into a Project, and you plan, build, and ship together, alongside all of your agents. As Cursor puts it, multi-agent systems have so far served one person. Projects is a multi-agent system built for a whole team.

That sounds like a small step — adding people to something that already worked. It isn't. The moment agents stop being personal assistants and start being team members, three hard problems show up: who is doing what, whose context is current, and what happens when two agents step on each other. The multiplayer upgrade is really an answer to all three.

There is no shortage of software for team collaboration. But, as Cursor points out, there has been none for coding agents — until now. Projects is the first workspace where a software team works side by side with all of its agents, with context, memory, and documents shared.

The centerpiece is a shared task board. It shows who's doing what, what everyone's agents are working on, and the pull request behind each task. Humans and agents get the same unified, up-to-date view. That matters more than it sounds: in a single-user agent setup, the "state of the project" lives in one person's head and one person's chat history. In a multiplayer Project, it lives in the workspace itself, where anyone — human or agent — can read it.

Today, agent context is siloed per person. Every new team member has to build and maintain their own context before their agents can do useful work — re-explaining the codebase, the conventions, the decisions that were already made somewhere else.

Multiplayer Projects keeps the personal layer and adds a shared one. Each teammate still gets their own one-to-one chat with their coordinator and worker agents, each with its own local context. On top of that, Projects adds a shared context system that holds requirements documents, diagrams, technical specs, mockups, and more. The shared layer is built once and grows with the team's work. A decision made in one chat is accessible to every agent in the Project.

The practical payoff is onboarding. When a new teammate is invited into a Project, the context is already pre-configured and up to date. Nobody has to rebuild it from scratch.

Cursor's own teams already use this for on-call rotations, and the example is telling. When one engineer recently took over a rotation from a teammate, their coordinator picked up right where things stood — after checking in first. It already knew what work was in flight, which issues were still unattended, which incidents and notes needed handing off, and which channels, Linear boards, and threads to keep an eye on. In-progress work can even be handed over directly: the new person's agent simply takes it on.

That is the difference between an agent that helps you and an agent that works on your team. The first one starts fresh every time. The second one inherits the state of the work.

A shared workspace and shared context open up something that wasn't possible before: agent-to-agent communication across teammates. Your agents can plan around each other and unblock each other in real time, the way people do.

Cursor gives two concrete examples, and both are about the collisions that normally only show up too late:

- An engineer is adding reviews to the product page while a designer is redesigning that same page. Their coordinators spot the overlap early and sync up, so the reviews are built straight into the new design — no refactor down the road.
- Your feature reads from a database while your teammate is changing its schema. The agents agree on the order of work up front, so neither of you hits a merge conflict at the end.

In both cases, the coordination happens before the work is wasted, not after. That is the whole point of putting agents in a shared space: conflicts that used to be discovered in code review, or in a broken merge, get resolved while the work is still in progress.

The single-user agent era had a clear ceiling: one person could move fast, but the team couldn't move together. Every handoff — between people, between an agent and a person, between one person's agents and another's — was a place where context leaked and work duplicated.

Multiplayer Projects attacks exactly those handoffs. A shared task board makes the state of work visible to everyone. A shared context layer means decisions don't have to be re-explained. Agent-to-agent communication means collisions get caught early instead of in review.

None of this removes the human from the loop. Teammates still direct their own coordinators, still review the work, still make the calls. What changes is that the agents stop being isolated helpers attached to individual developers and start behaving like members of one team — sharing the same picture of the work, the same context, and the same conversations about what to do next.

Multiplayer Projects is rolling out in beta to all Cursor users starting today. To try it, create a Project from the left-hand navigation, then invite your teammates to join.

The question worth watching now is not whether teams will try it — they will — but how the dynamics change once agents are shared resources instead of personal ones. Who owns a decision an agent made in a shared context? How does a team review work when three coordinators planned it together? Those are the questions multiplayer agent systems will have to answer next. For now, the first one is answered: the team and its agents finally share a room.

*Source: Cursor — "Cursor Projects is now multiplayer" (November 10, 2026).*

## Top comments (0)
