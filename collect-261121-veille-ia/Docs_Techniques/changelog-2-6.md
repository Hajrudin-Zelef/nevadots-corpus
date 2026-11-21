---
id: collect-261121-veille-ia/docs-techniques/changelog-2-6
title: "Changelog"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["patch", "evaluate", "typing", "json", "python", "improve", "error", "module"]
source: https://docs.python.org/release/3.15.1/whatsnew/changelog.html
source_anchor: ""
source_lines: [7501, 9000]
section: "Docs_Techniques"
---
of silently omitting them.
- gh-122905: `zipfile.Path` objects now sanitize names from the
zipfile.
- gh-122873: Enable `json` module to work as a script using the`-m` switch:`python -m json` . See the JSON command-line
interface documentation. Patch by Trey Hunner.
- gh-122858: Deprecate `asyncio.iscoroutinefunction()` in favor of`inspect.iscoroutinefunction()` .
- gh-116263: `logging.handlers.RotatingFileHandler` no longer rolls
over empty log files.
- gh-105376: Restore the deprecated `logging``warn()` method. It was
removed in Python 3.13 alpha 1. Keep the deprecated`warn()` method in
Python 3.13. Patch by Victor Stinner.
- gh-122311: Improve errors in the `pickle` module.`PicklingError` is now raised more often instead of`UnicodeEncodeError` ,`ValueError` and`AttributeError` ,
and the original exception is chained to it. Improve and unify error
messages in Python and C implementations.
- gh-122744: Bump the version of pip bundled in ensurepip to version 24.2.
- gh-118761: Improve import time of `pprint` by around seven times.
Patch by Hugo van Kemenade.
- gh-118974: Add `decorator` parameter to`dataclasses.make_dataclass()` to customize the functional creation of
dataclasses.
- gh-118814: Fix the `typing.TypeVar` constructor when name is passed
by keyword.
- gh-122637: Adjust `cmath.tanh(nanj)` and`cmath.tanh(infj)` for recent
C standards.
- gh-122478: Remove internal frames from tracebacks shown in `code.InteractiveInterpreter` with non-default`sys.excepthook()` . Save correct tracebacks in`sys.last_traceback` and update`__traceback__` attribute of`sys.last_value` and`sys.last_exc` .
- gh-116622: On Android, the `FICLONE` and`FICLONERANGE` constants are
no longer exposed by`fcntl` , as these ioctls are blocked by SELinux.
- gh-82378: Make sure that the new REPL interprets `sys.tracebacklimit` in the same way that the classic REPL did.
- gh-122334: Fix crash when importing `ssl` after the main interpreter
restarts.
- gh-122459: Optimize `pickling` by name objects without the`__module__` attribute.
- gh-87320: In `code.InteractiveInterpreter` , handle exceptions
caused by calling a non-default`sys.excepthook()` . Before, the
exception bubbled up to the caller, ending the REPL.
- gh-122272: On some platforms such as Linux, year with century was not 0-padded when formatted by `strftime()` with C99-specific
specifiers`'%C'` or`'%F'` . The 0-padding behavior is now guaranteed
when the format specifiers`'%C'` and`'%F'` are supported by the C
library. Patch by Ben Hsing
- gh-122400: Handle `ValueError` s raised by`os.stat()` in`filecmp.dircmp` and`filecmp.cmpfiles()` . Patch by BÃ©nÃ©dikt
Tran.
- gh-122311: Fix some error messages in `pickle` .
- gh-121650: `email` headers with embedded newlines are now quoted on
output. The`generator` will now refuse to serialize (write)
headers that are unsafely folded or delimited; see`verify_generated_headers` . (Contributed by Bas
Bloemsaat and Petr Viktorin in gh-121650.)
- gh-122332: Fixed segfault with `asyncio.Task.get_coro()` when using an
eager task factory.
- gh-105733: `ctypes.ARRAY()` is now soft deprecated: it no
longer emits deprecation warnings and is not scheduled for removal.
- gh-122213: Add notes for pickle serialization errors that allow to identify the source of the error.
- gh-119180: As part of **PEP 749** , add the following attributes for
customizing evaluation of annotation scopes:
  - `evaluate_value` on`typing.TypeAliasType`
  - `evaluate_bound` ,`evaluate_constraints` , and`evaluate_default` on`typing.TypeVar`
  - `evaluate_default` on`typing.ParamSpec`
  - `evaluate_default` on`typing.TypeVarTuple`
- gh-119180: Fix handling of classes with custom metaclasses in `annotationlib.get_annotations` .
- gh-122170: Handle `ValueError` s raised by`os.stat()` in`linecache` . Patch by BÃ©nÃ©dikt Tran.
- gh-122163: Add notes for JSON serialization errors that allow to identify the source of the error.
- gh-122129: Improve support of method descriptors and wrappers in the help title.
- gh-122145: Fix an issue when reporting tracebacks corresponding to Python code emitting an empty AST body. Patch by Nikita Sobolev and BÃ©nÃ©dikt Tran.
- gh-121723: Make `logging.config.dictConfig()` accept any object
implementing the Queue public API. See the queue configuration section for details. Patch by BÃ©nÃ©dikt Tran.
- gh-82951: Serializing objects with complex `__qualname__` (such as
unbound methods and nested classes) by name no longer involves serializing
parent objects by value in pickle protocols < 4.
- gh-120754: `Pathlib.read_bytes` no longer opens the file in Pythonâs
buffered I/O mode. This reduces overheads as the code reads a file in
whole leading to a modest speedup.
- gh-113785: `csv` now correctly parses numeric fields (when used with`csv.QUOTE_NONNUMERIC` or`csv.QUOTE_STRINGS` ) which start
with an escape character.
- gh-122088: `@warnings.deprecated` now copies the coroutine status of
functions and methods so that`inspect.iscoroutinefunction()` returns
the correct result.
- gh-122081: Fix a crash in the `decimal.IEEEContext()` optional
function available via the`EXTRA_FUNCTIONALITY` configuration flag.
- gh-73991: Add `pathlib.Path.move()` , which moves a file or directory
tree.
- gh-121268: Remove workarounds for non-IEEE 754 systems in `cmath` .
- gh-119698: Due to the lack of interest for `symtable.Class.get_methods()` , the method is marked as deprecated and
will be removed in Python 3.16. Patch by BÃ©nÃ©dikt Tran.
- gh-121889: Adjusts `cmath.acosh(complex('0+nanj'))` for recent C
standards.
- gh-121804: Correctly show error locations, when `SyntaxError` raised
in new repl. Patch by Sergey B Kirpichev.
- gh-121797: Add alternative `Fraction` constructor`Fraction.from_number()` .
- gh-121798: Add alternative `Decimal` constructor`Decimal.from_number()` .
- gh-120930: Fixed a bug introduced by gh-92081 that added an incorrect extra blank to encoded words occurring in wrapped headers.
- gh-57141: The *shallow* argument to`filecmp.dircmp` (new in Python
3.13) is now keyword-only.
- gh-121245: Simplify handling of the history file in `site.register_readline()` helper. The`CAN_USE_PYREPL` variable now
will be initialized, when imported.  Patch by Sergey B Kirpichev.
- gh-121249: Support the float complex and double complex C types in the `struct` module if the compiler has C11
complex arithmetic.  Patch by Sergey B Kirpichev.
- gh-121486: `math` functions`isqrt()` ,`log()` ,`log2()` and`log10()` now support integers larger
than`2**2**32` on 32-bit platforms.
- gh-121474: Fix missing sanity check for `parties` arg in`threading.Barrier` constructor. Patch by Clinton Christian
(pygeek).
- gh-121450: Hard-coded breakpoints ( `breakpoint()` and`pdb.set_trace()` ) now reuse the most recent`Pdb` instance that
calls`Pdb.set_trace()` , instead of creating a new one each time. As a
result, all the instance specific data like`display` and`commands` are preserved across Hard-coded breakpoints.
- gh-119169: Slightly speed up `os.walk()` by simplifying exception
handling.
- gh-121423: Improve import time of `socket` by lazy importing modules
and writing`socket.errorTab` as a constant.
- gh-59110: `zipimport` supports now namespace packages when no
directory entry exists.
- gh-119004: Fix a crash in OrderedDict.__eq__ when operands are mutated during the check. Patch by BÃ©nÃ©dikt Tran.
- gh-121313: Limit the reading size in the `multiprocessing.connection.Connection` class to 64 KiB to prevent
memory overallocation and unnecessary memory management system calls.
- gh-121332: Fix constructor of `ast` nodes with custom`_attributes` . Previously, passing custom attributes would raise a`DeprecationWarning` . Passing arguments to the constructor that
are not in`_fields` or`_attributes` remains deprecated. Patch by
Jelle Zijlstra.
- gh-121245: Fix a bug in the handling of the command history of the new REPL that caused the history file to be wiped at REPL exit.
- gh-121210: Handle AST nodes with missing runtime fields or attributes in `ast.compare()` . Patch by BÃ©nÃ©dikt Tran.
- gh-121163: Add support for `all` as an valid`action` for`warnings.simplefilter()` and`warnings.filterwarnings()` .
- gh-121151: Fix wrapping of long usage text of arguments inside a mutually exclusive group in `argparse` .
- gh-121141: Add support for `copy.replace()` to AST nodes. Patch by
BÃ©nÃ©dikt Tran.
- gh-87744: Fix waitpid race while calling `send_signal()` in asyncio. Patch by Kumar
Aditya.
- gh-121027: Add a future warning in `functools.partial.__get__()` . In
future Python versions`functools.partial` will be a method
descriptor.
- gh-121027: Make the `functools.partial` object a method descriptor.
- gh-117784: CPython now detects whether its linked TLS library supports TLSv1.3 post-handshake authentication and disables that feature if support is lacking.
- gh-121025: Improve the `__repr__()` of`functools.partialmethod` . Patch by BÃ©nÃ©dikt Tran.
- gh-121018: Fixed issues where `argparse.ArgumentParser.parse_args()` did not honor`exit_on_error=False` . Based on patch by Ben Hsing.
- gh-119614: Fix truncation of strings with embedded null characters in some internal operations in `tkinter` .
- gh-120910: When reading installed files from an egg, use `relative_to(walk_up=True)` to honor files installed outside of the
installation root.
- gh-61103: Support float complex, double complex and long double complex C types in `ctypes` as`c_float_complex` ,`c_double_complex` and`c_longdouble_complex` if the compiler has C11 complex
arithmetic. Patch by Sergey B Kirpichev.
- gh-120888: Upgrade pip wheel bundled with ensurepip (pip 24.1.1)
- gh-101830: Accessing the `tkinter` objectâs string representation no
longer converts the underlying Tcl object to a string on Windows.
- gh-120678: Fix regression in the new REPL that meant that globals from files passed using the `-i` argument would not be included in the REPLâs
global namespace. Patch by Alex Waygood.
- gh-120811: Fix possible memory leak in `contextvars.Context.run()` .
- gh-120782: Fix wrong references of the `datetime` types after
reloading the module.
- gh-120713: `datetime.datetime.strftime()` now 0-pads years with less
than four digits for the format specifiers`%Y` and`%G` on Linux.
Patch by Ben Hsing
- gh-120769: Make empty line in `pdb` repeats the last command even
when the command is from`cmdqueue` .
- gh-120780: Show string value of LOAD_SPECIAL oparg in `dis` output.
- gh-41431: Add `datetime.time.strptime()` and`datetime.date.strptime()` . Contributed by Wannes Boeykens.
- gh-120743: Soft deprecate `os.popen()` and`os.spawn*` functions. They should no longer be used to
write new code. The`subprocess` module is recommended instead. Patch
by Victor Stinner.
- gh-120732: Fix `name` passing to`unittest.mock.Mock` object when
using`unittest.mock.create_autospec()` .
- gh-111259: `re` now handles patterns like`"[\s\S]"` or`"\s|\S"` which match any character as effectively as a dot with the`DOTALL` modifier (`"(?s:.)"` ).
- gh-120683: Fix an error in `logging.LogRecord` , when the integer
part of the timestamp is rounded up, while the millisecond calculation
truncates, causing the log timestamp to be wrong by up to 999 ms (affected
roughly 1 in 8 million timestamps).
- gh-118710: `ipaddress.IPv4Address` and`ipaddress.IPv6Address` attributes`version` and`max_prefixlen` are now available on the class.
- gh-120633: Move scrollbar and remove tear-off menus in turtledemo.
- gh-120606: Allow users to use EOF to exit `commands` definition in`pdb`
- gh-120284: Allow `asyncio.Runner.run()` to accept awaitable
objects instead of simply coroutines.
- gh-120541: Improve the prompt in the âlessâ pager when `help()` is
called with non-string argument.
- gh-120495: Fix incorrect exception handling in Tab Nanny. Patch by Wulian233.
- gh-120388: Improve a warning message when a test method in `unittest` returns something other than`None` . Now we show the returned object
type and optional asyncio-related tip.
- gh-120381: Correct `inspect.ismethoddescriptor()` to check also for
the lack of`__delete__()` .  Patch by Jan Kaliszewski.
- gh-90425: The OS byte in gzip headers is now always set to 255 when using `gzip.compress()` .
- gh-120343: Fix column offset reporting for tokens that come after multiline f-strings in the `tokenize` module.
- gh-119180: As part of implementing **PEP 649** and**PEP 749** , add a new
module`annotationlib` . Add support for unresolved forward references in
annotations to`dataclasses` ,`typing.TypedDict` , and`typing.NamedTuple` .
- gh-119600: Fix `unittest.mock.patch()` to not read attributes of the
target when`new_callable` is set. Patch by Robert Collins.
- gh-120289: Fixed the use-after-free issue in `cProfile` by
disallowing`disable()` and`clear()` in external timers.
- gh-82017: Added support for converting any objects that have the `as_integer_ratio()` method to a`Fraction` .
- gh-114053: Fix edge-case bug where `typing.get_type_hints()` would
produce incorrect results if type parameters in a class scope were
overridden by assignments in a class scope and```
from __future__ import
annotations
```
semantics were enabled. Patch by Alex Waygood.
- gh-114053: Fix erroneous `NameError` when calling`inspect.get_annotations()` with`` eval_str=True` `` on a class that
made use of**PEP 695** type parameters in a module that had```
from
__future__ import annotations
```
at the top of the file. Patch by Alex
Waygood.
- gh-120268: Prohibit passing `None` to pure-Python`datetime.date.fromtimestamp()` to achieve consistency with
C-extension implementation.
- gh-120244: Fix memory leak in `re.sub()` when the replacement string
contains backreferences.
- gh-120254: Added `commands` argument to`pdb.set_trace()` which
allows users to send debugger commands from the source file.
- gh-120211: Fix `tkinter.ttk` with Tcl/Tk 9.0.
- gh-71587: Fix crash in C version of `datetime.datetime.strptime()` when called again on the restarted interpreter.
- gh-117983: Defer the `threading` import in`importlib.util` until lazy
loading is used.
- gh-120157: Remove unused constant `concurrent.futures._base._FUTURE_STATES` in`concurrent.futures` .
Patch by Clinton Christian (pygeek).
- gh-120161: `datetime` no longer crashes in certain complex reference
cycle situations.
- gh-119698: Fix `symtable.Class.get_methods()` and document its
behaviour. Patch by BÃ©nÃ©dikt Tran.
- gh-120121: Add `concurrent.futures.InvalidStateError` to moduleâs`__all__` .
- gh-119933: Add the `symtable.SymbolTableType` enumeration to
represent the possible outputs of the`symtable.SymbolTable.get_type` method. Patch by BÃ©nÃ©dikt Tran.
- gh-120029: Expose `symtable.Symbol` methods`is_free_class()` ,`is_comp_iter()` and`is_comp_cell()` . Patch by BÃ©nÃ©dikt Tran.
- gh-120108: Fix calling `copy.deepcopy()` on`ast` trees that have
been modified to have references to parent nodes. Patch by Jelle Zijlstra.
- gh-120056: Add `socket.IP_RECVERR` and`socket.IP_RECVTTL` constants (both available since Linux 2.2). And`socket.IP_RECVORIGDSTADDR` constant (available since Linux
2.6.29).
- gh-112672: Support building `tkinter` with Tcl 9.0.
- gh-120057: Added the `os.environ.refresh()` method to
update`os.environ` with changes to the environment made by`os.putenv()` , by`os.unsetenv()` , or made outside Python in the
same process. Patch by Victor Stinner.
- gh-120029: Expose `symtable.Symbol.is_type_parameter()` in the`symtable` module. Patch by BÃ©nÃ©dikt Tran.
- gh-119819: Fix regression to allow logging configuration with multiprocessing queue types.
- gh-65454: `unittest.mock.Mock.attach_mock()` no longer triggers a call
to a`PropertyMock` being attached.
- gh-117142: The `ctypes` module may now be imported in all
subinterpreters, including those that have their own GIL.
- gh-118835: Fix _pyrepl crash when using custom prompt with ANSI escape codes.
- gh-81936: `help()` and`showtopic()` methods now respect a
configured*output* argument to`pydoc.Helper` and not use the
pager in such cases. Patch by Enrico TrÃ¶ger.
- gh-117398: The `_datetime` module (C implementation for`datetime` )
now supports being imported in multiple interpreters.
- gh-119824: Print stack entry in `pdb` when and only when user input
is needed.
- gh-119838: In mixed arithmetic operations with `Fraction` and complex, the fraction is now converted to`float` instead of`complex` .
- gh-119770: Make `termios``ioctl()` constants positive. Patch by
Victor Stinner.
- gh-89727: Fix issue with `shutil.rmtree()` where a`RecursionError` is raised on deep directory trees.
- gh-119577: The `DeprecationWarning` emitted when testing the truth
value of an`xml.etree.ElementTree.Element` now describes
unconditionally returning`True` in a future version rather than raising
an exception in Python 3.14.
- gh-89727: Partially fix issue with `shutil.rmtree()` where a`RecursionError` is raised on deep directory trees. A recursion error
is no longer raised when`rmtree.avoids_symlink_attacks` is false.
- gh-93963: Remove deprecated names from `importlib.abc` as found in`importlib.resources.abc` .
- gh-119118: Fix performance regression in the `tokenize` module by
caching the`line` token attribute and calculating the column offset
more efficiently.
- gh-89727: Fix issue with `os.fwalk()` where a`RecursionError` was raised on deep directory trees by adjusting the implementation to be
iterative instead of recursive.
- gh-119594: If one calls pow(fractions.Fraction, x, module) with modulo not None, the error message now says that the types are incompatible rather than saying pow only takes 2 arguments. Patch by Wim Jeantine-Glenn and Mark Dickinson.
- gh-119588: `zipfile.Path.is_symlink` now assesses if the given path is a
symlink.
- gh-119562: Remove `ast.Num` ,`ast.Str` ,`ast.Bytes` ,`ast.NameConstant` and`ast.Ellipsis` . They had all emitted deprecation warnings since
Python 3.12. Patch by Alex Waygood.
- gh-119555: Catch `SyntaxError` from`compile()` in the
runsource() method of the InteractiveColoredConsole.  Patch by Sergey B
Kirpichev.
- gh-118908: Limit exposed globals from internal imports and definitions on new REPL startup. Patch by Eugene Triguba and Pablo Galindo.
- gh-117865: Improve the import time of the `ast` module by deferring
the import of`re` . Patch by Jelle Zijlstra.
- gh-119127: Positional arguments of `functools.partial()` objects now
support placeholders via`functools.Placeholder` .
- gh-113892: Now, the method `sock_connect` of`asyncio.ProactorEventLoop` raises a`ValueError` if given
socket is not in non-blocking mode, as well as in other loop
implementations.
- gh-119506: Fix `io.TextIOWrapper.write()` method breaks internal
buffer when the method is called again during flushing internal buffer.
- gh-69214: Fix `fcntl.ioctl()`*request* parameter: use an```
unsigned
long
```
instead of an`unsigned int` for the*request* parameter of`fcntl.ioctl()` to support requests larger than`UINT_MAX` . Patch by
Victor Stinner.
- gh-119105: `difflib` âs`DIffer.compare()` (and so also`ndiff` ) can
no longer be provoked into cubic-time behavior, or into unbounded
recursion, and should generally be faster in ordinary cases too. Results
may change in some cases, although that should be rare. Correctness of
diffs is not affected. Some similar lines far apart may be reported as
deleting one and adding the other, where before they were displayed on
adjacent output lines with markup showing the intraline differences.
- gh-119443: The interactive REPL no longer runs with ```
from __future__
import annotations
```
enabled. Patch by Jelle Zijlstra.
- gh-119461: Add `socket.VMADDR_CID_LOCAL` constant. Patch by Victor
Stinner.
- gh-117398: Objects in the datetime C-API are now all statically allocated, which means better memory safety, especially when the module is reloaded. This should be transparent to users.
- gh-118894: `asyncio` REPL now has the same capabilities as PyREPL.
- gh-118830: Bump `pickle` default protocol to`5` .
- gh-118911: In PyREPL, updated `maybe-accept` âs logic so that if the user
hits`Enter` twice, they are able to terminate the block even if
thereâs trailing whitespace. Also, now when the user hits arrow up, the
cursor is on the last functional line. This matches IPythonâs behavior.
Patch by Aya Elsayed.
- gh-115225: Raise error on certain technically valid but pathological ISO 8601 strings passed to `datetime.time.fromisoformat()` that were
previously parsed incorrectly.
- gh-111201: Remove dependency to `readline` from the new Python REPL.
- gh-119189: When using the `**` operator or`pow()` with`Fraction` as the base and an exponent that is not
rational, a float, or a complex, the fraction is no longer converted to a
float.
- gh-119174: Fix high DPI causes turtledemo(turtle-graphics examples) windows blurry Patch by Wulian233 and Terry Jan Reedy
- gh-119121: Fix a NameError happening in `asyncio.staggered.staggered_race` . This function is now tested.
- gh-119105: `difflib.Differ` is much faster for some cases of diffs where
many pairs of lines are equally similar.
- gh-119113: Fix issue where `pathlib.PurePath.with_suffix()` didnât
raise`TypeError` when given`None` as a suffix.
- gh-118643: Fix an AttributeError in the `email` module when re-fold a
long address list. Also fix more cases of incorrect encoding of the
address separator in the address list.
- gh-73991: Add `pathlib.Path.copy()` , which copies a file or directory
to another.
- gh-58933: Make `pdb` return to caller frame correctly when`f_trace` of the caller frame is not set
- gh-82805: Support single-dot file extensions in `pathlib.PurePath.suffix` and related attributes and methods. For
example, the`suffixes` of`PurePath('foo.bar.')` are now`['.bar', '.']` rather than`[]` .
This brings file extension splitting in line with`os.path.splitext()` .
- gh-118924: Remove `version` and`version_info` from`sqlite3` . Patch by Hugo van Kemenade.
- gh-118928: Disallow using a sequence of parameters with named placeholders in `sqlite3` queries. Patch by Erlend E. Aasland.
- gh-118895: Setting attributes on `typing.NoDefault` now raises`AttributeError` instead of`TypeError` .
- gh-118868: Fixed issue where kwargs were no longer passed to the logging handler QueueHandler
- gh-118827: Remove deprecated `Quoter` class from`urllib.parse` . It had previously raised a`DeprecationWarning` since Python 3.11. Patch by Nikita Sobolev.
- gh-118824: Remove deprecated `pty.master_open()` and`pty.slave_open()` . Use`pty.openpty()` instead. Patch by Nikita
Sobolev.
- gh-118851: `ctx` arguments to the constructors of`ast` node
classes now default to`ast.Load()` . Patch by Jelle
Zijlstra.
- gh-101588: Remove copy, deepcopy, and pickle from itertools. These had previously raised a DeprecationWarning since Python 3.12.
- gh-118805: Remove *type* ,*choices* , and*metavar* parameters of`argparse.BooleanOptionalAction` . They were deprecated since
Python 3.12.
- gh-118803: `typing.ByteString` and`collections.abc.ByteString` are removed. They had previously
raised a`DeprecationWarning` since Python 3.12.
- gh-118798: The *isdst* parameter has been removed from`email.utils.localtime()` . Patch by Hugo van Kemenade.
- gh-118760: Restore the default value of `tkiter.wantobjects` to`1` .
- gh-118760: Fix errors in calling Tkinter bindings on Windows.
- gh-74033: Drop support for passing keyword arguments to `pathlib.Path` .
- gh-101357: Suppress all `OSError` exceptions from`pathlib.Path.exists()` and`is_*()` methods, rather than a
selection of more common errors. The new behaviour is consistent with`os.path.exists()` ,`os.path.isdir()` , etc. Use`pathlib.Path.stat()` to retrieve the file status without suppressing
exceptions.
- gh-78707: Drop support for passing additional positional arguments to `pathlib.PurePath.relative_to()` and`is_relative_to()` .
- gh-118507: Fix `os.path.isfile()` on Windows for pipes. Speedup`os.path.isjunction()` and`os.path.lexists()` on Windows with a
native implementation.
- gh-118772: Allow `typing.TypeVar` instances without a default to
follow instances without a default in some cases. Patch by Jelle Zijlstra.
- gh-118714: Allow `restart` in post-mortem debugging of`pdb` .
Removed restart message when the user quits pdb from post-mortem mode.
- gh-118673: Removed executable bits and shebang from stdlib modules.
- gh-110863: `os.path.realpath()` now suppresses any`OSError` from`os.readlink()` when*strict* mode is disabled (the default).
- gh-118263: Speed up `os.path.splitroot()` &`os.path.normpath()` with a direct C call.
- gh-110190: Fix ctypes structs with array on SPARC by setting `MAX_STRUCT_SIZE` to 32 in stgdict. Patch by Jakub Kulik
- gh-118033: Fix `dataclasses.dataclass()` not creating a`__weakref__` slot when subclassing`typing.Generic` .
- gh-117766: Always use `str()` to print`choices` in`argparse` .
- gh-106531: In `importlib.resources` , sync with importlib_resources
6.3.2,
including:`MultiplexedPath` now expects`Traversable` paths,
deprecating string arguments to`MultiplexedPath` ; Enabled support for
resources in namespace packages in zip files; Fixed`NotADirectoryError` when calling files on a subdirectory of a namespace package.
- gh-115937: Removed extra preprocessing for the `__signature__` attribute: the code just check if itâs a`inspect.Signature` instance.  Patch by Sergey B Kirpichev.
- gh-115808: Add `operator.is_none()` and`operator.is_not_none()` functions.
- gh-114264: Improve performance of `copy.deepcopy()` by adding a fast
path for atomic types.
- gh-113878: Add *doc* parameter to`dataclasses.field()` , so it can be
stored and shown as a documentation / metadata. If`@dataclass(slots=True)` is used, then the supplied string is available
in the`__slots__` dict. Otherwise, the supplied string is
only available in the corresponding`dataclasses.Field` object.In order to support this feature we are changing the `__slots__` format
in dataclasses from`tuple` to`dict` when documentation /
metadata is present.
- gh-113978: Ignore warnings on text completion inside REPL.
- gh-113008: Correct argparse usage output for required, mutually exclusive groups containing a positional argument
- gh-108172: `webbrowser` honors OS preferred browser on Linux when its
desktop entry name contains the text of a known browser name.
- gh-109109: You can now get the raw TLS certificate chains from TLS connections via `ssl.SSLSocket.get_verified_chain()` and`ssl.SSLSocket.get_unverified_chain()` methods.Contributed by Mateusz Nowak.
- gh-105623: Fix performance degradation in `logging.handlers.RotatingFileHandler` . Patch by Craig Robson.
- gh-102450: Add missing ISO-8601 24:00 alternative to midnight of next day to `datetime.datetime.fromisoformat()` and`datetime.time.fromisoformat()` . Patch by Izan âTizzySaurusâ Robinson
(tizzysaurus@gmail.com)
- gh-103956: Fix lack of newline characters in `trace` module output
when line tracing is enabled but source code line for current frame is not
available.
- gh-92081: Fix missing spaces in email headers when the spaces are mixed with encoded 8-bit characters.
- gh-103194: Prepare Tkinter for C API changes in Tcl 8.7/9.0 to avoid `_tkinter.Tcl_Obj` being unexpectedly returned instead of`bool` ,`str` ,`bytearray` , or`int` .
- gh-87106: Fixed handling in `inspect.Signature.bind()` of keyword
arguments having the same name as positional-only arguments when a
variadic keyword argument (e.g.`**kwargs` ) is present.
- gh-103134: Add additional methods to proxy objects in the `multiprocessing` module:
  - `clear()` and`copy()` for proxies of`list`
  - `fromkeys()` ,`reversed(d)` ,`d | {}` ,`{} | d` ,`d |= {'b': 2}` for proxies of`dict`
- bpo-45767: Fix integer conversion in `os.major()` ,`os.minor()` ,
and`os.makedev()` . Support device numbers larger than`2**63-1` .
Support non-existent device number (`NODEV` ).
- bpo-44864: Do not translate user-provided strings in `argparse.ArgumentParser` .
- bpo-41843: Solaris now uses `os.sendfile()` fast-copy syscall for more
efficient`shutil` file copy related functions.
- bpo-15987: Implemented `ast.compare()` for comparing two ASTs. Patch
by Batuhan Taskaya with some help from Jeremy Hylton.
- gh-67693: Fix `urllib.parse.urlunparse()` and`urllib.parse.urlunsplit()` for URIs with path starting with multiple
slashes and no authority. Based on patch by Ashwin Ramaswami.
- bpo-14074: Fix `argparse` metavar processing to allow positional
arguments to have a tuple metavar.

### IDLEÂ¶

- gh-122392: Increase currently inadequate vertical spacing for the IDLE browsers (path, module, and stack) on high-resolution monitors.
- gh-112938: Fix uninteruptable hang when Shell gets rapid continuous output.
- gh-122482: Change About IDLE to direct users to discuss.python.org instead of the now unused idle-dev email and mailing list.
- gh-78889: Stop Shell freezes by blocking user access to non-method sys.stdout.shell attributes, which are all private.
- gh-120083: Add explicit black IDLE Hovertip foreground color needed for recent macOS. Fixes Sonoma showing unreadable white on pale yellow. Patch by John Riggles.
- gh-120104: Fix padding in config and search dialog windows in IDLE.

### DocumentationÂ¶

- gh-124872: Added definitions for context, current context, and context management protocol, updated related definitions to be consistent, and expanded the documentation for `contextvars.Context` .
- gh-125018: The `importlib.metadata` documentation now includes
semantic cross-reference targets for the significant documented APIs. This
means intersphinx references like`importlib.metadata.version()` will
now work as expected.
- gh-124720: Update âUsing Python on a Macâ section of the âPython Setup and Usageâ document and include information on installing free-threading support.
- gh-124457: Remove coverity scan from the CPython repo. It has not been used since 2020 and is currently unmaintained.
- gh-116622: Add an Android platform guide, and flag modules not available on Android.
- gh-123976: Refresh docs around custom providers.
- gh-70870: Clarified the dual usage of the term âfree variableâ (both the formal meaning of any reference to names defined outside the local scope, and the narrower pragmatic meaning of nonlocal variables named in `co_freevars` ).
- gh-121277: Writers of CPythonâs documentation can now use `next` as the
version for the`versionchanged` ,`versionadded` ,`deprecated` directives.
- gh-117765: Improved documentation for `unittest.mock.patch.dict()`
- gh-121749: Fix documentation for `PyModule_AddObjectRef()` .
- gh-120012: Clarify the behaviours of `multiprocessing.Queue.empty()` and`multiprocessing.SimpleQueue.empty()` on closed queues. Patch by
BÃ©nÃ©dikt Tran.
- gh-119574: Added some missing environment variables to the output of `--help-env` .
- bpo-34008: The `Py_Main()` documentation moved from the âVery High
Level APIâ section to the âInitialization and Finalizationâ section.Also make it explicit that we expect `Py_Main` to typically be called
instead of`Py_Initialize` rather than after it (since`Py_Main` makes
its own call to`Py_Initialize` ). Document that calling both is
supported but is version dependent on which settings will be applied
correctly.

### Core and BuiltinsÂ¶

- gh-124375: Fix a crash in the free threading build when the GC runs concurrently with a new thread starting.
- gh-125221: Fix possible race condition when calling `__reduce_ex__()` for the first time in the free threading
build.
- gh-125174: Make the handling of reference counts of immortal objects more robust. Immortal objects with reference counts that deviate from their original reference count by up to a billion (half a billion on 32 bit builds) are still counted as immortal.
- gh-125039: Make `this_instr` and`prev_instr` const in cases
generator.

### LibraryÂ¶

- gh-125008: Fix `tokenize.untokenize()` producing invalid syntax for
double braces preceded by certain escape characters.

### Core and BuiltinsÂ¶

- gh-124871: Fix compiler bug (in some versions of 3.13) where an assertion fails during reachability analysis.
- gh-123378: Fix a crash in the `__str__()` method of`UnicodeError` objects when the`UnicodeError.start` and`UnicodeError.end` values are invalid or out-of-range. Patch by
BÃ©nÃ©dikt Tran.
- gh-118093: Improve the experimental JIT compilerâs ability to stay âon traceâ when encountering highly-biased branches.
- gh-124642: Fixed scalability issue in free-threaded builds for lock-free reads from dictionaries in multi-threaded scenarios
- gh-116510: Fix a crash caused by immortal interned strings being shared between sub-interpreters that use basic single-phase init. In that case, the string can be used by an interpreter that outlives the interpreter that created and interned it. For interpreters that share obmalloc state, also share the interned dict with the main interpreter.
- gh-116510: Fix a bug that can cause a crash when sub-interpreters use âbasicâ single-phase extension modules. Shared objects could refer to PyGC_Head nodes that had been freed as part of interpreter cleanup.
- gh-119180: The `__main__` module no longer always contains an`__annotations__` dictionary in its global namespace.
- gh-124547: When deallocating an object with inline values whose `__dict__` is still live: if memory allocation for the inline values
fails, clear the dictionary. Prevents an interpreter crash.
- gh-124513: Fix a crash in FrameLocalsProxy constructor: check the number of arguments. Patch by Victor Stinner.
- gh-124442: Fix nondeterminism in compilation by sorting the value of `__static_attributes__` . Patch by kp2pml30.
- gh-124285: Fix bug where `bool(a)` can be invoked more than once during
the evaluation of a compound boolean expression.
- gh-123856: Fix PyREPL failure when a keyboard interrupt is triggered after using a history search
- gh-65961: Deprecate the setting and using `__package__` and`__cached__` .
- gh-119726: The JIT now generates more efficient code for calls to C functions resulting in up to 0.8% memory savings and 1.5% speed improvement on AArch64. Patch by Diego Russo.
- gh-122878: Use the `pager` binary, if available (e.g. on Debian and
derivatives), to display REPL`help()` .
- gh-124188: Fix reading and decoding a line from the source file with non-UTF-8 encoding for syntax errors raised in the compiler.
- gh-124027: Support `<page up>` ,`<page down>` , and`<delete>` keys
in the Python REPL when`$TERM` is set to`vt100` .
- gh-124022: Fix bug where docstring is removed from classes in interactive mode.
- gh-123958: docstrings are now removed from the optimized AST in optimization level 2.
- gh-123923: The `f_executable` field in the internal`_PyInterpreterFrame` struct now uses a tagged pointer.
Profilers and debuggers that uses this field should clear the least
significant bit to recover the PyObject* pointer.
- gh-77894: Fix possible crash in the garbage collector when it tries to break a reference loop containing a `memoryview` object. Now a`memoryview` object can only be cleared if there are no buffers
that refer it.
- gh-120221: asyncio REPL is now again properly recognizing KeyboardInterrupts. Display of exceptions raised in secondary threads is fixed.
- gh-119310: Allow the new interactive shell to read history files written with the editline library that use unicode-escaped entries. Patch by aorcajo and Åukasz Langa.
- gh-123572: Fix key mappings for various F-keys in Windows for the new REPL. Patch by devdanzin

### LibraryÂ¶

- gh-123614: Add `turtle.save()` to easily save Turtle drawings as
PostScript files. Patch by Marie Roald and Yngve Mardal Moe.

### Core and BuiltinsÂ¶

- gh-123339: Setting the `__module__` attribute for a class now
removes the`__firstlineno__` item from the typeâs dict, so they will no
longer be inconsistent.
- gh-119034: Change `<page up>` and`<page down>` keys of the Python
REPL to history search forward/backward. Patch by Victor Stinner.
- gh-123562: Improve `SyntaxError` message for using```
case ... as
...
```
with not a name.
- gh-123545: Fix a double decref in rare cases on experimental JIT builds.
- gh-123484: Fix `_Py_DebugOffsets` for long objects to be relative to the
start of the object rather than the start of a subobject.
- gh-123446: Fix empty function name in `TypeError` when builtin magic
methods are used without the required args.
- gh-123440: Improve `SyntaxError` message for using`except as` with
not a name.
- gh-116017: Improved JIT memory consumption by periodically freeing memory used by infrequently-executed code. This change is especially likely to improve the memory footprint of long-running programs.
- gh-123344: Add AST optimizations for type parameter defaults.
- gh-123321: Prevent Parser/myreadline race condition from segfaulting on multi-threaded use. Patch by Bar Harel and Amit Wienner.
- gh-123177: Fix a bug causing stray prompts to appear in the middle of wrapped lines in the new REPL.
- gh-122982: Extend the deprecation period for bool inversion ( `~` ) by two
years.
- gh-123271: Make concurrent iterations over the same `zip()` iterator
safe under free-threading.
- gh-123275: Support `-X gil=1` and`PYTHON_GIL=1` on non-free-threaded builds.
- gh-123177: Deactivate line wrap in the Apple Terminal via a ANSI escape code. Patch by Pablo Galindo
- gh-123229: Fix valgrind warning by initializing the f-string buffers to 0 in the tokenizer. Patch by Pablo Galindo
- gh-122298: Restore printout of GC stats when `gc.set_debug(gc.DEBUG_STATS)` is called. This feature was accidentally
removed when implementing incremental GC.
- gh-121804: Correctly show error locations when a `SyntaxError` is
raised in the basic REPL. Patch by Sergey B Kirpichev.
- gh-115776: Enables inline values (Pythonâs equivalent of hidden classes) on any class whoâs instances are of a fixed size.
- gh-123142: Fix too-wide source location in exception tracebacks coming from broken iterables in comprehensions.
- gh-123048: Fix a bug where pattern matching code could emit a `JUMP_FORWARD` with no source location.
- gh-118093: Break up `CALL_ALLOC_AND_ENTER_INIT` into micro-ops and relax
requirement for exact args, in order to increase the amount of code
supported by tier 2.
- gh-123123: Fix displaying `SyntaxError` exceptions covering multiple
lines. Patch by Pablo Galindo
- gh-123083: Fix a potential use-after-free in `STORE_ATTR_WITH_HINT` .
- gh-123022: Fix crash in free-threaded build when calling `Py_Initialize()` from a non-main thread.
- gh-118093: Add three specializations for `CALL_KW` :
  - `CALL_KW_PY` for calls to Python functions
  - `CALL_KW_BOUND_METHOD` for calls to bound methods
  - `CALL_KW_NON_PY` for all other calls
- gh-122821: Make sure that branches in `while` statements have
consistent offsets for`sys.monitoring` .`while` statements
are now compiled with a simple jump at the end of the body, instead of
duplicating the test.
- gh-122907: Building with `HAVE_DYNAMIC_LOADING` now works as well as it
did in 3.12. Existing deficiences will be addressed separately. (See
https://github.com/python/cpython/issues/122950.)
- gh-122888: Fix crash on certain calls to `str()` with positional
arguments of the wrong type. Patch by Jelle Zijlstra.
- gh-118093: Improve the experimental JITâs handling of polymorphic code.
- gh-122697: Fixed memory leaks at interpreter shutdown in the free-threaded build, and also reporting of leaked memory blocks via ```
-X
showrefcount
```
.
- gh-116622: Fix Android stdout and stderr messages being truncated or lost.
- gh-122527: Fix a crash that occurred when a `PyStructSequence` was
deallocated after its typeâs dictionary was cleared by the GC.  The typeâs`tp_basicsize` now accounts for non-sequence
fields that arenât included in the`Py_SIZE` of the sequence.
- gh-122445: Add only fields which are modified via self.* to `__static_attributes__` .
- gh-122417: In the free-threaded build, the reference counts for heap type objects are now partially stored in a distributed manner in per-thread arrays. This reduces contention on the heap typeâs reference count fields when creating or destroying instances of the same type from multiple threads concurrently.
- gh-116090: Fix an issue in JIT builds that prevented some `for` loops from correctly firing`RAISE` monitoring events.
- gh-122300: Preserve AST nodes for f-string with single-element format specifiers. Patch by Pablo Galindo
- gh-120906: `frame.f_locals` now supports arbitrary hashable objects
as keys.
- gh-122239: When a `list` ,`tuple` or`dict` with too
many elements is unpacked, show the actual length in the error message.
- gh-122245: Detection of writes to `__debug__` is moved from the
compilerâs codegen stage to the symtable. This means that these errors are
now detected even in code that is optimized away before codegen (such as
assertions with the`-O` command line option).
- gh-122234: Specializations for sums with float and complex inputs in `sum()` now always use compensated summation.  Also, for integer items
in above specializations:`PyLong_AsDouble()` is used, instead of`PyLong_AsLongAndOverflow()` .  Patch by Sergey B Kirpichev.
- gh-122208: Dictionary watchers now only deliver the PyDict_EVENT_ADDED event when the insertion is in a known good state to succeed.
- gh-122160: Remove the `BUILD_CONST_KEY_MAP` opcode. Use`BUILD_MAP` instead.
- gh-122029: Emit `c_call` events in`sys.setprofile()` when a`PyMethodObject` pointing to a`PyCFunction` is called.
- gh-122026: Fix a bug that caused the tokenizer to not correctly identify mismatched parentheses inside f-strings in some situations. Patch by Pablo Galindo
- gh-99108: Pythonâs hashlib now unconditionally uses the vendored HACL* library for Blake2. Python no longer accepts libb2 as an optional dependency for Blake2. We refreshed HACL* to the latest version, and now vendor HACL*âs 128-bit and 256-bit wide vector implementations for Blake2, which are used on x86/x64 toolchains when the required CPU features are available at runtime. HACL*âs 128-bit wide vector implementation of Blake2 can also run on ARM NEON and Power8, but lacking evidence of a performance gain, these are not enabled (yet).

### LibraryÂ¶

### Core and BuiltinsÂ¶

- gh-121860: Fix crash when rematerializing a managed dictionary after it was deleted.
- gh-121795: Improve performance of set membership testing, `set.remove()` and`set.discard()` when the argument is a set.
- gh-121814: Fixed the SegFault when `PyEval_SetTrace()` is used with
no Python frame on stack.
- gh-121295: Fix PyREPL console getting into a blocked state after interrupting a long paste
- gh-121794: Fix bug in free-threaded Python where a resurrected object could lead to a negative ref count assertion failure.
- gh-121657: Improve the `SyntaxError` message if the user tries to use`yield from` outside a function.
- gh-121609: Fix pasting of characters containing unicode character joiners in the new REPL. Patch by Marta Gomez Macias
- gh-121297: Previously, incorrect usage of `await` or asynchronous
comprehensions in code removed by the`-O` option was not flagged
by the Python compiler. Now, such code raises`SyntaxError` . Patch by
Jelle Zijlstra.
- gh-117482: Unexpected slot wrappers are no longer created for builtin static types in subinterpreters.
- gh-121562: Optimized performance of hex_from_char by replacing switch-case with a lookup table
- gh-121499: Fix a bug affecting how multi-line history was being rendered in the new REPL after interacting with the new screen cache. Patch by Pablo Galindo
- gh-121497: Fix a bug that was preventing the REPL to correctly respect the history when an input hook was set. Patch by Pablo Galindo
- gh-121012: Tier 2 execution now ensures that list iterators remain exhausted, once they become exhausted.
- gh-121439: Allow tuples of length 20 in the freelist to be reused.
- gh-121288: `ValueError` messages for`list.index()` ,`range.index()` ,`deque.index()` ,`deque.remove()` and`ShareableList.index()` no longer contain the repr of the searched
value (which can be arbitrary large) and are consistent with error
messages for other`index()` and`remove()` methods.
- gh-121368: Fix race condition in `_PyType_Lookup` in the free-threaded
build due to a missing memory fence.  This could lead to`_PyType_Lookup` returning incorrect results on arm64.
- gh-121149: Added specialization for summation of complexes, this also improves accuracy of builtin `sum()` for such inputs.  Patch by Sergey
B Kirpichev.
- gh-121130: Fix f-strings with debug expressions in format specifiers. Patch by Pablo Galindo

### LibraryÂ¶

- gh-121381: Remove `subprocess._USE_VFORK` escape hatch code and
documentation. It was added just in case, and doesnât have any known cases
that require it.

### Core and BuiltinsÂ¶

- gh-119726: Optimize code layout for calls to C functions from the JIT on AArch64. Patch by Diego Russo.
- gh-121115: `PyLong_AsNativeBytes()` no longer uses`__index__()` methods by default. The`Py_ASNATIVEBYTES_ALLOW_INDEX` flag has been added to allow it.
- gh-120838: `Py_Finalize()` and`Py_FinalizeEx()` now
always run with the main interpreter active.
- gh-113433: Subinterpreters now get cleaned up automatically during runtime finalization.
- gh-119726: Improve the speed and memory use of C function calls from JIT code on AArch64. Patch by Diego Russo
- gh-116017: Simplify the warmup mechanism used for âside exitsâ in JIT code, resulting in slightly better performance and slightly lower memory usage for most platforms.
- gh-98442: Fix too wide source locations of the cleanup instructions of a with statement.
- gh-120754: Reduce the number of system calls invoked when reading a whole file (ex. `open('a.txt').read()` ). For a sample program that reads the
contents of the 400+`.rst` files in the cpython repository`Doc` folder, there is an over 10% reduction in system call count.
- gh-119462: Make sure that invariants of type versioning are maintained: * Superclasses always have their version number assigned before subclasses * The version tag is always zero if the tag is not valid. * The version tag is always non-if the tag is valid.
- gh-120437: Fix `_CHECK_STACK_SPACE` optimization problems introduced in
gh-118322.
- gh-120722: Correctly set the bytecode position on return instructions within lambdas. Patch by Jelle Zijlstra.
- gh-120367: Fix bug where compiler creates a redundant jump during pseudo-op replacement. Can only happen with a synthetic AST that has a try on the same line as the instruction following the exception handler.
- gh-120507: Remove the `BEFORE_WITH` and`BEFORE_ASYNC_WITH` instructions. Add the new`LOAD_SPECIAL` instruction. Generate
code for`with` and`async with` statements using the new instruction.
- gh-113993: Strings interned with `sys.intern()` are again
garbage-collected when no longer used, as per the documentation. Strings
interned with the C function`PyUnicode_InternInPlace()` are still
immortal. Internals of the string interning mechanism have been changed.
This may affect performance and identities of`str` objects.

### LibraryÂ¶

- gh-120485: Add an override of `allow_reuse_port` on classes subclassing`socketserver.TCPServer` where`allow_reuse_address` is also
overridden.

### Core and BuiltinsÂ¶

- gh-120384: Fix an array out of bounds crash in `list_ass_subscript` ,
which could be invoked via some specifically tailored input: including
concurrent modification of a list object, where one thread assigns a slice
and another clears it.
- gh-120367: Fix crash in compiler on code with redundant NOPs and JUMPs which show up after exception handlers are moved to the end of the code.

### LibraryÂ¶

- gh-120380: Fix Python implementation of `pickle.Pickler` for`bytes` and`bytearray` objects when using protocol version
5. Patch by BÃ©nÃ©dikt Tran.

### Core and BuiltinsÂ¶

- gh-120397: Improve the throughput by up to two times for the `str.count()` ,`bytes.count()` and`bytearray.count()` methods
for counting single characters.
- gh-120221: Deliver real signals on Ctrl-C and Ctrl-Z in the new REPL. Patch by Pablo Galindo
- gh-120346: Respect `PYTHON_BASIC_REPL` when running in interactive
inspect mode (`python -i` ). Patch by Pablo Galindo
- gh-93691: Fix source locations of instructions generated for the iterator of a for statement.
- gh-120198: Fix a crash when multiple threads read and write to the same `__class__` of an object concurrently.
- gh-120298: Fix use-after free in `list_richcompare_impl` which can be
invoked via some specifically tailored evil input.
- gh-119666: Fix a compiler crash in the case where two comprehensions in class scope both reference `__class__` .
- gh-119726: JIT: Re-use trampolines on AArch64 when creating stencils. Patch by Diego Russo
- gh-120225: Fix crash in compiler on empty block at end of exception handler.
- gh-93691: Fix source locations of instructions generated for with statements.
- gh-120097: `FrameLocalsProxy` now subclasses`collections.abc.Mapping` and can be matched as a mapping in`match` statements
- gh-120080: Direct call to the `int.__round__()` now accepts`None` as a valid argument.
- gh-119933: Improve `SyntaxError` messages for invalid expressions in
a type parameters bound, a type parameter constraint tuple or a default
type parameter. Patch by BÃ©nÃ©dikt Tran.
- gh-119724: Reverted improvements to error messages for `elif` /`else` statements not matching any valid statements, which made in hard to locate
the syntax errors inside those`elif` /`else` blocks.
- gh-119879: String search is now slightly faster for certain cases. It now utilizes last character gap (good suffix rule) for two-way periodic needles.
- gh-119842: Honor `PyOS_InputHook()` in the new REPL. Patch by Pablo
Galindo
- gh-119180: `classmethod()` and`staticmethod()` now wrap the`__annotations__` and`__annotate__` attributes of their underlying callable lazily. See**PEP 649** . Patch by
Jelle Zijlstra.
- gh-119821: Fix execution of annotation scopes within classes when `globals` is set to a non-dict. Patch by Jelle
Zijlstra.
- gh-118934: Make `PyEval_GetLocals` return borrowed reference
- gh-119740: Remove the previously-deprecated delegation of `int()` to`__trunc__()` .
- gh-119689: Generate stack effect metadata for pseudo instructions from bytecodes.c.
- gh-109218: `complex()` accepts now a string only as a positional
argument. Passing a complex number as the ârealâ or âimagâ argument is
deprecated; it should only be passed as a single positional argument.
- gh-119548: Add a `clear` command to the REPL. Patch by Pablo Galindo
- gh-111999: Fix the signature of `str.format_map()` .
- gh-119560: An invalid assert in beta 1 has been removed. The assert would fail if `PyState_FindModule()` was used in an extension moduleâs init
function before the module def had been initialized.
- gh-119369: Fix deadlock during thread deletion in free-threaded build, which could occur when the GIL was enabled at runtime.
- gh-119525: Fix deadlock involving `_PyType_Lookup()` cache in the
free-threaded build when the GIL is dynamically enabled at runtime.
- gh-119258: Eliminate type version guards in the tier two interpreter. Note that setting the `tp_version_tag` manually (which has never been
supported) may result in crashes.
- gh-119311: Fix bug where names are unexpectedly mangled in the bases of generic classes.
- gh-119395: Fix bug where names appearing after a generic class are mangled as if they are in the generic class.
- gh-119372: Correct invalid corner cases in complex division (resulted in `(nan+nanj)` output), e.g.`1/complex('(inf+infj)')` .  Patch by
Sergey B Kirpichev.
- gh-119180: Evaluation of annotations is now deferred. See **PEP 649** for
details.
- gh-119180: Replace `LOAD_ASSERTION_ERROR` opcode with`LOAD_COMMON_CONSTANT` and add support for`NotImplementedError` .
- gh-119213: Non-builtin modules built with argument clinic were crashing if used in a subinterpreter before the main interpreter. The objects that were causing the problem by leaking between interpreters carelessly have been fixed.
- gh-119011: Fixes `type.__type_params__` to return an empty tuple instead
of a descriptor.
- gh-118692: Avoid creating unnecessary `StopIteration` instances for
monitoring.
- gh-119180: Add an `__annotate__` attribute to functions, classes, and
modules as part of**PEP 649** . Patch by Jelle Zijlstra.
- gh-119049: Fix displaying the source line for warnings created by the C API if the `warnings` module had not yet been imported.
- gh-119057: Improve `ZeroDivisionError` error message. Now, all error
messages are harmonized: all`/` ,`//` , and`%` operations just use
âdivision by zeroâ message. And`0 ** -1` operation uses âzero to a
negative powerâ.
- gh-118844: Fix build failures when configuring with both `--disable-gil` and`--enable-experimental-jit` .
- gh-118921: Add `copy()` method for`FrameLocalsProxy` which returns a
snapshot`dict` for local variables.
- gh-117657: Fix data races on the field that stores a pointer to the interpreterâs main thread that occur in free-threaded builds.
- gh-118750: If the C version of the `decimal` module is available,`int(str)` now uses it to supply an asymptotically much faster
conversion. However, this only applies if the string contains over about 2
million digits.
- gh-118767: Using `NotImplemented` in a boolean context now raises`TypeError` . Contributed by Jelle Zijlstra.
- gh-118561: Fix race condition in free-threaded build where `list.extend()` could expose uninitialised memory to concurrent
readers.
- gh-117139: Convert the Python evaluation stack to use internal stack references. The purpose is to support tagged pointers. In **PEP 703** , this
will allow for its form of deferred reference counting. For both the
default and free-threaded builds, this sets up the infrastructure for
unboxed integers in the future.

### LibraryÂ¶

- gh-115801: Raise `TypeError` when passing a string to`difflib.unified_diff()` and`difflib.context_diff()` .

### Core and BuiltinsÂ¶

- gh-117558: Improve error messages when a string, bytes or bytearray object of length 1 is expected.
- gh-117195: Avoid assertion failure for debug builds when calling `object.__sizeof__(1)`
- gh-116022: Improve the `__repr__()` output of`AST` nodes.
- gh-114091: Changed the error message for awaiting something that canât be awaited from âobject <type> canât be used in an await expressionâ to ââ<type>â object canât be awaitedâ.
- gh-113190: `Py_Finalize()` now deletes all interned strings.
- gh-84978: Add class methods `float.from_number()` and`complex.from_number()` .
- gh-95144: Improve the error message from `a in b` when`b` is not a
container to mention the term âcontainerâ.
- bpo-24766: Fix handling of `doc` argument to subclasses of`property` .

### C APIÂ¶

- gh-123961: Convert the curses.window static type exposed by the `PyCursesWindow_Type` macro in`Include/py_curses.h` to a heap type. Patch by
BÃ©nÃ©dikt Tran.
- gh-124502: Add `PyUnicode_Equal()` function to the limited C API:
test if two strings are equal. Patch by Victor Stinner.
- gh-124296: `PyDictObject` no longer maintains a private version
tag field`ma_version_tag` per**PEP 699** .  This field was originally
added in Python 3.6 (**PEP 509** ) and deprecated in Python 3.12.
- gh-124160: Fix crash when importing modules containing state and single-phase initialization in a subinterpreter.
- gh-124153: Add `PyType_GetBaseByToken()` and`Py_tp_token` slot for easier type checking, related to**PEP 489** and**PEP 630** .
- gh-124127: In the limited C API 3.14 and newer, `Py_REFCNT()` is now
implemented as an opaque function call to hide implementation details.
Patch by Victor Stinner.
- gh-123880: Fixed a bug that prevented circular imports of extension modules that use single-phase initialization.
- gh-123909: `PyType_FromSpec()` ,`PyType_FromSpecWithBases()` and`PyType_FromModuleAndSpec()` will now fail if the metaclass of
the new type has custom`tp_new` .
- gh-107954: Add functions to configure the Python initialization ( **PEP 741** ):Patch by Victor Stinner.
- gh-107954: Add functions to get and set the current runtime Python configuration: Patch by Victor Stinner.
- gh-123465: `Py_RELATIVE_OFFSET` is now allowed in`PyMemberDef` for the special offset member`"__vectorcalloffset__"` , as well as the discouraged special offset
members`"__dictoffset__"` and`"__weaklistoffset__"`
- gh-100554: Added a slot `Py_tp_vectorcall` to set`tp_vectorcall` via the`PyType_FromSpec()` function family. Limited API extensions can use this feature to provide
more efficient vector call-based implementation of`__new__` and`__init__` .
- gh-122854: Add `Py_HashBuffer()` to compute and return the hash
value of a buffer. Patch by Antoine Pitrou and Victor Stinner.
- gh-122728: Fix `PyEval_GetLocals()` to avoid`SystemError` (âbad argument to internal functionâ). Patch by Victor Stinner.
- gh-116622: Make `PyObject_Print` work around a bug in Android and
OpenBSD which prevented it from throwing an exception when trying to write
to a read-only stream.
- gh-105201: Add `PyIter_NextItem()` to replace`PyIter_Next()` ,
which has an ambiguous return value. Patch by Irit Katriel and Erlend
Aasland.
- gh-121489: Export private `_PyBytes_Join()` again.
- gh-121645: Add `PyBytes_Join(sep, iterable)` function, similar to`sep.join(iterable)` in Python. Patch by Victor
Stinner.
- gh-89364: Export the `PySignal_SetWakeupFd()` function. Previously,
the function was documented but it couldnât be used in 3rd party code.
Patch by Victor Stinner.
- gh-113993: `PyUnicode_InternInPlace()` no longer prevents its
argument from being garbage collected.Several functions that take `char *` are now documented as possibly
preventing string objects from being garbage collected; refer to their
documentation for details:`PyUnicode_InternFromString()` ,`PyDict_SetItemString()` ,`PyObject_SetAttrString()` ,`PyObject_DelAttrString()` ,`PyUnicode_InternFromString()` ,
and`PyModule_Add*` convenience functions.
- gh-113601: Removed debug build assertions related to interning strings, which were falsely triggered by stable ABI extensions.
- gh-112136: Restore the private `_PyArg_Parser` structure and the private`_PyArg_ParseTupleAndKeywordsFast()` function, previously removed in
Python 3.13 alpha 1. Patch by Victor Stinner.
- gh-120642: Remove the private `_Py_CODEUNIT` type from the public C API.
The internal`pycore_code.h` header should now be used to get this
internal type. Patch by Victor Stinner.
- gh-120858: `PyDict_Next()` no longer locks the dictionary in the
free-threaded build.  The locking needs to be done by the caller around
the entire iteration loop.
- gh-120642: Remove the following unstable functions: 
  - `PyUnstable_Replace_Executor()`
  - `PyUnstable_SetOptimizer()`
  - `PyUnstable_GetOptimizer()`
  - `PyUnstable_GetExecutor()`
  - `PyUnstable_Optimizer_NewCounter()`
  - `PyUnstable_Optimizer_NewUOpOptimizer()`
 Patch by Victor Stinner.
- gh-120389: Add new functions to convert C `<stdint.h>` numbers from/to
Python`int` :Patch by Victor Stinner.
- gh-120600: In the limited C API 3.14 and newer, `Py_TYPE()` is now
implemented as an opaque function call to hide implementation details.
Patch by Victor Stinner.
- gh-70278: `PyUnicode_FromFormat()` no longer produces the ending`\ufffd` character for truncated C string when use precision with`%s` and`%V` . It now truncates the string before the start of truncated
multibyte sequences.
- gh-119771: Set `errno` in`_Py_c_pow()` on overflows.  Patch
by Sergey B Kirpichev.
- gh-119182: Add a new `PyUnicodeWriter` API to create a Python`str` object:Patch by Victor Stinner.
- gh-119613: Soft deprecate the `Py_MEMCPY` macro: use directly`memcpy()` instead.  Patch by Victor Stinner.
- gh-119775: Creating `immutable types` with mutable bases was deprecated since 3.12 and now raises a`TypeError` .
- gh-119585: Fix crash when a thread state that was created by `PyGILState_Ensure()` calls a destructor that during`PyThreadState_Clear()` that calls back into`PyGILState_Ensure()` and`PyGILState_Release()` . This might
occur when in the free-threaded build or when using thread-local variables
whose destructors call`PyGILState_Ensure()` .
- gh-119613: Macros `Py_IS_NAN` ,`Py_IS_INFINITY` and`Py_IS_FINITE` are soft deprecated.
- gh-119336: Restore the removed `_PyLong_NumBits()` function. It is used
by the pywin32 project. Patch by Ethan Smith
- gh-119344: The critical section API is now public as part of the non-limited C API.
- gh-119333: Add `PyContext_AddWatcher()` and`PyContext_ClearWatcher()` APIs to register callbacks to receive
notification on enter and exit of context objects.
- gh-119247: Added `Py_BEGIN_CRITICAL_SECTION_SEQUENCE_FAST` and`Py_END_CRITICAL_SECTION_SEQUENCE_FAST` macros to make it possible to
use PySequence_Fast APIs safely when free-threaded, and update str.join to
work without the GIL using them.
- gh-111389: Add `PyHASH_MULTIPLIER` constant: prime multiplier
used in string and various other hashes. Patch by Victor Stinner.
- gh-118771: Several C declarations with names that didnât start with the `Py` or`_Py` prefixes, which were added by mistake in 3.13 alpha and
beta releases, were moved to internal headers.
- gh-116984: Make mimalloc includes relative to the current file to avoid embedders or extensions needing to include `Internal/mimalloc` if they
are already including internal CPython headers.
- gh-118789: Add `PyUnstable_Object_ClearWeakRefsNoCallbacks()` , which
clears weakrefs without calling their callbacks.
- gh-118789: Restore `_PyWeakref_ClearRef` that was previously removed in
Python 3.13 alpha 1.
- gh-117511: Make the `PyMutex` public in the non-limited C API.
- gh-116560: Add `PyLong_GetSign()` function.  Patch by Sergey B
Kirpichev.
- gh-97588: Fix creating bitfields in `ctypes` structures and unions.
Fields no longer overlap.
- gh-87135: Attempting to acquire the GIL after runtime finalization has begun in a different thread now causes the thread to hang rather than terminate, which avoids potential crashes or memory corruption caused by attempting to terminate a thread that is running code not specifically designed to support termination. In most cases this hanging is harmless since the process will soon exit anyway. The `PyThread_exit_thread` function is now deprecated.  Its behavior is
inconsistent across platforms, and it can only be used safely in the
unlikely case that every function in the entire call stack has been
designed to support the platform-dependent termination mechanism.  It is
recommended that users of this function change their design to not require
thread termination.  In the unlikely case that thread termination is
needed and can be done safely, users may migrate to calling
platform-specific APIs such as`pthread_exit` (POSIX) or`_endthreadex` (Windows) directly.
- bpo-34008: Added `Py_IsInitialized` to the list of APIs that are safe to
call before the interpreter is initialized, and updated the embedding
tests to cover it.

### BuildÂ¶

- gh-125269: Fix detection of whether `-latomic` is needed when
cross-compiling CPython using the configure script.
- gh-123990: Remove `WITH_FREELISTS` macro and`--without-freelists` build configuration
- gh-124102: Update internal documentation under PCbuild, so it now correctly states that Windows requires VS2017 or later and Python 3.10 or later
- gh-124043: Building using `--with-trace-refs` is (temporarily)
disallowed when the GIL is disabled.
- gh-123418: Updated Android build to use OpenSSL 3.0.15.
- gh-123297: Propagate the value of `LDFLAGS` to`LDCXXSHARED` in`sysconfig` . Patch by Pablo Galindo
- gh-121634: Allow for specifying the target compile triple for WASI.
- gh-122578: Use WASI SDK 24 for testing.
- gh-116622: Rename build variable `MODULE_LDFLAGS` back to`LIBPYTHON` ,
as itâs used by package build systems (e.g. Meson).
- gh-118943: Fix an issue where the experimental JIT could be built several times by the `make regen-all` target, leading to possible race
conditions on heavily parallelized builds.
- gh-121996: Introduce ./configure âdisable-safety and âenable-slower-safety options. Patch by Donghee Na.
- gh-120522: Added a `--with-app-store-compliance` option to patch
out known issues with macOS/iOS App Store review processes.
- gh-120371: Support WASI SDK 22 by explicitly skipping functions that are just stubs in wasi-libc.
- gh-121731: Fix mimalloc compile error on GNU/Hurd
- gh-121487: Fix deprecation warning for ATOMIC_VAR_INIT in mimalloc.
- gh-121467: Fix a Makefile bug that prevented mimalloc header files from being installed.
- gh-121103: On POSIX systems, excluding macOS framework installs, the lib directory for the free-threaded build now includes a âtâ suffix to avoid conflicts with a co-located default build installation.
- gh-120831: The default minimum iOS version was increased to 13.0.
- gh-121082: Fix build failure when the developer use `--enable-pystats` arguments in configuration command after #118450.
- gh-120671: Fix failing configure tests due to a missing space when appending to CFLAGS.
- gh-120602: Correctly handle LLVM installs with `LLVM_VERSION_SUFFIX` when building with`--enable-experimental-jit` .
- gh-120688: On WASI in debug mode, Python is now built with compiler flag `-O3` instead of`-Og` , to support more recursive calls. Patch by
Victor Stinner.
- gh-118943: Fix a possible race condition affecting parallel builds configured with `--enable-experimental-jit` , in which`FileNotFoundError` could be caused by another process already moving`jit_stencils.h.new` to`jit_stencils.h` .
- gh-120326: On Windows, fix build error when `--disable-gil` and`--experimental-jit` options are combined.
- gh-120291: Make the `python-config` shell script compatible with
non-bash shells.
- gh-113565: Improve `curses` and`curses.panel` dependency checks
in**configure** .
- gh-119729: On POSIX systems, the pkg-config ( `.pc` ) filenames now
include the ABI flags, which may include debug (âdâ) and free-threaded
(âtâ).  For example: *`python-3.14.pc` (default, non-debug build) *`python-3.14d.pc` (default, debug build) *`python-3.14t.pc` (free-threaded build)
- gh-119400: `make_ssl_certs` , the script that prepares certificate data
for the test suite, now allows specifying expiration dates.
- gh-115119: Fall back to the bundled libmpdec if a system version cannot be found.
- gh-119132: Update `sys.version` to identify whether the build is
default build or free-threading build. Patch By Donghee Na.
- gh-118836: Fix an `AssertionError` when building with`--enable-experimental-jit` and the compiler emits a`SHT_NOTE` section.
- gh-118943: Fix a possible race condition affecting parallel builds configured with `--enable-experimental-jit` , in which compilation errors
could be caused by an incompletely-generated header file.

## Python 3.13.0 beta 1Â¶

*Release date: 2024-05-08*

### SecurityÂ¶

### Core and BuiltinsÂ¶

- gh-118414: Add instrumented opcodes to YIELD_VALUE assertion for tracing cases.
- gh-117953: When a builtin or extension module is imported for the first time, while a subinterpreter is active, the moduleâs init function is now run by the main interpreter first before import continues in the subinterpreter. Consequently, single-phase init modules now fail in an isolated subinterpreter without the init function running under that interpreter, whereas before it would run under the subinterpreter *before* failing, potentially leaving behind global state and callbacks and
otherwise leaving the module in an inconsistent state.
- gh-117549: Donât use designated initializer syntax in inline functions in internal headers. They cause problems for C++ or MSVC users who arenât yet using the latest C++ standard (C++20). While internal, pycore_backoff.h, is included (indirectly, via pycore_code.h) by some key 3rd party software that does so for speed.

### LibraryÂ¶

- gh-95382: Improve performance of `json.dumps()` and`json.dump()` when using the argument*indent* . Depending on the data the encoding using`json.dumps()` with*indent* can be up to 2 to 3 times faster.

### Core and BuiltinsÂ¶

- gh-116322: In `--disable-gil` builds, the GIL will be enabled while
loading C extension modules. If the module indicates that it supports
running without the GIL, the GIL will be disabled once loading is
complete. Otherwise, the GIL will remain enabled for the remainder of the
interpreterâs lifetime. This behavior does not apply if the GIL has been
explicitly enabled or disabled with`PYTHON_GIL` or`-Xgil` .
- gh-118513: Fix incorrect `UnboundLocalError` when two comprehensions
in the same function both reference the same name, and in one
comprehension the name is bound while in the other itâs an implicit
global.
- gh-118518: Allow the Linux perf support to work without frame pointers using perfâs advanced JIT support. The feature is activated when using the `PYTHON_PERF_JIT_SUPPORT` environment variable or when running Python
with`-Xperf_jit` . Patch by Pablo Galindo.
- gh-117514: Add `sys._is_gil_enabled()` function that returns whether the
GIL is currently enabled.  In the default build it always returns`True` because the GIL is always enabled.  In the free-threaded build, it may
return`True` or`False` .
- gh-118164: Break a loop between the Python implementation of the `decimal` module and the Python code for integer to string
conversion. Also optimize integer to string conversion for values in the
range from 9_000 to 135_000 decimal digits.
- gh-118473: Fix `sys.set_asyncgen_hooks()` not to be partially set when
raising`TypeError` .
- gh-118465: Compiler populates the new `__firstlineno__` field on a class
with the line number of the first line of the class definition.
- gh-118492: Fix an issue where the type cache can expose a previously accessed attribute when a finalizer is run.
- gh-117714: update `async_generator.athrow().close()` and`async_generator.asend().close()` to close their section of the
underlying async generator
- gh-111201: The interactive interpreter is now implemented in Python, which allows for a number of new features like colors, multiline input, history viewing, and paste mode. Contributed by Pablo Galindo, Åukasz Langa and Lysandros Nikolaou based on code from the PyPy project.
- gh-74929: Implement PEP 667: converted `FrameType.f_locals` and`PyFrame_GetLocals()` to return a
write-through proxy object when the frame refers to a function or
comprehension.
- gh-116767: Fix crash in compiler on âasync withâ that has many context managers.
- gh-118335: Change how to use the tier 2 interpreter. Instead of running Python with `-X uops` or setting the environment variable`PYTHON_UOPS=1` , this choice is now made at build time by configuring
with`--enable-experimental-jit=interpreter` .**Beware!** This changes the environment variable to enable or disable
micro-ops to`PYTHON_JIT` . The old`PYTHON_UOPS` is no longer used.
- gh-118306: Update JIT compilation to use LLVM 18
- gh-118160: Annotation scopes within classes can now contain comprehensions. However, such comprehensions are not inlined into their parent scope at runtime. Patch by Jelle Zijlstra.
- gh-118272: Fix bug where `generator.close` does not free the generator
frameâs locals.
- gh-118216: Donât consider `__future__` imports with dots before the
module name.
- gh-118074: Make sure that the Executor objects in the COLD_EXITS array arenât assumed to be GC-able (which would access bytes outside the object).
- gh-107674: Lazy load frame line number to improve performance of tracing
- gh-118082: Improve `SyntaxError` message for imports without names,
like in`from x import` and`import` cases. It now points out to users
that`import` expects at least one name after it.
- gh-118090: Improve `SyntaxError` message for empty type param
brackets.
- gh-117958: Added a `get_jit_code()` method to access JIT compiled
machine code from the UOp Executor when the experimental JIT is enabled.
Patch by Anthony Shaw.
- gh-117901: Add option for compilerâs codegen to save nested instruction sequences for introspection.
- gh-116622: Redirect stdout and stderr to system log when embedded in an Android app.
- gh-109118: annotation scope within class scopes can now contain lambdas.
- gh-117894: Prevent `agen.aclose()` objects being re-used after`.throw()` .
- gh-117881: prevent concurrent access to an async generator via athrow().throw() or asend().throw()
- gh-117536: Fix a `RuntimeWarning` when calling`agen.aclose().throw(Exception)` .
- gh-117755: Fix mimalloc allocator for huge memory allocation (around 8,589,934,592 GiB) on s390x. Patch by Victor Stinner.
- gh-117750: Fix issue where an objectâs dict would get out of sync with the objectâs internal values when being cleared. `obj.__dict__.clear()` now
clears the internal values, but leaves the dict attached to the object.
- gh-117431: Improve the performance of the following `bytes` and`bytearray` methods by adapting them to the`METH_FASTCALL` calling convention:
  - `count()`
  - `find()`
  - `index()`
  - `rfind()`
  - `rindex()`
- gh-117709: Speed up calls to `str()` with positional-only argument, by
using the**PEP 590**`vectorcall` calling convention. Patch by Erlend
Aasland.
- gh-117680: Give `_PyInstructionSequence` a Python interface and use it
in tests.
- gh-115776: Statically allocated objects are, by definition, immortal so must be marked as such regardless of whether they are in extension modules or not.
- gh-117385: Remove unhandled `PY_MONITORING_EVENT_BRANCH` and`PY_MONITORING_EVENT_EXCEPTION_HANDLED` events from`sys.settrace()` .
- gh-116322: Extension modules may indicate to the runtime that they can run without the GIL. Multi-phase init modules do so by calling providing `Py_MOD_GIL_NOT_USED` for the`Py_mod_gil` slot, while single-phase
init modules call`PyUnstable_Module_SetGIL(mod, Py_MOD_GIL_NOT_USED)` from their init function.
- gh-116129: Implement **PEP 696** , adding support for defaults on type
parameters. Patch by Jelle Zijlstra.
- gh-93502: Add two new functions to the C-API, `PyRefTracer_SetTracer()` and`PyRefTracer_GetTracer()` , that
allows to track object creation and destruction the same way the`tracemalloc` module does. Patch by Pablo Galindo
- gh-107674: Improved the performance of `sys.settrace()` significantly
- gh-95754: Improve the error message when a script shadowing a module from the standard library causes `AttributeError` to be raised. Similarly,
improve the error message when a script shadowing a third party module
attempts to access an attribute from that third party module while still
initialising.
- gh-99180: Elide uninformative traceback indicators in `return` and
simple`assignment` statements. Patch by Pablo Galindo.
- gh-105879: Allow the *globals* and*locals* arguments to`exec()` and`eval()` to be passed as keywords.

### LibraryÂ¶

- gh-118418: A `DeprecationWarning` is now emitted if you fail to pass
a value to the new*type_params* parameter of`typing._eval_type()` or`typing.ForwardRef._evaluate()` . (Using either of these private and
undocumented functions is discouraged to begin with, but failing to pass a
value to the`type_params` parameter may lead to incorrect behaviour on
Python 3.12 or newer.)
- gh-118660: Add an optional second type parameter to `typing.ContextManager` and`typing.AsyncContextManager` ,
representing the return types of`__exit__()` and`__aexit__()` respectively. This parameter defaults to```
bool
| None
```
.
- gh-118650: The `enum` module allows method named`_repr_*` to be
defined on`Enum` types.
- gh-118648: Add type parameter defaults to `typing.Generator` and`typing.AsyncGenerator` .
- gh-101137: Mime type `text/x-rst` is now supported by`mimetypes` .
- gh-118164: The Python implementation of the `decimal` module could
appear to hang in relatively small power cases (like`2**117` ) if
context precision was set to a very high value. A different method to
check for exactly representable results is used now that doesnât rely on
computing`10**precision` (which could be effectively too large to
compute).
- gh-111744: `breakpoint()` and`pdb.set_trace()` now enter the debugger
immediately after the call rather than before the next line is executed.
- gh-118500: Add `pdb` support for zipapps
- gh-118406: Add signature for `sqlite3.Connection` objects.
- gh-101732: Use a Y2038 compatible openssl time function when available.
- gh-118404: Fix `inspect.signature()` for non-comparable callables.
- gh-118402: Fix `inspect.signature()` for the result of the`functools.cmp_to_key()` call.
- gh-116622: On Android, `sysconfig.get_platform` now returns the
format specified by**PEP 738** .
- gh-118285: Allow to specify the signature of custom callable instances of extension type by the `__text_signature__` attribute. Specify signatures
of`operator.attrgetter` ,`operator.itemgetter` , and`operator.methodcaller` instances.
- gh-118314: Fix an edge case in `binascii.a2b_base64()` strict mode,
where excessive padding is not detected when no padding is necessary.
- gh-118271: Add the `PhotoImage` methods`read()` to read an
image from a file and`data()` to get the image data. Add*background* and*grayscale* parameters to`PhotoImage` method`write()` .
- gh-118225: Add the `PhotoImage` method`copy_replace()` to
copy a region from one image to other image, possibly with pixel zooming
and/or subsampling. Add*from_coords* parameter to`PhotoImage` methods`copy()` ,`zoom()` and`subsample()` . Add*zoom* and*subsample* parameters to`PhotoImage` method`copy()` .
- gh-118221: Fix a bug where `sqlite3.Connection.iterdump()` could fail
if a custom`row factory` was used.
Patch by Erlend Aasland.
- gh-118013: Fix regression introduced in gh-103193 that meant that calling `inspect.getattr_static()` on an instance would cause a strong
reference to that instanceâs class to persist in an internal cache in the`inspect` module. This caused unexpected memory consumption if the
class was dynamically created, the class held strong references to other
objects which took up a significant amount of memory, and the cache
contained the sole strong reference to the class. The fix for the
regression leads to a slowdown in`getattr_static()` , but the
function should still be significantly faster than it was in Python 3.11.
Patch by Alex Waygood.
- gh-118218: Speed up `itertools.pairwise()` in the common case by up to
1.8x.
- gh-117486: Improve the behavior of user-defined subclasses of `ast.AST` . Such classes will now require no changes in the usual
case to conform with the behavior changes of the`ast` module in
Python 3.13. Patch by Jelle Zijlstra.
- gh-90848: Fixed `unittest.mock.create_autospec()` to configure parent
mock with keyword arguments.
- gh-118168: Fix incorrect argument substitution when `typing.Unpack` is used with the builtin`tuple` .`typing.Unpack` now raises`TypeError` when used with certain invalid types. Patch by Jelle
Zijlstra.
- gh-118131: Add command-line interface for the `random` module. Patch
by Hugo van Kemenade.
- gh-118107: Fix `zipimport` reading of ZIP64 files with file entries
that are too big or offset too far.
- gh-102511: Fix `os.path.normpath()` for UNC paths on Windows. Speed up`os.path.splitroot()` with a native implementation.
- gh-117535: Change the unknown filename of `warnings` from`sys` to`<sys>` to clarify that itâs not a real filename.
- gh-114053: Fix erroneous `NameError` when calling`typing.get_type_hints()` on a class that made use of**PEP 695** type
parameters in a module that had`from __future__ import annotations` at
the top of the file. Patch by Alex Waygood.
- gh-116931: Add parameter *fileobj* check for`tarfile.TarFile.addfile()`
- gh-117995: Donât raise `DeprecationWarning` when a sequence
of parameters is used to bind indexed, nameless placeholders. See also
gh-100668.
- gh-80361: Fix TypeError in `email.message.Message.get_payload()` when
the charset is**RFC 2231** encoded.
- gh-86650: Fix IndexError when parse some emails with invalid Message-ID (including one-off addresses generated by Microsoft Outlook).
- gh-117691: Improve the error messages emitted by `tarfile` deprecation warnings relating to PEP 706. If a`filter` argument is not
provided to`extract()` or`extractall` , the deprecation warning now
points to the line in the userâs code where the relevant function was
called. Patch by Alex Waygood.
- gh-115874: Fixed a possible segfault during garbage collection of `_asyncio.FutureIter` objects. Patch by Savannah Ostrowski.
- gh-115060: Speed up `pathlib.Path.glob()` by omitting an initial`is_dir()` call. As a result of this change,`glob()` can no longer raise`OSError` .
- gh-77102: `site` module now parses`.pth` file with UTF-8 first,
and locale encoding if`UnicodeDecodeError` happened. It
supported only locale encoding before.
- gh-76785: Weâve exposed the low-level `_interpreters` module for the
sake of the PyPI implementation of**PEP 734** .  It was sometimes available
as the`_xxsubinterpreters` module and was formerly used only for
testing. For the most part, it should be considered an internal module,
like`_thread` and`_imp` . See
https://discuss.python.org/t/pep-734-multiple-interpreters-in-the-stdlib/41147/26.
- gh-115060: Speed up `pathlib.Path.glob()` by not scanning directories
for non-wildcard pattern segments.
- gh-117727: Speed up `pathlib.Path.iterdir()` by using`os.scandir()` internally.
- gh-117586: Speed up `pathlib.Path.walk()` by working with strings
internally.
- gh-117722: Change the new multi-separator support in `asyncio.StreamReader.readuntil()` to only accept tuples of separators
rather than arbitrary iterables.
- gh-117692: Fixes a bug when `doctest.DocTestFinder` was failing on
wrapped`builtin_function_or_method` .
- gh-117348: Largely restored import time performance of configparser by avoiding dataclasses.
- gh-117641: Speedup `os.path.commonpath()` on Unix.
- gh-117663: Fix `_simple_enum` to detect aliases when multiple arguments
are present but only one is the member value.
- gh-117636: Speedup `os.path.join()` .
- gh-117618: Support `package.module` as`filename` for`break` command of`pdb`
- gh-102247: the status codes enum with constants in http.HTTPStatus are updated to include the names from RFC9110. This RFC includes some HTTP statuses previously only used for WEBDAV and assigns more generic names to them. The old constants are preserved for backwards compatibility.
- gh-117607: Speedup `os.path.relpath()` .
- gh-117586: Speed up `pathlib.Path.glob()` by working with strings
internally.
- gh-117225: Add colour to doctest output. Patch by Hugo van Kemenade.
- gh-117566: `ipaddress.IPv6Address.is_loopback()` will now return`True` for IPv4-mapped loopback addresses, i.e. addresses in the`::ffff:127.0.0.0/104` address space.
- gh-117546: Fix issue where `os.path.realpath()` stopped resolving
symlinks after encountering a symlink loop on POSIX.
- gh-116720: Improved behavior of `asyncio.TaskGroup` when an
external cancellation collides with an internal cancellation. For example,
when two task groups are nested and both experience an exception in a
child task simultaneously, it was possible that the outer task group would
misbehave, because its internal cancellation was swallowed by the inner
task group.In the case where a task group is cancelled externally and also must raise an `ExceptionGroup` , it will now call the parent taskâs`cancel()` method. This ensures that a`asyncio.CancelledError` will be raised at the next`await` ,
so the cancellation is not lost.An added benefit of these changes is that task groups now preserve the cancellation count ( `asyncio.Task.cancelling()` ).In order to handle some corner cases, `asyncio.Task.uncancel()` may
now reset the undocumented`_must_cancel` flag when the cancellation
count reaches zero.
- gh-117516: Add `typing.TypeIs` , implementing**PEP 742** . Patch by
Jelle Zijlstra.
- gh-117503: Fix support of non-ASCII user names in bytes paths in `os.path.expanduser()` on Posix.
- gh-117394: `os.path.ismount()` is now 2-3 times faster if the user has
permissions.
- gh-117313: Only treat `'\n'` ,`'\r'` and`'\r\n'` as line separators
in re-folding the`email` messages. Preserve control characters`'\v'` ,`'\f'` ,`'\x1c'` ,`'\x1d'` and`'\x1e'` and Unicode line
separators`'\x85'` ,`'\u2028'` and`'\u2029'` as is.
- gh-117142: Convert `_ctypes` to multi-phase initialisation
(**PEP 489** ).
- gh-66543: Add the `mimetypes.guess_file_type()` function which works
with file path. Passing file path instead of URL in`guess_type()` is soft deprecated.
- gh-68583: webbrowser CLI: replace getopt with argparse, add long options. Patch by Hugo van Kemenade.
- gh-116871: Name suggestions for `AttributeError` and`ImportError` now only include underscored names if the original name
was underscored.
- gh-116023: Donât show empty fields (value `None` or`[]` ) in`ast.dump()` by default. Add`show_empty=False` parameter to
optionally show them.
- gh-115961: Added `name` and`mode` attributes for compressed
and archived file-like objects in modules`bz2` ,`lzma` ,`tarfile` and`zipfile` . The value of the`mode` attribute of`gzip.GzipFile` was changed from integer (`1` or`2` ) to string (`'rb'` or`'wb'` ). The value of the`mode` attribute of the readable file-like object returned by`zipfile.ZipFile.open()` was changed from`'r'` to`'rb'` .
- gh-82062: Fix `inspect.signature()` to correctly handle parameter
defaults on methods in extension modules that use names defined in the
module namespace.
- gh-83856: Honor `atexit` for all`multiprocessing` start methods
- gh-113081: Print colorized exception just like built-in traceback in `pdb`
- gh-112855: Speed up pickling of `pathlib.PurePath` objects. Patch
by Barney Gale.
- gh-111744: Support opcode events in `bdb`
- gh-109617: `ncurses` : fixed a crash that could occur on macOS 13 or
earlier when Python was built with Apple Xcode 15âs SDK.
- gh-83151: Enabled arbitrary statements and evaluations in `pdb` shell
to access the local variables of the current frame, which made it possible
for multi-scope code like generators or nested function to work.
- gh-110209: Add `__class_getitem__()` to`types.GeneratorType` and`types.CoroutineType` for type
hinting purposes. Patch by James Hilton-Balfe.
- gh-108191: The `types.SimpleNamespace` now accepts an optional
positional argument which specifies initial values of attributes as a dict
or an iterable of key-value pairs.
- gh-62090: Fix assertion errors caused by whitespace in metavars or `SUPPRESS` -ed groups in`argparse` by simplifying usage formatting.
Patch by Ali Hamdan.
- gh-102402: Adjust `logging.LogRecord` to use`time.time_ns()` and fix
minor bug related to floating-point math.
- gh-100242: Bring pure Python implementation `functools.partial.__new__` more in line with the C-implementation by not just always checking for the
presence of the attribute`'func'` on the first argument of`partial` .
Instead, both the Python version and the C version perform an`isinstance(func, partial)` check on the first argument of`partial` .
- gh-99730: HEAD requests are no longer upgraded to GET request during redirects in urllib.
- gh-66410: Setting the `tkinter` module global`wantobjects` to`2` before creating the`Tk` object or call the`wantobjects()` method of the`Tk` object with argument`2` makes now arguments to callbacks registered in the`tkinter` module
to be passed as various Python objects (`int` ,`float` ,`bytes` ,`tuple` ), depending on their internal representation in Tcl, instead of
always`str` .`tkinter.wantobjects` is now set to`2` by
default.
- bpo-40943: Fix several IndexError when parse emails with truncated Message-ID, address, routes, etc, e.g. `example@` .
- bpo-39324: Add mime type mapping for .md <-> text/markdown
- bpo-18108: `shutil.chown()` now supports*dir_fd* and*follow_symlinks* keyword arguments.
- bpo-30988: Fix parsing of emails with invalid address headers having a leading or trailing dot. Patch by tsufeki.
- bpo-32839: Add the `after_info()` method for Tkinter widgets.

### DocumentationÂ¶

- gh-117928: The minimum Sphinx version required for the documentation is now 6.2.1.

### BuildÂ¶

- gh-118734: Fixes Windows build when invoked directly (not through the `build.bat` script) without specifying a value for`UseTIER2` .
- gh-115119: The `configure` option`--with-system-libmpdec` now defaults to`yes` . The bundled copy of`libmpdecimal` will be
removed in Python 3.15.
- gh-117845: Fix building against recent libedit versions by detecting readline hook signatures in **configure** .
- gh-116622: A testbed project was added to run the test suite on Android.
- gh-117645: Increase WASI stack size from 512 KiB to 8 MiB and the initial memory from 10 MiB to 20 MiB. Patch by Victor Stinner.
- gh-115119: **configure** now uses**pkg-config** to detect`decimal` dependencies if the`--with-system-libmpdec` option
is given.

### WindowsÂ¶

- gh-115119: Update Windows installer to use libmpdecimal 4.0.0.
- gh-118486: `os.mkdir()` now accepts*mode* of`0o700` to restrict
the new directory to the current user.
- gh-118347: Fixes launcher updates not being installed.
- gh-118293: The `multiprocessing` module now passes the`STARTF_FORCEOFFFEEDBACK` flag when spawning processes to tell Windows
not to change the mouse cursor.
- gh-115009: Update Windows installer to use SQLite 3.45.3.
- gh-90329: Suppress the warning displayed on virtual environment creation when the requested and created paths differ only by a short (8.3 style) name. Warnings will continue to be shown if a junction or symlink in the path caused the venv to be created in a different location than originally requested.
- gh-117786: Fixes virtual environments not correctly launching when created from a Store install.

### macOSÂ¶

- gh-115119: Update macOS installer to use libmpdecimal 4.0.0.
- gh-114099: iOS preprocessor symbol usage was made compatible with older macOS SDKs.
- gh-115009: Update macOS installer to use SQLite 3.45.3.
- gh-91629: Use `~/.config/fish/conf.d` configs and**fish_add_path** to set`PATH` when installing for the
Fish shell.

### IDLEÂ¶

- bpo-34774: Use user-selected color theme for Help => IDLE Doc.

### C APIÂ¶

- gh-118124: Fix `Py_BUILD_ASSERT` and`Py_BUILD_ASSERT_EXPR` for non-constant expressions: use`static_assert()` on C11 and newer. Patch by Victor Stinner.
- gh-110850: Add âRawâ variant of PyTime functions Patch by Victor Stinner.
- gh-117987: Restore functions removed in Python 3.13 alpha 1: Patch by Victor Stinner.
- gh-117929: Restore removed `PyEval_InitThreads()` function. Patch by
Victor Stinner.
- gh-117534: Improve validation logic in the C implementation of `datetime.datetime.fromisoformat()` to better handle invalid years.
Patch by Vlad Efanov.
- gh-68114: Fixed skipitem()âs handling of the old âwâ and âw#â formatters. These are no longer supported and now raise an exception if used.
- gh-111997: Add a C-API for firing monitoring events.

## Python 3.13.0 alpha 6Â¶

*Release date: 2024-04-09*

### Core and BuiltinsÂ¶

- gh-117494: Refactored the instruction sequence data structure out of compile.c into instruction_sequence.c.
- gh-116968: Introduce a unified 16-bit backoff counter type ( `_Py_BackoffCounter` ), shared between the Tier 1 adaptive specializer
and the Tier 2 optimizer. The API used for adaptive specialization
counters is changed but the behavior is (supposed to be) identical.The behavior of the Tier 2 counters is changed: 
  - There are no longer dynamic thresholds (we never varied these).
  - All counters now use the same exponential backoff.
  - The counter for `JUMP_BACKWARD` starts counting down from 16.
  - The `temperature` in side exits starts counting down from 64.
- gh-117431: Improve the performance of the following `bytes` and`bytearray` methods by adapting them to the`METH_FASTCALL` calling convention:
  - `endswith()`
  - `startswith()`
- gh-117431: Improve the performance of the following `str` methods
by adapting them to the`METH_FASTCALL` calling convention:
- gh-117411: Move `PyFutureFeatures` to an internal header and make it
private.
- gh-109120: Added handle of incorrect star expressions, e.g `f(3, *)` .
Patch by Grigoryev Semyon
- gh-117266: Fix crashes for certain user-created subclasses of `ast.AST` . Such classes are now expected to set the`_field_types` attribute.
- gh-99108: Updated the `hashlib` built-in HACL* project C code
from upstream that we use for many implementations when they are not
present via OpenSSL in a given build.  This also avoids the rare potential
for a C symbol name one definition rule linking issue.
- gh-117108: Change the old space bit of objects in the young generation from 0 to gcstate->visited, so that any objects created during GC will have the old bit set correctly if they get moved into the old generation.
- gh-117108: The cycle GC now chooses the size of increments based on the total heap size, instead of the rate of object creation. This ensures that it can keep up with growing heaps.
- gh-116735: For `INSTRUMENTED_CALL_FUNCTION_EX` , set`arg0` to`sys.monitoring.MISSING` instead of`None` for`CALL` event.
- gh-113964: Starting new threads and process creation through `os.fork()` are now only prevented once all non-daemon threads exit.
- gh-116626: Ensure `INSTRUMENTED_CALL_FUNCTION_EX` always emits`CALL`
- gh-116554: `list.sort()` now exploits more cases of partial ordering,
particularly those with long descending runs with sub-runs of equal
values. Those are recognized as single runs now (previously, each block of
repeated values caused a new run to be created).
- gh-114099: Added a Loader that can discover extension modules in an iOS-style Frameworks folder.
- gh-115775: Compiler populates the new `__static_attributes__` field on a
class with the names of attributes of this class which are accessed
through self.X from any function in its body.
- gh-115776: The array of values, the `PyDictValues` struct is now
embedded in the object during allocation. This provides better performance
in the common case, and does not degrade as much when the objectâs`__dict__` is materialized.
- gh-108362: Implement an incremental cyclic garbage collector. By collecting the old generation in increments, there is no need for a full heap scan. This can hugely reduce maximum pause time for programs with large heaps. Reduce the number of generations from three to two. The old generation is split into two spaces, âvisitedâ and âpendingâ. Collection happens in two steps:: * An increment is formed from the young generation and a small part of the pending space. * This increment is scanned and the survivors moved to the end of the visited space. When the collecting space becomes empty, the two spaces are swapped.

### LibraryÂ¶

### Core and BuiltinsÂ¶

- bpo-24612: Improve the `SyntaxError` that happens when ânotâ appears
after an operator. Patch by Pablo Galindo

### LibraryÂ¶

- gh-117648: Improve performance of `os.path.join()` and`os.path.expanduser()` .
- gh-117584: Raise `TypeError` for non-paths in`posixpath.relpath` .
- gh-117467: Preserve mailbox ownership when rewriting in `mailbox.mbox.flush` . Patch by Tony
Mountifield.
- gh-114848: Raise `FileNotFoundError` when`getcwd()` returns
â(unreachable)â, which can happen on Linux >= 2.6.36 with glibc < 2.27.
- gh-117459: `asyncio.asyncio.run_coroutine_threadsafe()` now keeps the
traceback of`CancelledError` ,`TimeoutError` and`InvalidStateError` which are raised in the coroutine.
- gh-117381: Fix error message for `ntpath.commonpath()` .
- gh-117337: Deprecate undocumented `glob.glob0()` and`glob.glob1()` functions. Use`glob.glob()` and pass a directory
to its*root_dir* argument instead.
- gh-117349: Optimise several functions in `os.path` .
- gh-117348: Refactored `configparser.RawConfigParser._read()` to reduce
cyclometric complexity and improve comprehensibility.
- gh-117335: Raise TypeError for non-sequences for `ntpath.commonpath()` .
- gh-66449: `configparser.ConfigParser` now accepts unnamed sections
before named ones, if configured to do so.
- gh-88014: In documentation of `gzip.GzipFile` in module gzip,
explain data type of optional constructor argument*mtime* , and recommend`mtime = 0` for generating deterministic streams.
- gh-117310: Fixed an unlikely early & extra `Py_DECREF` triggered crash
in`ssl` when creating a new`_ssl._SSLContext` if CPython was
built implausibly such that the default cipher list is empty**or** the
SSL library it was linked against reports a failure from its C`SSL_CTX_set_cipher_list()` API.
- gh-117294: A `DocTestCase` now reports as skipped if all examples in the
doctest are skipped.
- gh-98966: In `subprocess` , raise a more informative message when`stdout=STDOUT` .
- gh-117225: doctest: only print âand X failedâ when non-zero, donât pluralise â1 itemsâ. Patch by Hugo van Kemenade.
- gh-117205: Speed up `compileall.compile_dir()` by 20% when using
multiprocessing by increasing`chunksize` .
- gh-117178: Fix regression in lazy loading of self-referential modules, introduced in gh-114781.
- gh-112383: Fix `dis` moduleâs handling of`ENTER_EXECUTOR` instructions.
- gh-117182: Lazy-loading of modules that modify their own `__class__` no
longer reverts the`__class__` to`types.ModuleType` .
- gh-117084: Fix `zipfile` extraction for directory entries with the
name containing backslashes on Windows.
- gh-117114: Make `os.path.isdevdrive()` available on all platforms. For
those that do not offer Dev Drives, it will always return`False` .
- gh-117110: Fix a bug that prevents subclasses of `typing.Any` to be
instantiated with arguments. Patch by Chris Fu.
- gh-109653: Deferred select imports in importlib.metadata and importlib.resources for a 14% speedup.
- gh-70647: Start the deprecation period for the current behavior of `datetime.datetime.strptime()` and`time.strptime()` which always
fails to parse a date string with a`ValueError` involving a day of
month such as`strptime("02-29", "%m-%d")` when a year is**not** specified and the date happen to be February 29th.  This should help avoid
users finding new bugs every four years due to a natural mistaken
assumption about the API when parsing partial date values.
- gh-116987: Fixed `inspect.findsource()` for class code objects.
- gh-114099: Modify standard library to allow for iOS platform differences.
- gh-90872: On Windows, `subprocess.Popen.wait()` no longer calls`WaitForSingleObject()` with a negative timeout: pass`0` ms if the
timeout is negative. Patch by Victor Stinner.
- gh-116957: configparser: Donât leave ConfigParser values in an invalid state (stored as a list instead of a str) after an earlier read raised DuplicateSectionError or DuplicateOptionError.
- gh-115538: `_io.WindowsConsoleIO` now emit a warning if a boolean
value is passed as a filedescriptor argument.
- gh-90095: Ignore empty lines and comments in `.pdbrc`
- gh-106531: Refreshed zipfile._path from zipp 3.18, providing better compatibility for PyPy, better glob performance for deeply nested zipfiles, and providing internal access to `CompleteDirs.inject` for use
in other tests (like importlib.resources).
- gh-63207: On Windows, `time.time()` now uses the`GetSystemTimePreciseAsFileTime()` clock to have a resolution better
than 1 us, instead of the`GetSystemTimeAsFileTime()` clock which has a
resolution of 15.6 ms. Patch by Victor Stinner.
- gh-116764: Restore support of `None` and other false values in`urllib.parse` functions`parse_qs()` and`parse_qsl()` . Also, they now raise a TypeError for
non-zero integers and non-empty sequences.
- gh-116811: In `PathFinder.invalidate_caches` , delegate to`MetadataPathFinder.invalidate_caches` .
- gh-116647: Fix recursive child in dataclasses
- gh-113171: Fixed various false positives and false negatives in 
  - `ipaddress.IPv4Address.is_private` (see these docs for details)
  - `ipaddress.IPv4Address.is_global`
  - `ipaddress.IPv6Address.is_private`
  - `ipaddress.IPv6Address.is_global`
 Also in the corresponding `ipaddress.IPv4Network` and`ipaddress.IPv6Network` attributes.
- gh-63283: In `encodings.idna` , any capitalization of the ACE prefix
(`xn--` ) is now acceptable. Patch by Pepijn de Vos and Zackery Spytz.
- gh-71042: Add `platform.android_ver()` , which provides device and OS
information on Android.
- gh-73468: Added new `math.fma()` function, wrapping C99âs`fma()` operation: fused multiply-add function. Patch by Mark Dickinson and Victor
Stinner.
- gh-116608: The `importlib.resources` functions`is_resource()` ,`open_binary()` ,`open_text()` ,`path()` ,`read_binary()` , and`read_text()` are un-deprecated, and support
subdirectories via multiple positional arguments. The`contents()` function also allows subdirectories,
but remains deprecated.
- gh-116484: Change automatically generated `tkinter.Checkbutton` widget names to avoid collisions with automatically generated`tkinter.ttk.Checkbutton` widget names within the same parent
widget.
- gh-114314: In `ctypes` , ctype data is now stored in type objects
directly rather than in a dict subclass. This is an internal change that
should not affect usage.
- gh-116401: Fix blocking `os.fwalk()` and`shutil.rmtree()` on
opening named pipe.
- gh-71052: Implement `ctypes.util.find_library()` on Android.
- gh-90535: Fix support of *interval* values > 1 in`logging.TimedRotatingFileHandler` for`when='MIDNIGHT'` and`when='Wx'` .
- gh-113308: Remove some internal protected parts from `uuid` :`_has_uuid_generate_time_safe` ,`_netbios_getnode` ,`_ipconfig_getnode` , and`_load_system_functions` . They were unused.
- gh-115627: Fix the `ssl` module error handling of connection
terminate by peer. It now throws an OSError with the appropriate error
code instead of an EOFError.
- gh-114847: Speed up `os.path.realpath()` on non-Windows platforms.
- gh-114271: Fix a race in `threading.Thread.join()` .`threading._MainThread` now always represents the main thread of the
main interpreter.`PyThreadState.on_delete` and`PyThreadState.on_delete_data` have been
removed.
- gh-113538: Add `asyncio.Server.close_clients()` and`asyncio.Server.abort_clients()` methods which allow to more
forcefully close an asyncio server.
- gh-85287: Changes Unicode codecs to return UnicodeEncodeError or UnicodeDecodeError, rather than just UnicodeError.
- gh-113548: `pdb` now allows CLI arguments to`pdb -m` .
- gh-112948: Make completion of `pdb` similar to Python REPL
- gh-105866: Fixed `_get_slots` bug which caused error when defining
dataclasses with slots and a weakref_slot.
- gh-96471: Add `asyncio.Queue` termination with`shutdown()` method.
- gh-89739: The `zipimport` module can now read ZIP64 files.
- bpo-33533: `asyncio.as_completed()` now returns an object that is both
an asynchronous iterator and plain iterator. The new asynchronous
iteration pattern allows for easier correlation between prior tasks and
their completed results. This is a closer match to`concurrent.futures.as_completed()` âs iteration pattern. Patch by
Justin Arthur.
- bpo-27578: `inspect.getsource()` (and related functions) work with
empty module files, returning`'\n'` (or reasonable equivalent) instead
of raising`OSError` . Patch by Kernc.
- bpo-37141: Accept an iterable of separators in `asyncio.StreamReader.readuntil()` , stopping when one of them is
encountered.
- gh-66543: Make `mimetypes.guess_type()` properly parsing of URLs with
only a host name, URLs containing fragment or query, and filenames with
only a UNC sharepoint on Windows. Based on patch by Dong-hee Na.
- bpo-15010: `unittest.TestLoader.discover()` now saves the original
value of`unittest.TestLoader._top_level_dir` and restores it at the end
of the call.

### DocumentationÂ¶

### TestsÂ¶

- gh-83434: Disable JUnit XML output ( `--junit-xml=FILE` command line
option) in regrtest when hunting for reference leaks (`-R` option).
Patch by Victor Stinner.
- gh-117187: Fix XML tests for vanilla Expat <2.6.0.
- gh-116333: Tests of TLS related things (error codes, etc) were updated to be more lenient about specific error message strings and behaviors as seen in the BoringSSL and AWS-LC forks of OpenSSL.
- gh-117089: Consolidated tests for importlib.metadata in their own `metadata` package.
- gh-115979: Update test_importlib so that it passes under WASI SDK 21.
- gh-112536: Add âtsan to test.regrtest for running TSAN tests in reasonable execution times. Patch by Donghee Na.
- gh-116307: Added import helper `isolated_modules` as`CleanImport` does not remove modules imported during the context. Use it in
importlib.resources tests to avoid leaving`mod` around to impede
importlib.metadata tests.

### BuildÂ¶

### WindowsÂ¶

- gh-117267: Ensure `DirEntry.stat().st_ctime` behaves consistently with`os.stat()` during the deprecation period of`st_ctime` by
containing the same value as`st_birthtime` . After the deprecation
period,`st_ctime` will be the metadata change time (or unavailable
through`DirEntry` ), and only`st_birthtime` will contain the creation
time.
- gh-116195: Improves performance of `os.getppid()` by using an
alternate system API when available. Contributed by vxiiduu.
- gh-88494: On Windows, `time.monotonic()` now uses the`QueryPerformanceCounter()` clock to have a resolution better than 1 us,
instead of the`GetTickCount64()` clock which has a resolution of 15.6
ms. Patch by Victor Stinner.
- gh-116773: Fix instances of ```
<_overlapped.Overlapped object at 0xXXX>
still has pending operation at deallocation, the process may crash
```
.
- gh-91227: Fix the asyncio ProactorEventLoop implementation so that sending a datagram to an address that is not listening does not prevent receiving any more datagrams.
- gh-115119: Switched from vendored `libmpdecimal` code to a
separately-hosted external package in the`cpython-source-deps` repository when building the`_decimal` module.

### C APIÂ¶

- gh-117642: Fix **PEP 737** implementation for`%#T` and`%#N` .
- gh-87193: `_PyBytes_Resize()` can now be called for bytes objects
with reference count > 1, including 1-byte bytes objects. It creates a new
bytes object and destroys the old one if it has reference count > 1.
- gh-117021: Fix integer overflow in `PyLong_AsPid()` on non-Windows
64-bit platforms.
- gh-115756: `PyCode_GetFirstFree()` is an ustable API now and has
been renamed to`PyUnstable_Code_GetFirstFree()` . (Contributed by
Bogdan Romanyuk in gh-115781)
- gh-116869: Add `test_cext` test: build a C extension to check if the
Python C API emits C compiler warnings. Patch by Victor Stinner.
- gh-116869: Make the C API compatible with `-Werror=declaration-after-statement` compiler flag again. Patch by
Victor Stinner.
- gh-116936: Add `PyType_GetModuleByDef()` to the limited C API. Patch
by Victor Stinner.
- gh-116809: Restore removed private `_PyErr_ChainExceptions1()` function.
Patch by Victor Stinner.
- gh-115754: In the limited C API version 3.13, getting `Py_None` ,`Py_False` ,`Py_True` ,`Py_Ellipsis` and`Py_NotImplemented` singletons is now implemented as function calls at the stable ABI level to
hide implementation details. Getting these constants still return borrowed
references. Patch by Victor Stinner.
- gh-115754: Add `Py_GetConstant()` and`Py_GetConstantBorrowed()` functions to get constants. For example,`Py_GetConstant(Py_CONSTANT_ZERO)` returns a strong reference to
the constant zero. Patch by Victor Stinner.
- gh-111696: Add support for `%T` ,`%T#` ,`%N` and`%N#` formats to`PyUnicode_FromFormat()` : format the fully qualified name of an
object type and of a type: call`PyType_GetModuleName()` . See**PEP 737** for more information. Patch by Victor Stinner.
- gh-111696: Add `PyType_GetModuleName()` function to get the typeâs
module name. Equivalent to getting the`type.__module__` attribute.
Patch by Eric Snow and Victor Stinner.
- gh-111696: Add `PyType_GetFullyQualifiedName()` function to get the
typeâs fully qualified name. Equivalent to`f"{type.__module__}.{type.__qualname__}"` , or`type.__qualname__` if`type.__module__` is not a string or is equal to`"builtins"` . Patch
by Victor Stinner.
- gh-85283: The `fcntl` ,`grp` ,`pwd` ,`termios` ,`_statistics` and`_testconsole` C extensions are now built with the limited C
API. Patch by Victor Stinner.
- gh-111140: Add additional flags to `PyLong_AsNativeBytes()` and`PyLong_FromNativeBytes()` to allow the caller to determine how to
handle edge cases around values that fill the entire buffer.
- gh-113024: Add `PyObject_GenericHash()` function.

## Python 3.13.0 alpha 5Â¶

*Release date: 2024-03-12*
