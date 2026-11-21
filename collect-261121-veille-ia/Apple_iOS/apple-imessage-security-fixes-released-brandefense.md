---
id: collect-261121-veille-ia/apple-ios/apple-imessage-security-fixes-released-brandefense
title: "Apple iMessage Security Fixes Released - Brandefense"
domain: apple-ios
role: reference
task: article
actors: ["Apple"]
dates: ["2026-11-09"]
keywords: ["apple", "imessage", "bluetooth", "security", "fixes", "ipad", "devices", "flaws"]
source: https://brandefense.io/security-news/apple-imessage-bluetooth-security-fixes/
source_anchor: ""
source_lines: [1, 54]
section: "Apple_iOS"
---
**Apple iMessage security fixes** in iOS 26.7.3 and iPadOS 26.7.3 address two group-chat privacy flaws. The November 9 update also fixes separate Bluetooth, AirPlay, and memory safety issues.

Apple lists the release for iPhone 11 and later and several supported iPad generations. Teams should install the update, confirm the running version, and keep the iMessage and proximity-based risks separate during triage.

## What Do the Apple iMessage Security Fixes Change?

CVE-2026-113960 could let a newly added iMessage group member access messages sent before they joined. Apple removed the vulnerable code to address that privacy boundary failure.

CVE-2026-113961 is a separate authorization issue. Apple says a remote attacker may be able to join an iMessage group chat and receive its messages.

These Messages flaws do not depend on Bluetooth pairing. Administrators should treat unexpected group membership and access to earlier messages as distinct privacy scenarios, not one attack chain.

## Which Apple Versions and Devices Are Covered?

| Platform | Fixed release | Apple’s covered devices | 
|---|---|---|
| iPhone | iOS 26.7.3 | iPhone 11 and later | 
| iPad | iPadOS 26.7.3 | See the supported-model list below | 

Apple covers the 12.9-inch iPad Pro from its third generation and the 11-inch iPad Pro from its first generation. Coverage also starts with iPad Air 3, iPad 8, and iPad mini 5.

Apple released both updates on November 9, 2026. The same seven CVEs also appear in Apple’s iOS 27.2.1, macOS Golden Gate 27.2.1, and macOS Tahoe 26.7.3 security notes.

Organizations should follow their normal security patch process for supported Apple devices. A vulnerability management workflow can track devices that miss the initial rollout.

## How Do the Bluetooth and AirPlay Risks Differ?

CVE-2026-113964 may let an attacker in Bluetooth range bypass pairing-code rate limits and pair without user interaction. CVE-2026-113965 may let a malicious Bluetooth device establish a pairing without user interaction.

CVE-2026-113968 concerns an AirPlay pairing-code rate limit bypass from Bluetooth range. CVE-2026-113969 concerns an AirPlay pairing-code rate limit bypass by an attacker on the same network.

CVE-2026-113970 is different again. Apple describes a Bluetooth use-after-free issue that may let an attacker in Bluetooth range corrupt process memory.

Brandefense Blackhole recorded one dark-chat post for each of CVE-2026-113960, CVE-2026-113961, and CVE-2026-113964 during the 90-day review window. This limited chatter is a prioritization signal, not evidence of exploitation.

## Do This Now for Apple Devices

Prioritize devices used for sensitive communications or frequently exposed to shared networks and public spaces. Record update exceptions and give each owner a follow-up date.

Apple’s advisory does not report active exploitation for these seven CVEs. If device evidence suggests unauthorized access, preserve relevant records and follow the organization’s incident response plan.

## Frequently Asked Questions

## Which updates contain the Apple iMessage security fixes?

Apple lists the fixes in iOS 26.7.3 and iPadOS 26.7.3, released November 9, 2026.

## Do the iMessage flaws require Bluetooth access?

No. Apple's advisory describes the two Messages issues separately from the Bluetooth and AirPlay flaws.

## Has Apple reported active exploitation of these flaws?

No. Apple's security note does not report active exploitation for the seven CVEs covered here.
