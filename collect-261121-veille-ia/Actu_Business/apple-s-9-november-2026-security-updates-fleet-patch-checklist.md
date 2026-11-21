---
id: collect-261121-veille-ia/actu-business/apple-s-9-november-2026-security-updates-fleet-patch-checklist
title: "Apple's 9 November 2026 security updates: fleet patch checklist"
domain: actu-business
role: reference
task: article
actors: []
dates: []
keywords: ["apple", "ipad", "later", "november", "macos", "iphone", "security", "updates"]
source: https://ecorpit.com/apple-9-november-2026-security-updates-ios-macos-patching-checklist/
source_anchor: ""
source_lines: [1, 131]
section: "Actu_Business"
---
**Summary.** On 9 November 2026 Apple released security updates for three generations of its operating systems at once: iOS and iPadOS 27.2.1, 26.7.3 and 18.7.12, and macOS Golden Gate 27.2.1, macOS Tahoe 26.7.3 and macOS Sequoia 15.8.3, plus watchOS 27.2.1, watchOS 26.6.1 and visionOS 27.2.1 the same day. iOS 27.2.1 fixes 9 CVEs, iOS 26.7.3 and each macOS update fix 8, and iOS 18.7.12 fixes 6. Apple does not mark any of them as exploited. One of them, the Brotli decoder flaw CVE-2026-111497, is already in CISA's Known Exploited Vulnerabilities catalogue, added on 27 October with a federal due date of 10 November, and NVD scores it 9.8. The highest-scored Apple-specific bug is a Bluetooth use-after-free, CVE-2026-113970, rated 8.8 in CISA's enrichment of the CVE record. Our verdict: treat this as a priority update. Put every iPhone, iPad and Mac on the right version line, pilot for two days, and enforce the rest by a dated deadline. Under Cyber Essentials, an update fixing issues scored 7 or above must be installed within 14 days of release, which for these builds means by 23 November 2026.

**About this article.** For IT managers, endpoint and security teams in the US, UK and India who run mixed Apple fleets through a device management service. We checked every version number, device list, CVE and date on 17 November 2026 against Apple's security releases page and the individual security notes, the CVE records, NVD, the CISA KEV feed, Apple's software lookup service, Apple's Platform Deployment guide and the NCSC's Cyber Essentials requirements. This is defensive guidance, not legal advice; Apple may add entries to its notes later, so recheck before you close the change. No vendor paid for inclusion. Researched with AI assistance and checked against the primary sources listed in the references.

## What Apple shipped on 9 November 2026

Apple's security releases page lists the six updates in this article with a release date of 9 November 2026, and on 17 November it gives 27.2.1 as the latest version of iOS, iPadOS and macOS. The same page lists watchOS 27.2.1, watchOS 26.6.1 and visionOS 27.2.1 on 9 November, HomeView software 27.2 on 12 November and tvOS 27.2.1 on 13 November.

The three iPhone and iPad lines exist because of hardware support. Each note names the devices that can install it.

| Update | Devices | CVEs fixed | 
|---|---|---|
| iOS and iPadOS 27.2.1 | iPhone 11 and later; iPad Pro 12.9-inch 4th gen and later, iPad Pro 11-inch 2nd gen and later, iPad Air 4th gen and later, iPad 9th gen and later, iPad mini 6th gen and later | 9 | 
| iOS and iPadOS 26.7.3 | iPhone 11 and later; iPad Pro 12.9-inch 3rd gen and later, iPad Pro 11-inch 1st gen and later, iPad Air 3rd gen and later, iPad 8th gen and later, iPad mini 5th gen and later | 8 | 
| iOS and iPadOS 18.7.12 | iPhone XS, iPhone XS Max, iPhone XR, iPad 7th gen | 6 | 
| macOS Golden Gate 27.2.1 | Macs on macOS Golden Gate | 8 | 
| macOS Tahoe 26.7.3 | Macs on macOS Tahoe | 8 | 
| macOS Sequoia 15.8.3 | Macs on macOS Sequoia | 8 | 

The CVE counts come from the individual notes for each update. Two practical points follow. An iPhone 11 or later can sit on either 26.7.3 or 27.2.1, so the line it lands on is your decision, not the hardware's. And the iPhone XS, XS Max, XR and the 7th-generation iPad can only take 18.7.12.

Wearables and headsets need the same attention. watchOS 27.2.1 fixes 9 CVEs for Apple Watch Series 9 and later, watchOS 26.6.1 fixes 5 for Series 6 to 8, SE (2nd generation) and Apple Watch Ultra, and visionOS 27.2.1 fixes 9 for all Vision Pro models.

## The ten CVEs, and which update fixes which

Ten distinct CVEs appear across the six iPhone, iPad and Mac updates. Apple's notes describe the impact of each in one line; the CVSS 3.1 scores below come from CISA's enrichment of the CVE records for the Apple-assigned CVEs and from NVD for Brotli.

| CVE | Component and impact (paraphrased) | Score | Fixed in | 
|---|---|---|---|
| CVE-2026-111497 | brotli: crafted input can corrupt memory | 9.8 (NVD) | 27.2.1, 26.7.3, all three macOS | 
| CVE-2026-113970 | Bluetooth: attacker in range can corrupt process memory | 8.8 | All six | 
| CVE-2026-113961 | Messages: remote attacker can join an iMessage group and receive its messages | 7.5 | 27.2.1, 26.7.3, all three macOS | 
| CVE-2026-113969 | rapportd: same-network attacker can bypass the AirPlay pairing code rate limit | 7.5 | All six | 
| CVE-2026-113960 | Messages: new group member can read earlier messages | 6.5 | 27.2.1, 26.7.3, all three macOS | 
| CVE-2026-113963 | App Intents: an app can reach sensitive user data | 5.5 | iOS 27.2.1 only | 
| CVE-2026-113964 | CoreUtils: attacker in range can bypass the pairing rate limit and pair without interaction | 5.4 | All six | 
| CVE-2026-113965 | CoreUtils: malicious Bluetooth device can pair without interaction | 5.4 | All six | 
| CVE-2026-113968 | AirPlay: attacker in range can bypass the pairing code rate limit | 5.4 | All six | 
| CVE-2026-113971 | MediaRemote: physical attacker can pair with an AirPlay receiver from the lock screen | 3.5 | iOS 18.7.12 only | 

The fixed-in column comes from comparing the six notes line by line. Three differences are worth knowing.

- **iOS 18.7.12 has no Brotli or Messages entries.** Its note lists the Bluetooth, AirPlay, CoreUtils and rapportd fixes, plus the MediaRemote fix, which the 26 line already received in iOS 26.5. The note does not say whether iOS 18 was ever affected by the Brotli or iMessage bugs, so do not read the absence as a clean bill of health. Treat XS and XR handsets as the weakest part of the fleet.

- **The App Intents fix is in iOS 27.2.1 only** among the six, though it also appears in watchOS 27.2.1, visionOS 27.2.1 and tvOS 27.2.1.

- **The three macOS notes have the same CVE list** : 8 entries each, the iOS 27.2.1 list without App Intents.

Apple's notes give the one-line impact and no technical detail, which is its normal practice: it does not discuss security issues until an investigation is done and fixes are available. We give no more detail than Apple does.

## Is anything exploited? The Brotli catch

None of Apple's six notes says that an issue may have been exploited. CISA's enrichment of the CVE record for the Bluetooth bug, CVE-2026-113970, records exploitation as none, and the same is true for the other Apple-assigned CVEs we checked.

The exception is the bundled Brotli library. CVE-2026-111497 is a Google-assigned CVE for an out-of-bounds write in the Brotli decoder, affecting Brotli 1.0.3 through 1.2.0. CISA added it to the KEV catalogue on 27 October 2026 with a due date of 10 November for US federal agencies, and its entry lists known ransomware use as unknown. NVD scores it 9.8 under CVSS 3.1 and Google scores it 9.2 under CVSS 4.0. CISA's enrichment of the CVE record marks exploitation as active.

The KEV entry describes Brotli as a third-party component used by many products, and it does not say that Apple devices were attacked. What it does tell you is that exploitation of the library exists somewhere. For a patch priority decision that is enough. Any scanner that keys on KEV membership will flag Macs and iPhones that have not taken these updates, and the federal due date passed one day after Apple shipped. Our Brotli CVE-2026-111497 guide covers the server-side copies in Node.js, .NET and Envoy, which most teams should patch alongside their Apple fleet.

## Why the older lines are not equal to the newest

Apple's deployment guide says that, because of architecture and system changes, the current major versions (iOS 27, macOS 27) contain security fixes that the previous versions do not all receive. This release shows the effect in miniature: iOS 27.2.1 has 9 fixes, iOS 26.7.3 has 8 and iOS 18.7.12 has 6.

Background Security Improvements widen the gap. Apple delivers these smaller security releases only for the latest versions of iOS, iPadOS and macOS, so a device held on the 26 line will not receive them. That makes 26.7.3 a reasonable holding position for a few months while you test apps on 27, not a long-term home. Our macOS 27 Golden Gate IT guide covers compatibility and app testing for the Mac move.

Apple also says that once an iOS, iPadOS, tvOS, watchOS or visionOS update is installed, it cannot be downgraded. Pilot before you push, because there is no rollback.

## Deferral and enforcement dates for these builds

Supervised devices can have updates held back for 1 to 90 days after Apple publishes them. For a 9 November release, the maximum 90-day deferral ends on 7 February 2027 (9 November plus 21 days is 30 November, plus 31 is 31 December, plus 31 is 31 January, plus 7 is 7 February). Apple's guide adds that over-the-air updates typically stay available for up to 180 days after release. Apple's software lookup service, which the deployment guide uses for its release-date table, lists these iOS and macOS builds as installable until 15 February 2027, a date Apple says may change when later updates ship. It dates almost every build 9 November, with one iOS 27.2.1 device group dated 13 November.

A fleet that runs a standard 30-day deferral would not see these updates offered until 9 December 2026. That is too slow for a release containing a KEV-listed CVE, and it misses the 14-day Cyber Essentials window by more than two weeks. Two controls fix that without abandoning deferrals for everything else:

- **Enforce a specific version by a date.** Under declarative device management, an enforced update applies regardless of configured deferrals. The deadline applies in each device's local time, so one declaration works across India, the UK and the US.

- **Set a minimum version at enrolment.** On iOS 17, iPadOS 17, macOS 14 and later, Automated Device Enrollment can make a device update before it finishes Setup Assistant, which stops new or reset devices joining on an old build.

If your device management service still relies on older command-based update controls, our declarative device management migration guide explains what to move.

## The rollout checklist

This is the order we suggest for a mixed fleet, starting on 17 November 2026.

1. **Inventory by version line.** Export every device with model and OS build. Group them as 27.x, 26.x, 18.x and macOS 15.x, and list any device below the 9 November builds. Flag iPhone XS, XS Max, XR and 7th-generation iPads separately, since 18.7.12 is their ceiling.

1. **Decide the target line per group.** iPhone 11 and later can take 27.2.1 or 26.7.3. Choose 27.2.1 unless a business app is not yet certified on 27, and if you hold back, record the reason and a review date.

1. **Check the update path works.** Apple services fail any connection that goes through HTTPS interception, so exempt Apple's update hosts from TLS inspection on your proxy. If many Macs share one site, a Mac with content caching turned on can cut download traffic.

1. **Pilot for two working days.** Pick 5 to 10 per cent of each group, including the people who use Bluetooth accessories, AirPlay meeting-room displays and iMessage most, since those are the areas changed.

1. **Enforce with a dated deadline.** Declare the target version with an enforcement date a few days after the pilot ends, and no later than 23 November if you hold Cyber Essentials. Users see the deadline in Settings and in notifications, Do Not Disturb is ignored in the final 24 hours, and at the deadline a Mac force-quits open apps, unsaved documents included, before restarting. Warn Mac users about that last point.

1. **Mind battery and storage.** A user-started update needs at least 20 per cent battery on iPhone, iPad and Apple silicon Macs and 50 per cent on Intel Macs; automatic installs need 30 per cent on iPhone and iPad and 50 per cent on Macs. Devices short of space will fail and retry.

1. **Read the status reports.** Declarative device management reports whether each device is waiting, downloading or installing and gives error codes when it cannot finish, for example when it is offline or low on battery. Chase failures daily, not at the end.

1. **Set a retirement date for 18.x devices.** Handsets that stop at iOS 18 already receive fewer fixes than the 26 and 27 lines. Budget their replacement rather than waiting for the line to stop.

1. **Update the watches and headsets.** Push watchOS 27.2.1 or 26.6.1 and visionOS 27.2.1 in the same change.

1. **Close the change with evidence.** Export the post-deadline version report and attach it to the change record. Auditors may ask for it.

For tool choice and per-device costs if you are still managing Macs by hand, see our Intune, Jamf and Kandji comparison for 2027.

## A sample timeline

The timeline below starts today and ends inside the 14-day Cyber Essentials window, which closes on 23 November (9 November plus 14 days).

| Day | Date | Action | 
|---|---|---|
| 0 | Tue 17 Nov 2026 | Inventory, choose lines, exempt update hosts from TLS inspection | 
| 1 to 2 | 18 to 19 Nov | Pilot group on 27.2.1, 26.7.3, 18.7.12 and the macOS builds | 
| 2 | Thu 19 Nov, evening | Declare enforcement for all groups | 
| 6 | Mon 23 Nov | Enforcement deadline, 6 p.m. device local time | 
| 7 to 8 | 24 to 25 Nov | Chase failures; export the compliance report | 

The 6 p.m. deadline follows the example in Apple's deployment guide; pick a time that suits your users. Declaring on Thursday gives users the weekend and Monday to install at a time that suits them. This plan has every managed device on a fixed build 14 days after Apple's release.

## For teams in the UK, India and the US

**UK.** Cyber Essentials requires updates that fix critical or high-risk issues, meaning a CVSS v3 base score of 7 or above, to be installed within 14 days of release, and a bundled update containing any such fix counts in full. This release contains fixes scored 9.8, 8.8 and 7.5, so 23 November is the date to hit. Our Cyber Essentials Danzell explainer covers the 2027 question set and its patching rules.

**India.** The Digital Personal Data Protection Act, 2023 requires a data fiduciary to take reasonable security safeguards to prevent a personal data breach. Record this update as one of those safeguards and keep the version report as evidence. Check CERT-In's advisories for any note covering these builds.

**US.** Federal civilian agencies were bound by the KEV due date of 10 November for CVE-2026-111497. Private organisations are not bound by it, but the KEV date is a useful external benchmark for your own patch window.

## How eCorpIT can help

eCorpIT is a Gurugram-based technology organisation, founded in 2021, assessed at CMMI Level 5 and MSME certified, with senior-led engineering teams working across AWS, Microsoft and Google platforms. Our cybersecurity services team can inventory your Apple fleet by version line, set up declarative update enforcement and produce the compliance evidence for this and future Apple releases. Talk to us at /contact-us/.

Last updated: 17 November 2026.
