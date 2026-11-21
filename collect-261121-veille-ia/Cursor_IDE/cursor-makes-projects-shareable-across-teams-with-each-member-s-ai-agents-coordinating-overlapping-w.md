---
id: collect-261121-veille-ia/cursor-ide/cursor-makes-projects-shareable-across-teams-with-each-member-s-ai-agents-coordinating-overlapping-w
title: "Cursor makes 'Projects' shareable across teams, with each member's AI agents coordinating overlapping work; team plan required"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: []
keywords: ["agents", "work", "projects", "each", "team", "member", "cursor", "feature"]
source: https://ai-oriens.com/en/news/cursor-projects-multiplayer/
source_anchor: ""
source_lines: [1, 27]
section: "Cursor_IDE"
---
## A team's agents in one workspace

AI coding tool maker Cursor on Nov. 10 (Nov. 11 Japan time) announced that it has rebuilt "Projects," a feature that coordinates multiple AI agents to carry out large development work, so team members can use it together. Members and their agents gather in one workspace, and the agents coordinate one another's work. The feature is in beta, and the company said it is rolling out gradually to all users starting the day of the announcement.

Projects launched in beta on Sept. 10; users give a goal to a "coordinator" agent, which splits the work and assigns it to many agents. The coordinator does not write code itself and runs in the cloud, so work continues even if the user closes their computer. Until now, it was used by a single user directing their own agents.

## Shared task board and materials, individual chats

The new Projects has a shared task board. It shows who is responsible for what, what each member's agents are working on and which pull requests (proposed code changes) are behind each task.

Each member has their own one-on-one chat with their coordinator and worker agents. Separately, there is a shared layer holding requirements documents, diagrams, technical specifications and interface prototypes, and the company said decisions made in one chat are passed on to all agents in the Project. New members can start work from context that has already been set up.

## Agents coordinate overlaps and ordering with each other

According to the company, agents belonging to different members detect overlapping work and coordinate it. The blog gave examples of aligning a review feature built by an engineer with a page redesign led by a designer at an early stage, and of agents deciding the order of work when a change to a data structure (schema) affects another feature.

The company said it also uses the feature internally for on-call handoffs. The coordinator of the member taking over picks up the previous person's work in progress.

However, the company has not given figures on how often the coordination detects overlaps and dependencies, or how many misses or excessive warnings there are. AlphaSignal noted that how disagreements between coordinators are resolved, which decisions go into shared memory, and how permissions over changes to materials and agents are managed have also not been disclosed. Coordination is limited to agents within Cursor's Projects, and no mechanism for connecting with agents in other companies' products has been presented.

## Sharing requires a team plan; Free and Plus excluded

The blog promises availability for "all users," but the documentation limits who can use it. Projects is available on plans that include agents running in the cloud: Premium and above, Teams, Enterprise and legacy paid plans. It is not included in the Free, Plus or legacy Hobby plans.

Sharing a Project with other members requires a team account and Cursor 3.26.13 or later. Sharing is limited to the same Cursor team, and only invited people can join. Users with individual accounts and people on other teams cannot participate, and participants need access to the repositories (code storage) the Project works with. Users of the legacy privacy setting "Privacy Mode (Legacy)" cannot join, nor can Projects running on a company's own machines.

Costs are charged to each member. According to the documentation, each member's chat runs as that person and is billed to that person. Neither the blog nor the documentation states pricing for the sharing feature itself, the number of people who can join a single Project or the limit on how many agents can run at the same time.
