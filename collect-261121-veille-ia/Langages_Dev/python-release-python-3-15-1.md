---
id: collect-261121-veille-ia/langages-dev/python-release-python-3-15-1
title: "Python Release Python 3.15.1"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-10-21"]
keywords: ["python", "release", "changes", "install", "upgrade", "improvements", "more", "versions"]
source: https://www.python.org/downloads/release/python-3151/
source_anchor: ""
source_lines: [1, 125]
section: "Langages_Dev"
---
**Release date:** Oct. 21, 2026

## This is the first maintenance release of Python 3.15

Python 3.15.1 is an expedited security release and the first maintenance release of 3.15, containing around 330 bugfixes, build improvements and documentation changes from 117 contributors since 3.15.0.

## Security content in this release

This release upgrades the bundled pip to version 26.2.2, which includes certifi 2026.10.19 to address the eMudhra / emSign CA certificate security incident.

See the Discourse announcement for more information.

For all existing virtual environments on your system, please upgrade pip and uv to the latest versions.

To upgrade pip securely within a virtualenv:

```
python -m pip install "pip @ https://files.pythonhosted.org/packages/3f/06/3e4420302363fbb71be3c7f35ce042f54bfca330e3e66b3a2e3c4cca579f/pip-26.2.2-py3-none-any.whl#sha256=fd7f4dfa8190e014013b9f08f3e973f4e4c26ffd179a204895533a7254813a70"
```

If you cannot immediately upgrade to this release, you can upgrade certificates for a macOS installation and pip itself via:

```
/Library/Frameworks/Python.framework/Versions/3.15/bin/python3.15 -m pip install "pip @ https://files.pythonhosted.org/packages/3f/06/3e4420302363fbb71be3c7f35ce042f54bfca330e3e66b3a2e3c4cca579f/pip-26.2.2-py3-none-any.whl#sha256=fd7f4dfa8190e014013b9f08f3e973f4e4c26ffd179a204895533a7254813a70"
```

```
```
/Library/Frameworks/Python.framework/Versions/3.15/bin/python3.15 -m pip install --upgrade certifi

```
/Library/Frameworks/PythonT.framework/Versions/3.15/bin/python3.15t -m pip install "pip @ https://files.pythonhosted.org/packages/3f/06/3e4420302363fbb71be3c7f35ce042f54bfca330e3e66b3a2e3c4cca579f/pip-26.2.2-py3-none-any.whl#sha256=fd7f4dfa8190e014013b9f08f3e973f4e4c26ffd179a204895533a7254813a70"
```

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

### Incompatible changes, removals and new deprecations

- Incompatible changes
- Python removals and deprecations
- C API removals and deprecations
- Overview of all pending deprecations

## Python install manager

The installer we offer for Windows is being replaced by our new install manager, which can be installed from the Windows Store or from its download page. See our documentation for more information. The JSON file available for download contains the list of all the installable packages available as part of this release, including file URLs and hashes, but is not required to install the latest release. The traditional installer will remain available throughout the 3.14 and 3.15 releases.

## macOS 27.0 `IDLE` and `tkinter`

The macOS installer for this release is built with a pre-release of Tk, fixing a possible hang on macOS 27.0 in applications using `tkinter`, like `IDLE`, in older releases.

## More resources

- Online documentation
- PEP 790, 3.15 release schedule
- Report bugs at https://github.com/python/cpython/issues
- Help fund Python directly (or via GitHub Sponsors) and support the Python community

## Enjoy the new release

Thanks to all of the many volunteers who help make Python development and these releases possible! Please consider supporting our efforts by volunteering yourself or through organisation contributions to the Python Software Foundation.

### macOS

### Windows

### Source release

| Version | Operating system | Description | File size | Sigstore |  | SBOM | SHA-256 checksum | 
|---|---|---|---|---|---|---|---|
| Gzipped source tarball | Source release |  | 41.8 MB | .sigstore |  | SPDX | `133d85e118dc3262` | 
| XZ compressed source tarball | Source release |  | 34.1 MB | .sigstore |  | SPDX | `673ae7232d84c894` | 
| Android embeddable package (aarch64) | Android |  | 23.2 MB | .sigstore |  |  | `04e47e4b058845fa` | 
| Android embeddable package (x86_64) | Android |  | 23.5 MB | .sigstore |  |  | `0633782596c92524` | 
| iOS XCframework | iOS |  | 81.9 MB | .sigstore |  |  | `49db815c8e0c7243` | 
| macOS installer | macOS | for macOS 10.15 and later | 85.6 MB | .sigstore |  |  | `de6341e8a4386c82` | 
| Windows installer (64-bit) | Windows | Recommended | 43.4 MB | .sigstore |  | SPDX | `8b23839c6b2f3076` | 
| Windows installer (32-bit) | Windows |  | 41.8 MB | .sigstore |  | SPDX | `477dbb986b66a1ff` | 
| Windows installer (ARM64) | Windows |  | 42.7 MB | .sigstore |  | SPDX | `f335b7fabe85d9c1` | 
| Windows embeddable package (64-bit) | Windows |  | 13.1 MB | .sigstore |  | SPDX | `352e232f25b51205` | 
| Windows embeddable package (32-bit) | Windows |  | 11.4 MB | .sigstore |  | SPDX | `ef9ed8348e9b752c` | 
| Windows embeddable package (ARM64) | Windows |  | 12.4 MB | .sigstore |  | SPDX | `f652bba7398abbcc` | 
| Windows release manifest | Windows | Install with 'py install 3.15' | 15.0 KB | .sigstore |  |  | `b819787561df37ce` |
