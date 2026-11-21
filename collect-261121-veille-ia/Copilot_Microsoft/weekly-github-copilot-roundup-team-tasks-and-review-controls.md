---
id: collect-261121-veille-ia/copilot-microsoft/weekly-github-copilot-roundup-team-tasks-and-review-controls
title: "Weekly GitHub Copilot Roundup: Team Tasks and Review Controls"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-09"]
keywords: ["copilot", "github", "tasks", "teams", "coding", "team", "review", "task"]
source: https://tech.xebia.ms/all/roundups/weekly-github-copilot-roundup-2026-11-09
source_anchor: ""
source_lines: [1, 183]
section: "Copilot_Microsoft"
---
# Weekly GitHub Copilot Roundup: Team Tasks and Review Controls

@GitHub coding tasks are now generally available in Slack and Microsoft Teams, letting developers turn conversations into issues, code changes, and pull requests, with Teams Canvas providing shared task visibility. Copilot now supports local and isolated cloud tasks in JetBrains Air, while a public preview brings it into Xcode's built-in coding assistant starting with Xcode 27.3 beta. Code review gains REST and GraphQL configuration controls, multi-model balanced reviews, and outcome reporting, while usage reports add custom-model breakdowns, chat and agent adoption data, and indicators of missing telemetry. Together, these updates extend Copilot across team conversations and development tools while giving organizations more control over reviews and better visibility into usage and results.

## This Week's Overview

## Copilot coding tasks reach general availability in Slack and Teams

Developers can now start coding work from team conversations, with @GitHub generally available in Slack and Microsoft Teams. The Teams release adds shared task visibility and an onboarding path that requires administrator configuration.

### From conversation to pull request

@GitHub lets developers investigate bugs, create issues, implement changes, and open pull requests directly from Slack and Teams conversations. It now identifies the repository from a message and starts coding sessions without requiring repeated repository selection.

Created issues and pull requests include the originating conversation and username in their descriptions, preserving context for reviewers who were not part of the discussion. In Teams, Canvas lets teammates review implementation plans, code changes, and previews alongside the conversation.

### Teams deployment and shared task visibility

The generally available GitHub app for Microsoft Teams combines repository notifications and Copilot in one app. GitHub Canvas displays task plans, file changes, associated pull requests, and Copilot progress, while multi-repository support and Model Context Protocol (MCP) connections bring additional development tools and services into tasks.

Guided web onboarding connects GitHub accounts and grants organization and repository access, but users still depend on administrative setup. Administrators must enable resource-specific consent, allow the Teams app, configure the required GitHub capabilities and billing, and assign Copilot licenses.

## More ways to run Copilot in development tools

Copilot's client updates cover local and cloud tasks in JetBrains Air, a preview integration with Xcode's built-in assistant, and changes to terminal and editor workflows. The new integrations have different requirements and do not replace every existing plugin or extension option.

### JetBrains Air supports local and isolated cloud tasks

GitHub Copilot is now available in JetBrains Air through the Agent Client Protocol, supporting local coding and testing with project context as well as cloud tasks. Cloud work uses an existing Copilot subscription rather than JetBrains AI credits, and requires an Air Teams-enabled business organization plus administrator enablement.

Each cloud task runs in a Docker container on a fresh virtual machine, with proxied traffic and no state carried between tasks. Organizations should account for the management limitations: users cannot choose between personal and organizational plans per task, and Air neither includes Copilot usage analytics nor imposes Copilot spending limits.

A companion walkthrough covers connecting an existing Copilot account and configuring the plugin in JetBrains IDEs. The native GitHub Copilot plugin remains the option that provides the latest features and enterprise capabilities.

### Xcode's built-in assistant adds a Copilot preview

Copilot is available in public preview inside Xcode's built-in coding assistant starting with Xcode 27.3 beta. It supports all Copilot plans and requires a Mac running macOS Tahoe 26.6 or later.

Users configure the integration through `Xcode > Settings > Intelligence`. The existing GitHub Copilot for Xcode extension remains a separate option, so adopting the built-in assistant is a distinct choice.

### Settings, scheduling, and pull request controls

The weekly release summary adds practical controls alongside the Air and Xcode announcements. Copilot CLI's `/settings` command now opens a searchable dashboard covering plugins, MCP servers, and other settings, while VS Code local automations support cron expressions for scheduling and custom agents from the workspace.

Agent Merge can now be configured directly from a pull request. It stays enabled while required checks or rules are pending, avoiding the need to re-enable it while those requirements remain unresolved.

## Code review gains API controls and outcome reporting

Copilot code review updates address both how organizations configure reviews and how they evaluate the resulting feedback. The reporting changes distinguish addressed comments from the narrower action of applying a ready-made suggestion.

### Review configuration and execution

Copilot code review settings can now be managed through REST and GraphQL APIs at enterprise, organization, and repository levels. The updates are available to Copilot Pro, Pro+, Max, Business, and Enterprise plans, but authentication support differs by scope: enterprise and organization settings endpoints do not support GitHub Apps or fine-grained personal access tokens, while repository endpoints support both.

Balanced effort reviews now use three reviewer models, with another model combining their findings. Failed or timed-out reviews receive one automatic retry, and Copilot can switch to another eligible model when the preferred model is unavailable.

### Addressed comments and merged pull requests

Enterprise and organization `repos-1-day` reports now include a `copilot_code_review` object tracking addressed comments and merged pull requests without subsequent human change requests. The addressed rate is `total_comments_addressed` divided by `total_comments_evaluated`, with counts broken down by severity, review tier, comment type, and customization.

Comments count on the pull request's merge day, which matters when interpreting daily results. Bot-opened pull requests usually lack comment evaluation but still count toward the no-human-change-request metric. Access requires an eligible owner, billing manager, or custom role with View Copilot Metrics permission, plus an enabled Copilot usage metrics policy.

## Usage reporting separates model choices, adoption, and missing data

Copilot's reporting updates expose more detail without making every metric complete or directly comparable. Administrators get new model and feature breakdowns, alongside indicators that explain where client data is missing.

### Custom model usage becomes identifiable

Local Bring Your Own Key (BYOK) and enterprise custom model usage now appear in enterprise and organization reports at user and aggregate levels over 1-day and 28-day periods. The `is_custom_enterprise_model` field identifies enterprise custom model requests across every client, including IDEs, using server-side observations.

Existing totals remain unchanged because BYOK usage was already counted. Local BYOK counts can still undercount when client telemetry is disabled or providers do not report token usage, so the breakdown should not be treated as a complete accounting in those cases. Access requires an eligible owner, billing manager, or custom role permission and an enabled Copilot usage metrics policy.

### Adoption charts include chat and the VS Code Agents window

The Agent adoption chart now includes Copilot Chat and the VS Code Agents window, bringing reported feature engagement to nine experiences. Enterprise and organization 28-day reports add `chat` and `vscode_agent` entries under `copilot_feature_engagement.totals_by_feature`, with engagement requiring usage on at least two days during the period.

The dashboard renames Passive users to Chat-first users without changing cohort sizes, adoption phases, or the API value `No Cohort`. Chat includes captured agent mode usage, and access remains subject to the required administrative or metrics permission and enabled usage metrics policy.

### Telemetry gaps become visible

New per-user fields indicate whether client telemetry arrived and whether each IDE and Copilot extension meets usage metrics minimum versions. Affected-user counts appear in 1-day reports and each `day_totals` entry of 28-day reports, helping explain why active users may lack lines-of-code, feature, or model breakdowns.

The counts can overlap and must not be added together to calculate unique affected users. Null or omitted values mean data is unavailable, including for days before release, and access requires the relevant owner, billing, or View Copilot Metrics permission and an enabled usage metrics policy.

## Foundry Local connects on-device models to Copilot

A Foundry Local walkthrough connects a local model to GitHub Copilot through BYOK and a local API endpoint. The setup requires Foundry Local CLI 0.13.0 or later and an NVIDIA GPU with at least 24 GB of VRAM, with four-bit quantization reducing Qwen3.8-27B to approximately 18 GB.

CUDA and compatible cuDNN runtimes must be installed separately because Foundry Local does not download them. Its ONNX Runtime configuration uses speculative decoding, KV-cache quantization, prefix caching, continuous batching, and CPU offloading to support local inference. Developers evaluating the setup should check the hardware and runtime requirements before configuring Copilot's connection.

## Tool connections bring operational and data context into Copilot

Examples across Azure DevOps, OneLake, and PostgreSQL show how Copilot can work with development systems rather than relying only on source files. The workflows range from guarded pipeline operations to discovering query tools and exploring unfamiliar schemas.

### An MCP operations layer for image pipelines

A Python MCP server exposes four tools for listing Azure DevOps pipeline runs, inspecting status, checking release evidence, and optionally queueing a pipeline. VS Code starts the local server over standard input and output through workspace MCP configuration, making those tools available in Copilot Chat.

Queueing is disabled by default and, when enabled, requires explicit confirmation and allow-listed parameters. The evidence tool checks for build-summary, image-validation, security-scan, and promotion-record artifacts, while Azure DevOps approvals and permissions remain authoritative. Production guidance recommends least privilege and separate read and queue permissions.

### OneLake discovery selects the right data tools

OneLake offers a remotely hosted MCP server in preview while retaining the local option, and Fabric Core MCP adds tools for managing shortcuts and OneLake security. A preview endpoint discovery API returns available query engines, skills, MCP servers, and endpoints for Fabric items.

Copilot uses discovery to select engine-specific querying tools and run Spark analysis. Shortcuts allow queries against external data without copying it, while shortcut transforms convert files into tables and keep them updated as files change.

### Exploring an unfamiliar PostgreSQL database

A VS Code demonstration uses the PostgreSQL extension and Copilot Chat to explore schemas, map table relationships, and locate data needed by an application. It focuses on understanding an inherited database before working with its data.

## .NET migrations pair agent assistance with explicit validation

The .NET guidance covers two separate migration targets: Logic Apps custom code on .NET 10 and Windows applications moving to Arm64. Both require developers to verify the migrated application rather than treating configuration changes or a successful build as sufficient.

### Logic Apps custom code moves to .NET 10

Logic Apps Standard custom-code support requires extension version 5.1013.2 and extension bundle 1.194.31. New projects target only .NET 10, while existing .NET 8 projects require explicit configuration migration.

Deployed settings must include `LOGIC_APPS_CUSTOMCODE_DOTNETVERSION=net10.0` and `FUNCTIONS_INPROC_NET8_ENABLED=1`, with `FUNCTIONS_WORKER_RUNTIME` remaining `dotnet`. Verification includes building, checking publication under `lib\custom\net10.0`, executing the workflow, and verifying a function breakpoint. The optional migration skill requires GitHub CLI 2.90.0 or later, and installing it for Copilot does not execute the migration.

### The upgrade agent assists Windows Arm64 migration

The GitHub Copilot upgrade agent supports moving .NET projects from Windows x64 or x86 to Windows Arm64. It assesses dependencies and architecture assumptions, creates a migration plan, and helps update code, dependencies, and project configuration.

The agent can build migrated components and run tests when matching Windows Arm64 hardware or CI runners are available. A successful build does not replace runtime testing on Arm64, and the scenario focuses on portability and correctness rather than architecture-specific performance tuning.

## Agent-assisted engineering puts verification and human approval into practice

This week's guides and case studies describe agents working beyond implementation, including planning, testing, review, and deployment. They also show where teams retain human decisions and explicit release controls.

### Organizing the development loop

GitHub's agentic engineering guide organizes work into Plan, Build, Verify, and Observe. Sessions can continue across clients, cloud sandboxes let tasks run independently of the developer's machine, and Copilot Dreaming reviews eligible sessions and pull request feedback to update memories and propose instruction or skill changes.

Verification includes Agent Merge handling failing checks, review comments, and merge conflicts before merging when configured checks pass. The guide also covers GitHub package firewall, currently in public preview for npm, which can block downloads that violate configured security policies.

Related sessions connect these capabilities to team responsibilities and tool choice. Mario Rodriguez and Evan Boyle discuss developers setting outcomes and standards while agents plan, review, secure, and deploy, while a shared-project demonstration spans the Copilot app, CLI, VS Code, JetBrains, mobile, web, and Slack with attention to context, cost, and control.

- Beyond code generation: A developer’s guide to agentic engineering
- Humans set direction. Agents close the loop.
- Work together, your way, with GitHub Copilot

### Specialized agents with human approval gates

Levi Strauss & Co. built a multi-agent software development lifecycle on Copilot's custom agents platform. Seventeen specialized agents cover requirements, specification generation, automated testing, security scanning, accessibility validation, and deployment, with MCP connections to Jira, Confluence, and New Relic.

Each pull request receives review from 5 to 11 independent agents that fix issues and commit changes back to the branch. The workflow retains strict human approval gates, separating automated contributions from approval to ship.

### Repository requirements through production release

Bartolesi's product-building workflow keeps requirements in the repository, uses Copilot CLI for implementation, and ends with his pull request review after Claude Code review. His dotcraft plugin packages reusable skills and agents for .NET, Blazor, and MudBlazor across supported coding clients.

The examples make verification and deployment concrete: Brainy's Playwright CI job uses a real SQL Server database and uploads traces and screenshots on failure, while DayPulse creates Azure Static Web Apps previews for same-repository pull requests. A shared SaaS release workflow deploys versioned packages to Azure App Service using OpenID Connect (OIDC), with the explicit limitation that rolling back application code does not reverse database migrations.

### Maintenance work on Arnis

Arnis, which converts OpenStreetMap elements and detailed 3D models into Minecraft blocks, has reached close to 900,000 GitHub downloads and about 70 contributors since Louis Erbkamm started it in 2022. Erbkamm uses Copilot for issue summaries, bug investigation and fixes, performance reviews of memory usage and world generation time, and sub-agent checks of sensitive code paths and regression cases.

## Other GitHub Copilot News

GitHub Copilot verified proficiency lets developers opt in to assessment across five tiers using everyday product usage rather than a separate exam. Developers reaching proficient or higher can claim a Microsoft Pro Badge on Credly and showcase verified capability statements on LinkedIn, with the walkthrough covering GitHub Learn, results, privacy, and control.

Universe recaps examine Copilot Fusion's Single, Cascade, and Critique patterns, public-preview Projects, and cloud Automations requirements, including paid-plan eligibility and AI credit and GitHub Actions usage. They also cover GitHub's reported test of 7,200 writes per second with 5,000 agents targeting one repository, while the October Microsoft AI overview places Copilot's app, Fusion, reviews, Projects, session continuity, and Automations alongside broader model and infrastructure updates.

- GitHub Universe 2026: Announcements, Sessions, and My Technical Takeaways
- GitHub Universe 2026: My recap from San Francisco
- Microsoft AI Update October 2026

Creative-learning sessions offer smaller-scale ways to work with Copilot: Martin Woodward recommends timeboxed side projects, while Qianqian Ye and John Maeda discuss iterative coding, design, and creative judgment through p5.js and open source. A short VS Code demonstration covers creating images with Copilot.

- Give yourself permission to play: how to learn with side projects
- Sketching with code in the age of AI
- 🎨 Create Images with GitHub Copilot in #vscode #githubcopilot

The GitHub Universe 2026 badge doubles as a handheld gaming console with retro games and built-in apps. A walkthrough uses Copilot for setup help when adding community games from Ashley Wolf's repository and demonstrates installing the classic Microsoft 3D Maze screen saver.

## Also relevant this week

- Microsoft Foundry extension for Azure Developer CLI is generally available: The Microsoft Foundry extension bundle for Azure Developer CLI reaches version 1.0 and is generally available. Full story in the Azure roundup.
- Using PostgreSQL to reflect on its own history and code: introducing pghacker-mcp.dev: pghacker-mcp is a free, public, read-only MCP server with 17 tools connecting PostgreSQL source history and mailing-list discussions. Full story in the Artificial Intelligence roundup.
- Go beyond the basics: Explore specialized Azure plugins for your AI agent: The Azure plugin catalog includes six specialized plugins alongside the core Azure plugin. Full story in the Azure roundup.
- Your best AX fixes are hiding in agent trajectories: Five GitHub Copilot Chat runs using Claude Sonnet 5.5 without extensions produced working CI scripts, but their trajectories revealed 15 problems. Full story in the Artificial Intelligence roundup.
- Delivering Skills in NuGet packages: NuGet-delivered skills use the standard SKILL.md format, consisting of Markdown with a YAML header. Full story in the .NET roundup.
