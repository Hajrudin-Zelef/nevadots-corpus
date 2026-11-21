---
id: collect-261121-veille-ia/google-ecosystem/update-chrome-now-327-security-fixes-10-critical
title: "Update Chrome Now: 327 Security Fixes, 10 Critical"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["chrome", "security", "critical", "chromes", "google", "vulnerability", "fixes", "flaws"]
source: https://techwey.com/google-chrome-152-update-327-security-fixes-august-2026/
source_anchor: ""
source_lines: [1, 50]
section: "Google_Ecosystem"
---
Google released Chrome 152 yesterday — and the security numbers are unlike anything seen in a single browser update before. The release patches 327 vulnerabilities in total: 10 rated critical, 61 high, 184 medium, and 72 low. It is the largest security patch in Chrome’s history by vulnerability count.

More striking than the number is how most of them were found. Of the 327 flaws fixed, 299 were discovered internally by Google — using AI-powered vulnerability scanning tools. The use of AI to find security flaws has driven an extraordinary surge in Chrome vulnerability discovery this year, with Google patching well over 2,000 Chrome vulnerabilities to date in 2026 alone.

Chrome 152 is rolling out progressively as version 152.0.7977.64 for Linux and 152.0.7977.64/.65 for Windows and macOS. If you use Chrome, check your version now.

## What Makes a “Critical” Browser Vulnerability

Not all security flaws are equal. A critical-rated browser vulnerability is one that could let an attacker run code on your machine — without you downloading anything or opening a suspicious file. Just visiting a malicious or compromised webpage can be enough.

Most of Chrome 152’s ten critical flaws are use-after-free vulnerabilities. This is a memory-corruption class where software accesses a region of memory after it has already been freed and potentially reassigned. In a browser context, if an attacker can control what data ends up in that freed memory location, they may be able to redirect the program’s execution — running their own code in the process.

The most significant externally reported issue is CVE-2026-79282, a critical use-after-free in ANGLE — Chrome’s graphics translation layer, which processes web-delivered graphics content. Google awarded researcher Goodluck $25,000 for reporting this flaw. Other critical issues include use-after-free vulnerabilities in Aura (Chrome’s UI framework), Chromecast components, Views, and Chrome’s Safe Browsing module. There is also an uninitialized-variable flaw in Chrome’s mobile subsystem and an improper input validation bug in Chromecast.

## The AI-Driven Vulnerability Surge

The fact that 299 of 327 flaws were found internally by Google deserves its own section — because it signals something important about where security is heading.

Google has been deploying AI-powered code analysis and fuzzing tools at scale across its own codebase. These tools can analyse millions of code paths, try unusual inputs, and flag potential memory safety issues faster than any human team. The result is a dramatic increase in the rate at which vulnerabilities are being found — and patched before attackers find them.

This is the same dynamic playing out across the broader security industry. AI is making defenders faster at finding flaws — but it’s also making attackers faster at finding and exploiting them. Microsoft’s June Patch Tuesday broke records partly for the same reason. Apple’s August update was similarly large. The AI-accelerated vulnerability discovery cycle is compressing the time between a flaw existing and a patch being available. That’s generally positive — but it also means the pace of updates is accelerating, and keeping software current matters more than ever.

## Other Notable Fixes in Chrome 152

Beyond the critical issues, Chrome 152 contains a substantial set of high-severity fixes worth noting. V8 — Chrome’s JavaScript engine — received fixes for type confusion bugs. Type confusion in a JavaScript engine is particularly dangerous because JavaScript can be delivered directly through a webpage, giving attackers a low-friction path to exploitation.

WebRTC, Chrome’s real-time communication stack used for video calls and peer-to-peer connections, also received high-severity fixes. So did Chrome’s Extensions framework, Bluetooth handling, GPU components, and the browser sandbox — the security layer designed to prevent malicious web content from reaching your operating system.

Google has also announced that starting with Chrome 153, the release cycle will shorten to two weeks. Chrome 153 is scheduled for September 8. That faster cadence means security fixes will reach users more quickly going forward — but it also means keeping Chrome updated becomes even more important as a routine habit.

## How to Update Chrome Right Now

Chrome updates automatically, but the rollout is progressive — it may not have reached your device yet. To force the update immediately:

- Open Chrome and click the three-dot menu in the top right corner
- Go to Help → About Google Chrome
- Chrome will check for updates and download them if available
- Click Relaunch to complete the installation

You’re looking for version 152.0.7977.64 (Linux) or 152.0.7977.64 or .65 (Windows and macOS). If you see a lower version number, click update and relaunch before you browse further.

Google has withheld specific technical details on some vulnerabilities until the majority of Chrome users have updated — standard practice to reduce the window attackers have to exploit public information before patches are applied. That window is closing. Update now. For more on staying protected across all your devices, see our guide on protecting yourself from AI-powered cyber threats and our coverage of Microsoft’s record June Patch Tuesday.

## The Bottom Line

327 fixes. 10 critical. Most found by AI. Chrome 152 is the most significant browser security update in years, and it arrived yesterday. Check your version, force the update if needed, and relaunch. Ten minutes now is worth considerably more than the alternative.

Read more tech related articles here.

**Sources:** Malwarebytes, Cybersecurity News
