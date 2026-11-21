---
id: collect-261121-veille-ia/langages-dev/python-3-15-0-beta-3-is-here-python-insider
title: "Python 3.15.0 beta 3 is here! | Python Insider"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["python", "beta", "release", "changes", "possible", "features", "phase", "type"]
source: https://blog.python.org/2026/06/python-3150-beta-3/
source_anchor: ""
source_lines: [1, 75]
section: "Langages_Dev"
---
**This is a beta preview of Python 3.15**

Python 3.15 is still in development. This release, 3.15.0b3, is the third of four planned beta releases, containing around 195 bugfixes, build improvements and documentation changes from 86 contributors since 3.15.0b2.

Beta release previews are intended to give the wider community the opportunity to test new features and bug fixes and to prepare their projects to support the new feature release.

We ***strongly encourage*** maintainers of third-party Python projects to ***test with 3.15*** during the beta phase and report issues found to the Python bug tracker as soon as possible. While the release is planned to be feature-complete entering the beta phase, it is possible that features may be modified or, in rare cases, removed up until the start of the release candidate phase (2026-08-04). Our goal is to have ***no ABI changes*** after beta 4 and as few code changes as possible after the first release candidate. To achieve that, it will be ***extremely important*** to get as much exposure for 3.15 as possible during the beta phase.

This includes creating pre-release wheels for 3.15, as it helps other projects to do their own testing. However, we recommend that your regular production releases wait until 3.15.0rc1, to avoid the risk of ABI breaks.

Please keep in mind that this is a preview release and its use is ***not*** recommended for production environments.

## Major new features of the 3.15 series, compared to 3.14

Some of the major new features and changes in Python 3.15 are:

- PEP 810: Explicit lazy imports for faster startup times
- PEP 814: Add `frozendict` built-in type
- PEP 661: Add `sentinel` built-in type
- PEP 799: A dedicated profiling package for organizing Python profiling tools
- PEP 799: Tachyon: High frequency statistical sampling profiler
- PEP 831: Frame pointers are enabled by default for improved system-level observability
- PEP 798: Unpacking in comprehensions
- PEP 686: Python now uses UTF-8 as the default encoding
- PEP 829: Package startup configuration files
- PEP 728: `TypedDict` with typed extra items
- PEP 747: Annotating type forms with `TypeForm`
- PEP 800: Disjoint bases in the type system
- PEP 782: A new `PyBytesWriter` C API to create a Python bytes object
- PEP 803, 820, 793: Stable ABI for free-threaded builds and related C API
- The JIT compiler has been significantly upgraded, with 8-9% geometric mean performance improvement on x86-64 Linux over the standard interpreter, and 12-13% speedup on AArch64 macOS over the tail-calling interpreter
- The official Windows 64-bit binaries now use the tail-calling interpreter
- Improved error messages
- More color
- (Hey, **fellow core team member,** if a feature you find important is missing from this list, let Hugo know.)

For more details on the changes to Python 3.15, see What’s new in Python 3.15. The next pre-release of Python 3.15 will be 3.15.0b4, scheduled for 2026-07-18.

## Removals and new deprecations

- Python removals and deprecations
- C API removals and deprecations
- Overview of all pending deprecations

## More resources

- Online documentation
- PEP 790, 3.15 release schedule
- Report bugs at https://github.com/python/cpython/issues
- Help fund Python directly (or via GitHub Sponsors) and support the Python community

## And now for something completely different

“It’s a friend of mine—a Cheshire Cat,” said Alice: “allow me to introduce it.”

“I don’t like the look of it at all,” said the King: “however, it may kiss my hand if it likes.”

“I’d rather not,” the Cat remarked.


## Enjoy the new release

Thanks to all of the many volunteers who help make Python development and these releases possible! Please consider supporting our efforts by volunteering yourself or through organisation contributions to the Python Software Foundation.

Regards from a sunny Helsinki with a 3:54 sunrise and 22:50 sunset,

Your release team,

Hugo van Kemenade

Ned Deily

Steve Dower

Łukasz Langa
