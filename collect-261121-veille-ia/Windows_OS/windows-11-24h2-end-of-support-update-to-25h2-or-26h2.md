---
id: collect-261121-veille-ia/windows-os/windows-11-24h2-end-of-support-update-to-25h2-or-26h2
title: "Windows 11 24H2 End of Support: Update to 25H2 or 26H2"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-10-13"]
keywords: ["windows", "version", "update", "microsoft", "updates", "october", "support", "home"]
source: https://noxkeys.com/windows-11-24h2-end-of-support-update-25h2/
source_anchor: ""
source_lines: [1, 179]
section: "Windows_OS"
---
Quick answer

Windows 11 Home and Pro version 24H2 stopped getting security updates on **13 October 2026**. To fix it, open **Settings › Windows Update**, turn on **Get the latest updates as soon as they’re available**, select **Check for updates**, then **Download & install** the 25H2 or 26H2 feature update. It’s a small enablement package, so on most PCs it takes 5–10 minutes and one restart. Your files, apps and activation stay exactly as they are.

If your PC still says “Version 24H2” and you run Windows 11 Home or Pro, it stopped getting security fixes on 13 October 2026. The next Patch Tuesday, 10 November 2026, will skip your machine entirely. Every Windows vulnerability found from now on stays open on that PC.

The good news: this is one of the easiest upgrades Microsoft has ever shipped. Going from 24H2 to 25H2 (or 26H2) is a small “enablement package” and a single restart, not a reinstall. 24H2, 25H2 and 26H2 share the same core code, so Windows only switches on what’s already there.

This guide shows you how to check your version in 20 seconds, which version to pick, four ways to update, and what to do when Windows Update refuses to offer it.

## What Windows 11 24H2 End of Support Actually Means

Microsoft gives each Windows 11 version a fixed lifespan: 24 months for Home and Pro, 36 months for Enterprise and Education. 24H2 launched on 1 October 2024, so the consumer clock ran out in October 2026. You can confirm every date on Microsoft’s Windows 11 Home and Pro lifecycle page.

### Which editions are affected

### What stops and what keeps working

Your PC won’t shut down or lock you out. Windows keeps booting, your apps keep running and your licence stays activated. What stops is everything that keeps it safe and accurate:

- **Monthly security updates:** the important one. Ransomware gangs actively scan for unpatched Windows builds.
- **Bug fixes and time zone updates:** if a country changes its daylight-saving rules, your clock won’t follow.
- **Microsoft support:** contact Microsoft about a problem and the first answer will be “update to a supported version.”

**Antivirus alone won’t cover you**

Microsoft Defender’s virus definitions keep updating for a while, but they can’t fix holes in Windows itself. Antivirus on an unpatched OS is like a good lock on a door with a broken frame.

## How to Check Your Windows 11 Version

It takes about 20 seconds. Use whichever method you like.

### 1. The fastest way: winver

Press Win + R, type `winver` and press Enter. The About Windows box shows your version and build number on the second line.

### 2. Through Settings

Go to **Settings › System › About** and scroll to **Windows specifications**. Note both **Edition** and **Version**. A Pro PC on 24H2 needs updating now; an Enterprise PC on 24H2 has until October 2027.

### 3. With PowerShell (handy for several PCs)

Open Terminal and run this one-liner. It returns only the version name, which makes it easy to check every PC in a small office or around the house:

(Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion").DisplayVersion

## 25H2 or 26H2: Which Version Should You Update To?

Both are free, both install as a small enablement package, and both keep your files and apps. The real difference is how long you’re covered before you have to do this again.

**Our take:** if Windows Update offers 26H2, take it. You get an extra year before doing this again. Choose 25H2 if you manage business PCs and prefer the version with a year of fixes behind it, or if 26H2 is blocked on your machine (see the fixes further down).

### Heads-up: WMIC and PowerShell 2.0 are gone in 25H2

This is the one change that catches people out. The old command-line tool WMIC and the legacy PowerShell 2.0 engine are removed. For example, the popular command `wmic path softwarelicensingservice get OA3xOriginalProductKey`, used to read the Windows key stored in a laptop’s firmware, stops working after the update. The PowerShell replacement does the same job:

(Get-CimInstance -ClassName SoftwareLicensingService).OA3xOriginalProductKey

If you rely on old batch scripts, search them for `wmic` before you update. Our guide to finding your Windows product key using Command Prompt covers other ways to get the key.

## Before You Update: A 5-Minute Checklist

None of these are strictly required, but they prevent nearly every failed update we see.

1. **Back up anything you can’t lose.** The update keeps your files, but a power cut mid-install is the one scenario where you’ll want a copy. OneDrive or a USB drive is fine.
2. **Free up at least 20 GB.** The enablement package is tiny, but Windows needs room for the cumulative update it depends on.
3. **Plug in your laptop.** Windows can refuse to install feature updates on low battery.
4. **Know how your PC is activated.** Most PCs use a digital licence that survives any update. Not sure? Read digital licence vs product key.
5. **Disconnect VPN software.** Some VPN clients make installs fail with error`0x800f0922` .

## How to Update Windows 11 24H2 to 25H2: 4 Methods

All four methods end in the same place: a supported version of Windows 11 with your files, apps and activation untouched. Start with Method 1; most people never need the others.

### Method 1: Windows Update (recommended)

1. Open **Settings** (Win + I) and select**Windows Update** .
2. Turn on **Get the latest updates as soon as they’re available** .
3. Select **Check for updates** . Windows first installs any missing cumulative updates. Restart if it asks.
4. Check again. A card appears for **Windows 11, version 25H2** (or**version 26H2** ) with a**Download & install** button.
5. Select **Download & install** , then**Restart now** when it’s ready. That’s it.

**Example:** on a typical 24H2 laptop that’s already fully patched, the feature update appears straight away and the whole process, restart included, takes under 10 minutes. A PC that hasn’t been updated since the summer needs one or two cumulative updates first, so set aside 30–45 minutes and expect two restarts.

**You might already be done**

Microsoft is already moving unmanaged 24H2 Home and Pro PCs to 25H2 automatically. If your PC restarted recently and `winver` now says 25H2, you’re protected. Just check that Windows Update shows no errors.

### Method 2: Install the enablement package manually (KB5054156)

The 25H2 enablement package is KB5054156. It only installs on a 24H2 PC that already has the August 2025 preview update (build 26100.5074) or any later cumulative update. If you’ve installed any monthly update since then, you qualify.

This method is handy when Windows Update is stuck on “checking”, or when you’re updating several PCs from one download on a USB stick.

### Method 3: Windows 11 Installation Assistant

Download the Installation Assistant from Microsoft’s Windows 11 download page, run it and follow the prompts. It installs the latest version Microsoft currently offers and keeps your files and apps. Because it reinstalls Windows in the background, expect around an hour. Use it when Methods 1 and 2 fail with errors.

### Method 4: In-place upgrade with an ISO

On the same Microsoft page, download the Windows 11 ISO. Right-click it, choose **Mount**, then run `setup.exe` and pick **Keep personal files and apps**. This is the most reliable fix for a Windows installation that’s badly broken. If you’d rather start fresh, put the ISO on a stick with our Windows 11 bootable USB guide.

## Windows 11 25H2 Not Showing in Windows Update? Try These Fixes

### 1. A safeguard hold is blocking your PC

Microsoft holds the update back from PCs with drivers or apps known to clash with it, often older audio, Wi-Fi or anti-cheat drivers. Check the Windows 11 release health page for active holds. The fix is usually a newer driver from your PC maker’s website. Don’t force the update past a hold: it exists because something on your PC is likely to break.

### 2. A policy is pinning your PC to 24H2

This one is common on second-hand business laptops and on PCs that once had an “update blocker” tweak. Press Win + R, type `regedit` and go to:

HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate

Restart, then select **Check for updates** again. The feature update usually appears within a few minutes.

**Is it a work PC?**

If your PC is managed by your employer or school, ask IT before changing this. They may have pinned the version on purpose and will update it on their own schedule.

### 3. You’re missing the prerequisite update

If Windows Update offers nothing at all, install the latest cumulative update first, restart, then check again. The enablement package won’t appear until the base build is recent enough.

### 4. The install fails with an error code

For any other code, look it up in our Windows error code lookup.

## What If Your PC Doesn’t Meet Windows 11 Requirements?

Plenty of people got 24H2 onto older PCs using workarounds such as skipping the TPM 2.0 check. Windows Update usually won’t offer feature updates to those machines. The enablement package from Method 2 often still installs, because 25H2 is built on the same code as 24H2 and doesn’t re-check the hardware. Microsoft doesn’t officially support this, though, and a future update could stop it working.

Run your PC through our Windows 11 compatibility checker to see exactly what’s holding it back. Often it’s just TPM or Secure Boot switched off in the BIOS, which takes two minutes to turn on.

## Will Updating to 25H2 Affect My Windows Activation?

No. An enablement package doesn’t touch your licence. A PC activated on 24H2 stays activated on 25H2 and 26H2, whether it uses a digital licence or a product key.

Activation only becomes an issue after a clean install on new hardware or a motherboard swap. For those, see how to reactivate Windows 11 after a hardware change. If a watermark does appear after updating (rare, and usually a sign the PC was never properly licensed), this guide will help: fix “Windows is not activated” permanently.

## Running Windows 11 Enterprise or Education?

You have until 12 October 2027 on 24H2, so there’s no emergency. Use the time to test 25H2 or 26H2 on a pilot group, check line-of-business apps and scripts for `wmic` and PowerShell 2.0 dependencies, then deploy through Intune, WSUS or Configuration Manager. Going straight to 26H2 buys you support until October 2029.

Running Pro across a small business and wondering whether you need more? Our Windows 11 Home vs Pro comparison explains which business features each edition includes.

## Frequently Asked Questions

### Can I keep using Windows 11 24H2 after support ends?

Yes, it keeps working, but it no longer gets security patches. For a PC that browses the web, opens email attachments or does online banking, that risk grows every month. The update is free and takes minutes, so there’s little reason to stay.

### Does updating from 24H2 to 25H2 delete my files or apps?

No. Methods 1, 2 and 3, and Method 4 with “Keep personal files and apps” selected, all keep your files, apps and settings. Backing up first is still sensible.

### Can I skip 25H2 and go straight from 24H2 to 26H2?

Yes. Microsoft offers 26H2 directly to eligible 24H2 and 25H2 PCs through Windows Update. You don’t need to install 25H2 first.

### How big is the 25H2 update?

The enablement package itself is tiny; the 26H2 package is just 174 KB, according to Windows Latest’s testing. Most of the download is the cumulative update your PC needs first.

### Can I go back to 24H2 if something breaks?

Within 10 days, use **Settings › System › Recovery › Go back**. Treat it as a short-term escape while you wait for a driver fix, since going back puts you on an unsupported version again.

### Will updating to 25H2 affect my Windows activation?

No. An enablement package doesn’t change your licence. A PC activated on 24H2 stays activated on 25H2 and 26H2.

### Is Windows 12 coming soon instead?

Microsoft hasn’t announced a Windows 12 release, and waiting for it means running unpatched in the meantime. Update to 25H2 or 26H2 now. Our Windows 12 release date tracker follows what’s confirmed and what’s rumour.

## Final Verdict

If you’re on Windows 11 Home or Pro 24H2, your PC is already missing security fixes. Open Windows Update, flip the “get the latest updates” switch and install 25H2 or 26H2 today. For most people it’s a single restart, with nothing lost and no reactivation needed. If Windows Update won’t cooperate, the manual enablement package (KB5054156 for 25H2, KB5121794 for 26H2) usually gets you there in under five minutes.
