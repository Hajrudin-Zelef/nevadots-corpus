---
id: collect-261121-veille-ia/outils-agents/devin-cli-adds-retry-for-stalled-model-streams-devin-central
title: "Devin CLI adds retry for stalled model streams — Devin Central"
domain: outils-agents
role: reference
task: article
actors: []
dates: []
keywords: ["devin", "model", "session", "stalled", "streams", "longer", "adds", "retry"]
source: https://devincentral.com/continuity/cli/2026-w47
source_anchor: ""
source_lines: [1, 80]
section: "Outils_Agents"
---
3 releases · updated

# Devin CLI adds retry for stalled model streams

Three releases of the Devin CLI shipped this week, with the main change being the retry of stalled model streams. This helps users who experience stalled model streams. The releases also include various fixes and improvements.

## v3000.14.30821 · November 17, 2026

This release fixes several issues, including the retry of stalled model streams after 40 seconds. It also fixes issues with `devin worker start`, `/rename`, and Vim mode. The spinner above an open agents or shells tray no longer shows `esc twice to interrupt`. Whole-line yanks and deletes in Vim mode now preserve blank lines.

## Official release notes

#### Fixed

- The CLI now retries a stalled model stream after 40 seconds instead of staying on “Running tools” for 4 minutes.
- `devin worker start` now lets you choose another organization when the configured organization is unavailable to your account.
- `/rename` no longer includes surrounding quotes in session titles.
- Moving the cursor in `/resume` and other picker search boxes no longer changes the highlighted row.
- Alt+U, Alt+L, and Alt+C now stop at punctuation when changing word case.
- The spinner above an open agents or shells tray no longer shows `esc twice to interrupt` .
- In Vim mode, whole-line yanks and deletes (`yy` ,`dd` ) now preserve blank lines.
- In Vim mode, `J` now joins lines in a Visual selection.

## v3000.13.4 · November 16, 2026

This release adds local sessions running in a background process, allowing users to close or reopen Devin Desktop or the CLI without stopping a session. It also adds skills discovery in `.kiro/skills/<name>/SKILL.md` and thumbnails for attached images in kitty and Ghostty. The release changes the behavior of `devin ls`, `devin -r`, and `/resume` to open a session monitor and fixes several issues, including loading repo rules and handling malformed MCP config files.

## Official release notes

#### Added

- Local sessions now run in a background process, so you can close or reopen Devin Desktop or the CLI without stopping a session, and open the same session in multiple clients at once.
- With the background process, a session that is open in another terminal or editor is no longer locked: opening it joins the session, and every client shows its output and questions.
- `devin daemon` subcommands start, stop, and check the background process, which exits on its own about ten minutes after its last session and client are gone.
- The `--process-model` option chooses between the background process (`daemon` ) and running sessions inside the CLI itself (`in-process` ), and the`process_model` setting in`config.json` or`/config` sets the default.
- Skills in `.kiro/skills/<name>/SKILL.md` are now discovered, in a project and in`~/.kiro/skills/` . Set`read_config_from.kiro` to`false` to turn them off.
- In kitty and Ghostty, the prompt now shows a small thumbnail of each attached image.

#### Changed

- `devin ls` ,`devin -r` , and`/resume` now open a session monitor that lists sessions by last update with their status, directory, and message count.
- With the sandbox on, an enterprise domain allowlist is now a hard limit: the CLI blocks a domain outside the list instead of asking you to allow it.
- When you allow a cloud MCP tool that your organization set to “ask”, the tool now runs once with those arguments.
- Large responses now render faster, and cursor movement in a long prompt and prompt history search (Ctrl+R) respond faster.

#### Fixed

- A repo’s `AGENTS.md` ,`CLAUDE.md` , and other rules now load after a command enters the repo with`cd` .
- A malformed entry in an MCP config file or hooks file no longer hides the other servers and hooks in that file.
- The CLI no longer crashes when it runs a shell command while an environment variable is not valid UTF-8.

## v3000.13.3 · November 16, 2026

This release is similar to v3000.13.4, with the same additions and changes, including local sessions running in a background process and skills discovery. It also fixes the same issues, including loading repo rules and handling malformed MCP config files.

## Official release notes

#### Added

- Local sessions now run in a background process, so you can close or reopen Devin Desktop or the CLI without stopping a session, and open the same session in multiple clients at once.
- With the background process, a session that is open in another terminal or editor is no longer locked: opening it joins the session, and every client shows its output and questions.
- `devin daemon` subcommands start, stop, and check the background process, which exits on its own about ten minutes after its last session and client are gone.
- The `--process-model` option chooses between the background process (`daemon` ) and running sessions inside the CLI itself (`in-process` ), and the`process_model` setting in`config.json` or`/config` sets the default.
- Skills in `.kiro/skills/<name>/SKILL.md` are now discovered, in a project and in`~/.kiro/skills/` . Set`read_config_from.kiro` to`false` to turn them off.
- In kitty and Ghostty, the prompt now shows a small thumbnail of each attached image.

#### Changed

- `devin ls` ,`devin -r` , and`/resume` now open a session monitor that lists sessions by last update with their status, directory, and message count.
- With the sandbox on, an enterprise domain allowlist is now a hard limit: the CLI blocks a domain outside the list instead of asking you to allow it.
- When you allow a cloud MCP tool that your organization set to “ask”, the tool now runs once with those arguments.
- Large responses now render faster, and cursor movement in a long prompt and prompt history search (Ctrl+R) respond faster.

#### Fixed

- A repo’s `AGENTS.md` ,`CLAUDE.md` , and other rules now load after a command enters the repo with`cd` .
- A malformed entry in an MCP config file or hooks file no longer hides the other servers and hooks in that file.
- The CLI no longer crashes when it runs a shell command while an environment variable is not valid UTF-8.

*Fan coverage from Devin Central — not an official Cognition announcement. Devin is a trademark of Cognition.*
