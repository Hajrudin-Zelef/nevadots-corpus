---
id: collect-261121-veille-ia/langages-dev/linux-7-4-rc1-released-with-a-record-nearly-20-000-commits-ai-driven-fixes-begin-joining-regular-dev
title: "Linux 7.4-rc1 Released With a Record Nearly 20,000 Commits: AI-Driven Fixes Begin Joining Regular Development"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["fixes", "merge", "window", "torvalds", "commits", "development", "kernel", "during"]
source: https://dev.to/joho_todai/linux-74-rc1-released-with-a-record-nearly-20000-commits-ai-driven-fixes-begin-joining-regular-3mj4
source_anchor: ""
source_lines: [1, 57]
section: "Langages_Dev"
---
Linux 7.4-rc1 was released on November 1 local time, with nearly 20,000 merge-window commits setting a kernel record. Torvalds calls the growing influx of AI-driven fixes a "healthy model."

**Key points**

- 
**What happened:** Linux 7.4-rc1 recorded the largest merge window in kernel history, with nearly 20,000 commits
- 
**Why it matters:** Torvalds sees AI-driven fixes arriving during the merge window rather than the stabilization period as a healthier development model
- 
**What's included:** Support for next-generation hardware, including Zen 6, Snapdragon X2, Nova Lake, and Apple M4, has been integrated
- 
**Validation still ahead:** Whether the number of fixes during the RC period actually supports the merge window's "healthy" assessment will remain unclear until the final 7.4 release

In his release announcement, Torvalds described this merge window as "big." A typical merge window—the roughly two-week period when new changes are integrated into the kernel—collects **about 15,000 commits**, but 7.4 swelled to **19,671**. Comparing the last four merge windows makes the acceleration clear: 13,963 for 7.1, 14,395 for 7.2, 16,267 for 7.3, and 19,671 for 7.4.

However, more commits do not necessarily mean more user-facing features.

According to Torvalds, **about a third of the diff—the overall patch—consists of AMD GPU register offset and bitmask definitions**. Supporting each new GPU involves registering internal hardware addresses in large numbers of header files, a recurring structural source of inflated patch volume. Torvalds wrote that this should be ignored as noise.

Once that noise is excluded, the breakdown of changes is reportedly typical. About half concerns drivers, with the rest spread across architecture updates, filesystems, tools, and the core kernel, including networking, Rust, BPF, and memory management. The source tree has reached about 41.61 million lines, approaching the 42 million mark.

Another force pushing up the commit count is the influx of AI-related fixes.

In the release announcement, Torvalds welcomed AI-prompted fixes arriving during the merge window.

Having the flood of AI-prompted work arrive during the merge window is much preferable to seeing it mainly as additional fixes during the RC period. I hope this larger-than-usual merge window is a sign that AI fixes are becoming part of regular development rather than a flood of fixes pouring into the stabilization phase. I think that is a much healthier model.


Kernel development typically consists of a two-week merge window for integrating features, followed by several weeks of release candidates for stabilization fixes. If responses to AI-discovered bugs concentrate in the RC period, the stabilization phase itself becomes unstable. Torvalds views absorbing that work during the merge window as a healthy change.

He also said that patches proposed by AI itself are increasing, alongside fixes for problems AI has found. However, he added that they are often rewritten. AI drafts; people finish. That sentence offers a glimpse of the division of labor now taking place in kernel development.

The other side of this change is an increased burden on maintainers. In the networking subsystem (in Japanese), maintainers continue to be overwhelmed by the volume of AI-generated bug reports and patches. The asymmetry between the cost of writing patches and the cost of reviewing them had already surfaced in 7.3, and it seems reasonable to infer that the pressure increased further in 7.4 as the commit count reached 19,671.

Torvalds himself cautioned that AI is not the only reason for the increase in size. He also mentioned the possibility that developers' expectations of 7.4 becoming this year's LTS (long-term support) kernel encouraged them to bring changes forward. Because LTS kernels are supported longer than regular releases, developers have an incentive to get their changes into one.

A broad set of next-generation hardware support has been integrated into 7.4.

On the AMD side, preparations for Ryzen Zen 6 (in Japanese) are in place, and PerfOpt has been added to make AI and LLM processing more efficient on Radeon integrated GPUs. HDMI 2.1 output is now enabled by default in the AMDGPU driver.

On the Intel side, support has been added for the IPU8 image processing unit in Nova Lake, its next-generation laptop SoC.

Support for laptops powered by Qualcomm Snapdragon X2 has also expanded. In Japan, Microsoft sells the Snapdragon X2 Elite-powered 13.8-inch Surface Laptop (8th Edition) starting at 291,280 yen including tax. Progress in kernel support could eventually broaden the options for running Linux on such machines. However, kernel SoC support alone does not guarantee trouble-free power management or resume from sleep; practical usability on each model needs to be assessed alongside distribution support.

Initial Apple M4 support code (in Japanese) has also been integrated, but it has not reached the point of everyday use by end users. Mainline kernel support for the Google Tensor G5 SoC and Pixel 10 devices has also been added.

The EXT4 filesystem has gained buffered IOmap (in Japanese), an option for handling buffered I/O through the iomap infrastructure. Other changes integrated for 7.4 range from shorter kernel build times (in Japanese) to removal of the BFS filesystem (in Japanese), which had been present since 1999.

The release of 7.4-rc1 closes the feature integration period and begins the stage of bug fixes and stabilization through successive release candidates. Torvalds said he was still considering two pull requests that might be merged in rc2.

If development goes smoothly, 7.4 could receive its final release in December and become this year's LTS kernel, but neither the release date nor the LTS designation has been confirmed.

The record nearly 20,000 commits are just one result of adding AI as a variable in kernel development. This time, Torvalds called the change "healthy." How the number of fixes actually develops during the RC period will determine whether that description holds up.

*Originally published on Joho no Todai. The Japanese original is here.*

## Top comments (0)
