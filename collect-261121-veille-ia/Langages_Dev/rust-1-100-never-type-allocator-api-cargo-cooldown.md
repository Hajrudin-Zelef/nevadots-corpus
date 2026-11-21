---
id: collect-261121-veille-ia/langages-dev/rust-1-100-never-type-allocator-api-cargo-cooldown
title: "Rust 1.100: never type, Allocator API, Cargo cooldown"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-12"]
keywords: ["type", "never", "cargo", "rust", "allocator", "official", "dependency", "release"]
source: https://www.oflight.co.jp/en/columns/rust-1-100-never-type-allocator-api-release-2026
source_anchor: ""
source_lines: [1, 248]
section: "Langages_Dev"
---
Rust 1.100.0 (Nov 12) stabilizes the never type and the core Allocator API, and adds Cargo unused dependency warnings and min-publish-age and update steps.

**Rust 1.100.0** is a stable Rust release published by the Rust Release Team on November 12, 2026. Its highlights are the **never type `!`**, stabilized after about ten years, the **`Allocator` trait** now usable with `Vec` and `Box`, and two Cargo additions: **unused dependency warnings** and a **dependency cooldown setting**.

## Three-line summary

- Rust 1.100.0 was published on November 12, 2026. Update with `rustup update stable`. The never type `!` and the core of the `Allocator` API are stabilized

- **Breaking change**: in editions 2015, 2018 and 2021, the never type fallback changes from `()` to `!` (edition 2024 already made this change). Some dependency crates may need updating

- Cargo adds an unused dependency warning (`unused_dependencies` under `[lints.cargo]`) and `global-min-publish-age`, which avoids versions published too recently

## What is Rust 1.100.0

Rust 1.100.0 is the stable release announced in the official blog post "Announcing Rust 1.100.0" dated November 12, 2026. Existing users can update with `rustup update stable`. The release spans the language, the standard library and Cargo. The stabilization of the never type is a long-standing item that the official post says took about 10 years and 5 failed stabilization attempts.

This article covers only what the official blog post describes. For the full list of changes, see the changelogs for Rust, Cargo and Clippy linked from the official post.

## Summary of main changes

| Area | Change | Impact | 
|---|---|---|
| Language | Stabilized never type `!` ;`Infallible` becomes an alias for`!` | You can now write types such as `Result<T, !>` | 
| Language (breaking) | Never type fallback in editions 2015/2018/2021 changes from `()` to`!` | Existing code may fail to compile | 
| Standard library | Core of `allocator_api` , including the`Allocator` trait, is stable | Custom allocators work with `Vec` and`Box` | 
| Standard library | `core::mem::DropGuard` | Runs a closure on every exit path | 
| Standard library | `std::mem::conjure_zst` | The recommended unsafe way to create a zero-sized value | 
| Cargo | Unused dependency warning | Controlled via `[lints.cargo]` , warn by default | 
| Cargo | Fewer `build-dir` cache collisions | Workarounds in build processes may break | 
| Cargo | `global-min-publish-age` | Skips versions published too recently | 

## Stabilization of the never type `!`

The never type, written `!`, is the type of computations that never produce a value. Before this release it could be used implicitly, such as in a `match` arm ending in `return` or as a function return type, but it could not be written in other positions such as `Result<T, !>`. In 1.100.0 you can name `!` anywhere. Here is the example from the official post.

```
fn mrrrow(option: Option<u32>) {
    let value = match option {
        // `x` has type `u32`
        Some(x) => x,
        // `return` has type `!`, which is then coerced to `u32`,
        // allowing the `match` to pass type checking.
        None => return,
    };
    // ...
}
```
In the example above, `return` has type `!`, which is coerced to `u32`, so the `match` passes type checking. The next example shows `Result<_, !>`, which could not be written before. From a `Result` that cannot fail by type, you can extract the value with a plain pattern match.

```
fn infallible_unwrap<T>(res: Result<T, !>) -> T {
    // ^^ writing `Result<_, !>` was not allowed before
    let Ok(v) = res;
    v
}
```
At the same time, `Infallible` becomes an alias for `!`. The official post says older APIs that used `Infallible` work with `!`. `impl AsRef<T> for !` and `impl AsMut<T> for !` are also in the list of stabilized APIs.

## The never type fallback change and its impact on existing code

This is the only breaking change in this release. According to the official post, in editions 2015, 2018 and 2021 the never type fallback changes from `()` to `!`. Edition 2024 already made this change, so code on edition 2024 or later behaves the same as before.

Code that ignores a generic result may be affected. The compiler may also report more code as unreachable. The post says future-compatibility warnings were issued in advance. It does not name specific lints or describe an automatic fix with `cargo fix`.

The migration the post recommends is to update any dependency that fails because of this change; fixes are said to be already published for many crates. The following flow is a safe way to check.

1. Before updating, run `cargo check` on your current toolchain and see whether any warnings appear

2. Move to 1.100.0 with `rustup update stable`, then run `cargo check` and `cargo test`

3. If something fails, update the dependency crate named in the error, for example with `cargo update -p <crate>`

4. If your own code fails, add explicit types where you relied on `()`

Steps 3 and 4 are not steps from the official post; they are generic commands applying the official guidance (update dependencies).

## Stabilization of the Allocator API

The core of the `allocator_api` feature is now stable, and the `Allocator` trait is available in the standard library. For now, custom allocators work with `Vec` and `Box`. The official post positions this as the foundation of the "Allocators 1.0" project goal and says support for more collections, pointer types and a wider API surface will follow.

The stabilized APIs include the following.

- Trait and types: `std::alloc::Allocator`, `std::alloc::Global`, `std::alloc::System`

- `Allocator` implementations: `Global`, `System`, `&A`, `&mut A`, `Box<T, A>`, `Rc<T, A>`, `Arc<T, A>`

- `Box`: `new_in`, `from_raw_in`, `from_non_null_in`, `into_raw_with_allocator`, `into_non_null_with_allocator`, `allocator`

- `Vec`: `new_in`, `with_capacity_in`, `from_raw_parts_in`, `from_parts_in`, `into_raw_parts_with_allocator`, `into_parts_with_allocator`, `allocator`

This is useful when you want to combine purpose-specific allocators, such as arenas (allocate together, free together) or allocators that measure memory use, with the standard `Vec` and `Box`. The official post contains no usage code for this, so this article only introduces the API names.

## DropGuard: run a closure on every exit path

`core::mem::DropGuard` is a new helper that runs a closure when it is dropped. It wraps a value and implements `Deref` and `DerefMut`, so the value stays usable through the guard. When the guard is dropped, the closure runs and the value is consumed. Calling `DropGuard::dismiss` returns the value without running the closure. The post explains that `dismiss` is an associated function rather than a method to avoid name collisions with methods on the inner type. The official example rolls a `Vec` back to its original length if parsing fails.

```
use std::mem::DropGuard;
use std::num::ParseIntError;
/// Attempt to parse all `strings` and push them into `nums`.
/// If parsing any of them fails, `nums` remains unchanged.
fn parse_all(nums: &mut Vec<u32>, strings: &[&str]) -> Result<(), ParseIntError> {
    let len = nums.len();
    let mut nums = DropGuard::new(nums, move |nums| {
        nums.truncate(len);    // 1. Reset the vec on failure
    });
    for s in strings {
        nums.push(s.parse()?); // 2. If `?` returns an error, roll back
    }
    DropGuard::dismiss(nums);  // 3. Parsing successful, dismiss the guard
    Ok(())
}
```
Whether the function returns early via `?` or panics, the rollback runs when the guard is dropped. You release the guard with `dismiss` only on success.

## conjure_zst: creating zero-sized types

`std::mem::conjure_zst` is an unsafe function for creating a value of a zero-sized type, and it is the method the official post recommends. It replaces workarounds such as transmuting `()` or using `MaybeUninit`. It is unsafe because some APIs rely on the inability to construct a type for safety. The post cites PyO3's `Python<'py>`, a token that represents holding the Python GIL: if it could be created freely, safe code could bypass that requirement.

## Cargo: unused dependency warnings

Cargo now warns about unused dependencies. It is controlled by the `[lints.cargo]` system; the lint is `unused_dependencies` and it defaults to warn. Here is the official sample output.

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
The post lists two limitations. First, the warning appears only when every build target that may use a dependency is built. For example, `cargo check --bin foo` does not show unused `[dependencies]` warnings because another `bin` may use them. Second, unused `dev-dependencies` are not reported, because there is no way to ensure that every development build target is built.

## Cargo: fewer build-dir cache collisions

The `build-dir` now applies the build-configuration hash consistently. Directories are organized by build step instead of file type, and the hash moved from a file's name to its parent directory. This reduces both extra rebuilds and missed rebuilds.

However, build processes and tests that worked around the old behavior may break. In some cases the workaround is no longer needed. The post points to cargo#17287 for possible solutions and new use cases. If your CI refers directly to build artifact paths, check it after updating.

## Cargo: min-publish-age (dependency cooldowns)

`global-min-publish-age` makes Cargo skip versions that were published too recently, and it is stabilized in Rust 1.100. It is a so-called dependency cooldown that lowers the risk of pulling in a malicious version right after it is published. You set it in `.cargo/config.toml`.

```
# .cargo/config.toml
[registry]
global-min-publish-age = "3 days"
```
With this setting, `cargo update` holds back new versions that do not meet the age. Here is the official sample output.

```
$ cargo update
    Updating crates.io index
     Locking 2 packages to highest compatible version as of min-publish-age
      Adding cc v1.11.0 (available: v1.18.0, published 3 hours ago)
      Adding clap 4.6.7 (available: 4.6.9, published 2 days ago)
```
When you temporarily want the latest version, the environment variable `CARGO_RESOLVER_INCOMPATIBLE_PUBLISH_AGE=allow` lifts the restriction for a single update.

```
$ CARGO_RESOLVER_INCOMPATIBLE_PUBLISH_AGE=allow cargo update clap --precise 4.6.9
    Updating crates.io index
    Updating clap 4.6.7 -> clap 4.6.9 (published 2 days ago, minimum age 3 days)
```
There are three caveats. First, the feature depends on the registry recording each version's publish time; versions without a `pubtime` field in the index are never held back. Second, `cargo install` ignores the minimum publish age for both the installed crate and its dependencies; to work around this, pin a version such as `cargo install foo@1.2.3`. Third, the post states that cooldowns reduce some supply chain risk but are not a complete defense, so dependency updates should still be reviewed.

For a related supply chain measure, the .NET 11 SDK also has a setting that avoids packages published less than a set time ago. See the .NET 11 release guide for comparison.

## Other standard library stabilizations

The official list of stabilized APIs (47 items) includes the following, in addition to those covered above.

- `bool::toggle`

- `Box::take`, `Box::map`, `Rc::map`, `Arc::map`

- `str::trim_prefix` / `str::trim_suffix`, the slice methods `[T]::trim_prefix` / `[T]::trim_suffix`, and `Path::trim_prefix`

- `Vec::from_fn`

- `UnsafeCell::replace`, `UnsafeCell::as_ref_unchecked`, `UnsafeCell::as_mut_unchecked`

- `impl Default for std::sync::Once`

- `Child::main_thread_handle` and `os::windows::process::CommandExt::show_window`

- `PartialEq` implementations between `Vec<T, A>` or `&[T]` and `Cow<'_, [U]>`

## Update procedure

Update the toolchain with rustup and check the version.

```
# Update
rustup update stable
# Verify (should report 1.100.0)
rustc --version
cargo --version
```
After updating, run `cargo check` and `cargo test` in your project. The unused dependency warning starts appearing as soon as you update, so if your CI treats warnings as errors, it is safer to check locally first.

## Upgrade checklist

- Check which edition you use (2021 and earlier are subject to the never type fallback change)

- After `rustup update stable`, confirm that `cargo check` and `cargo test` pass

- If something fails, check whether an updated version of the dependency named in the error has been published

- If CI treats warnings as errors (for example `-D warnings`), check that unused dependency warnings do not break it

- Decide how to handle `unused_dependencies` under `[lints.cargo]` in `Cargo.toml`

- Check for CI or scripts that depend on build artifact paths or `build-dir`

- If you use cooldowns, set `global-min-publish-age` in `.cargo/config.toml` and share the environment variable override procedure for urgent updates

- Pin versions for tools installed with `cargo install`

## FAQ

When was Rust 1.100.0 released?

The official blog post is dated November 12, 2026. Existing users can update with `rustup update stable`.

Will the never type fallback change break my code?

In editions 2015, 2018 and 2021 the fallback changes from `()` to `!`, so code that ignores a generic result may be affected. Edition 2024 already made this change and is unaffected. The official post advises updating dependencies that fail, and fixes are said to be published for many crates.

Which collections can use the Allocator API?

According to the official post, custom allocators currently work with `Vec` and `Box`. Support for other collections and pointer types and a wider API are planned.

Why does `cargo install` still fetch the latest version with global-min-publish-age set?

`cargo install` ignores the minimum publish age for both the installed crate and its dependencies. Pin a version such as `cargo install foo@1.2.3`. Versions without a `pubtime` in the index are also not held back.

Why might the unused dependency warning not appear?

The warning appears only when every build target that may use the dependency is built. It does not appear when you build only part of the project, such as with `cargo check --bin foo`. `dev-dependencies` are also excluded.

株式会社オブライト

Oflight Inc. is a Tokyo-based IT company providing software development, digital signage, web development, network systems, and IoT/AR/VR technology services.

Related free tools (no sign-up, instant results)
