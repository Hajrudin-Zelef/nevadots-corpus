---
id: collect-261121-veille-ia/langages-dev/linux-7-4-rc1-closes-the-biggest-merge-window-in-years-with-almost-20-000-commits-free-linux
title: "Linux 7.4-rc1 closes the biggest merge window in years, with almost 20,000 commits | Free Linux"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["linux", "window", "merge", "commits", "perf", "pull", "torvalds", "almost"]
source: https://free-linux.com/articles/linux-7-4-rc1
source_anchor: ""
source_lines: [1, 37]
section: "Langages_Dev"
---
Linus Torvalds tagged **Linux 7.4-rc1** on **Sunday, 1 November 2026**, two weeks after Linux 7.3, and his announcement didn’t bother with understatement: “This is a *BIG* merge window.” He puts it at “almost 20k commits vs the more common ~15k”. We checked the Git tree: there are **19,780 commits** between the `v7.3` and `v7.4-rc1` tags, merges included. Counted the same way, the Linux 7.3 merge window brought in 16,267 and Linux 6.7, long considered the record holder, 16,203.

## Why so big

Torvalds gives two reasons. The first, which he calls “by now obvious”, is the flood of AI-triggered work, and he takes it as a good sign: he’d much rather see those fixes land during the merge window than pile up during the release candidates, because it means they’re becoming part of normal development. More and more patches are also being suggested by AI, he notes, although they often get rewritten. The second reason is that **people expect 7.4 to be the next LTS release**, which always makes a release bigger.

The diff is huge too, partly for a familiar reason: yet another AMD hardware register dump into giant headers. According to Torvalds, **a full third of the patch is AMD GPU register offsets and bit masks**. Ignore that and it looks fairly normal: about half drivers, with the rest spread across architectures, filesystems, tooling (mainly `perf` and selftests) and core kernel work in networking, Rust, BPF and memory management. The shortlog alone runs to 24,000 lines and 1.2 MB.

## AMD Zen 6: fewer mitigations, better frequency handling

The x86 speculation pull from **Borislav Petkov** has good news for upcoming **Zen 6** processors. Their branch target buffer tags entries per context, so they no longer need the costly **SRSO SafeRET** mitigation; the kernel now detects these chips and skips it.

On the power side, the `amd-pstate` driver gets initial support for maximum frequency capacity on Zen 6 client parts, plus new EPP (energy-performance preference) tuning values for that generation, mostly from **Mario Limonciello**.

## AMDGPU finally does HDMI 2.1 properly

Dave Airlie’s DRM pull says it with a “finally”: **AMDGPU enables HDMI 2.1 FRL** (Fixed Rate Link, the signalling mode behind HDMI 2.1’s higher bandwidth). Along with it come **FreeSync, ALLM and VRR over HDMI 2.1**, which matters if you plug a Radeon machine into a recent TV. The driver also picks up UALink over Ethernet support.

## perf: sturdier, faster Python scripts

**Arnaldo Carvalho de Melo**’s tooling pull ports **perf’s Python scripts** to the `perf` module and lets them run standalone, without going through `perf script`. The module itself gets lazy sample callchains and event copies (only computed when used), better GIL handling and improved session and thread lookups, plus new `treport`, flame graph and SQLite/PostgreSQL exporters. Data-type profiling (`perf annotate --data-type`) also reaches arm64.

## Ceph: lazy I/O for a whole mount

The in-kernel Ceph client gains a **`lazyio`** mount option from **Xiubo Li**. It turns on LazyIO for every file on the mount rather than per file via an `ioctl`, allowing buffered I/O through the page cache even when several clients have the same file open for writing. That suits HPC workloads that can live with looser cache coherency. In the same pull, Ilya Dryomov disables inline data support, which never left experimental status, and mentions “a large pile of LLM-based hardening patches”.

## The taint that came and went

The oddity of this cycle came from **Greg Kroah-Hartman**. The driver core pull added **`TAINT_FORCED_BIND`**, a taint flag (letter “Y”) set whenever someone writes to the sysfs `bind` and `unbind` files to attach a device to a driver by hand. The aim was to stem a stream of bogus bug reports from fuzzers binding random drivers to random devices, and the pointless patches that followed.

It didn’t last. Before the window closed, Torvalds merged a revert: it turns out **VFIO** relies on that mechanism to pass devices through to virtual machines, so ordinary workflows were tainting the kernel. In the commit, Greg notes that syzbot no longer does this, but that “in this madness of LLMs these days, you can’t be sure” nobody else will start, and a more targeted approach will be revisited after 7.4.

## What’s next

Torvalds still had two pull requests he was undecided about, which may show up in rc2. If the usual schedule of seven release candidates holds, **Linux 7.4 should be out around 20 December**, or a week later if an rc8 is needed. We covered the RISC-V side of this release in Linux 7.4 and RISC-V, kernel versions and dates are on the Linux page, and for comparison with an earlier cycle, here’s what Linux 7.1 brought.

**Sources:** Linus Torvalds’ “Linus 7.4-rc1” announcement to the kernel mailing list; the v7.4-rc1 tag and merges in Torvalds’ tree: x86 bugs, power management, DRM, perf tools, Ceph, driver core and the TAINT_FORCED_BIND revert.
