---
id: collect-261121-veille-ia/docs-techniques/changelog-2-5
title: "Changelog"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["free", "stack", "threading", "patch", "same", "bytearray", "build", "future"]
source: https://docs.python.org/release/3.15.1/whatsnew/changelog.html
source_anchor: ""
source_lines: [6001, 7500]
section: "Docs_Techniques"
---
- gh-130415: Improve JITâs ability to optimize strings in boolean contexts.
- gh-130396: Use actual stack limits (from *pthread_getattr_np(3)* ) for linux, and other systems with`_GNU_SOURCE` defined, when determining limits for C stack protection.
- gh-128396: Fix a crash that occurs when calling `locals()` inside an
inline comprehension that uses the same local variable as the outer frame
scope where the variable is a free or cell var.
- gh-129107: Fix two more `bytearray` functions for free
threading.
- gh-127705: Use tagged references ( `_PyStackRef` ) for the default build
as well as for the free-threading build. This has a small negative
performance impact short-term but will enable larger speedups in the
future and significantly reduce maintenance costs by allowing a single
implementation of tagged references in the future.
- gh-130094: Fix two race conditions involving concurrent imports that could lead to spurious failures with `ModuleNotFoundError` .
- gh-129107: Make `bytearray` iterator safe under free
threading.
- gh-115802: Use the more efficient âmediumâ code model for JIT-compiled code on supported platforms.
- gh-107956: A `build-details.json` file is now install in the
platform-independent standard library directory (**PEP 739** implementation).
- gh-116042: Fix location for SyntaxErrors of invalid escapes in the tokenizer. Patch by Pablo Galindo
- gh-91079: Change C stack overflow protection to consider the amount of stack consumed, rather than a counter. This allows deeper recursion in many cases, but remains safe.
- gh-129715: Improve the experimental JITâs handling of returns to unknown callers.

### LibraryÂ¶

- gh-129983: Fix data race in compile_template in `sre.c` .

### Core and BuiltinsÂ¶

- gh-129967: Fix a race condition in the free threading build when `repr(set)` is called concurrently with`set.clear()` .
- gh-129953: The internal (evaluation) stack is now spilled to memory whenever execution escapes from the interpreter or JIT compiled code. This should have no observable effect in either Python or builtin extensions, but will allow various important optimizations in the future.
- gh-129515: Clarify syntax error messages for conditional expressions when a statement is specified before an `if` or after an`else` keyword.
- gh-129349: `bytes.fromhex()` and`bytearray.fromhex()` now accepts
ASCII`bytes` and bytes-like objects.
- gh-129149: Add fast path for medium-size integers in `PyLong_FromSsize_t()` . Patch by Chris Eibl.
- gh-129107: Make the `bytearray` safe under free threading.
- gh-128974: Fix a crash in `UnicodeError.__str__` when custom attributes implement`__str__()` with
side-effects. Patch by BÃ©nÃ©dikt Tran.
- gh-126085: `typing.TypeAliasType` now supports star unpacking.
- gh-125331: `from __future__ import barry_as_FLUFL` now works in more
contexts, including when it is used in files, with the`-c` flag, and in
the REPL when there are multiple statements on the same line. Previously,
it worked only on subsequent lines in the REPL, and when the appropriate
flags were passed directly to`compile()` . Patch by Pablo Galindo.
- gh-121464: Make concurrent iterations over the same `enumerate()` iterator safe under free-threading. See Strategy for Iterators in Free
Threading.
- gh-87790: Support underscore and comma as thousands separators in the fractional part for floating-point presentation types of the new-style string formatting (with `format()` or f-strings).  Patch by
Sergey B Kirpichev.
- gh-124445: Fix specialization of generic aliases that are generic over a `typing.ParamSpec` and have been specialized with a nested type
variable.
- gh-120608: Adapt `reversed()` for use in the free-threading build. The`reversed()` is still not thread-safe in the sense that concurrent
iterations may see the same object, but they will not corrupt the
interpreter state.

### LibraryÂ¶

- gh-100388: Fix the `platform._sys_version()` method when`__DATE__` is
undefined at buildtime by changing default buildtime datetime string to
the UNIX epoch.

### Core and BuiltinsÂ¶

- bpo-44369: Improve syntax errors for incorrectly closed strings. Patch by Pablo Galindo

### C APIÂ¶

- gh-111178: Fix `PyCMethod` API: replace`size_t nargs` with`Py_ssize_t nargs` in`PyCMethod` . Patch by Victor Stinner.
- gh-130947: Add again `PySequence_Fast()` to the limited C API. Patch
by Victor Stinner.
- gh-128863: The following private functions are deprecated and planned for removal in Python 3.18: 
  - `_PyUnicodeWriter_Init()` :
replace`_PyUnicodeWriter_Init(&writer)` with`writer = PyUnicodeWriter_Create(0)` .
  - `_PyUnicodeWriter_Finish()` :
replace`_PyUnicodeWriter_Finish(&writer)` with`PyUnicodeWriter_Finish(writer)` .
  - `_PyUnicodeWriter_Dealloc()` :
replace`_PyUnicodeWriter_Dealloc(&writer)` with`PyUnicodeWriter_Discard(writer)` .
  - `_PyUnicodeWriter_WriteChar()` :
replace`_PyUnicodeWriter_WriteChar(&writer, ch)` with`PyUnicodeWriter_WriteChar(writer, ch)` .
  - `_PyUnicodeWriter_WriteStr()` :
replace`_PyUnicodeWriter_WriteStr(&writer, str)` with`PyUnicodeWriter_WriteStr(writer, str)` .
  - `_PyUnicodeWriter_WriteSubstring()` :
replace`_PyUnicodeWriter_WriteSubstring(&writer, str, start, end)` with`PyUnicodeWriter_WriteSubstring(writer, str, start, end)` .
  - `_PyUnicodeWriter_WriteASCIIString()` :
replace`_PyUnicodeWriter_WriteASCIIString(&writer, str)` with`PyUnicodeWriter_WriteUTF8(writer, str)` .
  - `_PyUnicodeWriter_WriteLatin1String()` :
replace`_PyUnicodeWriter_WriteLatin1String(&writer, str)` with`PyUnicodeWriter_WriteUTF8(writer, str)` .
  - `_PyUnicodeWriter_Prepare()` : (no replacement).
  - `_PyUnicodeWriter_PrepareKind()` : (no replacement).
 The pythoncapi-compat project can be used to get these new public functions on Python 3.13 and older. Patch by Victor Stinner.
- gh-45325: Add a new `p` format parameter to`Py_BuildValue()` that
allows to take a C integer and produce a Python`bool` object.
Patch by Pablo Galindo.

### BuildÂ¶

- gh-131035: Use `-flto=thin` for faster build times using clang-cl on
Windows. Patch by Chris Eibl.
- gh-130740: Ensure that `Python.h` is included before`stdbool.h` unless`pyconfig.h` is included before or in some platform-specific
contexts.
- gh-130090: Building with `PlatformToolset=ClangCL` on Windows now
supports PGO (profile guided optimization). Patch by Chris Eibl with
invaluable support from Steve Dover.
- gh-129819: Allow building the JIT with the tailcall interpreter.
- gh-129989: Fix a bug where the tailcall interpreter was enabled when `--without-tail-call-interp` was provided to the configure script.
- gh-129838: Donât redefine `_Py_NO_SANITIZE_UNDEFINED` when compiling
with a recent GCC version and undefined sanitizer enabled.
- gh-82909: `#pragma` -based linking with`python3*.lib` can now be
switched off with Py_NO_LINK_LIB. Patch by Jean-Christophe
Fillion-Robin.

## Python 3.14.0 alpha 5Â¶

*Release date: 2025-02-11*

### macOSÂ¶

- gh-91132: Update macOS installer to use ncurses 6.5.

### Tools/DemosÂ¶

### TestsÂ¶

- gh-129386: Add `test.support.reset_code` , which can be used to reset
various bytecode-level optimizations and local instrumentation for a
function.
- gh-128474: Disable `test_embed` test cases that segfault on BOLT
instrument binaries. The tests are only disabled when BOLT is enabled.
- gh-128003: Add an option `--parallel-threads=N` to the regression test
runner that runs individual tests in multiple threads in parallel in order
to find concurrency bugs.  Note that most of the test suite is not yet
reviewed for thread-safety or annotated with`@thread_unsafe` when
necessary.

### SecurityÂ¶

- gh-105704: When using `urllib.parse.urlsplit()` and`urllib.parse.urlparse()` host parsing would not reject domain names
containing square brackets (`[` and`]` ). Square brackets are only
valid for IPv6 and IPvFuture hosts according to RFC 3986 Section 3.2.2.
- gh-126108: Fix a possible `NULL` pointer dereference in`PySys_AddWarnOptionUnicode()` .
- gh-80222: Fix bug in the folding of quoted strings when flattening an email message using a modern email policy. Previously when a quoted string was folded so that it spanned more than one line, the surrounding quotes and internal escapes would be omitted. This could theoretically be used to spoof header lines using a carefully constructed quoted string if the resulting rendered email was transmitted or re-parsed.
- gh-119511: Fix a potential denial of service in the `imaplib` module.
When connecting to a malicious server, it could cause an arbitrary amount
of memory to be allocated. On many systems this is harmless as unused
virtual memory is only a mapping, but if this hit a virtual address size
limit it could lead to a`MemoryError` or other process crash. On
unusual systems or builds where all allocated memory is touched and backed
by actual ram or storage it couldâve consumed resources doing so until
similarly crashing.

### LibraryÂ¶

- gh-129939: Comparison pages with highlighted changes generated by the `difflib.HtmlDiff` class now support dark mode.
- gh-129928: Raise `sqlite3.ProgrammingError` if a user-defined SQL
function with invalid number of parameters is created. Patch by Erlend
Aasland.
- gh-129583: Update bundled pip to 25.0.1
- gh-129766: Fix crash in `warnings` , when calling`_release_lock()` with no existing lock.
- gh-129005: `_pyio.FileIO.readall()` now allocates, resizes, and fills a
data buffer using the same algorithm`_io.FileIO.readall()` uses.
- gh-129646: Update the locale alias mapping in the `locale` module to
match the latest X Org locale alias mapping and support new locales in
Glibc 2.41.
- gh-128317: Put CLI calendar highlighting in private class, removing `highlight_day` from public`calendar.TextCalendar` API. Patch by
Hugo van Kemenade.
- gh-129603: Fix bugs where `sqlite3.Row` objects could segfault if
their inherited`description` was set to`None` .
Patch by Erlend Aasland.
- gh-129559: Add `bytearray.resize()` method so`bytearray` can
be efficiently resized in place.
- gh-129502: Unlikely errors in preparing arguments for `ctypes` callback are now handled in the same way as errors raised in the callback
of in converting the result of the callback â using`sys.unraisablehook()` instead of`sys.excepthook()` and not
setting`sys.last_exc` and other variables.
- gh-129403: Corrected `ValueError` message for`asyncio.Barrier` and`threading.Barrier` .
- gh-129409: Fix an integer overflow in the `csv` module when writing a
data field larger than 2GB.
- gh-126400: Add a socket *timeout* keyword argument to`logging.handlers.SysLogHandler` .
- gh-118761: Always lazy import `warnings` in`threading` . Patch by
Taneli Hukkinen.
- gh-118761: Improve import time of `subprocess` by lazy importing`locale` and`signal` . Patch by Taneli Hukkinen.
- gh-129346: In `sqlite3` , handle out-of-memory when creating
user-defined SQL functions.
- gh-129005: Optimize `_pyio.FileIO.readinto` by avoiding unnecessary
objects and copies using`os.readinto()` .
- gh-129195: Support reporting call graph information from `asyncio.staggered.staggered_race()` .
- gh-129205: Add `os.readinto()` to read into a buffer object from a file descriptor.
- gh-128772: Fix `pydoc` for methods with the`__module__` attribute
equal to`None` .
- gh-129061: Fix FORCE_COLOR and NO_COLOR when empty strings. Patch by Hugo van Kemenade.
- gh-92897: Scheduled the deprecation of the `check_home` argument of`sysconfig.is_python_build()` to Python 3.15.
- gh-129064: Deprecate `sysconfig.expand_makefile_vars()` , in favor of
using`sysconfig.get_paths()` with the`vars` argument.
- gh-128550: Removed an incorrect optimization relating to eager tasks in `asyncio.TaskGroup` that resulted in cancellations being missed.
- gh-128991: Release the enter frame reference within `bdb` callback
- gh-118761: Reduce import time of `pstats` and`zipfile` by up to
20%, by removing unnecessary imports to`typing` . Patch by BÃ©nÃ©dikt
Tran.
- gh-128978: Fix a `NameError` in`sysconfig.expand_makefile_vars()` . Patch by BÃ©nÃ©dikt Tran.
- gh-128961: Fix a crash when setting state on an exhausted `array.array` iterator.
- gh-128894: Fix `traceback.TracebackException._format_syntax_error` not
to fail on exceptions with custom metadata.
- gh-128916: Do not attempt to set `SO_REUSEPORT` on sockets of address
families other than`AF_INET` and`AF_INET6` , as it is meaningless
with these address families, and the call with fail with Linux kernel
6.12.9 and newer.
- gh-118761: Improve import time of `tomllib` by removing`typing` ,`string` , and`tomllib._types` imports. Patch by Taneli Hukkinen.
- gh-128679: `tracemalloc` : Fix race conditions when`tracemalloc.stop()` is called by a thread, while other threads are
tracing memory allocations. Patch by Victor Stinner.
- gh-128891: Add specialized opcodes to `opcode.opname` .
- gh-118761: Reduce import time of `gettext` by up to ten times, by
importing`re` on demand. In particular,`re` is no longer
implicitly exposed as`gettext.re` . Patch by Eli Schwartz.
- gh-118761: Reduce the import time of `optparse` when no help text is
printed. Patch by Eli Schwartz.
- gh-128657: Fix possible extra reference when using objects returned by `hashlib.sha256()` under free threading.
- gh-118761: Reduce the import time of `csv` by up to five times, by
importing`re` on demand. In particular,`re` is no more implicitly
exposed as`csv.re` . Patch by BÃ©nÃ©dikt Tran.
- gh-128308: Support the *name* keyword argument for eager tasks in`asyncio.loop.create_task()` ,`asyncio.create_task()` and`asyncio.TaskGroup.create_task()` , by passing on all*kwargs* to the
task factory set by`asyncio.loop.set_task_factory()` .
- gh-118761: Improve the performance of `base64.b16decode()` by up to
ten times by more efficiently checking the byte-string for hexadecimal
digits. Reduce the import time of`base64` by up to six times, by no
longer importing`re` . Patch by BÃ©nÃ©dikt Tran, Chris Markiewicz, and
Adam Turner.
- gh-128156: When using macOS system `libffi` , support for complex types
in`ctypes` is now checked at runtime (macOS 10.15 or newer). The
types must also be available at build time.
- gh-128636: Fix PyREPL failure when `os.environ` is overwritten with
an invalid value.
- gh-128498: Default to stdout isatty for color detection instead of stderr. Patch by Hugo van Kemenade.
- gh-128384: Add locking to `warnings` to avoid some data races when
free-threading is used.  Change`_warnings_runtime_state.mutex` to be a
recursive mutex and expose it to`warnings` , via the`_acquire_lock()` and`_release_lock()` functions. The lock is
held when`filters` and`_filters_version` are updated.
- gh-128509: Add `sys._is_immortal()` for identifying immortal
objects at runtime.
- gh-128479: Fix `asyncio.staggered.staggered_race()` leaking tasks and
issuing an unhandled exception.
- gh-128427: `uuid.NIL` and`uuid.MAX` are now available to
represent the Nil and Max UUID formats as defined by**RFC 9562** .
- gh-91279: `zipfile.ZipFile.writestr()` now respect`SOURCE_DATE_EPOCH` that distributions can set centrally and have build
tools consume this in order to produce reproducible output.
- gh-112064: Fix incorrect handling of negative read sizes in `HTTPResponse.read` . Patch by Yury
Manushkin.
- gh-58956: Fixed a frame reference leak in `bdb` .
- gh-128131: Completely support random access of uncompressed unencrypted read-only zip files obtained by `ZipFile.open` .
- gh-127975: Avoid reusing quote types in `ast.unparse()` if not needed.
- gh-115514: Fix exceptions and incomplete writes after `asyncio._SelectorTransport` is closed before writes are
completed.
- gh-121604: Add missing Deprecation warnings for `importlib.machinery.DEBUG_BYTECODE_SUFFIXES` ,`importlib.machinery.OPTIMIZED_BYTECODE_SUFFIXES` ,`importlib.machinery.WindowsRegistryFinder` ,`importlib.abc.ResourceLoader` ,`importlib.abc.SourceLoader.path_mtime()` .
- gh-127873: When `-E` is set, only ignore`PYTHON_COLORS` and not`FORCE_COLOR` /`NO_COLOR` /`TERM` when colourising output. Patch by
Hugo van Kemenade.
- gh-125413: Add `pathlib.Path.info` attribute, which stores an object
implementing the`pathlib.types.PathInfo` protocol (also new). The
object supports querying the file type and internally caching`stat()` results. Path objects generated by`iterdir()` are initialized with file type information
gleaned from scanning the parent directory.
- gh-127712: Fix handling of the `secure` argument of`logging.handlers.SMTPHandler` .
- gh-127096: Do not recreate unnamed section on every read in `configparser.ConfigParser` . Patch by Andrey Efremov.
- gh-124369: Deprecate `pdb.Pdb.curframe_locals`
- gh-126332: Fix _pyrepl crash when entering a double CTRL-Z on an overflowing line.
- gh-125553: Fix round-trip invariance for backslash continuations in `tokenize.untokenize()` .
- gh-91048: Add `asyncio.capture_call_graph()` and`asyncio.print_call_graph()` functions.
- gh-124703: Quitting `pdb` in`inline` mode will emit a confirmation
prompt and exit gracefully now, instead of printing an exception
traceback.
- gh-123987: Fixed issue in NamespaceReader where a non-path item in a namespace path, such as a sentinel added by an editable installer, would break resource loading.
- gh-119349: Add the `ctypes.util.dllist()` function to list the loaded
shared libraries for the current process.
- gh-55454: Add IMAP4 `IDLE` support to the`imaplib` module.  Patch
by Forest.
- gh-119257: Show tab completions menu below the current line, which results in less janky behaviour, and fixes a cursor movement bug. Patch by Daniel Hollas
- gh-101410: Support custom messages for domain errors in the `math` module (`math.sqrt()` ,`math.log()` and`math.atanh()` were
modified as examples).  Patch by Charlie Zhao and Sergey B Kirpichev.
- gh-81340: Use `os.copy_file_range()` in`shutil.copy()` ,`shutil.copy2()` , and`shutil.copyfile()` functions by default. An
underlying Linux system call gives filesystems an opportunity to implement
the use of copy-on-write (in case of btrfs and XFS) or server-side copy
(in the case of NFS.) Patch by Illia Volochii.
- bpo-27307: Add attribute and item access support to `string.Formatter` in auto-numbering mode, which allows format
strings like â{.name}â and â{[1]}â.

### IDLEÂ¶

- gh-129873: Simplify displaying the IDLE doc by only copying the text section of idle.html to idlelib/help.html. Patch by Stan Ulbrych.

### DocumentationÂ¶

- gh-125722: Require Sphinx 8.1.3 or later to build the Python documentation. Patch by Adam Turner.
- gh-67206: Document that `string.printable` is not printable in the
POSIX sense. In particular,`string.printable.isprintable()` returns`False` . Patch by BÃ©nÃ©dikt Tran.

### Core and BuiltinsÂ¶

- gh-100239: Replace the opcode BINARY_SUBSCR and its family by BINARY_OP with oparg NB_SUBSCR.
- gh-129732: Fixed a race in `_Py_qsbr_reserve` in the free threading
build.
- gh-129763: Remove the internal `LLTRACE` macro (use`Py_DEBUG` instead).
- gh-129715: Improve JIT performance for generators.
- gh-129643: Fix thread safety of `PyList_Insert()` in free-threading
builds.
- gh-129668: Fix race condition when raising `MemoryError` in the free
threaded build.
- gh-129643: Fix thread safety of `PyList_SetItem()` in free-threading
builds. Patch by Kumar Aditya.
- gh-128563: Fix an issue where the âlltraceâ debug feature could have been incorrectly enabled for some frames.
- gh-129393: On FreeBSD, `sys.platform` doesnât contain the major
version anymore. It is always`'freebsd'` , instead of`'freebsd13'` or`'freebsd14'` .

### LibraryÂ¶

- gh-129345: Fix null pointer dereference in `syslog.openlog()` when an
audit hook raises an exception.

### Core and BuiltinsÂ¶

- gh-129231: Improve memory layout of JIT traces. Patch by Diego Russo
- gh-129149: Add fast path for medium-size integers in `PyLong_FromUnsignedLong()` ,`PyLong_FromUnsignedLongLong()` and`PyLong_FromSize_t()` .
- gh-129201: The free-threaded version of the cyclic garbage collector has been optimized to conditionally use CPU prefetch instructions during the collection. This can reduce collection times by making it more likely that data is in the CPU cache when it is needed. The prefetch instructions are enabled if the number of long-lived objects (objects surviving a full collection) exceeds a threshold.
- gh-129093: Fix f-strings such as `f'{expr=}'` sometimes not displaying
the full expression when the expression contains`!=` .
- gh-124363: Treat debug expressions in f-string as raw strings. Patch by Pablo Galindo
- gh-128714: Fix the potential races in get/set dunder methods `__annotations__` ,`__annotate__` and`__type_params__` for function
object, and add related tests.
- gh-128799: Add frame of `except*` to traceback when it wraps a naked
exception.
- gh-128842: Collect JIT memory stats using pystats. Patch by Diego Russo.
- gh-100239: Specialize `BINARY_OP` for bitwise logical operations on
compact ints.
- gh-128910: Undocumented and unused private C-API functions `_PyTrash_begin` and`_PyTrash_end` are removed.
- gh-128807: Add a marking phase to the free-threaded GC. This is similar to what was done in gh-126491. Since the free-threaded GC does not have generations and is not incremental, the marking phase looks for all objects reachable from known roots. The roots are objects known to not be garbage, like the module dictionary for `sys` .  For most programs,
this marking phase should make the GC a bit faster since typically less
work is done per object.
- gh-100239: Add opcode `BINARY_OP_EXTEND` which executes a pair of
functions (guard and specialization functions) accessed from the inline
cache.
- gh-128563: A new type of interpreter has been added to CPython. This interpreter uses tail calls for its instruction handlers. Preliminary benchmark results suggest 7-11% geometric mean faster on pyperformance (depending on platform), and up to 30% faster on Python-intensive workloads. This interpreter currently only works on newer compilers, such as `clang-19` . Other compilers will continue using the old interpreter.
Patch by Ken Jin, with ideas on how to implement this in CPython by Mark
Shannon, Garret Gu, Haoran Xu, and Josh Haberman.
- gh-126703: Improve performance of iterating over lists and tuples by using a freelist for the iterator objects.
- gh-127953: The time to handle a `LINE` event in sys.monitoring (and
sys.settrace) is now independent of the number of lines in the code
object.
- gh-128330: Restore terminal control characters on REPL exit.
- gh-128016: Improved the `SyntaxWarning` message for invalid escape
sequences to clarify that such sequences will raise a`SyntaxError` in
future Python releases. The new message also suggests a potential fix,
i.e.,`Did you mean "\\e"?` .
- gh-126004: Fix handling of `UnicodeError.start` and`UnicodeError.end` values in the`codecs.replace_errors()` error
handler. Patch by BÃ©nÃ©dikt Tran.
- gh-126004: Fix handling of `UnicodeError.start` and`UnicodeError.end` values in the`codecs.backslashreplace_errors()` error handler. Patch by BÃ©nÃ©dikt
Tran.
- gh-126004: Fix handling of `UnicodeError.start` and`UnicodeError.end` values in the`codecs.xmlcharrefreplace_errors()` error handler. Patch by BÃ©nÃ©dikt
Tran.
- gh-127119: Slightly optimize the `int` deallocator.
- gh-127349: Fixed the error when resizing terminal in Python REPL. Patch by Semyon Moroz.
- gh-125723: Fix crash with `gi_frame.f_locals` when generator frames
outlive their generator. Patch by Mikhail Efimov.

### LibraryÂ¶

- gh-126349: Add `turtle.fill()` ,`turtle.poly()` and`turtle.no_animation()` context managers. Patch by Marie Roald and
Yngve Mardal Moe.

### Core and BuiltinsÂ¶

- gh-115911: If the current working directory cannot be determined due to permissions, then import will no longer raise `PermissionError` .
Patch by Alex Willmer.

### LibraryÂ¶

- gh-112713: Added support for the `Partitioned` cookie flag in`http.cookies` .

### C APIÂ¶

- gh-129533: Update `PyGC_Enable()` ,`PyGC_Disable()` ,`PyGC_IsEnabled()` to use atomic operation for thread-safety at
free-threading build. Patch by Donghee Na.
- gh-89188: Implement `PyUnicode_KIND()` and`PyUnicode_DATA()` as function, in addition to the macros with the same names. The macros
rely on C bit fields which have compiler-specific layout. Patch by Victor
Stinner.
- gh-91417: Remove `PySequence_Fast()` from the limited C API, since
this function has to be used with`PySequence_Fast_GET_ITEM` which never worked in the limited C API. Patch by Victor Stinner.
- gh-128509: Add `PyUnstable_IsImmortal()` for determining whether an
object is immortal.
- gh-129033: Remove `_PyInterpreterState_GetConfigCopy()` and`_PyInterpreterState_SetConfig()` private functions. Use instead`PyConfig_Get()` and`PyConfig_Set()` , public C API added by**PEP 741** âPython Configuration C APIâ. Patch by Victor Stinner.
- gh-129033: Remove the private `_Py_InitializeMain()` function. It was a
provisional API added to Python 3.8 by**PEP 587** . Patch by Victor
Stinner.
- gh-128844: Add `PyUnstable_TryIncRef()` and`PyUnstable_EnableTryIncRef()` unstable APIs.  These are helpers for
dealing with unowned references in a thread-safe way, particularly in the
free threading build.
- gh-128911: Add `PyImport_ImportModuleAttr()` and`PyImport_ImportModuleAttrString()` helper functions to import a
module and get an attribute of the module. Patch by Victor Stinner.
- gh-128863: The following private functions are deprecated and planned for removal in Python 3.18: 
  - `_PyBytes_Join()` : use`PyBytes_Join()` .
  - `_PyDict_GetItemStringWithError()` : use`PyDict_GetItemStringRef()` .
  - `_PyDict_Pop()` : use`PyDict_Pop()` .
  - `_PyLong_Sign()` : use`PyLong_GetSign()` .
  - `_PyLong_FromDigits()` and`_PyLong_New()` :
use`PyLongWriter_Create()` .
  - `_PyThreadState_UncheckedGet()` : use`PyThreadState_GetUnchecked()` .
  - `_PyUnicode_AsString()` : use`PyUnicode_AsUTF8()` .
  - `_Py_HashPointer()` : use`Py_HashPointer()` .
  - `_Py_fopen_obj()` : use`Py_fopen()` .
 The pythoncapi-compat project can be used to get these new public functions on Python 3.13 and older. Patch by Victor Stinner.
- gh-126599: Remove some internal test APIs for the experimental JIT compiler.
- gh-127925: Convert the `decimal` module to use**PEP 757** C API
(export-import integers), offering some speed-up if the integer part of
the`Decimal` instance is small.  Patch by Sergey B
Kirpichev.

### BuildÂ¶

## Python 3.14.0 alpha 4Â¶

*Release date: 2025-01-14*

### macOSÂ¶

- gh-127592: Usage of the unified Apple System Log APIs was disabled when the minimum macOS version is earlier than 10.12.

### Tools/DemosÂ¶

- gh-128152: Fix a bug where Argument Clinicâs C pre-processor parser tried to parse pre-processor directives inside C comments. Patch by Erlend Aasland.

### TestsÂ¶

- gh-128690: Temporarily do not use test_embed in PGO profile builds until the problem with test_init_pyvenv_cfg failing in some configurations is resolved.

### LibraryÂ¶

- gh-128731: Fix `ResourceWarning` in`urllib.robotparser.RobotFileParser.read()` .
- gh-71339: Add new assertion methods for `unittest` :`assertHasAttr()` ,`assertNotHasAttr()` ,`assertIsSubclass()` ,`assertNotIsSubclass()``assertStartsWith()` ,`assertNotStartsWith()` ,`assertEndsWith()` and`assertNotEndsWith()` .
- gh-118761: Improve import time of `pickle` by 25% by removing an
unnecessary regular expression. As such,`re` is no more implicitly
available as`pickle.re` . Patch by BÃ©nÃ©dikt Tran.
- gh-128661: Fixes `typing.evaluate_forward_ref()` not showing
deprecation when`type_params` arg is not passed.
- gh-128562: Fix possible conflicts in generated `tkinter` widget names
if the widget class name ends with a digit.
- gh-128559: Improved import time of `asyncio` .
- gh-128552: Fix cyclic garbage introduced by `asyncio.loop.create_task()` and`asyncio.TaskGroup.create_task()` holding a reference to the created task if it is eager.
- gh-128340: Add internal thread safe handle to be used in `asyncio.loop.call_soon_threadsafe()` for thread safe cancellation.
- gh-128182: Fix crash when using `ctypes` pointers concurrently on the
free threaded build.
- gh-128400: Only show the current thread in `faulthandler` on the
free threaded build to prevent races.
- gh-128400: Fix crash when using `faulthandler.dump_traceback()` while
other threads are active on the free threaded
build.
- gh-128388: Fix `PyREPL` on Windows to support more keybindings, like the`Control` -`â` and`Control` -`â` word-skipping keybindings and those
with meta (i.e.`Alt` ), e.g.`Alt` -`d` to`kill-word` or`Alt` -`Backspace``backward-kill-word` .
- gh-88834: Unify the instance check for `typing.Union` and`types.UnionType` :`Union` now uses the instance checks
against its parameters instead of the subclass checks.
- gh-128302: Fix `xml.dom.xmlbuilder.DOMEntityResolver.resolveEntity()` , which was
broken by the Python 3.0 transition.
- gh-128317: Highlight today in colour in `calendar` âs CLI output.
Patch by Hugo van Kemenade.
- gh-128302: Allow `xml.dom.xmlbuilder.DOMParser.parse()` to correctly
handle`xml.dom.xmlbuilder.DOMInputSource` instances that only
have a`systemId` attribute set.
- gh-128151: Improve generation of `UUID` objects version 3, 4,
5, and 8 via their dedicated functions by 30%. Patch by BÃ©nÃ©dikt Tran.
- gh-128118: Improve performance of `copy.copy()` by 30% via a fast path
for atomic types and container types.
- gh-127946: Fix crash when modifying `ctypes._CFuncPtr` objects
concurrently on the free threaded build.
- gh-128062: Revert the font of `turtledemo` âs menu bar to its default
value and display the shortcut keys in the correct position.
- gh-128014: Fix resetting the default window icon by passing `default=''` to the`tkinter` method`wm_iconbitmap()` .
- gh-41872: Fix quick extraction of module docstrings from a file in `pydoc` . It now supports docstrings with single quotes, escape
sequences, raw string literals, and other Python syntax.
- gh-127060: Set TERM environment variable to âdumbâ to disable traceback colors in IDLE, since IDLE doesnât understand ANSI escape sequences. Patch by Victor Stinner.
- gh-126742: Fix support of localized error messages reported by *dlerror(3)* and*gdbm_strerror* in`ctypes` and`dbm.gnu` functions respectively. Patch by BÃ©nÃ©dikt
Tran.
- gh-122548: Adds two new local events to sys.monitoring, `BRANCH_LEFT` and`BRANCH_RIGHT` . This allows the two arms of the branch to be
disabled independently, which should hugely improve performance of
branch-level coverage tools. The old branch event,`BRANCH` is now
deprecated.
- gh-127847: Fix the position when doing interleaved seeks and reads in uncompressed, unencrypted zip files returned by `zipfile.ZipFile.open()` .
- gh-127688: Add the `SCHED_DEADLINE` and`SCHED_NORMAL` constants to the`os` module.
- gh-83662: Add missing `__class_getitem__` method to the Python
implementation of`functools.partial()` , to make it compatible with
the C version. This is mainly relevant for alternative Python
implementations like PyPy and GraalPy, because CPython will usually use
the C-implementation of that function.
- gh-127586: `multiprocessing.pool.Pool` now properly restores
blocked signal handlers of the parent thread when creating processes via
either*spawn* or*forkserver* .
- gh-98188: Fix an issue in `email.message.Message.get_payload()` where
data cannot be decoded if the Content Transfer Encoding mechanism contains
trailing whitespaces or additional junk text. Patch by Hui Liu.
- gh-127529: Correct behavior of `asyncio.selector_events.BaseSelectorEventLoop._accept_connection()` in handling`ConnectionAbortedError` in a loop. This improves
performance on OpenBSD.
- gh-127360: When a descriptive error message cannot be provided for an `ssl.SSLError` , the âunknown errorâ message now shows the internal
error code (as retrieved by`ERR_get_error` and similar OpenSSL
functions).
- gh-127196: Fix crash when dict with keys in invalid encoding were passed to several functions in `_interpreters` module.
- gh-124130: Fix a bug in matching regular expression `\B` in empty input
string. Now it is always the opposite of`\b` . To get an old behavior,
use`(?!\A\Z)\B` . To get a new behavior in old Python versions, use`(?!\b)` .
- gh-126639: `tempfile.NamedTemporaryFile` will now issue a`ResourceWarning` when it is finalized by the garbage collector
without being explicitly closed.
- gh-126624: Expose error code `XML_ERROR_NOT_STARTED` of Expat >=2.6.4
in`xml.parsers.expat.errors` .
- gh-126225: `getopt` and`optparse` are no longer marked as
deprecated. There are legitimate reasons to use one of these modules in
preference to`argparse` , and none of these modules are at risk of
being removed from the standard library. Of the three,`argparse` remains the recommended default choice,*unless* one of the concerns noted
at the top of the`optparse` module documentation applies.
- gh-124761: Add `SO_REUSEPORT_LB` constant to`socket` for FreeBSD.
- gh-121720: `enum.EnumDict` can now be used without resorting to
private API.
- gh-123424: Add `zipfile.ZipInfo._for_archive()` setting default
properties on`ZipInfo` objects. Patch by BÃ©nÃ©dikt Tran
and Jason R. Coombs.
- gh-121676: Deprecate calling the Python implementation of `functools.reduce()` with a`function` or`sequence` as a
keyword argument. This will be forbidden in Python 3.16 in order
to match the C implementation.
- gh-112015: `ctypes.memoryview_at()` now exists to create a`memoryview` object that refers to the supplied pointer and length.
This works like`ctypes.string_at()` except it avoids a buffer copy,
and is typically useful when implementing pure Python callback functions
that are passed dynamically-sized buffers.
- gh-95371: Added support for other image formats (PNG, PGM, and PPM) to the turtle module. Patch by Shin-myoung-serp.

### Core and BuiltinsÂ¶

- gh-128078: Fix a `SystemError` when using`anext()` with a
default tuple value. Patch by BÃ©nÃ©dikt Tran.
- gh-128717: Fix a crash when setting the recursion limit while other threads are active on the free threaded build.
- gh-124483: Treat `Py_DECREF` and variants as escaping when generating
opcode and uop metadata. This prevents the possibility of a`__del__` method causing the JIT to behave incorrectly.
- gh-126703: Improve performance of class methods by using a freelist.
- gh-128137: Update `PyASCIIObject` layout to handle interned field
with the atomic operation. Patch by Donghee Na.

### LibraryÂ¶

- gh-128192: Upgrade HTTP digest authentication algorithm for `urllib.request` by supporting SHA-256 digest authentication as
specified in**RFC 7616** .

### Core and BuiltinsÂ¶

- gh-126868: Increase usage of freelist for `int` allocation.
- gh-114203: Optimize `Py_BEGIN_CRITICAL_SECTION` for simple recursive
calls.
- gh-127705: Adds stackref debugging when `Py_STACKREF_DEBUG` is set.
Finds all double-closes and leaks, logging the origin and last borrow.Inspired by HPyâs debug mode. https://docs.hpyproject.org/en/latest/debug-mode.html
- gh-128079: Fix a bug where `except*` does not
properly check the return value of an`ExceptionGroup` âs`split()` function, leading to a crash in some
cases. Now when`split()` returns an invalid
object,`except*` raises a`TypeError` with
the original raised`ExceptionGroup` object chained to it.
- gh-128030: Avoid error from calling `PyModule_GetFilenameObject` on a
non-module object when importing a non-existent symbol from a non-module
object.

### LibraryÂ¶

- gh-128035: Indicate through `ssl.HAS_PHA` whether the`ssl` module supports TLSv1.3 post-handshake client authentication (PHA). Patch
by Will Childs-Klein.

### Core and BuiltinsÂ¶

- gh-127274: Add a new flag, `CO_METHOD` , to`co_flags` that indicates whether the code object belongs to a function defined in
class scope.
- gh-66409: During the path initialization, we now check if `base_exec_prefix` is the same as`base_prefix` before
falling back to searching the Python interpreter directory.
- gh-127970: We now use the location of the `libpython` runtime library
used in the current process to determine`sys.base_prefix` on all
platforms implementing the dladdr
function defined by the UNIX standard â this includes Linux, Android,
macOS, iOS, FreeBSD, etc. This was already the case on Windows and macOS
Framework builds.
- gh-127773: Do not use the type attribute cache for types with incompatible MRO.
- gh-127903: `Objects/unicodeobject.c` : fix a crash on DEBUG builds in`_copy_characters` when there is nothing to copy.
- gh-127809: Fix an issue where the experimental JIT may infer an incorrect result type for exponentiation ( `**` and`**=` ), leading to bugs or
crashes.
- gh-126862: Fix a possible overflow when a class inherits from an absurd number of super-classes. Reported by Valery Fedorenko. Patch by BÃ©nÃ©dikt Tran.

### C APIÂ¶

- gh-128400: `Py_FatalError()` no longer shows all threads on the
free threaded build to prevent crashes.
- gh-128629: Add macros `Py_PACK_VERSION()` and`Py_PACK_FULL_VERSION()` for bit-packing Python version numbers.
- gh-128008: Add `PyWeakref_IsDead()` function, which tests if a weak
reference is dead.
- gh-127350: Add `Py_fopen()` function to open a file. Similar to the`fopen()` function, but the*path* parameter is a Python object and
an exception is set on error. Add also`Py_fclose()` function to
close a file, function needed for Windows support. Patch by Victor
Stinner.

### BuildÂ¶

- gh-128627: For Emscripten builds the function pointer cast call trampoline now uses the wasm-gc ref.test instruction if itâs available instead of Wasm JS type reflection.
- gh-128472: Skip BOLT optimization of functions using computed gotos, fixing errors on build with LLVM 19.
- gh-115765: GNU Autoconf 2.72 is now required to generate `configure` . Patch by Erlend Aasland.
- gh-123925: Fix building the `curses` module on platforms with
libncurses but without libncursesw.
- gh-90905: Add support for cross-compiling to x86_64 on aarch64/arm64 macOS.
- gh-128321: Set `LIBS` instead of`LDFLAGS` when checking if`sqlite3` library functions are available. This fixes the ordering of
linked libraries during checks, which was incorrect when using a
statically linked`libsqlite3` .
- gh-100384: Error on `unguarded-availability` in macOS builds, preventing
invalid use of symbols that are not available in older versions of the OS.
- gh-128104: Remove `Py_STRFTIME_C99_SUPPORT` conditions in favor of
requiring C99*strftime(3)* specifier support at build time. When
cross-compiling, there is no build time check and support is assumed.
- gh-127951: Add option `--pystats` to the Windows build to enable
performance statistics collection.

## Python 3.14.0 alpha 3Â¶

*Release date: 2024-12-17*

### WindowsÂ¶

### Tools/DemosÂ¶

### TestsÂ¶

- gh-127906: Test the limited C API in test_cppext. Patch by Victor Stinner.
- gh-127637: Add tests for the `dis` command-line interface. Patch by
BÃ©nÃ©dikt Tran.
- gh-126925: iOS test results are now streamed during test execution, and the deprecated xcresulttool is no longer used.
- gh-127076: Disable strace based system call tests when LD_PRELOAD is set.
- gh-127076: Filter out memory-related `mmap` ,`munmap` , and`mprotect` calls from file-related ones when testing`io` behavior
using strace.

### SecurityÂ¶

- gh-127655: Fixed the `asyncio.selector_events._SelectorSocketTransport` transport not
pausing writes for the protocol when the buffer reaches the high water
mark when using`asyncio.WriteTransport.writelines()` .

### LibraryÂ¶

- gh-126907: Fix crash when using `atexit` concurrently on the
free-threaded build.
- gh-127870: Detect recursive calls in ctypes `_as_parameter_` handling.
Patch by Victor Stinner.
- gh-127732: The `platform` module now correctly detects Windows Server
2025.
- gh-126789: Fixed `sysconfig.get_config_vars()` ,`sysconfig.get_paths()` , and siblings, returning outdated cached data
if the value  of`sys.prefix` or`sys.exec_prefix` changes.
Overwriting`sys.prefix` or`sys.exec_prefix` still is
discouraged, as that might break other parts of the code.
- gh-127718: Add colour to `test.regrtest` output. Patch by Hugo van
Kemenade.
- gh-127610: Added validation for more than one var-positional or var-keyword parameters in `inspect.Signature` . Patch by Maxim
Ageev.
- gh-127627: Added `posix._emscripten_debugger()` to help with debugging
the test suite on the Emscripten target.
- gh-126821: macOS and iOS apps can now choose to redirect stdout and stderr to the system log during interpreter configuration.
- gh-93312: Include `<sys/pidfd.h>` to get`os.PIDFD_NONBLOCK` constant.
Patch by Victor Stinner.
- gh-127481: Add the `EPOLLWAKEUP` constant to the`select` module.
- gh-127065: Make `operator.methodcaller()` thread-safe and re-entrant
safe.
- gh-127321: `pdb.set_trace()` will not stop at an opcode that does not
have an associated line number anymore.
- gh-127429: Fixed bug where, on cross-builds, the `sysconfig` POSIX
data was being generated with the host Pythonâs`Makefile` . The data is
now generated from current buildâs`Makefile` .
- gh-127413: Add the `dis --specialized` command-line option to show
specialized bytecode. Patch by BÃ©nÃ©dikt Tran.
- gh-125413: Revert addition of `pathlib.Path.scandir()` . This method
was added in 3.14.0a2. The optimizations remain for file system paths, but
other subclasses should only have to implement`pathlib.Path.iterdir()` .
- gh-127257: In `ssl` , system call failures that OpenSSL reports using`ERR_LIB_SYS` are now raised as`OSError` .
- gh-59705: On Linux, `threading.Thread` now sets the thread name to
the operating system. Patch by Victor Stinner.
- gh-127303: Publicly expose `EXACT_TOKEN_TYPES` in`token.__all__` .
- gh-127331: `ssl` can show descriptions for errors added in OpenSSL
3.4.
- gh-123967: Fix faulthandler for trampoline frames. If the top-most frame is a trampoline frame, skip it. Patch by Victor Stinner.
- gh-127178: A `_sysconfig_vars_(...).json` file is now shipped in the
standard library directory. It contains the output of`sysconfig.get_config_vars()` on the default environment encoded as
JSON data. This is an implementation detail, and may change at any time.
- gh-127072: Remove outdated `socket.NETLINK_*` constants not present in
Linux kernels beyond 2.6.17.
- gh-127255: The `CopyComPointer()` function is now public.
Previously, this was private and only available in`_ctypes` .
- gh-127182: Fix `io.StringIO.__setstate__()` crash, when`None` was passed as the first value.
- gh-127217: Fix `urllib.request.pathname2url()` for paths starting with
multiple slashes on Posix.
- gh-125866: `urllib.request.pathname2url()` now adds an empty authority
when generating a URL for a path that begins with exactly one slash. For
example, the path`/etc/hosts` is converted to the scheme-less URL`///etc/hosts` . As a result of this change, URLs without authorities are
only generated for relative paths.
- gh-127221: Add colour to `unittest` output. Patch by Hugo van
Kemenade.
- gh-127035: Fix `shutil.which` on Windows. Now it looks at direct
match if and only if the command ends with a PATHEXT extension or X_OK is
not in mode. Support extensionless files if â.â is in PATHEXT. Support
PATHEXT extensions that end with a dot.
- gh-122273: Support PyREPL history on Windows. Patch by devdanzin and Victor Stinner.
- gh-125866: `urllib.request.pathname2url()` and`url2pathname()` no longer convert Windows drive
letters to uppercase.
- gh-127078: Fix issue where `urllib.request.url2pathname()` failed to
discard an extra slash before a UNC drive in the URL path on Windows.
- gh-126766: Fix issue where `urllib.request.url2pathname()` failed to
discard any âlocalhostâ authority present in the URL.
- gh-127065: Fix crash when calling a `operator.methodcaller()` instance
from multiple threads in the free threading build.
- gh-127090: Fix value of `urllib.response.addinfourl.url` for`file:` URLs that express relative paths and absolute Windows paths. The
canonical URL generated by`urllib.request.pathname2url()` is now
used.
- gh-126992: Fix LONG and INT opcodes to only use base 10 for string to integer conversion in `pickle` .
- gh-126997: Fix support of STRING and GLOBAL opcodes with non-ASCII arguments in `pickletools` .`pickletools.dis()` now outputs
non-ASCII bytes in STRING, BINSTRING and SHORT_BINSTRING arguments as
escaped (`\xXX` ).
- gh-126316: `grp` : Make`grp.getgrall()` thread-safe by adding a
mutex. Patch by Victor Stinner.
- gh-126618: Fix the representation of `itertools.count` objects when
the count value is`sys.maxsize` .
- gh-126615: The `COMError` exception is now public.
Previously, this was private and only available in`_ctypes` .
- gh-126985: When running under a virtual environment with the `site` disabled (see`-S` ),`sys.prefix` and`sys.base_prefix` will now point to the virtual environment, instead
of the base installation.
- gh-112192: In the `trace` module, increase the coverage precision
(`cov%` ) to one decimal.
- gh-118761: Improve import time of `mimetypes` by around 11-16 times.
Patch by Hugo van Kemenade.
- gh-126947: Raise `TypeError` in`_pydatetime.timedelta.__new__()` if the passed arguments are not`int` or`float` , so that the Python implementation is in
line with the C implementation.
- gh-126946: Improve the `GetoptError` error message when a
long option prefix matches multiple accepted options in`getopt.getopt()` and`getopt.gnu_getopt()` .
- gh-126899: Make tkinter widget methods `after()` and`after_idle()` accept arguments passed by keyword.
- gh-85168: Fix issue where `urllib.request.url2pathname()` and`pathname2url()` always used UTF-8 when quoting and
unquoting file URIs. They now use the filesystem encoding and error
handler.
- gh-126780: Fix `os.path.normpath()` for drive-relative paths on
Windows.
- gh-126775: Make `linecache.checkcache()` thread safe and GC
re-entrancy safe.
- gh-126601: Fix issue where `urllib.request.pathname2url()` raised`OSError` when given a Windows path containing a colon character not
following a drive letter, such as before an NTFS alternate data stream.
- gh-126727: `locale.nl_langinfo(locale.ERA)` now returns multiple era
description segments separated by semicolons. Previously it only returned
the first segment on platforms with Glibc.
- gh-85046: Add `EHWPOISON` error code to`errno` .
- gh-118201: Fixed intermittent failures of `os.confstr` ,`os.pathconf` and`os.sysconf` on iOS and Android.
- gh-86463: The `usage` parameter of`argparse.ArgumentParser` no
longer affects the default value of the`prog` parameter in subparsers.
- gh-124008: Fix possible crash (in debug build), incorrect output or returning incorrect value from raw binary `write()` when writing to
console on Windows.
- gh-123401: The `http.cookies` module now supports parsing obsolete**RFC 850** date formats, in accordance with**RFC 9110** requirements.
Patch by Nano Zheng.
- gh-122431: `readline.append_history_file()` now raises a`ValueError` when given a negative value.
- gh-122356: Guarantee that the position of a file-like object passed to `zipfile.is_zipfile()` is left untouched after the call. Patch by
BÃ©nÃ©dikt Tran.
- gh-122288: Improve the performances of `fnmatch.translate()` by a
factor 1.7. Patch by BÃ©nÃ©dikt Tran.
- gh-88110: Fixed `multiprocessing.Process` reporting a`.exitcode` of 1 even on success when using the`"fork"` start method while using a`concurrent.futures.ThreadPoolExecutor` .
- gh-97514: Authentication was added to the `multiprocessing` forkserver start method control socket so that only processes with the
authentication key generated by the process that spawned the forkserver
can control it.  This is an enhancement over the other gh-97514 fixes
so that access is no longer limited only by filesystem permissions.The file descriptor exchange of control pipes with the forked worker process now requires an explicit acknowledgement byte to be sent over the socket after the exchange on all forkserver supporting platforms. That makes testing the above much easier.

### DocumentationÂ¶

- gh-127347: Publicly expose `traceback.print_list()` in`traceback.__all__` .

### Core and BuiltinsÂ¶

- gh-127740: Fix error message in `bytes.fromhex()` when given an odd
number of digits to properly indicate that an even number of hexadecimal
digits is required.
- gh-127058: `PySequence_Tuple` now creates the resulting tuple
atomically, preventing partially created tuples being visible to the
garbage collector or through`gc.get_referrers()`
- gh-127599: Fix statistics for increments of object reference counts (in particular, when a reference count was increased by more than 1 in a single operation).
- gh-127651: When raising `ImportError` for missing symbols in`from` imports, use`__file__` in the error message if`__spec__.origin` is
not a location
- gh-127582: Fix non-thread-safe object resurrection when calling finalizers and watcher callbacks in the free threading build.
- gh-127434: The iOS compiler shims can now accept arguments with spaces.
- gh-127536: Add missing locks around some list assignment operations in the free threading build.
- gh-127085: Fix race when exporting a buffer from a `memoryview` object on the free-threaded build.
- gh-127238: Correct error message for `sys.set_int_max_str_digits()` .
- gh-113841: Fix possible undefined behavior division by zero in `complex` âs`_Py_c_pow()` .

### LibraryÂ¶

- gh-127133: Calling `argparse.ArgumentParser.add_argument_group()` on
an argument group, and calling`argparse.ArgumentParser.add_argument_group()` or`argparse.ArgumentParser.add_mutually_exclusive_group()` on a mutually
exclusive group now raise exceptions. This nesting was never supported,
often failed to work correctly, and was unintentionally exposed through
inheritance. This functionality has been deprecated since Python 3.11.

### Core and BuiltinsÂ¶

- gh-126491: Add a marking phase to the GC. All objects that can be transitively reached from builtin modules or the stacks are marked as reachable before cycle detection. This reduces the amount of work done by the GC by approximately half.
- gh-127020: Fix a crash in the free threading build when `PyCode_GetCode()` ,`PyCode_GetVarnames()` ,`PyCode_GetCellvars()` , or`PyCode_GetFreevars()` were called
from multiple threads at the same time.
- gh-127010: Simplify GC tracking of dictionaries. All dictionaries are tracked when created, rather than being lazily tracked when a trackable object was added to them. This simplifies the code considerably and results in a slight speedup.
- gh-126980: Fix `__buffer__()` of`bytearray` crashing
when`READ` or`WRITE` are passed as flags.
- gh-126937: Fix `TypeError` when a`ctypes.Structure` has a
field size that doesnât fit into an unsigned 16-bit integer. Instead, the
maximum number of*bits* is`sys.maxsize` .
- gh-126868: Increase performance of `int` by adding a freelist for
compact ints.
- gh-126881: Fix crash in finalization of dtoa state. Patch by Kumar Aditya.
- gh-126892: Require cold or invalidated code to âwarm upâ before being JIT compiled again.
- gh-126091: Ensure stack traces are complete when throwing into a generator chain that ends in a custom generator.
- gh-126024: Optimize decoding of short UTF-8 sequences containing non-ASCII characters by approximately 15%.
- gh-125420: Add `memoryview.index()` to`memoryview` objects.
Patch by BÃ©nÃ©dikt Tran.
- gh-125420: Add `memoryview.count()` to`memoryview` objects.
Patch by BÃ©nÃ©dikt Tran.
- gh-124470: Fix crash in free-threaded builds when replacing object dictionary while reading attribute on another thread
- gh-69639: Implement mixed-mode arithmetic rules combining real and complex numbers as specified by C standards since C99. Patch by Sergey B Kirpichev.
- gh-120010: Correct invalid corner cases which resulted in `(nan+nanj)` output in complex multiplication, e.g.,`(1e300+1j)*(nan+infj)` .  Patch
by Sergey B Kirpichev.
- gh-109746: If `_thread.start_new_thread()` fails to start a new
thread, it deletes its state from interpreter and thus avoids its repeated
cleanup on finalization.

### C APIÂ¶

- gh-127896: The previously undocumented function `PySequence_In()` is
soft deprecated. Use`PySequence_Contains()` instead.
- gh-127791: Fix loss of callbacks after more than one call to `PyUnstable_AtExit()` .
- gh-127691: The Unicode Exception Objects C API now raises a `TypeError` if its exception argument is not a`UnicodeError` object. Patch by BÃ©nÃ©dikt Tran.
- gh-123378: Ensure that the value of `UnicodeEncodeError.end` retrieved by`PyUnicodeEncodeError_GetEnd()` lies in`[min(1, objlen), max(min(1, objlen), objlen)]` where*objlen* is the length of`UnicodeEncodeError.object` .
Similar arguments apply to`UnicodeDecodeError` and`UnicodeTranslateError` and their corresponding C interface. Patch by
BÃ©nÃ©dikt Tran.
- gh-127314: Improve error message when calling the C API without an active thread state on the free-threaded build.
- gh-123378: Ensure that the value of `UnicodeEncodeError.start` retrieved by`PyUnicodeEncodeError_GetStart()` lies in`[0, max(0, objlen - 1)]` where*objlen* is the length of`UnicodeEncodeError.object` . Similar arguments
apply to`UnicodeDecodeError` and`UnicodeTranslateError` and
their corresponding C interface. Patch by BÃ©nÃ©dikt Tran.
- gh-109523: Reading text from a non-blocking stream with `read` may now
raise a`BlockingIOError` if the operation cannot immediately return
bytes.
- gh-102471: Add a new import and export API for Python `int` objects
(**PEP 757** ):Patch by Victor Stinner.
- gh-121058: `PyThreadState_Clear()` now warns (and calls`sys.excepthook` ) if the thread state still has an active exception.

### BuildÂ¶

- gh-127865: Fix build failure on systems without thread-locals support.
- gh-127629: Emscripten builds now include ctypes support.
- gh-127111: Updated the Emscripten web example to use ES6 modules and be built into a distinct `web_example` subfolder.
- gh-115869: Make `jit_stencils.h` (which is produced during JIT builds)
reproducible.
- gh-126898: The Emscripten build of Python is now based on ES6 modules.

## Python 3.14.0 alpha 2Â¶

*Release date: 2024-11-19*

### WindowsÂ¶

- gh-126911: Update credits command output.
- gh-118973: Ensures the experimental free-threaded install includes the `_tkinter` module. The optional Tcl/Tk component must also be installed
in order for the module to work.
- gh-126497: Fixes venv failure due to missing redirector executables in experimental free-threaded installs.
- gh-126074: Removed unnecessary DLLs from Windows embeddable package
- gh-125315: Avoid crashing in `platform` due to slow WMI calls on some
Windows machines.
- gh-126084: Fix venvwlauncher to launch pythonw instead of python so no extra console window is created.
- gh-125842: Fix a `SystemError` when`sys.exit()` is called with`0xffffffff` on Windows.
- gh-125550: Enable the Python install manager to detect Python 3.14 installs from the Windows Store.
- gh-123803: All Windows code pages are now supported as âcpXXXâ codecs on Windows.

### Tools/DemosÂ¶

### TestsÂ¶

### SecurityÂ¶

- gh-126623: Upgrade libexpat to 2.6.4

### LibraryÂ¶

- gh-85957: Add missing MIME types for images with RFCs: emf, fits, g3fax, jp2, jpm, jpx, t38, tiff-fx and wmf. Patch by Hugo van Kemenade.
- gh-126920: Fix the `prefix` and`exec_prefix` keys from`sysconfig.get_config_vars()` incorrectly having the same value as`sys.base_prefix` and`sys.base_exec_prefix` ,
respectively, inside virtual environments. They now accurately reflect`sys.prefix` and`sys.exec_prefix` .
- gh-67877: Fix memory leaks when `regular expression` matching
terminates abruptly, either because of a signal or because memory
allocation fails.
- gh-125063: `marshal` now supports`slice` objects. The marshal
format version was increased to 5.
- gh-126789: Fixed the values of `sysconfig.get_config_vars()` ,`sysconfig.get_paths()` , and their siblings when the`site` initialization happens after`sysconfig` has built a cache for`sysconfig.get_config_vars()` .
- gh-126188: Update bundled pip to 24.3.1
- gh-126766: Fix issue where `urllib.request.url2pathname()` failed to
discard two leading slashes introducing an empty authority section.
- gh-126705: Allow `os.PathLike` to be a base for Protocols.
- gh-126699: Allow `collections.abc.AsyncIterator` to be a base for
Protocols.
- gh-126654: Fix crash when non-dict was passed to several functions in `_interpreters` module.
- gh-104745: Limit starting a patcher (from `unittest.mock.patch()` or`unittest.mock.patch.object()` ) more than once without stopping it
- gh-126595: Fix a crash when instantiating `itertools.count` with an
initial count of`sys.maxsize` on debug builds. Patch by BÃ©nÃ©dikt
Tran.
- gh-120423: Fix issue where `urllib.request.pathname2url()` mishandled
Windows paths with embedded forward slashes.
- gh-126565: Improve performances of `zipfile.Path.open()` for
non-reading modes.
- gh-126505: Fix bugs in compiling case-insensitive ```
regular
expressions
```
with character classes containing non-BMP characters:
upper-case non-BMP character did was ignored and the ASCII flag was
ignored when matching a character range whose upper bound is beyond the
BMP region.
- gh-117378: Fixed the `multiprocessing``"forkserver"` start method
forkserver process to correctly inherit the parentâs`sys.path` during the importing of`multiprocessing.set_forkserver_preload()` modules in the same manner as`sys.path` is configured in workers
before executing work items.This bug caused some forkserver module preloading to silently fail to preload. This manifested as a performance degradation in child processes when the `sys.path` was required due to additional repeated work in
every worker.It could also have a side effect of `""` remaining in`sys.path` during forkserver preload imports instead of the absolute path from`os.getcwd()` at multiprocessing import time used in the worker`sys.path` .The `sys.path` differences between phases in the child process could
potentially have caused preload to import incorrect things from the wrong
location.  We are unaware of that actually having happened in practice.
- gh-125679: The `multiprocessing.Lock` and`multiprocessing.RLock``repr` values no longer say âunknownâ on
macOS.
- gh-126476: Raise `calendar.IllegalMonthError` (now a subclass of`IndexError` ) for`calendar.month()` when the input month is
not correct.
- gh-126489: The Python implementation of `pickle` no longer calls`pickle.Pickler.persistent_id()` for the result of`persistent_id()` .
- gh-126451: Register the `contextvars.Context` type to`collections.abc.Mapping` .
- gh-126175: Add `msg` ,`doc` ,`pos` ,`lineno` and`colno` attributes to`tomllib.TOMLDecodeError` . Deprecate instantiating with
free-form arguments.
- gh-89416: Add **RFC 9559** MIME types for Matroska audiovisual container
formats. Patch by Hugo van Kemenade.
- gh-126417: Register the `multiprocessing.managers.DictProxy` and`multiprocessing.managers.ListProxy` types in`multiprocessing.managers` to`collections.abc.MutableMapping` and`collections.abc.MutableSequence` , respectively.
- gh-126390: Add support for returning intermixed options and non-option arguments in order in `getopt.gnu_getopt()` .
- gh-126374: Add support for options with optional arguments in the `getopt` module.
- gh-126363: Speed up pattern parsing in `pathlib.Path.glob()` by
skipping creation of a`pathlib.Path` object for the pattern.
- gh-126353: `asyncio.get_event_loop()` now does not implicitly creates
an event loop. It now raises a`RuntimeError` if there is no set
event loop. Patch by Kumar Aditya.
- gh-126313: Fix an issue in `curses.napms()` when`curses.initscr()` has not yet been called. Patch by BÃ©nÃ©dikt Tran.
- gh-126303: Fix pickling and copying of `os.sched_param` objects.
- gh-126138: Fix a use-after-free crash on `asyncio.Task` objects
whose underlying coroutine yields an object that implements an evil`__getattribute__()` . Patch by Nico Posada.
- gh-120057: Replace the `os.environ.refresh()` method with a new`os.reload_environ()` function. Patch by Victor Stinner.
- gh-126220: Fix crash in `cProfile.Profile` and`_lsprof.Profiler` when their callbacks were directly called with
0 arguments.
- gh-126212: Fix issue where `urllib.request.pathname2url()` and`url2pathname()` removed slashes from Windows DOS
drive paths and URLs.
- gh-126223: Raise a `UnicodeEncodeError` instead of a`SystemError` upon calling`_interpreters.create()` with an
invalid Unicode character.
- gh-126205: Fix issue where `urllib.request.pathname2url()` generated
URLs beginning with four slashes (rather than two) when given a Windows
UNC path.
- gh-126156: Improved performances of creating `Morsel` objects by a factor of 3.8x.
- gh-126105: Fix a crash in `ast` when the`ast.AST._fields` attribute is deleted.
- gh-126106: Fixes a possible `NULL` pointer dereference in`ssl` .
- gh-126080: Fix a use-after-free crash on `asyncio.Task` objects for
which the underlying event loop implements an evil`__getattribute__()` . Reported by Nico-Posada. Patch by
BÃ©nÃ©dikt Tran.
- gh-125322: Correct detection of complex numbers support in libffi.
- gh-126083: Fixed a reference leak in `asyncio.Task` objects when
reinitializing the same object with a non-`None` context. Patch by Nico
Posada.
- gh-126068: Fix exceptions in the `argparse` module so that only error
messages for ArgumentError and ArgumentTypeError are now translated.
ArgumentError is now only used for command line errors, not for logical
errors in the program. TypeError is now raised instead of ValueError for
some logical errors.
- gh-125413: Add `pathlib.Path.scandir()` method to efficiently fetch
directory children and their file attributes. This is a trivial wrapper of`os.scandir()` .
- gh-125984: Fix use-after-free crashes on `asyncio.Future` objects
for which the underlying event loop implements an evil`__getattribute__()` . Reported by Nico-Posada. Patch by
BÃ©nÃ©dikt Tran.
- gh-125926: Fix `urllib.parse.urljoin()` for base URI with undefined
authority. Although**RFC 3986** only specify reference resolution for
absolute base URI,`urljoin()` should continue to return sensible
result for relative base URI.
- gh-125969: Fix an out-of-bounds crash when an evil `asyncio.loop.call_soon()` mutates the length of the internal
callbacks list. Patch by BÃ©nÃ©dikt Tran.
- gh-125966: Fix a use-after-free crash in `asyncio.Future.remove_done_callback()` . Patch by BÃ©nÃ©dikt Tran.
- gh-125789: Fix possible crash when mutating list of callbacks returned by `asyncio.Future._callbacks` . It now always returns a new copy in C
implementation`_asyncio` . Patch by Kumar Aditya.
- gh-126916: Allow the *initial* parameter of`functools.reduce()` to be
passed as a keyword argument. Patch by Sayandip Dutta.
- gh-124452: Fix an issue in `email.policy.EmailPolicy.header_source_parse()` and`email.policy.Compat32.header_source_parse()` that introduced spurious
leading whitespaces into header values when the header includes a newline
character after the header name delimiter (`:` ) and before the value.
- gh-117941: `argparse.BooleanOptionalAction` now rejects option
names starting with`--no-` .
- gh-125884: Fixed the bug for `pdb` where it canât set breakpoints on
functions with certain annotations.
- gh-125355: Fix several bugs in `argparse.ArgumentParser.parse_intermixed_args()` .
  - The parser no longer changes temporarily during parsing.
  - Default values are not processed twice.
  - Required mutually exclusive groups containing positional arguments are now supported.
  - The missing arguments report now includes the names of all required optional and positional arguments.
  - Unknown options can be intermixed with positional arguments in parse_known_intermixed_args().
- gh-125767: `super` objects are now`pickleable` and`copyable` .
- gh-124969: `locale.nl_langinfo(locale.ALT_DIGITS)` now returns a string
again. The returned value consists of up to 100 semicolon-separated
symbols.
- gh-84850: Remove `URLopener` and`FancyURLopener` classes
from`urllib.request` . They had previously raised`DeprecationWarning` since Python 3.3.
- gh-125666: Avoid the exiting the interpreter if a null byte is given as input in the new REPL.
- gh-125710: [Enum] fix hashable<->nonhashable comparisons for member values
- gh-125631: Restore ability to set `persistent_id` and`persistent_load` attributes of instances of
the`Pickler` and`Unpickler` classes in the`pickle` module.
- gh-125378: Fixed the bug in `pdb` where after a multi-line command,
an empty line repeats the first line of the multi-line command, instead of
the full command.
- gh-125682: Reject non-ASCII digits in the Python implementation of `json.loads()` conforming to the JSON specification.
- gh-125660: Reject invalid unicode escapes for Python implementation of `json.loads()` .
- gh-52551: Use `wcsftime()` to implement`time.strftime()` on
Windows.
- gh-125259: Fix the notes removal logic for errors thrown in enum initialization.
- gh-125633: Add function `inspect.ispackage()` to determine whether an
object is a package or not.
- gh-125614: In the `FORWARDREF` format of`annotationlib` , fix bug where nested expressions were not returned
as`annotationlib.ForwardRef` format.
- gh-125590: Allow `FrameLocalsProxy` to delete and pop if the key is not
a fast variable.
- gh-125600: Only show stale code warning in `pdb` when we display
source code.
- gh-125542: Deprecate passing keyword-only *prefix_chars* argument to`argparse.ArgumentParser.add_argument_group()` .
- gh-125541: Pressing `Ctrl` -`C` while blocked in`threading.Lock.acquire()` ,`threading.RLock.acquire()` , and`threading.Thread.join()` now interrupts the function call and raises
a`KeyboardInterrupt` exception on Windows, similar to how those
functions behave on macOS and Linux.
- gh-125519: Improve traceback if `importlib.reload()` is called with an
object that is not a module. Patch by Alex Waygood.
- gh-125451: Fix deadlock when `concurrent.futures.ProcessPoolExecutor` shuts down concurrently
with an error when feeding a job to a worker process.
- gh-125115: Fixed a bug in `pdb` where arguments starting with`-` canât be passed to the debugged script.
- gh-125398: Fix the conversion of the `VIRTUAL_ENV` path in the
activate script in`venv` when running in Git Bash for Windows.
- gh-125245: Fix race condition when importing `collections.abc` , which
could incorrectly return an empty module.
- gh-52551: Fix encoding issues in `time.strftime()` , the`strftime()` method of the`datetime` classes`datetime` ,`date` and`time` and formatting of these classes. Characters not
encodable in the current locale are now acceptable in the format string.
Surrogate pairs and sequence of surrogatescape-encoded bytes are no longer
recombinated. Embedded null character no longer terminates the format
string.
- gh-124984: Fixed thread safety in `ssl` in the free-threaded build.
OpenSSL operations are now protected by a per-object lock.
- gh-124651: Properly quote template strings in `venv` activation
scripts.
- gh-124694: Weâve added `concurrent.futures.InterpreterPoolExecutor` , which allows you to
run code in multiple isolated interpreters.  This allows you to circumvent
the limitations of CPU-bound threads (due to the GIL). Patch by Eric Snow.This addition is unrelated to **PEP 734** .
- gh-58032: Deprecate the `argparse.FileType` type converter.
- gh-99749: Adds a feature to optionally enable suggestions for argument choices and subparser names if mistyped by the user.
- gh-58956: Fixed a bug in `pdb` where sometimes the breakpoint wonât
trigger if it was set on a function which is already in the call stack.
- gh-124111: The tkinter module can now be built to use either the new version 9.0.0 of Tcl/Tk or the latest release 8.6.15 of Tcl/Tk 8. Tcl/Tk 9 includes many improvements, both to the Tcl language and to the appearance and utility of the graphical user interface provided by Tk.
- gh-80958: unittest discovery supports PEP 420 namespace packages as start directory again.
- gh-123370: Fix the canvas not clearing after running turtledemo clock.
- gh-89083: Add `uuid.uuid8()` for generating UUIDv8 objects as
specified in**RFC 9562** . Patch by BÃ©nÃ©dikt Tran
- gh-122549: Add `platform.invalidate_caches()` to invalidate cached
results.
- gh-120754: Update unbounded `read` calls in`zipfile` to specify an
explicit`size` putting a limit on how much data they may read. This
also updates handling around ZIP max comment size to match the standard
instead of reading comments that are one byte too long.
- gh-121267: Improve the performance of `tarfile` when writing files,
by caching user names and group names.
- gh-70764: Fixed an issue where `inspect.getclosurevars()` would
incorrectly classify an attribute name as a global variable when the name
exists both as an attribute name and a global variable.
- gh-118289: `posixpath.realpath()` now raises`NotADirectoryError` when*strict* mode is enabled and a
non-directory path with a trailing slash is supplied.
- gh-119826: Always return an absolute path for `os.path.abspath()` on
Windows.
- gh-97850: Remove deprecated `pkgutil.get_loader()` and`pkgutil.find_loader()` .
- gh-118986: Add `socket.IPV6_RECVERR` constant (available since
Linux 2.2).
- gh-116897: Accepting objects with false values (like `0` and`[]` )
except empty strings, byte-like objects and`None` in`urllib.parse` functions`parse_qsl()` and`parse_qs()` is now deprecated.
- gh-101955: Fix SystemError when match regular expression pattern containing some combination of possessive quantifier, alternative and capture group.
- gh-71936: Fix a race condition in `multiprocessing.pool.Pool` .
- bpo-46128: Strip `unittest.IsolatedAsyncioTestCase` stack frames
from reported stacktraces.
- gh-84852: Add MIME types for MS Embedded OpenType, OpenType Layout, TrueType, WOFF 1.0 and 2.0 fonts. Patch by Sahil Prajapati and Hugo van Kemenade.

### DocumentationÂ¶

- gh-126622: Added stub pages for removed modules explaining their removal, where to find replacements, and linking to the last Python version that supported them. Contributed by Ned Batchelder.
- gh-125277: Require Sphinx 7.2.6 or later to build the Python documentation. Patch by Adam Turner.
- gh-60712: Include the `object` type in the lists of documented
types. Change by Furkan Onder and Martin Panter.

### Core and BuiltinsÂ¶

- gh-126795: Increase the threshold for JIT code warmup. Depending on platform and workload, this can result in performance gains of 1-9% and memory savings of 3-5%.
- gh-126341: Now `ValueError` is raised instead of`SystemError` when trying to iterate over a released`memoryview` object.
- gh-126688: Fix a crash when calling `os.fork()` on some operating
systems, including SerenityOS.

### LibraryÂ¶

### Core and BuiltinsÂ¶

### LibraryÂ¶

- gh-126209: Fix an issue with `skip_file_prefixes` parameter which
resulted in an inconsistent behaviour between the C and Python
implementations of`warnings.warn()` . Patch by Daehee Kim.

### Core and BuiltinsÂ¶

- gh-126312: Fix crash during garbage collection on an object frozen by `gc.freeze()` on the free-threaded build.
- gh-103951: Relax optimization requirements to allow fast attribute access to module subclasses.
- gh-126072: Following gh-126101, for Code Objects like lambda, annotation and type alias, we no longer add `None` to its`co_consts` .
- gh-126195: Improve JIT performance by 1.4% on macOS Apple Silicon by using platform-specific memory protection APIs. Patch by Diego Russo.
- gh-126139: Provide better error location when attempting to use a future statement with an unknown future feature.
- gh-126072: Add a new attribute in `co_flags` to indicate
whether the first item in`co_consts` is the docstring.
If a code object has no docstring,`None` will**NOT** be inserted.
- gh-126076: Relocated objects such as `tuple` ,`bytes` and`str` objects are properly tracked by`tracemalloc` and its associated
hooks. Patch by Pablo Galindo.
- gh-90370: Avoid temporary tuple creation for vararg in argument passing with Argument Clinic generated code (if arguments either vararg or positional-only).
- gh-126018: Fix a crash in `sys.audit()` when passing a non-string as
first argument and Python was compiled in debug mode.
- gh-126012: The `memoryview` type now supports subscription, making
it a generic type.
- gh-125837: Adds `LOAD_SMALL_INT` and`LOAD_CONST_IMMORTAL` instructions.`LOAD_SMALL_INT` pushes a
small integer equal to the`oparg` to the stack.`LOAD_CONST_IMMORTAL` does the same as`LOAD_CONST` but is more efficient for immortal
objects. Removes`RETURN_CONST` instruction.
- gh-125942: On Android, the `errors` setting of`sys.stdout` was
changed from`surrogateescape` to`backslashreplace` .
- gh-125859: Fix a crash in the free threading build when `gc.get_objects()` or`gc.get_referrers()` is called during an
in-progress garbage collection.
- gh-125868: It was possible in 3.14.0a1 only for attribute lookup to give the wrong value. This was due to an incorrect specialization in very specific circumstances. This is fixed in 3.14.0a2.
- gh-125498: The JIT has been updated to leverage Clang 19âs new `preserve_none` attribute, which supports more platforms and is more
useful than LLVMâs existing`ghccc` calling convention. This also
removes the need to manually patch the calling convention in LLVM IR,
simplifying the JIT compilation process.
- gh-125703: Correctly honour `tracemalloc` hooks in specialized`Py_DECREF` paths. Patch by Pablo Galindo
- gh-125593: Use color to highlight error locations in traceback from exception group
- gh-125017: Fix crash on certain accesses to the `__annotations__` of`staticmethod` and`classmethod` objects.
- gh-125588: The Python PEG generator can now use f-strings in the grammar actions. Patch by Pablo Galindo
- gh-125444: Fix illegal instruction for older Arm architectures. Patch by Diego Russo, testing by Ross Burton.
- gh-118423: Add a new `INSTRUCTION_SIZE` macro to the cases generator
which returns the current instruction size.
- gh-125038: Fix crash when iterating over a generator expression after direct changes on `gi_frame.f_locals` . Patch by Mikhail Efimov.
- gh-124855: Donât allow the JIT and perf support to be active at the same time. Patch by Pablo Galindo
- gh-123714: Update JIT compilation to use LLVM 19
- gh-123930: Improve the error message when a script shadowing a module from the standard library causes `ImportError` to be raised during a
âfromâ import. Similarly, improve the error message when a script
shadowing a third party module attempts to âfromâ import an attribute from
that third party module while still initialising.
- gh-119793: The `map()` built-in now has an optional keyword-only*strict* flag like`zip()` to check that all the iterables are of
equal length. Patch by Wannes Boeykens.

### LibraryÂ¶

### C APIÂ¶

- gh-126554: Fix error handling in `ctypes.CDLL` objects which could
result in a crash in rare situations.
- gh-126061: Add `PyLong_IsPositive()` ,`PyLong_IsNegative()` and`PyLong_IsZero()` for checking if a`PyLongObject` is
positive, negative, or zero, respectively.
- gh-125608: Fix a bug where dictionary watchers (e.g., `PyDict_Watch()` ) on an objectâs attribute dictionary
(`__dict__` ) were not triggered when the objectâs attributes
were modified.
- gh-123619: Added the `PyUnstable_Object_EnableDeferredRefcount()` function for enabling**PEP 703** deferred reference counting.
- gh-121654: Add `PyType_Freeze()` function to make a type immutable.
Patch by Victor Stinner.
- gh-120026: The `Py_HUGE_VAL` macro is soft deprecated.

### BuildÂ¶

- gh-126691: Removed the `--with-emscripten-target` configure flag. We
unified the`node` and`browser` options and the same build can now be
used, independent of target runtime.
- gh-123877: Use `wasm32-wasip1` as the target triple for WASI instead of`wasm32-wasi` . The latter will eventually be reclaimed for WASI 1.0
while CPython currently only supports WASI preview1.
- gh-126458: Disable SIMD support for HACL under WASI.
- gh-89640: Hard-code float word ordering as little endian on WASM.
- gh-126206: `make clinic` now runs Argument Clinic using the`--force` option, thus forcefully regenerating generated code.
- gh-126187: Introduced `Tools/wasm/emscripten.py` to simplify doing
Emscripten builds.
- gh-124932: For cross builds, there is now support for having a different install `prefix` than the`host_prefix` used by`getpath.py` . This
is set to`/` by default for Emscripten, on other platforms the default
behavior is the same as before.
- gh-125946: The minimum supported Android version is now 7.0 (API level 24).
- gh-125940: The Android build now supports 16 KB page sizes.
- gh-89640: Improve detection of float word ordering on Linux when link-time optimizations are enabled.
- gh-124928: Emscripten builds now require node >= 18.
- gh-115382: Fix cross compile failures when the host and target SOABIs match.

## Python 3.14.0 alpha 1Â¶

*Release date: 2024-10-15*

### macOSÂ¶

### WindowsÂ¶

- gh-124487: Increases Windows required OS and API level to Windows 10.
- gh-124609: Fix `_Py_ThreadId` for Windows builds using MinGW. Patch by
Tony Roberts.
- gh-124448: Updated bundled Tcl/Tk to 8.6.15.
- gh-124254: Ensures experimental free-threaded binaries remain installed when updating.
- gh-123915: Ensure that `Tools\msi\buildrelease.bat` uses different
directories for AMD64 and ARM64 builds.
- gh-123418: Updated Windows build to use OpenSSL 3.0.15.
- gh-123476: Add support for `socket.TCP_QUICKACK` on Windows platforms.
- gh-122573: The Windows build of CPython now requires 3.10 or newer.
- gh-100256: `mimetypes` no longer fails when it encounters an
inaccessible registry key.
- gh-119679: Ensures correct import libraries are included in Windows installs.
- gh-119690: Adds Unicode support and fixes audit events for `_winapi.CreateNamedPipe` .
- gh-111201: Add support for new pyrepl on Windows
- gh-119070: Fixes `py.exe` handling of shebangs like```
/usr/bin/env
python3.12
```
, which were previously interpreted as`python3.exe` instead
of`python3.12.exe` .
- gh-117505: Fixes an issue with the Windows installer not running ensurepip in a fully isolated environment. This could cause unexpected interactions with the user site-packages.
- gh-118209: Avoid crashing in `mmap` on Windows when the mapped memory
is inaccessible due to file system errors or access violations.
- gh-79846: Makes `ssl.create_default_context()` ignore invalid
certificates in the Windows certificate store

### Tools/DemosÂ¶

- gh-123418: Update GitHub CI workflows to use OpenSSL 3.0.15 and multissltests to use 3.0.15, 3.1.7, and 3.2.3.

### TestsÂ¶

- gh-125041: Re-enable skipped tests for `zlib` on the s390x
architecture: only skip checks of the compressed bytes, which can be
different between zlibâs software implementation and the
hardware-accelerated implementation.
- gh-124378: Updated `test_ttk` to pass with Tcl/Tk 8.6.15.
- gh-124213: Detect whether the test suite is running inside a systemd-nspawn container with `--suppress-sync=true` option, and skip
the`test_os` and`test_mmap` tests that are failing in this scenario.
- gh-124190: Add capability to ignore entire files or directories in check warning CI tool
- gh-121921: Update `Lib/test/crashers/bogus_code_obj.py` so that it
crashes properly again.
- gh-112301: Add tooling to check for changes in compiler warnings. Patch by Nate Ohlson.
- gh-59022: Add tests for `pkgutil.extend_path()` . Patch by Andreas
Stocker.
- gh-99242: `os.getloadavg()` may throw`OSError` when running
regression tests under certain conditions (e.g. chroot). This error is now
caught and ignored, since reporting load average is optional.
- gh-121084: Fix test_typing random leaks. Clear typing ABC caches when running tests for refleaks ( `-R` option): call`_abc_caches_clear()` on typing abstract classes and their subclasses. Patch by Victor Stinner.
- gh-121160: Add a test for `readline.set_history_length()` . Note that
this test may fail on readline libraries.
- gh-121200: Fix `test_expanduser_pwd2()` of`test_posixpath` .  Call`getpwnam()` to get`pw_dir` , since it can be different than`getpwall()``pw_dir` . Patch by Victor Stinner.
- gh-121188: When creating the JUnit XML file, regrtest now escapes characters which are invalid in XML, such as the chr(27) control character used in ANSI escape sequences. Patch by Victor Stinner.
- gh-120801: Cleaned up fixtures for importlib.metadata tests and consolidated behavior with âtest.support.os_helperâ.
- gh-119727: Add `--single-process` command line option to Python test
runner (regrtest). Patch by Victor Stinner.
- gh-119273: Python test runner no longer runs tests using TTY (ex: test_ioctl) in a process group (using `setsid()` ). Previously, tests
using TTY were skipped. Patch by Victor Stinner.
- gh-119050: regrtest test runner: Add XML support to the refleak checker (-R option). Patch by Victor Stinner.
- gh-101525: Skip `test_gdb` if the binary is relocated by BOLT. Patch by
Donghee Na.
- gh-107562: Test certificates have been updated to expire far in the future. This allows testing Y2038 with system time set to after that, so that actual Y2038 issues can be exposed, and not masked by expired certificate errors.

### SecurityÂ¶

- gh-125140: Remove the current directory from `sys.path` when using
PyREPL.
- gh-123678: Upgrade libexpat to 2.6.3
- gh-112301: Enable compiler options that warn of potential security vulnerabilities.
- gh-122792: Changed IPv4-mapped `ipaddress.IPv6Address` to consistently
use the mapped IPv4 address value for deciding properties. Properties
which have their behavior fixed are`is_multicast` ,`is_reserved` ,`is_link_local` ,`is_global` , and`is_unspecified` .
- gh-112301: Add ability to ignore warnings per file with warning count in warning checking tooling. Patch by Nate Ohlson.
- gh-112301: Add macOS warning tracking to warning check tooling. Patch by Nate Ohlson.
- gh-122133: Authenticate the socket connection for the `socket.socketpair()` fallback on platforms where`AF_UNIX` is not
available like Windows.Patch by Gregory P. Smith <greg@krypto.org> and Seth Larson <seth@python.org>. Reported by Ellie <el@horse64.org>
- gh-121957: Fixed missing audit events around interactive use of Python, now also properly firing for `python -i` , as well as for```
python -m
asyncio
```
. The events in question are`cpython.run_stdin` and`cpython.run_startup` .
- gh-112301: Enable runtime protections for glibc to abort execution when unsafe behavior is encountered, for all platforms except Windows.
- gh-121285: Remove backtracking from tarfile header parsing for `hdrcharset` , PAX, and GNU sparse headers.
- gh-112301: Add default compiler options to improve security. Enable -Wimplicit-fallthrough, -fstack-protector-strong, -Wtrampolines.
- gh-118773: Fixes creation of ACLs in `os.mkdir()` on Windows to work
correctly on non-English machines.
- gh-118486: `os.mkdir()` on Windows now accepts*mode* of`0o700` to
restrict the new directory to the current user. This fixes**CVE 2024-4030** affecting`tempfile.mkdtemp()` in scenarios where the
base temporary directory is more permissive than the default.

### LibraryÂ¶

- gh-125422: Fixed the bug where `pdb` and`bdb` can step into the
bottom caller frame.
- gh-100141: Fixed the bug where `pdb` will be stuck in an infinite
loop when debugging an empty file.
- gh-53203: Fix `time.strptime()` for`%c` ,`%x` and`%X` formats
in many locales that use non-ASCII digits, like Persian, Burmese, Odia and
Shan.
- gh-125206: Workaround for old libffi versions is added. Module ctypes supports double complex only with libffi >= 3.3.0. Patch by Mikhail Efimov.
- gh-89967: Make `WeakKeyDictionary` and`WeakValueDictionary` safe against concurrent mutations
from other threads. Patch by Kumar Aditya.
- gh-125260: The `gzip.compress()`*mtime* parameter now defaults to 0
for reproducible output. Patch by Bernhard M. Wiedemann and Adam Turner.
- gh-125243: Fix data race when creating `zoneinfo.ZoneInfo` objects
in the free threading build.
- gh-125254: Fix a bug where ArgumentError includes the incorrect ambiguous option in `argparse` .
- gh-125235: Keep `tkinter` TCL paths in venv pointing to base
installation on Windows.
- gh-61011: Fix inheritance of nested mutually exclusive groups from parent parser in `argparse.ArgumentParser` . Previously, all nested
mutually exclusive groups lost their connection to the group containing
them and were displayed as belonging directly to the parser.
- gh-125118: Donât copy arbitrary values to _Bool in the `struct` module.
- gh-125069: Fix an issue where providing a `pathlib.PurePath` object
as an initializer argument to a second`PurePath` object
with a different`parser` resulted in arguments to
the former objectâs initializer being joined by the latter objectâs
parser.
- gh-125096: If the `PYTHON_BASIC_REPL` environment variable is set,
the`site` module no longer imports the`_pyrepl` module.
Moreover, the`site` module now respects`-E` and`-I` command line options: ignore`PYTHON_BASIC_REPL` in
this case. Patch by Victor Stinner.
- gh-124969: Fix `locale.nl_langinfo(locale.ALT_DIGITS)` . Now it returns a
tuple of up to 100 strings (an empty tuple on most locales). Previously it
returned the first item of that tuple or an empty string.
- gh-124960: Fix support for the `barry_as_FLUFL` future flag in the new
REPL.
- gh-69998: Fix `locale.nl_langinfo()` in case when different categories
have different locales. The function now sets temporarily the`LC_CTYPE` locale in some cases. This temporary change affects other threads.
- gh-124958: Fix refcycles in exceptions raised from `asyncio.TaskGroup` and the python implementation of`asyncio.Future`
- gh-53203: Fix `time.strptime()` for`%c` and`%x` formats in many
locales: Arabic, Bislama, Breton, Bodo, Kashubian, Chuvash, Estonian,
French, Irish, Geâez, Gurajati, Manx Gaelic, Hebrew, Hindi, Chhattisgarhi,
Haitian Kreyol, Japanese, Kannada, Korean, Marathi, Malay, Norwegian,
Nynorsk, Punjabi, Rajasthani, Tok Pisin, Yoruba, Yue Chinese, Yau/Nungon
and Chinese.
- gh-123961: Convert `curses` to multi-phase initialization
(**PEP 489** ), thereby fixing reference leaks at interpreter shutdown.
Patch by BÃ©nÃ©dikt Tran.
- gh-124944: Add `SO_ORIGINAL_DST` to the`socket` module.
- gh-117151: The default buffer size used by `shutil.copyfileobj()` has
been increased from 64k to 256k on non-Windows platforms. It was already
larger on Windows.
- gh-90102: Skip the `isatty` system call during open() when the file is
known to not be a character device. This provides a slight performance
improvement when reading whole files.
- gh-124917: Allow calling `os.path.exists()` and`os.path.lexists()` with keyword arguments on Windows. Fixes a
regression in 3.13.0.
- gh-65865: `argparse` now raises early error for invalid`help` arguments to`add_argument()` ,`add_subparsers()` and`add_parser()` .
- gh-124653: Fix detection of the minimal Queue API needed by the `logging` module. Patch by BÃ©nÃ©dikt Tran.
- gh-91818: The CLI of many modules ( `ast` ,`ensurepip` ,`json` ,`pdb` ,`sqlite3` ,`tokenize` ,`venv` ) now
uses the actual executable name instead of simply âpythonâ to display in
the usage message.
- gh-124858: Fix reference cycles left in tracebacks in `asyncio.open_connection()` when used with`happy_eyeballs_delay`
- gh-124390: Fixed `AssertionError` when using`asyncio.staggered.staggered_race()` with`asyncio.eager_task_factory` .
- gh-85935: `argparse.ArgumentParser.add_argument()` now raises an
exception if an action that does not consume arguments (like
âstore_constâ or âstore_trueâ) or explicit`nargs=0` are specified for
positional arguments.
- gh-124835: Make `tomllib.loads()` raise`TypeError` not`AttributeError` on bad input types that do not have the`replace` attribute. Improve error message when`bytes` is received.
- gh-124693: Fix a bug where `argparse` doesnât recognize negative
complex numbers or negative numbers using scientific notation.
- gh-124787: Fix `typing.TypeAliasType` with incorrect`type_params` argument. Now it raises a`TypeError` when a type
parameter without a default follows one with a default, and when an entry
in the`type_params` tuple is not a type parameter object.
- gh-66436: Improved prog default value for `argparse.ArgumentParser` . It will now include the name of the
Python executable along with the module or package name, or the path to a
directory, ZIP file, or directory within a ZIP file if the code was run
that way.
- gh-116850: Fix `argparse` for namespaces with not directly writable
dict (e.g. classes).
- gh-101552: Add an *annoation_format* parameter to`inspect.signature()` . Add an*quote_annotation_strings* parameter to`inspect.Signature.format()` . Use the new functionality to improve the
display of annotations in signatures in`pydoc` . Patch by Jelle
Zijlstra.
- gh-58573: Fix conflicts between abbreviated long options in the parent parser and subparsers in `argparse` .
- gh-124594: All `asyncio` REPL prompts run in the same`context` . Contributed by Bartosz SÅawecki.
- gh-61181: Fix support of choices with string value in `argparse` . Substrings of the specified string no longer considered
valid values.
- gh-116750: Provide `sys.monitoring.clear_tool_id()` to unregister all
events and callbacks set by the tool.
- gh-124552: Improve the accuracy of `bdb` âs check for the possibility
of breakpoint in a frame. This makes it possible to disable unnecessary
events in functions.
- gh-124538: Fixed crash when using `gc.get_referents()` on a capsule
object.
- gh-80259: Fix `argparse` support of positional arguments with`nargs='?'` ,`default=argparse.SUPPRESS` and specified`type` .
- gh-120378: Fix a crash related to an integer overflow in `curses.resizeterm()` and`curses.resize_term()` .
- gh-124498: Fix `typing.TypeAliasType` not to be generic, when`type_params` is an empty tuple.
- gh-53834: Fix support of arguments with choices in `argparse` .
Positional arguments with nargs equal to`'?'` or`'*'` no
longer check default against`choices` . Optional arguments with`nargs` equal to`'?'` no longer check const against`choices` .
- gh-123884: Fixed bug in itertools.tee() handling of other tee inputs (a tee in a tee). The output now has the promised *n* independent new
iterators.  Formerly, the first iterator was identical (not independent)
to the input iterator. This would sometimes give surprising results.
- gh-123017: Due to unreliable results on some devices, `time.strftime()` no longer accepts negative years on Android.
- gh-123014: `os.pidfd_open()` and`signal.pidfd_send_signal()` are
now unavailable when building against Android API levels older than 31,
since the underlying system calls may cause a crash.
- gh-124176: Add support for `dataclasses.dataclass()` in`unittest.mock.create_autospec()` . Now`create_autospec` will check
for potential dataclasses and use`dataclasses.fields()` function to
retrieve the spec information.
- gh-124345: `argparse` supports abbreviated single-dash long options
separated by`=` from its value.
- gh-124400: Fixed a `pdb` bug where`until` has no effect when it
appears in a`commands` sequence. Also avoid printing the frame
information at a breakpoint that has a command list containing a command
that resumes execution.
- gh-90562: Modify dataclasses to support zero-argument super() when `slots=True` is specified.  This works by modifying all references to`__class__` to point to the newly created class.
- gh-104860: Fix disallowing abbreviation of single-dash long options in `argparse` with`allow_abbrev=False` .
- gh-63143: Fix parsing mutually exclusive arguments in `argparse` .
Arguments with the value identical to the default value (e.g. booleans,
small integers, empty or 1-character strings) are no longer considered
ânot presentâ.
- gh-72795: Positional arguments with nargs equal to `'*'` or`argparse.REMAINDER` are no longer required. This allows to use
positional argument with`nargs='*'` and without`default` in mutually
exclusive group and improves error message about required arguments.
- gh-59317: Fix parsing positional argument with nargs equal to `'?'` or`'*'` if it is preceded by an option and another positional
argument.
- gh-100980: The `_fields_` attribute of`ctypes.Structure` and`Union` is no longer set if
the setattr operation raises an error.
- gh-53780: `argparse` now ignores the first`"--"` (double dash)
between an option and command.
- gh-124217: Add RFC 9637 reserved IPv6 block `3fff::/20` in`ipaddress` module.
- gh-111513: Improve the error message that may be raised by `datetime.date.fromtimestamp()` .
- gh-124248: Fixed potential crash when using `struct` to process
zero-width âPascal stringâ fields (`0p` ).
- gh-81691: Fix handling of multiple `"--"` (double dashes) in`argparse` . Only the first one has now been removed, all subsequent
ones are now taken literally.
- gh-87041: Fix a bug in `argparse` where lengthy subparser argument
help is incorrectly indented.
- gh-84559: The default `multiprocessing` start method on Linux and
other POSIX systems has been changed away from often unsafe`"fork"` to`"forkserver"` (when the platform supports sending file handles over
pipes as most do) or`"spawn"` .  Mac and Windows are unchanged as they
already default to`"spawn"` .
- gh-124212: Fix invalid variable in `venv` handling of failed symlink
on Windows
- gh-124171: Add workaround for broken `fmod()` implementations on
Windows, that loose zero sign (e.g.`fmod(-10, 1)` returns`0.0` ).
Patch by Sergey B Kirpichev.
- gh-123978: Remove broken `time.thread_time()` and`time.thread_time_ns()` on NetBSD.
- gh-123934: Fix `unittest.mock.MagicMock` resetting magic methods
return values after`.reset_mock(return_value=True)` was called.
- gh-124016: Update `unicodedata` database to Unicode 16.0.0.
- gh-123968: Fix the command-line interface for the `random` module to
select floats between 0 and N, not 1 and N.
- gh-123945: Fix a bug where `argparse` doesnât recognize negative
numbers with underscores
- gh-123935: Fix parent slots detection for dataclasses that inherit from classes with `__dictoffset__` .
- gh-123892: Add `"_wmi"` to`sys.stdlib_module_names` . Patch by
Victor Stinner.
- gh-84808: Fix error handling in `socket` method`connect_ex()` on platforms where`errno` can be negative.
- gh-123756: Added a new argument `mode` to`pdb.Pdb` . Only allow`pdb` from command line to use`restart` command.
- gh-122765: Fix unbalanced quote errors occurring when activate.csh in `venv` was sourced with a custom prompt containing unpaired quotes or
newlines.
- gh-123657: Fix crash and memory leak in `decimal.getcontext()` . It
crashed when using a thread-local context by`--with-decimal-contextvar=no` .
- gh-123339: Fix `inspect.getsource()` for classes in`collections.abc` and`decimal` (for pure Python implementation)
modules.`inspect.getcomments()` now raises OSError instead of
IndexError if the`__firstlineno__` value for a class is out of bound.
- gh-123374: Remove check for redefined memo entry in `pickletools.dis()` .
- gh-123504: Fixed reference leak in the finalization of `tkinter` .
- gh-123430: Pages generated by the `http.server` module allow the
browser to apply its default dark mode.
- gh-123446: Fix empty function name in `TypeError` when`csv.reader()` ,`csv.writer()` , or`csv.register_dialect()` are used without the required args.
- gh-123448: Fixed memory leak of `typing.NoDefault` by moving it to
the static types array.
- gh-123409: Fix `ipaddress.IPv6Address.reverse_pointer` output
according to**RFC 3596, Â§2.5** .  Patch by BÃ©nÃ©dikt
Tran.
- gh-123089: Make `weakref.WeakSet` safe against concurrent mutations
while it is being iterated. Patch by Kumar Aditya.
- gh-123363: Show string value of `CONTAINS_OP` oparg in`dis` output. Patch by Alexandr153.
- gh-123341: Add `__class_getitem__()` to`tkinter.Event` for type subscript support at runtime. Patch by
Adonis Rakateli.
- gh-123340: Show string value of `IS_OP` oparg in`dis` output.
- gh-119518: Speed up normalization of `pathlib.PurePath` and`Path` objects by not interning string parts.
- gh-123270: Applied a more surgical fix for malformed payloads in `zipfile.Path` causing infinite loops (gh-122905) without breaking
contents using legitimate characters.
- gh-73991: Add `pathlib.Path.copy_into()` and`move_into()` , which copy and move files and directories
into*existing* directories.
- gh-123228: Fix return type for `_pyrepl.readline._ReadlineWrapper.get_line_buffer()` to be`str()` .  Patch by Sergey B Kirpichev.
- gh-123240: Raise audit events for the `input()` in the new REPL.
- gh-76960: Fix `urllib.parse.urljoin()` and`urllib.parse.urldefrag()` for URIs containing empty components. For
example,`urljoin()` with relative reference â?â now sets empty query
and removes fragment. Preserve empty components (authority, params, query,
fragment) in`urljoin()` . Preserve empty components (authority,
params, query) in`urldefrag()` .
- gh-116810: Resolve a memory leak introduced in CPython 3.10âs `ssl` when the`ssl.SSLSocket.session` property was accessed.  Speeds up
read and write access to said property by no longer unnecessarily cloning
session objects via serialization.
- gh-123243: Fix memory leak in `_decimal` .
- gh-122546: Consistently use same file name for different exceptions in the new repl. Patch by Sergey B Kirpichev.
- gh-123213: `xml.etree.ElementTree.Element.extend()` and`Element` assignment no longer hide the
internal exception if an erroneous generator is passed. Patch by Bar
Harel.
- gh-85110: Preserve relative path in URL without netloc in `urllib.parse.urlunsplit()` and`urllib.parse.urlunparse()` .
- gh-123165: Add support for rendering `Positions` in`dis` .
- gh-122909: In urllib.request when URLError is raised opening an ftp URL, the exception argument is now consistently a string. Earlier versions passed either a string or an ftplib exception instance as the argument to URLError.
- gh-123084: Deprecate `shutil.ExecError` , which hasnât been raised
by any`shutil` function since Python 3.4. Itâs now an alias for`RuntimeError` .
- gh-123085: In a bare call to `importlib.resources.files()` , ensure the
callerâs frame is properly detected when`importlib.resources` is itself
available as a compiled module only (no source).
- gh-123067: Fix quadratic complexity in parsing `"` -quoted cookie values
with backslashes by`http.cookies` .
- gh-123049: Add support for `UNNAMED_SECTION` in`configparser.ConfigParser.add_section()` .
- gh-121735: When working with zip archives, importlib.resources now properly honors module-adjacent references (e.g. `files(pkg.mod)` and
not just`files(pkg)` ).
- gh-122981: Fix `inspect.getsource()` for generated classes with Python
base classes (e.g. enums).
- gh-122903: `zipfile.Path.glob` now correctly matches directories instead
