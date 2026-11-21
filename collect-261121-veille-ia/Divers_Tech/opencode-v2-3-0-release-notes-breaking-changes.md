---
id: collect-261121-veille-ia/divers-tech/opencode-v2-3-0-release-notes-breaking-changes
title: "OpenCode v2.3.0 release notes & breaking changes"
domain: divers-tech
role: reference
task: article
actors: []
dates: ["2026-11-13"]
keywords: ["opencode", "github", "anomalyco", "pull", "background", "notes", "session", "release"]
source: https://agentbreakingchanges.com/tools/opencode/v2.3.0/
source_anchor: ""
source_lines: [1, 92]
section: "Divers_Tech"
---
OpenCode v2.3.0 is a SST release published 2026-11-13 with 72 listed changes. Summary and full notes below, extracted from the official notes.

One free email: every breaking change, retirement and price change across 23 coding agents. No spam, unsubscribe anytime.

Subscribe free```
### Highlights
- **Background Subagents by Default**: Subagents now return immediately, continue in the background, and report back whenever they become idle. You can keep working in the parent session while multiple subagents run concurrently. ([#63382](https://github.com/anomalyco/opencode/pull/63382))
- **Quieter Transcripts by Default**: The TUI now summarizes each run of tool calls and thoughts into one collapsible activity row, so the answer is easier to follow. To see every step again, set **Verbosity** to `medium` or `high` in settings, or set `session.verbosity` in your TUI config; an existing setting is kept. ([#63207](https://github.com/anomalyco/opencode/pull/63207))
- **Jump to an Earlier Prompt**: In the desktop app, open **Jump to message** from the command palette or press `Cmd+Alt+G` (`Ctrl+Alt+G` on Windows and Linux) to search prompts across the current session and jump directly to one. ([#63585](https://github.com/anomalyco/opencode/pull/63585))
### Core
#### Improvements
- Subagents now always run in the background, return their session ID immediately, and report results to their parent whenever they become idle. ([#63382](https://github.com/anomalyco/opencode/pull/63382))
- Added experimental support for MCP events, behind the `mcp_events` flag. ([#63254](https://github.com/anomalyco/opencode/pull/63254), [#63261](https://github.com/anomalyco/opencode/pull/63261), [#63269](https://github.com/anomalyco/opencode/pull/63269))
- Shell commands now survive server handoffs and keep their output when interrupted.
- Added experimental support for moving a shell command to the background when its timeout expires, behind the `shell_timeout_background` flag. With the flag on, an explicit timeout on a background command stops it. ([#63024](https://github.com/anomalyco/opencode/pull/63024), [#63703](https://github.com/anomalyco/opencode/pull/63703))
- Local Windows shell commands now run through the process daemon.
- Added support for freeform tool input on compatible models.
- Added LM Studio decision-model support.
- Agents are now told when their session is a fork. Thanks @hasta84. ([#63235](https://github.com/anomalyco/opencode/pull/63235))
- Tool output truncation now keeps the beginning of the output and preserves a usable preview if the full output cannot be saved.
- Tail reads no longer load the entire file into memory.
- Providers can present a client certificate for gateways that require mutual TLS. Set `settings.tls` with `cert`, `key` and an optional `passphrase` on the provider; values accept `{file:}` and `{env:}` like `apiKey`. ([#61832](https://github.com/anomalyco/opencode/pull/61832))
- When a subagent goes idle while its own background work is still running, its report to the parent now says it is still running, so the parent waits for the final report instead of treating the partial answer as done. ([#63675](https://github.com/anomalyco/opencode/pull/63675))
- Code Mode gets simpler structured results from file and skill tools: `write` and `edit` return their confirmation as a string, `patch` returns an empty result, and `skill` returns the skill's directory and content. ([#63616](https://github.com/anomalyco/opencode/pull/63616), [#63635](https://github.com/anomalyco/opencode/pull/63635), [#63618](https://github.com/anomalyco/opencode/pull/63618), [#63623](https://github.com/anomalyco/opencode/pull/63623))
#### Bug fixes
- Fixed case-only file renames in patches on case-insensitive filesystems.
- Fixed stale thinking being retained after Claude Agent prompts or tools change.
- Fixed directly driven child sessions not resuming after a restart.
- Fixed plugin replacement stopping when cleanup of an earlier plugin fails.
- Fixed MCP OAuth refreshes and localhost redirects when a server rewrites `127.0.0.1`.
- Fixed saved API keys being omitted from local-model discovery.
- Local model discovery (Ollama, LM Studio, vLLM) waits up to 15 seconds per request instead of 1, so remote or tunnelled endpoints list their models, and a slow endpoint no longer delays other endpoints or config reloads. ([#63700](https://github.com/anomalyco/opencode/pull/63700))
- In Code Mode, calling a namespace as if it were a tool now suggests up to three real tools under it. ([#63697](https://github.com/anomalyco/opencode/pull/63697))
- Fixed retryable provider streams losing completed work when interrupted.
- Fixed GitHub Copilot requests using the wrong endpoint before models finish loading.
- Fixed exhausted request rate limits being treated as terminal. Thanks @Kyle-j-bush. ([#59816](https://github.com/anomalyco/opencode/pull/59816))
- Fixed LM Studio models with a null context length and accepted decision-type models. Thanks @tomzhu1024. ([#56214](https://github.com/anomalyco/opencode/pull/56214))
### TUI
#### Improvements
- Transcript verbosity now defaults to `low`, which summarizes each run of tools and thoughts into one activity row. A `session.verbosity` you already set is kept. ([#63207](https://github.com/anomalyco/opencode/pull/63207))
- Session IDs found in messages can now be opened directly.
- Activity tabs are clickable and open the correct view.
- Shell mode has its own prompt color under the new designs.
#### Bug fixes
- Restored pasted text when undo brings back its placeholder.
- Restored the live draft after browsing prompt history.
- Fixed copying a paste from within the prompt selection.
- Preserved the Mini composer caret when closing the command palette.
- Prevented image bytes from being retained in transcript storage when image previews are disabled.
- Fixed paste-chip contrast across light and dark themes.
- Fixed plugin-provided themes updating live.
- Session lists now open at the top rather than at the current session.
- Mini scrollback now shows the subagent card instead of obsolete launch instructions.
### Desktop
#### Improvements
- Added **Jump to message** to the command palette with `Cmd+Alt+G` / `Ctrl+Alt+G`, including prompts not yet loaded in the timeline. Thanks @usrnk1. ([#63585](https://github.com/anomalyco/opencode/pull/63585))
- Added the experimental **Recent** sessions sidebar, with cross-server recency, local pins, reordering, and standard session-row actions.
- Recent sessions are limited to the last seven days and 30 rows, and can be hidden or muted.
- Added an experimental customizable sessions sidebar.
- Mermaid diagrams can now be maximized for zooming and panning. ([#59250](https://github.com/anomalyco/opencode/pull/59250))
- Added `Alt+Enter` to place a follow-up first in the queue and `Mod+Enter` to steer the next queued prompt.
- Holding the modifier key now shows tab shortcut numbers.
- Each server’s terminal shell can be selected from its menu.
- Added a free-limit panel in place of the composer when applicable.
- Session IDs are easier to find and copy.
#### Bug fixes
- Recent sessions now keep stable recency ordering through list reloads, renames, and completed runs.
- Fixed browser panes not retrying after server plugins finish loading.
- Fixed mention suggestions being recreated while typing.
- Fixed SSH prompts failing to advance or close after connection.
- Fixed pasted provider-auth redirect URLs being rejected.
- Restored message footers in the New timeline.
- Fixed links whose labels contain inline code.
- Fixed the desktop main process freezing while probing the login-shell environment.
- Fixed the drag region after changing layout direction.
- Fixed vertical-tab previews snapping to the wrong tab.
- Fixed the file-tree sidebar opening animation.
- Fixed pairing QR-code contrast in light themes. Thanks @ssskay. ([#63220](https://github.com/anomalyco/opencode/pull/63220))
- Fixed activity-tab navigation and click handling. Thanks @OpeOginni. ([#60168](https://github.com/anomalyco/opencode/pull/60168))
### SDK
#### Improvements
- Event subscriptions can now receive `experimental.mcp.event` events from MCP event subscriptions.
- Model capability schemas now advertise supported freeform tool-input formats.
- Session throughput is now measured from the first streamed token.
- Session creation documentation now clarifies the `parentID` field.
### Extensions
#### Improvements
- Promise-based tools can cooperatively stop through `ToolContext.signal`.
- Added the experimental `session.fork` hook.
- Promise plugin cleanup failures no longer prevent subsequent plugin replacements.
- `@opencode/plugin/effect` now exports `Tool`, so an `execute.before` hook can reject a call with `Tool.Error` without importing `@opencode/schema/tool`. ([#63698](https://github.com/anomalyco/opencode/pull/63698))
```
