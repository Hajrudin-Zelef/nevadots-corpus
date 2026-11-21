---
id: collect-261121-veille-ia/langages-dev/rust-1-100-is-the-biggest-release-in-years-never-type-custom-allocators-and-dependency-cooldowns
title: "Rust 1.100 Is the Biggest Release in Years: Never Type, Custom Allocators, and Dependency Cooldowns"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-12"]
keywords: ["type", "layout", "rust", "never", "allocator", "release", "dependency", "result"]
source: https://dev.to/chenyuan20509/rust-1100-is-the-biggest-release-in-years-never-type-custom-allocators-and-dependency-cooldowns-2on2
source_anchor: ""
source_lines: [1, 111]
section: "Langages_Dev"
---
How long should it take to stabilize one type in a programming language? If the type is Rust's never type, the honest answer turned out to be ten years and five failed stabilization attempts. Rust 1.100.0, released on November 12, 2026, finally lands it — and that is not even the most operationally important change in the release. The same version stabilizes the core of the allocator API so `Vec` and `Box` can finally use custom allocators on stable, adds a `DropGuard` helper that makes rollback-style code a three-line pattern, warns you about dependencies you no longer use, reorganizes Cargo's build directory, and introduces dependency cooldowns that make the resolver ignore freshly published crate versions until they have survived a waiting window you choose.

That is an unusual amount of substance for one release. This article walks through each change with the code you would actually write, flags the one deliberate breaking change, and ends with a practical answer to the upgrade question.

The never type, spelled `!`, is Rust's canonical uninhabited type: the type of computations that never produce a value. Expressions that permanently divert control flow — `return`, `break`, `std::process::exit` — have type `!`, and because no value of type `!` can exist, Rust allows a never-to-any coercion that turns `!` into whatever type the surrounding context needs. That coercion is why a `match` arm ending in `return` type-checks next to arms that produce real values.

You could observe `!` before, and even write it in a function return position (`-> !`). What 1.100 adds is the ability to name it anywhere a type goes, which makes previously impossible signatures expressible:


```
fn infallible_unwrap<T>(res: Result<T, !>) -> T {
    // Writing `Result<_, !>` was not allowed before 1.100.
    let Ok(v) = res;
    v
}
```
A `Result<T, !>` is a result that cannot fail, and the function above unwraps it without a panic path, a dummy error case, or an `unsafe` block. The compiler accepts the irrefutable `let Ok(v)` pattern because it can prove the `Err` variant is uninhabited.

Two deeper changes ride along. First, `std::convert::Infallible` is now an alias for `!`, so every old API written against `Infallible` is automatically compatible with the never type — this was the plan from the start, and untangling it is a large part of why stabilization took five attempts. Second, the release changes "never type fallback" from `()` to `!` in editions 2015, 2018, and 2021, matching what edition 2024 already does. That is a deliberate breaking change. The team shipped future-compatibility warnings well in advance and fixed affected public crates, but the fallout is real if rare: code that ignores the result of a generic function can change meaning, and the compiler may now flag more code as unreachable. If a dependency fails to build after upgrading, check for a newer release of that dependency before doing anything else — the fix probably exists.

The second headline is the stabilization of the core of `allocator_api`: the `std::alloc::Allocator` trait, plus the `Global` and `System` allocator types. The practical consequence is that `Vec` and `Box` now have `_in` constructors on stable Rust — `Vec::new_in`, `Vec::with_capacity_in`, `Box::new_in`, `Box::from_raw_in`, and the matching accessors like `Vec::allocator` and `Box::into_raw_with_allocator`.


```
use std::alloc::{Allocator, Global, Layout, AllocError};
use std::ptr::NonNull;
struct Counting;
unsafe impl Allocator for Counting {
    fn allocate(&self, layout: Layout) -> Result<NonNull<[u8]>, AllocError> {
        let ptr = Global.allocate(layout)?;
        eprintln!("allocated {} bytes", layout.size());
        Ok(ptr)
    }
    unsafe fn deallocate(&self, ptr: NonNull<u8>, layout: Layout) {
        eprintln!("freed {} bytes", layout.size());
        unsafe { Global.deallocate(ptr, layout) }
    }
}
fn main() {
    let mut v: Vec<u32, Counting> = Vec::new_in(Counting);
    v.push(41); // allocation goes through your code
}
```
This is the foundation of the "Allocators 1.0" project goal, and the release notes are explicit that the job is not finished: more collections and pointer types still need `_in` variants. But the hard part — the trait and its object-safety design — is done. Arena allocation, per-request memory budgets in servers, and embedded allocators no longer require nightly. The community noticed immediately: one Hacker News commenter described spending days working around arena leaks with the third-party `allocator-api2` shim and being able to delete that workaround now that `Vec::new_in` is stable.

`Drop` already guarantees that code runs when a value goes out of scope, whether the exit is a return, a `?`, or a panic. But writing a whole struct with a `Drop` impl just to run one closure is friction, so Rust 1.100 stabilizes `core::mem::DropGuard`: a helper that wraps a variable and a closure, derefs to the variable so you can keep using it, and runs the closure — consuming the variable — when dropped. Its `dismiss` method defuses the guard and hands the variable back, which makes all-or-nothing, transaction-shaped APIs a small, local pattern:


```
use std::mem::DropGuard;
use std::num::ParseIntError;
/// Push every parsed string into `nums`, or leave `nums` untouched.
fn parse_all(nums: &mut Vec<u32>, strings: &[&str]) -> Result<(), ParseIntError> {
    let len = nums.len();
    let mut nums = DropGuard::new(nums, move |nums| {
        nums.truncate(len); // reset on failure
    });
    for s in strings {
        nums.push(s.parse()?); // an early `?` triggers the rollback
    }
    DropGuard::dismiss(nums); // success: keep the changes
    Ok(())
}
```
Before this, the same guarantee required either a bespoke guard type per call site or carefully ordered cleanup code on every error path. The guard version states the rollback once, next to the data it protects.

The strangest addition is a single unsafe function: `std::mem::conjure_zst`. It creates a value of a zero-sized type out of thin air, replacing the folk incantations people used before — transmuting `()`, or `MaybeUninit::uninit().assume_init()`. The reasoning is subtle: if a type has no size and is inhabited, there is exactly one bit pattern it can have, so materializing it is well-defined. The function is unsafe because APIs routinely rely on such types being impossible to construct. The release notes give a concrete example: PyO3's `Python<'py>` is a zero-sized token witnessing that the caller holds the Python GIL, and conjuring one would let safe PyO3 calls run without the lock. Reach for `conjure_zst` when you control the type's invariants; never to bypass someone else's.

Cargo gained a real linting system, configured through a `[lints.cargo]` table in `Cargo.toml`, and its first user-facing lint is on by default: unused dependency reporting. A build now emits warnings like:


```
warning: unused dependency `tracing-subscriber`
  --> Cargo.toml:23:1
   |
23 | tracing-subscriber.workspace = true
   | ^^^^^^^^^^^^^^^^^^
   |
   = note: `cargo::unused_dependencies` is set to `warn` by default
help: consider removing the dependency on `tracing-subscriber`
```
The existing rustc lint `unused_crate_dependencies` could not do this accurately, because Cargo shares dependency entries across every build target in a package — a crate used by one binary but not another would trigger false positives. The Cargo-level implementation understands target structure. Two limitations matter in practice: the warning only appears when you build every target that could use an entry (a bare `cargo check --bin foo` stays quiet about shared `[dependencies]`, because another bin might use them), and dev-dependencies are never flagged, since there is no way to guarantee every development target got built. Even with those caveats, this deletes a whole category of CI scripts and manual audits.

Cargo builds intermediates in a build directory and lifts final artifacts into the target directory, keeping configurations apart with a build-configuration hash. That hash was not always applied, because of platform constraints, which produced extra rebuilds, missed rebuilds, and genuine confusion when a build directory was shared across git worktrees. In 1.100 the layout is reorganized around the build step rather than the file type, and the hash moves from the file name into the parent directory, so it applies consistently.

This is the stabilized output of the build-directory-layout project goal started in 2025, and it exists to unblock cache garbage collection, cross-workspace cache deduplication, and finer-grained locking. It is an internal change, but the release notes warn plainly that it may break build scripts and tests that encoded workarounds for the old behavior — in many cases the right fix is deleting the workaround. Cargo issue 17287 collects migration notes and new use cases.

The supply-chain feature of the release is minimum publish age, from RFC 3923. Malicious crate versions are usually caught by scanners or the community within hours or days of publication. Cooldowns let you refuse to be the early adopter of a compromised release. Opt in once, in `.cargo/config.toml`:


```
[registry]
global-min-publish-age = "3 days"
```
From then on, version resolution skips anything published more recently than the window, and tells you what it held back:


```
     Locking 2 packages to highest compatible version as of min-publish-age
      Adding cc v1.11.0 (available: v1.18.0, published 3 hours ago)
      Adding clap 4.6.7 (available: 4.6.9, published 2 days ago)
```
When you genuinely need the new version now, override for a single update with `CARGO_RESOLVER_INCOMPATIBLE_PUBLISH_AGE=allow cargo update clap --precise 4.6.9`; the newer version is then recorded in `Cargo.lock` and kept until a later re-resolve. Three sharp edges are documented: the mechanism depends on the registry recording publication times, and versions without a `pubtime` field in the index are never held back; `cargo install` ignores the minimum age entirely, for the installed crate and its dependencies, so pin with `cargo install foo@1.2.3` when it matters; and cooldowns reduce supply-chain risk without being a complete defense — review updates as before. Quarantine support and automated detection of suspicious releases are planned under the crates.io supply-chain hardening goal.

Beyond the headlines, the stabilized API list has several small tools that remove daily friction. `bool::toggle` flips a flag in place. `Box::map`, `Rc::map`, and `Arc::map` transform the inner value while preserving the allocation. `str::trim_prefix`, `str::trim_suffix`, their slice equivalents, and `Path::trim_prefix` return the remainder without the `strip_prefix` Option dance when you only want best-effort trimming. `Vec::from_fn` builds a vector from an index-based closure. `Child::main_thread_handle` exposes the handle of a spawned process's main thread, `Once` gets a `Default` impl, `UnsafeCell` gains `replace` and unchecked reference accessors, and on Windows `CommandExt::show_window` controls console window visibility for spawned processes. Individually minor; together they delete a lot of utility code.

Upgrading is one `rustup update stable` away, and for most projects the answer is yes, with two checks first. Scan your CI for anything that pokes at the target directory layout or papers over the old hash behavior, because that is the change most likely to break a build that "worked by accident". Then watch the first build for new unreachable-code warnings and dependency compile errors, the visible symptoms of the never-type fallback change; the fix is almost always updating the affected crate. What you get in return is a type ten years in the making, stable custom allocators, a rollback primitive, a leaner dependency list, and a resolver that can wait out the window in which supply-chain attacks get caught. Few releases earn their minor version bump this thoroughly.

Originally published on Dispatch.

## Top comments (0)
