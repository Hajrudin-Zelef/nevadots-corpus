---
id: collect-261121-veille-ia/google-ecosystem/update-chrome-now-google-just-patched-327-security-bugs-six-of-them-critical-meridians
title: "Update Chrome Now: Google Just Patched 327 Security Bugs, Six of Them Critical — Meridians"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: ["2026-08-29"]
keywords: ["chrome", "google", "security", "update", "just", "meridians", "critical", "malicious"]
source: https://meridians.news/article/chrome-152-critical-update-327-security-fixes-2026-08-29
source_anchor: ""
source_lines: [1, 41]
section: "Google_Ecosystem"
---
The short answer

- Google released Chrome 152.0.7977.64/.65 (Windows/Mac) and 152.0.7977.64 (Linux) on August 25, 2026, fixing 327 security issues, according to Google's official Chrome Releases blog.
- Security researchers at Malwarebytes flagged two as especially serious: CVE-2026-79282, a critical use-after-free bug in the ANGLE graphics engine that can let a malicious webpage run code outside Chrome's sandbox, and CVE-2026-78899, a high-severity V8 JavaScript engine flaw triggered just by visiting a malicious site.
- Google has not said whether either bug is being actively exploited; the company routinely withholds exploit details "until a majority of users are updated with a fix."
- The fix requires a restart: open Settings > About Chrome to trigger the download, then restart the browser to apply it — closing tabs alone doesn't finish the update.

Google shipped Chrome 152 on August 25, 2026, fixing 327 security issues in one release. Most are routine. Two are not: security researchers say they could let a malicious website escape Chrome's sandbox and run code directly on your machine, just from a visit to the wrong page.

## What's actually wrong

- CVE-2026-79282 (Critical): A use-after-free flaw in ANGLE, Chrome's graphics engine, that can let a malicious webpage execute arbitrary code outside the browser's sandbox — meaning it isn't contained to the browser tab
- CVE-2026-78899 (High, CVSS 8.8): A use-after-free flaw in Chrome's V8 JavaScript engine that can allow remote code execution and, per Malwarebytes' analysis, can be triggered just by visiting a malicious website
- 327 total fixes in this release, addressing use-after-free errors, out-of-bounds writes, and other memory-safety bugs across components including ANGLE, Aura, Chromecast, Views and Safebrowsing

## Is it being exploited right now?

Google's release notes don't say. The company's standard practice is to keep technical details of a vulnerability restricted "until a majority of users are updated with a fix," specifically to avoid handing attackers a roadmap before most people have patched. That means there's no public confirmation either way on active exploitation — which is also why security researchers treat sandbox-escape bugs like CVE-2026-79282 as urgent regardless.

## How to update

Chrome usually updates itself in the background, but the fix only takes effect after a restart. Go to Settings, then About Chrome — this triggers the update to download if it hasn't already — and then restart the browser. Simply closing and reopening tabs, without a full restart, leaves the old, vulnerable version running.

## Sources

Every factual claim above is traceable to these documents. Check them — that is why they are here.

- Chrome Releases: Stable Channel Update for Desktop (August 25, 2026)Google Chrome Releases
- Update Chrome before you browse againMalwarebytes

About this byline

Meridians Money Desk is an editorial desk at Meridians, not an individual. A desk byline means the article was produced and fact-checked to that desk's published standards. Read our editorial standards and corrections policy.

The Meridians briefing

## One considered email a week

Independent reporting and buying advice, in your inbox.

Sign-up opens with our launch issue. Nothing is sent or stored yet.
