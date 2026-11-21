---
id: collect-261121-veille-ia/apple-ios/apple-patches-imessage-group-chat-privacy-flaws-and-bluetooth-pairing-vulnerabilities
title: "Apple Patches iMessage Group Chat Privacy Flaws and Bluetooth Pairing Vulnerabilities"
domain: apple-ios
role: reference
task: article
actors: ["Apple"]
dates: []
keywords: ["flaw", "could", "bluetooth", "imessage", "group", "updates", "notes", "flaws"]
source: https://thehackernews.com/2026/11/apple-patches-imessage-group-chat.html
source_anchor: ""
source_lines: [1, 23]
section: "Apple_iOS"
---
An iMessage flaw could let a remote attacker join a group chat and receive its messages, Apple said yesterday, and it released updates to fix it. The updates also fix a separate iMessage flaw that could let someone added to a group access messages sent before they joined.

As of today, other updates released yesterday for the iPhone XS, XS Max and XR, the iPad (7th generation) and some Apple Watch models do not list either fix.

The company's notes also do not say whether either flaw has been exploited, what an attacker would need, whether group members would see someone who joined this way, or whether every member must update to be protected.

The updates that fix the iMessage flaws also fix a flaw in Google's Brotli compression library. The notes do not mention any attacks using that flaw. The U.S. Cybersecurity and Infrastructure Security Agency (CISA) added it to its Known Exploited Vulnerabilities catalog on October 27.

The flaw that could let a remote attacker join a group chat is tracked as CVE-2026-113961. It is an authorization issue that the updates fix through improved validation. The flaw involving earlier messages is CVE-2026-113960.

### Which Updates Carry the Fixes

Each operating system version got its own update. The security advisory for iOS and iPadOS 27.2.1 describes all three flaws. The security releases list links to the notes for every update.

The iOS 18.7.12 notes and the watchOS 26.6.1 notes list other security fixes but not the iMessage or Brotli fixes.

The company does not say whether devices on those versions are affected by the three flaws. Its notes also offer no workaround for anyone who cannot update right away.

The Brotli flaw, CVE-2026-111497, is an out-of-bounds write in the library's decoder that could let malicious input corrupt memory. CISA's catalog entry for the Brotli decoder flaw names Google's Brotli as the affected product and says different products could use the component.

All nine updates fix Bluetooth and AirPlay flaws. Two could allow a malicious Bluetooth device, or an attacker within Bluetooth range, to pair with a device without user interaction. The notes do not say what a paired device could then do.

Two others could let an attacker bypass the limit on pairing code attempts, one from Bluetooth range and one from the same network. Another could let an attacker in Bluetooth range corrupt process memory.
