---
id: collect-261121-veille-ia/samsung-mobile/galaxy-s25-rce-tiff-flaw-patched-in-july-disclosure-arrives-in-august-deafnews
title: "Galaxy S25: RCE TIFF Flaw Patched in July, Disclosure Arrives in August | DeafNews"
domain: samsung-mobile
role: reference
task: article
actors: ["Samsung"]
dates: ["2026-08-12"]
keywords: ["tiff", "disclosure", "august", "galaxy", "samsung", "release", "flaw", "advisory"]
source: https://deafnews.it/en/news/vulnerabilities/galaxy-s25-rce-tiff-flaw-patched-in-july-disclosure-arrives-in-august
source_anchor: ""
source_lines: [1, 68]
section: "Samsung_Mobile"
---
## Get DeafLetter

A weekly selection of signals, vulnerabilities and guides. Critical alerts remain optional.

You can unsubscribe at any time. Privacy policy.

On **August 12, 2026**, Trend Micro's Zero Day Initiative published advisory **ZDI-26-529** covering a heap-based buffer overflow in the TIFF processing component of the Samsung Galaxy S25. The disclosure arrives with a significant lag: the vendor had already released the fix in the **SMR Jul-2026 Release 1**. The case highlights the persistent problem of the temporal gap between the technical availability of a patch and public awareness, an asymmetry that penalizes users with lax update management or corporate BYOD fleets with slow approval cycles.

**Key Takeaways**

- ZDI-26-529 / CVE-2026-21045: heap-based buffer overflow in Samsung Galaxy S25's `libimagecodec.media.quram.so` library, RCE impact, CVSS 4.0 score 8.4 HIGH
- Attack requires user interaction: opening a malicious TIFF file or visiting a malicious web page, a fertile vector for social engineering
- Fix available since **SMR Jul-2026 Release 1** ; coordinated public disclosure on**August 11, 2026** , roughly three months after the May 6 vendor report
- CVE record confirms devices "unaffected" on Android 14, 15, and 16 with SMR Jul-2026 Release; Samsung acknowledged the same vulnerability as SVE-2026-1087 in its own security advisory

## The Flaw: Heap Overflow in Proprietary TIFF Parser

The technical core resides in the `libimagecodec.media.quram.so` library, Samsung's proprietary image codec component. During TIFF format parsing, the lack of validation of user-supplied data length before copying it to a heap-based buffer allows controlled memory overwrite. Exploitation leads to arbitrary code execution in the context of the image codec process, with potential escalation to system privileges.

 "The specific flaw exists within the processing of TIFF files. The issue results from the lack of proper validation of the length of user-supplied data prior to copying it to a heap-based buffer. An attacker can leverage this vulnerability to execute code in the context of the current process."

   â ZDI Advisory ZDI-26-529


The TIFF format, designed for high-fidelity imaging and document scanning, remains pervasive in enterprise environments: email attachments, document workflows, network scanners, digital fax. Its seemingly innocuous presence lowers the end user's guard, making the "user interaction required" vector particularly insidious for spear-phishing campaigns or distribution of compromised documents.

## The Risk Metric: CVSS 8.4 and the Network Vector

According to the official CVE-2026-21045 record, the vulnerability scores a **CVSS 4.0 of 8.4 (HIGH)**. The full vector string â `CVSS:4.0/AV:N/AC:L/AT:P/PR:N/UI:N/VC:N/VI:H/VA:L/SC:N/SI:N/SA:N` â indicates a network-accessible attack with low complexity and no preliminary privileges required. The `AT:P` element (Attack Type: Phishing) specifies the need for human interaction, consistent with the ZDI description.

The HIGH severity, combined with network accessibility and no authentication requirements, places the vulnerability in a risk tier that mobile security teams cannot afford to underestimate. For Galaxy S25 devices in corporate BYOD configurations, the attack profile overlaps exactly with the weak points of patch management policies: users who delay SMR installation due to personal inertia or administrative constraints.

## Timeline and the Silent Patch Problem

The ZDI-documented timeline shows an **extended coordinated disclosure**: vendor report on May 6, 2026, public advisory release on August 11, 2026. The roughly three-month window between report and disclosure is typical for ZDI policies, but the critical security posture detail is different: the patch was already publicly available in July 2026, integrated into Samsung's monthly SMR cycle.

The CVE record explicitly confirms: "unaffected at SMR Jul-2026 Release in Android 14, 15, 16." Samsung, in its July security bulletin, classified the vulnerability as SVE-2026-1087 with the description: "Out-of-bounds write in parsing TIFF format in libimagecodec.media.quram.so prior to SMR Jul-2026 Release 1 allows remote attackers to write out-of-bounds memory." The textual correspondence with the ZDI advisory is total, confirming it is the same flaw tracked under different identifiers.

The "silent patch" effect â a fix distributed without a detailed advisory explaining severity and mechanism â leaves users unaware of the specific risk until coordinated disclosure. Proactive updaters are protected; others remain potentially exposed for weeks without understanding the urgency to update.

## What to Do Now

Sources document concrete, verifiable actions:

- **Verify Installed SMR** : confirm the Galaxy S25 device has the July 2026 Security Maintenance Release or later; the CVE record indicates "unaffected at SMR Jul-2026 Release in Android 14, 15, 16"
- **Prioritize TIFF Attachments** : treat TIFF files from unverified sources as high-risk vectors until patch application is confirmed, given the ease of distribution via email and messaging
- **Accelerate BYOD Cycles** : for corporate mobile fleets, reduce the time-to-patch for Samsung SMRs in device management policies, recognizing that the delay between fix and disclosure creates unperceived risk windows
- **Monitor SVE-2026-1087** : the Samsung source documents no alternative mitigations or workarounds; the only confirmed non-vulnerable state is post-SMR Jul-2026

No information emerges on in-the-wild exploits, public PoCs, or technical details of Samsung's implemented fix.

## Why the ZDI-26-529 Case Matters Beyond the Single Device

The issue does not resolve into individual patch management. The `libimagecodec.media.quram.so` TIFF parser is a Samsung proprietary component, not AOSP code: the fix depends solely on the vendor and is not auditable by the open source community. This architectural closure, typical of OEM Android software stacks, concentrates input validation responsibility in a single actor.

The disclosure gap â July fix, August awareness â is not technically a supply chain regression, but it reveals a structural tension: the vendor's response speed outpaces its risk communication speed, and users are not incentivized to promptly install updates whose criticality is not yet narratable. For the Galaxy S25, Samsung's flagship portfolio device, the attack surface is maximal: wide distribution, growing enterprise user base, TIFF format still pervasive in document workflows.

The generality of the "Galaxy S25 devices" wording in the ZDI advisory and the lack of build specification in the CVE record leave this dimension unverified.

*Information verified against cited sources and current as of publication.*

## Sources

## Get DeafLetter

A weekly selection of signals, vulnerabilities and guides. Critical alerts remain optional.

You can unsubscribe at any time. Privacy policy.
