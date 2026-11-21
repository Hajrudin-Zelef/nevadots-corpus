---
id: collect-261121-veille-ia/langages-dev/go-s-improving-simd-support-platform-independent-simd-interface
title: "Go's Improving SIMD Support, Platform-Independent SIMD Interface"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["simd", "support", "interface", "improving", "platform-independent", "also", "more", "there"]
source: https://www.phoronix.com/news/Go-SIMD-2026
source_anchor: ""
source_lines: [1, 11]
section: "Langages_Dev"
---
# Go's Improving SIMD Support, Platform-Independent SIMD Interface

Since Go 1.26 there has been experimental APIs for leveraging SIMD with modern CPUs. With Go 1.26 it initially target modern x86_64 capabilities like AVX while Go 1.27 expanded the support to the ARM64 world with NEON.

The Go language's SIMD package currently supports AVX, AVX2, AVX-512, Arm NEON, and WASM SIMD instructions.

Due to not only the varying implementations across CPU architectures but also the varying fixed-size vectors and behavioral differences there, Go 1.27 also introduced a fully portable, platform and size-agnostic SIMD interface they modeled around Google's Highway library.

With Go 1.28, developers are planning on adding Arm SVE support to the SIMD code. Plus more SIMD operations being supported by their interface is also planned.

Those wanting to learn more about this SIMD support in the Go programming language can learn more via the new post on the Go.dev blog.
