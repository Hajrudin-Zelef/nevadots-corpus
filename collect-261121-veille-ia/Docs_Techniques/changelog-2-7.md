---
id: collect-261121-veille-ia/docs-techniques/changelog-2-7
title: "Changelog"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["error", "patch", "list", "typing", "sslcontext", "core", "builtinsa", "status"]
source: https://docs.python.org/release/3.15.1/whatsnew/changelog.html
source_anchor: ""
source_lines: [9001, 10500]
section: "Docs_Techniques"
---
### SecurityÂ¶

- gh-115398: Allow controlling Expat >=2.6.0 reparse deferral ( **CVE 2023-52425** ) by adding five new methods:
- gh-114572: `ssl.SSLContext.cert_store_stats()` and`ssl.SSLContext.get_ca_certs()` now correctly lock access to the
certificate store, when the`ssl.SSLContext` is shared across
multiple threads.

### Core and BuiltinsÂ¶

- gh-116604: Respect the status of the garbage collector when indirect calls are made via `PyErr_CheckSignals()` and the evaluation breaker.
Patch by Pablo Galindo
- gh-112087: `list` is now compatible with the implementation of**PEP 703** .
- gh-116381: Add specialization for `CONTAINS_OP` .
- gh-116296: Fix possible refleak in `object.__reduce__()` internal
error handling.
- gh-115823: Properly calculate error ranges in the parser when raising `SyntaxError` exceptions caused by invalid byte sequences. Patch by
Pablo Galindo
- gh-115778: Add `tierN` annotation for instruction definition in
interpreter DSL.
- gh-115733: Fix crash when calling `next()` on exhausted list iterators.
- gh-115700: The regen-cases build stage now works on Windows.
- gh-115347: Fix bug where docstring was replaced by a redundant NOP when Python is run with `-OO` .
- gh-115323: Make error message more meaningful for when `bytearray.extend()` is called with a`str` object.
- gh-112175: Every `PyThreadState` now has its own`eval_breaker` ,
allowing specific threads to be interrupted.

### LibraryÂ¶

- gh-115154: Fix a bug that was causing the `tokenize.untokenize()` function to handle unicode named literals incorrectly. Patch by Pablo
Galindo

### Core and BuiltinsÂ¶

- gh-112433: Add ability to force alignment of `ctypes.Structure` by
way of the new`_align_` attribute on the class.

### LibraryÂ¶

- gh-104090: The multiprocessing resource tracker now exits with non-zero status code if a resource leak was detected. It still exits with status code 0 otherwise.

### Core and BuiltinsÂ¶

- gh-105858: Improve the constructors for `ast` nodes. Arguments of
list types now default to an empty list if omitted, and optional fields
default to`None` . AST nodes now have an`__annotations__` attribute
with the expected types of their attributes. Passing unrecognized extra
arguments to AST nodes is deprecated and will become an error in Python
3.15. Omitting a required argument to an AST node is deprecated and will
become an error in Python 3.15. Patch by Jelle Zijlstra.
- gh-101860: Expose `__name__` attribute on property.
- gh-96497: Fix incorrect resolution of mangled class variables used in assignment expressions in comprehensions.

### LibraryÂ¶

- gh-116600: Fix `repr()` for global`Flag` members.
- gh-116349: `platform.java_ver()` is deprecated and will be removed in
3.15. It was largely untested, had a confusing API, and was only useful
for Jython support.
- gh-116143: Fix a race in pydoc `_start_server` , eliminating a window in
which`_start_server` can return a thread that is âservingâ but without
a`docserver` set.
- gh-116127: `typing` : implement**PEP 705** which adds`typing.ReadOnly` support to`typing.TypedDict` .
- gh-116325: `typing` : raise`SyntaxError` instead of`AttributeError` on forward references as empty strings.
- gh-115957: When `asyncio.TaskGroup.create_task` is called on an inactive`asyncio.TaskGroup` , the given coroutine will be closed (which prevents
a`RuntimeWarning` ).
- gh-115978: Disable preadv(), readv(), pwritev(), and writev() on WASI. Under wasmtime for WASI 0.2, these functions donât pass test_posix (https://github.com/bytecodealliance/wasmtime/issues/7830).
- gh-88352: Fix the computation of the next rollover time in the `logging.TimedRotatingFileHandler` handler.`computeRollover()` now always returns a timestamp larger than the
specified time and works correctly during the DST change.`doRollover()` no longer overwrite the already rolled over file,
saving from data loss when run at midnight or during repeated time at the
DST change.
- gh-87115: Set `__main__.__spec__` to`None` when running a script with`pdb`
- gh-76511: Fix UnicodeEncodeError in `email.Message.as_string()` that
results when a message that claims to be in the ascii character set
actually has non-ascii characters. Non-ascii characters are now replaced
with the U+FFFD replacement character, like in the`replace` error
handler.
- gh-89547: Add support for nested typing special forms like Final[ClassVar[int]].
- gh-65824: Improve the `less` prompt in`pydoc` .
- gh-116040: [Enum] fix by-value calls when second value is falsey; e.g. Cardinal(1, 0)
- gh-115821: [Enum] Improve error message when calling super().__new__() in custom __new__.
- gh-85644: Use the `XDG_CURRENT_DESKTOP` environment variable in`webbrowser` to check desktop. Prefer it to the deprecated`GNOME_DESKTOP_SESSION_ID` for GNOME detection.
- gh-75988: Fixed `unittest.mock.create_autospec()` to pass the call
through to the wrapped object to return the real result.
- gh-115881: Fix issue where `ast.parse()` would incorrectly flag
conditional context managers (such as`with (x() if y else z()): ...` )
as invalid syntax if`feature_version=(3, 8)` was passed. This reverts
changes to the grammar made as part of gh-94949.
- gh-115886: Fix silent truncation of the name with an embedded null character in `multiprocessing.shared_memory.SharedMemory` .
- gh-115532: Add kernel density estimation to the statistics module.
- gh-115714: On WASI, the `time` module no longer get process time
using`times()` or`CLOCK_PROCESS_CPUTIME_ID` , system API is that is
unreliable and is likely to be removed from WASI. The affected clock
functions fall back to calling`clock()` .
- gh-115809: Improve algorithm for computing which rolled-over log files to delete in `logging.TimedRotatingFileHandler` . It is now reliable
for handlers without`namer` and with arbitrary deterministic`namer` that leaves the datetime part in the file name unmodified.
- gh-74668: `urllib.parse` functions`parse_qs()` and`parse_qsl()` now support bytes arguments containing raw
and percent-encoded non-ASCII data.
- gh-67044: `csv.writer()` now always quotes or escapes`'\r'` and`'\n'` , regardless of*lineterminator* value.
- gh-115712: Restore support of space delimiter with `skipinitialspace=True` in`csv` .`csv.writer()` now quotes
empty fields if delimiter is a space and skipinitialspace is true and
raises exception if quoting is not possible.
- gh-112364: Fixed `ast.unparse()` to handle format_spec with`"` ,`'` or`\\` . Patched by Frank Hoffmann.
- gh-112997: Stop logging potentially sensitive callback arguments in `asyncio` unless debug mode is active.
- gh-114914: Fix an issue where an abandoned `StreamWriter` would not
be garbage collected.
- gh-111358: Fix a bug in `asyncio.BaseEventLoop.shutdown_default_executor()` to ensure the
timeout passed to the coroutine behaves as expected.
- gh-115618: Fix improper decreasing the reference count for `None` argument in`property` methods`getter()` ,`setter()` and`deleter()` .
- gh-112720: Refactor `dis.ArgResolver` to make it possible to
subclass and change the way jump args are interpreted.
- gh-112006: Fix `inspect.unwrap()` for types with the`__wrapper__` data descriptor. Fix`inspect.Signature.from_callable()` for builtins`classmethod()` and`staticmethod()` .
- gh-101293: Support callables with the `__call__()` method and types with`__new__()` and`__init__()` methods set to class methods, static
methods, bound methods, partial functions, and other types of methods and
descriptors in`inspect.Signature.from_callable()` .
- gh-103092: Isolate `_lsprof` (apply**PEP 687** ).
- gh-113942: `pydoc` no longer skips global functions implemented as
builtin methods, such as`MethodDescriptorType` and`WrapperDescriptorType` .
- gh-115256: Added DeprecationWarning when accessing the tarfile attribute of TarInfo objects. The attribute is never used internally and is only attached to TarInfos when the tarfile is opened in write-mode, not read-mode. The attribute creates an unnecessary reference cycle which may cause corruption when not closing the handle after writing a tarfile.
- gh-115197: `urllib.request` no longer resolves the hostname before
checking it against the systemâs proxy bypass list on macOS and Windows.
- gh-113812: `DatagramTransport.sendto()` will now send zero-length
datagrams if called with an empty bytes object. The transport flow control
also now accounts for the datagram header when calculating the buffer
size.
- gh-114763: Protect modules loaded with `importlib.util.LazyLoader` from race conditions when multiple threads try to access attributes before
the loading is complete.
- gh-114709: `posixpath.commonpath()` now raises a`ValueError` exception when passed an empty iterable. Previously,`IndexError` was
raised.`posixpath.commonpath()` now raises a`TypeError` exception when
passed`None` . Previously,`ValueError` was raised.
- gh-114610: Fix bug where `pathlib.PurePath.with_stem()` converted a
non-empty path suffix to a stem when given an empty*stem* argument. It
now raises`ValueError` , just like`pathlib.PurePath.with_suffix()` does when called on a path with an
empty stem, given a non-empty*suffix* argument.
- gh-107361: Add `ssl.VERIFY_X509_PARTIAL_CHAIN` and`VERIFY_X509_STRICT` to the default SSL context created with`ssl.create_default_context()` .
- gh-112281: Allow creating union of types for `typing.Annotated` with unhashable metadata.
- gh-111775: Fix `importlib.resources.simple.ResourceHandle.open()` for
text mode, added missed`stream` argument.
- gh-90095: Make .pdbrc and -c work with any valid pdb commands.
- gh-107625: Raise `configparser.ParsingError` from`read()` and`read_file()` methods of`configparser.ConfigParser` if a key without a corresponding value
is continued (that is, followed by an indented line).
- gh-107155: Fix incorrect output of `help(x)` where`x` is a`lambda` function, which has an`__annotations__` dictionary
attribute with a`"return"` key.
- gh-57141: Add option for *non-shallow* comparisons to`filecmp.dircmp` like`filecmp.cmp()` . Original patch by Steven
Ward. Enhanced by Tobias Rautenkranz
- gh-69990: `Profile.print_stats()` has been improved to accept multiple
sort arguments. Patched by Chiu-Hsiang Hsu and Furkan Onder.
- gh-104061: Add `socket.SO_BINDTOIFINDEX` constant.
- gh-60346: Fix ArgumentParser inconsistent with parse_known_args.
- gh-102389: Add `windows_31j` to aliases for`cp932` codec
- gh-72249: Always include the module name in the `repr()` of`functools.partial()` objects.  Patch by Furkan Onder and Anilyka
Barry.
- gh-100985: Update HTTPSConnection to consistently wrap IPv6 Addresses when using a proxy.
- gh-100884: email: fix misfolding of comma in address-lists over multiple lines in combination with unicode encoding.
- gh-95782: Fix `io.BufferedReader.tell()` ,`io.BufferedReader.seek()` ,`_pyio.BufferedReader.tell()` ,`io.BufferedRandom.tell()` ,`io.BufferedRandom.seek()` and`_pyio.BufferedRandom.tell()` being able to return negative offsets.
- gh-96310: Fix a traceback in `argparse` when all options in a
mutually exclusive group are suppressed.
- gh-93205: Fixed a bug in `logging.handlers.TimedRotatingFileHandler` where multiple rotating
handler instances pointing to files with the same name but different
extensions would conflict and not delete the correct files.
- bpo-31116: Add Z85 encoding to `base64` .
- bpo-44865: Add missing call to localization function in `argparse` .
- bpo-43952: Fix `multiprocessing.connection.Listener.accept()` to
accept empty bytes as authkey. Not accepting empty bytes as key causes it
to hang indefinitely.
- bpo-42125: linecache: get module name from `__spec__` if available. This
allows getting source code for the`__main__` module when a custom
loader is used.
- bpo-41122: Failing to pass arguments properly to `functools.singledispatchmethod()` now throws a TypeError instead of
hitting an index out of bounds internally.
- bpo-40818: The asyncio REPL now runs `sys.__interactivehook__` on
startup. The default implementation of`sys.__interactivehook__` provides auto-completion to the asyncio REPL. Patch contributed by RÃ©mi
Lapeyre.
- bpo-33775: Add âdefaultâ and âversionâ help text for localization in argparse.

### DocumentationÂ¶

- gh-115399: Document **CVE 2023-52425** of Expat <2.6.0 under âXML
vulnerabilitiesâ.
- gh-109653: Improve import time of `uuid` on Linux.

### TestsÂ¶

- gh-71052: Add test exclusions to support running the test suite on Android.
- gh-71052: Enable `test_concurrent_futures` on platforms that support
threading but not multiprocessing.
- gh-115796: Make â_testinternalcapi.assemble_code_objectâ construct the exception table for the code object.
- gh-115720: Leak tests ( `-R` ,`--huntrleaks` ) now show a summary of the
number of leaks found in each iteration.
- gh-115122: Add `--bisect` option to regrtest test runner: run failed
tests with`test.bisect_cmd` to identify failing tests. Patch by Victor
Stinner.
- gh-115596: Fix `ProgramPriorityTests` in`test_os` permanently
changing the process priority.
- gh-115556: On Windows, commas passed in arguments to `Tools\buildbot\test.bat` and`PCbuild\\rt.bat` are now properly
handled.
- gh-115420: Fix translation of exception handler targets by `_testinternalcapi.optimize_cfg` .
- gh-115376: Fix segfault in `_testinternalcapi.compiler_codegen` on bad
input.

### BuildÂ¶

- gh-116313: Get WASI builds to work under wasmtime 18 w/ WASI 0.2/preview2 primitives.
- gh-71052: Change Androidâs `sys.platform` from`"linux"` to`"android"` .
- gh-116117: Backport `libb2` âs PR #42 to fix compiling CPython on 32-bit
Windows with`clang-cl` .
- gh-71052: Fix several Android build issues
- gh-114099: A testbed project was added to run the test suite on iOS.
- gh-115350: Fix building ctypes module with -DWIN32_LEAN_AND_MEAN defined
- gh-111225: Link extension modules against libpython on Android.
- gh-115737: The install name for libPython is now correctly set for non-framework macOS builds.
- gh-114099: Makefile targets were added to support compiling an iOS-compatible framework build.

### WindowsÂ¶

- gh-116012: Ensure the value of `GetLastError()` is preserved across GIL
operations.
- gh-115582: Building extensions intended for free-threaded builds of CPython now require compiling with `/DPy_GIL_DISABLED` manually when
using a regular install. This is expected to change in future releases.
- gh-115554: The installer now has more strict rules about updating the Python install manager. In general, most users only have a single launcher installed and will see no difference. When multiple launchers have been installed, the option to install the launcher is disabled until all but one have been removed. Downgrading the launcher (which was never allowed) is now more obviously blocked.
- gh-115543: Python install manager can now detect Python 3.13 when installed from the Microsoft Store, and will install Python 3.12 by default when `PYLAUNCHER_ALLOW_INSTALL` is set.

### macOSÂ¶

- gh-116145: Update macOS installer to Tcl/Tk 8.6.14.

### IDLEÂ¶

- gh-88516: On macOS show a proxy icon in the title bar of editor windows to match platform behaviour.

### Tools/DemosÂ¶

### C APIÂ¶

- gh-114626: Add again `_PyCFunctionFastWithKeywords` name, removed in
Python 3.13 alpha 4 by mistake.  Keep the old private`_PyCFunctionFastWithKeywords` name (Python 3.7) as an alias to the new
public name`PyCFunctionFastWithKeywords` (Python 3.13a4). Patch by
Victor Stinner.
- gh-111418: Add `PyHASH_MODULUS` ,`PyHASH_BITS` ,`PyHASH_INF` and`PyHASH_IMAG` C macros.  Patch by
Sergey B Kirpichev.

## Python 3.13.0 alpha 4Â¶

*Release date: 2024-02-15*

### SecurityÂ¶

- gh-115399: Update bundled libexpat to 2.6.0
- gh-115243: Fix possible crashes in `collections.deque.index()` when
the deque is concurrently modified.

### Core and BuiltinsÂ¶

- gh-112087: For an empty reverse iterator for list will be reduced to `reversed()` . Patch by Donghee Na
- gh-114570: Add `PythonFinalizationError` exception. This exception
derived from`RuntimeError` is raised when an operation is blocked
during the Python finalization. Patch by
Victor Stinner.
- gh-114695: Add `sys._clear_internal_caches()` , which clears all
internal performance-related caches (and deprecate the less-general`sys._clear_type_cache()` function).
- gh-114828: Fix compilation crashes in uncommon code examples using `super()` inside a comprehension in a class body.
- gh-112069: Adapt `set` and`frozenset` methods to Argument
Clinic.
- gh-115011: Setters for members with an unsigned integer type now support the same range of valid values for objects that has a `__index__()` method as for`int` .
- gh-114887: Changed socket type validation in `create_datagram_endpoint()` to accept all non-stream
sockets. This fixes a regression in compatibility with raw sockets.
- gh-114944: Fixes a race between `PyParkingLot_Park` and`_PyParkingLot_UnparkAll` .
- gh-113462: Limit the number of versions that a single class can use. Prevents a few wayward classes using up all the version numbers.
- gh-76763: The `chr()` builtin function now always raises`ValueError` for values outside the valid range. Previously it raised`OverflowError` for very large or small values.
- gh-114806: No longer specialize calls to classes, if those classes have metaclasses. Fixes bug where the `__call__` method of the metaclass was
not being called.
- gh-107944: Improve error message for function calls with bad keyword arguments via getargs
- gh-112529: The free-threaded build no longer allocates space for the `PyGC_Head` structure in objects that support cyclic garbage collection.
A number of other fields and data structures are used as replacements,
including`ob_gc_bits` ,`ob_tid` , and mimalloc internal data
structures.
- gh-114456: Lower the recursion limit under a debug build of WASI.
- gh-114083: Compiler applies folding of LOAD_CONST with following instruction in a separate pass before other optimisations. This enables jump threading in certain circumstances.
- gh-114388: Fix a `RuntimeWarning` emitted when assign an integer-like
value that is not an instance of`int` to an attribute that
corresponds to a C struct member of type T_UINT
and T_ULONG. Fix a double`RuntimeWarning` emitted when assign a
negative integer value to an attribute that corresponds to a C struct
member of type T_UINT.
- gh-114265: Compiler propagates line numbers before optimization, leading to more optimization opportunities and removing the need for the `guarantee_lineno_for_exits` hack.
- gh-112529: The free-threaded build now has its own thread-safe GC implementation that uses mimalloc to find GC tracked objects. It is non-generational, unlike the existing GC implementation.
- gh-114050: Fix segmentation fault caused by an incorrect format string in `TypeError` exception when more than two arguments are passed to`int` .
- gh-112354: The `END_FOR` instruction now pops only one value. This is to
better support side exits in loops.
- gh-113884: Make `queue.SimpleQueue` thread safe when the GIL is
disabled.
- gh-114058: Implement the foundations of the Tier 2 redundancy eliminator.
- gh-113939: frame.clear(): Clear frame.f_locals as well, and not only the fast locals. This is relevant once frame.f_locals was accessed, which would contain also references to all the locals.
- gh-112050: Convert `collections.deque` to use Argument Clinic.
- gh-112050: Make methods on `collections.deque` thread-safe when the
GIL is disabled.
- gh-113464: Add an option ( `--enable-experimental-jit` for`configure` -based builds or`--experimental-jit` for`PCbuild` -based
ones) to build an*experimental* just-in-time compiler, based on
copy-and-patch
- gh-113055: Make interp->obmalloc a pointer. For interpreters that share state with the main interpreter, this points to the same static memory structure. For interpreters with their own obmalloc state, it is heap allocated. Add free_obmalloc_arenas() which will free the obmalloc arenas and radix tree structures for interpreters with their own obmalloc state.
- gh-55664: Add warning when creating `type` using a namespace
dictionary with non-string keys. Patched by Daniel Urban and Furkan Onder.
- gh-104530: Use native Win32 condition variables.

### LibraryÂ¶

- gh-115392: Fix a bug in `doctest` where incorrect line numbers would
be reported for decorated functions.
- gh-114563: Fix several `format()` bugs when using the C implementation
of`Decimal` : * memory leak in some rare cases when using
the`z` format option (coerce negative 0) * incorrect output when
applying the`z` format option to type`F` (fixed-point with capital`NAN` /`INF` ) * incorrect output when applying the`#` format
option (alternate form)
- gh-102840: Fix confused traceback when floordiv, mod, or divmod operations happens between instances of `fractions.Fraction` and`complex` .
- gh-115165: Most exceptions are now ignored when attempting to set the `__orig_class__` attribute on objects returned when calling`typing` generic aliases (including generic aliases created using`typing.Annotated` ). Previously only`AttributeError` was
ignored. Patch by Dave Shawley.
- gh-112903: Fix âissubclass() arg 1 must be a classâ errors in certain cases of multiple inheritance with generic aliases (regression in early 3.13 alpha releases).
- gh-115133: Fix tests for `XMLPullParser` with Expat 2.6.0.
- gh-115059: `io.BufferedRandom.read1()` now flushes the underlying
write buffer.
- gh-79382: Trailing `**` no longer allows to match files and non-existing
paths in recursive`glob()` .
- gh-67837: Avoid race conditions in the creation of directories during concurrent extraction in `tarfile` and`zipfile` .
- gh-115060: Speed up `pathlib.Path.glob()` by removing redundant regex
matching.
- gh-97928: Partially revert the behavior of `tkinter.Text.count()` . By
default it preserves the behavior of older Python versions, except that
setting`wantobjects` to 0 no longer has effect. Add a new parameter*return_ints* : specifying`return_ints=True` makes`Text.count()` always returning the single count as an integer instead of a 1-tuple or`None` .
- gh-114628: When csv.Error is raised when handling TypeError, do not print the TypeError traceback.
- gh-85984: Added `_POSIX_VDISABLE` from Câs`<unistd.h>` to`termios` .
- gh-114965: Update bundled pip to 24.0
- gh-114959: `tarfile` no longer ignores errors when trying to extract
a directory on top of a file.
- gh-114894: Add `array.array.clear()` .
- gh-114071: Support tuple subclasses using auto() for enum member value.
- gh-109475: Fix support of explicit option value âââ in `argparse` (e.g.`--option=--` ).
- gh-49766: Fix `date` -`datetime` comparison. Now the special comparison methods like`__eq__` and`__lt__` return`NotImplemented` if one of comparands is`date` and other is`datetime` instead of ignoring the
time part and the time zone or forcefully return ânot equalâ or raise`TypeError` . It makes comparison of`date` and`datetime` subclasses more symmetric and allows to change the
default behavior by overriding the special comparison methods in
subclasses.
- gh-110190: Fix ctypes structs with array on Windows ARM64 platform by setting `MAX_STRUCT_SIZE` to 32 in stgdict. Patch by Diego Russo
- gh-114678: Ensure that deprecation warning for âNâ specifier in `Decimal` format is not raised for cases where âNâ appears
in other places in the format specifier. Based on patch by Stefan Krah.
- gh-70303: Return both files and directories from `pathlib.Path.glob()` if a pattern ends with â`**` â. Previously only directories were
returned.
- gh-109653: Improve import time of `importlib.metadata` and`email.utils` .
- gh-113280: Fix a leak of open socket in rare cases when error occurred in `ssl.SSLSocket` creation.
- gh-77749: `email.policy.EmailPolicy.fold()` now always encodes
non-ASCII characters in headers if`utf8` is false.
- gh-83383: Synchronization of the `dbm.dumb` database is now no-op if
there was no modification since opening or last synchronization. The
directory file for a newly created empty`dbm.dumb` database is now
created immediately after opening instead of deferring this until
synchronizing or closing.
- gh-91602: Add *filter* keyword-only parameter to`sqlite3.Connection.iterdump()` for filtering database objects to
dump. Patch by Mariusz Felisiak.
- gh-112451: Prohibit subclassing pure-Python `datetime.timezone` .
This is consistent with C-extension implementation. Patch by Mariusz
Felisiak.
- gh-69893: Add the `close()` method for the iterator returned by`xml.etree.ElementTree.iterparse()` .
- gh-109653: Reduce the import time of `threading` module by ~50%.
Patch by Daniel Hollas.
- gh-114492: Make the result of `termios.tcgetattr()` reproducible on
Alpine Linux. Previously it could leave a random garbage in some fields.
- gh-114315: Make `threading.Lock` a real class, not a factory
function. Add`__new__` to`_thread.lock` type.
- gh-100414: Add `dbm.sqlite3` as a backend to`dbm` , and make it
the new default`dbm` backend. Patch by Raymond Hettinger and Erlend
E. Aasland.
- gh-113267: Revert changes in gh-106584 which made calls of `TestResult` methods`startTest()` and`stopTest()` unbalanced.
- gh-75128: Ignore an `OSError` in`asyncio.BaseEventLoop.create_server()` when IPv6 is available but the
interface cannot actually support it.
- gh-114423: `_DummyThread` entries in`threading._active` are now
automatically removed when the related thread dies.
- gh-114257: Dismiss the `FileNotFound` error in`ctypes.util.find_library()` and just return`None` on Linux.
- gh-114321: Expose more platform specific constants in the `fcntl` module on Linux, macOS, FreeBSD and NetBSD.
- gh-114328: The `tty.setcbreak()` and new`tty.cfmakecbreak()` no
longer clears the terminal input ICRLF flag. This fixes a regression
introduced in 3.12 that no longer matched how OSes define cbreak mode in
their`stty(1)` manual pages.
- gh-114281: Remove type hints from `Lib/asyncio/staggered.py` . The
annotations in the typeshed
project should be used instead.
- gh-101438: Avoid reference cycle in ElementTree.iterparse. The iterator returned by `ElementTree.iterparse` may hold on to a file descriptor.
The reference cycle prevented prompt clean-up of the file descriptor if
the returned iterator was not exhausted.
- gh-114198: The signature for the `__replace__` method on`dataclasses` now has the first argument named`self` , rather than`obj` .
- gh-104522: `OSError` raised when run a subprocess now only has*filename* attribute set to*cwd* if the error was caused by a failed
attempt to change the current directory.
- gh-114149: Enum: correctly handle tuple subclasses in custom `__new__` .
- gh-83648: Support deprecation of options, positional arguments and subcommands in `argparse` .
- gh-114087: Speed up `dataclasses.asdict` up to 1.35x.
- gh-109534: Fix a reference leak in `asyncio.selector_events.BaseSelectorEventLoop` when SSL handshakes
fail. Patch contributed by Jamie Phan.
- gh-79634: Accept path-like objects as patterns in `pathlib.Path.glob()` and`rglob()` .
- gh-112202: Ensure that a `asyncio.Condition.notify()` call does not
get lost if the awakened`Task` is simultaneously cancelled or
encounters any other error.
- gh-113951: Fix the behavior of `tag_unbind()` methods of`tkinter.Text` and`tkinter.Canvas` classes with three
arguments. Previously,`widget.tag_unbind(tag, sequence, funcid)` destroyed the current binding for*sequence* , leaving*sequence* unbound,
and deleted the*funcid* command. Now it removes only*funcid* from the
binding for*sequence* , keeping other commands, and deletes the*funcid* command. It leaves*sequence* unbound only if*funcid* was the last bound
command.
- gh-97959: Fix rendering class methods, bound methods, method and function aliases in `pydoc` . Class methods no longer have âmethod of
builtins.type instanceâ note. Corresponding notes are now added for class
and unbound methods. Method and function aliases now have references to
the module or the class where the origin was defined if it differs from
the current. Bound methods are now listed in the static methods section.
Methods of builtin classes are now supported as well as methods of Python
classes.
- gh-113796: Add more validation checks in the `csv.Dialect` constructor.`ValueError` is now raised if the same character is used
in different roles.
- gh-113732: Fix support of `QUOTE_NOTNULL` and`QUOTE_STRINGS` in`csv.reader()` .
- gh-113225: Speed up `pathlib.Path.walk()` by using`os.DirEntry.path` where possible.
- gh-89039: When replace() method is called on a subclass of datetime, date or time, properly call derived constructor. Previously, only the base classâs constructor was called. Also, make sure to pass non-zero fold values when creating subclasses in various methods. Previously, fold was silently ignored.
- gh-112919: Speed-up `datetime.datetime.replace()` ,`datetime.date.replace()` and`datetime.time.replace()` .
- gh-59013: Set breakpoint on the first executable line of the function, instead of the line of function definition when the user do `break func` using`pdb`
- gh-112343: Improve handling of pdb convenience variables to avoid replacing string contents.
- gh-112240: Add option to calendar module CLI to specify the weekday to start each week. Patch by Steven Ward.
- gh-111741: Recognise `image/webp` as a standard format in the`mimetypes` module.
- gh-43457: Fix the `tkinter` widget method`wm_attributes()` . It
now accepts the attribute name without the minus prefix to get window
attributes and allows to specify attributes and values to set as keyword
arguments. Add new optional keyword argument*return_python_dict* : calling`w.wm_attributes(return_python_dict=True)` returns the attributes as a
dict instead of a tuple. Calling`w.wm_attributes()` now returns a tuple
instead of string if*wantobjects* was set to 0.
- gh-82626: Many functions now emit a warning if a boolean value is passed as a file descriptor argument.
- gh-111051: Added check for file modification during debugging with `pdb`
- gh-110345: Show the Tcl/Tk patchlevel (rather than version) in `tkinter._test()` .
- gh-38807: Fix race condition in `trace` . Instead of checking if a
directory exists and creating it, directly call`os.makedirs()` with
the kwarg`exist_ok=True` .
- gh-75705: Set unixfrom envelope in `mailbox.mbox` and`mailbox.MMDF` .
- gh-106233: Fix stacklevel in `InvalidTZPathWarning` during`zoneinfo` module import.
- gh-105102: Allow `ctypes.Union` to be nested in`ctypes.Structure` when the system endianness is the opposite of
the classes.
- gh-104282: Fix null pointer dereference in `lzma._decode_filter_properties()` due to improper handling of BCJ
filters with properties of zero length. Patch by Radislav Chugunov.
- gh-96471: Add `queue.Queue` termination with`shutdown()` .
- gh-101599: Changed argparse flag options formatting to remove redundancy.
- gh-85984: Add POSIX pseudo-terminal functions `os.posix_openpt()` ,`os.grantpt()` ,`os.unlockpt()` , and`os.ptsname()` .
- gh-102512: When `os.fork()` is called from a foreign thread (aka`_DummyThread` ), the type of the thread in a child process is changed to`_MainThread` . Also changed its name and daemonic status, it can be now
joined.
- gh-88569: Add `os.path.isreserved()` , which identifies reserved
pathnames such as âNULâ, âAUXâ and âCONâ. This function is only available
on Windows.Deprecate `pathlib.PurePath.is_reserved()` .
- bpo-38364: The `inspect` functions`isgeneratorfunction` ,`iscoroutinefunction` ,`isasyncgenfunction` now support`functools.partialmethod` wrapped functions the same way they support`functools.partial` .

### DocumentationÂ¶

- gh-115233: Fix an example for `LoggerAdapter` in the
Logging Cookbook.
- gh-114123: Move the `csv` module docstring to the`csv` module
instead of reexporting it from the internal`_csv` module, and
remove`__doc__` from`csv.__all__` .Move `csv.__version__` to the`csv` module instead of
reexporting it from the internal`_csv` module, and remove`__version__` from`csv.__all__` .

### TestsÂ¶

### BuildÂ¶

- gh-115167: Avoid vendoring `vcruntime140_threads.dll` when building with
Visual Studio 2022 version 17.8.
- gh-113632: Promote WASI to a tier 2 platform and drop Emscripten from tier 3 in configure.ac.
- gh-114099: configure and Makefile were refactored to accommodate framework builds on Apple platforms other than macOS.
- gh-114875: Add `getgrent()` as a prerequisite for building the`grp` module.

### WindowsÂ¶

- gh-115049: Fixes `py.exe` launcher failing when run as users without
user profiles.
- gh-115009: Update Windows installer to use SQLite 3.45.1.
- gh-109991: Update Windows build to use OpenSSL 3.0.13.
- gh-111239: Update Windows builds to use zlib v1.3.1.
- gh-100107: The `py.exe` launcher will no longer attempt to run the
Microsoft Store redirector when launching a script containing a`/usr/bin/env` shebang
- gh-112984: Adds free-threaded binaries to Windows installer as an optional component.
- gh-89240: Allows `multiprocessing` to create pools of greater than 62
processes.

### macOSÂ¶

- gh-115009: Update macOS installer to use SQLite 3.45.1.
- gh-109991: Update macOS installer to use OpenSSL 3.0.13.
- gh-114490: Add Mach-O linkage support for `platform.architecture()` .
- gh-87804: On macOS the result of `os.statvfs` and`os.fstatvfs` now
correctly report the size of very large disks, in previous versions the
reported number of blocks was wrong for disks with at least 2**32 blocks.

### IDLEÂ¶

### Tools/DemosÂ¶

- gh-113516: Donât set `LDSHARED` when building for WASI.
- gh-109991: Update GitHub CI workflows to use OpenSSL 3.0.13 and multissltests to use 1.1.1w, 3.0.13, 3.1.5, and 3.2.1.
- gh-115015: Fix a bug in Argument Clinic that generated incorrect code for methods with no parameters that use the METH_METHOD | METH_FASTCALL | METH_KEYWORDS calling convention. Only the positional parameter count was checked; any keyword argument passed would be silently accepted.

### C APIÂ¶

- gh-111140: Adds `PyLong_AsNativeBytes()` ,`PyLong_FromNativeBytes()` and`PyLong_FromUnsignedNativeBytes()` functions.
- gh-114685: `PyBuffer_FillInfo()` now raises a`SystemError` if
called with`PyBUF_READ` or`PyBUF_WRITE` as flags.
These flags should only be used with the`PyMemoryView_*` C API.
- gh-114685: `PyObject_GetBuffer()` now raises a`SystemError` if
called with`PyBUF_READ` or`PyBUF_WRITE` as flags.
These flags should only be used with the`PyMemoryView_*` C API.
- gh-114626: Add `PyCFunctionFast` and`PyCFunctionFastWithKeywords` typedefs (identical to the existing`_PyCFunctionFast` and`_PyCFunctionFastWithKeywords` typedefs, just without a leading`_` prefix).
- gh-114329: Add `PyList_GetItemRef()` , which is similar to`PyList_GetItem()` but returns a strong reference instead of
a borrowed reference.
- gh-110850: Add PyTime C API: 
  - `PyTime_t` type.
  - `PyTime_MIN` and`PyTime_MAX` constants.
  - `PyTime_AsSecondsDouble()` ,`PyTime_Monotonic()` ,`PyTime_PerfCounter()` , and`PyTime_Time()` functions.
 Patch by Victor Stinner.
- gh-112066: Add `PyDict_SetDefaultRef()` : insert a key and value into
a dictionary if the key is not already present. This is similar to`dict.setdefault()` , but returns an integer value indicating if the
key was already present. It is also similar to`PyDict_SetDefault()` , but returns a strong reference instead of a
borrowed reference.

## Python 3.13.0 alpha 3Â¶

*Release date: 2024-01-17*

### SecurityÂ¶

### Core and BuiltinsÂ¶

- gh-107901: Compiler duplicates basic blocks that have an eval breaker check, no line number, and multiple predecessors.
- gh-107901: A jump leaving an exception handler back to normal code no longer checks the eval breaker.
- gh-113655: Set the C recursion limit to 4000 on Windows, and 10000 on Linux/OSX. This seems to be near the sweet spot to maintain safety, but not compromise backwards compatibility.
- gh-113710: Add typed stack effects to the interpreter DSL, along with various instruction annotations.
- gh-77046: On Windows, file descriptors wrapping Windows handles are now created non inheritable by default ( **PEP 446** ). Patch by Zackery Spytz
and Victor Stinner.
- gh-113853: Guarantee that all executors make progress. This then guarantees that tier 2 execution always makes progress.
- gh-113753: Fix an issue where the finalizer of `PyAsyncGenASend` objects
might not be called if they were allocated from a free list.
- gh-107901: Compiler changed so that synthetic jumps which are not at loop end no longer check the eval breaker.
- gh-113703: Fix a regression in the `codeop` module that was causing
it to incorrectly identify incomplete f-strings. Patch by Pablo Galindo
- gh-89811: Check for a valid `tp_version_tag` before performing bytecode
specializations that rely on this value being usable.
- gh-111488: Changed error message in case of no âinâ keyword after âforâ in list comprehensions
- gh-113657: Fix an issue that caused important instruction pointer updates to be optimized out of tier two traces.
- gh-113603: Fixed bug where a redundant NOP is not removed, causing an assertion to fail in the compiler in debug mode.
- gh-113602: Fix an error that was causing the parser to try to overwrite existing errors and crashing in the process. Patch by Pablo Galindo
- gh-113486: No longer issue spurious `PY_UNWIND` events for optimized
calls to classes.
- gh-113297: Fix segfault in the compiler on with statement with 19 context managers.
- gh-113212: Improve `super` error messages.
- gh-111375: Only use `NULL` in the exception stack to indicate an
exception was handled. Patch by Carey Metcalfe.
- gh-112215: Increase the C recursion limit by a factor of 3 for non-debug builds, except for webassembly and s390 platforms which are unchanged. This mitigates some regressions in 3.12 with deep recursion mixing builtin (C) and Python code.
- gh-113054: Fixed bug where a redundant NOP is not removed, causing an assertion to fail in the compiler in debug mode.
- gh-106905: Use per AST-parser state rather than global state to track recursion depth within the AST parser to prevent potential race condition due to simultaneous parsing. The issue primarily showed up in 3.11 by multithreaded users of `ast.parse()` .  In 3.12 a change to when garbage collection can be
triggered prevented the race condition from occurring.
- gh-108866: Change the API and contract of `_PyExecutorObject` to return
the next_instr pointer, instead of the frame, and to always execute at
least one instruction.
- gh-90350: Optimize builtin functions `min()` and`max()` .
- gh-112943: Correctly compute end column offsets for multiline tokens in the `tokenize` module. Patch by Pablo Galindo
- gh-112125: Fix `None.__ne__(None)` returning`NotImplemented` instead
of`False` .
- gh-74616: `input()` now raises a ValueError when output on the
terminal if the prompt contains embedded null characters instead of
silently truncating it.
- gh-112716: Fix SystemError in the `import` statement and in`__reduce__()` methods of builtin types when`__builtins__` is not a
dict.
- gh-112730: Use color to highlight error locations in tracebacks. Patch by Pablo Galindo
- gh-112625: Fixes a bug where a bytearray object could be cleared while iterating over an argument in the `bytearray.join()` method that could
result in reading memory after it was freed.
- gh-112660: Do not clear unexpected errors during formatting error messages for ImportError and AttributeError for modules.
- gh-105967: Workaround a bug in Appleâs macOS platform zlib library where `zlib.crc32()` and`binascii.crc32()` could produce incorrect
results on multi-gigabyte inputs. Including when using`zipfile` on
zips containing large data.
- gh-95754: Provide a better error message when accessing invalid attributes on partially initialized modules. The origin of the module being accessed is now included in the message to help with the common issue of shadowing other modules.
- gh-112217: Add check for the type of `__cause__` returned from calling
the type`T` in`raise from T` .
- gh-111058: Change coro.cr_frame/gen.gi_frame to return `None` after the
coroutine/generator has been closed. This fixes a bug where`getcoroutinestate()` and`getgeneratorstate()` return the wrong state for a closed coroutine/generator.
- gh-112388: Fix an error that was causing the parser to try to overwrite tokenizer errors. Patch by pablo Galindo
- gh-112387: Fix error positions for decoded strings with backwards tokenize errors. Patch by Pablo Galindo
- gh-99606: Make code generated for an empty f-string identical to the code of an empty normal string.
- gh-112367: Avoid undefined behaviour when using the perf trampolines by not freeing the code arenas until shutdown. Patch by Pablo Galindo
- gh-112320: The Tier 2 translator now tracks the confidence level for staying âon traceâ (i.e. not exiting back to the Tier 1 interpreter) for branch instructions based on the number of bits set in the branch âcounterâ. Trace translation ends when the confidence drops below 1/3rd.
- gh-109598: `PyComplex_RealAsDouble()` /`PyComplex_ImagAsDouble()` now
tries to convert an object to a`complex` instance using its`__complex__()` method before falling back to the`__float__()` method. Patch by Sergey B Kirpichev.
- gh-94606: Fix UnicodeEncodeError when `email.message.get_payload()` reads a message with a Unicode surrogate character and the message content
is not well-formed for surrogateescape encoding. Patch by Sidney
Markowitz.
- bpo-21861: Use the objectâs actual class name in `_io.FileIO.__repr__()` ,`_io._WindowsConsoleIO()` and`_io.TextIOWrapper.__repr__()` , to make these methods subclass
friendly.
- bpo-45369: Remove LibreSSL workarounds as per **PEP 644** .
- bpo-34392: Added `sys._is_interned()` .

### LibraryÂ¶

- gh-114077: Fix possible `OverflowError` in`socket.socket.sendfile()` when pass*count* larger than 2 GiB on
32-bit platform.
- gh-111803: `plistlib` now supports loading more deeply nested lists
in binary format.
- gh-114014: Fixed a bug in `fractions.Fraction` where an invalid
string using`d` in the decimals part creates a different error compared
to other invalid letters/characters. Patch by Jeremiah Gabriel Pascual.
- gh-108364: `sqlite3.Connection.iterdump()` now ensures that foreign
key support is disabled before dumping the database schema, if there is
any foreign key violation. Patch by Erlend E. Aasland and Mariusz
Felisiak.
- gh-113971: The `zipfile.ZipInfo` previously protected`._compresslevel` attribute has been made public as`.compress_level` with the old`_compresslevel` name remaining available as a property to
retain compatibility.
- gh-113877: Fix `tkinter` method`winfo_pathname()` on 64-bit
Windows.
- gh-113868: Added `mmap.MAP_NORESERVE` ,`mmap.MAP_NOEXTEND` ,`mmap.MAP_HASSEMAPHORE` ,`mmap.MAP_NOCACHE` ,`mmap.MAP_JIT` ,`mmap.MAP_RESILIENT_CODESIGN` ,`mmap.MAP_RESILIENT_MEDIA` ,`mmap.MAP_32BIT` ,`mmap.MAP_TRANSLATED_ALLOW_EXECUTE` ,`mmap.MAP_UNIX03` and`mmap.MAP_TPRO` . All of them are`mmap(2)` flags on macOS.
- gh-113848: `asyncio.TaskGroup()` and`asyncio.timeout()` context
managers now handle`CancelledError` subclasses as well as
exact`CancelledError` .
- gh-113661: unittest runner: Donât exit 5 if tests were skipped. The intention of exiting 5 was to detect issues where the test suite wasnât discovered at all. If we skipped tests, it was correctly discovered.
- gh-96037: Insert `TimeoutError` in the context of the exception that
was raised during exiting an expired`asyncio.timeout()` block.
- gh-113781: Silence unraisable AttributeError when warnings are emitted during Python finalization.
- gh-113238: Add `Anchor` to`importlib.resources` (in order for the
code to comply with the documentation)
- gh-111693: `asyncio.Condition.wait()` now re-raises the same`CancelledError` instance that may have caused it to be interrupted.
Fixed race condition in`asyncio.Semaphore.acquire()` when interrupted
with a`CancelledError` .
- gh-113791: Add `CLOCK_MONOTONIC_RAW_APPROX` and`CLOCK_UPTIME_RAW_APPROX` to`time` on macOS. These are clocks
available on macOS 10.12 or later.
- gh-112932: Restore the ability for `zipfile` to`extractall` from
zip files with a â/â directory entry in them as is commonly added to zips
by some wiki or bug tracker data exporters.
- gh-113568: Raise deprecation warnings from `pathlib.PurePath` and
not its private base class`PurePathBase` .
- gh-113594: Fix `UnicodeEncodeError` in`email` when re-fold
lines that contain unknown-8bit encoded part followed by non-unknown-8bit
encoded part.
- gh-113538: In `asyncio.StreamReaderProtocol.connection_made()` , there
is callback that logs an error if the task wrapping the âconnected
callbackâ fails. This callback would itself fail if the task was
cancelled. Prevent this by checking whether the task was cancelled first.
If so, close the transport but donât log an error.
- gh-113626: Add support for the *allow_code* argument in the`marshal` module. Passing`allow_code=False` prevents serialization and
de-serialization of code objects which is incompatible between Python
versions.
- gh-85567: Fix resource warnings for unclosed files in `pickle` and`pickletools` command line interfaces.
- gh-113537: Support loads `str` in`plistlib.loads()` .
- gh-89850: Add default implementations of `pickle.Pickler.persistent_id()` and`pickle.Unpickler.persistent_load()` methods in the C implementation.
Calling`super().persistent_id()` and`super().persistent_load()` in
subclasses of the C implementation of`pickle.Pickler` and`pickle.Unpickler` classes no longer causes infinite recursion.
- gh-113569: Indicate if there were no actual calls in unittest `assert_has_calls()` failure.
- gh-101225: Increase the backlog for `multiprocessing.connection.Listener` objects created by`multiprocessing.manager` and`multiprocessing.resource_sharer` to significantly reduce the risk of getting a connection refused error
when creating a`multiprocessing.connection.Connection` to them.
- gh-113568: Raise audit events from `pathlib.Path` and not its
private base class`PathBase` .
- gh-113543: Make sure that `webbrowser.MacOSXOSAScript` sends`webbrowser.open` audit event.
- gh-113028: When a second reference to a string appears in the input to `pickle` , and the Python implementation is in use, we are guaranteed
that a single copy gets pickled and a single object is shared when
reloaded. Previously, in protocol 0, when a string contained certain
characters (e.g. newline) it resulted in duplicate objects.
- gh-113421: Fix multiprocessing logger for `%(filename)s` .
- gh-111784: Fix segfaults in the `_elementtree` module. Fix first
segfault during deallocation of`_elementtree.XMLParser` instances by
keeping strong reference to`pyexpat` module in module state for capsule
lifetime. Fix second segfault which happens in the same deallocation
process  by keeping strong reference to`_elementtree` module in`XMLParser` structure for`_elementtree` module lifetime.
- gh-113407: Fix import of `unittest.mock` when CPython is built
without docstrings.
- gh-113320: Fix regression in Python 3.12 where `Protocol` classes that were not marked as`runtime-checkable` would be unnecessarily introspected,
potentially causing exceptions to be raised if the protocol had
problematic members. Patch by Alex Waygood.
- gh-53502: Add a new option `aware_datetime` in`plistlib` to loads
or dumps aware datetime.
- gh-113358: Fix rendering tracebacks with exceptions with a broken __getattr__
- gh-113214: Fix an `AttributeError` during asyncio SSL protocol aborts in
SSL-over-SSL scenarios.
- gh-113246: Update bundled pip to 23.3.2.
- gh-87264: Fixed tarfile list() method to show file type.
- gh-112182: `asyncio.futures.Future.set_exception()` now transforms`StopIteration` into`RuntimeError` instead of hanging or other
misbehavior. Patch contributed by Jamie Phan.
- gh-113225: Speed up `pathlib.Path.glob()` by using`os.DirEntry.path` where possible.
- gh-113149: Improve error message when a JSON array or object contains a trailing comma. Patch by Carson Radtke.
- gh-113117: The `subprocess` module can now use the`os.posix_spawn()` function with`close_fds=True` on platforms where`posix_spawn_file_actions_addclosefrom_np` is available. Patch by Jakub
Kulik.
- gh-113199: Make `http.client.HTTPResponse.read1` and`http.client.HTTPResponse.readline` close IO after reading all data when
content length is known. Patch by Illia Volochii.
- gh-113191: Add support of `os.fchmod()` and a file descriptor in`os.chmod()` on Windows.
- gh-113188: Fix `shutil.copymode()` and`shutil.copystat()` on
Windows. Previously they worked differently if*dst* is a symbolic link:
they modified the permission bits of*dst* itself rather than the file it
points to if*follow_symlinks* is true or*src* is not a symbolic link,
and did not modify the permission bits if*follow_symlinks* is false and*src* is a symbolic link.
- gh-113119: `os.posix_spawn()` now accepts`env=None` , which makes
the newly spawned process use the current process environment. Patch by
Jakub Kulik.
- gh-113202: Add a `strict` option to`batched()` in the`itertools` module.
- gh-61648: Detect line numbers of properties in doctests.
- gh-113175: Sync with importlib_metadata 7.0, including improved type annotations, fixed issue with symlinked packages in `package_distributions` , added`EntryPoints.__repr__` , introduced the`diagnose` script, added`Distribution.origin` property, and removed
deprecated`EntryPoint` access by numeric index (tuple behavior).
- gh-59616: Add support of `os.lchmod()` and the*follow_symlinks* argument in`os.chmod()` on Windows. Note that the default value of*follow_symlinks* in`os.lchmod()` is`False` on Windows.
- gh-112559: `signal.signal()` and`signal.getsignal()` no longer
call`repr` on callable handlers.`asyncio.run()` and`asyncio.Runner.run()` no longer call`repr` on the task results.
Patch by Yilei Yang.
- gh-112962: `dis` module functions add cache information to the`Instruction` instance rather than creating fake`Instruction` instances to represent the cache entries.
- gh-112989: Reduce overhead to connect sockets with `asyncio` SelectorEventLoop.
- gh-112970: Use `closefrom()` on Linux where available (e.g.
glibc-2.34), rather than only FreeBSD.
- gh-110190: Fix ctypes structs with array on PPC64LE platform by setting `MAX_STRUCT_SIZE` to 64 in stgdict. Patch by Diego Russo.
- gh-112540: The statistics.geometric_mean() function now returns zero for datasets containing a zero. Formerly, it would raise an exception.
- gh-87286: Added `LOG_FTP` ,`LOG_NETINFO` ,`LOG_REMOTEAUTH` ,`LOG_INSTALL` ,`LOG_RAS` , and`LOG_LAUNCHD` tot the`syslog` module, all of them constants
on used on macOS.
- gh-112800: Fix `asyncio``SubprocessTransport.close()` not to throw`PermissionError` when used with setuid executables.
- gh-51944: Add the following constants to the `termios` module. These
values are present in macOS system headers:`ALTWERASE` ,`B14400` ,`B28800` ,`B7200` ,`B76800` ,`CCAR_OFLOW` ,`CCTS_OFLOW` ,`CDSR_OFLOW` ,`CDTR_IFLOW` ,`CIGNORE` ,`CRTS_IFLOW` ,`EXTPROC` ,`IUTF8` ,`MDMBUF` ,`NL2` ,`NL3` ,`NOKERNINFO` ,`ONOEOT` ,`OXTABS` ,`VDSUSP` ,`VSTATUS` .
- gh-79325: Fix an infinite recursion error in `tempfile.TemporaryDirectory()` cleanup on Windows.
- gh-94692: `shutil.rmtree()` now only catches OSError exceptions.
Previously a symlink attack resistant version of`shutil.rmtree()` could
ignore or pass to the error handler arbitrary exception when invalid
arguments were provided.
- gh-112736: The use of del-safe symbols in `subprocess` was refactored to
allow for use in cross-platform build environments.
- gh-112727: Speed up `pathlib.Path.absolute()` . Patch by Barney Gale.
- gh-74690: Speedup `issubclass()` checks against simple`runtime-checkable protocols` by around
6%. Patch by Alex Waygood.
- gh-74690: Speedup `isinstance()` checks by roughly 20% for`runtime-checkable protocols` that only
have one callable member. Speedup`issubclass()` checks for these
protocols by roughly 10%. Patch by Alex Waygood.
- gh-112645: Remove deprecation error on passing `onerror` to`shutil.rmtree()` .
- gh-112640: Add `kwdefaults` parameter to`types.FunctionType` to
set default keyword argument values.
- gh-112622: Ensure `name` parameter is passed to event loop in`asyncio.create_task()` .
- gh-112618: Fix a caching bug relating to `typing.Annotated` .`Annotated[str, True]` is no longer identical to`Annotated[str, 1]` .
- gh-112334: Fixed a performance regression in 3.12âs `subprocess` on
Linux where it would no longer use the fast-path`vfork()` system call
when it could have due to a logic bug, instead falling back to the safe
but slower`fork()` .Also fixed a second 3.12.0 potential security bug. If a value of `extra_groups=[]` was passed to`subprocess.Popen` or related APIs,
the underlying`setgroups(0, NULL)` system call to clear the groups list
would not be made in the child process prior to`exec()` .This was identified via code inspection in the process of fixing the first bug.
- gh-110190: Fix ctypes structs with array on Arm platform by setting `MAX_STRUCT_SIZE` to 32 in stgdict. Patch by Diego Russo.
- gh-81194: Fix a crash in `socket.if_indextoname()` with specific value
(UINT_MAX). Fix an integer overflow in`socket.if_indextoname()` on
64-bit non-Windows platforms.
- gh-112578: Fix a spurious `RuntimeWarning` when executing the`zipfile` module.
- gh-112516: Update the bundled copy of pip to version 23.3.1.
- gh-112510: Add `readline.backend` for the backend readline uses
(`editline` or`readline` )
- gh-112328: [Enum] Make `EnumDict` ,`EnumDict.member_names` ,`EnumType._add_alias_` and`EnumType._add_value_alias_` public.
- gh-112509: Fix edge cases that could cause a key to be present in both the `__required_keys__` and`__optional_keys__` attributes of a`typing.TypedDict` . Patch by Jelle Zijlstra.
- gh-101336: Add `keep_alive` keyword parameter for`AbstractEventLoop.create_server()` and`BaseEventLoop.create_server()` .
- gh-63284: Added support for TLS-PSK (pre-shared key) mode to the `ssl` module.
- gh-112414: Fix regression in Python 3.12 where calling `repr()` on a
module that had been imported using a custom loader could fail
with`AttributeError` . Patch by Alex Waygood.
- gh-112358: Revert change to `struct.Struct` initialization that
broke some cases of subclassing.
- gh-112405: Optimize `pathlib.PurePath.relative_to()` . Patch by Alex
Waygood.
- gh-94722: Fix bug where comparison between instances of `DocTest` fails if one of them has`None` as its lineno.
- gh-112361: Speed up a small handful of `pathlib` methods by removing
some temporary objects.
- gh-112345: Improve error message when trying to call `issubclass()` against a`typing.Protocol` that has non-method members. Patch by
Randolf Scholz.
- gh-112137: Change `dis` output to display no-lineno as âââ instead
of âNoneâ.
- gh-112332: Deprecate the `exc_type` field of`traceback.TracebackException` . Add`exc_type_str` to replace it.
- gh-81620: Add extra tests for `random.binomialvariate()`
- gh-112292: Fix a crash in `readline` when imported from a sub
interpreter. Patch by Anthony Shaw
- gh-77621: Slightly improve the import time of the `pathlib` module by
deferring some imports. Patch by Barney Gale.
- gh-112137: Change `dis` output to display logical labels for jump
targets instead of offsets.
- gh-112139: Add `Signature.format()` to format signatures to string
with extra options. And use it in`pydoc` to render more readable
signatures that have new lines between parameters.
- gh-112105: Make `readline.set_completer_delims()` work with libedit
- gh-106922: Display multiple lines with `traceback` when errors span
multiple lines.
- gh-111874: When creating a `typing.NamedTuple` class, ensure`__set_name__()` is called on all objects that define`__set_name__` and exist in the values of the`NamedTuple` classâs
class dictionary. Patch by Alex Waygood.
- gh-68166: Add support of the âvsapiâ element type in `tkinter.ttk.Style.element_create()` .
- gh-110275: Named tupleâs methods `_replace()` and`__replace__()` now
raise TypeError instead of ValueError for invalid keyword arguments.
- gh-99367: Do not mangle `sys.path[0]` in`pdb` if safe_path is set
- gh-111615: Fix a regression caused by a fix to gh-93162 whereby you couldnât configure a `QueueHandler` without specifying handlers.
- gh-75666: Fix the behavior of `tkinter` widgetâs`unbind()` method
with two arguments. Previously,`widget.unbind(sequence, funcid)` destroyed the current binding for*sequence* , leaving*sequence* unbound,
and deleted the*funcid* command. Now it removes only*funcid* from the
binding for*sequence* , keeping other commands, and deletes the*funcid* command. It leaves*sequence* unbound only if*funcid* was the last bound
command.
- gh-67790: Implement basic formatting support (minimum width, alignment, fill) for `fractions.Fraction` .
- gh-111049: Fix crash during garbage collection of the `io.BytesIO` buffer object.
- gh-102980: Redirect the output of `interact` command of`pdb` to
the same channel as the debugger. Add tests and improve docs.
- gh-102988: `email.utils.getaddresses()` and`email.utils.parseaddr()` now return`('', '')` 2-tuples in more
situations where invalid email addresses are encountered instead of
potentially inaccurate values. Add optional*strict* parameter to these
two functions: use`strict=False` to get the old behavior, accept
malformed inputs.```
getattr(email.utils, 'supports_strict_parsing',
False)
```
can be use to check if the*strict* parameter is available. Patch
by Thomas Dwyer and Victor Stinner to improve the**CVE 2023-27043** fix.
- gh-52161: `cmd.Cmd.do_help()` now cleans docstrings with`inspect.cleandoc()` before writing them. Patch by Filip Åapkiewicz.
- gh-82300: Add `track` parameter to`multiprocessing.shared_memory.SharedMemory` that allows using
shared memory blocks without having to register with the POSIX resource
tracker that automatically releases them upon process exit.
- gh-110109: Add private `pathlib._PurePathBase` class: a base class for`pathlib.PurePath` that omits certain magic methods. It may be made
public (along with`_PathBase` ) in future.
- gh-109858: Protect `zipfile` from âquoted-overlapâ zipbomb. It now
raises BadZipFile when try to read an entry that overlaps with other entry
or central directory.
- gh-109786: Fix possible reference leaks and crash when re-enter the `__next__()` method of`itertools.pairwise` .
- gh-91539: Small (10 - 20%) and trivial performance improvement of `urllib.request.getproxies_environment()` , typically useful when there
are many environment variables to go over.
- gh-103363: Add *follow_symlinks* keyword-only argument to`pathlib.Path.owner()` and`group()` , defaulting to`True` .
- gh-102130: Support tab completion in `cmd` for`editline` .
- gh-99437: `runpy.run_path()` now decodes path-like objects, making
sure __file__ and sys.argv[0] of the module being run are always strings.
- gh-104003: Add `warnings.deprecated()` , a decorator to mark deprecated
functions to static type checkers and to warn on usage of deprecated
classes and functions. See**PEP 702** . Patch by Jelle Zijlstra.
- gh-103708: Make hardcoded python name, a configurable parameter so that different implementations of python can override it instead of making huge diffs in sysconfig.py
- gh-66515: `mailbox.MH` now supports folders that do not contain a`.mh_sequences` file (e.g. Claws Mail IMAP-cache folders). Patch by
Serhiy Storchaka.
- gh-83162: Renamed `re.error` to`PatternError` for clarity, and
kept`re.error` for backward compatibility. Patch by Matthias
Bussonnier and Adam Chhina.
- gh-91133: Fix a bug in `tempfile.TemporaryDirectory` cleanup, which
now no longer dereferences symlinks when working around file system
permission errors.
- bpo-43153: On Windows, `tempfile.TemporaryDirectory` previously masked a`PermissionError` with`NotADirectoryError` during directory cleanup.
It now correctly raises`PermissionError` if errors are not ignored.
Patch by Andrei Kulakov and Ken Jin.
- bpo-32731: `getpass.getuser()` now raises`OSError` for all
failures rather than`ImportError` on systems lacking the`pwd` module or`KeyError` if the password database is empty.
- bpo-34321: `mmap.mmap` now has a*trackfd* parameter on Unix; if it
is`False` , the file descriptor specified by*fileno* will not be
duplicated.
- bpo-35332: The `shutil.rmtree()` function now ignores errors when
calling`os.close()` when*ignore_errors* is`True` , and`os.close()` no longer retried after error.
- bpo-35928: `io.TextIOWrapper` now correctly handles the decoding
buffer after`read()` and`write()` .
- bpo-26791: `shutil.move()` now moves a symlink into a directory when
that directory is the target of the symlink.  This provides the same
behavior as the mv shell command.  The previous behavior raised an
exception.  Patch by Jeffrey Kintscher.
- bpo-41422: Fixed memory leaks of `pickle.Pickler` and`pickle.Unpickler` involving cyclic references via the internal
memo mapping.
- bpo-19821: The `pydoc.ispackage()` function has been deprecated.
- bpo-40262: The `ssl.SSLSocket.recv_into()` method no longer requires
the*buffer* argument to implement`__len__` and supports buffers with
arbitrary item size.
- bpo-39912: `warnings.filterwarnings()` and`warnings.simplefilter()` now raise appropriate exceptions instead of`AssertionError` . Patch contributed by RÃ©mi Lapeyre.
- bpo-37260: Fixed a race condition in `shutil.rmtree()` in which
directory entries removed by another process or thread while`shutil.rmtree()` is running can cause it to raise FileNotFoundError.
Patch by Jeffrey Kintscher.
- bpo-36959: Fix some error messages for invalid ISO format string combinations in `strptime()` that referred to directives not contained
in the format string. Patch by Gordon P. Hemsley.
- bpo-18060: Fixed a class inheritance issue that can cause segfaults when deriving two or more levels of subclasses from a base class of Structure or Union.
- bpo-29779: Add a new `PYTHON_HISTORY` environment variable to set
the location of a`.python_history` file.
- bpo-21360: `mailbox.Maildir` now ignores files with a leading dot.

### DocumentationÂ¶

- gh-111699: Relocate `smtpd` deprecation notice to its own section rather
than under`locale` in Whatâs New in Python 3.12 document
- gh-110746: Improved markup for valid options/values for methods ttk.treeview.column and ttk.treeview.heading, and for Layouts.
- gh-95649: Document that the `asyncio` module contains code taken from
v0.16.0 of the uvloop project, as well as the
required MIT licensing information.

### TestsÂ¶

- gh-111798: Disable `test_super_deep()` from`test_call` under pydebug
builds on WASI; the stack depth is too small to make the test useful.
- gh-111801: Lower the recursion limit in `test_isinstance` for`test_infinitely_many_bases()` . This prevents a stack overflow on a
pydebug build of WASI.
- gh-111802: Specify a low recursion depth for `test_bad_getattr()` in`test.pickletester` to avoid exhausting the stack under a pydebug build
for WASI.
- gh-44626: Fix `os.path.isabs()` incorrectly returning`True` when
given a path that starts with exactly one (back)slash on Windows.Fix `pathlib.PureWindowsPath.is_absolute()` incorrectly returning`False` for some paths beginning with two (back)slashes.
- gh-113633: Use module state for the _testcapi extension module.
- gh-109980: Fix `test_tarfile_vs_tar` in`test_shutil` for macOS, where
system tar can include more information in the archive than`shutil.make_archive` .
- gh-112769: The tests now correctly compare zlib version when `zlib.ZLIB_RUNTIME_VERSION` contains non-integer suffixes. For
example zlib-ng defines the version as`1.3.0.zlib-ng` .
- gh-112334: Adds a regression test to verify that `vfork()` is used when
expected by`subprocess` on vfork enabled POSIX systems (Linux).
- gh-108927: Fixed order dependence in running tests in the same process when a test that has submodules (e.g. test_importlib) follows a test that imports its submodule (e.g. test_importlib.util) and precedes a test (e.g. test_unittest or test_compileall) that uses that submodule.
- bpo-40648: Test modes that file can get with chmod() on Windows.

### BuildÂ¶

- gh-114013: Fix `Tools/wasm/wasi.py` to not include the path to`python.wasm` as part of`HOSTRUNNER` . The environment variable is
meant to specify how to run the WASI host only, having`python.wasm` and
relevant flags appended to the`HOSTRUNNER` . This fixes`make test` work.
- gh-113258: Changed the Windows build to write out generated frozen modules into the build tree instead of the source tree.
- gh-112305: Fixed the `check-clean-src` step performed on out of tree
builds to detect errant`$(srcdir)/Python/frozen_modules/*.h` files and
recommend appropriate source tree cleanup steps to get a working build
again.
- gh-112536: Add support for thread sanitizer (TSAN)
- gh-112867: Fix the build for the case that WITH_PYMALLOC_RADIX_TREE=0 set.
- gh-103065: Introduce `Tools/wasm/wasi.py` to simplify doing a WASI
build.
- bpo-11102: The `os.major()` ,`os.makedev()` , and`os.minor()` functions are now available on HP-UX v3.
- bpo-36351: Do not set ipv6type when cross-compiling.

### WindowsÂ¶

- gh-114096: Process privileges that are activated for creating directory junctions are now restored afterwards, avoiding behaviour changes in other parts of the program.
- gh-111877: `os.stat()` calls were returning incorrect time values for
files that could not be accessed directly.
- gh-111973: Update Windows installer to use SQLite 3.44.2.
- gh-113009: `multiprocessing` : On Windows, fix a race condition in`Process.terminate()` : no longer set the`returncode` attribute to
always call`WaitForSingleObject()` in`Process.wait()` .  Previously,
sometimes the process was still running after`TerminateProcess()` even
if`GetExitCodeProcess()` is not`STILL_ACTIVE` . Patch by Victor
Stinner.
- gh-86179: Fixes path calculations when launching Python on Windows through a symlink.
- gh-71383: Update Tcl/Tk in Windows installer to 8.6.13 with a patch to suppress incorrect ThemeChanged warnings.
- gh-111650: Ensures the `Py_GIL_DISABLED` preprocessor variable is
defined in`pyconfig.h` so that extension modules written in C are
able to use it.
- gh-112278: Reduce the time cost for some functions in `platform` on
Windows if current user has no permission to the WMI.
- gh-73427: Deprecate `sys._enablelegacywindowsfsencoding()` . Use`PYTHONLEGACYWINDOWSFSENCODING` instead. Patch by Inada Naoki.
- gh-87868: Correctly sort and remove duplicate environment variables in `_winapi.CreateProcess()` .
- bpo-37308: Fix mojibake in `mmap.mmap` when using a non-ASCII*tagname* argument on Windows.

### macOSÂ¶

- gh-113666: Add the following constants to module `stat` :`UF_SETTABLE` ,`UF_TRACKED` ,`UF_DATAVAULT` ,`SF_SUPPORTED` ,`SF_SETTABLE` ,`SF_SYNTHETIC` ,`SF_RESTRICTED` ,`SF_FIRMLINK` and`SF_DATALESS` . The values`UF_SETTABLE` ,`SF_SUPPORTED` ,`SF_SETTABLE` and`SF_SYNTHETIC` are only available on macOS.
- gh-113536: `os.waitid()` is now available on macOS
- gh-110459: Running `configure ... --with-openssl-rpath=X/Y/Z` no longer
fails to detect OpenSSL on macOS.
- gh-74573: Document that `dbm.ndbm` can silently corrupt DBM files on
updates when exceeding undocumented platform limits, and can crash
(segmentation fault) when reading such a corrupted file. (FB8919203)
- gh-65701: The **freeze** tool doesnât work with framework builds of
Python. Document this and bail out early when running the tool with such a
build.
- gh-87277: webbrowser: Donât look for X11 browsers on macOS. Those are generally not used and probing for them can result in starting XQuartz even if it isnât used otherwise.
- gh-111973: Update macOS installer to use SQLite 3.44.2.
- gh-108269: Set `CFBundleAllowMixedLocalizations` to true in the
Info.plist for the framework, embedded Python.app and IDLE.app with
framework installs on macOS.  This allows applications to pick up the
userâs preferred locale when thatâs different from english.
- gh-102362: Make sure the result of `sysconfig.get_plaform()` includes
at least a major and minor versions, even if`MACOSX_DEPLOYMENT_TARGET` is set to only a major version during build to match the format expected
by pip.
- gh-110017: Disable a signal handling stress test on macOS due to a bug in macOS (FB13453490).
- gh-110820: Make sure the preprocessor definitions for `ALIGNOF_MAX_ALIGN_T` ,`SIZEOF_LONG_DOUBLE` and`HAVE_GCC_ASM_FOR_X64` are correct for Universal 2 builds on macOS.
- gh-109981: Use `/dev/fd` on macOS to determine the number of open files
in`test.support.os_helper.fd_count` to avoid a crash with âguardedâ
file descriptors when probing for open files.

### IDLEÂ¶

- gh-72284: Improve the lists of features, editor key bindings, and shell key bingings in the IDLE doc.
- gh-113903: Fix rare failure of test.test_idle, in test_configdialog.
- gh-113729: Fix the âHelp -> IDLE Docâ menu bug in 3.11.7 and 3.12.1.
- gh-113269: Fix test_editor hang on macOS Catalina.
- gh-112898: Fix processing unsaved files when quitting IDLE on macOS.
- bpo-13586: Enter the selected text when opening the âReplaceâ dialog.

### C APIÂ¶

- gh-106560: Fix redundant declarations in the public C API. Declare PyBool_Type, PyLong_Type and PySys_Audit() only once. Patch by Victor Stinner.
- gh-112438: Fix support of format units âesâ, âetâ, âes#â, and âet#â in nested tuples in `PyArg_ParseTuple()` -like functions.
- gh-111545: Add `Py_HashPointer()` function to hash a pointer. Patch
by Victor Stinner.
- gh-65210: Change the declaration of the *keywords* parameter of`PyArg_ParseTupleAndKeywords()` and`PyArg_VaParseTupleAndKeywords()` for better compatibility with C++.

## Python 3.13.0 alpha 2Â¶

*Release date: 2023-11-22*

### Core and BuiltinsÂ¶

- gh-112243: Donât include comments in f-string debug expressions. Patch by Pablo Galindo
- gh-112287: Slightly optimize the Tier 2 (uop) interpreter by only loading `oparg` and`operand` when needed. Also double the trace size limit
again, to 512 this time.
- gh-112266: Change docstrings of `__dict__` and`__weakref__` .
- gh-111807: Lower the max parser stack depth to 1000 under WASI debug builds.
- gh-111798: When Python is built in debug mode, set the C recursion limit to 500 instead of 1500. A debug build is likely built with low optimization level which implies higher stack memory usage than a release build. Patch by Victor Stinner.
- gh-106529: Enable translating unspecialized `FOR_ITER` to Tier 2.
- gh-111916: Make hashlib related modules thread-safe without the GIL
- gh-81137: Deprecate assignment to a functionâs `__code__` field when the
new code object is of a mismatched type (e.g., from a generator to a plain
function).
- gh-79932: Raise exception if `frame.clear()` is called on a suspended
frame.
- gh-81925: Implement native thread ids for GNU KFreeBSD.
- gh-111843: Use exponential backoff to reduce the number of failed tier 2 optimization attempts by over 99%.
- gh-110829: Joining a thread now ensures the underlying OS thread has exited. This is required for safer fork() in multi-threaded processes.
- gh-109369: Make sure that tier 2 traces are de-optimized if the code is instrumented
- gh-111772: Specialize slot loads and stores for _Py_T_OBJECT as well as Py_T_OBJECT_EX
- gh-111666: Speed up `BaseExceptionGroup.derive()` ,`BaseExceptionGroup.subgroup()` , and`BaseExceptionGroup.split()` by changing how they parse passed arguments.
- gh-111654: Fix runtime crash when some error happens in opcode `LOAD_FROM_DICT_OR_DEREF` .
- gh-111623: Add support for sharing tuples between interpreters using the cross-interpreter API. Patch by Anthony Shaw.
- gh-111354: The oparg of `YIELD_VALUE` is now`1` if the
instruction is part of a yield-from or await, and`0` otherwise.The SUSPENDED frame state is now split into `SUSPENDED` and`SUSPENDED_YIELD_FROM` . This simplifies the code in`_PyGen_yf` .
- gh-111520: Merge the Tier 1 (bytecode) and Tier 2 (micro-ops) interpreters together, moving the Tier 2 interpreter loop and switch into `_PyEval_EvalFrameDefault()` in`Python/ceval.c` . The`Python/executor.c` file is gone. Also the`TIER_ONE` and`TIER_TWO` macros are now handled by the code generator.**Beware!** This changes the environment variables to enable micro-ops and
their debugging to`PYTHON_UOPS` and`PYTHON_LLTRACE` .
- gh-109181: Speed up `Traceback` object creation by lazily compute
the line number. Patch by Pablo Galindo
- gh-111420: Allow type comments in parenthesized `with` statements
- gh-111438: Add support for sharing floats between interpreters using the cross-interpreter API. Patch by Anthony Shaw.
- gh-111435: Add support for sharing of `True` and`False` between
interpreters using the cross-interpreter API.  Patch by Anthony Shaw.
- gh-102388: Fix a bug where `iso2022_jp_3` and`iso2022_jp_2004` codecs
read out of bounds
- gh-111366: Fix an issue in the `codeop` that was causing`SyntaxError` exceptions raised in the presence of invalid syntax to
not contain precise error messages. Patch by Pablo Galindo
- gh-111380: Fix a bug that was causing `SyntaxWarning` to appear twice
when parsing if invalid syntax is encountered later. Patch by Pablo
galindo
- gh-111374: Added a new environment variable `PYTHON_FROZEN_MODULES` . It determines whether or not frozen
modules are ignored by the import machinery, equivalent of the```
-X
frozen_modules
```
command-line option.
- gh-111354: Remove `oparg` from`YIELD_VALUE` . Change`oparg` of`RESUME` to include information about the except-depth. These
changes make it possible to simplify the code in generator close.
- gh-94438: Fix a regression that prevented jumping across `is None` and`is not None` when debugging. Patch by Savannah Ostrowski.
- gh-67224: Show source lines in tracebacks when using the `-c` option
when running Python. Patch by Pablo Galindo
- gh-111123: Fix a bug where a `global` declaration in an`except` block is rejected when the global is used in the`else` block.
- gh-110938: Fix error messages for indented blocks with functions and classes with generic type parameters. Patch by Pablo Galindo
- gh-109214: Remove unnecessary instruction pointer updates before returning from frames.
- gh-110912: Correctly display the traceback for `MemoryError` exceptions using the`traceback` module. Patch by Pablo Galindo
- gh-109894: Fixed crash due to improperly initialized static `MemoryError` in subinterpreter.
- gh-110892: Return `NULL` for`PyTrace_RETURN` events caused by an
exception
- gh-110864: Fix argument parsing by `_PyArg_UnpackKeywordsWithVararg` for
functions defining pos-or-keyword, vararg, and kw-only parameters.
- gh-109094: Replace `prev_instr` on the interpreter frame by`instr_ptr` which points to the beginning of the instruction that is
currently executing (or will execute once the frame resumes).
- gh-110805: Allow the repl to show source code and complete tracebacks. Patch by Pablo Galindo
- gh-110722: Add `PYTHON_PRESITE=package.module` to import a module
early in the interpreter lifecycle before`site.py` is executed.  Python
needs to be built in debug mode for this option to
exist.
- gh-110481: Implement biased reference counting in `--disable-gil` builds.
- gh-110543: Fix regression in Python 3.12 where `types.CodeType.replace()` would produce a broken code object if
called on a module or class code object that contains a comprehension.
Patch by Jelle Zijlstra.
- gh-89519: Removed chained `classmethod` descriptors (introduced in
bpo-19072).  This can no longer be used to wrap other descriptors
such as`property` .  The core design of this feature was flawed and
caused a number of downstream problems.  To âpass-throughâ a`classmethod` , consider using the`__wrapped__` attribute
that was added in Python 3.10.
- gh-103615: Use local events for opcode tracing
- bpo-46657: Add mimalloc memory allocator support.
- gh-106718: When PyConfig.stdlib_dir is explicitly set, itâs now respected and wonât be overridden by PyConfig.home.
- gh-106905: Fix incorrect SystemError about AST constructor recursion depth mismatch.
- gh-100445: Improve error message for unterminated strings with escapes.
- bpo-45759: Improved error messages for `elif` /`else` statements not
matching any valid statements. Patch by Jeremiah Vivian.

### LibraryÂ¶

- gh-111942: Fix SystemError in the TextIOWrapper constructor with non-encodable âerrorsâ argument in non-debug mode.
- gh-111995: Added the `NI_IDN` constant to the`socket` module when
present in C at build time for use with`socket.getnameinfo()` .
- gh-109538: Issue warning message instead of having `RuntimeError` be displayed when event loop has already been closed at`StreamWriter.__del__()` .
- gh-111942: Fix crashes in `io.TextIOWrapper.reconfigure()` when pass
invalid arguments, e.g. non-string encoding.
- gh-111460: `curses` : restore wide character support (including`curses.unget_wch()` and`get_wch()` ) on macOS,
which was unavailable due to a regression in Python 3.12.
- gh-103791: `contextlib.suppress` now supports suppressing
exceptions raised as part of a`BaseExceptionGroup` , in addition to
the recent support for`ExceptionGroup` .
- gh-111835: The `mmap.mmap` class now has an`seekable()` method that can be used when a seekable
file-like object is required. The`seek()` method now
returns the new absolute position. Patch by Donghee Na.
- gh-111804: Remove posix.fallocate() under WASI as the underlying posix_fallocate() is not available in WASI preview2.
- gh-111841: Fix truncating arguments on an embedded null character in `os.putenv()` and`os.unsetenv()` on Windows.
- gh-111768: `wsgiref.util.is_hop_by_hop()` is now exposed correctly in`__all__` .
- gh-80731: Avoid executing the default function in `cmd.Cmd` in an
except block
- gh-111541: Fix `doctest` for`SyntaxError` not-builtin
subclasses.
- gh-111719: Add extra argument validation for `alias` command in`pdb`
- gh-111482: `time` : Make`time.clock_gettime()` and`time.clock_gettime_ns()` functions up to 2x faster by faster calling
convention. Patch by Victor Stinner.
- gh-110894: Call loop exception handler for exceptions in `client_connected_cb` of`asyncio.start_server()` so that
applications can handle it. Patch by Kumar Aditya.
- gh-111531: Fix reference leaks in `bind_class()` and`bind_all()` methods of`tkinter` widgets.
- gh-111246: `asyncio.loop.create_unix_server()` will now automatically
remove the Unix socket when the server is closed.
- gh-111356: Added `io.text_encoding()` ,`io.DEFAULT_BUFFER_SIZE` ,
and`io.IncrementalNewlineDecoder` to`io.__all__` .
- gh-66425: Remove the code to set the REMOTE_HOST header from wsgiref module, as it is unreachable. This header is used for performance reasons, which is not necessary in the wsgiref module.
- gh-111429: Speed up `pathlib.PurePath.relative_to()` and`is_relative_to()` .
- gh-111342: Fixed typo in `math.sumprod()` .
- gh-68166: Remove mention of not supported âvsapiâ element type in `tkinter.ttk.Style.element_create()` . Add tests for`element_create()` and other`ttk.Style` methods. Add examples for`element_create()` in the documentation.
- gh-111388: Add `show_group` parameter to`traceback.format_exception_only()` , which allows to format`ExceptionGroup` instances.
- gh-79033: Another attempt at fixing `asyncio.Server.wait_closed()` . It
now blocks until both conditions are true: the server is closed,*and* there are no more active connections. (This means that in some cases where
in 3.12.0 this function would*incorrectly* have returned immediately, it
will now block; in particular, when there are no active connections but
the server hasnât been closed yet.)
- gh-111259: Optimize recursive wildcards in `pathlib` .
- gh-111295: Fix `time` not checking for errors when initializing.
- gh-111253: Add error checking during `_socket` module init.
- gh-111251: Fix `_blake2` not checking for errors when initializing.
- gh-111233: Fix `select` not checking for errors when initializing.
- gh-111230: Fix `ssl` not checking for errors when initializing.
- gh-111174: Fix crash in `io.BytesIO.getbuffer()` called repeatedly for
empty BytesIO.
- gh-111187: Postpone removal version for locale.getdefaultlocale() to Python 3.15.
- gh-111159: Fix `doctest` output comparison for exceptions with notes.
- gh-110910: Fix invalid state handling in `asyncio.TaskGroup` and`asyncio.Timeout` . They now raise proper RuntimeError if they are
improperly used and are left in consistent state after this.
- gh-111092: Make turtledemo run without default root enabled.
- gh-110944: Support alias and convenience vars for `pdb` completion
- gh-110745: Added *newline* parameter to`pathlib.Path.read_text()` .
Patch by Junya Okabe.
- gh-84583: Make `pdb` enter post-mortem mode even for`SyntaxError`
- gh-80675: Set `f_trace_lines = True` on all frames upon`pdb.set_trace()`
- gh-110771: Expose the setup and cleanup portions of `asyncio.run_forever()` as the standalone methods`asyncio.run_forever_setup()` and`asyncio.run_forever_cleanup()` .
This allows for tighter integration with GUI event loops.
- gh-110774: Support setting the `asyncio.Runner` loop_factory kwarg
in`unittest.IsolatedAsyncioTestCase`
- gh-110392: Fix `tty.setraw()` and`tty.setcbreak()` : previously
they returned partially modified list of the original tty attributes.`tty.cfmakeraw()` and`tty.cfmakecbreak()` now make a copy of the
list of special characters before modifying it.
- gh-59013: Make line number of function breakpoint more precise in `pdb`
- gh-88434: Emit deprecation warning for non-integer numbers in `gettext` functions and methods that consider plural forms even if
the translation was not found.
- gh-110395: Ensure that `select.kqueue()` objects correctly appear as
closed in forked children, to prevent operations on an invalid file
descriptor.
- gh-110196: Add `__reduce__` method to`IPv6Address` in order to
keep`scope_id`
- gh-109747: Improve errors for unsupported look-behind patterns. Now re.error is raised instead of OverflowError or RuntimeError for too large width of look-behind pattern.
- gh-109466: Add the `ipaddress.IPv4Address.ipv6_mapped` property,
which returns the IPv4-mapped IPv6 address.
- gh-85098: Implement the CLI of the `symtable` module and improve the
repr of`Symbol` .
- gh-108791: Improved error handling in `pdb` command line interface,
making it produce more concise error messages.
- gh-105931: Change `compileall` to only strip the stripdir prefix from
the full path recorded in the compiled`.pyc` file, when the prefix
matches the start of the full path in its entirety. When the prefix does
not match, no stripping is performed and a warning to this effect is
displayed.Previously all path components of the stripdir prefix that matched the full path were removed, while those that did not match were left alone (including ones interspersed between matching components).
- gh-107431: Make the `DictProxy` and`ListProxy` types in`multiprocessing.managers` Generic Alias
Types for`[]` use in typing contexts.
- gh-72904: Add `glob.translate()` . This function converts a pathname
with shell-style wildcards to a regular expression.
- gh-90026: Define `USE_XATTRS` on Cygwin so that XATTR-related functions
in the`os` module become available.
- gh-90890: New methods `mailbox.Maildir.get_info()` ,`mailbox.Maildir.set_info()` ,`mailbox.Maildir.get_flags()` ,`mailbox.Maildir.set_flags()` ,`mailbox.Maildir.add_flag()` ,`mailbox.Maildir.remove_flag()` . These methods speed up accessing a
messageâs info and/or flags and are useful when it is not necessary to
access the messageâs contents, as when iterating over a Maildir to find
messages with specific flags.
- gh-102956: Fix returning of empty byte strings after seek in zipfile module
- gh-102895: Added a parameter `local_exit` for`code.interact()` to
prevent`exit()` and`quit` from closing`sys.stdin` and raise`SystemExit` .
- gh-97928: Change the behavior of `tkinter.Text.count()` . It now always
returns an integer if one or less counting options are specified.
Previously it could return a single count as a 1-tuple, an integer (only
if option`"update"` was specified) or`None` if no items found. The
result is now the same if`wantobjects` is set to`0` .
- gh-96954: Switch the storage of the unicode codepoint names to use a different data-structure, a directed acyclic word graph. This makes the unicodedata shared library about 440 KiB smaller. Contributed by Carl Friedrich Bolz-Tereick using code from the PyPy project.
- gh-73561: Omit the interface scope from an IPv6 address when used as Host header by `http.client` .
- gh-86826: `zipinfo` now supports the full range of values in the TZ
string determined by RFC 8536 and detects all invalid formats. Both Python
and C implementations now raise exceptions of the same type on invalid
data.

### TestsÂ¶

- gh-111808: Make the default value of `test.support.infinite_recursion()` to be conditional based on whether optimizations were used when compiling
the interpreter. This helps with platforms like WASI whose stack size is
greatly restricted in debug builds.
- gh-110722: Gathering line coverage of standard libraries within the regression test suite is now precise, as well as much faster. Patch by Åukasz Langa.
- gh-110367: Make regrtest `--verbose3` option compatible with`--huntrleaks -jN` options. The```
./python -m test -j1 -R 3:3
--verbose3
```
command now works as expected. Patch by Victor Stinner.
- gh-111165: Remove no longer used functions `run_unittest()` and`run_doctest()` from the`test.support` module.
- gh-110932: Fix regrtest if the `SOURCE_DATE_EPOCH` environment variable
is defined: use the variable value as the random seed. Patch by Victor
Stinner.
- gh-110995: test_gdb: Fix detection of gdb built without Python scripting support. Patch by Victor Stinner.
- gh-110918: Test case matching patterns specified by options `--match` ,`--ignore` ,`--matchfile` and`--ignorefile` are now tested in the
order of specification, and the last match determines whether the test
case be run or ignored.
- gh-108747: Add unit test for `usercustomize` and`sitecustomize` hooks
from`site` .

### BuildÂ¶

- gh-96954: Make `make regen-unicodedata` work for out-of-tree builds of
CPython.
- gh-112088: Add `Tools/build/regen-configure.sh` script to regenerate the`configure` with an Ubuntu container image. The`quay.io/tiran/cpython_autoconf:271` container image
(tiran/cpython_autoconf) is
no longer used. Patch by Victor Stinner.
- gh-111046: For wasi-threads, memory is now exported to fix compatibility issues with some wasm runtimes.
- gh-110828: AIX 32bit needs `-latomic` to build the`_testcapi` extension module.
- gh-85283: The `errno` ,`md5` ,`resource` ,`winsound` ,`_ctypes_test` ,`_multiprocessing.posixshmem` ,`_scproxy` ,`_stat` ,`_testimportmultiple` and`_uuid` C extensions are now
built with the limited C API. Patch by Victor
Stinner.

### WindowsÂ¶

- gh-111856: Fixes `fstat()` on file systems that do not support
file ID requests. This includes FAT32 and exFAT.
- gh-111293: Fix `os.DirEntry.inode` dropping higher 64 bits of a file
id on some filesystems on Windows.
- gh-110913: WindowsConsoleIO now correctly chunks large buffers without splitting up UTF-8 sequences.

### macOSÂ¶

- gh-59703: For macOS framework builds, in `getpath.c` use the system`dladdr` function to find the path to the shared library rather than
depending on deprecated macOS APIs.
- gh-110950: Update macOS installer to include an upstream Tcl/Tk fix for the `Secure coding is not enabled for restorable state!` warning
encountered in Tkinter on macOS 14 Sonoma.
- gh-111015: Ensure that IDLE.app and Python Launcher.app are installed with appropriate permissions on macOS builds.
- gh-71383: Update macOS installer to include an upstream Tcl/Tk fix for the `ttk::ThemeChanged` error encountered in Tkinter.
- gh-92603: Update macOS installer to include a fix accepted by upstream Tcl/Tk for a crash encountered after the first `tkinter.Tk()` instance
is destroyed.

### IDLEÂ¶

- bpo-35668: Add docstrings to the IDLE debugger module. Fix two bugs: initialize `Idb.botframe` (should be in Bdb); in`Idb.in_rpc_code` ,
check whether`prev_frame` is`None` before trying to use it. Greatly
expand test_debugger.

### Tools/DemosÂ¶

- gh-111903: Argument Clinic now supports the `@critical_section` directive that instructs Argument Clinic to generate a critical section
around the function call, which locks the`self` object in`--disable-gil` builds. Patch by Sam Gross.

### C APIÂ¶

- gh-112026: Add again the private `_PyThreadState_UncheckedGet()` function as an alias to the new public`PyThreadState_GetUnchecked()` function. Patch by Victor Stinner.
- gh-112026: Restore the removed `_PyDict_GetItemStringWithError()` function. It is used by numpy. Patch by Victor Stinner.
- gh-112026: Restore removed private C API functions, macros and structures which have no simple replacement for now: 
  - _PyDict_GetItem_KnownHash()
  - _PyDict_NewPresized()
  - _PyHASH_BITS
  - _PyHASH_IMAG
  - _PyHASH_INF
  - _PyHASH_MODULUS
  - _PyHASH_MULTIPLIER
  - _PyLong_Copy()
  - _PyLong_FromDigits()
  - _PyLong_New()
  - _PyLong_Sign()
  - _PyObject_CallMethodId()
  - _PyObject_CallMethodNoArgs()
  - _PyObject_CallMethodOneArg()
  - _PyObject_CallOneArg()
  - _PyObject_EXTRA_INIT
  - _PyObject_FastCallDict()
  - _PyObject_GetAttrId()
  - _PyObject_Vectorcall()
  - _PyObject_VectorcallMethod()
  - _PyStack_AsDict()
  - _PyThread_CurrentFrames()
  - _PyUnicodeWriter structure
  - _PyUnicodeWriter_Dealloc()
  - _PyUnicodeWriter_Finish()
  - _PyUnicodeWriter_Init()
  - _PyUnicodeWriter_Prepare()
  - _PyUnicodeWriter_PrepareKind()
  - _PyUnicodeWriter_WriteASCIIString()
  - _PyUnicodeWriter_WriteChar()
  - _PyUnicodeWriter_WriteLatin1String()
  - _PyUnicodeWriter_WriteStr()
  - _PyUnicodeWriter_WriteSubstring()
  - _PyUnicode_AsString()
  - _PyUnicode_FromId()
  - _PyVectorcall_Function()
  - _Py_IDENTIFIER()
  - _Py_c_abs()
  - _Py_c_diff()
  - _Py_c_neg()
  - _Py_c_pow()
  - _Py_c_prod()
  - _Py_c_quot()
  - _Py_c_sum()
  - _Py_static_string()
  - _Py_static_string_init()
 Patch by Victor Stinner.
- gh-112026: Add again `<ctype.h>` and`<unistd.h>` includes in`Python.h` , but donât include them in the limited C API version 3.13 and
newer. Patch by Victor Stinner.
- gh-111956: Add internal-only one-time initialization API: `_PyOnceFlag` and`_PyOnceFlag_CallOnce` .
- gh-111262: Add `PyDict_Pop()` and`PyDict_PopString()` functions: remove a key from a dictionary and optionally return the
removed value. This is similar to`dict.pop()` , but without the
default value and not raising`KeyError` if the key missing. Patch by
Stefan Behnel and Victor Stinner.
- gh-111863: Rename `Py_NOGIL` to`Py_GIL_DISABLED` . Patch by Hugo van
Kemenade.
- gh-111138: Add `PyList_Extend()` and`PyList_Clear()` functions: similar to Python`list.extend()` and`list.clear()` methods. Patch by Victor Stinner.
- gh-108765: On Windows, `Python.h` no longer includes the`<stddef.h>` standard header file. If needed, it should now be included explicitly.
Patch by Victor Stinner.
- gh-111569: Implement âPython Critical Sectionsâ from **PEP 703** . These are
macros to help replace the GIL with per-object locks in the`--disable-gil` build of CPython. The macros are no-ops in the default
build.
- gh-111506: In the limited C API version 3.13, `Py_SET_REFCNT()` function is now implemented as an opaque function call. Patch by Victor
Stinner.
- gh-108082: Add `PyErr_FormatUnraisable()` function.
- gh-110964: Move the undocumented private _PyArg functions and _PyArg_Parser structure to internal C API ( `pycore_modsupport.h` ). Patch
by Victor Stinner.
- gh-110815: Support non-ASCII keyword names in `PyArg_ParseTupleAndKeywords()` .
- gh-109587: Introduced `PyUnstable_PerfTrampoline_CompileCode()` ,`PyUnstable_PerfTrampoline_SetPersistAfterFork()` and`PyUnstable_CopyPerfMapFile()` . These functions allow extension
modules to initialize trampolines eagerly, after the application is
âwarmed upâ. This makes it possible to have perf-trampolines running in an
always-enabled fashion.
- gh-85283: Add the `PySys_Audit()` function to the limited C API.
Patch by Victor Stinner.
- gh-85283: Add `PyMem_RawMalloc()` ,`PyMem_RawCalloc()` ,`PyMem_RawRealloc()` and`PyMem_RawFree()` to the limited C
API. Patch by Victor Stinner.
- gh-106672: Functions `PyDict_GetItem()` ,`PyDict_GetItemString()` ,`PyMapping_HasKey()` ,`PyMapping_HasKeyString()` ,`PyObject_HasAttr()` ,`PyObject_HasAttrString()` , and`PySys_GetObject()` , which
clear all errors occurred during calling the function, report now them
using`sys.unraisablehook()` .
- gh-67565: Remove redundant C-contiguity check in `getargs.c` ,`binascii` ,`ssl` and Argument Clinic. Patched by Stefan Krah
and Furkan Onder

## Python 3.13.0 alpha 1Â¶

*Release date: 2023-10-13*

### SecurityÂ¶

- gh-108310: Fixed an issue where instances of `ssl.SSLSocket` were
vulnerable to a bypass of the TLS handshake and included protections (like
certificate verification) and treating sent unencrypted data as if it were
post-handshake TLS encrypted data.  Security issue reported as**CVE 2023-40217** by Aapo Oksman. Patch by Gregory P. Smith.
- gh-107774: PEP 669 specifies that `sys.monitoring.register_callback` will generate an audit event. Pre-releases of Python 3.12 did not generate
the audit event. This is now fixed.
- gh-102988: Reverted the `email.utils` security improvement change
released in 3.12beta4 that unintentionally caused`email.utils.getaddresses` to fail to parse email addresses with a
comma in the quoted name field. See gh-106669.
- gh-99108: Refresh our new HACL* built-in `hashlib` code from
upstream. Built-in SHA2 should be faster and an issue with SHA3 on 32-bit
platforms is fixed.
- gh-102509: Start initializing `ob_digit` during creation of`PyLongObject` objects. Patch by Illia Volochii.

### Core and BuiltinsÂ¶

- gh-110782: Fix crash when `typing.TypeVar` is constructed with a
keyword argument. Patch by Jelle Zijlstra.
- gh-110752: Reset `ceval.eval_breaker` in`interpreter_clear()`
- gh-110721: Use the `traceback` implementation for the default`PyErr_Display()` functionality. Patch by Pablo Galindo
- gh-110696: Fix incorrect error message for invalid argument unpacking. Patch by Pablo Galindo
- gh-104169: Split the tokenizer into two separate directories: - One part includes the actual lexeme producing logic and lives in `Parser/lexer` .
- The second part wraps the lexer according to the different tokenization
modes   we have (string, utf-8, file, interactive, readline) and lives in`Parser/tokenizer` .
- gh-110688: Remove undocumented `test_c_api` method from`set` ,
which was only defined for testing purposes under`Py_DEBUG` . Now we
have proper CAPI tests.
- gh-104584: Fix a reference leak when running with `PYTHONUOPS` or`-X uops` enabled.
- gh-110514: Add `PY_THROW` to`sys.setprofile()` events
- gh-110489: Optimise `math.ceil()` when the input is exactly a float,
resulting in about a 10% improvement.
- gh-110455: Guard `assert(tstate->thread_id > 0)` with```
#ifndef
HAVE_PTHREAD_STUBS
```
. This allows for pydebug builds to work under WASI
which (currently) lacks thread support.
- gh-110309: Remove unnecessary empty constant nodes in the ast of f-string specs.
- gh-110259: Correctly identify the format spec in f-strings (with single or triple quotes) that have multiple lines in the expression part and include a formatting spec. Patch by Pablo Galindo
- gh-110237: Fix missing error checks for calls to `PyList_Append` in`_PyEval_MatchClass` .
- gh-110164: regrtest: If the `SOURCE_DATE_EPOCH` environment variable is
defined, regrtest now disables tests randomization. Patch by Victor
Stinner.
- gh-109889: Fix the compilerâs redundant NOP detection algorithm to skip over NOPs with no line number when looking for the next instructionâs lineno.
- gh-109853: `sys.path[0]` is now set correctly for subinterpreters.
- gh-109923: Set line number on the `POP_TOP` that follows a`RETURN_GENERATOR` .
- gh-105716: Subinterpreters now correctly handle the case where they have threads running in the background. Before, such threads would interfere with cleaning up and destroying them, as well as prevent running another script.
- gh-109369: The internal eval_breaker and supporting flags, plus the monitoring version have been merged into a single atomic integer to speed up checks.
- gh-109823: Fix bug where compiler does not adjust labels when removing an empty basic block which is a jump target.
- gh-109793: The main thread no longer exits prematurely when a subinterpreter is cleaned up during runtime finalization. The bug was a problem particularly because, when triggered, the Python process would always return with a 0 exitcode, even if it failed.
- gh-109719: Fix missing jump target labels when compiler reorders cold/warm blocks.
- gh-109595: Add `-X cpu_count` command line option to
override return results of`os.cpu_count()` and`os.process_cpu_count()` . This option is useful for users who need to
limit CPU resources of a container system without having to modify the
container (application code). Patch by Donghee Na.
- gh-109627: Fix bug where the compiler does not assign a new jump target label to a duplicated small exit block.
- gh-109596: Fix some tokens in the grammar that were incorrectly marked as soft keywords. Also fix some repeated rule names and ensure that repeated rules are not allowed. Patch by Pablo Galindo
- gh-109496: On a Python built in debug mode, `Py_DECREF()` now
calls`_Py_NegativeRefcount()` if the object is a dangling pointer to
deallocated memory: memory filled with`0xDD` âdead byteâ by the debug
hook on memory allocators. The fix is to check the reference count*before* checking for`_Py_IsImmortal()` . Patch by Victor Stinner.
- gh-107265: Deopt opcodes hidden by the executor when base opcode is needed
- gh-109371: Deopted instructions correctly for tool initialization and modified the incorrect assertion in instrumentation, when a previous tool already sets INSTRUCTION events
- gh-105658: Fix bug where the line trace of an except block ending with a conditional includes an excess event with the line of the conditional expression.
- gh-109219: Fix compiling type param scopes that use a name which is also free in an inner scope.
- gh-109351: Fix crash when compiling an invalid AST involving a named (walrus) expression.
- gh-109341: Fix crash when compiling an invalid AST involving a `ast.TypeAlias` .
- gh-109195: Fix source location for the `LOAD_*` instruction preceding a`LOAD_SUPER_ATTR` to load the`super` global (or shadowing variable)
so that it encompasses only the name`super` and not the following
parentheses.
- gh-109256: Opcode IDs for specialized opcodes are allocated in their own range to improve stability of the IDs for the ârealâ opcodes.
- gh-109216: Fix possible memory leak in `BUILD_MAP` .
- gh-109207: Fix a SystemError in `__repr__` of symtable entry object.
- gh-109179: Fix bug where the C traceback display drops notes from `SyntaxError` .
- gh-109118: Disallow nested scopes (lambdas, generator expressions, and comprehensions) within PEP 695 annotation scopes that are nested within classes.
- gh-109156: Add tests for de-instrumenting instructions while keeping the instrumentation for lines
- gh-109114: Relax the detection of the error message for invalid lambdas inside f-strings to not search for arbitrary replacement fields to avoid false positives. Patch by Pablo Galindo
- gh-105848: Add a new `CALL_KW` opcode, used for calls containing
keyword arguments. Also, fix a possible crash when jumping over method
calls in a debugger.
- gh-109052: Use the base opcode when comparing code objects to avoid interference from instrumentation
- gh-109118: Fix interpreter crash when a NameError is raised inside the type parameters of a generic class.
- gh-88943: Improve syntax error for non-ASCII character that follows a numerical literal. It now points on the invalid non-ASCII character, not on the valid numerical literal.
- gh-108976: Fix crash that occurs after de-instrumenting a code object in a monitoring callback.
- gh-108732: Make iteration variables of module- and class-scoped comprehensions visible to pdb and other tools that use `frame.f_locals` again.
- gh-108959: Fix caret placement for error locations for subscript and binary operations that involve non-semantic parentheses and spaces. Patch by Pablo Galindo
- gh-104584: Fix a crash when running with `PYTHONUOPS` or`-X uops` enabled and an error occurs during optimization.
- gh-108727: Define `tp_dealloc` for`CounterOptimizer_Type` . This fixes
a segfault on deallocation.
- gh-108520: Fix `multiprocessing.synchronize.SemLock.__setstate__()` to
properly initialize`multiprocessing.synchronize.SemLock._is_fork_ctx` . This fixes a
regression when passing a SemLock across nested processes.Rename `multiprocessing.synchronize.SemLock.is_fork_ctx` to`multiprocessing.synchronize.SemLock._is_fork_ctx` to avoid exposing
it as public API.
- gh-108654: Restore locals shadowed by an inlined comprehension if the comprehension raises an exception.
- gh-108488: Change the initialization of inline cache entries so that the cache entry for `JUMP_BACKWARD` is initialized to zero, instead of the`adaptive_counter_warmup()` value used for all other instructions. This
counter, unique among instructions, counts up from zero.
- gh-108716: Turn off deep-freezing of code objects. Modules are still frozen, so that a file system search is not needed for common modules.
- gh-108614: Add RESUME_CHECK instruction, to avoid having to handle instrumentation, signals, and contexts switches in the tier 2 execution engine.
- gh-108487: Move an assert that would cause a spurious crash in a devious case that should only trigger deoptimization.
- gh-106176: Use a `WeakValueDictionary` to track the lists containing the
modules each thread is currently importing. This helps avoid a reference
leak from keeping the list around longer than necessary. Weakrefs are used
as GC canât interrupt the cleanup.
- gh-105481: The regen-opcode build stage was removed and its work is now done in regen-cases.
- gh-107901: Fix missing line number on `JUMP_BACKWARD` at the end
of a for loop.
- gh-108113: The `compile()` built-in can now accept a new flag,`ast.PyCF_OPTIMIZED_AST` , which is similar to`ast.PyCF_ONLY_AST` except that the returned`AST` is optimized according to the value of
the`optimize` argument.`ast.parse()` now accepts an optional argument`optimize` which is
passed on to the`compile()` built-in. This makes it possible to
obtain an optimized`AST` .
- gh-107971: Opcode IDs are generated from bytecodes.c instead of being hard coded in opcode.py.
- gh-107944: Improve error message for function calls with bad keyword arguments. Patch by Pablo Galindo
- gh-108390: Raise an exception when setting a non-local event ( `RAISE` ,`EXCEPTION_HANDLED` , etc.) in`sys.monitoring.set_local_events` .Fixes crash when tracing in recursive calls to Python classes.
- gh-108035: Remove the `_PyCFrame` struct, moving the pointer to the
current interpreter frame back to the threadstate, as it was for 3.10 and
earlier. The`_PyCFrame` existed as a performance optimization for
tracing. Since PEP 669 has been implemented, this optimization no longer
applies.
- gh-91051: Fix abort / segfault when using all eight type watcher slots, on platforms where `char` is signed by default.
- gh-106581: Fix possible assertion failures and missing instrumentation events when `PYTHONUOPS` or`-X uops` is enabled.
- gh-107526: Revert converting `vars` ,`dir` ,`next` ,`getattr` , and`iter` to argument clinic.
- gh-84805: Autogenerate signature for `METH_NOARGS` and`METH_O` extension functions.
- gh-107758: Make the `dump_stack()` routine used by the`lltrace` feature (low-level interpreter debugging) robust against recursion by
ensuring that it never calls a`__repr__` method implemented in Python.
Also make the similar output for Tier-2 uops appear on`stdout` (instead
of`stderr` ), to match the`lltrace` code in ceval.c.
- gh-107659: Add docstrings for `ctypes.pointer()` and`ctypes.POINTER()` .
- gh-105848: Modify the bytecode so that the actual callable for a `CALL` is at a consistent position on the stack (regardless of
whether or not bound-method-calling optimizations are active).
- gh-107674: Fixed performance regression in `sys.settrace` .
- gh-107724: In pre-release versions of 3.12, up to rc1, the sys.monitoring callback function for the `PY_THROW` event was missing the third,
exception argument. That is now fixed.
- gh-84436: Skip reference count modifications for many known immortal objects.
- gh-107596: Specialize subscripting `str` objects by`int` indexes.
- gh-107080: Trace refs builds ( `--with-trace-refs` ) were crashing when
used with isolated subinterpreters.  The problematic global state has been
isolated to each interpreter.  Other fixing the crashes, this change does
not affect users.
- gh-107557: Generate the cases needed for the barebones tier 2 abstract interpreter for optimization passes in CPython.
- gh-106608: Make `_PyUOpExecutorObject` variable length.
- gh-100964: Clear generatorsâ exception state after `return` to break
reference cycles.
- gh-107455: Improve error messages when converting an incompatible type to `ctypes.c_char_p` ,`ctypes.c_wchar_p` and`ctypes.c_void_p` .
- gh-107263: Increase C recursion limit for functions other than the main interpreter from 800 to 1500. This should allow functions like `list.__repr__` and`json.dumps` to handle all the inputs that they
could prior to 3.12
- gh-104584: Fix an issue which caused incorrect inline caches to be read when running with `PYTHONUOPS` or`-X uops` enabled.
- gh-104432: Fix potential unaligned memory access on C APIs involving returned sequences of `char *` pointers within the`grp` and`socket` modules. These were revealed using a`-fsaniziter=alignment` build on ARM macOS. Patch by Christopher Chavez.
- gh-106078: Isolate `_decimal` (apply**PEP 687** ). Patch by Charlie
Zhao.
- gh-106898: Add the exception as the third argument to `PY_UNIND` callbacks in`sys.monitoring` . This makes the`PY_UNWIND` callback
consistent with the other exception handling callbacks.
- gh-106895: Raise a `ValueError` when a monitoring callback function
returns`DISABLE` for events that cannot be disabled locally.
- gh-106897: Add a `RERAISE` event to`sys.monitoring` , which occurs
when an exception is reraise, either explicitly by a plain`raise` statement, or implicitly in an`except` or`finally` block.
- gh-77377: Ensure that multiprocessing synchronization objects created in a fork context are not sent to a different process created in a spawn context. This changes a segfault into an actionable RuntimeError in the parent process.
- gh-106931: Statically allocated string objects are now interned globally instead of per-interpreter. This fixes a situation where such a string would only be interned in a single interpreter. Normal string objects are unaffected.
- gh-104621: Unsupported modules now always fail to be imported.
- gh-107122: Add `dbm.ndbm.ndbm.clear()` to`dbm.ndbm` . Patch By
Donghee Na.
- gh-107122: Add `dbm.gnu.gdbm.clear()` to`dbm.gnu` . Patch By
Donghee Na.
- gh-107015: The ASYNC and AWAIT tokens are removed from the Grammar, which removes the possibility of making `async` and`await` soft keywords
when using`feature_version<7` in`ast.parse()` .
- gh-106917: Fix classmethod-style `super()` method calls (i.e., where
the second argument to`super()` , or the implied second argument drawn
from`self/cls` in the case of zero-arg super, is a type) when the
target of the call is not a classmethod.
