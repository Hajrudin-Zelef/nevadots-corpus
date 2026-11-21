---
id: collect-261121-veille-ia/linux-kernel/linux-kernel-7-4-rc3-released-with-usual-noise-and-one-big-iomap-change
title: "Linux Kernel 7.4-rc3 Released With Usual Noise and One Big iomap Change"
domain: linux-kernel
role: reference
task: article
actors: []
dates: []
keywords: ["kernel", "patches", "iomap", "linux", "torvalds", "fixes", "released", "usual"]
source: https://www.linuxcompatible.org/story/linux-kernel-74rc3-released-with-usual-noise-and-one-big-iomap-change/
source_anchor: ""
source_lines: [1, 39]
section: "Linux_Kernel"
---
#### Linux Kernel 7.4-rc3 Released: Torvalds Calls It "the Usual Noise All Over"

**Linus Torvalds has announced the third release candidate for Kernel 7.4. No fireworks this week. No splashy new features. Just bug fixes, a couple of GPU and networking drivers, and one line from the project's lead that captures the mood: "the usual noise all over."**

The 7.4 development run is the busiest the kernel has ever seen. When the first release candidate landed on 1 November, it closed the largest merge window on record with 19,671 commits. A typical window runs around 15,000. That blows past the 16,267 commits from the 7.3 cycle and the 14,395 from 7.2. If things hold, the full 7.2 through 7.3 stretch will cross 20,260 commits, the first time a complete cycle breaks that barrier.

The source tree is now nearing 42 million lines of code. A large portion of that is a single massive AMD DCN6 GPU register-header contribution, so the total isn't entirely a measure of organic growth. It's a big number, though the context makes it a bit less impressive.

And then there's the AI angle. In the rc1 announcement, Torvalds actually framed the surge of AI-generated patches positively, saying he'd rather have AI-triggered fixes arrive during the merge window as part of normal work than flood the stabilization phase as late emergency repairs. Still, many of those AI-drafted changes get rewritten before they land.

There's one data point worth sitting with. BPF maintainer Alexei Starovoitov reported that in the BPF subsystem, developers were resolving AI-triggered bugs faster than new reports arrived. That would be the first sign AI-assisted debugging yields net stability instead of endless churn. Torvalds warned most other subsystems haven't reached that point yet.

#### iomap leads the change list, networking brings the volume

Of everything in rc3, Torvalds singled out a calling-convention rework of iomap as large enough to move the filesystems category in the diffstat. Iomap is the kernel's shared layer that lets filesystems query storage by byte range. The old model relied on begin/end calls. Joanne Koong of Meta replaced that with an in-iter approach where callers advance via iomap_iter_next(). She submitted five patches this week as part of the wider multi-cycle effort.

The change ripples everywhere. XFS, btrfs, ext4, f2fs, gfs2, ntfs3, ntfs, erofs, zonefs, ext2, and FUSE's ext_map all had to convert. Torvalds called it "fairly trivial but large." Same logic, just touching a huge number of files.

Networking carries the real volume. Paulo Alcantara shipped 19 patches for the SMB client, covering writeback through O_DIRECT handles and retry backoff on reconnects. Avi Weiss delivered 19 for NFS and SUNRPC, with Jeff Layton adding 4. One fix stands out: Jérémy Jean closed an authentication-bypass hole in TCP auth options that let the option be circumvented.

Graphics drivers weren't idle either. Intel's drm/xe, AMD's amdgpu and amkfd, and Intel's i915 all picked up substantial patches. VRAM-page reuse prevention, TLB-invalidation fence handling, and Type-C port lock ordering all landed this week.

The rest of the tree got its share of pain relief too. Google's Masami Hiramatsu submitted 19 patches to kprobes and tracing. Meta's Gregory Price pushed 11 memory-management fixes. Bartosz Golaszewski turned in 12 GPIO fixes. And WiFi saw a big batch as well, led by Zhao Li's 15 patches to mac80211.

A few patches this week bite in a security sense as well. ksmbd got a coordinated set of ACL fixes so the kernel's SMB server evaluates access requests correctly. NTFS gained checks to bound index-entry name lengths and block reads outside the index allocation block. The KEYS subsystem hardened its bounds, and caam now rejects overlong RSA CRT parameters. Keep in mind that these are hardening fixes in a candidate, not a formal CVE advisory.

#### Linux kernel 7.4-rc3 released

Linux kernel version 7.4-rc3 is now available:


Full source:    https://git.kernel.org/torvalds/t/linux-7.4-rc3.tar.gz

Patch:          https://git.kernel.org/torvalds/p/v7.4-rc3/v7.3


You can view the summary of the changes at the following URL:

https://git.kernel.org/torvalds/ds/v7.4-rc3/v7.4-rc2
