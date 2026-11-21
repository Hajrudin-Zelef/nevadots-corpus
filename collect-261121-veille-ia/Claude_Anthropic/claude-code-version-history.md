---
id: collect-261121-veille-ia/claude-anthropic/claude-code-version-history
title: "Claude Code version history"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-17"]
keywords: ["claude", "code", "version", "settings", "release", "stable", "model", "changes"]
source: https://developertoolkit.ai/en/claude-code/reference/changelog/
source_anchor: ""
source_lines: [1, 246]
section: "Claude_Anthropic"
---
The Claude Code version history lists Anthropicâs Claude Code CLI releases from 1.0.0 (22 May 2025) to 2.1.296 (9 October 2026): each milestoneâs date, its main features, and the changes that break scripts or settings, such as removed commands, ignored settings, and new default models. Dates and wording follow Anthropicâs official changelog.

This page is for the developer or tech lead who owns Claude Code in a pipeline, a hook, or a shared `.claude/settings.json`. Claude Code ships roughly one release per day. A Monday morning can start with a teammate asking why their session now opens in auto mode, why `CLAUDE_CODE_ENABLE_AUTO_MODE` stopped doing anything, or why this weekâs bill doubled, and nobody on the team changed a line of config.

## What you get from this Claude Code changelog

Section titled âWhat you get from this Claude Code changelogâ
- The current version on each release channel, and the command that tells you which one you run.
- A table of the 2026 changes that break scripts, hooks, and settings, each with its version and replacement.
- A seven-step upgrade routine with a pass signal you can read without reading the agentâs output.
- Two copy-paste prompts: one that finds removed settings in your repository, one that writes your teamâs upgrade note.
- A month-by-month record of the releases that mattered, from general availability to today.

Claude Code has two release channels. `latest` gets every release as soon as it ships. `stable` is âtypically about one week oldâ and skips releases with major regressions, according to Anthropicâs setup guide. The same guide warns that a newly launched model can need a Claude Code version newer than `stable` serves. Name the channel whenever you name a version: on 2026-10-17 every change from 2.1.293 on is on `latest` only.

| Item | Current value (2026-10-17) | How to check | 
|---|---|---|
| `latest` channel | `2.1.302` , released 2026-10-16; the release rows on this page end at 2.1.296 | `claude --version` Â·`npm view @anthropic-ai/claude-code dist-tags` | 
| `stable` channel | `2.1.292` , released 2026-10-06; npm, the native installer, and the Homebrew cask`claude-code` agree | the `stable` entry in the same`dist-tags` output | 
| Default model on `latest` and`stable` | Claude Opus 5.5 on every paid plan and the API (needs 2.1.280 or later; `stable` 2.1.292 meets that) | `/model` in a session | 
| `sonnet` alias on the Anthropic API | Claude Sonnet 5.5 since 2.1.284, on both channels | `/model` in a session | 
| `haiku` alias on the Anthropic API | Claude Haiku 5.5 since 2.1.293, on `latest` only (`stable` 2.1.292 predates it; before 2.1.293 the alias pointed to Haiku 4.5) | `/model` in a session | 
| Claude Agent SDK | npm `@anthropic-ai/claude-agent-sdk``0.3.302` Â· PyPI`claude-agent-sdk``0.2.168` | `npm view @anthropic-ai/claude-agent-sdk version` | 
| Release notes inside the tool | `/release-notes` opens an interactive version picker (since 2.1.92) | type it in a session | 

Model prices and the full per-provider default and alias table live on the models hub. This page names models only where a release changed which one you get. SDK usage is covered in the Claude Agent SDK guide.

## Which Claude Code changes break scripts and settings?

Section titled âWhich Claude Code changes break scripts and settings?â
Most releases only add things. The rows below are the 2026 changes that make something you already have stop working, or start behaving differently, without an error message.

| Change | Since | What breaks | Use instead | 
|---|---|---|---|
| `/vim` and`/tag` commands removed | 2.1.92 (2026-04-04) | Team docs and muscle memory point at commands that no longer exist | Vim mode lives under `/config` â Editor mode | 
| `workflow` keyword renamed to`ultracode` | 2.1.160 (2026-06-02) | The word âworkflowâ in a prompt no longer starts a dynamic workflow | `/effort ultracode` , or ask for a workflow in your own words | 
| `/agents` wizard removed | 2.1.198 (2026-07-01) | Onboarding docs that say ârun `/agents` â | Ask Claude to create the subagent, or edit `.claude/agents/` directly.`claude agents` now opens agent view | 
| `default` permission mode shown as**Manual** | 2.1.200 (2026-07-03) | Screenshots and runbooks use the old label; the config value `default` still works | Write âManualâ in docs; keep `default` or use`manual` in config | 
| `CLAUDE_CODE_ENABLE_AUTO_MODE` has no effect | 2.1.207 (2026-07-11) | Scripts that export it to opt in or out change nothing | To turn auto mode off, set `permissions.disableAutoMode: "disable"` | 
| `autoMode` rules no longer read from`.claude/settings.local.json` | 2.1.207 (2026-07-11) | Repository-resident classifier rules are silently ignored | Move them to `~/.claude/settings.json` | 
| Task tool `mode` parameter ignored | 2.1.212 (2026-07-17) | Agent definitions or SDK code that set it get the default behaviour | Subagents inherit the parent sessionâs permission mode, so set the mode on the session | 
| `/ultraplan` removed from the command reference | 2.1.222 (2026-08-04) | Runbooks that start with `/ultraplan` | Plan mode ( `/plan` ) | 
| `defaultMode: "bypassPermissions"` in project settings ignored | 2.1.257 (2026-09-01) | A checked-in `.claude/settings.json` no longer grants bypass mode, the same as`"auto"` | User or managed settings, or `--permission-mode` on the command line, and only in a disposable sandbox | 
| TaskOutput tool removed | 2.1.277 (2026-09-18) | `taskOutputMaxChars` and`TASK_MAX_OUTPUT_LENGTH` stop having any effect; hooks that match`TaskOutput` never fire | Claude reads a background taskâs output file with `Read` | 
| `AGENTS.md` read when there is no`CLAUDE.md` | 2.1.277 first-party; 2.1.281 on Bedrock, Google Cloud, Foundry, gateways, and telemetry-off sessions | A repository that only has `AGENTS.md` now loads instructions it ignored before | Review `AGENTS.md` as if it were`CLAUDE.md` , or turn it off under âProject instructionsâ in`/config` | 
| Default model moves to Opus 5.5 | 2.1.280 (2026-09-22) | Cost, speed, and eval baselines shift for every session that does not pin a model; Pro and Team Standard move from Sonnet to Opus | Pin `--model` in CI and re-run your evals before you move the pin | 
| Auto mode becomes the starting mode everywhere | 2.1.283 (2026-09-25) | Interactive sessions on Bedrock, Google Cloud, Foundry, and telemetry-off installs now start in auto mode (Pro, Max, and Team already did from 2026-08-14) | Set `permissions.defaultMode` to keep another mode.`claude -p` and the Agent SDK still start in Manual (from 2.1.285 that no longer holds on third-party providers or with telemetry off; see the 2.1.285 row) | 
| Ultracode no longer forces `xhigh` | 2.1.284 (2026-09-28) | A runbook that expects `/effort ultracode` to set`xhigh` , or picking another level to turn it off, gets neither: the toggle leaves the effort level unchanged, and an effort cap below`xhigh` no longer makes ultracode unavailable | Set the level with `/effort` as its own step.`--effort ultracode` and the Agent SDK value`effortLevel: "ultracode"` still turn it on and set`xhigh` . Turn it off with`/effort` or the`ultracode` setting | 
| `claude -p` and Python Agent SDK sessions start in auto mode on third-party providers or with telemetry off | 2.1.285 (2026-09-29) | A headless job that sets no permission mode used to start in Manual and now starts in auto, where a classifier decides what runs. Elsewhere `-p` and the Agent SDK still start in Manual | Set `--permission-mode` or`permissions.defaultMode` in every unattended job. A settings file with`permissions.disableAutoMode: "disable"` keeps Manual | 
| Background Bash and PowerShell commands stop after a time limit | 2.1.285 (2026-09-29); from 2.1.288 (2026-10-02) the limit applies only in unattended sessions | A dev server or watcher that Claude starts in the background is stopped when its `timeout` runs out (default 30 minutes, maximum 2 hours). The unattended sessions are`-p` , the Agent SDK, CI, and cloud | Set a `timeout` that covers the job, up to 2 hours. Start anything that must live longer outside Claude Code | 
| A custom `ANTHROPIC_BASE_URL` gets the 1M window | 2.1.285 (2026-09-29) | Claude Code uses a 1M-token window for Opus 4.7 and later, Sonnet 5 and later, and Fable behind your gateway, even when the gateway stops at 200K | Run `/autocompact 200k` when your gateway stops at 200K | 
| Project sandbox settings can no longer widen an admin sandbox | 2.1.285 (2026-09-29) | A project setting that loosened the administratorâs sandbox no longer does | Change the sandbox in managed settings, not in the project | 
| `--bare` is stricter | 2.1.286 (2026-09-30) | `--bare` connects only the MCP servers named on the command line, sends no system reminders, and starts no background tasks. A scripted call that relied on configured MCP servers loses their tools | Name every MCP server the call needs on its command line | 
| MCP URL prompts from servers on the 2025-11-25 protocol | 2.1.287 (2026-10-01) | Claude Code now handles URL prompts from a server on that protocol, and some servers no longer connect | The changelog entry says to add `"bareElicitationCapability": true` ; read the 2.1.287 entry in the official changelog for where it goes | 
| Opus 4.7 and later and Fable default to a 1M window on Bedrock, Google Cloud, Foundry, and the Claude apps gateway | 2.1.287 (2026-10-01) | Sessions there get a 1M-token window instead of 200K, with no `[1m]` suffix | Set `CLAUDE_CODE_DISABLE_1M_CONTEXT=1` to keep 200K | 
| `claude project purge` renamed`claude purge` | 2.1.288 (2026-10-02) | The old name still works and prints a notice, so a script that captures its output sees a new line | Call `claude purge` | 
| An interactive sessionâs WebSearch budget refills at 100 calls an hour | 2.1.290 (2026-10-05) | An interactive session that relied on the hard stop after 200 calls keeps searching as the budget refills. Non-interactive sessions ( `-p` , the Agent SDK) default to no refill | `CLAUDE_CODE_WEB_SEARCH_REFILLS_PER_HOUR` sets the rate (`0` turns refill off);`CLAUDE_CODE_MAX_WEB_SEARCHES_PER_SESSION` raises the cap | 
| Project settings lose two switches | 2.1.290 (2026-10-05) | A projectâs settings can no longer turn on Claude in Chrome, and repository settings can no longer set `CLAUDE_CODE_DISABLE_ATTACHMENTS` | Set both in user or managed settings | 
| `pyright` and more forms of`ps` ask for permission | 2.1.290 (2026-10-05) | Calls to them that ran without a prompt now ask first | Add an allow rule for the exact command when a job must run it unattended | 
| Local (stdio) MCP servers and claude.ai connectors negotiate protocol 2026-07-28 by default | 2.1.292 (2026-10-06) for local servers on every install, on both channels; 2.1.295 (2026-10-08, `latest` only) for claude.ai connectors on installs that fetch no feature flags | An older server may stop answering the negotiation | `MCP_PROTOCOL_NEGOTIATION=legacy` opts out on that machine; update the server | 
| Network (UNC) path reads can no longer skip the permission prompt | 2.1.292 (2026-10-06) | A PreToolUse hook approval or auto mode no longer bypasses the prompt for file reads from network (UNC) paths | Expect the prompt; Anthropicâs changelog lists this as a security fix | 
| Subagents preload at most 32 skills | 2.1.295 (2026-10-08, `latest` only) | A subagent whose `skills` list names more than 32 preloads only 32 | Trim the list or split the work across subagents | 
| MCP size limits changed | 2.1.295 (2026-10-08) and 2.1.296 (2026-10-09), both `latest` only | WebSocket MCP messages over 16 MiB close the connection. Tool descriptions loaded through tool search are cut at 16,384 characters (was 2,048). From 2.1.296 the default limit on descriptions sent up front and on MCP server instructions is 4,096 characters (was 2,048) | Keep messages under 16 MiB and check that your server instructions still fit | 
| Cost figures use the lower Sonnet 5.5 cache-read rate | 2.1.296 (2026-10-09, `latest` only) | `/cost` , the status line,`--max-budget-usd` , and SDK cost figures price Sonnet 5.5 cache reads at the rate Anthropic cut on 2026-10-07 (the models hub has the price). A cap or alert tuned on the earlier figure now reads lower for the same work | Recalibrate budget caps and cost alerts from the new figures | 

Rows up to 2.1.283 were checked against the Claude Code 2.1.283 changelog on 2026-09-26. Rows from 2.1.284 on were checked against Anthropicâs changelog, model configuration page and permission modes page on 2026-10-12, with `claude --version` reporting 2.1.296. On 2026-10-17 `stable` is 2.1.292 (npm dist-tags), so every row up to 2.1.292 is on both channels; rows from 2.1.293 are on `latest` only until `stable` passes their version.

## How do you upgrade Claude Code without breaking CI?

Section titled âHow do you upgrade Claude Code without breaking CI?â
Treat a Claude Code upgrade like a dependency bump: one pull request, an exact version, and a smoke test whose result you can read without reading the agentâs transcript. The fleet-wide rollout (pilot group, staged waves, managed settings) is covered in Claude Code upgrade procedures; this routine is the per-repository gate.

1. 
Record what you run today and what each channel offers.
2. 
Read the release notes for every version in between. Run `/release-notes` in a session, or open the official changelog. Scan for âRemovedâ, âChangedâ, âdeprecatedâ, âno longerâ, and âdefaultâ. The second prompt below does this for you.
3. 
Search your repository for the removed settings in the table above. The first prompt below does this and proposes replacements.
4. 
Install the new version on a branch, pinned to an exact version. In CI, use npm (Node.js 22 or later) and turn off the background updater so the job cannot move under you: On a workstation with the native installer, `claude install 2.1.296` installs that exact version, and`claude install stable` installs the current stable build; set`autoUpdatesChannel: "stable"` to keep background updates on that channel.
5. 
Check the installation and make the smoke test cheap and bounded: `claude doctor` reports the installed version, whether auto-updates are on, and settings problems, without starting a session. The second command proves authentication, model access, and JSON output in one call; the pass signal is the literal`OK` . Set`--model` to the model your pipeline pins, so the test proves that model is still reachable on the new version rather than whatever the default happens to be.
6. 
Run your real automation once against a fixed fixture. If a job uses `--output-format json` or`--json-schema` , compare the result with the last known-good run: same keys, same verdicts on the same input. Trigger one hook on a fixture file and check its side effect (the formatter ran, the log line exists). On 2.1.283,`/doctor prompt-audit` also flags instructions in`CLAUDE.md` , skills, agents, and commands that were written for older models.
7. 
Bump the pin in CI in the same pull request. The engineer who owns the CI config approves the merge once steps 5 and 6 pass. If the nightly run fails after merge, revert the pin ( `npm install -g @anthropic-ai/claude-code@PREVIOUS_VERSION` ) before you debug.

Replace `PREVIOUS_VERSION` with the version from step 1.

Replace `2.1.292` and `2.1.296` with your version from step 1 and the version you want. Both prompts ask for the exact line or a quotation behind every claim, so the reviewer checks each finding against its source instead of trusting a summary.

## How do you keep a team on a known Claude Code version?

Section titled âHow do you keep a team on a known Claude Code version?â
These controls work at three levels: user settings, managed settings, and environment variables. The first two keys belong in user settings on a workstation, as in this example; the managed ones apply when the whole organization must follow them, and the environment variables suit CI runners and machines where you distribute Claude Code yourself.

- `autoUpdatesChannel` picks the channel for background updates and`claude update` . Homebrew ignores it: the cask name picks the channel (`claude-code` tracks stable,`claude-code@latest` tracks latest).
- `minimumVersion` is a floor for updates, so moving to`stable` does not downgrade you from a newer`latest` build.
- `requiredMinimumVersion` and`requiredMaximumVersion` (managed) make Claude Code refuse to start outside a version range.
- `availableModelsMatch: "exact"` (managed, 2.1.283 or later) makes an`availableModels` entry allow only the model version it names, so a new model stays blocked until your evals have run on it.
- `DISABLE_AUTOUPDATER=1` stops the background check only;`DISABLE_UPDATES` (2.1.118 or later) also blocks`claude update` and`claude install` , for teams that distribute Claude Code through their own channel.

The enterprise integration guide shows how to deliver managed settings to every machine.

## What changed in Claude Code, month by month?

Section titled âWhat changed in Claude Code, month by month?â
Highlights only. Minor fixes are left out; the official changelog has every line.

On 2026-10-17, 2.1.292 is the `stable` channel and 2.1.302 is `latest`. The releases from 2.1.293 on are on `latest` only; the rows below end at 2.1.296.

| Release | Date | Highlights | 
|---|---|---|
| 2.1.296 | 2026-10-09 | `latest` on 2026-10-12.`CLAUDE_CODE_WORKFLOW_SUBAGENT_MODEL` runs every workflow agent on one model; cost figures price Sonnet 5.5 cache reads at the lower rate; the default limit on MCP tool descriptions and server instructions rises to 4,096 characters | 
| 2.1.295 | 2026-10-08 | **Hooks accept `onFailure: "block"`** for command and HTTP hooks; subagents preload at most 32 skills; claude.ai connectors negotiate MCP 2026-07-28 by default; the Claude apps gateway takes per-upstream`models` lists | 
| 2.1.294 | 2026-10-08 | Better judging of instruction-style `prompt` hooks on`Stop` and`SubagentStop` | 
| 2.1.293 | 2026-10-07 | **Claude Haiku 5.5 added; the `haiku` alias points to it** on the Anthropic API;`agentType` in the`subagentStatusLine` payload | 
| 2.1.292 | 2026-10-06 | `stable` on 2026-10-17.**Local (stdio) MCP servers negotiate protocol 2026-07-28 by default** (`MCP_PROTOCOL_NEGOTIATION=legacy` opts out); the Agent tool takes an`effort` parameter;`claude plugin install --marketplace <source>` | 
| 2.1.291 | 2026-10-06 | Two regression fixes: cloud permission answers, and the last messages lost on quit | 
| 2.1.290 | 2026-10-05 | `claude attach <name>` and`claude logs <name>` accept part of a session name; an interactive sessionâs WebSearch budget refills at 100 calls an hour;`/claude-api managed-agents-onboard` | 
| 2.1.289 | 2026-10-03 | Mods can spawn teammates with `agent.spawn` | 
| 2.1.288 | 2026-10-02 | **`claude project purge` renamed `claude purge`** ;`/code-review --max-findings` caps the findings reported (a number or`all` );`/autocompact` saved per model; Ctrl+F in agent view | 
| 2.1.287 | 2026-10-01 | `stable` on 2026-10-12.**Claude Mods added: plugins can change deeper behaviour** ; Opus 4.7 and later and Fable default to a 1M window on Bedrock, Google Cloud, Foundry, and the Claude apps gateway; Sonnet 5.5 can act as an advisor for Opus 4.7 and 4.8 | 

| Release | Date | Highlights | 
|---|---|---|
| 2.1.286 | 2026-09-30 | `--bare` connects only the MCP servers named on the command line; permission prompts show a counter such as â2 of 5â; commit guidance runs a project or user skill named`verify` before committing | 
| 2.1.285 | 2026-09-29 | `claude --desktop` ;`allowedProviders` managed setting;`claude -p` and Python Agent SDK sessions start in auto mode on third-party providers or with telemetry off; background shell commands stop after a time limit | 
| 2.1.284 | 2026-09-28 | **Claude Sonnet 5.5 added; the `sonnet` alias points to it** on the Anthropic API; ultracode becomes its own toggle that no longer forces`xhigh` ;`/mcp reconnect all` | 
| 2.1.283 | 2026-09-25 | **Auto mode is the starting mode on every provider** ;`deniedModels` and`availableModelsMatch` managed settings;`/doctor prompt-audit` | 
| 2.1.281 | 2026-09-23 | `AGENTS.md` support extended to Bedrock, Google Cloud, Foundry, LLM gateways, and telemetry-off sessions | 
| 2.1.280 | 2026-09-22 | **Claude Opus 5.5 added as the default Opus model** ; Pro and Team Standard default moves from Sonnet to Opus | 
| 2.1.278 | 2026-09-19 | Auto mode uses the server-side classifier by default for the API, Enterprise, cloud providers, and gateways; new `Auto mode server` row in`/status` | 
| 2.1.277 | 2026-09-18 | **`AGENTS.md` read when a project has no `CLAUDE.md`** ; TaskOutput tool removed; invisible Unicode characters stripped from prompts before sending | 
| 2.1.275 | 2026-09-17 | Skills and plugins enabled on claude.ai sync to terminal sessions | 
| 2.1.274 | 2026-09-17 | MCP 2026-07-28 negotiation with direct HTTP servers becomes the default on Bedrock, Google Cloud, Foundry, and telemetry-off installs (opt out with `MCP_PROTOCOL_NEGOTIATION=legacy` ) | 
| 2.1.269 | 2026-09-11 | `/output-style [name]` | 
| 2.1.259 | 2026-09-02 | `--permission-prompts none` for unattended`-p` hosts: anything that would prompt is denied, while the permission mode keeps deciding everything else | 
| 2.1.257 | 2026-09-01 | **Claude Fable 5.1 becomes the default Fable model** ;`bypassPermissions` in project settings ignored | 

| Release | Date | Highlights | 
|---|---|---|
| 2.1.251 | 2026-08-28 | `PreModelSwitch` and`PostModelSwitch` hook events;`/effort` saved per model; seat-based Enterprise default moves to Opus | 
| 2.1.248 | 2026-08-27 | `--restricted` removes the built-in tools that run commands or code | 
| 2.1.246 | 2026-08-25 | Auto mode tab in `/permissions` to view and edit classifier rules | 
| â | 2026-08-14 | Auto mode becomes the starting mode on Pro, Max, and Team (Anthropicâs âWhatâs newâ, week 32) | 
| 2.1.224 | 2026-08-07 | Cross-session `SendMessage` with`ListAgents` ; self-hosted environments (`claude self-hosted-runner` , Team and Enterprise); 200-subagent-per-session cap removed | 
| 2.1.222 | 2026-08-04 | **Ultraplan removed** | 

| Release | Date | Highlights | 
|---|---|---|
| 2.1.219 | 2026-07-24 | Claude Opus 5 added as the default Opus; `DirectoryAdded` hook event; Opus 4.7 removed from fast mode | 
| 2.1.212 | 2026-07-17 | Task tool `mode` parameter deprecated | 
| 2.1.210 | 2026-07-14 | The auto mode classifier defaults to Sonnet 5 | 
| 2.1.207 | 2026-07-11 | Auto mode on Bedrock, Google Cloud, and Foundry without opt-in; `CLAUDE_CODE_ENABLE_AUTO_MODE` has no effect;`autoMode` no longer read from`.claude/settings.local.json` | 
| 2.1.202 | 2026-07-06 | âDynamic workflow sizeâ setting | 
| 2.1.200 | 2026-07-03 | The `default` permission mode is labelled**Manual** | 
| 2.1.198 | 2026-07-01 | Claude in Chrome generally available; `/agents` wizard removed | 

| Release | Date | Highlights | 
|---|---|---|
| 2.1.197 | 2026-06-30 | Claude Sonnet 5 added and made the default on Pro, Team Standard, and Enterprise seats | 
| 2.1.183 | 2026-06-19 | Auto mode blocks destructive git commands ( `git reset --hard` ,`git clean -fd` ) when you did not ask to discard work | 
| 2.1.178 | 2026-06-15 | Skills in nested `.claude/skills` directories load when you work on files there | 
| 2.1.170 | 2026-06-09 | Claude Fable 5 added | 
| 2.1.169 | 2026-06-08 | `/cd` moves a session to a new directory;`disableBundledSkills` setting | 
| 2.1.160 | 2026-06-02 | Dynamic-workflow keyword renamed from `workflow` to`ultracode` | 

| Release | Date | Highlights | 
|---|---|---|
| 2.1.154 | 2026-05-28 | Claude Opus 4.8 added, default effort `high` | 
| 2.1.152 | 2026-05-27 | Auto mode no longer requires opt-in consent; skills can set `disallowed-tools` in frontmatter | 
| 2.1.136 | 2026-05-08 | `autoMode.hard_deny` for classifier rules that block unconditionally | 
| 2.1.118 | 2026-04-23 | `DISABLE_UPDATES` blocks every update path;`"$defaults"` keeps the built-in auto mode rules alongside yours | 
| 2.1.111 | 2026-04-16 | Claude Opus 4.7 and the `xhigh` effort level | 
| 2.1.92 | 2026-04-04 | `/release-notes` becomes an interactive version picker;`/vim` and`/tag` removed | 
| 2.1.89 | 2026-04-01 | `PermissionDenied` hook event;`"defer"` decision for`PreToolUse` hooks in headless sessions | 
| 2.1.83 | 2026-03-25 | `sandbox.failIfUnavailable` ;`CLAUDE_CODE_SUBPROCESS_ENV_SCRUB=1` strips credentials from subprocess environments | 
| 2.1.81 | 2026-03-20 | `--bare` for scripted`-p` calls: skips hooks, LSP, plugin sync, and skill directory walks (on 2.1.283 it also skips`CLAUDE.md` auto-discovery) | 
| 2.1.75 | 2026-03-13 | 1M context for Opus 4.6 by default on Max, Team, and Enterprise; the old `C:\ProgramData\ClaudeCode` managed-settings path removed | 
| 2.1.68 | 2026-03-04 | Opus 4 and 4.1 removed from Claude Code on the first-party API; pinned users moved to Opus 4.6 | 

| Release | Date | Highlights | 
|---|---|---|
| 2.1.63 | 2026-02-28 | HTTP hooks: POST JSON to a URL instead of running a shell command | 
| 2.1.49 | 2026-02-19 | `--worktree` (`-w` ) starts Claude in an isolated git worktree | 
| 2.1.45 | 2026-02-17 | Claude Sonnet 4.6 support | 
| 2.1.32 | 2026-02-05 | Claude Opus 4.6; fast mode followed in 2.1.36 (2026-02-07) | 
| 2.1.7 | 2026-01-14 | MCP tool search on by default | 
| 2.1.3 | 2026-01-09 | Release channel ( `stable` or`latest` ) toggle in`/config` | 
| 2.1.0 | 2026-01-07 | Skill hot-reload; `context: fork` for skills; wildcard Bash permission rules such as`Bash(npm *)` ; hooks in agent and skill frontmatter | 

| Release | Date | Highlights | 
|---|---|---|
| 2.0.51 | 2025-11-24 | Claude Opus 4.5; Claude Code for Desktop | 
| 2.0.20 | 2025-10-16 | Skills support | 
| 2.0.17 | 2025-10-15 | Claude Haiku 4.5; the Explore subagent | 
| 2.0.12 | 2025-10-09 | Plugin system: commands, agents, hooks, and MCP servers from marketplaces | 
| 2.0.0 | 2025-09-29 | Native VS Code extension; `/rewind` ; the Claude Code SDK renamed the Claude Agent SDK;`claude config` command removed | 
| 1.0.60 | 2025-07-24 | Custom subagents | 
| 1.0.38 | 2025-06-30 | Hooks | 
| 1.0.0 | 2025-05-22 | Claude Code generally available, with Sonnet 4 and Opus 4 | 

The 0.2.x releases before 1.0.0 were the research preview.

## What breaks after a Claude Code upgrade, and how do you recover?

Section titled âWhat breaks after a Claude Code upgrade, and how do you recover?â
- **Sessions open in auto mode and a teammate did not expect it.** Version 2.1.283 made auto mode the starting mode everywhere it is supported, and it is on both channels since`stable` reached 2.1.287. Set`permissions.defaultMode` in user or managed settings to keep another mode. To turn it off for the organization, set`permissions.disableAutoMode: "disable"` in managed settings; the old environment variable does nothing.
- **A headless job approves things it used to refuse.** From 2.1.285,`claude -p` and Python Agent SDK sessions on third-party providers or with telemetry off start in auto mode when no permission mode is configured. Pass`--permission-mode` in the job, or set`permissions.disableAutoMode: "disable"` to keep Manual.
- **A background server dies after 30 minutes.** From 2.1.285, a background Bash or PowerShell command stops when its`timeout` runs out (default 30 minutes, maximum 2 hours). Since 2.1.288 (both channels on 2026-10-17) the limit applies only in unattended sessions. Raise the`timeout` , or start the process outside Claude Code.
- **A scripted `--bare` call lost its tools.** From 2.1.286,`--bare` connects only the MCP servers named on the command line. Name each server the call needs.
- **Costs or run times jump with no config change.** The default model moved (Opus 5.5 from 2.1.280). Pin`--model` in CI, compare a week of`/usage` numbers, and move the pin only after your evals pass. The new-model playbook covers that evaluation.
- **A checked-in bypass or auto setting stops applying.** Project settings can no longer grant`bypassPermissions` (2.1.257) or`auto` . That is deliberate: a cloned repository should not raise its own permissions. Move the setting to user or managed settings on a disposable, network-restricted sandbox only.
- **Claude follows instructions nobody wrote for it.** Since 2.1.277 a repository with`AGENTS.md` and no`CLAUDE.md` loads`AGENTS.md` . Ask Claude in a fresh session to list the project instructions it loaded; a surprising rule shows up in the first answer. Fix the file or turn the behaviour off under âProject instructionsâ in`/config` .
- **A hook stops firing.** Check its matcher against removed tools (TaskOutput in 2.1.277) and renamed events, then run`claude doctor` . The hooks guide shows how to test a hook in isolation.
- **An MCP server that worked last week fails to connect.** From 2.1.274 every install, including Bedrock, Google Cloud, Foundry, and telemetry-off installs, negotiates MCP 2026-07-28 with direct HTTP servers, and an older server may not answer that negotiation. From 2.1.292 (both channels on 2026-10-17), local (stdio) servers negotiate it on every install too. Confirm with`claude --version` and`/mcp` , set`MCP_PROTOCOL_NEGOTIATION=legacy` or`MCP_SDK_GENERATION=v1` on the affected machine as a stopgap, and upgrade the server. A server on the 2025-11-25 protocol that stopped connecting after 2.1.287 may need`"bareElicitationCapability": true` , as the changelogâs 2.1.287 entry says.
- **A model name fails on an older CLI.** Haiku 5.5 needs 2.1.293, Sonnet 5.5 needs 2.1.284, Opus 5.5 needs 2.1.280, Fable 5.1 needs 2.1.257, and Sonnet 5 needs 2.1.197. Haiku 5.5 is on`latest` only on 2026-10-17. Upgrade the CLI first; do not work around it by running a model the team has not evaluated.

When the fix is not obvious within the hour, revert the pin and debug on a branch. The troubleshooting guide covers installation and authentication failures.

## Where to go next after a Claude Code upgrade

Section titled âWhere to go next after a Claude Code upgradeâ
The sibling changelogs track the other two tools the same way: Codex version history and Cursor version history. For every line of every release, read Anthropicâs Claude Code changelog.

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
