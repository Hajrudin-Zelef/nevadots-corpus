---
id: collect-261121-veille-ia/langages-dev/linux-7-2-is-out-what-embedded-teams-should-test-first
title: "Linux 7.2 Is Out: What Embedded Teams Should Test First"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["cache", "linux", "embedded", "randomizer", "before", "changes", "support", "parts"]
source: https://dev.to/raghu_bharadwaj_404e60eb0c/linux-72-is-out-what-embedded-teams-should-test-first-1phe
source_anchor: ""
source_lines: [1, 99]
section: "Langages_Dev"
---
Linux 7.2 was released on 16 August 2026. For embedded products the changes worth testing are not the headline hardware support: cache-aware load balancing alters task placement on parts with more than one last-level cache, SPI NAND randomizer support changes what is physically written to flash, dma-buf heaps became loadable modules, and a set of removals will fail either at build time or silently at runtime.

Linus Torvalds released Linux 7.2 on 16 August. General coverage has focused on initial Apple M3 support and faster I/O. If you ship an embedded Linux product, neither is the reason to read this changelog. This article covers the changes most likely to alter behaviour on your hardware, and what to test before you commit to the bump.

The scheduler change most likely to move your numbers is cache-aware load balancing. The stated goal is "co-locating tasks that share data (ie. threads of the same process) within the same Last Level Cache domain", so that "the scheduler can reduce cache bouncing and cache misses, ultimately improving data access efficiency".

**This only has an effect on parts with more than one last-level cache domain, and multi-cluster is not the same thing as multi-LLC.** Most embedded ARM SoCs put every cluster behind a single shared DSU and L3, which is one LLC domain. On those parts the balancer has nothing to choose between and the feature does nothing. Check before you assume anything changed:


```
raghu@techveda.org:~$ cat /sys/devices/system/cpu/cpu*/cache/index*/shared_cpu_list
```
If every CPU shares one last-level cache, this change does not apply to you.

Where it does apply, be careful about who is exposed. Load balancing operates strictly inside a task's allowed CPU mask, so a thread pinned with `taskset`, `sched_setaffinity()` or a cpuset cannot be moved outside that mask. A fully pinned workload therefore sees no change and gains nothing from the feature. The exposure is unpinned threads, and mixed designs where some threads are pinned and the remainder may now be pulled toward a different cache domain than before.

Also in this release, sched_ext gained topological CPU IDs: "a dense, topology-ordered CPU numbering where the CPUs of a core, LLC, or NUMA node form contiguous ranges, so a topology unit becomes a (start, length) slice". If you are writing a BPF scheduler, that makes topology-aware placement much easier to express. We covered the callback and dispatch queue model in writing a minimal sched_ext scheduler.

The MTD work in this cycle is unusually product-relevant. SPI NAND gained randomizer support, with Macronix parts enabled specifically; Winbond continuous read landed; and SPI NOR software protection was enhanced.

The randomizer matters more than its one-line changelog entry suggests. A NAND randomizer whitens data before it is written, so that long runs of identical patterns do not produce correlated cell charge, program and read disturb, and error bursts that exceed what the ECC can correct. It is a bit-error-rate and disturb-margin mechanism. It is not a program/erase endurance mechanism, and it is not a tuning knob.

**Because it changes what is physically stored, a kernel that newly enables the randomizer cannot read flash that was written without it.** Treat it as a data-format change. If your product has fielded units, work out what happens to an existing UBI volume or root filesystem before you ship a kernel that turns it on.

On the filesystem side, f2fs reduced time spent in interrupt context, which addresses interrupt latency and system jitter rather than write stalls. Write stalls in f2fs come from garbage collection, checkpointing and free-segment exhaustion. The separate addition of iostat latency tracking for direct I/O is the item that helps you diagnose them.

DMA-BUF heaps were turned into modules in this cycle. The reason this is more than a packaging detail is that heaps register no modalias, so udev has nothing to match on and cannot autoload them. Opening a device under `/dev/dma_heap/` does not trigger anything either. A heap that was previously built in and always present must now be loaded deliberately.


```
raghu@techveda.org:~$ ls /dev/dma_heap/
system
```
An empty or short listing alone does not tell you whether the heap is missing or merely unloaded, so check `/sys/module/` or `modinfo` before concluding the kernel lacks support. The remedy is `/etc/modules-load.d/`, an explicit modprobe before your media stack starts, or keeping the heaps built in. Initramfs is usually the wrong place to look, because dma-buf allocation almost never happens before switch_root. Note also that a CMA-backed heap takes its name from the reserved-memory node in your device tree, so do not expect a heap literally called `cma`.

Linux 7.2 adds a dm-inlinecrypt target, described in the changelog as "a practical replacement for dm-crypt", with hardware-wrapped key support landing alongside it. That description is the author's framing, and it comes with conditions worth stating plainly:

- It needs an inline crypto engine, typically in a UFS or eMMC host controller. Without one there is nothing to offload to and no advantage over dm-crypt.
- Hardware-wrapped keys additionally depend on vendor firmware or TEE support.
- The available algorithms are whatever the engine implements, not dm-crypt's full cipher set.
- There is no in-place migration. Switching targets means re-encrypting the device, which for a fielded product means a full reflash or a dual-slot update.

Evaluate it, but full-disk encryption is not the place to be an early adopter without measurement and a rollback plan.

- 
**Qualcomm Dragonwing IPQ9650** , a wireless networking SoC with four Cortex-A55 and one Cortex-A78 core, described as "a significant upgrade from older generations".
- 
**Renesas R-Car M3Le (R8A779MD)** , a variant of the R-Car M3-N automotive SoC.
- 
**ASPEED AST27xx** , "their first baseboard managment controller using a 64-bit core, the Cortex-A35".
- 
**18 industrial embedded boards** using NXP i.MX6/8/9 and LX2160A from Variscite, Toradex and SolidRun, plus additional overlays.
- Initial **Rockchip rk3528** support, and new i.MX boards including the FRDM-IMX95-PRO.

On RISC-V, the notable kernel-side item is support for non-leaf and range invalidation.

Removals split into two kinds, and conflating them is how teams get caught.

**API removals break the build.** `strncpy()` has been removed from the kernel. Any out-of-tree driver still using it stops compiling. When you replace it, note that `strscpy()` does not NUL-pad the destination; for a fixed-size buffer handed to hardware, use `strtomem_pad()` or you will ship uninitialised tail bytes.

**Driver removals build cleanly and fail at runtime**, as a device that simply never probes. These are harder to catch because nothing complains until the hardware is in front of you.

| Removed in 7.2 | How it fails | 
|---|---|
| `strncpy()` | Build failure in any out-of-tree code still using it. | 
| All PCMCIA *Bluetooth* drivers (DTL1, BT3C, BlueCard) | Runtime. Note the PCMCIA-era networking drivers went in 7.1, not this release. | 
| EXT2 DAX support | Mount-time, for anything using direct access on ext2. | 
| EROFS fscache backend | Runtime, for read-only rootfs designs built on that combination. | 
| MTD `impa7` map driver | Runtime, on older ARM boards carrying that map. | 
| AF_ALG AIO and old-style drivers; zero-copy in skcipher and aead | Runtime, in user-space crypto paths using the AF_ALG socket interface. | 
| `exynos-rng` ,`xilinx_ps2` ,`atmel-isc` , several PCAP drivers, ARCnet | Runtime, on boards carrying any of these in a vendor tree. | 

One toolchain item is often misreported, so it is worth being precise: `CONFIG_WARN_CONTEXT_ANALYSIS`, an opt-in Clang context-analysis warning build, now requires Clang 23. The kernel's minimum Clang version is unchanged, so this only affects you if you deliberately enable that option.

A practical order for evaluating Linux 7.2 on an existing product:

1. Confirm the kernel you are moving to is supportable for your maintenance window before doing any engineering work.
2. Rebase and audit your out-of-tree patches, vendor drivers and device tree changes. On a real product this is the largest single item.
3. Build, and fix what no longer compiles. `strncpy()` is the likely culprit.
4. Run `make olddefconfig` and review every new symbol that defaulted on.
5. Rebuild all external and DKMS modules, and re-check module signing and secure boot keys.
6. Check userspace minimum versions and any sysfs or ioctl interface your applications depend on.
7. Measure: boot time, idle power, memory footprint, and scheduling latency if you have multiple LLC domains.
8. Test flash behaviour before shipping if you use SPI NAND, particularly around the randomizer.
9. Have a rollback or A/B path in place before any of this reaches a device you cannot physically retrieve.

- Linux 7.2 was released on 16 August 2026.
- Cache-aware load balancing only matters if you have more than one last-level cache domain, and it does not override affinity masks.
- SPI NAND randomizer support changes on-media layout, so a kernel that enables it cannot read flash written without it.
- dma-buf heaps are modules and register no modalias, so nothing will autoload them for you.
- dm-inlinecrypt needs an inline crypto engine, and there is no in-place migration from dm-crypt.
- Removals split into build failures (`strncpy()` ) and silent runtime failures (everything else).

- Linux 7.2 changelog — kernelnewbies, the per-subsystem list every feature above is drawn from.
- "Linux 7.2 Released With Faster I/O, New AMD & Intel Driver Improvements" — Phoronix release coverage.
- LWN 7.2 merge window, part 1 and part 2 — the merge-window summaries.
- "string: Remove strncpy() from the kernel" — the commit behind the one removal that breaks builds.

## Top comments (0)
