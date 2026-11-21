---
id: collect-261121-veille-ia/apple-ios/ios-vulnerabilities-2026-fixed-versions-by-iphone-model
title: "iOS Vulnerabilities 2026: Fixed Versions by iPhone Model"
domain: apple-ios
role: reference
task: article
actors: ["Apple"]
dates: ["2026-11-17"]
keywords: ["iphone", "apple", "security", "notes", "fixed", "version", "later", "none"]
source: https://mihata.jp/en/column/iphone-ios-vulnerability-updates
source_anchor: ""
source_lines: [1, 99]
section: "Apple_iOS"
---
Heard about an "iOS vulnerability" and want to know **which version your iPhone needs to be safe**? This page lists the iOS update each iPhone model should be on, and every 2026 flaw that Apple said "may have been exploited." It is updated whenever Apple ships a new security release.

Everything here comes from Apple's own security releases list and the security notes for each version. Dates are Apple's publication dates (US time). **Last checked: November 17, 2026**

## Update history

- **November 17, 2026** : Page published with iOS 27.2.1 / 26.7.3 / 18.7.12 (November 9) as the latest releases.

## Quick answer: iPhone 11 and later need iOS 27.2.1; iPhone XS and XR need iOS 18.7.12

**As of November 17, 2026, every iOS vulnerability Apple published in 2026 is fixed in the versions below.** If you are on one of these, there is nothing urgent to do.

| iPhone model | Version to install (date) | Notes | 
|---|---|---|
| iPhone 11 and later | iOS 27.2.1 (Nov 9) | If you are staying on iOS 26, iOS 26.7.3 (same day) fixes the same issues | 
| iPhone XS / XS Max / XR | iOS 18.7.12 (Nov 9) | These models cannot run iOS 26 or later; Apple keeps patching the 18.7 line | 
| iPhone 8 / 8 Plus / X | iOS 16.7.16 (May 11) | The only 2026 fix for this line (deleted notifications could be retained) | 
| iPhone 6s / 7 / SE (1st gen) | iOS 15.8.8 (May 11) | Still patched, but rarely. Consider retiring these from sensitive work | 

iPads get the same fixes in the matching iPadOS version (iPadOS 27.2.1 covers iPad Pro 12.9-inch 4th gen and later, iPad 9th gen and later, iPad mini 6th gen and later, and others). The exact list is under "Available for" in Apple's iOS 27.2.1 security notes.

## iOS flaws Apple said "may have been exploited" in 2026

Apple only adds this line when it has a report of real-world attacks: "Apple is aware of a report that this issue may have been exploited in an extremely sophisticated attack against specific targeted individuals." In 2026 it appeared for two iOS flaws. **Neither affects iOS 27.**

| CVE | Component | Versions reported as targeted | Fixed in (date) | 
|---|---|---|---|
| CVE-2026-86950 | CoreGraphics (a crafted file could run arbitrary code) | Before iOS 27 | iOS 26.7.1 (Sep 28), iOS 18.7.11 (Oct 26) | 
| CVE-2026-20700 | dyld (an attacker with memory write capability could run arbitrary code) | Before iOS 26 | iOS 26.3 (Feb 11) | 

Sources: iOS 26.7.1 security notes / iOS 18.7.11 security notes / iOS 26.3 security notes. "Specific targeted individuals" means these were not mass attacks. Even so, a fix with an exploitation note should be installed **the day it ships**.

## Recent releases and how many issues they fixed

Counts are the number of fixed items (Impact entries) in each version's security notes.

| Date | Version | Fixed items | Exploitation note | 
|---|---|---|---|
| Nov 9 | iOS 27.2.1 | 9 | None | 
| Nov 9 | iOS 26.7.3 | 8 | None | 
| Nov 9 | iOS 18.7.12 | 6 | None | 
| Oct 26 | iOS 27.2 | 141 | None | 
| Oct 26 | iOS 26.7.2 | 115 | None | 
| Oct 26 | iOS 18.7.11 | 3 | Yes (CVE-2026-86950) | 
| Sep 28 | iOS 26.7.1 | 1 | Yes (CVE-2026-86950) | 
| Sep 14 | iOS 27 | 157 | None | 

Apple lists iOS 27.0.1 (Sep 28) and iOS 27.1 (Oct 23, iPhone Duo only) as having "no published CVE entries," so they contain no security fixes.

### iOS 27.2.1 (Nov 9): nine Bluetooth and iMessage fixes

Per Apple's iOS 27.2.1 security notes, the nine fixes fall into three groups:

- **From Bluetooth range** : bypassing the pairing-code attempt limit, pairing without user interaction, and memory corruption (AirPlay, Bluetooth, CoreUtils)
- **From the same network** : bypassing the AirPlay pairing-code attempt limit (rapportd)
- **iMessage groups** : a remote attacker joining a group chat and receiving its messages, and a newly added member reading messages sent before they joined

The rest are a memory corruption fix in brotli (compression) and an App Intents issue that could let an app access sensitive data. For whether to install it right away, see should you update to iOS 27.2.1.

## How to check your iOS version and update

1. **Check your version** : Settings → General → About → iOS Version.
2. **Compare it with the table above** : the same or newer means you are covered.
3. **Update** : Settings → General → Software Update. Stay on Wi-Fi and plugged in so it does not stall.
4. **Turn on Automatic Updates** on the same screen so future fixes install without you looking for them.

A major jump from iOS 26 to 27 takes longer; see how long the iOS 27 update takes. The next release is tracked in iOS 27.3 release date.

## If you manage company iPhones

- **Push fixes with an exploitation note first.** For other releases, testing business apps on a few devices before rolling out to all is usually enough.
- **Devices held on iOS 26** still get 26.7.x fixes. If you cannot move to 27, keep them on the latest 26 (currently 26.7.3).
- **Models older than XS / XR** get fixes rarely; move them off work that touches sensitive data.

## Summary

- iPhone 11 and later: iOS 27.2.1 (or 26.7.3 on iOS 26). iPhone XS / XR: iOS 18.7.12. These cover every iOS flaw Apple published in 2026.
- Two 2026 flaws carried an exploitation note, CVE-2026-86950 (CoreGraphics) and CVE-2026-20700 (dyld); neither affects iOS 27.
- Check Settings → General → About; if you are behind, update and turn on Automatic Updates.

We also help small teams sort out device management and day-to-day security. If something here raised a question for your workplace, we would be glad to hear from you.

## FAQ

## Does iOS 27.2.1 fix any actively exploited vulnerability?

No. None of the nine fixes in Apple's iOS 27.2.1 security notes (November 9) carries an exploitation note.

## Am I affected by the exploited 2026 iOS flaws if I am on iOS 27?

No. Apple says CVE-2026-86950 was exploited on versions before iOS 27 and CVE-2026-20700 on versions before iOS 26. iOS 27 is not affected.

## Are iPhone XS and XR still getting security updates?

Yes. iOS 18.7.12 was released on November 9, 2026 and fixes six vulnerabilities.

## Where do I check my iPhone's iOS version?

Go to Settings, General, About and look at iOS Version. To update, go to Settings, General, Software Update.
