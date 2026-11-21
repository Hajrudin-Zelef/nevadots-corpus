---
id: collect-261121-veille-ia/langages-dev/rust-1-100-0-released-new-features-and-cargo-improvements-ostechnix
title: "Rust 1.100.0 Released: New Features and Cargo Improvements - OSTechNix"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-12"]
keywords: ["rust", "type", "value", "never", "system", "memory", "allocator", "released"]
source: https://ostechnix.com/rust-1-100-0-new-features/
source_anchor: ""
source_lines: [1, 509]
section: "Langages_Dev"
---
*152*

**Rust 1.100.0 is officially released on November 12, 2026.** This release brings several important improvements to the Rust programming language, standard library, and Cargo package manager.

The major changes include the stabilisation of the never type (`!`), custom memory allocator support, the new `DropGuard` API, unused dependency reporting, improved Cargo build-cache management, and dependency cooldowns for better software supply-chain security.

Rust 1.100 is not a release that completely changes how developers write Rust code. Instead, it makes existing features more complete, improves memory management, and provides better tools for building reliable applications.

In this guide, we will explore the important Rust 1.100.0 features, explain how they work, and look at how they affect existing Rust projects.

Table of Contents

## `!`) Is Now Stable

One of the biggest changes in Rust 1.100 is the stabilisation of the **never type**, represented by `!`. The never type represents a computation that never produces a normal value.

For example:

```
fn exit_program() -> ! {
    std::process::exit(1);
}
```
This function never returns control to its caller.

Other expressions that can have the never type include:

- `return`
- `break`
- `panic!()`
- `std::process::exit()`

Rust uses its type system to check whether different parts of a program are valid.

Consider this example:

```
fn get_value(option: Option<u32>) -> u32 {
    match option {
        Some(value) => value,
        None => return 0,
    }
}
```
The first branch produces a `u32`. The second branch returns from the function.

The never type makes it possible to handle branches that produce a value and branches that stop execution.

Rust allows a value of type `!` to coerce into another type because a computation of type `!` never produces a value.

### What's new in Rust 1.100?

Previously, developers could use `!` as a function return type, but they could not explicitly name it everywhere in the type system. Rust 1.100 allows developers to use the never type in more positions.

For example:

```
fn infallible_unwrap<T>(res: Result<T, !>) -> T {
    let Ok(value) = res;
    value
}
```
Here, `Result<T, !>` means that the result can contain a successful value but cannot contain an error.

Another important change is that:

std::convert::Infallible

is now an alias for `!`.

This makes the type system more consistent and allows APIs that previously used `Infallible` to work with the never type.

The never-type stabilisation also introduces a compatibility change. In Rust editions 2015, 2018, and 2021, the never-type fallback changes from `()` to `!`.

Most projects should not notice this change. However, some generic code may behave differently, especially when the return value of a generic function is ignored.

The compiler may also identify more unreachable code. If an older dependency stops compiling after upgrading to Rust 1.100, check whether an updated version is available.

Rust 1.100 stabilises the core parts of the `Allocator` API.

Memory allocation is the process of requesting space for data while a program is running. Rust normally handles memory allocation through its global allocator. However, some applications need more control over how memory is allocated and released.

The allocator API allows library developers to define custom memory allocators and use them with supported standard library types.

In Rust 1.100, the supported types include:

- `Vec<T, A>`
- `Box<T, A>`

The release also stabilises the `Allocator` trait and important allocator-related functions.

Rust provides the `System` allocator, which uses the system's standard memory allocation facilities.

You can use it with a vector:

use std::alloc::System;

fn main() {

let mut values: Vec<u32, System> =

Vec::new_in(System);

values.push(10);

values.push(20);

println!("{values:?}");

}

Here, the vector uses `System` as its allocator instead of relying on the default allocator type.

You can also create a boxed value with a specified allocator:

use std::alloc::System;

fn main() {

let value = Box::new_in(42, System);

println!("{}", *value);

}

Custom memory allocators are useful when an application needs a particular memory allocation strategy.

Common use cases include:

- Game engines.
- Database systems.
- Embedded applications.
- High-performance applications.
- Arena allocation.
- Memory pools.
- Applications that manage separate memory regions.

For example, an arena allocator can allocate several objects from one memory region and release that memory together. This can reduce allocation overhead in suitable workloads and simplify memory management.

**Important:** Custom allocators do not automatically make every Rust application faster. Performance depends on the application and the allocation strategy being used.

Rust 1.100 provides the foundation for broader custom allocator support. More standard library collections and pointer types still need additional stabilisation work.

Rust already provides automatic resource cleanup through the `Drop` trait. When a value goes out of scope, Rust runs its destructor. This also happens when a function exits early using `return` or `?`, and during panic unwinding. However, sometimes developers need to run a specific cleanup operation when a function exits.

Rust 1.100 introduces:

std::mem::DropGuard

`DropGuard` accepts a value and a cleanup closure. The closure runs when the guard is dropped.

It also implements `Deref` and `DerefMut`, so developers can continue working with the wrapped value normally.

Imagine that a function modifies a vector by adding numbers parsed from strings. If parsing fails halfway through, you may want to remove the values added during that operation.

You can use `DropGuard` to handle this:

use std::mem::DropGuard;

use std::num::ParseIntError;

/// Parse all strings and add their values to the vector.

/// If parsing fails, the vector returns to its original length.

fn parse_all(

nums: &mut Vec<u32>,

strings: &[&str],

) -> Result<(), ParseIntError> {

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

Here is how the code works:

1. Save the original vector length.
2. Create a guard with a cleanup closure.
3. Parse each string and add the result to the vector.
4. If parsing fails, `?` returns early.
5. The guard runs its cleanup closure and restores the original vector length.
6. If parsing succeeds, `dismiss()` prevents the cleanup closure from running.

This is useful for rollback operations, temporary modifications, and error handling. It can also reduce the need to create a separate custom guard type for every cleanup task.

One important detail: `DropGuard::dismiss()` is an associated function, not a normal method.

Also, cleanup during a panic depends on the panic strategy. With `panic = "abort"`, destructors are not run after a panic.

Rust 1.100 introduces another API for working with zero-sized types (ZSTs). A zero-sized type is a type that occupies zero bytes of memory.

For example:

struct Token;

Although this type does not store any data, it can still represent an important condition or capability. For example, PyO3 uses `Python<'py>` as a marker token representing access to the Python Global Interpreter Lock (GIL).

Some Python operations require the GIL to be held before they can be called safely. Creating a `Python<'py>` token without actually holding the GIL could violate those safety requirements.

Rust 1.100 introduces:

std::mem::conjure_zst

This provides a dedicated way to construct a zero-sized value when the developer knows that doing so is safe.

Before this API, developers sometimes used techniques such as:

std::mem::transmute(())

or:

MaybeUninit::uninit().assume_init()

for certain zero-sized type construction tasks.

The new function provides a dedicated API instead of relying on these workarounds. However, `conjure_zst` is an unsafe function. Developers must ensure that creating the particular type does not violate its safety requirements or invariants.

This is especially relevant to library authors, systems programmers, and developers working with unsafe Rust.

Cargo is Rust's package manager and build tool. It manages dependencies, downloads external crates, builds projects, and handles package configuration.

As projects grow, developers sometimes forget to remove dependencies that are no longer used.

For example:

[dependencies]

tracing-subscriber = "0.3"

If the project no longer uses `tracing-subscriber`, keeping it in `Cargo.toml` creates an unnecessary dependency.

Rust 1.100 improves Cargo's ability to identify and report unused dependencies.

Unnecessary dependencies can:

- Increase build time.
- Increase dependency maintenance work.
- Add extra crates to the dependency tree.
- Make dependency audits more difficult.
- Increase the amount of third-party code a project relies on.

Cargo now reports dependencies that are declared but not used by the relevant build targets.

For example, a warning might look like this:

warning: unused dependency `tracing-subscriber`

--> Cargo.toml:23:1

= note: cargo::unused_dependencies is set to warn by default

help: consider removing the dependency

Cargo needs to check every relevant build target before it can reliably identify unused dependencies.

For example:

cargo check --bin foo

may not report an unused dependency if another binary in the same package uses it.

Cargo also does not report unused `dev-dependencies` through this feature because it cannot guarantee that every development target has been built. This improvement is quite useful for large Rust workspaces, libraries, and applications with multiple binaries.

Cargo uses a build cache to reuse previously compiled files. When developers make a small change to a project, Cargo can often reuse existing build results instead of compiling everything again. This saves time, especially in large projects.

However, different build configurations need separate build outputs. For example:

cargo build

cargo build --features "feature-a"

cargo build --features "feature-b"

These commands can produce different versions of the same dependencies and application components. Cargo needs to keep these build results separate to avoid using the wrong files.

Cargo now applies build-configuration hashes more consistently across build-directory contents.

It also changes how build artifacts are organised:

- Build outputs are organised around their build steps.
- Configuration hashes move from filenames into parent directories.
- Different build configurations get more consistent separation.

This is useful for developers working with:

- Cargo workspaces.
- Multiple feature combinations.
- Git worktrees.
- Shared build directories.
- Projects with different build configurations.

The goal is to reduce build-cache collisions, avoid unnecessary recompilation, and make Cargo build behaviour more reliable. This is an internal Cargo change, but it may affect scripts and tools that depend on the previous build-directory layout. If your project uses workarounds for the old layout, check whether they are still needed after upgrading.

One of the most interesting changes in Rust 1.100 is the stabilisation of Cargo's minimum publish-age feature. This feature helps protect Rust projects against certain software supply-chain attacks.

Modern applications often depend on external libraries and packages.

For example, a Rust application might depend on dozens or hundreds of crates maintained by different developers. If an attacker compromises a crate maintainer's account or publishes a malicious version of a legitimate crate, applications could accidentally download and use that version. This is known as a **software supply-chain attack**.

A dependency cooldown gives developers and security researchers additional time to identify suspicious releases before Cargo selects them during dependency resolution.

Developers can configure Cargo to skip crate versions that were published too recently.

Add this configuration to `.cargo/config.toml`:

```
[registry]
global-min-publish-age = "3 days"
```
With this setting, Cargo skips versions published less than three days ago when selecting dependency versions.

For example, Cargo might display:

Updating crates.io index

Locking packages to highest compatible version

as of min-publish-age

Adding cc v1.11.0

(available: v1.18.0, published 3 hours ago)

Adding clap v4.6.7

(available: v4.6.9, published 2 days ago)

The newer versions are held back because they have not passed the configured waiting period. Cargo can still inform developers that newer versions are available.

You can override the cooldown for a particular update:

CARGO_RESOLVER_INCOMPATIBLE_PUBLISH_AGE=allow \

cargo update clap --precise 4.6.9

This allows Cargo to select the requested version even if it has not passed the configured waiting period. The selected version is recorded in `Cargo.lock` and remains there until a later dependency update changes the resolution.

There are several things developers should understand:

1. Cargo depends on registry publication timestamps (`pubtime` ). Versions without this information are not held back.
2. `cargo install` ignores the minimum publish-age setting for both the installed crate and its dependencies.
3. A malicious release can remain undetected beyond the configured waiting period.
4. Dependency cooldowns do not replace security audits or dependency reviews.
5. The feature provides an additional layer of protection, not complete security.

For production applications and projects that regularly update dependencies, this is a useful Cargo security feature.

Rust 1.100 also stabilises several smaller standard library APIs. These improvements provide additional tools for working with memory, strings, collections, and smart pointers.

Here are some notable additions:

| API | Description | 
| `bool::toggle()` | Changes a mutable Boolean value to its opposite | 
| `Box::take()` | Extracts a value while retaining its allocation | 
| `Box::map()` | Transforms the value inside a `Box` | 
| `Rc::map()` | Transforms the value inside an `Rc` | 
| `Arc::map()` | Transforms the value inside an `Arc` | 
| `str::trim_prefix()` | Removes a matching string prefix | 
| `str::trim_suffix()` | Removes a matching string suffix | 
| `[T]::trim_prefix()` | Removes a matching prefix from a slice | 
| `[T]::trim_suffix()` | Removes a matching suffix from a slice | 
| `Path::trim_prefix()` | Removes a matching path prefix | 
| `Vec::from_fn()` | Creates a vector using a function to generate its elements | 
| `UnsafeCell::replace()` | Replaces the stored value and returns the previous value | 
| `UnsafeCell::as_ref_unchecked()` | Creates a shared reference without checking the usual borrow rules | 
| `UnsafeCell::as_mut_unchecked()` | Creates a mutable reference without checking the usual borrow rules | 

Rust 1.100 also adds new `PartialEq` implementations for comparing vectors and slices with `Cow` values. These APIs are useful improvements for everyday Rust development, although their value depends on the type of application you are building.

If you manage your Rust installation using `rustup`, you can update the stable toolchain with:

rustup update stable

Check the installed versions:

rustc --version

cargo --version

After upgrading, check and test your existing projects:

cargo check --all-targets

cargo test --all-targets

These commands help identify compilation errors and test failures across the relevant targets. You should also review compiler warnings, especially those related to never-type fallback and unused dependencies.

For projects using a `rust-toolchain.toml` file, check the configured toolchain because the project may select a specific Rust version. If your Linux distribution manages Rust through its own package manager, you can continue using that method instead of `rustup`.

Here is a quick summary of the major Rust 1.100 improvements:

| Area | Main improvement | Benefit | 
| Rust language | Never type ( `!` ) stabilisation | More consistent type handling | 
| Memory management | Custom allocator API | More control over memory allocation | 
| Standard library | `DropGuard` | Easier cleanup and rollback | 
| Unsafe Rust | `conjure_zst` | Dedicated zero-sized type construction | 
| Cargo | Unused dependency reporting | Cleaner dependency lists | 
| Cargo | Build-cache improvements | More reliable build output separation | 
| Cargo security | `min-publish-age` | Additional protection against malicious releases | 
| Standard library | New stable APIs | More useful programming tools | 

Want to explore the new features in Rust 1.100.0 on your Linux system? You can install Rust using **rustup**, the official Rust toolchain installer.

If you haven't installed Rust yet, follow our **Rust installation guide for Linux** before continuing.

Open a terminal and check the installed compiler and Cargo versions:

rustc --version

cargo --version

To use Rust 1.100.0, your compiler should report a version beginning with:

rustc 1.100.0

If you already have rustup installed, update your stable toolchain:

rustup update stable

Make sure the stable toolchain is selected:

rustup default stable

Verify the installed version:

rustc --version

cargo --version

Create a new project using Cargo:

cargo new rust-100-demo

cd rust-100-demo

This creates a basic Rust application with a `src/main.rs` file.

Open `src/main.rs` and replace its contents with:

use std::process::Command;

fn main() {

let output = Command::new("rustc")

.arg("--version")

.output()

.expect("Failed to execute rustc");

println!(

"Installed Rust version: {}",

String::from_utf8_lossy(&output.stdout).trim()

);

println!("Ready to explore Rust 1.100.0!");

}

This program detects the installed Rust version and confirming that the environment is ready to try Rust 1.100.0.

Use Cargo to compile and execute your application:

cargo check

cargo run

You should see output similar to:

Installed Rust version: rustc 1.100.0 (0ef1bc386 2026-11-09)

Ready to explore Rust 1.100.0!

Once your environment is ready, try the features introduced in this release:

Rust 1.100.0 is an important improvement release for the Rust programming language and its development tools. It does not introduce a completely new programming model. Instead, it makes several existing parts of Rust more complete and practical.

The stabilisation of the never type is a significant language improvement. Custom allocator support gives systems programmers more control over memory. `DropGuard` provides a useful abstraction for cleanup and rollback operations.

Cargo also receives improvements that matter in everyday development. Unused dependency reporting helps keep projects clean, while build-cache changes improve build management. The dependency cooldown feature deserves particular attention because software supply-chain security is an increasingly important issue for modern application development.

**Overall, Rust 1.100 focuses on better type handling, more flexible memory allocation, reliable builds, and stronger dependency management.**

These improvements are valuable for developers maintaining Rust applications, libraries, command-line tools, and systems software. Whether you are building a small command-line utility or maintaining a large Rust workspace, Rust 1.100 provides several useful improvements worth understanding.

**Resources:**
