---
id: collect-261121-veille-ia/langages-dev/rust-1-100-3-errors-become-zero-with-the-never-type
title: "Rust 1.100: 3 errors become zero with the never type"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-12"]
keywords: ["type", "never", "result", "nums", "rust", "errors", "infallible", "error"]
source: https://dev.to/frankchu/rust-1100-3-errors-become-zero-with-the-never-type-4kae
source_anchor: ""
source_lines: [1, 118]
section: "Langages_Dev"
---
Rust 1.100.0 shipped today, and the never type took 10 years and 5 failed stabilization attempts to land. I installed both 1.99.0 and 1.100.0 and compiled the same file against each: 3 errors on 1.99, zero on 1.100 — and the new Cargo features that shipped alongside it are the ones that will actually change your builds.

The never type, spelled `!`, is the type of computations that never return a value. You could already write it as a function's return type (`-> !`), but naming it anywhere else — inside a `Result`, in a type annotation, in a generic argument — was an experimental feature gated behind issue #35121 since 2016. This release finally stabilizes it, and also makes `Infallible` an alias for `!`, which is what the type was always supposed to be.

Here is the file I compiled against both toolchains:


```
fn infallible_unwrap<T>(res: Result<T, !>) -> T {
    let Ok(v) = res;
    v
}
fn main() {
    let r: Result<u32, !> = Ok(42);
    println!("unwrapped: {}", infallible_unwrap(r));
    let _: Result<String, !> = Ok("never type named in a type position".to_string());
    println!("named ! in a type alias position: compiles");
}
```
1.99.0:


```
error[E0658]: the `!` type is experimental
 --> never.rs:1:40
  |
1 | fn infallible_unwrap<T>(res: Result<T, !>) -> T {
  |                                        ^
  |
  = note: see issue #35121 for more information
error[E0658]: the `!` type is experimental
 --> never.rs:7:24
  |
7 |     let r: Result<u32, !> = Ok(42);
  |                        ^
error[E0658]: the `!` type is experimental
 --> never.rs:9:27
  |
9 |     let _: Result<String, !> = Ok("never type named in a type position".to_string());
```
Three errors, one per use site. 1.100.0 compiles it clean and runs:


```
unwrapped: 42
named ! in a type alias position: compiles
```
The interesting part of `Result<T, !>` is what it says about the error channel: a function that returns `Result<T, !>` *cannot fail*. The `let Ok(v) = res;` in my example is irrefutable — there is no `Err` arm to write, because the type system knows no value of type `!` can exist. Before this release you had to spell that with `Infallible` and pretend it was a real error type; now the alias makes the two spellings the same type.

One breaking change rides along, and it is the kind that only shows up in weird code: the "never type fallback" in editions before 2024 changes from `()` to `!`. The release notes say it "should largely be unnoticeable" but can make the compiler detect more code as unreachable. I tried to trigger it with the documented shape — ignoring the result of a function returning a generic parameter — and got the identical `E0283: type annotations needed` on both toolchains, so the fallback change did not show up in my test at all. The Rust team says it already shipped fixes to public crates that were broken by it, which is the polite way of saying the ecosystem already absorbed the hit somewhere stranger than my test program.

The second headline feature is `core::mem::DropGuard`, a helper that runs a closure when it is dropped — the "run this on every exit path" primitive that previously required hand-writing a `Drop` impl. The release notes give a transactional parse example; I ran it verbatim:


```
use std::mem::DropGuard;
use std::num::ParseIntError;
fn parse_all(nums: &mut Vec<u32>, strings: &[&str]) -> Result<(), ParseIntError> {
    let len = nums.len();
    let mut nums = DropGuard::new(nums, move |nums| {
        nums.truncate(len);
    });
    for s in strings {
        nums.push(s.parse()?);
    }
    DropGuard::dismiss(nums);
    Ok(())
}
```
```
bad input -> true, vec after rollback: [1, 2, 3]
good input -> true, vec after commit: [1, 2, 3, 4, 5, 6]
```
The `?` on the failing parse returns early, the guard drops, the vec truncates back to length 3. On the success path `dismiss` hands the vec back without running the closure. It is a small API — `new`, `dismiss`, `Deref`/`DerefMut` — and it deletes an entire class of "forgot to restore state on the error path" bugs. The honest assessment: this is the least exciting feature in the release and the one I will actually use first.

Cargo now reports unused dependencies by default, as part of a new `[lints.cargo]` system. I built a toy crate depending on `itoa` and never using it:


```
warning: unused dependency `itoa`
 --> Cargo.toml:7:1
  |
7 | itoa = "1.0"
  | ^^^^^^^^^^^^
  |
  = note: `cargo::unused_dependencies` is set to `warn` by default
help: consider removing the dependency on `itoa`
```
It compiled `itoa v1.0.19` first and then warned — the lint fires at build time, not at resolve time, and only when every build target that could use the dependency is actually built (`cargo check --bin foo` will not warn, because another binary in the package might use it). No warning is shown for `dev-dependencies` at all. I only tested a single-binary crate, so I did not verify the multi-bin false-positive fix myself — the release notes describe it, my run does not prove it. Worth turning on in CI as a warning; worth leaving as a warning, not an error, until you have seen it on your own workspace.

The supply-chain feature is `global-min-publish-age`: tell Cargo to skip any crate version published more recently than N days, on the theory that malicious versions are usually caught within hours or days of publication. I set a 3-day cooldown and pointed a crate at `link-cli = "3"`:


```
error: failed to select a version for the requirement `link-cli = "^3"`
  version 3.0.0 is too new (published 2 days ago, minimum age 3 days)
  version 3.0.1 is too new (published 7 minutes ago, minimum age 3 days)
location searched: crates.io index
help: to preserve the min-publish-age, downgrade the requirement to "2.1.2"
help: to use too-new packages anyways, re-resolve with `CARGO_RESOLVER_INCOMPATIBLE_PUBLISH_AGE=allow`
```
That is not a holdback — that is a refusal to resolve at all. Every 3.x release of `link-cli` was younger than the window (3.0.1 was 7 minutes old when I ran it), so Cargo gave up and told me to downgrade to 2.1.2. The error message is good — it names both versions, their ages, and the exact escape hatch — but the behavior is worth knowing before you set a global 3-day cooldown on a workspace that depends on fast-moving crates. A cooldown is a policy with a failure mode, and the failure mode is "your build stops resolving on release day."

One more check, because the release notes' own example deserves the same treatment. The blog shows `cargo update` holding back `clap 4.6.9, published 2 days ago` in favor of 4.6.7. I checked crates.io: the latest clap release is 4.6.7, published September 14. There is no 4.6.9. The example is illustrative, not literal — the mechanism is real (my `link-cli` run proves it), but the numbers in the announcement are made up. Read the release notes the way you read benchmarks: verify the example, trust the mechanism.

| Feature | Measured result | 
|---|---|
| Never type on 1.99.0 | 3 × E0658 errors | 
| Never type on 1.100.0 | Compiles, runs, prints 42 | 
| DropGuard rollback | `[1, 2, 3]` restored on error,`[1, 2, 3, 4, 5, 6]` on success | 
| Unused-deps lint | Fires on unused `itoa` , names the line in Cargo.toml | 
| min-publish-age, 3 days | Refused to resolve `link-cli 3.x` (3.0.0: 2 days old, 3.0.1: 7 min old) | 

Ten years for a two-character type. The never type is the headline, but the thing that will change your Monday is the Cargo lint telling you which dependencies you forgot to delete, and the cooldown telling you — bluntly, at resolve time — that the crate you want was published seven minutes ago.

Sources: Rust blog, "Announcing Rust 1.100.0" (November 12, 2026); crates.io API, clap and link-cli version listings (checked November 12, 2026); all compiler and Cargo outputs are my own runs on rustc/cargo 1.99.0 and 1.100.0, November 12, 2026.

## Top comments (0)
