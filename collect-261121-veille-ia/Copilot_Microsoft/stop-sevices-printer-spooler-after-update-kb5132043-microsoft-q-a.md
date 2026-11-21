---
id: collect-261121-veille-ia/copilot-microsoft/stop-sevices-printer-spooler-after-update-kb5132043-microsoft-q-a
title: "Stop sevices Printer Spooler after update KB5132043 - Microsoft Q&A"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["spooler", "kb5132043", "microsoft", "after", "print", "printer", "update", "windows"]
source: https://learn.microsoft.com/en-us/answers/questions/6067631/stop-sevices-printer-spooler-after-update-kb513204
source_anchor: ""
source_lines: [1, 57]
section: "Copilot_Microsoft"
---
Any of you using Forcepoint web protection? Uninstalling the client/agent for that seems to fix this.

I'm trying a manual install of the KB5132061 .msu to see if that resolves this.

After installing KB5132043, printers show 'Not connected' and cannot print. Uninstalling KB5132043 resolves the issue.

We've had luck installing the non hotpatch version: KB5132061, which resolved the issue.

Hi Kien,

Since the Print Spooler service stops after installing KB5132043 and printing works again after uninstalling it, we recommend investigating a possible printer driver or service compatibility issue. However, Microsoft currently lists no known issues with this update.

Please try the following:

`services.msc` and check the Print Spooler service status.
Documentations for reference.

Hope this helps!

Ronald

AI answer

KB5132043 is a Windows 11 Hotpatch with no currently documented printing issue, so after the already-tested uninstall, temporarily pause its redeployment and reset the Print Spooler before testing the update again on one device. Microsoft lists KB5132043 for Windows 11 versions 24H2, 25H2, and 26H2 and currently reports no known issues with it.^1^

Run **PowerShell as administrator**:

```
Stop-Service Spooler -Force
Remove-Item "$env:WINDIR\System32\spool\PRINTERS\*" `
    -Force -ErrorAction SilentlyContinue
Set-Service Spooler -StartupType Automatic
Start-Service Spooler
Get-Service Spooler
```
Deleting the spool files discards pending print jobs, but does not remove installed printers. Microsoft documents stopping the service, clearing `%WINDIR%\System32\spool\PRINTERS`, and restarting it with startup type set to **Automatic**.^2^

The final command should show:

`Status`: `Name`: Then open **Settings â Bluetooth & devices â Printers & scanners**, select the printer, and print a test page.

Do not repeatedly restart it without identifying the component causing the termination.

`spoolsv.exe` stopped.```
   gpresult /h "%TEMP%\gpresult.html"
```
Microsoft identifies Group Policy, outdated drivers, antivirus interference, system instability, and vendor-supplied spooler components as relevant checks when the service will not remain running.^3^

If an event names a printer-driver DLL, remove that printer and its old or duplicate driver under **Device Manager â Print queues/Printers**, then install the current package from the printer manufacturer. Windows guidance specifically recommends removing conflicting or obsolete drivers when the spooler crashes.^4^

Because removing KB5132043 restores printing, keep it off production devices temporarily while reproducing the problem on a pilot device. On an unmanaged test device, use **Settings â Windows Update â Pause updates â Pick a date**. This is temporary; Windows will resume update installation when the pause ends.^5^

Compare these states on the pilot:

Report the reproducible behavior through **Feedback Hub** by pressing **Windows+F**, and attach screenshots or a recording plus the event details. Feedback Hub is the supported channel for reporting unexpected Windows behavior; immediate guided assistance should go through Microsoft Support.^6^

**References**
