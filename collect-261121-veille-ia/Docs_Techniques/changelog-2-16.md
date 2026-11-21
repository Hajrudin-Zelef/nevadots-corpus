---
id: collect-261121-veille-ia/docs-techniques/changelog-2-16
title: "Changelog"
domain: docs-techniques
role: reference
task: article
actors: []
dates: ["2019-02-03"]
keywords: ["python", "module", "patch", "longer", "fixed", "error", "default", "test"]
source: https://docs.python.org/release/3.15.1/whatsnew/changelog.html
source_anchor: ""
source_lines: [22501, 24000]
section: "Docs_Techniques"
---
## Python 3.8.0 alpha 1Â¶

*Release date: 2019-02-03*

### SecurityÂ¶

- bpo-35746: **CVE 2019-5010** : Fix a NULL pointer deref in ssl module. The
cert parser did not handle CRL distribution points with empty DP or URI
correctly. A malicious or buggy certificate can result into segfault.
Vulnerability (TALOS-2018-0758) reported by Colin Read and Nicolas Edet of
Cisco.
- bpo-34812: The `-I` command line option (run Python in isolated
mode) is now also copied by the`multiprocessing` and`distutils` modules when spawning child processes. Previously, only`-E` and`-s` options (enabled by`-I` ) were copied.
- bpo-34791: The xml.sax and xml.dom.domreg no longer use environment variables to override parser implementations when sys.flags.ignore_environment is set by -E or -I arguments.
- bpo-17239: The xml.sax and xml.dom.minidom parsers no longer processes external entities by default. External DTD and ENTITY declarations no longer load files or create network connections.
- bpo-34623: **CVE 2018-14647** : The C accelerated _elementtree module now
initializes hash randomization salt from _Py_HashSecret instead of
libexpatâs default CSPRNG.
- bpo-34405: Updated to OpenSSL 1.1.0i for Windows builds.
- bpo-33871: Fixed sending the part of the file in `os.sendfile()` on
macOS.  Using the*trailers* argument could cause sending more bytes from
the input file than was specified.
- bpo-32533: Fixed thread-safety of error handling in _ssl.
- bpo-33136: Harden ssl module against LibreSSL **CVE 2018-8970** .
X509_VERIFY_PARAM_set1_host() is called with an explicit namelen. A new
test ensures that NULL bytes are not allowed.
- bpo-33001: Minimal fix to prevent buffer overrun in os.symlink on Windows
- bpo-32981: Regexes in difflib and poplib were vulnerable to catastrophic backtracking. These regexes formed potential DOS vectors (REDOS). They have been refactored. This resolves **CVE 2018-1060** and**CVE 2018-1061** .
Patch by Jamie Davis.
- bpo-28414: The ssl module now allows users to perform their own IDN en/decoding when using SNI.

### Core and BuiltinsÂ¶

- bpo-35877: Make parenthesis optional for named expressions in while statement. Patch by Karthikeyan Singaravelan.
- bpo-35814: Allow same right hand side expressions in annotated assignments as in normal ones. In particular, `x: Tuple[int, int] = 1, 2` (without
parentheses on the right) is now allowed.
- bpo-35766: Add the option to parse PEP 484 type comments in the ast module. (Off by default.) This is merging the key functionality of the third party fork thereof, [typed_ast](https://github.com/python/typed_ast).
- bpo-35713: Reorganize Python initialization to get working exceptions and sys.stderr earlier.
- bpo-33416: Add end line and end column position information to the Python AST nodes. This is a C-level backwards incompatible change.
- bpo-35720: Fixed a minor memory leak in pymain_parse_cmdline_impl function in Modules/main.c
- bpo-35634: `func(**kwargs)` will now raise an error when`kwargs` is a
mapping containing multiple entries with the same key. An error was
already raised when other keyword arguments are passed before`**kwargs` since Python 3.6.
- bpo-35623: Fix a crash when sorting very long lists. Patch by Stephan Hohe.
- bpo-35214: clang Memory Sanitizer build instrumentation was added to work around false positives from posix, socket, time, test_io, and test_faulthandler.
- bpo-35560: Fix an assertion error in `format()` in debug build for
floating-point formatting with ânâ format, zero padding and small width.
Release build is not impacted. Patch by Karthikeyan Singaravelan.
- bpo-35552: Format characters `%s` and`%V` in`PyUnicode_FromFormat()` and`%s` in`PyBytes_FromFormat()` no longer read memory past the limit if*precision* is specified.
- bpo-35504: Fix segfaults and `SystemError` s when deleting certain
attributes. Patch by Zackery Spytz.
- bpo-35504: Fixed a SystemError when delete the characters_written attribute of an OSError.
- bpo-35494: Improved syntax error messages for unbalanced parentheses in f-string.
- bpo-35444: Fixed error handling in pickling methods when fail to look up builtin âgetattrâ. Sped up pickling iterators.
- bpo-35436: Fix various issues with memory allocation error handling. Patch by Zackery Spytz.
- bpo-35423: Separate the signal handling trigger in the eval loop from the âpending callsâ machinery. There is no semantic change and the difference in performance is insignificant.
- bpo-35357: Internal attributesâ names of unittest.mock._Call and unittest.mock.MagicProxy (name, parent & from_kall) are now prefixed with _mock_ in order to prevent clashes with widely used object attributes. Fixed minor typo in test function name.
- bpo-35372: Fixed the code page decoder for input longer than 2 GiB containing undecodable bytes.
- bpo-35336: Fix PYTHONCOERCECLOCALE=1 environment variable: only coerce the C locale if the LC_CTYPE locale is âCâ.
- bpo-31241: The *lineno* and*col_offset* attributes of AST nodes for list
comprehensions, generator expressions and tuples are now point to the
opening parenthesis or square brace. For tuples without parenthesis they
point to the position of the first item.
- bpo-33954: For `str.format()` ,`float.__format__()` and`complex.__format__()` methods for non-ASCII decimal point when using
the ânâ formatter.
- bpo-35269: Fix a possible segfault involving a newly created coroutine. Patch by Zackery Spytz.
- bpo-35224: Implement **PEP 572** (assignment expressions). Patch by Emily
Morehouse.
- bpo-32492: Speed up `namedtuple()` attribute access by 1.6x using a C
fast-path for the name descriptors. Patch by Pablo Galindo.
- bpo-35214: Fixed an out of bounds memory access when parsing a truncated unicode escape sequence at the end of a string such as `'\N'` .  It would
read one byte beyond the end of the memory allocation.
- bpo-35214: The interpreter and extension modules have had annotations added so that they work properly under clangâs Memory Sanitizer. A new configure flag âwith-memory-sanitizer has been added to make test builds of this nature easier to perform.
- bpo-35193: Fix an off by one error in the bytecode peephole optimizer where it could read bytes beyond the end of bounds of an array when removing unreachable code. This bug was present in every release of Python 3.6 and 3.7 until now.
- bpo-35169: Improved error messages for forbidden assignments.
- bpo-34022: Fix handling of hash-based bytecode files in `zipimport` .
Patch by Elvis Pranskevichus.
- bpo-28401: Debug builds will no longer to attempt to import extension modules built for the ABI as they were never compatible to begin with. Patch by Stefano Rivera.
- bpo-29341: Clarify in the docstrings of `os` methods that path-like
objects are also accepted as input parameters.
- bpo-35050: `socket` : Fix off-by-one bug in length check for`AF_ALG` name and type.
- bpo-29743: Raise `ValueError` instead of`OverflowError` in case
of a negative`_length_` in a`ctypes.Array` subclass.  Also
raise`TypeError` instead of`AttributeError` for non-integer`_length_` . Original patch by Oren Milman.
- bpo-16806: Fix `lineno` and`col_offset` for multi-line string tokens.
- bpo-35029: `SyntaxWarning` raised as an exception at code generation
time will be now replaced with a`SyntaxError` for better error
reporting.
- bpo-34983: Expose `symtable.Symbol.is_nonlocal()` in the symtable
module. Patch by Pablo Galindo.
- bpo-34974: `bytes` and`bytearray` constructors no longer
convert unexpected exceptions (e.g.`MemoryError` and`KeyboardInterrupt` ) to`TypeError` .
- bpo-34939: Allow annotated names in module namespace that are declared global before the annotation happens. Patch by Pablo Galindo.
- bpo-34973: Fixed crash in `bytes()` when the`list` argument is
mutated while it is iterated.
- bpo-34876: The *lineno* and*col_offset* attributes of the AST for
decorated function and class refer now to the position of the
corresponding`def` ,`async def` and`class` instead of the position
of the first decorator. This leads to more correct line reporting in
tracing. This is the only case when the position of child AST nodes can
precede the position of the parent AST node.
- bpo-34879: Fix a possible null pointer dereference in bytesobject.c. Patch by Zackery Spytz.
- bpo-34784: Fix the implementation of PyStructSequence_NewType in order to create heap allocated StructSequences.
- bpo-32912: A `SyntaxWarning` is now emitted instead of a`DeprecationWarning` for invalid escape sequences in string and bytes
literals.
- bpo-34854: Fixed a crash in compiling string annotations containing a lambda with a keyword-only argument that doesnât have a default value.
- bpo-34850: The compiler now produces a `SyntaxWarning` when identity
checks (`is` and`is not` ) are used with certain types of literals
(e.g. strings, ints).  These can often work by accident in CPython, but
are not guaranteed by the language spec.  The warning advises users to use
equality tests (`==` and`!=` ) instead.
- bpo-34824: Fix a possible null pointer dereference in Modules/_ssl.c. Patch by Zackery Spytz.
- bpo-30156: The C function `property_descr_get()` uses a âcachedâ tuple
to optimize function calls. But this tuple can be discovered in debug mode
with`sys.getobjects()` . Remove the optimization, itâs not really
worth it and it causes 3 different crashes last years.
- bpo-34762: Fix contextvars C API to use PyObject* pointer types.
- bpo-34751: The hash function for tuples is now based on xxHash which gives better collision results on (formerly) pathological cases. Additionally, on 64-bit systems it improves tuple hashes in general. Patch by Jeroen Demeyer with substantial contributions by Tim Peters.
- bpo-34735: Fix a memory leak in Modules/timemodule.c. Patch by Zackery Spytz.
- bpo-34683: Fixed a bug where some SyntaxError error pointed to locations that were off-by-one.
- bpo-34651: Only allow the main interpreter to fork. The avoids the possibility of affecting the main interpreter, which is critical to operation of the runtime.
- bpo-34653: Remove unused function PyParser_SimpleParseStringFilename.
- bpo-32236: Warn that line buffering is not supported if `open()` is
called with binary mode and`buffering=1` .
- bpo-34641: Further restrict the syntax of the left-hand side of keyword arguments in function calls. In particular, `f((keyword)=arg)` is now
disallowed.
- bpo-34637: Make the *start* argument to*sum()* visible as a keyword
argument.
- bpo-1621: Do not assume signed integer overflow behavior (C undefined behavior) when performing set hash table resizing.
- bpo-34588: Fix an off-by-one in the recursive call pruning feature of traceback formatting.
- bpo-34485: On Windows, the LC_CTYPE is now set to the user preferred locale at startup. Previously, the LC_CTYPE locale was âCâ at startup, but changed when calling setlocale(LC_CTYPE, ââ) or setlocale(LC_ALL, ââ).
- bpo-34485: Standard streams like sys.stdout now use the âsurrogateescapeâ error handler, instead of âstrictâ, on the POSIX locale (when the C locale is not coerced and the UTF-8 Mode is disabled).
- bpo-34485: Fix the error handler of standard streams like sys.stdout: PYTHONIOENCODING=â:â is now ignored instead of setting the error handler to âstrictâ.
- bpo-34485: Python now gets the locale encoding with C code to initialize the encoding of standard streams like sys.stdout. Moreover, the encoding is now initialized to the Python codec name to get a normalized encoding name and to ensure that the codec is loaded. The change avoids importing _bootlocale and _locale modules at startup by default.
- bpo-34527: On FreeBSD, Py_DecodeLocale() and Py_EncodeLocale() now also forces the ASCII encoding if the LC_CTYPE locale is âPOSIXâ, not only if the LC_CTYPE locale is âCâ.
- bpo-34527: The UTF-8 Mode is now also enabled by the âPOSIXâ locale, not only by the âCâ locale.
- bpo-34403: On HP-UX with C or POSIX locale, sys.getfilesystemencoding() now returns âasciiâ instead of âroman8â (when the UTF-8 Mode is disabled and the C locale is not coerced).
- bpo-34523: The Python filesystem encoding is now read earlier during the Python initialization.
- bpo-12458: Tracebacks show now correct line number for subexpressions in multiline expressions. Tracebacks show now the line number of the first line for multiline expressions instead of the line number of the last subexpression.
- bpo-34408: Prevent a null pointer dereference and resource leakage in `PyInterpreterState_New()` .
- bpo-34400: Fix undefined behavior in parsetok.c. Patch by Zackery Spytz.
- bpo-33073: Added as_integer_ratio to ints to make them more interoperable with floats.
- bpo-34377: Update valgrind suppression list to use `_PyObject_Free` /`_PyObject_Realloc` instead of`PyObject_Free` /`PyObject_Realloc` .
- bpo-34353: Added the âsocketâ option in the `stat.filemode()` Python
implementation to match the C implementation.
- bpo-34320: Fix `dict(od)` didnât copy iteration order of OrderedDict.
- bpo-34113: Fixed crash on debug builds when opcode stack was adjusted with negative numbers. Patch by Constantin Petrisor.
- bpo-34100: Compiler now merges constants in tuples and frozensets recursively. Code attributes like `co_names` are merged too.
- bpo-34151: Performance of list concatenation, repetition and slicing operations is slightly improved. Patch by Sergey Fedoseev.
- bpo-34170: -X dev: it is now possible to override the memory allocator using PYTHONMALLOC even if the developer mode is enabled.
- bpo-33237: Improved `AttributeError` message for partially
initialized module.
- bpo-34149: Fix min and max functions to get default behavior when key is None.
- bpo-34125: Profiling of unbound built-in methods now works when `**kwargs` is given.
- bpo-34141: Optimized pickling atomic types (None, bool, int, float, bytes, str).
- bpo-34126: Fix crashes when profiling certain invalid calls of unbound methods. Patch by Jeroen Demeyer.
- bpo-24618: Fixed reading invalid memory when create the code object with too small varnames tuple or too large argument counts.
- bpo-34068: In `io.IOBase.close()` , ensure that the`closed` attribute is not set with a live exception.
Patch by Zackery Spytz and Serhiy Storchaka.
- bpo-34087: Fix buffer overflow while converting unicode to numeric values.
- bpo-34080: Fixed a memory leak in the compiler when it raised some uncommon errors during tokenizing.
- bpo-34066: Disabled interruption by Ctrl-C between calling `open()` and
entering a**with** block in`with open()` .
- bpo-34042: Fix dict.copy() to maintain correct total refcount (as reported by sys.gettotalrefcount()).
- bpo-33418: Fix potential memory leak in function object when it creates reference cycle.
- bpo-33985: Implement contextvars.ContextVar.name attribute.
- bpo-33956: Update vendored Expat library copy to version 2.2.5.
- bpo-24596: Decref the module object in `PyRun_SimpleFileExFlags()` before calling`PyErr_Print()` .  Patch by Zackery Spytz.
- bpo-33451: Close directly executed pyc files before calling `PyEval_EvalCode()` .
- bpo-1617161: The hash of `BuiltinMethodType` instances (methods of
built-in classes) now depends on the hash of the identity of*__self__* instead of its value. The hash and equality of`ModuleType` and`MethodWrapperType` instances (methods of user-defined classes and
some methods of built-in classes like`str.__add__` ) now depend on the
hash and equality of the identity of*__self__* instead of its value.`MethodWrapperType` instances no longer support ordering.
- bpo-33824: Fix âLC_ALL=C python3.7 -Vâ: reset properly the command line parser when the encoding changes after reading the Python configuration.
- bpo-33803: Fix a crash in hamt.c caused by enabling GC tracking for an object that hadnât all of its fields set to NULL.
- bpo-33738: Seven macro incompatibilities with the Limited API were fixed, and the macros `PyIter_Check()` ,`PyIndex_Check()` and`PyExceptionClass_Name()` were added as functions. A script for
automatic macro checks was added.
- bpo-33786: Fix asynchronous generators to handle GeneratorExit in athrow() correctly
- bpo-30167: `PyRun_SimpleFileExFlags` removes`__cached__` from module
in addition to`__file__` .
- bpo-33706: Fix a crash in Python initialization when parsing the command line options. Thanks Christoph Gohlke for the bug report and the fix!
- bpo-33597: Reduce `PyGC_Head` size from 3 words to 2 words.
- bpo-30654: Fixed reset of the SIGINT handler to SIG_DFL on interpreter shutdown even when there was a custom handler set previously. Patch by Philipp Kerling.
- bpo-33622: Fixed a leak when the garbage collector fails to add an object with the `__del__` method or referenced by it into the`gc.garbage` list.`PyGC_Collect()` can now be called when an
exception is set and preserves it.
- bpo-33462: Make dict and dict views reversible. Patch by RÃ©mi Lapeyre.
- bpo-23722: A `RuntimeError` is now raised when the custom metaclass
doesnât provide the`__classcell__` entry in the namespace passed to`type.__new__` .  A`DeprecationWarning` was emitted in Python
3.6â3.7.
- bpo-33499: Add `PYTHONPYCACHEPREFIX` environment variable and`-X``pycache_prefix` command-line option to set an alternate
root directory for writing module bytecode cache files.
- bpo-25711: The `zipimport` module has been rewritten in pure Python.
- bpo-33509: Fix module_globals parameter of warnings.warn_explicit(): donât crash if module_globals is not a dict.
- bpo-31849: Fix signed/unsigned comparison warning in pyhash.c.
- bpo-33475: Fixed miscellaneous bugs in converting annotations to strings and optimized parentheses in the string representation.
- bpo-20104: Added support for the `setpgroup` ,`resetids` ,`setsigmask` ,`setsigdef` and`scheduler` parameters of`posix_spawn` . Patch by Pablo Galindo.
- bpo-33391: Fix a leak in set_symmetric_difference().
- bpo-33363: Raise a SyntaxError for `async with` and`async for` statements outside of async functions.
- bpo-28055: Fix unaligned accesses in siphash24(). Patch by Rolf Eike Beer.
- bpo-33128: Fix a bug that causes PathFinder to appear twice on sys.meta_path. Patch by Pablo Galindo Salgado.
- bpo-33331: Modules imported last are now cleared first at interpreter shutdown.
- bpo-33312: Fixed clang ubsan (undefined behavior sanitizer) warnings in dictobject.c by adjusting how the internal struct _dictkeysobject shared keys structure is declared.
- bpo-33305: Improved syntax error messages for invalid numerical literals.
- bpo-33306: Improved syntax error messages for unbalanced parentheses.
- bpo-33234: The list constructor will pre-size and not over-allocate when the input length is known.
- bpo-33270: Intern the names for all anonymous code objects. Patch by Zackery Spytz.
- bpo-30455: The C and Python code and the documentation related to tokens are now generated from a single source file `Grammar/Tokens` .
- bpo-33176: Add a `toreadonly()` method to memoryviews.
- bpo-33231: Fix potential memory leak in `normalizestring()` .
- bpo-33205: Change dict growth function from `round_up_to_power_2(used*2+hashtable_size/2)` to`round_up_to_power_2(used*3)` .  Previously, dict is shrunk only when`used == 0` . Now dict has more chance to be shrunk.
- bpo-29922: Improved error messages in âasync withâ when `__aenter__()` or`__aexit__()` return non-awaitable object.
- bpo-33199: Fix `ma_version_tag` in dict implementation is uninitialized
when copying from key-sharing dict.
- bpo-33053: When using the -m switch, sys.path[0] is now explicitly expanded as the *starting* working directory, rather than being left as
the empty path (which allows imports from the current working directory at
the time of the import)
- bpo-33138: Changed standard error message for non-pickleable and non-copyable types. It now says âcannot pickleâ instead of âcanât pickleâ or âcannot serializeâ.
- bpo-33018: Improve consistency of errors raised by `issubclass()` when
called with a non-class and an abstract base class as the first and second
arguments, respectively. Patch by Josh Bronson.
- bpo-33083: `math.factorial` no longer accepts arguments that are not
int-like. Patch by Pablo Galindo.
- bpo-33041: Added new opcode `END_ASYNC_FOR` and fixes the
following issues:
  - Setting global `StopAsyncIteration` no longer breaks`async for` loops.
  - Jumping into an `async for` loop is now disabled.
  - Jumping out of an `async for` loop no longer corrupts the stack.
- bpo-25750: Fix rare Python crash due to bad refcounting in `type_getattro()` if a descriptor deletes itself from the class. Patch
by Jeroen Demeyer.
- bpo-33041: Fixed bytecode generation for âasync forâ with a complex target. A StopAsyncIteration raised on assigning or unpacking will be now propagated instead of stopping the iteration.
- bpo-33026: Fixed jumping out of âwithâ block by setting f_lineno.
- bpo-33005: Fix a crash on fork when using a custom memory allocator (ex: using PYTHONMALLOC env var). _PyGILState_Reinit() and _PyInterpreterState_Enable() now use the default RAW memory allocator to allocate a new interpreters mutex on fork.
- bpo-32911: Due to unexpected compatibility issues discovered during downstream beta testing, reverted bpo-29463. `docstring` field is
removed from Module, ClassDef, FunctionDef, and AsyncFunctionDef ast nodes
which was added in 3.7a1.  Docstring expression is restored as a first
statement in their body. Based on patch by Inada Naoki.
- bpo-17288: Prevent jumps from âreturnâ and âexceptionâ trace events.
- bpo-32946: Importing names from already imported module with âfrom â¦ import â¦â is now 30% faster if the module is not a package.
- bpo-32932: Make error message more revealing when there are non-str objects in `__all__` .
- bpo-32925: Optimized iterating and containing test for literal lists consisting of non-constants: `x in [a, b]` and`for x in [a, b]` . The
case of all constant elements already was optimized.
- bpo-32889: Update Valgrind suppression list to account for the rename of `Py_ADDRESS_IN_RANG` to`address_in_range` .
- bpo-32836: Donât use temporary variables in cases of list/dict/set comprehensions
- bpo-31356: Remove the new API added in bpo-31356 (gc.ensure_disabled() context manager).
- bpo-32305: For namespace packages, ensure that both `__file__` and`__spec__.origin` are set to None.
- bpo-32303: Make sure `__spec__.loader` matches`__loader__` for
namespace packages.
- bpo-32711: Fix the warning messages for Python/ast_unparse.c. Patch by StÃ©phane Wirtel
- bpo-32583: Fix possible crashing in builtin Unicode decoders caused by write out-of-bound errors when using customized decode error handlers.
- bpo-32489: A `continue` statement is now allowed in the`finally` clause.
- bpo-17611: Simplified the interpreter loop by moving the logic of unrolling the stack of blocks into the compiler. The compiler emits now explicit instructions for adjusting the stack of values and calling the cleaning up code for `break` ,`continue` and`return` .Removed opcodes `BREAK_LOOP` ,`CONTINUE_LOOP` ,`SETUP_LOOP` and`SETUP_EXCEPT` . Added new opcodes`ROT_FOUR` ,`BEGIN_FINALLY` and`CALL_FINALLY` and`POP_FINALLY` . Changed the behavior of`END_FINALLY` and`WITH_CLEANUP_START` .
- bpo-32285: New function unicodedata.is_normalized, which can check whether a string is in a specific normal form.
- bpo-10544: Yield expressions are now disallowed in comprehensions and generator expressions except the expression for the outermost iterable.
- bpo-32117: Iterable unpacking is now allowed without parentheses in yield and return statements, e.g. `yield 1, 2, 3, *rest` . Thanks to David
Cuthbert for the change and Jordan Chapman for added tests.
- bpo-31902: Fix the `col_offset` attribute for ast nodes`ast.AsyncFor` ,`ast.AsyncFunctionDef` , and`ast.AsyncWith` .
Previously,`col_offset` pointed to the keyword after`async` .
- bpo-25862: Fix assertion failures in the `tell()` method of`io.TextIOWrapper` . Patch by Zackery Spytz.
- bpo-21983: Fix a crash in `ctypes.cast()` in case the type argument is a
ctypes structured data type. Patch by Eryk Sun and Oren Milman.
- bpo-31577: Fix a crash in `os.utime()` in case of a bad ns argument.
Patch by Oren Milman.
- bpo-29832: Remove references to âgetsockaddrargâ from various socket error messages. Patch by Oren Milman.

### LibraryÂ¶

- bpo-35845: Add âorderâ parameter to memoryview.tobytes().
- bpo-35864: The _asdict() method for collections.namedtuple now returns a regular dict instead of an OrderedDict.
- bpo-35537: An ExitStack is now used internally within subprocess.Popen to clean up pipe file handles. No behavior change in normal operation. But if closing one handle were ever to cause an exception, the others will now be closed instead of leaked. (patch by Giampaolo Rodola)
- bpo-35847: RISC-V needed the CTYPES_PASS_BY_REF_HACK. Fixes ctypes Structure test_pass_by_value.
- bpo-35813: Shared memory submodule added to multiprocessing to avoid need for serialization between processes
- bpo-35780: Fix lru_cache() errors arising in recursive, reentrant, or multi-threaded code. These errors could result in orphan links and in the cache being trapped in a state with fewer than the specified maximum number of links. Fix handling of negative maxsize which should have been treated as zero. Fix errors in toggling the âfullâ status flag. Fix misordering of links when errors are encountered. Sync-up the C code and pure Python code for the space saving path in functions with a single positional argument. In this common case, the space overhead of an lru cache entry is reduced by almost half. Fix counting of cache misses. In error cases, the miss count was out of sync with the actual number of times the underlying user function was called.
- bpo-35537: `os.posix_spawn()` and`os.posix_spawnp()` now have a*setsid* parameter.
- bpo-23846: `asyncio.ProactorEventLoop` now catches and logs send
errors when the self-pipe is full.
- bpo-34323: `asyncio` : Enhance`IocpProactor.close()` log: wait 1
second before the first log, then log every second. Log also the number of
seconds since`close()` was called.
- bpo-35674: Add a new `os.posix_spawnp()` function. Patch by Joannah
Nanjekye.
- bpo-35733: `ast.Constant(boolean)` no longer an instance of`ast.Num` .  Patch by Anthony Sottile.
- bpo-35726: QueueHandler.prepare() now makes a copy of the record before modifying and enqueueing it, to avoid affecting other handlers in the chain.
- bpo-35719: Sped up multi-argument `math` functions atan2(),
copysign(), remainder() and hypot() by 1.3â2.5 times.
- bpo-35717: Fix KeyError exception raised when using enums and compile. Patch contributed by RÃ©mi Lapeyre.
- bpo-35699: Fixed detection of Visual Studio Build Tools 2017 in distutils
- bpo-32710: Fix memory leaks in asyncio ProactorEventLoop on overlapped operation failure.
- bpo-35702: The `time.CLOCK_UPTIME_RAW` constant is now available
for macOS 10.12.
- bpo-32710: Fix a memory leak in asyncio in the ProactorEventLoop when `ReadFile()` or`WSASend()` overlapped operation fail immediately:
release the internal buffer.
- bpo-35682: Fix `asyncio.ProactorEventLoop.sendfile()` : donât attempt to
set the result of an internal future if itâs already done.
- bpo-35283: Add a deprecated warning for the `threading.Thread.isAlive()` method. Patch by Donghee Na.
- bpo-35664: Improve operator.itemgetter() performance by 33% with optimized argument handling and with adding a fast path for the common case of a single non-negative integer index into a tuple (which is the typical use case in the standard library).
- bpo-35643: Fixed a SyntaxWarning: invalid escape sequence in Modules/_sha3/cleanup.py. Patch by MickaÃ«l Schoentgen.
- bpo-35619: Improved support of custom data descriptors in `help()` and`pydoc` .
- bpo-28503: The `crypt` module now internally uses the`crypt_r()` library function instead of`crypt()` when available.
- bpo-35614: Fixed help() on metaclasses. Patch by Sanyam Khurana.
- bpo-35568: Expose `raise(signum)` as`raise_signal`
- bpo-35588: The floor division and modulo operations and the `divmod()` function on`fractions.Fraction` types are 2â4x faster. Patch by
Stefan Behnel.
- bpo-35585: Speed-up building enums by value, e.g. http.HTTPStatus(200).
- bpo-30561: random.gammavariate(1.0, beta) now computes the same result as random.expovariate(1.0 / beta). This synchronizes the two algorithms and eliminates some idiosyncrasies in the old implementation. It does however produce a difference stream of random variables than it used to.
- bpo-35537: The `subprocess` module can now use the`os.posix_spawn()` function in some cases for better performance.
- bpo-35526: Delaying the âjokeâ of barry_as_FLUFL.mandatory to Python version 4.0
- bpo-35523: Remove `ctypes` callback workaround: no longer create a
callback at startup. Avoid SELinux alert on`import ctypes` and```
import
uuid
```
.
- bpo-31784: `uuid.uuid1()` now calls`time.time_ns()` rather than`int(time.time() * 1e9)` .
- bpo-35513: `TextTestRunner` of`unittest.runner` now uses`time.perf_counter()` rather than`time.time()` to measure the execution time of a test:`time.time()` can go backwards, whereas`time.perf_counter()` is
monotonic.
- bpo-35502: Fixed reference leaks in `xml.etree.ElementTree.TreeBuilder` in case of unfinished building
of the tree (in particular when an error was raised during parsing XML).
- bpo-35348: Make `platform.architecture()` parsing of`file` command
output more reliable: add the`-b` option to the`file` command to
omit the filename, force the usage of the C locale, and search also the
âshared objectâ pattern.
- bpo-35491: `multiprocessing` : Add`Pool.__repr__()` and enhance`BaseProcess.__repr__()` (add pid and parent pid) to ease debugging.
Pool state constant values are now strings instead of integers, for
example`RUN` value becomes`'RUN'` instead of`0` .
- bpo-35477: `multiprocessing.Pool.__enter__()` now fails if the pool is
not running:`with pool:` fails if used more than once.
- bpo-31446: Copy command line that was passed to CreateProcessW since this function can change the content of the input buffer.
- bpo-35471: Python 2.4 dropped MacOS 9 support. The macpath module was deprecated in Python 3.7. The module is now removed.
- bpo-23057: Unblock Proactor event loop when keyboard interrupt is received on Windows
- bpo-35052: Fix xml.dom.minidom cloneNode() on a document with an entity: pass the correct arguments to the user data handler of an entity.
- bpo-20239: Allow repeated assignment deletion of `unittest.mock.Mock` attributes. Patch by Pablo Galindo.
- bpo-17185: Set `__signature__` on mock for`inspect` to get
signature. Patch by Karthikeyan Singaravelan.
- bpo-35445: Memory errors during creating posix.environ no longer ignored.
- bpo-35415: Validate fileno= argument to socket.socket().
- bpo-35424: `multiprocessing.Pool` destructor now emits`ResourceWarning` if the pool is still running.
- bpo-35330: When a `Mock` instance was used to wrap an object, if`side_effect` is used in one of the mocks of it methods, donât call the
original implementation and return the result of using the side effect the
same way that it is done with return_value.
- bpo-35346: Drop Mac OS 9 and Rhapsody support from the `platform` module. Rhapsody last release was in 2000. Mac OS 9 last release was in
2001.
- bpo-10496: `check_environ()` of`distutils.utils` now catches`KeyError` on calling`pwd.getpwuid()` : donât create
the`HOME` environment variable in this case.
- bpo-10496: `posixpath.expanduser()` now returns the input*path* unchanged if the`HOME` environment variable is not set and the current
user has no home directory (if the current user identifier doesnât exist
in the password database). This change fix the`site` module if the
current user doesnât exist in the password database (if the user has no
home directory).
- bpo-35389: `platform.libc_ver()` now uses`os.confstr('CS_GNU_LIBC_VERSION')` if available and the*executable* parameter is not set.
- bpo-35394: Add empty slots to asyncio abstract protocols.
- bpo-35310: Fix a bug in `select.select()` where, in some cases, the
file descriptor sequences were returned unmodified after a signal
interruption, even though the file descriptors might not be ready yet.`select.select()` will now always return empty lists if a timeout has
occurred.  Patch by Oran Avraham.
- bpo-35380: Enable TCP_NODELAY on Windows for proactor asyncio event loop.
- bpo-35341: Add generic version of `collections.OrderedDict` to the`typing` module. Patch by Ismo Toijala.
- bpo-35371: Fixed possible crash in `os.utime()` on Windows when pass
incorrect arguments.
- bpo-35346: `platform.uname()` now redirects`stderr` to`os.devnull` when running external programs like`cmd /c ver` .
- bpo-35066: Previously, calling the strftime() method on a datetime object with a trailing â%â in the format string would result in an exception. However, this only occurred when the datetime C module was being used; the python implementation did not match this behavior. Datetime is now PEP-399 compliant, and will not throw an exception on a trailing â%â.
- bpo-35345: The function `platform.popen` has been removed, it was
deprecated since Python 3.3: use`os.popen()` instead.
- bpo-35344: On macOS, `platform.platform()` now uses`platform.mac_ver()` , if it returns a non-empty release string, to get
the macOS version rather than the darwin version.
- bpo-35312: Make `lib2to3.pgen2.parse.ParseError` round-trip pickle-able.
Patch by Anthony Sottile.
- bpo-35308: Fix regression in `webbrowser` where default browsers may be
preferred over browsers in the`BROWSER` environment variable.
- bpo-24746: Avoid stripping trailing whitespace in doctest fancy diff. Original patch by R. David Murray & Jairo Trad. Enhanced by Sanyam Khurana.
- bpo-28604: `locale.localeconv()` now sets temporarily the`LC_CTYPE` locale to the`LC_MONETARY` locale if the two locales are different and
monetary strings are non-ASCII. This temporary change affects other
threads.
- bpo-35277: Update ensurepip to install pip 18.1 and setuptools 40.6.2.
- bpo-24209: Adds IPv6 support when invoking http.server directly.
- bpo-35226: Recursively check arguments when testing for equality of `unittest.mock.call` objects and add note that tracking of
parameters used to create ancestors of mocks in`mock_calls` is not
possible.
- bpo-29564: The warnings module now suggests to enable tracemalloc if the source is specified, the tracemalloc module is available, but tracemalloc is not tracing memory allocations.
- bpo-35189: Modify the following fnctl function to retry if interrupted by a signal (EINTR): flock, lockf, fnctl
- bpo-30064: Use add_done_callback() in sock_* asyncio API to unsubscribe reader/writer early on calcellation.
- bpo-35186: Removed the âbuilt withâ comment added when `setup.py upload` is used with either`bdist_rpm` or`bdist_dumb` .
- bpo-35152: Allow sending more than 2 GB at once on a multiprocessing connection on non-Windows systems.
- bpo-35062: Fix incorrect parsing of `io.IncrementalNewlineDecoder` âs*translate* argument.
- bpo-35065: Remove `StreamReaderProtocol._untrack_reader` . The call to`_untrack_reader` is currently performed too soon, causing the protocol
to forget about the reader before`connection_lost` can run and feed the
EOF to the reader.
- bpo-34160: ElementTree and minidom now preserve the attribute order specified by the user.
- bpo-35079: Improve difflib.SequenceManager.get_matching_blocks doc by adding ânon-overlappingâ and changing â!=â to â<â.
- bpo-33710: Deprecated `l*gettext()` functions and methods in the`gettext` module. They return encoded bytes instead of Unicode
strings and are artifacts from Python 2 times. Also deprecated functions
and methods related to setting the charset for`l*gettext()` functions
and methods.
- bpo-35017: `socketserver.BaseServer.serve_forever()` now exits
immediately if itâs`shutdown()` method is
called while it is polling for new events.
- bpo-35024: `importlib` no longer logs`wrote <bytecode path>` redundantly after`(created|could not create) <bytecode path>` is
already logged. Patch by Quentin Agren.
- bpo-35047: `unittest.mock` now includes mock calls in exception messages
if`assert_not_called` ,`assert_called_once` , or`assert_called_once_with` fails. Patch by Petter Strandmark.
- bpo-31047: Fix `ntpath.abspath` regression where it didnât remove a
trailing separator on Windows. Patch by Tim Graham.
- bpo-35053: tracemalloc now tries to update the traceback when an object is reused from a âfree listâ (optimization for faster object creation, used by the builtin list type for example).
- bpo-31553: Add the âjson-lines option to json.tool. Patch by hongweipeng.
- bpo-34794: Fixed a leak in Tkinter when pass the Python wrapper around Tcl_Obj back to Tcl/Tk.
- bpo-34909: Enum: fix grandchildren subclassing when parent mixed with concrete data types.
- bpo-35022: `unittest.mock.MagicMock` now supports the`__fspath__` method (from`os.PathLike` ).
- bpo-35008: Fixed references leaks when call the `__setstate__()` method
of`xml.etree.ElementTree.Element` in the C implementation for
already initialized element.
- bpo-23420: Verify the value for the parameter â-sâ of the cProfile CLI. Patch by Robert Kuska
- bpo-33947: dataclasses now handle recursive reprs without raising RecursionError.
- bpo-34890: Make `inspect.iscoroutinefunction()` ,`inspect.isgeneratorfunction()` and`inspect.isasyncgenfunction()` work with`functools.partial()` . Patch by Pablo Galindo.
- bpo-34521: Use `socket.CMSG_SPACE()` to calculate ancillary data size
instead of`socket.CMSG_LEN()` in`multiprocessing.reduction.recvfds()` as**RFC 3542** requires the use
of the former for portable applications.
- bpo-31522: The `mailbox.mbox.get_string` function*from_* parameter can
now successfully be set to a non-default value.
- bpo-34970: Protect tasks weak set manipulation in `asyncio.all_tasks()`
- bpo-34969: gzip: Add âfast, âbest on the gzip CLI, these parameters will be used for the fast compression method (quick) or the best method compress (slower, but smaller file). Also, change the default compression level to 6 (tradeoff).
- bpo-16965: The 2to3 `execfile` fixer now opens the file with mode`'rb'` .  Patch by Zackery Spytz.
- bpo-34966: `pydoc` now supports aliases not only to methods defined
in the end class, but also to inherited methods.  The docstring is not
duplicated for aliases.
- bpo-34926: `mimetypes.MimeTypes.guess_type()` now accepts
path-like object in addition to url strings. Patch by Mayank
Asthana.
- bpo-23831: Add `moveto()` method to the`tkinter.Canvas` widget. Patch
by Juliette Monsel.
- bpo-34941: Methods `find()` ,`findtext()` and`findall()` of the`Element` class in the`xml.etree.ElementTree` module are now able
to find children which are instances of`Element` subclasses.
- bpo-32680: `smtplib.SMTP` objects now always have a`sock` attribute present
- bpo-34769: Fix for async generators not finalizing when event loop is in debug mode and garbage collector runs in another thread.
- bpo-34936: Fix `TclError` in`tkinter.Spinbox.selection_element()` .
Patch by Juliette Monsel.
- bpo-34829: Add methods `selection_from` ,`selection_range` ,`selection_present` and`selection_to` to the`tkinter.Spinbox` for
consistency with the`tkinter.Entry` widget. Patch by Juliette Monsel.
- bpo-34911: Added *secure_protocols* argument to*http.cookiejar.DefaultCookiePolicy* to allow for tweaking of protocols
and also to add support by default for*wss* , the secure websocket
protocol.
- bpo-34922: Fixed integer overflow in the `digest()` and`hexdigest()` methods for the SHAKE algorithm in the`hashlib` module.
- bpo-34925: 25% speedup in argument parsing for the functions in the bisect module.
- bpo-34900: Fixed `unittest.TestCase.debug()` when used to call test
methods with subtests.  Patch by Bruno Oliveira.
- bpo-34844: logging.Formatter enhancement - Ensure styles and fmt matches in logging.Formatter - Added validate method in each format style class: StrFormatStyle, PercentStyle, StringTemplateStyle. - This method is called in the constructor of logging.Formatter class - Also re-raise the KeyError in the format method of each style class, so it would a bit clear that itâs an error with the invalid format fields.
- bpo-34897: Adjust test.support.missing_compiler_executable check so that a nominal command name of ââ is ignored. Patch by Michael Felt.
- bpo-34871: Fix inspect module polluted `sys.modules` when parsing`__text_signature__` of callable.
- bpo-34898: Add `mtime` argument to`gzip.compress` for reproducible
output. Patch by Guo Ci Teo.
- bpo-28441: On Cygwin and MinGW, ensure that `sys.executable` always
includes the full filename in the path, including the`.exe` suffix
(unless it is a symbolic link).
- bpo-34866: Adding `max_num_fields` to`cgi.FieldStorage` to make DOS
attacks harder by limiting the number of`MiniFieldStorage` objects
created by`FieldStorage` .
- bpo-34711: http.server ensures it reports HTTPStatus.NOT_FOUND when the local path ends with â/â and is not a directory, even if the underlying OS (e.g. AIX) accepts such paths as a valid file reference. Patch by Michael Felt.
- bpo-34872: Fix self-cancellation in C implementation of asyncio.Task
- bpo-34849: Donât log waiting for `selector.select` in asyncio loop
iteration. The waiting is pretty normal for any asyncio program, logging
its time just adds a noise to logs without any useful information
provided.
- bpo-34022: The `SOURCE_DATE_EPOCH` environment variable no longer
overrides the value of the*invalidation_mode* argument to`py_compile.compile()` , and determines its default value instead.
- bpo-34819: Use a monotonic clock to compute timeouts in `Executor.map()` and`as_completed()` , in order to prevent
timeouts from deviating when the system clock is adjusted.
- bpo-34758: Add .wasm -> application/wasm to list of recognized file types and content type headers
- bpo-34789: `xml.sax.make_parser()` now accepts any iterable as its*parser_list* argument.  Patch by AndrÃ©s Delfino.
- bpo-34334: In `QueueHandler` , clear`exc_text` from`LogRecord` to prevent traceback from being written twice.
- bpo-34687: On Windows, asyncio now uses ProactorEventLoop, instead of SelectorEventLoop, by default.
- bpo-5950: Support reading zip files with archive comments in `zipimport` .
- bpo-32892: The parser now represents all constants as `ast.Constant` instead of using specific constant AST types
(`Num` ,`Str` ,`Bytes` ,`NameConstant` and`Ellipsis` ). These
classes are considered deprecated and will be removed in future Python
versions.
- bpo-34728: Add deprecation warning when `loop` is used in methods:`asyncio.sleep` ,`asyncio.wait` and`asyncio.wait_for` .
- bpo-34738: ZIP files created by `distutils` will now include entries for
directories.
- bpo-34659: Add an optional *initial* argument to itertools.accumulate().
- bpo-29577: Support multiple mixin classes when creating Enums.
- bpo-34670: Add SSLContext.post_handshake_auth and SSLSocket.verify_client_post_handshake for TLS 1.3âs post handshake authentication feature.
- bpo-32718: The Activate.ps1 script from venv works with PowerShell Core 6.1 and is now available under all operating systems.
- bpo-31177: Fix bug that prevented using `reset_mock` on mock instances with deleted attributes
- bpo-34672: Add a workaround, so the `'Z'``time.strftime()` specifier on the musl C library can work in some cases.
- bpo-34666: Implement `asyncio.StreamWriter.awrite` and`asyncio.StreamWriter.aclose()` coroutines.  Methods are needed for
providing a consistent stream API with control flow switched on by
default.
- bpo-6721: Acquire the logging moduleâs commonly used internal locks while fork()ing to avoid deadlocks in the child process.
- bpo-34658: Fix a rare interpreter unhandled exception state SystemError only seen when using subprocess with a preexec_fn while an after_parent handler has been registered with os.register_at_fork and the fork system call fails.
- bpo-34652: Ensure `os.lchmod()` is never defined on Linux.
- bpo-34638: Store a weak reference to stream reader to break strong references loop between reader and protocol. It allows to detect and close the socket if the stream is deleted (garbage collected) without `close()` call.
- bpo-34536: `Enum._missing_` :  raise`ValueError` if None returned and`TypeError` if non-member is returned.
- bpo-34636: Speed up re scanning of many non-matching characters for s w and d within bytes objects. (microoptimization)
- bpo-24412: Add `addModuleCleanup()` and`addClassCleanup()` to unittest to support cleanups
for`setUpModule()` and`setUpClass()` . Patch by Lisa Roach.
- bpo-34630: Donât log SSL certificate errors in asyncio code (connection error logging is skipped already).
- bpo-32490: Prevent filename duplication in `subprocess` exception
messages.  Patch by Zackery Spytz.
- bpo-34363: dataclasses.asdict() and .astuple() now handle namedtuples correctly.
- bpo-34625: Update vendorized expat library version to 2.2.6.
- bpo-32270: The subprocess module no longer mistakenly closes redirected fds even when they were in pass_fds when outside of the default {0, 1, 2} set.
- bpo-34622: Create a dedicated `asyncio.CancelledError` ,`asyncio.InvalidStateError` and`asyncio.TimeoutError` exception
classes.  Inherit them from corresponding exceptions from`concurrent.futures` package. Extract`asyncio` exceptions into a
separate file.
- bpo-34610: Fixed iterator of `multiprocessing.managers.DictProxy` .
- bpo-34421: Fix distutils logging for non-ASCII strings. This caused installation issues on Windows.
- bpo-34604: Fix possible mojibake in the error message of `pwd.getpwnam` and`grp.getgrnam` using string representation because of invisible
characters or trailing whitespaces. Patch by William Grzybowski.
- bpo-30977: Make uuid.UUID use `__slots__` to reduce its memory
footprint. Based on original patch by Wouter Bolsterlee.
- bpo-34574: OrderedDict iterators are not exhausted during pickling anymore. Patch by Sergey Fedoseev.
- bpo-8110: Refactored `subprocess` to check for Windows-specific
modules rather than`sys.platform == 'win32'` .
- bpo-34530: `distutils.spawn.find_executable()` now falls back on`os.defpath` if the`PATH` environment variable is not set.
- bpo-34563: On Windows, fix multiprocessing.Connection for very large read: fix _winapi.PeekNamedPipe() and _winapi.ReadFile() for read larger than INT_MAX (usually `2**31-1` ).
- bpo-34558: Correct typo in Lib/ctypes/_aix.py
- bpo-34282: Move `Enum._convert` to`EnumMeta._convert_` and fix enum
members getting shadowed by parent attributes.
- bpo-22872: When the queue is closed, `ValueError` is now raised by`multiprocessing.Queue.put()` and`multiprocessing.Queue.get()` instead of`AssertionError` and`OSError` , respectively. Patch
by Zackery Spytz.
- bpo-34515: Fix parsing non-ASCII identifiers in `lib2to3.pgen2.tokenize` (**PEP 3131** ).
- bpo-13312: Avoids a possible integer underflow (undefined behavior) in the time moduleâs year handling code when passed a very low negative year value.
- bpo-34472: Improved compatibility for streamed files in `zipfile` .
Previously an optional signature was not being written and certain ZIP
applications were not supported. Patch by Silas Sewell.
- bpo-34454: Fix the .fromisoformat() methods of datetime types crashing when given unicode with non-UTF-8-encodable code points. Specifically, datetime.fromisoformat() now accepts surrogate unicode code points used as the separator. Report and tests by Alexey Izbyshev, patch by Paul Ganssle.
- bpo-6700: Fix inspect.getsourcelines for module level frames/tracebacks. Patch by Vladimir Matveev.
- bpo-34171: Running the `trace` module no longer creates the`trace.cover` file.
- bpo-34441: Fix crash when an `ABC` -derived class with invalid`__subclasses__` is passed as the second argument to`issubclass()` .
Patch by Alexey Izbyshev.
- bpo-34427: Fix infinite loop in `a.extend(a)` for`MutableSequence` subclasses.
- bpo-34412: Make `signal.strsignal()` work on HP-UX. Patch by Michael
Osipov.
- bpo-20849: shutil.copytree now accepts a new `dirs_exist_ok` keyword
argument. Patch by Josh Bronson.
- bpo-31715: Associate `.mjs` file extension with`application/javascript` MIME Type.
- bpo-34384: `os.readlink()` now accepts path-like and`bytes` objects on Windows.
- bpo-22602: The UTF-7 decoder now raises `UnicodeDecodeError` for
ill-formed sequences starting with â+â (as specified in RFC 2152).  Patch
by Zackery Spytz.
- bpo-2122: The `mmap.flush()` method now returns`None` on success, raises an exception on error under all platforms.
- bpo-34341: Appending to the ZIP archive with the ZIP64 extension no longer grows the size of extra fields of existing entries.
- bpo-34333: Fix %-formatting in `pathlib.PurePath.with_suffix()` when
formatting an error message.
- bpo-18540: The `imaplib.IMAP4` and`imaplib.IMAP4_SSL` classes now resolve to the local host IP correctly when the default value
of*host* parameter (`''` ) is used.
- bpo-26502: Implement `traceback.FrameSummary.__len__()` method to
preserve compatibility with the old tuple API.
- bpo-34318: `assertRaises()` ,`assertRaisesRegex()` ,`assertWarns()` and`assertWarnsRegex()` no longer success if the
passed callable is None. They no longer ignore unknown keyword arguments
in the context manager mode. A DeprecationWarning was raised in these
cases since Python 3.5.
- bpo-9372: Deprecate `__getitem__()` methods of`xml.dom.pulldom.DOMEventStream` ,`wsgiref.util.FileWrapper` and`fileinput.FileInput` .
- bpo-33613: Fix a race condition in `multiprocessing.semaphore_tracker` when the tracker receives SIGINT before it can register signal handlers
for ignoring it.
- bpo-34248: Report filename in the exception raised when the database file cannot be opened by `dbm.gnu.open()` and`dbm.ndbm.open()` due to
OS-related error. Patch by Zsolt Cserna.
- bpo-33089: Add math.dist() to compute the Euclidean distance between two points.
- bpo-34246: `smtplib.SMTP.send_message()` no longer modifies the
content of the*mail_options* argument. Patch by Pablo S. Blum de Aguiar.
- bpo-31047: Fix `ntpath.abspath` for invalid paths on windows. Patch by
Franz Woellert.
- bpo-32321: Add pure Python fallback for functools.reduce. Patch by Robert Wright.
- bpo-34270: The default asyncio task class now always has a name which can be get or set using two new methods ( `get_name()` and`set_name()` ) and is visible in the`repr()` output.
An initial name can also be set using the new`name` keyword argument to`asyncio.create_task()` or the`create_task()` method of the event loop.
If no initial name is set, the default Task implementation generates a
name like`Task-1` using a monotonic counter.
- bpo-34263: asyncioâs event loop will not pass timeouts longer than one day to epoll/select etc.
- bpo-34035: Fix several AttributeError in zipfile seek() methods. Patch by MickaÃ«l Schoentgen.
- bpo-32215: Fix performance regression in `sqlite3` when a DML
statement appeared in a different line than the rest of the SQL query.
- bpo-34075: Deprecate passing non-ThreadPoolExecutor instances to `AbstractEventLoop.set_default_executor()` .
- bpo-34251: Restore `msilib.Win64` to preserve backwards compatibility
since itâs already used by`distutils` â`bdist_msi` command.
- bpo-19891: Ignore errors caused by missing / non-writable homedir while writing history during exit of an interactive session. Patch by Anthony Sottile.
- bpo-33089: Enhanced math.hypot() to support more than two dimensions.
- bpo-34228: tracemalloc: PYTHONTRACEMALLOC=0 environment variable and -X tracemalloc=0 command line option are now allowed to disable explicitly tracemalloc at startup.
- bpo-13041: Use `shutil.get_terminal_size()` to calculate the terminal
width correctly in the`argparse.HelpFormatter` class.  Initial patch by
Zbyszek JÄdrzejewski-Szmek.
- bpo-34213: Allow frozen dataclasses to have a field named âobjectâ. Previously this conflicted with an internal use of âobjectâ.
- bpo-34052: `sqlite3.Connection.create_aggregate()` ,`sqlite3.Connection.create_function()` ,`sqlite3.Connection.set_authorizer()` ,`sqlite3.Connection.set_progress_handler()` methods raises TypeError
when unhashable objects are passed as callable. These methods now donât
pass such objects to SQLite API. Previous behavior could lead to
segfaults. Patch by Sergey Fedoseev.
- bpo-34197: Attributes *skipinitialspace* ,*doublequote* and*strict* of
the*dialect* attribute of the`csv` reader are now`bool` instances instead of integers 0 or 1.
- bpo-32788: Errors other than `TypeError` raised in methods`__adapt__()` and`__conform__()` in the`sqlite3` module are now
propagated to the user.
- bpo-21446: The `reload` fixer now uses`importlib.reload()` instead
of deprecated`imp.reload()` .
- bpo-940286: pydocâs `Helper.showtopic()` method now prints the cross
references of a topic correctly.
- bpo-34164: `base64.b32decode()` could raise UnboundLocalError or
OverflowError for incorrect padding.  Now it always raises`base64.Error` in these cases.
- bpo-33729: Fixed issues with arguments parsing in `hashlib` .
- bpo-34097: ZipFile can zip files older than 1980-01-01 and newer than 2107-12-31 using a new `strict_timestamps` parameter at the cost of
setting the timestamp to the limit.
- bpo-34108: Remove extraneous CR in 2to3 refactor.
- bpo-34070: Make sure to only check if the handle is a tty, when opening a file with `buffering=-1` .
- bpo-27494: Reverted bpo-27494. 2to3 rejects now a trailing comma in generator expressions.
- bpo-33967: functools.singledispatch now raises TypeError instead of IndexError when no positional arguments are passed.
- bpo-34041: Add the parameter *deterministic* to the`sqlite3.Connection.create_function()` method. Patch by Sergey
Fedoseev.
- bpo-34056: Ensure the loader shim created by `imp.load_module` always
returns bytes from its`get_data()` function. This fixes using`imp.load_module` with**PEP 552** hash-based pycs.
- bpo-34054: The multiprocessing module now uses the monotonic clock `time.monotonic()` instead of the system clock`time.time()` to
implement timeout.
- bpo-34043: Optimize tarfile uncompress performance about 15% when gzip is used.
- bpo-34044: `subprocess.Popen` now copies the*startupinfo* argument to
leave it unchanged: it will modify the copy, so that the same`STARTUPINFO` object can be used multiple times.
- bpo-34010: Fixed a performance regression for reading streams with tarfile. The buffered read should use a list, instead of appending to a bytes object.
- bpo-34019: webbrowser: Correct the arguments passed to Opera Browser when opening a new URL using the `webbrowser` module. Patch by Bumsik Kim.
- bpo-34003: csv.DictReader now creates dicts instead of OrderedDicts. Patch by Michael Selik.
- bpo-33978: Closed existing logging handlers before reconfiguration via fileConfig and dictConfig. Patch by Karthikeyan Singaravelan.
- bpo-14117: Make minor tweaks to turtledemo. The âwikipediaâ example is now ârosetteâ, describing what it draws. The âpenroseâ print output is reduced. Theâ1024â output of âtreeâ is eliminated.
- bpo-33974: Fixed passing lists and tuples of strings containing special characters `"` ,`\` ,`{` ,`}` and`\n` as options to`ttk` widgets.
- bpo-27500: Fix getaddrinfo to resolve IPv6 addresses correctly.
- bpo-24567: Improve random.choices() to handle subnormal input weights that could occasionally trigger an IndexError.
- bpo-33871: Fixed integer overflow in `os.readv()` ,`os.writev()` ,`os.preadv()` and`os.pwritev()` and in`os.sendfile()` with*headers* or*trailers* arguments (on BSD-based OSes and macOS).
- bpo-25007: Add `copy.copy()` and`copy.deepcopy()` support to zlib
compressors and decompressors.  Patch by Zackery Spytz.
- bpo-33929: multiprocessing: Fix a race condition in Popen of multiprocessing.popen_spawn_win32. The child process now duplicates the read end of pipe instead of âstealingâ it. Previously, the read end of pipe was âstolenâ by the child process, but it leaked a handle if the child process had been terminated before it could steal the handle from the parent process.
- bpo-33899: Tokenize module now implicitly emits a NEWLINE when provided with input that does not have a trailing new line. This behavior now matches what the C tokenizer does internally. Contributed by Ammar Askar.
- bpo-33897: Added a âforceâ keyword argument to logging.basicConfig().
- bpo-33695: `shutil.copytree()` uses`os.scandir()` function and
all copy functions depending from it use cached`os.stat()` values.
The speedup for copying a directory with 8000 files is around +9% on
Linux, +20% on Windows and + 30% on a Windows SMB share. Also the number
of`os.stat()` syscalls is reduced by 38% making`shutil.copytree()` especially faster on network filesystems.
(Contributed by Giampaolo Rodolaâ in bpo-33695.)
- bpo-33916: bz2 and lzma: When Decompressor.__init__() is called twice, free the old lock to not leak memory.
- bpo-32568: Make select.epoll() and its documentation consistent regarding *sizehint* and*flags* .
- bpo-33833: Fixed bug in asyncio where ProactorSocketTransport logs AssertionError if force closed during write.
- bpo-33663: Convert content length to string before putting to header.
- bpo-33721: `os.path` functions that return a boolean result like`exists()` ,`lexists()` ,`isdir()` ,`isfile()` ,`islink()` , and`ismount()` , and`pathlib.Path` methods that return a
boolean result like`exists()` ,`is_dir()` ,`is_file()` ,`is_mount()` ,`is_symlink()` ,`is_block_device()` ,`is_char_device()` ,`is_fifo()` ,`is_socket()` now return`False` instead of raising`ValueError` or its subclasses`UnicodeEncodeError` and`UnicodeDecodeError` for paths that contain characters or bytes
unrepresentable at the OS level.
- bpo-26544: Fixed implementation of `platform.libc_ver()` . It almost
always returned version â2.9â for glibc.
- bpo-33843: Remove deprecated `cgi.escape` ,`cgi.parse_qs` and`cgi.parse_qsl` .
- bpo-33842: Remove `tarfile.filemode` which is deprecated since Python
3.3.
- bpo-30167: Prevent site.main() exception if PYTHONSTARTUP is set. Patch by Steve Weber.
- bpo-33805: Improve error message of dataclasses.replace() when an InitVar is not specified
- bpo-33687: Fix the call to `os.chmod()` for`uu.decode()` if a mode is
given or decoded. Patch by Timo Furrer.
- bpo-33812: Datetime instance d with non-None tzinfo, but with d.tzinfo.utcoffset(d) returning None is now treated as naive by the astimezone() method.
- bpo-32108: In configparser, donât clear section when it is assigned to itself.
- bpo-27397: Make email module properly handle invalid-length base64 strings.
- bpo-33578: Implement multibyte encoder/decoder state methods
- bpo-30805: Avoid race condition with debug logging
- bpo-33476: Fix _header_value_parser.py when address group is missing final â;â. Contributed by Enrique Perez-Terron
- bpo-33694: asyncio: Fix a race condition causing data loss on pause_reading()/resume_reading() when using the ProactorEventLoop.
- bpo-32493: Correct test for `uuid_enc_be` availability in`configure.ac` . Patch by Michael Felt.
- bpo-33792: Add asyncio.WindowsSelectorEventLoopPolicy and asyncio.WindowsProactorEventLoopPolicy.
- bpo-33274: W3C DOM Level 1 specifies return value of Element.removeAttributeNode() as âThe Attr node that was removed.â xml.dom.minidom now complies with this requirement.
- bpo-33778: Update `unicodedata` âs database to Unicode version 11.0.0.
- bpo-33165: Added a stacklevel parameter to logging calls to allow use of wrapper/helper functions for logging APIs.
- bpo-33770: improve base64 exception message for encoded inputs of invalid length
- bpo-33769: asyncio/start_tls: Fix error message; cancel callbacks in case of an unhandled error; mark SSLTransport as closed if it is aborted.
- bpo-33767: The concatenation ( `+` ) and repetition (`*` ) sequence
operations now raise`TypeError` instead of`SystemError` when
performed on`mmap.mmap` objects.  Patch by Zackery Spytz.
- bpo-33734: asyncio/ssl: Fix AttributeError, increase default handshake timeout
- bpo-31014: Fixed creating a controller for `webbrowser` when a user
specifies a path to an entry in the BROWSER environment variable.  Based
on patch by John Still.
- bpo-2504: Add gettext.pgettext() and variants.
- bpo-33197: Add description property for _ParameterKind
- bpo-32751: When cancelling the task due to a timeout, `asyncio.wait_for()` will now wait until the cancellation is complete.
- bpo-32684: Fix gather to propagate cancellation of itself even with return_exceptions.
- bpo-33654: Support protocol type switching in SSLTransport.set_protocol().
- bpo-33674: Pause the transport as early as possible to further reduce the risk of data_received() being called before connection_made().
- bpo-33671: `shutil.copyfile()` ,`shutil.copy()` ,`shutil.copy2()` ,`shutil.copytree()` and`shutil.move()` use
platform-specific fast-copy syscalls on Linux and macOS in order to copy
the file more efficiently. On Windows`shutil.copyfile()` uses a
bigger default buffer size (1 MiB instead of 16 KiB) and a`memoryview()` -based variant of`shutil.copyfileobj()` is used.
The speedup for copying a 512MiB file is about +26% on Linux, +50% on
macOS and +40% on Windows. Also, much less CPU cycles are consumed.
(Contributed by Giampaolo Rodolaâ in bpo-25427.)
- bpo-33674: Fix a race condition in SSLProtocol.connection_made() of asyncio.sslproto: start immediately the handshake instead of using call_soon(). Previously, data_received() could be called before the handshake started, causing the handshake to hang or fail.
- bpo-31647: Fixed bug where calling write_eof() on a _SelectorSocketTransport after itâs already closed raises AttributeError.
- bpo-32610: Make asyncio.all_tasks() return only pending tasks.
- bpo-32410: Avoid blocking on file IO in sendfile fallback code
- bpo-33469: Fix RuntimeError after closing loop that used run_in_executor
- bpo-33672: Fix Task.__repr__ crash with Cythonâs bogus coroutines
- bpo-33654: Fix transport.set_protocol() to support switching between asyncio.Protocol and asyncio.BufferedProtocol. Fix loop.start_tls() to work with asyncio.BufferedProtocols.
- bpo-33652: Pickles of type variables and subscripted generics are now future-proof and compatible with older Python versions.
- bpo-32493: Fixed `uuid.uuid1()` on FreeBSD.
- bpo-33238: Add `InvalidStateError` to`concurrent.futures` .`Future.set_result` and`Future.set_exception` now raise`InvalidStateError` if the futures are not pending or running. Patch by
Jason Haydaman.
- bpo-33618: Finalize and document preliminary and experimental TLS 1.3 support with OpenSSL 1.1.1
- bpo-33625: Release GIL on `grp.getgrnam` ,`grp.getgrgid` ,`pwd.getpwnam` and`pwd.getpwuid` if reentrant variants of these
functions are available. Patch by William Grzybowski.
- bpo-33623: Fix possible SIGSGV when asyncio.Future is created in __del__
- bpo-11874: Use a better regex when breaking usage into wrappable parts. Avoids bogus assertion errors from custom metavar strings.
- bpo-30877: Fixed a bug in the Python implementation of the JSON decoder that prevented the cache of parsed strings from clearing after finishing the decoding. Based on patch by c-fos.
- bpo-33604: Remove HMAC default to md5 marked for removal in 3.8 (removal originally planned in 3.6, bump to 3.8 in PR 7062).
- bpo-33582: Emit a deprecation warning for inspect.formatargspec
- bpo-21145: Add `functools.cached_property` decorator, for computed
properties cached for the life of the instance.
- bpo-33570: Change TLS 1.3 cipher suite settings for compatibility with OpenSSL 1.1.1-pre6 and newer. OpenSSL 1.1.1 will have TLS 1.3 ciphers enabled by default.
- bpo-28556: Do not simplify arguments to `typing.Union` . Now`Union[Manager, Employee]` is not simplified to`Employee` at runtime.
Such simplification previously caused several bugs and limited
possibilities for introspection.
- bpo-12486: `tokenize.generate_tokens()` is now documented as a public
API to tokenize unicode strings. It was previously present but
undocumented.
- bpo-33540: Add a new `block_on_close` class attribute to`ForkingMixIn` and`ThreadingMixIn` classes of`socketserver` .
- bpo-33548: tempfile._candidate_tempdir_list should consider common TEMP locations
- bpo-33109: argparse subparsers are once again not required by default, reverting the change in behavior introduced by bpo-26510 in 3.7.0a2.
- bpo-33541: Remove unused private method `_strptime.LocaleTime.__pad` (a.k.a.`_LocaleTime__pad` ).
- bpo-33536: dataclasses.make_dataclass now checks for invalid field names and duplicate fields. Also, added a check for invalid field specifications.
- bpo-33542: Prevent `uuid.get_node` from using a DUID instead of a MAC on
Windows. Patch by Zvi Effron
- bpo-26819: Fix race condition with `ReadTransport.resume_reading` in
Windows proactor event loop.
- Fix failure in `typing.get_type_hints()` when ClassVar was provided as a
string forward reference.
- bpo-33516: `unittest.mock.MagicMock` now supports the`__round__` magic method.
- bpo-28612: Added support for Site Maps to urllibâs `RobotFileParser` as`RobotFileParser.site_maps()` . Patch by Lady Red, based
on patch by Peter Wirtz.
- bpo-28167: Remove platform.linux_distribution, which was deprecated since 3.5.
- bpo-33504: Switch the default dictionary implementation for `configparser` from`collections.OrderedDict` to the standard`dict` type.
- bpo-33505: Optimize asyncio.ensure_future() by reordering if checks: 1.17x faster.
- bpo-33497: Add errors param to cgi.parse_multipart and make an encoding in FieldStorage use the given errors (needed for Twisted). Patch by Amber Brown.
- bpo-29235: The `cProfile.Profile` class can now be used as a
context manager. Patch by Scott Sanderson.
- bpo-33495: Change dataclasses.Fields repr to use the repr of each of its members, instead of str. This makes it more clear what each field actually represents. This is especially true for the âtypeâ member.
- bpo-26103: Correct `inspect.isdatadescriptor` to look for`__set__` or`__delete__` .  Patch by Aaron Hall.
- bpo-29209: Removed the `doctype()` method and the*html* parameter of
the constructor of`XMLParser` .  The`doctype()` method defined in a subclass will no longer be called.
Deprecated methods`getchildren()` and`getiterator()` in the`ElementTree` module emit now a`DeprecationWarning` instead of`PendingDeprecationWarning` .
- bpo-33453: Fix dataclasses to work if using literal string type annotations or if using PEP 563 âPostponed Evaluation of Annotationsâ. Only specific string prefixes are detected for both ClassVar (âClassVarâ and âtyping.ClassVarâ) and InitVar (âInitVarâ and âdataclasses.InitVarâ).
- bpo-28556: Minor fixes in typing module: add annotations to `NamedTuple.__new__` , pass`*args` and`**kwds` in`Generic.__new__` .  Original PRs by Paulius Å arka and Chad Dombrova.
- bpo-33365: Print the header values besides the header keys instead just the header keys if *debuglevel* is set to >0 in`http.client` . Patch
by Marco Strigl.
- bpo-20087: Updated alias mapping with glibc 2.27 supported locales.
- bpo-33422: Fix trailing quotation marks getting deleted when looking up byte/string literals on pydoc. Patch by AndrÃ©s Delfino.
- bpo-28167: The function `platform.linux_distribution` and`platform.dist` now trigger a`DeprecationWarning` and have been
marked for removal in Python 3.8
- bpo-33281: Fix ctypes.util.find_library regression on macOS.
- bpo-33311: Text and html output generated by cgitb does not display parentheses if the current call is done directly in the module. Patch by StÃ©phane Blondon.
- bpo-27300: The file classes in *tempfile* now accept an*errors* parameter
that complements the already existing*encoding* .  Patch by Stephan Hohe.
- bpo-32933: `unittest.mock.mock_open()` now supports iteration over the
file contents. Patch by Tony Flury.
- bpo-33217: Raise `TypeError` when looking up non-Enum objects in Enum
classes and Enum members.
- bpo-33197: Update error message when constructing invalid inspect.Parameters Patch by Donghee Na.
- bpo-33383: Fixed crash in the get() method of the `dbm.ndbm` database
object when it is called with a single argument.
- bpo-33375: The warnings module now finds the Python file associated with a warning from the code object, rather than the frameâs global namespace. This is consistent with how tracebacks and pdb find filenames, and should work better for dynamically executed code.
- bpo-33336: `imaplib` now allows`MOVE` command in`IMAP4.uid()` (RFC
6851: IMAP MOVE Extension) and potentially as a name of supported method
of`IMAP4` object.
- bpo-32455: Added *jump* parameter to`dis.stack_effect()` .
- bpo-27485: Rename and deprecate undocumented functions in `urllib.parse()` .
- bpo-33332: Add `signal.valid_signals()` to expose the POSIX sigfillset()
functionality.
- bpo-33251: `ConfigParser.items()` was fixed so that key-value pairs
passed in via`vars()` are not included in the resulting output.
- bpo-33329: Fix multiprocessing regression on newer glibcs
- bpo-33334: `dis.stack_effect()` now supports all defined opcodes
including NOP and EXTENDED_ARG.
- bpo-991266: Fix quoting of the `Comment` attribute of`http.cookies.SimpleCookie` .
- bpo-33131: Upgrade bundled version of pip to 10.0.1.
- bpo-33308: Fixed a crash in the `parser` module when converting an ST
object to a tree of tuples or lists with`line_info=False` and`col_info=True` .
- bpo-23403: lib2to3 now uses pickle protocol 4 for pre-computed grammars.
- bpo-33266: lib2to3 now recognizes `rf'...'` strings.
- bpo-11594: Ensure line-endings are respected when using lib2to3.
- bpo-33254: Have `importlib.resources.contents()` and`importlib.abc.ResourceReader.contents()` return an iterable
instead of an iterator.
- bpo-33265: `contextlib.ExitStack` and`contextlib.AsyncExitStack` now
use a method instead of a wrapper function for exit callbacks.
- bpo-33263: Fix FD leak in `_SelectorSocketTransport` Patch by Vlad
Starostin.
- bpo-33256: Fix display of `<module>` call in the html produced by`cgitb.html()` . Patch by StÃ©phane Blondon.
- bpo-33144: `random.Random()` and its subclassing mechanism got optimized
to check only once at class/subclass instantiation time whether its`getrandbits()` method can be relied on by other methods, including`randrange()` , for the generation of arbitrarily large random integers.
Patch by Wolfgang Maier.
- bpo-33185: Fixed regression when running pydoc with the `-m` switch. (The regression was introduced in 3.7.0b3 by the resolution of
bpo-33053)This fix also changed pydoc to add `os.getcwd()` to`sys.path` when necessary, rather than adding`"."` .
- bpo-29613: Added support for the `SameSite` cookie flag to the`http.cookies` module.
- bpo-33169: Delete entries of `None` in`sys.path_importer_cache` when`importlib.machinery.invalidate_caches()` is called.
- bpo-33203: `random.Random.choice()` now raises`IndexError` for empty
sequences consistently even when called from subclasses without a`getrandbits()` implementation.
- bpo-33224: Update difflib.mdiff() for **PEP 479** .  Convert an uncaught
StopIteration in a generator into a return-statement.
- bpo-33209: End framing at the end of C implementation of `pickle.Pickler.dump()` .
- bpo-32861: The urllib.robotparserâs `__str__` representation now
includes wildcard entries and the âCrawl-delayâ and âRequest-rateâ fields.
Also removes extra newlines that were being appended to the end of the
string. Patch by Michael Lazar.
- bpo-23403: `DEFAULT_PROTOCOL` in`pickle` was bumped to 4. Protocol
4 is described in**PEP 3154** and available since Python 3.4. It offers
better performance and smaller size compared to protocol 3 introduced in
Python 3.0.
- bpo-20104: Improved error handling and fixed a reference leak in `os.posix_spawn()` .
- bpo-33106: Deleting a key from a read-only dbm database raises module specific error instead of KeyError.
- bpo-33175: In dataclasses, Field.__set_name__ now looks up the __set_name__ special method on the class, not the instance, of the default value.
- bpo-32380: Create functools.singledispatchmethod to support generic single dispatch on descriptors and methods.
- bpo-33141: Have Field objects pass through __set_name__ to their default values, if they have their own __set_name__.
- bpo-33096: Allow ttk.Treeview.insert to insert iid that has a false boolean value. Note iid=0 and iid=False would be same. Patch by Garvit Khatri.
- bpo-32873: Treat type variables and special typing forms as immutable by copy and pickle. This fixes several minor issues and inconsistencies, and improves backwards compatibility with Python 3.6.
- bpo-33134: When computing dataclassâs __hash__, use the lookup table to contain the function which returns the __hash__ value. This is an improvement over looking up a string, and then testing that string to see what to do.
- bpo-33127: The ssl module now compiles with LibreSSL 2.7.1.
- bpo-32505: Raise TypeError if a member variable of a dataclass is of type Field, but doesnât have a type annotation.
- bpo-33078: Fix the failure on OSX caused by the tests relying on sem_getvalue
- bpo-33116: Add âFieldâ to dataclasses.__all__.
- bpo-32896: Fix an error where subclassing a dataclass with a field that uses a default_factory would generate an incorrect class.
- bpo-33100: Dataclasses: If a field has a default value thatâs a MemberDescriptorType, then itâs from that field being in __slots__, not an actual default value.
- bpo-32953: If a non-dataclass inherits from a frozen dataclass, allow attributes to be added to the derived class. Only attributes from the frozen dataclass cannot be assigned to. Require all dataclasses in a hierarchy to be either all frozen or all non-frozen.
- bpo-33097: Raise RuntimeError when `executor.submit` is called during
interpreter shutdown.
- bpo-32968: Modulo and floor division involving Fraction and float should return float.
- bpo-33061: Add missing `NoReturn` to`__all__` in typing.py
- bpo-33078: Fix the size handling in multiprocessing.Queue when a pickling error occurs.
- bpo-33064: lib2to3 now properly supports trailing commas after `*args` and`**kwargs` in function signatures.
- bpo-33056: FIX properly close leaking fds in concurrent.futures.ProcessPoolExecutor.
- bpo-33021: Release the GIL during fstat() calls, avoiding hang of all threads when calling mmap.mmap(), os.urandom(), and random.seed(). Patch by Nir Soffer.
- bpo-31804: Avoid failing in multiprocessing.Process if the standard streams are closed or None at exit.
- bpo-33034: Providing an explicit error message when casting the port property to anything that is not an integer value using `urlparse()` and`urlsplit()` . Patch by Matt Eaton.
- bpo-30249: Improve struct.unpack_from() exception messages for problems with the buffer size and offset.
- bpo-33037: Skip sending/receiving data after SSL transport closing.
- bpo-27683: Fix a regression in `ipaddress` that result of`hosts()` is empty when the network is constructed by a tuple
containing an integer mask and only 1 bit left for addresses.
- bpo-22674: Add the strsignal() function in the signal module that returns the system description of the given signal, as returned by strsignal(3).
- bpo-32999: Fix C implementation of ```
ABC.__subclasscheck__(cls,
subclass)
```
crashed when`subclass` is not a type object.
- bpo-33009: Fix inspect.signature() for single-parameter partialmethods.
- bpo-32969: Expose several missing constants in zlib and fix corresponding documentation.
- bpo-32056: Improved exceptions raised for invalid number of channels and sample width when read an audio file in modules `aifc` ,`wave` and`sunau` .
- bpo-32970: Improved disassembly of the MAKE_FUNCTION instruction.
- bpo-32844: Fix wrong redirection of a low descriptor (0 or 1) to stderr in subprocess if another low descriptor is closed.
- bpo-32960: For dataclasses, disallow inheriting frozen from non-frozen classes, and also disallow inheriting non-frozen from frozen classes. This restriction will be relaxed at a future date.
- bpo-32713: Fixed tarfile.itn handling of out-of-bounds float values. Patch by Joffrey Fuhrer.
- bpo-32257: The ssl module now contains OP_NO_RENEGOTIATION constant, available with OpenSSL 1.1.0h or 1.1.1.
- bpo-32951: Direct instantiation of SSLSocket and SSLObject objects is now prohibited. The constructors were never documented, tested, or designed as public constructors. Users were suppose to use ssl.wrap_socket() or SSLContext.
- bpo-32929: Remove the tri-state parameter âhashâ, and add the boolean âunsafe_hashâ. If unsafe_hash is True, add a __hash__ function, but if a __hash__ exists, raise TypeError. If unsafe_hash is False, add a __hash__ based on the values of eq= and frozen=. The unsafe_hash=False behavior is the same as the old hash=None behavior. unsafe_hash=False is the default, just as hash=None used to be.
- bpo-32947: Add OP_ENABLE_MIDDLEBOX_COMPAT and test workaround for TLSv1.3 for future compatibility with OpenSSL 1.1.1.
- bpo-32146: Document the interaction between frozen executables and the spawn and forkserver start methods in multiprocessing.
- bpo-30622: The ssl module now detects missing NPN support in LibreSSL.
- bpo-32922: dbm.open() now encodes filename with the filesystem encoding rather than default encoding.
- bpo-32759: Free unused arenas in multiprocessing.heap.
- bpo-32859: In `os.dup2` , donât check every call whether the`dup3` syscall exists or not.
- bpo-32556: nt._getfinalpathname, nt._getvolumepathname and nt._getdiskusage now correctly convert from bytes.
- bpo-21060: Rewrite confusing message from setup.py upload from âNo dist file created in earlier commandâ to the more helpful âMust create and upload files in one commandâ.
- bpo-32857: In `tkinter` ,`after_cancel(None)` now raises a`ValueError` instead of canceling the first scheduled function.
Patch by Cheryl Sabella.
- bpo-32852: Make sure sys.argv remains as a list when running trace.
- bpo-31333: `_abc` module is added.  It is a speedup module with C
implementations for various functions and methods in`abc` .  Creating an
ABC subclass and calling`isinstance` or`issubclass` with an ABC
subclass are up to 1.5x faster. In addition, this makes Python start-up up
to 10% faster.Note that the new implementation hides internal registry and caches, previously accessible via private attributes `_abc_registry` ,`_abc_cache` , and`_abc_negative_cache` .  There are three debugging
helper methods that can be used instead`_dump_registry` ,`_abc_registry_clear` , and`_abc_caches_clear` .
- bpo-32841: Fixed `asyncio.Condition` issue which silently ignored
cancellation after notifying and cancelling a conditional lock. Patch by
Bar Harel.
- bpo-32819: ssl.match_hostname() has been simplified and no longer depends on re and ipaddress module for wildcard and IP addresses. Error reporting for invalid wildcards has been improved.
- bpo-19675: `multiprocessing.Pool` no longer leaks processes if its
initialization fails.
- bpo-32394: socket: Remove TCP_FASTOPEN,TCP_KEEPCNT,TCP_KEEPIDLE,TCP_KEEPINTVL flags on older version Windows during run-time.
- bpo-31787: Fixed refleaks of `__init__()` methods in various modules.
(Contributed by Oren Milman)
- bpo-30157: Fixed guessing quote and delimiter in csv.Sniffer.sniff() when only the last field is quoted. Patch by Jake Davis.
- bpo-30688: Added support of `\N{name}` escapes in regular expressions.
Based on patch by Jonathan Eunice.
- bpo-32792: collections.ChainMap() preserves the order of the underlying mappings.
- bpo-32775: `fnmatch.translate()` no longer produces patterns which
contain set operations. Sets starting with â[â or containing âââ, â&&â,
â~~â or â||â will be interpreted differently in regular expressions in
future versions. Currently they emit warnings. fnmatch.translate() now
avoids producing patterns containing such sets by accident.
- bpo-32622: Implement native fast sendfile for Windows proactor event loop.
- bpo-32777: Fix a rare but potential pre-exec child process deadlock in subprocess on POSIX systems when marking file descriptors inheritable on exec in the child process. This bug appears to have been introduced in 3.4.
- bpo-32647: The ctypes module used to depend on indirect linking for dlopen. The shared extension is now explicitly linked against libdl on platforms with dl.
- bpo-32749: A `dbm.dumb` database opened with flags ârâ is now
read-only.`dbm.dumb.open()` with flags ârâ and âwâ no longer creates
a database if it does not exist.
- bpo-32741: Implement `asyncio.TimerHandle.when()` method.
- bpo-32691: Use mod_spec.parent when running modules with pdb
- bpo-32734: Fixed `asyncio.Lock()` safety issue which allowed acquiring
and locking the same lock multiple times, without it being free. Patch by
Bar Harel.
- bpo-32727: Do not include name field in SMTP envelope from address. Patch by StÃ©phane Wirtel
- bpo-31453: Add TLSVersion constants and SSLContext.maximum_version / minimum_version attributes. The new API wraps OpenSSL 1.1 https://web.archive.org/web/20180309043602/https://www.openssl.org/docs/man1.1.0/ssl/SSL_CTX_set_min_proto_version.html feature.
- bpo-24334: Internal implementation details of ssl module were cleaned up. The SSLSocket has one less layer of indirection. Owner and session information are now handled by the SSLSocket and SSLObject constructor. Channel binding implementation has been simplified.
- bpo-31848: Fix the error handling in Aifc_read.initfp() when the SSND chunk is not found. Patch by Zackery Spytz.
- bpo-32585: Add Ttk spinbox widget to `tkinter.ttk` .  Patch by Alan D
Moore.
- bpo-32512: `profile` CLI accepts`-m module_name` as an alternative
to script path.
- bpo-8525: help() on a type now displays builtin subclasses. This is intended primarily to help with notification of more specific exception subclasses. Patch by Sanyam Khurana.
- bpo-31639: http.server now exposes a ThreadingHTTPServer class and uses it when the module is run with `-m` to cope with web browsers pre-opening
sockets.
- bpo-29877: compileall: import ProcessPoolExecutor only when needed, preventing hangs on low resource platforms
- bpo-32221: Various functions returning tuple containing IPv6 addresses now omit `%scope` part since the same information is already encoded in*scopeid* tuple item. Especially this speeds up`socket.recvfrom()` when it receives multicast packet since useless resolving of network
interface name is omitted.
- bpo-32147: `binascii.unhexlify()` is now up to 2 times faster. Patch
by Sergey Fedoseev.
- bpo-30693: The TarFile class now recurses directories in a reproducible way.
- bpo-30693: The ZipFile class now recurses directories in a reproducible way.
- bpo-31680: Added `curses.ncurses_version` .
- bpo-31908: Fix output of cover files for `trace` module command-line
tool. Previously emitted cover files only when`--missing` option was
used. Patch by Michael Selik.
- bpo-31608: Raise a `TypeError` instead of crashing if a`collections.deque` subclass returns a non-deque from`__new__` . Patch
by Oren Milman.
- bpo-31425: Add support for sockets of the AF_QIPCRTR address family, supported by the Linux kernel. This is used to communicate with services, such as GPS or radio, running on Qualcomm devices. Patch by Bjorn Andersson.
- bpo-22005: Implemented unpickling instances of `datetime` ,`date` and`time` pickled by Python 2.`encoding='latin1'` should
be used for successful decoding.
- bpo-27645: `sqlite3.Connection` now exposes a`backup` method, if the underlying SQLite
library is at version 3.6.11 or higher.  Patch by Lele Gaifax.
- bpo-16865: Support arrays >=2GiB in `ctypes` .  Patch by Segev Finer.
- bpo-31508: Removed support of arguments in `tkinter.ttk.Treeview.selection` . It was deprecated in 3.6.  Use
specialized methods like`selection_set` for changing the selection.
- bpo-29456: Fix bugs in hangul normalization: u1176, u11a7 and u11c3

### DocumentationÂ¶

- bpo-21257: Document `http.client.parse_headers()` .
- bpo-34764: Improve example of iter() with 2nd sentinel argument.
- bpo-35564: Explicitly set master_doc variable in conf.py for compliance with Sphinx 2.0
- bpo-35511: Specified that profile.Profile class doesnât not support enable or disable methods. Also, elaborated that Profile object as a context manager is only supported in cProfile module.
- bpo-10536: Enhance the gettext docs. Patch by Ãric Araujo
- bpo-35089: Remove mention of `typing.io` and`typing.re` . Their types
should be imported from`typing` directly.
- bpo-35038: Fix the documentation about an unexisting `f_restricted` attribute in the frame object. Patch by StÃ©phane Wirtel
- bpo-35042: Replace PEP XYZ by the pep role and allow to use the direct links to the PEPs.
- bpo-35044: Fix the documentation with the role `exc` for the
appropriated exception. Patch by StÃ©phane Wirtel
- bpo-35035: Rename documentation for `email.utils` to`email.utils.rst` .
- bpo-34967: Use app.add_object_type() instead of the deprecated Sphinx function app.description_unit()
- bpo-34913: Add documentation about the new command line interface of the gzip module.
- bpo-32174: chm document displays non-ASCII characters properly on some MBCS Windows systems.
- bpo-11233: Create availability directive for documentation. Original patch by Georg Brandl.
- bpo-34790: Document how passing coroutines to asyncio.wait() can be confusing.
- bpo-34552: Make clear that `==` operator sometimes is equivalent to`is` . The`<` ,`<=` ,`>` and`>=` operators are only defined
where they make sense.
- bpo-28617: Fixed info in the stdtypes docs concerning the types that support membership tests.
- bpo-20177: Migrate datetime.date.fromtimestamp to Argument Clinic. Patch by Tim Hoffmann.
- bpo-34065: Fix wrongly written basicConfig documentation markup syntax
- bpo-33460: replaced ellipsis with correct error codes in tutorial chapter 3.
- bpo-33847: Add â@â operator entry to index.
- bpo-33409: Clarified the relationship between **PEP 538** âs
PYTHONCOERCECLOCALE and PEP 540âs PYTHONUTF8 mode.
- bpo-33197: Add versionadded tag to the documentation of ParameterKind.description
- bpo-17045: Improve the C-API doc for PyTypeObject. This includes adding several quick-reference tables and a lot of missing slot/typedef entries. The existing entries were also cleaned up with a slightly more consistent format.
- bpo-33736: Improve the documentation of `asyncio.open_connection()` ,`asyncio.start_server()` and their UNIX socket counterparts.
- bpo-23859: Document that `asyncio.wait()` does not cancel its futures on
timeout.
- bpo-32436: Document **PEP 567** changes to asyncio.
- bpo-33604: Update HMAC md5 default to a DeprecationWarning, bump removal to 3.8.
- bpo-33594: Document `getargspec` ,`from_function` and`from_builtin` as deprecated in their respective docstring, and include version since
deprecation in DeprecationWarning message.
- bpo-33503: Fix broken pypi link
- bpo-33421: Add missing documentation for `typing.AsyncContextManager` .
- bpo-33487: BZ2file now emit a DeprecationWarning when buffering=None is passed, the deprecation message and documentation also now explicitly state it is deprecated since 3.0.
- bpo-33378: Add Korean language switcher for https://docs.python.org/3/
- bpo-33276: Clarify that the `__path__` attribute on modules cannot be
just any value.
- bpo-33201: Modernize documentation for writing C extension types.
- bpo-33195: Deprecate `Py_UNICODE` usage in`c-api/arg` document.`Py_UNICODE` related APIs are deprecated since Python 3.3, but it is
missed in the document.
- bpo-33126: Document PyBuffer_ToContiguous().
- bpo-27212: Modify documentation for the `islice()` recipe to consume
initial values up to the start index.
- bpo-28247: Update `zipapp` documentation to describe how to make
standalone applications.
- bpo-18802: Documentation changes for ipaddress. Patch by Jon Foster and Berker Peksag.
- bpo-27428: Update documentation to clarify that `WindowsRegistryFinder` implements`MetaPathFinder` . (Patch by Himanshu Lakhara)
- bpo-28124: The ssl module function ssl.wrap_socket() has been de-emphasized and deprecated in favor of the more secure and efficient SSLContext.wrap_socket() method.
- bpo-17232: Clarify docs for -O and -OO. Patch by Terry Reedy.
- bpo-32436: Add documentation for the contextvars module (PEP 567).
- bpo-32800: Update link to w3c doc for xml default namespaces.
- bpo-11015: Update `test.support` documentation.
- bpo-32613: Update the faq/windows.html to use the py command from PEP 397 instead of python.
- bpo-8722: Document `__getattr__()` behavior when property`get()` method raises`AttributeError` .
- bpo-32614: Modify RE examples in documentation to use raw strings to prevent `DeprecationWarning` and add text to REGEX HOWTO to highlight
the deprecation.
- bpo-20709: Remove the paragraph where we explain that os.utime() does not support a directory as path under Windows. Patch by Jan-Philip Gehrcke
- bpo-32722: Remove the bad example in the tutorial of the Generator Expression. Patch by StÃ©phane Wirtel
- bpo-31972: Improve docstrings for `pathlib.PurePath` subclasses.
- bpo-30607: Use the externalized `python-docs-theme` package when
building the documentation.
- bpo-8243: Add a note about curses.addch and curses.addstr exception behavior when writing outside a window, or pad.
- bpo-32337: Update documentation related with `dict` order.
- bpo-25041: Document `AF_PACKET` in the`socket` module.
- bpo-31432: Clarify meaning of CERT_NONE, CERT_OPTIONAL, and CERT_REQUIRED flags for ssl.SSLContext.verify_mode.

### TestsÂ¶

- bpo-35772: Fix sparse file tests of test_tarfile on ppc64 with the tmpfs filesystem. Fix the function testing if the filesystem supports sparse files: create a file which contains data and âholesâ, instead of creating a file which contains no data. tmpfs effective block size is a page size (tmpfs lives in the page cache). RHEL uses 64 KiB pages on aarch64, ppc64, ppc64le, only s390x and x86_64 use 4 KiB pages, whereas the test punch holes of 4 KiB.
- bpo-35045: Make ssl tests less strict and also accept TLSv1 as system default. The changes unbreaks test_min_max_version on Fedora 29.
- bpo-32710: `test_asyncio/test_sendfile.py` now resets the event loop
policy using`tearDownModule()` as done in other tests, to prevent a
warning when running tests on Windows.
- bpo-33717: test.pythoninfo now logs information of all clocks, not only time.time() and time.perf_counter().
- bpo-35488: Add a test to pathlibâs Path.match() to verify it does not support glob-style ** recursive pattern matching.
- bpo-31731: Fix a race condition in `check_interrupted_write()` of
test_io: create directly the thread with SIGALRM signal blocked, rather
than blocking the signal later from the thread. Previously, it was
possible that the thread gets the signal before the signal is blocked.
- bpo-35424: Fix test_multiprocessing_main_handling: use `multiprocessing.Pool` with a context manager and then explicitly
join the pool.
- bpo-35519: Rename `test.bisect` module to`test.bisect_cmd` to
avoid conflict with`bisect` module when running directly a test like`./python Lib/test/test_xmlrpc.py` .
- bpo-35513: Replace `time.time()` with`time.monotonic()` in tests
to measure time delta.
- bpo-34279: `test.support.run_unittest()` no longer raise`TestDidNotRun` if the test result contains skipped tests. The
exception is now only raised if no test have been run and no test have
been skipped.
- bpo-35412: Add testcase to `test_future4` : check unicode literal.
- bpo-26704: Added test demonstrating double-patching of an instance method. Patch by Anthony Sottile.
- bpo-33725: test_multiprocessing_fork may crash on recent versions of macOS. Until the issue is resolved, skip the test on macOS.
- bpo-35352: Modify test_asyncio to use the certificate set from the test directory.
- bpo-35317: Fix `mktime()` overflow error in`test_email` : run`test_localtime_daylight_true_dst_true()` and`test_localtime_daylight_false_dst_true()` with a specific timezone.
- bpo-21263: After several reports that test_gdb does not work properly on macOS and since gdb is not shipped by default anymore, test_gdb is now skipped on macOS when LLVM Clang has been used to compile Python. Patch by Lysandros Nikolaou
- bpo-34279: regrtest issue a warning when no tests have been executed in a particular test file. Also, a new final result state is issued if no test have been executed across all test files. Patch by Pablo Galindo.
- bpo-34962: make docstest in Doc now passes., and is enforced in CI
- bpo-23596: Use argparse for the command line of the gzip module. Patch by Antony Lee
- bpo-34537: Fix `test_gdb.test_strings()` when`LC_ALL=C` and GDB was
compiled with Python 3.6 or earlier.
- bpo-34587: test_socket: Remove RDSTest.testCongestion(). The test tries to fill the receiverâs socket buffer and expects an error. But the RDS protocol doesnât require that. Moreover, the Linux implementation of RDS expects that the producer of the messages reduces its rate, itâs not the role of the receiver to trigger an error. The test fails on Fedora 28 by design, so just remove it.
- bpo-34661: Fix test_shutil if unzip doesnât support -t.
- bpo-34200: Fixed non-deterministic flakiness of test_pkg by not using the scary test.support.module_cleanup() logic to save and restore sys.modules contents between test cases.
- bpo-34569: The experimental PEP 554 data channels now correctly pass negative PyLong objects between subinterpreters on 32-bit systems. Patch by Michael Felt.
- bpo-34594: Fix usage of hardcoded `errno` values in the tests.
- bpo-34579: Fix test_embed for AIX Patch by Michael Felt
- bpo-34542: Use 3072 RSA keys and SHA-256 signature for test certs and keys.
- bpo-11193: Remove special condition for AIX in `test_subprocess.test_undecodable_env`
- bpo-34347: Fix `test_utf8_mode.test_cmd_line` for AIX
- bpo-34490: On AIX with AF_UNIX family sockets getsockname() does not provide âsocknameâ, so skip calls to transport.get_extra_info(âsocknameâ)
- bpo-34391: Fix ftplib test for TLS 1.3 by reading from data socket.
- bpo-11192: Fix `test_socket` on AIX 6.1 and later IPv6 zone id supports
only supported by`inet_pton6_zone()` . Switch to runtime-based`platform.system()` to establish current platform rather than build-time
based`sys.platform()`
- bpo-34399: Update all RSA keys and DH params to use at least 2048 bits.
- bpo-34373: Fix `test_mktime` and`test_pthread_getcpuclickid` tests
for AIX Add range checking for`_PyTime_localtime` for AIX Patch by
Michael Felt
- bpo-11191: Skip the distutils test âtest_search_cppâ when using XLC as compiler patch by aixtools (Michael Felt)
- Improved an error message when mock assert_has_calls fails.
- bpo-33746: Fix test_unittest when run in verbose mode.
- bpo-33901: Fix test_dbm_gnu on macOS with gdbm 1.15: add a larger value to make sure that the file size changes.
- bpo-33873: Fix a bug in `regrtest` that caused an extra test to run if
âhuntrleaks/-R was used. Exit with error in case that invalid parameters
are specified to âhuntrleaks/-R (at least one warmup run and one
repetition must be used).
- bpo-33562: Check that a global asyncio event loop policy is not left behind by any tests.
- bpo-33655: Ignore test_posix_fallocate failures on BSD platforms that might be due to running on ZFS.
- bpo-32962: Fixed test_gdb when Python is compiled with flags -mcet -fcf-protection -O0.
- bpo-33358: Fix `test_embed.test_pre_initialization_sys_options()` when
the interpreter is built with`--enable-shared` .
- bpo-32872: Avoid regrtest compatibility issue with namespace packages.
- bpo-32517: Fix failing `test_asyncio` on macOS 10.12.2+ due to transport
of`KqueueSelector` loop was not being closed.
- bpo-32663: Making sure the `SMTPUTF8SimTests` class of tests gets run in`test_smtplib.py` .
- bpo-27643: Test_C test case needs âsigned shortâ bitfields, but the IBM XLC compiler (on AIX) does not support this Skip the code and test when AIX and XLC are used Applicable to Python2-2.7 and later
- bpo-19417: Add test_bdb.py.
- bpo-31809: Add tests to verify connection with secp ECDH curves.

### BuildÂ¶

- bpo-34691: The _contextvars module is now built into the core Python library on Windows.
- bpo-35683: Improved Azure Pipelines build steps and now verifying layouts correctly
- bpo-35642: Remove asynciomodule.c from pythoncore.vcxproj
- bpo-35550: Fix incorrect Solaris #ifdef checks to look for __sun && __SVR4 instead of sun when compiling.
- bpo-35499: `make profile-opt` no longer replaces`CFLAGS_NODIST` with`CFLAGS` . It now adds profile-guided optimization (PGO) flags to`CFLAGS_NODIST` : existing`CFLAGS_NODIST` flags are kept.
- bpo-35257: Avoid leaking the linker flags from Link Time Optimizations (LTO) into distutils when compiling C extensions.
- bpo-35351: When building Python with clang and LTO, LTO flags are no longer passed into CFLAGS to build third-party C extensions through distutils.
- bpo-35139: Fix a compiler error when statically linking `pyexpat` in`Modules/Setup` .
- bpo-35059: PCbuild: Set InlineFunctionExpansion to OnlyExplicitInline (â/Ob1â option) in pyproject.props in Debug mode to expand functions marked as inline. This change should make Python compiled in Debug mode a little bit faster on Windows.
- bpo-35011: Restores the use of pyexpatns.h to isolate our embedded copy of the expat C library so that its symbols do not conflict at link or dynamic loading time with an embedding application or other extension modules with their own version of libexpat.
- bpo-28015: Have âwith-lto works correctly with clang.
- bpo-34765: Update the outdated install-sh file to the latest revision from automake v1.16.1
- bpo-34585: Check for floating-point byte order in configure.ac using compilation tests instead of executing code, so that these checks work in cross-compiled builds.
- bpo-34710: Fixed SSL module build with OpenSSL & pedantic CFLAGS.
- bpo-34582: Add JUnit XML output for regression tests and update Azure DevOps builds.
- bpo-34081: Make Sphinx warnings as errors in the Docs Makefile.
- bpo-34555: Fix for case where it was not possible to have both `HAVE_LINUX_VM_SOCKETS_H` and`HAVE_SOCKADDR_ALG` be undefined.
- bpo-33015: Fix an undefined behaviour in the pthread implementation of `PyThread_start_new_thread()` : add a function wrapper to always
return`NULL` .
- bpo-34245: The Python shared library is now installed with write permission (mode 0755), which is the standard way of installing such libraries.
- bpo-34121: Fix detection of C11 atomic support on clang.
- bpo-32430: Rename Modules/Setup.dist to Modules/Setup, and remove the necessity to copy the former manually to the latter when updating the local source tree.
- bpo-30345: Add -g to LDFLAGS when compiling with LTO to get debug symbols.
- bpo-5755: Move `-Wstrict-prototypes` option to`CFLAGS_NODIST` from`OPT` . This option emitted annoying warnings when building extension
modules written in C++.
- bpo-33614: Ensures module definition files for the stable ABI on Windows are correctly regenerated.
- bpo-33648: The âwith-c-locale-warning configuration flag has been removed. It has had no effect for about a year.
- bpo-33522: Enable CI builds on Visual Studio Team Services at https://python.visualstudio.com/cpython
- bpo-33512: configureâs check for âlong doubleâ has been simplified
- bpo-33483: C compiler is now correctly detected from the standard environment variables. âwithout-gcc and âwith-icc options have been removed.
- bpo-33394: Enable the verbose build for extension modules, when GNU make is passed macros on the command line.
- bpo-33393: Update config.guess and config.sub files.
- bpo-33377: Add new triplets for mips r6 and riscv variants (used in extension suffixes).
- bpo-32232: By default, modules configured in `Modules/Setup` are no
longer built with`-DPy_BUILD_CORE` . Instead, modules that specifically
need that preprocessor definition include it in their individual entries.
- bpo-33182: The embedding tests can once again be built with clang 6.0
- bpo-33163: Upgrade pip to 9.0.3 and setuptools to v39.0.1.
- bpo-33012: gcc 8 has added a new warning heuristic to detect invalid function casts and a stock python build seems to hit that warning quite often. The most common is the cast of a METH_NOARGS function (that uses just one argument) to a PyCFunction. Fix this by adding a dummy argument to all functions that implement METH_NOARGS.
- bpo-32898: Fix the python debug build when using COUNT_ALLOCS.
- bpo-29442: Replace optparse with argparse in setup.py

### WindowsÂ¶

- bpo-35890: Fix API calling consistency of GetVersionEx and wcstok.
- bpo-32560: The `py` launcher now forwards its`STARTUPINFO` structure
to child processes.
- bpo-35854: Fix EnvBuilder and âsymlinks in venv on Windows
- bpo-35811: Avoid propagating venv settings when launching via py.exe
- bpo-35797: Fix default executable used by the multiprocessing module
- bpo-35758: Allow building on ARM with MSVC.
- bpo-29734: Fix handle leaks in os.stat on Windows.
- bpo-35596: Use unchecked PYCs for the embeddable distro to avoid zipimport restrictions.
- bpo-35596: Fix vcruntime140.dll being added to embeddable distro multiple times.
- bpo-35402: Update Windows build to use Tcl and Tk 8.6.9
- bpo-35401: Updates Windows build to OpenSSL 1.1.0j
- bpo-34977: venv on Windows will now use a python.exe redirector rather than copying the actual binaries from the base environment.
- bpo-34977: Adds support for building a Windows App Store package
- bpo-35067: Remove _distutils_findvs module and use vswhere.exe instead.
- bpo-32557: Allow shutil.disk_usage to take a file path on Windows
- bpo-34770: Fix a possible null pointer dereference in pyshellext.cpp.
- bpo-34603: Fix returning structs from functions produced by MSVC
- bpo-34581: Guard MSVC-specific code in socketmodule.c with ```
#ifdef
_MSC_VER
```
.
- bpo-34532: Fixes exit code of list version arguments for py.exe.
- bpo-34062: Fixed the ââlistâ and ââlist-pathsâ arguments for the py.exe launcher
- bpo-34225: Ensure INCLUDE and LIB directories do not end with a backslash.
- bpo-34011: A suite of code has been changed which copied across DLLs and init.tcl from the running Python location into a venv being created. These copies are needed only when running from a Python source build, and the copying code is now only run when that is the case, rather than whenever a venv is created.
- bpo-34006: Revert line length limit for Windows help docs. The line-length limit is not needed because the pages appear in a separate app rather than on a browser tab. It can also interact badly with the DPI setting.
- bpo-31546: Restore running PyOS_InputHook while waiting for user input at the prompt. The restores integration of interactive GUI windows (such as Matplotlib figures) with the prompt on Windows.
- bpo-30237: Output error when ReadConsole is canceled by CancelSynchronousIo instead of crashing.
- bpo-33895: GIL is released while calling functions that acquire Windows loader lock.
- bpo-33720: Reduces maximum marshal recursion depth on release builds.
- bpo-29097: Fix bug where `datetime.fromtimestamp()` erroneously throws
an`OSError` on Windows for values between 0 and 86400. Patch by
Ammar Askar.
- bpo-33316: PyThread_release_lock always fails
- bpo-33184: Update Windows installer to use OpenSSL 1.1.0h.
- bpo-32890: Fix usage of GetLastError() instead of errno in os.execve() and os.truncate().
- bpo-33016: Fix potential use of uninitialized memory in nt._getfinalpathname
- bpo-32903: Fix a memory leak in os.chdir() on Windows if the current directory is set to a UNC path.
- bpo-32901: Update Tcl and Tk versions to 8.6.8
- bpo-31966: Fixed WindowsConsoleIO.write() for writing empty data.
- bpo-32409: Ensures activate.bat can handle Unicode contents.
- bpo-32457: Improves handling of denormalized executable path when launching Python.
- bpo-32370: Use the correct encoding for ipconfig output in the uuid module. Patch by Segev Finer.
- bpo-29248: Fix `os.readlink()` on Windows, which was mistakenly
treating the`PrintNameOffset` field of the reparse data buffer as a
number of characters instead of bytes. Patch by Craig Holmquist and SSE4.
- bpo-1104: Correctly handle string length in `msilib.SummaryInfo.GetProperty()` to prevent it from truncating the
last character.

### macOSÂ¶

- bpo-35401: Update macOS installer to use OpenSSL 1.1.0j.
- bpo-35025: Properly guard the use of the `CLOCK_GETTIME` et al. macros
in`timemodule` on macOS.
- bpo-24658: On macOS, fix reading from and writing into a file with a size larger than 2 GiB.
- bpo-34405: Update to OpenSSL 1.1.0i for macOS installer builds.
- bpo-33635: In macOS stat on some file descriptors (/dev/fd/3 f.e) will result in bad file descriptor OSError. Guard against this exception was added in is_dir, is_file and similar methods. DirEntry.is_dir can also throw this exception so _RecursiveWildcardSelector._iterate_directories was also extended with the same error ignoring pattern.
- bpo-13631: The .editrc file in userâs home directory is now processed correctly during the readline initialization through editline emulation on macOS.
- bpo-33184: Update macOS installer build to use OpenSSL 1.1.0h.
- bpo-32726: Build and link with private copy of Tcl/Tk 8.6 for the macOS 10.6+ installer. The 10.9+ installer variant already does this. This means that the Python 3.7 provided by the python.org macOS installers no longer need or use any external versions of Tcl/Tk, either system-provided or user-installed, such as ActiveTcl.
- bpo-32901: Update macOS 10.9+ installer to Tcl/Tk 8.6.8.
- bpo-31903: In `_scproxy` , drop the GIL when calling into`SystemConfiguration` to avoid deadlocks.

### IDLEÂ¶

- bpo-35770: IDLE macosx deletes Options => Configure IDLE. It previously deleted Window => Zoom Height by mistake. (Zoom Height is now on the Options menu). On Mac, the settings dialog is accessed via Preferences on the IDLE menu.
- bpo-35769: Change IDLEâs new file name from âUntitledâ to âuntitledâ
- bpo-35660: Fix imports in idlelib.window.
- bpo-35641: Proper format `calltip` when the function has no docstring.
- bpo-33987: Use ttk Frame for ttk widgets.
- bpo-34055: Fix erroneous âsmartâ indents and newlines in IDLE Shell.
- bpo-35591: Find Selection now works when selection not found.
- bpo-35196: Speed up squeezer line counting.
- bpo-35598: Update config_key: use PEP 8 names and ttk widgets, make some objects global, and add tests.
- bpo-28097: Add Previous/Next History entries to Shell menu.
- bpo-35208: Squeezer now properly counts wrapped lines before newlines.
- bpo-35555: Gray out Code Context menu entry when itâs not applicable.
- bpo-35521: Document the IDLE editor code context feature. Add some internal references within the IDLE doc.
- bpo-22703: The Code Context menu label now toggles between Show/Hide Code Context. The Zoom Height menu now toggles between Zoom/Restore Height. Zoom Height has moved from the Window menu to the Options menu.
- bpo-35213: Where appropriate, use âmacOSâ in idlelib.
- bpo-34864: On macOS, warn if the system preference âPrefer tabs when opening documentsâ is set to âAlwaysâ.
- bpo-34864: Document two IDLE on MacOS issues. The System Preferences Dock âprefer tabs alwaysâ setting disables some IDLE features. Menus are a bit different than as described for Windows and Linux.
- bpo-35202: Remove unused imports from lib/idlelib
- bpo-33000: Document that IDLEâs shell has no line limit. A program that runs indefinitely can overfill memory.
- bpo-23220: Explain how IDLEâs Shell displays output.
- bpo-35099: Improve the doc about IDLE running user code. The section is renamed from âIDLE â console differencesâ is renamed âRunning user codeâ. It mostly covers the implications of using custom `sys.std`*xxx*
objects.
- bpo-35097: Add IDLE doc subsection explaining editor windows. Topics include opening, title and status bar, .py* extension, and running.
- bpo-35093: Document the IDLE document viewer in the IDLE doc. Add a paragraph in âHelp and preferencesâ, âHelp sourcesâ subsection.
- bpo-35088: Update idlelib.help.copy_string docstring. We now use git and backporting instead of hg and forward merging.
- bpo-35087: Update idlelib help files for the current doc build. The main change is the elimination of chapter-section numbers.
- bpo-34548: Use configured color theme for read-only text views.
- bpo-1529353: Enable âsqueezingâ of long outputs in the shell, to avoid performance degradation and to clean up the history without losing it. Squeezed outputs may be copied, viewed in a separate window, and âunsqueezedâ.
- bpo-34047: Fixed mousewheel scrolling direction on macOS.
- bpo-34275: Make IDLE calltips always visible on Mac. Some MacOS-tk combinations need .update_idletasks(). Patch by Kevin Walzer.
- bpo-34120: Fix unresponsiveness after closing certain windows and dialogs.
- bpo-33975: Avoid small type when running htests. Since part of the purpose of human-viewed tests is to determine that widgets look right, it is important that they look the same for testing as when running IDLE.
- bpo-33905: Add test for idlelib.stackview.StackBrowser.
- bpo-33924: Change mainmenu.menudefs key âwindowsâ to âwindowâ. Every other menudef key is lowercase version of main menu entry.
- bpo-33906: Rename idlelib.windows as window Match Window on the main menu and remove last plural module name.
- bpo-33917: Fix and document idlelib/idle_test/template.py. The revised file compiles, runs, and tests OK. idle_test/README.txt explains how to use it to create new IDLE test files.
- bpo-33904: IDLE: In rstrip, rename class RstripExtension as Rstrip
- bpo-33907: For consistency and clarity, rename an IDLE module and classes. Module calltips and its class CallTips are now calltip and Calltip. In module calltip_w, class CallTip is now CalltipWindow.
- bpo-33856: Add âhelpâ in the welcome message of IDLE
- bpo-33839: IDLE: refactor ToolTip and CallTip and add documentation and tests
- bpo-33855: Minimally test all IDLE modules. Add missing files, import module, instantiate classes, and check coverage. Check existing files.
- bpo-33656: On Windows, add API call saying that tk scales for DPI. On Windows 8.1+ or 10, with DPI compatibility properties of the Python binary unchanged, and a monitor resolution greater than 96 DPI, this should make text and lines sharper. It should otherwise have no effect.
- bpo-33768: Clicking on a context line moves that line to the top of the editor window.
- bpo-33763: IDLE: Use read-only text widget for code context instead of label widget.
- bpo-33664: Scroll IDLE editor text by lines. Previously, the mouse wheel and scrollbar slider moved text by a fixed number of pixels, resulting in partial lines at the top of the editor box. The change also applies to the shell and grep output windows, but not to read-only text views.
- bpo-33679: Enable theme-specific color configuration for Code Context. Use the Highlights tab to see the setting for built-in themes or add settings to custom themes.
- bpo-33642: Display up to maxlines non-blank lines for Code Context. If there is no current context, show a single blank line.
- bpo-33628: IDLE: Cleanup codecontext.py and its test.
- bpo-33564: IDLEâs code context now recognizes async as a block opener.
- bpo-21474: Update word/identifier definition from ascii to unicode. In text and entry boxes, this affects selection by double-click, movement left/right by control-left/right, and deletion left/right by control-BACKSPACE/DEL.
- bpo-33204: IDLE: consistently color invalid string prefixes. A âuâ string prefix cannot be paired with either ârâ or âfâ. Consistently color as much of the prefix, starting at the right, as is valid. Revise and extend colorizer test.
- bpo-32984: Set `__file__` while running a startup file.  Like Python,
IDLE optionally runs one startup file in the Shell window before
presenting the first interactive input prompt.  For IDLE,`-s` runs a
file named in environmental variable`IDLESTARTUP` or`PYTHONSTARTUP` ;`-r file` runs`file` .  Python sets`__file__` to the startup file name before running the file and unsets
it before the first prompt.  IDLE now does the same when run normally,
without the`-n` option.
- bpo-32940: Simplify and rename StringTranslatePseudoMapping in pyparse.
- bpo-32916: Change `str` to`code` in pyparse.
- bpo-32905: Remove unused code in pyparse module.
- bpo-32874: Add tests for pyparse.
- bpo-32837: Using the system and place-dependent default encoding for open() is a bad idea for IDLEâs system and location-independent files.
- bpo-32826: Add âencoding=utf-8â to open() in IDLEâs test_help_about. GUI test test_file_buttons() only looks at initial ascii-only lines, but failed on systems where open() defaults to âasciiâ because readline() internally reads and decodes far enough ahead to encounter a non-ascii character in CREDITS.txt.
- bpo-32831: Add docstrings and tests for codecontext.
- bpo-32765: Update configdialog General tab docstring to add new widgets to the widget list.

### Tools/DemosÂ¶

- bpo-35884: Add a benchmark script for timing various ways to access variables: `Tools/scripts/var_access_benchmark.py` .
- bpo-34989: python-gdb.py now handles errors on computing the line number of a Python frame.
- bpo-20260: Argument Clinic now has non-bitwise unsigned int converters.
- bpo-32962: python-gdb now catches `UnicodeDecodeError` exceptions when
calling`string()` .
- bpo-32962: python-gdb now catches ValueError on read_var(): when Python has no debug symbols for example.
- bpo-33189: **pygettext.py** now recognizes only literal strings as
docstrings and translatable strings, and rejects bytes literals and
f-string expressions.
- bpo-31920: Fixed handling directories as arguments in the `pygettext` script. Based on patch by Oleg Krasnikov.
- bpo-29673: Fix pystackv and pystack gdbinit macros.
- bpo-25427: Remove the pyvenv script in favor of `python3 -m venv` in
order to lower confusion as to what Python interpreter a virtual
environment will be created for.
- bpo-32885: Add an `-n` flag for`Tools/scripts/pathfix.py` to disable
automatic backup creation (files with`~` suffix).
- bpo-32222: Fix pygettext not extracting docstrings for functions with type annotated arguments. Patch by Toby Harradine.
- bpo-31583: Fix 2to3 for using with âadd-suffix option but without âoutput-dir option for relative path to files in current directory.

### C APIÂ¶

- bpo-35713: The `PyByteArray_Init()` and`PyByteArray_Fini()` functions have been removed. They did nothing since Python 2.7.4 and
Python 3.2.0, were excluded from the limited API (stable ABI), and were
not documented.
- bpo-33817: Fixed `_PyBytes_Resize()` for empty bytes objects.
- bpo-35322: Fix memory leak in `PyUnicode_EncodeLocale()` and`PyUnicode_EncodeFSDefault()` on error handling.
- bpo-35059: The following C macros have been converted to static inline functions: `Py_INCREF()` ,`Py_DECREF()` ,`Py_XINCREF()` ,`Py_XDECREF()` ,`PyObject_INIT()` ,`PyObject_INIT_VAR()` .
- bpo-35296: `make install` now also installs the internal API:`Include/internal/*.h` header files.
- bpo-35081: Internal APIs surrounded by `#ifdef Py_BUILD_CORE` have been
moved from`Include/*.h` headers to new header files`Include/internal/pycore_*.h` .
- bpo-35259: Conditionally declare `Py_FinalizeEx()` (new in 3.6)
based on Py_LIMITED_API. Patch by Arthur Neufeld.
- bpo-35081: The `_PyObject_GC_TRACK()` and`_PyObject_GC_UNTRACK()` macros have been removed from the public C
API.
- bpo-35134: Creation of a new `Include/cpython/` subdirectory.
- bpo-34725: Adds _Py_SetProgramFullPath so embedders may override sys.executable
- bpo-34910: Ensure that `PyObject_Print()` always returns`-1` on
error.  Patch by Zackery Spytz.
- bpo-34523: Py_DecodeLocale() and Py_EncodeLocale() now use the UTF-8 encoding on Windows if Py_LegacyWindowsFSEncodingFlag is zero.
- bpo-34193: Fix pluralization in TypeError messages in getargs.c and typeobject.c: â1 argumentâ instead of â1 argumentsâ and â1 elementâ instead of â1 elementsâ.
- bpo-34127: Return grammatically correct error message based on argument count. Patch by Karthikeyan Singaravelan.
- bpo-23927: Fixed `SystemError` in`PyArg_ParseTupleAndKeywords()` when the`w*` format unit is used
for optional parameter.
- bpo-32455: Added `PyCompile_OpcodeStackEffectWithJump()` .
- bpo-34008: Py_Main() can again be called after Py_Initialize(), as in Python 3.6.
- bpo-32500: Fixed error messages for `PySequence_Size()` ,`PySequence_GetItem()` ,`PySequence_SetItem()` and`PySequence_DelItem()` called with a mapping and`PyMapping_Size()` called with a sequence.
- bpo-33818: `PyExceptionClass_Name()` will now return```
const char
*
```
instead of`char *` .
- bpo-33042: Embedding applications may once again call PySys_ResetWarnOptions, PySys_AddWarnOption, and PySys_AddXOption prior to calling Py_Initialize.
- bpo-32374: Document that m_traverse for multi-phase initialized modules can be called with m_state=NULL, and add a sanity check
- bpo-30863: `PyUnicode_AsWideChar()` and`PyUnicode_AsWideCharString()` no longer cache the`wchar_t*` representation of string objects.

## Python 3.7.0 finalÂ¶

*Release date: 2018-06-27*

### LibraryÂ¶

- bpo-33851: Fix `ast.get_docstring()` for a node that lacks a
docstring.

### C APIÂ¶

- bpo-33932: Calling Py_Initialize() twice does nothing, instead of failing with a fatal error: restore the Python 3.6 behaviour.

## Python 3.7.0 release candidate 1Â¶

*Release date: 2018-06-12*

### Core and BuiltinsÂ¶

- bpo-33803: Fix a crash in hamt.c caused by enabling GC tracking for an object that hadnât all of its fields set to NULL.
- bpo-33706: Fix a crash in Python initialization when parsing the command line options. Thanks Christoph Gohlke for the bug report and the fix!
- bpo-30654: Fixed reset of the SIGINT handler to SIG_DFL on interpreter shutdown even when there was a custom handler set previously. Patch by Philipp Kerling.
- bpo-31849: Fix signed/unsigned comparison warning in pyhash.c.

### LibraryÂ¶

- bpo-30167: Prevent site.main() exception if PYTHONSTARTUP is set. Patch by Steve Weber.
- bpo-33812: Datetime instance d with non-None tzinfo, but with d.tzinfo.utcoffset(d) returning None is now treated as naive by the astimezone() method.
- bpo-30805: Avoid race condition with debug logging
- bpo-33694: asyncio: Fix a race condition causing data loss on pause_reading()/resume_reading() when using the ProactorEventLoop.
- bpo-32493: Correct test for `uuid_enc_be` availability in`configure.ac` . Patch by Michael Felt.
- bpo-33792: Add asyncio.WindowsSelectorEventLoopPolicy and asyncio.WindowsProactorEventLoopPolicy.
- bpo-33778: Update `unicodedata` âs database to Unicode version 11.0.0.
- bpo-33770: improve base64 exception message for encoded inputs of invalid length
- bpo-33769: asyncio/start_tls: Fix error message; cancel callbacks in case of an unhandled error; mark SSLTransport as closed if it is aborted.
- bpo-33767: The concatenation ( `+` ) and repetition (`*` ) sequence
operations now raise`TypeError` instead of`SystemError` when
performed on`mmap.mmap` objects.  Patch by Zackery Spytz.
- bpo-33734: asyncio/ssl: Fix AttributeError, increase default handshake timeout
- bpo-11874: Use a better regex when breaking usage into wrappable parts. Avoids bogus assertion errors from custom metavar strings.
- bpo-33582: Emit a deprecation warning for inspect.formatargspec

### DocumentationÂ¶

- bpo-33409: Clarified the relationship between **PEP 538** âs
PYTHONCOERCECLOCALE and PEP 540âs PYTHONUTF8 mode.
- bpo-33736: Improve the documentation of `asyncio.open_connection()` ,`asyncio.start_server()` and their UNIX socket counterparts.
- bpo-31432: Clarify meaning of CERT_NONE, CERT_OPTIONAL, and CERT_REQUIRED flags for ssl.SSLContext.verify_mode.

### BuildÂ¶

- bpo-5755: Move `-Wstrict-prototypes` option to`CFLAGS_NODIST` from`OPT` . This option emitted annoying warnings when building extension
modules written in C++.

### WindowsÂ¶

- bpo-33720: Reduces maximum marshal recursion depth on release builds.

### IDLEÂ¶

- bpo-33656: On Windows, add API call saying that tk scales for DPI. On Windows 8.1+ or 10, with DPI compatibility properties of the Python binary unchanged, and a monitor resolution greater than 96 DPI, this should make text and lines sharper. It should otherwise have no effect.
- bpo-33768: Clicking on a context line moves that line to the top of the editor window.
- bpo-33763: IDLE: Use read-only text widget for code context instead of label widget.
- bpo-33664: Scroll IDLE editor text by lines. Previously, the mouse wheel and scrollbar slider moved text by a fixed number of pixels, resulting in partial lines at the top of the editor box. The change also applies to the shell and grep output windows, but not to read-only text views.
- bpo-33679: Enable theme-specific color configuration for Code Context. Use the Highlights tab to see the setting for built-in themes or add settings to custom themes.
- bpo-33642: Display up to maxlines non-blank lines for Code Context. If there is no current context, show a single blank line.

## Python 3.7.0 beta 5Â¶

*Release date: 2018-05-30*

### Core and BuiltinsÂ¶

- bpo-33622: Fixed a leak when the garbage collector fails to add an object with the `__del__` method or referenced by it into the`gc.garbage` list.`PyGC_Collect()` can now be called when an
exception is set and preserves it.
- bpo-33509: Fix module_globals parameter of warnings.warn_explicit(): donât crash if module_globals is not a dict.
- bpo-20104: The new `os.posix_spawn` added in 3.7.0b1 was removed as we
are still working on what the API should look like.  Expect this in 3.8
instead.
- bpo-33475: Fixed miscellaneous bugs in converting annotations to strings and optimized parentheses in the string representation.
- bpo-33391: Fix a leak in set_symmetric_difference().
- bpo-28055: Fix unaligned accesses in siphash24(). Patch by Rolf Eike Beer.
- bpo-32911: Due to unexpected compatibility issues discovered during downstream beta testing, reverted bpo-29463. `docstring` field is
removed from Module, ClassDef, FunctionDef, and AsyncFunctionDef ast nodes
which was added in 3.7a1.  Docstring expression is restored as a first
statement in their body. Based on patch by Inada Naoki.
- bpo-21983: Fix a crash in `ctypes.cast()` in case the type argument is a
ctypes structured data type. Patch by Eryk Sun and Oren Milman.

### LibraryÂ¶

- bpo-32751: When cancelling the task due to a timeout, `asyncio.wait_for()` will now wait until the cancellation is complete.
- bpo-32684: Fix gather to propagate cancellation of itself even with return_exceptions.
- bpo-33654: Support protocol type switching in SSLTransport.set_protocol().
- bpo-33674: Pause the transport as early as possible to further reduce the risk of data_received() being called before connection_made().
- bpo-33674: Fix a race condition in SSLProtocol.connection_made() of asyncio.sslproto: start immediately the handshake instead of using call_soon(). Previously, data_received() could be called before the handshake started, causing the handshake to hang or fail.
- bpo-31647: Fixed bug where calling write_eof() on a _SelectorSocketTransport after itâs already closed raises AttributeError.
- bpo-32610: Make asyncio.all_tasks() return only pending tasks.
- bpo-32410: Avoid blocking on file IO in sendfile fallback code
- bpo-33469: Fix RuntimeError after closing loop that used run_in_executor
- bpo-33672: Fix Task.__repr__ crash with Cythonâs bogus coroutines
- bpo-33654: Fix transport.set_protocol() to support switching between asyncio.Protocol and asyncio.BufferedProtocol. Fix loop.start_tls() to work with asyncio.BufferedProtocols.
- bpo-33652: Pickles of type variables and subscripted generics are now future-proof and compatible with older Python versions.
- bpo-32493: Fixed `uuid.uuid1()` on FreeBSD.
- bpo-33618: Finalize and document preliminary and experimental TLS 1.3 support with OpenSSL 1.1.1
- bpo-33623: Fix possible SIGSGV when asyncio.Future is created in __del__
- bpo-30877: Fixed a bug in the Python implementation of the JSON decoder that prevented the cache of parsed strings from clearing after finishing the decoding. Based on patch by c-fos.
- bpo-33570: Change TLS 1.3 cipher suite settings for compatibility with OpenSSL 1.1.1-pre6 and newer. OpenSSL 1.1.1 will have TLS 1.3 ciphers enabled by default.
- bpo-28556: Do not simplify arguments to `typing.Union` . Now`Union[Manager, Employee]` is not simplified to`Employee` at runtime.
Such simplification previously caused several bugs and limited
possibilities for introspection.
- bpo-33540: Add a new `block_on_close` class attribute to`ForkingMixIn` and`ThreadingMixIn` classes of`socketserver` .
- bpo-33548: tempfile._candidate_tempdir_list should consider common TEMP locations
- bpo-33109: argparse subparsers are once again not required by default, reverting the change in behavior introduced by bpo-26510 in 3.7.0a2.
- bpo-33536: dataclasses.make_dataclass now checks for invalid field names and duplicate fields. Also, added a check for invalid field specifications.
- bpo-33542: Prevent `uuid.get_node` from using a DUID instead of a MAC on
Windows. Patch by Zvi Effron
- bpo-26819: Fix race condition with `ReadTransport.resume_reading` in
Windows proactor event loop.
- Fix failure in `typing.get_type_hints()` when ClassVar was provided as a
string forward reference.
- bpo-33505: Optimize asyncio.ensure_future() by reordering if checks: 1.17x faster.
- bpo-33497: Add errors param to cgi.parse_multipart and make an encoding in FieldStorage use the given errors (needed for Twisted). Patch by Amber Brown.
- bpo-33495: Change dataclasses.Fields repr to use the repr of each of its members, instead of str. This makes it more clear what each field actually represents. This is especially true for the âtypeâ member.
- bpo-33453: Fix dataclasses to work if using literal string type annotations or if using PEP 563 âPostponed Evaluation of Annotationsâ. Only specific string prefixes are detected for both ClassVar (âClassVarâ and âtyping.ClassVarâ) and InitVar (âInitVarâ and âdataclasses.InitVarâ).
- bpo-28556: Minor fixes in typing module: add annotations to `NamedTuple.__new__` , pass`*args` and`**kwds` in`Generic.__new__` .  Original PRs by Paulius Å arka and Chad Dombrova.
- bpo-20087: Updated alias mapping with glibc 2.27 supported locales.
- bpo-33422: Fix trailing quotation marks getting deleted when looking up byte/string literals on pydoc. Patch by AndrÃ©s Delfino.
- bpo-28167: The function `platform.linux_distribution` and`platform.dist` now trigger a`DeprecationWarning` and have been
marked for removal in Python 3.8
- bpo-33197: Update error message when constructing invalid inspect.Parameters Patch by Donghee Na.
- bpo-33263: Fix FD leak in `_SelectorSocketTransport` Patch by Vlad
Starostin.
- bpo-32861: The urllib.robotparserâs `__str__` representation now
includes wildcard entries and the âCrawl-delayâ and âRequest-rateâ fields.
Patch by Michael Lazar.
- bpo-32257: The ssl module now contains OP_NO_RENEGOTIATION constant, available with OpenSSL 1.1.0h or 1.1.1.
- bpo-16865: Support arrays >=2GiB in `ctypes` .  Patch by Segev Finer.

### DocumentationÂ¶

- bpo-23859: Document that `asyncio.wait()` does not cancel its futures on
timeout.
- bpo-32436: Document **PEP 567** changes to asyncio.
- bpo-33604: Update HMAC md5 default to a DeprecationWarning, bump removal to 3.8.
- bpo-33503: Fix broken pypi link
- bpo-33421: Add missing documentation for `typing.AsyncContextManager` .

### TestsÂ¶

### BuildÂ¶

- bpo-33614: Ensures module definition files for the stable ABI on Windows are correctly regenerated.
- bpo-33522: Enable CI builds on Visual Studio Team Services at https://python.visualstudio.com/cpython
- bpo-33012: Add `-Wno-cast-function-type` for gcc 8 for silencing
warnings about function casts like casting to PyCFunction in method
definition lists.

### macOSÂ¶

- bpo-13631: The .editrc file in userâs home directory is now processed correctly during the readline initialization through editline emulation on macOS.

### IDLEÂ¶

## Python 3.7.0 beta 4Â¶

*Release date: 2018-05-02*

### Core and BuiltinsÂ¶

- bpo-33363: Raise a SyntaxError for `async with` and`async for` statements outside of async functions.
- bpo-33128: Fix a bug that causes PathFinder to appear twice on sys.meta_path. Patch by Pablo Galindo Salgado.
- bpo-33312: Fixed clang ubsan (undefined behavior sanitizer) warnings in dictobject.c by adjusting how the internal struct _dictkeysobject shared keys structure is declared.
- bpo-33231: Fix potential memory leak in `normalizestring()` .
- bpo-33205: Change dict growth function from `round_up_to_power_2(used*2+hashtable_size/2)` to`round_up_to_power_2(used*3)` .  Previously, dict is shrunk only when`used == 0` . Now dict has more chance to be shrunk.
- bpo-29922: Improved error messages in âasync withâ when `__aenter__()` or`__aexit__()` return non-awaitable object.
- bpo-33199: Fix `ma_version_tag` in dict implementation is uninitialized
when copying from key-sharing dict.

### LibraryÂ¶

- bpo-33281: Fix ctypes.util.find_library regression on macOS.
- bpo-33383: Fixed crash in the get() method of the `dbm.ndbm` database
object when it is called with a single argument.
- bpo-33329: Fix multiprocessing regression on newer glibcs
- bpo-991266: Fix quoting of the `Comment` attribute of`http.cookies.SimpleCookie` .
- bpo-33131: Upgrade bundled version of pip to 10.0.1.
- bpo-33308: Fixed a crash in the `parser` module when converting an ST
object to a tree of tuples or lists with`line_info=False` and`col_info=True` .
- bpo-33266: lib2to3 now recognizes `rf'...'` strings.
- bpo-11594: Ensure line-endings are respected when using lib2to3.
- bpo-33254: Have `importlib.resources.contents()` and`importlib.abc.ResourceReader.contents()` return an iterable
instead of an iterator.
- bpo-33256: Fix display of `<module>` call in the html produced by`cgitb.html()` . Patch by StÃ©phane Blondon.
- bpo-33185: Fixed regression when running pydoc with the `-m` switch. (The regression was introduced in 3.7.0b3 by the resolution of
bpo-33053) This fix also changed pydoc to add`os.getcwd()` to`sys.path` when necessary, rather than adding`"."` .
- bpo-33169: Delete entries of `None` in`sys.path_importer_cache` when`importlib.machinery.invalidate_caches()` is called.
