---
id: collect-261121-veille-ia/langages-dev/go-language-1-27-adds-generic-methods-struct-improvement-more-simd
title: "Go Language 1.27 Adds Generic Methods, Struct Improvement & More SIMD"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["simd", "generic", "methods", "struct", "language", "more", "adds", "improvement"]
source: https://www.phoronix.com/news/Go-1.27
source_anchor: ""
source_lines: [1, 9]
section: "Langages_Dev"
---
# Go Language 1.27 Adds Generic Methods, Struct Improvement & More SIMD

Go 1.27 brings some language enhancements including generic methods, a key in a struct literal may now be any valid field selector for the struct type, and function type inference has been generalized to apply in all assignment contexts. As for Go 1.27's support for generic methods:

Go 1.27 also brings a number of tooling improvements, size-specialized memory allocation has reduced small object allocation costs by up to 30%, and there are new standard library additions. The encoding/json/v2 provides high-level JSON processing with new options and stricter defaults. Meanwhile crypto/mldsa provides post-quantum ML-DSA signature support. There is also a uuid package addition for generating and parsing UUIDs.

Go 1.27 has a new experimental SIMD package for portable and vector-size-agnostic SIMD support The simd/archsimd for architecture-specific SIMD operations has revised its AMD64 API and added support for Arm Neon 128-bit SIMD and WebAssembly 128-bit SIMD.

More details on the Go 1.27 update can be found via the project site at go.dev.
