---
id: collect-261121-veille-ia/langages-dev/linux-7-4s-merge-window-is-huge-and-linus-says-it-might-not-just-be-ai-linux-consultant
title: "Linux 7.4’s Merge Window is Huge, and Linus Says it Might Not Just Be AI – Linux Consultant"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["linux", "window", "merge", "linus", "says", "might", "huge", "just"]
source: https://www.linuxconsultant.org/linux-7-4s-merge-window-is-huge-and-linus-says-it-might-not-just-be-ai/
source_anchor: ""
source_lines: [1, 37]
section: "Langages_Dev"
---
## Linux 7.4’s Merge Window is Huge, and Linus Says it Might Not Just Be AI

The story starts when Linus Torvalds opened the Linux 7.4 merge window more than two weeks ago when he was headed out for his annual family vacation, resulting in some temporary disruptions to his work.

He warned that his schedule would be unusual and that merging might need an extra week, but RC1 still arrived according to the usual two-week schedule.

None of that made the window any smaller. By his count, it brought in **almost 20,000 commits**, where 15,000 or so is the usual number, while the shortlog alone ran to 24,000 lines.

## How big is it?

Phoronix went back through every merge window since Linux 3.1 in 2011 and counted the commits in each, with Linux 7.4 coming out on top at **19,671** and Linux 7.3 holding the previous high at **16,267**.

That did not come out of nowhere, since every window after 7.0 has topped the one before it, with 7.4 beating 7.3 by more than 3,400 commits.

Development cycles across kernel releases show the same trend, with Linux 7.3 being the first to pass the 20,000 commits mark at **20,260** and the 7.4 window coming quite close to it.

## Why?

Linus thinks AI is the reason, pointing to a surge of work set off by AI tools, including bug fixes and a growing number of patches the tools suggested, although those often end up rewritten.

He prefers receiving that work up front in the merge window over dealing with it as late fixes during the release candidate stage. He also hopes that the bigger window means **AI fixes are turning into an ordinary part of development**.

AI is not his only explanation. He says the expectation that 7.4 will be the next long-term support release may have played a minor role in its size, treating it as a separate driving factor.

## What’s included?

AMD GPU header files, which hold register definitions, account for about a third of the patch, and Linus tells people to ignore them. Drivers make up about half the patch, or roughly two thirds with the AMD headers counted.

The rest covers core areas such as *networking*, *Rust*, *BPF*, and *memory management*, along with *filesystems*, tooling like *perf* and *selftests*, and *architecture updates*.

The source tree grew along with it. By Phoronix’s cloc count, the tree stood at 41,605,120 lines ahead of RC1 on November 1, up from 41,019,555 when Linux 7.3 came out.

Actual code accounts for roughly **455,000** lines of that, with the rest being blank lines and comments.

**Suggested Read 📖:** The latest releases of openEuler come with a heavy dose of AI agents.

Source: It's FOSS
