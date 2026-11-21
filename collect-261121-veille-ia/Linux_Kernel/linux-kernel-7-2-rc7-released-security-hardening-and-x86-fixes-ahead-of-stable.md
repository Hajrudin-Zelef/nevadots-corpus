---
id: collect-261121-veille-ia/linux-kernel/linux-kernel-7-2-rc7-released-security-hardening-and-x86-fixes-ahead-of-stable
title: "Linux Kernel 7.2-rc7 Released: Security Hardening and x86 Fixes Ahead of Stable"
domain: linux-kernel
role: reference
task: article
actors: []
dates: []
keywords: ["fixes", "kernel", "hardening", "stable", "driver", "linux", "security", "window"]
source: https://www.linuxcompatible.org/story/linux-kernel-72rc7-released-security-hardening-and-x86-fixes-ahead-of-stable/
source_anchor: ""
source_lines: [1, 44]
section: "Linux_Kernel"
---
### Linux Kernel 7.2-rc7 Lands with Heavy Security Hardening and x86 Mitigation Fixes

**The seventh release candidate delivers Safe-RET improvements, SELinux policy validation overhauls, and a round of GPU and networking patches ahead of the stable release.**

Linux Kernel 7.2-rc7 dropped on August 9, 2026, and if you're watching the development branch, the message is clear. Secure the edges. Linus Torvalds shipped the seventh release candidate as a bug-fix-only window, but the patchset covers a lot of ground. We're looking at 412 files changed, nearly 400 non-merge commits, and a pronounced push to harden subsystems that have historically been soft targets.

At rc7, the merge window is long closed. You're in the refinement stage now. Distributions are already compiling their test images, and Greg Kroah-Hartman will likely branch the -stable tree within days. After the final 7.2 drop, the 7.3 merge window opens immediately. If you're running anything beyond 7.2-rc6, this one's worth testing before you roll to stable.

#### The security push and the x86 mitigation

The headline change is x86 microarchitecture hardening. Commit 7e7f81cf6f tackles the Speculative Return Stack Overflow mitigation, otherwise known as SRSO. The kernel already uses a "saferet" instruction sequence to dodge return-address prediction attacks, but interrupt injection during that window could still leak control flow. This patch emulates the saferet sequence and keeps the CPU from ever executing a direct RET during the vulnerable window. Affects all x86_64 processors running the SRSO path.

SELinux got a thorough validation overhaul too. Ten new checks now run at policy load time, rejecting unclaimed class values, undefined booleans, and malformed permissions before they ever touch the running system. One of the patches also fixes an ~89 to 94 percent throughput regression in System V IPC message queue operations that the previous isvalid checks accidentally introduced. Head here to grab the exact commit list if you're auditing your policy files.

On the networking side, the Open vSwitch peer driver added 204 lines of hardening across socket handling and Netlink generation. IPSet's hash implementation gained 304 lines of bounds and overflow checks. Race conditions in MPTCP fastopen and memory teardown paths got squashed. It's the kind of quiet, unglamorous work that keeps production stacks from quietly panicking at 3 AM.

#### GPU, networking, and the rest

GPU folks aren't left out. AMD's DPG mode JPEG queue reset failures and MMHUB0 pasid TLB flush checks are addressed for newer GMC12.1 and JPEG v4.0.5 through v5.3.0 silicon. Intel's Xe driver finally aligns its TTM beneficial order to 9, matching large GPU page sizes for proper system memory reclaim. VMware's vmwgfx virtual GPU driver picked up out-of-bounds DMA validation and NULL deref prevention from Zack Rusin.

Virtualization got a solid round of fixes across x86 KVM's shadow page metadata and s390 KVM's paravirtualization cleanup ordering, including a fix for leaking PGM_ADDRESSING back to userspace. The Rust-based Android binder driver also landed its first meaningful stability patch, stopping the driver from querying the current thread on every ioctl call. You'll want to watch that one. Rust driver maturity moves fast, and small threading fixes like this compound quickly.

The ftrace ring buffer got a crash fix when ERR_PTR accidentally reaches kthread_stop(). USB Type-C port controller interfaces finally get proper teardown ordering to prevent use-after-free during unregistration. Btrfs picked up subpage support improvements, and XFS scrub/repair gained aggressive header validation and realtime bitmap recovery logic. Thunderbolt drivers got consolidated for stability, and three new documentation files landed. The coding-assistants guide is a pretty telling development. It's the project quietly admitting that AI-assisted contributions are here to stay, and setting guardrails before the floodgates open.

#### Linux kernel 7.2-rc7 released

Linux kernel version 7.2-rc7 is now available:


Full source:    https://git.kernel.org/torvalds/t/linux-7.2-rc7.tar.gz

Patch:          https://git.kernel.org/torvalds/p/v7.2-rc7/v7.1


You can view the summary of the changes at the following URL:

https://git.kernel.org/torvalds/ds/v7.2-rc7/v7.2-rc6


#### What comes next

At 7.2-rc7, the window is narrow. Any critical regressions left will likely vanish in the final -rc cycles. Greg Kroah-Hartman will branch the stable tree soon, and distros will start shipping. If you're tracking the release, grab the source from Torvalds' tree and watch the stable queue. The final 7.2 drop is close, and 7.3-rc1 will follow right after.

Longterm branches continue to run alongside the new release. 7.1.8, 6.18.44, 6.12.103, and 6.6.151 all shipped alongside rc7, meaning servers and embedded devices on older kernels aren't being left behind while the mainline moves forward.
