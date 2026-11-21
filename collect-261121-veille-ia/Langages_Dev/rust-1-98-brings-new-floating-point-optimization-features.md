---
id: collect-261121-veille-ia/langages-dev/rust-1-98-brings-new-floating-point-optimization-features
title: "Rust 1.98 Brings New Floating-Point Optimization Features"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["rust", "floating-point", "algebraic", "compiler", "string", "methods", "integer", "stabilized"]
source: https://linuxiac.com/rust-1-98-brings-new-floating-point-optimization-features/
source_anchor: ""
source_lines: [1, 15]
section: "Langages_Dev"
---
The Rust team has announced Rust 1.98, the latest stable version of the popular systems programming language, introducing new algebraic floating-point methods, faster buffered integer formatting, and a broad selection of newly stabilized standard library APIs.

One of the most notable additions is a new family of “algebraic” methods for the `f32` and `f64` floating-point types. These cover addition, subtraction, multiplication, division, and remainder operations, allowing the compiler to perform optimizations based on the algebraic properties of real numbers even when those properties do not strictly hold for floating-point representations.

This gives the compiler more freedom to reorder floating-point operations and can enable broader loop vectorization and other performance optimizations. The Rust team compares these optimizations to those enabled by options like `-ffast-math` in other languages, although the exact set is intentionally unspecified.

For example, a normal expression such as `a + b + c + d` must retain its left-associative evaluation order because floating-point addition is not associative. When the same calculation is expressed using the new `algebraic_add` methods, however, the compiler is free to reorganize it, potentially evaluating partial sums in parallel.

Rust 1.98 also introduces a new `format_into` method for all primitive integer types. It works with the newly stabilized `NumBuffer` type, which provides enough storage for the decimal representation of any value of the corresponding integer type. The formatted result is returned as a string slice borrowing from that buffer.

Another change concerns the interaction between `ManuallyDrop` and `Box`. Before Rust 1.96, moving a `ManuallyDrop<Box<_>>` after explicitly dropping the contained Box could be undefined behavior due to a compiler issue. That was corrected in Rust 1.96, and Rust 1.98 updates the `ManuallyDrop` documentation to guarantee such code will not be considered undefined behavior going forward.

String handling also sees several additions, including `String::from_utf16le`, `String::from_utf16be`, and their lossy counterparts for converting explicitly little-endian and big-endian UTF-16 data. The new `strip_circumfix` method has also been stabilized for both strings and slices.

For additional details, see the announcement. As usual, existing Rust users can upgrade through rustup by running `rustup update stable`.
