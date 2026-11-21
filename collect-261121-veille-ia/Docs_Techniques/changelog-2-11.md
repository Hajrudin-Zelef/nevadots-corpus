---
id: collect-261121-veille-ia/docs-techniques/changelog-2-11
title: "Changelog"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["module", "tests", "raise", "patch", "asyncio", "test", "logging", "functions"]
source: https://docs.python.org/release/3.15.1/whatsnew/changelog.html
source_anchor: ""
source_lines: [15001, 16500]
section: "Docs_Techniques"
---
- bpo-46030: Add `LOCAL_CREDS` ,`LOCAL_CREDS_PERSISTENT` and`SCM_CREDS2` FreeBSD constants to the socket module.
- bpo-44439: Fix `.write()` method of a member file in`ZipFile` , when
the input data is an object that supports the buffer protocol, the file
length may be wrong.
- bpo-45171: Fix handling of the `stacklevel` argument to logging
functions in the`logging` module so that it is consistent across all
logging functions and, as advertised, similar to the`stacklevel` argument used in`warn()` .
- bpo-24959: Fix bug where `unittest` sometimes drops frames from
tracebacks of exceptions raised in tests.
- bpo-44859: Raise more accurate and **PEP 249** compatible exceptions in`sqlite3` .
  - Raise `InterfaceError` instead of`ProgrammingError` for`SQLITE_MISUSE` errors.
  - Donât overwrite `BufferError` with`ValueError` when conversion to
BLOB fails.
  - Raise `ProgrammingError` instead of`Warning` if
user tries to`execute()` more than one SQL statement.
  - Raise `ProgrammingError` instead of`ValueError` if an SQL
query contains null characters.
- bpo-44493: Add missing terminated NUL in sockaddr_unâs length This was potentially observable when using non-abstract AF_UNIX datagram sockets to processes written in another programming language.
- bpo-41930: Add `serialize()` and`deserialize()` support to`sqlite3` . Patch
by Erlend E. Aasland.
- bpo-33178: Added `ctypes.BigEndianUnion` and`ctypes.LittleEndianUnion` classes, as originally documented in the
library docs but not yet implemented.
- bpo-43352: Add an Barrier object in synchronization primitives of *asyncio* Lib in order to be consistent with Barrier from*threading* and*multiprocessing* libs*
- bpo-35859: `re` module, fix a few bugs about capturing group. In rare
cases, capturing group gets an incorrect string. Patch by Ma Lin.

### DocumentationÂ¶

- bpo-45099: Document internal `asyncio` API.
- bpo-47126: Update PEP URLs to **PEP 676** âs new canonical form.
- bpo-47040: Clarified the old Python versions compatibility note of `binascii.crc32()` /`zlib.adler32()` /`zlib.crc32()` functions.
- bpo-46033: Clarify `for` statement execution in its doc.
- bpo-45790: Adjust inaccurate phrasing in Defining Extension Types: Tutorial about the `ob_base` field and the
macros used to access its contents.
- bpo-42340: Document that in some circumstances `KeyboardInterrupt` may cause the code to enter an inconsistent state. Provided a sample
workaround to avoid it if needed.
- bpo-41233: Link the errnos referenced in `Doc/library/exceptions.rst` to
their respective section in`Doc/library/errno.rst` , and vice versa.
Previously this was only done for EINTR and InterruptedError. Patch by Yan
âyyyyyyyanâ Orestes.

### TestsÂ¶

- bpo-47205: Skip test for `sched_getaffinity()` and`sched_setaffinity()` error case on FreeBSD.
- bpo-46126: Restore âdescriptionsâ when running tests internally.
- bpo-47104: Rewrite `asyncio.to_thread()` tests to use`unittest.IsolatedAsyncioTestCase` .
- bpo-40280: The test suite is now passing on the Emscripten platform. All fork, socket, and subprocess-based tests are skipped.
- bpo-47037: Skip `strftime("%4Y")` feature test on Windows. It can cause
an assertion error in debug builds.
- bpo-46587: Skip tests if platformâs `strftime` does not support
non-portable glibc extensions.
- bpo-47015: A test case for `os.sendfile()` is converted from
deprecated`asyncore` (see**PEP 594** ) to`asyncio` . Patch by
Oleg Iarygin.

### BuildÂ¶

- bpo-40280: Add configure option `--enable-wasm-dynamic-linking` to
enable`dlopen` and MAIN_MODULE / SIDE_MODULE on`wasm32-emscripten` .
- bpo-46023: `makesetup` now detects and skips all duplicated module
definitions. The first entry wins.
- bpo-40280: Add SOABI `wasm32-emscripten` for Emscripten and`wasm32-wasi` for WASI on 32bit WASM as well as`wasm64` counter
parts.
- bpo-47032: Ensure Windows install builds fail correctly with a non-zero exit code when part of the build fails.
- bpo-47024: Update OpenSSL to 1.1.1n for macOS installers and all Windows builds.
- bpo-46996: The `tkinter` package now requires Tcl/Tk version 8.5.12
or newer.
- bpo-46973: Add `regen-configure` make target to regenerate configure
script with Christianâs container image`quay.io/tiran/cpython_autoconf:269` .
- bpo-46917: Building Python now requires support of IEEE 754 floating-point numbers. Patch by Victor Stinner.
- bpo-45774: `configure` now verifies that all SQLite C APIs needed for
the`sqlite3` extension module are found.

### WindowsÂ¶

- bpo-47194: Update `zlib` to v1.2.12 to resolve**CVE 2018-25032** .
- bpo-47171: Enables installing the `py.exe` launcher on Windows
ARM64.
- bpo-46566: Upgraded Python install manager to support a new `-V:company/tag` argument for full**PEP 514** support and to detect ARM64 installs. The`-64` suffix on arguments is deprecated, but still selects any
non-32-bit install. Setting`PYLAUNCHER_ALLOW_INSTALL` and
specifying a version that is not installed will attempt to install the
requested version from the Microsoft Store.
- bpo-47086: The installer for Windows now includes documentation as loose HTML files rather than a single compiled `.chm` file.
- bpo-46907: Update Windows installer to use SQLite 3.38.1.
- bpo-44549: Update bzip2 to 1.0.8 in Windows builds to mitigate **CVE 2016-3189** and**CVE 2019-12900** .
- bpo-46948: Prevent **CVE 2022-26488** by ensuring the Add to PATH option in
the Windows installer uses the correct path when being repaired.

### macOSÂ¶

### Tools/DemosÂ¶

- bpo-40280: Replace Emscriptenâs limited shell with Katie Bellâs browser-ui REPL from python-wasm project.

### C APIÂ¶

- bpo-40421: Add `PyFrame_GetBuiltins` ,`PyFrame_GetGenerator` and`PyFrame_GetGlobals` C-API functions to access frame object attributes
safely from C code.
- bpo-46850: Move the private `_PyFrameEvalFunction` type, and private`_PyInterpreterState_GetEvalFrameFunc()` and`_PyInterpreterState_SetEvalFrameFunc()` functions to the internal C
API. The`_PyFrameEvalFunction` callback function type now uses the`_PyInterpreterFrame` type which is part of the internal C API. Patch by
Victor Stinner.
- bpo-46850: Move the private undocumented `_PyEval_EvalFrameDefault()` function to the internal C API. The function now uses the`_PyInterpreterFrame` type which is part of the internal C API. Patch by
Victor Stinner.
- bpo-46850: Remove the private undocumented function `_PyEval_CallTracing()` from the C API. Call the public`sys.call_tracing()` function instead. Patch by Victor Stinner.
- bpo-46850: Remove the private undocumented function `_PyEval_GetCoroutineOriginTrackingDepth()` from the C API. Call the
public`sys.get_coroutine_origin_tracking_depth()` function instead.
Patch by Victor Stinner.
- bpo-46850: Remove the following private undocumented functions from the C API: 
  - `_PyEval_GetAsyncGenFirstiter()`
  - `_PyEval_GetAsyncGenFinalizer()`
  - `_PyEval_SetAsyncGenFirstiter()`
  - `_PyEval_SetAsyncGenFinalizer()`
 Call the public `sys.get_asyncgen_hooks()` and`sys.set_asyncgen_hooks()` functions instead. Patch by Victor Stinner.
- bpo-46987: Remove private functions `_PySys_GetObjectId()` and`_PySys_SetObjectId()` . Patch by Donghee Na.
- bpo-46906: Add new functions to pack and unpack C double (serialize and deserialize): `PyFloat_Pack2()` ,`PyFloat_Pack4()` ,`PyFloat_Pack8()` ,`PyFloat_Unpack2()` ,`PyFloat_Unpack4()` and`PyFloat_Unpack8()` . Patch by Victor
Stinner.

## Python 3.11.0 alpha 6Â¶

*Release date: 2022-03-07*

### Core and BuiltinsÂ¶

- bpo-46940: Avoid overriding `AttributeError` metadata information for
nested attribute access calls. Patch by Pablo Galindo.
- bpo-46927: Include the typeâs name in the error message for subscripting non-generic types.
- bpo-46921: Support vectorcall for `super()` . Patch by Ken Jin.
- bpo-46841: Fix incorrect handling of inline cache entries when specializing `BINARY_OP` .
- bpo-46841: Use an oparg to simplify the construction of helpful error messages in `GET_AWAITABLE` .
- bpo-46903: Make sure that str subclasses can be used as attribute names for instances with virtual dictionaries. Fixes regression in 3.11alpha
- bpo-46841: Add more detailed specialization failure stats for `COMPARE_OP` followed by`EXTENDED_ARG` .
- bpo-46891: Fix bug introduced during 3.11alpha where subclasses of `types.ModuleType` with`__slots__` were not initialized correctly,
resulting in an interpreter crash.
- bpo-46841: Use inline caching for `LOAD_ATTR` ,`LOAD_METHOD` , and`STORE_ATTR` .
- bpo-46841: Use inline cache for `BINARY_SUBSCR` .
- bpo-46841: Use inline caching for `COMPARE_OP` .
- bpo-46864: Deprecate `PyBytesObject.ob_shash` . It will be removed in
Python 3.13.
- bpo-46841: Use inline caching for `UNPACK_SEQUENCE` .
- bpo-46845: Reduces dict size by removing hash value from hash table when all inserted keys are Unicode. For example, `sys.getsizeof(dict.fromkeys("abcdefg"))` becomes 272 bytes from 352
bytes on 64bit platform.
- bpo-46841: Use inline cache for `LOAD_GLOBAL` .
- bpo-46852: Rename the private undocumented `float.__set_format__()` method to`float.__setformat__()` to fix a typo introduced in Python
3.7. The method is only used by test_float. Patch by Victor Stinner.
- bpo-46852: Remove the undocumented private `float.__set_format__()` method, previously known as`float.__setformat__()` in Python 3.7. Its
docstring said: âYou probably donât want to use this function. It exists
mainly to be used in Pythonâs test suite.â Patch by Victor Stinner.
- bpo-40116: Fix regression that dict.update(other) may donât respect iterate order of other when other is key sharing dict.
- bpo-46712: Share global string identifiers in deep-frozen modules.
- bpo-46430: Fix memory leak in interned strings of deep-frozen modules.
- bpo-46841: Store `BINARY_OP` caches inline using a new`CACHE` instruction.
- bpo-45107: Specialize `LOAD_METHOD` for instances with a dict.
- bpo-44337: Reduce the memory usage of specialized `LOAD_ATTR` and`STORE_ATTR` instructions.
- bpo-46729: Add number of sub-exceptions to `BaseException.__str__()` .
- bpo-45885: Donât un-adapt `COMPARE_OP` when collecting
specialization stats.
- bpo-46329: Fix specialization stats gathering for `PRECALL` instructions.

### LibraryÂ¶

- bpo-46794: Bump up the libexpat version into 2.4.6

### Core and BuiltinsÂ¶

- bpo-46823: Implement a specialized combined opcode `LOAD_FAST__LOAD_ATTR_INSTANCE_VALUE` .  Patch by Dennis Sweeney.
- bpo-46820: Fix parsing a numeric literal immediately (without spaces) followed by ânot inâ keywords, like in `1not in x` . Now the parser only
emits a warning, not a syntax error.
- bpo-46329: Move `KW_NAMES` before`PRECALL` instruction in call
sequence. Change`operand` of`CALL` to match`PRECALL` for easier
specialization.
- bpo-46808: Remove the `NEXT_BLOCK` macro from compile.c, and make the
compiler automatically generate implicit blocks when they are needed.
- bpo-46329: Add `PUSH_NULL` instruction. This is used as a prefix when
evaluating a callable, so that the stack has the same shape for methods
and other calls.`PRECALL_FUNCTION` and`PRECALL_METHOD` are merged
into a single`PRECALL` instruction.There is no change in semantics.
- bpo-46762: Fix an assert failure in debug builds when a â<â, â>â, or â=â is the last character in an f-string thatâs missing a closing right brace.
- bpo-46730: Message of AttributeError caused by getting, setting or deleting a property without the corresponding function now mentions that the attribute is in fact a property and also specifies type of the class that it belongs to.
- bpo-46724: Make sure that all backwards jumps use the `JUMP_ABSOLUTE` instruction, rather than`JUMP_FORWARD` with an argument of`(2**32)+offset` .
- bpo-46732: Correct the docstring for the `__bool__()` method.
Patch by Jelle Zijlstra.
- bpo-46072: Add more detailed specialization failure statistics for `BINARY_OP` .
- bpo-46707: Avoid potential exponential backtracking when producing some syntax errors involving lots of brackets. Patch by Pablo Galindo.

### LibraryÂ¶

### Core and BuiltinsÂ¶

- bpo-45923: Add a quickened form of `RESUME` that skips quickening
checks.
- bpo-46702: Specialize `UNPACK_SEQUENCE` for`tuple` and`list` unpackings.
- bpo-46072: Opcode pair stats are now gathered with `--enable-pystats` .
Defining`DYNAMIC_EXECUTION_PROFILE` or`DXPAIRS` no longer has any
effect.
- bpo-46675: Allow more than 16 items in a split dict before it is combined. The limit is now 254.

### LibraryÂ¶

- bpo-40479: Add a missing call to `va_end()` in`Modules/_hashopenssl.c` .
- bpo-46323: Use `PyObject_Vectorcall()` while calling ctypes callback
function. Patch by Donghee Na.

### Core and BuiltinsÂ¶

- bpo-46615: When iterating over sets internally in `setobject.c` , acquire
strong references to the resulting items from the set.  This prevents
crashes in corner-cases of various set operations where the set gets
mutated.
- bpo-45828: The bytecode compiler now attempts to apply runtime stack manipulations at compile-time (whenever it is feasible to do so).
- bpo-30496: Fixed a minor portability issue in the implementation of `PyLong_FromLong()` , and added a fast path for single-digit integers
to`PyLong_FromLongLong()` .

### LibraryÂ¶

- bpo-25707: Fixed a file leak in `xml.etree.ElementTree.iterparse()` when the iterator is not exhausted. Patch by Jacob Walls.
- bpo-46877: Export `unittest.doModuleCleanups()` in`unittest` .
Patch by Kumar Aditya.
- bpo-46848: For performance, use the optimized string-searching implementations from `find()` and`rfind()` for`find()` and`rfind()` .
- bpo-46736: `SimpleHTTPRequestHandler` now uses HTML5
grammar. Patch by Donghee Na.
- bpo-44886: Inherit asyncio proactor datagram transport from `asyncio.DatagramTransport` .
- bpo-46827: Support UDP sockets in `asyncio.loop.sock_connect()` for
selector-based event loops.  Patch by Thomas Grainger.
- bpo-46811: Make test suite support Expat >=2.4.5
- bpo-46252: Raise `TypeError` if`ssl.SSLSocket` is passed to
transport-based APIs.
- bpo-46784: Fix libexpat symbols collisions with user dynamically loaded or statically linked libexpat in embedded Python.
- bpo-46786: The HTML serialisation in xml.etree.ElementTree now writes `embed` ,`source` ,`track` and`wbr` as empty tags, as defined in
HTML 5.
- bpo-39327: `shutil.rmtree()` can now work with VirtualBox shared
folders when running from the guest operating-system.
- bpo-45390: Propagate `asyncio.CancelledError` message from inner task
to outer awaiter.
- bpo-46756: Fix a bug in `urllib.request.HTTPPasswordMgr.find_user_password()` and`urllib.request.HTTPPasswordMgrWithPriorAuth.is_authenticated()` which
allowed to bypass authorization. For example, access to URI`example.org/foobar` was allowed if the user was authorized for URI`example.org/foo` .
- bpo-46737: `random.gauss()` and`random.normalvariate()` now have
default arguments.
- bpo-46752: Add task groups to asyncio (structured concurrency, inspired by Trioâs nurseries). This also introduces a change to task cancellation, where a cancelled task canât be cancelled again until it calls .uncancel().
- bpo-46724: Fix `dis` behavior on negative jump offsets.
- bpo-46333: The `__repr__()` method of`typing.ForwardRef` now
includes the`module` parameter of`typing.ForwardRef` when it is
set.
- bpo-46643: In `typing.get_type_hints()` , support evaluating
stringified`ParamSpecArgs` and`ParamSpecKwargs` annotations. Patch
by Gregory Beauregard.
- bpo-45863: When the `tarfile` module creates a pax format archive, it
will put an integer representation of timestamps in the ustar header (if
possible) for the benefit of older unarchivers, in addition to the
existing full-precision timestamps in the pax extended header.
- bpo-46066: Deprecate kwargs-based syntax for `typing.TypedDict` definitions. It had confusing semantics when specifying totality, and was
largely unused. Patch by Jingchen Ye.
- bpo-46676: Make `typing.ParamSpec` args and kwargs equal to
themselves. Patch by Gregory Beauregard.
- bpo-46323: `ctypes.CFUNCTYPE()` and`ctypes.WINFUNCTYPE()` now fail to
create the type if its`_argtypes_` member contains too many arguments.
Previously, the error was only raised when calling a function. Patch by
Victor Stinner.
- bpo-46672: Fix `NameError` in`asyncio.gather()` when initial type
check fails.
- bpo-46659: The `calendar.LocaleTextCalendar` and`calendar.LocaleHTMLCalendar` classes now use`locale.getlocale()` , instead of using`locale.getdefaultlocale()` , if no locale is specified. Patch by
Victor Stinner.
- bpo-46659: The `locale.getdefaultlocale()` function is deprecated and
will be removed in Python 3.13. Use`locale.setlocale()` ,`locale.getpreferredencoding(False)` and`locale.getlocale()` functions instead.  Patch by Victor Stinner.
- bpo-46655: In `typing.get_type_hints()` , support evaluating bare
stringified`TypeAlias` annotations. Patch by Gregory Beauregard.
- bpo-45948: Fixed a discrepancy in the C implementation of the `xml.etree.ElementTree` module. Now, instantiating an`xml.etree.ElementTree.XMLParser` with a`target=None` keyword
provides a default`xml.etree.ElementTree.TreeBuilder` target as
the Python implementation does.
- bpo-46626: Expose Linuxâs `IP_BIND_ADDRESS_NO_PORT` option in`socket` .
- bpo-46521: Fix a bug in the `codeop` module that was incorrectly
identifying invalid code involving string quotes as valid code.
- bpo-46571: Improve `typing.no_type_check()` .Now it does not modify external classes and functions. We also now correctly mark classmethods as not to be type checked.
- bpo-46400: expat: Update libexpat from 2.4.1 to 2.4.4
- bpo-46556: Deprecate undocumented support for using a `pathlib.Path` object as a context manager.
- bpo-46534: Implement **PEP 673**`typing.Self` . Patch by James
Hilton-Balfe.
- bpo-46522: Make various module `__getattr__` AttributeErrors more
closely match a typical AttributeError
- bpo-46475: Add `typing.Never` and`typing.assert_never()` . Patch
by Jelle Zijlstra.
- bpo-46333: The `__eq__()` and`__hash__()` methods of`typing.ForwardRef` now honor the`module` parameter of`typing.ForwardRef` . Forward references from different modules are
now differentiated.
- bpo-46246: Add missing `__slots__` to`importlib.metadata.DeprecatedList` . Patch by Arie Bovenberg.
- bpo-46232: The `ssl` module now handles certificates with bit strings
in DN correctly.
- bpo-46195: `typing.get_type_hints()` no longer adds`Optional` to
parameters with`None` as a default. This aligns to changes to PEP 484
in https://github.com/python/peps/pull/689
- bpo-31369: Add `RegexFlag` to`re.__all__` and documented it.
Add`NOFLAG` to indicate no flags being set.
- bpo-45898: `ctypes` no longer defines`ffi_type_*` symbols in`cfield.c` . The symbols have been provided by libffi for over a decade.
- bpo-44953: Calling `operator.itemgetter` objects and`operator.attrgetter` objects is now faster due to use of the vectorcall
calling convention.
- bpo-44289: Fix an issue with `is_tarfile()` method when using*fileobj* argument: position in the*fileobj* was advanced forward which
made it unreadable with`tarfile.TarFile.open()` .
- bpo-44011: Reimplement SSL/TLS support in asyncio, borrow the implementation from uvloop library.
- bpo-41086: Make the `configparser.ConfigParser` constructor raise`TypeError` if the`interpolation` parameter is not of type`configparser.Interpolation`
- bpo-29418: Implement `inspect.ismethodwrapper()` and fix`inspect.isroutine()` for cases where methodwrapper is given. Patch by
Hakan Ãelik.
- bpo-14156: argparse.FileType now supports an argument of â-â in binary mode, returning the .buffer attribute of sys.stdin/sys.stdout as appropriate. Modes including âxâ and âaâ are treated equivalently to âwâ when argument is â-â. Patch contributed by Josh Rosenberg

### DocumentationÂ¶

- bpo-42238: `Doc/tools/rstlint.py` has moved to its own repository and is
now packaged on PyPI as`sphinx-lint` .

### TestsÂ¶

- bpo-46913: Fix test_faulthandler.test_sigfpe() if Python is built with undefined behavior sanitizer (UBSAN): disable UBSAN on the faulthandler_sigfpe() function. Patch by Victor Stinner.
- bpo-46760: Remove bytecode offsets from expected values in test.test_dis module. Reduces the obstacles to modifying the VM or compiler.
- bpo-46708: Prevent default asyncio event loop policy modification warning after `test_asyncio` execution.
- bpo-46678: The function `make_legacy_pyc` in`Lib/test/support/import_helper.py` no longer fails when`PYTHONPYCACHEPREFIX` is set to a directory on a different device from
where tempfiles are stored.
- bpo-46623: Skip test_pair() and test_speech128() of test_zlib on s390x since they fail if zlib uses the s390x hardware accelerator. Patch by Victor Stinner.

### BuildÂ¶

- bpo-46860: Respect `--with-suffix` when building on case-insensitive
file systems.
- bpo-46656: Building Python now requires a C11 compiler. Optional C11 features are not required. Patch by Victor Stinner.
- bpo-46656: Building Python now requires support for floating-point Not-a-Number (NaN): remove the `Py_NO_NAN` macro. Patch by Victor
Stinner.
- bpo-46640: Building Python now requires a C99 `<math.h>` header file
providing a`NAN` constant, or the`__builtin_nan()` built-in
function. Patch by Victor Stinner.
- bpo-46608: Exclude marshalled-frozen data if deep-freezing to save 300 KB disk space. This includes adding a new `is_package` field to`_frozen` . Patch by Kumar Aditya.
- bpo-40280: Fix wasm32-emscripten test failures and platform issues. - Disable syscalls that are not supported or donât work, e.g. wait, getrusage, prlimit, mkfifo, mknod, setres[gu]id, setgroups. - Use fd_count to count open fds. - Add more checks for subprocess and fork. - Add workarounds for missing _multiprocessing and failing socket.accept(). - Enable bzip2. - Disable large file support. - Disable signal.alarm.
- bpo-46430: Intern strings in deep-frozen modules. Patch by Kumar Aditya.

### WindowsÂ¶

- bpo-46744: The default all users install directory for ARM64 is now under the native `Program Files` folder, rather than`Program Files (Arm)` which is intended for ARM (32-bit) files.
- bpo-46567: Adds Tcl and Tk support for Windows ARM64. This also adds IDLE to the installation.
- bpo-46638: Ensures registry virtualization is consistently disabled. For 3.10 and earlier, it remains enabled (some registry writes are protected), while for 3.11 and later it is disabled (registry modifications affect all applications).

### IDLEÂ¶

### C APIÂ¶

- bpo-46748: Pythonâs public headers no longer import `<stdbool.h>` ,
leaving code that embeds/extends Python free to define`bool` ,`true` and`false` .
- bpo-46836: Move the `PyFrameObject` type definition (```
struct
_frame
```
) to the internal C API`pycore_frame.h` header file. Patch by
Victor Stinner.
- bpo-45459: Rename `Include/buffer.h` header file to`Include/pybuffer.h` to avoid conflicts with projects having an existing`buffer.h` header file. Patch by Victor Stinner.
- bpo-45412: Remove the `HAVE_PY_SET_53BIT_PRECISION` macro (moved to the
internal C API). Patch by Victor Stinner.
- bpo-46613: Added function `PyType_GetModuleByDef()` , which allows
access to module state when a methodâs defining class is not available.

## Python 3.11.0 alpha 5Â¶

*Release date: 2022-02-03*

### Core and BuiltinsÂ¶

- bpo-45773: Remove two invalid âpeepholeâ optimizations from the bytecode compiler.
- bpo-46564: Do not create frame objects when creating `super` object. Patch by Kumar Aditya.
- bpo-45885: Added more fined-grained specialization failure stats regarding the `COMPARE_OP` bytecode.
- bpo-44977: The delegation of `int()` to`__trunc__()` is now
deprecated. Calling`int(a)` when`type(a)` implements`__trunc__()` but not`__int__()` or`__index__()` now raises
a`DeprecationWarning` .
- bpo-46458: Reorder code emitted by the compiler for a `try` -`except` block so that the`else` blockâs
code immediately follows the`try` body (without a jump). This is
more optimal for the happy path.
- bpo-46527: Allow passing `iterable` as a keyword argument to`enumerate()` again. Patch by Jelle Zijlstra.
- bpo-46528: Replace several stack manipulation instructions ( `DUP_TOP` ,`DUP_TOP_TWO` ,`ROT_TWO` ,`ROT_THREE` ,`ROT_FOUR` , and`ROT_N` )
with new`COPY` and`SWAP` instructions.
- bpo-46329: Use two or three bytecodes to implement most calls. Calls without named arguments are implemented as a sequence of two instructions: `PRECALL; CALL` . Calls with named arguments are
implemented as a sequence of three instructions:```
PRECALL; KW_NAMES;
CALL
```
. There are two different`PRECALL` instructions:`PRECALL_FUNTION` and`PRECALL_METHOD` . The latter pairs with`LOAD_METHOD` .This partition into pre-call and call allows better specialization, and thus better performance ultimately. There is no change in semantics.
- bpo-46503: Fix an assert when parsing some invalid N escape sequences in f-strings.
- bpo-46431: Improve error message on invalid calls to `BaseExceptionGroup.__new__()` .
- bpo-46476: Fix memory leak in code objects generated by deepfreeze. Patch by Kumar Aditya.
- bpo-46481: Speed up calls to `weakref.ref.__call__()` by using the**PEP 590**`vectorcall` calling convention. Patch by Donghee Na.
- bpo-46417: Fix a race condition on setting a type `__bases__` attribute:
the internal function`add_subclass()` now gets the`PyTypeObject.tp_subclasses` member after calling`PyWeakref_NewRef()` which can trigger a garbage collection which
can indirectly modify`PyTypeObject.tp_subclasses` . Patch by Victor
Stinner.
- bpo-46417: `python -X showrefcount` now shows the total reference count
after clearing and destroyed the main Python interpreter. Previously, it
was shown before. Patch by Victor Stinner.
- bpo-43683: Add ASYNC_GEN_WRAP opcode to wrap the value to be yielded in async generators. Removes the need to special case async generators in the `YIELD_VALUE` instruction.
- bpo-46407: Optimize some modulo operations in `Objects/longobject.c` .
Patch by Jeremiah Vivian.
- bpo-46409: Add new `RETURN_GENERATOR` bytecode to make generators.
Simplifies calling Python functions in the VM, as they no longer any need
to special case generator functions.Also add `JUMP_NO_INTERRUPT` bytecode that acts like`JUMP_ABSOLUTE` ,
but does not check for interrupts.
- bpo-46406: The integer division `//` implementation has been optimized
to better let the compiler understand its constraints. It can be 20%
faster on the amd64 platform when dividing an int by a value smaller than`2**30` .

### LibraryÂ¶

- bpo-46383: Fix invalid signature of `_zoneinfo` âs`module_free` function to resolve a crash on wasm32-emscripten platform.

### Core and BuiltinsÂ¶

- bpo-46361: Ensure that âsmallâ integers created by `int.from_bytes()` and`decimal.Decimal` are properly cached.
- bpo-46161: Fix the class building error when the arguments are constants and CALL_FUNCTION_EX is used.
- bpo-46028: Fixes calculation of `sys._base_executable` when inside a
virtual environment that uses symlinks with different binary names than
the base environment provides.
- bpo-46091: Correctly calculate indentation levels for lines with whitespace character that are ended by line continuation characters. Patch by Pablo Galindo

### LibraryÂ¶

- bpo-30512: Add CAN Socket support for NetBSD.

### BuildÂ¶

- bpo-46045: Do not use POSIX semaphores on NetBSD

### Core and BuiltinsÂ¶

### LibraryÂ¶

- bpo-46624: Restore support for non-integer arguments of `random.randrange()` and`random.randint()` .
- bpo-46591: Make the IDLE doc URL on the About IDLE dialog clickable.
- bpo-46565: Remove loop variables that are leaking into modulesâ namespaces.
- bpo-46553: In `typing.get_type_hints()` , support evaluating bare
stringified`ClassVar` annotations. Patch by Gregory Beauregard.
- bpo-46544: Donât leak `x` &`uspace` intermediate vars in`textwrap.TextWrapper` .
- bpo-46487: Add the `get_write_buffer_limits` method to`asyncio.transports.WriteTransport` and to the SSL transport.
- bpo-45173: Note the configparser deprecations will be removed in Python 3.12.
- bpo-45162: The deprecated `unittest` APIs removed in 3.11a1 have been
temporarily restored to be removed in 3.12 while cleanups in external
projects go in.
- bpo-46539: In `typing.get_type_hints()` , support evaluating
stringified`ClassVar` and`Final` annotations inside`Annotated` .
Patch by Gregory Beauregard.
- bpo-46510: Add missing test for `types.TracebackType` and`types.FrameType` . Calculate them directly from the caught
exception without calling`sys.exc_info()` .
- bpo-46491: Allow `typing.Annotated` to wrap`typing.Final` and`typing.ClassVar` . Patch by Gregory Beauregard.
- bpo-46483: Remove `__class_getitem__()` from`pathlib.PurePath` as this class was not supposed to be generic.
- bpo-46436: Fix command-line option `-d` /`--directory` in module`http.server` which is ignored when combined with command-line option`--cgi` . Patch by GÃ©ry Ogam.
- bpo-41403: Make `mock.patch()` raise a`TypeError` with a
relevant error message on invalid arg. Previously it allowed a cryptic`AttributeError` to escape.
- bpo-46474: In `importlib.metadata.EntryPoint.pattern` , avoid potential
REDoS by limiting ambiguity in consecutive whitespace.
- bpo-46474: Removed private method from `importlib.metadata.Path` . Sync
with importlib_metadata 4.10.0.
- bpo-46470: Remove unused branch from `typing._remove_dups_flatten`
- bpo-46469: `asyncio` generic classes now return`types.GenericAlias` in`__class_getitem__` instead of the same
class.
- bpo-41906: Support passing filter instances in the `filters` values of`handlers` and`loggers` in the dictionary passed to`logging.config.dictConfig()` .
- bpo-46422: Use `dis.Positions` in`dis.Instruction` instead of a
regular`tuple` .
- bpo-46434: `pdb` now gracefully handles`help` when`__doc__` is missing, for example when run with pregenerated
optimized`.pyc` files.
- bpo-43869: Python uses the same time Epoch on all platforms. Add an explicit unit test to ensure that itâs the case. Patch by Victor Stinner.
- bpo-46414: Add `typing.reveal_type()` . Patch by Jelle Zijlstra.
- bpo-40280: `subprocess` now imports Windows-specific imports when`msvcrt` module is available, and POSIX-specific imports on all other
platforms. This gives a clean exception when`_posixsubprocess` is not
available (e.g. Emscripten browser target).
- bpo-40066: `IntEnum` ,`IntFlag` , and`StrEnum` use the mixed-in type
for their`str()` and`format()` output.
- bpo-46316: Optimize `pathlib.Path.iterdir()` by removing an
unnecessary check for special entries.
- bpo-29688: Document `pathlib.Path.absolute()` (which has always
existed).
- bpo-43012: The pathlib moduleâs obsolete and internal `_Accessor` class
has been removed to prepare the terrain for upcoming enhancements to the
module.
- bpo-46258: Speed up `math.isqrt()` for small positive integers by
replacing two division steps with a lookup table.
- bpo-46242: Improve error message when creating a new `enum.Enum` type subclassing an existing`Enum` with`_member_names_` using`enum.Enum.__call__()` .
- bpo-43118: Fix a bug in `inspect.signature()` that was causing it to
fail on some subclasses of classes with a`__text_signature__` referencing module globals. Patch by Weipeng Hong.
- bpo-26552: Fixed case where failing `asyncio.ensure_future()` did not
close the coroutine. Patch by Kumar Aditya.
- bpo-21987: Fix an issue with `tarfile.TarFile.getmember()` getting a
directory name with a trailing slash.
- bpo-46124: Update `zoneinfo` to rely on importlib.resources
traversable API.
- bpo-46103: Now `inspect.getmembers()` only gets`__bases__` attribute from class type. Patch by Weipeng Hong.
- bpo-46080: Fix exception in argparse help text generation if a `argparse.BooleanOptionalAction` argumentâs default is`argparse.SUPPRESS` and it has`help` specified.  Patch by Felix
Fontein.
- bpo-44791: Fix substitution of `ParamSpec` in`Concatenate` with different parameter expressions.
Substitution with a list of types returns now a tuple of types.
Substitution with`Concatenate` returns now a`Concatenate` with
concatenated lists of arguments.

### DocumentationÂ¶

- bpo-46463: Fixes `escape4chm.py` script used when building the CHM
documentation file

### TestsÂ¶

- bpo-43478: Mocks can no longer be provided as the specs for other Mocks. As a result, an already-mocked object cannot be passed to `mock.Mock()` .
This can uncover bugs in tests since these Mock-derived Mocks will always
pass certain tests (e.g. isinstance) and builtin assert functions (e.g.
assert_called_once_with) will unconditionally pass.
- bpo-46616: Ensures `test_importlib.test_windows` cleans up registry keys
after completion.
- bpo-44359: test_ftplib now silently ignores socket errors to prevent logging unhandled threading exceptions. Patch by Victor Stinner.
- bpo-46600: Fix test_gdb.test_pycfunction() for Python built with ```
clang
-Og
```
. Tolerate inlined functions in the gdb traceback. Patch by Victor
Stinner.
- bpo-46542: Fix a Python crash in test_lib2to3 when using Python built in debug mode: limit the recursion limit. Patch by Victor Stinner.
- bpo-46576: test_peg_generator now disables compiler optimization when testing compilation of its own C extensions to significantly speed up the testing on non-debug builds of CPython.
- bpo-46542: Fix `test_json` tests checking for`RecursionError` :
modify these tests to use`support.infinite_recursion()` . Patch by
Victor Stinner.
- bpo-13886: Skip test_builtin PTY tests on non-ASCII characters if the readline module is loaded. The readline module changes input() behavior, but test_builtin is not intended to test the readline module. Patch by Victor Stinner.
- bpo-40280: Add `test.support.requires_fork()` decorators to mark tests
that require a working`os.fork()` .
- bpo-40280: Add `test.support.requires_subprocess()` decorator to mark
tests which require working`subprocess` module or`os.spawn*` . The
wasm32-emscripten platform has no support for processes.
- bpo-46126: Disable âdescriptionsâ when running tests internally.

### BuildÂ¶

- bpo-46602: Tidied up configure.ac so that conftest.c is truncated rather than appended. This assists in the case where the ârmâ of conftest.c fails to happen between tests. Downstream issues such as a clobbered SOABI can result.
- bpo-46600: Fix the test checking if the C compiler supports `-Og` option
in the`./configure` script to also use`-Og` on clang which supports
it. Patch by Victor Stinner.
- bpo-38472: Fix GCC detection in setup.py when cross-compiling. The C compiler is now run with LC_ALL=C. Previously, the detection failed with a German locale.
- bpo-46513: **configure** no longer uses`AC_C_CHAR_UNSIGNED` macro and`pyconfig.h` no longer defines reserved symbol`__CHAR_UNSIGNED__` .
- bpo-46471: Use global singletons for single byte bytes objects in deepfreeze.
- bpo-46443: Deepfreeze now uses cached small integers as it saves some space for common small integers.
- bpo-46429: Merge all deep-frozen files into one for space savings. Patch by Kumar Aditya.
- bpo-45569: The build now defaults to using 30-bit digits for Python integers. Previously either 15-bit or 30-bit digits would be selected, depending on the platform. 15-bit digits may still be selected using the `--enable-big-digits=15` option to the`configure` script, or by
defining`PYLONG_BITS_IN_DIGIT` in`pyconfig.h` .
- bpo-45925: Update Windows installer to use SQLite 3.37.2.
- bpo-43112: Detect musl libc as a separate SOABI (tagged as `linux-musl` ).

### WindowsÂ¶

- bpo-33125: The traditional EXE/MSI based installer for Windows is now available for ARM64
- bpo-46362: os.path.abspath(âC:CONâ) is now fixed to return â\.CONâ, not the same path. The regression was true of all legacy DOS devices such as COM1, LPT1, or NUL.
- bpo-44934: The installer now offers a command-line only option to add the installation directory to the end of `PATH` instead of at the
start.

### macOSÂ¶

- bpo-45925: Update macOS installer to SQLite 3.37.2.

### IDLEÂ¶

- bpo-45296: Clarify close, quit, and exit in IDLE. In the File menu, âCloseâ and âExitâ are now âClose Windowâ (the current one) and âExitâ is now âExit IDLEâ (by closing all windows). In Shell, âquit()â and âexit()â mean âclose Shellâ. If there are no other windows, this also exits IDLE.

### C APIÂ¶

- bpo-40170: Remove the `PyHeapType_GET_MEMBERS()` macro. It was exposed
in the public C API by mistake, it must only be used by Python internally.
Use the`PyTypeObject.tp_members` member instead. Patch by Victor
Stinner.
- bpo-40170: Move _Py_GetAllocatedBlocks() and _PyObject_DebugMallocStats() private functions to the internal C API. Patch by Victor Stinner.
- bpo-46433: The internal function _PyType_GetModuleByDef now correctly handles inheritance patterns involving static types.
- bpo-45459: `Py_buffer` and various`Py_buffer` related functions
are now part of the limited API and stable ABI.
- bpo-14916: Fixed bug in the tokenizer that prevented `PyRun_InteractiveOne` from parsing from the provided FD.

## Python 3.11.0 alpha 4Â¶

*Release date: 2022-01-13*

### Core and BuiltinsÂ¶

- bpo-46070: `Py_EndInterpreter()` now explicitly untracks all objects
currently tracked by the GC. Previously, if an object was used later by
another interpreter, calling`PyObject_GC_UnTrack()` on the object
crashed if the previous or the next object of the`PyGC_Head` structure became a dangling pointer. Patch by Victor Stinner.
- bpo-46347: Fix memory leak in PyEval_EvalCodeEx.
- bpo-46339: Fix a crash in the parser when retrieving the error text for multi-line f-strings expressions that do not start in the first line of the string. Patch by Pablo Galindo
- bpo-46331: Do not set line number of instruction storing doc-string. Fixes regression introduced in 3.11 alpha.
- bpo-46314: Remove spurious âcallâ event when creating a lambda function that was accidentally introduced in 3.11a4.
- bpo-46289: ASDL declaration of `FormattedValue` has changed to reflect`conversion` field is not optional.
- bpo-46297: Fixed an interpreter crash on bootup with multiple PythonPaths set in the Windows registry. Patch by Derzsi DÃ¡niel.
- bpo-46237: Fix the line number of tokenizer errors inside f-strings. Patch by Pablo Galindo.
- bpo-46263: We always expect the âuse_frozen_modulesâ config to be set, now that getpath.c was rewritten in pure Python and the logic improved.
- bpo-46006: Fix a regression when a type method like `__init__()` is
modified in a subinterpreter. Fix a regression in`_PyUnicode_EqualToASCIIId()` and type`update_slot()` . Revert the
change which made the Unicode dictionary of interned strings compatible
with subinterpreters: the internal interned dictionary is shared again by
all interpreters. Patch by Victor Stinner.
- bpo-45923: Add RESUME opcode. This is a logical no-op. It is emitted by the compiler anywhere a Python function can be entered. It is used by the interpreter to perform tracing and optimizer checks.

### LibraryÂ¶

- bpo-46208: Fix the regression of os.path.normpath(âA/../../Bâ) not returning expected â../Bâ but âBâ.

### Core and BuiltinsÂ¶

- bpo-46240: Correct the error message for unclosed parentheses when the tokenizer doesnât reach the end of the source when the error is reported. Patch by Pablo Galindo
- bpo-46009: Remove the `GEN_START` opcode.
- bpo-46235: Certain sequence multiplication operations like `[0] * 1_000` are now faster due to reference-counting optimizations. Patch by Dennis
Sweeney.
- bpo-46221: `PREP_RERAISE_STAR` no longer pushes`lasti` to the
stack.
- bpo-46202: Remove `POP_EXCEPT_AND_RERAISE` and replace it by an
equivalent sequence of other opcodes.

### LibraryÂ¶

- bpo-46085: Fix iterator cache mechanism of `OrderedDict` .

### Core and BuiltinsÂ¶

- bpo-46055: Speed up shifting operation involving integers less than `PyLong_BASE` . Patch by Xinhang Xu.
- bpo-46110: Add a maximum recursion check to the PEG parser to avoid stack overflow. Patch by Pablo Galindo
- bpo-46107: Fix bug where `ExceptionGroup.split()` and`ExceptionGroup.subgroup()` did not copy the exception groupâs`__note__` field to the parts.
- bpo-45711: The interpreter stateâs representation of handled exceptions (a.k.a exc_info, or _PyErr_StackItem) now has only the `exc_value` field,`exc_type` and`exc_traceback` have been removed as their
values can be derived from`exc_value` .
- bpo-44525: Replace the four call bytecode instructions which one pre-call instruction and two call instructions. Removes `CALL_FUNCTION` ,`CALL_FUNCTION_KW` ,`CALL_METHOD` and`CALL_METHOD_KW` .Adds `CALL_NO_KW` and`CALL_KW` call instructions, and`PRECALL_METHOD` prefix for pairing with`LOAD_METHOD` .
- bpo-46039: Remove the `YIELD_FROM` instruction and replace it with the`SEND` instruction which performs the same operation, but without the
loop.
- bpo-45635: The code called from `_PyErr_Display()` was refactored
to improve error handling. It now exits immediately upon an unrecoverable
error.
- bpo-46054: Fix parser error when parsing non-utf8 characters in source files. Patch by Pablo Galindo.
- bpo-46042: Improve the location of the caret in `SyntaxError` exceptions emitted by the symbol table. Patch by Pablo Galindo.
- bpo-46049: Ensure `._pth` files work as intended on platforms other
than Windows.
- bpo-46048: Fixes parsing of `._pth` files on startup so that
single-character paths are correctly read.
- bpo-37971: Fix a bug where the line numbers given in a traceback when a decorator application raised an exception were wrong.
- bpo-46031: Add `POP_JUMP_IF_NOT_NONE` and`POP_JUMP_IF_NONE` opcodes to speed up conditional jumps.
- bpo-45654: Deepfreeze `runpy` , patch by Kumar Aditya.
- bpo-46025: Fix a crash in the `atexit` module involving functions
that unregister themselves before raising exceptions. Patch by Pablo
Galindo.

### LibraryÂ¶

### Core and BuiltinsÂ¶

- bpo-44525: Specialize the CALL_FUNCTION instruction for calls to builtin types with a single argument. Speeds up `range(x)` ,`list(x)` , and
specifically`type(obj)` .
- bpo-42918: Fix bug where the built-in `compile()` function did not
always raise a`SyntaxError` when passed multiple statements in
âsingleâ mode. Patch by Weipeng Hong.
- bpo-45953: The main interpreter in _PyRuntimeState.interpreters is now statically allocated (as part of _PyRuntime). Likewise for the initial thread state of each interpreter. This means less allocation during runtime init, as well as better memory locality for these key state objects.
- bpo-45292: Complete the **PEP 654** implementation: add`except*` .
- bpo-43413: Revert changes in `set.__init__` . Subclass of`set` needs to define a`__init__()` method if it defines a`__new__()` method with additional keyword parameters.
- bpo-43931: Added the `Py_Version` constant which bears the same
value as`PY_VERSION_HEX` . Patch by Gabriele N. Tornetta.

### LibraryÂ¶

- bpo-46342: The `@typing.final` decorator now sets the`__final__` attribute on the decorated object to allow runtime introspection. Patch by
Jelle Zijlstra.
- bpo-46328: Added the `sys.exception()` method which returns the active
exception instance.
- bpo-46307: Add `string.Template.is_valid()` and`string.Template.get_identifiers()` methods.
- bpo-46306: Assume that `types.CodeType` always has`types.CodeType.co_firstlineno` in`doctest` .
- bpo-40479: Fix `hashlib`*usedforsecurity* option to work correctly
with OpenSSL 3.0.0 in FIPS mode.
- bpo-46070: Fix possible segfault when importing the `asyncio` module
from different sub-interpreters in parallel. Patch by Erlend E. Aasland.
- bpo-46244: Removed `__slots__` from`typing.ParamSpec` and`typing.TypeVar` . They served no purpose. Patch by Arie Bovenberg.
- bpo-46278: Reflect `context` argument in`AbstractEventLoop.call_*()` methods. Loop implementations already support it.
- bpo-46269: Remove special-casing of `__new__` in`enum.Enum.__dir__()` .
- bpo-46266: Improve day constants in `calendar` .Now all constants ( `MONDAY` â¦`SUNDAY` ) are documented, tested, and
added to`__all__` .
- bpo-46257: Optimized the mean, variance, and stdev functions in the statistics module. If the input is an iterator, it is consumed in a single pass rather than eating memory by conversion to a list. The single pass algorithm is about twice as fast as the previous two pass code.
- bpo-41011: Added two new variables to *pyvenv.cfg* which is generated by`venv` module:*executable* for the executable and*command* for the
command line used to create the environment.
- bpo-46239: Improve error message when importing `asyncio.windows_events` on non-Windows.
- bpo-46238: Reuse `_winapi` constants in`asyncio.windows_events` .
- bpo-46222: Adding `SF_NOCACHE` sendfile constant for FreeBSD for the
posixmodule.
- bpo-37295: Add fast path for `0 <= k <= n <= 67` for`math.comb()` .
- bpo-46176: Adding the `MAP_STACK` constant for the mmap module.
- bpo-43424: Deprecate `webbrowser.MacOSXOSAScript._name` and use`name` instead.
- bpo-45321: Added missing error codes to module `xml.parsers.expat.errors` .
- bpo-46125: Refactor tests to test traversable API directly. Includes changes from importlib 5.4.0.
- bpo-46118: Moved importlib.resources and its related functionality to a package.
- bpo-37578: Add *include_hidden* parameter to`glob()` and`iglob()` to match hidden files and directories when using
special characters like`*` ,`**` ,`?` and`[]` .
- bpo-20369: `concurrent.futures.wait()` no longer blocks forever when
given duplicate Futures. Patch by Kumar Aditya.
- bpo-46105: Honor spec when generating requirement specs with urls and extras (importlib_metadata 4.8.3).
- bpo-44893: EntryPoint objects are no longer tuples. Recommended means to access is by attribute (â.nameâ, â.groupâ) or accessor (â.load()â). Access by index is deprecated and will raise deprecation warning.
- bpo-22815: Print unexpected successes together with failures and errors in summary in `unittest.TextTestResult` .
- bpo-22047: Calling `add_argument_group()` on an argument group is
deprecated. Calling`add_argument_group()` or`add_mutually_exclusive_group()` on a mutually exclusive group is
deprecated.These features were never supported and do not always work correctly. The functions exist on the API by accident through inheritance and will be removed in the future.
- bpo-26952: `argparse` raises`ValueError` with clear message
when trying to render usage for an empty mutually exclusive group.
Previously it raised a cryptic`IndexError` .
- bpo-45615: Functions in the `traceback` module raise`TypeError` rather than`AttributeError` when an exception argument is not of
type`BaseException` .
- bpo-16594: Add allow allow_reuse_port flag in socketserver.
- bpo-27718: Fix help for the `signal` module. Some functions (e.g.`signal()` and`getsignal()` ) were omitted.
- bpo-46032: The `registry()` method of`functools.singledispatch()` functions checks now the first argument or the first parameter annotation
and raises a TypeError if it is not supported. Previously unsupported
âtypesâ were ignored (e.g.`typing.List[int]` ) or caused an error at
calling time (e.g.`list[int]` ).
- bpo-46014: Add ability to use `typing.Union` and`types.UnionType` as
dispatch argument to`functools.singledispatch` . Patch provided by Yurii
Karabas.
- bpo-27062: Add `__all__` to`inspect` , patch by Kumar Aditya.
- bpo-46018: Ensure that `math.expm1()` does not raise on underflow.
- bpo-46016: Adding `F_DUP2FD` and`F_DUP2FD_CLOEXEC` constants from FreeBSD into the`fcntl` module.
- bpo-45755: `typing` generic aliases now reveal the class attributes
of the original generic class when passed to`dir()` . This was the
behavior up to Python 3.6, but was changed in 3.7-3.9.
- bpo-45874: The empty query string, consisting of no query arguments, is now handled correctly in `urllib.parse.parse_qsl` . This caused problems
before when strict parsing was enabled.
- bpo-44674: Change how dataclasses disallows mutable default values. It used to use a list of known types (list, dict, set). Now it disallows unhashable objects to be defaults. Itâs using unhashability as a proxy for mutability. Patch by Eric V. Smith, idea by Raymond Hettinger.
- bpo-23882: Remove namespace package (PEP 420) support from unittest discovery. It was introduced in Python 3.4 but has been broken since Python 3.7.
- bpo-25066: Added a `__repr__()` method to`multiprocessing.Event` objects, patch by Kumar Aditya.
- bpo-45643: Added `signal.SIGSTKFLT` on platforms where this signal
is defined.
- bpo-44092: Fetch across rollback no longer raises `InterfaceError` . Instead we leave it to the SQLite library
to handle these cases. Patch by Erlend E. Aasland.
- bpo-42413: Replace `concurrent.futures.TimeoutError` and`asyncio.TimeoutError` with builtin`TimeoutError` , keep these
names as deprecated aliases.

### DocumentationÂ¶

### TestsÂ¶

- bpo-46296: Add a test case for `enum` with`_use_args_ == True` and`_member_type_ == object` .
- bpo-46205: Fix hang in runtest_mp due to race condition
- bpo-46263: Fix test_capi on FreeBSD 14-dev: instruct jemalloc to not fill freed memory with junk byte.
- bpo-46262: Cover `ValueError` path in tests for`enum.Flag._missing_()` .
- bpo-46150: Now `fakename` in`test_pathlib.PosixPathTest.test_expanduser` is checked to be
non-existent.
- bpo-46129: Rewrite `asyncio.locks` tests with`unittest.IsolatedAsyncioTestCase` usage.
- bpo-23819: Fixed `asyncio` tests in python optimized mode. Patch by
Kumar Aditya.
- bpo-46114: Fix test case for OpenSSL 3.0.1 version. OpenSSL 3.0 uses `0xMNN00PP0L` .

### BuildÂ¶

- bpo-44133: When Python is configured with `--without-static-libpython` , the Python static library
(libpython.a) is no longer built. Patch by Victor Stinner.
- bpo-44133: When Python is built without `--enable-shared` , the`python` program is now linked to object files, rather than being linked
to the Python static library (libpython.a), to make sure that all symbols
are exported. Previously, the linker omitted some symbols like the`Py_FrozenMain()` function. Patch by Victor Stinner.
- bpo-40280: The `configure` script has a new option`--with-emscripten-target` to select browser or node as Emscripten build
target.
- bpo-46315: Added and fixed `#ifdef HAVE_FEATURE` checks for
functionality that is not available on WASI platform.
- bpo-45723: Fixed a regression in `configure` check for`select.epoll()` .
- bpo-46263: `configure` no longer sets`MULTIARCH` on FreeBSD
platforms.
- bpo-46106: Updated OpenSSL to 1.1.1m in Windows builds, macOS installer builds, and CI. Patch by Kumar Aditya.
- bpo-46088: Automatically detect or install bootstrap Python runtime when building from Visual Studio.
- bpo-46072: Add a âwith-pystats configure option to turn on internal statistics gathering.
- bpo-40280: A new directory `Tools/wasm` contains WebAssembly-related
helpers like`config.site` override for wasm32-emscripten, wasm assets
generator to bundle the stdlib, and a README.
- bpo-46023: **makesetup** no longer builds extensions that have been
marked as*disabled* . This allows users to disable modules in`Modules/Setup.local` .
- bpo-45949: Use pure Python `freeze_module` for all but importlib
bootstrap files.`--with-freeze-module`**configure** option is
no longer needed for cross builds.

### WindowsÂ¶

- bpo-46217: Removed parameter that is unsupported on Windows 8.1 and early Windows 10 and may have caused build or runtime failures.

### macOSÂ¶

- bpo-40477: The Python Launcher app for macOS now properly launches scripts and, if necessary, the Terminal app when running on recent macOS releases.

### C APIÂ¶

- bpo-46236: Fix a bug in `PyFunction_GetAnnotations()` that caused it
to return a`tuple` instead of a`dict` .
- bpo-46140: `PyBuffer_GetPointer()` ,`PyBuffer_FromContiguous()` ,`PyBuffer_ToContiguous()` and`PyMemoryView_FromBuffer()` now take buffer info by```
const
Py_buffer *
```
instead of`Py_buffer *` , as they do not need mutability.`PyBuffer_FromContiguous()` also now takes the source buffer as`const void *` , and similarly`PyBuffer_GetPointer()` takes the
strides as`const Py_ssize_t *` .
- bpo-45855: Document that the *no_block* argument to`PyCapsule_Import()` is a no-op now.
- bpo-45855: Replaced deprecated usage of `PyImport_ImportModuleNoBlock()` with`PyImport_ImportModule()` in stdlib modules. Patch by Kumar Aditya.
- bpo-46007: The `PyUnicode_CHECK_INTERNED()` macro has been excluded
from the limited C API. It was never usable there, because it used
internal structures which are not available in the limited C API. Patch by
Victor Stinner.

## Python 3.11.0 alpha 3Â¶

*Release date: 2021-12-08*

### Core and BuiltinsÂ¶

- bpo-46009: Restore behavior from 3.9 and earlier when sending non-None to newly started generator. In 3.9 this did not affect the state of the generator. In 3.10.0 and 3.10.1 `gen_func().send(0)` is equivalent to`gen_func().throw(TypeError(...)` which exhausts the generator. In
3.10.2 onward, the behavior has been reverted to that of 3.9.
- bpo-46004: Fix the `SyntaxError` location for errors involving for
loops with invalid targets. Patch by Pablo Galindo
- bpo-45711: `_PyErr_ChainStackItem()` no longer normalizes`exc_info` (including setting the traceback on the exception instance)
because`exc_info` is always normalized.
- bpo-45607: The `__note__` field was added to`BaseException` . It is`None` by default but can be set to a string which is added to the
exceptionâs traceback.
- bpo-45947: Place pointers to dict and values immediately before GC header. This reduces number of dependent memory loads to access either dict or values from 3 to 1.
- bpo-45915: `is_valid_fd` now uses faster`fcntl(fd, F_GETFD)` on
Linux, macOS, and Windows.
- bpo-44530: Reverts a change to the `code.__new__` audit event from an earlier prerelease.
- bpo-42268: Fail the configure step if the selected compiler doesnât support memory sanitizer. Patch by Pablo Galindo
- bpo-45711: The three values of `exc_info` are now always consistent with
each other. In particular, the`type` and`traceback` fields are now
derived from the exception instance. This impacts the return values of`sys.exc_info()` and`PyErr_GetExcInfo()` if the exception
instance is modified while the exception is handled, as well as`PyErr_SetExcInfo()` , which now ignores the`type` and`traceback` arguments provided to it.
- bpo-45727: Refine the custom syntax error that suggests that a comma may be missing to trigger only when the expressions are detected between parentheses or brackets. Patch by Pablo Galindo
- bpo-45885: Specialized the `COMPARE_OP` opcode using the PEP 659
machinery.
- bpo-45786: Allocate space for the interpreter frame in the frame object, to avoid an additional allocation when the frame object outlives the frame activation.

### LibraryÂ¶

### Core and BuiltinsÂ¶

- bpo-45813: Fix crash when calling coro.cr_frame.clear() after coroutine has been freed.
- bpo-45811: Improve the tokenizer errors when encountering invisible control characters in the parser. Patch by Pablo Galindo
- bpo-45848: Allow the parser to obtain error lines directly from encoded files. Patch by Pablo Galindo
- bpo-45709: Restore behavior from 3.10 when tracing an exception raised within a with statement.
- bpo-44525: Adds new `COPY_FREE_VARS` opcode, to make copying of
free variables from function to frame explicit. Helps optimization of
calls to Python function.
- bpo-45829: Specialize `BINARY_SUBSCR` for classes with a`__getitem__` method implemented in Python
- bpo-45826: Fixed a crash when calling `.with_traceback(None)` on`NameError` . This occurs internally in`unittest.TestCase.assertRaises()` .
- bpo-45822: Fixed a bug in the parser that was causing it to not respect **PEP 263** coding cookies when no flags are provided. Patch by Pablo
Galindo
- bpo-45820: Fix a segfault when the parser fails without reading any input. Patch by Pablo Galindo
- bpo-45636: Simplify the implementation of `BINARY_OP` by indexing
into an array of function pointers (rather than switching on the oparg).
- bpo-42540: Fix crash when `os.fork()` is called with an active
non-default memory allocator.
- bpo-45738: Fix computation of error location for invalid continuation characters in the parser. Patch by Pablo Galindo.
- bpo-45636: Remove an existing âfast pathâ for old-style string formatting, since it no longer appears to have any measurable impact.
- bpo-45753: Make recursion checks a bit more efficient by tracking amount of calls left before overflow.
- bpo-45773: Fix a compiler hang when attempting to optimize certain jump patterns.
- bpo-45764: The parser now gives a better error message when leaving out the opening parenthesis `(` after a`def` -statement:>>> def f: File "<stdin>", line 1 def f: ^ SyntaxError: expected '('
- bpo-45609: Specialized the `STORE_SUBSCR` opcode using the PEP 659
machinery.
- bpo-45636: Replace all numeric `BINARY_*` and`INPLACE_*` instructions
with a single`BINARY_OP` implementation.
- bpo-45582: Path calculation (known as `getpath` ) has been reimplemented
as a frozen Python module. This should have no visible impact, but may
affect calculation of all paths referenced in`sys` and`sysconfig` .
- bpo-45450: Improve the syntax error message for parenthesized arguments. Patch by Pablo Galindo.

### LibraryÂ¶

- bpo-27946: Fix possible crash when getting an attribute of `xml.etree.ElementTree.Element` simultaneously with replacing the`attrib` dict.
- bpo-45711: Make `asyncio` normalize exceptions as soon as they are
captured with`PyErr_Fetch()` , and before they are stored as an
exc_info triplet. This brings`asyncio` in line with the rest of the
codebase, where an exc_info triplet is always normalized.
- bpo-23819: Replaced asserts with exceptions in asyncio, patch by Kumar Aditya.
- bpo-13236: `unittest.TextTestResult` and`unittest.TextTestRunner` flush now the output stream more often.
- bpo-45917: Added `math.exp2()` :, which returns 2 raised to the power
of x.
- bpo-37658: Fix issue when on certain conditions `asyncio.wait_for()` may
allow a coroutine to complete successfully, but fail to return the result,
potentially causing memory leaks or other issues.
- bpo-45876: Improve the accuracy of stdev() and pstdev() in the statistics module. When the inputs are floats or fractions, the output is a correctly rounded float
- bpo-44649: Handle dataclass(slots=True) with a field that has default a default value, but for which init=False.
- bpo-45803: Added missing kw_only parameter to dataclasses.make_dataclass().
- bpo-45837: The `turtle.RawTurtle.settiltangle()` is deprecated since
Python 3.1, it now emits a deprecation warning and will be removed in
Python 3.13.Use `turtle.RawTurtle.tiltangle()` instead.`turtle.RawTurtle.tiltangle()` was earlier incorrectly marked as
deprecated, its docstring has been corrected.Patch by Hugo van Kemenade.
- bpo-45831: `faulthandler` can now write ASCII-only strings (like
filenames and function names) with a single write() syscall when dumping a
traceback. It reduces the risk of getting an unreadable dump when two
threads or two processes dump a traceback to the same file (like stderr)
at the same time. Patch by Victor Stinner.
- bpo-45828: `sqlite` C callbacks now use unraisable exceptions if
callback tracebacks are enabled. Patch by Erlend E. Aasland.
- bpo-41735: Fix thread lock in `zlib.Decompress.flush()` method before`PyObject_GetBuffer` .
- bpo-45235: Reverted an argparse bugfix that caused regression in the handling of default arguments for subparsers. This prevented leaf level arguments from taking precedence over root level arguments.
- bpo-45754: Fix a regression in Python 3.11a1 and 3.11a2 where `sqlite3` incorrectly would use`SQLITE_LIMIT_LENGTH` when checking
SQL statement lengths. Now,`SQLITE_LIMIT_SQL_LENGTH` is used. Patch by
Erlend E. Aasland.
- bpo-45766: Added *proportional* option to`statistics.linear_regression()` .
- bpo-45765: In importlib.metadata, fix distribution discovery for an empty path.
- bpo-45757: Fix bug where `dis` produced an incorrect oparg when`EXTENDED_ARG` is followed by an opcode that does not use its
argument.
- bpo-45644: In-place JSON file formatting using ```
python3 -m json.tool
infile infile
```
now works correctly, previously it left the file empty.
Patch by Chris Wesseling.
- bpo-45703: When a namespace package is imported before another module from the same namespace is created/installed in a different `sys.path` location while the program is running, calling the`importlib.invalidate_caches()` function will now also guarantee the
new module is noticed.
- bpo-45535: Improve output of `dir()` with Enums.
- bpo-45664: Fix `types.resolve_bases()` and`types.new_class()` for`types.GenericAlias` instance as a base.
- bpo-45663: Fix `dataclasses.is_dataclass()` for dataclasses which are
subclasses of`types.GenericAlias` .
- bpo-45662: Fix the repr of `dataclasses.InitVar` with a type alias
to the built-in class, e.g.`InitVar[list[int]]` .
- bpo-43137: Launch GNOME web browsers via gio tool instead of obsolete gvfs-open
- bpo-45429: On Windows, `time.sleep()` now uses a waitable timer which
supports high-resolution timers. Patch by Donghee Na and Eryk Sun.
- bpo-37295: Optimize `math.comb()` and`math.perm()` .
- bpo-45514: Deprecated legacy functions in `importlib.resources` .
- bpo-45507: Add tests for truncated/missing trailers in gzip.decompress implementation.
- bpo-45359: Implement **PEP 585** for`graphlib.TopologicalSorter` .
- bpo-44733: Add `max_tasks_per_child` to`concurrent.futures.ProcessPoolExecutor` . This allows users to
specify the maximum number of tasks a single process should execute before
the process needs to be restarted.
- bpo-28806: Improve netrc library. netrc file no longer needs to contain all tokens. And if the login name is anonymous, security check is no longer need.
- bpo-43498: Avoid a possible *âRuntimeError: dictionary changed size during
iterationâ* when adjusting the process count of`ProcessPoolExecutor` .
- bpo-42158: Add MIME types for N-quads, N-triples, Notation3 and TriG to `mimetypes` .
- bpo-30533: Add `inspect.getmembers_static()` , it return all members
without triggering dynamic lookup via the descriptor protocol. Patch by
Weipeng Hong.

### DocumentationÂ¶

- bpo-42238: `make -C Doc suspicious` will be removed soon in favor of`make -C Doc check` , mark it as deprecated.
- bpo-45840: Improve cross-references in the documentation for the data model.
- bpo-45640: Properly marked-up grammar tokens in the documentation are now clickable and take you to the definition of a given piece of grammar. Patch by Arthur Milchior.
- bpo-45788: Link doc for sys.prefix to sysconfig doc on installation paths.
- bpo-45772: `socket.socket` documentation is corrected to a class from a
function.
- bpo-45392: Update the docstring of the `type` built-in to remove a
redundant line and to mention keyword arguments for the constructor.
- bpo-45250: Update the documentation to note that CPython does not consistently require iterators to define `__iter__` .
- bpo-25381: In the extending chapter of the extending doc, update a paragraph about the global variables containing exception information.
- bpo-43905: Expanded `astuple()` and`asdict()` docs, warning about deepcopy being applied and
providing a workaround.

### TestsÂ¶

- bpo-45695: Out-of-tree builds with a read-only source directory are now tested by CI.
- bpo-19460: Add new Test for `Lib/email/mime/nonmultipart.py::MIMENonMultipart` .
- bpo-45835: Fix race condition in test_queue tests with multiple âfeederâ threads.
- bpo-45783: The test for the freeze tool now handles file moves and deletions.
- bpo-45745: Remove the `--findleaks` command line option of regrtest: use
the`--fail-env-changed` option instead. Since Python 3.7, it was a
deprecated alias to the`--fail-env-changed` option.
- bpo-45701: Add tests with `tuple` type with`functools.lru_cache()` to`test_functools` .

### BuildÂ¶

- bpo-44035: CI now verifies that autoconf files have been regenerated with a current and unpatched autoconf package.
- bpo-45950: The build system now uses a **_bootstrap_python** interpreter for freezing and deepfreezing again. To speed up build process
the build tools**_bootstrap_python** and**_freeze_module** are no longer build with LTO.
- bpo-45881: The **configure** script now accepts`--with-build-python` and`--with-freeze-module` options to make cross
compiling easier.
- bpo-40280: Emscripten platform now uses `.wasm` suffix by default.
- bpo-40280: Disable unusable core extension modules on WASM/Emscripten targets.
- bpo-40280: `configure` now checks for socket`shutdown` function. The
check makes it possible to disable`SYS_shutdown` with`ac_cv_func_shutdown=no` in CONFIG_SITE.
- bpo-40280: `configure` now checks for functions```
fork1, getegid,
geteuid, getgid, getppid, getuid, opendir, pipe, system, wait, ttyname
```
.
- bpo-33393: Update `config.guess` to 2021-06-03 and`config.sub` to
2021-08-14.`Makefile` now has an`update-config` target to make
updating more convenient.
- bpo-45866: `make regen-all` now produces the same output when run from a
directory other than the source tree: when building Python out of the
source tree. pegen now strips directory of the âgenerated by pygen from
<FILENAME>â header Patch by Victor Stinner.
- bpo-40280: `configure` now accepts machine`wasm32` or`wasm64` and
OS`wasi` or`emscripten` for cross building, e.g.`wasm32-unknown-emscripten` ,`wasm32-wasi` , or`wasm32-unknown-wasi` .
- bpo-41498: Python now compiles on platforms without `sigset_t` . Several
functions in`signal` are not available when`sigset_t` is missing.Based on patch by Roman Yurchak for pyodide.
- bpo-45881: `setup.py` now uses`CC` from environment first to discover
multiarch and cross compile paths.
- bpo-45886: The `_freeze_module` program path can now be overridden on
the command line, e.g.```
make
FREEZE_MODULE=../x86_64/Program/_freeze_module
```
.
- bpo-45873: Get rid of the `_bootstrap_python` build step. The
deepfreeze.py script is now run using`$(PYTHON_FOR_REGEN)` which can be
Python 3.7 or newer (on Windows, 3.8 or newer).
- bpo-45847: Port builtin hashlib extensions to `PY_STDLIB_MOD` macro and`addext()` .
- bpo-45723: Add `autoconf` helpers for saving and restoring environment
variables:
  - `SAVE_ENV` : Save`$CFLAGS` ,`$LDFLAGS` ,`$LIBS` , and`$CPPFLAGS` .
  - `RESTORE_ENV` : Restore`$CFLAGS` ,`$LDFLAGS` ,`$LIBS` , and`$CPPFLAGS` .
  - `WITH_SAVE_ENV([SCRIPT])` : Run`SCRIPT` wrapped with`SAVE_ENV` and`RESTORE_ENV` .
 Patch by Erlend E. Aasland.
- bpo-45573: Mandatory core modules, that are required to bootstrap Python, are now in `Modules/Setup.bootstrap` .
- bpo-45573: `configure` now creates`Modules/Setup.stdlib` with
conditionally enabled/disabled extension module lines. The file is not
used, yet.
- bpo-45573: `configure` now uses a unified format to set state, compiler
flags, and linker flags in Makefile. The new macro`PY_STDLIB_MOD` sets
three variables that are consumed by`Modules/Setup` and`setup.py` .
- bpo-45816: Python now supports building with Visual Studio 2022 (MSVC v143, VS Version 17.0). Patch by Jeremiah Vivian.
- bpo-45800: Settings for `pyexpat` C extension are now detected by`configure` . The bundled`expat` library is built in`Makefile` .
- bpo-45798: Settings for `decimal` internal C extension are now
detected by`configure` . The bundled`libmpdec` library is built in`Makefile` .
- bpo-45723: **configure** has a new option`--with-pkg-config` to
disable or require pkg-config.
- bpo-45774: The build dependencies for `sqlite3` are now detected by`configure` and`pkg-config` . Patch by Erlend E. Aasland.
- bpo-45763: The build dependencies for `zlib` ,`bz2` , and`lzma` are now detected by`configure` .
- bpo-45747: gdbm and dbm build dependencies are now detected by `configure` .
- bpo-45743: On macOS, the build system no longer passes `search_paths_first` to the linker. The flag has been the default since
Xcode 4 / macOS 10.6.
- bpo-45723: `configure.ac` is now compatible with autoconf 2.71.
Deprecated checks`STDC_HEADERS` and`AC_HEADER_TIME` have been
removed.
- bpo-45723: `configure` now prints a warning when pkg-config is missing.
- bpo-45731: `configure --enable-loadable-sqlite-extensions` is now
handled by new`PY_SQLITE_ENABLE_LOAD_EXTENSION` macro instead of logic
in setup.py.
- bpo-45723: configure.ac now uses custom helper macros and `AC_CACHE_CHECK` to simplify and speed up configure runs.
- bpo-45696: Skip the marshal step for frozen modules by generating C code that produces a set of ready-to-use code objects. This speeds up startup time by another 10% or more.
- bpo-45561: Run smelly.py tool from $(srcdir).

### WindowsÂ¶

- bpo-46105: Fixed calculation of `sys.path` in a venv on Windows.
- bpo-45901: When installed through the Microsoft Store and set as the default app for `*.py` files, command line arguments will now be
passed to Python when invoking a script without explicitly launching
Python (that is,`script.py args` rather than```
python script.py
args
```
).
- bpo-45616: Fix Python Launcherâs ability to distinguish between versions 3.1 and 3.10 when either one is explicitly requested. Previously, 3.1 would be used if 3.10 was requested but not installed, and 3.10 would be used if 3.1 was requested but 3.10 was installed.
- bpo-45850: Implement changes to build with deep-frozen modules on Windows. Note that we now require Python 3.10 as the âbootstrapâ or âhostâ Python.
- bpo-45732: Updates bundled Tcl/Tk to 8.6.12.
- bpo-45720: Internal reference to `shlwapi.dll` was dropped to help
improve startup time. This DLL will no longer be loaded at the start of
every Python process.

### macOSÂ¶

- bpo-45732: Update python.org macOS installer to use Tcl/Tk 8.6.12.

### C APIÂ¶

- bpo-39026: Fix Python.h to build C extensions with Xcode: remove a relative include from `Include/cpython/pystate.h` .

## Python 3.11.0 alpha 2Â¶

*Release date: 2021-11-05*

### Core and BuiltinsÂ¶

- bpo-45716: Improve the `SyntaxError` message when using`True` ,`None` or`False` as keywords in a function call. Patch by Pablo
Galindo.
- bpo-45688: `sys.stdlib_module_names` now contains the macOS-specific
module`_scproxy` .
- bpo-45379: Clarify `ImportError` message when we try to explicitly
import a frozen module but frozen modules are disabled.
- bpo-44525: Specialize simple calls to Python functions (no starargs, keyword dict, or closure)
- bpo-45530: Cases of sorting using tuples as keys may now be significantly faster in some cases. Patch by Tim Peters. The order of the result may differ from earlier releases if the tuple elements donât define a total ordering (see Value comparisons for information on total ordering). Itâs generally true that the result of sorting simply isnât well-defined in the absence of a total ordering on list elements.
- bpo-45526: In obmalloc, set ADDRESS_BITS to not ignore any bits (ignored 16 before). That is safer in the case that the kernel gives user-space virtual addresses that span a range greater than 48 bits.
- bpo-30570: Fixed a crash in `issubclass()` from infinite recursion when
searching pathological`__bases__` tuples.
- bpo-45521: Fix a bug in the obmalloc radix tree code. On 64-bit machines, the bug causes the tree to hold 46-bits of virtual addresses, rather than the intended 48-bits.
- bpo-45494: Fix parser crash when reporting errors involving invalid continuation characters. Patch by Pablo Galindo.
- bpo-45445: Python now fails to initialize if it finds an invalid `-X` option in the command line. Patch by Pablo Galindo.
- bpo-45340: Object attributes are held in an array instead of a dictionary. An objectâs dictionary are created lazily, only when needed. Reduces the memory consumption of a typical Python object by about 30%. Patch by Mark Shannon.
- bpo-45408: Fix a crash in the parser when reporting tokenizer errors that occur at the same time unclosed parentheses are detected. Patch by Pablo Galindo.
- bpo-29410: Add SipHash13 for string hash algorithm and use it by default.
- bpo-45385: Fix reference leak from descr_check. Patch by Donghee Na.
- bpo-45367: Specialized the `BINARY_MULTIPLY` opcode to`BINARY_MULTIPLY_INT` and`BINARY_MULTIPLY_FLOAT` using the PEP 659
machinery.
- bpo-21736: Frozen stdlib modules now have `__file__` to the .py file
they would otherwise be loaded from, if possible.  For packages,`__path__` now has the correct entry instead of being an empty list,
which allows unfrozen submodules to be imported.  These are set only if
the stdlib directory is known when the runtime is initialized.  Note that
the file at`__file__` is not guaranteed to exist.  None of this affects
non-stdlib frozen modules nor, for now, frozen modules imported using`PyImport_ImportFrozenModule()` .  Also, at the moment`co_filename` is
not updated for the module.
- bpo-45020: For frozen stdlib modules, record the original module name as `module.__spec__.loader_state.origname` .  If the value is different than`module.__spec__.name` then the module was defined as an alias in
Tools/scripts/freeze_modules.py.  If it is`None` then the module comes
from a source file outside the stdlib.
- bpo-45324: In FrozenImporter.find_spec(), we now preserve the information needed in exec_module() to load the module. This change mostly impacts internal details, rather than changing the importerâs behavior.
- bpo-45292: Implement **PEP 654** . Add`ExceptionGroup` and`BaseExceptionGroup` . Update traceback display code.
- bpo-40116: Change to the implementation of split dictionaries. Classes where the instances differ either in the exact set of attributes, or in the order in which those attributes are set, can still share keys. This should have no observable effect on users of Python or the C-API. Patch by Mark Shannon.
- bpo-44050: Extensions that indicate they use global state (by setting `m_size` to -1) can again be used in multiple interpreters. This reverts
to behavior of Python 3.8.
- bpo-44525: Setup initial specialization infrastructure for the `CALL_FUNCTION` opcode. Implemented initial specializations for C
function calls:
  - `CALL_FUNCTION_BUILTIN_O` for`METH_O` flag.
  - `CALL_FUNCTION_BUILTIN_FAST` for`METH_FASTCALL` flag without keywords.
  - `CALL_FUNCTION_LEN` for`len(o)` .
  - `CALL_FUNCTION_ISINSTANCE` for`isinstance(o, t)` .
- bpo-44511: Improve the generated bytecode for class and mapping patterns.
- bpo-43706: Speed up calls to `enumerate()` by using the**PEP 590**`vectorcall` calling convention. Patch by Donghee Na.

### LibraryÂ¶

- bpo-45679: Fix caching of multi-value `typing.Literal` .`Literal[True, 2]` is no longer equal to`Literal[1, 2]` .
- bpo-42064: Convert `sqlite3` to multi-phase initialisation (PEP 489).
Patches by Erlend E. Aasland.
- bpo-45438: Fix typing.Signature string representation for generic builtin types.
- bpo-45613: `sqlite3` now sets`sqlite3.threadsafety` based on
the default threading mode the underlying SQLite library has been compiled
with. Patch by Erlend E. Aasland.
- bpo-45574: Fix warning about `print_escape` being unused.
- bpo-45581: `sqlite3.connect()` now correctly raises`MemoryError` if the underlying SQLite API signals memory error. Patch by Erlend E.
Aasland.
- bpo-45557: `pprint.pprint()` now handles*underscore_numbers* correctly. Previously it was always setting it to`False` .
- bpo-44019: Add `operator.call()` to`operator.__all__` . Patch by
Kreusada.
- bpo-42174: `shutil.get_terminal_size()` now falls back to sane values
if the column or line count are 0.
- bpo-35673: Improve the introspectability of the `__loader__` attribute
for namespace packages.`importlib.machinery.NamespaceLoader` is
now public, and implements the`importlib.abc.InspectLoader` interface.`_NamespaceLoader` is kept for backward compatibility.
- bpo-45515: Add references to `zoneinfo` in the`datetime` documentation, mostly replacing outdated references to`dateutil.tz` .
Change by Paul Ganssle.
- bpo-45475: Reverted optimization of iterating `gzip.GzipFile` ,`bz2.BZ2File` , and`lzma.LZMAFile` (see bpo-43787) because
it caused regression when user iterate them without having reference of
them. Patch by Inada Naoki.
- bpo-45489: Update `ForwardRef` to support`|` operator.
Patch by Donghee Na.
- bpo-42222: Removed deprecated support for float arguments in *randrange()* .
- bpo-45428: Fix a regression in py_compile when reading filenames from standard input.
- bpo-45467: Fix incremental decoder and stream reader in the âraw-unicode-escapeâ codec. Previously they failed if the escape sequence was split.
- bpo-45461: Fix incremental decoder and stream reader in the âunicode-escapeâ codec. Previously they failed if the escape sequence was split.
- bpo-45239: Fixed `email.utils.parsedate_tz()` crashing with`UnboundLocalError` on certain invalid input instead of returning`None` . Patch by Ben Hoyt.
- bpo-45417: Fix quadratic behaviour in the enum module: Creation of enum classes with a lot of entries was quadratic.
- bpo-45249: Fix the behaviour of `traceback.print_exc()` when
displaying the caret when the`end_offset` in the exception is set to 0.
Patch by Pablo Galindo
- bpo-45416: Fix use of `asyncio.Condition` with explicit`asyncio.Lock` objects, which was a regression due to removal of
explicit loop arguments. Patch by Joongi Kim.
- bpo-20028: Empty escapechar/quotechar is not allowed when initializing `csv.Dialect` . Patch by Vajrasky Kok and Donghee Na.
- bpo-44904: Fix bug in the `doctest` module that caused it to fail if
a docstring included an example with a`classmethod``property` . Patch
by Alex Waygood.
- bpo-45406: Make `inspect.getmodule()` catch`FileNotFoundError` raised by`inspect.getabsfile()` , and return`None` to indicate that
the module could not be determined.
- bpo-45411: Add extensions for files containing subtitles - .srt & .vtt - to the mimetypes.py module.
- bpo-10716: Migrated pydoc to HTML5 (without changing the look of it). Side effect is to update xmlrpcâs `ServerHTMLDoc` which now uses the CSS too.
cgitb now relies less on pydoc (as it canât use the CSS file).
- bpo-27580: Add support of null characters in `csv` .
- bpo-45262: Prevent use-after-free in asyncio. Make sure the cached running loop holder gets cleared on dealloc to prevent use-after-free in get_running_loop
- bpo-45386: Make `xmlrpc.client` more robust to C runtimes where the
underlying C`strftime` function results in a`ValueError` when
testing for year formatting options.
- bpo-20028: Improve error message of `csv.Dialect` when
initializing. Patch by Vajrasky Kok and Donghee Na.
- bpo-45343: Update bundled pip to 21.2.4 and setuptools to 58.1.0
- bpo-45328: Fixed `http.client.HTTPConnection` to work properly in
OSs that donât support the`TCP_NODELAY` socket option.
- bpo-45243: Add `setlimit()` and`getlimit()` to`sqlite3.Connection` for
setting and getting SQLite limits by connection basis. Patch by Erlend E.
Aasland.
- bpo-45320: Removed from the `inspect` module:
    - the `getargspec` function, deprecated since Python 3.0;
    - use `inspect.signature()` or`inspect.getfullargspec()` instead.
  - the 
  - the `formatargspec` function, deprecated since Python 3.5;
use the`inspect.signature()` function and`Signature` object
directly.
  - the undocumented `Signature.from_builtin` and`Signature.from_function` functions, deprecated since Python 3.5; use the`Signature.from_callable()` method
instead.
 Patch by Hugo van Kemenade.
- bpo-45192: Fix the `tempfile._infer_return_type` function so that the`dir` argument of the`tempfile` functions accepts an object
implementing the`os.PathLike` protocol.Patch by Kyungmin Lee.
- bpo-45160: When tracing a tkinter variable used by a ttk OptionMenu, callbacks are no longer made twice.
- bpo-25625: Added non parallel-safe `chdir()` context
manager to change the current working directory and then restore it on
exit. Simple wrapper around`chdir()` .
- bpo-24139: Add support for SQLite extended result codes in `sqlite3.Error` . Patch by Erlend E. Aasland.
- bpo-24444: Fixed an error raised in `argparse` help display when help
for an option is set to 1+ blank spaces or when*choices* arg is an empty
container.
- bpo-44547: Implement `Fraction.__int__` , so that a`fractions.Fraction` instance`f` passes an```
isinstance(f,
typing.SupportsInt)
```
check.
- bpo-40321: Adds support for HTTP 308 redirects to `urllib` . See**RFC 7538** for details. Patch by Jochem Schulenklopper.
- bpo-41374: Ensure that `socket.TCP_*` constants are exposed on Cygwin
3.1.6 and greater.
- bpo-35970: Add help flag to the base64 moduleâs command line interface. Patch contributed by Robert Kuska.

### DocumentationÂ¶

- bpo-45726: Improve documentation for `functools.singledispatch()` and`functools.singledispatchmethod` .
- bpo-45680: Amend the docs on `GenericAlias` objects to clarify that
non-container classes can also implement`__class_getitem__` . Patch
contributed by Alex Waygood.
- bpo-45618: Update Sphinx version used to build the documentation to 4.2.0. Patch by Maciej Olko.
- bpo-45655: Add a new ârelevant PEPsâ section to the top of the documentation for the `typing` module. Patch by Alex Waygood.
- bpo-45604: Add `level` argument to`multiprocessing.log_to_stderr` function docs.
- bpo-45516: Add protocol description to the `importlib.abc.TraversableResources` documentation.
- bpo-45464: Mention in the documentation of Built-in Exceptions that inheriting from multiple exception types in a single subclass is not recommended due to possible memory layout incompatibility.
- bpo-45449: Add note about **PEP 585** in`collections.abc` .
- bpo-45516: Add protocol description to the `importlib.abc.Traversable` documentation.
- bpo-20692: Add Programming FAQ entry explaining that int literal attribute access requires either a space after or parentheses around the literal.

### TestsÂ¶

- bpo-45678: Add tests for scenarios in which `functools.singledispatchmethod` is stacked on top of a method that
has already been wrapped by two other decorators. Patch by Alex Waygood.
- bpo-45578: Add tests for `dis.distb()`
- bpo-45678: Add tests to ensure that `functools.singledispatchmethod` correctly wraps the attributes of the target function.
- bpo-45668: PGO tests now pass when Python is built without test extension modules.
- bpo-45577: Add subtests for all `pickle` protocols in`test_zoneinfo` .
- bpo-45566: Fix `test_frozen_pickle` in`test_dataclasses` to check all`pickle` versions.
- bpo-43592: `test.libregrtest` now raises the soft resource limit for
the maximum number of file descriptors when the default is too low for our
test suite as was often the case on macOS.
- bpo-39679: Add more test cases for `@functools.singledispatchmethod` when combined with`@classmethod` or`@staticmethod` .
- bpo-45410: When libregrtest spawns a worker process, stderr is now written into stdout to keep messages order. Use a single pipe for stdout and stderr, rather than two pipes. Previously, messages were out of order which made analysis of buildbot logs harder Patch by Victor Stinner.
- bpo-45402: Fix test_tools.test_sundry() when Python is built out of tree: fix how the freeze_modules.py tool locates the _freeze_module program. Patch by Victor Stinner.
- bpo-45403: Fix test_sys.test_stdlib_dir() when Python is built outside the source tree: compare normalized paths. Patch by Victor Stinner.
- bpo-45400: Fix test_name_error_suggestions_do_not_trigger_for_too_many_locals() of test_exceptions if a directory name contains âa1â (like âPython-3.11.0a1â): use a stricter regular expression. Patch by Victor Stinner.
- bpo-10572: Rename `sqlite3` tests from`test_sqlite` to`test_sqlite3` , and relocate them to`Lib/test/test_sqlite3` . Patch by
Erlend E. Aasland.

### BuildÂ¶

- bpo-43158: `setup.py` now uses values from configure script to build the`_uuid` extension module. Configure now detects util-linuxâs`libuuid` , too.
- bpo-45666: Fix warning of `swprintf` and`%s` usage in`_testembed.c`
- bpo-45548: `Modules/Setup` and`Modules/makesetup` have been improved.
The`Setup` file now contains working rules for all extensions. Outdated
comments have been removed. Rules defined by`makesetup` track
dependencies correctly.
- bpo-45548: The `math` and`cmath` implementation now require a
C99 compatible`libm` and no longer ship with workarounds for missing
acosh, asinh, atanh, expm1, and log1p functions.
- bpo-45595: `setup.py` and`makesetup` now track build dependencies on
all Python header files and module specific header files.
- bpo-45571: `Modules/Setup` now use`PY_CFLAGS_NODIST` instead of`PY_CFLAGS` to compile shared modules.
- bpo-45570: `pyexpat` and`_elementtree` no longer define
obsolete macros`HAVE_EXPAT_CONFIG_H` and`USE_PYEXPAT_CAPI` .`XML_POOR_ENTROPY` is now defined in`expat_config.h` .
- bpo-43974: `setup.py` no longer defines`Py_BUILD_CORE_MODULE` .
Instead every module, that uses the internal API, defines the macro.
- bpo-45548: Fill in missing entries in Modules/Setup.
- bpo-45532: Update `sys.version` to use`main` as fallback
information. Patch by Jeong YunWon.
- bpo-45536: The `configure` script now checks whether OpenSSL headers and
libraries provide required APIs. Most common APIs are verified. The check
detects outdated or missing OpenSSL. Failures do not stop configure.
- bpo-45221: Fixed regression in handling of `LDFLAGS` and`CPPFLAGS` options where`argparse.parse_known_args()` could interpret an option
as one of the built-in command line argument, for example`-h` for help.
- bpo-45440: Building Python now requires a C99 `<math.h>` header file
providing the following functions:`copysign()` ,`hypot()` ,`isfinite()` ,`isinf()` ,`isnan()` ,`round()` . Patch by Victor
Stinner.
- bpo-45405: Prevent `internal configure error` when running`configure` with recent versions of non-Apple clang.  Patch by David Bohman.
- bpo-45433: Avoid linking libpython with libcrypt.

### WindowsÂ¶

- bpo-43652: Update Tcl/Tk to 8.6.11, actually this time. The previous update incorrectly included 8.6.10.
- bpo-45337: venv now warns when the created environment may need to be accessed at a different path, due to redirections, links or junctions. It also now correctly installs or upgrades components when the alternate path is required.
- bpo-43851: Build SQLite `SQLITE_OMIT_AUTOINIT` on Windows. Patch by
Erlend E. Aasland.

### macOSÂ¶

- bpo-44828: Avoid tkinter file dialog failure on macOS 12 Monterey when using the Tk 8.6.11 provided by python.org macOS installers. Patch by Marc Culler of the Tk project.

### IDLEÂ¶

- bpo-45495: Add context keywords âcaseâ and âmatchâ to completions list.

### C APIÂ¶

- bpo-29103: `PyType_FromSpec*` now
copies the class name from the spec to a buffer owned by the class, so the
original can be safely deallocated. Patch by Petr Viktorin.
- bpo-45522: The internal freelists for frame, float, list, dict, async generators, and context objects can now be disabled.
- bpo-35134: Exclude `PyWeakref_GET_OBJECT()` from the limited C API.
It never worked since the`PyWeakReference` structure is opaque
in the limited C API.
- bpo-35081: Move the `interpreteridobject.h` header file from`Include/` to`Include/internal/` . It only provides private functions.
Patch by Victor Stinner.
- bpo-35134: The non-limited API files `cellobject.h` ,`classobject.h` ,`context.h` ,`funcobject.h` ,`genobject.h` and`longintrepr.h` have been moved to the`Include/cpython` directory. Moreover, the`eval.h` header file was removed. These files must not be included
directly, as they are already included in`Python.h` : Include
Files. If they have been included directly, consider
including`Python.h` instead. Patch by Victor Stinner.
- bpo-45474: The following items are no longer available when `Py_LIMITED_API` is defined:
  - `PyMarshal_WriteLongToFile()`
  - `PyMarshal_WriteObjectToFile()`
  - `PyMarshal_ReadObjectFromString()`
  - `PyMarshal_WriteObjectToString()`
  - the `Py_MARSHAL_VERSION` macro
 These are not part of the limited API. Patch by Victor Stinner.
- bpo-45434: Remove the `pystrhex.h` header file. It only contains private
functions. C extensions should only include the main`<Python.h>` header
file. Patch by Victor Stinner.
- bpo-45440: Remove the `Py_FORCE_DOUBLE()` macro. It was used by the`Py_IS_INFINITY()` macro. Patch by Victor Stinner.
- bpo-45434: `<Python.h>` no longer includes the header files`<stdlib.h>` ,`<stdio.h>` ,`<errno.h>` and`<string.h>` when the`Py_LIMITED_API` macro is set to`0x030b0000` (Python 3.11) or higher.
C extensions should explicitly include the header files after```
#include
<Python.h>
```
. Patch by Victor Stinner.
- bpo-41123: Remove `Py_UNICODE_COPY()` and`Py_UNICODE_FILL()` macros,
deprecated since Python 3.3. Use`PyUnicode_CopyCharacters()` or`memcpy()` (`wchar_t*` string), and`PyUnicode_Fill()` functions
instead. Patch by Victor Stinner.
- bpo-45412: Remove the following math macros using the `errno` variable:
  - `Py_ADJUST_ERANGE1()`
  - `Py_ADJUST_ERANGE2()`
  - `Py_OVERFLOWED()`
  - `Py_SET_ERANGE_IF_OVERFLOW()`
  - `Py_SET_ERRNO_ON_MATH_ERROR()`
 Patch by Victor Stinner.
- bpo-45395: Custom frozen modules (the array set to `PyImport_FrozenModules` ) are now treated as additions, rather than
replacing all the default frozen modules. Frozen stdlib modules can still
be disabled by setting the âcodeâ field of the custom array entry to NULL.
- bpo-43760: Add new `PyThreadState_EnterTracing()` , and`PyThreadState_LeaveTracing()` functions to the limited C API to
suspend and resume tracing and profiling. Patch by Victor Stinner.
- bpo-44220: `PyStructSequence_UnnamedField` is added to the Stable
ABI.

## Python 3.11.0 alpha 1Â¶

*Release date: 2021-10-05*

### SecurityÂ¶

- bpo-42278: Replaced usage of `tempfile.mktemp()` with`TemporaryDirectory` to avoid a potential race condition.
- bpo-44600: Fix incorrect line numbers while tracing some failed patterns in match statements. Patch by Charles Burkland.
- bpo-41180: Add auditing events to the `marshal` module, and stop
raising`code.__init__` events for every unmarshalled code object.
Directly instantiated code objects will continue to raise an event, and
audit event handlers should inspect or collect the raw marshal data. This
reduces a significant performance overhead when loading from`.pyc` files.
- bpo-44394: Update the vendored copy of libexpat to 2.4.1 (from 2.2.8) to get the fix for the **CVE 2013-0340** âBillion Laughsâ vulnerability. This
copy is most used on Windows and macOS.
- bpo-43124: Made the internal `putcmd` function in`smtplib` sanitize input for presence of`\r` and`\n` characters to avoid
(unlikely) command injection.
- bpo-44022: `http.client` now avoids infinitely reading potential HTTP
headers after a`100 Continue` status response from the server.

### Core and BuiltinsÂ¶

- bpo-43760: The number of hardware branches per instruction dispatch is reduced from two to one by adding a special instruction for tracing. Patch by Mark Shannon.
- bpo-45061: Add a deallocator to the bool type to detect refcount bugs in C extensions which call Py_DECREF(Py_True) or Py_DECREF(Py_False) by mistake. Detect also refcount bugs when the empty tuple singleton or the Unicode empty string singleton is destroyed by mistake. Patch by Victor Stinner.
- bpo-24076: sum() was further optimised for summing up single digit integers.
- bpo-45190: Update Unicode databases to Unicode 14.0.0.
- bpo-45167: Fix deepcopying of `types.GenericAlias` objects.
- bpo-45155: `int.to_bytes()` and`int.from_bytes()` now take a
default value of`"big"` for the`byteorder` argument.`int.to_bytes()` also takes a default value of`1` for the`length` argument.

### LibraryÂ¶

- bpo-44219: Release the GIL while performing `isatty` system calls on
arbitrary file descriptors. In particular, this affects`os.isatty()` ,`os.device_encoding()` and`io.TextIOWrapper` . By extension,`io.open()` in text mode is also affected. This change solves a
deadlock in`os.isatty()` . Patch by Vincent Michel in bpo-44219.

### Core and BuiltinsÂ¶

- bpo-44959: Added fallback to extension modules with â.slâ suffix on HP-UX

### LibraryÂ¶

- bpo-45121: Fix issue where `Protocol.__init__` raises`RecursionError` when itâs called directly or via`super()` . Patch provided by Yurii
Karabas.

### Core and BuiltinsÂ¶

- bpo-44348: The deallocator function of the `BaseException` type now
uses the trashcan mechanism to prevent stack overflow. For example, when a`RecursionError` instance is raised, it can be linked to another
RecursionError through the`__context__` attribute or the`__traceback__` attribute, and then a chain of exceptions is created.
When the chain is destroyed, nested deallocator function calls can crash
with a stack overflow if the chain is too long compared to the available
stack memory. Patch by Victor Stinner.
- bpo-45123: Fix PyAiter_Check to only check for the __anext__ presence (not for __aiter__). Rename PyAiter_Check to PyAIter_Check, PyObject_GetAiter -> PyObject_GetAIter.
- bpo-1514420: Interpreter no longer attempts to open files with names in angle brackets (like â<string>â or â<stdin>â) when formatting an exception.
- bpo-41031: Match C and Python code formatting of unprintable exceptions and exceptions in the `__main__` module.
- bpo-37330: `open()` ,`io.open()` ,`codecs.open()` and`fileinput.FileInput` no longer accept`'U'` (âuniversal
newlineâ) in the file mode. This flag was deprecated since Python 3.3.
Patch by Victor Stinner.
- bpo-45083: When the interpreter renders an exception, its name now has a complete qualname. Previously only the class name was concatenated to the module name, which sometimes resulted in an incorrect full name being displayed. (This issue impacted only the C code exception rendering, the `traceback` module was using qualname already).
- bpo-34561: List sorting now uses the merge-ordering strategy from Munro and Wildâs `powersort()` . Unlike the former strategy, this is provably
near-optimal in the entropy of the distribution of run lengths. Most uses
of`list.sort()` probably wonât see a significant time difference, but
may see significant improvements in cases where the former strategy was
exceptionally poor. However, as these are all fast linear-time
approximations to a problem thatâs inherently at best quadratic-time to
solve truly optimally, itâs also possible to contrive cases where the
former strategy did better.
- bpo-45056: Compiler now removes trailing unused constants from co_consts.
- bpo-45020: Add a new command line option, â-X frozen_modules=[on|off]â to opt out of (or into) using optional frozen modules. This defaults to âonâ (or âoffâ if itâs running out of the source tree).

### LibraryÂ¶

- bpo-45012: In `posix` , release GIL during`stat()` ,`lstat()` ,
and`fstatat()` syscalls made by`os.DirEntry.stat()` . Patch by
StanisÅaw Skonieczny.

### Core and BuiltinsÂ¶

- bpo-45018: Fixed pickling of range iterators that iterated for over `2**32` times.
- bpo-45000: A `SyntaxError` is now raised when trying to delete`__debug__` . Patch by Donghee Na.
- bpo-44963: Implement `send()` and`throw()` methods for`anext_awaitable` objects. Patch by Pablo Galindo.

### LibraryÂ¶

- bpo-44962: Fix a race in WeakKeyDictionary, WeakValueDictionary and WeakSet when two threads attempt to commit the last pending removal. This fixes asyncio.create_task and fixes a data loss in asyncio.run where shutdown_asyncgens is not run

### Core and BuiltinsÂ¶

- bpo-24234: Implement the `__bytes__()` special method on the`bytes` type, so a bytes object`b` passes an```
isinstance(b,
typing.SupportsBytes)
```
check.
- bpo-24234: Implement the `__complex__()` special method on the`complex` type, so a complex number`z` passes an```
isinstance(z,
typing.SupportsComplex)
```
check.
- bpo-44954: Fixed a corner case bug where the result of `float.fromhex('0x.8p-1074')` was rounded the wrong way.
- bpo-44947: Refine the syntax error for trailing commas in import statements. Patch by Pablo Galindo.
- bpo-44945: Specialize the BINARY_ADD instruction using the PEP 659 machinery. Adds five new instructions: 
  - BINARY_ADD_ADAPTIVE
  - BINARY_ADD_FLOAT
  - BINARY_ADD_INT
  - BINARY_ADD_UNICODE
  - BINARY_ADD_UNICODE_INPLACE_FAST

### LibraryÂ¶

- bpo-44929: Fix some edge cases of `enum.Flag` string representation in
the REPL. Patch by Pablo Galindo.

### Core and BuiltinsÂ¶

- bpo-44914: Class version tags are no longer recycled. This means that a version tag serves as a unique identifier for the state of a class. We rely on this for effective specialization of the LOAD_ATTR and other instructions.
- bpo-44698: Restore behaviour of complex exponentiation with integer-valued exponent of type `float` or`complex` .
- bpo-44895: A debug variable `PYTHONDUMPREFSFILE` is added for
creating a dump file which is generated by`--with-trace-refs` .
Patch by Donghee Na.
- bpo-44900: Add five superinstructions for PEP 659 quickening: 
  - LOAD_FAST LOAD_FAST
  - STORE_FAST LOAD_FAST
  - LOAD_FAST LOAD_CONST
  - LOAD_CONST LOAD_FAST
  - STORE_FAST STORE_FAST
- bpo-44889: Initial implementation of adaptive specialization of `LOAD_METHOD` . The following specialized forms were added:
  - `LOAD_METHOD_CACHED`
  - `LOAD_METHOD_MODULE`
  - `LOAD_METHOD_CLASS`
- bpo-44890: Specialization stats are always collected in debug builds.
- bpo-44885: Correct the ast locations of f-strings with format specs and repeated expressions. Patch by Pablo Galindo
- bpo-44878: Remove the loop from the bytecode interpreter. All instructions end with a DISPATCH macro, so the loop is now redundant.
- bpo-44878: Remove switch statement for interpreter loop when using computed gotos. This makes sure that we only have one dispatch table in the interpreter.
- bpo-44874: Deprecate the old trashcan macros ( `Py_TRASHCAN_SAFE_BEGIN` /`Py_TRASHCAN_SAFE_END` ). They should be
replaced by the new macros`Py_TRASHCAN_BEGIN` and`Py_TRASHCAN_END` .
- bpo-44872: Use new trashcan macros (Py_TRASHCAN_BEGIN/END) in frameobject.c instead of the old ones (Py_TRASHCAN_SAFE_BEGIN/END).
- bpo-33930: Fix segmentation fault with deep recursion when cleaning method objects. Patch by Augusto Goulart and Pablo Galindo.
- bpo-25782: Fix bug where `PyErr_SetObject` hangs when the current
exception has a cycle in its context chain.
- bpo-44856: Fix reference leaks in the error paths of `update_bases()` and`__build_class__` . Patch by Pablo Galindo.
- bpo-44826: Initial implementation of adaptive specialization of STORE_ATTR Three specialized forms of STORE_ATTR are added: 
  - STORE_ATTR_SLOT
  - STORE_ATTR_SPLIT_KEYS
  - STORE_ATTR_WITH_HINT
- bpo-44838: Fixed a bug that was causing the parser to raise an incorrect custom `SyntaxError` for invalid âifâ expressions. Patch by Pablo
Galindo.
- bpo-44821: Create instance dictionaries (__dict__) eagerly, to improve regularity of object layout and assist specialization.
- bpo-44792: Improve syntax errors for if expressions. Patch by Miguel Brito
- bpo-34013: Generalize the invalid legacy statement custom error message (like the one generated when âprintâ is called without parentheses) to include more generic expressions. Patch by Pablo Galindo
- bpo-44732: Rename `types.Union` to`types.UnionType` .
- bpo-44725: Expose specialization stats in python via `_opcode.get_specialization_stats()` .
- bpo-44717: Improve AttributeError on circular imports of submodules.
- bpo-44698: Fix undefined behaviour in complex object exponentiation.
- bpo-44653: Support `typing` types in parameter substitution in the
union type.
- bpo-44676: Add ability to serialise `types.Union` objects. Patch
provided by Yurii Karabas.
- bpo-44633: Parameter substitution of the union type with wrong types now raises `TypeError` instead of returning`NotImplemented` .
- bpo-44661: Update `property_descr_set` to use vectorcall if possible.
Patch by Donghee Na.
- bpo-44662: Add `__module__` to`types.Union` . This also fixes`types.Union` issues with`typing.Annotated` . Patch provided by Yurii
Karabas.
- bpo-44655: Include the name of the type in unset __slots__ attribute errors. Patch by Pablo Galindo
- bpo-44655: Donât include a missing attribute with the same name as the failing one when offering suggestions for missing attributes. Patch by Pablo Galindo
- bpo-44646: Fix the hash of the union type: it no longer depends on the order of arguments.
- bpo-44636: Collapse union of equal types. E.g. the result of `int | int` is now`int` . Fix comparison of the union type with non-hashable
objects. E.g.`int | str == {}` no longer raises a TypeError.

### LibraryÂ¶

- bpo-44611: On Windows, `os.urandom()` : uses BCryptGenRandom API
instead of CryptGenRandom API which is deprecated from Microsoft Windows
API. Patch by Donghee Na.

### Core and BuiltinsÂ¶

- bpo-44635: Convert `None` to`type(None)` in the union type
constructor.
- bpo-26280: Implement adaptive specialization for BINARY_SUBSCR Three specialized forms of BINARY_SUBSCR are added: 
  - BINARY_SUBSCR_LIST_INT
  - BINARY_SUBSCR_TUPLE_INT
  - BINARY_SUBSCR_DICT
