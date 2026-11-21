---
id: collect-261121-veille-ia/google-ecosystem/chrome-152-patches-327-vulnerabilities-including-sandbox-escape-rce-deafnews
title: "Chrome 152 Patches 327 Vulnerabilities, Including Sandbox-Escape RCE | DeafNews"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: ["2026-08-26"]
keywords: ["sandbox", "vulnerabilities", "chrome", "use-after-free", "code", "cve-2026-79282", "angle", "arbitrary"]
source: https://deafnews.it/en/news/vulnerabilities/chrome-152-patches-327-vulnerabilities-including-sandbox-escape-rce
source_anchor: ""
source_lines: [1, 63]
section: "Google_Ecosystem"
---
## Get DeafLetter

A weekly selection of signals, vulnerabilities and guides. Critical alerts remain optional.

You can unsubscribe at any time. Privacy policy.

Google shipped Chrome 152.0.7977.64/.65 on August 26, 2026, with a record payload of fixes: 327 security patches, 10 of them critical. Two vulnerabilities dominate the risk profile. CVE-2026-79282, a use-after-free in the ANGLE graphics component, allows arbitrary code execution outside the browser sandbox â the worst-case scenario for software that handles untrusted content. CVE-2026-78899, a use-after-free in the V8 JavaScript engine, remains confined to the sandbox but carries a CVSS score of 8.8 out of 10. The update is immediate for users: both trigger on a simple visit to a malicious HTML page.

**Key Takeaways**

- Chrome 152.0.7977.64/.65 delivers 327 security fixes, 10 critical, for Windows, Mac, and Linux.
- CVE-2026-79282 is a use-after-free in ANGLE with sandbox-escape RCE: visiting a malicious site compromises the underlying OS directly.
- CVE-2026-78899, a V8 use-after-free rated CVSS 8.8, enables RCE inside the sandbox but can be chained for escape.

## The Dangerous Core: ANGLE and the Sandbox Bypass

CVE-2026-79282 resides in ANGLE, the Chromium graphics backend that translates OpenGL ES calls into native APIs (DirectX, Metal, Vulkan). The official CVE record rates it Critical with a "crafted HTML page" vector and impact of "execute arbitrary code outside the sandbox."

 "A remote attacker could exploit it using a crafted HTML page to execute arbitrary code outside the browser sandbox"

Chrome's sandbox is the defensive perimeter that isolates the renderer â the process interpreting HTML, CSS, and JavaScript â from the operating system. ANGLE occupies a privileged position in the rendering pipeline. A use-after-free in this component does not require multi-stage exploit chains: arbitrary code exits the protected container directly. According to the source, this vulnerability "turn[s] a visit to a malicious or compromised website into direct code execution on the underlying operating system, often without requiring additional exploitation steps."

The CVE-2026-79282 record specifies: "Google Chrome on Android prior to 152.0.7977.65." The primary source, Malwarebytes, instead describes the threat as relevant to desktop browsers on Windows, Mac, and Linux.

## V8 and the Compromise Chain: Inside and Outside the Sandbox

CVE-2026-78899 hits V8, Chrome's JavaScript engine, and is classified as a use-after-free with a CVSS score of 8.8 out of 10. The vector is identical: a crafted HTML page. The impact differs: arbitrary execution inside the sandbox, not outside.

The distinction is not academic. An inside-sandbox vulnerability typically requires a second flaw â an escape or broker bypass â to reach the operating system. However, the CVSS 8.8 indicates high impact on confidentiality, integrity, and availability, with a network vector, low attack complexity, and user interaction limited to a single click (or less, in the case of already-open tabs or invisible redirects). The source explicitly states this flaw "can be chained with others for escape."

The combination of the two vulnerabilities builds a particularly efficient scenario for attackers: CVE-2026-78899 as a first stage to gain control in the renderer, CVE-2026-79282 or another flaw as a second stage to exit the sandbox. The advisory does not specify whether this chain has been demonstrated or whether public exploit code exists.

## Why It Matters

The dossier documents no specific mitigations beyond updating to version 152.0.7977.64/.65.

The risk profile of the two primary vulnerabilities â activation via web page, no additional user action required, impact up to full system compromise â places this release in the category of updates that tolerate no delay. The source does not specify the nature of data potentially exposed in a successful exploit, nor whether the vulnerabilities have been subject to documented attacks.

Chrome's automatic patching, enabled by default, fails when the user does not restart the browser. The update mechanism downloads the new binary in the background but requires closure and reopening for activation. The browser session â with tabs, extensions, persistent authentications â creates friction against restarting. This activation delay exposes measurable windows of vulnerability in hours or days even where the patch is already present on disk.

## Patching Automation and the Activation Gap

Chrome's update mechanism is technically continuous but practically discrete: the new version replaces the previous one only at session end.

The combination of sandbox-escape RCE with activation latency creates a structural attack surface: exploits targeting unpatched versions find targets in a heterogeneous population of installations, where the mere presence of the patch on the server does not guarantee effective protection.

For organizations, this translates into a governance problem: visibility into the version actually running on endpoints, not merely the one installed, becomes the relevant datum.

## The Offensive Value of Sandbox-Escape Vulnerabilities

The source explicitly links technical severity to attacker utility. Vulnerabilities enabling RCE outside the sandbox are "particularly valuable to attackers" because they convert ordinary web traffic â a visit to a compromised or malicious site â into privileged code execution on the host system. This profile lowers initial-access requirements, eliminates the need for targeted social engineering, and suits mass campaigns.

The CVSS 9.6 score for CVE-2026-79282 reflects this configuration: remote attack, low complexity, no privileges required, minimal user interaction (single click or visit), changed scope (compromise exceeds the vulnerable component's security boundary), and maximum impact on confidentiality, integrity, and availability.

The absence of attribution or evidence of active exploitation does not diminish patching priority: the offensive potential, documented in the technical specifications, is sufficient to classify this release as critical for operational security.

## Sources

## Get DeafLetter

A weekly selection of signals, vulnerabilities and guides. Critical alerts remain optional.

You can unsubscribe at any time. Privacy policy.
