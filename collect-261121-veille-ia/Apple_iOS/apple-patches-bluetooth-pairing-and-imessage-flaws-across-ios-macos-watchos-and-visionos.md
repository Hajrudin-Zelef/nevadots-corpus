---
id: collect-261121-veille-ia/apple-ios/apple-patches-bluetooth-pairing-and-imessage-flaws-across-ios-macos-watchos-and-visionos
title: "Apple Patches Bluetooth Pairing and iMessage Flaws Across iOS, macOS, watchOS and visionOS"
domain: apple-ios
role: reference
task: article
actors: ["Apple"]
dates: ["2026-11-09"]
keywords: ["bluetooth", "pairing", "watchos", "later", "macos", "ipad", "ipados", "apple"]
source: https://beyondmachines.net/event_details/apple-patches-bluetooth-pairing-and-imessage-flaws-across-ios-macos-watchos-and-visionos-5-e-e-u-f/gD2P6Ple2L
source_anchor: ""
source_lines: [1, 44]
section: "Apple_iOS"
---
# Apple Patches Bluetooth Pairing and iMessage Flaws Across iOS, macOS, watchOS and visionOS

On November 9, 2026, Apple released iOS 27.2.1, iPadOS 27.2.1, iOS 26.7.3, iPadOS 26.7.3, iOS 18.7.12, iPadOS 18.7.12, macOS Golden Gate 27.2.1, macOS Tahoe 26.7.3, macOS Sequoia 15.8.3, watchOS 27.2.1, watchOS 26.6.1 and visionOS 27.2.1.

The updates patch 10 distinct CVEs. Most of them affect Bluetooth and AirPlay pairing (the AirPlay, Bluetooth, CoreUtils and rapportd components) and the Messages app. Five Bluetooth and pairing fixes ship on every platform in this release. The other fixes depend on the version: the Messages and brotli fixes are missing from iOS 18.7.12 and watchOS 26.6.1. The App Intents fix ships only in the 27.2.1 releases of iOS, watchOS and visionOS. The MediaRemote fix ships only in iOS 18.7.12.

The Bluetooth flaws require the attacker to be physically nearby. This limits mass exploitation, and the flaws stay practical in airports, conferences, offices and public transport. A silent pairing gives the attacker a trusted link to the device.

**Bluetooth and AirPlay pairing issues**

Five of the fixes target device pairing. Combined, they let an attacker within Bluetooth range or on the same network brute-force pairing codes and pair with a device without the user’s approval. These fixes ship on all platforms in this release:

- **CVE-2026-113965** (CoreUtils) - A malicious Bluetooth device may be able to establish a pairing without user interaction
- **CVE-2026-113964** (CoreUtils) - An attacker in Bluetooth range may be able to bypass the rate limit on pairing code attempts and pair with a device without user interaction
- **CVE-2026-113970** (Bluetooth) - Use-after-free allowing an attacker in Bluetooth range to corrupt process memory
- **CVE-2026-113968** (AirPlay) - An attacker in Bluetooth range may be able to bypass the rate limit on pairing code attempts
- **CVE-2026-113969** (rapportd) - An attacker on the same network may be able to bypass the rate limit on AirPlay pairing code attempts
- **CVE-2026-113971** (MediaRemote) - An attacker with physical access may be able to pair a device with an AirPlay receiver from the lock screen (iOS/iPadOS 18.7.12 only)

**Messages issues**

Two iMessage group chat flaws expose message content to people who should not see it. Both are fixed on all platforms in this release except iOS/iPadOS 18.7.12 and watchOS 26.6.1:

- **CVE-2026-113961** (Messages) - Authorization flaw allowing a remote attacker to join an iMessage group chat and receive its messages
- **CVE-2026-113960** (Messages) - A user added to an iMessage group may be able to access messages sent before they joined

**Other fixes**

- **CVE-2026-111497** (brotli) - Out-of-bounds write in the third-party open-source compression library, where processing maliciously crafted input may lead to memory corruption (all platforms in this release except iOS/iPadOS 18.7.12 and watchOS 26.6.1)
- **CVE-2026-113963** (App Intents) - Authorization flaw allowing an app to access sensitive user data (iOS/iPadOS 27.2.1, watchOS 27.2.1 and visionOS 27.2.1 only)

Updated OS versions:

- iOS 27.2.1 / iPadOS 27.2.1 is available for iPhone 11 and later; iPad Pro 12.9-inch (4th gen and later), iPad Pro 11-inch (2nd gen and later), iPad Air (4th gen and later), iPad (9th gen and later), and iPad mini (6th gen and later)
- iOS 26.7.3 / iPadOS 26.7.3 for iPhone 11 and later; iPad Pro 12.9-inch (3rd gen and later), iPad Pro 11-inch (1st gen and later), iPad Air (3rd gen and later), iPad (8th gen and later), and iPad mini (5th gen and later)
- iOS 18.7.12 / iPadOS 18.7.12 for iPhone XS, iPhone XS Max, iPhone XR and iPad (7th gen)
- macOS Golden Gate 27.2.1, macOS Tahoe 26.7.3 and macOS Sequoia 15.8.3 for the three supported macOS generations
- watchOS 27.2.1 for Apple Watch Series 9 and later
- watchOS 26.6.1 for Apple Watch Series 6, Series 7, Series 8, SE (2nd gen) and Ultra
- visionOS 27.2.1 for Apple Vision Pro (all models)

Apple did not indicate that any of these issues have been exploited in the wild.

Users on iOS 18.7.12 and watchOS 26.6.1 do not receive the iMessage fixes. All users are advised to update as soon as possible.
