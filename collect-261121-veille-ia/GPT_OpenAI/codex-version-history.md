---
id: collect-261121-veille-ia/gpt-openai/codex-version-history
title: "Codex version history"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["codex", "version", "openai", "release", "since", "history", "changes", "removed"]
source: https://developertoolkit.ai/en/codex/reference/changelog/
source_anchor: ""
source_lines: [1, 197]
section: "GPT_OpenAI"
---
The Codex version history lists the OpenAI Codex CLI releases from 0.95.0 (February 2026) to 0.162.1 (9 October 2026), with each releaseâs npm publish date, its main features, and the changes that break scripts or configuration, such as removed flags and retired models. Dates come from the npm registry; features come from OpenAIâs GitHub release notes.

This page is for the developer or tech lead who owns Codex in a pipeline, a script, or a shared team config. Codex ships a minor release roughly every week. A nightly `codex exec` job that installs `@openai/codex` without a version pin can fail one morning with `unexpected argument '--full-auto'`, and nobody on the team changed anything.

- The breaking changes since August 2026 in one table, each with its version, date, and replacement.
- A seven-step upgrade routine that proves a new version works before your CI depends on it.
- Two copy-paste prompts: one finds removed flags in your repository, one writes an upgrade note for your team.
- The release history by month, one line per release, so you can see what arrived between your version and the latest.

As of 12 October 2026:

| Item | Current value | How to check | 
|---|---|---|
| Codex CLI (npm `latest` ) | `0.162.1` , published 2026-10-09 | `codex --version` Â·`npm view @openai/codex version` | 
| Pre-release channel (npm `alpha` ) | `0.163.0-alpha.5` on 2026-10-12; changes almost daily, so do not pin it in CI | `npm view @openai/codex dist-tags` | 
| TypeScript and Python SDKs | `@openai/codex-sdk` 0.162.1 Â· PyPI`openai-codex` 0.162.1 (versions match the CLI since 0.155.0) | `npm view @openai/codex-sdk version` | 
| GitHub Action | `openai/codex-action@v1` (newest tag v1.13, 2026-10-05). Since v1.13, Linux`drop-sudo` requires`setfacl` (the`acl` package) and ACL support on`/run` | the actionâs tags on GitHub | 
| Default model | GPT-6.1 Sol since CLI 0.159.1 (2026-09-29), at `low` effort in Codex; GPT-6 Astra is priority 2 in the picker. Since 0.162.0, new TUI threads respect the model default the server sends, so the effective default for a signed-in account is still set server-side | `/model` in the TUI Â·`codex debug models --bundled` | 

For model prices, context windows, and effort defaults, see the models hub. This page names models only where a release changed what Codex runs.

## Which Codex changes break scripts and config?

Section titled âWhich Codex changes break scripts and config?â
Rows up to 0.157.1 were checked against the installed CLI, `codex-cli 0.157.1`, on 26 September 2026. Where the removal version is known, it comes from OpenAIâs release notes. Rows for 0.158.0 and later, and the dated rows at the end of the table, come from OpenAIâs release notes, OpenAIâs Codex documentation, and the stable `codex-cli 0.162.1` binary, all read on 12 October 2026.

| Change | Since | What breaks | Use instead | 
|---|---|---|---|
| `codex exec --full-auto` removed | 0.147.0 (2026-08-07) | `error: unexpected argument '--full-auto' found` | A permission profile, `-c default_permissions=":workspace"` (beta); or`--approve-for-me` ; or`--sandbox workspace-write` . Pick one: do not combine a profile with`--sandbox` or`--approve-for-me` ; OpenAI says the profile and legacy sandbox systems do not compose | 
| `codex mcp-server` removed | 0.154.0 (2026-09-09) | Codex can no longer run as an MCP server. In 0.157.1 the CLI reads `mcp-server` as a prompt and tries to open the TUI, so an MCP client that launches it fails with`stdin is not a terminal` | `codex exec` , the Codex SDK, or`codex app-server` (experimental).`codex mcp` only manages the servers Codex connects to | 
| `-a on-failure` and`-a untrusted` rejected on the command line | checked on 0.157.1; re-run on 0.160.1 (2026-10-10) and on 0.162.1 (2026-10-12) | `error: invalid value 'on-failure' for '--ask-for-approval'` ; 0.160.1 prints`error: invalid value 'on-failure' for '--ask-for-approval <APPROVAL_POLICY>'` and`[possible values: on-request, never]` ; 0.162.1 rejects both values with the same possible-values list | `-a on-request` or`-a never` . In`config.toml` ,`approval_policy = "on-failure"` still parses as`on-request` | 
| `-a` placed after`exec` | checked on 0.157.1; re-run on 0.162.1 (2026-10-12) | `codex exec -a never â¦` fails with`unexpected argument '-a'` | Drop the flag: `codex exec` already runs with approval`never` (`on-request` under`--approve-for-me` ), and a`codex -a â¦ exec` prefix does not change that | 
| Untrusted projects no longer supply project `AGENTS.md` | 0.150.0 (2026-08-26) | Codex silently ignores your repositoryâs instructions in a directory you have not trusted | Trust the project before relying on its `AGENTS.md` , and check the loaded instructions in a first run | 
| Planning tool off by default | 0.152.0 (2026-09-01) | Prompts or harnesses that expect `update_plan` output get none | Plan mode ( `/plan` ), or set`tools.update_plan.enabled = true` | 
| Default model changed to GPT-6 Astra | 0.153.4 (2026-09-04) | Cost, speed, and eval baselines shift for every run that does not set `model` | Pin `model` in`config.toml` or pass`-m` in CI, and re-run your evals before you move the pin | 
| GPT-5.4 retired; GPT-5.2 and GPT-5.4-mini removed from the bundled catalog | 2026-08-31 Â· 2026-09-09 | Configs that name them no longer get those models; GPT-5.4 users see âGPT-5.4 is no longer availableâ and move to GPT-6 Sol | Choose a current model from the models hub | 
| `--profile` reads`$CODEX_HOME/<name>.config.toml` | 0.134.0 (2026-05-26) made `--profile` the primary selector | Team docs that tell people to edit `[profiles.<name>]` describe the legacy form (still parsed in 0.157.1) | One file per profile, layered on the base config with `-p <name>` | 
| Feature flags `steer` ,`tool_search` ,`remote_control` ,`personality` retired | stage `removed` in 0.157.1`codex features list` | `--enable steer` or`--disable tool_search` has no effect | Nothing to toggle: the behaviour, where it still exists, is built in. The `personality` setting is deprecated and selects nothing | 
| GPT-5.4 removed from the bundled catalogs | 0.158.0 (2026-09-28) | In 0.157.1 GPT-5.4 was still a hidden, retired entry; it is now gone from the catalog. OpenAI retired it from Codex with ChatGPT sign-in on 2026-08-31, and the API is not affected | Choose a current model from the models hub | 
| Terminal input approval on by default | 0.158.0 (2026-09-28) | No error. Commands that run with elevated permissions go through terminal input approval by default ( `write_stdin_approval` is stable and on in 0.162.1; it was under development and off in 0.157.1) | Re-run the smoke test from step 5 of the upgrade routine under the approval policy your CI uses | 
| Bundled `plugin-creator` skill removed | 0.159.0 (2026-09-29) | A prompt, runbook, or plugin guide that calls the `plugin-creator` skill finds no such bundled skill. The 0.162.1 bundle holds`imagegen` ,`openai-docs` ,`review-agent` ,`skill-creator` , and`skill-installer` | The bundled `skill-creator` skill, or the âBuild pluginsâ flow in OpenAIâs docs | 
| `tui.prompt_suggestions` setting and automatic follow-up prompt suggestions removed | 0.159.0 (2026-09-29) | The setting no longer exists, so a shared `config.toml` that sets it carries a dead key | Delete the key | 
| Default model changed to GPT-6.1 Sol | 0.159.1 (2026-09-29) | Cost, speed, and eval baselines shift again for every run that does not set `model` ; Codex runs GPT-6.1 Sol at`low` effort by default | Pin `model` in`config.toml` or pass`-m` in CI, and re-run your evals before you move the pin. GPT-6 Astra (priority 2 in the 0.162.1 catalog) stays available for the hardest work | 
| `/daybreak` unavailable by default | 0.161.0 (2026-10-07) | Daybreak is opt-in: `daybreak = true` alone is not enough, and`/daybreak` does not appear | `--enable cli_daybreak` or`features.cli_daybreak = true` (stage: under development in 0.162.1) | 
| API-key model discovery on by default | 0.161.0 (2026-10-07) | No error. `api_key_model_discovery` was under development and off in 0.157.1 and is stable and on in 0.162.1, so a pipeline that logs in with an API key can behave differently from an older pin | Re-run the smoke test with the same login your CI uses | 
| `apply_patch` keeps CRLF line endings without an opt-in | 0.162.0 (2026-10-08) | The `apply_patch_preserve_line_endings` flag is at stage`removed` in`codex features list` because the behaviour is built in | Delete the flag from any `--enable` list | 
| New TUI threads follow the serverâs model and reasoning-summary defaults; the model picker no longer labels the default | 0.162.0 (2026-10-08) | The model a signed-in account starts on is the serverâs choice unless you override it at launch, so it can differ from the bundled GPT-6.1 Sol | Pass `-m` or set`model` in`config.toml` : an explicit launch override still wins | 
| GPT-5.5 retires from ChatGPT, ChatGPT Work and Codex | 2026-10-14, on all plans (OpenAI models page, checked 2026-10-12) | From that date `gpt-5.5` is no longer available in Codex; the OpenAI API is not affected. The 0.162.1 catalog still shows`retirement_at: null` for it, so the server enforces the date, not the binary | A current model: the 0.162.1 catalogâs upgrade prompt for GPT-5.5 points to GPT-6 Sol. See the models hub | 
| Slack integration is now `@ChatGPT` | OpenAI docs, checked 2026-10-12 | OpenAIâs Slack page is titled âUse ChatGPT in Slackâ: mention `@ChatGPT` , and with Cloud delegation enabled, repository work goes to a Codex Cloud task. Cloud delegation requires a published environment shared with the workspace. Runbooks that tell people to mention`@Codex` in Slack no longer match that page | Mention `@ChatGPT` in Slack. Linear still uses`@Codex` | 
| Chat Completions-only providers not supported | OpenAI docs, checked 2026-10-12 (the release that dropped them is not named) | OpenAIâs models page says âCurrent Codex releases donât support `wire_api = "chat"` or a Chat Completions-only endpointâ | A custom provider that supports the Responses API | 

Newer models also need newer CLIs: GPT-6 Astra needs Codex CLI 0.153.0 or later, and GPT-6 Sol and GPT-6 Luna need 0.155.0 or later. A pinned old CLI cannot select them, so move the CLI pin before the model pin.

The stable `codex-cli 0.162.1` binary, run on 12 October 2026, prints the same top-level `codex --help` as 0.157.1: no top-level flag or subcommand was added or removed, and `mcp-server` is still not in the list. The traps in this table still hold on 0.162.1 (checked 2026-10-12): `-a` accepts only `on-request` and `never`, `codex exec` has no `-a`, and `--full-auto` is rejected as an unexpected argument. There is still no `/loop` in Codex 0.162.1 (not in the slash-command enum at tag `rust-v0.162.1`, not in the binary, and not in OpenAIâs developer-commands reference). `mcp_2026_07_28` is still `under development` and off in `codex features list` on 0.162.1.

## How do you upgrade Codex without breaking CI?

Section titled âHow do you upgrade Codex without breaking CI?â
Treat a Codex upgrade like a dependency bump: one pull request, a pinned version, and a smoke test whose result you can read without reading the agentâs output line by line.

1. 
Record what you run today and what is available.
2. 
Read the release notes for every version in between. Each release lives at `https://github.com/openai/codex/releases/tag/rust-v<VERSION>` , for example`rust-v0.157.0` . Scan for âremovedâ, âdeprecatedâ, âdefaultâ, and âno longerâ. The second prompt below does this for you.
3. 
Search your repository for the removed flags in the table above. The first prompt below does this and proposes the replacements.
4. 
Install the new version on a branch, pinned to an exact version: On a workstation, `codex update` also updates an existing install to the latest version.
5. 
Run the diagnostics and make config drift fail loudly: `codex doctor` exits with status 1 when any check fails, so a CI step can gate on it. In a runner without outbound network access, the reachability check fails by design; run it where Codex can reach its provider.`--strict-config` stops on any`config.toml` field the new version does not recognise, instead of ignoring it.
6. 
Run your real automation once with structured output. If your job uses `codex exec --output-schema schema.json -o result.json` , compare`result.json` against the last known-good run: same keys, same verdicts on the same fixture. A schema-valid answer on a fixed fixture is your pass signal.
7. 
Bump the pin in CI in the same pull request. The engineer who owns the CI config approves the merge once steps 5 and 6 pass. If the nightly run fails after merge, revert the pin ( `npm install -g @openai/codex@<PREVIOUS_VERSION>` ) before you debug.

Replace `<PREVIOUS_VERSION>` with the version from step 1.

Start Codex with `codex --search` so it can read the release pages, then paste:

Replace `0.150.0` with your version from step 1. The prompt asks for a quotation behind every breaking change, so the reviewer checks each claim against its source sentence instead of trusting a summary.

Dates are npm publish dates (UTC). Each row lists the releaseâs headline changes, not every fix; the full notes are on the GitHub release page for that tag. Model and product events appear in the month they happened.

| Release | Date | Highlights | 
|---|---|---|
| 0.162.1 | 2026-10-09 | Fixes a TUI crash on multi-line asynchronous questions and startup failures when a running background serverâs feature settings differ from the CLI defaults | 
| 0.162.0 | 2026-10-08 | Managed Git worktree tools (create and list worktrees from trusted projects); `/copy` and`Ctrl+Insert` ;`tui.mouse_scroll_speed` ; clickable URLs; live web access and remote compaction for custom Responses-compatible providers; new TUI threads respect the serverâs model and reasoning-summary defaults;**CRLF preserved in `apply_patch` without an opt-in** ; Ultrafast service tiers for Amazon Bedrock Astra models | 
| Model | 2026-10-08 | Ultrafast speed tier for GPT-6.1 Sol in Codex (GPT-6 Astra has it in Codex and ChatGPT Work from the week of 2026-09-28). In Codex it needs the Pro plan at $500 per month or an eligible Enterprise or Edu plan, and uses 8x included usage or 6x credits (OpenAI speed page, checked 2026-10-12) | 
| 0.161.0 | 2026-10-07 | `/mcp login <name>` ;`codex exec --cyber-access-program` (experimental, per turn:`standard` ,`daybreak_blue` , or`daybreak_red` ) and the TypeScript SDK option`cyberAccessProgram` ;**Daybreak made opt-in** (`/daybreak` needs`cli_daybreak` ); API-key model discovery on by default; GPT-6.1 Sol restated as the default in the bundled and Amazon Bedrock catalogs; Bedrock multi-agent V2, Ultra reasoning, and GovCloud regions; microphone and speaker selection for voice | 
| 0.160.1 | 2026-10-05 | Fixes the environment variables ( `SYSTEMROOT` ,`TEMP` ,`TMP` ) passed to remote stdio MCP servers | 
| 0.160.0 | 2026-10-01 | Projectless sessions with workspace defaults; âShow moreâ in the agent command center; saved permissions restored on resume; opt-in Guardian (auto-review) context features; explicit provider model catalogs treated as authoritative | 

| Release | Date | Highlights | 
|---|---|---|
| 0.159.3 | 2026-09-30 | Optional account-security-setup reminders for ChatGPT sign-in | 
| 0.159.2 | 2026-09-30 | Fixes the console-window flash on Windows | 
| 0.159.1 | 2026-09-29 | **GPT-6.1 Sol becomes the bundled default model** , in the bundled and Amazon Bedrock Mantle and Runtime catalogs | 
| 0.159.0 | 2026-09-29 | Compact welcome screen; plan-review scrolling; Mermaid rendering; `.aws` protected under writable roots;**bundled `plugin-creator` skill and `tui.prompt_suggestions` removed** | 
| Model | 2026-09-29 | GPT-6.1 Sol released at DevDay 2026, with near-Astra performance at a lower cost, according to OpenAIâs Codex changelog. GPT-6 Astra Ultrafast reaches Codex and ChatGPT Work in the same week | 
| Product | 2026-09-29 | DevDay 2026: Codex Cloud is rebuilt around published, reusable cloud environments, and the earlier setup becomes âCodex Cloud (Legacy)â | 
| 0.158.0 | 2026-09-28 | Copy-on-select and right-click paste in the fullscreen TUI; `codex mcp add --oauth-client-secret` for pre-registered OAuth clients; bearer tokens for exec-server WebSockets;**terminal input approval on by default** for commands that run with elevated permissions;**GPT-5.4 removed from the bundled catalogs** ; sandbox fixes on Windows, Linux, and macOS | 
| 0.157.1 | 2026-09-26 | Patch release; OpenAI published no highlights | 
| 0.157.0 | 2026-09-25 | GPT-6 Sol and GPT-6 Luna across providers, including Amazon Bedrock, with migration prompts from older models; fullscreen transcripts by default; the background server starts automatically | 
| 0.156.1 | 2026-09-23 | GPT-6 Sol and GPT-6 Luna in the model picker; the rate-limit prompt recommends GPT-6 Luna | 
| 0.156.0 | 2026-09-22 | Optional fullscreen `/tui` ; voice on by default;`/usage` ; worktree support on by default (a session uses one only when you pass`--worktree` or run`/worktree` );`/daemon` | 
| 0.155.0 | 2026-09-17 | Experimental `/voice` ; live reasoning summaries; archive and delete tasks in the agents overview; Touch ID confirmation for MCP requests on Macs; Python SDK versions now match the CLI | 
| Model | 2026-09-14 | GPT-5.3-Codex-Spark retired: no longer available in the ChatGPT desktop app, the Codex CLI, or the IDE extension (OpenAI models page, checked 2026-10-12) | 
| 0.154.0 | 2026-09-09 | GPT-6 Astra in the picker and Bedrock catalogs; experimental `--worktree` ; inline answers while Codex works;**`codex mcp-server` removed** | 
| 0.153.3â0.153.4 | 2026-09-04 | GPT-6 Astra in the Bedrock picker; **GPT-6 Astra becomes the bundled default model** | 
| 0.153.0 | 2026-09-03 | `codex plugin` CLI with remote marketplaces;`tui.auto_recap = false` turns off automatic recaps; earlier allowance warnings for Plus and Team | 
| 0.152.0 | 2026-09-01 | **Planning tool off by default** ; rate-limit banners with credit actions; per-tool MCP`output_token_limit` | 

| Release | Date | Highlights | 
|---|---|---|
| 0.151.0 | 2026-08-29 | Grace period for optional MCP server discovery; extensions can rewrite MCP results; nested subagent tokens count toward goal budgets | 
| 0.150.0 | 2026-08-26 | `@` -mention other tasks;`Interrupt` hooks;**untrusted projects no longer supply project `AGENTS.md`** | 
| 0.149.0 | 2026-08-20 | `codex agents` dashboard to search, start, open, rename, and stop tasks;`/cd` ,`/pwd` ,`/cwd` ;`codex queue` ; broader`codex doctor` checks; SDK accepts`max` and`ultra` effort | 
| 0.148.0 | 2026-08-18 | `/export` to Markdown;`codex exec fork` ; estimated credits or cost in`/status` for eligible workspaces; built-in Amazon Bedrock provider; async hooks and MCP-tool hooks | 
| 0.147.0 | 2026-08-07 | Agent Plugins install; `--approve-for-me` (automatic approval review); opt-in MCP 2026-07-28 protocol, still under development and off in 0.157.1;**`codex exec --full-auto` removed** | 

| Release | Date | Highlights | 
|---|---|---|
| 0.146.0 | 2026-07-29 | Named and pinned sessions; side conversations; Agent Plugins manifests and workspace plugin publishing; admin controls for in-app updates | 
| 0.145.0 | 2026-07-21 | `/import` of settings, MCP servers, plugins, and sessions from Cursor and Claude Code; experimental Bedrock login; audio input; GPT-5.4 users migrated to GPT-5.6 Terra or Luna | 
| 0.144.6 | 2026-07-18 | Refreshed instructions for GPT-5.6 Sol, Terra, and Luna and corrected their context windows to 272,000 tokens | 
| 0.144.0 | 2026-07-09 | `writes` app-approval mode (read-only actions run, writes ask first); usage-limit reset credits now show their type and expiry and let you choose which one to redeem; a warning when Ultra reasoning with high multi-agent concurrency may use allowance quickly | 
| Model | 2026-07-09 | GPT-5.6 Sol, Terra, and Luna become generally available; they need Codex CLI 0.144.0 or later. The same day, OpenAI moved the Codex desktop app into the ChatGPT desktop app (secondary: Neowin) | 
| 0.143.0 | 2026-07-08 | Remote plugins on by default; system proxy (PAC and WPAD) support; `codex remote-control pair` ; MCP tool search on by default | 
| 0.142.5 | 2026-07-01 | Patch in the 0.142 line, which added redeeming usage-limit reset credits in `/usage` and configurable multi-agent delegation (0.142.0, 2026-06-22) | 

| Release | Date | Highlights | 
|---|---|---|
| 0.142.0 | 2026-06-22 | `/usage` shows and redeems usage-limit reset credits;`/plugins` groups remote plugins into OpenAI Curated, Workspace, and Shared; rollout token budgets across agent threads; multi-agent delegation set to disabled, explicit-only, or proactive; indexed web-search mode | 
| 0.141.0 | 2026-06-18 | End-to-end encrypted relay channels for remote executors; executor plugins can start their stdio MCP servers per thread; TUI prompts can auto-resolve after inactivity | 
| 0.140.0 | 2026-06-15 | `/usage` token views;`codex delete` and`/delete` ;`/import` from Claude Code;`@` opens the unified mentions menu by default; encrypted local storage for CLI and MCP OAuth credentials | 
| 0.138.0 | 2026-06-08 | `/app` hands the current CLI thread to the desktop app on macOS and Windows; saved image paths exposed to the model; plugin commands gain JSON output | 
| 0.137.0 | 2026-06-04 | F13âF24 keybindings; `codex remote-control` controller pairing; parallel standalone web search | 
| 0.136.0 | 2026-06-01 | `/archive` ,`codex archive` , and`codex unarchive` ; clickable links in TUI Markdown; alpha`codex sandbox setup --elevated` for Windows admins | 

| Release | Date | Highlights | 
|---|---|---|
| 0.135.0 | 2026-05-28 | Richer `codex doctor` diagnostics;`/status` shows remote connection details; Vim text objects;`/permissions` lists named permission profiles | 
| 0.134.0 | 2026-05-26 | Search across local conversation history; `--profile` becomes the primary profile selector; per-server environment targeting and OAuth options for MCP; read-only MCP tools run concurrently when they set`readOnlyHint` | 
| 0.133.0 | 2026-05-21 | Goals on by default; `codex remote-control` runs in the foreground; permission profiles gain inheritance and managed`requirements.toml` support | 
| 0.132.0 | 2026-05-20 | Python SDK authentication; `codex exec resume` accepts`--output-schema` | 
| 0.131.0 | 2026-05-18 | `codex doctor` ; one`@` picker for files, directories, plugins, and skills; Python SDK renamed to`openai-codex` | 
| 0.130.0 | 2026-05-08 | `codex remote-control` for a headless app server; plugin sharing controls; thread pagination for app-server clients; Bedrock auth from`aws login` profiles | 
| 0.129.0 | 2026-05-07 | Modal Vim editing ( `/vim` ); redesigned resume and fork picker; browse and toggle hooks in`/hooks` ; theme-aware status line | 

| Release | Date | Highlights | 
|---|---|---|
| 0.128.0 | 2026-04-30 | Persistent `/goal` workflows;`codex update` ; permission profiles with built-in defaults; plugin marketplace install and plugin-bundled hooks; import of sessions from external agents; explicit multi-agent v2 limits | 
| 0.124.0 | 2026-04-23 | `Alt+,` and`Alt+.` lower and raise reasoning effort; Amazon Bedrock support with AWS SigV4 signing; hooks stable and configurable in`config.toml` and`requirements.toml` ; Fast service tier by default on eligible ChatGPT plans | 
| Model | 2026-04-23 | GPT-5.5 ( `gpt-5.5-2026-04-23` ) released; Codex now labels it a legacy model | 
| 0.122.0 | 2026-04-20 | `/side` conversations; Plan mode shows context usage before implementation; deny-read filesystem globs;`codex exec --ignore-user-config` and`--ignore-rules` for isolated runs | 
| 0.121.0 | 2026-04-15 | `codex marketplace add` (now`codex plugin marketplace add` );`Ctrl+R` prompt history search; memory reset and deletion controls; secure devcontainer profile | 

| Release | Date | Highlights | 
|---|---|---|
| 0.117.0 | 2026-03-26 | Plugins become a first-class workflow ( `/plugins` , startup sync, install and remove); subagents get path-based addresses such as`/root/agent_a` | 
| 0.98.0 | 2026-02-05 | GPT-5.3-Codex support; steer mode stable and on by default ( `Enter` sends during a running task,`Tab` queues) | 
| 0.95.0 | 2026-02-04 | `codex app <path>` launches the desktop app on macOS; personal skills from`~/.agents/skills` ; shell tools run in parallel | 

## What breaks after a Codex upgrade, and how do you recover?

Section titled âWhat breaks after a Codex upgrade, and how do you recover?â
- **A CI job fails with `unexpected argument`.** A flag was removed or moved. Match the message against the breaking-changes table, apply the replacement, and re-run the smoke test from step 5. If the fix is not obvious within the hour, revert the pin and fix it in a branch.
- **An MCP client hangs or reports `stdin is not a terminal` when it starts Codex.** The client still launches`codex mcp-server` , which 0.154.0 removed. Drive Codex through`codex exec` or the SDK instead, and delete the server entry from the clientâs config.
- **The agent ignores `AGENTS.md` after the upgrade.** Since 0.150.0, an untrusted project supplies no project instructions. Trust the directory, then ask Codex in a fresh session to restate the repository rules it loaded; a missing rule shows up in the first answer.
- **Costs or run times jump with no config change.** The default model moved (GPT-6 Astra from 0.153.4, GPT-6.1 Sol from 0.159.1). Pin`model` explicitly, compare a week of`/status` or`/usage` numbers, and change the pin only after your evals pass.
- **`codex --strict-config` fails on a key you have used for months.** The new version no longer recognises it. Look the key up in the release notes, move it to its replacement, or delete it if it was a retired feature flag.
- **A model name fails on an older CLI.** GPT-6 models need 0.153.0 (Astra) or 0.155.0 (Sol and Luna). Upgrade the CLI first; do not work around it by running with a model the team has not evaluated.
- **A task that names `gpt-5.5` fails in Codex.** GPT-5.5 retires from Codex on 2026-10-14 (OpenAI models page, checked 2026-10-12). Move the pin to a current model from the models hub and re-run your evals first.
- **A custom provider stops working.** Current Codex releases do not support`wire_api = "chat"` (OpenAI models page, checked 2026-10-12). Point Codex at a provider that supports the Responses API.

The sibling changelogs track the other two tools the same way: Claude Code version history and Cursor version history. For the full, unabridged notes, read the Codex releases on GitHub.

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
