---
id: collect-261121-veille-ia/langages-dev/rust-1-100-the-never-type-custom-allocators-and-cargo-cooldowns
title: "Rust 1.100: The Never Type, Custom Allocators and Cargo Cooldowns"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["type", "never", "cargo", "infallible", "rust", "custom", "allocators", "cooldowns"]
source: https://medium.com/@abhinav.dobhal/rust-1-100-the-never-type-custom-allocators-and-cargo-cooldowns-13c0a6e131a2
source_anchor: ""
source_lines: [1, 232]
section: "Langages_Dev"
---
# Rust 1.100: The Never Type, Custom Allocators and Cargo Cooldowns

## Rust 1.100 lands November 12, 2026. The never type is stable after about ten years, Vec and Box accept custom allocators, and Cargo can refuse brand-new crate versions. Here’s what that means if you build inference servers and agent tooling, plus what to check before you upgrade.

Rust 1.100 lands on Thursday, November 12, 2026. I expected a quiet release to go with the round number, and it has more in it than most. The never type is finally stable, the allocator API ships for `Vec` and `Box`, and Cargo gets a setting that makes it ignore crate versions published too recently.

The official announcement isn’t on blog.rust-lang.org yet as I write this on November 11. So I worked from the pre-release release notes, the draft release notes issue, the pull requests behind each feature, and the beta Cargo docs. Where something only comes from a draft, I say so.

A note on how this was made: I wrote it with help from Claude, Anthropic’s AI model, and checked each API name and config key against the PRs, the pre-release notes and the std and Cargo docs. The images were generated with ChatGPT.

I compiled and ran every Rust example below on `rustc 1.101.0-beta.1` (aarch64 macOS), which already contains everything stabilized in 1.100. I didn't run them on the 1.100.0 pre-release build itself.

## The short version

- The never type `!` is stable.`Infallible` becomes an alias for`!` , and the never type fallback changes from`()` to`!` on editions 2015, 2018 and 2021. That last part is a breaking change, though a small one.
- The core of the `Allocator` API is stable. You can put a`Vec` or`Box` in your own arena or bump allocator with`Vec::new_in` ,`Vec::with_capacity_in` and`Box::new_in` .
- Cargo has dependency cooldowns. Set `registry.global-min-publish-age` and the resolver skips versions newer than that age, unless they're already in`Cargo.lock` . The default is`"0"` , so nothing happens until you opt in.
- Cargo has its own lints now, configured under `[lints.cargo]` , including`unused_dependencies` .`cargo install` also uses the packaged lockfile by default.
- Smaller items include `bool::toggle` ,`Box::take` ,`Box::map` ,`Arc::map` ,`core::mem::DropGuard` ,`wasm32-wasip3` at Tier 2, and the end of host tools for 32-bit Windows.

## The never type, ten years later

The never type is the type of an expression that never produces a value: `return`, `break`, `panic!()`, `std::process::exit`. You could already write `-> !` on a function. What changes in 1.100 is that you can name `!` anywhere a type goes.

The draft blog section in the release notes tracking issue puts it as “After 10 years and 5 failed stabilization attempts”. The practical win for me is `Result<T, !>`, which says in the type that an operation can't fail, and lets you destructure it without a dead `Err` arm:

```
// A config parser that cannot fail, written with `!` directly.
fn parse_infallible(raw: &str) -> Result<String, !> {
    Ok(raw.trim().to_owned())
}
fn into_value<T>(res: Result<T, !>) -> T {
    let Ok(v) = res;
    v
}
fn check_infallible_is_never(e: std::convert::Infallible) -> ! {
    e
}
fn main() {
    let model = into_value(parse_infallible("  llama-3-8b  "));
    println!("{model}");
    let _ = check_infallible_is_never;
}
```
That compiles on the beta and prints `llama-3-8b`. The last function only type-checks because `Infallible` is now the same type as `!`. The draft blog text says this makes old APIs that used `Infallible` compatible with `!`, which covers things like the error type of `TryFrom` impls that can't fail.

In agent code I see this pattern a lot. A tool trait returns `Result<Output, Self::Error>`, and some tools can't fail. With `!` as the error type, the caller can unwrap with a plain `let Ok(x) = ...` and the compiler proves there's nothing to handle.

### The compatibility risk

Making `Infallible = !` work required changing never type fallback. When the compiler has an inference variable created by a diverging expression and nothing else constrains it, older editions picked `()`. Starting with 1.100, every edition picks `!`, which edition 2024 already did.

Here is the kind of code that breaks. It’s a generic function whose return type is only constrained by a `?`:

```
fn load<T: Default>() -> Result<T, String> {
    Ok(T::default())
}
fn run() -> Result<(), String> {
    load()?;
    Ok(())
}
```
On the beta with `--edition 2021`, this fails with ``the trait` Default `is not implemented for` ! `. The compiler points to issue #148922 and suggests you might have meant` ()`. The fix is to say the type:` load::<()>()?;` compiles fine.

I also checked older compilers. On 1.98.1 and 1.99.1, the same code already fails under the deny-by-default `dependency_on_unit_never_type_fallback` lint, with a suggestion to add `::<()>`. If your crate builds cleanly on 1.99 today, the cases that lint catches are already fixed.

The draft blog text adds that the change can also make the compiler see more code as unreachable, and that updating a broken dependency is worth trying first, since fixes have been released for public crates.

## Custom allocators for Vec and Box

The second big item is a subset of `allocator_api`, first proposed in RFC 1398 and unstable until now. PR #156882 stabilizes the `Allocator` trait, the `Global` and `System` allocators, and allocator-aware methods on `Box` and `Vec`. The PR says the remaining parts of the feature will be renamed `allocator_ext`.

The trait has two required methods, `allocate(&self, layout: Layout) -> Result<NonNull<[u8]>, AllocError>` and the unsafe `deallocate`. `grow`, `shrink` and the zeroed variants have default implementations. The PR notes that the trait's safety requirements were tightened to the strictest sound form, so the team can relax them later.

For AI serving, the use case is per-request memory. A tokenizer or an inference server handles a request, allocates a lot of small buffers for token IDs, scores and scratch space, then throws all of it away. An arena lets you skip the individual frees and release the whole request at once.

Here’s a deliberately tiny bump allocator over a fixed buffer. It never frees anything until the arena itself is dropped:

```
use std::alloc::{AllocError, Allocator, Layout};
use std::cell::{Cell, UnsafeCell};
use std::ptr::NonNull;
// A tiny bump arena: hands out slices of one fixed buffer and never frees.
struct Bump<const N: usize> {
    buf: UnsafeCell<[u8; N]>,
    used: Cell<usize>,
}
impl<const N: usize> Bump<N> {
    fn new() -> Self {
        Bump { buf: UnsafeCell::new([0; N]), used: Cell::new(0) }
    }
}
unsafe impl<const N: usize> Allocator for Bump<N> {
    fn allocate(&self, layout: Layout) -> Result<NonNull<[u8]>, AllocError> {
        let base = self.buf.get() as *mut u8;
        let start = (base as usize + self.used.get()).next_multiple_of(layout.align()) - base as usize;
        let end = start.checked_add(layout.size()).ok_or(AllocError)?;
        if end > N {
            return Err(AllocError);
        }
        self.used.set(end);
        let ptr = unsafe { NonNull::new_unchecked(base.add(start)) };
        Ok(NonNull::slice_from_raw_parts(ptr, layout.size()))
    }
    unsafe fn deallocate(&self, _ptr: NonNull<u8>, _layout: Layout) {
        // Memory is released all at once when the arena is dropped.
    }
}
fn main() {
    let arena = Bump::<4096>::new();
    let mut token_ids: Vec<u32, &Bump<4096>> = Vec::with_capacity_in(16, &arena);
    token_ids.extend([101, 7592, 2088, 102]);
    let mut scratch = Vec::new_in(&arena);
    scratch.push(0.5f32);
    println!("{:?} {:?} used={}", token_ids, scratch, arena.used.get());
}
```
It prints `[101, 7592, 2088, 102] [0.5] used=80` on the beta. Passing `&arena` works because `&A` implements `Allocator` whenever `A` does, which is one of the stabilized impls. The signatures I used match the PR: `Vec::new_in(alloc: A)` and `Vec::with_capacity_in(capacity: usize, alloc: A)`.

I wouldn’t ship this as is. A real arena needs to grow, handle alignment edge cases carefully, and probably sit on a crate like `bumpalo`. What changed in 1.100 is that arena crates can now plug into `std::vec::Vec` on a stable toolchain.

Two caveats from the sources. The release notes tracking issue says only `Vec` and `Box` are covered for now, with other collections still to come. The compatibility notes also warn that some `Box` impls were generalized to arbitrary allocators (PR #161946), which may cause new type inference errors.

## Cargo dependency cooldowns

This is the change I’d turn on first. The pre-release notes call it “minimum publish age configurations, also known as dependency cooldowns” (RFC 3923). When it’s set, the resolver ignores versions published more recently than the configured age, unless they’re already in `Cargo.lock`.

The reason is supply-chain attacks. My reasoning: if a malicious version gets spotted and pulled soon after it’s published, waiting a week before picking up new releases means you’re less likely to be the one who installs it. The PR description says the goal is to ensure dependencies have had a chance to be scanned.

This matters more now that AI coding agents run `cargo add` and `cargo update` for us. An agent will happily pull a version that went up an hour ago, and a cooldown lives in Cargo config, where the resolver enforces it whatever the agent decides.

The config key is `registry.global-min-publish-age`, and per the Cargo config reference its default is `"0"`. Put this in `.cargo/config.toml` in your repo, or in your user-level Cargo config:

```
[registry]
global-min-publish-age = "7 days"
```
Accepted values are an integer followed by seconds, minutes, hours, days, weeks or months, or `"0"` to allow everything. For other registries there's `registries.<name>.min-publish-age`. The environment variable is `CARGO_REGISTRY_GLOBAL_MIN_PUBLISH_AGE`.

When a security fix needs to go in right away, the PR shows how to let one update through:

`$ CARGO_RESOLVER_INCOMPATIBLE_PUBLISH_AGE=allow cargo update -p foo`
That maps to `resolver.incompatible-publish-age`, which defaults to `"deny"`. The PR says the allowed version is then preserved in the lockfile.

I tried a 14-day setting with the beta Cargo on a fresh project. The locking message said it was resolving versions “as of 14 days ago”, and it listed a newer `syn` it had skipped because that version was published 47 hours earlier.

There are two limits. The PR says the resolver config doesn’t affect `cargo install`, matching how `incompatible-rust-version` works. The config docs also say the setting is ignored if a registry doesn't support it.

## Cargo lints, including unused_dependencies

Cargo now has a lint system of its own, configured in a `[lints.cargo]` table next to your existing `[lints.rust]` and `[lints.clippy]`. Per the Cargo lints reference, the initial set includes `unused_dependencies` (warn by default), `unused_workspace_dependencies`, `unknown_lints`, and two deny-by-default lints for text-direction codepoints in comments and literals.

`unused_dependencies` is the one I care about for agent-written code. Agents add crates while trying an approach, then switch approaches and leave the dependency behind. You can make that a hard error:

```
[lints.cargo]
unused_dependencies = "deny"
```
With two unused crates in a test project, `cargo check` on the beta failed with `unused dependency` errors pointing at each line in `Cargo.toml`. The lints docs say `cargo check` lints `[dependencies]` and `[build-dependencies]`, and doesn't lint `[dev-dependencies]` even with `--all-targets`.

The other Cargo change for supply chains is in `cargo install`. Per PR #17388, it now uses the lockfile packaged in the crate by default, so `--locked` becomes a no-op. That means you install with the dependency versions the author built and tested against.

## Smaller things worth knowing

These are all stable in 1.100 per the pre-release notes, and I checked the signatures in the std docs:

- `bool::toggle(&mut self)` flips a bool in place. It's handy for feature flags and streaming state.
- `Box::take` returns the value plus a`Box<MaybeUninit<T>>` , so you can reuse the allocation.`Box::map` ,`Rc::map` and`Arc::map` apply a function and reuse the allocation where possible.`Box::map` takes the value, while`Arc::map` gets a reference.
- `core::mem::DropGuard::new(value, f)` runs`f(value)` when the guard drops, and`DropGuard::dismiss` gets the value back without running it. I'd use it to flush a partial batch on early return.
- `extern "custom"` is stable, for naked functions with a calling convention Rust doesn't know. The PR's example is a compiler builtin,`__aeabi_uidivmod` , and Rust code can't call these functions directly; per the PR, calls go through inline assembly.
- `wasm32-wasip3` moves to Tier 2, so rustup will ship prebuilt std for it. PR #161940 says cooperative threading on the target isn't ready in the ecosystem yet.
- `i686-pc-windows-gnu` becomes Tier 2 without host tools. The Rust blog's October 2 post says`i686-pc-windows-msvc` also loses host tools in 1.100, so build on 64-bit Windows and cross-compile for 32-bit.

Here’s the small-API example I ran:

```
use std::mem::DropGuard;
use std::sync::Arc;
fn main() {
    let mut streaming = false;
    streaming.toggle();
    assert!(streaming);
    let b = Box::new(7);
    let (value, uninit) = Box::take(b);
    let b = Box::write(uninit, value + 1);
    let doubled = Box::map(b, |x| x * 2);
    assert_eq!(*doubled, 16);
    let cfg = Arc::new(String::from("max_tokens=512"));
    let len = Arc::map(cfg, |s| s.len());
    assert_eq!(*len, 14);
    let mut batch = DropGuard::new(Vec::<u32>::new(), |b| println!("flushing {} items", b.len()));
    batch.push(1);
    batch.push(2);
}
```
It prints `flushing 2 items` when `main` ends.

## Before you upgrade

The compatibility notes are long this time. These are the ones I’d check in an AI or infra codebase:

1. Never type fallback. Build on the pre-release with your real edition, and look for errors that mention `!` and issue #
2. `box` patterns are no longer syntactically allowed. They were nightly-only, but PR #156749 notes that some crates relied on the old gating, so a dependency could break.
3. New type inference errors from the generalized `Box` impls.
4. Closures in `if let` capture variables more precisely, and trait objects are checked for dyn-compatibility in more places. Both can cause new errors.
5. Cargo’s build directory layout changed. Per the pre-release notes, tools that read the build directory should use Cargo’s JSON messages instead.
6. On ARM, `-Ctarget-feature` and`-Ctarget-cpu` values that would change the ABI are now hard errors. If you build for edge devices with custom flags, try that build early.

The pre-release can be installed today with `RUSTUP_DIST_SERVER=https://dev-static.rust-lang.org rustup update stable`, per the internals thread. When I checked, the thread had no replies.

My own plan for November 12 is short. I’ll add the seven-day cooldown and `unused_dependencies = "deny"` to every repo where an agent writes code, and try an arena on the tokenizer path of one service.

If you read Rust 1.99 for AI Engineers, my last post in this series, this is the follow-up. Follow the Rust for AI series for the next one. Which of these will you turn on first?

## Quick answers

### When is Rust 1.100 released?

Rust 1.100.0 is scheduled for stable release on Thursday, November 12, 2026. A pre-release build was announced on the Rust internals forum on November 10 and can be installed with RUSTUP_DIST_SERVER=https://dev-static.rust-lang.org rustup update stable.

### What does the never type stabilization change in Rust 1.100?

You can now write the never type, !, anywhere a type goes, for example Result<T, !>. std::convert::Infallible becomes a type alias for !. The never type fallback also changes from () to ! on editions 2015, 2018 and 2021, matching what edition 2024 already does, which can break a small amount of code that relied on the old fallback.

### How do I turn on Cargo dependency cooldowns?

Set registry.global-min-publish-age in a Cargo config file, for example global-min-publish-age = “7 days” under [registry] in .cargo/config.toml. The default is “0”, so cooldowns are off until you opt in. Per-registry overrides use registries.<name>.min-publish-age, and CARGO_RESOLVER_INCOMPATIBLE_PUBLISH_AGE=allow lets a specific update through.

### Is the Allocator API fully stable in Rust 1.100?

A core subset is stable: the Allocator trait, Global and System, and allocator-aware methods on Vec and Box such as Vec::new_in, Vec::with_capacity_in and Box::new_in. Other collections and the rest of the old allocator_api feature, which the stabilization PR says will be renamed allocator_ext, remain unstable.
