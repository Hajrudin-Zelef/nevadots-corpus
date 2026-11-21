---
id: collect-261121-veille-ia/gemini-google/this-week-in-claude-code-codex-and-gemini-cli-week-of-october-11-2026
title: "This week in Claude Code, Codex and Gemini CLI (week of October 11, 2026)"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-10-11"]
keywords: ["claude", "week", "haiku", "code", "model", "default", "hooks", "also"]
source: https://dev.to/aicoding-guide/this-week-in-claude-code-codex-and-gemini-cli-week-of-october-11-2026-2k6d
source_anchor: ""
source_lines: [1, 90]
section: "Gemini_Google"
---
*Originally published at https://aicoding-guide.com.*

The change with the widest reach this week is **Claude Haiku 5.5**. It is now the default Haiku model on the Anthropic API, with a 1M-token context, and short prompts cost a tenth of what Haiku 4.5 did. On the Codex side, the **default model is now GPT-6.1 Sol**.

Claude Code also landed several fixes for permission rules that were being skipped. If you lean on the sandbox or on hooks to make permission decisions, this is a week to update early.

**Key point**

This week's highlights


- Claude Haiku 5.5 is the default Haiku on the API (1M context, $0.10 / $0.50)
- Claude Code fixed Bash deny and ask rules being skipped, and a permission bypass for UNC-path file reads
- Codex's default model is GPT-6.1 Sol, and Daybreak now needs an explicit opt-in

`claude-haiku-5-5` was added and is now the default Haiku model on the Anthropic API. The CHANGELOG reads "1M context, $0.10/$0.50 per Mtok ($0.50/$2.50 for prompts over 100K)", and the official pricing page fetched on 2026-10-11 has the same split.

| Model | Input (per Mtok) | Output (per Mtok) | 
|---|---|---|
| Claude Haiku 5.5 (prompts up to 100K) | $0.10 | $0.50 | 
| Claude Haiku 5.5 (prompts over 100K) | $0.50 | $2.50 | 
| Claude Haiku 4.5 | $1 | $5 | 

A tenth of Haiku 4.5 on short prompts — but the rate is 5x higher once you pass 100,000 tokens.

A cluster of fixes landed for the category "the thing you thought your settings blocked went through anyway."

- When the sandbox auto-allows commands, a Bash command with an **environment-variable prefix** such as`TZ="$HOME" rm -rf build` did not match deny or ask rules. The same held for a bare variable assignment before the command
- 
`Read` deny rules did not apply to files @-mentioned or selected in the IDE through a symlink
- 
**Security** : PreToolUse hook approvals and auto mode bypassed the permission prompt for file reads from network (UNC) paths
- A notebook or PDF read on macOS and Windows could return a file outside what was approved, through a link swapped in mid-read

Designing deny rules is covered in Design permissions in Claude Code's settings.json — though this week is also a run of examples showing that having written the rule is not itself the guarantee.

Command and HTTP hooks take `onFailure: "block"`. A hook that can't start, times out, or exits with an unexpected code now **blocks** the action instead of letting it through.


```
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          { "type": "command", "command": "bash .claude/hooks/check.sh", "onFailure": "block" }
        ]
      }
    ]
  }
}
```
If you use hooks as enforcement rather than as a request, a broken hook used to mean the action sailed past. 2.1.294 also fixed `prompt` and `agent` hooks written as instructions allowing what they should block.

| What was added | What it does | 
|---|---|
| `effort` on the Agent tool | Runs a subagent at the effort level you ask for | 
| `autoCompactWindow` in subagent frontmatter | Lets a subagent auto-compact earlier than the main conversation (also in `--agents` definitions) | 
| `CLAUDE_CODE_WORKFLOW_SUBAGENT_MODEL` | Runs every workflow agent on one model while other subagents keep theirs | 
| `allow_large` on the Read tool | Reads a text file past the usual size limits in one call | 

See Create subagents in Claude Code for the rest. `CLAUDE_CODE_OVERLOADED_RETRY_BASE_DELAY_MS` and `..._MAX_DELAY_MS` were also added, to lengthen the backoff when retrying an overloaded (529) request.

2.1.290 additionally fixed WebFetch silently dropping page text past 100,000 characters; it now says how much was unread and takes an `offset` to read on.

The release notes read "GPT-6.1 Sol is now the default model in the bundled and Amazon Bedrock catalogs." What is stated as default is **the bundled catalog and the Amazon Bedrock catalog**; other setups are not addressed. `/mcp login <name>` also arrived, for signing in to MCP servers from an active terminal session.

`daybreak=true` on its own is not enough. You need `--enable cli_daybreak` or `features.cli_daybreak=true`. By default the controls and indicators are hidden and `/daybreak` is unavailable; saved preferences themselves remain intact. For writing `config.toml`, see What you can configure in Codex's config.toml.

With the worktrees feature enabled, there are now **tools for creating and listing managed Git worktrees** from trusted local projects. `apply_patch` preserves existing CRLF line endings without an opt-in. In the TUI, `/copy` navigates and copies transcript blocks, and `tui.mouse_scroll_speed` tunes mouse-wheel scrolling.

Approved filesystem escalation can now grant broader write access while preserving denied reads and network restrictions, and background tasks retain their originating turn's permissions. 0.162.0 fixed Linux sandbox startup with multiple denied files and stopped **ripgrep configuration from weakening deny-glob masks**.

v0.63.0 is 15 fixes with no new features and no new setting keys. The two that matter: the removal of what caused `cannot spawn : No such file or directory` during diffs (Gemini CLI was overriding Git's `diff.external` with an empty string), and the fix for a negative `tools.truncateToolOutputThreshold` roughly doubling output instead of disabling truncation. Details in Gemini CLI v0.63 changes.

1. Move Claude Code to 2.1.296 or later — every permission-bypass fix this week is in that range
2. If you rely on Bash deny or ask rules under sandbox auto-allow, test a command with an environment-variable prefix and confirm it still stops
3. If you use hooks as enforcement, add `onFailure: "block"` to them
4. Review the model id where you use Haiku and move to `claude-haiku-5-5` , noting the 5x rate above 100,000 tokens
5. If you were using Daybreak in Codex, switch to `--enable cli_daybreak` or`features.cli_daybreak=true`

- Claude Haiku 5.5 is the default Haiku on the API: 1M context, $0.10 / $0.50 up to 100K tokens
- Claude Code 2.1.289 to 2.1.292 concentrate the fixes for skipped permission rules; the UNC-path read is marked Security
- 
`onFailure: "block"` stops the action when a hook itself fails
- Codex defaults to GPT-6.1 Sol in the bundled and Bedrock catalogs, and Daybreak needs the `cli_daybreak` opt-in
- Gemini CLI v0.63.0 is fixes only

## Top comments (0)
