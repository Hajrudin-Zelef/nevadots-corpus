---
id: collect-261121-veille-ia/windows-os/windows-11-insider-beta-preview-build-26320-10003-windows-insider-program
title: "Windows 11 Insider Beta Preview Build 26320.10003 - Windows Insider Program"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-16"]
keywords: ["windows", "insider", "users", "standard", "notification", "maps", "features", "preboot"]
source: https://learn.microsoft.com/en-us/windows-insider/release-notes/beta/preview-build-26320-10003
source_anchor: ""
source_lines: [1, 56]
section: "Windows_OS"
---
Release date: 16 November 2026

## Changes and improvements

### [File Explorer]

- Search from **This PC** in File Explorer is now indexer-based, making it much faster and consistent with search from File Explorer**Home** .
- Improved progress indication when navigating to locations that take longer to load, such as network drives, makes it easier to see that loading is underway.

### [BitLocker]

- Standard users can now set a BitLocker preboot PIN. Before this change, users without admin rights couldn't add the TPM+PIN protector. Now, standard users on managed devices can add the PIN themselves.
- **How it works:** When the policies below apply to a device that doesn't have a PIN yet, you get a notification asking you to add one. Selecting the notification opens a window where you can set the preboot PIN.

**Set it up**

In Microsoft Intune or another device management solution, configure these existing BitLocker policies to require TPM+PIN for non-administrators:

| Policy | Value | 
| `AllowStandardUserEncryption` | ON | 
| `AllowWarningForOtherDiskEncryption` | OFF | 
| `RequireDeviceEncryption` | ON | 
| `SystemDrivesRequireStartupAuthentication` | ON | 

Important

Target policy that requires startup authentication and device encryption only to your standard user test group that has this feature enabled in the Windows Insider Program.

Standard users without this feature can't add the required TPM+PIN protector, and device encryption subsequently fails with this configuration. Standard users without this feature who are already encrypted with TPM only report as non-compliant with policy.

 
- All users targeted with these policies get a notification to add a PIN to their device if they don't already have one. Selecting the notification opens a window where you can set the preboot PIN. 
 *Standard users get a notification asking them to add a preboot PIN.*
 
 *Selecting the notification opens a window where you set your BitLocker preboot PIN.*

### [Removal of Offline Maps]

- Following the April 2025 announcement of the upcoming deprecation of the Windows UWP Map control and Windows Maps platform APIs, we are removing the Offline Maps feature and related UX from Windows Settings. With the upcoming removal of the Maps control and APIs, the offline maps feature will no longer be functional. The removal includes these features in Windows Settings:
- For resources related to the upcoming removal of the Maps control and platform APIs, see this documentation.

### [Remote desktop]

- Fixed an issue where Remote Desktop Protocol (RDP) connections might fail on the client due to a certificate mismatch after the latest flights.

### [Known issues]

- Weâre investigating reports from Insiders that the CPU speed showing in Task Manager no longer updates after the latest flights.

## Reminders for Windows Insiders in the Beta channel

- Updates are based on Windows 11, version 26H2 via an enablement package (Build 26320.10003).
- The desktop watermark shown at the lower right corner of the desktop is normal for Windows Insider pre-release builds.
- Features and experiences included in these builds might never get released as we try out different concepts and get feedback. Features might change over time, be removed, or replaced and never get released beyond Windows Insiders. Some of these features and experiences could show up in future Windows releases when they're ready.
- Some features in active development we preview with Windows Insiders might not be fully localized, and localization will happen over time as features are finalized. As you see issues with localization in your language, report those issues to us via Feedback Hub.
- Check out Flight Hub for a complete look at what build is in which Insider channel.
