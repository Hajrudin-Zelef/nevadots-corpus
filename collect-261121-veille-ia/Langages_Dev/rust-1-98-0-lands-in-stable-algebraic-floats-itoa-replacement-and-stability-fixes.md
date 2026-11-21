---
id: collect-261121-veille-ia/langages-dev/rust-1-98-0-lands-in-stable-algebraic-floats-itoa-replacement-and-stability-fixes
title: "Rust 1.98.0 Lands in Stable: Algebraic Floats, itoa Replacement, and Stability Fixes"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["stable", "algebraic", "rust", "itoa", "standard", "replacement", "floats", "library"]
source: https://www.linuxcompatible.org/story/rust-1980-lands-in-stable-algebraic-floats-itoa-replacement-and-stability-fixes/
source_anchor: ""
source_lines: [1, 26]
section: "Langages_Dev"
---
### Rust 1.98.0 Ships in Stable: Algebraic Floats, itoa Replacement, and a Few Other Tidbits

**The new version of the systems language focuses on performance tuning for numerical workloads and finally brings integer serialization into the standard library.**

The Rust team has officially shipped Rust 1.98.0, and if you squeeze numbers for a living, this one will interest you. The release lands a new set of stable APIs, performance-oriented floating-point methods, and a standard library replacement for the long-running itoa crate.

#### Algebraic floats finally hit stable

The headline feature is a set of methods for f32 and f64 called algebraic_add, algebraic_sub, algebraic_mul, algebraic_div, and algebraic_rem. The idea is straightforward: tell the compiler it can reorder these operations using standard algebra rules. Since normal Rust floating-point math locks things to left-to-right evaluation, the compiler has always been handcuffed when trying to vectorize nested loops. Now it can evaluate (a + b) + (c + d) instead of ((a + b) + c) + d. That opens the door to broader loop vectorization and other speed boosts.

There's a tradeoff, though. These methods are explicitly marked as non-deterministic. You will get slightly different results between builds. The Rust team is clear about that boundary. They never trigger undefined behavior, but if you're building a financial calculator that needs bit-exact reproducibility, stick to the standard operators. For scientific computing and large-scale numerical simulations where performance outweighs exact decimal matches, this is the kind of optimization the language has been quietly waiting for.

#### format_into takes the standard library seat at the table

If you've ever written hot paths that serialize integers, you've probably reached for itoa. Now you don't have to. All primitive integer types gain a format_into method that writes directly into a stack-allocated NumBuffer. It drops the dynamic dispatch overhead that usually creeps into buffered formatting macros.

let mut buf = NumBuffer::<usize>::INIT;
let s = 42.format_into(&mut buf);

Benchmarks against itoa-benchmark show it runs on par with the crate itself. That makes it a legitimate drop-in replacement for integer serialization. If you're writing a parser, a log formatter, or anything that spills numbers to disk or a network socket, the standard library just saved you a dependency. As of right now, the ecosystem has been waiting for a standard library integer formatter to match itoa's performance for years, and 1.98 finally delivers.

The rest of the release is mostly a collection of stabilization touches. There's an official stable documentation guarantee around ManuallyDrop<Box<_>> behavior, formalizing a compiler quirk that was patched in 1.96.0 under RFC 3336. Not a bug fix in 1.98 itself, but good to see the docs finally catch up to the compiler.

Other new stable APIs include str::substr_range and [T]::subslice_range, a strip_circumfix method for both, Send and Sync implementations for std::process::CommandArgs, radix parsing on NonZero integers, lossy UTF-16 conversion methods on String, and a handful of atomic mutation helpers. There's also a std::range::legacy module if you're maintaining older codebases.

Upgrading is as straightforward as running rustup update stable. If you're new to the ecosystem, installation instructions are right there on the official website. Full compiler, Cargo, and Clippy changes are tracked in the GitHub release notes and the dedicated stable release page.
