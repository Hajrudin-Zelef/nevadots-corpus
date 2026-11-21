---
id: collect-261121-veille-ia/langages-dev/python-3-15-new-features-updates
title: "Python 3.15: New Features & Updates"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["python", "lazy", "release", "import", "features", "october", "teams", "imports"]
source: https://www.codetrade.io/blog/python-3-15/
source_anchor: ""
source_lines: [1, 179]
section: "Langages_Dev"
---
### Summarize with AI

# What makes Python 3.15 different? New features explained

## Key takeaways:

- Python 3.15.0 shipped on October 9, 2026, one week after its planned date, because a third release candidate fixed late lazy import bugs.
- Among the Python 3.15 features, lazy imports, frozendict, sentinel, comprehension unpacking, and UTF-8 by default are the ones most teams will notice first.
- The experimental JIT compiler is now 7 to 8 percent faster on average on x86-64 Linux, although results vary widely by workload.
- UTF-8 by default is the biggest compatibility risk for older code that opens files without naming an encoding.
- Most 3.14 applications can move to 3.15 after a dependency check, a test run with warnings enabled, and an encoding audit.

## Introduction

For teams tracking the Python latest version, the answer is now 3.15, released by the Python core team on October 9, 2026. According to the official release announcement, it contains 5,643 commits from 1,012 contributors. As a result, the release touches almost every layer of the language, from syntax and built-in types to the interpreter and the C API.

For engineering teams, the practical impact falls into four areas: how fast applications start, how text files are read, how performance is measured, and how native extensions are built. This Python 3.15 developer guide explains each major change, shows short code examples, and closes with an upgrade plan that teams can follow step by step.

## Python 3.15 release date and support timeline

PEP 790 originally scheduled the final release for October 1, 2026.

However, release manager Hugo van Kemenade published a third release candidate on October 2 to fix last-minute lazy import blockers, which moved the final release to October 9. The first bugfix update, 3.15.1, followed on October 21, according to the Python documentation archive.

Here is what the timeline means for planning:

- **Feature freeze:** No new features were added after beta 1 on May 7, 2026, so the feature set has been stable for months.
- **Wheel compatibility:** Binary wheels built against the release candidates work with every 3.15 release, so many libraries already ship support.
- **Bugfix window:** Bugfix releases arrive roughly every two months for about two years, followed by security-only updates.
- **Older versions:** Python 3.10 reached end of life in October 2026, so teams still on it now run an unsupported interpreter.

Meanwhile, Python programming language updates now reach a large share of the developer population. In the Stack Overflow Developer Survey 2026, 58.0% of respondents reported using Python, and it ranked as the most desired language at 33.4%.

## Python 3.15 new features explained

### 1. Explicit lazy imports (PEP 810)

By default, Python loads every imported module the moment it reaches the import line. Consequently, a file with dozens of heavy imports pays that cost before any real work begins. The new lazy keyword defers loading until the code first touches the module.

```
lazy import json
lazy from pathlib import Path
print("Starting up")                     # json and pathlib are not loaded yet
data = json.loads('{"a": 1}')            # json loads here
```
Teams can control lazy loading at several levels:

- **Per import:** The lazy keyword works with both import and from … import statements.
- **Whole application:** The -X lazy_imports flag or PYTHON_LAZY_IMPORTS variable applies a mode across the program.
- **Filtered:** sys.set_lazy_imports_filter() lets teams make only their own modules lazy.
- **Older versions:** A module can declare __lazy_modules__ to opt in without new syntax, which keeps code runnable on earlier releases.

Before adopting it, keep three limits in mind.

First, import side effects such as plugin registration now run at first use instead of at startup. Second, lazy only works at the module level, so using it inside a function or try block raises a SyntaxError.

Finally, a failing lazy import reports its error at first use, and the traceback shows both locations.

### 2. frozendict, a new immutable built-in (PEP 814)

Python has long offered tuple and frozenset as frozen versions of lists and sets. Now dictionaries have one too. A frozendict cannot change after creation, which makes it a natural fit for configuration, constants, and cache keys.

```
a = frozendict(x=1, y=2)
b = frozendict(y=2, x=1)
a == b and hash(a) == hash(b)         # True
```
- **Hashable:** It can serve as a dictionary key or set member when all its keys and values are hashable.
- **Order:** It keeps insertion order, although equality checks ignore order.
- **Type checks:** It is not a subclass of dict, so code that relies on isinstance(x, dict) needs a review.

### 3. sentinel for clear marker values (PEP 661)

Functions often need a default that means “no value given” when None is already a valid input. Developers have used object() for this, but it prints as an unreadable address. The new built-in sentinel type gives the marker a readable name, which makes logs and debugger output far easier to follow.

```
MISSING = sentinel("MISSING")
def fetch(key, default=MISSING):
    ...
```
### 4. UTF-8 as the default encoding (PEP 686)

Previously, open() without an encoding argument used the operating system’s locale setting. As a result, the same script could read a file correctly on Linux and fail on a Windows machine. In 3.15, text files open as UTF-8 on every platform.

- **Main risk:** Code that reads legacy files saved in another encoding, such as cp1252, without naming that encoding.
- **Find the gaps:** Run with -X warn_default_encoding to flag every file operation that skips the argument.
- **Temporary opt out:** Set PYTHONUTF8=0 or -X utf8=0 while the fixes are in progress.

### 5. Unpacking in comprehensions (PEP 798)

The * and ** operators now work inside list, set, and dict comprehensions, as well as generator expressions. This removes the need for nested loops or itertools.chain in common flattening and merging tasks.

```
flat = [*row for row in [[1, 2], [3, 4]]]      # [1, 2, 3, 4]
merged = {**d for d in [{"a": 1}, {"b": 2}]}   # {'a': 1, 'b': 2}
```
### 6. Python 3.15 performance improvements

The experimental JIT compiler received its largest upgrade since it arrived in 3.13. It now covers more bytecode and control flow, keeps some values in CPU registers, and simplifies work when it can prove a value is constant.

According to the official Python 3.15 release announcement, this delivers a 7 to 8 percent geometric mean speedup on x86-64 Linux over the standard interpreter. On AArch64 macOS, it achieves an 11 to 12 percent speedup over the tail-calling interpreter.

Those figures are averages, so individual programs can land well above or below them. In addition, the release brings several runtime and build changes:

- **JIT status:** It remains experimental and opt-in, through PYTHON_JIT=1, so test it in staging first.
- **Windows builds:** Official 64-bit Windows binaries now use the faster tail-calling interpreter.
- **Frame pointers (PEP 831):** Enabled by default, which helps profilers and eBPF tools read call stacks reliably.
- **macOS installers:** Official macOS binaries now install free-threading support by default, while the standard build still keeps the GIL.

### 7. Tachyon and the new profiling package (PEP 799)

A new profiling package groups Python’s profilers in one place. The existing tracing profiler moves to profiling.tracing, and cProfile stays as an alias. Alongside it, profiling.sampling introduces Tachyon, a high-frequency sampling profiler.

`python -m profiling.sampling run my_script.py`
- **Live attach:** Tachyon can attach to a running process by PID without code changes or a restart.
- **Modes:** It measures wall-clock time, CPU time, GIL-holding time, and exception handling time.
- **Outputs:** Results export as flame graphs, heatmaps, pstats, collapsed stacks, or Firefox Profiler format.
- **Deprecation:** The older profile module is deprecated and scheduled for removal in 3.17.

### 8. Typing, error messages, and color

Type checker users get three additions. TypeForm (PEP 747) annotates values that are themselves type expressions. TypedDict now accepts closed and extra_items arguments (PEP 728) to define exactly which keys a payload allows. @typing.disjoint_base (PEP 800) mainly serves type checker authors. Separately, error messages now suggest nested attributes and recognize method names from other languages, and the interpreter adds more color to its output.

## Packaging and C API changes

These Python 3.15 updates mainly affect library maintainers and teams that build native extensions:

- **Startup files (PEP 829):** New .start files replace import lines in .pth files, which are now quietly deprecated.
- **Free-threaded stable ABI:** The new abi3t ABI (PEPs 803, 820, and 793) covers free-threaded builds with a new export hook.
- **PyBytesWriter (PEP 782):** A new C API builds Python bytes objects more efficiently.
- **Finalization safety (PEP 788):** The C API gains protection against interpreter finalization.

## Python 3.15 vs. Python 3.14

| Area | Python 3.14 | Python 3.15 | 
|---|---|---|
| Imports | Eager only | Explicit lazy imports | 
| Default encoding | Locale-based | UTF-8 everywhere | 
| Immutable mapping | Not built in | frozendict | 
| Comprehensions | No unpacking | * and ** allowed | 
| Profiling | cProfile | profiling package with Tachyon | 
| JIT | Early experimental | Wider coverage, register allocation | 
| Frame pointers | Off by default | On by defaultz | 

## Breaking changes to check before upgrading

Most code runs unchanged, yet a few areas deserve a quick review. The official What’s new in Python 3.15 page lists every removal.

- **File reads:** Any code relying on the locale encoding may now read text differently.
- **Module attributes:** __cached__ is no longer set, so use __spec__.cached instead.
- **array.typecodes:** It is now a tuple instead of a string.
- **importlib.metadata:** A metadata folder without a metadata file now raises MetadataNotFound.
- **tarfile:** Security fixes tighten extraction, so retest code that unpacks untrusted archives.
- **macOS 27:** IDLE and other tkinter apps may hang on dialogs because of an operating system change that affects all current Python versions.

## How to upgrade to Python 3.15

A careful upgrade checks dependencies and encoding early, because those cause most failures. Teams without spare capacity often hire Python developers for the audit and migration work.

1. **Check the gap:** Run python –version to see how many releases the upgrade spans.
2. **Create a clean environment:** Build a fresh virtual environment with python3.15 -m venv .venv.
3. **Reinstall dependencies:** Watch for packages that fail to build, since they need 3.15 wheels first.
4. **Test with warnings as errors:** Run python -W error -m pytest, then repeat with -X warn_default_encoding.
5. **Benchmark and experiment:** Measure startup time, then trial lazy imports and the JIT in staging.
6. **Update the toolchain:** Refresh CI images, Docker base images, and debuggers such as debugpy.

## Should you upgrade to Python 3.15?

The answer depends on the workload.

Command-line tools and services with long import chains gain the most from lazy imports. Similarly, teams chasing intermittent slowdowns benefit from Tachyon’s live attach.

On the other hand, codebases that process legacy files or depend on compiled packages without 3.15 wheels should finish an audit before switching production.

For new projects, starting on 3.15 is the sensible default, since it carries the longest support window. Teams that want a structured plan can work with Python development services to assess readiness.

## Conclusion

Python 3.15 changes how applications start, read text, and report performance, while keeping most existing code compatible. The most valuable gains come from applying each feature where it fits: lazy imports for startup-heavy services, frozendict for shared configuration, and Tachyon for production diagnostics. Above all, a tested, staged rollout turns the release into a measurable improvement instead of a surprise.
