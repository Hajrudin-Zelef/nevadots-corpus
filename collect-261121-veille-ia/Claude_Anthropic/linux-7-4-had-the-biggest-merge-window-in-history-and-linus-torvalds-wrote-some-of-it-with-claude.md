---
id: collect-261121-veille-ia/claude-anthropic/linux-7-4-had-the-biggest-merge-window-in-history-and-linus-torvalds-wrote-some-of-it-with-claude
title: "Linux 7.4 Had the Biggest Merge Window in History, and Linus Torvalds Wrote Some of It With Claude"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["torvalds", "claude", "kernel", "wrote", "linux", "biggest", "merge", "window"]
source: https://medium.com/soursen/linux-7-4-had-the-biggest-merge-window-in-history-and-linus-torvalds-wrote-some-of-it-with-claude-49b4d92e9f7e
source_anchor: ""
source_lines: [1, 45]
section: "Claude_Anthropic"
---
# **Linux 7.4 Had the Biggest Merge Window in History, and Linus Torvalds Wrote Some of It With Claude**

*The kernel just took in nearly 20,000 commits in one cycle, a record. The reason is the same force straining its maintainers, AI. The release is the clearest look yet at what that help gives and what it costs.*

Linux 7.4 landed the largest merge window in kernel history. The numbers tell the scale plainly, 19,671 commits in a single cycle, past the old record of 16,267 set by 7.3, against a source tree now approaching 42 million lines. A lot of that growth traces to one cause, AI tools writing and suggesting code faster than people used to. I want to walk through this release carefully, because it is the clearest picture we have of what AI is actually doing inside the most important open source project on Earth, the good and the mess, and because the person demonstrating the good is Linus Torvalds himself.

## Torvalds started writing code again, with Claude

For years Torvalds has mostly reviewed code rather than written it, saying yay or nay on what makes it into the kernel. In 7.4 he showed up as a contributor again, and he used Claude to do it.

The cleanest example is a piece of housekeeping he had wanted done for a long time. He removed two old kernel functions, `__get_user()` and `__put_user()`, that move a single value between user space and the kernel. The versions with the underscores skip the pointer validation that the safe versions do, so they are faster and more dangerous, and modern helpers already do the job better. Torvalds described using Claude for the cleanup as treating it like a fancy sort of Coccinelle script, referring to a tool kernel developers use for automated source transformations. His own commit message was blunt about why the functions had to go. “The __get_user() and __put_user() functions are entirely historical,” he wrote, and “when mis-used (read: every case) they are not only slower than the right thing.”

That was not his only AI assisted work in the cycle. The week before, he used Claude Code on a performance idea for the case where several CPU threads keep reading small amounts of data from the same file. He came up with the approach and wrote the boilerplate, Claude reworked it to cut unnecessary memory reference counting so CPUs can cache data more efficiently, and a Meta engineer finished and tested it. He also pointed Claude at the kernel’s associative array library, cleaned it up, and turned up three real bugs that had been sitting there since the library was first added in 2013.

## Torvalds thinks the model for AI help got healthier

What makes this more than a stunt is the stance behind it. Torvalds has moved from wary to cautiously positive, saying the kernel is now using a much healthier model for AI contributions than the panic of a year ago suggested. His position is consistent. An experienced developer who understands the code can point an AI at legacy cruft, clean it up, and surface hidden bugs, and that is a real gain. The trouble starts when the person steering does not have that understanding, which is exactly what the rest of this release shows.

## The performance wins are large and specific

Before the mess, the wins, because 7.4 is stacked with them. The one that made me do a double take is the FAT32 file system. A small change to how it preallocates space, allocating multiple clusters in a single operation instead of one cluster at a time, made preallocation up to 265 times faster in testing. A handful of modified lines, four files changed, and a decades old file system got hundreds of times quicker at one job.

Networking got one of the biggest swings. Reworking how the stack handles mixed big TCP and standard TCP traffic pushed throughput up to 70% higher, alongside hardware pacing offload that lets certain network cards manage outgoing packet timing and take load off the CPU. On the scheduler side, a change to the EEVDF scheduler for short slice tasks, the quick bursts of CPU work that pile up on machines with huge core counts, cut latency by up to 55% on hardware like Nvidia’s Vera CPUs and high core count servers.

There is more underneath. Intel’s Battle Mage GPUs got ultra low latency scheduling that handles memory page faults better, trimming 30 to 40 microseconds off GPU copy operations. RISC-V gained runtime detection of efficient unaligned memory access, so distributions can get an 11% memory copy bandwidth gain without recompiling the kernel for specific chips. AMD Zen 6 groundwork lets the kernel drop the expensive SRSO security mitigation on chips that provide branch target buffer isolation in hardware, keeping the protection while removing its cost.

## The speedup AI found by writing ugly code

The performance story that fits this release best is the kernel build itself. Lorenzo Stoakes, an ARM engineer and longtime kernel developer, posted a series of 23 patches to speed up kernel compilation. He used an AI model to find the bottlenecks and then to work out fixes, and he was honest about the output. “It generated a lot of code, much of it hideous,” he wrote. The AI found single threaded steps across Kbuild, kallsyms, modpost, objtool, and the Rust build system that could be parallelized.

The results are large. Full kernel builds run about 36% faster, incremental builds up to 70% faster, and builds with nothing to do up to roughly 90% faster. For people who rebuild the kernel dozens of times a day, that is a real change to the working day. Stoakes was careful to add that he checked correctness himself in both the build and the running kernels, which is the pattern Torvalds keeps describing, AI does the grunt work, a human who knows the system signs off.

## The AI flood the maintainers are drowning in

The other side of the same force is ugly. The sound subsystem maintainer opened his 7.4 pull by calling it, plainly, the AI flood. Networking maintainers have described themselves as completely overwhelmed. AI coding agents are generating a torrent of kernel patches and bug reports, and a large share is low quality, unrealistic, or plain wrong, which someone experienced still has to read and reject.

One feature in 7.4 captures the problem exactly. Greg Kroah-Hartman, effectively the second in command of the kernel, wrote a feature called Taint Force bind meant to flag the kind of nonsense configurations that AI assisted fuzzers were generating, where tools bind hardware devices to incompatible drivers at random and then file bug reports about the resulting crashes. The feature shipped, then broke legitimate GPU and PCI Express passthrough setups, so developers reverted it in the same release. An attempt to filter out AI generated noise created its own regression and had to be pulled. That is the tax in one story.

## Everything else that landed

The rest of the release is the usual broad sweep that makes Linux run on nearly anything. Rust support grew by about 17,000 lines, including support for self referential data structures that make certain complex drivers safer to write. Valve funded a new driver for Nintendo’s Switch 2 controllers, including the Joy-Con 2 and the Switch 2 Pro Controller. Apple’s M4 and A18 chips got initial mainline support, much of it from the Asahi Linux project, a first step toward booting Linux on recent Macs. HDMI 2.1 finally got enabled by default in the AMD GPU driver after years of fighting the HDMI Forum’s licensing restrictions. The kernel even keeps fixing memory safety bugs in its floppy disk driver, three decades of hardware supported in one tree.

## What this release actually tells you

Strip away the individual features and 7.4 is a single argument with two sides. AI in experienced hands produced a record cleanup from Torvalds, a 265x FAT32 speedup, and a 36% faster kernel build. AI in careless hands produced a flood of junk patches that maintainers now spend real time filtering, and at least one well meaning filter that broke working hardware. The kernel did not choose between those outcomes. It got both at once, in the same cycle, and the difference every time came down to whether a person who understood the code was the one steering. That is the model Torvalds says is working, and 7.4 is the proof and the warning in one release.
