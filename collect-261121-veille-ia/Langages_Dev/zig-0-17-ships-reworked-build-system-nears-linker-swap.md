---
id: collect-261121-veille-ia/langages-dev/zig-0-17-ships-reworked-build-system-nears-linker-swap
title: "Zig 0.17 Ships Reworked Build System, Nears Linker Swap"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["build", "linker", "system", "linuxiac", "support", "release", "linux", "incremental"]
source: https://sharedot.com/story/uitm_602e5219af304c7f91bb68b573d5bf7a/zig-0-17-ships-reworked-build-system-nears-linker-swap
source_anchor: ""
source_lines: [1, 43]
section: "Langages_Dev"
---
Zig 0.17 arrives with a rebuilt build system, faster binary caching and an ELF linker that could fully replace the legacy one next release.

### A systems language hits 0.17 with big under-the-hood moves

Linuxiac reports that Zig 0.17 is out, with the biggest progress in two places: the new ELF linker introduced in the previous release, and the build system.

The linker has gained full x86_64 and SPARC64 support plus partial LoongArch support, and Linuxiac says it can now generate static and shared libraries, handle GNU symbol versioning, DWARF debugging information, GOT generation, copy relocations and symbol hash tables — the pieces required for real-world Linux software.

It is still not the default because it does not fully match the older ELF implementation's feature set, but developers say it can already build most Zig projects targeting x86_64 Linux, and the goal is to retire the legacy linker entirely in the next release.

Under the hood, the LLVM-based toolchain is upgraded to LLVM and Clang 22.1.8, glibc 2.44 is available for cross-compilation, and the release bundles Linux kernel 7.2 headers while continuing to ship musl 1.2.5 with backported fixes, according to Linuxiac.

### Incremental compilation finally pairs with watch mode

The linker progress feeds directly into the feature working programmers care most about day to day: incremental compilation. According to Linuxiac, Zig 0.17 significantly improves it, letting projects rebuild only the parts affected by source changes instead of recompiling everything.

For most x86_64 Linux projects, incremental compilation can now be used together with Zig's watch mode, so the build system detects code changes and performs near-immediate incremental rebuilds.

That combination is the kind of feedback-loop improvement that typically decides whether contributors enjoy hacking on a codebase or drift away.

Linuxiac also highlights platform work: the release adds loongarch32-linux targets, brings generally usable 64-bit SPARC support, introduces very early xtensa-linux support, and improves support across OpenBSD, FreeBSD, NetBSD, ARM and AArch64.

Elsewhere in the backends, Linuxiac notes the SPIR-V linker has been rewritten to support incremental compilation and linking of external SPIR-V object files, the SPIR-V compiler backend is now multithreaded, and early work continues on self-hosted LoongArch and AArch64 backends.

### Build system splits into two processes — and briefly breaks ZLS

Linuxiac reports that the configuration stage and the part that executes the build graph now run as separate processes, and the caching system has been reworked: Zig now uses a binary cache format about 25% smaller, with cache hits 5% to 10% faster in testing.

Alongside that comes a new Build Server Protocol designed to give external tools such as IDEs access to the build graph, build steps, options, dependencies, generated files and build status.

There is a short-term cost — Linuxiac notes the architectural changes currently break some functionality in ZLS, the Zig Language Server, and the Zig and ZLS teams are working together to extend the new protocol so the language server can regain and eventually improve its previous integration.

Language-level changes include a revised @bitCast, removal of the old array multiplication syntax in favor of @splat, and the removal of @cImport, deprecated in 0.16, with C-to-Zig translation moving to an official external translate-c package maintained by the Zig Software Foundation.

### Why it matters: systems languages push into critical code

Zig's push to make its toolchain boringly reliable for production Linux software lands in the same week that rival Rust cleared a milestone in the most demanding corner of safety-critical software.

According to Rail Business UK, Derby-based scale-up Universal Signalling has signed a partnership with the Ferrocene team to develop what it calls the first railway certification package for Rust complying with the EN 50716 software certification standards.

Universal Signalling develops its products primarily in Rust on QNX OS for Safety, and Ferrocene will provide a Rust integration layer for QNX plus qualification materials for its toolchain, with the certification package expected in an upcoming Ferrocene release, per Rail Business UK.

For Zig, the stated path forward is retirement of the legacy ELF linker in the next release, more incremental compilation coverage, and repaired language-server integration — the slow, deliberate work of a language inching toward production trust.
