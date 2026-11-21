---
id: collect-261121-veille-ia/linux-kernel/linux-kernel-7-4-rc1-is-officially-released-here-s-what-s-new-ostechnix
title: "Linux Kernel 7.4 RC1 is Officially Released: Here's What's New - OSTechNix"
domain: linux-kernel
role: reference
task: article
actors: []
dates: []
keywords: ["linux", "kernel", "merge", "release", "commits", "window", "developers", "greg"]
source: https://ostechnix.com/linux-kernel-7-4-rc1-released/
source_anchor: ""
source_lines: [1, 79]
section: "Linux_Kernel"
---
*497*

Linus Torvalds has officially released **Linux 7.4-rc1**, marking the end of a record-breaking two-week merge window. The **first release candidate (RC1)** for the upcoming **Linux kernel 7.4** is now available for public testing, following its release on Sunday, November 1, 2026.

This release stands out for having the largest merge window in Git history. It brings significant changes across kernel development, numerous hardware support updates, and renewed discussion about which kernel version will become the next Long-Term Support (LTS) release.

Table of Contents

The Linux 7.4 merge window opened shortly after the scheduled release of **Linux 7.3**. Over the course of 13.92 days, maintainers merged **19,780 total commits** into the mainline kernel tree.

To understand the scale of this release, consider these key statistics:

- **Non-Merge Commits:** 18,591 commits introduced direct code changes.
- **Merge Commits:** Maintainers accepted 1,189 merge commits from subsystem trees.
- **Daily Pace:** Developers integrated an average of 1,335 non-merge commits per day.

This volume represents a **20.6% increase in non-merge commits** over Linux 6.7, making **Linux 7.4 the largest single merge window** since Git tracking began with version 2.6.30.

| Kernel Release | Total Commits | Non-Merge Commits | Merge Commits | Window Duration | Daily Pace (Non-Merge) | 
| **Linux 7.4-rc1** | **19,780** | **18,591** | **1,189** | **13.92 days** | **1,335 / day** | 
| Linux 7.3 | 16,267 | 15,267 | 1,000 | 13.96 days | 1,094 / day | 
| Linux 6.7 | 16,203 | 15,418 | 785 | 13.91 days | 1,108 / day | 

We will publish a detailed analysis of the Linux kernel merge windows soon, explaining our methodology and how we arrived at these figures.

One of the most notable developments in Linux 7.4 is the expanding role of Artificial Intelligence (AI) tools in kernel development.

In previous development cycles, developers often used AI tools to generate bug reports or highlight issues during the testing phase. In Linux 7.4, however, developers actively used AI tools to write and refine patch proposals before the merge window opened.

Linus Torvalds noted this shift positively. When developers submit AI-assisted bug fixes and code improvements directly during the merge window, maintainers can review them early. This prevents a flood of late-stage fixes during the stabilization phase, leading to a smoother release cycle overall.

Although the commit count is exceptionally high, the core structure of the Linux kernel remains stable and predictable.

A large portion of the total patch size comes from automated hardware data. Specifically, automated dumps of AMD GPU register offsets and bit masks make up roughly one-third of the raw patch diff.

When you exclude these massive header files, the remaining changes follow a typical distribution:

- **Device Drivers:** Account for about half of all remaining changes.
- **Core Infrastructure:** Subsystem updates cover architecture support, memory management, and security tools.
- **Filesystems and Tooling:** Ongoing maintenance and performance fixes across major filesystems.

In the **Linux 7.4-rc1 announcement**, Linus Torvalds speculated that part of the commit rush occurred because developers expected Linux 7.4 to become the next Long-Term Support (LTS) release.

However, lead stable maintainer Greg Kroah-Hartman (Greg KH) clarified his plans in a reply on the mailing list. In reply to his Linus mail, Greg stated: *"so am planning on 7.3 to be the next LTS kernel..."* and advised developers to *"count on 7.3.y to be the next LTS unless something really crazy comes up"*.

Greg explained that when contributors try to "force" features into a release to hit an expected LTS window, it introduces instability. While Greg has outlined his plan for Linux 7.3, Linus Torvalds has not publicly replied to that specific thread yet.

Please note that while Greg manages LTS kernel selections, this reflects the current maintainer planning direction rather than a closed debate.

If you want to test the latest release candidate, you can download **Linux 7.4 release candidates** directly from the **Official Linux Kernel Archives** or inspect the source code in **Linus Torvalds' Git tree**.

Before downloading and installing Linux 7.4-rc1, keep these points in mind:

- **Target Audience:** Release candidate (RC) versions are pre-release snapshots meant primarily for kernel developers, testers, and Linux enthusiasts who feel comfortable compiling the kernel from source.
- **Pre-Release Risks:** Like any release candidate, Linux 7.4-rc1 may contain bugs, regressions, or incomplete features.
- **System Safety:** If you rely on your primary computer for daily work or critical tasks, it is safest to remain on the latest stable Linux kernel until Linux 7.4 reaches its official final release.

With the release of 7.4-rc1, the merge window is officially closed. No new features can be added until the next release cycle.

Over the coming weeks, developers and testers will focus on finding bugs, testing driver compatibility, and refining code quality. If testing proceeds on schedule, the final Linux 7.4 kernel will launch around late December 2026.

Linux 7.4-rc1 was released on November 1, 2026. Based on the Linux kernel's typical development cycle, Linux kernel 7.4 final is likely to be released in mid-to-late December 2026.

The usual Linux kernel release process is:

- RC1: Merge window closes and the first release candidate is published.
- RC2–RC7: Weekly release candidates, primarily for bug fixes and stabilization.
- RC8: An additional release candidate if the kernel needs more stabilization.
- Final release: Usually after 7–8 weeks from RC1.

Interestingly, there is already a more precise estimate. Greg Kroah-Hartman explicitly indicated **December 20, 2026**, as the projected Linux 7.4 final release date in his November 2 mailing-list response.

- **7.4 Merge Window Opened:** October 19, 2026
- **7.4 Merge Window Closed (-rc1):** November 1, 2026
- **Projected 7.4 Final Release:****December 20, 2026**

The date is subject to change if additional release candidates are needed. The standard schedule is documented by kernel.org.

In my estimation, **December 20, 2026, is the most likely release date for Linux kernel 7.4**, barring any unexpected delays during the stabilization process.
