---
id: collect-261121-veille-ia/gpt-openai/openai-codex-0-168-0-adds-model-provider-picker-and-side-chats
title: "OpenAI Codex 0.168.0 Adds Model Provider Picker and Side Chats"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-11-13"]
keywords: ["codex", "provider", "model", "chats", "providers", "features", "release", "terminal"]
source: https://draftship.dev/news/openai-codex-0-168-0-adds-model-provider-picker-and
source_anchor: ""
source_lines: [1, 41]
section: "GPT_OpenAI"
---
## Codex 0.168.0 lands with a provider picker

OpenAI Codex 0.168.0 shipped on 13 November, tagged rust-v0.168.0 and carrying 424 commits to main since the previous release. The release notes list a model provider browser, an optional side-conversation pane in the terminal interface, recording attachments for audio-capable models and animated GIF output. Most of the visible changes sit in the terminal UI, but the provider work also reaches the app-server documentation.

The full entry list is on the 0.168.0 release page.

## What the source confirms

- **Model providers.** You can browse providers under Sessions → Model providers and choose a default for new chats. Two fixes relate to this: open and resumed chats keep their saved provider after you change the default, and local history and resume pickers filter newly created threads by the signed-in ChatGPT account.
- **Side conversations.** Setting`features.tui_workspace` opens a side conversation beside the main chat. You drag the divider to resize the panes, or press leader z to zoom the focused pane.
- **Audio attachments.** With an audio-capable model you can attach recordings. Codex skips recordings that were confirmed rejected on later turns, and uses clip duration to keep more short recordings in context.
- **Animated GIFs.** Setting`features.image_generation_animation` adds generation or editing of animated GIFs, with optional frame count and timing controls.
- **Terminal details.** Fullscreen Codex shows image previews and pets in tmux on Ghostty and Kitty when`allow-passthrough all` is set.`tui.notification_method = "osc777"` turns on terminal notifications, and approval notifications now show longer command previews and separate terminal input from network access requests.

## Fixes that will be felt more than the features

A cluster of the bug fixes targets session continuity. Replies interrupted by tool calls now render as a single Markdown message, and reasoning summaries are restored when a conversation is replayed. Steering messages sent during the final transcript save, or after switching back to a long-running chat, now reach the running turn. Fuzzy file search weights filename matches more heavily so matching parent directories crowd them out less often. On Windows, idle local MCP servers no longer hold filesystem I/O away from the rest of Codex.

The app-server reference now covers listing model providers, creating custom gateways and updating gateway, Bedrock and GCP settings. Codex also warns when `features.multi_agent` is explicitly configured, pointing to `agents.enabled` for turning subagent tools off.

## A practical example the notes support

If you run several chats against different providers, the workflow the release describes is to set the default under Sessions → Model providers and leave existing chats alone: open and resumed conversations keep the provider they were saved with. The source does not describe a per-chat override command beyond that.

## Limits and open questions

The notes name `features.tui_workspace` and `features.image_generation_animation` as feature flags but do not say whether either is on by default, so treat both as opt-in until you check your own config. The source does not identify which models count as audio-capable, does not give the frame count or timing ranges for GIF output, and does not explain what "pets" refers to in the tmux rendering item. There is no statement about pricing, plan gating or a stable release channel, and no dates beyond 13 November.

## What this likely signals

*Inference, not confirmed by the source:* the provider browser plus app-server support for custom gateways and Bedrock or GCP settings suggests Codex is being set up for teams that route chats through their own endpoints rather than a single hosted default. The account-scoped history filtering points the same way, since it keeps separate ChatGPT accounts from sharing a thread list on one machine.

## For people building apps with AI

If you write code with an agent and then move to a builder, the model choice you just configured stops travelling with you, because hosted builders pick the model for you. Draftship is an AI app builder where you describe an app in plain language and it writes, builds and hosts it, with a built-in database and sign-in; paid plans add a choice of AI model, source access and custom domains. It is not affiliated with OpenAI, and nothing here suggests it supports the features in this release.

## Primary sources

- 0.168.0 · OpenAI Codex releases, Nov 13, 2026

This article was drafted with AI assistance from the primary sources listed below and checked automatically against them. An editor hasn't reviewed it yet.
