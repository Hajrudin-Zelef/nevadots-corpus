---
id: collect-261121-veille-ia/apple-ios/apple-fixes-imessage-flaws-that-exposed-group-chat-privacy
title: "Apple Fixes iMessage Flaws That Exposed Group Chat Privacy"
domain: apple-ios
role: reference
task: article
actors: ["Apple"]
dates: []
keywords: ["imessage", "fixes", "apple", "group", "could", "flaws", "brotli", "updates"]
source: https://osintsights.com/apple-fixes-imessage-flaws-that-exposed-group-chat-privacy
source_anchor: ""
source_lines: [1, 38]
section: "Apple_iOS"
---
"An iMessage flaw could let a remote attacker join a group chat and receive its messages, Apple said yesterday."

## CVE-2026-113961 and CVE-2026-113960: two iMessage privacy defects

Apple disclosed two distinct iMessage defects that affect group conversations. The first, tracked as CVE-2026-113961, is an authorization issue that "could let a remote attacker join a group chat and receive its messages," and Apple says the update fixes it through improved validation. The second, CVE-2026-113960, could allow "someone added to a group [to] access messages sent before they joined."

Apple released updates that include fixes for both flaws, but the company’s advisory does not say whether either flaw has been exploited, what an attacker would specifically need to do to exploit them, whether group members would see someone who joined this way, or whether every member of a group chat must update to be protected.

## CVE-2026-111497: Brotli decoder out-of-bounds write and CISA listing

The iMessage fixes were bundled with a separate correction for a flaw in Google's Brotli compression library, tracked as CVE-2026-111497. Apple’s notes characterize the Brotli issue as "an out-of-bounds write in the library's decoder that could let malicious input corrupt memory." The U.S. Cybersecurity and Infrastructure Security Agency (CISA) added that Brotli decoder flaw to its Known Exploited Vulnerabilities catalog on October 27, and CISA’s catalog entry explicitly names Google's Brotli as the affected product while noting that different products may use the component.

Apple’s release notes do not mention any observed attacks using CVE-2026-111497.

### Audit-ready is a season. It shouldn't be.

Evidence in spreadsheets, controls drifting between audits, frameworks multiplying on flat headcount. Nubivance runs continuous compliance on Rapid7 Cyber GRC - SOC 2, HIPAA, ISO 27001, PCI, CMMC.

End the scramble
## Bluetooth and AirPlay pairing vulnerabilities across nine updates

All nine updates Apple published in this cycle address multiple Bluetooth and AirPlay vulnerabilities. The advisory lists several distinct pairing and memory-corruption issues. Two Bluetooth flaws could allow a malicious Bluetooth device, or "an attacker within Bluetooth range," to pair with an Apple device without user interaction. Apple’s notes do not specify what a paired device could then do.

Two other bugs could let an attacker bypass the limit on pairing-code attempts — one from Bluetooth range and another from the same network — and an additional Bluetooth-range flaw could allow an attacker to corrupt process memory. These Bluetooth and AirPlay fixes were included in the same set of updates that corrected the iMessage and Brotli problems.

## Which releases include the fixes — and which do not

Each operating system version received its own update. The security advisory for iOS and iPadOS 27.2.1 describes all three flaws (the two iMessage issues and the Brotli decoder flaw). The security releases page links to the notes for every update.

However, as of today the notes for some other updates released yesterday — specifically iOS 18.7.12 and watchOS 26.6.1 — list other security fixes but do not mention the iMessage or Brotli corrections. Separately, updates released yesterday for the iPhone XS, XS Max and XR, the iPad (7th generation) and some Apple Watch models also do not list either iMessage fix. Apple’s documentation does not say whether devices running those versions are affected by the three flaws, nor does it offer any workaround for users who cannot update right away.

## What this means for security teams, end users, and CISA

- Security teams and technologists: Prioritize review of the iOS and iPadOS 27.2.1 advisory, which documents all three flaws, and evaluate inventory to determine which devices received the fixes. The absence of fixes listed in iOS 18.7.12, watchOS 26.6.1 and updates for older models requires careful tracking of which endpoints remain exposed.
- End users and device owners: Apply updates where available. Note that Apple’s release notes do not provide a workaround for those who cannot update immediately, and some updates for older hardware do not list the iMessage or Brotli fixes as of today.
- CISA and regulators: CISA has already added the Brotli decoder issue (CVE-2026-111497) to its Known Exploited Vulnerabilities catalog, highlighting the componentized risk that a widely reused library poses when incorporated into multiple products.

Apple has issued patches that address a trio of messaging, compression-library, and Bluetooth/AirPlay vulnerabilities, but its published notes leave several practical questions open — including whether any of the flaws have been observed in the wild, what an attacker would need to exploit them, and how updates on different devices affect group-chat protections. Organizations and users should match installed versions to Apple’s advisory and prioritize devices that have the documented fixes. The next concrete signals to watch will be updated release notes for the older-device updates and any public reports of exploitation tied to the CVE identifiers listed above.
