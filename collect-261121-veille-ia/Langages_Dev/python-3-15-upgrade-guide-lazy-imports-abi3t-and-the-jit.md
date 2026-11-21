---
id: collect-261121-veille-ia/langages-dev/python-3-15-upgrade-guide-lazy-imports-abi3t-and-the-jit
title: "Python 3.15 upgrade guide: lazy imports, abi3t and the JIT"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["release", "october", "candidate", "python", "final", "lazy", "imports", "abi3t"]
source: https://ecorpit.com/python-3-15-upgrade-free-threaded-abi-lazy-imports-jit-2026/
source_anchor: ""
source_lines: [1, 244]
section: "Langages_Dev"
---
**Summary.** Python 3.15 reached release candidate 1 on 4 August 2026 and candidate 2 on 1 September; a third candidate followed on 2 October, and the final release shipped on 9 October 2026, per PEP 790. Four changes carry real migration cost. PEP 810 adds a `lazy` soft keyword for deferred imports, with the PEP claiming a 50-70% startup reduction and 30-40% memory savings in real workloads. PEP 803 adds `abi3t`, a stable ABI for free-threaded builds, and CPython's final release notes list meson-python, scikit-build-core and Maturin as build tools that support it; setuptools is not on that list. PEP 686 makes UTF-8 the default encoding regardless of the system locale, which is the change most likely to break code silently. The JIT posts a 7-8% geometric mean gain over the standard interpreter on x86-64 Linux and 11-12% over the tail-calling interpreter on AArch64 macOS in the final release notes, and it stays off by default. Python 3.15 gets bugfix updates for two years and security patches until approximately October 2031, so the version you land on now is the one you run into the next decade. For teams paying GitHub's $0.002 per minute Actions cloud platform charge on a large CI fleet, a 50% startup cut on test collection is a line item, not a curiosity.

Update, 7 November 2026: Python 3.15.0 was released on 9 October 2026, not on 1 October as PEP 790 had planned, after a third release candidate on 2 October. The final release notes also put the JIT gain at 7-8% on x86-64 Linux and 11-12% on AArch64 macOS, and list meson-python, scikit-build-core and Maturin as build tools with abi3t support. The figures below follow those final notes.

Most upgrade guides for a new CPython release read as a changelog with adjectives. This one is organised around a different question: which of these changes will make your build fail, your tests behave differently, or your wheels stop shipping, and what do you do about each one between the 4 August release candidate and the 9 October general availability.

## The release schedule you are planning against

Hugo van Kemenade is the Python 3.15 release manager. PEP 790 sets the calendar, and the support window matters more than the feature list because it determines how long you live with this decision.

| Milestone | Date | What it means for you | 
|---|---|---|
| Beta 4 | 18 July 2026 (shipped) | Feature freeze is long past; the API surface is settled | 
| Release candidate 1 | 4 August 2026 | Start CI runs against it; report breakage while fixes are still cheap | 
| Release candidate 2 | 1 September 2026 | Last chance for a regression report to land before GA | 
| Final release | 9 October 2026, after a third release candidate on 2 October | Production-ready; begin staged rollout | 
| Bugfix updates | Every two months, for two years | Routine patch cadence through roughly October 2028 | 
| Security-only updates | Until approximately October 2031 | Source-only patches for five years after GA | 

PEP 790 states it plainly: "Python 3.15 will receive bugfix updates approximately every second month for two years." After the 3.17 release the final 3.15 bugfix update ships, and the version moves to source-only security patches.

The practical read is that release candidate 1, landing on 4 August 2026, is the moment to add a `3.15-rc` job to your CI matrix. Nothing further is going to change in the language surface. Every hour spent on breakage now is an hour you do not spend in October under deadline. The same discipline applies to any managed runtime, which is why we treat runtime upgrades as a scheduled engineering activity rather than an emergency, the way a Node.js LTS upgrade cadence works.

## PEP 810 lazy imports: the syntax, and where it refuses to work

PEP 810 was created on 2 October 2025 and resolved on 3 November 2025. It is now Final, and the canonical documentation has moved into the language reference. The proposal adds `lazy` as a soft keyword that defers loading and execution of a module until the first time an imported name is actually used.

The grammar in the PEP is narrow:

Everything else raises `SyntaxError`. The 3.15 documentation is explicit: "Lazy imports are only permitted at module scope; using `lazy` inside a function, class body, or `try`/`except`/`finally` block raises a `SyntaxError`. Neither star imports nor future imports can be lazy." Imports inside `with` blocks are allowed; an earlier draft banned them and that restriction was dropped.

### The motivation number, and how much to trust it

PEP 810 opens with a measurement worth quoting because it explains why the feature exists at all: "Analysis of the Python standard library shows that approximately 17% of all imports outside tests (nearly 3500 total imports across 730 files) are already placed inside functions or methods specifically to defer their execution."

That is the honest case for the feature. Deferred imports are already standard practice in the standard library; developers just implement them by hand, inside functions, where they cost a dictionary lookup on every call and hide the dependency from static analysis.

The PEP also claims that lazy imports "can reduce startup time by 50-70% in practice" and that "memory savings of 30-40% have been observed in real workloads." Treat those two figures with care. They sit in the Motivation section without a named benchmark or a reproducible workload attached, which is a different class of evidence from the overhead measurement the PEP does document.

That overhead measurement is solid, and it is the one that matters for a migration decision. Using hyperfine across 278 top-level standard library modules resolving to 392 total loaded modules, with everything forced to reify:

| Configuration | Wall time | Delta vs eager | 
|---|---|---|
| Eager baseline | 161.2 Â± 4.3 ms | Baseline | 
| Lazy, filter forcing eager | 161.7 Â± 4.2 ms | +0.3% | 
| Lazy, filter allowing lazy, then reified | 162.0 Â± 4.0 ms | +0.5% | 
| Lazy, no filter, then reified | 161.4 Â± 4.3 ms | +0.1% | 
| PySide on-demand initializer (prior art) | Not stated | 10-20% startup improvement | 

The PEP's conclusion: "Lazy imports have no measurable performance overhead." The worst case is within noise. That is the number that lets you adopt the feature without a rollback plan for the performance regression that does not exist.

### Control surface: flags, filters and introspection

You do not have to edit source to try this. The control surface is wider than the keyword:

- `-X lazy_imports=<mode>` on the command line

- `PYTHON_LAZY_IMPORTS=<mode>` in the environment

- `sys.set_lazy_imports(mode, /)` and`sys.get_lazy_imports()`

- `sys.set_lazy_imports_filter(func)` and`sys.get_lazy_imports_filter()` , where the filter is`func(importer: str, name: str, fromlist: tuple[str, ...] | None) -> bool`

Precedence runs `sys.set_lazy_imports()` above `-X lazy_imports=` above `PYTHON_LAZY_IMPORTS=`. One inconsistency to watch: the PEP documents three modes (`normal`, `all`, `none`), while the 3.15 release notes describe the flag as accepting two values, `all` and `normal` (the default). If you are scripting the flag, test the value you pass rather than trusting either document.

For introspection there is `types.LazyImportType`, `sys.lazy_modules` (the set of fully-qualified names imported lazily), a module-level `__lazy_modules__` container that makes plain `import` statements lazy and is ignored on Python below 3.15, and a `.resolve()` method to force reification. Lazy modules do not appear in `sys.modules` until they reify. The PEP is specific that `globals()`, a module's `__dict__`, `dir()` at global scope and `dir(mod)` all avoid triggering reification.

### The failure mode you must design for

This is the part that changes how you write error handling. From the PEP: "If reification fails (e.g., due to an `ImportError`), the lazy object is not reified or replaced. Subsequent uses of the lazy object will re-try the reification. Exceptions that happen during reification are raised as normal, but the exception is enhanced with chaining to show both where the lazy import was defined and where it was accessed."

An import error now surfaces at first use, not at module load. In a long-running service, that moves a startup crash into a request handler. The wrapper is an `ImportError` reading `lazy import of '<name>' raised an exception during resolution`, chained as the direct cause of the underlying exception, so the traceback tells you both locations. Plan for it anyway: anything whose absence should abort the process should stay eager.

There is no dedicated C API for creating or resolving lazy imports, and the PEP says none is planned. `.pth` files stay eager.

## PEP 803: the abi3t change that can stop your wheels shipping

This is the change most likely to cost a maintainer a weekend, and it is easy to misread.

Free-threading stopped being experimental in Python 3.14 under PEP 779, which was accepted on 16 June 2025 and set the criteria for supported status. That already happened. The 3.15 story is not the status of free-threading; it is the ABI. PEP 803 adds a stable ABI for free-threaded builds, tagged `abi3t`.

The 3.15 release notes describe what porting costs: "C extensions that target the Stable ABI can now be compiled for the new Stable ABI for Free-Threaded Builds (also known as `abi3t`), which makes them compatible with free-threaded builds of CPython. This usually requires some non-trivial changes to the source code."

Those changes are named. You switch to the API introduced in PEP 697 for Python 3.12, using negative `basicsize` and `PyObject_GetTypeData()` rather than making `PyObject` part of the instance struct. You switch from a `PyInit_` function to the new `PyModExport_*` export hook introduced in PEP 793, with the `PySlot` structure from PEP 820.

Then comes the part that determines your schedule. The release notes say the free-threaded stable ABI should normally be selected in a build tool, and the final 3.15.0 notes list meson-python, scikit-build-core and Maturin as tools with `abi3t` support; setuptools is not on that list. If your tool lacks it, compile for `cp315t` separately.

So if your build tool lacks abi3t support, the practical answer is: keep building `abi3` for the GIL-enabled stable ABI, and build `cp315t` separately for free-threaded 3.15. If you are building without a packaging tool, or writing one, you select `abi3t` by defining `Py_TARGET_ABI3T`. CPython ships a migration guide for the port.

One packaging detail that will confuse a CI script: file names of stable ABI extensions using the `.so` suffix may now include a multiarch tuple, for example `foo.abi3-x86-64-linux-gnu.so`. Any glob or artefact-matching regex in your release pipeline needs checking against that shape.

To detect a free-threaded interpreter at runtime, the free-threading how-to is unambiguous: `sysconfig.get_config_var("Py_GIL_DISABLED")` returns `1` on a build that supports free threading. Configuring with `--disable-gil` defines the `Py_GIL_DISABLED` macro and adds `"t"` to `sys.abiflags`.

The cost side is documented too. On the pyperformance suite, single-threaded overhead of the free-threaded build "ranges from about 1% on macOS aarch64 to 8% on x86-64 Linux systems". If your workload is single-threaded and CPU-bound, the free-threaded build is a tax, not a win. If it is genuinely parallel and currently paying for process-per-core with multiprocessing, the arithmetic changes.

| Build target | When to use it | Status in 3.15 | 
|---|---|---|
| `abi3` (stable ABI, GIL enabled) | Default for most published wheels | Fully supported by existing build tools | 
| `abi3t` (stable ABI, free-threaded) | Long-term target for free-threaded support | Supported by CPython, meson-python, scikit-build-core and Maturin; setuptools is not listed in the 3.15 notes | 
| `cp315t` (version-specific, free-threaded) | Interim answer for shipping free-threaded wheels now | Works today; needs a rebuild for every minor version | 
| Pure-Python wheel | No C extension | Unaffected by the ABI change | 
| GIL-enabled version-specific | Legacy builds | Still works; more wheels to publish | 

## The JIT: 7-8%, off by default, and still called experimental

CPython's JIT has been the most over-reported feature of the last three releases. Python 3.15 is the version where the numbers become worth acting on, with two caveats.

The final 3.15.0 release notes give the figures: on the pyperformance suite, the JIT averages a 7-8% geometric mean gain over the standard interpreter built with all optimisations on x86-64 Linux, and 11-12% over the tail-calling interpreter on AArch64 macOS. Individual benchmarks range from roughly 15% slower to more than twice as fast with the JIT as without it (leaving out the `unpack_sequence` microbenchmark) on both platforms.

The pre-release notes warned that these results were not yet final. The final notes drop that warning and settle one point lower than the 8-9% and 12-13% in the beta notes.

Earlier numbers circulate widely and differ, because they measured an earlier alpha. Ken Jin, writing on CPython's official Python Insider blog on 23 March 2026 about 3.15a7, reported: "The 3.15 alpha JIT is about 11-12% faster on macOS AArch64 than the tail calling interpreter, and 5-6% faster than the standard interpreter on x86_64 Linux. These numbers are geometric means and are preliminary." If you see 5-6% quoted for x86-64 Linux, that is the March alpha figure, not the current one. The stated team goal was a 5% faster JIT by 3.15 and 10% by 3.16, with free-threading support to follow.

What changed under the hood in 3.15: LLVM 21 as the build-time dependency, a new tracing frontend, basic register allocation, more optimizations, GDB and GNU `backtrace()` unwinding support, and better machine code generation. Trace recording alone increased JIT code coverage by 50% during the optimization effort.

Enabling it is a build-time decision plus a runtime switch:

On Windows, `PCbuild\build.bat --experimental-jit`. LLVM 21 is the officially supported version at build time, selectable with `LLVM_VERSION` and `LLVM_TOOLS_INSTALL_DIR`. End users installing a prebuilt Python do not need LLVM.

The status has not changed: the configure flag is still called `--enable-experimental-jit`, the JIT README still opens by calling it experimental, and it is not on by default in 3.15. Treat it as a benchmark-and-decide feature for a specific hot service, not a fleet-wide default. Separately, and often confused with the JIT, the official Windows 64-bit binaries now use the tail-calling interpreter.

The dispersion is the real story. A range from a 15% slowdown to over a 100% speedup means the geometric mean tells you nothing about your workload. Measure yours. The real cost is usually the benchmark harness, not the flag.

## UTF-8 by default: the change that breaks code quietly

PEP 686 lands in 3.15 and it is the one to brief your team on first, because nothing about it fails loudly.

From the release notes: "Python now uses UTF-8 as the default encoding, independent of the system's environment. This means that I/O operations without an explicit encoding, for example, `open('flying-circus.txt')`, will use UTF-8. This only applies when no `encoding` argument is given."

Every `open()` call in your codebase without an explicit `encoding=` argument changes behaviour on any machine whose locale was not already UTF-8. On a Windows box with a legacy code page, on a container with a minimal locale, on a CI runner with a different `LANG`, files that previously round-tripped now decode differently or raise `UnicodeDecodeError`.

Two escape hatches exist. `PYTHONUTF8=0` or `-X utf8=0` disables UTF-8 mode and restores the previous behaviour. `encoding='locale'` on an individual call still selects the locale encoding.

The migration is mechanical and should be done before you upgrade, not after: grep for `open(` without `encoding=`, and for `subprocess` calls that decode output, and make the encoding explicit. Code that names its encoding is unaffected by this change in either direction, which is the whole point.

## The new sampling profiler, and the deprecation attached to it

PEP 799 restructures profiling into a `profiling` package. `profiling.tracing` holds the deterministic profiler relocated from `cProfile`, and `profiling.sampling` holds the new statistical sampler, named Tachyon. `cProfile` remains as an alias. The `profile` module is deprecated and scheduled for removal in Python 3.17, so any tooling that imports it has roughly two release cycles to move.

The sampler's claim from the release notes: "This approach provides virtually zero overhead while achieving sampling rates of up to 1,000,000 Hz, making it the fastest sampling profiler available for Python (at the time of its contribution)."

The CLI surface is where this becomes useful in production. It supports `attach` (profile an already-running process), `run`, `run -m` and `dump`. Modes are `--mode wall|cpu|gil|exception`, with wall as the default, and `-a` covers all threads. Output formats include `--pstats`, `--collapsed`, `--flamegraph`, `--gecko` and `--heatmap`, plus `--live`, `--async-aware` and `--opcodes`.

A `gil` mode and an `attach` mode together mean you can profile GIL contention on a running production process without restarting it or importing a third-party agent. For teams currently running a commercial APM purely to answer "where is this service spending its time", that is a real substitution, and it is the kind of change worth revisiting during a Kubernetes platform migration rather than in isolation.

## frozendict, and the isinstance trap

PEP 814 adds `frozendict` to builtins. The release notes: "A new immutable type, `frozendict`, is added to the `builtins` module. A `frozendict` is not a subclass of `dict`; it inherits directly from `object`. A `frozendict` is hashable as long as all of its keys and values are hashable. A `frozendict` preserves insertion order, but comparison does not take order into account."

The trap is stated in the documentation itself: `isinstance(arg, dict)` will not match a `frozendict`. Any library that validates arguments with an `isinstance` check against `dict` will reject a `frozendict` its caller reasonably expected to work. If you maintain a library with duck-typed dictionary arguments, this is a compatibility issue to handle before your users hit it.

The standard library has been updated to accept it in `copy`, `decimal`, `json`, `marshal`, `plistlib` (serialization only), `pickle`, `pprint` and `xml.etree.ElementTree`. `eval()` and `exec()` accept it for globals; `type()` and `str.maketrans()` accept it where a dict is expected.

Other additions worth knowing: PEP 661 adds a `sentinel` builtin type. PEP 798 allows unpacking in comprehensions, including `[*L for L in lists]` and `{**d for d in dicts}`, and works with `async for`. PEP 831 turns on frame pointers by default via `-fno-omit-frame-pointer` and `-mno-omit-leaf-frame-pointer`, propagated through `sysconfig`, and the documentation warns that third-party build backends must preserve those flags. PEP 829 introduces `.start` files, with `import` lines in `.pth` files silently deprecated and ignored when a matching `.start` file exists. PEP 788 adds interpreter guards and views, and soft-deprecates the whole `PyGILState` family including `PyGILState_Ensure()` and `PyGILState_Release()`, with no removal planned. There is a new `math.integer` module.

## What breaks: the removal list

Python's deprecation schedule for 3.15 was published in advance and is the authoritative list of what disappears.

| Removed or changed | Replacement | Who this bites | 
|---|---|---|
| `importlib``load_module()` ;`zipimport.zipimporter.load_module()` | `exec_module()` | Custom importers, plugin loaders, packaging tooling | 
| `http.server.CGIHTTPRequestHandler` and`python -m http.server --cgi` | No direct replacement | Legacy internal tools and demo servers | 
| `pathlib.PurePath.is_reserved()` | `os.path.isreserved()` | Cross-platform path validation, mostly Windows-facing | 
| `threading.RLock()` arguments | `RLock()` now takes no arguments | Any call passing positional or keyword arguments | 
| `types.CodeType.co_lnotab` | Modern line-table APIs | Debuggers, coverage tools, profilers, bytecode rewriters | 
| `typing` keyword-argument`NamedTuple` syntax;`TypedDict("TD")` and`TypedDict("TD", None)` ;`typing.no_type_check_decorator()` | Class-based syntax; explicit fields | Older typed codebases and generated stubs | 
| `ctypes.SetPointerType()` ;`platform.java_ver()` ;`sysconfig.is_python_build()`*check_home* argument;`wave``getmark()` ,`setmark()` ,`getmarkers()` | Documented equivalents or removal | Narrow, but silent when it hits | 

Two further changes at the import-system level: `__cached__` set without `__spec__.cached`, and `__package__` set without `__spec__.parent`, cease to be set or considered.

One important caveat on the sources. CPython's 3.15 release notes page has a "Removed" section covering `ast`, `collections.abc`, `ctypes`, `datetime`, `glob`, `http.server`, `importlib.resources`, `pathlib`, `platform`, `sre_*`, `sysconfig`, `threading`, `types`, `typing`, `wave` and `zipimport`. The final 3.15.0 release notes also confirm that `sre_compile`, `sre_constants` and `sre_parse` are removed, and that `glob.glob0()` and `glob.glob1()` are removed. `datetime.utcnow()` and `utcfromtimestamp()` appear to remain deprecated rather than removed in 3.15.

## A rollout plan for the window between 4 August and 9 October 2026

Eight weeks is enough time if you sequence it. This is the order we would run it.

**Week 1, from release candidate 1 on 4 August.** Add a `3.15-rc` job to CI, allowed to fail. Do not gate merges on it yet. Run the full test suite and record every failure without fixing anything. The goal is an inventory.

**Week 2, encoding sweep.** Handle PEP 686 first because it is the highest-volume and lowest-risk fix. Find every `open()` without `encoding=`, every `subprocess` call decoding bytes, every file read in a test fixture. Make the encoding explicit. This work is safe on 3.11 through 3.14 as well, so it ships independently of the upgrade.

**Week 3, dependency audit.** For each C-extension dependency, check whether the maintainer publishes a 3.15 wheel. Where there is no wheel, decide now whether you wait, pin, vendor or replace. This is where upgrades actually stall, and it is the same failure pattern as any application modernization and framework upgrade programme: the runtime is easy, the transitive dependency graph is not.

**Week 4, removals.** Work the removal table above against your codebase and your first-party libraries. `co_lnotab`, `load_module()` and the `typing` syntax changes are the ones that show up in older internal tooling.

**Week 5, isinstance and API surface.** If you maintain internal libraries, audit `isinstance(x, dict)` checks for `frozendict` compatibility, and check any artefact-matching in your release pipeline against the new multiarch `.so` naming.

**Week 6, benchmark the JIT.** Only now, and only on a service you have a real benchmark for. Build with `--enable-experimental-jit=yes-off` so you can toggle `PYTHON_JIT` per deployment and compare on identical hardware. A geometric mean across pyperformance is not a prediction for your service.

**Week 7, lazy imports where they pay.** Command-line tools, test collection and serverless entry points first, since those are the workloads where startup dominates. Keep imports eager wherever a missing dependency should crash at boot. Use `-X lazy_imports=all` in a throwaway run to find out how much is available before you edit any source.

**Week 8, from release candidate 2 on 1 September.** Flip the CI job to required. Anything still failing is now a known, tracked issue rather than a surprise on 9 October.

The economics are worth stating once. GitHub reduced GitHub-hosted runner prices by up to 39% on 1 January 2026, and the current rates include a $0.002 per minute Actions cloud platform charge. On a fleet running thousands of CI minutes a day, a large cut in interpreter startup during test collection shows up on the invoice. That is a secondary benefit, not a reason to upgrade, but it is the kind of number that gets a migration approved. The same reasoning applies to how we scope runtime security patch response work: the upgrade is cheaper when it is scheduled than when it is forced.

## India-specific considerations

For teams in India running Python services for regulated domains, three points change the priority order.

The PEP 686 encoding change deserves extra attention where your data includes Indic scripts. Applications reading Devanagari, Tamil, Bengali or other non-Latin content from files without an explicit encoding have historically depended on the locale being set correctly on every host. Under 3.15 that dependency goes away in the common case, which is an improvement, but the transition itself can change behaviour on a host that was previously reading Latin-1 or a Windows code page. Test with real data, not ASCII fixtures.

Under the Digital Personal Data Protection Act 2023, the argument for the `attach` mode in the new sampling profiler is a data-handling one as much as a performance one. Profiling a running process locally, without shipping traces to a third-party service, keeps the diagnostic data inside your own boundary. Where you currently export application traces to an external APM, the residency question is worth revisiting alongside the upgrade.

Finally, the support window. Security patches until approximately October 2031 mean 3.15 is a reasonable long-term target for a platform you do not intend to revisit annually. For teams standardising a base image across several services, the two-year bugfix window and five-year security window are the numbers that belong in the decision record, not the JIT percentage. The same long-horizon logic applies when choosing any backend runtime, which is the trade-off we work through in comparing Deno, Bun and Node.js for production backends, and it sits inside the broader web platform developer guide view of how runtime and platform choices compound.

## FAQ

## How eCorpIT can help

eCorpIT is a Gurugram-based technology consulting organisation with senior engineering teams that run runtime and framework upgrades as scheduled programmes rather than emergencies. We audit dependency graphs for wheel availability, run the encoding and removal sweeps described above, and benchmark JIT and free-threaded builds on your own workloads before anything reaches production. We are CMMI Level 5, MSME certified and ISO 27001:2022 certified, and we design applications aligned with DPDP requirements where profiling and telemetry data are involved. If you are planning a Python 3.15 rollout now that 3.15.0 has shipped, contact us to scope the migration.

## References

1. PEP 790 - Python 3.15 Release Schedule, Hugo van Kemenade, Python Enhancement Proposals, last modified 26 October 2026.

1. What's new in Python 3.15, CPython documentation, 3.15.2 build.

1. PEP 810 - Explicit lazy imports, Python Enhancement Proposals, created 2 October 2025, resolved 3 November 2025.

1. Lazy imports - Python language reference, CPython documentation.

1. PEP 803 - Stable ABI for free-threaded builds, Python Enhancement Proposals.

1. abi3t migration how-to, CPython documentation.

1. Python 3.15's JIT is now back on track, Ken Jin, Python Insider, 23 March 2026.

1. CPython JIT build documentation, python/cpython on GitHub.

1. Python configure options, including --enable-experimental-jit and --disable-gil, CPython documentation.

1. Python support for free threading, CPython documentation.

1. Pending removal in Python 3.15, CPython documentation.

1. Deprecations index for Python 3.15, CPython documentation.

1. Update to GitHub Actions pricing, GitHub Changelog, 16 December 2025, updated 17 December 2025.

1. Doc/whatsnew/3.15.rst source, python/cpython on GitHub.

1. PEP 779 - Criteria for supported status for free-threaded Python, Thomas Wouters, Matt Page and Sam Gross, Python Enhancement Proposals, accepted 16 June 2025.

Last updated: 7 November 2026.
