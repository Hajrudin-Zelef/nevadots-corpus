---
id: collect-261121-veille-ia/linux-kernel/developers-released-mold-3-0-linker-wisevoter
title: "Developers Released Mold 3.0 Linker | Wisevoter"
domain: linux-kernel
role: reference
task: article
actors: []
dates: ["2026-10-05"]
keywords: ["mold", "linker", "developers", "rust", "software", "released", "build", "programming"]
source: https://wisevoter.com/world/2026/10/05/developers-released-mold-3-0
source_anchor: ""
source_lines: [1, 53]
section: "Linux_Kernel"
---
# Developers Released Mold 3.0 Linker

The latest version of the high-speed linker has transitioned from C++ to the Rust programming language.

Updated on Oct. 5, 2026 in Software

Live Poll

Do you trust that rewriting software in newer programming languages makes systems safer and more reliable?

Developers have released Mold 3.0, a significant update that rewrites the software from C++ to Rust. This new iteration succeeds version 2.42.1, which serves as the final C++ release for the project.

## Why it matters

The transition to Rust aims to improve memory safety when processing potentially corrupted input files. Developers expect these changes to help close compatibility gaps with GNU LD and facilitate the software's adoption as a default Linux linker.

Mold 3.0 introduces a fundamental shift by moving from a C++ codebase to Rust 1.95 and replacing the CMake build system with Cargo. Additionally, the update removes the previous dependency on Intel oneTBB.

## The players

Mold

Mold is a high-performance linker designed to speed up the software compilation process.

GNU LD

GNU LD is the standard linker used in most Linux-based operating systems and development environments.

## The details

The rewrite ensures that the linker can handle complex build operations more securely than previous versions. By standardizing on Cargo, the project aims to simplify the compilation process for developers integrating the tool into their environments.

## Timeline

1. Mold 3.0 was officially released on October 5, 2026.

## The Tech Race

This migration follows the broader industry shift toward integrating the Rust programming language memory safety model to replace aging C++ infrastructure. By moving away from legacy architectures, the project positions itself to better compete with established tools like GNU LD.

Developers and system administrators must now ensure their build environments support Rust 1.95 to utilize this version. The transition to the Cargo build system also requires teams to update their existing configuration workflows.

## The takeaway

Transitioning foundational tools to memory-safe languages like Rust is becoming a standard practice for open-source projects aiming for long-term stability. Users should prioritize updating their build pipelines to Cargo to maintain compatibility with future Mold 3.x iterations.

## Further reading

Learn more about the latest development tools in our Software section.

Live Poll

Do you trust that rewriting software in newer programming languages makes systems safer and more reliable?
