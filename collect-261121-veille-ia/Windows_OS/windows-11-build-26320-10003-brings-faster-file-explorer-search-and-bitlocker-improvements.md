---
id: collect-261121-veille-ia/windows-os/windows-11-build-26320-10003-brings-faster-file-explorer-search-and-bitlocker-improvements
title: "Windows 11 build 26320.10003 brings faster File Explorer search and BitLocker improvements"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["windows", "maps", "bitlocker", "file", "explorer", "users", "search", "offline"]
source: https://pureinfotech.com/build-26320-10003-windows-11-26h2/
source_anchor: ""
source_lines: [1, 56]
section: "Windows_OS"
---
- File Explorer now uses the Windows Search indexer when searching in This PC, delivering faster, more consistent results.
- Standard users on eligible managed devices can configure a BitLocker preboot PIN without administrator privileges.
- Microsoft is removing Offline Maps and its related storage options from the Settings app as it phases out the underlying Maps platform APIs.
- The update fixes Remote Desktop connection, mouse cursor visibility, and voice typing issues, but introduces a known Task Manager CPU speed reporting problem.

**Windows 11 build 26320.10003** is now available in the Beta channel for version 26H2 through the **Windows Insider Program**, introducing improvements to File Explorer, BitLocker, and system reliability. This preview speeds up searches in This PC by using the Windows Search indexer, improves loading indicators when accessing slower locations such as network drives, and lets standard users on managed devices set up a BitLocker preboot PIN without administrator privileges.

The update also begins removing the Offline Maps feature from the Settings app as Microsoft phases out the underlying Maps platform APIs. In addition, build 26320.10003 addresses several issues affecting Remote Desktop connections, mouse cursor visibility, and voice typing settings. However, the company is still investigating a known issue that prevents Task Manager from updating processor speed information.

## What’s new on Windows 11 build 26320.10003 (Beta)

These are the new features and improvements rolling out in the Beta channel:

### File Explorer gets faster search

File Explorer now uses the Windows Search indexer when searching from **“This PC,”** delivering faster results and a more consistent experience with searches from “Home.” The Windows team has also improved progress indicators when opening locations that take longer to load, such as network drives, making it easier to tell when File Explorer is still working.

### Standard users can configure BitLocker PINs

Microsoft is making BitLocker security easier to manage on organizational devices by allowing standard users to configure a preboot PIN without administrator privileges.

In the past, adding the TPM+PIN protector required admin rights. Now, when administrators apply the required policies through Microsoft Intune or another device management solution, eligible users receive a notification prompting them to create a PIN. Selecting the notification opens a dedicated setup window where users can enter and confirm their new PIN.

To enable this capability, administrators must configure the following existing BitLocker policies:

- `AllowStandardUserEncryption` –**ON**
- `AllowWarningForOtherDiskEncryption` –**OFF**
- `RequireDeviceEncryption` –**ON**
- `SystemDrivesRequireStartupAuthentication` –**ON**

It’s important to note that administrators should target the startup authentication and device encryption policies only to a standard user test group that has this feature enabled through the Windows Insider Program. Standard users without the feature cannot add the required TPM+PIN protector, which causes device encryption to fail with this configuration. Users already protected with TPM-only encryption may also appear as non-compliant with the policy.

### Offline Maps feature is going away

The software giant is removing Offline Maps and its related options from the Settings app following its April 2025 announcement about deprecating the Windows UWP Map control and Windows Maps platform APIs. Since the underlying technology will no longer be supported, offline maps will stop working. The changes remove two options from the Settings app:

- **Offline Maps:** Previously available under**Settings > Apps > Offline Maps** .
- **Maps storage:** Previously available under**Settings > Storage > Local Disk** .

### Other fixes

The Windows team has also addressed several smaller issues to improve the overall experience:

- **Remote Desktop:** Fixed a problem that could cause Remote Desktop Protocol (RDP) connections to fail on the client because of a certificate mismatch after recent preview builds.
- **Mouse cursor:** Resolved an underlying issue that could cause the cursor to disappear unexpectedly in certain situations.
- **Voice typing:** Improved the reliability of configuring voice typing settings.

### Known issues

Microsoft is investigating reports that Task Manager no longer updates the processor speed information after installing recent preview builds.

## Download Windows 11 update 26320.10003

To download and install these Windows 11 builds, enroll your Devices in the **Beta** channel through the “Windows Insider Program” settings from the “Update & Security” section. Once you enroll the computer in the program, you can download the build from the “Windows Update” settings by turning on the **“Get the latest updates as soon as they’re available”** option and clicking the **“Check for Updates”** button.

Alternatively, you can also download the ISO files from the **Windows Insider Preview Downloads** page. Specific preview builds are available for a limited time only.
