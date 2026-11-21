---
id: collect-261121-veille-ia/cursor-ide/cursor-changelog-and-release-notes-nov-2026
title: "Cursor Changelog and Release Notes (Nov 2026)"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: []
keywords: ["cursor", "agents", "releases", "build", "product", "work", "apps", "cloud"]
source: https://thismonthinai.com/updates/cursor
source_anchor: ""
source_lines: [1, 238]
section: "Cursor_IDE"
---
## November 2026 at a glance

Cursor released the Origin App Marketplace, enabling integrations with over 20 apps; Cloud Agent Canvases and Environment API improvements; the Agent SDK for deploying agents; and iOS remote control for local agents.

Summary of October 2026, generated from the releases listed below.

This month

2

-2 vs last month

Last 12 months

68

Busiest: May 2026

Most shipped

Product

47 releases

So far, November is Cursor's 11th busiest month of the last 12, below its monthly average of 5.7. All of them are Product. Among AI coding tools, only OpenCode and Windsurf have shipped more this month.

## Key Cursor launches

New models and major product launches from the last 90 days. The full log follows.

- · Product You can now talk with your agents in a realtime voice call.
- · Product Agents have lowered the cost of writing code, but most apps and tools that users build never make it off of localhost.
- · Product Cloud agents can now build interactive canvases and manage environments with the Environments API, supporting up to 300 repositories and faster loading.

## Latest Cursor releases

Newest first. Each entry links to the official page it came from.

### November 20262 releases

Agents have lowered the cost of writing code, but most apps and tools that users build never make it off of localhost. Now, you can build live apps directly in Cursor and share them with a link. Describe what you want to create and Cursor will handle the rest: build the app, set…

### October 20264 releases

Cloud agents can now build interactive canvases and manage environments with the Environments API, supporting up to 300 repositories and faster loading.

Origin now integrates with over 20 apps for deployments, CI, code review, security, monitoring, and project management, from partners including Cloudflare, Linear, Sentry, and Socket Security. Starting today, the marketplace is also open to public submissions, so any team buildi…

### September 20264 releases

Projects lets you take on larger bodies of work, such as a feature, a migration, or a full app. It maintains context over months of work, delegates tasks to thousands of subagents, and performs recurring work without being prompted.

Cursor supports self-hosted machines, which let you keep tool execution entirely in your own network. Your codebase, build outputs, and secrets all stay on internal machines running in your infrastructure, while the agent handles tool calls locally.

### August 20266 releases

Cloud Agents no longer require a connected GitHub or other third-party SCM provider to get started. Prompt from the get-go, then save your work to a Cursor Origin repo.

Cloud agents can automatically pick up work in response to events, hold a goal until it's met, and stay on course through long-running sessions. Subscriptions, custom modes, subagents on their own machines, /goal command, and steering improvements added.

Cursor can now host your code. Origin begins rolling out today in early beta on all paid plans. Features include repos, pull requests, code browsing, GitHub sync, and agent-native features.

### July 20265 releases

Cursor for iPad is now available on all paid plans. New to both iPhone and iPad: an inbox to stay organized, and a review experience that covers the full PR. Create, review, and merge from anywhere. # Built for the bigger screen The iPad layout is rebuilt around the extra space.…

Cursor Router is introduced as an intelligent model router that analyzes requests and sends them to the appropriate model. Features include optimization modes (Intelligence, Balance, Cost) and admin controls for team management.

Cursor in Slack now shares a plan before it starts, runs in multi-repo environments, and can work across channels and threads. # Interaction improvements Cursor now responds with a plan before it begins, so you can jump in and redirect early. As it works, it updates its status s…

### June 202610 releases

Expanded team marketplaces to support Team MCPs and organization groups. Admins can configure Team MCP servers for cloud agents, agents window, IDE, and CLI. Team marketplaces now support organization groups in addition to team-level SCIM directory groups.

Customize page consolidates plugins, skills, MCPs, subagents, and team/workspace management. Marketplace leaderboard, plugin canvases, and team marketplaces with GitLab/BitBucket/Azure DevOps imports.

/automate skill for creating automations, new GitHub/Slack triggers, computer use tool for demos. Automations can be saved incompletely, open PRs by default, and delete memory files.

Design Mode in the Cursor browser allows clicking, drawing, or describing changes by voice to help agents update your UI. Multi-select elements, voice input, and visual relationships on the page.

### May 202613 releases

Cursor is now available in Jira. Assign work items to Cursor, or mention @Cursor in a comment to kick off a cloud agent. Cursor uses the work item title, description, comments, and your team's repository settings to scope the task.

This release introduces quality-of-life improvements to the Agents Window including full-screen tabs and compact chat responses. It also includes numerous improvements and bug fixes for agent workflows.

Mention @Cursor in any Teams channel to delegate a task to a cloud agent or pull information from Cursor into Teams. Cursor automatically picks the right repository and model based on your prompt and recent agent activity. It reads the entire thread for context before implementi…

Teams admins and Individual plan users can now customize the effort level Bugbot uses for its PR reviews, with three different configurations: Default, High, and Custom. Customers must be on usage-based billing for Bugbot to customize effort levels.

This release introduces updates for Enterprise admins: new model access controls, soft spend limits, and enhanced usage analytics. Includes granular model blocking, intelligent alerts, and user-specific analytics filtering.

Admins can now create a team marketplace without connecting a repository first. Add, remove, and configure install behavior for first-party plugins directly in team marketplace settings. Plugins bundle capabilities like MCP servers, skills, subagents, rules, and hooks that exten…

### April 20268 releases

Cursor Security Review is now in beta on Teams and Enterprise plans. Includes Security Reviewer (PR security checks) and Vulnerability Scanner (scheduled codebase scans) with Slack integration and custom configuration options.

We've shipped quality-of-life improvements to the Cursor CLI to make working with agents in the terminal more delightful. # Debug Mode in CLI Use /debug to find root causes and fix tricky bugs that are hard to reproduce or understand. In Debug Mode , Cursor generates hypotheses,…

Cursor 3 is now available. # Agents Window The new Cursor interface allows you to run many agents in parallel across repos and environments: locally, in worktrees, in the cloud, and on remote SSH. It's simpler, more powerful, and centered around agents, while keeping the depth o…

### March 20266 releases

Cursor now supports self-hosted cloud agents that keep your code and tool execution entirely in your own network. Your codebase, build outputs, and secrets all stay on internal machines running in your infrastructure, while the agent handles tool calls locally. Self-hosted cloud…

### February 20265 releases

Cloud agents can now use the software they create to test changes and demo their work. Each agent runs in an isolated VM with a full development environment, producing merge-ready PRs with artifacts (videos, screenshots, and logs).

Cursor updated the CLI with plan-to-cloud handoff, inline Mermaid diagrams, and a long list of productivity fixes for terminal-first workflows.

Cursor introduced plugins on the marketplace, sandbox egress controls, and asynchronous subagents to make larger agent workflows easier to compose.

### January 20263 releases

Version 2.4 release with Subagents, Agent Skills, Image Generation, Cursor Blame, Clarification Questions, and numerous editor/CLI improvements across agent workflows

Cursor added Plan and Ask modes to the CLI, plus cloud handoff for background tasks and one-click MCP auth. The update pushed more of Cursor's agent workflow into the terminal.

Cursor shipped new CLI commands for model, rules, and MCP management. Hooks also became significantly faster, making the terminal workflow feel lighter and more practical.

### December 20252 releases

Enterprise features including Conversation Insights, Shared Agent Transcripts, Billing Groups, Linux Sandboxing, and Service Accounts for Cursor. Service accounts roll out to Enterprise accounts starting 12/22.

## Cursor Cloud updates

12 releases in the last 12 months, the latest on .

## Cursor Enterprise updates

5 releases in the last 12 months, the latest on .

## Bugbot updates

4 releases in the last 12 months, the latest on .

## Shipping cadence

Releases per month over the last 12 months. Each bar is one calendar month, and the last bar is the month in progress. Busiest month: May 2026, 13 releases.

## What Cursor ships

Releases in the last 12 months by category and product.

### By category

- Product47
- Developer Tools10
- Ecosystem3
- Infrastructure3
- Enterprise & Security2
- Model2

### By product

- Cursor38
- Cursor Cloud12
- Cursor Enterprise5
- Bugbot4
- Cursor CLI2

## How Cursor compares

Other AI coding tools we track, ranked by releases so far in November 2026.

## Cursor updates FAQ

## ### When did Cursor last ship an update?


The most recent Cursor update we track is “Voice Chat with Agents”, released on November 6, 2026.

## ### What did Cursor release in November 2026?


Cursor shipped 2 updates in November 2026, including “Voice Chat with Agents”, “Build Live Apps in Cursor”.

## ### What models has Cursor released in 2026?


We tracked 2 model releases from Cursor in 2026, most recently “Composer 2.5” (May 18), “Composer 2” (Mar 19). See the latest model release

## ### When was Composer 2.5 released?


Cursor released Composer 2.5 on May 18, 2026. Composer 2.5 improves intelligence and behavior for long tasks, with new pricing and usage incentives. Read the entry

## ### How often does Cursor release updates?


We tracked 68 updates from Cursor in the last 12 months, about 6 a month. The busiest month was May 2026, with 13 releases.

## ### Where is Cursor's official changelog?


Cursor publishes release notes on Cursor Changelog, linked at the top of this page. Every entry here links back to the official page it came from.

## How we track Cursor

Entries come from official Cursor sources: Cursor Changelog, Cursor Community Forum. We watch official sites continuously for new pages, check GitHub releases and RSS feeds every 15 minutes, and re-read changelog pages every 6 hours.

Each entry is written by an AI model from the official page and links back to it, and is checked against releases we already have to keep repeats out. Dates are the publish date on the official page where it gives one. This page last changed on .

How we track AI releases explains the method in full.

## Guides

## Earlier Cursor releases

2 releases from before the last 12 months, newest first. Each links to its official source.

Newsletter

## Get Cursor's updates in your monthly brief.

Each month we pull Cursor's biggest releases into one clear read, alongside the launches that mattered from the rest of AI, so you don't have to watch every changelog yourself.

### Monthly signal

The standout launches, model drops, and platform moves worth tracking.

### Zero feed fatigue

One tight read instead of bouncing between company blogs and timelines.

### Built for operators

Clear summaries you can scan fast and forward to the team when needed.

Monthly dispatch

### Subscribe to the newsletter

Get a clean monthly dispatch covering the launches that mattered, not every minor product breadcrumb.

One email each month, tuned for signal over noise.
