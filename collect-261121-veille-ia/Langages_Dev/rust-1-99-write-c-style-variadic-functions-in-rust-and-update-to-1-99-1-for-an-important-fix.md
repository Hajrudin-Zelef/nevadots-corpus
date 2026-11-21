---
id: collect-261121-veille-ia/langages-dev/rust-1-99-write-c-style-variadic-functions-in-rust-and-update-to-1-99-1-for-an-important-fix
title: "Rust 1.99: write C-style variadic functions in Rust — and update to 1.99.1 for an important fix"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["rust", "functions", "update", "stable", "release", "team", "october", "c-style"]
source: https://www.jmtechpower.com/article/80
source_anchor: ""
source_lines: [1, 61]
section: "Langages_Dev"
---
*News · written 8 November 2026. A summary for readers, not
technical advice — check the sources at the end for the details.*

**Rust** is a free, open-source programming language
known for speed and memory safety. It is used for command-line tools,
desktop apps, web services and parts of operating systems. A new stable
version comes out every six weeks; the latest is
**Rust 1.99**.

## When

The Rust Release Team announced **Rust 1.99.0** on
**1 October 2026** and a fix release, **Rust
1.99.1**, on **15 October 2026**. If you already have
Rust installed with rustup, one command updates it:

`rustup update stable`
## What is new in 1.99

- **C-style variadic functions written in Rust.** Rust
could already*call* C functions that take a variable number of
arguments (like`printf` ). Now you can also*define* such functions in Rust, for the`"C"` and`"C-unwind"` ABIs, with the extra arguments read
through a`VaList` . That makes it easier to replace C code
piece by piece.
- **Layout information from raw pointers:**`Layout::for_value_raw` ,`mem::size_of_val_raw` and`mem::align_of_val_raw` are now stable.
- **More stable library functions** , among them`Vec::into_parts` and`Vec::from_parts` ,`Box::into_non_null` ,`VecDeque::retain_back` ,`String::from_utf8_lossy_owned` and`std::fs::set_times` .

The team notes that there are "no changes to the language semantics" in 1.99 beyond these additions.

## Why 1.99.1 matters

Two weeks later, 1.99.1 fixed three problems that had slipped in:

- On **x86-64** , Rust functions returning a`bool` to other languages (through FFI) could leave part of the
return value undefined — a**miscompilation** , meaning the
compiler could produce wrong code.
- Some targets, including UEFI, got link errors about a missing
`wcslen` function.
- On macOS, the bundled `rust-lld` linker could fail to start
when run without the rustup proxy.

## What it means for small publishers

If you build and ship software with Rust, update to
**1.99.1** rather than staying on 1.99.0 — especially if
your program exposes functions to C or other languages on x86-64 — and
rebuild your release files so your users get the fixed code.

## Sources

- The Rust Release Team — Announcing Rust 1.99.0, 1 October 2026 (features, stable APIs, update command).
- The Rust Release Team — Announcing Rust 1.99.1, 15 October 2026 (the three fixes).

More news and guides at https://jmtechpower.com/articles.

## Comments (0)

Please login to post comments

Login
No comments yet. Be the first to comment!
