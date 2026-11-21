---
id: collect-261121-veille-ia/langages-dev/discord-is-moving-completely-to-rust-daily-dev
title: "Discord is moving completely to Rust... | daily.dev"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["discord", "rust", "desktop", "current", "daily", "phoenix", "framework", "about"]
source: https://daily.dev/posts/discord-is-moving-completely-to-rust--hvu4eiaje
source_anchor: ""
source_lines: [1, 15]
section: "Langages_Dev"
---
Discord is rewriting its entire client in Rust under the codename Project Phoenix, replacing Electron on desktop and React Native on mobile. In a demo video, the current desktop app takes 5.5 seconds to start up versus 33 milliseconds for the Phoenix version. The mobile app drops from 4.2 seconds to half a second. Memory use falls from 1 to 2 GB down to 290 MB. The desktop UI layer uses GPUi, the GPU-accelerated Rust framework built by the Zed team, which Discord reportedly forked and patched. Discord's CTO says Rust has powered parts of its infrastructure since 2018. Phoenix will first ship to about 100 test users before a wider rollout.

Questions this post answers

## How much faster does Discord's new Rust-based desktop app start up compared to the current Electron version?

The Rust-based Project Phoenix desktop app starts in about 33 milliseconds, compared to roughly 5.5 seconds for the current Electron-based Discord client. On mobile, the rewritten app starts in about half a second versus 4.2 seconds for the current React Native app. Developers evaluating Electron alternatives can follow rewrite benchmarks like this on daily.dev.

## What GPU-accelerated UI framework is Discord using to rewrite its desktop app in Rust?

Discord chose GPUi, a native GPU-accelerated UI framework written in Rust and created by the Zed team for their Zed code editor, instead of more common alternatives like Iced or Slint, or the expected Tauri. Discord reportedly forked and patched GPUi for its own use, despite the project not having had a new release in about a year. Anyone weighing Rust UI frameworks for a desktop rewrite can track choices like this on daily.dev.

## How much memory does Discord's new Rust desktop client use compared to the current version?

The Project Phoenix desktop app uses about 290 megabytes of memory, compared to 1 to 2 gigabytes for the current Electron-based Discord desktop client. This reduction comes alongside the move from Electron to the GPUi Rust UI framework for the rewritten application. Teams weighing memory footprint when picking a desktop framework can track cases like this on daily.dev.
