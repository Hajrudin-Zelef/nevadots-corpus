---
id: collect-261121-veille-ia/langages-dev/zig-0-17-programming-language-lands-with-reworked-build-system
title: "Zig 0.17 Programming Language Lands With Reworked Build System"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["build", "language", "system", "support", "linux", "changes", "been", "linker"]
source: https://linuxiac.com/zig-0-17-programming-language-lands-with-reworked-build-system/
source_anchor: ""
source_lines: [1, 31]
section: "Langages_Dev"
---
Zig, a general-purpose systems programming language, has just released version 0.17 with major improvements to its ELF linker and build system, and expanded incremental compilation support for x86_64 Linux projects.

One of the biggest areas of progress is Zig’s new ELF linker, which was introduced in the previous release and has now gained full x86_64 and SPARC64 support, along with partial LoongArch support.

It can now generate static and shared libraries, handle GNU symbol versioning, DWARF debugging information, GOT generation, copy relocations, symbol hash tables, arbitrary section alignment, and other elements required for real-world Linux software.

The linker is not yet the default because it does not fully match the feature set of Zig’s older ELF implementation. However, developers say it can already build most Zig projects targeting x86_64 Linux. The goal is to retire the legacy ELF linker entirely in the next release.

That progress also directly affects incremental compilation. Zig 0.17 significantly improves this feature, allowing projects to rebuild only parts affected by source changes instead of recompiling everything.

For most x86_64 Linux projects, incremental compilation can now be used together with Zig’s watch mode, allowing the build system to detect code changes and perform near-immediate incremental rebuilds.

Another major area of work is the Zig build system. The configuration stage and the part that executes the build graph now run as separate processes. The caching system has also been reworked: Zig now uses a binary cache format about 25% smaller, and cache hits are 5% to 10% faster in testing.

Alongside that comes the new Build Server Protocol, designed to give external tools such as IDEs access to the build graph, build steps, options, dependencies, generated files, and build status.

There is one short-term downside: the architectural changes currently break some functionality in ZLS, the Zig Language Server. The Zig and ZLS teams are working together to extend the new protocol so the language server can regain and eventually improve its previous integration.

There are numerous language-level changes as well. Among them, the behavior of `@bitCast` has been revised, the old array multiplication syntax has been removed in favor of `@splat`, and several older or deprecated language constructs have been dropped.

Another notable change concerns C interoperability. `@cImport`, deprecated in Zig 0.16, has now been removed. C-to-Zig translation is moving to an official external translate-c package maintained by the Zig Software Foundation, allowing it to evolve independently from the main Zig compiler.

On the platform support side, the release adds `loongarch32-linux` targets, brings generally usable 64-bit SPARC support, introduces very early `xtensa-linux` support, and improves support across OpenBSD, FreeBSD, NetBSD, ARM, AArch64, and several less common architectures.

Under the hood, Zig 0.17 upgrades its LLVM-based toolchain to LLVM and Clang 22.1.8. For cross-compilation, glibc 2.44 is now available. The release also bundles Linux kernel 7.2 headers. Zig continues shipping musl 1.2.5 with backported security and portability fixes.

Just for reference, the minimum supported Linux version for Zig’s standard library remains Linux 5.10.

Beyond the headline changes, the SPIR-V linker has been rewritten and can now perform incremental compilation and link external SPIR-V object files. The SPIR-V compiler backend is also now multithreaded. Finally, early work continues on self-hosted LoongArch and AArch64 compiler backends.

More details about all changes in Zig 0.17 are available in the project’s official release notes.
