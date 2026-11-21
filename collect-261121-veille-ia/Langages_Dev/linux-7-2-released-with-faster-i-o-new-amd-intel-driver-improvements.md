---
id: collect-261121-veille-ia/langages-dev/linux-7-2-released-with-faster-i-o-new-amd-intel-driver-improvements
title: "Linux 7.2 Released With Faster I/O, New AMD & Intel Driver Improvements"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["linux", "intel", "improvements", "there", "faster", "driver", "released", "performance"]
source: https://www.phoronix.com/news/Linux-7.2-Released
source_anchor: ""
source_lines: [1, 25]
section: "Langages_Dev"
---
# Linux 7.2 Released With Faster I/O, New AMD & Intel Driver Improvements

Linux 7.2 brings many new features including Cache Aware Scheduling, the Intel-developed USB4STREAM protocol, various Intel and AMD hardware driver improvements, and AMDGPU HDMI 2.1 FRL and other great features.

On the performance side, Linux 7.2 brings some nice I/O performance improvements for both AMD as well as Intel systems. There are also some Intel Arc B390 graphics performance improvements and also faster poll performance.

During this final week of the Linux 7.2 cycle there were last minute sound device quirks/additions, a revert back to the DRM FIFO scheduler as the new "fair" default yielded a regression, and merged just today was adding the new tlbi=ipi boot option. Plus many other small bug/regression fixes.

Linus Torvalds wrote in the 7.2 announcement:

"Well, this last week of the release was - once again - bigger than I would have wished for, but hey, with the whole "new normal" thing, if I delayed releases for that reason we'd probably never have a release at all.


There are a number of fairly late reverts - the drm scheduling reverts stand out, but there's a few other ones in here too. It may not be pretty, but it's the correct way to deal with "oh, that code wasn't ready and caused problems". People will be trying that again later.


But while the drm reverts stand out and are some of the bigger patches in here, there's a lot of small fixes all over. Mostly in drivers, but networking remained fairly active this last week too. And then there are a few random patches to architecture files and perf core.


With this, the merge window for 7.3 obviously opens tomorrow, and I already have 40 pending pull requests. I appreciate all the timely people. You know who you are,


Linus"

Now onward to the Linux 7.3 merge window that is looking very exciting.
