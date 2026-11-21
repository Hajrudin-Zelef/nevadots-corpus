---
id: collect-261121-veille-ia/docs-techniques/changelog-2-14
title: "Changelog"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["pyunicode", "size", "pyobject", "longer", "type", "patch", "unicode", "python"]
source: https://docs.python.org/release/3.15.1/whatsnew/changelog.html
source_anchor: ""
source_lines: [19501, 21000]
section: "Docs_Techniques"
---
and`PyUnicode_FromStringAndSize(NULL, size)` with`size > 0` .
- bpo-36346: Mark `Py_UNICODE_COPY` ,`Py_UNICODE_FILL` ,`PyUnicode_WSTR_LENGTH` ,`PyUnicode_FromUnicode` ,`PyUnicode_AsUnicode` , and`PyUnicode_AsUnicodeAndSize` as deprecated
in C. Remove`Py_UNICODE_MATCH` which was deprecated and broken since
Python 3.3.
- bpo-40989: The `PyObject_INIT()` and`PyObject_INIT_VAR()` macros become aliases to, respectively,`PyObject_Init()` and`PyObject_InitVar()` functions.
- bpo-36020: On Windows, `#include "pyerrors.h"` no longer defines`snprintf` and`vsnprintf` macros.
- bpo-40943: The `PY_SSIZE_T_CLEAN` macro must now be defined to use`PyArg_ParseTuple()` and`Py_BuildValue()` formats which use`#` :`es#` ,`et#` ,`s#` ,`u#` ,`y#` ,`z#` ,`U#` and`Z#` . See Parsing arguments and building values and
the**PEP 353** .
- bpo-40910: Export explicitly the `Py_GetArgcArgv()` function to the
C API and document the function. Previously, it was exported implicitly
which no longer works since Python is built with`-fvisibility=hidden` .
- bpo-40724: Allow defining buffer slots in type specs.
- bpo-40679: Fix a `_PyEval_EvalCode()` crash if*qualname* argument is
NULL.
- bpo-40839: Calling `PyDict_GetItem()` without GIL held had
been allowed for historical reason. It is no longer allowed.
- bpo-40826: `PyOS_InterruptOccurred()` now fails with a fatal error
if it is called with the GIL released.
- bpo-40792: The result of `PyNumber_Index()` now always has exact
type`int` . Previously, the result could have been an instance of a
subclass of`int` .
- bpo-39573: Convert `Py_REFCNT()` and`Py_SIZE()` macros to
static inline functions. They cannot be used as l-value anymore: use`Py_SET_REFCNT()` and`Py_SET_SIZE()` to set an object
reference count and size. This change is backward incompatible on purpose,
to prepare the C API for an opaque`PyObject` structure.
- bpo-40703: The PyType_FromSpec*() functions no longer overwrite the typeâs â__module__â attribute if it is set via âPy_tp_membersâ or âPy_tp_getsetâ.
- bpo-39583: Remove superfluous âextern Câ declarations from `Include/cpython/*.h` .

## Python 3.9.0 beta 1Â¶

*Release date: 2020-05-19*

### SecurityÂ¶

### Core and BuiltinsÂ¶

- bpo-40663: Correctly generate annotations where parentheses are omitted but required (e.g: `Type[(str, int, *other))]` .
- bpo-40596: Fixed `str.isidentifier()` for non-canonicalized strings
containing non-BMP characters on Windows.
- bpo-40593: Improved syntax errors for invalid characters in source code.
- bpo-40585: Fixed a bug when using `codeop.compile_command()` that was
causing exceptions to be swallowed with the new parser. Patch by Pablo
Galindo
- bpo-40566: Apply **PEP 573** to`abc` .
- bpo-40502: Initialize `n->n_col_offset` . (Patch by Joannah Nanjekye)
- bpo-40527: Fix command line argument parsing: no longer write errors multiple times into stderr.
- bpo-1635741: Port `errno` to multiphase initialization (**PEP 489** ).
- bpo-40523: Add pass-throughs for `hash()` and`reversed()` to`weakref.proxy` objects. Patch by Pablo Galindo.
- bpo-1635741: Port `syslog` to multiphase initialization (**PEP 489** ).
- bpo-40246: Reporting a specialised error message for invalid string prefixes, which was introduced in bpo-40246, is being reverted due to backwards compatibility concerns for strings that immediately follow a reserved keyword without whitespace between them. Constructs like `bg="#d00" if clear else"#fca"` were failing to parse, which is not an
acceptable breakage on such short notice.
- bpo-40417: Fix imp module deprecation warning when PyImport_ReloadModule is called. Patch by Robert Rouhani.
- bpo-40408: Fixed support of nested type variables in GenericAlias (e.g. `list[list[T]]` ).
- bpo-1635741: Port _stat module to multiphase initialization ( **PEP 489** ).
- bpo-29587: Enable implicit exception chaining when calling `generator.throw()` .
- bpo-40328: Add tools for generating mappings headers for CJKCodecs.
- bpo-40228: Setting frame.f_lineno is now robust w.r.t. changes in the source-to-bytecode compiler
- bpo-38880: Added the ability to list interpreters associated with channel ends in the internal subinterpreters module.
- bpo-37986: Improve performance of `PyLong_FromDouble()` for values
that fit into long.

### LibraryÂ¶

- bpo-40662: Fixed `ast.get_source_segment()` for ast nodes that have
incomplete location information. Patch by Irit Katriel.
- bpo-40665: Convert `bisect` to use Argument Clinic.
- bpo-40536: Added the `available_timezones()` function to the`zoneinfo` module. Patch by Paul Ganssle.
- bpo-40645: The `hmac.HMAC` exposes internal implementation details.
The attributes`digest_cons` ,`inner` , and`outer` are deprecated
and will be removed in the future.
- bpo-40645: The internal module `_hashlib` wraps and exposes OpenSSLâs
HMAC API. The new code will be used in Python 3.10 after the internal
implementation details of the pure Python HMAC module are no longer part
of the public API.
- bpo-40637: Builtin hash modules can now be disabled or selectively enabled with `configure --with-builtin-hashlib-hashes=sha3,blake1` or`--without-builtin-hashlib-hashes` .
- bpo-37630: The `hashlib` module can now use SHA3 hashes and SHAKE XOF
from OpenSSL when available.
- bpo-40479: The `hashlib` now compiles with OpenSSL 3.0.0-alpha2.
- bpo-40257: Revert changes to `inspect.getdoc()` .
- bpo-40607: When cancelling a task due to timeout, `asyncio.wait_for()` will now propagate the exception if an error happens during cancellation.
Patch by Roman Skurikhin.
- bpo-40612: Fix edge cases in SyntaxError formatting. If the offset is <= 0, no caret is printed. If the offset is > line length, the caret is printed pointing just after the last character.
- bpo-40597: If text content lines are longer than policy.max_line_length, always use a content-encoding to make sure they are wrapped.
- bpo-40571: Added functools.cache() as a simpler, more discoverable way to access the unbounded cache variant of lru_cache(maxsize=None).
- bpo-40503: **PEP 615** , the`zoneinfo` module. Adds support for the
IANA time zone database.
- bpo-40397: Removed attributes `__args__` and`__parameters__` from
special generic aliases like`typing.List` (not subscripted).
- bpo-40549: Convert posixmodule.c (âposixâ or ântâ module) to the multiphase initialization (PEP 489).
- bpo-31033: Add a `msg` argument to`Future.cancel()` and`Task.cancel()` .
- bpo-40541: Added an optional *counts* parameter to random.sample().
- bpo-40515: The `ssl` and`hashlib` modules now actively check
that OpenSSL is build with thread support. Python 3.7.0 made thread
support mandatory and no longer works safely with a no-thread builds.
- bpo-31033: When a `asyncio.Task` is cancelled, the exception
traceback now chains all the way back to where the task was first
interrupted.
- bpo-40504: `functools.lru_cache()` objects can now be the targets of
weakrefs.
- bpo-40559: Fix possible memory leak in the C implementation of `asyncio.Task` .
- bpo-40480: `fnmatch.fnmatch()` could take exponential time in the
presence of multiple`*` pattern characters.  This was repaired by
generating more elaborate regular expressions to avoid futile
backtracking.
- bpo-40495: `compileall` is now able to use hardlinks to prevent
duplicates in a case when`.pyc` files for different optimization levels
have the same content.
- bpo-40457: The ssl module now support OpenSSL builds without TLS 1.0 and 1.1 methods.
- bpo-40355: Improve error reporting in `ast.literal_eval()` in the
presence of malformed`ast.Dict` nodes instead of silently ignoring
any non-conforming elements. Patch by Curtis Bucher.
- bpo-40465: Deprecated the optional *random* argument to*random.shuffle()* .
- bpo-40459: `platform.win32_ver()` now produces correct*ptype* strings
instead of empty strings.
- bpo-39435: The first argument of `pickle.loads()` is now
positional-only.
- bpo-39305: Update `nntplib` to merge`nntplib.NNTP` and`nntplib._NNTPBase` . Patch by Donghee Na.
- bpo-32494: Update `dbm.gnu` to use gdbm_count if possible when
calling`len()` . Patch by Donghee Na.
- bpo-40453: Add `isolated=True` keyword-only parameter to`_xxsubinterpreters.create()` . An isolated subinterpreter cannot spawn
threads, spawn a child process or call`os.fork()` .
- bpo-40286: Remove `_random.Random.randbytes()` : the C implementation of`randbytes()` . Implement the method in Python to ease subclassing:`randbytes()` now directly reuses`getrandbits()` .
- bpo-40394: Added default arguments to `difflib.SequenceMatcher.find_longest_match()` .
- bpo-39995: Fix a race condition in concurrent.futures._ThreadWakeup: access to _ThreadWakeup is now protected with the shutdown lock.
- bpo-30966: `Process.shutdown(wait=True)` of`concurrent.futures` now closes explicitly the result queue.
- bpo-30966: Add a new `close()` method to
the`SimpleQueue` class to explicitly close the
queue.
- bpo-39966: Revert bpo-25597. `unittest.mock.MagicMock` with wrapsâ
set uses default return values for magic methods.
- bpo-39791: Added `files()` function to importlib.resources with support
for subdirectories in package data, matching backport in
importlib_resources 1.5.
- bpo-40375: `imaplib.IMAP4.unselect()` is added. Patch by Donghee Na.
- bpo-40389: `repr()` now returns`typing.Optional[T]` when called for`typing.Union` of two types, one of which is`NoneType` .
- bpo-40291: Add support for CAN_J1939 sockets (available on Linux 5.4+)
- bpo-40273: `types.MappingProxyType` is now reversible.
- bpo-39075: The repr for `types.SimpleNamespace` is now insertion
ordered rather than alphabetical.
- bpo-40192: On AIX, `thread_time()` is now implemented with`thread_cputime()` which has nanosecond resolution, rather than`clock_gettime(CLOCK_THREAD_CPUTIME_ID)` which has a resolution of 10
milliseconds. Patch by Batuhan Taskaya.
- bpo-40025: Raise TypeError when _generate_next_value_ is defined after members. Patch by Ethan Onstott.
- bpo-39058: In the argparse module, the repr for Namespace() and other argument holders now displayed in the order attributes were added. Formerly, it displayed in alphabetical order even though argument order is preserved the user visible parts of the module.
- bpo-24416: The `isocalendar()` methods of`datetime.date` and`datetime.datetime` now return a named tuple instead of a`tuple` .

### DocumentationÂ¶

- bpo-34790: Add version of removal for explicit passing of coros to `asyncio.wait()` âs documentation
- bpo-40561: Provide docstrings for webbrowser open functions.
- bpo-40499: Mention that `asyncio.wait()` requires a non-empty set of
awaitables.
- bpo-39705: Tutorial example for sorted() in the Loop Techniques section is given a better explanation. Also a new example is included to explain sorted()âs basic behavior.
- bpo-39435: Fix an incorrect signature for `pickle.loads()` in the docs

### TestsÂ¶

### BuildÂ¶

### WindowsÂ¶

- bpo-40650: Include winsock2.h in pytime.c for timeval.
- bpo-40458: Increase reserved stack space to prevent overflow crash on Windows.
- bpo-39148: Add IPv6 support to `asyncio` datagram endpoints in
ProactorEventLoop. Change the raised exception for unknown address
families to ValueError as itâs not coming from Windows API.

### macOSÂ¶

- bpo-34956: When building Python on macOS from source, `_tkinter` now
links with non-system Tcl and Tk frameworks if they are installed in`/Library/Frameworks` , as had been the case on older releases of macOS.
If a macOS SDK is explicitly configured, by using`--enable-universalsdk=` or`-isysroot` , only the SDK itself is
searched. The default behavior can still be overridden with`--with-tcltk-includes` and`--with-tcltk-libs` .
- bpo-35569: Expose RFC 3542 IPv6 socket options.

### Tools/DemosÂ¶

- bpo-40479: Update multissltest helper to test with latest OpenSSL 1.0.2, 1.1.0, 1.1.1, and 3.0.0-alpha.
- bpo-40431: Fix a syntax typo in `turtledemo` that now raises a`SyntaxError` .
- bpo-40163: Fix multissltest tool. OpenSSL has changed download URL for old releases. The multissltest tool now tries to download from current and old download URLs.

### C APIÂ¶

- bpo-39465: Remove the `_PyUnicode_ClearStaticStrings()` function from
the C API.
- bpo-38787: Add PyCFunction_CheckExact() macro for exact type checks now that we allow subtypes of PyCFunction, as well as PyCMethod_CheckExact() and PyCMethod_Check() for the new PyCMethod subtype.
- bpo-40545: Declare `_PyErr_GetTopmostException()` with`PyAPI_FUNC()` to properly export the function in the C API. The function remains private
(`_Py` ) prefix.
- bpo-40412: Nullify inittab_copy during finalization, preventing future interpreter initializations in an embedded situation from crashing. Patch by Gregory Szorc.
- bpo-40429: The `PyThreadState_GetFrame()` function now returns a
strong reference to the frame.
- bpo-40428: Remove the following functions from the C API. Call `PyGC_Collect()` explicitly to free all free lists.
  - `PyAsyncGen_ClearFreeLists()`
  - `PyContext_ClearFreeList()`
  - `PyDict_ClearFreeList()`
  - `PyFloat_ClearFreeList()`
  - `PyFrame_ClearFreeList()`
  - `PyList_ClearFreeList()`
  - `PySet_ClearFreeList()`
  - `PyTuple_ClearFreeList()`
- bpo-40421: New `PyFrame_GetBack()` function: get the frame next
outer frame.
- bpo-40421: New `PyFrame_GetCode()` function: return a borrowed
reference to the frame code.
- bpo-40217: Ensure that instances of types created with `PyType_FromSpecWithBases()` will visit its class object when
traversing references in the garbage collector (implemented as an
extension of the provided`tp_traverse` ). Patch by
Pablo Galindo.
- bpo-38787: Module C state is now accessible from C-defined heap type methods ( **PEP 573** ). Patch by Marcel Plch and Petr Viktorin.

## Python 3.9.0 alpha 6Â¶

*Release date: 2020-04-27*

### SecurityÂ¶

- bpo-40121: Fixes audit events raised on creating a new socket.
- bpo-39073: Disallow CR or LF in email.headerregistry.Address arguments to guard against header injection attacks.
- bpo-39503: **CVE 2020-8492** : The`AbstractBasicAuthHandler` class of the`urllib.request` module uses an inefficient regular expression which
can be exploited by an attacker to cause a denial of service. Fix the
regex to prevent the catastrophic backtracking. Vulnerability reported by
Ben Caller and Matt Schwager.

### Core and BuiltinsÂ¶

- bpo-40313: Improve the performance of bytes.hex().
- bpo-40334: Switch to a new parser, based on PEG. For more details see PEP 617. To temporarily switch back to the old parser, use `-X oldparser` or`PYTHONOLDPARSER=1` .  In Python 3.10 we will remove the old parser
completely, including the`parser` module (already deprecated) and
anything that depends on it.
- bpo-40267: Fix the tokenizer to display the correct error message, when there is a `SyntaxError` on the last input character and no newline
follows. It used to be`unexpected EOF while parsing` , while it should
be`invalid syntax` .
- bpo-39522: Correctly unparse explicit `u` prefix for strings when
postponed evaluation for annotations activated. Patch by Batuhan Taskaya.
- bpo-40246: Report a specialized error message, `invalid string prefix` ,
when the tokenizer encounters a string with an invalid prefix.
- bpo-40082: Fix the signal handler: it now always uses the main interpreter, rather than trying to get the current Python thread state.
- bpo-37388: str.encode() and str.decode() no longer check the encoding and errors in development mode or in debug mode during Python finalization. The codecs machinery can no longer work on very late calls to str.encode() and str.decode().
- bpo-40077: Fix possible refleaks in `_json` , memo of PyScannerObject
should be traversed.
- bpo-37207: Speed up calls to `dict()` by using the**PEP 590**`vectorcall` calling convention.
- bpo-40141: Add column and line information to `ast.keyword` nodes. Patch
by Pablo Galindo.
- bpo-1635741: Port `resource` to multiphase initialization
(**PEP 489** ).
- bpo-1635741: Port `math` to multiphase initialization (**PEP 489** ).
- bpo-1635741: Port _uuid module to multiphase initialization ( **PEP 489** ).
- bpo-40077: Convert json module to use `PyType_FromSpec()` .
- bpo-40067: Improve the error message for multiple star expressions in an assignment. Patch by Furkan Onder
- bpo-1635741: Port _functools module to multiphase initialization (PEP 489). Patch by Paulo Henrique Silva.
- bpo-1635741: Port operator module to multiphase initialization (PEP 489). Patch by Paulo Henrique Silva.
- bpo-20526: Fix `PyThreadState_Clear()` .`PyThreadState.frame` is
a borrowed reference, not a strong reference:`PyThreadState_Clear()` must not call`Py_CLEAR(tstate->frame)` .
- bpo-1635741: Port time module to multiphase initialization ( **PEP 489** ).
Patch by Paulo Henrique Silva.
- bpo-1635741: Port _weakref extension module to multiphase initialization ( **PEP 489** ).
- bpo-40020: Fix a leak and subsequent crash in parsetok.c caused by realloc misuse on a rare codepath.
- bpo-39939: Added str.removeprefix and str.removesuffix methods and corresponding bytes, bytearray, and collections.UserString methods to remove affixes from a string if present. See **PEP 616** for a full
description. Patch by Dennis Sweeney.
- bpo-39481: Implement PEP 585. This supports list[int], tuple[str, â¦] etc.
- bpo-32894: Support unparsing of infinity numbers in postponed annotations. Patch by Batuhan TaÅkaya.
- bpo-37207: Speed up calls to `list()` by using the**PEP 590**`vectorcall` calling convention. Patch by Mark Shannon.

### LibraryÂ¶

- bpo-40398: `typing.get_args()` now always returns an empty tuple for
special generic aliases.
- bpo-40396: Functions `typing.get_origin()` ,`typing.get_args()` and`typing.get_type_hints()` support now generic aliases like`list[int]` .
- bpo-38061: Optimize the `subprocess` module on FreeBSD using`closefrom()` . A single`close(fd)` syscall is cheap, but when`sysconf(_SC_OPEN_MAX)` is high, the loop calling`close(fd)` on each
file descriptor can take several milliseconds.The workaround on FreeBSD to improve performance was to load and mount the fdescfs kernel module, but this is not enabled by default. Initial patch by Ed Maste (emaste), Conrad Meyer (cem), Kyle Evans (kevans) and Kubilay Kocak (koobs): https://bugs.freebsd.org/bugzilla/show_bug.cgi?id=242274
- bpo-38061: On FreeBSD, `os.closerange(fd_low, fd_high)` now calls`closefrom(fd_low)` if*fd_high* is greater than or equal to`sysconf(_SC_OPEN_MAX)` .Initial patch by Ed Maste (emaste), Conrad Meyer (cem), Kyle Evans (kevans) and Kubilay Kocak (koobs): https://bugs.freebsd.org/bugzilla/show_bug.cgi?id=242274
- bpo-40360: The `lib2to3` module is pending deprecation due to**PEP 617** .
- bpo-40138: Fix the Windows implementation of `os.waitpid()` for exit
code larger than`INT_MAX >> 8` . The exit status is now interpreted as
an unsigned number.
- bpo-39942: Set â__main__â as the default module name when â__name__â is missing in `typing.TypeVar` . Patch by Weipeng Hong.
- bpo-40275: The `logging` package is now imported lazily in`unittest` only when the`assertLogs()` assertion is used.
- bpo-40275: The `asyncio` package is now imported lazily in`unittest` only when the`IsolatedAsyncioTestCase` class is used.
- bpo-40330: In `ShareableList.__setitem__()` , check the size of a new
string item after encoding it to utf-8, not before.
- bpo-40148: Added `pathlib.Path.with_stem()` to create a new Path with
the stem replaced.
- bpo-40325: Deprecated support for set objects in random.sample().
- bpo-40257: Improved help for the `typing` module. Docstrings are now
shown for all special forms and special generic aliases (like`Union` and`List` ). Using`help()` with generic alias like`List[int]` will
show the help for the correspondent concrete type (`list` in this case).
- bpo-40257: `inspect.getdoc()` no longer returns docstring inherited
from the type of the object or from parent class if it is a class if it is
not defined in the object itself. In`pydoc` the documentation string
is now shown not only for class, function, method etc, but for any object
that has its own`__doc__` attribute.
- bpo-40287: Fixed `SpooledTemporaryFile.seek()` to return the position.
- bpo-40290: Added zscore() to statistics.NormalDist().
- bpo-40282: Allow `random.getrandbits(0)` to succeed and to return 0.
- bpo-40286: Add `random.randbytes()` function and`random.Random.randbytes()` method to generate random bytes.
- bpo-40277: `collections.namedtuple()` now provides a human-readable
repr for its field accessors.
- bpo-40270: The included copy of sqlite3 on Windows is now compiled with the json extension. This allows the use of functions such as `json_object` .
- bpo-29255: Wait in `KqueueSelector.select` when no fds are registered
- bpo-40260: Ensure `modulefinder` uses`io.open_code()` and
respects coding comments.
- bpo-40234: Allow again to spawn daemon threads in subinterpreters (revert change which denied them).
- bpo-39207: Workers in `ProcessPoolExecutor` are
now spawned on demand, only when there are no available idle workers to
reuse. This optimizes startup overhead and reduces the amount of lost CPU
time to idle workers. Patch by Kyle Stanley.
- bpo-40091: Fix a hang at fork in the logging module: the new private _at_fork_reinit() method is now used to reinitialize locks at fork in the child process.
- bpo-40149: Implement traverse and clear slots in _abc._abc_data type.
- bpo-40208: Remove deprecated `symtable.SymbolTable.has_exec()` .
- bpo-40196: Fix a bug in the `symtable` module that was causing
incorrectly report global variables as local. Patch by Pablo Galindo.
- bpo-40190: Add support for `_SC_AIX_REALMEM` to`posix.sysconf()` .
- bpo-40182: Removed the `_field_types` attribute of the`typing.NamedTuple` class.
- bpo-36517: Multiple inheritance with `typing.NamedTuple` now raises
an error instead of silently ignoring other types.
- bpo-40126: Fixed reverting multiple patches in unittest.mock. Patcherâs `__exit__()` is now never called if its`__enter__()` is failed.
Returning true from`__exit__()` silences now the exception.
- bpo-40094: CGIHTTPRequestHandler of http.server now logs the CGI script exit code, rather than the CGI script exit status of os.waitpid(). For example, if the script is killed by signal 11, it now logs: âCGI script exit code -11.â
- bpo-40108: Improve the error message when triying to import a module using `runpy` and incorrectly using the â.pyâ extension at the end of the
module name. Patch by Pablo Galindo.
- bpo-40094: Add `os.waitstatus_to_exitcode()` function: convert a wait
status to an exit code.
- bpo-40089: Fix threading._after_fork(): if fork was not called by a thread spawned by threading.Thread, threading._after_fork() now creates a _MainThread instance for _main_thread, instead of a _DummyThread instance.
- bpo-40089: Add a private `_at_fork_reinit()` method to`_thread.Lock` ,`_thread.RLock` ,`threading.RLock` and`threading.Condition` classes: reinitialize the lock at fork in
the child process, reset the lock to the unlocked state. Rename also the
private`_reset_internal_locks()` method of`threading.Event` to`_at_fork_reinit()` .
- bpo-25780: Expose `CAN_RAW_JOIN_FILTERS` in the`socket` module.
- bpo-39503: `AbstractBasicAuthHandler` of`urllib.request` now parses all WWW-Authenticate HTTP headers and
accepts multiple challenges per header: use the realm of the first Basic
challenge.
- bpo-39812: Removed daemon threads from `concurrent.futures` by adding
an internal`threading._register_atexit()` , which calls registered
functions prior to joining all non-daemon threads. This allows for
compatibility with subinterpreters, which donât support daemon threads.
- bpo-40050: Fix `importlib._bootstrap_external` : avoid creating a new`winreg` builtin module if itâs already available in`sys.modules` , and remove redundant imports.
- bpo-40014: Fix `os.getgrouplist()` : if`getgrouplist()` function fails
because the group list is too small, retry with a larger group list. On
failure, the glibc implementation of`getgrouplist()` sets`ngroups` to the total number of groups. For other implementations, double the group
list size.
- bpo-40017: Add `time.CLOCK_TAI` constant if the operating system
support it.
- bpo-40016: In re docstring, clarify the relationship between inline and argument compile flags.
- bpo-39953: Update internal table of OpenSSL error codes in the `ssl` module.
- bpo-36144: Added **PEP 584** operators to`weakref.WeakValueDictionary` .
- bpo-36144: Added **PEP 584** operators to`weakref.WeakKeyDictionary` .
- bpo-38891: Fix linear runtime behaviour of the `__getitem__` and`__setitem__` methods in`multiprocessing.shared_memory.ShareableList` . This avoids
quadratic performance when iterating a`ShareableList` . Patch by Thomas
Krennwallner.
- bpo-39682: Remove undocumented support for *closing* a`pathlib.Path` object via its context manager. The context manager magic methods remain,
but they are now a no-op, making`Path` objects immutable.
- bpo-36144: Added **PEP 584** operators (`|` and`|=` ) to`collections.ChainMap` .
- bpo-39011: Normalization of line endings in ElementTree attributes was removed, as line endings which were replaced by entity numbers should be preserved in original form.
- bpo-38410: Properly handle `sys.audit()` failures in`sys.set_asyncgen_hooks()` .
- bpo-36541: lib2to3 now recognizes named assignment expressions (the walrus operator, `:=` )
- bpo-35967: In platform, delay the invocation of âuname -pâ until the processor attribute is requested.
- bpo-35113: `inspect.getsource()` now returns correct source code for
inner class with same name as module level class. Decorators are also
returned as part of source of the class. Patch by Karthikeyan
Singaravelan.
- bpo-33262: Deprecate passing None as an argument for `shlex.split()` âs`s` parameter.  Patch by Zackery Spytz.
- bpo-31758: Prevent crashes when using an uninitialized `_elementtree.XMLParser` object. Patch by Oren Milman.

### DocumentationÂ¶

- bpo-27635: The pickle documentation incorrectly claimed that `__new__` isnât called by default when unpickling.
- bpo-39879: Updated Data model docs to include `dict()` insertion
order preservation. Patch by Furkan Onder and Samy Lahfa.
- bpo-38387: Document `PyDoc_STRVAR` macro in the C-API reference.
- bpo-13743: Some methods within xml.dom.minidom.Element class are now better documented.

### TestsÂ¶

- bpo-31904: Set expected default encoding in test_c_locale_coercion.py for VxWorks RTOS.
- bpo-40162: Update Travis CI configuration to OpenSSL 1.1.1f.
- bpo-40146: Update OpenSSL to 1.1.1f in Azure Pipelines.
- bpo-40094: Add `test.support.wait_process()` function.
- bpo-40003: `test.bisect_cmd` now copies Python command line options like`-O` or`-W` . Moreover, emit a warning if`test.bisect_cmd` is used
with`-w` /`--verbose2` option.
- bpo-39380: Add the encoding in `ftplib.FTP` and`ftplib.FTP_TLS` to the constructor as keyword-only and change the
default from`latin-1` to`utf-8` to follow**RFC 2640** .
- bpo-39793: Use the same domain when testing `make_msgid` . Patch by
Batuhan Taskaya.
- bpo-1812: Fix newline handling in doctest.testfile when loading from a package whose loader has a get_data method. Patch by Peter Donis.

### BuildÂ¶

- bpo-38360: Support single-argument form of macOS -isysroot flag.
- bpo-40158: Fix CPython MSBuild Properties in NuGet Package (build/native/python.props)
- bpo-38527: Fix configure check on Solaris for âfloat word orderingâ: sometimes, the correct âgrepâ command was not being used. Patch by Arnon Yaari.

### WindowsÂ¶

### macOSÂ¶

- bpo-38329: python.org macOS installers now update the Current version symlink of /Library/Frameworks/Python.framework/Versions for 3.9 installs. Previously, Current was only updated for Python 2.x installs. This should make it easier to embed Python 3 into other macOS applications.
- bpo-40164: Update macOS installer builds to use OpenSSL 1.1.1g.

### IDLEÂ¶

### Tools/DemosÂ¶

- bpo-40385: Removed the checkpyc.py tool. Please see compileall without force mode as a potential alternative.
- bpo-40179: Fixed translation of `#elif` in Argument Clinic.
- bpo-40094: Fix `which.py` script exit code: it now uses`os.waitstatus_to_exitcode()` to convert`os.system()` exit status
into an exit code.

### C APIÂ¶

- bpo-40241: Move the `PyGC_Head` structure to the internal C API.
- bpo-40170: Convert `PyObject_IS_GC()` macro to a function to hide
implementation details.
- bpo-40241: Add the functions `PyObject_GC_IsTracked()` and`PyObject_GC_IsFinalized()` to the public API to allow to query if
Python objects are being currently tracked or have been already finalized
by the garbage collector respectively. Patch by Pablo Galindo.
- bpo-40170: The `PyObject_NEW()` macro becomes an alias to the`PyObject_New()` macro, and the`PyObject_NEW_VAR()` macro
becomes an alias to the`PyObject_NewVar()` macro, to hide
implementation details. They no longer access directly the`PyTypeObject.tp_basicsize` member.
- bpo-40170: `PyType_HasFeature()` now always calls`PyType_GetFlags()` to hide implementation details. Previously, it
accessed directly the`PyTypeObject.tp_flags` member when the
limited C API was not used.
- bpo-40170: Convert the `PyObject_GET_WEAKREFS_LISTPTR()` macro to a
function to hide implementation details: the macro accessed directly to
the`PyTypeObject.tp_weaklistoffset` member.
- bpo-40170: Convert `PyObject_CheckBuffer()` macro to a function to
hide implementation details: the macro accessed directly the`PyTypeObject.tp_as_buffer` member.
- bpo-40170: Always declare `PyIndex_Check()` as an opaque function to
hide implementation details: remove`PyIndex_Check()` macro. The macro
accessed directly the`PyTypeObject.tp_as_number` member.
- bpo-39947: Add `PyThreadState_GetID()` function: get the unique
identifier of a Python thread state.

## Python 3.9.0 alpha 5Â¶

*Release date: 2020-03-23*

### SecurityÂ¶

- bpo-38576: Disallow control characters in hostnames in http.client, addressing **CVE 2019-18348** . Such potentially malicious header injection
URLs now cause a InvalidURL to be raised.

### Core and BuiltinsÂ¶

- bpo-40010: Optimize pending calls in multithreaded applications. If a thread different than the main thread schedules a pending call ( `Py_AddPendingCall()` ), the bytecode evaluation loop is no longer
interrupted at each bytecode instruction to check for pending calls which
cannot be executed. Only the main thread can execute pending calls.Previously, the bytecode evaluation loop was interrupted at each instruction until the main thread executes pending calls.
- bpo-1635741: Port _weakref extension module to multiphase initialization ( **PEP 489** ).
- bpo-1635741: Port _collections module to multiphase initialization ( **PEP 489** ).
- bpo-40010: Optimize signal handling in multithreaded applications. If a thread different than the main thread gets a signal, the bytecode evaluation loop is no longer interrupted at each bytecode instruction to check for pending signals which cannot be handled. Only the main thread of the main interpreter can handle signals. Previously, the bytecode evaluation loop was interrupted at each instruction until the main thread handles signals.
- bpo-39984: If `Py_AddPendingCall()` is called in a subinterpreter,
the function is now scheduled to be called from the subinterpreter, rather
than being called from the main interpreter. Each subinterpreter now has
its own list of scheduled calls.
- bpo-1635741: Port _heapq module to multiphase initialization.
- bpo-1635741: Port itertools module to multiphase initialization ( **PEP 489** ).
- bpo-37207: Speed up calls to `frozenset()` by using the**PEP 590**`vectorcall` calling convention. Patch by Donghee Na.
- bpo-39984: subinterpreters: Move `_PyRuntimeState.ceval.tracing_possible` to`PyInterpreterState.ceval.tracing_possible` : each interpreter now has
its own variable.
- bpo-37207: Speed up calls to `set()` by using the**PEP 590**`vectorcall` calling convention. Patch by Donghee Na.
- bpo-1635741: Port _statistics module to multiphase initialization ( **PEP 489** ).
- bpo-39968: Use inline function to replace extension modulesâ get_module_state macros.
- bpo-39965: Correctly raise `SyntaxError` if*await* is used inside
non-async functions and`PyCF_ALLOW_TOP_LEVEL_AWAIT` is set (like in the
asyncio REPL). Patch by Pablo Galindo.
- bpo-39562: Allow executing asynchronous comprehensions on the top level when the `PyCF_ALLOW_TOP_LEVEL_AWAIT` flag is given. Patch by Batuhan
Taskaya.
- bpo-37207: Speed up calls to `tuple()` by using the**PEP 590**`vectorcall` calling convention. Patch by Donghee Na.
- bpo-38373: Changed list overallocation strategy. It no longer overallocates if the new size is closer to overallocated size than to the old size and adds padding.
- bpo-39926: Update Unicode database to Unicode version 13.0.0.
- bpo-19466: Clear the frames of daemon threads earlier during the Python shutdown to call objects destructors. So âunclosed fileâ resource warnings are now emitted for daemon threads in a more reliable way.
- bpo-38894: Fix a bug that was causing incomplete results when calling `pathlib.Path.glob` in the presence of symlinks that point to files
where the user does not have read access. Patch by Pablo Galindo and Matt
Wozniski.
- bpo-39877: Fix `PyEval_RestoreThread()` random crash at exit with
daemon threads. It now accesses the`_PyRuntime` variable directly
instead of using`tstate->interp->runtime` , since`tstate` can be a
dangling pointer after`Py_Finalize()` has been called. Moreover,
the daemon thread now exits before trying to take the GIL.
- bpo-39871: Fix a possible `SystemError` in`math.{atan2,copysign,remainder}()` when the first argument cannot be
converted to a`float` . Patch by Zackery Spytz.
- bpo-39776: Fix race condition where threads created by PyGILState_Ensure() could get a duplicate id. This affects consumers of tstate->id like the contextvar caching machinery, which could return invalid cached objects under heavy thread load (observed in embedded scenarios).
- bpo-39778: Fixed a crash due to incorrect handling of weak references in `collections.OrderedDict` classes. Patch by Pablo Galindo.
- bpo-1635741: Port audioop extension module to multiphase initialization ( **PEP 489** ).
- bpo-39702: Relax decorator grammar restrictions to allow any valid expression ( **PEP 614** ).
- bpo-38091: Tweak import deadlock detection code to not deadlock itself.
- bpo-1635741: Port _locale extension module to multiphase initialization ( **PEP 489** ).
- bpo-39087: Optimize `PyUnicode_AsUTF8()` and`PyUnicode_AsUTF8AndSize()` slightly when they need to create
internal UTF-8 cache.
- bpo-39520: Fix unparsing of ext slices with no items ( `foo[:,]` ). Patch
by Batuhan Taskaya.
- bpo-39220: Do not optimize annotations if âfrom __future__ import annotationsâ is used. Patch by Pablo Galindo.
- bpo-35712: Using `NotImplemented` in a boolean context has been
deprecated. Patch contributed by Josh Rosenberg.
- bpo-22490: Donât leak environment variable `__PYVENV_LAUNCHER__` into
the interpreter session on macOS.

### LibraryÂ¶

- bpo-39830: Add `zipfile.Path` to`__all__` in the`zipfile` module.
- bpo-40000: Improved error messages for validation of `ast.Constant` nodes. Patch by Batuhan Taskaya.
- bpo-39999: `__module__` of the AST node classes is now set to âastâ
instead of â_astâ. Added docstrings for dummy AST node classes and
deprecated attributes.
- bpo-39991: `uuid.getnode()` now skips IPv6 addresses with the same
string length than a MAC address (17 characters): only use MAC addresses.
- bpo-39988: Deprecated `ast.AugLoad` and`ast.AugStore` node classes
because they are no longer used.
- bpo-39656: Ensure `bin/python3.#` is always present in virtual
environments on POSIX platforms - by Anthony Sottile.
- bpo-39969: Deprecated `ast.Param` node class because itâs no longer
used. Patch by Batuhan Taskaya.
- bpo-39360: Ensure all workers exit when finalizing a `multiprocessing.Pool` implicitly via the module finalization
handlers of multiprocessing. This fixes a deadlock situation that can be
experienced when the Pool is not properly finalized via the context
manager or a call to`multiprocessing.Pool.terminate` . Patch by Batuhan
Taskaya and Pablo Galindo.
- bpo-35370: sys.settrace(), sys.setprofile() and _lsprof.Profiler.enable() now properly report `PySys_Audit()` error if âsys.setprofileâ or
âsys.settraceâ audit event is denied.
- bpo-39936: AIX: Fix _aix_support module when the subprocess is not available, when building Python from scratch. It now uses new private _bootsubprocess module, rather than having two implementations depending if subprocess is available or not. So _aix_support.aix_platform() result is now the same if subprocess is available or not.
- bpo-36144: `collections.OrderedDict` now implements`|` and`|=` (**PEP 584** ).
- bpo-39652: The column name found in `sqlite3.Cursor.description` is now
truncated on the first â[â only if the PARSE_COLNAMES option is set.
- bpo-39915: Ensure `unittest.mock.AsyncMock.await_args_list` has call
objects in the order of awaited arguments instead of using`unittest.mock.Mock.call_args` which has the last value of the call.
Patch by Karthikeyan Singaravelan.
- bpo-36144: Updated `os.environ` and`os.environb` to support**PEP 584** âs merge (`|` ) and update (`|=` ) operators.
- bpo-38662: The `ensurepip` module now invokes`pip` via the`runpy` module. Hence it is no longer tightly coupled with the internal API of the
bundled`pip` version, allowing easier updates to a newer`pip` version both internally and for distributors.
- bpo-38075: Fix the `random.Random.seed()` method when a`bool` is passed as the seed.
- bpo-39916: More reliable use of `os.scandir()` in`Path.glob()` . It no
longer emits a ResourceWarning when interrupted.
- bpo-39850: `multiprocessing` now supports abstract socket addresses
(if abstract sockets are supported in the running platform). When creating
arbitrary addresses (like when default-constructing`multiprocessing.connection.Listener` objects) abstract sockets are
preferred to avoid the case when the temporary-file-generated address is
too large for an AF_UNIX socket address. Patch by Pablo Galindo.
- bpo-36287: `ast.dump()` no longer outputs optional fields and
attributes with default values. The default values for optional fields and
attributes of AST nodes are now set as class attributes (e.g.`Constant.kind` is set to`None` ).
- bpo-39889: Fixed `ast.unparse()` for extended slices containing a
single element (e.g.`a[i:j,]` ). Remove redundant tuples when index with
a tuple (e.g.`a[i, j]` ).
- bpo-39828: Fix `json.tool` to catch`BrokenPipeError` . Patch by
Donghee Na.
- bpo-13487: Avoid a possible *âRuntimeError: dictionary changed size during
iterationâ* from`inspect.getmodule()` when it tried to loop through`sys.modules` .
- bpo-39674: Revert âbpo-37330: open() no longer accept âUâ in file modeâ. The âUâ mode of open() is kept in Python 3.9 to ease transition from Python 2.7, but will be removed in Python 3.10.
- bpo-28577: The hosts method on 32-bit prefix length IPv4Networks and 128-bit prefix IPv6Networks now returns a list containing the single Address instead of an empty list.
- bpo-39826: Add getConnection method to logging HTTPHandler to enable custom connections.
- bpo-39763: Reimplement `distutils.spawn.spawn` function with the`subprocess` module.
- bpo-39794: Add âwithout-decimal-contextvar build option. This enables a thread-local rather than a coroutine local context.
- bpo-36144: `collections.defaultdict` now implements`|` (**PEP 584** ).
- bpo-39517: Fix runpy.run_path() when using pathlike objects
- bpo-39775: Change `inspect.Signature.parameters` back to`collections.OrderedDict` . This was changed to`dict` in Python
3.9.0a4.
- bpo-39678: Refactor queue_manager in `concurrent.futures.ProcessPoolExecutor` to make it easier to
maintain.
- bpo-39764: Fix AttributeError when calling get_stack on a PyAsyncGenObject Task
- bpo-39769: The `compileall.compile_dir()` functionâs*ddir* parameter
and the compileall command line flag`-d` no longer write the wrong
pathname to the generated pyc file for submodules beneath the root of the
directory tree being compiled.  This fixes a regression introduced with
Python 3.5.
- bpo-36144: `types.MappingProxyType` objects now support the merge
(`|` ) operator from**PEP 584** .
- bpo-38691: The `importlib` module now ignores the`PYTHONCASEOK` environment variable when the`-E` or`-I` command line options are being used.
- bpo-39719: Remove `tempfile.SpooledTemporaryFile.softspace()` as files
no longer have the`softspace` attribute in Python 3. Patch by Shantanu.
- bpo-39667: Improve pathlib.Path compatibility on zipfile.Path and correct performance degradation as found in zipp 3.0.
- bpo-39638: Keep ASDL signatures in the docstrings for `AST` nodes. Patch
by Batuhan Taskaya
- bpo-39639: Deprecated `ast.Suite` node class because itâs no longer
used. Patch by Batuhan Taskaya.
- bpo-39609: Add thread_name_prefix to default asyncio executor
- bpo-39548: Fix handling of header in `urllib.request.AbstractDigestAuthHandler` when the optional`qop` parameter is not present.
- bpo-39509: HTTP status codes `103 EARLY_HINTS` and`425 TOO_EARLY` are
added to`http.HTTPStatus` . Patch by Donghee Na.
- bpo-39507: Adding HTTP status 418 âIâm a Teapotâ to HTTPStatus in http library. Patch by Ross Rhodes.
- bpo-39495: Remove default value from *attrs* parameter of`xml.etree.ElementTree.TreeBuilder.start()` for consistency between
Python and C implementations.
- bpo-38971: Open issue in the BPO indicated a desire to make the implementation of codecs.open() at parity with io.open(), which implements a try/except to assure file stream gets closed before an exception is raised.
- bpo-38641: Added starred expressions support to `return` and`yield` statements for`lib2to3` . Patch by Vlad Emelianov.
- bpo-37534: When using minidom module to generate XML documents the ability to add Standalone Document Declaration is added. All the changes are made to generate a document in compliance with Extensible Markup Language (XML) 1.0 (Fifth Edition) W3C Recommendation (available here: https://www.w3.org/TR/xml/#sec-prolog-dtd).
- bpo-34788: Add support for scoped IPv6 addresses to `ipaddress` .
Patch by Oleksandr Pavliuk.
- bpo-34822: Simplified AST for subscription. Simple indices are now represented by their value, extended slices are represented as tuples. `ast` classes`Index` and`ExtSlice` are considered deprecated
and will be removed in future Python versions. In the meantime,`Index(value)` now returns a`value` itself,`ExtSlice(slices)` returns`Tuple(slices, Load())` .

### DocumentationÂ¶

- bpo-39868: Updated the Language Reference for **PEP 572** .
- bpo-13790: Change âstringâ to âspecificationâ in format doc.
- bpo-17422: The language reference no longer restricts default class namespaces to dicts only.
- bpo-39530: Fix misleading documentation about mixed-type numeric comparisons.
- bpo-39718: Update `token` documentation to reflect additions in
Python 3.8
- bpo-39677: Changed operand name of **MAKE_FUNCTION** from*argc* to*flags* for module`dis`

### TestsÂ¶

- bpo-40019: test_gdb now skips tests if it detects that gdb failed to read debug information because the Python binary is optimized.
- bpo-27807: `test_site.test_startup_imports()` is now skipped if a path
of`sys.path` contains a`.pth` file.
- bpo-26067: Do not fail test_shutil test_chown test when uid or gid of user cannot be resolved to a name.
- bpo-39855: test_subprocess.test_user() now skips the test on an user name if the user name doesnât exist. For example, skip the test if the user ânobodyâ doesnât exist on Linux.

### BuildÂ¶

- bpo-39761: Fix build with DTrace but without additional DFLAGS.
- bpo-39763: setup.py now uses a basic implementation of the `subprocess` module if the`subprocess` module is not available:
before required C extension modules are built.
- bpo-1294959: Add `--with-platlibdir` option to the configure script:
name of the platform-specific library directory, stored in the new`sys.platlibdir` attribute. It is used to build the path of
platform-specific extension modules and the path of the standard library.
It is equal to`"lib"` on most platforms. On Fedora and SuSE, it is
equal to`"lib64"` on 64-bit platforms. Patch by Jan MatÄjek, MatÄj
Cepl, Charalampos Stratakis and Victor Stinner.

### WindowsÂ¶

- bpo-39930: Ensures the required `vcruntime140.dll` is included in
install packages.
- bpo-39847: Avoid hang when computer is hibernated whilst waiting for a mutex (for lock-related objects from `threading` ) around 49-day
uptime.
- bpo-38597: `distutils` will no longer statically link`vcruntime140.dll` when a redistributable version is unavailable.
All future releases of CPython will include a copy of this DLL to ensure
distributed extensions can continue to load.
- bpo-38380: Update Windows builds to use SQLite 3.31.1
- bpo-39789: Update Windows release build machines to Visual Studio 2019 (MSVC 14.2).
- bpo-34803: Package for nuget.org now includes repository reference and bundled icon image.

### macOSÂ¶

- bpo-38380: Update macOS builds to use SQLite 3.31.1

### IDLEÂ¶

- bpo-27115: For âGo to Lineâ, use a Query box subclass with IDLE standard behavior and improved error checking.
- bpo-39885: Since clicking to get an IDLE context menu moves the cursor, any text selection should be and now is cleared.
- bpo-39852: Edit âGo to lineâ now clears any selection, preventing accidental deletion. It also updates Ln and Col on the status bar.
- bpo-39781: Selecting code context lines no longer causes a jump.

### Tools/DemosÂ¶

- bpo-36184: Port python-gdb.py to FreeBSD. python-gdb.py now checks for âtake_gilâ function name to check if a frame tries to acquire the GIL, instead of checking for âpthread_cond_timedwaitâ which is specific to Linux and can be a different condition than the GIL.
- bpo-38080: Added support to fix `getproxies` in the`lib2to3.fixes.fix_urllib` module. Patch by JosÃ© Roberto Meza
Cabrera.

### C APIÂ¶

- bpo-40024: Add `PyModule_AddType()` helper function: add a type to a
module. Patch by Donghee Na.
- bpo-39946: Remove `_PyRuntime.getframe` hook and remove`_PyThreadState_GetFrame` macro which was an alias to`_PyRuntime.getframe` . They were only exposed by the internal C API.
Remove also`PyThreadFrameGetter` type.
- bpo-39947: Add `PyThreadState_GetFrame()` function: get the current
frame of a Python thread state.
- bpo-37207: Add _PyArg_NoKwnames helper function. Patch by Donghee Na.
- bpo-39947: Add `PyThreadState_GetInterpreter()` : get the interpreter
of a Python thread state.
- bpo-39947: Add `PyInterpreterState_Get()` function to the limited C
API.
- bpo-35370: If `PySys_Audit()` fails in`PyEval_SetProfile()` or`PyEval_SetTrace()` , log the error as an unraisable exception.
- bpo-39947: Move the static inline function flavor of Py_EnterRecursiveCall() and Py_LeaveRecursiveCall() to the internal C API: they access PyThreadState attributes. The limited C API provides regular functions which hide implementation details.
- bpo-39947: Py_TRASHCAN_BEGIN_CONDITION and Py_TRASHCAN_END macro no longer access PyThreadState attributes, but call new private _PyTrash_begin() and _PyTrash_end() functions which hide implementation details.
- bpo-39884: `PyDescr_NewMethod()` and`PyCFunction_NewEx()` now
include the method name in the SystemError âbad call flagsâ error message
to ease debug.
- bpo-39877: Deprecated `PyEval_InitThreads()` and`PyEval_ThreadsInitialized()` . Calling`PyEval_InitThreads()` now does nothing.
- bpo-38249: `Py_UNREACHABLE` is now implemented with`__builtin_unreachable()` and analogs in release mode.
- bpo-38643: `PyNumber_ToBase()` now raises a`SystemError` instead of crashing when called with invalid base.
- bpo-39882: The `Py_FatalError()` function is replaced with a macro
which logs automatically the name of the current function, unless the`Py_LIMITED_API` macro is defined.
- bpo-39824: Extension modules: `m_traverse` ,`m_clear` and`m_free` functions of`PyModuleDef` are no longer called if the module
state was requested but is not allocated yet. This is the case immediately
after the module is created and before the module is executed
(`Py_mod_exec` function). More precisely, these functions are not
called if`m_size` is greater than 0 and the module
state (as returned by`PyModule_GetState()` ) is`NULL` .Extension modules without module state ( `m_size <= 0` ) are not affected.
- bpo-38913: Fixed segfault in `Py_BuildValue()` called with a format
containing â#â and undefined PY_SSIZE_T_CLEAN whwn an exception is set.
- bpo-38500: Add a private API to get and set the frame evaluation function: add `_PyInterpreterState_GetEvalFrameFunc()` and`_PyInterpreterState_SetEvalFrameFunc()` C functions. The`_PyFrameEvalFunction` function type now takes a*tstate* parameter.

## Python 3.9.0 alpha 4Â¶

*Release date: 2020-02-25*

### SecurityÂ¶

### Core and BuiltinsÂ¶

- bpo-39382: Fix a use-after-free in the single inheritance path of `issubclass()` , when the`__bases__` of an object has a single
reference, and so does its first item. Patch by Yonatan Goldschmidt.
- bpo-39573: Update clinic tool to use `Py_IS_TYPE()` . Patch by
Donghee Na.
- bpo-39619: Enable use of `os.chroot()` on HP-UX systems.
- bpo-39573: Add `Py_IS_TYPE()` static inline function to check
whether the object*o* type is*type* .
- bpo-39606: Fix regression caused by fix for bpo-39386, that prevented calling `aclose` on an async generator that had already been closed or
exhausted.
- bpo-39579: Change the ending column offset of `Attribute` nodes
constructed in`ast_for_dotted_name` to point at the end of the current
node and not at the end of the last`NAME` node.
- bpo-1635741: Port _crypt extension module to multiphase initialization ( **PEP 489** ).
- bpo-1635741: Port _contextvars extension module to multiphase initialization ( **PEP 489** ).
- bpo-39510: Fix segfault in `readinto()` method on closed BufferedReader.
- bpo-39502: Fix `time.localtime()` on 64-bit AIX  to support years
before 1902 and after 2038. Patch by M Felt.
- bpo-39492: Fix a reference cycle in the C Pickler that was preventing the garbage collection of deleted, pickled objects.
- bpo-39453: Fixed a possible crash in `list.__contains__()` when a list
is changed during comparing items. Patch by Donghee Na.
- bpo-39434: floor division of float operation now has a better performance. Also the message of `ZeroDivisionError` for this
operation is updated. Patch by Donghee Na.
- bpo-1635741: Port _codecs extension module to multiphase initialization ( **PEP 489** ).
- bpo-1635741: Port _bz2 extension module to multiphase initialization ( **PEP 489** ).
- bpo-1635741: Port _abc extension module to multiphase initialization ( **PEP 489** ).
- bpo-39320: Replace two complex bytecodes for building dicts with two simpler ones. The new bytecodes `DICT_MERGE` and`DICT_UPDATE` have
been added The old bytecodes`BUILD_MAP_UNPACK` and`BUILD_MAP_UNPACK_WITH_CALL` have been removed.
- bpo-39219: Syntax errors raised in the tokenizer now always set correct âtextâ and âoffsetâ attributes.
- bpo-36051: Drop the GIL during large `bytes.join` operations. Patch by
Bruce Merry.
- bpo-38960: Fix DTrace build issues on FreeBSD. Patch by David Carlier.
- bpo-37207: Speed up calls to `range()` by about 30%, by using the PEP
590`vectorcall` calling convention. Patch by Mark Shannon.
- bpo-36144: `dict` (and`collections.UserDict` ) objects now
support PEP 584âs merge (`|` ) and update (`|=` ) operators. Patch by
Brandt Bucher.
- bpo-32856: Optimized the idiom for assignment a temporary variable in comprehensions. Now `for y in [expr]` in comprehensions is as fast as a
simple assignment`y = expr` .

### LibraryÂ¶

- bpo-30566: Fix `IndexError` when trying to decode an invalid string
with punycode codec.
- bpo-39649: Remove obsolete check for `__args__` in`bdb.Bdb.format_stack_entry` .
- bpo-39648: Expanded `math.gcd()` and`math.lcm()` to handle
multiple arguments.
- bpo-39681: Fix a regression where the C pickle module wouldnât allow unpickling from a file-like object that doesnât expose a readinto() method.
- bpo-35950: Raise `io.UnsupportedOperation` in`io.BufferedReader.truncate()` when it is called on a read-only`io.BufferedReader` instance.
- bpo-39479: Add `math.lcm()` function: least common multiple.
- bpo-39674: Revert âDo not expose abstract collection classes in the collections moduleâ change (bpo-25988). Aliases to ABC like collections.Mapping are kept in Python 3.9 to ease transition from Python 2.7, but will be removed in Python 3.10.
- bpo-39104: Fix hanging ProcessPoolExcutor on `shutdown(wait=False)` when
a task has failed pickling.
- bpo-39627: Fixed TypedDict totality check for inherited keys.
- bpo-39474: Fixed starting position of AST for expressions like `(a)(b)` ,`(a)[b]` and`(a).b` .
- bpo-21016: The `pydoc` and`trace` modules now use the`sysconfig` module to get the path to the Python standard library, to
support uncommon installation path like`/usr/lib64/python3.9/` on
Fedora. Patch by Jan MatÄjek.
- bpo-39590: Collections.deque now holds strong references during deque.__contains__ and deque.count, fixing crashes.
- bpo-39586: The distutils `bdist_msi` command is deprecated in Python
3.9, use`bdist_wheel` (wheel packages) instead.
- bpo-39595: Improved performance of zipfile.Path for files with a large number of entries. Also improved performance and fixed minor issue as published with importlib_metadata 1.5.
- bpo-39350: Fix regression in `fractions.Fraction` if the numerator
and/or the denominator is an`int` subclass. The`math.gcd()` function is now used to normalize the*numerator* and*denominator* .`math.gcd()` always return a`int` type. Previously, the GCD
type depended on*numerator* and*denominator* .
- bpo-39567: Added audit for `os.walk()` ,`os.fwalk()` ,`pathlib.Path.glob()` and`pathlib.Path.rglob()` .
- bpo-39559: Remove unused, undocumented argument `getters` from`uuid.getnode()`
- bpo-38149: `sys.audit()` is now called only once per call of`glob.glob()` and`glob.iglob()` .
- bpo-39546: Fix a regression in `ArgumentParser` where`allow_abbrev=False` was ignored for long options that used a prefix
character other than â-â.
- bpo-39450: Striped whitespace from docstring before returning it from `unittest.case.shortDescription()` .
- bpo-12915: A new function `resolve_name` has been added to the`pkgutil` module. This resolves a string of the form`'a.b.c.d'` or`'a.b:c.d'` to an object. In the example,`a.b` is a package/module
and`c.d` is an object within that package/module reached via recursive
attribute access.
- bpo-39353: The `binascii.crc_hqx()` function is no longer deprecated.
- bpo-39493: Mark `typing.IO.closed` as a property
- bpo-39491: Add `typing.Annotated` and`include_extras` parameter
to`typing.get_type_hints()` as part of**PEP 593** . Patch by Till
Varoquaux, documentation by Till Varoquaux and Konstantin Kashin.
- bpo-39485: Fix a bug in `unittest.mock.create_autospec()` that would
complain about the wrong number of arguments for custom descriptors
defined in an extension module returning functions.
- bpo-38932: Mock fully resets child objects on reset_mock(). Patch by Vegard Stikbakke
- bpo-39082: Allow AsyncMock to correctly patch static/class methods
- bpo-39432: Implement PEP-489 algorithm for non-ascii âPyInit_â¦â symbol names in distutils to make it export the correct init symbol also on Windows.
- bpo-18819: Omit `devmajor` and`devminor` fields for non-device files
in`tarfile` archives, enabling bit-for-bit compatibility with GNU`tar(1)` .
- bpo-39349: Added a new *cancel_futures* parameter to`concurrent.futures.Executor.shutdown()` that cancels all pending
futures which have not started running, instead of waiting for them to
complete before shutting down the executor.
- bpo-39274: `bool(fraction.Fraction)` now returns a boolean even if
(numerator != 0) does not return a boolean (ex: numpy number).
- bpo-34793: Remove support for `with (await asyncio.lock):` and```
with
(yield from asyncio.lock):
```
.  The same is correct for`asyncio.Condition` and`asyncio.Semaphore` .
- bpo-25597: Ensure, if `wraps` is supplied to`unittest.mock.MagicMock` , it is used to calculate return values
for the magic methods instead of using the default return values. Patch by
Karthikeyan Singaravelan.
- bpo-36350: `inspect.Signature.parameters` and`inspect.BoundArguments.arguments` are now dicts instead of
OrderedDicts. Patch contributed by RÃ©mi Lapeyre.
- bpo-35727: Fix sys.exit() and sys.exit(None) exit code propagation when used in multiprocessing.Process.
- bpo-32173: * Add `lazycache` function to`__all__` . * Use`dict.clear` to clear the cache. * Refactoring`getline` function and`checkcache` function.

### DocumentationÂ¶

- bpo-17422: The language reference now specifies restrictions on class namespaces. Adapted from a patch by Ethan Furman.
- bpo-39572: Updated documentation of `total` flag of`TypedDict` .
- bpo-39654: In pyclbr doc, update âclassâ to âmoduleâ where appropriate and add readmodule comment. Patch by Hakan Ãelik.
- bpo-39153: Clarify refcounting semantics for the following functions: - PyObject_SetItem - PyMapping_SetItemString - PyDict_SetItem - PyDict_SetItemString
- bpo-39392: Explain that when filling with turtle, overlap regions may be left unfilled.
- bpo-39369: Update mmap readline method description. The fact that the readline method does update the file position should not be ignored since this might give the impression for the programmer that it doesnât update it.
- bpo-9056: Include subsection in TOC for PDF version of docs.

### TestsÂ¶

- bpo-38325: Skip tests on non-BMP characters of test_winconsoleio.
- bpo-39502: Skip test_zipfile.test_add_file_after_2107() if `time.localtime()` fails with`OverflowError` . It is the case on
AIX 6.1 for example.

### BuildÂ¶

- bpo-39489: Remove `COUNT_ALLOCS` special build.

### WindowsÂ¶

- bpo-39553: Delete unused code related to SxS manifests.
- bpo-39439: Honor the Python path when a virtualenv is active on Windows.
- bpo-39393: Improve the error message when attempting to load a DLL with unresolved dependencies.
- bpo-38883: `home()` and`expanduser()` on Windows now prefer`USERPROFILE` and no longer use`HOME` , which is not normally set for regular user accounts. This
makes them again behave like`os.path.expanduser()` , which was changed
to ignore`HOME` in 3.8, see bpo-36264.
- bpo-39185: The build.bat script has additional options for very-quiet output (-q) and very-verbose output (-vv)

### IDLEÂ¶

- bpo-39663: Add tests for pyparse find_good_parse_start().
- bpo-39600: In the font configuration window, remove duplicated font names.
- bpo-30780: Add remaining configdialog tests for buttons and highlights and keys tabs.
- bpo-39388: IDLE Settings Cancel button now cancels pending changes
- bpo-38792: Close an IDLE shell calltip if a `KeyboardInterrupt` or
shell restart occurs.  Patch by Zackery Spytz.

### C APIÂ¶

- bpo-35081: Move the `bytes_methods.h` header file to the internal C API
as`pycore_bytes_methods.h` : it only contains private symbols (prefixed
by`_Py` ), except of the`PyDoc_STRVAR_shared()` macro.
- bpo-35081: Move the `dtoa.h` header file to the internal C API as`pycore_dtoa.h` : it only contains private functions (prefixed by`_Py` ). The`math` and`cmath` modules must now be compiled
with the`Py_BUILD_CORE` macro defined.
- bpo-39573: Add `Py_SET_SIZE()` function to set the size of an
object.
- bpo-39500: `PyUnicode_IsIdentifier()` does not call`Py_FatalError()` anymore if the string is not ready.
- bpo-39573: Add `Py_SET_TYPE()` function to set the type of an
object.
- bpo-39573: Add a `Py_SET_REFCNT()` function to set the reference
counter of an object.
- bpo-39542: Convert `PyType_HasFeature()` ,`PyType_Check()` and`PyType_CheckExact()` macros to static inline functions.
- bpo-39542: In the limited C API, `PyObject_INIT()` and`PyObject_INIT_VAR()` are now defined as aliases to`PyObject_Init()` and`PyObject_InitVar()` to make their
implementation opaque. It avoids to leak implementation details in the
limited C API. Exclude the following functions from the limited C API:`_Py_NewReference()` ,`_Py_ForgetReference()` ,`_PyTraceMalloc_NewReference()` and`_Py_GetRefTotal()` .
- bpo-39542: Exclude trashcan mechanism from the limited C API: it requires access to PyTypeObject and PyThreadState structure fields, whereas these structures are opaque in the limited C API.
- bpo-39511: The `PyThreadState_Clear()` function now calls the`PyThreadState.on_delete` callback. Previously, that happened in`PyThreadState_Delete()` .
- bpo-38076: Fix to clear the interpreter state only after clearing module globals to guarantee module state access from C Extensions during runtime destruction
- bpo-39245: The Vectorcall API (PEP 590) was made public, adding the functions `PyObject_Vectorcall` ,`PyObject_VectorcallMethod` ,`PyVectorcall_Function` ,`PyObject_CallOneArg` ,`PyObject_CallMethodNoArgs` ,`PyObject_CallMethodOneArg` ,`PyObject_FastCallDict` , and the flag`Py_TPFLAGS_HAVE_VECTORCALL` .

## Python 3.9.0 alpha 3Â¶

*Release date: 2020-01-24*

### Core and BuiltinsÂ¶

- bpo-39427: Document all possibilities for the `-X` options in the
command line help section. Patch by Pablo Galindo.
- bpo-39421: Fix possible crashes when operating with the functions in the `heapq` module and custom comparison operators.
- bpo-39386: Prevent double awaiting of async iterator.
- bpo-17005: Add `functools.TopologicalSorter` to the`functools` module to offers functionality to perform topological
sorting of graphs. Patch by Pablo Galindo, Tim Peters and Larry Hastings.
- bpo-39320: Replace four complex bytecodes for building sequences with three simpler ones. The following four bytecodes have been removed: 
  - BUILD_LIST_UNPACK
  - BUILD_TUPLE_UNPACK
  - BUILD_SET_UNPACK
  - BUILD_TUPLE_UNPACK_WITH_CALL
 The following three bytecodes have been added: 
  - LIST_TO_TUPLE
  - LIST_EXTEND
  - SET_UPDATE
- bpo-39336: Import loaders which publish immutable module objects can now publish immutable packages in addition to individual modules.
- bpo-39322: Added a new function `gc.is_finalized()` to check if an
object has been finalized by the garbage collector. Patch by Pablo
Galindo.
- bpo-39048: Improve the displayed error message when incorrect types are passed to `async with` statements by looking up the`__aenter__()` special method before the`__aexit__()` special method when entering
an asynchronous context manager. Patch by GÃ©ry Ogam.
- bpo-39235: Fix AST end location for lone generator expression in function call, e.g. f(i for i in a).
- bpo-39209: Correctly handle multi-line tokens in interactive mode. Patch by Pablo Galindo.
- bpo-1635741: Port _json extension module to multiphase initialization ( **PEP 489** ).
- bpo-39216: Fix constant folding optimization for positional only arguments - by Anthony Sottile.
- bpo-39215: Fix `SystemError` when nested function has annotation on
positional-only argument - by Anthony Sottile.
- bpo-39200: Correct the error message when calling the `min()` or`max()` with no arguments. Patch by Donghee Na.
- bpo-39200: Correct the error message when trying to construct `range` objects with no arguments. Patch by Pablo Galindo.
- bpo-39166: Fix incorrect line execution reporting in trace functions when tracing the last iteration of asynchronous for loops. Patch by Pablo Galindo.
- bpo-39114: Fix incorrect line execution reporting in trace functions when tracing exception handlers with name binding. Patch by Pablo Galindo.
- bpo-39156: Split the COMPARE_OP bytecode instruction into four distinct instructions. 
  - COMPARE_OP for rich comparisons
  - IS_OP for âisâ and âis notâ tests
  - CONTAINS_OP for âinâ and âis notâ tests
  - JUMP_IF_NOT_EXC_MATCH for checking exceptions in âtry-exceptâ statements.
 This improves the clarity of the interpreter and should provide a modest speedup.
- bpo-38588: Fix possible crashes in dict and list when calling `PyObject_RichCompareBool()` .
- bpo-13601: By default, `sys.stderr` is line-buffered now, even if`stderr` is redirected to a file. You can still make`sys.stderr` unbuffered by passing the`-u` command-line option or setting the`PYTHONUNBUFFERED` environment variable.(Contributed by Jendrik Seipp in bpo-13601.)
- bpo-38610: Fix possible crashes in several list methods by holding strong references to list elements when calling `PyObject_RichCompareBool()` .
- bpo-32021: Include brotli .br encoding in mimetypes encodings_map

### LibraryÂ¶

- bpo-39430: Fixed race condition in lazy imports in `tarfile` .
- bpo-39413: The `os.unsetenv()` function is now also available on
Windows.
- bpo-39390: Fixed a regression with the `ignore` callback of`shutil.copytree()` . The argument types are now`str` and`List[str]` again.
- bpo-39395: The `os.putenv()` and`os.unsetenv()` functions are now
always available.
- bpo-39406: If `setenv()` C function is available,`os.putenv()` is
now implemented with`setenv()` instead of`putenv()` , so Python
doesnât have to handle the environment variable memory.
- bpo-39396: Fix `math.nextafter(-0.0, +0.0)` on AIX 7.1.
- bpo-29435: Allow `tarfile.is_tarfile()` to be used with file and
file-like objects, like`zipfile.is_zipfile()` . Patch by William
Woodruff.
- bpo-39377: Removed `encoding` option from`json.loads()` .  It has
been deprecated since Python 3.1.
- bpo-39389: Write accurate compression level metadata in `gzip` archives, rather than always signaling maximum compression.
- bpo-39366: The previously deprecated `xpath()` and`xgtitle()` methods
of`nntplib.NNTP` have been removed.
- bpo-39357: Remove the *buffering* parameter of`bz2.BZ2File` . Since
Python 3.0, it was ignored and using it was emitting`DeprecationWarning` . Pass an open file object, to control how the
file is opened. The*compresslevel* parameter becomes keyword-only.
- bpo-39353: Deprecate binhex4 and hexbin4 standards. Deprecate the `binhex` module and the following`binascii` functions:`b2a_hqx()` ,`a2b_hqx()` ,`rlecode_hqx()` ,`rledecode_hqx()` ,`crc_hqx()` .
- bpo-39351: Remove `base64.encodestring()` and`base64.decodestring()` ,
aliases deprecated since Python 3.1: use`base64.encodebytes()` and`base64.decodebytes()` instead.
- bpo-39350: Remove `fractions.gcd()` function, deprecated since Python
3.5 (bpo-22486): use`math.gcd()` instead.
- bpo-39329: `LMTP` constructor now has an optional*timeout* parameter. Patch by Donghee Na.
- bpo-39313: Add a new `exec_function` option (*âexec-function* in the
CLI) to`RefactoringTool` for making`exec` a function. Patch by
Batuhan Taskaya.
- bpo-39259: `FTP_TLS` and`FTP_TLS` now raise
a`ValueError` if the given timeout for their constructor is zero
to prevent the creation of a non-blocking socket. Patch by Donghee Na.
- bpo-39259: `SMTP` and`SMTP_SSL` now raise
a`ValueError` if the given timeout for their constructor is zero
to prevent the creation of a non-blocking socket. Patch by Donghee Na.
- bpo-39310: Add `math.ulp()` : return the value of the least significant
bit of a float.
- bpo-39297: Improved performance of importlib.metadata distribution discovery and resilients to inaccessible sys.path entries (importlib_metadata v1.4.0).
- bpo-39259: `NNTP` and`NNTP_SSL` now raise a`ValueError` if the given timeout for their constructor is zero to
prevent the creation of a non-blocking socket. Patch by Donghee Na.
- bpo-38901: When you specify prompt=â.â or equivalently python -m venv âprompt . â¦ the basename of the current directory is used to set the created venvâs prompt when itâs activated.
- bpo-39288: Add `math.nextafter()` : return the next floating-point
value after*x* towards*y* .
- bpo-39259: `POP3` and`POP3_SSL` now raise a`ValueError` if the given timeout for their constructor is zero to
prevent the creation of a non-blocking socket. Patch by Donghee Na.
- bpo-39242: Updated the Gmane domain from news.gmane.org to news.gmane.io which is used for examples of `NNTP` news reader server and
nntplib tests.
- bpo-35292: Proxy the `SimpleHTTPRequestHandler.guess_type` to`mimetypes.guess_type` so the`mimetypes.init` is called lazily to
avoid unnecessary costs when`http.server` module is imported.
- bpo-39239: The `select.epoll.unregister()` method no longer ignores
the`EBADF` error.
- bpo-38907: In http.server script, restore binding to IPv4 on Windows.
- bpo-39152: Fix ttk.Scale.configure([name]) to return configuration tuple for name or all options. Giovanni Lombardo contributed part of the patch.
- bpo-39198: If an exception were to be thrown in `Logger.isEnabledFor` (say, by asyncio timeouts or stopit) , the`logging` global lock may not
be released appropriately, resulting in deadlock.  This change wraps that
block of code with`try...finally` to ensure the lock is released.
- bpo-39191: Perform a check for running loop before starting a new task in `loop.run_until_complete()` to fail fast; it prevents the side effect of
new task spawning before exception raising.
- bpo-38871: Correctly parenthesize filter-based statements that contain lambda expressions in `lib2to3` . Patch by Donghee Na.
- bpo-39142: A change was made to logging.config.dictConfig to avoid converting instances of named tuples to ConvertingTuple. Itâs assumed that named tuples are too specialised to be treated like ordinary tuples; if a user of named tuples requires ConvertingTuple functionality, they will have to implement that themselves in their named tuple class.
- bpo-39158: ast.literal_eval() now supports empty sets.
- bpo-39129: Fix import path for `asyncio.TimeoutError`
- bpo-39057: `urllib.request.proxy_bypass_environment()` now ignores
leading dots and no longer ignores a trailing newline.
- bpo-39056: Fixed handling invalid warning category in the -W option. No longer import the re module if it is not needed.
- bpo-39055: `base64.b64decode()` with`validate=True` raises now a
binascii.Error if the input ends with a single`\n` .
- bpo-21600: Fix `mock.patch.stopall()` to stop active patches that were
created with`mock.patch.dict()` .
- bpo-39019: Implement dummy `__class_getitem__` for`tempfile.SpooledTemporaryFile` .
- bpo-39019: Implement dummy `__class_getitem__` for`subprocess.Popen` ,`subprocess.CompletedProcess`
- bpo-38914: Adjusted the wording of the warning issued by distutilsâ `check` command when the`author` and`maintainer` fields are
supplied but no corresponding e-mail field (`author_email` or`maintainer_email` ) is found. The wording now reflects the fact that
these fields are suggested, but not required. Patch by Juergen Gmach.
- bpo-38878: Fixed __subclasshook__ of `os.PathLike` to return a
correct result upon inheritance. Patch by Bar Harel.
- bpo-38615: `IMAP4` and`IMAP4_SSL` now
have an optional*timeout* parameter for their constructors. Also, the`open()` method now has an optional*timeout* parameter
with this change. The overridden methods of`IMAP4_SSL` and`IMAP4_stream` were applied to this change. Patch by
Donghee Na.
- bpo-35182: Fixed `Popen.communicate()` subsequent call crash when the
child process has already closed any piped standard stream, but still
continues to be running. Patch by Andriy Maletsky.
- bpo-38630: On Unix, `subprocess.Popen.send_signal()` now polls the
process status. Polling reduces the risk of sending a signal to the wrong
process if the process completed, the`subprocess.Popen.returncode` attribute is still`None` , and the pid has been reassigned (recycled) to
a new different process.
- bpo-38536: Removes trailing space in formatted currency with `international=True` and a locale with symbol following value. E.g.`locale.currency(12.34, international=True)` returned`'12,34 EUR '` instead of`'12,34 EUR'` .
- bpo-38473: Use signature from inner mock for autospecced methods attached with `unittest.mock.attach_mock()` . Patch by Karthikeyan Singaravelan.
- bpo-38361: Fixed an issue where `ident` could include a leading path
separator when`syslog.openlog()` was called without arguments.
- bpo-38293: Add `copy.copy()` and`copy.deepcopy()` support to`property()` objects.
- bpo-37958: Added the pstats.Stats.get_profile_dict() method to return the profile data as a StatsProfile instance.
- bpo-28367: Termios magic constants for the following baud rates: - B500000 - B576000 - B921600 - B1000000 - B1152000 - B1500000 - B2000000 - B2500000 - B3000000 - B3500000 - B4000000 Patch by Andrey Smirnov

### DocumentationÂ¶

- bpo-39381: Mention in docs that `asyncio.get_event_loop()` implicitly
creates new event loop only if called from the main thread.
- bpo-38918: Add an entry for `__module__` in the âfunctionâ & âmethodâ
sections of the`inspect` docsâ Types and members table.
- bpo-3530: In the `ast` module documentation, fix a misleading`NodeTransformer` example and add advice on when to use the`fix_missing_locations` function.

### BuildÂ¶

- bpo-39395: On non-Windows platforms, the `setenv()` and`unsetenv()` functions are now required to build Python.
- bpo-39160: Updated the documentation in `./configure --help` to show
default values, reference documentation where required and add additional
explanation where needed.
- bpo-39144: The ctags and etags build targets both include Modules/_ctypes and Python standard library source files.

### IDLEÂ¶

- bpo-39050: Make IDLE Settings dialog Help button work again.
- bpo-34118: Tag memoryview, range, and tuple as classes, the same as list, etcetera, in the library manual built-in functions list.
- bpo-32989: Add tests for editor newline_and_indent_event method. Remove dead code from pyparse find_good_parse_start method.

### C APIÂ¶

- bpo-39372: Clean header files of interfaces defined but with no implementation. The public API symbols being removed are: `_PyBytes_InsertThousandsGroupingLocale` ,`_PyBytes_InsertThousandsGrouping` ,`_Py_InitializeFromArgs` ,`_Py_InitializeFromWideArgs` ,`_PyFloat_Repr` ,`_PyFloat_Digits` ,`_PyFloat_DigitsInit` ,`PyFrame_ExtendStack` ,`_PyAIterWrapper_Type` ,`PyNullImporter_Type` ,`PyCmpWrapper_Type` ,`PySortWrapper_Type` ,`PyNoArgsFunction` .
- bpo-39164: Add a private `_PyErr_GetExcInfo()` function to retrieve
exception information of the specified Python thread state.

## Python 3.9.0 alpha 2Â¶

*Release date: 2019-12-18*

### SecurityÂ¶

- bpo-38945: Newline characters have been escaped when performing uu encoding to prevent them from overflowing into to content section of the encoded file. This prevents malicious or accidental modification of data during the decoding process.
- bpo-37228: Due to significant security concerns, the *reuse_address* parameter of`asyncio.loop.create_datagram_endpoint()` is no longer
supported. This is because of the behavior of`SO_REUSEADDR` in UDP. For
more details, see the documentation for`loop.create_datagram_endpoint()` . (Contributed by Kyle Stanley, Antoine
Pitrou, and Yury Selivanov in bpo-37228.)
- bpo-38804: Fixes a ReDoS vulnerability in `http.cookiejar` . Patch by
Ben Caller.

### Core and BuiltinsÂ¶

- bpo-39028: Slightly improve the speed of keyword argument parsing with many kwargs by strengthening the assumption that kwargs are interned strings.
- bpo-39080: Fix the value of *end_col_offset* for Starred Expression AST
nodes when they are among the elements in the*args* attribute of Call AST
nodes.
- bpo-39031: When parsing an âelifâ node, lineno and col_offset of the node now point to the âelifâ keyword and not to its condition, making it consistent with the âifâ node. Patch by Lysandros Nikolaou.
- bpo-20443: In Python 3.9.0a1, sys.argv[0] was made an absolute path if a filename was specified on the command line. Revert this change, since most users expect sys.argv to be unmodified.
- bpo-39008: `PySys_Audit()` now requires`Py_ssize_t` to be used
for size arguments in the format string, regardless of whether`PY_SSIZE_T_CLEAN` was defined at include time.
- bpo-38673: In REPL mode, donât switch to PS2 if the line starts with comment or whitespace. Based on work by Batuhan TaÅkaya.
- bpo-38922: Calling `replace` on a code object now raises the`code.__new__` audit event.
- bpo-38920: Add audit hooks for when `sys.excepthook()` and`sys.unraisablehook()` are invoked.
- bpo-38892: Improve documentation for audit events table and functions.
- bpo-38852: Set the thread stack size to 8 Mb for debug builds on android platforms.
- bpo-38858: Each Python subinterpreter now has its own âsmall integer singletonsâ: numbers in [-5; 257] range. It is no longer possible to change the number of small integers at build time by overriding `NSMALLNEGINTS` and`NSMALLPOSINTS` macros: macros should now be
modified manually in`pycore_pystate.h` header file.
- bpo-36854: The garbage collector state becomes per interpreter ( `PyInterpreterState.gc` ), rather than being global
(`_PyRuntimeState.gc` ).
- bpo-38835: The `PyFPE_START_PROTECT()` and`PyFPE_END_PROTECT()` macros are empty: they have been doing nothing for the last year, so stop
using them.
- bpo-38328: Sped up the creation time of constant `list` and`set` displays. Patch by Brandt Bucher.
- bpo-38707: `MainThread.native_id` is now correctly reset in child
processes spawned using`multiprocessing.Process` , instead of
retaining the parentâs value.
- bpo-38629: Added `__floor__` and`__ceil__` methods to float object.
Patch by Batuhan TaÅkaya.
- bpo-27145: int + int and int - int operators can now return small integer singletons. Patch by hongweipeng.
- bpo-38021: Provide a platform tag for AIX that is sufficient for PEP425 binary distribution identification. Patch by Michael Felt.
- bpo-35409: Ignore GeneratorExit exceptions when throwing an exception into the aclose coroutine of an asynchronous generator.
- bpo-33387: Removed WITH_CLEANUP_START, WITH_CLEANUP_FINISH, BEGIN_FINALLY, END_FINALLY, CALL_FINALLY and POP_FINALLY bytecodes. Replaced with RERAISE and WITH_EXCEPT_START bytecodes. The compiler now generates different code for exceptional and non-exceptional branches for âwithâ and âtry-exceptâ statements. For âtry-finallyâ statements the âfinallyâ block is replicated for each exit from the âtryâ body.

### LibraryÂ¶

- bpo-39033: Fix `NameError` in`zipimport` . Patch by Karthikeyan
Singaravelan.
- bpo-39022: Update importlib.metadata to include improvements from importlib_metadata 1.3 including better serialization of EntryPoints and improved documentation for custom finders.
- bpo-39006: Fix asyncio when the ssl module is missing: only check for ssl.SSLSocket instance if the ssl module is available.
- bpo-38708: Fix a potential IndexError in email parser when parsing an empty msg-id.
- bpo-38698: Add a new `InvalidMessageID` token to email parser to
represent invalid Message-ID headers.  Also, add defects when there is
remaining value after parsing the header.
- bpo-38994: Implement `__class_getitem__` for`os.PathLike` ,`pathlib.Path` .
- bpo-38979: Return class from `ContextVar.__class_getitem__` to simplify
subclassing.
- bpo-38978: Implement `__class_getitem__` on asyncio objects (Future,
Task, Queue). Patch by Batuhan Taskaya.
- bpo-38916: `array.array` : Remove`tostring()` and`fromstring()` methods. They were aliases to`tobytes()` and`frombytes()` , deprecated since Python 3.2.
- bpo-38986: Make repr of C accelerated TaskWakeupMethWrapper the same as of pure Python version.
- bpo-38982: Fix asyncio `PidfdChildWatcher` : handle`waitpid()` error.
If`waitpid()` is called elsewhere,`waitpid()` call fails with`ChildProcessError` : use return code 255 in this case, and log a
warning. It ensures that the pidfd file descriptor is closed if this error
occurs.
- bpo-38529: Drop too noisy asyncio warning about deletion of a stream without explicit `.close()` call.
- bpo-27413: Added ability to pass through `ensure_ascii` options to
json.dumps in the`json.tool` command-line interface.
- bpo-38634: The `readline` module now detects if Python is linked to
libedit at runtime on all platforms.  Previously, the check was only done
on macOS.
- bpo-33684: Fix `json.tool` failed to read a JSON file with non-ASCII
characters when locale encoding is not UTF-8.
- bpo-38698: Prevent UnboundLocalError to pop up in parse_message_id. parse_message_id() was improperly using a token defined inside an exception handler, which was raising `UnboundLocalError` on parsing an
invalid value. Patch by Claudiu Popa.
- bpo-38927: Use `python -m pip` instead of`pip` to upgrade
dependencies in venv.
- bpo-26730: Fix `SpooledTemporaryFile.rollover()` might corrupt the file
when it is in text mode. Patch by Serhiy Storchaka.
- bpo-38881: random.choices() now raises a ValueError when all the weights are zero.
- bpo-38876: Raise pickle.UnpicklingError when loading an item from memo for invalid input. The previous code was raising a `KeyError` for both the Python and C
implementation. This was caused by the specified index of an invalid input
which did not exist in the memo structure, where the pickle stores what
objects it has seen. The malformed input would have caused either a`BINGET` or`LONG_BINGET` load from the memo, leading to a`KeyError` as the determined index was bogus. Patch by Claudiu Popa.
- bpo-38688: Calling func: `shutil.copytree` to copy a directory tree from
one directory to another subdirectory resulted in an endless loop and a
RecursionError. A fix was added to consume an iterator and create the list
of the entries to be copied, avoiding the recursion for newly created
directories. Patch by Bruno P. Kinoshita.
- bpo-38863: Improve `is_cgi()` function in`http.server` , which
enables processing the case that cgi directory is a child of another
directory other than root.
- bpo-37838: `typing.get_type_hints()` properly handles functions
decorated with`functools.wraps()` .
- bpo-38870: Expose `ast.unparse()` as a function of the`ast` module that can be used to unparse an`ast.AST` object and produce
a string with code that would produce an equivalent`ast.AST` object when parsed. Patch by Pablo Galindo and Batuhan Taskaya.
- bpo-38859: AsyncMock now returns StopAsyncIteration on the exhaustion of a side_effects iterable. Since PEP-479 its Impossible to raise a StopIteration exception from a coroutine.
- bpo-38857: AsyncMock fix for return values that are awaitable types. This also covers side_effect iterable values that happened to be awaitable, and wraps callables that return an awaitable type. Before these awaitables were being awaited instead of being returned as is.
- bpo-38834: `typing.TypedDict` subclasses now track which keys are
optional using the`__required_keys__` and`__optional_keys__` attributes, to enable runtime validation by downstream projects.  Patch by
Zac Hatfield-Dodds.
- bpo-38821: Fix unhandled exceptions in `argparse` when
internationalizing error messages for arguments with`nargs` set to
special (non-integer) values. Patch by Federico Bond.
- bpo-38820: Make Python compatible with OpenSSL 3.0.0. `ssl.SSLSocket.getpeercert()` no longer returns IPv6 addresses with a
trailing new line.
- bpo-38811: Fix an unhandled exception in `pathlib` when`os.link()` is missing. Patch by Toke HÃ¸iland-JÃ¸rgensen.
- bpo-38686: Added support for multiple `qop` values in`urllib.request.AbstractDigestAuthHandler` .
- bpo-38712: Add the Linux-specific `signal.pidfd_send_signal()` function, which allows sending a signal to a process identified by a file
descriptor rather than a pid.
- bpo-38348: Add `-i` and`--indent` (indentation level), and`--no-type-comments` (type comments) command line options to ast parsing
tool.
- bpo-37523: Change `zipfile.ZipExtFile` to raise`ValueError` when
trying to access the underlying file object after it has been closed. This
new behavior is consistent with how accessing closed files is handled in
other parts of Python.
- bpo-38045: Improve the performance of `enum._decompose()` in`enum` . Patch by hongweipeng.
- bpo-36820: Break cycle generated when saving an exception in socket.py, codeop.py and dyld.py as they keep alive not only the exception but user objects through the `__traceback__` attribute. Patch by Mario Corchero.
- bpo-36406: Handle namespace packages in `doctest` . Patch by
Karthikeyan Singaravelan.
- bpo-34776: Fix dataclasses to support forward references in type annotations
- bpo-20928: ElementTree supports recursive XInclude processing. Patch by Stefan Behnel.
- bpo-29636: Add whitespace options for formatting JSON with the `json.tool` CLI. The following mutually exclusive options are now
supported:`--indent` for setting the indent level in spaces;`--tab` for indenting with tabs;`--no-indent` for suppressing newlines; and`--compact` for suppressing all whitespace. The default behavior remains
the same as`--indent=4` .

### DocumentationÂ¶

- bpo-38928: Correct when venvâs `upgrade_dependencies()` and`--upgrade-deps` are added.
- bpo-38899: Update documentation to state that to activate virtual environments under fish one should use `source` , not`.` as documented
at https://fishshell.com/docs/current/cmds/source.html.
- bpo-22377: Improves documentation of the values that `datetime.datetime.strptime()` accepts for`%Z` . Patch by Karl
Dubost.

### TestsÂ¶

- bpo-38546: Fix test_ressources_gced_in_workers() of test_concurrent_futures: explicitly stop the manager to prevent leaking a child process running in the background after the test completes.
- bpo-38546: Multiprocessing and concurrent.futures tests now stop the resource tracker process when tests complete.
- bpo-38614: Replace hardcoded timeout constants in tests with new `test.support` constants:`LOOPBACK_TIMEOUT` ,`INTERNET_TIMEOUT` ,`SHORT_TIMEOUT` and`LONG_TIMEOUT` . It becomes easier to adjust these four
timeout constants for all tests at once, rather than having to adjust
every single test file.
- bpo-38547: Fix test_pty: if the process is the session leader, closing the master file descriptor raises a SIGHUP signal: simply ignore SIGHUP when running the tests.
- bpo-38992: Fix a test for `math.fsum()` that was failing due to
constant folding.
- bpo-38991: `test.support` :`run_python_until_end()` ,`assert_python_ok()` and`assert_python_failure()` functions no longer strip
whitespaces from stderr. Remove`test.support.strip_python_stderr()` function.
- bpo-38965: Fix test_faulthandler on GCC 10. Use the âvolatileâ keyword in `faulthandler._stack_overflow()` to prevent tail call optimization on
any compiler, rather than relying on compiler specific pragma.
- bpo-38875: test_capi: trashcan tests now require the test âcpuâ resource.
- bpo-38841: Skip asyncio test_create_datagram_endpoint_existing_sock_unix on platforms lacking a functional bind() for named unix domain sockets.
- bpo-38692: Skip the test_posix.test_pidfd_open() test if `os.pidfd_open()` fails with a`PermissionError` . This situation
can happen in a Linux sandbox using a syscall whitelist which doesnât
allow the`pidfd_open()` syscall yet.
- bpo-38839: Fix some unused functions in tests. Patch by Adam Johnson.
- bpo-38669: Raise `TypeError` when passing target as a string with`unittest.mock.patch.object()` .
- bpo-37957: test.regrtest now can receive a list of test patterns to ignore (using the -i/âignore argument) or a file with a list of patterns to ignore (using the âignore-file argument). Patch by Pablo Galindo.

### BuildÂ¶

- bpo-37404: `asyncio` now raises`TypeError` when calling
incompatible methods with an`ssl.SSLSocket` socket.  Patch by Ido
Michael.
- bpo-36500: Added an optional âregenâ project to the Visual Studio solution that will regenerate all grammar, tokens, and opcodes.

### WindowsÂ¶

### macOSÂ¶

- bpo-37931: Fixed a crash on OSX dynamic builds that occurred when re-initializing the posix module after a Py_Finalize if the environment had changed since the previous `import posix` . Patch by BenoÃ®t Hudson.

### IDLEÂ¶

- bpo-38944: Escape key now closes IDLE completion windows. Patch by Johnny Najera.
- bpo-38943: Fix IDLE autocomplete windows not always appearing on some systems. Patch by Johnny Najera.
- bpo-38862: âStrip Trailing Whitespaceâ on the Format menu removes extra newlines at the end of non-shell files.
- bpo-38636: Fix IDLE Format menu tab toggle and file indent width. These functions (default shortcuts Alt-T and Alt-U) were mistakenly disabled in 3.7.5 and 3.8.0.

### C APIÂ¶

- bpo-38896: Remove `PyUnicode_ClearFreeList()` function: the Unicode free
list has been removed in Python 3.3.
- bpo-37340: Remove `PyMethod_ClearFreeList()` and`PyCFunction_ClearFreeList()` functions: the free lists of bound method
objects have been removed.
- bpo-38835: Exclude `PyFPE_START_PROTECT()` and`PyFPE_END_PROTECT()` macros of`pyfpe.h` from`Py_LIMITED_API` (stable API).

## Python 3.9.0 alpha 1Â¶

*Release date: 2019-11-19*

### SecurityÂ¶

- bpo-38722: `runpy` now uses`io.open_code()` to open code files.
Patch by Jason Killen.
- bpo-38622: Add additional audit events for the `ctypes` module.
- bpo-38418: Fixes audit event for `os.system()` to be named`os.system` .
- bpo-38243: Escape the server title of `xmlrpc.server.DocXMLRPCServer` when rendering the document page as
HTML. (Contributed by Donghee Na in bpo-38243.)
- bpo-38174: Update vendorized expat library version to 2.2.8, which resolves **CVE 2019-15903** .
- bpo-37764: Fixes email._header_value_parser.get_unstructured going into an infinite loop for a specific case in which the email header does not have trailing whitespace, and the case in which it contains an invalid encoded word. Patch by Ashwin Ramaswami.
- bpo-37461: Fix an infinite loop when parsing specially crafted email headers. Patch by Abhilash Raj.
- bpo-37363: Adds audit events for the range of supported run commands (see Command line and environment).
- bpo-37463: ssl.match_hostname() no longer accepts IPv4 addresses with additional text after the address and only quad-dotted notation without trailing whitespaces. Some inet_aton() implementations ignore whitespace and all data after whitespace, e.g. â127.0.0.1 whateverâ.
- bpo-37363: Adds audit events for `ensurepip` ,`ftplib` ,`glob` ,`imaplib` ,`nntplib` ,`pdb` ,`poplib` ,`shutil` ,`smtplib` ,`sqlite3` ,`subprocess` ,`telnetlib` ,`tempfile` and`webbrowser` , as well as`os.listdir()` ,`os.scandir()` and`breakpoint()` .
- bpo-37364: `io.open_code()` is now used when reading`.pth` files.
- bpo-34631: Updated OpenSSL to 1.1.1c in Windows installer
- bpo-34155: Fix parsing of invalid email addresses with more than one `@` (e.g. a@b@c.com.) to not return the part before 2nd`@` as valid email
address. Patch by maxking & jpic.

### Core and BuiltinsÂ¶

- bpo-38631: Replace `Py_FatalError()` call with a regular`RuntimeError` exception in`float.__getformat__()` .
- bpo-38639: Optimized `math.floor()` ,`math.ceil()` and`math.trunc()` for floats.
- bpo-38640: Fixed a bug in the compiler that was causing to raise in the presence of break statements and continue statements inside always false while loops. Patch by Pablo Galindo.
- bpo-38613: Optimized some set operations (e.g. `|` ,`^` , and`-` ) of`dict_keys` .`d.keys() | other` was slower than`set(d) | other` but
they are almost same performance for now.
- bpo-28029: `"".replace("", s, n)` now returns`s` instead of an empty
string for all non-zero`n` .  There are similar changes for`bytes` and`bytearray` objects.
- bpo-38535: Fixed line numbers and column offsets for AST nodes for calls without arguments in decorators.
- bpo-38525: Fix a segmentation fault when using reverse iterators of empty `dict` objects. Patch by Donghee Na and Inada Naoki.
- bpo-38465: `bytearray` ,`array` and`mmap` objects allow now to export more than`2**31` buffers at a time.
- bpo-38469: Fixed a bug where the scope of named expressions was not being resolved correctly in the presence of the *global* keyword. Patch by Pablo
Galindo.
- bpo-38437: Activate the `GC_DEBUG` macro for debug builds of the
interpreter (when`Py_DEBUG` is set). Patch by Pablo Galindo.
- bpo-38379: When the garbage collector makes a collection in which some objects resurrect (they are reachable from outside the isolated cycles after the finalizers have been executed), do not block the collection of all objects that are still unreachable. Patch by Pablo Galindo and Tim Peters.
- bpo-38379: When cyclic garbage collection (gc) runs finalizers that resurrect unreachable objects, the current gc run ends, without collecting any cyclic trash. However, the statistics reported by `collect()` and`get_stats()` claimed that all cyclic trash found was collected, and
that the resurrected objects were collected.   Changed the stats to report
that none were collected.
- bpo-38392: In debug mode, `PyObject_GC_Track()` now calls`tp_traverse()` of the object type to ensure that the object is valid:
test that objects visited by`tp_traverse()` are valid.
- bpo-38210: Remove unnecessary intersection and update set operation in dictview with empty set. (Contributed by Donghee Na in bpo-38210.)
- bpo-38402: Check the error from the systemâs underlying `crypt` or`crypt_r` .
- bpo-37474: On FreeBSD, Python no longer calls `fedisableexcept()` at
startup to control the floating-point control mode. The call became
useless since FreeBSD 6: it became the default mode.
- bpo-38006: Fix a bug due to the interaction of weakrefs and the cyclic garbage collector. We must clear any weakrefs in garbage in order to prevent their callbacks from executing and causing a crash.
- bpo-38317: Fix warnings options priority: `PyConfig.warnoptions` has the
highest priority, as stated in the**PEP 587** .
- bpo-38310: Predict `BUILD_MAP_UNPACK_WITH_CALL` ->`CALL_FUNCTION_EX` opcode pairs in the main interpreter loop. Patch by Brandt Bucher.
- bpo-36871: Improve error handling for the assert_has_calls and assert_has_awaits methods of mocks. Fixed a bug where any errors encountered while binding the expected calls to the mockâs spec were silently swallowed, leading to misleading error output.
- bpo-11410: Better control over symbol visibility is provided through use of the visibility attributes available in gcc >= 4.0, provided in a uniform way across POSIX and Windows. The POSIX build files have been updated to compile with -fvisibility=hidden, minimising exported symbols.
- bpo-38219: Optimized the `dict` constructor and the`update()` method for the case when the argument is a dict.
- bpo-38236: Python now dumps path configuration if it fails to import the Python codecs of the filesystem and stdio encodings.
- bpo-38013: Allow to call `async_generator_athrow().throw(...)` even for
non-started async generator helper. It fixes annoying warning at the end
of`asyncio.run()` call.
- bpo-38124: Fix an off-by-one error in PyState_AddModule that could cause out-of-bounds memory access.
- bpo-38116: The select module is now PEP-384 compliant and no longer has static state
- bpo-38113: ast module updated to PEP-384 and all statics removed
- bpo-38076: The struct module is now PEP-384 compatible
- bpo-38075: The random module is now PEP-384 compatible
- bpo-38074: zlib module made PEP-384 compatible
- bpo-38073: Make pwd extension module PEP-384 compatible
- bpo-38072: grp module made PEP-384 compatible
- bpo-38069: Make _posixsubprocess PEP-384 compatible
- bpo-38071: Make termios extension module PEP-384 compatible
- bpo-38005: Fixed comparing and creating of InterpreterID and ChannelID.
- bpo-36946: Fix possible signed integer overflow when handling slices. Patch by hongweipeng.
- bpo-37994: Fixed silencing arbitrary errors if an attribute lookup fails in several sites. Only AttributeError should be silenced.
- bpo-8425: Optimize set difference_update for the case when the other set is much larger than the base set. (Suggested by Evgeny Kapun with code contributed by Michele OrrÃ¹).
- bpo-37966: The implementation of `is_normalized()` has
been greatly sped up on strings that arenât normalized, by implementing
the full normalization-quick-check algorithm from the Unicode standard.
- bpo-37947: Adjust correctly the recursion level in the symtable generation for named expressions. Patch by Pablo Galindo.
- bpo-37812: The `CHECK_SMALL_INT` macro used inside`Object/longobject.c` has been replaced with an explicit`return` at each call site.
- bpo-37751: Fix `codecs.lookup()` to normalize the encoding name the
same way than`encodings.normalize_encoding()` , except that`codecs.lookup()` also converts the name to lower case.
- bpo-37830: Fixed compilation of `break` and`continue` in the`finally` block when the corresponding`try` block contains`return` with a non-constant value.
- bpo-20490: Improve import error message for partially initialized module on circular `from` imports - by Anthony Sottile.
- bpo-37840: Fix handling of negative indices in `sq_item` of`bytearray` . Patch by
Sergey Fedoseev.
- bpo-37802: Slightly improve performance of `PyLong_FromUnsignedLong()` ,`PyLong_FromUnsignedLongLong()` and`PyLong_FromSize_t()` . Patch by Sergey Fedoseev.
- bpo-37409: Ensure explicit relative imports from interactive sessions and scripts (having no parent package) always raise ImportError, rather than treating the current module as the package. Patch by Ben Lewis.
- bpo-32912: Reverted bpo-32912: emitting `SyntaxWarning` instead
of`DeprecationWarning` for invalid escape sequences in string and
bytes literals.
- bpo-37757: **PEP 572** : As described in the PEP, assignment expressions now
raise`SyntaxError` when their interaction with comprehension scoping
results in an ambiguous target scope.The `TargetScopeError` subclass originally proposed by the PEP has been
removed in favour of just raising regular syntax errors for the disallowed
cases.
- bpo-36279: Fix potential use of uninitialized memory in `os.wait3()` .
- bpo-36311: Decoding bytes objects larger than 2GiB is faster and no longer fails when a multibyte characters spans a chunk boundary.
- bpo-34880: The `assert` statement now works properly if the`AssertionError` exception is being shadowed. Patch by Zackery Spytz.
- bpo-37340: Removed object cache ( `free_list` ) for bound method objects.
Temporary bound method objects are less used than before thanks to the`LOAD_METHOD` opcode and the`_PyObject_VectorcallMethod` C API.
- bpo-37648: Fixed minor inconsistency in `list.__contains__()` ,`tuple.__contains__()` and a few other places. The collectionâs item
is now always at the left and the needle is on the right of`==` .
- bpo-37444: Update differing exception between `builtins.__import__()` and`importlib.__import__()` .
- bpo-37619: When adding a wrapper descriptor from one class to a different class (for example, setting `__add__ = str.__add__` on an`int` subclass), an exception is correctly raised when the operator is called.
- bpo-37593: Swap the positions of the *posonlyargs* and*args* parameters
in the constructor of`ast.parameters` nodes.
- bpo-37543: Optimized pymalloc for non PGO build.
- bpo-37537: Compute allocated pymalloc blocks inside _Py_GetAllocatedBlocks(). This slows down _Py_GetAllocatedBlocks() but gives a small speedup to _PyObject_Malloc() and _PyObject_Free().
- bpo-37467: Fix `sys.excepthook()` and`PyErr_Display()` if a
filename is a bytes string. For example, for a SyntaxError exception where
the filename attribute is a bytes string.
- bpo-37433: Fix `SyntaxError` indicator printing too many spaces for
multi-line strings - by Anthony Sottile.
- bpo-37417: `bytearray.extend()` now correctly handles errors that
arise during iteration. Patch by Brandt Bucher.
- bpo-37414: The undocumented `sys.callstats()` function has been removed.
Since Python 3.7, it was deprecated and always returned`None` . It
required a special build option`CALL_PROFILE` which was already removed
in Python 3.7.
- bpo-37392: Remove `sys.getcheckinterval()` and`sys.setcheckinterval()` functions. They were deprecated since Python
3.2. Use`sys.getswitchinterval()` and`sys.setswitchinterval()` instead. Remove also`check_interval` field of the`PyInterpreterState` structure.
- bpo-37388: In development mode and in debug build, *encoding* and*errors* arguments are now checked on string encoding and decoding operations.
Examples:`open()` ,`str.encode()` and`bytes.decode()` .By default, for best performances, the *errors* argument is only checked
at the first encoding/decoding error, and the*encoding* argument is
sometimes ignored for empty strings.
- bpo-37348: Optimized decoding short ASCII string with UTF-8 and ascii codecs. `b"foo".decode()` is about 15% faster.  Patch by Inada Naoki.
- bpo-24214: Improved support of the surrogatepass error handler in the UTF-8 and UTF-16 incremental decoders.
- bpo-37330: `open()` ,`io.open()` ,`codecs.open()` and`fileinput.FileInput` no longer accept`'U'` (âuniversal
newlineâ) in the file mode. This flag was deprecated since Python 3.3.
- bpo-35224: Reverse evaluation order of key: value in dict comprehensions as proposed in PEP 572. I.e. in `{k: v for ...}` ,`k` will be
evaluated before`v` .
- bpo-37316: Fix the `PySys_Audit()` call in`mmap.mmap` .
- bpo-37300: Remove an unnecessary Py_XINCREF in classobject.c.
- bpo-37269: Fix a bug in the peephole optimizer that was not treating correctly constant conditions with binary operators. Patch by Pablo Galindo.
- bpo-20443: Python now gets the absolute path of the script filename specified on the command line (ex: âpython3 script.pyâ): the __file__ attribute of the __main__ module and sys.path[0] become an absolute path, rather than a relative path.
- bpo-37257: Pythonâs small object allocator ( `obmalloc.c` ) now allows (no
more than) one empty arena to remain available for immediate reuse,
without returning it to the OS.  This prevents thrashing in simple loops
where an arena could be created and destroyed anew on each iteration.
- bpo-37231: The dispatching of type slots to special methods (for example calling `__mul__` when doing`x * y` ) has been made faster.
- bpo-36974: Implemented separate vectorcall functions for every calling convention of builtin functions and methods. This improves performance for calls.
- bpo-37213: Handle correctly negative line offsets in the peephole optimizer. Patch by Pablo Galindo.
- bpo-37219: Remove erroneous optimization for empty set differences.
- bpo-15913: Implement `PyBuffer_SizeFromFormat()` function
(previously documented but not implemented): call`struct.calcsize()` .
Patch by Joannah Nanjekye.
- bpo-36922: Slot functions optimize any callable with `Py_TPFLAGS_METHOD_DESCRIPTOR` instead of only instances of`function` .
- bpo-36974: The slot `tp_vectorcall_offset` is inherited unconditionally
to support`super().__call__()` when the base class uses vectorcall.
- bpo-37160: `threading.get_native_id()` now also supports NetBSD.
- bpo-37077: Add `threading.get_native_id()` support for AIX. Patch by
M. Felt
- bpo-36781: `sum()` has been optimized for boolean values.
- bpo-34556: Add `--upgrade-deps` to venv module. Patch by Cooper Ry Lees
- bpo-20523: `pdb.Pdb` supports ~/.pdbrc in Windows 7. Patch by Tim Hopper
and Dan Lidral-Porter.
- bpo-35551: Updated encodings: - Removed the âtis260â encoding, which was an alias for the nonexistent âtactisâ codec. - Added âmac_centeuroâ as an alias for the mac_latin2 encoding.
- bpo-19072: The `classmethod` decorator can now wrap other
descriptors such as property objects.  Adapted from a patch written by
Graham Dumpleton.
- bpo-27575: Improve speed of dictview intersection by directly using set intersection logic. Patch by David Su.
- bpo-30773: Prohibit parallel running of aclose() / asend() / athrow(). Fix ag_running to reflect the actual running status of the AG.

### LibraryÂ¶

- bpo-36589: The `curses.update_lines_cols()` function now returns`None` instead of`1` on success.
- bpo-38807: Update `TypeError` messages for`os.path.join()` to
include`os.PathLike` objects as acceptable input types.
- bpo-38724: Add a repr for `subprocess.Popen` objects. Patch by Andrey
Doroschenko.
- bpo-38786: pydoc now recognizes and parses HTTPS URLs. Patch by python273.
- bpo-38785: Prevent asyncio from crashing if parent `__init__` is not
called from a constructor of object derived from`asyncio.Future` .
- bpo-38723: `pdb` now uses`io.open_code()` to trigger auditing
events.
- bpo-27805: Allow opening pipes and other non-seekable files in append mode with `open()` .
- bpo-38438: Simplify the `argparse` usage message for`nargs="*"` .
- bpo-38761: WeakSet is now registered as a collections.abc.MutableSet.
- bpo-38716: logging: change RotatingHandler namer and rotator to class-level attributes. This stops __init__ from setting them to None in the case where a subclass defines them with eponymous methods.
- bpo-38713: Add `os.P_PIDFD` constant, which may be passed to`os.waitid()` to wait on a Linux process file descriptor.
- bpo-38692: Add `asyncio.PidfdChildWatcher` , a Linux-specific child
watcher implementation that polls process file descriptors.
- bpo-38692: Expose the Linux `pidfd_open` syscall as`os.pidfd_open()` .
- bpo-38602: Added constants `F_OFD_GETLK` ,`F_OFD_SETLK` and`F_OFD_SETLKW` to the`fcntl` module. Patch by Donghee Na.
- bpo-38334: Fixed seeking backward on an encrypted `zipfile.ZipExtFile` .
- bpo-38312: Add `curses.get_escdelay()` ,`curses.set_escdelay()` ,`curses.get_tabsize()` , and`curses.set_tabsize()` functions - by
Anthony Sottile.
- bpo-38586: Now `fileConfig()` correctly sets the .name
of handlers loaded.
- bpo-38565: Add new cache_parameters() method for functools.lru_cache() to better support pickling.
- bpo-34679: asynci.ProactorEventLoop.close() now only calls signal.set_wakeup_fd() in the main thread.
- bpo-31202: The case the result of `pathlib.WindowsPath.glob()` matches
now the case of the pattern for literal parts.
- bpo-36321: Remove misspelled attribute. The 3.8 changelog noted that this would be removed in 3.9.
- bpo-38521: Fixed erroneous equality comparison in statistics.NormalDist().
- bpo-38493: Added `CLD_KILLED` and`CLD_STOPPED` for`si_code` . Patch by Donghee Na.
- bpo-38478: Fixed a bug in `inspect.signature.bind()` that was causing
it to fail when handling a keyword argument with same name as
positional-only parameter. Patch by Pablo Galindo.
- bpo-33604: Fixed `hmac.new` and`hmac.HMAC` to raise TypeError instead
of ValueError when the digestmod parameter, now required in 3.8, is
omitted. Also clarified the hmac module documentation and docstrings.
- bpo-38378: Parameters *out* and*in* of`os.sendfile()` was renamed to*out_fd* and*in_fd* .
- bpo-38417: Added support for setting the umask in the child process to the subprocess module on POSIX systems.
- bpo-38449: Revert PR 15522, which introduces a regression in `mimetypes.guess_type()` due to improper handling of filenames as
urls.
- bpo-38431: Fix `__repr__` method for`dataclasses.InitVar` to
support typing objects, patch by Samuel Colvin.
- bpo-38109: Add missing `stat.S_IFDOOR` ,`stat.S_IFPORT` ,`stat.S_IFWHT` ,`stat.S_ISDOOR()` ,`stat.S_ISPORT()` , and`stat.S_ISWHT()` values to the Python implementation of`stat` .
- bpo-38422: Clarify docstrings of pathlib suffix(es)
- bpo-38405: Nested subclasses of `typing.NamedTuple` are now
pickleable.
- bpo-38332: Prevent `KeyError` thrown by`_encoded_words.decode()` when given an encoded-word with invalid
content-type encoding from propagating all the way to`email.message.get()` .
- bpo-38371: Deprecated the `split()` method in`_tkinter.TkappType` in favour of the`splitlist()` method which
has more consistent and predictable behavior.
- bpo-38341: Add `smtplib.SMTPNotSupportedError` to the`smtplib` exported names.
- bpo-38319: sendfile() used in socket and shutil modules was raising OverflowError for files >= 2GiB on 32-bit architectures. (patch by Giampaolo Rodola)
- bpo-38242: Revert the new asyncio Streams API
- bpo-13153: OS native encoding is now used for converting between Python strings and Tcl objects. This allows to display, copy and paste to clipboard emoji and other non-BMP characters. Converting strings from Tcl to Python and back now never fails (except MemoryError).
- bpo-38019: Correctly handle pause/resume reading of closed asyncio unix pipe.
- bpo-38163: Child mocks will now detect their type as either synchronous or asynchronous, asynchronous child mocks will be AsyncMocks and synchronous child mocks will be either MagicMock or Mock (depending on their parent type).
- bpo-38161: Removes _AwaitEvent from AsyncMock.
- bpo-38216: Allow the rare code that wants to send invalid http requests from the `http.client` library a way to do so.  The fixes for bpo-30458
led to breakage for some projects that were relying on this ability to
test their own behavior in the face of bad requests.
- bpo-28286: Deprecate opening `GzipFile` for writing
implicitly.  Always specify the*mode* argument for writing.
- bpo-38108: Any synchronous magic methods on an AsyncMock now return a MagicMock. Any asynchronous magic methods on a MagicMock now return an AsyncMock.
- bpo-38265: Update the *length* parameter of`os.pread()` to accept`Py_ssize_t` instead of int.
- bpo-38112: `compileall` has a higher default recursion limit and new
command-line arguments for path manipulation, symlinks handling, and
multiple optimization levels.
- bpo-38248: asyncio: Fix inconsistent immediate Task cancellation
- bpo-38237: The arguments for the builtin pow function are more descriptive. They can now also be passed in as keywords.
- bpo-34002: Improve efficiency in parts of email package by changing while-pop to a for loop, using isdisjoint instead of set intersections.
- bpo-38191: Constructors of `NamedTuple` and`TypedDict` types now accept arbitrary keyword argument
names, including âclsâ, âselfâ, âtypenameâ, â_typenameâ, âfieldsâ and
â_fieldsâ.
- bpo-38155: Add `__all__` to`datetime` . Patch by Tahia Khan.
- bpo-38185: Fixed case-insensitive string comparison in `sqlite3.Row` indexing.
- bpo-38136: Changes AsyncMock call count and await count to be two different counters. Now await count only counts when a coroutine has been awaited, not when it has been called, and vice-versa. Update the documentation around this.
- bpo-37828: Fix default mock name in `unittest.mock.Mock.assert_called()` exceptions. Patch by Abraham
Toriz Cruz.
- bpo-38175: Fix a memory leak in comparison of `sqlite3.Row` objects.
- bpo-33936: _hashlib no longer calls obsolete OpenSSL initialization function with OpenSSL 1.1.0+.
- bpo-34706: Preserve subclassing in inspect.Signature.from_callable.
- bpo-38153: Names of hashing algorithms from OpenSSL are now normalized to follow Pythonâs naming conventions. For example OpenSSL uses sha3-512 instead of sha3_512 or blake2b512 instead of blake2b.
- bpo-38115: Fix a bug in dis.findlinestarts() where it would return invalid bytecode offsets. Document that a code objectâs co_lnotab can contain invalid bytecode offsets.
- bpo-38148: Add slots to `asyncio` transport classes, which can reduce
memory usage.
- bpo-38142: The _hashlib OpenSSL wrapper extension module is now PEP-384 compliant.
- bpo-9216: hashlib constructors now support usedforsecurity flag to signal that a hashing algorithm is not used in a security context.
- bpo-36991: Fixes a potential incorrect AttributeError exception escaping ZipFile.extract() in some unsupported input error situations.
- bpo-38134: Remove obsolete copy of PBKDF2_HMAC_fast. All supported OpenSSL versions contain a fast implementation.
- bpo-38132: The OpenSSL hashlib wrapper uses a simpler implementation. Several Macros and pointless caches are gone. The hash name now comes from OpenSSLâs EVP. The algorithm name stays the same, except it is now always lower case.
- bpo-38008: Fix parent class check in protocols to correctly identify the module that provides a builtin protocol, instead of assuming they all come from the `collections.abc` module
- bpo-34037: For `asyncio` , add a new coroutine`loop.shutdown_default_executor()` . The new coroutine provides an API
to schedule an executor shutdown that waits on the threadpool to finish
closing. Also,`asyncio.run()` has been updated to utilize the new
coroutine. Patch by Kyle Stanley.
- bpo-37405: Fixed regression bug for socket.getsockname() for non-CAN_ISOTP AF_CAN address family sockets by returning a 1-tuple instead of string.
- bpo-38121: Update parameter names on functions in importlib.metadata matching the changes in the 0.22 release of importlib_metadata.
- bpo-38110: The os.closewalk() implementation now uses the libc fdwalk() API on platforms where it is available.
- bpo-38093: Fixes AsyncMock so it doesnât crash when used with AsyncContextManagers or AsyncIterators.
- bpo-37488: Add warning to `datetime.utctimetuple()` ,`datetime.utcnow()` and`datetime.utcfromtimestamp()` .
- bpo-35640: Allow passing a path-like object as `directory` argument to the`http.server.SimpleHTTPRequestHandler` class. Patch
by GÃ©ry Ogam.
- bpo-38086: Update importlib.metadata with changes from importlib_metadata 0.21.
- bpo-37251: Remove `__code__` check in AsyncMock that incorrectly
evaluated function specs as async objects but failed to evaluate classes
with`__await__` but no`__code__` attribute defined as async objects.
- bpo-38037: Fix reference counters in the `signal` module.
- bpo-38066: Hide internal asyncio.Stream methods: feed_eof(), feed_data(), set_exception() and set_transport().
- bpo-38059: inspect.py now uses sys.exit() instead of exit()
- bpo-38049: Added command-line interface for the `ast` module.
- bpo-37953: In `typing` , improved the`__hash__` and`__eq__` methods for`ForwardReferences` .
- bpo-38026: Fixed `inspect.getattr_static()` used`isinstance` while
it should avoid dynamic lookup.
- bpo-35923: Update `importlib.machinery.BuiltinImporter` to use`loader._ORIGIN` instead of a hardcoded value. Patch by Donghee Na.
- bpo-38010: In `importlib.metadata` sync with`importlib_metadata` 0.20, clarifying behavior of`files()` and fixing issue where only one
requirement was returned for`requires()` on`dist-info` packages.
- bpo-38006: weakref.WeakValueDictionary defines a local remove() function used as callback for weak references. This function was created with a closure. Modify the implementation to avoid the closure.
- bpo-37995: Added the *indent* option to`ast.dump()` which allows it
