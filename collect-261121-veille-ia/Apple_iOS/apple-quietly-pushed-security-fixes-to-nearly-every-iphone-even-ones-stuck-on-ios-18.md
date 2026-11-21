---
id: collect-261121-veille-ia/apple-ios/apple-quietly-pushed-security-fixes-to-nearly-every-iphone-even-ones-stuck-on-ios-18
title: "Apple quietly pushed security fixes to nearly every iPhone, even ones stuck on iOS 18"
domain: apple-ios
role: reference
task: article
actors: ["Apple"]
dates: []
keywords: ["iphone", "apple", "security", "fixes", "pairing", "updates", "every", "apples"]
source: https://morningoverview.com/apple-quietly-pushed-security-fixes-to-nearly-every-iphone-even-ones-stuck-on-ios-18/
source_anchor: ""
source_lines: [1, 38]
section: "Apple_iOS"
---
Apple’s November 9 software update arrived with a one-line note promising nothing more than bug fixes and security updates. Behind that line were nine security fixes, including one that could let a stranger slip into an iMessage group chat, and Apple pushed versions of them not just to the newest iPhones but to models that cannot run its latest software.

**Read next on Morning Overview:** Arizona’s governor declared a statewide emergency after Flagstaff logged its wettest Nov. 1 on record

The company released iOS 27.2.1 for current devices, plus iOS 26.7.3 for those staying on last year’s system and iOS 18.7.12 for the iPhone XS, iPhone XS Max and iPhone XR, which are stuck on iOS 18. Between them, the three updates reach nearly every iPhone Apple still supports.

## Three updates for three generations of iPhone

Apple’s security releases page lists all three iOS updates on November 9. iOS 27.2.1 and iOS 26.7.3 cover the iPhone 11 and later, while iOS 18.7.12 is for the iPhone XS, iPhone XS Max and iPhone XR, plus the 7th-generation iPad. Apple shipped matching updates the same day for the Mac, Apple Watch and Vision Pro, including macOS 27.2.1 and backports for macOS Tahoe 26.7.3.

The release notes users see on their phones gave no detail. “This is the boilerplate text for security releases,” Macworld senior editor Jason Cross wrote in Macworld’s report on the updates, noting that Apple’s security page had not yet listed the specific vulnerabilities when the updates first appeared. The detailed list appeared afterward. Apple routinely withholds those details until fixes are available, in line with its stated policy of not disclosing security issues before patches ship.

## An iMessage group-chat flaw and a cluster of pairing bugs

Apple’s security notes for iOS 27.2.1 list nine fixes across seven system components. The two that apply most directly to everyday users involve Messages. CVE-2026-113961 could let a remote attacker join an iMessage group chat and receive its messages; Apple fixed the authorization problem with improved validation. CVE-2026-113960 let someone added to a group read messages sent before they joined, and Apple removed the vulnerable code, according to iThinkDiff’s breakdown. Both were reported by an anonymous researcher.

Most of the remaining fixes deal with pairing. An AirPlay bug, CVE-2026-113968, let an attacker within Bluetooth range get around the limit on how many pairing codes can be tried, and a flaw in the rapportd service, CVE-2026-113969, allowed the same from the same network. Two CoreUtils bugs went further: CVE-2026-113965 let a malicious Bluetooth device pair without any user interaction, and CVE-2026-113964 combined the rate-limit bypass with no-interaction pairing. A separate Bluetooth bug, CVE-2026-113970, could let a nearby attacker corrupt memory. An App Intents flaw that could expose sensitive user data and a memory bug in the brotli compression library round out the list.

Apple did not say any of the flaws had been exploited in the wild, iPhone Wired noted, citing MacRumors. Apple credited its own engineers with four of the nine reports. The update followed iOS 27.2 on October 26, which patched 163 flaws.

Pairing bugs sound minor next to an iMessage leak, but they matter because pairing is how an iPhone decides which nearby devices it trusts. A rate limit on pairing codes is what stops an attacker from simply guessing every possible code, and a prompt is what gives the owner a chance to say no. Removing either safeguard turns proximity, such as sitting near someone on a train or in a cafe, into an opening. Apple fixed all four pairing bugs with improved state management or improved checks, iThinkDiff reported.

## Why older iPhones still get patches

The iPhone XS, XS Max and XR, released in 2018, cannot run iOS 26 or iOS 27, which require an iPhone 11 or later. Apple keeps shipping security fixes to such devices on the older system for a period after they lose feature updates, which is why iOS 18.7.12 exists. For owners of those phones, it is the only way to get the protections the newest models received.

The pace is unlikely to slow. Cross wrote that frequent releases fixing dozens of vulnerabilities are “the new normal,” because the latest AI coding tools make it much easier both to find and exploit security holes and to find and fix them. “The race is on,” he wrote, predicting that larger security updates will be a regular occurrence for at least a year.

Installing the update takes a few steps: Settings, General, Software Update. The version number on that screen should read 27.2.1, 26.7.3 or 18.7.12, depending on the phone. Once the details of a fix are public, devices that have not updated are more exposed than before, because anyone can study what was changed. Automatic updates, which can be turned on from the same screen, install new security releases overnight without waiting for a reminder.

*This article was produced with the assistance of AI and reviewed by Morning Overview editors prior to publication.*

**More from Morning Overview**

- **11 engines built to run well past 200,000 miles**
- **Two Kentucky residents died when a small plane crashed at Missouri’s Lake of the Ozarks, and the NTSB is investigating**
- **A handful of car transmissions are so tough that mechanics say they almost never die**
- **Melting permafrost is setting off a newly identified kind of catastrophic landslide, scientists say**
