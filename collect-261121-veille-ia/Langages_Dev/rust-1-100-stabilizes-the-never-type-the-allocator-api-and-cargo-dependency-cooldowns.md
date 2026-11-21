---
id: collect-261121-veille-ia/langages-dev/rust-1-100-stabilizes-the-never-type-the-allocator-api-and-cargo-dependency-cooldowns
title: "Rust 1.100 Stabilizes the Never Type, the Allocator API and Cargo Dependency Cooldowns"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-12"]
keywords: ["type", "never", "cargo", "allocator", "rust", "code", "dependency", "stabilizes"]
source: https://vinish.dev/rust-1-100-never-type-allocator-api-cargo-cooldowns
source_anchor: ""
source_lines: [1, 60]
section: "Langages_Dev"
---
The Rust Release Team shipped Rust 1.100.0 on November 12, 2026. The headline change is the never type, written as an exclamation mark, which can now be named anywhere in stable code after a decade of work and five abandoned attempts.

The release also stabilizes the core of the custom allocator API, adds a DropGuard helper for cleanup code, and gives Cargo a way to hold back freshly published crates. That last feature is a practical defense against supply chain attacks, so it is worth a look even for teams that do not care about type system details.

## The never type reaches stable Rust

The never type describes a computation that does not produce a value at all. Expressions such as return, break and a call to process exit already had this type, and functions could declare a never return. What was missing was the right to use it in other positions.

From 1.100 onward you can write types like Result with a never error parameter. That lets a function state, inside its signature, that an error branch cannot happen, and the compiler then accepts an irrefutable pattern that pulls the success value out without any unwrap call.

To make this fit with older code, the standard library now defines Infallible as an alias for the never type. Every API that took Infallible therefore works with the new spelling, which was the plan from the day Infallible was added.

## A breaking change for older editions

Turning Infallible into an alias required one compatibility break. In the 2015, 2018 and 2021 editions, the type the compiler picks when it falls back from never was the unit type. It now becomes the never type, matching what edition 2024 already does.

The team says most projects will not notice. Rare cases can break, such as code that discards the result of a generic function, and the compiler may now flag more code as unreachable. Warnings about this were issued well in advance, and fixes were sent to affected public crates, so updating a failing dependency is the first thing to try.

## Allocator API, DropGuard and other library additions

The core of the allocator API is now stable through a new Allocator trait. Library authors can plug their own allocators into Vec and Box, using constructors such as Vec::new_in and Box::new_in. The project calls this the foundation of its Allocators 1.0 goal, with more collections and pointer types to follow.

Other additions worth knowing:

- **core::mem::DropGuard** wraps a value together with a closure that runs when the guard goes out of scope, on any exit path. A dismiss method hands the value back without running the closure, which makes rollback logic short and clear.
- **std::mem::conjure_zst** is an unsafe function that builds an instance of a type that occupies no memory, replacing transmute tricks and uninitialized memory hacks used in the past.
- New helpers including trim_prefix and trim_suffix for strings, slices and paths, bool::toggle, Vec::from_fn, Box::take, and map methods on Box, Rc and Arc.
- A Default implementation for std::sync::Once and new unchecked accessors on UnsafeCell.

## Cargo changes: unused dependencies, cache layout and cooldowns

Cargo now warns when a dependency listed in Cargo.toml is not used. The lint is on by default and lives in a new lint system configured through a lints.cargo table. It only fires when every build target that could use the entry has been built, and it never fires for dev-dependencies.

The build directory layout has also been reorganized. Files are now grouped by build step, and the configuration hash sits on the parent folder rather than on each file name. This removes extra or missed rebuilds when features differ, but scripts that relied on the old paths may need changes.

The most useful new setting for many teams is min-publish-age. You set a global minimum age in the registry section of .cargo/config.toml, for example three days, and Cargo skips any crate version younger than that during resolution. It reports which newer versions it held back, and an environment variable can override the check for one urgent update.

There are limits. A version that lacks a publish timestamp in the registry index is not delayed, and cargo install ignores the setting entirely. The Rust team also stresses that a cooldown reduces risk but does not replace reviewing dependency updates.

## What it means for developers

For application teams, the safest first step is to update a branch, run the full test suite and watch for new unreachable code warnings or inference errors caused by the never fallback change. Projects still on the 2021 edition or older are the ones most likely to see differences.

The dependency cooldown deserves attention from anyone running Rust services in production. Poisoned crate releases tend to be spotted and pulled soon after they appear, so a waiting period of a few days blocks a large share of poisoned releases at almost no cost. Pair it with a plan for urgent security updates, since those will need the manual override.

Build engineers should check CI caching and any scripts that read files from the target or build directory, because the new layout moves things around. The unused dependency warning is also a cheap way to trim compile times on large workspaces.

## How to upgrade

If Rust was installed with rustup, running rustup update stable fetches 1.100.0. The detailed release notes list every change to the compiler, Cargo and Clippy, and the cargo issue tracker collects known problems with the new build directory layout.

## Conclusion

Rust 1.100 closes a long chapter with the never type and opens a new one with stable custom allocators. For most teams the immediate value lies in Cargo, where dependency cooldowns and unused dependency warnings make builds safer and leaner with only a few lines of configuration.

## Sources

**Get more Vinish.dev tutorials on Google**

See my articles more often when you search Google.
