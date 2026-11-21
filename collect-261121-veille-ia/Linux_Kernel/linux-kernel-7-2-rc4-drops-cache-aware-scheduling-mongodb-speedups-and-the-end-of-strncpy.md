---
id: collect-261121-veille-ia/linux-kernel/linux-kernel-7-2-rc4-drops-cache-aware-scheduling-mongodb-speedups-and-the-end-of-strncpy
title: "Linux Kernel 7.2-rc4 Drops: Cache-Aware Scheduling, MongoDB Speedups, and the End of strncpy"
domain: linux-kernel
role: reference
task: article
actors: []
dates: ["2026-07-19"]
keywords: ["linux", "kernel", "support", "strncpy", "scheduling", "mongodb", "driver", "cache-aware"]
source: https://www.linuxcompatible.org/story/linux-kernel-72rc4-drops-cacheaware-scheduling-mongodb-speedups-and-the-end-of-strncpy/
source_anchor: ""
source_lines: [1, 70]
section: "Linux_Kernel"
---
### Linux Kernel 7.2-rc4 Landed: 43 Million Lines and the "New Normal" of Kernel Development

**The fourth release candidate brings cache-aware scheduling, MongoDB speedups, and the final nail in the coffin for strncpy. Stable release expected in August.**

Linux 7.2-rc4 hit the wire on July 19, 2026. Linus Torvalds didn't mince words when reviewing the patch set. He called it the "new normal." By this stage of an rc cycle, you'd expect the patch flow to dry up. Developers go on vacation. The tree stabilizes. Not this time. The numbers show a steady stream of fixes, many uncovered by AI and LLM tools, across the codebase.

"It's not a small rc4," Torvalds noted. The codebase now exceeds 43 million lines. That growth continues without mercy. Keep in mind that the stable kernel is still weeks away. Expect Linux 7.2 to debut in August 2026.

#### Cache Aware Scheduling and Zen 5 Wins

One of the biggest technical wins in this cycle is the merge of Cache Aware Scheduling. This feature improves task placement on CPUs with multiple last-level cache domains. If you're running Intel or AMD hardware with complex cache topologies, this is relevant to you. Benchmarks point to meaningful performance gains on AMD Zen 5 processors, particularly for PostgreSQL, Valkey, and network workloads.

The optimizer also targeted the MongoDB workload. MGLRU improvements can boost throughput by 30 to 100 percent in benchmarks. You'll see faster poll performance, reduced container exit latency, and a massive 444 percent speedup for reading /proc/filesystems.

Next, the slab allocator got a general optimization pass. Pipes for shell workflows got attention too. Anonymous and unnamed pipe performance improved, which matters if you're piping data between processes in tight loops.

#### Graphics, Storage, and the "New" Old Hardware

The AMDGPU driver picked up initial support for HDMI 2.1 FRL. That's Fixed Refresh Rate, paving the way for full HDMI 2.1 support upstream. The Display Core also gained a new power management module to handle the workload. The AMDISP4 driver finally merged. This enables web camera support on the HP ZBook Ultra G1a and other high-end Ryzen laptops.

Storage changes include Btrfs large folios enabled by default. XFS zone allocator is no longer experimental. If you need to open only regular files, OPENAT2_REGULAR is the syscall to look at. Graphics updates aren't limited to AMD. Intel DRM picked up a background color property. Nouveau gained support for GA100 accelerators. NVIDIA has early VFIO preparations for Blackwell-Next.

#### Cleaning the Attic: strncpy Finally Dead

It took six years and over 362 patches to kill the strncpy API. You might wonder why it took so long to replace a single string function, but the Linux kernel history is full of legacy code that refused to die. strncpy is now gone. It's been replaced by strscpy, strscpy_pad, and similar safe alternatives. The removal was long overdue, especially given the buffer overflow risks the function historically carried.

The cleanup crew didn't stop there. Several ancient drivers got the ax. The Hercules Monochrome ISA graphics driver is dropped. AppleTalk support is gone. ARCnet ISA/PCMCIA is out. The ISA speech synthesizer driver joined the graveyard.

We're also seeing the continued phase-out of i486 support, which was initially disabled in Linux 7.1. At this point, a 40-year-old CPU architecture no longer has a home in the mainline kernel. To say the least, keeping up with hardware changes gets harder the further back you go.

#### Early Apple M3 Support and Security

Apple M3 devices can now boot from the mainline Linux 7.2 kernel. Keep in mind that this support isn't in a practical stage for end users just yet. It's more of a proof of concept than a daily driver.

Intel TDX gets runtime updates for Trusted Domain Extensions, meaning security updates without a reboot. Security also brought IMA/EVM post-quantum ML-DSA signature support. Intel Key Protection Technology landed for QAT Gen6 devices. Timers got DoS protections to guard against "stupid or malicious" denial of service attempts. The kernel is explicitly targeting timer arming attacks.

#### Developer Tooling and AI

Linus confirmed the kernel community is evaluating AI-generated patches. The project is not "anti-AI," nor is it a "social warrior" project. The steady patch flow through rc4 included fixes identified by LLM tools, and that trend shows no sign of slowing down.

Developer tooling saw sched_ext restructured. KUnit now supports JUnit XML output. The Rust Zerocopy library arrived to help eliminate unsafe code.

#### Linux 7.2 Timeline

Here's where things stand for the release cycle:

| Version | Release Date | 
|---|---|
| v7.2-rc1 | June 28, 2026 | 
| v7.2-rc2 | July 6, 2026 | 
| v7.2-rc3 | July 12, 2026 | 
| **v7.2-rc4** | **July 19, 2026** | 
| Linux 7.2 stable | August 2026 | 


For the changelog and download link, see the official announcement below:

#### Linux kernel 7.2-rc4 released

Linux kernel version 7.2-rc4 is now available:


Full source:    https://git.kernel.org/torvalds/t/linux-7.2-rc4.tar.gz

Patch:          https://git.kernel.org/torvalds/p/v7.2-rc4/v7.1


You can view the summary of the changes at the following URL:

https://git.kernel.org/torvalds/ds/v7.2-rc4/v7.2-rc3
