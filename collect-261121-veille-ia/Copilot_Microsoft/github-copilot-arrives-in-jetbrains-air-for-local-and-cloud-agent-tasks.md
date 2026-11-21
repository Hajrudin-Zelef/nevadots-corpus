---
id: collect-261121-veille-ia/copilot-microsoft/github-copilot-arrives-in-jetbrains-air-for-local-and-cloud-agent-tasks
title: "GitHub Copilot Arrives in JetBrains Air for Local and Cloud Agent Tasks"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["copilot", "jetbrains", "github", "agent", "cloud", "tasks", "environment", "runs"]
source: https://vinish.dev/github-copilot-jetbrains-air-agent-integration
source_anchor: ""
source_lines: [1, 67]
section: "Copilot_Microsoft"
---
GitHub said on November 3, 2026 that GitHub Copilot can now be picked as a coding agent inside JetBrains Air, the agentic development environment from JetBrains. Developers can hand work to Copilot, steer it while it runs and check the resulting changes, either on their own machine through a JetBrains IDE or in a remote cloud sandbox.

The news matters for teams that already pay for Copilot but write code in IntelliJ IDEA and other JetBrains IDEs. Cloud runs started from Air are billed against the Copilot plan, not against JetBrains AI credits, so organizations do not need a second AI budget to try agent-driven workflows in Air.

## What GitHub Announced

The integration is built on the Agent Client Protocol, an open protocol that lets an editor talk to different AI agents through one interface. In Air, Copilot now shows up as an option in the agent picker, next to the other agents the environment supports.

GitHub lists three main capabilities in this release:

- Selecting Copilot in the Air agent picker and signing in with an existing Copilot subscription.
- Running several agent sessions side by side, with access to project context and IDE tooling, and reviewing the edits next to the code.
- Handing tasks to separate cloud sandboxes via Air Teams, started from the IDE or from a browser.

This option sits alongside the existing GitHub Copilot plugin for JetBrains IDEs rather than replacing it. GitHub says developers can use both and pick whichever workflow suits the job at hand.

## How Cloud Tasks Work

When a Copilot task runs in the cloud through Air, it executes inside a Docker container on a newly created virtual machine. Network traffic is routed through a proxy, and nothing is kept from one task to the next. The agent can compile code, run tests and install packages without touching the developer's laptop.

JetBrains described the thinking behind this setup in a recent post on agent sandboxing. It compares local runs, Git worktrees and remote options, and argues that a fresh remote environment for each task gives the strongest isolation by default, because the agent never shares a file system with the developer's own machine.

Cloud tasks can be launched in two ways. In the browser, a user opens Air, links GitHub under Settings and VCS Providers, picks a repository and remote branch, selects Copilot and enters a prompt. In the IDE, Air offers a Cloud choice in its environment selector.

## Requirements and Limits

Local tasks only need Air installed in a JetBrains IDE and a connected Copilot account. Cloud tasks have stricter conditions. The user needs a JetBrains account that belongs to a business organization with Air Teams turned on, an active Copilot subscription, and an Air administrator who has allowed Copilot.

GitHub also points out several gaps that admins should know about before a rollout:

- Connecting the two products does not ask for Copilot permissions up front. Air may only check the subscription or credit balance when a cloud environment starts, so a missing plan shows up as a failed task with an error in the session.
- A user cannot choose whether a given task runs on a personal Copilot plan or on the organization plan.
- The governance and analytics views in Air do not include Copilot usage.
- Air admins can switch Copilot off through a governance policy, but they cannot set Copilot spending caps from inside Air.

## What It Means for Developers

For JetBrains users, this removes one of the main reasons to leave the IDE for agent work. You can keep your usual project, inspections and tool windows open, while Copilot handles a refactor or a test fix in a sandbox and you review the diff in familiar panes.

The billing model is the practical win. Companies that have standardized on Copilot licenses can now use them in Air without buying extra JetBrains AI capacity. On the other hand, the missing usage data in Air means cost tracking still has to happen on the GitHub side, so finance and platform teams should watch Copilot reports closely during a pilot.

The sandbox design also matters for anyone working with real data.

A cloud task cannot reach your local database connections or files, which is safer. It also means tests that depend on a local Oracle or PostgreSQL instance will need a container or a mocked setup in the repository. Plan for that before handing Copilot work on the data layer.

Because the link uses the Agent Client Protocol, the same pattern may spread to other editors that adopt ACP. Teams evaluating agent tools should watch that standard, since it reduces lock-in.

If you rely on JetBrains IDEs, our report on JetBrains fixes for compromised eMudhra certificates covers another update worth installing, and our piece on how AI helps developers write secure code gives useful context on reviewing agent output.

## How to Get Started

For local use, add Air to your JetBrains IDE, open the agent picker, choose GitHub Copilot and sign in with your GitHub account. You can then start a task against the open project and review the proposed changes when it completes.

For cloud use, ask your Air administrator to confirm that Air Teams is active and that Copilot is allowed by policy. After that, start a task from the browser version of Air or choose Cloud in the IDE plugin. GitHub is collecting feedback through in-product forms, its feedback repository and a short survey.

## Conclusion

GitHub Copilot is now a selectable agent in JetBrains Air, with local runs in the IDE and isolated cloud runs paid from the Copilot plan. The feature is useful for JetBrains shops already invested in Copilot, but the missing usage reporting and spending controls inside Air mean admins should keep governance on the GitHub side for now.

## Sources

- GitHub Changelog: GitHub Copilot is now available in JetBrains Air
- JetBrains Blog: How Cloud-Based Agent Sandboxing Works in Air

**Get more Vinish.dev tutorials on Google**

See my articles more often when you search Google.
