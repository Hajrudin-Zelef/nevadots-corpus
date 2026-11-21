---
id: collect-261121-veille-ia/langages-dev/rust-1-99-c-variadic-functions-vec-into-parts-and-cargo-changes-full-stack-web-dev
title: "Rust 1.99: C-Variadic Functions, Vec::into_parts and Cargo Changes | Full-Stack Web Dev"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["rust", "double", "c-variadic", "parts", "cargo", "unsafe", "functions", "release"]
source: https://fullstackweb.dev/blog/rust-1-99-c-variadic-functions-debug-profile/
source_anchor: ""
source_lines: [1, 178]
section: "Langages_Dev"
---
Rust 1.99.0 shipped on 1 October 2026, and 1.99.1 followed on 15 October with three small fixes. Most Rust releases are a long list of small items, and this one is no different, but four of them matter to teams that run Rust in production: you can now *define* C-variadic functions in stable Rust, `Vec` can be split into raw parts and rebuilt without helper crates, CI builds stop using incremental compilation by default, and Cargo gains a `debug` profile that signals a change coming to `dev`.

This article covers those four changes, the compatibility notes worth reading before you bump a toolchain, and a short upgrade checklist. Release details come from the Rust projectâs `RELEASES.md`. I compiled and ran the code below with `rustc 1.99.1` (edition 2024) on Linux x86_64, and I say where I did not test something.

## What is in 1.99

Rust ships a new stable release every six weeks, so each one is deliberately modest. 1.99.0 is mostly lints, standard library additions and compiler plumbing. The items that change how you write or ship code are:

| Change | Area | Who it affects | 
|---|---|---|
| C-variadic function definitions stabilised | Language | Anyone exposing or implementing C ABI callbacks | 
| `Vec::into_parts` /`Vec::from_parts` stabilised | Standard library | FFI and low-level buffer code | 
| Incremental compilation off by default when `CI` is set | Cargo | Every team with Rust CI | 
| New built-in `debug` profile | Cargo | Teams that customise `[profile.dev]` | 

The same release also stabilises `VecDeque::retain_back`, `String::from_utf8_lossy_owned`, `std::fs::set_times`, `LocalKey<Cell<T>>::update` and `IntoIterator` for boxed arrays, among other things. See the release notes for the full list.

## C-variadic function definitions

Before 1.99, stable Rust could *call* C variadic functions such as `printf`, declared in an `extern` block, but it could not *define* one. If a C library took a callback with a `...` parameter, or you wanted to export a Rust function that C code calls like `printf`, you needed nightly or a C shim.

Stable Rust now accepts the definition. Here is a function that sums `count` doubles:

```
use std::ffi::{c_double, c_int};
/// Sums `count` doubles passed as C variadic arguments.
///
/// # Safety
/// The caller must pass exactly `count` arguments, each a `c_double`.
pub unsafe extern "C" fn sum_doubles(count: c_int, mut args: ...) -> c_double {
    let mut total = 0.0;
    for _ in 0..count {
        total += unsafe { args.next_arg::<c_double>() };
    }
    total
}
fn main() {
    let f: unsafe extern "C" fn(c_int, ...) -> c_double = sum_doubles;
    let total = unsafe { f(3, 1.5f64, 2.5f64, 3.0f64) };
    println!("{total}"); // 7
}
```
I compiled and ran this on 1.99.1, and it prints `7`. Points worth knowing:

- **The definition must be `unsafe extern "C"`** and the`...` parameter comes last, declared with a name (`mut args: ...` ). The value you read from is a`VaList` , and`core::ffi::VaList` is among the APIs stabilised in this release.
- **Reading an argument is `unsafe`.** Rust cannot check what the caller pushed. Reading a type the caller did not pass, or reading more arguments than were passed, is undefined behaviour, exactly as in C.
- **The method name is `next_arg` in my test.** The first version of my example used`args.arg::<c_double>()` , which does not exist on 1.99.1. If you find older nightly-era snippets online, expect the method names to differ, and check against the`VaList` documentation.
- **Pass floats as `f64`.** C promotes`float` to`double` in variadic calls, and rustc rejects an`f32` argument with error E0617 (I checked this on 1.99.1). In the call above,`1.5f64` matches what the function reads.

### When this matters

Most web and API teams will never write one. It matters when you are building a Rust library that a C, C++ or Python extension consumes, implementing logging or formatting hooks that a C host calls with printf-style arguments, or replacing a C shim that existed only because stable Rust could not define the function. For everything else, keep using ordinary Rust APIs and leave the `...` to the boundary.

## Vec::into_parts and Vec::from_parts

Handing a `Vec` buffer to foreign code used to mean a small dance with `ManuallyDrop`, `as_mut_ptr`, `len` and `capacity`, followed by `Vec::from_raw_parts`. Rust 1.99 stabilises `Vec::into_parts` and `Vec::from_parts`, which do the split and the rebuild in one call each:

```
fn main() {
    let mut v: Vec<u32> = Vec::with_capacity(8);
    v.extend([1, 2, 3]);
    // Consumes the Vec without dropping the allocation.
    let (ptr, len, cap) = v.into_parts();
    println!("{len} {cap}"); // 3 8
    // ... hand ptr, len and cap to foreign code, then take them back ...
    // SAFETY: the three values came from into_parts and were not changed.
    let v = unsafe { Vec::from_parts(ptr, len, cap) };
    println!("{v:?}"); // [1, 2, 3]
}
```
I ran this on 1.99.1 and it prints `3 8` and `[1, 2, 3]`. On 1.97.0, the same call fails to compile with `use of unstable library feature box_vec_non_null`, which confirms the API is new to stable in this release. The release notes also stabilise `Box::into_non_null` and `Box::from_non_null`, and the `ptr` returned by `into_parts` is a `NonNull<T>` rather than a raw pointer, so you do not need a null check on the way back in.

`from_parts` is still `unsafe`. The pointer, length and capacity must describe an allocation that came from the same allocator with the same element type, and you must not rebuild it twice. The gain is clarity: the pairing is visible in one place, instead of being spread across three method calls and a `ManuallyDrop`.

## Cargo: CI builds and the debug profile

### Incremental compilation is off by default in CI

Per the compatibility notes, Cargo now disables incremental compilation by default when the `CI` environment variable is set. Incremental compilation helps local rebuilds, but in a clean CI checkout it mostly costs disk and time, because there is nothing to reuse and the incremental cache is rarely restored.

I checked this by building a new project with `cargo build -v`. Without `CI` set, the verbose `rustc` invocation included an `incremental=` argument. With `CI=true`, it did not.

What to do about it:

- **Usually, nothing.** Most CI jobs build from scratch, so you may see slightly faster or smaller builds.
- **If you cache `target/` between CI runs** and relied on incremental artefacts, you may see longer builds. Set`CARGO_INCREMENTAL=1` for that job, or`[profile.dev] incremental = true` in the config, and measure before deciding.
- **If your local scripts set `CI=true`** for other tools, your local builds change too.

### The new debug profile

Cargo has a new built-in profile named `debug`. According to the notes, it prepares for a future change that moves the `dev` profile away from debugging, so that `dev` becomes a faster default for everyday iteration. Today there is no difference between `dev` and `debug`.

I confirmed that `cargo build --profile debug` works on 1.99.1 and that a `[profile.debug]` table with `inherits = "dev"` is accepted. The practical advice is to watch your own `Cargo.toml`:

- If you tune `[profile.dev]` for debugging, such as`opt-level = 0` or full debug info, think about moving those settings to`[profile.debug]` now so they survive the transition.
- If you tune `[profile.dev]` for speed, you may not need to change anything.
- I have not seen the final shape of the future `dev` profile, so do not rewrite build scripts around guesses. Read the Cargo profiles reference when the change lands.

## Compatibility notes to read first

The 1.99.0 compatibility section is long. These are the entries most likely to affect an application team:

- **Legacy integer modules are fully deprecated.**`std::i32::MAX` should be`i32::MAX` . The fix is mechanical.
- **`no_mangle_generic_items` is now a hard error.** If you put`#[no_mangle]` on a generic item, the build fails.
- **`Pin::new_unchecked` has slightly different safety invariants.** Audit any`unsafe` code that creates pinned values by hand.
- **References to extern statics are no longer promoted** , and**inferred types of `let` patterns must typecheck** . Both can surface as new compile errors in unusual code.
- **Attributes that apply to nothing are an error in doc-comment code blocks.** If doctests start failing after an upgrade, look here first.
- **`semicolon_in_expressions_from_macros` now fires for non-local macros.** Report the warning to the crate that provides the macro rather than silencing it.
- **Update to LLVM 23** is an internal change. It rarely matters, but it is the layer where codegen differences come from. Release 1.99.1 fixes an FFI`bool` return miscompilation on x86_64, so use 1.99.1 rather than 1.99.0 if you cross the FFI boundary.

## Upgrade checklist

1. Run `rustup update stable` locally and in CI, and pin the version in`rust-toolchain.toml` so every machine agrees.
2. Build with warnings visible: `cargo build --all-targets` and`cargo test` . Collect new lint output before deciding what to fix.
3. Search for `std::i32::` ,`std::u64::` and similar module paths and replace them with the associated constants.
4. Grep for `no_mangle` on generic items, and for`Pin::new_unchecked` .
5. Compare CI timings for one week, and re-enable incremental compilation for a job only if the numbers justify it.
6. Move debug-oriented settings from `[profile.dev]` to`[profile.debug]` if you want to be ready for the`dev` change.
7. Update `rust-version` in`Cargo.toml` only if you start using a 1.99 API in a published library.

## Advantages and limitations

**Advantages**

- Stable C-variadic definitions remove a reason to keep C shims or nightly toolchains.
- `into_parts` and`from_parts` make buffer handoff shorter and easier to audit.
- CI defaults are closer to what most pipelines want.

**Limitations**

- C-variadic code is `unsafe` and as easy to get wrong as in C. The compiler cannot check the argument types.
- Both `Vec` APIs are for FFI and low-level code. Ordinary application code does not need them.
- The `debug` profile does nothing yet, so it is a heads-up rather than a feature.

## Common mistakes

- **Reading the wrong type from a `VaList`.** Reading an`i32` when the caller passed an`i64` is undefined behaviour.
- **Passing `f32` to a variadic function.** The compiler rejects it; cast to`f64` .
- **Calling `Vec::from_parts` with changed values.** Do not change the capacity or pointer between the split and the rebuild.
- **Assuming CI behaviour matches local builds.** Setting`CI=true` locally changes Cargoâs incremental default.
- **Staying on 1.99.0.** The 1.99.1 point release fixes an FFI miscompilation.

## Conclusion

Rust 1.99 is not a headline release, and that is fine. The useful changes are at the edges: a cleaner path for FFI code, a saner CI default, and an early notice about the `dev` profile. For most backend teams the upgrade is `rustup update`, a build with warnings visible, and a look at the compatibility list. If you cross the C ABI, take 1.99.1.

## Frequently asked questions

### Can stable Rust define C variadic functions?

Yes, from Rust 1.99. The function must be `unsafe extern "C"` and take a trailing `...` parameter. Calling variadic functions declared in `extern` blocks was already possible.

### What does Vec::into_parts return?

A pointer, the length and the capacity, with the pointer as a `NonNull`. `Vec::from_parts` rebuilds the vector from the same three values and is `unsafe`.

### Does Cargo disable incremental compilation in CI?

Starting with Rust 1.99, yes by default, when the `CI` environment variable is set. You can turn it back on with `CARGO_INCREMENTAL=1`.

### What is the Cargo debug profile?

A new built-in profile that currently behaves the same as `dev`. It prepares for a change that makes `dev` a faster default for development iteration.

### Which Rust version is current?

As of 8 November 2026, the newest stable release I could confirm in the release notes is 1.99.1, published on 15 October 2026. A newer release may have landed since.

## References and further reading

- Rust `RELEASES.md` , the source for the 1.99.0 and 1.99.1 notes.
- Stabilize C-variadic function definitions (rust-lang/rust#155697).
- Cargo: add a built-in `debug` profile (rust-lang/cargo#17214).
- Cargo: disable incremental compilation in CI by default (rust-lang/cargo#17220).
- Cargo profiles reference.
- Rust blog, for the announcement posts.

If youâre deciding where Rust fits next to Go or Node.js in a backend, see my backend and API development work, or tell me about your project. For a comparable language release, read Go 1.26: new(expr), go fix Modernizers and Goroutine Leak Profiles. For CI and delivery work, see cloud and DevOps.
