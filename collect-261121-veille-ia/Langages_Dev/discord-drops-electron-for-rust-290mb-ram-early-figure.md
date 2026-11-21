---
id: collect-261121-veille-ia/langages-dev/discord-drops-electron-for-rust-290mb-ram-early-figure
title: "Discord Drops Electron for Rust: 290MB RAM Early Figure"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-05"]
keywords: ["discord", "rust", "electron", "native", "gpui", "early", "desktop", "client"]
source: https://tech-insider.org/discord-electron-rust-gpui-rebuild-290mb-ram-2026/
source_anchor: ""
source_lines: [1, 101]
section: "Langages_Dev"
---
Discord is dropping Electron and rebuilding its apps fully in Rust, and an early Windows build shown in a screenshot used 290.2MB of RAM. The Verge reported on November 5, 2026 that co-founder and CTO Stanislav Vishnevskiy confirmed the plan on X. The 290.2MB reading is a single early snapshot with no stated workload, there is no release date, and the app people use today still runs on Electron.

## What Discord Confirmed About Leaving Electron

Discord’s desktop client currently runs on Electron, and Discord’s own September 8, 2026 patch notes say the team upgraded the desktop app to Electron 42. Two months later, Vishnevskiy told the public that Electron goes away. The replacement is a client written in Rust, with GPUI drawing the desktop interface. GPUI is the Rust UI framework that grew out of the team behind the Zed editor, according to reports.

Vishnevskiy framed the move as the end of a long road: “We’ve been early adopters of Rust since 2018 and a lot of our infra, voice tech, and even some of the current client is already Rust, the holy grail was getting the whole client there.” On scope, the same post said: “That means Electron goes away, and so does React Native, mobile keeps native UI but shares app code with desktop via Rust.” Discord hopes to get the new app into testing with users soon, but no outlet has a date.

Mobile is part of the plan. React Native goes away alongside Electron, phones keep native interfaces, and desktop and mobile share application code through Rust. One core codebase instead of separate stacks is the real prize behind the memory headline.

### Timeline from patch notes to a screenshot

| Date | Event | Source | 
|---|---|---|
| December 2025 | A Discord employee says the team is testing a client restart when memory passes 4GB | Reddit post, relayed by Windows Latest and 9to5Windows | 
| September 8, 2026 | Desktop app upgraded to Electron 42, mobile React Native moved from .81 to .86 | Discord patch notes | 
| November 4, 2026 | Vishnevskiy confirms a Rust rebuild on X and says GPUI, not Tauri | X post, cited by tbreak and The Verge | 
| November 5, 2026 | The Verge publishes the story with the 290MB screenshot | The Verge | 
| November 5, 2026 | Windows Latest reports the 290.2MB reading for a build labelled “Discord Phoenix” | Windows Latest, via 9to5Windows and tbreak | 

The September entry is the awkward one. Discord’s patch notes said the React Native upgrade improved average CPU usage on Android and iOS by about 5 percent, and that average app launch times improved by a similar amount. Eight weeks later React Native leaves the product. Teams upgrade dependencies they plan to keep, so either the rewrite accelerated recently or the upgrade was a stopgap while the Rust work matured. Discord has not said which.

## Why GPUI and Not Tauri Is the Detail to Watch

Before the confirmation, replies under Vishnevskiy’s posts guessed Tauri, the Rust-based framework that wraps a web interface in the operating system’s own webview. A joke about Discord running two copies of Chromium drew the reply “Or zero.”, per tbreak‘s account of VGTimes’ reporting. The CTO’s follow-up, as reproduced by Windows Latest, closed the question: “And it’s not Tauri, it’s GPUI, so good guess everyone.”

The distinction is not cosmetic. 9to5Windows explained that choosing Rust alone would not have said whether Discord was keeping a web-based interface. Tauri typically renders a web UI through the system webview, while GPUI is a native GUI toolkit that draws the interface itself. Confirming GPUI means the main window is being built without a browser engine underneath it.

### Where the web still lives

tbreak, citing VGTimes, noted that Discord’s embedded Activities, such as Wordle and Putt Party, run in web frames today and may remain an exception. Discord has not confirmed that. If it holds, the new client would still call a webview for embedded content, just not for the chat window, server list and voice controls that stay open all day.

### Electron vs Tauri vs GPUI vs native toolkits

The table is a qualitative comparison built from the descriptions above. It makes no benchmark claims.

| Approach | How the UI is drawn | Browser engine | Main trade-off | 
|---|---|---|---|
| Electron | Web page in a bundled Chromium window | Ships its own copy per app | Fastest path to cross-platform, highest baseline memory | 
| Tauri | Web UI in the OS webview, Rust backend | Uses the system webview | Smaller bundles, still a web interface | 
| GPUI | Native Rust toolkit draws the interface itself | None for the main UI | Younger ecosystem, full control over rendering | 
| Fully native (per platform) | OS toolkits such as those on Windows, macOS, Linux | None | Best fit per platform, separate code per platform | 

GPUI sits in an interesting middle. It avoids the three-codebase cost of per-platform native work and the bundled browser of Electron. The price is a young ecosystem with fewer ready-made accessibility, text input and internationalization components. Discord has the engineering depth to build what is missing, and most teams do not. That is the test to apply before copying the move: if you cannot staff the missing pieces, Tauri’s smaller step may be the realistic one.

## The 290.2MB Screenshot: What It Does and Does Not Prove

The number driving the headlines comes from one image. Vishnevskiy shared a screenshot of an early Windows 11 build, and Windows Latest read the Task Manager entry as 290.2MB of RAM for a client labelled “Discord Phoenix”. The Verge described it as a clear improvement and noted that the current Windows app can easily reach 1GB or more once you join a few servers.

tbreak was direct about the limits: the reading is one early screenshot with no stated workload, and Windows Latest said it still has to test the new client itself. We do not know how many servers were joined, how long the session ran, whether voice or video was active, or whether the figure covers one process or the full process tree.

| Claim | Status | Basis | 
|---|---|---|
| Discord is rebuilding its client in Rust | Confirmed | Vishnevskiy on X, reported by The Verge and tbreak | 
| The desktop interface uses GPUI | Confirmed | Vishnevskiy on X | 
| An early Windows 11 build showed 290.2MB of RAM | Reported | Screenshot shared by Vishnevskiy, read by Windows Latest | 
| The new app is generally 290MB versus 4GB | Unconfirmed | One screenshot, no workload stated | 
| The current Windows app can reach 4GB | Acknowledged by Discord | Reddit post cited by Windows Latest and 9to5Windows | 
| Discord has fully dropped Electron | Not yet true | Replacement is still in development | 
| A public release date or test channel exists | No | None announced | 

The headline comparison of 290MB against 4GB sets a fresh early build against the worst case of the old app. The fairer comparison is the one The Verge made: the new reading against the 1GB or more users commonly see after joining a few servers. Even that is a rough guide until someone measures both clients under the same load.

## How Heavy the Current Electron App Is

Every Electron app ships its own copy of Chromium plus a Node.js runtime. 9to5Windows put it plainly: unlike Microsoft’s WebView2, which shares resources with Edge, each Electron app carries its own browser engine, which is why Discord’s AppData folder is packed with Chromium files. Windows Latest’s author said Discord sits at 1 to 2GB of RAM on his PC even when idle. Discord’s own account differs on the baseline. In a December 2025 Reddit post relayed by Windows Latest, a Discord employee wrote, “Yes, we’re testing a restart when memory exceeds 4GB (normal usage is <1GB).” Both can be true, since a median session and a long-running heavy session give different readings.

### What Discord tried before rewriting

The same thread reported a 5 percent drop in p95 memory. The company’s wording, as quoted by Windows Latest, was: “Some users are seeing numbers that are at least lower than before. Overall, we’re seeing a 5% reduction in p95 memory so far, with more work to be done.” Per 9to5Windows, Discord added that it had uncovered issues upstream at the platform, OS, driver and hardware level and worked with partners on fixes.

PC Gamer wrote about a resource-light Game Mode for Discord and noted that the announcement said nothing about reducing memory usage. CPU and GPU relief helps a game in the foreground but leaves the RAM question untouched.

Read together, these steps look like a team squeezing a framework with a hard floor. A restart at 4GB treats the symptom, and a 5 percent trim at the 95th percentile is small next to a client that starts several hundred megabytes lighter. Removing the Chromium layer removes the thing that was being restarted, which is the case for the rewrite.

## Why Electron Won and What Changes for Desktop Developers

Electron won because it solved a hiring and shipping problem. A team that knew HTML, CSS and JavaScript could ship a Windows, macOS and Linux app from one codebase and reuse its web product. 9to5Windows called the Rust rebuild a signal of where desktop software is heading and argued that the memory and CPU costs of Electron have become harder to justify. That is the outlet’s judgment.

Nothing here breaks Electron overnight. What changes is the argument: “Discord uses it” was a shorthand defense of Electron at scale, and that defense loses its best example over the long term. Teams weighing a new cross-platform build now have a case study in the other direction, but Discord is leaving after years of owning an Electron product, with a large engineering team and in-house Rust expertise. A small team should not read this as a reason to rewrite. A sensible rule is to measure your own app’s memory and user complaints first, and move only if the numbers justify the cost.

A big name adopting GPUI also gives the framework visibility, which tends to bring more contributors, more libraries and more scrutiny. More on how these tools fit together is in our software coverage.

## What It Means for Windows Gamers and RAM Budgets

The practical reader runs Discord beside a game, a browser and a launcher. A chat app that idles at 1 to 2GB eats into that budget, and a lighter client frees headroom without a hardware purchase. That matters more when memory is expensive: our report on DDR5 chip prices up 452 percent covers why adding RAM is a costly fix right now, and our 32GB vs 64GB RAM guide shows what a gaming build needs. Software that asks for less memory is the cheapest upgrade available.

### What to do today

Nothing changes for you yet. 9to5Windows advised users to keep running the current app. There is no download, no beta channel and no sign-up. Windows Latest also understands the new client will skip the “Starting” screen at launch because it no longer loads Electron libraries, but that is the outlet’s understanding and not a Discord statement.

## What Discord Has Not Said About Release, Mods and Platforms

Discord has not given a release date or opened a public test. It has not said whether the browser version changes, how the rebuild affects third-party client mods, or how much of the current feature set ships on day one. It has not published a benchmark methodology, so the memory figure cannot be reproduced. Nothing in the available reporting says which desktop platforms get the GPUI client first, and the screenshot is from Windows 11. A framework that draws its own interface has to handle text rendering, input methods and accessibility on each operating system.

Our own expectations, not statements from Discord:

- **A limited test comes first.** Vishnevskiy said testing with users is the hope, so expect an opt-in group before any general rollout.
- **Independent measurements will narrow the gap.** Once reviewers run both clients with the same servers and voice sessions, the difference will likely stay large but smaller than 290MB versus 4GB suggests.
- **Embedded web content will stay.** Activities depend on web technology, so a native shell with webview panels is the most probable outcome.
- **GPUI will get hard scrutiny.** Accessibility, input handling and platform quirks will surface quickly at Discord’s scale.
