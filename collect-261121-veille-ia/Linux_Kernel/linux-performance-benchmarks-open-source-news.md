---
id: collect-261121-veille-ia/linux-kernel/linux-performance-benchmarks-open-source-news
title: "Linux Performance, Benchmarks & Open-Source News"
domain: linux-kernel
role: reference
task: article
actors: []
dates: []
keywords: ["linux", "kernel", "news", "patches", "merge", "window", "open-source", "other"]
source: https://www.phoronix.com/linux/Linux+Kernel
source_anchor: ""
source_lines: [1, 105]
section: "Linux_Kernel"
---
A patch that is queued up ahead of the Linux 7.5 merge window allows for optionally disabling /proc/kcore access at boot time. This can allow for better system security though will remain enabled by default for kernel builds with the typical CONFIG_PROC_KCORE option.

# Linux Kernel News Archives

*4,374 Linux Kernel open-source and Linux related news articles on Phoronix since 2006.*

The out-of-tree Multikernel Linux patches for allowing multiple independent kernel instances to run on the same physical machine is out with a new version.

Linux kernel patches were posted tonight for introducing RBRD, the RAM-backed block driver written in the Rust programming language.

A request for comments patch series from a NVIDIA engineer would allow Linux to expose a new sysfs control so user-space can express preferred CPU cores. This could be tied into user-space solutions like Thermald for preferring certain CPU cores over the others based on the CPU core temperatures or other external factors.

The third weekly release candidate of Linux 7.4 is now available for testing.

A new tool expected to be merged to the Linux 7.5 source tree provides a new exercise for the kernel and in the process bringing down the system.

Sent out today was the drm-misc-next pull request for the week of new material to queue in DRM-Next until the Linux 7.5 merge window gets underway in late December. Most notable this week is the introduction of the new GlandaGPU driver.

Linux memory management expert Lorenzo Stoakes at Arm who has been contributing to the kernel since 2014 is stepping away from some reviewership roles within the Linux kernel due to all of the noise from the flood of kernel patches resulting fro AI/LLMs.

The Linux kernel's ChaCha symmetric stream cipher code has long had an AVX-512 optimized code path but it was catering to the early generations of Intel CPUs with AVX-512 (Skylake) that suffered from down-clocking under use. But now slated for arrival with Linux 7.5 is tuning the code to perform better on today's AVX-512 Intel and AMD processors that don't suffer from this limitation. In turn the ChaCha performance is as much as 99% faster with current-generation AMD Zen 5 processors.

Linus Torvalds just released the second weekly release candidate of Linux 7.4 with some notable bug fixes and other changes routed in past the merge window. It's another big one in the AI era.

Even before the Linux v7.4 merge window was over, there were already two pull requests of drm-misc-next material targeting for what will be Linux 7.5. This week already saw the third drm-misc-next pull request of more feature patches targeting this next kernel version.

This week's batch of x86 fixes have been sent out ahead of the Linux 7.4-rc2 kernel releasing later today.

Kmorph aims to provide reboot-less kernel crash recovery for Linux systems and can yield as little as ~2 second downtime for recovering the Nginx web server or other important services in the case of a kernel crash.

With the Linux kernel having phased out i486 CPU support and dropping old AMD Elan SoC code along with removing other obsolete and stagnate 32-bit x86 hardware support, some x86 32-bit clean-ups are proceeding along with being able to make the kernel depend unconditionally now on some features.

FEX 2611 is out for running Linux (and Wine) x86/x86_64 games/applications on AArch64 Linux. FEX 2611 brings JIT improvements while also waiting for the Linux kernel to align with the AArch64 specification and Windows on ARM and macOS Apple Silicon behavior around a particular register's usage.

Linux kernel developers are looking at dropping the RT_GROUP_SCHED feature that is used for group scheduling of real-time tasks using cgroups.

With Linux 7.4 the patches to speed-up the kernel build process began to land. This is the work that's yielded extremely fast incremental builds and noop builds. The patch series in full is also allowing kernel build times approaching just ten seconds with enough CPU cores. In today's article is a look at the Linux 7.3 vs. 7.4-rc1 build time performance on the Framework Laptop 13 Pro powered by Intel Panther Lake.

New to the Linux 7.4 kernel cycle is the Core MM tree as the route for herding some of the core memory management Linux kernel patches to the mainline kernel rather than it all going through Andrew Morton's "MM" tree. As the second pull request from this new kernel tree, a number of fixes were sent out today to address "nasty long-standing bugs" within the kernel.

While the Linux 7.4 kernel will be released in the 2026 calendar year in the last week or two of December if the schedule holds, it turns out Linux 7.3 is likely to be this year's Long Term Support kernel version.

For what will likely be merged to the Linux 7.5 kernel, the mainline kernel is preparing to drop its last optimization its been carrying for far outdated MMX processors. This affects the likes of Intel Pentium MMX and Pentium II processors running modern software with up-to-date kernels when Linux 7.5 is released in 2027.

Even with Linus Torvalds having been on holiday, Linux 7.4-rc1 is out today on-schedule for capping off the Linux 7.4 merge window.

After writing this morning with some stats about the size of the Linux 7.4 merge window and the kernel's source tree approaching 42 million lines, I was curious to run some numbers for putting the size of the merge window into perspective against the major kernel releases of the past 15 years. This shows the definite recent uptick due to AI/LLM activity.

Merged this weekend to Linux 7.4 Git were all of the perf subsystem tooling updates. Most notable to these tooling updates for dealing with performance counters and more on Linux is an overhaul to the Python code.

Linux 7.3 started out just under 41 million lines in total in the kernel source tree. In the end the Linux v7.3 tag came in at 41,019,555 lines as measured by cloc while now with Linux 7.4 we are inching ever closer to 42 million lines. On a Git commit basis, the Linux 7.4 merge window has been the busiest on record.

A new feature merged last week for Linux 7.4 to help reduce AI/LLM generated noise from invalid/irrelevant testing configurations is now reverted due to a "crazy beast" in the VFIO subsystem.

Since last week drm-misc-next pull requests have already been sent out to DRM-Next to begin staging new feature code for what's intended for the Linux 7.5 kernel cycle kicking off at the end of the year.

Merged to the kernel all the way back in Linux 2.6.26 in 2008 was KGDB for improving the kernel debugging experience. KGDB works in conjunction with the GNU Debugger (GDB) to debug the kernel in a similar fashion to user-space debugging. KGDB requires the use of a second computer connected via serial or network cable. KGDB isn't widely used in recent years and a patch was floated today to remove it from the Linux kernel.

An interesting new open-source project born out of university research is Beeper to provide application-layer parsing within the in-kernel eBPF. With an HTTP parser implemented by Beeper in eBPF and providing an in-kernel HTTP cache, the throughput for web servers could be improved by 3x at high rates.

Core memory management patches to the Linux kernel have typically been routed to mainline via Andrew Morton's "MM" branch, which is where also a variety of broader memory management related patches are also queued like all the ongoing work to DAMON/DAMOS. Kernel maintainers have now agreed to a new "Core MM" tree as the home for all of the core memory management changes moving forward.

Merged for Linux 7.4 Git and marked for back-porting to Linux 7.2/7.3 stable is a regression fix affecting at least the AMD Ryzen AI 9 HX 370 "Strix Point" laptop SoC when making use of Cache Aware Scheduling.

Linus Torvalds merged the "char/misc" changes for the Linux 7.4 kernel driver on Saturday. Most notable this cycle for char/misc is what's being removed: Google's C-written Binder driver has left the kernel with the Rust-based modern Binder driver ready to take for the role of being used by all future Android devices.

Sent out today for the Linux 7.4 kernel were a set of memblock improvements. Most notable is a performance optimization within mm/memblock.c that implements the kernel's early boot-time physical memory allocator that is used prior to the bring-up of the full page allocator.

Andrew Morton sent out the big set of memory management "MM" related patches for the Linux 7.4 merge window. This cycle it topped out at 539 total patches and a reviewed rate of 76%.

The slab pull request was just merged for Linux 7.4. For end users nothing too incredibly exciting but there is one change that does yield a 24% throughput improvement in a benchmark exercising the kernel's memory management subsystem.

The power management subsystem updates were merged this week for Linux 7.4 along with the related ACPI and thermal control updates too.

Adding to all the exciting performance optimizations and other improvements for Linux 7.4 is an exciting workqueue improvement. It's not too often getting to talk about "10 to 100 times faster" software improvements.

The scheduler updates have been merged for the in-development Linux 7.4 kernel to deliver several nice scheduling enhancements.

Linux user-space for several years had shadow stack support for hardware-enforced control-flow enforcement in defending against return oriented programming (ROP) attacks. Intel Tigerlake and newer CPUs or AMD Zen 3 and newer have the necessary hardware shadow stacks support. Shadow stacks have been effective for helping user-space software fend ROP attacks while now there is new work toward bringing shadow stack support for kernel space on Linux.

There are notable patches by Arm engineer Lorenzo Stoakes that have begun landing in Linux 7.4 for speeding up the Linux kernel build process. This work can provide some significant reductions in the Linux kernel build time.

Linus Torvalds continues with his recent uptick in kernel patches himself, assisted in part by AI with Claude Code. Merged today was removing the historical and unsafe __get_user() and __put_user() kernel functions.

The latest Linux kernel cryptographic subsystem performance work carried out by Google engineer Eric Biggers is migrating x86/x86_64 and RISC-V optimized implementations of different AES modes into the lib/crypto kernel crypto library to enjoy faster performance there.

Of the many VFS changes to be merged for Linux 7.4 is now closing files synchrously for close_range, exec, and exit calls. In turn when needing to close many file descriptors this provides for much lower latency than the status quo.

Kernfs as the functions/frameworks used for creating pseudo file-systems on Linux like sysfs and configfs have seen some nice scalability work carried out by Meta. After Meta engineers discovered some system daemons getting stuck for minutes on their massive fleet of servers, they uncovered some scalability bottlenecks within Kernfs that now look to be resolved for Linux 7.4.

Added to the Linux kernel last year was Coredump support via Unix domain sockets as an alternative to the pipe-based Coredumps. With code now merged for Linux 7.4, the core dumps via sockets has the potential of being much smaller thanks to sparse support. In extreme cases they can be 1% of the size without sparse support.

Linux 7.4 is seeing a new emulation function added for cmpxchg() for compare and exchange. This is being done so that two-byte cmpxchg() can be used in common Linux kernel code that is already natively supported on newer CPU architectures. The pull request notes though that some old systems might no longer be supported by the Linux kernel moving ahead.

Among the early pull requests sent out in advance of the Linux 7.4 kernel merge window opening were the driver core updates. Standing out here is the new TAINT_FORCED_BIND for dealing with the recent major fuzzing "attack" by AI agents/bots testing impractical driver/hardware combinations.

The Linux 7.3 kernel is out today for an on-time release of this next stable kernel version.

The Linux 7.3 kernel is expected to be released as stable later today and with that version there are many exciting changes and improvements. However, the Linux 7.4 kernel merge window will then open up and there are even more exciting changes expected to be merged for this follow-on kernel cycle. Here is a preview of some of the many exciting feature changes expected for Linux 7.4.

Among the flurry of last minute patch activity ahead of tomorrow's presumed Linux 7.3 stable kernel release is a fix for addressing some rare hangs at boot time. The reports varied but ultimately tracked down to a kernel change made two years ago.

The Linux 7.3 kernel is slated to be officially released on Sunday, 18 October, barring any last minute issues from coming about. With that said, here's a reminder about some of the most exciting features to look forward to in this next kernel version.

4374 Linux Kernel news articles published on Phoronix.
