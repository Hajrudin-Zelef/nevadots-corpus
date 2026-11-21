---
id: collect-261121-veille-ia/langages-dev/zig-0-17-0-brings-a-split-build-system-and-fast-rebuilds
title: "Zig 0.17.0 brings a split build system and fast rebuilds"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["build", "release", "notes", "system", "incremental", "rebuilds", "split", "work"]
source: https://dev.to/techaiwire/zig-0170-brings-a-split-build-system-and-fast-rebuilds-19cc
source_anchor: ""
source_lines: [1, 63]
section: "Langages_Dev"
---
The Zig project released Zig 0.17.0 on October 2, 2026, after five months of work. Its headline change is a rebuilt build system that splits setup from execution, plus incremental compilation that works for most projects on x86_64 Linux. Zig is a systems programming language often used as a modern alternative to C. For Zig users, the release means faster rebuilds, but also a long list of breaking changes and a broken editor setup for now.

The release notes say 0.17.0 contains changes from 206 contributors across 925 commits. LWN.net, which covered the release on October 3, notes that the cycle ran larger and longer than first expected. The main pieces:

| Area | Change in 0.17.0 | 
|---|---|
| Build system | Configuration and execution now run as separate processes | 
| Tooling | A new Build Server Protocol for third-party tools | 
| Compilation | Incremental builds for most x86_64 Linux projects | 
| Linker | The ELF linker gains full x86_64 support, SPARC64, libraries and debug info | 
| LLVM | Upgraded to LLVM 22.1.8 | 
| Targets | Adds loongarch32, aarch64-switch, arm-gba and xtensa-linux | 

On LLVM, the notes mention a workaround for loop vectorization that stays in place until LLVM 23. LLVM is the compiler toolkit Zig uses to produce optimized machine code.

A Zig project describes its build in a file called `build.zig`, written in Zig itself. Until now, one process compiled that file and also ran the whole build.

In 0.17.0, that work is split in two. Squared Tech described the design in May, while it was still in development. A "configurer" process compiles `build.zig` in debug mode to work out the build steps. A separate "maker" process, compiled in release mode, then carries out the build.

The split pays off in speed. A blog post by bokvi reports that `zig build -h` dropped from 150 milliseconds to 14.3 milliseconds in May 2026, as the rework landed in the development branch.

Incremental compilation means rebuilding only the code that changed instead of the whole program. The release notes say it now works for most projects that target x86_64 Linux. Developers turn it on with `zig build -fincremental --watch`. The build system then watches the source files and rebuilds almost instantly after each edit.

This builds on months of work. In May, Squared Tech reported incremental rebuilds of 228 to 288 milliseconds on a project whose full build took about 36 seconds. At that point, incremental mode also worked with external libraries and C source files. It still lacked DWARF debug information, the data debuggers use to map machine code back to source lines. The 0.17.0 notes list DWARF support as part of the improved ELF linker.

Incremental mode is off by default and only runs on x86_64 Linux, according to Squared Tech.

Zig has not reached 1.0, and this release breaks a lot of code. The release notes list these among the changes:

- 
`@bitCast` has been redesigned.
- 
`@intFromEnum` and`@enumFromInt` are replaced by`@backingInt` and`@fromBackingInt` .
- Array multiplication syntax is removed.
- The `void{}` syntax is removed.
- 
`errdefer` can no longer capture values.
- 
`@cImport` , the built-in for importing C headers directly, is deprecated.

Two targets were dropped: `powerpc-linux-gnueabi` and `powerpc64-linux-gnu`.

The new process split has a cost. The release notes say the separation of the maker and configurer processes breaks integration with ZLS, the Zig Language Server. ZLS is what gives editors autocomplete and go-to-definition for Zig. The notes say work on the Build Server Protocol is in progress to restore it.

Do not upgrade a working project in the middle of a deadline. The list of removed syntax and renamed built-ins means most nontrivial codebases will need edits. Take extra care with `@bitCast`, since a redesigned conversion can change behavior in ways a quick scan misses. Read that section of the release notes before you rely on old casts.

Keep your editor in mind. If you depend on ZLS for autocomplete, stay on 0.16 until ZLS supports 0.17.0, or accept a plain-text editing session for now.

Try incremental builds if you work on x86_64 Linux. Run `zig build -fincremental --watch` on a branch and measure your own edit-to-run time. Squared Tech's sub-300-millisecond rebuilds came from one project, so your numbers will differ.

If you use Zig mainly as a C compiler or to call C code, check every `@cImport`. It is deprecated in 0.17.0, so plan the move before a later release removes it.

*This article was first published on Tech AI Wire.*

Deutsch · 日本語 · Français · Español · Português

- 0.17.0 Release Notes - Zig
- 0.17.0 Released - Zig
- Zig 0.17 released - LWN.net
- Zig Incremental Compilation: Fastest Builds Revealed - Squared Tech
- Zig in 2026: Colorless Async I/O and the Road to 1.0 - bokvi

## Top comments (0)
