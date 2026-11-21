---
id: collect-261121-veille-ia/windows-os/november-2026-windows-update-problems-and-fixes-kb5132061-kb5131096
title: "November 2026 Windows Update Problems and Fixes (KB5132061, KB5131096)"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-11"]
keywords: ["windows", "update", "microsoft", "november", "kb5132061", "kb5131096", "home", "slmgr"]
source: https://yourlocalcomputerguy.co.uk/blog/november-2026-update-problems.html
source_anchor: ""
source_lines: [1, 136]
section: "Windows_OS"
---
# November 2026 Windows Update Problems and Fixes (KB5132061, KB5131096)

November's Windows update has been out for 11 days. Here's what's gone wrong and what you can do about it.

Eight problems so far, and Microsoft has confirmed six of them. Most won't touch a normal home PC. One stopped the Windows 11 update installing on some work and school machines, and Microsoft fixed it from its end on 16 November. Another broke a command-line activation tool that IT people use, mainly on Windows 10, and Microsoft released a fix for that one on 13 November. There's a SQL Server bug on Windows 11 that actually started with October's update, and a stricter check on signed PowerShell scripts that only matters to IT departments. There's also a set of reports about printers, Citrix and Sage 50 failing after the update, which Microsoft hasn't acknowledged. The newest is a bug in the `KB5132043` hotpatch update that makes some apps fail with error `0xC0000099`, and it only hits business PCs managed through Windows Autopatch.

The two a home user might actually notice are a printing bug with some documents and the old right-click menu tweak on Windows 11 no longer working. Compared with October's list, it's still fairly quiet. Microsoft has also put out an optional preview update, `KB5132057`, which I've covered near the end.

## KB5132061 won't install on Windows 11 24H2 Enterprise and Education (0x80070306, 0x800f0989, 0x800f0993)

On some Windows 11 24H2 machines, this month's update (`KB5132061`) fails to install, or it keeps trying again and again. Microsoft lists three error codes for it: `0x80070306`, `0x800f0989` and `0x800f0993`.

It only affects the Enterprise and Education editions of 24H2. Those are the versions you find on work laptops and school machines managed by an IT department. If your PC came from a shop with Windows 11 Home or Pro on it, this isn't your problem.

You don't need to do anything. Microsoft says affected devices should be repaired automatically within 24 to 48 hours, starting from 11 November 2026 at 10am Pacific time (6pm UK time). So if a work laptop keeps nagging about a failed update, leave it switched on and connected and give it a couple of days. If it's still failing after 48 hours, Microsoft says to contact its support.

**Update (18 November):** Microsoft marked this one resolved on 16 November. The automatic repair ran on affected devices that were online and getting updates through Windows Update, and those devices then install the November update through Windows Update as normal. Nobody needs to do anything by hand.

This one got plenty of coverage, including BleepingComputer's write-up. Microsoft's own KB5132061 page is where any change will show up first.

If you're on Home or Pro and you're seeing one of these codes, that's something different and Microsoft's automatic repair won't cover it. Get in touch and I'll take a look.

## Windows 10 KB5131096: slmgr commands fail with error 0x80041008

After installing `KB5131096` on Windows 10, the `slmgr.vbs` tool can fail with error `0x80041008` and the message "Invalid parameter (SWbemObjectEx)".

`slmgr.vbs` is a script for managing Windows activation from the command line. Microsoft says it can fail when you use it to activate Windows, to point a PC at a company's KMS activation server (`slmgr.vbs /skms-domain <domain>`), or to put in a product key (`slmgr.vbs /ipk <key>`).

That's a business problem. KMS is how larger organisations activate lots of PCs at once, and typing slmgr commands is an IT job. Microsoft doesn't say anything about normal activation on home PCs being affected, and I wouldn't worry about it on a Windows 10 machine at home that's already activated.

There aren't any steps to follow here. Microsoft says a workaround is available to organisations that need it straight away, through Microsoft Support for business, and that it expects a proper fix in the coming days. If you look after work PCs and it's holding you up, that's the route. Everyone else can wait for the fix to arrive through Windows Update. The details are on Microsoft's KB5131096 page.

**Update (13 November):** the fix is out. Microsoft says the problem is resolved in Windows updates released on and after 13 November 2026, such as `KB5136493`. That's an out-of-band update for Windows 10 22H2 (OS Builds 19045.7850 and 19044.7850). If you look after Windows 10 PCs that need activating or re-keying, install the latest update on them first and the slmgr commands should work again. It's the organisations running these commands by hand, or through scripts and deployment tools, that need to act on this.

Microsoft released matching out-of-band fixes the same day for other versions too: `KB5136483` for Windows 11 23H2 (build 22631.7714), `KB5136490` for Windows 10 1809 and LTSC 2019, and updates for Windows Server 2025 and 2022. Same advice for those. Install the latest update before running slmgr.

## SQL Server linked server connections might stay open on Windows 11 24H2, 25H2 and 26H2

Microsoft confirmed on 11 November 2026 that SQL Server linked server connections might remain open on Windows 11 24H2, 25H2 and 26H2. A linked server is a SQL Server setting that lets one database server query another, so this is firmly in IT territory.

November's update didn't cause this one on its own. Microsoft says it goes back to October's update (`KB5127648`), which means holding off November's update won't make it go away.

If you run SQL Server for a business, it's worth a look. If your PC is for email, browsing and the odd game, you almost certainly haven't got SQL Server on it, let alone linked servers, and you can ignore it.

I'm not going to give steps for this, because Microsoft hasn't published any. It says a mitigation is available for affected enterprise devices through Microsoft Support for business, and that an out-of-band update (one released outside the normal monthly cycle) is coming in the next few days. If it's hitting your servers, contact Microsoft Support for business, and watch Microsoft's Windows release health page for the update.

## Documents with an invisible image or watermark might not print

Microsoft is investigating a printing bug that came in with this month's updates. A document can fail to print if it contains an image that ends up with zero height or width at the printer's resolution. Microsoft's example is an invisible watermark.

So far it's been seen when printing Word documents through GDI, the older Windows printing route that most desktop programs still use. Microsoft says other apps might be affected as well. It covers Windows 11 (`KB5132061`, `KB5132059` and `KB5131087`), Windows 10 (`KB5131096`) and Windows Server.

Of everything on this list, this is the one most likely to reach a home PC. Most documents won't have an image like that in them. A template from work, or a document someone else put together, could have one tucked away without you knowing.

There's no fix or workaround from Microsoft yet. It's tracking the problem as WI1549472 and I'll update this post when that changes.

In the meantime, this is my general advice, not something Microsoft has published: if one document won't print but others do, try removing the watermark or any images you don't need and print it again. Or export the document to PDF and print the PDF instead. Neither is guaranteed to work, but both take a minute to try.

Don't mix this up with printer error 87 on printers published in Active Directory. That's a separate problem on business networks, and November's update fixes it.

## KB5132043 and KB5132061: printers not connected, Citrix Workspace won't launch, Sage 50 crashing

Since `KB5132043` (the hotpatch release) and `KB5132061` went on to Windows 11 24H2, 25H2 and 26H2, some people are reporting three separate faults. Printers show as not connected, or the print spooler (the Windows service that handles print jobs) keeps stopping. Citrix Workspace App, the LTSR version, refuses to launch. And Sage 50 2026.2 crashes, mostly in the supplier bills module.

Microsoft hasn't acknowledged any of it. Its page for `KB5132043` lists no known issues. The reports come from Neowin, threads on Microsoft Q&A and a Sage 50 support blog.

Nearly all of it is coming from businesses. Citrix and Sage 50 are office software, so if you've never heard of either, the only part that could apply to you is the printing. This also isn't the invisible watermark bug above, which Microsoft has confirmed and which only hits certain documents.

No official fix yet. The only remedy people have reported is uninstalling the update, and that takes this month's security fixes off with it, including the fix for an Outlook flaw that's already being used in attacks. If your business relies on Citrix or Sage 50, talk to your IT support or your software supplier before you uninstall anything. They can weigh that up for your setup.

For the printer problem, the Sage 50 support blog says reinstalling the printer may help. Try that first, before going anywhere near removing an update. I'll add to this section if Microsoft confirms it.

## KB5132043 hotpatch update: apps fail with error 0xC0000099 (ERROR_ALLOTTED_SPACE_EXCEEDED)

Microsoft has confirmed that after the hotpatch update `KB5132043`, apps that load the 32-bit `msi.dll` (part of Windows Installer) can fail with error `0xC0000099`, ERROR_ALLOTTED_SPACE_EXCEEDED. It applies to Windows 11 24H2 and later. Born's Blog names Outlook, other Office apps and some third-party apps.

You won't find it on Microsoft's public `KB5132043` page, which still says there are no known issues. Microsoft published it as a service alert (WI1559710) in the admin centre on 18 November, so it's IT admins who will have seen it.

Hotpatch updates put security fixes on without a restart. BleepingComputer says the bug affects hotpatch updates delivered through Windows Autopatch, and PCs that install the standard Patch Tuesday update aren't affected. Autopatch is a service businesses use to manage updates on their PCs. If you've got Windows 11 Home or Pro at home and updates come through Windows Update as normal, you're not on hotpatch and you can ignore this one.

There's no fix yet. Microsoft says it will fix it in a later update, and until a fix is available it has given IT admins a registry workaround. Microsoft also says to back up the registry before changing anything.

1. Open the Start menu, type "regedit", and select Registry Editor from the results.
2. Navigate to the HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management folder.
3. In the right-hand panel, locate the "HotPatchTableSize" registry key, open it, and set the Value to "2000". Click OK.
4. Close the registry editor and restart the device.

Neither source says what type of value `HotPatchTableSize` should be. If it isn't there already, check Microsoft's alert in the admin centre before you create it by hand.

The restart is the awkward bit. As Born's Blog points out, the point of hotpatching is that machines don't need restarting, and this workaround needs one. On a fleet of managed PCs that means scheduling it like any other reboot.

## Signed PowerShell scripts with invalid UTF-8 fail signature validation

Microsoft made this change on purpose. Since `KB5132061` on Windows 11 and `KB5131096` on Windows 10, PowerShell checks the text encoding of signed scripts more strictly. A signed script with invalid UTF-8 in it might now fail signature validation, even if it ran fine last month.

Script signing is an IT job. Companies sign their PowerShell scripts so their machines only run code they trust. If you've never signed a script, you can skip this one.

For anyone who has, Microsoft's guidance is to save the affected script as valid UTF-8 with a byte order mark (BOM), then sign it again. It's covered under "Encoding Guidance for Signed PowerShell Scripts" on Microsoft's KB5132061 page. Re-signing needs your organisation's code-signing certificate, so it falls to whoever looks after your scripts.

## Windows 11 classic right-click menu registry tweak stops working after KB5132061

If you used a registry tweak to bring back the old, full right-click menu on Windows 11, it stops working after `KB5132061`. You get the newer, shorter menu back.

Microsoft hasn't said anything about it, but it looks deliberate. Pureinfotech and WinCentral both report that the update replaces the code behind the right-click menu, so Windows now ignores the old tweak. The same update adds a right-click menu you can customise, which seems to be Microsoft's answer to people wanting the old one back.

If you never applied the tweak, there's nothing to fix.

If you did, there's no supported registry alternative, and I wouldn't go hunting for a new hack that the next update could break just the same. Hold Shift while you right-click and you get the full menu straight away. Shift + F10 does the same from the keyboard. Microsoft's KB5132061 page has the details of what changed in the update.

## KB5132057: the optional November preview update for Windows 11 25H2 and 26H2

Microsoft released `KB5132057` on 19 November. It's an optional preview update for Windows 11 25H2 and 26H2 (OS builds 26200.10075 and 26300.10075), and it has no security fixes in it. There's no November preview for 24H2.

It's reported to fix boot failures and TPM initialisation errors on Arm devices that use the TPM 2.0 CRB interface. It also brings a redesigned results list in Windows Search, and searching from This PC in File Explorer now uses the Windows indexer. The new features are rolling out gradually, so installing it doesn't mean you'll see them straight away.

Microsoft hasn't listed any known issues for it yet. It won't install by itself either. You only get it if you go into Windows Update and choose to download and install it.

On a home PC I'd leave it. This update becomes the mandatory one in December's Patch Tuesday, so you'll get the same changes then without doing anything. The only case where I'd think about installing it now is an Arm laptop with the boot or TPM fault it's meant to fix.

## Should you still install the update?

Yes. For most people I can't see a reason to hold off.

This month patched 1,235 vulnerabilities, including an Outlook flaw that's already turning up in phishing emails. The full rundown is in my November update post. The problems above are narrow. One has already fixed itself on managed work machines, one only hit people running activation commands and is now fixed, the SQL Server one came in with October's update anyway, and the PowerShell change only matters if you sign scripts. The Citrix and Sage 50 reports are unconfirmed and almost all from businesses. The hotpatch bug only hits managed business PCs on Windows Autopatch. The printing bug is the one a home user is most likely to run into, and even that needs a document with an odd invisible image in it. Losing the old right-click menu is annoying, but Shift + right-click still gets you there. None of them is a reason to leave a home PC unpatched.

If you look after Windows 10 PCs at work and need to activate or re-key machines, there's no longer any reason to pause `KB5131096` on them. Install the latest update, including `KB5136493`, and the activation commands are fixed.

I'll add to this post if anything else turns up over the next few weeks. And if an update has left your PC in a mess, that's what my computer repairs in St Helens are for.

Last month's biggest headache was USB headsets going silent with a Code 10 error, and Microsoft says November's update finally fixes it. That, the Xbox sign-in error `0x87DD0005` and the rest of October's list are in my October update problems post.

Mark has been fixing computers since the late '90s and went self-employed in 2008. Based in St Helens since 2013, he works evenings and weekends from his home in Laffak, doing friendly, affordable repairs for PCs, laptops, and Macs. See reviews on Google

### Update gone wrong? Get in touch.

If this month's update won't install, Windows is complaining about activation, or something else has broken since the update, I can sort it.
