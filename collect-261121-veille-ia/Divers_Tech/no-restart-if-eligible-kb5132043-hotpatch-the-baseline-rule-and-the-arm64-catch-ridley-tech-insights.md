---
id: collect-261121-veille-ia/divers-tech/no-restart-if-eligible-kb5132043-hotpatch-the-baseline-rule-and-the-arm64-catch-ridley-tech-insights
title: "No Restart, If Eligible: KB5132043 Hotpatch, the Baseline Rule, and the Arm64 Catch | Ridley Tech Insights"
domain: divers-tech
role: reference
task: article
actors: []
dates: []
keywords: ["hotpatch", "restart", "kb5132043", "baseline", "arm64", "windows", "doesn", "eligible"]
source: http://ridley-tech-insights.bridley5189.chatgpt.site/articles/kb5132043-hotpatch-baseline-arm64/
source_anchor: ""
source_lines: [1, 27]
section: "Divers_Tech"
---
November's hotpatch, KB5132043 (released November 10), takes Windows 11 26H2, 25H2, and 24H2 to builds 26300.9911, 26200.9911, and 26100.9911 â and on eligible managed devices it installs with no restart. Microsoft lists no known issues for the release. The catch is in that word eligible: a matching Windows version alone doesn't put a device on the hotpatch track.

## The baseline rule

Hotpatching trades restarts on a schedule, not forever. Quarterly baseline updates carry the cumulative foundation and do require a restart; the two monthly hotpatches that follow don't. For KB5132043, Microsoft requires baseline KB5127648 to be installed first, a servicing stack of 26100.9703 or later (the release bundles SSU KB5132688, version 26100.9971), VBS running, an eligible license (Windows 11 Enterprise E3/E5, Microsoft 365 F3, Education A3/A5, Business Premium, or Windows 365 Enterprise), and a hotpatch-enabled quality update policy in Intune with "apply without restarting the device" set to Allow.

Two checks worth scripting into your preflight:

- Servicing stack: DISM /Online /Get-Packages | findstr /i "ServicingStack" â the Package Identity must read 26100.9703 or later. (This does not confirm the baseline; check KB5127648 separately.)
- VBS: System Information â System Summary â "Virtualization-based security" must say Running.

And set the expectation with your team now: a device that misses a prerequisite doesn't fail â it quietly takes the standard cumulative update instead, restart and all. A restart prompt is not evidence the hotpatch broke.

## The Arm64 catch

Arm64 has homework the x64 fleet doesn't. Microsoft's Arm64 instructions name 25H2 and 24H2 (don't extend them to 26H2 â the KB doesn't), and require Compiled Hybrid PE (CHPE) disabled via the HotPatchRestrictions DWORD set to 1 under HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management, followed by a one-time restart. That prep restart is real, and it comes with a genuine trade-off: CHPE is what makes 32-bit x86 apps run efficiently on Arm64, so disabling it can bite legacy 32-bit Office, VBA, and COM add-in scenarios. Test against the actual app estate, or exclude those devices from the hotpatch policy.

## Delivery and proof

Two operational notes. First, this hotpatch comes through Windows Update â the release doesn't offer a standalone Microsoft Update Catalog package, so don't build a deployment plan around one. Second, verify results, not assignments: the device should land on its branch's .9911 build, and the Hotpatch quality updates report (Reports â Windows Autopatch â Windows quality updates) shows deployment status and errors.

One last separation to keep clean: the Secure Boot certificate notice riding along in the release notes is a different maintenance item. A green Secure Boot badge doesn't prove the certificate transition is complete, and installing KB5132043 doesn't finish it. Track the two separately.

## Sources

- WindowsForum's breakdown of KB5132043 (eligibility, baseline, Arm64 requirements): https://windowsforum.com/news/windows-11-kb5132043-hotpatch-no-restart-security-update-eligibility-and-arm64-requirements.451795/
- Microsoft's KB5132043 release notes and Intune hotpatch guidance, as cited therein
