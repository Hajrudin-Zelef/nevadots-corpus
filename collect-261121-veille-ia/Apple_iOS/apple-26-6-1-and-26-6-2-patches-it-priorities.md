---
id: collect-261121-veille-ia/apple-ios/apple-26-6-1-and-26-6-2-patches-it-priorities
title: "Apple 26.6.1 and 26.6.2 patches: IT priorities"
domain: apple-ios
role: reference
task: article
actors: ["Apple"]
dates: []
keywords: ["apple", "safari", "tahoe", "ipados", "macos", "ipsec", "sonoma", "sequoia"]
source: https://apple.underside.be/en/article-correctifs-apple-2661-2662-entreprise-2026
source_anchor: ""
source_lines: [1, 41]
section: "Apple_iOS"
---
# Apple 26.6.1 and 26.6.2 patches: prioritize enterprise fleet patching

Apple released iOS and iPadOS 26.6.1 and macOS Tahoe 26.6.2 on August 17, followed by Safari 26.6.1 on August 18, 2026. The detailed security advisories published on August 20 warrant a coordinated campaign: some flaws affect everyday content such as images and web pages, while another concerns IPSec authentication on iPhone.

## 1. Three releases, several attack surfaces

iOS 26.6.1 and iPadOS 26.6.1 fix, among other issues, an information leak through Audio, denial of service and arbitrary code execution when processing an image, memory corruption in IOGPUFamily and the kernel, and multiple WebKit defects. On iPhone, Apple also fixed a Telephony flaw: an attacker in a privileged network position could bypass IPSec authentication and intercept traffic.

macOS Tahoe 26.6.2 includes several of the same fixes, covering ImageIO, IOGPUFamily, the kernel, and WebKit. For Macs still running Sonoma or Sequoia, Safari 26.6.1 supplies the corresponding WebKit fixes, including memory corruption issues and a potential history data leak. Apple does not say that these CVEs were actively exploited, so technical severity should not be presented as evidence of an incident.

## 2. The scope extends beyond devices on the latest OS

The target fleet splits into three groups: iPhones and iPads compatible with 26.6.1, Tahoe Macs moving to 26.6.2, and Sonoma or Sequoia Macs whose Safari version must reach 26.6.1. Apple also released iOS and iPadOS 18.7.10 for certain older devices; those devices belong in a separate deployment ring rather than disappearing from reports because they cannot run iOS 26.

This distinction matters in Jamf or any other MDM. A rule based only on the macOS version does not prove that Safari was patched on older releases. Conversely, the presence of Safari 26.6.1 does not replace macOS Tahoe 26.6.2 on a compatible Mac.

## 3. What changes for a Belgian or French business?

For an SMB, the priority is to identify devices that are actually exposed and avoid a manual campaign with no proof of outcome. For a mid-market company, large enterprise, or public institution in Belgium or France, MDM inventory, Safari versions, VPN/IPSec access, mobile populations, and business exceptions need to come together in one compliance view.

The Telephony fix deserves particular attention for iPhones using an IPSec VPN on untrusted networks: travel, guest Wi-Fi, hotels, or partner access. It does not prove that an enterprise VPN was compromised, but it supports shortening the deployment window, testing critical tunnels, and monitoring authentication failures after the update.

## 4. Underside analysis: manage by surface and evidence

Our assessment is that a single “OS up to date” indicator is insufficient. The campaign should generate at least four pieces of evidence: a compliant OS version, a compliant Safari version on Sonoma and Sequoia, working apps that process images or embedded web content, and validated VPN access on affected iPhones. Every deferral needs an owner and an expiry date.

This work extends our Apple declarative update framework and the dedicated analysis of Safari 26.6 on Sonoma and Sequoia. The former structures MDM deployment rings; the latter highlights that the browser has its own compliance level.

## 5. Recommended deployment plan

- Segment iOS/iPadOS 26, iOS/iPadOS 18, macOS Tahoe, and Safari on Sonoma/Sequoia in the MDM inventory.
- Test 26.6.1 and 26.6.2 in a pilot ring with business apps, image content, embedded browsers, and IPSec VPN.
- Deploy quickly to mobile or exposed populations, then expand with a measurable deadline.
- Track Safari separately on Macs that are not yet moving to Tahoe.
- Record incompatibilities, deferrals, the risk owner, and a reassessment date.
- Export final compliance evidence from Jamf or the MDM instead of stopping when the command is sent.

Objective: turn three Apple updates into one segmented, demonstrable campaign across every device.

Structure your Apple patching
Official sources: iOS 26.6.1 and iPadOS 26.6.1 security content, macOS Tahoe 26.6.2 security content, and Safari 26.6.1 security content (Apple, documents published August 20, 2026).
