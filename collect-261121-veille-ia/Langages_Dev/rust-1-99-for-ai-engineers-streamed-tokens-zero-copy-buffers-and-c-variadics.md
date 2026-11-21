---
id: collect-261121-veille-ia/langages-dev/rust-1-99-for-ai-engineers-streamed-tokens-zero-copy-buffers-and-c-variadics
title: "Rust 1.99 for AI Engineers: Streamed Tokens, Zero-Copy Buffers and C Variadics"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-10-01"]
keywords: ["rust", "string", "self", "tokens", "pending", "streamed", "utf8", "bytes"]
source: https://medium.com/@abhinav.dobhal/rust-1-99-for-ai-engineers-streamed-tokens-zero-copy-buffers-and-c-variadics-796a8026a4a5
source_anchor: ""
source_lines: [1, 400]
section: "Langages_Dev"
---
# Rust 1.99 for AI Engineers: Streamed Tokens, Zero-Copy Buffers and C Variadics

The release notes are short. Read them with LLM code in mind and four changes stand out. I ran each one, including from C.

Rust 1.99.0 shipped on October 1, 2026, and 1.99.1 followed on October 15. The announcement is short and says there are no changes to language semantics, which makes it easy to skim past. If you write code around LLMs, don’t. Four of the changes land exactly where AI code tends to hurt: decoding streamed tokens, handing tensor and embedding buffers across a C boundary, accepting callbacks from C libraries, and a codegen bug in FFI that 1.99.1 fixes.

I ran every example below on Rust 1.99.0 (Homebrew build, aarch64 macOS) and called the FFI parts from real C compiled with Apple clang 21 under AddressSanitizer. The output is copied from the terminal.

## What Rust 1.99 actually changed

From the Rust 1.99.0 announcement:

- **C variadic functions written in Rust.** You can now define`unsafe extern "C" fn f(args: ...)` , not just call ones like`printf` . The`...` has type`VaList` .
- **Layout of raw pointers to unsized types:**`Layout::for_value_raw` ,`mem::size_of_val_raw` ,`mem::align_of_val_raw` .
- **New stable APIs** , including`Vec::into_parts` /`Vec::from_parts` ,`Box::into_non_null` /`Box::from_non_null` ,`String::from_utf8_lossy_owned` ,`FromUtf8Error::into_utf8_lossy` ,`VecDeque::retain_back` ,`std::fs::set_times` and`set_times_nofollow` , and`IntoIterator` for boxed arrays.
- **A documentation change with teeth:**`Box::leak` docs now recommend against leaking memory and freeing it later, because that pattern causes problems with current and future optimizations and with the upcoming custom allocator support. Use`Box::into_raw` or`Box::into_non_null` instead.

Then Rust 1.99.1 fixed three regressions, one of which is an FFI miscompile. More on that below.

## 1. Decoding streamed LLM tokens without broken characters

Byte-level BPE tokenizers work on bytes, not characters, so a single emoji or CJK character can be split across two tokens. If you decode each streamed chunk on its own, you print replacement characters where the user should see text.

Here is that bug, and a decoder that fixes it. The new API is `String::from_utf8_lossy_owned`, used once at the end of the stream:

```
//! Byte-level BPE tokenizers can split one UTF-8 character across two tokens.
//! This shows what goes wrong when you decode each chunk on its own, and a fix.
```
/// Decodes a stream of byte chunks, holding back an incomplete character until the next chunk.

#[derive(Default)]

struct Utf8Stream {

pending: Vec<u8>,

}impl Utf8Stream {

fn push(&mut self, chunk: &[u8]) -> String {

self.pending.extend_from_slice(chunk);

let mut out = String::new();

loop {

match std::str::from_utf8(&self.pending) {

Ok(s) => {

out.push_str(s);

self.pending.clear();

return out;

}

Err(e) => {

let good = e.valid_up_to();

out.push_str(std::str::from_utf8(&self.pending[..good]).unwrap());

match e.error_len() {

// Incomplete character at the end: keep it for the next chunk.

None => {

self.pending.drain(..good);

return out;

}

// Genuinely invalid bytes: replace them and keep going.

Some(bad) => {

out.push('\u{FFFD}');

self.pending.drain(..good + bad);

}

}

}

}

}

} /// End of stream: whatever is left can never be completed.

fn finish(self) -> String {

String::from_utf8_lossy_owned(self.pending) // 1.99

}

}fn main() {

let text = "Rust 🦀 ships";

let bytes = text.as_bytes();

// Split inside the 4-byte crab emoji, the way a byte-level tokenizer can.

let chunks = [&bytes[..6], &bytes[6..8], &bytes[8..]];

println!("chunks: {:?}", chunks.iter().map(|c| c.len()).collect::<Vec<_>>()); let naive: String = chunks.iter().map(|c| String::from_utf8_lossy(c).into_owned()).collect();

println!("decode each chunk : {naive:?}"); let mut s = Utf8Stream::default();

let mut fixed = String::new();

for c in chunks {

let piece = s.push(c);

println!(" chunk -> {piece:?}");

fixed.push_str(&piece);

}

fixed.push_str(&s.finish());

println!("buffered decode : {fixed:?}"); // A stream that ends mid-character: finish() replaces the leftover once.

let mut s = Utf8Stream::default();

let mut cut = s.push(&bytes[..7]);

cut.push_str(&s.finish());

println!("cut mid-emoji : {cut:?}"); // from_utf8_lossy_owned reuses the Vec's allocation when the bytes are already valid.

let v = b"already valid".to_vec();

let before = v.as_ptr();

let s = String::from_utf8_lossy_owned(v);

println!("valid input reused allocation: {}", s.as_ptr() == before); // FromUtf8Error::into_utf8_lossy recovers from a failed strict decode without re-reading.

let err = String::from_utf8(vec![b'o', b'k', 0xff]).unwrap_err();

println!("into_utf8_lossy : {:?}", err.into_utf8_lossy());

}

Output:

```
chunks: [6, 2, 7]
decode each chunk : "Rust ���� ships"
  chunk -> "Rust "
  chunk -> ""
  chunk -> "🦀 ships"
buffered decode   : "Rust 🦀 ships"
cut mid-emoji     : "Rust �"
valid input reused allocation: true
into_utf8_lossy   : "ok�"
```
One crab became four replacement characters with the naive approach: one for the dangling lead byte, two for the orphaned continuation bytes in the middle chunk, and one more at the start of the last chunk. The buffered decoder emits nothing for the middle chunk and the whole emoji with the next one.

Why the new API matters here: `String::from_utf8_lossy(&v)` borrows and returns a `Cow`, so getting an owned `String` out of a `Vec<u8>` you already own meant an extra copy in the common valid case. `from_utf8_lossy_owned` takes the `Vec` by value. When the bytes are valid it hands the same allocation back as a `String`, which the pointer check above confirms, and it only allocates when it has to insert replacement characters.

## 2. Handing embedding buffers to C without copying

Inference engines, vector databases and GPU runtimes are full of C and C++ APIs. Since Rust 1.93 you could split a `Vec` with `Vec::into_raw_parts`, which hands back a raw `*mut T`, and rebuild it with `Vec::from_raw_parts`. Rust 1.99 adds `Vec::into_parts` and `Vec::from_parts`, the same operation typed with `NonNull<T>`, so "this pointer is never null" is in the signature instead of a comment. It also adds `Box::into_non_null` / `Box::from_non_null` for opaque handles.

Here is a small library with a model handle C can’t see inside, an embedding buffer C reads in place, and matching free functions:

```
use std::ffi::{c_double, c_int};
use std::ptr::NonNull;
```
/// An opaque "model" handle that C code holds but never looks inside.

pub struct Model {

dim: usize,

}/// Hand C an owning pointer. 1.99's docs steer you to `Box::into_non_null` instead of

/// `Box::leak` when the memory will be freed later.

#[unsafe(no_mangle)]

pub extern "C" fn rs_model_new(dim: usize) -> *mut Model {

Box::into_non_null(Box::new(Model { dim })).as_ptr()

}/// # Safety

/// `m` must come from `rs_model_new` and must not be used afterwards.

#[unsafe(no_mangle)]

pub unsafe extern "C" fn rs_model_free(m: *mut Model) {

if let Some(m) = NonNull::new(m) {

drop(unsafe { Box::from_non_null(m) });

}

}/// A buffer C can read without copying: pointer, length and the capacity Rust needs back.

#[repr(C)]

pub struct Embedding {

ptr: *mut f32,

len: usize,

cap: usize,

}/// # Safety

/// `m` must be a live handle from `rs_model_new`.

#[unsafe(no_mangle)]

pub unsafe extern "C" fn rs_embed(m: *const Model, token_id: u32) -> Embedding {

let dim = unsafe { (*m).dim };

let v: Vec<f32> = (0..dim).map(|i| (token_id as f32 + i as f32) / 10.0).collect();

let (ptr, len, cap) = v.into_parts(); // 1.99: NonNull<T> instead of *mut T

Embedding { ptr: ptr.as_ptr(), len, cap }

}/// # Safety

/// `e` must come from `rs_embed` and must be freed exactly once.

#[unsafe(no_mangle)]

pub unsafe extern "C" fn rs_embedding_free(e: Embedding) {

if let Some(ptr) = NonNull::new(e.ptr) {

drop(unsafe { Vec::from_parts(ptr, e.len, e.cap) });

}

}

The capacity travels with the buffer on purpose. `Vec::from_parts` needs the original capacity to free the allocation correctly, and C has no reason to know it unless you pass it along.

The `Box::leak` note in the release is aimed at exactly this kind of code. Leaking a box to get a `&'static mut` and later rebuilding it to free it is the pattern the docs now warn against. If the memory is coming back, `into_non_null` says so in the type.

## 3. Writing C variadic callbacks in Rust

Plenty of C libraries take printf-style callbacks or expose variadic entry points. Until 1.99 you could call those from Rust but not implement one. Now you can:

```
/// A printf-style C variadic written in Rust (stable in 1.99).
/// C promotes `float` arguments to `double`, so they must be read as `c_double`.
///
/// # Safety
/// Must be called with `count` further arguments, each a C `double`.
#[unsafe(no_mangle)]
pub unsafe extern "C" fn rs_mean(count: c_int, mut args: ...) -> c_double {
    let mut sum = 0.0;
    for _ in 0..count {
        sum += unsafe { args.next_arg::<c_double>() };
    }
    if count > 0 { sum / count as c_double } else { 0.0 }
}
```
I built the crate as a static library and called all of it from C:

```
#include <stdio.h>
#include <stddef.h>
```
typedef struct Model Model;

typedef struct { float *ptr; size_t len; size_t cap; } Embedding;Model *rs_model_new(size_t dim);

void rs_model_free(Model *m);

Embedding rs_embed(const Model *m, unsigned token_id);

void rs_embedding_free(Embedding e);

double rs_mean(int count, ...);int main(void) {

Model *m = rs_model_new(4);

Embedding e = rs_embed(m, 7);

printf("embedding len=%zu cap=%zu: %.1f %.1f %.1f %.1f\n", e.len, e.cap, e.ptr[0], e.ptr[1], e.ptr[2], e.ptr[3]);

float a = 0.25f, b = 0.5f, c = 0.75f; /* floats: promoted to double by C */

printf("rs_mean(3, 0.25f, 0.5f, 0.75f) = %.4f\n", rs_mean(3, a, b, c));

printf("rs_mean(2, 1.0, 4.0) = %.4f\n", rs_mean(2, 1.0, 4.0));

rs_embedding_free(e);

rs_model_free(m);

puts("freed embedding and model");

return 0;

}

Built with `cargo build --release` and `cc -fsanitize=address -g c/main.c target/release/librust199.a`:

```
embedding len=4 cap=4: 0.7 0.8 0.9 1.0
rs_mean(3, 0.25f, 0.5f, 0.75f) = 0.5000
rs_mean(2, 1.0, 4.0) = 2.5000
freed embedding and model
```
AddressSanitizer reported nothing, so the buffer and the handle were each freed once, by the allocator that created them.

### The float trap, caught at compile time

Look at the C call again: it passes `float` values, and the Rust side reads `c_double`. That is not a typo. C promotes `float` to `double` (and small integers to `int`) when they go through `...`. Reading an `f32` would read the wrong bytes. Rust 1.99 refuses to compile it:

```
error[E0277]: the trait bound `f32: VaArgSafe` is not satisfied
   --> bad.rs:4:41
    |
  4 |         sum += unsafe { args.next_arg::<f32>() };
    |                              --------   ^^^ the nightly-only, unstable trait `VaArgSafe` is not implemented for `f32`
    |                              |
    |                              required by a bound introduced by this call
    |
    = help: the following other types implement trait `VaArgSafe`:
              f64
              i128
              i32
              i64
              isize
```
The `VaArgSafe` trait itself is still unstable, but `next_arg` is bounded by it, so the promotion rules are enforced on stable. The standard library docs also say not to rely on impls like `i32` directly and to use `c_int`, `c_long`, `c_double` and friends, because the set of valid types differs by platform.

## 4. Rust 1.99.1 fixes an FFI bool miscompile: upgrade

Rust 1.99.1 fixes three regressions. The one AI and FFI code should care about is rust-lang/rust#163911: on x86_64, a Rust function returning `bool` across FFI could leave bits 1 to 7 of the return register unspecified, so a C caller could read a value other than 0 or 1. The fix zero-extends the return value again, as the ABI requires. Think of every binding that exposes `is_ready()`, `has_gpu()` or `should_stop()` to C, C++ or Python through a C layer.

The other two fixes are a missing `wcslen` symbol on some targets (UEFI hit hardest) and `rust-lld` failing to find `libLLVM` on macOS when the compiler runs without the rustup proxy (Rust 1.99.1 announcement).

The fix is `rustup update stable`. One catch I hit while writing this: my Homebrew-installed toolchain was still 1.99.0 even though Homebrew's formula already offers 1.99.1, because nothing had run `brew upgrade rust`. If your CI or your laptop installs Rust from a package manager rather than rustup, check `rustc --version` rather than assuming.

## Smaller changes worth knowing

**VecDeque::retain_back(n)**
`n` elements and drops the rest, which is exactly a chat-history window. Pushing seven turns with`retain_back(4)` after each one left`["a2", "u3", "a3", "u4"]` .
**std::fs::set_times**
**IntoIterator**
 **for** **Box<[T; N]>**

## What I did not test

- **The 1.99.1 bool fix.** I ran 1.99.0 on aarch64, and the bug is specific to x86_64. I am reporting it from the announcement and the issue, not reproducing it.
- **Windows and Linux.** Everything ran on macOS. The variadic ABI and`VaArgSafe` impls are target-specific by design.
- **Performance.** The zero-copy claims are about allocations (the pointer check shows reuse), not a benchmark.

## Takeaways

1. Buffer streamed token bytes and decode the valid prefix; use `String::from_utf8_lossy_owned` to flush at the end without an extra copy.
2. Pass buffers to C with `Vec::into_parts` and take them back with`Vec::from_parts` , carrying the capacity with the pointer.
3. Use `Box::into_non_null` instead of`Box::leak` for handles you will free later.
4. Read C variadic arguments as `c_int` and`c_double` ; the compiler now enforces it.
5. Upgrade to 1.99.1 if you have FFI code on x86_64, and check what your package manager actually installed.

This is part of my Rust for AI series. Part 1 covered making forbidden agent tools unrepresentable. Which of these changes will you use first?

## FAQ

### What is new in Rust 1.99?

Rust 1.99.0 (October 1, 2026) stabilizes defining C variadic functions in Rust, layout functions for raw pointers to unsized types, and APIs including `Vec::into_parts` and `Vec::from_parts`, `Box::into_non_null` and `Box::from_non_null`, `String::from_utf8_lossy_owned`, `VecDeque::retain_back` and `std::fs::set_times`.

### Should I upgrade to Rust 1.99.1?

Yes, especially if you have FFI code. 1.99.1 (October 15, 2026) fixes a miscompilation on x86_64 where a Rust function returning `bool` across FFI could leave bits 1 to 7 of the return register unspecified, so C callers could see values other than 0 or 1.

### How do I decode streamed LLM tokens in Rust without broken characters?

Buffer the bytes. Decode the valid prefix with `std::str::from_utf8` and `Utf8Error::valid_up_to`, keep an incomplete trailing character for the next chunk, and at the end of the stream flush what is left with `String::from_utf8_lossy_owned`.

### Why can’t I read an f32 from a C variadic in Rust?

C promotes `float` arguments to `double` when they are passed through `...`, so the value on the stack is a double. Rust encodes this rule in the `VaArgSafe` trait: `f32` does not implement it on most targets, and the compiler rejects `next_arg::<f32>()`. Read `c_double` instead.
