---
id: collect-261121-veille-ia/langages-dev/allow-shortening-lifetime-in-coerceunsized-for-mut-by-theemathas-pull-request-149219-rust-lang-rust
title: "Allow shortening lifetime in CoerceUnsized for &mut by theemathas · Pull Request #149219 · rust-lang/rust"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-08-20"]
keywords: ["cell", "trait", "thing", "lifetime", "currently", "test", "change", "shortening"]
source: https://github.com/rust-lang/rust/pull/149219
source_anchor: ""
source_lines: [1, 321]
section: "Langages_Dev"
---
## Conversation

| I changed the PR to just edit the impl for `&mut` , and leave the impls for`Ref` and`RefMut` alone. I don't know if this is the right way to go though... | 

| So, there should be some test that we can add to observe this behavior change (stable or unstable). Without it, we definitely shouldn't be making it. I can think more about this and try to come up with such a test later this week, but @theemathas perhaps you can try before I get to it, since you're proposing the change there must be *some* reason. | 

| One thing that you could try is to remove the lifetime shortening on the other impl(s) and see if you can find tests that require that - and use that as a starting point for a similar test. | 

| I just realized: This change actually *does* have a visible effect in stable rust. Consider the following code: ``` use std::cell::Cell;  struct Thing; trait Trait {} impl Trait for Thing {}  fn works<'a: 'b, 'b>(x: Cell<&'a Thing>) -> Cell<&'b dyn Trait> {     x }  fn fails<'a: 'b, 'b>(x: Cell<&'a mut Thing>) -> Cell<&'b mut dyn Trait> {     x } ``` Currently, the `works` function compiles, and the`fails` function does not compile. ``` error: lifetime may not live long enough   --> src/lib.rs:12:5    \| 11 \| fn fails<'a: 'b, 'b>(x: Cell<&'a mut Thing>) -> Cell<&'b mut dyn Trait> {    \|          --      -- lifetime `'b` defined here    \|          \|    \|          lifetime `'a` defined here 12 \|     x    \|     ^ function was supposed to return data with lifetime `'a` but it is returning data with lifetime `'b`    \|    = help: consider adding the following bound: `'b: 'a`    = note: requirement occurs because of the type `Cell<&mut dyn Trait>`, which makes the generic argument `&mut dyn Trait` invariant    = note: the struct `Cell<T>` is invariant over the parameter `T`    = help: see <https://doc.rust-lang.org/nomicon/subtyping.html> for more information about variance  ``` This PR makes it so that both functions compile instead. I am now unsure on what the best way forward is. | 

| Even "no-op" unsize-coercions have strange behavior: ``` use std::cell::Cell; struct Thing; trait Trait {}  // currently errors fn with_thing<'a: 'b, 'b>(x: Cell<&'a Thing>) -> Cell<&'b Thing> {     x }  // currently compiles fn with_trait<'a: 'b, 'b>(x: Cell<&'a dyn Trait>) -> Cell<&'b dyn Trait> {     x }  // currently errors, will compile with this PR fn with_trait_mut<'a: 'b, 'b>(x: Cell<&'a mut dyn Trait>) -> Cell<&'b mut dyn Trait> {     x } ```  | 

| Okay, discussion on Zulip hasn't raised any issues here. Going to fcp merge for types here. I'm going to ping @rust-lang/libs-api, but I think this is probably "just okay" to have as a types team PR. This isn't really a new "API surface" as much as a change to what coercions we accept. This impl change allows (as an example) the following to compile: ``` // currently compiles fn with_trait<'a: 'b, 'b>(x: Cell<&'a dyn Trait>) -> Cell<&'b dyn Trait> {     x }  // currently errors, will compile with this PR fn with_trait_mut<'a: 'b, 'b>(x: Cell<&'a mut dyn Trait>) -> Cell<&'b mut dyn Trait> {     x } ``` @rfcbot merge types | 

| Team member @jackh726 has proposed to merge this. The next step is review by the rest of the tagged team members: No concerns currently listed. Once a majority of reviewers approve (and at most 2 approvals are outstanding), this will enter its final comment period. If you spot a major issue that hasn't been raised at any point in this process, please speak up! See this document for info about what commands tagged team members can give me. | 

Rollup of 7 pull requests
Successful merges:
 - #157681 (Add instrument_fn attribute)
 - #149219 (Allow shortening lifetime in CoerceUnsized for &mut)
 - #157539 (Rename `RandomSource` -> `Rng`, `DefaultRandomSource` -> `SystemRng`)
 - #157980 (Some minor cleanups around hir ty/pat/expr)
 - #157988 (Fix incremental-finalize-fail proc macro test on AIX)
 - #157989 (run-make: handle AIX symbol cdylib export test)
 - #157998 (Add big disclaimer to the description of lint `explicit_outlives_requirements`)

Rollup merge of #149219 - theemathas:coerce-unsized-shorten, r=jackh726
Allow shortening lifetime in CoerceUnsized for &mut
This modifies the &mut -> &mut CoerceUnsized impl so that, it can shorten the lifetime.
Note that there are already two impls that allow shortening the lifetime like this (the &mut T -> &U and the &T -> &U impls). So this change makes the impls consistent with each other.
I initially tried to also do the same to the CoerceUnsized impl for core::cell::{Ref, RefMut}. However, this can't be done because Ref and RefMut "store" the lifetime and the data in different fields, and CoerceUnsized can only coerce one field.
This change has an effect on stable code, since it allows shortening lifetimes inside invariant types via a &mut -> &mut unsize coercion.

This MR contains the following updates:
| Package | Type | Update | Change |
|---|---|---|---|
| [rust](https://github.com/rust-lang/rust) | tools | minor | `1.97.1` → `1.98.0` |
MR created with the help of [el-capitano/tools/renovate-bot](https://gitlab.com/el-capitano/tools/renovate-bot).
**Proposed changes to behavior should be submitted there as MRs.**
---
### Release Notes
<details>
<summary>rust-lang/rust (rust)</summary>
### [`v1.98.0`](https://github.com/rust-lang/rust/blob/HEAD/RELEASES.md#Version-1980-2026-08-20)
[Compare Source](rust-lang/rust@`1.97.1...1.98.0`)
\==========================
<a id="1.98.0-Language"></a>
## Language
- [Allow shortening lifetime of `&mut` when unsize-coercing, even in an invariant position.](rust-lang/rust#149219) For example, you can now coerce a `Cell<&'long mut i32>` to a `Cell<&'short mut dyn Send>`. Such shortenings were already previously allowed when coercing a `&mut` to a `&`, or coercing a `&` to a `&`.
- [Add deny-by-default `invalid_runtime_symbol_definitions` lint and warn-by-default `suspicious_runtime_symbol_definitions` lint](rust-lang/rust#155521)
  - The lints currently specifically targets `core` runtime symbols like `memcmp`, `memset`, `strlen`, ... and is planned to be expanded in the next few releases.
- [Add warn-by-default `c_void_returns` lint to check `core::ffi::c_void` as a return type](rust-lang/rust#156379)
<a id="1.98.0-Platform-Support"></a>
## Platform Support
- [Add `powerpc64-unknown-linux-gnuelfv2` as Tier 3](rust-lang/rust#144220)
- [Add `aarch64-unknown-linux-pauthtest` as Tier 3 target](rust-lang/rust#155722)
- [Promote `thumbv7a-none-eabi` to Tier 2](rust-lang/rust#155763)
- [Promote `thumbv7a-none-eabihf` to Tier 2](rust-lang/rust#155763)
- [Promote `thumbv7r-none-eabi` to Tier 2](rust-lang/rust#155763)
- [Promote `thumbv7r-none-eabihf` to Tier 2](rust-lang/rust#155763)
- [Promote `thumbv8r-none-eabihf` to Tier 2](rust-lang/rust#155763)
Refer to Rust's [platform support page][platform-support-doc]
for more information on Rust's tiered platform support.
[platform-support-doc]: https://doc.rust-lang.org/rustc/platform-support.html
<a id="1.98.0-Libraries"></a>
## Libraries
- [Change `Location<'_>` lifetime to `'static` in `Panic[Hook]Info`](rust-lang/rust#146561)
- [Document panic in `RangeInclusive::from(legacy::RangeInclusive)`](rust-lang/rust#155421)
- [Document that `ManuallyDrop`'s `Box` interaction has been fixed](rust-lang/rust#155750)
- [Stabilize LoongArch CRC Intrinsics](rust-lang/rust#156908)
- [The `derive` macro is available at `{core,std}::derive`.](rust-lang/rust#154645) This was previously [unintentionally stabilized in 1.96](rust-lang/rust#159856), but is now [explicitly accepted](rust-lang/rust#154645) as a stabilized API.
  - Please note that the MSRV for `{core,std}::derive` will be 1.96, and not 1.98.
<a id="1.98.0-Stabilized-APIs"></a>
## Stabilized APIs
- [`str::substr_range`](https://doc.rust-lang.org/stable/std/primitive.str.html#method.substr_range)
- [`[T]::subslice_range`](https://doc.rust-lang.org/stable/std/primitive.slice.html#method.subslice_range)
- [`core::fmt::NumBuffer`](https://doc.rust-lang.org/stable/core/fmt/struct.NumBuffer.html)
- [`<{integer}>::format_into`](https://doc.rust-lang.org/stable/core/primitive.usize.html#method.format_into)
- [`Send/Sync for std::process::CommandArgs`](https://doc.rust-lang.org/stable/std/process/struct.CommandArgs.html#impl-Send-for-CommandArgs%3C'a%3E)
- [`{fN}::algebraic_add`](https://doc.rust-lang.org/stable/core/primitive.f32.html#method.algebraic_add)
- [`{fN}::algebraic_sub`](https://doc.rust-lang.org/stable/core/primitive.f32.html#method.algebraic_sub)
- [`{fN}::algebraic_mul`](https://doc.rust-lang.org/stable/core/primitive.f32.html#method.algebraic_mul)
- [`{fN}::algebraic_div`](https://doc.rust-lang.org/stable/core/primitive.f32.html#method.algebraic_div)
- [`{fN}::algebraic_rem`](https://doc.rust-lang.org/stable/core/primitive.f32.html#method.algebraic_rem)
- [`NonZero<{integer}>::from_str_radix`](https://doc.rust-lang.org/stable/core/num/struct.NonZero.html#method.from_str_radix-4)
- [`String::from_utf16le`](https://doc.rust-lang.org/stable/std/string/struct.String.html#method.from_utf16le)
- [`String::from_utf16le_lossy`](https://doc.rust-lang.org/stable/std/string/struct.String.html#method.from_utf16le_lossy)
- [`String::from_utf16be`](https://doc.rust-lang.org/stable/std/string/struct.String.html#method.from_utf16be)
- [`String::from_utf16be_lossy`](https://doc.rust-lang.org/stable/std/string/struct.String.html#method.from_utf16be_lossy)
- [`[T]::strip_circumfix`](https://doc.rust-lang.org/stable/core/primitive.slice.html#method.strip_circumfix)
- [`str::strip_circumfix`](https://doc.rust-lang.org/stable/core/primitive.str.html#method.strip_circumfix)
- [`Atomic<T>::from_mut`](https://doc.rust-lang.org/stable/core/sync/atomic/struct.Atomic.html#method.from_mut)
- [`Atomic<T>::get_mut_slice`](https://doc.rust-lang.org/stable/core/sync/atomic/struct.Atomic.html#method.get_mut_slice)
- [`Atomic<T>::from_mut_slice`](https://doc.rust-lang.org/stable/core/sync/atomic/struct.Atomic.html#method.from_mut_slice)
- [`std::range::legacy`](https://doc.rust-lang.org/stable/std/range/legacy/index.html)
<a id="1.98.0-Compatibility-Notes"></a>
## Compatibility Notes
- [If fully elided, lifetime bounds of trait object types may now resolve differently or even get rejected in very specific niche scenarios](rust-lang/rust#129543)
- [Error in more cases of ambiguous imports](rust-lang/rust#145108)
- [Switch the destructors implementation for thread locals on Windows to use Fiber Local Storage (FLS)](rust-lang/rust#148799)
- [Convert some cases of the `ambiguous_glob_imports` lint into a hard error](rust-lang/rust#149195)
- [Where-bounds of the form `Type = Type` and `Type == Type` are no longer syntactically allowed](rust-lang/rust#153513)
- [Ensure Send/Sync is not implemented for std::env::Vars{,Os}](rust-lang/rust#155153)
- [Fix that in some attributes, arguments were not properly rejected](rust-lang/rust#155193)
- [`repr(transparent)` is now more strict about which fields have "trivial" layout and hence can be ignored: `repr(C)` types, types with private fields, and `#[non_exhaustive]` types are no longer considered "trivial"](rust-lang/rust#155299)
- [Correctly check whether types have equal size in `transmute()` when some `repr` attributes are involved.](rust-lang/rust#155418)
- [More characters are escaped when printing strings and chars](rust-lang/rust#155527)
- [Implement fast path for `derive(PartialOrd)` when deriving `Ord`](rust-lang/rust#155598)
  This can break crates in practice where a type's PartialOrd and Ord impls were inconsistent with each other.
- [Add temporary scope to `assert_eq` and `assert_ne`](rust-lang/rust#155739)
- Closed a hole in the pattern matching [structural equality](https://doc.rust-lang.org/reference/patterns.html#constant-patterns) check, preventing cases where a match of a constant would be allowed, despite disagreeing with a manually written `PartialEq` implementation, when a `derive(PartialEq)` implementation for that type also exists.
- [On Emscripten the WASM exception handling ABI is now unconditionally used](rust-lang/rust#156928) The `-Zemscripten-wasm-eh=false` flag to switch back to JS exceptions has been removed.
- [The UNSAFE\_CODE lint is now consistently emitted for all unsafe attributes](rust-lang/rust#157201)
- [Solaris: remove `File::lock` implementation, it has the wrong semantics (return "unsupported" instead)](rust-lang/rust#157509)
- [Windows-gnu targets now specify baseline tools versions](rust-lang/rust#158020)
- [rustfmt now discovers module files that are defined in `cfg_select!`](rust-lang/rust#158372)
  This may cause more code to be formatted which was previously ignored.
</details>
---
### Configuration
📅 **Schedule**: (UTC)
- Branch creation
  - At any time (no schedule defined)
- Automerge
  - At any time (no schedule defined)
🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.
♻ **Rebasing**: Whenever MR becomes conflicted, or you tick the rebase/retry checkbox.
🔕 **Ignore**: Close this MR and you won't be reminded about this update again.
---
 - [ ] <!-- rebase-check -->If you want to rebase/retry this MR, check this box
---
This MR has been generated by [Mend Renovate](https://github.com/renovatebot/renovate).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0My4yODguMCIsInVwZGF0ZWRJblZlciI6IjQzLjI4OC4wIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJSZW5vdmF0ZSBCb3QiLCJhdXRvbWF0aW9uOmJvdC1hdXRob3JlZCIsImRlcGVuZGVuY3ktdHlwZTo6bWlub3IiXX0=-->

Pkgsrc changes:
 * Update patches to match new vendored versions.
 * Associated checksum changes.
 * Add cargo option to permit linker warnings (NetBSD compat code)
   (taken from rust-beta wip package)
Upstream changes:
Version 1.98.0 (2026-08-20)
==========================
Language
--------
- [Allow shortening lifetime of `&mut` when unsize-coercing, even
  in an invariant position.]
  (rust-lang/rust#149219) For example, you
  can now coerce a `Cell<&'long mut i32>` to a `Cell<&'short mut
  dyn Send>`. Such shortenings were already previously allowed when
  coercing a `&mut` to a `&`, or coercing a `&` to a `&`.
- [Add deny-by-default `invalid_runtime_symbol_definitions` lint
  and warn-by-default `suspicious_runtime_symbol_definitions` lint]
  (rust-lang/rust#155521)
    - The lints currently specifically targets `core` runtime
      symbols like `memcmp`, `memset`, `strlen`, ... and is planned
      to be expanded in the next few releases.
- [Add warn-by-default `c_void_returns` lint to check `core::ffi::c_void`
  as a return type] (rust-lang/rust#156379)
Platform Support
----------------
- [Add `powerpc64-unknown-linux-gnuelfv2` as Tier 3]
  (rust-lang/rust#144220)
- [Add `aarch64-unknown-linux-pauthtest` as Tier 3 target]
  (rust-lang/rust#155722)
- [Promote `thumbv7a-none-eabi` to Tier 2]
  (rust-lang/rust#155763)
- [Promote `thumbv7a-none-eabihf` to Tier 2]
  (rust-lang/rust#155763)
- [Promote `thumbv7r-none-eabi` to Tier 2]
  (rust-lang/rust#155763)
- [Promote `thumbv7r-none-eabihf` to Tier 2]
  (rust-lang/rust#155763)
- [Promote `thumbv8r-none-eabihf` to Tier 2]
  (rust-lang/rust#155763)
Refer to Rust's [platform support page][platform-support-doc]
for more information on Rust's tiered platform support.
[platform-support-doc]: https://doc.rust-lang.org/rustc/platform-support.html
Libraries
---------
- [Change `Location<'_>` lifetime to `'static` in `Panic[Hook]Info`]
  (rust-lang/rust#146561)
- [Document panic in `RangeInclusive::from(legacy::RangeInclusive)`]
  (rust-lang/rust#155421)
- [Document that `ManuallyDrop`'s `Box` interaction has been fixed]
  (rust-lang/rust#155750)
- [Stabilize LoongArch CRC Intrinsics]
  (rust-lang/rust#156908)
- [The `derive` macro is available at `{core,std}::derive`.]
  (rust-lang/rust#154645) This was
  previously [unintentionally stabilized in 1.96]
  (rust-lang/rust#159856), but is now
  [explicitly accepted] (rust-lang/rust#154645)
  as a stabilized API.
- Please note that the MSRV for
  `{core,std}::derive` will be 1.96, and not 1.98.
Stabilized APIs
---------------
- [`str::substr_range`]
  (https://doc.rust-lang.org/stable/std/primitive.str.html#method.substr_range)
- [`[T]::subslice_range`]
  (https://doc.rust-lang.org/stable/std/primitive.slice.html#method.subslice_range)
- [`core::fmt::NumBuffer`]
  (https://doc.rust-lang.org/stable/core/fmt/struct.NumBuffer.html)
- [`<{integer}>::format_into`]
  (https://doc.rust-lang.org/stable/core/primitive.usize.html#method.format_into)
- [`Send/Sync for std::process::CommandArgs`]
  (https://doc.rust-lang.org/stable/std/process/struct.CommandArgs.html#impl-Send-for-CommandArgs%3C'a%3E)
- [`{fN}::algebraic_add`]
  (https://doc.rust-lang.org/stable/core/primitive.f32.html#method.algebraic_add)
- [`{fN}::algebraic_sub`]
  (https://doc.rust-lang.org/stable/core/primitive.f32.html#method.algebraic_sub)
- [`{fN}::algebraic_mul`]
  (https://doc.rust-lang.org/stable/core/primitive.f32.html#method.algebraic_mul)
- [`{fN}::algebraic_div`]
  (https://doc.rust-lang.org/stable/core/primitive.f32.html#method.algebraic_div)
- [`{fN}::algebraic_rem`]
  (https://doc.rust-lang.org/stable/core/primitive.f32.html#method.algebraic_rem)
- [`NonZero<{integer}>::from_str_radix`]
  (https://doc.rust-lang.org/stable/core/num/struct.NonZero.html#method.from_str_radix-4)
- [`String::from_utf16le`]
  (https://doc.rust-lang.org/stable/std/string/struct.String.html#method.from_utf16le)
- [`String::from_utf16le_lossy`]
  (https://doc.rust-lang.org/stable/std/string/struct.String.html#method.from_utf16le_lossy)
- [`String::from_utf16be`]
  (https://doc.rust-lang.org/stable/std/string/struct.String.html#method.from_utf16be)
- [`String::from_utf16be_lossy`]
  (https://doc.rust-lang.org/stable/std/string/struct.String.html#method.from_utf16be_lossy)
- [`[T]::strip_circumfix`]
  (https://doc.rust-lang.org/stable/core/primitive.slice.html#method.strip_circumfix)
- [`str::strip_circumfix`]
  (https://doc.rust-lang.org/stable/core/primitive.str.html#method.strip_circumfix)
- [`Atomic<T>::from_mut`]
  (https://doc.rust-lang.org/stable/core/sync/atomic/struct.Atomic.html#method.from_mut)
- [`Atomic<T>::get_mut_slice`]
  (https://doc.rust-lang.org/stable/core/sync/atomic/struct.Atomic.html#method.get_mut_slice)
- [`Atomic<T>::from_mut_slice`]
  (https://doc.rust-lang.org/stable/core/sync/atomic/struct.Atomic.html#method.from_mut_slice)
- [`std::range::legacy`]
  (https://doc.rust-lang.org/stable/std/range/legacy/index.html)
Compatibility Notes
-------------------
- [If fully elided, lifetime bounds of trait object types may now
  resolve differently or even get rejected in very specific niche
  scenarios] (rust-lang/rust#129543)
- [Error in more cases of ambiguous imports]
  (rust-lang/rust#145108)
- [Switch the destructors implementation for thread locals on
  Windows to use Fiber Local Storage (FLS)]
  (rust-lang/rust#148799)
- [Convert some cases of the `ambiguous_glob_imports` lint into a hard error]
  (rust-lang/rust#149195)
- [Where-bounds of the form `Type = Type` and `Type == Type` are
  no longer syntactically allowed]
  (rust-lang/rust#153513)
- [Ensure Send/Sync is not implemented for std::env::Vars{,Os}]
  (rust-lang/rust#155153)
- [Fix that in some attributes, arguments were not properly rejected]
  (rust-lang/rust#155193)
- [`repr(transparent)` is now more strict about which fields have
  "trivial" layout and hence can be ignored: `repr(C)` types, types
  with private fields, and `#[non_exhaustive]` types are no longer
  considered "trivial"] (rust-lang/rust#155299)
- [Correctly check whether types have equal size in `transmute()`
  when some `repr` attributes are involved.]
  (rust-lang/rust#155418)
- [More characters are escaped when printing strings and chars]
  (rust-lang/rust#155527)
- [Implement fast path for `derive(PartialOrd)` when deriving `Ord`]
  (rust-lang/rust#155598)
  This can break crates in practice where a type's PartialOrd and
  Ord impls were inconsistent with each other.
- [Add temporary scope to `assert_eq` and `assert_ne`]
  (rust-lang/rust#155739)
- Closed a hole in the pattern matching [structural equality]
  (https://doc.rust-lang.org/reference/patterns.html#constant-patterns)
  check, preventing cases where a match of a constant would be
  allowed, despite disagreeing with a manually written `PartialEq`
  implementation, when a `derive(PartialEq)` implementation for
  that type also exists.
- [On Emscripten the WASM exception handling ABI is now unconditionally used]
  (rust-lang/rust#156928) The
  `-Zemscripten-wasm-eh=false` flag to switch back to JS exceptions
  has been removed.
- [The UNSAFE_CODE lint is now consistently emitted for all unsafe attributes]
  (rust-lang/rust#157201)
- [Solaris: remove `File::lock` implementation, it has the wrong
  semantics (return "unsupported" instead)]
  (rust-lang/rust#157509)
- [Windows-gnu targets now specify baseline tools versions]
  (rust-lang/rust#158020)
- [rustfmt now discovers module files that are defined in `cfg_select!`]
  (rust-lang/rust#158372)
  This may cause more code to be formatted which was previously ignored.

- Re-probe every quoted diagnostic on rustc 1.98.1 (was 1.97.0).
- Add the unsize trap: CoerceUnsized lets an invariant Cell shorten.
- Note 1.98 extends it to an inner &mut (rust-lang/rust#149219).
- Wrap probe types in Vec; add a 1.97.1 vs 1.98.1 unsize table.
- Fix struct rule: mixed co- and contravariant uses give invariance.
- Add the 1.94 rule: raw-pointer casts cannot extend dyn lifetimes.
- Add a PhantomData<fn(T) -> T> row and a compile_fail doctest lock.
- Move worked pairs and the exact-owner accessor to variance-tables.md.
- Drop Purpose and Checklist; SKILL.md keeps triage and the rules.
- Rewrite description to lead with the error text; routing unchanged.

…epted, release by release
tests/stabilized.rs: each change of RELEASES.md's Language sections that decides whether a
program builds, as a small crate checked with nothing said, 26 tests; where a release added a
diagnostic, rustc's own UI test and .stderr at the pinned commit (clone-never, method-on-never,
repr-packed-contains-align, detect-invisible-delimiter).
minicore's `&mut T: CoerceUnsized<&mut U>` may shorten the lifetime, as core's since 1.98
(rust-lang/rust#149219).

View all comments
This modifies the &mut -> &mut CoerceUnsized impl so that, it can shorten the lifetime.

Note that there are already two impls that allow shortening the lifetime like this (the &mut T -> &U and the &T -> &U impls). So this change makes the impls consistent with each other.

I initially tried to also do the same to the CoerceUnsized impl for core::cell::{Ref, RefMut}. However, this can't be done because Ref and RefMut "store" the lifetime and the data in different fields, and CoerceUnsized can only coerce one field.

This change has an effect on stable code, since it allows shortening lifetimes inside invariant types via a &mut -> &mut unsize coercion.
