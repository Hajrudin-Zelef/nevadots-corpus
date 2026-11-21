---
id: collect-261121-veille-ia/langages-dev/discord-reveals-major-app-overhaul-that-could-make-it-faster-and-use-less-ram-dexerto
title: "Discord reveals major app overhaul that could make it faster and use less ram - Dexerto"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["discord", "client", "rust", "would", "desktop", "using", "could", "major"]
source: https://www.dexerto.com/entertainment/discord-reveals-major-app-overhaul-that-could-make-it-faster-and-use-less-ram-3425203/
source_anchor: ""
source_lines: [1, 35]
section: "Langages_Dev"
---
Discord is working on a major overhaul of its desktop and mobile apps, with plans to rewrite its client using Rust in an effort to improve performance and reduce resource usage.

The company has already been using Rust for parts of its infrastructure and voice technology for years, but the latest effort aims to bring the programming language to the entire client.

The changes could affect how Discord runs across multiple platforms, including Windows, macOS, and mobile devices, while potentially addressing concerns about the app’s memory consumption.

## Discord’s new app could use significantly less RAM

On November 5, Discord co-founder and CTO Stanislav Vishnevskiy revealed that the company is working toward moving its entire client to Rust, a programming language known for its performance and memory safety.

“We’ve been early adopters of Rust since 2018 and a lot of our infra, voice tech, and even some of the current client is already Rust,” he wrote on X.

“The holy grail was getting the whole client there.”

According to Vishnevskiy, the transition would also involve moving away from Electron, the framework currently used for Discord’s desktop application, as well as React Native on mobile.

Instead, the mobile app would retain its native user interface while sharing application code with the desktop version through Rust.

He also clarified that the new desktop client would not use Tauri, another framework commonly associated with Rust-based applications, but would instead rely on GPUI.

Alongside the announcement, Vishnevskiy shared a screenshot of Windows Task Manager showing an application labeled “Discord Phoenix” using approximately 290 MB of RAM.

The screenshot also showed Google Chrome using roughly 1.57 GB of memory, Spotify consuming 653 MB, and Steam Client WebHelper using 856 MB.

## Related

In a follow-up post, Discord revealed just how much faster the new app loads on desktop.

“On the left is the Discord you use daily. And on the right is a new version we’re working on, rebuilt in Rust,” they said.

While the image suggests the experimental Discord client can operate with a relatively small memory footprint, it does not establish how much RAM the current Discord app would use under identical conditions.

The rewrite is not Discord’s only effort to reduce its impact on PC performance. In September 2026, the company began testing a new Game Mode designed to automatically reduce animations and disable visual effects while gaming, lowering CPU and GPU usage.

Unlike Game Mode, which focuses on reducing resource consumption during gameplay, the move to Rust would change the underlying technology powering Discord itself. However, the company has yet to reveal when the rewritten app will become available to the public.
