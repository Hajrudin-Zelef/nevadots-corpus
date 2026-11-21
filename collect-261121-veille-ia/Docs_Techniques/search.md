---
id: collect-261121-veille-ia/docs-techniques/search
title: "Linux Kernel 7.2 Officially Released with Cache-Aware Scheduling, USB4STREAM, and Major AMD Improvements"
domain: docs-techniques
role: reference
task: article
actors: []
dates: ["2026-08-16"]
keywords: ["linux", "kernel", "scheduling", "hdmi", "between", "cache-aware", "usb4stream", "development"]
source: https://www.linuxjournal.com/index%2ephp/content/linux-kernel-72-officially-released-cache-aware-scheduling-usb4stream-and-major-amd
source_anchor: ""
source_lines: [1, 174]
section: "Docs_Techniques"
---
# Linux Kernel 7.2 Officially Released with Cache-Aware Scheduling, USB4STREAM, and Major AMD Improvements

Linux creator **Linus Torvalds has officially released Linux Kernel 7.2**, opening another chapter in the development of the world's most widely deployed open-source kernel. The final release arrived on **August 16, 2026**, following seven release candidates and roughly two months of development. Kernel.org now lists Linux 7.2 as the latest mainline release.

Linux 7.2 is a substantial update with improvements spanning CPU scheduling, storage performance, AMD and Intel hardware support, graphics, networking, virtualization, RISC-V, and Apple Silicon. Among its biggest additions are **Cache-Aware Scheduling, USB4STREAM, AMD ISP4 support, and initial AMDGPU HDMI 2.1 FRL functionality**.

## **Cache-Aware Scheduling Lands in Linux**

One of the most interesting performance additions in Linux 7.2 is **Cache-Aware Scheduling (CAS)**.

Modern processors frequently contain multiple last-level caches shared between groups of CPU cores. Traditional scheduling decisions don't always account optimally for those cache relationships, potentially moving tasks between cores in ways that increase cache misses.

Cache-Aware Scheduling gives the Linux scheduler additional information about cache topology so it can make smarter decisions about where workloads should run.

Early testing during development produced particularly impressive results for some server workloads, including major gains in certain MySQL configurations. The improvement varies significantly by hardware and workload, so users shouldn't expect similarly dramatic gains everywhere.

For modern AMD and Intel systems with increasingly complex CPU topologies, however, CAS provides another tool for extracting better performance from existing hardware.

## **USB4STREAM Enables High-Speed Connections Between Computers**

Linux 7.2 also introduces **USB4STREAM**, an interesting new capability for transferring data directly between computers over USB4 or Thunderbolt connections.

Rather than treating USB primarily as a traditional host-to-device interface, USB4STREAM can facilitate high-speed communication between systems.

Potential applications include:

- Large file transfers
- Development environments
- High-speed system-to-system networking
- Debugging
- Specialized workstation workflows

With USB4 and Thunderbolt increasingly common on laptops and desktops, this could eventually become a useful option for moving large amounts of data between Linux machines.

## **AMD ISP4 Finally Reaches the Mainline Kernel**

AMD users receive another important addition with the arrival of the **AMD ISP4 driver**.

ISP stands for **Image Signal Processor**, hardware responsible for processing camera data before it reaches applications.

The driver is particularly relevant for newer AMD-powered laptops containing integrated camera hardware that depends on AMD's ISP architecture.

Bringing the driver upstream reduces the need for vendor-specific kernel patches and should make camera support easier for Linux distributions shipping on newer AMD notebooks.

## **Initial AMDGPU HDMI 2.1 FRL Support**

Another long-awaited improvement is initial **HDMI 2.1 Fixed Rate Link (FRL)** support within AMDGPU.

FRL provides the increased bandwidth necessary for high-resolution, high-refresh-rate HDMI configurations that exceed the capabilities of traditional TMDS signaling.

This is particularly relevant for Linux users connecting Radeon GPUs to modern:

- 4K gaming monitors
- High-refresh-rate televisions
- HDMI 2.1 displays
- Home theater equipment

Linux's HDMI 2.1 situation has been complicated by restrictions surrounding the HDMI specification, so the arrival of initial FRL functionality represents an important development for Radeon users.

## **More AMD Zen 6 Preparation**

Linux 7.2 continues preparing the kernel for **AMD's upcoming Zen 6 processors**.

As is typical before new CPU generations reach consumers, AMD engineers have been upstreaming support gradually so Linux is ready when the hardware becomes commercially available.

The kernel also receives numerous improvements affecting current AMD systems, including audio, graphics, power management, and platform support.

This early upstream development is particularly important for Linux distributions because it reduces the amount of downstream patching required when new hardware launches.

## **Faster Storage and I/O**

Storage performance receives considerable attention in Linux 7.2.

One surprisingly small optimization affecting **EXT4 and XFS** reportedly produced around a **5% IOPS improvement** in testing simply by changing the ordering of two operations within a frequently executed path.

Another optimization significantly speeds up reads of `/proc/filesystems`, an operation that turns out to occur much more frequently than many users might expect.

Linux 7.2 also includes broader improvements across the kernel's storage and I/O infrastructure, helping modern NVMe SSDs and high-performance servers handle workloads more efficiently.

## **MGLRU Gets Faster**

Linux's **Multi-Gen LRU (MGLRU)** memory-management infrastructure also receives an important optimization.

MGLRU is designed to make memory reclamation more efficient by identifying which memory pages are most likely to be needed again.

Testing during the Linux 7.2 development cycle showed substantial improvements for some MongoDB workloads, with reported throughput gains ranging from roughly **30% to 100%** under particular configurations.

As with all benchmarks, these results are workload-specific, but they demonstrate how seemingly low-level kernel changes can significantly influence real-world server performance.

## **Apple M3 Can Now Boot the Mainline Kernel**

Linux 7.2 also reaches an important milestone for **Apple Silicon**.

Initial upstream support now allows **Apple M3 hardware to boot the mainline Linux kernel**.

That doesn't mean M3 Macs suddenly have complete upstream Linux support. Significant hardware enablement is still required before these machines provide the polished experience available on older Apple Silicon hardware through projects such as Asahi Linux.

Nevertheless, successfully booting the mainline kernel establishes an important foundation for future development.

## **RISC-V Keeps Moving Forward**

RISC-V continues receiving substantial attention throughout the kernel.

Linux 7.2 expands support for newer RISC-V processors and development boards while enabling **ESWIN SoC support by default** for RISC-V kernel configurations. Work also continues around platforms such as SpacemiT's K1 and K3 processors.

The steady stream of upstream RISC-V improvements demonstrates how quickly the architecture is maturing as a Linux platform.

## **Intel Hardware Improvements**

Intel users aren't left out of this release.

Linux 7.2 contains numerous updates for Intel processors, graphics hardware, laptops, and enterprise technologies. One particularly notable virtualization improvement makes **Intel TDX** more practical by allowing certain important updates without requiring a reboot.

Intel graphics and platform drivers also continue preparing for newer hardware generations.

## **NVIDIA’s Open-Source Nova Driver Continues Growing**

Work also continues on **Nova**, the Rust-based open-source NVIDIA GPU driver being developed within the Linux DRM subsystem.

Linux 7.2 brings additional Nova infrastructure along with other Rust-related DRM work.

Nova remains under development and isn't yet positioned as a replacement for the established Nouveau or NVIDIA proprietary/open kernel driver stack for ordinary users. However, its continued progress is one of the more interesting long-term developments in Linux graphics.

## **DRM Scheduler Regression Reverted Before Release**

Linux 7.2's development cycle wasn't entirely smooth.

A new fair scheduling policy for the DRM GPU scheduler caused serious performance problems under some sustained GPU workloads, including gaming scenarios. Kernel developers ultimately **reverted the change before the final release**, restoring the established FIFO scheduling behavior as the default.

The FAIR policy remains experimental while developers continue investigating the problems.

This means users installing the final Linux 7.2 kernel receive the safer scheduler configuration rather than the problematic default tested during the release-candidate period.

## **Old Hardware Continues to Disappear**

Linux 7.2 also removes additional obsolete hardware support.

One particularly nostalgic casualty is the driver for the **Hercules monochrome graphics adapter**, technology dating back more than four decades.

Removing ancient and effectively unused drivers reduces maintenance requirements and allows kernel developers to spend more time working on hardware people actually use.

## **One of the Busiest Kernel Development Cycles**

Linux 7.2 was also notable simply for the scale of development.

The merge window contained **13,412 non-merge commits from 2,138 contributors**, including **404 first-time contributors**, making it one of the busiest kernel merge windows in recent years.

The sheer number of contributors demonstrates just how enormous Linux kernel development has become, encompassing everything from tiny embedded boards to smartphones, gaming PCs, cloud servers, supercomputers, and AI infrastructure.

## **Should You Upgrade to Linux 7.2?**

Users don't generally need to download and compile Linux 7.2 manually.

Rolling-release distributions will typically adopt the new kernel relatively quickly, while distributions such as Ubuntu, Debian, and enterprise Linux systems may continue using older supported kernels while backporting important security and hardware fixes.

Kernel.org explains that mainline kernels introduce new features, while subsequent stable releases provide fixes that are backported from newer development.

Users who need support for very new hardware may have stronger reasons to move to Linux 7.2, particularly those running recent AMD platforms or experimenting with hardware that depends on newly upstreamed drivers.

## **What Comes Next**

With Linux 7.2 officially released, development now shifts toward **Linux 7.3**.

The usual two-week merge window follows the mainline release, during which maintainers submit the major features intended for the next kernel. Afterward, Linux 7.3 will move through its weekly release-candidate cycle before reaching stable status.

Meanwhile, Linux 7.2 will receive incremental 7.2.x stable updates containing important bug and regression fixes.

## **Conclusion**

**Linux Kernel 7.2 is a substantial release** that combines important new hardware support with meaningful performance work. Cache-Aware Scheduling could improve task placement on modern CPUs, USB4STREAM opens interesting possibilities for high-speed system-to-system communication, and AMD users gain ISP4 support alongside initial HDMI 2.1 FRL functionality.

Add improvements for storage, memory management, Intel hardware, RISC-V, Apple M3 systems, and the continuing development of Rust-based graphics infrastructure, and Linux 7.2 represents another broad upgrade to the kernel's capabilities.

For most users these changes will arrive quietly through future distribution updates, but underneath the desktop, Linux 7.2 lays important groundwork for the next generation of CPUs, GPUs, laptops, servers, and high-performance Linux systems.
