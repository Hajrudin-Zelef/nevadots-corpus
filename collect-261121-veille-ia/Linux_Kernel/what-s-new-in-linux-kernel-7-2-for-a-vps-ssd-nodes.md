---
id: collect-261121-veille-ia/linux-kernel/what-s-new-in-linux-kernel-7-2-for-a-vps-ssd-nodes
title: "What's new in Linux kernel 7.2 for a VPS · SSD Nodes"
domain: linux-kernel
role: reference
task: article
actors: []
dates: ["2026-08-16"]
keywords: ["kernel", "cache", "process", "linux", "threads", "memory", "load", "share"]
source: https://www.ssdnodes.com/learn/whats-new-in-linux-kernel-7-2
source_anchor: ""
source_lines: [1, 176]
section: "Linux_Kernel"
---
## What's new in Linux kernel 7.2

Linux kernel 7.2 was released on 16 August 2026, and the change worth your attention is cache aware scheduling, built behind the new `CONFIG_SCHED_CACHE` option. The scheduler now tries to keep the threads of one process on CPUs that share the same last level cache (LLC). The release also changes CPU placement on systems with asymmetric CPU capacities and SMT, preferring fully idle cores and rejecting misfit pulls onto busy SMT siblings.

The rest of 7.2, in one line: a rework of the ext4 fast commit path, improvements to MGLRU (the multi-generational least recently used memory reclaim code), a new `dm-inlinecrypt` device mapper target for inline block device encryption, and the removal of the last `strncpy()` call from the kernel source.

One fact decides whether the headline feature can do anything for you, so it comes first. Cache aware load balancing activates only when a NUMA (non-uniform memory access) node contains more than one LLC. A VPS guest is usually not shown that layout, so on most guests the code is compiled in and never engages. Checking that takes two commands, in "Does a VPS guest see any of this" below.

The original technical sources for this page were the 7.2 changelog and the cache aware scheduling patch series itself, read on 18 August 2026. The debugfs tunables were re-checked on 29 October 2026 against the Linux 7.2 source. The sources are listed near the end so you can check them against what your own kernel does.

## Why the scheduler needed to know about caches

A modern server socket does not have one last level cache. An AMD EPYC package is built from several core complexes, and each complex has its own L3. Recent Intel Xeon parts also split a socket into more than one cache domain. So a single NUMA node can hold four, eight or more separate LLCs, and two threads of the same program can end up in different ones.

That placement costs time. When two threads share a page from different LLCs, each cache keeps its own copy of the line. A write on one side invalidates the copy on the other side, so the next read has to cross the interconnect or go out to main memory. This is cache bouncing. It shows up as cycles spent waiting, not as CPU idle time, which is why it is easy to miss when you are watching a load average.

Before 7.2 the load balancer placed tasks using load, utilisation and idle CPUs. It had no input that said "these two tasks read the same memory". 7.2 adds one, using an approximation: the threads of a process share one address space, so treat them as likely to share data.

## How the kernel picks a preferred LLC

The tracking hangs off the process, in `mm_struct`, the kernel structure that represents one address space. The kernel periodically samples where the threads of that process are running and counts, per LLC, how much of the process is sitting in each one. The LLC holding the most becomes the preferred LLC for the whole process, and that single number is what later decisions read.

During load balancing, when tasks must move between scheduler groups, it prefers to move tasks that already prefer the destination LLC, and it avoids dragging a task away from the LLC it prefers.

The guardrails matter as much as the feature does, because packing every thread of a busy process into one cache domain can leave that domain overloaded while the rest of the socket sits idle. The tunables live in debugfs, the kernel's debug filesystem, in the `llc_balancing` directory under `/sys/kernel/debug/sched/`:

- `enabled` , default`1` , is a plain on and off switch for the whole feature.
- `aggr_tolerance` accepts an unsigned 32-bit value, with no ceiling of 100, and sets how hard the kernel aggregates.`0` suppresses aggregation eligibility but leaves the feature's static key and accounting callbacks active.`1` , the default, is the careful setting: aggregation is skipped when the active-thread capacity check fails, or when the memory footprint exceeds the LLC with NUMA balancing enabled. Ordinary load balancing still moves those tasks. Values of`100` or more remove the footprint and active-thread capacity limits, but single-threaded processes remain excluded and migration load constraints still apply.
- `overaggr_pct` , default`50` , sets the LLC utilisation threshold used alongside source utilisation and imbalance checks when deciding migrations. With one hardware thread per core, the default threshold becomes 75%. It is not an absolute cutoff for accepting aggregated tasks.
- `imb_pct` , default`20` , sets the percentage margin when comparing source and destination LLC utilisation to avoid excessive imbalance and task bouncing.
- `epoch_period` , default about`10` ms (HZ/100, and the file holds the value in jiffies), is how often occupancy is collected.
- `epoch_affinity_timeout` , default`5` epochs, about`50` ms, is how long an inactive process keeps its preference before the kernel drops it.

Read your own values before changing any of them, because a distribution can ship different defaults: `sudo cat /sys/kernel/debug/sched/llc_balancing/aggr_tolerance`. A patch series posted in July 2026, after 7.2 was merged, proposes splitting `aggr_tolerance` into `aggr_tolerance_nr` and `aggr_tolerance_size`, so a later kernel may list those two files in its place.

## Which workloads plausibly gain, and which do not

The numbers below are the figures published with the patch series, measured on server class hardware, and some of them with the tolerance knob pushed to an aggressive setting. Read them as the good case on bare metal, not as a promise for your box.

## The data behind this chart

```
[
  {
    "label": "hackbench, 1 group, Xeon Sapphire Rapids",
    "gain_pct": 30.57
  },
  {
    "label": "schbench, 4 threads, p99 wakeup latency, Sapphire Rapids",
    "gain_pct": 37.78
  },
  {
    "label": "ChaCha20 run time reduction, AMD Genoa, aggressive tolerance",
    "gain_pct": 44
  }
]
```
Hackbench with a single group improved by **30.57%**, and the ChaCha20 run time on AMD Genoa fell by **44%**. All 3 results were taken on multi-LLC server hardware that the tester controlled end to end.

The shape of a workload that gains looks like this:

- More than one thread in a single process, so there is something to group.
- Real sharing between those threads, so a bounced cache line is a cost you are actually paying.
- A working set that fits inside one LLC, because a process larger than the cache cannot be given cache locality by moving it.
- Spare capacity on the machine, so the scheduler has a real choice about where to put the next thread.

And the cases where there is nothing to win:

- A machine already at full capacity. Every CPU is busy, so placement is forced and the reported gains fade.
- Single threaded processes, and pools of independent processes that share no data.
- A working set much larger than the LLC, which the careful `aggr_tolerance` setting deliberately skips when NUMA balancing is enabled.
- A node that reports one LLC, where the feature never turns on at all.

There is a cost side, and the series is open about it. Collecting occupancy is work done in the context of the task, and some runs showed request latency getting worse because that work delayed the task's return to user space. Aggregation can also raise latency variance even where average throughput improves. If you care about the tail rather than the average, measure your own tail.

## Does a VPS guest see any of this

Two facts decide the answer.

First, the feature is gated on topology. Cache aware load balancing is only enabled when more than one LLC exists inside one NUMA node, and the kernel records that during topology setup. Where a node reports a single LLC, the cache aware path stays inactive no matter how the tunables are set.

Second, the cache topology your guest reads is not the host's. It is whatever the hypervisor's CPU model presents. A default KVM (kernel based virtual machine) guest is normally not handed the host's real L3 layout, so the guest is reasoning about a simplified picture.

Check what your own guest sees:

```
systemd-detect-virt
lscpu --caches
for cache in /sys/devices/system/cpu/cpu*/cache/index*; do
  [ "$(cat "$cache/level")" = 3 ] && cat "$cache/shared_cpu_list"
done | sort -u
```
Cache indices enumerate entries; the `level` file identifies L3. One line listing every vCPU means the guest sees a single L3 domain, so the feature has nothing to arrange at that level. A missing `index3` alone does not establish that L3 is absent. Check `index*/level`; if no entry reports level 3, inspect the highest exposed cache level and its `shared_cpu_list` instead.

Then there is double scheduling, which is the honest caveat for any tenant. Your guest kernel places threads on vCPUs. The host kernel places those vCPU threads on physical cores. A guest that carefully groups four threads onto vCPU 0 to 3 has expressed a preference about four host threads, and the host is free to place them on different physical cache domains and to move them later. The guest decision is not wrong, it just is not the final one. That is the same layer boundary that produces the steal time a noisy neighbour leaves on your vCPUs.

So where does this feature reach a VPS tenant? Two places. On plans where the topology is real rather than synthetic, such as dedicated cores or larger instances with a passed-through layout, the guest scheduler is making a decision about hardware that exists. And on the provider's own host kernel, where cache aware placement of your vCPU threads is the provider's win to take, not yours. Cache layout also differs by architecture, which is one more variable when you compare an Arm VPS against an x86 VPS.

Measuring cache behaviour inside a guest is harder than on metal. `perf stat -e cache-misses` reports `<not supported>` when the guest lacks support for that event, including when the hypervisor does not expose the PMU (performance monitoring unit). Measure the throughput and latency of your own application instead, and use the debugfs knob as the switch between the two runs.

## Check whether your kernel has CONFIG_SCHED_CACHE

```
uname -r
grep -E '^(CONFIG_SCHED_CACHE=|# CONFIG_SCHED_CACHE is not set)' /boot/config-$(uname -r) || echo 'not set in this build'
sudo ls /sys/kernel/debug/sched/llc_balancing/
```
`CONFIG_SCHED_CACHE=y` means your kernel was built with it. A line reading `# CONFIG_SCHED_CACHE is not set` means the option exists in that version and your distribution turned it off. No matching line in an available config file means the option is absent from that configuration; check `uname -r` and the distribution's kernel source before concluding that the kernel predates it. Some minimal cloud images ship no `/boot/config-*` file, and there you read `zcat /proc/config.gz` instead, which works only when the kernel was built with `CONFIG_IKCONFIG_PROC`.

The `ls` line prints the tunables (`enabled`, `aggr_tolerance` and the rest) when the feature is compiled in. If it reports `No such file or directory` while `CONFIG_SCHED_CACHE=y`, mount debugfs first with `sudo mount -t debugfs none /sys/kernel/debug`.

If `uname -r` shows a 7.2 kernel, check the point release too. CVE-2026-98866, published on 18 October 2026, is a use-after-free in the cache aware scheduling code, and the kernel's CVE record lists 7.2 before 7.2.10 as affected, with 7.2.10 and 7.3 fixed. A distribution can backport the fix into a build with an older number, so its own security notice is the final word.

To compare your workload with the feature on and off, record the current value first, since you have to put it back:

```
sudo cat /sys/kernel/debug/sched/llc_balancing/enabled
sudo sh -c 'echo 0 > /sys/kernel/debug/sched/llc_balancing/enabled'
```
Run your benchmark, write the value you noted back, and run it again. Setting `aggr_tolerance` to `0` suppresses aggregation eligibility but leaves accounting active, so it is not the same switch and does not avoid vulnerable accounting code on an unfixed kernel. Debugfs writes do not survive a reboot, which is what you want while testing.

## When will a distribution kernel carry 7.2

Mainline is not what your VPS boots. The version in `uname -r` came from your distribution, and each distribution has its own path from a mainline release to your server.

Fedora rebases its stable releases onto new mainline kernels during their supported life, so `sudo dnf upgrade --refresh` plus a reboot is the whole process there. That cadence is part of what you are choosing when you run Fedora Server on a VPS.

Ubuntu ships a new kernel with each six month release, then carries it to the previous long term support (LTS) release through the HWE (hardware enablement) stack. As of 29 October 2026, Canonical's HWE table lists 6.8 from April 2024 as the GA kernel of Ubuntu 24.04 LTS. The 24.04 HWE stack moved to 6.17 in February 2026 and to 7.0 in August 2026, and that 7.0 HWE kernel is listed with support until April 2029. So installing the 24.04 HWE stack today gives you 7.0, which predates `CONFIG_SCHED_CACHE`. The step itself is quick: 7.0 shipped in Ubuntu 26.04 LTS in April 2026 and reached the 24.04 HWE stack in August 2026, four months later. 7.2 is not on that path, though. On 1 September 2026 Canonical's kernel team announced that Ubuntu 26.10 would target Linux 7.3 instead of 7.2, and Ubuntu 26.10, released on 15 October 2026, ships 7.3 (its release images carry a 7.3 build based on 7.3-rc6, because the final 7.3 arrived after the release). Ubuntu also carried 7.2 during 26.10 development: its archive records acceptance of `linux 7.2.0-5.5` into `stonking-proposed` on 28 August 2026. 7.3 is built on top of 7.2, and once you have it, the `grep` above tells you whether Ubuntu's build enables the option.

```
apt-cache policy linux-generic-hwe-24.04
sudo apt install --install-recommends linux-generic-hwe-24.04
```
Debian stable keeps one kernel for the life of the release and offers newer ones through backports, which you opt into per package. As of 29 October 2026, `trixie-backports` lists `linux-image-amd64` version `7.2.11-1~bpo13+1`:

```
echo 'deb http://deb.debian.org/debian trixie-backports main' | sudo tee /etc/apt/sources.list.d/backports.list
sudo apt update
sudo apt install -t trixie-backports linux-image-amd64
```
After any of these, reboot and confirm with `uname -r` and the `grep` above. A new kernel is not live-loadable: live kernel patching on a VPS replaces the code of individual functions in the running kernel, rather than installing a complete new kernel release. Cache aware scheduling adds fields to `mm_struct`, so getting the feature through a distribution kernel upgrade requires booting that kernel.

Two practical follow-ups. Keep the old kernel bootable until the new one has carried your load for a while, which is what pinning which kernel boots on a VPS is for. And watch `/boot`, since a small VPS boot partition fills up after a few kernel upgrades, as covered in cleaning up old kernels on Ubuntu.

One last piece of realism about ownership. On a KVM VPS the guest kernel is yours: you pick it, you boot it, you roll it back. The host kernel is your provider's, and no setting inside your guest changes which scheduler the hypervisor runs. So a release note about scheduler placement is only half a story for a tenant, and the half you control is the guest side.

## Sources used for this page

- The 7.2 changelog summary at kernelnewbies.org, for the release date of 16 August 2026 and the non-scheduler changes.
- The cache aware scheduling patch series archived at lwn.net/Articles/1041668 and lwn.net/Articles/1058288, for the per process preference mechanism and the reported benchmark figures.
- The merged tunables patch, "sched/cache: Add user control to adjust the aggressiveness of cache-aware scheduling", the scheduler pull request for v7.2, and the Linux 7.2 scheduler implementation and debugfs controls, read on 29 October 2026, for task placement, the tunables and their defaults.
- The patch that gates the feature on topology, "sched/cache: Introduce sched_cache_present", for the rule that cache aware load balancing needs more than one LLC in a NUMA node.
- The Linux 7.2 CPU sysfs ABI, read on 29 October 2026, for cache levels and shared CPU lists.
- The CVE-2026-98866 record, read on 29 October 2026, for the affected and fixed 7.2 versions.
- Canonical's HWE kernel lifecycle table and the Ubuntu 26.10 release notes, read on 29 October 2026, the kernel team's Ubuntu 26.10 kernel announcement, and the 7.2.0-5.5 archive acceptance, for the Ubuntu kernel versions and dates.
- Debian's linux-image-amd64 package in trixie-backports, read on 29 October 2026, for the available backported kernel.

For the previous release, see what changed in Linux kernel 7.1. For how the version numbers got here, see the Linux kernel history timeline.

## FAQ

### Does cache aware scheduling in Linux 7.2 make a VPS faster?

Usually not by itself. The feature only activates when a NUMA node reports more than one last level cache, and a typical KVM guest is not shown that layout, so the code never engages. Where it does engage, the guest is still scheduled twice: your kernel picks a vCPU, and the host kernel decides which physical core that vCPU thread runs on, so a guest side cache decision can be undone by the host. Run `lscpu --caches` and check the `index*/level` and `shared_cpu_list` files shown above. One LLC shared by every vCPU means there is nothing for the feature to arrange.

### How do I check whether my kernel has CONFIG_SCHED_CACHE?

Run `grep -E '^(CONFIG_SCHED_CACHE=|# CONFIG_SCHED_CACHE is not set)' /boot/config-$(uname -r)`. `CONFIG_SCHED_CACHE=y` means it is built in, and `# CONFIG_SCHED_CACHE is not set` means your distribution disabled it. No matching line in an available config file means the option is absent from that configuration; check the kernel version and distribution source before concluding that it predates the option. If the image has no `/boot/config-*` file, try `zcat /proc/config.gz`, which exists only on kernels built with `CONFIG_IKCONFIG_PROC`. You can confirm at runtime with `sudo ls /sys/kernel/debug/sched/llc_balancing/`, which lists the tunables when the feature is present.

### How do I turn cache aware scheduling off without rebooting?

Write `0` to the plain switch: `sudo sh -c 'echo 0 > /sys/kernel/debug/sched/llc_balancing/enabled'`. That disables the feature at runtime, which makes it a clean A/B switch for a benchmark. Read the current value first with `sudo cat /sys/kernel/debug/sched/llc_balancing/enabled` and write it back afterwards, because defaults vary between builds. Setting `aggr_tolerance` to `0` suppresses aggregation eligibility but leaves accounting callbacks active; it does not disable the feature or avoid vulnerable accounting code on an unfixed kernel. Nothing written to debugfs survives a reboot. If `cat` reports `No such file or directory` and debugfs is mounted, check your kernel configuration and which tunables that version provides.

### When will Ubuntu or Debian ship a 7.2 based kernel?

Fedora rebases stable releases onto new mainline kernels through normal kernel updates and a reboot. Ubuntu ships new kernels with each six month release and carries them to the previous LTS through the HWE stack. In Canonical's HWE table that step took four months for 7.0, from Ubuntu 26.04 in April 2026 to the 24.04 HWE stack in August 2026, and 24.04's GA kernel is still 6.8. Ubuntu carried 7.2 during 26.10 development, including `7.2.0-5.5` accepted into `stonking-proposed` on 28 August 2026. Ubuntu 26.10, released on 15 October 2026, ships 7.3 instead, which includes the scheduler change. Debian stable keeps one kernel for the release and offers newer ones through `trixie-backports`, which you install per package with `apt install -t trixie-backports linux-image-amd64`. As of 29 October 2026, that package is based on 7.2.11.
