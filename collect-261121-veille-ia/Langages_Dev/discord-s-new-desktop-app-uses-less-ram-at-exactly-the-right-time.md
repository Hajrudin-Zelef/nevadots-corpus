---
id: collect-261121-veille-ia/langages-dev/discord-s-new-desktop-app-uses-less-ram-at-exactly-the-right-time
title: "Discord's new desktop app uses less RAM at exactly the right time"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["discord", "windows", "electron", "desktop", "less", "more", "apps", "microsoft"]
source: https://www.windowscentral.com/microsoft/microsoft-teams/discord-is-ditching-electron-and-the-result-is-a-faster-app-that-uses-less-ram
source_anchor: ""
source_lines: [1, 31]
section: "Langages_Dev"
---
The **Discord** app will soon use **less RAM and power**, all without losing a single feature. Discord is rebuilding its desktop app in Rust, **replacing the Electron-based client** it uses today. Discord has used Rust in parts of its technology stack for years, but the desktop application itself has historically been built on Electron.

**The new app is faster and more efficient**, as demonstrated by Discord CTO Stanislav Vishnevskiy. He showed the current Discord app and the new Rust client of Discord on a pair of Lenovo ThinkPads running Windows 11.

In that side-by-side comparison, the new Rust-based client launched more quickly than the current Electron version.

Electron apps often draw criticism because they use a lot of system memory. The apps rely on web technology, which means every single Electron app essentially ships with a mini browser. As a result, Electron apps are often more resource-heavy and less efficient than native desktop applications.

Some workloads can benefit from native code rather than web technologies, especially when performance and efficiency are priorities. That may make you question why Discord has had an Electron app for so long, but there are plus sides to the framework.

The biggest benefit of Electron is that it allows developers to **use the same codebase across major platforms**. Rather than maintaining separate applications for Windows, macOS, and Linux, developers can build a single application.

That advantage has kept Electron popular for years, powering everything from Discord and Slack to Microsoft Teams and Visual Studio Code. But some companies and developers have deemed the tradeoff too great and have migrated to other frameworks in pursuit of better performance and efficiency.

Discord's new app comes at a time when RAM is at a premium. Due to the ongoing RAM crisis, **many people cannot upgrade to PCs**. Even those looking to purchase new computers may have to pick models with only 8GB of RAM because of the increased cost of memory.

Microsoft is working to **optimize Windows 11 for systems with 8GB of RAM**. Alongside optimization efforts for Windows 11, Microsoft is reducing the memory needs of many of its apps.

Even with Windows 11 performing better on low-RAM devices, it's nice to see Discord ditch Electron and create a more efficient app. Discord has long been criticized for eating up memory and performing slowly. The new app should fix both those issues.

*Join us on* *Reddit at r/WindowsCentral* *to share your insights and discuss our latest news, reviews, and more.*

Sean Endicott is a News Writer at Windows Central, where he covers Windows 11, Surface hardware, Microsoft 365, AI, apps, and the broader PC ecosystem. Since joining the site in 2017, he has written well over a thousand articles across the Microsoft landscape, covering breaking news, analysis, and feature reporting.

He writes Windows Wrap, a weekly column covering the biggest stories in Windows and the PC industry, and what they mean for the platform going forward.

Before joining Windows Central full-time, Sean worked in journalism and media production after earning a First Class degree in Broadcast Journalism from Nottingham Trent University. Outside of tech, he is an award-winning American football coach based in Nottingham, England, and was named BAFCA Youth Coach of the Year in 2024.

You must confirm your public display name before commenting

Please logout and then login again, you will then be prompted to enter your display name.
