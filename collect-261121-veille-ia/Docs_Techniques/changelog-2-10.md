---
id: collect-261121-veille-ia/docs-techniques/changelog-2-10
title: "Changelog"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["instruction", "patch", "path", "number", "column", "method", "cache", "bytecode"]
source: https://docs.python.org/release/3.15.1/whatsnew/changelog.html
source_anchor: ""
source_lines: [13501, 15000]
section: "Docs_Techniques"
---
- gh-93671: Fix some exponential backtrace case happening with deeply nested sequence patterns in match statements. Patch by Pablo Galindo
- gh-93662: Make sure that the end column offsets are correct in multi-line method calls. Previously, the end column could precede the column offset.

### LibraryÂ¶

- gh-93461: `importlib.invalidate_caches()` now drops entries from`sys.path_importer_cache` with a relative path as name. This solves
a caching issue when a process changes its current working directory.`FileFinder` no longer inserts a dot in the path, e.g.`/egg/./spam` is now`/egg/spam` .

### Core and BuiltinsÂ¶

- gh-93621: Change order of bytecode instructions emitted for `with` and`async with` to reduce the number of entries
in the exception table.
- gh-93533: Reduce the size of the inline cache for `LOAD_METHOD` by 2
bytes.
- gh-93444: Removed redundant fields from the compilerâs basicblock struct: `b_nofallthrough` ,`b_exit` ,`b_return` . They can be easily
calculated from the opcode of the last instruction of the block.
- gh-93429: `LOAD_METHOD` instruction has been removed. It was merged back
into`LOAD_ATTR` .
- gh-93418: Fixed an assert where an f-string has an equal sign â=â following an expression, but thereâs no trailing brace. For example, fâ{i=â.
- gh-93382: Cache the result of `PyCode_GetCode()` function to restore
the*O* (1) lookup of the`co_code` attribute.
- gh-93359: Ensure that custom `ast` nodes without explicit end
positions can be compiled. Patch by Pablo Galindo.
- gh-93356: Code for exception handlers is emitted at the end of the code unitâs bytecode. This avoids one jump when no exception is raised.
- gh-93354: Use exponential backoff for specialization counters in the interpreter. Can reduce the number of failed specializations significantly and avoid slowdown for those parts of a program that are not suitable for specialization.
- gh-93283: Improve error message for invalid syntax of conversion character in f-string expressions.
- gh-93345: Fix a crash in substitution of a `TypeVar` in nested generic
alias after`TypeVarTuple` .
- gh-93223: When a bytecode instruction jumps to an unconditional jump instruction, the first instruction can often be optimized to target the unconditional jumpâs target directly. For tracing reasons, this would previously only occur if both instructions have the same line number. This also now occurs if the unconditional jump is artificial, i.e., if it has no associated line number.
- gh-84694: The `--experimental-isolated-subinterpreters` configure option
and`EXPERIMENTAL_ISOLATED_SUBINTERPRETERS` macro have been removed.
- gh-91924: Fix `__lltrace__` debug feature if the stdout encoding is not
UTF-8. Patch by Victor Stinner.
- gh-93040: Wraps unused parameters in `Objects/obmalloc.c` with`Py_UNUSED` .
- gh-93143: Avoid `NULL` checks for uninitialized local variables by
determining at compile time which variables must be initialized.
- gh-93061: Backward jumps after `async for` loops are no longer given
dubious line numbers.
- gh-93065: Fix contextvars HAMT implementation to handle iteration over deep trees. The bug was discovered and fixed by Eli Libman. See MagicStack/immutables#84 for more details.
- gh-93012: Added the new function `PyType_FromMetaclass()` , which
generalizes the existing`PyType_FromModuleAndSpec()` using an
additional metaclass argument. This is useful for language binding tools,
where it can be used to intercept type-related operations like subclassing
or static attribute access by specifying a metaclass with custom slots.Importantly, `PyType_FromMetaclass()` is available in the Limited
API, which provides a path towards migrating more binding tools onto the
Stable ABI.
- gh-93021: Fix the `__text_signature__` for`__get__()` methods
implemented in C. Patch by Jelle Zijlstra.
- gh-89914: The operand of the `YIELD_VALUE` instruction is set to the
stack depth. This is done to help frame handling on`yield` and may
assist debuggers.
- gh-92955: Fix memory leak in code objectâs lines and positions iterators as they were not finalized at exit. Patch by Kumar Aditya.
- gh-92930: Fixed a crash in `_pickle.c` from mutating collections during`__reduce__` or`persistent_id` .
- gh-90690: The PRECALL instruction has been removed. It offered only a small advantage for specialization and is not needed in the vast majority of cases.
- gh-92914: Always round the allocated size for lists up to the nearest even number.
- gh-92858: Improve error message for some suites with syntax error before â:â
- gh-90473: Decrease default recursion limit on WASI to address limited call stack size.
- gh-92804: Fix memory leak in `memoryview` iterator as it was not
finalized at exit. Patch by Kumar Aditya.
- gh-92777: Specialize `LOAD_METHOD` for objects with lazy dictionaries.
Patch by Ken Jin.
- gh-92658: Add support for connecting and binding to Hyper-V sockets on Windows Hyper-V hosts and guests.
- gh-92236: Remove spurious âLINEâ event when starting a generator or coroutine, visible tracing functions implemented in C.

### LibraryÂ¶

- gh-91102: `_warnings.warn_explicit()` is ported to Argument Clinic.

### Core and BuiltinsÂ¶

- gh-92619: Make the compiler duplicate an exit block only if none of its instructions have a lineno (previously only the first instruction in the block was checked, leading to unnecessarily duplicated blocks).
- gh-88750: The deprecated debug build only `PYTHONTHREADDEBUG` environment variable no longer does anything.

### LibraryÂ¶

- gh-92261: Fix hang when trying to iterate over a `typing.Union` .

### Core and BuiltinsÂ¶

- gh-91432: Specialized the `FOR_ITER` opcode using the PEP 659
machinery
- gh-91399: Removed duplicate â{0, 0, 0, 0, 0, 0}â entry in âObjects/unicodetype_db.hâ.
- gh-91578: Updates the error message for abstract class.
- bpo-47091: Improve performance of repetition of `list` and`tuple` by using`memcpy` to copy data and performing the
reference increments in one step.
- bpo-46142: Make `--help` output shorter by moving some info to the new`--help-env` and`--help-xoptions` command-line options. Also add`--help-all` option to print complete usage.
- bpo-42316: Document some places where an assignment expression needs parentheses.

### LibraryÂ¶

- gh-89237: Fix hang on Windows in `subprocess.wait_closed()` in`asyncio` with`ProactorEventLoop` . Patch by Kumar
Aditya.
- gh-97928: `tkinter.Text.count()` raises now an exception for options
starting with â-â instead of silently ignoring them.
- gh-98393: The `os` module no longer accepts bytes-like paths, like`bytearray` and`memoryview` types: only the exact`bytes` type is accepted for bytes strings. Patch by Victor
Stinner.
- gh-98363: Added itertools.batched() to batch data into lists of a given length with the last list possibly being shorter than the others.
- gh-98331: Update the bundled copies of pip and setuptools to versions 22.3 and 65.5.0 respectively.
- gh-98307: A `createSocket()` method
was added to`SysLogHandler` .
- gh-96035: Fix bug in `urllib.parse.urlparse()` that causes certain
port numbers containing whitespace, underscores, plus and minus signs, or
non-ASCII digits to be incorrectly accepted.
- gh-98257: Make `sys.setprofile()` and`sys.settrace()` functions
reentrant. They can no long fail with:```
RuntimeError("Cannot install a
trace function while another trace function is being installed")
```
. Patch
by Victor Stinner.
- gh-98251: Allow `venv` to pass along`PYTHON*` variables to`ensurepip` and`pip` when they do not impact path resolution
- gh-94597: Deprecated `asyncio.AbstractEventLoopPolicy.get_child_watcher()` and`asyncio.AbstractEventLoopPolicy.set_child_watcher()` methods to be
removed in Python 3.14. Patch by Kumar Aditya.
- gh-98178: On macOS, fix a crash in `syslog.syslog()` in multi-threaded
applications. On macOS, the libc`syslog()` function is not thread-safe,
so`syslog.syslog()` no longer releases the GIL to call it. Patch by
Victor Stinner.
- gh-44098: Release the GIL when creating `mmap.mmap` objects on
Unix.
- gh-87730: Wrap network errors consistently in urllib FTP support, so the test suite doesnât fail when a network is available but the public internet is not reachable.
- gh-94597: The child watcher classes `MultiLoopChildWatcher` ,`FastChildWatcher` and`SafeChildWatcher` are deprecated and will be removed in Python 3.14. Patch by Kumar Aditya.
- gh-98023: Change default child watcher to `PidfdChildWatcher` on Linux systems which supports it.
Patch by Kumar Aditya.
- gh-90985: Earlier in 3.11 we deprecated `asyncio.Task.cancel("message")` . We realized we were too harsh, and
have undeprecated it.
- gh-65961: Do not rely solely on `__cached__` on modules; code will also
support`__spec__.cached` .
- gh-97646: Replace deprecated `application/javascript` with`text/javascript` in`mimetypes` . See**RFC 9239** . Patch by Noam
Cohen.
- gh-97930: Apply changes from importlib_resources 5.8 and 5.9: `Traversable.joinpath` provides a concrete implementation.`as_file` now supports directories of resources.
- gh-97850: Remove deprecated `importlib.util.set_loader()` and`importlib.util.module_for_loader()` from`importlib.util` .
- gh-97837: Change deprecate warning message in `unittest` from`It is deprecated to return a value!=None`to `It is deprecated to return a value that is not None from a test case`
- gh-97825: Fixes `AttributeError` when`subprocess.check_output()` is used with argument`input=None` and either of the arguments*encoding* or*errors* are used.
- gh-97008: `NameError` and`AttributeError` spelling suggestions
provided since gh-82711 are now also emitted by the pure Python`traceback` module. Tests for those suggestions now exercise both
implementations to ensure they are equivalent. Patch by Carl Friedrich
Bolz-Tereick and Åukasz Langa.
- gh-97799: `dataclass` now uses`inspect.get_annotations()` to
examine the annotations on class objects.
- gh-97781: Removed deprecated interfaces in `importlib.metadata` (entry
points accessed as dictionary, implicit dictionary construction of
sequence of`EntryPoint` objects, mutablility of`EntryPoints` result,
access of entry point by index).`entry_points` now has a simpler, more
straightforward API (returning`EntryPoints` ).
- gh-96827: Avoid spurious tracebacks from `asyncio` when default
executor cleanup is delayed until after the event loop is closed (e.g. as
the result of a keyboard interrupt).
- gh-95534: `gzip.GzipFile.read()` reads 10% faster.
- gh-97592: Avoid a crash in the C version of `asyncio.Future.remove_done_callback()` when an evil argument is
passed.
- gh-97639: Remove `tokenize.NL` check from`tabnanny` .
- gh-97545: Make Semaphore run faster.
- gh-73588: Fix generation of the default name of `tkinter.Checkbutton` . Previously, checkbuttons in different parent
widgets could have the same short name and share the same state if
arguments ânameâ and âvariableâ are not specified. Now they are globally
unique.
- gh-96865: fix Flag to use boundary CONFORM This restores previous Flag behavior of allowing flags with non-sequential values to be combined; e.g. class Skip(Flag): TWO = 2 EIGHT = 8 Skip.TWO | Skip.EIGHT -> <Skip.TWO|EIGHT: 10>
- gh-97005: Update bundled libexpat to 2.4.9
- gh-85760: Fix race condition in `asyncio` where`process_exited()` called before the`pipe_data_received()` leading to
inconsistent output. Patch by Kumar Aditya.
- gh-96704: Pass the correct `contextvars.Context` when a`asyncio` exception handler is called on behalf of a task or callback handle. This
adds a new`Task` method,`get_context` , and also a new`Handle` method with the same name. If this method is not found on a task object
(perhaps because it is a third-party library that does not yet provide
this method), the context prevailing at the time the exception handler is
called is used.
- gh-96819: Fixed check in `multiprocessing.resource_tracker` that
guarantees that the length of a write to a pipe is not greater than`PIPE_BUF` .
- gh-95865: Reduce `urllib.parse.quote_from_bytes()` memory use on large
values.Contributed by Dennis Sweeney.
- gh-96741: Corrected type annotation for dataclass attribute `pstats.FunctionProfile.ncalls` to be`str` .
- gh-96734: Update `unicodedata` database to Unicode 15.0.0.
- gh-96735: Fix undefined behaviour in `struct.unpack()` .
- gh-46412: Improve performance of `bool(db)` for large ndb/gdb databases.
Previously this would call`len(db)` which would iterate over all keys
â the answer (empty or not) is known after the first key.
- gh-96652: Fix the faulthandler implementation of `faulthandler.register(signal, chain=True)` if the`sigaction()` function is not available: donât call the previous signal handler if itâs
NULL. Patch by Victor Stinner.
- gh-68163: Correct conversion of `numbers.Rational` âs to`float` .
- gh-96538: Speed up `bisect.bisect()` functions by taking advantage of
type-stability.
- gh-96465: Fraction hashes are now cached.
- gh-96079: In `typing` , fix missing field`name` and incorrect`__module__` in _AnnotatedAlias.
- gh-96415: Remove `types._cell_factory` from module namespace.
- gh-95987: Fix `repr` of`Any` subclasses.
- gh-96388: Work around missing socket functions in `socket` âs`__repr__` .
- gh-96385: Fix `TypeVarTuple.__typing_prepare_subst__` .`TypeError` was
not raised when using more than one`TypeVarTuple` , like`[*T, *V]` in
type alias substitutions.
- gh-96142: Add `match_args` ,`kw_only` ,`slots` , and`weakref_slot` to`_DataclassParams` .
- gh-96073: In `inspect` , fix overeager replacement of â`typing.` â in
formatting annotations.
- gh-89258: Added a `getChildren()` method to`logging.Logger` , to get the immediate child loggers of a logger.
- gh-96346: Use double caching for compiled RE patterns.
- gh-96349: Fixed a minor performance regression in `threading.Event.__init__()`
- gh-90467: Fix `asyncio.streams.StreamReaderProtocol` to keep a
strong reference to the created task, so that itâs not garbage collected
- gh-96172: Fix a bug in `unicodedata` :`east_asian_width` used to
return the wrong value for unassigned characters; and for yet unassigned,
but reserved characters.
- gh-96159: Fix a performance regression in logging TimedRotatingFileHandler. Only check for special files when the rollover time has passed.
- gh-96175: Fix unused `localName` parameter in the`Attr` class in`xml.dom.minidom` .
- gh-96145: Add AttrDict to JSON module for use with object_hook.
- gh-96052: Fix handling compiler warnings (SyntaxWarning and DeprecationWarning) in `codeop.compile_command()` when checking for
incomplete input. Previously it emitted warnings and raised a SyntaxError.
Now it always returns`None` for incomplete input without emitting any
warnings.
- gh-96125: Fix incorrect condition that causes `sys.thread_info.name` to
be wrong on pthread platforms.
- gh-96019: Fix a bug in the `makeunicodedata.py` script leading to about
13 KiB of space saving in the`unicodedata` module, specifically the
character decomposition data.
- gh-95463: Remove an incompatible change from bpo-28080 that caused a regression that ignored the utf8 in `ZipInfo.flag_bits` . Patch by Pablo
Galindo.
- gh-69142: Add `%:z` strftime format code (generates tzoffset with colons
as separator), see strftime() and strptime() behavior.
- gh-95899: Fix `asyncio.Runner` to call`asyncio.set_event_loop()` only once to avoid calling`attach_loop()` multiple times on child
watchers. Patch by Kumar Aditya.
- gh-95736: Fix `unittest.IsolatedAsyncioTestCase` to set event loop
before calling setup functions. Patch by Kumar Aditya.
- gh-95865: Speed up `urllib.parse.quote_from_bytes()` by replacing a
list comprehension with`map()` .
- gh-95861: Add support for computing Spearmanâs correlation coefficient to the existing statistics.correlation() function.
- gh-95804: Fix `logging` shutdown handler so it respects`MemoryHandler.flushOnClose` .
- gh-95704: When a task catches `asyncio.CancelledError` and raises
some other error, the other error should generally not silently be
suppressed.
- gh-95149: The `HTTPStatus` enum offers a couple
of properties to indicate the HTTP status category e.g.`HTTPStatus.OK.is_success` .
- gh-95609: Update bundled pip to 22.2.2.
- gh-95289: Fix `asyncio.TaskGroup` to propagate exception when`asyncio.CancelledError` was replaced with another exception by a
context manager. Patch by Kumar Aditya and Guido van Rossum.
- gh-94909: Fix incorrect joining of relative Windows paths with drives in `pathlib.PurePath` initializer.
- gh-95385: Faster `json.dumps()` when sorting of keys is not requested
(default).
- gh-83901: Improve `Signature.bind` error
message for missing keyword-only arguments.
- gh-95339: Update bundled pip to 22.2.1.
- gh-95045: Fix GC crash when deallocating `_lsprof.Profiler` by
untracking it before calling any callbacks. Patch by Kumar Aditya.
- gh-95231: Fail gracefully if `EPERM` or`ENOSYS` is raised when loading`crypt` methods. This
may happen when trying to load`MD5` on a Linux kernel with FIPS enabled.
- gh-95097: Fix `asyncio.run()` for`asyncio.Task` implementations without`uncancel()` method. Patch by
Kumar Aditya.
- gh-95087: Fix IndexError in parsing invalid date in the `email` module.
- gh-95199: Upgrade bundled setuptools to 63.2.0.
- gh-95194: Upgrade bundled pip to 22.2.
- gh-93899: Fix check for existence of `os.EFD_CLOEXEC` ,`os.EFD_NONBLOCK` and`os.EFD_SEMAPHORE` flags on older
kernel versions where these flags are not present. Patch by Kumar Aditya.
- gh-95166: Fix `concurrent.futures.Executor.map()` to cancel the
currently waiting on future on an error - e.g. TimeoutError or
KeyboardInterrupt.
- gh-95132: Fix a `sqlite3` regression where`*args` and`**kwds` were incorrectly relayed from`connect()` to the`Connection` factory. The regression was introduced in
3.11a1 with PR 24421 (gh-85128). Patch by Erlend E. Aasland.
- gh-93157: Fix `fileinput` module didnât support`errors` option
when`inplace` is true.
- gh-91212: Fixed flickering of the turtle window when the tracer is turned off. Patch by Shin-myoung-serp.
- gh-95077: Add deprecation warning for enum `member.member` access (e.g.`Color.RED.BLUE` ). Remove`EnumMeta.__getattr__` .
- gh-95109: Ensure that timeouts scheduled with `asyncio.Timeout` that have already expired are delivered promptly.
- gh-95105: `wsgiref.types.InputStream.__iter__()` should return`Iterator[bytes]` , not`Iterable[bytes]` . Patch by Shantanu Jain.
- gh-95066: Replaced assert with exception in `ast.parse()` , when`feature_version` has an invalid major version. Patch by Shantanu Jain.
- gh-77617: Add `sqlite3` command-line interface.
Patch by Erlend Aasland.
- gh-95005: Replace _PyAccu with _PyUnicodeWriter in JSON encoder and StringIO and remove the _PyAccu implementation.
- gh-90085: Remove `-c/--clock` and`-t/--time` CLI options of`timeit` . The options had been deprecated since Python 3.3 and the
functionality was removed in Python 3.7. Patch by Shantanu Jain.
- gh-94857: Fix refleak in `_io.TextIOWrapper.reconfigure` . Patch by Kumar
Aditya.
- gh-94821: Fix binding of unix socket to empty address on Linux to use an available address from the abstract namespace, instead of â0â.
- gh-94736: Fix crash when deallocating an instance of a subclass of `_multiprocessing.SemLock` . Patch by Kumar Aditya.
- gh-81620: Add random.binomialvariate().
- gh-74116: Allow `asyncio.StreamWriter.drain()` to be awaited
concurrently by multiple tasks. Patch by Kumar Aditya.
- gh-87822: When called with `capture_locals=True` , the`traceback` module functions swallow exceptions raised from calls to`repr()` on
local variables of frames. This is in order to prioritize the original
exception over rendering errors.  An indication of the failure is printed
in place of the missing value.  (Patch by Simon-Martin Schroeder).
- gh-88050: Fix `asyncio` subprocess transport to kill process cleanly
when process is blocked and avoid`RuntimeError` when loop is closed.
Patch by Kumar Aditya.
- gh-94637: `SSLContext.set_default_verify_paths()` now releases the GIL
around`SSL_CTX_set_default_verify_paths` call. The function call
performs I/O and CPU intensive work.
- gh-94309: Deprecate aliases `typing.Hashable` and`typing.Sized`
- gh-92546: An undocumented `python -m pprint` benchmark is moved into`pprint` suite of pyperformance. Patch by Oleg Iarygin.
- gh-94607: Fix subclassing complex generics with type variables in `typing` . Previously an error message saying```
Some type variables
... are not listed in Generic[...]
```
was shown.`typing` no longer
populates`__parameters__` with the`__parameters__` of a Python
class.
- gh-94619: Remove the long-deprecated `module_repr()` from`importlib` .
- gh-93910: The ability to access the other values of an enum on an enum (e.g. `Color.RED.BLUE` ) has been restored in order to fix a performance
regression.
- gh-93896: Fix `asyncio.run()` and`unittest.IsolatedAsyncioTestCase` to always the set event loop as
it was done in Python 3.10 and earlier. Patch by Kumar Aditya.
- gh-94343: Allow setting the attributes of `reprlib.Repr` during object
initialization
- gh-94382: Port static types of `_multiprocessing` module to heap types.
Patch by Kumar Aditya.
- gh-78724: Fix crash in `struct.Struct` when it was not completely
initialized by initializing it in`__new__()` .  Patch by Kumar
Aditya.
- gh-94510: Re-entrant calls to `sys.setprofile()` and`sys.settrace()` now raise`RuntimeError` . Patch by Pablo
Galindo.
- gh-92336: Fix bug where `linecache.getline()` fails on bad files with`UnicodeDecodeError` or`SyntaxError` . It now returns an empty
string as per the documentation.
- gh-94398: Once a `asyncio.TaskGroup` has started shutting down
(i.e., at least one task has failed and the task group has started
cancelling the remaining tasks), it should not be possible to add new
tasks to the task group.
- gh-94383: `xml.etree` : Remove the`ElementTree.Element.copy()` method of the pure Python implementation, deprecated in Python 3.10, use
the`copy.copy()` function instead. The C implementation of`xml.etree` has no`copy()` method, only a`__copy__()` method.
Patch by Victor Stinner.
- gh-94379: `zipimport` : Remove`find_loader()` and`find_module()` methods, deprecated in Python 3.10: use the`find_spec()` method
instead. See**PEP 451** for the rationale. Patch by Victor Stinner.
- gh-94352: `shlex.split()` : Passing`None` for*s* argument now
raises an exception, rather than reading`sys.stdin` . The feature
was deprecated in Python 3.9. Patch by Victor Stinner.
- gh-94318: Strip trailing spaces in `pydoc` text output.
- gh-89988: Fix memory leak in `pickle.Pickler` when looking up`dispatch_table` . Patch by Kumar Aditya.
- gh-90016: Deprecate `sqlite3` default adapters and converters. Patch by Erlend E. Aasland.
- gh-94254: Fixed types of `struct` module to be immutable. Patch by
Kumar Aditya.
- gh-93259: Now raise `ValueError` when`None` or an empty string are
passed to`Distribution.from_name` (and other callers).
- gh-74696: `shutil.make_archive()` now passes the*root_dir* argument
to custom archivers which support it.
- gh-94216: The `dis` module now has the opcodes for pseudo
instructions (those which are used by the compiler during code generation
but then removed or replaced by real opcodes before the final bytecode is
emitted).
- gh-93096: Removed undocumented `python -m codecs` . Use```
python -m
unittest test.test_codecs.EncodedFileTest
```
instead.
- gh-94207: Made `_struct.Struct` GC-tracked in order to fix a
reference leak in the`_struct` module.
- gh-93096: Removed undocumented `-t` argument of`python -m base64` .
Use```
python -m unittest
test.test_base64.LegacyBase64TestCase.test_encodebytes
```
instead.
- gh-94226: Remove the `locale.format()` function, deprecated in Python
3.7: use`locale.format_string()` instead. Patch by Victor Stinner.
- gh-94199: Remove the `ssl.match_hostname()` function. The`ssl.match_hostname()` was deprecated in Python 3.7. OpenSSL performs
hostname matching since Python 3.7, Python no longer uses the`ssl.match_hostname()` function. Patch by Victor Stinner.
- gh-94214: Document the `context` object used in the`venv.EnvBuilder` class, and add the new environmentâs library path to it.
- gh-94199: Remove the `ssl.wrap_socket()` function, deprecated in
Python 3.7: instead, create a`ssl.SSLContext` object and call its`ssl.SSLContext.wrap_socket` method. Any package that still uses`ssl.wrap_socket()` is broken and insecure. The function neither sends
a SNI TLS extension nor validates server hostname. Code is subject to**CWE 295** Improper Certificate Validation. Patch by Victor Stinner.
- gh-94199: Remove the `ssl.RAND_pseudo_bytes()` function, deprecated in
Python 3.6: use`os.urandom()` or`ssl.RAND_bytes()` instead.
Patch by Victor Stinner.
- gh-94199: `hashlib` : Remove the pure Python implementation of`hashlib.pbkdf2_hmac()` , deprecated in Python 3.10. Python 3.10 and
newer requires OpenSSL 1.1.1 (**PEP 644** ): this OpenSSL version provides a
C implementation of`pbkdf2_hmac()` which is faster. Patch by
Victor Stinner.
- gh-94196: `gzip` : Remove the`filename` attribute of`gzip.GzipFile` , deprecated since Python 2.6, use the`name` attribute instead. In write mode, the`filename` attribute added`'.gz'` file extension if it was not
present. Patch by Victor Stinner.
- gh-94182: run the `asyncio.PidfdChildWatcher` on the running loop,
this allows event loops to run subprocesses when there is no default event
loop running on the main thread
- gh-94169: Remove `io.OpenWrapper` and`_pyio.OpenWrapper` , deprecated
in Python 3.10: just use`open()` instead. The`open()` (`io.open()` ) function is a built-in function. Since Python 3.10,`_pyio.open()` is also a static method. Patch by Victor Stinner.
- gh-91742: Fix `pdb` crash after jump caused by a null pointer
dereference. Patch by Kumar Aditya.
- gh-94101: Manual instantiation of `ssl.SSLSession` objects is no
longer allowed as it lead to misconfigured instances that crashed the
interpreter when attributes where accessed on them.
- gh-84753: `inspect.iscoroutinefunction()` ,`inspect.isgeneratorfunction()` , and`inspect.isasyncgenfunction()` now properly return`True` for
duck-typed function-like objects like instances of`unittest.mock.AsyncMock` .This makes `inspect.iscoroutinefunction()` consistent with the
behavior of`asyncio.iscoroutinefunction()` .  Patch by Mehdi ABAAKOUK.
- gh-94028: Fix a regression in the `sqlite3` where statement objects
were not properly cleared and reset after use in cursor iters. The
regression was introduced by PR 27884 in Python 3.11a1. Patch by Erlend E.
Aasland.
- gh-93973: Add keyword argument `all_errors` to`asyncio.create_connection` so that multiple connection errors can be
raised as an`ExceptionGroup` .
- gh-93963: Officially deprecate from `importlib.abc` classes moved to`importlib.resources.abc` .
- gh-93858: Prevent error when activating venv in nested fish instances.
- gh-93820: Pickle `enum.Flag` by name.
- gh-93847: Fix repr of enum of generic aliases.
- gh-91404: Revert the `re` memory leak when a match is terminated by a
signal or memory allocation failure as the implemented fix caused a major
performance regression.
- gh-83499: Fix double closing of file description in `tempfile` .
- gh-93820: Fixed a regression when `copy.copy()` -ing`enum.Flag` with multiple flag members.
- gh-79512: Fixed names and `__module__` value of`weakref` classes`ReferenceType` ,`ProxyType` ,`CallableProxyType` . It makes them pickleable.
- gh-91389: Fix an issue where `dis` utilities could report missing or
incorrect position information in the presence of`CACHE` entries.
- gh-93626: Set `__future__.annotations` to have a`None` mandatoryRelease to indicate that it is currently âTBDâ.
- gh-90473: Emscripten and WASI have no home directory and cannot provide **PEP 370** user site directory.
- gh-90494: `copy.copy()` and`copy.deepcopy()` now always raise a
TypeError if`__reduce__()` returns a tuple with length 6 instead of
silently ignore the 6th item or produce incorrect result.
- gh-90549: Fix a multiprocessing bug where a global named resource (such as a semaphore) could leak when a child process is spawned (as opposed to forked).
- gh-93521: Fixed a case where dataclasses would try to add `__weakref__` into the`__slots__` for a dataclass that specified`weakref_slot=True` when it was already defined in one of its bases.
This resulted in a`TypeError` upon the new class being created.
- gh-79579: `sqlite3` now correctly detects DML queries with leading
comments. Patch by Erlend E. Aasland.
- gh-93421: Update `sqlite3.Cursor.rowcount` when a DML statement has
run to completion. This fixes the row count for SQL queries like```
UPDATE
... RETURNING
```
. Patch by Erlend E. Aasland.
- gh-93475: Expose `FICLONE` and`FICLONERANGE` constants in`fcntl` . Patch by Illia Volochii.
- gh-93370: Deprecate `version` and`version_info` .
- gh-91810: Suppress writing an XML declaration in open files in `ElementTree.write()` with`encoding='unicode'` and`xml_declaration=None` .
- gh-91162: Support splitting of unpacked arbitrary-length tuple over `TypeVar` and`TypeVarTuple` parameters. For example:
  - `A[T, *Ts][*tuple[int, ...]]` ->`A[int, *tuple[int, ...]]`
  - `A[*Ts, T][*tuple[int, ...]]` ->`A[*tuple[int, ...], int]`
- gh-93353: Fix the `importlib.resources.as_file()` context manager to
remove the temporary file if destroyed late during Python finalization:
keep a local reference to the`os.remove()` function. Patch by Victor
Stinner.
- gh-83658: Make `multiprocessing.Pool` raise an exception if`maxtasksperchild` is not`None` or a positive int.
- gh-93312: Add `os.PIDFD_NONBLOCK` flag to open a file descriptor
for a process with`os.pidfd_open()` in non-blocking mode. Patch by
Kumar Aditya.
- gh-88123: Implement `Enum.__contains__` that returns`True` or`False` to replace the deprecated behaviour that would sometimes raise a`TypeError` .
- gh-93297: Make asyncio task groups prevent child tasks from being GCed
- gh-85308: Changed `argparse.ArgumentParser` to use
filesystem encoding and error handler instead of default text
encoding to read arguments from file (e.g.`fromfile_prefix_chars` option). This change affects Windows; argument file should be encoded with
UTF-8 instead of ANSI Codepage.
- gh-93156: Accessing the `pathlib.PurePath.parents` sequence of an
absolute path using negative index values produced incorrect results.
- gh-93162: Add the ability for `logging.config.dictConfig()` to
usefully configure`QueueHandler` and`QueueListener` as a pair, and add`logging.getHandlerByName()` and`logging.getHandlerNames()` APIs
to allow access to handlers by name.
- gh-93243: The `smtpd` module was removed per the schedule in**PEP 594** .
- gh-92886: Replace `assert` statements with`raise AssertionError()` in`BaseHandler` so that the tested behaviour is maintained
running with optimizations`(-O)` .
- gh-90155: Fix broken `asyncio.Semaphore` when acquire is cancelled.
- gh-90817: The `locale.resetlocale()` function is deprecated and will
be removed in Python 3.13. Use`locale.setlocale(locale.LC_ALL, "")` instead. Patch by Victor Stinner.
- gh-91513: Added `taskName` attribute to`logging` module for use
with`asyncio` tasks.
- gh-74696: `shutil.make_archive()` no longer temporarily changes the
current working directory during creation of standard`.zip` or tar
archives.
- gh-92728: The `re.template()` function and the corresponding`re.TEMPLATE` and`re.T` flags are restored after they were
removed in 3.11.0b1, but they are now deprecated, so they might be removed
from Python 3.13.
- gh-93033: Search in some strings (platform dependent i.e [U+0xFFFF, U+0x0100] on Windows or [U+0xFFFFFFFF, U+0x00010000] on Linux 64-bit) are now up to 10 times faster.
- gh-89973: Fix `re.error` raised in`fnmatch` if the pattern
contains a character range with upper bound lower than lower bound (e.g.`[c-a]` ). Now such ranges are interpreted as empty ranges.
- gh-93044: No longer convert the database argument of `sqlite3.connect()` to bytes before passing it to the factory.
- gh-93010: In a very special case, the email package tried to append the nonexistent `InvalidHeaderError` to the defect list. It should have been`InvalidHeaderDefect` .
- gh-92986: Fix `ast.unparse()` when`ImportFrom.level` is`None`
- gh-92932: Now `dis()` and`get_instructions()` handle
operand values for instructions prefixed by`EXTENDED_ARG_QUICK` . Patch
by Sam Gross and Donghee Na.
- gh-92675: Fix `venv.ensure_directories()` to accept`pathlib.Path` arguments in addition to`str` paths. Patch
by David Foster.
- gh-87901: Removed the `encoding` argument from`os.popen()` that was
added in 3.11b1.
- gh-91922: Fix function `sqlite.connect()` and the`sqlite.Connection` constructor on non-UTF-8 locales. Also, they
now support bytes paths non-decodable with the current FS encoding.
- gh-92869: Added `c_time_t` to`ctypes` , which has the
same size as the`time_t` type in C.
- gh-92839: Fixed crash resulting from calling bisect.insort() or bisect.insort_left() with the key argument not equal to `None` .
- gh-90473: `subprocess` now fails early on Emscripten and WASI
platforms to work around missing`os.pipe()` on WASI.
- gh-89325: Removed many old deprecated `unittest` features:`TestCase` method aliases, undocumented and broken`TestCase` method`assertDictContainsSubset` ,
undocumented`TestLoader.loadTestsFromModule` parameter*use_load_tests* , and
an underscored alias of the`TextTestResult` class.
- gh-92734: Allow multi-element reprs emitted by `reprlib` to be
pretty-printed using configurable indentation.
- gh-92671: Fixed `ast.unparse()` for empty tuples in the assignment
target context.
- gh-91581: `utcfromtimestamp()` no longer attempts
to resolve`fold` in the pure Python implementation, since the fold is
never 1 in UTC. In addition to being slightly faster in the common case,
this also prevents some errors when the timestamp is close to`datetime.min` .  Patch by Paul Ganssle.
- gh-86388: Removed randrange() functionality deprecated since Python 3.10. Formerly, randrange(10.0) losslessly converted to randrange(10). Now, it raises a TypeError. Also, the exception raised for non-integral values such as randrange(10.5) or randrange(â10â) has been changed from ValueError to TypeError.
- gh-90385: Add `pathlib.Path.walk()` as an alternative to`os.walk()` .
- gh-92550: Fix `pathlib.Path.rglob()` for empty pattern.
- gh-92591: Allow `logging` filters to return a`logging.LogRecord` instance so that filters attached to`logging.Handler` s can enrich records without side effects on
other handlers.
- gh-92445: Fix a bug in `argparse` where`nargs="*"` would raise an
error instead of returning an empty list when 0 arguments were supplied if
choice was also defined in`parser.add_argument` .
- gh-92547: Remove undocumented `sqlite3` features deprecated in Python
3.10:
  - `sqlite3.enable_shared_cache()`
  - `sqlite3.OptimizedUnicode`
 Patch by Erlend E. Aasland.
- gh-92530: Fix an issue that occurred after interrupting `threading.Condition.notify()` .
- gh-92531: The statistics.median_grouped() function now always return a float. Formerly, it did not convert the input type when for sequences of length one.
- gh-84131: The `pathlib.Path` deprecated method`link_to` has been
removed. Use 3.10âs`hardlink_to()` method instead as
its semantics are consistent with that of`symlink_to()` .
- gh-89336: Removed `configparser` module APIs: the`SafeConfigParser` class alias, the`ParsingError.filename` property
and parameter, and the`ConfigParser.readfp` method, all of which were
deprecated since Python 3.2.
- gh-92391: Add `__class_getitem__()` to`csv.DictReader` and`csv.DictWriter` , allowing them to be
parameterized at runtime. Patch by Marc Mueller.
- gh-91968: Add `SO_RTABLE` and`SO_USER_COOKIE` constants to`socket` .
- gh-91810: `ElementTree` method`write()` and function`tostring()` now use the text fileâs encoding
(âUTF-8â if not available) instead of locale encoding in XML declaration
when`encoding="unicode"` is specified.
- gh-81790: `os.path.splitdrive()` now understands DOS device paths with
UNC links (beginning`\\?\UNC\` ). Contributed by Barney Gale.
- gh-91760: Apply more strict rules for numerical group references and group names in regular expressions. Only sequence of ASCII digits is now accepted as a numerical reference. The group name in bytes patterns and replacement strings can now only contain ASCII letters and digits and underscore.
- gh-90622: Worker processes for `concurrent.futures.ProcessPoolExecutor` are no longer spawned on
demand (a feature added in 3.9) when the multiprocessing context start
method is`"fork"` as that can lead to deadlocks in the child processes
due to a fork happening while threads are running.
- gh-91577: Move imports in `SharedMemory` methods
to module level so that they can be executed late in python finalization.
- gh-91581: Remove an unhandled error case in the C implementation of calls to `datetime.fromtimestamp` with
no time zone (i.e. getting a local time from an epoch timestamp). This
should have no user-facing effect other than giving a possibly more
accurate error message when called with timestamps that fall on
10000-01-01 in the local time. Patch by Paul Ganssle.
- gh-91539: Improve performance of `urllib.request.getproxies_environment` when there are many environment variables
- gh-91524: Speed up the regular expression substitution (functions `re.sub()` and`re.subn()` and corresponding`re.Pattern` methods) for replacement strings containing group references by 2â3
times.
- gh-91447: Fix findtext in the xml module to only give an empty string when the text attribute is set to `None` .
- gh-91456: Deprecate current default auto() behavior: In 3.13 the default will be for for auto() to always return the largest member value incremented by 1, and to raise if incompatible value types are used.
- bpo-47231: Fixed an issue with inconsistent trailing slashes in tarfile longname directories.
- bpo-39064: `zipfile.ZipFile` now raises`zipfile.BadZipFile` instead of`ValueError` when reading a corrupt zip file in which the
central directory offset is negative.
- bpo-41287: Fix handling of the `doc` argument in subclasses of`property()` .
- gh-90005: `ctypes` dependency`libffi` is now detected with`pkg-config` .
- bpo-32547: The constructors for `DictWriter` and`DictReader` now coerce the`fieldnames` argument to a`list` if it is an iterator.
- bpo-35540: Fix `dataclasses.asdict()` crash when`collections.defaultdict` is present in the attributes.
- bpo-47063: Add an index_pages parameter to support using non-default index page names.
- bpo-47025: Drop support for `bytes` on`sys.path` .
- bpo-46951: Order the contents of zipapp archives, to make builds more reproducible.
- bpo-42777: Implement `pathlib.Path.is_mount()` for Windows paths.
- bpo-46755: In `QueueHandler` , clear`stack_info` from`LogRecord` to prevent stack trace from being written twice.
- bpo-45393: Fix the formatting for `await x` and`not x` in the
operator precedence table when using the`help()` system.
- bpo-46642: Improve error message when trying to subclass an instance of `typing.TypeVar` ,`typing.ParamSpec` ,`typing.TypeVarTuple` , etc. Based on patch by Gregory Beauregard.
- bpo-46364: Restrict use of sockets instead of pipes for stdin of subprocesses created by `asyncio` to AIX platform only.
- bpo-28249: Set `doctest.DocTest.lineno` to`None` when an object
does not have`__doc__` .
- bpo-46197: Fix `ensurepip` environment isolation for subprocess
running`pip` .
- bpo-45924: Fix `asyncio` incorrect traceback when futureâs exception
is raised multiple times. Patch by Kumar Aditya.
- bpo-45046: Add support of context managers in `unittest` : methods`enterContext()` and`enterClassContext()` of class`TestCase` , method`enterAsyncContext()` of class`IsolatedAsyncioTestCase` and function`unittest.enterModuleContext()` .
- bpo-44173: Enable fast seeking of uncompressed unencrypted `zipfile.ZipExtFile`
- bpo-42627: Fix incorrect parsing of Windows registry proxy settings
- bpo-42047: Add `threading.get_native_id()` support for DragonFly BSD.
Patch by David Carlier.
- bpo-14243: The `tempfile.NamedTemporaryFile` function has a new
optional parameter*delete_on_close*
- bpo-41246: Give the same callback function for when the overlapped operation is done to the functions `recv` ,`recv_into` ,`recvfrom` ,`sendto` ,`send` and`sendfile` inside`IocpProactor` .
- bpo-39264: Fixed `collections.UserDict.get()` to not call`__missing__()` when a value is not found. This matches the
behavior of`dict` . Patch by Bar Harel.
- bpo-38693: `importlib` now uses f-strings internally instead of`str.format` .
- bpo-38267: Add *timeout* parameter to`asyncio.loop.shutdown_default_executor()` . The default value is`None` , which means the executor will be given an unlimited amount of
time. When called from`asyncio.Runner` or`asyncio.run()` , the
default timeout is 5 minutes.
- bpo-34828: `sqlite3.Connection.iterdump()` now handles databases that
use`AUTOINCREMENT` in one or more tables.
- bpo-32990: Support reading wave files with the `WAVE_FORMAT_EXTENSIBLE` format in the`wave` module.
- bpo-26253: Allow adjustable compression level for tarfile streams in `tarfile.open()` .

### DocumentationÂ¶

- gh-85525: Remove extra row
- gh-86404: Deprecated tools `make suspicious` and`rstlint.py` are now
removed. They have been replaced by sphinx-lint.
- gh-97741: Fix `!` in c domain ref target syntax via a`conf.py` patch,
so it works as intended to disable ref target resolution.
- gh-96432: Fraction literals now support whitespace around the forward slash, `Fraction('2 / 3')` .
- gh-96098: Improve discoverability of the higher level concurrent.futures module by providing clearer links from the lower level threading and multiprocessing modules.
- gh-95957: Whatâs New 3.11 now has instructions for how to provide compiler and linker flags for Tcl/Tk and OpenSSL on RHEL 7 and CentOS 7.
- gh-95588: Clarified the conflicting advice given in the `ast` documentation about`ast.literal_eval()` being âsafeâ for use on
untrusted input while at the same time warning that it can crash the
process. The latter statement is true and is deemed unfixable without a
large amount of work unsuitable for a bugfix. So we keep the warning and
no longer claim that`literal_eval` is safe.
- gh-91207: Fix stylesheet not working in Windows CHM htmlhelp docs and add warning that they are deprecated. Contributed by C.A.M. Gerlach.
- gh-95454: Replaced incorrectly written true/false values in documentation. Patch by Robert OâShea
- gh-95451: Update library documentation with availability information on WebAssembly platforms `wasm32-emscripten` and`wasm32-wasi` .
- gh-95415: Use consistent syntax for platform availability. The directive now supports a content body and emits a warning when it encounters an unknown platform.
- gh-94321: Document the **PEP 246** style protocol type`sqlite3.PrepareProtocol` .
- gh-86128: Document a limitation in ThreadPoolExecutor where its exit handler is executed before any handlers in atexit.
- gh-61162: Clarify `sqlite3` behavior when
How to use the connection context manager.
- gh-87260: Align `sqlite3` argument specs with the actual
implementation.
- gh-86986: The minimum Sphinx version required to build the documentation is now 3.2.
- gh-88831: Augmented documentation of asyncio.create_task(). Clarified the need to keep strong references to tasks and added a code snippet detailing how to do this.
- gh-86438: Clarify that `-W` and`PYTHONWARNINGS` are
matched literally and case-insensitively, rather than as regular
expressions, in`warnings` .
- gh-93031: Update tutorial introduction output to use 3.10+ SyntaxError invalid range.
- gh-92240: Added release dates for âWhatâs New in Python 3.Xâ for 3.0, 3.1, 3.2, 3.8 and 3.10
- bpo-47161: Document that `pathlib.PurePath` does not collapse
initial double slashes because they denote UNC paths.
- bpo-40838: Document that `inspect.getdoc()` ,`inspect.getmodule()` , and`inspect.getsourcefile()` might return`None` .
- bpo-43689: The `Differ` documentation now also mentions other whitespace
characters, which make it harder to understand the diff output.
- bpo-38056: Overhaul the Error Handlers documentation in `codecs` .
- bpo-13553: Document tkinter.Tk args.

### TestsÂ¶

- gh-95027: On Windows, when the Python test suite is run with the `-jN` option, the ANSI code page is now used as the encoding for the stdout
temporary file, rather than using UTF-8 which can lead to decoding errors.
Patch by Victor Stinner.
- gh-96624: Fixed the failure of repeated runs of `test.test_unittest` caused by side effects in`test_dotted_but_module_not_loaded` .
- gh-95243: Mitigate the inherent race condition from using find_unused_port() in testSockName() by trying to find an unused port a few times before failing. Patch by Ross Burton.
- gh-95573: Lib/test/test_asyncio/test_ssl.py exposed a bug in the macOS kernel where intense concurrent load on non-blocking sockets occasionally causes `errno.ENOBUFS` (âNo buffer space availableâ)
to be emitted. FB11063974 filed with Apple, in the mean time as a
workaround buffer size used in tests on macOS is decreased to avoid
intermittent failures.  Patch by Fantix King.
- gh-95280: Fix problem with `test_ssl``test_get_ciphers` on systems
that require perfect forward secrecy (PFS) ciphers.
- gh-95212: Make multiprocessing test case `test_shared_memory_recreate` parallel-safe.
- gh-95218: Move tests for importlib.resources into test_importlib.resources.
- gh-93963: Updated tests to use preferred location for `importlib.resources` ABCs.
- gh-94675: Add a regression test for `re` exponentional slowdown when
using rjsmin.
- gh-91330: Added more tests for `dataclasses` to cover behavior with
data descriptor-based fields.
- gh-94208: `test_ssl` is now checking for supported TLS version and
protocols in more tests.
- gh-94315: Tests now check for DAC override capability instead of relying on `os.geteuid()` .
- gh-54781: Rename test_tk to test_tkinter, and rename test_ttk_guionly to test_ttk. Patch by Victor Stinner.
- gh-93839: Move `Lib/ctypes/test/` to`Lib/test/test_ctypes/` . Patch by
Victor Stinner.
- gh-93951: In test_bdb.StateTestCase.test_skip, avoid including auxiliary importers.
- gh-93957: Provide nicer error reporting from subprocesses in test_venv.EnsurePipTest.test_with_pip.
- gh-93884: Add test cases for `PyNumber_ToBase()` that take a large
number or a non-int object as parameter.
- gh-93852: test_asyncio, test_logging, test_socket and test_socketserver now create AF_UNIX domains in the current directory to no longer fail with `OSError("AF_UNIX path too long")` if the temporary directory (the`TMPDIR` environment variable) is too long. Patch by Victor
Stinner.
- gh-93353: regrtest now checks if a test leaks temporary files or directories if run with -jN option. Patch by Victor Stinner.
- gh-84461: `run_tests.py` now handles cross compiling env vars correctly
and pass`HOSTRUNNER` to regression tests.
- gh-93616: `test_modulefinder` now creates a temporary directory in`ModuleFinderTest.setUp()` instead of module scope.
- gh-93575: Fix issue with test_unicode test_raiseMemError. The test case now use `test.support.calcobjsize` to calculate size of PyUnicode
structs.`sys.getsizeof()` may return different size when string has
UTF-8 memory.
- gh-90473: WASI does not have a `chmod(2)` syscall.`os.chmod()` is
now a dummy function on WASI. Skip all tests that depend on working`os.chmod()` .
- gh-90473: Skip tests on WASI that require symlinks with absolute paths.
- gh-57539: Increase calendar test coverage for `calendar.LocaleTextCalendar.formatweekday()` .
- gh-90473: Skip symlink tests on WASI. wasmtime uses `openat2(2)` with`RESOLVE_BENEATH` flag, which prevents symlinks with absolute paths.
- gh-89858: Fix `test_embed` for out-of-tree builds. Patch by Kumar
Aditya.
- gh-92886: Fixing tests that fail when running with optimizations ( `-O` )
in`test_imaplib.py` .
- gh-92886: Fixing tests that fail when running with optimizations ( `-O` )
in`test_zipimport.py`
- gh-92886: Fixing tests that fail when running with optimizations ( `-O` )
in`test_py_compile.py`
- gh-92886: Fixing tests that fail when running with optimizations ( `-O` )
in`test_sys_settrace.py` .
- gh-92886: Fixing tests that fail when running with optimizations ( `-O` )
in`_test_multiprocessing.py`
- gh-92670: Skip `test_shutil.TestCopy.test_copyfile_nonexistent_dir` test
on AIX as the test uses a trailing slash to force the OS consider the path
as a directory, but on AIX the trailing slash has no effect and is
considered as a file.
- gh-92514: Remove unused `test.support.BasicTestRunner` . Patch by Jelle
Zijlstra.
- bpo-47016: Create a GitHub Actions workflow for verifying bundled pip and setuptools. Patch by Illia Volochii and Adam Turner.

### BuildÂ¶

- gh-96761: Fix the build process of clang compiler for **_bootstrap_python** if LTO optimization is applied. Patch by
Matthias GÃ¶rgens and Donghee Na.
- gh-96883: `wasm32-emscripten` builds for browsers now include`concurrent.futures` for`asyncio` and`unittest.mock` .
- gh-85936: CPython now uses the ThinLTO option as the default policy if the Clang compiler accepts the flag. Patch by Donghee Na.
- gh-96729: Ensure that Windows releases built with `Tools\msi\buildrelease.bat` are upgradable to and from official Python
releases.
- gh-96269: Shared module targets now depend on new `MODULE_DEPS` variable, which includes`EXPORTSYMS` . This fixes a build order issue on
unsupported AIX platform.
- gh-84461: `wasm32-emscripten` platform no longer builds`resource` module,`getresuid()` ,`getresgid()` , and their setters.
The APIs are stubs and not functional.
- gh-95973: Add a new `--with-dsymutil` configure option to link debug
information in macOS. Patch by Pablo Galindo.
- gh-90536: Use the BOLT post-link optimizer to improve performance, particularly on medium-to-large applications.
- gh-93744: Remove the `configure --with-cxx-main` build option: it didnât
work for many years. Remove the`MAINCC` variable from`configure` and`Makefile` . Patch by Victor Stinner.
- gh-94801: Fix a regression in `configure` script that caused some header
checks to ignore custom`CPPFLAGS` . The regression was introduced in
gh-94802.
- gh-95145: wasm32-wasi builds no longer depend on WASIXâs pthread stubs. Python now has its own stubbed pthread API.
- gh-95174: Python now detects missing `dup` function in WASI and works
around some missing`errno` ,`select` , and`socket` constants.
- gh-95174: Python now skips missing `socket` functions and methods on
WASI. WASI can only create sockets from existing fd / accept and has no
netdb.
- gh-95085: Platforms `wasm32-unknown-emscripten` and`wasm32-unknown-wasi` have been promoted to**PEP 11** tier 3 platform
support.
- gh-94847: Fixed `_decimal` module build issue on GCC when compiling with
LTO and pydebug. Debug builds no longer force inlining of functions.
- gh-94841: Fix the possible performance regression of `PyObject_Free()` compiled with MSVC version 1932.
- gh-94801: `configure` now uses custom flags like`ZLIB_CFLAGS` and`ZLIB_LIBS` when searching for headers and libraries.
- gh-94773: `deepfreeze.py` now supports code object with frozensets that
contain incompatible, unsortable types.
- gh-94682: Build and test with OpenSSL 1.1.1q
- gh-90005: Dependencies of `readline` and`curses` module are now
detected in`configure` script with`pkg-config` . Only`ncurses` /`ncursesw` are detected automatically. The old`curses` library is not
configured automatically. Workaround for missing`termcap` or`tinfo` library has been removed.
- gh-90005: Fix building `_ctypes` extension without`pkg-config` .
- gh-90005: `_dbm` module dependencies are now detected by configure.
- gh-94404: `makesetup` now works around an issue with sed on macOS and
uses correct CFLAGS for object files that end up in a shared extension.
Module CFLAGS are used before PY_STDMODULE_CFLAGS to avoid clashes with
system headers.
- gh-93939: C extension modules are now built by `configure` and`make` instead of`distutils` and`setup.py` .
- gh-93939: The `2to3` ,`idle` , and`pydoc` scripts are now generated
and installed by`Makefile` instead of`setup.py` .
- gh-94280: Updated pegen regeneration script on Windows to find and use Python 3.9 or higher. Prior to this, pegen regeneration already required 3.9 or higher, but the script may have used lower versions of Python.
- gh-93584: Address race condition in `Makefile` when installing a PGO
build. All`test` and`install` targets now depend on`all` target.
- gh-93491: `configure` now detects and reports**PEP 11** support tiers.
- gh-69093: Fix `Modules/Setup.stdlib.in` rule for`_sqlite3` extension.
- gh-93207: `va_start()` with two parameters, like```
va_start(args,
format),
```
is now required to build Python.`va_start()` is no longer
called with a single parameter. Patch by Kumar Aditya.
- gh-93202: Python now always use the `%zu` and`%zd` printf formats to
format a`size_t` or`Py_ssize_t` number. Building Python 3.12
requires a C11 compiler, so these printf formats are now always supported.
Patch by Victor Stinner.
- gh-90473: Disable pymalloc and increase stack size on `wasm32-wasi` .
- bpo-34449: Drop invalid compiler switch `-fPIC` for HP aCC on HP-UX.
Patch by Michael Osipov.

### WindowsÂ¶

- gh-98360: Fixes `multiprocessing` spawning child processes on Windows
from a virtual environment to ensure that child processes that also use`multiprocessing` to spawn more children will recognize that they are
in a virtual environment.
- gh-98414: Fix `py.exe` launcher handling of`-V:`*<company>* /
option when default preferences have been set in environment variables or
configuration files.
- gh-97728: Fix possible crashes caused by the use of uninitialized variables when pass invalid arguments in `os.system()` on Windows and
in Windows-specific modules (like`winreg` ).
- gh-90989: Made Python install manager install per-user by default (unless an all users install already exists), and clarify some text in the installer.
- gh-97649: The `Tools` directory is no longer installed on Windows
- gh-96965: Update libffi to 3.4.3
- gh-96577: Fixes a potential buffer overrun in `msilib` .
- gh-96559: Fixes the Windows launcher not using the compatible interpretation of default tags found in configuration files when no tag was passed to the command.
- gh-94781: Fix `pcbuild.proj` to clean previous instances of output
files in`Python\deepfreeze` and`Python\frozen_modules` directories
on Windows. Patch by Charlie Zhao.
- gh-89545: Updates `platform` code getting the Windows version to use
native Windows Management Instrumentation (WMI) queries to determine OS
version, type, and architecture.
- gh-95733: Make certain requirements of the Windows Store package optional to allow installing on earlier updates of Windows.
- gh-95656: Enable the `enable_load_extension()``sqlite3` API.
- gh-95587: Fixes some issues where the Windows installer would incorrectly detect certain features of an existing install when upgrading.
- gh-94399: Restores the behaviour of Python install manager for `/usr/bin/env` shebang lines, which will now search`PATH` for an executable
matching the given command. If none is found, the usual search process is
used.
- gh-95445: Fixes the unsuccessful removal of the HTML document directory when uninstalling with Windows msi.
- gh-95359: Fix Python install manager handling of `py.ini` commands (it was
incorrectly expecting a`py_` prefix on keys) and crashes when reading
per-user configuration file.
- gh-95285: Fix Python install manager handling of command lines where it is only passed a short executable name.
- gh-90844: Allow virtual environments to correctly launch when they have spaces in the path.
- gh-94772: Fix incorrect handling of shebang lines in py.exe launcher
- gh-94018: `zipfile` will now remove trailing spaces from path
components when extracting files on Windows.
- gh-93824: Drag and drop of files onto Python files in Windows Explorer has been enabled for Windows ARM64.
- gh-43414: `os.get_terminal_size()` now attempts to read the size from
any provided handle, rather than only supporting file descriptors 0, 1 and
2.
- gh-92817: Ensures that `py.exe` will prefer an active virtual
environment over default tags specified with environment variables or
through a`py.ini` file.
- gh-92984: Explicitly disable incremental linking for non-Debug builds
- gh-92841: `asyncio` no longer throws```
RuntimeError: Event loop is
closed
```
on interpreter exit after asynchronous socket activity. Patch by
Oleg Iarygin.
- bpo-46907: Update Windows installer to use SQLite 3.38.4.
- gh-91061: Accept os.PathLike for the argument to winsound.PlaySound
- bpo-42658: Support native Windows case-insensitive path comparisons by using `LCMapStringEx` instead of`str.lower()` in`ntpath.normcase()` . Add`LCMapStringEx` to the`_winapi` module.
- bpo-38704: Prevent installation on unsupported Windows versions.

### macOSÂ¶

- gh-97897: The macOS 13 SDK includes support for the `mkfifoat` and`mknodat` system calls. Using the`dir_fd` option with either`os.mkfifo()` or`os.mknod()` could result in a segfault if
cpython is built with the macOS 13 SDK but run on an earlier version of
macOS. Prevent this by adding runtime support for detection of these
system calls (âweaklinkingâ) as is done for other newer syscalls on macOS.

### IDLEÂ¶

- gh-97527: Fix a bug in the previous bugfix that caused IDLE to not start when run with 3.10.8, 3.12.0a1, and at least Microsoft Python 3.10.2288.0 installed without the Lib/test package. 3.11.0 was never affected.
- gh-65802: Document handling of extensions in Save As dialogs.
- gh-95191: Include prompts when saving Shell (interactive input and output).
- gh-95511: Fix the Shell context menu copy-with-prompts bug of copying an extra line when one selects whole lines.
- gh-95471: In the Edit menu, move `Select All` and add a new separator.
- gh-95411: Enable using IDLEâs module browser with .pyw files.
- gh-89610: Add .pyi as a recognized extension for IDLE on macOS. This allows opening stub files by double clicking on them in the Finder.

### Tools/DemosÂ¶

- gh-68686: Remove ptags and eptags scripts.
- gh-97681: Remove the `Tools/demo/` directory which contained old demo
scripts. A copy can be found in the old-demos project.  Patch by Victor Stinner.
- gh-97669: Remove outdated example scripts of the `Tools/scripts/` directory. A copy can be found in the old-demos project. Patch by Victor Stinner.
- gh-95853: The `wasm_build.py` script now pre-builds Emscripten ports,
checks for broken EMSDK versions, and warns about pkg-config env vars.
- gh-95853: The new tool `Tools/wasm/wasm_builder.py` automates configure,
compile, and test steps for building CPython on WebAssembly platforms.
- gh-95731: Fix handling of module docstrings in `Tools/i18n/pygettext.py` .
- gh-93939: Add script `Tools/scripts/check_modules.py` to check and
validate builtin and shared extension modules. The script also handles`Modules/Setup` and will eventually replace`setup.py` .
- gh-94538: Fix Argument Clinic output to custom file destinations. Patch by Erlend E. Aasland.
- gh-94430: Allow parameters named `module` and`self` with custom C
names in Argument Clinic. Patch by Erlend E. Aasland
- gh-86087: The `Tools/scripts/parseentities.py` script used to parse
HTML4 entities has been removed.

### C APIÂ¶

- gh-98393: The `PyUnicode_FSDecoder()` function no longer accepts
bytes-like paths, like`bytearray` and`memoryview` types:
only the exact`bytes` type is accepted for bytes strings. Patch by
Victor Stinner.
- gh-91051: Add `PyType_Watch()` and related APIs to allow callbacks
on`PyType_Modified()` .
- gh-95756: Lazily create and cache `co_` attributes for better
performance for code getters.
- gh-96512: Configuration for the integer string conversion length limitation now lives in the PyConfig C API struct.
- gh-95589: Extensions classes that set `tp_dictoffset` and`tp_weaklistoffset` lose the support for multiple inheritance, but are
now safe. Extension classes should use`Py_TPFLAGS_MANAGED_DICT` and`Py_TPFLAGS_MANAGED_WEAKREF` instead.
- gh-95781: An unrecognized format character in `PyUnicode_FromFormat()` and`PyUnicode_FromFormatV()` now
sets a`SystemError` . In previous versions it caused all the rest of
the format string to be copied as-is to the result string, and any extra
arguments discarded.
- gh-92678: Restore the 3.10 behavior for multiple inheritance of C extension classes that store their dictionary at the end of the struct.
- gh-92678: Support C extensions using managed dictionaries by setting the `Py_TPFLAGS_MANAGED_DICT` flag.
- gh-93274: API for implementing vectorcall ( `Py_TPFLAGS_HAVE_VECTORCALL` ,`PyVectorcall_NARGS()` and`PyVectorcall_Call()` ) was added to the limited API and stable ABI.
- gh-95504: Fix sign placement when specifying width or precision in `PyUnicode_FromFormat()` and`PyUnicode_FromFormatV()` . Patch
by Philip Georgi.
- gh-93012: The `Py_TPFLAGS_HAVE_VECTORCALL` flag is now removed
from a class when the classâs`__call__()` method is
reassigned. This makes vectorcall safe to use with mutable types (i.e.
heap types without the`immutable` flag). Mutable types that do not override`tp_call` now inherit the`Py_TPFLAGS_HAVE_VECTORCALL` flag.
- gh-95388: Creating `immutable types` with mutable bases is deprecated and is planned to be disabled in Python
3.14.
- gh-92678: Adds unstable C-API functions `_PyObject_VisitManagedDict` and`_PyObject_ClearManagedDict` to allow C extensions to allow the VM to
manage their objectâs dictionaries.
- gh-94936: Added `PyCode_GetVarnames()` ,`PyCode_GetCellvars()` and`PyCode_GetFreevars()` for accessing`co_varnames` ,`co_cellvars` and`co_freevars` respectively via the C API.
- gh-94930: Fix `SystemError` raised when`PyArg_ParseTupleAndKeywords()` is used with`#` in`(...)` but
without`PY_SSIZE_T_CLEAN` defined.
- gh-94731: Python again uses C-style casts for most casting operations when compiled with C++. This may trigger compiler warnings, if they are enabled with e.g. `-Wold-style-cast` or`-Wzero-as-null-pointer-constant` options for`g++` .
- gh-93937: The following frame functions and type are now directly available with `#include <Python.h>` , itâs no longer needed to add`#include <frameobject.h>` :Patch by Victor Stinner.
- gh-91321: Fix the compatibility of the Python C API with C++ older than C++11. Patch by Victor Stinner.
- gh-91731: Avoid defining the `static_assert` when compiling with C++ 11,
where this is a keyword and redefining it can lead to undefined behavior.
Patch by Pablo Galindo
- gh-89546: `PyType_FromMetaclass()` (and other`PyType_From*` functions) now check that offsets and the base classâs`tp_basicsize` fit in the new classâs`tp_basicsize` .
- gh-93503: Add two new public functions to the public C-API, `PyEval_SetProfileAllThreads()` and`PyEval_SetTraceAllThreads()` , that allow to set tracing and
profiling functions in all running threads in addition to the calling one.
Also, two analogous functions to the`threading` module
(`threading.setprofile_all_threads()` and`threading.settrace_all_threads()` ) that allow to do the same from
Python. Patch by Pablo Galindo
- gh-93442: Add C++ overloads for _Py_CAST_impl() to handle 0/NULL. This will allow C++ extensions that pass 0 or NULL to macros using _Py_CAST() to continue to compile.
- gh-93466: Slot IDs in PyType_Spec may not be repeated. The documentation was updated to mention this. For some cases of repeated slots, PyType_FromSpec and related functions will now raise an exception.
- gh-92898: Fix C++ compiler warnings when casting function arguments to `PyObject*` . Patch by Serge Guelton.
- gh-93103: Deprecate global configuration variables, like `Py_IgnoreEnvironmentFlag` , in the documentation: the`Py_InitializeFromConfig()` API should be instead. Patch by Victor
Stinner.
- gh-77782: Deprecate global configuration variable like `Py_IgnoreEnvironmentFlag` : the`Py_InitializeFromConfig()` API should be instead. Patch by Victor Stinner.
- gh-92913: Ensures changes to `PyConfig.module_search_paths` are
ignored unless`PyConfig.module_search_paths_set` is set
- gh-92781: Avoid mixing declarations and code in the C API to fix the compiler warning: âISO C90 forbids mixed declarations and codeâ [-Werror=declaration-after-statement]. Patch by Victor Stinner.
- gh-92651: Remove the `token.h` header file. There was never any public
tokenizer C API. The`token.h` header file was only designed to be used
by Python internals. Patch by Victor Stinner.
- gh-92536: Remove legacy Unicode APIs based on `Py_UNICODE*` .
- gh-85858: Remove the `PyUnicode_InternImmortal()` function and the`SSTATE_INTERNED_IMMORTAL` macro. Patch by Victor Stinner.
- gh-92193: Add new function `PyFunction_SetVectorcall()` to the C API
which sets the vectorcall field of a given`PyFunctionObject` .Warning: extensions using this API must preserve the behavior of the unaltered function!
- gh-59121: Fixed an assert that prevented `PyRun_InteractiveOne` from
providing tracebacks when parsing from the provided FD.
- bpo-45383: The `PyType_FromSpec()` API will now find and use a
metaclass based on the provided bases. An error will be raised if there is
a metaclass conflict.

## Python 3.11.0 beta 1Â¶

*Release date: 2022-05-06*

### SecurityÂ¶

- gh-57684: Add the `-P` command line option and the`PYTHONSAFEPATH` environment variable to not prepend a potentially
unsafe path to`sys.path` . Patch by Victor Stinner.

### Core and BuiltinsÂ¶

- gh-89519: Chaining classmethod descriptors (introduced in bpo-19072) is deprecated. It can no longer be used to wrap other descriptors such as property(). The core design of this feature was flawed, and it caused a number of downstream problems.
- gh-92345: `pymain_run_python()` now imports`readline` and`rlcompleter` before sys.path is extended to include the current working
directory of an interactive interpreter. Non-interactive interpreters are
not affected.
- bpo-43857: Improve the `AttributeError` message when deleting a
missing attribute. Patch by GÃ©ry Ogam.
- gh-92245: Make sure that PEP 523 is respected in all cases. In 3.11a7, specialization may have prevented Python-to-Python calls respecting PEP 523.
- gh-92203: Add a closure keyword-only parameter to `exec()` . It can
only be specified when exec-ing a code object that uses free variables.
When specified, it must be a tuple, with exactly the number of cell
variables referenced by the code object. closure has a default value of`None` , and it must be`None` if the code object doesnât refer to any
free variables.
- gh-91173: Disable frozen modules in debug builds. Patch by Kumar Aditya.
- gh-92114: Improve error message when subscript a type with `__class_getitem__` set to`None` .
- gh-92112: Fix crash triggered by an evil custom `mro()` on a metaclass.
- gh-92063: The `PRECALL_METHOD_DESCRIPTOR_FAST_WITH_KEYWORDS` instruction
now ensures methods are called only on objects of the correct type.
- gh-92031: Deoptimize statically allocated code objects during `Py_FINALIZE()` so that future`_PyCode_Quicken` calls always start
with unquickened code.
- gh-92036: Fix a crash in subinterpreters related to the garbage collector. When a subinterpreter is deleted, untrack all objects tracked by its GC. To prevent a crash in deallocator functions expecting objects to be tracked by the GC, leak a strong reference to these objects on purpose, so they are never deleted and their deallocator functions are not called. Patch by Victor Stinner.
- gh-92032: The interpreter can now autocomplete soft keywords, as of now `match` ,`case` , and`_` (wildcard pattern) from**PEP 634** .
- gh-87999: The warning emitted by the Python parser for a numeric literal immediately followed by keyword has been changed from deprecation warning to syntax warning.
- gh-91869: Fix an issue where specialized opcodes with extended arguments could produce incorrect tracing output or lead to assertion failures.
- gh-91603: Speed up `types.UnionType` instantiation. Based on patch
provided by Yurii Karabas.
- gh-89373: If Python is built in debug mode, Python now ensures that deallocator functions leave the current exception unchanged. Patch by Victor Stinner.
- gh-91632: Fix a minor memory leak at exit: release the memory of the `generic_alias_iterator` type. Patch by Donghee Na.
- gh-81548: Octal escapes with value larger than `0o377` now produce a`DeprecationWarning` . In a future Python version they will be a`SyntaxWarning` and eventually a`SyntaxError` .
- bpo-43950: Use a single compact table for line starts, ends and column offsets. Reduces memory consumption for location info by half
- gh-91102: Use Argument Clinic for `EncodingMap` .  Patch by Oleg
Iarygin.
- gh-91636: Fixed a crash in a garbage-collection edge-case, in which a `PyFunction_Type.tp_clear` function could leave a python function object
in an inconsistent state.
- gh-91603: Speed up `isinstance()` and`issubclass()` checks for`types.UnionType` . Patch by Yurii Karabas.
- gh-91625: Fixed a bug in which adaptive opcodes ignored any preceding `EXTENDED_ARG` s on specialization failure.
- gh-78607: The LLTRACE special build now looks for the name `__lltrace__` defined in module globals, rather than the name`__ltrace__` , which had
been introduced as a typo.
- gh-91576: Speed up iteration of ascii strings by 50%. Patch by Kumar Aditya.
- gh-89279: Improve interpreter performance on Windows by inlining a few specific macros.
- gh-91502: Add a new `_PyFrame_IsEntryFrame()` API function, to
check if a`PyFrameObject` is an entry frame. Patch by Pablo
Galindo.
- gh-91266: Refactor the `bytearray` strip methods`strip` ,`lstrip` and`rstrip` to use a common implementation.
- gh-91479: Replaced the `__note__` field of`BaseException` (added
in an earlier version of 3.11) with the final design of**PEP 678** .
Namely,`BaseException` gets an`add_note()` method, and its`__notes__` field is created when necessary.
- gh-46055: Speed up right shift of negative integers, by removing unnecessary creation of temporaries. Original patch by Xinhang Xu, reworked by Mark Dickinson.
- gh-91462: Make the interpreterâs low-level tracing (lltrace) feature output more readable by displaying opcode names (rather than just numbers), and by displaying stack contents before each opcode.
- gh-89455: Fixed an uninitialized bool value in the traceback printing code path that was introduced by the initial bpo-45292 exception groups work.
- gh-91421: Fix a potential integer overflow in _Py_DecodeUTF8Ex.
- gh-91428: Add `static const char *const _PyOpcode_OpName[256] = {...};` to`opcode.h` for debug builds to assist in debugging the Python
interpreter. It is now more convenient to make various forms of debugging
output more human-readable by including opcode names rather than just the
corresponding decimal digits.
- bpo-47120: Make `POP_JUMP_IF_TRUE` ,`POP_JUMP_IF_FALSE` ,`POP_JUMP_IF_NONE` and`POP_JUMP_IF_NOT_NONE` virtual,
mapping to new relative jump opcodes.
- bpo-45317: Add internal documentation explaining design of new (for 3.11) frame stack.

### LibraryÂ¶

- bpo-47197: ctypes used to mishandle `void` return types, so that for
instance a function declared like`ctypes.CFUNCTYPE(None, ctypes.c_int)` would be called with signature`int f(int)` instead of`void f(int)` .
Wasm targets require function pointers to be called with the correct
signatures so this led to crashes. The problem is now fixed.

### Core and BuiltinsÂ¶

- bpo-47120: Make opcodes `JUMP_IF_TRUE_OR_POP` and`JUMP_IF_FALSE_OR_POP` relative rather than absolute.
- bpo-47177: Replace the `f_lasti` member of the internal`_PyInterpreterFrame` structure with a`prev_instr` pointer, which
reduces overhead in the main interpreter loop. The`f_lasti` attribute
of Python-layer frame objects is preserved for backward-compatibility.
- bpo-46961: Integer mod/remainder operations, including the three-argument form of `pow()` , now consistently return ints from the global small
integer cache when applicable.
- bpo-46962: Classes and functions that unconditionally declared their docstrings ignoring the `--without-doc-strings` compilation flag no
longer do so.The classes affected are `ctypes.UnionType` ,`pickle.PickleBuffer` ,`testcapi.RecursingInfinitelyError` ,
and`types.GenericAlias` .The functions affected are 24 methods in `ctypes` .Patch by Oleg Iarygin.
- bpo-46942: Use Argument Clinic for the `types.MethodType` constructor.  Patch by Oleg Iarygin.
- bpo-46764: Fix wrapping bound methods with @classmethod
- bpo-43464: Optimize `set.intersection()` for non-set arguments.
- bpo-46721: Optimize `set.issuperset()` for non-set argument.
- bpo-46509: Add type-specialized versions of the `Py_DECREF()` , and use
them for`float` ,`int` ,`str` ,`bool` , and`None` to avoid
pointer-chasing at runtime where types are known at C compile time.

### BuildÂ¶

- bpo-46045: Do not use POSIX semaphores on NetBSD

### Core and BuiltinsÂ¶

- bpo-36819: Fix crashes in built-in encoders with error handlers that return position less or equal than the starting position of non-encodable characters.
- bpo-34093: `marshal.dumps()` uses`FLAG_REF` for all interned strings.
This makes output more deterministic and helps reproducible build.
- bpo-26579: Added `object.__getstate__` which provides the default
implementation of the`__getstate__()` method.Copying and pickling instances of subclasses of builtin types bytearray, set, frozenset, collections.OrderedDict, collections.deque, weakref.WeakSet, and datetime.tzinfo now copies and pickles instance attributes implemented as slots.

### LibraryÂ¶

- gh-87901: Add the *encoding* parameter to`os.popen()` .
- gh-90997: Fix an issue where `dis` utilities may interpret populated
inline cache entries as valid instructions.
- gh-92332: Deprecate `typing.Text` (removal of the class is
currently not planned). Patch by Alex Waygood.
- gh-78157: Deprecate nested classes in enum definitions becoming members â in 3.13 they will be normal classes; add `member` and`nonmember` functions to allow control over results now.
- gh-92356: Fixed a performance regression in ctypes function calls.
- gh-90997: Show the actual named values stored in inline caches when `show_caches=True` is passed to`dis` utilities.
- gh-92301: Prefer `close_range()` to iterating over procfs for file
descriptor closing in`subprocess` for better performance.
- gh-67248: Sort the miscellaneous topics in Cmd.do_help()
- gh-92210: Port `socket.__init__` to Argument Clinic. Patch by Cinder.
- gh-80010: Add support for generalized ISO 8601 parsing to `datetime.datetime.fromisoformat()` ,`datetime.date.fromisoformat()` and`datetime.time.fromisoformat()` . Patch by Paul Ganssle.
- gh-92118: Fix a 3.11 regression in `contextmanager()` ,
which caused it to propagate exceptions with incorrect tracebacks.
- gh-90887: Adding `COPYFILE_STAT` ,`COPYFILE_ACL` and`COPYFILE_XATTR` constants for`os.fcopyfile()` available in macOs.
- gh-91215: For `@~dataclasses.dataclass` , add*weakref_slot* . The new
parameter defaults to`False` . If true, and if`slots=True` , add a
slot named`"__weakref__"` , which will allow instances to be weakrefâd.
Contributed by Eric V. Smith
- gh-85984: New function os.login_tty() for Unix.
- gh-92128: Add `__class_getitem__()` to`logging.LoggerAdapter` and`logging.StreamHandler` ,
allowing them to be parameterized at runtime. Patch by Alex Waygood.
- gh-92049: Forbid pickling constants `re._constants.SUCCESS` etc.
Previously, pickling did not fail, but the result could not be unpickled.
- gh-92062: `inspect.Parameter` now raises`ValueError` if`name` is a keyword, in addition to the existing check that it is an
identifier.
- gh-87390: Add an `__unpacked__` attribute to`types.GenericAlias` . Patch by Jelle Zijlstra.
- gh-88089: Add support for generic `typing.NamedTuple` .
- gh-91996: New http.HTTPMethod enum to represent all the available HTTP request methods in a convenient way
- gh-91984: Modified test strings in test_argparse.py to not contain trailing spaces before end of line.
- gh-91952: Add `encoding="locale"` support to`TextIOWrapper.reconfigure()` .
- gh-91954: Add *encoding* and*errors* arguments to`subprocess.getoutput()` and`subprocess.getstatusoutput()` .
- bpo-47029: Always close the read end of the pipe used by `multiprocessing.Queue`*after* the last write of buffered data to
the write end of the pipe to avoid`BrokenPipeError` at garbage
collection and at`multiprocessing.Queue.close()` calls. Patch by GÃ©ry
Ogam.
- gh-91928: Add `datetime.UTC` alias for`datetime.timezone.utc` .Patch by Kabir Kwatra.
- gh-68966: The `mailcap` module is now deprecated and will be removed
in Python 3.13. See**PEP 594** for the rationale and the`mimetypes` module for an alternative. Patch by Victor Stinner.
- gh-91401: Provide a way to disable `subprocess` use of`vfork()` just in case it is ever needed and document the existing mechanism for`posix_spawn()` .
- gh-64783: Fix `signal.NSIG` value on FreeBSD to accept signal
numbers greater than 32, like`signal.SIGRTMIN` and`signal.SIGRTMAX` . Patch by Victor Stinner.
- gh-91910: Add missing f prefix to f-strings in error messages from the `multiprocessing` and`asyncio` modules.
- gh-91860: Add `typing.dataclass_transform()` , implementing**PEP 681** .
Patch by Jelle Zijlstra.
- gh-91832: Add `required` attribute to`argparse.Action` repr
output.
- gh-91827: In the `tkinter` module add method`info_patchlevel()` which returns the exact version of the Tcl library as a named tuple
similar to`sys.version_info` .
- gh-84461: Add `--enable-wasm-pthreads` to enable pthreads support
for WASM builds.`Emscripten/node` no longer has threading enabled by
default. Include additional file systems.
- gh-91821: Fix unstable `test_from_tuple` test in`test_decimal.py` .
- gh-91217: Deprecate the xdrlib module.
- gh-91217: Deprecate the uu module.
- gh-91760: More strict rules will be applied for numerical group references and group names in regular expressions. For now, a deprecation warning is emitted for group references and group names which will be errors in future Python versions.
- gh-84461: Add provisional `sys._emscripten_info` named tuple with
build-time and run-time information about Emscripten platform.
- gh-90623: `signal.raise_signal()` and`os.kill()` now check
immediately for pending signals. Patch by Victor Stinner.
- gh-91734: Fix OSS audio support on Solaris.
- gh-90633: Include the passed value in the exception thrown by `typing.assert_never()` . Patch by Jelle Zijlstra.
- gh-91700: Compilation of regular expression containing a conditional expression `(?(group)...)` now raises an appropriate`re.error` if
the group number refers to not defined group. Previously an internal
RuntimeError was raised.
- gh-91231: Add an optional keyword *shutdown_timeout* parameter to the`multiprocessing.BaseManager` constructor. Kill the process if
terminate() takes longer than the timeout. Patch by Victor Stinner.
- gh-91621: Fix `typing.get_type_hints()` for`collections.abc.Callable` . Patch by Shantanu Jain.
- gh-90568: Parsing `\N` escapes of Unicode Named Character Sequences in a`regular expression` raises now`re.error` instead of`TypeError` .
- gh-91670: Remove deprecated `SO` config variable in`sysconfig` .
- gh-91217: Deprecate the telnetlib module.
- gh-91217: Deprecate the sunau module.
- gh-91217: Deprecate the spwd module.
- gh-91217: Deprecate the sndhdr module, as well as inline needed functionality for `email.mime.MIMEAudio` .
- gh-91616: `re` module, fix`fullmatch()` mismatch
when using Atomic Grouping or Possessive Quantifiers.
- gh-91217: Deprecate the âpipesâ module.
- gh-91217: Deprecate the ossaudiodev module.
- bpo-47256: `re` module, limit the maximum capturing group to
1,073,741,823 in 64-bit build, this increases the depth of backtracking.
- gh-91217: Deprecate the nis module.
- gh-91595: Fix the comparison of character and integer inside `Tools.gdb.libpython.write_repr()` . Patch by Yu Liu.
- gh-74166: Add option to raise all errors from `create_connection()` in an`ExceptionGroup` when it
fails to create a connection. The default remains to raise only the last
error that had occurred when multiple addresses were tried.
- gh-91487: Optimize asyncio UDP speed, over 100 times faster when transferring a large file.
- gh-91575: Update case-insensitive matching in the `re` module to the
latest Unicode version.
- gh-90622: In `concurrent.futures.process.ProcessPoolExecutor` disallow
the âforkâ multiprocessing start method when the new`max_tasks_per_child` feature is used as the mix of threads+fork can
hang the child processes. Default to using the safe âspawnâ start method
in that circumstance if no`mp_context` was supplied.
- gh-89022: In `sqlite3` ,`SQLITE_MISUSE` result codes are now mapped
to`InterfaceError` instead of`ProgrammingError` . Also, more accurate exceptions are
raised when binding parameters fail. Patch by Erlend E. Aasland.
- gh-91526: Stop calling `os.device_encoding(file.fileno())` in`TextIOWrapper` . It was complex, never documented, and didnât work
for most cases. (Patch by Inada Naoki.)
- gh-88116: Change the frame-related functions in the `inspect` module
to return a regular object (that is backwards compatible with the old
tuple-like interface) that include the extended**PEP 657** position
information (end line number, column and end column). The affected
functions are:`inspect.getframeinfo()` ,`inspect.getouterframes()` ,`inspect.getinnerframes()` ,`inspect.stack()` and`inspect.trace()` . Patch by Pablo Galindo.
- gh-69093: Add indexing and slicing support to `sqlite3.Blob` . Patch
by Aviv Palivoda and Erlend E. Aasland.
- gh-69093: Add context manager support to `sqlite3.Blob` .
Patch by Aviv Palivoda and Erlend E. Aasland.
- gh-91217: Deprecate nntplib.
- gh-91217: Deprecate msilib.
- gh-91404: Improve the performance of `re` matching by using computed
gotos (or âthreaded codeâ) on supported platforms and removing expensive
pointer indirections.
- gh-91217: Deprecate the imghdr module.
- gh-91217: Deprecate the crypt module.
- gh-91276: Make space for longer opcodes in `dis` output.
- bpo-47000: Make `TextIOWrapper` uses locale encoding when`encoding="locale"` is specified even in UTF-8 mode.
- gh-91230: `warnings.catch_warnings()` now accepts arguments for`warnings.simplefilter()` , providing a more concise way to locally
ignore warnings or convert them to errors.
- gh-91217: Deprecate the chunk module.
- gh-91498: Add the `TCP_CONNECTION_INFO` option (available on macOS) to`socket` .
- bpo-47260: Fix `os.closerange()` potentially being a no-op in a Linux
seccomp sandbox.
- bpo-47087: Implement `typing.Required` and`typing.NotRequired` (**PEP 655** ). Patch by David Foster and Jelle Zijlstra.
- bpo-47061: Deprecate cgi and cgitb.
- bpo-47061: Deprecate audioop.
- bpo-47000: Add `locale.getencoding()` to get the current locale
encoding. It is similar to`locale.getpreferredencoding(False)` but
ignores the Python UTF-8 Mode.
- bpo-42012: Add `wsgiref.types` , containing WSGI-specific types for
static type checking.
- bpo-47227: Suppress expression chaining for more `re` parsing errors.
- bpo-47211: Remove undocumented and never working function `re.template()` and flag`re.TEMPLATE` . This was later reverted in
3.11.0b2 and deprecated instead.
- bpo-47135: `decimal.localcontext()` now accepts context attributes via
keyword arguments
- bpo-43323: Fix errors in the `email` module if the charset itself
contains undecodable/unencodable characters.
- bpo-46841: Disassembly of quickened code.
- bpo-46681: Forward gzip.compress() compresslevel to zlib.
- bpo-45100: Add `typing.get_overloads()` and`typing.clear_overloads()` . Patch by Jelle Zijlstra.
- bpo-44807: `typing.Protocol` no longer silently replaces`__init__()` methods defined on subclasses. Patch by Adrian Garcia
Badaracco.
- bpo-46787: Fix `concurrent.futures.ProcessPoolExecutor` exception
memory leak
- bpo-46720: Add support for path-like objects to `multiprocessing.set_executable()` for Windows to be on a par with
Unix-like systems. Patch by GÃ©ry Ogam.
- bpo-46696: Add `SO_INCOMING_CPU` constant to`socket` .
- bpo-46053: Fix OSS audio support on NetBSD.
- bpo-45639: `image/avif` and`image/webp` were added to`mimetypes` .
- bpo-46285: Add command-line option `-p` /`--protocol` to module`http.server` which specifies the HTTP version to which the server is
conformant (HTTP/1.1 conformant servers can now be run from the
command-line interface of module`http.server` ). Patch by GÃ©ry Ogam.
- bpo-44791: Accept ellipsis as the last argument of `typing.Concatenate` .
- bpo-46547: Remove variables leaking into `pydoc.Helper` class namespace.
- bpo-46415: Fix ipaddress.ip_{address,interface,network} raising TypeError instead of ValueError if given invalid tuple as address parameter.
- bpo-46075: `CookieJar` with`DefaultCookiePolicy` now can process
cookies from localhost with domain=localhost explicitly specified in
Set-Cookie header.
- bpo-45995: Add a âzâ option to the string formatting specification that coerces negative zero floating-point values to positive zero after rounding to the format precision. Contributed by John Belmonte.
- bpo-26175: Fully implement the `io.BufferedIOBase` or`io.TextIOBase` interface for`tempfile.SpooledTemporaryFile` objects. This lets them work
correctly with higher-level layers (like compression modules). Patch by
Carey Metcalfe.
- bpo-45138: Fix a regression in the `sqlite3` trace callback where
bound parameters were not expanded in the passed statement string. The
regression was introduced in Python 3.10 by bpo-40318. Patch by
Erlend E. Aasland.
- bpo-44863: Allow `TypedDict` subclasses to also include`Generic` as a base class in class based syntax. Thereby
allowing the user to define a generic`TypedDict` , just like a
user-defined generic but with`TypedDict` semantics.
- bpo-44587: Fix BooleanOptionalAction to not automatically add a default string. If a default string is desired, use a formatter to add it.
- bpo-43827: All positional-or-keyword parameters to `ABCMeta.__new__` are
now positional-only to avoid conflicts with keyword arguments to be passed
to`__init_subclass__()` .
- bpo-43218: Prevent creation of a venv whose path contains the PATH separator. This could affect the usage of the activate script. Patch by Dustin Rodrigues.
- bpo-38435: Add a `process_group` parameter to`subprocess.Popen` to help move more things off of the unsafe`preexec_fn` parameter.
- bpo-42066: Fix cookies getting sorted in `CookieJar.__iter__()` which
is an extra behavior and not mentioned in RFC 2965 or Netscape cookie
protocol. Now the cookies in`CookieJar` follows the order of the`Set-Cookie` header. Patch by Iman Kermani.
- bpo-40617: Add `create_window_function()` to`sqlite3.Connection` for creating aggregate window functions. Patch
by Erlend E. Aasland.
- bpo-40676: Convert `csv` to use Argument Clinic for`csv.field_size_limit()` ,`csv.get_dialect()` ,`csv.unregister_dialect()` and`csv.list_dialects()` .
- bpo-39716: Raise an `ArgumentError` when the same subparser name is
added twice to an`argparse.ArgumentParser` .  This is consistent with
the (default) behavior when the same option string is added twice to an`ArgumentParser` .
- bpo-36073: Raise `ProgrammingError` instead of segfaulting
on recursive usage of cursors in`sqlite3` converters. Patch by
Sergey Fedoseev.
- bpo-34975: Adds a `start_tls()` method to`StreamWriter` , which upgrades the connection with
TLS using the given`SSLContext` .
- bpo-22276: `Path` methods`glob()` and`rglob()` return only directories if*pattern* ends with
a pathname components separator (`/` or`sep` ). Patch by
Eisuke Kawashima.
- bpo-24905: Add `blobopen()` to`sqlite3.Connection` .`sqlite3.Blob` allows incremental I/O
operations on blobs. Patch by Aviv Palivoda and Erlend E. Aasland.

### DocumentationÂ¶

- gh-91888: Add a new `gh` role to the documentation to link to GitHub
issues.
- gh-91783: Document security issues concerning the use of the function `shutil.unpack_archive()`
- gh-91547: Remove âUndocumented modulesâ page.
- gh-91298: In `importlib.resources.abc` , refined the documentation of the
Traversable Protocol, applying changes from importlib_resources 5.7.1.
- bpo-44347: Clarify the meaning of *dirs_exist_ok* , a kwarg of`shutil.copytree()` .
- bpo-36329: Remove âmake -C Doc serveâ in favour of âmake -C Doc htmlviewâ
- bpo-47189: Add a Whatâs New in Python 3.11 entry for the Faster CPython project. Documentation by Ken Jin and Kumar Aditya.
- bpo-38668: Update the introduction to documentation for `os.path` to
remove warnings that became irrelevant after the implementations of**PEP 383** and**PEP 529** .
- bpo-47115: The documentation now lists which members of C structs are part of the Limited API/Stable ABI.
- bpo-46962: All docstrings in code snippets are now wrapped into `PyDoc_STR` to follow the guideline of**PEP 7âs
Documentation Strings paragraph** . Patch by
Oleg Iarygin.
- bpo-26792: Improve the docstrings of `runpy.run_module()` and`runpy.run_path()` . Original patch by Andrew Brezovsky.

### TestsÂ¶

- gh-92169: Use `warnings_helper.import_deprecated()` to import deprecated
modules uniformly in tests. Patch by Hugo van Kemenade.
- gh-84461: When multiprocessing is enabled, libregrtest can now use a Python executable other than `sys.executable` via the`--python` flag.
- gh-91904: Fix initialization of `PYTHONREGRTEST_UNICODE_GUARD` which prevented running regression tests on non-UTF-8 locale.
- gh-91752: Added @requires_zlib to test.test_tools.test_freeze.TestFreeze.
- gh-91607: Fix `test_concurrent_futures` to test the correct
multiprocessing start method context in several cases where the test logic
mixed this up.
- bpo-40280: Threading tests are now skipped on WASM targets without pthread support.
- bpo-47109: Test for `ctypes.macholib.dyld` ,`ctypes.macholib.dylib` , and`ctypes.macholib.framework` are
brought from manual pre-`unittest` times to`ctypes.test` location and structure. Patch by Oleg Iarygin.
- bpo-29890: Add tests for `ipaddress.IPv4Interface` and`ipaddress.IPv6Interface` construction with tuple arguments.
Original patch and tests by louisom.

### BuildÂ¶

- gh-89452: gdbm-compat is now preferred over ndbm if both are available on the system. This allows avoiding the problematic ndbm.h on macOS.
- gh-91731: Python is now built with `-std=c11` compiler option, rather
than`-std=c99` . Patch by Victor Stinner.
- bpo-47152: Add script and make target for generating `sre_constants.h` .
- bpo-47103: Windows `PGInstrument` builds now copy a required DLL into
the output directory, making it easier to run the profile stage of a PGO
build.

### WindowsÂ¶

- bpo-46907: Update Windows installer to use SQLite 3.38.3.
- bpo-47239: Fixed âlist and âlist-paths output for Python install manager when used in an active virtual environment.
- bpo-46907: Update Windows installer to use SQLite 3.38.2.
- bpo-46785: Fix race condition between `os.stat()` and unlinking a file
on Windows, by using errors codes returned by`FindFirstFileW()` when
appropriate in`win32_xstat_impl` .
- bpo-40859: Update Windows build to use xz-5.2.5

### macOSÂ¶

- bpo-46907: Update macOS installer to SQLite 3.38.4.

### Tools/DemosÂ¶

- gh-91583: Fix regression in the code generated by Argument Clinic for functions with the `defining_class` parameter.
- gh-91575: Add script `Tools/scripts/generate_re_casefix.py` and the make
target`regen-re` for generating additional data for case-insensitive
matching according to the current Unicode version.
- gh-91551: Remove the ancient Pynche color editor. It has moved to https://gitlab.com/warsaw/pynche

### C APIÂ¶

- gh-88279: Deprecate the C functions: `PySys_SetArgv()` ,`PySys_SetArgvEx()` ,`PySys_SetPath()` . Patch by Victor
Stinner.
- gh-92154: Added the `PyCode_GetCode()` function. This function does
the equivalent of the Python code`getattr(code_object, 'co_code')` .
- gh-92173: Fix the `closure` argument to`PyEval_EvalCodeEx()` .
- gh-91320: Fix C++ compiler warnings about âold-style castâ ( ```
g++
-Wold-style-cast
```
) in the Python C API. Use C++`reinterpret_cast<>` and`static_cast<>` casts when the Python C API is used in C++. Patch by
Victor Stinner.
- gh-80527: Mark functions as deprecated by **PEP 623** :`PyUnicode_AS_DATA()` ,`PyUnicode_AS_UNICODE()` ,`PyUnicode_GET_DATA_SIZE()` ,`PyUnicode_GET_SIZE()` . Patch
by Victor Stinner.
- gh-91768: `Py_REFCNT()` ,`Py_TYPE()` ,`Py_SIZE()` and`Py_IS_TYPE()` functions argument type is now`PyObject*` , rather
than`const PyObject*` . Patch by Victor Stinner.
- gh-91020: Add `PyBytes_Type.tp_alloc` to initialize`PyBytesObject.ob_shash` for bytes subclasses.
- bpo-40421: Add `PyFrame_GetLasti` C-API function to access frame
objectâs`f_lasti` attribute safely from C code.
- bpo-35134: Remove the `Include/code.h` header file. C extensions should
only include the main`<Python.h>` header file. Patch by Victor Stinner.
- bpo-47169: `PyOS_CheckStack()` is now exported in the Stable ABI on
Windows.
- bpo-47169: `PyThread_get_thread_native_id()` is excluded from the
stable ABI on platforms where it doesnât exist (like Solaris).
- bpo-46343: Added `PyErr_GetHandledException()` and`PyErr_SetHandledException()` as simpler alternatives to`PyErr_GetExcInfo()` and`PyErr_SetExcInfo()` .They are included in the stable ABI.

## Python 3.11.0 alpha 7Â¶

*Release date: 2022-04-05*

### Core and BuiltinsÂ¶

- bpo-47212: Raise `IndentationError` instead of`SyntaxError` for
a bare`except` with no following indent. Improve`SyntaxError` locations for an un-parenthesized generator used as arguments. Patch by
Matthieu Dartiailh.
- bpo-47186: Replace `JUMP_IF_NOT_EG_MATCH` by`CHECK_EG_MATCH` + jump.
- bpo-47176: Emscripten builds cannot handle signals in the usual way due to platform limitations. Python can now handle signals. To use, set Module.Py_EmscriptenSignalBuffer to be a single byte SharedArrayBuffer and set Py_EMSCRIPTEN_SIGNAL_HANDLING to 1. Writing a number into the SharedArrayBuffer will cause the corresponding signal to be raised into the Python thread.
- bpo-47186: Replace `JUMP_IF_NOT_EXC_MATCH` by`CHECK_EXC_MATCH` + jump.
- bpo-47120: Replace the absolute jump opcode `JUMP_NO_INTERRUPT` by
the relative`JUMP_BACKWARD_NO_INTERRUPT` .
- bpo-46841: Avoid unnecessary allocations when comparing code objects.
- bpo-47182: Fix a crash when using a named unicode character like `"\N{digit nine}"` after the main interpreter has been initialized a
second time.
- bpo-47162: WebAssembly cannot deal with bad function pointer casts (different count or types of arguments). Python can now use call trampolines to mitigate the problem. Define `PY_CALL_TRAMPOLINE` to enable call trampolines.
- bpo-46775: Some Windows system error codes(>= 10000) are now mapped into the correct errno and may now raise a subclass of `OSError` . Patch by
Donghee Na.
- bpo-47129: Improve error messages in f-string syntax errors concerning empty expressions.
- bpo-47117: Fix a crash if we fail to decode characters in interactive mode if the tokenizer buffers are uninitialized. Patch by Pablo Galindo.
- bpo-47127: Speed up calls to c functions with keyword arguments by 25% with specialization. Patch by Kumar Aditya.
- bpo-47120: Replaced `JUMP_ABSOLUTE` by the relative jump`JUMP_BACKWARD` .
- bpo-42197: `PyFrame_FastToLocalsWithError()` and`PyFrame_LocalsToFast()` are no longer called during profiling nor
tracing. C code can access the`f_locals` attribute of`PyFrameObject` by calling`PyFrame_GetLocals()` .
- bpo-47070: Improve performance of `array_inplace_repeat` by reducing the
number of invocations of`memcpy` . Refactor the`repeat` and inplace`repeat` methods of`array` ,`bytes` ,`bytearray` and`unicodeobject` to use the common`_PyBytes_Repeat` .
- bpo-47053: Reduce de-optimization in the specialized `BINARY_OP_INPLACE_ADD_UNICODE` opcode.
- bpo-47045: Remove the `f_state` field from the _PyInterpreterFrame
struct. Add the`owner` field to the _PyInterpreterFrame struct to make
ownership explicit to simplify clearing and deallocing frames and
generators.

### LibraryÂ¶

- bpo-46968: Check for the existence of the âsys/auxv.hâ header in `faulthandler` to avoid compilation problems in systems where this
header doesnât exist. Patch by Pablo Galindo

### Core and BuiltinsÂ¶

- bpo-46329: Use low bit of `LOAD_GLOBAL` to indicate whether to push a`NULL` before the global. Helps streamline the call sequence a bit.
- bpo-46841: Quicken bytecode in-place by storing it as part of the corresponding `PyCodeObject` .
- bpo-47012: Speed up iteration of `bytes` and`bytearray` by
30%. Patch by Kumar Aditya.
- bpo-47009: Improved the performance of `list.append()` and list
comprehensions by optimizing for the common case, where no resize is
needed. Patch by Dennis Sweeney.
- bpo-47005: Improve performance of `bytearray_repeat` and`bytearray_irepeat` by reducing the number of invocations of`memcpy` .

### LibraryÂ¶

- bpo-46829: Deprecate passing a message into `asyncio.Future.cancel()` and`asyncio.Task.cancel()`

### Core and BuiltinsÂ¶

- bpo-46993: Speed up `bytearray` creation from`list` and`tuple` by 40%. Patch by Kumar Aditya.
- bpo-39829: Removed the `__len__()` call when initializing a list and
moved initializing to`list_extend` . Patch by Jeremiah Pascual.
- bpo-46944: Speed up throwing exception in generator with `METH_FASTCALL` calling convention. Patch by Kumar Aditya.
- bpo-46841: Modify `STORE_SUBSCR` to use an inline cache entry
(rather than its oparg) as an adaptive counter.
- bpo-46841: Use inline caching for `PRECALL` and`CALL` ,
and remove the internal machinery for managing the (now unused) non-inline
caches.
- bpo-46881: Statically allocate and initialize the latin1 characters.
- bpo-46838: Improve syntax errors for incorrect function definitions. Patch by Pablo Galindo
- bpo-43721: Fix docstrings of `getter` ,`setter` , and`deleter` to clarify that
they create a new copy of the property.
- bpo-43224: Make grammar changes required for PEP 646.

### LibraryÂ¶

- bpo-47208: Allow vendors to override `CTYPES_MAX_ARGCOUNT` .
- bpo-23689: `re` module: fix memory leak when a match is terminated by
a signal or memory allocation failure. Patch by Ma Lin.
- bpo-47167: Allow overriding a future compliance check in `asyncio.Task` .
- bpo-47151: When subprocess tries to use vfork, it now falls back to fork if vfork returns an error. This allows use in situations where vfork isnât allowed by the OS kernel.
- bpo-47152: Convert the `re` module into a package. Deprecate modules`sre_compile` ,`sre_constants` and`sre_parse` .
- bpo-4833: Add `ZipFile.mkdir()`
- bpo-27929: Fix `asyncio.loop.sock_connect()` to only resolve names for`socket.AF_INET` or`socket.AF_INET6` families. Resolution
may not make sense for other families, like`socket.AF_BLUETOOTH` and`socket.AF_UNIX` .
- bpo-14265: Adds the fully qualified test name to unittest output
- bpo-47061: Deprecate the aifc module.
- bpo-39622: Handle Ctrl+C in asyncio programs to interrupt the main task.
- bpo-47101: `hashlib.algorithms_available` now lists only algorithms
that are provided by activated crypto providers on OpenSSL 3.0. Legacy
algorithms are not listed unless the legacy provider has been loaded into
the default OSSL context.
- bpo-47099: All `URLError` exception messages raised in`urllib.request.URLopener` now contain a colon between```
ftp
error
```
and the rest of the message. Previously,`open_ftp()` missed the colon. Patch by Oleg
Iarygin.
- bpo-47099: Exception chaining is changed from `Exception.with_traceback()` /`sys.exc_info()` to**PEP 3134** .
Patch by Oleg Iarygin.
- bpo-47095: `hashlib` âs internal`_blake2` module now prefers`libb2` from https://www.blake2.net/ over Pythonâs vendored copy of
blake2.
- bpo-47098: The Keccak Code Package for `hashlib` âs internal`_sha3` module has been replaced with tiny_sha3. The module is used as fallback
when Python is built without OpenSSL.
- bpo-47088: Implement `typing.LiteralString` , part of**PEP 675** .
Patch by Jelle Zijlstra.
- bpo-42885: Optimize `re.search()` ,`re.split()` ,`re.findall()` ,`re.finditer()` and`re.sub()` for regular
expressions starting with`\A` or`^` .
- bpo-23691: Protect the `re.finditer()` iterator from re-entering.
- bpo-47067: Optimize calling `GenericAlias` objects by using**PEP 590**`vectorcall` and by replacing`PyObject_SetAttrString` with`PyObject_SetAttr` .
- bpo-28080: Add the *metadata_encoding* parameter in the`zipfile.ZipFile` constructor and the`--metadata-encoding` option in the`zipfile` CLI to allow reading zipfiles using
non-standard codecs to encode the filenames within the archive.
- bpo-47000: Make `io.text_encoding()` returns âutf-8â when UTF-8 mode
is enabled.
- bpo-42369: Fix thread safety of `zipfile._SharedFile.tell()` to avoid
a âzipfile.BadZipFile: Bad CRC-32 for fileâ exception when reading a`ZipFile` from multiple threads.
- bpo-38256: Fix `binascii.crc32()` when it is compiled to use zlibâc
crc32 to work properly on inputs 4+GiB in length instead of returning the
wrong result. The workaround prior to this was to always feed the function
data in increments smaller than 4GiB or to just call the zlib module
function.We also have `binascii.crc32()` release the GIL when computing on
larger inputs as`zlib.crc32()` and`hashlib` do.This also boosts performance on Windows as it now uses the zlib crc32 implementation for `binascii.crc32()` for a 2-3x speedup.That the stdlib has a crc32 API in two modules is a known historical oddity. This moves us closer to a single implementation behind them.
- bpo-47066: Global inline flags (e.g. `(?i)` ) can now only be used at the
start of the regular expressions.  Using them not at the start of
expression was deprecated since Python 3.6.
- bpo-39394: A warning about inline flags not at the start of the regular expression now contains the position of the flag.
- bpo-433030: Add support of atomic grouping ( `(?>...)` ) and possessive
quantifiers (`*+` ,`++` ,`?+` ,`{m,n}+` ) in```
regular
expressions
```
.
- bpo-47062: Implement `asyncio.Runner` context manager.
- bpo-46382: `dataclass()``slots=True` now correctly
omits slots already defined in base classes. Patch by Arie Bovenberg.
- bpo-47057: Use FASTCALL convention for `FutureIter.throw()`
- bpo-47061: Deprecate the various modules listed by **PEP 594** :aifc, asynchat, asyncore, audioop, cgi, cgitb, chunk, crypt, imghdr, msilib, nntplib, nis, ossaudiodev, pipes, smtpd, sndhdr, spwd, sunau, telnetlib, uu, xdrlib
- bpo-34790: Remove passing coroutine objects to `asyncio.wait()` .
- bpo-47039: Normalize `repr()` of asyncio future and task objects.
- bpo-2604: Fix bug where doctests using globals would fail when run multiple times.
- bpo-45150: Add `hashlib.file_digest()` helper for efficient hashing of
file object.
- bpo-34861: Made cumtime the default sorting key for cProfile
- bpo-45997: Fix `asyncio.Semaphore` re-aquiring FIFO order.
- bpo-47022: The `asynchat` ,`asyncore` and`smtpd` modules have been deprecated since at least Python 3.6. Their
documentation and deprecation warnings and have now been updated to note
they will removed in Python 3.12 (**PEP 594** ).
- bpo-43253: Fix a crash when closing transports where the underlying socket handle is already invalid on the Proactor event loop.
- bpo-40280: `select.select()` now passes`NULL` to`select` for
each empty fdset.
- bpo-47004: Apply bugfixes from importlib_metadata 4.11.3, including bugfix for EntryPoint.extras, which was returning match objects and not the extras strings.
- bpo-46998: Allow subclassing of `typing.Any` . Patch by Shantanu
Jain.
- bpo-46995: Deprecate missing `asyncio.Task.set_name()` for third-party
task implementations, schedule making it mandatory in Python 3.13.
- bpo-46994: Accept explicit contextvars.Context in `asyncio.create_task()` and`asyncio.loop.create_task()` .
- bpo-46981: `typing.get_args(typing.Tuple[()])` now returns`()` instead of`((),)` .
- bpo-46968: Add `os.sysconf_names['SC_MINSIGSTKSZ']` .
- bpo-46985: Upgrade pip wheel bundled with ensurepip (pip 22.0.4)
- bpo-46968: `faulthandler` : On Linux 5.14 and newer, dynamically
determine size of signal handler stack size CPython allocates using`getauxval(AT_MINSIGSTKSZ)` . This changes allows for Python extensionâs
request to Linux kernel to use AMX_TILE instruction set on Sapphire Rapids
Xeon processor to succeed, unblocking use of the ISA in frameworks.
- bpo-46917: The `math.nan` value is now always available. Patch by
Victor Stinner.
- bpo-46955: Expose `asyncio.base_events.Server` as`asyncio.Server` . Patch by Stefan Zabka.
- bpo-23325: The `signal` module no longer assumes that`SIG_IGN` and`SIG_DFL` are small int
singletons.
- bpo-46932: Update bundled libexpat to 2.4.7
- bpo-46933: The `pwd` module is now optional.`os.path.expanduser()` returns the path when the`pwd` module is
not available.
- bpo-40059: **PEP 680** , the`tomllib` module. Adds support for parsing
TOML.
- bpo-464471: `asyncio.timeout()` and`asyncio.timeout_at()` context
managers added. Patch by Tin TvrtkoviÄ and Andrew Svetlov.
- bpo-46805: Added raw datagram socket functions for asyncio: `sock_sendto()` ,`sock_recvfrom()` and`sock_recvfrom_into()` .
- bpo-46644: No longer require valid typeforms to be callable. This allows `typing.Annotated` to wrap`typing.ParamSpecArgs` and`dataclasses.InitVar` . Patch by Gregory Beauregard.
- bpo-46581: Brings `ParamSpec` propagation for`GenericAlias` in line with`Concatenate` (and others).
- bpo-45413: Define *posix_venv* and*nt_venv* sysconfig installation
schemes to be used for bootstrapping new virtual
environments. Add*venv* sysconfig installation scheme to get the
appropriate one of the above. The schemes are identical to the
pre-existing*posix_prefix* and*nt* install schemes. The`venv` module now uses the*venv* scheme to create new virtual environments
instead of hardcoding the paths depending only on the platform. Downstream
Python distributors customizing the*posix_prefix* or*nt* install scheme
in a way that is not compatible with the install scheme used in virtual
environments are encouraged not to customize the*venv* schemes. When
Python itself runs in a virtual environment,`sysconfig.get_default_scheme()` and`sysconfig.get_preferred_scheme()` with`key="prefix"` returns*venv* .
- bpo-43224: Implement support for PEP 646 in typing.py.
- bpo-43224: Allow unpacking types.GenericAlias objects, e.g. ```
*tuple[int,
str]
```
.
- bpo-46557: Warnings captured by the logging module are now logged without a format string to prevent systems that group logs by the msg argument from grouping captured warnings together.
- bpo-41370: `typing.get_type_hints()` now supports evaluating strings
as forward references in PEP 585 generic aliases.
- bpo-46607: Add `DeprecationWarning` to`LegacyInterpolation` ,
deprecated in the docstring since Python 3.2. Will be removed in Python
3.13. Use`BasicInterpolation` or`ExtendedInterpolation` instead.
- bpo-26120: `pydoc` now excludes __future__ imports from the moduleâs
data items.
- bpo-46480: Add `typing.assert_type()` . Patch by Jelle Zijlstra.
- bpo-46421: Fix a unittest issue where if the command was invoked as `python -m unittest` and the filename(s) began with a dot (.), a`ValueError` is returned.
- bpo-46245: Add optional parameter *dir_fd* in`shutil.rmtree()` .
- bpo-22859: `unittest.TestProgram.usageExit()` is marked as
deprecated, to be removed in Python 3.13.
- bpo-46170: Improve the error message when you try to subclass an instance of `typing.NewType` .
- bpo-40296: Fix supporting generic aliases in `pydoc` .
- bpo-20392: Fix inconsistency with uppercase file extensions in `MimeTypes.guess_type()` . Patch by Kumar Aditya.
