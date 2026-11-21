---
id: collect-261121-veille-ia/langages-dev/discord-is-rebuilding-its-app-in-rust-early-build-uses-just-290mb-of-ram-shane-the-gamer
title: "Discord Is Rebuilding Its App in Rust, Early Build Uses Just 290MB of RAM - Shane the Gamer"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-05"]
keywords: ["rust", "discord", "vishnevskiy", "build", "native", "early", "client", "electron"]
source: https://www.shanethegamer.com/news/discord-rebuilding-app-in-rust-290mb-ram/
source_anchor: ""
source_lines: [1, 47]
section: "Langages_Dev"
---
# Discord Is Rebuilding Its App in Rust, Early Build Uses Just 290MB of RAM

Discord is rebuilding its app in Rust, and the company has now shown the new client running next to the current one in an official post on X on 5 November. Co-founder and chief technology officer Stanislav Vishnevskiy, who confirmed the rebuild a day earlier, said lower memory use is “just part of the story”, pointing to the new app’s boot time and saying that clicking around it “feels instant”.

On the left is the Discord you use daily. And on the right is a new version we’re working on, rebuilt in Rust

— Discord (@discord) November 5, 2026

The rebuild replaces Electron, the web-based framework behind the current desktop client, and an early Windows 11 build Vishnevskiy shared, listed as “Discord Phoenix” in Task Manager, was using 290.2MB of RAM while signed in to two accounts at once. Mobile is included too, with React Native being dropped on phones. There’s no release date yet. Discord calls it a new version it’s working on, and Vishnevskiy said the company hopes to get it into testing with users “soon™”.

## What Moving From Electron to Rust Means

Electron lets a company build a desktop app out of the same web code it uses for a website. The catch is that every Electron app ships with its own copy of Chromium, the engine behind Google Chrome, so running Discord on your PC effectively means running a stripped-down browser in the background. Rust is a programming language that compiles straight into native code for the machine it runs on, with no browser layer underneath.

Discord isn’t new to Rust. Vishnevskiy said the company has used it since 2018, and that much of its infrastructure, its voice tech, and parts of the current client already run on it. In a 2020 engineering blog post, Discord explained how it moved Read States, the service that tracks which channels and messages each user has read, from Go to Rust to get rid of latency spikes. At the time it also used Rust for its game SDK and for video capture and encoding in Go Live, its screen-sharing feature.

We’ve been early adopters of Rust since 2018 and a lot of our infra, voice tech, and even some of the current client is already Rust, the holy grail was getting the whole client there. That means Electron goes away, and so does React Native, mobile keeps native UI but shares app code with desktop via Rust. We hope to get it into testing with users soon™. (And it’s not Tauri, it’s GPUI, so good guess everyone.) Alright, back to staring at the WoW Forever login screen.

— Stanislav (@svishnevskiy) November 4, 2026

The rest of the client was the hard part. Vishnevskiy called getting all of it onto Rust “the holy grail”, and the mobile history shows why. The iOS app started on React Native, while the Android app moved over to it a couple of years ago because it kept falling behind on features. Under the rebuild, the mobile apps keep their native interface but share their underlying code with desktop through Rust. One shared codebase should let Discord build a feature once and ship it everywhere, which is the same problem React Native was brought in to solve on Android.

## GPUI Instead of Tauri, and Why It Matters

Vishnevskiy had been hinting at the change for days. On 3 November he told a user on X that the team was busy with a much bigger update, and when another user joked about Discord running two copies of Chromium, he replied “Or zero.” Most guesses then landed on Tauri, a popular Rust-based alternative to Electron. Vishnevskiy shut that down: “it’s not Tauri, it’s GPUI, so good guess everyone.”

The distinction matters because Tauri apps are written in Rust but usually still display a web interface through the operating system’s built-in webview. Choosing Rust alone wouldn’t have told anyone whether Discord was keeping its web-based UI. GPUI, the interface framework built by the team behind the Zed code editor, draws the interface natively instead. Vishnevskiy said the new app itself is native and uses the system webview only for embedded content such as YouTube videos and Activities, the in-app games like Wordle and Putt Party.

Discord has added its own patches on top of GPUI, and Vishnevskiy said the company will see what it can contribute back to the framework where it’s relevant to the wider community.

*Read More: Discord Update Adds New Features to Desktop and Mobile | How Much RAM You Need for PC Gaming in 2026*

For users, the switch shouldn’t be visible on screen. Asked by a bot developer how buttons, menus and other message components would render in the new app, Vishnevskiy said “The app looks and works identically to the current one.” Asked whether performance would improve, he replied “We expect it to be better.”

## Discord Rust Client Speed and RAM Usage Compared to the Current App

Discord’s official comparison, posted on 5 November, puts the app people use daily on the left and the Rust rebuild on the right. Vishnevskiy framed it around speed rather than memory, saying the lower RAM use is “just part of the story” and pointing viewers to the boot time. That lines up with what Windows Latest understands about launch: the new client shouldn’t show the “Starting” screen, because it no longer has to load Electron’s libraries first. Discord hasn’t confirmed that detail.

Discord is one of the heavier Electron apps on Windows. Mayank Parmar of Windows Latest wrote that Discord sits at 1 to 2GB of RAM on his PC even when idle, and climbs further as he opens servers. Discord has acknowledged the problem itself. In a Reddit post in December 2025, a Discord employee confirmed the company was testing an automatic restart of the client once memory use passes 4GB, adding that normal usage is under 1GB. Discord said at the time it had cut its p95 memory figure, the level that 95% of sessions stay under, by 5%, with more work to do.

Measured against those numbers, the early build is light. 290.2MB is about 7% of the 4GB restart threshold and under a third of the 1GB Discord calls normal, and the screenshot showed it running two accounts at the same time, which the new app supports. The restart test and the 5% cut tried to trim Chromium’s footprint; the rebuild takes Chromium out of the main app altogether, which is the most likely source of the saving.

Both the speed demo and the 290MB screenshot come from Discord itself, though, and nobody outside the company has tested the new client yet. Windows Latest says it plans to run its own comparison. We’d treat them as a promising starting point until a public build holds up with busy servers open, a voice call running and a game in the background.

Discord also hasn’t said whether the browser version changes with the rebuild, or what happens to third-party client mods built for the Electron app.

## Join the Conversation
