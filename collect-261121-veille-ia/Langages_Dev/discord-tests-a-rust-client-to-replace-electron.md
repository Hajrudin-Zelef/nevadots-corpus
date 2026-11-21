---
id: collect-261121-veille-ia/langages-dev/discord-tests-a-rust-client-to-replace-electron
title: "Discord Tests a Rust Client to Replace Electron"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["client", "discord", "electron", "rust", "desktop", "windows", "current", "tauri"]
source: https://hwbusters.com/news/discord-is-dropping-electron-for-a-rust-client-and-an-early-build-idles-at-290mb-of-ram/
source_anchor: ""
source_lines: [1, 24]
section: "Langages_Dev"
---
**Discord is rebuilding its desktop and mobile apps in Rust, dropping Electron and React Native along the way. An early Windows 11 build, nicknamed “Phoenix,” was shown idling at about 290MB of RAM, against the gigabyte or more the current client routinely holds.**


Anyone who keeps Discord open next to a game knows the feeling: the chat app quietly grows until it is one of the heaviest things in Task Manager. That may finally be about to change. Discord co-founder and CTO Stanislav Vishnevskiy confirmed on X on November 4 that the company is rewriting its client in Rust, and that the new desktop app will not be built on Electron.

“We’ve been early adopters of Rust since 2018 and a lot of our infra, voice tech, and even some of the current client is already Rust, the holy grail was getting the whole client there,” he wrote. “That means Electron goes away, and so does React Native.” Mobile keeps a native interface but will share its app code with desktop through Rust.

## Not Tauri, but GPUI

The interesting part is the UI layer. When people heard “Rust,” most assumed Tauri, the popular Electron alternative. Vishnevskiy shut that down directly: “it’s not Tauri, it’s GPUI.” That distinction matters. Tauri still renders a web interface, just through the operating system’s own webview instead of a bundled copy of Chromium. GPUI is the GPU-accelerated UI framework that Zed Industries built for its Zed code editor, and it draws the interface natively. In other words, Discord is not just swapping one browser engine for a lighter one; it is getting rid of the browser entirely.

That is a bigger engineering bet than it sounds. Discord’s client is enormous, with voice, video, screen sharing, overlays, rich embeds and in-app activities that all lean on web technology today. Rebuilding that natively is the kind of project most companies talk about for years and never ship.

## What the numbers say, and what they don’t

Discord shared a screenshot of the new Windows 11 client, labelled “Discord Phoenix,” showing 290.2MB of memory use. Windows Latest, which spotted the posts, says its own copy of the current Electron client sits at 1-2GB even when idle. Discord itself has admitted the problem: late last year it began testing an automatic client restart once memory use passed 4GB, noting that normal usage was under 1GB.

Take the 290MB with a pinch of salt for now. It is a single screenshot of an early build, almost certainly with fewer features wired up and no way to know which servers or channels were loaded. Electron apps also tend to look fine at launch and balloon over hours of use, so the real test is a long session with voice and screen sharing running. Still, even if the final number lands at two or three times that, it would be a meaningful cut on a system with 16GB of RAM, where every gigabyte freed up goes back to the game. One small extra: Windows Latest reports the new client should skip the familiar “Starting” splash, since it no longer has Electron libraries to load.

## No date yet

There is no release window. According to Inven Global, Vishnevskiy said the team hopes to get the new client “into testing with users soon,” and Discord has not yet published a formal announcement. Expect a staged test before anything replaces the current app, and expect third-party client mods that hook into Electron to break when it does.

If Discord pulls this off, it will also be the most visible proof yet that a big consumer app can walk away from Electron without losing features. Teams, Slack and WhatsApp’s desktop clients have all taken criticism for the same memory habits. A lean Discord would make their excuses harder to sustain.
