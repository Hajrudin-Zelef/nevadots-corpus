---
id: collect-261121-veille-ia/docs-techniques/changelog-2-12
title: "Changelog"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["patch", "objects", "types", "pablo", "galindo", "union", "python", "object"]
source: https://docs.python.org/release/3.15.1/whatsnew/changelog.html
source_anchor: ""
source_lines: [16501, 18000]
section: "Docs_Techniques"
---
- bpo-44589: Mapping patterns in `match` statements with two or more equal
literal keys will now raise a`SyntaxError` at compile-time.
- bpo-44606: Fix `__instancecheck__` and`__subclasscheck__` for the
union type.
- bpo-42073: The `@classmethod` decorator can now wrap other
classmethod-like descriptors.
- bpo-41972: Tuned the string-searching algorithm of fastsearch.h to have a shorter inner loop for most cases.
- bpo-44590: All necessary data for executing a Python function (local variables, stack, etc) is now kept in a per-thread stack. Frame objects are lazily allocated on demand. This increases performance by about 7% on the standard benchmark suite. Introspection and debugging are unaffected as frame objects are always available when needed. Patch by Mark Shannon.
- bpo-44584: The threading debug ( `PYTHONTHREADDEBUG` environment
variable) is deprecated in Python 3.10 and will be removed in Python 3.12.
This feature requires a debug build of Python. Patch by Victor Stinner.
- bpo-43895: An obsolete internal cache of shared object file handles added in 1995 that attempted, but did not guarantee, that a .so would not be dlopenâed twice to work around flaws in mid-1990s posix-ish operating systems has been removed from dynload_shlib.c.

### LibraryÂ¶

### Core and BuiltinsÂ¶

- bpo-43950: Code objects can now provide the column information for instructions when available. This is levaraged during traceback printing to show the expressions responsible for errors. Contributed by Pablo Galindo, Batuhan Taskaya and Ammar Askar as part of **PEP 657** .
- bpo-44562: Remove uses of `PyObject_GC_Del()` in error path when
initializing`types.GenericAlias` .

### LibraryÂ¶

- bpo-41486: Fix a memory consumption and copying performance regression in earlier 3.10 beta releases if someone used an output buffer larger than 4GiB with zlib.decompress on input data that expands that large.

### Core and BuiltinsÂ¶

- bpo-43908: Heap types with the `Py_TPFLAGS_IMMUTABLETYPE` flag
can now inherit the**PEP 590** vectorcall protocol.  Previously, this was
only possible for static types.  Patch by Erlend E.
Aasland.
- bpo-44553: Implement GC methods for `types.Union` to break reference
cycles and prevent memory leaks.
- bpo-44490: Add `__parameters__` attribute and`__getitem__` operator
to`types.Union` . Patch provided by Yurii Karabas.
- bpo-44523: Remove the pass-through for `hash()` of`weakref.proxy` objects to prevent unintended consequences when the
original referred object dies while the proxy is part of a hashable
object. Patch by Pablo Galindo.
- bpo-44483: Fix a crash in `types.Union` objects when creating a union of
an object with bad`__module__` field.
- bpo-44486: Modules will always have a dictionary, even when created by `types.ModuleType.__new__()`
- bpo-44472: Fix ltrace functionality when exceptions are raised. Patch by Pablo Galindo
- bpo-12022: A `TypeError` is now raised instead of an`AttributeError` in`with` and`async with` statements for objects which do not support the context manager or
asynchronous context manager protocols correspondingly.
- bpo-44297: Make sure that the line number is set when entering a comprehension scope. This ensures that backtraces including generator expressions show the correct line number.
- bpo-44456: Improve the syntax error when mixing positional and keyword patterns. Patch by Pablo Galindo.
- bpo-44409: Fix error location information for tokenizer errors raised on initialization of the tokenizer. Patch by Pablo Galindo.
- bpo-44396: Fix a possible crash in the tokenizer when raising syntax errors for unclosed strings. Patch by Pablo Galindo.
- bpo-44376: Exact integer exponentiation (like `i**2` or`pow(i, 2)` )
with a small exponent is much faster, due to reducing overhead in such
cases.
- bpo-44313: Directly imported objects and modules (through import and from import statements) donât generate `LOAD_METHOD` /`CALL_METHOD` for
directly accessed objects on their namespace. They now use the regular`LOAD_ATTR` /`CALL_FUNCTION` .
- bpo-44338: Implement adaptive specialization for LOAD_GLOBAL Two specialized forms of LOAD_GLOBAL are added: 
  - LOAD_GLOBAL_MODULE
  - LOAD_GLOBAL_BUILTIN
- bpo-44368: Improve syntax errors for invalid âasâ targets. Patch by Pablo Galindo
- bpo-44349: Fix an edge case when displaying text from files with encoding in syntax errors. Patch by Pablo Galindo.
- bpo-44337: Initial implementation of adaptive specialization of LOAD_ATTR Four specialized forms of LOAD_ATTR are added: 
  - LOAD_ATTR_SLOT
  - LOAD_ATTR_SPLIT_KEYS
  - LOAD_ATTR_WITH_HINT
  - LOAD_ATTR_MODULE
- bpo-44335: Fix a regression when identifying incorrect characters in syntax errors. Patch by Pablo Galindo
- bpo-43693: Computation of the offsets of cell variables is done in the compiler instead of at runtime. This reduces the overhead of handling cell and free variables, especially in the case where a variable is both an argument and cell variable.
- bpo-44317: Improve tokenizer error with improved locations. Patch by Pablo Galindo.

### LibraryÂ¶

### Core and BuiltinsÂ¶

- bpo-44305: Improve error message for `try` blocks without`except` or`finally` blocks. Patch by Pablo Galindo.
- bpo-43413: Constructors of subclasses of some builtin classes (e.g. `tuple` ,`list` ,`frozenset` ) no longer accept
arbitrary keyword arguments. [reverted in 3.11a4] Subclass of`set` can now define a`__new__()` method with additional keyword parameters
without overriding also`__init__()` .
- bpo-43667: Improve Unicode support in non-UTF locales on Oracle Solaris. This issue does not affect other Solaris systems.
- bpo-43693: A new opcode MAKE_CELL has been added that effectively moves some of the work done on function entry into the compiler and into the eval loop. In addition to creating the required cell objects, the new opcode converts relevant arguments (and other locals) to cell variables on function entry.
- bpo-44232: Fix a regression in `type()` when a metaclass raises an
exception. The C function`type_new()` must properly report the
exception when a metaclass constructor raises an exception and the winner
class is not the metaclass. Patch by Victor Stinner.
- bpo-44201: Avoid side effects of checking for specialized syntax errors in the REPL that was causing it to ask for extra tokens after a syntax error had been detected. Patch by Pablo Galindo
- bpo-43693: `PyCodeObject` gained`co_fastlocalnames` and`co_fastlocalkinds` as the authoritative source of fast locals info.
Marshaled code objects have changed accordingly.
- bpo-44184: Fix a crash at Python exit when a deallocator function removes the last strong reference to a heap type. Patch by Victor Stinner.
- bpo-44187: Implement quickening in the interpreter. This offers no advantages as yet, but is an enabler of future optimizations. See PEP 659 for full explanation.
- bpo-44180: The parser doesnât report generic syntax errors that happen in a position further away that the one it reached in the first pass. Patch by Pablo Galindo
- bpo-44168: Fix error message in the parser involving keyword arguments with invalid expressions. Patch by Pablo Galindo
- bpo-44156: String caches in `compile.c` are now subinterpreter
compatible.
- bpo-44143: Fixed a crash in the parser that manifest when raising tokenizer errors when an existing exception was present. Patch by Pablo Galindo.
- bpo-44032: Move âfastâ locals and other variables from the frame object to a per-thread datastack.
- bpo-44114: Fix incorrect dictkeys_reversed and dictitems_reversed function signatures in C code, which broke webassembly builds.
- bpo-44110: Improve `str.__getitem__()` error message
- bpo-26110: Add `CALL_METHOD_KW` opcode to speed up method calls with
keyword arguments.  Idea originated from PyPy.  A side effect is executing`CALL_METHOD` is now branchless in the evaluation loop.
- bpo-28307: Compiler now optimizes simple C-style formatting with literal format containing only format codes %s, %r and %a by converting them to f-string expressions.
- bpo-43149: Correct the syntax error message regarding multiple exception types to not refer to âexception groupsâ. Patch by Pablo Galindo
- bpo-43822: The parser will prioritize tokenizer errors over custom syntax errors when raising exceptions. Patch by Pablo Galindo.
- bpo-40222: âZero costâ exception handling. 
  - Uses a lookup table to determine how to handle exceptions.
  - Removes SETUP_FINALLY and POP_TOP block instructions, eliminating the runtime overhead of try statements.
  - Reduces the size of the frame object by about 60%.
 Patch by Mark Shannon
- bpo-43918: Document the signature and `default` argument in the
docstring of the new`anext` builtin.
- bpo-43833: Emit a deprecation warning if the numeric literal is immediately followed by one of keywords: and, else, for, if, in, is, or. Raise a syntax error with more informative message if it is immediately followed by other keyword or identifier.
- bpo-43879: Add native_thread_id to PyThreadState. Patch by Gabriele N. Tornetta.
- bpo-43693: Compute cell offsets relative to locals in compiler. Allows the interpreter to treats locals and cells a single array, which is slightly more efficient. Also make the LOAD_CLOSURE opcode an alias for LOAD_FAST. Preserving LOAD_CLOSURE helps keep bytecode a bit more readable.
- bpo-17792: More accurate error messages for access of unbound locals or free vars.
- bpo-28146: Fix a confusing error message in `str.format()` .
- bpo-11105: When compiling `ast.AST` objects with recursive
references through`compile()` , the interpreter doesnât crash anymore
instead it raises a`RecursionError` .
- bpo-39091: Fix crash when using passing a non-exception to a generatorâs `throw()` method. Patch by Noah Oxer
- bpo-33346: Asynchronous comprehensions are now allowed inside comprehensions in asynchronous functions. Outer comprehensions implicitly become asynchronous.

### LibraryÂ¶

- bpo-45371: Fix clang rpath issue in `distutils` . The UnixCCompiler now
uses correct clang option to add a runtime library directory (rpath) to a
shared library.
- bpo-45329: Fix freed memory access in `pyexpat.xmlparser` when
building it with an installed expat library <= 2.2.0.
- bpo-41710: On Unix, if the `sem_clockwait()` function is available in
the C library (glibc 2.30 and newer), the`threading.Lock.acquire()` method now uses the monotonic clock (`time.CLOCK_MONOTONIC` ) for
the timeout, rather than using the system clock
(`time.CLOCK_REALTIME` ), to not be affected by system clock
changes. Patch by Victor Stinner.
- bpo-1596321: Fix the `threading._shutdown()` function when the`threading` module was imported first from a thread different than
the main thread: no longer log an error at Python exit.
- bpo-45274: Fix a race condition in the `Thread.join()` method of the`threading` module. If the
function is interrupted by a signal and the signal handler raises an
exception, make sure that the thread remains in a consistent state to
prevent a deadlock. Patch by Victor Stinner.
- bpo-21302: In Unix operating systems, `time.sleep()` now uses the`nanosleep()` function, if`clock_nanosleep()` is not available but`nanosleep()` is available.`nanosleep()` allows to sleep with
nanosecond precision.
- bpo-21302: On Windows, `time.sleep()` now uses a waitable timer which
has a resolution of 100 nanoseconds (10<sup>-7</sup> seconds). Previously,
it had a resolution of 1 millisecond (10<sup>-3</sup> seconds). Patch by
Benjamin SzÅke and Victor Stinner.
- bpo-45238: Fix `unittest.IsolatedAsyncioTestCase.debug()` : it runs now
asynchronous methods and callbacks.
- bpo-36674: `unittest.TestCase.debug()` raises now a`unittest.SkipTest` if the class or the test method are decorated
with the skipping decorator.
- bpo-45235: Fix an issue where argparse would not preserve values in a provided namespace when using a subparser with defaults.
- bpo-45183: Have zipimport.zipimporter.find_spec() not raise an exception when the underlying zip file has been deleted and the internal cache has been reset via invalidate_cache().
- bpo-45234: Fixed a regression in `copyfile()` ,`copy()` ,`copy2()` raising`FileNotFoundError` when source is a directory, which should raise`IsADirectoryError`
- bpo-45228: Fix stack buffer overflow in parsing J1939 network address.
- bpo-45225: use map function instead of genexpr in capwords.
- bpo-42135: Fix typo: `importlib.find_loader` is really slated for
removal in Python 3.12 not 3.10, like the others in PR 25169.Patch by Hugo van Kemenade.
- bpo-20524: Improves error messages on `.format()` operation for`str` ,`float` ,`int` , and`complex` . New format now shows the problematic
pattern and the object type.
- bpo-45168: Change `dis.dis()` output to omit op arg values that cannot
be resolved due to`co_consts` ,`co_names` etc not being provided.
Previously the oparg itself was repeated in the value field, which is not
useful and can be confusing.
- bpo-21302: In Unix operating systems, `time.sleep()` now uses the`clock_nanosleep()` function, if available, which allows to sleep for an
interval specified with nanosecond precision.
- bpo-45173: Remove from the `configparser` module: the`SafeConfigParser` class, the`filename` property of the`ParsingError` class, the`readfp()` method of
the`ConfigParser` class, deprecated since Python
3.2.Patch by Hugo van Kemenade.
- bpo-44987: Pure ASCII strings are now normalized in constant time by `unicodedata.normalize()` . Patch by Donghee Na.
- bpo-35474: Calling `mimetypes.guess_all_extensions()` with`strict=False` no longer affects the result of the following call with`strict=True` . Also, mutating the returned list no longer affects the
global state.
- bpo-45166: `typing.get_type_hints()` now works with`Final` wrapped in`ForwardRef` .
- bpo-45162: Remove many old deprecated `unittest` features:
  - â `fail*` â and â`assert*` â aliases of`TestCase` methods.
  - Broken from start `TestCase` method`assertDictContainsSubset()` .
  - Ignored `<unittest.TestLoader.loadTestsFromModule> TestLoader.loadTestsFromModule()` parameter*use_load_tests* .
  - Old alias `_TextTestResult` of`TextTestResult` .
- bpo-38371: Remove the deprecated `split()` method of`_tkinter.TkappType` . Patch by Erlend E. Aasland.
- bpo-20499: Improve the speed and accuracy of statistics.pvariance().
- bpo-45132: Remove `__getitem__()` methods of`xml.dom.pulldom.DOMEventStream` ,`wsgiref.util.FileWrapper` and`fileinput.FileInput` , deprecated since Python 3.9.Patch by Hugo van Kemenade.
- bpo-45129: Due to significant security concerns, the *reuse_address* parameter of`asyncio.loop.create_datagram_endpoint()` , disabled in
Python 3.9, is now entirely removed. This is because of the behavior of
the socket option`SO_REUSEADDR` in UDP.Patch by Hugo van Kemenade.
- bpo-45124: The `bdist_msi` command, deprecated in Python 3.9, is now
removed.Use `bdist_wheel` (wheel packages) instead.Patch by Hugo van Kemenade.
- bpo-30856: `unittest.TestResult` methods`addFailure()` ,`addError()` ,`addSkip()` and`addSubTest()` are now called immediately after
raising an exception in test or finishing a subtest. Previously they were
called only after finishing the test clean up.
- bpo-45034: Changes how error is formatted for `struct.pack` with`'H'` and`'h'` modes and too large / small numbers. Now it shows the actual
numeric limits, while previously it was showing arithmetic expressions.
- bpo-25894: `unittest` now always reports skipped and failed subtests
separately: separate characters in default mode and separate lines in
verbose mode. Also the test description is now output for errors in test
method, class and module cleanups.
- bpo-45081: Fix issue when dataclasses that inherit from `typing.Protocol` subclasses have wrong`__init__` . Patch provided by
Yurii Karabas.
- bpo-45085: The `binhex` module, deprecated in Python 3.9, is now
removed. The following`binascii` functions, deprecated in Python
3.9, are now also removed:
  - `a2b_hqx()` ,`b2a_hqx()` ;
  - `rlecode_hqx()` ,`rledecode_hqx()` .
 The `binascii.crc_hqx()` function remains available.Patch by Victor Stinner.
- bpo-40360: The `lib2to3` package is now deprecated and may not be
able to parse Python 3.10 or newer. See the**PEP 617** (New PEG parser for
CPython). Patch by Victor Stinner.
- bpo-45075: Rename `traceback.StackSummary.format_frame()` to`traceback.StackSummary.format_frame_summary()` . This method was added
for 3.11 so it was not released yet.Updated code and docs to better distinguish frame and FrameSummary.
- bpo-31299: Add option to completely drop frames from a traceback by returning `None` from a`format_frame()` override.
- bpo-41620: `run()` now always return a`TestResult` instance. Previously it returned`None` if
the test class or method was decorated with a skipping decorator.
- bpo-45021: Fix a potential deadlock at shutdown of forked children when using `concurrent.futures` module
- bpo-43913: Fix bugs in cleaning up classes and modules in `unittest` :
  - Functions registered with `addModuleCleanup()` were not called unless the user defines`tearDownModule()` in their test module.
  - Functions registered with `addClassCleanup()` were not called if`tearDownClass` is set to`None` .
  - Buffering in `TestResult` did not work with functions registered with`addClassCleanup()` and`addModuleCleanup()` .
  - Errors in functions registered with `addClassCleanup()` and`addModuleCleanup()` were not handled correctly in buffered and debug modes.
  - Errors in `setUpModule()` and functions registered with`addModuleCleanup()` were reported in wrong order.
  - And several lesser bugs.
- bpo-45030: Fix integer overflow in pickling and copying the range iterator.
- bpo-45001: Made email date parsing more robust against malformed input, namely a whitespace-only `Date:` header. Patch by Wouter Bolsterlee.
- bpo-45010: Remove support of special method `__div__` in`unittest.mock` . It is not used in Python 3.
- bpo-39218: Improve accuracy of variance calculations by using `x*x` instead of`x**2` .
- bpo-43613: Improve the speed of `gzip.compress()` and`gzip.decompress()` by compressing and decompressing at once in memory
instead of in a streamed fashion.
- bpo-37596: Ensure that `set` and`frozenset` objects are
always`marshalled` reproducibly.
- bpo-44019: A new function `operator.call` has been added, such that`operator.call(obj, *args, **kwargs) == obj(*args, **kwargs)` .
- bpo-42255: `webbrowser.MacOSX` is deprecated and will be removed
in Python 3.13. It is untested and undocumented and also not used by`webbrowser` itself. Patch by Donghee Na.
- bpo-44955: Method `stopTestRun()` is now always
called in pair with method`startTestRun()` for`TestResult` objects implicitly created in`run()` . Previously it was not called for test
methods and classes decorated with a skipping decorator.
- bpo-39039: tarfile.open raises `ReadError` when a zlib error
occurs during file extraction.
- bpo-44935: `subprocess` on Solaris now also uses`os.posix_spawn()` for better performance.
- bpo-44911: `IsolatedAsyncioTestCase` will no longer throw
an exception while cancelling leaked tasks. Patch by Bar Harel.
- bpo-41322: Added `DeprecationWarning` for tests and async tests that
return a value!=None (as this may indicate an improperly written test, for
example a test written as a generator function).
- bpo-44524: Make exception message more useful when subclass from typing special form alias. Patch provided by Yurii Karabas.
- bpo-38956: `argparse.BooleanOptionalAction` âs default value is no
longer printed twice when used with`argparse.ArgumentDefaultsHelpFormatter` .
- bpo-44860: Fix the `posix_user` scheme in`sysconfig` to not depend
on`sys.platlibdir` .
- bpo-44859: Improve error handling in `sqlite3` and raise more
accurate exceptions.
  - `MemoryError` is now raised instead of`sqlite3.Warning` when memory is not enough for encoding a statement to UTF-8 in`Connection.__call__()` and`Cursor.execute()` .
  - `UnicodEncodeError` is now raised instead of`sqlite3.Warning` when the statement contains surrogate characters in`Connection.__call__()` and`Cursor.execute()` .
  - `TypeError` is now raised instead of`ValueError` for non-string script argument in`Cursor.executescript()` .
  - `ValueError` is now raised for script containing the null character instead of truncating it in`Cursor.executescript()` .
  - Correctly handle exceptions raised when getting boolean value of the result of the progress handler.
  - Add many tests covering different corner cases.
- bpo-44581: Upgrade bundled pip to 21.2.3 and setuptools to 57.4.0
- bpo-44849: Fix the `os.set_inheritable()` function on FreeBSD 14 for
file descriptor opened with the`O_PATH` flag: ignore the`EBADF` error on`ioctl()` , fallback on the`fcntl()` implementation. Patch by Victor Stinner.
- bpo-44605: The @functools.total_ordering() decorator now works with metaclasses.
- bpo-44524: Fixed an issue wherein the `__name__` and`__qualname__` attributes of subscribed specialforms could be`None` .
- bpo-44839: `MemoryError` raised in user-defined functions will now
produce a`MemoryError` in`sqlite3` .`OverflowError` will
now be converted to`DataError` . Previously`OperationalError` was produced in these cases.
- bpo-44822: `sqlite3` user-defined functions and aggregators returning`strings` with embedded NUL characters are no longer
truncated. Patch by Erlend E. Aasland.
- bpo-44801: Ensure that the `ParamSpec` variable in Callable
can only be substituted with a parameters expression (a list of types, an
ellipsis, ParamSpec or Concatenate).
- bpo-44806: Non-protocol subclasses of `typing.Protocol` ignore now
the`__init__` method inherited from protocol base classes.
- bpo-27275: `collections.OrderedDict.popitem()` and`collections.OrderedDict.pop()` no longer call`__getitem__` and`__delitem__` methods of the OrderedDict subclasses.
- bpo-44793: Fix checking the number of arguments when subscribe a generic type with `ParamSpec` parameter.
- bpo-44784: In importlib.metadata tests, override warnings behavior under expected DeprecationWarnings (importlib_metadata 4.6.3).
- bpo-44667: The `tokenize.tokenize()` doesnât incorrectly generate a`NEWLINE` token if the source doesnât end with a new line character but
the last line is a comment, as the function is already generating a`NL` token. Patch by Pablo Galindo
- bpo-44771: Added `importlib.simple` module implementing adapters from a
low-level resources reader interface to a`TraversableResources` interface. Legacy API (`path` ,`contents` , â¦) is now supported
entirely by the`.files()` API with a compatibility shim supplied for
resource loaders without that functionality. Feature parity with`importlib_resources` 5.2.
- bpo-44752: `rcompleter` does not call`getattr()` on`property` objects to avoid the side-effect of  evaluating the
corresponding method.
- bpo-44747: Refactor usage of `sys._getframe` in`typing` module. Patch
provided by Yurii Karabas.
- bpo-42378: Fixes the issue with log file being overwritten when `logging.FileHandler` is used in`atexit` with*filemode* set
to`'w'` . Note this will cause the message in*atexit* not being logged
if the log stream is already closed due to shutdown of logging.
- bpo-44720: `weakref.proxy` objects referencing non-iterators now raise`TypeError` rather than dereferencing the null`tp_iternext` slot and
crashing.
- bpo-44704: The implementation of `collections.abc.Set._hash()` now
matches that of`frozenset.__hash__()` .
- bpo-44666: Fixed issue in `compileall.compile_file()` when`sys.stdout` is redirected. Patch by Stefan HÃ¶lzl.
- bpo-44688: `sqlite3.Connection.create_collation()` now accepts
non-ASCII collation names. Patch by Erlend E. Aasland.
- bpo-44690: Adopt *binacii.a2b_base64* âs strict mode in*base64.b64decode* .
- bpo-42854: Fixed a bug in the `_ssl` module that was throwing`OverflowError` when using`_ssl._SSLSocket.write()` and`_ssl._SSLSocket.read()` for a big value of the`len` parameter.
Patch by Pablo Galindo.
- bpo-44686: Replace `unittest.mock._importer` with`pkgutil.resolve_name` .
- bpo-44353: Make `NewType.__call__` faster by implementing it in C. Patch
provided by Yurii Karabas.
- bpo-44682: Change the `pdb`*commands* directive to disallow setting
commands for an invalid breakpoint and to display an appropriate error.
- bpo-44353: Refactor `typing.NewType` from function into callable class.
Patch provided by Yurii Karabas.
- bpo-44678: Added a separate error message for discontinuous padding in *binascii.a2b_base64* strict mode.
- bpo-44524: Add missing `__name__` and`__qualname__` attributes to`typing` module classes. Patch provided by Yurii Karabas.
- bpo-40897: Give priority to using the current class constructor in `inspect.signature()` . Patch by Weipeng Hong.
- bpo-44638: Add a reference to the zipp project and hint as to how to use it.
- bpo-44648: Fixed wrong error being thrown by `inspect.getsource()` when examining a class in the interactive session. Instead of`TypeError` , it should be`OSError` with appropriate error
message.
- bpo-44608: Fix memory leak in `_tkinter._flatten()` if it is called
with a sequence or set, but not list or tuple.
- bpo-44594: Fix an edge case of `ExitStack` and`AsyncExitStack` exception chaining.  They will now match`with` block behavior when`__context__` is explicitly set to`None` when the
exception is in flight.
- bpo-42799: In `fnmatch` , the cache size for compiled regex patterns
(`functools.lru_cache()` ) was bumped up from 256 to 32768, affecting
functions:`fnmatch.fnmatch()` ,`fnmatch.fnmatchcase()` ,`fnmatch.filter()` .
- bpo-41928: Update `shutil.copyfile()` to raise`FileNotFoundError` instead of confusing`IsADirectoryError` when a path ending with a`os.path.sep` does not exist;`shutil.copy()` and`shutil.copy2()` are also affected.
- bpo-44569: Added the `StackSummary.format_frame()` function in`traceback` . This allows users to customize the way individual lines
are formatted in tracebacks without re-implementing logic to handle
recursive tracebacks.
- bpo-44566: handle StopIteration subclass raised from @contextlib.contextmanager generator
- bpo-44558: Make the implementation consistency of `indexOf()` between C and Python versions. Patch by Donghee
Na.
- bpo-41249: Fixes `TypedDict` to work with`typing.get_type_hints()` and postponed evaluation of annotations across modules.
- bpo-44554: Refactor argument processing in `pdb.main()` to simplify
detection of errors in input loading and clarify behavior around module or
script invocation.
- bpo-34798: Break up paragraph about `pprint.PrettyPrinter` construction parameters to make it easier to read.
- bpo-44539: Added support for recognizing JPEG files without JFIF or Exif markers.
- bpo-44461: Fix bug with `pdb` âs handling of import error due to a
package which does not have a`__main__` module
- bpo-43625: Fix a bug in the detection of CSV file headers by `csv.Sniffer.has_header()` and improve documentation of same.
- bpo-44516: Update vendored pip to 21.1.3
- bpo-42892: Fixed an exception thrown while parsing a malformed multipart email by `email.message.EmailMessage` .
- bpo-44468: `typing.get_type_hints()` now finds annotations in classes
and base classes with unexpected`__module__` . Previously, it skipped
those MRO elements.
- bpo-44491: Allow clearing the `sqlite3` authorizer callback by
passing`None` to`set_authorizer()` . Patch
by Erlend E. Aasland.
- bpo-43977: Set the proper `Py_TPFLAGS_MAPPING` and`Py_TPFLAGS_SEQUENCE` flags for subclasses created before a
parent has been registered as a`collections.abc.Mapping` or`collections.abc.Sequence` .
- bpo-44482: Fix very unlikely resource leak in `glob` in alternate
Python implementations.
- bpo-44466: The `faulthandler` module now detects if a fatal error
occurs during a garbage collector collection. Patch by Victor Stinner.
- bpo-44471: A `TypeError` is now raised instead of an`AttributeError` in`contextlib.ExitStack.enter_context()` and`contextlib.AsyncExitStack.enter_async_context()` for objects which do
not support the context manager or asynchronous context
manager protocols correspondingly.
- bpo-44404: `tkinter` âs`after()` method now supports callables
without the`__name__` attribute.
- bpo-41546: Make `pprint` (like the builtin`print` ) not attempt to
write to`stdout` when it is`None` .
- bpo-44458: `BUFFER_BLOCK_SIZE` is now declared static, to avoid linking
collisions when bz2, lmza or zlib are statically linked.
- bpo-44464: Remove exception for flake8 in deprecated importlib.metadata interfaces. Sync with importlib_metadata 4.6.
- bpo-44446: Take into account that `lineno` might be`None` in`traceback.FrameSummary` .
- bpo-44439: Fix in `bz2.BZ2File.write()` /`lzma.LZMAFile.write()` methods, when the input data is an object that supports the buffer
protocol, the file length may be wrong.
- bpo-44434: _thread.start_new_thread() no longer calls PyThread_exit_thread() explicitly at the thread exit, the call was redundant. On Linux with the glibc, pthread_exit() aborts the whole process if dlopen() fails to open libgcc_s.so file (ex: EMFILE error). Patch by Victor Stinner.
- bpo-42972: The _thread.RLock type now fully implement the GC protocol: add a traverse function and the `Py_TPFLAGS_HAVE_GC` flag. Patch by
Victor Stinner.
- bpo-44422: The `threading.enumerate()` function now uses a reentrant
lock to prevent a hang on reentrant call. Patch by Victor Stinner.
- bpo-38291: Importing typing.io or typing.re now prints a `DeprecationWarning` .
- bpo-37880: argparse actions store_const and append_const each receive a default value of `None` when the`const` kwarg is not provided.
Previously, this raised a`TypeError` .
- bpo-44389: Fix deprecation of `ssl.OP_NO_TLSv1_3`
- bpo-27827: `pathlib.PureWindowsPath.is_reserved()` now identifies a
greater range of reserved filenames, including those with trailing spaces
or colons.
- bpo-44395: Fix `as_string()` to pass unixfrom
properly. Patch by Donghee Na.
- bpo-34266: Handle exceptions from parsing the arg of `pdb` âs
run/restart command.
- bpo-44362: Improve `ssl` moduleâs deprecation messages, error
reporting, and documentation for deprecations.
- bpo-44342: [Enum] Change pickling from by-value to by-name.
- bpo-44356: [Enum] Allow multiple data-type mixins if they are all the same.
- bpo-44351: Restore back `parse_makefile()` in`distutils.sysconfig` because it behaves differently than the similar implementation in`sysconfig` .
- bpo-35800: `smtpd.MailmanProxy` is now removed as it is unusable
without an external module,`mailman` . Patch by Donghee Na.
- bpo-44357: Added a function that returns cube root of the given number `math.cbrt()`
- bpo-44339: Change `math.pow(Â±0.0, -math.inf)` to return`inf` instead
of raising`ValueError` . This brings the special-case handling of`math.pow` into compliance with the IEEE 754 standard.
- bpo-44242: Remove missing flag check from Enum creation and move into a `verify` decorator.
- bpo-44246: In `importlib.metadata` , restore compatibility in the result
from`Distribution.entry_points` (`EntryPoints` ) to honor expectations
in older implementations and issuing deprecation warnings for these cases:
A.`EntryPoints` objects are once again mutable, allowing   for`sort()` and other list-based mutation operations.   Avoid deprecation
warnings by casting to a   mutable sequence (e.g.`list(dist.entry_points).sort()` ). B.`EntryPoints` results once again
allow   for access by index. To avoid deprecation warnings,   cast the
result to a Sequence first   (e.g.`tuple(dist.entry_points)[0]` ).
- bpo-44246: In importlib.metadata.entry_points, de-duplication of distributions no longer requires loading the full metadata for PathDistribution objects, improving entry point loading performance by ~10x.
- bpo-43858: Added a function that returns a copy of a dict of logging levels: `logging.getLevelNamesMapping()`
- bpo-44260: The `random.Random` constructor no longer reads system
entropy without need.
- bpo-44254: On Mac, give turtledemo button text a color that works on both light or dark background. Programmers cannot control the latter.
- bpo-44258: Support PEP 515 for Fractionâs initialization from string.
- bpo-44235: Remove deprecated functions in the `gettext` . Patch by
Donghee Na.
- bpo-38693: Prefer f-strings to `.format` in importlib.resources.
- bpo-33693: Importlib.metadata now prefers f-strings to .format.
- bpo-44241: Incorporate minor tweaks from importlib_metadata 4.1: SimplePath protocol, support for Metadata 2.2.
- bpo-43216: Remove the `@asyncio.coroutine` decorator enabling
legacy generator-based coroutines to be compatible with async/await code;
remove`asyncio.coroutines.CoroWrapper` used for wrapping legacy
coroutine objects in the debug mode. The decorator has been deprecated
since Python 3.8 and the removal was initially scheduled for Python 3.10.
Patch by Illia Volochii.
- bpo-44210: Make importlib.metadata._meta.PackageMetadata public.
- bpo-43643: Declare readers.MultiplexedPath.name as a property per the spec.
- bpo-27334: The `sqlite3` context manager now performs a rollback
(thus releasing the database lock) if commit failed.  Patch by Luca Citi
and Erlend E. Aasland.
- bpo-4928: Documented existing behavior on POSIX: NamedTemporaryFiles are not deleted when creating process is killed with SIGKILL
- bpo-44154: Optimize `fractions.Fraction` pickling for large
components.
- bpo-33433: For IPv4 mapped IPv6 addresses ( **RFC 4291** Section 2.5.5.2),
the`ipaddress.IPv6Address.is_private` check is deferred to the
mapped IPv4 address. This solves a bug where public mapped IPv4 addresses
were considered private by the IPv6 check.
- bpo-44150: Add optional *weights* argument to statistics.fmean().
- bpo-44142: `ast.unparse()` will now drop the redundant parentheses
when tuples used as assignment targets (e.g in for loops).
- bpo-44145: `hmac` computations were not releasing the GIL while
calling the OpenSSL`HMAC_Update` C API (a new feature in 3.9).  This
unintentionally prevented parallel computation as other`hashlib` algorithms support.
- bpo-44095: `zipfile.Path` now supports`zipfile.Path.stem` ,`zipfile.Path.suffixes` , and`zipfile.Path.suffix` attributes.
- bpo-44077: Itâs now possible to receive the type of service (ToS), a.k.a. differentiated services (DS), a.k.a. differentiated services code point (DSCP) and explicit congestion notification (ECN) IP header fields with `socket.IP_RECVTOS` .
- bpo-37788: Fix a reference leak when a Thread object is never joined.
- bpo-38908: Subclasses of `typing.Protocol` which only have data
variables declared will now raise a`TypeError` when checked with`isinstance` unless they are decorated with`runtime_checkable()` .
Previously, these checks passed silently. Patch provided by Yurii Karabas.
- bpo-44098: `typing.ParamSpec` will no longer be found in the`__parameters__` of most`typing` generics except in valid use
locations specified by**PEP 612** . This prevents incorrect usage like`typing.List[P][int]` . This change means incorrect usage which may have
passed silently in 3.10 beta 1 and earlier will now error.
- bpo-44089: Allow subclassing `csv.Error` in 3.10 (it was allowed in 3.9
and earlier but was disallowed in early versions of 3.10).
- bpo-44081: `ast.unparse()` now doesnât use redundant spaces to
separate`lambda` and the`:` if there are no parameters.
- bpo-44061: Fix regression in previous release when calling `pkgutil.iter_modules()` with a list of`pathlib.Path` objects
- bpo-44059: Register the SerenityOS Browser in the `webbrowser` module.
- bpo-36515: The `hashlib` module no longer does unaligned memory
accesses when compiled for ARM platforms.
- bpo-40465: Remove random module features deprecated in Python 3.9.
- bpo-44018: random.seed() no longer mutates bytearray inputs.
- bpo-38352: Add `IO` ,`BinaryIO` ,`TextIO` ,`Match` , and`Pattern` to`typing.__all__` . Patch by Jelle Zijlstra.
- bpo-44002: `urllib.parse` now uses`functool.lru_cache()` for its
internal URL splitting and quoting caches instead of rolling its own like
its the â90s.The undocumented internal `urllib.parse``Quoted` class API is now
deprecated, for removal in 3.14.
- bpo-43972: When `http.server.SimpleHTTPRequestHandler` sends a`301 (Moved Permanently)` for a directory path not ending with`/` ,
add a`Content-Length: 0` header. This improves the behavior for certain
clients.
- bpo-28528: Fix a bug in `pdb` where`checkline()` raises`AttributeError` if it is called after`reset()` .
- bpo-43853: Improved string handling for `sqlite3` user-defined
functions and aggregates:
  - It is now possible to pass strings with embedded null characters to UDFs
  - Conversion failures now correctly raise `MemoryError`
 Patch by Erlend E. Aasland.
- bpo-43666: AIX: `Lib/_aix_support.get_platform()` may fail in an AIX
WPAR. The fileset bos.rte appears to have a builddate in both LPAR and
WPAR so this fileset is queried rather than bos.mp64. To prevent a similar
situation (no builddate in ODM) a value (9988) sufficient for completing a
build is provided. Patch by M Felt.
- bpo-43650: Fix `MemoryError` in`shutil.unpack_archive()` which
fails inside`shutil._unpack_zipfile()` on large files. Patch by Igor
Bolshakov.
- bpo-43612: `zlib.compress()` now accepts a wbits parameter which
allows users to compress data as a raw deflate block without zlib headers
and trailers in one go. Previously this required instantiating a`zlib.compressobj` . It also provides a faster alternative to`gzip.compress` when wbits=31 is used.
- bpo-43392: `importlib._bootstrap._find_and_load()` now implements a
two-step check to avoid locking when modules have been already imported
and are ready. This improves performance of repeated calls to`importlib.import_module()` and`importlib.__import__()` .
- bpo-43318: Fix a bug where `pdb` does not always echo cleared
breakpoints.
- bpo-43234: Prohibit passing non- `concurrent.futures.ThreadPoolExecutor` executors to`loop.set_default_executor()` following a deprecation in Python 3.8.
Patch by Illia Volochii.
- bpo-43232: Prohibit previously deprecated potentially disruptive operations on `asyncio.trsock.TransportSocket` . Patch by Illia
Volochii.
- bpo-30077: Added support for Appleâs aifc/sowt pseudo-compression
- bpo-42971: Add definition of `errno.EQFULL` for platforms that define
this constant (such as macOS).
- bpo-43086: Added a new optional `strict_mode` parameter to*binascii.a2b_base64* . When`scrict_mode` is set to`True` ,
the*a2b_base64* function will accept only valid base64 content. More
details about what âvalid base64 contentâ is, can be found in the
functionâs documentation.
- bpo-43024: Improve the help signature of `traceback.print_exception()` ,`traceback.format_exception()` and`traceback.format_exception_only()` .
- bpo-33809: Add the `traceback.TracebackException.print()` method which
prints the formatted exception information.
- bpo-42862: `sqlite3` now utilizes`functools.lru_cache()` to
implement the connection statement cache. As a small optimisation, the
default statement cache size has been increased from 100 to 128. Patch by
Erlend E. Aasland.
- bpo-41818: Soumendra Ganguly: add termios.tcgetwinsize(), termios.tcsetwinsize().
- bpo-40497: `subprocess.check_output()` now raises`ValueError` when the invalid keyword argument*check* is passed by user code.
Previously such use would fail later with a`TypeError` . Patch by
RÃ©mi Lapeyre.
- bpo-37449: `ensurepip` now uses`importlib.resources.files()` traversable APIs
- bpo-40956: Use Argument Clinic in `sqlite3` .  Patches by Erlend E.
Aasland.
- bpo-41730: `DeprecationWarning` is now raised when importing`tkinter.tix` , which has been deprecated in documentation since
Python 3.6.
- bpo-20684: Remove unused `_signature_get_bound_param` function from`inspect` - by Anthony Sottile.
- bpo-41402: Fix `email.message.EmailMessage.set_content()` when called
with binary data and`7bit` content transfer encoding.
- bpo-32695: The *compresslevel* and*preset* keyword arguments of`tarfile.open()` are now both documented and tested.
- bpo-41137: Use utf-8 encoding while reading .pdbrc files. Patch by Srinivas Reddy Thatiparthy
- bpo-24391: Improved reprs of `threading` synchronization objects:`Semaphore` ,`BoundedSemaphore` ,`Event` and`Barrier` .
- bpo-5846: Deprecated the following `unittest` functions, scheduled
for removal in Python 3.13:
  - `findTestCases()`
  - `makeSuite()`
  - `getTestCaseNames()`
 Use `TestLoader` methods instead:
  - `unittest.TestLoader.loadTestsFromModule()`
  - `unittest.TestLoader.loadTestsFromTestCase()`
  - `unittest.TestLoader.getTestCaseNames()`
 Patch by Erlend E. Aasland.
- bpo-40563: Support pathlike objects on dbm/shelve. Patch by Hakan Ãelik and Henry-Joseph AudÃ©oud.
- bpo-34990: Fixed a Y2k38 bug in the compileall module where it would fail to compile files with a modification time after the year 2038.
- bpo-39549: Whereas the code for reprlib.Repr had previously used a hardcoded string value of ââ¦â, this PR updates it to use of a âfillvalueâ attribute, whose value defaults to ââ¦â and can be reset in either individual reprlib.Repr instances or in subclasses thereof.
- bpo-37022: `pdb` now displays exceptions from`repr()` with its`p` and`pp` commands.
- bpo-38840: Fix `test___all__` on platforms lacking a shared memory
implementation.
- bpo-39359: Add one missing check that the password is a bytes object for an encrypted zipfile.
- bpo-38741: `configparser` : using â]â inside a section header will no
longer cut the section name short at the â]â
- bpo-38415: Added missing behavior to `contextlib.asynccontextmanager()` to match`contextlib.contextmanager()` so decorated functions can themselves be
decorators.
- bpo-30256: Pass multiprocessing BaseProxy argument `manager_owned` through AutoProxy.
- bpo-27513: `email.utils.getaddresses()` now accepts`email.header.Header` objects along with string values. Patch by
Zackery Spytz.
- bpo-16379: Add SQLite error code and name to `sqlite3` exceptions.
Patch by Aviv Palivoda, Daniel Shahaf, and Erlend E. Aasland.
- bpo-26228: pty.spawn no longer hangs on FreeBSD, macOS, and Solaris.
- bpo-33349: lib2to3 now recognizes async generators everywhere.
- bpo-29298: Fix `TypeError` when required subparsers without`dest` do
not receive arguments. Patch by Anthony Sottile.

### DocumentationÂ¶

- bpo-45216: Remove extra documentation listing methods in `difflib` . It
was rendering twice in pydoc and was outdated in some places.
- bpo-45024: `collections.abc` documentation has been expanded to
explicitly cover how instance and subclass checks work, with additional
doctest examples and an exhaustive list of ABCs which test membership
purely by presence of the right special methods. Patch by Raymond
Hettinger.
- bpo-44957: Promote PEP 604 union syntax by using it where possible. Also, mention `X | Y` more prominently in section about`Union` and mention`X | None` at all in section about`Optional` .
- bpo-16580: Added code equivalents for the `int.to_bytes()` and`int.from_bytes()` methods, as well as tests ensuring that these code
equivalents are valid.
- bpo-44903: Removed the `othergui.rst` file, any references to it, and
the list of GUI frameworks in the FAQ. In their place Iâve added links to
the Python Wiki page on GUI frameworks.
- bpo-33479: Tkinter documentation has been greatly expanded with new âArchitectureâ and âThreading modelâ sections.
- bpo-36700: `base64` RFC references were updated to point to**RFC 4648** ; a section was added to point users to the new âsecurity
considerationsâ section of the RFC.
- bpo-44740: Replaced occurrences of uppercase âWebâ and âInternetâ with lowercase versions per the 2016 revised Associated Press Style Book.
- bpo-44693: Update the definition of __future__ in the glossary by replacing the confusing word âpseudo-moduleâ with a more accurate description.
- bpo-35183: Add typical examples to os.path.splitext docs
- bpo-30511: Clarify that `shutil.make_archive()` is not thread-safe due
to reliance on changing the current working directory.
- bpo-44561: Update of three expired hyperlinks in Doc/distributing/index.rst: âProject structureâ, âBuilding and packaging the projectâ, and âUploading the project to the Python Packaging Indexâ.
- bpo-44651: Delete entry âcoercionâ in Doc/glossary.rst for its outdated definition.
- bpo-42958: Updated the docstring and docs of `filecmp.cmp()` to be
more accurate and less confusing especially in respect to*shallow* arg.
- bpo-44631: Refactored the `repr()` code of the`_Environ` (os module).
- bpo-44613: importlib.metadata is no longer provisional.
- bpo-44558: Match the docstring and python implementation of `countOf()` to the behavior of its c implementation.
- bpo-44544: List all kwargs for `textwrap.wrap()` ,`textwrap.fill()` , and`textwrap.shorten()` . Now, there are nav
links to attributes of`TextWrap` , which makes navigation much
easier while minimizing duplication in the documentation.
- bpo-38062: Clarify that atexit uses equality comparisons internally.
- bpo-40620: Convert examples in tutorial controlflow.rst section 4.3 to be interpreter-demo style.
- bpo-43066: Added a warning to `zipfile` docs: filename arg with a
leading slash may cause archive to be un-openable on Windows systems.
- bpo-39452: Rewrote `Doc/library/__main__.rst` . Broadened scope of the
document to explicitly discuss and differentiate between`__main__.py` in packages versus the`__name__ == '__main__'` expression (and the
idioms that surround it).
- bpo-13814: In the Design FAQ, answer âWhy donât generators support the with statement?â
- bpo-27752: Documentation of csv.Dialect is more descriptive.
- bpo-44453: Fix documentation for the return type of `sysconfig.get_path()` .
- bpo-44392: Added a new section in the C API documentation for types used in type hinting. Documented `Py_GenericAlias` and`Py_GenericAliasType` .
- bpo-38291: Mark `typing.io` and`typing.re` as deprecated since Python
3.8 in the documentation. They were never properly supported by type
checkers.
- bpo-44322: Document that SyntaxError args have a details tuple and that details are adjusted for errors in f-string field replacement expressions.
- bpo-42392: Document the deprecation and removal of the `loop` parameter
for many functions and classes in`asyncio` .
- bpo-44195: Corrected references to `TraversableResources` in docs. There
is no`TraversableReader` .
- bpo-41963: Document that `ConfigParser` strips off comments when reading
configuration files.
- bpo-44072: Correct where in the numeric ABC hierarchy `**` support is
added, i.e., in numbers.Complex, not numbers.Integral.
- bpo-43558: Add the remark to `dataclasses` documentation that the`__init__()` of any base class has to be called in`__post_init__()` , along with a code example.
- bpo-44025: Clarify when â_â in match statements is a keyword, and when not.
- bpo-41706: Fix docs about how methods like `__add__` are invoked when
evaluating operator expressions.
- bpo-41621: Document that `collections.defaultdict` parameter`default_factory` defaults to`None` and is positional-only.
- bpo-41576: document BaseException in favor of bare except
- bpo-21760: The description for __file__ fixed. Patch by Furkan Onder
- bpo-39498: Add a âSecurity Considerationsâ index which links to standard library modules that have explicitly documented security considerations.
- bpo-33479: Remove the unqualified claim that tkinter is threadsafe. It has not been true for several years and likely never was. An explanation of what is true may be added later, after more discussion, and possibly after patching _tkinter.c,

### TestsÂ¶

- bpo-40173: Fix `test.support.import_helper.import_fresh_module()` .
- bpo-45280: Add a test case for empty `typing.NamedTuple` .
- bpo-45269: Cover case when invalid `markers` type is supplied to`c_make_encoder` .
- bpo-45128: Fix `test_multiprocessing_fork` failure due to`test_logging` and`sys.modules` manipulation.
- bpo-45209: Fix `UserWarning: resource_tracker` warning in`_test_multiprocessing._TestSharedMemory.test_shared_memory_cleaned_after_process_termination`
- bpo-45185: Enables `TestEnumerations` test cases in`test_ssl` suite.
- bpo-45195: Fix test_readline.test_nonascii(): sometimes, the newline character is not written at the end, so donât expect it in the output. Patch by Victor Stinner.
- bpo-45156: Fixes infinite loop on `unittest.mock.seal()` of mocks
created by`create_autospec()` .
- bpo-45125: Improves pickling tests and docs of `SharedMemory` and`SharableList` objects.
- bpo-44860: Update `test_sysconfig.test_user_similar()` for the
posix_user scheme:`platlib` doesnât use`sys.platlibdir` . Patch
by Victor Stinner.
- bpo-45052: `WithProcessesTestSharedMemory.test_shared_memory_basics` test was ignored, because`self.assertEqual(sms.size, sms2.size)` line
was failing. It is now removed and test is unskipped.The main motivation for this line to be removed from the test is that the `size` of`SharedMemory` is not ever guaranteed to be the same. It is
decided by the platform.
- bpo-44895: libregrtest now clears the type cache later to reduce the risk of false alarm when checking for reference leaks. Previously, the type cache was cleared too early and libregrtest raised a false alarm about reference leaks under very specific conditions. Patch by Irit Katriel and Victor Stinner.
- bpo-45042: Fixes that test classes decorated with `@hashlib_helper.requires_hashdigest` were skipped all the time.
- bpo-25130: Add calls of `gc.collect()` in tests to support PyPy.
- bpo-45011: Made tests relying on the `_asyncio` C extension module
optional to allow running on alternative Python implementations. Patch by
Serhiy Storchaka.
- bpo-44949: Fix auto history tests of test_readline: sometimes, the newline character is not written at the end, so donât expect it in the output.
- bpo-44891: Tests were added to clarify `id()` is preserved when```
obj
* 1
```
is used on`str` and`bytes` objects. Patch by Nikita
Sobolev.
- bpo-44852: Add ability to wholesale silence DeprecationWarnings while running the regression test suite.
- bpo-40928: Notify users running test_decimal regression tests on macOS of potential harmless âmalloc canât allocate regionâ messages spewed by test_decimal.
- bpo-44734: Fixed floating-point precision issue in turtle tests.
- bpo-44708: Regression tests, when run with -w, are now re-running only the affected test methods instead of re-running the entire test file.
- bpo-42095: Added interop tests for Apple plists: generate plist files with Python plistlib and parse with Apple plutil; and the other way round.
- bpo-44647: Added a permanent Unicode-valued environment variable to regression tests to ensure they handle this use case in the future. If your test environment breaks because of that, report a bug to us, and temporarily set PYTHONREGRTEST_UNICODE_GUARD=0 in your test environment.
- bpo-44515: Adjust recently added contextlib tests to avoid assuming the use of a refcounted GC
- bpo-44287: Fix asyncio test_popen() of test_windows_utils by using a longer timeout. Use military grade battle-tested `test.support.SHORT_TIMEOUT` timeout rather than a hardcoded timeout
of 10 seconds: itâs 30 seconds by default, but it is made longer on slow
buildbots. Patch by Victor Stinner.
- bpo-44451: Reset `DeprecationWarning` filters in`test.test_importlib.test_metadata_api.APITests.test_entry_points_by_index` to avoid`StopIteration` error if`DeprecationWarnings` are ignored.
- bpo-44363: Account for address sanitizer in test_capi. test_capi now passes when run GCC address sanitizer.
- bpo-44364: Add non integral tests for `math.sqrt()` function.
- bpo-43921: Fix test_ssl.test_wrong_cert_tls13(): use `suppress_ragged_eofs=False` , since`read()` can raise`ssl.SSLEOFError` on Windows. Patch by Victor Stinner.
- bpo-43921: Fix test_pha_required_nocert() of test_ssl: catch two more EOF cases (when the `recv()` method returns an empty string). Patch by
Victor Stinner.
- bpo-44131: Add test_frozenmain to test_embed to test the `Py_FrozenMain()` C function. Patch by Victor Stinner.
- bpo-31904: Ignore error string case in test_file_not_exists().
- bpo-42083: Add test to check that `PyStructSequence_NewType` accepts a`PyStructSequence_Desc` with`doc` field set to`NULL` .
- bpo-35753: Fix crash in doctest when doctest parses modules that include unwrappable functions by skipping those functions.
- bpo-30256: Add test for nested queues when using `multiprocessing` shared objects`AutoProxy[Queue]` inside`ListProxy` and`DictProxy`

### BuildÂ¶

- bpo-45220: Avoid building with the Windows 11 SDK previews automatically. This may be overridden by setting the `DefaultWindowsSDKVersion` environment variable before building.
- bpo-45020: Freeze stdlib modules that are imported during startup. This provides significant performance improvements to startup. If necessary, use the previously added â-X frozen_modules=offâ commandline option to force importing the source modules.
- bpo-45188: Windows builds now regenerate frozen modules as the first part of the build. Previously the regeneration was later in the build, which would require it to be restarted if any modules had changed.
- bpo-45163: Fixes Haiku platform build.
- bpo-45067: The ncurses function extended_color_content was introduced in 2017 (https://invisible-island.net/ncurses/NEWS.html#index-t20170401). The ncurses-devel package in CentOS 7 had a older version ncurses resulted in compilation error. For compiling ncurses with extended color support, we verify the version of the ncurses library >= 20170401.
- bpo-45019: Generate lines in relevant files for frozen modules. Up until now each of the files had to be edited manually. This change makes it easier to add to and modify the frozen modules.
- bpo-44340: Add support for building with clang thin lto via âwith-lto=thin/full. Patch by Donghee Na and Brett Holman.
- bpo-44535: Enable building using a Visual Studio 2022 install on Windows.
- bpo-43298: Improved error message when building without a Windows SDK installed.
- bpo-44381: The Windows build now accepts `EnableControlFlowGuard` set to`guard` to enable CFG.
- bpo-41282: Fix broken `make install` that caused standard library
extension modules to be unnecessarily and incorrectly rebuilt during the
install phase of cpython.

### WindowsÂ¶

- bpo-45375: Fixes an assertion failure due to searching for the standard library in unnormalised paths.
- bpo-45022: Update Windows release to include libffi 3.4.2
- bpo-45007: Update to OpenSSL 1.1.1l in Windows build
- bpo-44848: Upgrade Windows installer to use SQLite 3.36.0.
- bpo-44572: Avoid consuming standard input in the `platform` module
- bpo-44582: Accelerate speed of `mimetypes` initialization using a
native implementation of the registry scan.
- bpo-41299: Fix 16 milliseconds jitter when using timeouts in `threading` , such as with`threading.Lock.acquire()` or`threading.Condition.wait()` .
- bpo-42686: Build `sqlite3` with math functions enabled. Patch by
Erlend E. Aasland.
- bpo-40263: This is a follow-on bug from https://bugs.python.org/issue26903. Once that is applied we run into an off-by-one assertion problem. The assert was not correct.

### macOSÂ¶

- bpo-45007: Update macOS installer builds to use OpenSSL 1.1.1l.
- bpo-34602: When building CPython on macOS with ```
./configure
--with-undefined-behavior-sanitizer --with-pydebug
```
, the stack size is
now quadrupled to allow for the entire test suite to pass.
- bpo-44848: Update macOS installer to use SQLite 3.36.0.
- bpo-44689: `ctypes.util.find_library()` now works correctly on macOS
11 Big Sur even if Python is built on an older version of macOS.
Previously, when built on older macOS systems,`find_library` was not
able to find  macOS system libraries when running on Big Sur due to
changes in  how system libraries are stored.
- bpo-41972: The framework buildâs user header path in sysconfig is changed to add a âpythonX.Yâ component to match distutilsâs behavior.
- bpo-43109: Allow âwith-lto configure option to work with Apple-supplied Xcode or Command Line Tools.
- bpo-34932: Add socket.TCP_KEEPALIVE support for macOS. Patch by Shane Harvey.

### IDLEÂ¶

- bpo-45296: On Windows, change exit/quit message to suggest Ctrl-D, which works, instead of <Ctrl-Z Return>, which does not work in IDLE.
- bpo-45193: Make completion boxes appear on Ubuntu again.
- bpo-40128: Mostly fix completions on macOS when not using tcl/tk 8.6.11 (as with 3.9). The added update_idletask call should be harmless and possibly helpful otherwise.
- bpo-33962: Move the indent space setting from the Font tab to the new Windows tab. Patch by Mark Roseman and Terry Jan Reedy.
- bpo-40468: Split the settings dialog General tab into Windows and Shell/ED tabs. Move help sources, which extend the Help menu, to the Extensions tab. Make space for new options and shorten the dialog. The latter makes the dialog better fit small screens.
- bpo-41611: Avoid uncaught exceptions in `AutoCompleteWindow.winconfig_event()` .
- bpo-41611: Fix IDLE sometimes freezing upon tab-completion on macOS.
- bpo-44010: Highlight the new match statementâs soft keywords: `match` ,`case` , and`_` . However, this highlighting is not
perfect and will be incorrect in some rare cases, including some`_` -s
in`case` patterns.
- bpo-44026: Include interpreterâs typo fix suggestions in message line for NameErrors and AttributeErrors. Patch by E. Paine.

### Tools/DemosÂ¶

- bpo-44786: Fix a warning in regular expression in the c-analyzer script.
- bpo-44967: pydoc now returns a non-zero status code when a module cannot be found.
- bpo-44978: Allow the Argument Clinic tool to handle `__complex__` special methods.
- bpo-43425: Removed the âtest2to3â demo project that demonstrated using lib2to3 to support Python 2.x and Python 3.x from a single source in a distutils package. Patch by Donghee Na
- bpo-44074: Make patchcheck automatically detect the correct base branch name (previously it was hardcoded to âmasterâ)
- bpo-20291: Added support for variadic positional parameters in Argument Clinic.

### C APIÂ¶

- bpo-41710: The PyThread_acquire_lock_timed() function now clamps the timeout if it is too large, rather than aborting the process. Patch by Victor Stinner.
- bpo-44687: `BufferedReader.peek()` no longer raises`ValueError` when the entire file has already been buffered.
- bpo-45116: Add the `Py_ALWAYS_INLINE` macro to ask the compiler
to always inline a static inline function. The compiler can ignore it and
decides to not inline the function. Patch by Victor Stinner.
- bpo-45094: Add the `Py_NO_INLINE` macro to disable inlining on a
function. Patch by Victor Stinner.
- bpo-45061: Add a deallocator to the `bool` type to detect refcount
bugs in C extensions which call`Py_DECREF(Py_True);` or`Py_DECREF(Py_False);` by mistake. Patch by Victor Stinner.
- bpo-42035: Add a new `PyType_GetQualName()` function to get typeâs
qualified name.
- bpo-41103: Reverts removal of the old buffer protocol because they are part of stable ABI.
- bpo-44751: Remove `crypt.h` include from the public`Python.h` header.
- bpo-42747: The `Py_TPFLAGS_HAVE_VERSION_TAG` type flag now does nothing.
The`Py_TPFLAGS_HAVE_AM_SEND` flag (which was added in 3.10) is removed.
Both were unnecessary because it is not possible to have type objects with
the relevant fields missing.
- bpo-44530: Added the `co_qualname` to the`PyCodeObject` structure to
propagate the qualified name from the compiler to code objects.Patch by Gabriele N. Tornetta
- bpo-44441: `Py_RunMain()` now resets`PyImport_Inittab` to
its initial value at exit. It must be possible to call`PyImport_AppendInittab()` or`PyImport_ExtendInittab()` at
each Python initialization. Patch by Victor Stinner.
- bpo-39947: Remove 4 private trashcan C API functions which were only kept for the backward compatibility of the stable ABI with Python 3.8 and older, since the trashcan API was not usable with the limited C API on Python 3.8 and older. The trashcan API was excluded from the limited C API in Python 3.9. Removed functions: 
  - _PyTrash_deposit_object()
  - _PyTrash_destroy_chain()
  - _PyTrash_thread_deposit_object()
  - _PyTrash_thread_destroy_chain()
 The trashcan C API was never usable with the limited C API, since old trashcan macros accessed directly `PyThreadState` members like`_tstate->trash_delete_nesting` , whereas the`PyThreadState` structure is opaque in the limited C API.Exclude also the `PyTrash_UNWIND_LEVEL` constant from the C API.Patch by Victor Stinner.
- bpo-40939: Removed documentation for the removed `PyParser_*` C API.
- bpo-43795: The list in Contents of Limited API now shows the public name `PyFrameObject` rather than`_frame` . The non-existing entry`_node` no longer appears in the list.
- bpo-44378: `Py_IS_TYPE()` no longer uses`Py_TYPE()` to avoid
a compiler warning: no longer cast`const PyObject*` to`PyObject*` .
Patch by Victor Stinner.
- bpo-39573: Convert the `Py_TYPE()` and`Py_SIZE()` macros to
static inline functions. The`Py_SET_TYPE()` and`Py_SET_SIZE()` functions must now be used to set an object type and
size. Patch by Victor Stinner.
- bpo-44263: The `PyType_Ready()` function now raises an error if a
type is defined with the`Py_TPFLAGS_HAVE_GC` flag set but has no
traverse function (`PyTypeObject.tp_traverse` ). Patch by Victor
Stinner.
- bpo-43795: The undocumented function `Py_FrozenMain()` is removed
from the Limited API.
- bpo-44113: Deprecate the following functions to configure the Python initialization: 
  - `PySys_AddWarnOptionUnicode()`
  - `PySys_AddWarnOption()`
  - `PySys_AddXOption()`
  - `PySys_HasWarnOptions()`
  - `Py_SetPath()`
  - `Py_SetProgramName()`
  - `Py_SetPythonHome()`
  - `Py_SetStandardStreamEncoding()`
  - `_Py_SetProgramFullPath()`
 Use the new `PyConfig` API of the Python Initialization
Configuration instead (**PEP 587** ).
- bpo-44094: Remove `PyErr_SetFromErrnoWithUnicodeFilename()` ,`PyErr_SetFromWindowsErrWithUnicodeFilename()` , and`PyErr_SetExcFromWindowsErrWithUnicodeFilename()` . They are not
documented and have been deprecated since Python 3.3.
- bpo-43795: `PyCodec_Unregister()` is now properly exported as a
function in the Windows Stable ABI DLL.
- bpo-44029: Remove deprecated `Py_UNICODE` APIs:`PyUnicode_Encode` ,`PyUnicode_EncodeUTF7` ,`PyUnicode_EncodeUTF8` ,`PyUnicode_EncodeUTF16` ,`PyUnicode_EncodeUTF32` ,`PyUnicode_EncodeLatin1` ,`PyUnicode_EncodeMBCS` ,`PyUnicode_EncodeDecimal` ,`PyUnicode_EncodeRawUnicodeEscape` ,`PyUnicode_EncodeCharmap` ,`PyUnicode_EncodeUnicodeEscape` ,`PyUnicode_TransformDecimalToASCII` ,`PyUnicode_TranslateCharmap` ,`PyUnicodeEncodeError_Create` ,`PyUnicodeTranslateError_Create` . See**PEP 393** and**PEP 624** for reference.
- bpo-42035: Add a new `PyType_GetName()` function to get typeâs short
name.

## Python 3.10.0 beta 1Â¶

*Release date: 2021-05-03*

### SecurityÂ¶

- bpo-43434: Creating `sqlite3.Connection` objects now also produces`sqlite3.connect` and`sqlite3.connect/handle` auditing events. Previously these events were only produced by`sqlite3.connect()` calls. Patch by Erlend E. Aasland.
- bpo-43998: The `ssl` module sets more secure cipher suites defaults.
Ciphers without forward secrecy and with SHA-1 MAC are disabled by
default. Security level 2 prohibits weak RSA, DH, and ECC keys with less
than 112 bits of security.`SSLContext` defaults to minimum
protocol version TLS 1.2. Settings are based on Hynek Schlawackâs
research.
- bpo-43882: The presence of newline or tab characters in parts of a URL could allow some forms of attacks. Following the controlling specification for URLs defined by WHATWG `urllib.parse()` now removes ASCII newlines and tabs from URLs,
preventing such attacks.
- bpo-43472: Ensures interpreter-level audit hooks receive the `cpython.PyInterpreterState_New` event when called through the`_xxsubinterpreters` module.
- bpo-43362: Fix invalid free in _sha3 module. The issue was introduced in 3.10.0a1. Python 3.9 and earlier are not affected.
- bpo-43762: Add audit events for `sqlite3.connect/handle()` ,`sqlite3.Connection.enable_load_extension()` , and`sqlite3.Connection.load_extension()` . Patch by Erlend E. Aasland.
- bpo-43756: Add new audit event `glob.glob/2` to incorporate the new*root_dir* and*dir_fd* arguments added to`glob.glob()` and`glob.iglob()` .
- bpo-36384: `ipaddress` module no longer accepts any leading zeros in
IPv4 address strings. Leading zeros are ambiguous and interpreted as octal
notation by some libraries. For example the legacy function`socket.inet_aton()` treats leading zeros as octal notation. glibc
implementation of modern`inet_pton()` does not accept any
leading zeros. For a while the`ipaddress` module used to accept
ambiguous leading zeros.
- bpo-43075: Fix Regular Expression Denial of Service (ReDoS) vulnerability in `urllib.request.AbstractBasicAuthHandler` .  The ReDoS-vulnerable
regex has quadratic worst-case complexity and it allows cause a denial of
service when identifying crafted invalid RFCs. This ReDoS issue is on the
client side and needs remote attackers to control the HTTP server.
- bpo-42800: Audit hooks are now fired for frame.f_code, traceback.tb_frame, and generator code/frame attribute access.
- bpo-37363: Add audit events to the `http.client` module.

### Core and BuiltinsÂ¶

- bpo-43977: Prevent classes being both a sequence and a mapping when pattern matching.
- bpo-43977: Use `tp_flags` on the class object to
determine if the subject is a sequence or mapping when pattern matching.
Avoids the need to import`collections.abc` when pattern matching.
- bpo-43892: Restore proper validation of complex literal value patterns when parsing `match` blocks.
- bpo-43933: Set frame.f_lineno to the line number of the âwithâ kweyword when executing the call to `__exit__` .
- bpo-43933: If the current position in a frame has no line number then set the f_lineno attribute to None, instead of -1, to conform to PEP 626. This should not normally be possible, but might occur in some unusual circumstances.
- bpo-43963: Importing the `_signal` module in a subinterpreter has no
longer side effects.
- bpo-42739: The internal representation of line number tables is changed to not use sentinels, and an explicit length parameter is added to the out of process API function `PyLineTable_InitAddressRange` . This makes the
handling of line number tables more robust in some circumstances.

### LibraryÂ¶

- bpo-43908: Make `re` types immutable. Patch by Erlend E. Aasland.
- bpo-43908: Make the `array.array` type immutable. Patch by Erlend
E. Aasland.

### Core and BuiltinsÂ¶

- bpo-43901: Change class and module objects to lazy-create empty annotations dicts on demand. The annotations dicts are stored in the objectâs __dict__ for backwards compatibility.
- bpo-43892: Match patterns now use new dedicated AST nodes ( `MatchValue` ,`MatchSingleton` ,`MatchSequence` ,`MatchStar` ,`MatchMapping` ,`MatchClass` ) rather than reusing expression AST nodes.`MatchAs` and`MatchOr` are now defined as pattern nodes rather than as expression
nodes. Patch by Nick Coghlan.
- bpo-42725: Usage of `await` /`yield` /`yield from` and named
expressions within an annotation is now forbidden when PEP 563 is
activated.
- bpo-43754: When performing structural pattern matching ( **PEP 634** ),
captured names are now left unbound until the*entire* pattern has matched
successfully.
- bpo-42737: Annotations for complex targets (everything beside simple names) no longer cause any runtime effects with ```
from __future__ import
annotations
```
.
- bpo-43914: `SyntaxError` exceptions raised by the interpreter will
highlight the full error range of the expression that constitutes the
syntax error itself, instead of just where the problem is detected. Patch
by Pablo Galindo.
- bpo-38605: Revert making `from __future__ import annotations` the
default. This follows the Steering Council decision to postpone PEP 563
changes to at least Python 3.11. See the original email for more
information regarding the decision:
https://mail.python.org/archives/list/python-dev@python.org/thread/CLVXXPQ2T2LQ5MP2Y53VVQFCXYWQJHKZ/.
Patch by Pablo Galindo.
- bpo-43475: Hashes of NaN values now depend on object identity. Formerly, they always hashed to 0 even though NaN values are not equal to one another. Having the same hash for unequal values caused pile-ups in hash tables.
- bpo-43859: Improve the error message for `IndentationError` exceptions. Patch by Pablo Galindo
- bpo-41323: Constant tuple folding in bytecode optimizer now reuses tuple in constant table.
- bpo-43846: Data stack usage is much reduced for large literal and call expressions.
- bpo-38530: When printing `NameError` raised by the interpreter,`PyErr_Display()` will offer suggestions of similar variable names
in the function that the exception was raised from. Patch by Pablo Galindo
- bpo-43823: Improve syntax errors for invalid dictionary literals. Patch by Pablo Galindo.
- bpo-43822: Improve syntax errors in the parser for missing commas between expressions. Patch by Pablo Galindo.
- bpo-43798: `ast.alias` nodes now include source location metadata
attributes e.g. lineno, col_offset.
- bpo-43797: Improve `SyntaxError` error messages for invalid comparisons.
Patch by Pablo Galindo.
- bpo-43760: Move the flag for checking whether tracing is enabled to the C stack, from the heap. Should speed up dispatch in the interpreter.
- bpo-43682: Static methods ( `@staticmethod` ) and class methods
(`@classmethod` ) now inherit the method attributes (`__module__` ,`__name__` ,`__qualname__` ,`__doc__` ,`__annotations__` ) and have
a new`__wrapped__` attribute. Patch by Victor Stinner.
- bpo-43751: Fixed a bug where `anext(ait, default)` would erroneously
return None.
- bpo-42128: `__match_args__` is no longer allowed to be a
list.
- bpo-43683: Add GEN_START opcode. Marks start of generator, including async, or coroutine and handles sending values to a newly created generator or coroutine.

### LibraryÂ¶

- bpo-43105: Importlib now resolves relative paths when creating module spec objects from file locations.

### Core and BuiltinsÂ¶

- bpo-43682: Static methods ( `@staticmethod` ) are now callable as
regular functions. Patch by Victor Stinner.
- bpo-42609: Prevented crashes in the AST validator and optimizer when compiling some absurdly long expressions like `"+0"*1000000` .`RecursionError` is now raised instead.
- bpo-38530: When printing `AttributeError` ,`PyErr_Display()` will offer suggestions of similar attribute names in the object that the
exception was raised from. Patch by Pablo Galindo

### LibraryÂ¶

- bpo-44015: In @dataclass(), raise a TypeError if KW_ONLY is specified more than once.
- bpo-25478: Added a *total()* method to collections.Counter() to compute
the sum of the counts.
- bpo-43733: Change `netrc.netrc` to use UTF-8 encoding before using
locale encoding.
- bpo-43979: Removed an unnecessary list comprehension before looping from `urllib.parse.parse_qsl()` .  Patch by Christoph Zwerschke and Donghee
Na.
- bpo-43993: Update bundled pip to 21.1.1.
- bpo-43957: [Enum] Deprecate `TypeError` when non-member is used in a
containment check; In 3.12`True` or`False` will be returned instead,
and containment will return`True` if the value is either a member of
that enum or one of its membersâ value.
- bpo-42904: For backwards compatibility with previous minor versions of Python, if `typing.get_type_hints()` receives no namespace dictionary
arguments,`typing.get_type_hints()` will search through the global
then local namespaces during evaluation of stringized type annotations
(string forward references) inside a class.
- bpo-43945: [Enum] Deprecate non-standard mixin format() behavior: in 3.12 the enum member, not the memberâs value, will be used for format() calls.
- bpo-41139: Deprecate undocumented `cgi.log()` API.
- bpo-43937: Fixed the `turtle` module working with non-default root
window.
- bpo-43930: Update bundled pip to 21.1 and setuptools to 56.0.0
- bpo-43907: Fix a bug in the pure-Python pickle implementation when using protocol 5, where bytearray instances that occur several time in the pickled object graph would incorrectly unpickle into repeated copies of the bytearray object.
- bpo-43926: In `importlib.metadata` , provide a uniform interface to`Description` , allow for any field to be encoded with multiline values,
remove continuation lines from multiline values, and add a`.json` property for easy access to the PEP 566 JSON-compatible form. Sync with`importlib_metadata 4.0` .
- bpo-43920: OpenSSL 3.0.0: `load_verify_locations()` now returns a consistent error message when cadata contains no valid
certificate.
- bpo-43607: `urllib` can now convert Windows paths with`\\?\` prefixes into URL paths.
- bpo-43817: Add `inspect.get_annotations()` , which safely computes the
annotations defined on an object.  It works around the quirks of accessing
the annotations from various types of objects, and makes very few
assumptions about the object passed in.`inspect.get_annotations()` can also correctly un-stringize stringized annotations.`inspect.signature()` ,`inspect.from_callable()` , and`inspect.from_function()` now call`inspect.get_annotations()` to
retrieve annotations.  This means`inspect.signature()` and`inspect.from_callable()` can now un-stringize stringized annotations,
too.
- bpo-43284: platform.win32_ver derives the windows version from sys.getwindowsversion().platform_version which in turn derives the version from kernel32.dll (which can be of a different version than Windows itself). Therefore change the platform.win32_ver to determine the version using the platform moduleâs _syscmd_ver private function to return an accurate version.
- bpo-42854: The `ssl` module now uses`SSL_read_ex` and`SSL_write_ex` internally. The functions support reading and writing of
data larger than 2 GB. Writing zero-length data no longer fails with a
protocol violation error.
- bpo-42333: Port `_ssl` extension module to multiphase initialization.
- bpo-43880: `ssl` now raises DeprecationWarning for OP_NO_SSL/TLS*
options, old TLS versions, old protocols, and other features that have
been deprecated since Python 3.6, 3.7, or OpenSSL 1.1.0.
- bpo-41559: **PEP 612** is now implemented purely in Python; builtin`types.GenericAlias` objects no longer include`typing.ParamSpec` in`__parameters__` (with the exception of`collections.abc.Callable` âs`GenericAlias` ). This means previously invalid uses of`ParamSpec` (such as`list[P]` ) which worked in earlier versions of Python 3.10
alpha, will now raise`TypeError` during substitution.
- bpo-43867: The `multiprocessing``Server` class now explicitly
catches`SystemExit` and closes the client connection in this case.
It happens when the`Server.serve_client()` method reaches the end of
file (EOF).
- bpo-40443: Remove unused imports: pyclbr no longer uses copy, and typing no longer uses ast. Patch by Victor Stinner.
- bpo-43820: Remove an unneeded copy of the namespace passed to dataclasses.make_dataclass().
- bpo-43787: Add `__iter__()` method to`bz2.BZ2File` ,`gzip.GzipFile` , and`lzma.LZMAFile` . It makes iterating
them about 2x faster. Patch by Inada Naoki.
- bpo-43680: Deprecate io.OpenWrapper and _pyio.OpenWrapper: use io.open and _pyio.open instead. Until Python 3.9, _pyio.open was not a static method and builtins.open was set to OpenWrapper to not become a bound method when set to a class variable. _io.open is a built-in function whereas _pyio.open is a Python function. In Python 3.10, _pyio.open() is now a static method, and builtins.open() is now io.open().
- bpo-43680: The Python `_pyio.open()` function becomes a static method
to behave as`io.open()` built-in function: donât become a bound
method when stored as a class variable. It becomes possible since static
methods are now callable in Python 3.10. Moreover,`_pyio.OpenWrapper()` becomes a simple alias to`_pyio.open()` .
Patch by Victor Stinner.
- bpo-41515: Fix `KeyError` raised in`typing.get_type_hints()` due
to synthetic modules that donât appear in`sys.modules` .
- bpo-43776: When `subprocess.Popen` args are provided as a string or
as`pathlib.Path` , the Popen instance repr now shows the right
thing.
- bpo-42248: [Enum] ensure exceptions raised in `_missing__` are released
- bpo-43744: fix issue with enum member name matching the start of a private variable name
- bpo-43772: Fixed the return value of `TypeVar.__ror__` . Patch by Jelle
Zijlstra.
- bpo-43764: Add match_args parameter to @dataclass decorator to allow suppression of __match_args__ generation.
- bpo-43799: OpenSSL 3.0.0: define `OPENSSL_API_COMPAT` 1.1.1 to suppress
deprecation warnings. Python requires OpenSSL 1.1.1 APIs.
- bpo-43478: Mocks can no longer be used as the specs for other Mocks. As a result, an already-mocked object cannot have an attribute mocked using `autospec=True` or be the subject of a`create_autospec(...)` call.
This can uncover bugs in tests since these Mock-derived Mocks will always
pass certain tests (e.g.`isinstance()` ) and builtin assert functions
(e.g. assert_called_once_with) will unconditionally pass.
- bpo-43794: Add `ssl.OP_IGNORE_UNEXPECTED_EOF` constants (OpenSSL
3.0.0)
- bpo-43785: Improve `bz2.BZ2File` performance by removing the RLock from
BZ2File. This makes BZ2File thread unsafe in the face of multiple
simultaneous readers or writers, just like its equivalent classes in`gzip` and`lzma` have always been.  Patch by Inada Naoki.
- bpo-43789: OpenSSL 3.0.0: Donât call the password callback function a second time when first call has signaled an error condition.
- bpo-43788: The header files for `ssl` error codes are now OpenSSL
version-specific. Exceptions will now show correct reason and library
codes. The`make_ssl_data.py` script has been rewritten to use OpenSSLâs
text file with error codes.
- bpo-43766: Implement **PEP 647** in the`typing` module by adding`TypeGuard` .
- bpo-25264: `os.path.realpath()` now accepts a*strict* keyword-only
argument. When set to`True` ,`OSError` is raised if a path doesnât
exist or a symlink loop is encountered.
- bpo-43780: In `importlib.metadata` , incorporate changes from
importlib_metadata 3.10: Add mtime-based caching during distribution
discovery. Flagged use of dict result from`entry_points()` as
deprecated.
- gh-47383: The `P.args` and`P.kwargs` attributes of`typing.ParamSpec` are now instances of the new classes`typing.ParamSpecArgs` and`typing.ParamSpecKwargs` , which
enables a more useful`repr()` . Patch by Jelle Zijlstra.
- bpo-43731: Add an `encoding` parameter`logging.fileConfig()` .
- bpo-43712: Add `encoding` and`errors` parameters to`fileinput.input()` and`fileinput.FileInput` .
- bpo-38659: A `simple_enum` decorator is added to the`enum` module to
convert a normal class into an Enum.`test_simple_enum` added to test
simple enums against a corresponding normal Enum.  Standard library
modules updated to use`simple_enum` .
- bpo-43764: Fix an issue where `__match_args__` generation
could fail for some`dataclasses` .
- bpo-43752: Fix `sqlite3` regression for zero-sized blobs with
converters, where`b""` was returned instead of`None` . The regression
was introduced by PR 24723. Patch by Erlend E. Aasland.
- bpo-43655: `tkinter` dialog windows are now recognized as dialogs by
window managers on macOS and X Window.
- bpo-43723: The following `threading` methods are now deprecated and
should be replaced:
  - `currentThread` =>`threading.current_thread()`
  - `activeCount` =>`threading.active_count()`
  - `Condition.notifyAll` =>`threading.Condition.notify_all()`
  - `Event.isSet` =>`threading.Event.is_set()`
  - `Thread.setName` =>`threading.Thread.name`
  - `thread.getName` =>`threading.Thread.name`
  - `Thread.isDaemon` =>`threading.Thread.daemon`
  - `Thread.setDaemon` =>`threading.Thread.daemon`
 Patch by Jelle Zijlstra.
- bpo-2135: Deprecate find_module() and find_loader() implementations in importlib and zipimport.
- bpo-43534: `turtle.textinput()` and`turtle.numinput()` create now
a transient window working on behalf of the canvas window.
- bpo-43532: Add the ability to specify keyword-only fields to dataclasses. These fields will become keyword-only arguments to the generated __init__.
- bpo-43522: Fix problem with `hostname_checks_common_name` . OpenSSL does not copy
hostflags from*struct SSL_CTX* to*struct SSL* .
- bpo-8978: Improve error message for `tarfile.open()` when`lzma` /`bz2` are unavailable.  Patch by Anthony Sottile.
- bpo-42967: Allow `bytes``separator` argument in`urllib.parse.parse_qs` and`urllib.parse.parse_qsl` when parsing`str` query strings. Previously, this raised a`TypeError` .
- bpo-43296: Improve `sqlite3` error handling:`sqlite3_value_blob()` errors that set`SQLITE_NOMEM` now raise`MemoryError` . Patch by
Erlend E. Aasland.
- bpo-43312: New functions `sysconfig.get_preferred_scheme()` and`sysconfig.get_default_scheme()` are added to query a platform for its
preferred âuserâ, âhomeâ, and âprefixâ (default) scheme names.
- bpo-43265: Improve `sqlite3.Connection.backup()` error handling. The
error message for non-existent target database names is now```
unknown
database <database name>
```
instead of`SQL logic error` . Patch by Erlend
E. Aasland.
- bpo-41282: Install schemes in `distutils.command.install` are now loaded
from`sysconfig` .
- bpo-41282: `distutils.sysconfig` has been merged to`sysconfig` .
- bpo-43176: Fixed processing of a dataclass that inherits from a frozen dataclass with no fields. It is now correctly detected as an error.
- bpo-43080: `pprint` now has support for`dataclasses.dataclass` . Patch by Lewis Gaul.
- bpo-39950: Add `pathlib.Path.hardlink_to()` method that supersedes`link_to()` . The new method has the same argument order as`symlink_to()` .
- bpo-42904: `typing.get_type_hints()` now checks the local namespace of
a class when evaluating**PEP 563** annotations inside said class.
- bpo-42269: Add `slots` parameter to`dataclasses.dataclass` decorator
to automatically generate`__slots__` for class. Patch provided by Yurii
Karabas.
- bpo-39529: Deprecated use of `asyncio.get_event_loop()` without
running event loop. Emit deprecation warning for`asyncio` functions
which implicitly create a`Future` or`Task` objects if there is no running event loop and no
explicit*loop* argument is passed:`ensure_future()` ,`wrap_future()` ,`gather()` ,`shield()` ,`as_completed()` and constructors of`Future` ,`Task` ,`StreamReader` ,`StreamReaderProtocol` .
- bpo-18369: Certificate and PrivateKey classes were added to the ssl module. Certificates and keys can now be loaded from memory buffer, too.
- bpo-41486: Use a new output buffer management code for `bz2` /`lzma` /`zlib` modules, and add`.readall()` function to`_compression.DecompressReader` class. These bring some performance
improvements. Patch by Ma Lin.
- bpo-31870: The `ssl.get_server_certificate()` function now has a*timeout* parameter.
- bpo-41735: Fix thread locks in zlib module may go wrong in rare case. Patch by Ma Lin.
- bpo-36470: Fix dataclasses with `InitVar` s and`replace()` . Patch by Claudiu Popa.
- bpo-40849: Expose X509_V_FLAG_PARTIAL_CHAIN ssl flag
- bpo-35114: `ssl.RAND_status()` now returns a boolean value (as
documented) instead of`1` or`0` .
- bpo-39906: `pathlib.Path.stat()` and`chmod()` now
accept a*follow_symlinks* keyword-only argument for consistency with
corresponding functions in the`os` module.
- bpo-39899: `os.path.expanduser()` now refuses to guess Windows home
directories if the basename of current userâs home directory does not
match their username.`pathlib.Path.expanduser()` and`home()` now
consistently raise`RuntimeError` exception when a home directory
cannot be resolved. Previously a`KeyError` exception could be raised
on Windows when the`"USERNAME"` environment variable was unset.
- bpo-36076: Added SNI support to `ssl.get_server_certificate()` .
- bpo-38490: Covariance, Pearsonâs correlation, and simple linear regression functionality was added to statistics module. Patch by Tymoteusz WoÅodÅºko.
- bpo-33731: Provide a locale.localize() function, which converts a normalized number string into a locale format.
- bpo-32745: Fix a regression in the handling of ctypesâ `ctypes.c_wchar_p` type: embedded null characters would cause a`ValueError` to be raised. Patch by Zackery Spytz.

### DocumentationÂ¶

- bpo-43987: Add âAnnotations Best Practicesâ document as a new HOWTO.
- bpo-43977: Document the new `Py_TPFLAGS_MAPPING` and`Py_TPFLAGS_SEQUENCE` type flags.
- bpo-43959: The documentation on the PyContextVar C-API was clarified.
- bpo-43938: Update dataclasses documentation to express that FrozenInstanceError is derived from AttributeError.
- bpo-43778: Fix the Sphinx glossary_search extension: create the _static/ sub-directory if it doesnât exist.
- bpo-43755: Update documentation to reflect that unparenthesized lambda expressions can no longer be the expression part in an `if` clause in
comprehensions and generator expressions since Python 3.9.
- bpo-43739: Fixing the example code in Doc/extending/extending.rst to declare and initialize the pmodule variable to be of the right type.

### TestsÂ¶

- bpo-43961: Fix test_logging.test_namer_rotator_inheritance() on Windows: use `os.replace()` rather than`os.rename()` . Patch by Victor
Stinner.
- bpo-43842: Fix a race condition in the SMTP test of test_logging. Donât close a file descriptor (socket) from a different thread while asyncore.loop() is polling the file descriptor. Patch by Victor Stinner.
- bpo-43843: `test.libregrtest` now marks a test as ENV_CHANGED
(altered the execution environment) if a thread raises an exception but
does not catch it. It sets a hook on`threading.excepthook()` . Use`--fail-env-changed` option to mark the test as failed. Patch by Victor
Stinner.
- bpo-43811: Tests multiple OpenSSL versions on GitHub Actions. Use ccache to speed up testing.
- bpo-43791: OpenSSL 3.0.0: Disable testing of legacy protocols TLS 1.0 and 1.1. Tests are failing with TLSV1_ALERT_INTERNAL_ERROR.

### BuildÂ¶

### WindowsÂ¶

- bpo-35306: Adds additional arguments to `os.startfile()` function.
- bpo-43538: Avoid raising errors from `pathlib.Path.exists()` when
passed an invalid filename.
- bpo-38822: Fixed `os.stat()` failing on inaccessible directories with
a trailing slash, rather than falling back to the parent directoryâs
metadata. This implicitly affected`os.path.exists()` and`os.path.isdir()` .
- bpo-26227: Fixed decoding of host names in `socket.gethostbyaddr()` and`socket.gethostbyname_ex()` .
- bpo-40432: Updated pegen regeneration script on Windows to find and use Python 3.8 or higher. Prior to this, pegen regeneration already required 3.8 or higher, but the script may have used lower versions of Python.
- bpo-43745: Actually updates Windows release to OpenSSL 1.1.1k. Earlier releases were mislabelled and actually included 1.1.1i again.
- bpo-43652: Update Tcl and Tk to 8.6.11 in Windows installer.
- bpo-43492: Upgrade Windows installer to use SQLite 3.35.5.
- bpo-30555: Fix `WindowsConsoleIO` errors in the presence of fd
redirection. Patch by Segev Finer.

### macOSÂ¶

- bpo-42119: Fix check for macOS SDK paths when building Python. Narrow search to match contents of SDKs, namely only files in `/System/Library` ,`/System/IOSSupport` , and`/usr` other than`/usr/local` . Previously, anything under`/System` was assumed to be
in an SDK which causes problems with the new file system layout in 10.15+
where user file systems may appear to be mounted under`/System` .  Paths
in`/Library` were also incorrectly treated as SDK locations.
- bpo-43568: Drop support for MACOSX_DEPLOYMENT_TARGET < 10.3
- bpo-44009: Provide âpython3.x-intel64â executable to allow reliably forcing macOS universal2 framework builds to run under Rosetta 2 Intel-64 emulation on Apple Silicon Macs. This can be useful for testing or when universal2 wheels are not yet available.
- bpo-43851: Build SQLite with `SQLITE_OMIT_AUTOINIT` on macOS. Patch by
Erlend E. Aasland.
- bpo-43492: Update macOS installer to use SQLite 3.35.4.
- bpo-42235: `Mac/BuildScript/build-installer.py` will now use
ââenable-optimizationsâ and`--with-lto` when building on macOS 10.15
or later.

### IDLEÂ¶

- bpo-37903: Add mouse actions to the shell sidebar. Left click and optional drag selects one or more lines, as with the editor line number sidebar. Right click after selecting raises a context menu with âcopy with promptsâ. This zips together prompts from the sidebar with lines from the selected text.
- bpo-43981: Fix reference leak in test_sidebar and test_squeezer. Patches by Terry Jan Reedy and Pablo Galindo
- bpo-37892: Indent IDLE Shell input with spaces instead of tabs
- bpo-43655: IDLE dialog windows are now recognized as dialogs by window managers on macOS and X Window.
- bpo-37903: IDLEâs shell now shows prompts in a separate side-bar.

### C APIÂ¶

- bpo-43916: Add a new `Py_TPFLAGS_DISALLOW_INSTANTIATION` type
flag to disallow creating type instances. Patch by Victor Stinner.
- bpo-43774: Remove the now unused `PYMALLOC_DEBUG` macro. Debug hooks on
memory allocators are now installed by default if Python is built in debug
mode (if`Py_DEBUG` macro is defined). Moreover, they can now be used on
Python build in release mode (ex: using`PYTHONMALLOC=debug` environment
variable).
- bpo-43962: _PyInterpreterState_IDIncref() now calls _PyInterpreterState_IDInitref() and always increments id_refcount. Previously, calling _xxsubinterpreters.get_current() could create an id_refcount inconsistency when a _xxsubinterpreters.InterpreterID object was deallocated. Patch by Victor Stinner.
- bpo-28254: Add new C-API functions to control the state of the garbage collector: `PyGC_Enable()` ,`PyGC_Disable()` ,`PyGC_IsEnabled()` , corresponding to the functions in the`gc` module.
- bpo-43908: Introduce `Py_TPFLAGS_IMMUTABLETYPE` flag for
immutable type objects, and modify`PyType_Ready()` to set it for
static types. Patch by Erlend E. Aasland.
- bpo-43795: `PyMem_Calloc()` is now available in the limited C API
(`Py_LIMITED_API` ).
- bpo-43868: `PyOS_ReadlineFunctionPointer()` is no longer exported by
limited C API headers and by`python3.dll` on Windows. Like any function
that takes`FILE*` , it is not part of the stable ABI.
- bpo-43795: Stable ABI and limited API definitions are generated from a central manifest ( **PEP 652** ).
- bpo-43753: Add the `Py_Is(x, y)` function to test if the*x* object is the*y* object, the same as`x is y` in Python. Add also
the`Py_IsNone()` ,`Py_IsTrue()` ,`Py_IsFalse()` functions to test if an object is, respectively, the`None` singleton,
the`True` singleton or the`False` singleton. Patch by Victor
Stinner.

## Python 3.10.0 alpha 7Â¶

*Release date: 2021-04-05*

### SecurityÂ¶

- bpo-42988: **CVE 2021-3426** : Remove the`getfile` feature of the`pydoc` module which could be abused to read arbitrary files on the
disk (directory traversal vulnerability). Moreover, even source code of
Python modules can contain sensitive data like passwords. Vulnerability
reported by David SchwÃ¶rer.
- bpo-43285: `ftplib` no longer trusts the IP address value returned
from the server in response to the PASV command by default.  This prevents
a malicious FTP server from using the response to probe IPv4 address and
port combinations on the client network.Code that requires the former vulnerable behavior may set a `trust_server_pasv_ipv4_address` attribute on their`ftplib.FTP` instances to`True` to re-enable it.
- bpo-43439: Add audit hooks for `gc.get_objects()` ,`gc.get_referrers()` and`gc.get_referents()` . Patch by Pablo
Galindo.

### Core and BuiltinsÂ¶

- bpo-27129: Update CPython bytecode magic number.
- bpo-43672: Raise ImportWarning when calling find_loader().
- bpo-43660: Fix crash that happens when replacing `sys.stderr` with a
callable that can remove the object while an exception is being printed.
Patch by Pablo Galindo.
- bpo-27129: The bytecode interpreter uses instruction, rather byte, offsets internally. This reduces the number of EXTENDED_ARG instructions needed and streamlines instruction dispatch a bit.

### LibraryÂ¶

- bpo-40645: Fix reference leak in the `_hashopenssl` extension. Patch
by Pablo Galindo.

### Core and BuiltinsÂ¶

- bpo-42134: Calls to find_module() by the import system now raise ImportWarning.
- bpo-41064: Improve the syntax error for invalid usage of double starred elements (â**â) in f-strings. Patch by Pablo Galindo.
- bpo-43575: Speed up calls to `map()` by using the**PEP 590**`vectorcall` calling convention. Patch by Donghee Na.
- bpo-42137: The import system now prefers using `__spec__` for`ModuleType.__repr__` over`module_repr()` .
- bpo-43452: Added micro-optimizations to `_PyType_Lookup()` to improve
cache lookup performance in the common case of cache hits.
- bpo-43555: Report the column offset for `SyntaxError` for invalid
line continuation characters. Patch by Pablo Galindo.
- bpo-43517: Fix misdetection of circular imports when using ```
from pkg.mod
import attr
```
, which caused false positives in non-trivial multi-threaded
code.
- bpo-43497: Emit SyntaxWarnings for assertions with tuple constants, this is a regression introduced in python3.7
- bpo-39316: Tracing now has correct line numbers for attribute accesses when the attribute is on a different line from the object. Improves debugging and profiling for multi-line method chains.
- bpo-35883: Python no longer fails at startup with a fatal error if a command line argument contains an invalid Unicode character. The `Py_DecodeLocale()` function now escapes byte sequences which would
be decoded as Unicode characters outside the [U+0000; U+10ffff] range.
- bpo-43410: Fix a bug that was causing the parser to crash when emitting syntax errors when reading input from stdin. Patch by Pablo Galindo
- bpo-43406: Fix a possible race condition where `PyErr_CheckSignals` tries to execute a non-Python signal handler.
- bpo-42128: Add `__match_args__` to struct sequence objects. Patch by Pablo Galindo.
- bpo-43390: CPython now sets the `SA_ONSTACK` flag in`PyOS_setsig` for
the VMâs default signal handlers.  This is friendlier to other in-process
code that an extension module or embedding use could pull in (such as
Golangâs cgo) where tiny thread stacks are the norm and`sigaltstack()` has been used to provide for signal handlers.  This is a no-op change for
the vast majority of processes that donât use sigaltstack.
- bpo-43287: Speed up calls to `filter()` by using the**PEP 590**`vectorcall` calling convention. Patch by Donghee Na.
- bpo-37448: Add a radix tree based memory map to track in-use obmalloc arenas. Use to replace the old implementation of address_in_range(). The radix tree approach makes it easy to increase pool sizes beyond the OS page size. Boosting the pool and arena size allows obmalloc to handle a significantly higher percentage of requests from its ultra-fast paths. It also has the advantage of eliminating the memory unsanitary behavior of the previous address_in_range(). The old address_in_range() was marked with the annotations _Py_NO_SANITIZE_ADDRESS, _Py_NO_SANITIZE_THREAD, and _Py_NO_SANITIZE_MEMORY. Those annotations are no longer needed. To disable the radix tree map, set a preprocessor flag as follows: `-DWITH_PYMALLOC_RADIX_TREE=0` .Co-authored-by: Tim Peters <tim.peters@gmail.com>
- bpo-29988: Only handle asynchronous exceptions and requests to drop the GIL when returning from a call or on the back edges of loops. Makes sure that `__exit__()` is always called in with statements, even
for interrupts.

### LibraryÂ¶

- bpo-43720: Document various stdlib deprecations in imp, pkgutil, and importlib.util for removal in Python 3.12.
- bpo-43433: `xmlrpc.client.ServerProxy` no longer ignores query and
fragment in the URL of the server.
- bpo-31956: The `index()` method of`array.array` now has optional*start* and*stop* parameters.
- bpo-40066: Enum: adjust `repr()` to show only enum and member name (not
value, nor angle brackets) and`str()` to show only member name.  Update
and improve documentation to match.
- bpo-42136: Deprecate all module_repr() methods found in importlib as their use is being phased out by Python 3.12.
- bpo-35930: Raising an exception raised in a âfutureâ instance will create reference cycles.
- bpo-41369: Finish updating the vendored libmpdec to version 2.5.1. Patch by Stefan Krah.
- bpo-43422: Revert the _decimal C API which was added in bpo-41324.
- bpo-43577: Fix deadlock when using `ssl.SSLContext` debug callback
with`ssl.SSLContext.sni_callback()` .
- bpo-43571: Itâs now possible to create MPTCP sockets with IPPROTO_MPTCP
- bpo-43542: `image/heic` and`image/heif` were added to`mimetypes` .
- bpo-40645: The `hmac` module now uses OpenSSLâs HMAC implementation
when digestmod argument is a hash name or builtin hash function.
- bpo-43510: Implement **PEP 597** : Add`EncodingWarning` warning,```
-X
warn_default_encoding
```
option,`PYTHONWARNDEFAULTENCODING` environment variable and`encoding="locale"` argument value.
- bpo-43521: `ast.unparse` can now render NaNs and empty sets.
- bpo-42914: `pprint.pprint()` gains a new boolean`underscore_numbers` optional argument to emit integers with thousands
separated by an underscore character for improved readability (for example`1_000_000` instead of`1000000` ).
- bpo-41361: `rotate()` calls are now slightly faster
due to faster argument parsing.
- bpo-43423: `subprocess.communicate()` no longer raises an IndexError
when there is an empty stdout or stderr IO buffer during a timeout on
Windows.
- bpo-27820: Fixed long-standing bug of smtplib.SMTP where doing AUTH LOGIN with initial_response_ok=False will fail. The cause is that SMTP.auth_login _always_ returns a password if provided with a challenge string, thus non-compliant with the standard for AUTH LOGIN. Also fixes bug with the test for smtpd.
- bpo-43445: Add frozen modules to `sys.stdlib_module_names` . For
example, add`"_frozen_importlib"` and`"_frozen_importlib_external"` names.
- bpo-43245: Add keyword arguments support to `ChainMap.new_child()` .
- bpo-29982: Add optional parameter *ignore_cleanup_errors* to`tempfile.TemporaryDirectory()` and allow multiple`cleanup()` attempts. Contributed by C.A.M. Gerlach.
- bpo-43428: Include changes from importlib_metadata 3.7: Performance enhancements to distribution discovery. `entry_points` only returns unique distributions.Introduces new `EntryPoints` object for containing a set of entry points
with convenience methods for selecting entry points by group or name.`entry_points` now returns this object if selection parameters are
supplied but continues to return a dict object for compatibility. Users
are encouraged to rely on the selection interface. The dict object result
is likely to be deprecated in the future.Added packages_distributions function to return a mapping of packages to the distributions that provide them.
- bpo-43332: Improves the networking efficiency of `http.client` when
using a proxy via`set_tunnel()` .  Fewer small send
calls are made during connection setup.
- bpo-43420: Improve performance of `fractions.Fraction` arithmetics
for large components.  Contributed by Sergey B. Kirpichev.
- bpo-43356: Allow passing a signal number to `_thread.interrupt_main()` .
- bpo-43399: Fix `ElementTree.extend` not working on iterators when using
the Python implementation
- bpo-43369: Improve `sqlite3` error handling: If`sqlite3_column_text()` and`sqlite3_column_blob()` set`SQLITE_NOMEM` ,`MemoryError` is now raised. Patch by Erlend E.
Aasland.
- bpo-43368: Fix a regression introduced in PR 24562, where an empty bytestring was fetched as `None` instead of`b''` in`sqlite3` .
Patch by Mariusz Felisiak.
- bpo-41282: Fixed stacklevel of `DeprecationWarning` emitted from`import distutils` .
- bpo-42129: `importlib.resources` now honors namespace packages, merging
resources from each location in the namespace as introduced in`importlib_resources` 3.2 and including incidental changes through
5.0.3.
- bpo-43295: `datetime.datetime.strptime()` now raises`ValueError` instead of`IndexError` when matching`'z'` with the`%z` format
specifier.
- bpo-43125: Return empty string if base64mime.body_encode receive empty bytes
- bpo-43084: `curses.window.enclose()` returns now`True` or`False` (as was documented) instead of`1` or`0` .
- bpo-42994: Add MIME types for opus, AAC, 3gpp and 3gpp2
- bpo-14678: Add an invalidate_caches() method to the zipimport.zipimporter class to support importlib.invalidate_caches(). Patch by Desmond Cheong.
- bpo-42782: Fail fast in `shutil.move()` to avoid creating destination
directories on failure.
- bpo-40066: Enumâs `repr()` and`str()` have changed:`repr()` is now*EnumClass.MemberName* and`str()` is*MemberName* .  Additionally,
stdlib Enumâs whose contents are available as module attributes, such as`RegexFlag.IGNORECASE` , have their`repr()` as*module.name* , e.g.`re.IGNORECASE` .
- bpo-26053: Fixed bug where the `pdb` interactive run command echoed
the args from the shell command line, even if those have been overridden
at the pdb prompt.
- bpo-24160: Fixed bug where breakpoints did not persist across multiple debugger sessions in `pdb` âs interactive mode.
- bpo-40701: When the `tempfile.tempdir` global variable is set to a
value of type bytes, it is now handled consistently.  Previously
exceptions could be raised from some tempfile APIs when the directory did
not already exist in this situation.  Also ensures that the`tempfile.gettempdir()` and`tempfile.gettempdirb()` functions*always* return`str` and`bytes` respectively.
- bpo-39342: Expose `X509_V_FLAG_ALLOW_PROXY_CERTS` as`VERIFY_ALLOW_PROXY_CERTS` to allow proxy certificate
validation as explained in
https://docs.openssl.org/1.1.1/man7/proxy-certificates/.
- bpo-31861: Add builtins.aiter and builtins.anext. Patch by Joshua Bronson (@jab), Daniel Pope (@lordmauve), and Justin Wang (@justin39).

### DocumentationÂ¶

- bpo-43199: Answer âWhy is there no goto?â in the Design and History FAQ.
- bpo-43407: Clarified that a result from `time.monotonic()` ,`time.perf_counter()` ,`time.process_time()` , or`time.thread_time()` can be compared with the result from any
following call to the same function - not just the next immediate call.
- bpo-43354: Fix type documentation for `Fault.faultCode` ; the type has to
be`int` instead of`str` .
- bpo-41933: Clarified wording of s * n in the Common Sequence Operations

### TestsÂ¶

### BuildÂ¶

- bpo-43179: Introduce and correctly use ALIGNOF_X in place of SIZEOF_X for alignment-related code in optimized string routines. Patch by Jessica Clarke.
- bpo-43631: Update macOS, Windows, and CI to OpenSSL 1.1.1k.
- bpo-43617: Improve configure.ac: Check for presence of autoconf-archive package and remove our copies of M4 macros.
- bpo-43466: The `configure` script now supports`--with-openssl-rpath` option.
- bpo-43372: Use `_freeze_importlib` to generate code for the`__hello__` module. This approach ensures the code matches the
interpreter version.  Previously, PYTHON_FOR_REGEN was used to generate
the code, which might be wrong.  The marshal format for code objects has
changed with bpo-42246, commit 877df851. Update the code and the expected
code sizes in ctypes test_frozentable.

### WindowsÂ¶

### IDLEÂ¶

- bpo-42225: Document that IDLE can fail on Unix either from misconfigured IP masquerade rules or failure displaying complex colored (non-ascii) characters.

### C APIÂ¶

- bpo-43688: The limited C API is now supported if Python is built in debug mode (if the `Py_DEBUG` macro is defined). In the limited C API, the`Py_INCREF()` and`Py_DECREF()` functions are now implemented
as opaque function calls, rather than accessing directly the`PyObject.ob_refcnt` member, if Python is built in debug mode
and the`Py_LIMITED_API` macro targets Python 3.10 or newer. It became
possible to support the limited C API in debug mode because the`PyObject` structure is the same in release and debug mode since
Python 3.8 (see bpo-36465).The limited C API is still not supported in the `--with-trace-refs` special build (`Py_TRACE_REFS` macro).Patch by Victor Stinner.
- bpo-43244: Remove the `pyarena.h` header file with functions:
  - `PyArena_New()`
  - `PyArena_Free()`
  - `PyArena_Malloc()`
  - `PyArena_AddPyObject()`
 These functions were undocumented, excluded from the limited C API, and were only used internally by the compiler. Patch by Victor Stinner.
- bpo-43244: Remove the compiler and parser functions using `struct _mod` type, because the public AST C API was removed:
  - `PyAST_Compile()`
  - `PyAST_CompileEx()`
  - `PyAST_CompileObject()`
  - `PyFuture_FromAST()`
  - `PyFuture_FromASTObject()`
  - `PyParser_ASTFromFile()`
  - `PyParser_ASTFromFileObject()`
  - `PyParser_ASTFromFilename()`
  - `PyParser_ASTFromString()`
  - `PyParser_ASTFromStringObject()`
 These functions were undocumented and excluded from the limited C API. Patch by Victor Stinner.
- bpo-43244: Remove `ast.h` ,`asdl.h` , and`Python-ast.h` header
files. These functions were undocumented and excluded from the limited C
API. Most names defined by these header files were not prefixed by`Py` and so could create names conflicts. For example,`Python-ast.h` defined
a`Yield` macro which was conflict with the`Yield` name used by the
Windows`<winbase.h>` header. Use the Python`ast` module instead.
Patch by Victor Stinner.
- bpo-43541: Fix a `PyEval_EvalCodeEx()` regression: fix reference
counting on builtins. Patch by Victor Stinner.
- bpo-43244: Remove the `symtable.h` header file and the undocumented
functions:
  - `PyST_GetScope()`
  - `PySymtable_Build()`
  - `PySymtable_BuildObject()`
  - `PySymtable_Free()`
  - `Py_SymtableString()`
  - `Py_SymtableStringObject()`
 The `Py_SymtableString()` function was part the stable ABI by mistake
but it could not be used, because the`symtable.h` header file was
excluded from the limited C API.The Python `symtable` module remains available and is unchanged.Patch by Victor Stinner.
- bpo-43244: Remove the `PyAST_Validate()` function. It is no longer
possible to build a AST object (`mod_ty` type) with the public C API.
The function was already excluded from the limited C API (**PEP 384** ).
Patch by Victor Stinner.

## Python 3.10.0 alpha 6Â¶

*Release date: 2021-03-01*

### SecurityÂ¶

- bpo-42967: Fix web cache poisoning vulnerability by defaulting the query args separator to `&` , and allowing the user to choose a custom
separator.

### Core and BuiltinsÂ¶

- bpo-43321: Fix `SystemError` raised when`PyArg_Parse*()` is used with`#` but without`PY_SSIZE_T_CLEAN` defined.
- bpo-36346: `PyArg_Parse*()` functions now emits`DeprecationWarning` when`u` or`Z` format is used. See**PEP 623** for detail.
- bpo-43277: Add a new `PySet_CheckExact()` function to the C-API to
check if an object is an instance of`set` but not an instance of a
subtype. Patch by Pablo Galindo.
- bpo-42990: The `types.FunctionType` constructor now inherits the
current builtins if the*globals* dictionary has no`"__builtins__"` key, rather than using`{"None": None}` as builtins: same behavior as`eval()` and`exec()` functions. Defining a function with```
def
function(...): ...
```
in Python is not affected, globals cannot be
overridden with this syntax: it also inherits the current builtins. Patch
by Victor Stinner.
- bpo-42990: Functions have a new `__builtins__` attribute which is used
to look for builtin symbols when a function is executed, instead of
looking into`__globals__['__builtins__']` . Patch by Mark Shannon and
Victor Stinner.
- bpo-43149: Improve the error message in the parser for exception groups without parentheses. Patch by Pablo Galindo.
- bpo-43121: Fixed an incorrect `SyntaxError` message for missing comma
in literals. Patch by Pablo Galindo.

### LibraryÂ¶

- bpo-42819: `readline` : Explicitly disable bracketed paste in the
interactive interpreter, even if itâs set in the inputrc, is enabled by
default (eg GNU Readline 8.1), or a user calls`readline.read_init_file()` . The Python REPL has not implemented
bracketed paste support. Also, bracketed mode writes the`"\x1b[?2004h"` escape sequence into stdout which causes test failures in applications
that donât support it. It can still be explicitly enabled by calling`readline.parse_and_bind("set enable-bracketed-paste on")` . Patch by
Dustin Rodrigues.

### Core and BuiltinsÂ¶

- bpo-42808: Simple calls to `type(object)` are now faster due to the`vectorcall` calling convention. Patch by Dennis Sweeney.
- bpo-42217: Make the compiler merges same co_code and co_linetable objects in a module like already did for co_consts.
- bpo-41972: Substring search functions such as `str1 in str2` and`str2.find(str1)` now sometimes use the âTwo-Wayâ string comparison
algorithm to avoid quadratic behavior on long strings.
- bpo-42128: Implement **PEP 634** (structural pattern matching). Patch by
Brandt Bucher.

### LibraryÂ¶

- bpo-40692: In the `concurrent.futures.ProcessPoolExecutor` ,
validate that`multiprocess.synchronize()` is available on a given
platform and rely on that check in the`concurrent.futures` test
suite so we can run tests that are unrelated to`ProcessPoolExecutor` on those platforms.

### Core and BuiltinsÂ¶

- bpo-38302: If `object.__ipow__()` returns`NotImplemented` , the
operator will correctly fall back to`object.__pow__()` and`object.__rpow__()` as expected.

### LibraryÂ¶

- bpo-43316: The `python -m gzip` command line application now properly
fails when detecting an unsupported extension. It exits with a non-zero
exit code and prints an error message to stderr.
- bpo-43317: Set the chunk size for the `gzip` module main function to
io.DEFAULT_BUFFER_SIZE. This is slightly faster than the 1024 bytes
constant that was used previously.
- bpo-43146: Handle None in single-arg versions of `print_exception()` and`format_exception()` .
- bpo-43260: Fix TextIOWrapper can not flush internal buffer forever after very large text is written.
- bpo-43258: Prevent needless allocation of `sqlite3` aggregate
function context when no rows match an aggregate query. Patch by Erlend E.
Aasland.
- bpo-43251: Improve `sqlite3` error handling:`sqlite3_column_name()` failures now result in`MemoryError` . Patch
by Erlend E. Aasland.
- bpo-40956: Fix segfault in `sqlite3.Connection.backup()` if no
argument was provided. The regression was introduced by PR 23838. Patch by
Erlend E. Aasland.
- bpo-43172: The readline module now passes its tests when built directly against libedit. Existing irreconcilable API differences remain in `readline.get_begidx()` and`readline.get_endidx()` behavior based
on libreadline vs libedit use.
- bpo-43163: Fix a bug in `codeop` that was causing it to not ask for
more input when multi-line snippets have unclosed parentheses. Patch by
Pablo Galindo
- bpo-43162: deprecate unsupported ability to access enum members as attributes of other enum members
- bpo-43146: Fix recent regression in None argument handling in `traceback` module functions.
- bpo-43102: The namedtuple __new__ method had its __builtins__ set to None instead of an actual dictionary. This created problems for introspection tools.
- bpo-43106: Added `O_EVTONLY` ,`O_FSYNC` ,`O_SYMLINK` and`O_NOFOLLOW_ANY` for macOS. Patch by
Donghee Na.
- bpo-42960: Adds `resource.RLIMIT_KQUEUES` constant from FreeBSD to
the`resource` module.
- bpo-42151: Make the pure Python implementation of `xml.etree.ElementTree` behave the same as the C implementation
(`_elementree` ) regarding default attribute values (by not setting`specified_attributes=1` ).
- bpo-29753: In ctypes, now packed bitfields are calculated properly and the first item of packed bitfields is now shrank correctly.

### DocumentationÂ¶

### TestsÂ¶

- bpo-43288: Fix test_importlib to correctly skip Unicode file tests if the filesystem does not support them.

### BuildÂ¶

- bpo-43174: Windows build now uses `/utf-8` compiler option.
- bpo-43103: Add a new configure `--without-static-libpython` option to
not build the`libpythonMAJOR.MINOR.a` static library and not install
the`python.o` object file.
- bpo-13501: The configure script can now use *libedit* instead of*readline* with the command line option`--with-readline=editline` .
- bpo-42603: Make configure script use pkg-config to detect the location of Tcl/Tk headers and libraries, used to build tkinter. On macOS, a Tcl/Tk configuration provided by pkg-config will be preferred over Tcl/Tk frameworks installed in `/{System/,}Library/Frameworks` . If
both exist and the latter is preferred, the appropriate`--with-tcltk-*` configuration options need to be explicitly set.
