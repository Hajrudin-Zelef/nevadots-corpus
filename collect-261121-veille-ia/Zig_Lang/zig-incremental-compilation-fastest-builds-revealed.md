---
id: collect-261121-veille-ia/zig-lang/zig-incremental-compilation-fastest-builds-revealed
title: "Zig Incremental Compilation: Fastest Builds Revealed"
domain: zig-lang
role: reference
task: article
actors: []
dates: []
keywords: ["incremental", "compilation", "build", "lugg", "external", "libraries", "linker", "thats"]
source: https://www.squaredtech.co/zig-incremental-compilation-gets-fastest-builds-yet
source_anchor: ""
source_lines: [1, 58]
section: "Zig_Lang"
---
- Zig incremental compilation now works with external libraries and C sources on x86_64 Linux, cutting rebuild times to under 300ms.
- The new ELF linker supporting Zig incremental compilation can build the full self-hosted Zig compiler with LLVM and LLD enabled.
- A major build system overhaul splits the build process into a fast configurer and an optimised maker process for dramatically quicker turnarounds.
- Zig 0.17.0 is expected soon, bringing these incremental build improvements to users on stable tagged releases.

## Table of Contents

## Zig Incremental Compilation Is No Longer a Niche Experiment

Zig incremental compilation has moved from a promising prototype to something genuinely useful for day-to-day development. In the latest updates from the Zig core team, contributor Matthew Lugg has pushed the new ELF linker — which quietly debuted in Zig 0.16.0 — to a point where it can handle real-world complexity, including linking against external libraries, C sources, and even the full self-hosted Zig compiler with LLVM and LLD support baked in.

That’s a significant threshold to cross. When the linker first appeared in 0.16.0, it was essentially a skeleton — capable of linking Zig-only binaries with no external dependencies, which is a long way from production-ready. It remained disabled by default, available only behind the **-fnew-linker** flag for the curious and the brave. What’s changed in the weeks since is the kind of steady, unglamorous engineering that makes programming tools actually usable. Zig incremental compilation, in particular, has benefited most visibly from this sustained effort.

## From 36 Seconds to 244 Milliseconds

The numbers tell the story more clearly than anything else. Lugg shared build logs showing the Zig compiler itself being rebuilt using the new incremental system. The first full build takes around 36 seconds — expected, given it’s compiling everything from scratch. Subsequent incremental rebuilds? Between 228ms and 288ms consistently. That’s not a rounding error. That’s roughly a 125x speedup on repeated builds.

For anyone who’s spent an afternoon waiting on compile cycles — tweaking a single function, waiting, tweaking again — that difference is transformative in practice. Lugg demonstrated the same capability on Andrew Kelley’s Tetris clone written in Zig, showing that Zig incremental compilation speed gains aren’t limited to the compiler itself but apply to real application code linking against external libraries.

What makes Zig incremental compilation particularly interesting is that the performance overhead of linking external libraries is essentially zero with the new approach. Previously, incremental compilation in Zig was only viable for pure Zig codebases. That’s not how most real software is built. The ability to bring in libc, LLVM, LLD, or arbitrary C libraries without sacrificing millisecond rebuild times removes a major practical blocker.

### What’s Still Missing

Lugg is upfront about the gaps. The most significant is DWARF debug information — the new ELF linker doesn’t yet generate it for Zig code. For developers who rely on debuggers like GDB or LLDB to step through their programs, that’s a real limitation. Lugg has flagged it as the next priority, so it’s a matter of when, not if. In the meantime, he points out that even without proper debug info, Zig incremental compilation makes print-based debugging significantly more bearable — when you can see the result of a change in under 300ms, the friction of iterating drops dramatically.

The linker is still disabled by default. Users on the Zig master branch running x86_64 Linux can enable it manually, and Lugg expects many projects will already work without issues. Anyone hitting bugs is encouraged to open an issue on the Zig GitHub repository.

## The Build System Overhaul: Maker vs Configurer

Separately — though related in spirit — Zig’s founder Andrew Kelley has landed what he calls a “big branch”: a fundamental restructuring of how the **zig build** command works internally. This is the kind of change that doesn’t sound exciting until you understand what was wrong before.

Previously, everything involved in a build — the user’s **build.zig** logic and the entire build system implementation — was compiled together into a single monolithic process running in Debug mode. Every time your build.zig changed, the whole thing got recompiled. As the build system itself grew more capable (adding **–watch**, **–fuzz**, and **–webui** flags), the cost of that recompilation grew with it.

The new architecture splits this into two separate processes. First, a *configurer* process — small, compiled in debug mode, containing only the user’s build.zig logic — runs and serialises the build graph into a binary configuration file. Then a *maker* process, compiled in release mode with full optimisations, picks up that config file and executes the actual build. Critically, the maker process only needs to be compiled once per Zig version, since it doesn’t contain any user code. It lives in the global cache and is reused across projects.

The practical benefits stack up in three ways. Only the user’s build.zig gets recompiled when it changes, not the entire build system. The build system itself can now grow more sophisticated without making zig build slower for everyone. And in cases where nothing has changed — for example, adding **-freference-trace** to the command line — the system can skip re-running build.zig entirely and use the cached configuration directly. This architectural separation also creates a cleaner foundation for Zig incremental compilation to be wired deeper into the build pipeline in future releases.

### The Benchmark Makes the Case

Kelley shared benchmarks for something as simple as **zig build –help**. Before the change, running help on the master branch clocked a mean wall time of 150ms with 995 million CPU instructions. The post-change figures show meaningful reductions in CPU cycles and cache pressure — and that’s for a trivially simple operation. For more complex build graphs, the gains compound.

It’s also worth thinking about what this enables for the future. A build system that pays no penalty for adding new features is one that can evolve freely. Kelley and the team have been adding increasingly sophisticated tooling — fuzzing integration, a web UI for build monitoring — without Zig’s core value proposition of being fast getting undermined. That’s a good architectural bet.

## Why This Matters Beyond Zig’s Own Ecosystem

Zig is still a young language. Its official site describes it as a general-purpose systems programming language positioning itself as a successor to C — not Rust, notably, though comparisons are inevitable. It’s found an interesting niche as both a standalone language and a cross-compilation toolchain used by developers in other ecosystems, including JavaScript (via Bun) and even some Rust projects that use zig cc as a C compiler wrapper.

The toolchain quality — build speed, linker behaviour, cross-compilation reliability — matters to that broader audience just as much as to Zig-native developers. When Kelley’s build system changes make **zig build** faster and smarter, that has downstream effects across every project using Zig’s infrastructure as a dependency. Zig incremental compilation, as it matures, stands to benefit that wider ecosystem too.

The broader industry trend here is the renewed focus on developer iteration speed. Whether it’s Rust’s incremental compilation work, Go’s already-fast compile times, or the various hot-reload systems emerging in frontend tooling, the message from developers is consistent: waiting on builds is dead time, and toolchains that minimise it win adoption. Zig incremental compilation makes a credible case that Zig takes this seriously at the architectural level, not just as an afterthought.

## When Can You Actually Use This?

If you’re on the Zig master branch and running x86_64 Linux, Zig incremental compilation is available today via **-fincremental –watch** combined with the new linker flag. Lugg expects broad compatibility with existing codebases.

For everyone else — the majority of Zig users who track stable tagged releases — Kelley has confirmed that Zig 0.17.0 is close. The build system changes Kelley described are already merged into main. That means the next stable release will ship with the maker/configurer split, faster **zig build** performance, and at minimum a much more capable experimental ELF linker for those willing to enable it.

The gap between what’s possible on master and what lands in a stable release has historically been where Zig does its most interesting work. If the current pace of linker progress holds — DWARF support being the next milestone — 0.17.0 could be the release where Zig incremental compilation stops being a feature to watch and starts being a reason to switch.
