---
id: collect-261121-veille-ia/langages-dev/discord-is-rebuilding-its-desktop-app-in-rust-with-the-new-client-using-290-mb-of-ram-ghacks-tech-ne
title: "Discord Is Rebuilding Its Desktop App in Rust, With the New Client Using 290 MB of RAM - gHacks Tech News"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-06"]
keywords: ["discord", "client", "rust", "desktop", "electron", "memory", "vishnevskiy", "windows"]
source: https://www.ghacks.net/2026/11/06/discord-is-rebuilding-its-desktop-app-in-rust-with-the-new-client-using-290-mb-of-ram/
source_anchor: ""
source_lines: [1, 43]
section: "Langages_Dev"
---
Discord is rebuilding its desktop app in Rust and replacing the Electron framework the current client is built on, according to Discord CTO Stanislav Vishnevskiy.

In posts on X on November 4 and 5, Vishnevskiy showed the new client running on Windows 11 with 290.2 MB of memory in use while active, and he said Discord wants to start testing it with users soon. No release date has been given.

The current Electron client regularly uses more than 500 MB of memory on Windows. Electron is built on Chromium, so every Electron app ships with what amounts to its own browser engine, and that overhead is what the move to Rust is meant to remove.

## How Discord's Rust Client Is Built

Discord has used Rust since 2018. Vishnevskiy says a large part of the company's infrastructure, its voice technology, and some of the current client already run on Rust, and that moving the whole client to Rust was "the holy grail." With the rebuild, Electron goes away, and so does React Native.

The change also affects mobile. Discord's mobile apps keep their native interface but will share app code with the desktop client through Rust.

Some developers expected Discord to use Tauri, an open-source framework for desktop and mobile apps. Vishnevskiy says the company is using GPUI, a framework written in Rust, with Discord's own patches applied on top.

Startup speed is the second claim. Vishnevskiy showed two Lenovo ThinkPads running Windows 11 side by side, one with the current client and one with the Rust version, and the new client opened noticeably faster. These figures come from Discord itself. Independent measurements are not available yet, since the client is not in public testing.

## Why Lower Memory Use Matters for Discord Users

The rebuild comes while memory prices remain high, and buyers of new PCs often end up with 8GB of RAM to keep costs down. Microsoft is working on the same problem from the operating system side and has published Windows 11 26H2 performance claims for 8 GB PCs.

Electron stayed popular because it lets developers keep one codebase for Windows, macOS, and Linux. Slack, Microsoft Teams, and Visual Studio Code are built on it as well. Discord now joins the developers who decided the efficiency cost was too high.

Users who want a baseline before the new client arrives can check how much memory Discord uses today:

1. Press Ctrl+Shift+Esc to open Task Manager.
2. Select the Processes tab and expand the Discord entry.
3. Note the combined value in the Memory column while Discord is active.

Discord has not announced when testing of the Rust client will begin, which platforms get it first, or whether early builds will be opt-in.

Moonchildsaid on
I, for one, welcome this change. The fewer instances of Blink/V8 I have to run at the same time on my workstation, the better.

Mystiquesaid on
Discord is a just terrible excuse for a product where information will inevitably be lost forever in worse ways than reddit where information is gatekept behind so many barriers that its impossible for the every day person to access not to mention the archival mess it is to follow up on.

Let’s also not forget how terrible their track record is for handing over data and also exposing its users voluntarily or otherwise.

Grahamsaid on
Trading one Google-sponsored system for another.

Dimitrisaid on
Welp another piece of trash.. Glad I got off the platform years ago.
