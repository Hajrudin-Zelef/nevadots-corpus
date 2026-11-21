---
id: collect-261121-veille-ia/langages-dev/whatas-new-in-python-3-15-2-2
title: "Whatâs new in Python 3.15"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["python", "deprecated", "contributed", "trace", "tkinter", "removal", "removed", "version"]
source: https://docs.python.org/dev/whatsnew/3.15.html
source_anchor: ""
source_lines: [1501, 1968]
section: "Langages_Dev"
---
  - The `tkinter.Variable` methods`trace_variable()` ,`trace()` (an alias of`trace_variable()` ),`trace_vdelete()` and`trace_vinfo()` , deprecated since
Python 3.14, are scheduled for removal in Python 3.17.
Use`trace_add()` ,`trace_remove()` and`trace_info()` instead.
(Contributed by Serhiy Storchaka in gh-120220.)

### Pending removal in Python 3.18Â¶

- No longer accept a boolean value when a file descriptor is expected. (Contributed by Serhiy Storchaka in gh-82626.)
- `decimal` :
- `urllib.parse` :
  - Accepting objects with false values (like `0` and`[]` ) except empty
strings, byte-like objects and`None` in the URL parsing functions has
been deprecated since Python 3.16.
(Contributed by Jacob Walls in gh-63293.)
- Deprecations defined by **PEP 829** :
  - `import` lines in*name* .pth
 (Contributed by Barry Warsaw in gh-148641.)

### Pending removal in Python 3.19Â¶

- `builtins` :
- `ctypes` :
- `hashlib` :
  - In hash function constructors such as `new()` or the
direct hash-named constructors such as`md5()` and`sha256()` , their optional initial data parameter could
also be passed a keyword argument named`data=` or`string=` in
various`hashlib` implementations.Support for the `string` keyword argument name is now deprecated
and slated for removal in Python 3.19.Before Python 3.13, the `string` keyword parameter was not correctly
supported depending on the backend implementation of hash functions.
Prefer passing the initial data as a positional argument for maximum
backwards compatibility.
- `http.cookies` :
  - `http.cookies.Morsel.js_output()` is deprecated and will be
removed in Python 3.19.
  - `http.cookies.BaseCookie.js_output()` is deprecated and will be
removed in Python 3.19.
- `imaplib` :
  - Altering `IMAP4.file` is now deprecated
and slated for removal in Python 3.19. This property is now unused
and changing its value does not automatically close the current file.Before Python 3.14, this property was used to implement the corresponding `read()` and`readline()` methods for`IMAP4` but this
is no longer the case since then.
- `tkinter` :
  - `tkinter.filedialog.askopenfiles()` has been deprecated since Python
3.16.  Iterate over the names returned by`askopenfilenames()` and open them one by one
instead.
(Contributed by Serhiy Storchaka in gh-152638.)

### Pending removal in Python 3.20Â¶

- Calling the `__new__()` method of`struct.Struct` without the*format* argument is deprecated and will be removed in Python 3.20.  Calling`__init__()` method on initialized`Struct` objects is deprecated and will be removed in Python 3.20.(Contributed by Sergey B Kirpichev and Serhiy Storchaka in gh-143715.)
- The `__version__` ,`version` and`VERSION` attributes have been
deprecated in these standard library modules and will be removed in
Python 3.20. Use`sys.version_info` instead.
  - `argparse`
  - `csv`
  - `ctypes`
  - `ctypes.macholib`
  - `decimal` (use`decimal.SPEC_VERSION` instead)
  - `http.server`
  - `imaplib`
  - `ipaddress`
  - `json`
  - `logging` (`__date__` also deprecated)
  - `optparse`
  - `pickle`
  - `platform`
  - `re`
  - `socketserver`
  - `tabnanny`
  - `tarfile`
  - `tkinter.font`
  - `tkinter.ttk`
  - `wsgiref.simple_server`
  - `xml.etree.ElementTree`
  - `xml.sax.expatreader`
  - `xml.sax.handler`
  - `zlib`
 (Contributed by Hugo van Kemenade and Stan Ulbrych in gh-76007.)
- The `__author__` ,`__credits__` ,`__copyright__` and`__date__` attributes have been deprecated in these standard library modules and will
be removed in Python 3.20. Attribution is preserved in the source code
and the version control history.
  - `bz2`
  - `concurrent.futures`
  - `inspect`
  - `io`
  - `json`
  - `logging`
  - `optparse`
  - `platform`
  - `pydoc`
  - `tarfile`
  - `tkinter.ttk`
  - `tokenize`
  - `uuid`
 (Contributed by Stan Ulbrych in gh-159240.)
- Deprecations defined by **PEP 829** :
  - Warnings are produced for `import` lines found in*name* .pth
*name* .pth
**MUST** be encoded in`utf-8-sig` .
 (Contributed by Barry Warsaw in gh-148641.)
- `ast` :
  - Creating instances of abstract AST nodes (such as `ast.AST` or`ast.expr` ) is deprecated and will raise an error in Python 3.20.
- `typing` :
  - It is deprecated to call `isinstance()` and`issubclass()` checks on
protocol classes that were not explicitly decorated with`runtime_checkable()` but that inherit from a runtime-checkable protocol class.
This will raise a`TypeError` in Python 3.20.(Contributed by Bartosz SÅawecki in gh-132604.)

### Pending removal in Python 3.21Â¶

- `abc`
  - Soft-deprecated since Python 3.3 `abc.abstractclassmethod` ,`abc.abstractstaticmethod` , and`abc.abstractproperty` now raise a`DeprecationWarning` .
These classes will be removed in Python 3.21, instead
use`abc.abstractmethod()` with`classmethod()` ,`staticmethod()` , and`property` respectively.
- `ast` :
  - Classes `slice` ,`Index` ,`ExtSlice` ,`Suite` ,`Param` ,`AugLoad` and`AugStore` , will be removed in Python 3.21. These types
are not generated by the parser or accepted by the code generator.
  - The `dims` property of`ast.Tuple` will be removed in Python 3.21. Use
the`ast.Tuple.elts` property instead.
- `functools` :
  - Using a return annotation to infer the dispatch type for `functools.singledispatch()` or`functools.singledispatchmethod` is deprecated since Python 3.16
and will raise`TypeError` in Python 3.21. Annotate the dispatch
parameter or pass the dispatch type explicitly instead.
(Contributed by Bartosz SÅawecki in gh-143465.)
- `struct` :
  - Soft-deprecated since Python 3.15, using `'F'` and`'D'` type codes are now
deprecated.  These codes will be removed in Python 3.21.  Use instead
two-letter forms`'Zf'` and`'Zd'` .
- `tempfile` :
  - `tempfile._TemporaryFileWrapper` will be removed in Python 3.21. Use the
public`tempfile.TemporaryFileWrapper` instead.
- `typing` :

### Pending removal in future versionsÂ¶

The following APIs will be removed in the future, although there is currently no date scheduled for their removal.

- `argparse` :
  - Nesting argument groups and nesting mutually exclusive groups are deprecated.
  - Passing the undocumented keyword argument *prefix_chars* to`add_argument_group()` is now
deprecated.
  - The `argparse.FileType` type converter is deprecated.
- `builtins` :
  - Generators: `throw(type, exc, tb)` and`athrow(type, exc, tb)` signature is deprecated: use`throw(exc)` and`athrow(exc)` instead,
the single argument signature.
  - Currently Python accepts numeric literals immediately followed by keywords, for example `0in x` ,`1or x` ,`0if 1else 2` .  It allows confusing and
ambiguous expressions like`[0x1for x in y]` (which can be interpreted as`[0x1 for x in y]` or`[0x1f or x in y]` ).  A syntax warning is raised
if the numeric literal is immediately followed by one of keywords`and` ,`else` ,`for` ,`if` ,`in` ,`is` and`or` .  In a future release it
will be changed to a syntax error. (gh-87999)
  - Support for `__index__()` and`__int__()` method returning non-int type:
these methods will be required to return an instance of a strict subclass of`int` .
  - Support for `__float__()` method returning a strict subclass of`float` : these methods will be required to return an instance of`float` .
  - Support for `__complex__()` method returning a strict subclass of`complex` : these methods will be required to return an instance of`complex` .
- `calendar` :`calendar.January` and`calendar.February` constants are
deprecated and replaced by`calendar.JANUARY` and`calendar.FEBRUARY` .
(Contributed by Prince Roshan in gh-103636.)
- `codecs` : use`open()` instead of`codecs.open()` . (gh-133038)
- `codeobject.co_lnotab` : use the`codeobject.co_lines()` method
instead.
- `datetime` :
  - `utcnow()` :
use`datetime.datetime.now(tz=datetime.UTC)` .
  - `utcfromtimestamp()` :
use`datetime.datetime.fromtimestamp(timestamp, tz=datetime.UTC)` .
- `gettext` : Plural value must be an integer.
- `importlib` :
  - `cache_from_source()`*debug_override* parameter is
deprecated: use the*optimization* parameter instead.
- `importlib.metadata` :
  - `EntryPoints` tuple interface.
  - Implicit `None` on return values.
- `logging` : the`warn()` method has been deprecated
since Python 3.3, use`warning()` instead.
- `mailbox` : Use of StringIO input and text mode is deprecated, use
BytesIO and binary mode instead.
- `os` : Calling`os.register_at_fork()` in a multi-threaded process.
- `os.path` :`os.path.commonprefix()` is deprecated, use`os.path.commonpath()` for path prefixes. The`os.path.commonprefix()` function is being deprecated due to having a misleading name and module.
The function is not safe to use for path prefixes despite being included in a
module about path manipulation, meaning it is easy to accidentally
introduce path traversal vulnerabilities into Python programs by using this
function.
- `pydoc.ErrorDuringImport` : A tuple value for*exc_info* parameter is
deprecated, use an exception instance.
- `re` : More strict rules are now applied for numerical group references
and group names in regular expressions.  Only sequence of ASCII digits is now
accepted as a numerical reference.  The group name in bytes patterns and
replacement strings can now only contain ASCII letters and digits and
underscore.
(Contributed by Serhiy Storchaka in gh-91760.)
- `shutil` :`rmtree()` âs*onerror* parameter is deprecated in
Python 3.12; use the*onexc* parameter instead.
- `ssl` options and protocols:
  - `ssl.SSLContext` without protocol argument is deprecated.
  - `ssl.SSLContext` :`set_npn_protocols()` and`selected_npn_protocol()` are deprecated: use ALPN
instead.
  - `ssl.OP_NO_SSL*` options
  - `ssl.OP_NO_TLS*` options
  - `ssl.PROTOCOL_SSLv3`
  - `ssl.PROTOCOL_TLS`
  - `ssl.PROTOCOL_TLSv1`
  - `ssl.PROTOCOL_TLSv1_1`
  - `ssl.PROTOCOL_TLSv1_2`
  - `ssl.TLSVersion.SSLv3`
  - `ssl.TLSVersion.TLSv1`
  - `ssl.TLSVersion.TLSv1_1`
- `threading` methods:
  - `threading.Condition.notifyAll()` : use`notify_all()` .
  - `threading.Event.isSet()` : use`is_set()` .
  - `threading.Thread.isDaemon()` ,`threading.Thread.setDaemon()` :
use`threading.Thread.daemon` attribute.
  - `threading.Thread.getName()` ,`threading.Thread.setName()` :
use`threading.Thread.name` attribute.
  - `threading.currentThread()` : use`threading.current_thread()` .
  - `threading.activeCount()` : use`threading.active_count()` .
- `typing.Text` (gh-92332).
- The internal class `typing._UnionGenericAlias` is no longer used to implement`typing.Union` . To preserve compatibility with users using this private
class, a compatibility shim will be provided until at least Python 3.17. (Contributed by
Jelle Zijlstra in gh-105499.)
- `unittest.IsolatedAsyncioTestCase` : it is deprecated to return a value
that is not`None` from a test case.
- `urllib.parse` deprecated functions:`urlparse()` instead
  - `splitattr()`
  - `splithost()`
  - `splitnport()`
  - `splitpasswd()`
  - `splitport()`
  - `splitquery()`
  - `splittag()`
  - `splittype()`
  - `splituser()`
  - `splitvalue()`
  - `to_bytes()`
- `wsgiref` :`SimpleHandler.stdout.write()` should not do partial
writes.
- `xml.etree.ElementTree` : Testing the truth value of an`Element` is deprecated. In a future release it
will always return`True` . Prefer explicit`len(elem)` or`elem is not None` tests instead.
- `sys._clear_type_cache()` is deprecated:
use`sys._clear_internal_caches()` instead.

### Soft deprecationsÂ¶

There are no plans to remove soft deprecated APIs.

- `re.match()` and`re.Pattern.match()` are now
soft deprecated in favor of the new`re.prefixmatch()` and`re.Pattern.prefixmatch()` APIs, which have been added as alternate,
more explicit names. These are intended to be used to alleviate confusion
around what*match* means by following the Zen of Pythonâs*âExplicit is
better than implicitâ* mantra. Most other language regular expression
libraries use an API named*match* to mean what Python has always called*search* .We **do not** plan to remove the older`match()` name, as it has been
used in code for over 30 years. Code supporting older versions of Python
should continue to use`match()` , while new code should prefer`prefixmatch()` . See prefixmatch() vs. match().(Contributed by Gregory P. Smith in gh-86519 and Hugo van Kemenade in gh-148100.)

## C API changesÂ¶

### New featuresÂ¶

- Add `PyArg_ParseArray()` and`PyArg_ParseArrayAndKeywords()` functions to parse arguments of functions using the`METH_FASTCALL` calling convention.
(Contributed by Victor Stinner in gh-144175.)
- Add the following functions for the new `frozendict` type:(Contributed by Victor Stinner in gh-141510.)
- Add the **PEP 757** export/import functions to the limited C API:(Contributed by Sergey B Kirpichev in gh-143869.)
- Add `PyObject_CallFinalizerFromDealloc()` function to the limited C
API.
(Contributed by Victor Stinner in gh-146063.)
- Add `PySys_GetAttr()` ,`PySys_GetAttrString()` ,`PySys_GetOptionalAttr()` , and`PySys_GetOptionalAttrString()` functions as replacements for`PySys_GetObject()` .
(Contributed by Serhiy Storchaka in gh-108512.)
- Add `PyUnstable_Unicode_GET_CACHED_HASH()` to get the cached hash of
a string. See the documentation for caveats.
(Contributed by Petr Viktorin in gh-131510.)
- Add API for checking an extension moduleâs ABI compatibility: `Py_mod_abi` ,`PyABIInfo_Check()` ,
and`PyABIInfo_VAR` .
(Contributed by Petr Viktorin in gh-137210.)

- Implement **PEP 782** , the PyBytesWriter API.
Add functions:(Contributed by Victor Stinner in gh-129813.)
- `PyCriticalSection` and related functions are added to the Stable
ABI.
(Contributed by Petr Viktorin in gh-149225.)
- Add a new `PyImport_CreateModuleFromInitfunc()` C API for creating
a module from a*spec* and*initfunc* .
(Contributed by Itamar Oren in gh-116146.)
- Add `PyTuple_FromArray()` to create a`tuple` from an array.
(Contributed by Victor Stinner in gh-111489.)
- Add a new module export hook, `PyModExport_*` .(Contributed by Petr Viktorin in **PEP 793** and gh-140550.)
- Add functions that are guaranteed to be safe for use in `tp_traverse` handlers:`PyObject_GetTypeData_DuringGC()` ,`PyObject_GetItemData_DuringGC()` ,`PyType_GetModuleState_DuringGC()` ,`PyModule_GetState_DuringGC()` ,`PyModule_GetToken_DuringGC()` ,`PyType_GetBaseByToken_DuringGC()` ,`PyType_GetModule_DuringGC()` ,`PyType_GetModuleByToken_DuringGC()` .
(Contributed by Petr Viktorin in gh-145925.)
- Add `PyObject_Dump()` to dump an object to`stderr` .
It should only be used for debugging.
(Contributed by Victor Stinner in gh-141070.)
- Implement **PEP 820** :`PySlot` â Unified slot system for the C API.
See Definition slots for documentation.This adds: 
  - The `PySlot` struct;
  - the `PyType_FromSlots()` function;
  - new slot IDs: `Py_slot_end` ,`Py_slot_invalid` ;`Py_slot_subslots` ,`Py_tp_slots` ,`Py_mod_slots` ;`Py_tp_name` ,`Py_tp_basicsize` ,`Py_tp_extra_basicsize` ,`Py_tp_itemsize` ,`Py_tp_flags` ,`Py_tp_metaclass` ,`Py_tp_module` ;
  - convenience macros: `PySlot_DATA` ,`PySlot_FUNC` ,`PySlot_SIZE` ,`PySlot_INT64` ,`PySlot_UINT64` ,`PySlot_STATIC_DATA` ,`PySlot_END` ,`PySlot_PTR` ,`PySlot_PTR_STATIC` .
 The `PyModule_FromSlotsAndSpec()` function and`PyModExport` module export hook also
use the new`PySlot` struct.The following functions are soft deprecated: The slots `Py_tp_bases` and`Py_tp_base` are now
equivalent: they can be set either to a single type or a tuple of types.
The`Py_tp_bases` slot is preferred; the other is ignored if both
are specified.(Contributed by Petr Viktorin in gh-149044.)
- Add `PyUnstable_ThreadState_SetStackProtection()` and`PyUnstable_ThreadState_ResetStackProtection()` functions to set
the stack protection base address and stack protection size of a Python
thread state.
(Contributed by Victor Stinner in gh-139653.)
- Add `PyUnstable_SetImmortal()` C API function to mark objects as immortal.
(Contributed by Kumar Aditya in gh-143300.)
- Restore private provisional `_Py_InitializeMain()` function removed in
Python 3.14.
(Contributed by Victor Stinner in gh-142417.)
- Add `PyUnstable_DumpTraceback()` and`PyUnstable_DumpTracebackThreads()` functions to output Python
stacktraces.
(Contributed by Alex Malyshev in gh-145559.)

### Changed C APIsÂ¶

- If the `Py_TPFLAGS_MANAGED_DICT` or`Py_TPFLAGS_MANAGED_WEAKREF` flag is set then`Py_TPFLAGS_HAVE_GC` must be set too.
(Contributed by Sergey Miryanov in gh-134786.)
- `PyDateTime_IMPORT` is now thread safe. Code that directly checks`PyDateTimeAPI` for`NULL` should be updated to call`PyDateTime_IMPORT` instead.
(Contributed by Kumar Aditya in gh-141563.)

### Removed C APIsÂ¶

- Remove deprecated `PyUnicode` functions:
  - `PyUnicode_AsDecodedObject()` :
Use`PyCodec_Decode()` instead.
  - `PyUnicode_AsDecodedUnicode()` :
Use`PyCodec_Decode()` instead; note that some codecs (for example, âbase64â)
may return a type other than`str` , such as`bytes` .
  - `PyUnicode_AsEncodedObject()` :
Use`PyCodec_Encode()` instead.
  - `PyUnicode_AsEncodedUnicode()` :
Use`PyCodec_Encode()` instead; note that some codecs (for example, âbase64â)
may return a type other than`bytes` , such as`str` .
 (Contributed by Stan Ulbrych in gh-133612.)
- `PyImport_ImportModuleNoBlock()` : deprecated alias
of`PyImport_ImportModule()` .
(Contributed by BÃ©nÃ©dikt Tran in gh-133644.)
- `PyWeakref_GetObject()` and`PyWeakref_GET_OBJECT` :
use`PyWeakref_GetRef()` instead. The pythoncapi-compat project
can be used to get`PyWeakref_GetRef()` on Python 3.12 and older.
(Contributed by BÃ©nÃ©dikt Tran in gh-133644.)
- Remove deprecated `PySys_ResetWarnOptions()` .
Clear`sys.warnoptions` and`warnings.filters` instead.(Contributed by Nikita Sobolev in gh-138886.)

The following functions are removed in favor of `PyConfig_Get()`.
The pythoncapi-compat project can be used to get `PyConfig_Get()`
on Python 3.13 and older.

- Python initialization functions: 
  - `Py_GetExecPrefix()` :
use`PyConfig_Get("base_exec_prefix")` (`sys.base_exec_prefix` ) instead.
Use`PyConfig_Get("exec_prefix")` (`sys.exec_prefix` ) if virtual environments
need to be handled.
  - `Py_GetPath()` :
use`PyConfig_Get("module_search_paths")` (`sys.path` ) instead.
  - `Py_GetPrefix()` :
use`PyConfig_Get("base_prefix")` (`sys.base_prefix` ) instead.
Use`PyConfig_Get("prefix")` (`sys.prefix` ) if virtual environments
need to be handled.
  - `Py_GetProgramFullPath()` :
use`PyConfig_Get("executable")` (`sys.executable` ) instead.
  - `Py_GetProgramName()` :
use`PyConfig_Get("executable")` (`sys.executable` ) instead.
  - `Py_GetPythonHome()` :
use`PyConfig_Get("home")` or the`PYTHONHOME` environment variable instead.
 (Contributed by BÃ©nÃ©dikt Tran in gh-133644.)

### Deprecated C APIsÂ¶

- Deprecate **PEP 456** support for providing an external definition
of the string hashing scheme. Removal is scheduled for Python 3.19.Previously, embedders could define `Py_HASH_ALGORITHM` to be`Py_HASH_EXTERNAL` to indicate that the hashing scheme was provided
externally but this feature was undocumented, untested and most likely
unused.(Contributed by BÃ©nÃ©dikt Tran in gh-141226.)
- For unsigned integer formats in `PyArg_ParseTuple()` ,
accepting Python integers with value that is larger than the maximal value
for the C type or less than the minimal value for the corresponding
signed integer type of the same size is now deprecated.
(Contributed by Serhiy Storchaka in gh-132629.)
- `PyBytes_FromStringAndSize(NULL, len)` and`_PyBytes_Resize()` are soft deprecated,
use the`PyBytesWriter` API instead.
(Contributed by Victor Stinner in gh-129813.)
- `_PyObject_CallMethodId()` ,`_PyObject_GetAttrId()` and`_PyUnicode_FromId()` are deprecated since Python 3.15 and will be removed in
Python 3.20. Instead, use`PyUnicode_InternFromString()` and cache the result in
the module state, then call`PyObject_CallMethod()` or`PyObject_GetAttr()` .
(Contributed by Victor Stinner in gh-141049.)
- Deprecate `cval` field of the`PyComplexObject` type.
Use`PyComplex_AsCComplex()` and`PyComplex_FromCComplex()` to convert a Python complex number to/from the C`Py_complex` representation.
(Contributed by Sergey B Kirpichev in gh-128813.)
- Functions `_Py_c_sum()` ,`_Py_c_diff()` ,`_Py_c_neg()` ,`_Py_c_prod()` ,`_Py_c_quot()` ,`_Py_c_pow()` and`_Py_c_abs()` are soft deprecated.
(Contributed by Sergey B Kirpichev in gh-128813.)
- `bytes_warning` is deprecated
since Python 3.15 and will be removed in Python 3.17.
(Contributed by Nikita Sobolev in gh-136355.)
- `Py_INFINITY` macro is soft deprecated,
use the C11 standard`<math.h>``INFINITY` instead.
(Contributed by Sergey B Kirpichev in gh-141004.)
- The following macros are soft deprecated: 
  - `Py_ALIGNED` : Prefer`alignas` instead.
  - `PY_FORMAT_SIZE_T` : Use`"z"` directly.
  - `Py_LL` and`Py_ULL` :
Use standard suffixes,`LL` and`ULL` .
  - `PY_LONG_LONG` ,`PY_LLONG_MIN` ,`PY_LLONG_MAX` ,`PY_ULLONG_MAX` ,`PY_INT32_T` ,`PY_UINT32_T` ,`PY_INT64_T` ,`PY_UINT64_T` ,`PY_SIZE_MAX` :
Use C99 types/limits.
  - `Py_UNICODE_SIZE` : Use`sizeof(wchar_t)` directly.
  - `Py_VA_COPY` : Use`va_copy` directly.
 The macro `Py_UNICODE_WIDE` , which was scheduled for removal,
is soft deprecated instead.(Contributed by Petr Viktorin in gh-146175.)
- `Py_MATH_El` and`Py_MATH_PIl` are deprecated
since Python 3.15 and will be removed in Python 3.20.
(Contributed by Sergey B Kirpichev in gh-141004.)

## Build changesÂ¶

- Removed implicit fallback to the bundled copy of the `libmpdec` library.
Now this should be explicitly enabled with`--with-system-libmpdec` set to`no` or with`--without-system-libmpdec` .
(Contributed by Sergey B Kirpichev in gh-115119.)
- The new configure option `--with-missing-stdlib-config=FILE` allows
distributors to pass a JSON
configuration file containing custom error messages for standard library
modules that are missing or packaged separately.
(Contributed by Stan Ulbrych and Petr Viktorin in gh-139707.)
- The new configure option `--with-pymalloc-hugepages` enables huge
page support for pymalloc arenas. When enabled, arena size
increases to 2 MiB and allocation uses`MAP_HUGETLB` (Linux) or`MEM_LARGE_PAGES` (Windows) with automatic fallback to regular pages.
On Windows, use`build.bat --pymalloc-hugepages` .
At runtime, huge pages must be explicitly enabled by setting the`PYTHON_PYMALLOC_HUGEPAGES` environment variable to`1` .
(Contributed by Pablo Galindo Salgado in gh-144319.)
- Annotating anonymous mmap usage is now supported if the Linux kernel supports *PR_SET_VMA_ANON_NAME* (Linux 5.17 or newer).
Annotations are visible in`/proc/<pid>/maps` if the kernel supports the feature
and`-X dev` is passed to Python,
or Python is built in debug mode.
(Contributed by Donghee Na in gh-141770.)
- CPython is now built with frame pointers enabled by default ( **PEP 831** ). Pass`--without-frame-pointers` to opt out.Authors of C extensions and native libraries built with custom build systems should ensure the unwind chain is intact. This is usually done by adding `-fno-omit-frame-pointer` and
similar flags to`CFLAGS` . See`--without-frame-pointers` documentation for the specific flags Python uses.(Contributed by Pablo Galindo Salgado and Savannah Ostrowski in gh-149201.)

- 64-bit builds using Visual Studio 2026 (MSVC 18) may now use the new tail-calling interpreter. Results on Visual Studio 18.1.1 report between 15-20% speedup on the geometric mean of pyperformance on Windows x86-64 over the switch-case interpreter on an AMD Ryzen 7 5800X. We have observed speedups ranging from 14% for large pure-Python libraries to 40% for long-running small pure-Python scripts on Windows. This was made possible by a new feature introduced in MSVC 18, which the official Windows 64-bit binaries on python.org now use. (Contributed by Chris Eibl, Ken Jin, and Brandt Bucher in gh-143068. Special thanks to Steve Dower, and the MSVC team including Hulon Jenkins.)

## Porting to Python 3.15Â¶

This section lists previously described changes and other bugfixes that may require changes to your code.

- `sqlite3.Connection` APIs have been cleaned up.
  - All parameters of `sqlite3.connect()` except*database* are now keyword-only.
  - The first three parameters of methods `create_function()` and`create_aggregate()` are now positional-only.
  - The first parameter of methods `set_authorizer()` ,`set_progress_handler()` and`set_trace_callback()` is now positional-only.
 (Contributed by Serhiy Storchaka in gh-133595.)
- `resource.RLIM_INFINITY` is now always positive.
Passing a negative integer value that corresponded to its old value
(such as`-1` or`-3` , depending on platform) to`resource.setrlimit()` and`resource.prlimit()` is now deprecated.
(Contributed by Serhiy Storchaka in gh-137044.)
- `mmap.mmap.resize()` has been removed on platforms that donât support the
underlying syscall, instead of raising a`SystemError` .
(Contributed by An Long in gh-138205.)
- A resource warning is now emitted for an unclosed `xml.etree.ElementTree.iterparse()` iterator if it opened a file.
Use its`close()` method or the`contextlib.closing()` context
manager to close it.
(Contributed by Osama Abdelkader and Serhiy Storchaka in gh-140601.)
- If a short option and a single-dash long option are passed to `argparse.ArgumentParser.add_argument()` ,*dest* is now inferred from
the single-dash long option. For example, in`add_argument('-f', '-foo')` ,*dest* is now`'foo'` instead of`'f'` .
Pass an explicit*dest* argument to preserve the old behavior.
(Contributed by Serhiy Storchaka in gh-138697.)
- Padding of input no longer required in `base64.urlsafe_b64decode()` .
Pass a new argument`padded=True` or use`base64.b64decode()` with argument`altchars=b'-_'` (this works with older Python versions)
to make padding required.
(Contributed by Serhiy Storchaka in gh-73613.)
- Since `unittest.TestCase.assertWarns()` and`unittest.TestCase.assertWarnsRegex()` no longer swallow warnings that
do not match the specified category or regex, your tests may start leaking
some warnings that were previously masked.
Use warning filters to silence them or additional`assertWarns*()` to catch and check them.
(Contributed by Serhiy Storchaka in gh-143231.)
