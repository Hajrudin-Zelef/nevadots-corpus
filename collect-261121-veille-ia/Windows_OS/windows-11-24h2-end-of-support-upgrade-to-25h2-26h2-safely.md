---
id: collect-261121-veille-ia/windows-os/windows-11-24h2-end-of-support-upgrade-to-25h2-26h2-safely
title: "Windows 11 24H2 End of Support: Upgrade to 25H2/26H2 Safely"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["windows", "october", "support", "home", "update", "upgrade", "version", "updates"]
source: https://dev.to/theglitchfixer/windows-11-24h2-end-of-support-upgrade-to-25h226h2-safely-40pp
source_anchor: ""
source_lines: [1, 94]
section: "Windows_OS"
---
*Last updated: October 31, 2026*

**Short answer:** Windows 11 24H2 support ended on October 13, 2026, for Home and Pro editions, so those PCs will no longer receive monthly security updates, bug fixes, or technical support. Your computer will not stop working, but newly discovered vulnerabilities will stay open, so upgrade to 25H2 or 26H2, which arrive as a small enablement package rather than a full reinstall.

On **13 October 2026**, Microsoft pulled the plug on **Windows 11 version 24H2** for Home and Pro editions. From now on, your 24H2 PC will no longer receive monthly security updates, bug fixes, or technical support. Your computer will not stop working — but every newly discovered vulnerability will stay open, and attackers notice exactly that kind of thing.

The good news: moving off 24H2 is one of the easiest Windows upgrades Microsoft has ever shipped. Versions 24H2, 25H2 and 26H2 share the same servicing foundation, so the upgrade arrives as a small enablement package rather than a full OS reinstall. This guide walks you through checking your version, backing up, and upgrading to 25H2 or 26H2 safely — plus what to do if Windows Update refuses to offer the new version.

**Update: October 28, 2026.** Microsoft's lifecycle page confirms Windows 11 24H2 Home and Pro reached end of updates on October 13, 2026, and lists version 26H2, released September 29, 2026, as supported until October 10, 2028. Version 25H2 remains supported until October 12, 2027, and Enterprise and Education editions of 24H2 remain supported until October 12, 2027. For PCs already on 24H2 or 25H2, 26H2 arrives as a small enablement package through Windows Update and completes with a single restart, and it is now the upgrade that gives the longest remaining support. If Windows Update has not offered 26H2 yet, check again after a few days, as Microsoft rolls new versions out in phases.

Microsoft gives each Windows 11 Home and Pro feature update roughly 24 months of support. Here is where things stand as of today:

- 
**Windows 11 24H2 (Home, Pro, Pro Education, Pro for Workstations)** — support**ended 13 October 2026** . The 13 October 2026 Patch Tuesday cumulative update was its last.
- 
**Windows 11 24H2 (Enterprise, Education)** — supported until**12 October 2027** . No action needed yet.
- 
**Windows 11 25H2 (Home and Pro)** — supported until**12 October 2027** .
- 
**Windows 11 26H2 (Home and Pro)** — released 29 September 2026, supported until**10 October 2028** .
- Windows 11 SE also ended October 13, 2026, with no later version path.

If you are on 24H2 Home or Pro, your upgrade path is 25H2 or 26H2. Both are delivered through Windows Update.

Before changing anything, confirm you are actually on 24H2:

1. Press **Windows key + R** to open the Run dialogue.
2. Type **winver** and press Enter.
3. Read the line that says **Version** . If it says**24H2** , you are affected by today's deadline.

You can also go to **Settings > System > About** and look under "Windows specifications" — it shows both the version and the edition (Home, Pro, Enterprise, etc.). Note the edition: Enterprise and Education editions of 24H2 get another year of support, so they can follow their IT department's schedule instead.

Feature updates are safe for the vast majority of PCs, but a backup turns a bad day into a minor inconvenience:

- Copy important documents, photos and project files to an external drive or cloud storage.
- If your drive is encrypted with BitLocker, save your recovery key now: go to **Settings > Privacy & security > Device encryption** (or search "Manage BitLocker") and back the key up to your Microsoft account or print it. Windows occasionally asks for this key after major updates — having it to hand saves a panic.
- Plug laptops into power and make sure you have a stable internet connection before starting the upgrade.

1. Open **Settings > Windows Update** .
2. Select **Check for updates** .
3. First, install any pending cumulative update — the Patch Tuesday release was 24H2's final security update, so take the final security update.
4. Then look for a **"Download and install"** option under**Windows 11, version 25H2** (or 26H2). Select it.
5. Restart when Windows asks you to. The download is small — this is an enablement package, not a full reinstall, so most PCs finish in well under an hour.
6. Run **winver** again and confirm the version now reads 25H2 or 26H2.

One caveat worth knowing: recent Windows updates have had their share of teething problems. If you have been holding off because of the headlines, two of the biggest recent bugs already have documented fixes — see our guides on how to fix games crashing after a Windows 11 update and what to do when Windows 11 audio breaks after an update. Read them before you upgrade so you know exactly what to do if anything misbehaves.

October's KB5127648 also left Smart App Control greyed out on some PCs — the official workaround is here.

Heads-up before you update: the October cumulative update carries a confirmed USB audio Code 10 bug that silences USB headsets — check the fix guide if your audio dies after installing.

Sometimes Windows Update stays silent even though a newer version exists. The most common reason is a **safeguard hold**: Microsoft temporarily blocks the upgrade on PCs with a known incompatible driver or application. This is protection, not a bug. Here is what to do:

- 
**Wait a few days and check again.** Safeguard holds are lifted as Microsoft and vendors ship fixed drivers. Forcing the upgrade around a hold is how people end up with broken hardware.
- 
**Update your drivers first.** Open Device Manager, check for driver updates — especially graphics, audio and network drivers — then re-check Windows Update.
- 
**Run the PC Health Check app** from Microsoft to confirm your PC meets the requirements for the newer version.
- 
**Use the official Windows 11 Installation Assistant** from Microsoft's website as a last resort. It performs the same upgrade Windows Update would, on your schedule. Only download it from microsoft.com — never from third-party "activator" sites.

Microsoft has also said that unmanaged 24H2 Home and Pro PCs will be moved to 25H2 automatically over time. You can wait for that — but doing it yourself means you choose the day and the restart time, instead of discovering it when Windows decides.

Whichever one Windows Update offers you is the right answer for your PC:

- 
**Windows 11 26H2** gives you the longest runway — support until 10 October 2028. If it is offered, take it.
- 
**Windows 11 25H2** is the confirmed, widely-available step off 24H2, supported until October 2027. Nothing wrong with landing here.

Either way you end up on a supported version with monthly security updates, which is the entire point.

Nothing dramatic happens tonight. Your PC boots tomorrow exactly as it does today, and your apps and games keep working. What changes is invisible and cumulative: every new vulnerability Microsoft patches for supported versions stays unpatched on yours. Security researchers find new flaws constantly, and unsupported systems are disproportionately targeted precisely because they cannot be fixed.

September's Patch Tuesday alone fixed two vulnerabilities that were already being actively exploited in the wild — that is the calibre of threat an unsupported PC simply absorbs. Antivirus software helps, but it is not a substitute for OS-level security patches. The upgrade is free, small, and takes under an hour. There is no good reason to stay.

No. Microsoft ended support for Windows 11 24H2 Home, Pro, Pro Education and Pro for Workstations on 13 October 2026. Enterprise and Education editions of 24H2 remain supported until 12 October 2027.

Your PC keeps working, but it stops receiving security updates, bug fixes and technical support. Each month the risk grows as new vulnerabilities are discovered and left unpatched. Upgrade to 25H2 or 26H2 to stay protected.

Go to Settings > Windows Update, click "Check for updates", then select "Download and install" under Windows 11, version 25H2. Restart when prompted, then run winver to confirm the new version.

Microsoft's notice names 25H2 as automatic upgrade for unmanaged Home/Pro; 26H2 offered in phases, support until Oct 10, 2028 — take whichever Windows Update offers.

Take whichever version Windows Update offers. 26H2 gives the longest support (until 10 October 2028); 25H2 is supported until 12 October 2027.

The October 13, 2026 Patch Tuesday cumulative security update is the final scheduled update for Windows 11 24H2 Home and Pro. After it, no further monthly updates will be released for this version.

An enablement package — well under an hour, single restart.

It is likely a safeguard hold — wait days, do not force around the hold.

## Top comments (0)
