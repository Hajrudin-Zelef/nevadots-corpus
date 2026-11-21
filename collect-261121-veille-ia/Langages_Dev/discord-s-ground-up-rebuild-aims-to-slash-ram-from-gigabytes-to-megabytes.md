---
id: collect-261121-veille-ia/langages-dev/discord-s-ground-up-rebuild-aims-to-slash-ram-from-gigabytes-to-megabytes
title: "Discord's Ground-Up Rebuild Aims To Slash RAM From Gigabytes To Megabytes"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-04"]
keywords: ["discord", "native", "rust", "ground-up", "rebuild", "aims", "slash", "gigabytes"]
source: https://hothardware.com/news/discords-ground-up-rebuild-aims-to-slash-ram-from-gigabytes-to-megabytes
source_anchor: ""
source_lines: [1, 11]
section: "Langages_Dev"
---
# Discord's Ground-Up Rebuild Aims To Slash RAM From Gigabytes To Megabytes

As we've seen on the Windows side of things, there are major detriments to memory and CPU usage by using this approach instead of writing a fully native application. In fact, most of the performance gains we've been seeing in recent Windows updates can be attested to more parts of Windows switching from the also-web-based React over to the native WinUI framework, for example.

We've been early adopters of Rust since 2018 and a lot of our infra, voice tech, and even some of the current client is already Rust, the holy grail was getting the whole client there. That means Electron goes away, and so does React Native, mobile keeps native UI but shares app code with desktop via Rust. We hope to get it into testing with users soon™. (And it's not Tauri, it's GPUI, so good guess everyone.) Alright, back to staring at the WoW Forever login screen.

— Stanislav (@svishnevskiy) November 4, 2026

Another key benefit of the Discord Phoenix rewrite is that it is making use of the GPUI native user interface framework, which is designed to maximize speed and reduce memory overhead. It allows for direct GPU rendering, high-refresh rate responsiveness, and best of all, full code sharing across devices, as it's a fully cross-platform library, meaning that the move to native Rust doesn't leave desktop or mobile behind.

The Discord Phoenix Rust rewrite certainly looks promising; the huge cut to memory usage will be huge for low-end and mid-range PC gamers who need Discord for their team or party chat. Unfortunately, Discord hasn't revealed when this wondrous revamp of the ubiquitous chat app will make its way to gamers. Hopefully the answer is sooner than later.
