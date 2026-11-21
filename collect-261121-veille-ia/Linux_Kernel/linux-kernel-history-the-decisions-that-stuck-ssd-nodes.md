---
id: collect-261121-veille-ia/linux-kernel/linux-kernel-history-the-decisions-that-stuck-ssd-nodes
title: "Linux kernel history: the decisions that stuck · SSD Nodes"
domain: linux-kernel
role: reference
task: article
actors: []
dates: ["2026-11-17"]
keywords: ["kernel", "linux", "version", "history", "release", "decisions", "licence", "first"]
source: https://www.ssdnodes.com/learn/linux-kernel-history-timeline
source_anchor: ""
source_lines: [1, 154]
section: "Linux_Kernel"
---
## The short version of Linux kernel history

Linux kernel history runs from version 0.01 in September 1991 to the 7.x series booting on servers today. The release list is the least interesting part of it. A small number of decisions set the shape of the thing, and each one still has a consequence on a machine you rent this afternoon. Why a new kernel was needed in 1991 at all belongs to the longer story of Unix, AT&T licensing and the lawsuit that stalled BSD.

Dates and version numbers here come from kernel.org and the release history it publishes. The current state, as of 17 November 2026: 7.0 arrived on 12 April 2026, 7.1 on 14 June 2026, 7.2 in mid-August 2026, and 7.3 on 18 October 2026. kernel.org lists 7.4-rc3 as mainline and 7.3.4 as the current stable release. The 7.2 branch reached end of life with 7.2.15 on 1 November 2026.

## Why the GPL choice in 1992 still matters

Version 0.01 was published on 17 September 1991 under a licence Torvalds wrote himself. It required that source be distributed, and it added a line that mattered more: "You may not distribute this for a fee, not even 'handling' costs." In 1991 software shipped on floppy disks, and copying and posting floppy disks costs money. That clause made a commercial Linux distribution impossible.

He changed it. The move to the GNU General Public License (GPL) was announced in the 0.12 release notes in January 1992 and took effect on 1 February 1992. Version 0.95, in March 1992, was the first release published under it. Every business later built on Linux rests on that change.

The kernel is GPL version 2 only, and it never moved to version 3. Torvalds refused in 2007, mostly over the anti-tivoisation rule in GPLv3, which requires installation information for modified versions of covered code conveyed with a consumer device, unless neither the supplier nor any third party can install modified code, as with code installed in ROM. He treated locked hardware as the manufacturer's own business. In 2017 kernel developers published the Kernel Enforcement Statement, which borrows one piece of GPLv3 anyway: someone who receives a first notice of a violation from a copyright holder and fixes it within 30 days gets their licence back permanently, instead of losing it at the first breach.

Two consequences follow on a server. The kernel binary you boot carries a right to the matching source, so nobody can hand you a Linux kernel you may not inspect or rebuild. And the copyright notice on the kernel says the licence does not cover user programs that use kernel services by normal system calls, which is why proprietary databases and monitoring agents ship for Linux without breaking anything. A permissive licence creates the opposite pressure, and that difference is worth understanding before you pick a platform: see Linux and FreeBSD as server platforms.

## Why the monolithic kernel won in practice

On 29 January 1992, Andrew Tanenbaum posted a message titled "LINUX is obsolete" to the comp.os.minix newsgroup. He made two claims. Monolithic kernels, where drivers and filesystems run inside one privileged address space, were a design of the 1970s, while microkernels, where those parts run as ordinary processes, were the future. And Linux was welded to the Intel 386, so it would never travel.

The portability claim was answered by porting. When version 1.2 shipped in March 1995, ports to the DEC Alpha, MIPS, SPARC and the Motorola 68000 family were still under way. Version 2.0 in June 1996 was the first stable release to support the 64-bit Alpha alongside the 386, and by early 1997 SPARC had joined them as an integrated port.

The design claim was answered by a compromise. Linux never became a microkernel. It got loadable kernel modules: object files you insert into a running kernel and remove again, so a driver ships separately from the kernel binary.

```
lsmod | head
modinfo virtio_net | head -5
```
`lsmod` lists what is loaded right now. `modinfo` prints the file the module came from and the parameters it accepts. On a virtual server most of the disk and network path is modules, which is why one kernel image boots on hardware it has never met. The kernel only announces new hardware, and a user space daemon decides which module to load and what to name the device, which is how device management came to sit inside the init system and part of why systemd became hard to avoid.

Modules bought that flexibility without the cost the microkernel design carried. Isolating a driver in its own process means paying for a context switch and a message on every call, and in 1992 that cost was large.

The cost Linux kept is the one to plan around: a module runs with full kernel privileges, so a bad module can take the whole machine down instead of one process. Out-of-tree modules are where this bites. A vendor driver that is not in mainline must be rebuilt against every new kernel, which is what DKMS does during an upgrade, and when that build fails the device is simply missing after the reboot.

## Why SMP took fifteen years to finish

Linux 2.0 in June 1996 was the first stable kernel to support symmetric multiprocessing (SMP), meaning more than one CPU running one kernel. The first implementation used a single lock, the big kernel lock (BKL), so only one processor could be inside kernel code at a time. A second CPU therefore helped a workload that computes in user space, and helped very little with a workload of system calls, because those queued behind the same lock.

Removing that lock took fifteen years. The remaining users were converted to fine-grained locking, largely by Arnd Bergmann, and the BKL was deleted in 2.6.39, released on 18 May 2011. The scheduler moved on the same slow timescale: the O(1) scheduler in 2.6.0, the Completely Fair Scheduler (CFS) from 2.6.23 in 2007, and EEVDF, which replaced CFS in 6.6 in October 2023.

That work is why a 4 vCPU plan is unremarkable now. It also marks a limit worth knowing. On a shared virtual server your kernel schedules your threads, and the hypervisor schedules your kernel. Run `top` and read the `%st` field. Steal time is CPU that your kernel was ready to use and the host gave to another guest, so no tuning inside your kernel recovers it.

## Why the 2.6 series changed how the kernel is built

Before 2.6, version numbers came in pairs. An even second number meant a stable series (2.4), an odd one meant development (2.5). 2.4 was released on 4 January 2001 and 2.6 on 17 December 2003, so users waited nearly three years for the next stable series. Distributions could not wait, so they backported. Two vendors both shipping "2.4" shipped kernels thousands of patches apart.

The split was dropped after 2.6. Mainline now opens a merge window of about two weeks, takes new work, then runs release candidates until it goes quiet, and ships every 9 to 10 weeks, the cadence kernel.org still documents. The other half of the model arrived on 4 March 2005 with the first release of the stable tree, a fix-only update to 2.6.11, maintained by Greg Kroah-Hartman and Chris Wright. The stable tree takes fixes and refuses features.

One side effect: the version number stopped being a promise. 3.0, 4.0, 5.0 and 7.0 are not rewrites. Torvalds raises the first number when the second one grows large enough to bother him, which is why 7.0 followed 6.19 in April 2026. What matters for a server is which branch your distribution tracks, and whether that branch still gets fixes.

## How the BitKeeper break produced git in April 2005

From February 2002 the kernel was developed in BitKeeper, a proprietary distributed version control system from Larry McVoy's company BitMover, starting with the 2.5 series. BitMover gave kernel developers a free licence with conditions attached: you could not work on a competing version control tool, and you could not reverse engineer BitKeeper. Many developers disliked building a free kernel with a tool they were not allowed to read.

It broke in April 2005, after Andrew Tridgell demonstrated a program that talked to BitKeeper repositories. BitMover called that reverse engineering and withdrew the free licence. The kernel lost its version control system in the middle of a development cycle.

Work on git started in April 2005. Torvalds announced the break with BitKeeper on 6 April. On 7 April git was self-hosting, which means git's own history was already kept in git. Torvalds announced the first successful git merge on 17 April. In June 2005, git managed the release of 2.6.12. Torvalds handed maintenance to Junio Hamano soon after and went back to the kernel.

The design fell straight out of the problem: thousands of contributors, and maintainers who pull from each other across a network nobody trusts. Every object is named by the hash of its content, so changing one byte of old history changes the name of every commit after it. That is why a clone is evidence rather than a claim. Every deploy pipeline, every configuration repository, the code host most teams push to and the git server you can run yourself grew out of a licensing argument about a kernel.

## What the LTS model promises, and what it does not

Mainline is not what you run. A mainline release is superseded 9 to 10 weeks later. An ordinary stable branch typically keeps receiving fixes for a little more than one development cycle, so it overlaps the next mainline release by a few weeks and then ends, unless it is kept as longterm. Longterm branches, usually written LTS, carry fixes for years, and those are the ones many distributions build on.

2.6.32, released in December 2009, is where the model proved itself. RHEL 6, Debian 6, SUSE Linux Enterprise 11 SP1 and Ubuntu 10.04 LTS all shipped it, and the branch was maintained until March 2016, more than six years after it appeared. Red Hat went further still and carried its 2.6.32-based kernel on its own backports until RHEL 6 left its maintenance phase in 2020, and it still sells selected security and urgent bug fixes for RHEL 6 through a paid Long-Life add-on with yearly terms running to the end of 2030. Reproducing that decade of support for free is the whole reason CentOS existed and Rocky Linux and AlmaLinux replaced it.

The promise has moved more than once. It was two years, then six years for some branches. In 2023 the stable maintainers cut the default back to two years, because backporting into old trees costs maintainer time and old branches get little real testing. On 25 February 2026 Greg Kroah-Hartman published longer projections again, after discussion with the companies that depend on those branches, but new longterm kernels still usually start with a two-year projected end of life.

## The data behind this chart

```
[
  {
    "kernel": "5.10",
    "released": "2020-12-13",
    "eol_projected": "Dec 2026",
    "maintained_for": 6.0
  },
  {
    "kernel": "5.15",
    "released": "2021-10-31",
    "eol_projected": "Dec 2026",
    "maintained_for": 5.1
  },
  {
    "kernel": "6.1",
    "released": "2022-12-11",
    "eol_projected": "Dec 2027",
    "maintained_for": 5.0
  },
  {
    "kernel": "6.6",
    "released": "2023-10-29",
    "eol_projected": "Dec 2027",
    "maintained_for": 4.2
  },
  {
    "kernel": "6.12",
    "released": "2024-11-17",
    "eol_projected": "Dec 2028",
    "maintained_for": 4.1
  },
  {
    "kernel": "6.18",
    "released": "2025-11-30",
    "eol_projected": "Dec 2028",
    "maintained_for": 3.1
  }
]
```
kernel.org lists 6 longterm branches as of November 2026. The oldest, 5.10, will have carried fixes for 6.0 years when it ends in Dec 2026. The newest, 6.18, is projected to run to Dec 2028, which is 3.1 years of fixes.

Read those dates as projections rather than a contract. The 6.6 and 6.12 projections both moved outward in February 2026, and a branch nobody uses can be dropped instead. Your distribution normally makes the choice for you: Debian 13 ships 6.12, and Ubuntu 26.04 LTS ships 7.0. That gap is the practical content of the LTS against interim release question on a server, and it is what really changes underneath you when you upgrade Ubuntu 24.04 to 26.04.

One trap follows from this. `uname -r` on Ubuntu 24.04 prints something like `6.8.0-51-generic`. That is an upstream base plus the distribution's own backports, so the number tells you where the branch started and not which fixes are in it. Scanners that judge a kernel by its version string raise false alarms against distribution kernels for exactly this reason.

## What the kernel is arguing about right now

Two arguments are live, and both are about who does the work.

Rust landed as infrastructure in 6.1 in December 2022. In 7.0 the experimental label came off, so the kernel's core languages are C, assembly and Rust. Building with a stable Rust compiler was already supported in 6.1: its Makefile sets `RUSTC_BOOTSTRAP=1` to enable unstable features in stable compilers. The dispute is about maintenance. A C maintainer who changes an interface can break Rust bindings they do not read, and the argument is over whose job it is to repair them.

The second argument is AI contribution. Sasha Levin proposed a policy in July 2025, after a rising volume of machine-assisted patches reached the lists. The document was committed on 6 January 2026 and now sits in the kernel's own process documentation at docs.kernel.org/process/coding-assistants.html. An AI agent must not add a `Signed-off-by` tag, because that line certifies the Developer Certificate of Origin (DCO) and only a person can certify it. The current kernel documentation no longer requires an `Assisted-by` tag and instead refers contributors to the tool-generated-content guidelines. Contributors are still expected to explain tool use in cover letters and changelogs. Generated code must be compatible with GPL-2.0-only. The human who sends the patch reviews it and carries the responsibility for it.

The pressure behind the policy is review time. Generating a patch takes seconds, and reviewing one takes a maintainer's afternoon. A tag does not fix that imbalance. Human sign-off preserves provenance: the history keeps recording who signed for each change, which is the property the DCO was introduced to protect in 2004.

## What this history means for the server you rent

- The licence is why you can read and rebuild the kernel your provider boots, and why proprietary software still runs on it.
- The monolithic design is why one driver bug can bring down the whole machine, and why an out-of-tree module has to be rebuilt on every kernel upgrade.
- The release model is why the version number tells you little, while the branch and its end-of-life date tell you almost everything.
- The virtualisation type decides what you may do at all: on KVM you boot your own kernel and load modules, while on container virtualisation that shares the host kernel, `uname -r` shows the host's version, and permission to use`modprobe` or change sysctls depends on the privileges and configuration the host grants.

## FAQ

### Why is the Linux kernel still GPLv2 and not GPLv3?

Torvalds decided against GPLv3 in 2007, mainly over its anti-tivoisation requirement, which requires installation information for modified versions of covered code conveyed with a consumer device, unless neither the supplier nor any third party can install modified code, as with code installed in ROM. He treats locked hardware as the manufacturer's business. Relicensing is close to impossible in practice as well, because copyright in the kernel is held by thousands of contributors and there is no assignment agreement to fall back on. The kernel is GPL-2.0-only, so code offered under GPLv3 alone cannot be merged.

### Is the Linux kernel a monolithic kernel or a microkernel?

Monolithic, with loadable modules. Drivers and filesystems run inside the kernel's address space, and `lsmod` shows the ones loaded right now. The effect is speed on one side and blast radius on the other: a faulty module can panic the whole machine, where a microkernel would lose one process. The picture has softened since 1992 through FUSE filesystems in user space and eBPF programs that the kernel verifies before it runs them.

### What is the difference between mainline, stable and longterm kernels?

Mainline is Torvalds' tree, released every 9 to 10 weeks, and new features land there first. Stable takes a mainline release and receives bug fixes for a little more than one development cycle, so a stable branch usually ends a few weeks after the next mainline release, unless it is kept as longterm. Longterm branches keep receiving fixes for years, and many distributions build their kernels on them. kernel.org lists the current longterm branches with a projected end-of-life date for each one.

### Does the Linux kernel accept code written by AI?

Yes, under a policy committed in January 2026. The current kernel documentation no longer requires an `Assisted-by` tag; contributors are expected to explain the origin of tool-generated content in cover letters and changelogs. An AI agent must not add a `Signed-off-by` line, and generated code has to be compatible with GPL-2.0-only. The human submitter signs off, which means they reviewed the patch and take responsibility for it under the Developer Certificate of Origin.

### Which kernel version should I run on a server?

The one your distribution maintains, in almost every case. A distribution kernel is an upstream branch plus backported fixes plus the vendor's testing, and it is what your provider's images and your support arrangements assume. Build a newer mainline kernel when you need a specific driver or feature, and check the end-of-life date of the branch you are moving to before you commit to it.
