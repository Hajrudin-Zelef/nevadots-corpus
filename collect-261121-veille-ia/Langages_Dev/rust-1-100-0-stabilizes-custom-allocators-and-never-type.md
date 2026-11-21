---
id: collect-261121-veille-ia/langages-dev/rust-1-100-0-stabilizes-custom-allocators-and-never-type
title: "Rust 1.100.0 Stabilizes Custom Allocators and Never Type"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["type", "rust", "cargo", "never", "custom", "allocators", "stabilizes", "allocator"]
source: https://talaai.io/blog/rust-1-100-0-stabilizes-custom-allocators-and-never-type
source_anchor: ""
source_lines: [1, 53]
section: "Langages_Dev"
---
# Rust 1.100.0 Stabilizes Custom Allocators and Never Type

By TALA AI Editorial · · 3 min read

Rust 1.100.0 stabilizes the Allocator API for custom memory management, standardizes the never type (!), and introduces Cargo dependency cooldowns.

Rust 1.100.0 stabilizes custom allocators for Box and Vec, and stabilizes the never type (!), the canonical uninhabited type for diverging computations.

## Key takeaways

- std::alloc::Allocator, Global, and System are now stable, enabling custom allocators for Box and Vec.
- The never type (!) is stabilized, and Infallible is now a type alias to !.
- Cargo introduces a linting system with a built-in cargo::unused_dependencies check and minimum publish age configurations (dependency cooldowns).
- Box::new_in, Vec::new_in, Arc::map, and Rc::map are among the new stable APIs in version 1.100.0.
- The standard library now uses Unicode version 18.0.0 and the WASI SDK version was bumped from 33 to 34.
- Closures now more precisely capture variables used in if let, which may cause new compilation errors in existing code.

## Rust 1.100.0 Stabilized Custom Allocator API

The Rust release notes confirm the stabilization of `std::alloc::Allocator`, along with the `Global` and `System` allocators Rust 1.100.0. This change permits `Box` and `Vec` to use custom allocators through the newly stabilized `Allocator` trait. New stable methods include `Box::new_in`, `Box::from_raw_in`, and `Box::into_raw_with_allocator` for `Box` types, and `Vec::new_in`, `Vec::with_capacity_in`, and `Vec::from_raw_parts_in` for `Vec` types. Additionally, `Arc::map`, `Rc::map`, `Box::take`, and `Box::map` have been stabilized. The release also implements `Allocator` for `&A`, `&mut A`, `Box`, `Arc`, and `Rc`, allowing these types to function as allocators for other data structures.

## Never Type and Infallible Changes in Rust 1.100.0

The never type (`!`) has been stabilized in Rust 1.100.0, providing a standard type for code paths that do not return Rust 1.100.0. Concurrently, the `Infallible` type is now defined as a type alias to `!` rather than an independent type. This change is accompanied by the stabilization of `impl AsRef<T> for !` and `impl AsMut<T> for !`, allowing the never type to interact with generic code expecting `AsRef` or `AsMut` traits. Furthermore, the fallback behavior for the never type has been changed to `!` on all editions.

## Cargo Linting System and Dependency Cooldowns

Cargo 1.100.0 introduces a native linting system, allowing users to control specific Cargo warnings via the new `[lints.cargo]` configuration table Rust 1.100.0. The system includes the built-in `cargo::unused_dependencies` lint, which reports unused dependencies in `Cargo.toml`. The release also implements minimum publish age configurations, or "dependency cooldowns," based on RFC 3923. When this configuration is enabled, the Cargo resolver ignores dependency versions published more recently than the specified age, unless those versions are already present in the `Cargo.lock` file. Additionally, `cargo-install` now uses the existing lockfile found in `.crate` tarballs by default, rendering the `--locked` flag a no-op for this command. Cargo also uses a new directory layout for intermediate build artifacts; the layout is not a stable interface, and tools may need to use Cargo’s JSON messages to locate build artifacts.

## Rust 1.100.0 Language Feature Changes

- Lint against repeated `#[repr(..)]` attributes
- Stabilize `extern "custom"`
- `aligned_fields_in_packed` deny-by-default lint for`repr(align)` fields in`repr(packed)` types
- Volatile accesses to non-AM (atomic memory) can now trap
- `box` patterns are no longer syntactically allowed

## Rust 1.100.0 Compatibility and Breaking Changes

| Change | Detail | 
|---|---|
| Closure Capture | Closures now more precisely capture variables used in `if let` , which may cause new compilation errors Rust 1.100.0. | 
| Infallible Type | `Infallible` is now a type alias to the never type`!` . | 
| ARMv7-A Hardfloat | Targets previously erroneously requiring the `VFPv3-D32` FPU have been corrected; breakage can be fixed by passing`-Ctarget-feature=+d32` . | 
| SSE/AVX Error | Enabling SSE/AVX on x86 (32-bit or 64-bit) softfloat targets now emits an error. | 
| Unicode Version | The standard library now uses Unicode version 18.0.0. | 
| WASI SDK | The WASI SDK version was bumped from 33 to 34. | 

## Sources

- Rust 1.100.0 — GitHub
- Build Cache - The Cargo Book — doc.rust-lang.org
- char - Rust — doc.rust-lang.org
