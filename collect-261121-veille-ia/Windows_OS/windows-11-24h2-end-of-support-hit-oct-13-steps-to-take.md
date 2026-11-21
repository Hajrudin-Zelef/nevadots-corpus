---
id: collect-261121-veille-ia/windows-os/windows-11-24h2-end-of-support-hit-oct-13-steps-to-take
title: "Windows 11 24H2 End of Support Hit Oct. 13: Steps to Take"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["windows", "update", "microsoft", "support", "security", "updates", "editions", "education"]
source: https://windows.gadgethacks.com/news/windows-11-24h2-end-of-support-hits-oct-13-steps-to-take/
source_anchor: ""
source_lines: [1, 85]
section: "Windows_OS"
---
Windows 11 24H2 reached end of support Oct. 13, 2026, for Home, Pro, Pro Education, and Pro for Workstations editions. The deadline has already passed, and the October security update was the last monthly update for those editions.

Microsoft says affected PCs no longer receive monthly security and non-security preview updates containing protections against newly discovered threats.

Enterprise and Education editions of 24H2 get another year, with support running until Oct. 12, 2027.

Here's the sequence to work through: confirm your version and edition, back up important files and locate your BitLocker recovery key, then install whichever supported feature update Windows Update offers. Both 25H2 and 26H2 are now supported upgrade destinations for eligible 24H2 PCs, with 26H2 reaching general availability Sept. 29.

Press **Windows key + R**, type `winver`, and press Enter to see the installed Windows version.

If that dialog reads "24H2," check the edition under **Settings > System > About**.

Home, Pro, Pro Education, and Pro for Workstations editions have reached the end of updates. Enterprise and Education on 24H2 remain supported until Oct. 12, 2027.

Three outcomes matter here. If you're running an affected 24H2 edition, back up and update now. If you're on 24H2 Enterprise or Education, this particular deadline hasn't arrived yet. Any other version number, such as 25H2, 26H1, or 26H2, puts the PC on a different servicing schedule.

Oct. 13 was a patch-eligibility cutoff, not a shutdown date. A Windows 11 24H2 PC doesn't suddenly stop working because support ended.

Microsoft says affected editions no longer receive monthly security and non-security preview updates containing protections against the latest security threats. Before the deadline, Microsoft also warned that end of servicing meant losing fixes for known issues, time zone updates, technical support, and monthly security and preview updates.

The October 2026 security update was the final regular monthly update for these editions.

That doesn't mean an unpatched PC becomes compromised the next day. It means newly discovered Windows vulnerabilities can go unpatched on that installation going forward, making the security gap more significant over time.

Back up important files before installing a feature update.

Windows Backup can preserve supported files, settings, preferences, credentials, and information used to restore apps on a new or reset PC. A free Microsoft account includes 5GB of OneDrive storage, so check whether that's enough for the files you need or use another backup method, such as an external drive.

Locate your BitLocker recovery key too if device encryption or BitLocker is enabled.

A BitLocker recovery key is a 48-digit code Windows can request when it can't automatically unlock an encrypted drive. Microsoft warns that it can't retrieve, provide, or recreate a lost BitLocker recovery key for you.

That's not a reason to expect an ordinary Windows feature update to trigger recovery. It's a precaution worth taking before a major OS update so you know where the key is if Windows does request it.

Open **Settings > Windows Update** and select **Check for updates**.

Microsoft currently recommends moving affected 24H2 PCs to Windows 11 25H2 or 26H2.

Windows 11 25H2 remains supported for Home and Pro through Oct. 12, 2027.

Windows 11 26H2 is no longer an unsettled future release. Microsoft made it generally available Sept. 29, 2026, and lists support for Home and Pro through Oct. 10, 2028.

Microsoft says 26H2 is rolling out gradually to eligible 24H2 and 25H2 devices. PCs with **Get the latest updates as soon as they're available** enabled can receive it when Microsoft determines the update is ready for that device.

If Windows Update offers 26H2 directly, there's no need to install 25H2 first. Microsoft says 26H2 uses servicing technology to provide a relatively fast installation experience for eligible 24H2 and 25H2 PCs.

If Windows Update offers 25H2 instead, that's also a supported way off 24H2. You don't need to force 26H2 simply because it's newer.

Look for a compatibility warning or other explanation in Windows Update before trying anything else.

Microsoft can place a safeguard hold on a feature update when it identifies a known compatibility problem involving particular hardware, software, or drivers. These holds temporarily prevent affected devices from receiving an update until the problem is resolved.

Don't bypass a safeguard hold simply to get off 24H2 faster. Microsoft specifically warns against manually overriding these protections in unsupported scenarios because doing so can leave the PC with known performance or reliability problems.

If Windows Update doesn't identify a hold and your PC should be eligible, use Microsoft's official installation tools and support resources rather than downloading Windows images from third-party sites.

An in-place installation using official Windows installation media can preserve personal files and apps when the appropriate option is available. Confirm that **Keep personal files and apps** is selected before proceeding.

If that option isn't available, or Setup reports a compatibility problem, stop and investigate before continuing rather than falling into an unintended clean installation.

Some PCs released in early 2026 run Windows 11 26H1, a separate Windows release intended for certain newer hardware.

If `winver` shows 26H1, the 24H2 deadline doesn't apply. Microsoft lists 26H1 Home and Pro support through March 14, 2028.

There's an important wrinkle: Microsoft says 26H2 isn't offered as an in-place update to 26H1 devices. Those PCs remain on their separate servicing path, so don't try to force the 24H2-to-26H2 upgrade instructions onto a 26H1 system.

Windows 10 Extended Security Updates don't extend Windows 11 24H2 support. They're a separate program for Windows 10 devices and aren't an escape hatch for a Windows 11 installation that has reached end of servicing.

One more 2026 issue is worth checking after updating: Microsoft's original Secure Boot certificates started expiring this year. The Windows Production PCA 2011 certificate expires Oct. 19, while other 2011 certificates began expiring in June.

Microsoft is distributing replacement 2023 certificates through Windows Update, and most consumer PCs will receive them automatically. A PC without the replacements will continue to boot and receive normal Windows updates, but it can eventually lose access to new protections for the early boot process.

That's separate from the Windows 11 24H2 support deadline, not a reason to delay upgrading.

Run `winver`, then check **Settings > System > About** for your edition.

If the result is an affected Windows 11 24H2 edition, the Oct. 13 deadline has already passed. Back up important files, locate your BitLocker recovery key if encryption is enabled, then open **Settings > Windows Update > Check for updates**.

Install Windows 11 26H2 if Microsoft offers it to your device. If Windows Update offers 25H2 instead, that's also a supported upgrade and gets the PC off the now-unsupported 24H2 release.

Don't force an update past a compatibility hold. The goal isn't simply to make the version number bigger. It's to move the PC onto a currently supported Windows release that Microsoft has determined is ready for its hardware.

## Comments

Be the first, drop a comment!
