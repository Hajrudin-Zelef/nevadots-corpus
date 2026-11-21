---
id: collect-261121-veille-ia/claude-anthropic/claude-code-october-2026-features-and-changes
title: "Claude Code, October 2026: features and changes"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "code", "agent", "vscode", "model", "october", "default", "session"]
source: https://ai-news.cortexit.de/claude_code/2026-10
source_anchor: ""
source_lines: [1, 26]
section: "Claude_Anthropic"
---
Claude Code spent October on agent controls, VSCode improvements and guided setup. It also moved to a new default Haiku model. Releases ran from v2.1.288 to v2.1.304.

## Agents and automation

- **Guided auto-mode setup** : The`/auto-mode-setup` command (v2.1.304) scans your project and recent sessions to configure the environment automatically.
- **Agent view controls** : The`/effort` and`/fast` commands open a picker for default session settings, and`/model` opens it too. A new`n:` filter searches session names and tasks.
- **Sub-agent options** : The Agent tool gets an effort parameter, and`agent.spawn` creates teammate agents.`CLAUDE_CODE_WORKFLOW_SUBAGENT_MODEL` sets one model for all workflow agents.
- **Code Review depth** : Reviews support Quick, Standard and Deep levels, with defaults set per organization or repository. Admins can also enable automatic review of every repository in a GitHub organization.
- **You Should Know side agent** : A built-in side agent flags potential issues. It is available for first-party sessions with telemetry enabled.

## Models

- **Claude Haiku 5.5 default** : It is now the default Anthropic Haiku model, with a 1 million-token context window and updated pricing.

## VSCode and interface

- **Background work in VSCode** : You can run commands and sub-agents in the background while you continue other work.
- **LaTeX rendering** : VSCode responses now render LaTeX in`$$` , inline and bracket formats.
- **File row and plugin dialog** : VSCode lists files Claude sends in a chat row you can click to open them. The Manage plugins dialog can now review and run plugin marketplace commands.
- **Terminal input** : Vim mode adds a`%` motion that jumps to the matching bracket.

## Sessions and administration

- **Partial session names** :`claude attach` and`claude logs` accept partial session names. Sign-in approval also has a Deny button to cancel pending logins.

Some other features were released but are not listed here.
