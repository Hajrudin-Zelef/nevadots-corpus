---
id: collect-261121-veille-ia/gpt-openai/codex-cli-v0-161-and-v0-162-changes-and-gpt-6-1-sol-becoming-the-default-model
title: "Codex CLI v0.161 and v0.162 changes, and GPT-6.1 Sol becoming the default model"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["daybreak", "model", "codex", "default", "gpt-6", "features", "changes", "configuration"]
source: https://dev.to/aicoding-guide/codex-cli-v0161-and-v0162-changes-and-gpt-61-sol-becoming-the-default-model-502m
source_anchor: ""
source_lines: [1, 91]
section: "GPT_OpenAI"
---
*Originally published at https://aicoding-guide.com.*

Codex CLI v0.161.0 and v0.162.0 carry two changes that reach into your configuration: **the default model was swapped** and **the way Daybreak is enabled changed**.

Two things are worth checking because of them: whether you pin a model in `config.toml`, and whether you have added the `cli_daybreak` opt-in if you use Daybreak.

Setting keys below are only the ones confirmed against the official configuration reference as fetched on 2026-10-12; anything unconfirmed is marked as such.

**Key point**

What you will learn


- Where GPT-6.1 Sol became the default, and how to pin a model yourself
- That Daybreak now requires an explicit opt-in
- The approval and sandbox changes, and where the worktree tools stand

The v0.161.0 release notes read "GPT-6.1 Sol is now the default model in the bundled and Amazon Bedrock catalogs." What is stated as default is **the bundled catalog and the Amazon Bedrock catalog**; other setups are not addressed.

The `model` row in the configuration reference now uses `gpt-6.1-sol` as its example:

| Key | Type | Description | 
|---|---|---|
| `model` | `string` | Model to use (e.g., `gpt-6.1-sol` ) | 

To pin it instead of taking the default, put it at the top level of `~/.codex/config.toml`:


```
model = "gpt-6.1-sol"
```
**Check availability on your account first**

The reference says to "Set `model` to one available to your signed-in account or workspace," and tells you to check GPT-6.1 Sol availability before using that example value. If your organization's workspace doesn't offer it, writing it into `model` won't make it work.


Amazon Bedrock also gained multi-agent V2 and Ultra reasoning support. For setting reasoning depth, see Change the reasoning effort in Codex.

This is the change most likely to break an existing setup. The release notes read: "Daybreak is opt-in through `--enable cli_daybreak` or `features.cli_daybreak=true`; `daybreak=true` alone is insufficient."


```
# v0.161.0 and later. daybreak = true on its own does nothing
[features]
cli_daybreak = true
```
By default the controls and indicators are hidden and `/daybreak` is unavailable — and automatic Cyber routing is omitted even for saved Daybreak threads. Saved preferences themselves remain intact, so adding `cli_daybreak` restores the previous state. `codex exec --cyber-access-program` was also added for choosing a Cyber access program per turn; that override stays available with `cli_daybreak` disabled.

**features.cli_daybreak is not in the configuration reference**

As of 2026-10-12 there is no `cli_daybreak` row in the official reference's `features.*` table. The key name appears only in the release notes. The rows that *are* listed include `features.apps`, `features.hooks`, `features.memories`, `features.multi_agent` and `features.network_proxy`.


For `config.toml` as a whole, see What you can configure in Codex's config.toml.

Permissions got fixes in both releases.

- 
**Approved escalation** : approved filesystem escalation can now grant broader write access while preserving denied reads and network restrictions. Background tasks retain their originating turn's permissions
- 
**Linux sandbox** : startup with multiple denied files is fixed, and writable sandbox-construction executables are now rejected
- 
**ripgrep config** : ripgrep configuration no longer weakens deny-glob masks. Paths you were protecting with a deny rule may have been visible to search, so this is worth upgrading for if you exclude sensitive files in the sandbox
- 
**Windows** : elevated terminal sessions can start using an embedded server, and ordinary drive-letter file access is restored on Windows 10

For the approval policy and sandbox settings themselves, see Codex approval policy and sandbox settings.

In v0.162.0, **when the worktrees feature is enabled**, there are tools for creating and listing managed Git worktrees from trusted local projects. The key that enables that feature could not be confirmed in the official configuration reference.

In the TUI: `/copy` navigates and copies transcript blocks, `Ctrl+Insert` copies a selection, mouse-wheel speed is tunable (the release notes name `tui.mouse_scroll_speed`, which likewise does not appear in the reference's `tui.*` table), and URLs are clickable in approval headers, questions, MCP prompts, warnings and banners, including across wrapped lines.

The rest, as it touches configuration and operations:

| Version | Change | 
|---|---|
| 0.160.1 | Preserves `SYSTEMROOT` ,`TEMP` and`TMP` when launching remote stdio MCP servers | 
| 0.161.0 | `/mcp login <name>` signs in to an MCP server from an active session | 
| 0.162.0 | `apply_patch` preserves existing CRLF line endings without an opt-in; Windows releases ship a signed PowerShell installer | 
| 0.162.1 | Fixes a TUI crash on multi-line asynchronous questions, and startup failures from feature-setting differences between a running background server and CLI defaults | 

For adding MCP servers, see Set up MCP servers in Codex; for the previous release, Codex CLI v0.160 changes.

- v0.161.0 makes GPT-6.1 Sol the default in the bundled and Amazon Bedrock catalogs; confirm separately that your account can use it
- To pin a model, set `model = "gpt-6.1-sol"` at the top level of`config.toml`
- Daybreak needs `--enable cli_daybreak` or`features.cli_daybreak = true` ;`daybreak = true` alone does nothing
- ripgrep configuration no longer weakens deny-glob masks — worth upgrading for if the sandbox excludes sensitive files
- 
`features.cli_daybreak` , the worktrees feature flag and`tui.mouse_scroll_speed` are all absent from the official configuration reference's tables

## Top comments (0)
