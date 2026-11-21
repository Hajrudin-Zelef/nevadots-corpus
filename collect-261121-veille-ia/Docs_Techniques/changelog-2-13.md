---
id: collect-261121-veille-ia/docs-techniques/changelog-2-13
title: "Changelog"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["patch", "function", "macro", "directly", "string", "parser", "included", "python"]
source: https://docs.python.org/release/3.15.1/whatsnew/changelog.html
source_anchor: ""
source_lines: [18001, 19500]
section: "Docs_Techniques"
---
- bpo-39448: Add the âregen-frozenâ makefile target that regenerates the code for the frozen `__hello__` module.

### WindowsÂ¶

- bpo-43155: `PyCMethod_New()` is now present in`python3.lib` .

### macOSÂ¶

- bpo-41837: Update macOS installer build to use OpenSSL 1.1.1j.

### IDLEÂ¶

- bpo-43283: Document why printing to IDLEâs Shell is often slower than printing to a system terminal and that it can be made faster by pre-formatting a single string before printing.

### C APIÂ¶

- bpo-43278: Always put compiler and system information on the first line of the REPL welcome message.
- bpo-43270: Remove the private `_PyErr_OCCURRED()` macro: use the public`PyErr_Occurred()` function instead.
- bpo-35134: Move odictobject.h, parser_interface.h, picklebufobject.h, pydebug.h, and pyfpe.h into the cpython/ directory. They must not be included directly, as they are already included by Python.h: Include Files.
- bpo-35134: Move pyarena.h, pyctype.h, and pytime.h into the cpython/ directory. They must not be included directly, as they are already included by Python.h: Include Files.
- bpo-40170: `PyExceptionClass_Name()` is now always declared as a
function, in order to hide implementation details. The macro accessed`PyTypeObject.tp_name` directly.  Patch by Erlend E. Aasland.
- bpo-43239: The `PyCFunction_New()` function is now exported in the
ABI when compiled with`-fvisibility=hidden` .
- bpo-40170: `PyIter_Check()` is now always declared as a function, in
order to hide implementation details. The macro accessed`PyTypeObject.tp_iternext` directly. Patch by Erlend E. Aasland.
- bpo-40170: Convert `PyDescr_IsData()` macro to a function to hide
implementation details: The macro accessed`PyTypeObject.tp_descr_set` directly. Patch by Erlend E.
Aasland.
- bpo-43181: Convert `PyObject_TypeCheck()` macro to a static inline
function. Patch by Erlend E. Aasland.

## Python 3.10.0 alpha 5Â¶

*Release date: 2021-02-02*

### SecurityÂ¶

- bpo-42938: Avoid static buffers when computing the repr of `ctypes.c_double` and`ctypes.c_longdouble` values.

### Core and BuiltinsÂ¶

- bpo-42990: Refactor the `PyEval_` family of functions.
  - An new function `_PyEval_Vector` is added to simplify calls to Python from C.
  - `_PyEval_EvalCodeWithName` is removed
  - `PyEval_EvalCodeEx` is retained as part of the API, but is not used internally
- bpo-38631: Replace `Py_FatalError()` calls in the compiler with
regular`SystemError` exceptions. Patch by Victor Stinner.
- bpo-42997: Improve error message for missing â:â before blocks. Patch by Pablo Galindo.
- bpo-43017: Improve error message in the parser when using un-parenthesised tuples in comprehensions. Patch by Pablo Galindo.
- bpo-42986: Fix parser crash when reporting syntax errors in f-string with newlines. Patch by Pablo Galindo.
- bpo-40176: Syntax errors for unterminated string literals now point to the start of the string instead of reporting EOF/EOL.
- bpo-42927: The inline cache for `LOAD_ATTR` now also optimizes access to
attributes defined by`__slots__` . This makes reading such attribute up
to 30% faster.
- bpo-42864: Improve error messages in the parser when parentheses are not closed. Patch by Pablo Galindo.
- bpo-42924: Fix `bytearray` repetition incorrectly copying data from the
start of the buffer, even if the data is offset within the buffer (e.g.
after reassigning a slice at the start of the`bytearray` to a shorter
byte string).
- bpo-42882: Fix the `_PyUnicode_FromId()` function
(_Py_IDENTIFIER(var) API) when`Py_Initialize()` /`Py_Finalize()` is called multiple times: preserve`_PyRuntime.unicode_ids.next_index` value.
- bpo-42827: Fix a crash when working out the error line of a `SyntaxError` in some multi-line expressions.
- bpo-42823: frame.f_lineno is correct even if frame.f_trace is set to True

### LibraryÂ¶

- bpo-37324: Remove deprecated aliases to Collections Abstract Base Classes from the `collections` module.

### Core and BuiltinsÂ¶

### LibraryÂ¶

- bpo-43108: Fixed a reference leak in the `curses` module. Patch by
Pablo Galindo
- bpo-43077: Update the bundled pip to 21.0.1 and setuptools to 52.0.0.
- bpo-41282: Deprecate `distutils` in documentation and add warning on
import.
- bpo-43014: Improve performance of `tokenize` by 20-30%.  Patch by
Anthony Sottile.
- bpo-42323: Fix `math.nextafter()` for NaN on AIX.
- bpo-42955: Add `sys.stdlib_module_names` , containing the list of the
standard library module names. Patch by Victor Stinner.
- bpo-42944: Fix `random.Random.sample` when`counts` argument is not`None` .
- bpo-42934: Use `TracebackException` âs new`compact` param in`TestResult` to reduce time and memory consumed
by traceback formatting.
- bpo-42931: Add `randbytes()` to`random.__all__` .
- bpo-38250: [Enum] Flags consisting of a single bit are now considered canonical, and will be the only flags returned from listing and iterating over a Flag class or a Flag member. Multi-bit flags are considered aliases; they will be returned from lookups and operations that result in their value. Iteration for both Flag and Flag members is in definition order.
- bpo-42877: Added the `compact` parameter to the constructor of`traceback.TracebackException` to reduce time and memory for use
cases that only need to call`TracebackException.format()` and`TracebackException.format_exception_only()` .
- bpo-42923: The `Py_FatalError()` function and the`faulthandler` module now dump the list of extension modules on a
fatal error.
- bpo-42848: Removed recursion from `TracebackException` to allow it to handle long exception chains.
- bpo-42901: [Enum] move member creation from `EnumMeta.__new__` to`_proto_member.__set_name__` , allowing members to be created and visible
in`__init_subclass__` .
- bpo-42780: Fix os.set_inheritable() for O_PATH file descriptors on Linux.
- bpo-42866: Fix a reference leak in the `getcodec()` function of CJK
codecs. Patch by Victor Stinner.
- bpo-42846: Convert the 6 CJK codec extension modules (_codecs_cn, _codecs_hk, _codecs_iso2022, _codecs_jp, _codecs_kr and _codecs_tw) to the multiphase initialization API ( **PEP 489** ). Patch by Victor Stinner.
- bpo-42851: remove __init_subclass__ support for Enum members
- bpo-42834: Make internal caches of the `_json` module compatible with
subinterpreters.
- bpo-41748: Fix HTMLParser parsing rules for element attributes containing commas with spaces. Patch by Karl Dubost.
- bpo-40810: Require SQLite 3.7.15 or newer. Patch by Erlend E. Aasland.
- bpo-1635741: Convert the _multibytecodec extension module (CJK codecs) to multi-phase initialization ( **PEP 489** ).  Patch by Erlend E. Aasland.
- bpo-42802: The distutils `bdist_wininst` command deprecated in Python
3.8 has been removed. The distutils`bdist_wheel` command is now
recommended to distribute binary packages on Windows.
- bpo-24464: The undocumented built-in function `sqlite3.enable_shared_cache` is now deprecated, scheduled for removal
in Python 3.12.  Its use is strongly discouraged by the SQLite3
documentation.  Patch by Erlend E. Aasland.
- bpo-42384: Make pdb populate sys.path[0] exactly the same as regular python execution.
- bpo-42383: Fix pdb: previously pdb would fail to restart the debugging target if it was specified using a relative path and the current directory changed.
- bpo-42005: Fix CLI of `cProfile` and`profile` to catch`BrokenPipeError` .
- bpo-41604: Donât decrement the reference count of the previous user_ptr when set_panel_userptr fails.
- bpo-41149: Allow executing callables that have a boolean value of `False` when passed to`Threading.thread` as the target. Patch
contributed by Barney Stratford.
- bpo-38307: Add an âend_linenoâ attribute to the Class and Function objects that appear in the tree returned by pyclbr functions. This and the existing âlinenoâ attribute define the extent of class and def statements. Patch by Aviral Srivastava.
- bpo-39273: The `BUTTON5_*` constants are now exposed in the`curses` module if available.
- bpo-33289: Correct call to `tkinter.colorchooser` to return RGB
triplet of ints instead of floats.  Patch by Cheryl Sabella.

### DocumentationÂ¶

### TestsÂ¶

### BuildÂ¶

- bpo-43031: Pass `--timeout=$(TESTTIMEOUT)` option to the default profile
task`./python -m test --pgo` command.
- bpo-36143: `make regen-all` now also runs`regen-keyword` . Patch by
Victor Stinner.
- bpo-42874: Removed the grep -q and -E flags in the tzpath validation section of the configure script to better accommodate users of some platforms (specifically Solaris 10).
- bpo-31904: Add library search path by wr-cc in add_cross_compiling_paths() for VxWorks.
- bpo-42856: Add `--with-wheel-pkg-dir=PATH` option to the`./configure` script. If specified, the`ensurepip` module looks for`setuptools` and`pip` wheel packages in this directory: if both are present, these
wheel packages are used instead of ensurepip bundled wheel packages.Some Linux distribution packaging policies recommend against bundling dependencies. For example, Fedora installs wheel packages in the `/usr/share/python-wheels/` directory and donât install the`ensurepip._bundled` package.

### WindowsÂ¶

### macOSÂ¶

- bpo-42504: Ensure that the value of sysconfig.get_config_var(âMACOSX_DEPLOYMENT_TARGETâ) is always a string, even in when the value is parsable as an integer.

### IDLEÂ¶

- bpo-43008: Make IDLE invoke `sys.excepthook()` in normal, 2-process
mode. Patch by Ken Hilton.
- bpo-33065: Fix problem debugging user classes with __repr__ method.
- bpo-23544: Disable Debug=>Stack Viewer when user code is running or Debugger is active, to prevent hang or crash. Patch by Zackery Spytz.
- bpo-32631: Finish zzdummy example extension module: make menu entries work; add docstrings and tests with 100% coverage.

### C APIÂ¶

- bpo-42979: When Python is built in debug mode (with C assertions), calling a type slot like `sq_length` (`__len__()` in Python) now fails with a
fatal error if the slot succeeded with an exception set, or failed with no
exception set. The error message contains the slot, the type name, and the
current exception (if an exception is set). Patch by Victor Stinner.
- bpo-43030: Fixed a compiler warning in `Py_UNICODE_ISSPACE()` on
platforms with signed`wchar_t` .

## Python 3.10.0 alpha 4Â¶

*Release date: 2021-01-04*

### Core and BuiltinsÂ¶

- bpo-42814: Fix undefined behavior in `Objects/genericaliasobject.c` .
- bpo-42806: Fix the column offsets for f-strings `ast` nodes
surrounded by parentheses and for nodes that spawn multiple lines. Patch
by Pablo Galindo.
- bpo-40631: Fix regression where a single parenthesized starred expression was a valid assignment target.
- bpo-27794: Improve the error message for failed writes/deletes to property objects. When possible, the attribute name is now shown. Patch provided by Yurii Karabas.
- bpo-42745: Make the type attribute lookup cache per-interpreter. Patch by Victor Stinner.
- bpo-42246: Jumps to jumps are not eliminated when it would break PEP 626.
- bpo-42246: Make sure that the `f_lasti` and`f_lineno` attributes of a
frame are set correctly when an exception is raised or re-raised. Required
for PEP 626.
- bpo-32381: The coding cookie (ex: `# coding: latin1` ) is now ignored in
the command passed to the`-c` command line option. Patch by
Victor Stinner.
- bpo-30858: Improve error location in expressions that contain assignments. Patch by Pablo Galindo and Lysandros Nikolaou.
- bpo-42615: Remove jump commands made redundant by the deletion of unreachable bytecode blocks

### LibraryÂ¶

### Core and BuiltinsÂ¶

- bpo-32381: Fix encoding name when running a `.pyc` file on Windows:`PyRun_SimpleFileExFlags()` now uses the correct encoding to
decode the filename.

### LibraryÂ¶

- bpo-42195: The `__args__` of the parameterized generics for`typing.Callable` and`collections.abc.Callable` are now
consistent.  The`__args__` for`collections.abc.Callable` are
now flattened while`typing.Callable` âs have not changed.  To allow
this change,`types.GenericAlias` can now be subclassed and`collections.abc.Callable` âs`__class_getitem__` will now return a
subclass of`types.GenericAlias` .  Tests for typing were also updated to
not subclass things like`Callable[..., T]` as that is not a valid base
class.  Finally, both`Callable` s no longer validate their`argtypes` , in`Callable[[argtypes], resulttype]` to prepare for**PEP 612** .  Patch by Ken Jin.
- bpo-40137: Convert functools module to use `PyType_FromModuleAndSpec()` .
- bpo-40077: Convert `array` to use heap types, and establish module
state for these.
- bpo-42008: Fix _random.Random() seeding.
- bpo-1635741: Port the `pyexpat` extension module to multi-phase
initialization (**PEP 489** ).

### Core and BuiltinsÂ¶

- bpo-40521: Make the Unicode dictionary of interned strings compatible with subinterpreters. Patch by Victor Stinner.
- bpo-39465: Make `_PyUnicode_FromId()` function compatible with
subinterpreters. Each interpreter now has an array of identifier objects
(interned strings decoded from UTF-8). Patch by Victor Stinner.

### LibraryÂ¶

- bpo-42257: Handle empty string in variable executable in platform.libc_ver()
- bpo-42772: randrange() now raises a TypeError when step is specified without a stop argument. Formerly, it silently ignored the step argument.
- bpo-42759: Fixed equality comparison of `tkinter.Variable` and`tkinter.font.Font` . Objects which belong to different Tcl
interpreters are now always different, even if they have the same name.
- bpo-42756: Configure LMTP Unix-domain socket to use socket global default timeout when a timeout is not explicitly provided.
- bpo-23328: Allow / character in username, password fields on _PROXY envars.
- bpo-42740: `typing.get_args()` and`typing.get_origin()` now
support**PEP 604** union types and**PEP 612** additions to`Callable` .
- bpo-42655: `subprocess`*extra_groups* is now correctly passed into
setgroups() system call.
- bpo-42727: `EnumMeta.__prepare__` now accepts`**kwds` to properly
support`__init_subclass__`
- bpo-38308: Add optional *weights* to*statistics.harmonic_mean()* .
- bpo-42721: When simple query dialogs ( `tkinter.simpledialog` ),
message boxes (`tkinter.messagebox` ) or color choose dialog
(`tkinter.colorchooser` ) are created without arguments*master* and*parent* , and the default root window is not yet created, and`NoDefaultRoot()` was not called, a new temporal hidden root
window will be created automatically. It will not be set as the default
root window and will be destroyed right after closing the dialog window.
It will help to use these simple dialog windows in programs which do not
need other GUI.
- bpo-25246: Optimized `collections.deque.remove()` .
- bpo-35728: Added a root parameter to `tkinter.font.nametofont()` .
- bpo-15303: `tkinter` supports now widgets with boolean value False.
- bpo-42681: Fixed range checks for color and pair numbers in `curses` .
- bpo-42685: Improved placing of simple query windows in Tkinter (such as `tkinter.simpledialog.askinteger()` ). They are now centered at the
center of the parent window if it is specified and shown, otherwise at the
center of the screen.
- bpo-9694: Argparse help no longer uses the confusing phrase, âoptional argumentsâ. It uses âoptionsâ instead.
- bpo-1635741: Port the `_thread` extension module to the multiphase
initialization API (**PEP 489** ) and convert its static types to heap
types.
- bpo-37961: Fix crash in `tracemalloc.Traceback.__repr__()` (regressed
in Python 3.9).
- bpo-42630: `tkinter` functions and constructors which need a default
root window raise now`RuntimeError` with descriptive message instead
of obscure`AttributeError` or`NameError` if it is not created
yet or cannot be created automatically.
- bpo-42639: `atexit._run_exitfuncs()` now logs callback exceptions
using`sys.unraisablehook` , rather than logging them directly into`sys.stderr` and raise the last exception.
- bpo-42644: `logging.disable` will now validate the types and value of
its parameter. It also now accepts strings representing the levels (as
does`logging.setLevel` ) instead of only the numerical values.
- bpo-42639: At Python exit, if a callback registered with `atexit.register()` fails, its exception is now logged. Previously,
only some exceptions were logged, and the last exception was always
silently ignored.
- bpo-36541: Fixed lib2to3.pgen2 to be able to parse PEP-570 positional only argument syntax.
- bpo-42382: In `importlib.metadata` : -`EntryPoint` objects now expose
a`.dist` object referencing the`Distribution` when constructed from
a`Distribution` . - Add support for package discovery under package
normalization rules. - The object returned by`metadata()` now has a
formally defined protocol called`PackageMetadata` with declared support
for the`.get_all()` method. - Synced with importlib_metadata 3.3.
- bpo-41877: A check is added against misspellings of autospect, auto_spec and set_spec being passed as arguments to patch, patch.object and create_autospec.
- bpo-39717: [tarfile] update nested exception raising to use `from None` or`from e`
- bpo-41877: AttributeError for suspected misspellings of assertions on mocks are now pointing out that the cause are misspelled assertions and also what to do if the misspelling is actually an intended attribute name. The unittest.mock document is also updated to reflect the current set of recognised misspellings.
- bpo-41559: Implemented **PEP 612** : added`ParamSpec` and`Concatenate` to`typing` .  Patch by Ken Jin.
- bpo-42385: StrEnum: fix _generate_next_value_ to return a str
- bpo-31904: Define THREAD_STACK_SIZE for VxWorks.
- bpo-34750: [Enum] `_EnumDict.update()` is now supported.
- bpo-42517: Enum: private names do not become members / do not generate errors â they remain normal attributes
- bpo-42678: `Enum` : call`__init_subclass__` after members have been
added
- bpo-28964: `ast.literal_eval()` adds line number information (if
available) in error message for malformed nodes.
- bpo-42470: `random.sample()` no longer warns on a sequence which is
also a set.
- bpo-31904: `posixpath.expanduser()` returns the input*path* unchanged
if user home directory is None on VxWorks.
- bpo-42388: Fix subprocess.check_output(â¦, input=None) behavior when text=True to be consistent with that of the documentation and universal_newlines=True.
- bpo-34463: Fixed discrepancy between `traceback` and the interpreter
in formatting of SyntaxError with lineno not set (`traceback` was
changed to match interpreter).
- bpo-42393: Raise `OverflowError` instead of silent truncation in`socket.ntohs()` and`socket.htons()` .  Silent truncation was
deprecated in Python 3.7. Patch by Erlend E. Aasland
- bpo-42222: Harmonized `random.randrange()` argument handling to match`range()` .
  - The integer test and conversion in `randrange()` now uses`operator.index()` .
  - Non-integer arguments to `randrange()` are deprecated.
  - The `ValueError` is deprecated in favor of a`TypeError` .
  - It now runs a little faster than before.
 (Contributed by Raymond Hettinger and Serhiy Storchaka.)
- bpo-42163: Restore compatibility for `os.uname_result` around
deepcopy and _replace.
- bpo-42090: `zipfile.Path.joinpath` now accepts arbitrary arguments, same
as`pathlib.Path.joinpath` .
- bpo-1635741: Port the _csv module to the multi-phase initialization API ( **PEP 489** ).
- bpo-42059: `typing.TypedDict` types created using the alternative
call-style syntax now correctly respect the`total` keyword argument
when setting their`__required_keys__` and`__optional_keys__` class
attributes.
- bpo-41960: Add `globalns` and`localns` parameters to the`inspect.signature()` and`inspect.Signature.from_callable()` .
- bpo-41907: fix `format()` behavior for`IntFlag`
- bpo-41891: Ensure asyncio.wait_for waits for task completion
- bpo-24792: Fixed bug where `zipimporter` sometimes reports an
incorrect cause of import errors.
- bpo-31904: Fix site and sysconfig modules for VxWorks RTOS which has no home directories.
- bpo-41462: Add `os.set_blocking()` support for VxWorks RTOS.
- bpo-40219: Lowered `tkinter.ttk.LabeledScale` dummy widget to
prevent hiding part of the content label.
- bpo-37193: Fixed memory leak in `socketserver.ThreadingMixIn` introduced
in Python 3.7.
- bpo-39068: Fix initialization race condition in `a85encode()` and`b85encode()` in`base64` . Patch by Brandon Stansbury.

### DocumentationÂ¶

- bpo-17140: Add documentation for the `multiprocessing.pool.ThreadPool` class.
- bpo-34398: Prominently feature listings from the glossary in documentation search results. Patch by Ammar Askar.

### TestsÂ¶

- bpo-42794: Update test_nntplib to use official group name of news.aioe.org for testing. Patch by Donghee Na.
- bpo-31904: Skip some asyncio tests on VxWorks.
- bpo-42641: Enhance `test_select.test_select()` : it now takes 500
milliseconds rather than 10 seconds. Use Python rather than a shell to
make the test more portable.
- bpo-31904: Skip some tests in _test_all_chown_common() on VxWorks.
- bpo-42199: Fix bytecode helper assertNotInBytecode.
- bpo-41443: Add more attribute checking in test_posix.py
- bpo-31904: Disable os.popen and impacted tests on VxWorks
- bpo-41439: Port test_ssl and test_uuid to VxWorks RTOS.

### BuildÂ¶

- bpo-42692: Fix __builtin_available check on older compilers. Patch by Joshua Root.
- bpo-27640: Added `--disable-test-modules` option to the`configure` script: donât build nor install test modules. Patch by Xavier de Gaye,
Thomas Petazzoni and Peixing Xin.
- bpo-42604: Now all platforms use a value for the âEXT_SUFFIXâ build variable derived from SOABI (for instance in freeBSD, âEXT_SUFFIXâ is now â.cpython-310d.soâ instead of â.soâ). Previously only Linux, Mac and VxWorks were using a value for âEXT_SUFFIXâ that included âSOABIâ.
- bpo-42598: Fix implicit function declarations in configure which could have resulted in incorrect configuration checks. Patch contributed by Joshua Root.
- bpo-31904: Enable libpython3.so for VxWorks.
- bpo-29076: Add fish shell support to macOS installer.

### macOSÂ¶

### Tools/DemosÂ¶

### C APIÂ¶

- bpo-42591: Export the `Py_FrozenMain()` function: fix a Python 3.9.0
regression. Python 3.9 uses`-fvisibility=hidden` and the function was
not exported explicitly and so not exported.
- bpo-32381: Remove the private `_Py_fopen()` function which is no
longer needed. Use`_Py_wfopen()` or`_Py_fopen_obj()` instead. Patch by Victor Stinner.
- bpo-1635741: Port `resource` extension module to module state
- bpo-42111: Update the `xxlimited` module to be a better example of how
to use the limited C API.
- bpo-40052: Fix an alignment build warning/error in function `PyVectorcall_Function()` . Patch by Andreas Schneider, Antoine Pitrou
and Petr Viktorin.

## Python 3.10.0 alpha 3Â¶

*Release date: 2020-12-07*

### SecurityÂ¶

- bpo-40791: Add `volatile` to the accumulator variable in`hmac.compare_digest` , making constant-time-defeating optimizations less
likely.

### Core and BuiltinsÂ¶

- bpo-42576: `types.GenericAlias` will now raise a`TypeError` when
attempting to initialize with a keyword argument.  Previously, this would
cause the interpreter to crash if the interpreter was compiled with debug
symbols. This does not affect interpreters compiled for release.  Patch by
Ken Jin.
- bpo-42536: Several built-in and standard library types now ensure that their internal result tuples are always tracked by the garbage collector: Previously, they could have become untracked by a prior garbage collection. Patch by Brandt Bucher.
- bpo-42500: Improve handling of exceptions near recursion limit. Converts a number of Fatal Errors in RecursionErrors.
- bpo-42246: PEP 626: After a return, the f_lineno attribute of a frame is always the last line executed.
- bpo-42435: Speed up comparison of bytes objects with non-bytes objects when option `-b` is specified. Speed up comparison of bytarray
objects with non-buffer object.

### LibraryÂ¶

- bpo-1635741: Port the `_warnings` extension module to the multi-phase
initialization API (**PEP 489** ). Patch by Victor Stinner.

### Core and BuiltinsÂ¶

- bpo-41686: On Windows, the `SIGINT` event,`_PyOS_SigintEvent()` , is
now created even if Python is configured to not install signal handlers
(if`PyConfig.install_signal_handlers` equals to 0, or`Py_InitializeEx(0)` ).
- bpo-42381: Allow assignment expressions in set literals and set comprehensions as per PEP 572. Patch by Pablo Galindo.
- bpo-42202: Change function parameters annotations internal representation to tuple of strings. Patch provided by Yurii Karabas.
- bpo-42374: Fix a regression introduced by the new parser, where an unparenthesized walrus operator was not allowed within generator expressions.
- bpo-42316: Allow an unparenthesized walrus in subscript indexes.
- bpo-42349: Make sure that the compiler front-end produces a well-formed control flow graph. Be more aggressive in the compiler back-end, as it is now safe to do so.
- bpo-42296: On Windows, fix a regression in signal handling which prevented to interrupt a program using CTRL+C. The signal handler can be run in a thread different than the Python thread, in which case the test deciding if the thread can handle signals is wrong.
- bpo-42332: `types.GenericAlias` objects can now be the targets of
weakrefs.
- bpo-42282: Optimise constant subexpressions that appear as part of named expressions (previously the AST optimiser did not descend into named expressions). Patch by Nick Coghlan.
- bpo-42266: Fixed a bug with the LOAD_ATTR opcode cache that was not respecting monkey-patching a class-level attribute to make it a descriptor. Patch by Pablo Galindo.

### LibraryÂ¶

### Core and BuiltinsÂ¶

- bpo-42246: Improved accuracy of line tracing events and f_lineno attribute of Frame objects. See PEP 626 for details.

### LibraryÂ¶

### Core and BuiltinsÂ¶

- bpo-42233: Allow `GenericAlias` objects to use union type
expressions. This allows expressions like```
list[int] |
dict[float, str]
```
where previously a`TypeError` would have been
thrown.  This also fixes union type expressions not de-duplicating`GenericAlias` objects. (Contributed by Ken Jin in bpo-42233.)
- bpo-26131: The import system triggers a `ImportWarning` when it falls
back to using`load_module()` .

### LibraryÂ¶

- bpo-5054: CGIHTTPRequestHandler.run_cgi() HTTP_ACCEPT improperly parsed. Replace the special purpose getallmatchingheaders with generic get_all method and add relevant tests. Original Patch by Martin Panter. Modified by Senthil Kumaran.
- bpo-42562: Fix issue when dis failed to parse function that has no line numbers. Patch provided by Yurii Karabas.
- bpo-17735: `inspect.findsource()` now raises`OSError` instead of`IndexError` when`co_lineno` of a code object is greater than
the file length. This can happen, for example, when a file is edited after
it was imported.  PR by Irit Katriel.
- bpo-42116: Fix handling of trailing comments by `inspect.getsource()` .
- bpo-42532: Remove unexpected call of `__bool__` when passing a`spec_arg` argument to a Mock.
- bpo-38200: Added itertools.pairwise()
- bpo-41818: Fix test_master_read() so that it succeeds on all platforms that either raise OSError or return bââ upon reading from master.
- bpo-42487: ChainMap.__iter__ no longer calls __getitem__ on underlying maps
- bpo-42482: `TracebackException` no longer holds a
reference to the exceptionâs traceback object. Consequently, instances of
TracebackException for equivalent but non-equal exceptions now compare as
equal.
- bpo-41818: Make test_openpty() avoid unexpected success due to number of rows and/or number of columns being == 0.
- bpo-42392: Remove loop parameter from `asyncio.subprocess` and`asyncio.tasks` functions. Patch provided by Yurii Karabas.
- bpo-42392: Remove loop parameter from `asyncio.open_connection` and`asyncio.start_server` functions. Patch provided by Yurii Karabas.
- bpo-28468: Add `platform.freedesktop_os_release()` function to parse
freedesktop.org`os-release` files.
- bpo-42299: Removed the `formatter` module, which was deprecated in
Python 3.4. It is somewhat obsolete, little used, and not tested. It was
originally scheduled to be removed in Python 3.6, but such removals were
delayed until after Python 2.7 EOL. Existing users should copy whatever
classes they use into their code. Patch by Donghee Na and Terry J. Reedy.
- bpo-26131: Deprecate zipimport.zipimporter.load_module() in favour of exec_module().
- bpo-41818: Updated tests for the pty library. test_basic() has been changed to test_openpty(); this additionally checks if slave termios and slave winsize are being set properly by pty.openpty(). In order to add support for FreeBSD, NetBSD, OpenBSD, and Darwin, this also adds test_master_read(), which demonstrates that pty.spawn() should not depend on an OSError to exit from its copy loop.
- bpo-42392: Remove loop parameter from `__init__` in all`asyncio.locks` and`asyncio.Queue` classes. Patch provided by Yurii
Karabas.
- bpo-15450: Make `filecmp.dircmp` respect subclassing. Now the`filecmp.dircmp.subdirs` behaves as expected when subclassing
dircmp.
- bpo-42413: The exception `socket.timeout` is now an alias of`TimeoutError` .
- bpo-31904: Support signal module on VxWorks.
- bpo-42406: We fixed an issue in `pickle.whichmodule` in which importing`multiprocessing` could change the how pickle identifies which module an
object belongs to, potentially breaking the unpickling of those objects.
- bpo-42403: Simplify the `importlib` external bootstrap code:`importlib._bootstrap_external` now uses regular imports to import
builtin modules. When it is imported, the builtin`__import__()` function is already fully working and so can be used to import builtin
modules like`sys` . Patch by Victor Stinner.
- bpo-1635741: Convert _sre module types to heap types (PEP 384). Patch by Erlend E. Aasland.
- bpo-42375: subprocess module update for DragonFlyBSD support.
- bpo-41713: Port the `_signal` extension module to the multi-phase
initialization API (**PEP 489** ). Patch by Victor Stinner and Mohamed
Koubaa.
- bpo-37205: `time.time()` ,`time.perf_counter()` and`time.monotonic()` functions can no longer fail with a Python fatal
error, instead raise a regular Python exception on failure.
- bpo-42328: Fixed `tkinter.ttk.Style.map()` . The function accepts now
the representation of the default state as empty sequence (as returned by`Style.map()` ). The structure of the result is now the same on all
platform and does not depend on the value of`wantobjects` .
- bpo-42345: Fix various issues with `typing.Literal` parameter handling
(flatten, deduplicate, use type to cache key). Patch provided by Yurii
Karabas.
- bpo-37205: `time.perf_counter()` on Windows and`time.monotonic()` on macOS are now system-wide. Previously, they used an offset computed at
startup to reduce the precision loss caused by the float type. Use`time.perf_counter_ns()` and`time.monotonic_ns()` added in Python
3.7 to avoid this precision loss.
- bpo-42318: Fixed support of non-BMP characters in `tkinter` on macOS.
- bpo-42350: Fix the `threading.Thread` class at fork: do nothing if
the thread is already stopped (ex: fork called at Python exit).
Previously, an error was logged in the child process.
- bpo-42333: Port _ssl extension module to heap types.
- bpo-42014: The `onerror` callback from`shutil.rmtree` now receives
correct function when`os.open` fails.
- bpo-42237: Fix `os.sendfile()` on illumos.
- bpo-42308: Add `threading.__excepthook__` to allow retrieving the
original value of`threading.excepthook()` in case it is set to a
broken or a different value. Patch by Mario Corchero.
- bpo-42131: Implement PEP 451/spec methods on zipimport.zipimporter: find_spec(), create_module(), and exec_module(). This also allows for the documented deprecation of find_loader(), find_module(), and load_module().
- bpo-41877: Mock objects which are not unsafe will now raise an AttributeError if an attribute with the prefix asert, aseert, or assrt is accessed, in addition to this already happening for the prefixes assert or assret.
- bpo-42264: `sqlite3.OptimizedUnicode` has been undocumented and obsolete
since Python 3.3, when it was made an alias to`str` .  It is now
deprecated, scheduled for removal in Python 3.12.
- bpo-42251: Added `threading.gettrace()` and`threading.getprofile()` to retrieve the functions set by`threading.settrace()` and`threading.setprofile()` respectively.
Patch by Mario Corchero.
- bpo-42249: Fixed writing binary Plist files larger than 4 GiB.
- bpo-42236: On Unix, the `os.device_encoding()` function now returns`'UTF-8'` rather than the device encoding if the Python UTF-8 Mode is enabled.
- bpo-41754: webbrowser: Ignore *NotADirectoryError* when calling`xdg-settings` .
- bpo-42183: Fix a stack overflow error for asyncio Task or Future repr(). The overflow occurs under some circumstances when a Task or Future recursively returns itself.
- bpo-42140: Improve asyncio.wait function to create the futures set just one time.
- bpo-42133: Update various modules in the stdlib to fall back on `__spec__.loader` when`__loader__` isnât defined on a module.
- bpo-26131: The `load_module()` methods found in`importlib` now
trigger a`DeprecationWarning` .
- bpo-39825: Windows: Change `sysconfig.get_config_var('EXT_SUFFIX')` to
the expected full`platform_tag.extension` format. Previously it was
hard-coded to`.pyd` , now it is compatible with`distutils.sysconfig` and will result in something like`.cp38-win_amd64.pyd` . This brings
windows into conformance with the other platforms.
- bpo-26389: The `traceback.format_exception()` ,`traceback.format_exception_only()` , and`traceback.print_exception()` functions can now take an exception
object as a positional-only argument.
- bpo-41889: Enum: fix regression involving inheriting a multiply inherited enum
- bpo-41861: Convert `sqlite3` to use heap types (PEP 384). Patch by
Erlend E. Aasland.
- bpo-40624: Added support for the XPath `!=` operator in xml.etree
- bpo-28850: Fix `pprint.PrettyPrinter.format()` overrides being ignored
for contents of small containers. The`pprint._safe_repr()` function
was removed.
- bpo-41625: Expose the `splice()` as`os.splice()` in the`os` module. Patch by Pablo Galindo
- bpo-34215: Clarify the error message for `asyncio.IncompleteReadError` when`expected` is`None` .
- bpo-41543: Add async context manager support for contextlib.nullcontext.
- bpo-21041: `pathlib.PurePath.parents` now supports negative
indexing. Patch contributed by Yaroslav Pankovych.
- bpo-41332: Added missing connect_accepted_socket() method to `asyncio.AbstractEventLoop` .
- bpo-12800: Extracting a symlink from a tarball should succeed and overwrite the symlink if it already exists. The fix is to remove the existing file or symlink before extraction. Based on patch by Chris AtLee, Jeffrey Kintscher, and Senthil Kumaran.
- bpo-40968: `urllib.request` and`http.client` now send`http/1.1` ALPN extension during TLS handshake when no custom context is
supplied.
- bpo-41001: Add `os.eventfd()` to provide a low level interface for
Linuxâs event notification file descriptor.
- bpo-40816: Add AsyncContextDecorator to contextlib to support async context manager as a decorator.
- bpo-40550: Fix time-of-check/time-of-action issue in subprocess.Popen.send_signal.
- bpo-39411: Add an `is_async` identifier to`pyclbr` âs`Function` objects. Patch by Batuhan Taskaya
- bpo-35498: Add slice support to `pathlib.PurePath.parents` .

### DocumentationÂ¶

- bpo-42238: Tentative to deprecate `make suspicious` by first removing it
from the CI and documentation builds, but keeping it around for manual
uses.
- bpo-42153: Fix the URL for the IMAP protocol documents.
- bpo-41028: Language and version switchers, previously maintained in every cpython branches, are now handled by docsbuild-script.

### TestsÂ¶

- bpo-41473: Re-enable test_gdb on gdb 9.2 and newer: https://bugzilla.redhat.com/show_bug.cgi?id=1866884 bug is fixed in gdb 10.1.
- bpo-42553: Fix `test_asyncio.test_call_later()` race condition: donât
measure asyncio performance in the`call_later()` unit test. The test
failed randomly on the CI.
- bpo-31904: Fix test_netrc on VxWorks: create temporary directories using temp_cwd().
- bpo-31904: skip test_getaddrinfo_ipv6_scopeid_symbolic and test_getnameinfo_ipv6_scopeid_symbolic on VxWorks
- bpo-31904: skip test_test of test_mailcap on VxWorks
- bpo-31904: add shell requirement for test_pipes
- bpo-31904: skip some tests related to fifo on VxWorks
- bpo-31904: Fix test_doctest.py failures for VxWorks.
- bpo-40754: Include `_testinternalcapi` module in Windows installer for
test suite
- bpo-41561: test_ssl: skip test_min_max_version_mismatch when TLS 1.0 is not available
- bpo-31904: Fix os module failures for VxWorks RTOS.
- bpo-31904: Fix fifo test cases for VxWorks RTOS.

### BuildÂ¶

- bpo-31904: remove libnet dependency from detect_socket() for VxWorks
- bpo-42398: Fix a race condition in âmake regen-allâ when make -jN option is used to run jobs in parallel. The clinic.py script now only use atomic write to write files. Moveover, generated files are now left unchanged if the content does not change, to not change the file modification time.
- bpo-41617: Fix building `pycore_bitutils.h` internal header on old clang
version without`__builtin_bswap16()` (ex: Xcode 4.6.3 on Mac OS X
10.7). Patch by Joshua Root and Victor Stinner.
- bpo-38823: It is no longer possible to build the `_ctypes` extension
module without`wchar_t` type: remove`CTYPES_UNICODE` macro.
Anyway, the`wchar_t` type is required to build Python. Patch by
Victor Stinner.
- bpo-42087: Support was removed for AIX 5.3 and below. See bpo-40680.
- bpo-40998: Addressed three compiler warnings found by undefined behavior sanitizer (ubsan).

### WindowsÂ¶

### macOSÂ¶

- bpo-42504: Fix build on macOS Big Sur when MACOSX_DEPLOYMENT_TARGET=11
- bpo-41116: Ensure distutils.unixxcompiler.find_library_file can find system provided libraries on macOS 11.
- bpo-41100: Add support for macOS 11 and Apple Silicon systems. It is now possible to build âUniversal 2â binaries using ââenable-universalsdk âwith-universal-archs=universal2â. Binaries build on later macOS versions can be deployed back to older versions (tested up to macOS 10.9), when using the correct deployment target. This is tested using Xcode 11 and later.
- bpo-42232: Added Darwin specific madvise options to mmap module.
- bpo-38443: The `--enable-universalsdk` and`--with-universal-archs` options for the configure script now check that the specified
architectures can be used.

### IDLEÂ¶

- bpo-42508: Keep IDLE running on macOS. Remove obsolete workaround that prevented running files with shortcuts when using new universal2 installers built on macOS 11.
- bpo-42426: Fix reporting offset of the RE error in searchengine.
- bpo-42415: Get docstrings for IDLE calltips more often by using inspect.getdoc.

### Tools/DemosÂ¶

### C APIÂ¶

- bpo-42423: The `PyType_FromSpecWithBases()` and`PyType_FromModuleAndSpec()` functions now accept a single class as
the*bases* argument.
- bpo-1635741: Port `select` extension module to multiphase
initialization (**PEP 489** ).
- bpo-1635741: Port _posixsubprocess extension module to multiphase initialization ( **PEP 489** ).
- bpo-1635741: Port _posixshmem extension module to multiphase initialization ( **PEP 489** )
- bpo-1635741: Port _struct extension module to multiphase initialization ( **PEP 489** )
- bpo-1635741: Port `spwd` extension module to multiphase
initialization (**PEP 489** )
- bpo-1635741: Port `gc` extension module to multiphase initialization
(**PEP 489** )
- bpo-1635741: Port _queue extension module to multiphase initialization ( **PEP 489** )
- bpo-39573: Convert `Py_TYPE()` and`Py_SIZE()` back to macros
to allow using them as an l-value. Many third party C extension modules
rely on the ability of using Py_TYPE() and Py_SIZE() to set an object type
and size:`Py_TYPE(obj) = type;` and`Py_SIZE(obj) = size;` .
- bpo-1635741: Port `symtable` extension module to multiphase
initialization (**PEP 489** )
- bpo-1635741: Port `grp` and`pwd` extension modules to
multiphase initialization (**PEP 489** )
- bpo-1635741: Port _random extension module to multiphase initialization ( **PEP 489** )
- bpo-1635741: Port _hashlib extension module to multiphase initialization ( **PEP 489** )
- bpo-41713: Removed the undocumented `PyOS_InitInterrupts()` function.
Initializing Python already implicitly installs signal handlers: see`PyConfig.install_signal_handlers` . Patch by Victor Stinner.
- bpo-40170: The `Py_TRASHCAN_BEGIN` macro no longer accesses PyTypeObject
attributes, but now can get the condition by calling the new private`_PyTrash_cond()` function which hides implementation details.
- bpo-42260: `Py_GetPath()` ,`Py_GetPrefix()` ,`Py_GetExecPrefix()` ,`Py_GetProgramFullPath()` ,`Py_GetPythonHome()` and`Py_GetProgramName()` functions now
return`NULL` if called before`Py_Initialize()` (before Python is
initialized). Use the new Python Initialization Configuration API to get the Python Path Configuration.. Patch by Victor Stinner.
- bpo-42260: The `PyConfig_Read()` function now only parses`PyConfig.argv` arguments once:`PyConfig.parse_argv` is set to`2` after arguments are parsed. Since Python arguments are
strippped from`PyConfig.argv` , parsing arguments twice would
parse the application options as Python options.
- bpo-42262: Added `Py_NewRef()` and`Py_XNewRef()` functions to
increment the reference count of an object and return the object. Patch by
Victor Stinner.
- bpo-42260: When `Py_Initialize()` is called twice, the second call
now updates more`sys` attributes for the configuration, rather than
only`sys.argv` . Patch by Victor Stinner.
- bpo-41832: The `PyType_FromModuleAndSpec()` function now accepts
NULL`tp_doc` slot.
- bpo-1635741: Added `PyModule_AddObjectRef()` function: similar to`PyModule_AddObject()` but donât steal a reference to the value on
success. Patch by Victor Stinner.
- bpo-42171: The `METH_FASTCALL` calling convention is added to the
limited API. The functions`PyModule_AddType()` ,`PyType_FromModuleAndSpec()` ,`PyType_GetModule()` and`PyType_GetModuleState()` are added to the limited API on Windows.
- bpo-42085: Add dedicated entry to PyAsyncMethods for sending values
- bpo-41073: `PyType_GetSlot()` can now accept static types.
- bpo-30459: `PyList_SET_ITEM()` ,`PyTuple_SET_ITEM()` and`PyCell_SET()` macros can no longer be used as l-value or r-value.
For example,`x = PyList_SET_ITEM(a, b, c)` and```
PyList_SET_ITEM(a, b,
c) = x
```
now fail with a compiler error. It prevents bugs like```
if
(PyList_SET_ITEM (a, b, c) < 0) ...
```
test. Patch by Zackery Spytz and
Victor Stinner.

## Python 3.10.0 alpha 2Â¶

*Release date: 2020-11-03*

### SecurityÂ¶

- bpo-42103: Prevented potential DoS attack via CPU and RAM exhaustion when processing malformed Apple Property List files in binary format.
- bpo-42051: The `plistlib` module no longer accepts entity
declarations in XML plist files to avoid XML vulnerabilities. This should
not affect users as entity declarations are not used in regular plist
files.

### Core and BuiltinsÂ¶

- bpo-42236: If the `nl_langinfo(CODESET)` function returns an empty
string, Python now uses UTF-8 as the filesystem encoding. Patch by Victor
Stinner.
- bpo-42218: Fixed a bug in the PEG parser that was causing crashes in debug mode. Now errors are checked in left-recursive rules to avoid cases where such errors do not get handled in time and appear as long-distance crashes in other places.
- bpo-42214: Fixed a possible crash in the PEG parser when checking for the â!=â token in the `barry_as_flufl` rule. Patch by Pablo Galindo.
- bpo-42206: Propagate and raise the errors caused by `PyAST_Validate()` in the parser.
- bpo-41796: The `ast` module internal state is now per interpreter.
Patch by Victor Stinner.
- bpo-42143: Fix handling of errors during creation of `PyFunctionObject` ,
which resulted in operations on uninitialized memory. Patch by Yonatan
Goldschmidt.
- bpo-41659: Fix a bug in the parser, where a curly brace following a `primary` didnât fail immediately. This led to invalid expressions like`a {b}` to throw a`SyntaxError` with a wrong offset, or invalid
expressions ending with a curly brace like`a {` to not fail immediately
in the REPL.
- bpo-42150: Fix possible buffer overflow in the new parser when checking for continuation lines. Patch by Pablo Galindo.
- bpo-42123: Run the parser two times. On the first run, disable all the rules that only generate better error messages to gain performance. If thereâs a parse failure, run the parser a second time with those enabled.
- bpo-42093: The `LOAD_ATTR` instruction now uses new âper opcode cacheâ
mechanism and it is about 36% faster now. Patch by Pablo Galindo and Yury
Selivanov.
- bpo-42030: Support for the legacy AIX-specific shared library loading support has been removed. All versions of AIX since 4.3 have supported and defaulted to using the common Unix mechanism instead.
- bpo-41984: The garbage collector now tracks all user-defined classes. Patch by Brandt Bucher.
- bpo-41993: Fixed potential issues with removing not completely initialized module from `sys.modules` when import fails.
- bpo-41979: Star-unpacking is now allowed for with itemâs targets in the PEG parser.
- bpo-41974: Removed special methods `__int__` ,`__float__` ,`__floordiv__` ,`__mod__` ,`__divmod__` ,`__rfloordiv__` ,`__rmod__` and`__rdivmod__` of the`complex` class.  They
always raised a`TypeError` .
- bpo-41902: Micro optimization when compute `sq_item` and`mp_subscript` of`range` . Patch by
Donghee Na.
- bpo-41894: When loading a native module and a load failure occurs, prevent a possible UnicodeDecodeError when not running in a UTF-8 locale by decoding the load error message using the current localeâs encoding.
- bpo-41902: Micro optimization for range.index if step is 1. Patch by Donghee Na.

### LibraryÂ¶

- bpo-41435: Add `sys._current_exceptions()` function to retrieve a
dictionary mapping each threadâs identifier to the topmost exception
currently active in that thread at the time the function is called.

### Core and BuiltinsÂ¶

- bpo-38605: Enable `from __future__ import annotations` (**PEP 563** ) by
default. The values found in`__annotations__` dicts are now
strings, for example`{"x": "int"}` instead of`{"x": int}` .

### LibraryÂ¶

- bpo-35455: On Solaris, `thread_time()` is now implemented with`gethrvtime()` because`clock_gettime(CLOCK_THREAD_CPUTIME_ID)` is not
always available. Patch by Jakub Kulik.
- bpo-42233: The `repr()` of`typing` types containing
Generic Alias Types previously did not show
the parameterized types in the`GenericAlias` .  They have now been
changed to do so.
- bpo-29566: `binhex.binhex()` consistently writes macOS 9 line endings.
- bpo-26789: The `logging.FileHandler` class now keeps a reference to
the builtin`open()` function to be able to open or reopen the file
during Python finalization. Fix errors like:```
NameError: name 'open' is
not defined
```
. Patch by Victor Stinner.
- bpo-42157: Removed the `unicodedata.ucnhash_CAPI` attribute which was an
internal PyCapsule object. The related private`_PyUnicode_Name_CAPI` structure was moved to the internal C API. Patch by Victor Stinner.
- bpo-42157: Convert the `unicodedata` extension module to the
multiphase initialization API (**PEP 489** ) and convert the`unicodedata.UCD` static type to a heap type. Patch by Mohamed Koubaa
and Victor Stinner.
- bpo-42146: Fix memory leak in `subprocess.Popen()` in case an uid
(gid) specified in`user` (`group` ,`extra_groups` ) overflows`uid_t` (`gid_t` ).
- bpo-42103: `InvalidFileException` and`RecursionError` are now the only errors caused by loading malformed binary Plist file
(previously ValueError and TypeError could be raised in some specific
cases).
- bpo-41490: In `importlib.resources` ,`.path` method is more aggressive
about releasing handles to zipfile objects early, enabling use-cases like
certifi to leave the context open but delete the underlying zip file.
- bpo-41052: Pickling heap types implemented in C with protocols 0 and 1 raises now an error instead of producing incorrect data.
- bpo-42089: In `importlib.metadata.PackageNotFoundError` , make reference
to the package metadata being missing to improve the user experience.
- bpo-41491: plistlib: fix parsing XML plists with hexadecimal integer values
- bpo-42065: Fix an incorrectly formatted error from `_codecs.charmap_decode()` when called with a mapped value outside
the range of valid Unicode code points. PR by Max Bernstein.
- bpo-41966: Fix pickling pure Python `datetime.time` subclasses.
Patch by Dean Inwood.
- bpo-19270: `sched.scheduler.cancel()` will now cancel the correct
event, if two events with same priority are scheduled for the same time.
Patch by Bar Harel.
- bpo-28660: `textwrap.wrap()` now attempts to break long words after
hyphens when`break_long_words=True` and`break_on_hyphens=True` .
- bpo-35823: Use `vfork()` instead of`fork()` for`subprocess.Popen()` on Linux to improve performance in cases where it
is deemed safe.
- bpo-42043: Add support for `zipfile.Path` inheritance.`zipfile.Path.is_file()` now returns False for non-existent names.`zipfile.Path` objects now expose a`.filename` attribute and rely on
that to resolve`.name` and`.parent` when the`Path` object is at
the root of the zipfile.
- bpo-42021: Fix possible ref leaks in `sqlite3` module init.
- bpo-39101: Fixed tests using IsolatedAsyncioTestCase from hanging on BaseExceptions.
- bpo-41976: Fixed a bug that was causing `ctypes.util.find_library()` to return`None` when triying to locate a library in an environment when
gcc>=9 is available and`ldconfig` is not. Patch by Pablo Galindo
- bpo-41943: Fix bug where TestCase.assertLogs doesnât correctly filter messages by level.
- bpo-41923: Implement **PEP 613** , introducing`typing.TypeAlias` annotation.
- bpo-41905: A new function in abc: *update_abstractmethods* to re-calculate
an abstract classâs abstract status. In addition,*dataclass* has been
changed to call this function.
- bpo-23706: Added *newline* parameter to`pathlib.Path.write_text()` .
- bpo-41876: Tkinter font class repr uses font name
- bpo-41831: `str()` for the`type` attribute of the`tkinter.Event` object always returns now the numeric code returned by Tk instead of the
name of the event type.
- bpo-39337: `encodings.normalize_encoding()` now ignores non-ASCII
characters.
- bpo-41747: Ensure all methods that generated from `dataclasses.dataclass()` objects now have the proper`__qualname__` attribute referring to the class they belong to. Patch by Batuhan Taskaya.
- bpo-30681: Handle exceptions caused by unparsable date headers when using email âdefaultâ policy. Patch by Tim Bell, Georges Toth
- bpo-41586: Add F_SETPIPE_SZ and F_GETPIPE_SZ to fcntl module. Allow setting pipesize on subprocess.Popen.
- bpo-41229: Add `contextlib.aclosing` for deterministic cleanup of async
generators which is analogous to`contextlib.closing` for non-async
generators. Patch by Joongi Kim and John Belmonte.
- bpo-16396: Allow `ctypes.wintypes` to be imported on non-Windows
systems.
- bpo-4356: Add a key function to the bisect module.
- bpo-40592: `shutil.which()` now ignores empty entries in`PATHEXT` instead of treating them as a match.
- bpo-40492: Fix `--outfile` for`cProfile` /`profile` not
writing the output file in the original directory when the program being
profiled changes the working directory.  PR by Anthony Sottile.
- bpo-34204: The `shelve` module now uses`pickle.DEFAULT_PROTOCOL` by default instead of`pickle` protocol`3` .
- bpo-27321: Fixed KeyError exception when flattening an email to a string attempts to replace a non-existent Content-Transfer-Encoding header.
- bpo-38976: The `http.cookiejar` module now supports the parsing of
cookies in CURL-style cookiejar files through MozillaCookieJar on all
platforms. Previously, such cookie entries would be silently ignored when
loading a cookiejar with such entries.Additionally, the HTTP Only attribute is persisted in the object, and will be correctly written to file if the MozillaCookieJar object is subsequently dumped.

### DocumentationÂ¶

- bpo-42061: Document __format__ functionality for IP addresses.
- bpo-41910: Document the default implementation of `object.__eq__` .
- bpo-42010: Clarify that subscription expressions are also valid for certain classes and types in the standard library, and for user-defined classes and types if the classmethod `__class_getitem__()` is provided.
- bpo-41805: Documented generic alias type and `types.GenericAlias` . Also added an entry in glossary for
generic types.
- bpo-39693: Fix tarfileâs extractfile documentation
- bpo-39416: Document some restrictions on the default string representations of numeric classes.

### TestsÂ¶

- bpo-41739: Fix test_logging.test_race_between_set_target_and_flush(): the test now waits until all threads complete to avoid leaking running threads.
- bpo-41970: Avoid a test failure in `test_lib2to3` if the module has
already imported at the time the test executes. Patch by Pablo Galindo.
- bpo-41944: Tests for CJK codecs no longer call `eval()` on content
received via HTTP.
- bpo-41306: Fixed a failure in `test_tk.test_widgets.ScaleTest` happening
when executing the test with Tk 8.6.10.

### BuildÂ¶

- bpo-38980: Add `-fno-semantic-interposition` to both the compile and
link line when building with`--enable-optimizations` . Patch by Victor
Stinner and Pablo Galindo.

### WindowsÂ¶

- bpo-38439: Updates the icons for IDLE in the Windows Store package.
- bpo-38252: Use 8-byte step to detect ASCII sequence in 64-bit Windows build.
- bpo-39107: Update Tcl and Tk to 8.6.10 in Windows installer.
- bpo-41557: Update Windows installer to use SQLite 3.33.0.
- bpo-38324: Avoid Unicode errors when accessing certain locale data on Windows.

### macOSÂ¶

- bpo-41471: Ignore invalid prefix lengths in system proxy excludes.

### IDLEÂ¶

- bpo-33987: Mostly finish using ttk widgets, mainly for editor, settings, and searches. Some patches by Mark Roseman.
- bpo-40511: Typing opening and closing parentheses inside the parentheses of a function call will no longer cause unnecessary âflashingâ off and on of an existing open call-tip, e.g. when typed in a string literal.
- bpo-38439: Add a 256Ã256 pixel IDLE icon to the Windows .ico file. Created by Andrew Clover. Remove the low-color gif variations from the .ico file.

### C APIÂ¶

- bpo-42157: The private `_PyUnicode_Name_CAPI` structure of the PyCapsule
API`unicodedata.ucnhash_CAPI` has been moved to the internal C API.
Patch by Victor Stinner.
- bpo-42015: Fix potential crash in deallocating method objects when dynamically allocated `PyMethodDef` âs lifetime is managed through
the`self` argument of a`PyCFunction` .
- bpo-40423: The `subprocess` module and`os.closerange` will now use
the`close_range(low, high, flags)` syscall when it is available for
more efficient closing of ranges of descriptors.
- bpo-41845: `PyObject_GenericGetDict()` is available again in the
limited API when targeting 3.10 or later.
- bpo-40422: Add `_Py_closerange` function to provide performant closing
of a range of file descriptors.
- bpo-41986: `Py_FileSystemDefaultEncodeErrors` and`Py_UTF8Mode` are available again in limited API.
- bpo-41756: Add `PyIter_Send` function to allow sending value into
generator/coroutine/iterator without raising StopIteration exception to
signal return.
- bpo-41784: Added `PyUnicode_AsUTF8AndSize` to the limited C API.

## Python 3.10.0 alpha 1Â¶

*Release date: 2020-10-05*

### SecurityÂ¶

- bpo-41304: Fixes `python3x._pth` being ignored on Windows, caused by the
fix for bpo-29778 (**CVE 2020-15801** ).
- bpo-41162: Audit hooks are now cleared later during finalization to avoid missing events.
- bpo-29778: Ensure `python3.dll` is loaded from correct locations
when Python is embedded (**CVE 2020-15523** ).
- bpo-41004: The __hash__() methods of ipaddress.IPv4Interface and ipaddress.IPv6Interface incorrectly generated constant hash values of 32 and 128 respectively. This resulted in always causing hash collisions. The fix uses hash() to generate hash values for the tuple of (address, mask length, network address).
- bpo-39603: Prevent http header injection by rejecting control characters in http.client.putrequest(â¦).

### Core and BuiltinsÂ¶

- bpo-41909: Fixed stack overflow in `issubclass()` and`isinstance()` when getting the`__bases__` attribute leads to
infinite recursion.
- bpo-41922: Speed up calls to `reversed()` by using the**PEP 590**`vectorcall` calling convention. Patch by Donghee Na.
- bpo-41873: Calls to `float()` are now faster due to the`vectorcall` calling convention. Patch by Dennis Sweeney.
- bpo-41870: Speed up calls to `bool()` by using the**PEP 590**`vectorcall` calling convention. Patch by Donghee Na.

### LibraryÂ¶

- bpo-1635741: Port the `_bisect` module to the multi-phase
initialization API (**PEP 489** ).

### Core and BuiltinsÂ¶

- bpo-39934: Correctly count control blocks in âexceptâ in compiler. Ensures that a syntax error, rather a fatal error, occurs for deeply nested, named exception handlers.
- bpo-41780: Fix `__dir__()` of`types.GenericAlias` . Patch by
Batuhan Taskaya.

### LibraryÂ¶

- bpo-1635741: Port the `_lsprof` extension module to multi-phase
initialization (**PEP 489** ).
- bpo-1635741: Port the `cmath` extension module to multi-phase
initialization (**PEP 489** ).
- bpo-1635741: Port the `_scproxy` extension module to multi-phase
initialization (**PEP 489** ).
- bpo-1635741: Port the `termios` extension module to multi-phase
initialization (**PEP 489** ).
- bpo-1635741: Convert the `_sha256` extension module types to heap
types.

### Core and BuiltinsÂ¶

- bpo-41690: Fix a possible stack overflow in the parser when parsing functions and classes with a huge amount of arguments. Patch by Pablo Galindo.

### LibraryÂ¶

- bpo-1635741: Port the `_overlapped` extension module to multi-phase
initialization (**PEP 489** ).
- bpo-1635741: Port the `_curses_panel` extension module to
multi-phase initialization (**PEP 489** ).
- bpo-1635741: Port the `_opcode` extension module to multi-phase
initialization (**PEP 489** ).

### Core and BuiltinsÂ¶

- bpo-41681: Fixes the wrong error description in the error raised by using 2 `,` in format string in f-string and`str.format()` .

### LibraryÂ¶

- bpo-41675: The implementation of `signal.siginterrupt()` now uses`sigaction()` (if it is available in the system) instead of the
deprecated`siginterrupt()` . Patch by Pablo Galindo.

### Core and BuiltinsÂ¶

- bpo-41670: Prevent line trace being skipped on platforms not compiled with `USE_COMPUTED_GOTOS` . Fixes issue where some lines nested within a
try-except block were not being traced on Windows.
- bpo-41654: Fix a crash that occurred when destroying subclasses of `MemoryError` . Patch by Pablo Galindo.

### LibraryÂ¶

- bpo-1635741: Port the `zlib` extension module to multi-phase
initialization (**PEP 489** ).

### Core and BuiltinsÂ¶

- bpo-41631: The `_ast` module uses again a global state. Using a module
state per module instance is causing subtle practical problems. For
example, the Mercurial project replaces the`__import__()` function to
implement lazy import, whereas Python expected that`import _ast` always
return a fully initialized`_ast` module.

### LibraryÂ¶

- bpo-40077: Convert `_operator` to use`PyType_FromSpec()` .

### Core and BuiltinsÂ¶

- bpo-1653741: Port `_sha3` to multi-phase init.  Convert static types
to heap types.

### LibraryÂ¶

- bpo-1635741: Port the `_blake2` extension module to the multi-phase
initialization API (**PEP 489** ).

### Core and BuiltinsÂ¶

- bpo-41533: Free the stack allocated in `va_build_stack` if`do_mkstack` fails and the stack is not a`small_stack` .
- bpo-41531: Fix a bug that was dropping keys when compiling dict literals with more than 0xFFFF elements. Patch by Pablo Galindo.
- bpo-41525: The output of `python --help` contains now only ASCII
characters.

### LibraryÂ¶

- bpo-1635741: Port the `_sha1` ,`_sha512` , and`_md5` extension modules to multi-phase initialization API (**PEP 489** ).

### Core and BuiltinsÂ¶

- bpo-41431: Optimize `dict_merge()` for copying dict (e.g.`dict(d)` and`{}.update(d)` ).
- bpo-41428: Implement PEP 604. This supports (int | str) etc. in place of Union[str, int].
- bpo-41340: Removed fallback implementation for `strdup` .
- bpo-38156: Handle interrupts that come after EOF correctly in `PyOS_StdioReadline` .
- bpo-41342: `round()` with integer argument is now faster (9â60%).
- bpo-41334: Constructors `str()` ,`bytes()` and`bytearray()` are now faster (around 30â40% for small objects).
- bpo-41295: Resolve a regression in CPython 3.8.4 where defining â__setattr__â in a multi-inheritance setup and calling up the hierarchy chain could fail if builtins/extension types were involved in the base types.
- bpo-41323: Bytecode optimizations are performed directly on the control flow graph. This will result in slightly more compact code objects in some circumstances.
- bpo-41247: Always cache the running loop holder when running `asyncio.set_running_loop` .
- bpo-41252: Fix incorrect refcounting in _ssl.câs `_servername_callback()` .
- bpo-1635741: Port `multiprocessing` to multi-phase initialization

### LibraryÂ¶

- bpo-1635741: Port `winapi` to multiphase initialization

### Core and BuiltinsÂ¶

- bpo-41215: Use non-NULL default values in the PEG parser keyword list to overcome a bug that was preventing Python from being properly compiled when using the XLC compiler. Patch by Pablo Galindo.
- bpo-41218: Python 3.8.3 had a regression where compiling with ast.PyCF_ALLOW_TOP_LEVEL_AWAIT would aggressively mark list comprehension with CO_COROUTINE. Now only list comprehension making use of async/await will tagged as so.

### LibraryÂ¶

- bpo-1635741: Port `faulthandler` to multiphase initialization.
- bpo-1635741: Port `sha256` to multiphase initialization

### Core and BuiltinsÂ¶

- bpo-41175: Guard against a NULL pointer dereference within bytearrayobject triggered by the `bytearray() + bytearray()` operation.
- bpo-41100: add arm64 to the allowable Mac OS arches in mpdecimal.h
- bpo-41094: Fix decoding errors with audit when open files with non-ASCII names on non-UTF-8 locale.
- bpo-39960: The âhackcheckâ that prevents sneaking around a typeâs __setattr__() by calling the superclass method was rewritten to allow C implemented heap types.
- bpo-41084: Prefix the error message with âf-string: â, when parsing an f-string expression which throws a `SyntaxError` .
- bpo-40521: Empty frozensets are no longer singletons.
- bpo-41076: Pre-feed the parser with the location of the f-string expression, not the f-string itself, which allows us to skip the shifting of the AST node locations after the parsing is completed.
- bpo-41056: Fixes a reference to deallocated stack space during startup when constructing sys.path involving a relative symlink when code was supplied via -c. (discovered via Coverity)
- bpo-41061: Fix incorrect expressions and asserts in hashtable code and tests.
- bpo-41052: Opt out serialization/deserialization for _random.Random
- bpo-40939: Rename `PyPegen*` functions to`PyParser*` , so that we can
remove the old set of`PyParser*` functions that were using the old
parser, but keep everything backwards-compatible.
- bpo-35975: Stefan Behnel reported that cf_feature_version is used even when PyCF_ONLY_AST is not set. This is against the intention and against the documented behavior, so itâs been fixed.
- bpo-40939: Remove the remaining files from the old parser and the `symbol` module.

### LibraryÂ¶

- bpo-40077: Convert `_bz2` to use`PyType_FromSpec()` .

### Core and BuiltinsÂ¶

- bpo-41006: The `encodings.latin_1` module is no longer imported at
startup. Now it is only imported when it is the filesystem encoding or the
stdio encoding.
- bpo-40636: `zip()` now supports**PEP 618** âs`strict` parameter,
which raises a`ValueError` if the arguments are exhausted at
different lengths. Patch by Brandt Bucher.
- bpo-1635741: Port `_gdbm` to multiphase initialization.
- bpo-40985: Fix a bug that caused the `SyntaxError` text to be empty
when a file ends with a line ending in a line continuation character (i.e.
backslash). The error text should contain the text of the last line.
- bpo-40958: Fix a possible buffer overflow in the PEG parser when gathering information for emitting syntax errors. Patch by Pablo Galindo.
- bpo-1635741: Port `_dbm` to multiphase initialization.
- bpo-40957: Fix refleak in _Py_fopen_obj() when PySys_Audit() fails

### LibraryÂ¶

### Core and BuiltinsÂ¶

- bpo-40947: The Python Path Configuration now takes `PyConfig.platlibdir` in account.
- bpo-40939: Remove the old parser, the `parser` module and all
associated support code, command-line options and environment variables.
Patch by Pablo Galindo.
- bpo-40847: Fix a bug where a line with only a line continuation character is not considered a blank line at tokenizer level. In such cases, more than a single `NEWLINE` token was emitted. The old parser was working
around the issue, but the new parser threw a`SyntaxError` for valid
input due to this. For example, an empty line following a line
continuation character was interpreted as a`SyntaxError` .
- bpo-40890: Each dictionary view now has a `mapping` attribute that
provides a`types.MappingProxyType` wrapping the original
dictionary.  Patch contributed by Dennis Sweeney.
- bpo-40889: Improved the performance of symmetric difference operations on dictionary item views. Patch by Dennis Sweeney.
- bpo-40904: Fix possible segfault in the new PEG parser when parsing f-string containing yield statements with no value ( `f"{yield}"` ).
Patch by Pablo Galindo
- bpo-40903: Fixed a possible segfault in the new PEG parser when producing error messages for invalid assignments of the form `p=p=` . Patch by
Pablo Galindo
- bpo-40880: Fix invalid memory read in the new parser when checking newlines in string literals. Patch by Pablo Galindo.
- bpo-40883: Fix memory leak in when parsing f-strings in the new parser. Patch by Pablo Galindo
- bpo-40870: Raise `ValueError` when validating custom ASTâs where the
constants`True` ,`False` and`None` are used within a`ast.Name` node.
- bpo-40854: Allow overriding `sys.platlibdir` via a new`PYTHONPLATLIBDIR` environment variable.
- bpo-40826: Fix GIL usage in `PyOS_Readline()` : lock the GIL to set
an exception and pass the Python thread state when checking if there is a
pending signal.
- bpo-1635741: Port `fcntl` to multiphase initialization.
- bpo-19468: Delete unnecessary instance check in importlib.reload(). Patch by Furkan Ãnder.
- bpo-40824: Unexpected errors in calling the `__iter__` method are no
longer masked by`TypeError` in the`in` operator and functions`contains()` ,`indexOf()` and`countOf()` of the`operator` module.
- bpo-40792: Attributes `start` ,`stop` and`step` of the`range` object now always has exact type`int` .  Previously,
they could have been an instance of a subclass of`int` .
- bpo-40780: Fix a corner case where g-style string formatting of a float failed to remove trailing zeros.
- bpo-38964: When thereâs a `SyntaxError` in the expression part of an
fstring, the filename attribute of the`SyntaxError` gets correctly
set to the name of the file the fstring resides in.
- bpo-40750: Support the â-dâ debug flag in the new PEG parser. Patch by Pablo Galindo
- bpo-40217: Instances of types created with `PyType_FromSpecWithBases()` will no longer automatically visit
their class object when traversing references in the garbage collector.
The user is expected to manually visit the objectâs class. Patch by Pablo
Galindo.
- bpo-39573: `Py_TYPE()` is changed to the inline static function.
Patch by Donghee Na.
- bpo-40696: Fix a hang that can arise after `generator.throw()` due to
a cycle in the exception context chain.
- bpo-40521: Each interpreter now its has own free lists, singletons and caches: 
  - Free lists: float, tuple, list, dict, frame, context, asynchronous generator, MemoryError.
  - Singletons: empty tuple, empty bytes string, empty Unicode string, single byte character, single Unicode (latin1) character.
  - Slice cache.
 They are no longer shared by all interpreters.
- bpo-40679: Certain `TypeError` messages about missing or extra
arguments now include the functionâs qualified name.  Patch by
Dennis Sweeney.
- bpo-29590: Make the stack trace correct after calling `generator.throw()` on a generator that has yielded from a```
yield
from
```
.
- bpo-4022: Improve performance of generators by not raising internal StopIteration.

### LibraryÂ¶

- bpo-1635741: Port `mmap` to multiphase initialization.
- bpo-1635741: Port `_lzma` to multiphase initialization.

### Core and BuiltinsÂ¶

- bpo-37999: Builtin and extension functions that take integer arguments no longer accept `Decimal` s,`Fraction` s and other objects that can be converted to integers only with a loss
(e.g. that have the`__int__()` method but do not have the`__index__()` method).
- bpo-29882: Add `int.bit_count()` , counting the number of ones in the
binary representation of an integer. Patch by Niklas Fiekas.

### LibraryÂ¶

- bpo-36982: Use ncurses extended color functions when available to support terminals with 256 colors, and add the new function `curses.has_extended_color_support()` to indicate whether extended
color support is provided by the underlying ncurses library.

### Core and BuiltinsÂ¶

### LibraryÂ¶

- bpo-41900: C14N 2.0 serialisation in xml.etree.ElementTree failed for unprefixed attributes when a default namespace was defined.
- bpo-41887: Strip leading spaces and tabs on `ast.literal_eval()` . Also
document stripping of spaces and tabs for`eval()` .
- bpo-41773: Note in documentation that `random.choices()` doesnât
support non-finite weights, raise`ValueError` when given non-finite
weights.
- bpo-41840: Fix a bug in the `symtable` module that was causing
module-scope global variables to not be reported as both local and global.
Patch by Pablo Galindo.
- bpo-41842: Add `codecs.unregister()` function to unregister a codec
search function.
- bpo-40564: In `zipfile.Path` , mutate the passed ZipFile object type
instead of making a copy. Prevents issues when both the local copy and the
callerâs copy attempt to close the same file handle.
- bpo-40670: More reliable validation of statements in `timeit.Timer` . It now accepts âemptyâ statements (only whitespaces
and comments) and rejects misindentent statements.
- bpo-41833: The `threading.Thread` constructor now uses the target
name if the*target* argument is specified but the*name* argument is
omitted.
- bpo-41817: fix `tkinter.EventType` Enum so all members are strings, and
none are tuples
- bpo-41810: `types.EllipsisType` ,`types.NotImplementedType` and`types.NoneType` have been reintroduced, providing a new set of
types readily interpretable by static type checkers.
- bpo-41815: Fix SQLite3 segfault when backing up closed database. Patch contributed by Peter David McCormick.
- bpo-41816: StrEnum added: it ensures that all members are already strings or string candidates
- bpo-41517: fix bug allowing Enums to be extended via multiple inheritance
- bpo-39587: use the correct mix-in data type when constructing Enums
- bpo-41792: Add is_typeddict function to typing.py to check if a type is a TypedDict class Previously there was no way to check that without using private API. See the relevant issue in python/typing.
- bpo-41789: Honor `object` overrides in`Enum` class creation
(specifically,`__str__` ,`__repr__` ,`__format__` , and`__reduce_ex__` ).
- bpo-32218: `enum.Flag` and`enum.IntFlag` members are now iterable.
- bpo-39651: Fix a race condition in the `call_soon_threadsafe()` method
of`asyncio.ProactorEventLoop` : do nothing if the self-pipe socket has
been closed.
- bpo-1635741: Port the `mashal` extension module to the multi-phase
initialization API (**PEP 489** ).
- bpo-1635741: Port the `_string` extension module to the multi-phase
initialization API (**PEP 489** ).
- bpo-41732: Added an iterator to `memoryview` .
- bpo-41720: Fixed `turtle.Vec2D.__rmul__()` for arguments which are not
int or float.
- bpo-41696: Fix handling of debug mode in `asyncio.run()` . This allows
setting`PYTHONASYNCIODEBUG` or`-X dev` to enable asyncio debug mode
when using`asyncio.run()` .
- bpo-41687: Fix implementation of sendfile to be compatible with Solaris.
- bpo-41662: No longer override exceptions raised in `__len__()` of a
sequence of parameters in`sqlite3` with`ProgrammingError` .
- bpo-39010: Restarting a `ProactorEventLoop` on Windows no longer logs
spurious`ConnectionResetErrors` .
- bpo-41638: `ProgrammingError` message for absent parameter
in`sqlite3` contains now the name of the parameter instead of its
index when parameters are supplied as a dict.
- bpo-41662: Fixed crash when mutate list of parameters during iteration in `sqlite3` .
- bpo-41513: Improved the accuracy of math.hypot(). Internally, each step is computed with extra precision so that the result is now almost always correctly rounded.
- bpo-41609: The pdb whatis command correctly reports instance methods as âMethodâ rather than âFunctionâ.
- bpo-39994: Fixed pprintâs handling of dict subclasses that override __repr__.
- bpo-32751: When cancelling the task due to a timeout, `asyncio.wait_for()` will now wait until the cancellation is complete
also in the case when*timeout* is <= 0, like it does with positive
timeouts.
- bpo-37658: `asyncio.wait_for()` now properly handles races between
cancellation of itself and the completion of the wrapped awaitable.
- bpo-40782: Change the method asyncio.AbstractEventLoop.run_in_executor to not be a coroutine.
- bpo-41520: Fix `codeop` regression that prevented turning compile
warnings into errors.
- bpo-41528: turtle uses math module functions to convert degrees to radians and vice versa and to calculate vector norm
- bpo-41513: Minor algorithmic improvement to math.hypot() and math.dist() giving small gains in speed and accuracy.
- bpo-41503: Fixed a race between setTarget and flush in logging.handlers.MemoryHandler.
- bpo-41497: Fix potential UnicodeDecodeError in dis module.
- bpo-41467: On Windows, fix asyncio `recv_into()` return value when the
socket/pipe is closed (`BrokenPipeError` ): return`0` rather than
an empty byte string (`b''` ).
- bpo-41425: Make tkinter doc example runnable.
- bpo-41421: Make an algebraic simplification to random.paretovariate(). It now is slightly less subject to round-off error and is slightly faster. Inputs that used to cause ZeroDivisionError now cause an OverflowError instead.
- bpo-41440: Add `os.cpu_count()` support for VxWorks RTOS.
- bpo-41316: Fix the `tarfile` module to write only basename of TAR
file to GZIP compression header.
- bpo-41384: Raise TclError instead of TypeError when an unknown option is passed to tkinter.OptionMenu.
- bpo-41317: Use add_done_callback() in asyncio.loop.sock_accept() to unsubscribe reader early on cancellation.
- bpo-41364: Reduce import overhead of `uuid` .
- bpo-35328: Set the environment variable `VIRTUAL_ENV_PROMPT` at`venv` activation.
- bpo-41341: Recursive evaluation of `typing.ForwardRef` in`get_type_hints` .
- bpo-41344: Prevent creating `shared_memory.SharedMemory` objects
with`size=0` .
- bpo-41333: `collections.OrderedDict.pop()` is now 2 times faster.
- bpo-41288: Unpickling invalid NEWOBJ_EX opcode with the C implementation raises now UnpicklingError instead of crashing.
- bpo-39017: Avoid infinite loop when reading specially crafted TAR files using the tarfile module ( **CVE 2019-20907** ).
- bpo-41273: Speed up any transport using `_ProactorReadPipeTransport` by
calling`recv_into` instead of`recv` , thus not creating a new buffer
for each`recv` call in the transportâs read loop.
- bpo-41235: Fix the error handling in `ssl.SSLContext.load_dh_params()` .
- bpo-41207: In distutils.spawn, restore expectation that DistutilsExecError is raised when the command is not found.
- bpo-29727: Register `array.array` as a`MutableSequence` . Patch by Pablo Galindo.
- bpo-39168: Remove the `__new__` method of`typing.Generic` .
- bpo-41194: Fix a crash in the `_ast` module: it can no longer be loaded
more than once. It now uses a global state rather than a module state.
- bpo-41195: Add read-only ssl.SSLContext.security_level attribute to retrieve the contextâs security level.
- bpo-41193: The `write_history()` atexit function of the readline
completer now ignores any`OSError` to ignore error if the filesystem
is read-only, instead of only ignoring`FileNotFoundError` and`PermissionError` .
- bpo-41182: selector: use DefaultSelector based upon implementation
- bpo-41161: The decimal module now requires libmpdec-2.5.0. Users of âwith-system-libmpdec should update their system library.
- bpo-40874: The decimal module now requires libmpdec-2.5.0.
- bpo-41138: Fixed the `trace` module CLI for Python source files with
non-UTF-8 encoding.
- bpo-31082: Use the term âiterableâ in the docstring for `functools.reduce()` .
- bpo-40521: Remove freelist from collections.deque().
- bpo-31938: Fix default-value signatures of several functions in the `select` module - by Anthony Sottile.
- bpo-41068: Fixed reading files with non-ASCII names from ZIP archive directly after writing them.
- bpo-41058: `pdb.find_function()` now correctly determines the source
file encoding.
- bpo-41056: Invalid file descriptor values are now prevented from being passed to os.fpathconf. (discovered by Coverity)
- bpo-41056: Fix a NULL pointer dereference within the ssl module during a MemoryError in the keylog callback. (discovered by Coverity)
- bpo-41056: Fixed an instance where a MemoryError within the zoneinfo module might not be reported or not reported at its source. (found by Coverity)
- bpo-41048: `mimetypes.read_mime_types()` function reads the rule file
using UTF-8 encoding, not the locale encoding. Patch by Srinivas Reddy
Thatiparthy.
- bpo-41043: Fixed the use of `glob()` in the stdlib: literal part
of the path is now always correctly escaped.
- bpo-41025: Fixed an issue preventing the C implementation of `zoneinfo.ZoneInfo` from being subclassed.
- bpo-35018: Add the `xml.sax.handler.LexicalHandler` class that is
present in other SAX XML implementations.
- bpo-41002: Improve performance of HTTPResponse.read with a given amount. Patch by Bruce Merry.
- bpo-40448: `ensurepip` now disables the use of`pip` cache when
installing the bundled versions of`pip` and`setuptools` .  Patch by
Krzysztof Konopko.
- bpo-40967: Removed `asyncio.Task.current_task()` and`asyncio.Task.all_tasks()` . Patch contributed by RÃ©mi Lapeyre.
- bpo-40924: Ensure `importlib.resources.path` returns an extant path for
the SourceFileLoaderâs resource reader. Avoids the regression identified
in master while a long-term solution is devised.
- bpo-40955: Fix a minor memory leak in `subprocess` module when
extra_groups was specified.
- bpo-40855: The standard deviation and variance functions in the statistics module were ignoring their mu and xbar arguments.
- bpo-40939: Use the new PEG parser when generating the stdlib `keyword` module.
- bpo-23427: Add `sys.orig_argv` attribute: the list of the original
command line arguments passed to the Python executable.
- bpo-33689: Ignore empty or whitespace-only lines in .pth files. This matches the documentated behavior. Before, empty lines caused the site-packages dir to appear multiple times in sys.path. By Ido Michael, contributors Malcolm Smith and Tal Einat.
- bpo-40884: Added a `defaults` parameter to`logging.Formatter` ,
to allow specifying default values for custom fields. Patch by Asaf Alon
and Bar Harel.
- bpo-40876: Clarify error message in the `csv` module.
- bpo-39791: Refresh importlib.metadata from importlib_metadata 1.6.1.
- bpo-40807: Stop codeop._maybe_compile, used by code.InteractiveInterpreter (and IDLE). from emitting each warning three times.
- bpo-32604: Fix reference leak in the `select` module when the module
is imported in a subinterpreter.
- bpo-39791: Built-in loaders (SourceFileLoader and ZipImporter) now supply `TraversableResources` implementations for`ResourceReader` , and the
fallback function has been removed.
- bpo-39314: `rlcompleter.Completer` and the standard Python shell
now close the parenthesis for functions that take no arguments. Patch
contributed by RÃ©mi Lapeyre.
- bpo-17005: The topological sort functionality that was introduced initially in the `functools` module has been moved to a new`graphlib` module to better accommodate the new tools and keep the
original scope of the`functools` module. Patch by Pablo Galindo
- bpo-40834: Fix truncate when sending str object with_xxsubinterpreters.channel_send.
- bpo-40755: Add rich comparisons to collections.Counter().
- bpo-26407: Unexpected errors in calling the `__iter__` method are no
longer masked by`TypeError` in`csv.reader()` ,`csv.writer.writerow()` and`csv.writer.writerows()` .
- bpo-39384: Fixed email.contentmanager to allow set_content() to set a null string.
- bpo-40744: The `sqlite3` module uses SQLite API functions that
require SQLite v3.7.3 or higher.  This patch removes support for older
SQLite versions, and explicitly requires SQLite 3.7.3 both at build,
compile and runtime.  Patch by Sergey Fedoseev and Erlend E. Aasland.
- bpo-40777: Initialize PyDateTime_IsoCalendarDateType.tp_base at run-time to avoid errors on some compilers.
- bpo-38488: Update ensurepip to install pip 20.1.1 and setuptools 47.1.0.
- bpo-40792: The result of `operator.index()` now always has exact type`int` . Previously, the result could have been an instance of a
subclass of`int` .
- bpo-40767: `webbrowser` now properly finds the default browser in
pure Wayland systems by checking the WAYLAND_DISPLAY environment variable.
Patch contributed by JÃ©rÃ©my Attali.
- bpo-40791: `hashlib.compare_digest()` uses OpenSSLâs`CRYPTO_memcmp()` function when OpenSSL is available.
- bpo-40795: `ctypes` module: If ctypes fails to convert the result of
a callback or if a ctypes callback function raises an exception,
sys.unraisablehook is now called with an exception set. Previously, the
error was logged into stderr by`PyErr_Print()` .
- bpo-16995: Add `base64.b32hexencode()` and`base64.b32hexdecode()` to support the Base32 Encoding with Extended Hex Alphabet.
- bpo-30008: Fix `ssl` code to be compatible with OpenSSL 1.1.x builds
that use`no-deprecated` and`--api=1.1.0` .
- bpo-30064: Fix asyncio `loop.sock_*` race condition issue
- bpo-40759: Deprecate the `symbol` module.
- bpo-40756: The second argument (extra) of `LoggerAdapter.__init__` now
defaults to None.
- bpo-37129: Add a new `os.RWF_APPEND` flag for`os.pwritev()` .
- bpo-40737: Fix possible reference leak for `sqlite3` initialization.
- bpo-40726: Handle cases where the `end_lineno` is`None` on`ast.increment_lineno()` .
- bpo-40698: `distutils` upload creates SHA2-256 and Blake2b-256 digests.
MD5 digests is skipped if platform blocks MD5.
- bpo-40695: `hashlib` no longer falls back to builtin hash
implementations when OpenSSL provides a hash digest and the algorithm is
blocked by security policy.
- bpo-9216: `hashlib.new()` passed`usedforsecurity` to OpenSSL EVP
constructor`_hashlib.new()` . test_hashlib and test_smtplib handle
strict security policy better.
- bpo-40614: `ast.parse()` will not parse self documenting expressions
in f-strings when passed`feature_version` is less than`(3, 8)` .
- bpo-40626: Add h5 file extension as MIME Type application/x-hdf5, as per HDF Group recommendation for HDF5 formatted data files. Patch contributed by Mark Schwab.
- bpo-25920: On macOS, when building Python for macOS 10.4 and older, which wasnât the case for python.org macOS installer, `socket.getaddrinfo()` no longer uses an internal lock to prevent race conditions when calling`getaddrinfo()` which is thread-safe since macOS 10.5. Python 3.9
requires macOS 10.6 or newer. The internal lock caused random hang on fork
when another thread was calling`socket.getaddrinfo()` . The lock was
also used on FreeBSD older than 5.3, OpenBSD older than 201311 and NetBSD
older than 4.
- bpo-40671: Prepare `_hashlib` for**PEP 489** and use`PyModule_AddType()` .
- bpo-32309: Added a new coroutine `asyncio.to_thread()` . It is
mainly used for running IO-bound functions in a separate thread to avoid
blocking the event loop, and essentially works as a high-level version of`run_in_executor()` that can directly take keyword
arguments.
- bpo-36543: Restored the deprecated `xml.etree.cElementTree` module.
- bpo-40611: `MAP_POPULATE` constant has now been added to the
list of exported`mmap` module flags.
- bpo-39881: PEP 554 for use in the test suite. (Patch By Joannah Nanjekye)
- bpo-13097: `ctypes` now raises an`ArgumentError` when a callback is
invoked with more than 1024 arguments.
- bpo-39385: A new test assertion context-manager, `unittest.assertNoLogs()` will ensure a given block of code emits no
log messages using the logging module. Contributed by Kit Yan Choi.
- bpo-23082: Updated the error message and docs of PurePath.relative_to() to better reflect the function behaviour.
- bpo-40318: Use SQLite3 trace v2 API, if it is available.
- bpo-40105: ZipFile truncates files to avoid corruption when a shorter comment is provided in append (âaâ) mode. Patch by Jan Mazur.
- bpo-40084: Fix `Enum.__dir__` : dir(Enum.member) now includes attributes
as well as methods.
- bpo-31122: ssl.wrap_socket() now raises ssl.SSLEOFError rather than OSError when peer closes connection during TLS negotiation
- bpo-39728: fix default `_missing_` so a duplicate`ValueError` is not
set as the`__context__` of the original`ValueError` .
- bpo-39244: Fixed `multiprocessing.context.get_all_start_methods` to
properly return the default method first on macOS.
- bpo-39040: Fix parsing of invalid mime headers parameters by collapsing whitespace between encoded words in a bare-quote-string.
- bpo-38731: Add `--quiet` option to command-line interface of`py_compile` . Patch by Gregory Schevchenko.
- bpo-35714: `struct.error` is now raised if there is a null character
in a`struct` format string.
- bpo-38144: Added the *root_dir* and*dir_fd* parameters in`glob.glob()` .
- bpo-26543: Fix `IMAP4.noop()` when debug mode is enabled (ex:`imaplib.Debug = 3` ).
- bpo-12178: `csv.writer()` now correctly escapes*escapechar* when
input contains*escapechar* .  Patch by Catalin Iacob, Berker Peksag, and
Itay Elbirt.
- bpo-36290: AST nodes are now raising `TypeError` on conflicting
keyword arguments. Patch contributed by RÃ©mi Lapeyre.
- bpo-33944: Added site.py site-packages tracing in verbose mode.
- bpo-35078: Refactor formatweekday, formatmonthname methods in LocaleHTMLCalendar and LocaleTextCalendar classes in calendar module to call the base class methods.This enables customizable CSS classes for LocaleHTMLCalendar. Patch by Srinivas Reddy Thatiparthy
- bpo-29620: `assertWarns()` no longer raises a`RuntimeException` when accessing a moduleâs`__warningregistry__` causes importation of a new module, or when a new module is imported in
another thread. Patch by Kernc.
- bpo-31844: Remove `ParserBase.error()` method from the private and
undocumented`_markupbase` module.`html.parser.HTMLParser` is
the only subclass of`ParserBase` and its`error()` implementation was
deprecated in Python 3.4 and removed in Python 3.5.
- bpo-34226: Fix `cgi.parse_multipart` without content_length. Patch by
Roger Duran
- bpo-33660: Fix pathlib.PosixPath to resolve a relative path located on the root directory properly.
- bpo-28557: Improve the error message for a misbehaving `rawio.readinto`
- bpo-26680: The d.is_integer() method is added to the Decimal type, for compatibility with other number types.
- bpo-26680: The x.is_integer() method is incorporated into the abstract types of the numeric tower, Real, Rational and Integral, with appropriate default implementations.

### DocumentationÂ¶

- bpo-41428: Add documentation for **PEP 604** (Allow writing union types as`X | Y` ).
- bpo-41774: In Programming FAQ âSequences (Tuples/Lists)â section, add âHow do you remove multiple items from a listâ.
- bpo-35293: Fix RemovedInSphinx40Warning when building the documentation. Patch by Donghee Na.
- bpo-37149: Change Shipman tkinter doc link from archive.org to TkDocs. (The doc has been removed from the NMT server.) The new link responds much faster and includes a short explanatory note.
- bpo-41726: Update the refcounts info of `PyType_FromModuleAndSpec` .
- bpo-41624: Fix the signature of `typing.Coroutine` .
- bpo-40204: Enable Sphinx 3.2 `c_allow_pre_v3` option and disable`c_warn_on_allowed_pre_v3` option to make the documentation compatible
with Sphinx 2 and Sphinx 3.
- bpo-41045: Add documentation for debug feature of f-strings.
- bpo-41314: Changed the release when `from __future__ import annotations` becomes the default from`4.0` to`3.10` (following a change in PEP
563).
- bpo-40979: Refactored typing.rst, arranging more than 70 classes, functions, and decorators into new sub-sections.
- bpo-40552: Fix in tutorial section 4.2. Code snippet is now correct.
- bpo-39883: Make code, examples, and recipes in the Python documentation be licensed under the more permissive BSD0 license in addition to the existing Python 2.0 license.
- bpo-37703: Updated Documentation to comprehensively elaborate on the behaviour of gather.cancel()

### TestsÂ¶

- bpo-41939: Fix test_site.test_license_exists_at_url(): call `urllib.request.urlcleanup()` to reset the global`urllib.request._opener` . Patch by Victor Stinner.
- bpo-41731: Make test_cmd_line_script pass with option â-vvâ.
- bpo-41602: Add tests for SIGINT handling in the runpy module.
- bpo-41521: `test.support` : Rename`blacklist` parameter of`check__all__()` to`not_exported` .
- bpo-41477: Make ctypes optional in test_genericalias.
- bpo-41085: Fix integer overflow in the `array.array.index()` method on
64-bit Windows for index larger than`2**31` .
- bpo-41069: `test.support.TESTFN` and the current directory for tests
when run via`test.regrtest` contain now non-ascii characters if
possible.
- bpo-38377: On Linux, skip tests using multiprocessing if the current user cannot create a file in `/dev/shm/` directory. Add the`skip_if_broken_multiprocessing_synchronize()` function
to the`test.support` module.
- bpo-41009: Fix use of `support.require_{linux|mac|freebsd}_version()` decorators as class decorator.
- bpo-41003: Fix `test_copyreg` when`numpy` is installed:`test.pickletester` now saves/restores warnings filters when importing`numpy` , to ignore filters installed by`numpy` .
- bpo-40964: Disable remote `imaplib` tests, host cyrus.andrew.cmu.edu
is blocking incoming connections.
- bpo-40927: Fix test_binhex when run twice: it now uses import_fresh_module() to ensure that it raises DeprecationWarning each time.
- bpo-17258: Skip some `multiprocessing` tests when MD5 hash digest is
blocked.
- bpo-31904: Increase LOOPBACK_TIMEOUT to 10 for VxWorks RTOS.
- bpo-38169: Increase code coverage for SharedMemory and ShareableList
- bpo-34401: Make test_gdb properly run on HP-UX. Patch by Michael Osipov.

### BuildÂ¶

- bpo-38249: Update `Py_UNREACHABLE` to use __builtin_unreachable()
if only the compiler is able to use it. Patch by Donghee Na.
- bpo-41617: Fix `pycore_bitutils.h` header file to support old clang
versions:`__builtin_bswap16()` is not available in LLVM clang 3.0.
- bpo-40204: Pin Sphinx version to 2.3.1 in `Doc/Makefile` .
- bpo-36020: The C99 functions `snprintf()` and`vsnprintf()` are now required to build Python.
- bpo-40684: `make install` now uses the`PLATLIBDIR` variable for the
destination`lib-dynload/` directory when```
./configure
--with-platlibdir
```
is used.
- bpo-40683: Fixed an issue where the `zoneinfo` module and its tests
were not included when Python is installed with`make` .

### WindowsÂ¶

- bpo-41744: Fixes automatic import of props file when using the Nuget package.
- bpo-41627: The user site directory for 32-bit now includes a `-32` suffix to distinguish it from the 64-bit interpreterâs directory.
- bpo-41526: Fixed layout of final page of the installer by removing the special thanks to Mark Hammond (with his permission).
- bpo-41492: Fixes the description that appears in UAC prompts.
- bpo-40948: Improve post-install message to direct people to the âpyâ command.
- bpo-41412: The installer will now fail to install on Windows 7 and Windows 8. Further, the UCRT dependency is now always downloaded on demand.
- bpo-40741: Update Windows release to include SQLite 3.32.3.
- bpo-41142: `msilib` now supports creating CAB files with non-ASCII
file path and adding files with non-ASCII file path to them.
- bpo-41074: Fixed support of non-ASCII names in functions `msilib.OpenDatabase()` and`msilib.init_database()` and
non-ASCII SQL in method`msilib.Database.OpenView()` .
- bpo-41039: Stable ABI redirection DLL (python3.dll) now uses ```
#pragma
comment(linker)
```
for re-exporting.
- bpo-40164: Updates Windows OpenSSL to 1.1.1g
- bpo-39631: Changes the registered MIME type for `.py` files on Windows
to`text/x-python` instead of`text/plain` .
- bpo-40677: Manually define IO_REPARSE_TAG_APPEXECLINK in case some old Windows SDK doesnât have it.
- bpo-37556: Extend py.exe help to mention overrides via venv, shebang, environmental variables & ini files.

### macOSÂ¶

- bpo-41557: Update macOS installer to use SQLite 3.33.0.
- bpo-39580: Avoid opening Finder window if running installer from the command line. Patch contributed by Rick Heil.
- bpo-41100: Fix configure error when building on macOS 11. Note that the current Python release was released shortly after the first developer preview of macOS 11 (Big Sur); there are other known issues with building and running on the developer preview. Big Sur is expected to be fully supported in a future bugfix release of Python 3.8.x and with 3.9.0.
- bpo-40741: Update macOS installer to use SQLite 3.32.3.
- bpo-41005: fixed an XDG settings issue not allowing macos to open browser in webbrowser.py
- bpo-40741: Update macOS installer to use SQLite 3.32.2.

### IDLEÂ¶

- bpo-41775: Use âIDLE Shellâ as shell title
- bpo-35764: Rewrite the Calltips doc section.
- bpo-40181: In calltips, stop reminding that â/â marks the end of positional-only arguments.
- bpo-41468: Improve IDLE run crash error message (which users should never see).
- bpo-41373: Save files loaded with no line ending, as when blank, or different line endings, by setting its line ending to the system default. Fix regression in 3.8.4 and 3.9.0b4.
- bpo-41300: Save files with non-ascii chars. Fix regression released in 3.9.0b4 and 3.8.4.
- bpo-37765: Add keywords to module name completion list. Rewrite Completions section of IDLE doc.
- bpo-41152: The encoding of `stdin` ,`stdout` and`stderr` in IDLE is
now always UTF-8.
- bpo-41144: Make Open Module open a special module such as os.path.
- bpo-39885: Make context menu Cut and Copy work again when right-clicking within a selection.
- bpo-40723: Make test_idle pass when run after import.

### C APIÂ¶

- bpo-41936: Removed undocumented macros `Py_ALLOW_RECURSION` and`Py_END_ALLOW_RECURSION` and the`recursion_critical` field of the`PyInterpreterState` structure.
- bpo-41692: The `PyUnicode_InternImmortal()` function is now deprecated
and will be removed in Python 3.12: use`PyUnicode_InternInPlace()` instead. Patch by Victor Stinner.
- bpo-41842: Add `PyCodec_Unregister()` function to unregister a codec
search function.
- bpo-41834: Remove the `_Py_CheckRecursionLimit` variable: it has been
replaced by`ceval.recursion_limit` of the`PyInterpreterState` structure. Patch by Victor Stinner.
- bpo-41689: Types created with `PyType_FromSpec()` now make any
signature in their`tp_doc` slot accessible from`__text_signature__` .
- bpo-41524: Fix bug in `PyOS_mystrnicmp()` and`PyOS_mystricmp()` that incremented pointers beyond the end of a
string.
- bpo-41324: Add a minimal decimal capsule API. The API supports fast conversions between Decimals up to 38 digits and their triple representation as a C struct.
- bpo-30155: Add `PyDateTime_DATE_GET_TZINFO()` and`PyDateTime_TIME_GET_TZINFO()` macros for accessing the`tzinfo` attributes of`datetime.datetime` and`datetime.time` objects.
- bpo-40170: Revert `PyType_HasFeature()` change: it reads again
directly the`PyTypeObject.tp_flags` member when the limited C
API is not used, rather than always calling`PyType_GetFlags()` which hides implementation details.
- bpo-41123: Remove `PyUnicode_AsUnicodeCopy` .
- bpo-41123: Removed `PyLong_FromUnicode()` .
- bpo-41123: Removed `PyUnicode_GetMax()` .
- bpo-41123: Removed `Py_UNICODE_str*` functions manipulating`Py_UNICODE*` strings.
- bpo-41103: `PyObject_AsCharBuffer()` ,`PyObject_AsReadBuffer()` ,`PyObject_CheckReadBuffer()` , and`PyObject_AsWriteBuffer()` are
removed. Please migrate to new buffer protocol;`PyObject_GetBuffer()` and`PyBuffer_Release()` .
- bpo-36346: Raises DeprecationWarning for ```
PyUnicode_FromUnicode(NULL,
size)
```
