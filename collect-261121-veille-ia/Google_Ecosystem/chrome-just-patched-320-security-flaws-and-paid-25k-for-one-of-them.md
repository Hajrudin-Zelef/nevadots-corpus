---
id: collect-261121-veille-ia/google-ecosystem/chrome-just-patched-320-security-flaws-and-paid-25k-for-one-of-them
title: "Chrome just patched 320+ security flaws, and paid $25K for one of them"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["chrome", "vulnerabilities", "google", "security", "flaws", "patched", "version", "search"]
source: https://www.pcworld.com/article/3221154/chrome-just-patched-320-security-flaws-and-paid-25k-for-one-of-them.html
source_anchor: ""
source_lines: [1, 19]
section: "Google_Ecosystem"
---
In the newest versions of Chrome—152.0.7977.64/65 for Windows and macOS, and 152.0.7977.64 for Linux—the developers have fixed over 320 vulnerabilities, some of which are critical. According to Google, none of the patched vulnerabilities are being exploited in the wild yet.

Google has also released Chrome for Android 152.0.7977.64 and Chrome for iOS 152.0.7977.64 this week. The Android version addresses the same vulnerabilities as the desktop versions. The Extended Stable Channel for Windows and macOS now includes Chromium version 152.0.7977.65.

The next version, Chrome 153, is due to be released in two weeks.

## What’s new in Chrome 152?

The browser’s built-in “What’s New?” page highlights a new “AI” mode in Chrome, which mainly relates to Google’s search engine. You can use it to supplement your search queries with open tabs, images, and files. If you follow a link from the search results while in “AI mode,” the search result remains displayed alongside it so you don’t lose that context.

Chrome usually updates automatically when a new version is available. You can manually check for updates via the menu option **Help → About Google Chrome** (alternatively, Settings → About Google Chrome).

## Over 300 security flaws patched

In the Chrome Releases blog post, Srinivas Sista lists 327 vulnerabilities that’ve been fixed and states that Google itself discovered 299 of them. The remaining 28 vulnerabilities were detected and reported by external security researchers. Google has awarded said researchers around $38,000 in bounties, with the lion’s share of $25,000 going to CVE-2026-79282, discovered by a researcher who goes by the name “Goodluck.” This is a critical use-after-free (UAF) vulnerability in the Angle graphics API, which was reported back in March.

Nine of the 10 flaws classified as critical are use-after-free (UAF) vulnerabilities in various components, including the Angle and Aura graphics interfaces. A further 61 vulnerabilities are classified as high risk. Of the remaining vulnerabilities, 184 are medium risk and 72 low risk. The most common types of flaws are use-after-free and incorrect authorization (50 cases each), followed by insufficient input validation (36 cases).

**Tip:** Whether you keep your browser up to date, you need proper antivirus protections if you want your PC to remain secure and private. Check out our picks for the best antivirus software for Windows as well as best VPN services to stay ahead of security problems.
