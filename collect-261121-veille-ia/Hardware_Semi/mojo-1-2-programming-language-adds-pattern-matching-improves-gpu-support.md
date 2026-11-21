---
id: collect-261121-veille-ia/hardware-semi/mojo-1-2-programming-language-adds-pattern-matching-improves-gpu-support
title: "Mojo 1.2 Programming Language Adds Pattern Matching, Improves GPU Support"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["mojo", "programming", "language", "pattern", "matching", "also", "developers", "improves"]
source: https://linuxiac.com/mojo-1-2-programming-language-adds-pattern-matching-improves-gpu-support/
source_anchor: ""
source_lines: [1, 21]
section: "Hardware_Semi"
---
Following Mojo 1.1, which opened the compiler to external contributions, the Mojo programming language, designed to combine Python-like syntax with the performance and control of systems programming languages, has released version 1.2.

The most notable addition is pattern matching through new `match` and `case` statements. Developers can now compare values against different patterns, including literals, tuples, structures, and conditional guards, executing code based on the first successful match.

The feature also supports `comptime match` for compile-time evaluation, letting developers select implementations based on values or types known during compilation. Mojo 1.2 also introduces `if case` and `while case` constructs, plus list destructuring to extract elements from collections.

Another improvement concerns GPU computing. More than 20 functions in Mojo’s standard math library now use native Mojo implementations instead of relying on the system’s math library. As a result, functions such as `atan()`, `tan()`, `log10()`, `hypot()`, and several Bessel functions can execute on GPUs as well as CPUs.

Using shared implementations across both architectures improves numerical consistency, though some results may still differ slightly depending on hardware.

On the performance side, the compiler now handles large packages much faster. According to developers, compiling the `nn` package takes about 61 seconds, down from 98 seconds, a reduction of nearly 38%.

Linux users also benefit from the compiler’s switch to the jemalloc memory allocator, improving compilation speed for large GPU kernel files by about 10% to 13%.

The standard library has also received performance optimizations. String padding operations can be up to 86 times faster in some cases, while string repetition is about twice as fast for large strings.

Other changes include new array slicing capabilities, improved tuple handling, and additional methods for working with memory spans. Generic programming also gets clearer constraint diagnostics through updated `where` clauses, while structures can now explicitly opt out of implementing particular traits.

Finally, the release expands the language documentation, including updated explanations of pattern matching, control flow, and language syntax, along with a revised guide for Python developers transitioning to Mojo.

For more information, see the release notes.
