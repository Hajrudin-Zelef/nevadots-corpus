---
id: collect-261121-veille-ia/docs-techniques/changelog-2-9
title: "Changelog"
domain: docs-techniques
role: reference
task: article
actors: []
dates: ["2023-04-04"]
keywords: ["patch", "test", "python", "http", "request", "configure", "tests", "bolt"]
source: https://docs.python.org/release/3.15.1/whatsnew/changelog.html
source_anchor: ""
source_lines: [12001, 13500]
section: "Docs_Techniques"
---
not`/` . Patch by Carey Metcalfe.
- bpo-46797: Deprecation warnings are now emitted for `ast.Num` ,`ast.Bytes` ,`ast.Str` ,`ast.NameConstant` and`ast.Ellipsis` . These have been documented as deprecated since
Python 3.8, and will be removed in Python 3.14.
- bpo-44844: Enables `webbrowser` to detect and launch Microsoft Edge
browser.
- bpo-45606: Fixed the bug in `pathlib.Path.glob()` â previously a
dangling symlink would not be found by this method when the pattern is an
exact match, but would be found when the pattern contains a wildcard or
the recursive wildcard (`**` ). With this change, a dangling symlink will
be found in both cases.
- bpo-23041: Add `QUOTE_STRINGS` and`QUOTE_NOTNULL` to the suite of`csv` module quoting styles.
- bpo-24964: Added `http.client.HTTPConnection.get_proxy_response_headers()` that
provides access to the HTTP headers on a proxy server response to the`CONNECT` request.
- bpo-17258: `multiprocessing` now supports stronger HMAC algorithms
for inter-process connection authentication rather than only HMAC-MD5.
- bpo-39744: Make `asyncio.subprocess.Process.communicate()` close the
subprocessâs stdin even when called with`input=None` .
- bpo-22708: http.client CONNECT method tunnel improvements: Use HTTP 1.1 protocol; send a matching Host: header with CONNECT, if one is not provided; convert IDN domain names to Punycode. Patch by Michael Handler.

### DocumentationÂ¶

- gh-67056: Document that the effect of registering or unregistering an `atexit` cleanup function from within a registered cleanup function
is undefined.
- gh-103629: Mention the new way of typing `**kwargs` with`Unpack` and`TypedDict` introduced in**PEP 692** .
- gh-48241: Clarifying documentation about the url parameter to urllib.request.urlopen and urllib.request.Request needing to be encoded properly.
- gh-86094: Add support for Unicode Path Extra Field in ZipFile. Patch by Yeojin Kim and Andrea Giudiceandrea
- gh-99202: Fix extension type from documentation for compiling in C++20 mode

### TestsÂ¶

- gh-104494: Update `test_pack_configure_in` and`test_place_configure_in` for changes to error message formatting in Tk
8.7.
- gh-104461: Run test_configure_screen on X11 only, since the `DISPLAY` environment variable and`-screen` option for toplevels are not useful
on Tk for Win32 or Aqua.
- gh-86275: Added property-based tests to the `zoneinfo` tests, along
with stubs for the`hypothesis` interface. (Patch by Paul Ganssle)
- gh-103329: Regression tests for the behaviour of `unittest.mock.PropertyMock` were added.
- gh-102795: fix use of poll in test_epollâs test_control_and_wait
- gh-75729: Fix the `os.spawn*` tests failing on Windows
when the working directory or interpreter path contains spaces.

### BuildÂ¶

- gh-101282: BOLT optimization is now applied to the libpython shared library if building a shared library. BOLT instrumentation and application settings can now be influenced via the `BOLT_INSTRUMENT_FLAGS` and`BOLT_APPLY_FLAGS` configure variables.
- gh-99017: `PYTHON_FOR_REGEN` now require Python 3.10 or newer.
- gh-104490: Define `.PHONY` / virtual make targets consistently and
properly.
- gh-104106: Add gcc fallback of mkfifoat/mknodat for macOS. Patch by Donghee Na.
- gh-103532: The `TKINTER_PROTECT_LOADTK` macro is no longer defined or
used in the`_tkinter` module.  It was previously only defined when
building against Tk 8.4.13 and older, but Tk older than 8.5.12 has been
unsupported since gh-91152.
- gh-99069: Extended workaround defining `static_assert` when missing from
the libc headers to all clang and gcc builds. In particular, this fixes
building on macOS <= 10.10.
- gh-100220: Changed the default value of the `SHELL` Makefile variable
from`/bin/sh` to`/bin/sh -e` to ensure that complex recipes
correctly fail after an error. Previously,`make install` could fail to
install some files and yet return a successful result.
- gh-90656: Add platform triplets for 64-bit LoongArch: 
  - loongarch64-linux-gnusf
  - loongarch64-linux-gnuf32
  - loongarch64-linux-gnu
 Patch by Zhang Na.

### WindowsÂ¶

- gh-104623: Update Windows installer to use SQLite 3.42.0.
- gh-82814: Fix a potential `[Errno 13] Permission denied` when using`shutil.copystat()` within Windows Subsystem for Linux (WSL) on a
mounted filesystem by adding`errno.EACCES` to the list of ignored
errors within the internal implementation.
- gh-103088: Fix virtual environment `activate` script having
incorrect line endings for Cygwin.
- gh-103088: Fixes venvs not working in bash on Windows across different disks
- gh-102997: Update Windows installer to use SQLite 3.41.2.
- gh-88013: Fixed a bug where `TypeError` was raised when calling`ntpath.realpath()` with a bytes parameter in some cases.

### macOSÂ¶

- gh-99834: Update macOS installer to Tcl/Tk 8.6.13.
- gh-104623: Update macOS installer to SQLite 3.42.0.
- gh-103545: Add `os.PRIO_DARWIN_THREAD` ,`os.PRIO_DARWIN_PROCESS` ,`os.PRIO_DARWIN_BG` and`os.PRIO_DARWIN_NONUI` . These can be used with`os.setpriority` to run the process at a lower priority and make use of
the efficiency cores on Apple Silicon systems.
- gh-104180: Support reading SOCKS proxy configuration from macOS System Configuration. Patch by Sam Schott.
- gh-60436: update curses textbox to additionally handle backspace using the `curses.ascii.DEL` key press.
- gh-102997: Update macOS installer to SQLite 3.41.2.

### IDLEÂ¶

### Tools/DemosÂ¶

### C APIÂ¶

- gh-101291: Added unstable C API for extracting the value of âcompactâ integers: `PyUnstable_Long_IsCompact()` and`PyUnstable_Long_CompactValue()` .
- gh-104109: Weâve added `Py_NewInterpreterFromConfig()` and`PyInterpreterConfig` to the public C-API (but not the stable ABI; not
yet at least).  The new function may be used to create a new interpreter
with various features configured.  The function was added to support PEP
684 (per-interpreter GIL).
- gh-103968: `PyType_FromSpec()` and its variants now allow creating
classes whose metaclass overrides`tp_new` . The`tp_new` is ignored. This behavior is deprecated and will be disallowed
in 3.14+. The new`PyType_FromMetaclass()` already disallows it.
- gh-103743: Add `PyUnstable_Object_GC_NewWithExtraData()` function
that can be used to allocate additional memory after an object for data
not managed by Python.
- gh-103295: Introduced `PyUnstable_WritePerfMapEntry()` ,`PyUnstable_PerfMapState_Init()` and`PyUnstable_PerfMapState_Fini()` . These allow extension modules (JIT
compilers in particular) to write to perf-map files in a thread safe
manner. The Python support for the perf map compatible profilers also uses these APIs to write
entries in the perf-map file.
- gh-103509: Added C API for extending types whose instance memory layout is opaque: `PyType_Spec.basicsize` can now be zero or negative,`PyObject_GetTypeData()` can be used to get subclass-specific data,
and`Py_TPFLAGS_ITEMS_AT_END` can be used to safely extend
variable-size objects. See**PEP 697** for details.
- gh-103091: Add a new C-API function to eagerly assign a version tag to a PyTypeObject: `PyUnstable_Type_AssignVersionTag()` .
- gh-101408: `PyObject_GC_Resize` should calculate preheader size
if needed. Patch by Donghee Na.
- gh-98836: Add support of more formatting options (left aligning, octals, uppercase hexadecimals, `intmax_t` ,`ptrdiff_t` ,`wchar_t` C strings, variable width and precision) in`PyUnicode_FromFormat()` and`PyUnicode_FromFormatV()` .
- gh-96803: Add unstable C-API functions to get the code object, lasti and line number from the internal `_PyInterpreterFrame` in the limited API.
The functions are:
  - `PyCodeObject * PyUnstable_InterpreterFrame_GetCode(struct _PyInterpreterFrame *frame)`
  - `int PyUnstable_InterpreterFrame_GetLasti(struct _PyInterpreterFrame *frame)`
  - `int PyUnstable_InterpreterFrame_GetLine(struct _PyInterpreterFrame *frame)`

## Python 3.12.0 alpha 7Â¶

*Release date: 2023-04-04*

### Core and BuiltinsÂ¶

- gh-102192: Deprecated `_PyErr_ChainExceptions` in favour of`_PyErr_ChainExceptions1` .
- gh-89987: Reduce the number of inline `CACHE` entries for`BINARY_SUBSCR` .
- gh-102859: Removed `JUMP_IF_FALSE_OR_POP` and`JUMP_IF_TRUE_OR_POP` instructions.
- gh-101975: Fixed `stacktop` value on tracing entries to avoid corruption
on garbage collection.
- gh-102778: Add `sys.last_exc` and deprecate`sys.last_type` ,`sys.last_value` and`sys.last_traceback` , which hold the same
information in its legacy form.
- gh-100982: Replace all occurrences of `COMPARE_AND_BRANCH` with`COMPARE_OP` .
- gh-102701: Fix overflow when creating very large dict.
- gh-102755: Add `PyErr_DisplayException()` which takes just an
exception instance, to replace the legacy`PyErr_Display()` which
takes the`(typ, exc, tb)` triplet.
- gh-102594: Add note to exception raised in `PyErr_SetObject` when
normalization fails.
- gh-90997: Shrink the number of inline `CACHE` entries used by`LOAD_GLOBAL` .

### LibraryÂ¶

- gh-102491: Improve import time of `platform` by removing IronPython
version parsing. The IronPython version parsing was not functional (see
https://github.com/IronLanguages/ironpython3/issues/1667).

### Core and BuiltinsÂ¶

- gh-101291: Rearrage bits in first field (after header) of PyLongObject. * Bits 0 and 1: 1 - sign. I.e. 0 for positive numbers, 1 for zero and 2 for negative numbers. * Bit 2 reserved (probably for the immortal bit) * Bits 3+ the unsigned size. This makes a few operations slightly more efficient, and will enable a more compact and faster 2s-complement representation of most ints in future.
- gh-102397: Fix segfault from race condition in signal handling during garbage collection. Patch by Kumar Aditya.
- gh-102406: `codecs` encoding/decoding errors now get the context
information (which operation and which codecs) attached as**PEP 678** notes instead of through chaining a new instance of the exception.
- gh-102281: Fix potential nullptr dereference and use of uninitialized memory in fileutils. Patch by Max Bachmann.
- gh-102300: Reuse operands with refcount of 1 in float specializations of BINARY_OP.
- gh-102213: Fix performance loss when accessing an objectâs attributes with `__getattr__` defined.
- gh-102255: Improve build support for the Xbox. Patch by Max Bachmann.

### BuildÂ¶

- gh-102027: Fix SSE2 and SSE3 detection in `_blake2` internal module.
Patch by Max Bachmann.

### Core and BuiltinsÂ¶

- gh-101865: Deprecate `co_lnotab` in code objects, schedule it for
removal in Python 3.14

### LibraryÂ¶

- bpo-1635741: Adapt `_pickle` to**PEP 687** . Patch by Mohamed Koubaa
and Erlend Aasland.
- gh-103085: Pure python `locale.getencoding()` will not warn
deprecation.
- gh-103068: Itâs no longer possible to register conditional breakpoints in `Pdb` that raise`SyntaxError` . Patch by Tian Gao.
- gh-102549: Donât ignore exceptions in member type creation.
- gh-103056: Ensure final `_generate_next_value_` is a`staticmethod` .
- gh-103046: Display current line label correctly in `dis` when`show_caches` is False and`lasti` points to a CACHE entry.
- gh-102433: `isinstance()` checks against```
runtime-checkable
protocols
```
now use`inspect.getattr_static()` rather than`hasattr()` to lookup
whether attributes exist. This means that descriptors and`__getattr__()` methods are no longer unexpectedly evaluated
during`isinstance()` checks against runtime-checkable protocols.
However, it may also mean that some objects which used to be considered
instances of a runtime-checkable protocol may no longer be considered
instances of that protocol on Python 3.12+, and vice versa. Most users are
unlikely to be affected by this change. Patch by Alex Waygood.
- gh-103023: Itâs no longer possible to register expressions to display in `Pdb` that raise`SyntaxError` . Patch by Tian Gao.
- gh-102947: Improve traceback when `dataclasses.fields()` is called on
a non-dataclass. Patch by Alex Waygood
- gh-102780: The `asyncio.Timeout` context manager now works reliably
even when performing cleanup due to task cancellation.  Previously it
could raise a`CancelledError` instead of an`TimeoutError` in such cases.
- gh-102871: Remove support for obsolete browsers from `webbrowser` .
Removed browsers include Grail, Mosaic, Netscape, Galeon, Skipstone,
Iceape, Firebird, and Firefox versions 35 and below.
- gh-102839: Improve performance of `math.log()` arguments handling by
removing the argument clinic.
- gh-102828: Add the `onexc` arg to`shutil.rmtree()` , which is like`onerror` but expects an exception instance rather than an exc_info
tuple. Deprecate`onerror` .
- gh-88965: typing: Fix a bug relating to substitution in custom classes generic over a `ParamSpec` . Previously, if the`ParamSpec` was substituted with a parameters list that itself contained
a`TypeVar` , the`TypeVar` in the parameters list could
not be subsequently substituted. This is now fixed.Patch by Nikita Sobolev.
- gh-76846: Fix issue where `__new__()` and`__init__()` methods of`pathlib.PurePath` and`Path` subclasses were not
called in some circumstances.
- gh-78530: `asyncio.wait()` now accepts generators yielding tasks.
Patch by Kumar Aditya.
- gh-102748: `asyncio.iscoroutine()` now returns`False` for
generators as`asyncio` does not support legacy generator-based
coroutines. Patch by Kumar Aditya.
- gh-102670: Optimized fmean(), correlation(), covariance(), and linear_regression() using the new math.sumprod() function.
- gh-102615: Typing: Improve the `repr` of generic aliases for classes
generic over a`ParamSpec` . (Use square brackets to
represent a parameter list.)
- gh-100112: `asyncio.Task.get_coro()` now always returns a coroutine
when wrapping an awaitable object. Patch by Kumar Aditya.
- gh-102578: Speed up setting or deleting mutable attributes on non-dataclass subclasses of frozen dataclasses. Due to the implementation of `__setattr__` and`__delattr__` for frozen dataclasses, this
previously had a time complexity of*O* (*n* ). It now has a time
complexity of*O* (1).
- gh-102519: Add `os.listdrives()` ,`os.listvolumes()` and`os.listmounts()` functions on Windows for enumerating drives, volumes
and mount points
- gh-74468: Attribute name of the extracted `tarfile` file object now
holds filename of itself rather than of the archive it is contained in.
Patch by Oleg Iarygin.
- gh-102378: Private helper method `inspect._signature_strip_non_python_syntax` will no longer strip`/` from the input string.
- gh-79940: Add `inspect.getasyncgenstate()` and`inspect.getasyncgenlocals()` . Patch by Thomas Krennwallner.
- gh-102103: Add `module` argument to`dataclasses.make_dataclass()` and make classes produced by it pickleable.
- gh-102069: Fix `__weakref__` descriptor generation for custom
dataclasses.
- gh-102038: Skip a `stat` in`site` if we have already found a`pyvenv.cfg`
- gh-98886: Fix issues when defining dataclasses that have fields with specific underscore names that arenât clearly reserved by `dataclasses` .
- gh-101673: Fix a `pdb` bug where`ll` clears the changes to local
variables.
- gh-101313: Added -h and âhelp arguments to the webbrowser CLI
- gh-100372: `ssl.SSLContext.load_verify_locations()` no longer
incorrectly accepts some cases of trailing data when parsing DER.
- gh-89727: Fix pathlib.Path.walk RecursionError on deep directory trees by rewriting it using iteration instead of recursion.
- gh-100131: Added an optional `delete` keyword argument to`tempfile.TemporaryDirectory` .
- gh-48330: Added `--durations` command line option, showing the N slowest
test cases.`unittest.TextTestRunner` and`unittest.TextTestResult` constructors accept a new*durations* keyword argument. Subclasses should take this into account or accept`**kwargs` . Added`unittest.TestResult.addDuration()` method and`unittest.TestResult.collectedDurations` attribute.(Contributed by Giampaolo Rodola)
- gh-98169: Fix `dataclasses.astuple()` crash when`collections.defaultdict` is present in the attributes.
- gh-96931: Fix incorrect results from `ssl.SSLSocket.shared_ciphers()`
- gh-95495: When built against OpenSSL 3.0, the `ssl` module had a bug
where it reported unauthenticated EOFs (i.e. without close_notify) as a
clean TLS-level EOF. It now raises`SSLEOFError` , matching the
behavior in previous versions of OpenSSL. The`options` attribute on`SSLContext` also
no longer includes`OP_IGNORE_UNEXPECTED_EOF` by default. This
option may be set to specify the previous OpenSSL 3.0 behavior.
- gh-94684: Now `uuid.uuid3()` and`uuid.uuid5()` functions support`bytes` objects as their*name* argument.
- gh-94440: Fix a `concurrent.futures.process` bug where`ProcessPoolExecutor` shutdown could hang after a future has been
quickly submitted and canceled.
- gh-72346: Added deprecation warning to *isdst* parameter of`email.utils.localtime()` .
- bpo-36305: Fix handling of Windows filenames that resemble drives, such as `./a:b` , in`pathlib` .

### DocumentationÂ¶

- gh-103112: Add docstring to `http.client.HTTPResponse.read()` to fix`pydoc` output.

### TestsÂ¶

### BuildÂ¶

### WindowsÂ¶

- gh-102690: Update `webbrowser` to fall back to Microsoft Edge instead
of Internet Explorer.
- gh-99726: Improves correctness of stat results for Windows, and uses faster API when available

### Tools/DemosÂ¶

- gh-102809: `Misc/gdbinit` was removed.

### C APIÂ¶

- gh-102013: Add a new (unstable) C-API function for iterating over GCâable objects using a callback: `PyUnstable_VisitObjects` .

## Python 3.12.0 alpha 6Â¶

*Release date: 2023-03-07*

### SecurityÂ¶

- gh-99108: Replace builtin hashlib implementations of MD5 and SHA1 with verified ones from the HACL* project.
- gh-101727: Updated the OpenSSL version used in Windows and macOS binary release builds to 1.1.1t to address **CVE 2023-0286** ,**CVE 2022-4303** ,
and**CVE 2022-4303** per the OpenSSL 2023-02-07 security advisory.
- gh-99108: Replace the builtin `hashlib` implementations of SHA2-384
and SHA2-512 originally from LibTomCrypt with formally verified,
side-channel resistant code from the HACL* project. The builtins remain a
fallback only used when OpenSSL does not provide them.
- gh-101283: `subprocess.Popen` now uses a safer approach to find`cmd.exe` when launching with`shell=True` . Patch by Eryk Sun, based
on a patch by Oleg Iarygin.

### Core and BuiltinsÂ¶

- gh-102493: Fix regression in semantics of normalisation in `PyErr_SetObject` .
- gh-102416: Do not memoize incorrectly automatically generated loop rules in the parser. Patch by Pablo Galindo.
- gh-102356: Fix a bug that caused a crash when deallocating deeply nested filter objects. Patch by Marta GÃ³mez MacÃas.
- gh-102336: Cleanup Windows 7 specific special handling. Patch by Max Bachmann.
- gh-102250: Fixed a segfault occurring when the interpreter calls a `__bool__` method that raises.
- gh-102126: Fix deadlock at shutdown when clearing thread states if any finalizer tries to acquire the runtime head lock. Patch by Kumar Aditya.
- gh-102027: Use `GetCurrentProcessId` on Windows when`getpid` is
unavailable. Patch by Max Bachmann.
- gh-102056: Fix error handling bugs in interpreterâs exception printing code, which could cause a crash on infinite recursion.
- gh-100982: Restrict the scope of the `FOR_ITER_RANGE` instruction
to the scope of the original`FOR_ITER` instruction, to allow
instrumentation.
- gh-101967: Fix possible segfault in `positional_only_passed_as_keyword` function, when new list created.
- gh-101952: Fix possible segfault in `BUILD_SET` opcode, when new set
created.
- gh-74895: `socket.getaddrinfo` no longer raises`OverflowError` for`int`**port** values outside of the C
long range. Out of range values are left up to the underlying string based
C library API to report. A`socket.gaierror``SAI_SERVICE` may
occur instead, or no error at all as not all platform C libraries generate
an error.
- gh-101799: Add `CALL_INTRINSIC_2` and use it instead of`PREP_RERAISE_STAR` .
- gh-101857: Fix xattr support detection on Linux systems by widening the check to linux, not just glibc. This fixes support for musl.
- gh-84783: Make the slice object hashable. Patch by Will Bradshaw and Furkan Onder.
- gh-87849: Change the `SEND` instruction to leave the receiver on the
stack. This allows the specialized form of`SEND` to skip the chain of C
calls and jump directly to the`RESUME` in the generator or coroutine.
- gh-101765: Fix SystemError / segmentation fault in iter `__reduce__` when internal access of`builtins.__dict__` keys mutates the iter
object.
- gh-101430: Update `tracemalloc` to handle presize of object properly.
Patch by Donghee Na.
- gh-101696: Invalidate type version tag in `_PyStaticType_Dealloc` for
static types, avoiding bug where a false cache hit could crash the
interpreter. Patch by Kumar Aditya.
- gh-101632: Adds a new `RETURN_CONST` instruction.
- gh-100719: Remove gi_code field from generator (and coroutine and async generator) objects as it is redundant. The frame already includes a reference to the code object.
- gh-98627: When an interpreter is configured to check (and only then), importing an extension module will now fail when the extension does not support multiple interpreters (i.e. doesnât implement PEP 489 multi-phase init). This does not apply to the main interpreter, nor to subinterpreters created with `Py_NewInterpreter()` .

### LibraryÂ¶

- gh-102302: Micro-optimise hashing of `inspect.Parameter` , reducing
the time it takes to hash an instance by around 40%.
- gh-101979: Fix a bug where parentheses in the `metavar` argument to`argparse.ArgumentParser.add_argument()` were dropped. Patch by Yeojin
Kim.
- gh-91038: `platform.platform()` now has boolean default arguments.
- gh-81652: Add `mmap.MAP_ALIGNED_SUPER` FreeBSD and`mmap.MAP_CONCEAL` OpenBSD constants to`mmap` . Patch by
Yeojin Kim.
- gh-102179: Fix `os.dup2()` error message for negative fds.
- gh-101961: For the binary mode, `fileinput.hookcompressed()` doesnât
set the`encoding` value even if the value is`None` . Patch by Gihwan
Kim.
- gh-101936: The default value of `fp` becomes`io.BytesIO` if`HTTPError` is initialized without a designated`fp` parameter. Patch by Long Vo.
- gh-101566: In zipfile, sync Path with zipp 3.14, including fix for extractall on the underlying zipfile after being wrapped in `Path` .
- gh-97930: Apply changes from importlib_resources 5.12, including fix for `MultiplexedPath` to support directories in multiple
namespaces (python/importlib_resources#265).
- gh-101997: Upgrade pip wheel bundled with ensurepip (pip 23.0.1)
- gh-99108: The built-in extension modules for `hashlib` SHA2
algorithms, used when OpenSSL does not provide them, now live in a single
internal`_sha2` module instead of separate`_sha256` and`_sha512` modules.
- gh-101892: Callable iterators no longer raise `SystemError` when
the callable object exhausts the iterator but forgets to either return a
sentinel value or raise`StopIteration` .
- gh-87634: Remove locking behavior from `functools.cached_property()` .
- gh-97786: Fix potential undefined behaviour in corner cases of floating-point-to-time conversions.
- gh-101517: Fixed bug where `bdb` looks up the source line with`linecache` with a`lineno=None` , which causes it to fail with an
unhandled exception.
- gh-101773: Optimize `fractions.Fraction` for small components. The
private argument`_normalize` of the`fractions.Fraction` constructor has been removed.
- gh-101693: In `sqlite3.Cursor.execute()` ,`DeprecationWarning` is
now emitted when named placeholders are used
together with parameters supplied as a sequence instead of as a`dict` . Starting from Python 3.14, using named placeholders with
parameters supplied as a sequence will raise a`ProgrammingError` . Patch by Erlend E. Aasland.
- gh-101446: Change repr of `collections.OrderedDict` to use regular
dictionary formatting instead of pairs of keys and values.
- gh-101362: Speed up `pathlib.PurePath` construction by handling
arguments more uniformly. When a`pathlib.Path` argument is
supplied, we use its string representation rather than joining its parts
with`os.path.join()` .
- gh-101362: Speed up `pathlib.PurePath` construction by calling`os.path.join()` only when two or more arguments are given.
- gh-101362: Speed up `pathlib.Path` construction by running the path
flavour compatibility check only when pathlib is imported.
- gh-85984: Refactored the implementation of `pty.fork()` to use`os.login_tty()` .A `DeprecationWarning` is now raised by`pty.master_open()` and`pty.slave_open()` . They were undocumented and deprecated long long ago
in the docstring in favor of`pty.openpty()` .
- gh-101561: Add a new decorator `typing.override()` . See**PEP 698** for
details. Patch by Steven Troxler.
- gh-63301: Set exit code when `tabnanny` CLI exits on error.
- gh-101360: Fix anchor matching in `pathlib.PureWindowsPath.match()` .
Path and pattern anchors are now matched with`fnmatch` , just like
other path parts. This allows patterns such as`"*:/Users/*"` to be
matched.
- gh-101277: Remove global state from `itertools` module (**PEP 687** ).
Patches by Erlend E. Aasland.
- gh-100809: Fix handling of drive-relative paths (like âC:â and âC:fooâ) in `pathlib.Path.absolute()` . This method now uses the OS API to retrieve
the correct current working directory for the drive.
- gh-99138: Apply **PEP 687** to`zoneinfo` . Patch by Erlend E. Aasland.
- gh-96764: `asyncio.wait_for()` now uses`asyncio.timeout()` as its
underlying implementation. Patch by Kumar Aditya.
- gh-88233: Correctly preserve âextraâ fields in `zipfile` regardless of
their ordering relative to a zip64 âextra.â
- bpo-23224: Fix segfaults when creating `lzma.LZMADecompressor` and`bz2.BZ2Decompressor` objects without calling`__init__()` , and
fix leakage of locks and internal buffers when calling the`__init__()` methods of`lzma.LZMADecompressor` ,`lzma.LZMACompressor` ,`bz2.BZ2Compressor` , and`bz2.BZ2Decompressor` objects
multiple times.

### DocumentationÂ¶

- gh-85417: Update `cmath` documentation to clarify behaviour on branch
cuts.
- gh-97725: Fix `asyncio.Task.print_stack()` description for`file=None` . Patch by Oleg Iarygin.

### TestsÂ¶

- gh-102019: Fix deadlock on shutdown if `test_current_{exception,frames}` fails. Patch by Jacob Bower.
- gh-85984: Utilize new âwinsizeâ functions from termios in pty tests.
- gh-89792: `test_tools` now copies up to 10x less source data to a
temporary directory during the`freeze` test by ignoring git metadata
and other artifacts.  It also limits its python build parallelism based on
os.cpu_count instead of hard coding it as 8 cores.

### BuildÂ¶

- gh-99942: On Android, in a static build, python-config in embed mode no longer incorrectly reports a library to link to.
- gh-99942: On Android, python.pc now correctly reports the library to link to, the same as python-config.sh.
- gh-100221: Fix creating install directories in `make sharedinstall` if
they exist outside`DESTDIR` already.
- gh-96821: Explicitly mark C extension modules that need defined signed integer overflow, and add a configure option `--with-strict-overflow` . Patch by Matthias GÃ¶rgens and Shantanu
Jain.

### WindowsÂ¶

- gh-102344: Implement `winreg.QueryValue` using`QueryValueEx` and`winreg.SetValue` using`SetValueEx` . Patch by Max Bachmann.
- gh-101881: Handle read and write operations on non-blocking pipes properly on Windows.
- gh-101881: Add support for the os.get_blocking() and os.set_blocking() functions on Windows.
- gh-101849: Ensures installer will correctly upgrade existing `py.exe` launcher installs.
- gh-101763: Updates copy of libffi bundled with Windows installs to 3.4.4.
- gh-101759: Update Windows installer to SQLite 3.40.1.
- gh-101614: Correctly handle extensions built against debug binaries that reference `python3_d.dll` .
- gh-101196: The functions `os.path.isdir` ,`os.path.isfile` ,`os.path.islink` and`os.path.exists` are now 13% to 28% faster on
Windows, by making fewer Win32 API calls.

### macOSÂ¶

- gh-101759: Update macOS installer to SQLite 3.40.1.

### C APIÂ¶

- gh-101907: Removes use of non-standard C++ extension in public header files.
- gh-99293: Document that the Py_TPFLAGS_VALID_VERSION_TAG is an internal feature, should not be used, and will be removed.
- gh-101578: Add `PyErr_GetRaisedException()` and`PyErr_SetRaisedException()` for saving and restoring the current
exception. These functions return and accept a single exception object,
rather than the triple arguments of the now-deprecated`PyErr_Fetch()` and`PyErr_Restore()` . This is less error
prone and a bit more efficient.Add `PyException_GetArgs()` and`PyException_SetArgs()` as
convenience functions for retrieving and modifying the`args` passed to the exceptionâs constructor.
- gh-91744: Introduced the *Unstable C API tier* , marking APi that is
allowed to change in minor releases without a deprecation period. See**PEP 689** for details.

## Python 3.12.0 alpha 5Â¶

*Release date: 2023-02-07*

### SecurityÂ¶

### Core and BuiltinsÂ¶

- gh-92173: Fix the `defs` and`kwdefs` arguments to`PyEval_EvalCodeEx()` and a reference leak in that function.
- gh-59956: The GILState API is now partially compatible with subinterpreters. Previously, `PyThreadState_GET()` and`PyGILState_GetThisThreadState()` would get out of sync, causing
inconsistent behavior and crashes.
- gh-101400: Fix wrong lineno in exception message on `continue` or`break` which are not in a loop. Patch by Donghee Na.
- gh-101372: Fix `is_normalized()` to properly handle the
UCD 3.2.0 cases. Patch by Donghee Na.
- gh-101266: Fix `sys.getsizeof()` reporting for`int` subclasses.
- gh-101291: Refactor the `PyLongObject` struct into a normal Python
object header and a`PyLongValue` struct.
- gh-101046: Fix a possible memory leak in the parser when raising `MemoryError` . Patch by Pablo Galindo
- gh-101037: Fix potential memory underallocation issue for instances of `int` subclasses with value zero.
- gh-100762: Record the (virtual) exception block depth in the oparg of `YIELD_VALUE` . Use this to avoid the expensive`throw()` when
closing generators (and coroutines) that can be closed trivially.
- gh-100982: Adds a new `COMPARE_AND_BRANCH` instruction. This is a
bit more efficient when performing a comparison immediately followed by a
branch, and restores the design intent of PEP 659 that specializations are
local to a single instruction.
- gh-100942: Fixed segfault in property.getter/setter/deleter that occurred when a property subclass overrode the `__new__` method to return a
non-property instance.
- gh-100923: Remove the `mask` cache entry for the`COMPARE_OP` instruction and embed the mask into the oparg.
- gh-100892: Fix race while iterating over thread states in clearing `threading.local` . Patch by Kumar Aditya.
- gh-91351: Fix a case where re-entrant imports could corrupt the import deadlock detection code and cause a `KeyError` to be raised out of`importlib/_bootstrap` .  In addition to the straightforward cases,
this could also happen when garbage collection leads to a warning being
emitted â as happens when it collects an open socket or file)
- gh-100726: Optimize construction of `range` object for medium size
integers.
- gh-100712: Added option to build cpython with specialization disabled, by setting `ENABLE_SPECIALIZATION=False` in`opcode` , followed by`make regen-all` .

### LibraryÂ¶

- bpo-32780: Inter-field padding is now inserted into the PEP3118 format strings obtained from `ctypes.Structure` objects, reflecting their
true representation in memory.
- gh-101541: [Enum] - fix psuedo-flag creation
- gh-101570: Upgrade pip wheel bundled with ensurepip (pip 23.0)
- gh-101323: Fix a bug where errors where not thrown by zlib._ZlibDecompressor if encountered during decompressing.
- gh-101317: Add *ssl_shutdown_timeout* parameter for`asyncio.StreamWriter.start_tls()` .
- gh-101326: Fix regression when passing `None` as second or third
argument to`FutureIter.throw` .
- gh-92123: Adapt the `_elementtree` extension module to multi-phase init
(**PEP 489** ). Patches by Erlend E. Aasland.
- gh-100795: Avoid potential unexpected `freeaddrinfo` call (double free)
in`socket` when a libc`getaddrinfo()` implementation leaves
garbage in an output pointer when returning an error. Original patch by
Sergey G. Brester.
- gh-101143: Remove unused references to `TimerHandle` in`asyncio.base_events.BaseEventLoop._add_callback` .
- gh-101144: Make `zipfile.Path.open()` and`zipfile.Path.read_text()` also accept`encoding` as a positional
argument. This was the behavior in Python 3.9 and earlier.  3.10
introduced a regression where supplying it as a positional argument would
lead to a`TypeError` .
- gh-94518: Group-related variables of `_posixsubprocess` module are
renamed to stress that supplementary group affinity is added to a fork,
not replace the inherited ones. Patch by Oleg Iarygin.
- gh-101015: Fix `typing.get_type_hints()` on`'*tuple[...]'` and`*tuple[...]` . It must not drop the`Unpack` part.
- gh-101000: Add `os.path.splitroot()` , which splits a path into a
3-item tuple`(drive, root, tail)` . This new function is used by`pathlib` to improve the performance of path construction by up to a
third.
- gh-100573: Fix a Windows `asyncio` bug with named pipes where a
client doing`os.stat()` on the pipe would cause an error in the server
that disabled serving future requests.
- gh-39615: `warnings.warn()` now has the ability to skip stack frames
based on code filename prefix rather than only a numeric`stacklevel` via the new`skip_file_prefixes` keyword argument.
- gh-100750: pass encoding kwarg to subprocess in platform
- gh-100160: Emit a deprecation warning in `asyncio.DefaultEventLoopPolicy.get_event_loop()` if there is no
current event loop set and it decides to create one.
- gh-96290: Fix handling of partial and invalid UNC drives in `ntpath.splitdrive()` , and in`ntpath.normpath()` on non-Windows
systems. Paths such as â\serverâ and â\â are now considered by`splitdrive()` to contain only a drive, and consequently are not
modified by`normpath()` on non-Windows systems. The behaviour of`normpath()` on Windows systems is unaffected, as native OS APIs are
used. Patch by Eryk Sun, with contributions by Barney Gale.
- gh-99952: Fix a reference undercounting issue in `ctypes.Structure` with`from_param()` results larger than a C pointer.
- gh-67790: Add float-style formatting support for `fractions.Fraction` instances.
- gh-99266: Preserve more detailed error messages in `ctypes` .
- gh-86682: Ensure runtime-created collections have the correct module name using the newly added (internal) `sys._getframemodulename()` .
- gh-88597: `uuid` now has a command line interface. Try```
python -m
uuid -h
```
.
- gh-60580: `ctypes.wintypes.BYTE` definition changed from`c_byte` to`c_ubyte` to match Windows SDK.
Patch by Anatoly Techtonik and Oleg Iarygin.
- gh-94518: `_posixsubprocess` now initializes all UID and GID variables
using a reserved`-1` value instead of a separate flag. Patch by Oleg
Iarygin.
- bpo-38941: The `xml.etree.ElementTree` module now emits`DeprecationWarning` when testing the truth value of an`xml.etree.ElementTree.Element` . Before, the Python implementation
emitted`FutureWarning` , and the C implementation emitted nothing.
- bpo-40077: Convert `elementtree` types to heap types. Patch by Erlend
E. Aasland.
- bpo-29847: Fix a bug where `pathlib.Path` accepted and ignored
keyword arguments. Patch provided by Yurii Karabas.
- gh-77772: `ctypes.CDLL` ,`ctypes.OleDLL` ,`ctypes.WinDLL` , and`ctypes.PyDLL` now accept
path-like objects as their`name` argument.
Patch by Robert Hoelzl.

### DocumentationÂ¶

- gh-88324: Reword `subprocess` to emphasize default behavior of*stdin* ,*stdout* , and*stderr* arguments. Remove inaccurate statement
about child file handle inheritance.

### TestsÂ¶

- gh-101334: `test_tarfile` has been updated to pass when run as a high
UID.

### BuildÂ¶

- gh-101282: Update BOLT configuration not to use deprecated usage of `--split functions` . Patch by Donghee Na.
- gh-101522: Allow overriding Windows dependencies versions and paths using MSBuild properties.
- gh-77532: Minor fixes to allow building with `PlatformToolset=ClangCL` on Windows.
- gh-101152: In accordance with **PEP 699** , the`ma_version_tag` field in`PyDictObject` is deprecated for extension modules. Accessing this
field will generate a compiler warning at compile time. This field will be
removed in Python 3.14.
- gh-100340: Allows -Wno-int-conversion for wasm-sdk 17 and onwards, thus enables building WASI builds once against the latest sdk.
- gh-101060: Conditionally add `-fno-reorder-blocks-and-partition` in
configure. Effectively fixes`--enable-bolt` when using Clang, as this
appears to be a GCC-only flag.
- gh-98705: `__bool__` is defined in AIX system header files which breaks
the build in AIX, so undefine it.
- gh-98636: Fix a regression in detecting `gdbm_compat` library for the`_gdbm` module build.
- gh-96305: `_aix_support` now uses a simple code to get platform details
rather than the now non-existent`_bootsubprocess` during bootstrap.

### WindowsÂ¶

- gh-101543: Ensure the install path in the registry is only used when the standard library hasnât been located in any other way.
- gh-101467: The `py.exe` launcher now correctly filters when only a
single runtime is installed. It also correctly handles prefix matches on
tags so that`-3.1` does not match`3.11` , but would still match`3.1-32` .
- gh-99834: Updates bundled copy of Tcl/Tk to 8.6.13.0
- gh-101135: Restore ability to launch older 32-bit versions from the `py.exe` launcher when both 32-bit and 64-bit installs of the same
version are available.
- gh-82052: Fixed an issue where writing more than 32K of Unicode output to the console screen in one go can result in mojibake.
- gh-100320: Ensures the `PythonPath` registry key from an install is used
when launching from a different copy of Python that relies on an existing
install to provide a copy of its modules and standard library.
- gh-100247: Restores support for the `py.exe` launcher finding
shebang commands in its configuration file using the full command name.

## Python 3.12.0 alpha 4Â¶

*Release date: 2023-01-10*

### Core and BuiltinsÂ¶

- gh-100776: Fix misleading default value in `input()` âs`__text_signature__` .
- gh-99005: Remove `UNARY_POSITIVE` ,`ASYNC_GEN_WRAP` and`LIST_TO_TUPLE` , replacing them with intrinsics.
- gh-99005: Add new `CALL_INTRINSIC_1` instruction. Remove`IMPORT_STAR` ,`PRINT_EXPR` and`STOPITERATION_ERROR` , replacing them with the`CALL_INTRINSIC_1` instruction.
- gh-100288: Remove the LOAD_ATTR_METHOD_WITH_DICT specialized instruction. Stats show it is not useful.
- gh-100720: Added `_PyFrame_NumSlotsForCodeObject` , which returns the
number of slots needed in a frame for a given code object.
- gh-100719: Removed the co_nplaincellvars field from the code object, as it is redundant.
- gh-100637: Fix `int.__sizeof__()` calculation to include the 1-element`ob_digit` array for`0` and`False` .
- gh-100649: Update the native_thread_id field of PyThreadState after fork.
- gh-100126: Fix an issue where âincompleteâ frames could be briefly visible to C code while other frames are being torn down, possibly resulting in corruption or hard crashes of the interpreter while running finalizers.
- gh-87447: Fix `SyntaxError` on comprehension rebind checking with
names that are not actually redefined.Now reassigning `b` in`[(b := 1) for a, b.prop in some_iter]` is
allowed. Reassigning`a` is still disallowed as per**PEP 572** .
- gh-100268: Add `int.is_integer()` to improve duck type compatibility
between`int` and`float` .
- gh-100425: Improve the accuracy of `sum()` with compensated summation.

### LibraryÂ¶

- gh-100374: Fix incorrect result and delay in `socket.getfqdn()` . Patch
by Dominic Socular.

### Core and BuiltinsÂ¶

- gh-100357: Convert `vars` ,`dir` ,`next` ,`getattr` , and`iter` to argument clinic.
- gh-100117: Improve the output of `codeobject.co_lines()` by emitting
only one entry for each line range.
- gh-90043: Handle NaNs when specializing `COMPARE_OP` for`float` values.
- gh-100222: Redefine the `_Py_CODEUNIT` typedef as a union to describe
its layout to the C compiler, avoiding type punning and improving clarity.
- gh-99955: Internal compiler functions (in compile.c) now consistently return -1 on error and 0 on success.
- gh-100188: The `BINARY_SUBSCR_LIST_INT` and`BINARY_SUBSCR_TUPLE_INT` instructions are no longer used for negative integers because those
instructions always miss when encountering negative integers.
- gh-99110: Initialize frame->previous in frameobject.c to fix a segmentation fault when accessing frames created by `PyFrame_New()` .
- gh-94155: Improved the hashing algorithm for code objects, mitigating some hash collisions.
- gh-99540: `None` now hashes to a constant value. This is not a
requirements change.
- gh-100143: When built with `--enable-pystats` , stats collection is now
off by default. To enable it early at startup, pass the`-Xpystats` flag.  Stats are now always dumped, even if switched off.
- gh-100146: Improve `BUILD_LIST` opcode so that it works similarly to the`BUILD_TUPLE` opcode, by stealing references from the stack rather than
repeatedly using stack operations to set list elements.  Implementation
details are in a new private API`_PyList_FromArraySteal()` .
- gh-100110: Specialize `FOR_ITER` for tuples.
- gh-100050: Honor existing errors obtained when searching for mismatching parentheses in the tokenizer. Patch by Pablo Galindo
- gh-92216: Improve the performance of `hasattr()` for type objects with
a missing attribute.
- gh-99582: Freeze `zipimport` module into`_bootstrap_python` .
- gh-99554: Pack debugging location tables more efficiently during bytecode compilation.
- gh-98522: Add an internal version number to code objects, to give better versioning of inner functions and comprehensions, and thus better specialization of those functions. This change is invisible to both Python and C extensions.
- gh-94603: Improve performance of `list.pop` for small lists.

### LibraryÂ¶

### Core and BuiltinsÂ¶

- bpo-32782: `ctypes` arrays of length 0 now report a correct itemsize
when a`memoryview` is constructed from them, rather than always giving
a value of 0.

### LibraryÂ¶

- gh-100833: Speed up `math.fsum()` by removing defensive`volatile` qualifiers.
- gh-100805: Modify `random.choice()` implementation to once again work
with NumPy arrays.
- gh-100813: Add `socket.IP_PKTINFO` constant.
- gh-100792: Make `email.message.Message.__contains__()` twice as fast.
- gh-91851: Microoptimizations for `fractions.Fraction.__round__()` ,`fractions.Fraction.__ceil__()` and`fractions.Fraction.__floor__()` .
- gh-90104: Avoid RecursionError on `repr` if a dataclass field definition
has a cyclic reference.
- gh-100689: Fix crash in `pyexpat` by statically allocating`PyExpat_CAPI` capsule.
- gh-100740: Fix `unittest.mock.Mock` not respecting the spec for
attribute names prefixed with`assert` .
- gh-91219: Change `SimpleHTTPRequestHandler` to support subclassing to
provide a different set of index file names instead of using`__init__` parameters.
- gh-100690: `Mock` objects which are not unsafe will now raise an`AttributeError` when accessing an attribute that matches the name of an
assertion but without the prefix`assert_` , e.g. accessing`called_once` instead of`assert_called_once` . This is in addition to
this already happening for accessing attributes with prefixes`assert` ,`assret` ,`asert` ,`aseert` , and`assrt` .
- gh-89727: Simplify and optimize `os.walk()` by using`isinstance()` checks to check the top of the stack.
- gh-100485: Add math.sumprod() to compute the sum of products.
- gh-86508: Fix `asyncio.open_connection()` to skip binding to local
addresses of different family. Patch by Kumar Aditya.
- gh-97930: `importlib.resources.files` now accepts a module as an anchor
instead of only accepting packages. If a module is passed, resources are
resolved adjacent to that module (in the same package or at the package
root). The parameter was renamed from`package` to`anchor` with a
compatibility shim for those passing by keyword. Additionally, the new`anchor` parameter is now optional and will default to the callerâs
module.
- gh-100585: Fixed a bug where importlib.resources.as_file was leaving file pointers open
- gh-100562: Improve performance of `pathlib.Path.absolute()` by nearly
2x. This comes at the cost of a performance regression in`pathlib.Path.cwd()` , which is generally used less frequently in user
code.
- gh-100519: Small simplification of `http.cookiejar.eff_request_host()` that improves readability and better matches the RFC wording.
- gh-100287: Fix the interaction of `unittest.mock.seal()` with`unittest.mock.AsyncMock` .
- gh-100488: Add `Fraction.is_integer()` to check whether a`fractions.Fraction` is an integer. This improves duck type
compatibility with`float` and`int` .
- gh-100474: `http.server` now checks that an index page is actually a
regular file before trying to serve it.  This avoids issues with
directories named`index.html` .
- gh-100363: Speed up `asyncio.get_running_loop()` by removing redundant`getpid` checks. Patch by Kumar Aditya.
- gh-78878: Fix crash when creating an instance of `_ctypes.CField` .
- gh-100348: Fix ref cycle in `asyncio._SelectorSocketTransport` by
removing`_read_ready_cb` in`close` .
- gh-100344: Provide C implementation for `asyncio.current_task()` for a
4x-6x speedup.
- gh-100272: Fix JSON serialization of OrderedDict. It now preserves the order of keys.
- gh-83076: Instantiation of `Mock()` and`AsyncMock()` is now 3.8x
faster.
- gh-100234: Set a default value of 1.0 for the `lambd` parameter in
random.expovariate().
- gh-100228: A `DeprecationWarning` may be raised when`os.fork()` or`os.forkpty()` is called from multi-threaded processes.  Forking
with threads is unsafe and can cause deadlocks, crashes and subtle
problems. Lack of a warning does not indicate that the fork call was
actually safe, as Python may not be aware of all threads.
- gh-100039: Improve signatures for enums and flags.
- gh-100133: Fix regression in `asyncio` where a subprocess would
sometimes lose data received from pipe.
- bpo-44592: Fixes inconsistent handling of case sensitivity of *extrasaction* arg in`csv.DictWriter` .
- gh-100098: Fix `tuple` subclasses being cast to`tuple` when used as
enum values.
- gh-85432: Rename the *fmt* parameter of the pure-Python implementation of`datetime.time.strftime()` to*format* . Rename the*t* parameter of`datetime.datetime.fromtimestamp()` to*timestamp* . These changes mean
the parameter names in the pure-Python implementation now match the
parameter names in the C implementation. Patch by Alex Waygood.
- gh-98778: Update `HTTPError` to be initialized
properly, even if the`fp` is`None` . Patch by Donghee Na.
- gh-99925: Unify error messages in JSON serialization between `json.dumps(float('nan'), allow_nan=False)` and`json.dumps(float('nan'), allow_nan=False, indent=<SOMETHING>)` . Now
both include the representation of the value that could not be serialized.
- gh-89727: Fix issue with `os.walk()` where a`RecursionError` would occur on deep directory structures by adjusting the implementation
of`os.walk()` to be iterative instead of recursive.
- gh-94943: Add Dataclass support to the `Enum``__repr__()` . When inheriting from a`dataclass` , only show the field names in the value
section of the member`repr()` , and not the dataclassâ class name.
- gh-83035: Fix `inspect.getsource()` handling of decorator calls with
nested parentheses.
- gh-99576: Fix `.save()` method for`LWPCookieJar` and`MozillaCookieJar` : saved file was not truncated on repeated save.
- gh-94912: Add `inspect.markcoroutinefunction()` decorator which
manually marks a function as a coroutine for the benefit of`iscoroutinefunction()` .
- gh-99509: Add **PEP 585** support for`multiprocessing.queues.Queue` .
- gh-99482: Remove `Jython` partial compatibility code from several stdlib
modules.
- gh-99433: Fix `doctest` failure on`types.MethodWrapperType` in modules.
- gh-85267: Several improvements to `inspect.signature()` âs handling of`__text_signature` . - Fixes a case where`inspect.signature()` dropped parameters - Fixes a case where`inspect.signature()` raised`tokenize.TokenError` - Allows`inspect.signature()` to
understand defaults involving binary operations of constants -`inspect.signature()` is documented as only raising`TypeError` or`ValueError` , but sometimes raised`RuntimeError` . These
cases now raise`ValueError` - Removed a dead code path
- gh-91166: `asyncio` is optimized to avoid excessive copying when
writing to socket and use`sendmsg()` if the platform
supports it. Patch by Kumar Aditya.
- gh-98030: Add missing TCP socket options from Linux: `TCP_MD5SIG` ,`TCP_THIN_LINEAR_TIMEOUTS` ,`TCP_THIN_DUPACK` ,`TCP_REPAIR` ,`TCP_REPAIR_QUEUE` ,`TCP_QUEUE_SEQ` ,`TCP_REPAIR_OPTIONS` ,`TCP_TIMESTAMP` ,`TCP_CC_INFO` ,`TCP_SAVE_SYN` ,`TCP_SAVED_SYN` ,`TCP_REPAIR_WINDOW` ,`TCP_FASTOPEN_CONNECT` ,`TCP_ULP` ,`TCP_MD5SIG_EXT` ,`TCP_FASTOPEN_KEY` ,`TCP_FASTOPEN_NO_COOKIE` ,`TCP_ZEROCOPY_RECEIVE` ,`TCP_INQ` ,`TCP_TX_DELAY` .
- gh-88500: Reduced the memory usage of `urllib.parse.unquote()` and`urllib.parse.unquote_to_bytes()` on large values.
- gh-96127: `inspect.signature` was raising`TypeError` on call with
mock objects. Now it correctly returns`(*args, **kwargs)` as inferred
signature.
- gh-95882: Fix a 3.11 regression in `asynccontextmanager()` , which caused it to propagate
exceptions with incorrect tracebacks and fix a 3.11 regression in`contextmanager()` , which caused it to propagate
exceptions with incorrect tracebacks for`StopIteration` .
- gh-78707: Deprecate passing more than one positional argument to `pathlib.PurePath.relative_to()` and`is_relative_to()` .
- gh-92122: Fix reStructuredText syntax errors in docstrings in the `enum` module.
- gh-91851: Optimize the `Fraction` arithmetics for small
components.
- bpo-24132: Make `pathlib.PurePath` and`Path` subclassable (private to start). Previously, attempting to instantiate a
subclass resulted in an`AttributeError` being raised. Patch by
Barney Gale.
- bpo-40447: Accept `os.PathLike` (such as`pathlib.Path` ) in
the`stripdir` arguments of`compileall.compile_file()` and`compileall.compile_dir()` .
- bpo-36880: Fix a reference counting issue when a `ctypes` callback
with return type`py_object` returns`None` , which could
cause crashes.

### DocumentationÂ¶

- gh-100616: Document existing `attr` parameter to`curses.window.vline()` function in`curses` .
- gh-100472: Remove claim in documentation that the `stripdir` ,`prependdir` and`limit_sl_dest` parameters of`compileall.compile_dir()` and`compileall.compile_file()` could
be`bytes` .
- bpo-25377: Clarify use of octal format of mode argument in help(os.chmod) as well as help(os.fchmod)

### TestsÂ¶

- gh-100454: Start running SSL tests with OpenSSL 3.1.0-beta1.
- gh-100086: The Python test runner (libregrtest) now logs Python build information like âdebugâ vs âreleaseâ build, or LTO and PGO optimizations. Patch by Victor Stinner.
- gh-93018: Make two tests forgiving towards host system libexpat with backported security fixes applied.

### BuildÂ¶

- gh-100540: Removed the `--with-system-ffi``configure` option;`libffi` must now always be supplied by the system on all non-Windows
platforms.  The option has had no effect on non-Darwin platforms for
several releases, and in 3.11 only had the non-obvious effect of invoking`pkg-config` to find`libffi` and never setting`-DUSING_APPLE_OS_LIBFFI` .  Now on Darwin platforms`configure` will
first check for the OS`libffi` and then fall back to the same
processing as other platforms if it is not found.
- gh-88267: Avoid exporting Python symbols in linked Windows applications when the core is built as static.
- bpo-41916: Allow override of ac_cv_cxx_thread so that cross compiled python can set -pthread for CXX.

### WindowsÂ¶

- gh-100180: Update Windows installer to OpenSSL 1.1.1s
- gh-99191: Use `_MSVC_LANG >= 202002L` instead of less-precise```
_MSC_VER
>=1929
```
to more accurately test for C++20 support in`PC/_wmimodule.cpp` .
- gh-79218: Define `MS_WIN64` for Mingw-w64 64bit, fix cython compilation
failure.
- gh-99941: Ensure that `asyncio.Protocol.data_received()` receives an
immutable`bytes` object (as documented), instead of`bytearray` .
- bpo-43984: `winreg.SetValueEx()` now leaves the target value untouched
in the case of conversion errors. Previously,`-1` would be written in
case of such errors.
- bpo-34816: `hasattr(ctypes.windll, 'nonexistant')` now returns`False` instead of raising`OSError` .

### macOSÂ¶

### Tools/DemosÂ¶

- bpo-45256: Fix a bug that caused an `AttributeError` to be raised in`python-gdb.py` when`py-locals` is used without a frame.
- gh-100342: Add missing `NULL` check for possible allocation failure in`*args` parsing in Argument Clinic.

### C APIÂ¶

- gh-99947: Raising SystemError on import will now have its cause be set to the original unexpected exception.
- gh-99240: In argument parsing, after deallocating newly allocated memory, reset its pointer to NULL.
- gh-98724: The `Py_CLEAR` ,`Py_SETREF` and`Py_XSETREF` macros now only evaluate their arguments once. If an
argument has side effects, these side effects are no longer duplicated.
Patch by Victor Stinner.

## Python 3.12.0 alpha 3Â¶

*Release date: 2022-12-06*

### SecurityÂ¶

- gh-100001: `python -m http.server` no longer allows terminal control
characters sent within a garbage request to be printed to the stderr
server log.This is done by changing the `http.server``BaseHTTPRequestHandler``.log_message` method to replace control
characters with a`\x`*HH*
hex escape before printing.
- gh-87604: Avoid publishing list of active per-interpreter audit hooks via the `gc` module

### Core and BuiltinsÂ¶

- gh-99891: Fix a bug in the tokenizer that could cause infinite recursion when showing syntax warnings that happen in the first line of the source. Patch by Pablo Galindo
- gh-91054: Add `PyCode_AddWatcher()` and`PyCode_ClearWatcher()` APIs to register callbacks to receive
notification on creation and destruction of code objects.
- gh-99729: Fix an issue that could cause frames to be visible to Python code as they are being torn down, possibly leading to memory corruption or hard crashes of the interpreter.
- gh-99708: Fix bug where compiler crashes on an if expression with an empty body block.
- gh-99578: Fix a reference bug in `_imp.create_builtin()` after the
creation of the first sub-interpreter for modules`builtins` and`sys` . Patch by Victor Stinner.
- gh-99581: Fixed a bug that was causing a buffer overflow if the tokenizer copies a line missing the newline character from a file that is as long as the available tokenizer buffer. Patch by Pablo galindo
- gh-99553: Fix bug where an `ExceptionGroup` subclass can wrap a`BaseException` .

### LibraryÂ¶

### Core and BuiltinsÂ¶

- gh-99377: Add audit events for thread creation and clear operations.
- gh-98686: Remove the `BINARY_OP_GENERIC` and`COMPARE_OP_GENERIC` âspecializationsâ.
- gh-99298: Remove the remaining error paths for attribute specializations, and refuse to specialize attribute accesses on types that havenât had `PyType_Ready()` called on them yet.
- gh-99127: Allow some features of `syslog` to the main interpreter
only. Patch by Donghee Na.
- gh-91053: Optimizing interpreters and JIT compilers may need to invalidate internal metadata when functions are modified. This change adds the ability to provide a callback that will be invoked each time a function is created, modified, or destroyed.
- gh-90994: Improve error messages when thereâs a syntax error with call arguments. The following three cases are covered: - No value is assigned to a named argument, eg `foo(a=)` . - A value is assigned to a star
argument, eg`foo(*args=[0])` . - A value is assigned to a double-star
keyword argument, eg`foo(**kwarg={'a': 0})` .
- bpo-45026: Optimize the `range` object iterator. It is now smaller,
faster iteration of ranges containing large numbers. Smaller pickles,
faster unpickling.
- bpo-31718: Raise `ValueError` instead of`SystemError` when
methods of uninitialized`io.IncrementalNewlineDecoder` objects are
called. Patch by Oren Milman.
- bpo-38031: Fix a possible assertion failure in `io.FileIO` when the
opener returns an invalid file descriptor.

### LibraryÂ¶

- gh-100001: Also escape s in the http.server BaseHTTPRequestHandler.log_message so that it is technically possible to parse the line and reconstruct what the original data was. Without this a xHH is ambiguous as to if it is a hex replacement we put in or the characters râxâ came through in the original request line.
- gh-99957: Add `frozen_default` parameter to`typing.dataclass_transform()` .
- gh-79033: Fix `asyncio.Server.wait_closed()` to actually do what the
docs promise â wait for all existing connections to complete, after
closing the server.
- gh-51524: Fix bug when calling trace.CoverageResults with valid infile.
- gh-99645: Fix a bug in handling class cleanups in `unittest.TestCase` .  Now`addClassCleanup()` uses separate lists
for different`TestCase` subclasses, and`doClassCleanups()` only
cleans up the particular class.
- gh-99508: Fix `TypeError` in`Lib/importlib/_bootstrap_external.py` while calling`_imp.source_hash()` .
- gh-66285: Fix `asyncio` to not share event loop and signal wakeupfd
in forked processes. Patch by Kumar Aditya.
- gh-97001: Release the GIL when calling termios APIs to avoid blocking threads.
- gh-92647: Use final status of an enum to determine lookup or creation branch of functional API.
- gh-99388: Add *loop_factory* parameter to`asyncio.run()` to allow
specifying a custom event loop factory. Patch by Kumar Aditya.
- gh-99341: Fix `ast.increment_lineno()` to also cover`ast.TypeIgnore` when changing line numbers.
- gh-99382: Check the number of arguments in substitution in user generics containing a `TypeVarTuple` and one or more`TypeVar` .
- gh-99379: Fix substitution of `ParamSpec` followed by`TypeVarTuple` in generic aliases.
- gh-99344: Fix substitution of `TypeVarTuple` and`ParamSpec` together in user generics.
- gh-99284: Remove `_use_broken_old_ctypes_structure_semantics_` old
untested and undocumented hack from`ctypes` .
- gh-99201: Fix `IndexError` when initializing the config variables on
Windows if`HAVE_DYNAMIC_LOADING` is not set.
- gh-99240: Fix double-free bug in Argument Clinic `str_converter` by
extracting memory clean up to a new`post_parsing` section.
- gh-64490: Fix refcount error when arguments are packed to tuple in Argument Clinic.
- gh-99029: `pathlib.PurePath.relative_to()` now treats naked Windows
drive paths as relative. This brings its behaviour in line with other
parts of pathlib.
- gh-98253: The implementation of the typing module is now more resilient to reference leaks in binary extension modules. Previously, a reference leak in a typed C API-based extension module could leak internals of the typing module, which could in turn introduce leaks in essentially any other package with typed function signatures. Although the typing package is not the original source of the problem, such non-local dependences exacerbate debugging of large-scale projects, and the implementation was therefore changed to reduce harm by providing better isolation.
- gh-98458: Fix infinite loop in unittest when a self-referencing chained exception is raised
- gh-93453: `asyncio.get_event_loop()` and many other`asyncio` functions like`asyncio.ensure_future()` ,`asyncio.shield()` or`asyncio.gather()` , and also the`get_event_loop()` method of`asyncio.BaseDefaultEventLoopPolicy` now raise a`RuntimeError` if called when there is no running event loop and the
current event loop was not set. Previously they implicitly created and set
a new current event loop.`DeprecationWarning` is no longer emitted
if there is no running event loop but the current event loop was set.
- gh-97966: On `os.uname_result` , restored expectation that`_fields` and`_asdict` would include all six properties including`processor` .
- gh-98248: Provide informative error messages in `struct.pack()` when
its integral arguments are not in range.
- gh-98108: `zipfile.Path` is now pickleable if its initialization
parameters were pickleable (e.g. for file system paths).
- gh-98098: Created packages from zipfile and test_zipfile modules, separating `zipfile.Path` functionality.
- gh-82836: Fix `is_private` properties in the`ipaddress` module. Previously non-private networks (0.0.0.0/0) would
return`True` from this method; now they correctly return`False` .
- gh-96828: Add an `OP_ENABLE_KTLS` option for enabling the use
of the kernel TLS (kTLS). Patch by Illia Volochii.
- gh-88863: To avoid apparent memory leaks when `asyncio.open_connection()` raises, break reference cycles generated
by local exception and future instances (which has exception instance as
its member var). Patch by Dong Uk, Kang.
- gh-91078: `TarFile.next()` now returns`None` when called on an
empty tarfile.
- bpo-47220: Document the optional *callback* parameter of`WeakMethod` . Patch by GÃ©ry Ogam.
- bpo-44817: Ignore WinError 53 (ERROR_BAD_NETPATH), 65 (ERROR_NETWORK_ACCESS_DENIED) and 161 (ERROR_BAD_PATHNAME) when using ntpath.realpath().
- bpo-41260: Rename the *fmt* parameter of the pure Python implementation of`datetime.date.strftime()` to*format* .
- bpo-15999: All built-in functions now accept arguments of any type instead of just `bool` and`int` for boolean parameters.

### DocumentationÂ¶

- gh-99931: Use sphinxext-opengraph to generate OpenGraph metadata.
- gh-89682: Reworded docstring of the default `__contains__` to clarify
that it returns a`bool` .
- gh-88330: Improved the description of what a resource is in importlib.resources docs.
- gh-92892: Document that calling variadic functions with ctypes requires special care on macOS/arm64 (and possibly other platforms).
- bpo-41825: Restructured the documentation for the `os.wait*` family of functions, and improved the docs for`os.waitid()` with more explanation of the possible argument
constants.

### TestsÂ¶

- gh-99892: Skip test_normalization() of test_unicodedata if it fails to download NormalizationTest.txt file from pythontest.net. Patch by Victor Stinner.
- gh-99934: Correct test_marsh on (32 bit) x86: test_deterministic sets was failing.
- gh-99741: Weâve implemented multi-phase init (PEP 489/630/687) for the internal (for testing) _xxsubinterpreters module.
- gh-99659: Optional big memory tests in `test_sqlite3` now catch the
correct`sqlite.DataError` exception type in case of too large
strings and/or blobs passed.
- gh-99593: Cover the Unicode C API with tests.
- gh-96002: Add functional test for Argument Clinic.

### BuildÂ¶

- gh-99086: Fix `-Wimplicit-int` ,`-Wstrict-prototypes` , and`-Wimplicit-function-declaration` compiler warnings in**configure** checks.
- gh-99337: Fix a compilation issue with GCC 12 on macOS.
- gh-99289: Add a `COMPILEALL_OPTS` variable in Makefile to override`compileall` options (default:`-j0` ) in`make install` . Also
merged the`compileall` commands into a single command building .pyc
files for the all optimization levels (0, 1, 2) at once. Patch by Victor
Stinner.
- gh-98872: Fix a possible fd leak in `Programs/_freeze_module.c` introduced in Python 3.11.
- gh-88226: Always define `TARGET_*` labels in`Python/ceval.c` , even if`USE_COMPUTED_GOTOS` is disabled.  This allows breakpoints to be set at
those labels in (for instance)`gdb` .

### WindowsÂ¶

- gh-99345: Use faster initialization functions to detect install location for Windows Store package
- gh-98629: Fix initialization of `sys.version` and`sys._git` on
Windows
- gh-99442: Fix handling in Python install manager when `argv[0]` does not
include a file extension.
- bpo-40882: Fix a memory leak in `multiprocessing.shared_memory.SharedMemory` on Windows.

### macOSÂ¶

### Tools/DemosÂ¶

- gh-64490: Argument Clinic varargs bugfixes 
  - Fix out-of-bounds error in `_PyArg_UnpackKeywordsWithVararg()` .
  - Fix incorrect check which allowed more than one varargs in clinic.py.
  - Fix miscalculation of `noptargs` in generated code.
  - Do not generate `noptargs` when there is a vararg argument and no optional argument.

### C APIÂ¶

- gh-98680: `PyBUF_*` constants were marked as part of Limited API of
Python 3.11+. These were available in 3.11.0 with`Py_LIMITED_API` defined for 3.11, and are necessary to use the
buffer API.
- gh-99612: Fix `PyUnicode_DecodeUTF8Stateful()` for ASCII-only data:`*consumed` was not set.
- gh-47146: The `structmember.h` header is deprecated. Its non-deprecated
contents are now available just by including`Python.h` , with a`Py_` prefix added if it was missing. (Deprecated contents are`T_OBJECT` ,`T_NONE` , and no-op flags.) Patch by Petr
Viktorin, based on earlier work by Alexander Belopolsky and Matthias
Braun.

## Python 3.12.0 alpha 2Â¶

*Release date: 2022-11-14*

### SecurityÂ¶

- gh-98433: The IDNA codec decoder used on DNS hostnames by `socket` or`asyncio` related name resolution functions no longer involves a
quadratic algorithm. This prevents a potential CPU denial of service if an
out-of-spec excessive length hostname involving bidirectional characters
were decoded. Some protocols such as`urllib` http`3`*xx*
redirects potentially allow for an attacker to supply such a name.Individual labels within an IDNA encoded DNS name will now raise an error early during IDNA decoding if they are longer than 1024 unicode characters given that each decoded DNS label must be 63 or fewer characters and the entire decoded DNS name is limited to 255. Only an application presenting a hostname or label consisting primarily of **RFC 3454** section 3.1
âNothingâ characters to be removed would run into of this new limit. See
also**RFC 5894** section 6 and**RFC 3491** .
- gh-98739: Update bundled libexpat to 2.5.0

### Core and BuiltinsÂ¶

- gh-81057: The docs clearly say that `PyImport_Inittab` ,`PyImport_AppendInittab()` , and`PyImport_ExtendInittab()` should not be used after`Py_Initialize()` has been called. We now
enforce this for the two functions.  Additionally, the runtime now uses an
internal copy of`PyImport_Inittab` , to guard against
modification.
- gh-99298: Fix an issue that could potentially cause incorrect error handling for some bytecode instructions.
- gh-99254: The compiler now removes all unused constants from code objects (except the first one, which may be a docstring).
- gh-99205: Fix an issue that prevented `PyThreadState` and`PyInterpreterState` memory from being freed properly.
- gh-81057: The 18 global C variables holding the state of the allocators have been moved to `_PyRuntimeState` .  This is a strictly internal
change with no change in behavior.
- gh-99181: Fix failure in `except*` with unhashable
exceptions.
- gh-99204: Fix calculation of `sys._base_executable` when inside a
POSIX virtual environment using copies of the python binary when the base
installation does not provide the executable name used by the venv.
Calculation will fall back to alternative names (âpython<MAJOR>â,
âpython<MAJOR>.<MINOR>â).
- gh-96055: Update `faulthandler` to emit an error message with the
proper unexpected signal number. Patch by Donghee Na.
- gh-99153: Fix location of `SyntaxError` for a`try` block
with both`except` and`except*` .
- gh-98686: Merge the adaptive opcode logic into each instructionâs unquickened variant, and merge the logic in `EXTENDED_ARG_QUICK` into`EXTENDED_ARG` . With these changes, the quickening that happens at
code object creation is now only responsible for initializing warmup
counters and inserting superinstructions.
- gh-99103: Fix the error reporting positions of specialized traceback anchors when the source line contains Unicode characters.
- gh-99139: Improve the error suggestion for `NameError` exceptions for
instances. Now if a`NameError` is raised in a method and the
instance has an attribute thatâs exactly equal to the name in the
exception, the suggestion will include`self.<NAME>` instead of the
closest match in the method scope. Patch by Pablo Galindo
- gh-98401: Octal escapes with value larger than `0o377` (ex:`"\477"` ),
deprecated in Python 3.11, now produce a`SyntaxWarning` , instead of`DeprecationWarning` . In a future Python version they will be
eventually a`SyntaxError` . Patch by Victor Stinner.
- gh-98401: A backslash-character pair that is not a valid escape sequence now generates a `SyntaxWarning` , instead of`DeprecationWarning` .  For example,`re.compile("\d+\.\d+")` now
emits a`SyntaxWarning` (`"\d"` is an invalid escape sequence), use
raw strings for regular expression:`re.compile(r"\d+\.\d+")` . In a
future Python version,`SyntaxError` will eventually be raised,
instead of`SyntaxWarning` . Patch by Victor Stinner.
- gh-96793: Handle StopIteration and StopAsyncIteration raised in generator or coroutines in the bytecode, rather than in wrapping C code.
- gh-98931: Improve the `SyntaxError` error message when the user types`import x from y` instead of`from y import x` . Patch by Pablo Galindo
- gh-98852: Fix subscription of type aliases containing bare generic types or types like `TypeVar` : for example`tuple[A, T][int]` and`tuple[TypeVar, T][int]` , where`A` is a generic type, and`T` is a type variable.
- gh-98925: Lower the recursion depth for marshal on WASI to support (in-development) wasmtime 2.0.
- gh-98783: Fix multiple crashes in debug mode when `str` subclasses are
used instead of`str` itself.
- gh-98811: Use complete source locations to simplify detection of `__future__` imports which are not at the beginning of the file. Also
corrects the offset in the exception raised in one case, which was off by
one and impeded highlighting.
- gh-96793: Add specialization of `FOR_ITER` for generators. Saves
multiple layers of dispatch and checking to get from the`FOR_ITER` instruction in the caller to the`RESUME` in
the generator.
- gh-98762: Fix source locations of `match` sub-patterns.
- gh-98586: Added the methods `PyObject_Vectorcall()` and`PyObject_VectorcallMethod()` to the Limited API
along with the auxiliary macro constant`PY_VECTORCALL_ARGUMENTS_OFFSET` .The availability of these functions enables more efficient **PEP 590** vector calls from binary extension modules that avoid argument
boxing/unboxing overheads.
- gh-99257: Fix an issue where member descriptors (such as those for `__slots__` ) could behave incorrectly or crash instead of
raising a`TypeError` when accessed via an instance of an invalid
type.
- gh-93143: Rather than changing `co_code` , the
interpreter will now display a`RuntimeWarning` and assign`None` to any fast locals that are left unbound after jumps or`del` statements executed while tracing.
- gh-96421: When calling into Python code from C code, through `PyEval_EvalFrameEx()` or a related C-API function, a shim frame in
inserted into the call stack. This occurs in the`_PyEval_EvalFrameDefault()` function. The extra frame should be
invisible to all Python and most C extensions, but out-of-process
profilers and debuggers need to be aware of it. These shim frames can be
detected by checking`frame->owner == FRAME_OWNED_BY_CSTACK` .Extensions implementing their own interpreters using PEP 523 need to be aware of this shim frame and the changes to the semantics of `RETURN_VALUE` ,`YIELD_VALUE` , and`RETURN_GENERATOR` , which now clear the frame.

### BuildÂ¶

### Core and BuiltinsÂ¶

- gh-98686: Quicken all code objects, and specialize adaptive bytecode instructions more aggressively.
- gh-92119: Print exception class name instead of its string representation when raising errors from `ctypes` calls.
- gh-91058: `ImportError` raised from failed```
from <module> import
<name>
```
now include suggestions for the value of`<name>` based on the
available names in`<module>` . Patch by Pablo Galindo
- gh-96793: The `FOR_ITER` now leaves the iterator on the stack on
termination of the loop. This is to assist specialization of loops for
generators.
- gh-90716: Add _pylong.py module. It includes asymptotically faster algorithms that can be used for operations on integers with many digits. It is used by longobject.c to speed up some operations.
- gh-95389: Expose `ETH_P_ALL` and some of the
ETHERTYPE_* constants in`socket` .
Patch by Noam Cohen.

### LibraryÂ¶

- gh-93696: Allow `pdb` to locate source for frozen modules in the
standard library.
- gh-99418: Fix bug in `urllib.parse.urlparse()` that causes URL schemes
that begin with a digit, a plus sign, or a minus sign to be parsed
incorrectly.
- gh-94597: Deprecate `asyncio.AbstractChildWatcher` to be removed in
Python 3.14. Patch by Kumar Aditya.
- gh-99305: Improve performance of `secrets.token_hex()` .
- gh-74044: Fixed bug where `inspect.signature()` reported incorrect
arguments for decorated methods.
- gh-99275: Fix `SystemError` in`ctypes` when exception was not set
during`__initsubclass__` .
- gh-99277: Remove older version of `_SSLProtocolTransport.get_write_buffer_limits` in`asyncio.sslproto`
- gh-99248: fix negative numbers failing in verify()
- gh-99155: Fix `statistics.NormalDist` pickle with`0` and`1` protocols.
- gh-93464: `enum.auto()` is now correctly activated when combined with
other assignment values.  E.g.`ONE = auto(), 'some text'` will now
evaluate as`(1, 'some text')` .
- gh-99134: Update the bundled copy of pip to version 22.3.1.
- gh-92584: Remove the `distutils` package. It was deprecated in Python
3.10 by**PEP 632** âDeprecate distutils moduleâ. For projects still using`distutils` and cannot be updated to something else, the`setuptools` project can be installed: it still provides`distutils` . Patch by Victor
Stinner.
- gh-98999: Now `_pyio` is consistent with`_io` in raising`ValueError` when executing methods over closed buffers.
- gh-83004: Clean up refleak on failed module initialisation in `_zoneinfo`
- gh-83004: Clean up refleaks on failed module initialisation in `_pickle`
- gh-83004: Clean up refleak on failed module initialisation in `_io` .
- gh-98897: Fix memory leak in `math.dist()` when both points donât have
the same dimension. Patch by Kumar Aditya.
- gh-98878: Use the frame bound builtins when offering a name suggestion in `traceback` to prevent crashing when`__builtins__` is not a dict.
- gh-98139: In `importlib._bootstrap` , enhance namespace package repr
to`<module 'x' (namespace) from ['path']>` .
- gh-90352: Fix `_SelectorDatagramTransport` to inherit from`DatagramTransport` in`asyncio` . Patch by Kumar
Aditya.
- gh-98793: Fix argument typechecks in `_overlapped.WSAConnect()` and`_overlapped.Overlapped.WSASendTo()` functions.
- gh-98744: Prevent crashing in `traceback` when retrieving the
byte-offset for some source files that contain certain unicode characters.
- gh-98740: Fix internal error in the `re` module which in very rare
circumstances prevented compilation of a regular expression containing a
conditional expression without the
âelseâ branch.
- gh-98703: Fix `asyncio.StreamWriter.drain()` to call`protocol.connection_lost` callback only once on Windows.
- gh-98624: Add a mutex to unittest.mock.NonCallableMock to protect concurrent access to mock attributes.
- gh-98658: The `array.array` class now supports subscripting, making
it a generic type.
- gh-98284: Improved `TypeError` message for undefined abstract
methods of a`abc.ABC` instance. The names of the missing methods
are surrounded by single-quotes to highlight them.
- gh-96151: Allow `BUILTINS` to be a valid field name for frozen
dataclasses.
- gh-98086: Make sure `patch.dict()` can be applied on async functions.
- gh-72719: Remove modules `asyncore` and`asynchat` , which were
deprecated by**PEP 594** .
- gh-96192: Fix handling of `bytes` path-like objects in`os.ismount()` .
- gh-94172: `ftplib` : Remove the`FTP_TLS.ssl_version` class
attribute: use the*context* parameter instead. Patch by Victor Stinner
- gh-94172: Remove the *keyfile* and*certfile* parameters from the`ftplib` ,`imaplib` ,`poplib` and`smtplib` modules,
and the*key_file* ,*cert_file* and*check_hostname* parameters from the`http.client` module, all deprecated since Python 3.6. Use the*context* parameter (*ssl_context* in`imaplib` ) instead. Patch by
Victor Stinner.
- gh-83638: Add the `autocommit` attribute to`sqlite3.Connection` and the*autocommit* parameter to`sqlite3.connect()` to control**PEP 249** -compliant transaction
handling. Patch by Erlend E.
Aasland.
- gh-92452: Fixed a race condition that could cause `sysconfig.get_config_var()` to incorrectly return`None` in
multi-threaded programs.
- gh-91803: Fix an error when using a method of objects mocked with `unittest.mock.create_autospec()` after it was sealed with`unittest.mock.seal()` function.
- bpo-38523: `shutil.copytree()` now applies the*ignore_dangling_symlinks* argument recursively.
- bpo-40358: Add walk_up argument in `pathlib.PurePath.relative_to()` .
- bpo-36267: Fix IndexError in `argparse.ArgumentParser` when a`store_true` action is given an explicit argument.

### DocumentationÂ¶

- gh-98832: Changes wording of docstring for `pathlib.Path.iterdir()` .
- gh-97966: Update uname docs to clarify the special nature of the platform attribute and to indicate when it became late-bound.

### TestsÂ¶

- gh-98903: The Python test suite now fails with exit code 4 if no tests ran. It should help detecting typos in test names and test methods.
- gh-98713: Fix a bug in the `typing` tests where a test relying on
CPython-specific implementation details was not decorated with`@cpython_only` and was not skipped on other implementations.
- gh-87390: Add tests for star-unpacking with PEP 646, and some other miscellaneous PEP 646 tests.
- gh-96853: Added explicit coverage of `Py_Initialize` (and hence`Py_InitializeEx` ) back to the embedding tests (all other embedding
tests migrated to`Py_InitializeFromConfig` in Python 3.11)
- bpo-34272: Some C API tests were moved into the new Lib/test/test_capi/ directory.

### BuildÂ¶

- gh-99086: Fix `-Wimplicit-int` compiler warning in**configure** check for`PTHREAD_SCOPE_SYSTEM` .
- gh-99016: Fix build with `PYTHON_FOR_REGEN=python3.8` .
- gh-97731: Specify the full path to the source location for ```
make
docclean
```
(needed for cross-builds).
- gh-98949: Drop unused build dependency on `readelf` .
- gh-98989: Use `python3.11` , if available, for regeneration and freezing.
- gh-98831: Add new tooling, in `Tools/cases_generator` , to generate the
interpreter switch statement from a list of opcode definitions. This only
affects adding, modifying or removing instruction definitions. The
instruction definitions now live in`Python/bytecodes.c` , in the form of
a custom DSL (under development).
The tooling reads this file and writes`Python/generated_cases.c.h` ,
which is then included by`Python/ceval.c` to provide most of the cases
of the main interpreter switch.
- gh-98817: Remove PCbuild/lib.pyproj: itâs not used for anything, is only a minor convenience for Visual Studio users (who probably mostly donât even know about it), and it takes a lot of maintenance effort to keep updated.
- gh-98776: Fix `make regen-test-levenshtein` for out-of-tree builds.
- gh-98707: Donât use vendored `libmpdec` headers if`--with-system-libmpdec` is passed to**configure** . Donât
use vendored`libexpat` headers if`--with-system-expat` is
passed to**configure** .

### WindowsÂ¶

- gh-98689: Update Windows builds to zlib v1.2.13. v1.2.12 has **CVE 2022-37434** , but the vulnerable`inflateGetHeader` API is not used
by Python.
- gh-98790: Assumes that a missing `DLLs` directory means that standard
extension modules are in the executableâs directory.
- gh-98745: Update `py.exe` launcher to install 3.11 by default and
3.12 on request.
- gh-98692: Fix the Python install manager ignoring unrecognized shebang lines instead of treating them as local paths
- gh-94328: Update Windows installer to use SQLite 3.39.4.

### macOSÂ¶

- gh-94328: Update macOS installer to SQLite 3.39.4.

### C APIÂ¶

- gh-98724: The `Py_CLEAR` ,`Py_SETREF` and`Py_XSETREF` macros now only evaluate their argument once. If the
argument has side effects, these side effects are no longer duplicated.
Patch by Victor Stinner.
- gh-98978: Fix use-after-free in `Py_SetPythonHome(NULL)` ,`Py_SetProgramName(NULL)` and`_Py_SetProgramFullPath(NULL)` function
calls. Issue reported by Benedikt Reinartz. Patch by Victor Stinner.
- gh-98410: Add `getbufferproc` and`releasebufferproc` to the stable
API.
- gh-98610: Some configurable capabilities of sub-interpreters have changed. They always allow subprocesses ( `subprocess` ) now, whereas before
subprocesses could be optionally disallowed for a sub-interpreter. Instead`os.exec()` can now be disallowed. Disallowing daemon threads is now
supported.  Disallowing all threads is still allowed, but is never done by
default. Note that the optional restrictions are only available through`_Py_NewInterpreterFromConfig()` , which isnât a public API. They do not
affect the main interpreter, nor`Py_NewInterpreter()` .
- gh-98608: A `_PyInterpreterConfig` has been added and`_Py_NewInterpreter()` has been renamed to`_Py_NewInterpreterFromConfig()` .  The âisolated_subinterpretersâ
argument is now a granular config that captures the previous behavior.
Note that this is all âprivateâ API.
- gh-96853: `Py_InitializeEx` now correctly calls`PyConfig_Clear` after
initializing the interpreter (the omission didnât cause a memory leak only
because none of the dynamically allocated config fields are populated by
the wrapper function)
- gh-91248: Add `PyFrame_GetVar()` and`PyFrame_GetVarString()` functions to get a frame variable by its name. Patch by Victor Stinner.

## Python 3.12.0 alpha 1Â¶

*Release date: 2022-10-25*

### SecurityÂ¶

- gh-97616: Fix multiplying a list by an integer ( `list *= int` ): detect
the integer overflow when the new allocated length is close to the maximum
size. Issue reported by Jordan Limor.  Patch by Victor Stinner.
- gh-97514: On Linux the `multiprocessing` module returns to using
filesystem backed unix domain sockets for communication with the*forkserver* process instead of the Linux abstract socket namespace.  Only
code that chooses to use the âforkserverâ start method is affected.Abstract sockets have no permissions and could allow any user on the system in the same network namespace (often the whole system) to inject code into the multiprocessing *forkserver* process. This was a potential privilege escalation. Filesystem based
socket permissions restrict this to the*forkserver* process user as was
the default in Python 3.8 and earlier.This prevents Linux **CVE 2022-42919** .
- gh-87389: `http.server` : Fix an open redirection vulnerability in the
HTTP server when an URI path starts with`//` .  Vulnerability
discovered, and initial fix proposed, by Hamza Avvan.
- gh-79096: LWPCookieJar and MozillaCookieJar create files with file mode 600 instead of 644 (Microsoft Windows is not affected)
- gh-92888: Fix `memoryview` use after free when accessing the backing
buffer in certain cases.
- gh-68966: The deprecated mailcap module now refuses to inject unsafe text (filenames, MIME types, parameters) into shell commands. Instead of using such text, it will warn and act as if a match was not found (or for test commands, as if the test failed).

### Core and BuiltinsÂ¶

- gh-98374: Suppress ImportError for invalid query for help() command. Patch by Donghee Na.
- gh-98461: Fix source location in bytecode for list, set and dict comprehensions as well as generator expressions.
- gh-98354: Added unicode check for `name` attribute of`spec` argument
passed in`_imp.create_builtin()` function.
- gh-98398: Fix source location of âassertâ bytecodes.
- gh-98390: Fix location of sub-expressions of boolean expressions, by reducing their scope to that of the sub-expression.
- gh-98254: Modules from the standard library are now potentially suggested as part of the error messages displayed by the interpreter when an `NameError` is raised to the top level. Patch by Pablo Galindo
- gh-97997: Add running column offset to the tokenizer state to avoid calculating AST column information with pointer arithmetic.
- gh-97973: Modify the tokenizer to return all necessary information the parser needs to set location information in the AST nodes, so that the parser does not have to calculate those doing pointer arithmetic.

### LibraryÂ¶

- gh-96078: `os.sched_yield()` now release the GIL while calling
sched_yield(2). Patch by Donghee Na.
- gh-97955: Migrate `zoneinfo` to Argument Clinic.

### Core and BuiltinsÂ¶

- gh-97912: The compiler now avoids quadratic behavior when finding which instructions should use the `LOAD_FAST_CHECK` opcode.
- gh-97002: Fix an issue where several frame objects could be backed by the same interpreter frame, possibly leading to corrupted memory and hard crashes of the interpreter.
- gh-97943: Bugfix: `PyFunction_GetAnnotations()` should return a
borrowed reference. It was returning a new reference.
- gh-97922: The Garbage Collector now runs only on the eval breaker mechanism of the Python bytecode evaluation loop instead on object allocations. The GC can also run when `PyErr_CheckSignals()` is
called so C extensions that need to run for a long time without executing
any Python code also have a chance to execute the GC periodically.
- gh-65961: When `__package__` is different than`__spec__.parent` ,
raise a`DeprecationWarning` instead of`ImportWarning` .Also remove `importlib.util.set_package()` which was scheduled for
removal.
- gh-97850: Long deprecated, `module_repr()` should now be completely
eradicated.
- gh-86298: In cases where `warnings.warn_explicit()` consults the
moduleâs loader, an`DeprecationWarning` is issued when`m.__loader__` differs from`m.__spec__.loader` .
- gh-97779: Ensure that all Python frame objects are backed by âcompleteâ frames.
- gh-91052: Add API for subscribing to modification events on selected dictionaries.
- gh-97752: Fix possible data corruption or crashes when accessing the `f_back` member of newly-created generator or coroutine frames.
- gh-97591: Fixed a missing incref/decref pair in `Exception.__setstate__()` . Patch by Ofey Chan.
- gh-97670: Remove the `sys.getdxp()` function and the`Tools/scripts/analyze_dxp.py` script. DXP stands for âdynamic execution
pairsâ. They were related to`DYNAMIC_EXECUTION_PROFILE` and`DXPAIRS` macros which have been removed in Python 3.11. Python can now be built
with`./configure --enable-pystats` to gather
statistics on Python opcodes.  Patch by Victor Stinner.
- gh-94526: Fix the Python path configuration used to initialized `sys.path` at Python startup. Paths are no longer encoded to
UTF-8/strict to avoid encoding errors if it contains surrogate characters
(bytes paths are decoded with the surrogateescape error handler). Patch by
Victor Stinner.
- gh-96670: The parser now raises `SyntaxError` when parsing source
code containing null bytes. Patch by Pablo Galindo
- gh-96975: Fix a crash occurring when `PyEval_GetFrame()` is called
while the topmost Python frame is in a partially-initialized state.
- gh-96848: Fix command line parsing: reject `-X int_max_str_digits` option with no value (invalid) when the`PYTHONINTMAXSTRDIGITS` environment variable is set to a valid
limit. Patch by Victor Stinner.
- gh-95921: Fix overly-broad source position information for chained comparisons used as branching conditions.

### LibraryÂ¶

- gh-96821: Fix undefined behaviour in `audioop.c` .

### Core and BuiltinsÂ¶

- gh-96821: Fix undefined behaviour in `_testcapimodule.c` .
- gh-95778: When `ValueError` is raised if an integer is larger than
the limit, mention the`sys.set_int_max_str_digits()` function in the
error message. Patch by Victor Stinner.
- gh-96387: At Python exit, sometimes a thread holding the GIL can wait forever for a thread (usually a daemon thread) which requested to drop the GIL, whereas the thread already exited. To fix the race condition, the thread which requested the GIL drop now resets its request before exiting. Issue discovered and analyzed by Mingliang ZHAO. Patch by Victor Stinner.
- gh-96864: Fix a possible assertion failure, fatal error, or `SystemError` if a line tracing event raises an exception while
opcode tracing is enabled.
- gh-95778: The `PyLong_FromString` function was refactored to make it
more maintainable and extensible.
- gh-96678: Fix undefined behaviour in C code of null pointer arithmetic.
- gh-96754: Make sure that all frame objects created are created from valid interpreter frames. Prevents the possibility of invalid frames in backtraces and signal handlers.
- gh-90997: Improve the performance of reading and writing inline bytecode caches on some platforms.
- gh-96751: Remove dead code from `CALL_FUNCTION_EX` opcode.
- gh-90751: `memoryview` now supports half-floats. Patch by Donghee
Na and Antoine Pitrou.
- gh-96678: Fix case of undefined behavior in ceval.c

### LibraryÂ¶

### Core and BuiltinsÂ¶

- gh-96636: Ensure that tracing, `sys.setrace()` , is turned on
immediately. In pre-release versions of 3.11, some tracing events might
have been lost when turning on tracing in a`__del__` method or
interrupt.
- gh-96572: Fix use after free in trace refs build mode. Patch by Kumar Aditya.
- gh-96611: When loading a file with invalid UTF-8 inside a multi-line string, a correct SyntaxError is emitted.
- gh-96612: Make sure that incomplete frames do not show up in tracemalloc traces.
- gh-90230: Fix compiler warnings and test failures when building with `--enable-pystats` .
- gh-96587: Correctly raise `SyntaxError` on exception groups (**PEP 654** )
on python versions prior to 3.11
- gh-96569: Remove two cases of undefined behavior, by adding NULL checks.
- gh-96582: Fix possible `NULL` pointer dereference in`_PyThread_CurrentFrames` . Patch by Kumar Aditya.
- gh-91079: Separate Python recursion checking from C recursion checking which reduces the chance of C stack overflow and allows the recursion limit to be increased safely.
- gh-93911: Fix an issue that could prevent `LOAD_ATTR` from
specializing properly when accessing properties.
- gh-96348: Emit a DeprecationWarning when `throw()` ,`throw()` or`athrow()` are called with more than
one argument.
- gh-95196: Disable incorrect pickling of the C implemented classmethod descriptors.
- gh-96364: Fix text signatures of `list.__getitem__` and`dict.__getitem__` .
- gh-96352: Fix `AttributeError` missing`name` and`obj` attributes in`object.__getattribute__()` . Patch by Philip Georgi.
- gh-93554: Change the jump opcodes so that all conditional jumps are forward jumps. Backward jumps are converted by the assembler into a conditional forward jump whose target is the fallthrough block (and with a reversed condition), followed by an unconditional backward jump. For example: `POP_JUMP_IF_TRUE BACKWARD_TARGET` becomes```
POP_JUMP_IF_FALSE
NEXT_BLOCK; JUMP BACKWARD_TARGET
```
.All the directed conditional jump opcodes were removed: `POP_JUMP_FORWARD_IF_TRUE` ,`POP_JUMP_BACKWARD_IF_TRUE` ,`POP_JUMP_FORWARD_IF_FALSE` ,`POP_JUMP_BACKWARD_IF_FALSE` ,`POP_JUMP_FORWARD_IF_NONE` ,`POP_JUMP_BACKWARD_IF_NONE` ,`POP_JUMP_FORWARD_IF_NOT_NONE` ,`POP_JUMP_BACKWARD_IF_NOT_NONE` .The corresponding opcodes without direction are no longer pseudo-instructions, and they implement the forward conditional jumps.
- gh-96268: Loading a file with invalid UTF-8 will now report the broken character at the correct location.
- gh-96237: The internal field `_PyInterpreterFrame.f_func` is renamed to`_PyInterpreterFrame.f_funcobj` and may be any object. The`f_globals` and`f_builtin` fields may hold junk values.It is safest to treat the `_PyInterpreterFrame` struct as opaque.
- gh-96187: Fixed a bug that caused `_PyCode_GetExtra` to return garbage
for negative indexes. Patch by Pablo Galindo
- gh-96143: Add a new `-X perf` Python command line option as well as`sys.activate_stack_trampoline()` and`sys.deactivate_stack_trampoline()` function in the`sys` module
that allows to set/unset the interpreter in a way that the Linux`perf` profiler can detect Python calls. The new`sys.is_stack_trampoline_active()` function allows to query the state
of the perf trampoline. Design by Pablo Galindo. Patch by Pablo Galindo
and Christian Heimes with contributions from Gregory P. Smith [Google] and
Mark Shannon.
- gh-96071: Fix a deadlock in `PyGILState_Ensure()` when allocating
new thread state. Patch by Kumar Aditya.
- gh-96046: `PyType_Ready()` now initializes`ht_cached_keys` and
performs additional checks to ensure that type objects are properly
configured. This avoids crashes in 3rd party packages that donât use
regular API to create new types.
- gh-96005: On WASI `ENOTCAPABLE` is now mapped to`PermissionError` . The`errno` modules exposes the new error
number.`getpath.py` now ignores`PermissionError` when it cannot
open landmark files`pybuilddir.txt` and`pyenv.cfg` .
- gh-93678: Added test a harness for direct unit tests of the compilerâs optimization stage. The `_testinternalcapi.optimize_cfg()` function runs
the optimiser on a sequence of instructions. The`CfgOptimizationTestCase` class in`test.support` has utilities for
invoking the optimizer and checking the output.
- gh-95245: Reduces the size of a âsimpleâ Python object from 8 to 6 words by moving the weakreflist pointer into the pre-header directly before the objectâs dict/values pointer.
- gh-90997: Compile virtual `try` /`except` blocks to
handle exceptions raised during`close()` or`throw()` calls through a suspended frame.
- gh-95977: Optimized calling `__get__()` with vectorcall. Patch
by Kumar Aditya.
- gh-91210: Improve error message when a parameter without a default value follows one with a default value, and show the same message, even when the non-default/default sequence is preceded by positional-only parameters.
- gh-95922: Fixed bug where the compilerâs `eliminate_empty_basic_blocks` function ignores the last block of the code unit.
- gh-95818: Skip over incomplete frames in `PyThreadState_GetFrame()` .
- gh-95876: Fix format string in `_PyPegen_raise_error_known_location` that can lead to memory corruption on some 64bit systems. The function was
building a tuple with`i` (int) instead of`n` (Py_ssize_t) for
Py_ssize_t arguments.
- gh-95605: Fix misleading contents of error message when converting an all-whitespace string to `float` .
- gh-95150: Update code object hashing and equality to consider all debugging and exception handling tables. This fixes an issue where certain non-identical code objects could be âdeduplicatedâ during compilation.
- gh-91146: Reduce allocation size of `list` from`str.split()` and`str.rsplit()` . Patch by Donghee Na and Inada Naoki.
- gh-87092: Create a âjump target labelâ abstraction in the compiler so that the compilerâs codegen stage does not work directly with basic blocks. This prepares the code for changes to the underlying CFG generation mechanism.
- gh-95355: `_PyPegen_Parser_New` now properly detects token memory
allocation errors. Patch by Honglin Zhu.
- gh-90081: Run Python code in tracer/profiler function at full speed. Fixes slowdown in earlier versions of 3.11.
- gh-95324: Emit a warning in debug mode if an object does not call `PyObject_GC_UnTrack()` before deallocation. Patch by Pablo Galindo.
- gh-95245: Merge managed dict and values pointer into a single tagged pointer to save one word in the pre-header.
- gh-93678: Add cfg_builder struct and refactor the relevant code so that a cfg can be constructed without an instance of the compiler struct.
- gh-95185: Prevented crashes in the AST constructor when compiling some absurdly long expressions like `"+0"*1000000` .`RecursionError` is
now raised instead. Patch by Pablo Galindo
- gh-93351: `ast.AST` node positions are now validated when provided
to`compile()` and other related functions. If invalid positions are
detected, a`ValueError` will be raised.
- gh-94438: Fix an issue that caused extended opcode arguments and some conditional pops to be ignored when calculating valid jump targets for assignments to the `f_lineno` attribute of frame objects. In some cases,
this could cause inconsistent internal state, resulting in a hard crash of
the interpreter.
- gh-95060: Undocumented `PyCode_Addr2Location` function now properly
returns when`addrq` argument is less than zero.
- gh-95113: Replace all `EXTENDED_ARG_QUICK` instructions with basic`EXTENDED_ARG` instructions in unquickened code. Consumers of
non-adaptive bytecode should be able to handle extended arguments the same
way they were handled in CPython 3.10 and older.
- gh-91409: Fix incorrect source location info caused by certain optimizations in the bytecode compiler.

### LibraryÂ¶

- gh-95023: Implement `os.setns()` and`os.unshare()` for Linux.
Patch by Noam Cohen.

### Core and BuiltinsÂ¶

- gh-94036: Fix incorrect source location info for some multi-line attribute accesses and method calls.
- gh-94938: Fix error detection in some builtin functions when keyword argument name is an instance of a str subclass with overloaded `__eq__` and`__hash__` . Previously it could cause SystemError or other undesired
behavior.

### LibraryÂ¶

- gh-94996: `ast.parse()` will no longer parse function definitions with
positional-only params when passed`feature_version` less than```
(3,
8)
```
. Patch by Shantanu Jain.

### Core and BuiltinsÂ¶

- gh-94739: Allow jumping within, out of, and across exception handlers in the debugger.

### LibraryÂ¶

- gh-94949: `ast.parse()` will no longer parse parenthesized context
managers when passed`feature_version` less than`(3, 9)` . Patch by
Shantanu Jain.
- gh-94947: `ast.parse()` will no longer parse assignment expressions
when passed`feature_version` less than`(3, 8)` . Patch by Shantanu
Jain.

### Core and BuiltinsÂ¶

- gh-91256: Ensures the program name is known for help text during interpreter startup.
- gh-94869: Fix the column offsets for some expressions in multi-line f-strings `ast` nodes. Patch by Pablo Galindo.
- gh-94893: Fix an issue where frame object manipulations could corrupt inline bytecode caches.
- gh-94822: Fix an issue where lookups of metaclass descriptors may be ignored when an identically-named attribute also exists on the class itself.
- gh-91153: Fix an issue where a `bytearray` item assignment could
crash if itâs resized by the new valueâs`__index__()` method.
- gh-90699: Fix reference counting bug in `bool.__repr__()` . Patch by
Kumar Aditya.
- gh-94694: Fix an issue that could cause code with multi-line method lookups to have misleading or incorrect column offset information. In some cases (when compiling a hand-built AST) this could have resulted in a hard crash of the interpreter.
- gh-93252: Fix an issue that caused internal frames to outlive failed Python function calls, possibly resulting in memory leaks or hard interpreter crashes.
- gh-94215: Fix an issue where exceptions raised by line-tracing events would cause frames to be left in an invalid state, possibly resulting in a hard crash of the interpreter.
- gh-92228: Disable the compilerâs inline-small-exit-blocks optimization for exit blocks that are associated with source code lines. This fixes a bug where the debugger cannot tell where an exception handler ends and the following code block begins.
- gh-94485: Line number of a moduleâs `RESUME` instruction is set to 0 as
specified in**PEP 626** .
- gh-94438: Account for instructions that can push NULL to the stack when setting line number in a frame. Prevents some (unlikely) crashes.
- gh-91719: Reload `opcode` when raising`unknown opcode error` in the
interpreter main loop, for C compilers to generate dispatching code
independently.
- gh-94329: Compile and run code with unpacking of extremely large sequences (1000s of elements). Such code failed to compile. It now compiles and runs correctly.
- gh-94360: Fixed a tokenizer crash when reading encoded files with syntax errors from `stdin` with non utf-8 encoded text. Patch by Pablo Galindo
- gh-88116: Fix an issue when reading line numbers from code objects if the encoded line numbers are close to `INT_MIN` . Patch by Pablo Galindo
- gh-94262: Donât create frame objects for incomplete frames. Prevents the creation of generators and closures from being observable to Python and C extensions, restoring the behavior of 3.10 and earlier.
- gh-94192: Fix error for dictionary literals with invalid expression as value.
- gh-87995: `types.MappingProxyType` instances are now hashable if
the underlying mapping is hashable.
- gh-93883: Revise the display strategy of traceback enhanced error locations. The indicators are only shown when the location doesnât span the whole line.
- gh-94163: Add `BINARY_SLICE` and`STORE_SLICE` instructions for more efficient handling and better specialization of
slicing operations, where the slice is explicit in the source code.
- gh-94021: Fix unreachable code warning in `Python/specialize.c` .
- gh-93911: Specialize `LOAD_ATTR` for objects with custom`__getattribute__` .
- gh-93955: Improve performance of attribute lookups on objects with custom `__getattribute__` and`__getattr__` . Patch by Ken Jin.
- gh-93911: Specialize `LOAD_ATTR` for`property()` attributes.
- gh-93678: Refactor compiler optimisation code so that it no longer needs the `struct assembler` and`struct compiler` passed around. Instead,
each function takes the CFG and other data that it actually needs. This
will make it possible to test this code directly.
- gh-93841: When built with `-enable-pystats` ,`sys._stats_on()` ,`sys._stats_off()` ,`sys._stats_clear()` and`sys._stats_dump()` functions have been added to enable gathering stats for parts of programs.
- gh-93516: Store offset of first traceable instruction in code object to avoid having to recompute it for each instruction when tracing.
- gh-93516: Lazily create a table mapping bytecode offsets to line numbers to speed up calculation of line numbers when tracing.
- gh-89828: `types.GenericAlias` no longer relays the`__class__` attribute. For example,`isinstance(list[int], type)` no longer returns`True` .
- gh-93678: Refactor the compiler to reduce boilerplate and repetition.
