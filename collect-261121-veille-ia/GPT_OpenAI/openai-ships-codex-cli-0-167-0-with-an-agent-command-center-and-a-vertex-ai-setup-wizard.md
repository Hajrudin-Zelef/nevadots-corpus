---
id: collect-261121-veille-ia/gpt-openai/openai-ships-codex-cli-0-167-0-with-an-agent-command-center-and-a-vertex-ai-setup-wizard
title: "OpenAI Ships Codex CLI 0.167.0 With an Agent Command Center and a Vertex AI Setup Wizard"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-11-06"]
keywords: ["command", "agent", "center", "session", "setup", "codex", "wizard", "vertex"]
source: https://dev.to/delimitter/openai-ships-codex-cli-01670-with-an-agent-command-center-and-a-vertex-ai-setup-wizard-4ohb
source_anchor: ""
source_lines: [1, 21]
section: "GPT_OpenAI"
---
📅 November 6, 2026 • GitHub

OpenAI published a stable release of Codex CLI, version **0.167.0**, with release notes on GitHub dated November 6. The stable tag now tops the repo release list, ahead of the **0.168.0-alpha.7** pre-release. For anyone pinning versions in CI or across team machines the distinction matters, because alpha tags are pre-releases and are not intended as a default install target. The **0.167.0** line is the one meant for everyday terminal work.

The headline feature is a new agent command center. The command **codex agents --no-daemon** opens it without spawning a background daemon, keeping the workflow visible and easy to inspect. Both the command center and the session picker now mark unread results, so an engineer supervising several parallel coding agents can see at a glance which session produced new output since the last check. That turns the terminal into something closer to a cockpit for background work, where the state of every running agent is visible in one place.

The unread markers are a small change with a large effect on daily workflow. Parallel agent runs tend to produce long stretches of silence followed by bursts of output, and until now the only way to check progress was to open each session in turn. With results marked as unread, the command center works like an inbox for agent activity, and the session picker applies the same convention, so triage happens before attention is committed to any single task.

Backend switching also moved forward, and multi-cloud setup is now built in. Developers can change between connected backends directly from the terminal session picker while keeping their drafts intact. A **gcp_setup_wizard** configuration block sets up Google Cloud Vertex AI using either Application Default Credentials or a credential file, giving teams on Google infrastructure a guided path instead of hand-editing authentication settings.

Smaller additions include previewing local image attachments while composing on supported terminals, expanding patch details inline during approval prompts, and opting into macOS and Windows system proxy settings. On the fix side, saved chats stay visible after switching model providers, network approval prompts close when their requests end, and context compaction now accounts for retained output from failed responses, which protects context quality after an error.

**IMPACT**

For engineers running several agents at once, the command center makes unread results visible without opening each session. The Vertex AI wizard reduces GCP setup to one config block. Pin **0.167.0** for stable work and treat the 0.168.0-alpha.7 tag as a pre-release rather than a default.

**Sources:** GitHub

**Continue the conversation:** https://t.me/AIAutomationArchitecture · Bluesky

## Top comments (0)
