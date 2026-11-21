---
id: collect-261121-veille-ia/windows-os/office-2021-and-windows-11-24h2-lose-security-updates-on-13-october-a-windows-10-gets-another-year
title: "Office 2021 and Windows 11 24H2 Lose Security Updates on 13 October â Windows 10 Gets Another Year"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["windows", "version", "home", "office", "october", "updates", "security", "update"]
source: https://securerank.info/blog/windows-11-24h2-office-2021-end-of-support-2026/
source_anchor: ""
source_lines: [1, 73]
section: "Windows_OS"
---
**The TL;DR**

**Windows 11 24H2** (Home, Pro, Pro Education, Pro for Workstations) and **Office 2021** (Windows and Mac) reach end of support on 13 October 2026, Pacific time. Move Windows to 25H2 or 26H2 â on 24H2 it is a small enablement package, not a reinstall â and replace Office 2021 with Microsoft 365, Office 2024 or a free suite. **Windows 10** is the exception: consumer Extended Security Updates now run to **12 October 2027**, and if you already enrolled, you don't need to do anything. Antivirus is a layer on top of patches, not a replacement for them.

## 1. What ends on 13 October â and what doesn't

Microsoft publishes these dates on its lifecycle pages in Pacific time, so "13 October" means the end of the day in Redmond, which is the morning of 14 October in Europe. In practice, the October 2026 Patch Tuesday is the last one these products will ever get.

| Product | Editions | Security fixes until | 
|---|---|---|
| Windows 11, version 24H2 | Home, Pro, Pro Education, Pro for Workstations (and SE, for which 24H2 was the last version) | **13 Oct 2026** | 
| Office 2021 | Home & Student, Home & Business, Professional; Home & Student and Home & Business for Mac | **13 Oct 2026** | 
| Windows 11, version 25H2 | Home and Pro | October 2027 | 
| Windows 11, version 26H2 | Home and Pro (released 29 Sep 2026) | October 2028 | 
| Windows 10, version 22H2 with consumer ESU | Home, Pro, Pro Education, Workstations | **12 Oct 2027** (was Oct 2026) | 
| Office 2024 | Home, Home & Business (Windows and Mac) | October 2029 | 

Two things in that table surprise people. Windows 11 is not one product with one date: each yearly version of the Home and Pro editions gets 24 months of fixes, so a PC that simply stopped accepting feature updates two years ago is now out of support even though it runs "Windows 11". And Office has no paid lifeline the way Windows 10 does â when Office 2021's date passes, there is no programme to buy your way into another year.

## 2. Check what you run (two minutes)

- **Windows version:** press**Windows + R** , type`winver` , press Enter. You'll see something like "Version 24H2 (OS Build 26100â¦)". Or go to**Settings â System â About** and look under Windows specifications for the edition and version.
- **Office version:** open Word, go to**File â Account** and read the name under Product Information. "Microsoft 365" means a subscription that updates itself; "Office 2021" or "Office Professional Plus 2021" means you are affected. On a Mac: Word â About Word.
- **Windows 10 enrolment:** on a Windows 10 PC, open**Settings â Update & Security â Windows Update** . The top of the page shows whether the PC is enrolled in Extended Security Updates, or offers an Enroll now link if it qualifies and isn't.

## 3. Windows 11 24H2: move to 25H2 or 26H2

The good news first. Windows 11 24H2, 25H2 and 26H2 share one servicing branch â the same code base and the same monthly fixes, with newer versions switching on extra features. So moving from 24H2 to a newer version is not the hour-long reinstall it used to be. Microsoft delivers it as a small **enablement package** that installs much like a monthly update and usually needs one restart. Version 26H2 started rolling out on 29 September 2026 and is supported for 24 months on Home and Pro.

1. Install every pending update first: **Settings â Windows Update â Check for updates** , then restart.
2. Check again. If you see **"Feature update to Windows 11, version 26H2"** (or 25H2), choose**Download and install** . Turning on*Get the latest updates as soon as they're available* on the same page brings the offer forward.
3. Run `winver` afterwards to confirm the new version.

If nothing is offered, the usual reason is a **compatibility hold**: Microsoft keeps a feature update back from PCs with a driver or app it knows causes trouble. Microsoft has in the past moved Home and Pro PCs that are approaching end of servicing to a newer version automatically, but treat that as a safety net, not a plan. One exception worth knowing: PCs that shipped with Windows 11 version **26H1** â a separate branch that ships on some new PCs â can't move to 26H2, and Microsoft says they will get a path to a future release instead. They are not affected by the 13 October date.

## 4. Office 2021: pick a replacement

Office 2021 will keep opening your documents on 14 October. What changes is that any new hole found in Word, Excel, PowerPoint or Outlook 2021 will never be fixed â and documents and email attachments remain one of the most common ways malware is delivered to home users. A file-format bug in an office suite is the same class of problem as the malicious-PDF flaw Apple patched last week: you open what looks like an ordinary file, and the file does the rest.

- **Microsoft 365** (subscription) â always on the current version, includes OneDrive storage. Best if you share files with people who use Office heavily.
- **Office 2024** Home or Home & Business (one-time purchase) â supported until October 2029. The closest like-for-like replacement for Office 2021.
- **A free suite** such as LibreOffice â fine for most home documents and spreadsheets, as long as you keep it updated too. Complex macros and some Excel features don't always translate.
- **Stop-gap:** the free web versions of Word, Excel and PowerPoint, if you need to open something compatibly while you decide.

Until you switch, the old rules apply with extra force: don't enable macros in files from email or the web, keep Protected View on, and treat unexpected attachments â invoices especially, after the Fakturownia breach â as suspect until you've confirmed them another way.

## 5. Windows 10: the deadline that moved

If you bought a laptop in a panic last month, this is the annoying part. Microsoft's own ESU page now says consumers can enrol in Windows 10 Extended Security Updates **at any time until the programme ends on 12 October 2027**, and that PCs already enrolled stay covered to that date automatically. The original end date was October 2026, and plenty of guides still say so.

- **Who qualifies:** Windows 10 version 22H2, Home, Pro, Pro Education or Workstations, with the latest updates installed, signed in with a Microsoft account that is an administrator (not a child account). PCs managed by an employer use a separate business programme.
- **What it costs:** free if you sync your PC settings with Windows Backup; or 1,000 Microsoft Rewards points; or a one-time 30 US dollars plus tax. One licence covers up to ten PCs on the same Microsoft account. Options can differ in the European Economic Area.
- **What it is not:** ESU delivers critical and important security fixes only. No new features, no other bug fixes, no technical support.

The extra year is breathing room, not a reprieve. If your PC can run Windows 11, the upgrade is still the long-term fix; if it can't, you now have a year to replace it on your own schedule instead of this week's.

## 6. What antivirus can and can't do here

We get this question every time a support date passes: "I have a good antivirus â isn't that enough?" Partly. A security suite, Microsoft Defender included, stops known malware and flags suspicious behaviour, and that matters. What it cannot do is repair a flaw in Windows or Office. A newly discovered exploit for an unpatched system is the thing it is least likely to recognise on the first day, which is when it does the most damage. Our antivirus rankings and the Defender vs. paid antivirus comparison assume a patched system underneath. If you are reviewing what you pay for while you're at it, read our note on the year-two renewal price jump before your subscription renews.

**2027 heads-up: Windows Update certificates expire**

Microsoft announced on 8 October that a set of Windows Update certificates expires on **17 May 2027** and **19 June 2027**. PCs on unsupported versions of Windows will lose access to Windows Update entirely and receive no updates at all. Supported versions already have the replacement certificates if they are up to date: 25H2 and later need nothing; 24H2 needs the September 2025 update or later before 19 June 2027; other supported versions, Windows 10 with ESU included, need the July 2026 update or later. Staying on a supported version is about to stop being optional in a very literal way.

## 7. The ten-minute checklist

1. Run `winver` on every Windows PC in the house. Anything on 24H2 Home or Pro: install updates and take the 25H2/26H2 feature update.
2. Open File â Account in Word on each machine. Anything that says Office 2021: plan the switch this month.
3. On Windows 10 PCs, check the ESU status in Windows Update. Not enrolled? Use the free settings-sync option and enrol.
4. Make sure Microsoft Defender or your third-party suite is up to date and actually running.
5. Put 12 October 2027 in your calendar for any Windows 10 machine you're keeping.

For the rest of an autumn tidy-up â router firmware, password manager, browser â our October security stack refresh walks through it in one sitting.
