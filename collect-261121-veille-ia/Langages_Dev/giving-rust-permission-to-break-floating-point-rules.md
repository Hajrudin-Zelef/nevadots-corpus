---
id: collect-261121-veille-ia/langages-dev/giving-rust-permission-to-break-floating-point-rules
title: "Giving Rust Permission to Break Floating-Point Rules"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-08-20"]
keywords: ["algebraic", "values", "rust", "floating-point", "says", "same", "permission", "normal"]
source: https://www.jijobose.com/blogs/rust-1-98-introduces-algebraic-floating-point-methods/
source_anchor: ""
source_lines: [1, 100]
section: "Langages_Dev"
---
Rust 1.98.0, released on August 20, 2026, adds five methods to `f32` and `f64`: `algebraic_add`, `algebraic_sub`, `algebraic_mul`, `algebraic_div`, and `algebraic_rem`. Each one is a `const fn`. Calling one tells the compiler that this operation may be optimized using the algebraic properties of real numbers, including properties that do not hold for IEEE 754 floating-point values.

The 1.98.0 announcement says the exact set of optimizations is unspecified, and that it may resemble `-ffast-math` in other languages. The normative description is the `f32` algebraic operators section (the `f64` section matches it). That section is broader than “you may reorder the sum.”

### What the compiler is allowed to do

Ordinary `+`, `-`, `*`, `/`, and `%` on `f32` and `f64` are rounded IEEE 754 operations. Addition is not associative, so this expression is evaluated strictly left to right:

```
((a + b) + c) + d
```
The compiler has to preserve that grouping. A chain of algebraic adds does not:

```
let x = a.algebraic_add(b).algebraic_add(c).algebraic_add(d);
```
The standard library’s own example says this may be rewritten as the original grouping, or as `(a + c) + (b + d)`, which shortens the dependency chain and is often what lets a loop vectorize.

Reassociation is the case the release notes lead with. The same documentation says the optimizations typically also include:

- combining operations, such as contracting a multiply and an add
- converting a division into a multiplication by a reciprocal
- disregarding the sign of zero

The precision of an individual algebraic operation is unspecified. `NaN`, infinities, and `-0.0` may behave in unexpected ways. Two evaluations with the same inputs may return different values, even inside one run of the program. Reading one result twice is stable: `x.to_bits() == x.to_bits()` holds. Comparing two separate calls is not. The docs’ non-determinism example says `assert!(!x2.is_nan())` may fail even when neither input was `NaN`.

The value guarantee is small. Each call returns some floating-point value. It never causes undefined behavior. Unsafe code must not rely on any property of that value for soundness. Implementations are expected to pick a reasonable tradeoff between performance and accuracy, which is why a lone `algebraic_add` of two ordinary finite numbers usually returns the same bits as `+`. “Usually” is a quality-of-implementation observation. It is not a promise you can test against.

This is the part of `-ffast-math` that Rust copied, and the part it left out. GCC and Clang’s full fast-math mode also assumes operands and results are never `NaN` or infinite. Rust’s unsafe `f*_fast` intrinsics make that same assumption, and breaking it is undefined behavior. The algebraic methods do not. They are safe to call on every `f32` and `f64`, including `NaN` and infinity. In Rust 1.98, rustc’s LLVM backend lowers them with the `reassoc`, `contract`, `arcp`, and `nsz` flags, and leaves `nnan` and `ninf` off. The stable promise is the library documentation above. The flag set is how that backend implements it.

Permission is not a requirement. An unoptimized build, or a loop the optimizer cannot see through, can still evaluate `algebraic_add` exactly like `+`.

### A sum where grouping changes the result

```
fn normal_sum(values: &[f64]) -> f64 {
    values.iter().fold(0.0, |sum, &x| sum + x)
}
fn algebraic_sum(values: &[f64]) -> f64 {
    values.iter().fold(0.0, |sum, &x| sum.algebraic_add(x))
}
fn main() {
    let values = vec![1.0e16, 1.0, -1.0e16, 2.0, 3.0, 4.0];
    println!("normal:    {}", normal_sum(&values));
    println!("algebraic: {}", algebraic_sum(&values));
}
```
`normal_sum` is required to produce `9.0`.

`1e16` is exactly representable as `f64`, but at that magnitude the gap between representable values is `2`. The exact sum `1e16 + 1.0` sits halfway between `1e16` and `1e16 + 2`, and ties round to even, so the `1.0` is lost. Adding `-1e16` then cancels the large term, and `2 + 3 + 4` is all that remains.

A grouping that cancels `1e16` with `-1e16` before adding `1.0` keeps that `1.0` and produces `10.0`. `+` cannot do this. `algebraic_add` can. Whether this `fold` actually does it depends on the compiler version, the optimization level, and the target. Both functions are allowed to print `9`. The algebraic function is also allowed to print something else, because its precision is unspecified. The speedup these methods are for shows up on long reductions, where the same permission lets the compiler build partial sums and vectorize the loop.

### Dot products

A dot product is the loop these methods were added for. With ordinary operators the compiler has to keep a single left-to-right chain of rounded adds, which blocks the vectorizer. Marking the multiply and the add as algebraic lets it form partial products and partial sums:

```
fn dot_product(a: &[f64], b: &[f64]) -> f64 {
    a.iter().zip(b).fold(0.0, |sum, (&x, &y)| {
        sum.algebraic_add(x.algebraic_mul(y))
    })
}
fn main() {
    let a = vec![1.0, 2.0, 3.0, 4.0];
    let b = vec![5.0, 6.0, 7.0, 8.0];
    println!("dot product = {}", dot_product(&a, &b));
}
```
Conceptually the sum is `1×5 + 2×6 + 3×7 + 4×8`. The optimizer may compute the products and add them in any association, and it may contract `x * y + sum` into a fused multiply-add. A fused contraction rounds once. A separate multiply and add round twice. That is a different numeric result, on top of any difference that comes from regrouping.

For these four small inputs the mathematical result is `70`, and every reasonable grouping of these particular values stays at `70`. The interesting case is a long pair of slices, where the same source can vectorize once the adds are algebraic.

### All five methods

```
fn main() {
    let a = 10.0_f64;
    let b = 3.0_f64;
    let add = a.algebraic_add(b);
    let sub = a.algebraic_sub(b);
    let mul = a.algebraic_mul(b);
    let div = a.algebraic_div(b);
    let rem = a.algebraic_rem(b);
    println!("add = {add}");
    println!("sub = {sub}");
    println!("mul = {mul}");
    println!("div = {div}");
    println!("rem = {rem}");
}
```
There is nothing here to reassociate, so a typical compiler prints the same values as `+`, `-`, `*`, `/`, and `%`: `13`, `7`, `30`, a rounded `10/3`, and `1`. `algebraic_div` is still allowed to become a reciprocal multiplication, which can round differently, and `algebraic_sub` is allowed to disregard the sign of zero. A single call is the wrong place to look for a speedup. The methods pay off when several of them form an expression or a loop the optimizer can rewrite.

### Where they belong

Use them inside a numeric kernel that already treats its reduction as an approximation: a graphics or audio inner loop, a machine-learning dot product, a pairwise sum whose algorithm does not depend on left-to-right order. Keep ordinary operators on the steps whose error bound depends on evaluation order.

Leave them out of code that has to reproduce the same bits from one build, one machine, or one call to the next. That includes money, protocol-defined rounding, and tests that compare floating-point bit patterns. Leave them out where the sign of zero, a quiet `NaN`, or an infinity is part of the answer. Unsafe code cannot use the returned bits to justify a safety invariant.

The library documentation is the contract. The 1.98.0 announcement is the shorter version that points at it.
