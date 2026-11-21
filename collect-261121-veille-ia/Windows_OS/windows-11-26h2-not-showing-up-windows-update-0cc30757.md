---
id: collect-261121-veille-ia/windows-os/windows-11-26h2-not-showing-up-windows-update-0cc30757
title: "Windows 11 26h2 not showing up windows update"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-10-28"]
keywords: ["windows", "update", "version", "check", "page", "microsofts", "offer", "updates"]
source: ""
source_anchor: ""
source_lines: [1, 274]
section: "Windows_OS"
---
Windows Update says “You’re up to date,” but the Windows 11 version 26H2 update (the Windows 11 2026 Update) never shows up. Microsoft lists 26H2 as available for all eligible devices, so a missing offer usually points to an eligibility gap, a safeguard hold, an IT policy, or a local Windows Update glitch that you can check in a few minutes. See also our guide on How to Check Your Laptop Battery Health and Cycle Count in Windows 11.
Windows 11 Version 26H2: Quick Answer
Windows 11, version 26H2 is Microsoft’s 2026 annual feature update for Windows 11. PCs already on version 24H2 or 25H2 get it through Windows Update as an enablement package, and Microsoft’s release-health page lists it as available for all eligible devices. Windows 10 PCs need a full upgrade to Windows 11 instead, and a safeguard hold or management policy can still keep the offer off an eligible PC.
| Method / Fix | Best For | Quick Steps / Key Detail | 
|---|---|---|
| Fix #1: Confirm 24H2 or 25H2 | Anyone unsure of their version | Settings > System > About > Version | 
| Fix #2: Check for updates | Eligible PCs with no offer yet | Turn on early availability, click Check for updates | 
| Fix #3: Check for a safeguard hold | One PC that never gets the offer | Look for Learn more in Windows Update, review known issues | 
| Fix #4: Run the troubleshooter | “You’re up to date” for days | System > Troubleshoot > Other troubleshooters | 
| Fix #5: Update drivers | Older PCs or custom hardware | Optional updates, then the manufacturer’s support site | 
| Fix #6: Free disk space | Nearly full C: drive | System > Storage > Temporary files | 
| Fix #7: Installation Assistant | x64 PCs with no hold, after waiting | Microsoft’s Download Windows 11 page | 
| Fix #8: Microsoft Update Catalog | Admins patching PCs already on 26H2 | Install a specific KB, never a generic “26H2” package | 
Quick Diagnosis
These checks take about two minutes. They tell you whether something is actually wrong or whether Windows is holding the offer on purpose.
- Open Settings > System > About and check the Version field. You need 24H2 or 25H2 to get 26H2 through Windows Update.
- Open Settings > Windows Update and click Check for updates.
- Look for the Get the latest updates as soon as they’re available toggle on the same page and note whether it’s on.
- Look for the message “Some settings are managed by your organization” near the top of the Windows Update page. If it’s there, your PC’s update policy is controlled by a work or school administrator.
- Check Microsoft’s Windows 11, version 26H2 known issues page for anything that matches your hardware or apps.
If you’re on 24H2 or 25H2, the PC isn’t managed by an organization, and nothing on the known issues page applies to you, work through the fixes below in order.
On a managed PC, a missing offer is often intentional. Windows Update policies, feature-update deferrals, Intune, Configuration Manager, or Windows Autopatch can all decide when 26H2 arrives. Ask your IT department before trying any of the fixes below.
What 26H2 Is and Why It May Not Appear Yet
Windows 11, version 26H2 is Microsoft’s annual feature update for Windows 11. On PCs already running 24H2 or 25H2, it’s delivered as a small enablement package. Recent monthly updates stage most of the new code on up-to-date PCs, and the enablement package switches it on. Exactly what’s staged depends on your PC’s update baseline. The process doesn’t reinstall Windows (Microsoft 26H2 update history).
That explanation applies only to PCs already on 24H2 or 25H2. Windows 10 and older Windows 11 releases take a different, larger upgrade path.
Three separate things decide whether you see 26H2:
- Broad availability. Microsoft first offered 26H2 in waves. As of October 28, 2026, Microsoft’s release-health page says it’s available for all eligible devices. Microsoft’s update-history page still describes delivery as a controlled feature rollout, though, so broad availability doesn’t mean every PC gets the offer the same day.
- Eligibility. The PC must meet version, hardware, servicing, and policy requirements. Edition, processor architecture, activation, and organization policies all play a part.
- The actual offer. Windows Update can still withhold 26H2 because of a safeguard hold, an administrator policy, a missing prerequisite, or a local update problem.
A safeguard hold is a block Microsoft places on PCs with a known compatibility problem in a specific device, driver, or app. It’s a protective measure. Windows offers the update automatically once the problem is resolved.
Fixes for 26H2 Not Showing Up
Do these few things before you start. They prevent the most common install failures and take only a few minutes.
- Back up your important files to an external hard drive or cloud storage.
- Unplug nonessential external devices like printers, card readers, and spare drives.
- Install every quality and driver update Windows Update already offers.
- Confirm your date, time, and time zone are correct in Settings > Time & language > Date & time.
- Make sure your system drive has plenty of free space (Fix #6 covers this).
Fix #1: Confirm Your PC Is on 24H2 or 25H2
Symptoms:
- Windows Update finds monthly updates but never offers 26H2
- You aren’t sure which Windows version you’re running
Why it happens: 26H2 reaches Windows 11 PCs already on 24H2 or 25H2 through Windows Update as an enablement package. Other versions don’t get that offer.
If you’re still on Windows 10, Windows Update won’t offer 26H2 as an enablement package. Moving from Windows 10 is a full upgrade to Windows 11. In addition to the version and license requirements, the PC must meet the Windows 11 hardware requirements (TPM 2.0, a supported processor, and so on).
If your PC is on an earlier Windows 11 release, such as 23H2, it needs a feature update to an eligible version first. Check Windows 11 release information for which versions are still serviced. Ordinary monthly updates won’t move you to a new version on their own.
Steps:
- Press Win + I to open Settings.
- Go to System > About.
- Scroll to Windows specifications and read the Version line.
- While you’re on this page, note the System type line under Device specifications. It tells you whether you have an x64 or Arm-based processor, which matters for Fix #7.
Verify: If Version reads 24H2 or 25H2, your PC is on the normal path for 26H2. Move on to Fix #2. If it shows something older, upgrade to an eligible version first.
The version number alone isn’t a complete eligibility test. Hardware compatibility, servicing state, and organization policies can still block the offer.
Fix #2: Check for Updates and Turn On Early Availability
Symptoms:
- Your PC is eligible, but 26H2 isn’t listed in Windows Update
- Another PC in your home or office already has it
Why it happens: Windows Update may not have rechecked since 26H2 became broadly available. The delivery is still controlled, so a fresh check sometimes surfaces it.
Microsoft recommends turning on Get the latest updates as soon as they’re available if you want updates as soon as they’re ready (Microsoft 26H2 update history). The toggle raises your PC’s priority for feature and non-security updates. It doesn’t override a safeguard hold, an IT policy, a hardware requirement, or a servicing prerequisite.
Steps:
- Open Settings > Windows Update.
- Turn on Get the latest updates as soon as they’re available. The switch should read On.
- Click Check for updates. On some builds the button reads Check for Windows updates.
- If Windows 11, version 26H2 appears, click Download & install. Depending on your build, the button might read Download and install.
- Restart your PC when prompted. Keep a laptop plugged in until it’s done.
Verify: Press Win + R, type winver, and press Enter. The About Windows dialog should report version 26H2.
Tip: If 26H2 still isn’t offered after this, check for a safeguard hold next (Fix #3). A general rollout delay is less likely now that availability covers all eligible devices.
If the toggle is missing or greyed out
A greyed-out early availability toggle usually isn’t a bug. It typically means an organization controls the PC’s update settings.
- Look for “Some settings are managed by your organization” at the top of the Windows Update page.
- Open Settings > Accounts > Access work or school and check whether a work or school account is connected.
If either check confirms a managed PC, your administrator’s update policy decides when 26H2 arrives. Contact IT rather than trying to work around it.
If the PC isn’t managed and the toggle still doesn’t appear, your edition or build may lay out this page differently. Skip the toggle and just run Check for updates. The toggle only affects priority, so the rest of this article still applies.
Fix #3: Check Whether a Safeguard Hold Applies to Your PC
Symptoms:
- One specific PC never gets the offer, while others do
- Everything else on that PC updates normally
Why it happens: Microsoft has flagged a known compatibility issue that could affect that device, a driver, or an app on it. The hold lifts automatically once Microsoft resolves the issue.
Microsoft publicly documents broadly applied holds. Holds caused by third-party drivers, software, or devices are disclosed only in some cases (Microsoft KB5006965). The known issues page can point you toward a likely hold. Finding no match there doesn’t prove your PC is hold-free.
Steps:
- In Settings > Windows Update, look for a message saying the update is on its way or not ready for your device.
- If the message has a Learn more link, select it to see a summary of any safeguard holds affecting your device.
- Open the Windows 11, version 26H2 known issues and notifications page and compare the listed issues to your hardware, drivers, and installed apps.
- Check the 26H2 resolved issues page to see whether a matching issue has already been fixed.
- Install any driver, firmware, or app updates that the known issue mentions.
- Keep installing monthly updates in Settings > Windows Update so your PC is ready when the hold lifts.
Verify: Revisit the known issues page every week or two. Once a matching issue moves to resolved, run Check for updates again.
Don’t try to force your way past a safeguard hold. Registry edits and policy tweaks that hide the hold don’t fix the compatibility problem behind it. The hold exists to stop an install that Microsoft already knows can cause problems on your configuration.
Fix #4: Run the Windows Update Troubleshooter
Symptoms:
- Windows Update says “You’re up to date” for days, even with early availability turned on
- No safeguard hold seems to match your PC
Why it happens: Once you’ve ruled out eligibility and safeguard holds, a glitch in the local Windows Update components may be the cause. The Windows Update Troubleshooter can repair some of these problems. It can’t override an eligibility restriction or a safeguard hold.
Steps:
- Open Settings > System > Troubleshoot > Other troubleshooters. If you can’t find that path, type “troubleshoot” into the Settings search box instead.
- Find Windows Update in the list.
- Click Run.
- Follow the prompts and apply any fixes it suggests. On some Windows 11 builds, Microsoft’s Get Help app opens with an automated troubleshooting flow instead of a classic troubleshooter window. Follow its prompts the same way.
- Restart your PC.
Troubleshooter placement can shift between Windows 11 builds. If the menu path above doesn’t match, searching in Settings is the more reliable route.
Verify: Go back to Settings > Windows Update and click Check for updates.
If the troubleshooter finds nothing
Try these checks next, in order:
- Restart the PC once more. Pending updates sometimes block detection until a full restart finishes.
- Press Win + R , typeservices.msc , and pressEnter .
- Double-click Windows Update. The Startup type should be anything except Disabled. If it says Disabled, change it to Manual and click OK.
- Temporarily disconnect any VPN or proxy software, then check for updates again.
- Confirm the C: drive has free space and isn’t showing errors in File Explorer.
Be careful with manual “reset Windows Update” guides that delete update folders. Deleting the wrong folder can create new servicing problems. Avoid third-party Windows Update repair utilities as well.
Fix #5: Update Your Device Drivers
Symptoms:
- The update isn’t detected on an older PC or one with custom hardware
- The PC hasn’t had driver updates in a long time
Why it happens: Drivers matter most for compatibility holds and for a clean install. An outdated driver that matches a known issue can keep a hold in place. Microsoft doesn’t publish driver updates as general 26H2 detection guidance, so this step helps by clearing a possible driver-related hold.
Steps:
- Open Settings > Windows Update > Advanced options > Optional updates.
- Expand Driver updates, select any listed drivers, and click Download & install.
- Open Device Manager and look for yellow warning icons. “Update driver” there won’t always find the newest manufacturer driver, so treat it as a check rather than the final word.
- If a known issue in Fix #3 names a driver, or your manufacturer has issued 26H2 guidance, download that driver from your PC or motherboard manufacturer’s support site for your exact model. This matters most for graphics, storage, chipset, Wi-Fi, Bluetooth, and firmware.
- Install the drivers and restart.
Avoid generic third-party driver-update utilities. Skip beta drivers too. A driver can itself be the reason for a safeguard hold, and a beta build won’t help with that.
Verify: Click Check for updates again in Settings > Windows Update.
Fix #6: Free Up Disk Space
Symptoms:
- Your system drive is nearly full
- Updates stall or don’t appear at all
Why it happens: Low disk space more often blocks downloading, staging, or installing an update than it hides the offer itself. Feature updates also need temporary working space and room for rollback files. The manual Installation Assistant in Fix #7 needs at least 9 GB free, and that’s a minimum for that tool, not a comfortable target.
Steps:
- Open Settings > System > Storage.
- Click Temporary files and remove what you don’t need. Check the Downloads box only if you’re sure that folder has nothing you want.
- Empty the Recycle Bin after confirming nothing in it is needed.
- Move large photo, video, or game folders to an external hard drive instead of deleting them.
- Uninstall apps you no longer use.
Leave the Windows.old folder alone if it’s listed. Deleting it removes your ability to roll back a recent feature update.
Verify: Confirm the free space on your C: drive, then check for updates again. If your drive is constantly full, a larger SSD is the permanent fix.
Fix #7: Install 26H2 With the Windows 11 Installation Assistant
Symptoms:
- You want 26H2 installed now rather than waiting on Windows Update
- Windows Update still doesn’t offer it after Fixes #1–6
Why it happens: The current 26H2 Installation Assistant installs Windows 11 from Microsoft’s download page instead of through Windows Update. Microsoft’s Ways to install Windows 11 page recommends waiting for Windows Update to offer the upgrade first. The Assistant skips the normal offer check without fixing whatever blocked it.
Caution: Don’t use this tool to get around a safeguard hold identified in Fix #3. If a hold applies to your PC, wait for Microsoft to resolve it.
Requirements (Microsoft download page):
- A valid Windows 10 or Windows 11 license
- An administrator account on the PC
- Windows 10, version 2004 or later, if you’re upgrading from Windows 10 (the PC must also meet the Windows 11 hardware requirements)
- At least 9 GB of free disk space
- An x64 processor (the tool doesn’t run on Arm-based PCs)
Steps:
- Back up your important files.
- Go to Microsoft’s Download Windows 11 page. The tool is also listed on the Download Center as Windows 11 Installation Assistant 26H2.
- Under Windows 11 Installation Assistant, click Download Now.
- Right-click Windows11InstallationAssistant.exe and select Run as administrator.
- Let the tool finish its hardware compatibility check. If it rejects the PC, stop here and check the requirements above.
- When the license terms appear, click Accept and install.
- When prompted, click Restart now.
- Expect several restarts. Don’t turn off the PC while it installs. Keep a laptop plugged in, and a desktop on a UPS battery backup is safer if your power flickers.
Verify: Sign in, run winver, and confirm it reports 26H2.
If Fix #3 showed a safeguard hold that matches your PC, wait instead. Manual tools don’t make a known compatibility issue go away.
Fix #8: Use the Microsoft Update Catalog to Patch an Already-Installed 26H2 PC (Advanced)
Symptoms:
- You manage several PCs already running version 26H2 and want to deploy the latest cumulative update manually, outside of Windows Update
- You need a scripted or offline way to apply a 26H2 servicing update across multiple machines
Why it happens: The Microsoft Update Catalog hosts specific packages identified by a KB number, such as cumulative updates and out-of-band fixes. Microsoft’s KB5121794 documentation lists the Catalog as one way to obtain the 26H2 enablement package under its specific KB and prerequisites, but that path requires manually satisfying all prerequisites and is intended for IT administrators rather than ordinary home users. Windows Update is the recommended route for most people.
Searching the Catalog for a generic “26H2 feature update” term is insufficient; you need the exact KB number from Microsoft’s documentation and must meet all stated prerequisites first. If you haven’t yet upgraded to 26H2 and are an ordinary home user, use Fix #2 (Windows Update) or Fix #7 (Installation Assistant) instead. Come back to the Catalog once your PC is on 26H2 and needs a later cumulative update.
Steps:
- Confirm the PC is already on version 26H2 in Settings > System > About.
- Open Microsoft’s Windows 11, version 26H2 update history page and note the KB number of the latest applicable cumulative update. (To see what’s already installed on your PC, see how to check your update history.)
- Search the Microsoft Update Catalog for that exact KB number.
- Pick the package matching your PC’s processor architecture, shown under System type in About.
- Run the downloaded package and restart when asked.
Verify: Run winver and confirm the build number matches the cumulative update you installed.
Catalog pages and labels can change. A mismatch between the package and your PC’s architecture or installed version can make the install fail. If anything doesn’t match Microsoft’s update history page, stop and don’t force the install.
Error Messages and Symptoms Quick Reference
| Message or symptom | What it means | What to do | 
|---|---|---|
| “You’re up to date” but no 26H2 | Offer delayed on your PC, a safeguard hold, or a policy | Fix #2, then Fix #3 | 
| Version shows something other than 24H2 or 25H2 | PC isn’t eligible for the enablement package yet | Fix #1 | 
| One PC never gets the offer while others do | Likely a safeguard hold | Fix #3 | 
| “The update is on its way” | Controlled delivery or a safeguard hold | Select Learn more and wait for the fix | 
| Early availability toggle missing or greyed out | Organization policy, managed device, or build layout | Check work or school management (Fix #2) | 
| Installation Assistant won’t start or rejects the PC | Arm PC, too little space, no valid license, Windows 10 older than 2004, or hardware that doesn’t meet Windows 11 requirements | Check requirements in Fix #7 | 
| Download fails repeatedly | VPN or proxy, low space, or damaged update components | Fix #4 and Fix #6 | 
| Update installs but version doesn’t change | Pending restart or a failed enablement step | Restart, then check Update history | 
| Update never detected on an older PC | Outdated drivers or a Windows Update component glitch | Fix #4 and Fix #5 | 
| Black screen after sign-in following the update | Resolved known issue on FSLogix virtual desktops, or a display driver problem | See Platform-Specific Issues below | 
| Domain trust failure after the update | Credential Guard and Machine Identity Isolation configuration | Review the 26H2 known issues page with IT | 
Platform-Specific Issues
On Arm-Based Windows 11 PCs
The Installation Assistant only supports x64 processors, so an Arm-based PC can’t use Fix #7. Running the x64 tool under emulation isn’t a supported install method either.
On an Arm PC, do this instead:
- Use Settings > Windows Update (Fixes #2–6) to receive the 26H2 enablement package.
- Install every offered cumulative, firmware, and driver update.
- Check your PC manufacturer’s support page for Arm-compatible recovery or installation options.
The Microsoft Update Catalog (Fix #8) doesn’t work around this limitation. It only helps once your Arm PC is already on 26H2 and needs a later cumulative update.
On Domain-Joined Work PCs
Microsoft’s 26H2 resolved issues page documents a domain problem affecting some Credential Guard-protected machine accounts. It applies when Machine Identity Isolation settings are used outside supported Windows Server 2025 domain functional level environments. Microsoft resolved it with the September 22, 2026 update (KB5124010) and later updates.
If your work PC loses its domain trust after updating, install the latest cumulative update and send that page to your IT team. Don’t upgrade domain-managed PCs yourself without checking release-health notices first.
Virtual machines bring their own variables too. Virtual TPM, Secure Boot, virtual CPU settings, and guest integration drivers can all affect eligibility.
Black Screen After Sign-In Following the Update
Microsoft documented a problem where the desktop fails to load after sign-in. It was seen primarily on Azure Virtual Desktop hosts using FSLogix, not typical home PCs. Microsoft lists it as resolved by the October 13, 2026 update, KB5127648, and later updates (26H2 update history, 26H2 resolved issues).
If you see it on a PC that hasn’t installed that update yet, install the latest cumulative update. Until then, Microsoft’s documented workaround is to open Task Manager (Ctrl + Shift + Esc), select Run new task, enter explorer.exe, and select OK.
On a regular home PC, a black screen after a feature update usually points to the display setup. Try these in order:
- Press Win + Ctrl + Shift + B to reset the graphics driver. You’ll hear a beep and the screen may flicker.
- Press Ctrl + Shift + Esc . If Task Manager opens, select Run new task, typeexplorer.exe , and select OK. Your taskbar and desktop should reappear.
- Unplug docking stations, USB display adapters, and secondary monitors.
- Press Win + P and pick the display mode that matches your setup.
- Boot into Safe Mode and roll back or reinstall the display driver.
If a BitLocker recovery screen appears instead, you’ll need your recovery key. Don’t disable encryption to get around it.
How to Roll Back Windows 11 26H2 If Something Breaks
If a problem started right after 26H2 installed and the fixes above don’t help, you may be able to go back to your previous version.
- Back up your important files first.
- Open Settings > System > Recovery.
- Look for Go back under Recovery options. If it’s available, click it.
- Answer the short survey about why you’re going back, then follow the prompts.
- Let the PC restart. Keep it plugged in until it finishes.
Verify: Run winver and confirm it reports your previous version, 24H2 or 25H2.
If Go back is missing or greyed out, the rollback files may already be gone. This happens when Windows.old has been deleted. In that case, check the known issues page for a fix and contact your PC manufacturer or Microsoft Support. On a managed PC, your administrator controls rollbacks and feature-update deferrals.
Configuration Mistakes to Avoid
- Not checking the early availability setting. Microsoft recommends turning on Get the latest updates as soon as they’re available if you want updates as soon as they’re ready. It won’t bypass a safeguard hold, but it’s worth checking in Settings > Windows Update.
- Assuming a greyed-out toggle is a bug. On a work or school PC, it’s usually an IT policy.
- Checking the wrong field. Edition tells you Home or Pro. Version tells you 24H2 or 25H2. Eligibility depends on Version.
- Running the Installation Assistant without admin rights. Right-click and choose Run as administrator.
- Running the Installation Assistant on an Arm PC. It’s x64-only.
- Forcing past a safeguard hold. Registry edits and undocumented policy changes don’t fix the problem behind the hold.
- Disabling Secure Boot, TPM, or security features to make the update appear.
- Installing random packages from the Update Catalog. Install only a specific KB that Microsoft’s documentation tells you to.
- Deleting update folders or services without a documented recovery plan.
Getting Help
If your PC still won’t get 26H2, collect these details before asking for help:
- Your Version, OS build, Edition, and System type from Settings > System > About
- The output of winver
- Any message Windows Update displays, copied word for word
- The KB number and error code from Settings > Windows Update > Update history, if an install failed
- Whether the PC is managed by work or school
Then work through these in order:
- Check the official release-health pages:
– Windows 11, version 26H2 known issues and notifications – Windows 11, version 26H2 resolved issues – Windows 11 release information
- Contact your PC manufacturer for firmware, storage, graphics, and OEM driver problems.
- Contact your organization’s IT administrator for managed PCs.
- Use Microsoft Support or Microsoft Q&A for unresolved home PC cases.
If Windows Update shows a numbered error code rather than simply not offering 26H2, that’s a different problem. Troubleshoot that specific error code instead.
Prevention Tips
- Install monthly updates promptly. The enablement package relies on features staged by recent 24H2 and 25H2 monthly updates.
- Keep drivers and firmware current. Check Optional updates and your manufacturer’s site a few times a year.
- Leave headroom on your system drive. Keep well over 9 GB free so the Installation Assistant stays an option.
- Use a stable connection for big updates. A Cat6 Ethernet cable is more reliable than flaky Wi-Fi when you’re downloading updates.
- Back up before any feature update. An in-place upgrade is designed to keep your files and apps, but no upgrade is risk-free.
- Save your BitLocker recovery key somewhere other than the PC itself.
- Skip debloat and privacy scripts that alter Windows servicing components. They can break future updates.
Frequently Asked Questions
Is it normal that I don’t see 26H2 yet?
It can be. Microsoft completed the broader 26H2 rollout for eligible devices.
A specific PC can still go without an offer because of a safeguard hold, a management policy, a missing prerequisite, an architecture limitation, or a local update problem. A missing offer doesn’t mean your PC is broken.
How do I know if my PC is eligible for 26H2?
Open Settings > System > About and check Version. If it says 24H2 or 25H2, you’re on the right path. Eligibility also depends on Windows 11 hardware compatibility, servicing state, free storage, and any administrator restrictions.
What is a safeguard hold, and how do I know if I’m on one?
It’s a temporary block Microsoft places on PCs with a known compatibility issue. Check Settings > Windows Update for a message with a Learn more link.
Then compare your hardware and apps against the 26H2 known issues page. Not every hold is publicly listed, so the page can suggest a hold but can’t confirm one for your specific PC.
What’s the fastest safe way to get 26H2 now?
Turn on Get the latest updates as soon as they’re available and click Check for updates. If 26H2 appears, select Download & install. If it doesn’t appear and nothing suggests a safeguard hold, the Installation Assistant is the fallback on x64 PCs, though Microsoft recommends waiting for the Windows Update offer first.
Will installing 26H2 erase my files or apps?
A normal in-place upgrade is designed to keep personal files and installed apps. On 24H2 and 25H2, the enablement package switches on features already staged by monthly updates. Back up anyway before any feature update.
How do I confirm 26H2 actually installed?
Press Win + R, type winver, and press Enter. The About Windows dialog shows your installed version. You can also check Settings > Windows Update > Update history for the feature update entry.
Wrapping Up
Fix #2 solves this for most eligible PCs. With 26H2 available to all eligible devices, a fresh Check for updates with early availability turned on is often all it takes. If one PC never gets the offer, check for a safeguard hold and IT management before reaching for the Installation Assistant.
If Windows Update fails on every update, including ones other than 26H2, stop troubleshooting the rollout. Run a disk health check, scan for malware, and contact your PC manufacturer, since a failing drive or deeper servicing damage is the more likely cause.
| Step | Action | Applies To | 
|---|---|---|
| 1 | Check Version in Settings > System > About | All PCs | 
| 2 | Turn on early availability and check for updates | 24H2 / 25H2 | 
| 3 | Check the known issues page for safeguard holds | PCs that never get the offer | 
| 4–6 | Run the troubleshooter, update drivers, free disk space | PCs where the update isn’t detected | 
| 7 | Run the Windows 11 Installation Assistant | x64 PCs with a valid license, 9 GB free, and no hold | 
| 8 | Download a specific cumulative update from the Microsoft Update Catalog | Admins patching PCs already on 26H2 | 
Last updated: November 8, 2026 | Applies to Windows 11, version 24H2 and 25H2 devices upgrading to version 26H2
