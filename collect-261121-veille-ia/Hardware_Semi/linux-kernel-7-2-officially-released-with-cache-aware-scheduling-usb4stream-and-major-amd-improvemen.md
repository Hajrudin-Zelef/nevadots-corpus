---
id: collect-261121-veille-ia/hardware-semi/linux-kernel-7-2-officially-released-with-cache-aware-scheduling-usb4stream-and-major-amd-improvemen
title: "Linux Kernel 7.2 Officially Released with Cache-Aware Scheduling, USB4STREAM, and Major AMD Improvements – Linux Consultant"
domain: hardware-semi
role: reference
task: article
actors: []
dates: ["2026-08-16"]
keywords: ["linux", "scheduling", "kernel", "cache-aware", "usb4stream", "between", "officially", "released"]
source: https://www.linuxconsultant.org/linux-kernel-7-2-officially-released-with-cache-aware-scheduling-usb4stream-and-major-amd-improvements/
source_anchor: ""
source_lines: [1, 41]
section: "Hardware_Semi"
---
## Linux Kernel 7.2 Officially Released with Cache-Aware Scheduling, USB4STREAM, and Major AMD Improvements

Linux creator **Linus Torvalds has officially released Linux Kernel 7.2**, opening another chapter in the development of the world’s most widely deployed open-source kernel. The final release arrived on **August 16, 2026**, following seven release candidates and roughly two months of development. Kernel.org now lists Linux 7.2 as the latest mainline release.

Linux 7.2 is a substantial update with improvements spanning CPU scheduling, storage performance, AMD and Intel hardware support, graphics, networking, virtualization, RISC-V, and Apple Silicon. Among its biggest additions are **Cache-Aware Scheduling, USB4STREAM, AMD ISP4 support, and initial AMDGPU HDMI 2.1 FRL functionality**.

## **Cache-Aware Scheduling Lands in Linux**

One of the most interesting performance additions in Linux 7.2 is **Cache-Aware Scheduling (CAS)**.

Modern processors frequently contain multiple last-level caches shared between groups of CPU cores. Traditional scheduling decisions don’t always account optimally for those cache relationships, potentially moving tasks between cores in ways that increase cache misses.

Cache-Aware Scheduling gives the Linux scheduler additional information about cache topology so it can make smarter decisions about where workloads should run.

Early testing during development produced particularly impressive results for some server workloads, including major gains in certain MySQL configurations. The improvement varies significantly by hardware and workload, so users shouldn’t expect similarly dramatic gains everywhere.

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

Source: Linux Journal - The Original Magazine of the Linux Community
