---
id: collect-261121-veille-ia/langages-dev/discord-is-ditching-electron-for-rust
title: "Discord is Ditching Electron for Rust"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["discord", "rust", "electron", "current", "native", "ditching", "performance", "client"]
source: https://www.thurrott.com/cloud/343547/discord-is-ditching-electron-for-rust
source_anchor: ""
source_lines: [1, 9]
section: "Langages_Dev"
---
Discord is completely rewriting its app in Rust and will realize dramatic performance and security advances as a result.

“We’ve been early adopters of Rust since 2018, and a lot of our infrastructure, voice tech, and even some of the current client is already Rust,” Discord CTO Stanislav Vishnevskiy tweeted. “The holy grail [is] getting the whole client there. That means Electron goes away, and so does React Native. Mobile keeps [the] native UI, but shares app code with desktop via Rust. We hope to get it into testing with users soon.”

Discord is among my least-favorite apps, and I only use it because I have to for the Windows Weekly livestream. It installs an update almost every single time I launch it, and the performance and resource utilization are horrible.

I can’t see how Rust will solve the updating issue, but it will definitely fix the resource usage, and Vishnevskiy’s tweet includes a graphic showing the Rust-based Discord using just 290 MB of RAM. Running Discord now and waiting for an update to install–yes, really–Task Manager shows the current app consuming almost 1 GB of RAM when idle.

Vishnevskiy says this version of Discord is native on all desktop and mobile platforms and “looks and works identically to the current one.”
