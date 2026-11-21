---
id: collect-261121-veille-ia/dev-langages/copilot-model-deprecations-fable-5-2-codex-exec-trust-rulefiles
title: "Copilot model deprecations, Fable 5.2, Codex exec trust | Rulefiles"
domain: dev-langages
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["github", "copilot", "source", "claude", "codex", "wording", "code", "model"]
source: https://rulefiles.dev/digest/2026-W46/
source_anchor: ""
source_lines: [1, 51]
section: "GPT_OpenAI"
---
# Copilot model deprecations, Fable 5.2, Codex exec trust

2026-W46, to . Checked against the sources on .

## In short

## Versions this week

## Claude Code 4

- 
Claude Codeversion 2.1.325 ### Claude Code v2.1.325Source wording: Fixed a security issue in how zstd-compressed responses are decoded Source: Claude Code releases
- 
Claude Codeversion 2.1.323 ### Claude Code v2.1.323Source wording: Added Code Mode, an experimental opt-in in which Claude writes JavaScript to call its tools; see https://code.claude.com/docs/en/code-mode to turn it on, or to turn it off for an organization Source: Claude Code releases
- 
Claude Codeversion 2.1.322 ### Claude Code v2.1.322Source wording: Added fast mode on Vertex in the global, us and eu regions, falling back to standard speed if Vertex refuses it Source: Claude Code releases
- 
Claude Codeversion 2.1.321 ### Claude Code v2.1.321Source wording: Added Claude Fable 5.2 ( `claude-fable-5-2` ), now the default Fable model — 1M context, $8/$40 per Mtok with $0.20/Mtok cache readsSource: Claude Code releases

## Codex 4

- 
Codexversion 0.168.0 ### Codex 0.168.0Source wording: Browse model providers under Sessions → Model providers and choose the default for new chats. Source: Codex release notes
- 
Codexversion 0.169.0 ### Codex 0.169.0Source wording: `codex exec` keeps project configuration and MCP servers disabled in untrusted directories, even with`--sandbox workspace-write` . Use`--trust` with`codex exec` or`codex review` to explicitly save project trust.Source: Codex release notes
- 
Codexversion 0.167.2 ### Codex 0.167.2Source wording: Codex now sends tool updates incrementally by default on Google Cloud Vertex AI. To opt out, set `incremental_tools_legacy_gcp = false` under`[features]` in`config.toml` .Source: Codex release notes
- 
Codex ### ChatGPT for iOS 1.2026.307Source wording: Paste images and files into the full-screen composer, and open file attachments from sent prompts. Source: Codex changelog

## GitHub Copilot 7

- 
GitHub Copilot ### Gemini 4 Hydrogen in GitHub CopilotSource wording: Gemini 4 Hydrogen, Google’s latest Flash model, is now generally available in GitHub Copilot. Source: GitHub changelog
- 
GitHub Copilot ### GitHub Enterprise Server support in the GitHub Copilot appSource wording: You can now bring your work on GitHub Enterprise Server (GHES) into the GitHub Copilot app. Source: GitHub changelog
- 
GitHub Copilot ### Upcoming deprecation of selected GitHub Copilot models in DecemberSource wording: We will deprecate the following models across all GitHub Copilot experiences (including Copilot Chat, inline edits, ask and agent modes, and code completions) on December 14, 2026 Source: GitHub changelog
- 
GitHub Copilot ### C++ language server now auto-configures MSBuild buildsSource wording: The Microsoft C++ Language Server for GitHub Copilot CLI now bundles its MSBuild compile commands extractor and invokes it automatically. Source: GitHub changelog
- 
GitHub Copilot ### Gemini 4 Carbon in GitHub CopilotSource wording: Gemini 4 Carbon, Google’s latest model, is now generally available in GitHub Copilot. Source: GitHub changelog
- 
GitHub Copilot ### GitHub App support for Enterprise Copilot seat managementSource wording: Enterprise GitHub Apps can now call the enterprise-level Copilot seat management APIs to control which users and enterprise teams are assigned Copilot seats. Source: GitHub changelog
- 
GitHub Copilot ### Claude Fable 5.2 in GitHub CopilotSource wording: Claude Fable 5.2, Anthropic’s newest Fable model, is now generally available in GitHub Copilot. Source: GitHub changelog

## Gemini CLI 1

- 
Gemini CLIversion 0.68.0 ### Gemini CLI v0.68.0Source wording: feat(cli): add extension validation command and evaluation tests Source: Gemini CLI releases
