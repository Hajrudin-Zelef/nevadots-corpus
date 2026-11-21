---
id: collect-261121-veille-ia/go-json/numbers-numeric-computing
title: "Numbers & Numeric Computing"
domain: go-json
role: reference
task: article
actors: []
dates: ["2026-10-06"]
keywords: ["algebraic", "numeric", "println", "numbers", "integer", "iter", "computing", "float"]
source: https://www.irurueta.com/programming-languages/rust/numbers-and-numeric-computing.html
source_anchor: ""
source_lines: [1, 175]
section: "Go_JSON"
---
# Numbers & Numeric Computing

|  | This content was generated with the assistance of AI and should be verified against the official documentation before being relied on in production. | 

This page is written against Rust 1.99.0, num-traits 0.2, ndarray 0.17, nalgebra 0.35, faer 0.24, statrs 0.19 and rand
0.10, checked on 2026-10-06. Rust’s numeric types are covered on Types
& Values; this page focuses on what changes when the numbers **are** the program: float accuracy and NaN policies, generic
numeric code, and the crates the scientific Rust ecosystem is built on. The Crossley et al. book was consulted only to
check which topics a reference should cover; no content is taken from it.

- 
Integer overflow panics in debug builds and wraps in release builds; say what you mean with `checked_` ,`wrapping_` ,`saturating_` ,`overflowing_` or`strict_` methods. Division by zero always panics.
- 
`f32` and`f64` are IEEE 754 binary floats:`0.1 + 0.2 != 0.3` , there are signed zeros, infinities and NaN.
- 
Floats implement only `PartialEq` /`PartialOrd` . Sorting and`max` over floats need an explicit policy:`sort_by(f64::total_cmp)` (NaN sorts last),`f64::max` /`min` (ignore NaN), or filtering NaN first.
- 
`as` casts from float to integer saturate and map NaN to 0; from integer to float they round.

```
fn kahan_sum(values: &[f64]) -> f64 {
    let (mut sum, mut compensation) = (0.0, 0.0);
    for &v in values {
        let y = v - compensation;
        let t = sum + y;
        compensation = (t - sum) - y;   // the low-order bits lost in sum + y
        sum = t;
    }
    sum
}
fn main() {
    let tenths = vec![0.1; 1_000_000];
    let naive: f64 = tenths.iter().sum();
    println!("naive {naive:.10}  kahan {:.10}", kahan_sum(&tenths)); // naive drifts, kahan ~ 100000.0
    // mul_add computes a*b + c with one rounding (a fused multiply-add instruction where available).
    let (a, b, c) = (0.1_f64, 10.0, -1.0);
    println!("{:e} vs {:e}", a * b + c, a.mul_add(b, c));
    let readings = [3.2, f64::NAN, 2.9, 4.1];
    let max_ignoring_nan = readings.iter().copied().fold(f64::NEG_INFINITY, f64::max);
    let any_nan = readings.iter().any(|x| x.is_nan());
    println!("{max_ignoring_nan} {any_nan}");                         // 4.1 true
    let mut sorted = readings;
    sorted.sort_by(f64::total_cmp);
    println!("{sorted:?}");                                           // NaN last
    println!("{}", (0.1_f64 + 0.2 - 0.3).abs() < 1e-12);              // compare with a tolerance
}
```
By default, the compiler must preserve the exact IEEE 754 result of every float operation, so it cannot reorder
`a + b + c` or use fused multiply-add on its own — which also blocks auto-vectorisation of reductions. Rust 1.98
stabilised the **algebraic** methods `algebraic_add`, `algebraic_sub`, `algebraic_mul`, `algebraic_div` and `algebraic_rem`
on `f32` and `f64`: they compute the same operation but allow the optimiser to treat it as if it were associative and
commutative (and to fuse multiply and add). Results may differ in the last bits between optimisation levels and targets;
use them in hot numeric loops where that is acceptable.

```
fn dot_strict(a: &[f64], b: &[f64]) -> f64 {
    a.iter().zip(b).map(|(x, y)| x * y).sum()
}
fn dot_algebraic(a: &[f64], b: &[f64]) -> f64 {
    a.iter().zip(b).fold(0.0, |acc, (x, y)| acc.algebraic_add(x.algebraic_mul(*y))) // vectorisable
}
fn main() {
    let a: Vec<f64> = (0..1000).map(|i| f64::from(i) * 0.5).collect();
    let b: Vec<f64> = (0..1000).map(|i| f64::from(i % 7)).collect();
    println!("{} {}", dot_strict(&a, &b), dot_algebraic(&a, &b));
}
```
- 
`std::num::NonZero<T>` (generic since 1.79) is an integer that cannot be zero. It documents an invariant (a page size, a
divisor, an ID) and lets`Option<NonZero<u32>>` be 4 bytes.`n.get()` returns the plain integer.
- 
`u128` and`i128` are native 128-bit integers, handy for exact money arithmetic in small units, large counters,
identifiers (UUIDs fit in a`u128` ) and intermediate products that would overflow`u64` . They are slower than 64-bit
arithmetic on most CPUs.

```
use std::num::NonZero;
fn pages(total_items: u64, per_page: NonZero<u64>) -> u64 {
    total_items.div_ceil(per_page.get())   // no division-by-zero check needed
}
fn main() {
    let per_page = NonZero::new(20).expect("non-zero");
    println!("{} pages", pages(95, per_page));
    let big = u128::from(u64::MAX) * u128::from(u64::MAX);  // fits easily
    println!("{big} {}", u128::BITS);
}
```
The standard library has no trait for "any number". Generic numeric code uses the **num-traits** crate: `Zero`, `One`,
`Num`, `Float`, `NumCast`, `Bounded`, `CheckedAdd` and so on, implemented for all primitive numeric types:

```
use num_traits::{Float, Zero};
fn mean<T: Float>(values: &[T]) -> Option<T> {
    if values.is_empty() {
        return None;
    }
    let sum = values.iter().fold(T::zero(), |acc, &x| acc + x);
    Some(sum / T::from(values.len())?)
}
fn sum_all<T: Zero + Copy + std::ops::Add<Output = T>>(values: &[T]) -> T {
    values.iter().fold(T::zero(), |acc, &x| acc + x)
}
fn main() {
    println!("{:?} {:?}", mean(&[4.0_f64, 5.0, 3.5]), mean::<f32>(&[]));
    println!("{} {}", sum_all(&[1u8, 2, 3]), sum_all(&[1.5_f32, 2.5]));
}
```
Rust’s numeric ecosystem is younger than Python’s or Julia’s but solid for production use. The main crates, as pointers:

| Crate | What it is for | 
|---|---|
| ndarray | N-dimensional arrays with slicing, broadcasting and views — the NumPy equivalent; optional BLAS acceleration and Rayon parallelism | 
| nalgebra | linear algebra with statically and dynamically sized vectors and matrices, geometry (rotations, isometries) — popular in robotics, graphics and physics | 
| faer | high-performance dense linear algebra (decompositions, solvers) written in pure Rust, competitive with optimised BLAS/LAPACK | 
| statrs | probability distributions and statistics | 
| rand | random number generation; 0.9 renamed `thread_rng` /`gen_range` to`rng` /`random_range` , and 0.10 renamed the`Rng` extension trait to`RngExt` | 
| polars | dataframes (with a Python front end of the same name) | 

```
use nalgebra::{Matrix2, Vector2};
use ndarray::{Array2, array, s};
fn main() {
    // ndarray: 2-D array, slicing and element-wise operations.
    let ratings: Array2<f64> = array![[5.0, 4.0, 3.0], [2.0, 5.0, 4.0]]; // rows = readers, cols = books
    let book_means = ratings.mean_axis(ndarray::Axis(0)).expect("non-empty");
    println!("{book_means}");                       // [3.5, 4.5, 3.5]
    println!("{}", ratings.slice(s![.., 1..]));     // all rows, books 1 and 2
    println!("{}", &ratings * 2.0);
    // nalgebra: solve a small linear system A x = b.
    let a = Matrix2::new(2.0, 1.0, 1.0, 3.0);
    let b = Vector2::new(3.0, 5.0);
    let x = a.lu().solve(&b).expect("invertible");
    println!("x = {x}");
    // rand 0.10: free functions for the common cases.
    let sample: f64 = rand::random_range(0.0..1.0);
    println!("{}", (0.0..1.0).contains(&sample));
}
```
For another scientific-computing ecosystem, one built around statistics, see R: Statistical Models. For a matrix-first language with the same kinds of arrays and linear algebra, see MATLAB: Matrices and Arrays.

ndarray (with its `blas` feature) and many other crates can delegate matrix products to a **BLAS** library: OpenBLAS,
Intel MKL, Apple Accelerate or BLIS, selected through the `blas-src` crate and its features. This gives the same
performance as NumPy for large matrix multiplications, at the cost of a native dependency. faer avoids that dependency
by implementing the kernels in Rust.

Element-wise and reduction loops over large arrays parallelise well with Rayon (`par_iter`, or ndarray’s `par_map_inplace`
and `Zip::par_for_each` with the `rayon` feature). Memory bandwidth, not core count, is often the limit; measure with
criterion. See Data Parallelism with Rayon, and for
data layout (struct of arrays versus array of structs) see Memory
Layout & repr.

Much numeric work lives in Python (NumPy), C and Fortran. Rust can call C and Fortran libraries through FFI (Fortran with
`bind©` interfaces) and expose Rust kernels to Python with PyO3 and the `numpy` crate, which maps NumPy arrays to
ndarray views without copying. See FFI & C Interop and
Python Interop with PyO3. The Fortran side of the same boundary
is covered in Interoperability with C.

The Crossley et al. book (consulted for topic coverage only) uses ndarray 0.15, rayon 1.7 and rand 0.8’s `thread_rng` and
`gen_range`; current versions are ndarray 0.17, rayon 1.12 and rand 0.10 (`rand::rng()`, `random_range`, `RngExt`). The
algebraic float methods (1.98) and generic `NonZero<T>` (1.79) are newer than all the books.

- 
std: f64 and u128
- 
std::num::NonZero
- 
The Rust Reference: Numeric types
- 
num-traits
- 
ndarray, nalgebra, faer, statrs, rand
- 
Rayon
- 
Ethan Crossley, Hayden Van Der Post and Alice Schwartz, Rust for Scientific Computing (Reactive Publishing, 2026) — consulted for topic coverage only
