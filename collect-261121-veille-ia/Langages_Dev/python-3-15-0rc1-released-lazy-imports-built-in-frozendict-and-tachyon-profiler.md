---
id: collect-261121-veille-ia/langages-dev/python-3-15-0rc1-released-lazy-imports-built-in-frozendict-and-tachyon-profiler
title: "Python 3.15.0rc1 Released: Lazy Imports, Built-in frozendict, and Tachyon Profiler"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-09-01"]
keywords: ["lazy", "imports", "frozendict", "python", "tachyon", "profiler", "import", "module"]
source: https://www.linuxcompatible.org/story/python-3150rc1-released-lazy-imports-builtin-frozendict-and-tachyon-profiler/
source_anchor: ""
source_lines: [1, 77]
section: "Langages_Dev"
---
### Python 3.15.0rc1 ships with lazy imports, frozendict, and a new statistical profiler

**The first release candidate for Python 3.15 is out. Announced today by the release team led by Hugo van Kemenade, this marks the start of a two-RC phase. rc2 lands September 1. The final release is scheduled for October 2026, per PEP 790.**

From here on, only reviewed bug fixes are allowed. No ABI changes. Minimal code changes expected.

It's a rather significant feature drop, though lazy imports will be the one drawing the most attention from framework authors and performance junkies. Ned Deily, Steve Dower, Łukasz Langa, and the rest of the team have packed PEP 810, PEP 814, PEP 799, and JIT speedups into this candidate.

#### PEP 810: lazy keyword for deferred imports

Write lazy import json, and Python creates a types.LazyImportType proxy. The real module loads on first access, making the proxy indistinguishable from an eager import thereafter.

Benchmarks from Meta and Hudson River Trading show startup time improvements of 50–70% and memory savings of 30–40% on large codebases.

Keep in mind that lazy is only permitted at module scope. You can't sneak it into a function, class body, or try/except block. Star imports (lazy from x import *) and from __future__ imports aren't allowed.

The bytecode side sets a flag on IMPORT_NAME. The adaptive interpreter specializes reification via LOAD_GLOBAL / LOAD_NAME to LOAD_GLOBAL_MODULE after about two or three accesses. Next, the overhead is near zero. Benchmarks show 0.1–0.5% variance versus baseline when a filter is present.

Global control is available via -X lazy_imports=all or the PYTHON_LAZY_IMPORTS=all environment variable. You can also use sys.set_lazy_imports("all") or sys.get_lazy_imports() for runtime control. sys.set_lazy_imports_filter(func) accepts a callable (importer, name, fromlist) -> bool for fine-grained decisions, like making only your own app modules lazy while keeping third-party dependencies eager.

A module can declare __lazy_modules__ = ["json"] to treat matching imports as lazy. Older Python versions ignore it, which provides a bridge for cross-version compatibility.

#### PEP 814: Built-in frozendict

PEP 814 adds frozendict directly to builtins. It's not a dict subclass; it inherits from object. That matters if you were relying on isinstance(x, dict).

The type is hashable when all keys and values are hashable. It preserves insertion order but compares equal regardless of order. frozendict(y=2, x=1) == frozendict(x=1, y=2) returns True.

The standard library has been updated to accept frozendict in copy, decimal, json, marshal, plistlib, pickle, pprint, and xml.etree.ElementTree. eval() and exec() will also accept it for globals. type() and str.maketrans() accept it for dict arguments.

#### PEP 661: sentinel type

Creating unique sentinel values just got a built-in. PEP 661 adds a sentinel type with proper string representation. Sentinels preserve identity when copied, support the | operator in type expressions, and can be pickled as long as you can import them by module and name. Jelle Zijlstra contributed the code; Tal Einat wrote the PEP.

#### PEP 799: profiling package and Tachyon sampler

Profiling gets a major overhaul. PEP 799 introduces the profiling package. profiling.tracing handles deterministic function-call tracing. cProfile remains as a backwards-compatible alias. profiling.sampling brings Tachyon, a high-frequency statistical sampler hitting up to 1,000,000 Hz.

Tachyon supports multiple modes. Use --mode wall for real elapsed time, --mode cpu for active CPU execution, --mode gil to see which threads dominate GIL usage, or --mode exception for exception handling time. It can attach to running processes by PID, run scripts directly, or capture one-shot snapshots for hung processes.

Output formats include pstats tables, collapsed stacks for flame graphs, interactive HTML flame graphs via D3.js, Firefox Profiler format, per-file heatmaps, and a live TUI with --live. Async-aware profiling is supported with task-based stack reconstruction.

The old profile module is now deprecated and will be removed in Python 3.17.

#### JIT Compiler and ABI Updates

The JIT compiler picked up significant speed. Benchmarks show an 8–9% geometric mean speedup on x86-64 Linux and 12–13% on AArch64 macOS over the standard interpreter. Official Windows 64-bit binaries now use the tail-calling interpreter by default. Official macOS binaries install free-threading support by default.

Free-threaded builds now support the abi3t stable ABI. A new PyModExport_* export hook and PySlot structure enable C extensions to target this ABI. A migration guide is available.

PEP 831 enables frame pointers by default via -fno-omit-frame-pointer on supported platforms. This benefits profilers, debuggers, crash analyzers, and eBPF observability tools without requiring users to rebuild Python.

#### Other Notable Changes

PEP 798 allows unpacking in comprehensions. [*L for L in lists] flattens iterables. {*s for s in sets} merges sets. {**d for d in dicts} merges dicts. Async generators support (*a async for a in agen()). This is a cleaner alternative to nested comprehensions or itertools.chain().

PEP 686 makes UTF-8 the default encoding for I/O operations regardless of system locale. encoding='locale' still works. You can revert with PYTHONUTF8=0 or -X utf8=0. The opt-in warning can identify affected code.

PEP 829 introduces .start files for package startup configuration. .pth files can now declare entry points as pkg.mod:callable. This replaces the opaque practice of embedding arbitrary code via import ... lines, which are now silently deprecated. site.StartupState batches path extensions before executing any startup code.

PEP 728 adds extra_items=SomeType to TypedDict for dynamic key shapes. The type system gains a TypeForm protocol (PEP 747) and disjoint bases (PEP 800).

The C API gets a new PyBytesWriter (PEP 782) for efficient bytes creation. PEP 788 adds interpreter guards to prevent deadlocks during finalization, soft-deprecating the legacy PyGILState_* family.

Other additions include math.integer for integer-specific math, bytearray.take_bytes() to extract bytes without copying, and regex filtering for -W warning filters. The import deadlock bug affecting sibling submodules is fixed. Error messages now suggest .inner.attr_name for nested attributes and cross-language method equivalents like [].push() → .append(). Help output, tab completions, and several other tools now render with color.

#### Removed and Deprecated

The old profile module is deprecated. .pth files can no longer use import ... lines. Several items from ast, typing, pathlib, platform, glob, http.server, importlib.resources, sre_*, sysconfig, threading, types, wave, and zipimport have been removed.

#### Timeline

Python 3.15.0rc1 is available now. Python 3.15.0rc2 is scheduled for September 1, 2026. The final Python 3.15.0 release is expected in October 2026.

The release team strongly encourages third-party maintainers to test against 3.15rc1 and publish Python 3.15 wheels on PyPI. Binary wheels built against RCs will work with the final release, since the ABI is stable from this point.

Head here to the Python Insider post for the full changelog and benchmark details.
