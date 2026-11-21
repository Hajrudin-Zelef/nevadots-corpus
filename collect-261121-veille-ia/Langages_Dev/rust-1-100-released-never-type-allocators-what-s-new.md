---
id: collect-261121-veille-ia/langages-dev/rust-1-100-released-never-type-allocators-what-s-new
title: "Rust 1.100 Released: Never Type, Allocators & What's New"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-12"]
keywords: ["type", "never", "cargo", "rust", "stable", "allocator", "allocators", "unused"]
source: https://tecadmin.net/rust-1-100-released-whats-new/
source_anchor: ""
source_lines: [1, 128]
section: "Langages_Dev"
---
**Rust 1.100.0** was released on 12 November 2026, and it closes one of the longest-running stories in the language: the **never type (`!`) is finally stable**, after about ten years and five earlier attempts. The release also stabilizes the core of the **custom allocator API**, and Cargo gains two supply-chain features: warnings for **unused dependencies** and optional **dependency cooldowns** that skip freshly published crate versions.

The never type change can break a small amount of existing code, so read the compatibility notes below before you upgrade your toolchain in CI.

## Rust 1.100 at a Glance

| Release date | 12 November 2026 | 
|---|---|
| Version | Rust 1.100.0 | 
| Headline feature | The never type `!` is stable | 
| Also stable | Core custom allocator API ( `Allocator` ,`Vec::new_in` ,`Box::new_in` ) | 
| Cargo | Unused dependency warnings, `global-min-publish-age` cooldowns | 
| Upgrade | `rustup update stable` | 
| Previous release | Rust 1.99 (no longer supported) | 

## What’s New in Rust 1.100

### The never type is stable

The never type, written `!`, is the type of an expression that never produces a value: a `panic!()`, an endless `loop`, or a function that exits the process. Until now you could only use it as a function’s return type (`-> !`). From Rust 1.100 you can name it anywhere, for example in `Result<T, !>` for an operation that cannot fail:

```
```
```
fn infallible_unwrap<T>(res: Result<T, !>) -> T {
    match res {
        Ok(value) => value,
        Err(never) => never, // a value of type ! converts to any type
    }
}
```
`std::convert::Infallible`, the stand-in type that libraries have used for “this can’t fail”, is now simply an alias for `!`, so existing APIs that use `Infallible` accept `!` too.

### Custom allocators

The core of the long-awaited allocator API is now stable. Library authors can implement the `std::alloc::Allocator` trait, and `Vec` and `Box` can store their data in a chosen allocator through new constructors such as `Vec::new_in`, `Vec::with_capacity_in` and `Box::new_in`. The `Global` and `System` allocators implement the trait:

```
```
```
use std::alloc::System;
let mut buffer = Vec::with_capacity_in(1024, System);
buffer.push(42u8);
```
This is the first step of the Allocators 1.0 project goal. Other collections and smart pointers will follow in later releases.

### DropGuard and other library additions

`core::mem::DropGuard` runs a closure when it goes out of scope, which makes cleanup on early returns easy. It wraps a value, gives access to it through `Deref` and `DerefMut`, and can be cancelled with the associated function `DropGuard::dismiss`. Other notable stabilized APIs include:

- `bool::toggle`
- `Box::take` ,`Box::map` ,`Rc::map` and`Arc::map`
- `str::trim_prefix` and`str::trim_suffix` (also on slices and`Path::trim_prefix` )
- `Vec::from_fn`
- `std::mem::conjure_zst` , an unsafe function for creating values of zero-sized types
- `impl Default for std::sync::Once`

### Cargo warns about unused dependencies

Cargo now reports dependencies listed in `Cargo.toml` that your code never uses, through the new `cargo::unused_dependencies` lint. It is set to **warn** by default. You can change the level in the `[lints.cargo]` table, for example to fail CI builds:

```
```
```
[lints.cargo]
unused_dependencies = "deny"
```
The check only runs when every target that could use a dependency is built, so `cargo check --bin foo` on its own won’t show it, and it doesn’t warn about `dev-dependencies`.

### Dependency cooldowns with min-publish-age

Many supply-chain attacks publish a malicious version of a popular crate and rely on projects picking it up within hours. Rust 1.100 stabilizes **publish-age aware resolution**: Cargo can skip crate versions that were published more recently than a minimum age. It is opt-in, in `.cargo/config.toml`:

```
```
```
[registry]
global-min-publish-age = "3 days"
```
When you need a brand-new version, for example for a security fix, you can override the rule for a single update:

```
```
`CARGO_RESOLVER_INCOMPATIBLE_PUBLISH_AGE=allow cargo update clap --precise 4.6.9`
Note that `cargo install` ignores the minimum publish age, and the Rust team stresses that cooldowns reduce risk but aren’t a complete defence.

### Other changes

- `extern "custom"` is stabilized.
- `cargo install` now uses the lockfile shipped inside the crate by default, so`--locked` has no extra effect there.
- `wasm32-wasip3` is promoted to Tier 2, and`i686-pc-windows-gnu` becomes a Tier 2 target without host tools.
- The standard library now uses Unicode 18.0.0.

## Before You Upgrade: Compatibility Notes

- **Never type fallback:** in editions before 2024 (2015, 2018 and 2021), the type the compiler picks for unconstrained never-type expressions changes from`()` to`!` , as edition 2024 already does. Most code won’t notice. It can affect code that ignores the result of a generic function, and the compiler may report more code as unreachable. Warnings were issued in earlier releases and popular crates were fixed, so if a dependency fails to build, update it first.
- **Cargo build directory layout** has changed. Scripts or tests that look for files inside`target/` should use Cargo’s JSON messages instead; the layout is not a stable interface.
- **`box` patterns** are no longer accepted by the parser.
- **Closures** capture variables used in`if let` more precisely, which can cause new borrow errors in rare cases.
- **ARM targets:**`-Ctarget-feature` or`-Ctarget-cpu` values that would change the ABI are now hard errors.

The full list is in the Rust 1.100.0 release notes.

## Support Timeline

Rust ships a new stable version every six weeks, and only the latest stable release receives fixes. With 1.100 out, Rust 1.99 is no longer supported. Following the six-week cycle, Rust 1.101 is expected around 24 December 2026. See the Rust release history for every version and its dates.

## How to Upgrade to Rust 1.100

If you installed Rust with rustup, update the stable toolchain and check the version:

`rustup update stable`
`rustc --version`

The output should start with **rustc 1.100.0**. To install Rust for the first time on Linux or macOS, use the official rustup installer, which also installs Cargo:

```
```
`curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`
Rust packages from Linux distributions (for example `apt install rustc`) usually lag several releases behind, so use rustup if you want 1.100 now.

## Should You Upgrade?

Yes. Only the newest stable release is supported, and upgrades between stable versions are normally painless. Run your test suite once on 1.100 to catch any never-type fallback changes, and update dependencies that fail to compile. If you publish or consume many crates, consider enabling `global-min-publish-age` as a cheap extra layer of supply-chain protection.

Source: Announcing Rust 1.100.0 (Rust Release Team).

How did it make you feel?
