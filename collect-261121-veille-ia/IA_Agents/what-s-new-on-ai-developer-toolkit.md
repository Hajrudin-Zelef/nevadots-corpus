---
id: collect-261121-veille-ia/ia-agents/what-s-new-on-ai-developer-toolkit
title: "What's New on AI Developer Toolkit"
domain: ia-agents
role: reference
task: article
actors: []
dates: ["2026-10-14"]
keywords: ["sonnet", "claude", "haiku", "changes", "agent", "october", "developer", "changelog"]
source: https://developertoolkit.ai/en/resources/whats-new/
source_anchor: ""
source_lines: [1, 303]
section: "IA_Agents"
---
The AI Developer Toolkit changelog records the Claude Code, Codex and Cursor releases that change how an agent setup behaves (default models, removed flags, permission defaults), the fix for each, and every guide this site published. As of 12 October 2026, the changes needing action are the new Claude Sonnet 5.5, Haiku 5.5 and GPT-6.1 Sol, plus the retirements of GPT-5.5 in Codex (2026-10-14) and Claude Sonnet 4.5 (2026-11-30).

A guide written in July tells you to run `codex exec --full-auto`, and your CI job now fails with an unexpected-argument error. A teammateâs Claude Code session runs Opus 5 while yours runs Opus 5.5, and neither of you changed a setting. This page is for the developer or tech lead who owns a teamâs shared agent configuration (`CLAUDE.md`, `AGENTS.md`, `.claude/settings.json`, Codexâs `config.toml`, the CI jobs that call an agent) and needs to know which release caused the change and what to edit.

## What you get from this changelog as the owner of a teamâs agent config

Section titled âWhat you get from this changelog as the owner of a teamâs agent configâ
- Tables of the October and September 2026 changes that break or silently change a setup, each with the version that introduced it and the fix.
- A ten-minute check that tells you whether your machines and CI are behind this page.
- Three copy-paste prompts: one audits a repository for stale model IDs and removed flags, one applies the fixes on a branch with evidence, and one filters an upstream changelog down to what touches your config.
- A way to prove an upgrade did not break your agent workflow without reading every transcript.
- A dated record of what this guide shipped, so you can tell whether a page predates the release it describes.

## Which changes from October 2026 need action?

Section titled âWhich changes from October 2026 need action?â
Every row was checked on 2026-10-12 against the vendorâs changelog, documentation, release tags or the CLI binaries (`claude` 2.1.296, `codex-cli` 0.162.1; Cursor 3.24 from Cursorâs release notes), and covers 2026-09-27 to 2026-10-12. Prices and context windows live on one page, the AI model comparison for coding agents, so this table names models without repeating their prices. The September table further down stays as the record of 2026-09-26; where October changed a September row, the October row says so.

| Change | Since | What to do | 
|---|---|---|
| The `stable` channel catches up to v2.1.287 | `stable` 2.1.287 and`latest` 2.1.296 on 2026-10-12 | `AGENTS.md` reading (v2.1.277), the Opus 5.5 default (v2.1.280), auto mode as the starting mode (v2.1.283) and Sonnet 5.5 (v2.1.284) now apply on both channels, so a team on`stable` that missed the September changes gets them now. On 2026-10-12, changes from v2.1.288 on were on`latest` only; on 2026-10-17`stable` is v2.1.292, so changes from v2.1.293 on, such as the`haiku` alias, are the ones on`latest` only. Compare`claude --version` across machines. | 
| Claude Sonnet 5.5 is the `sonnet` alias on the Anthropic API | v2.1.284 (2026-09-28), both channels; Sonnet 5.5 needs v2.1.284 or later | Remove hard-coded `claude-sonnet-5` pins unless your evals need them; Sonnet 5 is now legacy. In Claude Code, Opus 5.5, Sonnet 5.5 and Haiku 5.5 all start at`medium` effort (Opus 4.7 at`xhigh` , other models that support effort at`high` ).`sonnet` still resolves to Sonnet 4.6 on Claude Platform on AWS and to Sonnet 4.5 on Bedrock, Google Cloud and Foundry. Auto modeâs classifier still runs on Claude Sonnet 5 by default, not on your`/model` choice. If your own API code runs Sonnet with thinking off, move to`thinking: {"type": "between_tools"}` first:`disabled` returns a 400 on Sonnet 5.5. | 
| Claude Haiku 5.5 is the `haiku` alias on the Anthropic API | v2.1.293 (2026-10-07), `latest` channel only | Haiku 5.5 needs v2.1.293 or later, so on `stable` (2.1.292 on 2026-10-17)`haiku` still means Haiku 4.5; Anthropicâs setup page says to move to`latest` to run a newly launched model. Other providers keep Haiku 4.5, now legacy. Haiku 5.5 has an effort setting (`medium` by default) and a 1M window on every plan with no`[1m]` suffix. Auto mode now supports Haiku 5.5, but not Haiku 4.5 or Sonnet 4.5 on any provider. Its price depends on prompt length, so re-price subagents that run on`haiku` from the models hub. | 
| Claude Sonnet 4.5 is deprecated and retires on 2026-11-30 | Deprecated 2026-09-30; retires 2026-11-30 on Anthropic-operated platforms | Anthropicâs deprecation page applies that date to the Claude API, Claude Platform on AWS and Microsoft Foundry; Bedrock and Google Cloud set their own dates. Claude Codeâs model-config page still names Sonnet 4.5 as Foundryâs default model and as the `sonnet` alias on Bedrock, Google Cloud and Foundry (checked 2026-10-12). On those providers, pin a model explicitly before the date; Anthropicâs recommended replacement is`claude-sonnet-5-5` . | 
| Ultracode is its own toggle | v2.1.284 (2026-09-28), both channels | Turning ultracode on or off with `/effort` or the`ultracode` setting leaves the effort level unchanged;`--effort ultracode` still sets`xhigh` . An effort cap no longer makes it unavailable: it stays on at the capped level, and Claude Codeâs model-config page lists only workflows turned off or a model without`xhigh` as reasons it is unavailable. Review any policy that used a cap to keep ultracode off. See dynamic workflows and ultracode. | 
| `claude -p` and Agent SDK sessions start in auto mode on third-party providers | v2.1.285 (2026-09-29), both channels | With no permission mode configured, `claude -p` and Agent SDK sessions on third-party providers, or with telemetry off, start in auto mode. Elsewhere they still start in Manual, as do sessions in an organization with the HIPAA configuration (from v2.1.285) and any session whose settings file sets`disableAutoMode: "disable"` . Pass`--permission-mode manual` in CI jobs that must not approve actions on their own. | 
| Unattended runs are stricter | `--bare` in v2.1.286 (2026-09-30); background-command limit in v2.1.285 (2026-09-29), both channels | `--bare` connects only the MCP servers named on the command line, sends no system reminders and starts no background tasks. Background shell commands stop after their`timeout` (default 30 minutes, maximum 2 hours); from v2.1.288 (both channels on 2026-10-17) that limit applies only in unattended sessions (`-p` , the Agent SDK, CI and cloud). Check CI jobs that use`--bare` or leave a dev server running in the background. | 
| `claude project purge` is now`claude purge` | v2.1.288 (2026-10-02), both channels on 2026-10-17 | The old name still works and prints a notice. `claude purge` deletes all Claude Code state for a project. Switch scripts once every machine runs v2.1.288 or later;`stable` is 2.1.292 on 2026-10-17. | 
| The earlier Claude in Slack app is retired | 2026-10-05 | In workspaces paired with a Claude organization, the earlier app (shown as Legacy in admin settings) no longer answers in channels; direct messages are not affected. Claude Tag (Public Beta; Team and Enterprise plans, Anthropicâs first-party service only) replaces it. Pro and Max accounts keep the earlier app only in workspaces no organization has connected to Claude Tag. See Claude Tag. | 
| Max and Team plans include monthly API credits | 2026-10-07 | Max 5x gets $100, Max 20x $200, and Team $20 per Standard seat or $100 per Premium seat, pooled and capped at $500. The credits cover the Claude API (Messages and Batches), Claude Managed Agents and the Claude Agent SDK. They do not cover interactive Claude Code in the terminal, IDE, desktop or web, and they do not change your usage limits. Claim them by linking one Claude Console organization; they do not roll over. Free, Pro and Enterprise plans are not eligible. | 

| Change | Since | What to do | 
|---|---|---|
| GPT-6.1 Sol replaces GPT-6 Astra as the bundled default | CLI 0.159.1 (2026-09-29); restated in 0.161.0 (2026-10-07) | CLIs before 0.159.1 still bundle GPT-6 Astra as the default; it held that role from 0.153.4 and is second in the picker now. Solâs default effort in Codex is `low` ; raise effort before you switch models. A`model` pin in`config.toml` overrides the default, so a pin to`gpt-6-astra` keeps Astra. Since 0.162.0, new TUI threads respect the serverâs model default unless you pass an explicit launch override. OpenAIâs Codex docs recommend Sol for complex coding and agentic work and Astra when a task needs the strongest capability across steps and tools. | 
| GPT-5.5 leaves ChatGPT, ChatGPT Work and Codex | 2026-10-14, all plans (OpenAIâs models page, checked 2026-10-12) | Change any `model = "gpt-5.5"` in`config.toml` or CI before that date, so the file says what runs. The OpenAI API is not affected. The bundled catalog in 0.162.1 carries no retirement date for GPT-5.5 (`retirement_at` is`null` ), so the date comes from OpenAIâs page, not from the binary. Codexâs own upgrade prompt for GPT-5.5 points to GPT-6 Sol. | 
| Codex in Slack is `@ChatGPT` | OpenAIâs Slack page, checked 2026-10-12 | The page is now titled âUse ChatGPT in Slackâ: mention `@ChatGPT` in a channel to start a request. With Cloud delegation enabled, ChatGPT can send repository work to a separate Codex Cloud task and return the result to the thread; that needs a published environment shared with the workspace. The`@Codex` sentence the page carried on 2026-08-28 is gone. Linear still uses`@Codex` and GitHub review still uses`@codex review` . Update runbooks that tell people to mention`@Codex` in Slack. See Slack and Linear. | 
| Codex Cloud environments are published and reusable | DevDay (2026-09-29); OpenAIâs cloud pages, checked 2026-10-12 | Create or select an environment, let Codex prepare your project, then review and publish it; new tasks start in isolated workspaces from the prepared filesystem. The earlier setup is now Codex Cloud (Legacy), which âcontinues to support environments for Code Review and the Linear and GitHub integrationsâ. Check which kind of environment each of your integrations uses. See cloud environments. | 
| The bundled `plugin-creator` skill and`tui.prompt_suggestions` are removed | CLI 0.159.0 (2026-09-29) | Remove prompts, docs and `config.toml` entries that call the`plugin-creator` skill or set`tui.prompt_suggestions` . The bundled sample skills in 0.162.1 are`imagegen` ,`openai-docs` ,`review-agent` ,`skill-creator` and`skill-installer` . | 
| Terminal input needs approval by default | CLI 0.158.0 (2026-09-28) | Terminal input approval is on by default for commands running with elevated permissions. Expect approval prompts for those commands, and test your attended and unattended flows after you update. | 
| Septemberâs removed flags are still removed | Checked on 0.162.1 (2026-10-12) | `codex --help` is identical to 0.157.1: no top-level flag or subcommand was added or removed.`--full-auto` is still an unexpected argument,`-a` still accepts only`on-request` and`never` ,`codex exec` still has no`-a` , and`mcp_2026_07_28` is still under development and off in`codex features list` . There is no`/loop` command in Codex 0.162.1. The September fixes still apply. | 

cursor.com was reachable on 2026-10-12, so these rows come from Cursorâs documentation, help pages and release notes. They take precedence over the September note below that Cursor could not be re-verified.

| Change | Since | What to do | 
|---|---|---|
| Version numbers live in Cursorâs release notes | Cursor 3.24 (2026-10-09) | The marketing changelog stopped printing version labels after 3.11 (2026-07-10); the release notes number every minor version of Cursor IDE and the Agents Window, and the CLI uses date versions (v2026.10.08). Read your version in the About dialog and compare it with Cursorâs release notes. 3.24 opens the Goal, queue, debug, canvas and browser panels from pills above the prompt, makes Auto-review stop risky deletes for your approval, and adds the managed policy `AgentRemoteWorkspaceAuthorities` , which lets admins restrict Agent to approved remote workspaces. | 
| Run modes have names: Auto-review, Allowlist and Run Everything | Auto-review has been the recommended default since 3.6 (2026-05-29) | Find them under **Settings > Agents > Approvals & Execution** . Auto-review runs allowlisted calls at once, runs other shell commands in the sandbox when possible and sends the rest to a classifier model. Cursorâs docs name that model as Gemini 3.5 Flash Lite with Claude 4.5 Haiku as the fallback, and say blocking Claude 4.5 Haiku âcan disable Auto-reviewâ. Cursor also says Auto-review is not a security boundary. Since 3.5, Ask Every Time is deprecated (use Allowlist with an empty allowlist) and Run in Sandbox is folded into Allowlist. Cloud Agents do not use run modes. See essential Cursor configuration. | 
| `AGENTS.md` and`CLAUDE.md` are read;`.cursorrules` is legacy | Cursorâs rules docs and help pages, checked 2026-10-12 | Cursor supports `AGENTS.md` in the project root and in subdirectories, with the more specific file taking precedence, and reads`CLAUDE.md` the same way, always applied. Project rules are`.mdc` files in`.cursor/rules` ; a plain`.md` file there is ignored. Cursorâs help says the`.cursorrules` file âis legacy and will be deprecatedâ: migrate it to a project rule set to Always Apply. Rules apply to Agent chat, not to Tab completion, Inline Edit or Bugbot reviews. See project rules. | 
| Claude Opus 5.5, Sonnet 5.5 and Haiku 5.5 are in Cursor; no GPT-6 model is | Opus 5.5 in the week of 2026-09-21, Sonnet 5.5 in the week of 2026-09-28 | Cursorâs release notes list Opus 5.5 and Sonnet 5.5 as new models, and cursor.com has a model page and a price for Haiku 5.5. Version 3.24 adds Opus 5.5 and Sonnet 5.5 on AWS Bedrock for bring-your-own-key setups. Cursorâs newest OpenAI models are GPT-5.6 Sol, Terra and Luna, and its models page lists no GPT-6 model (neither GPT-6.1 Sol nor Astra; checked 2026-10-12). Its own model pool is Grok 4.7, Grok 4.6, Grok 4.5 and Composer 2.5, and Cursor calls Grok 4.7 its flagship. Prices are on Cursorâs models and pricing page. | 
| The CLI command is `agent` | Cursorâs CLI docs, checked 2026-10-12 | Cursorâs docs name the command `agent` : check it with`agent --version` , update with`agent update` , and run headless with`agent -p` . The name`cursor-agent` appears on no page of Cursorâs docs, help or CLI changelog, so check the scripts and CI jobs that call it. | 
| Remote control for local agents | 2026-10-06 | You can see and reply to the agents running on your computer from the Cursor iOS app. It is on by default except for Enterprise organizations, where admins enable it at Org settings > Security and identity > Remote control. Cursor says remote control âdoesnât move your agents anywhereâ and doesnât require cloud agents. Check that the default fits your security policy. | 

Cursorâs `/goal` is still labelled ârolling outâ in its agent overview, and `/loop` is a built-in skill (checked 2026-10-12).

Three changes cut across tools:

- **MCP specification 2026-07-28.** Claude Code now also negotiates the new version by default with local (stdio) servers on every install, including Bedrock, Google Cloud and Foundry, from v2.1.292 on both release channels; set`MCP_PROTOCOL_NEGOTIATION=legacy` to opt out. Codex 0.162.1 still does not negotiate it (`mcp_2026_07_28` is under development and off, checked 2026-10-12). MCP Server Cards (SEP-2127), a format that describes a server before a client connects, became Final on 2026-10-06; the specification version is unchanged. Test your MCP server against both protocol versions. See MCP best practices.
- **GitHub Copilotâs four retirements happened on 2026-10-02.** GitHub removed Kimi K2.7 Code, Gemini 3.5 Flash, Gemini 3.6 Flash and Claude Opus 4.7 from its model tables. GitHubâs announcement suggests Claude Opus 5.5, not Claude Opus 5 as the cross-tool note in the September section below says, as the replacement for Opus 4.7. Copilot added Claude Sonnet 5.5 (2026-09-28), GPT-6.1 Sol (2026-09-29, not on the Pro plan) and Claude Haiku 5.5 (2026-10-07), and GitHubâs docs list no further retirement (checked 2026-10-12). See the GitHub Copilot comparison.
- **The official Terminal-Bench 4.0 leaderboard.** On the official board (read 2026-10-12), Claude Opus 5.5 at max effort in Claude Code scores 64.8% Â± 3.1 and Claude Sonnet 5.5 61.8% Â± 2.9; GPT-6 Astra and GPT-6.1 Sol, both at max effort in Codex, tie at 58.2% (Â± 2.8 and Â± 3.1). The Opus 5.5, Sonnet 5.5 and GPT-6.1 Sol rows were created on 2026-10-06. Correction: GPT-6 Astra has had rows on this board since 2026-09-03; the 2026-09-26 read of the GitHub submissions folder missed them. Vendor launch tables report different figures for the same models, so keep the two sources apart. See the benchmarks page.

## Which changes from September 2026 need action?

Section titled âWhich changes from September 2026 need action?â
Every row was checked on 2026-09-26 against the vendorâs changelog, release tags or the installed CLIs (`claude` 2.1.283, `codex-cli` 0.157.1). Prices and context windows live on one page, the AI model comparison for coding agents, so this table names models without repeating their prices. These tables are the record of 2026-09-26; the October section above says where a later release changed a row.

| Change | Since | What to do | 
|---|---|---|
| Claude Opus 5.5 is the default model on every paid plan and the API | v2.1.280, `latest` channel (2026-09-22) | Remove hard-coded `claude-opus-5` pins unless your evals need them. Opus 5.5 runs at`medium` effort by default: raise it with`/effort` before you switch models. | 
| The `stable` channel lags behind | `stable` was v2.1.274 on 2026-09-26 | On 2026-09-26 `stable` was v2.1.274: there Pro and Team Standard still defaulted to Sonnet 5, other plans to Opus 5, and Opus 5.5 was unavailable. Check where each channel is now with`npm view @anthropic-ai/claude-code dist-tags` . Put the whole team on one channel with`autoUpdatesChannel` . | 
| Claude Fable 5.1 replaces Fable 5 | v2.1.257 (2026-09-01) | The `fable` alias now resolves to Fable 5.1. Fable is never a default; select it deliberately with`/model fable` , and expect it to bill to usage credits on some plans. | 
| Auto mode is the starting permission mode | v2.1.283, `latest` channel (2026-09-25) | Interactive terminal and VS Code sessions start in auto mode. `claude -p` and the Claude Agent SDK still start in Manual. To keep Manual organisation-wide, set`permissions.disableAutoMode: "disable"` in managed settings. Sessions on a model auto mode does not support (for example Haiku, or Sonnet 4.5, the default on Microsoft Foundry) also start in Manual. | 
| `AGENTS.md` is read when a project has no`CLAUDE.md` | v2.1.277 (2026-09-18); v2.1.281 on Bedrock, Google Cloud, Foundry and gateways | Read the `AGENTS.md` in repositories without a`CLAUDE.md` : Claude now loads it. Toggle this under**Project instructions** in`/config` . | 

| Change | Since | What to do | 
|---|---|---|
| GPT-6 Astra is the bundled default | CLI 0.153.4 (2026-09-04); needs 0.153.0+ | Update CLIs older than 0.153.0. Astraâs Codex default effort is `low` ; raise effort before switching models. | 
| GPT-6 Sol and GPT-6 Luna join the picker | CLI 0.156.1 (2026-09-23); need 0.155.0+ | Use Sol for everyday work and Luna for cheap, high-volume tasks. There is no GPT-6 Terra: Terra exists only as `gpt-5.6-terra` . | 
| GPT-5.4 is retired in Codex | 2026-08-31 | A config that pins `gpt-5.4` is migrated to GPT-6 Sol. Change`model` in`config.toml` so the file says what actually runs. | 
| `codex exec --full-auto` is removed | CLI 0.147.0 (2026-08-07) | Use a permission profile ( `-c default_permissions=":workspace"` , beta),`--approve-for-me` , or`--sandbox workspace-write` . Do not combine a profile with`--sandbox` : OpenAI says the two systems do not compose. | 
| `-a` accepts only`on-request` and`never` | checked on 0.157.1 | Replace `-a on-failure` and`-a untrusted` . Put the flag before the subcommand:`codex -a never exec â¦` , because`codex exec` has no`-a` of its own. | 
| `codex mcp-server` is removed | CLI 0.154.0 (2026-09-09) | Codex can no longer run as an MCP server. Use `codex exec` , the Codex SDK, or`codex app-server` (experimental). Claude Code still offers`claude mcp serve` . | 

Cursorâs changelog and documentation could not be re-verified for September 2026: cursor.com was unreachable from the environment this page was checked in on 2026-09-26. The last verified Cursor facts date from 2026-08-28 and are summarised on the Cursor changelog page. Read Cursorâs own changelog before you act on any Cursor version, model or price claim, including ones on this site.

Two changes cut across tools:

- **MCP specification 2026-07-28** removed the`initialize` handshake and the`Mcp-Session-Id` header. Claude Code negotiates the new version by default with direct HTTP servers (all installs by v2.1.274); Codex 0.157.1 does not yet (`mcp_2026_07_28` is off in`codex features list` ). If you maintain an MCP server, test it against both protocol versions. See MCP best practices.
- **GitHub Copilot retires four models on 2026-10-02** : Kimi K2.7 Code (replaced by Kimi K3), Gemini 3.5 Flash and Gemini 3.6 Flash (replaced by Gemini 3.8 Flash), and Claude Opus 4.7 (replaced by Claude Opus 5). Update any Copilot model policy that names them.

## Check whether your setup is behind this page

Section titled âCheck whether your setup is behind this pageâ
Run these on a developer machine and in one CI job. The whole check takes about ten minutes and changes nothing.

1. 
Record the versions you actually run, in a terminal: For Cursor, read the version in its About dialog and compare it with Cursorâs release notes. On 2026-10-12 the current versions are Claude Code 2.1.296 ( `latest` ) and 2.1.287 (`stable` ), Codex 0.162.1 and Cursor 3.24; on 2026-10-17 Claude Code is 2.1.302 (`latest` ) and 2.1.292 (`stable` ).
2. 
Find the Claude Code release channel. Look for `autoUpdatesChannel` in`~/.claude/settings.json` and in managed settings;`"stable"` means you are about a week behind`latest` . On 2026-10-17`stable` is 2.1.292 and`latest` is 2.1.302. To choose a channel, set`autoUpdatesChannel` to`"stable"` or`"latest"` in settings;`claude install stable` installs the stable build now.
3. 
List the models your Codex binary knows and the state of its feature flags:
4. 
Run both health checks. They read your settings files and report problems before an agent run does:
5. 
Search the repository for the removed flags and legacy model pins from the tables, from the repository root: The last pattern matches `claude-opus-5` and`claude-fable-5` but not`claude-opus-5-5` . The pattern`gpt-5\.[45]` matches both`gpt-5.4` and`gpt-5.5` ; the retirement on 2026-10-14 does not affect the OpenAI API, so change only the pins that Codex reads.
6. 
Update on a branch, not on `main` :`claude update` and`codex update` , then repeat steps 1â4 and run the prompts below.

## Copy-paste prompts for keeping agent config current

Section titled âCopy-paste prompts for keeping agent config currentâ
Run the first prompt in Claude Code or Codex from the repository root. It reads and reports; it edits nothing, so you can run it with read-only permissions.

The second prompt applies the fixes and produces the evidence a reviewer needs, so the pull request can be approved on its checks.

The third prompt turns a long upstream changelog into the handful of entries that touch your configuration. Replace `2.1.292` (the `stable` version on 2026-10-17) with the version your team runs; `npm view @anthropic-ai/claude-code dist-tags` shows the current channel versions.

## How to prove an agent tool upgrade did not break your workflow

Section titled âHow to prove an agent tool upgrade did not break your workflowâ
Nobody on the team should have to read transcripts to approve a version bump. Make the upgrade a change like any other, with checks that fail loudly:

1. **Pin the versions CI installs.** Install an exact version, for example`npm install -g @anthropic-ai/claude-code@2.1.296` or`npm install -g @openai/codex@0.162.1` , so CI only changes when someone bumps the pin in a pull request.
2. **Run one fixed smoke task headlessly on both versions.** Pick a small task with a known correct outcome, such as adding a test for an existing function, and run it with`claude -p â¦ --output-format json` or`codex exec â¦ --output-schema schema.json` . The gate is that your test suite passes afterwards, not that the transcript looks right.
3. **Re-run your evals when the default model changes.** A new default model is a model switch you did not choose. Follow the 48-hour evaluation playbook for a new model and the evals for coding agents guide before you accept it for unattended work.
4. **Let the config owner sign off on evidence.** The tech lead who owns the shared configuration approves the bump when tests, lint, type checks and the smoke task are green on the new version. If something regresses, revert the pin; the previous version is one commit away.

## What breaks after an agent tool update, and how to recover

Section titled âWhat breaks after an agent tool update, and how to recoverâ
**Teammates see different default models.** Cause: some machines follow Claude Codeâs `stable` channel and others `latest`, or Homebrewâs `claude-code` cask (which tracks `stable`) sits next to a native install. On 2026-10-12 both channels default to Opus 5.5, but `haiku` resolves to Haiku 5.5 only from v2.1.293 (`latest`), and Microsoft Foundry still defaults to Sonnet 4.5. Recovery: set one `autoUpdatesChannel` for the team in managed settings, and pin a model with `--model` where a result must be reproducible.

**A CI job fails with âunexpected argumentâ or âinvalid valueâ.** Cause: a removed Codex flag (`--full-auto`, `-a on-failure`) or `-a` placed after `exec`. Recovery: apply the Codex rows of the tables above, then pin the Codex version so the next removal arrives in a pull request instead of in a failed deploy.

**Codex reports that GPT-5.4 is no longer available.** Cause: a `model = "gpt-5.4"` pin. Codex migrates the session to GPT-6 Sol. Recovery: set the model you mean in `config.toml`, or remove the pin and use the bundled default.

**The agent runs commands you expected to approve.** Cause: auto mode became the starting mode in v2.1.283, and a classifier now approves actions it judges safe. Since v2.1.285, `claude -p` and Agent SDK sessions on third-party providers, or with telemetry off, start in auto mode too. Recovery: start sessions with `--permission-mode manual`, or turn auto mode off organisation-wide with `permissions.disableAutoMode: "disable"` in managed settings. `auto` set in project settings is ignored, so a repository cannot switch it on for you.

**Claude follows instructions nobody wrote for it.** Cause: from v2.1.277 Claude Code loads `AGENTS.md` when there is no `CLAUDE.md`, so a file written for Codex now steers Claude too; on 2026-10-12 that holds on both release channels. Recovery: review that `AGENTS.md`, or add a `CLAUDE.md`, or switch the behaviour off under **Project instructions** in `/config`.

**A pinned model is about to be retired.** Cause: `gpt-5.5` leaves Codex on 2026-10-14, and Claude Sonnet 4.5 retires on 2026-11-30 on Anthropic-operated platforms while Claude Code still names it as the `sonnet` alias on Bedrock, Google Cloud and Foundry. Recovery: change the pin before the date, name the model you mean in `config.toml` or your Claude Code settings, and run the repository search from the ten-minute check.

**A headless Claude Code run loses an MCP server or a background process.** Cause: since v2.1.286 `--bare` connects only the MCP servers named on the command line, and since v2.1.285 background shell commands stop after their `timeout` (default 30 minutes, maximum 2 hours). Recovery: name every server the job needs on the command line, and give long-lived commands a longer `timeout`.

**A guide on this site contradicts the tables above.** Cause: the guide predates the release. Every page shows its last-updated date; if it is older than the release, trust this page and the per-tool changelog, and report the page so it gets fixed.

## Vendor releases in 2026 that changed agent setups

Section titled âVendor releases in 2026 that changed agent setupsâ
Only changes that alter defaults, remove options or add a model appear here; per-version detail is on the Claude Code changelog, Codex changelog and Cursor changelog pages. Dates marked âsecondaryâ come from press reports rather than the vendor.

- **Oct 9 Â· Cursor 3.24.** Auto-review stops risky deletes for your approval, admins can restrict Agent to approved remote workspaces with the managed policy`AgentRemoteWorkspaceAuthorities` , and Claude Opus 5.5 and Sonnet 5.5 run on AWS Bedrock with your own key.
- **Oct 9 Â· Claude Code v2.1.296** (`latest` ). Adds`CLAUDE_CODE_WORKFLOW_SUBAGENT_MODEL` to run every workflow agent on one model, and prices Sonnet 5.5 cache reads at the lowered rate in`/cost` , the status line and SDK cost figures.
- **Oct 9 Â· Codex CLI 0.162.1** (stable). Since 0.157.1 no top-level flag or subcommand was added or removed; 0.162.0 (Oct 8) added managed Git worktree tools and`/copy` .
- **Oct 8 Â· Claude Code v2.1.295** (`latest` ). Command and HTTP hooks gain`onFailure` ,`"continue"` by default or`"block"` .
- **Oct 7 Â· Anthropic: Claude Haiku 5.5.** The`haiku` alias on the Anthropic API from v2.1.293 (`latest` channel).
- **Oct 7 Â· Anthropic: monthly API credits for Max and Team plans.** They cover the API and the Agent SDK, not interactive Claude Code.
- **Oct 7 Â· Codex CLI 0.161.0.** Adds`/mcp login <name>` and`codex exec --cyber-access-program` , and restates GPT-6.1 Sol as the default in the bundled and Amazon Bedrock catalogs.
- **Oct 6 Â· Cursor: remote control for local agents.** See and reply to the agents on your computer from the Cursor iOS app.
- **Oct 6 Â· Official Terminal-Bench 4.0 leaderboard.** Eight rows are added, among them Claude Opus 5.5, Claude Sonnet 5.5 and GPT-6.1 Sol. GPT-6 Astra has had rows since Sep 3, which the 2026-09-26 read of the GitHub submissions folder missed.
- **Oct 6 Â· Anthropic: Claude Mythos 5.1 now requires verification.** The Cyber Verification Program grows to three tiers, and Mythos 5.1 is available only to organizations verified through Anthropicâs verification programs, which replaces the invitation-only access in the Sep 1 entry below.
- **Oct 5 Â· Anthropic: the earlier Claude in Slack app is retired** in workspaces paired with a Claude organization; Claude Tag replaces it.
- **Oct 2 Â· GitHub Copilot removes four models** : Kimi K2.7 Code, Gemini 3.5 Flash, Gemini 3.6 Flash and Claude Opus 4.7. GitHubâs announcement suggests Claude Opus 5.5 for Opus 4.7, not Claude Opus 5 as the cross-tool note in the September section says.
- **Oct 2 Â· Claude Code v2.1.288** (`latest` ).`claude project purge` becomes`claude purge` .
- **Oct 1 Â· Claude Code v2.1.287.** Adds Claude Mods, plugins that modify deeper behavior, and defaults Opus 4.7 and later and Fable to a 1M window on Bedrock, Google Cloud and Foundry. It is the`stable` channel on Oct 12.

- **Sep 30 Â· Anthropic deprecates Claude Sonnet 4.5.** It retires on Nov 30 on Anthropic-operated platforms; Anthropic names`claude-sonnet-5-5` as the replacement.
- **Sep 29 Â· OpenAI: GPT-6.1 Sol** (DevDay). Becomes Codexâs bundled default in CLI 0.159.1, and GPT-6 Astra moves to second in the picker. Codex CLI 0.159.0 removed the bundled`plugin-creator` skill.
- **Sep 29 Â· Claude Code v2.1.285.**`claude -p` and Agent SDK sessions on third-party providers, or with telemetry off, start in auto mode; adds the`allowedProviders` managed setting and`claude --desktop` .
- **Sep 28 Â· Anthropic: Claude Sonnet 5.5.** The`sonnet` alias on the Anthropic API from Claude Code v2.1.284, where ultracode also becomes its own toggle; Sonnet 5 moves to legacy.
- **Sep 28 Â· Codex CLI 0.158.0.** Terminal input approval is on by default, and GPT-5.4 leaves the bundled catalogs.
- **Sep 25 Â· Claude Code v2.1.283.** Auto mode becomes the starting permission mode for interactive terminal and VS Code sessions on every plan and provider (`latest` channel).
- **Sep 23 Â· Codex CLI 0.156.1.** GPT-6 Sol and GPT-6 Luna join the model picker, below GPT-6 Astra.
- **Sep 22 Â· Anthropic: Claude Opus 5.5.** Becomes Claude Codeâs default from v2.1.280; Opus 5 moves to legacy. Anthropic also raised five-hour usage limits on Pro, Max, Team and seat-based Enterprise.
- **Sep 21 Â· SpaceXAI: Grok 4.7.** Added to GitHub Copilot the same day; available in Cursor from day one (secondary: MarkTechPost).
- **Sep 18 Â· Claude Code v2.1.277.** Reads`AGENTS.md` when a project has no`CLAUDE.md` .
- **Sep 9 Â· Codex CLI 0.154.0.** Removes`codex mcp-server` .
- **Sep 4 Â· Codex CLI 0.153.4.** GPT-6 Astra becomes the bundled default.
- **Sep 2 Â· Google: Gemini 3.8 Flash** (secondary: The Register). In GitHub Copilot from Sep 3.
- **Sep 1 Â· Anthropic: Claude Fable 5.1 and Claude Mythos 5.1.** Fable 5.1 replaces Fable 5 in Claude Code from v2.1.257; Mythos 5.1 is invitation-only through Project Glasswing.

- **Aug 31 Â· Codex retires GPT-5.4** , migrating pinned sessions to GPT-6 Sol.
- **Aug 14 Â· Claude Code auto mode** becomes the starting mode on Pro, Max and Team.
- **Aug 7 Â· Codex CLI 0.147.0.** Removes`codex exec --full-auto` ; automatic approval review (`--approve-for-me` ) routes approvals through a reviewer model.
- **Aug 4 Â· Claude Code v2.1.222** removes`/ultraplan` from the command reference; use plan mode (`/plan` ). The same day OWASP published the GenAI LLM Top 10 2026.

- **Jul 28 Â· MCP specification 2026-07-28.** A stateless core without the`initialize` handshake; tools adopt it at different speeds (see the cross-tool notes above).
- **Jul 24 Â· Anthropic: Claude Opus 5.**
- **Jul 9 Â· GPT-5.6 Sol, Terra and Luna reach GA on Azure** (model version 2026-07-09); now labelled âolderâ in Codex. The Codex desktop app merged into the ChatGPT desktop app as a Codex mode (secondary: Neowin).

- **Jun 30 Â· Anthropic: Claude Sonnet 5.** Its announced price rise was later cancelled; current prices are on the models hub.
- **Jun 9 Â· Anthropic: Claude Fable 5 and Claude Mythos 5.**
- **Jun 1 Â· Claude Code v2.1.160.** The dynamic-workflow trigger keyword changes from`workflow` to`ultracode` .
- **Jun 1 Â· GitHub Copilot** replaces request-based billing with usage-based billing.
- **June Â· Windsurf renamed Devin Desktop** (secondary: creeta).

- **May 20 Â· Cursor 3.5** ships a bundled`/loop` skill (verified on 2026-08-28).
- **May 15 Â· Roo Code** extension shut down, per the Roo-Code README; the repository is archived.

Each entry links to the page it describes. Every page also carries its own last-updated date.

- **Oct 16 Â· Code-review guides updated from Anthropicâs 2026-10-15 article.** Anthropicâs Claude Code team reported how it layers review on its own repositories (How the Claude Code team does code review; its figures are self-reported). The guides now cover managed Code Review levels, merge gates and costs, a sharper`REVIEW.md` , answering every finding, review-round caps and blocking security review, with the live Code Review documentation re-checked on 2026-10-16: automated code reviews with Claude Code, security audits with Claude Code, reviewing an agentâs pull request, the review queue, AI code review bots, reading evidence instead of code, governance and autonomy and human in the loop.
- **Oct 12 Â· Docs re-checked against the October releases.** Pages that name models and tool versions were re-checked against Claude Code 2.1.296, Codex 0.162.1 and Cursor 3.24, with Claude Sonnet 5.5, Claude Haiku 5.5 and GPT-6.1 Sol covered across the guides and the models hub. The last-updated date on each page tells you whether it has been re-checked.
- **Oct 10 Â· Vibe Coding vs Agentic Engineering: 21 Terms Compared.** A free article that defines 21 names for working with AI against vibe coding, each with its origin and a dated, linked quote. The state of agentic engineering now says that Karpathy endorsed the name âagentic engineeringâ on 4 February 2026 rather than naming it on 30 April 2026, and the AâZ reference gained 16 entries.

- **Sep 26 Â· Documentation reorganised by role and lifecycle stage.** Role reading paths under Start here for developers, tech leads, CTOs and executives; the method in Method; cross-tool practice in Workflows; models, MCP, skills, plugins and observability under Ecosystem; and new leadership sections, Strategy and business and Engineering organization. Old URLs redirect to their new location.
- **Sep 26 Â· Model facts re-checked.** New model pages: new-model evaluation playbook, benchmarks and open-weight models. Pages are re-verified against the installed CLIs as they are rewritten; the last-updated date on each page tells you whether it has been.
- **September Â· The AI-native lifecycle.** The lifecycle section maps the artifact chain, plan, design, build, test, deploy, maintain and metrics onto Claude Code, Codex and Cursor.
- **September Â· Human-in-the-loop default changed.** Layered agent review plus human gates replaced blanket line-by-line review, which is now reserved for critical and regulated code.
- **September Â· Scorecards rebuilt.** The developer scorecard guide and CTO scorecard guide now measure lifecycle maturity.

- **Aug 28 Â· The Ladder.** Six free hub pages organise the documentation by autonomy level, from assisted coding to running a software factory.
- **Aug 28 Â· The State of Agentic Engineering, August 2026.** Third-party evidence, each number with publisher and date, on how much production code agents write, with the counter-evidence sourced the same way.
- **Aug 28 Â· Claude Code routines** and**Cursor cloud agents and automations.** Unattended cloud runs in both tools: triggers, scoping and the untrusted-input rule.
- **Aug 24 Â· Compound engineering.** Everyâs loop as a cross-tool guide, with an audit of the plugin behind it.
- **Aug 20 Â· The AI Developer Toolkit MCP server.** A public, read-only endpoint with`search` and`fetch` across every guide on the site.
- **Aug 19 Â· Anti-slop writing skills.**`unslop` and`miodkuj` , including the deterministic checks that run in CI without an agent.
- **Aug 13 Â· `/goal` and `/loop` corrected.** Tool support re-verified: Cursorâs`/loop` skill and Codex goals being on by default had been missed.
- **Aug 12 Â· Software factories** and**Dr. Skill.** When a factory works and when it erodes the codebase; a read-only audit of your skill and MCP loadout.
- **Aug 6 Â· Answer-first rewrite.** Every article opens with a paragraph that answers its title, enforced by the build.
- **Aug 4 Â· Pruning context files.** An ablation protocol for cutting`CLAUDE.md` , Cursor rules and`AGENTS.md` .

- **Jul 30 Â· herdr, tmux for agent fleets and agent shells.** Running and supervising many agents from one terminal or shell.
- **Jul 27 Â· Claude Opus 5 across the model guides.**
- **Jul 25 Â· Mobbin MCP** and**Impeccable rebuilt for v4.**
- **Jul 21 Â· Taste Skill.** The anti-slop frontend skill library.
- **Jul 11 Â· Superpowers.** A skills plugin that enforces a design-to-PR workflow.
- **Jul 3 Â· Claude Code features you havenât tried.**

- **Jun 24 Â· `/grill-me`** and the first cross-tool`/goal` and`/loop` guides.
- **Jun 10 Â· Claude Fable 5** covered across the model and routing guides.
- **Jun 8 Â· Official skills** from 11 more publishers.
- **June Â· Reducing MCP token cost** and skill supply-chain security.

- **May 2026 Â·** Per-question guides for the developer and CTO scorecards, and bilingual scorecards.
- **April 2026 Â·** The four-stage AI design pipeline and the official skills guide.
- **February 2026 Â·** A full Codex section alongside Cursor and Claude Code, and Polish translations of every section.
- **17 July 2025 Â· Version 1.0.0.** The first release, covering Cursor and Claude Code.

## Where to go next with tool and model changes

Section titled âWhere to go next with tool and model changesâ
- The AI model comparison for coding agents holds the current prices, context windows and defaults this page links to instead of repeating.
- The per-tool changelog pages carry every version: Claude Code, Codex and Cursor.
- The troubleshooting guide covers failures that are not caused by a release.
- The Whatâs New page shows the newest guides as cards; send feedback when a page lags behind a release.

### Why does my teammateâs Claude Code session use a different default model?

Section titled âWhy does my teammateâs Claude Code session use a different default model?â
Claude Code ships on two release channels (`latest` and `stable`), and third-party cloud providers set their own model aliases. Compare `claude --version` and `autoUpdatesChannel` across machines, and check current channel tags with `npm view @anthropic-ai/claude-code dist-tags`.

### How do I check whether my agent setup is behind the current releases?

Section titled âHow do I check whether my agent setup is behind the current releases?â
Record `claude --version` and `codex --version`, run `claude doctor` and `codex doctor`, inspect bundled models with `codex debug models --bundled`, and search the repository for removed flags and legacy model IDs before updating on a branch.

### How do I know an agent tool upgrade did not break our workflow?

Section titled âHow do I know an agent tool upgrade did not break our workflow?â
Pin the CLI versions installed in CI, bump them in a single pull request, and require the test suite, lint, type checks, and a fixed headless smoke task to pass on the new version before merging.

## Did this guide help you?

One tap, no sign-in.


What will you do with it?

How did it read?

What got in the way? Pick any that apply

Your role?

Thanks. I read every answer. MichaÅ JaskÃ³lski

Did this guide help?

One tap each. Close whenever you like.

no sign-in

## Frequently asked questions

## Which Claude Code, Codex and Cursor changes from October 2026 need action?

 Claude Sonnet 5.5 became the `sonnet` alias in Claude Code v2.1.284 and Claude Haiku 5.5 the `haiku` alias in v2.1.293 (`latest` channel only). GPT-6.1 Sol replaced GPT-6 Astra as Codex's bundled default in CLI 0.159.1 on 2026-09-29, and GPT-5.5 leaves Codex on 2026-10-14. Claude Sonnet 4.5 retires on 2026-11-30 on Anthropic-operated platforms. Cursor 3.24 is current and documents its run modes and AGENTS.md support. 

## Which Claude Code and Codex changes from September 2026 need action?

 Claude Opus 5.5 became Claude Code's default model from v2.1.280 on the latest channel, and auto mode became the starting permission mode from v2.1.283. GPT-6 Astra became Codex's bundled default in CLI 0.153.4 on 2026-09-04, and Codex removed `codex exec --full-auto` (0.147.0) and `codex mcp-server` (0.154.0). 

## Why does my teammate's Claude Code session use a different default model?

 Claude Code has two release channels. On 2026-10-17 the latest channel is v2.1.302 and the stable channel is v2.1.292, and both default to Opus 5.5 (on 2026-09-26 stable was v2.1.274 and still defaulted to Sonnet 5 or Opus 5). Sessions still differ by version and provider: `haiku` resolves to Haiku 5.5 only from v2.1.293 on latest, and Microsoft Foundry still defaults to Sonnet 4.5 (checked 2026-10-12). Compare `claude --version` and the `autoUpdatesChannel` setting, and check where each channel is today with `npm view @anthropic-ai/claude-code dist-tags`. 

## How do I check whether my agent setup is behind the current releases?

 Record `claude --version` and `codex --version`, run `claude doctor` and `codex doctor`, list the models your Codex binary knows with `codex debug models --bundled`, and search the repository for removed flags and pinned legacy model IDs before updating on a branch. 

## How do I know an agent tool upgrade did not break our workflow?

Pin the CLI versions your CI installs, bump them in one pull request, and let the evidence decide: the test suite, lint and type gates, and a fixed headless smoke task must pass on the new version before the config owner approves the bump.
