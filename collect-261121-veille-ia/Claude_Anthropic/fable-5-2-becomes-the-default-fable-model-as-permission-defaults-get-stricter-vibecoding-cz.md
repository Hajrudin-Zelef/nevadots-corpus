---
id: collect-261121-veille-ia/claude-anthropic/fable-5-2-becomes-the-default-fable-model-as-permission-defaults-get-stricter-vibecoding-cz
title: "Fable 5.2 becomes the default Fable model as permission defaults get stricter | Vibecoding.cz"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["code", "fable", "model", "default", "claude", "mode", "permission", "defaults"]
source: https://www.vibecoding.cz/en/claude-code-weekly-2026-w46/
source_anchor: ""
source_lines: [1, 81]
section: "Claude_Anthropic"
---
*Defaults shifted under your sessions this week. The default Fable model changed, sandboxed Bash and read-only git commands in other repositories now ask before they run, and a model ID Claude Code does not recognize gets a 1M context window instead of 200K. Code Mode, an experimental way for Claude to script its own tool calls, arrived at the same time, and admins got new material for Google Cloud and the Claude apps gateway.*

Week 46 (November 9-13, 2026) brought versions 2.1.321 to 2.1.325: four releases with 552 changelog entries. Below are the changes that alter how your sessions behave, plus a handful of smaller ones worth knowing.

## What changes when Fable 5.2 becomes your default

**A new default model.** Claude Fable 5.2 (`claude-fable-5-2`) is now the default Fable model. It comes with a 1M context window and costs $8/$40 per Mtok, with cache reads at $0.20/Mtok. The best-practices docs now point to it for `/model fable`, and they list it at medium default effort, the same as Opus 5.5, Sonnet 5.5 and Haiku 5.5. Medium is described there as plenty for everyday feature work with a clear scope. Details are in the model configuration docs.

**Your saved pick may already have moved.** If you saved Fable 5.1 as your model, that pick now moves to `fable`. The timing depends on your plan:

- On Pro and Max plans, the move happens at once.
- On Team and Enterprise plans, it happens once your organization allows it.

**Going back.** `/model claude-fable-5-1` switches you back to the previous model. If you compare outputs between the two, pin the model explicitly so the comparison does not depend on what `fable` resolves to that day.

## Changes for everyday work

**Code Mode lets Claude script its tool calls.** In Code Mode, Claude writes JavaScript that makes several tool calls in one step. It can run commands in parallel, chain results and return only the output it needs, instead of requesting each call and reading each result in turn. The feature is experimental and off by default, and the docs say plainly that it may change or be removed in any release. To enable it, set `CLAUDE_CODE_EXPERIMENTAL_CODE_MODE=1` in your shell or user settings (project settings are ignored) and confirm the first-run prompt. The Code Mode page has the full description. The conditions:

- Claude Code v2.1.323 or later on either release channel, and the native build.
- An interactive terminal session. It does not work with `-p` , the Agent SDK, VS Code, Desktop, cloud sessions, Remote Control, Chrome or computer use.
- A Claude subscription or Console account. Gateways, Bedrock, Google Cloud, Foundry and Claude Platform on AWS are excluded.
- Opus 5.5, Sonnet 5.5, Haiku 5.5 or Fable 5.2.
- Not available for HIPAA or zero data retention organizations. Windows needs Git for Windows.

**Code Mode runs have time limits and their own permission rules.** Foreground code stops after ten minutes. Background code can run up to two hours and works best in auto or `bypassPermissions` mode. In a Code Mode session, the auto mode classifier applies `hard_deny` and `soft_deny` to the code Claude writes, not to each tool call that code makes. `permissions.deny` rules still apply per call. If you maintain auto mode rules, check how they read against whole scripts.

**Administrators can block Code Mode but cannot enable it centrally.** Setting the variable to 0 in managed settings blocks it for everyone. Turning it on stays with the individual user.

**Four permission defaults got stricter in 2.1.325.** Read the permissions docs if any of your automation runs unattended:

- Sandboxed Bash commands follow the normal permission flow by default. Set `sandbox.autoAllowBashIfSandboxed: true` to keep running them without prompts. The Agent SDK docs show the same default flipped from true to false, and the false default needs Agent SDK v2.1.325 or later. If managed settings turn the sandbox on, set the same option in your highest-ranking managed source to keep the old behavior.
- Read-only git commands ask first in a repository that is not the projectâs or one you trusted. To run them without prompts, name the repository with `--add-dir` . The`additionalDirectories` setting does not do this.
- Scheduled tasks and `/loop` ask before running a skill that declares`allowed-tools` or hooks. Add`Skill(name)` to`permissions.allow` to run it unattended.
- In Desktop, IDE and SDK hosts, âAlways allowâ is offered as a rule alone only when saving it would allow that command again. A scheduled task that relied on such a rule can now wait for you.

The practical impact: a task that ran overnight last week can sit at a prompt this week without anything having failed. Look for that before you blame the task.

**Unknown model IDs now get a 1M context window.** The context window assumed for a model ID Claude Code does not recognize rises from 200K to 1M (2.1.323). Use `/autocompact` to compact earlier, or set `CLAUDE_CODE_DISABLE_1M_CONTEXT=1` to keep 200K. This matters for sessions on other vendorsâ models with a smaller window. Once such a session fills up, it can fail every turn until you run `/clear`, so set the variable before that happens. As of 2.1.325, `CLAUDE_CODE_MAX_CONTEXT_TOKENS` also sets the window for unrecognized model IDs that start with `claude-`, such as a gatewayâs `claude-sonnet-latest`. The context window docs cover the details.

## Technical notes

**Google Cloud.** A new docs page describes running Claude Code against Claude Models on Google Cloud (Anthropic managed). This is the Claude API, run by Anthropic and reached through your Google Cloud project, with Google Cloud credentials, IAM and Marketplace billing. Setup steps:

- Create Application Default Credentials with `gcloud` , or point`GOOGLE_APPLICATION_CREDENTIALS` at a service account key file.
- Set `CLAUDE_CODE_USE_ANTHROPIC_GOOGLE_CLOUD=1` ,`ANTHROPIC_GOOGLE_CLOUD_PROJECT` and`ANTHROPIC_GOOGLE_CLOUD_WORKSPACE_ID` (starts with`wrkspc_` ) in`~/.claude/settings.json` .
- Confirm with `/status` .
- Each Google or service account needs the `roles/claude.user` IAM role. No Anthropic account login is required.

Fast mode, Remote Control, cloud sessions, claude.ai MCP connectors, Channels, `/design-sync`, `/import` and server-managed settings are unavailable on this provider. For team sharing, the page suggests putting the env block in managed settings files on each developer machine and pinning model IDs so updates do not shift everyone to a newer model. The full walkthrough is in the setup docs.

**Advisor tool.** The advisor works on Claude Models on Google Cloud (Anthropic managed), the same as on Claude Platform on AWS. On both platforms it no longer needs the feature flag fetched from Anthropic, so variables that disable flag fetching do not turn it off.

**Fast mode on Vertex.** Fast mode is now available on Vertex in the global, us and eu regions (2.1.322). If Vertex refuses it, the session falls back to standard speed.

**Claude apps gateway.** The gateway config gains a top-level `login_notice` key. It shows developers up to 500 characters on the verification page and the terminal sign-in screen. Admins can also define a pooled budget, a single monthly limit that a whole group spends against, by adding `pooled_budget` to the request that sets the groupâs monthly cap. Two new settings control which pooled budget applies:

- `pooled_conflict_rule` is`default_order` (the default) or`priority_list` . It sets the order in which the gateway tries a developerâs pooled budgets.
- `pooled_priority_order` lists up to 50 group names, each once and spelled as your IdP sends it. It takes effect only with`priority_list` . Without it, the gateway ignores the list and logs a warning at boot.

All of this needs a recent gateway version. A gateway on an earlier version refuses to start when it finds these keys, so upgrade it before you edit the config. Developers on a gateway with an admin block who have no applicable cap can now see estimated spend under Spend (UTC) in `/usage`, for today, this week and this month.

**Plugins.** A plugin can declare `engines["claude-code"]` in `plugin.json`. Plugins outside their stated version range are not installed or loaded. `claude plugin validate` now also type-checks a pluginâs TypeScript hooks module (2.1.323).

**Sessions and remote.** Sessions on the same machine can now send each other about 100,000 messages without a prompt, up from about 20. The Claude Desktop Code tab limit rises the same way (2.1.323). `claude remote-control` accepts `--settings <file>`, which applies a settings file to every session it starts, and has a short alias, `claude rc` (2.1.321).

**Terminal.** Prompt suggestions now appear wherever the terminal shows them, and Tab accepts the suggested next prompt. Set `promptSuggestionEnabled: false` in `settings.json` to turn them off (2.1.325). `/rewind` has gained `/undo` as an alias for undoing Claudeâs file edits. When a model needs usage credits that are off or used up, an options menu offers to stop, switch to usage credits, or upgrade (2.1.323).

**VS Code.** VS Code shows a Claude Code trust prompt before the first session in an untrusted folder. If you decline, that folderâs settings and MCP servers stay unused. The live output of a running Bash or PowerShell command now appears in the chat under the commandâs card (both 2.1.323).

**Admin and cloud.** Code Review admin settings have a Download CSV button that exports review costs per repository by month, week or day (2.1.323). Cloud environments rename Trusted network access to Limited, and new environments start there with Allow package managers checked. Uploads to a project now accept up to 1,000 files and 2 GB at a time, with a single file up to 480 MB.

## What to take away

**If you work in the terminal.** Run `/model` and check what `fable` now resolves to. If you want Fable 5.1 for a while, use `/model claude-fable-5-1`. Watch for new prompts from sandboxed Bash and from read-only git commands in repositories outside your project, and use `--add-dir` for the ones you trust. Code Mode is optional. If you try it, expect the experimental caveats and remember that it only runs in an interactive native build session. Tab now accepts prompt suggestions, and `promptSuggestionEnabled: false` removes them.

**If you run parallel sessions and scheduled work.** List the scheduled tasks and `/loop` jobs that use skills with `allowed-tools` or hooks, and add `Skill(name)` to `permissions.allow` for the ones that should run unattended. Check any task that depended on an âAlways allowâ rule from Desktop, IDE or SDK, because it can now wait for you. Sessions on the same machine can message each other far more freely now (about 100,000 instead of about 20 without a prompt). If you run sessions on models with smaller windows, set `CLAUDE_CODE_DISABLE_1M_CONTEXT=1` before they fill.

**If you administer integrations and company deployments.** Decide whether your sandboxed setups need `sandbox.autoAllowBashIfSandboxed: true` in the highest-ranking managed source, and check that SDK-based agents are on v2.1.325 or later when you rely on the new false default. Set `CLAUDE_CODE_EXPERIMENTAL_CODE_MODE` to 0 in managed settings if you want Code Mode blocked. Upgrade gateways before adding `login_notice`, `pooled_budget`, `pooled_conflict_rule` or `pooled_priority_order`. For Google Cloud rollouts, grant `roles/claude.user`, pin model IDs, and plan around the missing features listed above. Check `plugin.json` engines declarations on plugins you distribute, since out-of-range plugins no longer load.

*Sources: the Claude Code changelog, versions 2.1.321 to 2.1.325 (released November 9-13, 2026), and the Claude Code documentation. Code Mode is experimental and off by default, and its availability and behavior may change in any release.*
