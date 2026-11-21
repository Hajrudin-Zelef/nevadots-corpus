---
id: collect-261121-veille-ia/langages-dev/discord-is-ditching-electron-for-rust-with-early-windows-build-using-just-290mb-of-ram
title: "Discord Is Ditching Electron For Rust, With Early Windows Build Using Just 290MB Of RAM"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["discord", "electron", "application", "rust", "memory", "windows", "early", "just"]
source: https://wccftech.com/discord-is-ditching-electron-for-rust-with-early-windows-build-using-just-290mb-of-ram/
source_anchor: ""
source_lines: [1, 32]
section: "Langages_Dev"
---
**Discord** is preparing one of the most significant technical overhauls in its history, **abandoning the Chromium-based Electron framework in favor of a native application written in Rust**. An early **Windows 11** build reportedly uses just **290 MB of RAM**, a considerable improvement over the existing desktop client, which can sometimes consume several gigabytes.

The transition was confirmed on **X** by Discord co-founder and CTO **Stanislav Vishnevskiy**, who explained that the company has been using Rust since 2018 across its infrastructure, voice technology, and portions of the existing application.

"That means Electron goes away, and so does React Native, mobile keeps native UI but shares app code with desktop via Rust."


The new desktop application, internally named **Discord Phoenix**, uses **GPUI**, a **GPU-accelerated, Rust-based interface framework**, rather than **Tauri** or another webview-based alternative.

This distinction is important. Electron packages **Chromium** alongside desktop applications, introducing additional memory and processing overhead. GPUI allows Discord to render its interface natively, although certain embedded features, including YouTube content and Activities, will reportedly continue using system webviews.

According to Windows Latest, a screenshot shared by Vishnevskiy shows Discord Phoenix consuming just **290.2 MB of system memory**. Interestingly, the developer subsequently clarified that the application was running two Discord accounts simultaneously.

That's particularly encouraging considering the current Electron client can consume upwards of 1–2 GB under certain circumstances.

In December 2025, a Discord employee acknowledged on the **r/discordapp subreddit** that the application could exceed **4 GB of RAM**, prompting an experimental automatic restart mechanism. The company emphasized that normal memory consumption should remain below 1 GB and that the restart mechanism was a temporary measure, not a permanent solution.

**Discord had also fixed more than nine memory-related problems and achieved a 5% reduction in 95th-percentile memory consumption**.

More recently, its **September 2026** patch notes revealed that upgrading to **Electron 42** reduced median CPU usage by approximately **17%**, although median memory consumption actually increased by around **4%**.

The move to Rust could address these longstanding inefficiencies more fundamentally, particularly when memory prices remain exceptionally high. As Wccftech previously reported, **DRAM supply constraints could persist through 2027, making memory-efficient applications increasingly valuable**.

The announcement has been well received on **Reddit,** although some users have expressed concerns about compatibility with third-party modifications and whether the application will retain its improved memory footprint during prolonged use.

More importantly, **290 MB is an early snapshot, not a controlled benchmark against the existing client**, and it remains unclear how Discord Phoenix behaves during voice calls, video streaming, or extended gaming sessions.

Vishnevskiy expects the native client to perform better than its predecessor and hopes to begin user testing soon. However, **Discord has not announced a public release date**, meaning the existing Electron application will remain in use for the foreseeable future.

News Sources: *Stanislav Vishnevskiy (X), Windows Latest, and the r/pcmasterrace subreddit*

Follow us to get more of our coverage in Google: Wccftech on Google
