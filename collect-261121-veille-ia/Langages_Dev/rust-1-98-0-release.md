---
id: collect-261121-veille-ia/langages-dev/rust-1-98-0-release
title: "Rust 1.98.0 release"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["rust", "rust-lang", "github", "pull", "tier", "note", "release-notes", "lint"]
source: https://verdict-tracker.vercel.app/technologies/rust/1.98.0
source_anchor: ""
source_lines: [1, 99]
section: "Langages_Dev"
---
|On Emscripten the WASM exception handling ABI is now unconditionally used ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

=help: Remove any use of the -Zemscripten-wasm-eh=false flag from your build configuration.

key points

note[1]:Several compatibility changes can break existing code, so check them before upgrading.release-notes:68

note[2]:Mutable references can now have their lifetimes shortened when unsize-coercing, even in invariant positions.release-notes:5

note[3]:A new deny-by-default lint flags invalid definitions of core runtime symbols such as memcmp.release-notes:6

note[4]:Several thumbv7 and thumbv8r bare-metal targets are promoted to Tier 2.release-notes:16

note[5]:LoongArch CRC intrinsics are stabilized.release-notes:31

release-notesgithub.com/rust-lang/rust · 84 lines

<a id="1.98.0-Language"></a>

## Language

- [Allow shortening lifetime of `&mut` when unsize-coercing, even in an invariant position.](https://github.com/rust-lang/rust/pull/149219) For example, you can now coerce a `Cell<&'long mut i32>` to a `Cell<&'short mut dyn Send>`. Such shortenings were already previously allowed when coercing a `&mut` to a `&`, or coercing a `&` to a `&`.

- [Add deny-by-default `invalid_runtime_symbol_definitions` lint and warn-by-default `suspicious_runtime_symbol_definitions` lint](https://github.com/rust-lang/rust/pull/155521)

- The lints currently specifically targets `core` runtime symbols like `memcmp`, `memset`, `strlen`, ... and is planned to be expanded in the next few releases.

- [Add warn-by-default `c_void_returns` lint to check `core::ffi::c_void` as a return type](https://github.com/rust-lang/rust/pull/156379)

<a id="1.98.0-Platform-Support"></a>

## Platform Support

- [Add `powerpc64-unknown-linux-gnuelfv2` as Tier 3](https://github.com/rust-lang/rust/pull/144220)

- [Add `aarch64-unknown-linux-pauthtest` as Tier 3 target](https://github.com/rust-lang/rust/pull/155722)

- [Promote `thumbv7a-none-eabi` to Tier 2](https://github.com/rust-lang/rust/pull/155763)

- [Promote `thumbv7a-none-eabihf` to Tier 2](https://github.com/rust-lang/rust/pull/155763)

- [Promote `thumbv7r-none-eabi` to Tier 2](https://github.com/rust-lang/rust/pull/155763)

- [Promote `thumbv7r-none-eabihf` to Tier 2](https://github.com/rust-lang/rust/pull/155763)

- [Promote `thumbv8r-none-eabihf` to Tier 2](https://github.com/rust-lang/rust/pull/155763)

Refer to Rust's [platform support page](https://doc.rust-lang.org/rustc/platform-support.html) for more information on Rust's tiered platform support.

<a id="1.98.0-Libraries"></a>

## Libraries

- [Change `Location<'_>` lifetime to `'static` in `Panic[Hook]Info`](https://github.com/rust-lang/rust/pull/146561)

- [Document panic in `RangeInclusive::from(legacy::RangeInclusive)`](https://github.com/rust-lang/rust/pull/155421)

- [Document that `ManuallyDrop`'s `Box` interaction has been fixed](https://github.com/rust-lang/rust/pull/155750)

- [The `derive` macro is available at `{core,std}::derive`.](https://github.com/rust-lang/rust/issues/154645) This was previously [unintentionally stabilized in 1.96](https://github.com/rust-lang/rust/issues/159856), but is now [explicitly accepted](https://github.com/rust-lang/rust/issues/154645) as a stabilized API.

- Please note that the MSRV for `{core,std}::derive` will be 1.96, and not 1.98.

- [If fully elided, lifetime bounds of trait object types may now resolve differently or even get rejected in very specific niche scenarios](https://github.com/rust-lang/rust/pull/129543)

- [Error in more cases of ambiguous imports](https://github.com/rust-lang/rust/pull/145108)

- [Switch the destructors implementation for thread locals on Windows to use Fiber Local Storage (FLS)](https://github.com/rust-lang/rust/pull/148799)

- [Convert some cases of the `ambiguous_glob_imports` lint into a hard error](https://github.com/rust-lang/rust/pull/149195)

- [Where-bounds of the form `Type = Type` and `Type == Type` are no longer syntactically allowed](https://github.com/rust-lang/rust/pull/153513)

- [Ensure Send/Sync is not implemented for std::env::Vars{,Os}](https://github.com/rust-lang/rust/pull/155153)

- [Fix that in some attributes, arguments were not properly rejected](https://github.com/rust-lang/rust/pull/155193)

- [`repr(transparent)` is now more strict about which fields have "trivial" layout and hence can be ignored: `repr(C)` types, types with private fields, and `#[non_exhaustive]` types are no longer considered "trivial"](https://github.com/rust-lang/rust/pull/155299)

- [Correctly check whether types have equal size in `transmute()` when some `repr` attributes are involved.](https://github.com/rust-lang/rust/pull/155418)

- [More characters are escaped when printing strings and chars](https://github.com/rust-lang/rust/pull/155527)

- [Implement fast path for `derive(PartialOrd)` when deriving `Ord`](https://github.com/rust-lang/rust/pull/155598) This can break crates in practice where a type's PartialOrd and Ord impls were inconsistent with each other.

- [Add temporary scope to `assert_eq` and `assert_ne`](https://github.com/rust-lang/rust/pull/155739)

- Closed a hole in the pattern matching [structural equality](https://doc.rust-lang.org/reference/patterns.html#constant-patterns) check, preventing cases where a match of a constant would be allowed, despite disagreeing with a manually written `PartialEq` implementation, when a `derive(PartialEq)` implementation for that type also exists.

- [On Emscripten the WASM exception handling ABI is now unconditionally used](https://github.com/rust-lang/rust/pull/156928) The `-Zemscripten-wasm-eh=false` flag to switch back to JS exceptions has been removed.

- [The UNSAFE\_CODE lint is now consistently emitted for all unsafe attributes](https://github.com/rust-lang/rust/pull/157201)

- [Solaris: remove `File::lock` implementation, it has the wrong semantics (return "unsupported" instead)](https://github.com/rust-lang/rust/pull/157509)

- [Windows-gnu targets now specify baseline tools versions](https://github.com/rust-lang/rust/pull/158020)

- [rustfmt now discovers module files that are defined in `cfg_select!`](https://github.com/rust-lang/rust/pull/158372) This may cause more code to be formatted which was previously ignored.
