---
id: collect-261121-veille-ia/divers-tech/side-chat-tabs-arrive-in-the-terminal-workspace-vibecoding-cz
title: "Side-chat tabs arrive in the terminal workspace | Vibecoding.cz"
domain: divers-tech
role: reference
task: article
actors: []
dates: []
keywords: ["terminal", "side", "tabs", "workspace", "chats", "conversation", "without", "provider"]
source: https://www.vibecoding.cz/en/codex-weekly-2026-w46/
source_anchor: ""
source_lines: [1, 79]
section: "Divers_Tech"
---
*One chat thread means every side question either interrupts the main task or waits until it ends. This week the terminal gets a workspace that keeps a second conversation next to the first, now with tabs for several of them. Alongside it come provider management without leaving the TUI, a quieter transcript view and a handful of changes to check in your config.*

This update covers week 46 (November 11-13, 2026): versions 0.167.2 to 0.169.0, three releases and 29 changelog entries.

## Side chats get panes, tabs and a restart-proof history

**What it is.** Enable `features.tui_workspace` and the terminal shows a side conversation beside the main chat. Version 0.168.0 introduced the layout. Version 0.169.0 added tabs to it.

**How you handle the panes.** Drag the divider to resize the two panes. Press `leader z` to zoom the focused pane when you want one conversation to fill the screen, and press it again to get the split back.

**What the tabs change.** As of 0.169.0 the workspace supports multiple side-chat tabs. You switch between them with the keyboard or the mouse, so you are no longer limited to one side conversation at a time.

**Saved side chats come back.** Saved local side chats can be reopened after you restart Codex. A side conversation therefore no longer disappears with the terminal session that created it.

**Conditions.**

- The workspace is opt-in. Nothing changes until you set `features.tui_workspace` .
- Tabs need 0.169.0 or later. On 0.168.0 you get the single side pane with the divider and zoom.
- Only saved local side chats can be reopened after a restart.

(The tab shortcuts are not spelled out in the changelog, so check what your build offers before building muscle memory around them.)

## Changes for everyday work

**Providers from the terminal.** Under Sessions â Model providers you can browse the available providers and choose the default for new chats. Version 0.169.0 adds the editing side: you can add or edit custom providers without leaving the terminal.

**Gateway sign-in over SSH.** With custom gateway onboarding enabled, you can create a provider during sign-in. For supported gateways you can use a device code, which helps on an SSH session or any machine without a local browser.

**Existing chats keep their provider.** Open and resumed chats keep their saved model provider after you change the default for new chats. Switching the default will not move a running conversation to a different provider.

**A shorter transcript.** Turn on **Collapse turn details** in the full-screen transcript to fold completed activity beneath each prompt. Answers stay visible and tool output remains searchable, so folding hides the clutter without removing anything you might grep for later.

**Audio attachments.** With an audio-capable model you can attach recordings to a conversation. Codex skips recordings it has confirmed as rejected on later turns, and it uses clip duration to keep more short recordings in context.

**Animated GIFs.** Enable `features.image_generation_animation` to generate or edit animated GIFs. Optional controls set the frame count and timing.

**Cloud from the terminal.** The opt-in Cloud connection on macOS and Linux lets you send images, choose a model and reasoning effort, and archive or restore Cloud conversations without leaving the terminal.

## Technical notes

**Vertex AI.** Version 0.167.2 turns on incremental tool updates for Vertex AI by default. To opt out, set `incremental_tools_legacy_gcp = false` under `[features]` in `config.toml`. If you run Vertex AI, this is the one change in the week that applies without you doing anything.

**Terminal display and notifications.**

- Fullscreen Codex shows image previews and pets in tmux on Ghostty and Kitty when tmux has `allow-passthrough all` set.
- Set `tui.notification_method = "osc777"` to get terminal notifications through OSC 777.
- Approval notifications show longer command previews and distinguish terminal input requests from network access requests.

**Agent command center.** You can now search older tasks by name, prompt preview or working directory.

**Subagent configuration.** Codex warns when `features.multi_agent` is explicitly configured. The warning points you to `agents.enabled` for disabling subagent tools.

**Trust in non-interactive runs.** `codex exec` keeps project config and MCP servers disabled in untrusted directories. To save project trust explicitly, pass `--trust` to `codex exec` or `codex review`.

**Plan handoff.** Choosing âYes, clear context and implementâ now hands the full approved plan to the fresh session. If the handoff fails, the current session stays open, so the plan is not lost.

## What to take away

**If you work mostly in the terminal.**

- Enable `features.tui_workspace` and try a side chat next to a running task. Resize the divider, zoom with`leader z` and open a second tab.
- Turn on **Collapse turn details** in the full-screen transcript for long sessions.
- Running tmux in Ghostty or Kitty? Add `allow-passthrough all` to get image previews and pets.
- Set `tui.notification_method = "osc777"` if your terminal supports it.

**If you run parallel sessions.**

- Restart Codex once and check that your saved local side chats reopen as expected.
- Use the search in the agent command center to find older tasks by name, prompt preview or working directory.
- Change the default provider under Sessions â Model providers and confirm that your open and resumed chats keep the provider they started with.

**If you administer integrations and company deployments.**

- On Vertex AI, decide whether incremental tool updates suit you. The opt-out is `incremental_tools_legacy_gcp = false` under`[features]` .
- Search your shared configs for `features.multi_agent` . The new warning points to`agents.enabled` .
- Review scripts that call `codex exec` or`codex review` in untrusted directories. Project config and MCP servers stay disabled there unless you pass`--trust` .
- If you use custom gateways, test the device-code sign-in over SSH on a supported gateway.

*Sources: Codex CLI changelog, versions 0.167.2 to 0.169.0 (November 11-13, 2026), and the Codex documentation. The terminal workspace, the Cloud connection, custom gateway onboarding and GIF generation are opt-in or behind feature flags, so availability may change.*
