---
id: collect-261121-veille-ia/langages-dev/release-of-mojo-1-2-programming-language-and-publication-of-max-framework-engine-code
title: "Release Of Mojo 1.2 Programming Language And Publication Of MAX Framework Engine Code"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["language", "code", "mojo", "programming", "release", "project", "under", "llvm"]
source: https://www.altusintel.com/public-yyr7c4/
source_anchor: ""
source_lines: [1, 9]
section: "Langages_Dev"
---
Introduced release of the programming language Mojo 1.2. The Mojo language is being developed under the leadership of Chris Lattner, founder and chief architect of the LLVM project and creator of the Swift programming language. The code of the compiler and accompanying tools necessary for building projects in the Mojo language is distributed under the Apache 2.0 license with exceptions from the LLVM project, which allows mixing with code under the GPLv2 license.

Mojo’s syntax is based on the Python language, and the type system is close to C/C++. The project is presented as a general-purpose language, extending the capabilities of the Python language with systems programming tools, suitable for a wide range of tasks and combining ease of use for research development and rapid prototyping with suitability for the formation of high-performance final products.

Simplicity is achieved through the use of the familiar syntax of the Python language, and the development of final products is facilitated by the ability to compile into machine code, mechanisms safe operation of memory and the use of hardware acceleration tools. To achieve high performance, parallelization of calculations is supported using all the hardware resources of heterogeneous systems available in the system, such as GPUs, specialized accelerators for machine learning and vector processor instructions (SIMD). For intensive computations, parallelization and utilization of all computing resources makes it possible to achieve performance superior to C/C++ applications.

The language supports static typing and features for safe low-level memory handling, reminiscent of Rust-like features such as reference lifetime tracking and borrow checker. At the same time, opportunities for low-level work are also available in the language, for example, it is possible to directly access memory in unsafe mode using the Pointer type, call individual SIMD instructions, or access to hardware extensions such as TensorCores and AMX.

Mojo can be used both in interpretation mode using JIT, and for compiling into executable files (AOT, ahead-of-time). The compiler has built-in modern technologies for automatic optimization, caching and distributed compilation. Source code in the Mojo language is converted into low-level intermediate code MLIR (Multi-Level Intermediate Representation), developed by the LLVM project. The compiler allows you to use various backends that support MLIR to generate machine code.
