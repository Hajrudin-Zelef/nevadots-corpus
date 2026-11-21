---
id: collect-261121-veille-ia/copilot-microsoft/kb5132061-26300-9974-will-not-install-on-window11-desktop-home-microsoft-q-a
title: "(KB5132061) (26300.9974) will not install on Window11 desktop home - Microsoft Q&A"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["windows", "update", "kb5132061", "microsoft", "install", "settings", "open", "system"]
source: https://learn.microsoft.com/en-us/answers/questions/6067407/kb5132061-26300-9974-will-not-install-on-window11
source_anchor: ""
source_lines: [1, 65]
section: "Copilot_Microsoft"
---
A documented next step is to verify the Windows 11 26H2 servicing prerequisites, then install KB5132061âs catalog packages in the required checkpoint order; error `0x800f0989` alone does not establish the underlying cause.

Build **26300.9974** is Windows 11 **26H2**, and Home edition is supported. Microsoft has not listed `0x800f0989` as a known KB5132061-wide issue.^1^

### 1. Check the 26H2 prerequisites

1.  Open **Settings â Windows Update â Update history** .
2.  Look for **KB5124010** , dated September 22, 2026, or any later successfully installed Windows cumulative update.
3.  If none is present:
  -  Open the Microsoft Update Catalog.
  -  Search for `KB5124010` .
  -  Download the package matching **Windows 11 26H2** and your system type, normally**x64** .
  -  Install it and restart.
4.  Open **Command Prompt as administrator** and run this single-line command:

```
DISM /Online /Get-Packages | findstr /i "ServicingStack"
```

The returned servicing-stack package must be version **26100.9539 or later** for 26H2. Both the September 22, 2026-or-later update and this SSU level are documented prerequisites for KB5132061.^1^

If the SSU is older, return to **Settings â Windows Update**, install every earlier cumulative/security update offered, and restart. Do not force KB5132061 manually until the required SSU is present. If Windows Update offers nothing and the SSU remains below the minimum, proceed to the repair reinstall in step 3.

### 2. Install the checkpoint and KB5132061 in order

First confirm the architecture at **Settings â System â About â System type**. For a typical Intel/AMD desktop showing **64-bit operating system, x64-based processor**:

1.  Create `C:\Packages` .
2.  Download these two Microsoft packages:
  1.  Required checkpoint KB5043080
  2.  Target update KB5132061
3.  Save both in `C:\Packages` , with no other files there.
4.  Double-click **KB5043080** first. âAlready installedâ is acceptable.
5.  Double-click **KB5132061** .
6.  Restart, then open **Settings â Windows Update â Update history** and confirm KB5132061 installed.

Microsoft specifically requires catalog checkpoint cumulative updates to be installed before the target cumulative update.^2^

If double-clicking the target package still fails, run this from **Command Prompt as administrator**:

```
DISM /Online /Add-Package /PackagePath:C:\Packages\windows11.0-kb5132061-x64_aedc878e9e078df2ec7e2cfb98dd5b21831cdbcc.msu
```

### 3. If it still fails, repair Windows without removing apps or files

Since the general online fixes have already been unsuccessful, use Windows 11âs supported component-repair reinstall:

1.  Keep the PC connected to power and the internet.
2.  Open **Settings â System â Recovery** .
3.  Under **Fix problems using Windows Update** , select**Reinstall now** .
4.  Select **OK** , allow the repair version of Windows to download and install, and restart.
5.  Return to **Settings â Windows Update** and retry KB5132061.

This reinstalls the same Windows version, repairs system files and components, and preserves apps, files, and settings.^3^

If **Reinstall now** is absent, it can be unavailable on update-managed devices or systems missing earlier prerequisite updates. On an unmanaged Windows 11 Home PC, open the **Get Help** app and request Windows Update support after recording the DISM error shown during step 2.^4^


**References**

1.  November 10, 2026âKB5132061 (OS Builds 26300.9974, 26200.9974 and 26100 ...
2.  [Catalog](https://support.microsoft.com/servicing/os/windows-11/2026/11/kb5132061-windows-11-24h2-25h2-26h2-security-update)
3.  Fix issues by reinstalling the current version of Windows | Microsoft Support
4.  Windows 11, version 26H2 known issues and notifications
