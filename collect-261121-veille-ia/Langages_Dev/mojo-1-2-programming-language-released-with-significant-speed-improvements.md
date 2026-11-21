---
id: collect-261121-veille-ia/langages-dev/mojo-1-2-programming-language-released-with-significant-speed-improvements
title: "Mojo 1.2 programming language released with significant speed improvements"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["speed", "mojo", "released", "functions", "faster", "compilation", "memory", "linux"]
source: https://dev.to/hacksgr/mojo-12-programming-language-released-with-significant-speed-improvements-45pi
source_anchor: ""
source_lines: [1, 35]
section: "Langages_Dev"
---
Mojo 1.2 is out with pattern matching, native math functions for CPUs and GPUs, faster nn package compilation, and jemalloc memory allocator integration for Linux.

The most important addition in version 1.2 is support for pattern matching through new checking commands. This feature allows for checking values at compile time and extracting elements from lists.

In the core math library, more than 20 functions—such as tangent, logarithm, and other mathematical operations—now have native implementations. These functions can run on both central processors and graphics cards.

Compilation speed has been significantly improved. Compiling the machine learning package now takes about 61 seconds instead of 98 seconds, a reduction of almost 38%.

READ ALSO

Kali Linux 2026.3 released with new tools and upgrades

For Linux users, changing the memory management system to jemalloc improves compilation speed for large graphics card files by about 10% to 13%.

Additionally, text editing functions became much faster, with character addition operations being up to 86 times faster in some cases and text repetition about twice as fast for large texts.

The new version also brings:

- Table slicing capabilities and improved tuple handling.
- New methods for memory areas.
- Clearer restrictions on general planning through updated rules.
- Ability for structures to be explicitly excluded from the use of specific features.
- Expanded documentation with updated explanations and a revised guide for developers coming from Python.

G

Hacks.gr on Google

Set it as a preferred source for cybersecurity updates.

Preferred source

end of article

## Top comments (0)
