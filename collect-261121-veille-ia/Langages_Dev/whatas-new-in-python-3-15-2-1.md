---
id: collect-261121-veille-ia/langages-dev/whatas-new-in-python-3-15-2-1
title: "Whatâs new in Python 3.15"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["python", "import", "lazy", "module", "imported", "changes", "imports", "code"]
source: https://docs.python.org/dev/whatsnew/3.15.html
source_anchor: ""
source_lines: [1, 1500]
section: "Langages_Dev"
---
# Whatâs new in Python 3.15Â¶

- Editor:
- Hugo van Kemenade

This article explains the new features in Python 3.15, compared to 3.14. Python 3.15 was released on 9 October 2026. For full details, see the changelog.

## Summary â Release highlightsÂ¶

Python 3.15 is the latest stable release of the Python programming language, with a mix of changes to the language, the implementation, and the standard library. The biggest changes include lazy imports, frozendict and sentinel builtins, UTF-8 as the default encoding, unpacking in comprehensions, and a stable ABI for free-threaded builds.

The library changes include a new profiling package with Tachyon, a high-frequency statistical sampling profiler, more color in command-line output, as well as the usual deprecations and removals, and improvements in user-friendliness and correctness.

This article doesnât attempt to provide a complete specification of all new features, but instead gives a convenient overview. For full details refer to the documentation, such as the Library Reference and Language Reference. To understand the complete implementation and design rationale for a change, refer to the PEP for a particular new feature; but note that PEPs usually are not kept up-to-date once a feature has been fully implemented.

See Porting to Python 3.15 for guidance on upgrading from earlier versions of Python.

Interpreter improvements:

Significant improvements in the standard library:

New typing features:

C API improvements:

- **PEP 782** : A new PyBytesWriter C API to create a Python bytes object
- **PEP 788** : Protection against finalization in the C API
- **PEP 803** ,**820** ,**793** :
Stable ABI for free-threaded builds and
related C API

Build changes:

Release changes:

- The official Windows 64-bit binaries now use the tail-calling interpreter
- The official macOS binaries now install free-threading support by default

## New featuresÂ¶

Large Python applications often suffer from slow startup times. A significant contributor to this problem is the import system: when a module is imported, Python must locate the file, read it from disk, compile it to bytecode, and execute all top-level code. For applications with deep dependency trees, this process can take seconds, even when most of the imported code is never actually used during a particular run.

Developers have worked around this by moving imports inside functions, using
`importlib` to load modules on demand, or restructuring code to avoid
unnecessary dependencies. These approaches work but make code harder to read
and maintain, scatter import statements throughout the codebase, and require
discipline to apply consistently.

Python now provides a cleaner solution through explicit `lazy`
imports using the new `lazy` soft keyword. When you mark an import as
lazy, Python defers the actual module loading until the imported name is
first used. This gives you the organizational benefits of declaring all
imports at the top of the file while only paying the loading cost for
modules you actually use.

The `lazy` keyword works with both `import` and `from ... import`
statements. When you write `lazy import heavy_module`, Python does not
immediately load the module. Instead, it creates a lightweight proxy object.
The actual module loading happens transparently when you first access the
name:

```
lazy import json
lazy from pathlib import Path
print("Starting up...")  # json and pathlib are not fully imported yet
data = json.loads('{"key": "value"}')  # json gets fully imported here
p = Path(".")  # pathlib gets fully imported here
```
This mechanism is particularly useful for applications that import many modules at the top level but may only use a subset of them in any given run. The deferred loading reduces startup latency without requiring code restructuring or conditional imports scattered throughout the codebase.

In the case where loading a lazily imported module fails (for example, if the module does not exist), Python raises the exception at the point of first use rather than at import time. The associated traceback includes both the location where the name was accessed and the original import statement, making it straightforward to diagnose and debug the failure.

For cases where you want to enable lazy loading globally without modifying
source code, Python provides the `-X lazy_imports` command-line
option and the `PYTHON_LAZY_IMPORTS` environment variable. Both
accept two values: `all` makes all imports lazy by default, and `normal`
(the default) respects the `lazy` keyword in source code. The
`sys.set_lazy_imports()` and `sys.get_lazy_imports()` functions allow
changing and querying this mode at runtime.

For more selective control, `sys.set_lazy_imports_filter()` accepts a
callable that determines whether a specific module should be loaded lazily.
The filter receives three arguments: the importing moduleâs name (or
`None`), the imported moduleâs name, and the fromlist (or `None` for
regular imports). It should return `True` to allow the import to be lazy,
or `False` to force eager loading. This allows patterns like making only
your own applicationâs modules lazy while keeping third-party dependencies
eager:

```
import sys
def myapp_filter(importing, imported, fromlist):
    return imported.startswith("myapp.")
sys.set_lazy_imports_filter(myapp_filter)
sys.set_lazy_imports("all")
import myapp.slow_module  # lazy (matches filter)
import json               # eager (does not match filter)
```
The proxy type itself is available as `types.LazyImportType` for code
that needs to detect lazy imports programmatically.

There are some restrictions on where the `lazy` keyword can be used. Lazy
imports are only permitted at module scope; using `lazy` inside a
function, class body, or `try`/`except`/`finally` block raises a
`SyntaxError`. Neither star imports nor future imports can be lazy
(`lazy from module import *` and `lazy from __future__ import ...` both
raise `SyntaxError`).

For code that cannot use the `lazy` keyword directly (for example, when
supporting Python versions older than 3.15 while still using lazy
imports on 3.15+), a module can define
`__lazy_modules__` as a container of fully qualified module
name strings.  Regular `import` statements for those modules are then treated
as lazy, with the same semantics as the `lazy` keyword:

```
__lazy_modules__ = ["json", "pathlib"]
import json     # lazy
import os       # still eager
```
(Contributed by Pablo Galindo Salgado and Dino Viehland in gh-142349.)

A new immutable type, `frozendict`, is added to the `builtins` module.
It does not allow modification after creation. A `frozendict` is not a subclass of `dict`;
it inherits directly from `object`. A `frozendict` is hashable
as long as all of its keys and values are hashable. A `frozendict` preserves
insertion order, but comparison does not take order into account.

For example:

```
>>> a = frozendict(x=1, y=2)
>>> a
frozendict({'x': 1, 'y': 2})
>>> a['z'] = 3
Traceback (most recent call last):
  File "<python-input-2>", line 1, in <module>
    a['z'] = 3
    ~^^^^^
TypeError: 'frozendict' object does not support item assignment
>>> b = frozendict(y=2, x=1)
>>> hash(a) == hash(b)
True
>>> a == b
True
```
The following standard library modules have been updated to accept
`frozendict`: `copy`, `decimal`, `json`, `marshal`,
`plistlib` (only for serialization), `pickle`, `pprint` and
`xml.etree.ElementTree`.

`eval()` and `exec()` accept `frozendict` for *globals*, and
`type()` and `str.maketrans()` accept `frozendict` for *dict*.

Code checking for `dict` type using `isinstance(arg, dict)` can be
updated to `isinstance(arg, (dict, frozendict))` to accept also the
`frozendict` type, or to `isinstance(arg, collections.abc.Mapping)`
to accept also other mapping types such as `MappingProxyType`.

(Contributed by Victor Stinner and Donghee Na in gh-141510.)

A new `sentinel` type is added to the `builtins` module for
creating unique sentinel values with a concise representation.  Sentinel
objects preserve identity when copied, support use in type expressions with
the `|` operator, and can be pickled when they are importable by module and
name.

(Contributed by Jelle Zijlstra in gh-148829; PEP 661 written by Tal Einat.)

A new `profiling` module has been added to organize Pythonâs built-in
profiling tools under a single, coherent namespace. This module contains:

- `profiling.tracing` : deterministic function-call tracing (relocated from`cProfile` ).
- `profiling.sampling` : a new statistical sampling profiler (named Tachyon).

The `cProfile` module remains as an alias for backwards compatibility.
The `profile` module is deprecated and will be removed in Python 3.17.

(Contributed by Pablo Galindo and LÃ¡szlÃ³ Kiss KollÃ¡r in gh-138122.)

### Tachyon: High-frequency statistical sampling profilerÂ¶

A new statistical sampling profiler (Tachyon) has been added as
`profiling.sampling`. This profiler enables low-overhead performance analysis of
running Python processes without requiring code modification or process restart.

Tachyon follows earlier work on external sampling profilers such as Austin, py-spy, Pyflame, and rbspy. We thank their authors and the wider profiling community for their work and ideas.

Unlike deterministic profilers (such as `profiling.tracing`) that instrument
every function call, the sampling profiler periodically captures stack traces from
running processes.  This approach provides virtually zero overhead while achieving
sampling rates of **up to 1,000,000 Hz**, making it the fastest sampling profiler
available for Python (at the time of its contribution) and ideal for debugging
performance issues in production environments. This capability is particularly
valuable for debugging performance issues in production systems where traditional
profiling approaches would be too intrusive.

Key features include:

- **Zero-overhead profiling** : Attach to any running Python process without
affecting its performance. Ideal for production debugging where you canât afford
to restart or slow down your application.
- **No code modification required** : Profile existing applications without restart.
Simply point the profiler at a running process by PID and start collecting data.
- **Flexible target modes** :
  - Profile running processes by PID ( `attach` ) - attach to already-running applications
  - Run and profile scripts directly ( `run` ) - profile from the very start of execution
  - Execute and profile modules ( `run -m` ) - profile packages run as`python -m module`
  - Capture a one-shot snapshot of a running process ( `dump` ) - print a
traceback-style stack of every thread (or all asyncio tasks with`--async-aware` ). Useful for investigating hung processes.
- **Multiple profiling modes** : Choose what to measure based on your performance investigation:
  - **Wall-clock time** (`--mode wall` , default): Measures real elapsed time including I/O,
network waits, and blocking operations. Use this to understand where your program spends
calendar time, including when waiting for external resources.
  - **CPU time** (`--mode cpu` ): Measures only active CPU execution time, excluding I/O waits
and blocking. Use this to identify CPU-bound bottlenecks and optimize computational work.
  - **GIL-holding time** (`--mode gil` ): Measures time spent holding Pythonâs Global Interpreter
Lock. Use this to identify which threads dominate GIL usage in multi-threaded applications.
  - **Exception handling time** (`--mode exception` ): Captures samples only from threads with
an active exception. Use this to analyze exception handling overhead.
- **Thread-aware profiling** : Option to profile all threads (`-a` ) or just the main thread,
essential for understanding multi-threaded application behavior.
- **Multiple output formats** : Choose the visualization that best fits your workflow:
  - `--pstats` : Detailed tabular statistics compatible with`pstats` . Shows function-level
timing with direct and cumulative samples. Best for detailed analysis and integration with
existing Python profiling tools.
  - `--collapsed` : Generates collapsed stack traces (one line per stack). This format is
specifically designed for creating flame graphs with external tools like Brendan Greggâs
FlameGraph scripts or speedscope.
  - `--flamegraph` : Generates a self-contained interactive HTML flame graph using D3.js.
Opens directly in your browser for immediate visual analysis. Flame graphs show the call
hierarchy where width represents time spent, making it easy to spot bottlenecks at a glance.
  - `--gecko` : Generates Gecko Profiler format compatible with Firefox Profiler. Upload the output to Firefox Profiler for advanced
timeline-based analysis with features like stack charts, markers, and network activity.
  - `--heatmap` : Generates an interactive HTML heatmap visualization with line-level sample
counts. Creates a directory with per-file heatmaps showing exactly where time is spent
at the source code level.
- **Live interactive mode** : Real-time TUI profiler with a top-like interface (`--live` ).
Monitor performance as your application runs with interactive sorting and filtering.
- **Async-aware profiling** : Profile async/await code with task-based stack reconstruction
(`--async-aware` ). See which coroutines are consuming time, with options to show only
running tasks or all tasks including those waiting.
- **Opcode-level profiling** : Gather bytecode opcode information for instruction-level
profiling (`--opcodes` ). Shows which bytecode instructions are executing, including
specializations from the adaptive interpreter.

See `profiling.sampling` for the complete documentation, including all
available output formats, profiling modes, and configuration options.

(Contributed by Pablo Galindo and LÃ¡szlÃ³ Kiss KollÃ¡r in gh-135953 and gh-138122.)

CPython is now built with frame pointers by default on platforms that support
them.  This uses the compiler flags `-fno-omit-frame-pointer` and
`-mno-omit-leaf-frame-pointer`, making native stack unwinding faster and
more reliable for system profilers, debuggers, crash analysis tools, and
eBPF-based observability tools.

The flags are exposed through `sysconfig`, so extension modules built by
tools that consume Pythonâs build configuration inherit frame pointers by
default.  This propagation is intentional: mixed Python/native profiling needs
an unbroken frame-pointer chain through the interpreter, extension modules,
embedding applications, and native libraries.

Important

Third-party build backends and native build systems should preserve these
flags when they consume Pythonâs `sysconfig` values.  Build systems
that compile C, C++, Rust, or other native code without inheriting Pythonâs
compiler flags should enable equivalent frame-pointer flags themselves.  A
single native component built without frame pointers can break stack
unwinding for the whole Python process.

(Contributed by Pablo Galindo Salgado and Savannah Ostrowski in gh-149201; PEP 831 written by Pablo Galindo Salgado, Ken Jin, Savannah Ostrowski, and Diego Russo.)

List, set, and dictionary comprehensions, as well as generator expressions, now
support unpacking with `*` and `**`.  This extends the unpacking syntax
from **PEP 448** to comprehensions, providing a new syntax for combining an
arbitrary number of iterables or dictionaries into a single flat structure.
This new syntax is a direct alternative to nested comprehensions,
`itertools.chain()`, and `itertools.chain.from_iterable()`.  For
example:

```
>>> lists = [[1, 2], [3, 4], [5]]
>>> [*L for L in lists]  # equivalent to [x for L in lists for x in L]
[1, 2, 3, 4, 5]
>>> sets = [{1, 2}, {2, 3}, {3, 4}]
>>> {*s for s in sets}  # equivalent to {x for s in sets for x in s}
{1, 2, 3, 4}
>>> dicts = [{'a': 1}, {'b': 2}, {'a': 3}]
>>> {**d for d in dicts}  # equivalent to {k: v for d in dicts for k,v in d.items()}
{'a': 3, 'b': 2}
```
Generator expressions can similarly use unpacking to yield values from multiple iterables:

```
>>> gen = (*L for L in lists)  # equivalent to (x for L in lists for x in L)
>>> list(gen)
[1, 2, 3, 4, 5]
```
This change also extends to asynchronous generator expressions, such that, for
example, `(*a async for a in agen())` is equivalent to ```
(x async for a in
agen() for x in a)
```
.

(Contributed by Adam Hartz in gh-143055.)

Loaded by the `site` module when `-S` is not given, .pth files
can contain lines that both extend `sys.path` and execute arbitrary code
when the line starts with `import` (followed by a space or tab).  The latter
functionality can be problematic, since it is difficult to know exactly what
gets executed when Python starts up.

As a step towards improving the ability to audit pre-start executable code,
Python 3.15 introduces .start files which contain entry point
specifications of the form `pkg.mod:callable` where `pkg.mod` is the
import path to the given callable.  When Python starts up, the callable is
located and called with no arguments.

`import` lines in `.pth` files are silently deprecated.  When a
matching `.start` file is found, `import` lines in `.pth` files
are ignored.  There is no change to `sys.path` extension lines in
`.pth` files.

The `site` module also provides `site.StartupState` to batch
startup processing for multiple site directories, ensuring all static path
extensions are applied before any startup code is executed.  `site.main()`
uses an instance of this class implicitly to batch process all startup
configuration files during normal interpreter startup.  Callers needing the
same batching behavior can build a `StartupState` directly and
drive it with `addsitedir()`,
`addusersitepackages()`, and
`addsitepackages()`, then call
`process()` once at the end of the batch.

C extensions that target the Stable ABI can now be
compiled for the new *Stable ABI for Free-Threaded Builds* (also known
as `abi3t`), which makes them compatible with
free-threaded builds of CPython.
This usually requires some non-trivial changes to the source code;
specifically:

- Switching to API introduced in **PEP 697** (Python 3.12), such as
negative`basicsize` and`PyObject_GetTypeData()` , rather than making`PyObject` part of the instance struct; and
- Switching from a `PyInit_` function to a new export hook,`PyModExport_*` , introduced for this
purpose in**PEP 793** , with a new`PySlot` structure
introduced in**PEP 820** .

Note that Stable ABI does not offer all the functionality that CPython
has to offer.
Extensions that cannot switch to `abi3t` should continue to build for
the existing Stable ABI (`abi3`) and the version-specific ABI for
free-threading (`cp315t`) separately.

Stable ABI for Free-Threaded Builds should typically
be selected in a build tool. Tools with `abi3t` support include
meson-python, scikit-build-core, and Maturin; see
Setting up the build for links to their documentation on `abi3t` builds.
If your build tool does not support `abi3t`, compile for `cp315t` separately.
If not using a build tool â or when writing such a tool â you can select
`abi3t` by setting the macro `Py_TARGET_ABI3T` as discussed
in Compiling for Stable ABI.

A practical migration guide for switching to
`abi3t` is available.
The community-maintained Python Free-Threading Guide provides examples of building
and testing `abi3t` wheels.

In the C API, interpreter finalization can be problematic for many extensions, because attaching a thread state will permanently hang the thread, resulting in deadlocks and other spurious issues. Additionally, it has historically been impossible to safely check whether an interpreter is alive before using it, leading to crashes when a thread concurrently deletes an interpreter while another thread is trying to attach to it.

There are now several new suites of APIs to circumvent these problems:

- Interpreter guards, which prevent an interpreter from finalizing.
- Interpreter views, which allow thread-safe access to an interpreter that may be concurrently finalizing or deleted.
- New APIs to automatically attach and detach thread states that come with built-in protection against finalization.

In addition, APIs in the `PyGILState` family (most notably
`PyGILState_Ensure()` and `PyGILState_Release()`) have been
soft deprecated. There is **no** plan to remove them, and existing
code will continue to work, but there will be no new `PyGILState` APIs
in future versions of Python.

(Contributed by Peter Bierma in gh-149101.)

### Improved error messagesÂ¶

- The interpreter now provides more helpful suggestions in `AttributeError` exceptions when accessing an attribute on an object that does not exist, but
a similar attribute is available through one of its members.For example, if the object has an attribute that itself exposes the requested name, the error message will suggest accessing it via that inner attribute: from dataclasses import dataclass from math import pi @dataclass class Circle: radius: float @property def area(self) -> float: return pi * self.radius**2 class Container: def __init__(self, inner: Circle) -> None: self.inner = inner circle = Circle(radius=4.0) container = Container(circle) print(container.area) Running this code now produces a clearer suggestion: Traceback (most recent call last): File "example.py", line 18, in <module> print(container.area) ^^^^^^^^^^^^^^ AttributeError: 'Container' object has no attribute 'area'. Did you mean '.inner.area' instead of '.area'?
- When an `AttributeError` on a builtin type has no close match via
Levenshtein distance, the error message now checks a static table of common
method names from other languages (JavaScript, Java, Ruby, C#) and suggests
the Python equivalent:>>> [1, 2, 3].push(4) Traceback (most recent call last): ... AttributeError: 'list' object has no attribute 'push'. Did you mean '.append'? >>> 'hello'.toUpperCase() Traceback (most recent call last): ... AttributeError: 'str' object has no attribute 'toUpperCase'. Did you mean '.upper'? When the Python equivalent is a language construct rather than a method, the hint describes the construct directly: >>> {}.put("a", 1) Traceback (most recent call last): ... AttributeError: 'dict' object has no attribute 'put'. Use d[k] = v. When a mutable method is called on an immutable type, the hint suggests the mutable counterpart: >>> (1, 2, 3).append(4) Traceback (most recent call last): ... AttributeError: 'tuple' object has no attribute 'append'. Did you mean to use a 'list' object? These hints also work for subclasses of builtin types. (Contributed by Matt Van Horn in gh-146406.)
- The interpreter now tries to provide a suggestion when `delattr()` fails due to a missing attribute.
When an attribute name that closely resembles an existing attribute is used,
the interpreter will suggest the correct attribute name in the error message.
For example:>>> class A: ... pass >>> a = A() >>> a.abcde = 1 >>> del a.abcdf Traceback (most recent call last): ... AttributeError: 'A' object has no attribute 'abcdf'. Did you mean: 'abcde'? (Contributed by Nikita Sobolev and Pranjal Prajapati in gh-136588.)
- Several error messages incorrectly using the term âargumentâ have been corrected. (Contributed by Stan Ulbrych in gh-133382.)

## Other language changesÂ¶

- Python now uses UTF-8 as the default encoding, independent of the systemâs environment. This means that I/O operations without an explicit encoding, for example, `open('flying-circus.txt')` , will use UTF-8.
UTF-8 is a widely-supported Unicode character encoding that has become a*de facto* standard for representing text, including nearly every webpage
on the internet, many common file formats, programming languages, and more.This only applies when no `encoding` argument is given. For best
compatibility between versions of Python, ensure that an explicit`encoding` argument is always provided. The opt-in encoding warning
can be used to identify code that may be affected by this change.
The special`encoding='locale'` argument uses the current locale
encoding, and has been supported since Python 3.10.To retain the previous behaviour, Pythonâs UTF-8 mode may be disabled with the `PYTHONUTF8=0` environment variable or the`-X utf8=0` command-line option.(Contributed by Adam Turner in gh-133711; PEP 686 written by Inada Naoki.)
- The interpreter help (such as `python --help` ) is now in color.
This can be controlled by environment variables.
(Contributed by Hugo van Kemenade in gh-148766.)
- Unraisable exceptions are now highlighted with color by default. This can be controlled by environment variables. (Contributed by Peter Bierma in gh-134170.)
- More color in argparse, ast, calendar, difflib, http.server, pickletools, PyREPL tab completion, python âhelp, sqlite3, timeit, tokenize, unraisable exceptions and stdlib (ast, compileall, doctest, gzip, inspect, json.tool, pdb, profiling.sampling, random, regrtest, sqlite3, timeit, tokenize, trace, unittest, uuid, zipapp, zipfile) CLI help.
- The `__repr__()` of`ImportError` and`ModuleNotFoundError` now shows ânameâ and âpathâ as`name=<name>` and`path=<path>` if they were given
as keyword arguments at construction time.
(Contributed by Serhiy Storchaka, Oleg Iarygin, and Yoav Nir in gh-74185.)
- The `__dict__` and`__weakref__` descriptors now use a
single descriptor instance per interpreter, shared across all types that
need them.
This speeds up class creation, and helps avoid reference cycles.
(Contributed by Petr Viktorin in gh-135228.)
- The `-W` option and the`PYTHONWARNINGS` environment variable
can now specify regular expressions instead of literal strings to match
the warning message and the module name, if the corresponding field starts
and ends with a forward slash (`/` ).
(Contributed by Serhiy Storchaka in gh-134716.)
- Functions that take timestamp or timeout arguments now accept any real numbers (such as `Decimal` and`Fraction` ),
not only integers or floats, although this does not improve precision.
(Contributed by Serhiy Storchaka in gh-67795.)

- Added `bytearray.take_bytes(n=None, /)` to take
bytes out of a`bytearray` without copying. This enables optimizing code
which must return`bytes` after working with a mutable buffer of bytes
such as data buffering, network protocol parsing, encoding, decoding,
and compression. Common code patterns which can be optimized with`take_bytes()` are listed below.Â¶ Description Old New Return `bytes` after working with`bytearray`def read() -> bytes: buffer = bytearray(1024) ... return bytes(buffer) def read() -> bytes: buffer = bytearray(1024) ... return buffer.take_bytes() Empty a buffer getting the bytes buffer = bytearray(1024) ... data = bytes(buffer) buffer.clear() buffer = bytearray(1024) ... data = buffer.take_bytes() Split a buffer at a specific separator buffer = bytearray(b'abc\ndef') n = buffer.find(b'\n') data = bytes(buffer[:n + 1]) del buffer[:n + 1] assert data == b'abc\n' assert buffer == bytearray(b'def') buffer = bytearray(b'abc\ndef') n = buffer.find(b'\n') data = buffer.take_bytes(n + 1) Split a buffer at a specific separator; discard after the separator buffer = bytearray(b'abc\ndef') n = buffer.find(b'\n') data = bytes(buffer[:n]) buffer.clear() assert data == b'abc' assert len(buffer) == 0 buffer = bytearray(b'abc\ndef') n = buffer.find(b'\n') buffer.resize(n) data = buffer.take_bytes() (Contributed by Cody Maloney in gh-139871.)
- Many functions related to compiling or parsing Python code, such as `compile()` ,`ast.parse()` ,`symtable.symtable()` ,
and`importlib.abc.InspectLoader.source_to_code()` , now allow the module
name to be passed. It is needed to unambiguously filter
syntax warnings by module name.
(Contributed by Serhiy Storchaka in gh-135801.)
- Allowed defining the *__dict__* and*__weakref__* __slots__
for any class.
(Contributed by Serhiy Storchaka in gh-41779.)
- Allowed defining any __slots__ for a class derived from `tuple` (including classes created by`collections.namedtuple()` ).
(Contributed by Serhiy Storchaka in gh-41779.)
- The `slice` type now supports subscription,
making it a generic type.
(Contributed by James Hilton-Balfe in gh-128335.)
- The class `memoryview` now supports the float complex and
double complex C types: formatting characters`'Zf'` and`'Zd'` respectively.
(Contributed by Victor Stinner in gh-146151 and gh-148675.)
- Allow the *count* argument of`bytes.replace()` to be a keyword.
(Contributed by Stan Ulbrych in gh-147856.)
- Unary plus is now accepted in `match` literal patterns, mirroring
the existing support for unary minus.
(Contributed by Bartosz SÅawecki in gh-145239.)
- The import system now acquires per-module locks in hierarchical order (parent packages before their submodules). This fixes a long-standing deadlock where one thread importing `pkg.sub` and another importing`pkg.sub.mod` could each block the other when`pkg/sub/__init__.py` imports`pkg.sub.mod` .
(Contributed by Gregory P. Smith in gh-83065.)
- File names of Stable ABI extensions that use the `.so` suffix may now
include a multiarch tuple, for example,`foo.abi3-x86-64-linux-gnu.so` .
This permits stable ABI extensions for multiple architectures to be
co-installed into the same directory, without clashing with each
other, as regular dynamic extensions do.
(Contributed by Stefano Rivera in gh-122931.)
- The `__cached__` attribute on modules, which was deprecated since version
3.13, is no longer set or taken into consideration by the import system or
standard library.
Use`__spec__.cached` instead.
(Contributed by Brett Cannon in gh-97879.)Note that the `__loader__` and`__package__` attributes are also deprecated and scheduled for removal.

## Default interactive shellÂ¶

- Tab completions are now colored by object kind, based on fancycompleter. Set `PYTHON_BASIC_COMPLETER` to fall back to`rlcompleter` .
Color can also be controlled by environment variables.
(Contributed by Antonio Cuni and Pablo Galindo in gh-130472.)
- Tab completion now suggests module attributes in `from ... import` statements.
Attributes can only be suggested once the module is imported, so
stdlib modules are imported automatically, while for
other modules the completer offers to import them when`Tab` is pressed a second time.
(Contributed by LoÃ¯c Simon and Pablo Galindo in gh-140870.)

## New modulesÂ¶

### math.integerÂ¶

This module provides access to the mathematical functions for integer
arguments (**PEP 791**).
(Contributed by Serhiy Storchaka in gh-81313.)

## Improved modulesÂ¶

### argparseÂ¶

- The `BooleanOptionalAction` action now supports single-dash
long options and alternate prefix characters.
(Contributed by Serhiy Storchaka in gh-138525.)
- Changed the *suggest_on_error* parameter of`argparse.ArgumentParser` to
default to`True` . This enables suggestions for mistyped arguments by default.
(Contributed by Jakob Schluse in gh-140450.)
- Added backtick markup support in `ArgumentParser` description
and epilog text to highlight inline code when color output is enabled.
(Contributed by Savannah Ostrowski in gh-142390.)
- Extended backtick markup to argument `help` text and added support for
double backticks (RST inline-literal style).
(Contributed by Hugo van Kemenade in gh-149375.)

### arrayÂ¶

- Support the float complex and double complex C types: formatting characters `'Zf'` and`'Zd'` respectively.
(Contributed by Victor Stinner in gh-146151 and gh-148675.)
- Support half-floats (16-bit IEEE 754 binary interchange format): formatting character `'e'` .
(Contributed by Sergey B Kirpichev in gh-146238.)
- The `array.typecodes` type changed from`str` to`tuple` to support type codes longer than 1 character (`Zf` and`Zd` ).
(Contributed by Victor Stinner in gh-148675.)

### astÂ¶

- Add *color* parameter to`dump()` .
If`True` , the returned string is syntax highlighted using ANSI escape
sequences.
If`False` (the default), colored output is always disabled.
(Contributed by Stan Ulbrych in gh-148981.)
- The command-line output is now syntax highlighted by default. This can be controlled using environment variables. (Contributed by Stan Ulbrych in gh-148981.)

### asyncioÂ¶

- Added `TaskGroup.cancel` to allow early
termination of a task group, for instance, when the goal of the tasks has
been achieved or their services are no longer needed.
Previously this would involve unintuitive boilerplate such as an extra task
raising a custom exception which is then suppressed as it exits the task group.
(Contributed by John Belmonte in gh-127214.)

### base64Â¶

- Added the *pad* parameter in`z85encode()` .
(Contributed by Hauke DÃ¤mpfling in gh-143103.)
- Added the *padded* parameter in`b32encode()` ,`b32decode()` ,`b32hexencode()` ,`b32hexdecode()` ,`b64encode()` ,`b64decode()` ,`urlsafe_b64encode()` , and`urlsafe_b64decode()` .
(Contributed by Serhiy Storchaka in gh-73613.)
- Added the *wrapcol* parameter in`b16encode()` ,`b32encode()` ,`b32hexencode()` ,`b64encode()` ,`b85encode()` , and`z85encode()` .
(Contributed by Serhiy Storchaka in gh-143214 and gh-146431.)
- Added the *ignorechars* parameter in`b16decode()` ,`b32decode()` ,`b32hexdecode()` ,`b64decode()` ,`b85decode()` , and`z85decode()` .
(Contributed by Serhiy Storchaka in gh-144001 and gh-146431.)
- Added the *canonical* parameter in`b32decode()` ,`b32hexdecode()` ,`b64decode()` ,`a85decode()` ,`b85decode()` , and`z85decode()` ,
to reject encodings with non-zero padding bits or other non-canonical
forms.
(Contributed by Gregory P. Smith in gh-146311.)

### binasciiÂ¶

- Added functions for Base32 encoding: (Contributed by James Seo in gh-146192.)
- Added functions for Ascii85, Base85, and Z85 encoding: (Contributed by James Seo and Serhiy Storchaka in gh-101178.)
- Added the *padded* parameter in`b2a_base32()` ,`a2b_base32()` ,`b2a_base64()` , and`a2b_base64()` .
(Contributed by Serhiy Storchaka in gh-73613.)
- Added the *wrapcol* parameter in`b2a_base64()` .
(Contributed by Serhiy Storchaka in gh-143214.)
- Added the *alphabet* parameter in`b2a_base64()` and`a2b_base64()` .
(Contributed by Serhiy Storchaka in gh-145980.)
- Added the *ignorechars* parameter in`a2b_hex()` ,`unhexlify()` , and`a2b_base64()` .
(Contributed by Serhiy Storchaka in gh-144001 and gh-146431.)
- Added the *canonical* parameter in`a2b_base64()` ,
to reject encodings with non-zero padding bits.
(Contributed by Gregory P. Smith in gh-146311.)

### calendarÂ¶

- `calendar` âs command-line text output has more
color. This can be controlled with environment variables.
(Contributed by Hugo van Kemenade in gh-148352.)
- The `calendar` âs command-line HTML output now
accepts the year-month option:`python -m calendar -t html 2009 06` .
(Contributed by PÃ¥l GrÃ¸nÃ¥s Drange in gh-140212.)
- Calendar pages generated by the `calendar.HTMLCalendar` class now support
dark mode and have been migrated to the HTML5 standard for improved accessibility.
(Contributed by Jiahao Li and Hugo van Kemenade in gh-137634.)

### collectionsÂ¶

### concurrent.futuresÂ¶

- Improved error reporting when a child process in a `concurrent.futures.ProcessPoolExecutor` terminates abruptly.
The resulting traceback will now tell you the PID and exit code of the
terminated process.
(Contributed by Jonathan Berg in gh-139486.)

### contextlibÂ¶

- Added support for arbitrary descriptors `__enter__()` ,`__exit__()` ,`__aenter__()` , and`__aexit__()` in`ExitStack` and`contextlib.AsyncExitStack` , for
consistency with the`with` and`async with` statements.
(Contributed by Serhiy Storchaka in gh-144386.)
- `ContextDecorator` and`AsyncContextDecorator` (and therefore`contextmanager()` and`asynccontextmanager()` used as decorators) now detect generator functions, coroutine functions, and
asynchronous generator functions and keep the context manager open across
iteration or await.  Previously the context manager exited as soon as the
generator or coroutine object was created.
(Contributed by Alex GrÃ¶nholm & Gregory P. Smith in gh-125862.)

### ctypesÂ¶

- Change the `_type_` of`c_float_complex` ,`c_double_complex` and`c_longdouble_complex` from`F` ,`D` and`G` to`Zf` ,`Zd` and`Zg` for compatibility with NumPy.
(Contributed by Victor Stinner in gh-148675.)

### dataclassesÂ¶

- Annotations for generated `__init__` methods no longer include internal
type names.
(Contributed by David Ellis in gh-137530.)

### dbmÂ¶

- Added new `reorganize()` methods to`dbm.dumb` and`dbm.sqlite3` to recover unused free space previously occupied by deleted entries.
(Contributed by Andrea Oliveri in gh-134004.)

### difflibÂ¶

- Introduced the optional *color* parameter to`difflib.unified_diff()` ,
enabling color output similar to**git diff** .
This can be controlled by environment variables.
(Contributed by Douglas Thor in gh-133725.)
- Improved the styling of HTML diff pages generated by the `difflib.HtmlDiff` class, and migrated the output to the HTML5 standard.
(Contributed by Jiahao Li in gh-134580.)

### emailÂ¶

- Email generators now raise an error when an `EmailMessage` cannot be
accurately flattened due to a non-ASCII email address (mailbox) in an address
header. Options for supporting Email Address Internationalization (EAI) are
discussed in`EmailPolicy.utf8` .
(Contributed by R David Murray and Mike Edmunds in gh-122540.)

### faulthandlerÂ¶

- Added the *max_threads* parameter in`faulthandler.enable()` ,`faulthandler.dump_traceback()` ,`faulthandler.dump_traceback_later()` ,
and`faulthandler.register()` .
(Contributed by Eric Froemling in gh-149085.)

### functoolsÂ¶

- `singledispatchmethod()` now supports non-descriptor
callables.
(Contributed by Serhiy Storchaka in gh-140873.)
- `singledispatchmethod()` now dispatches on the second argument
if it wraps a regular method and is called as a class attribute.
(Contributed by Bartosz SÅawecki in gh-143535.)

### gcÂ¶

- Python 3.14.0-3.14.4 shipped with a new incremental garbage collector. However, due to a number of reports of significant memory pressure in production environments, it has been reverted back to the generational GC from 3.13. This is the GC now used in Python 3.14.5 and later and Python 3.15. (Contributed by Sergey Miryanov in gh-148726.)

### hashlibÂ¶

- Ensure that hash functions guaranteed to be always *available* exist as
attributes of`hashlib` even if they will not work at runtime due to
missing backend implementations. For instance,`hashlib.md5` will no
longer raise`AttributeError` if OpenSSL is not available and Python
has been built without MD5 support.
(Contributed by BÃ©nÃ©dikt Tran in gh-136929.)

### http.clientÂ¶

- A new *max_response_headers* keyword-only parameter has been added to`HTTPConnection` and`HTTPSConnection` constructors. This parameter overrides the default maximum number of allowed
response headers.
(Contributed by Alexander Enrique Urieles Nieto in gh-131724.)

### http.serverÂ¶

- The logging of `BaseHTTPRequestHandler` ,
as used by the command-line interface,
is colored by default.
This can be controlled with environment variables.
(Contributed by Hugo van Kemenade in gh-146292.)
- Added `default_content_type` and the`--content-type` command-line
option to allow customizing the default`Content-Type` header
for files with unknown extensions.
(Contributed by John Comeau and Hugo van Kemenade in gh-113471.)
- Add a new `extra_response_headers` keyword argument to`SimpleHTTPRequestHandler` to support custom headers in
HTTP responses.
(Contributed by Anton I. Sipos in gh-135057.)
- Add a `-H/--header` option to the**python -m http.server** command-line interface to support custom headers in HTTP responses.
(Contributed by Anton I. Sipos in gh-135057.)

### importlib.metadataÂ¶

- Previously, when accessing a distribution metadata directory not containing a metadata file, `metadata()` and`Distribution.metadata` would return an empty`PackageMetadata` object as if the file
was present but empty. Now, a`MetadataNotFound` exception is raised.
See importlib_metadata#493
for background and rationale and gh-143387 for rationale on the
compatibility concerns.
(Contributed by Jason R. Coombs.)

### inspectÂ¶

### jsonÂ¶

- Add the *array_hook* parameter to`load()` and`loads()` functions:
allow a callback for JSON literal array types to customize Python lists in
the resulting decoded object. Passing combined`frozendict` to*object_pairs_hook* parameter and`tuple` to*array_hook* will yield a
deeply nested immutable Python structure representing the JSON data.
(Contributed by Joao S. O. Bueno in gh-146440.)

### localeÂ¶

- `setlocale()` now supports language codes with`@` -modifiers.`@` -modifiers are no longer silently removed in`getlocale()` ,
but included in the language code.
(Contributed by Serhiy Storchaka in gh-137729.)
- Undeprecate the `locale.getdefaultlocale()` function.
(Contributed by Victor Stinner in gh-130796.)

### mathÂ¶

- Add `math.isnormal()` and`math.issubnormal()` functions.
(Contributed by Sergey B Kirpichev in gh-132908.)
- Add `math.fmax()` ,`math.fmin()` and`math.signbit()` functions.
(Contributed by BÃ©nÃ©dikt Tran in gh-135853.)

### mimetypesÂ¶

- Add more MIME types. (Contributed by Benedikt Johannes, Charlie Lin, Foolbar, Gil Forcada and John Franey in gh-144217, gh-145720, gh-140937, gh-139959, gh-145698, gh-145718, gh-145918, and gh-144213.)
- Rename `application/x-texinfo` to`application/texinfo` .
(Contributed by Charlie Lin in gh-140165.)
- Changed the MIME type for `.ai` files to`application/pdf` .
(Contributed by Stan Ulbrych in gh-141239.)

### mmapÂ¶

- `mmap.mmap` now has a*trackfd* parameter on Windows;
if it is`False` , the file handle corresponding to*fileno* will
not be duplicated.
(Contributed by Serhiy Storchaka in gh-78502.)
- Added the `mmap.mmap.set_name()` method
to annotate an anonymous memory mapping
if Linux kernel supports*PR_SET_VMA_ANON_NAME* (Linux 5.17 or newer).
(Contributed by Donghee Na in gh-142419.)

### osÂ¶

- Add `os.statx()` on Linux kernel versions 4.11 and later with
glibc versions 2.28 and later.
(Contributed by Jeffrey Bosboom and Victor Stinner in gh-83714.)
- `os.makedirs()` function now has a*parent_mode* parameter that allows
specifying the mode for intermediate directories. This can be used to match
the behavior from Python 3.6 and earlier by passing`parent_mode=mode` .
(Contributed by Zackery Spytz and Gregory P. Smith in gh-86533.)

### os.pathÂ¶

- Add support of the all-but-last mode in `realpath()` .
(Contributed by Serhiy Storchaka in gh-71189.)
- The *strict* parameter to`os.path.realpath()` accepts a new value,`os.path.ALLOW_MISSING` .
If used, errors other than`FileNotFoundError` will be re-raised;
the resulting path can be missing but it will be free of symlinks.
(Contributed by Petr Viktorin for**CVE 2025-4517** .)

### pathlibÂ¶

- `pathlib.Path.mkdir()` now has a*parent_mode* parameter that allows
specifying the mode for intermediate directories when`parents=True` .
(Contributed by Gregory P. Smith in gh-86533.)

### pdbÂ¶

### pickleÂ¶

- Add support for pickling private methods and nested classes. (Contributed by Zackery Spytz and Serhiy Storchaka in gh-77188.)

### pickletoolsÂ¶

- The output of the `pickletools` command-line interface is colored by
default. This can be controlled with
environment variables.
(Contributed by Hugo van Kemenade in gh-149026.)

### pprintÂ¶

- Add an *expand* keyword argument for`pprint.pprint()` ,`pprint.pformat()` ,`pprint.pp()` . If true, the output will be
formatted similar to pretty-printed`json.dumps()` when*indent* is supplied.
(Contributed by Stefan Todoran, Semyon Moroz and Hugo van Kemenade in
gh-112632.)
- Add t-string support to `pprint` .
(Contributed by LoÃ¯c Simon and Hugo van Kemenade in gh-134551.)

### reÂ¶

- `re.prefixmatch()` and a corresponding`re.Pattern.prefixmatch()` have been added as alternate, more explicit names for the existing
and now soft deprecated`re.match()` and`re.Pattern.match()` APIs. These are intended
to be used to alleviate confusion around what*match* means by following the
Zen of Pythonâs*âExplicit is better than implicitâ* mantra. Most other
language regular expression libraries use an API named*match* to mean what
Python has always called*search* .
(Contributed by Gregory P. Smith in gh-86519.)

### resourceÂ¶

- Add new constants: `RLIMIT_NTHR` ,`RLIMIT_UMTXP` ,`RLIMIT_THREADS` ,`RLIM_SAVED_CUR` , and`RLIM_SAVED_MAX` .
(Contributed by Serhiy Storchaka in gh-137512.)

### shelveÂ¶

### shutilÂ¶

- `shutil.copyfile()` now also raises`SpecialFileError` for
sockets and device files.
(Contributed by Savannah Ostrowski in gh-142693.)

### socketÂ¶

- Add constants for the ISO-TP CAN protocol. (Contributed by Patrick Menschel and Stefan Tatschner in gh-86819.)

### sqlite3Â¶

- The command-line interface has several new features: 
  - SQL keyword completion on `Tab` .
(Contributed by Long Tan in gh-133393.)
 
  - Prompts, error messages, and help text are now colored. This is enabled by default, see Controlling color for details. (Contributed by Stan Ulbrych and Åukasz Langa in gh-133461.)
  - Table, index, trigger, view, column, function, and schema completion on `Tab` .
(Contributed by Long Tan in gh-136101.)

### sslÂ¶

- Indicate through `ssl.HAS_PSK_TLS13` whether the`ssl` module
supports âExternal PSKsâ in TLSv1.3, as described in**RFC 9258** .
(Contributed by Will Childs-Klein in gh-133624.)
- Added new methods for managing groups used for SSL key agreement: 
  - `ssl.SSLContext.set_groups()` sets the groups allowed for doing
key agreement, extending the previous`ssl.SSLContext.set_ecdh_curve()` method.
This new API provides the ability to list multiple groups and
supports finite-field and post-quantum groups in addition to ECDH
curves. This method can also be used to control what key shares
are sent in the TLS handshake.
  - `ssl.SSLSocket.group()` returns the group selected for doing key
agreement on the current connection after the TLS handshake completes.
This call requires OpenSSL 3.2 or later.
  - `ssl.SSLContext.get_groups()` returns a list of all available key
agreement groups compatible with the minimum and maximum TLS versions
currently set in the context. This call requires OpenSSL 3.5 or later.
 (Contributed by Ron Frederick in gh-136306.)
- Added a new method `ssl.SSLContext.set_ciphersuites()` for setting TLS 1.3
ciphers. For TLS 1.2 or earlier,`ssl.SSLContext.set_ciphers()` should
continue to be used. Both calls can be made on the same context and the
selected cipher suite will depend on the TLS version negotiated when a
connection is made.
(Contributed by Ron Frederick in gh-137197.)
- Added new methods for managing signature algorithms: 
  - `ssl.get_sigalgs()` returns a list of all available TLS signature
algorithms. This call requires OpenSSL 3.4 or later.
  - `ssl.SSLContext.set_client_sigalgs()` sets the signature algorithms
allowed for certificate-based client authentication.
  - `ssl.SSLContext.set_server_sigalgs()` sets the signature algorithms
allowed for the server to complete the TLS handshake.
  - `ssl.SSLSocket.client_sigalg()` returns the signature algorithm
selected for client authentication on the current connection. This call
requires OpenSSL 3.5 or later.
  - `ssl.SSLSocket.server_sigalg()` returns the signature algorithm
selected for the server to complete the TLS handshake on the current
connection. This call requires OpenSSL 3.5 or later.
 (Contributed by Ron Frederick in gh-138252.)

### subprocessÂ¶

- `subprocess.Popen.wait()` : when`timeout` is not`None` and the
platform supports it, an efficient event-driven mechanism is used to wait for
process termination:
  - Linux >= 5.3 uses `os.pidfd_open()` +`select.poll()` .
  - macOS and other BSD variants use `select.kqueue()` +`KQ_FILTER_PROC` +`KQ_NOTE_EXIT` .
  - Windows keeps using `WaitForSingleObject` (unchanged).
 If none of these mechanisms are available, the function falls back to the traditional busy loop (non-blocking call and short sleeps). (Contributed by Giampaolo Rodola in gh-83069.)

### symtableÂ¶

- Add `symtable.Function.get_cells()` and`symtable.Symbol.is_cell()` methods.
(Contributed by Yashp002 in gh-143504.)

### sysÂ¶

- Add `sys.abi_info` namespace to improve access to ABI information.
(Contributed by Klaus Zimmermann in gh-137476.)

### sys.monitoringÂ¶

- The other events ( `PY_THROW` ,`PY_UNWIND` ,`RAISE` ,`EXCEPTION_HANDLED` , and`RERAISE` ) can now be turned on and disabled on a per code
object basis. Returning`DISABLE` from a callback for
one of these events disables the event for the entire code object (for the
current tool), rather than raising`ValueError` as in prior versions.
(Contributed by Gabriele N. Tornetta in gh-146182.)

### tarfileÂ¶

- `data_filter()` now normalizes symbolic link targets in order to
avoid path traversal attacks.
(Contributed by Petr Viktorin in gh-127987 and**CVE 2025-4138** .)
- `extractall()` now skips fixing up directory attributes
when a directory was removed or replaced by another kind of file.
(Contributed by Petr Viktorin in gh-127987 and**CVE 2024-12718** .)
- `extract()` and`extractall()` now (re-)apply the extraction filter when substituting a link (hard or
symbolic) with a copy of another archive member, and when fixing up
directory attributes.
The former raises a new exception,`LinkFallbackError` .
(Contributed by Petr Viktorin for**CVE 2025-4330** and**CVE 2024-12718** .)
- `extract()` and`extractall()` no longer extract rejected members when`errorlevel()` is zero.
(Contributed by Matt Prodani and Petr Viktorin in gh-112887
and**CVE 2025-4435** .)
- `extract()` and`extractall()` now replace slashes with backslashes in symlink targets on Windows to prevent
creation of corrupted links.
(Contributed by Christoph Walcher in gh-57911.)
- `gettarinfo()` now replaces backslashes with slashes in
symlink targets on Windows to conform to the tar format standard.
(Contributed by Daniele Nicolodi in gh-151669.)

### threadingÂ¶

- Added `serialize_iterator` ,`synchronized_iterator()` ,
and`concurrent_tee()` to support concurrent access to
generators and iterators.
(Contributed by Raymond Hettinger in gh-124397.)

### timeitÂ¶

- The output of the `timeit` command-line interface is colored by default.
This can be controlled with
environment variables.
(Contributed by Hugo van Kemenade in gh-146609.)
- The command-line interface now colorizes error tracebacks by default. This can be controlled with environment variables. (Contributed by Yi Hong in gh-139374.)
- Make the target time of `timeit.Timer.autorange()` configurable
and add`--target-time` option to the command-line interface.
(Contributed by Alessandro Cucci and Miikka Koskinen in gh-80642.)

### tkinterÂ¶

- The `tkinter.Text.search()` method now supports two additional
arguments:*nolinestop* which allows the search to
continue across line boundaries;
and*strictlimits* which restricts the search to within the specified range.
(Contributed by Rihaan Meher in gh-130848.)
- A new method `tkinter.Text.search_all()` has been introduced.
This method allows for searching for all matches of a pattern
using Tclâs`-all` and`-overlap` options.
(Contributed by Rihaan Meher in gh-130848.)
- Added new methods `pack_content()` ,`place_content()` and`grid_content()` which use Tk commands with new names (introduced
in Tk 8.6) instead of`*_slaves()` methods which use Tk commands
with outdated names.
(Contributed by Serhiy Storchaka in gh-143754.)
- Added `Event` attributes`user_data` for Tk virtual events
and`detail` for`Enter` ,`Leave` ,`FocusIn` ,`FocusOut` ,
and`ConfigureRequest` events.
(Contributed by Matthias Kievernagel and Serhiy Storchaka in gh-47655.)

### tokenizeÂ¶

- The output of the `tokenize` command-line interface is colored by default. This can be controlled with
environment variables.
(Contributed by Hugo van Kemenade in gh-148991.)

### tomllibÂ¶

- The `tomllib` module now supports TOML 1.1.0.
This is a backwards compatible update, meaning that all valid TOML 1.0.0
documents are parsed the same way.The changes, according to the official TOML changelog, are: 
  - Allow newlines and trailing commas in inline tables. Previously an inline table had to be on a single line and couldnât end with a trailing comma. This is now relaxed so that the following is valid: tbl = { key = "a string", moar-tbl = { key = 1, }, }
  - Add `\xHH` notation to basic strings for codepoints under 255,
and the`\e` escape for the escape character:null = "null byte: \x00; letter a: \x61" csi = "\e["
  - Seconds in datetime and time values are now optional. The following are now valid: dt = 2010-02-03 14:15 t = 14:15
 (Contributed by Taneli Hukkinen in gh-142956.)

### typesÂ¶

- Expose the write-through `locals()` proxy type
as`types.FrameLocalsProxyType` .
This represents the type of the`frame.f_locals` attribute,
as described in**PEP 667** .
(Contributed by Peter Bierma in gh-136492.)

### typingÂ¶

- **PEP 747** : Add`TypeForm` , a new special form for annotating
values that are themselves type expressions.`TypeForm[T]` means âa type form object describing`T` (or a type
assignable to`T` )â. At runtime,`TypeForm(x)` simply returns`x` ,
which allows explicit annotation of type-form values without changing
behavior.This helps libraries that accept user-provided type expressions (for example `int` ,`str | None` ,`TypedDict` classes, or`list[int]` ) expose precise signatures:from typing import Any, TypeForm def cast[T](typ: TypeForm[T], value: Any) -> T: ... (Contributed by Jelle Zijlstra in gh-145033.)

- **PEP 728** : Add support in`TypedDict` for the*closed* and*extra_items* class arguments. A closed`TypedDict` does not allow extra keys beyond those specified in the class body, while
a`TypedDict` with`extra_items` allows arbitrary extra
items where the values are of the specified type. (Contributed by Angela
Liss in gh-137840.)
- Code like `class ExtraTypeVars(P1[S], Protocol[T, T2]): ...` now raises
a`TypeError` , because`S` is not listed in`Protocol` parameters.
(Contributed by Nikita Sobolev in gh-137191.)
- Code like `class B2(A[T2], Protocol[T1, T2]): ...` now correctly handles
type parameters order: it is`(T1, T2)` , not`(T2, T1)` as it was incorrectly inferred at runtime before.
(Contributed by Nikita Sobolev in gh-137191.)
- **PEP 800** : Add`@typing.disjoint_base` , a new decorator marking a class
as a disjoint base. This is an advanced feature primarily intended to allow
type checkers to faithfully reflect the runtime semantics of types defined
as builtins or in compiled extensions. If a class`C` is a disjoint base, then
child classes of that class cannot inherit from other disjoint bases that are
not parent or child classes of`C` . (Contributed by Jelle Zijlstra in gh-148639.)
- `TypeVarTuple` now accepts`bound` ,`covariant` ,`contravariant` , and`infer_variance` keyword arguments, matching the
interface of`TypeVar` and`ParamSpec` .`bound` semantics remain undefined in the specification.

### unicodedataÂ¶

- The Unicode database has been updated to Unicode 17.0.0. (Contributed by Benjamin Peterson in gh-138706.)
- Add `unicodedata.isxidstart()` and`unicodedata.isxidcontinue()` functions to check whether a character can start or continue a
Unicode Standard Annex #31 identifier.
(Contributed by Stan Ulbrych in gh-129117.)
- Add the `iter_graphemes()` function to iterate over grapheme clusters according to rules defined in
Unicode Standard Annex #29, âUnicode Text Segmentationâ.
Add`grapheme_cluster_break()` ,`indic_conjunct_break()` and`extended_pictographic()` functions to get the properties
of the character which are related to the above algorithm.
(Contributed by Serhiy Storchaka and Guillaume Sanchez in gh-74902.)
- Add `block()` function to return the Unicode block
assigned to a character.
(Contributed by Stan Ulbrych in gh-66802.)

### unittestÂ¶

- `unittest.TestCase.assertLogs()` will now accept a formatter
to control how messages are formatted.
(Contributed by Garry Cairns in gh-134567.)
- `unittest.TestCase.assertWarns()` and`unittest.TestCase.assertWarnsRegex()` no longer swallow warnings that
do not match the specified category or regex.
Nested context managers are now supported.
(Contributed by Serhiy Storchaka in gh-143231.)

### urllib.parseÂ¶

- Add the *missing_as_none* parameter to`urlsplit()` ,`urlparse()` and`urldefrag()` functions.
Add the*keep_empty* parameter to`urlunsplit()` and`urlunparse()` functions.
This allows distinguishing between empty and undefined URI components
and preserving empty components.
(Contributed by Serhiy Storchaka in gh-67041.)

### venvÂ¶

- On POSIX platforms, platlib directories will be created if needed when creating virtual environments, instead of using a `lib64 -> lib` symlink.
This means purelib and platlib of virtual environments no longer share the
same`lib` directory on platforms where`sys.platlibdir` is not
equal to`lib` .
(Contributed by Rui Xi in gh-133951.)

### warningsÂ¶

- Improve filtering by module in `warnings.warn_explicit()` if no*module* argument is passed.
It now tests the module regular expression in the warnings filter not only
against the filename with`.py` stripped, but also against module names
constructed starting from different parent directories of the filename
(with`/__init__.py` ,`.py` and, on Windows,`.pyw` stripped).
(Contributed by Serhiy Storchaka in gh-135801.)

### waveÂ¶

- Added support for IEEE floating-point WAVE audio ( `WAVE_FORMAT_IEEE_FLOAT` ) in`wave` .
- Added `wave.Wave_read.getformat()` ,`wave.Wave_write.getformat()` ,
and`wave.Wave_write.setformat()` for explicit frame format handling.
- `wave.Wave_write.setparams()` accepts both 7-item tuples including`format` and 6-item tuples for backwards compatibility (defaulting to`WAVE_FORMAT_PCM` ).
- `WAVE_FORMAT_IEEE_FLOAT` output now includes a`fact` chunk,
as required for non-PCM WAVE formats.

(Contributed by Lionel Koenig and Michiel W. Beijen in gh-60729.)

### webbrowserÂ¶

- On macOS, the new `webbrowser.MacOS` class opens URLs via**/usr/bin/open** instead of constructing and executing AppleScript
via**osascript** . The default browser is detected from the
LaunchServices preferences file using`plistlib` , with`com.apple.Safari` as the fallback on fresh installations.
For non-HTTP(S) URLs,**open -b <bundle-id>** is used to route the
URL through a browser rather than the OS file handler, preventing
file injection attacks.
(Contributed by Jeff Lyon in gh-137586.)

### xmlÂ¶

- Add the `xml.is_valid_name()` function to check
whether a string can be used as an element or attribute name in XML.
(Contributed by Serhiy Storchaka in gh-139489.)
- Add the `xml.is_valid_text()` function to check
whether a string can be used in an XML document.
(Contributed by Serhiy Storchaka in gh-139489.)

### xml.parsers.expatÂ¶

- Add `SetAllocTrackerActivationThreshold()` and`SetAllocTrackerMaximumAmplification()` to xmlparser objects to tune protections against
disproportional amounts of dynamic memory usage from within an Expat parser.
(Contributed by BÃ©nÃ©dikt Tran in gh-90949.)
- Add `SetBillionLaughsAttackProtectionActivationThreshold()` and`SetBillionLaughsAttackProtectionMaximumAmplification()` to xmlparser objects to tune protections against
billion laughs attacks.
(Contributed by BÃ©nÃ©dikt Tran in gh-90949.)

### zlibÂ¶

- Allow combining two Adler-32 checksums via `adler32_combine()` .
(Contributed by Callum Attryde and BÃ©nÃ©dikt Tran in gh-134635.)
- Allow combining two CRC-32 checksums via `crc32_combine()` .
(Contributed by BÃ©nÃ©dikt Tran in gh-134635.)

## OptimizationsÂ¶

- `mimalloc` is now used as the default allocator for
raw memory allocations such as via`PyMem_RawMalloc()` for better performance on free-threaded builds.
(Contributed by Kumar Aditya in gh-144914.)

### base64 & binasciiÂ¶

- CPythonâs underlying base64 implementation now encodes 2x faster and decodes 3x faster thanks to simple CPU pipelining optimizations. (Contributed by Gregory P. Smith and Serhiy Storchaka in gh-143262.)
- Implementation for Ascii85, Base85, and Z85 encoding has been rewritten in C. Encoding and decoding is now two orders of magnitude faster and consumes two orders of magnitude less memory. (Contributed by James Seo and Serhiy Storchaka in gh-101178.)
- Implementation for Base32 has been rewritten in C. Encoding and decoding is now two orders of magnitude faster. (Contributed by James Seo in gh-146192.)

### csvÂ¶

- `csv.Sniffer.sniff()` delimiter detection is now up to 1.6x faster.
(Contributed by Maurycy PawÅowski-WieroÅski in gh-137628.)

### Upgraded experimental JIT compilerÂ¶

Results from the pyperformance
benchmark suite report a
7-8%
geometric mean performance improvement for the JIT over the standard CPython
interpreter built with all optimizations enabled on x86-64 Linux. On AArch64
macOS, the JIT has an
11-12%
speedup over the tail calling interpreter
with all optimizations enabled. The speedups for JIT
builds versus no JIT builds range from roughly 15% slowdown to over
100% speedup (ignoring the `unpack_sequence` microbenchmark) on
x86-64 Linux and AArch64 macOS systems.

The major upgrades to the JIT are:

- LLVM 21 build-time dependency
- New tracing frontend
- Basic register allocation in the JIT
- More JIT optimizations
- GDB and GNU `backtrace()` unwinding support
- Better machine code generation

LLVM 21 build-time dependency

The JIT compiler now uses LLVM 21 for build-time stencil generation. As always, LLVM is only needed when building CPython with the JIT enabled; end users running Python do not need LLVM installed. Instructions for installing LLVM can be found in the JIT compiler documentation for all supported platforms. (Contributed by Savannah Ostrowski in gh-140973.)

A new tracing frontend

The JIT compiler now supports significantly more bytecode operations and control flow than in Python 3.14, enabling speedups on a wider variety of code. For example, simple Python object creation is now understood by the 3.15 JIT compiler. Overloaded operations and generators are also partially supported. This was made possible by an overhauled JIT tracing frontend that records actual execution paths through code, rather than estimating them as the previous implementation did. (Contributed by Ken Jin in gh-139109. Support for Windows added by Mark Shannon in gh-141703.)

Basic register allocation in the JIT

A basic form of register allocation has been added to the JIT compilerâs optimizer. This allows the JIT compiler to avoid certain stack operations altogether and instead operate on registers. This allows the JIT to produce more efficient traces by avoiding reads and writes to memory. (Contributed by Mark Shannon in gh-135379.)

More JIT optimizations

More constant-propagation is now performed. This means when the JIT compiler detects that certain user code results in constants, the code can be simplified by the JIT. (Contributed by Ken Jin and Savannah Ostrowski in gh-132732.)

Reference counts are avoided whenever it is safe to do so. This generally reduces the cost of most operations in Python. (Contributed by Ken Jin, Donghee Na, Zheao Li, Hai Zhu, Savannah Ostrowski, Reiden Ong, Noam Cohen, Tomas Roun, PuQing, Cajetan Rodrigues, and Sacul in gh-134584.)

By tracking unique references to objects, the JIT optimizer can now eliminate reference count updates and perform in-place operations on ints and floats. (Contributed by Reiden Ong, and Pieter Eendebak in gh-143414 and gh-146306.)

The JIT optimizer now supports significantly more operations than in 3.14. (Contributed by Kumar Aditya, Ken Jin, Jiahao Li, and Sacul in gh-131798.)

GDB and GNU `backtrace()` unwinding support

The JIT compiler now publishes unwind information for generated machine code to the
GDB interface on supported Linux ELF platforms. When libgcc frame
registration is available, the same unwind information is also registered for
GNU `backtrace()` stack walkers. This allows native debuggers, crash
handlers, and diagnostic tools using these mechanisms to unwind through JIT
frames instead of stopping at generated code.
(Contributed by Diego Russo and Pablo Galindo Salgado in gh-146071 and
gh-149104.)

Better machine code generation

The JIT compilerâs machine code generator now produces better machine code for x86-64 and AArch64 macOS and Linux targets. In general, users should experience lower memory usage for generated machine code and more efficient machine code versus 3.14. (Contributed by Brandt Bucher in gh-136528 and gh-135905. Implementation for AArch64 contributed by Mark Shannon in gh-139855. Additional optimizations for AArch64 contributed by Mark Shannon and Diego Russo in gh-140683 and gh-142305.)

Maintainability

The JIT optimizerâs operations have been simplified. This was made possible by a refactoring of JIT data structures. (Contributed by Zhongtian Zheng in gh-148211 and Hai Zhu in gh-143421.)

## RemovedÂ¶

### astÂ¶

- The constructors of AST nodes now raise a `TypeError` when a required argument is omitted or when a keyword argument that does not
map to a field on the AST node is passed. These cases had previously raised a`DeprecationWarning` since Python 3.13.
(Contributed by Brian Schubert and Jelle Zijlstra in gh-137600 and gh-105858.)

### collections.abcÂ¶

- `collections.abc.ByteString` has been removed from`collections.abc.__all__` .`collections.abc.ByteString` has been
deprecated since Python 3.12, and is scheduled for removal in Python 3.17.
(Contributed by Alex Waygood in gh-118803.)

### ctypesÂ¶

- Removed the undocumented function `ctypes.SetPointerType()` ,
which has been deprecated since Python 3.13.
(Contributed by BÃ©nÃ©dikt Tran in gh-133866.)

### datetimeÂ¶

- `strptime()` now raises`ValueError` when the
format string contains`%d` (day of month) without a year directive.
This has been deprecated since Python 3.13.
(Contributed by Stan Ulbrych and Gregory P. Smith in gh-70647.)

### globÂ¶

- Removed the undocumented `glob.glob0()` and`glob.glob1()` functions, which have been deprecated since Python 3.13. Use`glob.glob()` and pass a directory to its*root_dir* argument instead.
(Contributed by Barney Gale in gh-137466.)

### http.serverÂ¶

- Removed the `CGIHTTPRequestHandler` class
and the`--cgi` flag from the**python -m http.server** command-line interface. They were deprecated in Python 3.13.
(Contributed by BÃ©nÃ©dikt Tran in gh-133810.)

### importlibÂ¶

- The `load_module()` methods of`Loader` and its
subclasses is removed.
The import system will no longer call it when defined on custom subclasses.
The method has been deprecated in favor of`exec_module()` since Python 3.4.
(Contributed by Brett Cannon in gh-97850.)

### importlib.resourcesÂ¶

- Removed deprecated `package` parameter
from`importlib.resources.files()` function.
(Contributed by Semyon Moroz in gh-138044.)

### pathlibÂ¶

- Removed deprecated `pathlib.PurePath.is_reserved()` .
Use`os.path.isreserved()` to detect reserved paths on Windows.
(Contributed by Nikita Sobolev in gh-133875.)

### platformÂ¶

- Removed the `platform.java_ver()` function,
which was deprecated since Python 3.13.
(Contributed by Alexey Makridenko in gh-133604.)

### sre_*Â¶

- Removed `sre_compile` ,`sre_constants` and`sre_parse` modules.
(Contributed by Stan Ulbrych in gh-135994.)

### sysconfigÂ¶

- Removed the *check_home* parameter of`sysconfig.is_python_build()` .
(Contributed by Filipe LaÃns in gh-92897.)

### threadingÂ¶

### typesÂ¶

- Removed `codeobject.co_lnotab` from`types.CodeType` ,
deprecated since Python 3.12 by**PEP 626** .
(Contributed by Nikita Sobolev in gh-134690.)

### typingÂ¶

- `typing.ByteString` has been removed from`typing.__all__` .`typing.ByteString` has been deprecated since Python 3.9, and is
scheduled for removal in Python 3.17.
(Contributed by Alex Waygood in gh-118803.)
- The undocumented keyword argument syntax for creating `NamedTuple` classes (for example,`Point = NamedTuple("Point", x=int, y=int)` ) is no longer supported.
Use the class-based syntax or the functional syntax instead.
(Contributed by BÃ©nÃ©dikt Tran in gh-133817.)
- Using `TD = TypedDict("TD")` or`TD = TypedDict("TD", None)` to
construct a`TypedDict` type with zero fields is no
longer supported. Use`class TD(TypedDict): pass` or`TD = TypedDict("TD", {})` instead.
(Contributed by BÃ©nÃ©dikt Tran in gh-133823.)
- Deprecated `typing.no_type_check_decorator` has been removed.
(Contributed by Nikita Sobolev in gh-133601.)

### waveÂ¶

- Removed the `getmark()` ,`setmark()` and`getmarkers()` methods
of the`Wave_read` and`Wave_write` classes,
which were deprecated since Python 3.13.
(Contributed by BÃ©nÃ©dikt Tran in gh-133873.)

### zipimportÂ¶

- Remove deprecated `zipimport.zipimporter.load_module()` .
Use`zipimport.zipimporter.exec_module()` instead.
(Contributed by Jiahao Li in gh-133656.)

## DeprecatedÂ¶

### New deprecationsÂ¶

- `ast` :
- `base64` :
  - Accepting the `+` and`/` characters with an alternative alphabet in`b64decode()` and`urlsafe_b64decode()` is now
deprecated.
In future Python versions they will be errors in the strict mode and
discarded in the non-strict mode.
(Contributed by Serhiy Storchaka in gh-125346.)
- CLI: 
  - Deprecate `-b` and`-bb` command-line options
and schedule them to become no-ops in Python 3.17.
These were primarily helpers for the Python 2 -> 3 transition.
Starting with Python 3.17, no`BytesWarning` will be raised
for these cases; use a type checker instead.(Contributed by Nikita Sobolev in gh-136355.)
- `collections.abc` :
  - The following statements now cause `DeprecationWarning` s to be emitted
at runtime:
    - `from collections.abc import ByteString`
    - `import collections.abc; collections.abc.ByteString` .
 `DeprecationWarning` s were already emitted if`collections.abc.ByteString` was subclassed or used as the second
argument to`isinstance()` or`issubclass()` , but warnings were not
previously emitted if it was merely imported or accessed from the`collections.abc` module.
(Contributed by Alex Waygood in gh-118803.)
- `hashlib` :
  - In hash function constructors such as `new()` or the
direct hash-named constructors such as`md5()` and`sha256()` , the optional initial data parameter could
also be passed as a keyword argument named`data=` or`string=` in
various`hashlib` implementations.Support for the `string` keyword argument name is now deprecated and
is slated for removal in Python 3.19. Prefer passing the initial data as
a positional argument for maximum backwards compatibility.(Contributed by BÃ©nÃ©dikt Tran in gh-134978.)
- `http.cookies` :
  - `Morsel.js_output` and`BaseCookie.js_output` are
deprecated and will be removed in Python 3.19. Use`Morsel.output` or`BaseCookie.output` instead.
(Contributed by kishorhange111 in gh-148849.)
- `imaplib` :
  - Altering `IMAP4.file` is now deprecated
and slated for removal in Python 3.19. This property is now unused
and changing its value does*not* explicitly close the current file.
(Contributed by BÃ©nÃ©dikt Tran in gh-142307.)
- `profile` :
  - The `profile` module is deprecated and will be removed in
Python 3.17. Use`profiling.tracing` instead, which provides a
compatible API. See**PEP 799** for details.
(Contributed by Pablo Galindo and LÃ¡szlÃ³ Kiss KollÃ¡r in gh-138122.)
- `re` :
  - `re.match()` and`re.Pattern.match()` are now
soft deprecated in favor of the new`re.prefixmatch()` and`re.Pattern.prefixmatch()` APIs, which have been added as alternate,
more explicit names. These are intended to be used to alleviate confusion
around what*match* means by following the Zen of Pythonâs*âExplicit is
better than implicitâ* mantra. Most other language regular expression
libraries use an API named*match* to mean what Python has always called*search* .We **do not** plan to remove the older`match()` name, as it has been
used in code for over 30 years. Code supporting older versions of Python
should continue to use`match()` , while new code should prefer`prefixmatch()` . See prefixmatch() vs. match().(Contributed by Gregory P. Smith in gh-86519 and Hugo van Kemenade in gh-148100.)
- `struct` :
  - Calling `Struct.__new__()` without a required argument is now
deprecated and will be removed in Python 3.20.  Calling the`__init__()` method on an initialized`Struct` object is deprecated and will be removed in Python 3.20.(Contributed by Sergey B Kirpichev and Serhiy Storchaka in gh-143715.)
  - The `'F'` and`'D'` type codes are now soft deprecated
in favor of the two-letter forms`'Zf'` and`'Zd'` .
(Contributed by Sergey B Kirpichev in gh-121249.)
- `typing` :
  - The following statements now cause `DeprecationWarning` s to be emitted
at runtime:
    - `from typing import ByteString`
    - `import typing; typing.ByteString` .
 `DeprecationWarning` s were already emitted if`typing.ByteString` was subclassed or used as the second argument to`isinstance()` or`issubclass()` , but warnings were not previously emitted if it was
merely imported or accessed from the`typing` module.
(Contributed by Alex Waygood in gh-118803.)
  - It is deprecated to call `isinstance()` and`issubclass()` checks on
protocol classes that were not explicitly decorated with`runtime_checkable()` but that inherit from a runtime-checkable protocol class.
This will raise a`TypeError` in Python 3.20.(Contributed by Bartosz SÅawecki in gh-132604.)
- `webbrowser` :
  - `webbrowser.MacOSXOSAScript` is deprecated in favour of`webbrowser.MacOS` and scheduled for removal in Python 3.17.
(Contributed by Jeff Lyon in gh-137586.)
- `__version__` :
  - The `__version__` ,`version` and`VERSION` attributes have been
deprecated in these standard library modules and will be removed in
Python 3.20. Use`sys.version_info` instead.
    - `argparse`
    - `csv`
    - `ctypes`
    - `ctypes.macholib`
    - `decimal` (use`decimal.SPEC_VERSION` instead)
    - `http.server`
    - `imaplib`
    - `ipaddress`
    - `json`
    - `logging` (`__date__` also deprecated)
    - `optparse`
    - `pickle`
    - `platform`
    - `re`
    - `socketserver`
    - `tabnanny`
    - `tarfile`
    - `tkinter.font`
    - `tkinter.ttk`
    - `wsgiref.simple_server`
    - `xml.etree.ElementTree`
    - `xml.sax.expatreader`
    - `xml.sax.handler`
    - `zlib`
 (Contributed by Hugo van Kemenade and Stan Ulbrych in gh-76007.)

### Pending removal in Python 3.16Â¶

- The import system: 
  - Setting `__loader__` on a module while
failing to set`__spec__.loader` is deprecated. In Python 3.16,`__loader__` will cease to be set or
taken into consideration by the import system or the standard library.
  - Setting `__package__` on a module while
failing to set`__spec__.parent` is deprecated. In Python 3.16,`__package__` will cease to be
taken into consideration by the import system or standard library. (gh-97879)
- The bundled copy of `libmpdec` .
- `array` :
  - The `'u'` format code (`wchar_t` )
has been deprecated in documentation since Python 3.3
and at runtime since Python 3.13.
Use the`'w'` format code (`Py_UCS4` )
for Unicode characters instead.
- `asyncio` :
  - `asyncio.iscoroutinefunction()` is deprecated
and will be removed in Python 3.16;
use`inspect.iscoroutinefunction()` instead.
(Contributed by Jiahao Li and Kumar Aditya in gh-122875.)
  - `asyncio` policy system is deprecated and will be removed in Python 3.16.
In particular, the following classes and functions are deprecated:
    - `asyncio.AbstractEventLoopPolicy`
    - `asyncio.DefaultEventLoopPolicy`
    - `asyncio.WindowsSelectorEventLoopPolicy`
    - `asyncio.WindowsProactorEventLoopPolicy`
    - `asyncio.get_event_loop_policy()`
    - `asyncio.set_event_loop_policy()`
 Users should use `asyncio.run()` or`asyncio.Runner` with*loop_factory* to use the desired event loop implementation.For example, to use `asyncio.SelectorEventLoop` on Windows:import asyncio async def main(): ... asyncio.run(main(), loop_factory=asyncio.SelectorEventLoop) (Contributed by Kumar Aditya in gh-127949.)
- `builtins` :
  - Bitwise inversion on boolean types, `~True` or`~False` has been deprecated since Python 3.12,
as it produces surprising and unintuitive results (`-2` and`-1` ).
Use`not x` instead for the logical negation of a Boolean.
In the rare case that you need the bitwise inversion of
the underlying integer, convert to`int` explicitly (`~int(x)` ).
- `functools` :
  - Calling the Python implementation of `functools.reduce()` with*function* or*sequence* as keyword arguments has been deprecated since Python 3.14.
- `logging` :
  - Support for custom logging handlers with the *strm* argument is deprecated
and scheduled for removal in Python 3.16. Define handlers with the*stream* argument instead. (Contributed by Mariusz Felisiak in gh-115032.)
- `mimetypes` :
  - Valid extensions start with a â.â or are empty for `mimetypes.MimeTypes.add_type()` .
Undotted extensions are deprecated and will
raise a`ValueError` in Python 3.16.
(Contributed by Hugo van Kemenade in gh-75223.)
- `shutil` :
  - The `ExecError` exception
has been deprecated since Python 3.14.
It has not been used by any function in`shutil` since Python 3.4,
and is now an alias of`RuntimeError` .
- `symtable` :
  - The `symtable.Class.get_methods()` method
has been deprecated since Python 3.14.
- `sys` :
  - The `_enablelegacywindowsfsencoding()` function
has been deprecated since Python 3.13.
Use the`PYTHONLEGACYWINDOWSFSENCODING` environment variable instead.
- `sysconfig` :
  - The `sysconfig.expand_makefile_vars()` function
has been deprecated since Python 3.14.
Use the`vars` argument of`sysconfig.get_paths()` instead.
- `tarfile` :
  - The undocumented and unused `TarInfo.tarfile` attribute
has been deprecated since Python 3.13.

### Pending removal in Python 3.17Â¶

- `datetime` :
  - `strptime()` calls using a format string containing`%e` (day of month) without a year.
This has been deprecated since Python 3.15.
(Contributed by Stan Ulbrych in gh-70647.)
- `collections.abc` :
  - `collections.abc.ByteString` is scheduled for removal in Python 3.17.Use `isinstance(obj, collections.abc.Buffer)` to test if`obj` implements the buffer protocol at runtime. For use
in type annotations, either use`Buffer` or a union
that explicitly specifies the types your code supports (e.g.,`bytes | bytearray | memoryview` ).`ByteString` was originally intended to be an abstract class that
would serve as a supertype of both`bytes` and`bytearray` .
However, since the ABC never had any methods, knowing that an object was an
instance of`ByteString` never actually told you anything useful
about the object. Other common buffer types such as`memoryview` were also never understood as subtypes of`ByteString` (either at
runtime or by static type checkers).See **PEP 688** for more details.
(Contributed by Shantanu Jain in gh-91896.)
- `encodings` :
  - Passing non-ascii *encoding* names to`encodings.normalize_encoding()` is deprecated and scheduled for removal in Python 3.17.
(Contributed by Stan Ulbrych in gh-136702.)
- `profile` :
  - The `profile` module is deprecated and will be removed in
Python 3.17. Use`profiling.tracing` instead, which provides a
compatible API. See**PEP 799** for details.
(Contributed by Pablo Galindo and LÃ¡szlÃ³ Kiss KollÃ¡r in gh-138122.)
- `webbrowser` :
  - `webbrowser.MacOSXOSAScript` is deprecated in favour of`webbrowser.MacOS` . (gh-137586)
- `typing` :
  - Before Python 3.14, old-style unions were implemented using the private class `typing._UnionGenericAlias` . This class is no longer needed for the implementation,
but it has been retained for backward compatibility, with removal scheduled for Python
3.17. Users should use documented introspection helpers like`typing.get_origin()` and`typing.get_args()` instead of relying on private implementation details.
  - `typing.ByteString` , deprecated since Python 3.9, is scheduled for removal in
Python 3.17.Use `isinstance(obj, collections.abc.Buffer)` to test if`obj` implements the buffer protocol at runtime. For use
in type annotations, either use`Buffer` or a union
that explicitly specifies the types your code supports (e.g.,`bytes | bytearray | memoryview` ).`ByteString` was originally intended to be an abstract class that
would serve as a supertype of both`bytes` and`bytearray` .
However, since the ABC never had any methods, knowing that an object was an
instance of`ByteString` never actually told you anything useful
about the object. Other common buffer types such as`memoryview` were also never understood as subtypes of`ByteString` (either at
runtime or by static type checkers).See **PEP 688** for more details.
(Contributed by Shantanu Jain in gh-91896.)
- `tkinter` :
