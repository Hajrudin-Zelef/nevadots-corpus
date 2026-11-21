---
id: collect-261121-veille-ia/langages-dev/rust-1-99-0-c-variadic-functions-and-what-to-check-a-codercops
title: "Rust 1.99.0: C-Variadic Functions and What to Check â CODERCOPS"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-10-01"]
keywords: ["rust", "functions", "stable", "unsafe", "leak", "variadic", "code", "each"]
source: https://blog.codercops.com/blog/rust-1-99-0-c-variadic-functions-upgrade-guide-2026
source_anchor: ""
source_lines: [1, 100]
section: "Langages_Dev"
---
Sponsored

Rust 1.99.0 landed on October 1, 2026 (see the official announcement), and the answer for most teams is simple: run `rustup update stable`, let CI run, ship. The release notes describe no changes to language semantics. The interesting part is for people who live near the C boundary, because Rust can now *define* variadic functions instead of only calling them.

This is a short release with one real feature, one documentation warning that could bite old unsafe code, and a batch of small standard library additions. Here is what each one means in practice.

## C-variadic functions can now be written in Rust

For years Rust could call `printf` but could not be `printf`. If a C library wanted a callback with a variadic signature, you wrote a C shim. In 1.99.0, defining variadic functions with the `"C"` and `"C-unwind"` ABIs is stable.

```
/// SAFETY: must be called with (at least) 2 i32 arguments.
unsafe extern "C" fn sum(mut args: ...) -> i32 {
    // SAFETY: guaranteed by the caller.
    let a = unsafe { args.next_arg::<i32>() };
    let b = unsafe { args.next_arg::<i32>() };
    a + b
}
fn foo() -> i32 {
    unsafe { sum(0i32, 2i32) }
}
```
The `...` parameter has type `VaList`, which is ABI-compatible with Câs `va_list` on every target. A trait called `VaArgSafe` decides which types you may pull out of it. Naked variadic functions with non-âCâ ABIs are stable too, though those have to be written in inline assembly.

Notice the safety comment on the function. It is not decoration. Rust cannot know how many arguments the caller passed, so each `next_arg` is `unsafe` and reading a third `i32` from a two-argument call is undefined behavior. C has had this exact problem since the 1970s. Rust did not fix it, it just lets you write the function in a language with better tooling around it.

Who needs this? Plugin systems with a C ABI, logging callbacks passed to C libraries, and anyone replacing a C codebase one function at a time. If you write web backends, you will probably never touch it.

## Raw pointer layout queries for unsized types

For `Sized` types, asking a raw pointer for size and alignment was already safe on stable. For unsized values (slices, trait objects) the safety rules are now settled and three functions are stable:

- `Layout::for_value_raw`
- `mem::size_of_val_raw`
- `mem::align_of_val_raw`

These matter to people building custom smart pointers, arena allocators or intrusive collections, where you hold a raw pointer to a `dyn Trait` and need to free it correctly without first turning it back into a reference. It is niche, and for the people it applies to it removes an awkward workaround.

## The Box::leak warning is the one to read

Nothing changed in the compiler here, and that is what makes it easy to miss. But the docs for `Box::leak` now recommend against round-trip patterns where you leak a box and later deallocate the memory. The release notes say that code has caused problems with current and future compiler optimizations, and that the issue gets sharper as custom allocators come to stable. The same advice applies to the other `leak` functions in the standard library.

The fix is to use the pair designed for ownership handoff: `Box::into_raw` or the newly stable `Box::into_non_null`, then rebuild with `Box::from_raw` or `Box::from_non_null`.

```
let boxed = Box::new(String::from("shared with C"));
let ptr = Box::into_non_null(boxed);   // you own this pointer now
// ... hand `ptr` across an FFI boundary, get it back later ...
let boxed = unsafe { Box::from_non_null(ptr) };
drop(boxed); // freed through the original allocator
```
To find affected code, search your tree for `Box::leak` and read each call site. Ask one question at each: does anything later free this memory? If yes, switch it. If the leak is permanent (a `&'static` config built once at startup), you are fine.

## Smaller additions

None of these will change your architecture, but a few will delete helper code.

| Area | What is stable | 
|---|---|
| Collections | `IntoIterator` for`Box<[T; N]>` and references to it,`VecDeque::retain_back` ,`FusedIterator` for`StepBy<I>` | 
| Box and Vec | `Box::into_non_null` ,`Box::from_non_null` ,`Vec::into_parts` ,`Vec::from_parts` | 
| Strings | `String::from_utf8_lossy_owned` ,`FromUtf8Error::into_utf8_lossy` | 
| Filesystem | `std::fs::set_times` ,`std::fs::set_times_nofollow` | 
| FFI and layout | `core::ffi::VaList` and the raw layout functions above | 

`String::from_utf8_lossy_owned` is the one I expect to see in real code first. Converting bytes of unknown quality into a `String` without cloning is a daily chore in log processors and proxies, and until now it meant reaching for `String::from_utf8_lossy(...).into_owned()` and paying for a copy. `set_times` is welcome for build tools and archivers that need to restore file timestamps without a third-party crate.

## Should you upgrade?

For safe-Rust applications, yes, this week. The release notes list no language-semantics changes, so the realistic risk is a new clippy lint or a dependency that pinned an old toolchain. For crates with FFI or hand-written unsafe code, upgrade too, but read the `Box::leak` guidance first because that is where latent bugs hide. The full detail sits in the Rust release notes, including Cargo and Clippy changes that the announcement only links to.

Yes, for nearly everyone. A sensible order:

1. Bump `rust-toolchain.toml` to 1.99.0 on a branch.
2. Run the full test suite and `cargo clippy` .
3. Grep for `Box::leak` , and read every site where the memory is later freed.
4. If you ship C-facing code, check whether any C shim exists only because Rust could not define a variadic function.

One thing to keep in mind from the same week: Rust also announced that, starting with 1.100.0, 32-bit Windows hosts lose host tools. We cover the workaround in cross-compiling Rust for 32-bit Windows. And if your dependency tree concerns you more than your toolchain, our Cargo dependency audit guide is the better next read.

Teams that need unsafe Rust reviewed by someone who has shipped it can talk to CODERCOPS about engineering support. Everyone else can treat 1.99.0 as a routine bump with one grep to run.

## Frequently asked questions

- How do I upgrade to Rust 1.99.0?
- Run rustup update stable. If you pin a toolchain in rust-toolchain.toml, change the channel to 1.99.0 and let CI run before you merge. The release has no stated breaking changes, so most projects compile without edits.
- Can Rust define variadic functions now?
- Yes, with the C and C-unwind ABIs, as of 1.99.0. The function takes a parameter written as ..., whose type is VaList. You read arguments one at a time with next_arg, and every read is unsafe because the caller controls how many arguments exist.
- Is Box::leak deprecated in 1.99.0?
- No. The release makes no change to language semantics. The documentation now recommends against patterns that leak a box and later deallocate that memory, because they can break under current and future compiler optimizations. Prefer Box::into_raw or Box::into_non_null for that round trip.
- Should I upgrade a production service to 1.99.0 right away?
- For pure safe-Rust services, upgrading this week is low risk. Pin the toolchain, run your tests and clippy, and ship. If your code calls into C, uses custom allocators or round-trips leaked boxes, read the unsafe sections of the release notes first.

Sources

Sponsored

Sponsored

Sponsored
