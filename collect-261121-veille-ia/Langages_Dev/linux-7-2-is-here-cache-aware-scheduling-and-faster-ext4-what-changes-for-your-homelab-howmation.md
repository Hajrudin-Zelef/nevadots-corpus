---
id: collect-261121-veille-ia/langages-dev/linux-7-2-is-here-cache-aware-scheduling-and-faster-ext4-what-changes-for-your-homelab-howmation
title: "Linux 7.2 is here, cache-aware scheduling and faster ext4, what changes for your homelab - Howmation"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["linux", "same", "scheduling", "cache-aware", "release", "threads", "cache", "changes"]
source: https://www.howmation.com/en_US/blog/article/linux-7-2-cache-aware-scheduling-faster-ext4-what-changes-for-your-homelab
source_anchor: ""
source_lines: [1, 93]
section: "Langages_Dev"
---
## Introduction: Linux 7.2 is out, and your server is affected

Your home is full of Linux. The mini PC running Home Assistant, the NAS storing your backups, the server hosting your services, often even your ISP’s router box. All these devices share the same brain, the **Linux kernel**, the program that connects your applications to the hardware.

Since August 16, 2026, that brain has a new version. **Linux 7.2** was released at the end of a nine-week development cycle, and for once the changes aren’t just about datacenter racks. Task scheduling gets smarter on multi-core processors, ext4 and Btrfs gain performance, containers shut down much faster, and a new protocol lets you transfer data between two machines over a simple USB4 cable.

My goal here is to separate the signal from the noise. Many articles simply rehash the list of new features without asking what they actually change for a machine running 24/7 in a closet. So I’m giving you the numbers with their sources, the incident that nearly cost NAS users their data, and the answer to the only question that matters: should you rush to upgrade? No. But do read the section on encryption anyway.

## An on-time release, with a last-minute setback

The stable version landed on Sunday, August 16, exactly within the announced window. Linus Torvalds published it himself, acknowledging that this final week had again been busier than he would have liked, before adding that if he pushed back a release for that reason, we’d never get any:

« We’d probably never have a release at all »

This end of cycle came with a significant setback. The new **DRM “fair”** GPU scheduler, presented for months as one of the headline features of 7.2, was pulled at the last moment in favor of the old FIFO, because of performance regressions observed on certain AMD cards. In practical terms: if you read an article written before the release, you’ll still see it listed among the new features. It isn’t there, and fixes are already on their way to bring it back later.

Statistics-wise, the cycle broke a record. LWN counted **2,138 contributors** during the merge window alone, which is very likely a record for a merge window, including 404 newcomers, for 13,412 non-merge commits. Among them, 706 commits, roughly **5%**, carry an “Assisted-by” tag indicating the use of an AI tool. That’s a statistic, not a judgment, but it sheds light on this “new normal” that Linus endures more than he celebrates.

## Cache-aware scheduling: the real centerpiece of this release

The kernel component that decides which thread runs on which core, and when. It sits at the heart of your machine’s responsiveness.

Until now, it distributed threads by balancing load, without checking whether those threads shared memory. The result: two threads from the same process could land on distant cores, each fetching the same data the other had just loaded. That’s **cache bouncing**, and it’s expensive in performance terms.

Linux 7.2 makes the scheduler **cache-aware**: it recognizes threads belonging to the same process and keeps them on cores that share a last-level cache. With guardrails, because a greedy process must not monopolize a cache domain at the expense of others. The scheduler stops grouping threads if the process exceeds **25%** of CPU time, or **33%** of a cache domain’s load.

Imagine a kitchen brigade where each cook works in a separate room, with their own fridge. As soon as they need the same ingredient, you have to buy it again and store it twice. Cache-aware scheduling means putting the brigade back in the same kitchen, around the same fridge. Less back and forth, less waste.

Who benefits? AMD and Intel processors with multiple cache domains — in other words, most of the recent x86 fleet, including the Ryzen-based mini PCs many people use as servers. The workloads concerned are multi-threaded ones: a compilation, several virtual machines, a stack of containers.

And contrary to what you sometimes read, public measurements do exist. Phoronix had tested the feature before it was merged, on EPYC Turin, Xeon 6, and Zen 5, with clear gains on PostgreSQL, Valkey, and networking. Since then, the site compared 7.1 and 7.2 on a 96-core Threadripper workstation: I/O up, the poll call noticeably faster, and a few regressions along the way. I’d rather give you that nuance than a single percentage that would say nothing about your machine.

## ext4 and Btrfs: the homelab’s filesystems take center stage

A word about where the following figures come from, because they’re not all equal. The percentages related to filesystems come from benchmarks published in the commits by the developers themselves, on their hardware and their test workloads: they indicate a direction, not a promise. Phoronix’s independent measurements confirm the trend on the I/O side, with more mixed results depending on the tests.

### ext4, the system disk of Ubuntu and Debian

**ext4** is the default filesystem of Ubuntu and Debian, and therefore of most of your home servers. Three changes this time. The first fits in two relocated lines of code: removing a now-useless memory zeroing in the iomap layer yields about **5% more IOPS** on small 4K random reads on NVMe in io_uring polling mode, a fix shared with XFS. The second concerns directory hash computation, which now processes data in four-byte blocks instead of byte by byte, roughly doubling the speed of that function on long filenames. The third rewrites the fast commit to eliminate lock contention and deadlocks under heavy load.

### Btrfs, the filesystem of NAS devices

**Btrfs** is the default on Fedora and openSUSE, and on many consumer NAS devices. The announcements are more spectacular: large folios — memory management in bigger blocks — move from experimental to enabled by default after maturing since Linux 6.17, where they promised single-digit gains on real-world workloads. Direct I/O is no longer serialized, with **+59%** write throughput announced, and sequential writes gain 15% thanks to a cap on writeback request size that avoids latency spikes. Huge folios up to 2 MB make their debut, but remain experimental.

And then there’s the most instructive episode of the whole cycle, one that few articles mention. During the merge window, Btrfs removed its “COW fixup” mechanism, which detects pages modified behind the filesystem’s back. The developers thought the kernel’s memory management had made this safeguard unnecessary. In reality, its absence could cause **silent data loss**. Six hundred lines were hastily restored in rc7, in early August, ten days before the release. The final kernel is sound, but keep this story in mind the next time someone tells you a version is ready on release day.

### exFAT, MGLRU, and the gains you don’t see

exFAT, the format of your SD cards and USB sticks, also switches to iomap, with up to 87% faster write speed under heavy load according to the commit’s measurements. Handy if your backups land on removable media. MGLRU, the mechanism that decides what the kernel frees when memory runs short, claims up to 30% improvement on MongoDB with YCSB on NVMe storage, and up to 100% when storage is slow — which concerns quite a few homelabs still equipped with mechanical drives. As a bonus, fewer unexpected OOM kills. Unix pipes, those conduits you use whenever you chain two commands, gain 6 to 28% in performance and lose 5 to 22% in write latency. Finally, /proc/filesystems reads up to 140% faster, and up to 444% when twenty processes query it at the same time: your monitoring scripts will appreciate it.

### My next articles, straight to your inbox

Once a week, I send you my new tutorials, the news that matters, my tests and the real deals I spot. Nothing else, I promise.

## Containers, encryption, and hardware: what changes day to day

### Containers finally shut down quickly

If you run Docker or LXC, this one is for you. A series of fixes signed by an Alibaba engineer corrects a race condition when a container stops, the one that produces the “VFS: Busy inodes after unmount” message and could lead to a use-after-free memory access. Along the way, an unnecessary global serialization was removed. On a 16-vCPU test machine under heavy write load, median unmount latency drops from roughly **92 to 138 milliseconds** to 5 to 8 milliseconds. The race fix will be backported to stable kernels: you’ll likely benefit from it before even upgrading to 7.2.

### LUKS and the deprecation of AF_ALG

Here’s the point that deserves your attention if your disks are encrypted, and it appears in no feature list because it isn’t one. Linux 7.2 deprecates **AF_ALG**, the interface that gave programs direct access to the kernel’s cryptography, deemed to carry too large an attack surface. Yet cryptsetup, the tool that manages **LUKS** volumes on pretty much every encrypted Linux server, relied on it heavily.

Cryptsetup 2.8.7, released on July 21, 2026, switches to a user-space encryption library, with a fallback to a temporary dm-crypt mapping for LUKS keyslots. Not everything follows: Adiantum encryption exists only in the kernel, Serpent and Twofish in XTS mode are missing from several user libraries, and the cryptsetup benchmark command is no longer available without AF_ALG. Check the algorithm of your encrypted volumes before any migration, and let your distribution ship the right version of cryptsetup along with the kernel.

### USB4STREAM and newly supported hardware

**USB4STREAM** is the new feature that made me smile the most. Developed by Intel, this protocol lets you exchange data directly between two machines connected by a USB4 or Thunderbolt cable, with no network or switch. Each machine exposes /dev/tbstreamX devices, usable by any program that knows how to read and write a file, cat and dd included, and it all coexists with the existing Thunderbolt networking. Direct backup between two servers, moving a large volume without saturating the network, sharing a webcam between two machines: it’s exactly the kind of tool for which you find ten uses in a single evening.

On the hardware side, take stock of your setup. Linux 7.2 brings the Realtek RTL8159 ASIC for 10 Gbit/s USB network adapters, the MediaTek MT7927 (Filogic 380) for full Wi-Fi 7 advertised beyond 2 Gbit/s on the 6 GHz band plus Bluetooth 5.4, temperature and fan monitoring on more ASUS and ASRock motherboards, and a driver for the ARCTIC fan controller — a product that isn’t even on sale yet. Booting on Apple M3 Macs now works, but console-only: encouraging for the Asahi project, not yet enough to turn your Mac into a server.

This release also does some spring cleaning in the attic. More than 13,000 lines of i486 processor emulation disappear, as does the driver for the Hercules graphics card that will have served for forty years, the AppleTalk protocol, and the ARCnet drivers for ISA and PCMCIA. The kernel exceeds 43 million lines of code, so every bit of spring cleaning counts.

## Upgrade or hold off: the verdict by use case

Nothing in Linux 7.2 is risky for a homelab. But nothing is urgent either, and the Btrfs episode in rc7 is a good reminder of what plays out behind the scenes in the final weeks of a cycle. The simplest approach is to let your distribution do the work: kernel updates arrive through the package manager, tested and accompanied by security fixes.

The **multi-core server**, the one running VMs, containers, or compilations, has the most to gain, between cache-aware scheduling and unmount latency. The **NAS** will benefit from the Btrfs gains, but via the vendor’s update, and that’s just fine: you don’t update a NAS on kernel release day. Desktop use won’t notice anything obvious. And if your volumes are encrypted, whatever the profile, it’s **cryptsetup** that should guide your schedule, not Btrfs percentages.

Before you rejoice about write gains on your NAS, make sure what lands there is properly backed up elsewhere.

To give you a concrete example: the mini PC I use as a video recording server, the one from my series on local video surveillance, runs Debian. It will get the 7.2 kernel in the next update cycle without me lifting a finger, exactly like my self-hosted services like Vaultwarden. If you’re itching to test it, a disposable virtual machine costs less than a weekend of data recovery, and I happen to have an article on RAM prices for your homelab if you’re wondering how to give it some room.

As for the release calendar, the situation is ironic. Canonical was targeting **Linux 7.2** for Ubuntu 26.10, but its policy is to ship the very latest in-development version at the time of the kernel freeze, set for August 20. Since 7.2 was released before that date, the 7.3 merge window is already open, which technically makes it “the latest in-development version”. As of this writing, on August 17, 2026, Canonical has not made a decision. Ubuntu 26.04 users will get the result via the HWE backport, expected in early 2027 with the 26.04.2 point release. Fedora 45 is also due to ship 7.2.

Your server is going to get a little faster, a little smarter, and you won’t have to do anything. That’s exactly the kind of update I like.

## Conclusion: let your distribution handle it

**Linux 7.2** is a very good release for the homelab, without being a release that turns everything upside down. **Cache-aware scheduling** is the most promising change for multi-core machines, the drop in container unmount latency is the most immediately visible gain if you live in Docker, the ext4 and Btrfs announcements are heading in the right direction for NAS devices and backups, and USB4STREAM is the toy I’ll rush to test. None of these gains justifies rushing your upgrade.

My recommendation is simple, and it boils down to one sentence: let your distribution do the work, and read the note about AF_ALG if your disks are encrypted. That’s the only point in this release that can break something on your end, and it won’t resolve itself if you compile your kernel by hand. To dig deeper, the announcement is on LWN, the consumer-friendly summary at OMG! Ubuntu, and the independent benchmarks at Phoronix.
