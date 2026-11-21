---
id: collect-261121-veille-ia/divers-tech/windows-11-25h2-how-can-i-restore-printing-after-kb5132043-without-leaving-security-updates-uninstal
title: "Windows 11 25H2: How can I restore printing after KB5132043 without leaving security updates uninstalled? - Microsoft Q&A"
domain: divers-tech
role: reference
task: article
actors: []
dates: []
keywords: ["windows", "print", "spooler", "printing", "after", "kb5132043", "security", "updates"]
source: https://learn.microsoft.com/en-us/answers/questions/6068257/windows-11-25h2-how-can-i-restore-printing-after-k
source_anchor: ""
source_lines: [1, 65]
section: "Divers_Tech"
---
Same issue seen on some devices after installing hotfix November 10, 2026âHotpatch KB5132043. Will be logging with MS

# Windows 11 25H2: How can I restore printing after KB5132043 without leaving security updates uninstalled?

Intune managed devices lost access to physical and virtual printers after the November 2026 updates. One affected computer was running Windows 11 build 26200.9911 with KB5132043 and KB5135385 installed.

Settings â Bluetooth & devices â Printers & scanners shows all printers as âNot connected,â including Microsoft Print to PDF. Printing is unavailable, and Add Printer reports that the local Print Spooler service is not running.

Diagnostic results:

Get-Printer returns âThe spooler service is not reachable,â HRESULT 0x800706BA.

sc.exe queryex spooler reports STOPPED, WIN32_EXIT_CODE 1067, and PID 0.

RPC-related services remain running.

Updating Windows, restarting the computer, and restarting the spooler did not resolve the issue. Uninstalling KB5132043 restored printing on one affected computer. I have not established the underlying cause.

What troubleshooting or supported remediation should I use to restore printing while keeping these computers protected with current security updates?

## Windows for business | Windows Client for IT Pros | User experience | Print, fax, and scan

## 4 answers

2. 
						I had the exact same problem of all my printers showing "not connected" and had no way to resolve this issue. I did a system restore back to before this Windows 11 25H2 update and my printers are all now working again.. So, this is an issue with the Windows Update that just came out and it is not, as suggested above, a local issue. We use a print spooler on a server. I have a dozen or more printers installed on my PC that use the spooler service. None were available after Windows 11 25H2. I would recommend to everyone to not install Windows 11 25H2 until Microsoft fixes this issue.
3. 
						Hi Red, Based on your findings, the most significant clue is that the Print Spooler service is terminating with Win32 Exit Code 1067, and uninstalling KB5132043 temporarily restores printing. This suggests that the update may be exposing an existing issue with a print driver, print monitor, printer management software, or spooler dependency rather than the update itself being the sole root cause. My first recommendation is to review Event Viewer > Windows Logs > Application and System, as well as Applications and Services Logs > Microsoft > Windows > PrintService, looking for spooler crashes, faulting modules, or driver-related events that occur when the spooler starts. Since Microsoft Print to PDF is also reported as unavailable, I would investigate whether the problem affects the entire print subsystem rather than a specific printer driver. It is also worth testing whether the spooler starts successfully after temporarily removing third-party printer drivers and print management utilities from an affected device. To remain protected with current security updates, I would avoid relying on uninstalling the update as a long-term solution. Instead, compare a healthy updated machine with an affected one and look for differences in installed printer drivers, print processors, language monitors, or endpoint security software. If the spooler crash consistently references the same module or driver, updating or removing that component is often enough to restore printing while keeping the security updates installed. Additionally, verify the integrity of the print subsystem and confirm that the Print Spooler dependencies and services are configured correctly. If multiple devices are affected, gathering crash details from several systems can help identify a common component responsible for the failures. I hope the response provided some helpful insight. If you find this answer useful, please hit **âaccept answerâ** so I know it addressed your concern.Jason
4. 
						AI answer 2026-11-11T23:25:19.77+00:00 Replace the November hotpatch with the latest standard cumulative update on a pilot device; this keeps the device on current security fixes while moving it from build 26200.9911 to the standard November build 26200.9974. ### Supported protected remediationMicrosoft explicitly recommends this path when a hotpatch causes an unexpected problem: uninstall the hotpatch, install the latest standard cumulative update (LCU), and restart. Automatic hotpatch rollback is not supported. ^1^ 
  1.  In **Intune admin center â Devices â Windows updates â Quality updates** , exclude a small affected-device group from the hotpatch-enabled policy or assign it a quality-update policy with hotpatch disabled.
  2.  On one pilot device, uninstall **KB5132043** .
  3.  Scan Windows Update and install the November standard LCU, **KB5132061** .
  4. Restart the device.
  5.  Confirm:
 The expected OS build for Windows 11 25H2 is```
   Get-ComputerInfo | Select-Object WindowsVersion, OsBuildNumber
   Get-Service Spooler
   Get-Printer
```
**26200.9974** , with the spooler running and printers enumerable. KB5132061 applies to all Windows 11 25H2 editions and retains the November security improvements. ^2^
  6.  Test physical printing and **Microsoft Print to PDF** , then deploy through the normal Intune rings.
 This differs from leaving KB5132043 uninstalled: the device immediately receives the standard cumulative security update instead. **KB5135385 does not provide a printing remediation.** It is a standalone PowerShell security update offered to eligible hotpatch devices after the November hotpatch, and Microsoft currently lists no known issues for it. ^3^### If the spooler still exits with 1067 on build 26200.9974Because restarting it has already failed, capture the crash event and then reset its queue rather than repeating another ordinary restart. 
  1.  Check **Event Viewer â Windows Logs â Application** and**Applications and Services Logs â Microsoft â Windows â PrintService â Admin** . Record the faulting module, exception code, and any print provider, processor, monitor, or driver named immediately before`spoolsv.exe` exits. Microsoft recommends checking event logs for errors initializing the local print provider. ^4^
  2.  Clear the spool directory. **This deletes queued print jobs, so preserve any required jobs first.**
 Clearing stale```
   Stop-Service Spooler -Force -ErrorAction SilentlyContinue
   $queue = "$env:windir\System32\spool\PRINTERS"
   Get-ChildItem $queue -Force |
       Remove-Item -Force -Recurse
   Set-Service Spooler -StartupType Automatic
   Start-Service Spooler
   Get-Service Spooler
   Get-Printer
```
`.SPL` /`.SHD` files and starting the service with an Automatic startup type is Microsoftâs documented spooler reset. ^5^
  3. If it crashes again and the event identifies a third-party component, remove or update that specific driver/package. Do not remove every queue first: failure of Microsoft Print to PDF is consistent with a spooler-wide failure, not proof that each printer connection is defective.
  4.  Where the vendor driver supports it, configure it as **Shared** or**Isolated** in**Print Management â Print Servers â local computer â Drivers â driver properties â Driver Isolation** . Isolation prevents a faulting driver component from halting the spooler itself. ^6^
  5. If no vendor feature is required, pilot Microsoftâs IPP inbox class driver/modern print platform instead of the vendor driver. Microsoft identifies that platform as preferred on Windows 10 and 11. ^6^
 ### Enterprise escalationThere is currently no documented KB5132043 spooler/1067 known issue: its release notes state that Microsoft is not aware of issues with the update. ^7^ Do not deploy an unrelated Known Issue Rollback. KIR normally rolls back specific **nonsecurity** changes and requires Microsoft to publish a matching policy definition; no matching printing KIR is documented here. ^8^If the standard LCU does not resolve the crash, reproduce it on one device and collect Microsoftâs printing trace before opening a support case: ```
.\TSS.ps1 -start -UEX_Print -Procmon -netsh
```
The printing scenario gathers spooler, process-monitor, and network evidence suitable for support analysis. ^9^ Include the `.zip` , affected/unaffected build numbers, installed printer-driver inventory, Application and PrintService events, and the result showing that uninstalling KB5132043 changed the behavior.
 **References**
5.  In
