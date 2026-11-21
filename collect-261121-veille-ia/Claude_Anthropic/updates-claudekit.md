---
id: collect-261121-veille-ia/claude-anthropic/updates-claudekit
title: "Updates — ClaudeKit"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "code", "mode", "patch", "changes", "including", "bash", "fixes"]
source: https://claudekit.io/en/updates/
source_anchor: ""
source_lines: [1, 362]
section: "Claude_Anthropic"
---
§ 1 

 ## All entries

89 of 89
  [01]    [02]    [03]    [04]    [05]    [06]    [07]    [08]    [09]    [10]    [11]    [12]    [13]    [14]    [15]    [16]    [17]    [18]    [19]    [20]    [21]    [22]    [23]    [24]    [25]    [26]    [27]    [28]    [29]    [30]    [31]    [32]    [33]    [34]    [35]    [36]    [37]    [38]    [39]    [40]    [41]    [42]    [43]    [44]    [45]    [46]    [47]    [48]    [49]    [50]    [51]    [52]    [53]    [54]    [55]    [56]    [57]    [58]    [59]    [60]    [61]    [62]    [63]    [64]    [65]    [66]    [67]    [68]    [69]    [70]    [71]    [72]    [73]    [74]    [75]    [76]    [77]    [78]    [79]    [80]    [81]    [82]    [83]    [84]    [85]    [86]    [87]    [88]    [89]    

  
[PATCH]
 Claude Code 2.1.329  · The trust dialog now says where a folder's hooks are declared, and `/tasks` adds this session's scheduled tasks, `/loop` wakeups, the goal and the conversation's forks. About 130 changes, including fixes for `.claude.json` being reset to defaults and repeated sign-outs on macOS, and more than 15 for Code Mode. 

   
[PATCH]
 Claude Code 2.1.328  · Adds auto mode for Claude Opus 4.6 and Sonnet 4.6 on Amazon Bedrock, Google Cloud's Agent Platform, Microsoft Foundry and Claude apps gateway, and `/code-review` on most models now reports only bugs and CLAUDE.md violations. Over 140 changes, including more than 20 for Code Mode and a fix for Bash commands in zsh stopping at a word that starts with `=`. 

   
[PATCH]
 Claude Code 2.1.327  · Shell start-up files may now run before a session's first Bash command is approved, to make it faster, and auto mode ignores more allow rules that run any package. Over 170 changes, including fixes for Remote Control in worktree mode deleting git-ignored files such as `.env`, and `claude mcp add` deleting other top-level keys from `.mcp.json`. 

   
[NEW]
 Linked billing on the Claude Platform — let customers pay for Claude API usage without sharing an API key (beta)  · Linked billing is now available in beta on the Claude Platform. Third-party platforms, such as coding agents and workflow automation tools, can let their end customers pay for Claude API usage without sharing an API key. Customers link their Claude Platform organization, and usage bills directly to it. Customers approve each linked platform, set a spend limit, and can unlink at any time. 

   
[PATCH]
 Claude Code 2.1.326  · Sessions now start in auto mode in organizations with the Claude Code + Cowork HIPAA configuration and when your organization's policy hasn't loaded, and permission checks for Bash, PowerShell, the sandbox and bypass permissions mode are stricter. Over 360 changes, including fixes for parallel MCP tool calls reading another call's saved result and `claude purge` hiding its prompts when stdout is redirected. 

   
[PATCH]
 Claude Code 2.1.325  · Sandboxed Bash commands now follow the normal permission flow by default, and scheduled tasks and `/loop` ask before running a skill that declares `allowed-tools` or hooks. Over 130 changes, including fixes for a security issue in how zstd-compressed responses are decoded and a gap in Bash permission checks where some compound commands could run without asking. 

   
[PATCH]
 Claude Code 2.1.323  · Adds Code Mode, an experimental opt-in in which Claude writes JavaScript to call its tools, and `engines["claude-code"]` in `plugin.json` for a plugin's Claude Code version range. Over 290 changes, including fixes for `--permission-mode auto` starting in a mode that could allow more when auto mode was unavailable, and Bash deny rules missing commands run through `sudo`, `env` and similar wrappers. 

   
[NEW]
 Self-hosted Claude Managed Agents — operate Managed Agents inside your security perimeter (private beta)  · Self-hosted Claude Managed Agents is now available in private beta. You host the Managed Agents service and its sandboxes on your own infrastructure, in a cloud account or an on-premise data center, store agents, sessions, and session history in your own database, and choose where requests to Claude are sent: the Claude API, Amazon Bedrock, Claude Models on Google Cloud (Google managed), Microsoft Foundry, or your own model gateway. 

   
[PATCH]
 Claude Code 2.1.321  · Adds Claude Fable 5.2 (`claude-fable-5-2`), now the default Fable model, and a `--settings` option for `claude remote-control`. Over 110 changes, including fixes for MCP OAuth requests sending secrets such as refresh tokens to another host on a redirect, and a read-only Bash command running outside the sandbox without a permission prompt. 

   
[NEW]
 Claude Fable 5.2 and Claude Mythos 5.2  · Anthropic upgraded Claude Fable and Claude Mythos on November 9, 2026. Fable 5.2 and Mythos 5.2 improve on coding, reasoning, and vision, generate outputs up to 30% faster than the 5.1 models, and are available at a lower price: $8 per million input tokens and $40 per million output tokens. An optional fast mode speeds up token generation by up to 2.5 times at $16 / $80 per MTok. The model ID is claude-fable-5-2. Four changes from Claude Fable 5.1 can break your code, starting with the prefix check now applying to every account. Mythos 5.2 offers the same capabilities only to organizations verified through Anthropic's verification programs. 

   
[PATCH]
 Claude Code 2.1.319  · Adds support for MCP tasks in the terminal, the `permissions.denyInsteadOfPrompting` setting and `/claude-api latency-optimize`. Auto mode with telemetry or feature flags off, including behind LLM gateways, no longer falls back to the built-in classifier. Over 140 changes, including fixes for a Bash permission bypass and a sandbox escape when creating a worktree. 

   
[PATCH]
 Claude Code 2.1.318  · Adds the date a stored claude.ai sign-in ends with an advance warning, an `effort` option for a mod's `$.model.fork`, and `assume_role` on `mantle` upstreams of the Claude apps gateway. Over 80 changes, including fixes for config files and the stored login being lost or changes being dropped with no word on shared network home directories. 

   
[PATCH]
 Claude Code 2.1.317  · Adds a check that asks before a web or shell tool call sends your account email address to the network, a way for failed Agent SDK hook callbacks to block the action, and a cost report on the Claude apps gateway admin API. Over 80 changes, including a fix for `claude -p` sharing the session ID and transcript of the cloud or self-hosted runner session it runs inside. 

   
[NEW]
 Make long-running agents faster with message threads (public beta)  · Message threads, now in public beta on the Claude Platform, let you send your conversation once so the API receives only what's new with every turn. This reduces time to first token (TTFT) in long sessions, while token usage stays the same. 

   
[PATCH]
 Claude Code 2.1.316  · Adds `CLAUDE_CODE_DISABLE_ULTRATHINK`, which turns the `ultrathink` keyword off, `CLAUDE_CODE_SUBAGENT_EFFORT` for the effort level subagents run at, and pooled group budgets on the Claude apps gateway. Fast mode for Opus 4.8 is deprecated as of October 30, 2026 and will be removed on November 30, 2026. Over 50 changes, including a fix for shell commands being left running when Claude Code exits by itself. 

   
[PATCH]
 Claude Code 2.1.314  · Adds `permissions.blockWritesOutsideWorkingDirectories`, which makes Write, Edit and NotebookEdit refuse paths outside the working directories in every permission mode, and idle compaction in VS Code and Claude Desktop's Code tab. Over 100 changes, including fixes for sessions failing turn after turn with "Invalid effort level" when an admin caps effort and `claude -p` exiting with code 0 and no output when startup failed. 

   
[PATCH]
 Claude Code 2.1.312  · The Claude apps gateway gains `oidc.user_id_claim`, and Bedrock, Vertex, Foundry and Mantle now use the shorter system prompt for model names Claude Code doesn't recognize. Over 20 fixes, including Claude overwriting a very large CLAUDE.md in a subdirectory after seeing only its start and managed settings not being fetched after signing in from the VS Code extension. 

   
[PATCH]
 Claude Code 2.1.311  · `ultrathink` now runs its turn at xhigh effort on Opus 5.5, Sonnet 5.5, Haiku 5.5 and Fable 5.1, and `self-hosted-runner` gains options to keep an idle session. Over 50 fixes, including standard-context sessions stopping with "Usage credits required for 1M context" past 200K tokens and long freezes when a CLAUDE.md file holds a very long list item. 

   
[PATCH]
 Claude Code 2.1.310  · Adds the Monitor tool and plugin monitors on Bedrock, Vertex and Foundry, and when telemetry or nonessential traffic is disabled. Fixes requests failing behind some gateways and background cleanup deleting recent files when `cleanupPeriodDays` is very large, and adds cloud sessions and Remote Control permissions to Enterprise custom roles. 

   
[PATCH]
 Claude Code 2.1.309  · A sandbox and permission-check hardening release: fixes sandboxed Bash commands running the shell's startup files outside the sandbox, and sandboxed commands being able to modify the MCP config file passed with `--mcp-config`. Also changes `sed -i` to run as a normal Bash command and `claude ultrareview` to require workspace trust. 

   
[NEW]
 The full Claude Platform, available on Google Cloud (Anthropic managed GA)  · Claude Models on Google Cloud (Anthropic managed) is now generally available, giving Google Cloud customers the full set of Claude Platform features through Google Cloud authentication and billing. It includes Claude Managed Agents (beta), Advisor strategy (beta), the MCP connector (beta), Fast Mode on Claude Opus 5.5, and the Claude Console. 

   
[PATCH]
 Claude Code 2.1.307  · Adds `/claude-api api-version` to move Claude API code to API version `2026-10-21`, and a `claude_code.safety_event` OpenTelemetry event for each tool call a permission rule, auto mode, a safety check, a hook or the sandbox stopped, prompted or sent back. Also fixes HTTP MCP tool calls being sent twice and MCP servers failing while the macOS login keychain was locked or the computer went to sleep. 

   
[PATCH]
 Claude Code 2.1.305  · Fixes every request failing on Vertex, Bedrock and other providers when the endpoint or a gateway rejects tool search (a 2.1.304 regression). Adds `sandbox.allowNestedUserNamespaces` for Linux sandboxed commands and `CLAUDE_CODE_RETRY_WATCHDOG_USAGE_LIMIT_RECHECK_MS` for unattended retry mode, and turns each `/usage` warning into a one-line fix that `f` applies. 

   
[PATCH]
 Claude Code 2.1.304  · Adds `/auto-mode-setup`, a guided setup that reads your project and recent sessions to teach auto mode about your environment, `/claude-api managed-agents-secure` to compare a Managed Agents agent's settings with Anthropic's published security guide, and a warning when resuming an inactive conversation would use a noticeable share of your 5-hour usage limit. Fixes `claude --worktree` removing the worktree and its branch without asking when HEAD was detached, and `rm -r` of a mounted drive not getting the safety prompt. 

   
[PATCH]
 Claude Code 2.1.302  · Adds `CLAUDE_CODE_AUTO_MODE_MODEL` to choose the model the client-side auto mode classifier uses, an `Idle compaction` row in `/config`, and a notice that standalone Claude Design closes by December 14, 2026. Fork subagents are now on by default in `claude -p`, the Agent SDK, the desktop app and VS Code, and startup is faster with many plugins enabled. Fixes a "Prompt is too long" error when stopping a turn during compaction, and the Read tool loading image files of any size into memory. 

   
[PATCH]
 Claude Code 2.1.301  · Adds `CLAUDE_CODE_RETRY_WATCHDOG_MAX_USAGE_LIMIT_WAIT_MS` for unattended retry mode, the `%` motion in vim mode, and auto mode classifier fields on OpenTelemetry `tool_decision` events. Switching to a model with a smaller context window now has the previous model summarize first. Fixes quitting removing a worktree that may hold unpushed commits, and Grep and Glob reporting no matches when the search failed. 

   
[NEW]
 Code Review is now generally available in Claude Code, with about 3x faster reviews and per-repository review levels  · On October 15, 2026, Anthropic moved Code Review out of research preview and made it generally available to Claude Team and Enterprise organizations. Most reviews now finish about 3x faster (from roughly 20 minutes to 6-10 minutes), and admins can choose Quick, Standard, or Deep for each repository. 

   
[PATCH]
 Claude Code 2.1.300  · Adds `/advisor` on Claude Platform on AWS, cache writes split by lifetime in the OpenTelemetry `api_request` event, and GenAI semantic-convention attributes on trace spans. `updatedInput` from PreToolUse and PermissionRequest hooks is now validated like Claude's own input. Fixes `--worktree` in reftable repositories, resumed sessions losing compacted history, and conversations failing every turn on a rejected MCP tool schema. 

   
[PATCH]
 Claude Code 2.1.299  · Adds idle compaction, which compacts a large idle conversation before its one-hour prompt cache expires, and `CLAUDE_CODE_DISABLE_FILE_MENTIONS`, which gives Claude the paths of `@` file mentions instead of their contents. MCP allow rules now reach only the server they name. Fixes requests to Anthropic carrying the API key configured for a third-party gateway, and several sandbox escapes. 

   
[PATCH]
 Claude Code 2.1.296  · Adds an `allow_large` option to the Read tool and `autoCompactWindow` for subagents. Fixes Edit and NotebookEdit replacing every non-ASCII character in files that are not valid UTF-8, and managed-settings hooks refusing a tool call but not ending the turn. Raises the default limit on MCP tool descriptions sent up front from 2,048 to 4,096 characters. 

   
[PATCH]
 Claude Code 2.1.295  · Adds `onFailure: "block"` for command and HTTP hooks, so a hook that can't start or times out blocks the action, and Program Status Protocol (OSC 7501) support. Fixes every request failing on a `[1m]` model when a gateway, Bedrock, Vertex or Foundry refuses the context-1m beta, `claude -p` text output dropping earlier responses, and the terminal freezing on very long responses. 

   
[PATCH]
 Claude Code 2.1.293  · Adds Claude Haiku 5.5 (`claude-haiku-5-5`), now the default Haiku model on the Anthropic API. Fixes Claude treating its own last actions before a context compaction as done after it, and path-scoped rules and nested CLAUDE.md files not loading when Claude views a file through the Bash tool. Reverts the 2.1.290 cloud session wakeup fix. 

   
[NEW]
 Claude Haiku 5.5  · Anthropic introduced Claude Haiku 5.5 on October 7, 2026: the cheapest, fastest, and most capable small model it has ever released, designed for high-volume, cost-sensitive tasks. For requests up to 100,000 tokens it is priced 90% lower than Claude Haiku 4.5, at $0.10 per million input tokens and $0.50 per million output tokens, and on average it costs around 75% less to run. It is the first Haiku-class model with an adjustable effort setting, and the model ID is claude-haiku-5-5. 

   
[PATCH]
 Claude Code 2.1.292  · Adds `--marketplace <source>` to `claude plugin install` and an `effort` parameter to the Agent tool. Fixes PreToolUse hook approvals and auto mode bypassing the permission prompt for file reads from network (UNC) paths, and saved scheduled tasks created after `/resume`, `/branch` or `/clear` never firing. 

   
[PATCH]
 Claude Code 2.1.290  · Closes many ways deny and ask rules and the outside-directory read block could be missed, and fixes scheduled tasks like `/loop` silently not coming back after compaction or a background hand-off. The WebSearch budget now refills at 100 calls/hour instead of ending after 200 calls. 

   
[PATCH]
 Claude Code 2.1.289  · Closes several ways deny and ask rules could be missed (compound shell commands, symlinks, environment variable prefixes) and fixes the terminal freezing on short code blocks. A plugin or mod component that fails while drawn no longer takes down the whole session. 

   
[PATCH]
 Claude Code 2.1.288  · A prompt cleared with Ctrl+C can now be brought back with Up, /code-review gains `--max-findings`, and the agents view adds Ctrl+F to find a session by name. Mid-response API timeouts no longer fail the turn, and fixes focus on resume, auto mode, plugins, permission safeguards, and cloud sessions. 

   
[PATCH]
 Claude Code 2.1.287  · Claude Mods lets plugins modify deeper behavior, and You should know arrives as a built-in mod where a side agent watches your back and flags things you or Claude might miss. Opus 4.7+ and Fable now use a 1M context window by default on Bedrock, Vertex and Foundry, and MCP `alwaysLoad: false` defers all of a server's tools behind tool search. Fixes focus on screen reader mode, file delivery in Remote Control and cloud sessions, MCP, and permission safeguards. 

   
[NEW]
 Customize Claude Code with mods  · On October 1, 2026, Anthropic introduced mods for Claude Code: plugins made of JavaScript or TypeScript event handlers that can rewrite prompts, guard tool calls, and add panes and buttons to the interface. Mods require Claude Code v2.1.287 or later and work in the CLI and the Code tab of the Desktop app. 

   
[PATCH]
 Claude Code 2.1.286  · The permission prompt now shows a count such as "2 of 5" when several requests stack up, and the "N more" rows of fullscreen lists respond to the mouse. Failed API requests now share one retry limit per model call (at most 14 requests by default), and `--bare` connects only the MCP servers named on the command line. Fixes focus on `--resume` losing turns, secret redaction in logs and transcripts, Remote Control, subagents and MCP connections. 

   
[PATCH]
 Claude Code 2.1.285  · Adds `claude --desktop`, `claude plugin configure`, the `allowedProviders` managed setting and the `CLAUDE_CODE_DISABLE_WEB_FETCH` environment variable. Background Bash and PowerShell commands now stop after a time limit (default 30 min), and sessions behind a custom `ANTHROPIC_BASE_URL` use the 1M context window. Fixes focus on the Artifact tool, `/ultrareview` uploads, plugin installs, subagent permission modes and Remote Control. 

   
[PATCH]
 Claude Code 2.1.284  · Claude Sonnet 5.5 (`claude-sonnet-5-5`) is added and is now the default Sonnet model on the Anthropic API. Interactive terminal and VS Code sessions start in auto mode when no permission mode is configured, on every plan and provider, and Ultracode becomes its own toggle in `/effort`. Fixes focus on damaged response streams, "Prompt is too long" after compacting, MCP connections and plugin permissions. 

   
[NEW]
 Claude Sonnet 5.5  · Anthropic introduced Claude Sonnet 5.5 on September 28, 2026, the second model in the Claude 5.5 family. It's a clear upgrade over Claude Sonnet 5, runs 30%+ faster, and costs up to 30% less for most work. It's priced the same as Sonnet 5 at $2 per million input tokens, $10 per million output tokens, and $0.20 per million tokens for cache reads, and the model ID is claude-sonnet-5-5. It scores 70.6% on Terminal-Bench 4.0, compared to Sonnet 5's 10.3%, and two points below Opus 5.5 on GDPval-AA. 

   
[PATCH]
 Claude Code 2.1.283  · New `availableModelsMatch` and `deniedModels` managed settings let organizations pin allowed models to exact versions and block specific models, and `/doctor prompt-audit` audits CLAUDE.md files, skills, agents and commands for prompting patterns written for older models. Fixes focus on MCP server reliability, `installed_plugins.json` handling and vim mode, and the 2.1.282 reservation of the `claude-ai` name is reverted. 

   
[PATCH]
 Claude Code 2.1.282  · A new `maxProseWidth` setting caps the width of Claude's prose in wide terminals, and a startup notice lists telemetry variables in a project's settings files that were ignored or that turned telemetry off. Fixes focus on continued or resumed sessions losing earlier extended thinking, and on every request failing with a 400 error when history holds web search results the API cannot decrypt. Project and local settings now ignore OpenTelemetry export variables, and the `anthropic-skills` and `claude-ai` namespaces are reserved for skills synced from claude.ai. 

   
[PATCH]
 Claude Code 2.1.281  · `"attribution": false` in `settings.json` hides all commit and PR attribution, MCP servers on 2026-07-28 protocol connections can open a browser-based flow through URL-mode elicitation, and `claude plugin validate` now checks MCP servers. A recursive `rm` whose target is only command-substitution output, such as `rm -rf "$(pwd)"`, no longer runs unprompted in auto mode. Fixes focus on resumed sessions re-sending earlier turns in a changed form, and responses cut short or tool calls run twice behind a proxy or gateway. 

   
[NEW]
 Claude Marketplace: one place to discover plugins, agents, and services  · On September 23, 2026, Anthropic launched Claude Marketplace, bringing plugins and connectors, agents and products, and service partners into one place. Choose from more than 2,000 connectors and plugins, and use a portion of your committed Anthropic spend on Claude-powered software from partners. 

   
[PATCH]
 Claude Code 2.1.280  · Claude Opus 5.5 (`claude-opus-5-5`) arrives as the default Opus model, and the default model on Pro and Team Standard plans changes from Sonnet to Opus. A stray `y` or `n` no longer confirms or closes dialogs, and `CLAUDE_CODE_MAX_MCP_DESCRIPTION_LENGTH` changes the 2,048-character cap on MCP tool descriptions. Fixes include writes through a symlinked path being judged by their in-tree spelling, auto mode retrying or denying actions over and over, and Ctrl+C or Ctrl+D pressed twice in most dialogs quitting Claude Code. 

   
[NEW]
 Claude Opus 5.5  · Anthropic introduced Claude Opus 5.5 on September 22, 2026, the first model in the new Claude 5.5 family. It performs at the level of Claude Fable 5.1 on most work and costs 40% less to run than Opus 5. Input and output tokens are $4 and $20 per million, 20% less than Opus 5, and cache reads are $0.20 per million tokens, 60% less. The model ID is claude-opus-5-5, and it generates output more than 30% faster than Opus 5. Five-hour usage limits are increasing on Pro, Max, Team, and seat-based Enterprise plans. 

   
[NEW]
 Projects redesigned: from folder to conversation  · On September 17, 2026, Anthropic introduced a new experience for Claude projects, now available in beta in Claude Code. You describe what needs to get done and Claude manages the work: a coordinator directs threads, and each thread is a Claude Code cloud session working on its own branch and copy of the repo. Threads add to and draw from a shared memory, and a library collects the files you add and the artifacts Claude produces. It is rolling out first to select Claude Pro and Max subscribers who use cloud sessions, with updated projects across all of Claude and Team and Enterprise plans coming after that. 

   
[NEW]
 Building commerce agents with Claude  · Anthropic released a blueprint for building commerce agents on Claude on September 2, 2026. It ships two agents defined once and deployable three ways — a shopping agent for customers and a merchant agent for staff, running on the Messages API, the Claude Agent SDK, or Claude Managed Agents — along with four runnable vertical examples covering retail, travel, telecom, and entertainment. A Claude Code plugin called commerce-builder scaffolds a custom agent for your own store. The code is available under Apache License 2.0 at anthropics/commerce-agents, and every example runs on fictional data with no live orders or charges. 

   
[NEW]
 Claude Fable 5.1 and Claude Mythos 5.1  · Anthropic introduced Claude Fable 5.1 and Claude Mythos 5.1 on September 1, 2026, describing them as the world's most advanced models for coding and knowledge work. Input and output pricing is unchanged from Claude Fable 5 at $10 / $50 per MTok, but cache reads drop to $0.25 per MTok, making Fable 5.1 an estimated 25% less expensive than Fable 5 for typical workloads. The model ID is claude-fable-5-1, with a 1M token context window and 128k max output tokens. Three changes are breaking for existing Fable 5 callers, starting with forced tool use. Mythos 5.1 offers the same capabilities to Project Glasswing participants only. 

   
[NEW]
 Computer use, the Skills API, and the Files API are now generally available — plus a new browser use tool  · Computer use, the Skills API, and the Files API are now generally available on the Claude Platform, alongside a new browser use tool for web applications. Computer use now executes multiple actions per turn instead of one per model call and is eligible for HIPAA-regulated workloads under Anthropic's BAA. The Skills API gets a simplified upload and versioning interface, and the Files API adds automatic expiration, 5x higher rate limits, and 1 TB of storage per organization. 

   
[NEW]
 Auto mode becomes the default in Claude Code for Pro, Max, and Team plans  · Starting on August 14, new sessions on Pro, Max, and Team plans will run in auto mode. Instead of permission prompts, auto mode routes each tool call through a classifier targeted at blocking actions that are irreversible, destructive, or aimed outside your environment. Users can switch modes with Shift+Tab in the CLI or the mode dropdown on the desktop app, and admins can pin an org-wide default with defaultMode or turn auto mode off entirely with disableAutoMode. 

   
[NEW]
 Run Claude Code sessions on your own compute with self-hosted environments  · Now in public beta, self-hosted environments let you run Claude Code sessions on your own infrastructure. You operate runners in either fixed or on-demand mode, and each session runs in its own checkout so work stays isolated between developers and accounts. Repository checkouts, build artifacts, and secrets stay on infrastructure you provision, while the conversation is sent to Anthropic for inference. Available to organizations on Claude Team and Enterprise plans. 

   
[NEW]
 Bringing MCP 2026-07-28 to Claude — a stateless core, standardized extensions, and hardened auth  · MCP 2026-07-28, the fifth spec release of the Model Context Protocol, is live. It moves MCP from a bidirectional stateful protocol to a stateless request/response core so servers can deploy on serverless and edge infrastructure, graduates MCP Apps and Tasks into a versioned extensions framework, and aligns authorization with production OAuth 2.0 and OIDC deployments. Support is rolling out across Claude products. 

   
[NEW]
 Claude Opus 5  · Anthropic introduced Claude Opus 5 on July 24, 2026, for complex agentic coding and enterprise work. It is priced at $5 per million input tokens and $25 per million output tokens, the same as Opus 4.8, with the model ID claude-opus-5, a 1M token context window, and 128k max output tokens. Adaptive thinking is now on by default, effort runs up to max, and two betas ship alongside it: mid-conversation tool changes and automatic fallbacks. It is the new default model on Claude Max and the strongest model on Claude Pro. 

   
[NEW]
 Ask Claude about the Anthropic Economic Index  · A new connector lets you query the Anthropic Economic Index in claude.ai in plain language. Ask questions like "Which occupations use AI the most?" to explore AI adoption, task automation, and geographic usage patterns. There's nothing to install, but you do connect it once in the claude.ai connectors settings. 

   
[NEW]
 The iOS Simulator pane comes to Claude Code Desktop (public beta)  · Claude Code Desktop opens your app in an iOS Simulator pane when Claude builds, runs, or checks it, streaming the device screen live next to your conversation. Watch Claude install and tap through the app to verify its own changes, or tap through it yourself. It's in public beta on macOS, available on Pro, Max, and Team plans. 

   
[NEW]
 Introducing Claude for Teachers  · Claude for Teachers gives verified K-12 educators in the US free access to Claude, with teaching skills for lesson planning and differentiation, a Learning Commons of academic standards across all 50 states, and connectors to nine educational platforms. Sign up by June 30, 2027, for a full year of access. 

   
[NEW]
 Claude Cowork is coming to mobile and web  · Claude Cowork is rolling out to mobile and web, so your sessions and files go where you go, on any device. Work continues in the background, and when Claude reaches a call only you can make, the question reaches your phone. Beta access rolls out over the next several weeks, starting with Max users. 

   
[NEW]
 Introducing Claude Science  · Anthropic launched Claude Science in beta on June 30, 2026, an AI workbench that integrates fragmented scientific research tools into a unified environment. Researchers can run complete workflows — from literature analysis through manuscript preparation — in one platform, with specialist agents that query and synthesize across more than 60 curated scientific databases including UniProt, PDB, Ensembl, and ChEMBL. It's available to Claude Pro, Max, Team, and Enterprise users on macOS and Linux. 

   
[NEW]
 Introducing Claude Sonnet 5  · Anthropic released Claude Sonnet 5 on June 30, 2026, an agentic model that approaches Opus 4.8 performance at lower prices. It can make plans, use tools like browsers and terminals, and run autonomously, with benchmark gains over Sonnet 4.6 across coding, reasoning, tool use, and knowledge work. The model ID is claude-sonnet-5; it is the default model for Free and Pro plans and is available in Claude Code, the Claude Platform, and the Claude API. 

   
[NEW]
 Claude in Microsoft Foundry is now generally available  · Claude models are now generally available in Microsoft Foundry, hosted on Azure. Claude runs in your Azure environment with the authentication, billing, and governance controls your teams already use, and you can choose where inference is processed, including a US data zone for teams with data residency requirements. Claude Opus 4.8 and Claude Haiku 4.5 are available, with support for the Messages API, prompt caching, and extended thinking. 

   
[NEW]
 Introducing the Claude apps gateway — centrally manage Claude Code on Amazon Bedrock and Google Cloud  · The Claude apps gateway is a self-hosted control plane for managing Claude Code deployments on Amazon Bedrock and Google Cloud. It provides corporate SSO sign-in, centrally managed settings enforced on every request, and per-user cost attribution, and ships as a stateless container within the existing claude binary. It supports OIDC-based authentication, control over allowed models, and spend caps per organization, group, or user. 

   
[NEW]
 Introducing Claude Tag — work with @Claude in your Slack channels  · Claude Tag is a new way for teams to work with Claude. Tag @Claude in a Slack channel to delegate a task, and it completes work autonomously while keeping context across the channel's conversations. Everyone in a channel works with the same Claude, it learns from channel history and connected data, and you can hand off work asynchronously. Built on Opus 4.8 and available in beta for Claude Enterprise and Team customers. 

   
[NEW]
 Claude Code now supports artifacts — turn session work into live web pages  · Claude Code can now turn session work into artifacts — live, shareable web pages built from full session context. Ask for a PR walkthrough, dashboard, or incident timeline, and the open page refreshes in place when Claude Code updates it. Artifacts are private by default and viewable only by authenticated members of your org, available in beta for Claude Team and Enterprise orgs. 

   
[NEW]
 Claude Design now stays on brand for daily work  · Claude Design now imports your design system — from GitHub repos, design files, or raw uploads — so Claude builds with your components and validates against your system. New /design-sync and /design commands connect it to Claude Code, the editor gains rich layout controls and hundreds of stability fixes, usage limits are now shared with chat, Claude Cowork, and Claude Code, and connectors expand to Adobe, Base44, Canva, Gamma, Lovable, Miro, Replit, Vercel, and Wix. 

   
[NEW]
 Workload Identity Federation is now generally available on the Claude Platform  · Workload Identity Federation is generally available on the Claude Platform, letting developers authenticate with short-lived, scoped credentials from OIDC-compliant identity providers instead of static API keys. Federation rules bind external identities — AWS IAM, GCP, Azure, Kubernetes, GitHub Actions, Okta — to service accounts that issue tokens at request time across all Claude API endpoints. 

   
[NEW]
 Claude Fable 5 and Claude Mythos 5  · Anthropic released Claude Fable 5 and Claude Mythos 5 on June 9, 2026. Fable 5 is a Mythos-class model made safe for general use — state-of-the-art on nearly all tested benchmarks, with the `claude-fable-5` model ID. Mythos 5 is identical with cybersecurity safeguards lifted, available in limited release through Project Glasswing. Both support a 1M token context window and up to 128k output tokens by default, priced at $10/$50 per MTok. When a safety classifier declines a request, the Messages API returns stop_reason: "refusal" as an HTTP 200, and the fallbacks parameter (beta) or SDK middleware retries on another Claude model. 

   
[NEW]
 What's new in Claude Managed Agents — scheduled deployments and environment variables in vaults  · Two capabilities land in public beta on the Claude Platform. Scheduled deployments give an agent a cron schedule — each time the schedule fires, the agent starts a new session and completes its task, with no scheduler for you to build or host. Environment variables in vaults store API keys tied to specific approved domains: the agent never sees your key because the sandbox only holds a placeholder, and the real key is attached at the network boundary, only on requests to domains you allow. Supports the Browserbase, KERNEL, Notion, Ramp, and Sentry CLIs plus any CLI using HTTP authentication. 

   
[NEW]
 Claude in Apple's Foundation Models framework  · Anthropic released a Swift package that lets Apple developers use Claude through Apple's Foundation Models framework. The framework gives developers access to tap into models natively from Swift and can return typed Swift values through guided generation in as few as three lines of code. Apps can use on-device models for simple tasks, then hand off to Claude when workflows require multi-step reasoning or code generation, with streaming, tool calls, and structured responses flowing back into your SwiftUI view. Supports iOS 27, iPadOS 27, macOS 27, visionOS 27, and watchOS 27; an Anthropic API key is required. 

   
[NEW]
 Claude Opus 4.8  · Claude Opus 4.8 builds on Claude Opus 4.7 with improvements across benchmarks and is a more effective collaborator, available today for the same price. It targets long-horizon agentic coding with better long-context handling and compaction recovery, more reliable effort calibration, and better tool triggering. The model ID is claude-opus-4-8; it supports the 1M token context window by default on the Claude API, Amazon Bedrock, and Vertex AI, with 128k max output tokens. 

   
[NEW]
 Dynamic workflows in Claude Code  · Claude breaks a problem into subtasks and fans them out across tens to hundreds of subagents running in parallel, dynamically writing orchestration scripts that run in a single session and checking its work before anything reaches you. It targets broad work like codebase-wide bug hunts and security audits and large migrations spanning thousands of files, verifying each result before folding it into the final answer. Available as a research preview on Max, Team, and Enterprise plans. 

   
[NEW]
 Claude Managed Agents — Self-hosted sandboxes and MCP tunnels  · Claude Managed Agents now supports self-hosted sandboxes (public beta) and MCP tunnels (research preview). The agent orchestration loop stays on Anthropic infrastructure while tool execution runs in customer infrastructure or with managed sandbox providers — Cloudflare, Daytona, Modal, Vercel — and agents reach private-network MCP servers through a lightweight gateway with a single outbound connection, no inbound firewall rules required. 

   
[NEW]
 Agent view in Claude Code  · Agent view is a centralized interface inside the Claude Code CLI for managing multiple concurrent agent sessions. Now in Research Preview, it lets developers see which agents need input, are actively working, or have completed tasks — and scale up background work via peek, `/bg`, and `claude --bg`. 

   
[NEW]
 Claude Platform on AWS is now generally available  · The Claude Platform on AWS — Anthropic-operated, accessed via AWS IAM with CloudTrail audit logging and a single AWS invoice — is generally available. AWS customers get day-one access to all native Claude API features, including Claude Managed Agents (beta), Advisor strategy (beta), Files API (beta), Skills (beta), and the MCP connector (beta), with Claude Opus 4.7, Sonnet 4.6, and Haiku 4.5 available at launch. 

   
[NEW]
 What's new in Claude Managed Agents — Dreaming, Outcomes, Multiagent, Webhooks  · A month after the April 8 beta launch, Managed Agents added two new features and promoted three from research preview to public beta. Dreaming (research preview) curates memory between sessions for self-improvement, and outcomes, multiagent orchestration, memory, and webhooks all ship as public beta. 

   
[NEW]
 Claude Financial Services Agents — 10 templates, Moody's MCP, and 17 data connectors  · Ten ready-to-run financial services agents — pitch builder, KYC screener, month-end closer, and more — ship as plugins in Claude Cowork and Claude Code, and as cookbooks for Claude Managed Agents. A Moody's MCP app brings proprietary credit ratings and data on 600M+ companies, and 17 data partners are integrated alongside. 

   
[NEW]
 Claude for Microsoft 365 — Excel, PowerPoint, and Word now GA, Outlook joins in beta  · The Claude add-ins for Excel, PowerPoint, and Word are generally available on all paid plans, and Claude for Outlook joins in public beta. Once installed, context carries automatically between apps so a model built in Excel can become a PowerPoint deck or a Word memo without re-explaining inputs, assumptions, or calculation flow. 

   
[NEW]
 Claude Security Enters Public Beta  · Anthropic launched Claude Security in public beta — a Claude Opus 4.7-powered tool that scans codebases for vulnerabilities and generates targeted patches. Available directly at claude.ai/security and embedded in CrowdStrike, Microsoft Security, Wiz, and other platforms. Currently for Claude Enterprise customers; Claude Team and Max coming soon. 

   
[NEW]
 Claude Platform on AWS Announced (Coming Soon)  · AWS announced Claude Platform on AWS — a new way to use Anthropic's native Claude Platform with AWS credentials, billing, and access controls. It sits alongside Claude on Amazon Bedrock as a second path with different data-residency tradeoffs. 

   
[NEW]
 Running Claude Cowork in Amazon Bedrock  · AWS published the official guide to running Claude Cowork on Amazon Bedrock — reuse the same Bedrock infrastructure you built for Claude Code to bring Claude to every knowledge worker, inside your AWS perimeter. 

   
[NEW]
 Claude for Word  · A Microsoft Word add-in that brings Claude into your document — with native tracked changes, template-aware drafting, and consistency checks. 

   
[NEW]
 Claude Design by Anthropic Labs  · Collaborative visual creation with Claude — designs, interactive prototypes, presentations, and more through conversation. 

   
[NEW]
 Claude Opus 4.7  · A new Opus model with significant gains in coding, high-resolution vision, and instruction following. Claude Code adds /ultrareview and the xhigh effort level. 

   
[NEW]
 Routines — automated Claude Code workflows  · Autonomous routines in Claude Code triggered by schedules, API calls, and GitHub events. Run code review, bug fixes, and deploy verification from the cloud — laptop closed. 

   
[NEW]
 Claude Managed Agents  · A new API alongside Messages that provides a pre-built agent harness and managed infrastructure — run Claude agents without building your own loop, sandbox, or tool-execution layer. 

   
[NEW]
 Claude Code Remote Control  · Keep a local Claude Code session running and control it from claude.ai/code or the mobile apps — with your local files, MCP servers, and tools intact.
