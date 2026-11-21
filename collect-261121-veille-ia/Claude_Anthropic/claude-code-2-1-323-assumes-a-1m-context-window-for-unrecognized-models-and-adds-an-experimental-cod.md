---
id: collect-261121-veille-ia/claude-anthropic/claude-code-2-1-323-assumes-a-1m-context-window-for-unrecognized-models-and-adds-an-experimental-cod
title: "Claude Code 2.1.323 assumes a 1M context window for unrecognized models and adds an experimental Code Mode · Dark Factory Dev"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-12"]
keywords: ["code", "claude", "mode", "notes", "context", "window", "used", "assumes"]
source: https://darkfactory.dev/news/2026-11-12-evening/story-1
source_anchor: ""
source_lines: [1, 15]
section: "Claude_Anthropic"
---
**Claude Code v2.1.323** · Anthropic · GitHub release notes, November 12, 2026

The release notes are long. Four groups of changes bear on anyone running Claude Code without a person at the keyboard.

The context window default changed. Claude Code used to assume 200K for a model ID it did not recognize and now assumes 1M. The notes say to use /autocompact to compact earlier, or to set CLAUDE_CODE_DISABLE_1M_CONTEXT=1 to keep 200K. They add a warning for sessions on other vendors' models with a smaller window: "once full, they can fail every turn until /clear".

Code Mode is new. The notes describe it in one line, as "an experimental opt-in in which Claude writes JavaScript to call its tools", and point to a documentation page for turning it on, or turning it off for an organization. That page returned "Page Not Found" when we checked on the evening of November 12, so the one line is all we have read about how it works.

Several settings now stop or ask where they used to proceed. If auto mode is requested and unavailable, a session used to start in a mode that could allow more; "It now starts in Manual mode, which asks first". If the settings that turn the sandbox on fail validation, claude -p and the Agent SDK used to run Bash unsandboxed; "Claude Code now stops at start-up and names the invalid setting". When a PreToolUse hook approves a tool call and the permission check then fails with an error, Claude Code decides the call as if no hook had answered. The tool that finishes an MCP server sign-in asks for confirmation even in bypass and auto modes.

Sessions can also say much more to each other. The number of messages one session can send to your other sessions on the same machine without a prompt from you went "from about 20 to 100,000". Separately, the v1 MCP client is removed; MCP_SDK_GENERATION=v1 now runs the v2 client in a compatibility mode.

We read the full notes. We did not install the release or test any of these behaviors, and the notes give no figures on how often the fixed cases occurred.

**Why it matters:** If you point Claude Code at another vendor's model through a gateway, the window it assumes just grew fivefold, and Anthropic's own notes say such a session can fail every turn once it fills. Set CLAUDE_CODE_DISABLE_1M_CONTEXT=1 on those runners before the update reaches them. A headless job with an invalid sandbox setting now stops at start-up where it used to run unsandboxed, so a scheduled run that fails after the upgrade may be reporting a setting that was already broken.
