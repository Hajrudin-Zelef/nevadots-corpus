---
id: collect-261121-veille-ia/dev-langages/this-week-in-claude-code-codex-and-gemini-cli-week-of-november-15-2026
title: "This week in Claude Code, Codex and Gemini CLI (week of November 15, 2026)"
domain: dev-langages
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["claude", "code", "week", "mode", "fixes", "four", "permission", "codex"]
source: https://dev.to/aicoding-guide/this-week-in-claude-code-codex-and-gemini-cli-week-of-november-15-2026-4pmm
source_anchor: ""
source_lines: [1, 110]
section: "Gemini_Google"
---
*Originally published at https://aicoding-guide.com.*

The biggest change this week is **four permission and sandbox fixes in Claude Code v2.1.321**. All four are cases where a prompt that should have appeared did not, so your behaviour changes without you touching any setting. There is no reason to defer this update.

On the feature side, v2.1.323 adds **Code Mode**, an experimental opt-in where Claude writes JavaScript to call its tools. Codex shipped four releases, 0.167.1 through 0.169.0.

**Key point**

This week in brief


- Claude Code v2.1.321 fixes four permission and sandbox gaps; v2.1.325 fixes a security issue in zstd decoding
- v2.1.323 adds Code Mode as an experimental opt-in, plus
`engines` version-range gating in `plugin.json`
- Codex v0.169.0 changes how
`codex exec` decides trust: `--sandbox workspace-write` alone no longer enables project config or MCP

Four releases: v2.1.321, v2.1.322, v2.1.323 and v2.1.325. **There is no v2.1.320 and no v2.1.324** — the CHANGELOG has no entry for either.

Four permission and sandbox fixes, quoted from the CHANGELOG:

- "Fixed a case where a read-only Bash command ran outside the sandbox without a permission prompt"
- "Fixed a case where Bash permission checks judged a file write from the wrong folder and didn't prompt"
- "Fixed deny rules and protected-file prompts not applying to the file that a `cp` or`mv` into a directory writes"
- "Fixed MCP OAuth requests sending secrets such as refresh tokens to another host when a server redirected them"

The third is the easiest to miss in practice. When you name a directory as the destination, as in `cp src/x.env dist/`, the file actually written (`dist/x.env`) was not checked against deny rules or the protected-files prompt. If you rely on deny rules to guard paths, that route was open until this fix. For how to write those rules, see Claude Code permissions in settings.json and settings.local.json.

A model landed too: "Added Claude Fable 5.2 (`claude-fable-5-2`), now the default Fable model — 1M context, $8/$40 per Mtok with $0.20/Mtok cache reads" (confirmed in the CHANGELOG on 2026-11-15).

There is also a `/config` display fix. Where new sessions start in auto mode, the default permission mode was shown as Manual; and pressing Space on the "Auto (default)" row now saves Don't Ask rather than Plan. For how the modes differ, see Claude Code starts in auto mode by default.

A single bullet: "Added fast mode on Vertex in the global, us and eu regions, falling back to standard speed if Vertex refuses it".

**Code Mode** arrives: "Added Code Mode, an experimental opt-in in which Claude writes JavaScript to call its tools", with `https://code.claude.com/docs/en/code-mode` covering how to turn it on, or off for an organization. Being an opt-in, it does nothing by default.

Two plugin changes matter. `plugin.json` gains `engines["claude-code"]`: a plugin outside its stated version range is not installed, updated or loaded, and install and update pick the newest version that fits. And `claude plugin validate` now type-checks a plugin's TypeScript hooks module — each finding is a warning, and `--strict` fails on it.

There is a new warning about rule syntax: "Added a start-up warning for Bash allow rules with a `*` in a variable's value, such as `Bash(PORT=* npm run dev)`".

Worth noting among the fixes: running `/compact` again, or after a command such as `/context`, no longer summarizes away a message Claude had not answered yet.

One security fix leads the entry: "Fixed a security issue in how zstd-compressed responses are decoded".

Two fixes help long sessions. One is "Fixed sessions on many third-party gateways and Foundry stopping with a context length error on every turn instead of compacting". The other restores recent turns that could go missing from a resumed session after Claude Code stopped without a clean exit.

Subagents get two fixes: one where a subagent with its own model was moved to the plan-mode model when the model setting is `opusplan` or `haiku`, and one where a subagent on a different model sized file reads and MCP results using the main session's model.

Four releases: 0.167.1, 0.167.2, 0.168.0 and 0.169.0. **0.167.1 shipped with no release notes at all** — the page carries only a compare link.

One item. On Google Cloud Vertex AI, Codex now sends tool updates incrementally by default. To opt out, set `incremental_tools_legacy_gcp = false` under `[features]` in `config.toml`.

Three new configuration keys.

| Key | What it does | 
|---|---|
| `features.tui_workspace` | View a side conversation beside the main chat. Drag the divider to resize the panes, or press `leader z` to zoom the focused pane | 
| `features.image_generation_animation` | Generate or edit animated GIFs, with optional frame count and timing controls | 
| `tui.notification_method = "osc777"` | Terminal notifications. Approval notifications show longer command previews and distinguish terminal input from network access requests | 

One deprecation notice: "Codex warns when `features.multi_agent` is explicitly configured and points to `agents.enabled` for disabling subagent tools."

One behavioural change is the thing to know here.

**How codex exec decides trust has changed**

"`codex exec` keeps project configuration and MCP servers disabled in untrusted directories, even with `--sandbox workspace-write`. Use `--trust` with `codex exec` or `codex review` to explicitly save project trust."

In an untrusted directory, `--sandbox workspace-write` is no longer enough to bring project config and MCP servers in. If a CI job using `codex exec` has stopped loading your MCP servers, pass `--trust`. For how trust levels work, see Stop Codex asking whether to trust this folder with trust_level in config.toml.


Among the new features, the opt-in terminal workspace now supports multiple side-chat tabs, switchable by keyboard or mouse, and saved local side chats reopen after a Codex restart. For getting diagnostics out of a locked-down environment: "Press `Ctrl+S` in the terminal report editor to save a diagnostic ZIP locally, even when uploads are disabled."

One release, v0.68.0. It ships **with no summary prose at all** — just a list of pull requests. One feature, the rest fixes.

The ones touching sandboxing and path handling:

- 
`fix(sandbox): supply --userns=keep-id for rootless podman` (#30118) — sandboxed execution under rootless podman
- 
`fix(core): make isWithinRoot case-insensitive on Windows` (#29999) — root checks no longer depend on case
- 
`fix(core): resolve symlinked paths canonically in GlobTool` (#30042) — symlinked paths resolved canonically
- 
`fix(core): honor .git/info/exclude and core.excludesFile in shadow repo` (#30034) — both exclude sources respected
- 
`fix(hooks): support SubagentStop event mapping and merge hook definitions` (#30111) — hooks gain`SubagentStop`

The one feature is `feat(cli): add extension validation command and evaluation tests` (#29980), which adds a command for validating extensions. Rate-limit feedback improved too: a 429 cool-down and countdown now appear in the loading indicator (#30012).

1. 
**Update Claude Code to v2.1.321 or later.** Four permission gaps and a security fix. No configuration changes needed
2. 
**Re-examine the paths your deny rules guard.** If anything in your workflow uses`cp` or`mv` with a directory destination, that route was unchecked before v2.1.321
3. 
**Look for allow rules shaped like `Bash(PORT=* npm run dev)`.** From v2.1.323 these produce a start-up warning
4. 
**If CI uses `codex exec`, check whether you now need `--trust`.** From v0.169.0, MCP servers stay disabled in untrusted directories
5. 
**If Codex on Vertex AI misbehaves, try `incremental_tools_legacy_gcp = false`.** Incremental sending became the default in v0.167.2

- Nine releases this week: four for Claude Code, four for Codex, one for Gemini CLI
- Top priority is Claude Code v2.1.321's four permission and sandbox fixes, plus the zstd decoding fix in v2.1.325
- v2.1.323 adds Code Mode as an experimental opt-in, and plugins are now gated by `engines["claude-code"]`
- Codex v0.169.0 changes `codex exec` trust handling, so`--sandbox workspace-write` alone no longer enables project config or MCP
- Gemini CLI v0.68.0 is mostly sandbox and path-resolution fixes, with one new extension validation command

## Top comments (1)

Some comments may only be visible to logged-in visitors. Sign in to view all comments.
