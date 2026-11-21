---
id: collect-261121-veille-ia/langages-dev/mold-3-0-is-a-rust-rewrite-aimed-at-default-linker-status
title: "Mold 3.0 is a Rust rewrite aimed at default linker status"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-10-05"]
keywords: ["linker", "mold", "rust", "default", "rewrite", "output", "version", "project"]
source: https://betanews.com/article/mold-3-rust-rewrite-linker/
source_anchor: ""
source_lines: [1, 27]
section: "Langages_Dev"
---
Released on October 5, 2026, Mold 3.0 is the first version of the high-speed linker written in Rust. Version 2.42.1 was the last release of the C++ codebase, and the project's GitHub announcement describes 3.0 as a drop-in replacement for it.

Next up for the 3.x series is an effort to “close the remaining compatibility gaps with GNU ld, particularly in linker script support,” the page states. Preparing Mold for adoption as the default linker in Linux distributions is the other stated aim.

Command-line options and target architectures match those of 2.42.1. Output stays identical apart from the listed bug fixes, and linking performance remains on par with the earlier release.

Testing ran the suite on every supported target and compared linker output across a broad range of real-world workloads and option combinations. Every Gentoo package was built as well, and the project found no regressions.

The C++ version could read memory out of bounds on corrupted input files and crash with a segmentation fault. Reads are now bounds-checked in 3.0, and Mold stops with a panic at the faulty access. Fixes for many bugs landed during the rewrite, and the full list appears on the GitHub page, where Mold 3.0 is available to download.

Anyone building Mold from source now needs Rust 1.95 or later plus a C compiler. Cargo, Rust's build system, replaces CMake, and the old CMake options have been removed. No dependency on oneTBB remains.

A linker is the final build step that combines compiled object files into one executable or shared library, the project's README explains. Linking can take considerable time when the build output is large. Positioned against GNU gold, GNU ld and LLVM's lld, Mold is an open-source linker.

Rui Ueyama announced the rewrite in a September 1 post on X and said support for linker scripts is being added too.

We are rewriting the mold linker in Rust and adding linker script support so that mold can link essentially anything GNU ld can, including kernels and embedded programs. This will be mold 3.0. We started this effort to get Linux distros to replace their default linker with mold.

— Rui Ueyama (@rui314) September 1, 2026

Full details followed in the 2.42.1 announcement, and from a user's perspective the stated goal is for 3.0 to “continue to work just as before.”

Most Linux distributions still ship GNU ld as the system linker at /usr/bin/ld, the 2.42.1 post says. Closing the linker script gaps comes first on the path to linking kernels and firmware as well as user-space programs, the post adds. Linker scripts are files that describe how input sections map into the output file and control its memory layout, according to GNU's documentation.

Marcus Pruitt covers Windows, desktop software and the open-source tools that help people get more from their computers. His work focuses on solving practical problems, from failed updates and confusing system settings to clean installations, bootable media and everyday performance issues. Marcus approaches technical advice from the reader’s point of view: explain what is happening, identify the safest fix and make every step easy to follow. He is especially interested in useful software that gives users greater control over their devices. When he is not troubleshooting Windows, he keeps an eye on the tools and changes shaping the wider PC ecosystem.

## Discussion
