---
id: collect-261121-veille-ia/windows-os/windows-11-insider-beta-preview-build-26320-9876-windows-insider-program
title: "Windows 11 Insider Beta Preview Build 26320.9876 - Windows Insider Program"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-06"]
keywords: ["windows", "settings", "build", "insider", "search", "microsoft", "storage", "update"]
source: https://learn.microsoft.com/en-us/windows-insider/release-notes/beta/preview-build-26320-9876
source_anchor: ""
source_lines: [1, 58]
section: "Windows_OS"
---
Release date: 6 November 2026

[Update 9 November 2026: This build has been bumped up to version 26320.9877]

## Changes and improvements

### [Windows Update]

- With this update, the Beta channel moves to Windows 11, version 26H2, with the build number increasing to Build 26320. You shouldn't expect any significant change from this build number increase alone, as it's delivered via an enablement package (eKB) update.

### [Windows Search]

**New Windows Search experience with inline actions:** A new Windows Search experience built on WinUI 3 is beginning to roll out. It's faster, more efficient, and features a streamlined design that makes results easier to scan. Inline actions let you quickly complete tasks directly. Turn on dark mode or Bluetooth, adjust your screen brightness, mute your device, or arrange windows from Windows Search.
- Windows Search is improving how it understands what you're looking for, including better matching for typos and synonyms. These improvements will continue to be fine-tuned over the coming weeks. For more information about the changes, check out the Windows Insider Blog.
- English is the only supported language for this initial release. 
 *Screenshot of the new Windows Search experience.*

### [Live captions]

- Live captions now gives you control over what gets transcribed. Previously, captions used system audio, with an option to add your microphone. Now you can choose system audio, your microphone, or both, so captions follow the sound you care about. Caption an in-person conversation without picking up system audio, or follow audio from your PC during a meeting or while watching a video.

### [Gaming]

- Quick settings is coming to the Gaming Task View navigation bar in XBOX mode, giving you access to the full set of Quick settings available on the Windows taskbar without having to leave XBOX mode. 
 *Quick settings in the Gaming Task View navigation bar in XBOX mode.*

### [Web sign-in]

- Web sign-in now works on Microsoft Entra hybrid joined devices. Before this update, it was only available on Microsoft Entra joined devices. You can sign in to Windows at the lock screen with a passwordless credential: a FIDO2 security key, a passkey on your phone, or a Temporary Access Pass (TAP). You don't need a password. Once you're signed in, Windows gets a Kerberos ticket, so on-premises file shares and internal sites keep working. For more information, see Use web sign-in to enable passwordless sign-in in Windows.

### [Windows Hello]

- Windows Hello facial recognition can temporarily brighten your screen to help you sign in when you're in a dark or dimly lit room.
- To turn on this feature, go to **Settings > Accounts > Sign-in options > Facial recognition (Windows Hello)** and turn on**Automatically adjust screen brightness during face recognition** .
 
 *The setting to automatically adjust screen brightness during Windows Hello facial recognition.*

- The dialogs that ask whether you'd like to turn on Sticky Keys or Filter Keys are being updated, making it easier to dismiss these prompts permanently. If you don't use these keyboard features, you can choose not to be asked again directly from the dialog, helping you continue what you're doing with fewer interruptions.

### [Settings]

**See your family's shared Microsoft 365 cloud storage in Windows Settings**

- Windows Settings now contains shared storage usage across Microsoft 365 Family subscriptions. A breakdown for each family member is coming soon. If your subscription was purchased after October 8, 2026, you can check the view by going to **Settings > Accounts > Cloud storage** . If your subscription was purchased before October 8, 2026, this view will be available for you after the first subscription renewal on or after May 2, 2027. Learn more about the changes to Microsoft 365 storage.
 
 *Shared cloud storage usage across Microsoft 365 Family members in Settings > Accounts > Cloud storage.*

### [File Explorer]

- The **Address Bar** now supports`~` , making it quicker to navigate directly to your user profile folder and subfolders, for example,`~\AppData` .

## Reminders for Windows Insiders in the Beta channel

- Updates are based on Windows 11, version 26H2 via an enablement package (Build 26320.9876).
- The desktop watermark shown at the lower right corner of the desktop is normal for Windows Insider pre-release builds.
- Features and experiences included in these builds might never get released as we try out different concepts and get feedback. Features might change over time, be removed, or replaced and never get released beyond Windows Insiders. Some of these features and experiences could show up in future Windows releases when they're ready.
- Some features in active development we preview with Windows Insiders might not be fully localized, and localization will happen over time as features are finalized. As you see issues with localization in your language, report those issues to us via Feedback Hub.
- Check out Flight Hub for a complete look at what build is in which Insider channel.
