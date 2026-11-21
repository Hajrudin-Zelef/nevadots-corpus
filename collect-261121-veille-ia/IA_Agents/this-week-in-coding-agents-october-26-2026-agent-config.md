---
id: collect-261121-veille-ia/ia-agents/this-week-in-coding-agents-october-26-2026-agent-config
title: "This Week in Coding Agents: October 26, 2026 | agent-config"
domain: ia-agents
role: reference
task: article
actors: []
dates: ["2026-10-26"]
keywords: ["october", "claude", "gemini", "longer", "week", "permission", "coding", "agents"]
source: https://agent-config.com/this-week/2026-w44/
source_anchor: ""
source_lines: [1, 88]
section: "IA_Agents"
---
# This Week in Coding Agents: October 26, 2026

Week 44, 2026 · Published

| Claude Code | 2.1.310 | 
| Codex CLI | 0.164.0 | 
| Gemini CLI | 0.65.0 | 

## The short version

- Claude Code’s changelog lists 2.1.310 as the top release. npm latest is `2.1.310` . 2.1.310 adds the Monitor tool and plugin monitors on Bedrock, Vertex, Foundry, and when telemetry or nonessential traffic is off.`/auto-mode-setup` landed in 2.1.304. 2.1.309 makes`claude ultrareview` require workspace trust.
- Codex CLI 0.164.0 (October 23, tag `rust-v0.164.0` ) is the current stable. Save reusable prompts in ten`/presets` slots.`/model` and`/permissions` now take inline arguments. The Amazon Bedrock setup wizard is on by default. The releases list shows`0.165.0-alpha.1` as a prerelease above that stable tag.
- Cursor has no version number and no changelog entry in this window. The newest post I opened is still Cloud Agent Canvases and Environment Improvements from October 16.
- GitHub Copilot has no version number. On October 19 it dropped Gemini 3.7 Flash, GPT-5.5, GPT-5.4, GPT-5.4 mini, GPT-5 mini, and Grok 4.5. The usage metrics API now reports `totals_by_client` for tools such as OpenCode, Claude Code, and Codex. The global default for Copilot features is generally available, with enforcement rolling through October 26.
- Gemini CLI 0.65.0 shipped October 20. PR #29583 stops `gemini mcp add` and`gemini mcp remove` from writing`.gemini/settings.json` in an untrusted folder.

## Claude Code

2.1.304 (October 19) through 2.1.310 (October 23). I opened the live CHANGELOG.md. The first heading is 2.1.310. npm latest is `2.1.310`.

**2.1.310 (October 23).** The Monitor tool and plugin monitors now run on Bedrock, Vertex, and Foundry, and when telemetry or nonessential traffic is disabled. Long foreground sleeps are blocked in those sessions too. Requests that fail behind a gateway or compatible server that rejects the `effort-2025-11-24` beta value, or mid-conversation system messages, retry without them. Background cleanup no longer deletes recent files such as pasted images when `cleanupPeriodDays` is set very large. Compaction after a “Prompt is too long” error keeps less of a dense conversation (base64, SVG), so the next request is less likely to fail again. File-read permission prompts now show where a symlink leads. Windows: PreToolUse and PermissionRequest hooks no longer let a tool call through when Git Bash is missing and the hook command is only quoted strings. Auto mode’s first outside-working-directory read prompt now says that blocking those reads makes shell commands ask for permission. Non-interactive sessions (`-p`, Agent SDK) use less memory during compaction when the transcript file is very large. Linux: `/sandbox` and the sandbox startup warning name Ubuntu’s AppArmor restriction on user namespaces, and how to lift it, when it keeps bubblewrap from starting. An MCP tool call that times out in a non-interactive session while the server’s elicitation is unanswered now tells Claude that no answer came and not to repeat the call. Enterprise custom roles can grant Cloud sessions and Remote Control separately. Owners get a confirmation that lists the custom roles with that permission when they turn either on.

**2.1.309 (October 22).** Sandboxed Bash on macOS and Linux no longer runs the shell’s startup files outside the sandbox before each command. macOS `denyWrite` globs now apply at a path’s real location when the pattern’s folder is reached through a symlink. Sandboxed commands and auto-accepted edits can no longer modify the MCP config file passed with `--mcp-config`. A disabled MCP server is no longer reconnected by a later permission prompt in print and SDK sessions. Bash permission checks no longer trust a variable set by a command whose redirect can fail to open, and they now prompt for some compound commands that change directory before running git when the `cd` is wrapped or prefixed. An `Edit(path)` deny rule now blocks a Bash redirect to that path in some commands that also held a subshell. Worktree-isolated sessions refuse more locale-prefixed and zsh arithmetic commands. Nested `CLAUDE.md` files over the size limit load only their start, which stops repeated compaction after reading files under a huge nested file. PowerShell hooks now block on exit code 2, and Windows `PreToolUse` and `PermissionRequest` hooks with `"shell": "powershell"` no longer fail open when Git for Windows is missing. `sed -i` gets the standard Bash prompt and runs as written. It is no longer restored by `/rewind`. `claude ultrareview` requires workspace trust: it asks on a terminal, else refuses. Scripts pass `--trust-workspace` (no value; put a PR number or base branch before it) or set `CLAUDE_CODE_DISABLE_ULTRAREVIEW_TRUST_CHECK=1`. In bypass permissions mode in the Desktop app and VS Code, a dangerous `rm` or `rmdir` is denied at once with a rewrite hint so unattended sessions keep going. Commands the settings exclude from the sandbox now ask before writing to write-list paths while the sandbox is enabled but cannot run.

**2.1.308 (October 22).** The changelog line is “Bug fixes and reliability improvements”. I am not filling that in from memory.

**2.1.307 (October 21).** `/claude-api api-version` moves Claude API code to API version `2026-10-21`. `claude_code.safety_event` is a new OpenTelemetry event for each tool call a permission rule, auto mode, a safety check, a hook, or the sandbox stopped, prompted, or sent back. Requests no longer fail with a model-not-found error when `CLAUDE_CODE_DISABLE_1M_CONTEXT` is set and the model alias has a `[1m]` suffix. The `prompt-caching-scope` beta header is no longer sent behind custom `ANTHROPIC_BASE_URL` gateways or on Foundry. MCP servers in `claude -p` on macOS now say the login keychain is locked instead of “needs authentication”, and the next run after the keychain is open connects without a new sign-in. HTTP MCP tool calls that close mid-call are retried only when the tool is read-only or idempotent. Self-hosted runner checkout hooks stop after 30 minutes by default (`--checkout-hook-timeout-min`, or `0` for no limit). `@claude review stop` (also `off` and `pause`) ends an `@claude review always` subscription on a pull request. `axScreenReader` and `CLAUDE_AX_SCREEN_READER` in a settings `env` block apply only from user, managed, or `--settings` settings.

**2.1.306 (October 21).** `claude -p` inside cloud sessions no longer reports “Not logged in” when started without the session’s environment variables. Sessions with many MCP tools no longer replace an API error with “prompt is too long” and an unneeded compaction.

**2.1.305 (October 20).** `sandbox.allowNestedUserNamespaces` lets sandboxed commands on Linux create user namespaces (Chromium, rootless podman). The changelog says this lets them get around the sandbox’s file protections. `CLAUDE_CODE_RETRY_WATCHDOG_USAGE_LIMIT_RECHECK_MS` makes unattended retry mode notice a usage limit that was lifted before its reset. A 2.1.304 regression that failed every request on Vertex, Bedrock, and other providers when the endpoint rejected tool search is fixed: Claude Code retries without it. `allowedMcpServers` no longer filters `managed-mcp.json` stdio servers that use `${VAR}` only in `env`. A server your allowlist left out now loads on upgrade. Use `deniedMcpServers` to keep it off. Plugins that Claude Desktop mounts by path no longer keep their `allowed-tools` when managed settings set `allowManagedPermissionRulesOnly`. `/usage` warnings are now a one-line fix that names its source, and `f` applies the fixes.

**2.1.304 (October 19).** `/auto-mode-setup` is a guided setup that reads your project and recent sessions to teach auto mode about your environment. It is available on Bedrock, Vertex, Foundry, Claude apps gateway, and telemetry-disabled sessions. A mod’s `tool.check` can return `decider: "person"` (only the permission dialog or a host callback can approve; refused where nobody can be asked) and sees `e.isReadOnly`. `/claude-api managed-agents-secure` compares a Managed Agents agent’s settings with Anthropic’s published security guide without changing anything. A plugin can no longer keep a headless run going past `--max-turns`, Stop, or ended input by spawning subagents. `claude --worktree` no longer removes the worktree and its branch without asking at exit when HEAD is detached or on another branch. `CLAUDE_CODE_AUTO_COMPACT_WINDOW=200k` no longer silently sets a 100K window: the variable now accepts `k` and `M` suffixes like `--autocompact`. Tool search is on by default behind a custom `ANTHROPIC_BASE_URL`; a 400 or 422 from the gateway is retried without it. Code Review gains Quick, Standard (default), and Deep levels, plus an option to review every repository in a GitHub organization.

I also opened the memory docs and the permissions page. Memory docs still say Claude Code reads `AGENTS.md` on its own when there is no `CLAUDE.md` or `CLAUDE.local.md` in the working directory or above it (v2.1.277+; default `claude-md-or-agents-md`). `CLAUDE.local.md` counts as a CLAUDE.md and blocks direct `AGENTS.md` load unless you set Project instructions to `claude-md-and-agents-md`. Permissions still label `default` as Manual.

## Codex CLI

0.164.0 (October 23). I opened the `rust-v0.164.0` release page. The releases list shows `0.165.0-alpha.1` as a prerelease above the current stable tag `rust-v0.164.0`. `src/data/tool-versions.json` records latest as `0.164.0`.

**0.164.0 (October 23).** `/presets` stores reusable prompts and slash commands in ten numbered slots, with a keyboard menu. `/model` and `/permissions` take inline arguments for model, reasoning effort, speed, and permission profiles. The fullscreen transcript previews viewed and generated images on terminals that support Kitty or Sixel graphics. Enterprise MCP connections can authenticate with eligible Codex account grants or a dedicated sign-in, including OAuth clients that require secrets. Credentials are masked during supported remote Bash commands and Zsh snapshot sessions, including credentials from custom providers. The Amazon Bedrock setup wizard is on by default, and API key entry sits with the other AWS sign-in methods when credentials are detected.

Failed compaction no longer leaves partial summaries in conversation history. Resuming a session finds compressed histories and restores saved thread settings instead of replacing them with global defaults. Windows HTTPS and WebSocket connections handle proxy authentication challenges with the signed-in user’s Negotiate or NTLM credentials. CLI configuration overrides stay in place when `-c` or `--config` appears at more than one subcommand level. Queued prompts wait for pending permission selections before continuing.

## GitHub Copilot

No version number. I opened each changelog URL below. The Copilot label index lists the same eight posts for this week.

**Usage metrics API adds usage from tools like OpenCode (October 23).** Enterprise, organization, enterprise-user, and organization-user 1-day and 28-day reports gain a `totals_by_client` array. Each entry names a client (`opencode`, `claude_code`, `codex`, `zed`, `copilot_cli`, `github_app`, `cloud_agent`, `code_review`, `vscode`, `jetbrains`, or `other`), plus request count, session count, token usage, and `totals_by_model` with `model_selection_mode` (`user_selected`, `auto`, `product_selected`, `other`, `unknown`). This is model traffic, not agent-app job counts. `request_count` counts model calls, not prompts. Clients are identified by the integration ID they send. The Copilot usage metrics policy must be on.

**Turn notifications into code with GitHub Copilot in Teams (October 23).** The GitHub app in Teams (previously GitHub notifications) puts notifications and Copilot in one app. `@mention` GitHub on an issue notification to investigate, fix, and open a PR. If you already have GitHub notifications installed, update it. The legacy GitHub app will be renamed “GitHub (deprecated)” and retired on November 16. Install the consolidated app and mention `@GitHub`.

**GitHub Copilot in Visual Studio, September update (October 22).** Visual Studio 2026: the Git agent can explore a pull request from chat when you attach it from the Git Repository window (GitHub and Azure DevOps MCP). Bring Your Own Model is in preview and on by default across Community, Professional, and Enterprise. BYOM now requires the new Agent (Preview). The previous Ask and Agent modes no longer support BYOM, and you must re-add previously configured Ollama models. Providers listed: Microsoft Foundry, OpenAI, Anthropic, Ollama. NuGet Audit warnings in the Error List get a fixer that opens Copilot Chat in agent mode against the NuGet MCP server.

**Custom skills, agents, and MCP servers in usage metrics API (October 22).** `totals_by_skill`, `totals_by_custom_agent`, `totals_by_mcp`, `totals_by_slash_cmd`, and `totals_by_plugin` now show the configured name instead of `other` or `custom`. Each list holds up to 25 entries. Names come from Copilot CLI (interactive and prompt mode), the Copilot app, ACP integrations such as JetBrains, and the Copilot SDK. Prompt mode, the app, and ACP need Copilot CLI 1.0.97 or later. The Copilot SDK needs v1.0.21 or later. IDEs report names when their agent experiences run on that SDK. Names that look like file paths stay grouped under `other` or `custom`.

**Global default for Copilot features generally available (October 22).** Enforcement of the September global default policy is rolling through October 26 on Business and Enterprise. Eligible generally available features left Unconfigured are set to the selected default (Enabled, Disabled, or Let organizations decide). That assignment is one-time. Explicit enable/disable choices stay. Preview features stay opt-in. Review the setting under AI Controls → Copilot → Default policy for new features.

**Industry comparisons in the Copilot impact dashboard (October 22).** Opt-in, off by default. Enterprises, and organizations not owned by an enterprise, can compare agentic adoption, and how agentic users’ merge rate compares with non-agentic users, against aggregated peer benchmarks by company size and industry. The same policy controls contributing data and seeing comparisons. Rankings appear only after enough peers opt in. Data residency compares only within the same region. Comparisons are dashboard-only, not in the usage metrics API.

**Copilot model policy API (October 19).** GET and PATCH for model policies at `/enterprises/{enterprise}/copilot/model-policies`, `/enterprises/{enterprise}/teams/{team_slug}/copilot/model-policies`, `/enterprises/{enterprise}/copilot/apps/model-policies`, and `/orgs/{org}/copilot/model-policies`.

**Models deprecated on October 19 (October 19).** These left most Copilot surfaces (chat, inline edits, ask and agent modes, completions), as announced September 18: Gemini 3.7 Flash (use Gemini 3.8 Flash), GPT-5.5 and GPT-5.4 (use GPT-6.1 Sol), GPT-5.4 mini and GPT-5 mini (use GPT-6 Luna), Grok 4.5 (use Grok 4.7). Enterprise admins may need to enable the replacements in model policy.

## Gemini CLI

0.65.0 (October 20). I opened the v0.65.0 release. The releases list shows that tag as the current stable release. Nightly `v0.67.0-*` tags and `v0.66.0-preview.0` sit above it.

PR #29583 marks workspace settings read-only when the folder is untrusted. `gemini mcp add` and `gemini mcp remove` targeting workspace scope exit with status 1 and leave `.gemini/settings.json` on disk. User-scope still works. PR #29617 stops `@<directory>` from eagerly reading every nested file into the prompt; the model gets the path and can list or search. PR #29641 adds `otlpHeaders` for OTLP HTTP and gRPC, merged from settings, `OTEL_EXPORTER_OTLP_HEADERS`, `GEMINI_TELEMETRY_OTLP_HEADERS`, and CLI args. PR #29672 cuts false-positive untrusted-flag prompts on commands such as `ls -ld`, `grep -rn`, and `git -C`, and allows a compound loop when every sub-command matches an ALLOW rule.

## What this means for your setup

- Install Claude Code 2.1.310. If you pin `@anthropic-ai/claude-code` , bump the pin.
- Run `/auto-mode-setup` once if you use auto mode and want it taught from this repo’s recent sessions. It now works on Bedrock, Vertex, Foundry, a Claude apps gateway, and telemetry-off sessions.
- Leave `sandbox.allowNestedUserNamespaces` off unless you need Chromium or rootless podman inside the sandbox and accept that those processes can bypass the sandbox file protections.
- After 2.1.305, a `managed-mcp.json` stdio server that only interpolates`${VAR}` in`env` loads even when`allowedMcpServers` omitted it. Add it to`deniedMcpServers` if that was intentional.
- `claude ultrareview` in CI needs`--trust-workspace` (or`CLAUDE_CODE_DISABLE_ULTRAREVIEW_TRUST_CHECK=1` ). Put the PR number or base branch before that flag.
- `sed -i` is a real Bash edit as of 2.1.309.`/rewind` will not put the file back.
- A plugin can no longer keep `claude -p` alive past`--max-turns` by spawning subagents (2.1.304). Permissions and unattended runs already starts`-p` in Manual on 2.1.302+ and already passes`dontAsk` . Unattended agents in CI already passes`dontAsk` .
- Claude Code already reads `AGENTS.md` when no`CLAUDE.md` or`CLAUDE.local.md` sits in the working directory or above it. New repo agent setup still says Claude Code reads`CLAUDE.md` , not`AGENTS.md` . Context is a budget still describes only`CLAUDE.md` load at session start.
- Codex 0.164.0: use `/presets` for prompts you reuse.`/model <name>` and`/permissions <profile>` skip the menus. If you use Bedrock, the setup wizard now appears without an extra flag.
- Copilot Business and Enterprise: check AI Controls → Copilot → Default policy for new features before October 26 if you left features Unconfigured. Move workflows off Gemini 3.7 Flash, GPT-5.5, GPT-5.4, GPT-5.4 mini, GPT-5 mini, and Grok 4.5. The November 16 drop of Opus 4.8, Opus 5, and GPT-5.6 Terra from last week is still ahead. Teams users on the legacy GitHub app need the consolidated app before November 16. Usage metrics now name third-party clients and your own skills; treat `client` as a best-effort grouping.
- Multi-model routing still prices Haiku 4.5, Sonnet 5, Opus 5, and GPT-5.6 Terra, and still names GPT-5 Mini as the successor to o4-mini.
- Gemini CLI 0.65.0: do not run `gemini mcp add` against workspace scope in an untrusted folder.`@some/dir` no longer dumps the tree into the prompt. Set`telemetry.otlpHeaders` or`GEMINI_TELEMETRY_OTLP_HEADERS` if your collector needs a bearer token.

## Pages we updated this week

None this week.
