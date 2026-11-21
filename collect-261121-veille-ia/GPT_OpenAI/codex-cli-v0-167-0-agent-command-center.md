---
id: collect-261121-veille-ia/gpt-openai/codex-cli-v0-167-0-agent-command-center
title: "Codex CLI v0.167.0: agent command center"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["codex", "command", "center", "chat", "agent", "vertex", "backend", "backends"]
source: https://www.turboai.dev/blog/codex-cli-v0-167-0
source_anchor: ""
source_lines: [1, 40]
section: "GPT_OpenAI"
---
# Codex CLI v0.167.0: agent command center, Vertex AI onboarding, backend switching#

Published by Alex Sorokoletov

Part of the Codex CLI Version Tracker series. | Codex on GitHub | Official Changelog


Codex CLI v0.167.0 follows v0.166.0 with an agent command center opened by `codex agents --no-daemon`, Google Cloud Vertex AI setup inside terminal onboarding, and switching between connected backends from the session picker without losing a draft. Fixes land for context compaction, network approval prompts, and realtime transcripts.<sup>[1]</sup>

## What shipped in the terminal#

| Change | What it does | 
|---|---|
| `codex agents --no-daemon` | Opens the agent command center; the command center and the session picker mark unread results. <sup>[1]</sup> | 
| Backend switching | Switch between connected backends from the terminal session picker and keep your drafts when switching. <sup>[1]</sup> | 
| Local image preview | Preview local image attachments while composing on supported terminals, and open viewed local images from clickable transcript paths. <sup>[1]</sup> | 
| Vertex AI onboarding | "Sign in another way" exposes API key and cloud provider options; `gcp_setup_wizard` sets up Google Cloud Vertex AI with Application Default Credentials or a credential file.<sup>[1]</sup> | 
| System proxy opt-in | Opt into macOS or Windows system proxy settings from `/experimental` .<sup>[1]</sup> | 
| Patch detail expansion | Expand patch details directly in the transcript while an approval prompt is open. <sup>[1]</sup> | 

## Fixes#

- Saved chats stay visible after you switch model providers. A chat that cannot resume stays readable, and you can prepare an editable prompt from it to continue in a new chat.<sup>[1]</sup>
- `Ctrl+L` clears the terminal display while keeping earlier messages available through`Ctrl+T` .<sup>[1]</sup>
- Deleting text that exposes a leading `!` keeps an ordinary draft in chat mode.<sup>[1]</sup>
- Network approval prompts close when their requests end, and concurrent requests to the same endpoint prompt one at a time.<sup>[1]</sup>
- Long realtime V3 conversations save completed speech to chat history as the transcript grows.<sup>[1]</sup>
- Automatic context compaction accounts for retained output from failed responses and from responses with missing token-usage data.<sup>[1]</sup>

## Sessions that survive a backend switch#

Several changes converge on one thing: what a session keeps when the backend or provider under it changes. You can switch between connected backends from the session picker and keep a draft you were typing, saved chats stay visible after a model-provider switch, and the resume picker keeps paginating across backends. A chat that cannot resume still hands you its transcript and an editable prompt to carry the work into a new chat.

Read next to the Vertex AI onboarding path and the macOS and Windows system-proxy opt-in, the release points Codex at workspaces that reach more than one backend and cloud provider at a time. Where moving between backends is routine, the work open at the moment of the move, the draft, the saved chat, the resume position, has to travel with it. The approval surface tightens on the same axis: network prompts clear when their requests finish, and requests to one endpoint queue to a single prompt at a time.

*All trademarks and software referenced belong to their respective owners.*

## Sources#

1. Codex CLI release notes, rust-v0.167.0
