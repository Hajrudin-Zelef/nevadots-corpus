---
id: collect-261121-veille-ia/langages-dev/go-language-1-27-released-with-generic-methods-json-v2-simd-support
title: "Go Language 1.27 Released with Generic Methods, JSON v2, SIMD Support"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["support", "package", "language", "version", "generic", "json", "simd", "released"]
source: https://ubuntuhandbook.org/index.php/2026/08/go-language-1-27-released-with-generic-methods-json-v2-simd-support/
source_anchor: ""
source_lines: [1, 41]
section: "Langages_Dev"
---
Go language (aka Golang) released new major 1.27 version in last week, six months after the last 1.26.

The new version of this popular programming language added support **generic methods**, which can now declare their own type parameters, making generics more flexible.


As an example, Rand now supports a generic method N, matching the behavior of the top-level N function.

func (r *Rand) N[Int intType](n Int) Int

N returns a pseudo-random number in the half-open interval [0,n). The type parameter Int can be any integer type. It panics if n <= 0.

The new 1.27 version also expanded the standard library with many new packages.

It now has a new `encoding/json/v2` package with stricter defaults and significantly faster unmarshaling. While, the existing `encoding/json` API remains compatible and is now backed by v2 internally.

There’s also new `crypto/mldsa` package that implements ML-DSA (FIPS 204), with ML-DSA support added to X.509 and TLS 1.3. TLS also adds ML-KEM-1024.

It as well added new `uuid` package for generating and parsing UUIDs, and, experimental `simd` package plus expanded `simd/archsimd` support for AMD64, ARM64 NEON, and WebAssembly.

Go 1.27 also features many toolchain updates.

Compiler tools now support `@file` response files, compatible with GCC’s format. `go test` runs the `stdversion` vet check by default, helping detect use of APIs newer than the module’s declared Go version. And, `go doc` gains `package@version` and executable-example support.

For macOS, Go 1.27 now requires macOS 13 Ventura or newer. While, the big-endian 64-bit PowerPC port in Linux now uses the ELFv2 ABI, with cgo, PIE, and external linking supported.

Other changes include:

- Specialized allocation routines can reduce the cost of some small allocations by up to 30%.
- `goroutineleak` is now a generally available.
- `go tool trace -http=:port` now listens on localhost only by default.
- Direct fetching from Bazaar repositories is removed.
- `time` channels now always synchronous/unbuffered.
- The unicode package support updated from v15 to 17.

For more about Go 1.27, see the official release note.

### How to Install Golang 1.27

The packages for Linux, macOS, and Windows, as well as the source tarball is available to download in go website via the link below:

For Linux, select download the tarball for your CPU architecture type, then extract. For beginners, see this step by step guide for how to set up the environment variables.
