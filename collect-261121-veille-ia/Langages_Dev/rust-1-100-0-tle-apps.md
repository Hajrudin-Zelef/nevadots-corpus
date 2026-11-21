---
id: collect-261121-veille-ia/langages-dev/rust-1-100-0-tle-apps
title: "Rust 1.100.0 | Tle Apps"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["type", "allocator", "stable", "rust", "never", "feature", "nightly", "instead"]
source: https://tleapps.com/version/rust/rust-1-100-0/
source_anchor: ""
source_lines: [1, 44]
section: "Langages_Dev"
---
**Verdict up front:** this is one of the few Rust releases in 2026 worth scheduling real time for. Two long-running nightly features landed on stable at once: the never type and the allocator API. I moved three workspaces to 1.100.0 over a day, and the only friction came from crates that had been papering over `!` with manual panics and a pile of `unreachable!()` calls.

## The Never Type Is Real Now

`!` graduated from a perma-unstable feature on November 12 to a type you can write in signatures and generic positions. The stabilized surface includes `impl AsRef<T> for !`, which tells you how far the team went: it now participates in bounds instead of merely type-checking as a bottom type.

Practically, that means diverging functions can declare `-> !` honestly, and generic code that returns early through a panic no longer needs a sentinel type to satisfy the compiler. If your crate had `#![feature(never_type)]` in it, delete the attribute and the `#![cfg_attr]` guard around it. That cleanup alone removed a dozen lines from one of my workspace roots.

## Custom Allocators, Finally Stable

The `allocator_api` feature has been on nightly since before most current Rust jobs existed. As of 1.100.0 the `Allocator` trait, `Global`, `System`, and the `_in` constructors are stable: `Box::new_in`, `Vec::new_in`, `Vec::with_capacity_in`, plus accessors like `Vec::allocator()` and `Box::allocator()`.

The blanket impls matter as much as the constructors. `Rc`, `Arc`, and `Box` now forward `Allocator`, so collections held behind a smart pointer keep the arena you gave them instead of silently falling back to the global heap. If you build embedded or game-engine style code with arena allocators, this is the release where you stop maintaining a fork.

## Language and Lints

- **Repetition of `#[repr(..)]`** is now linted, so accidental stacked attributes surface instead of being silently merged.
- **`aligned_fields_in_packed`** turns the old hard error for over-aligned fields inside a packed type into a deny-by-default lint, which gives`#[expect]` and`#[allow]` a hook.
- **`drop_in_place` coverage** for the three`dropping_*` lints, which catches UB-adjacent mistakes in raw unsafe code.
- **`extern "custom"`** is stable for targets that define their own calling convention.
- **Volatile accesses** to non-AM memory are now allowed to trap, closing a soundness hole in`core::ptr::volatile_*` .

## Compiler and Platform Moves

`-Cprofile-sample-use` is stable, so sample-based PGO no longer needs a nightly toolchain. The new trait solver is on by default in orphan-check paths, which is the kind of change that usually shows up as a slightly different set of coherence errors rather than a build failure.

On the platform side, `i686-pc-windows-gnu` dropped to Tier 2 without host tools, `wasm32-wasip3` moved up to Tier 2, and `powerpc64-sony-ps3` joined Tier 3. If your CI did cross builds for 32-bit Windows GNU and assumed host binaries were present, that pipeline needs attention this week, not next quarter.

## Small Standard Library Comforts

- `bool::toggle` replaces the`!self.flag` dance.
- `Box::take` ,`Box::map` ,`Rc::map` , and`Arc::map` let you transform the inner value without unwrapping.
- `PartialEq<Cow<[U]>>` for`Vec` and slices removes a`.as_ref()` at comparison sites.
- `Default for std::sync::Once` and`Child::main_thread_handle` on Windows fill smaller gaps.

## How I Upgraded

1. Ran `rustup update stable` and confirmed`rustc 1.100.0` before touching any manifest.
2. Built the workspace with the old toolchain flag still available, so I could bisect any failure to a single crate.
3. Grep for `#![feature(` and removed the never type and allocator gates.
4. Checked `#[repr(...)]` stacks and packed structs, since both now produce warnings that CI may treat as errors.
5. Ran the full test suite, including the FFI crate, because ABI-adjacent changes are where point releases bite.

Nothing in my test suite failed. That is unusual for a release this large, and it says something about how long both features sat in nightly. The work that remains is cleanup: deleting feature gates, replacing sentinel types with `!`, and deciding whether an arena allocator is worth wiring into the hot path.
