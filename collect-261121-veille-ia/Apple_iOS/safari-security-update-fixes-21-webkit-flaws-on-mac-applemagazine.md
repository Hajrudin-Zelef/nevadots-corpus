---
id: collect-261121-veille-ia/apple-ios/safari-security-update-fixes-21-webkit-flaws-on-mac-applemagazine
title: "Safari Security Update Fixes 21 WebKit Flaws on Mac - AppleMagazine"
domain: apple-ios
role: reference
task: article
actors: ["Apple"]
dates: []
keywords: ["safari", "webkit", "memory", "security", "browser", "update", "flaws", "corruption"]
source: https://amstage.consumertech.news/safari-security-update-26-6-1-webkit-flaws/amp/
source_anchor: ""
source_lines: [1, 79]
section: "Apple_iOS"
---
Safari security is the focus of Safari 26.6.1, a standalone browser update that patches 21 CVE-listed vulnerabilities on Macs running macOS Sonoma and macOS Sequoia. Most of the flaws affect WebKit, the browser engine responsible for loading and processing websites in Safari.

The update follows the release of iOS 26.6.1, iPadOS 26.6.1, visionOS 26.6.1, and macOS Tahoe 26.6.2 on August 17. Safari 26.6.1 arrived one day later, bringing the relevant browser fixes to users who remain on either of the previous two macOS generations.

There are no new browsing features or visible interface changes attached to the release. Its purpose is to replace vulnerable WebKit components before malicious or poorly formed web content can take advantage of them.

## Safari Security Update Covers 21 CVEs

The security bulletin for Safari 26.6.1 contains 19 vulnerabilities assigned directly to WebKit. It also includes one flaw affecting WebKit History and another involving WebKit Storage.

A CVE identifier is used to track a publicly documented security vulnerability. The presence of 21 identifiers does not necessarily mean that users faced 21 entirely separate attack methods. Several entries are related to similar weaknesses in memory handling, browser state management, input validation, and the way WebKit responds to unexpected data.

WebKit performs much of the invisible work behind a web page. It interprets HTML, runs JavaScript, handles page storage, processes visual elements, and manages interactions between the website and the browser. A problem inside that process can be triggered while Safari is attempting to load specially prepared web content.

According to the bulletin, many of the vulnerabilities could cause Safari or one of its supporting processes to crash. Other entries could lead to memory corruption, which occurs when software reads, writes, or manages memory in an unsafe way.

Crashes may sound less serious than data theft or arbitrary code execution, but they can still be signs of deeper weaknesses. A reliable crash can sometimes give researchers a starting point for discovering whether the underlying memory error could be used for a more damaging attack.

## Memory Corruption

Several of the patched flaws involve memory corruption, use-after-free errors, out-of-bounds access, or weak memory management.

A use-after-free vulnerability can occur when software attempts to access an area of memory after it has already been released. Safari 26.6.1 addresses two such WebKit problems, tracked as CVE-2026-64715 and CVE-2026-64787, through improved memory management.

CVE-2026-64784 involved out-of-bounds access. This type of error happens when software tries to access memory outside the area assigned to a particular operation. WebKit now applies improved bounds checking to prevent the affected process from reaching an invalid location.

Other vulnerabilities were addressed through improved locking. Locking helps coordinate processes that may attempt to read or modify the same resource at nearly the same time. Weak coordination can leave data in an inconsistent state, causing memory corruption or an unexpected browser crash.

WebKit also received stronger input validation and state management. Those protections help the browser reject unusual or malformed information instead of allowing it to reach a part of the engine that was not prepared to handle it.

Apple describes the likely consequences conservatively. The listed effects include Safari crashes, process termination, and memory corruption after processing maliciously crafted web content. The bulletin does not claim that these particular Safari flaws allow arbitrary code execution, so users should be careful about reports that assign capabilities beyond the published findings.

## One WebKit History Flaw Could Leak Sensitive Data

CVE-2026-64778 stands apart from most of the other entries because its stated effect is not a crash. Apple says that visiting a maliciously crafted website could cause sensitive data to leak.

The vulnerability appears under WebKit History and was fixed through improved checks. The bulletin does not identify the exact information that could be exposed, nor does it describe the method a website would use to retrieve it.

That limited disclosure is intentional. Technical details are often withheld until patches have reached enough devices, reducing the chance that the publication itself will help attackers build an exploit while many users remain unprotected.

WebKit Storage also received a fix for CVE-2026-64779. That vulnerability involved memory corruption caused by inadequate locking and could lead to an unexpected Safari crash. Web storage is commonly used by websites to keep preferences, temporary information, sign-in state, and other browser-side data, making reliable separation between sites essential.

Private Browsing cannot replace this Safari security update. Privacy controls can reduce the amount of information retained during a session, but they do not repair vulnerabilities in the browser engine processing the page. Content blockers and careful browsing habits are helpful layers, although neither can guarantee that a legitimate website has not been compromised or that an unfamiliar page is harmless.

## Fixes First Appeared in the Golden Gate Beta

Apple says the Safari 26.6.1 protections were first made available through the macOS Golden Gate 27 beta. The company has now brought those fixes back to macOS Sonoma and Sequoia rather than requiring users to install an unfinished operating system or wait for the next major macOS release.

This approach allows older supported systems to receive an updated browser engine while keeping the rest of the operating system on its current version. Safari is closely integrated with macOS, but its security components can still be delivered separately when an operating system update is not required.

Macs running macOS Tahoe received the same group of WebKit protections through macOS Tahoe 26.6.2. On iPhone and iPad, they arrived as part of iOS 26.6.1 and iPadOS 26.6.1.

The overlapping releases help keep WebKit protections aligned across devices. A flaw in the engine may affect more than Safari on Mac because WebKit is also used to display web content in many apps and system views.

## No Active Exploitation Disclosed

Apple did not state that any of the 21 Safari vulnerabilities had been exploited in attacks before Safari 26.6.1 became available. Its security notices normally include explicit language when the company is aware that a vulnerability may have been actively exploited.

The absence of that warning should not be interpreted as proof that exploitation is impossible. It only means the bulletin does not report known attacks at the time of publication.

Security researchers from several organizations and independent teams received credit for finding the flaws. The acknowledgments include researchers associated with Cisco Talos, TrendAI Zero Day Initiative, Calif.io, Citadelo, Braze Security Team, and OpenAI Codex Security.

Amy Burnett of OpenAI Codex Security received credit on eight of the CVE entries, including one shared with another researcher. The range of contributors suggests that the update combines findings from different research methods rather than addressing a single connected attack campaign.

## Check Safari After Installing the Update

#### To install Safari 26.6.1:

**System Settings > General > Software Update**

The Safari package may appear as a separate update or under the additional information available on the Software Update screen. Save active browser work before installing because Safari may need to close and reopen to load the corrected WebKit components.

#### To verify the installed version:

**Safari > About Safari**

The version window should report Safari 26.6.1. If Software Update says the Mac is current but Safari displays an earlier version, check that the computer is running macOS Sonoma or macOS Sequoia and look for any separately listed browser update.

Managed Macs may receive the package according to an organization’s deployment schedule. In that situation, the version displayed under About Safari provides a more reliable confirmation than the last time the Mac checked for system updates.
