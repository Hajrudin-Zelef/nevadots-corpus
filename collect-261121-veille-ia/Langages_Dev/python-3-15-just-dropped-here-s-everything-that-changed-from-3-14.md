---
id: collect-261121-veille-ia/langages-dev/python-3-15-just-dropped-here-s-everything-that-changed-from-3-14
title: "Python 3.15 just dropped. Here's everything that changed from 3.14"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["lazy", "imports", "python", "import", "json", "just", "everything", "module"]
source: https://dev.to/mursalfk/python-315-just-dropped-heres-everything-that-changed-from-314-36kj
source_anchor: ""
source_lines: [1, 354]
section: "Langages_Dev"
---
Python 3.15 landed on 9 October 2026 and okay, this one is actually a lot. We got a `lazy` keyword for imports, two new builtin types, UTF-8 as the default encoding, a real sampling profiler in the standard library, `*` and `**` inside comprehensions, a JIT that's finally getting somewhere, and the usual pile of removals that will ruin somebody's Friday.

**I went through the release notes and the deprecations page so you don't have to.**

Everything below is grouped by what it means for your code, not by whatever order the changelog happened to list it. Every item has a short explanation, an example where one helps, and a **Timeline** line telling you when it landed, or when something you rely on stops working.

The interactive diagrams under each section are there so you can poke at the idea instead of just reading about it. Click stuff. It's the fastest way to get it.

Slow startup is one of those problems everybody has and nobody fixes, because the usual fix is ugly: shove `import` statements inside functions and hope your linter doesn't yell. Python 3.15 gives you a proper answer. A new `lazy` soft keyword tells Python "don't actually load this module until somebody uses it."

You still write your imports at the top of the file like a normal person. Python just doesn't pay for them until the name is touched. If a run never reaches the code that needs `json`, `json` never loads.


```
lazy import json
lazy from pathlib import Path
print("Starting up...")              # neither module is fully imported yet
data = json.loads('{"key": "value"}')  # json loads here
p = Path(".")                          # pathlib loads here
```

This is huge for CLI tools, where you pay the whole import bill just to print `--help`, and for anything that starts a lot, like serverless functions and test runners. The load costs in the diagram above are illustrative, so don't quote them anywhere. The idea is real though: pay only for what a run touches.

How you control it:

- 
**Global switch:**`-X lazy_imports=all|normal` or the`PYTHON_LAZY_IMPORTS` environment variable.`normal` is the default and only honours the`lazy` keyword.
- 
**Runtime control:**`sys.set_lazy_imports()` ,`sys.get_lazy_imports()` , and`sys.set_lazy_imports_filter()` if you want to decide per module.
- 
**Backport-friendly form:** set`__lazy_modules__ = ["json", "pathlib"]` and plain`import` statements for those modules become lazy. This is the one to use if you still support older Pythons.

Things that will bite you:

- Errors show up at first use, not at import time. A typo'd or missing module won't blow up on line 1 anymore, it'll blow up later. The traceback does include the original import line, so you can still find it.
- 
`lazy` only works at module scope. Inside a function, a class, or a`try` /`except` /`finally` it's a`SyntaxError` . Star imports and`__future__` imports can't be lazy either.

**Timeline:** available now in 3.15. Use `__lazy_modules__` if you must still support older versions.

For years the "immutable dict" move was either `types.MappingProxyType` (a read-only view, not actually frozen) or rolling your own. Now there's a real builtin. A `frozendict` can't be changed after you make it, and it's hashable as long as everything inside it is hashable, which means you can finally use one as a dict key or drop it in a set.

Two things that surprise people. It is not a `dict` subclass, it inherits from `object`. And it remembers insertion order, but equality ignores order, so two frozendicts with the same items are equal no matter how you built them.


```
>>> a = frozendict(x=1, y=2)
>>> a['z'] = 3
TypeError: 'frozendict' object does not support item assignment
>>> b = frozendict(y=2, x=1)
>>> hash(a) == hash(b), a == b
(True, True)
```

It plays nice with the rest of the stdlib: `copy`, `decimal`, `json`, `marshal`, `plistlib` (serialize only), `pickle`, `pprint`, and `xml.etree.ElementTree` all accept it, and `eval()`/`exec()` take one for globals. It also pairs well with the new `json.loads(..., object_pairs_hook=frozendict, array_hook=tuple)`, which gives you a fully immutable parsed document. Config files that nobody can accidentally mutate. Love that.

The one migration trap: if you check `isinstance(x, dict)`, a frozendict will fail it. Switch to `isinstance(x, (dict, frozendict))`, or better, check against `collections.abc.Mapping`.

**Timeline:** available now in 3.15.

You've written this a hundred times: `_MISSING = object()`, then `if arg is _MISSING`. It works, but the repr is a useless memory address, copying it can break the identity check, and type checkers have no idea what to do with it.

`sentinel` is the grown-up version. You get a unique marker with a readable name. It keeps its identity when copied, works in type expressions with `|`, and pickles when it's importable by module and name.


```
MISSING = sentinel("MISSING")
def fetch(key, default=MISSING):
    ...
```

Flip the toggle in the diagram. The old `object()` trick fails all three probes, which is a pretty good argument for switching. It's most useful anywhere `None` is a legitimate value and you need to tell "they passed None" apart from "they passed nothing."

**Timeline:** available now in 3.15.

This is the one most likely to break something, so read it twice. `open("file.txt")` with no `encoding=` now uses UTF-8 no matter what your system locale says. Before, Python quietly used whatever your OS preferred, which on a lot of Windows machines meant cp1252. Same code, different behaviour depending on whose laptop it ran on. That's the bug class this kills.

If your files are already UTF-8, nothing changes except it now works everywhere. If you've got legacy cp1252 files and relied on the default, a bare `open()` will now raise a `UnicodeDecodeError`, or worse, decode wrong without complaining. Mess with the three selectors in the diagram to see all four outcomes.

What to do about it:

- Always pass `encoding=` explicitly in code that has to run across versions.
- 
`encoding="locale"` opts you into the system encoding on purpose (supported since 3.10).
- Find the call sites that depend on the old default with the opt-in encoding warning, `-X warn_default_encoding` .
- Need the old behaviour back for now? `PYTHONUTF8=0` or`-X utf8=0` restores it.


**Timeline:** effective in 3.15.

Flattening a list of lists used to mean a nested comprehension that reads backwards, or `itertools.chain.from_iterable`, or a loop. Now `*` and `**` work directly inside list, set, and dict comprehensions and generator expressions.


```
>>> lists = [[1, 2], [3, 4], [5]]
>>> [*L for L in lists]
[1, 2, 3, 4, 5]
>>> dicts = [{'a': 1}, {'b': 2}, {'a': 3}]
>>> {**d for d in dicts}
{'a': 3, 'b': 2}
>>> list(x for x in (*L for L in lists))
[1, 2, 3, 4, 5]
```

For the dict version, later keys win, which is why `'a'` ends up as `3`. Async generator expressions get the same treatment, so `(*a async for a in agen())` is valid. It's a small quality of life change, but it's the kind you'll use weekly.

**Timeline:** available now in 3.15.

Profiling in Python used to mean restart the app under `cProfile`, eat the slowdown, and hope you reproduce the problem. Tachyon flips that. It's a statistical sampling profiler that attaches to a process that's already running, by PID, with no restart and no code changes. Sampling rates go up to 1,000,000 Hz.

To make room for it there's a new `profiling` package that groups the built-in profilers:

- 
`profiling.tracing` is the deterministic profiler, moved over from`cProfile` (which stays as an alias).
- 
`profiling.sampling` is Tachyon.

Modes are `attach`, `run`, `run -m`, and `dump`, which is a one-shot stack snapshot of every thread, or of all asyncio tasks with `--async-aware`. You can measure wall time (the default, `--mode wall`), CPU time, GIL-holding time, or exception-handling time. Output formats cover pretty much every tool people actually use: `--pstats`, `--collapsed` for flame-graph tools, `--flamegraph` (a self-contained HTML file), `--gecko` for the Firefox Profiler, and `--heatmap` for line-level data. There's also a live `top`-style TUI with `--live` and opcode-level detail with `--opcodes`.

Drag the sample rate in the diagram. At 1 Hz you miss the short function completely, and that's the whole tradeoff of sampling in one picture.


**Timeline:** Tachyon and `profiling` ship in 3.15. The old pure-Python `profile` module is deprecated and **removed in 3.17**. Use `profiling.tracing`, which has a compatible API.

This one is invisible until you need it, and then it's the whole game. CPython is now built with frame pointers on platforms that support them. In plain terms, system profilers, debuggers, crash tools, and eBPF observability can walk the native stack faster and more reliably, without guessing.

The catch is that stack unwinding is only as good as its weakest link. If you build C, C++, or Rust extensions with your own toolchain, keep frame pointers on. One native component without them can break unwinding for the whole process, which is exactly what the diagram shows when you click a layer off. The flags are exposed through `sysconfig`, so extension builds can inherit them. If you really want out of the CPython default, there's `--without-frame-pointers`.


**Timeline:** default from 3.15.

Quick background. `.pth` files in `site-packages` can run arbitrary code at interpreter startup, through any line that begins with `import`. It's powerful and honestly kind of scary, because it's hard to audit what's running before your program even starts.

3.15 adds `.start` files. They hold entry points in `pkg.mod:callable` form, and Python calls each callable at startup. When a matching `.start` file exists, the `import` lines in the `.pth` file are ignored. Plain path-extension lines in `.pth` files don't change.

Slide the version control in the diagram to watch the `.pth` import line go from "works" to "ignored" to "warns."


**Timeline:** `.start` files arrive in 3.15, and `import` lines in `.pth` files are silently deprecated now. Per the 3.15 deprecations page, they're silently ignored in **3.18**, and in **3.20** they produce warnings and `.pth` files must be `utf-8-sig` encoded. If you ship a package that relies on a `.pth` import line, you've got time, but not infinite time.

If you write C extensions, this matters. Until now, the stable ABI (`abi3`) didn't cover free-threaded CPython, so you ended up building separate wheels. 3.15 adds `abi3t`, a Stable ABI compatible with free-threaded builds.

The price of admission is moving to the PEP 697 type APIs and the new `PyModExport_*` module hook with the `PySlot` structure. `meson-python`, `scikit-build-core`, and Maturin already support it. You can pick it manually with the `Py_TARGET_ABI3T` macro. If your extension can't make the jump yet, keep building `abi3` plus a separate `cp315t` wheel and move over later.


**Timeline:** available now in 3.15.

Shutting down an interpreter while another thread is still poking at it is a classic way to get deadlocks and crashes. 3.15 adds interpreter guards, interpreter views, and attach/detach APIs so extensions can say "I'm still using this, don't finalize yet."

As part of that, the `PyGILState_*` family is now soft deprecated. Soft deprecated means "we'd prefer you don't use this in new code," and there's no plan to remove it, so nothing breaks.

There's also a handful of other C API additions worth knowing about: `PyBytesWriter` (PEP 782), `PyTuple_FromArray`, `PyArg_ParseArray`, `PyAnyDict_*` and `PyFrozenDict_*`, `PyObject_Dump`, and the `PySys_GetAttr` family. Click the chips in the diagram for a one-line description of each.


**Timeline:** available now in 3.15; no removal scheduled.

The experimental JIT got a real glow-up this release. It now needs LLVM 21 to build, has a new tracing frontend that records the paths your code really takes, basic register allocation, more constant propagation, and reference-count elimination where it's safe. GDB and `backtrace()` can also unwind through JIT frames now, which makes debugging a lot less painful.

The numbers from the release notes: an 8 to 9% geometric-mean gain on pyperformance (x86-64 Linux) over the standard interpreter, and 12 to 13% on AArch64 macOS over the tail-calling interpreter. Against no-JIT builds the spread is wide, anywhere from roughly a 15% slowdown to over 100% speedup. The source text flags these figures as not final, so treat them as a vibe check and benchmark your own workload before you tell anyone it's faster.


**Timeline:** still experimental in 3.15.

Official 64-bit Windows binaries now use the tail-calling interpreter (built with MSVC 18 / Visual Studio 2026). That's reported at a 15 to 20% geometric-mean speedup over the old switch-based interpreter, with 14% to 40% showing up across real workloads. And official macOS binaries now install free-threading support by default. So if you're on either, you got faster for free.

A bunch of boring-but-great stuff:

- 
`base64` : encoding is about 2x faster, decoding about 3x faster.
- Ascii85, Base85, Z85 and Base32 were rewritten in C. That's roughly two orders of magnitude faster, and the Base85 family also uses far less memory.
- 
`csv.Sniffer.sniff()` : up to 1.6x faster delimiter detection.
- 
`mimalloc` is the default raw allocator on free-threaded builds.
- 
`subprocess.Popen.wait(timeout=...)` is now event-driven (`pidfd_open` on Linux 5.3+,`kqueue` on macOS/BSD) instead of a busy loop. Less CPU burned waiting around.
- Class creation is faster thanks to shared `__dict__` /`__weakref__` descriptors.


Remember the incremental garbage collector from early 3.14? It's gone. After reports of memory pressure, the team reverted to the generational GC. 3.14.5+ and 3.15 both use the generational collector.

If you live in type hints, this section is for you. Four things landed:

- 
**`TypeForm` (PEP 747):** lets you annotate values that are themselves type expressions, like`def cast[T](typ: TypeForm[T], value: Any) -> T: ...` . At runtime,`TypeForm(x)` just returns`x` .
- 
**Closed and extra-item `TypedDict` (PEP 728):**`closed=True` forbids keys you didn't declare, and`extra_items=<type>` allows extras of one specific type. Play with the selector in the diagram to see what a checker would say.
- 
**`@typing.disjoint_base` (PEP 800):** lets type checkers model builtins and extension types that can't be multiply inherited.
- 
**`TypeVarTuple`** now accepts`bound` ,`covariant` ,`contravariant` , and`infer_variance` .

There's also a protocol fix. `class X(P1[S], Protocol[T, T2])` with `S` missing now raises `TypeError`, and type-parameter order is inferred correctly.


**Timeline:** all available in 3.15.

The small stuff that makes daily Python nicer:

- 
**Better error messages.** Nested attribute hints (`Did you mean '.inner.area' instead of '.area'?` ). Cross-language hints for people whose muscle memory is from JavaScript:`[].push` suggests`.append` ,`'x'.toUpperCase` suggests`.upper` , and`{}.put` tells you to use`d[k] = v` . There are also mutable-on-immutable hints for tuples and suggestions for`delattr()` . Pick one in the diagram.
- 
**More colour across the CLI:**`python --help` , unraisable exceptions,`argparse` ,`ast` ,`calendar` ,`difflib` ,`http.server` ,`pickletools` ,`sqlite3` ,`timeit` ,`tokenize` , and plenty of stdlib CLI help screens.
- 
**`bytearray.take_bytes(n=None)`** moves bytes out of a buffer without copying them.
- 
**`slice` is subscriptable** now (it's a generic type).`__slots__` can include`__dict__` and`__weakref__` on any class, and tuple subclasses can have any slots.
- 
**`match` statements:** literal patterns accept unary plus.
- 
**Import deadlock fixed:** per-module locks are now taken in parent-before-child order.
- 
**`-W` and `PYTHONWARNINGS`** accept regular expressions when a field is wrapped in`/.../` .
- 
**Timestamps and timeouts** accept any real number, so`Decimal` and`Fraction` work.
- 
**Interactive shell:** completions are coloured by kind, and`from x import <Tab>` suggests module attributes.`pdb` uses the new shell too.


Integer-specific maths functions now live in their own module instead of being mixed in with the float stuff.

| Module | Change | 
|---|---|
| `asyncio` | `TaskGroup.cancel()` ends a group early without the old exception-raising boilerplate | 
| `re` | `re.prefixmatch()` /`Pattern.prefixmatch()` added as the explicit name for`match()` ;`match()` is soft deprecated with no removal planned | 
| `tomllib` | TOML 1.1.0: multiline inline tables, trailing commas, `\xHH` and`\e` escapes, optional seconds | 
| `unicodedata` | Unicode 17.0.0; `iter_graphemes()` ,`block()` , XID helpers | 
| `threading` | `serialize_iterator` ,`synchronized_iterator()` ,`concurrent_tee()` for sharing iterators across threads | 
| `contextlib` | `@contextmanager` used on generator/async functions now stays open across iteration or await | 
| `base64` /`binascii` | New `padded` ,`wrapcol` ,`ignorechars` ,`canonical` ,`alphabet` options; Base32/Ascii85/Base85/Z85 in`binascii` | 
| `json` | `array_hook` parameter on`load` /`loads` | 
| `pprint` | `expand=True` for JSON-style layout; t-string support | 
| `os` /`pathlib` | `os.statx()` (Linux);`parent_mode` on`os.makedirs()` and`Path.mkdir()` | 
| `os.path` | `realpath(strict=os.path.ALLOW_MISSING)` | 
| `ssl` | `set_groups()` , post-quantum group support,`set_ciphersuites()` , signature-algorithm APIs | 
| `argparse` | `suggest_on_error` now defaults to`True` ; backtick markup in help text | 
| `http.server` | `--header/-H` ,`--content-type` ,`extra_response_headers` | 
| `sqlite3` CLI | SQL keyword, table, column, and schema tab completion | 
| `zlib` | `adler32_combine()` ,`crc32_combine()` | 
| `unittest` | `assertWarns` /`assertWarnsRegex` no longer swallow non-matching warnings | 
| `tarfile` | Multiple extraction-filter hardening fixes (several CVEs) | 
| `array` /`memoryview` /`ctypes` | Complex ( `Zf` ,`Zd` ) and half-float (`e` ) formats | 
| `locale` | `getdefaultlocale()` un-deprecated | 

A few worth a closer look. `re.prefixmatch()` is just the honest name for what `match()` always did: anchor at the start of the string. People mix up `match` and `search` constantly, so the new name saves a few confused Stack Overflow questions. `match()` isn't going anywhere, it's only soft deprecated. Type any pattern and string into the diagram to see how `prefixmatch` and `search` differ. `asyncio.TaskGroup.cancel()` is the one I'd grab first if you write async code, since ending a group early used to need awkward boilerplate. And `tarfile` got several security fixes, so update if you handle untrusted archives.


These were deprecated earlier and are now gone. If your CI goes red after upgrading, start here:

- 
`ast` : constructing nodes with missing or unknown fields raises`TypeError` (it's been warning since 3.13).
- 
`collections.abc.ByteString` and`typing.ByteString` dropped from`__all__` (full removal is still due in 3.17).
- 
`ctypes.SetPointerType()` .
- 
`datetime.strptime()` with`%d` and no year now raises`ValueError` .
- 
`glob.glob0()` and`glob.glob1()` .
- 
`http.server.CGIHTTPRequestHandler` and the`--cgi` flag.
- 
`importlib.resources.files(package=...)` parameter.
- 
`pathlib.PurePath.is_reserved()` . Use`os.path.isreserved()` instead.
- 
`platform.java_ver()` .
- 
`sre_compile` ,`sre_constants` ,`sre_parse` .
- 
`sysconfig.is_python_build(check_home=...)` .
- C `RLock` arbitrary args.
- 
`types.CodeType.co_lnotab` .
- 
`typing` : keyword-argument`NamedTuple` syntax, zero-field`TypedDict("TD")` , and`no_type_check_decorator` .
- 
`wave` :`getmark()` ,`setmark()` ,`getmarkers()` .
- 
`zipimport.zipimporter.load_module()` .
- 
`module.__cached__` is no longer set or consulted. Use`__spec__.cached` .
- C API: the `PyUnicode_AsDecodedObject` family,`PyImport_ImportModuleNoBlock` ,`PyWeakref_GetObject` ,`PySys_ResetWarnOptions` , and the`Py_GetPath` /`Py_GetPrefix` /`Py_GetExecPrefix` /`Py_GetProgramFullPath` /`Py_GetProgramName` /`Py_GetPythonHome` initialization getters. Use`PyConfig_Get` for those.

This is the part to screenshot. What's deprecated, and when it goes. Plan migrations against this table, and use the search box in the diagram to check if your own imports show up.


| Removal | What goes | 
|---|---|
| **3.16** | `asyncio` policy system (`AbstractEventLoopPolicy` ,`DefaultEventLoopPolicy` ,`Windows*EventLoopPolicy` ,`get/set_event_loop_policy` ) and`asyncio.iscoroutinefunction()` ; bitwise`~True` /`~False` ;`array` type code`'u'` ;`logging` handlers using`strm` ;`mimetypes.add_type()` with undotted extensions (becomes`ValueError` );`__loader__` taken into account by the import system;`functools.reduce` keyword args in the Python implementation;`shutil.ExecError` ;`symtable.Class.get_methods` ;`sys._enablelegacywindowsfsencoding()` ;`sysconfig.expand_makefile_vars()` ;`TarFile.tarfile` ; bundled`libmpdec` (C) | 
| **3.17** | `profile` module;`collections.abc.ByteString` and`typing.ByteString` ;`datetime.strptime()` with`%e` and no year;`webbrowser.MacOSXOSAScript` ;`tkinter.Variable.trace_variable()` /`trace()` /`trace_vdelete()` /`trace_vinfo()` ; non-ASCII names in`encodings.normalize_encoding()` ;`typing._UnionGenericAlias` ;`-b` /`-bb` become no-ops;`PyConfig.bytes_warning` (C) | 
| **3.18** | Boolean values accepted as file descriptors; `Decimal``'N'` format specifier;`import` lines in`.pth` files ignored (PEP 829); private C helpers (`_PyBytes_Join` ,`_PyUnicodeWriter_*` ,`_PyLong_New` , and others) | 
| **3.19** | `ctypes` implicit MSVC struct layout from`_pack_` without`_layout_` ;`hashlib``string=` keyword;`http.cookies``js_output()` ;`imaplib.IMAP4.file` assignment; PEP 456 external hash definition (C) | 
| **3.20** | `__version__` /`version` /`VERSION` attributes in about two dozen stdlib modules (use`sys.version_info` ); abstract`ast` node instantiation;`isinstance()` /`issubclass()` on non-`runtime_checkable` protocols inheriting from runtime-checkable ones;`struct.Struct.__new__` without`format` ; C:`_PyObject_CallMethodId` ,`_PyObject_GetAttrId` ,`_PyUnicode_FromId` ,`PyComplexObject.cval` ,`Py_MATH_El` ,`Py_MATH_PIl` | 
| **No date** | `datetime.utcnow()` /`utcfromtimestamp()` ,`logging.warn()` ,`threading` camelCase aliases,`codecs.open()` ,`os.path.commonprefix()` ,`urllib.parse.split*()` helpers,`typing.Text` ,`argparse.FileType` ,`ssl.PROTOCOL_*` constants,`shutil.rmtree(onerror=)` , and more | 

The 3.16 column is the one that'll hurt most people. The `asyncio` policy system is going, and a lot of older async code touches it without the author even knowing. Check that one first.

`base64.b64decode()` / `urlsafe_b64decode()` accepting `+` and `/` with an alternative alphabet; the `-b`/`-bb` CLI flags; runtime warnings on importing `ByteString`; `hashlib` `string=`; `http.cookies.*.js_output()`; `imaplib.IMAP4.file`; the `__version__` attributes; `webbrowser.MacOSXOSAScript`; abstract `ast` node instantiation; `Struct.__new__`/`__init__` misuse.

`re.match()` / `Pattern.match()` (prefer `prefixmatch()` in new code, keep `match()` if you support older versions); `struct` codes `'F'`/`'D'` (prefer `'Zf'`/`'Zd'`); `PyGILState_*`; the `PyType_FromSpec*` family (prefer `PySlot` / `PyType_FromSlots`); and several legacy C macros.

Tick these off in the diagram as you go. It remembers your progress in your browser, when the page lets it.


1. 
**Default encoding is UTF-8.** Audit every`open()` ,`read_text()` , and`write_text()` without`encoding=` . This is the big one.
2. 
**`argparse`:**`suggest_on_error` is now`True` . Also, with`add_argument('-f', '-foo')` ,`dest` is now`'foo'` (it used to be`'f'` ). Pass`dest=` explicitly to keep the old name.
3. 
**`base64.urlsafe_b64decode()`** no longer requires padding. Pass`padded=True` for the old strictness.
4. 
**`unittest`:**`assertWarns` no longer hides non-matching warnings, so tests may now leak warnings that were previously masked. Your suite might get noisier without anything being newly broken.
5. 
**`sqlite3.connect()`:** every parameter except`database` is keyword-only, and the first parameters of`create_function` ,`create_aggregate` , and the handler setters are positional-only.
6. 
**`resource.RLIM_INFINITY`** is always positive now. Passing the old negative values is deprecated.
7. 
**`importlib.metadata`** raises`MetadataNotFound` instead of returning empty metadata.
8. 
**`array.typecodes`** is now a`tuple` , not a`str` .
9. 
**`ctypes`** complex types changed their`_type_` codes to`Zf` ,`Zd` ,`Zg` .
10. 
**`venv`** on POSIX creates a real`lib64` directory instead of a`lib64 -> lib` symlink.
11. 
**`hashlib`** constructors like`hashlib.md5` always exist as attributes, even without backend support.
12. 
**Build systems:** keep the frame-pointer flags from`sysconfig` , and note that`libmpdec` no longer falls back to the bundled copy implicitly.

For people who package or compile Python:

- New `--with-missing-stdlib-config=FILE` lets distributors ship custom "this module is packaged separately" messages.
- New `--with-pymalloc-hugepages` plus`PYTHON_PYMALLOC_HUGEPAGES=1` at runtime enables 2 MiB huge-page arenas.
- 
`mmap.set_name()` annotates anonymous mappings on Linux 5.17+.

3.15 isn't one giant headline. It's three shifts landing together.

**Startup and observability finally got real attention.** `lazy` imports, Tachyon, frame pointers, and JIT unwinding all point the same way: Python is getting easier to speed up and easier to diagnose in production, without rewriting anything.

**Python grew the primitives people were hand-rolling for years.** `frozendict` and `sentinel` are the "why didn't we have this already" additions. Expect to delete some homemade helper classes.

**Encoding and import behaviour got stricter and more predictable.** UTF-8 by default, `.start` files, and a long, dated list of removals. Less "works on my machine," more "works the same everywhere."

If you only do one thing this week, run your test suite on 3.15 with `-X warn_default_encoding` and `-W error::DeprecationWarning`. Then check your code against the 3.16 column of the removal calendar. Future you will be thankful.

Sources: What's new in Python 3.15 · Python 3.15 deprecations · PEP 790: 3.15 release schedule

## Top comments (2)

Nice one, i am curious how was AI agents will know the diff between old and new versions🙃

Thanks! 😄 Great question... by default they won’t. A model only knows what was in its training data, so a brand-new release like 3.15 is a blind spot. The agents that get it right pull the release notes or docs at runtime, check the project’s pinned Python version, and run the tests to see what breaks. So the real answer is: give them the changelog and let the tests be the judge.
