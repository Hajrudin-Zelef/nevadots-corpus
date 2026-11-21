---
id: collect-261121-veille-ia/langages-dev/discord-cto-confirms-the-desktop-and-mobile-apps-are-being-rebuilt-in-rust
title: "Discord CTO confirms the desktop and mobile apps are being rebuilt in Rust"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["discord", "rust", "vishnevskiy", "mobile", "said", "desktop", "client", "native"]
source: https://www.invenglobal.com/articles/28182/discord-cto-confirms-the-desktop-and-mobile-apps-are-being-rebuilt-in-rust
source_anchor: ""
source_lines: [1, 22]
section: "Langages_Dev"
---
Discord is rebuilding its whole app in the Rust programming language, dropping Electron on desktop and React Native on mobile, according to Co-founder and Chief Technology Officer (CTO) Stanislav Vishnevskiy.


Vishnevskiy confirmed the plan on X (formerly Twitter) on November 4th, writing that "the holy grail was getting the whole client there." He said mobile will keep its native interface but share app code with desktop through Rust, and that Discord hopes to get the new client "into testing with users soon™." He gave no release date, and Discord has not published an announcement of its own. A day earlier, Vishnevskiy had told a user on X that the team was "busy with a much bigger update."


## **What the new client looks like**


Vishnevskiy said the new app is built with the GPUI framework, not Tauri, as some users had guessed. A Windows Task Manager screenshot he shared showed a process named 'Discord Phoenix' using 290.2 MB of memory. He later replied that the client in the image was running two accounts at once, which the new app supports.


Asked by a bot developer about how the app will render, Vishnevskiy said: "The app looks and works identically to the current one." He explained that the app itself is native and uses the system webview only for things like YouTube and Activities. Discord has its own patches on top of GPUI and will "see what we can upstream if relevant to the community," he added. Asked whether performance would improve, he replied: "We expect it to be better."


## **Discord's history with Rust**


Vishnevskiy said Discord has used Rust since 2018, and that much of its infrastructure, its voice technology and parts of the current client already run on it. In a 2020 engineering blog post, Discord explained how it moved its Read States service, which tracks the channels and messages each user has read, from Go to Rust to get rid of latency spikes. At the time, the company said it also used Rust for its game SDK and for video capture and encoding in Go Live.


On mobile, Vishnevskiy said the iOS app started out on React Native, while the Android app was moved over to it a couple of years ago because it kept lagging behind on features.
