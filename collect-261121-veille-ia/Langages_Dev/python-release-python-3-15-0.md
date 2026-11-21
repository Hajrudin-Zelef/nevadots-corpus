---
id: collect-261121-veille-ia/langages-dev/python-release-python-3-15-0
title: "Python Release Python 3.15.0"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["python", "release", "macos", "interpreter", "type", "idle", "features", "changes"]
source: https://www.python.org/downloads/release/python-3150/
source_anchor: ""
source_lines: [1, 115]
section: "Langages_Dev"
---
**Release date:** Oct. 9, 2026

## This is the stable release of Python 3.15.0

Python 3.15.0 is the newest major release of the Python programming language, and it contains many new features and optimisations compared to Python 3.14, in 5,643 commits from 1,012 contributors.

## Major new features of the 3.15 series, compared to 3.14

Some of the major new features and changes in Python 3.15 are:

### Interpreter improvements

- PEP 810: Explicit lazy imports for faster startup times
- PEP 814: Add `frozendict` built-in type
- PEP 661: Add `sentinel` built-in type
- PEP 798: Unpacking in comprehensions
- PEP 686: Python now uses UTF-8 as the default encoding
- PEP 829: Package startup configuration files
- The experimental JIT compiler has been significantly upgraded, with 7-8% geometric mean performance improvement on x86-64 Linux over the standard interpreter, and 11-12% speedup on AArch64 macOS over the tail-calling interpreter
- Improved error messages

### Significant improvements in the standard library

- PEP 799: Tachyon: High frequency statistical sampling profiler
- PEP 799: A dedicated profiling package for organizing Python profiling tools
- More color

### New typing features

- PEP 728: `TypedDict` with typed extra items
- PEP 747: Annotating type forms with `TypeForm`
- PEP 800: Disjoint bases in the type system

### C API improvements

- PEP 803, 820, 793: Stable ABI for free-threaded builds and related C API
- PEP 782: A new `PyBytesWriter` C API to create a Python bytes object
- PEP 788: Protection against finalization in the C API

### Build changes

- PEP 831: Frame pointers are enabled by default for improved system-level observability

### Release changes

- The official Windows 64-bit binaries now use the tail-calling interpreter
- The official macOS binaries now install free-threading support by default

For more details on the changes to Python 3.15, see What’s new in Python 3.15.

### Attention macOS 27.0 `IDLE` or `tkinter` users

When running IDLE or other GUI applications that use the `tkinter` module, these applications may hang when using an application's menu command that opens a dialog (for example, IDLE's `About IDLE`, `Settings`, and `Open Module` commands) resulting in a spinning beach ball with `Force Quit` needed.

This problem is due to an operating system behavior change in macOS 27.0 that is believed to affect all current versions of the Tk graphics toolkit and thus the `tkinter` module in all current Python versions.

If you depend on Tk-based applications (like IDLE) on macOS, you may want to consider deferring installing macOS 27.0 until a Tk or macOS workaround is available or testing that your application workflow is not affected. Follow issue #158053 for updates.

## More resources

- Online documentation
- PEP 790, 3.15 release schedule
- Report bugs at https://github.com/python/cpython/issues
- Help fund Python directly (or via GitHub Sponsors) and support the Python community

# And now for something completely different

To celebrate the new 3.15, Barry Warsaw has a present for us!

I had this idea to whip up a little TUI adventure game that takes you through a What’s New in Python 3.15. I present to you “whatsnewt”:

From the README:

A TUI text adventure through what’s new in Python 3.15.

You wake up in the Startup Foyer, somewhere inside the interpreter, and work your way to the Release Gate. Along the way there are eighteen puzzles, and every one of them is a real 3.15 feature you have to actually use. You’re not just answering boring questions, you’re actually writing and running code, in a Python 3.15 interpreter.

Some puzzles require a type checker, and in those cases, answers are verified by pyrefly, which is a dependency for exactly that reason, and the verdict quotes what it says back.

The easiest way to play it is to run:

`uvx --python 3.15 whatsnewt`

And yes, there are easter eggs


## Enjoy the new release

Thanks to all of the many volunteers who help make Python development and these releases possible! Please consider supporting our efforts by volunteering yourself or through organisation contributions to the Python Software Foundation.

Huge thanks to Georgi Ker and Marie Nordin for the Python 3.15 logo!

And also huge thanks to the Sovereign Tech Agency for supporting Hugo van Kemenade's work as release manager for Python 3.14 and 3.15 through the Sovereign Tech Fellowship.

### macOS

### Windows

### Source release

| Version | Operating system | Description | File size | Sigstore |  | SBOM | SHA-256 checksum | 
|---|---|---|---|---|---|---|---|
| Gzipped source tarball | Source release |  | 41.7 MB | .sigstore |  | SPDX | `438596cac081036d` | 
| XZ compressed source tarball | Source release |  | 34.0 MB | .sigstore |  | SPDX | `ba4bed1ba346b916` | 
| Android embeddable package (aarch64) | Android |  | 23.1 MB | .sigstore |  |  | `571e4cf4e0aee3d1` | 
| Android embeddable package (x86_64) | Android |  | 23.4 MB | .sigstore |  |  | `2413d51948d612cf` | 
| iOS XCframework | iOS |  | 80.6 MB | .sigstore |  |  | `68e98ab35cc0643b` | 
| macOS installer | macOS | for macOS 10.15 and later | 84.9 MB | .sigstore |  |  | `7c7e7eca61cbfa5f` | 
| Windows installer (64-bit) | Windows | Recommended | 43.3 MB | .sigstore |  | SPDX | `67d9a63eb145a34a` | 
| Windows installer (32-bit) | Windows |  | 41.6 MB | .sigstore |  | SPDX | `222f79df7d2d5653` | 
| Windows installer (ARM64) | Windows |  | 42.6 MB | .sigstore |  | SPDX | `bb1c5123cc63d7bb` | 
| Windows embeddable package (64-bit) | Windows |  | 13.1 MB | .sigstore |  | SPDX | `a6999bcbcbe56f93` | 
| Windows embeddable package (32-bit) | Windows |  | 11.4 MB | .sigstore |  | SPDX | `da1ca8f1a491444b` | 
| Windows embeddable package (ARM64) | Windows |  | 12.4 MB | .sigstore |  | SPDX | `abedaa8460572f9b` | 
| Windows release manifest | Windows | Install with 'py install 3.15' | 15.0 KB | .sigstore |  |  | `ebbd6a12b77ee2cb` |
