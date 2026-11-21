---
id: collect-261121-veille-ia/langages-dev/linux-7-4-rc3-released-with-large-iomap-calling-convention-change-and-smb-client-fixes
title: "Linux 7.4-rc3 Released With Large iomap Calling Convention Change And SMB Client Fixes"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["linux", "fixes", "kernel", "release", "large", "also", "torvalds", "released"]
source: https://commandlinux.com/news/linux-7-4-rc3-released-with-large-iomap-calling-convention-change-and-smb-client-fixes/
source_anchor: ""
source_lines: [1, 19]
section: "Langages_Dev"
---
Linus Torvalds released *Linux 7.4-rc3* on ***November 15, 2026***, the third weekly test build of the 7.4 kernel. In his announcement to the Linux kernel mailing list, he described it as another big release candidate that otherwise looks fairly normal, with networking and GPU drivers making up the largest share of changes. 

The number of changes is high, he wrote, but most individual patches are tiny. The kernel.org release listing now shows 7.4-rc3 as the mainline version and 7.3.4 as the latest stable kernel.

Filesystem code shows up more than usual in this week’s diffstat. Torvalds put that down to a couple of larger patches, chiefly a change to the calling convention for iomap, the shared layer filesystems use to map file ranges to storage, which he described as trivial in substance but large in size. A few SMB client fixes landed there as well. The rest, he wrote, is selftests, documentation updates and “the usual noise all over.”

Pull requests merged on November 14, ahead of the tag, include hardware monitoring fixes from Guenter Roeck’s hwmon tree, watchdog fixes and a second batch of keys fixes from Jarkko Sakkinen’s TPM tree, according to the commit history of Torvalds’ tree.

Neowin’s report on the release also lists a mitigation for Transient Scheduler Attacks (TSA) on AMD Zen 5 processors. The underlying patch, x86/bugs: Mitigate TSA on AMD Zen5, was written by Borislav Petkov of AMD and also shipped in the stable Linux 7.3.4 release on ***November 10***. Its commit message states that Zen 5 is affected by TSA, along with a third variant named TSA_FILL. Updated microcode sets the VERW_CLEAR CPUID bit, so the kernel can detect the mitigation on Zen 5 without keeping a table of microcode revisions.

AMD published security bulletin AMD-SB-7073 the same day. It covers CVE-2024-36350 (TSA-SQ), which could let an attacker infer data from earlier stores, and CVE-2024-36357 (TSA-L1), which could expose data held in the L1 data cache across privilege boundaries. AMD gives both a CVSS 3.1 base score of 5.6 and says Zen 5 parts are affected differently from the processors covered in its earlier notice.

Listed products include EPYC 9005, Ryzen 9000, Ryzen AI 300 and Threadripper PRO 9000 WX-Series chips. AMD says full mitigation needs both platform firmware from system vendors and operating system updates.

The 7.4 cycle opened with an unusually large merge window. In the rc1 announcement on November 1, Torvalds called it a big merge window and said he was happier to see AI-triggered work arrive then than as extra fixes during the release-candidate weeks.

He also wrote that people expect 7.4 to be the next long-term support kernel, which he offered as another reason for its size. Kernel.org has not yet listed 7.4 as a longterm branch. The volume of AI-generated patches has also weighed on individual maintainers, and a Linux memory-management reviewer recently stepped back from several roles for that reason.

Release candidates will keep arriving weekly. If 7.4 follows the usual seven-candidate schedule, rc7 would come on December 13 and the final release on ***December 20***, the target Linux Compatible’s report on rc3 also gives.
