---
id: collect-261121-veille-ia/linux-kernel/linux-kernel-7-4-rc2-released-largest-merge-window-in-history-meets-stabilization-test
title: "Linux Kernel 7.4-rc2 Released: Largest Merge Window in History Meets Stabilization Test"
domain: linux-kernel
role: reference
task: article
actors: []
dates: []
keywords: ["kernel", "linux", "torvalds", "merge", "window", "fixes", "largest", "release"]
source: https://www.linuxcompatible.org/story/linux-kernel-74rc2-released-largest-merge-window-in-history-meets-stabilization-test/
source_anchor: ""
source_lines: [1, 73]
section: "Linux_Kernel"
---
Linux Kernel 7.4-rc2 shipped on November 9, 2026, as the second testing candidate following what Torvalds calls the largest merge window in kernel history at nearly 20,000 commits. Most of that record volume came from AMD GPU register dumps and deferred automated coccinelle conversions, so the actual changes are more ordinary than the diffstat suggests. A significant portion of the work is AI-generated, touching core memory management, BPF, and filesystem fixes, with maintainers reporting that AI-discovered fixes are now starting to outpace new bug reports in some areas. The final kernel is expected around late December 2026 and may become the next LTS release.

### Linux Kernel 7.4-rc2 Lands After a Record-Breaking Merge Window

**Torvalds says the second release candidate looks ordinary once you ignore the AI-generated noise.**

The second release candidate for Linux Kernel 7.4 is out. Linus Torvalds pushed it to the Linux Kernel Mailing List, eight days after rc1 and right on the heels of what he's calling the largest merge window in kernel history. Roughly a fifth of what shipped this cycle comes from AI tools, and Torvalds didn't pretend otherwise.

Here's the thing though: the diffstat looks way bigger than the release actually is.

A release candidate isn't a product launch. It's a weekly test snapshot from which only bug fixes should land, not new features, so testers can shake out regressions before the final kernel ships. rc2 is the second of usually seven or eight of these checkpoints. By the time rc7 or rc8 rolls around, 7.4 is done.

Torvalds' announcement was candid, maybe a little tired. He said he'd picked up a pile of automated coccinelle conversions during the merge window and deliberately deferred them into rc2 because he didn't want to deal with them while traveling, and then didn't want to deal with them afterward either, to be honest.

"Ok, a big -rc2 to go with the largest ever -rc1," he wrote. "But the diffstat looks fairly benign, and is dominated by some automated coccinelle updates."

The word "benign" does a lot of work here.

Those coccinelle updates are scripted, mechanical code conversions. They're "largely just one- and two-liners," Torvalds said, spread across thousands of files. They inflate the numbers without adding much substance. Mask them out and the release breaks into three equal halves: drivers (GPU, networking, sound), networking and BPF, and "the rest" (filesystems, memory management, architecture updates).

If you want the actual files, the source sits at git.kernel.org. Torvalds posted the full tarball, a patch against 7.3, and a change summary. kernel.org now lists 7.4-rc2 as the current mainline build.

#### What actually landed

Peeling back the noise, rc2 carries a real batch of fixes, and several of them matter.

The one with the biggest everyday punch is a FAT32 speedup. Developer Matheus Alves de Almeida preallocates space before files expand, moving the work into a new fat_preallocate() helper and using a FAT-specific path to avoid re-checking file size. Per Phoronix benchmarks, the wins show up on synchronous mounts with small cluster sizes, hitting improvements of up to roughly 162x. A separate FAT optimization merged earlier in the cycle reportedly offered up to 265x.

Then there's core memory management. A new Core MM tree now routes some patches through David Hildenbrand rather than solely through Andrew Morton. Keep in mind that's a structural change, not just another bug fix, and this batch landed what Hildenbrand called "nasty long-standing bugs." The scope was broad: transparent huge pages, GUP, KSM, USERFAULTFD, shmem and memfd, and hugetlb. The fixes cover information leaks, out-of-bounds writes, data loss, corruption, use-after-free bugs, deadlocks, and unkillable loops.

Hildenbrand was straight about where it came from. "Most issues were reported by Anthropic," he said. "Others were found while reworking proposed fixes, and of course some of the fixes were just reported and fixed on the mailing list."

That's AI companies feeding real bug reports straight into the kernel's security mailbox.

The BPF subsystem got hit just as hard. Alexei Starovoitov shipped 40 patches covering scalar-evolution tracking, stack handling, arena mapping safety, and network-buffer edge cases. Chris Mason added another 33, including memory-safety rejections for kfunc arguments and cgroup detach checks. It's the one area Torvalds flagged as potentially reaching the point where fixes arrive faster than new bugs turn up.

Other fixes scattered across the tree include Bluetooth concurrency protection, malformed IPsec mark rejection in xfrm, NVMe use-after-free and queue dereference fixes, KVM guest-MMU work from Sean Christopherson, and a 22-patch hwmon sweep by Guenter Roeck.

#### The AI question the project can't ignore

What makes 7.4-rc2 worth watching goes way beyond any single patch. It's the first real test of whether Torvalds' "healthier model" actually holds up under testing.

Torvalds' argument isn't about banning AI. It's about timing. He said he's "much happier with the flood of AI-triggered work going into the merge window than it showing up mainly as extra fixes during the -rc period." The theory is simple: changes arriving in the merge window get organized through maintainers, leaving the fragile rc weeks free to focus on genuine regressions instead of a trickle of urgent AI fixes.

There's a wrinkle worth flagging though. Torvalds has himself become an AI-assisted contributor, building patches with tools like Claude Code. Notable climb-down for the developer who once dismissed much of the AI industry as "90% marketing."

The whole system only works if reviewers keep up. AI suggestions "often get rewritten" by humans, so the kernel's real defense against automated noise isn't a ban on AI. It's the maintainer review culture plus the rule of named accountability.

Earlier in 2026, reports surfaced that one AI system had spotted 948 local privilege-escalation vulnerabilities in the kernel. That kind of volume strains any volunteer model, which is exactly why this testing phase matters.

#### So what happens next?

For most users, stay put. 7.4-rc2 is a testing build aimed at developers and enthusiasts, not production systems. The advice is consistent across the board: sit on your distribution's supported stable kernel until 7.4 finalizes and racks up a few stable updates.

If the usual cadence holds, 7.4 could land around December 20 with seven release candidates, or December 27 with eight. It's widely expected to be picked up by stable-kernel maintainer Greg Kroah-Hartman as the next LTS kernel in early 2027, which would give it years of backported fixes.

7.4-rc2 is the first real test of the "healthier model" idea. The merge window proved the project can absorb a record volume of AI-driven change. The rc weeks will prove whether that volume survives testing.

Torvalds closed his announcement with his usual bluntness. "Don't hold your breath, but maybe cross your fingers we'll be getting there in the not entirely distant future. In the meantime, please keep testing."

#### Linux kernel 7.4-rc2 released

Linux kernel version 7.4-rc2 is now available:


Full source:    https://git.kernel.org/torvalds/t/linux-7.4-rc2.tar.gz

Patch:          https://git.kernel.org/torvalds/p/v7.4-rc2/v7.3


You can view the summary of the changes at the following URL:

https://git.kernel.org/torvalds/ds/v7.4-rc2/v7.3
