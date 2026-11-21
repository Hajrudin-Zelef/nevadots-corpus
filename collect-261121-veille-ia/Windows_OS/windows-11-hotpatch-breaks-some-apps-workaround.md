---
id: collect-261121-veille-ia/windows-os/windows-11-hotpatch-breaks-some-apps-workaround
title: "Windows 11 Hotpatch Breaks Some Apps: Workaround"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["windows", "microsoft", "hotpatch", "updates", "apps", "workaround", "some", "devices"]
source: https://shiftdelete.net/en/windows-11-hotpatch-app-issues
source_anchor: ""
source_lines: [1, 29]
section: "Windows_OS"
---
# Windows 11 Hotpatch Breaks Some Apps; Microsoft Shares a Workaround

Microsoft has confirmed that November's hotpatch updates can stop some apps from installing or launching on Windows 11. A registry workaround is available while a fix is prepared.

Microsoft has confirmed that its November 2026 hotpatch updates are causing some applications to fail on Windows 11. Affected users may see errors when installing, launching or simply using certain apps, and a permanent fix is not yet available.

The problem was first reported by BleepingComputer, which saw Microsoft's service alert to IT administrators. According to the alert, tracked as WI1559710 in the Windows release health dashboard, the issue affects Windows 11 version 24H2 and later on devices that received the November hotpatch updates through Windows Autopatch.

Systems that installed the regular November 2026 Patch Tuesday security updates should not be affected. The problem is specific to hotpatching, a delivery method that applies security fixes without requiring an immediate restart.

## What goes wrong

Users on affected machines may see error messages similar to "ERROR_ALLOTTED_SPACE_EXCEEDED (0xC0000099)" when an app installs, starts or runs. Microsoft's investigation so far links the issue to applications that load the 32-bit Windows Installer library, msi.dll. It shows up more often with 32-bit apps, but 64-bit apps can also be affected under certain conditions.

Hotpatch updates arrive through Windows Autopatch, a service organizations use to manage updates on their devices. That means the people most likely to run into the bug are employees on company laptops and the IT teams that support them, rather than home users who install the regular monthly updates.

## The temporary workaround

Until a fix ships, Microsoft is offering a registry change as a temporary workaround. Administrators can increase the HotPatchTableSize value to 2000 under HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management, then restart the device.

Microsoft's own guidance comes with the usual warning: "Before you modify the registry, back it up and make sure that you know how to restore it if a problem occurs." In practice, organizations will want to test the change on a small group of devices before rolling it out widely through their management tools.

## Why more devices are getting hotpatches

The bug lands at a time when hotpatching is reaching many more PCs. In March, Microsoft said it would turn on hotpatch security updates by default for eligible devices managed through the Microsoft Graph API and Microsoft Intune, starting after the May 2026 Windows security updates were installed.

Hotpatching is designed to cut the number of restarts that interrupt work, since security fixes can take effect without rebooting the device right away. Wider default use, however, also means that a faulty hotpatch reaches more managed machines.

Separately, Microsoft CEO Satya Nadella recently warned that much of the world's critical infrastructure cannot be patched at all, as we reported in our coverage of his comments. Microsoft has not said when a permanent fix for the hotpatch issue will arrive.
