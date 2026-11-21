---
id: collect-261121-veille-ia/docs-techniques/changelog-2-2
title: "Changelog"
domain: docs-techniques
role: reference
task: article
actors: []
dates: ["2026-06-02"]
keywords: ["crash", "instead", "module", "datetime", "patch", "object", "allocation", "memory"]
source: https://docs.python.org/release/3.15.1/whatsnew/changelog.html
source_anchor: ""
source_lines: [1501, 3000]
section: "Docs_Techniques"
---
- gh-150633: Fix the frozen importer accepting module names with embedded null bytes, which caused it to bypass the `sys.modules` cache and
create duplicate module objects.
- gh-148613: Fix a data race in the free-threaded build between `gc.set_threshold()` and garbage collection scheduling during object
allocation.
- gh-150207: Fix a crash when a memory allocation fails during tokenizer initialization. A proper `MemoryError` is now raised instead.
- gh-149805: Fix a `SystemError` when compiling a compiling`__classdict__` class annotation. Found by OSS-Fuzz in
#512907042.
- gh-149321: Do not support `none` as a lazy imports mode.

### LibraryÂ¶

- gh-75666: Fix a reference leak in `tkinter` : the Tcl commands created
for event callbacks are now deleted when a binding is replaced or unbound.
- gh-151770: Fix `datetime.datetime.fromisoformat()` raising`AssertionError` instead of`ValueError` for an out-of-range
month combined with a`24:00` time.
- gh-151665: `inspect.signature()` now works on the lazy evaluators of
type aliases and type parameters instead of raising`ValueError` .
- gh-151695: Fix a use-after-free in the `curses` module.  The encoding
of the initial screen, used by`curses.unctrl()` and`curses.ungetch()` to encode non-ASCII characters, is now kept as a
private copy instead of a borrowed pointer to a window object that may be
deallocated.
- gh-151436: Fix skewed stack trackes in the Tachyon profiler when caching is enabled and when generators and coroutines are profiled, by updating `tstate->last_profiled_frame` at every frame-removal site. The issue
resulted in total erasure of some callers. Patch by Maurycy
PawÅowski-WieroÅski.
- gh-151426: Fix impossible stack traces (callers and callees cross called, orphans and incorrect lines) in the Tachyon profiler when caching frames, by snapshotting the stack chunks before walking the frame chain on a cache miss. Patch by Maurycy PawÅowski-WieroÅski.
- gh-151403: Fixed a crash in `subprocess.Popen` (and`_posixsubprocess.fork_exec` ) when an`argv` itemâs`__fspath__()` concurrently mutates the`args` sequence
being converted.
- gh-151390: Colorize `match` in the REPL when followed by a unary`+` or`-` operator. Patch by Bartosz SÅawecki.
- gh-151126: Fix crash on unset `MemoryError` on allocation failure in`ctypes.get_errno()` .
- gh-151337: Avoid possible memory leak in `tkinter.c` on Windows.
- gh-151126: Fix a crash when `MemoryError` in`os._path_splitroot()` was not set properly.
- gh-149671: Restore compatibility with setuptools `-nspkg.pth` files in
the`site` module. Inject`sitedir` variable in the frame which
executes pth code. Patch by Victor Stinner.
- gh-151295: Fixed a crash (use-after-free) in `bytes.join()` and`bytearray.join()` that could occur if an itemâs`__buffer__()` concurrently mutates the sequence being joined.
The mutation is now reported as a`RuntimeError` instead.
- gh-109940: Fix Windows `venv` activation in`cmd.exe` to respect`VIRTUAL_ENV_DISABLE_PROMPT` .
- gh-150771: Fix `email` messages created with`shift_jis` or`euc-jp` charsets.`set_content()` now stores the payload using the
output charset (`iso-2022-jp` ) so printing the message no longer raises`UnicodeEncodeError` .
- gh-151039: Fix a crash when static `datetime` types outlive the`_datetime` module.
- gh-151021: Fix `mmap.mmap.find()` and`rfind()` to
return`-1` when searching for an empty subsequence with a start
position past the end of the mapping.
- gh-62825: Encodings âKS_C_5601-1987â, âKS X 1001â, etc are now aliases of âCP949â instead of âEUC-KRâ.
- gh-150913: Fix `sqlite3.Blob` slice assignment to raise`TypeError` and`IndexError` for type and size mismatches
respectively, even when the target slice is empty.
- gh-143008: Fix race conditions when re-initializing a `io.TextIOWrapper` object.
- gh-150662: Fix the `--gecko` collector in`profiling.sampling` that
kept every sample in memory. It now writes sample and marker data to
temporary files and reads them back, ultimately building the output file
at the end. Patch by Pablo Galindo and Maurycy PawÅowski-WieroÅski.
- gh-150750: Fix a race condition in `collections.deque.index()` with
free-threading.
- gh-148932: Fix `profiling.sampling` on Windows virtual environments to
resolve the actual Python PID from a virtual environment shim.
- gh-149816: Fix race condition in `ssl.SSLContext.sni_callback`
- gh-53144: The `email` package now supports all aliases of Python
codecs and uses MIME/IANA names for all IANA registered charsets.
- gh-149891: Add support for more encoding aliases officially registered in IANA.
- gh-149473: Calling `os.environ.clear()` now emits`os._clearenv` auditing event. Patch by Victor Stinner.
- gh-148954: Fix XML injection vulnerability in `xmlrpc.client.dumps()` where the`methodname` was not being escaped before interpolation into
the XML body.
- gh-143988: Fixed crashes in `socket.socket.sendmsg()` and`socket.socket.recvmsg_into()` that could occur if buffer sequences
are concurrently mutated.
- gh-120665: Fixed an issue where `unittest` loaders would load and
instantiate`unittest.TestCase` -derived subclasses that are also
abstract base classes, which canât be instantiated.
- gh-91099: `imaplib.IMAP4.login()` now raises exceptions with`str` instead of`bytes` . Patch by Florian Best.
- gh-101267: When a worker process terminates unexpectedly, `concurrent.futures.ProcessPoolExecutor` now sets a separate`BrokenProcessPool` exception on each
pending future instead of sharing a single instance among them all.
Sharing one exception produced malformed tracebacks: each`Future.result()` call re-raised
the same object, appending another copy of the traceback to it.

### DocumentationÂ¶

- gh-86726: Greatly expand the `tkinter` documentation to cover the
full public API of the package and its submodules.  The descriptions are
oriented towards Python rather than Tcl/Tk, with corrected return types
and`versionadded` /`versionchanged` information.
- gh-150319: Generic builtin and standard library types now document the meaning of their type parameters.
- gh-109503: Fix documentation for `shutil.move()` on usage of`os.rename()` since nonatomic move might be used even if the files are
on the same filesystem. Patch by Fang Li

### TestsÂ¶

- gh-151130: Add more tests for `PyWeakref_*` C API.
- gh-150966: Avoid prematurely terminating failing live sampling profiler test targets, which made stderr assertions flaky on ASAN buildbots.
- gh-148853: Fix tests failing on FreeBSD in test.supportâs in_systemd_nspawn_sync_suppressed() due to unreadable /run directory.

### BuildÂ¶

- gh-151163: Updated Android build to include SQLite version 3.53.2.

### WindowsÂ¶

### macOSÂ¶

- gh-151163: Updated macOS installer to include SQLite version 3.53.2.

### IDLEÂ¶

- bpo-6699: Warn the user if a file will be overwritten when saving.

### C APIÂ¶

- gh-141510: Add `frozendict` to the fast paths of`PyMapping_GetOptionalItem()` ,`PyMapping_Keys()` ,`PyMapping_Values()` , and`PyMapping_Items()` .
- gh-123619: `PyUnstable_Object_EnableDeferredRefcount()` now returns`0` if the object is not tracked by the garbage collector: if`gc.is_tracked()` is false. Patch by Victor Stinner.
- gh-149044: Improved error message when specifying non-type base classes in `Py_tp_bases` ,`Py_tp_base` , and*bases* argument to`PyType_FromMetaclass()` and other`PyType_From*` functions.
- gh-150907: Fix `dynamic_annotations.h` header file when built with C++
and Valgrind: add`extern "C++" scope` for the C++ template. Patch by
Victor Stinner.

## Python 3.15.0 beta 2Â¶

*Release date: 2026-06-02*

### SecurityÂ¶

- gh-149698: Update bundled libexpat to version 2.8.1 for the fix for **CVE 2026-45186** .
- gh-87451: The `ftplib` moduleâs undocumented`ftpcp` function no
longer trusts the IPv4 address value returned from the source server in
response to the`PASV` command by default, completing the fix for**CVE 2021-4189** .  As with`ftplib.FTP` , the former behavior can be
re-enabled by setting the`trust_server_pasv_ipv4_address` attribute on
the source`ftplib.FTP` instance to`True` .  Thanks to Qi Deng at
Aurascape AI for the report.
- gh-149474: Fix the binary writer in `profiling.sampling` not firing
the audit (**PEP 578** ) when creating the output file. The writer and the
reader now accept any path-like object. Patch by Maurycy
PawÅowski-WieroÅski.
- gh-149486: `tarfile.data_filter()` now validates link targets using
the same normalised value that is written to disk, strips trailing
separators from the member name when resolving a symlinkâs directory, and
rejects link members that would replace the destination directory itself.
This closes several path-traversal bypasses of the`data` extraction
filter.
- gh-149079: Fix a potential denial of service in `unicodedata.normalize()` . The canonical ordering step of Unicode
normalization used a quadratic-time insertion sort for reordering
combining characters, which could be exploited with crafted input
containing many combining characters in non-canonical order. Replaced with
a linear-time counting sort for long runs.
- gh-149018: Improved protection against XML hash-flooding attacks in `xml.parsers.expat` and`xml.etree.ElementTree` when Python is
compiled with libExpat 2.8.0 or later.

### Core and BuiltinsÂ¶

- gh-150374: Fix double release of the import lock on lazy import reification errors.
- gh-149156: Fix an intermittent crash after `os.fork()` when perf
trampoline profiling is enabled and the child returns through trampoline
frames inherited from the parent process.
- gh-149449: Fix a use-after-free crash when the `unicodedata` module
was removed from`sys.modules` and garbage-collected between calls
that decode`\N{...}` escapes or use the`namereplace` codec error
handler.
- gh-150107: `asyncio` :`sendfile()` and`sock_sendfile()` event
loop methods now call`file.seek(offset)` if*file* has a`seek()` method, even if*offset* is`0` (default value).
- gh-150146: Fix a crash on a complex type variable substitution. ```
from typing import TypeVar; memoryview[TypeVar("")][*typing.Mapping[...,
...]]
```
used to fail due to missing`NULL` check on`_unpack_args` C
function call.
- gh-148587: `sys.lazy_modules` is now a set instead of a dict as
initially spelled out in PEP 810.
- gh-150042: Fix refleak in queue.SimpleQueue.put if memory allocation fails.
- gh-149590: Fix crash when faulthandler is imported more than once.
- gh-149816: Fix a race condition in `_PyBytes_FromList` in free-threading
mode.
- gh-149816: Fix a race condition in `memoryview` with
free-threading.
- gh-149807: Fix `hash(frozendict)` : compute the hash of each```
(key,
value)
```
pair correctly. Patch by Victor Stinner.
- gh-149738: `sqlite3` : Disallow removing`row_factory` and`text_factory` attributes of a connection to prevent a crash on a query.
- gh-139808: Add branch protections for AArch64 (BTI/PAC) in assembly code used by `-X perf_jit` (Linux perf profiler integration).
- gh-149676: Fix `frozendict | frozendict` hash.
- gh-148829: `sentinel` objects now support a`repr=` argument and
their`__module__` attribute is writable.
- gh-149642: Allow imports inside `exec()` calls within functions under`PYTHON_LAZY_IMPORTS=all` .
- gh-144957: Fix lazy `from` imports of module attributes provided by
module-level`__getattr__` .
- gh-149459: Fix a crash in the JIT optimizer when a specialized `LOAD_SPECIAL` guard deoptimized after inserting the synthetic`NULL` stack entry.
- gh-148450: Fix `abc.register()` so it invalidates type version tags for
registered classes.

### LibraryÂ¶

- gh-150685: Update bundled pip to 26.1.2
- gh-150228: The new `site.StartupState` class lets callers
batch-process**PEP 829** startup configuration files across multiple site
directories before any startup code runs, with public`addsitedir()` ,`addusersitepackages()` ,`addsitepackages()` , and`process()` methods.  The signature of`site.addsitedir()` is unchanged from Python 3.14.  The`defer_processing_start_files` argument and the`process_startup_files()` function added earlier in the 3.15 cycle have
been removed; use`site.StartupState` instead.
- gh-150406: Fix a possible crash occurring during `socket` module
initialization when the system is out of memory on platforms without a
reentrant`gethostbyname` .
- gh-150372: `readline` : Fix a potential crash during tab completion
caused by an out-of-memory error during module initialization.
- gh-150157: Fix a crash in free-threaded builds that occurs when pickling by name objects without a `__module__` attribute while`sys.modules` is concurrently being modified.
- gh-150175: Fix race condition in `unittest.mock.ThreadingMock` where concurrent calls could lose increments to`call_count` and other
attributes due to a missing lock in`_increment_mock_call` .
- gh-84353: Preserve non-UTF-8 encoded filenames when appending to a `zipfile.ZipFile` .  Previously, non-ASCII names stored in a legacy
encoding (without the UTF-8 flag bit set) could be corrupted when the
central directory was rewritten: they were decoded as cp437 and then
re-stored as UTF-8.
- gh-149189: Revert the changes to `pprint` defaults. Patch by Hugo van
Kemenade.
- gh-149995: Update various docstrings in `typing` .
- gh-88726: The `email` package now uses standard MIME charset names
âgb2312â and âbig5â instead of non-standard names âeucgb2312_cnâ and
âbig5_twâ.
- gh-149571: Fix the C implementation of `xml.etree.ElementTree.Element.itertext()` : it no longer emits text
for comments and processing instructions.
- gh-149921: Fix reference leaks in error paths of the `_interpchannels` and`_interpqueues` extension modules.
- gh-142349: Add `lazy` to the list of support topic by`help()` .
- gh-149819: Fix regression in `site.addsitedir()` where`.pth` files
were no longer processed in Python subprocesses. This happened because`site.main()` seeded`known_paths` with entries inherited from the
parent process, causing`addsitedir` to skip`.pth` processing.
- gh-149816: Fix a race condition in `_random.Random.__init__` method in
free-threading mode.
- gh-149801: Add IANA registered names and aliases with leading zeros before number (like IBM00858, CP00858, IBM01140, CP01140) for corresponding codecs.
- gh-149718: Coalesce consecutive identical stack frames in Tachyon, so aggregating collectors (pstats, collapsed, flamegraph, gecko) receive one collect. Improves sample rate 3x, error rate and missed rate drop by 70%. Patch by Maurycy PawÅowski-WieroÅski.
- gh-149701: Fix bad return code from Lib/venv/bin/activate if hashing is disabled
- gh-149504: Fix `site.addsitedir()` to allow re-entrant calls from
within startup files.  Previously, a`.pth` file containing an`import` line that called`site.addsitedir()` (or a`.start` entry
point doing the same) could crash with```
RuntimeError: dictionary changed
size during iteration
```
during site initialization, breaking tools such as`uv run --with` .
- gh-149584: Fix excessive overhead in the Tachyon profiler when inspecting a remote process by avoiding repeated remote page-cache scans, batching predicted remote reads, and reusing cached profiler result objects. Patch by Pablo Galindo and Maurycy PawÅowski-WieroÅski.
- gh-139489: Add `xml.is_valid_text()` to`xml.__all__` .
- gh-149614: Fix a regression that broke the ability to deepcopy `argparse.ArgumentParser` instances.
- gh-112821: In the REPL, autocompletion might run arbitrary code in the getter of a descriptor. If that getter raised an exception, autocompletion would fail to present any options for the entire object. Autocompletion now works as expected for these objects.
- gh-149534: Fix merging of `collections.defaultdict` and`frozendict` .
- gh-149388: Make `asyncio.windows_utils.PipeHandle` closing
idempotent.
- gh-149489: Fix `ElementTree` serialization to HTML. The
content of comments, processing instructions and elements âxmpâ, âiframeâ,
ânoembedâ, ânoframesâ, and âplaintextâ is no longer escaped. The
âplaintextâ element no longer have the closing tag. Add support of empty
attributes (with value`None` ).
- gh-149056: Fix `json.load()` not forwarding the*array_hook* argument
to`json.loads()` . Patch by Thomas Kowalski.
- gh-149046: `io` : Fix`io.StringIO` serialization: no longer
call`str(obj)` on`str` subclasses. Patch by Thomas Kowalski.
- gh-148441: `xml.parsers.expat` : prevent a crash in`CharacterDataHandler()` when the
character data size exceeds the parserâs`buffer size` .
- gh-146452: Fix segfault in `pickle` when pickling a dictionary
concurrently mutated by another thread in the free-threaded build.
- gh-86533: The `os.makedirs()` function and`pathlib.Path.mkdir()` method now have a*parent_mode* parameter to specify the mode for
intermediate directories when creating parent directories. This allows one
to match the behavior from Python 3.6 and earlier for`os.makedirs()` .
- gh-134261: zip: On reproducible builds, ZipFile uses UTC instead of the local time when writing file datetimes to avoid underflows.
- gh-133998: Fix `struct.error` exception when creating a file with`gzip.GzipFile` or compressing data with`gzip.compress()` if
the system time is outside the range 00:00:00 UTC, January 1, 1970 through
06:28:15 UTC, February 7, 2106, or explicitly passed*mtime* argument is
outside the range`0` to`2**32-1` .
- gh-128110: Fix bug in the parsing of `email` address headers that
could result in extraneous spaces in the decoded text when using a modern
email policy. Space between pairs of adjacent**RFC 2047** encoded-words is
now ignored, per section 6.2 (and consistent with existing parsing of
unstructured headers like*Subject* ).
- gh-107398: Fix `tarfile` stream mode exception when process the file
with the gzip extra field.
- gh-121109: Fix `tarfile` performance issue when reading archives in
streaming mode (e.g.`r|*` ).
- bpo-45509: Gzip headers are now checked for corrupted NAME, COMMENT and HCRC fields.

### TestsÂ¶

- gh-150387: Fix hang in `test.test_profiling.test_sampling_profiler.test_live_collector_ui.TestLiveModeErrors.test_run_failed_script_live` on slow buildbots. The test now always queues a final`q` keystroke so
the live TUI loop exits even when the profiler collects enough samples to
enter the post-finished input loop.
- gh-149776: Fix test_socket on Linux kernel 7.1 and newer: skip UDP Lite tests if itâs not supported. Patch by Victor Stinner.

### BuildÂ¶

### WindowsÂ¶

- gh-149786: Fixes virtual environment launchers on Windows free-threaded builds.
- gh-124111: Updated Windows builds to use Tcl/Tk 9.0.3.
- gh-138489: Windows distributions now include a `build-details.json` file (see**PEP 739** ). The legacy installer does not install it, but all
other distributions from python.org and all preset configurations in the`PC\layout` script will include one.
- gh-149029: Update Windows installer to ship with SQLite 3.53.1.

### macOSÂ¶

### Tools/DemosÂ¶

- gh-150258: Update the tooltip on the Tachyon flame graph to show both absolute and relative percentages.

### C APIÂ¶

- gh-149725: Add `PySentinel_CheckExact()` for exact`sentinel` type tests to accompany the existing`PySentinel_Check()` .
- gh-145235: Made `PyDict_AddWatcher()` ,`PyDict_ClearWatcher()` ,`PyDict_Watch()` , and`PyDict_Unwatch()` thread-safe on the free threaded build.

## Python 3.15.0 beta 1Â¶

*Release date: 2026-05-07*

### SecurityÂ¶

- gh-149254: Update Android and iOS installer to use OpenSSL 3.5.6.
- gh-149017: Update bundled libexpat to version 2.8.0.
- gh-148252: Fixed string table and sample record bounds checks in `_remote_debugging` when decoding certain`.pyb` inputs on 32-bit
builds. Patch by Maurycy PawÅowski-WieroÅski.
- gh-90309: Base64-encode values when embedding cookies to JavaScript using the `http.cookies.BaseCookie.js_output()` method to avoid injection
and escaping.
- gh-148808: Added buffer boundary check when using `nbytes` parameter
with`asyncio.AbstractEventLoop.sock_recvfrom_into()` . Only relevant
for Windows and the`asyncio.ProactorEventLoop` .
- gh-148395: Fix a dangling input pointer in `lzma.LZMADecompressor` ,`bz2.BZ2Decompressor` , and internal`zlib._ZlibDecompressor` when memory allocation fails with`MemoryError` , which could let a subsequent`decompress()` call
read or write through a stale pointer to the already-released caller
buffer.
- gh-148252: Fixed stack depth calculation in `_remote_debugging` when
decoding certain`.pyb` inputs on 32-bit builds. Issue originally
identified and diagnosed by Tristan Madani (@TristanInSec on GitHub).
- gh-148178: Hardened `_remote_debugging` by validating remote debug
offset tables before using them to size memory reads or interpret remote
layouts.
- gh-148169: A bypass in `webbrowser` allowed URLs prefixed with`%action` to pass the dash-prefix safety check.
- gh-146581: Fix vulnerability in `shutil.unpack_archive()` for ZIP
files on Windows which allowed to write files outside of the destination
tree if the patch in the archive contains a Windows drive prefix. Now such
invalid paths will be skipped. Files containing â..â in the name (like
âfoo..barâ) are no longer skipped.
- gh-137586: Fix a PATH-injection vulnerability in `webbrowser` on
macOS where`osascript` was invoked without an absolute path. The new`MacOS` class uses`/usr/bin/open` directly, eliminating the
dependency on`osascript` entirely.
- gh-146333: Fix quadratic backtracking in `configparser.RawConfigParser` option parsing regexes (`OPTCRE` and`OPTCRE_NV` ). A crafted configuration line with many whitespace
characters could cause excessive CPU usage.
- gh-146211: Reject CR/LF characters in tunnel request headers for the HTTPConnection.set_tunnel() method.

### Core and BuiltinsÂ¶

- gh-148940: Revert the process size based deferral of garbage collection (gh-133464). The performance issue this change resolves is also fixed by gh-142562. This approach has the problem that process size as seen by the OS (e.g. the resident size or RSS) does not immediately decrease after cyclic garbage is freed since mimalloc defers returning memory of the OS. This change applies to the free-threaded GC only.
- gh-149243: Check for recursion limits in `CALL_ALLOC_AND_ENTER_INIT` opcode.
- gh-126910: Add support for unwinding JIT frames using GNU backtrace. Patch by Diego Russo and Pablo Galindo
- gh-149171: Allow assignment to the `__module__` attribute of`typing.TypeAliasType` instances.
- gh-149122: Fix a crash in optimized calls to `all()` ,`any()` ,`tuple()` ,`list()` , and`set()` with an async generator
expression argument (for example,`tuple(await x for x in y)` ). These
calls now correctly raise`TypeError` instead of crashing.
- gh-149049: Fix stack underflow for `BINARY_OP` in tier 2.
- gh-83065: Fix a deadlock that could occur when one thread is importing a submodule (for example `import pkg.sub.mod` ) while another thread is
importing one of its parent packages (for example`import pkg.sub` ) and
that parentâs`__init__.py` itself imports the submodule. The import
system now acquires module locks in hierarchical (parent-before-child)
order so the two threads serialise instead of raising`_DeadlockError` .
- gh-113956: Fix a data race in `sys.intern()` in the free-threaded
build when interning a string owned by another thread. An interned copy
owned by the current thread is used instead when it is not safe to
immortalize the original.
- gh-148850: Fix the memory sanitizer false positive in `os.getrandom()` .
- gh-148820: Fix a race in `_PyRawMutex` on the free-threaded build
where a`Py_PARK_INTR` return from`_PySemaphore_Wait` could let the
waiter destroy its semaphore before the unlocking threadâs`_PySemaphore_Wakeup` completed, causing a fatal`ReleaseSemaphore` error.
- gh-148829: Add `sentinel` , implementing**PEP 661** . PEP by Tal
Einat; patch by Jelle Zijlstra.
- gh-146270: Fix a sequential consistency bug in `structmember.c` .
- gh-148766: The interpreter help (such as `python --help` ) is now in
color. Patch by Hugo van Kemenade.
- gh-148571: Fix a crash in the JIT optimizer when specialized opcode families inherited incompatible recorded operand layouts.
- gh-148653: Forbid `marshalling` recursive code objects,`slice` and`frozendict` objects which cannot be correctly
unmarshalled.
- gh-142516: Forward-port the generational cycle garbage collector to the default 3.15 build, replacing the incremental collector while leaving the free-threaded collector unchanged.
- gh-146462: Added `PyTypeObject.tp_basicsize` ,`PyTypeObject.tp_dictoffset` , and`PyHeapTypeObject.ht_cached_keys` offsets to`_Py_DebugOffsets` to support version-independent
read-only dict introspection tools.
- gh-145239: Unary plus is now accepted in `match` literal
patterns, mirroring the existing support for unary minus. Patch by Bartosz
SÅawecki.
- gh-148515: Fix a bug in the JIT optimizer reading operands for uops with multiple caches.
- gh-148390: Fix an undefined behavior in `memoryview` when using the
native boolean format (`?` ) in`cast()` . Previously, on
some common platforms, calling`memoryview(b).cast("?").tolist()` incorrectly returned`[False]` instead of`[True]` for any even byte*b* . Patch by BÃ©nÃ©dikt Tran.
- gh-148418: Fix a possible reference leak in a corrupted `TYPE_CODE` marshal stream.
- gh-148393: Fix data races between `PyDict_Watch()` /`PyDict_Unwatch()` and concurrent dict mutation in the
free-threaded build.
- gh-148398: Fix a bug in the JIT optimizer where class attribute loads were not invalidated after type mutation.
- gh-146527: Add a `GCMonitor` class with a`get_gc_stats` method to the`_remote_debugging` module to allow reading GC statistics from an
external Python process without requiring the full`RemoteUnwinder` functionality. Patch by Sergey Miryanov and Pablo Galindo.
- gh-148284: Fix high stack consumption in Pythonâs interpreter loop on Clang 22 by setting function limits for inlining when building with computed gotos.
- gh-148037: Remove critical section from `PyCode_Addr2Line()` in
free-threading.
- gh-115802: Improve JIT code generation on Linux AArch64 by reducing the indirect call to external symbols. Patch by Diego Russo.
- gh-148189: Repaired undercount of bytes in type-specific free lists reported by sys._debugmallocstats(). For types that participate in cyclic garbage collection, it was missing two pointers used by GC.
- gh-148222: Fix vectorcall support in `types.GenericAlias` when the
underlying type does not support the vectorcall protocol. Fix possible
leaks in`types.GenericAlias` and`types.UnionType` in case
of memory error.
- gh-148208: Fix recursion depth leak in `PyObject_Print()`
- gh-95004: The specializing interpreter now specializes for `enum.Enum` improving performance and scaling in free-threading.
Patch by Kumar Aditya.
- gh-149202: Enable frame pointers by default for GCC-compatible CPython builds, including `-mno-omit-leaf-frame-pointer` ,`-marm` on 32-bit
ARM, and/or`-mbackchain` on s390x platforms when the compiler supports
them, so profilers and debuggers can unwind native interpreter frames more
reliably. Users can pass`--without-frame-pointers` to`./configure` to opt out.
- gh-148014: Accept a function name in `-X presite` command
line option and`PYTHON_PRESITE` environment variable. Patch by
Victor Stinner.
- gh-147998: Fixed a memory leak in interpreter helper calls so cleanup works when an operation falls across interpreter boundaries. Patch by Maurycy PawÅowski-WieroÅski.
- gh-146455: Fix O(NÂ²) compile-time regression in constant folding after it was moved from AST to CFG optimizer.
- gh-146306: Specialize float true division in the tier 2 optimizer with inplace mutation for uniquely-referenced operands.
- gh-142186: Global `sys.monitoring` events can now be turned on and
disabled on a per code object basis. Returning`DISABLE` from a callback
disables the event for the entire code object (for the current tool).
- gh-126910: Add support for unwinding JIT frames using GDB. Patch by Diego Russo and Pablo Galindo.
- gh-146031: The unstable API _PyInterpreterState_SetEvalFrameFunc has a companion function _PyInterpreterState_SetEvalFrameAllowSpecialization to specify if specialization should be allowed. When this option is set to 1 the specializer will turn Python -> Python calls into specialized opcodes which the replacement interpreter loop can choose to respect and perform inlined dispatch.
- gh-145278: The `encodings` is now partially frozen, including the`aliases` and`utf_8` submodules.The `linecache` is now frozen.
- gh-134584: Optimize and eliminate redundant ref-counting for `MAKE_FUNCTION` in the JIT.
- gh-143886: Reorder function annotations so positional-only arguments are returned before other arguments. This fixes how `functools.singledispatch()` registers functions with positional-only
arguments.
- gh-98894: Restore `function__entry` and`function__return` DTrace/SystemTap probes that were broken since Python 3.11.
- gh-116021: Support for creating instances of abstract AST nodes from the `ast` module is deprecated and scheduled for removal in Python 3.20.
Patch by Brian Schubert.
- gh-137814: Fix the `__qualname__` attribute of`__annotate__` functions on functions.
- gh-137600: `ast` : The constructors of AST nodes now raise a`TypeError` when a required argument is omitted or when a keyword
argument that does not map to a field on the AST node is passed. These
cases had previously raised a`DeprecationWarning` since Python 3.13.
Patch by Brian Schubert.
- gh-137293: Fix `SystemError` when searching ELF Files in`sys.remote_exec()` .
- gh-135357: Add support for `socket.SO_PASSRIGHTS` on Linux.
- gh-134690: Removed deprecated in **PEP 626** since Python 3.12`codeobject.co_lnotab` from`types.CodeType` .
- gh-100239: Specialize `BINARY_OP` for concatenation of lists and tuples,
and propagate the result type through`_BINARY_OP_EXTEND` in the tier 2
optimizer so that follow-up type guards can be eliminated.

### LibraryÂ¶

- gh-148823: Defer the import of `_colorize` in`argparse` until needed
for coloring output.
- gh-141560: Add an *annotation_format* parameter to`inspect.getfullargspec()` .
- gh-139489: Add the `xml.is_valid_text()` function, which allows to
check whether a string can be used in the XML document.
- gh-142389: Add backticks to stdlib argparse help to display in colour. Patch by Hugo van Kemenade.
- gh-149377: Update bundled pip to 26.1.1
- gh-142389: Add backtick markup support in `argparse` option help text
to highlight inline code when color output is enabled. Patch by Hugo van
Kemenade.
- gh-148675: Remove `F` and`D` formats from`array` and`memoryview` . Patch by Victor Stinner.
- gh-149342: Fix `_remote_debugging` binary writing so that sampling a
thread whose Python frame stack is empty (for example while it is in a C
call or mid-syscall) no longer raises```
RuntimeError("Invalid stack
encoding type")
```
, and so that`BinaryWriter.total_samples` after`finalize()` or context-manager exit includes samples flushed from
the RLE buffer. Patch by Maurycy PawÅowski-WieroÅski.
- gh-149010: The `inspect` module CLI now reports as much information as
it has available for non-source modules when`--details` is specified,
and provides an error message rather than a traceback when`--details` is omitted. It also reports improved information when the given target
location is not the targetâs defining location and when the given target
is a data value rather than a class or function definition.
- gh-146609: Use `argparse` for colour help`timeit` CLI. Patch by
Hugo van Kemenade.
- gh-142389: Add backticks for colour to regrtest and pdbâs help description. Patch by Hugo van Kemenade.
- gh-144384: Lazily import `_colorize` . Patch by Hugo van Kemenade.
- gh-149321: Fix import cycles exposed by running standard library modules with `-X lazy_imports=none` .
- gh-145378: Generate consistent colors for `pdb` commands in`pdb` REPL.
- gh-149296: Add a `dump` subcommand to`profiling.sampling` that
prints a single traceback-style snapshot of a running processâs Python
stack, including per-thread status, source line highlighting, optional
bytecode opcode names, and async-aware task reconstruction. Patch by Pablo
Galindo.
- gh-143231: A *module* attribute has been added to`warnings.WarningMessage` .
- gh-148675: `ctypes` : Change the`_type_` of`c_float_complex` ,`c_double_complex` and`c_longdouble_complex` from`F` ,`D` and`G` to`Zf` ,`Zd` and`Zg` for compatibility with numpy. Patch by Victor
Stinner.
- gh-148675: The `array.typecodes` type changed from`str` to`tuple` to support type codes longer than 1 character (`Zf` and`Zd` ). Patch by Victor Stinner.
- gh-149221: Catch rare math domain error for `random.binomialvariate()` .
- gh-149231: In `tomllib` , the number of parts in TOML keys is now
limited.
- gh-143231: `unittest.TestCase.assertWarns()` and`unittest.TestCase.assertWarnsRegex()` no longer swallow warnings that
do not match the specified category or regex. Nested context managers are
now supported.
- gh-149214: Fix `_remote_debugging` misreading non-ASCII Unicode
strings (Latin-1, BMP and non-BMP) from a remote process. Filenames and
function names that contain non-ASCII characters are now reported
correctly in stack traces, the sampling profiler, and`asyncio` task
introspection.
- gh-149189: `pprint` now uses modern defaults:`indent=4` and`width=88` , and the default`compact=False` output is now formatted
similar to pretty-printed`json.dumps()` , with opening parentheses and
brackets followed by a newline and the contents indented by one level. The*expand* parameter, added in 3.15.0a8, has been removed;`compact=False` (the default) now produces the former`expand=True` layout. Patch by
Hugo van Kemenade.
- gh-149173: Fix inverted `PYTHON_BASIC_REPL` environment check in`pdb._pyrepl_available` .
- gh-149117: Fix `runpy.run_module()` and`runpy.run_path()` to set
the`name` attribute on the`ImportError` they
raise.
- gh-149148: `ensurepip` : Upgrade bundled pip to 26.1. This version
fixes the**CVE 2026-3219** vulnerability. Patch by Victor Stinner.
- gh-149009: Validate that `profiling.sampling` binary profiles do not
contain more unique (thread, interpreter) pairs than declared in the
header. Patch by Maurycy PawÅowski-WieroÅski.
- gh-148292: `ssl` : Update`ssl.SSLSocket` and`ssl.SSLObject` for OpenSSL 4. The classes now remember if they get
a`ssl.SSLEOFError` . In this case, following`read()` ,`sendfile()` ,`write()` , and`do_handshake()` calls raise`ssl.SSLEOFError` without calling the underlying OpenSSL
function. Thanks to that,`ssl.SSLSocket` behaves the same on all
OpenSSL versions on EOF.  Patch by Victor Stinner.
- gh-149085: Add a *max_threads* keyword argument to`faulthandler.dump_traceback()` ,`faulthandler.dump_traceback_later()` ,`faulthandler.enable()` ,
and`faulthandler.register()` .
- gh-148641: `pkgutil.resolve_name()` gets a new optional, keyword-only
argument called`strict` .  The default is`False` for backward
compatibility.
- gh-148093: Fix an out-of-bounds read of one byte in `binascii.a2b_uu()` . Raise`binascii.Error` , instead of reading
past the buffer end.
- gh-149083: `dataclasses.MISSING` and`dataclasses.KW_ONLY` are
now instances of`sentinel` .
- gh-148914: Fix memoization of in-band `PickleBuffer` in the
Python implementation of`pickle` . Previously, identical`PickleBuffer` s did not preserve identity, and empty writable`PickleBuffer` memoized an empty bytearray object in place of`b''` , so the following references to`b''` were unpickled as an empty
bytearray object.
- gh-149026: Add colour to `pickletools` CLI output. Patch by Hugo van
Kemenade.
- gh-148991: Add colour to `tokenize` CLI output. Patch by Hugo van
Kemenade.
- gh-138907: Support **RFC 9309** in`urllib.robotparser` .
- gh-148981: Add *color* parameter to`ast.dump()` .
- gh-148849: Deprecate `http.cookies.Morsel.js_output()` and`http.cookies.BaseCookie.js_output()` , which will be removed in Python
3.19. Use`http.cookies.Morsel.output()` or`http.cookies.BaseCookie.output()` instead.
- gh-146311: Add a *canonical* keyword-only parameter to the base32, base64,
base85, ascii85, and Z85 decoders in`base64` and`binascii` .
When true, encodings with non-zero padding bits (base32/64) or
non-canonical encodings (base85/ascii85) are rejected.  Single-character
final groups in`binascii.a2b_ascii85()` and`binascii.a2b_base85()` are now always rejected as encoding
violations, regardless of*canonical* ; previously they were silently
ignored and produced no output bytes.
- gh-148947: Fix crash in `@dataclasses.dataclass` with`slots=True` that occurred when a function found within the class had an empty`__class__` cell.
- gh-148680: `ForwardRef` objects that contain internal names to represent
known objects now show the`type_repr` of the known object rather than
the internal`__annotationlib_name_x__` name when evaluated as strings.
- gh-124397: The threading module added tooling to support concurrent iterator access: `threading.serialize_iterator` ,`threading.synchronized_iterator()` , and`threading.concurrent_tee()` .
- gh-148801: `xml.etree.ElementTree` : Fix a crash in`Element.__deepcopy__` on deeply nested trees.
- gh-148735: `xml.etree.ElementTree` : Fix a use-after-free in`Element.findtext` when the
element tree is mutated concurrently during the search.
- gh-148740: Fix usage for `uuid` command-line interface to support a
custom namespace be provided for uuid3 and uuid5.
- gh-148688: `bz2` ,`compression.zstd` ,`lzma` ,`zlib` :
Fix a double free on memory allocation failure. Patch by Victor Stinner.
- gh-148675: `array` ,`struct` : Add support for`Zd` and`Zf` formats for double complex and float complex. Patch by Victor Stinner.
- gh-148651: Fix reference leak in `compression.zstd.ZstdDecompressor` when an invalid option key is
passed.
- gh-148641: **PEP 829** (package startup configuration files) implements a
new format`<name>.start` parallel to`<name>.pth` files, to replace`import` lines in the latter.
- gh-148639: Implement **PEP 800** , adding the`@typing.disjoint_base` decorator. Patch by Jelle Zijlstra.
- gh-148615: Fix `pdb` to accept standard â end of options separator.
Reported by haampie. Patched by Shrey Naithani.
- gh-146553: Fix infinite loop in `typing.get_type_hints()` when`__wrapped__` forms a cycle. Patch by Shamil Abdulaev.
- gh-148599: Update the `socket` moduleâs WSA error messages to match
official documentation.
- gh-148508: An intermittent timing error when running SSL tests on iOS has been resolved.
- gh-144881: `asyncio` debugging tools (`python -m asyncio ps` and`pstree` ) now retry automatically on transient errors that can occur
when attaching to a process under active thread delegation. The number of
retries can be controlled with the`--retries` flag. Patch by Bartosz
SÅawecki.
- gh-148518: If an email containing an address header that ended in an open double quote was parsed with a non- `compat32` policy, accessing the`username` attribute of the mailbox accessed through that header object
would result in an`IndexError` . It now correctly returns an empty
string as the result.
- gh-148464: Add missing `__ctype_le/be__` attributes for`c_float_complex` and`c_double_complex` .
Patch by Sergey B Kirpichev.
- gh-148370: `configparser` : prevent quadratic behavior when a`ParsingError` is raised after a parser fails to parse
multiple lines. Patch by BÃ©nÃ©dikt Tran.
- gh-121190: `importlib.resources.files()` now emits a more meaningful
error message when module spec is None (as found in some`__main__` modules).
- gh-127012: `importlib.abc.Traversable.read_text` now allows/solicits an`errors` parameter.
- gh-137855: Improve import time of `dataclasses` module by lazy
importing`re` and`copy` modules.
- gh-148352: Add more color to `calendar` âs CLI output. Patch by Hugo
van Kemenade.
- gh-148254: Use singular âsecâ instead of âsecsâ in `timeit` verbose
output for consistency with other time units.
- gh-130472: Integrate fancycompleter with import completions.
- gh-148241: `json` : Fix serialization: no longer call`str(obj)` on`str` subclasses. Patch by Victor Stinner.
- gh-148225: The `profiling.sampling``replay` command now rejects
non-binary profile files with a clear error explaining that replay only
accepts files created with`--binary` .
- gh-148192: `email.generator.Generator._make_boundary` could fail to
detect a duplicate boundary string if linesep was not n. It now correctly
detects boundary strings when linesep is rn as well.
- gh-148207: `typing.TypeVarTuple` now accepts`bound` ,`covariant` ,`contravariant` , and`infer_variance` parameters,
matching the interface of`typing.TypeVar` and`typing.ParamSpec` .
- gh-148100: Soft deprecate `re.match()` and`re.Pattern.match()` in favour of`re.prefixmatch()` and`re.Pattern.prefixmatch()` . Patch by Hugo van Kemenade.
- gh-147991: Improve `tomllib` import time (up to 10x faster). Patch by
Victor Stinner.
- gh-147957: Guarantees that `collections.UserDict.popitem()` will pop
in the same order as the wrapped dictionary rather than an arbitrary
order.
- gh-146256: The `profiling.sampling` module now supports JSONL output
format via`--jsonl` . Each run emits a newline-delimited JSON file that
is sequentially parseable by external tools, scripts, and programmatic
consumers. Patch by Maurycy PawÅowski-WieroÅski.
- gh-146609: Add colour to `timeit` CLI output. Patch by Hugo van
Kemenade.
- gh-146563: `xml.parsers.expat` : add an exception note when a custom
Expat handler return value cannot be properly interpreted. Patch by
BÃ©nÃ©dikt Tran.
- gh-137586: Add `MacOS` to`webbrowser` for macOS, which opens
URLs via`/usr/bin/open` instead of piping AppleScript to`osascript` .
Deprecate`MacOSXOSAScript` in favour of`MacOS` .
- gh-146406: Cross-language method suggestions are now shown for `AttributeError` on builtin types and their subclasses. For example,`[].push()` suggests`append` ,`(1,2).append(3)` suggests using a`list` ,`None.keys()` suggests expecting a`dict` , and`1.0.__or__` suggests using an`int` .
- gh-146313: Fix a deadlock in `multiprocessing` âs resource tracker
where the parent process could hang indefinitely in`os.waitpid()` during interpreter shutdown if a child created via`os.fork()` still
held the resource trackerâs pipe open.
- gh-146292: Add colour to `BaseHTTPRequestHandler` logs,
as used by the`http.server` CLI. Patch by Hugo van Kemenade.
- gh-145917: Add MIME types for TTC and Haptics formats to `mimetypes` .
(Contributed by Charlie Lin in gh-145918.)
- gh-145846: Fix memory leak in `_lsprof` when`clear()` is called
during active profiling with nested calls.`clearEntries()` now walks
the entire`currentProfilerContext` linked list instead of only freeing
the top context.
- gh-145831: Fix `email.quoprimime.decode()` leaving a stray`\r` when`eol='\r\n'` by stripping the full*eol* string instead of one
character.
- gh-145378: Use `PyREPL` as the default input console for`pdb`
- gh-145244: Fixed a use-after-free in `json` encoder when a`default` callback mutates the dictionary being serialized.
- gh-117716: Fix `wave` writing of odd-sized`data` chunks by
appending the required RIFF pad byte and correcting the RIFF chunk size
field accordingly.
- gh-145200: `hashlib` : fix a memory leak when allocating or
initializing an OpenSSL HMAC context fails.
- gh-145056: Add support for `frozendict` in`dataclasses.asdict()` and`dataclasses.astuple()` .
- gh-145105: Fix crash in `csv` reader when iterating with a re-entrant
iterator that calls`next()` on the same reader from within`__next__` .
- gh-130750: Restore quoting of choices in `argparse` error messages
for improved clarity and consistency with documentation.
- gh-137855: Reduce the import time of `dataclasses` module by ~20%.
- gh-70647: `strptime()` now raises`ValueError` when the format string contains`%d` without a year directive. Using`%e` without a year now emits a`DeprecationWarning` .
- gh-105936: Attempting to mutate non-field attributes of `dataclasses` with both*frozen* and*slots* being`True` now raises`FrozenInstanceError` instead of`TypeError` .
Their non-dataclass subclasses can now freely mutate non-field attributes,
and the original non-slotted class can be garbage collected.
- gh-142831: Fix a crash in the `json` module where a use-after-free
could occur if the object being encoded is modified during serialization.
- gh-108411: `typing.IO` and`typing.BinaryIO` method arguments are now
positional-only.
- gh-130273: Fix traceback color output with Unicode characters.
- gh-142307: `imaplib` : deprecate support for`IMAP4.file` . This attribute was never meant to be part of the
public interface and altering its value may result in unclosed files or
other synchronization issues with the underlying socket. Patch by BÃ©nÃ©dikt
Tran.
- gh-141449: Improve tests and documentation for non-function callables as annotate functions.
- gh-140287: The `asyncio` REPL now handles exceptions when executing`PYTHONSTARTUP` scripts. Patch by Bartosz SÅawecki.
- gh-139489: Add the `xml.is_valid_name()` function, which allows to
check whether a string can be used as an element or attribute name in XML.
- gh-75707: Add optional `mtime` argument to`tarfile.open()` , for
setting the`mtime` header field in`.tar.gz` archives.
- gh-125862: The `contextlib.contextmanager()` and`contextlib.asynccontextmanager()` decorators now work correctly with
generators, coroutine functions, and async generators when the wrapped
callables are used as decorators.
- gh-135528: `http.cookiejar` : add âtvâ, âorâ, ânomâ, âschâ, and âwebâ
to the default list of supported country code second-level domains.
- gh-135056: Add a `-H` or`--header` CLI option to**python -m
http.server** . Contributed by Anton I. Sipos.
- gh-134551: Add t-strings support to pprint functions
- gh-133956: Fix bug where `@dataclass` wouldnât detect`ClassVar` fields if`ClassVar` was re-exported from a
module other than`typing` .
- gh-132631: Fix âI/O operation on closed fileâ when parsing JSON Lines file with `JSON CLI` .
- gh-108951: `asyncio` : Add`TaskGroup.cancel` which cancels unfinished tasks and exits the
group without raising`asyncio.CancelledError` .
- gh-123853: Update the table of Windows language code identifiers (LCIDs) used by `locale.getdefaultlocale()` on Windows to protocol version
16.0 (2024-04-23).
- gh-122476: The `email` module no longer incorrectly uses**RFC 2047** encoding for a mailbox with non-ASCII characters in its local-part. Under
a policy with`utf8` set`False` ,
attempting to serialize such a message will now raise an`HeaderWriteError` . There is no valid 7-bit encoding
for an internationalized local-part. Use`email.policy.SMTPUTF8` (or
another policy with`utf8=True` ) to correctly pass through the
local-part as Unicode characters.
- gh-83938: The `email` module no longer incorrectly uses**RFC 2047** encoding for a mailbox with non-ASCII characters in its domain. Under a
policy with`utf8` set`False` ,
attempting to serialize such a message will now raise an`HeaderWriteError` . Either apply an appropriate IDNA
encoding to convert the domain to ASCII before serialization, or use`email.policy.SMTPUTF8` (or another policy with`utf8=True` ) to
correctly pass through the internationalized domain name as Unicode
characters.
- gh-81074: The `email` module no longer treats email addresses with
non-ASCII characters as defects when parsing a Unicode string or in the`addr_spec` parameter to`email.headerregistry.Address` .**RFC 5322** permits such addresses, and they were already supported when
parsing bytes and in the Address`username` parameter.The (undocumented) `email.errors.NonASCIILocalPartDefect` is no
longer used and should be considered deprecated.
- gh-70039: Fixed bug where `smtplib.SMTP.starttls()` could fail if`smtplib.SMTP.connect()` is called explicitly rather than implicitly.
- gh-113471: Allow `http.server` to set a default content-type when
serving files with an unknown or missing extension.
- gh-83281: `email` : improve handling trailing garbage in address lists
to avoid throwing AttributeError in certain edge cases
- gh-96894: Do not turn echo off for subsequent commands in batch activators ( `activate.bat` and`deactivate.bat` ) of`venv` .

### DocumentationÂ¶

- gh-148663: Document that `calendar.IllegalMonthError` is a subclass
of both`ValueError` and`IndexError` since Python 3.12.
- gh-146646: Document that `glob.glob()` ,`glob.iglob()` ,`pathlib.Path.glob()` , and`pathlib.Path.rglob()` silently
suppress`OSError` exceptions raised from scanning the filesystem.

### TestsÂ¶

### BuildÂ¶

- gh-149353: Avoid unnecessary JIT-related rebuilds during `make install` after`--enable-optimizations` builds.
- gh-149351: Avoid possible broken macOS framework install names when DESTDIR is specified during builds.
- gh-149252: Update to WASI SDK 33.
- gh-148690: Windows free-threaded builds now output to a different default path with default filenames, for example, `PCbuild/amd64t/python.exe` rather than`PCbuild/amd64/python3.15t.exe` . The`PC/layout` script
has been updated to ensure compatibility of generated layouts.
- gh-146475: Block Apple Clang from being used to build the JIT as it ships without required LLVM tools.
- gh-148644: Errors during the PGO training job on Windows are no longer ignored, and a non-zero return code will cause the build to fail.
- gh-148535: No longer use the `gcc -fprofile-update=atomic` flag on i686.
The flag has been added to fix a random GCC internal error on PGO build
(gh-145801) caused by corruption of profile data (.gcda files). The
problem is that it makes the PGO build way slower (up to 47x slower) on
i686. Since the GCC internal error was not seen on i686 so far, donât use`-fprofile-update=atomic` on i686 anymore. Patch by Victor Stinner.
- gh-148483: Use `Py_GCC_ATTRIBUTE(unused)` for stop_tracing label.
- gh-148474: Fixed compilation of `Python/pystrhex.c` with older clang
versions.
- gh-146445: The Android build tools have been moved to the Platforms folder.
- gh-146264: Fix static module builds on non-WASI targets by linking HACL dependencies as static libraries when `MODULE_BUILDTYPE=static` ,
preventing duplicate`_Py_LibHacl_*` symbol errors at link time.
- gh-138451: Allow for custom LLVM path using `LLVM_TOOLS_INSTALL_DIR` during JIT build.
- gh-133312: Add a new `./configure` option`--enable-static-libpython-for-interpreter` which, when used with`--enable-shared` , continues to build the shared library but does
not use it for the interpreter. Instead, libpython is statically linked
into the interpreter, as if`--enable-shared` had not been used.
This allows you to do a single build and get a Python interpreter binary
that does not use a shared library but also get a shared library for use
by other programs.

### WindowsÂ¶

- gh-149254: Updated bundled version of OpenSSL to 3.5.6.
- gh-148690: Non-freethreaded builds on Windows now support extensions linked to `python3t.dll` , and will include a copy of that library in
normal installs that references the non-freethreaded runtime.
- gh-146458: Fix incorrect REPL height and width tracking on console window resize on Windows.

### macOSÂ¶

### IDLEÂ¶

- gh-94523: Detect file if modified at local disk and prompt to ask refresh. Patch by Shixian Li.
- gh-139551: Support rendering `BaseExceptionGroup` in IDLE.
- gh-89520: Make IDLE extension configuration look at user config files, allowing user-installed extensions to have settings and key bindings defined in ~/.idlerc.

### C APIÂ¶

- gh-149225: `PyCriticalSection` and related functions are added to
the Stable ABI.
- gh-149216: `PyType_WatchCallback` callbacks registered via`PyType_AddWatcher()` are now also invoked when a watched heap type
is deallocated. Previously, type watchers were only notified of
modifications, which could cause stale references when a type was freed
and its address was reused.
- gh-149101: Implement **PEP 788** .
- gh-149044: Implement **PEP 820** : Unified slot system for the C API.
- gh-148267: Using `Py_LIMITED_API` on a non-Windows free-threaded
build no longer needs an extra`Py_GIL_DISABLED` .
- gh-145559: Rename `_Py_DumpTraceback` and`_Py_DumpTracebackThreads` to`PyUnstable_DumpTraceback()` and`PyUnstable_DumpTracebackThreads()` .
- gh-146636: Implement **PEP 803** â`abi3t` : Stable ABI for Free-Threaded
Builds.
- gh-146302: `Py_IsInitialized()` no longer returns true until
initialization has fully completed, including import of the`site` module.  The underlying runtime flags now use atomic operations.
- gh-146063: Add `PyObject_CallFinalizerFromDealloc()` function to the
limited C API. Patch by Victor Stinner.
- gh-145921: Add functions that are guaranteed to be safe for use in `tp_traverse` handlers:`PyObject_GetTypeData_DuringGC()` ,`PyObject_GetItemData_DuringGC()` ,`PyType_GetModuleState_DuringGC()` ,`PyModule_GetState_DuringGC()` ,`PyModule_GetToken_DuringGC()` ,`PyType_GetBaseByToken_DuringGC()` ,`PyType_GetModule_DuringGC()` ,`PyType_GetModuleByToken_DuringGC()` .

## Python 3.15.0 alpha 8Â¶

*Release date: 2026-04-07*

### SecurityÂ¶

- gh-145986: `xml.parsers.expat` : Fixed a crash caused by unbounded C
recursion when converting deeply nested XML content models with`ElementDeclHandler()` . This addresses**CVE 2026-4224** .
- gh-145599: Reject control characters in `http.cookies.Morsel``update()` and`js_output()` . This addresses**CVE 2026-3644** .
- gh-143930: Reject leading dashes in URLs passed to `webbrowser.open()` .

### Core and BuiltinsÂ¶

- gh-148157: Fix an unlikely crash when parsing an invalid type comments for function parameters. Found by OSS Fuzz in #492782951.
- gh-100239: Propagate result type and uniqueness information through `_BINARY_OP_EXTEND` in the tier 2 optimizer, enabling elimination of
downstream type guards and selection of inplace float operations.
- gh-148144: Initialize `_PyInterpreterFrame.visited` when copying
interpreter frames so incremental GC does not read an uninitialized byte
from generator and frame-object copies.
- gh-148072: Cache `pickle.dumps` and`pickle.loads` per interpreter in
the XIData framework, avoiding repeated module lookups on every
cross-interpreter data transfer.  This speeds up`InterpreterPoolExecutor` for mutable types
(`list` ,`dict` ) by 1.7xâ3.3x.
- gh-148110: Fix `sys.set_lazy_imports_filter()` so relative lazy
imports pass the resolved imported module name to the filter callback.
Patch by Pablo Galindo.
- gh-148083: Constant-fold `_CONTAINS_OP_SET` for`frozenset` .
Patch by Donghee Na.
- gh-144319: Fix a bug that could cause applications with specific allocation patterns to leak memory via Huge Pages if compiled with Huge Page support. Patch by Pablo Galindo
- gh-147985: Make `PySet_Contains()` attempt a lock-free lookup,
similar to`set.__contains__()` .  This avoids acquiring the set
object mutex in the normal case.
- gh-147856: Allow the *count* argument of`bytes.replace()` to be a
keyword.
- gh-146615: Fix a crash in `__get__()` for
METH_METHOD descriptors when an invalid (non-type) object is
passed as the second argument. Patch by Steven Sun.
- gh-146306: Optimize compact integer arithmetic in the JIT by mutating uniquely-referenced operands in place, avoiding allocation of a new int object. Speeds up the pyperformance `spectral_norm` benchmark by ~10%.
- gh-146587: Fix type slot assignment incase of multiple slots for same name in type object implementation. Patch by Kumar Aditya.
- gh-126910: Set frame pointers in `aarch64-unknown-linux-gnu` JIT code,
allowing most native profilers and debuggers to unwind through them. Patch
by Diego Russo
- gh-146388: Adds a null check to handle when the JIT optimizer runs out of space when dealing with contradictions in `make_bottom` .
- gh-146308: Fixed multiple error handling issues in the `_remote_debugging` module including a double-free in code object
caching, memory leaks on allocation failure, missing exception checks in
binary format varint decoding, reference leaks on error paths in frame
chain processing, and inconsistent thread status error reporting across
platforms. Patch by Pablo Galindo.
- gh-146306: Optimize float arithmetic in the JIT by mutating uniquely-referenced operands in place, avoiding allocation of a new float object. Speeds up the pyperformance `nbody` benchmark by ~19%.
- gh-146128: Fix a bug which could cause constant values to be partially corrupted in AArch64 JIT code. This issue is theoretical, and hasnât actually been observed in unmodified Python interpreters.
- gh-146250: Fixed a memory leak in `SyntaxError` when re-initializing
it.
- gh-146245: Fixed reference leaks in `socket` when audit hooks raise
exceptions in`socket.getaddrinfo()` and`socket.sendto()` .
- gh-146151: `memoryview` now supports the float complex
and double complex C types: formatting characters`'F'` and`'D'` respectively.  Patch by Sergey B Kirpichev.
- gh-146196: Fix potential Undefined Behavior in `PyUnicodeWriter_WriteASCII()` by adding a zero-length check. Patch
by Shamil Abdulaev.
- gh-146227: Fix wrong type in `_Py_atomic_load_uint16` in the C11 atomics
backend (`pyatomic_std.h` ), which used a 32-bit atomic load instead of
16-bit. Found by Mohammed Zuhaib.
- gh-146205: Fixed a bug where `select.epoll.close()` ,`select.kqueue.close()` , and`select.devpoll.close()` silently
ignored errors.
- gh-146199: Comparison of code objects now handles errors correctly.
- gh-145667: Remove the `GET_ITER_YIELD_FROM` instruction, modifying`SEND` to pair with`GET_ITER` when compiling`yield from` expressions.
- gh-146192: Add Base32 support to `binascii` and improve the
performance of the Base32 converters in`base64` . Patch by James Seo.
- gh-135871: Improve multithreaded scaling of PyMutex in low-contention scenarios by reloading the lockâs internal state, without slowing down high-contention scenarios.
- gh-146096: Fixed segmentation fault when called repr for BaseExceptionGroup with empty or 1-size tuple args.
- gh-146056: Fix `repr()` for lists and tuples containing`NULL` s.
- gh-145059: Fixed `sys.lazy_modules` to include lazy modules
without submodules. Patch by Bartosz SÅawecki.
- gh-146041: Fix free-threading scaling bottleneck in `sys.intern()` and`PyObject_SetAttr()` by avoiding the interpreter-wide lock when the
string is already interned and immortalized.
- gh-145990: `python --help-env` sections are now sorted by environment
variable name.
- gh-145990: `python --help-xoptions` is now sorted by`-X` option name.
- gh-145876: `AttributeError` s and`KeyError` s raised in`keys()` or`__getitem__()` during dictionary unpacking
(`{**mymapping}` or`func(**mymapping)` ) are no longer masked by`TypeError` .
- gh-127958: Support tracing from function entrypoints in the JIT. Patch by Ken Jin.
- gh-145376: Fix GC tracking in `structseq.__replace__()` .
- gh-145792: Fix out-of-bounds access when invoking faulthandler on a CPython build compiled without support for VLAs.
- gh-142183: Avoid a pathological case where repeated calls at a specific stack depth could be significantly slower.
- gh-145779: Improve scaling of `classmethod()` and`staticmethod()` calls in the free-threaded build by avoiding the descriptor`__get__` call.
- gh-145783: Fix an unlikely crash in the parser when certain errors were erroneously not propagated. Found by OSS Fuzz in #491369109.
- gh-145685: Improve scaling of type attribute lookups in the free-threaded build by avoiding contention on the internal type lock.
- gh-145713: Make `bytearray.resize()` thread-safe in the free-threaded
build by using a critical section and calling the lock-held variant of the
resize function.
- gh-145036: In free-threaded build, fix race condition when calling `__sizeof__()` on a`list`
- gh-134584: Eliminate redundant refcounting for `MATCH_CLASS` in the JIT.
- gh-69605: Add `math.integer` to REPL auto-completion of
imports.
- gh-131798: Optimize `_ITER_CHECK_RANGE` and`_ITER_CHECK_LIST` in the
JIT
- gh-143414: Add tracking to the JIT optimizer to determine whether a reference is uniquely owned or shared
- gh-143636: Fix a crash when calling `SimpleNamespace.__replace__()` on non-namespace instances. Patch by BÃ©nÃ©dikt
Tran.
- gh-126910: Set frame pointers in `x86_64-unknown-linux-gnu` JIT code,
allowing most native profilers and debuggers to unwind through them.
- gh-140594: Fix an out of bounds read when a single NUL character is read from the standard input. Patch by Shamil Abdulaev.
- gh-140870: Add support for module attributes in the REPL auto-completion of imports.

### LibraryÂ¶

- gh-144503: Fix a regression introduced in 3.14.3 and 3.13.12 where the `multiprocessing``forkserver` start method would fail with`BrokenPipeError` when the parent process had a very large`sys.argv` .  The argv is now passed to the forkserver as separate
command-line arguments rather than being embedded in the`-c` command
string, avoiding the operating systemâs per-argument length limit.
- gh-148153: `base64.b32encode()` now always raises`ValueError` instead of`AssertionError` for the value of*map01* with invalid
length.
- gh-73613: Add the *padded* parameter in functions related to Base32 and
Base64 codecs in the`binascii` and`base64` modules. In the
encoding functions it controls  whether the pad character can be added in
the output, in the decoding functions it controls whether padding is
required in input. Padding of input no longer required in`base64.urlsafe_b64decode()` by default.
- gh-146613: `itertools` : Fix a crash in`itertools.groupby()` when
the grouper iterator is concurrently mutated.
- gh-147944: Accepted range for the *bytes_per_sep* argument of`bytes.hex()` ,`bytearray.hex()` ,`memoryview.hex()` , and`binascii.b2a_hex()` is now increased, so passing`sys.maxsize` and`-sys.maxsize` is now valid.
- gh-146080: `ssl` : fix a crash when an SNI callback tries to use an
SSL object that has already been garbage-collected. Patch by BÃ©nÃ©dikt
Tran.
- gh-146556: Fix `annotationlib.get_annotations()` hanging indefinitely
when called with`eval_str=True` on a callable that has a circular`__wrapped__` chain (e.g.`f.__wrapped__ = f` ). Cycle detection using
an id-based visited set now stops the traversal and falls back to the
globals found so far, mirroring the approach of`inspect.unwrap()` .
- gh-146090: `sqlite3` : fix a crash when`sqlite3.Connection.create_collation()` fails with SQLITE_BUSY. Patch by BÃ©nÃ©dikt Tran.
- gh-146090: `sqlite3` : properly raise`MemoryError` instead of`SystemError` when a context callback fails to be allocated. Patch by
BÃ©nÃ©dikt Tran.
- gh-146507: Make `asyncio.SelectorEventLoop()` stream transportâs`get_write_buffer_size()` O(1) by maintaining
a running byte counter instead of iterating the buffer on every call.
- gh-145056: Fix merging of `collections.OrderedDict` and`frozendict` .
- gh-145056: Add support for merging `collections.UserDict` and`frozendict` .
- gh-145633: Fix `struct.pack('f', float)` : use`PyFloat_Pack4()` to
raise`OverflowError` . Patch by Sergey B Kirpichev and Victor
Stinner.
- gh-146440: `json` : Add the*array_hook* parameter to`load()` and`loads()` functions: allow a callback for
JSON literal array types to customize Python lists in the resulting
decoded object. Passing combined`frozendict` to*object_pairs_hook* param and`tuple` to`array_hook` will yield
a deeply nested immutable Python structure representing the JSON data.
- gh-146431: Add the *wrapcol* parameter to`base64` functions`b16encode()` ,`b32encode()` ,`b32hexencode()` ,`b85encode()` and`z85encode()` , and`binascii` functions`b2a_base32()` and`b2a_base85()` . Add the*ignorechars* parameter to`base64` functions`b16decode()` ,`b32decode()` ,`b32hexdecode()` ,`b85decode()` and`z85decode()` , and`binascii` functions`a2b_hex()` ,`unhexlify()` ,`a2b_base32()` and`a2b_base85()` .
- gh-146310: The `ensurepip` module no longer looks for`pip-*.whl` wheel packages in the current directory.
- gh-141510: Support `frozendict` in`plistlib` , for
serialization only. Patch by Hugo van Kemenade.
- gh-146238: Support half-floats (type code `'e'` of the`struct` module) in the`array` module.  Patch by Sergey B Kirpichev.
- gh-140947: Fix incorrect contextvars handling in server tasks created by `asyncio` . Patch by Kumar Aditya.
- gh-146151: Support the float complex and double complex C types in the `array` module: formatting characters`'F'` and`'D'` respectively.  Patch by Sergey B Kirpichev.
- gh-143387: In importlib.metadata, when a distribution file is corrupt and there is no metadata file, calls to `Distribution.metadata()` (including
implicit calls from other properties like`.name` and`.requires` )
will now raise a`MetadataNotFound` Exception. This allows callers to
distinguish between missing metadata and a degenerate (empty) metadata.
Previously, if the file was missing, an empty`PackageMetadata` would be
returned and would be indistinguishable from the presence of an empty
file.
- gh-146228: Cached FastPath objects in importlib.metadata are now cleared on fork, avoiding broken references to zip files during fork.
- gh-146171: Nested `AttributeError` suggestions now include
property-backed attributes on nested objects without executing the
property getter.
- gh-145410: On Windows, `sysconfig.get_platform()` now gets the
platform from the`_sysconfig` module instead of parsing`sys.version` string. Patch by Victor Stinner.
- gh-146091: Fix a bug in `termios.tcsetwinsize()` where passing a
sequence that raises an exception in`__getitem__` would cause a`SystemError` instead of propagating the original exception.
- gh-146083: Update bundled libexpat to version 2.7.5.
- gh-146076: `zoneinfo` : fix crashes when deleting`_weak_cache` from
a`zoneinfo.ZoneInfo` subclass.
- gh-123471: Make concurrent iteration over `itertools.zip_longest` safe under free-threading.
- gh-146075: Errors when calling `functools.partial()` with a malformed
keyword will no longer crash the interpreter.
- gh-146054: Limit the size of `encodings.search_function()` cache.
Found by OSS Fuzz in #493449985.
- gh-146004: All `-X` options from the Python command line are now
propagated to child processes spawned by`multiprocessing` , not just
a hard-coded subset.  This makes the behavior consistent between default
âspawnâ and âforkserverâ start methods and the old âforkâ start method.
The options that were previously not propagated are:`context_aware_warnings` ,`cpu_count` ,`disable-remote-debug` ,`int_max_str_digits` ,`lazy_imports` ,`no_debug_ranges` ,`pathconfig_warnings` ,`perf` ,`perf_jit` ,`presite` ,`pycache_prefix` ,`thread_inherit_context` , and`warn_default_encoding` .
- gh-145980: Added the *alphabet* parameter in`b2a_base64()` ,`a2b_base64()` ,`b2a_base85()` and`a2b_base85()` and a number of`*_ALPHABET` constants in
the`binascii` module. Removed`b2a_z85()` and`a2b_z85()` .
- gh-145968: Fix translation in `base64.b64decode()` when altchars
overlaps with the standard ones.
- gh-145966: Non- `AttributeError` exceptions raised during dialect
attribute lookup in`csv` are no longer silently suppressed.
- gh-145883: `zoneinfo` : Fix heap buffer overflow reads from malformed
TZif data. Found by OSS Fuzz, issues #492245058 and
#492230068.
- gh-145850: Changed some implementation details in `struct.Struct` :
calling it with non-ASCII string format will now raise a`ValueError` instead of`UnicodeEncodeError` , calling it with non-ASCII bytes
format will now raise a`ValueError` instead of`struct.error` ,
getting the`format` attribute of uninitialized object will now
raise an`AttributeError` instead of`RuntimeError` .
- gh-123720: asyncio: Fix `asyncio.Server.serve_forever()` shutdown
regression. Since 3.12, cancelling`serve_forever()` could hang waiting
for a handler blocked on a read from a client that never closed
(effectively requiring two interrupts to stop); the shutdown sequence now
ensures client streams are closed so`serve_forever()` exits promptly
and handlers observe EOF.
- gh-138122: The `profiling.sampling` module now supports differential
flamegraph visualization via`--diff-flamegraph` to compare two
profiling runs. Functions are colored red (regressions), blue
(improvements), gray (neutral), or purple (new). Elided stacks show code
paths that disappeared between runs.
- gh-145754: Request signature during mock autospec with `FORWARDREF` annotation format. This prevents runtime errors when an annotation uses a
name that is not defined at runtime.
- gh-145750: Avoid undefined behaviour from signed integer overflow when parsing format strings in the `struct` module. Found by OSS Fuzz in
#488466741.
- gh-145719: Add `application/efi` MIME type to`mimetypes` .
- gh-145717: Add a few Microsoft-specific MIME types.
- gh-145703: `asyncio` : Make sure that`loop.call_at` and`loop.call_later` trigger scheduled events on time when the clock
resolution becomes too small.
- gh-145697: Add `application/sql` and`application/vnd.sqlite3` into`mimetypes` .
- gh-145492: Fix infinite recursion in `collections.defaultdict``__repr__` when a`defaultdict` contains itself. Based on analysis by
KowalskiThomas in gh-145492.
- gh-145650: Add `__repr__()` support to`logging.Formatter` and`logging.Filter` , showing the format
string and filter name respectively.
- gh-145587: Resolved a performance regression in `multiprocessing.connection.wait` on Windows that caused infinite busy
loops when called with no objects. The function now properly yields
control to the OS to conserve CPU resources. Patch By Shrey Naithani
- gh-145616: Detect Android sysconfig ABI correctly on 32-bit ARM Android on 64-bit ARM kernel
- gh-145546: Fix `unittest.util.sorted_list_difference()` to deduplicate
remaining elements when one input list is exhausted before the other.
- gh-145446: Now `functools` is safer in free-threaded build when using
keywords in`functools.partial()`
- gh-145264: Base64 decoder (see `binascii.a2b_base64()` ,`base64.b64decode()` , etc) no longer ignores excess data after the
first padded quad in non-strict (default) mode.  Instead, in conformance
with**RFC 4648** , section 3.3, it now ignores the pad character, â=â, if
it is present before the end of the encoded data.
- gh-145035: Allows omitting the internal library `_pyrepl` with limited
loss of functionality. This allows complete removal of the modern REPL,
which is an unsupported configuration, but still desirable for some
distributions.
- gh-144270: Made the *tag* parameter of`xml.etree.ElementTree.Element` and the*parent* and*tag* parameters of`xml.etree.ElementTree.SubElement()` positional-only,
matching the behavior of the C accelerator.
- gh-144984: Fix crash in `xml.parsers.expat.xmlparser.ExternalEntityParserCreate()` when an
allocation fails. The error paths could dereference NULL`handlers` and
double-decrement the parent parserâs reference count.
- gh-144975: `wave.Wave_write.setframerate()` now validates the frame
rate after rounding to an integer, preventing values like`0.5` from
being accepted and causing confusing errors later. Patch by Michiel
Beijen.
- gh-140715: Add `%n` and`%t` support to`strptime()` .
- gh-144259: Fix inconsistent display of long multiline pasted content in the REPL.
- gh-140814: `multiprocessing.freeze_support()` no longer sets the
default start method as a side effect, which previously caused a
subsequent`multiprocessing.set_start_method()` call to raise`RuntimeError` .
- gh-123471: Make concurrent iteration over `itertools.accumulate` safe under free-threading.
- gh-143715: Calling the `Struct.__new__()` without required argument now
is deprecated. Calling`__init__()` method on initialized`Struct` objects is deprecated.
- gh-142763: Fix a race condition between `zoneinfo.ZoneInfo` creation and`zoneinfo.ZoneInfo.clear_cache()` that could raise`KeyError` .
- gh-141707: Donât change `tarfile.TarInfo` type from`AREGTYPE` to`DIRTYPE` when parsing GNU long name or link headers.
- gh-138577: `getpass.getpass()` with non-empty`echo_char` now
handles keyboard shortcuts including Ctrl+A/E (cursor movement), Ctrl+K/U
(kill line), Ctrl+W (erase word), and Ctrl+V (literal next) by reading the
terminalâs control character settings and processing them appropriately in
non-canonical mode. Patch by Sanyam Khurana.
- gh-140049: `traceback.format_exception_only()` now colorizes exception
notes.
- gh-139933: Improve `AttributeError` suggestions for classes with a
custom`__dir__()` method returning a list of unsortable
values. Patch by BÃ©nÃ©dikt Tran.
- gh-139633: The `netrc` security check is now run once per parse
rather than once per entry.
- gh-130472: Add fancycompleter and enable it by default when using pyrepl. This gives colored tab completion.
- gh-112632: Add an *expand* keyword argument for`pprint.pprint()` ,`pprint.pformat()` ,`pprint.pp()` by passing on all*kwargs* and`pprint.PrettyPrinter` . Contributed by Stefan Todoran and Semyon
Moroz.
- gh-66419: Optional argument with nargs equals to `argparse.REMAINDER` now consumes all remaining arguments including`'--'` .
- gh-60729: Add support for floating point audio wave files in `wave` .
- bpo-36461: Make the target time of `timeit.Timer.autorange()` configurable and add`--target-time` option to the command-line
interface of`timeit` .

### DocumentationÂ¶

### TestsÂ¶

- gh-144418: The Android testbedâs emulator RAM has been increased from 2 GB to 4 GB.
- gh-146202: Fix a race condition in regrtest: make sure that the temporary directory is created in the worker process. Previously, temp_cwd() could fail on Windows if the âbuildâ directory was not created. Patch by Victor Stinner.

### BuildÂ¶

- gh-146541: The Android testbed can now be built for 32-bit ARM and x86 targets.
- gh-146498: The iOS XCframework build script now ensures libpython isnât included in installed app content, and is more robust in identifying standard library binary content that requires processing.
- gh-146450: The Android build script was modified to improve parity with other platform build scripts.
- gh-146446: The clean target for the Apple/iOS XCframework build script is now more selective when targeting a single architecture.
- gh-146444: The Apple/iOS build script has been moved to the Platforms directory.
- gh-146210: Fix building the jit stencils on Windows when the interpreter is built with a different clang version. Patch by Chris Eibl.
- gh-145844: Update to WASI SDK 32.
- gh-145801: When Python build is optimized with GCC using PGO, use `-fprofile-update=atomic` option to use atomic operations when updating
profile information. This option reduces the risk of gcov Data Files
(.gcda) corruption which can cause random GCC crashes. Patch by Victor
Stinner.
- gh-138850: Add `--disable-epoll` to`configure`
- gh-145633: Remove support for ancient ARM platforms (ARMv4L and ARMv5L OABI boards), using mixed-endian representation for doubles. Patch by Sergey B Kirpichev.
- gh-85277: Fix building without `stropts.h` or empty`stropts.h`

### WindowsÂ¶

- gh-140131: Fix REPL cursor position on Windows when module completion suggestion line hits console width.

### macOSÂ¶

- gh-137586: Invoke **osascript** with absolute path in`webbrowser` and`turtledemo` .

### Tools/DemosÂ¶

- gh-135953: Properly identify the main thread in the Gecko profiler collector by using a status flag from the interpreter state instead of relying on `threading.main_thread()` in the collector process.
- gh-145976: Remove `Misc/indent.pro` , a configuration file for GNU*indent(1)* .
- gh-145976: Remove `Misc/vgrindefs` and`Misc/Porting` .

### C APIÂ¶

- gh-146636: The `Py_mod_abi` slot is now mandatory for modules
created from a slots array (using`PyModule_FromSlotsAndSpec()` or
the`PyModExport_*` export hook).
- gh-146175: The following macros are soft deprecated: `Py_ALIGNED` ,`PY_FORMAT_SIZE_T` ,`Py_LL` ,`Py_ULL` ,`PY_LONG_LONG` ,`PY_LLONG_MIN` ,`PY_LLONG_MAX` ,`PY_ULLONG_MAX` ,`PY_INT32_T` ,`PY_UINT32_T` ,`PY_INT64_T` ,`PY_UINT64_T` ,`PY_SIZE_MAX` ,`Py_UNICODE_SIZE` ,`Py_VA_COPY` .The macro `Py_UNICODE_WIDE` , which was scheduled for removal, is
soft deprecated instead.
- gh-146143: `PyUnicodeWriter_WriteUCS4()` now accepts a pointer to a
constant buffer of`Py_UCS4` .
- gh-146056: `PyUnicodeWriter_WriteRepr()` now supports`NULL` argument.
- gh-145010: Use GCC dialect alternatives for inline assembly in `object.h` so that the Python headers compile correctly with`-masm=intel` .

## Python 3.15.0 alpha 7Â¶

*Release date: 2026-03-10*

### WindowsÂ¶

- gh-145731: Fix negative timestamp during DST on Windows. Patch by Hugo van Kemenade.
- gh-145307: Defers loading of the `psapi.dll` module until it is used by`ctypes.util.dllist()` .
- gh-144551: Updated bundled version of OpenSSL to 3.5.5.

### TestsÂ¶

- gh-144741: Fix `test_frame_pointer_unwind` when Python is built with`--enable-shared` . Classify also libpython frames as`"python"` .
Patch by Victor Stinner.
- gh-144739: When Python was compiled with system expat older then 2.7.2 but tests run with newer expat, still skip `test.test_pyexpat.MemoryProtectionTest` .

### SecurityÂ¶

- gh-145506: Fixes **CVE 2026-2297** by ensuring that`SourcelessFileLoader` uses`io.open_code()` when opening`.pyc` files.
- gh-144370: Disallow usage of control characters in status in `wsgiref.handlers` to prevent HTTP header injections. Patch by
Benedikt Johannes.

### LibraryÂ¶

- gh-145623: Fix crash in `struct` when calling`repr()` or`__sizeof__()` on an uninitialized`struct.Struct` object created
via`Struct.__new__()` without calling`__init__()` .
- gh-145551: Fix InvalidStateError when cancelling process created by `asyncio.create_subprocess_exec()` or`asyncio.create_subprocess_shell()` . Patch by Daan De Meyer.
- gh-141510: `marshal` now supports`frozendict` objects. The
marshal format version was increased to 6. Patch by Victor Stinner.
- gh-145417: `venv` : Prevent incorrect preservation of SELinux context
when copying the`Activate.ps1` script. The script inherited the SELinux
security context of the system template directory, rather than the
destination project directory.
- gh-145335: `os.listdir(-1)` and`os.scandir(-1)` now fail with`OSError(errno.EBADF)` rather than listing the current directory.`os.listxattr(-1)` now fails with`OSError(errno.EBADF)` rather than
listing extended attributes of the current directory. Patch by Victor
Stinner.
- gh-145376: Fix double free and null pointer dereference in unusual error scenarios in `hashlib` and`hmac` modules.
- gh-145301: `hmac` : fix a crash when the initialization of the
underlying C extension module fails.
- gh-145301: `hashlib` : fix a crash when the initialization of the
underlying C extension module fails.
- gh-76007: The `version` attribute of the`tarfile` module is
deprecated and slated for removal in Python 3.20.
- gh-145158: Avoid undefined behaviour from signed integer overflow when parsing format strings in the `struct` module.
- gh-123853: Removed Windows 95 compatibility for `locale.getdefaultlocale()` .
- gh-66802: Add `unicodedata.block()` function to return the Unicode
block
of a character.
- gh-145033: Add `typing.TypeForm` , implementing**PEP 747** . Patch by
Jelle Zijlstra.
- gh-141510: `dataclasses.field()` : if*metadata* is`None` , use an
empty`frozendict` , instead of a`MappingProxyType()` of
an empty`dict` . Patch by Victor Stinner.
- gh-145006: Add `ModuleNotFoundError` hints when a module for a
different ABI exists.
- gh-141510: `ParameterizedMIMEHeader.params` of`email.headerregistry` is now a`frozendict` instead of a`types.MappingProxyType` . Patch by Victor Stinner.
- gh-134872: Add valid import name suggestions on `ModuleNotFoundError` .
- gh-88091: Fix `unicodedata.decomposition()` for Hangul characters.
- gh-144986: Fix a memory leak in `atexit.register()` . Patch by Shamil
Abdulaev.
- gh-144777: Fix data races in `io.IncrementalNewlineDecoder` in the
free-threaded build.
- gh-144809: Make `collections.deque` copy atomic in the
free-threaded build.
- gh-141510: The `copy` module now supports the`frozendict` type. Patch by Pieter Eendebak based on work by Victor Stinner.
- gh-141510: The `json` module now supports the`frozendict` type. Patch by Victor Stinner.
- gh-144835: Added missing explanations for some parameters in `glob.glob()` and`glob.iglob()` .
- gh-144833: Fixed a use-after-free in `ssl` when`SSL_new()` returns
NULL in`newPySSLSocket()` . The error was reported via a dangling
pointer after the object had already been freed.
- gh-140715: Add `'%D'` support to`strptime()` .
- gh-144782: Fix `argparse.ArgumentParser` to be`pickleable` .
- gh-144763: Fix a race condition in `tracemalloc` : it no longer
detaches the attached thread state to acquire its internal lock. Patch by
Victor Stinner.
- gh-142224: `unicodedata.bidirectional()` now return the correct
default bidi class for unassigned code points.
- gh-117865: Reduce the import time of `inspect` module by ~20%.
- gh-144156: Fix the folding of headers by the `email` library when**RFC 2047** encoded words are used.  Now whitespace is correctly preserved
and also correctly added between adjacent encoded words.  The latter
property was broken by the fix for gh-92081, which mostly fixed previous
failures to preserve whitespace.
- gh-66305: Fixed a hang on Windows in the `tempfile` module when
trying to create a temporary file or subdirectory in a non-writable
directory.
- gh-144615: Methods directly decorated with `@functools.singledispatchmethod` now dispatch on the second argument
when called after being accessed as class attributes. Patch by Bartosz
SÅawecki.
- gh-144321: The functional syntax for creating `typing.NamedTuple` classes now supports passing any iterable of fields and types.
Previously, only sequences were supported.
- gh-144475: Calling `repr()` on`functools.partial()` is now safer
when the partial objectâs internal attributes are replaced while the
string representation is being generated.
- gh-144285: Attribute suggestions in `AttributeError` tracebacks are
now formatted differently to make them easier to understand, for example:`Did you mean '.datetime.now' instead of '.now'` . Contributed by Bartosz
SÅawecki.
- gh-144316: Fix crash in `_remote_debugging` that caused`test_external_inspection` to intermittently fail. Patch by Taegyun Kim.
- gh-143637: Fixed a crash in socket.sendmsg() that could occur if ancillary data is mutated re-entrantly during argument parsing.
- gh-140652: Fix a crash in `_interpchannels.list_all()` after closing
a channel.
- gh-143698: Allow *scheduler* and*setpgroup* arguments to be explicitly`None` when calling`os.posix_spawn()` or`os.posix_spawnp()` . Patch by BÃ©nÃ©dikt Tran.
- gh-143698: Raise `TypeError` instead of`SystemError` when the*scheduler* in`os.posix_spawn()` or`os.posix_spawnp()` is not a
tuple. Patch by BÃ©nÃ©dikt Tran.
- gh-142516: `ssl` : fix reference leaks in`ssl.SSLContext` objects. Patch by BÃ©nÃ©dikt Tran.
- gh-85809: Added path-like object support for `shutil.make_archive()` .
- gh-143304: Fix `ctypes.CDLL` to honor the`handle` parameter on
POSIX systems.
- gh-142781: `zoneinfo` : fix a crash when instantiating`ZoneInfo` objects for which the internal class-level
cache is inconsistent.
- gh-142787: Fix assertion failure in `sqlite3` blob subscript when
slicing with indices that result in an empty slice.
- gh-142352: Fix `asyncio.StreamWriter.start_tls()` to transfer buffered
data from`StreamReader` to the SSL layer, preventing data
loss when upgrading a connection to TLS mid-stream (e.g., when
implementing PROXY protocol support).
- gh-139899: Introduced `importlib.abc.MetaPathFinder.discover()` and`importlib.abc.PathEntryFinder.discover()` to allow module and
submodule name discovery without assuming the use of traditional
filesystem based imports.
- gh-137335: Get rid of any possibility of a name conflict for named pipes in `multiprocessing` and`asyncio` on Windows, no matter how
small.
- gh-135883: Fix `sqlite3` âs interactive shell
keeping part of previous commands when scrolling history.
- gh-124748: Improve `TypeError` error message when`weakref.WeakKeyDictionary.update()` is used with keyword-only
parameters.
- gh-80667: Add support for Tangut Ideographs names in `unicodedata` .
- bpo-42353: The `re` module gains a new`re.prefixmatch()` function as an explicit spelling of what has to date always been known as`re.match()` .`re.Pattern` similary gains a`re.Pattern.prefixmatch()` method.Why? Explicit is better than implicit. Other widely used languages all use the term âmatchâ to mean what Python uses the term âsearchâ for. The unadorened âmatchâ name in Python has been a frequent case of confusion and coding bugs due to the inconsistency with the rest if the software industry. We do not plan to deprecate and remove the older `match` name.
- bpo-40243: Fix `unicodedata.ucd_3_2_0.numeric()` for non-decimal
values.
- bpo-40212: Re-enable `os.posix_fallocate()` and`os.posix_fadvise()` on AIX.
- bpo-3405: Add support for user data of Tk virtual events and detail for `Enter` ,`Leave` ,`FocusIn` ,`FocusOut` , and`ConfigureRequest` events to`tkinter` .
- bpo-32234: `mailbox.Mailbox` instances can now be used as a context
manager. The Mailbox is locked on context entry and unlocked and closed at
context exit.

### DocumentationÂ¶

- gh-145450: Document missing public `wave.Wave_write` getter
methods.
- gh-110937: Document rest of full public `importlib.metadata.Distribution` API. Also add the (already
documented)`PackagePath` to`__all__` .
- gh-136246: A new âImprove this pageâ link is available in the left-hand sidebar of the docs, offering links to create GitHub issues, discussion forum posts, or pull requests.

### Core and BuiltinsÂ¶

- gh-145701: Fix `SystemError` when`__classdict__` or`__conditional_annotations__` is in a class-scope inlined comprehension.
Found by OSS Fuzz in #491105000.
- gh-145615: Fixed a memory leak in the free-threaded build where mimalloc pages could become permanently unreclaimable until the owning thread exited.
- gh-116738: Make `mmap.mmap.set_name()` thread-safe on the free
threaded build.
- gh-145566: In the free threading build, skip the stop-the-world pause when reassigning `__class__` on a newly created object.
- gh-143055: Fix crash in AST unparser when unparsing dict comprehension unpacking. Found by OSS Fuzz in #489790200.
- gh-145335: Fix a crash in `os.pathconf()` when called with`-1` as
the path argument.
- gh-145376: Fix reference leaks in various unusual error scenarios.
- gh-145234: Fixed a `SystemError` in the parser when an encoding cookie
(for example, UTF-7) decodes to carriage returns (`\r` ). Newlines are
now normalized after decoding in the string tokenizer.Patch by Pablo Galindo.
- gh-145275: Added the `-X pathconfig_warnings` and`PYTHON_PATHCONFIG_WARNINGS` options, allowing to disable warnings
from The initialization of the sys.path module search path.
- gh-145273: A warning is now shown during The initialization of the sys.path module search path if it canât find a valid standard library.
- gh-145241: Specialized the parser error for when `with` items are
followed by a trailing comma (for example,`with item,:` ), raising a
clearer`SyntaxError` message. Patch by Pablo Galindo and Bartosz
SÅawecki.
- gh-130555: Fix use-after-free in `dict.clear()` when the dictionary
values are embedded in an object and a destructor causes re-entrant
mutation of the dictionary.
- gh-145197: Fix JIT trace crash when recording function from cleared generator frame.
- gh-145187: Fix compiler assertion fail when a type parameter bound contains an invalid expression in a conditional block.
- gh-145142: Fix a crash in the free-threaded build when the dictionary argument to `str.maketrans()` is concurrently modified.
- gh-145118: `str.maketrans()` now accepts`frozendict` .
- gh-144015: Speed up `bytes.hex()` ,`bytearray.hex()` ,`binascii.hexlify()` , and`hashlib``.hexdigest()` operations
with SIMD on x86-64, ARM64, and ARM32 with NEON when built with gcc
(version 12 or higher) or clang (version 3 or higher) compilers.  Around
1.1-3x faster for common 16-64 byte inputs such as hashlib hex digests,
and up to 8x faster for larger data.
- gh-145118: `type()` now accepts`frozendict` as an argument.
- gh-145064: Fix JIT optimizer assertion failure during `CALL_ALLOC_AND_ENTER_INIT` side exit.
- gh-145055: `exec()` and`eval()` now accept`frozendict` for*globals* . Patch by Victor Stinner.
- gh-145058: Fix a crash when `__lazy_import__()` is passed a
non-string argument, by raising an`TypeError` instead.
- gh-144995: Optimize `memoryview` comparison: a`memoryview` is equal to itself, there is no need to compare values. Patch by Victor
Stinner.
- gh-141510: Update specializer to support frozendict. Patch by Donghee Na.
- gh-141510: Optimize `frozendict.fromkeys()` to avoid unnecessary
thread-safety operations in frozendict cases. Patch by Donghee Na.
- gh-100239: Speedup `BINARY_OP_EXTEND` for exact floats and medium-size
integers by up to 15%. Patch by Chris Eibl.
- gh-144914: Use `mimalloc` for raw memory allocations such as via`PyMem_RawMalloc()` for better performance on free-threaded
builds. Patch by Kumar Aditya.
- gh-144872: Fix heap buffer overflow in the parser found by OSS-Fuzz.
- gh-144766: Fix a crash in fork child process when perf support is enabled.
- gh-144759: Fix undefined behavior in the lexer when `start` and`multi_line_start` pointers are`NULL` in`_PyLexer_remember_fstring_buffers()` and`_PyLexer_restore_fstring_buffers()` . The`NULL` pointer arithmetic
(`NULL - valid_pointer` ) is now guarded with explicit`NULL` checks.
- gh-141510: Add built-in `frozendict` type. Patch by Victor Stinner.
- gh-144681: Fix a JIT assertion failure when a conditional branch jumps to the same target as the fallthrough path.
- gh-143300: Add `PyUnstable_SetImmortal()` C-API function to mark
objects as immortal.
- gh-144702: Clarify the error message raised when a class pattern is used to match on a non-class object.
- gh-144569: Optimize `BINARY_SLICE` for list, tuple, and unicode by
avoiding temporary`slice` object creation.
- gh-144438: Align the QSBR thread state array to a 64-byte cache line boundary to avoid false sharing in the free-threaded build.
- gh-142349: Implement **PEP 810** . Patch by Pablo Galindo and Dino Viehland.
- gh-141226: Deprecate **PEP 456** support for providing an external
definition of the string hashing scheme. Removal is scheduled for Python
3.19. Patch by BÃ©nÃ©dikt Tran.
- gh-138912: Improve `MATCH_CLASS` performance by up to 52% in
certain cases. Patch by Marc Mueller.
- gh-130327: Fix erroneous clearing of an objectâs `__dict__` if overwritten at runtime.
- gh-80667: Literals using the `\N{name}` escape syntax can now construct
CJK ideographs and Hangul syllables using case-insensitive names.

### C APIÂ¶

- gh-142417: Restore private provisional `_Py_InitializeMain()` function
removed in Python 3.14. Patch by Victor Stinner.
- gh-144748: `PyErr_CheckSignals()` now raises the exception scheduled
by`PyThreadState_SetAsyncExc()` , if any.
- gh-144981: Made `PyUnstable_Code_SetExtra()` ,`PyUnstable_Code_GetExtra()` , and`PyUnstable_Eval_RequestCodeExtraIndex()` thread-safe on the
free threaded build.
- gh-141510: Add the following functions for the new `frozendict` type:Patch by Victor Stinner.
- gh-121617: `Python.h` now also includes`<string.h>` in the limited C
API version 3.11 and newer to fix the`Py_CLEAR` macro which uses`memcpy()` . Patch by Victor Stinner.
- gh-144175: Add `PyArg_ParseArray()` and`PyArg_ParseArrayAndKeywords()` functions to parse arguments of
functions using the`METH_FASTCALL` calling convention. Patch by
Victor Stinner.

### BuildÂ¶

- gh-144533: Use wasmtimeâs `--argv0` to auto-discover sysconfig in WASI
builds
- gh-145110: Fix targets âCleanâ and âCLeanAllâ in case of PGO builds on Windows. Patch by Chris Eibl.
- gh-144679: When building with Visual Studio 2026 (Version 18), use PlatformToolSet v145 by default. Patch by Chris Eibl.
- gh-144675: Update to WASI SDK 30.
- gh-136677: Introduce executable specific linker flags to `./configure` .

## Python 3.15.0 alpha 6Â¶

*Release date: 2026-02-11*

### macOSÂ¶

### WindowsÂ¶

- gh-80620: Support negative timestamps in `time.gmtime()` ,`time.localtime()` , and various`datetime` functions.

### TestsÂ¶

- gh-144415: The Android testbed now distinguishes between stdout/stderr messages which were triggered by a newline, and those triggered by a manual call to `flush` . This fixes logging of progress indicators and
similar content.
- gh-65784: Add support for parametrized resource `wantobjects` in
regrtests, which allows to run Tkinter tests with the specified value of`tkinter.wantobjects` , for example`-u wantobjects=0` .

### SecurityÂ¶

- gh-144125: `BytesGenerator` will now refuse to
serialize (write) headers that are unsafely folded or delimited; see`verify_generated_headers` . (Contributed by Bas
Bloemsaat and Petr Viktorin in gh-121650).
- gh-143935: Fixed a bug in the folding of comments when flattening an email message using a modern email policy. Comments consisting of a very long sequence of non-foldable characters could trigger a forced line wrap that omitted the required leading space on the continuation line, causing the remainder of the comment to be interpreted as a new header field. This enabled header injection with carefully crafted inputs.
- gh-143925: Reject control characters in `data:` URL media types.
- gh-143923: Reject control characters in POP3 commands.
- gh-143921: Reject control characters in IMAP commands.
- gh-143919: Reject control characters in `http.cookies.Morsel` fields and values.
- gh-143916: Reject C0 control characters within wsgiref.headers.Headers fields, values, and parameters.

### LibraryÂ¶

- gh-144538: Bump the version of pip bundled in ensurepip to version 26.0.1
- gh-144493: Improve an exception error message in `_overlapped.BindLocal()` that is raised when`asyncio.loop.sock_connect()` is called on a`asyncio.ProactorEventLoop` with a socket that has an invalid
address family.
- gh-144386: Add support for arbitrary descriptors `__enter__()` ,`__exit__()` ,`__aenter__()` , and`__aexit__()` in`contextlib.ExitStack` and`contextlib.AsyncExitStack` , for
consistency with the`with` and`async with` statements.
- gh-123471: Make concurrent iteration over `itertools.combinations_with_replacement` and`itertools.permutations` safe under free-threading.
- gh-74453: Deprecate `os.path.commonprefix()` in favor of`os.path.commonpath()` for path segment prefixes.The `os.path.commonprefix()` function is being deprecated due to
having a misleading name and module. The function is not safe to use for
path prefixes despite being included in a module about path manipulation,
meaning it is easy to accidentally introduce path traversal
vulnerabilities into Python programs by using this function.
- gh-144380: Improve performance of `io.BufferedReader` line
iteration by ~49%.
- gh-144363: Update bundled libexpat to 2.7.4
- gh-140824: When `faulthandler` dumps the list of third-party
extension modules, ignore sub-modules of stdlib packages. Patch by Victor
Stinner.
- gh-144206: Improve error messages for buffer overflow in `fcntl.fcntl()` and`fcntl.ioctl()` .
- gh-144264: Speed up Base64 decoding of data containing ignored characters (both in non-strict mode and with an explicit *ignorechars* argument). It
is now up to 2 times faster for multiline Base64 data.
- gh-144249: Add filename context to `OSError` exceptions raised by`ssl.SSLContext.load_cert_chain()` , allowing users to have more
context.
- gh-132888: Fix incorrect use of `ctypes.GetLastError()` and add
missing error checks for Windows API calls in`_pyrepl.windows_console` .
- gh-142956: Updated `tomllib` to parse TOML 1.1.0.
- gh-144217: `mimetypes` : Add support for DICOM files (for medical
imaging) with the official MIME type`application/dicom` . Patch by
Benedikt Johannes.
- gh-144212: Mime type `image/jxl` is now supported by`mimetypes` .
- gh-143594: Add `symtable.Function.get_cells()` and`symtable.Symbol.is_cell()` methods.
- gh-144169: Fix three crashes when non-string keyword arguments are supplied to objects in the `ast` module.
- gh-144128: Fix a crash in `array.array.fromlist()` when an elementâs`__index__()` method mutates the input list during conversion.
- gh-144100: Fixed a crash in ctypes when using a deprecated `POINTER(str)` type in`argtypes` . Instead of aborting, ctypes now
raises a proper Python exception when the pointer target type is
unresolved.
- gh-143658: `importlib.metadata` : Use`str.lower()` and`str.replace()` to further improve performance of`importlib.metadata.Prepared.normalize()` . Patch by Hugo van Kemenade
and Henry Schreiner.
- gh-144050: Fix `stat.filemode()` in the pure-Python implementation to
avoid misclassifying invalid mode values as block devices.
- gh-83069: `subprocess.Popen.wait()` : when`timeout` is not`None` ,
an efficient event-driven mechanism now waits for process termination, if
available. Linux >= 5.3 uses`os.pidfd_open()` +`select.poll()` .
macOS and other BSD variants use`select.kqueue()` +`KQ_FILTER_PROC` +`KQ_NOTE_EXIT` . Windows keeps using`WaitForSingleObject` (unchanged). If none of these mechanisms are
available, the function falls back to the traditional busy loop
(non-blocking call and short sleeps). Patch by Giampaolo Rodola.
- gh-144030: The Python implementation of `functools.lru_cache()` differed from the default C implementation in that it did not check that
its argument is callable. This discrepancy is now fixed and both raise a`TypeError` .
- gh-144001: Added the *ignorechars* parameter in`binascii.a2b_base64()` and`base64.b64decode()` .
- gh-144023: Fixed validation of file descriptor 0 in posix functions when used with follow_symlinks parameter.
- gh-143999: Fix an issue where `inspect.getgeneratorstate()` and`inspect.getcoroutinestate()` could fail for generators wrapped by`types.coroutine()` in the suspended state.
- gh-143952: Fixed `asyncio` debugging tools to work with new remote
debugging API. Patch by Bartosz SÅawecki.
- gh-143904: `struct.pack_into()` now raises OverflowError instead of
IndexError for too large*offset* argument.
- gh-143897: Remove the `isxidstart()` and`isxidcontinue()` methods of`unicodedata.ucd_3_2_0` . They are now only exposed as`unicodedata.isxidstart()` and`unicodedata.isxidcontinue()` .
- gh-143831: `annotationlib.ForwardRef` objects are now hashable when
created from annotation scopes with closures. Previously, hashing such
objects would throw an exception. Patch by Bartosz SÅawecki.
- gh-143874: Fixed a bug in `pdb` where expression results were not
sent back to remote client.
- gh-143754: Add new `tkinter` widget methods`pack_content()` ,`place_content()` and`grid_content()` which are alternative
spelling of old`*_slaves()` methods.
- gh-143756: Fix potential thread safety issues in `ssl` module.
- gh-132604: Previously, `Protocol` classes that were not
decorated with`@~typing.runtime_checkable` , but that inherited from
another`Protocol` class that did have this decorator, could be used in`isinstance()` and`issubclass()` checks. This behavior is now
deprecated and such checks will throw a`TypeError` in Python 3.20.
Patch by Bartosz SÅawecki.
- gh-143543: Fix a crash in itertools.groupby that could occur when a user-defined `__eq__()` method re-enters the iterator during
key comparison.
- gh-143689: Fix `io.BufferedReader.read1()` state cleanup on buffer
allocation failure.
- gh-143602: Fix a inconsistency issue in `write()` that
leads to unexpected buffer overwrite by deduplicating the buffer exports.
- gh-142434: Use `ppoll()` if available in`select.poll()` to have a
timeout resolution of 1 nanosecond, instead of a resolution of 1 ms. Patch
by Victor Stinner.
- gh-140557: `array.array` buffers now have the same alignment when
empty as when allocated. Unaligned buffers can still be created by
slicing.
- gh-143423: Fix free-threaded build detection in the sampling profiler when Py_GIL_DISABLED is set to 0.
- gh-101178: Add Ascii85, Base85, and Z85 support to `binascii` and
improve the performance of the base-85 converters in`base64` .
- gh-142966: Fix `ctypes.POINTER.set_type()` not updating the format
string to match the type.
- gh-142555: `array` : fix a crash in`a[i] = v` when converting*i* to an index via`i.__index__` or`i.__float__` mutates the array.
- gh-142438: Fix _decimal builds configured with EXTRA_FUNCTIONALITY by correcting the Context.apply wrapper to pass the right argument.
- gh-141860: Add an `on_error` keyword-only parameter to`multiprocessing.set_forkserver_preload()` to control how import
failures during module preloading are handled. Accepts`'ignore'` (default, silent),`'warn'` (emit`ImportWarning` ), or`'fail'` (raise exception). Contributed by Nick Neumann and Gregory P. Smith.
- gh-125346: Accepting `+` and`/` characters with an alternative
alphabet in`base64.b64decode()` and`base64.urlsafe_b64decode()` is now deprecated. In future Python versions they will be errors in the
strict mode and discarded in the non-strict mode.
- gh-140715: Add `'%F'` support to`strptime()` .
- gh-67041: Add the *missing_as_none* parameter to`urlparse()` ,`urlsplit()` and`urldefrag()` functions. Add the*keep_empty* parameter
to`urlunparse()` and`urlunsplit()` functions. This allows to distinguish between empty and not defined URI
components and preserve empty components.
- gh-77188: The `pickle` module now properly handles name-mangled
private methods.

### IDLEÂ¶

- gh-143774: Better explain the operation of Format / Format Paragraph.

### Core and BuiltinsÂ¶

- gh-134584: Optimize and eliminate ref-counting in `_BINARY_OP_SUBSCR_LIST_SLICE`
- gh-144563: Fix interaction of the Tachyon profiler and `ctypes` and
other modules that load the Python shared library (if present) in an
independent map as this was causing the mechanism that loads the binary
information to be confused. Patch by Pablo Galindo
- gh-144601: Fix crash when importing a module whose `PyInit` function
raises an exception from a subinterpreter.
- gh-144549: Fix building the tail calling interpreter on Visual Studio 2026 with free-threading.
- gh-144513: Fix potential deadlock when using critical sections during stop-the-world pauses in the free-threaded build.
- gh-131798: Optimise `_GUARD_TOS_SLICE` in the JIT.
- gh-144330: Move `classmethod` and`staticmethod` initialization from`__init__()` to`__new__()` . Patch by Victor Stinner.
- gh-144446: Fix data races in the free-threaded build when reading frame object attributes while another thread is executing the frame.
- gh-120321: Add `gi_state` ,`cr_state` , and`ag_state` attributes to
generators, coroutines, and async generators that return the current state
as a string (e.g.,`GEN_RUNNING` ). The`inspect` module functions`getgeneratorstate()` ,`getcoroutinestate()` ,
and`getasyncgenstate()` now return these attributes
directly.
- gh-141563: Fix thread safety of  `PyDateTime_IMPORT` .
- gh-144280: Fix a bug in JIT where the predicate symbol had no truthiness
- gh-140550: In `PyModuleDef.m_slots` , allow slots that repeat
information present in`PyModuleDef` .
- gh-139103: Improve scaling of `namedtuple()` instantiation in the free-threaded build.
- gh-144307: Prevent a reference leak in module teardown at interpreter finalization.
- gh-144319: Add huge pages support for the pymalloc allocator. Patch by Pablo Galindo
- gh-120321: Made `gi_yieldfrom` thread-safe in the free-threading build
by using a lightweight lock on the frame state.
- gh-144194: Fix error handling in perf jitdump initialization on memory allocation failure.
- gh-143962: Name suggestion for not normalized name suggests now the normalized name or the closest name to the normalized name. If the suggested name is not ASCII, include also its ASCII representation.
- gh-144157: `bytes.translate()` now allows the compiler to unroll its
loop more usefully for a 2x speedup in the common no-deletions specified
case.
- gh-144068: Fix JIT tracer memory leak, ensure the JIT tracer state is freed when daemon threads are cleaned up during interpreter shutdown.
- gh-144012: Check if the result is `NULL` in`BINARY_OP_EXTENT` opcode.
- gh-144007: Eliminate redundant refcounting in the JIT for `BINARY_OP` .
- gh-144005: Eliminate redundant refcounting from `BINARY_OP_EXTEND` .
- gh-143939: Fix erroneous âcannot reuse already awaited coroutineâ error that could occur when a generator was run during the process of clearing a coroutineâs frame.
- gh-141805: Fix crash in `set` when objects with the same hash are
concurrently added to the set after removing an element with the same hash
while the set still contains elements with the same hash.
- gh-143670: Fixes a crash in `ga_repr_items_list` function.
- gh-143650: Fix race condition in `importlib` where a thread could
receive a stale module reference when another threadâs import fails.
- gh-143569: Generator expressions in 3.15 now conform to the documented behavior when the iterable does not support iteration. This matches the behavior in 3.14 and earlier
- gh-143192: Improve performance of bitwise operations on multi-digit ints.
- gh-132657: If we are specializing to `LOAD_GLOBAL_MODULE` or`LOAD_ATTR_MODULE` , try to enable deferred reference counting for the
value, if the object is owned by a different thread.  This applies to the
free-threaded build only and should improve scaling of multi-threaded
programs. Note that when deferred reference counting is enabled, the
object will be deallocated by the GC, rather than by`Py_DECREF()` .
- gh-143055: Implement **PEP 798** (Unpacking in Comprehensions).  Patch by
Adam Hartz.
- gh-142037: Improve error messages for printf-style formatting. For errors in the format string, always include the position of the start of the format unit. For errors related to the formatted arguments, always include the number or the name of the argument. Raise more specific errors and include more information (type and number of arguments, most probable causes of error).
- gh-140557: `bytearray` buffers now have the same alignment when
empty as when allocated. Unaligned buffers can still be created by
slicing.
- gh-140232: Frozenset objects with immutable elements are no longer tracked by the garbage collector.
- gh-115231: Setup `__module__` attribute for built-in static methods.
Patch by Sergey B Kirpichev.
