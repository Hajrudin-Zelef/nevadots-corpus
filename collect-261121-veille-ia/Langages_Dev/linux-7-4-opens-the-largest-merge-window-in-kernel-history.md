---
id: collect-261121-veille-ia/langages-dev/linux-7-4-opens-the-largest-merge-window-in-kernel-history
title: "Linux 7.4 opens the largest merge window in kernel history"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-01"]
keywords: ["window", "merge", "kernel", "linux", "largest", "opens", "history", "phoronix"]
source: https://www.8bitsbytes.com/articles/linux-74-rc1-largest-merge-window-ai.html
source_anchor: ""
source_lines: [1, 55]
section: "Langages_Dev"
---
# Linux 7.4 opens the largest merge window in kernel history — and AI is why

Linux **7.4-rc1** was tagged on **1 November 2026**, closing a merge window that Phoronix describes as the largest in the kernel's history: roughly **19,671 commits** since Linux 7.3, once every merge-window statistic is counted. The release candidate itself brings the usual first-cut summary of a cycle — notable performance work, **AMD Zen 6 preparations**, and **HDMI 2.1 enabled by default for AMDGPU** — but the headline number is the volume of change. Phoronix

That volume is not only a curiosity for kernel-watchers. It changes the release engineering problem, the review burden on maintainers, and the calculations for anyone who runs Linux in production and has to decide how quickly to adopt a new series.

## The number that matters

A normal merge window lands in the thousands of commits. Nineteen thousand is a different category of change, and Phoronix attributes a large part of the upward trend in patch volume to AI-driven development work arriving in the kernel's normal flow. Michael Larabel's coverage of the merge window called it easily the largest on record, and the numbers back that up: nearly twenty thousand changes across two weeks, with the review capacity of a few hundred maintainers as the standing constraint. Phoronix: merge window · Phoronix: commit count

This is not a new debate. Linus Torvalds has already described AI-assisted contributions as "the new normal" in a prior cycle, and the community has been arguing about attribution, review standards and maintainer load since. What is new is the scale. A "new normal" of tens of thousands of merge-window commits per cycle is a materially different maintenance environment from one where a busy window meant six thousand.

Every patch that lands still has to be reviewed by a human who understands the subsystem. If the input grows faster than the review capacity, the shortage is no longer patches — it is attention.


## What is actually in 7.4

Beyond the volume, the release candidate carries the kind of work that shows up in upgrade planning:

#### Notable in 7.4-rc1

- **AMD Zen 6 preparations** — groundwork for the next-generation Ryzen parts, landed early as usual.
- **AMDGPU HDMI 2.1 by default** — a display-output change that matters for home theatre and multi-monitor Linux desktops.
- **Performance work** — scheduler, I/O and memory-management improvements of the kind that arrive every cycle and accumulate over a year.
- **Hardware enablement** — the usual long tail of device support, which is why kernel upgrades and new laptops travel together.

## The support picture around it

While the merge window was open, the stable branch kept moving. Kernel.org listed **7.3.1** as stable on 30 October, and marked **7.2.15** as end-of-life on 1 November. For administrators who track releases this is the familiar rhythm: a new mainline candidate opens while the previous stable series receives point releases, and the one before that stops receiving them. If your fleet is sitting on 7.2 because it was current when you built the image, it is now outside support. kernel.org

Ubuntu 26.10 already shipped on the 7.3 series in October, which means the next LTS will most likely land with either 7.3 or 7.4 depending on timing — the kind of detail worth checking before committing to a hardware refresh that depends on a specific driver.

## What to do with this as an operator

Very little, immediately, and that is the point. Release candidates are not for production. The practical takeaways are about planning:

**Track the series you actually run.** The useful question is not "what is new in 7.4" but "which supported series am I on, and when does it leave support". Kernel.org's release table answers that in ten seconds.

**Budget for review, not just adoption.** If you maintain out-of-tree modules, a large merge window usually means more internal API churn to absorb. A record cycle is a good moment to check whether your drivers, filesystems and tracing tools still build against the latest candidate.

**Watch the AI-patch debate for policy implications.** Organisations increasingly require disclosure when AI assisted a contribution. The kernel's own norms — review by a human who understands the code, with attribution expectations — are a reasonable template for internal policy elsewhere.

Linux 7.4 will spend the next several weeks in release-candidate cycles before a stable release. Given the size of what landed, expect an unusually busy rc series. Analysis

## Is Linux 7.4 stable yet?

No. As of 3 November 2026 only 7.4-rc1 has been tagged, on 1 November. Stable typically follows several weekly release candidates. Linux 7.3.1 is the current stable 7.3 point release.

## Why is the merge window so large this cycle?

Phoronix attributes much of the growth to AI-driven development work entering the normal patch flow, on top of the kernel's already-rising baseline of hardware enablement and driver work. The result is the largest merge window on record, at roughly 19,671 commits.

## Should I upgrade my servers to 7.4 when it ships?

Only on your distribution's schedule. Kernel upgrades in production should follow the vendor kernel for your release, which carries backported fixes without the churn of mainline. Running mainline is for hardware enablement and testing.
