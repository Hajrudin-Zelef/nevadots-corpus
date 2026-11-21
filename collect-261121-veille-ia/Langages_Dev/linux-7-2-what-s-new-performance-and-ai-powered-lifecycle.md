---
id: collect-261121-veille-ia/langages-dev/linux-7-2-what-s-new-performance-and-ai-powered-lifecycle
title: "Linux 7.2: What's new, performance, and AI-powered lifecycle"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["linux", "performance", "been", "improvements", "intel", "kernel", "code", "fixes"]
source: https://en.linuxadictos.com/Linux-7.2-is-here%3A-performance--new-features--and-a-cycle-marked-by-AI.html
source_anchor: ""
source_lines: [1, 31]
section: "Langages_Dev"
---
Linus Torvalds has officially released the Linux kernel **7.2** as a stable version, following a particularly intense development cycle marked by the influence of artificial intelligence tools on code review. This new version comes with a series of performance improvements, new features, and a good number of bug fixes affecting multiple subsystems. The announcement was made on the kernel mailing list, where Torvalds emphasized that, although the past week had been busier than expected, there was no reason to delay the release.

Linux 7.2 thus becomes the foundation for future distributions like Ubuntu 26.10 (although its change in philosophy leads us to believe that the final version of Stonking Stingray will be 7.3), and it will also arrive in Ubuntu 26.04 LTS as the hardware-enabled kernel. The developers have already confirmed that the kernel will be available in the repositories of major distributions in the coming weeks, and more adventurous users can already try it through Ubuntu's mainline packages.

## Key new features of Linux 7.2

Among the most striking features of Linux 7.2 is **Cache Aware Scheduling** , a functionality that promises to optimize task scheduling on systems with multiple last-level cache domains. The **USB4STREAM** protocol , developed by Intel, has also been incorporated, enabling high-speed data transfers between USB4 and Thunderbolt devices. In terms of graphics, initial support for **AMDGPU HDMI 2.1 FRL** stands out , although it is disabled by default. Improvements have also been made to the hardware drivers for both Intel and AMD, including preparation for AMD Zen 6 processors and basic support for Apple M3 chips.

Another significant addition is the ISP4 driver for AMD laptop webcams, which has been in development for some time. In the audio realm, fixes have been included to ensure the internal microphone of HyperX OMEN Gaming 16-ap1xxx laptops functions correctly, along with an adjustment for the SteelSeries Arctis Nova 5 headset. Furthermore, HDMI-In capture has been enabled for upcoming Intel Nova Lake platforms, an improvement that content creators will appreciate.

## Performance and improvements in Linux 7.2

Regarding performance, tests conducted on Intel Xeon 600 and AMD EPYC Turin platforms show **clear improvements in storage I/O operations** , especially in sequential reads and writes, as well as random writes. Polling performance, measured with Stress-NG, has improved several times compared to previous versions, a result consistent with that seen on Threadripper and EPYC processors. However, database workloads such as ClickHouse, memcached, and PostgreSQL have barely seen any changes, and in some cases, performance is virtually flat.

On the downside, a regression in futex performance was detected in Stress-NG, something also observed on AMD platforms. For NVIDIA graphics users with the Nouveau driver, there are no significant improvements, except for a small gain in VKMark with Linux 7.1. In networking, the only noticeable improvement is in ping-pong latency with Sockperf. In contrast, on Intel's Panther Lake platform (Framework Laptop 13 Pro), Linux 7.1 and 7.2 offer consistent improvements in web browsing, code compilation, PyTorch, and creative workloads, in addition to reducing SoC power consumption from 14 to 12 watts in browser benchmarks.

## A cycle marked by AI

The Linux 7.2 development cycle has been heavily influenced by the use of artificial intelligence tools and large language models (LLM) for code review. According to Torvalds, the volume of patches and fixes has increased significantly because these tools continuously scan the code and detect errors that human reviewers had missed for years. In the seventh release candidate (rc7), more than 400 fixes signed by over 230 contributors were incorporated—an unusually high number for a week before the stable release.

One of the most notable findings was a race condition in the ptdump memory subsystem, which had gone undetected for eight years and could lead to a use-after-free vulnerability. The bug was flagged by Google's Syzbot fuzzer in June, and the initial fix was implemented with the help of Claude Opus 4.8, though it was later reworked by maintainer Lorenzo Stoakes. This fix has been included in rc7 and is expected to be rolled back to stable branches. Torvalds has defended the use of AI in the project, stating that "Linux is not one of those anti-AI projects" and that it is a useful tool, though he has also acknowledged that AI-generated duplicate reports have overloaded the kernel's private security list.

## Final touches and corrections

In the final week of the cycle, several last-minute patches were merged. Among them, the most notable was the reversion of the FIFO DRM scheduler, which had been switched to a new "fair" mode but caused a regression, so the previous scheme has been reinstated. The **tlbi=ipi** boot option was also added to control TLB invalidation, and numerous minor fixes were included for drivers, networking, and architectures. Furthermore, the 1999 Moxa Intellio driver, which was no longer needed, was removed, and the Btrfs fixup infrastructure was restored to prevent silent data loss.

In the audio section, quirks have been added for the microphones of the HyperX OMEN Gaming Laptop 16-ap1xxx and the SteelSeries Arctis Nova 5, which weren't working correctly with recent kernels. HDMI-In capture has also been enabled for Intel Nova Lake, an improvement that content creators will appreciate. All these fixes are included in the final version, which is now available for download.

With the release of Linux 7.2, the kernel has solidified its position as one of the most important components of the free software ecosystem, and its adoption in popular distributions ensures that desktop and server users will be able to enjoy these improvements in the coming months. Although the cycle has been turbulent, the result is a more robust kernel with improved performance in key areas, especially storage and polling. The influence of AI on kernel development appears to be here to stay, and maintainers will have to adapt to this new workflow.

Table of Contents
