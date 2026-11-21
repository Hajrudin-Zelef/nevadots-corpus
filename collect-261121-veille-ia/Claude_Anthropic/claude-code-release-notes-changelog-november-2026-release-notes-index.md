---
id: collect-261121-veille-ia/claude-anthropic/claude-code-release-notes-changelog-november-2026-release-notes-index
title: "Claude Code Release Notes & Changelog · November 2026 — Release Notes Index"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2025-11-24"]
keywords: ["claude", "code", "release", "mode", "sessions", "longer", "notes", "json"]
source: https://releases.sh/anthropic/claude-code
source_anchor: ""
source_lines: [1, 40]
section: "Claude_Anthropic"
---
Fixed ~/.claude.json sometimes being reset to defaults when a network stall or file lock made it unreadable during a save, along with repeated macOS sign-outs when Claude Code could read the Keychain but not save to it. Saved scheduled tasks in new signed-in sessions mostly need scheduling again, and 2.1.326's automatic retry at the max_tokens limit is removed.

# Claude Code

AI coding agent for the terminal and IDE

`npx @buildinternet/releases get claude-code`
Interactive sessions with no permission mode now start in auto mode for Opus 4.6 and Sonnet 4.6 on Bedrock, Google Cloud's Agent Platform, Microsoft Foundry and the Claude apps gateway. MCP tool hooks gain "onFailure": "block", and a scheduled task save no longer erases a project's other saved tasks when the tasks file can't be read.

Stopping the Remote Control server in worktree mode no longer deletes running sessions' git-ignored files such as .env, and project-scope claude mcp add and remove no longer strip other top-level keys from .mcp.json. The release also adds a stdin form of claude mcp add-json that keeps tokens off the command line and a /status list of broken agents, skills, commands, keybinding errors, and hook matchers that never match.

Sandboxed commands on macOS, Linux and WSL no longer write to some sensitive home folders by default, and file tools now refuse writes to Claude Code's sign-in files such as .credentials.json. Added support for turning on Code Mode while agent teams are enabled, plus fixes for resumed sessions losing recent turns after a crash and background sessions losing the LLM gateway your shell exported.

A security issue in how zstd-compressed responses are decoded is fixed. Sandboxed Bash commands now follow the normal permission flow by default unless sandbox.autoAllowBashIfSandboxed is set, and read-only git commands ask first in untrusted repositories.

Code Mode is available as an experimental opt-in where Claude writes JavaScript to call its tools, and plugins can now declare an `engines["claude-code"]` version range in `plugin.json` so installs pick the newest compatible version. The release also removes the `subagentStatusLine` setting and deprecates a plugin's `agent` setting with startup warnings, alongside a large batch of fixes to credentials, permissions, and background sessions.

MCP OAuth requests no longer send secrets such as refresh tokens to another host when a server redirects them, and a read-only Bash command can no longer run outside the sandbox without a permission prompt. Claude Fable 5.2 is now the default Fable model with 1M context, and saved Fable 5.1 model picks move to fable.

Auto mode with telemetry or feature flags off now asks about or denies calls the server can't check instead of falling back to the built-in classifier, with CLAUDE_CODE_AUTO_MODE_SERVER=0 restoring the old behavior. The release also fixes a sandbox escape when creating a worktree and a Bash permission bypass where a crafted environment prefix could hide part of a command from permission checks.

Stored claude.ai sign-in end dates now appear in claude auth status and claude -p JSON results, with a stderr warning starting 10 days before expiry. Fixed config and login loss on shared network home directories, and a regression in 2.1.314 where --continue could lose the earlier conversation.

CLAUDE_CODE_DISABLE_NESTED_SESSION_ID=1 makes claude -p inside a self-hosted runner session use that session's ID and transcript as in earlier versions, and project and local MCP servers no longer disconnect after the first turn in some Claude desktop sessions. The release also adds a warning when /schedule assigns unrequested connectors and a check that asks before a web or shell tool call sends your account email to the network.

Fast mode for Opus 4.8 is deprecated and will be removed on November 30, 2026, after which requests return an error without falling back to standard Opus 4.8; migrating to Opus 5.5 keeps the speedup. Also added CLAUDE_CODE_SUBAGENT_EFFORT to set subagent effort instead of inheriting the session's, plus pooled group budgets and layered policy mode for the Claude apps gateway, and fixes for orphaned shell commands on exit and Bash tool alias handling.

Added permissions.blockWritesOutsideWorkingDirectories, which makes Write, Edit and NotebookEdit refuse paths outside the working directories in every permission mode, plus idle compaction for 200K+ token conversations left idle in VS Code and Claude Desktop's Code tab. Fixed sessions failing turn after turn with "Invalid effort level" when an admin caps a model's effort, claude -p exiting 0 with no output on startup failure, and Bedrock sessions with many images failing compaction with 400 Input is too long.

The Write tool previously could overwrite a very large CLAUDE.md, rules file, or @-included file in a subdirectory after seeing only its start; it now asks Claude to read the whole file or edit in place. Also fixed per-directory and paths: skills not being found after a Bash cd deeper into a git repository, invalid command names blocking all command loading and leaving claude -p with no output, and managed settings not being fetched for the rest of the session after signing in from VS Code.

Fixed long startup and post-edit freezes caused by very long CLAUDE.md list items, and standard-context sessions that hit "Usage credits required for 1M context" over 200K tokens now compact and continue instead of stopping. Also fixed interrupt latency during stalled MCP reads, out-of-order transcript messages in claude -p and SDK sessions, a 5-second Write/Edit stall on thousands of lines, and subagents losing worktrees or missing late-connecting MCP servers, plus dozens of smaller fixes.

Fixed background cleanup deleting recent files such as pasted images when cleanupPeriodDays is set to a very large value, and requests failing behind gateways that reject the effort-2025-11-24 beta value or mid-conversation system messages now retry without them. Added the Monitor tool and plugin monitors on Bedrock, Vertex, and Foundry, plus Enterprise custom roles for cloud sessions and Remote Control.

Sandboxed Bash commands on macOS and Linux no longer run the shell's startup files outside the sandbox, symlinked macOS denyWrite globs now apply at real paths, and sandboxed commands and auto-accepted edits can no longer modify the MCP config passed with --mcp-config. Also fixes a compaction loop from large nested CLAUDE.md files, lost Claude Desktop session messages on connection refresh, and ctrl+enter queuing behavior.

Added /claude-api api-version to move Claude API code to API version 2026-10-21, and a claude_code.safety_event OpenTelemetry event for each stopped, prompted, or sent-back tool call. Fixed HTTP MCP tool calls being sent twice when a connection closed mid-call by retrying only read-only or idempotent tools, plus dozens of smaller fixes across MCP, cloud sessions, VSCode, and the self-hosted runner.

Fixed claude -p inside cloud sessions reporting "Not logged in" when started without the session's environment variables. Also fixed an API error being replaced by "prompt is too long" and an unneeded compaction in sessions with many MCP tools.
