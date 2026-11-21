---
id: collect-261121-veille-ia/claude-anthropic/cursor-voice-chat-claude-code-2-1-319-codex-cli-0-167-a-origotech
title: "Cursor voice chat, Claude Code 2.1.319, Codex CLI 0.167 Â· OrigoTech"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-09"]
keywords: ["voice", "claude", "cursor", "code", "agent", "chat", "agents", "call"]
source: https://origotech.dev/articles/cursor-voice-chat-claude-code-2-1-319-codex-cli-0-167/
source_anchor: ""
source_lines: [1, 63]
section: "Claude_Anthropic"
---
Agents

# Cursor lets you talk to coding agents by voice, Claude Code 2.1.319 sends MCP tasks to the background, Codex CLI 0.167 opens an agent command center: the November 9 roundup

Cursor adds realtime voice calls with its agents (powered by Grok Voice), Claude Code 2.1.319 moves MCP tasks to the background and can deny instead of prompting, Codex CLI 0.167 adds an agent command center, and @GitHub is generally available in Slack and Teams.

Four updates shipped on November 6 for developers who run coding agents: Cursor turns the agent chat into a spoken conversation, Anthropic and OpenAI each publish a new release of their terminal agents, and GitHub makes its `@GitHub` agent generally available in Slack and Microsoft Teams.

## Cursor: realtime voice chat with agents

Cursor announced on November 6 that you can now **talk with your agents in a realtime voice call** (Cursor changelog; docs). Voice chat works in the **Agents Window, the IDE, on the web and in Cursor for iOS**. It is speech-to-speech: you talk to a voice agent, which replies out loud and **hands coding work to the agent in your chat**, then keeps the conversation going while that work runs.

During a call, the voice agent can send a task or follow-up to the coding agent, read diffs, runs and agents to say what changed (Cursor describes these lookups as **read-only**), search earlier messages and calls with that agent, and switch between workers in a Project. The coding agent writes short spoken progress notes and a brief answer when it is done; the call transcript appears in the chat.

- **Model** : voice chat runs on**Grok Voice** , which handles listening and speaking; the coding agent keeps the model you picked. Named voices include Ara, Eve, Leo, Rex and Sal.
- **Privacy** : with Privacy Mode on, voice chat runs with**zero data retention** , according to Cursor.
- **iOS** : "Hey Siri, call Cursor" starts a call with a new agent; mute and hang up from the Lock Screen through a Live Activity.
- **Limits** : a call hangs up after**about 10 minutes of silence** when the agent has no work in progress, and there is a**daily limit** on voice calls (no number published). Sessions authenticated with an API key cannot start voice chat; the button is hidden in subagents, Project workers, archived chats and Claude Code chats.

Voice chat is separate from dictation (formerly called Voice Mode), which still turns speech into text in the chat input.

## Claude Code v2.1.319: MCP tasks in the background, deny instead of prompting

Anthropic published **Claude Code v2.1.319** on November 6 (release notes). The changes most relevant to teams:

- **MCP tasks** : when an MCP server runs a tool call as a task, Claude Code now**moves it to the background right away** and tells Claude the result when it finishes.`CLAUDE_CODE_DISABLE_MCP_TASK_BACKGROUND` restores the old behaviour (waiting inside the tool call).
- **`permissions.denyInsteadOfPrompting`**: a tool call that would wait on a permission prompt is** denied and the turn continues**, in terminal sessions, subagents and teammates; useful for unattended runs.
- **Auto mode behind gateways** : with telemetry or feature flags off, including behind LLM gateways, calls the server cannot check are now asked about or denied,**with no fallback to the built-in classifier** ;`CLAUDE_CODE_AUTO_MODE_SERVER=0` keeps the old behaviour. Bedrock, Vertex and Foundry are unchanged.
- **New tools** :`/claude-api latency-optimize` looks for where your Claude API code makes people wait and tries faster settings against your eval;`CLAUDE_CODE_DISABLE_FEATURE_FLAGS` stops feature-flag fetching without turning off telemetry.
- **Claude apps gateway** : spend for today, this week and this month in`/usage` , rules to pick which pooled group budget pays first, an optional`login_notice` , JSON logs, and start-up warnings ahead of a Claude Desktop change dated November 10.
- **Fixes** : among many,`claude -p` now exits 1 instead of 0 when auto mode refused actions because no safety check was available.

## Codex CLI 0.167.0: an agent command center in the terminal

OpenAI released **Codex CLI 0.167.0** on November 6 (01:40 UTC, 02:40 Paris time) (release notes). New features listed by OpenAI:

- **Agent command center** : open it with`codex agents --no-daemon` ; the command center and the session picker now**mark unread results** .
- **Several backends** : switch between connected backends from the terminal session picker, keeping your drafts.
- **Images** : preview local image attachments while composing on supported terminals, and open viewed images from clickable transcript paths.
- **Sign-in** : API key and cloud provider options move under "Sign in another way"; the`gcp_setup_wizard` flag sets up**Google Cloud Vertex AI** with Application Default Credentials or a credential file.
- **Proxy and review** : opt into macOS or Windows system proxy settings from`/experimental` , and expand patch details in the transcript while an approval prompt is open.

On the fix side, saved chats stay visible after switching model providers, `Ctrl+L` clears the display while keeping earlier messages available through `Ctrl+T`, and concurrent network approval requests to the same endpoint now prompt one at a time. Pre-release builds of 0.169.0 (alpha) appeared on November 6 and 7; they are not a stable release.

## @GitHub generally available in Slack and Microsoft Teams

GitHub announced on November 6 that **`@GitHub` in Slack and Teams is generally available **(GitHub changelog). From a conversation, you can investigate bugs, create issues, implement changes and open pull requests. In Teams,** Canvas** shows implementation plans, code changes and previews next to the conversation. `@GitHub` now identifies the repository from your message and launches the session without asking you to pick it again, and issues and pull requests it creates say "Created from a Slack/Teams conversation with @username".

## What to keep in mind

- Cursor's zero data retention claim applies with Privacy Mode on; the daily call limit is not quantified.
- `permissions.denyInsteadOfPrompting` and the new auto mode behaviour change how unattended Claude Code runs end: check scripts that relied on the old fallback or on exit code 0.
- Codex CLI 0.167.0 is the stable release; 0.169.0 builds are still alpha.

## Sources

- Cursor, "Voice Chat with Agents", November 6, 2026, checked November 9, 2026
- Cursor docs, Voice chat, checked November 9, 2026
- Anthropic, Claude Code v2.1.319 release notes, November 6, 2026, checked November 9, 2026
- OpenAI, Codex CLI 0.167.0 release notes, November 6, 2026, checked November 9, 2026
- GitHub Changelog, "@GitHub in Slack and Teams is generally available and more", November 6, 2026, checked November 9, 2026

Spotted an error in this article? Report it Â· Our corrections policy
