---
id: collect-261121-veille-ia/google-ecosystem/google-releases-chrome-152-patching-327-vulnerabilities-including-10-critical-flaws-biggo-finance
title: "Google Releases Chrome 152, Patching 327 Vulnerabilities Including 10 Critical Flaws — BigGo Finance"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["chrome", "critical", "vulnerabilities", "google", "patching", "including", "flaws", "user"]
source: https://finance.biggo.com/news/d7d2b2ea-db03-40d7-8a7c-079b9398975c
source_anchor: ""
source_lines: [1, 48]
section: "Google_Ecosystem"
---
# Google Releases Chrome 152, Patching 327 Vulnerabilities Including 10 Critical Flaws

Google released Chrome 152 stable for desktop on August 25. In addition to patching 327 vulnerabilities—10 of which are classified as "critical"—the update introduces a host of new developer-facing features, including the CPU Performance API, which allows websites to determine the CPU performance tier of a user's device, and a Connection Allowlist that restricts connections to external servers.

The update is rolling out progressively across Windows, macOS, and Linux, and is expected to reach all users over the coming days to weeks. Users can also manually trigger the update from Chrome's settings menu.

### Breakdown by Severity and Key Vulnerabilities

The 327 vulnerabilities addressed in this release break down by severity as follows: 10 critical, 61 high, 184 medium, and 72 low.

| Severity | Count | 
|---|---|
| Critical | 10 | 
| High | 61 | 
| Medium | 184 | 
| Low | 72 | 

Note: Counts are based on Google's published security fix information for Chrome 152.

Critical vulnerabilities include a use-after-free in ANGLE, the graphics processing component (CVE-2026-79282), a use-after-free in Aura, the UI framework (CVE-2026-79290), and a Chromecast-related use-after-free (CVE-2026-79054). Use-after-free flaws are serious issues that can lead to arbitrary code execution by incorrectly referencing memory regions that have already been freed.

Other fixes include an out-of-bounds read in ANGLE (CVE-2026-78989), a memory corruption in the Tint shader compiler (CVE-2026-79069), a type confusion in accessibility features (CVE-2026-79175), a type confusion in animation processing (CVE-2026-79209), and a buffer overflow in the Blink rendering engine (CVE-2026-79216).

Patched version numbers vary by operating system: Windows and macOS receive version 152.0.7977.64 or .65, while Linux receives 152.0.7977.64.

### A Wave of New Developer Features

Beyond security fixes, Chrome 152 stands out for its expanded feature set for web developers. The most notable addition is the CPU Performance API. This API enables web applications to identify the CPU performance tier of a user's device. Apps can use this information to adjust rendering quality and processing load based on device capabilities, optimizing the user experience. When needed, it can be combined with the existing Compute Pressure API to respond to fluctuations in CPU load.

On the security front, a Connection Allowlist has been added. This mechanism restricts connections initiated from documents or workers via the Fetch API and other means, giving developers explicit control over external endpoints. Chrome checks destinations against the allowlist before establishing connections, permitting only verified endpoints.

In CSS, support for the pseudo-elements `::backdrop`, `::scroll-marker`, and `::view-transition` has been expanded. Developers can now write concise code for the close-on-backdrop-click behavior in dialogs. Additionally, media element pseudo-classes that toggle display based on playback state have been added, with seven now available: `:playing`, `:paused`, `:seeking`, `:buffering`, `:stalled`, `:muted`, and `:volume-locked`. These are positioned as a key focus area of Interop 2026, an initiative aimed at improving cross-browser compatibility.

In HTML, the `autocorrect` attribute has been shipped, allowing developers to control whether automatic correction is applied to user input in editable elements such as input fields and text areas.

### Privacy Sandbox API Consolidation Continues

Meanwhile, Chrome 152 announces the end of support and removal of the Private Aggregation API and related Privacy Sandbox APIs. This API was designed to obtain aggregated advertising data while protecting user privacy, but it was only exposed through the Shared Storage API and Protected Audience API, both of which are also being deprecated and slated for removal.

WebGPU, which enables high-speed computation leveraging the GPU, gains an optional feature called `subgroup-size-control`. This allows developers to specify the size of groups processed at once in compute shaders, which is said to be particularly well-suited for performance optimization in AI workloads.

### Release Cycle Set to Shorten

Google has updated Chrome every four weeks since 2021, but in March 2026 the company announced plans to shorten the release cycle to two weeks. The new cycle is scheduled to begin with Chrome 153, whose stable version is expected on September 8, 2026. The Chrome 153 beta was already released on August 20.

Once added, BigGo Finance appears first in Google Search Top Stories, so you get the broadest, most up-to-the-minute, and most comprehensive global financial news first.

Add preferred source
