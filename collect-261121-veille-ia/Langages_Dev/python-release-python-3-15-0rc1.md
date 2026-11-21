---
id: collect-261121-veille-ia/langages-dev/python-release-python-3-15-0rc1
title: "Python Release Python 3.15.0rc1"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-09-01"]
keywords: ["python", "release", "changes", "candidate", "type", "first", "final", "projects"]
source: https://test.python.org/downloads/release/python-3150rc1/
source_anchor: ""
source_lines: [1, 90]
section: "Langages_Dev"
---
**Release date:** Aug. 4, 2026

It's the first 3.15 release candidate!

**This is a candidate preview of Python 3.15**

This release, **3.15.0rc1**, is the first of two planned candidate releases. Entering the release candidate phase, only reviewed code changes which are clear bug fixes are allowed between this release candidate and the final release.

The next release of Python 3.15 will be 3.15.0rc2, scheduled for 2026-09-01.

There will be ***no ABI changes*** from this point forward in the 3.15 series, and the goal is that there will be as few code changes as possible.

# Call to action

We ***strongly encourage*** maintainers of third-party Python projects to prepare their projects for 3.15 during this phase, and publish Python 3.15 wheels on PyPI to be ready for the final release of 3.15.0, and to help other projects do their own testing. Any binary wheels built against Python 3.15.0 release candidates ***will work*** with future versions of Python 3.15. As always, report any issues to the Python bug tracker.

Please keep in mind that this is a preview release and while it's as close to the final release as we can get it, its use is ***not*** recommended for production environments.

## Core team: time to work on documentation now

- Are all your changes properly documented?
- Are they mentioned in What's New?
- Did you notice other changes with insufficient documentation?

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
- The official macOS binaries now install free-threading support by default
- Improved error messages
- More color
- (Hey, **fellow core team member,** if a feature you find important is missing from this list, let Hugo know.)

For more details on the changes to Python 3.15, see What’s new in Python 3.15.

## More resources

- Online documentation
- PEP 790, 3.15 release schedule
- Report bugs at https://github.com/python/cpython/issues
- Help fund Python directly (or via GitHub Sponsors) and support the Python community

# And now for something completely different

Eurynome brought the seat at once and set a fleece upon it, and as soon as Ulysses had sat down Penelope began by saying, “Stranger, I shall first ask you who and whence are you? Tell me of your town and parents.”

“Madam,” answered Ulysses, “who on the face of the whole earth can dare to chide with you? Your fame reaches the firmament of heaven itself; you are like some blameless king, who upholds righteousness, as the monarch over a great and valiant nation: the earth yields its wheat and barley, the trees are loaded with fruit, the ewes bring forth lambs, and the sea abounds with fish by reason of his virtues, and his people do good deeds under him. Nevertheless, as I sit here in your house, ask me some other question and do not seek to know my race and family, or you will recall memories that will yet more increase my sorrow. I am full of heaviness, but I ought not to sit weeping and wailing in another person’s house, nor is it well to be thus grieving continually. I shall have one of the servants or even yourself complaining of me, and saying that my eyes swim with tears because I am heavy with wine.”


## Enjoy the new release

Thanks to all of the many volunteers who help make Python development and these releases possible! Please consider supporting our efforts by volunteering yourself or through organisation contributions to the Python Software Foundation.

### macOS

### Windows

### Source release

| Version | Operating system | Description | File size | Sigstore |  | SBOM | SHA-256 checksum | 
|---|---|---|---|---|---|---|---|
| Gzipped source tarball | Source release |  | 41.6 MB | .sigstore |  | SPDX | `32c2b9878bbd6ac3` | 
| XZ compressed source tarball | Source release |  | 33.9 MB | .sigstore |  | SPDX | `f84dad680ab21474` | 
| Android embeddable package (aarch64) | Android |  | 22.7 MB | .sigstore |  |  | `7d6fb28a69368841` | 
| Android embeddable package (x86_64) | Android |  | 23.0 MB | .sigstore |  |  | `f048f5c60b538efc` | 
| iOS XCframework | iOS |  | 80.2 MB | .sigstore |  |  | `178bf7bef9cd0f18` | 
| macOS installer | macOS | for macOS 10.15 and later | 88.3 MB | .sigstore |  |  | `57e0b1095f5dda52` | 
| Windows installer (64-bit) | Windows | Recommended | 43.1 MB | .sigstore |  | SPDX | `be7efa348927146e` | 
| Windows installer (32-bit) | Windows |  | 41.4 MB | .sigstore |  | SPDX | `faddd74cbcfd5b7d` | 
| Windows installer (ARM64) | Windows | Experimental | 42.4 MB | .sigstore |  | SPDX | `8d7be0dabfee4e04` | 
| Windows embeddable package (64-bit) | Windows |  | 13.1 MB | .sigstore |  | SPDX | `0bac7f0d3c6766ab` | 
| Windows embeddable package (32-bit) | Windows |  | 11.4 MB | .sigstore |  | SPDX | `57e40b629fdfd627` | 
| Windows embeddable package (ARM64) | Windows |  | 12.5 MB | .sigstore |  | SPDX | `f8d118bfafdf76ea` | 
| Windows release manifest | Windows | Install with 'py install 3.15' | 15.2 KB | .sigstore |  |  | `87b359b95dcb5f71` |
