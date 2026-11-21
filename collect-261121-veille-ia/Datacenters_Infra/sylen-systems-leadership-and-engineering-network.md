---
id: collect-261121-veille-ia/datacenters-infra/sylen-systems-leadership-and-engineering-network
title: "SYLEN | Systems Leadership and Engineering Network"
domain: datacenters-infra
role: reference
task: article
actors: []
dates: []
keywords: ["type", "standard", "dropguard", "rust", "core", "allocator", "cargo", "types"]
source: https://www.sylen.org/news?article=fa94d3d3-ae0c-4c11-8e98-ee7005fdcc26
source_anchor: ""
source_lines: [1, 38]
section: "Datacenters_Infra"
---
# Rust 1.100 Stabilizes the Never Type, Core Allocator APIs, and Transactional Drop Guards

Rust 1.100 marks the stabilization of the canonical uninhabited never type (`!`), `Infallible` aliasing, and the foundational core of the `Allocator` trait for core collections. The release also introduces standard transactional rollback tooling via `DropGuard`, explicit ZST construction primitives, and structural Cargo cache refactoring.

## Uninhabited Types and the Never Type

After ten years and five stabilization attempts, Rust 1.100 officially stabilizes the never type (`!`). Serving as the canonical uninhabited type, `!` represents diverging control flow operations such as `return`, `break`, and `std::process::exit`. Because an instance of `!` cannot exist, the language permits never-to-any coercion, allowing expressions of type `!` to satisfy any type signature during type checking.

Developers can now explicitly name `!` in any type signature, including generic parameter positions like `Result

## Custom Memory Allocation Interfaces

Rust 1.100 stabilizes the core interfaces of the long-awaited `allocator_api`, formalizing the `Allocator` trait. This release allows library authors to pass custom memory allocators directly to standard library heap allocations, specifically supporting `Vec` and `Box`.

While the current stabilization focuses strictly on the foundational trait and a subset of standard library types, it establishes the standard memory allocation abstraction for the ecosystem. Future releases will extend allocator parameters across additional collection types, smart pointers, and specialized allocation interfaces.

## Deterministic Cleanup with DropGuard

The addition of `core::mem::DropGuard` introduces a standard scope-based execution mechanism that runs a closure upon drop, handling exits triggered by standard returns, error propagation via `?`, or panics. `DropGuard` accepts a target variable along with a cleanup closure, implementing `Deref` and `DerefMut` to allow standard access to the underlying wrapped value.

```rust
fn parse_all(nums: &mut Vec

for s in strings { nums.push(s.parse()?); }

DropGuard::dismiss(nums); Ok(()) } ```

To support transactional, all-or-nothing operations, `DropGuard` includes a static `dismiss` function. Calling `DropGuard::dismiss(guard)` consumes the guard and extracts the underlying resource without executing the registered cleanup closure. This enables concise rollback state management in parser pipelines and unsafe critical sections.

## Low-Level Primitives and Cargo Infrastructure

For low-level memory operations, `std::mem::conjure_zst` provides a single, dedicated function for instantiating zero-sized types (ZSTs). It replaces legacy idioms like transmuting `()` or calling `MaybeUninit::uninit().assume_init()`. Because zero-sized types often carry implicit safety invariants—such as token markers representing lock ownership—`conjure_zst` is explicitly marked as `unsafe`.

Cargo has received two major infrastructural updates:

- Unused dependency reporting via the `cargo::unused_dependencies` lint in `Cargo.toml`, replacing unreliable `rustc` compiler lints for target-shared dependencies.
- Build directory reorganization, moving the configuration hash from individual artifact filenames to parent step directories. This eliminates cache collisions across git worktrees and unblocks cross-workspace deduplication and cache garbage collection.
- Dependency cooldown resolution via stabilized `min-publish-age` controls.
