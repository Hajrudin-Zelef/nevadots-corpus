---
id: collect-261121-veille-ia/langages-dev/linux-7-4-lands-with-a-massive-flood-of-ai-patches-and-record-commits
title: "Linux 7.4 lands with a massive flood of AI patches and record commits"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["linux", "release", "torvalds", "support", "kernel", "developers", "patches", "more"]
source: https://www.howtogeek.com/linux-74-rc1-released/
source_anchor: ""
source_lines: [1, 29]
section: "Langages_Dev"
---
Linux kernel developers have spent much of 2026 debating how to deal with AI-assisted code contributions, including whether developers should disclose their use of AI tools and how maintainers should review machine-generated patches. Following an ever-busier Linux 7.3 testing cycle, Linus Torvalds appears to be more at ease with machine-assisted contributions as he released Linux 7.4-rc1.

The Linux 7.4 kernel is turning into one of the busiest releases in recent memory with nearly 20,000 commits getting into the first release candidate. Linux 7.4-rc1 is the first release candidate and wraps up an unusually busy merge window. For context, a normal kernel release comes with about 15,000 commits, so this cycle’s workload is considerably larger. In his latest kernel development update, Torvalds recognized the increase, saying AI-generated contributions are becoming a more organic part of the process.

## AI is becoming part of the development pipeline

### AI contributions are arriving earlier in the process

Torvalds isn’t celebrating AI-generated code without reservations. His concern is when those contributions come into the development cycle. In the past, AI-assisted bug reports and suggested patches often showed up at the release candidate stage, when developers should be fixing bugs and stabilizing the kernel.

He'd like to see that work into the first merge window, giving maintainers more time to review, rewrite, and test it. Torvalds also said that AI-suggested patches are becoming more common but often still need to be rewritten by the developers before they are ready.

There may be another explanation for the unusually busy cycle. There is an expectation that Linux 7.4 will become the next long-term support (LTS) release, and that could encourage developers to shove more changes into the release. But Torvalds cautioned that AI alone may not explain the rise.

## AMD, Apple, Google, and Qualcomm get noticed

### New processor and hardware support lead the changes

Outside of the AI talk, Linux 7.4 also has plenty of hardware-related improvements. About a third of the massive patch is for AMD graphics changes, mostly register offsets and bit-mask changes.

Other notable additions are full support for AMD’s upcoming Ryzen Zen 6 processors, Radeon PerfOpt improvements aimed at AI and large language model workloads, and AMDGPU HDMI 2.1 support enabled by default, as noted by Phoronix.

Apple's M4 silicon also gets basic kernel enablement, but it's not ready for everyday users yet. It adds support for the Google Tensor G5 and Pixel 10, and Qualcomm’s Snapdragon X2 PC platform has had a few more tweaks. Intel’s Nova Lake laptops will also receive IPU8 support.

There are smaller improvements across drivers, filesystems, networking, Rust, BPF, memory management, performance tools, and selftests. Other fixes and performance improvements include support for a 10-inch monitor for Raspberry Pi users.

Linux 7.4-rc1 is the start of the stabilization phase, and developers may expect seven or eight weekly release candidates over the next two months before the stable version arrives. Torvalds also has two large pull requests that might make it into rc2. If the development goes as planned, Linux 7.4 could be out by the end of December and will be the LTS release for this year.

Source: Linus Torvalds
