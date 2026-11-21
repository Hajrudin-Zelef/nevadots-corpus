---
id: collect-261121-veille-ia/claude-anthropic/claude-code-ships-v2-1-323-stable-with-experimental-code-mode-and-a-breaking-1m-context-default-digi
title: "Claude Code Ships v2.1.323 Stable with Experimental Code Mode and a Breaking 1M Context Default | DigIntoAI"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-12"]
keywords: ["code", "claude", "mode", "experimental", "context", "settings", "official", "managed"]
source: https://digintoai.com/en/articles/claude-code-ships-v2-1-323-stable-with-experimental-code-mode-and-a-breaking-1m-3385f9ddb4a4
source_anchor: ""
source_lines: [1, 15]
section: "Claude_Anthropic"
---
Anthropic published Claude Code v2.1.323 (commit 89eb88e) as the latest stable release on GitHub and its official changelog (dated November 12, 2026 PT / morning of November 13 Beijing time), bundling 294 feature additions, behavioral changes, and security fixes.

Key Additions and Breaking Behavioral Changes

Experimental Code Mode (Opt-In): When enabled, Claude can write JavaScript directly to invoke and orchestrate its tools rather than relying purely on sequential JSON tool calls. Official docs say organizations can block Code Mode by setting CLAUDE_CODE_EXPERIMENTAL_CODE_MODE to 0 in managed settings, and cannot turn it on for users from managed settings.

Unrecognized Model IDs Now Default to 1M Context: Claude Code raised the assumed context window for unrecognized model identifiers—such as custom OpenRouter slugs, Bedrock ARNs, or third-party proxy endpoints—from 200K to 1M tokens. Anthropic explicitly warns that models with smaller actual windows will fail every turn once full until /clear is run, advising users to set CLAUDE_CODE_DISABLE_1M_CONTEXT=1 before filling the context.

Plugin Validation, Heartbeats & Sandbox Fixes: plugin.json now supports engines["claude-code"] version constraints; claude plugin validate type-checks TypeScript hook modules (failing under --strict); the Artifact tool accepts .docx uploads; and long-running hooks emit a 30-second progress heartbeat in claude -p and SDK runs so hosts like Claude Desktop do not time out. Security patches also close permission bypasses on rm path checks, unspaced wildcards like Bash(ls*), and Write/Edit calls targeting Read-denied paths.

Verification Notes & What This Is Not

The official Code Mode docs page is now live: the setup URL cited in the release notes, https://code.claude.com/docs/en/code-mode, returns HTTP 200. Official developer account @ClaudeDevs posted at 05:18:55 Asia/Shanghai on November 18, 2026 (2026-11-17T21:18:55Z) describing Code Mode as “a new, experimental way for Claude Code to work with tools,” and said to opt in with CLAUDE_CODE_EXPERIMENTAL_CODE_MODE=1. The official docs state that Code Mode is experimental and off by default, and may change or be removed in any release. It requires Claude Code v2.1.323 or later; using it with agent teams requires v2.1.326 or later. It works with a Claude subscription or Console account, and cannot be used through a gateway, Amazon Bedrock, Claude Platform on AWS, Google Cloud’s Agent Platform, Microsoft Foundry, or Claude Models on Google Cloud (Anthropic managed). Organizations that use the HIPAA configuration or zero data retention cannot use it. Supported models are Claude Opus 5.5, Sonnet 5.5, Haiku 5.5, or Fable 5.2. It works only in an interactive claude session in a terminal, and does not work with -p, the Agent SDK, the VS Code panel, the Desktop app, or cloud sessions. The environment variable may also be set in the env block of ~/.claude/settings.json; Claude Code ignores this variable in a project’s settings files. Organizations can block it by setting the variable to 0 in managed settings, and cannot turn it on for users from managed settings.

Client CLI and IDE release only: This update modifies the Claude Code CLI, VS Code extension, and desktop runtime—not a new Claude model or API pricing tier.
