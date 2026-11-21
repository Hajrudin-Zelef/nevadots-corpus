---
id: collect-261121-veille-ia/windows-os/microsoft-confirms-windows-11-hotpatch-breaks-app-installs-and-shares-a-registry-workaround-ghacks-t
title: "Microsoft Confirms Windows 11 Hotpatch Breaks App Installs and Shares a Registry Workaround - gHacks Tech News"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-19"]
keywords: ["microsoft", "windows", "hotpatch", "updates", "devices", "registry", "error", "november"]
source: https://www.ghacks.net/2026/11/19/microsoft-confirms-windows-11-hotpatch-breaks-app-installs-and-shares-a-registry-workaround/
source_anchor: ""
source_lines: [1, 32]
section: "Windows_OS"
---
Microsoft has confirmed that the November 2026 hotpatch update for Windows 11 can break the installation and launch of some applications, including Office. The problem affects devices running Windows 11 version 24H2 and later, and Microsoft has shared a registry workaround until a fix is ready.

Affected devices show an error when installing, launching or running some apps. "Devices which installed the November 2026 hotpatch update might experience an error when installing, launching, or running some applications," Microsoft told administrators in a service alert. The error reads "ERROR_ALLOTTED_SPACE_EXCEEDED (0xC0000099)" or something similar.

The problem can appear at any point, not only during installation. An app that installed and opened normally before the hotpatch can show the error later during normal use.

## Which Windows 11 Devices Are Affected

The bug only hits devices that installed the November hotpatch through Windows Autopatch, Microsoft's cloud service that automates Windows and Microsoft 365 updates for businesses. Devices that installed the standard November 2026 Patch Tuesday security updates are not affected, according to Microsoft.

Microsoft says the issue appears in applications that load the 32 bit Windows Installer DLL, msi.dll. "This can happen more frequently with 32-bit applications, but can also occur for 64-bit applications under certain conditions," the company said.

Hotpatch updates install security fixes without a restart. In March, Microsoft said it would turn on hotpatch security updates by default for all eligible Windows devices managed through the Microsoft Graph API and Microsoft Intune, after the May 2026 security updates were installed. Windows Autopatch is free for customers with a Windows 10 or 11 Enterprise E3 license or higher and has been generally available since July 2022.

It is not the first Autopatch problem this year. In April, Microsoft fixed an Autopatch bug that deployed driver updates blocked by administrative policies to some managed devices belonging to customers in the European Union.

## How to Apply the Hotpatch Workaround

Until a fix is available, administrators can raise the HotPatchTableSize value in the registry:

1. Open the Start menu, type regedit and select Registry Editor.
2. Go to HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management.
3. In the right panel, open the HotPatchTableSize value and set it to 2000. Click OK.
4. Close Registry Editor and restart the device.

Microsoft recommends backing up the registry before making the change and making sure you know how to restore it if something goes wrong.

The hotpatch bug is the latest in a run of problems with this month's Windows updates. Microsoft has already confirmed activation failures and printing issues after the November security updates, and released emergency out of band updates earlier this week to fix the activation error 0x80041008.

Microsoft has not said when a fix for the hotpatch issue will be released.

## Leave a Reply
