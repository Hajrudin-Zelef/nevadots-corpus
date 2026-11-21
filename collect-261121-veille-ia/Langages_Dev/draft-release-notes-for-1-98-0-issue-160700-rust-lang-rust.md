---
id: collect-261121-veille-ia/langages-dev/draft-release-notes-for-1-98-0-issue-160700-rust-lang-rust
title: "Draft release notes for 1.98.0 · Issue #160700 · rust-lang/rust"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-08-20"]
keywords: ["release", "issue", "notes", "type", "rust", "types", "lint", "rust-lang"]
source: https://github.com/rust-lang/rust/issues/160700
source_anchor: ""
source_lines: [1, 135]
section: "Langages_Dev"
---
## Description

**NOTE:** Use the 📝 links to edit those that have a `relnotes-tracking-issue`,

and they will be updated when we regenerate the notes periodically (manually).

# Version 1.98.0 (2026-08-20)

## Language

- Allow shortening lifetime of `&mut` when unsize-coercing, even in an invariant position. For example, you can now coerce a`Cell<&'long mut i32>` to a`Cell<&'short mut dyn Send>` . Such shortenings were already previously allowed when coercing a`&mut` to a`&` , or coercing a`&` to a`&` .
📝
- riscv: `d` ,`e` , and`f` target_features are now stable in`cfg(target_feature = "?")`
📝

## Compiler

## Platform Support

Refer to Rust's platform support page

for more information on Rust's tiered platform support.

## Libraries

## Stabilized APIs

These previously stable APIs are now stable in const contexts:

## Cargo

## Rustdoc

## Compatibility Notes

- If fully elided, lifetime bounds of trait object types may now resolve differently or even get rejected in very specific niche scenarios
📝
- Error in more cases of ambiguous imports
📝
- Switch the destructors implementation for thread locals on Windows to use FLS
📝
- Convert some cases of the `ambiguous_glob_imports` lint into a hard error
📝
- Where-bounds of the form `Type = Type` and`Type == Type` are no longer syntactically allowed
📝
- Ensure Send/Sync is not implemented for std::env::Vars{,Os}
📝
- Fix that in some attributes, arguments were not properly rejected
📝
- `repr(transparent)` is now more strict about which fields have "trivial" layout and hence can be ignored:`repr(C)` types, types with private fields, and`#[non_exhaustive]` types are no longer considered "trivial"
📝
- Correctly check whether types have equal size in `transmute()` when some`repr` attributes are involved.
📝
- Add deny-by-default `invalid_runtime_symbol_definitions` lint and warn-by-default`suspicious_runtime_symbol_definitions` lint
  - The lints currently specifically targets `core` runtime symbols like`memcmp` ,`memset` ,`strlen` , ... and is planned be embellished in the next few releases.
📝
- The lints currently specifically targets 
- More characters are escaped when printing strings an chars
📝
- Implement fast path for `derive(PartialOrd)` when deriving`Ord`
 This can break crates in practice where a type's PartialOrd and Ord impls were inconsistent with each other.
📝
- Add temporary scope to `assert_eq` and`assert_ne`
📝
- lint on `core::ffi::c_void` as a return type
📝
- Closed a hole in the pattern matching structural equality check, preventing cases where a match of a constant would be allowed, despite disagreeing with a manually written `PartialEq` implementation, when a`derive(PartialEq)` implementation for that type also exists.
📝
- On Emscripten the WASM exception handling ABI is now unconditionally used The `-Zemscripten-wasm-eh=false` flag to switch back to JS exceptions has been removed.
📝
- The UNSAFE_CODE lint is now consistently emitted for all unsafe attributes
📝
- Solaris: remove `File::lock` implementation, it has the wrong semantics (return "unsupported" instead)
📝
- Windows-gnu targets now specify baseline tools versions
📝

## Internal Changes

These changes do not affect any public interfaces of Rust, but they represent

significant improvements to the performance or internals of rustc and related

tools.

## Other

## Activity

1. addedT-releaseRelevant to the release subteam, which will review and decide on the PR/issue.Relevant to the release subteam, which will review and decide on the PR/issue.relnotes-tracking-issueMarks issues tracking what text to put in release notes.Marks issues tracking what text to put in release notes.on Aug 7, 2026
2. addedneeds-triageThis issue may need triage. Remove when done. See docs forge.rust-lang.org/release/issue-triagingThis issue may need triage. Remove when done. See docs forge.rust-lang.org/release/issue-triagingon Aug 7, 2026
3. Hi relnotes-interest-group, this issue/PR could use some help in reviewing / 
 adjusting release notes. Could you take a look if available? Thanks <3cc @alex-semenyuk @jieyouxu @joshtriplett @lcnr @traviscross @weihanglo Reacted by Weihang Lo and alexey semenyuk
4. removedneeds-triageThis issue may need triage. Remove when done. See docs forge.rust-lang.org/release/issue-triagingThis issue may need triage. Remove when done. See docs forge.rust-lang.org/release/issue-triagingon Aug 16, 2026

## BoxyUwU commentedon Aug 7, 2026  

cc @rust-lang/release

@rustbot ping relnotes-interest-group

## rustbot commentedon Aug 7, 2026  

## joshtriplett commentedon Aug 10, 2026  

Looks great.

## wr7 commentedon Aug 12, 2026  

This is missing

`<[T]>::subslice_range`and`str::substr_range`
PR:#141266

Relnotes issue: #148225

## apiraino commentedon Aug 13, 2026  

mentioning #145108 as well, which caused some linting warnings (which in part have been fixing).

## jhpratt commentedon Aug 16, 2026  

## jieyouxu commentedon Aug 16, 2026  

## jhpratt commentedon Aug 16, 2026  

## cuviper commentedon Aug 17, 2026  

@apiraino that one is in the compat section as "Error in more cases of ambiguous imports", which can be adjusted in that relnote tracker if needed.

## BoxyUwU commentedon Aug 18, 2026  

#161293

to join this conversation on GitHub.Already have an account? Sign in to comment
