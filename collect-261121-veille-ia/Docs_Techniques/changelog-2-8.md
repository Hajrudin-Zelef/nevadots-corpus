---
id: collect-261121-veille-ia/docs-techniques/changelog-2-8
title: "Changelog"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["load", "longer", "attr", "debug", "patch", "error", "python", "interpreter"]
source: https://docs.python.org/release/3.15.1/whatsnew/changelog.html
source_anchor: ""
source_lines: [10501, 12000]
section: "Docs_Techniques"
---
- gh-105699: Python no longer crashes due an infrequent race when initializing per-interpreter interned strings. The crash would manifest when the interpreter was finalized.
- gh-105699: Python no longer crashes due to an infrequent race in setting `Py_FileSystemDefaultEncoding` and`Py_FileSystemDefaultEncodeErrors` (both deprecated), when simultaneously initializing two isolated
subinterpreters.  Now they are only set during runtime initialization.
- gh-106908: Fix various hangs, reference leaks, test failures, and tracing/introspection bugs when running with `PYTHONUOPS` or`-X uops` enabled.
- gh-106092: Fix a segmentation fault caused by a use-after-free bug in `frame_dealloc` when the trashcan delays the deallocation of a`PyFrameObject` .
- gh-106485: Reduce the number of materialized instances dictionaries by dematerializing them when possible.
- gh-106719: No longer suppress arbitrary errors in the `__annotations__` getter and setter in the type and module types.
- gh-106723: Propagate `frozen_modules` to multiprocessing spawned process
interpreters.
- gh-104909: Split `LOAD_ATTR_INSTANCE_VALUE` into micro-ops.
- gh-104909: Split `LOAD_GLOBAL` specializations into micro-ops.
- gh-106597: A new debug structure of offsets has been added to the `_PyRuntimeState` that will help out-of-process debuggers and profilers
to obtain the offsets to relevant interpreter structures in a way that is
agnostic of how Python was compiled and that doesnât require copying the
headers. Patch by Pablo Galindo
- gh-106487: Allow the *count* argument of`str.replace()` to be a
keyword. Patch by Hugo van Kemenade.
- gh-96844: Improve error message of `list.remove()` . Patch by Donghee
Na.
- gh-81283: Compiler now strips indents from docstrings. It reduces `pyc` file size 5% when the module is heavily documented. This change affects to`__doc__` so tools like doctest will be affected.
- gh-106396: When the format specification of an f-string expression is empty, the parser now generates an empty `ast.JoinedStr` node for
it instead of an one-element`ast.JoinedStr` with an empty string`ast.Constant` .
- gh-100288: Specialize `LOAD_ATTR` for non-descriptors on the
class. Adds`LOAD_ATTR_NONDESCRIPTOR_WITH_VALUES` and`LOAD_ATTR_NONDESCRIPTOR_NO_DICT` .
- gh-106008: Fix possible reference leaks when failing to optimize comparisons with `None` in the bytecode compiler.
- gh-106145: Make `end_lineno` and`end_col_offset` required on`type_param` ast nodes.
- gh-106213: Changed the way that Emscripten call trampolines work for compatibility with Wasm/JS Promise integration.
- gh-106182: `sys.getfilesystemencoding()` and`sys.getfilesystemencodeerrors` now return interned Unicode object.
- gh-106210: Removed Emscripten import trampoline as it was no longer necessary for Pyodide.
- gh-104584: Added a new, experimental, tracing optimizer and interpreter (a.k.a. âtier 2â). This currently pessimizes, so donât use yet â this is infrastructure so we can experiment with optimizing passes. To enable it, pass `-Xuops` or set`PYTHONUOPS=1` . To get debug output, set`PYTHONUOPSDEBUG=N` where`N` is a debug level (0-4, where 0 is no
debug output and 4 is excessively verbose).
- gh-105775: `LOAD_CLOSURE` is now a pseudo-op.
- gh-105730: Allow any callable other than type objects as the condition predicate in `BaseExceptionGroup.split()` and`BaseExceptionGroup.subgroup()` .
- gh-105979: Fix crash in `_imp.get_frozen_object()` due to improper
exception handling.
- gh-106003: Add a new `TO_BOOL` instruction, which performs boolean
conversions for`POP_JUMP_IF_TRUE` ,`POP_JUMP_IF_FALSE` ,
and`UNARY_NOT` (which all expect exact`bool` values now).
Also, modify the oparg of`COMPARE_OP` to include an optional
âboolean conversionâ flag.
- gh-98931: Ensure custom `SyntaxError` error messages are raised for
invalid imports with multiple targets. Patch by Pablo Galindo
- gh-105724: Improve `assert` error messages by providing exact error
range.
- gh-105908: Fixed bug where gh-99111 breaks future import `barry_as_FLUFL` in the Python REPL.
- gh-105840: Fix possible crashes when specializing function calls with too many `__defaults__` .
- gh-105831: Fix an f-string bug, where using a debug expression (the `=` sign) that appears in the last line of a file results to the debug buffer
that holds the expression text being one character too small.
- gh-105800: Correctly issue `SyntaxWarning` in f-strings if invalid
sequences are used. Patch by Pablo Galindo
- gh-105340: Include the comprehension iteration variable in `locals()` inside a module- or class-scope comprehension.
- gh-105331: Raise `ValueError` if the`delay` argument to`asyncio.sleep()` is a NaN (matching`time.sleep()` ).
- gh-105587: The runtime canât guarantee that immortal objects will not be mutated by Extensions. Thus, this modifies _PyStaticObject_CheckRefcnt to warn instead of asserting.
- gh-105564: Donât include artificil newlines in the `line` attribute of
tokens in the APIs of the`tokenize` module. Patch by Pablo Galindo
- gh-105549: Tokenize separately `NUMBER` and`NAME` tokens that are not
ambiguous. Patch by Pablo Galindo.
- gh-105588: Fix an issue that could result in crashes when compiling malformed `ast` nodes.
- gh-100987: Allow objects other than code objects as the âexecutableâ in internal frames. In the long term, this can help tools like Cython and PySpy interact more efficiently. In the shorter term, it allows us to perform some optimizations more simply.
- gh-105375: Fix bugs in the `builtins` module where exceptions could
end up being overwritten.
- gh-105375: Fix bug in the compiler where an exception could end up being overwritten.
- gh-105375: Improve error handling in `PyUnicode_BuildEncodingMap()` where an exception could end up being overwritten.
- gh-105486: Change the repr of `ParamSpec` list of args in`types.GenericAlias` .
- gh-105678: Break the `MAKE_FUNCTION` instruction into two parts,`MAKE_FUNCTION` which makes the function and`SET_FUNCTION_ATTRIBUTE` which sets the attributes on the function. This makes the stack effect of`MAKE_FUNCTION` regular to ease optimization and code generation.
- gh-105435: Fix spurious newline character if file ends on a comment without a newline. Patch by Pablo Galindo
- gh-105390: Correctly raise `tokenize.TokenError` exceptions instead
of`SyntaxError` for tokenize errors such as incomplete input. Patch
by Pablo Galindo
- gh-105259: Donât include newline character for trailing `NEWLINE` tokens
emitted in the`tokenize` module. Patch by Pablo Galindo
- gh-104635: Eliminate redundant `STORE_FAST` instructions in the
compiler. Patch by Donghee Na and Carl Meyer.
- gh-105324: Fix the main function of the `tokenize` module when
reading from`sys.stdin` . Patch by Pablo Galindo
- gh-33092: Simplify and speed up interpreter for f-strings. Removes `FORMAT_VALUE` opcode. Add`CONVERT_VALUE` ,`FORMAT_SIMPLE` and`FORMAT_WITH_SPEC` opcode. Compiler emits more efficient sequence for
each format expression.
- gh-105229: Remove remaining two-codeunit superinstructions. All remaining superinstructions only take a single codeunit, simplifying instrumentation and quickening.
- gh-105235: Prevent out-of-bounds memory access during `mmap.find()` calls.
- gh-98963: Restore the ability for a subclass of `property` to
define`__slots__` or otherwise be dict-less by ignoring failures to set
a docstring on such a class.  This behavior had regressed in 3.12beta1.
An`AttributeError` where there had not previously been one was
disruptive to existing code.
- gh-104812: The âpending callâ machinery now works for all interpreters, not just the main interpreter, and runs in all threads, not just the main thread. Some calls are still only done in the main thread, ergo in the main interpreter. This change does not affect signal handling nor the existing public C-API ( `Py_AddPendingCall()` ), which both still only
target the main thread. The new functionality is meant strictly for
internal use for now, since consequences of its use are not well
understood yet outside some very restricted cases.  This change brings the
capability in line with the intention when the state was made
per-interpreter several years ago.
- gh-105194: Do not escape with backslashes f-string format specifiers. Patch by Pablo Galindo
- gh-105229: Replace some dynamic superinstructions with single instruction equivalents.
- gh-105162: Fixed bug in generator.close()/throw() where an inner iterator would be ignored when the outer iterator was instrumented.
- gh-105164: Ensure annotations are set up correctly if the only annotation in a block is within a `match` block. Patch by Jelle Zijlstra.
- gh-105148: Make `_PyASTOptimizeState` internal to ast_opt.c. Make`_PyAST_Optimize` take two integers instead of a pointer to this struct.
This avoids the need to include pycore_compile.h in ast_opt.c.
- gh-104799: Attributes of `ast` nodes that are lists now default to
the empty list if omitted. This means that some code that previously
raised`TypeError` when the AST node was used will now proceed with
the empty list instead. Patch by Jelle Zijlstra.
- gh-105111: Remove the old trashcan macros `Py_TRASHCAN_SAFE_BEGIN` and`Py_TRASHCAN_SAFE_END` . They should be replaced by the new macros`Py_TRASHCAN_BEGIN` and`Py_TRASHCAN_END` .
- gh-105035: Fix `super()` calls on types with custom`tp_getattro` implementation (e.g. meta-types.)
- gh-105017: Show CRLF lines in the tokenize string attribute in both NL and NEWLINE tokens. Patch by Marta GÃ³mez.
- gh-105013: Fix handling of multiline parenthesized lambdas in `inspect.getsource()` . Patch by Pablo Galindo
- gh-105017: Do not include an additional final `NL` token when parsing
files having CRLF lines. Patch by Marta GÃ³mez.
- gh-104976: Ensure that trailing `DEDENT``tokenize.TokenInfo` objects emitted by the`tokenize` module are reported as in Python
3.11. Patch by Pablo Galindo
- gh-104972: Ensure that the `line` attribute in`tokenize.TokenInfo` objects in the`tokenize` module are
always correct. Patch by Pablo Galindo
- gh-104955: Fix signature for the new `__release_buffer__()` slot. Patch by Jelle Zijlstra.
- gh-104690: Starting new threads and process creation through `os.fork()` during interpreter shutdown (such as from`atexit` handlers) is no longer supported.  It can lead to race condition between
the main Python runtime thread freeing thread states while internal`threading` routines are trying to allocate and use the state of just
created threads. Or forked children trying to use the mid-shutdown runtime
and thread state in the child process.
- gh-104879: Fix crash when accessing the `__module__` attribute of type
aliases defined outside a module. Patch by Jelle Zijlstra.
- gh-104825: Tokens emitted by the `tokenize` module do not include an
implicit`\n` character in the`line` attribute anymore. Patch by
Pablo Galindo
- gh-104770: If a generator returns a value upon being closed, the value is now returned by `generator.close()` .
- gh-89091: Raise `RuntimeWarning` for unawaited async generator
methods like`asend()` ,`athrow()` and`aclose()` . Patch by Kumar Aditya.
- gh-96663: Add a better, more introspect-able error message when setting attributes on classes without a `__dict__` and no slot member for the
attribute.
- gh-93627: Update the Python pickle module implementation to match the C implementation of the pickle module. For objects setting reduction methods like `__reduce_ex__()` or`__reduce__()` to`None` , pickling will result in a`TypeError` .
- gh-101006: Improve error handling when read `marshal` data.
- gh-91095: Specializes calls to most Python classes. Specifically, any class that inherits from `object` , or another Python class, and does not
override`__new__` .The specialized instruction does the following: 
  1. Creates the object (by calling `object.__new__` )
  2. Pushes a shim frame to the frame stack (to cleanup after `__init__` )
  3. Pushes the frame for `__init__` to the frame stack
 Speeds up the instantiation of most Python classes.

### LibraryÂ¶

- gh-110786: `sysconfig` âs CLI now ignores`BrokenPipeError` ,
making it exit normally if its output is being piped and the pipe closes.
- gh-103480: The `sysconfig` module is now a package, instead of a
single-file module.
- gh-110733: Micro-optimization: Avoid calling `min()` ,`max()` in`BaseEventLoop._run_once()` .
- gh-94597: Added `asyncio.EventLoop` for use with the`asyncio.run()`*loop_factory* kwarg to avoid calling the asyncio
policy system.
- gh-110682: `runtime-checkable protocols` used to consider`__match_args__` a protocol member in`__instancecheck__` if it was present on the protocol. Now, this
attribute is ignored if it is present.
- gh-110488: Fix a couple of issues in `pathlib.PurePath.with_name()` : a
single dot was incorrectly considered a valid name, and in`PureWindowsPath` , a name with an NTFS alternate data stream, like`a:b` , was incorrectly considered invalid.
- gh-110590: Fix a bug in `_sre.compile()` where`TypeError` would
be overwritten by`OverflowError` when the*code* argument was a list
of non-ints.
- gh-65052: Prevent `pdb` from crashing when trying to display
undisplayable objects
- gh-110519: Deprecation warning about non-integer number in `gettext` now always refers to the line in the user code where gettext function or
method is used. Previously it could refer to a line in`gettext` code.
- gh-89902: Deprecate non-standard format specifier âNâ for `decimal.Decimal` . It was not documented and only supported in the
C implementation.
- gh-110378: `contextmanager()` and`asynccontextmanager()` context managers now close an
invalid underlying generator object that yields more then one value.
- gh-106670: In `pdb` , set convenience variable`$_exception` for
post mortem debugging.
- gh-110365: Fix `termios.tcsetattr()` bug that was overwriting existing
errors during parsing integers from`term` list.
- gh-109653: Slightly improve the import time of several standard-library modules by deferring imports of `warnings` within those modules.
Patch by Alex Waygood.
- gh-110273: `dataclasses.replace()` now raises TypeError instead of
ValueError if specify keyword argument for a field declared with
init=False or miss keyword argument for required InitVar field.
- gh-110249: Add `--inline-caches` flag to`dis` command line.
- gh-109653: Fix a Python 3.12 regression in the import time of `random` . Patch by Alex Waygood.
- gh-110222: Add support of struct sequence objects in `copy.replace()` .
Patched by Xuehai Pan.
- gh-109649: `multiprocessing` ,`concurrent.futures` ,`compileall` : Replace`os.cpu_count()` with`os.process_cpu_count()` to select the default number of worker
threads and processes. Get the CPU affinity if supported. Patch by Victor
Stinner.
- gh-110150: Fix base case handling in statistics.quantiles. Now allows a single data point.
- gh-110036: On Windows, multiprocessing `Popen.terminate()` now catches`PermissionError` and get the process exit code. If the process is
still running, raise again the`PermissionError` . Otherwise, the
process terminated as expected: store its exit code. Patch by Victor
Stinner.
- gh-110038: Fixed an issue that caused `KqueueSelector.select()` to not
return all the ready events in some cases when a file descriptor is
registered for both read and write.
- gh-110045: Update the `symtable` module to support the new scopes
introduced by**PEP 695** .
- gh-88402: Add new variables to `sysconfig.get_config_vars()` on
Windows:`LIBRARY` ,`LDLIBRARY` ,`LIBDIR` ,`SOABI` , and`Py_NOGIL` .
- gh-109631: `re` functions such as`re.findall()` ,`re.split()` ,`re.search()` and`re.sub()` which perform short
repeated matches can now be interrupted by user.
- gh-109653: Reduce the import time of `email.utils` by around 43%.
This results in the import time of`email.message` falling by around
18%, which in turn reduces the import time of`importlib.metadata` by
around 6%. Patch by Alex Waygood.
- gh-109818: Fix `reprlib.recursive_repr()` not copying`__type_params__` from decorated function.
- gh-109047: `concurrent.futures` : The*executor manager thread* now
catches exceptions when adding an item to the*call queue* . During Python
finalization, creating a new thread can now raise`RuntimeError` .
Catch the exception and call`terminate_broken()` in this case. Patch by
Victor Stinner.
- gh-109782: Ensure the signature of `os.path.isdir()` is identical on
all platforms. Patch by Amin Alaee.
- gh-109653: Improve import time of `functools` by around 13%. Patch by
Alex Waygood.
- gh-109590: `shutil.which()` will prefer files with an extension in`PATHEXT` if the given mode includes`os.X_OK` on win32. If no`PATHEXT` match is found, a file without an extension in`PATHEXT` can
be returned. This change will have`shutil.which()` act more similarly
to previous behavior in Python 3.11.
- gh-109653: Reduce the import time of `enum` by over 50%. Patch by
Alex Waygood.
- gh-109593: Avoid deadlocking on a reentrant call to the multiprocessing resource tracker. Such a reentrant call, though unlikely, can happen if a GC pass invokes the finalizer for a multiprocessing object such as SemLock.
- gh-109653: Reduce the import time of `typing` by around a third.
Patch by Alex Waygood.
- gh-109649: Add `os.process_cpu_count()` function to get the number of
logical CPUs usable by the calling thread of the current process. Patch by
Victor Stinner.
- gh-74481: Add `set_error_mode` related constants in`msvcrt` module in
Python debug build.
- gh-109613: Fix `os.stat()` and`os.DirEntry.stat()` : check for
exceptions. Previously, on Python built in debug mode, these functions
could trigger a fatal Python error (and abort the process) when a function
succeeded with an exception set. Patch by Victor Stinner.
- gh-109599: Expose the type of PyCapsule objects as `types.CapsuleType` .
- gh-109109: You can now get the raw TLS certificate chains from TLS connections via `ssl.SSLSocket.get_verified_chain()` and`ssl.SSLSocket.get_unverified_chain()` methods.Contributed by Mateusz Nowak.
- gh-109559: Update `unicodedata` database to Unicode 15.1.0.
- gh-109543: Remove unnecessary `hasattr()` check during`typing.TypedDict` creation.
- gh-109495: Remove unnecessary extra `__slots__` in`datetime` 's pure python implementation to reduce memory
size, as they are defined in the superclass. Patch by James Hilton-Balfe
- gh-109461: `logging` : Use a context manager for lock acquisition.
- gh-109096: `http.server.CGIHTTPRequestHandler` has been deprecated
for removal in 3.15.  Its design is old and the web world has long since
moved beyond CGI.
- gh-109409: Fix error when it was possible to inherit a frozen dataclass from multiple parents some of which were possibly not frozen.
- gh-109375: The `pdb``alias` command now prevents registering
aliases without arguments.
- gh-109319: Deprecate the `dis.HAVE_ARGUMENT` field in favour of`dis.hasarg` .
- gh-107219: Fix a race condition in `concurrent.futures` . When a process
in the process pool was terminated abruptly (while the future was running
or pending), close the connection write end. If the call queue is blocked
on sending bytes to a worker process, closing the connection write end
interrupts the send, so the queue can be closed. Patch by Victor Stinner.
- gh-66143: The `codecs.CodecInfo` object has been made copyable and
pickleable. Patched by Robert Lehmann and Furkan Onder.
- gh-109187: `pathlib.Path.resolve()` now treats symlink loops like
other errors: in strict mode,`OSError` is raised, and in non-strict
mode, no exception is raised.
- gh-50644: Attempts to pickle or create a shallow or deep copy of `codecs` streams now raise a TypeError. Previously, copying failed
with a RecursionError, while pickling produced wrong results that
eventually caused unpickling to fail with a RecursionError.
- gh-109174: Add support of `types.SimpleNamespace` in`copy.replace()` .
- gh-109164: `pdb` : Replace`getopt` with`argparse` for
parsing command line arguments.
- gh-109151: Enable `readline` editing features in the sqlite3
command-line interface (`python -m sqlite3` ).
- gh-108987: Fix `_thread.start_new_thread()` race condition. If a
thread is created during Python finalization, the newly spawned thread now
exits immediately instead of trying to access freed memory and lead to a
crash. Patch by Victor Stinner.
- gh-108682: Enum: require `names=()` or`type=...` to create an empty
enum using the functional syntax.
- gh-109033: Exceptions raised by os.utime builtin function now include the related filename
- gh-108843: Fix an issue in `ast.unparse()` when unparsing f-strings
containing many quote types.
- gh-108469: `ast.unparse()` now supports new f-string syntax
introduced in Python 3.12. Note that the f-string quotes are
reselected for simplicity under the new syntax. (Patch by Steven Sun)
- gh-108751: Add `copy.replace()` function which allows to create a
modified copy of an object. It supports named tuples, dataclasses, and
many other objects.
- gh-108682: Enum: raise `TypeError` if`super().__new__()` is called
from a custom`__new__` .
- gh-108278: Deprecate passing the callback callable by keyword for the following `sqlite3.Connection` APIs:The affected parameters will become positional-only in Python 3.15. Patch by Erlend E. Aasland.
- gh-105829: Fix concurrent.futures.ProcessPoolExecutor deadlock
- gh-108295: Fix crashes related to use of weakrefs on `typing.TypeVar` .
- gh-108463: Make expressions/statements work as expected in pdb
- gh-108277: Add `os.timerfd_create()` ,`os.timerfd_settime()` ,`os.timerfd_gettime()` ,`os.timerfd_settime_ns()` , and`os.timerfd_gettime_ns()` to provide a low level interface for Linuxâs
timer notification file descriptor.
- gh-107811: `tarfile` : extraction of members with overly large UID or
GID (e.g. on an OS with 32-bit`id_t` ) now fails in the same way
as failing to set the ID.
- gh-64662: Fix support for virtual tables in `sqlite3.Connection.iterdump()` . Patch by Aviv Palivoda.
- gh-108111: Fix a regression introduced in gh-101251 for 3.12, resulting in an incorrect offset calculation in `gzip.GzipFile.seek()` .
- gh-108294: `time.sleep()` now raises an auditing event.
- gh-108278: Deprecate passing name, number of arguments, and the callable as keyword arguments, for the following `sqlite3.Connection` APIs:The affected parameters will become positional-only in Python 3.15. Patch by Erlend E. Aasland.
- gh-108322: Speed-up NormalDist.samples() by using the inverse CDF method instead of calling random.gauss().
- gh-83417: Add the ability for venv to create a `.gitignore` file which
causes the created environment to be ignored by Git. It is on by default
when venv is called via its CLI.
- gh-105736: Harmonized the pure Python version of `OrderedDict` with the C version. Now, both versions
set up their internal state in`__new__` .  Formerly, the pure Python
version did the set up in`__init__` .
- gh-108083: Fix bugs in the constructor of `sqlite3.Connection` and`sqlite3.Connection.close()` where exceptions could be leaked. Patch
by Erlend E. Aasland.
- gh-107932: Fix `dis` module to properly report and display bytecode that
do not have source lines.
- gh-105539: `sqlite3` now emits an`ResourceWarning` if a`sqlite3.Connection` object is not`closed` explicitly. Patch by Erlend E. Aasland.
- gh-107995: The `__module__` attribute on instances of`functools.cached_property` is now set to the name of the module in
which the cached_property is defined, rather than âfunctoolsâ. This means
that doctests in`cached_property` docstrings are now properly collected
by the`doctest` module. Patch by Tyler Smart.
- gh-107963: Fix `multiprocessing.set_forkserver_preload()` to check the
given list of modules names. Patch by Donghee Na.
- gh-106242: Fixes `os.path.normpath()` to handle embedded null
characters without truncating the path.
- gh-81555: `xml.dom.minidom` now only quotes`"` in attributes.
- gh-50002: `xml.dom.minidom` now preserves whitespaces in attributes.
- gh-93057: Passing more than one positional argument to `sqlite3.connect()` and the`sqlite3.Connection` constructor is
deprecated. The remaining parameters will become keyword-only in Python
3.15. Patch by Erlend E. Aasland.
- gh-76913: Add *merge_extra* parameter/feature to`logging.LoggerAdapter`
- gh-107913: Fix possible losses of `errno` and`winerror` values in`OSError` exceptions if they were cleared or modified by the cleanup
code before creating the exception object.
- gh-107845: `tarfile.data_filter()` now takes the location of symlinks
into account when determining their target, so it will no longer reject
some valid tarballs with`LinkOutsideDestinationError` .
- gh-107812: Extend socketâs netlink support to the FreeBSD platform.
- gh-107805: Fix signatures of module-level generated functions in `turtle` .
- gh-107782: `pydoc` is now able to show signatures which are not
representable in Python, e.g. for`getattr` and`dict.pop` .
- gh-56166: Deprecate passing optional arguments *maxsplit* ,*count* and*flags* in module-level functions`re.split()` ,`re.sub()` and`re.subn()` as positional. They should only be passed by keyword.
- gh-107710: Speed up `logging.getHandlerNames()` .
- gh-107715: Fix `doctest.DocTestFinder.find()` in presence of class
names with special characters. Patch by Gertjan van Zwieten.
- gh-100814: Passing a callable object as an option value to a Tkinter image now raises the expected TclError instead of an AttributeError.
- gh-72684: Add `tkinter` widget methods:`tk_busy_hold()` ,`tk_busy_configure()` ,`tk_busy_cget()` ,`tk_busy_forget()` ,`tk_busy_current()` , and`tk_busy_status()` .
- gh-106684: Raise `ResourceWarning` when`asyncio.StreamWriter` is not closed leading to memory leaks. Patch by Kumar Aditya.
- gh-107465: Add `pathlib.Path.from_uri()` classmethod.
- gh-107077: Seems that in some conditions, OpenSSL will return `SSL_ERROR_SYSCALL` instead of`SSL_ERROR_SSL` when a certification
verification has failed, but the error parameters will still contain`ERR_LIB_SSL` and`SSL_R_CERTIFICATE_VERIFY_FAILED` . We are now
detecting this situation and raising the appropriate`ssl.SSLCertVerificationError` . Patch by Pablo Galindo
- gh-107576: Fix `types.get_original_bases()` to only return`__orig_bases__` if it is present on`cls` directly. Patch by
James Hilton-Balfe.
- gh-105481: Remove `opcode.is_pseudo` ,`opcode.MIN_PSEUDO_OPCODE` and`opcode.MAX_PSEUDO_OPCODE` , which were added in 3.12, were never
documented and were not intended to be used externally.
- gh-105481: `opcode.ENABLE_SPECIALIZATION` (which was added in 3.12
but never documented or intended for external usage) is moved to`_opcode.ENABLE_SPECIALIZATION` where tests can access it.
- gh-107396: tarfiles; Fixed use before assignment of self.exception for gzip decompression
- gh-107409: Set `__wrapped__` attribute in`reprlib.recursive_repr()` .
- gh-107406: Implement new `__repr__()` method for`struct.Struct` . Now it returns`Struct(<format repr>)` .
- gh-107369: Optimize `textwrap.indent()` . It is ~30% faster for large
input. Patch by Inada Naoki.
- gh-78722: Fix issue where `pathlib.Path.iterdir()` did not raise`OSError` until iterated.
- gh-105578: Deprecate `typing.AnyStr` in favor of the new Type
Parameter syntax. See PEP 695.
- gh-62519: Make `gettext.pgettext()` search plural definitions when
translation is not found.
- gh-107089: Shelves opened with `shelve.open()` have a much faster`clear()` method. Patch by James Cave.
- gh-82500: Fix overflow on 32-bit systems with `asyncio``os.sendfile()` implementation.
- gh-83006: Document behavior of `shutil.disk_usage()` for non-mounted
filesystems on Unix.
- gh-65495: Use lowercase `mail from` and`rcpt to` in`smptlib.SMTP` .
- gh-106186: Do not report `MultipartInvariantViolationDefect` defect when
the`email.parser.Parser` class is used to parse emails with`headersonly=True` .
- gh-105002: Fix invalid result from `PurePath.relative_to()` method
when attempting to walk a â`..` â segment in*other* with*walk_up* enabled. A`ValueError` exception is now raised in this case.
- gh-106739: Add the `rtype_cache` to the warning message (as an addition
to the type of leaked objects and the number of leaked objects already
included in the message) to make debugging leaked objects easier when the
multiprocessing resource tracker process finds leaked objects at shutdown.
This helps more quickly identify what was leaked and/or why the leaked
object was not properly cleaned up.
- gh-106751: Optimize `SelectSelector.select()` for many iteration case.
Patch By Donghee Na.
- gh-106751: Optimize `_PollLikeSelector.select()` for many iteration
case.
- gh-106751: Optimize `KqueueSelector.select()` for many iteration case.
Patch By Donghee Na.
- gh-106831: Fix potential missing `NULL` check of`d2i_SSL_SESSION` result in`_ssl.c` .
- gh-105481: The various opcode lists in the `dis` module are now
generated from bytecodes.c instead of explicitly constructed in opcode.py.
- gh-106727: Make `inspect.getsource()` smarter for class for same name
definitions
- gh-106789: Remove import of `pprint` from`sysconfig` .
- gh-105726: Added `__slots__` to`contextlib.AbstractContextManager` and`contextlib.AbstractAsyncContextManager` so that child classes can
use`__slots__` .
- gh-106774: Update the bundled copy of pip to version 23.2.1.
- gh-106751: `selectors` : Optimize`EpollSelector.select()` code by
moving some code outside of the loop.
- gh-106752: Fixed several bugs in zipfile.Path, including: in `zipfile.Path.match()` , Windows separators are no longer honored (and
never were meant to be); Fixed`name` /`suffix` /`suffixes` /`stem` operations when no filename is present and the Path is not at the root of
the zipfile; Reworked glob for performance and more correct matching
behavior.
- gh-105293: Remove call to `SSL_CTX_set_session_id_context` during client
side context creation in the`ssl` module.
- gh-106734: Disable tab completion in multiline mode of `pdb`
- gh-105481: Expose opcode metadata through `_opcode` .
- gh-106670: Add the new `exceptions` command to the Pdb debugger. It
makes it possible to move between chained exceptions when using post
mortem debugging.
- gh-106602: Add __copy__ and __deepcopy__ in `enum`
- gh-106664: `selectors` : Add`_SelectorMapping.get()` method and
optimize`_SelectorMapping.__getitem__()` .
- gh-106628: Speed up parsing of emails by about 20% by not compiling a new regular expression for every single email.
- gh-89427: Set the environment variable `VIRTUAL_ENV_PROMPT` at`venv` activation, even when`VIRTUAL_ENV_DISABLE_PROMPT` is set.
- gh-106530: Revert a change to `colorsys.rgb_to_hls()` that caused
division by zero for certain almost-white inputs.  Patch by Terry Jan
Reedy.
- gh-106584: Fix exit code for `unittest` if all tests are skipped. Patch
by Egor Eliseev.
- gh-106566: Optimize `(?!)` (pattern which always fails) in regular
expressions.
- gh-106554: `selectors` : Reduce Selector overhead by using a`dict.get()` to lookup file descriptors.
- gh-106558: Remove ref cycle in callers of `convert_to_error()` by deleting`result` from scope in a`finally` block.
- gh-100502: Add `pathlib.PurePath.pathmod` class attribute that
stores the implementation of`os.path` used for low-level path
operations: either`posixpath` or`ntpath` .
- gh-106527: Reduce overhead to add and remove `asyncio` readers and
writers.
- gh-106524: Fix crash in `_sre.template()` with templates containing
invalid group indices.
- gh-106531: Removed `_legacy` and the names it provided from`importlib.resources` :`Resource` ,`contents` ,`is_resource` ,`open_binary` ,`open_text` ,`path` ,`read_binary` , and`read_text` .
- gh-106052: `re` module: fix the matching of possessive quantifiers in
the case of a subpattern containing backtracking.
- gh-106510: Improve debug output for atomic groups in regular expressions.
- gh-106503: Fix ref cycle in `asyncio._SelectorSocketTransport` by
removing`_write_ready` in`close` .
- gh-105497: Fix flag mask inversion when unnamed flags exist.
- gh-90876: Prevent `multiprocessing.spawn` from failing to*import* in
environments where`sys.executable` is`None` .  This regressed in 3.11
with the addition of support for path-like objects in multiprocessing.
- gh-106403: Instances of `typing.TypeVar` ,`typing.ParamSpec` ,`typing.ParamSpecArgs` ,`typing.ParamSpecKwargs` , and`typing.TypeVarTuple` once
again support weak references, fixing a regression introduced in Python
3.12.0 beta 1. Patch by Jelle Zijlstra.
- gh-89812: Add private `pathlib._PathBase` class, which provides
experimental support for virtual filesystems, and may be made public in a
future version of Python.
- gh-106292: Check for an instance-dict cached value in the `__get__()` method of`functools.cached_property()` . This better matches the
pre-3.12 behavior and improves compatibility for users subclassing`functools.cached_property()` and adding a`__set__()` method.
- gh-106350: Detect possible memory allocation failure in the libtommath function `mp_init()` used by the`_tkinter` module.
- gh-106330: Fix incorrect matching of empty paths in `pathlib.PurePath.match()` . This bug was introduced in Python 3.12.0
beta 1.
- gh-106309: Deprecate `typing.no_type_check_decorator()` . No major
type checker ever added support for this decorator. Patch by Alex Waygood.
- gh-102541: Make `pydoc.doc()` catch bad module`ImportError` when
output stream is not`None` .
- gh-106263: Fix crash when calling `repr` with a manually constructed
SignalDict object. Patch by Charlie Zhao.
- gh-106236: Replace `assert` statements with`raise RuntimeError` in`threading` , so that`_DummyThread` cannot be joined even with`-OO` .
- gh-106238: Fix rare concurrency bug in lock acquisition by the logging package.
- gh-106152: Added PY_THROW event hook for `cProfile` for generators
- gh-106075: Added `asyncio.taskgroups.__all__` to`asyncio.__all__` for
export in star imports.
- gh-104527: Zipapp will now skip over appending an archive to itself.
- gh-106046: Improve the error message from `os.fspath()` if called on
an object where`__fspath__` is set to`None` . Patch by Alex Waygood.
- gh-105987: Fix crash due to improper reference counting in `asyncio` eager task factory internal routines.
- gh-105974: Fix bug where a `typing.Protocol` class that had one or
more non-callable members would raise`TypeError` when`issubclass()` was called against it, even if it defined a custom`__subclasshook__` method. The behaviour in Python 3.11 and lower â
which has now been restored â was not to raise`TypeError` in these
situations if a custom`__subclasshook__` method was defined. Patch by
Alex Waygood.
- gh-96145: Reverted addition of `json.AttrDict` .
- gh-89812: Add `pathlib.UnsupportedOperation` , which is raised instead
of`NotImplementedError` when a path operation isnât supported.
- gh-105808: Fix a regression introduced in gh-101251 for 3.12, causing `gzip.GzipFile.flush()` to not flush the compressor (nor pass along
the`zip_mode` argument).
- gh-105481: `stack_effect()` no longer raises an exception if an`oparg` is provided for an`opcode` that doesnât use its arg, or when
it is not provided for an`opcode` that does use it. In the latter case,
the stack effect is returned for`oparg=0` .
- gh-104799: Enable `ast.unparse()` to unparse function and class
definitions created without the new`type_params` field from**PEP 695** .
Patch by Jelle Zijlstra.
- gh-105793: Add *follow_symlinks* keyword-only argument to`pathlib.Path.is_dir()` and`is_file()` , defaulting
to`True` .
- gh-105570: Deprecate two methods of creating `typing.TypedDict` classes with 0 fields using the functional syntax:```
TD =
TypedDict("TD")
```
and`TD = TypedDict("TD", None)` . Both will be
disallowed in Python 3.15. To create a`TypedDict` class with 0 fields,
either use`class TD(TypedDict): pass` or`TD = TypedDict("TD", {})` .
- gh-105745: Fix `webbrowser.Konqueror.open` method.
- gh-105733: `ctypes` : Deprecate undocumented`ctypes.SetPointerType()` and`ctypes.ARRAY()` functions. Patch
by Victor Stinner.
- gh-105687: Remove deprecated `re.template` ,`re.T` ,`re.TEMPLATE` ,`sre_constans.SRE_FLAG_TEMPLATE` .
- gh-105684: Supporting `asyncio.Task.set_name()` is now mandatory for
third party task implementations. The undocumented`_set_task_name()` function (deprecated since 3.8) has been removed. Patch by Kumar Aditya.
- gh-105375: Fix a bug in `_Unpickler_SetInputStream()` where an
exception could end up being overwritten in case of failure.
- gh-105626: Change the default return value of `http.client.HTTPConnection.get_proxy_response_headers()` to be`None` and not`{}` .
- gh-105375: Fix bugs in `sys` where exceptions could end up being
overwritten because of deferred error handling.
- gh-105605: Harden `pyexpat` error handling during module
initialisation to prevent exceptions from possibly being overwritten, and
objects from being dereferenced twice.
- gh-105375: Fix bug in `decimal` where an exception could end up being
overwritten.
- gh-105375: Fix bugs in `_datetime` where exceptions could be
overwritten in case of module initialisation failure.
- gh-105375: Fix bugs in `_ssl` initialisation which could lead to
leaked references and overwritten exceptions.
- gh-105375: Fix a bug in `array.array` where an exception could end
up being overwritten.
- gh-105375: Fix bugs in `_ctypes` where exceptions could end up being
overwritten.
- gh-105375: Fix a bug in the `posix` module where an exception could
be overwritten.
- gh-105375: Fix bugs in `_elementtree` where exceptions could be
overwritten.
- gh-105375: Fix bugs in `zoneinfo` where exceptions could be
overwritten.
- gh-105375: Fix bugs in `errno` where exceptions could be overwritten.
- gh-105566: Deprecate creating a `typing.NamedTuple` class using
keyword arguments to denote the fields (```
NT = NamedTuple("NT", x=int,
y=str)
```
). This will be disallowed in Python 3.15. Use the class-based
syntax or the functional syntax instead.Two methods of creating `NamedTuple` classes with 0 fields using the
functional syntax are also deprecated, and will be disallowed in Python
3.15:`NT = NamedTuple("NT")` and`NT = NamedTuple("NT", None)` . To
create a`NamedTuple` class with 0 fields, either use```
class
NT(NamedTuple): pass
```
or`NT = NamedTuple("NT", [])` .
- gh-105545: Remove deprecated in 3.11 `webbrowser.MacOSXOSAScript._name` attribute.
- gh-105497: Fix flag inversion when alias/mask members exist.
- gh-105509: `typing.Annotated` is now implemented as an instance of`typing._SpecialForm` rather than a class. This should have no
user-facing impact for users of the`typing` module public API.
- gh-105375: Fix bugs in `pickle` where exceptions could be
overwritten.
- gh-70303: Emit `FutureWarning` from`pathlib.Path.glob()` and`rglob()` if the given pattern ends with â`**` â. In a
future Python release, patterns with this ending will match both files and
directories. Add a trailing slash to only match directories.
- gh-105375: Fix a bug in `sqlite3` where an exception could be
overwritten in the`collation` callback.
- gh-105382: Remove *cafile* ,*capath* and*cadefault* parameters of the`urllib.request.urlopen()` function, deprecated in Python 3.6. Patch
by Victor Stinner.
- gh-105376: `logging` : Remove undocumented and untested`Logger.warn()` and`LoggerAdapter.warn()` methods and`logging.warn()` function. Deprecated since Python 3.3, they were
aliases to the`logging.Logger.warning()` method,`logging.LoggerAdapter.warning()` method and`logging.warning()` function. Patch by Victor Stinner.
- gh-105332: Revert pickling method from by-name back to by-value.
- gh-104554: Add RTSPS scheme support in urllib.parse
- gh-105292: Add option to `traceback.format_exception_only()` to
recurse into the nested exception of a`BaseExceptionGroup` .
- gh-105280: Fix bug where `isinstance([], collections.abc.Mapping)` could
evaluate to`True` if garbage collection happened at the wrong time. The
bug was caused by changes to the implementation of`typing.Protocol` in Python 3.12.
- gh-80480: `array` : Add`'w'` typecode that represents`Py_UCS4` .
- gh-105239: Fix longstanding bug where ```
issubclass(object,
typing.Protocol)
```
would evaluate to`True` in some edge cases. Patch by
Alex Waygood.
- gh-104310: In the beta 1 release we added a utility function for extension module authors, to use when testing their module for support in multiple interpreters or under a per-interpreter GIL. The name of that function has changed from `allowing_all_extensions` to`_incompatible_extension_module_restrictions` .  The default for the
âdisable_checkâ argument has change from`True` to`False` , to better
match the new function name.
- gh-105080: Fixed inconsistent signature on derived classes for `inspect.signature()`
- gh-105144: Fix a recent regression in the `typing` module. The
regression meant that doing`class Foo(X, typing.Protocol)` , where`X` was a class that had`abc.ABCMeta` as its metaclass, would then
cause subsequent`isinstance(1, X)` calls to erroneously raise`TypeError` . Patch by Alex Waygood.
- gh-62948: The `io.IOBase` finalizer now logs the`close()` method
errors with`sys.unraisablehook` . Previously, errors were ignored
silently by default, and only logged in Python Development Mode or on Python built on debug mode.  Patch
by Victor Stinner.
- gh-105096: `wave` : Deprecate the`getmark()` ,`setmark()` and`getmarkers()` methods of the`wave.Wave_read` and`wave.Wave_write` classes. They will be removed in Python 3.15.
Patch by Victor Stinner.
- gh-104992: Remove the untested and undocumented `unittest.TestProgram.usageExit()` method, deprecated in Python 3.11.
Patch by Hugo van Kemenade.
- gh-104996: Improve performance of `pathlib.PurePath` initialisation
by deferring joining of paths when multiple arguments are given.
- gh-101588: Deprecate undocumented copy/deepcopy/pickle support for itertools.
- gh-103631: Fix `pathlib.PurePosixPath(pathlib.PureWindowsPath(...))` not
converting path separators to restore 3.11 compatible behavior.
- gh-104947: Make comparisons between `pathlib.PureWindowsPath` objects consistent across Windows and Posix to match 3.11 behavior.
- gh-104773: **PEP 594** : Remove the`audioop` module, deprecated in
Python 3.11. Patch by Victor Stinner.
- gh-104773: **PEP 594** : Remove the`aifc` module, deprecated in
Python 3.11. Patch by Victor Stinner.
- gh-104773: **PEP 594** : Remove the`uu` module, deprecated in Python
3.11. Patch by Victor Stinner.
- gh-104935: Fix bugs with the interaction between `typing.runtime_checkable()` and`typing.Generic` that were
introduced by the**PEP 695** implementation. Patch by Jelle Zijlstra.
- gh-104773: **PEP 594** : Remove the`crypt` module and its private`_crypt` extension, deprecated in Python 3.11. Patch by Victor
Stinner.
- gh-104773: **PEP 594** : Remove the`nis` module, deprecated in Python
3.11. Patch by Victor Stinner.
- gh-104898: Add missing `__slots__` to`os.PathLike` .
- gh-104773: **PEP 594** : Remove the`xdrlib` module, deprecated in
Python 3.11. Patch by Victor Stinner.
- gh-104773: **PEP 594** : Remove the`nntplib` module, deprecated in
Python 3.11. Patch by Victor Stinner.
- gh-104886: Remove the undocumented `configparser.LegacyInterpolation` class, deprecated in the
docstring since Python 3.2, and with a deprecation warning since Python
3.11. Patch by Hugo van Kemenade.
- gh-104786: Remove kwargs-based `typing.TypedDict` creation
- gh-104876: Remove the `turtle.RawTurtle.settiltangle()` method,
deprecated in docs since Python 3.1 and with a deprecation warning since
Python 3.11. Patch by Hugo van Kemenade.
- gh-104773: **PEP 594** : Removed the`msilib` package, deprecated in
Python 3.11.
- gh-104773: **PEP 594** : Remove the`spwd` module, deprecated in
Python 3.11: the python-pam project can be used instead. Patch by
Victor Stinner.
- gh-75552: Removed the `tkinter.tix` module, deprecated since Python 3.6.
- gh-104773: **PEP 594** : Remove the`chunk` module, deprecated in
Python 3.11. Patch by Victor Stinner.
- gh-104773: **PEP 594** : Remove the`mailcap` module, deprecated in
Python 3.11. Patch by Victor Stinner.
- gh-104773: **PEP 594** : Remove the`sunau` module, deprecated in
Python 3.11. Patch by Victor Stinner.
- gh-104780: **PEP 594** : Remove the`ossaudiodev` module, deprecated
in Python 3.11. Patch Victor Stinner.
- gh-104773: **PEP 594** : Remove the`pipes` module, deprecated in
Python 3.11. Patch by Victor Stinner.
- gh-104873: Add `typing.get_protocol_members()` to return the set of
members defining a`typing.Protocol` .  Add`typing.is_protocol()` to check whether a class is a`typing.Protocol` . Patch by Jelle Zijlstra.
- gh-104874: Document the `__name__` and`__supertype__` attributes of`typing.NewType` . Patch by Jelle Zijlstra.
- gh-104835: Removed the following `unittest` functions, deprecated in
Python 3.11:
  - `unittest.findTestCases()`
  - `unittest.makeSuite()`
  - `unittest.getTestCaseNames()`
 Use `TestLoader` methods instead:
  - `unittest.TestLoader.loadTestsFromModule()`
  - `unittest.TestLoader.loadTestsFromTestCase()`
  - `unittest.TestLoader.getTestCaseNames()`
 Patch by Hugo van Kemenade.
- gh-104804: Remove the untested and undocumented `webbrowser``MacOSX` class, deprecated in Python 3.11. Patch by Hugo van
Kemenade.
- gh-83863: Support for using `pathlib.Path` objects as context
managers has been removed. Before Python 3.9, exiting the context manager
marked a path as âclosedâ, which caused some (but not all!) methods to
raise when called. Since Python 3.9, using a path as a context manager
does nothing.
- gh-104799: Adjust the location of the (see **PEP 695** )`type_params` field on`ast.ClassDef` ,`ast.AsyncFunctionDef` , and`ast.FunctionDef` to better preserve backward compatibility. Patch
by Jelle Zijlstra
- gh-104797: Allow `typing.Protocol` classes to inherit from`collections.abc.Buffer` . Patch by Jelle Zijlstra.
- gh-104783: Remove `locale.resetlocale()` function deprecated in Python
3.11. Patch by Victor Stinner.
- gh-104780: Remove the `2to3` program and the`lib2to3` module,
deprecated in Python 3.11. Patch by Victor Stinner.
- gh-104773: **PEP 594** : Remove the`telnetlib` module, deprecated in
Python 3.11. Patch by Victor Stinner.
- gh-104773: **PEP 594** : Remove the`imghdr` module, deprecated in
Python 3.11. Patch by Victor Stinner.
- gh-104773: **PEP 594** : Remove the`cgi` and`cgitb` modules,
deprecated in Python 3.11. Patch by Victor Stinner.
- gh-104773: **PEP 594** : Remove the`sndhdr` module, deprecated in
Python 3.11. Patch by Victor Stinner.
- gh-104372: On Linux where `subprocess` can use the`vfork()` syscall for faster spawning, prevent the parent process from blocking
other threads by dropping the GIL while it waits for the vforkâed child
process`exec()` outcome. This prevents spawning a binary from a slow
filesystem from blocking the rest of the application.
- gh-99108: We now release the GIL around built-in `hashlib` computations of reasonable size for the SHA families and MD5 hash
functions, matching what our OpenSSL backed hash computations already
does.
- gh-102613: Improve performance of `pathlib.Path.glob()` when expanding
a pattern with a non-terminal â`**` â component by filtering walked paths
through a regular expression, rather than calling`os.scandir()` more
than once on each directory.
- gh-104399: Prepare the `_tkinter` module for building with Tcl 9.0 and
future libtommath by replacing usage of deprecated functions`mp_to_unsigned_bin_n()` and`mp_unsigned_bin_size()` when
necessary.
- gh-102676: Add fields `start_offset` ,`cache_offset` ,`end_offset` ,`baseopname` ,`baseopcode` ,`jump_target` and`oparg` to`dis.Instruction` .
- gh-103558: Fixed `parent` argument validation mechanism of`argparse` . Improved test coverage.
- gh-103464: Provide helpful usage messages when parsing incorrect `pdb` commands.
- gh-103384: Generalize the regex pattern `BaseConfigurator.INDEX_PATTERN` to allow spaces and non-alphanumeric characters in keys.
- gh-103124: Added multiline statement support for `pdb`
- gh-101162: Forbid using `builtins.issubclass()` with`types.GenericAlias` as the first argument.
- gh-103200: Fix cache repopulation semantics of zipimport.invalidate_caches(). The cache is now repopulated upon retrieving files with an invalid cache, not when the cache is invalidated.
- gh-100061: Fix a bug that causes wrong matches for regular expressions with possessive qualifier.
- gh-77609: Add *follow_symlinks* argument to`pathlib.Path.glob()` and`rglob()` , defaulting to false.
- gh-102541: Hide traceback in `help()` prompt, when import failed.
- gh-102120: Added a stream mode to `tarfile` that allows for reading
archives without caching info about the inner files.
- gh-102029: Deprecate passing any arguments to `threading.RLock()` .
- gh-88233: Refactored `zipfile._strip_extra` to use higher level
abstractions for extras instead of a heavy-state loop.
- gh-102024: Reduce calls of `_idle_semaphore.release()` in`concurrent.futures.thread._worker()` .
- gh-73435: Add support for recursive wildcards in `pathlib.PurePath.match()` .
- gh-84867: `unittest.TestLoader` no longer loads test cases from
exact`unittest.TestCase` and`unittest.FunctionTestCase` classes.
- gh-99203: Restore following CPython <= 3.10.5 behavior of `shutil.make_archive()` : do not create an empty archive if`root_dir` is not a directory, and, in that case, raise`FileNotFoundError` or`NotADirectoryError` regardless of`format` choice. Beyond the brought-back behavior, the function may now
also raise these exceptions in`dry_run` mode.
- gh-80480: Emit `DeprecationWarning` for`array` âs`'u'` type
code, deprecated in docs since Python 3.3.
- gh-94924: `unittest.mock.create_autospec()` now properly returns
coroutine functions compatible with`inspect.iscoroutinefunction()`
- gh-94777: Fix hanging `multiprocessing``ProcessPoolExecutor` when
a child process crashes while data is being written in the call queue.
- gh-92871: Remove the `typing.io` and`typing.re` namespaces,
deprecated since Python 3.8. All items are still available from the main`typing` module.
- bpo-43633: Improve the textual representation of IPv4-mapped IPv6 addresses ( **RFC 4291** Sections 2.2, 2.5.5.2) in`ipaddress` . Patch
by Oleksandr Pavliuk.
- bpo-44850: Improve performance of `operator.methodcaller()` using the**PEP 590**`vectorcall` convention. Patch by Anthony Lee and Pieter
Eendebak.
- bpo-44185: `unittest.mock.mock_open()` will call the`close()` method of the file handle mock when it is exiting from the context
manager. Patch by Samet Yaslan.
- bpo-40988: Improve performance of `functools.singledispatchmethod` by caching the generated dispatch wrapper. Optimization suggested by
frederico. Patch by @mental32, Alex Waygood and Pieter Eendebak.
- bpo-41768: `unittest.mock` speccing no longer calls class properties.
Patch by Melanie Witt.
- bpo-18319: Ensure `gettext(msg)` retrieve translations even if a plural
form exists. In other words:`gettext(msg) == ngettext(msg, '', 1)` .
- bpo-17013: Add `ThreadingMock` to`unittest.mock` that can be used
to create Mock objects that can wait until they are called. Patch by
Karthikeyan Singaravelan and Mario Corchero.

### DocumentationÂ¶

- gh-109209: The minimum Sphinx version required for the documentation is now 4.2.
- gh-108826: `dis` module command-line interface is now mentioned in
documentation.
- gh-107305: Add documentation for `PyInterpreterConfig` and`Py_NewInterpreterFromConfig()` .  Also clarify some of the nearby
docs relative to per-interpreter GIL.
- gh-107008: Document the `curses` module variables`LINES` and`COLS` .
- gh-106948: Add a number of standard external names to `nitpick_ignore` .
- gh-106232: Make timeit doc command lines compatible with Windows by using double quotes for arguments. This works on linux and macOS also.
- gh-105172: Fixed `functools.lru_cache()` docstring accounting for`typed` argumentâs different handling of str and int. Patch by Bar
Harel.
- gh-105052: Update `timeit` doc to specify that time in seconds is just
the default.
- gh-89455: Add missing documentation for the `max_group_depth` and`max_group_width` parameters and the`exceptions` attribute of the`traceback.TracebackException` class.
- gh-89412: Add missing documentation for the `end_lineno` and`end_offset` attributes of the`traceback.TracebackException` class.
- gh-104943: Remove mentions of old Python versions in `typing.NamedTuple` .
- gh-54738: Add documentation on how to localize the `argparse` module.
- gh-102823: Document the return type of `x // y` when`x` and`y` have type`float` .
- gh-102759: Align function signature for `functools.reduce` in
documentation and docstring with the C implementation.

### TestsÂ¶

- gh-110647: Fix test_stress_modifying_handlers() of test_signal. Patch by Victor Stinner.
- gh-103053: Fix test_tools.test_freeze on FreeBSD: run âmake distcleanâ instead of âmake cleanâ in the copied source directory to remove also the âpythonâ program. Patch by Victor Stinner.
- gh-110167: Fix a deadlock in test_socket when server fails with a timeout but the client is still running in its thread. Donât hold a lock to call cleanup functions in doCleanups(). One of the cleanup function waits until the client completes, whereas the client could deadlock if it called addCleanup() in such situation. Patch by Victor Stinner.
- gh-110388: Add tests for `tty` .
- gh-81002: Add tests for `termios` .
- gh-110367: regrtest: When using worker processes (-jN) with âverbose3 option, regrtest can now display the worker output even if a worker process does crash. Previously, sys.stdout and sys.stderr were replaced and so the worker output was lost on a crash. Patch by Victor Stinner.
- gh-110267: Add tests for pickling and copying PyStructSequence objects. Patched by Xuehai Pan.
- gh-110171: `libregrtest` now always sets and shows`random.seed` , so
tests are more reproducible. Use`--randseed` flag to pass the explicit
random seed for tests.
- gh-110152: Remove `Tools/scripts/run_tests.py` and```
make
hostrunnertest
```
. Just run`./python -m test --slow-ci` ,```
make
buildbottest
```
or`make test` instead. Python test runner (regrtest) now
handles cross-compilation and HOSTRUNNER. It also adds options to Python
such fast`-u -E -W default -bb` when`--fast-ci` or`--slow-ci` option is used. Patch by Victor Stinner.
- gh-110031: Skip test_threading tests using thread+fork if Python is built with Address Sanitizer (ASAN). Patch by Victor Stinner.
- gh-110088: Fix test_asyncio timeouts: donât measure the maximum duration, a test should not measure a CI performance. Only measure the minimum duration when a task has a timeout or delay. Add `CLOCK_RES` to`test_asyncio.utils` . Patch by Victor Stinner.
- gh-109974: Fix race conditions in test_threading lock tests. Wait until a condition is met rather than using `time.sleep()` with a hardcoded
number of seconds. Patch by Victor Stinner.
- gh-110033: Fix `test_interprocess_signal()` of`test_signal` . Make
sure that the`subprocess.Popen` object is deleted before the test
raising an exception in a signal handler. Otherwise,`Popen.__del__()` can get the exception which is logged as`Exception ignored in: ...` and
the test fails. Patch by Victor Stinner.
- gh-109594: Fix test_timeout() of test_concurrent_futures.test_wait. Remove the future which may or may not complete depending if it takes longer than the timeout or not. Keep the second future which does not complete before wait() timeout. Patch by Victor Stinner.
- gh-109972: Split test_gdb.py file into a test_gdb package made of multiple tests, so tests can now be run in parallel. Patch by Victor Stinner.
- gh-109566: regrtest: When `--fast-ci` or`--slow-ci` option is used,
regrtest now replaces the current process with a new process to add```
-u
-W default -bb -E
```
options to Python. Patch by Victor Stinner.
- gh-109748: Fix `test_zippath_from_non_installed_posix()` of test_venv:
donât copy`__pycache__/` sub-directories, because they can be modified
by other Python tests running in parallel. Patch by Victor Stinner.
- gh-109739: regrtest: Fix reference leak check on Windows. Disable the load tracker on Windows in the reference leak check mode (-R option). Patch by Victor Stinner.
- gh-109276: regrtest: When a test fails with âenv changedâ and the ârerun option is used, the test is now re-run in verbose mode in a fresh process. Patch by Victor Stinner.
- gh-103053: Skip test_freeze_simple_script() of test_tools.test_freeze if Python is built with `./configure --enable-optimizations` , which means
with Profile Guided Optimization (PGO): it just makes the test too slow.
The freeze tool is tested by many other CIs with other (faster) compiler
flags. Patch by Victor Stinner.
- gh-109580: Skip `test_perf_profiler` if Python is built with ASAN, MSAN
or UBSAN sanitizer. Python does crash randomly in this test on such build.
Patch by Victor Stinner.
- gh-109566: regrtest: Add `--fast-ci` and`--slow-ci` options.`--fast-ci` uses a default timeout of 10 minutes and`-u all,-cpu` (skip slowest tests).`--slow-ci` uses a default timeout of 20 minutes
and`-u all` (run all tests). Patch by Victor Stinner.
- gh-109425: libregrtest now decodes stdout of test worker processes with the âbackslashreplaceâ error handler to log corrupted stdout, instead of failing with an error and not logging the stdout. Patch by Victor Stinner.
- gh-109396: Fix `test_socket.test_hmac_sha1()` in FIPS mode. Use a longer
key: FIPS mode requires at least of at least 112 bits. The previous key
was only 32 bits. Patch by Victor Stinner.
- gh-104736: Fix test_gdb on Python built with LLVM clang 16 on Linux ppc64le (ex: Fedora 38). Search patterns in gdb âbtâ command output to detect when gdb fails to retrieve the traceback. For example, skip a test if `Backtrace stopped: frame did not save the PC` is found. Patch by
Victor Stinner.
- gh-109276: libregrtest now calls `random.seed()` before running each
test file when`-r/--randomize` command line option is used. Moreover,
itâs also called in worker processes.  It should help to make tests more
deterministic. Previously, it was only called once in the main process
before running all test files and it was not called in worker processes.
Patch by Victor Stinner.
- gh-109276: libregrtest now uses a separated file descriptor to write test result as JSON. Previously, if a test wrote debug messages late around the JSON, the main test process failed to parse JSON. Patch by Victor Stinner.
- gh-108996: Fix and enable `test_msvcrt` .
- gh-109237: Fix `test_site.test_underpth_basic()` when the working
directory contains at least one non-ASCII character: encode the`._pth` file to UTF-8 and enable the UTF-8 Mode to use UTF-8 for the child process
stdout. Patch by Victor Stinner.
- gh-109230: Fix `test_pyexpat.test_exception()` : it can now be run from a
directory different than Python source code directory. Before, the test
failed in this case.  Skip the test if Modules/pyexpat.c source is not
available. Skip also the test on Python implementations other than
CPython. Patch by Victor Stinner.
- gh-108996: Add tests for `msvcrt` .
- gh-109015: Fix test_asyncio, test_imaplib and test_socket tests on FreeBSD if the TCP blackhole is enabled ( `sysctl net.inet.tcp.blackhole` ). Skip
the few tests which failed with`ETIMEDOUT` which such non standard
configuration. Currently, the FreeBSD GCP image enables TCP and UDP
blackhole (```
sysctl
net.inet.tcp.blackhole=2
```
and`sysctl net.inet.udp.blackhole=1` ).
Patch by Victor Stinner.
- gh-91960: Skip `test_gdb` if gdb is unable to retrieve Python frame
objects: if a frame is`<optimized out>` . When Python is built with
âclang -Ogâ, gdb can fail to retrieve the*frame* parameter of`_PyEval_EvalFrameDefault()` . In this case, tests like`py_bt()` are
likely to fail. Without getting access to Python frames,`python-gdb.py` is mostly clueless on retrieving the Python traceback. Moreover,`test_gdb` is no longer skipped on macOS if Python is built with Clang.
Patch by Victor Stinner.
- gh-108962: Skip `test_tempfile.test_flags()` if`chflags()` fails with
âOSError: [Errno 45] Operation not supportedâ (ex: on FreeBSD 13). Patch
by Victor Stinner.
- gh-91960: FreeBSD 13.2 CI coverage for pull requests is now provided by Cirrus-CI (a hosted CI service that supports Linux, macOS, Windows, and FreeBSD).
- gh-89392: Removed support of `test_main()` function in tests. They now
always use normal unittest test runner.
- gh-108851: Fix `test_tomllib` recursion tests for WASI buildbots: reduce
the recursion limit and compute the maximum nested array/dict depending on
the current available recursion limit. Patch by Victor Stinner.
- gh-108851: Add `get_recursion_available()` and`get_recursion_depth()` functions to the`test.support` module. Patch by Victor Stinner.
- gh-108834: Add `--fail-rerun option` option to regrtest: if a test
failed when then passed when rerun in verbose mode, exit the process with
exit code 2 (error), instead of exit code 0 (success). Patch by Victor
Stinner.
- gh-108834: Rename regrtest `--verbose2` option (`-w` ) to`--rerun` .
Keep`--verbose2` as a deprecated alias. Patch by Victor Stinner.
- gh-108834: When regrtest reruns failed tests in verbose mode ( ```
./python
-m test --rerun
```
), tests are now rerun in fresh worker processes rather
than being executed in the main process. If a test does crash or is killed
by a timeout, the main process can detect and handle the killed worker
process. Tests are rerun in parallel if the`-jN` option is used to run
tests in parallel. Patch by Victor Stinner.
- gh-108822: `regrtest` now computes statistics on all tests: successes,
failures and skipped.`test_netrc` ,`test_pep646_syntax` and`test_xml_etree` now return results in their`test_main()` function.
Patch by Victor Stinner and Alex Waygood.
- gh-108794: The `doctest.DocTestRunner.run()` method now counts the
number of skipped tests. Add`doctest.DocTestRunner.skips` and`doctest.TestResults.skipped` attributes. Patch by Victor Stinner.
- gh-108388: Convert test_concurrent_futures to a package of 7 sub-tests. Patch by Victor Stinner.
- gh-108388: Split test_multiprocessing_fork, test_multiprocessing_forkserver and test_multiprocessing_spawn into test packages. Each package is made of 4 sub-tests: processes, threads, manager and misc. It allows running more tests in parallel and so reduce the total test duration. Patch by Victor Stinner.
- gh-105776: Fix test_cppext when the C compiler command `-std=c11` option: remove`-std=` options from the compiler command. Patch by
Victor Stinner.
- gh-107652: Set up CIFuzz to run fuzz targets in GitHub Actions. Patch by Illia Volochii.
- gh-107237: `test_logging` : Fix`test_udp_reconnection()` by increasing
the timeout from 100 ms to 5 minutes (LONG_TIMEOUT). Patch by Victor
Stinner.
- gh-107178: Add the C API test for functions in the Mapping Protocol, the Sequence Protocol and some functions in the Object Protocol.
- gh-106714: test_capi: Fix test_no_FatalError_infinite_loop() to no longer write a coredump, by using test.support.SuppressCrashReport. Patch by Victor Stinner.
- gh-104090: Avoid creating a reference to the test object in `collectedDurations()` .
- gh-106752: Moved tests for `zipfile.Path` into`Lib/test/test_zipfile/_path` . Made`zipfile._path` a package.
- gh-106690: Add .coveragerc to cpython repository for use with coverage package.
- gh-101634: When running the Python test suite with `-jN` option, if a
worker stdout cannot be decoded from the locale encoding report a failed
testn so the exitcode is non-zero. Patch by Victor Stinner.
- gh-105084: When the Python build is configured `--with-wheel-pkg-dir` ,
tests requiring the`setuptools` and`wheel` wheels will search for
the wheels in`WHEEL_PKG_DIR` .
- gh-81005: String tests are modified to reflect that `str` and`unicode` are merged in Python 3. Patch by Daniel Fortunov.
- gh-103186: Suppress and assert expected RuntimeWarnings in test_sys_settrace.py
- gh-69714: Add additional tests to `calendar` to achieve full test
coverage.

### BuildÂ¶

- gh-103053: âmake check-clean-srcâ now also checks if the âpythonâ program is found in the source directory: fail with an error if it does exist. Patch by Victor Stinner.
- gh-109191: Fix compile error when building with recent versions of libedit.
- gh-110276: No longer ignore `PROFILE_TASK` failure silently:
command used by Profile Guided Optimization (PGO). Patch by Victor
Stinner.
- gh-109566: Remove `make testall` target: use`make buildbottest` instead. Patch by Victor Stinner.
- gh-109740: The experimental `--disable-gil` configure flag now includes
âtâ (for âthreadedâ) in extension ABI tags.
- gh-109054: Fix building the `_testcapi` extension on Linux AArch64 which
requires linking to libatomic when`<cpython/pyatomic.h>` is used: the`_Py_atomic_or_uint64()` function requires libatomic`__atomic_fetch_or_8()` on this platform. The configure script now
checks if linking to libatomic is needed and generates a new LIBATOMIC
variable used to build the _testcapi extension. Patch by Victor Stinner.
- gh-63760: Fix Solaris build: no longer redefine the `gethostname()` function. Solaris defines the function since 2005. Patch by Victor
Stinner, original patch by Jakub KulÃk.
- gh-108740: Fix a race condition in `make regen-all` . The`deepfreeze.c` source and files generated by Argument Clinic are now
generated or updated before generating âglobal objectsâ. Previously, some
identifiers may miss depending on the order in which these files were
generated. Patch by Victor Stinner.
- gh-108634: Python built with `configure``--with-trace-refs` (tracing references) is now ABI compatible with Python release build and
debug build. Patch by Victor Stinner.
- gh-85283: The `_stat` C extension is now built with the limited C
API. Patch by Victor Stinner.
- gh-108447: Fix x86_64 GNU/Hurd build
- gh-107814: When calling `find_python.bat` with`-q` it did not
properly silence the output of nuget. That is now fixed.
- gh-105481: Remove the make target `regen-opcode-targets` , merge its work
into`regen-opcode` which repeats most of the calculation. This
simplifies the code for the build and reduces code duplication.
- gh-106881: Check for `linux/limits.h` before including it in`Modules/posixmodule.c` .
- gh-95855: Refactor platform triplet detection code and add detection for MIPS soft float and musl libc.
- gh-106962: Detect MPI compilers in `configure` .
- gh-106118: Fix compilation for platforms without `O_CLOEXEC` . The
issue was introduced with Python 3.12b1 in gh-103295. Patch by Erlend
Aasland.
- gh-105875: SQLite 3.15.2 or newer is required to build the `sqlite3` extension module. Patch by Erlend Aasland.
- gh-90005: Fix a regression in `configure` where we could end up
unintentionally linking with`libbsd` .
- gh-102404: Document how to perform a WASI build on Linux. Also add Tools/wasm/build_wasi.sh as a reference implementation of the docs.
- gh-89886: Autoconf 2.71 and aclocal 1.16.4 is now required to regenerate `configure` .
- gh-104692: Include `commoninstall` as a prerequisite for`bininstall`This ensures that `commoninstall` is completed before`bininstall` is
started when parallel builds are used (`make -j install` ), and so the`python3` symlink is only installed after all standard library modules
are installed.
- gh-101538: Add experimental wasi-threads support. Patch by Takashi Yamamoto.

### WindowsÂ¶

- gh-110437: Allows overriding the source of VC redistributables so that releases can be guaranteed to never downgrade between updates.
- gh-109286: Update Windows installer to use SQLite 3.43.1.
- gh-82367: `os.path.realpath()` now resolves MS-DOS style file names
even if the file is not accessible. Patch by Moonsik Park.
- gh-109991: Update Windows build to use OpenSSL 3.0.11.
- gh-106242: Fixes `realpath()` to behave consistently when
passed a path containing an embedded null character on Windows. In strict
mode, it now raises`OSError` instead of the unexpected`ValueError` , and in non-strict mode will make the path absolute.
- gh-83180: Changes the Python install manager to prefer an active virtual environment when the launched script has a shebang line using a Unix-like virtual command, even if the command requests a specific version of Python.
- gh-106844: Fix integer overflow and truncating by the null character in `_winapi.LCMapStringEx()` which affects`ntpath.normcase()` .
- gh-105436: Ensure that an empty environment block is terminated by two null characters, as is required by Windows.
- gh-105146: Updated the links at the end of the installer to point to Discourse rather than the mailing lists.
- gh-103646: When installed from the Microsoft Store, `pip` no longer
defaults to per-user installs. However, as the install directory is
unwritable, it should automatically decide to do a per-user install
anyway. This should resolve issues when`pip` is passed an option that
conflicts with`--user` .
- gh-88745: Improve performance of `shutil.copy2()` by using the
operating systemâs`CopyFile2` function. This may result in subtle
changes to metadata copied along with some files, bringing them in line
with normal OS behavior.
- gh-104820: Fixes `stat()` and related functions on file systems
that do not support file ID requests. This includes FAT32 and exFAT.
- gh-104803: Add `os.path.isdevdrive()` to detect whether a path is on a
Windows Dev Drive. Returns`False` on platforms that do not support Dev
Drive, and is absent on non-Windows platforms.

### macOSÂ¶

### IDLEÂ¶

- gh-104719: Remove IDLEâs modification of tokenize.tabsize and test other uses of tokenize data and methods.

### Tools/DemosÂ¶

- gh-109991: Update GitHub CI workflows to use OpenSSL 3.0.11 and multissltests to use 1.1.1w, 3.0.11, and 3.1.3.
- gh-108494: Argument Clinic now has a partial support of the Limited API: see documentation in the Python Developerâs Guide Patch by Victor Stinner.
- gh-107704: It is now possible to deprecate passing keyword arguments for keyword-or-positional parameters with Argument Clinic, using the new ```
/
[from X.Y]
```
syntax. (To be read as*âpositional-only from Python version
X.Yâ* .) See documentation in the Python Developerâs Guide
for more information.
- gh-107880: Argument Clinic can now clone `__init__()` and`__new__()` methods.
- gh-104683: Add `--exclude` option to Argument Clinic CLI.
- gh-95065: Argument Clinic now supports overriding automatically generated signature by using directive `@text_signature` . See documentation in
the Python Developerâs Guide
- gh-107609: Fix duplicate module check in Argument Clinic. Previously, a duplicate definition would incorrectly be silently accepted. Patch by Erlend E. Aasland.
- gh-107467: The Argument Clinic command-line tool now prints to stderr instead of stdout on failure.
- gh-106970: Fix bugs in the Argument Clinic `destination <name> clear` command; the destination buffers would never be cleared, and the`destination` directive parser would simply continue to the fault
handler after processing the command. Patch by Erlend E. Aasland.
- gh-106706: Change bytecode syntax for families to remove redundant name matching pseudo syntax.
- gh-106359: Argument Clinic now explicitly forbids âkwarg splatsâ in function calls used as annotations.
- gh-103186: `freeze` now fetches`CONFIG_ARGS` from the original
CPython instance the Makefile uses to call utility scripts. Patch by
Ijtaba Hussain.
- gh-95065: It is now possible to deprecate passing parameters positionally with Argument Clinic, using the new `* [from X.Y]` syntax. (To be read
as*âkeyword-only from Python version X.Yâ* .) See documentation in the
Python Developerâs Guide
for more information. Patch by Erlend E. Aasland with help from Alex
Waygood, Nikita Sobolev, and Serhiy Storchaka.

### C APIÂ¶

- gh-85283: If the `Py_LIMITED_API` macro is defined,`Py_BUILD_CORE` ,`Py_BUILD_CORE_BUILTIN` and`Py_BUILD_CORE_MODULE` macros are now undefined by`<Python.h>` . Patch by Victor Stinner.
- gh-110289: Add `PyUnicode_EqualToUTF8AndSize()` and`PyUnicode_EqualToUTF8()` functions.
- gh-110235: Raise `TypeError` for duplicate/unknown fields in`PyStructSequence` constructor. Patched by Xuehai Pan.
- gh-110014: Remove undocumented `PY_TIMEOUT_MAX` constant from the
limited C API. Patch by Victor Stinner.
- gh-109521: `PyImport_GetImporter()` now sets RuntimeError if it
fails to get`sys.path_hooks` or`sys.path_importer_cache` or
they are not list and dict correspondingly. Previously it could return
NULL without setting error in obscure cases, crash or raise SystemError if
these attributes have wrong type.
- gh-108724: Add `PyMutex` internal-only lightweight locking API.
- gh-85283: Add `PySys_AuditTuple()` function: similar to`PySys_Audit()` , but pass event arguments as a Python`tuple` object.  Patch by Victor Stinner.
- gh-108867: Add `PyThreadState_GetUnchecked()` function: similar to`PyThreadState_Get()` , but donât kill the process with a fatal
error if it is NULL. The caller is responsible to check if the result is
NULL. Previously, the function was private and known as`_PyThreadState_UncheckedGet()` . Patch by Victor Stinner.
- gh-108765: `Python.h` no longer includes the`<ctype.h>` standard
header file. If needed, it should now be included explicitly. For example,
it provides`isalpha()` and`tolower()` functions which are locale
dependent. Python provides locale independent functions, like`Py_ISALPHA()` and`Py_TOLOWER()` . Patch by Victor Stinner.
- gh-108765: `Python.h` no longer includes the`<unistd.h>` standard
header file. If needed, it should now be included explicitly. For example,
it provides the functions:`close()` ,`getpagesize()` ,`getpid()` and`sysconf()` . Patch by Victor Stinner.
- gh-108765: `Python.h` no longer includes the`<ieeefp.h>` standard
header. It was included for the`finite()` function which is now
provided by the`<math.h>` header. It should now be included explicitly
if needed. Remove also the`HAVE_IEEEFP_H` macro. Patch by Victor
Stinner.
- gh-108765: `Python.h` no longer includes these standard header files:`<time.h>` ,`<sys/select.h>` and`<sys/time.h>` . If needed, they
should now be included explicitly. For example,`<time.h>` provides the`clock()` and`gmtime()` functions,`<sys/select.h>` provides the`select()` function, and`<sys/time.h>` provides the`futimes()` ,`gettimeofday()` and`setitimer()` functions. Patch by Victor Stinner.
- gh-108511: Add functions `PyObject_HasAttrWithError()` ,`PyObject_HasAttrStringWithError()` ,`PyMapping_HasKeyWithError()` and`PyMapping_HasKeyStringWithError()` .
- gh-107073: Add `PyObject_VisitManagedDict()` and`PyObject_ClearManagedDict()` functions which must be called by the
traverse and clear functions of a type using`Py_TPFLAGS_MANAGED_DICT` flag. Patch by Victor Stinner.
- gh-108634: Python built with `configure``--with-trace-refs` (tracing references) now supports the Limited API.
Patch by Victor Stinner.
- gh-108014: Add `PyLong_AsInt()` function: similar to`PyLong_AsLong()` , but store the result in a C int instead
of a C long. Previously, it was known as the private function`_PyLong_AsInt()` (with an underscore prefix). Patch by Victor
Stinner.
- gh-108314: Add `PyDict_ContainsString()` function: same as`PyDict_Contains()` , but*key* is specified as a const char* UTF-8 encoded bytes string, rather than a PyObject*.
Patch by Victor Stinner.
- gh-108337: Add atomic operations on additional data types in pyatomic.h.
- gh-108014: Add `Py_IsFinalizing()` function: check if the main
Python interpreter is shutting down. Patch
by Victor Stinner.
- gh-107916: C API functions `PyErr_SetFromErrnoWithFilename()` ,`PyErr_SetExcFromWindowsErrWithFilename()` and`PyErr_SetFromWindowsErrWithFilename()` save now the error code
before calling`PyUnicode_DecodeFSDefault()` .
- gh-107915: Such C API functions as `PyErr_SetString()` ,`PyErr_Format()` ,`PyErr_SetFromErrnoWithFilename()` and many others
no longer crash or ignore errors if it failed to format the error message
or decode the filename. Instead, they keep a corresponding error.
- gh-107810: Improve `DeprecationWarning` for uses of`PyType_Spec` with metaclasses that have custom`tp_new` .
- gh-107249: Implement the `Py_UNUSED` macro for Windows MSVC
compiler. Patch by Victor Stinner.
- gh-107226: `PyModule_AddObjectRef()` is now only available in the
limited API version 3.10 or later.
- gh-106320: Remove private `_PyUnicode_AsString()` alias to`PyUnicode_AsUTF8()` . It was kept for backward compatibility with
Python 3.0 - 3.2. The`PyUnicode_AsUTF8()` is available since Python
3.3. The`PyUnicode_AsUTF8String()` function can be used to keep
compatibility with Python 3.2 and older. Patch by Victor Stinner.
- gh-106572: Convert `PyObject_DelAttr()` and`PyObject_DelAttrString()` macros to functions. Patch by Victor
Stinner.
- gh-106307: Add `PyMapping_GetOptionalItem()` function.
- gh-106521: Add `PyObject_GetOptionalAttr()` and`PyObject_GetOptionalAttrString()` functions.
- gh-106320: Remove `_PyInterpreterState_Get()` alias to`PyInterpreterState_Get()` which was kept for backward
compatibility with Python 3.8. Patch by Victor Stinner.
- gh-106316: Remove `cpython/pytime.h` header file: it only contained
private functions. Patch by Victor Stinner.
- gh-106023: Remove private `_PyObject_FastCall()` function: use`PyObject_Vectorcall()` which is available since Python 3.8
(**PEP 590** ). Patch by Victor Stinner.
- gh-106168: If Python is built in debug mode or `with assertions` ,`PyTuple_SET_ITEM()` and`PyList_SET_ITEM()` now check the index argument with an
assertion. If the assertion fails, make sure that the size is set before.
Patch by Victor Stinner.
- gh-106084: Remove the old aliases to functions calling functions which were kept for backward compatibility with Python 3.8 provisional API: 
  - `_PyObject_CallMethodNoArgs()` : use`PyObject_CallMethodNoArgs()`
  - `_PyObject_CallMethodOneArg()` : use`PyObject_CallMethodOneArg()`
  - `_PyObject_CallOneArg()` : use`PyObject_CallOneArg()`
  - `_PyObject_FastCallDict()` : use`PyObject_VectorcallDict()`
  - `_PyObject_Vectorcall()` : use`PyObject_Vectorcall()`
  - `_PyObject_VectorcallMethod()` : use`PyObject_VectorcallMethod()`
  - `_PyVectorcall_Function()` : use`PyVectorcall_Function()`
 Just remove the underscore prefix to update your code. Patch by Victor Stinner.
- gh-106004: Adds `PyDict_GetItemRef()` and`PyDict_GetItemStringRef()` functions: similar to`PyDict_GetItemWithError()` but returning a strong reference
instead of a borrowed reference. Patch by Victor Stinner.
- gh-105927: Deprecate the `PyWeakref_GetObject()` and`PyWeakref_GET_OBJECT()` functions: use the new`PyWeakref_GetRef()` function instead. Patch by Victor Stinner.
- gh-105927: Add `PyWeakref_GetRef()` function: similar to`PyWeakref_GetObject()` but returns a strong reference, or`NULL` if the referent is no longer live. Patch by Victor Stinner.
- gh-105922: Add `PyImport_AddModuleRef()` : similar to`PyImport_AddModule()` , but return a strong reference
instead of a borrowed reference. Patch by Victor Stinner.
- gh-105227: The new `PyType_GetDict()` provides the dictionary for
the given type object that is normally exposed by`cls.__dict__` .
Normally itâs sufficient to use`tp_dict` , but for
the static builtin types`tp_dict` is now always`NULL` .`PyType_GetDict()` provides the correct dict object instead.
- gh-105375: Fix a bug in `PyErr_WarnExplicit()` where an exception
could end up being overwritten if the API failed internally.
- gh-105603: Weâve renamed the new (in 3.12) `PyInterpreterConfig.own_gil` to`PyInterpreterConfig.gil` and changed the meaning of the value from
âboolâ to an integer with supported values of`PyInterpreterConfig_DEFAULT_GIL` ,`PyInterpreterConfig_SHARED_GIL` ,
and`PyInterpreterConfig_OWN_GIL` . The default is âsharedâ.
- gh-105387: In the limited C API version 3.12, `Py_INCREF()` and`Py_DECREF()` functions are now implemented as opaque function calls
to hide implementation details. Patch by Victor Stinner.
- gh-105396: Deprecate the `PyImport_ImportModuleNoBlock()` function
which is just an alias to`PyImport_ImportModule()` since Python
3.3. Patch by Victor Stinner.
- gh-103968: `PyType_FromMetaclass()` now allows metaclasses with`tp_new` set to`NULL` .
- gh-105268: Remove the old private, undocumented and untested `_PyGC_FINALIZED()` macro which was kept for backward compatibility with
Python 3.8 and older. Patch by Victor Stinner.
- gh-105182: Remove `PyEval_AcquireLock()` and`PyEval_ReleaseLock()` functions, deprecated in Python 3.2. Patch by Victor Stinner.
- gh-105182: Remove `PyEval_InitThreads()` and`PyEval_ThreadsInitialized()` functions, deprecated in Python 3.9. Patch
by Victor Stinner.
- gh-105145: Deprecate old Python initialization functions: 
  - `PySys_ResetWarnOptions()`
  - `Py_GetExecPrefix()`
  - `Py_GetPath()`
  - `Py_GetPrefix()`
  - `Py_GetProgramFullPath()`
  - `Py_GetProgramName()`
  - `Py_GetPythonHome()`
 Patch by Victor Stinner.
- gh-85275: `PyObject_AsCharBuffer()` ,`PyObject_AsReadBuffer()` ,`PyObject_CheckReadBuffer()` , and`PyObject_AsWriteBuffer()` are
removed. Please migrate to new buffer protocol;`PyObject_GetBuffer()` and`PyBuffer_Release()` .
- gh-105156: Deprecate the old `Py_UNICODE` and`PY_UNICODE_TYPE` types:
use directly the`wchar_t` type instead. Since Python 3.3,`Py_UNICODE` and`PY_UNICODE_TYPE` are just aliases to`wchar_t` . Patch by Victor Stinner.
- gh-105145: Remove the following old functions to configure the Python initialization, deprecated in Python 3.11: 
  - `PySys_AddWarnOptionUnicode()`
  - `PySys_AddWarnOption()`
  - `PySys_AddXOption()`
  - `PySys_HasWarnOptions()`
  - `PySys_SetArgvEx()`
  - `PySys_SetArgv()`
  - `PySys_SetPath()`
  - `Py_SetPath()`
  - `Py_SetProgramName()`
  - `Py_SetPythonHome()`
  - `Py_SetStandardStreamEncoding()`
  - `_Py_SetProgramFullPath()`
 Patch by Victor Stinner.
- gh-105107: Remove functions deprecated in Python 3.9. 
  - `PyEval_CallObject()` ,`PyEval_CallObjectWithKeywords()` : use`PyObject_CallNoArgs()` and`PyObject_Call()` (positional
arguments must not be*NULL* ) instead.
  - `PyEval_CallFunction()` : use`PyObject_CallFunction()` instead.
  - `PyEval_CallMethod()` : use`PyObject_CallMethod()` instead.
  - `PyCFunction_Call()` : use`PyObject_Call()` instead.
 Patch by Victor Stinner.
- gh-105115: `PyTypeObject.tp_bases` (and`tp_mro` ) for builtin static
types are now shared by all interpreters, whereas in 3.12-beta1 they were
stored on`PyInterpreterState` .  Also note that now the tuples are
immortal objects.
- gh-105071: Add `PyUnstable_Exc_PrepReraiseStar` to the unstable C api to
expose the implementation of`except*` .
- gh-104922: `PY_SSIZE_T_CLEAN` is no longer required to use`'#'` formats in APIs like`PyArg_ParseTuple()` and`Py_BuildValue()` . They uses`Py_ssize_t` for`'#'` regardless`PY_SSIZE_T_CLEAN` .
- gh-104584: Add an unstable C API for hooking in an optimizer. This is mainly internal, but marked âunstableâ to allow third-party experimentation.
- gh-104668: Donât call `PyOS_InputHook` or`PyOS_ReadlineFunctionPointer` in subinterpreters, since itâs
generally difficult to avoid using global state in their registered
callbacks. This also avoids situations where extensions may find
themselves running in a subinterpreter they donât support (or havenât yet
been loaded in).
- bpo-42327: Add `PyModule_Add()` function: similar to`PyModule_AddObjectRef()` and`PyModule_AddObject()` , but
always steals a reference to the value.
- bpo-40309: Properly handle trailing spaces before closing parenthesis in `Py_BuildValue()` format strings.

## Python 3.12.0 beta 1Â¶

*Release date: 2023-05-22*

### SecurityÂ¶

- gh-99889: Fixed a security in flaw in `uu.decode()` that could allow
for directory traversal based on the input if no`out_file` was
specified.
- gh-104049: Do not expose the local on-disk location in directory indexes produced by `http.client.SimpleHTTPRequestHandler` .
- gh-99108: Upgrade built-in `hashlib` SHA3 implementation to a
verified implementation from the`HACL*` project.  Used when OpenSSL is
not present or lacks SHA3.
- gh-102153: `urllib.parse.urlsplit()` now strips leading C0 control and
space characters following the specification for URLs defined by WHATWG in
response to**CVE 2023-24329** . Patch by Illia Volochii.

### LibraryÂ¶

### Core and BuiltinsÂ¶

- gh-104615: Fix wrong ordering of assignments in code like `a, a = x, y` .
Contributed by Carl Meyer.
- gh-104572: Improve syntax error message for invalid constructs in **PEP 695** contexts and in annotations when```
from __future__ import
annotations
```
is active.
- gh-104482: Fix three error handling bugs in ast.câs validation of pattern matching statements.
- gh-102818: Do not add a frame to the traceback in the `sys.setprofile` and`sys.settrace` trampoline functions. This ensures that frames are
not duplicated if an exception is raised in the callback function, and
ensures that frames are not omitted if a C callback is used and that does
not add the frame.
- gh-104405: Fix an issue where some bytecode instructions could ignore **PEP 523** when âinliningâ calls.
- gh-103082: Change behavior of `sys.monitoring.events.LINE` events in`sys.monitoring` : Line events now occur when a new line is reached
dynamically, instead of using a static approximation, as before. This
makes the behavior very similar to that of âlineâ events in`sys.settrace` . This should ease porting of tools from 3.11 to 3.12.
- gh-104263: Fix `float("nan")` to produce a quiet NaN on platforms (like
MIPS) where the meaning of the signalling / quiet bit is inverted from its
usual meaning. Also introduce a new macro`Py_INFINITY` matching C99âs`INFINITY` , and refactor internals to rely on C99âs`NAN` and`INFINITY` macros instead of hard-coding bit patterns for infinities and
NaNs. Thanks Sebastian Berg.
- gh-99113: Multi-phase init extension modules may now indicate that they support running in subinterpreters that have their own GIL. This is done by using `Py_MOD_PER_INTERPRETER_GIL_SUPPORTED` as the value for the`Py_mod_multiple_interpreters` module def slot.  Otherwise the module,
by default, cannot be imported in such subinterpreters.  (This does not
affect the main interpreter or subinterpreters that do not have their own
GIL.)  In addition to the isolation that multi-phase init already normally
requires, support for per-interpreter GIL involves one additional
constraint: thread-safety.  If the module has external (linked)
dependencies and those libraries have any state that isnât thread-safe
then the module must do the additional work to add thread-safety.  This
should be an uncommon case.
- gh-99113: The GIL is now (optionally) per-interpreter. This is the fundamental change for PEP 684. This is all made possible by virtue of the isolated state of each interpreter in the process. The behavior of the main interpreter remains unchanged. Likewise, interpreters created using `Py_NewInterpreter()` are not affected.  To get an interpreter
with its own GIL, call`Py_NewInterpreterFromConfig()` .
- gh-104108: Multi-phase init extension modules may now indicate whether or not they actually support multiple interpreters. By default such modules are expected to support use in multiple interpreters. In the uncommon case that one does not, it may use the new `Py_mod_multiple_interpreters` module def slot.  A value of`0` means
the module does not support them.`1` means it does.  The default is`1` .
- gh-104142: Fix an issue where `list` or`tuple` repetition
could fail to respect**PEP 683** .
- gh-104078: Improve the performance of `PyObject_HasAttrString()`
- gh-104066: Improve the performance of `hasattr()` for module objects
with a missing attribute.
- gh-104028: Reduce object creation while calling callback function from gc. Patch by Donghee Na.
- gh-104018: Disallow the âzâ format specifier in %-format of bytes objects.
- gh-102213: Fix performance loss when accessing an objectâs attributes with `__getattr__` defined.
- gh-103895: Improve handling of edge cases in showing `Exception.__notes__` . Ensures that the messages always end with a
newline and that string/bytes are not exploded over multiple lines. Patch
by Carey Metcalfe.
- gh-103907: Donât modify the refcounts of known immortal objects ( `True` ,`False` , and`None` ) in the main interpreter
loop.
- gh-103899: Provide a helpful hint in the `TypeError` message when
accidentally calling a module object that has a callable attribute
of the same name (such as`dis.dis()` or`datetime.datetime` ).
- gh-103845: Remove both line and instruction instrumentation before adding new ones for monitoring, to avoid newly added instrumentation being removed immediately.
- gh-103763: Implement **PEP 695** , adding syntactic support for generic
classes, generic functions, and type aliases.A new `type X = ...` syntax is added for type aliases, which resolves at
runtime to an instance of the new class`typing.TypeAliasType` . The
value is lazily evaluated and is accessible through the`.__value__` attribute. This is implemented as a new AST node`ast.TypeAlias` .New syntax ( `class X[T]: ...` ,`def func[T](): ...` ) is added for
defining generic functions and classes. This is implemented as a new`type_params` attribute on the AST nodes for classes and functions. This
node holds instances of the new AST classes`ast.TypeVar` ,`ast.ParamSpec` , and`ast.TypeVarTuple` .`typing.TypeVar` ,`typing.ParamSpec` ,`typing.ParamSpecArgs` ,`typing.ParamSpecKwargs` ,`typing.TypeVarTuple` , and`typing.Generic` are now implemented in C rather than Python.There are new bytecode instructions `LOAD_LOCALS` ,`LOAD_CLASSDICT_OR_GLOBAL` , and`LOAD_CLASSDICT_OR_DEREF` to support
correct resolution of names in class namespaces.Patch by Eric Traut, Larry Hastings, and Jelle Zijlstra.

### LibraryÂ¶

- gh-103801: Adds three minor linting fixes to the wasm module caught that were caught by ruff.
- gh-103793: Optimized asyncio Task creation by deferring expensive string formatting (task name generation) from Task creation to the first time `get_name` is called. This makes asyncio benchmarks up to 5% faster.

### Core and BuiltinsÂ¶

- gh-102310: Change the error range for invalid bytes literals.
- gh-103590: Do not wrap a single exception raised from a `try-except*` construct in an`ExceptionGroup` .
- gh-103650: Change the perf map format to remove the â0xâ prefix from the addresses
- gh-102856: Implement the required C tokenizer changes for PEP 701. Patch by Pablo Galindo Salgado, Lysandros Nikolaou, Batuhan Taskaya, Marta GÃ³mez MacÃas and sunmy2019.
- gh-100530: Clarify the error message raised when the called part of a class pattern isnât actually a class.
- gh-101517: Fix bug in line numbers of instructions emitted for `except*` .
- gh-103492: Clarify `SyntaxWarning` with literal`is` comparison by
specifying which literal is problematic, since comparisons using`is` with e.g.`None` and bool literals are idiomatic.
- gh-87729: Add `LOAD_SUPER_ATTR` (and a specialization for`super().method()` ) to speed up`super().method()` and`super().attr` . This makes`super().method()` roughly 2.3x faster and
brings it within 20% of the performance of a simple method call. Patch by
Vladimir Matveev and Carl Meyer.
- gh-103488: Change the internal offset distinguishing yield and return target addresses, so that the instruction pointer is correct for exception handling and other stack unwinding.
- gh-82012: The bitwise inversion operator ( `~` ) on bool is deprecated. It
returns the bitwise inversion of the underlying`int` representation
such that`bool(~True) == True` , which can be confusing. Use`not` for
logical negation of bools. In the rare case that you really need the
bitwise inversion of the underlying`int` , convert to int explicitly`~int(x)` .
- gh-77757: Exceptions raised in a typeobjectâs `__set_name__` method are
no longer wrapped by a`RuntimeError` . Context information is added
to the exception as a**PEP 678** note.
- gh-103333: `AttributeError` now retains the`name` attribute when
pickled and unpickled.
- gh-103242: Migrate `set_ecdh_curve()` method not to
use deprecated OpenSSL APIs. Patch by Donghee Na.
- gh-103323: Weâve replaced our use of `_PyRuntime.tstate_current` with a
thread-local variable.  This is a fairly low-level implementation detail,
and there should be no change in behavior.
- gh-84436: The implementation of PEP-683 which adds Immortal Objects by using a fixed reference count that skips reference counting to make objects truly immutable.
- gh-102700: Allow built-in modules to be submodules. This allows submodules to be statically linked into a CPython binary.
- gh-103082: Implement **PEP 669** Low Impact Monitoring for CPython.
- gh-88691: Reduce the number of inline `CACHE` entries for`CALL` .
- gh-102500: Make the buffer protocol accessible in Python code using the new `__buffer__` and`__release_buffer__` magic methods. See**PEP 688** for details. Patch by Jelle Zijlstra.
- gh-97933: **PEP 709** : inline list, dict and set comprehensions to improve
performance and reduce bytecode size.
- gh-99184: Bypass instance attribute access of `__name__` in`repr` of`weakref.ref` .
- gh-98003: Complex function calls are now faster and consume no C stack space.
- bpo-39610: `len()` for 0-dimensional`memoryview` objects (such
as`memoryview(ctypes.c_uint8(42))` ) now raises a`TypeError` .
Previously this returned`1` , which was not consistent with`mem_0d[0]` raising an`IndexError` .

### LibraryÂ¶

- bpo-31821: Fix `pause_reading()` to work when called from`connection_made()` in`asyncio` .
- gh-104600: `functools.update_wrapper()` now sets the`__type_params__` attribute (added by**PEP 695** ).
- gh-104340: When an `asyncio` pipe protocol loses its connection due to
an error, and the caller doesnât await`wait_closed()` on the
corresponding`StreamWriter` , donât log a warning about an exception
that was never retrieved. After all, according to the`StreamWriter.close()` docs, the`wait_closed()` call is optional
(ânot mandatoryâ).
- gh-104555: Fix issue where an `issubclass()` check comparing a class`X` against a`runtime-checkable protocol``Y` with non-callable members would not
cause`TypeError` to be raised if an`isinstance()` call had
previously been made comparing an instance of`X` to`Y` . This issue
was present in edge cases on Python 3.11, but became more prominent in
3.12 due to some unrelated changes that were made to runtime-checkable
protocols. Patch by Alex Waygood.
- gh-104372: Refactored the `_posixsubprocess` internals to avoid Python C
API usage between fork and exec when marking`pass_fds=` file
descriptors inheritable.
- gh-104484: Added *case_sensitive* argument to`pathlib.PurePath.match()`
- gh-75367: Fix data descriptor detection in `inspect.getattr_static()` .
- gh-104536: Fix a race condition in the internal `multiprocessing.process` cleanup logic that could manifest as an
unintended`AttributeError` when calling`process.close()` .
- gh-103857: Update datetime deprecationsâ stracktrace to point to the calling line
- gh-101520: Move the core functionality of the `tracemalloc` module in
the`Python/` folder, leaving just the module wrapper in`Modules/` .
- gh-104392: Remove undocumented and unused `_paramspec_tvars` attribute
from some classes in`typing` .
- gh-102613: Fix issue where `pathlib.Path.glob()` raised`RecursionError` when walking deep directory trees.
- gh-103000: Improve performance of `dataclasses.asdict()` for the
common case where*dict_factory* is`dict` . Patch by David C Ellis.
- gh-104301: Allow leading whitespace in disambiguated statements in `pdb` .
- gh-104139: Teach `urllib.parse.unsplit()` to retain the`"//"` when
assembling`itms-services://?action=generate-bugs` style Apple Platform
Deployment
URLs.
- gh-104307: `socket.getnameinfo()` now releases the GIL while
contacting the DNS server
- gh-104310: Users may now use `importlib.util.allowing_all_extensions()` (a context manager) to temporarily disable the strict compatibility checks
for importing extension modules in subinterpreters.
- gh-87695: Fix issue where `pathlib.Path.glob()` raised`OSError` when it encountered a symlink to an overly long path.
- gh-104265: Prevent possible crash by disallowing instantiation of the `_csv.Reader` and`_csv.Writer` types. The regression was
introduced in 3.10.0a4 with PR 23224 (bpo-14935). Patch by Radislav
Chugunov.
- gh-102613: Improve performance of `pathlib.Path.glob()` when expanding
recursive wildcards (â`**` â) by merging adjacent wildcards and
de-duplicating results only when necessary.
- gh-65772: Remove unneeded comments and code in turtle.py.
- gh-90208: Fixed issue where `pathlib.Path.glob()` returned incomplete
results when it encountered a`PermissionError` . This method now
suppresses all`OSError` exceptions, except those raised from calling`is_dir()` on the top-level path.
- gh-104144: Optimize `asyncio.TaskGroup` when using`asyncio.eager_task_factory()` . Skip scheduling a done callback if a
TaskGroup task completes eagerly.
- gh-104144: Optimize `asyncio.gather()` when using`asyncio.eager_task_factory()` to complete eagerly if all fututres
completed eagerly. Avoid scheduling done callbacks for futures that
complete eagerly.
- gh-104114: Fix issue where `pathlib.Path.glob()` returns paths using
the case of non-wildcard segments for corresponding path segments, rather
than the real filesystem case.
- gh-104104: Improve performance of `pathlib.Path.glob()` by using`re.IGNORECASE` to implement case-insensitive matching.
- gh-104102: Improve performance of `pathlib.Path.glob()` when
evaluating patterns that contain`'../'` segments.
- gh-103822: Update the return type of `weekday` to the newly added Day
attribute
- gh-103629: Update the `repr` of`typing.Unpack` according to**PEP 692** .
- gh-103963: Make `dis` display the names of the args for`CALL_INTRINSIC_*` .
- gh-104035: Do not ignore user-defined `__getstate__` and`__setstate__` methods for slotted frozen dataclasses.
- gh-103987: In `mmap` , fix several bugs that could lead to access to
memory-mapped files after they have been invalidated.
- gh-103977: Improve import time of `platform` module.
- gh-88773: Added `turtle.teleport()` to the`turtle` module to
move a turtle to a new point without tracing a line, visible or invisible.
Patch by Liam Gersten.
- gh-103935: Use `io.open_code()` for files to be executed instead of
raw`open()`
- gh-68968: Fixed garbled output of `assertEqual()` when an input lacks final newline.
- gh-100370: Fix potential `OverflowError` in`sqlite3.Connection.blobopen()` for 32-bit builds. Patch by Erlend E.
Aasland.
- gh-102628: Substitute CTRL-D with CTRL-Z in `sqlite3` CLI banner when
running on Windows.
- gh-103636: Module-level attributes `January` and`February` are
deprecated from`calendar` .
- gh-103583: Isolate `_multibytecodec` and codecs extension modules.
Patches by Erlend E. Aasland.
- gh-103848: Add checks to ensure that `[` bracketed`]` hosts found by`urllib.parse.urlsplit()` are of IPv6 or IPvFuture format.
- gh-103872: Update the bundled copy of pip to version 23.1.2.
- gh-99944: Make `dis` display the value of oparg of`KW_NAMES` .
- gh-74940: The C.UTF-8 locale is no longer converted to en_US.UTF-8, enabling the use of UTF-8 encoding on systems which have no locales installed.
- gh-103861: Fix `zipfile.Zipfile` creating invalid zip files when`force_zip64` was used to add files to them. Patch by Carey Metcalfe.
- gh-103857: Deprecated `datetime.datetime.utcnow()` and`datetime.datetime.utcfromtimestamp()` . (Patch by Paul Ganssle)
- gh-103839: Avoid compilation error due to tommath.h not being found when building Tkinter against Tcl 8.7 built with bundled libtommath.
- gh-103791: `contextlib.suppress` now supports suppressing
exceptions raised as part of an`ExceptionGroup` . If other exceptions
exist on the group, they are re-raised in a group that does not contain
the suppressed exceptions.
- gh-90750: Use `datetime.datetime.fromisocalendar()` in the
implementation of`datetime.datetime.strptime()` , which should now
accept only valid ISO dates. (Patch by Paul Ganssle)
- gh-103685: Prepare `tkinter.Menu.index()` for Tk 8.7 so that it does
not raise`TclError: expected integer but got ""` when it should return`None` .
- gh-81403: `urllib.request.CacheFTPHandler` no longer raises`URLError` if a cached FTP instance is reused. ftplibâs endtransfer
method calls voidresp to drain the connection to handle FTP instance reuse
properly.
- gh-103699: Add `__orig_bases__` to non-generic TypedDicts, call-based
TypedDicts, and call-based NamedTuples. Other TypedDicts and NamedTuples
already had the attribute.
- gh-103693: Add convenience variable feature to `pdb`
- gh-92248: Deprecate `type` ,`choices` , and`metavar` parameters of`argparse.BooleanOptionalAction` .
- gh-89415: Add `socket` constants for source-specific multicast. Patch
by Reese Hyde.
- gh-103673: `socketserver` gains`ForkingUnixStreamServer` and`ForkingUnixDatagramServer` classes. Patch by Jay Berry.
- gh-103636: Added Enum for months and days in the calendar module.
- gh-84976: Create a new `Lib/_pydatetime.py` file that defines the Python
version of the`datetime` module, and make`datetime` import the
contents of the new library only if the C implementation is missing.
Currently, the full Python implementation is defined and then deleted if
the C implementation is not available, slowing down`import datetime` unnecessarily.
- gh-103596: Attributes/methods are no longer shadowed by same-named enum members, although they may be shadowed by enum.propertyâs.
- gh-103584: Updated `importlib.metadata` with changes from`importlib_metadata` 5.2 through 6.5.0, including: Support`installed-files.txt` for`Distribution.files` when present.`PackageMetadata` now stipulates an additional`get` method allowing
for easy querying of metadata keys that may not be present.`packages_distributions` now honors packages and modules with Python
modules that not`.py` sources (e.g.`.pyc` ,`.so` ). Expand protocol
for`PackageMetadata.get_all` to match the upstream implementation of`email.message.Message.get_all` in python/typeshed#9620. Deprecated use
of`Distribution` without defining abstract methods. Deprecated
expectation that`PackageMetadata.__getitem__` will return`None` for
missing keys. In the future, it will raise a`KeyError` .
- gh-103578: Fixed a bug where `pdb` crashes when reading source file
with different encoding by replacing`io.open()` with`io.open_code()` . The new method would also call into the hook set by`PyFile_SetOpenCodeHook()` .
- gh-103556: Now creating `inspect.Signature` objects with
positional-only parameter with a default followed by a
positional-or-keyword parameter without one is impossible.
- gh-103559: Update the bundled copy of pip to version 23.1.1.
- gh-103548: Improve performance of `pathlib.Path.absolute()` and`cwd()` by joining paths only when necessary. Also
improve performance of`pathlib.PurePath.is_absolute()` on Posix by
skipping path parsing and normalization.
- gh-103538: Remove `_tkinter` module code guarded by definition of the`TK_AQUA` macro which was only needed for Tk 8.4.7 or earlier and was
never actually defined by any build system or documented for manual use.
- gh-103533: Update `cProfile` to use PEP 669 API
- gh-103525: Fix misleading exception message when mixed `str` and`bytes` arguments are supplied to`pathlib.PurePath` and`Path` .
- gh-103489: Add `getconfig()` and`setconfig()` to`Connection` to
make configuration changes to a database connection. Patch by Erlend E.
Aasland.
- gh-103365: Set default Flag boundary to `STRICT` and fix bitwise
operations.
- gh-103472: Avoid a potential `ResourceWarning` in`http.client.HTTPConnection` by closing the proxy / tunnelâs
CONNECT response explicitly.
- gh-103462: Fixed an issue with using `writelines()` in`asyncio` to send very
large payloads that exceed the amount of data that can be written in one
call to`socket.socket.send()` or`socket.socket.sendmsg()` ,
resulting in the remaining buffer being left unwritten.
- gh-103449: Fix a bug in doc string generation in `dataclasses.dataclass()` .
- gh-103092: Isolate `_collections` (apply**PEP 687** ). Patch by
Erlend E. Aasland.
- gh-103357: Added support for `logging.Formatter``defaults` parameter to`logging.config.dictConfig()` and`logging.config.fileConfig()` . Patch by Bar Harel.
- gh-103092: Adapt the `winreg` extension module to**PEP 687** .
- gh-74690: The performance of `isinstance()` checks against`runtime-checkable protocols` has been
considerably improved for protocols that only have a few members. To
achieve this improvement, several internal implementation details of the`typing` module have been refactored, including`typing._ProtocolMeta.__instancecheck__` ,`typing._is_callable_members_only` , and`typing._get_protocol_attrs` .
Patches by Alex Waygood.
- gh-74690: The members of a runtime-checkable protocol are now considered âfrozenâ at runtime as soon as the class has been created. See âWhatâs new in Python 3.12â for more details.
- gh-103256: Fixed a bug that caused `hmac` to raise an exception when
the requested hash algorithm was not available in OpenSSL despite being
available separately as part of`hashlib` itself.  It now falls back
properly to the built-in. This could happen when, for example, your
OpenSSL does not include SHA3 support and you want to compute`hmac.digest(b'K', b'M', 'sha3_256')` .
- gh-102778: Support `sys.last_exc` in`idlelib` .
- gh-103285: Improve performance of `ast.get_source_segment()` .
- gh-103225: Fix a bug in `pdb` when displaying line numbers of
module-level source code.
- gh-103092: Adapt the `msvcrt` extension module to**PEP 687** .
- gh-103092: Adapt the `winsound` extension module to**PEP 687** .
- gh-93910: Remove deprecation of enum `member.member` access.
- gh-102978: Fixes `unittest.mock.patch()` not enforcing function
signatures for methods decorated with`@classmethod` or`@staticmethod` when patch is called with`autospec=True` .
- gh-103092: Isolate `_socket` (apply**PEP 687** ). Patch by Erlend E.
Aasland.
- gh-100479: Add `pathlib.PurePath.with_segments()` , which creates a
path object from arguments. This method is called whenever a derivative
path is created, such as from`pathlib.PurePath.parent` . Subclasses
may override this method to share information between path objects.
- gh-103220: Fix issue where `os.path.join()` added a slash when joining
onto an incomplete UNC drive with a trailing slash on Windows.
- gh-103204: Fixes `http.server` accepting HTTP requests with HTTP
version numbers preceded by â+â, or â-â, or with digit-separating â_â
characters.  The length of the version numbers is also constrained.
- gh-75586: Fix various Windows-specific issues with `shutil.which` .
- gh-103193: Improve performance of `inspect.getattr_static()` . Patch by
Alex Waygood.
- gh-103176: `sys._current_exceptions()` now returns a mapping from
thread-id to an exception instance, rather than to a`(typ, exc, tb)` tuple.
- gh-103143: Polish the help messages and docstrings of `pdb` .
- gh-103015: Add *entrypoint* keyword-only parameter to`sqlite3.Connection.load_extension()` , for overriding the SQLite
extension entry point. Patch by Erlend E. Aasland.
- gh-103000: Improve performance of `dataclasses.astuple()` and`dataclasses.asdict()` in cases where the contents are common Python
types.
- gh-102953: The extraction methods in `tarfile` , and`shutil.unpack_archive()` , have a new a*filter* argument that allows
limiting tar features than may be surprising or dangerous, such as
creating files outside the destination directory. See
Extraction filters for details.
- gh-97696: Implemented an eager task factory in asyncio. When used as a task factory on an event loop, it performs eager execution of coroutines. Coroutines that are able to complete synchronously (e.g. return or raise without blocking) are returned immediately as a finished task, and the task is never scheduled to the event loop. If the coroutine blocks, the (pending) task is scheduled and returned.
- gh-81079: Add *case_sensitive* keyword-only argument to`pathlib.Path.glob()` and`rglob()` .
- gh-101819: Isolate the `io` extension module by applying**PEP 687** .
Patch by Kumar Aditya, Victor Stinner, and Erlend E. Aasland.
- gh-91896: Deprecate `collections.abc.ByteString`
- gh-101362: Speed up `pathlib.Path` construction by omitting the
path anchor from the internal list of path parts.
- gh-102114: Functions in the `dis` module that accept a source code
string as argument now print a more concise traceback when the string
contains a syntax or indentation error.
- gh-62432: The `unittest` runner will now exit with status code 5 if
no tests were run. It is common for test runner misconfiguration to fail
to find any tests, this should be an error.
- gh-78079: Fix incorrect normalization of UNC device path roots, and partial UNC share path roots, in `pathlib.PurePath` . Pathlib no
longer appends a trailing slash to such paths.
- gh-85984: Add `tty.cfmakeraw()` and`tty.cfmakecbreak()` to`tty` and modernize, the behavior of`tty.setraw()` and`tty.setcbreak()` to use POSIX.1-2017 Chapter 11 âGeneral Terminal
Interfaceâ flag masks by default.
- gh-101688: Implement `types.get_original_bases()` to provide further
introspection for types.
- gh-101640: `argparse.ArgumentParser` now catches errors when
writing messages, such as when`sys.stderr` is`None` . Patch by
Oleg Iarygin.
- gh-83861: Fix datetime.astimezone method return value when invoked on a naive datetime instance that represents local time falling in a timezone transition gap. PEP 495 requires that instances with fold=1 produce earlier times than those with fold=0 in this case.
- gh-89550: Decrease execution time of some `gzip` file writes by 15%
by adding more appropriate buffering.
- gh-95299: Remove the bundled setuptools wheel from `ensurepip` , and stop
installing setuptools in environments created by`venv` .
- gh-99353: Respect the `http.client.HTTPConnection``.debuglevel` flag in`urllib.request.AbstractHTTPHandler` when its constructor
parameter`debuglevel` is not set. And do the same for`*HTTPS*` .
- gh-98040: Remove the long-deprecated `imp` module.
- gh-97850: Deprecate `pkgutil.find_loader()` and`pkgutil.get_loader()` in favor of`importlib.util.find_spec()` .
- gh-94473: Flatten arguments in `tkinter.Canvas.coords()` . It now
accepts not only`x1, y1, x2, y2, ...` and`[x1, y1, x2, y2, ...]` ,
but also`(x1, y1), (x2, y2), ...` and`[(x1, y1), (x2, y2), ...]` .
- gh-98040: Remove more deprecated importlib APIs: `find_loader()` ,`find_module()` ,`importlib.abc.Finder` ,`pkgutil.ImpImporter` ,`pkgutil.ImpLoader` .
- gh-96522: Fix potential deadlock in pty.spawn()
- gh-96534: Support divert(4) added in FreeBSD 14.
- gh-87474: Fix potential file descriptor leaks in `subprocess.Popen` .
- gh-94906: Support multiple steps in `math.nextafter()` . Patch by
Shantanu Jain and Matthias Gorgens.
- gh-51574: Make `tempfile.mkdtemp()` return absolute paths when its*dir* parameter is relative.
- gh-94518: Convert private `_posixsubprocess.fork_exec()` to use
Argument Clinic.
- gh-92184: When creating zip files using `zipfile` ,`os.altsep` , if
not`None` , will always be treated as a path separator even when it is
