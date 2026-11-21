---
id: collect-261121-veille-ia/windows-os/every-windows-11-app-should-kill-electron-and-discord-just-showed-why
title: "Every Windows 11 app should kill Electron, and Discord just showed why"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["discord", "windows", "electron", "client", "rust", "gpui", "desktop", "application"]
source: https://blogs.techbytes.app/posts/every-windows-11-app-should-kill-electron-and-discord-just/
source_anchor: ""
source_lines: [1, 108]
section: "Windows_OS"
---
# Every Windows 11 app should kill Electron, and Discord just showed why

Discord demonstrated a native Windows 11 client built with Rust and GPUI that boots instantly and consumes 290.2MB of RAM while running two accounts at once.

By Dillip Chowdary â¢ Nov 08, 2026 â¢ Source: windowslatest.com

At a glance

- Framework
- Rust with GPUI
- Idle memory
- 290.2MB with two accounts logged in
- Electron baseline
- Typical use below 1GB
- Startup
- Window fades in within one second
- Platforms
- Windows, macOS, Linux
- Token storage
- Windows Credential Manager

In this post

Discord is replacing its long-standing desktop architecture with a custom native rebuild written in Rust and rendered via GPUI, dropping the bundled Chromium engine that powered its chat application for a decade. According to details highlighted in windowslatest.com's report, video footage shared by Discord co-founder and chief technology officer Stanislav Vishnevskiy demonstrated the new client, internally labeled Discord Phoenix, starting from a cold boot on Windows 11 Lenovo ThinkPads.

In side-by-side demonstrations, the native client loaded the interface and populated a user's friends list before the legacy Electron build cleared its initial loading screen. Task Manager metrics revealed the new client idling at 290.2MB of memory while simultaneously keeping two accounts logged in, representing an approximate 70 percent reduction compared to typical sub-1GB baseline usage.

This breakdown examines the technical shift Discord executed to replace its multi-process web foundation with GPU-rendered native code, covering how memory usage fell, how background updates replace modal spinners, and how code sharing will function across desktop, mobile, and web clients. The analysis serves engineers, system administrators, and everyday Windows 11 users evaluating desktop application overhead, resource-heavy communication tools, and the architectural differences between traditional Chromium wrappers and GPU-accelerated application frameworks.

## 01What actually changed

Discord demonstrated an overhauled desktop build that abandons the multi-process Chromium and Node.js runtime that the service used since launching on desktop platforms. In the demonstration recorded on Lenovo ThinkPad machines equipped with Copilot keys, the legacy Electron application opened to a blank dark window for three seconds before displaying a startup screen with trivia notes, taking roughly five seconds before reaching an empty interface stating that no friends were online. The new client faded in within a second of being double-clicked, rendering the full interface and displaying populated friend cards immediately.

Internal testing metrics shared by Discord leadership confirmed that the 290.2MB working set registered in Windows Task Manager operated with two accounts logged in concurrently. Historical usage noted by Discord engineers in December 2025 placed typical Electron consumption beneath 1GB, with tests evaluating forced client restarts at 4GB thresholds, while day-to-day testing routinely recorded idle baselines between 1GB and 2GB. Prior internal optimizations to Electron failed to bridge that performance divide, as an update to Electron 42 in September reduced median CPU consumption by 17 percent while causing a 4 percent regression in p50 memory overhead.

## 02How it works

Rather than adopting Tauri, which pairs Rust application logic with system web engines like Microsoft WebView2, Discord selected GPUI, an immediate and retained mode GPU-accelerated framework created by the developers of the Zed text editor. GPUI renders user interface elements directly on graphics hardware without HTML or CSS parsing, while Rust manages system resources without garbage collection pauses.

Discord maintains custom patches on top of the framework, with Vishnevskiy noting intentions to upstream applicable improvements to the community. Embedded web features like Activities and YouTube video players remain isolated inside native webview components rather than drawing through the primary graphics pipeline.

| Component | Framework | Role in Architecture | 
|---|---|---|
| New Desktop Client | Rust with GPUI | Native windowing, logic, and GPU rendering | 
| Legacy Desktop Client | Electron | Multi-process Chromium runtime being replaced | 
| Mobile Applications | Native UI with Rust | Phasing out React Native while sharing core logic | 
| Web Browser Client | WebAssembly with HTML | Compiling shared Rust logic to WASM | 
| Embedded Features | System WebView | Rendering YouTube embeds and Discord Activities | 

Advertisement

Tech Pulse Daily

### Get tomorrow's pulse first

Join engineers who read Tech Pulse before stand-up. Free, weekday mornings.

The desktop overhaul synchronizes with structural changes across Discord's remaining software targets. The company is phasing out React Native across its iOS and Android builds, shifting to native platform user interfaces backed by shared Rust code.

In the browser, Discord is transitioning client logic from JavaScript to WebAssembly so that the web application executes the same shared Rust codebase while still rendering through standard DOM structures. Login security relies on native platform storage, with audio and video lead Coral confirming authentication tokens persist directly within Windows Credential Manager.

## 03Why it matters

The transition eliminates noticeable user delays while freeing significant memory reserves on client devices. Traditional Electron deployments require distinct main and renderer processes for each window, imposing base memory footprints before chat content loads.

The native GPUI client eliminates the legacy modal update dialog that previously held users for five to ten seconds upon startup. The client bootstraps instantly and executes routine software patching in the background without routing distribution through Microsoft Store infrastructure or the Windows Update Orchestration Platform.

The architecture also expands practical multi-account workflows for users balancing separate profiles. In the existing desktop application, an account switcher accommodates five profiles but demands teardown operations when cycling between identities.

The new client maintains active sessions for multiple profiles simultaneously without restarting client processes or clearing memory states. Discord engineering confirmed that user interfaces, menus, and layout structures remain visually identical to the outgoing client, preserving existing interactions while removing the underlying browser overhead.

## 04Who is affected

The performance gains directly benefit users on memory-constrained systems, multi-tasking setups, and laptops balancing active chat sessions alongside heavy workstation software or games. Operating systems running competing communication tools have faced compounding memory burdens, with tests demonstrating combined idle consumption for WhatsApp, Microsoft Teams, and Discord reaching 1.5GB on Windows 11. WhatsApp previously shifted from a native architecture requiring under 100MB of idle memory to a WebView2 wrapper demanding up to 1.2GB, while Microsoft Teams idled near 963MB.

Discord's native client rollout will reach across all primary desktop platforms rather than remaining confined to Windows. Marketing director Ryan K.

Rigney confirmed Linux compatibility, while engineering staff confirmed wide internal use across macOS hardware. Because GPUI natively supports Windows, macOS, and Linux, Discord bypassed Windows-only alternatives like WinUI, ensuring cross-platform parity without maintaining separate desktop UI implementations for non-Windows operating systems.

## 05What to watch

Discord is preparing to release the Rust client into private testing phases before scheduling a general public deployment. VP of Engineering Mark Smith noted that accessibility and security testing remain primary prerequisites before wider distribution, while Rigney stated that private evaluation will begin soon. Company representatives have not issued a firm calendar date for the general release, focusing validation on edge-case stability across internal builds.

The transition arrives amid broader industry pressure on system memory allocations, driven by escalating data center dynamic RAM consumption and rising hardware costs. Microsoft Windows executive Pavan Davuluri recently characterized memory management as an industry-wide priority as Windows 11 engineers work to trim operating system baselines. Discord's demonstration proves that large-scale consumer applications can abandon bundled Chromium runtimes for GPU-accelerated code, establishing an optimization model for other major desktop productivity suites to follow.

## Frequently asked questions

### What framework is Discord using to replace Electron?

Discord is using GPUI, a GPU-accelerated user interface framework written in Rust developed by the creators of the Zed code editor.

### How much RAM does the new Discord client use compared to the old one?

The new Rust client idle footprint was measured at 290.2MB with two accounts logged in concurrently, roughly 70 percent below typical 1GB baseline usage.

### Will the new Discord client support Linux and macOS?

Yes, Discord confirmed that the new GPUI client runs on Linux and macOS alongside Windows 11.

### Why does the new Discord client skip the initial starting screen?

The client bootstraps instantly due to its new architecture and processes application updates automatically in the background instead of displaying a startup modal.

## Sources

## Related on Tech Bytes

Advertisement
