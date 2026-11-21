---
id: collect-261121-veille-ia/linux-kernel/linux-kernel-7-2-rc7-released-stable-version-scheduled-for-next-weekend-ostechnix
title: "Linux Kernel 7.2 RC7 Released: Stable Version Scheduled for Next Weekend - OSTechNix"
domain: linux-kernel
role: reference
task: article
actors: []
dates: []
keywords: ["kernel", "code", "linux", "release", "fixes", "linus", "stable", "lines"]
source: https://ostechnix.com/linux-kernel-7-2-rc7-released/
source_anchor: ""
source_lines: [1, 31]
section: "Linux_Kernel"
---
*272*

Linus Torvalds released the **seventh release candidate** (RC7) for **Linux Kernel 7.2** on Sunday, August 9, 2026. This release marks the final testing phase of the 7.2 cycle. If everything goes according to plan, we will see the stable release very soon.

## The "New Normal" Size

In the **Kernel 7.2-rc7 announcement mail**, Linus admitted that he is not exactly thrilled about the large size of this release candidate. However, he accepts this heavy volume of code as the "new normal" for kernel development.

The total Linux kernel tree recently blew past the **43 million lines of code** milestone. Although developers continuously clean up old code, such as removing the ancient i486 processor support, the kernel continues to grow rapidly. Much of this size comes from modern hardware drivers. Specifically, the AMD graphics driver (AMDGPU) alone accounts for over **6.3 million lines of code**.

The following data, synthesized from the `cloc` (Count Lines of Code) utility, captures the growth across the transition to the 7.2 branch:

| Metric | Linux 7.1 | Linux 7.2 | Growth (Delta) | 
| **Blank Lines** | 5,107,123 | 5,211,184 | +104,061 | 
| **Code Comments** | 4,841,507 | 5,033,878 | +192,371 | 
| **Actual Code** | 32,975,752 | 33,653,681 | +677,929 | 
| **Total Lines** | **42,924,382** | **43,898,743** | **+974,361** | 

Why does the kernel have so many changes late in the development cycle? Interestingly, Linus pointed out that **various AI tools reviewed and generated many of these fixes**.

Earlier in this cycle, developers noticed that nearly **29%** of recent commits carried **"`Fixes:`"** tags. They suggested that AI bots have been finding a high volume of small, hidden bugs. This is why we are seeing a constant stream of tiny, precise fixes all over the code.

While the vast majority of the update contains tiny fixes spread across drivers, filesystems, and architecture code, a few larger changes stand out in the data:

- **System Cryptography:** The patch features notable security fixes for the`s390/zcrypt` device driver.
- **Btrfs Filesystem:** Developers brought back the important`btrfs` fixup worker infrastructure to manage storage writebacks safely.
- **Network Security:** The release includes several critical`netfilter ipset` fixes to protect network routing.

Fortunately, none of these late-stage changes look scary or broken. Therefore, Linus sees no reason to delay the final release. Unless an unexpected, major bug pops up, **we should expect the official, stable Linux 7.2 launch next weekend**.

If you want to help ensure a smooth release, you can grab the latest code from the **Official Linux Kernel Archives** or **Linus Torvalds' Git tree** and start testing today.
