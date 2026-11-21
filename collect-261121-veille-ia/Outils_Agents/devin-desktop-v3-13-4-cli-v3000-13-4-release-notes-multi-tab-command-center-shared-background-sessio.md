---
id: collect-261121-veille-ia/outils-agents/devin-desktop-v3-13-4-cli-v3000-13-4-release-notes-multi-tab-command-center-shared-background-sessio
title: "Devin Desktop v3.13.4 & CLI v3000.13.4 Release Notes: Multi-Tab Command Center & Shared Background Sessions | DigIntoAI"
domain: outils-agents
role: reference
task: article
actors: []
dates: ["2026-11-17"]
keywords: ["devin", "desktop", "sessions", "november", "v3000", "background", "local", "adds"]
source: https://digintoai.com/en/articles/devin-desktop-v3-13-4-cli-v3000-13-4-release-notes-multi-tab-command-center-shar-23a6557d84b3
source_anchor: ""
source_lines: [1, 27]
section: "Outils_Agents"
---
## On this page

Cognition's official stable release notes highlight Devin Desktop v3.13.4 (dated November 17, 2026) and Devin CLI v3000.13.4 (dated November 16, 2026).

Official stable changelogs now also list Desktop v3.14.30819 and CLI v3000.14.30821, both dated November 17, 2026.

## **Devin Desktop v3.13.4 (November 17, 2026): Tabs, Background Sessions & Previews**

**New Features:** Adds tabs to the Agent Command Center with independent Back and Forward histories per tab; runs local sessions in a background process so closing or reopening Devin Desktop or the CLI does not stop a session and multiple clients can open it simultaneously; adds local PDF preview, table views for CSV and TSV files, and broader media playback in the Files tab; and nests each Devin Local subagent's output beneath it alongside a running subagent count.

**Changes & Fixes:** The embedded IDE now requires Cmd/Ctrl+S to save files (marking unsaved tabs with a dot) rather than auto-saving, and chat diffs use Devin diff colors. Fixes resolve starred/last-picked default agents, Devin Local reconnection and ACP authentication errors, large file-write crashes, connection-reset login issues, and Linux/Windows window crashes.

## **Devin Desktop v3.14.30819 (November 17, 2026)**

Devin Bots are now available. A Bot can be given an outcome to own in its chat; it plans the work and keeps running sessions until that work is done. Official documentation says a Bot’s work counts toward the plan’s usage like any other session.

Enterprise administrators can turn on proxy auto-detection for the Devin Desktop language server on managed macOS, Windows, and Linux machines, using the DetectProxy device policy or the devin.network.detectProxy setting. The image viewer adds zoom buttons and supports Cmd/Ctrl + +, −, and 0.

Devin Local now retries a stalled model stream after 40 seconds instead of remaining on “Running tools” for four minutes, and long Devin Local sessions on Anthropic models keep the prompt cache across turns.

## **Devin CLI v3000.13.4 (November 16, 2026): Daemon Controls & .kiro/skills Discovery**

**New CLI Capabilities:** Shares the background session process across clients without locking sessions open in another terminal or editor; adds devin daemon subcommands to start, stop, and check the background process (which exits automatically about ten minutes after its last session and client disconnect); adds --process-model and the process_model config setting to switch between daemon and in-process modes; discovers skills in .kiro/skills/{name}/SKILL.md across projects and ~/.kiro/skills/ (disable via read_config_from.kiro = false); and displays image attachment thumbnails in kitty and Ghostty.

**CLI Changes & Fixes:** devin ls, devin -r, and /resume now open a session monitor sorted by last update; sandboxed enterprise domain allowlists act as a hard block; approved ask-mode cloud MCP tools execute once with those arguments; and large response rendering and Ctrl+R history search are faster. Fixes ensure AGENTS.md/CLAUDE.md load after cd, malformed MCP/hooks entries no longer hide sibling entries, and non-UTF-8 environment variables no longer crash shell commands.

The CLI stable changelog lists v3000.14.30821, also dated November 17, 2026. It likewise retries a stalled model stream after 40 seconds instead of remaining on “Running tools” for four minutes.
