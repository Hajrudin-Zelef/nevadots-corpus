---
id: collect-261121-veille-ia/gpt-openai/codex-cli-0-1670-169-adds-side-chats-provider-controls-and-audio
title: "Codex CLI 0.167–0.169 adds side chats, provider controls, and audio"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["codex", "openai", "changelog", "adds", "november", "audio", "developers", "provider"]
source: https://authorityaitools.com/blog/openai-codex-cli-0-167-0-169-0-november-2026
source_anchor: ""
source_lines: [1, 54]
section: "GPT_OpenAI"
---
OpenAI shipped several Codex CLI releases from November 6 to 13, 2026. The updates span session navigation, model-provider setup, audio and image workflows, and the opt-in terminal workspace. The release notes are versioned, so check the relevant entry before relying on a feature in a specific installation. (https://developers.openai.com/codex/changelog)

## Codex CLI 0.167.0: command center and provider switching

Version **0.167.0**, dated November 6, adds an agent command center that can be opened with `codex agents --no-daemon`, with unread-result indicators in the command center and session picker. Users can switch between connected backends from the terminal session picker without losing drafts, preview local image attachments while composing on supported terminals, and expand patch details from an open approval prompt. The release also documents a Google Cloud Vertex AI setup wizard behind `gcp_setup_wizard`. (https://developers.openai.com/codex/changelog)

## Codex CLI 0.167.2: incremental Vertex AI tool updates

Version **0.167.2**, dated November 11, sends tool updates incrementally by default on Google Cloud Vertex AI. The changelog documents an opt-out through `incremental_tools_legacy_gcp = false` under `[features]` in `config.toml`. The intermediate **0.167.1** release, dated November 10, has no user-facing notes on the changelog page. (https://developers.openai.com/codex/changelog)

## Codex CLI 0.168.0: model providers, audio, and animated images

Version **0.168.0**, dated November 13, adds a model-provider browser under Sessions so users can choose a default provider for new chats. The opt-in `features.tui_workspace` setting opens a side conversation beside the main chat; users can resize or zoom the panes. The release also adds audio recordings as attachments for audio-capable models and an opt-in `features.image_generation_animation` setting for creating or editing animated GIFs with frame-count and timing controls. (https://developers.openai.com/codex/changelog)

## Codex CLI 0.169.0: searchable tasks and persistent side-chat tabs

Version **0.169.0**, also dated November 13, adds multiple side-chat tabs in the opt-in terminal workspace and allows saved local side chats to be reopened after a restart. Users can search older tasks by name, prompt preview, or working directory, fold completed turn details in the full-screen transcript, and add or edit custom model providers from the terminal. With the opt-in Cloud connection on macOS and Linux, the terminal can send images, choose a model and reasoning effort, and archive or restore Cloud conversations. (https://developers.openai.com/codex/changelog)

The release notes include additional bug fixes and documentation changes beyond these highlights. Feature gates and cloud-connection requirements vary by capability; consult the changelog and local Codex configuration before enabling them.

## Source

- OpenAI — Codex CLI changelog (November 6–13, 2026): https://developers.openai.com/codex/changelog
- OpenAI — Codex releases: https://github.com/openai/codex/releases

Free Resource

### 2026 AI Coding Tools Comparison Chart

Side-by-side comparison of features, pricing, and capabilities for every major AI coding tool.

No spam, unsubscribe anytime.

## Frequently Asked Questions

## What is Codex CLI 0.167–0.169 adds side chats, provider controls, and audio workflows?

## Related Articles

### ChatGPT Business plan usage expands to participating third-party apps

ChatGPT Business members can use eligible plan allowances in participating apps such as Notion, Warp, and OpenCode, subject to member approval and workspace controls.

Read more →Product Updates
### OpenAI launches Business Hub and Business Agents in ChatGPT

Businesses can claim profiles, provide product feeds, and review aggregated discovery insights in ChatGPT; eligible US ecommerce and online-service businesses can also launch a Business Agent.

Read more →Product Updates
### Claude adds interactive MCP Apps to connected tools

Anthropic's November 10 update lets supported connector apps open inside Claude, where users and Claude can work together in an embedded interface.

Read more →
