---
id: collect-261121-veille-ia/langages-dev/rust-1-99-lets-you-write-printf-in-rust-alb-wiki
title: "Rust 1.99 Lets You Write printf in Rust — ALB Wiki"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["rust", "layout", "write", "printf", "microsoft", "lets", "variadic", "size"]
source: https://www.alb-lang.org/wiki/Rust_1.99_Variadics
source_anchor: ""
source_lines: [1, 118]
section: "Langages_Dev"
---
# Rust 1.99 Lets You Write printf in Rust

**Rust 1.99.0**, released 1 October 2026, stabilizes the ability to **define** C-ABI variadic functions directly in Rust — the same calling convention behind `printf`, `scanf`, and thousands of C APIs. Until now Rust could only *call* foreign variadics. Now you can *write* them.

The release lands weeks after Microsoft's Azure CTO Mark Russinovich told RustConf 2025 that Rust is shipping inside Windows, Azure, and the company's cryptographic stack — and that Microsoft is building AI tools to accelerate migrating C++ codebases to Rust at scale.

## What changed in 1.99

### extern "C" variadics

The headline feature. A Rust function can now accept a C-compatible variable argument list:

```
/// SAFETY: must be called with (at least) 2 i32 arguments.
unsafe extern "C" fn sum(mut args: ...) -> i32 {
    let a = unsafe { args.next_arg::<i32>() };
    let b = unsafe { args.next_arg::<i32>() };
    a + b
}
```
The type of `...` is `core::ffi::VaList`, ABI-compatible with C's `va_list` across all targets. Which types can be read from a `VaList` is guarded by the `VaArgSafe` trait — a compile-time safety net that C never had.

This matters for anyone writing Rust replacements for C libraries that expose variadic interfaces. Before 1.99, you had to keep a C shim or use `extern` blocks. Now the entire function can live in Rust.

Naked variadic functions with non-`"C"` ABIs are also stabilized in this release, for low-level use cases requiring inline assembly.

### Raw-pointer layout helpers

Three APIs for querying layout information from raw pointers are now stable:

- `Layout::for_value_raw` — returns the`Layout` of the value behind a raw pointer
- `mem::size_of_val_raw` — size from a`*const T`
- `mem::align_of_val_raw` — alignment from a`*const T`

These settle long-standing safety requirements for `Sized` and `!Sized` types and give allocator authors the tools they need without going through references.

### Box::leak guidance change

No behavioral change — but the documentation now explicitly recommends against "round-trip unleaking" (calling `Box::leak` and later freeing the returned reference). This pattern makes assumptions about the allocator that may not hold with upcoming custom allocator support. Use `Box::into_non_null` or `Box::into_raw` instead.

### All 17 stabilized APIs

| API | What it does | 
|---|---|
| `IntoIterator for Box<[T; N]>` | Iterate owned boxed arrays (plus `&` and`&mut` variants) | 
| `VecDeque::retain_back` | Retain from the back of a deque | 
| `core::ffi::VaList` | The variadic argument list type | 
| `Box::into_non_null` /`Box::from_non_null` | Non-null pointer conversion for boxes | 
| `Vec::into_parts` /`Vec::from_parts` | Decompose/recompose a Vec's raw parts | 
| `size_of_val_raw` /`align_of_val_raw` | Size and alignment from raw pointers | 
| `Layout::for_value_raw` | Layout from a raw pointer | 
| `String::from_utf8_lossy_owned` | Lossy conversion that takes ownership | 
| `FromUtf8Error::into_utf8_lossy` | Recover from a failed UTF-8 conversion | 
| `FusedIterator for StepBy` | StepBy now implements FusedIterator | 
| `std::fs::set_times` /`set_times_nofollow` | Set file timestamps without following symlinks | 

Update with:

`rustup update stable`
### Download Rust 1.99.0

Official installers, mirrored on alb-lang.org:

- rust-1.99.0-x86_64-pc-windows-gnu.msi — Windows 64-bit (363 MB)
- rust-1.99.0-i686-pc-windows-gnu.msi — Windows 32-bit (358 MB)
- rust-1.99.0-x86_64-unknown-linux-gnu.tar.xz — Linux 64-bit (201 MB)

Verify your download against SHA256SUMS.

## How Microsoft went all-in on Rust

The story starts with a statistic: **70% of Microsoft's security patches** over the prior twelve years were fixes for memory-safety bugs — buffer overflows, use-after-free, dangling pointers — all rooted in C and C++.

Microsoft had known this for years. Internal research kept surfacing the same number. The question was what to do about it at the scale of Windows.

### The tweet that started a war

On **19 September 2022**, Mark Russinovich — CTO of Microsoft Azure, author of the Sysinternals tools, and a legend in Windows internals — posted on Twitter:

The reaction was immediate and loud. C and C++ developers pushed back. Rust advocates celebrated. Russinovich clarified: he was not suggesting anyone rewrite existing codebases overnight. He himself had 85,000 lines of Sysinternals C/C++ he maintained. But new tools? He would bias toward Rust.

### The receipts

Microsoft didn't just talk. Here's what shipped:

- **February 2021** — Microsoft joined the Rust Foundation as a**founding Platinum member** and formed an in-house Rust team to contribute compiler and tooling work.
- **January 2020 onward** —`windows-rs` shipped: official Rust bindings for the entire Win32/WinRT API surface.
- **May 2023** —**Rust code arrived in the Windows 11 kernel** . The`win32kbase_rs.sys` driver, a Rust port of the Win32k GDI subsystem, appeared in Insider Build 25905. No performance regressions. All tests passed.
- **2023–2026** —`windows-drivers-rs` matured: a framework for writing WDM/KMDF/UMDF drivers in Rust. The Surface team shipped real drivers built on it.
- **2025** — Microsoft shipped`edit` , a terminal text editor written in Rust, under 250 KB. And`sudo` for Windows — also Rust.
- **RustConf 2025** — Russinovich's keynote revealed that**Hyperlight** , Azure's micro-VM sandbox, was rewritten from C# to Rust for performance and is now handling real traffic in Azure Front Door. He also demonstrated a Cargo plugin for MSBuild that drops Rust modules into C++ and C# codebases, and a transpiler that converts verified C crypto libraries to verified Rust automatically.
- **Azure Boost** — the hardware-offload layer under Azure's VMs — made Rust the**primary language for all new code** .

### What people are thinking

The conversation has shifted. In 2022, Russinovich's tweet was controversial. By 2026, the question isn't *whether* to use Rust for systems programming — it's *how fast* to adopt it.

The **White House** and **CISA** have issued guidance recommending memory-safe languages for critical infrastructure. The Linux kernel has accepted Rust as a second implementation language. Google's Android team reports that memory-safety bugs in new code dropped sharply after introducing Rust.

None of this means C and C++ disappear. Billions of lines of working C and C++ will be maintained for decades. But the direction is clear: **new systems code is increasingly written in Rust**, and Rust 1.99 — with its C-variadic bridge — makes the boundary between the two worlds thinner than ever.

### See also

- Rust — the programming language
- Memory safety — why it matters
- C Programming Language — the language Rust aims to complement
- History of Programming — the big picture

### References

### Footnotes

1. ↑ Rust Blog, "Announcing Rust 1.99.0," 1 October 2026. https://blog.rust-lang.org/2026/10/01/Rust-1.99.0/
2. ↑ Mark Russinovich (@markrussinovich), Twitter, 19 September 2022. Archived by The Register: https://www.theregister.com/2022/09/20/in_rust_we_trust_microsoft_azure_cto_shuns_c_and_c/
3. ↑ Neowin, "Microsoft delivers on its promise, Rust has arrived in the Windows 11 kernel," 11 May 2023.
4. ↑ The New Stack, "Microsoft Goes All-in on Rust for Core Infrastructure and Much More," 19 September 2025.
5. ↑ Microsoft Tech Community, "Towards Rust in Windows Drivers," 2024.
6. ↑ LWN.net, "Rust 1.99.0 released," 1 October 2026.
