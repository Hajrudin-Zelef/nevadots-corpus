---
id: collect-261121-veille-ia/langages-dev/rust-1-100-released-the-never-type-stabilizes-after-a-decade-while-cargo-adds-supply-chain-protectio
title: "Rust 1.100 Released: The Never Type Stabilizes After a Decade, While Cargo Adds Supply Chain Protection"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["type", "never", "rust", "allocator", "after", "released", "cargo", "infallible"]
source: http://dev.to/joho_todai/rust-1100-released-the-never-type-stabilizes-after-a-decade-while-cargo-adds-supply-chain-5e90
source_anchor: ""
source_lines: [1, 73]
section: "Langages_Dev"
---
Rust 1.100.0 was released on November 12. The release marks major milestones across the language and toolchain, led by stabilization of the never type after five failed attempts over 10 years, the foundation of the Allocator API, and a dependency cooldown feature in Cargo.

**Key points**

- 
**What happened:** Rust 1.100 was released on November 12, stabilizing the never type (!) across all editions after five failed attempts over 10 years
- 
**Why it was difficult:** The plan to make Infallible an alias for the never type required a breaking change to fallback rules, and pre-release testing found 3,277 regressions
- 
**Practical progress:** Alongside stabilization of the Allocator API foundation, Cargo gained the min-publish-age feature, which automatically holds newly published packages and adds another supply chain defense option

Rust has a special type written as `!`. Known as the **never type**, it represents in the type system that a value can never exist. Expressions that do not return, such as `return`, `break`, and `std::process::exit`, all have this `!` type. Because no value can exist, `!` can be converted to any type. This property allows type checking to succeed even when one arm of a `match` ends with `return`.

The concept of the never type had long existed internally in Rust. A function return type could be written as `-> !` to declare that the function never returns to its caller. However, it could not be used freely as a type parameter—for example, as `Result<T, !>` to express that a Result can never fail.

Rust 1.100 removes this restriction. `!` can now be written in any type position, completing the never-type project goal.

The tracking issue for the original proposal, RFC 1216 (#35121), was opened in July 2016. Since then, **all five stabilization attempts** have failed.

The core difficulty was integration with `Infallible`. Rust's standard library includes a type called `std::convert::Infallible` that represents an operation that cannot fail. `Infallible` was introduced with the plan to eventually make it an alias for the never type, but doing so required changing the fallback rules used when the compiler cannot infer a type.

Previously, the compiler automatically assigned the empty tuple `()` where it could not determine a type. This had to be changed to `!` to unify Infallible and the never type. However, changing this fallback can alter the behavior of existing code, making it a breaking change.

Rust adopted a phased strategy: switch the fallback to `!` first in the Rust 2024 edition, allow the change to spread through the ecosystem, and then apply it to all editions—2015, 2018, and 2021—in version 1.100. Pre-release testing against crates published on crates.io initially found 3,277 regressions. After fixes spanning 18 commits, compatibility patches were also distributed in advance to affected public crates.

If updating a dependency breaks code, an update to that crate may already provide a fix. The compiler may also mark a broader range of code as unreachable.

Another major stabilization is the foundation of the Allocator API. Long restricted to nightly under `#![feature(allocator_api)]`, the `Allocator` trait has entered stable Rust, allowing library authors to define custom memory allocators and pass them to `Vec` and `Box`.

Many systems, including game engines, databases, and embedded systems, need fine-grained control over memory allocation. Until now, developers had to replace the global allocator for the entire system or manage memory themselves with unsafe code. The Allocator API allows different allocators to be assigned safely on a per-type basis.

Only `Vec` and `Box` are currently supported. Expansion to other collection types, including `HashMap` and `Rc`, is planned as future work under the Allocators 1.0 project goal.

`core::mem::DropGuard` has been added as a new helper type that guarantees a closure, or function object, will run when a scope is exited. It accepts a variable and a closure and implements `Deref` and `DerefMut`, so the wrapped variable can continue to be used directly.

A typical use is implementing transactional rollback: if an operation fails partway through, the DropGuard closure automatically reverts the state. If it succeeds, the `dismiss` method cancels execution of the closure and commits the change.

Rust's `Drop` trait can accomplish the same result, but the practical benefit is that DropGuard needs only a closure and does not require defining a type.

`std::mem::conjure_zst` has been added as an official `unsafe` function for creating values of zero-sized types (ZSTs). A type with no size has no representation in memory, and only one value can exist. Previously, developers created such values through techniques such as `transmute(())` or `MaybeUninit::uninit().assume_init()`, but these were workarounds outside the intended purpose of those APIs.

The function is `unsafe` because some APIs ensure safety by intentionally preventing construction of a zero-sized type; such types must not be created without a valid reason. The official blog gives PyO3's `Python<'py>` token as an example. This type is a marker indicating that Python's Global Interpreter Lock (GIL) is held. If the token could be freely created, Python APIs could be called without holding the GIL.

Cargo now reports a warning for dependencies listed in `Cargo.toml` but unused by every build target. It was introduced as part of the new `[lints.cargo]` lint system, completing this project goal as well.

There is one limitation: the warning appears only when all build targets in the package are built. If only a specific binary is built, as with `cargo check --bin foo`, Cargo does not warn about dependencies used by other binaries. Unused `dev-dependencies` are also excluded from detection.

Cargo's build-directory layout has been redesigned so that the build-configuration hash is applied consistently to every artifact. Platform constraints previously meant that some artifacts received no hash, causing unnecessary rebuilds, missed rebuilds, and confusion when building across git worktrees. The work began with the 2025 build directory layout project goal and opens the way for future cache garbage collection and sharing across workspaces.

The change may affect tools and tests that depend on the internal structure of the build directory.

Malicious package versions (in Japanese) are often discovered by automated scanners or the community within hours or days of publication. **The min-publish-age feature** prevents new versions from being pulled in automatically until that detection window has passed.

It can be configured in `.cargo/config.toml` as follows.


```
[registry]
global-min-publish-age = "3 days"
```
When `cargo update` runs, versions newer than the configured period are automatically held back, and Cargo displays what was withheld. If a new version is needed urgently, the restriction can be overridden for that run with the `CARGO_RESOLVER_INCOMPATIBLE_PUBLISH_AGE=allow` environment variable.

The restriction does not apply to `cargo install`. Versions without a publication-time field in the registry index are also excluded.

The official blog says the feature reduces the risk of supply chain attacks but is not a complete defense on its own. Quarantine procedures for crates and automatic detection of suspicious releases are also planned under the crates.io supply chain hardening project goal.

Rust 1.100 may look like a milestone by version number, but the substance marks a deeper transition. Stabilizing the never type after a decade demonstrates Rust's stabilization method itself: not avoiding changes that might break code, but measuring the scope of breakage, fixing it, and migrating in stages. Stabilizing the Allocator API increases freedom in memory management, while Cargo's min-publish-age draws a defensive line across the vulnerable period in the software supply chain.

Language maturity and practical safety have arrived in the same release.

*Originally published on Joho no Todai. The Japanese original is here.*

## Top comments (0)
