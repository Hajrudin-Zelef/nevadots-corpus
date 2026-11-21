---
id: collect-261121-veille-ia/windows-os/discord-is-killing-its-electron-app-that-can-eat-4gb-of-ram-on-windows-11-and-the-rust-rebuild-uses-
title: "Discord is killing its Electron app that can eat 4GB of RAM on Windows 11, and the Rust rebuild uses just 290MB"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-05"]
keywords: ["discord", "electron", "rust", "windows", "client", "using", "other", "vishnevskiy"]
source: https://www.windowslatest.com/2026/11/05/discord-is-killing-its-electron-app-that-can-eat-4gb-of-ram-on-windows-11-and-the-rust-rebuild-uses-just-290mb/
source_anchor: ""
source_lines: [1, 39]
section: "Windows_OS"
---
Discord is finally getting rid of Electron! Co-founder and CTO Stanislav Vishnevskiy has confirmed that Discord is rebuilding its client in Rust, and it’ll be using GPUI to handle the desktop interface.

Discord also shared an early Windows 11 screenshot of the new app written in Rust, listed as “Discord Phoenix,” and it’s using just 290.2MB of RAM:

I’ve been saying for a long time that one of Windows 11’s biggest problems is that it’s full of popular apps built using resource-intensive frameworks like Electron and WebView2 and other forms of web crap. That includes Outlook, Teams, WhatsApp, and Discord. Discord uses Electron, which I think is worse than the other web app approaches I listed.

For a long time, neither Microsoft nor other companies have done enough about it because it’s easier to package a website in Electron and ship it across different platforms.

## Discord can use gigabytes of RAM, but the new client looks promising

Unlike WebView, which at least shares resources with Microsoft Edge, every Electron app is packaged with its own copy of Chromium. You can verify it yourself by going to the AppData folder on Windows and navigating to the Discord directory. Here’s my AppData folder with the Discord package:

You can easily spot Chromium/Chrome and other files. You’re also going to find similar files inside the installation directories of Microsoft Edge, Brave, many other browsers, and apps built using Electron, such as Facebook’s Messenger.

Now, an Electron app won’t necessarily use several gigabytes of your RAM, but Discord is actually one of the worst offenders, and possibly one of the least optimized Electron apps. I always have Discord running in the background, and it’s constantly using 1–2GB of RAM, even when it’s idle.

Here’s how Discord runs when I open it:

You’ll notice that RAM usage increases as you open a server and keep using the app. Discord itself has acknowledged that its Windows app can reach 4GB of RAM, and it quietly rolled out a test that restarts the client when memory usage exceeds that point, which was a band-aid fix at best.

*“Yes, we’re testing a restart when memory exceeds 4GB (normal usage is <1GB),”* a Discord employee wrote in a Reddit post in December 2025, as spotted by Windows Latest.

*“Some users are seeing numbers that are at least lower than before. Overall, we’re seeing a 5% reduction in p95 memory so far, with more work to be done,”* the company noted last year. *“[We] uncovered issues upstream at the platform/OS/driver/hardware level, and we’ve worked closely with partners to provide stand-alone repros and ultimately fixes.”*

The new client looks promising, although we’ll have to test it ourselves to see how it compares.

## Discord is rebuilding its client in Rust

Over the past few days, Vishnevskiy has been dropping hints that a big update is coming to the desktop client and that it will not be based on Electron (Chromium).

In a post on X, as first spotted by Windows Latest, Vishnevskiy wrote:

*“We’ve been early adopters of Rust since 2018 and a lot of our infra, voice tech, and even some of the current client is already Rust, the holy grail was getting the whole client there,”* he said.*“That means Electron goes away, and so does React Native, mobile keeps native UI but shares app code with desktop via Rust.”*

*“And it’s not Tauri, it’s GPUI, so good guess everyone,”* Vishnevskiy added.

Tauri also supports Rust, but typically displays a web interface using the operating system’s webview. Choosing Rust alone would not tell us whether Discord was keeping a web interface, which is why the GPUI confirmation is useful.

Windows Latest understands that Discord will also no longer launch with the “Starting” screen, as it won’t have to load Electron libraries, which would make loading times faster as well.
