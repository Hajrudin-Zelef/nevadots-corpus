---
id: collect-261121-veille-ia/gpt-openai/codex-cli-guide-2026-setup-sandbox-agents-md-mcp-2
title: "Codex CLI Guide 2026: Setup, Sandbox, AGENTS.md & MCP"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["approval", "sandbox", "permissions", "flag", "codex", "default", "profiles", "project-safe"]
source: https://blakecrosley.com/guides/codex
source_anchor: ""
source_lines: [1501, 3000]
section: "GPT_OpenAI"
---
The **`--approve-for-me`** CLI flag turns approval prompts into **automatically reviewed approvals**: instead of interrupting you, each request is adjudicated by a review pass and approved or refused on your behalf.<sup>123</sup> The flag sets three things at once: the `auto_review` approvals reviewer, the `on-request` approval policy, and the `workspace-write` sandbox, so it cannot be combined with `--sandbox` or `--dangerously-bypass-approvals-and-sandbox`. In `codex exec`, which otherwise never asks, this flag (or `approvals_reviewer = "auto_review"` in config) is how approval requests get answered.<sup>239</sup> It sits between “approve everything” (the posture `--full-auto` used to encourage, now removed) and “interrupt for everything”: the prompts still exist as decision points, but a reviewer answers them. Pair it with the v0.146.1 safer auto-review defaults for cyber-capable models, which this flag inherits. Treat it like any delegation of authority: right for long unattended runs where interruption means a stall, wrong for sessions touching credentials or production state where you want to be the reviewer.

### Approval for Escalated Terminal Input (v0.151.0; on by default since v0.158.0)

The `write_stdin_approval` feature flag – off by default when it arrived in v0.151.0, on by default since v0.158.0 – requires a fresh approval before non-empty input is sent to an escalated unified-exec terminal. These reviews route through hooks, Guardian, the app server, and the TUI as `writeStdin` approvals; the terminal’s environment, launch directory, and escalation state persist across turns, and the process identity is revalidated after approval before any bytes are written. The fully formatted action and approval reason must fit within 8,000 bytes – oversized or truncated actions are rejected before approval is requested or anything reaches the terminal.<sup>145</sup> v0.158.0 turns the flag on (#47799) and stops runtime-only grants, such as those plugins use for metrics, from triggering a review, while extra agent permissions, sandbox bypasses, and policy changes since launch still do (#48073). **To turn it off:** set `write_stdin_approval = false` under `[features]` in `config.toml`.<sup>177</sup>

### Permission Profiles (v0.113.0+, expanded in v0.128.0 and v0.133.0)

Permission profiles split filesystem and network sandbox policies into named, reusable sections. Set `default_permissions` to a built-in profile such as `:read-only`, `:workspace`, or point it at a custom `[permissions.<name>]` table.<sup>87</sup> v0.133.0 promotes profiles as a managed surface: list APIs expose available profile metadata, profiles can inherit from one another, managed `requirements.toml` can declare permission requirements, active profiles refresh at runtime, and Windows sandbox setup now consumes the resolved profile rather than a separate ad hoc policy.<sup>98</sup>

```
default_permissions = "project-safe"
[permissions.project-safe.filesystem]
":minimal" = "read"          # Platform and runtime paths common tools need
"/usr/local" = "read"
glob_scan_max_depth = 3
[permissions.project-safe.filesystem.":workspace_roots"]
"." = "write"
"**/*.env" = "deny"
[permissions.project-safe.network]
enabled = true
mode = "limited"
[permissions.project-safe.network.domains]
"api.github.com" = "allow"
"registry.npmjs.org" = "allow"
```
Use `deny` for sensitive files and globs that should be unreadable even when the workspace root is writable. Older configs spell these `:project_roots` and `none`; both still load as legacy aliases at v0.167.1, but the docs and the source use `:workspace_roots` and `deny`. Off macOS the sandbox cannot match an unbounded `**` in a `deny` glob natively: Codex warns at startup unless `glob_scan_max_depth` is set, which caps how deep the glob is expanded, so set it as the example does or list explicit depths such as `*.env` and `*/*.env`.<sup>213</sup> Domain rules under `[permissions.<name>.network.domains]` are enforced only while the network proxy is on (`network_proxy = true` under `[features]`). For one-off command exceptions, prefer rules over broadly expanding a profile.<sup>87</sup>

**v0.151.0:** restored app-server threads keep their permission profiles instead of being flattened into legacy per-turn sandbox overrides – profiles survive turn routing and compatible `/cd` directory changes, and a `/cd` that cannot represent the restored profile safely as a legacy sandbox mode is rejected rather than silently weakening restrictions.<sup>144</sup>

### Legacy `--full-auto` Guidance

Older guides described `--full-auto` as a convenience alias for:

```
codex --sandbox workspace-write --ask-for-approval on-request
```
In v0.128.0, release notes mark `--full-auto` deprecated, and current CLI help no longer lists it for interactive runs. Use the explicit flags above or a named permissions profile instead.<sup>86</sup>

**v0.147.0 completes the removal: `codex exec --full-auto` is gone.** A script still passing the flag now errors rather than running. The migration is mechanical: replace it with `--sandbox workspace-write`, or with `--approve-for-me` on its own if you want requests to leave the sandbox reviewed automatically (the two flags cannot be combined, and `codex exec` never prompts a person).<sup>239</sup> Audit any automation that shells out to `codex exec` before upgrading: the failure shows up as a broken pipeline, not a deprecation warning.<sup>123</sup>

**Sandbox reliability (v0.129.0):** Linux sandbox startup hardening reduces races on slow filesystems or symlinked checkouts; Windows sandbox reliability improvements address several edge-case crashes during long-running runs; and the vendored Bubblewrap is bumped to **0.11.2** with upstream security patches. No config changes required. Run `codex update` to pick these up.<sup>89</sup>


### Recommended Configurations

**Daily development** (safe default):

```
sandbox_mode = "workspace-write"
approval_policy = "on-request"
```
**Power user** (full access, human-in-the-loop):

```
sandbox_mode = "danger-full-access"
approval_policy = "on-request"   # `untrusted` retired in v0.149.0
```
Community write-ups recommend the pairing as a “sweet spot”<sup>8</sup>; understand what it actually does before repeating the advice: approval prompts fire when a command needs to leave the sandbox or reach the network, and `danger-full-access` removes both boundaries, so `on-request` has almost nothing left to escalate. It is maximum capability with minimal asking, not maximum capability with a human check on every command; if you want the latter, keep `workspace-write` and let the sandbox boundary do the prompting.<sup>132</sup>

**CI/CD automation**:

```
sandbox_mode = "workspace-write"
approval_policy = "never"
```
### Smart Approvals with Auto-Review (v0.115.0+; formerly Guardian subagent)

Smart Approvals can route review requests through a **reviewer subagent** (auto-review, launched under the name Guardian) instead of requiring human approval for every action. The reviewer session persists across approvals to reuse prompt cache and avoid startup overhead. Each review gets a clean history (prior decisions don’t leak into later reviews).<sup>73</sup>

Configure the reviewer in `config.toml`:

```
approvals_reviewer = "auto_review"   # "user" (default) or "auto_review"
```
This is particularly useful for CI/CD workflows where you want automated review with reasoning rather than blanket `approval_policy = "never"`. `auto_review` is the documented value (“`auto_review` uses the reviewer subagent”); `guardian_subagent`, the name the setting launched with, is still accepted as a legacy alias at v0.167.1. The setting does not change sandboxing.<sup>206</sup>

**Guardian v2 defaults flipped (v0.151.0):** Guardian v2 now defaults to computer-use reviews with images. `features.guardianv2.review_scope.computer_use_only` defaults to `true` – set it to `false` to retain the broader tool review scope – and `features.guardianv2.transcript.include_images` defaults to `true` – set it to `false` to keep images out of Guardian transcripts. Both shipped as configurable, default-off surfaces in v0.150.0; v0.151.0 is where the defaults turned on.<sup>145</sup>

**Guardian scope follows the approval mode (v0.153.0):** two modes now skip Guardian work that cannot change the outcome. In **Full Access** (which already combines `approvalPolicy: "never"` with unrestricted permissions), confirmation-only Guardian and MCP requests are approved without synchronous review, sampler prewarming, or background scoring – pending, failed, or restricted environments are not treated as Full Access, and the active permission state is re-evaluated each turn so an existing thread can enter or leave Full Access safely. In **User approval mode** (`approvals_reviewer = "user"`), Guardian prewarming and asynchronous scoring are skipped and ordinary `node_repl.js` execution confirmations are accepted automatically, while sensitive-action checks and requests for user input keep their existing behavior.<sup>149</sup> This narrows *when* Guardian runs, not the human approval layer: the prompting behavior this guide describes for `danger-full-access` under `on-request` is approval policy, a separate mechanism. **v0.152.1** additionally lets Guardian approval review source its Node REPL policy from model metadata (`auto_review.node_repl_policy`), falling back to the bundled policy when the field is absent.<sup>148</sup>

**Guardian computer-use scoring honors model requirements (v0.153.1):** computer-use-only Guardian scoring and fast approval decisions now run only when the active model sets `node_repl_auto_review_required` in its metadata, and prior or in-flight scores are invalidated when a model switch skips scoring – so switching back to a reviewed model cannot revive a stale approval decision. This is the main change in the v0.153.1 patch, and the release notes do not list it: #42422 appears only in the v0.153.0 to v0.153.1 commit comparison.<sup>151</sup>

**Guardian parent-compaction reuse is on by default (v0.156.0):** #46522 promotes `guardian_reuse_parent_compaction` to a stable feature and enables it by default, which lets Guardian reuse the parent thread’s encrypted compaction when it restarts a review session. Per the PR’s tests, cached review sessions are still invalidated when the parent history changes. **To restore the previous behavior:** set `guardian_reuse_parent_compaction = false` under `[features]` in `config.toml`, or run `codex features disable guardian_reuse_parent_compaction`.<sup>170</sup><sup>172</sup>

**Guardian thread context is on by default (v0.157.0):** #47275 promotes `guardianv2.thread_context` to stable and enables it for synchronous and asynchronous Guardian reviews, so reviews use thread-owned context. It is independent of the Guardian v2 `enabled` toggle. In v0.157.0 you could turn it off with `thread_context = false` under `[features.guardianv2]`; v0.158.0 removed that option.<sup>175</sup>

**Guardian thread context is always on (v0.158.0):** #47686 and #47689 make thread-owned context unconditional for Guardian session capture and subagent history forks. Codex ignores `thread_context` under `[features.guardianv2]`, including profile overrides and managed requirements, and shows a deprecation notice asking you to remove it from `config.toml`. There is no replacement off-switch.<sup>177</sup>

**Guardian can read earlier user messages (v0.160.0, opt-in):** the transcript Guardian reviews can omit an earlier instruction, restriction, or revoked permission. With `guardian_conversation_history_tools = true` under `[features]` and Apps enabled, the reviewer gets two tools, `user_message.search_messages` and `user_message.read_messages`, served through the parent session’s own Apps connection and identity. Each call rechecks the parent’s current app and tool policy and fails closed on a disabled tool or one that needs approval. Two `[auto_review]` keys tune it: `conversation_history_max_output_tokens` caps each response (an estimated 4,000 tokens by default; stricter parent limits still apply), and `experimental_conversation_history_prompt` replaces the built-in retrieval instructions (blank uses the default). A second opt-in flag, `guardian_root_handoff_context`, narrows the root-conversation evidence for a worker agent’s review to the messages around each handoff to it, plus the latest three. Both flags are under development and off by default; the config reference does not list them yet.<sup>183</sup>

```
[features]
guardian_conversation_history_tools = true
[auto_review]
conversation_history_max_output_tokens = 4000
```
**PowerShell classifier tightening (v0.142.2):** PowerShell commands containing executable AST regions the safety classifier cannot inspect now **require approval** instead of slipping through auto-approval — closing a gap where obfuscated or dynamically built PowerShell could evade classification on Windows.<sup>110</sup>

**Dangerous-command detection expanded (v0.144.5):** Detection now recognizes more forced `rm` forms, and rejected commands come back with **clearer rejection reasons** instead of an opaque denial — useful when you are tuning approval policies and need to know why a command was blocked.<sup>115</sup>

### Enabling Network Access

Codex blocks network access by default in `workspace-write` mode. Enable it when needed:

```
# Per-run
codex -c 'sandbox_workspace_write.network_access=true' "install the packages"
```
Or persist it in `config.toml`:

```
[sandbox_workspace_write]
network_access = true
writable_roots = ["/path/to/extra/dir"]   # Additional writable directories
exclude_slash_tmp = false                  # true removes /tmp from the writable roots
exclude_tmpdir_env_var = false             # true removes $TMPDIR from the writable roots
```
### WebSocket Proxy Support (v0.104.0+)

When the network proxy is active, Codex sets `WS_PROXY` and `WSS_PROXY` (and their lowercase forms) for sandboxed commands, pointing them at the same HTTP proxy listener as `HTTP_PROXY` and `HTTPS_PROXY`, so WebSocket clients that read only the dedicated variables go through the proxy too:<sup>57</sup>

```
# What a sandboxed command sees with the default proxy_url
WS_PROXY=http://127.0.0.1:3128
WSS_PROXY=http://127.0.0.1:3128
```
Codex sets these itself, so you do not export them. The SOCKS5 listener (v0.93.0+) reaches commands the same way, through `ALL_PROXY`.

**v0.146.0 is the release that made “all transport layers” actually true.** Configured proxies are now honored across authentication, plugin downloads, MCP authorization, remote execution, WebSocket connections, HTTP redirects, and LM Studio connections — seven paths that the changelog lists as previously ignoring the proxy.<sup>121</sup> If you ran Codex behind a corporate proxy before v0.146.0 and saw logins or plugin installs fail while ordinary model calls worked, that gap is the likely explanation.

**System proxy on macOS (v0.142.2+):** with `respect_system_proxy` enabled, macOS authentication clients honor the system proxy, PAC, and WPAD settings, so corporate Macs that push proxy config via profiles no longer need per-shell proxy env vars for auth traffic.<sup>110</sup> **As of v0.143.0 this extends to Windows and to Responses API traffic:** Codex routes both authentication and Responses API calls through the macOS or Windows system proxy, including PAC and WPAD autoconfiguration, closing the last gap for locked-down corporate networks on either OS.<sup>111</sup> **v0.167.0** lists the setting as “System proxy” in `/experimental`, so you can turn it on without editing `config.toml`; `respect_system_proxy` is experimental and off by default.<sup>200</sup><sup>201</sup>

### Testing the Sandbox

Verify sandbox behavior before trusting it. `codex sandbox` runs one command under the sandbox Codex would use, without starting a model session. At v0.167.1 the form is `codex sandbox [OPTIONS] -- <command>`: there is no platform word, and the profile flag is `--permission-profile` (`--permissions-profile` and `-P` also parse).<sup>214</sup>

```
# Run from a project directory that is not your home directory and not under /tmp or $TMPDIR.
# 1. Control: a write inside the workspace succeeds.
codex sandbox --permission-profile :workspace -- touch ./codex-sandbox-ok
# 2. A write outside the workspace must be denied.
codex sandbox --permission-profile :workspace -- touch "$HOME/codex-sandbox-test"
ls "$HOME/codex-sandbox-test"     # must say the file does not exist
# 3. Under :read-only, a write inside the workspace must be denied too.
codex sandbox --permission-profile :read-only -- touch ./codex-sandbox-ro
```
What to expect follows from how v0.167.1 defines the two profiles: `:workspace` grants writes to the workspace roots and the system temp directories and nowhere else, and `:read-only` grants no writes at all. Step 1 should create its file; steps 2 and 3 should fail with a permission error and create nothing. A denied write prints a permission error; on macOS it reads `Operation not permitted`.<sup>214</sup> If the file from step 2 exists, the sandbox is not confining writes and your configuration needs investigation. The location matters because `:workspace` also allows writes to the system temp directories: a project under `/tmp` proves nothing here. Delete `codex-sandbox-ok` afterwards.

Reads are a separate question. `:workspace` and `workspace-write` do not restrict reads, so `ls /etc/passwd` succeeds under a healthy sandbox, and a file such as `/etc/shadow` is unreadable to a normal user with no sandbox at all; neither tells you anything. To test a read restriction, run a read through a permission profile that marks the path `"deny"`. With the `project-safe` profile above, first make sure a `.env` file exists in the workspace and run a control read of a file the profile leaves readable: `codex sandbox --permission-profile project-safe -- cat README.md` must print the file. Then `codex sandbox --permission-profile project-safe -- cat .env` should fail with a permission error, because the profile marks that path `"deny"`. The second result is evidence only when the first succeeded. On macOS, add `--log-denials` to list what the sandbox blocked.<sup>213</sup><sup>214</sup>

One trap: the OpenAI approvals page still prints an older form with a platform word (`codex sandbox macos ...`). At v0.167.1 the command defines no such subcommand, so the word is taken as the program to run, and unless a program named `macos` is on your `PATH` the command fails for a reason that has nothing to do with the sandbox. A failure is only evidence when the control in step 1 succeeds.<sup>214</sup>

On Windows, the docs give a one-line check that MXC starts (CLI 0.162.0 or later); the expected output is `MXC_OK` with exit code `0`:<sup>2</sup>

```
codex -c windows.sandbox=mxc sandbox --include-managed-config --permission-profile :workspace -- cmd.exe /d /c echo MXC_OK
$LASTEXITCODE
```
## How Does AGENTS.md Work?

AGENTS.md is Codex’s project instruction system — an open standard<sup>9</sup> now governed by the Linux Foundation’s Agentic AI Foundation. Supported by Codex, Cursor, Copilot, Amp, Jules (Google), Gemini CLI, Windsurf, Cline, Aider, Zed, Factory, RooCode, and 60,000+ open source projects. It defines how Codex behaves within a specific repository or directory. See Skills for reusable expertise packages that complement AGENTS.md.

### Discovery Hierarchy

Codex builds an instruction chain at session start by walking the directory tree:

1. **Global** (`~/.codex/` ):`AGENTS.override.md` >`AGENTS.md`
2. **Project** (git root to current directory): Each level checked for`AGENTS.override.md` >`AGENTS.md` > fallback filenames
3. **Merging** : Files concatenate root-down; closer files appear later in the prompt and override earlier guidance

```
~/.codex/AGENTS.md                     ← Global defaults
  └─ /repo/AGENTS.md                   ← Project-wide rules
      └─ /repo/services/AGENTS.md      ← Service-specific rules
          └─ /repo/services/payments/
               AGENTS.override.md      ← Read instead of this directory's AGENTS.md
```
**Untrusted projects skip project AGENTS.md (v0.150.0):** the walk above assumes the project is trusted. Since v0.150.0, “Untrusted projects no longer supply project-level AGENTS.md instructions, and managed deny-read rules remain enforced after permission changes” – project-scoped discovery is skipped while user-level `~/.codex/AGENTS.md` instructions are preserved, so an untrusted checkout cannot inject an operating contract before you trust it.<sup>141</sup>

**Desktop multi-folder projects (26.715, July 23, 2026):** The walk above describes the CLI, which resolves everything relative to the current working directory. The ChatGPT desktop app now lets a local project span several related folders, and it resolves discovery against exactly one of them: “New chats, Git operations, and automatic discovery of AGENTS.md, skills, and config.toml use the primary folder. Secondary folders remain available for file search, reading, and editing.” You choose which folder is primary from the project’s menu via **Edit project**.<sup>118</sup>

That single setting governs three of Codex’s five core systems at once — instructions, skills, and configuration all follow the primary folder. Practically: an AGENTS.md in a secondary folder is a file Codex can open, not an operating contract it loads. If the repo carrying your rules is the one you attached second, either promote it to primary or restate those rules in the primary folder’s AGENTS.md; the same applies to repository skill folders you expected to be picked up. Nothing about the CLI’s directory walk changed — this is desktop-app project scoping.

### What Makes a Great AGENTS.md

Based on direct guidance from Codex itself and community patterns<sup>10</sup>:

**DO:**

- Be specific: `"Use rg --files for discovery"` beats`"search efficiently"`
- Define closure: What does “done” mean? (tests pass, lint clean, etc.)
- Include commands: Build, test, lint, format (exact invocations)
- Organize by task: Coding, review, release, incident/debug sections
- Define escalation: What to do when blocked or encountering unexpected state

**DON’T:**

- Dump entire style guides without execution rules
- Use ambiguous directives (“be careful,” “optimize”)
- Mix contradictory priorities (speed + exhaustive verification + no runtime budget)
- Write prose documentation (AGENTS.md is operational policy, not README)

### Example: Production AGENTS.md

```
# Repository Guidelines
## Build, Test, and Development Commands
- Run API (dev): `python3 -m uvicorn main:app --reload`
- Install deps: `pip install -r requirements.txt`
- Lint: `python3 -m ruff check .` (auto-fix: `--fix`)
- Format: `python3 -m ruff format .`
- Tests: `python3 -m pytest -v`
- Coverage: `python3 -m pytest --cov=app --cov-report=term-missing`
## Coding Style & Naming Conventions
- Python 3.11+. Type hints on all functions.
- Ruff enforced: 88-char lines, double quotes, spaces for indent.
- Naming: modules `snake_case.py`, classes `PascalCase`, functions `snake_case`.
## Commit & Pull Request Guidelines
- Conventional Commits: `feat:`, `fix:`, `docs:`, `refactor:`, `chore:`, `test:`
- Commits should be small and focused.
- PRs must include: description, test plan, and screenshots for UI changes.
## Security
- Never commit secrets. Use `.env` for local config.
- Validate all external API calls with proper error handling.
```
### Secret Handling in Agent Sessions

Treat Codex-visible history as a security surface, not just source code. Codex release notes document shell snapshotting and environment-variable redaction work, and the memory system scans memory writes for secrets, but those protections do not turn command output, session transcripts, shell snapshots, local logs, or helper scripts into safe places to print credentials.<sup>37</sup><sup>50</sup><sup>95</sup>

The operating rule is simple: do not print secrets for the model to inspect; keep helper credentials in environment-required config; separate executable source, docs, generated caches, session transcripts, shell snapshots, logs, and intentional secret stores during audits; redact local history when a high-confidence secret shape appears; and promote prevention hooks only after the manual hygiene loop is proven.<sup>95</sup>

### The Override Mechanism

`AGENTS.override.md` at any directory level replaces the normal `AGENTS.md` for that scope. Use for:

- **Release freezes** : “No new features, fixes only”
- **Incident mode** : “All changes must be reviewed by on-call”
- **Temporary hardening** : “No dependency updates this sprint”

### Configuration

```
# Custom fallback filenames (in addition to AGENTS.md)
project_doc_fallback_filenames = ["TEAM_GUIDE.md", ".agents.md"]
# Increase max size for large instruction files
project_doc_max_bytes = 65536    # 64 KiB (default: 32 KiB)
```
### Scaffold Generation

```
codex                           # Launch TUI
/init                           # Generate AGENTS.md scaffold
```
Or verify your instruction chain:

```
codex --ask-for-approval never "Summarize your current instructions"
```
## Hooks

Codex introduced hooks in v0.99.0 and v0.100.0 under the legacy `AfterAgent` and `AfterToolUse` names, added an experimental hooks engine in v0.114.0 with `SessionStart` and `Stop`,<sup>70</sup> and made hooks stable in v0.124.0 (April 23, 2026).<sup>85</sup> The engine registers **fourteen events** since v0.164.0 (`HOOK_EVENT_NAMES` in `codex-rs/hooks/src/lib.rs`): eleven at v0.149.1, twelve from v0.150.0 with `Interrupt`,<sup>141</sup> and fourteen once the observation-only `PreModelRequest` and `PostModelRequest` arrived (#55144).<sup>208</sup> The legacy names are gone from the configurable surface: `AfterToolUse` no longer appears anywhere in the source tree, and `AfterAgent` survives only as the internal payload name of the legacy `notify` bridge.<sup>133</sup> Hooks run scripts or MCP tools during the agentic loop. Matching hooks from multiple files all run, multiple matching command hooks for one event launch concurrently, and every non-managed hook must be reviewed and trusted before it runs.<sup>133</sup> **v0.148.0 extends the engine: hooks can run commands asynchronously and can invoke MCP tools** – set `async = true` on a command handler to let it run in the background while Codex continues, or use an `mcp_tool` handler to reach the same MCP servers the agent uses.<sup>127</sup><sup>133</sup>

### Available Hook Events

| Event | When It Fires | Matcher Applies To | 
|---|---|---|
| `SessionStart` | When a session starts ( `startup` ,`resume` ,`clear` , or`compact` ); plain-text stdout is added as developer context before the first turn | `source` | 
| `SessionEnd` | When the main thread ends: archiving or deleting an open conversation, a normal close, or 30 minutes idle with no connected client; never for subagents; 1-second default timeout, 3-second maximum, always synchronous | end reason (currently only `other` ) | 
| `UserPromptSubmit` | Before a user prompt is sent; receives `prompt` and can block or augment it | not supported | 
| `PreModelRequest` | Before each model request attempt, retries within a turn included (v0.164.0+); receives the model and `turn_id` , plus`agent_id` and`agent_type` when available. Observation only: “Hook output is diagnostic only. It can’t change the model’s context or block a request.”<sup>208</sup> | not supported | 
| `PostModelRequest` | After each model request attempt (v0.164.0+); adds `status` (`completed` ,`failed` , or`cancelled` ),`error` , and`token_usage` . Observation only<sup>208</sup> | not supported | 
| `PreToolUse` | Before Bash, `apply_patch` file edits, MCP tool calls, and other local function tools run; can block or rewrite the call | `tool_name` (`Bash` ;`apply_patch` ,`Edit` , or`Write` ; MCP names such as`mcp__fs__read` ) | 
| `PermissionRequest` | When Codex is about to ask for approval (shell escalation, managed-network approval); can allow, deny, or defer to the normal prompt; skipped for commands that need no approval | tool name | 
| `PostToolUse` | After supported tools produce output, including Bash commands that exit non-zero; cannot undo side effects; supports `continue: false` | tool name | 
| `PreCompact` | Before Codex compacts the chat | `trigger` (`manual` or`auto` ) | 
| `PostCompact` | After Codex compacts the chat | `trigger` (`manual` or`auto` ) | 
| `SubagentStart` | When a subagent starts | `agent_type` | 
| `SubagentStop` | When a subagent stops | `agent_type` | 
| `Stop` | When a turn stops (the doc lists it under “During a turn”); `stop_hook_active` reports whether the turn was already continued by`Stop` ; expects JSON on stdout. Not the session end, which is`SessionEnd` | not supported | 
| `Interrupt` | When an active top-level turn is interrupted (v0.150.0+): “New Interrupt hooks can run commands or MCP handlers when an active top-level turn is interrupted.” Never for subagents; 1-second default timeout, 3-second maximum (the same clamp as `SessionEnd` ), and async handlers are supported<sup>141</sup> | not supported | 

Hosted tools such as `WebSearch` do not pass through the hook path, and some specialized tool paths can opt out; OpenAI’s own framing is “a useful guardrail, not a complete enforcement boundary.”<sup>133</sup>

### Hook Configuration

Hooks are organized in three levels – an event, a matcher group, and one or more handlers – and live either in `hooks.json` (`~/.codex/hooks.json`, `<repo>/.codex/hooks.json`, or a plugin’s `hooks/hooks.json`) or inline in `config.toml` and `requirements.toml`. Project-local hooks load only when the project `.codex/` layer is trusted.<sup>133</sup> The inline TOML form:

```
# ~/.codex/config.toml or <repo>/.codex/config.toml
[[hooks.SessionStart]]
matcher = "startup|resume"
[[hooks.SessionStart.hooks]]
type = "command"
command = "python3 ~/.codex/hooks/session_start.py"
additionalContextLimit = 5000
[[hooks.PreToolUse]]
matcher = "^Bash$"
[[hooks.PreToolUse.hooks]]
type = "command"
command = '/usr/bin/python3 "$(git rev-parse --show-toplevel)/.codex/hooks/pre_tool_use_policy.py"'
timeout = 30
statusMessage = "Checking Bash command"
[[hooks.PostToolUse]]
matcher = "^Bash$"
[[hooks.PostToolUse.hooks]]
type = "command"
command = "python3 ~/.codex/hooks/post_tool_use_review.py"
async = true
timeout = 120
```
Handler fields: `type` (`command` or `mcp_tool`; `prompt` and `agent` handlers are parsed but skipped), `command`, `timeout` in seconds (600 by default; `SessionEnd` and `Interrupt` 1 second, up to 3), `statusMessage`, `additionalContextLimit` (how much `additionalContext` reaches the model before Codex saves the full text to disk and sends a preview), `commandWindows` (Windows-only override), and `async`. Commands run with the session `cwd` as their working directory, so repo-local hooks should resolve paths from `git rev-parse --show-toplevel` rather than a relative `.codex/hooks/...` path.<sup>133</sup>

**Review and trust.** Codex records trust against each non-managed hook’s current hash; new or changed hooks are marked for review and skipped until trusted. Run `/hooks` to inspect hook sources, review, trust, or disable individual hooks; when hooks need review at startup, Codex prints a warning pointing at `/hooks`. Managed hooks from system, MDM, cloud, or `requirements.toml` sources are trusted by policy and cannot be disabled from the user hook browser. `--dangerously-bypass-hook-trust` runs enabled hooks without persisted trust for one invocation, for automation that vets hook sources elsewhere.<sup>133</sup>

The `SessionStart` hook’s stdout feeds into the model’s context, making it ideal for injecting dynamic information (dates, branch names, environment variables) at session start.

### Replicating Claude Code Hook Patterns

Codex uses the same event names as Claude Code for the events it implements, so most hook configs port by moving the JSON into `hooks.json` and trusting it through `/hooks`:

| Claude Code Pattern | Codex Alternative | 
|---|---|
| `PreToolUse` file blocking | `PreToolUse` hook matched on`apply_patch` (or`Edit` /`Write` ) or`Bash` ; return the deny shape to block, or rewrite the call | 
| `PostToolUse` linting | `PostToolUse` hook matched on`Bash` or`apply_patch` running your linter; add`async = true` to keep it off the critical path | 
| `SessionStart` context injection | `SessionStart` hook returning`additionalContext` (plain stdout also works) | 
| `Stop` notification | `Stop` hook with a notification command (JSON output required) | 
| `SubagentStop` notification | `SubagentStop` hook, matched on`agent_type` | 
| `PreCompact` | `PreCompact` and`PostCompact` , matched on`manual` or`auto` | 
| `SessionEnd` cleanup | `SessionEnd` hook; 1-second default timeout, 3 maximum, runs synchronously | 
| Async hooks | `async = true` on a command handler (v0.148.0);`SessionEnd` hooks always run synchronously | 

**Expert tip:** New hook events still ship in releases (v0.150.0 added `Interrupt`,<sup>141</sup> and v0.164.0 added `PreModelRequest` and `PostModelRequest`<sup>208</sup>), so check `HOOK_EVENT_NAMES` in `codex-rs/hooks/src/lib.rs` and the Codex changelog for the current list.

**In-TUI hook browser (v0.129.0):** Run `/hooks` in the TUI to discover available hooks, see which are currently active, and toggle individual hooks without editing `config.toml`. Useful when troubleshooting a misbehaving plugin-bundled hook or temporarily disabling a `PostToolUse` linter for a heads-down editing session.<sup>89</sup>


## What Is MCP (Model Context Protocol)?

MCP extends Codex’s capabilities by connecting to external tools and services. The `codex mcp` command group is listed as stable in the CLI command reference.<sup>218</sup> Codex supports two transport types: STDIO (local processes) and Streamable HTTP (remote servers).<sup>11</sup>

**v0.121.0 MCP changes:** Tools are now registered with a **namespace**, so tool names in listings appear as `<server>:<tool>` rather than bare names — update any scripts or prompts that grep against unqualified tool names. A new `supports_parallel_tool_calls` flag is plumbed through to included MCPs, enabling parallel execution for servers that declare support. Sandbox-state metadata now flows through MCP tool metadata so servers can adapt behavior (e.g., warn if running under a read-only sandbox). The custom `codex/sandbox-state` request has been **removed** — use the metadata path instead. Phase 3 of the MCP Apps rollout lands here with tool-call support; flattened deferred tool calls are now supported for servers using the deferred-call pattern.<sup>82</sup>

**v0.142.2 MCP changes (June 25, 2026):** MCP tools now use **tool search by default when supported** — instead of loading every tool definition upfront, Codex discovers tools on demand, improving discovery in tool-heavy setups while staying compatible with older models and providers. Remote stdio MCP servers also accept absolute working directories written in the remote platform’s path format (a Windows-style path from a macOS client, or vice versa).<sup>110</sup>

**v0.147.0 MCP changes (August 7, 2026):** Codex adds **opt-in support for the MCP 2026-07-28 protocol revision**: paginated discovery (large tool catalogs stream in pages instead of one oversized listing), **multi-round requests** (a server can hold a multi-step exchange within one logical request), and **non-blocking server startup**, with cached MCP tools exposed before the server finishes starting so a slow server no longer stalls session launch. It is opt-in, so servers on the older revision keep working unchanged.<sup>123</sup> At v0.167.1 the switch is the `mcp_2026_07_28` feature flag, which is under development and off by default; set `mcp_2026_07_28 = true` under `[features]` to turn it on.<sup>247</sup>

**v0.151.0 MCP changes (August 29, 2026):** Optional MCP servers get a **configurable startup grace period** – `mcp_optional_startup_grace_ms` (default 1,000 ms) controls how long tool catalog capture waits for optional servers, `0` disables the shared grace so optional servers fall back to their configured `startup_timeout_sec`, and updated values apply during runtime and MCP configuration refreshes. Extensions can also **inspect or replace MCP tool results** before they reach the model: `ToolLifecycleContributor::on_mcp_tool_result` runs before MCP completion is published and before the result is prepared for the model, for successful and error results alike, including through Code Mode.<sup>144</sup>

**v0.152.0 MCP changes (September 1, 2026):** Three additive changes. **`openai/elicitation` form requests** land: Codex advertises form support when the client declares an object-valued `form` capability, handles `openai/elicitation/create` requests, and forwards their metadata and opaque schema through the app server as `openaiForm` (preserving `x-openai-*` annotations); legacy `openai/form` handling stays independent, and the TUI automatically declines form requests it cannot render. **Per-tool output limits**: each entry under an MCP server’s `tools` configuration accepts a positive `output_token_limit` (for example `[mcp_servers.context7.tools.search]` with `output_token_limit = 2000`); the most restrictive limit wins when plugin and user policies overlap, and the effective budget is carried in conversation history so tool output, post-tool hook responses, and resumed sessions truncate at the same limit – additive alongside the global `tool_output_token_limit`. **Package-style server names**: MCP server names may now contain `:`, `@`, `/`, and `.` (so `npm:@modelcontextprotocol/server-sequential.thinking` is a legal name), preserved across `mcp add`/`get`/`list`/`remove`, runtime tool namespaces, and OAuth credential lookup, with non-bare names quoted in generated `config.toml` recovery hints.<sup>147</sup>


### Configuring MCP Servers

**STDIO servers** (local processes):

```
# In ~/.codex/config.toml or .codex/config.toml
[mcp_servers.context7]
enabled = true
required = true                         # Fail startup if unavailable
command = "npx"
args = ["-y", "@upstash/context7-mcp"]
env = { "MY_VAR" = "value" }            # Static env vars
env_vars = ["PATH", "HOME"]             # Forward host env vars
cwd = "/path/to/project"                # Optional working directory
startup_timeout_sec = 10
tool_timeout_sec = 60
enabled_tools = ["search", "summarize"]  # Tool allowlist
disabled_tools = ["slow-tool"]           # Tool denylist
```
**HTTP servers** (remote):

```
[mcp_servers.figma]
enabled = true
url = "https://mcp.figma.com/mcp"
bearer_token_env_var = "FIGMA_OAUTH_TOKEN"
http_headers = { "X-Figma-Region" = "us-east-1" }
env_http_headers = { "X-Org-Id" = "FIGMA_ORG_ID" }  # Headers from env vars
startup_timeout_sec = 10
tool_timeout_sec = 60
```
### CLI Management

```
codex mcp add context7 -- npx -y @upstash/context7-mcp
codex mcp add context7 --env API_KEY=... -- npx -y @upstash/context7-mcp   # With env vars
codex mcp add figma --url https://mcp.figma.com/mcp --bearer-token-env-var FIGMA_OAUTH_TOKEN
codex mcp list                          # List all configured servers
codex mcp list --json                   # JSON output
codex mcp get context7                  # Show server config
codex mcp get context7 --json           # JSON output
codex mcp login <server>                # OAuth flow for HTTP servers
codex mcp logout <server>               # Remove OAuth credentials
codex mcp remove <server>               # Delete server definition
```
In-session: `/mcp` shows active servers and available tools. `/mcp verbose` (v0.123.0+)<sup>84</sup> returns full server diagnostics, resources, and resource templates — useful when a server is failing to load or tools aren’t appearing where expected. Plain `/mcp` stays fast. As of **v0.144.0**, MCP tools can request authentication **interactively without an experimental opt-in** — a server that needs auth mid-session prompts through the normal flow instead of failing until you pre-authenticate with `codex mcp login`.<sup>112</sup>

Plugin MCP loading (v0.123.0+) accepts both the standard `mcpServers` schema and top-level server maps in `.mcp.json`, so plugins authored against either convention load cleanly.<sup>84</sup>

**MCP user verification (v0.155.0, supported Macs):** local TUI sessions on supported Macs can verify MCP requests with **Touch ID, signed by the Secure Enclave** (#43624, #43712, #43715). The verification identity is restricted to the workspace it was enrolled in (#43524), and verification carries natively into MCP tool continuations (#44346).<sup>165</sup>

### Running Codex AS an MCP Server

**Removed (v0.154.0, September 9, 2026).** The `codex mcp-server` command and the standalone `codex-mcp-server` binary are gone as of stable 0.154.0 (#42993), completing the August 24, 2026 deprecation. The official changelog (September 5, 2026): “The `codex mcp-server` command and standalone `codex-mcp-server` binary have been removed after their deprecation on August 24, 2026. Update integrations that launch either command before upgrading Codex.” Integrations should target the app-server protocol, the interface behind the VS Code extension and desktop app (authentication, conversation history, approvals, streamed agent events) – which the removal entry itself labels experimental and “isn’t supported for production workloads” – or, from Claude Code, the Codex plugin for Claude Code: “Use Codex from Claude Code to review code or delegate tasks.” Connecting Codex TO external MCP servers (`codex mcp ...`, `/mcp`) is unaffected.<sup>134</sup><sup>161</sup><sup>162</sup>


On CLI 0.153.4 and earlier the command still exists (deprecated since August 24, 2026; since v0.149.0 it prints ``warning: `codex mcp-server` is deprecated and will be removed in a future release.`` to stderr and then starts anyway, PR #39657), so pinning `npm install -g @openai/codex@0.153.4` is the stopgap if an integration cannot migrate immediately:<sup>12</sup><sup>134</sup><sup>157</sup><sup>161</sup>

```
codex mcp-server                        # Removed in 0.154.0; works only on 0.153.x and earlier (deprecated, warns)
```
On 0.153.x and earlier the server exposes two tools:
1. **`codex()`**: Start a new session with prompt, sandbox, model, and approval parameters
2. **`codex-reply()`**: Continue an existing session with `threadId` and prompt

**Use with the Agents SDK (Python) – CLI 0.153.4 or earlier only:**

```
from agents import Agent, Runner
from agents.mcp import MCPServerStdio
async with MCPServerStdio(
    name="Codex CLI",
    params={"command": "npx", "args": ["-y", "@openai/codex@0.153.4", "mcp-server"]},  # pin: removed in 0.154.0
    client_session_timeout_seconds=360000,
) as codex_mcp_server:
    agent = Agent(name="Developer", mcp_servers=[codex_mcp_server])
    result = await Runner.run(agent, "Fix the failing tests")
```
### Notable MCP Servers

| Server | Purpose | Install | 
|---|---|---|
| Context7 | Up-to-date library docs | `npx -y @upstash/context7-mcp` | 
| Figma | Design file access | HTTP: `https://mcp.figma.com/mcp` | 
| Playwright | Browser automation | `npx -y @playwright/mcp` | 
| Sentry | Error monitoring | HTTP: `https://mcp.sentry.dev/mcp` | 
| GitHub | Repository operations | HTTP: `https://api.githubcopilot.com/mcp/` (GitHub’s official MCP server) | 

### Practical Patterns

**Pattern 1: Context-aware development** — Pair Context7 with your framework docs so Codex always has current API references:

```
[mcp_servers.context7]
enabled = true
required = true
command = "npx"
args = ["-y", "@upstash/context7-mcp"]
```
**Pattern 2: Output limits**: a tool’s output is cut to the active model’s output budget before it is stored in history, and that budget is 10,000 tokens for every model in the catalog bundled with v0.167.1. `tool_output_token_limit` changes it globally, and `mcp_servers.<id>.tools.<tool>.output_token_limit` (v0.152.0+) changes it for one MCP tool. For tools that return large payloads (database queries, log captures), use `enabled_tools` to restrict to specific tools and keep responses focused.<sup>205</sup><sup>206</sup>

**Pattern 2a: Multimodal tool output (v0.107.0)** — Custom tools can now return multimodal output (images, rich content) alongside text. This enables tools that produce visual artifacts — screenshots, diagrams, chart renders — to pass them directly to the model for analysis.<sup>62</sup>

**Pattern 3: Enterprise MCP governance.** Lock down which MCP servers developers can use via `requirements.toml`. The server name must match the name in the user’s config, and the identity must match how that server is launched:

```
# In /etc/codex/requirements.toml: only approved servers stay enabled.
# Matches a server the user configures as: command = "npx", args = ["@company/internal-mcp"]
[mcp_servers.approved-internal.identity]
command = { executable = "npx", args = [
  { match = "exact", value = "@company/internal-mcp" },
] }
```
The shorter string form, `identity = { command = "npx" }`, compares only the configured `command` and ignores the arguments, so it would approve any `npx` package under that server name. The structured form above pins the invocation: the executable, the argument count, and the argument order must all match (`exact`, `prefix`, and `regex` matchers are available). For remote servers, match on `identity = { url = "..." }`. A server that is not listed or does not match is disabled, not started, and an `mcp_servers` table that is present but empty disables every MCP server.<sup>219</sup> See Enterprise Deployment for full policy configuration.

## Code Mode (experimental)

Code mode (first shipped as an experiment in v0.114.0) is a way of calling tools, not a limit on what the agent may do.<sup>70</sup> Instead of one tool call per step, the model writes JavaScript that Codex evaluates “in a fresh V8 isolate as an async module,” with the session’s tools on a global `tools` object (`await tools.exec_command(...)`, MCP tools included), so one script can chain several calls. The script itself gets “no Node, no file system, no network access, no console”: everything it does goes through those tools, and hook decisions apply to each nested call. The `code_mode` feature flag is under development and off by default at v0.167.1; `features.code_mode.excluded_tool_namespaces` keeps named tool namespaces out of it and `direct_only_tool_namespaces` limits them to direct calls. Separately, the catalog bundled with the CLI marks the current GPT-6 and GPT-5.6 models `code_mode_only` in their `tool_mode` field.<sup>205</sup><sup>211</sup>

As of v0.139.0, code mode can call **standalone web search directly**, including from nested JavaScript tool calls, and receives plaintext results, so a code-mode flow can fetch live information without leaving the sandboxed coding context.<sup>106</sup> v0.146.0 extends standalone web search to **compatible custom model providers**, so the capability is no longer limited to OpenAI-hosted models.<sup>121</sup> At v0.167.1 the standalone web search tool has its own flag, `standalone_web_search`, which is under development and off by default, like `code_mode` itself.<sup>247</sup>

v0.146.0 let app-server connect to **remote Code Mode hosts**, so the code-mode runtime no longer has to live on the same machine as the client – originally over WebSocket.<sup>121</sup> **v0.151.0 removes the WebSocket transport** (#40692): app server `--code-mode-host` connections accept only `http://` and `https://` gRPC endpoints, and the standalone code-mode host is limited to stdio and gRPC listeners – at tag rust-v0.151.0 its listener doc reads verbatim “Transport endpoint: `stdio`, `stdio://`, or `grpc://IP:PORT`.” WebSocket survives only as an optional endpoint that streams raw OTLP trace batches for tracing, not as a session transport.<sup>145</sup>

Code mode is under development and off by default at v0.167.1. To try it, set `enabled = true` under `[features.code_mode]`, and expect it to change between releases.<sup>247</sup>


## JavaScript REPL Runtime (removed)

Codex v0.100.0 added an experimental JavaScript REPL runtime (`js_repl`) and v0.106.0 promoted it through the `/experimental` surface.<sup>60</sup> That guidance is now historical. In v0.128.0, the release changelog includes “Remove js_repl feature,” and the current feature list marks both `js_repl` and `js_repl_tools_only` as removed.<sup>86</sup>

Do not add `features.js_repl = true` to new configs. Use shell commands, checked-in scripts, MCP tools, or a Codex skill with a `scripts/` directory when you need repeatable executable logic.

## What Are Skills?

Skills are reusable, task-specific capability packages that Codex loads on demand. They follow the open agent skills standard.<sup>13</sup>

### Skill Structure

```
my-skill/
  SKILL.md           (required: instructions)
  scripts/           (optional: executable scripts)
  references/        (optional: reference docs)
  assets/            (optional: images, icons)
  agents/openai.yaml (optional: metadata, UI, dependencies)
```
### Discovery Locations

Codex reads your personal skills from `~/.agents/skills` (`$HOME/.agents/skills`), the user location the skills documentation lists today. `$CODEX_HOME/skills` (default `~/.codex/skills`) is the older user location: the docs no longer list it, and the source at v0.167.1 still reads it but labels it the “Deprecated user skills location (`$CODEX_HOME/skills`), kept for backward compatibility,” so keep new skills in `~/.agents/skills` and move old ones when convenient. Bundled system skills are installed under `$CODEX_HOME/skills/.system`. Codex supports symlinked skill folders.<sup>210</sup>

| Scope | Path | 
|---|---|
| Project/team | `.agents/skills/` in every directory from the current working directory up to the repository root | 
| User | `~/.agents/skills/` (deprecated but still read:`~/.codex/skills/` ) | 
| Admin | `/etc/codex/skills/` | 
| System | Bundled by OpenAI, including `skill-creator` and`skill-installer` (installed under`~/.codex/skills/.system/` ) | 

### Creating a Skill

**SKILL.md format:**

```
---
name: security-audit
description: Run a thorough security audit on the codebase.
---
## Security Audit Procedure
1. Scan for hardcoded secrets using `rg -i "(api_key|password|secret|token)\s*=" --type py`
2. Check for SQL injection: look for string interpolation in queries
3. Verify input validation on all API endpoints
4. Check dependency vulnerabilities: `pip-audit` or `npm audit`
5. Review authentication and authorization patterns
6. Report findings with severity levels (Critical/High/Medium/Low)
```
**Metadata (agents/openai.yaml):**

```
interface:
  display_name: "Security Audit"
  short_description: "Full codebase security review"
  icon_small: "./assets/shield.svg"
  brand_color: "#DC2626"
  default_prompt: "Run a security audit on this repository"
policy:
  allow_implicit_invocation: false    # Require explicit $skill
dependencies:
  tools:
    - type: "mcp"
      value: "securityScanner"
      description: "Vulnerability scanner MCP server"
      transport: "streamable_http"
      url: "https://mcp.example.com/mcp"   # placeholder: your scanner's MCP endpoint
```
A `dependencies.tools` entry takes `type`, `value`, `description`, `transport`, and `url`; the URL above is a placeholder for a server you run or trust.<sup>250</sup>

### Invoking Skills

- **Explicit** :`/skills` menu or`$skill-name` mention in prompt
- **Implicit** : Codex auto-detects matching skills from task description (if`allow_implicit_invocation: true` )
- **Creator** : Use`$skill-creator` to interactively build a new skill
- **Installer** : Use`$skill-installer <name>` to add a curated skill (for example`$skill-installer linear` ); you can also ask the installer to download skills from other repositories<sup>250</sup>
- **Executor-provided (v0.146.0)** : Codex discovers skills offered by the executor and reads their associated resources, including skills you select explicitly<sup>121</sup>

**Context budgets truncate skill catalogs.** v0.146.0 retains more available skills under tight context and **warns when a catalog has to be truncated**.<sup>121</sup> Before this, a large skill library could be silently trimmed, so a skill you expected to be in scope simply was not offered. If you rely on implicit invocation across many skills, watch for that warning.


### Enable/Disable

```
[[skills.config]]
path = "/path/to/skill/SKILL.md"
enabled = false
```
### Skills vs Slash Commands

|  | Skills | Slash Commands | 
|---|---|---|
| **Defined in** | `SKILL.md` files with optional metadata | Built into Codex CLI binary | 
| **Scope** | Project, user, or admin level | Global (always available) | 
| **Invocation** | `$skill-name` in prompt,`/skills` menu, or implicit detection | `/command` syntax | 
| **Customizable** | Fully — you write the instructions | Fixed behavior | 
| **Dependencies** | Can declare MCP server requirements | None | 
| **Sharing** | Commit the skill folder under the repo’s `.agents/skills/` , or copy it to`~/.agents/skills/` | Not shareable | 

### Debugging Skills

If a skill isn’t activating:

1. Check discovery: `/skills` should list it in the TUI
2. Verify path: Ensure the skill folder is in a recognized location (`~/.agents/skills/` , a repository`.agents/skills/` folder, or`/etc/codex/skills/` )
3. Check `enabled` : Skills with`enabled = false` in config.toml won’t load
4. Check implicit activation: If relying on auto-detection, ensure `allow_implicit_invocation: true` in`agents/openai.yaml`
5. Use keywords: Include the skill’s `description` terms in your prompt to improve implicit matching

### Production Example: Deploy Skill

A complete multi-file skill showing references and scripts working together:

```
deploy-skill/
  SKILL.md
  references/
    runbook.md
    rollback-checklist.md
  scripts/
    pre-deploy-check.sh
    smoke-test.sh
  agents/openai.yaml
```
**SKILL.md:**

```
---
name: deploy
description: Deploy the application to staging or production. Runs pre-flight checks, executes deployment, and verifies with smoke tests.
---
## Deployment Procedure
### Pre-flight
1. Run `scripts/pre-deploy-check.sh` to verify:
   - All tests pass
   - No uncommitted changes
   - Branch is up to date with remote
2. Review the runbook at `references/runbook.md` for environment-specific steps.
### Deploy
3. Execute the deployment command for the target environment.
4. Monitor logs for errors during rollout.
### Verify
5. Run `scripts/smoke-test.sh <environment-url>` to confirm critical paths.
6. If smoke tests fail, follow `references/rollback-checklist.md`.
```
Invoke with: `$deploy to staging` or `$deploy production with canary rollout`

## Plugins

Plugins unify skills, MCP entries, hooks, and app connectors into a single installable package (v0.110.0+).<sup>65</sup> As of v0.117.0, plugins are first-class citizens: product-scoped plugins sync automatically at startup and `/plugins` provides an in-TUI browser for discovery and management.<sup>75</sup> v0.128.0 expanded plugin workflows with marketplace installation, remote bundle caching, remote uninstall APIs, plugin-bundled hooks, hook enablement state, and external-agent config import.<sup>86</sup> **v0.129.0 (May 7, 2026)** adds **plugin workspace sharing** (push a plugin set to teammates without re-publishing), **share access controls** (per-recipient enable/disable, revoke), **source filtering** (limit which marketplaces a workspace pulls from), and **marketplace operations** that can be invoked directly from the `/plugins` browser instead of the CLI.<sup>89</sup> **v0.133.0 (May 21, 2026)** makes plugin discovery easier to audit: list output is marketplace-aware, installed versions are visible, marketplace roots are listed, and remote plugin collections can be surfaced without guessing which registry a result came from.<sup>98</sup> **v0.130.0 (May 8, 2026)** makes plugin packaging more transparent and the share workflow more controllable:<sup>91</sup>

- **Bundled hooks visible in plugin details.** The`/plugins` detail view now lists every lifecycle hook a plugin bundles (`SessionStart` ,`UserPromptSubmit` ,`Stop` , etc.). Before installing a plugin, you can see exactly which hooks it will register against your session — no more surprise hook side effects from a plugin you trust on its tools alone.
- **Plugin share metadata in `shareContext`.** When a plugin is shared from a workspace, the share-link payload now exposes link metadata (creator, scope, freshness) so receiving sessions can show provenance and decide whether to accept.
- **Discoverability controls on share settings.** Share settings expose a discoverability toggle so teams can publish plugins to specific workspaces or recipient lists without making them generally listable across an organization.

**v0.143.0 (July 8, 2026) turns remote plugins on by default.** Remote plugins are now enabled without an opt-in flag, the `/plugins` catalog gains richer rows (npm marketplace sources alongside git and local), and each entry shows both its remote and locally installed versions so drift is visible at a glance.<sup>111</sup> For a team standardizing on a shared plugin set, remote-by-default removes the per-machine enablement step that used to gate rollout.

### Plugin Sources

| Source | Location | Description | 
|---|---|---|
| **Config** | `config.toml` | Manually declared plugins | 
| **Local marketplace** | `marketplace.json` | Project-local plugin catalog | 
| **Install endpoint** | App server v2 | Remote plugin installation | 
| **Product-scoped** | Synced at startup | Automatically synchronized plugins (v0.117.0+) | 
| **Portable Agent Plugins** | Installable packages | Plugin packages in the cross-vendor Agent Plugins 1.0 format — portable across Codex, ChatGPT, Cursor, VS Code, Copilot, and Kiro, not just across machines (v0.147.0+) <sup>126</sup> | 

As of **v0.147.0**, plugin search spans **local, personal, workspace, and remote catalogs** in one query — you search for a capability, not for the catalog that happens to hold it. Combined with portable Agent Plugin packages, a plugin built in one environment installs cleanly in another instead of being tied to the machine or workspace where it was authored.<sup>123</sup>

### Plugin Discovery

Codex tells the model which plugins are enabled at session start (v0.111.0), improving discovery of installed MCPs, apps, and skills.<sup>65</sup> The model can suggest relevant plugins during a session based on the task context. In v0.117.0, product-scoped plugins are synced at startup, ensuring the latest plugin catalog is available without manual intervention.<sup>75</sup> As of v0.142.0, the `/plugins` menu organizes remote plugins into **OpenAI Curated**, **Workspace**, and **Shared with me** sections, and eligible turns can recommend and install relevant plugins inline.<sup>109</sup> As of v0.151.0, `plugin/list` and `plugin/installed` load the effective plugin configuration for every requested `cwd` – local marketplaces combine in request order, installed and enabled state merges across repositories, and invalid project configurations are reported through `marketplaceLoadErrors` without dropping valid local catalogs or globally enabled remote catalogs.<sup>144</sup>

### @plugin Mentions (v0.112.0+)

Reference any installed plugin directly in chat with `@plugin-name`.<sup>68</sup> When you mention a plugin, its context (capabilities, tools, configuration) is automatically included in the model’s context window — no need to describe what the plugin does.

```
@deploy push this branch to staging with canary rollout
@linter check for unused imports in src/
```
This works with any installed plugin, including custom skills, MCP servers, and app connectors.

### Plugin Marketplace (v0.113.0+)

The plugin marketplace now includes richer discovery with metadata, categories, and ratings.<sup>69</sup> Install-time auth checks verify that plugins requiring API keys or OAuth have valid credentials before installation. An uninstall endpoint cleanly removes plugins and their associated configuration.

### Adding Third-Party Marketplaces (v0.121.0+)

Current OpenAI Codex docs keep marketplace source management under `codex plugin marketplace`. This formalizes third-party plugin distribution beyond OpenAI’s first-party marketplace and supports GitHub repo shorthand, HTTP(S) Git URLs, SSH URLs, and local marketplace root directories; use `--ref` to pin a Git ref and repeat `--sparse PATH` only for Git-backed marketplace repos.<sup>93</sup>

```
# GitHub repository (shorthand)
codex plugin marketplace add owner/repo
# Arbitrary git URL
codex plugin marketplace add https://git.example.com/team/plugins.git
# SSH Git URL
codex plugin marketplace add git@example.com:team/plugins.git
# Local directory
codex plugin marketplace add /path/to/local/marketplace
# Upgrade or remove a configured marketplace
codex plugin marketplace upgrade <marketplace-name>
codex plugin marketplace remove <marketplace-name>
```
Once added, a marketplace’s plugins appear in the `/plugins` browser alongside the default ones. App-server callers (IDE/desktop integrations) have a parallel endpoint to register marketplaces programmatically.<sup>82</sup>

**Security consideration**: Third-party marketplaces run arbitrary plugin code with your Codex permissions. Vet sources before adding them, and prefer sandboxed execution during first runs.


### Agent Plugins: the Cross-Vendor Standard (v0.146.0–v0.147.0)

“Agent Plugins” is not just a Codex feature name — it is an **open, vendor-neutral packaging standard**, published as **Agent Plugins 1.0.0 on August 6, 2026**, whose official site summarizes it as “A portable package format for reusable components that extend AI agents.”<sup>126</sup> A plugin is a directory with a required `plugin.json` manifest (a closed schema of exactly ten permitted top-level fields — `$schema`, `name`, `version`, `description`, `author`, `homepage`, `repository`, `license`, `keywords`, `extensions` — where “Clients MUST report and ignore each unknown field”), an optional `skills/` folder (each immediate child directory containing a `SKILL.md` is one skill), an optional `mcp.json` declaring stdio, Streamable HTTP, or legacy HTTP+SSE servers, and optional reverse-domain client-namespace directories that other clients “MUST ignore.” Its normative scope is packaging: the spec “defines the canonical Agent Plugins Specification v1.0.0 for packaging reusable components that extend AI agents into distributable plugins” — Agent Skills and MCP keep their own specifications underneath it.<sup>126</sup>

The governance is the story. Vercel initiated the proposal and developed the 1.0 specification with Amazon, Anysphere (Cursor), GitHub, Microsoft, and OpenAI; the spec repository’s maintainer roster spans Amazon, Cursor, Microsoft, OpenAI, and Vercel, and Google announced on launch day that it is joining the core maintainers.<sup>126</sup> The named launch clients span ChatGPT, Codex, Cursor, GitHub Copilot, Kiro, and VS Code. One conspicuous absence: **Anthropic, which created the underlying Agent Skills format and MCP itself, is not among the maintainers** — Claude Code keeps its own plugin format, and Codex bridges the gap from its side with the Claude Code marketplace source (v0.146.0) and `/import`.<sup>126</sup>

In Codex terms: **v0.146.0** adds Agent Plugins manifests, **workspace plugin publishing**, and two additional plugin marketplaces — **Amazon Bedrock** and **Claude Code**<sup>121</sup> — and **v0.147.0** completes the loop with portable Agent Plugins installation and search across local, personal, workspace, and remote catalogs.<sup>123</sup> Workspace publishing is the operationally useful half for teams: publish plugins into your own workspace instead of standing up a public marketplace or asking every developer to add a Git source by hand.

The release notes announce the Codex surfaces without publishing their configuration keys or CLI syntax. Codex supports the capability, but how to drive it from the CLI is not documented yet. At v0.167.1 `codex plugin` has four subcommands (`add`, `list`, `marketplace`, `remove`) and no `publish`; treat the manifest-authoring workflow as unverified until OpenAI’s docs cover it.<sup>245</sup>


### Managing Plugins

```
codex plugin marketplace add <src>       # Add a marketplace source
codex plugin marketplace upgrade [name]  # Upgrade one marketplace or all
codex plugin marketplace remove <name>   # Remove a configured marketplace
codex plugin list                        # List plugins; includes remote catalog entries (v0.153.0)
codex plugin list --json                 # JSON adds source, version, install + auth policy (v0.153.0)
```
**v0.153.0 extends the plugin CLI from marketplace management to plugin-level operations against remote marketplaces**: `codex plugin list` includes remote catalog entries (with their source, version, install policy, and authentication policy in JSON output), and remote plugins can be added and removed through the existing plugin CLI. Remote catalogs are cached by scope and collection – fresh cached results are preferred, an add request that misses a plugin in the cache refetches once, and the local curated catalog is preserved when an unfiltered remote listing fails (errors still surface for explicitly selected remote marketplaces).<sup>149</sup>

In the TUI, use `/plugins` (v0.117.0+) to browse, install, and remove individual plugins interactively without leaving your session.<sup>75</sup>

**Expert tip**: Plugins consolidate what used to require separate MCP config, skill installation, and app connector setup. A single plugin can bundle all three — making team onboarding faster and configuration more portable.


## Plan Mode & Collaboration

Plan mode lets Codex design an approach before executing changes. It’s enabled by default (since v0.94.0).<sup>14</sup> See Decision Frameworks for the “Plan Mode vs Direct Execution” decision tree.

**Plan mode vs the `update_plan` tool (v0.152.0):** these are two different things, and only one of them is on by default. **Plan mode** – the collaboration mode this section describes, where Codex proposes before executing – has been enabled by default since v0.94.0 and is unchanged. **`update_plan`** is a planning *tool* the model can call to maintain a structured plan inside a turn, and it shipped **opt-in** in v0.152.0: `tools.update_plan.enabled` defaults to `false`, and while it is disabled Codex also strips the bundled `update_plan` guidance from its model, collaboration-mode, multi-agent, compaction, prewarm, and goal-continuation prompts (custom base instructions, model catalog instructions, collaboration policies, and user goal text are preserved even when they mention planning). Enable it explicitly with `tools.update_plan.enabled = true` in `config.toml`.<sup>147</sup>


### Entering Plan Mode

```
/plan                              # Switch to plan mode
/plan "redesign the API layer"     # Plan mode with initial prompt
```
In plan mode, Codex:

- Reads files and analyzes the codebase
- Proposes an implementation plan
- Does **not** make changes until you approve
- Streams the plan in a dedicated TUI view

### Steer Mode

Steer mode (enabled by default since v0.98.0) allows you to inject new instructions while Codex is actively working, without interrupting its current task.<sup>14</sup>

There are two injection methods:

| Input | Behavior | When to Use | 
|---|---|---|
| **Enter** | Sends instructions immediately; Codex sees them during the current turn | Urgent corrections (“stop — don’t modify that file”), clarifications (“the config is in `/etc/app.conf` , not the default path”), or priority changes (“focus on the tests first”) | 
| **Tab** | Queues instructions for the next turn; Codex finishes its current work first | Follow-up tasks (“after this, also update the changelog”), scope additions (“when you’re done, run the linter”), or non-urgent context (“the deploy target is staging, not prod”) | 

**Practical examples:**

```
# Codex is refactoring the auth module...
[Enter] "Use bcrypt instead of argon2 — we already have it as a dependency"
→ Codex adjusts immediately, mid-turn
[Tab] "Once auth is done, update the migration script too"
→ Codex finishes auth refactor, then starts the migration
```
Steer mode is always active in the TUI. If you prefer to wait until Codex finishes before giving instructions, just type normally after the turn completes — no special mode needed.

### TUI Enhancements (v0.105.0–v0.106.0)

**Syntax highlighting (v0.105.0):** The TUI now syntax-highlights fenced code blocks and diffs inline. Use `/theme` to choose a color scheme.<sup>61</sup>

**New TUI commands (v0.105.0+):**<sup>61</sup>

| Command / Key | Description | 
|---|---|
| `/copy` | Copy picker (v0.150.0+): choose the full response, an individual code block, or a blockquote; earlier versions copy the last response <sup>141</sup> .**v0.162.0:** navigate and copy transcript blocks from the picker, and`Ctrl+Insert` copies a mouse selection (#50434, #50215)<sup>188</sup> | 
| `/clear` | Clear the terminal and start a new chat; `/clear <name>` names the new chat (use`Ctrl+L` to clear the display and keep the conversation)<sup>217</sup> | 
| `Ctrl+L` | Clear the terminal display; since v0.167.0 earlier messages stay available through `Ctrl+T`<sup>200</sup> | 
| `/theme` | Preview and persist a syntax-highlighting theme; v0.156.0 adds six bundled themes ( `ada` ,`babbage` ,`curie` ,`cushman` ,`dali` ,`davinci` ) and theme-aware accents (#46504)<sup>170</sup> | 
| `/plugins` | Browse, install, and remove plugins (v0.117.0+) <sup>75</sup> | 
| `/title` | Configure which items appear in the terminal title; works in TUI and app-server TUI (v0.117.0+) <sup>75</sup> | 
| `/archive` | Archive the current session; archived sessions are protected from resume/fork until restored (v0.136.0+) <sup>103</sup> | 

**Voice transcription (v0.105.0, removed in v0.118.0):** v0.105.0 added experimental hold-the-spacebar dictation in the composer, behind the `voice_transcription` flag.<sup>61</sup> v0.118.0 removed it (PR #16114, “Remove TUI voice transcription feature”), and the TUI has no dictation key at v0.167.1. For voice in the terminal, use `/voice`.<sup>237</sup> As of v0.107.0, realtime voice sessions support microphone and speaker device selection, allowing you to choose specific audio input/output hardware.<sup>62</sup> **Removed in v0.140.0, returned in v0.145.0:** the experimental `/realtime` voice controls and their audio dependencies were dropped from the TUI in v0.140.0.<sup>107</sup> v0.145.0 brings voice back in a new form: **audio inputs and audio tool outputs** in common local audio formats, plus **streaming realtime V3 conversations**.<sup>117</sup>

**v0.155.0 and v0.156.0: live voice conversations.** v0.155.0 (September 17, 2026) added `/voice` as an experimental, opt-in feature: a live conversation with a streaming transcript and microphone controls, enabled through `/experimental` on supported builds (#43581, #43651, #44331), with a configurable mute shortcut (#43690) and signed voice resources in the macOS packages (#43983). v0.156.0 (September 22, 2026) made it stable and on by default (#44921), and the docs changelog announced it the same day: “Voice conversations are enabled by default, with an F8 toggle, a `/voice settings` picker, and bundled audio runtimes for Linux and Windows.” The off switch that shipped with it, `realtime_conversation = false` under `[features]`, stopped working in v0.164.0, which removed the flag with the feature left on.<sup>197</sup><sup>201</sup> The CLI slash-command reference lists `/voice` now (it did not on September 22), and the docs Voice feature page covers ChatGPT Voice in the desktop app, a separate surface.<sup>165</sup><sup>170</sup>

**Other improvements:**

- Long links now remain clickable even when wrapped across TUI lines (v0.105.0)<sup>61</sup>
- Local file links render with improved formatting (v0.106.0)<sup>60</sup>
- TUI markdown keeps web links clickable via OSC 8 metadata, and cramped tables fall back to readable key/value records without losing link targets (v0.136.0)<sup>103</sup>
- Markdown links appear as clickable labels in supported terminals, with visible URLs retained elsewhere (v0.150.0)<sup>141</sup>
- `Ctrl+C` handling for sub-agents fixed to properly terminate child processes (v0.106.0)<sup>60</sup>

## Memory System

Codex has a local memory system (v0.100.0+) that carries useful context from earlier sessions into later ones.<sup>24</sup> It is off by default: turn it on with `memories = true` under `[features]`. Once on, Codex writes the memories itself, in the background, from eligible earlier chats; you do not enter them by hand.<sup>209</sup>

### Memory Commands

| Command | Description | 
|---|---|
| `/memories` | Choose whether Codex uses existing memories, generates new ones, or keeps memory behavior off, without leaving the TUI | 
| `codex debug clear-memories` | Reset local memory state for a fresh start (v0.107.0+; a hidden, internal subcommand at v0.167.1) <sup>62</sup><sup>209</sup> | 

Memories are generated files under your Codex home: `~/.codex/memories/` (v1) or, for a profile that turned memories on with no v1 store present, `~/.codex/memories_v2/`. Codex skips active or short-lived sessions, redacts secrets from generated memory fields, and updates memories in the background rather than at the end of every chat. The docs say to “treat these files as generated state,” so inspect them when troubleshooting but do not rely on hand edits. There is no `/m_update` or `/m_drop` command.<sup>209</sup>

### What to Store

Memories are a recall layer for preferences and project facts that came up in earlier sessions, such as:

- **Project conventions** : “This project uses tabs, not spaces” or “API responses always include a`meta` field”
- **Tool preferences** : “Use`pnpm` instead of`npm` ” or “Run tests with`pytest -x --tb=short` “
- **Architecture decisions** : “The auth module is in`src/core/auth/` , not`src/middleware/` “
- **Workflow preferences** : “Always run the linter before showing me a diff”

Rules that must hold every time belong in `AGENTS.md`: the docs tell you to “treat memories as a helpful recall layer, not as the only source for rules that must always apply.”<sup>209</sup>

### Memory in Pipelines

The memories documentation describes interactive sessions and does not say whether `codex exec` runs read the store. For CI/CD pipelines and scripts, put the instructions a run needs in `AGENTS.md` or in the prompt instead of counting on memories.<sup>209</sup>

### Memory Refinements (v0.101.0 onward)

- **Secret sanitization** : Memories are automatically scanned for secrets before being written to disk
- **CWD awareness** : Memory files now include the working directory context for project-specific recall
- **Developer message exclusion** : Developer/system messages are excluded from phase-1 memory input, improving memory quality by focusing on user interactions
- **Diff-based forgetting (v0.106.0)** : Memory now uses diff-based forgetting to remove stale facts, keeping the memory store lean and relevant over time<sup>60</sup>
- **Usage-aware selection (v0.106.0)** : Memory retrieval is now usage-aware, prioritizing frequently accessed and recently relevant memories<sup>60</sup>
- **Configurable memories (v0.107.0)** : Memories are now fully configurable. Use`codex debug clear-memories` to reset all stored memories for a clean slate — useful when switching contexts between unrelated projects or when memory state has drifted<sup>62</sup>
- **Phase-2 model upgrade (v0.121.0)** : the phase-2 consolidation model moved to`gpt-5.4` at the time. The phase-2 pipeline runs between sessions to distill phase-1 transcripts into durable facts. That default has since been replaced (see the v0.167.0 entry below).<sup>82</sup>
- **TUI memories menu (v0.121.0)** : A new in-session UI exposes memory mode, individual memory deletion, and a reset button. Memory reset now**preserves past rollouts** rather than invalidating them, so resetting clears the future recall surface without breaking session replay.<sup>82</sup>
- **Memory v2 for fresh profiles (v0.165.0)** : a profile with no`~/.codex/memories/` directory starts on memory v2, stored under`~/.codex/memories_v2/` ; existing v1 profiles stay on v1,`memories.version` (`v1` or`v2` ) sets it explicitly, and`memories.dual_write = false` opts a fresh profile out. Importing memories from external agents into v2 sits behind the`external_agent_memory_import` flag, off and under development at v0.166.0.<sup>198</sup><sup>199</sup>
- **Default memory models updated (v0.167.0)** : the default models for memory extraction and consolidation changed (#58091). At v0.167.1 the built-in defaults are`gpt-6-luna` for extraction and`gpt-6.1-terra` for consolidation;`memories.extract_model` and`memories.consolidation_model` override them.<sup>200</sup><sup>206</sup><sup>226</sup>

### Memory vs AGENTS.md

| Use Case | Memory (generated) | AGENTS.md | 
|---|---|---|
| Personal preferences | Use memory (persists across all projects) | Not appropriate | 
| Project conventions | Either (memory for personal recall, AGENTS.md for team sharing) | AGENTS.md for team | 
| Architecture decisions | AGENTS.md (shared context) | Primary choice | 
| Tool commands | Memory (quick personal reference) | AGENTS.md for team | 

**Tip**: Leave memory generation on (`/memories`) for sessions whose context is worth recalling later, and turn it off for throwaway ones. For session-specific context, just tell Codex directly in the conversation. For shared team context and anything that must always apply, use AGENTS.md.


## Session Management

Codex persists sessions under `~/.codex/sessions/`, enabling resume, forking, and multi-thread workflows across CLI and desktop surfaces.

**v0.146.0 makes threads addressable.** `/new` and `/clear` now take a session name, important threads can be pinned, and side conversations stay open instead of forcing you to close one to reach another.<sup>121</sup> The practical effect is that a long-running project stops being a list of timestamps you have to recognize by date. **v0.147.0 adds conversation sections**: conversations organize into persistent, manually ordered sections, and long transcripts browse incrementally instead of loading whole.<sup>123</sup> **v0.148.0 adds `/export` and command-line forking**: `/export` writes the complete conversation to Markdown (clipboard or a new file), `codex exec fork` forks a session non-interactively, and the resume picker can archive and restore sessions.<sup>127</sup> **v0.149.0 adds `codex queue`** — send messages to existing local or remote sessions from the command line — and fixes resumed/forked threads to restore their active permission profile.<sup>129</sup> **v0.150.0 adds automatic titles**: unnamed terminal tasks receive descriptive titles automatically, and `/rename` suggests an editable title based on the conversation.<sup>141</sup> **v0.151.0 adds `/recap`**: request a short summary of the conversation’s recent progress and next steps on demand; Codex also generates the same recap automatically for eligible idle, unfocused conversations and adds it to the transcript, with bounded, isolated requests and retries only for automatic failures.<sup>145</sup> **v0.153.0 makes the automatic half optional**: `tui.auto_recap = false` cancels scheduled automatic recap checks and discards pending automatic results, while manual `/recap` keeps working.<sup>149</sup>

**`codex agents` (v0.149.0)** is the interactive dashboard for every active agent session on the machine: search, start, open, rename, and stop tasks from one screen, with configurable shortcuts. Inside a session, `/agents` switches between all active agent sessions and `/subagents` switches between the current session’s subagents.<sup>129</sup><sup>138</sup> **v0.150.0 makes tasks addressable from the conversation**: reference other Codex tasks with `@` mentions, and ask agents to read, create, or message tasks from the terminal.<sup>141</sup> **v0.166.0 adds `codex queue --resume`**: after queueing a message to a thread that is not loaded, Codex tries to resume it on the local daemon or the selected `--remote` server; a failed resume warns and the queue still succeeds.<sup>198</sup><sup>199</sup> **v0.167.0 adds `codex agents --no-daemon`**: the command center opens in an embedded session, without the shared background server, and both it and the session picker mark sessions with unread results.<sup>200</sup>

### Resume

Pick up where you left off:

```
codex resume                          # Interactive picker (sorted by recency)
codex resume <SESSION_ID>             # Resume a specific session
codex exec resume --last "continue"   # Non-interactive: resume most recent
```
The `/resume` slash command inside the TUI opens the same interactive picker with search.

**Paginated thread history (v0.145.0, promoted in v0.151.0):** Paginated thread history brings efficient resume, search across history, persisted thread names, sub-agent support, and memories to session management – history loads page by page instead of all at once, which matters for long-lived working directories with hundreds of sessions. Introduced as experimental in v0.145.0; **v0.151.0 promotes the APIs out of experimental** (`thread/turns/list`, `thread/items/list`, and `thread/revert` no longer need the experimental API capability) and makes paginated the resolved default only for durable threads whose store supports `thread/turns/list` and `thread/items/list`, and only when `thread/start` omits `historyMode`. Ephemeral threads and threads backed by stores without those APIs still default to legacy, explicit selections keep their behavior, and the protocol-level `ThreadHistoryMode` enum still defaults to `Legacy` at tag rust-v0.151.0 (`protocol.rs:755-758`) – a resolved default for supporting durable threads, not a blanket flip.<sup>117</sup><sup>145</sup>

### Fork

Branch a conversation to explore alternatives without losing your current progress:

```
/fork                              # Fork current conversation
/fork redis-experiment             # Fork and name the new chat
```
The text after `/fork` names the new chat; it is not sent as a prompt, so type the prompt once the fork opens.<sup>253</sup>

Forks create independent threads sharing the same history up to the fork point. As of v0.146.0, fork history is **paginated**, and forks can be created as **temporary** — a temporary fork does not appear in thread listings, which keeps throwaway experiments out of the thread list you actually navigate.<sup>121</sup> Changes in one fork don’t affect the other. This is useful for comparing approaches (e.g., “fork and try Redis instead of Memcached”) or exploring risky changes safely.

**Thread forking into sub-agents (v0.107.0):** Threads can now be forked into independent sub-agents, allowing a conversation to spawn parallel work streams that execute autonomously. This extends the existing fork model — instead of just branching the conversation, the forked thread becomes a sub-agent with its own execution context.<sup>62</sup> As of v0.117.0, sub-agents use path-based addresses (e.g., `/root/agent_a`) with structured inter-agent messaging, making multi-agent coordination more explicit and debuggable.<sup>75</sup>

**Encrypted inter-agent messages (v0.138.0+):** Multi-agent v2 now **encrypts the task text a parent agent sends when it spawns or messages a sub-agent**. The message parameters of `spawn_agent`, `send_message`, and `followup_task` are marked encrypted: the Responses API encrypts the payload when the parent model emits the tool call, the locally running Codex carries only ciphertext (`InterAgentCommunication.encrypted_content`, with empty plaintext content), and the API decrypts it server-side when handing it to the receiving model.<sup>113</sup> Three practical consequences:

- **Your local session logs no longer contain delegation plaintext.** You can see that a parent spawned a sub-agent and what came back, but not the exact instruction it sent. If you audit or debug multi-agent runs from session history, that record is gone; the open regression issue tracking this has no linked fix as of mid-July 2026.<sup>114</sup> Extension lifecycle events (subagent start/stop, tool execution, turn metadata — v0.133.0+) and app-server observability are the remaining hooks for reconstructing what happened.<sup>98</sup>
- **Custom-provider models can break.** A model endpoint that is not configured for encrypted tool use rejects the encrypted`spawn_agent` schema with a 400 (`"declares encrypted parameters but is not configured for encrypted tool use"` ). If multi-agent delegation fails with that error on a custom provider, that is the encryption change, not your config.<sup>114</sup>
- **Codex-generated notifications stay readable.** Child-completion notifications are produced by Codex itself, so they remain plaintext and now render as typed`agent_message` items rather than serialized JSON envelopes.<sup>113</sup>

OpenAI has not published the threat model behind the change; the PR describes the ciphertext flow without stating what it protects against.

**Multi-agent v2 stabilized (v0.145.0, opt-in):** The multi-agent v2 experience that debuted in v0.137.0 and gained encrypted delegation in v0.138.0 was **stabilized as an opt-in feature** in v0.145.0. You rarely need to opt in at v0.167.1: the `multi_agent_v2` flag is stable and still off by default, but the model catalog bundled with the CLI selects v2 for GPT-6 Astra, GPT-6.1 Sol and Terra, GPT-6 Sol and Luna, and GPT-5.6 Sol and Terra (GPT-5.6 Luna stays on v1), and `multi_agent_v2 = true` under `[features]` forces v2 for every model.<sup>247</sup> v0.145.0 adds configurable sub-agent models, per-sub-agent reasoning levels, configurable concurrency, restored multi-agent roles, and improved agent navigation. If you held off on v2 during its v0.137/v0.138 shakeout, this is the release where the configuration surface settles: sub-agent model and reasoning choices now live in config rather than being fixed by the runtime.<sup>117</sup>

### Thread Listing

View and manage active sessions:

```
/status                            # Current session info and token usage
/ps                                # Show background terminals in session
```
In the desktop app, threads are visible in the sidebar with full history and diff previews.

### Session Lifecycle

| Action | CLI | Desktop App | 
|---|---|---|
| Start new | `codex` or`/new` | New Thread button | 
| Resume | `codex resume` or`/resume` | Click thread in sidebar | 
| Fork | `/fork` | Right-click thread → Fork | 
| End | `/quit` or`Ctrl+C` | Close thread tab | 
| Delete | `/delete` in the session, or`codex delete <session>` by ID or name (v0.140.0+)<sup>107</sup> | Right-click → Delete | 

Sessions sync between CLI and desktop app — start in one, continue in the other.

## Non-Interactive Mode (codex exec)

`codex exec` runs Codex non-interactively for scripting, CI/CD, and automation.<sup>15</sup>

### Basic Usage

```
codex exec "summarize the repository structure"
codex exec --sandbox workspace-write "fix the CI failure"   # exec never prompts: a command the sandbox blocks fails, and the model sees the failure
codex exec --approve-for-me "fix the CI failure"             # workspace-write, with automatic review answering approval requests
codex exec --json "triage open bugs" -o result.txt
```
By default, `codex exec` writes progress/events to `stderr` and the final agent message to `stdout`. The design makes it composable with standard Unix pipelines.

Two defaults differ from the interactive TUI. `codex exec` runs in a read-only sandbox unless you choose another, so a task that edits files needs `--sandbox workspace-write` (or `sandbox_mode` in config). And it never stops to ask a person: it forces the approval policy to `never`, and neither `-c approval_policy=on-request` nor `approval_policy` in `config.toml` changes that. A command the sandbox blocks fails, and the model sees the failure. The exception is automatic review: with `--approve-for-me`, or `approvals_reviewer = "auto_review"` in config, exec keeps the configured policy and a reviewer answers each request.<sup>239</sup>

### Session Archiving (v0.136.0)

Sessions can be archived to keep the resume/fork list focused without deleting history. Archive from the TUI with `/archive`, or from the shell:<sup>103</sup>

```
codex archive <session-id>     # archive a session
codex unarchive <session-id>   # restore it
```
An archived session is protected from resume or fork operations until you unarchive it — a guardrail against accidentally continuing a session you meant to retire. Same release: `codex app-server --stdio` launches the app-server in stdio mode for editor/host integrations, and `/diff` is now blocked from executing repository-provided Git helpers (a command-safety fix). On Windows, an alpha provisioning path adds `codex sandbox setup --elevated` for administrators.<sup>103</sup>

### JSON Lines Output

With `--json`, `stdout` becomes a JSONL event stream:

```
codex exec --json "fix the tests" | jq
```
Event types: `thread.started`, `turn.started/completed/failed`, `item.started/updated/completed`, `error`<sup>246</sup>

```
{"type":"thread.started","thread_id":"019c5c94-..."}
{"type":"turn.started"}
{"type":"item.started","item":{"id":"item_1","type":"command_execution","status":"in_progress"}}
{"type":"item.completed","item":{"id":"item_3","type":"agent_message","text":"..."}}
{"type":"turn.completed","usage":{"input_tokens":24763,"cached_input_tokens":24448,"cache_write_input_tokens":0,"output_tokens":122,"reasoning_output_tokens":0}}
```
### Structured Output

Enforce response shape with JSON Schema:

```
codex exec "Extract project metadata" \
  --output-schema ./schema.json \
  -o ./project-metadata.json
```
`-o` / `--output-last-message` writes the final message to a file.

### Session Resume and Review

```
codex exec resume --last "continue where you left off"
codex exec resume <SESSION_ID> "fix the remaining issues"
codex exec review --base main           # Code review against a branch
```
### Key Flags

| Flag | Description | 
|---|---|
| `--sandbox workspace-write` | Workspace-scoped automation. `codex exec` has no`--ask-for-approval` flag and runs with approval policy`never` ;`-c approval_policy=...` does not change that, so a command the sandbox blocks fails instead of prompting<sup>239</sup> | 
| `--approve-for-me` | Automatic review answers approval requests, with the `workspace-write` sandbox and the`on-request` policy; cannot be combined with`--sandbox` or`--dangerously-bypass-approvals-and-sandbox`<sup>239</sup> | 
| `--thread-source <SOURCE>` | Classify newly created or forked threads: `user` (default),`subagent` ,`memory_consolidation` , or a feature name; global across`codex exec` subcommands, ignored when resuming;`threadSource` in the TypeScript SDK (v0.149.1)<sup>135</sup> | 
| `--json` | JSONL event stream to stdout | 
| `-o` ,`--output-last-message <file>` | Save final message to file | 
| `--output-schema <file>` | JSON Schema file describing the shape of the final response <sup>246</sup> | 
| `--ephemeral` | Don’t persist session files | 
| `-C` ,`--cd <dir>` | Set working directory | 
| `--add-dir <dir>` | Extra writable directories | 
| `--skip-git-repo-check` | Allow running outside git repos | 
| `--dangerously-bypass-approvals-and-sandbox` | No sandbox and no approvals; only for runs that an outer sandbox already contains, such as a disposable CI container <sup>239</sup> | 

### CI Authentication

`codex exec` supports `CODEX_API_KEY` for non-interactive authentication in automation environments.

**`codex exec` startup banner (v0.130.0).** The `codex exec` startup banner no longer prints the legacy “research preview” wording. If your CI scrapes startup output, the banner text is now slimmer; structured `--json` events are unchanged.<sup>91</sup>


### `codex remote-control` (v0.130.0+)

`codex remote-control` is an experimental top-level command that starts the app-server with remote control enabled, for managed remote-control clients and SSH remote workflows. The bare command uses the managed background server when eligible and falls back to the foreground; `--no-daemon` forces the foreground, and `start` and `stop` manage the daemon explicitly. It is not a replacement for `codex app-server --listen`: the docs say to keep using that when you are building a local protocol client.<sup>218</sup> The command arrived in v0.130.0 as a headless app-server entrypoint.<sup>91</sup> v0.133.0 gave it its current shape: it can run like a foreground command, wait for readiness, and report machine status, alongside the explicit `start` / `stop` commands.<sup>98</sup> **v0.143.0 adds `codex remote-control pair`**, which prints a short-lived manual pairing code from a running daemon so a controller can connect without the automatic discovery handshake, which is useful when the controlling process is on a different host or network segment than the daemon.<sup>111</sup> Add `--json` to any remote-control command for machine-readable output.

```
# Start remote control (managed background server when eligible, else foreground)
codex remote-control
codex remote-control --no-daemon   # Force the foreground
# Manage the daemon explicitly
codex remote-control start
codex remote-control stop
# Print a short-lived manual pairing code from a running daemon (v0.143.0+)
codex remote-control pair
# Same lifecycle as a TUI session: thread store, hooks, plugins, MCP, sandbox
# all initialize from your normal config.toml.
```
If you are building your own UI or local protocol client, use `codex app-server --listen` with the pagination APIs below, which let you enumerate large thread histories without hydrating every turn into memory at once.

### App-Server Thread Pagination (v0.130.0+)

App-server clients can now page large threads through three distinct turn item views:<sup>91</sup>

| View | Use Case | 
|---|---|
| **Unloaded** | Listing-only — thread structure without turn payload (cheapest) | 
| **Summary** | Compact per-turn metadata — useful for sidebars and resume pickers | 
| **Full** | Complete turn payload, including tool calls and outputs | 

Pair pagination with the `ThreadStore` interface introduced in v0.121.0 to walk long-running threads efficiently, especially in remote-control deployments where the orchestrator may be on a different machine than the rollout files.<sup>82</sup> **v0.143.0 widens what a controller can inspect:** app-server clients can now enumerate available environments, list a thread’s descendant threads, and fork history through a specific turn — the primitives a UI needs to branch a run from an earlier point without replaying it forward.<sup>111</sup> **v0.151.0 promotes the paginated thread history APIs out of experimental** and defaults durable threads with supporting stores to paginated history when `thread/start` omits `historyMode` (ephemeral threads and unsupporting stores stay legacy); full-history hydration is deprecated for paginated threads (#40676).<sup>145</sup>

### Live App-Server Config Refresh (v0.130.0+)

Live app-server threads now pick up `config.toml` changes without requiring a restart — edit the config, save, and the running thread reflects the new values on its next turn. This is the bug-fix counterpart to `codex remote-control`: a long-running headless server can be reconfigured in place rather than torn down.<sup>91</sup>

**Live turn-scoped settings (v0.151.0):** a running turn’s settings can now change mid-flight. The experimental `turn/settings/update` app-server method updates `model`, `effort`, `summary`, and `serviceTier` for subsequent steps of a specific live turn without changing future turns, reporting whether the update was applied, rejected, or lost its target; managed constraints are revalidated and the turn’s admitted approval and Guardian safety properties are preserved before an updated settings snapshot is published. `turn/start` also accepts `serviceTierForTurn`, letting one new turn override the service tier without touching the thread’s saved tier (“default” means standard speed; omitted or `null` inherits the thread tier).<sup>145</sup>

### Encrypted Remote Executors (v0.141.0)

As of v0.141.0 (stable, June 18, 2026), remote executors connect over **authenticated, end-to-end encrypted Noise-relay channels** — the control plane and the executor no longer have to trust the relay sitting between them, which matters whenever your orchestrator drives an app-server across a network boundary. The same release made **cross-platform remote execution preserve the executor’s native working directory and shell**, so a controller on one OS driving an executor on another no longer has paths or shell semantics rewritten underneath it. TLS also now accepts **P-521 certificate signatures** for enterprise-proxy compatibility.<sup>108</sup>

## Codex Cloud & Background Tasks

**GitLab support (beta, August 19, 2026):** Codex cloud now connects GitLab projects on all ChatGPT plans — link a project, create environments, start tasks from issues and merge requests, and request reviews; Self-Managed GitLab needs 19.0+.<sup>128</sup>

**Status**: the `codex cloud` CLI command is marked **experimental**; its flags and output may change between releases. The Codex Cloud docs carry no such label for the product itself.<sup>218</sup> OpenAI manages the cloud environments, and you do not control the infrastructure.


Codex Cloud runs tasks asynchronously in OpenAI-managed environments.<sup>4</sup> As of CLI v0.150.0, tasks are also addressable from a terminal conversation: reference other Codex tasks with `@` mentions, and ask agents to read, create, or message tasks.<sup>141</sup> See also GitHub Action & CI/CD for integrating Codex into your CI pipeline.

**Cloud task credentials hardened (v0.152.0):** cloud task requests can carry saved ChatGPT credentials, so their destination is now constrained before authentication is even loaded – `CODEX_CLOUD_TASKS_BASE_URL` is validated against trusted ChatGPT HTTPS origins on port 443 (URLs containing user information, queries, or fragments are rejected), and redirects are disabled for the cloud task backend and environment-discovery clients so credentials are never forwarded to a redirect destination.<sup>147</sup>


### How It Works

1. Submit a task (via chatgpt.com/codex, Slack integration, or CLI)
2. Codex clones your repo into an isolated cloud sandbox
3. The agent works independently: reads code, runs tests, makes changes
4. When done, Codex creates a PR or provides a diff for review
5. Apply results locally with `codex apply <TASK_ID>`

### Internet Access in Cloud

Agent internet is **off by default** and configured per environment:

- **Off** : No agent internet access (default)
- **On** : Optional domain allowlist + HTTP method restrictions

```
Allowed domains: pypi.org, npmjs.com, github.com
Allowed methods: GET, HEAD, OPTIONS
```
Setup scripts can still use internet to install dependencies, even when agent internet is off.


### Slack Integration

Mention `@ChatGPT` in a Slack channel to start a request; `@Codex` was the earlier mention, and the docs say to start new requests with `@ChatGPT` once an admin enables the ChatGPT app. If the workspace has Cloud delegation enabled, ChatGPT can send repository work to a separate Codex Cloud task and return the result to the thread.<sup>222</sup>

**Prerequisites:**
1. A ChatGPT workspace owner or admin sets up the workspace’s Slack deployment
2. The app is enabled in the channel (Slack Connect channels shared with another organization are not supported)
3. For repository work: Cloud delegation enabled, plus a published cloud environment shared with the workspace (personal environments are not used)
4. Your own account connected when ChatGPT asks, with access to the environment and its repositories

For coding work, name the repository in the request. Follow-ups go in the same thread, mentioning `@ChatGPT` again.

### Cloud CLI (experimental)

```
codex cloud exec --env <ENV_ID> "Fix failing tests"  # Start a cloud task
codex cloud status <TASK_ID>                          # Check task progress
codex cloud diff <TASK_ID>                            # View task diff
codex cloud list                                      # List recent tasks
codex cloud list --json                               # JSON output
codex cloud apply <TASK_ID>                           # Apply from cloud subcommand
codex apply <TASK_ID>                                 # Apply diff (top-level shortcut)
```
## The Codex Desktop App

The ChatGPT desktop app, where Codex runs next to Chat and Work (macOS and Windows; Linux in preview), provides a graphical interface optimized for multi-project management.<sup>16</sup><sup>216</sup> The Windows version launched on March 4, 2026 with native PowerShell support and native Windows sandbox.<sup>66</sup>

### Installation

```
codex app                      # Auto-downloads and installs on first run (macOS and Windows)
```
Or download directly: Codex.dmg (macOS) | Available in the Microsoft Store (Windows) | Linux (preview): follow the Linux installation guide for Ubuntu 24.04 and 26.04 LTS, Debian 13, Fedora 43 and 44, and Arch, on x64 and ARM64. Some features, Computer Use among them, are not in the Linux preview yet.<sup>216</sup>

### Key Features

| Feature | Description | 
|---|---|
| **Parallel threads** | Run multiple tasks across projects simultaneously | 
| **Thread modes** | Start threads in `Local` ,`Worktree` , or`Cloud` mode | 
| **Built-in Git tools** | Review diffs, add comments, stage/revert chunks, commit/push, create PRs | 
| **Integrated terminal** | Per-thread terminal ( `Ctrl` + backtick)<sup>255</sup> | 
| **Voice dictation** | Prompt by voice: select **Dictate** in the composer, then review the transcript before sending<sup>249</sup> | 
| **Scheduled tasks** (formerly Automations) | Schedule recurring tasks <sup>255</sup> | 
| **Notifications** | Completion/approval notifications when app is backgrounded | 
| **Prevent sleep** | Optional setting to keep machine awake while tasks run | 
| **Skills + MCP** | Shared configuration across app, CLI, and IDE extension | 
| **MCP shortcuts** | Quick-access MCP tool shortcuts in the composer (App v26.226) <sup>63</sup> | 
| **Review @mentions** | @mention collaborators in code review comments (App v26.226) <sup>63</sup> | 
| **Custom themes** | Color controls and font selection in Settings (App v26.312) <sup>72</sup> | 
| **App-server TUI** | Enabled by default (v0.117.0+): `!` shell commands, filesystem watch, remote WebSocket with bearer auth, prompt history recall across sessions<sup>75</sup> | 
| **Appshots** | Appshots (macOS and Windows) attach the frontmost app window to a thread, including a screenshot and available text, and can be added with the Appshots hotkey. <sup>99</sup><sup>255</sup> | 
| **Built-in browser comments** | The built-in browser can preview local/public pages and accept element or area comments for precise rendered-page feedback. <sup>99</sup> | 
| **Computer Use + locked use** | Computer Use lets Codex operate allowed apps on macOS and Windows for scoped GUI tasks; locked use is macOS-only, opt-in, and limited to trusted active Computer Use turns after the Mac locks. <sup>99</sup><sup>255</sup> | 

### Appshots, Browser Comments, and Computer Use

The May 21 app updates make the desktop app a stronger context surface rather than just a thread manager. Use **Appshots** when Codex needs the state of another app (macOS or Windows) before it can act: Codex captures the frontmost window, available visible/off-screen text exposed by the app, and stores the attachment locally in the session history.<sup>99</sup><sup>255</sup>

For web and frontend work, use the **built-in browser** first: it gives Codex and you a shared rendered preview, supports browser-use actions such as clicking, screenshots, asset downloads, and read-only inspection JavaScript, and lets you mark page regions with comments that Codex can address in the next turn.<sup>99</sup> It keeps its own profile, and you can sign in there when a task needs an account; to work in a browser where you are already signed in, use the browser extension (Chrome, Edge, Brave, Opera, or Vivaldi; Opera has no side chat).<sup>255</sup> **Site tools (WebMCP; launched August 25, 2026)**: in the desktop app’s built-in browser, ChatGPT Work and Codex can use tools a website provides to work with the page. The docs say site tools “require a supported model and a page that provides tools” and that “Your account or your managed workspace’s browser policy can grant access”; on eligible managed plans an administrator controls that path with `browser_use.allow_webmcp` in `requirements.toml`. At launch the changelog limited them to GPT-5.6 Sol and GPT-5.6 Terra and excluded Enterprise and Edu workspaces.<sup>136</sup><sup>252</sup>

Use **Computer Use** only when a structured integration or browser preview cannot verify the task. It can inspect and operate allowed apps on macOS and Windows, but it inherits Codex approvals and sandbox rules for file edits and shell commands, and locked use, which is macOS-only, stays narrow: Codex can temporarily access allowed apps during active trusted Computer Use turns after the Mac locks, with relocking safeguards and local-input detection.<sup>99</sup><sup>255</sup>

### Thread Modes

Each thread runs in one of three modes, selected when you create it:

| Mode | Isolation | File Access | Best For | 
|---|---|---|---|
| **Local** | None — works directly in your project directory | Full read/write | Quick tasks, exploration, non-destructive work | 
| **Worktree** | Git worktree — isolated branch copy of your repo | Isolated copy | Feature development, risky refactors, parallel experiments | 
| **Cloud** | Remote server — runs on OpenAI infrastructure | No local access | Long-running tasks, CI-like workflows, async delegation | 

**Worktree isolation mechanics:**

When you start a Worktree thread, the desktop app:
1. Creates a new git worktree (`git worktree add`) in a temporary directory
2. Checks out a fresh branch from your current HEAD
3. Runs the agent inside the worktree — all file changes are isolated
4. Presents a diff review when done — you choose which changes to merge back

This means multiple Worktree threads can run simultaneously on the same repo without conflicts. Each gets its own branch and working directory.

**Worktree settings and thread ownership (v0.151.0):** managed worktree behavior is resolved from the existing `[desktop]` configuration by a dedicated settings parser – the worktree root defaults to `$CODEX_HOME/worktrees`, automatic cleanup defaults to enabled, and 15 worktrees are retained, with configured roots, cleanup flags, and retention counts validated before use. Each managed linked worktree can also carry a versioned `codex-thread.json` ownership record in its Git metadata binding it to a thread: writes are atomic and no-clobber (repeat bindings are idempotent, conflicting owners rejected), and primary, nested, and unmanaged checkouts are rejected before ownership metadata is touched.<sup>145</sup>

**Managed worktrees reach the CLI in v0.154.0 (experimental):** `--worktree` at launch or `/worktree` in the TUI creates an isolated checkout for a new or forked session, `codex exec` supports managed worktrees for automation, and TUI session commands plus a worktree browser let you list, browse, and resume worktree sessions (#42196, #42366, #42652, #43069, #43120, #43286). **v0.155.0 adds the management adjacents while worktrees stay non-default:** the agents overview can hide, archive, and delete worktree sessions, ownership details are surfaced, and deleting a clean managed worktree asks for confirmation (#43942, #44424, #44433). The default-on flip (#44870) merged September 11 but was not in stable 0.155.0.<sup>161</sup><sup>165</sup>

**Managed worktrees become the default in v0.156.0:** #44870 promotes `worktrees` to a stable feature, enables it by default, and removes it from `/experimental`. Shipping alongside it: worktree sessions can attach to an **existing local daemon** instead of starting their own (#46498), and worktree sessions can be created directly from the **agents overview** (#45276), which also gains status filter tabs (#46839). If you have scripts or configs written on the assumption that worktrees are off unless requested, 0.156.0 is the release that changes their behavior. **The opt-out is gone:** from v0.156.0 through v0.163.0, `worktrees = false` under `[features]` (per run: `--disable worktrees`) turned the feature off; v0.164.0 removed the flag with the feature left on, and the key is now ignored.<sup>197</sup><sup>201</sup> **Older local daemons:** if worktree creation or `/cd` is blocked with “The local Codex service cannot check background terminals,” run `codex app-server daemon update` and restart Codex.<sup>170</sup><sup>172</sup>

**Worktree tools for the model (v0.162.0):** in a trusted local project with the feature on, the model itself can create and list managed worktrees through new tools (#50148), and the new-conversation and fork prompts explain the worktree choices (#50416).<sup>188</sup>

### Automations

Automations, which the docs now call scheduled tasks, run **locally in the app**, so the app must be running and the project available on disk:<sup>255</sup>

- In Git repos, each task runs either in your local project or on a dedicated background worktree (isolated from your working directory)
- In non-Git projects, runs execute directly in the project directory
- Automations use your default sandbox settings

**Setting up an automation:**
1. Open a project in the desktop app
2. Ask Codex in a chat to create the scheduled task; **Scheduled** in the sidebar lists tasks and their runs
3. Set the schedule; event triggers (Gmail, Slack, GitHub) exist only in ChatGPT on the web and mobile
4. Write the prompt and select execution mode (local or worktree)
5. Set the reasoning level for the automation run (App v26.312)<sup>72</sup>
6. Automations run on schedule and report results in **Scheduled**

**Example use cases:**

- **Issue triage** : Automatically categorize and prioritize new issues
- **CI monitoring** : Watch build failures and suggest fixes
- **Alert response** : React to monitoring alerts with diagnostic analysis
- **Dependency updates** : Check for and apply security patches

Runs with findings appear in **Scheduled**, which works as an inbox with an unread indicator.

### Windows Support

The Codex Desktop App launched on Windows on March 4, 2026 (App v26.304) with native PowerShell support, a native Windows sandbox, and the same core features as the rest of the app (skills, automations, and worktrees), without requiring WSL.<sup>66</sup>

## GitHub Action & CI/CD

The official GitHub Action integrates Codex into your CI/CD pipeline.<sup>18</sup>

### Basic Usage

```
# .github/workflows/codex.yml
name: Codex
on:
  pull_request:
    types: [opened]
jobs:
  codex:
    runs-on: ubuntu-latest
    outputs:
      final_message: ${{ steps.run_codex.outputs.final-message }}
    steps:
      - uses: actions/checkout@v5
      - name: Run Codex
        id: run_codex
        uses: openai/codex-action@v1
        with:
          openai-api-key: ${{ secrets.OPENAI_API_KEY }}
          prompt-file: .github/codex/prompts/review.md
          sandbox: workspace-write
          safety-strategy: drop-sudo
```
### Configuration Options

| Input | Purpose | 
|---|---|
| `openai-api-key` | API key for proxy/auth setup | 
| `responses-api-endpoint` | Override endpoint (e.g. Azure Responses URL) | 
| `prompt` /`prompt-file` | Task instructions (one required) | 
| `working-directory` | Directory passed to `codex exec --cd` | 
| `sandbox` | Legacy sandbox mode: `workspace-write` /`read-only` /`danger-full-access` | 
| `permission-profile` | Permission profile for the run, such as `:read-only` ,`:workspace` , or a named profile from`codex-home/config.toml` ; the action’s own description prefers it over`sandbox` for new workflows | 
| `codex-args` | Extra CLI flags (JSON array or shell string) | 
| `output-schema` /`output-schema-file` | Structured output schema for `--output-schema` | 
| `model` /`effort` | Agent configuration | 
| `output-file` | Save final message to disk | 
| `codex-version` | Pin CLI version | 
| `codex-home` | Custom Codex home directory | 
| `allow-users` /`allow-bots` /`allow-bot-users` | Trigger allowlist controls | 
| `safety-strategy` /`codex-user` | Privilege-reduction behavior and user selection | 

**Output**: `final-message`, the final Codex response text for downstream steps/jobs.

### Safety Strategies

| Strategy | Description | 
|---|---|
| `drop-sudo` (default) | Linux and macOS runners only; drops sudo privileges from the default user before Codex runs, and cannot be undone for the rest of the job (or afterward on a reused self-hosted runner) <sup>251</sup> | 
| `unprivileged-user` | Run Codex as a pre-created low-privilege user | 
| `read-only` | Read-only sandbox (runner/user privilege risk still applies) | 
| `unsafe` | No privilege reduction; required on Windows runners | 

### Access Controls

```
with:
  allow-users: "admin,maintainer"     # Limit who can trigger
  allow-bots: false                   # Block bot-triggered runs
```
Default: Only collaborators with write access can trigger Codex workflows.

## Codex SDK

The Codex SDK embeds Codex’s agent capabilities into custom applications. It ships for TypeScript and for Python.<sup>19</sup><sup>225</sup>

### Installation

```
npm install @openai/codex-sdk     # TypeScript (server-side; Node.js 18 or later)
pip install openai-codex          # Python 3.10 or later; controls the local Codex app-server over JSON-RPC
```
The Python SDK is a stable release, and its published builds pin a Codex CLI runtime, so its version numbers track the CLI (0.167.1 on PyPI on November 11, 2026). The examples below use the TypeScript SDK.

### Basic Usage

```
import { Codex } from "@openai/codex-sdk";
const codex = new Codex();
const thread = codex.startThread();
// Multi-turn conversation
const turn1 = await thread.run("Diagnose CI failures and propose a fix");
console.log(turn1.finalResponse);
const turn2 = await thread.run("Implement the fix and add tests");
console.log(turn2.items);
// Resume a previous session
const resumed = codex.resumeThread("<thread-id>");
await resumed.run("Continue from previous work");
```
### Advanced SDK Features

- **`runStreamed(...)`** : Async event stream for intermediate updates
- **Python SDK auth (v0.132.0+)** : API-key login, ChatGPT browser/device-code flows, account inspection, and logout are first-class SDK paths.<sup>97</sup>
- **Text-only turn convenience (v0.132.0+)** : Python turn APIs accept plain strings and return richer`TurnResult` metadata with collected items, timing, and usage.<sup>97</sup>
- **`outputSchema`** : Enforce JSON-shaped final output
- **Multimodal input** : Pass text + local images (`{ type: "local_image", path: "..." }` )
- **Image workflows (v0.117.0)** :`view_image` returns URLs, generated images are reopenable, and image history survives session resume<sup>75</sup>
- **Multi-environment `view_image` (v0.130.0)** : For sessions that span multiple environments (introduced in v0.124.0 with per-turn environment + working-directory selection, refined in v0.125.0 with sticky environments),`view_image` now resolves file paths through the**selected environment** rather than the orchestrator’s local filesystem. An image attached from a remote environment is fetched relative to that environment’s working directory, not the host running the SDK.<sup>91</sup>

### Thread and Client Configuration

```
// Custom working directory, skip git check
const thread = codex.startThread({
  workingDirectory: "/path/to/project",
  skipGitRepoCheck: true,
});
// Custom environment and config overrides
const codex = new Codex({
  env: { CODEX_API_KEY: process.env.MY_KEY },
  config: { model: "gpt-6.1-sol" },
});
```
Sessions persist under `~/.codex/sessions`.

**Runtime**: Node.js 18+.

## Performance Optimization

### Context Management

Context windows vary by model. In the catalog bundled with v0.167.1, GPT-6.1 Sol, GPT-6 Astra, GPT-6 Sol, GPT-6 Luna, and the GPT-5.6 family run a **272K** window and GPT-6.1 Terra **500K**; OpenAI’s models docs give GPT-6.1 Sol a **500K** default in Codex and ChatGPT Work. The first number is the `context_window` field in the catalog file that ships inside the CLI and the second is the documented default, so check `~/.codex/models_cache.json` for the value your account was served. GPT-5.5 (retired from Codex for ChatGPT sign-in on October 14, 2026) and GPT-5.4 / GPT-5.4-mini (retired August 31, 2026) are API-key options now. All of them fill faster than you’d expect, so manage proactively:<sup>202</sup><sup>205</sup>

1. **Use `/compact` regularly** : Summarizes conversation history to free tokens
2. **Provide local docs** : High-quality`AGENTS.md` and local documentation reduce exploration overhead (which consumes context)
3. **Use `@` to attach specific files** : Reference files directly instead of asking Codex to find them
4. **Keep prompts focused** : Scoped prompts with exact files consume less context than open-ended exploration

**Experimental context management (v0.153.0, disabled by default):** `features.context_management.experimental_mode` activates a coordinated context stack – token-budget context, history notes, and a `new_context` tool – for eligible ChatGPT Plus, Pro, and Pro Lite sessions using the Codex backend. API-key sessions, custom providers, non-Codex endpoints, and temporary structured threads are excluded, and the flag is explicitly under development: treat it as a preview of where context handling is heading, not something to build workflows on.<sup>149</sup> **v0.154.0 adds a capability gate**: eligibility is resolved against the active model’s capabilities at session startup (#43147) – the same surface the Codex models docs tied to Astra by name for Plus and Pro sign-in in September 2026 (see the cross-context note under Which Model Should I Choose?).<sup>155</sup><sup>161</sup> As of v0.157.0 no model in the bundled catalog is marked as supporting it (#47397), so with that catalog the setting has no effect.<sup>175</sup> GPT-6.1 Sol, added in v0.159.1, is not marked either, and the config reference now describes the key as “not currently available.”<sup>179</sup><sup>233</sup>

### Token Efficiency

| Technique | Impact | 
|---|---|
| Leave `model_reasoning_summary` unset (`none` for every bundled model) | No reasoning summaries are requested <sup>253</sup> | 
| Use `model_verbosity = "low"` | Shorter explanations, more action | 
| Use lighter tiers ( `gpt-6-luna` ) for simple tasks | Significantly cheaper per message | 
| Split complex tasks into focused sessions | Better per-session token efficiency | 
| Use profiles to switch settings by task | Avoid paying for high reasoning on routine work | 

### Speed Optimization

1. **Fast mode** (`/fast` , or`service_tier = "fast"` ): faster responses on supported GPT-6 models at 2.5x included usage (2x for purchased credits); Ultrafast for GPT-6.1 Sol and GPT-6 Astra on Pro $500 and eligible Enterprise and Edu plans<sup>204</sup>
2. **`--profile fast`** : Pre-configured`gpt-6-luna` with low reasoning
3. **Parallel tool execution** : Codex runs independent reads/checks concurrently, so structure prompts to enable this
4. **Outcome-driven loops** : Ask for “implement, test, fix, stop when green” instead of step-by-step instructions

## How Do I Debug Issues?

### Common Problems and Solutions

| Problem | Cause | Solution | 
|---|---|---|
| “Re-connecting” loop | Multiple Codex instances | Kill all processes, wait 60s, restart single instance | 
| 401 auth errors | Stale credentials | `codex logout && codex login` (clears whichever store`cli_auth_credentials_store` selects) | 
| Startup fails on 0.162.0 with a feature-settings mismatch while a background server runs | The CLI compared the running server’s feature settings with its own defaults | Upgrade to 0.162.1, where the check covers only explicit command-line feature overrides; or launch with `--no-daemon` | 
| Network blocked in sandbox | Default behavior | `-c 'sandbox_workspace_write.network_access=true'` | 
| WSL2 disconnections | WSL state corruption | `wsl --shutdown` in PowerShell, wait 1 min, restart | 
| Patch failures | Line ending mismatches | Normalize to LF, provide exact file text | 
| Context compacting fails | Too much context | Reduce reasoning effort, split into smaller tasks | 
| Model changes unexpectedly | Config.toml override | Run `/debug-config` to inspect config layers and requirement sources | 
| Plan mode makes a change | Reported in early 2026; the issue was closed as not planned on February 8, 2026 | When you need a guarantee that nothing is written, run with the `read-only` sandbox instead of relying on plan mode; history in Issue #11115 | 
| Forgetting AGENTS.md instructions | Context limits | Keep instructions concise; use skill files for detailed procedures | 
| Stalling in Read Only mode | Historical report from 2025 (answered; no activity since December 2025) | Discussion #7380 | 

### Error Symptoms Reference

Codex’s exact error wording changes between versions, so this table describes symptoms instead of quoting messages. The last three rows are literal messages.

| Symptom | Meaning | Fix | 
|---|---|---|
| Permission denied or `Operation not permitted` on a file write | The sandbox blocked a file operation | Check the sandbox mode or permission profile; use `workspace-write` (`:workspace` ) if Codex needs to edit files | 
| A rate-limit error from the API | API or plan rate limit hit | Wait and retry; reduce `model_reasoning_effort` or switch to a lighter model | 
| Context window exceeded | The conversation exceeded the model’s context window (the size differs by model) | Use `/compact` to summarize, or start a new session with`/new` | 
| An MCP server fails to start or times out | The server process crashed or took too long | Check `codex mcp get <name>` for config; increase`startup_timeout_sec` | 
| Not signed in, or authentication required | No valid session or API key | Run `codex login` ; for`codex exec` , set`CODEX_API_KEY` | 
| A command fails only inside the sandbox | The command needs something the sandbox denies | Re-run it with `codex sandbox --permission-profile <profile> -- <command>` to see the failure in isolation (`--log-denials` on macOS lists what was blocked) | 
| A skill is not listed | The skill is not at the expected path | Check the `/skills` list; verify the skill folder location | 
| `wire_api = "chat"` is no longer supported | A provider entry still sets the removed Chat Completions wire API | Set `wire_api = "responses"` (or delete the key; it is the default) and point the provider at a Responses-compatible endpoint (see Custom Model Providers)<sup>207</sup> | 
| legacy `profile = "<name>"` config is no longer supported | `config.toml` still has the top-level`profile` selector that 0.134.0 retired | Delete the `profile = ...` line, keep that preset’s settings in`~/.codex/<name>.config.toml` , and start Codex with`--profile <name>` (see Profiles)<sup>238</sup> | 
| `approval_policy = "untrusted"` is no longer supported; remove this setting | A config file or profile file still names the approval policy retired in v0.149.0 | Delete the line, or set `approval_policy = "on-request"`<sup>243</sup> | 

### What Does `codex doctor` Do?

`codex doctor` is the single command to run before filing a bug or debugging a broken
install. It captures the local runtime, configuration, auth, terminal, network, and state
in one report, and the project’s own issue template asks you to paste its output.<sup>120</sup>

```
codex doctor                           # Full human-readable report (the default)
codex doctor --network-only            # Only configuration, connectivity, and WebSocket diagnostics (v0.166.0+)
codex doctor --summary                 # Compact view
codex doctor --json                    # Redacted structured JSON, for bug reports
codex doctor --all                     # Expand truncated lists
codex doctor --no-color                # Strip ANSI color
```
The report is organized into stable sections, so you can skim to the one you need:<sup>120</sup>

| Section | What it reports | 
|---|---|
| **Notes** | Anomalies promoted to the top: available updates, oversized rollout directories, optional MCP problems, mixed auth signals | 
| **Environment** | Runtime provenance, install consistency, search-tool readiness, terminal/multiplexer metadata, database health | 
| **Configuration** | `config.toml` load status, auth details, MCP server config, sandbox policy, feature flags | 
| **Updates** | Whether update configuration is locally consistent | 
| **Connectivity** | Network environment, WebSocket diagnostics, provider-aware endpoint reachability | 
| **Background Server** | App-server daemon status | 

Each check carries a status marker – `✓` ok, `✗` fail, `⚠` warn, `○` idle – with a summary
line. `--json` keys every check by a stable identifier, which makes it usable in CI or a
support script rather than only by eye.<sup>120</sup>

The command has grown across releases: it landed in v0.131.0, gained environment, Git,
terminal, app-server, and thread-inventory reporting in v0.135.0, and added editor and pager
details (with sensitive values redacted in JSON) in v0.139.0.<sup>96</sup><sup>102</sup><sup>106</sup>

**Use it first, not last.** Most “Codex is broken” reports resolve to something `doctor`
already names: a stale auth mode, a `config.toml` that failed to parse, or a second install
shadowing the one on your `PATH`.


### Diagnostic Tools

```
codex doctor                           # Full local diagnostic (start here)
codex --version                        # Check CLI version
codex login status                     # Verify authentication
codex mcp list                         # Check MCP server status
codex debug app-server --help          # Debug app server issues
```
In-session TUI diagnostics:

```
/status                                # Token/session overview
/debug-config                          # Show config layers and requirement sources
/compact                               # Summarize history to reclaim context
```
**Note**: `codex --verbose` is **not** a valid top-level flag. Use the debug subcommands and TUI diagnostics above.


### Clean Reinstall

```
npm uninstall -g @openai/codex && npm install -g @openai/codex@latest
```
Preserves `~/.codex/` (auth, config, sessions). For pinning to an exact version, downgrading
after a bad release, or a full state wipe, see
Update, Downgrade, or Uninstall.

### Debug Mode

```
codex debug app-server send-message-v2 "hello"  # Send one message through the app-server test client; the message is required
```
The message argument is required.<sup>253</sup>

### Reporting Issues

```
/feedback                              # Send logs to Codex maintainers (in TUI)
```
Or file issues at github.com/openai/codex/issues.<sup>1</sup>

## Codex Security

Codex Security entered research preview on March 6, 2026, bringing context-aware application security review into the Codex stack.<sup>77</sup> It now ships four ways: Daybreak Defense Factory (formerly Codex Security Cloud; a research preview on the web and in the desktop app, for connected GitHub repositories), the Codex Security plugin (local scans in the desktop app, also usable from Codex CLI), a standalone CLI for repeatable terminal scans and CI checks, and an SDK. The CLI and SDK are in the public `@openai/codex-security` package:<sup>221</sup>

```
npm install -g @openai/codex-security   # Node.js 22.13.0 or later in the 22 line, or Node.js 24 or 26; also needs Python 3.10 or later
```
Scans that use OpenAI inference require Codex Security access, and requirements differ by route, so check the quickstart for the one you pick.

**How it works**: Codex Security analyzes repositories to build a project-specific threat model, identifies vulnerabilities classified by real-world impact, and pressure-tests findings in a sandboxed environment to validate them. The agent surfaces higher-confidence findings with fixes, reducing noise from insignificant bugs.

**Performance**: During research preview, Codex Security scanned 1.2 million commits and identified 10,561 high-severity vulnerabilities. Precision improved over time — cutting noise by 84%, reducing over-reported severity by 90%+, and halving false positive rates. The system has found real vulnerabilities in OpenSSH, GnuTLS, and Chromium, with 14 CVEs assigned.<sup>77</sup>

**Note**: Codex Security is separate from the CLI’s built-in sandbox security model. The sandbox protects your machine from Codex; Codex Security protects your codebase from vulnerabilities.


## Enterprise Deployment

### Admin Controls (requirements.toml)

Administrators enforce enterprise policy via **requirements.toml**, an admin-enforced configuration file that constrains security-sensitive settings users cannot override:<sup>21</sup><sup>219</sup>

**v0.146.0 adds enterprise-plan recognition and administrator controls over in-app updates.**<sup>121</sup> The documented key is `in_app_updates = false` under `[features]` in `requirements.toml`. It disables the ChatGPT desktop app’s own updater on restart, where supported, so the admin’s package deployment decides when a version rolls out; it does not affect external package deployment.<sup>219</sup>


```
# /etc/codex/requirements.toml
# Restrict which approval policies users can select. Add "untrusted" to keep the
# stricter approvals Codex derives for projects marked trust_level = "untrusted";
# approval_policy = "untrusted" itself was retired in v0.149.0.
allowed_approval_policies = ["on-request", "never"]
# Limit available sandbox modes
allowed_sandbox_modes = ["read-only", "workspace-write"]
# Control web search capabilities
allowed_web_search_modes = ["cached"]
# Allowlist MCP servers by identity (both name and identity must match).
# This form pins the executable and every argument: command = "npx", args = ["approved-mcp-server"].
# The string form, identity = { command = "npx" }, compares the command only.
[mcp_servers.approved-server.identity]
command = { executable = "npx", args = [{ match = "exact", value = "approved-mcp-server" }] }
# Admin-enforced command restrictions
[[rules.prefix_rules]]
pattern = [{ token = "rm" }, { any_of = ["-rf", "-fr"] }]
decision = "forbidden"
justification = "Recursive force-delete is prohibited by IT policy"
[[rules.prefix_rules]]
pattern = [{ token = "sudo" }]
decision = "prompt"
justification = "Elevated commands require explicit approval"
```
Unlike user-level `config.toml` which sets preferences, `requirements.toml` is a hard constraint layer that restricts what values users can select, and users cannot override it. Admin requirements rules can only **prompt** or **forbid** (never silently allow).
