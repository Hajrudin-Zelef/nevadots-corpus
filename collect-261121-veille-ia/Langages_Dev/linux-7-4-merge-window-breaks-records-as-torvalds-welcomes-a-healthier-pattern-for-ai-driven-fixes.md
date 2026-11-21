---
id: collect-261121-veille-ia/langages-dev/linux-7-4-merge-window-breaks-records-as-torvalds-welcomes-a-healthier-pattern-for-ai-driven-fixes
title: "Linux 7.4 Merge Window Breaks Records as Torvalds Welcomes a “Healthier” Pattern for AI-Driven Fixes"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["linux", "torvalds", "merge", "window", "fixes", "support", "healthier", "pattern"]
source: https://pbxscience.com/linux-7-4-merge-window-breaks-records-as-torvalds-welcomes-a-healthier-pattern-for-ai-driven-fixes/
source_anchor: ""
source_lines: [1, 33]
section: "Langages_Dev"
---
## Linux 7.4 Merge Window Breaks Records as Torvalds Welcomes a “Healthier” Pattern for AI-Driven Fixes

The Linux 7.4 merge window has become the busiest in the kernel’s history, with a surge of AI- and LLM-assisted work pushing the commit count to almost 20,000. Linus Torvalds, the creator and lead maintainer of Linux, released Linux 7.4-rc1 on Sunday, November 1, and said he was pleased with where that work is landing in the development cycle.

### A record-setting merge window

According to Phoronix, the merge window brought in 19,671 commits since Linux 7.3. That compares with 16,267 for the Linux 7.3 merge window, 14,395 for 7.2 and 13,963 for 7.1. Torvalds himself put the figure at almost 20,000 commits, against the more typical figure of roughly 15,000. Phoronix also noted that the kernel source tree is closing in on 42 million lines of code.

### Why Torvalds is encouraged

In his rc1 announcement, Torvalds said the main driver of the size was the “by now obvious” rise in AI-triggered work. Rather than treating that as a problem, he called it a good sign. He said he much prefers AI-generated fixes flowing into the merge window over having them show up mostly as extra fixes during the release-candidate stabilization period. His hope is that such fixes are becoming part of normal development, which he described as “a much healthier model.” He added that it remains to be seen how the upcoming release candidates look.

Torvalds’ view: AI-triggered patches arriving early in the cycle are preferable to a flood of fixes during stabilization.

### Other factors behind the volume

Torvalds noted that AI is probably not the only reason for the size. Many developers expect Linux 7.4 to be this year’s long-term support (LTS) kernel, and releases with that expectation tend to be larger. The diff is also inflated by a familiar cause: a large AMD GPU register dump in header files, which accounts for about a third of the patch. Torvalds said such additions are routine and that the resulting noise can simply be ignored.

### What’s in Linux 7.4

Beyond the commit totals, Phoronix highlights a broad set of hardware and performance changes in the new kernel:

- A new AMD PerfOpt feature for Radeon integrated GPUs, aimed at improving AI/LLM workloads
- Preparatory support for AMD Ryzen Zen 6 processors, and HDMI 2.1 enabled by default in the AMDGPU driver
- Intel IPU8 support for upcoming Nova Lake laptops
- Further enablement for Qualcomm Snapdragon X2 laptops
- Basic Apple M4 support, which is not yet ready for end users
- Mainline support for the Google Tensor G5 SoC and Pixel 10 devices
- Optional buffered IOmap support for EXT4 and faster kernel build times

### What comes next

With rc1 out on schedule, development now moves into the stabilization phase, with new release candidates arriving roughly weekly. If the schedule holds, the final Linux 7.4 release is expected in about six to eight weeks, which points to mid-to-late December. Phoronix says that would likely make it this year’s LTS kernel, provided there are no significant delays late in the cycle. Torvalds’ own comment that the rc releases will show whether the new pattern holds is worth watching.
