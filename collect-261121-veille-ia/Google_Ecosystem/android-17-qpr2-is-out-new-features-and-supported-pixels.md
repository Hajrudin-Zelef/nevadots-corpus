---
id: collect-261121-veille-ia/google-ecosystem/android-17-qpr2-is-out-new-features-and-supported-pixels
title: "Android 17 QPR2 Is Out: New Features and Supported Pixels"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-15"]
keywords: ["pixel", "android", "qpr2", "also", "features", "november", "security", "update"]
source: https://itechify.com/2026/11/15/android-17-qpr2-pixel-features-supported-devices/
source_anchor: ""
source_lines: [1, 145]
section: "Google_Ecosystem"
---
**Android 17 QPR2** began rolling out to Pixel phones on November 10, 2026, bundled with the November Pixel Drop and the November security patch. It arrives roughly a month earlier than the usual quarterly schedule. If you own a recent Pixel, you can check for it now, though the rollout is staged and your phone may not see it on day one.

This guide covers which devices are eligible, the features worth trying, the security changes, and how to install the update. It also covers what developers should know.

## What is Android 17 QPR2 and why did it arrive early?

A **Quarterly Platform Release (QPR)** is a mid-cycle update to the current Android version. It adds features, developer APIs, and fixes without waiting for the next full Android release. Android 17 itself reached stable status on June 16, 2026, after several betas. QPR2 is the second QPR built on it, following QPR1 and earlier Feature Drops.

This time Google shipped it in one package with the November Pixel Drop and the monthly security update. That makes the download larger than a typical patch. It also means you get new features, fixes, and security protections in a single install.

Non-Pixel phones will get QPR2 later, once their makers port and test it.

## Which Pixel phones get Android 17 QPR2?

Eligible devices include:

- Pixel 6a
- Pixel 7, Pixel 7 Pro, and Pixel 7a
- Pixel Tablet and Pixel Fold
- Pixel 8 series, including the 8a
- Pixel 9 series, including the 9a
- Pixel 10 series, including the 10a
- Pixel 11 series

The **Pixel 6 and Pixel 6 Pro** are not on the build list. Google’s five years of updates for those phones reportedly ended with the October 2026 update. If you own one, don’t expect this release.

The rollout is staged. As of November 11, most Pixels should receive it over about a week. Feature availability also varies by model and country, so treat Google’s official Pixel Drop support page as the final word on what your phone gets.

## What’s new in Android 17 QPR2 for everyday use?

Most changes are small quality-of-life upgrades. Together they make the interface feel more customizable.

### Colors and wallpaper

The Material You color picker now offers a slider for custom accent colors. You can also pick from four theme styles: **Neutral, Soft, Bright, and Bold**. Adaptive wallpapers can shift between light and dark looks. You’ll find these options in Wallpaper & style.

### A new status bar page

Open **Settings > System > Status bar** to choose which notification icons appear. Your options are All, Priority, or None. You can also toggle system icons such as Mute, Vibrate, and Battery percentage. If your status bar feels crowded, this is the first place to look.

### Quick Settings upgrades

Quick Settings gains two new tiles, **Audio streams** and **Voice broadcast**. The battery icon now gets an outline while charging or when Battery Saver is on. Two other additions are useful:

- Brightness override through Quick Settings, for apps that lock screen brightness
- A keyboard switcher, so you can change input methods without digging through menus

QPR2 also adds Live Updates for printing documents, so you can follow a print job from the notification shade.

### Visual polish and foldables

Blur effects extend to more of the interface, including lock screen shortcuts, the fingerprint button, notifications, and the power menu. There’s also a new long-press menu on the home screen. On foldables and tablets, a window handle lets you switch between full-screen, split-screen, and floating layouts.

One thing to know: a bigger update that lets you rearrange the brightness slider and media player is not part of this release.

## What privacy, security, and anti-scam changes are included?

Google put real weight on scam protection this time.

- **Call-forwarding protection:** Apps can no longer trigger call-forwarding USSD codes. If you dial one manually, the phone asks you to confirm. Scammers have used these codes to quietly reroute calls.
- **Risky actions blocked during calls:** While you’re on a call with an unknown contact, the system blocks actions such as enabling Developer options. That stops a common social-engineering trick.
- **Private Compute Core log:** A new log shows what data is shared with on-device AI features.
- **Agents dashboard:** It sits under Privacy controls, so you can see what AI agents are doing.
- **SIM security timeline viewer:** It supports multiple SIMs and shows the security measures a carrier uses.

The November 2026 security patch also resolves 232 Android vulnerabilities across two patch levels. It adds 166 Pixel-specific fixes on top. That alone is a good reason not to delay too long.

## What extras come with the November Pixel Drop?

The Pixel Drop layers hardware-specific features over the platform update. Battery and charging settings now live in a unified **Battery & charging** menu in Settings, which makes them easier to find.

Newer hardware gets some perks:

- **Pixel 11 Pro, Pro XL, and Pro Fold:** HiLight now supports message notifications, blinking twice. These models also get Auracast Voice Broadcast.
- **Pixel 10 series:** Extreme Charging Mode expands to the Pixel 10, 10 Pro, 10 Pro XL, and 10 Pro Fold. The Pixel 10a is excluded.
- **Pixel 10a:** It gets search and icon styles but not Gemini Intelligence or Rambler.
- **Pixel 9 and newer:** Automated message summaries and notification summaries reach more languages.

If a feature is missing on your phone, it’s probably a hardware or regional limit, not a failed install.

## Does it fix bugs, and should you install it now?

The update bundles more than 30 bug fixes, and some outlets count 33. They cover keyboard glitches, sudden volume spikes, and camera save errors on the Pixel 11. Reports also say this is the first stable build to address the stuttering and freezing that some owners noticed after QPR1. Your results may differ, so treat that as encouraging rather than guaranteed.

Should you install it right away? For most people, yes. The security fixes are substantial. Still, a few precautions make sense:

- Back up your phone first.
- Charge it above half and use Wi-Fi, since the download is large.
- If you depend on your phone for work, wait a few days and watch for early bug reports.

## How do you check for and install the update?

1. Open **Settings** .
2. Tap **System** , then**System update** .
3. Tap **Check for update** .
4. Download and install when it appears, then restart if prompted.
5. Go to **Settings > About phone** to confirm the Android version, build, and security patch.

If nothing shows up, the staged rollout probably hasn’t reached you yet. Check again over the next several days. Pixel 6 and 6 Pro owners won’t see it at all, so it’s worth weighing a replacement or a different long-term plan for that device.

## What should developers know, and what comes next?

The Android 17 QPR2 release notes list a minor SDK release and the new call-forwarding security restrictions. On QPR2 and higher, self-broadcasts are delivered more efficiently, which helps overall system health. If your app sends broadcasts to itself, test its behavior on the new build.

For history, Android 17 QPR2 Beta 7.1 rolled out on October 16, 2026. It used build CP41.260831.019 and the October 2026 security patch level. The full details live in the Android 17 QPR2 release notes.

Looking ahead, non-Pixel phones will receive QPR2 later, and a QPR3 beta is already in the pipeline. Samsung, Motorola, and others set their own timelines, so expect a wait.

## Frequently Asked Questions

### Is my Pixel getting Android 17 QPR2?

Supported devices include the Pixel 6a, Pixel 7 series, Pixel Tablet, Pixel Fold, and the Pixel 8, 9, 10, and 11 series, including the a models. The rollout is staged, so it may take about a week to reach you.

### Does the Pixel 6 get Android 17 QPR2?

No. The Pixel 6 and Pixel 6 Pro are not on the build list, and their five years of updates reportedly ended with the October 2026 update.

### How do I install Android 17 QPR2 on my Pixel?

Go to Settings > System > System update and tap Check for update. If it doesn’t appear yet, the staged rollout hasn’t reached your phone, so try again over the next few days.

### What is the difference between Android 17 QPR2 and the November Pixel Drop?

QPR2 is the platform update with system features, APIs, and fixes. The Pixel Drop adds Pixel-specific features on top, and this month both arrived together with the security patch.

### What new features does Android 17 QPR2 add to Quick Settings?

It adds Audio streams and Voice broadcast tiles, a brightness override for apps that lock brightness, and a keyboard switcher. The battery icon also gets an outline while charging or in Battery Saver.

### How do I change which notification icons show in the status bar?

Open Settings > System > Status bar. Choose All, Priority, or None for notification icons, and toggle icons like Mute, Vibrate, and Battery percentage.

### Does Android 17 QPR2 fix Pixel stuttering or battery drain?

The update bundles over 30 bug fixes, and reports say it is the first stable build to address stuttering and freezing seen since QPR1. Results can vary by device, so it’s worth installing and testing.

### When will non-Pixel phones get Android 17 QPR2?

Google says it will reach eligible non-Pixel devices later, but no single date applies. Each manufacturer sets its own schedule.

### Is it safe to install Android 17 QPR2 right away?

For most people, yes, especially given the large security patch. Back up first, and if you rely on your phone for work, waiting a few days is a reasonable precaution.
