---
id: collect-261121-veille-ia/langages-dev/discord-is-ditching-electron-and-its-new-rust-client-runs-two-accounts-in-just-290-mb-of-ram
title: "Discord Is Ditching Electron, And Its New Rust Client Runs Two Accounts in Just 290 MB of RAM"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["discord", "client", "vishnevskiy", "rust", "electron", "desktop", "accounts", "said"]
source: https://www.talkesport.com/tech/discord-rust-client-phoenix-electron-ram/
source_anchor: ""
source_lines: [1, 23]
section: "Langages_Dev"
---
Discord is finally getting rid of Electron, the web-based framework behind its desktop app for years. Co-founder and chief technology officer Stanislav Vishnevskiy has confirmed that the company is rebuilding its entire client in Rust. An early Windows build, codenamed Discord Phoenix, is already running at a fraction of the memory the current app uses. In a screenshot he shared on 4 November, Windows Task Manager showed Discord Phoenix at just 290.2 MB while it was signed in to two accounts at once.

For a desktop app that players have complained about for years, that is a dramatic change. Vishnevskiy wrote that Discord has been an early adopter of Rust since 2018, and that much of its infrastructure, voice technology and even parts of the current client already use it. “The holy grail was getting the whole client there,” he added. He confirmed that the new desktop interface is built with GPUI, the GPU-accelerated UI framework made by the team behind the Zed code editor, rather than with Tauri, as many people had guessed.

## What Discord Phoenix showed in Task Manager

The screenshot is more revealing than the headline number. Discord Phoenix appears as a single entry with no child processes, while the apps around it are split into several. Google Chrome sits at 1,568.6 MB across 19 processes, the Steam Client WebHelper at 856 MB across seven and Notion at 932.5 MB across nine, so the contrast with a native app is plain to see.

Vishnevskiy also used the replies to explain what users can expect. He said the new client “looks and works identically to the current one”. It also has full multi-account support on desktop and mobile, so you can even sit in voice channels on two accounts at the same time. When one user joked about the RAM figure, he pointed out that the build in the screenshot was running two accounts concurrently, a feature the new client supports.

Vishnevskiy has also made it clear that the change goes well beyond memory. A day after the screenshot, he posted a boot-time comparison filmed on two older laptops. “Clicking around feels instant, I can’t go back,” he wrote, in a post that has since passed 1.8 million views among PC players who keep Discord running next to their games.

## Why Electron has been Discord’s biggest headache

The current desktop app is built on Electron, which bundles a Chromium browser engine with every app and splits the work across several processes. That approach made it easier to ship one codebase on Windows, macOS and Linux, but it has also made Discord one of the heaviest apps many gamers run in the background. Last December, Discord confirmed it was testing an automatic restart for the Windows app whenever memory use passed 4GB, as long as the user had been idle for 30 minutes and was not in a call.

Discord described that restart test as temporary relief, and its later fixes only went so far. In September, the company said an upgrade to Electron 42 had cut median CPU use by about 17%, although median memory use rose by around 4% at the same time. Rewriting the client removes that trade-off at the source, which is why Vishnevskiy called it the holy grail rather than another patch.

The mobile apps are part of the same plan, too. Vishnevskiy said React Native is going away as well, with the mobile apps keeping their native interfaces while sharing app code with desktop through Rust. For the browser version, Discord is writing its own UI wrapper that can render either to HTML or to GPUI depending on the platform. Vishnevskiy said the team wants to make sure accessibility is handled properly.

Rust is not new territory for the company either. Discord has used it in production for years, and its engineers famously explained in 2020 why they moved a key backend service from Go to Rust to remove latency spikes. Some embedded content, such as YouTube videos and Activities, will still rely on web technology, but Vishnevskiy said the app itself is fully native. The timing matters for players as well as for Discord. Memory has become one of the most expensive parts of a gaming PC this year, and the AI-driven RAM crunch has made every gigabyte that a background app frees up more valuable. It also lands during a busy stretch for the company, which is making public server messages searchable on Google from December while its IPO plans remain stalled.

Discord has not given a release date for Discord Phoenix. Vishnevskiy said the company hopes to get the new client into testing with users “soon™”, and the screenshots so far come from a single early Windows build rather than from a public beta. Until testing opens, the current Electron app remains the only version available on Windows, macOS and Linux.
