---
id: collect-261121-veille-ia/langages-dev/linux-7-4-rc1-released-a-huge-ai-driven-merge-window-ubuntu-free
title: "Linux 7.4-rc1 Released: A Huge AI-Driven Merge Window - Ubuntu Free"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["kernel", "ubuntu", "linux", "merge", "torvalds", "window", "release", "released"]
source: https://www.ubuntufree.com/linux-7-4-rc1-released/
source_anchor: ""
source_lines: [1, 31]
section: "Langages_Dev"
---
**The short version:** Linus Torvalds released **Linux 7.4-rc1** on Sunday, 1 November 2026, closing a two-week merge window with almost 20,000 commits. That is far more than usual, and Torvalds says AI-found fixes are a big part of the reason. Linux 7.4 is also widely expected to become the next long-term support (LTS) kernel.

**“This is a *BIG* merge window”:** those are Torvalds’ own words in the rc1 announcement. He counts almost 20,000 commits, compared with the more common 15,000 or so, and the diff is huge too.

**AI is the main reason:** Torvalds says the release is “full of fixes for issues found by AI”, and more and more patches are suggested by AI too, although they often get rewritten. He is happy that this work arrived in the merge window rather than as a flood of extra fixes later in the cycle: “I think that’s a much healthier model. But we’ll see how the -rc’s look.”

**The LTS effect:** he also points to a second reason. People are expecting 7.4 to be the next LTS release, and LTS kernels tend to attract bigger merges. “So it may not *just* be the flood of AI patches,” he wrote.

**A third of the patch is AMD headers:** a full third of the diff is AMD GPU register offsets and bit masks. Torvalds tells readers to “just ignore the noise”. Even the shortlog is 24,000 lines and 1.2 MB, so he only posted his usual merge summary.

**Two pull requests still pending:** Torvalds still had two pull requests he was thinking about, so they may or may not show up in rc2.

**The usual spread:** about half of the changes are drivers, or closer to two thirds if you count the AMD GPU headers. The rest covers architecture updates, filesystems, tooling (mainly perf and selftests) and core kernel work in networking, Rust, BPF and memory management. Here are some areas from Torvalds’ merge summary and early coverage of the cycle:

**About seven weeks to go:** new kernels usually get a release candidate every Sunday, and most cycles end after rc7 or rc8. That points to a final Linux 7.4 release around **20 December 2026**, or 27 December if an rc8 is needed.

**Why the LTS matters:** a long-term support kernel gets bug and security fixes for years, so it ends up in many distributions, Android devices and embedded products. The last one was Linux 6.18. Kernel.org has not formally named 7.4 as an LTS yet, so treat it as expected rather than confirmed.

**7.2.15 was the last update:** on 1 November, Greg Kroah-Hartman released 7.2.15 and marked the 7.2.y series as end of life. His note was simple: “Please move to 7.3.y at this time.” He said 7.2 went end of life a little sooner than normal, so the stable team does not have to keep eight branches alive at once.

**Where that leaves you:** the newest stable kernel on kernel.org is 7.3.1, released on 30 October. If you build or install your own kernels, move off 7.2 now, or pick an LTS branch such as 6.18, 6.12, 6.6 or 6.1.

**Nothing changes today:** Ubuntu does not ship release candidates. Ubuntu 26.10 already uses Linux 7.3, and Ubuntu 26.04 LTS uses Linux 7.0. See our Ubuntu 26.10 new features roundup for what the 7.3 kernel brought.

**Check which kernel you run:** this command prints your current kernel version:

**Ubuntu 27.04 is still open:** Ubuntu has not said which kernel 27.04 Tenacious T-Rex will ship. Its Kernel Freeze is on 8 April 2027, so check our Ubuntu 27.04 release schedule for the key dates.

**Should you test rc1?** Only on a spare PC or a virtual machine. Release candidates are meant for developers and testers, and a lot of code is still settling. Your Ubuntu kernel stays on its supported version and keeps getting security fixes through normal updates.

Linux 7.4-rc1 is out with a record-sized merge window, driven largely by AI-found fixes, and 7.4 looks set to be the next LTS around Christmas. For Ubuntu users nothing changes yet, but if you run Linux 7.2 yourself, it is time to move on.
