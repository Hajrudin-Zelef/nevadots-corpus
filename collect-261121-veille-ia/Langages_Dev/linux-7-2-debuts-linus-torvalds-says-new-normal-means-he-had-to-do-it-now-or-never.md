---
id: collect-261121-veille-ia/langages-dev/linux-7-2-debuts-linus-torvalds-says-new-normal-means-he-had-to-do-it-now-or-never
title: "Linux 7.2 debuts, Linus Torvalds says ‘new normal’ means he had to do it now ... or never?"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-08-17"]
keywords: ["linux", "kernel", "torvalds", "normal", "release", "linus", "never", "debuts"]
source: https://www.theregister.com/os-platforms/2026/08/17/linux-72-debuts-linus-torvalds-says-new-normal-means-he-had-to-do-it-now-or-never/5288250
source_anchor: ""
source_lines: [1, 27]
section: "Langages_Dev"
---
OS Platforms

# Linux 7.2 debuts, Linus Torvalds says ‘new normal’ means he had to do it now ... or never?

Say hello to smarter use of caches to speed things up, and a very odd gaming controller. Say goodbye to some PCMCIA support

Linus Torvalds has decided version 7.2 of the Linux kernel is ready for release, albeit in a “new normal” state that he seems not to entirely love.

The kernel boss announced the debut of a new kernel in his weekly development status update, which on August 16 opened with the observation that “this last week of the release was – once again – bigger than I would have wished for.”

He attributed that uncomfortable size to what he last week described as “the new normal” for the kernel at a time when developers have increased the volume of contributions using AI coding tools.

“If I delayed releases for that reason we'd probably never have a release at all,” Torvalds wrote, before adding that the release includes “a number of fairly late reverts - the drm scheduling reverts stand out, but there's a few other ones in here too.”

“It may not be pretty, but it's the correct way to deal with ‘Oh, that code wasn't ready and caused problems,’” he wrote.

As ever, the new kernel release includes important and seemingly frivolous inclusions.

Among the latter is support for a gaming controller called the “Zenaim Leverless,” which offers a collection of buttons on a black slab. Apparently e-sports pros think it’s just the sort of thing they need to rack up high scores or crush their foes during tournaments. And now they can use it with Linux!

Perhaps more relevant to a majority of *Reg* readers is the inclusion of a tech called “Cache Aware Scheduling” that makes the kernel better at handling the data stored in caches across manycore chips like AMD’s EPYC 5 and Intel’s Xeon 6.

Qualcomm senior engineer Vishnu Santhosh wrote a good explainer about the tech. Long story short, it’s about making processors aware of useful data already in a cache, so they can use it instead of doing extra work to access the relevant data.

The release also includes work that makes it possible to run Linux on Apple M3 devices, the usual handful of graphics updates, and work to ensure that the kernel will be ready to support next-gen chips from AMD, Intel, and Nvidia.

Notable deprecations include ending support for AppleTalk, and for old-school ISA and PCMCIA adapters used on ARCNet networks. This hits hard because *The Register* believes that PCMCIA stands for People Can’t Memorize Computer Industry Acronyms, and not for the Personal Computer Memory Card International Association. ®
