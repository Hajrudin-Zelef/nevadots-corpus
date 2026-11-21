---
id: collect-261121-veille-ia/linux-kernel/linux-kernel-7-4-rc3-released-small-patches-and-iomap-cleanup-ostechnix
title: "Linux Kernel 7.4 RC3 Released: Small Patches and iomap Cleanup - OSTechNix"
domain: linux-kernel
role: reference
task: article
actors: []
dates: ["2026-11-15"]
keywords: ["linux", "kernel", "iomap", "patches", "small", "release", "november", "daily"]
source: https://ostechnix.com/linux-kernel-7-4-rc3-released/
source_anchor: ""
source_lines: [1, 38]
section: "Linux_Kernel"
---
*133*

Linus Torvalds announced the **third release candidate (RC3)** for the upcoming **Linux kernel 7.4** on Sunday, November 15, 2026. In the **Linux 7.4-rc3 announcement mail**, he noted that the cycle delivered "another big -rc," yet the overall development pattern looks routine and healthy.

Table of Contents

- **High Volume, Small Patches:** Developers merged many patches, but most changes remain small and focused.
- **Driver Leadership:** Networking and GPU drivers continue to lead daily commit activity.
- **iomap Refactoring:** A widespread calling convention update for`iomap` added line changes across filesystems.

Filesystems stand out in the 7.4-rc3 diffstat because maintainers integrated several structural cleanup patches.

Specifically, developers updated the `iomap` subsystem to use `->iomap_next()` instead of legacy handlers. Although this calling convention change touches many filesystem files, the core code logic remains simple and safe.

Additionally, maintainers merged several targeted fixes for SMB client operations, overlayfs redirects, and NFS storage handling.

Device drivers account for the majority of daily commit activity in this release candidate.

- **Networking & GPU Drivers:** Maintainers integrated fresh fixes across`mlx5` ,`fbnic` ,`stmmac` , and AMDGPU display stacks.
- **Architecture & Memory:** Updates span arm64 errata workarounds, x86 CPU speculation mitigations, and page allocation adjustments.
- **Tooling & Tests:** Maintainers updated kernel selftests,`perf` tools, and core developer documentation to ensure smooth automated testing.

Kernel enthusiasts can download Linux 7.4-rc3 source code directly from the **Official Linux Kernel Archives** or **Linus Torvalds' Git repository**.

Before installing this test candidate, keep these essential safety practices in mind:

- **Use Dedicated Test Machines:** Pre-release kernels carry unresolved bugs, so test them on secondary hardware or virtual machines.
- **Keep Stable Kernels on Daily Systems:** If you rely on your main computer for daily work, keep running the latest stable release.

Greg Kroah-Hartman already previously outlined the maintainer schedule scripts for the 7.4 cycle.

Because 7.4-rc3 arrived on schedule, the development team targets the following milestones:

| Development Phase | Target Date | Status | 
| **7.4 Merge Window Close (-rc1)** | November 1, 2026 | Completed | 
| **Linux 7.4-rc2 Tag** | November 8, 2026 | Completed | 
| **Linux 7.4-rc3 Tag** | November 15, 2026 | Current Tag | 
| **Projected Final 7.4 Release** | December 20, 2026 | Scheduled |
