---
id: collect-261121-veille-ia/divers-tech/opencode-changelog-and-release-notes-for-every-version-15
title: "OpenCode | Changelog and release notes for every version"
domain: divers-tech
role: reference
task: article
actors: []
dates: []
keywords: ["session", "opencode", "plugin", "subagent", "open", "tool", "changelog", "improvements"]
source: https://opencode.ai/changelog?area=terminal,extensions
source_anchor: ""
source_lines: [1, 85]
section: "Divers_Tech"
---
# Changelog

New updates and improvements to OpenCode

1. Removed time to first token (ttft) from the assistant message footer. Tokens per second still shows when See all highlights in v2.3.1`session.tps` is on.#63759### Improvements
  - TerminalPermission prompts from a subagent, and from that subagent's own subagents, now appear while you view the subagent's session. Prompts also show in front of an open composer.#64343
  - TerminalThe export dialog under the new designs can save to the system temp directory as well as the project folder. Thanks @Tarquinen.#62490
  - ExtensionsEach tool call inside a Code Mode program now has its own call ID in plugin hooks and traces. Its permission requests still show on the `execute` call's row.#64324
  - ExtensionsPlugin `SessionIdleBackground` items can now be`{ kind: "codemode", sessionID, jobID }` , and GUI plugin`BridgeLayout` accepts an optional`clip` rectangle.#64343#63819
 ### Fixes
  - TerminalWhen another TUI sharing your tabs closes the session you're viewing, closing it here now moves to the neighbouring tab.#64261
  - TerminalFixed broken characters at the edges of the shell output viewer and the inline shell display when output is long.#63967#63984
  - ExtensionsThe browser extension's URL bar action buttons are now spaced 6px apart.#64214
2. The TUI now summarizes each run of tool calls and thoughts into one collapsible activity row, so the answer is easier to follow. To see every step again, set See all highlights in v2.3.0**Verbosity** to`medium` or`high` in settings, or set`session.verbosity` in your TUI config; an existing setting is kept.#63207### Improvements 7
  - TerminalSession IDs found in messages can now be opened directly.
  - TerminalActivity tabs are clickable and open the correct view.
  - TerminalShell mode has its own prompt color under the new designs.
  - ExtensionsPromise-based tools can cooperatively stop through `ToolContext.signal` .
  - ExtensionsAdded the experimental `session.fork` hook.
  - ExtensionsPromise plugin cleanup failures no longer prevent subsequent plugin replacements.
  - Extensions`@opencode/plugin/effect` now exports`Tool` , so an`execute.before` hook can reject a call with`Tool.Error` without importing`@opencode/schema/tool` .#63698
 ### Fixes 9
  - TerminalRestored pasted text when undo brings back its placeholder.
  - TerminalRestored the live draft after browsing prompt history.
  - TerminalFixed copying a paste from within the prompt selection.
  - TerminalPreserved the Mini composer caret when closing the command palette.
  - TerminalPrevented image bytes from being retained in transcript storage when image previews are disabled.
  - TerminalFixed paste-chip contrast across light and dark themes.
  - TerminalFixed plugin-provided themes updating live.
  - TerminalSession lists now open at the top rather than at the current session.
  - TerminalMini scrollback now shows the subagent card instead of obsolete launch instructions.
3. GUI plugins can now add color themes, render or add actions to code blocks, and use native settings and menu components.#61485#61533#61527#61509 See all highlights in v2.2.6### Improvements
  - TerminalAdded **Scroll on send** in Settings. Choose**Keep** (`session.submit_scroll: "keep"` ) to retain your reading position after submitting a prompt.#61460
 ### Fixes 11
  - TerminalAllow Ctrl+Z to suspend OpenCode while a form or dialog is open.#62118
  - TerminalKeep focus on an open dialog when a question arrives and contain plugin dialogs that throw during rendering.#60365#62110
  - TerminalShow an error when Ctrl+V cannot read the clipboard. Thanks @jimmyallinai.#61094
  - TerminalPrevent session titles containing newlines from crashing the TUI. Thanks @vishalshekhar.#57523
  - TerminalImproved message spacing and prevented older history from flashing while scrolling.#62104#59236
  - TerminalJump to the first or last message with Home, End, and Ctrl+G in subagent sessions. Thanks @ricky9408.#60817
  - TerminalKeypad Home, End, Page Up, and Page Down now navigate the transcript like the main keys in terminals that use the kitty keyboard protocol. Thanks @argszero.#59770
  - Terminal`opencode mini` now uses the agent colors from your config. Thanks @ferdinandyb.#62067
  - TerminalReturn to the `/mcp` list after signing in to an MCP server, with that server still selected. Thanks @Tarquinen.#62245
  - TerminalAllow favoriting models with Ctrl+F in `/models` when they come from a provider declared in config or an add-on instead of a saved connection. Thanks @Tarquinen.#62244
  - ExtensionsIsolated plugin dialogs, Markdown renderers, terminal views, lazy chunks, and artifact viewers so failures do not take down their host surface.#62110#62123#61993
4. TerminalAdded 
5. Enable See all highlights in v2.2.4`"experimental": { "artifacts": true }` in`opencode.json` to let agents publish, update, and serve HTML or static artifacts through the built-in local provider or plugin-provided storage.#60351### Fixes
6. Added lazy loading for stored image references when See all highlights in v2.2.3`experimental.media_blob` is enabled. Images are downloaded only for previews, while undo, revert, fork, and prompt recall restore referenced attachments safely.#60116### Breaking changes
 #60385`@opencode/plugin` now requires`@opentui/core` and`@opentui/solid` 0.8.0 or later. In OpenTUI 0.8, a ScrollBox's`wrapper` ,`viewport` ,`content` , and scrollbars are facade objects instead of renderables, and a custom`RenderContext` must implement`clock` and`onAnimationTick` . See the OpenTUI 0.8.0 release notes.### Improvements### Fixes
  - TerminalFixed PHP syntax highlighting for code blocks without an opening `<?php` tag.#60359
  - TerminalPreserved `#` in file-search paths unless it introduces a valid line range.#60336
  - TerminalKept the configured default cursor style after config reloads.#60302
  - TerminalMatched the search cursor to the highlighted row when a selection dialog has no query.#60310
  - ExtensionsPreserved `null` values in Code Mode text encoding.#57639@xiajiahang
  - ExtensionsTidied nested Extensions menus.#59468
7. TerminalFixed PHP syntax highlighting for code blocks without an opening 
8. Timeline quotes, terminal quotes, and browser annotations now share a compact optional-comment editor, while source highlights remain attached to local drafts.#60164 See all highlights in v2.2.2### Fixes
  - TerminalPreserved captured attachments and their original source paths.#60222@OpeOginni
  - TerminalPrevented middle-truncated paths from overflowing at very narrow terminal widths.
  - TerminalImproved Go usage-limit messages with the reset time and account page link.
  - TerminalCorrectly typed and displayed Go and ChatGPT usage-limit errors.
  - ExtensionsCorrected visible flowchart edges connected through Merman subgraphs.
9. Run See all highlights in v2.2.1`/goal <objective>` in the TUI or Desktop to let the session continue until a reviewer confirms the objective is met. Use`/goal pause` ,`/goal resume` , or`/goal clear` to control it.#59437### Improvements
  - ExtensionsLet Code Mode extension globals return Effects and declare required services.#59948
 ### Fixes
  - TerminalShow execution failures that occur before the first model step.#60079
  - TerminalKeep the activity spinner visible while a continued subagent runs.#59950
  - TerminalSend pasted image and PDF paths correctly when connected to a local server.#59833
  - TerminalImprove spacing in an empty `/usage` dialog.#59918@Tarquinen
10. See all highlights in v2.2.0### Breaking changes
 #59295`@opencode/plugin` now requires`@opentui/core` and`@opentui/solid` 0.7.0, up from 0.6.8 or later. Plugins that depend on OpenTUI 0.6 fail to install until they upgrade. In OpenTUI 0.7, custom renderables read their hooks from their class at construction: assign`renderBefore` ,`renderAfter` , and`onLifecyclePass` in the constructor instead of as class fields, call`refreshHooks()` after adding a hook by reflection, and replace`defineNativeIntegration()` with a`static nativeIntegration` declaration. See the OpenTUI 0.7.0 release notes.### Improvements
  - TerminalList OpenCode Console before OpenCode Go in the integration picker.#59107
  - TerminalAdd `/usage` , model-picker, prompt, sidebar, warning, and error details for OpenCode Console usage limits.#58731
  - TerminalMake CLI service registration more reliable when concurrent processes update its files.#59105
 ### Fixes 10
  - TerminalReturn `opencode run --format json` server connection errors as JSON.#59366@andreivince
  - TerminalPreserve pasted prompt text when copying it back out.#59209
  - TerminalReliably update the background service on Windows when its files are briefly locked.#59105
  - TerminalKeep the background service state directory out of tool shells and identify processes blocking its port.#58815
  - TerminalRefresh Git branch details after switching branches.#58982
  - TerminalKeep the TUI running when a plugin slot or route renders bare text.#58144@anmol0b@Tarquinen
  - TerminalAccept `--server` and`--standalone` anywhere in supported CLI commands.#58505
  - TerminalRelease closed and background session transcripts to prevent memory growth.#58434#57025
  - TerminalConnect remote access tunnels through HTTP proxies.#58464
  - TerminalKeep development builds on a separate server, database, and port from released builds.#58331
