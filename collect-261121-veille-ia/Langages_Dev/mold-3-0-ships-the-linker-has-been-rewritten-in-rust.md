---
id: collect-261121-veille-ia/langages-dev/mold-3-0-ships-the-linker-has-been-rewritten-in-rust
title: "Mold 3.0 Ships: The Linker Has Been Rewritten in Rust"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["rust", "mold", "linker", "rewrite", "project", "developers", "systems", "files"]
source: https://freedium-mirror.cfd/https://medium.com/@thyzmile24/mold-3-0-ships-the-linker-has-been-rewritten-in-rust-7478f5574a84
source_anchor: ""
source_lines: [1, 304]
section: "Langages_Dev"
---
#### A complete C++-to-Rust rewrite brings a new chapter for one of Linux's fastest linkers â but compatibility, tooling, and ecosystem adoption are the real tests.

When developers talk about Rust replacing C++, the conversation usually focuses on memory safety, performance, and the difficulty of maintaining large systems.

But there is a more interesting way to evaluate that debate.

Take an established systems tool that developers already depend on. Rewrite its implementation in Rust. Then ask a difficult question:

**Can you change the language underneath without forcing users to change how they work?**

That is the challenge behind Mold 3.0.0.

Released on October 6, 2026, Mold 3.0 marks the first release of the linker rewritten entirely from C++ to Rust. It follows Mold 2.42.1, which was announced as the likely final C++ release. According to the project, the new version preserves the existing command-line interface, supports the same target architectures, and delivers comparable linking performance. The project also reports that its compatibility testing found no regressions when building Gentoo packages. (Mold 3.0.0 release notes)

This is not simply another Rust project appearing on GitHub.

Mold is part of the software infrastructure developers use to turn compiled code into executable programs.

And its rewrite raises an important question for the wider systems programming community:

**Can Rust make foundational infrastructure safer without sacrificing the performance and compatibility that users already expect?**

### First, what does a linker actually do?

Before looking at the rewrite, it helps to understand the problem Mold solves.

When you compile a program written in Rust, C, or C++, the compiler typically translates source code into object files. Those files contain machine code and information about functions, symbols, and references to other parts of the program.

A linker takes those pieces and combines them into an executable or shared library.

For example, a large application might depend on hundreds of libraries. The linker needs to resolve references between those components and produce an output that the operating system can load.

The process looks simple from the outside:

1. Compile the source files.
2. Link the resulting object files.
3. Produce an executable.

But linking can become expensive in large projects.

A build may involve thousands of object files, large dependency graphs, debug information, and complicated symbol resolution. Even if compilation is fast, linking can become a bottleneck during repeated development builds.

That is why tools such as GNU `ld`, LLVM's `lld`, and Mold matter.

Mold was designed as a high-performance alternative to traditional Unix linkers, with a strong emphasis on parallelism and efficient algorithms. Its purpose is to reduce the time developers spend waiting for executables to be linked. (Mold project documentation)

### Why rewrite a working C++ linker in Rust?

Rewriting established software is risky.

Mold already had users, an existing command-line interface, supported architectures, and years of development behind it. A rewrite could introduce regressions, change output behavior, or make the project more difficult to build.

So why do it?

The project maintainer's explanation is closely tied to the expected lifetime of systems software.

A linker is foundational infrastructure. It may remain in use for many years, and its implementation needs to be maintainable over that period.

Rust offers a different set of engineering trade-offs from C++.

Its ownership model, borrowing rules, and memory-safety checks help prevent many classes of memory errors that can occur in C and C++ programs. Rust also supports low-level systems programming without requiring developers to abandon native performance as a design goal.

That does not mean Rust automatically makes every program faster or eliminates every security vulnerability.

It means the language can help developers rule out certain dangerous behaviors while building performance-sensitive software.

For a project such as Mold, that is a meaningful reason to consider a rewrite.

The goal is not simply to replace C++ syntax with Rust syntax.

It is to preserve the tool's established behavior while changing the engineering foundation underneath it.

### Mold 3.0 keeps the familiar interface

One of the most important details in the release is what users are *not* required to change.

Mold 3.0 is intended to be a drop-in replacement for the previous C++ version.

According to the release notes, it retains the same command-line options and supported target architectures, with comparable linking performance. The project reports testing the supported targets, comparing output across real-world workloads and option combinations, and building Gentoo packages to look for regressions. (Release notes)

Why does that matter?

Imagine a development team already using Mold in its build pipeline.

The team may have configured its compiler, CI jobs, build scripts, and local development environments around the existing linker.

A rewrite that requires everyone to change their workflow creates a migration problem.

A compatible rewrite has a different proposition:

The implementation changes, but the tool should continue to fit into the existing workflow.

That is particularly important for infrastructure software. Developers rarely want to spend time adapting their applications to a linker when their actual goal is to build and ship those applications.

Compatibility is therefore not a minor release detail.

It is one of the main requirements for a successful rewrite.

### Performance remains part of the contract

Rust's memory-safety story is important, but Mold cannot succeed on safety alone.

Developers use Mold because linking performance matters.

If the Rust rewrite introduced a major performance regression, users would have a practical reason to remain on the older implementation or switch to another linker.

The Mold 3.0 release notes report that linking performance remains on par with version 2.42.1.

That is an important result, but it should be interpreted carefully.

It does not mean Rust is inherently faster than C++, nor does it prove that every workload will produce identical timings. Actual results can depend on the target architecture, input files, linker options, hardware, and build configuration.

The more meaningful achievement is that the project reports preserving its performance characteristics while changing the implementation language.

**A successful systems-language rewrite should preserve the properties users rely on, not merely demonstrate that the new language can compile the code.**

For developers evaluating Rust for existing infrastructure, that is a more useful lesson than a broad claim that one language is always faster than another.

### Memory safety matters even in a linker

A linker processes complex input files generated by compilers, build tools, and other parts of the software toolchain.

Those files can contain unexpected or corrupted data.

According to the Mold 3.0 release notes, the previous C++ implementation could perform out-of-bounds reads when processing certain corrupted inputs, potentially leading to a segmentation fault.

In the Rust version, the relevant accesses are bounds-checked, so the corresponding failure becomes a panic rather than the same kind of out-of-bounds memory access. (Mold 3.0.0 release notes)

That is a meaningful change.

However, a panic is not the same as graceful error handling. It can still terminate the process, and memory safety does not guarantee that every malformed input is handled correctly.

The important distinction is that the rewrite can prevent a particular class of unsafe memory access rather than merely hoping every input follows the expected path.

This matters because development tools are part of the software supply chain.

Compilers, assemblers, linkers, package managers, and build systems operate throughout the process of creating software. Weaknesses in those tools can affect more than one application.

Making foundational tools safer is therefore valuable even when the tools are not directly exposed to end users.

### The rewrite changes how Mold is built

For developers who simply use Mold, the transition is designed to be largely transparent.

For distribution maintainers and people who build Mold from source, the changes are more significant.

The project has replaced CMake with Cargo as its build system.

The new build requirements include:

- Rust 1.95 or later.
- A C compiler.
- Cargo for building the project.
- Updated installation procedures and configuration.

The Intel oneTBB dependency has also been removed. Some build and installation details have changed, including how `MOLD_LIBDIR` is configured when Mold's libraries are installed outside the usual library directory. (Mold 3.0.0 release notes)

A basic source build now uses Cargo:

```
git clone https://github.com/rui314/mold.git
cd mold
cargo build --release
```
The resulting build artifacts include the `mold` executable and its companion `mold-wrapper.so` library under `target/release`.

For installation instructions and platform-specific requirements, developers should consult the project's official documentation rather than assuming older CMake instructions still apply.

This change illustrates an important distinction: **rewriting a program changes more than its source code.**

It can affect the build environment, dependency management, packaging rules, testing commands, and the experience of downstream maintainers.

A successful migration needs to account for the entire development and distribution process.

### Architecture support is part of the challenge

A linker cannot be judged only by whether it works on one developer's machine.

It needs to support the architectures and environments its users depend on.

Mold 3.0 includes architecture-specific fixes affecting targets such as:

- AArch64 and ARM32
- RISC-V
- LoongArch
- PowerPC 32-bit and 64-bit
- SH-4
- SPARC64

These details may not attract the same attention as the language rewrite, but they are essential to the project's real-world usefulness.

Systems software often operates across different processors, operating systems, toolchains, and distribution configurations. A change that works on one target can still introduce problems elsewhere.

That is why cross-target testing matters.

It also explains why a large-scale rewrite needs more than a successful compilation on the maintainer's computer.

The new implementation must preserve the behavior that downstream projects depend on across a wide range of environments.

### The next challenge is compatibility with GNU ld

Mold 3.0 is an important milestone, but it does not mean the project has finished its work.

One of the project's longer-term goals is to make Mold a viable default linker on Linux distributions.

The existing GNU linker remains deeply integrated into Linux development environments. Replacing it requires more than comparable performance on common workloads.

A default system tool needs to handle the behavior that existing build systems and software packages expect.

One important remaining area is linker script support.

Linker scripts describe how input sections and symbols should be organized in an output file. They can be essential to operating-system components, specialized binaries, and other software with particular layout requirements.

Supporting the options and workflows developers already use is part of what makes a linker a practical replacement rather than simply a faster alternative for selected projects.

Mold's stated direction for the 3.x series is to close compatibility gaps, particularly around linker scripts, and move closer to becoming a default Linux linker. (Mold release notes)

That goal is ambitious.

The project will need to balance performance, compatibility, maintainability, and the expectations of Linux distributions.

And because linkers sit so close to the foundation of software builds, regressions can affect many downstream projects at once.

The challenge is not just making Mold work.

It is making it dependable enough that other projects can rely on it by default.

### What this means for Rust developers

Mold 3.0 offers several lessons for developers considering Rust beyond new applications.

### 1. Rust is becoming a practical option for established infrastructure

Rust is no longer interesting only because developers can use it to start new command-line tools or web services.

The Mold rewrite demonstrates its use in an existing, performance-sensitive systems project.

That is a different test from building a small application from scratch.

### 2. Memory safety is valuable, but it is not the whole story

The language can help prevent certain memory errors, but the project still needs correct algorithms, robust input handling, comprehensive testing, and careful release management.

Safety is a property of the engineering process as well as the programming language.

### 3. Compatibility can matter more than the rewrite itself

Users do not necessarily care which language implements a tool.

They care whether it works, whether it is fast enough, and whether they can continue using it without unexpected changes.

Preserving the interface makes the benefits of a new implementation easier to adopt.

### 4. Build and packaging experience matters

Changing from CMake to Cargo makes sense for a Rust project, but downstream maintainers still need to adapt.

A language migration is more successful when the surrounding tooling and documentation are treated as part of the product.

### 5. Infrastructure adoption takes time

Even if Mold offers strong performance and a safer implementation, becoming a default system linker requires broad compatibility and trust.

The project must demonstrate that it can support the diversity of real Linux workloads, not just a carefully selected benchmark.

### Does this mean Rust will replace C++?

Not by itself.

Mold 3.0 is one concrete example of a project choosing Rust for a performance-sensitive systems tool. It shows that a rewrite can preserve the existing interface and reported performance while changing the implementation language.

But one project cannot establish that every C++ codebase should be rewritten.

A migration still needs a clear justification. Existing C++ software may be stable, well-maintained, deeply integrated with platform libraries, or too expensive to replace.

Rust is especially worth evaluating when memory safety, long-term maintainability, and the ability to incrementally improve a codebase are important priorities.

Even then, the best decision depends on the project.

The lesson is not that C++ has become useless.

It is that Rust has matured into a credible option for more kinds of systems software, including tools whose correctness and performance affect developers every day.

### The bigger lesson behind Mold 3.0

The headline is easy to understand: a popular linker has been rewritten from C++ to Rust.

But the more interesting story is what the project is trying to preserve.

Mold needs to remain fast. It needs to support the same architectures. It needs to work with established build systems. And it needs to earn the confidence of the developers and Linux distributions that depend on it.

The Rust rewrite is only one part of that work.

Mold 3.0 is a meaningful milestone because it moves the project onto a new implementation foundation while aiming to preserve the behavior that made the tool useful in the first place.

The next stage will be about proving that this foundation can support broader compatibility and long-term adoption.

If Mold succeeds, its significance will extend beyond one linker.

It will offer another example of how Rust can be used to modernize systems infrastructure without requiring users to abandon the workflows they already trust.

**The most successful language rewrite is not the one that attracts the most attention. It is the one that makes the software safer to maintain while allowing users to keep doing their work.**

And that is the real test Mold 3.0 now faces.
