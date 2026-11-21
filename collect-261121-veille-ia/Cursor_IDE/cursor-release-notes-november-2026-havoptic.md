---
id: collect-261121-veille-ia/cursor-ide/cursor-release-notes-november-2026-havoptic
title: "Cursor Release Notes — November 2026 | Havoptic"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: ["2026-11-05"]
keywords: ["cursor", "voice", "november", "chat", "agents", "agent", "v2026", "live"]
source: https://www.havoptic.com/releases/cursor/2026/11
source_anchor: ""
source_lines: [1, 24]
section: "Cursor_IDE"
---
# Cursor — November 2026 Releases

2 releases in November 2026.

## What's new in Cursor, November 2026

**Cursor adds live app building from cloud agents and realtime voice chat with agents**

So far in November 2026, Cursor has added the ability to build and publish live apps from a cloud agent, plus realtime voice chat with agents. Two releases have shipped, from v2026.11.05 to v2026.11.06, and the month is still in progress.

For live apps, you start a cloud agent and tell it you want to build an app. Cursor scaffolds the web app, spins up a Postgres database if needed, sets up authentication behind Cursor SSO, and provisions hosting. If the app needs to reach your systems, such as a data warehouse, Cursor asks for the secrets it needs. At publish time you pick a unique domain and get a live link. Apps can be deployed from new or existing repos.

Voice chat lets you talk to an agent in a realtime call in the Agents Window, the IDE, on web and in Cursor for iOS. It runs on Grok Voice, with several voices in Voice settings, and with Privacy Mode on it runs with zero data retention. Dictation is a separate feature.

- Live apps: a cloud agent builds the app, a Postgres database if needed, auth behind Cursor SSO and hosting, then returns a shareable link on a domain you pick (v2026.11.05)
- Cursor can deploy apps from new or existing repos and asks for secrets when an app needs to connect to systems like a data warehouse (v2026.11.05)
- Realtime voice chat with agents via the Start voice chat waveform button, in the Agents Window, IDE, web and iOS (v2026.11.06)
- Voice chat runs on Grok Voice with selectable voices; Privacy Mode on means zero data retention (v2026.11.06)
- On iOS, say "Hey Siri, call Cursor" to start a call with a new agent, and mute or hang up from the Lock Screen (v2026.11.06)

Upgrading from Agent SDK? See every change through Voice Chat with Agents.

- Voice Chat with Agents (November 6, 2026) — Talk to your coding agent out loud. Cursor 2026.11.06 adds realtime voice chat: describe changes, check progress, and hear updates while it works. Rea
- Build Live Apps in Cursor (November 5, 2026) — Stop stopping at localhost. Describe an app in Cursor and it builds, database, auth, and hosting included, then hands you a shareable link. Your whole
