---
id: collect-261121-veille-ia/langages-dev/mold-3-0-high-speed-linker-rewritten-in-rust
title: "mold 3.0 High-Speed Linker Rewritten in Rust"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["mold", "rust", "linker", "version", "output", "high-speed", "project", "release"]
source: https://linuxiac.com/mold-3-0-high-speed-linker-rewritten-in-rust/
source_anchor: ""
source_lines: [1, 21]
section: "Langages_Dev"
---
The high-speed mold linker, a high-performance drop-in replacement for existing Unix linkers designed to speed up builds, has reached version 3.0, marking a major milestone as the project completes its transition from C++ to Rust. The project is developed by Rui Ueyama, the original author of LLVM’s lld linker.

According to the developers, mold 3.0 is the first release based on the new Rust implementation, while version 2.42.1 remains the final release built from the old C++ codebase.

Mold 3.0 accepts the same command-line options, supports the same target architectures, and should produce the same output except for the bug fixes included in the release. The developers also say linking performance remains on par with the previous C++ version.

The move to Rust also improves how mold handles corrupted input files. In the previous C++ implementation, invalid data could cause out-of-bounds memory reads and segmentation faults. In the Rust version, those accesses are bounds-checked, causing mold to stop at the faulty operation.

Importantly, there are significant changes for users who build mold from source. The project moved from CMake to Cargo, requiring Rust 1.95 or newer alongside a C compiler. The former CMake configuration options are gone, and target selection is now handled through Cargo features.

At the same time, mold has dropped its dependency on oneTBB. It continues to statically link mimalloc by default, although users can choose the system allocator when building the linker.

Beyond the Rust rewrite, mold 3.0 includes many compatibility improvements and bug fixes. These include fixes for crashes involving statically linked executables and version scripts, corrupted output when an output file is also used as an input file, several nondeterministic output cases, and multiple issues affecting relocatable linking with the -r option.

Compatibility with GNU ld has improved in several areas, including command-line option parsing, symbol version handling, and relocation behavior. Some cases that previously produced broken output or crashes are now rejected with proper errors.

Finally, architecture-specific fixes cover AArch64, ARM32, i386, RISC-V, LoongArch, PPC32, PPC64, m68k, SH4, and SPARC64. The release also adds support for additional relocation types on AArch64 and ARM32 and resolves several relocation and TLS-related issues on RISC-V and LoongArch.

Looking ahead, the mold developers say the broader goal of the 3.x series is to close the remaining compatibility gaps with GNU ld, particularly around linker scripts. Ultimately, the project hopes that this work will help pave the way for mold to be adopted as the default linker by Linux distributions.

More details about mold 3.0 are available in the release’s changelog.
