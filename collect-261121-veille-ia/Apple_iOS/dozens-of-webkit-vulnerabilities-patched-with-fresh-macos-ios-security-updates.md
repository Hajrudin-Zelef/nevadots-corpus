---
id: collect-261121-veille-ia/apple-ios/dozens-of-webkit-vulnerabilities-patched-with-fresh-macos-ios-security-updates
title: "Dozens of WebKit Vulnerabilities Patched With Fresh macOS, iOS Security Updates"
domain: apple-ios
role: reference
task: article
actors: ["Apple"]
dates: []
keywords: ["security", "updates", "vulnerabilities", "memory", "kernel", "fresh", "macos", "webkit"]
source: https://www.securityweek.com/dozens-of-webkit-vulnerabilities-patched-with-fresh-macos-ios-security-updates/
source_anchor: ""
source_lines: [1, 27]
section: "Apple_iOS"
---
**Apple on Monday announced a fresh round of macOS, iOS, and iPadOS security updates that address dozens of vulnerabilities, most of which affect the web browser engine WebKit.**

macOS Tahoe 26.6.2 is now rolling out with fixes for 28 security defects, including 21 in WebKit that could lead to Safari/process crashes, memory corruption, and sensitive data disclosure.

The update also resolves seven issues in Audio, ImageIO, IOGPUFamily, and Kernel that could lead to sensitive user information disclosure, denial-of-service (DoS), arbitrary code execution, memory corruption, system termination, and kernel memory disclosure or corruption.

iOS 26.6.1 and iPadOS 26.6.1 were released with patches for all these 28 vulnerabilities, as well as with fixes for an authentication issue in Telephony that could allow attackers to bypass IPSec authentication and intercept network traffic.

The fresh iOS 26.6.1 and iPadOS 26.6.1 updates are likely setting up the stage for the iOS 27 and iPadOS 27 releases, which are expected to arrive next month.

On Monday, Apple also announced fresh updates for iOS 18.7.10 and iPadOS 18.7.10 that squash over 120 bugs, including more than 40 in WebKit.

In addition to crashes, memory corruption, and data disclosure, these flaws could also lead to sandbox escape and cross-origin data exfiltration.

The update also resolves 18 vulnerabilities in Kernel that could be exploited to corrupt kernel memory, crash the system, disclose kernel memory, bypass network filters, write kernel memory, leak sensitive kernel state, and access sensitive user data.

Security defects were also addressed in Accessibility, AirDrop, App Store, AVEVideoEncoder, Contacts, CoreAudio, CoreMedia, Foundation, ImageIO, IOSkywalkFamily, Maps, MediaRemote, Model I/O, SceneKit, Siri, WebRTC, and other components.

Apple makes no mention of any of these vulnerabilities being exploited in the wild, but users are advised to apply the patches as soon as possible. Additional information can be found on Apple’s security updates page.

**Related:** Recent macOS Screen Sharing Vulnerability Exploited in Attacks

**Related:** Critical SAP Commerce Cloud Vulnerability Exploited 3 Days After Disclosure

**Related:** Hackers Exploiting Unpatched GeoServer Zero-Day

**Related:** Adobe Commerce Bug Targeted Immediately After Disclosure
