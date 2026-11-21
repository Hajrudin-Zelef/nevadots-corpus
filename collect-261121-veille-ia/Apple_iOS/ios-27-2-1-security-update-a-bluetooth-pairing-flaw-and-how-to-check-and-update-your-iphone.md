---
id: collect-261121-veille-ia/apple-ios/ios-27-2-1-security-update-a-bluetooth-pairing-flaw-and-how-to-check-and-update-your-iphone
title: "iOS 27.2.1 Security Update: A Bluetooth Pairing Flaw, and How to Check and Update Your iPhone"
domain: apple-ios
role: reference
task: article
actors: ["Apple"]
dates: []
keywords: ["bluetooth", "update", "ipad", "pairing", "iphone", "security", "apple", "ipados"]
source: https://candklab.com/en/blog/ios-27-2-1-security-update/
source_anchor: ""
source_lines: [1, 88]
section: "Apple_iOS"
---
# iOS 27.2.1 Security Update: A Bluetooth Pairing Flaw, and How to Check and Update Your iPhone

This post was translated from the original Korean article. Screenshots show the Korean interface.

**On November 9, 2026 (US time), Apple released iOS and iPadOS 27.2.1 together with iOS 26.7.3, iOS 18.7.12, and security updates for macOS, watchOS and visionOS.** The fixes include **a vulnerability that let an attacker in Bluetooth range pair with a device without user interaction** and **one that let a remote attacker join an iMessage group chat and receive its messages**. If you use an iPhone, iPad, Mac or Apple Watch, update to these versions.

This article is based on Appleâs security releases page and the security content document for each version, checked as of November 16, 2026.

## What changed

| Item | Details | 
|---|---|
| Release date | November 9, 2026 (US time); tvOS 27.2.1 on November 13 | 
| iPhone and iPad | iOS and iPadOS 27.2.1, iOS and iPadOS 26.7.3, iOS and iPadOS 18.7.12 | 
| Mac | macOS Golden Gate 27.2.1, macOS Tahoe 26.7.3, macOS Sequoia 15.8.3 | 
| Apple Watch | watchOS 27.2.1, watchOS 26.6.1 | 
| Exploited in the wild? | The Apple documents I checked carry no âmay have been exploitedâ note | 

The main fixes listed in Appleâs documents are below (descriptions paraphrased from Apple).

- **Bluetooth pairing (CVE-2026-113964, CVE-2026-113965):** An attacker in Bluetooth range could bypass the rate limit on pairing code attempts and pair with a device**without user interaction** , and a malicious Bluetooth device could establish a pairing without user interaction.
- **Bluetooth memory corruption (CVE-2026-113970):** An attacker in Bluetooth range could corrupt process memory.
- **AirPlay pairing (CVE-2026-113968, CVE-2026-113969):** An attacker in Bluetooth range or on the same network (Wi-Fi) could bypass the rate limit on AirPlay pairing code attempts.
- **iMessage group chats (CVE-2026-113961, CVE-2026-113960):** A remote attacker could join an iMessage group chat and receive its messages, and a person newly added to a group could read messages sent before they joined.
- **Other fixes:** Memory corruption in the brotli decompression component (CVE-2026-111497), and an issue that let an app access sensitive user data (CVE-2026-113963, iOS 27.2.1 only).

**Exploited in the wild?** As of this check, Appleâs documents do not say any of these âmay have been exploited.â But these are flaws **an attacker can target just by being nearby** over Bluetooth or Wi-Fi, so the more you use your phone in crowded places like the subway or cafÃ©s, the sooner you should update.

## Does this affect me?

- **iPhone 11 and later:** Yes. If you are on iOS 27, the fixed version is**27.2.1** ; if you have stayed on iOS 26, it is**26.7.3** . Both versions include the Bluetooth and iMessage fixes.
- **iPhone XS, XS Max, XR:** These canât run iOS 27 or 26, and Apple released**iOS 18.7.12** for them. Along with the Bluetooth pairing fixes, it fixes**an issue that let someone pair a device with an AirPlay receiver from the lock screen (CVE-2026-113971)** .
- **iPad:** iPad (9th gen), iPad mini (6th gen), iPad Air (4th gen) and later get iPadOS 27.2.1; somewhat older models (iPad 8th gen, iPad mini 5th gen, iPad Air 3rd gen, etc.) get iPadOS 26.7.3; iPad (7th gen) gets iPadOS 18.7.12.
- **Mac:** Updates were released for macOS Golden Gate, Tahoe and Sequoia.
- **Apple Watch:** Series 9 and later get watchOS 27.2.1; Series 6, 7, 8, SE (2nd gen) and Ultra get watchOS 26.6.1.
- **Galaxy and other Android phones:** Not affected by this update. For Android, see the November Android security update post.

## How to check

### iPhone and iPad

1. Go to **Settings** >**General** >**About** .
2. Look at the **iOS Version** (on iPad,**iPadOS Version** ) number.
3. If it is at or above the version for your device (**27.2.1** ,**26.7.3** or**18.7.12** ), the fixes are installed.

### Mac

1. Click the **Apple menu** in the top-left corner >**About This Mac** .
2. Check that the version next to the macOS name is at least **27.2.1** (Golden Gate),**26.7.3** (Tahoe) or**15.8.3** (Sequoia).

### Apple Watch

1. Open the **Watch** app on your iPhone.
2. Go to **General** >**About** and check the version.**27.2.1** or**26.6.1** or later is what you want.

## What to do

1. **Update your iPhone and iPad.** Go to**Settings** >**General** >**Software Update** and tap**Update Now** . Do it on Wi-Fi with the battery above 50% or plugged into a charger.
2. **If you want to stay on iOS 26, choose 26.7.3.** The same screen may show a separate option to upgrade to iOS 27. If you donât want to move to iOS 27, installing just the iOS 26.7.3 update still gets you the Bluetooth and iMessage fixes.
3. **Turn on automatic updates.** In**Settings** >**General** >**Software Update** >**Automatic Updates** , turn on the download and install options so future security updates install overnight on their own.
4. **Update your Mac and Apple Watch too.** On a Mac, use**System Settings** >**General** >**Software Update** ; for Apple Watch, use the**Watch** app on your iPhone >**General** >**Software Update** . The watch installs updates only while it is on its charger.
5. **Be careful with Bluetooth requests until you update.** Donât accept pairing requests from unknown devices, and clean up Bluetooth devices you no longer use in**Settings** >**Bluetooth** by tapping the info button next to the device name and choosing**Forget This Device** . If you canât update right away, turning Bluetooth off for a while in crowded places is another option.

**Note:** Apple says that after you install these versions, iOS, iPadOS, watchOS and others **cannot be downgraded to the previous version**. Backing up to iCloud or a computer before updating gives you peace of mind.

## If you see a stranger in an iMessage group

- Tap the names (or the member count) at the top of the group chat to see who is in it. If there is an unfamiliar number or email address, donât send sensitive information such as personal details or verification codes in that chat.
- After updating, if needed, create a new group with only the people you know and move the conversation there.
- Ignore requests to sign in through links received in the chat. If you are worried about your account password, see how to check whether your password has leaked for the steps.

## Summary

| Device | OK if at least | Where to update | 
|---|---|---|
| iPhone 11 and later (iOS 27) | 27.2.1 | Settings > General > Software Update | 
| iPhone 11 and later (staying on iOS 26) | 26.7.3 | Settings > General > Software Update | 
| iPhone XS, XS Max, XR | 18.7.12 | Settings > General > Software Update | 
| Mac | 27.2.1 / 26.7.3 / 15.8.3 | System Settings > General > Software Update | 
| Apple Watch | 27.2.1 / 26.6.1 | Watch app > General > Software Update | 

- On November 9 (US time), Apple released updates fixing 10 vulnerabilities, including unauthorized Bluetooth pairing and strangers joining iMessage group chats.
- As of this check, there is no report of active exploitation, but these flaws can be targeted just by being nearby, so itâs best to update right away.
- With automatic updates on, you wonât need to think about the next one.

## Comments

No comments yet. Feel free to leave a question or comment.
