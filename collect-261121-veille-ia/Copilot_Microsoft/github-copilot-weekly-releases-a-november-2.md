---
id: collect-261121-veille-ia/copilot-microsoft/github-copilot-weekly-releases-a-november-2
title: "GitHub Copilot weekly releases â November 2"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["github", "copilot", "xcode", "pull", "weekly", "agent", "views", "across"]
source: https://blogs.techbytes.app/posts/github-copilot-weekly-releases-november-2/
source_anchor: ""
source_lines: [1, 100]
section: "Copilot_Microsoft"
---
# GitHub Copilot weekly releases â November 2

GitHub Copilot added JetBrains Air and Xcode 27.3 beta support while introducing Agent Merge pull request automation and VS Code 1.145 scheduled cron tasks.

By Dillip Chowdary â¢ Nov 06, 2026 â¢ Source: GitHub Changelog

At a glance

- Xcode
- Native Copilot integration, public preview in Xcode 27.3 beta
- Visual Studio Code
- Version 1.145 enables experimental chat view modes
- My Work views
- More than ten saved custom views (previously 10)
- Terminal command
- /settings shows extensions, plugins, and MCP servers
- Cloud agent uploads
- Image files only
- Approvals
- Personal instructions are opt-in, local sessions only

In this post

GitHub launched an extensive weekly update across its developer ecosystem, delivering native integrations for JetBrains Air and Apple Xcode alongside workflow upgrades for pull requests, automated agents, and workspace configuration. As detailed in GitHub Changelog's report, the engineering team expanded the assistant's reach across the GitHub Copilot desktop app, the command-line interface, JetBrains environments, Xcode 27.3 beta, and the Visual Studio Code 1.145 release. The release focuses on eliminating friction in multi-session coordination, improving pull request handling, and giving software engineers direct visibility into model reasoning.

This technical breakdown covers the complete set of capabilities introduced in the November release cycle, detailing IDE integrations, automated pull request merging, and agent control panels. It is designed for software developers, engineering leads, and DevOps practitioners who rely on GitHub Copilot across multiple development environments and want to streamline automation. Readers will discover how to configure unattended tool approvals, manage scheduled background jobs with cron patterns, separate nested discussion threads, and debug remote applications through interactive web canvases without leaving their primary text editor.

## 01What shipped

The release establishes native integration within JetBrains Air, allowing developers to orchestrate concurrent agent sessions and execute tasks locally in JetBrains IDEs or in cloud workspaces via Air Teams using existing subscriptions. Concurrently, Apple ecosystem engineers gain access to Copilot through a native integration with the built-in coding assistant in Xcode, entering public preview with the Xcode 27.3 beta release. These IDE expansions allow engineers working across diverse stacks to run contextual agent workflows directly within their platform-specific development tools.

In the standalone GitHub Copilot application, the team shipped Agent Merge to automate pull request closure as soon as all conditions are fulfilled. The tool actively monitors reviewer status, enables additional review requests from the pull request view, and lets developers highlight comment text to quote or discuss directly with the model. Navigation also improved with support for dragging data files onto the sidebar, rendering comma-separated and tab-separated files as interactive tables featuring pinned headers and source toggles.

## 02What changed for builders

Engineering teams gained several productivity enhancements designed to simplify project tracking and multi-tasking. The My Work interface now supports more than ten saved custom views, giving developers granular filters across ongoing assignments.

In session management, developers can archive pinned sessions once associated pull requests merge or close, execute bulk pin or unpin operations across multiple threads, and detach nested conversations into separate windows without losing message history. When conversations hit context window boundaries, users can compact dialogue or inspect token allocation directly from the error banner.

| Capability | Baseline Implementation | Weekly Update Enhancement | 
|---|---|---|
| My Work Views | Limited to 10 saved custom views | Supports more than 10 saved custom views | 
| Agent Merge | Pauses or disconnects on pending rules | Stays active while required checks or rules remain pending | 
| Terminal History | Loads entire past output at once | Renders on demand as sections expand or reveal | 
| IDE Ecosystem | Standard JetBrains desktop editors | Adds JetBrains Air, Air Teams, and Xcode 27.3 beta | 

Inside Visual Studio Code 1.145, builders receive experimental chat modes that collapse or reveal model reasoning steps and tool execution without resetting ongoing conversations. Teams orchestrating background routines can now configure local automations using standard cron expressions paired with custom workspace agents to execute recurring tasks. Furthermore, developers inspecting remote services can preview remote applications and interact with canvases directly in the Agents window by activating remote browser support in settings, preventing context switching across independent browser windows during live testing.

Advertisement

Tech Pulse Daily

### Get tomorrow's pulse first

Join engineers who read Tech Pulse before stand-up. Free, weekday mornings.

## 03How to install or upgrade

Upgrading across developer environments requires updating respective client applications and command-line utilities. Engineers utilizing the terminal interface can access a consolidated settings dashboard to inspect installed extensions, plugins, and Model Context Protocol servers by entering the settings command directly inside their active command line session. This unified console aggregates developer configurations into a searchable interface, simplifying management across local development environments, system plugins, and connected servers without requiring manual edits to dispersed configuration files across operating system paths.

`/settings`
Developers targeting JetBrains IDEs can access the updated integration by downloading the JetBrains Air environment and connecting their active Copilot account to initiate local or cloud runs through Air Teams. Apple developers must install the Xcode 27.3 beta build to activate the public preview inside Xcode's native coding assistant interface. In Visual Studio Code, updating the editor to the 1.145 release enables the experimental chat view modes, scheduled cron automations, and remote agent preview canvases through standard application settings.

## 04Gotchas and compatibility

Teams adopting the new capabilities must navigate several platform boundaries and permission constraints. The personal instructions system for assisted approvals, which provides automated decision guidelines for tool invocations without manual approval prompts, is strictly opt-in and restricted exclusively to local sessions. Similarly, the Agents window preview canvases and remote application inspections in Visual Studio Code 1.145 are not active by default; developers must explicitly enable remote browser support alongside the canvases feature flag within their client configuration before accessing remote instances.

Context handling across local and cloud environments also exhibits functional differences that developers should note. When adding external project references through the Add Context file picker, developers can import local screenshots, architectural mockups, and arbitrary files without placing them inside the primary project workspace.

However, cloud-hosted agent sessions maintain stricter format constraints and exclusively accept image files. Additionally, long terminal output in chat now renders dynamically upon expansion rather than loading synchronously, which changes how users inspect historical logs during active debugging sessions.

## 05What to watch next

The introduction of automated background workflows and external tool management signals a significant transition toward unattended developer operations. As Agent Merge remains persistently active while required integration checks and repository branch rules remain pending, development teams should review repository governance policies to ensure automated pull request merges align with organizational quality gates. The addition of opt-in personal instructions for assisted approvals similarly requires development teams to establish clear local safety boundaries before delegating autonomous command executions to local coding assistants.

Engineers should also monitor the progression of preview integrations across Apple and JetBrains environments as broader stability updates roll out. The Xcode 27.3 beta coding assistant preview represents GitHub's first deep native integration for Apple platform developers, making subsequent beta iterations critical for production adoption. Teams running Visual Studio Code 1.145 should evaluate how cron-based local automations and workspace agents integrate into existing continuous integration routines, tracking how scheduled agent tasks and remote canvas debugging alter daily developer workflows across complex software repositories.

## Frequently asked questions

### Which IDEs received new GitHub Copilot integrations in the weekly release?

GitHub Copilot is now available in JetBrains Air, supporting local tasks and Air Teams cloud execution under existing subscriptions. It is also available in public preview for Apple developers starting with Xcode 27.3 beta.

### How does Agent Merge handle pending pull request checks?

Agent Merge can be set up directly from a pull request and remains enabled while required checks or repository rules are pending. It automatically completes the merge as soon as all requirements pass.

### What context management options are available when conversations reach limits?

When a conversation reaches its context limit, developers can compact the conversation or inspect what data is using available space directly from the error display.

### What file formats are supported when attaching context to chat sessions?

Developers can bring screenshots, mockups, and other files into chat using Add Context without moving them into the workspace. Cloud sessions exclusively support image files.

## Sources

## Related on Tech Bytes

Advertisement
