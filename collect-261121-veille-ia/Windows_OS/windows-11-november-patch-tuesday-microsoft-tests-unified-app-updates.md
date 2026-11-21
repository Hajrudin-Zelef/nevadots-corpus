---
id: collect-261121-veille-ia/windows-os/windows-11-november-patch-tuesday-microsoft-tests-unified-app-updates
title: "Windows 11 November Patch Tuesday: Microsoft tests unified app updates"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["windows", "update", "menu", "microsoft", "november", "settings", "updates", "patch"]
source: https://www.notebookcheck.net/Windows-11-November-Patch-Tuesday-Microsoft-tests-unified-app-updates.1446690.0.html
source_anchor: ""
source_lines: [1, 15]
section: "Windows_OS"
---
Microsoft has started rolling out its November 2026 Patch Tuesday update for Windows 11 versions 25H2 and 26H2. Released as KB5132061, the mandatory security update moves 25H2 to build 26200.9974 and 26H2 to build 26300.9974, and carries everything from the optional KB5127936 preview published on October 28. Most of the headline features arrive through a gradual rollout, so they may not appear on every PC straight away.

## A context menu you can finally trim

The redesigned File Explorer right-click menu is the most visible change. Selecting “Customize menu” at the bottom of the menu opens Settings, Personalization, and the Context menu, where users can switch built-in commands such as Send to and Create shortcut on or off, add Print, and show Cut, Copy, Paste, Rename, Delete, and Share as text labels instead of a row of icons. Properties can move back to the bottom, and third-party app extensions can sit in the main menu, in a submenu, or be hidden entirely. Tucking them into a submenu also speeds the menu up, because those extensions only load once it is opened.

Cloud rebuild is the other big addition. The Rebuild PC option under Settings, System, and Recovery restarts into the Windows Recovery Environment (WinRE) and reinstalls Windows 11 using an image and drivers downloaded from Windows Update, with no USB drive required. It also works when Windows fails to start, and users can choose to securely erase the drive first.

## Xbox mode, Magnifier, and battery tweaks

Xbox mode gains a refreshed Gaming Task View with a navigation bar along the bottom for Exit to Desktop and power options, including Shut down and Restart. Magnifier can now use a separate zoom level on each monitor, and modern standby PCs hibernate less often above 80 percent battery and sooner below 10 percent. The update also fixes a bug that left the Smart App Control toggle grayed out after October’s release.

## One settings page for app updates

Separately, third-party media reported on November 9 that an “App updates” page under Settings, Apps, has resurfaced in a Windows 11 26H2 preview build. It builds on the Windows Update orchestration platform Microsoft announced in May 2025. Developers in Microsoft’s preview can register MSIX, APPX, and Win32 apps through Windows Runtime APIs and PowerShell, letting Windows schedule update checks and installs around user activity, battery, and network conditions, and log update history in Settings. The page is not functional yet, and Windows Latest’s test PC showed no registered app providers.
