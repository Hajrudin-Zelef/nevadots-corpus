---
id: collect-261121-veille-ia/divers-tech/background-subagents-by-default
title: "Background Subagents by Default"
domain: divers-tech
role: reference
task: article
actors: []
dates: []
keywords: ["background", "experimental", "subagents", "session", "tool", "coreadded", "support", "output"]
source: https://opencode.ai/changelog/v2.3.0
source_anchor: ""
source_lines: [1, 69]
section: "Divers_Tech"
---
1. Subagents now return immediately, continue in the background, and report back whenever they become idle. You can keep working in the parent session while multiple subagents run concurrently.#63382 The TUI now summarizes each run of tool calls and thoughts into one collapsible activity row, so the answer is easier to follow. To see every step again, set **Verbosity** to`medium` or`high` in settings, or set`session.verbosity` in your TUI config; an existing setting is kept.#63207In the desktop app, open **Jump to message** from the command palette or press`Cmd+Alt+G` (`Ctrl+Alt+G` on Windows and Linux) to search prompts across the current session and jump directly to one.#63585## Improvements
  - CoreAdded experimental support for MCP events, behind the `mcp_events` flag.#63254#63261#63269
  - CoreShell commands now survive server handoffs and keep their output when interrupted.
  - CoreAdded experimental support for moving a shell command to the background when its timeout expires, behind the `shell_timeout_background` flag. With the flag on, an explicit timeout on a background command stops it.#63024#63703
  - CoreLocal Windows shell commands now run through the process daemon.
  - CoreAdded support for freeform tool input on compatible models.
  - CoreAdded LM Studio decision-model support.
  - CoreAgents are now told when their session is a fork. Thanks @hasta84.#63235
  - CoreTool output truncation now keeps the beginning of the output and preserves a usable preview if the full output cannot be saved.
  - CoreTail reads no longer load the entire file into memory.
  - CoreProviders can present a client certificate for gateways that require mutual TLS. Set `settings.tls` with`cert` ,`key` and an optional`passphrase` on the provider; values accept`{file:}` and`{env:}` like`apiKey` .#61832
  - CoreWhen a subagent goes idle while its own background work is still running, its report to the parent now says it is still running, so the parent waits for the final report instead of treating the partial answer as done.#63675
  - CoreCode Mode gets simpler structured results from file and skill tools: `write` and`edit` return their confirmation as a string,`patch` returns an empty result, and`skill` returns the skill's directory and content.#63616#63635#63618#63623
  - TerminalSession IDs found in messages can now be opened directly.
  - TerminalActivity tabs are clickable and open the correct view.
  - TerminalShell mode has its own prompt color under the new designs.
  - DesktopAdded the experimental **Recent** sessions sidebar, with cross-server recency, local pins, reordering, and standard session-row actions.
  - DesktopRecent sessions are limited to the last seven days and 30 rows, and can be hidden or muted.
  - DesktopAdded an experimental customizable sessions sidebar.
  - DesktopMermaid diagrams can now be maximized for zooming and panning.#59250
  - DesktopAdded `Alt+Enter` to place a follow-up first in the queue and`Mod+Enter` to steer the next queued prompt.
  - DesktopHolding the modifier key now shows tab shortcut numbers.
  - DesktopEach serverâs terminal shell can be selected from its menu.
  - DesktopAdded a free-limit panel in place of the composer when applicable.
  - DesktopSession IDs are easier to find and copy.
  - SDKEvent subscriptions can now receive `experimental.mcp.event` events from MCP event subscriptions.
  - SDKModel capability schemas now advertise supported freeform tool-input formats.
  - SDKSession throughput is now measured from the first streamed token.
  - SDKSession creation documentation now clarifies the `parentID` field.
  - ExtensionsPromise-based tools can cooperatively stop through `ToolContext.signal` .
  - ExtensionsAdded the experimental `session.fork` hook.
  - ExtensionsPromise plugin cleanup failures no longer prevent subsequent plugin replacements.
  - Extensions`@opencode/plugin/effect` now exports`Tool` , so an`execute.before` hook can reject a call with`Tool.Error` without importing`@opencode/schema/tool` .#63698
 ## Fixes
  - CoreFixed case-only file renames in patches on case-insensitive filesystems.
  - CoreFixed stale thinking being retained after Claude Agent prompts or tools change.
  - CoreFixed directly driven child sessions not resuming after a restart.
  - CoreFixed plugin replacement stopping when cleanup of an earlier plugin fails.
  - CoreFixed MCP OAuth refreshes and localhost redirects when a server rewrites `127.0.0.1` .
  - CoreFixed saved API keys being omitted from local-model discovery.
  - CoreLocal model discovery (Ollama, LM Studio, vLLM) waits up to 15 seconds per request instead of 1, so remote or tunnelled endpoints list their models, and a slow endpoint no longer delays other endpoints or config reloads.#63700
  - CoreIn Code Mode, calling a namespace as if it were a tool now suggests up to three real tools under it.#63697
  - CoreFixed retryable provider streams losing completed work when interrupted.
  - CoreFixed GitHub Copilot requests using the wrong endpoint before models finish loading.
  - CoreFixed exhausted request rate limits being treated as terminal. Thanks @Kyle-j-bush.#59816
  - CoreFixed LM Studio models with a null context length and accepted decision-type models. Thanks @tomzhu1024.#56214
  - TerminalRestored pasted text when undo brings back its placeholder.
  - TerminalRestored the live draft after browsing prompt history.
  - TerminalFixed copying a paste from within the prompt selection.
  - TerminalPreserved the Mini composer caret when closing the command palette.
  - TerminalPrevented image bytes from being retained in transcript storage when image previews are disabled.
  - TerminalFixed paste-chip contrast across light and dark themes.
  - TerminalFixed plugin-provided themes updating live.
  - TerminalSession lists now open at the top rather than at the current session.
  - TerminalMini scrollback now shows the subagent card instead of obsolete launch instructions.
  - DesktopRecent sessions now keep stable recency ordering through list reloads, renames, and completed runs.
  - DesktopFixed browser panes not retrying after server plugins finish loading.
  - DesktopFixed mention suggestions being recreated while typing.
  - DesktopFixed SSH prompts failing to advance or close after connection.
  - DesktopFixed pasted provider-auth redirect URLs being rejected.
  - DesktopRestored message footers in the New timeline.
  - DesktopFixed links whose labels contain inline code.
  - DesktopFixed the desktop main process freezing while probing the login-shell environment.
  - DesktopFixed the drag region after changing layout direction.
  - DesktopFixed vertical-tab previews snapping to the wrong tab.
  - DesktopFixed the file-tree sidebar opening animation.
  - DesktopFixed pairing QR-code contrast in light themes. Thanks @ssskay.#63220
  - DesktopFixed activity-tab navigation and click handling. Thanks @OpeOginni.#60168
2. CoreAdded experimental support for MCP events, behind the
