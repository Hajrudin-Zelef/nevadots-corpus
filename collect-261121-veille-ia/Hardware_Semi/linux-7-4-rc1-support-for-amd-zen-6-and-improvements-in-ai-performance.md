---
id: collect-261121-veille-ia/hardware-semi/linux-7-4-rc1-support-for-amd-zen-6-and-improvements-in-ai-performance
title: "Linux 7.4-rc1: Support for AMD Zen 6 and Improvements in AI Performance"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["support", "kernel", "been", "linux", "hardware", "improvements", "release", "performance"]
source: https://www.pcmasters.net/news/133716928-linux-7-4-rc1-support-for-amd-zen-6-and-improvements-in-ai-performance.html
source_anchor: ""
source_lines: [1, 32]
section: "Hardware_Semi"
---
Linux Kernel Updates (Image © DALL-E)

A significant portion of the update for Linux 7.4-rc1 focuses on AMD hardware. This release introduces the AMD PerfOpt feature specifically for integrated Radeon GPUs (iGPUs), which is designed to boost performance for workloads in the areas of artificial intelligence (AI) and large language models (LLMs).

Preparations for the next generation of client CPUs have also been incorporated; the kernel now includes basic support for AMD Ryzen Zen 6. Additionally, HDMI 2.1 is now enabled by default in the AMDGPU driver.

The size of the current update is partly due to a substantial amount of AMD hardware register data. A large portion of the patch consists of GPU register offsets and bitmasks that have been added to the kernel headers.

## Comprehensive Hardware Support

Linux 7.4-rc1 extends compatibility to several additional hardware platforms:

- 
**Intel** : The kernel now supports the Intel IPU8, which is intended for upcoming Nova Lake laptops.
- 
**Qualcomm** : Additional support for Snapdragon X2 laptop processors has been added.
- 
**Google** : Support for the Google Tensor G5 SoC and Pixel 10 devices has been integrated into the mainline kernel.
- 
**Apple** : Basic support for the Apple M4 chip has been enabled, though this functionality has not yet been optimized for general use by end users.

In addition, this release includes various bug fixes and enhancements that address specific quirks of laptop hardware.

## Kernel and File System Improvements

Beyond the hardware drivers, the update brings technical improvements to the kernel itself. Optional support for buffered IOmap has been added for the EXT4 file system. Developers will also notice that kernel build times have been reduced, enabling faster compilation.

## Development Trends and Commit Volume

The merge window for Linux 7.4 saw a record number of commits—nearly 20,000 in total, compared to the usual average of 15,000. This surge in activity is largely due to a sharp increase in work related to AI integration and optimization.

The large number of AI-related patches submitted during the merge window suggests that these fixes are increasingly being integrated early in the development phase rather than waiting until the later stabilization phase. Another factor contributing to the large release size is the expectation that version 7.4 will serve as the next LTS release, which prompted more contributors to submit their changes before the window closed.
