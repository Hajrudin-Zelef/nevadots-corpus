---
id: collect-261121-veille-ia/langages-dev/kernel-embedded-news-linux-7-2-nears-release-swap-i-o-rework
title: "Kernel & Embedded News: Linux 7.2 Nears Release; Swap I/O Rework"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["swap", "kernel", "release", "stable", "linux", "series", "embedded", "devices"]
source: http://dev.to/raghu_bharadwaj_404e60eb0c/kernel-embedded-news-linux-72-nears-release-swap-io-rework-52e0
source_anchor: ""
source_lines: [1, 52]
section: "Langages_Dev"
---
Linux 7.2 is expected as a stable release around 16 August 2026, so teams carrying an embedded BSP should test against the release candidates now, especially for the removals that can break out-of-tree code. This edition also covers an upstream RFC that reworks how the kernel distributes I/O across multiple swap devices, the support-window gap between Buildroot's three-month stable series and its long-term-support line, and Bootlin's July update to the SiFive RISC-V Yocto SDK, which now tracks the current Yocto release and the 6.18 kernel.


*Linux 7.2 heads toward release, an RFC reworks multiple-swap-device I/O, Buildroot's stable series nears end of life while its LTS runs to 2028, and Bootlin updates its SiFive RISC-V Yocto SDK.*

The common thread this fortnight is maintainability: which kernel is about to arrive, how a core subsystem is being restructured, and which build baselines you can realistically support across a product's life.

- 
**Linux 7.2 nears its stable release.** The kernel is in its release-candidate phase, with the stable version expected around 16 August 2026 and several removals that affect out-of-tree code.
- 
**A swap-device rework drops a global lock.** An RFC redistributes I/O across multiple swap devices and replaces the global priority list and lock with per-CPU structures.
- 
**Buildroot's stable series nears end of life.** The current three-month stable reaches end of life in September 2026, while the LTS line is supported to 2028.
- 
**Bootlin updates its SiFive RISC-V Yocto SDK.** Freedom-U-SDK 2026.07.00-lts tracks the current Yocto release and the 6.18 kernel for SiFive platforms.

Linux 7.2 is in its release-candidate phase, and the stable kernel is expected around 16 August 2026. The 7.2 merge window closed on 28 June, after the 7.1 stable release on 14 June. Once released, 7.2 will feed distributions such as Fedora 45 and Ubuntu 26.10, and it will be the version many embedded BSP rebases target next.

For product teams the removals matter more than the headline features. Linux 7.2 finished eliminating the long-deprecated `strncpy()` API from the kernel, dropped the architecture-specific optimised MD5 implementations, and began deprecating AF_ALG, the user-space crypto socket interface. The Kbuild changes also raise the minimum LLVM/Clang version, which matters if you build your BSP with Clang rather than GCC.

None of these break a typical distribution kernel, but they can break out-of-tree drivers and older userspace that a product carries. The practical step is to build your out-of-tree module tree against a 7.2 release candidate now and boot it on real hardware, so any removed symbol or raised toolchain requirement shows up while there is still time to report a regression before the stable tag. Treat the mid-August date as a planning marker, not a signal to ship.

Kairui Song of Tencent has posted an RFC patch series that reworks how Linux distributes I/O across multiple swap devices. The series removes the mutable priority list (plist) and the global `swap_avail_lock`, drops the global cluster cache, and lets device allocation and rotation happen more freely. It is aimed particularly at swap tiering and layered swap, where the kernel needs to prefer some swap devices over others.

The substance is a locking change. Today a global per-CPU cluster cache sits above device selection, and rotating between devices for round-robin requires holding `swap_avail_lock`, so every CPU contends on the same list head. The series guards all swap devices and a new per-CPU reader priority queue with a per-CPU rwsem, which scales better and removes the fast-path versus slow-path split that had accumulated workarounds over the years.

Most embedded systems use a single swap area or zram, so this changes nothing for them directly, and the work is an RFC that will land only after the swap tiering series it depends on. Its value is as a study of how the kernel takes a global lock off a hot path. Replacing a shared plist and lock with per-CPU state guarded by a per-CPU rwsem is a pattern that recurs across the kernel's scalability work, and it is worth reading the series for that pattern rather than for swap tiering itself.

Buildroot issued the point releases 2026.05.1 and 2025.02.16 on 15 July 2026. What matters more than any single release note is the support window on each line: the current stable series 2026.05.x reaches end of life in September 2026, while the long-term-support series 2025.02.x is supported until March 2028.

Buildroot's release model is worth understanding before you pick a baseline. A new stable release appears roughly every three months and is maintained only until the next one, while an LTS line is started every two years and carries a three-year support window with monthly point releases. The consequence is direct: the freshest-looking release is also the one that reaches end of life soonest. You can check which release a tree is on with a single command:


```
raghu@techveda.org:~$ git -C buildroot describe --tags
2025.02.16
```
For a product you intend to ship and maintain, base the build on the LTS series (2025.02.x today), not on the newest three-month stable, unless you specifically need a package or board that was added after the LTS branched. Point releases carry security and build fixes, so track the CHANGES file and rebase to each one rather than pinning a single tag and forgetting it. Choosing the right Buildroot or Yocto baseline, and planning the rebase cadence around its support window, is a routine but consequential decision, and one we work through in our embedded Linux courses.

Bootlin maintains Freedom-U-SDK (FUSDK), the Yocto/OpenEmbedded software development kit for SiFive RISC-V platforms, in collaboration with SiFive. Its latest release, 2026.07.00-lts, tracks the current Yocto Project release, Wrynose (6.0), and targets the HiFive Unmatched board and QEMU RISC-V 64-bit. Support is split into a machine BSP layer (meta-sifive) and the FUSDK demo and SDK layer.

What is instructive is the multi-year version history, which shows how a real RISC-V BSP has tracked upstream. The kernel moved from 5.19 to the 6.1 LTS branch in early 2023, then 6.6 in 2024, then 6.12 in early 2025, and to the 6.18 branch in 2026, while the toolchain in 2026.07.00-lts reached LLVM 22.1.3, GCC 15.2, Glibc 2.43 and Binutils 2.46. You can fetch the release directly:


```
raghu@techveda.org:~$ git clone -b 2026.07.00-lts https://github.com/sifive/freedom-u-sdk.git
```
Two points are worth taking from this. RISC-V embedded support has reached the stage where a maintained Yocto BSP tracks current toolchains and the newest kernel LTS, rather than sitting years behind, which lowers the risk of basing a RISC-V product on it. And the version table is a model for planning your own kernel-LTS cadence: each jump (6.1, 6.6, 6.12, 6.18) is a deliberate migration to budget for, not an accident. Reading a well-documented BSP history like this is a useful exercise for anyone learning how embedded Linux platforms are maintained over time.

— Raghu Bharadwaj

## Top comments (0)
