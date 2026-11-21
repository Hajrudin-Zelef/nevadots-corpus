---
id: collect-261121-veille-ia/linux-kernel/linux-kernel-7-4-rc1-records-biggest-kernel-merge-window-ever-with-20-000-commits
title: "Linux Kernel 7.4-rc1 Records Biggest Kernel Merge Window Ever With ~20,000 Commits"
domain: linux-kernel
role: reference
task: article
actors: []
dates: []
keywords: ["kernel", "window", "merge", "patches", "linux", "torvalds", "gets", "commits"]
source: https://www.linuxcompatible.org/story/linux-kernel-74rc1-records-biggest-kernel-merge-window-ever-with-commits/
source_anchor: ""
source_lines: [1, 45]
section: "Linux_Kernel"
---
### Linux Kernel 7.4-rc1 Ships With the Largest Merge Window in History

**Linus Torvalds pushed out Linux 7.4-rc1 on Sunday, and the commit count makes the argument for him. Nearly 20,000 commits made it through the two-week window. That's the biggest merge window in the roughly 35-year history of the kernel. And he's not going to pretend that's all good news, because he's pointed straight at AI as a big driver.**

To get why that's even a story, you have to know how the kernel ships. A new major version opens a two-week merge window. Subsystem maintainers then ask Torvalds to pull their chosen patches. That's where most new features land. Once the window shuts, you get release candidates, usually seven or eight, one per Sunday, and those are for bug fixes and cleanup. A big merge window, then, means a lot shipped fast before anyone had a chance to shake out the problems.

For decades the cadence has been rock-steady. The volume, on the other hand, hasn't. Commit counts hovered in the 13,000 to 14,000 range for much of the past ten years. 7.3 broke out first, landing near 16,000. Now 7.4 has blown straight past everything.

Torvalds was pretty candid about the size in his LKML post Sunday afternoon. He had a few pull requests still sitting in his inbox, ones he hadn't settled on, and rather than sit there "vacillating" he just filed them for rc2. The message itself was all about scale.

"This is a BIG merge window," he wrote. "I'm actually taking it as a good sign: I'm much happier with the flood of AI triggered work going into the merge window than it showing up mainly as extra fixes during the -rc period."

His read is that AI fixes arriving as part of the normal build is healthier than a wave of patches landing during stabilization. "I think that's a much healthier model. But we'll see how the -rc's look."

He also offered a second, independent reason. Some of the surge is just people banking on 7.4 being the next LTS kernel. As the last major release of the calendar year, 7.4 is the natural LTS pick, and Greg Kroah-Hartman is set to take it over in early 2027. So it may not just be the AI patches.

That AI framing is where the argument gets loudest. Subsystem maintainers have said publicly that they feel "completely overwhelmed" by waves of AI-generated bug reports and fixes. In September alone, AI-generated code was estimated at roughly 17 percent of all kernel patches. And Torvalds admitted the AI-suggested patches "often get rewritten" by humans, a quiet way of saying much of the raw output still needs cleaning up.

The community is split down the middle. Some welcome transparent AI-assisted contributions. Veterans push back hard on low-quality submissions, arguing that automated "fix everything" PRs do more harm than good. There's even been talk of banning AI patches from the staging area. Torvalds is one of the most vocal critics of automated slop, though, so his willingness to call this surge "a good sign" carries weight. His optimism is entirely conditional on human review staying in the loop. Whether that holds is exactly what the coming release candidates will decide.

#### What's actually in it

Strip away the counts and there's a genuine feature set here.

Hardware support gets a big haul. Apple M4 gets initial support, including the A18 Pro-based "MacBook Neo," though it's not ready for everyday users yet. AMD Ryzen Zen 6 work begins ahead of next-gen client chips. AMDGPU gains HDMI 2.1 FRL, FreeSync, ALLM, and VRR, plus UALoE support and a PerfOpt feature aimed at AI and LLM performance on Radeon iGPUs. Intel Xe gets memory page offlining and VRAM temperature monitoring. Google Tensor G5 and the Pixel 10 get mainline treatment, and more Qualcomm Snapdragon X2 laptops round it out.

Performance is where the flashier numbers live. FAT32 reportedly gains speedups of up to ~265x in some cases. An early-boot memory allocator called memblock reportedly falls from ~268s down to ~50ms. Btrfs gets zstd decompression wins. The schedulers get updates covering load-balancing, NUMA-balancing, and energy-aware work.

Filesystems and core work round out the release. EXT4 gets buffered iomap support. SMB compression can now be toggled via chattr. Christian Brauner alone pushed about 21 items through the VFS layer. Security gets composable Landlock controls and a rework of ACPI device enumeration so power management doesn't fire early. Virtualization, Rust in the kernel, perf, and kselftest all get continued updates. Networking lands TCP ROCCET congestion control, a CUBIC variant for cellular networks, and hardware pacing offload.

Timing-wise, the final 7.4 is expected around December 20, 2026, or December 27 if eight release candidates ship. Greg Kroah-Hartman is slated to take 7.4 over as the LTS kernel in early 2027, with support stretching well past. You're still on 7.3.1 (released October 30) if you want stable. 7.2.15 has hit end of life.

#### Linux kernel 7.4-rc1 released

Linux kernel version 7.4-rc1 is now available:


Full source:    https://git.kernel.org/torvalds/t/linux-7.4-rc1.tar.gz

Patch:          https://git.kernel.org/torvalds/p/v7.4-rc1/v7.3


You can view the summary of the changes at the following URL:

https://git.kernel.org/torvalds/ds/v7.4-rc1/v7.3
