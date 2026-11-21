---
id: collect-261121-veille-ia/windows-os/microsoft-confirms-november-windows-11-hotpatch-can-stop-office-and-other-apps-from-launching
title: "Microsoft Confirms November Windows 11 Hotpatch Can Stop Office and Other Apps From Launching"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-19"]
keywords: ["windows", "microsoft", "november", "hotpatch", "office", "devices", "applications", "update"]
source: https://pbxscience.com/microsoft-confirms-november-windows-11-hotpatch-can-stop-office-and-other-apps-from-launching/
source_anchor: ""
source_lines: [1, 35]
section: "Windows_OS"
---
## Microsoft Confirms November Windows 11 Hotpatch Can Stop Office and Other Apps From Launching

November 19, 2026 · Windows 11 news

Microsoft has told IT administrators that the November 2026 hotpatch for Windows 11, delivered through Windows Autopatch, can cause Office and third-party applications to fail during installation, launch or normal use. Devices that received the standard November monthly security update are not expected to be affected, and a temporary registry workaround is available.

A security update released on November 10 for Windows 11 may leave some applications, including Microsoft Office, unable to start. The problem was reported on November 19 based on a service alert that Microsoft sent to IT administrators, first seen by BleepingComputer. Microsoft says the issue does not affect systems that installed the regular November Patch Tuesday security update. It applies only to devices that received the hotpatch.

### Which update and which devices are affected

Hotpatch updates apply security fixes without requiring a device restart. The update in question is `KB5132043`, released on November 10 for Windows 11 versions 24H2, 25H2 and 26H2 (OS builds 26100.9911, 26200.9911 and 26300.9911).

According to Microsoft’s alert, the problem occurs on devices that installed the hotpatch through Windows Autopatch, the company’s cloud service that automates Windows and Microsoft 365 updates for organizations. This means the issue mainly concerns enterprise environments rather than typical consumer PCs.

### What users may see

Affected devices may show the error `ERROR_ALLOTTED_SPACE_EXCEEDED (0xC0000099)` when installing, opening or running Office and some third-party applications. An application that worked normally before the update can also display the error later, during regular use.

Microsoft’s analysis links the failures to applications that load the 32-bit version of the Windows Installer library, `msi.dll`. 32-bit applications are the most likely to be affected, but 64-bit applications can also encounter the problem under certain conditions.

### Temporary workaround for administrators

Until a permanent fix is released, Microsoft recommends that IT administrators raise the `HotPatchTableSize` value in the Windows registry to 2000. Because this involves editing the registry, back it up first so it can be restored if anything goes wrong.

- Open the Start menu, type `regedit` and select Registry Editor.
- Go to the following key:
      HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management
- In the right-hand pane, open `HotPatchTableSize` and set its value to`2000` , then click OK.
- Close Registry Editor and restart the device.

Microsoft has not yet announced when a permanent fix will arrive. Administrators who manage hotpatch-enrolled devices should watch the Windows release health dashboard (service alert WI1559710) for updates.

### What this means

Organizations that use Windows Autopatch with hotpatching should check whether their devices have installed KB5132043 and look for 0xC0000099 errors in Office or line-of-business applications. Devices on the regular monthly security update track are not expected to see the issue.
