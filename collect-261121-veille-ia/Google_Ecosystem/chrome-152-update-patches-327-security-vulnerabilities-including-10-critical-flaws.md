---
id: collect-261121-veille-ia/google-ecosystem/chrome-152-update-patches-327-security-vulnerabilities-including-10-critical-flaws
title: "Chrome 152 Update Patches 327 Security Vulnerabilities, including 10 Critical Flaws"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: ["2026-08-25"]
keywords: ["chrome", "exploited", "update", "critical", "cwe-416", "flaws", "google", "security"]
source: https://securityonline.info/chrome-152-security-update/
source_anchor: ""
source_lines: [1, 52]
section: "Google_Ecosystem"
---
## TL;DR

Google shipped Chrome 152 to the stable channel on August 25, 2026. The Chrome 152 update fixes 327 security vulnerabilities, including 10 rated Critical. Most of the top flaws are use-after-free bugs, so update soon.

- **Total:** 10 CVEs
- **Severity:** 7 Critical · 3 High
- **Actively exploited:** None confirmed
- **Highest severity:** 9.6 (Critical · CVSSv3) — CVE-2026-79282
- **Action:** Apply the latest security updates now

Notable CVEs

| CVE | CVSS (CVSSv3) | Type | Fixed in | Status | 
|---|---|---|---|---|
| CVE-2026-79282 | 9.6 | CWE-416 | 152.0.7977.65 | Not exploited | 
| CVE-2026-79290 | 9.6 | CWE-416 | 152.0.7977.65 | Not exploited | 
| CVE-2026-79052 | 9.6 | CWE-416 | 152.0.7977.65 | Not exploited | 
| CVE-2026-79150 | 9.6 | CWE-416 | 152.0.7977.65 | Not exploited | 
| CVE-2026-78935 | 9.6 | CWE-457 | 152.0.7977.65 | Not exploited | 
| CVE-2026-79012 | 9.6 | CWE-416 | 152.0.7977.65 | Not exploited | 
| CVE-2026-79200 | 9.6 | CWE-416 | 152.0.7977.65 | Not exploited | 
| CVE-2026-79054 | 8.3 | CWE-416 | 152.0.7977.65 | Not exploited | 

## Why This Chrome 152 Update Matters

Chrome runs on billions of desktops worldwide. As a result, a browser flaw gives attackers a wide target. This Chrome 152 update lands one of the year’s largest single batches of fixes.

Google paid a $25,000 reward for the top bug. That figure signals real severity. Memory-safety flaws like these can let a malicious web page run code on your machine.

## How the Attacks Work

Most Critical fixes address use-after-free bugs. These happen when code reuses memory that was already released. Consequently, an attacker can craft a page that corrupts memory and hijacks the process.

The flaws touch several browser parts. For example, CVE-2026-79282 sits in ANGLE, Chrome’s graphics layer. Others affect Aura, Views, Chromecast, and Safebrowsing. One Mobile bug, CVE-2026-78935, stems from an uninitialized variable.

### Is It Being Exploited?

No exploitation in the wild has been confirmed. Google reports no active attacks tied to these bugs. Likewise, no public proof-of-concept exists yet. Still, Google restricts bug details until most users patch.

## Affected Versions

Any Chrome build before 152 is affected. The fixed release is 152.0.7977.64 for Linux. Windows and Mac get 152.0.7977.64 or .65. The rollout reaches users over the coming days and weeks.

## Patch and Mitigation Steps

Update Chrome right away. Open the menu, then choose Help and About Google Chrome. The browser checks for updates and installs them. Afterward, relaunch to apply the patch.

For the full list, read Google’s Chrome 152 stable channel update. Chromium-based browsers like Edge and Brave will ship the same fixes soon.

#### Support Our Threat Intelligence

Find our vulnerability reports and weekly recaps helpful? Support our work today and unlock a **100% ad-free** reading experience!
