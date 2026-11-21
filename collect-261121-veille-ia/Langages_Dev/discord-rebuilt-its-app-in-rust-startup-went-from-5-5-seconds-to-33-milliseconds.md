---
id: collect-261121-veille-ia/langages-dev/discord-rebuilt-its-app-in-rust-startup-went-from-5-5-seconds-to-33-milliseconds
title: "Discord Rebuilt Its App in Rust. Startup Went from 5.5 Seconds to 33 Milliseconds"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["discord", "rust", "rebuilt", "startup", "went", "seconds", "milliseconds", "video"]
source: https://medium.com/@highsongs2020/discord-rebuilt-its-app-in-rust-startup-went-from-5-5-seconds-to-33-milliseconds-b0b7fb13c395
source_anchor: ""
source_lines: [1, 13]
section: "Langages_Dev"
---
Member-only story

# Discord Rebuilt Its App in Rust. Startup Went from 5.5 Seconds to 33 Milliseconds

I use Discord every day, like millions of other people. I also know the small ritual of clicking the icon and waiting. Sometimes I check a message on my phone while the desktop app wakes up.

So when Discord published a video on its official YouTube channel called **“Rebuilding Discord in Rust, Project Phoenix,”** I watched it twice. The demo is hard to ignore, and the technical choices behind it are more surprising than the speed numbers.

In this article I’ll walk through what Discord showed, why the architecture matters, and what I think developers should take from it. I’m working from Discord’s public video and the CTO’s comments on X. I haven’t tested Phoenix myself, because it isn’t available to the public yet. I’ll point out where numbers come from Discord’s own demo and not from independent testing.

## The Demo: Two Apps, One Race

The video shows two apps side by side. On the left is the Discord you use today, built with Electron. On the right is **Discord Phoenix**, the new version written in…
