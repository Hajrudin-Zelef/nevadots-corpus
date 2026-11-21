---
id: collect-261121-veille-ia/hardware-semi/linux-7-2-is-out-with-cache-aware-scheduling-early-amd-hdmi-2-1-frl-and-the-end-of-strncpy
title: "Linux 7.2 Is Out With Cache-Aware Scheduling, Early AMD HDMI 2.1 FRL and the End of strncpy"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["linux", "support", "cache-aware", "scheduling", "hdmi", "strncpy", "torvalds", "release"]
source: https://www.webpronews.com/linux-kernel-7-2-arrives-cache-aware-scheduling-and-fresh-hardware-support-reshape-server-and-desktop-performance/
source_anchor: ""
source_lines: [1, 21]
section: "Hardware_Semi"
---
Linus Torvalds released Linux 7.2 on Sunday, on schedule despite another heavy final week. “Well, this last week of the release was – once again – bigger than I would have wished for, but hey, with the whole “new normal” thing, if I delayed releases for that reason we’d probably never have a release at all,” he wrote in the announcement, as quoted by Phoronix. The “new normal” is a cycle swollen by a surge of AI-assisted patches and bug reports.

The last-minute changes included a revert of the DRM subsystem’s new “fair” scheduler back to FIFO after a regression, a batch of sound-device quirks and a new tlbi=ipi boot option for x86. Torvalds defended the late reverts: “It may not be pretty, but it’s the correct way to deal with ‘oh, that code wasn’t ready and caused problems’.”

## The features worth knowing

The headline addition is cache-aware scheduling, which helps systems with multiple last-level cache domains keep related work close to its data. Phoronix’s feature roundup calls it “easily one of the most exciting features” of the release for modern multi-LLC hardware.

Hardware support moves on several fronts:

– **AMD:** Mainline support for Zen 6, now shipping in EPYC 9006 “Venice,” looks to be in good shape for standard functionality. AMDGPU gets initial HDMI 2.1 FRL support, disabled by default and without VRR or ALLM yet, and the ISP4 driver arrives for webcams on the HP ZBook Ultra G1a and upcoming Ryzen laptops.
– **Intel:** Initial support for the rugged Panther Lake R, more Diamond Rapids and Nova Lake H preparation, Arc B390 graphics performance gains, WiFi 8 groundwork in iwlwifi, and USB4STREAM, an Intel-developed protocol for moving data packets quickly between USB4 or Thunderbolt-connected machines.
– **Apple:** Basic M3 support, enough to boot to a console. M3 Pro, Max and Ultra support isn’t expected until Linux 7.3.

Performance tweaks include I/O gains on both AMD and Intel systems, faster poll, quicker reads of /proc/filesystems and an MGLRU change that helps MongoDB.

## Cleanup, finally

Linux 7.2 also finishes a long job: the kernel’s strncpy() API is gone. The patch notes call it “a persistent source of bugs” because it may not NUL-terminate its destination and always zero-pads. As XDA Developers noted, removing it took more than six years and hundreds of commits, 211 of them from Justin Stitt. Old drivers for Hercules monochrome ISA cards, Profibus and ISA and PCMCIA ARCnet were dropped too, and AF_ALG is now deprecated, so out-of-tree module maintainers should test before upgrading.

Linux 7.2 is the kernel planned for Ubuntu 26.10. The Linux 7.3 merge window opens Monday, and Torvalds said he already had 40 pull requests waiting.
