---
id: collect-261121-veille-ia/langages-dev/mold-3-0-is-out-rewritten-from-c-to-rust-daily-dev
title: "Mold 3.0 is out, rewritten from C++ to Rust | daily.dev"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["mold", "rust", "daily", "build", "rewritten", "version", "rewrite", "toolchain"]
source: https://daily.dev/posts/mold-3-0-is-out-rewritten-from-c-to-rust-fr0z3ewr8
source_anchor: ""
source_lines: [1, 31]
section: "Langages_Dev"
---
# Mold 3.0 is out, rewritten from C++ to Rust

•2 sources

## Community take

335

comments

Reaction splits into two threads: one group flags the AI-assisted Rust rewrite as controversial tagging/labeling (is this 'vibecoding'?), while a much larger discussion fixates on the practical fallout for Linux distro bootstrapping, where the switch from C++ to Rust breaks early-stage build chains that relied on mold being buildable without a Rust toolchain.

Deep dive

Questions this post answers

## What changed in Mold 3.0 and is it still compatible with the C++ version?

Mold 3.0.0 is the first release written entirely in Rust, replacing the prior C++ codebase at version 2.42.1. It remains a drop-in replacement, accepting the same command-line options and supporting the same target architectures with comparable linking speed, verified by running test suites and building every Gentoo package with no regressions found. Teams planning the switch to mold 3.0 can track compatibility notes and migration details on daily.dev.

## What build system and toolchain does mold 3.0 require?

Mold 3.0 replaces CMake with Cargo as its build system and requires Rust 1.95 or later plus a C compiler. The Intel oneTBB dependency has been removed, and install scripts along with environment variables like MOLD_LIBDIR have changed, which packagers need to account for when updating build pipelines. Packagers updating build scripts for the new toolchain can follow mold's changes via daily.dev.

## How does mold 3.0 handle corrupted or malformed input compared to the old C++ version?

In the Rust rewrite, mold panics on corrupted input instead of risking a crash from an out-of-bounds read, which was possible in the C++ version. This is a direct consequence of switching the implementation language, improving safety around malformed linker input. Developers evaluating linker safety guarantees can follow mold's Rust rewrite progress on daily.dev.

2 Comments

Share this post
