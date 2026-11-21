---
id: collect-261121-veille-ia/langages-dev/mold-3-0-linker-rust-rewrite-rust-1-95-no-onetbb
title: "Mold 3.0 linker: Rust rewrite, Rust 1.95, no oneTBB"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-10-05"]
keywords: ["mold", "linker", "rust", "same", "rewrite", "version", "linux", "default"]
source: https://techaiwire.com/articles/mold-3-0-rust-rewrite-drop-in-linker/
source_anchor: ""
source_lines: [1, 82]
section: "Langages_Dev"
---
# Mold 3.0 ships its Rust rewrite as a drop-in linker

Mold 3.0 is the first release written in Rust. It needs Rust 1.95, drops Intel's oneTBB and links as fast as 2.42.1, the last C++ version.

Drafted with AI from the sources linked in this article, then fact-checked and edited by Claire Whitmore before publishing. How we work

By the numbers

- minimum Rust version needed to build mold 3.0
- 1.95
- the last C++ release, which 3.0 replaces
- 2.42.1
- mold's median speedup over LLVM lld, per its README
- 4.9x

Mold 3.0, the first version of the fast Linux linker written in Rust, was released on October 5, 2026. Its author, Rui Ueyama, rewrote the whole program from C++. The release notes promise a clean swap: the same options, the same output and the same speed as mold 2.42.1, the last C++ version. That matters because mold now wants to become the default linker on Linux.

## What a linker does and where mold fits

A linker is the last step of building a program. The compiler turns each source file into an object file. The linker then stitches those pieces into one program you can run. On big projects this step can take many seconds, and it runs again after every small change.

Most Linux systems use GNU ld, the linker from the GNU project, by default. LLVM's lld is a faster alternative. Mold was built to beat both. Its repository lists these link times from tests run in August 2026:

| Program | mold | LLVM lld | Speedup | 
|---|---|---|---|
| Chromium 145 | 1.65 s | 16.64 s | 10.1x | 
| TensorFlow 2.21 | 3.15 s | 50.73 s | 16.1x | 
| ClickHouse 26.1 | 0.98 s | 6.62 s | 6.7x | 

Those numbers were measured on the C++ version. The 3.0 release notes say linking speed "remains on par with 2.42.1," so the Rust version claims the same results. Ueyama first laid out the plan in September, when mold's Rust rewrite set its sights on becoming Linux's default linker.

## What changed in mold 3.0

For anyone who simply calls mold, nothing should look different. According to the GitHub release notes and Phoronix, mold 3.0:

- accepts the same command-line options as 2.42.1
- supports the same CPU architectures, including x86-64, AArch64, RISC-V and LoongArch
- produces the same output, except where bugs were fixed
- links at the same speed as 2.42.1

The real changes are in how mold itself is built. It now uses Cargo, Rust's standard build tool, instead of CMake. Building it requires Rust 1.95 or later.

It also drops oneTBB, an Intel library that splits work across processor cores. That was one more package that Linux distributions had to ship next to mold.

## Why rewrite a working linker in Rust

The main reason is safety. A linker reads object files that it did not create. It then does a great deal of low-level memory work on them.

In C++, one broken input file can make that code read memory it should never touch. Hardware Busters reported in September that this is exactly where a memory bug turns into a security bug. Rust checks those reads automatically. Phoronix says the result is that mold is now safer when it handles corrupted input files.

## What comes next: linker scripts and the default slot

The 3.x series has one clear target. The project's stated goal is to "close the remaining compatibility gaps with GNU ld, particularly in linker script support." It also wants "to pave the way for mold to be adopted as the default linker in Linux distributions."

Linker scripts are small files that tell a linker exactly where to place code in memory. Operating system kernels, bootloaders and firmware for small devices depend on them. Mold could not link those programs before, Hardware Busters reported. Until it can, distributions cannot make it the system default.

## What this means for developers

Treat 3.0 as a drop-in, then verify it. If you already use mold, your build flags should not need to change. Link one real project with both 2.42.1 and 3.0 first. Then run your full test suite before you switch your CI machines.

Check your compiler if you build mold yourself. Rust 1.95 or later is required, so the older Rust packaged by some long-term-support distributions may not build it. Packagers lose the oneTBB dependency but take on a Cargo build.

Measure your own link times. The speedups in the table come from mold's own repository, and they were measured before the rewrite. Time a full build and a small incremental rebuild on your own code before and after the switch.

Watch the distributions. If a major distribution makes mold its default linker, every build on that system will use it without anyone asking. That decision now depends on linker-script support in the 3.x series. Kernel and embedded developers are the group to watch first.

## Sources

1. Mold 3.0 High Speed Linker Released Following Rust Rewrite - Phoronix
2. Release mold 3.0.0 - GitHub
3. rui314/mold - a modern linker - GitHub
4. The Mold Linker Is Being Rewritten in Rust, and It Wants GNU ld's Job - Hardware Busters

## Related articles

Mold links 4.9x faster than LLVM lld. Its author is rewriting it in Rust and wants distributions to install it as the default linker, /usr/bin/ld.

COSMIC Epoch 1.11 keeps modal dialogs on top with Wayland's xdg-dialog-v1 and sends games to the right GPU on hybrid laptops, 12 days after 1.10.

## The weekly digest

One email every Friday with the week's top stories from all six desks: AI, dev tools, coding, the tech industry, startups and what's next. Free, no spam.
