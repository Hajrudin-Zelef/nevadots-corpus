---
id: collect-261121-veille-ia/gemini-google/cursor-projects-multiplayer-vs-antigravity-swarm
title: "Cursor Projects Multiplayer vs Antigravity Swarm"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["cursor", "agents", "projects", "project", "docs", "multiplayer", "antigravity", "swarm"]
source: https://dev.to/aitecharchive/cursor-projects-multiplayer-vs-antigravity-swarm-5bcl
source_anchor: ""
source_lines: [1, 99]
section: "Gemini_Google"
---
Cursor Projects multiplayer, announced on 10 November 2026 in Cursor's own post, lets the coding agents of different teammates coordinate inside one shared Project. Google's Antigravity Swarm, by contrast, organises the agents working for a single developer. Our verdict: for a team on a paid Cursor plan, Cursor Projects multiplayer is the more useful of the two because it solves handoffs and overlapping work between people. For a solo developer, Swarm is the only one of the two that does anything for you, and Cursor's release adds nothing.

- Cursor Projects multiplayer is in beta and began rolling out to all Cursor users on 10 November 2026, per the announcement.
- Each teammate who joins gets their own chat, coordinator and worker agents, billed to that person.
- Members share two things only: the task board and one shared Context store.
- Messages between teammates' agents are treated as untrusted input, and the side chats that answer them are deliberately restricted.
- Antigravity Swarm works within one developer's assignment; Cursor works across teammates' assignments.
- Pilot it on one bounded workflow, such as an on-call rotation. Free and Plus users cannot use it.

Projects first launched in September 2026 as a single-engineer feature: one person directs a fleet of agents through a coordinator that plans and delegates but does not write code itself. Projects run on Cloud Agents, so closing your laptop does not stop the work, per the Projects documentation. Our piece on Cursor Live Apps and cloud agents covers the hosting side.

The November change is that you can invite teammates into the same Project. Cursor's product manager Aryaman Khandelwal framed it in the announcement: multi-agent systems have so far served one person; Projects is now built for a whole team. The forum thread "Multiplayer Cursor Projects" shows users had been asking for this, since teammates' agents could not see each other. Independent write-ups from unite.ai and ai.info match the announcement on the rollout date, the shared task board and the team-account gating.

Each person who joins gets their own chat inside the Project, with their own coordinator and worker agents. That chat runs and bills as that person, not as the Project owner, according to the docs.

What is shared is deliberately narrow:

- 
**The task board.** Everyone sees the same tasks in the Project's list and board.
- 
**Context.** One Agent Store, mounted at`/cursor/stores/self` , that every member's agents can read and write.

Your personal store at `/cursor/stores/user` stays private. Only your own agents mount it, and it holds your preferences and skills. When a teammate joins, your coordinator can offer to copy approved entries into Project rules, so shared conventions are an explicit choice rather than a leak.

The docs list real gating conditions - check these before planning a pilot.

| Requirement | Detail | Source | 
|---|---|---|
| Account type | A team account | Docs | 
| Cursor version | 3.26.13 or later for sharing a Project; Enterprise teams need 3.21.9 or later for base Projects | Docs | 
| Plan | Any plan that includes Cloud Agents: Premium and higher, Teams, Enterprise, legacy paid plans | Docs | 
| Repository access | Each invitee must have access to the Project's repository | Docs | 
| Privacy | Joining is unavailable with Privacy Mode (Legacy) | Docs | 
| Project state | Not available on a self-hosted machine or an archived Project | Docs | 

Free, Plus and legacy Hobby plans are not covered. If you are weighing plans, the comparison in Google Antigravity vs Cursor vs Claude Code is a sensible companion read.

Cursor gives two examples: an engineer adding reviews to a product page while a designer redesigns it, whose coordinators spot the overlap and sync so the reviews land in the new design; and a feature reading a database while a teammate changes the schema, where agents agree on work order up front to avoid merge conflicts.

The second is more convincing: schema-versus-reader conflicts are tedious to spot in review and costly to untangle after both branches exist.

The on-call case is specific: several Cursor teams run rotations in a shared Project, and an engineer taking over finds the coordinator already knows work in flight, unattended issues, incidents and notes needing handoff, plus channels, Linear boards and threads to watch. In-progress work can be handed over directly.

We have not run this ourselves; these are vendor-described scenarios and the feature is in beta.

Cursor's answer is to restrict what the receiving side can do. Per the docs, a message from a teammate's coordinator is labelled as agent input. The receiving agent is told which teammate's agent sent it, treats the text as untrusted, and asks before sharing that person's personal files or connector data.

The message lands in a side chat forked from your chat, named "Chat with {name}'s agent". It answers from what your chat already knows, the reply goes back to the sender, and your chat gets a note on its next turn.

| Side chat can | Side chat cannot | 
|---|---|
| Answer from your chat's context | Share secrets, credentials or tokens | 
| Send the reply back to the sender's chat | Change the task board | 
| Read the task board | Ask questions or request access | 
| Be read, stopped or archived by you | Connect source control or manage routines | 

You cannot message a side chat; you can only read, stop or archive it. This is a sound default: agent-to-agent text is an injection surface, and documenting what the responder cannot do beats a general safety promise. It does not remove the need to review what lands in shared Context, because every member's agents can write there.

The two tools sit at different layers. According to The New Stack's coverage, Swarm works within one developer's assignment, while multiplayer Projects works across teammates' assignments. Google's Swarm documentation describes the single-developer model. The same report puts Swarm in preview for Google AI Pro and Ultra subscribers, and multiplayer Projects in beta for team-account users.

|  | Cursor Projects multiplayer | Antigravity Swarm | 
|---|---|---|
| Scope | Across teammates | One developer | 
| Status | Beta, team accounts | Preview, Google AI Pro and Ultra | 
| Best for | Handoffs, overlapping work | Parallelising your own task | 
| Weak spot | Needs plan, version and repo access | No cross-person coordination | 

The verdict follows from that split. A team sharing a codebase gets more from Cursor, because the expensive failures are between people. A solo developer gets nothing from Cursor's release and should look at Swarm or at the options in our Claude Code 2.1.323 analysis. For a wider view of Cursor's direction, see our Cursor Origin review.

Context: Cursor now belongs to SpaceX, as the company announced in August 2026 — that affects model supply more than this feature, so we would not weigh it heavily in a tooling decision. The wider trend matters more: multi-agent coding is moving from personal tooling to team infrastructure, and agent identity, attribution and untrusted-input handling are now product features rather than afterthoughts.

Within our 72 winnable keywords, 20 carry a difficulty score of zero, meaning no established competitor holds the result set (measured 2026-11-12, n=72). This feature is one of those gaps, so most results today are the vendor's own pages. Treat third-party summaries with the same caution and read the docs directly.

Pick one bounded workflow and one small group.

- Choose an on-call rotation or migration handoff, where continuity matters and scope is clear.
- Confirm each invitee is on a team account, on Cursor 3.26.13 or later, and has repository access.
- Decide which conventions go into Project rules and which stay in personal stores.
- After the first week, review what agents wrote to shared Context and compare handoff time and merge conflicts against your previous rotation.

Skip it if you work alone or your plan lacks Cloud Agents. VS Code users may find our note on VS Code 1.146 Inbox mode relevant.

**Q: What is Cursor Projects multiplayer?**

**A:** A beta feature, rolling out from 10 November 2026, that lets teammates join one Cursor Project; each gets own coordinator and worker agents, while the task board and a shared Context store are common to all, per the announcement.

**Q: Who can join a shared Project?**

**A:** Teammates on a team account running Cursor 3.26.13 or later, with access to the Project's repository, as set out in the docs. Joining is unavailable with Privacy Mode (Legacy), on a self-hosted Project, or on an archived Project.

**Q: Is Cursor Projects multiplayer free?**

**A:** No. Projects need a plan that includes Cloud Agents — Premium and higher, Teams or Enterprise — and are unavailable on Free, Plus or legacy Hobby, per the docs. Each member's chat is billed to that member.

**Q: How do agents from different teammates communicate safely?**

**A:** Incoming messages are labelled with the sender and treated as untrusted input. They are answered in a restricted side chat that cannot share secrets, change the task board, ask questions or manage routines, per the docs.

## Top comments (0)
