---
id: collect-261121-veille-ia/langages-dev/torvalds-opens-linux-7-4-with-record-patch-flow-says-ai-work-is-now-much-healthier-biggo-finance
title: "Torvalds Opens Linux 7.4 With Record Patch Flow, Says AI Work Is Now 'Much Healthier' — BigGo Finance"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["torvalds", "work", "kernel", "much", "support", "linux", "patch", "healthier"]
source: https://finance.biggo.com/news/3f60411f-a8b2-43a7-9f2b-a1b557a3de9b
source_anchor: ""
source_lines: [1, 35]
section: "Langages_Dev"
---
# Torvalds Opens Linux 7.4 With Record Patch Flow, Says AI Work Is Now 'Much Healthier'

Linus Torvalds has rolled out the first release candidate for Linux kernel 7.4, closing a merge window that pulled in close to 20,000 commits - well above the roughly 15,000 patches developers typically submit for a new kernel cut - and he is treating the surge as evidence the project has settled on a better way of absorbing machine-generated code.

In his weekly state-of-the-kernel note, Torvalds wrote that he is "much happier with the flood of AI triggered work going into the merge window than it showing up mainly as extra fixes during the -rc period." He said he hopes the unusually heavy volume signals that AI-assisted fixes are becoming part of ordinary development rather than a wave of patches that lands during the stabilization phase. "I think that's a much healthier model," he wrote.

He stopped short of calling the timing conclusive. Torvalds noted that 7.4 is widely expected to become the next long-term support release, an unrelated factor that tends to swell some cycles, and added that the jump "may not *just* be the flood of AI patches."

His endorsement of AI contributions also came with a caveat. Torvalds characterized the release as "Large, and full of fixes for issues found by AI (and more and more the patches have been suggested by AI too, although they often get rewritten)."

AMD graphics work dominates the code base this cycle: register offsets and bit masks for the company's GPUs account for about a third of the changes, a pattern Torvalds said the project has seen before and will see again. "Just ignore the noise," he advised.

Beyond the volume, the release candidate carries a broad set of hardware and performance changes. Highlights compiled by kernel watcher Phoronix include:

| Area | Change in Linux 7.4 | 
|---|---|
| AMD | New PerfOpt feature for Radeon integrated GPUs aimed at AI/LLM workloads; Ryzen Zen 6 support finalized ahead of launch | 
| Graphics | AMDGPU HDMI 2.1 enabled by default | 
| Apple | Basic M4 enablement, not yet considered ready for end users | 
|  | Tensor G5 SoC and Pixel 10 device support in the mainline kernel | 
| Qualcomm | Further Snapdragon X2 enablement for PCs | 
| Intel | IPU8 support for upcoming Nova Lake laptops | 
| Other | Optional buffered IOmap for EXT4, laptop quirks and additions, faster kernel build times, Raspberry Pi 10-inch monitor support | 

Note: Feature list drawn from Phoronix's summary of the 7.4 merge window.

Timing matters for the release. The merge window closed on schedule despite Torvalds having been on holiday, and the report said 7.4 still has a good chance of shipping before the end of December, which would make it this year's long-term support kernel. That outcome depends on no serious delays late in the cycle.

The bigger picture is a kernel whose patch pipeline is being reshaped by automated tooling. Torvalds' framing suggests maintainers would rather see AI-assisted work arrive early, when it can be reviewed and reworked alongside everything else, than late, when it competes with stabilization fixes. The fact that many machine-suggested patches are rewritten before merging indicates human review remains the filter, even as the raw volume climbs.

For the hardware industry, the contents of 7.4 matter as much as its size. Mainline support for Ryzen Zen 6, Snapdragon X2 and Tensor G5 lands ahead of the products themselves, giving vendors a working upstream base before launch. Default-on HDMI 2.1 for AMD GPUs and the Radeon PerfOpt work also signal where the kernel community expects demand to grow: display bandwidth and local AI inference on integrated graphics.

Once added, BigGo Finance appears first in Google Search Top Stories, so you get the broadest, most up-to-the-minute, and most comprehensive global financial news first.

Add preferred source
