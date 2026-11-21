---
id: collect-261121-veille-ia/langages-dev/go-1-27-simd-package-brings-portable-emulated-simd
title: "Go 1.27 simd package brings portable, emulated SIMD"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["simd", "vector", "package", "portable", "code", "instructions", "width", "bits"]
source: https://dev.to/techaiwire/go-127-simd-package-brings-portable-emulated-simd-53ii
source_anchor: ""
source_lines: [1, 50]
section: "Langages_Dev"
---
The Go team has explained how its new portable SIMD package works, in a post on the Go blog dated September 24, 2026. Go 1.27 ships an experimental `simd` package that lets one piece of Go code use vector instructions on many different CPUs. That matters because until Go 1.26, the only way to reach those instructions from Go was to write assembly by hand.

SIMD stands for single instruction, multiple data. It is a CPU feature that applies one operation to a whole batch of values at once, such as adding eight pairs of numbers in a single step. It speeds up work like compression, cryptography, image processing and machine learning.

Go now has two SIMD packages, and both are still experiments. The first, `archsimd`, arrived in Go 1.26. It is architecture-dependent: it exposes each CPU family's own instructions, so code written for Intel chips does not run on Arm.

The second is the new portable `simd` package in Go 1.27. The post's authors, David Chase and Junyang Shao, describe it as "an experimental platform-agnostic SIMD API." Phoronix notes that the design is modeled on Highway, Google's C++ library for portable vector code.

Both packages sit behind the same switch. You build with the environment variable `GOEXPERIMENT=simd` to turn them on.

CPU families disagree on almost everything about vectors. The Go blog lists the vector widths each platform supports.

| Platform | Vector instructions | Vector width | 
|---|---|---|
| amd64 | AVX, AVX2, AVX-512 | 128, 256 and 512 bits | 
| arm64 | NEON | 128 bits | 
| arm64 (planned for Go 1.28) | SVE | 128 to 2,048 bits | 
| wasm | WebAssembly SIMD | 128 bits | 
| riscv64 | RVV | 128 to 65,536 bits | 

The post also names loong64, ppc64 and s390x as supported. Some chips only reveal their vector width when a program starts, so code cannot assume a size at build time.

The `simd` package solves this by leaving the width out of the types. Vector types are named like capitalized plural primitives: `simd.Float32s`, `simd.Int8s`, `simd.Uint64s`. A `Float32s` holds as many 32-bit floats as the current CPU can process at once. Masks, the true-or-false values that comparisons produce, get matching types such as `Mask32s`.

The compiler does the specializing. According to the Go blog, it rewrites functions that use `simd` into variants for each vector width, labeled like `@simd128`. The program then runs the variant that fits the machine, with no dispatch overhead inside the hot loop.

A `GODEBUG` setting controls the choice at run time. `simd=0` turns vector code off, and `simd=128`, `simd=256` or `simd=512` cap the width. No recompiling is needed to test each path.

Where a platform has no suitable instructions, every operation is emulated. The authors list their goals for the package. It should be "as efficient as assembly language when the source code operations match the underlying hardware." It should be "emulated as well as possible" otherwise. And it should be "easy to read and understand (even/especially if an LLM ends up writing the code)."

The first release has clear gaps. There is no horizontal reduction, which means no built-in way to sum all the elements of one vector. The blog's own example writes a small scalar loop for that step and says a `ReduceSum` operation will come in the next release.

Arm's SVE instructions are also scheduled for Go 1.28, Phoronix reports, along with more operations. Some operations are unavailable on some architectures today.

If your Go service has a hot loop over slices of numbers, try the package now on a branch. Good candidates are checksums, parsing, distance calculations for vector search and image filters. The portable API lets you write that loop once instead of once per CPU family.

Benchmark every width, not just your laptop's. Run the same benchmark with `GODEBUG=simd=0`, `simd=128`, `simd=256` and `simd=512`. The zero setting gives you the scalar baseline for free, and the others show whether wider vectors actually pay off on your data.

Keep it out of anything you cannot rebuild quickly. Both SIMD packages are gated behind `GOEXPERIMENT`, and experimental APIs can change between releases. Plan for the missing `ReduceSum` too, since any code that sums a vector needs a temporary scalar loop until Go 1.28.

This is the same trade other ecosystems are making. Go already had one real-world win: Debian Code Search dropped its last C dependency using the older `archsimd` package. Rust took the library route this month, when Fearless SIMD 1.0 shipped a stable, safe SIMD crate. Go is building the capability into its toolchain, with the compiler doing the per-CPU work.

*This article was first published on Tech AI Wire.*

Deutsch · 日本語 · Français · Español · Português

- Platform-independent SIMD in Go - The Go Blog
- Go's Improving SIMD Support, Platform-Independent SIMD Interface - Phoronix

## Top comments (0)
