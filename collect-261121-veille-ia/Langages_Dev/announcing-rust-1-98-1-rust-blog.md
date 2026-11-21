---
id: collect-261121-veille-ia/langages-dev/announcing-rust-1-98-1-rust-blog
title: "Announcing Rust 1.98.1 | Rust Blog"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-09-03"]
keywords: ["rust", "rustup", "pointer", "announcing", "blog", "vtable", "some", "null"]
source: https://blog.rust-lang.org/2026/09/03/Rust-1.98.1/
source_anchor: ""
source_lines: [1, 21]
section: "Langages_Dev"
---
The Rust team has published a new point release of Rust, 1.98.1. Rust is a programming language that is empowering everyone to build reliable and efficient software.

If you have a previous version of Rust installed via rustup, getting Rust 1.98.1 is as easy as:

`rustup update stable`
If you don't have it already, you can get `rustup` from the appropriate page on our website.

## What's in 1.98.1

Rust 1.98.1 fixes a miscompilation in vtable generation.

In Rust 1.98.0, in some circumstances, rustc would incorrectly generate a trait object vtable with a null pointer where a function pointer should be. This leads to undefined behavior in the emitted code. In some cases this may 'just' cause segfaults due to the null pointer being loaded, but it is possible for it to be justification for arbitrary effects (as is typical for UB).

If you'd like to help us out by testing future releases, you might consider
using the beta (`rustup default beta`) and nightly (`rustup default nightly`) channels locally and in your CI. Please
report any bugs you
might come across!

### Contributors to 1.98.1

Many people came together to create Rust 1.98.1. We couldn't have done it without all of you. Thanks!
