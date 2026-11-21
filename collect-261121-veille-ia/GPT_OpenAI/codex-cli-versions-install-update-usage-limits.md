---
id: collect-261121-veille-ia/gpt-openai/codex-cli-versions-install-update-usage-limits
title: "Codex CLI Versions: Install, Update, Usage Limits"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["codex", "usage", "install", "openai", "chatgpt", "update", "limit", "version"]
source: https://www.turboai.dev/blog/codex-cli-versions
source_anchor: ""
source_lines: [1, 192]
section: "GPT_OpenAI"
---
# Codex CLI Version Tracker: Environment Variables, Models & Features#

Published by Alex Sorokoletov

We analyze OpenAI's Codex CLI releases to document the environment variables, model IDs, and feature surface of each build. Because Codex is open source, every finding is grounded in the published source at the release tag and the official changelog.

**Tracking the other agents too?** See the Claude Code Version Tracker, OpenCode Version Tracker, Gemini CLI Version Tracker, and Amp Version Tracker.

## How to install Codex CLI#

The npm package is `@openai/codex`, not `codex`. `npm install codex` installs an unrelated package named `codex`, a static site and code documentation generator.

```
# npm (any OS)
npm install -g @openai/codex
# Homebrew (macOS)
brew install --cask codex
# Standalone installer, macOS and Linux
curl -fsSL https://chatgpt.com/codex/install.sh | sh
```
On Windows, run the standalone installer from PowerShell:

```
powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 | iex"
```
The standalone installers download from `releases.openai.com/codex` and fall back to GitHub Releases when that download fails. Set `CODEX_INSTALLER_USE_RELEASES_OPENAI_COM=false` to use GitHub Releases only. Each GitHub Release also has plain binaries, for example `codex-aarch64-apple-darwin.tar.gz` for Apple Silicon and `codex-x86_64-unknown-linux-musl.tar.gz` for x86_64 Linux. Run `codex` and pick **Sign in with ChatGPT** to use a ChatGPT plan, or set up an API key.<sup>[1]</sup>

## How to update Codex CLI#

Run `codex update`. It finds how Codex was installed and runs the matching command. If it cannot tell, it stops with "Could not detect the Codex installation method. Please update manually". `codex --version` prints the installed version, and `codex doctor` shows the update command for your install.<sup>[2]</sup>

| Installed with | `codex update` runs | 
|---|---|
| npm | `npm install -g @openai/codex` | 
| pnpm | `pnpm add -g @openai/codex` | 
| Bun | `bun install -g @openai/codex` | 
| Homebrew | `brew upgrade --cask codex` | 
| Standalone, macOS or Linux | `install.sh` again, with`CODEX_NON_INTERACTIVE=1` | 
| Standalone, Windows | `$env:CODEX_NON_INTERACTIVE=1; irm https://chatgpt.com/codex/install.ps1 \| iex` in PowerShell | 

## Codex usage limits, /status and /usage#

With **Sign in with ChatGPT**, Codex uses your plan's usage limits. The README lists Plus, Pro, Business, Edu and Enterprise plans.<sup>[1]</sup> Each limit is a time window that OpenAI's server reports; Codex names it by its length: `5h`, `daily`, `weekly`, `monthly` or `annual`.<sup>[3]</sup>

- **`/status`** shows each window as a bar, for example`5h limit` and`weekly limit` , with the percent left and the reset time:`(resets 3:40 PM)` . It also shows the session's token usage.
- **Warnings.** At 50%, 75%, 90% and 95% used, Codex says "Heads up, you have less than 25% of your weekly limit left. Run /status for a breakdown." On Plus and Team, the 5-hour window warns from 50% used; other plans and windows warn from 75% used.
- **At the limit** , the message starts with "You've hit your usage limit." and ends with "Try again at" plus the local reset time. The middle part depends on the plan: Plus gets links to upgrade to Pro or to buy credits at`chatgpt.com/settings/usage` , Pro gets the credits link, Free and Go get an upgrade-to-Plus link, and Business and Team are told to ask their admin.
- **`/usage`** opens**Usage: Account usage and resets.** It has**View analytics** (usage history) and**Redeem reset** , which lists the usage limit resets on your account with their expiry dates. A**Full reset** resets your current usage limits.`/usage` needs ChatGPT sign-in; with an API key Codex replies "Sign in with ChatGPT to use /usage."

This page does not list plan prices or limit sizes. OpenAI sets both per plan, and the server sends the windows that apply to your account.

## "MCP client for codex_apps timed out after 30 seconds"#

Codex waits 30 seconds for each MCP server to start. When a server takes longer, Codex shows:

```
â  MCP client for `codex_apps` timed out after 30 seconds. Add or adjust `startup_timeout_sec` in your config.toml:
  [mcp_servers.codex_apps]
  startup_timeout_sec = XX
â  MCP startup incomplete (failed: codex_apps)
```
For an MCP server that you added yourself, follow the notice: set `startup_timeout_sec` (seconds, decimals allowed) or `startup_timeout_ms` in that server's table. The Codex config.toml reference lists both keys with the other MCP server settings.

```
[mcp_servers.docs]
command = "npx"
args = ["-y", "@example/docs-mcp"]
startup_timeout_sec = 60
```
`codex_apps` is different. It is Codex's built-in connection to ChatGPT apps and connectors, added when you sign in with ChatGPT, and its 30-second timeout cannot be changed in v0.166.0. A `[mcp_servers.codex_apps]` table with only `startup_timeout_sec` makes Codex fail to start with "Error loading config.toml: invalid transport in `mcp_servers.codex_apps`" (issue #29396, open). If you do not use apps in Codex, turn the feature off to remove the server and the notice:

```
[features]
apps = false
```
*Sources: [1] Codex README, rust-v0.166.0 | [2] Codex update commands, rust-v0.166.0 | [3] Codex source at rust-v0.166.0*

## Version History#

Incremental tools Â· OpenAI Responses Â· Amazon Bedrock Â· patch over v0.167.0

A small patch over v0.167.0. It adds opt-in incremental tools on the regular OpenAI Responses path (#59832) and restricts the existing legacy incremental tools to Amazon Bedrock providers (#60040). Each change names a different backend, so the two provider paths move separately in the same release. No environment variables, model identifiers, or command names changed between the two versions.

`codex agents` command center Â· Vertex AI onboarding Â· backend switching Â· `/experimental` system proxy

v0.167.0 opens an agent command center with `codex agents --no-daemon`, adds Google Cloud Vertex AI setup to terminal onboarding via `gcp_setup_wizard`, and lets you switch between connected backends from the session picker without dropping a draft. Several fixes keep session state intact across a model-provider switch, and context compaction now accounts for output from failed responses and responses missing token-usage data.

`codex queue --resume` Â· `codex doctor --network-only` Â· keyboard transcript selection Â· resume & fork keep state

v0.166.0 adds keyboard-driven transcript selection with search to the TUI, a `codex queue --resume` command for unloaded threads, and `codex doctor --network-only` for focused connectivity diagnostics. A cluster of fixes makes resume and fork restore a thread's execution context: approval and permission settings, reasoning-token accounting, and spawned descendant threads. The standalone executor now ships in the canonical CLI and app-server release packages, and builds standardized on Node.js 24.22.1 and Rust 1.95.0.

Vertex AI (`model_provider = "gcp"`) Â· gpt-6.1-terra Â· memory v2 default Â· mid-conversation reasoning effort

v0.165.0 makes Google Cloud Vertex AI a selectable model backend (`model_provider = "gcp"`), so the host a Codex install talks to is now a config choice across more than one vendor; GPT-6.1 Terra is a separate addition to the model catalogs. Memory v2 becomes the default for fresh profiles and can import external-agent memories, supported models can change reasoning effort during a conversation, and the session picker filters by repository to navigate linked worktrees. Around the new backend the release extends the network proxy (AWS SigV4 signing, negotiated policy metadata on MCP calls) and managed configuration (environment-scoped hooks, loaders for custom config servers), and closes a marketplace URL validation gap that could send Git to an unchecked host.

`/presets` slots Â· Inline `/model` args Â· Credential masking Â· Enterprise MCP sign-in Â· Bedrock wizard default

Codex CLI v0.164.0 follows v0.163.0 with features you reach by keystroke: ten numbered `/presets` slots hold reusable prompts and slash commands, and inline arguments on `/model` and `/permissions` set the model, reasoning effort, speed, and permission profile in one line. A credential layer masks secrets during remote Bash commands and Zsh snapshot sessions, including custom-provider credentials; enterprise MCP connections authenticate through Codex account grants or a dedicated sign-in; the Amazon Bedrock setup wizard runs by default; and Windows proxy challenges answer with Negotiate or NTLM credentials. Compaction that fails no longer corrupts history, and a resumed session reads compressed rollouts and keeps its own saved settings. Images preview in the fullscreen transcript on Kitty or Sixel terminals.

Steer a running turn Â· Searchable `/config` Â· Parallel read-only tools Â· Permission-checked network proxy Â· Standard shell executor

Codex CLI v0.163.0 makes a running session something you act on: a follow-up steers the current turn, an interrupt stops active Code Mode cells and their nested tool calls, `/config` searches and applies preferences live, and `/daemon` restarts a background server and reconnects. Independent read-only tools run in parallel, and `/side` returns to an existing side conversation after switching threads. The network proxy checks permission before a DNS lookup, rejects duplicate Host headers, and blocks credential aliases from bypassing its inspection hooks, while Codex retires its bundled patched zsh for the standard shell executor.

TUI crash fix Â· background-server startup fix Â· patch on 0.162.0

Codex CLI v0.162.1 is a patch release over v0.162.0 that fixes two reliability problems. A multi-line asynchronous question no longer crashes the terminal UI, and the fix keeps line breaks and complete hyperlink destinations intact. Startup no longer fails when a resident background server's feature settings differ from the CLI's defaults; the compatibility check now fires only for feature overrides passed explicitly on the command line, so a server started earlier does not block a later CLI invocation whose defaults have drifted.

managed worktrees Â· `/copy` Â· clickable links Â· Guardian key scrubbing Â· codex-cloud-client

v0.162.0 lands managed Git worktree tools for trusted local projects (behind the worktrees feature flag), a /copy command with Ctrl+Insert for copying transcript text, and clickable URLs across approval headers, questions, and MCP prompts. Code Mode gets opt-in ranked tool search and streaming JavaScript helpers, custom Responses-compatible providers can configure live web access and remote compaction, and a new Guardian decisions key is protected from environment forwarding. A new crate, codex-cloud-client, is a native gRPC client for resuming and attaching to cloud threads, so a cloud thread can be resumed locally.

`/mcp login` Â· `cli_daybreak` opt-in Â· `--cyber-access-program` Â· sandbox + SQLite recovery fixes

v0.161.0 adds in-session MCP sign-in via `/mcp login <name>`, with enterprise auth that fails closed on config refresh and cleans up account-scoped grants. The Daybreak interface moves behind an opt-in `cli_daybreak` flag, a per-turn `codex exec --cyber-access-program` override lands alongside an SDK equivalent, and voice gains microphone input channel selection. GPT-6.1 Sol becomes the default model in the bundled and Amazon Bedrock catalogs (#49318, #49339). The bug-fix set covers broader-but-bounded filesystem escalation, inherited permissions for background tasks, Windows sandbox fixes for elevated terminals and PowerShell, buffered-input submission after paste, and earlier SQLite corruption recovery with database backups.

projectless sessions Â· Guardian review context Â· command-center pagination Â· queued-message resume Â· SQLite store fixes

v0.160.0 follows v0.159.3 with four user-facing additions and a batch of reliability work. Sessions can start outside a project directory with workspace defaults when policy permits, Guardian reviews gain opt-in retrieval of earlier user instructions and agent-handoff context, the agent command center paginates older tasks behind a keyboard-accessible Show more, and Linux X11 terminals get middle-click paste in fullscreen. The fixes stop queued messages from double-sending after a reconnect, preserve server provider and reasoning settings in the TUI, harden the Windows sandbox with PowerShell fallbacks and long-path repairs, and keep the local SQLite session store from stalling while reclaiming its unused space.

Windows console fix Â· background command spawns Â· account security reminders

v0.159.3 is a maintenance pair on the 0.159 line. v0.159.2 stops Codex from flashing a console window on Windows when it launches background and sandboxed commands. v0.159.3 lets eligible local sessions signed in with ChatGPT show optional reminders to finish account security setup. Both are backports onto the 0.159 line rather than work developed on it.

gpt-6.1-sol default Â· Amazon Bedrock catalogs Â· 0.159 backport

v0.159.1 is a patch on the 0.159 line: it makes GPT-6.1 Sol the default model in Codex's bundled catalog and adds the same model to Amazon Bedrock's Mantle and Runtime catalogs so AWS-routed teams get it on the same release. The same model is available on the OpenAI API path and the Bedrock path in this release.

`instant_interrupt` Â· TUI welcome screen Â· Windows console fixes Â· Seatbelt TLS Â· `.aws` protection

Mid-turn steering is the largest interaction change in v0.159.0: the opt-in instant_interrupt lets new input redirect Codex during a model response or a long code-mode call, alongside fixes that keep queued output and WebSocket continuations intact when the turn is steered. The TUI gets a compact welcome screen, in-turn tips, a keep-for-later action in the warnings viewer, wider native Mermaid coverage, and transcript copies that preserve Markdown tables and whitespace. Windows launches stop flashing stray console windows for MCP servers, code-mode hosts, and piped commands, with an embedded-mode fallback under restrictive launchers. Sandbox work leans toward remote and network-enabled execution: macOS Seatbelt profiles allow TLS trust evaluation, proxy requirements are honored, the exec-server can proxy permitted private IPs upstream, and .aws directories are protected by default under writable roots.

copy-on-select TUI Â· `--oauth-client-secret` Â· exec-server bearer tokens Â· codex-websocket-auth

v0.158.0 hardens the resident-runtime plumbing introduced in the 0.157 line. Direct exec-server WebSocket connections, including those through app-server, can now carry bearer tokens, and WebSocket auth was pulled into a dedicated codex-websocket-auth component. Command spawning (pipes, PTYs, shell snapshots, hooks) routes through a shared child-process launcher with a reap-only drop policy and fork-safe Linux descriptor cleanup. User-facing, the fullscreen terminal gains copy-on-select, right-click paste, and Markdown-preserving copy; MCP servers requiring pre-registered OAuth client secrets are reachable; image generation can request transparent backgrounds; and elevated commands prompt for approval by default. Sandbox fixes span Windows 10 paths, Linux nested writable roots, and macOS path aliases.

voice Â· fullscreen transcripts Â· GPT-6 Astra/Sol/Luna Â· `OPENAI_FILE_BLOB_UPLOAD_TIMEOUT`

This catch-up from v0.153.4 spans v0.154 through v0.157. Voice conversations ship with a Linux and Windows audio runtime, fullscreen transcripts became the default in v0.157.0 with Shift-click selection, worktree-isolated sessions and a background server (/daemon, --no-daemon) are supported, and /import reached remote and background-server sessions. GPT-6 Astra, Sol, and Luna are in the model picker, with Sol and Luna adding Amazon Bedrock support in v0.157.0. OPENAI_FILE_BLOB_UPLOAD_TIMEOUT raised the file-upload default from 60 seconds to 5 minutes.

gpt-6-astra default Â· Vim undo/redo Â· plugin marketplaces

v0.153.4 makes GPT-6-Astra the bundled default, the last step of a four-release rollout that moved it from API-only to Bedrock catalogs to the model picker. The feature release in the range, v0.153.0, added Vim undo and redo in the composer, remote-marketplace install and removal in the plugin CLI, an optional automatic recap (tui.auto_recap), and a disabled-by-default context_management.experimental_mode that activates token-budget context, history notes, and a new_context tool for eligible ChatGPT-plan sessions on the Codex backend.

Vim `/` `?` search Â· actionable rate-limit banners Â· per-tool MCP `output_token_limit` Â· Guardian review hardening

v0.152.0 (with 0.151.0) adds Vim / and ? search in the composer with match highlighting and n/N navigation, turns rate-limit banners into actions for checking usage, managing credits, resetting limits, and managing plans, and shows credential-refresh progress in the terminal and codex exec including Amazon Bedrock reauthentication. MCP tools gain a per-tool output_token_limit with resume-consistent truncation, server names accept :, @, /, and . for package-style naming, and extensions can inspect or replace a tool result before it reaches the model. The planning tool is now disabled by default. Beneath those, the range hardens the Guardian automatic approval-review path (context rollover, token budgets, distinct turn identities, shared transcript primitives) and sandbox correctness across macOS Seatbelt, the Windows sandbox, and remote executors.

`codex agents` dashboard Â· MCP 2026-07-28 Â· Bedrock provider Â· workload identity

v0.150.1 caps a four-release run (0.147 through 0.150 plus a backport) that turns Codex into a controller for many concurrent agent tasks: an interactive `codex agents` dashboard, `codex queue` for messaging local or remote sessions, and `@`-mentions to read, create, and message tasks from the terminal. Alongside it: portable Agent Plugins with catalog search, opt-in MCP 2026-07-28 protocol, a built-in Amazon Bedrock provider with GPT-5.6 routing, scriptable and Interrupt hooks, and session export, fork, and archive. A new workload-identity and federation surface forms underneath.

Thread fork/pin/name Â· Agent Plugin marketplaces Â· Code Mode over WebSocket

Codex CLI v0.146.0 is the first release tracked since the 0.144.3 baseline. It makes conversations first-class threads you can name with /new or /clear, pin, and fork with full paginated history, building on the experimental thread history, resume, and memories added in 0.145.0. Agent Plugins gain manifests, workspace publishing, and marketplaces for Amazon Bedrock and Claude Code, while 0.145.0's /import pulls settings, MCP servers, plugins, and memories out of Cursor and Claude Code. The app-server now connects to remote Code Mode hosts over WebSocket, standalone web search opens to custom providers, and reliability work honors proxies across every network path.

`178 env vars` Â· `40 model IDs` Â· baseline snapshot

The first tracked Codex CLI version, read from OpenAI's Rust source at tag `rust-v0.144.3`. 178 env vars and 40 model-ID strings cluster around sandboxing, a credential-brokering network proxy, cloud execution, and an app marketplace. The reference every later post diffs against.

## FAQ#

**Q: Why does `npm install codex` not install Codex CLI?**

A: The `codex` package on npm is an unrelated static site and code documentation generator. OpenAI's package is `@openai/codex`: run `npm install -g @openai/codex`.

**Q: How do I install Codex CLI on Windows PowerShell?**

A: Run `powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 | iex"`, or `npm install -g @openai/codex` if Node.js is installed.

**Q: How do I update Codex CLI?**

A: Run `codex update`. It runs `npm install -g @openai/codex`, `brew upgrade --cask codex` or the standalone installer, to match how Codex was installed.

**Q: How do I check my Codex usage?**

A: Run `/status` in Codex. It shows each usage limit window, such as the 5h and weekly limits, with the percent left and the reset time. `/usage` opens usage history and usage limit resets, with ChatGPT sign-in only.

**Q: What are Codex usage limit resets?**

A: Resets that your account holds and can redeem in `/usage` under **Redeem reset**. A full reset resets your current usage limits. Each reset shows its expiry date.

**Q: How do I fix "MCP client for `codex_apps` timed out after 30 seconds"?**

A: `codex_apps` is the built-in ChatGPT apps connection, and adding `[mcp_servers.codex_apps]` to config.toml fails with "invalid transport". Set `apps = false` under `[features]` if you do not use apps. For your own MCP servers, raise `startup_timeout_sec` in the server's table.

Codex CLI coverage

- 20 release analyses published.
- First: Codex CLI 0.144.3 â The Baseline Snapshot on July 14, 2026.
- Latest: v0.167.1 on November 10, 2026.
- Cadence: one analysis per release, with new releases checked hourly.
- Source of record: the official Codex CLI changelog.
