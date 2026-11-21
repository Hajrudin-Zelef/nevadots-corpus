---
id: collect-261121-veille-ia/langages-dev/announcing-rust-1-99-1-daily-dev
title: "Announcing Rust 1.99.1 | daily.dev"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["rust", "rustup", "daily", "bool", "could", "rust-lld", "fixes", "regressions"]
source: https://daily.dev/posts/announcing-rust-1-99-1-bqunvdcku
source_anchor: ""
source_lines: [1, 19]
section: "Langages_Dev"
---
Rust 1.99.1 is a point release that fixes three regressions from prior releases: a missing wcslen symbol causing link errors on certain targets including UEFI, a miscompilation where bool return values crossing FFI boundaries on x86_64 could leave unspecified bits instead of being zero-extended, and a macOS issue where rust-lld could fail to load libLLVM when the compiler was invoked without the rustup proxy. Users can update via rustup update stable, and the team encourages testing beta and nightly channels to catch future regressions.

Questions this post answers

## Why is my Rust bool returning a value other than 0 or 1 through FFI on x86_64?

This was caused by a miscompilation in Rust where functions returning bool through FFI on x86_64 could leave bits 1-7 of the return register unspecified. The return type was not properly marked for zero-extension as required by the ABI, so callers could observe raw values other than 0 or 1. Rust 1.99.1 fixes this by re-marking the bool return type for zero-extension. Teams shipping FFI-heavy Rust code can track compiler regression fixes like this via daily.dev.

## Why does rust-lld fail to load libLLVM on macOS?

In Rust releases prior to 1.99.1, rust-lld could fail to run if the compiler was invoked without the rustup proxy, because there was no direct setting for the library search path and it couldn't find libLLVM.dylib on macOS. The fix in 1.99.1 builds rust-lld with an rpath entry pointing to the directory containing libLLVM.dylib. Debugging linker errors after a Rust upgrade gets easier when release notes like this are easy to find on daily.dev.

## How do I update to Rust 1.99.1?

Run rustup update stable if Rust was installed via rustup; this pulls in the 1.99.1 point release, which fixes regressions including a missing wcslen symbol on certain targets, an FFI bool zero-extension bug on x86_64, and a macOS rust-lld loading failure. Rustup can be installed from the official Rust website for those who don't already have it. Developers tracking Rust point releases to avoid regressions can follow version updates on daily.dev.

1 Comment

Share this post
