---
id: collect-261121-veille-ia/docs-techniques/changelog-2-3
title: "Changelog"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["patch", "pylongwriter", "object", "victor", "stinner", "pylong", "python", "main"]
source: https://docs.python.org/release/3.15.1/whatsnew/changelog.html
source_anchor: ""
source_lines: [3001, 4500]
section: "Docs_Techniques"
---
### C APIÂ¶

- gh-143869: Added `PyLong_GetNativeLayout()` ,`PyLongLayout` ,`PyLongExport` ,`PyLong_Export()` ,`PyLong_FreeExport()` ,`PyLongWriter` ,`PyLongWriter_Create()` ,`PyLongWriter_Finish()` and`PyLongWriter_Discard()` to the
limited API.
- gh-141070: Renamed `PyUnstable_Object_Dump()` to`PyObject_Dump()` .

### BuildÂ¶

- gh-140421: Disable the perf trampoline on older macOS versions where it cannot be built.
- gh-144309: Build Python with POSIX 2024, instead of POSIX 2008. Patch by Victor Stinner.
- gh-144278: Enables defining the `_PY_IMPL_NAME` and`_PY_IMPL_CACHE_TAG` preprocessor definitions to override`sys.implementation` at build time. Definitions need to include
quotes when setting to a string literal. Setting the cache tag to`NULL` has the effect of completely disabling automatic creation and use of`.pyc` files.
- gh-143960: Add support for OpenSSL 3.6, drop EOL 3.2. Patch by Hugo van Kemenade.
- gh-143941: Move WASI-related files to `Platforms/WASI` . Along the
way, leave a deprecated`Tools/wasm/wasi/__main__.py` behind for
backwards-compatibility.
- gh-143842: Prevent static builds from clashing with curses by making the optimizer COLORS table static.

## Python 3.15.0 alpha 5Â¶

*Release date: 2026-01-14*

### WindowsÂ¶

### Tools/DemosÂ¶

- gh-142095: Make gdb âpy-btâ command use frame from thread local state when available. Patch by Sam Gross and Victor Stinner.

### TestsÂ¶

### LibraryÂ¶

- gh-143706: Fix `multiprocessing` forkserver so that`sys.argv` is correctly set before`__main__` is preloaded. Previously,`sys.argv` was empty during main module import in forkserver child
processes. This fixes a regression introduced in 3.13.8 and 3.14.1. Root
caused by Aaron Wieczorek, test provided by Thomas Watson, thanks!
- gh-143638: Forbid reentrant calls of the `pickle.Pickler` and`pickle.Unpickler` methods for the C implementation. Previously,
this could cause crash or data corruption, now concurrent calls of methods
of the same object raise`RuntimeError` .
- gh-143658: `importlib.metadata` : Use`str.translate()` to improve
performance of`importlib.metadata.Prepared.normalize()` . Patch by
Hugo van Kemenade and Henry Schreiner.
- gh-78724: Raise `RuntimeError` âs when user attempts to call methods
on half-initialized`Struct` objects, For example, created
by`Struct.__new__(Struct)` .  Patch by Sergey B Kirpichev.
- gh-143196: Fix crash when the internal encoder object returned by undocumented function `json.encoder.c_make_encoder()` was called with
non-zero second (*_current_indent_level* ) argument.
- gh-143191: `_thread.stack_size()` now raises`ValueError` if the
stack size is too small. Patch by Victor Stinner.
- gh-143547: Fix `sys.unraisablehook()` when the hook raises an
exception and changes`sys.unraisablehook()` : hold a strong reference
to the old hook. Patch by Victor Stinner.
- gh-139686: Revert 0a97941245f1dda6d838f9aaf0512104e5253929 and 57db12514ac686f0a752ec8fe1c08b6daa0c6219 which made importlib.reload a no-op for lazy modules; caused Buildbot failures.
- gh-143517: `annotationlib.get_annotations()` no longer raises a`SyntaxError` when evaluating a stringified starred annotation that
starts with one or more whitespace characters followed by a`*` . Patch
by Bartosz SÅawecki.
- gh-143474: Add `os.RWF_ATOMIC` constant for Linux 6.11+.
- gh-143445: Speed up `copy.deepcopy()` by 1.04x.
- gh-143378: Fix use-after-free crashes when a `BytesIO` object
is concurrently mutated during`write()` or`writelines()` .
- gh-143368: Fix endless retry loop in `profiling.sampling` blocking
mode when threads cannot be seized due to`EPERM` . Such threads are now
skipped instead of causing repeated error messages. Patch by Pablo
Galindo.
- gh-143346: Fix incorrect wrapping of the Base64 data in `plistlib._PlistWriter` when the indent contains a mix of tabs and
spaces.
- gh-140025: `queue` : Fix`SimpleQueue.__sizeof__()` computation.
- gh-143310: `tkinter` : fix a crash when a Python`list` is
mutated during the conversion to a Tcl object (e.g., when setting a Tcl
variable). Patch by BÃ©nÃ©dikt Tran.
- gh-143309: Fix a crash in `os.execve()` on non-Windows platforms when
given a custom environment mapping which is then mutated during parsing.
Patch by BÃ©nÃ©dikt Tran.
- gh-143308: `pickle` : fix use-after-free crashes when a`PickleBuffer` is concurrently mutated by a custom buffer
callback during pickling. Patch by BÃ©nÃ©dikt Tran and Aaron Wieczorek.
- gh-142939: Performance optimisations for `difflib.get_close_matches()`
- gh-124951: The base64 implementation behind the `binascii` ,`base64` , and related codec has been optimized for modern pipelined
CPU architectures and now performs 2-3x faster across all platforms.
- gh-143237: Fix support of named pipes in the rotating `logging` handlers.
- gh-143249: Fix possible buffer leaks in Windows overlapped I/O on error handling.
- gh-143241: `zoneinfo` : fix infinite loop in`ZoneInfo.from_file` when parsing a malformed TZif file. Patch
by Fatih Celik.
- gh-142830: `sqlite3` : fix use-after-free crashes when the
connectionâs callbacks are mutated during a callback execution. Patch by
BÃ©nÃ©dikt Tran.
- gh-143200: `xml.etree.ElementTree` : fix use-after-free crashes in`__getitem__()` and`__setitem__()` methods of`Element` when the element is concurrently
mutated. Patch by BÃ©nÃ©dikt Tran.
- gh-143214: Add the *wrapcol* parameter in`binascii.b2a_base64()` and`base64.b64encode()` .
- gh-142195: Updated timeout evaluation logic in `subprocess` to be
compatible with deterministic environments like Shadow where time moves
exactly as requested.
- gh-140739: Fix several crashes due to reading invalid memory in the new Tachyon sampling profiler. Patch by Pablo Galindo.
- gh-142164: Fix the ctypes bitfield overflow error message to report the correct offset and size calculation.
- gh-143145: Fixed a possible reference leak in ctypes when constructing results with multiple output parameters on error.
- gh-143103: Add padding support to `base64.z85encode()` via the`pad` parameter.
- gh-130796: Undeprecate the `locale.getdefaultlocale()` function. Patch
by Victor Stinner.
- gh-74902: Add the `iter_graphemes()` function in the`unicodedata` module to iterate over grapheme clusters according to
rules defined in Unicode Standard Annex #29, âUnicode Text Segmentationâ. Add`grapheme_cluster_break()` ,`indic_conjunct_break()` and`extended_pictographic()` functions to get the properties
of the character which are related to the above algorithm.
- gh-143004: Fix a potential use-after-free in `collections.Counter.update()` when user code mutates the Counter
during an update.
- gh-140648: The `asyncio` REPL now respects the`-I` flag
(isolated mode). Previously, it would load and execute`PYTHONSTARTUP` even if the flag was set. Contributed by Bartosz
SÅawecki.
- gh-142991: Fixed socket operations such as recvfrom() and sendto() for FreeBSD divert(4) socket.
- gh-116738: Make the attributes in `lzma` thread-safe on the
free threaded build.
- gh-142950: Fix regression in `argparse` where format specifiers in
help strings raised`ValueError` .
- gh-142881: Fix concurrent and reentrant call of `atexit.unregister()` .
- gh-142615: Fix possible crashes when initializing `asyncio.Task` or`asyncio.Future` multiple times. These classes can now be
initialized only once and any subsequent initialization attempt will raise
a RuntimeError. Patch by Kumar Aditya.
- gh-142517: The non- `compat32``email` policies now correctly handle
refolding encoded words that contain bytes that can not be decoded in
their specified character set.  Previously this resulted in an encoding
exception during folding.
- gh-138122: The Tachyon profilerâs live TUI now integrates with the experimental `_colorize` theming system. Users can customize colors
via`_colorize.set_theme()` (experimental API, subject to change). A`LiveProfilerLight` theme is provided for light terminal
backgrounds. Patch by Pablo Galindo.
- gh-142306: Improve errors for `Element.remove` .
- gh-63016: Add a `flags` parameter to`mmap.mmap.flush()` to control
synchronization behavior.
- gh-139262: Some keystrokes can be swallowed in the new `PyREPL` on
Windows, especially when used together with the ALT key. Fix by Chris
Eibl.
- gh-138897: Improved `license` /`copyright` /`credits` display in the REPL: now uses a pager.
- gh-135852: Add `_winapi.RegisterEventSource()` ,`_winapi.DeregisterEventSource()` and`_winapi.ReportEvent()` .
Using these functions in`NTEventLogHandler` to
replace`pywin32` .
- gh-109263: Starting a process from spawn context in `multiprocessing` no longer sets the start method globally.
- gh-132715: Skip writing objects during marshalling once a failure has occurred.

### DocumentationÂ¶

- gh-140806: Add documentation for `enum.bin()` .

### Core and BuiltinsÂ¶

- gh-134584: Eliminate redundant refcounting from `_CONTAINS_OP` ,`_CONTAINS_OP_SET` and`_CONTAINS_OP_DICT` .
- gh-143604: Fix a reference counting issue in the JIT tracer where the current executor could be prematurely freed during tracing.
- gh-143469: Enable `LOAD_ATTR_MODULE` specialization even if`__getattr__()` is defined in module.
- gh-134584: Eliminate redundant refcounting from `TO_BOOL_STR` .
- gh-143377: Fix a crash in `_interpreters.capture_exception()` when
the exception is incorrectly formatted. Patch by BÃ©nÃ©dikt Tran.
- gh-139757: Add `BINARY_OP_SUBSCR_USTR_INT` to specialize reading an
ASCII character from any string. Patch by Chris Eibl.
- gh-141504: Factor out tracing and optimization heuristics into a single object. Patch by Donghee Na.
- gh-142982: Specialize `CALL_FUNCTION_EX` for Python and non-Python
callables.
- gh-136924: The interactive help mode in the REPL no longer incorrectly syntax highlights text input as Python code. Contributed by Olga Matoula.
- gh-139757: Fix unintended bytecode specialization for non-ascii string. Patch by Donghee Na, Ken Jin and Chris Eibl.
- gh-143361: Add `PY_VECTORCALL_ARGUMENTS_OFFSET` to`_Py_CallBuiltinClass_StackRefSteal` to avoid redundant allocations
- gh-131798: The JIT optimizer now understands more generator instructions.
- gh-134584: Eliminate redundant refcounting from `_LOAD_ATTR_SLOT` .
- gh-143189: Fix crash when inserting a non- `str` key into a split
table dictionary when the key matches an existing key in the split table
but has no corresponding value in the dict.
- gh-143228: Fix use-after-free in perf trampoline when toggling profiling while threads are running or during interpreter finalization with daemon threads active. The fix uses reference counting to ensure trampolines are not freed while any code object could still reference them. Pach by Pablo Galindo
- gh-142664: Fix a use-after-free crash in `memoryview.__hash__` when the`__hash__` method of the referenced object
mutates that object or the view. Patch by BÃ©nÃ©dikt Tran.
- gh-142557: Fix a use-after-free crash in bytearray.__mod__ when the `bytearray` is mutated while
formatting the`%` -style arguments. Patch by BÃ©nÃ©dikt Tran.
- gh-143195: Fix use-after-free crashes in `bytearray.hex()` and`memoryview.hex()` when the separatorâs`__len__()` mutates the original object. Patch by BÃ©nÃ©dikt Tran.
- gh-143183: Fix a bug in the JIT when dealing with unsupported control-flow or operations.
- gh-142975: Fix crash after unfreezing all objects tracked by the garbage collector on the free threaded build.
- gh-143135: Set `sys.flags.inspect` to`1` when`PYTHONINSPECT` is`0` . Previously, it was set to`0` in this
case.
- gh-143123: Protect the JIT against recursive tracing.
- gh-143092: Fix a crash in the JIT when dealing with `list.append(x)` style code.
- gh-143003: Fix an overflow of the shared empty buffer in `bytearray.extend()` when`__length_hint__()` returns 0 for
non-empty iterator.
- gh-143006: Fix a possible assertion error when comparing negative non-integer `float` and`int` with the same number of bits in the
integer part.
- gh-116738: Fix thread safety of `contextvars.Context.run()` .
- gh-142829: Fix a use-after-free crash in `contextvars.Context` comparison when a custom`__eq__` method modifies the context via`set()` .
- gh-142863: Generate optimized bytecode when calling `list` or`set` with generator expression.
- gh-41779: Allowed defining any __slots__ for a class derived from `tuple` (including classes created by`collections.namedtuple()` ).
- gh-69605: Fix edge-cases around already imported modules in the REPL auto-completion of imports.
- gh-138568: Adjusted the built-in `help()` function so that empty
inputs are ignored in interactive mode.
- gh-131798: Remove bounds check when indexing into tuples with a constant index.
- gh-134584: Eliminate redundant refcounting from `_CALL_TYPE_1` . Patch by
Tomas Roun
- gh-132108: Speed up `int.from_bytes()` when passed object supports
buffer protocol, like`bytearray` by ~1.2x.
- gh-128334: Make the `slice` class subscriptable at runtime to be
consistent with typing implementation.

### C APIÂ¶

- gh-141671: `PyMODINIT_FUNC` (and the new`PyMODEXPORT_FUNC` ) now adds a linkage declaration
(`__declspec(dllexport)` ) on Windows.

## Python 3.15.0 alpha 4Â¶

*Release date: 2026-01-13*

### TestsÂ¶

- gh-142836: Accommodated Solaris in `test_pdb.test_script_target_anonymous_pipe` .

### LibraryÂ¶

- gh-122431: Corrected the error message in `readline.append_history_file()` to state that`nelements` must be
non-negative instead of positive.
- gh-143046: The `asyncio` REPL no longer prints copyright and version
messages in the quiet mode (`-q` ). Patch by Bartosz SÅawecki.
- gh-80744: Fix issue where `pdb` would read a`.pdbrc` twice if
launched from the home directory
- gh-138122: Add blocking mode to Tachyon for accurate stack traces in applications with many generators or fast-changing call stacks. Patch by Pablo Galindo.
- gh-143010: Fixed a bug in `mailbox` where the precise timing of an
external event could result in the library opening an existing file
instead of a file it expected to create.
- gh-112127: Fix possible use-after-free in `atexit.unregister()` when
the callback is unregistered during comparison.
- gh-138122: Fix incomplete stack traces in the Tachyon profilerâs frame cache when profiling code with deeply nested generators. The frame cache now validates that stack traces reach the base frame before caching, preventing broken flamegraphs. Patch by Pablo Galindo.
- gh-142834: Change the `pdb``commands` command to use the last
available breakpoint instead of failing when the most recently created
breakpoint was deleted.
- gh-142783: Fix zoneinfo use-after-free with descriptor _weak_cache. a descriptor as _weak_cache could cause crashes during object creation. The fix ensures proper reference counting for descriptor-provided objects.
- gh-76007: Deprecate `VERSION` from`xml.etree.ElementTree` and`version` from`xml.sax.expatreader` and`xml.sax.handler` .
Patch by Hugo van Kemenade.
- gh-142784: The `asyncio` REPL now properly closes the loop upon the
end of interactive session. Previously, it could cause surprising
warnings. Contributed by Bartosz SÅawecki.
- gh-138122: Add binary output format to `profiling.sampling` for
compact storage of profiling data. The new`--binary` option captures
samples to a file that can be converted to other formats using the`replay` command. Patch by Pablo Galindo
- gh-142495: `collections.defaultdict` now prioritizes`__setitem__()` when inserting default values from`default_factory` . This prevents race conditions where a default value
would overwrite a value set before`default_factory` returns.
- gh-142654: Show the clearer error message when using `profiling.sampling` on an unknown PID.
- gh-142560: Fix use-after-free in `bytearray` search-like methods
(`find()` ,`count()` ,`index()` ,`rindex()` , and`rfind()` ) by marking the storage as exported which causes
reallocation attempts to raise`BufferError` . For`contains()` ,`split()` , and`rsplit()` the buffer protocol is
used for this.
- gh-142419: `mmap.mmap.set_name()` method added to annotate an
anonymous memory map if Linux kernel supports`PR_SET_VMA_ANON_NAME` (Linux 5.17 or newer). Patch by Donghee Na.
- gh-139971: `pydoc` : Ensure that the link to the online documentation
of a stdlib module is correct.
- gh-124098: Fix issue where methods in handlers that lacked the protocol name but matched a valid base handler method (e.g., `_open()` or`error()` ) were incorrectly added to`urllib.request.OpenerDirector` âs handlers. Contributed by Andrea
Mattei.
- gh-136282: Add support for `UNNAMED_SECTION` when
creating a section via the mapping protocol access

### Core and BuiltinsÂ¶

- gh-143057: Avoid locking in `PyTraceMalloc_Track()` and`PyTraceMalloc_Untrack()` when`tracemalloc` is not enabled.
- gh-139109: Add missing terminator in certain cases when tracing in the new JIT compiler.
- gh-142961: Fix a segfault in the JIT when constant folding `len(tuple)` .
- gh-142776: Fix a file descriptor leak in import.c
- gh-139757: Fix building JIT stencils on free-threaded builds.
- gh-129068: Make concurrent iteration over the same range iterator thread-safe in the free threading build.
- gh-142543: Fix a stack overflow on Clang JIT build configurations with full LTO.
- gh-142448: Fix a bug when using monitoring with the JIT.
- gh-142766: Clear the frame of a generator when `generator.close()` is
called.
- gh-134584: Eliminate redundant refcounting from `_LOAD_ATTR_INSTANCE_VALUE` .
- gh-134584: Eliminate redundant refcounting from `_STORE_ATTR_WITH_HINT` .
- gh-142476: Fix a memory leak in the experimental Tier 2 optimizer when creating executors. Patched by Shamil Abdulaev.
- gh-100964: Fix reference cycle in exhausted generator frames. Patch by Savannah Ostrowski.
- gh-139922: Allow building CPython with the tail calling interpreter on Visual Studio 2026 MSVC. This provides a performance gain over the prior interpreter for MSVC. Patch by Ken Jin, Brandt Bucher, and Chris Eibl. With help from the MSVC team including Hulon Jenkins.

## Python 3.15.0 alpha 3Â¶

*Release date: 2025-12-16*

### Tools/DemosÂ¶

- gh-141692: Each slice of an iOS XCframework now contains a `lib` folder
that contains a symlink to the libpython dylib. This allows binary modules
to be compiled for iOS using dynamic libreary linking, rather than
Framework linking.

### TestsÂ¶

- gh-140381: Fix flaky test_profiling tests on i686 and s390x architectures by increasing slow_fibonacci call frequency from every 5th iteration to every 2nd iteration.
- gh-140210: Make `test_sysconfig.test_parse_makefile_renamed_vars` less
fragile by clearing the environment variables before parsing the Makefile.

### SecurityÂ¶

- gh-142145: Remove quadratic behavior in `xml.minidom` node ID cache
clearing.
- gh-42400: Fix buffer overflow in `_Py_wrealpath()` for paths exceeding`MAXPATHLEN` bytes by using dynamic memory allocation instead of
fixed-size buffer. Patch by Shamil Abdulaev.
- gh-119451: Fix a potential memory denial of service in the `http.client` module. When connecting to a malicious server, it could
cause an arbitrary amount of memory to be allocated. This could have led
to symptoms including a`MemoryError` , swapping, out of memory (OOM)
killed processes or containers, or even system crashes.
- gh-119342: Fix a potential memory denial of service in the `plistlib` module. When reading a Plist file received from untrusted source, it could
cause an arbitrary amount of memory to be allocated. This could have led
to symptoms including a`MemoryError` , swapping, out of memory (OOM)
killed processes or containers, or even system crashes.

### LibraryÂ¶

- gh-142754: Add the *ownerDocument* attribute to`xml.dom.minidom` elements and attributes created by directly instantiating the`Element` or`Attr` class. Note that this way of creating nodes is not supported;
creator functions like`xml.dom.Document.documentElement()` should
be used instead.
- gh-142594: Fix crash in `TextIOWrapper.close()` when the underlying
bufferâs`closed` property calls`detach()` .
- gh-76007: Deprecate `__version__` from`ctypes` . Patch by Hugo van
Kemenade.
- gh-76007: Deprecate `__version__` from`wsgiref.simple_server` .
Patch by Hugo van Kemenade.
- gh-142651: `unittest.mock` : fix a thread safety issue where`Mock.call_count` may return
inaccurate values when the mock is called concurrently from multiple
threads.
- gh-76007: Deprecate `__version__` from`http.server` . Patch by Hugo
van Kemenade.
- gh-138122: Add `--subprocesses` flag to`profiling.sampling` CLI to
automatically profile subprocesses spawned by the target. When enabled,
the profiler monitors for new Python subprocesses and profiles each one
separately, writing results to individual output files. This is useful for
profiling applications that use`multiprocessing` ,`ProcessPoolExecutor` , or other
subprocess-based parallelism. Patch by Pablo Galindo.
- gh-142595: Added type check during initialization of the `decimal` module to prevent a crash in case of broken stdlib.  Patch by Sergey B
Kirpichev.
- gh-142556: Fix crash when a task gets re-registered during finalization in `asyncio` . Patch by Kumar Aditya.
- gh-138122: Add `--mode=exception` to the sampling profiler to capture
samples only from threads with an active exception, useful for analyzing
exception handling overhead. Patch by Pablo Galindo.
- gh-142539: `traceback` : Fix location of carets in`SyntaxError` s when the source contains wide characters.
- gh-123241: Avoid reference count operations in garbage collection of `ctypes` objects.
- gh-142451: `hmac` : correctly copy`HMAC` attributes for
objects copied through`HMAC.copy()` . Patch by
BÃ©nÃ©dikt Tran.
- gh-138122: The `profiling.sampling` flamegraph profiler now supports
inverted flamegraph view that aggregates all leaf nodes. In a standard
flamegraph, if a hot function is called from multiple locations, it
appears multiple times as separate leaf nodes. In the inverted flamegraph,
all occurrences of the same leaf function are merged into a single
aggregated node at the root, showing the total hotness of that function in
one place. The children of each aggregated node represent its callers,
making it easier to identify which functions consume the most CPU time and
where they are called from.
- gh-112527: The help text for required options in `argparse` no longer
extended with â (default: None)â.
- gh-142438: Fixed a possible leaked GIL in _PySSL_keylog_callback.
- gh-138122: Add bytecode-level instruction profiling to the sampling profiler via the new `--opcodes` flag. When enabled, the profiler
captures which bytecode opcode is executing at each sample, including
Python 3.11+ adaptive specializations, and visualizes this data in the
heatmap, flamegraph, gecko, and live output formats. Patch by Pablo
Galindo
- gh-142389: Add backtick markup support in `argparse` description and
epilog text to highlight inline code when color output is enabled.
- gh-142346: Fix usage formatting for mutually exclusive groups in `argparse` when they are preceded by positional arguments or followed
or intermixed with other optional arguments.
- gh-142374: Fix cumulative percentage calculation for recursive functions in the new sampling profiler. When profiling recursive functions, cumulative statistics (cumul%, cumtime) could exceed 100% because each recursive frame in a stack was counted separately. For example, a function recursing 500 times in every sample would show 50000% cumulative presence. The fix deduplicates locations within each sample so cumulative stats correctly represent âpercentage of samples where this function was on the stackâ. Patch by Pablo Galindo.
- gh-142315: Pdb can now run scripts from anonymous pipes used in process substitution. Patch by Bartosz SÅawecki.
- gh-64532: Subparser help now includes required optional arguments from the parent parser in the usage, making it clearer what arguments are needed to run a subcommand. Patch by Savannah Ostrowski.
- gh-142207: Fix: profiling.sampling may cause assertion ```
!(has_gil &&
gil_requested)
```
- gh-142332: Fix usage formatting for positional arguments in mutually exclusive groups in `argparse` . in`argparse` .
- gh-142282: Fix `winreg.QueryValueEx()` to not accidentally read
garbage buffer under race condition.
- gh-142318: Fix typing `'q'` at the help of the interactive tachyon
profiler exiting the profiler.
- gh-75949: Fix `argparse` to preserve`|` separators in mutually
exclusive groups when the usage line wraps due to length.
- gh-142267: Improve `argparse` performance by caching the formatter
used for argument validation.
- gh-139862: Remove `color` parameter from`argparse.HelpFormatter` constructor. Color is controlled by`ArgumentParser` .
- gh-68552: `MisplacedEnvelopeHeaderDefect` and`Missing header name` defects are now correctly passed to the`handle_defect` method of`policy` in`FeedParser` .
- gh-142206: The resource tracker in the `multiprocessing` module can
now understand messages from older versions of itself. This avoids issues
with upgrading Python while it is running. (Note that such âin-placeâ
upgrades are not tested.)
- gh-142214: Fix two regressions in `dataclasses` in Python 3.14.1
related to annotations.
  - An exception is no longer raised if `slots=True` is used and the`__init__` method does not have an`__annotate__` attribute
(likely because`init=False` was used).
  - An exception is no longer raised if annotations are requested on the `__init__` method and one of the fields is not present in the class
annotations. This can occur in certain dynamic scenarios.
 Patch by Jelle Zijlstra.
- gh-142203: Remove the *debug_override* parameter from`importlib.util.cache_from_source()` which has been deprecated since
Python 3.5.
- gh-138122: The `_remote_debugging` module now implements frame caching
in the`RemoteUnwinder` class to reduce memory reads when profiling
remote processes. When`cache_frames=True` , unchanged portions of the
call stack are reused from previous samples, significantly improving
profiling performance for deep call stacks.
- gh-116738: Fix `cmath` data race when initializing trigonometric
tables with subinterpreters.
- gh-141982: Allow `pdb` to set breakpoints on async functions with
function names.
- gh-74389: When the stdin being used by a `subprocess.Popen` instance is closed, this is now ignored in`subprocess.Popen.communicate()` instead of leaving the class in an
inconsistent state.
- gh-87512: Fix `subprocess.Popen.communicate()` timeout handling on
Windows when writing large input. Previously, the timeout was ignored
during stdin writing, causing the method to block indefinitely if the
child process did not consume input quickly. The stdin write is now
performed in a background thread, allowing the timeout to be properly
enforced.
- gh-141939: Add color to all interpolated values in `argparse` help,
like`%(default)s` or`%(choices)s` . Patch by Alex PrengÃ¨re.
- gh-141473: When `subprocess.Popen.communicate()` was called with*input* and a*timeout* and is called for a second time after a`TimeoutExpired` exception before the process has died,
it should no longer hang.
- gh-141999: Correctly allow `KeyboardInterrupt` to stop the process
when using`profiling.sampling` .
- gh-142006: Fix a bug in the `email.policy.default` folding algorithm
which incorrectly resulted in a doubled newline when a line ending at
exactly max_line_length was followed by an unfoldable token.
- gh-141968: Remove data copy from `re` compilation of regexes with
large charsets by using`bytearray.take_bytes()` .
- gh-141968: Remove data copy from `encodings.idna``encode()` and`encode()` by using`bytearray.take_bytes()` .
- gh-141968: Remove data copy from `codecs``punycode` encoding by
using`bytearray.take_bytes()` .
- gh-141968: Remove data copy from `wave.Wave_read.readframes()` and`wave.Wave_write.writeframes()` by using`bytearray.take_bytes()` .
- gh-141968: Remove a data copy from `base64.b32decode()` and`base64.b32encode()` by using`bytearray.take_bytes()` .
- gh-59000: Fix `pdb` breakpoint resolution for class methods when the
module defining the class is not imported.
- gh-116738: Fix thread safety issue with `re` scanner objects in
free-threaded builds.
- gh-138122: The `profiling.sampling` flamegraph profiler now displays
thread status statistics showing the percentage of time threads spend
holding the GIL, running without the GIL, waiting for the GIL, and
performing garbage collection. These statistics help identify GIL
contention and thread behavior patterns. When filtering by thread, the
display shows per-thread metrics.
- gh-141781: Fixed an issue where pdb.line_prefix assignment was ignored if assigned after the module was imported.
- gh-141863: Update Streams to use `bytearray.take_bytes()` for a over 10% performance improvement on
pyperformance asyncio_tcp benchmark.
- gh-141817: Add `socket.IPV6_HDRINCL` constant.
- gh-105836: Fix `asyncio.run_coroutine_threadsafe()` leaving underlying
cancelled asyncio task running.
- gh-141570: Support file-like object raising `OSError` from`fileno()` in color detection
(`_colorize.can_colorize()` ). This can occur when`sys.stdout` is
redirected.
- gh-141679: Add colour to defaults in `argparse` help. Patch by Hugo
van Kemenade.
- gh-141686: Break reference cycles created by each call to `json.dump()` or`json.JSONEncoder.iterencode()` .
- gh-141659: Fix bad file descriptor errors from `_posixsubprocess` on
AIX.
- gh-141645: Add a new `--live` mode to the tachyon profiler in`profiling.sampling` module. This mode consist of a live TUI that
displays real-time profiling statistics as the target application runs,
similar to`top` . Patch by Pablo Galindo
- gh-141615: Check `stdin` instead of`stdout` for`use_rawinput` in`pdb` .
- gh-69113: Fix `doctest` to correctly report line numbers for doctests
in`__test__` dictionary when formatted as triple-quoted strings by
finding unique lines in the string and matching them in the source file.
- gh-141600: Fix musl version detection on Void Linux.
- gh-48752: Add `readline.get_pre_input_hook()` function to retrieve the
current pre-input hook. This allows applications to save and restore the
hook without overwriting user settings. Patch by Sanyam Khurana.
- gh-141565: Add async-aware profiling to the Tachyon sampling profiler. The profiler now reconstructs and displays async task hierarchies in flamegraphs, making the output more actionable for users. Patch by Savannah Ostrowski and Pablo Galindo Salgado.
- gh-60107: Remove a copy from `io.RawIOBase.read()` . If the underlying
I/O class keeps a reference to the mutable memory, raise a`BufferError` .
- gh-116738: Make csv module thread-safe on the free threaded build.
- gh-140911: `collections` : Ensure that the methods`UserString.rindex()` and`UserString.index()` accept`collections.UserString` instances as the sub argument.
- gh-140875: Fix handling of unclosed character references (named and numerical) followed by the end of file in `html.parser.HTMLParser` with`convert_charrefs=False` .
- gh-140677: Add heatmap visualization mode to the Tachyon sampling profiler. The new `--heatmap` output format provides a line-by-line view
showing execution intensity with color-coded samples, inline statistics,
and interactive call graph navigation between callers and callees.
- gh-139946: Distinguish stdout and stderr when colorizing output in argparse module.
- gh-76007: `pydoc` : Fix`DeprecationWarning` being raised when
generating doc for stdlib modules.
- gh-138697: Fix inferring *dest* from a single-dash long option in`argparse` . If a short option and a single-dash long option are
passed to`add_argument()` ,*dest* is now inferred from the
single-dash long option.
- gh-138525: Add support for single-dash long options and alternate prefix characters in `argparse.BooleanOptionalAction` .
- gh-79986: Add parsing for `References` and`In-Reply-To` headers to
the`email` library that parses the header content as lists of
message id tokens.  This prevents them from being folded incorrectly.
- gh-135559: Flag: a `dir()` on a`Flag` enumeration now shows
non-canonical members. (i.e. aliases).
- gh-134453: Fixed `subprocess.Popen.communicate()``input=` handling
of`memoryview` instances that were non-byte shaped on POSIX
platforms. Those are now properly cast to a byte shaped view instead of
truncating the input.  Windows platforms did not have this bug.
- gh-127930: Add `__all__` to`tkinter.simpledialog` .
- gh-115952: Fix a potential memory denial of service in the `pickle` module. When reading a pickled data received from untrusted source, it
could cause an arbitrary amount of memory to be allocated, even if the
code that is allowed to execute is restricted by overriding the`find_class()` method. This could have led to
symptoms including a`MemoryError` , swapping, out of memory (OOM)
killed processes or containers, or even system crashes.
- bpo-40350: Fix support for namespace packages in `modulefinder` .

### DocumentationÂ¶

- gh-141994: `xml.sax.handler` : Make Documentation of`xml.sax.handler.feature_external_ges` warn of opening up to
external entity attacks. Patch by
Sebastian Pipping.

### Core and BuiltinsÂ¶

- gh-134584: Eliminate redundant refcounting from `_STORE_ATTR_INSTANCE_VALUE` .
- gh-142718: JIT: Fix segfault caused by not flushing the stack to memory at side exits.
- gh-142737: Tracebacks will be displayed in fallback mode even if `io.open()` is lost. Previously, this would crash the interpreter.
Patch by Bartosz SÅawecki.
- gh-116738: Make the attributes in `bz2` thread-safe on the
free threaded build.
- gh-134584: Eliminate redundant refcounting from `_CALL_LIST_APPEND` .
- gh-142554: Fix a crash in `divmod()` when`_pylong.int_divmod()` does not return a tuple of length two exactly. Patch by BÃ©nÃ©dikt Tran.
- gh-142531: Fix a free-threaded GC performance regression. If there are many untracked tuples, the GC will run too often, resulting in poor performance. The fix is to include untracked tuples in the âlong livedâ object count. The number of frozen objects is also now included since the free-threaded GC must scan those too.
- gh-142402: Fix reference counting when adjacent literal parts are merged while constructing `string.templatelib.Template` , preventing the
displaced string object from leaking.
- gh-116738: Make the attributes in `zlib` thread-safe on the
free threaded build.
- gh-142343: Fix SIGILL crash on m68k due to incorrect assembly constraint.
- gh-142236: Improve the âPerhaps you forgot a comma?â syntax error for multi-line string concatenations to point to the last string instead of the first, making it easier to locate where the comma is missing. Patch by Pablo Galindo.
- gh-142236: Fix incorrect keyword suggestions for syntax errors in `traceback` . The keyword typo suggestion mechanism would incorrectly
suggest replacements when the extracted source code was incomplete rather
than containing an actual typo. Patch by Pablo Galindo.
- gh-142305: Decrease the size of the generated stencils and the runtime JIT code. Patch by Diego Russo.
- gh-135379: Implement a limited form of register allocation known as âtop of stack cachingâ in the JIT. It works by keeping 0-3 of the top items in the stack in registers. The code generator generates multiple versions of those uops that do not escape and are relatively small. During JIT compilation, the copy that produces the least memory traffic is selected, spilling or reloading values when needed.
- gh-142276: Fix missing type watcher when promoting attribute loads to constants in the JIT. Patch by Ken Jin. Reproducer by Yuancheng Jiang.
- gh-142218: Fix crash when inserting into a split table dictionary with a non `str` key that matches an existing key.
- gh-141976: Check against abstract stack overflow in the JIT optimizer.
- gh-97850: Remove all `*.load_module()` usage and definitions from the
import system and importlib. The method has been deprecated in favor of`importlib.abc.Loader.exec_module()` since Python 3.4.
- gh-142048: Fix quadratically increasing garbage collection delays in free-threaded build.
- gh-65961: Stop setting `__cached__` on modules.
- gh-141770: Annotate anonymous mmap usage only when supported by the Linux kernel and if `-X dev` is used or Python is built in debug mode. Patch
by Donghee Na.
- gh-142029: Raise `ModuleNotFoundError` instead of crashing when a
nonexistent module is used as a name in`_imp.create_builtin()` .
- gh-142029: Raise `ValueError` instead of crashing when empty string
is used as a name in`_imp.create_builtin()` .
- gh-141976: Protect against specialization failures in the tracing JIT compiler for performance reasons.
- gh-141861: Fix invalid memory read in the `ENTER_EXECUTOR` instruction.
- gh-141930: When importing a module, use Pythonâs regular file object to ensure that writes to `.pyc` files are complete or an appropriate error
is raised.
- gh-138122: Add incomplete sample detection to prevent corrupted profiling data. Each thread state now contains an embedded base frame (sentinel at the bottom of the frame stack) with owner type `FRAME_OWNED_BY_INTERPRETER` . The profiler validates that stack
unwinding terminates at this sentinel frame. Samples that fail to reach
the base frame (due to race conditions, memory corruption, or other
errors) are now rejected rather than being included as spurious data.
- gh-120158: Fix inconsistent state when enabling or disabling monitoring events too many times.
- gh-140638: Expose a `"candidates"` stat in`gc.get_stats()` and`gc.callbacks` .
- gh-141780: Fix `Py_mod_gil` with API added in**PEP 793** :`PyModule_FromSlotsAndSpec()` and`PyModExport` hooks
- gh-141732: Ensure the `__repr__()` for`ExceptionGroup` and`BaseExceptionGroup` does not change when the exception sequence
that was original passed in to its constructor is subsequently mutated.
- gh-140638: Expose a `"duration"` stat in`gc.get_stats()` and`gc.callbacks` .
- gh-139653: Only raise a `RecursionError` or trigger a fatal error if the
stack pointer is both below the limit pointer*and* above the stack base.
If outside of these bounds assume that it is OK. This prevents false
positives when user-space threads swap stacks.
- gh-41779: Allowed defining the *__dict__* and*__weakref__* __slots__ for any class.
- gh-139103: Improve multithreaded scaling of dataclasses on the free-threaded build.
- gh-141589: Change `backoff counter` to use prime numbers instead of
powers of 2. Use only 3 bits for`counter` and 13 bits for`value` .
This allows to support values up to 8191. Patch by Mikhail Efimov.
- gh-137007: Fix a bug during JIT compilation failure which caused garbage collection debug assertions to fail.
- gh-132657: For the free-threaded build, avoid locking the `set` object for the`__contains__` method.
- gh-134584: Eliminate redundant refcounting from `_CALL_STR_1` .
- gh-134584: Eliminate redundant refcounting from `_CALL_BUILTIN_O` .
- gh-134584: Eliminate redundant refcounting from `_CALL_TUPLE_1` . Patch
by Noam Cohen

### C APIÂ¶

- gh-142589: Fix `PyUnstable_Object_IsUniqueReferencedTemporary()` handling of tagged ints on the interpreter stack.
- gh-142571: `PyUnstable_CopyPerfMapFile()` now checks that opening
the file succeeded before flushing.
- gh-142225: Fixed the `PyABIInfo_VAR` macro.
- gh-141049: `_PyObject_CallMethodId()` ,`_PyObject_GetAttrId()` and`_PyUnicode_FromId()` are
deprecated since 3.15 and will be removed in 3.20. Instead, use`PyUnicode_InternFromString()` and cache the result in the module
state, then call`PyObject_CallMethod()` or`PyObject_GetAttr()` . Patch by Victor Stinner.
- gh-142163: Fix the `HAVE_THREAD_LOCAL` macro being defined without the`Py_BUILD_CORE` macro set after including`Python.h` .
- gh-137422: Fix free threading race condition in `PyImport_AddModuleRef()` . It was previously possible for two calls
to the function return two different objects, only one of which was stored
in`sys.modules` .
- gh-141726: Add `PyDict_SetDefaultRef()` to the Stable ABI.
- gh-140042: Removed the sqlite3_shutdown call that could cause closing connections for sqlite when used with multiple sub interpreters.
- gh-141070: Add `PyUnstable_Object_Dump()` to dump an object to`stderr` . It should only be used for debugging. Patch by Victor Stinner.
- gh-139165: Expose the functions `Py_SIZE()` ,`Py_IS_TYPE()` and`Py_SET_SIZE()` in the Stable ABI.

### BuildÂ¶

- gh-131372: Add `LDVERSION` and`EXE` to the`base_interpreter` value
of`build-details.json` .
- gh-142454: When calculating the digest of the JIT stencils input, sort the hashed files by filenames before adding their content to the hasher. This ensures deterministic hash input and hence deterministic hash, independent on filesystem order.
- gh-131372: `build-details.py` will only be installed as part of the main
install (`make install` ).`make altinstall` will no longer include it.
- gh-142234: Allow `--enable-wasm-dynamic-linking` for WASI. While CPython
doesnât directly support it so external/downstream users do not have to
patch in support for the flag.
- gh-142050: Fixed a bug where JIT stencils produced on Windows contained debug data. Patch by Chris Eibl.
- gh-141808: Do not generate the jit stencils twice in case of PGO builds on Windows.
- gh-141926: `RUNSHARED` is no longer cleared when cross-compiling.
Previously,`RUNSHARED` was cleared when cross-compiling, which breaks
PGO when using`--enabled-shared` on systems where the cross-compiled
CPython is otherwise executable (e.g., via transparent emulation).
- gh-141808: When running `make clean-retain-profile` , keep the generated
JIT stencils. That way, the stencils are not generated twice when
Profile-guided optimization (PGO) is used. It also allows distributors to
supply their own pre-built JIT stencils.
- gh-141784: Fix `_remote_debugging_module.c` compilation on 32-bit Linux.
Include Python.h before system headers to make sure that`_remote_debugging_module.c` uses the same types (ABI) than Python.
Patch by Victor Stinner.
- gh-141172: Update to WASI SDK 29.
- gh-139707: Add configure option `--with-missing-stdlib-config=FILE` allows which distributors to
pass a JSON configuration file
containing custom error messages for missing standard library
modules.
- gh-108819: Honor `--with-platlibdir` in the pure-Python standard
library installation path, if`PLATLIBDIR` doesnât match the value used
in`LIBDIR` .

## Python 3.15.0 alpha 2Â¶

*Release date: 2025-11-18*

### WindowsÂ¶

- gh-140849: Update bundled liblzma to version 5.8.1.

### Tools/DemosÂ¶

### TestsÂ¶

- gh-140482: Preserve and restore the state of `stty echo` as part of the
test environment.
- gh-140082: Update `python -m test` to set`FORCE_COLOR=1` when being
run with color enabled so that`unittest` which is run by it with
redirected output will output in color.
- gh-136442: Use exitcode `1` instead of`5` if`unittest.TestCase.setUpClass()` raises an exception

### SecurityÂ¶

- gh-137836: Add support of the âplaintextâ element, RAWTEXT elements âxmpâ, âiframeâ, ânoembedâ and ânoframesâ, and optionally RAWTEXT element ânoscriptâ in `html.parser.HTMLParser` .
- gh-136063: `email.message` : ensure linear complexity for legacy HTTP
parameters parsing. Patch by BÃ©nÃ©dikt Tran.
- gh-136065: Fix quadratic complexity in `os.path.expandvars()` .

### LibraryÂ¶

- gh-141497: `ipaddress` : ensure that the methods`IPv4Network.hosts()` and`IPv6Network.hosts()` always return an
iterator.
- gh-140938: The `statistics.stdev()` and`statistics.pstdev()` functions now raise a`ValueError` when the input contains an
infinity or a NaN.
- gh-124111: Updated Tcl threading configuration in `_tkinter` to
assume that threads are always available in Tcl 9 and later.
- gh-137109: The `os.fork` and related forking APIs will no longer warn
in the common case where Linux or macOS platform APIs return the number of
threads in a process and find the answer to be 1 even when a`os.register_at_fork()``after_in_parent=` callback (re)starts a
thread.
- gh-141314: Fix assertion failure in `io.TextIOWrapper.tell()` when
reading files with standalone carriage return (`\r` ) line endings.
- gh-141311: Fix assertion failure in `io.BytesIO.readinto()` and
undefined behavior arising when read position is above capcity in`io.BytesIO` .
- gh-87710: `mimetypes` : Update mime type for`.ai` files to`application/pdf` .
- gh-85524: Update `io.FileIO.readall` , an implementation of`io.RawIOBase.readall()` , to follow`io.IOBase` guidelines and
raise`io.UnsupportedOperation` when a file is in âwâ mode rather
than`OSError`
- gh-141141: Fix a thread safety issue with `base64.b85decode()` .
Contributed by Benel Tayar.
- gh-141018: `mimetypes` : Update`.exe` ,`.dll` ,`.rtf` and (when`strict=False` )`.jpg` to their correct IANA mime type.
- gh-137969: Fix `annotationlib.ForwardRef.evaluate()` returning`ForwardRef` objects which donât update with new
globals.
- gh-75593: Add support of path-like objects and bytes-like objects in `wave.open()` .
- gh-140797: The undocumented `re.Scanner` class now forbids regular
expressions containing capturing groups in its lexicon patterns. Patterns
using capturing groups could previously lead to crashes with segmentation
fault. Use non-capturing groups (?:â¦) instead.
- gh-125115: Refactor the `pdb` parsing issue so positional arguments
can pass through intuitively.
- gh-140815: `faulthandler` now detects if a frame or a code object is
invalid or freed. Patch by Victor Stinner.
- gh-100218: Correctly set `errno` when`socket.if_nametoindex()` or`socket.if_indextoname()` raise an`OSError` . Patch by BÃ©nÃ©dikt Tran.
- gh-140734: `multiprocessing` : fix off-by-one error when checking the
length of a temporary socket file path. Patch by BÃ©nÃ©dikt Tran.
- gh-140873: Add support of non-descriptor callables in `functools.singledispatchmethod()` .
- gh-140874: Bump the version of pip bundled in ensurepip to version 25.3
- gh-140826: Now `winreg.HKEYType` objects are compared by their
underlying Windows registry handle value instead of their object identity.
- gh-140808: The internal class `mailbox._ProxyFile` is no longer a
parameterized generic.
- gh-140691: In `urllib.request` , when opening a FTP URL fails because
a data connection cannot be made, the control connectionâs socket is now
closed to avoid a`ResourceWarning` .
- gh-103847: Fix hang when cancelling process created by `asyncio.create_subprocess_exec()` or`asyncio.create_subprocess_shell()` . Patch by Kumar Aditya.
- gh-137821: Convert `_json` module to use Argument Clinic. Patched by
Yoonho Hann.
- gh-140790: Initialize all Pdbâs instance variables in `__init__` ,
remove some hasattr/getattr
- gh-140766: Add `enum.show_flag_values()` and`enum.bin` to`enum.__all__` .
- gh-120057: Add `os.reload_environ()` to`os.__all__` .
- gh-140741: Fix `profiling.sampling.sample()` incorrectly handling a`FileNotFoundError` or`PermissionError` .
- gh-140228: Avoid making unnecessary filesystem calls for frozen modules in `linecache` when the global module cache is not present.
- gh-139946: Error and warning keywords in `argparse.ArgumentParser` messages are now colorized when color output is enabled, fixing a visual
inconsistency in which they remained plain text while other output was
colorized.
- gh-140590: Fix arguments checking for the `functools.partial.__setstate__()` that may lead to internal state
corruption and crash. Patch by Sergey Miryanov.
- gh-125434: Display thread name in `faulthandler` on Windows. Patch by
Victor Stinner.
- gh-140634: Fix a reference counting bug in `os.sched_param.__reduce__()` .
- gh-140650: Fix an issue where closing `io.BufferedWriter` could
crash if the closed attribute raised an exception on access or could not
be converted to a boolean.
- gh-140633: Ignore `AttributeError` when setting a moduleâs`__file__` attribute when loading an extension module packaged as Apple
Framework.
- gh-140601: `xml.etree.ElementTree.iterparse()` now emits a`ResourceWarning` when the iterator is not explicitly closed and was
opened with a filename. This helps developers identify and fix resource
leaks. Patch by Osama Abdelkader.
- gh-140593: `xml.parsers.expat` : Fix a memory leak that could affect
users with`ElementDeclHandler()` set to
a custom element declaration handler. Patch by Sebastian Pipping.
- gh-140607: Inside `io.RawIOBase.read()` , validate that the count of
bytes returned by`io.RawIOBase.readinto()` is valid (inside the
provided buffer).
- gh-138162: Fix `logging.LoggerAdapter` with`merge_extra=True` and without the*extra* argument.
- gh-140481: Improve error message when trying to iterate a Tk widget, image or font.
- gh-138774: `ast.unparse()` now generates full source code when
handling`ast.Interpolation` nodes that do not have a specified
source.
- gh-140474: Fix memory leak in `array.array` when creating arrays
from an empty`str` and the`u` type code.
- gh-140448: Change the default of `suggest_on_error` to`True` in`argparse.ArgumentParser` .
- gh-137530: `dataclasses` Fix annotations for generated`__init__` methods by replacing the annotations that were in-line in the generated
source code with`__annotate__` functions attached to the methods.
- gh-140348: Fix regression in Python 3.14.0 where using the `|` operator
on a`typing.Union` object combined with an object that is not a
type would raise an error.
- gh-76007: `decimal` : Deprecate`__version__` and replace with`decimal.SPEC_VERSION` .
- gh-76007: Deprecate `__version__` from`imaplib` . Patch by Hugo van
Kemenade.
- gh-140272: Fix memory leak in the `clear()` method of the`dbm.gnu` database.
- gh-129117: `unicodedata` : Add`isxidstart()` and`isxidcontinue()` functions to check whether a character
can start or continue a Unicode Standard Annex #31 identifier.
- gh-140251: Colorize the default import statement `import asyncio` in
asyncio REPL.
- gh-140212: Calendarâs HTML formatting now accepts year and month as options. Previously, running `python -m calendar -t html 2025 10` would
result in an error message. It now generates an HTML document displaying
the calendar for the specified month. Contributed by PÃ¥l GrÃ¸nÃ¥s Drange.
- gh-135801: Improve filtering by module in `warnings.warn_explicit()` if no*module* argument is passed. It now tests the module regular
expression in the warnings filter not only against the filename with`.py` stripped, but also against module names constructed starting from
different parent directories of the filename (with`/__init__.py` ,`.py` and, on Windows,`.pyw` stripped).
- gh-139707: Improve `ModuleNotFoundError` error message when a
standard library module is missing.
- gh-140041: Fix import of `ctypes` on Android and Cygwin when ABI
flags are present.
- gh-140120: Fixed a memory leak in `hmac` when it was using the
hacl-star backend. Discovered by`@ashm-dev` using AddressSanitizer.
- gh-140141: The `importlib.metadata.PackageNotFoundError` traceback raised when`importlib.metadata.Distribution.from_name` cannot
discover a distribution no longer includes a transient`StopIteration` exception trace.Contributed by Bartosz SÅawecki in gh-140142.
- gh-140166: `mimetypes` : Per the IANA assignment,
update the MIME type for the`.texi` and`.texinfo` file formats to`application/texinfo` , instead of`application/x-texinfo` .
- gh-140135: Speed up `io.RawIOBase.readall()` by using PyBytesWriter
API (about 4x faster)
- gh-76007: `zlib` : Deprecate`__version__` and schedule for removal
in Python 3.20.
- gh-136702: `encodings` : Deprecate passing a non-ascii*encoding* name
to`encodings.normalize_encoding()` and schedule removal of support
for Python 3.17.
- gh-139940: Print clearer error message when using `pdb` to attach to a
non-existing process.
- gh-139462: When a child process in a `concurrent.futures.ProcessPoolExecutor` terminates abruptly, the
resulting traceback will now tell you the PID and exit code of the
terminated process. Contributed by Jonathan Berg.
- gh-63161: Fix `tokenize.detect_encoding()` . Support non-UTF-8 shebang
and comments if non-UTF-8 encoding is specified. Detect decoding error for
non-UTF-8 encoding. Detect null bytes in source code.
- gh-101828: Fix `'shift_jisx0213'` ,`'shift_jis_2004'` ,`'euc_jisx0213'` and`'euc_jis_2004'` codecs truncating null chars as
they were treated as part of multi-character sequences.
- gh-139246: fix: paste zero-width in default repl width is wrong.
- gh-83714: Implement `os.statx()` on Linux kernel versions 4.11 and
later with glibc versions 2.28 and later.  Contributed by Jeffrey Bosboom
and Victor Stinner.
- gh-138891: Fix `SyntaxError` when```
inspect.get_annotations(f,
eval_str=True)
```
is called on a function annotated with a**PEP 646**`star_expression`
- gh-138859: Fix generic type parameterization raising a `TypeError` when omitting a`ParamSpec` that has a default which is not a list
of types.
- gh-138764: Prevent `annotationlib.call_annotate_function()` from
calling`__annotate__` functions that donât support`VALUE_WITH_FAKE_GLOBALS` in a fake globals namespace with empty
globals.Make `FORWARDREF` and`STRING` annotations fall back to using`VALUE` annotations in the case that neither their own format, nor`VALUE_WITH_FAKE_GLOBALS` are supported.
- gh-138775: Use of `python -m` with`base64` has been fixed to
detect input from a terminal so that it properly notices EOF.
- gh-98896: Fix a failure in multiprocessing resource_tracker when SharedMemory names contain colons. Patch by Rani Pinchuk.
- gh-138425: Fix partial evaluation of `annotationlib.ForwardRef` objects which rely on names defined as globals.
- gh-138151: In `annotationlib` , improve evaluation of forward
references to nonlocal variables that are not yet defined when the
annotations are initially evaluated.
- gh-69528: The `mode` attribute of files opened in the`'wb+'` mode is now`'wb+'` instead of`'rb+'` .
- gh-137627: Speed up `csv.Sniffer.sniff()` delimiter detection by up to
1.6x.
- gh-55531: `encodings` : Improve`normalize_encoding()` performance by implementing the function in C using the private`_Py_normalize_encoding` which has been modified to make lowercase
conversion optional.
- gh-136057: Fixed the bug in `pdb` and`bdb` where`next` and`step` canât go over the line if a loop exists in the line.
- gh-133390: Support table, index, trigger, view, column, function, and schema completion for `sqlite3` âs command-line interface.
- gh-135307: `email` : Fix exception in`set_content()` when encoding
text and max_line_length is set to`0` or`None` (unlimited).
- gh-133789: Fix unpickling of `pathlib` objects that were pickled in
Python 3.13.
- gh-133601: Remove deprecated `typing.no_type_check_decorator()` .
- gh-132686: Add parameters *inherit_class_doc* and*fallback_to_class_doc* for`inspect.getdoc()` .
- gh-131116: `inspect.getdoc()` now correctly returns an inherited
docstring on`cached_property` objects if none is given
in a subclass.
- gh-130693: Add support for `-nolinestop` , and`-strictlimits` options
to`tkinter.Text.search()` . Also add the`tkinter.Text.search_all()` method for`-all` and`-overlap` options.
- gh-122255: In the `linecache` module and in the Python implementation
of the`warnings` module, a`DeprecationWarning` is issued when`mod.__loader__` differs from`mod.__spec__.loader` (like in the C
implementation of the`warnings` module).
- gh-121011: `math.log()` now supports arbitrary large integer-like
arguments in the same way as arbitrary large integer arguments.
- gh-119668: Publicly expose and document `importlib.machinery.NamespacePath` .
- gh-102431: Clarify constraints for âlogicalâ arguments in methods of `decimal.Context` .
- gh-81313: Add the `math.integer` module (**PEP 791** ).

### Core and BuiltinsÂ¶

- gh-141579: Fix `sys.activate_stack_trampoline()` to properly support
the`perf_jit` backend. Patch by Pablo Galindo.
- gh-114203: Skip locking if object is already locked by two-mutex critical section.
- gh-141528: Suggest using `concurrent.interpreters.Interpreter.close()` instead of the private`_interpreters.destroy` function when warning
about remaining subinterpreters. Patch by Sergey Miryanov.
- gh-141367: Specialize `CALL_LIST_APPEND` instruction only for lists, not
for list subclasses, to avoid unnecessary deopt. Patch by Mikhail Efimov.
- gh-141312: Fix the assertion failure in the `__setstate__` method of the
range iterator when a non-integer argument is passed. Patch by Sergey
Miryanov.
- gh-140643: Add support for `<GC>` and`<native>` frames to`profiling.sampling` output to denote active garbage collection and
calls to native code.

### LibraryÂ¶

### Core and BuiltinsÂ¶

- gh-140479: Update JIT compilation to use LLVM 21 at build time.
- gh-140939: Fix memory leak when `bytearray` or`bytes` is
formated with the`%*b` format with a large width that results in a`MemoryError` .

### LibraryÂ¶

### Core and BuiltinsÂ¶

- gh-140530: Fix a reference leak when `raise exc from cause` fails. Patch
by BÃ©nÃ©dikt Tran.

### LibraryÂ¶

- gh-90344: Replace `io.IncrementalNewlineDecoder` with non
incremental newline decoders in codebase where`io.IncrementalNewlineDecoder.decode()` was being called once.

### Core and BuiltinsÂ¶

- gh-140373: Correctly emit `PY_UNWIND` event when generator object is
closed. Patch by Mikhail Efimov.
- gh-140729: Fix pickling error in the sampling profiler when using `concurrent.futures.ProcessPoolExecutor` script can not be properly
pickled and executed in worker processes.
- gh-131527: Dynamic borrow checking for stackrefs is added to `Py_STACKREF_DEBUG` mode. Patch by Mikhail Efimov.
- gh-140576: Fixed crash in `tokenize.generate_tokens()` in case of
specific incorrect input. Patch by Mikhail Efimov.
- gh-140544: Speed up accessing interpreter state by caching it in a thread local variable. Patch by Kumar Aditya.
- gh-140551: Fixed crash in `dict` if`dict.clear()` is called at
the lookup stage. Patch by Mikhail Efimov and Inada Naoki.
- gh-140517: Fixed a reference leak when iterating over the result of `map()` with`strict=True` when the input iterables have different
lengths. Patch by Mikhail Efimov.
- gh-133467: Fix race when updating `type.__bases__` that could allow
a read of`type.__base__` to observe an inconsistent value on the
free threaded build.
- gh-140471: Fix potential buffer overflow in `ast.AST` node
initialization when encountering malformed`_fields` containing non-`str` .

### LibraryÂ¶

- gh-140443: The logarithm functions (such as `math.log10()` and`math.log()` ) may now produce slightly different results for extremely
large integers that cannot be converted to floats without overflow. These
results are generally more accurate, with reduced worst-case error and a
tighter overall error distribution.

### Core and BuiltinsÂ¶

- gh-140431: Fix a crash in Pythonâs garbage collector due to partially initialized coroutine objects when coroutine origin tracking depth is enabled ( `sys.set_coroutine_origin_tracking_depth()` ).
- gh-140476: Optimize `PySet_Add()` for`frozenset` in
free threaded build.
- gh-135904: Add special labels to the assembly created during stencil creation to support relocations that the native object file format does not support. Specifically, 19 bit branches for AArch64 in Mach-O object files.

### LibraryÂ¶

- gh-140398: Fix memory leaks in `readline` functions`read_init_file()` ,`read_history_file()` ,`write_history_file()` , and`append_history_file()` when`PySys_Audit()` fails.

### Core and BuiltinsÂ¶

- gh-140406: Fix memory leak when an objectâs `__hash__()` method returns an object that isnât an`int` .
- gh-140358: Restore elapsed time and unreachable object count in GC debug output. These were inadvertently removed during a refactor of `gc.c` .
The debug log now again reports elapsed collection time and the number of
unreachable objects. Contributed by PÃ¥l GrÃ¸nÃ¥s Drange.
- gh-136895: Update JIT compilation to use LLVM 20 at build time.
- gh-139109: A new tracing frontend for the JIT compiler has been implemented. Patch by Ken Jin. Design for CPython by Ken Jin, Mark Shannon and Brandt Bucher.
- gh-140306: Fix memory leaks in cross-interpreter channel operations and shared namespace handling.
- gh-116738: Make _suggestions module thread-safe on the free threaded build.
- gh-140301: Fix memory leak of `PyConfig` in subinterpreters.
- gh-140257: Fix data race between interpreter_clear() and take_gil() on eval_breaker during finalization with daemon threads.
- gh-139951: Fixes a regression in GC performance for a growing heap composed mostly of small tuples. 
  - Counts number of actually tracked objects, instead of trackable objects. This ensures that untracking tuples has the desired effect of reducing GC overhead.
  - Does not track most untrackable tuples during creation. This prevents large numbers of small tuples causing excessive GCs.
- gh-140253: Wrong placement of a double-star pattern inside a mapping pattern now throws a specialized syntax error. Contributed by Bartosz SÅawecki in gh-140253.
- gh-140104: Fix a bug with exception handling in the JIT. Patch by Ken Jin. Bug reported by Daniel Diniz.
- gh-140149: Speed up parsing bytes literals concatenation by using PyBytesWriter API and a single memory allocation (about 3x faster).
- gh-140061: Fixing the checking of whether an object is uniquely referenced to ensure free-threaded compatibility. Patch by Sergey Miryanov.
- gh-140080: Fix hang during finalization when attempting to call `atexit` handlers under no memory.
- gh-139871: Update `bytearray` to use a`bytes` under the
hood as its buffer and add`bytearray.take_bytes()` to take it out.
- gh-140067: Fix memory leak in sub-interpreter creation.
- gh-139914: Restore support for HP PA-RISC, which has an upwards-growing stack.
- gh-139817: Attribute `__qualname__` is added to`typing.TypeAliasType` . Patch by Mikhail Efimov.
- gh-135801: Many functions related to compiling or parsing Python code, such as `compile()` ,`ast.parse()` ,`symtable.symtable()` , and`importlib.abc.InspectLoader.source_to_code()` now allow to specify
the module name. It is needed to unambiguous filter syntax warnings by module name.
- gh-139640: `ast.parse()` no longer emits syntax warnings for`return` /`break` /`continue` in`finally` (see**PEP 765** ) â they
are only emitted during compilation.
- gh-139640: Fix swallowing some syntax warnings in different modules if they accidentally have the same message and are emitted from the same line. Fix duplicated warnings in the `finally` block.
- gh-139475: Changes in stackref debugging mode when `Py_STACKREF_DEBUG` is set. We use the same pattern of refcounting for stackrefs as in
production build.
- gh-139269: Fix undefined behavior when using unaligned store in JITâs `patch_*` functions.
- gh-138944: Fix `SyntaxError` message when invalid syntax appears on
the same line as a valid`import ... as ...` or```
from ... import ... as
...
```
statement. Patch by Brian Schubert.
- gh-138857: Improve `SyntaxError` message for`case` keyword placed
outside`match` body.
- gh-131253: Support the `--enable-pystats` build option for the
free-threaded build.
- gh-136327: Errors when calling functions with invalid values after `*` and`**` now do not include the function name. Patch by Ilia Solin.
- gh-134786: If `Py_TPFLAGS_MANAGED_DICT` and`Py_TPFLAGS_MANAGED_WEAKREF` are used, then`Py_TPFLAGS_HAVE_GC` must be used as well.

### C APIÂ¶

- gh-141341: On Windows, rename the `COMPILER` macro to`_Py_COMPILER` to avoid name conflicts. Patch by Victor Stinner.
- gh-116146: Add a new `PyImport_CreateModuleFromInitfunc()` C-API for
creating a module from a*spec* and*initfunc* . Patch by Itamar Oren.
- gh-141042: Make qNaN in `PyFloat_Pack2()` and`PyFloat_Pack4()` , if while conversion to a narrower precision
floating-point format â the remaining after truncation payload will be
zero.  Patch by Sergey B Kirpichev.
- gh-141004: `Py_MATH_El` and`Py_MATH_PIl` are
deprecated.
- gh-141004: The `Py_INFINITY` macro is soft deprecated.
- gh-140556: **PEP 793** : Add a new entry point for C extension modules,`PyModExport_<modulename>` .
- gh-140487: Fix `Py_RETURN_NOTIMPLEMENTED` in limited C API 3.11
and older: donât treat`Py_NotImplemented` as immortal. Patch by Victor
Stinner.
- gh-140153: Fix `Py_REFCNT()` definition on limited C API 3.11-3.13.
Patch by Victor Stinner.
- gh-139653: Add `PyUnstable_ThreadState_SetStackProtection()` and`PyUnstable_ThreadState_ResetStackProtection()` functions to set the
stack protection base address and stack protection size of a Python thread
state. Patch by Victor Stinner.

### BuildÂ¶

- gh-140454: When building the JIT, match the jit_stencils filename expectations in Makefile with the generator script. This avoid needless JIT recompilation during `make install` .
- gh-140768: Warn when the WASI SDK version doesnât match whatâs supported.
- gh-140513: Generate a clear compilation error when `_Py_TAIL_CALL_INTERP` is enabled but either`preserve_none` or`musttail` is not supported.
- gh-140475: Support WASI SDK 25.
- gh-140239: Check `statx` availability only on Linux (including Android).
- gh-140189: iOS builds were added to CI.
- gh-137618: `PYTHON_FOR_REGEN` now requires Python 3.10 to Python 3.15.
Patch by Adam Turner.

## Python 3.15.0 alpha 1Â¶

*Release date: 2025-10-14*

### macOSÂ¶

- gh-115119: Update macOS installer to use libmpdecimal 4.0.1.
- gh-124111: Update macOS installer to use Tcl/Tk 9.0.2.
- gh-132339: Update macOS installer version of OpenSSL to 3.5.4.
- gh-137450: macOS installer shell path management improvements: separate the installer `Shell profile updater` postinstall script from the`Update Shell Profile.command` to enable more robust error handling.
- gh-137134: Update macOS installer to ship with SQLite version 3.50.4.

### WindowsÂ¶

- gh-139810: Installing with `py install 3[.x]-dev` will now select final
versions as well as prereleases.
- gh-139573: Updated bundled version of OpenSSL to 3.0.18.
- gh-138896: Fix error installing C runtime on non-updated Windows machines
- gh-138314: Add `winreg.DeleteTree()` .
- gh-137136: Suppress build warnings when build on Windows with `--experimental-jit-interpreter` .
- gh-137134: Update Windows installer to ship with SQLite 3.50.4.
- gh-135099: Fix a crash that could occur on Windows when a background thread waits on a `PyMutex` while the main thread is shutting down
the interpreter.
- gh-130727: Fix a race in internal calls into WMI that can result in an âinvalid handleâ exception under high load. Patch by Chris Eibl.
- gh-76023: Make `os.path.realpath()` ignore Windows error 1005 when in
non-strict mode.
- gh-133779: Reverts the change to generate different `pyconfig.h` files based on compiler settings, as it was frequently causing extension
builds to break. In particular, the`Py_GIL_DISABLED` preprocessor
variable must now always be defined explicitly when compiling for the
experimental free-threaded runtime. The`sysconfig.get_config_var()` function can be used to determine whether the current runtime was compiled
with that flag or not.
- gh-133626: Ensures packages are not accidentally bundled into the traditional installer.
- gh-133580: Fix `sys.getwindowsversion()` failing without setting an
exception when called on some WinAPI partitions.
- gh-133572: Avoid LsaNtStatus to WinError conversion on unsupported WinAPI partitions.
- gh-133568: Fix compile error when using a WinAPI partition that doesnât support the RPC runtime library.
- gh-133562: Disable handling of security descriptors by `os.mkdir()` with mode`0o700` on WinAPI partitions that do not support it. This only
affects custom builds for specialized targets.
- gh-133537: Avoid using console I/O in WinAPI partitions that donât support it
- gh-131942: Use the Python-specific `Py_DEBUG` macro rather than`_DEBUG` in Windows-related C code. Patch by Xuehai Pan.

### Tools/DemosÂ¶

- gh-139330: SBOM generation tool didnât cross-check the version and checksum values against the `Modules/expat/refresh.sh` script, leading
to the values becoming out-of-date during routine updates.
- gh-132006: XCframeworks now include privacy manifests to satisfy Apple App Store submission requirements.
- gh-138171: A script for building an iOS XCframework was added. As part of this change, the top level `iOS` folder has been moved to be a
subdirectory of the`Apple` folder.
- gh-137873: The iOS test runner has been simplified, resolving some issues that have been observed using the runner in GitHub Actions and Azure Pipelines test environments.
- gh-137484: Have `Tools/wasm/wasi` put the build Python into a directory
named after the build triple instead of âbuildâ.
- gh-137025: The `wasm_build.py` script has been removed.`Tools/wasm/emscripten` and`Tools/wasm/wasi` should be used instead,
as described in the Dev Guide.
- gh-137248: Add a `--logdir` option to`Tools/wasm/wasi` for specifying
where to write log files.
- gh-137243: Have Tools/wasm/wasi detect a WASI SDK install in /opt when it was directly extracted from a release tarball.
- gh-136251: Fixes and usability improvements for `Tools/wasm/emscripten/web_example`
- gh-135968: Stubs for `strip` are now provided as part of an iOS install.
- gh-135379: The cases generator no longer accepts type annotations on stack items. Conversions to non-default types are now done explicitly in bytecodes.c and optimizer_bytecodes.c. This will simplify code generation for top-of-stack caching and other future features.
- gh-134215: REPL import autocomplete only suggests private modules when explicitly specified.

### TestsÂ¶

- gh-139208: Fix regrtest `--fast-ci --verbose` : donât ignore the`--verbose` option anymore. Patch by Victor Stinner.
- gh-138313: Restore skipped test and add janky workaround to prevent select buildbots from failing with a ResourceWarning.
- gh-135966: The iOS testbed now handles the `app_packages` folder as a
site directory.
- gh-135494: Fix regrtest to support excluding tests from `--pgo` tests.
Patch by Victor Stinner.
- gh-132815: Fix test__opcode: add `JUMP_BACKWARD` to specialization
stats.
- gh-135489: Show verbose output for failing tests during PGO profiling step with âenable-optimizations.
- gh-135401: Add a new GitHub CI job to test the `ssl` module with
AWS-LC as the backing cryptography and
TLS library.
- gh-135120: Add `test.support.subTests()` .
- gh-134567: Expose log formatter to users in TestCase.assertLogs. `unittest.TestCase.assertLogs()` will now optionally accept a
formatter that will be used to format the strings in output if provided.
- gh-133744: Fix multiprocessing interrupt test. Add an event to synchronize the parent process with the child process: wait until the child process starts sleeping. Patch by Victor Stinner.
- gh-133682: Fixed test case `test.test_annotationlib.TestStringFormat.test_displays` which ensures
proper handling of complex data structures (lists, sets, dictionaries, and
tuples) in string annotations.
- gh-133639: Fix `TestPyReplAutoindent.test_auto_indent_default()` doesnât
run`input_code` .

### SecurityÂ¶

- gh-139700: Check consistency of the zip64 end of central directory record. Support records with âzip64 extensible dataâ if there are no bytes prepended to the ZIP file.
- gh-139400: `xml.parsers.expat` : Make sure that parent Expat parsers
are only garbage-collected once they are no longer referenced by
subparsers created by`ExternalEntityParserCreate()` . Patch by
Sebastian Pipping.
- gh-139283: `sqlite3` : correctly handle maximum number of rows to
fetch in`Cursor.fetchmany` and reject
negative values for`Cursor.arraysize` .
Patch by BÃ©nÃ©dikt Tran.
- gh-136053: `marshal` : fix a possible crash when deserializing`slice` objects.
- gh-135661: Fix parsing start and end tags in `html.parser.HTMLParser` according to the HTML5 standard.
  - Whitespaces no longer accepted between `</` and the tag name.
E.g.`</ script>` does not end the script section.
  - Vertical tabulation ( `\v` ) and non-ASCII whitespaces no longer recognized
as whitespaces. The only whitespaces are`\t\n\r\f` and space.
  - Null character (U+0000) no longer ends the tag name.
  - Attributes and slashes after the tag name in end tags are now ignored, instead of terminating after the first `>` in quoted attribute value.
E.g.`</script/foo=">"/>` .
  - Multiple slashes and whitespaces between the last attribute and closing `>` are now ignored in both start and end tags. E.g.`<a foo=bar/ //>` .
  - Multiple `=` between attribute name and value are no longer collapsed.
E.g.`<a foo==bar>` produces attribute âfooâ with value â=barâ.
- gh-135661: Fix CDATA section parsing in `html.parser.HTMLParser` according to the HTML5 standard:`] ]>` and`]] >` no longer end the
CDATA section. Add private method`_set_support_cdata()` which can be
used to specify how to parse`<[CDATA[` â as a CDATA section in
foreign content (SVG or MathML) or as a bogus comment in the HTML
namespace.
- gh-102555: Fix comment parsing in `html.parser.HTMLParser` according to the HTML5 standard.`--!>` now ends the comment.`-- >` no longer ends the comment. Support abnormally ended empty comments`<-->` and`<--->` .
- gh-135462: Fix quadratic complexity in processing specially crafted input in `html.parser.HTMLParser` . End-of-file errors are now handled
according to the HTML5 specs â comments and declarations are
automatically closed, tags are ignored.
- gh-118350: Fix support of escapable raw text mode (elements âtextareaâ and âtitleâ) in `html.parser.HTMLParser` .
- gh-135034: Fixes multiple issues that allowed `tarfile` extraction
filters (`filter="data"` and`filter="tar"` ) to be bypassed using
crafted symlinks and hard links.Addresses **CVE 2024-12718** ,**CVE 2025-4138** ,**CVE 2025-4330** , and**CVE 2025-4517** .
- gh-133767: Fix use-after-free in the âunicode-escapeâ decoder with a non-âstrictâ error handler.
- gh-133623: Indicate through `ssl.HAS_PSK_TLS13` whether the`ssl` module supports âExternal PSKsâ in TLSv1.3, as described in RFC
9258. Patch by Will Childs-Klein.
- gh-128840: Short-circuit the processing of long IPv6 addresses early in `ipaddress` to prevent excessive memory consumption and a minor
denial-of-service.

### LibraryÂ¶

- gh-139482: Optimize `os.environ.clear()` by calling*clearenv(3)* when this function is available. Patch by Victor
Stinner.
- gh-139958: The `application/toml` mime type is now supported by`mimetypes` . Patch by Gil Forcada.
- gh-139823: `ensurepip` now fails with a nicer error message when the`zlib` module is not available.
- gh-139905: Add suggestion to error message for `typing.Generic` subclasses when`cls.__parameters__` is missing due to a parent class
failing to call`super().__init_subclass__()` in its`__init_subclass__` .
- gh-139894: Fix incorrect sharing of current task with the child process while forking in `asyncio` . Patch by Kumar Aditya.
- gh-139845: Fix to not print KeyboardInterrupt twice in default asyncio REPL.
- gh-139783: Fix `inspect.getsourcelines()` for the case when a
decorator is followed by a comment or an empty line.
- gh-139809: Prevent premature colorization of subparser `prog` in`argparse.ArgumentParser.add_subparsers()` to respect color
environment variable changes after parser creation.
- gh-139736: Fix excessive indentation in the default `argparse``HelpFormatter` . Patch by Alexander Edland.
- gh-70765: `http.server` : fix default handling of HTTP/0.9 requests in`BaseHTTPRequestHandler` . Previously,`BaseHTTPRequestHandler.parse_request()` incorrectly waited for
headers in the request although those are not supported in HTTP/0.9. Patch
by BÃ©nÃ©dikt Tran.
- gh-139322: Fix `os.getlogin()` error handling: fix the error number.
Patch by Victor Stinner.
- gh-135953: Add a Gecko format output to the tachyon profiler via `--gecko` .
- gh-139184: `os.forkpty()` does now make the returned file descriptor
non-inheritable.
- gh-139391: Fix an issue when, on non-Windows platforms, it was not possible to gracefully exit a `python -m asyncio` process suspended by
Ctrl+Z and later resumed by*fg* other than with*kill* .
- gh-139374: `timeit` : Add color to error tracebacks.
- gh-90949: Add `SetBillionLaughsAttackProtectionActivationThreshold()` and`SetBillionLaughsAttackProtectionMaximumAmplification()` to xmlparser objects to tune protections
against billion laughs attacks. Patch by
BÃ©nÃ©dikt Tran.
- gh-139312: Upgrade bundled libexpat to 2.7.3
- gh-139289: Do a real lazy-import on `rlcompleter` in`pdb` and
restore the existing completer after importing`rlcompleter` .
- gh-90949: Add `SetAllocTrackerActivationThreshold()` and`SetAllocTrackerMaximumAmplification()` to xmlparser objects to tune protections
against disproportional amounts of dynamic memory usage from within an
Expat parser. Patch by BÃ©nÃ©dikt Tran.
- gh-67795: Functions that take timestamp or timeout arguments now accept any real numbers (such as `Decimal` and`Fraction` ), not only integers or floats, although this
does not improve precision.
- gh-95953: A CSS class, `diff_changed` , was added to the changed lines in
the`make_table` output of`difflib.HtmlDiff` . Patch by Katie
Gardner.
- gh-139210: Fix use-after-free when reporting unknown event in `xml.etree.ElementTree.iterparse()` . Patch by Ken Jin.
- gh-138860: Lazy import `rlcompleter` in`pdb` to avoid deadlock
in subprocess.
- gh-112729: Fix crash when calling `concurrent.interpreters.create()` when the process is out of memory.
- gh-126016: Fix an assertion failure when sending `KeyboardInterrupt` to a Python process running a subinterpreter in a separate thread.
- gh-118803: `collections.abc.ByteString` has been removed from`collections.abc.__all__` , and`typing.ByteString` has been
removed from`typing.__all__` . The former has been deprecated since
Python 3.12, and the latter has been deprecated since Python 3.9. Both
classes are scheduled for removal in Python 3.17.Additionally, the following statements now cause `DeprecationWarning` s
to be emitted at runtime:`from collections.abc import ByteString` ,`from typing import ByteString` ,```
import collections.abc;
collections.abc.ByteString
```
and`import typing; typing.ByteString` .
Both classes already caused`DeprecationWarning` s to be emitted if
they were subclassed or used as the second argument to`isinstance()` or`issubclass()` , but they did not previously lead to`DeprecationWarning` s if they were merely imported or accessed from
their respective modules.
- gh-135729: Fix unraisable exception during finalization when using `concurrent.interpreters` in the REPL.
- gh-139076: Fix a bug in the `pydoc` module that was hiding functions
in a Python module if they were implemented in an extension module and the
module did not have`__all__` .
- gh-139090: Add `os.RWF_DONTCACHE` constant for Linux 6.14+.
- gh-139065: Fix trailing space before a wrapped long word if the line length is exactly *width* in`textwrap` .
- gh-139001: Fix race condition in `pathlib.Path` on the internal`_raw_paths` field.
- gh-138813: `multiprocessing.BaseProcess` defaults`kwargs` to`None` instead of a shared dictionary.
- gh-138998: Update bundled libexpat to 2.7.2
- gh-138993: Dedent `credits` text.
- gh-118803: Add back `collections.abc.ByteString` and`typing.ByteString` . Both had been removed in prior alpha, beta and
release candidates for Python 3.14, but their removal has now been
postponed to Python 3.17.
- gh-130567: Fix possible crash in `locale.strxfrm()` due to a platform
bug on macOS.
- gh-137226: Fix `typing.get_type_hints()` calls on generic`typing.TypedDict` classes defined with string annotations.
- gh-138899: Executing `quit` command in`pdb` will raise`bdb.BdbQuit` when`pdb` is started from an asyncio console
using`breakpoint()` or`pdb.set_trace()` .
- gh-138804: Raise `TypeError` instead of`AttributeError` when an
argument of incorrect type is passed to`shlex.quote()` . This restores
the behavior of the function prior to 3.14.
- gh-138779: Support device numbers larger than `2**63-1` for the`st_rdev` field of the`os.stat_result` structure.
- gh-138682: Added symmetric difference support to `collections.Counter` objects.
- gh-138712: Add `os.NODEV` .
- gh-128636: Fix crash in PyREPL when os.environ is overwritten with an invalid value for mac
- gh-138720: Fix an issue where `io.BufferedWriter` and`io.BufferedRandom` had different definitions of âclosedâ for`close()` and`flush()` which resulted in an
exception when close called flush but flush thought the file was already
closed.
- gh-138706: Update `unicodedata` database to Unicode 17.0.0.
- gh-76007: Deprecate `__version__` from a number of standard library
modules. Patch by Hugo van Kemenade.
- gh-138535: Speed up `os.stat()` for files with reasonable timestamps.
Contributed by Jeffrey Bosboom.
- gh-116946: `curses.panel` : the type of`curses.panel.new_panel()` is now immutable. Patch by BÃ©nÃ©dikt Tran.
- gh-116946: `zlib` : the types of`zlib.compressobj()` and`zlib.decompressobj()` are now immutable. Patch by BÃ©nÃ©dikt Tran.
- gh-116946: `os` : the`os.DirEntry` type and the type of`os.scandir()` are now immutable. Patch by BÃ©nÃ©dikt Tran.
- gh-116946: `tkinter` : the types`_tkinter.Tcl_Obj` (wrapper
for Tcl objects),`_tkinter.tktimertoken` (obtained by calling`createtimerhandler()` on a`Tk` application) and`_tkinter.tkapp` (the runtime type of Tk applications) are now
immutable. Patch by BÃ©nÃ©dikt Tran.
- gh-138514: Raise `ValueError` when a multi-character string is passed
to the*echo_char* parameter of`getpass.getpass()` . Patch by Benjamin
Johnson.
- gh-137706: Fix the partial evaluation of annotations that use `typing.Annotated[T, x]` where`T` is a forward reference.
- gh-88375: Fix normalization of the `robots.txt` rules and URLs in the`urllib.robotparser` module. No longer ignore trailing`?` .
Distinguish raw special characters`?` ,`=` and`&` from the
percent-encoded ones.
- gh-138515: `email` is added to Emscripten build.
- gh-99948: `ctypes.util.find_library()` now works in Emscripten build.
- gh-111788: Fix parsing errors in the `urllib.robotparser` module.
Donât fail trying to parse weird paths. Donât fail trying to decode
non-UTF-8`robots.txt` files.
- gh-138432: `zoneinfo.reset_tzpath()` will now convert any`os.PathLike` objects it receives into strings before adding them
to`TZPATH` . It will raise`TypeError` if anything other than a string
is found after this conversion. If given an`os.PathLike` object
that represents a relative path, it will now raise`ValueError` instead
of`TypeError` , and present a more informative error message.
- gh-132657: Improve the scaling of `copy.copy()` and`copy.deepcopy()` in the free-threading build.
- gh-116946: The types of `select.poll()` and`select.epoll()` objects are now immutable. Patch by BÃ©nÃ©dikt Tran.
- gh-116946: The `_random.Random` C type is now immutable. Patch by
BÃ©nÃ©dikt Tran.
- gh-57911: When extracting tar files on Windows, slashes in symlink targets will be replaced by backslashes to prevent corrupted links.
- gh-138205: Removed the `resize()` method on platforms that
donât support the underlying syscall, instead of raising a`SystemError` .
- gh-138008: Fix segmentation faults in the `ctypes` module due to
invalid`argtypes` . Patch by Dung Nguyen.
- gh-138252: `ssl` :`SSLContext` objects can now set client
and server TLS signature algorithms. If Python has been built with OpenSSL
3.5 or later,`SSLSocket` objects can return the signature
algorithms selected on a connection.
- gh-138253: Add the *block* parameter in the`put()` and`get()` methods of the`concurrent.interpreters` queues for compatibility
with the`queue.Queue` interface.
- gh-60462: Fix `locale.strxfrm()` on Solaris (and possibly other
platforms).
- gh-138239: The REPL now highlights `type` as a soft keyword in
type statements.
- gh-78502: `mmap.mmap` now has a*trackfd* parameter on Windows; if
it is`False` , the file handle corresponding to*fileno* will not be
duplicated.
- gh-138204: Forbid expansion of shared anonymous `memory maps` on Linux, which caused a bus error.
- gh-138010: Fix an issue where defining a class with a `@warnings.deprecated` -decorated base class may not invoke the
correct`__init_subclass__()` method in cases involving
multiple inheritance. Patch by Brian Schubert.
- gh-134716: Add support of regular expressions in the `-W` option
and the`PYTHONWARNINGS` environment variable.
- gh-138133: Prevent infinite traceback loop when sending CTRL^C to Python through `strace` .
- gh-138122: Implement **PEP 799** â A dedicated profiling package for
organizing Python profiling tools. Patch by Pablo Galindo.
- gh-138092: Fixed a bug in `mmap.mmap.flush()` where calling with only
an offset parameter would fail.
- gh-138044: Remove compatibility shim for deprecated parameter *package* in`importlib.resources.files()` . Patch by Semyon Moroz.
- gh-86819: `socket` : Add missing constants for ISO-TP sockets.
- gh-137884: Add `threading.get_native_id()` support for
Illumos/Solaris. Patch by YÃ¼ce Tekol.
- gh-134869: Fix an issue where pressing Ctrl+C during tab completion in the REPL would leave the autocompletion menu in a corrupted state.
- gh-137840: `typing.TypedDict` now supports the`closed` and`extra_items` keyword arguments (as described in**PEP 728** ) to control
whether additional non-required keys are allowed and to specify their
value type.
- gh-132947: Applied changes to `importlib.metadata` from
importlib_metadata 8.7,
including`dist` now disallowed for`EntryPoints.select` ; deferred
imports for faster import times; added support for metadata with newlines
(python/cpython#119650); and`metadata()` function now returns`None` when a metadata directory is present but no metadata is present.
- gh-90548: Fix `musl` detection for`platform.libc_ver()` on Alpine
Linux if compiled with âstrip-all.
- gh-137317: `inspect.signature()` now correctly handles classes that
use a descriptor on a wrapped`__init__()` or`__new__()` method. Contributed by Yongyu Yan.
- gh-137754: Fix import of the `zoneinfo` module if the C
implementation of the`datetime` module is not available.
- gh-125854: Improve error messages for invalid category in `warnings.warn()` .
- gh-137729: `locale.setlocale()` now supports language codes with`@` -modifiers.`@` -modifier are no longer silently removed in`locale.getlocale()` , but included in the language code.
- gh-73487: Speedup processing arguments (up to 1.5x) in the `decimal` module methods, that now using`METH_FASTCALL` calling
convention.  Patch by Sergey B Kirpichev.
- gh-137634: Calendar pages generated by the `calendar.HTMLCalendar` class now support dark mode and have been migrated to the HTML5 standard
for improved accessibility.
- gh-137630: The `_interpreters` module now uses Argument Clinic to
parse arguments. Patch by Adam Turner.
- gh-137583: Fix a deadlock introduced in 3.13.6 when a call to `ssl.SSLSocket.recv` was blocked in one thread,
and then another method on the object (such as`ssl.SSLSocket.send` ) was subsequently called in another thread.
- gh-92936: Update regex used by `http.cookies.SimpleCookie` to handle
values containing double quotes.
- gh-137426: Remove the code deprecation of `importlib.abc.ResourceLoader` . It is documented as deprecated, but left
for backwards compatibility with other classes in`importlib.abc` .
- gh-137490: Handle `ECANCELED` in the same way as`EINTR` in`signal.sigwaitinfo()` on NetBSD.
- gh-137512: Add new constants in the `resource` module:`RLIMIT_NTHR` ,`RLIMIT_UMTXP` ,`RLIMIT_PIPEBUF` ,`RLIMIT_THREADS` ,`RLIM_SAVED_CUR` , and`RLIM_SAVED_MAX` .
- gh-137044: `resource.RLIM_INFINITY` is now always a positive
integer. On all supported platforms, it is larger than any limited
resource value, which simplifies comparison of the resource values.
Previously, it could be negative, such as -1 or -3, depending on platform.
- gh-137477: Fix `inspect.getblock()` ,`inspect.getsourcelines()` and`inspect.getsource()` for generator expressions.
- gh-137481: Calendar uses the lengths of the localeâs weekdays to decide if the width requires abbreviation.
- gh-137466: Remove undocumented `glob.glob0()` and`glob.glob1()` functions, which have been deprecated since Python 3.13. Use`glob.glob()` and pass a directory to its*root_dir* argument instead.
- gh-137044: Return large limit values as positive integers instead of negative integers in `resource.getrlimit()` . Accept large values and
reject negative values (except`RLIM_INFINITY` ) for limits
in`resource.setrlimit()` .
- gh-115766: Fix `ipaddress.IPv4Interface.is_unspecified` .
- gh-75989: `tarfile.TarFile.extractall()` and`tarfile.TarFile.extract()` now overwrite symlinks when extracting
hardlinks. (Contributed by Alexander Enrique Urieles Nieto in
gh-75989.)
- gh-137017: Fix `threading.Thread.is_alive` to remain`True` until
the underlying OS thread is fully cleaned up. This avoids false negatives
in edge cases involving thread monitoring or premature`threading.Thread.is_alive` calls.
- gh-137273: Fix debug assertion failure in `locale.setlocale()` on
Windows.
- gh-137191: Fix how type parameters are collected, when `typing.Protocol` are specified with explicit parameters. Now,`typing.Generic` and`typing.Protocol` always dictate the
parameter number and parameter ordering of types. Previous behavior was a
bug.
- gh-137282: Fix tab completion and `dir()` on`concurrent.futures` .
- gh-137257: Bump the version of pip bundled in ensurepip to version 25.2
- gh-137239: `heapq` : Update`heapq.__all__` with`*_max` functions.
- gh-124503: `ast.literal_eval()` is 10-20% faster for small inputs.
- gh-137226: Fix behavior of `annotationlib.ForwardRef.evaluate()` when
the*type_params* parameter is passed and the name of a type param is also
present in an enclosing scope.
- gh-137197: `SSLContext` objects can now set TLS 1.3 cipher
suites via`set_ciphersuites()` .
- gh-81325: `tarfile.TarFile` now accepts a path-like when working on a tar archive. (Contributed by
Alexander Enrique Urieles Nieto in gh-81325.)
- gh-137185: Fix a potential async-signal-safety issue in `faulthandler` when printing C stack traces.
- gh-133951: Remove lib64-lib symlink creation when creating new virtual environments in `venv` module
- gh-130522: Fix unraisable `TypeError` raised during
interpreter shutdown in the`threading` module.
- gh-137059: Fix handling of file URLs with a Windows drive letter in the URL authority by `urllib.request.url2pathname()` . This fixes a
regression in earlier pre-releases of Python 3.14.
- gh-136980: Remove unused C tracing code in bdb for event type `c_call` ,`c_return` and`c_exception`
- gh-130577: `tarfile` now validates archives to ensure member offsets
are non-negative.  (Contributed by Alexander Enrique Urieles Nieto in
gh-130577.)
- gh-136170: Removed the unreleased `zipfile.ZipFile.data_offset` property
added in 3.14.0a7 as it wasnât fully clear which behavior it should have
in some situations so the result was not always what a user might expect.
- gh-121237: Support `%:z` directive for`datetime.datetime.strptime()` ,`datetime.time.strptime()` and`time.strptime()` . Patch by Lucas Esposito and Semyon Moroz.
- gh-136929: Ensure that hash functions guaranteed to be always *available* exist as attributes of`hashlib` even if they will not work at
runtime due to missing backend implementations. For instance,`hashlib.md5` will no longer raise`AttributeError` if OpenSSL is
not available and Python has been built without MD5 support. Patch by
BÃ©nÃ©dikt Tran.
- gh-124621: pyrepl now works in Emscripten.
- gh-136914: Fix retrieval of `doctest.DocTest.lineno` for objects
decorated with`functools.cache()` or`functools.cached_property` .
- gh-136912: `hmac.digest()` now properly handles large keys and
messages by falling back to the pure Python implementation when necessary.
Patch by BÃ©nÃ©dikt Tran.
- gh-83424: Allows creating a `ctypes.CDLL` without name when passing
a handle as an argument.
- gh-135228: When `dataclasses` replaces a class with a slotted
dataclass, the original class can now be garbage collected again. Earlier
changes in Python 3.14 caused this class to always remain in existence
together with the replacement class synthesized by`dataclasses` .
- gh-136874: Discard URL query and fragment in `urllib.request.url2pathname()` .
- gh-136787: `hashlib` : improve exception messages when a hash
algorithm is not recognized, blocked by the current security policy or
incompatible with the desired operation (for instance, using HMAC with
SHAKE). Patch by BÃ©nÃ©dikt Tran.
- gh-130645: Enable color help by default in `argparse` .
- gh-131724: In `http.client` , a new*max_response_headers* keyword-only parameter has been added to`HTTPConnection` and`HTTPSConnection` constructors. This parameter sets
the maximum number of allowed response headers, helping to prevent
denial-of-service attacks.
- gh-135427: With `-Werror` , the DeprecationWarning emitted by`os.fork()` and`os.forkpty()` in mutli-threaded processes
is now raised as an exception. Previously it was silently ignored. Patch
by Rani Pinchuk.
- gh-136234: Fix `asyncio.WriteTransport.writelines()` to be robust to
connection failure, by using the same behavior as`write()` .
- gh-53144: `encodings.aliases` : Add`latin_N` aliases
- gh-136669: `_asyncio` is now statically linked for improved
performance.
- gh-136134: `SMTP.auth_cram_md5()` now raises an`SMTPException` instead of a`ValueError` if Python has
been built without MD5 support. In particular,`SMTP` clients will not attempt to use this method even if the remote server is
assumed to support it. Patch by BÃ©nÃ©dikt Tran.
- gh-136134: `IMAP4.login_cram_md5` now
raises an`IMAP4.error` if CRAM-MD5
authentication is not supported. Patch by BÃ©nÃ©dikt Tran.
- gh-136591: `_hashlib` : avoid using deprecated functions*ERR_func_error_string* and*EVP_MD_CTX_md* when using
OpenSSL 3.0 and later. Patch by BÃ©nÃ©dikt Tran.
- gh-136571: `datetime.date.fromisocalendar()` can now raise
OverflowError for out of range arguments.
- gh-136549: Fix signature of `threading.excepthook()` .
- gh-136492: Expose **PEP 667** âs`FrameLocalsProxyType` in the`types` module.
- gh-83336: `utf8_sig` is now aliased to`encodings.utf_8_sig`
- gh-136523: Fix `wave.Wave_write` emitting an unraisable when open
raises.
- gh-136507: Fix mimetypes CLI to handle multiple file parameters.
- gh-52876: Add missing `keepends` (default`True` ) parameter to`codecs.StreamReaderWriter.readline()` and`codecs.StreamReaderWriter.readlines()` .
- gh-136470: Correct `concurrent.futures.InterpreterPoolExecutor` âs
default thread name.
- gh-136476: Fix a bug that was causing the `get_async_stack_trace` function to miss some frames in the stack trace.
- gh-136434: Fix docs generation of `UnboundItem` in`concurrent.interpreters` when running with`-OO` .
- gh-136380: Raises `AttributeError` when accessing`concurrent.futures.InterpreterPoolExecutor` and subinterpreters
are not available.
- gh-72327: Suggest using the system command prompt when `pip install` is
typed into the REPL. Patch by Tom Viner, Richard Si, and Brian Schubert.
- gh-135953: Implement a new high-frequency runtime profiler that leverages the existing remote debugging functionality to collect detailed execution statistics from running Python processes. This tool is exposed in the `profile.sample` module and enables non-intrusive observation of
production applications by attaching to already-running processes without
requiring any code modifications, restarts, or special startup flags. The
observer can perform extremely high-frequency sampling of stack traces and
interpreter state, providing detailed runtime execution analysis of live
applications.
- gh-136021: Make `type_params` parameter required in`typing._eval_type()` after a deprecation period for not providing
this parameter. Also remove the`DeprecationWarning` for the old
behavior.
- gh-136286: Fix pickling failures for protocols 0 and 1 for many objects related to subinterpreters.
- gh-136047: Fix issues with `typing` when the C implementation of`abc` is not available.
- gh-136316: Improve support for evaluating nested forward references in `typing.evaluate_forward_ref()` .
- gh-136306: `ssl` can now get and set groups used for key agreement.
- gh-136156: `tempfile.TemporaryFile()` no longer uses`os.O_EXCL` with`os.O_TMPFILE` , so itâs possible to use`linkat()` on the
file descriptor. Patch by Victor Stinner.
- gh-133982: Update Python implementation of `io.BytesIO` to be
thread safe.
- gh-136193: Improve `TypeError` error message, when richcomparing two`types.SimpleNamespace` objects.
- gh-136097: Fix potential infinite recursion and KeyError in ```
sysconfig
--generate-posix-vars
```
.
- gh-85702: If `zoneinfo._common.load_tzdata` is given a package without a
resource a`zoneinfo.ZoneInfoNotFoundError` is raised rather than a`PermissionError` . Patch by Victor Stinner.
- gh-90733: Improve error messages when reporting invalid parameters in `hashlib.scrypt()` . Patch by BÃ©nÃ©dikt Tran.
- gh-134759: Fix `UnboundLocalError` in`email.message.Message.get_payload()` when the payload to decode is a`bytes` object. Patch by Kliment Lamonov.
- gh-136028: Fix parsing month names containing âÄ°â (U+0130, LATIN CAPITAL LETTER I WITH DOT ABOVE) in `time.strptime()` . This affects locales
az_AZ, ber_DZ, ber_MA and crh_UA.
- gh-87135: Acquiring a `threading.Lock` or`threading.RLock` at interpreter shutdown will raise`PythonFinalizationError` if
Python can determine that it would otherwise deadlock.
- gh-135995: In the palmos encoding, make byte `0x9b` decode to`âº` (U+203A - SINGLE RIGHT-POINTING ANGLE QUOTATION MARK).
- gh-105456: Removed `sre_compile` ,`sre_constants` and`sre_parse` modules.
- gh-53203: Fix `time.strptime()` for`%c` and`%x` formats on
locales byn_ER, wal_ET and lzh_TW, and for`%X` format on locales ar_SA,
bg_BG and lzh_TW.
- gh-135878: Fixes a crash of `types.SimpleNamespace` on free
threading builds, when several threads were calling its`__repr__()` method at the same time.
- gh-135853: Add `math.fmax()` and`math.fmin()` to get the larger
and smaller of two floating-point values. Patch by BÃ©nÃ©dikt Tran.
- gh-135836: Fix `IndexError` in`asyncio.loop.create_connection()` that could occur when non-`OSError` exception is raised during
connection and socketâs`close()` raises`OSError` .
- gh-135853: `math` : expose C99`signbit()` function to
determine whether the sign bit of a floating-point value is set. Patch by
BÃ©nÃ©dikt Tran.
- gh-134531: `hmac` : use the*EVP_MAC(3ssl)* interface for
HMAC when Python is built with OpenSSL 3.0 and later instead of the
deprecated*HMAC_CTX(3ssl)* interface. Patch by
BÃ©nÃ©dikt Tran.
- gh-135836: Fix `IndexError` in`asyncio.loop.create_connection()` that could occur when the Happy Eyeballs algorithm resulted in an empty
exceptions list during connection attempts.
- gh-135855: Raise `TypeError` instead of`SystemError` when`_interpreters.set___main___attrs()` is passed a non-dict object.
Patch by Brian Schubert.
- gh-135823: `netrc` : improve the error message when the security check
for the ownership of the default configuration file`~/.netrc` fails.
Patch by BÃ©nÃ©dikt Tran.
- gh-135815: `netrc` : skip security checks if`os.getuid()` is
missing. Patch by BÃ©nÃ©dikt Tran.
- gh-135640: Address bug where it was possible to call `xml.etree.ElementTree.ElementTree.write()` on an ElementTree object
with an invalid root element. This behavior blanked the file passed to`write` if it already existed.
- gh-90117: Speed up `pprint` for`list` and`tuple` .
- gh-135759: `hashlib` : reject negative digest lengths in OpenSSL-based
SHAKE objects by raising a`ValueError` . Previously, negative lengths
were implicitly rejected by raising a`MemoryError` or a`SystemError` . Patch by BÃ©nÃ©dikt Tran.
- gh-123471: Make concurrent iterations over `itertools.chain` safe
under free threading.
- gh-135645: Added `supports_isolated_interpreters` field to`sys.implementation` .
- gh-135646: Raise consistent `NameError` exceptions in`annotationlib.ForwardRef.evaluate()`
- gh-135557: Fix races on `heapq` updates and`list` reads on
the free threaded build.
- gh-119180: Only fetch globals and locals if necessary in `annotationlib.get_annotations()`
- gh-135561: Fix a crash on DEBUG builds when an HACL* HMAC routine fails. Patch by BÃ©nÃ©dikt Tran.
- gh-135386: Fix opening a `dbm.sqlite3` database for reading from
read-only file or directory.
- gh-135444: Fix `asyncio.DatagramTransport.sendto()` to account for
datagram header size when data cannot be sent.
- gh-65697: `configparser` âs error message when attempting to write
an invalid key is now more helpful.
- gh-135497: Fix `os.getlogin()` failing for longer usernames on
BSD-based platforms.
- gh-135487: Fix `reprlib.Repr.repr_int()` when given integers with
more than`sys.get_int_max_str_digits()` digits. Patch by BÃ©nÃ©dikt
Tran.
- gh-135429: Fix the argument mismatch in `_lsprof` for`PY_THROW` event.
- gh-135368: Fix `unittest.mock.Mock` generation on`dataclasses.dataclass()` objects. Now all special attributes are set
as it was before gh-124429.
- gh-135336: `json` now encodes strings up to 2.2x faster if they
consist solely of characters that donât require escaping.
- gh-135335: `multiprocessing` : Flush`stdout` and`stderr` after
preloading modules in the`forkserver` .
- gh-126631: Fix `multiprocessing``forkserver` bug which prevented`__main__` from being preloaded.
- gh-133967: Do not normalize `locale` name âC.UTF-8â to âen_US.UTF-8â.
- gh-130870: Preserve `types.GenericAlias` subclasses in`typing.get_type_hints()`
- gh-135321: Raise a correct exception for values greater than 0x7fffffff for the `BINSTRING` opcode in the C implementation of`pickle` .
- gh-121914: Changed the names of the symbol tables for lambda expressions and generator expressions to â<lambda>â and â<genexpr>â respectively to avoid conflicts with user-defined names.
- gh-135276: Synchronized zipfile.Path with zipp 3.23, including improved performance of `zipfile.Path.open()` for non-reading modes, rely on`functools.cached_property()` to cache values on the instance. Rely on`save_method_args` to save the initialization method arguments. Fixed`.name` ,`.stem` and other basename-based properties on Windows when
working with a zipfile on disk.
- gh-135234: `hashlib` : improve exception messages when an OpenSSL
function failed. When memory allocation fails on OpenSSLâs side, a`MemoryError` is raised instead of a`ValueError` . Patch by
BÃ©nÃ©dikt Tran.
- gh-135244: `uuid` : when the MAC address cannot be determined, the
48-bit node ID is now generated with a cryptographically-secure
pseudo-random number generator (CSPRNG) as per**RFC 9562, Â§6.10.3** . This affects`uuid1()` and`uuid6()` .
- gh-135241: The `INT` opcode of the C accelerator`_pickle` module was updated to look only for â00â and â01â to push booleans onto
the stack, aligning with the Python`pickle` module.
- gh-133934: Improve `sqlite3` CLIâs`.help` message.
- gh-135069: Fix the âInvalid error handlingâ exception in `encodings.idna.IncrementalDecoder` to correctly replace the
âerrorsâ parameter.
- gh-130662: +Accept leading zeros in precision and width fields for +:class: `Decimal` formatting, for example```
format(Decimal(1.25),
'.016f')
```
.
- gh-130662: Accept leading zeros in precision and width fields for `Fraction` formatting, for example```
format(Fraction(1,
3), '.016f')
```
.
- gh-135004: Rewrite and cleanup the internal `_blake2` module. Some
exception messages were changed but their types were left untouched. Patch
by BÃ©nÃ©dikt Tran.
- gh-134953: Expand `_colorize` theme with`keyword_constant` and
implement in repl.
- gh-134978: `hashlib` : Supporting the`string` keyword parameter in
hash function constructors such as`new()` or the direct
hash-named constructors such as`md5()` and`sha256()` is now deprecated and slated for removal in Python
3.19. Prefer passing the initial data as a positional argument for maximum
backwards compatibility. Patch by BÃ©nÃ©dikt Tran.
- gh-134970: Fix the âunknown actionâ exception in `argparse.ArgumentParser.add_argument_group()` to correctly replace
