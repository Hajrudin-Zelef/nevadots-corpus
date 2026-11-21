---
id: collect-261121-veille-ia/docs-techniques/changelog-2-19
title: "Changelog"
domain: docs-techniques
role: reference
task: article
actors: []
dates: ["2016-08-15"]
keywords: ["patch", "module", "types", "xiang", "zhang", "regular", "always", "used"]
source: https://docs.python.org/release/3.15.1/whatsnew/changelog.html
source_anchor: ""
source_lines: [27001, 28500]
section: "Docs_Techniques"
---
- bpo-27811: Fix a crash when a coroutine that has not been awaited is finalized with warnings-as-errors enabled.
- bpo-27587: Fix another issue found by PVS-Studio: Null pointer check after use of âdefâ in _PyState_AddModule(). Initial patch by Christian Heimes.
- bpo-27792: The modulo operation applied to `bool` and other`int` subclasses now always returns an`int` . Previously the return type
depended on the input values. Patch by Xiang Zhang.
- bpo-26984: int() now always returns an instance of exact int.
- bpo-25604: Fix a minor bug in integer true division; this bug could potentially have caused off-by-one-ulp results on platforms with unreliable ldexp implementations.
- bpo-24254: Make class definition namespace ordered by default.
- bpo-27662: Fix an overflow check in `List_New` : the original code was
checking against`Py_SIZE_MAX` instead of the correct upper bound of`Py_SSIZE_T_MAX` . Patch by Xiang Zhang.
- bpo-27782: Multi-phase extension module import now correctly allows the `m_methods` field to be used to add module level functions to instances
of non-module types returned from`Py_create_mod` . Patch by Xiang Zhang.
- bpo-27936: The round() function accepted a second None argument for some types but not for others. Fixed the inconsistency by accepting None for all numeric types.
- bpo-27487: Warn if a submodule argument to âpython -mâ or runpy.run_module() is found in sys.modules after parent packages are imported, but before the submodule is executed.
- bpo-27157: Make only type() itself accept the one-argument form. Patch by Eryk Sun and Emanuel Barry.
- bpo-27558: Fix a SystemError in the implementation of âraiseâ statement. In a brand new thread, raise a RuntimeError since there is no active exception to reraise. Patch written by Xiang Zhang.
- bpo-28008: Implement **PEP 530** â asynchronous comprehensions.
- bpo-27942: Fix memory leak in codeobject.c

### LibraryÂ¶

- bpo-28732: Fix crash in os.spawnv() with no elements in args
- bpo-28485: Always raise ValueError for negative compileall.compile_dir(workers=â¦) parameter, even when multithreading is unavailable.
- bpo-28037: Use sqlite3_get_autocommit() instead of setting Connection->inTransaction manually.
- bpo-25283: Attributes tm_gmtoff and tm_zone are now available on all platforms in the return values of time.localtime() and time.gmtime().
- bpo-24454: Regular expression match object groups are now accessible using __getitem__. âmo[x]â is equivalent to âmo.group(x)â.
- bpo-10740: sqlite3 no longer implicitly commit an open transaction before DDL statements.
- bpo-17941: Add a *module* parameter to collections.namedtuple().
- bpo-22493: Inline flags now should be used only at the start of the regular expression. Deprecation warning is emitted if uses them in the middle of the regular expression.
- bpo-26885: xmlrpc now supports unmarshalling additional data types used by Apache XML-RPC implementation for numerics and None.
- bpo-28070: Fixed parsing inline verbose flag in regular expressions.
- bpo-19500: Add client-side SSL session resumption to the ssl module.
- bpo-28022: Deprecate ssl-related arguments in favor of SSLContext. The deprecation include manual creation of SSLSocket and certfile/keyfile (or similar) in ftplib, httplib, imaplib, smtplib, poplib and urllib.
- bpo-28043: SSLContext has improved default settings: OP_NO_SSLv2, OP_NO_SSLv3, OP_NO_COMPRESSION, OP_CIPHER_SERVER_PREFERENCE, OP_SINGLE_DH_USE, OP_SINGLE_ECDH_USE and HIGH ciphers without MD5.
- bpo-24693: Changed some RuntimeErrorâs in the zipfile module to more appropriate types. Improved some error messages and debugging output.
- bpo-17909: `json.load` and`json.loads` now support binary input
encoded as UTF-8, UTF-16 or UTF-32. Patch by Serhiy Storchaka.
- bpo-27137: the pure Python fallback implementation of `functools.partial` now matches the behaviour of its accelerated C
counterpart for subclassing, pickling and text representation purposes.
Patch by Emanuel Barry and Serhiy Storchaka.
- Fix possible integer overflows and crashes in the mmap module with unusual usage patterns.
- bpo-1703178: Fix the ability to pass the âlink-objects option to the distutils build_ext command.
- bpo-28019: itertools.count() no longer rounds non-integer step in range between 1.0 and 2.0 to 1.
- bpo-18401: Pdb now supports the âreadrcâ keyword argument to control whether .pdbrc files should be read. Patch by Martin Matusiak and Sam Kimbrel.
- bpo-25969: Update the lib2to3 grammar to handle the unpacking generalizations added in 3.5.
- bpo-14977: mailcap now respects the order of the lines in the mailcap files (âfirst matchâ), as required by RFC 1542. Patch by Michael Lazar.
- bpo-28082: Convert re flag constants to IntFlag.
- bpo-28025: Convert all ssl module constants to IntEnum and IntFlags. SSLContext properties now return flags and enums.
- bpo-23591: Add Flag, IntFlag, and auto() to enum module.
- bpo-433028: Added support of modifier spans in regular expressions.
- bpo-24594: Validates persist parameter when opening MSI database
- bpo-17582: xml.etree.ElementTree nows preserves whitespaces in attributes (Patch by Duane Griffin. Reviewed and approved by Stefan Behnel.)
- bpo-28047: Fixed calculation of line length used for the base64 CTE in the new email policies.
- bpo-27576: Fix call order in OrderedDict.__init__().
- email.generator.DecodedGenerator now supports the policy keyword.
- bpo-28027: Remove undocumented modules from `Lib/plat-*` : IN, CDROM,
DLFCN, TYPES, CDIO, and STROPTS.
- bpo-27445: Donât pass str(_charset) to MIMEText.set_payload(). Patch by Claude Paroz.
- bpo-24277: The new email API is no longer provisional, and the docs have been reorganized and rewritten to emphasize the new API.
- bpo-22450: urllib now includes an `Accept: */*` header among the default
headers. This makes the results of REST API requests more consistent and
predictable especially when proxy servers are involved.
- lib2to3.pgen3.driver.load_grammar() now creates a stable cache file between runs given the same Grammar.txt input regardless of the hash randomization setting.
- bpo-28005: Allow ImportErrors in encoding implementation to propagate.
- bpo-26667: Support path-like objects in importlib.util.
- bpo-27570: Avoid zero-length memcpy() etc calls with null source pointers in the âctypesâ and âarrayâ modules.
- bpo-22233: Break email header lines *only* on the RFC specified CR and LF
characters, not on arbitrary unicode line breaks.  This also fixes a bug
in HTTP header parsing.
- bpo-27331: The email.mime classes now all accept an optional policy keyword.
- bpo-27988: Fix email iter_attachments incorrect mutation of payload list.
- bpo-16113: Add SHA-3 and SHAKE support to hashlib module.
- Eliminate a tautological-pointer-compare warning in _scproxy.c.
- bpo-27776: The `os.urandom()` function does now block on Linux 3.17
and newer until the system urandom entropy pool is initialized to increase
the security. This change is part of the**PEP 524** .
- bpo-27778: Expose the Linux `getrandom()` syscall as a new`os.getrandom()` function. This change is part of the**PEP 524** .
- bpo-27691: Fix ssl moduleâs parsing of GEN_RID subject alternative name fields in X.509 certs.
- bpo-18844: Add random.choices().
- bpo-25761: Improved error reporting about truncated pickle data in C implementation of unpickler. UnpicklingError is now raised instead of AttributeError and ValueError in some cases.
- bpo-26798: Add BLAKE2 (blake2b and blake2s) to hashlib.
- bpo-26032: Optimized globbing in pathlib by using os.scandir(); it is now about 1.5â4 times faster.
- bpo-25596: Optimized glob() and iglob() functions in the glob module; they are now about 3â6 times faster.
- bpo-27928: Add scrypt (password-based key derivation function) to hashlib module (requires OpenSSL 1.1.0).
- bpo-27850: Remove 3DES from ssl moduleâs default cipher list to counter measure sweet32 attack ( **CVE 2016-2183** ).
- bpo-27766: Add ChaCha20 Poly1305 to ssl moduleâs default cipher list. (Required OpenSSL 1.1.0 or LibreSSL).
- bpo-25387: Check return value of winsound.MessageBeep.
- bpo-27866: Add SSLContext.get_ciphers() method to get a list of all enabled ciphers.
- bpo-27744: Add AF_ALG (Linux Kernel crypto) to socket module.
- bpo-26470: Port ssl and hashlib module to OpenSSL 1.1.0.
- bpo-11620: Fix support for SND_MEMORY in winsound.PlaySound. Based on a patch by Tim Lesher.
- bpo-11734: Add support for IEEE 754 half-precision floats to the struct module. Based on a patch by Eli Stevens.
- bpo-27919: Deprecated `extra_path` distribution option in distutils
packaging.
- bpo-23229: Add new `cmath` constants:`cmath.inf` and`cmath.nan` to
match`math.inf` and`math.nan` , and also`cmath.infj` and`cmath.nanj` to match the format used by complex repr.
- bpo-27842: The csv.DictReader now returns rows of type OrderedDict. (Contributed by Steve Holden.)
- Remove support for passing a file descriptor to os.access. It never worked but previously didnât raise.
- bpo-12885: Fix error when distutils encounters symlink.
- bpo-27881: Fixed possible bugs when setting sqlite3.Connection.isolation_level. Based on patch by Xiang Zhang.
- bpo-27861: Fixed a crash in sqlite3.Connection.cursor() when a factory creates not a cursor. Patch by Xiang Zhang.
- bpo-19884: Avoid spurious output on OS X with Gnu Readline.
- bpo-27706: Restore deterministic behavior of random.Random().seed() for string seeds using seeding version 1. Allows sequences of calls to random() to exactly match those obtained in Python 2. Patch by Nofar Schnider.
- bpo-10513: Fix a regression in Connection.commit(). Statements should not be reset after a commit.
- bpo-12319: Chunked transfer encoding support added to http.client.HTTPConnection requests. The urllib.request.AbstractHTTPHandler class does not enforce a Content-Length header any more. If a HTTP request has a file or iterable body, but no Content-Length header, the library now falls back to use chunked transfer-encoding.
- A new version of typing.py from https://github.com/python/typing: - Collection (only for 3.6) (bpo-27598) - Add FrozenSet to __all__ (upstream #261) - fix crash in _get_type_vars() (upstream #259) - Remove the dict constraint in ForwardRef._eval_type (upstream #252)
- bpo-27832: Make `_normalize` parameter to`Fraction` constructor
keyword-only, so that`Fraction(2, 3, 4)` now raises`TypeError` .
- bpo-27539: Fix unnormalised `Fraction.__pow__` result in the case of
negative exponent and negative base.
- bpo-21718: cursor.description is now available for queries using CTEs.
- bpo-27819: In distutils sdists, simply produce the âgztarâ (gzipped tar format) distributions on all platforms unless âformatsâ is supplied.
- bpo-2466: posixpath.ismount now correctly recognizes mount points which the user does not have permission to access.
- bpo-9998: On Linux, ctypes.util.find_library now looks in LD_LIBRARY_PATH for shared libraries.
- bpo-27573: exit message for code.interact is now configurable.
- bpo-27930: Improved behaviour of logging.handlers.QueueListener. Thanks to Paulo Andrade and Petr Viktorin for the analysis and patch.
- bpo-6766: Distributed reference counting added to multiprocessing to support nesting of shared values / proxy objects.
- bpo-21201: Improves readability of multiprocessing error message. Thanks to Wojciech Walczak for patch.
- asyncio: Add set_protocol / get_protocol to Transports.
- bpo-27456: asyncio: Set TCP_NODELAY by default.

### IDLEÂ¶

- bpo-15308: Add âinterrupt executionâ (^C) to Shell menu. Patch by Roger Serwy, updated by Bayard Randel.
- bpo-27922: Stop IDLE tests from âflashingâ gui widgets on the screen.
- bpo-27891: Consistently group and sort imports within idlelib modules.
- bpo-17642: add larger font sizes for classroom projection.
- Add version to title of IDLE help window.
- bpo-25564: In section on IDLE â console differences, mention that using exec means that __builtins__ is defined for each statement.
- bpo-27821: Fix 3.6.0a3 regression that prevented custom key sets from being selected when no custom theme was defined.

### C APIÂ¶

### TestsÂ¶

- bpo-27427: Additional tests for the math module. Patch by Francisco Couzo.
- bpo-27953: Skip math and cmath tests that fail on OS X 10.4 due to a poor libm implementation of tan.
- bpo-26040: Improve test_math and test_cmath coverage and rigour. Patch by Jeff Allen.
- bpo-27787: Call gc.collect() before checking each test for âdangling threadsâ, since the dangling threads are weak references.

### BuildÂ¶

- bpo-27566: Fix clean target in freeze makefile (patch by Lisa Roach)
- bpo-27705: Update message in validate_ucrtbase.py
- bpo-27976: Deprecate building _ctypes with the bundled copy of libffi on non-OSX UNIX platforms.
- bpo-27983: Cause lack of llvm-profdata tool when using clang as required for PGO linking to be a configure time error rather than make time when `--with-optimizations` is enabled.  Also improve our ability to find the
llvm-profdata tool on MacOS and some Linuxes.
- bpo-21590: Support for DTrace and SystemTap probes.
- bpo-26307: The profile-opt build now applies PGO to the built-in modules.
- bpo-26359: Add the âwith-optimizations flag to turn on LTO and PGO build support when available.
- bpo-27917: Set platform triplets for Android builds.
- bpo-25825: Update references to the $(LIBPL) installation path on AIX. This path was changed in 3.2a4.
- Update OS X installer to use SQLite 3.14.1 and XZ 5.2.2.
- bpo-21122: Fix LTO builds on OS X.
- bpo-17128: Build OS X installer with a private copy of OpenSSL. Also provide a sample Install Certificates command script to install a set of root certificates from the third-party certifi module.

### Tools/DemosÂ¶

- bpo-27952: Get Tools/scripts/fixcid.py working with Python 3 and the current âreâ module, avoid invalid Python backslash escapes, and fix a bug parsing escaped C quote signs.

### WindowsÂ¶

- bpo-28065: Update xz dependency to 5.2.2 and build it from source.
- bpo-25144: Ensures TargetDir is set before continuing with custom install.
- bpo-1602: Windows console doesnât input or print Unicode (PEP 528)
- bpo-27781: Change file system encoding on Windows to UTF-8 (PEP 529)
- bpo-27731: Opt-out of MAX_PATH on Windows 10
- bpo-6135: Adds encoding and errors parameters to subprocess.
- bpo-27959: Adds oem encoding, alias ansi to mbcs, move aliasmbcs to codec lookup.
- bpo-27982: The functions of the winsound module now accept keyword arguments.
- bpo-20366: Build full text search support into SQLite on Windows.
- bpo-27756: Adds new icons for Python files and processes on Windows. Designs by Cherry Wang.
- bpo-27883: Update sqlite to 3.14.1.0 on Windows.

## Python 3.6.0 alpha 4Â¶

*Release date: 2016-08-15*

### Core and BuiltinsÂ¶

- bpo-27704: Optimized creating bytes and bytearray from byte-like objects and iterables. Speed up to 3 times for short objects. Original patch by Naoki Inada.
- bpo-26823: Large sections of repeated lines in tracebacks are now abbreviated as â[Previous line repeated {count} more times]â by the builtin traceback rendering. Patch by Emanuel Barry.
- bpo-27574: Decreased an overhead of parsing keyword arguments in functions implemented with using Argument Clinic.
- bpo-22557: Now importing already imported modules is up to 2.5 times faster.
- bpo-17596: Include <wincrypt.h> to help with Min GW building.
- bpo-17599: On Windows, rename the privately defined REPARSE_DATA_BUFFER structure to avoid conflicting with the definition from Min GW.
- bpo-27507: Add integer overflow check in bytearray.extend(). Patch by Xiang Zhang.
- bpo-27581: Donât rely on wrapping for overflow check in PySequence_Tuple(). Patch by Xiang Zhang.
- bpo-1621: Avoid signed integer overflow in list and tuple operations. Patch by Xiang Zhang.
- bpo-27419: Standard __import__() no longer look up â__import__â in globals or builtins for importing submodules or âfrom importâ. Fixed a crash if raise a warning about unabling to resolve package from __spec__ or __package__.
- bpo-27083: Respect the PYTHONCASEOK environment variable under Windows.
- bpo-27514: Make having too many statically nested blocks a SyntaxError instead of SystemError.
- bpo-27366: Implemented **PEP 487** (Simpler customization of class
creation). Upon subclassing, the __init_subclass__ classmethod is called
on the base class. Descriptors are initialized with __set_name__ after
class creation.

### LibraryÂ¶

- bpo-26027: Add **PEP 519** /__fspath__() support to the os and os.path
modules. Includes code from Jelle Zijlstra. (See also: bpo-27524)
- bpo-27598: Add Collections to collections.abc. Patch by Ivan Levkivskyi, docs by Neil Girdhar.
- bpo-25958: Support âanti-registrationâ of special methods from various ABCs, like __hash__, __iter__ or __len__. All these (and several more) can be set to None in an implementation class and the behavior will be as if the method is not defined at all. (Previously, this mechanism existed only for __hash__, to make mutable classes unhashable.) Code contributed by Andrew Barnert and Ivan Levkivskyi.
- bpo-16764: Support keyword arguments to zlib.decompress(). Patch by Xiang Zhang.
- bpo-27736: Prevent segfault after interpreter re-initialization due to ref count problem introduced in code for bpo-27038 in 3.6.0a3. Patch by Xiang Zhang.
- bpo-25628: The *verbose* and*rename* parameters for
collections.namedtuple are now keyword-only.
- bpo-12345: Add mathematical constant tau to math and cmath. See also **PEP 628** .
- bpo-26823: traceback.StackSummary.format now abbreviates large sections of repeated lines as â[Previous line repeated {count} more times]â (this change then further affects other traceback display operations in the module). Patch by Emanuel Barry.
- bpo-27664: Add to concurrent.futures.thread.ThreadPoolExecutor() the ability to specify a thread name prefix.
- bpo-27181: Add geometric_mean and harmonic_mean to statistics module.
- bpo-27573: code.interact now prints an message when exiting.
- bpo-6422: Add autorange method to timeit.Timer objects.
- bpo-27773: Correct some memory management errors server_hostname in _ssl.wrap_socket().
- bpo-26750: unittest.mock.create_autospec() now works properly for subclasses of property() and other data descriptors. Removes the never publicly used, never documented unittest.mock.DescriptorTypes tuple.
- bpo-26754: Undocumented support of general bytes-like objects as path in compile() and similar functions is now deprecated.
- bpo-26800: Undocumented support of general bytes-like objects as paths in os functions is now deprecated.
- bpo-26981: Add _order_ compatibility shim to enum.Enum for Python 2/3 code bases.
- bpo-27661: Added tzinfo keyword argument to datetime.combine.
- In the curses module, raise an error if window.getstr() or window.instr() is passed a negative value.
- bpo-27783: Fix possible usage of uninitialized memory in operator.methodcaller.
- bpo-27774: Fix possible Py_DECREF on unowned object in _sre.
- bpo-27760: Fix possible integer overflow in binascii.b2a_qp.
- bpo-27758: Fix possible integer overflow in the _csv module for large record lengths.
- bpo-27568: Prevent HTTPoxy attack ( **CVE 2016-1000110** ). Ignore the
HTTP_PROXY variable when REQUEST_METHOD environment is set, which
indicates that the script is in CGI mode.
- bpo-7063: Remove dead code from the âarrayâ moduleâs slice handling. Patch by Chuck.
- bpo-27656: Do not assume sched.h defines any SCHED_* constants.
- bpo-27130: In the âzlibâ module, fix handling of large buffers (typically 4 GiB) when compressing and decompressing. Previously, inputs were limited to 4 GiB, and compression and decompression operations did not properly handle results of 4 GiB.
- bpo-24773: Implemented **PEP 495** (Local Time Disambiguation).
- Expose the EPOLLEXCLUSIVE constant (when it is defined) in the select module.
- bpo-27567: Expose the EPOLLRDHUP and POLLRDHUP constants in the select module.
- bpo-1621: Avoid signed int negation overflow in the âaudioopâ module.
- bpo-27533: Release GIL in nt._isdir
- bpo-17711: Fixed unpickling by the persistent ID with protocol 0. Original patch by Alexandre Vassalotti.
- bpo-27522: Avoid an unintentional reference cycle in email.feedparser.
- bpo-27512: Fix a segfault when os.fspath() called an __fspath__() method that raised an exception. Patch by Xiang Zhang.

### IDLEÂ¶

- bpo-27714: text_textview and test_autocomplete now pass when re-run in the same process. This occurs when test_idle fails when run with the -w option but without -jn. Fix warning from test_config.
- bpo-27621: Put query response validation error messages in the query box itself instead of in a separate messagebox. Redo tests to match. Add Mac OSX refinements. Original patch by Mark Roseman.
- bpo-27620: Escape key now closes Query box as cancelled.
- bpo-27609: IDLE: tab after initial whitespace should tab, not autocomplete. This fixes problem with writing docstrings at least twice indented.
- bpo-27609: Explicitly return None when there are also non-None returns. In a few cases, reverse a condition and eliminate a return.
- bpo-25507: IDLE no longer runs buggy code because of its tkinter imports. Users must include the same imports required to run directly in Python.
- bpo-27173: Add âIDLE Modern Unixâ to the built-in key sets. Make the default key set depend on the platform. Add tests for the changes to the config module.
- bpo-27452: add line counter and crc to IDLE configHandler test dump.

### TestsÂ¶

- bpo-25805: Skip a test in test_pkgutil as needed that doesnât work when `__name__ == __main__` . Patch by SilentGhost.
- bpo-27472: Add test.support.unix_shell as the path to the default shell.
- bpo-27369: In test_pyexpat, avoid testing an error message detail that changed in Expat 2.2.0.
- bpo-27594: Prevent assertion error when running test_ast with coverage enabled: ensure code object has a valid first line number. Patch suggested by Ivan Levkivskyi.

### WindowsÂ¶

### BuildÂ¶

- bpo-27713: Suppress spurious build warnings when updating importlibâs bootstrap files. Patch by Xiang Zhang
- bpo-25825: Correct the references to Modules/python.exp, which is required on AIX. The references were accidentally changed in 3.5.0a1.
- bpo-27453: CPP invocation in configure must use CPPFLAGS. Patch by Chi Hsuan Yen.
- bpo-27641: The configure script now inserts comments into the makefile to prevent the pgen and _freeze_importlib executables from being cross-compiled.
- bpo-26662: Set PYTHON_FOR_GEN in configure as the Python program to be used for file generation during the build.
- bpo-10910: Avoid C++ compilation errors on FreeBSD and OS X. Also update FreedBSD version checks for the original ctype UTF-8 workaround.

## Python 3.6.0 alpha 3Â¶

*Release date: 2016-07-11*

### SecurityÂ¶

### Core and BuiltinsÂ¶

- bpo-27473: Fixed possible integer overflow in bytes and bytearray concatenations. Patch by Xiang Zhang.
- bpo-23034: The output of a special Python build with defined COUNT_ALLOCS, SHOW_ALLOC_COUNT or SHOW_TRACK_COUNT macros is now off by default. It can be re-enabled using the â-X showalloccountâ option. It now outputs to stderr instead of stdout.
- bpo-27443: __length_hint__() of bytearray iterators no longer return a negative integer for a resized bytearray.
- bpo-27007: The fromhex() class methods of bytes and bytearray subclasses now return an instance of corresponding subclass.

### LibraryÂ¶

- bpo-26844: Fix error message for imp.find_module() to refer to âpathâ instead of ânameâ. Patch by Lev Maximov.
- bpo-23804: Fix SSL zero-length recv() calls to not block and not raise an error about unclean EOF.
- bpo-27466: Change time format returned by http.cookie.time2netscape, confirming the netscape cookie format and making it consistent with documentation.
- bpo-21708: Deprecated dbm.dumb behavior that differs from common dbm behavior: creating a database in ârâ and âwâ modes and modifying a database in ârâ mode.
- bpo-26721: Change the socketserver.StreamRequestHandler.wfile attribute to implement BufferedIOBase. In particular, the write() method no longer does partial writes.
- bpo-22115: Added methods trace_add, trace_remove and trace_info in the tkinter.Variable class. They replace old methods trace_variable, trace, trace_vdelete and trace_vinfo that use obsolete Tcl commands and might not work in future versions of Tcl. Fixed old tracing methods: trace_vdelete() with wrong mode no longer break tracing, trace_vinfo() now always returns a list of pairs of strings, tracing in the âuâ mode now works.
- bpo-26243: Only the level argument to zlib.compress() is keyword argument now. The first argument is positional-only.
- bpo-27038: Expose the DirEntry type as os.DirEntry. Code patch by Jelle Zijlstra.
- bpo-27186: Update os.fspath()/PyOS_FSPath() to check the return value of __fspath__() to be either str or bytes.
- bpo-18726: All optional parameters of the dump(), dumps(), load() and loads() functions and JSONEncoder and JSONDecoder class constructors in the json module are now keyword-only.
- bpo-27319: Methods selection_set(), selection_add(), selection_remove() and selection_toggle() of ttk.TreeView now allow passing multiple items as multiple arguments instead of passing them as a tuple. Deprecated undocumented ability of calling the selection() method with arguments.
- bpo-27079: Fixed curses.ascii functions isblank(), iscntrl() and ispunct().
- bpo-27294: Numerical state in the repr for Tkinter event objects is now represented as a combination of known flags.
- bpo-27177: Match objects in the re module now support index-like objects as group indices. Based on patches by Jeroen Demeyer and Xiang Zhang.
- bpo-26754: Some functions (compile() etc) accepted a filename argument encoded as an iterable of integers. Now only strings and byte-like objects are accepted.
- bpo-26536: socket.ioctl now supports SIO_LOOPBACK_FAST_PATH. Patch by Daniel Stokes.
- bpo-27048: Prevents distutils failing on Windows when environment variables contain non-ASCII characters
- bpo-27330: Fixed possible leaks in the ctypes module.
- bpo-27238: Got rid of bare excepts in the turtle module. Original patch by Jelle Zijlstra.
- bpo-27122: When an exception is raised within the context being managed by a contextlib.ExitStack() and one of the exit stack generators catches and raises it in a chain, do not re-raise the original exception when exiting, let the new chained one through. This avoids the **PEP 479** bug described
in issue25782.
- bpo-16864: sqlite3.Cursor.lastrowid now supports REPLACE statement. Initial patch by Alex LordThorsen.
- bpo-26386: Fixed ttk.TreeView selection operations with item idâs containing spaces.
- bpo-8637: Honor a pager set by the env var MANPAGER (in preference to one set by the env var PAGER).
- bpo-16182: Fix various functions in the âreadlineâ module to use the locale encoding, and fix get_begidx() and get_endidx() to return code point indexes.
- bpo-27392: Add loop.connect_accepted_socket(). Patch by Jim Fulton.

### IDLEÂ¶

- bpo-27477: IDLE search dialogs now use ttk widgets.
- bpo-27173: Add âIDLE Modern Unixâ to the built-in key sets. Make the default key set depend on the platform. Add tests for the changes to the config module.
- bpo-27452: make command line âidle-test> python test_help.pyâ work. __file__ is relative when python is started in the fileâs directory.
- bpo-27452: add line counter and crc to IDLE configHandler test dump.
- bpo-27380: IDLE: add query.py with base Query dialog and ttk widgets. Module had subclasses SectionName, ModuleName, and HelpSource, which are used to get information from users by configdialog and file =>Load Module. Each subclass has itw own validity checks. Using ModuleName allows users to edit bad module names instead of starting over. Add tests and delete the two files combined into the new one.
- bpo-27372: Test_idle no longer changes the locale.
- bpo-27365: Allow non-ascii chars in IDLE NEWS.txt, for contributor names.
- bpo-27245: IDLE: Cleanly delete custom themes and key bindings. Previously, when IDLE was started from a console or by import, a cascade of warnings was emitted. Patch by Serhiy Storchaka.
- bpo-24137: Run IDLE, test_idle, and htest with tkinter default root disabled. Fix code and tests that fail with this restriction. Fix htests to not create a second and redundant root and mainloop.
- bpo-27310: Fix IDLE.app failure to launch on OS X due to vestigial import.

### C APIÂ¶

- bpo-26754: PyUnicode_FSDecoder() accepted a filename argument encoded as an iterable of integers. Now only strings and byte-like objects are accepted.

### BuildÂ¶

- bpo-28066: Fix the logic that searches build directories for generated include files when building outside the source tree.
- bpo-27442: Expose the Android API level that python was built against, in sysconfig.get_config_vars() as âANDROID_API_LEVELâ.
- bpo-27434: The interpreter that runs the cross-build, found in PATH, must now be of the same feature version (e.g. 3.6) as the source being built.
- bpo-26930: Update Windows builds to use OpenSSL 1.0.2h.
- bpo-23968: Rename the platform directory from plat-$(MACHDEP) to plat-$(PLATFORM_TRIPLET). Rename the config directory (LIBPL) from config-$(LDVERSION) to config-$(LDVERSION)-$(PLATFORM_TRIPLET). Install the platform specific _sysconfigdata module into the platform directory and rename it to include the ABIFLAGS.
- Donât use largefile support for GNU/Hurd.

### Tools/DemosÂ¶

### DocumentationÂ¶

### TestsÂ¶

- bpo-27027: Added test.support.is_android that is True when this is an Android build.

## Python 3.6.0 alpha 2Â¶

*Release date: 2016-06-13*

### SecurityÂ¶

- bpo-26556: Update expat to 2.1.1, fixes **CVE 2015-1283** .
- Fix TLS stripping vulnerability in smtplib, **CVE 2016-0772** . Reported by
Team Oststrom.
- bpo-26839: On Linux, `os.urandom()` now calls`getrandom()` with`GRND_NONBLOCK` to fall back on reading`/dev/urandom` if the urandom
entropy pool is not initialized yet. Patch written by Colm Buckley.

### Core and BuiltinsÂ¶

- bpo-27095: Simplified MAKE_FUNCTION and removed MAKE_CLOSURE opcodes. Patch by Demur Rumed.
- bpo-27190: Raise NotSupportedError if sqlite3 is older than 3.3.1. Patch by Dave Sawyer.
- bpo-27286: Fixed compiling BUILD_MAP_UNPACK_WITH_CALL opcode. Calling function with generalized unpacking (PEP 448) and conflicting keyword names could cause undefined behavior.
- bpo-27140: Added BUILD_CONST_KEY_MAP opcode.
- bpo-27186: Add support for os.PathLike objects to open() (part of **PEP 519** ).
- bpo-27066: Fixed SystemError if a custom opener (for open()) returns a negative number without setting an exception.
- bpo-26983: float() now always return an instance of exact float. The deprecation warning is emitted if __float__ returns an instance of a strict subclass of float. In a future versions of Python this can be an error.
- bpo-27097: Python interpreter is now about 7% faster due to optimized instruction decoding. Based on patch by Demur Rumed.
- bpo-26647: Python interpreter now uses 16-bit wordcode instead of bytecode. Patch by Demur Rumed.
- bpo-23275: Allow assigning to an empty target list in round brackets: () = iterable.
- bpo-27243: Update the __aiter__ protocol: instead of returning an awaitable that resolves to an asynchronous iterator, the asynchronous iterator should be returned directly. Doing the former will trigger a PendingDeprecationWarning.

### LibraryÂ¶

- Comment out socket (SO_REUSEPORT) and posix (O_SHLOCK, O_EXLOCK) constants exposed on the API which are not implemented on GNU/Hurd. They would not work at runtime anyway.
- bpo-27025: Generated names for Tkinter widgets are now more meaningful and recognizable.
- bpo-25455: Fixed crashes in repr of recursive ElementTree.Element and functools.partial objects.
- bpo-27294: Improved repr for Tkinter event objects.
- bpo-20508: Improve exception message of IPv{4,6}Network.__getitem__. Patch by Gareth Rees.
- bpo-21386: Implement missing IPv4Address.is_global property. It was documented since 07a5610bae9d. Initial patch by Roger Luethi.
- bpo-27029: Removed deprecated support of universal newlines mode from ZipFile.open().
- bpo-27030: Unknown escapes consisting of `'\'` and an ASCII letter in
regular expressions now are errors.  The re.LOCALE flag now can be used
only with bytes patterns.
- bpo-27186: Add os.PathLike support to DirEntry (part of **PEP 519** ).
Initial patch by Jelle Zijlstra.
- bpo-20900: distutils register command now decodes HTTP responses correctly. Initial patch by ingrid.
- bpo-27186: Add os.PathLike support to pathlib, removing its provisional status (part of PEP 519). Initial patch by Dusty Phillips.
- bpo-27186: Add support for os.PathLike objects to os.fsencode() and os.fsdecode() (part of **PEP 519** ).
- bpo-27186: Introduce os.PathLike and os.fspath() (part of **PEP 519** ).
- A new version of typing.py provides several new classes and features: @overload outside stubs, Reversible, DefaultDict, Text, ContextManager, Type[], NewType(), TYPE_CHECKING, and numerous bug fixes (note that some of the new features are not yet implemented in mypy or other static analyzers). Also classes for **PEP 492** (Awaitable, AsyncIterable,
AsyncIterator) have been added (in fact they made it into 3.5.1 but were
never mentioned).
- bpo-25738: Stop http.server.BaseHTTPRequestHandler.send_error() from sending a message body for 205 Reset Content. Also, donât send Content header fields in responses that donât have a body. Patch by Susumu Koshiba.
- bpo-21313: Fix the âplatformâ module to tolerate when sys.version contains truncated build information.
- bpo-23883: Added missing APIs to __all__ to match the documented APIs for the following modules: cgi, mailbox, mimetypes, plistlib and smtpd. Patches by Jacek KoÅodziej.
- bpo-27164: In the zlib module, allow decompressing raw Deflate streams with a predefined zdict. Based on patch by Xiang Zhang.
- bpo-24291: Fix wsgiref.simple_server.WSGIRequestHandler to completely write data to the client. Previously it could do partial writes and truncate data. Also, wsgiref.handler.ServerHandler can now handle stdout doing partial writes, but this is deprecated.
- bpo-21272: Use _sysconfigdata.py to initialize distutils.sysconfig.
- bpo-19611: `inspect` now reports the implicit`.0` parameters
generated by the compiler for comprehension and generator expression
scopes as if they were positional-only parameters called`implicit0` .
Patch by Jelle Zijlstra.
- bpo-26809: Add `__all__` to`string` .  Patch by Emanuel Barry.
- bpo-26373: subprocess.Popen.communicate now correctly ignores BrokenPipeError when the child process dies before .communicate() is called in more/all circumstances.
- signal, socket, and ssl module IntEnum constant name lookups now return a consistent name for values having multiple names. Ex: signal.Signals(6) now refers to itself as signal.SIGALRM rather than flipping between that and signal.SIGIOT based on the interpreterâs hash randomization seed.
- bpo-27167: Clarify the subprocess.CalledProcessError error message text when the child process died due to a signal.
- bpo-25931: Donât define socketserver.Forking* names on platforms such as Windows that do not support os.fork().
- bpo-21776: distutils.upload now correctly handles HTTPError. Initial patch by Claudiu Popa.
- bpo-26526: Replace custom parse tree validation in the parser module with a simple DFA validator.
- bpo-27114: Fix SSLContext._load_windows_store_certs fails with PermissionError
- bpo-18383: Avoid creating duplicate filters when using filterwarnings and simplefilter. Based on patch by Alex Shkop.
- bpo-23026: winreg.QueryValueEx() now return an integer for REG_QWORD type.
- bpo-26741: subprocess.Popen destructor now emits a ResourceWarning warning if the child process is still running.
- bpo-27056: Optimize pickle.load() and pickle.loads(), up to 10% faster to deserialize a lot of small objects.
- bpo-21271: New keyword only parameters in reset_mock call.

### IDLEÂ¶

- bpo-5124: Paste with text selected now replaces the selection on X11. This matches how paste works on Windows, Mac, most modern Linux apps, and ttk widgets. Original patch by Serhiy Storchaka.
- bpo-24750: Switch all scrollbars in IDLE to ttk versions. Where needed, minimal tests are added to cover changes.
- bpo-24759: IDLE requires tk 8.5 and availability ttk widgets. Delete now unneeded tk version tests and code for older versions. Add test for IDLE syntax colorizer.
- bpo-27239: idlelib.macosx.isXyzTk functions initialize as needed.
- bpo-27262: move Aqua unbinding code, which enable context menus, to macosx.
- bpo-24759: Make clear in idlelib.idle_test.__init__ that the directory is a private implementation of test.test_idle and tool for maintainers.
- bpo-27196: Stop âThemeChangedâ warnings when running IDLE tests. These persisted after other warnings were suppressed in #20567. Apply Serhiy Storchakaâs update_idletasks solution to four test files. Record this additional advice in idle_test/README.txt
- bpo-20567: Revise idle_test/README.txt with advice about avoiding tk warning messages from tests. Apply advice to several IDLE tests.
- bpo-24225: Update idlelib/README.txt with new file names and event handlers.
- bpo-27156: Remove obsolete code not used by IDLE.
- bpo-27117: Make colorizer htest and turtledemo work with dark themes. Move code for configuring text widget colors to a new function.
- bpo-24225: Rename many `idlelib/*.py` and`idle_test/test_*.py` files.
Edit files to replace old names with new names when the old name referred
to the module rather than the class it contained. See the issue and IDLE
section in Whatâs New in 3.6 for more.
- bpo-26673: When tk reports font size as 0, change to size 10. Such fonts on Linux prevented the configuration dialog from opening.
- bpo-21939: Add test for IDLEâs percolator. Original patch by Saimadhav Heblikar.
- bpo-21676: Add test for IDLEâs replace dialog. Original patch by Saimadhav Heblikar.
- bpo-18410: Add test for IDLEâs search dialog. Original patch by Westley MartÃnez.
- bpo-21703: Add test for undo delegator. Patch mostly by Saimadhav Heblikar .
- bpo-27044: Add ConfigDialog.remove_var_callbacks to stop memory leaks.
- bpo-23977: Add more asserts to test_delegator.

### DocumentationÂ¶

- bpo-16484: Change the default PYTHONDOCS URL to âhttps:â, and fix the resulting links to use lowercase. Patch by Sean Rodman, test by Kaushik Nadikuditi.
- bpo-24136: Document the new **PEP 448** unpacking syntax of 3.5.
- bpo-22558: Add remaining doc links to source code for Python-coded modules. Patch by Yoni Lavi.

### TestsÂ¶

### WindowsÂ¶

- bpo-27064: The py.exe launcher now defaults to Python 3. The Windows launcher `py.exe` no longer prefers an installed Python 2 version over
Python 3 by default when used interactively.
- bpo-17500: Remove unused and outdated icons. (See also: https://github.com/python/pythondotorg/issues/945)

### BuildÂ¶

### C APIÂ¶

### Tools/DemosÂ¶

- bpo-26282: Argument Clinic now supports positional-only and keyword parameters in the same function.

## Python 3.6.0 alpha 1Â¶

*Release date: 2016-05-16*

### SecurityÂ¶

- bpo-26657: Fix directory traversal vulnerability with http.server on Windows. This fixes a regression that was introduced in 3.3.4rc1 and 3.4.0rc1. Based on patch by Philipp Hagemeister.
- bpo-26313: ssl.py _load_windows_store_certs fails if windows cert store is empty. Patch by Baji.
- bpo-25939: On Windows open the cert store readonly in ssl.enum_certificates.

### Core and BuiltinsÂ¶

- bpo-20041: Fixed TypeError when frame.f_trace is set to None. Patch by Xavier de Gaye.
- bpo-26168: Fixed possible refleaks in failing Py_BuildValue() with the âNâ format unit.
- bpo-26991: Fix possible refleak when creating a function with annotations.
- bpo-27039: Fixed bytearray.remove() for values greater than 127. Based on patch by Joe Jevnik.
- bpo-23640: int.from_bytes() no longer bypasses constructors for subclasses.
- bpo-27005: Optimized the float.fromhex() class method for exact float. It is now 2 times faster.
- bpo-18531: Single var-keyword argument of dict subtype was passed unscathed to the C-defined function. Now it is converted to exact dict.
- bpo-26811: gc.get_objects() no longer contains a broken tuple with NULL pointer.
- bpo-20120: Use RawConfigParser for .pypirc parsing, removing support for interpolation unintentionally added with move to Python 3. Behavior no longer does any interpolation in .pypirc files, matching behavior in Python 2.7 and Setuptools 19.0.
- bpo-26249: Memory functions of the `PyMem_Malloc()` domain
(`PYMEM_DOMAIN_MEM` ) now use the pymalloc allocator rather than system`malloc()` . Applications calling`PyMem_Malloc()` without holding the GIL can now crash: use`PYTHONMALLOC=debug` environment variable to validate the usage of
memory allocators in your application.
- bpo-26802: Optimize function calls only using unpacking like `func(*tuple)` (no other positional argument, no keyword): avoid copying
the tuple. Patch written by Joe Jevnik.
- bpo-26659: Make the builtin slice type support cycle collection.
- bpo-26718: super.__init__ no longer leaks memory if called multiple times. NOTE: A direct call of super.__init__ is not endorsed!
- bpo-27138: Fix the doc comment for FileFinder.find_spec().
- bpo-27147: Mention **PEP 420** in the importlib docs.
- bpo-25339: PYTHONIOENCODING now has priority over locale in setting the error handler for stdin and stdout.
- bpo-26494: Fixed crash on iterating exhausting iterators. Affected classes are generic sequence iterators, iterators of str, bytes, bytearray, list, tuple, set, frozenset, dict, OrderedDict, corresponding views and os.scandir() iterator.
- bpo-26574: Optimize `bytes.replace(b'', b'.')` and`bytearray.replace(b'', b'.')` . Patch written by Josh Snider.
- bpo-26581: If coding cookie is specified multiple times on a line in Python source code file, only the first one is taken to account.
- bpo-19711: Add tests for reloading namespace packages.
- bpo-21099: Switch applicable importlib tests to use **PEP 451** API.
- bpo-26563: Debug hooks on Python memory allocators now raise a fatal error if functions of the `PyMem_Malloc()` family are called without
holding the GIL.
- bpo-26564: On error, the debug hooks on Python memory allocators now use the `tracemalloc` module to get the traceback where a memory block
was allocated.
- bpo-26558: The debug hooks on Python memory allocator `PyObject_Malloc()` now detect when functions are called without
holding the GIL.
- bpo-26516: Add `PYTHONMALLOC` environment variable to set the
Python memory allocators and/or install debug hooks.
- bpo-26516: The `PyMem_SetupDebugHooks()` function can now also be
used on Python compiled in release mode.
- bpo-26516: The `PYTHONMALLOCSTATS` environment variable can now
also be used on Python compiled in release mode. It now has no effect if
set to an empty string.
- bpo-26516: In debug mode, debug hooks are now also installed on Python memory allocators when Python is configured without pymalloc.
- bpo-26464: Fix str.translate() when string is ASCII and first replacements removes character, but next replacement uses a non-ASCII character or a string longer than 1 character. Regression introduced in Python 3.5.0.
- bpo-22836: Ensure exception reports from PyErr_Display() and PyErr_WriteUnraisable() are sensible even when formatting them produces secondary errors. This affects the reports produced by sys.__excepthook__() and when __del__() raises an exception.
- bpo-26302: Correct behavior to reject comma as a legal character for cookie names.
- bpo-26136: Upgrade the warning when a generator raises StopIteration from PendingDeprecationWarning to DeprecationWarning. Patch by Anish Shah.
- bpo-26204: The compiler now ignores all constant statements: bytes, str, int, float, complex, name constants (None, False, True), Ellipsis and ast.Constant; not only str and int. For example, `1.0` is now ignored in`def f(): 1.0` .
- bpo-4806: Avoid masking the original TypeError exception when using star ( `*` ) unpacking in function calls.  Based on patch by Hagen FÃ¼rstenau
and Daniel Urban.
- bpo-26146: Add a new kind of AST node: `ast.Constant` . It can be used by
external AST optimizers, but the compiler does not emit directly such
node.
- bpo-23601: Sped-up allocation of dict key objects by using Pythonâs small object allocator. (Contributed by Julian Taylor.)
- bpo-18018: Import raises ImportError instead of SystemError if a relative import is attempted without a known parent package.
- bpo-25843: When compiling code, donât merge constants if they are equal but have a different types. For example, ```
f1, f2 = lambda: 1, lambda:
1.0
```
is now correctly compiled to two different functions:`f1()` returns`1` (`int` ) and`f2()` returns`1.0` (`float` ), even if`1` and`1.0` are equal.
- bpo-26107: The format of the `co_lnotab` attribute of code objects
changes to support negative line number delta.
- bpo-26154: Add a new private _PyThreadState_UncheckedGet() function to get the current Python thread state, but donât issue a fatal error if it is NULL. This new function must be used instead of accessing directly the _PyThreadState_Current variable. The variable is no more exposed since Python 3.5.1 to hide the exact implementation of atomic C types, to avoid compiler issues.
- bpo-25791: If __package__ != __spec__.parent or if neither __package__ or __spec__ are defined then ImportWarning is raised.
- bpo-22995: [UPDATE] Comment out the one of the pickleability tests in _PyObject_GetState() due to regressions observed in Cython-based projects.
- bpo-25961: Disallowed null characters in the type name.
- bpo-25973: Fix segfault when an invalid nonlocal statement binds a name starting with two underscores.
- bpo-22995: Instances of extension types with a state that arenât subclasses of list or dict and havenât implemented any pickle-related methods (__reduce__, __reduce_ex__, __getnewargs__, __getnewargs_ex__, or __getstate__), can no longer be pickled. Including memoryview.
- bpo-20440: Massive replacing unsafe attribute setting code with special macro Py_SETREF.
- bpo-25766: Special method __bytes__() now works in str subclasses.
- bpo-25421: __sizeof__ methods of builtin types now use dynamic basic size. This allows sys.getsize() to work correctly with their subclasses with __slots__ defined.
- bpo-25709: Fixed problem with in-place string concatenation and utf-8 cache.
- bpo-5319: New Py_FinalizeEx() API allowing Python to set an exit status of 120 on failure to flush buffered streams.
- bpo-25485: telnetlib.Telnet is now a context manager.
- bpo-24097: Fixed crash in object.__reduce__() if slot name is freed inside __getattr__.
- bpo-24731: Fixed crash on converting objects with special methods __bytes__, __trunc__, and __float__ returning instances of subclasses of bytes, int, and float to subclasses of bytes, int, and float correspondingly.
- bpo-25630: Fix a possible segfault during argument parsing in functions that accept filesystem paths.
- bpo-23564: Fixed a partially broken sanity check in the _posixsubprocess internals regarding how fds_to_pass were passed to the child. The bug had no actual impact as subprocess.py already avoided it.
- bpo-25388: Fixed tokenizer crash when processing undecodable source code with a null byte.
- bpo-25462: The hash of the key now is calculated only once in most operations in C implementation of OrderedDict.
- bpo-22995: Default implementation of __reduce__ and __reduce_ex__ now rejects builtin types with not defined __new__.
- bpo-24802: Avoid buffer overreads when int(), float(), compile(), exec() and eval() are passed bytes-like objects. These objects are not necessarily terminated by a null byte, but the functions assumed they were.
- bpo-25555: Fix parser and AST: fill lineno and col_offset of âargâ node when compiling AST from Python objects.
- bpo-24726: Fixed a crash and leaking NULL in repr() of OrderedDict that was mutated by direct calls of dict methods.
- bpo-25449: Iterating OrderedDict with keys with unstable hash now raises KeyError in C implementations as well as in Python implementation.
- bpo-25395: Fixed crash when highly nested OrderedDict structures were garbage collected.
- bpo-25401: Optimize bytes.fromhex() and bytearray.fromhex(): they are now between 2x and 3.5x faster.
- bpo-25399: Optimize bytearray % args using the new private _PyBytesWriter API. Formatting is now between 2.5 and 5 times faster.
- bpo-25274: sys.setrecursionlimit() now raises a RecursionError if the new recursion limit is too low depending at the current recursion depth. Modify also the âlower-water markâ formula to make it monotonic. This mark is used to decide when the overflowed flag of the thread state is reset.
- bpo-24402: Fix input() to prompt to the redirected stdout when sys.stdout.fileno() fails.
- bpo-25349: Optimize bytes % args using the new private _PyBytesWriter API. Formatting is now up to 2 times faster.
- bpo-24806: Prevent builtin types that are not allowed to be subclassed from being subclassed through multiple inheritance.
- bpo-25301: The UTF-8 decoder is now up to 15 times as fast for error handlers: `ignore` ,`replace` and`surrogateescape` .
- bpo-24848: Fixed a number of bugs in UTF-7 decoding of misformed data.
- bpo-25267: The UTF-8 encoder is now up to 75 times as fast for error handlers: `ignore` ,`replace` ,`surrogateescape` ,`surrogatepass` .
Patch co-written with Serhiy Storchaka.
- bpo-25280: Import trace messages emitted in verbose (-v) mode are no longer formatted twice.
- bpo-25227: Optimize ASCII and latin1 encoders with the `surrogateescape` error handler: the encoders are now up to 3 times as fast. Initial patch
written by Serhiy Storchaka.
- bpo-25003: On Solaris 11.3 or newer, os.urandom() now uses the getrandom() function instead of the getentropy() function. The getentropy() function is blocking to generate very good quality entropy, os.urandom() doesnât need such high-quality entropy.
- bpo-9232: Modify Pythonâs grammar to allow trailing commas in the argument list of a function declaration. For example, âdef f(*, a = 3,): passâ is now legal. Patch from Mark Dickinson.
- bpo-24965: Implement **PEP 498** âLiteral String Interpolationâ. This
allows you to embed expressions inside f-strings, which are converted to
normal strings at run time. Given x=3, then fâvalue={x}â == âvalue=3â.
Patch by Eric V. Smith.
- bpo-26478: Fix semantic bugs when using binary operators with dictionary views and tuples.
- bpo-26171: Fix possible integer overflow and heap corruption in zipimporter.get_data().
- bpo-25660: Fix TAB key behaviour in REPL with readline.
- bpo-26288: Optimize PyLong_AsDouble.
- bpo-26289: Optimize floor and modulo division for single-digit longs. Microbenchmarks show 2-2.5x improvement. Built-in âdivmodâ function is now also ~10% faster. (See also: bpo-26315)
- bpo-25887: Raise a RuntimeError when a coroutine object is awaited more than once.

### LibraryÂ¶

- bpo-27057: Fix os.set_inheritable() on Android, ioctl() is blocked by SELinux and fails with EACCESS. The function now falls back to fcntl(). Patch written by MichaÅ Bednarski.
- bpo-27014: Fix infinite recursion using typing.py. Thanks to Kalle Tuure!
- bpo-27031: Removed dummy methods in Tkinter widget classes: tk_menuBar() and tk_bindForTraversal().
- bpo-14132: Fix urllib.request redirect handling when the target only has a query string. Original fix by JÃ¡n Janech.
- bpo-17214: The âurllib.requestâ module now percent-encodes non-ASCII bytes found in redirect target URLs. Some servers send Location header fields with non-ASCII bytes, but âhttp.clientâ requires the request target to be ASCII-encodable, otherwise a UnicodeEncodeError is raised. Based on patch by Christian Heimes.
- bpo-27033: The default value of the decode_data parameter for smtpd.SMTPChannel and smtpd.SMTPServer constructors is changed to False.
- bpo-27034: Removed deprecated class asynchat.fifo.
- bpo-26870: Added readline.set_auto_history(), which can stop entries being automatically added to the history list. Based on patch by Tyler Crompton.
- bpo-26039: zipfile.ZipFile.open() can now be used to write data into a ZIP file, as well as for extracting data. Patch by Thomas Kluyver.
- bpo-26892: Honor debuglevel flag in urllib.request.HTTPHandler. Patch contributed by Chi Hsuan Yen.
- bpo-22274: In the subprocess module, allow stderr to be redirected to stdout even when stdout is not redirected. Patch by Akira Li.
- bpo-26807: mock_open âfilesâ no longer error on readline at end of file. Patch from Yolanda Robla.
- bpo-25745: Fixed leaking a userptr in curses panel destructor.
- bpo-26977: Removed unnecessary, and ignored, call to sum of squares helper in statistics.pvariance.
- bpo-26002: Use bisect in statistics.median instead of a linear search. Patch by Upendra Kuma.
- bpo-25974: Make use of new Decimal.as_integer_ratio() method in statistics module. Patch by Stefan Krah.
- bpo-26996: Add secrets module as described in **PEP 506** .
- bpo-26881: The modulefinder module now supports extended opcode arguments.
- bpo-23815: Fixed crashes related to directly created instances of types in _tkinter and curses.panel modules.
- bpo-17765: weakref.ref() no longer silently ignores keyword arguments. Patch by Georg Brandl.
- bpo-26873: xmlrpc now raises ResponseError on unsupported type tags instead of silently return incorrect result.
- bpo-26915: The __contains__ methods in the collections ABCs now check for identity before checking equality. This better matches the behavior of the concrete classes, allows sensible handling of NaNs, and makes it easier to reason about container invariants.
- bpo-26711: Fixed the comparison of plistlib.Data with other types.
- bpo-24114: Fix an uninitialized variable in `ctypes.util` . The bug only
occurs on SunOS when the ctypes implementation searches for the`crle` program. Patch by Xiang Zhang.  Tested on SunOS by Kees Bos.
- bpo-26864: In urllib.request, change the proxy bypass host checking against no_proxy to be case-insensitive, and to not match unrelated host names that happen to have a bypassed hostname as a suffix. Patch by Xiang Zhang.
- bpo-24902: Print server URL on http.server startup. Initial patch by Felix Kaiser.
- bpo-25788: fileinput.hook_encoded() now supports an âerrorsâ argument for passing to open. Original patch by Joseph Hackman.
- bpo-26634: recursive_repr() now sets __qualname__ of wrapper. Patch by Xiang Zhang.
- bpo-26804: urllib.request will prefer lower_case proxy environment variables over UPPER_CASE or Mixed_Case ones. Patch contributed by Hans-Peter Jansen.
- bpo-26837: assertSequenceEqual() now correctly outputs non-stringified differing items (like bytes in the -b mode). This affects assertListEqual() and assertTupleEqual().
- bpo-26041: Remove âwill be removed in Python 3.7â from deprecation messages of platform.dist() and platform.linux_distribution(). Patch by Kumaripaba Miyurusara Athukorala.
- bpo-26822: itemgetter, attrgetter and methodcaller objects no longer silently ignore keyword arguments.
- bpo-26733: Disassembling a class now disassembles class and static methods. Patch by Xiang Zhang.
- bpo-26801: Fix error handling in `shutil.get_terminal_size()` , catch`AttributeError` instead of`NameError` . Patch written by
Emanuel Barry.
- bpo-24838: tarfileâs ustar and gnu formats now correctly calculate name and link field limits for multibyte character encodings like utf-8.
- bpo-26717: Stop encoding Latin-1-ized WSGI paths with UTF-8. Patch by Anthony Sottile.
- bpo-26782: Add STARTUPINFO to subprocess.__all__ on Windows.
- bpo-26404: Add context manager to socketserver. Patch by Aviv Palivoda.
- bpo-26735: Fix `os.urandom()` on Solaris 11.3 and newer when reading
more than 1,024 bytes: call`getrandom()` multiple times with a limit of
1024 bytes per call.
- bpo-26585: Eliminate http.server._quote_html() and use html.escape(quote=False). Patch by Xiang Zhang.
- bpo-26685: Raise OSError if closing a socket fails.
- bpo-16329: Add .webm to mimetypes.types_map. Patch by Giampaolo Rodolaâ.
- bpo-13952: Add .csv to mimetypes.types_map. Patch by Geoff Wilson.
- bpo-26587: the site module now allows .pth files to specify files to be added to sys.path (e.g. zip files).
- bpo-25609: Introduce contextlib.AbstractContextManager and typing.ContextManager.
- bpo-26709: Fixed Y2038 problem in loading binary PLists.
- bpo-23735: Handle terminal resizing with Readline 6.3+ by installing our own SIGWINCH handler. Patch by Eric Price.
- bpo-25951: Change SSLSocket.sendall() to return None, as explicitly documented for plain socket objects. Patch by Aviv Palivoda.
- bpo-26586: In http.server, respond with â413 Request header fields too largeâ if there are too many header fields to parse, rather than killing the connection and raising an unhandled exception. Patch by Xiang Zhang.
- bpo-26676: Added missing XMLPullParser to ElementTree.__all__.
- bpo-22854: Change BufferedReader.writable() and BufferedWriter.readable() to always return False.
- bpo-26492: Exhausted iterator of array.array now conforms with the behavior of iterators of other mutable sequences: it lefts exhausted even if iterated array is extended.
- bpo-26641: doctest.DocFileTest and doctest.testfile() now support packages (module split into multiple directories) for the package parameter.
- bpo-25195: Fix a regression in mock.MagicMock. _Call is a subclass of tuple (changeset 3603bae63c13 only works for classes) so we need to implement __ne__ ourselves. Patch by Andrew Plummer.
- bpo-26644: Raise ValueError rather than SystemError when a negative length is passed to SSLSocket.recv() or read().
- bpo-23804: Fix SSL recv(0) and read(0) methods to return zero bytes instead of up to 1024.
- bpo-26616: Fixed a bug in datetime.astimezone() method.
- bpo-26637: The `importlib` module now emits an`ImportError` rather than a`TypeError` if`__import__()` is tried during the
Python shutdown process but`sys.path` is already cleared (set to`None` ).
- bpo-21925: `warnings.formatwarning()` now catches exceptions when
calling`linecache.getline()` and`tracemalloc.get_object_traceback()` to be able to log`ResourceWarning` emitted late during the Python shutdown process.
- bpo-23848: On Windows, faulthandler.enable() now also installs an exception handler to dump the traceback of all Python threads on any Windows exception, not only on UNIX signals (SIGSEGV, SIGFPE, SIGABRT).
- bpo-26530: Add C functions `_PyTraceMalloc_Track()` and`_PyTraceMalloc_Untrack()` to track memory blocks using the`tracemalloc` module. Add`_PyTraceMalloc_GetTraceback()` to
get the traceback of an object.
- bpo-26588: The _tracemalloc now supports tracing memory allocations of multiple address spaces (domains).
- bpo-24266: Ctrl+C during Readline history search now cancels the search mode when compiled with Readline 7.
- bpo-26590: Implement a safe finalizer for the _socket.socket type. It now releases the GIL to close the socket.
- bpo-18787: spwd.getspnam() now raises a PermissionError if the user doesnât have privileges.
- bpo-26560: Avoid potential ValueError in BaseHandler.start_response. Initial patch by Peter Inglesby.
- bpo-26567: Add a new function `PyErr_ResourceWarning()` function to
pass the destroyed object. Add a*source* attribute to`warnings.WarningMessage` . Add warnings._showwarnmsg() which uses
tracemalloc to get the traceback where source object was allocated.
- bpo-26569: Fix `pyclbr.readmodule()` and`pyclbr.readmodule_ex()` to support importing packages.
- bpo-26499: Account for remaining Content-Length in HTTPResponse.readline() and read1(). Based on patch by Silent Ghost. Also document that HTTPResponse now supports these methods.
- bpo-25320: Handle sockets in directories unittest discovery is scanning. Patch from Victor van den Elzen.
- bpo-16181: cookiejar.http2time() now returns None if year is higher than datetime.MAXYEAR.
- bpo-26513: Fixes platform module detection of Windows Server
- bpo-23718: Fixed parsing time in week 0 before Jan 1. Original patch by TamÃ¡s Bence Gedai.
- bpo-26323: Add Mock.assert_called() and Mock.assert_called_once() methods to unittest.mock. Patch written by Amit Saha.
- bpo-20589: Invoking Path.owner() and Path.group() on Windows now raise NotImplementedError instead of ImportError.
- bpo-26177: Fixed the keys() method for Canvas and Scrollbar widgets.
- bpo-15068: Got rid of excessive buffering in fileinput. The bufsize parameter is now deprecated and ignored.
- bpo-19475: Added an optional argument timespec to the datetime isoformat() method to choose the precision of the time component.
- bpo-2202: Fix UnboundLocalError in AbstractDigestAuthHandler.get_algorithm_impls. Initial patch by Mathieu Dupuy.
- bpo-26167: Minimized overhead in copy.copy() and copy.deepcopy(). Optimized copying and deepcopying bytearrays, NotImplemented, slices, short lists, tuples, dicts, sets.
- bpo-25718: Fixed pickling and copying the accumulate() iterator with total is None.
- bpo-26475: Fixed debugging output for regular expressions with the (?x) flag.
- bpo-26482: Allowed pickling recursive dequeues.
- bpo-26335: Make mmap.write() return the number of bytes written like other write methods. Patch by Jakub Stasiak.
- bpo-26457: Fixed the subnets() methods in IP network classes for the case when resulting prefix length is equal to maximal prefix length. Based on patch by Xiang Zhang.
- bpo-26385: Remove the file if the internal open() call in NamedTemporaryFile() fails. Patch by Silent Ghost.
- bpo-26402: Fix XML-RPC client to retry when the server shuts down a persistent connection. This was a regression related to the new http.client.RemoteDisconnected exception in 3.5.0a4.
- bpo-25913: Leading `<~` is optional now in base64.a85decode() with
adobe=True.  Patch by Swati Jaiswal.
- bpo-26186: Remove an invalid type check in importlib.util.LazyLoader.
- bpo-26367: importlib.__import__() raises ImportError like builtins.__import__() when `level` is specified but without an
accompanying package specified.
- bpo-26309: In the âsocketserverâ module, shut down the request (closing the connected socket) when verify_request() returns false. Patch by Aviv Palivoda.
- bpo-23430: Change the socketserver module to only catch exceptions raised from a request handler that are derived from Exception (instead of BaseException). Therefore SystemExit and KeyboardInterrupt no longer trigger the handle_error() method, and will now to stop a single-threaded server.
- bpo-25995: os.walk() no longer uses FDs proportional to the tree depth.
- bpo-25994: Added the close() method and the support of the context manager protocol for the os.scandir() iterator.
- bpo-23992: multiprocessing: make MapResult not fail-fast upon exception.
- bpo-26243: Support keyword arguments to zlib.compress(). Patch by Aviv Palivoda.
- bpo-26117: The os.scandir() iterator now closes file descriptor not only when the iteration is finished, but when it was failed with error.
- bpo-25949: __dict__ for an OrderedDict instance is now created only when needed.
- bpo-25911: Restored support of bytes paths in os.walk() on Windows.
- bpo-26045: Add UTF-8 suggestion to error message when posting a non-Latin-1 string with http.client.
- bpo-26039: Added zipfile.ZipInfo.from_file() and zipinfo.ZipInfo.is_dir(). Patch by Thomas Kluyver.
- bpo-12923: Reset FancyURLopenerâs redirect counter even if there is an exception. Based on patches by Brian Brazil and Daniel Rocco.
- bpo-25945: Fixed a crash when unpickle the functools.partial object with wrong state. Fixed a leak in failed functools.partial constructor. âargsâ and âkeywordsâ attributes of functools.partial have now always types tuple and dict correspondingly.
- bpo-26202: copy.deepcopy() now correctly copies range() objects with non-atomic attributes.
- bpo-23076: Path.glob() now raises a ValueError if itâs called with an invalid pattern. Patch by Thomas Nyberg.
- bpo-19883: Fixed possible integer overflows in zipimport.
- bpo-26227: On Windows, getnameinfo(), gethostbyaddr() and gethostbyname_ex() functions of the socket module now decode the hostname from the ANSI code page rather than UTF-8.
- bpo-26099: The site module now writes an error into stderr if sitecustomize module can be imported but executing the module raise an ImportError. Same change for usercustomize.
- bpo-26147: xmlrpc now works with strings not encodable with used non-UTF-8 encoding.
- bpo-25935: Garbage collector now breaks reference loops with OrderedDict.
- bpo-16620: Fixed AttributeError in msilib.Directory.glob().
- bpo-26013: Added compatibility with broken protocol 2 pickles created in old Python 3 versions (3.4.3 and lower).
- bpo-26129: Deprecated accepting non-integers in grp.getgrgid().
- bpo-25850: Use cross-compilation by default for 64-bit Windows.
- bpo-25822: Add docstrings to the fields of urllib.parse results. Patch contributed by Swati Jaiswal.
- bpo-22642: Convert trace module option parsing mechanism to argparse. Patch contributed by SilentGhost.
- bpo-24705: Fix sysconfig._parse_makefile not expanding ${} vars appearing before $() vars.
- bpo-26069: Remove the deprecated apis in the trace module.
- bpo-22138: Fix mock.patch behavior when patching descriptors. Restore original values after patching. Patch contributed by Sean McCully.
- bpo-25672: In the ssl module, enable the SSL_MODE_RELEASE_BUFFERS mode option if it is safe to do so.
- bpo-26012: Donât traverse into symlinks for `**` pattern in
pathlib.Path.[r]glob().
- bpo-24120: Ignore PermissionError when traversing a tree with pathlib.Path.[r]glob(). Patch by Ulrich Petri.
- bpo-21815: Accept ] characters in the data portion of imap responses, in order to handle the flags with square brackets accepted and produced by servers such as gmail.
- bpo-25447: fileinput now uses sys.stdin as-is if it does not have a buffer attribute (restores backward compatibility).
- bpo-25971: Optimized creating Fractions from floats by 2 times and from Decimals by 3 times.
- bpo-25802: Document as deprecated the remaining implementations of importlib.abc.Loader.load_module().
- bpo-25928: Add Decimal.as_integer_ratio().
- bpo-25447: Copying the lru_cache() wrapper object now always works, independently from the type of the wrapped object (by returning the original object unchanged).
- bpo-25768: Have the functions in compileall return booleans instead of ints and add proper documentation and tests for the return values.
- bpo-24103: Fixed possible use after free in ElementTree.XMLPullParser.
- bpo-25860: os.fwalk() no longer skips remaining directories when error occurs. Original patch by Samson Lee.
- bpo-25914: Fixed and simplified OrderedDict.__sizeof__.
- bpo-25869: Optimized deepcopying ElementTree; it is now 20 times faster.
- bpo-25873: Optimized iterating ElementTree. Iterating elements Element.iter() is now 40% faster, iterating text Element.itertext() is now up to 2.5 times faster.
- bpo-25902: Fixed various refcount issues in ElementTree iteration.
- bpo-22227: The TarFile iterator is reimplemented using generator. This implementation is simpler that using class.
- bpo-25638: Optimized ElementTree.iterparse(); it is now 2x faster. Optimized ElementTree parsing; it is now 10% faster.
- bpo-25761: Improved detecting errors in broken pickle data.
- bpo-25717: Restore the previous behaviour of tolerating most fstat() errors when opening files. This was a regression in 3.5a1, and stopped anonymous temporary files from working in special cases.
- bpo-24903: Fix regression in number of arguments compileall accepts when â-dâ is specified. The check on the number of arguments has been dropped completely as it never worked correctly anyway.
- bpo-25764: In the subprocess module, preserve any exception caused by fork() failure when preexec_fn is used.
- bpo-25771: Tweak the exception message for importlib.util.resolve_name() when âpackageâ isnât specified but necessary.
- bpo-6478: _strptimeâs regexp cache now is reset after changing timezone with time.tzset().
- bpo-14285: When executing a package with the âpython -m packageâ option, and package initialization fails, a proper traceback is now reported. The ârunpyâ module now lets exceptions from package initialization pass back to the caller, rather than raising ImportError.
- bpo-19771: Also in runpy and the â-mâ option, omit the irrelevant message ââ¦ is a package and cannot be directly executedâ if the package could not even be initialized (e.g. due to a bad `*.pyc` file).
- bpo-25177: Fixed problem with the mean of very small and very large numbers. As a side effect, statistics.mean and statistics.variance should be significantly faster.
- bpo-25718: Fixed copying object with state with boolean value is false.
- bpo-10131: Fixed deep copying of minidom documents. Based on patch by Marian Ganisin.
- bpo-7990: dir() on ElementTree.Element now lists properties: âtagâ, âtextâ, âtailâ and âattribâ. Original patch by Santoso Wijaya.
- bpo-25725: Fixed a reference leak in pickle.loads() when unpickling invalid data including tuple instructions.
- bpo-25663: In the Readline completer, avoid listing duplicate global names, and search the global namespace before searching builtins.
- bpo-25688: Fixed file leak in ElementTree.iterparse() raising an error.
- bpo-23914: Fixed SystemError raised by unpickler on broken pickle data.
- bpo-25691: Fixed crash on deleting ElementTree.Element attributes.
- bpo-25624: ZipFile now always writes a ZIP_STORED header for directory entries. Patch by Dingyuan Wang.
- bpo-25626: Change three zlib functions to accept sizes that fit in Py_ssize_t, but internally cap those sizes to UINT_MAX. This resolves a regression in 3.5 where GzipFile.read() failed to read chunks larger than 2 or 4 GiB. The change affects the zlib.Decompress.decompress() max_length parameter, the zlib.decompress() bufsize parameter, and the zlib.Decompress.flush() length parameter.
- bpo-25583: Avoid incorrect errors raised by os.makedirs(exist_ok=True) when the OS gives priority to errors such as EACCES over EEXIST.
- bpo-25593: Change semantics of EventLoop.stop() in asyncio.
- bpo-6973: When we know a subprocess.Popen process has died, do not allow the send_signal(), terminate(), or kill() methods to do anything as they could potentially signal a different process.
- bpo-23883: Added missing APIs to __all__ to match the documented APIs for the following modules: calendar, csv, enum, fileinput, ftplib, logging, optparse, tarfile, threading and wave. Also added a test.support.check__all__() helper. Patches by Jacek KoÅodziej, Mauro S. M. Rodrigues and Joel Taddei.
- bpo-25590: In the Readline completer, only call getattr() once per attribute. Also complete names of attributes such as properties and slots which are listed by dir() but not yet created on an instance.
- bpo-25498: Fix a crash when garbage-collecting ctypes objects created by wrapping a memoryview. This was a regression made in 3.5a1. Based on patch by Eryksun.
- bpo-25584: Added âescapeâ to the __all__ list in the glob module.
- bpo-25584: Fixed recursive glob() with patterns starting with `**` .
- bpo-25446: Fix regression in smtplibâs AUTH LOGIN support.
- bpo-18010: Fix the pydoc web serverâs module search function to handle exceptions from importing packages.
- bpo-25554: Got rid of circular references in regular expression parsing.
- bpo-18973: Command-line interface of the calendar module now uses argparse instead of optparse.
- bpo-25510: fileinput.FileInput.readline() now returns bââ instead of ââ at the end if the FileInput was opened with binary mode. Patch by Ryosuke Ito.
- bpo-25503: Fixed inspect.getdoc() for inherited docstrings of properties. Original patch by John Mark Vandenberg.
- bpo-25515: Always use os.urandom as a source of randomness in uuid.uuid4.
- bpo-21827: Fixed textwrap.dedent() for the case when largest common whitespace is a substring of smallest leading whitespace. Based on patch by Robert Li.
- bpo-25447: The lru_cache() wrapper objects now can be copied and pickled (by returning the original object unchanged).
- bpo-25390: typing: Donât crash on Union[str, Pattern].
- bpo-25441: asyncio: Raise error from drain() when socket is closed.
- bpo-25410: Cleaned up and fixed minor bugs in C implementation of OrderedDict.
- bpo-25411: Improved Unicode support in SMTPHandler through better use of the email package. Thanks to user simon04 for the patch.
- Move the imp module from a PendingDeprecationWarning to DeprecationWarning.
- bpo-25407: Remove mentions of the formatter module being removed in Python 3.6.
- bpo-25406: Fixed a bug in C implementation of OrderedDict.move_to_end() that caused segmentation fault or hang in iterating after moving several items to the start of ordered dict.
- bpo-25382: pickletools.dis() now outputs implicit memo index for the MEMOIZE opcode.
- bpo-25357: Add an optional newline parameter to binascii.b2a_base64(). base64.b64encode() uses it to avoid a memory copy.
- bpo-24164: Objects that need calling `__new__` with keyword arguments,
can now be pickled using pickle protocols older than protocol version 4.
- bpo-25364: zipfile now works in threads disabled builds.
- bpo-25328: smtpdâs SMTPChannel now correctly raises a ValueError if both decode_data and enable_SMTPUTF8 are set to true.
- bpo-16099: RobotFileParser now supports Crawl-delay and Request-rate extensions. Patch by Nikolay Bogoychev.
- bpo-25316: distutils raises OSError instead of DistutilsPlatformError when MSVC is not installed.
- bpo-25380: Fixed protocol for the STACK_GLOBAL opcode in pickletools.opcodes.
- bpo-23972: Updates asyncio datagram create method allowing reuseport and reuseaddr socket options to be set prior to binding the socket. Mirroring the existing asyncio create_server method the reuseaddr option for datagram sockets defaults to True if the O/S is âposixâ (except if the platform is Cygwin). Patch by Chris Laws.
- bpo-25304: Add asyncio.run_coroutine_threadsafe(). This lets you submit a coroutine to a loop from another thread, returning a concurrent.futures.Future. By Vincent Michel.
- bpo-25232: Fix CGIRequestHandler to split the query from the URL at the first question mark (?) rather than the last. Patch from Xiang Zhang.
- bpo-24657: Prevent CGIRequestHandler from collapsing slashes in the query part of the URL as if it were a path. Patch from Xiang Zhang.
- bpo-25287: Donât add crypt.METHOD_CRYPT to crypt.methods if itâs not supported. Check if it is supported, it may not be supported on OpenBSD for example.
- bpo-23600: Default implementation of tzinfo.fromutc() was returning wrong results in some cases.
- bpo-25203: Failed readline.set_completer_delims() no longer left the module in inconsistent state.
- bpo-25011: rlcompleter now omits private and special attribute names unless the prefix starts with underscores.
- bpo-25209: rlcompleter now can add a space or a colon after completed keyword.
- bpo-22241: timezone.utc name is now plain âUTCâ, not âUTC-00:00â.
- bpo-23517: fromtimestamp() and utcfromtimestamp() methods of datetime.datetime now round microseconds to nearest with ties going to nearest even integer (ROUND_HALF_EVEN), as round(float), instead of rounding towards -Infinity (ROUND_FLOOR).
- bpo-23552: Timeit now warns when there is substantial (4x) variance between best and worst times. Patch from Serhiy Storchaka.
- bpo-24633: site-packages/README -> README.txt.
- bpo-24879: help() and pydoc can now list named tuple fields in the order they were defined rather than alphabetically. The ordering is determined by the _fields attribute if present.
- bpo-24874: Improve speed of itertools.cycle() and make its pickle more compact.
- Fix crash in itertools.cycle.__setstate__() when the first argument wasnât a list.
- bpo-20059: urllib.parse raises ValueError on all invalid ports. Patch by Martin Panter.
- bpo-24360: Improve __repr__ of argparse.Namespace() for invalid identifiers. Patch by Matthias Bussonnier.
- bpo-23426: run_setup was broken in distutils. Patch from Alexander Belopolsky.
- bpo-13938: 2to3 converts StringTypes to a tuple. Patch from Mark Hammond.
- bpo-2091: open() accepted a âUâ mode string containing â+â, but âUâ can only be used with ârâ. Patch from Jeff Balogh and John OâConnor.
- bpo-8585: improved tests for zipimporter2. Patch from Mark Lawrence.
- bpo-18622: unittest.mock.mock_open().reset_mock would recurse infinitely. Patch from Nicola Palumbo and Laurent De Buyst.
- bpo-24426: Fast searching optimization in regular expressions now works for patterns that starts with capturing groups. Fast searching optimization now canât be disabled at compile time.
- bpo-23661: unittest.mock side_effects can now be exceptions again. This was a regression vs Python 3.4. Patch from Ignacio Rossi
- bpo-13248: Remove deprecated inspect.getmoduleinfo function.
- bpo-25578: Fix (another) memory leak in SSLSocket.getpeercer().
- bpo-25530: Disable the vulnerable SSLv3 protocol by default when creating ssl.SSLContext.
- bpo-25569: Fix memory leak in SSLSocket.getpeercert().
- bpo-25471: Sockets returned from accept() shouldnât appear to be nonblocking.
- bpo-25319: When threading.Event is reinitialized, the underlying condition should use a regular lock rather than a recursive lock.
- Skip getaddrinfo if host is already resolved. Patch by A. Jesse Jiryu Davis.
- bpo-26050: Add asyncio.StreamReader.readuntil() method. Patch by ÐÐ°ÑÐº ÐÐ¾ÑÐµÐ½Ð±ÐµÑÐ³.
- bpo-25924: Avoid unnecessary serialization of getaddrinfo(3) calls on OS X versions 10.5 or higher. Original patch by A. Jesse Jiryu Davis.
- bpo-26406: Avoid unnecessary serialization of getaddrinfo(3) calls on current versions of OpenBSD and NetBSD. Patch by A. Jesse Jiryu Davis.
- bpo-26848: Fix asyncio/subprocess.communicate() to handle empty input. Patch by Jack OâConnor.
- bpo-27040: Add loop.get_exception_handler method
- bpo-27041: asyncio: Add loop.create_future method

### IDLEÂ¶

- bpo-20640: Add tests for idlelib.configHelpSourceEdit. Patch by Saimadhav Heblikar.
- In the âIDLE-console differencesâ section of the IDLE doc, clarify how running with IDLE affects sys.modules and the standard streams.
- bpo-25507: fix incorrect change in IOBinding that prevented printing. Augment IOBinding htest to include all major IOBinding functions.
- bpo-25905: Revert unwanted conversion of â to â RIGHT SINGLE QUOTATION MARK in README.txt and open this and NEWS.txt with âasciiâ. Re-encode CREDITS.txt to utf-8 and open it with âutf-8â.
- bpo-15348: Stop the debugger engine (normally in a user process) before closing the debugger window (running in the IDLE process). This prevents the RuntimeErrors that were being caught and ignored.
- bpo-24455: Prevent IDLE from hanging when a) closing the shell while the debugger is active (15347); b) closing the debugger with the [X] button (15348); and c) activating the debugger when already active (24455). The patch by Mark Roseman does this by making two changes. 1. Suspend and resume the gui.interaction method with the tcl vwait mechanism intended for this purpose (instead of root.mainloop & .quit). 2. In gui.run, allow any existing interaction to terminate first.
- Change âThe programâ to âYour programâ in an IDLE âkill program?â message to make it clearer that the program referred to is the currently running user program, not IDLE itself.
- bpo-24750: Improve the appearance of the IDLE editor window status bar. Patch by Mark Roseman.
- bpo-25313: Change the handling of new built-in text color themes to better address the compatibility problem introduced by the addition of IDLE Dark. Consistently use the revised idleConf.CurrentTheme everywhere in idlelib.
- bpo-24782: Extension configuration is now a tab in the IDLE Preferences dialog rather than a separate dialog. The former tabs are now a sorted list. Patch by Mark Roseman.
- bpo-22726: Re-activate the config dialog help button with some content about the other buttons and the new IDLE Dark theme.
- bpo-24820: IDLE now has an âIDLE Darkâ built-in text color theme. It is more or less IDLE Classic inverted, with a cobalt blue background. Strings, comments, keywords, â¦ are still green, red, orange, â¦ . To use it with IDLEs released before November 2015, hit the âSave as New Custom Themeâ button and enter a new name, such as âCustom Darkâ. The custom theme will work with any IDLE release, and can be modified.
- bpo-25224: README.txt is now an idlelib index for IDLE developers and curious users. The previous user content is now in the IDLE doc chapter. âIDLEâ now means âIntegrated Development and Learning Environmentâ.
- bpo-24820: Users can now set breakpoint colors in Settings -> Custom Highlighting. Original patch by Mark Roseman.
- bpo-24972: Inactive selection background now matches active selection background, as configured by users, on all systems. Found items are now always highlighted on Windows. Initial patch by Mark Roseman.
- bpo-24570: Idle: make calltip and completion boxes appear on Macs affected by a tk regression. Initial patch by Mark Roseman.
- bpo-24988: Idle ScrolledList context menus (used in debugger) now work on Mac Aqua. Patch by Mark Roseman.
- bpo-24801: Make right-click for context menu work on Mac Aqua. Patch by Mark Roseman.
- bpo-25173: Associate tkinter messageboxes with a specific widget. For Mac OSX, make them a âsheetâ. Patch by Mark Roseman.
- bpo-25198: Enhance the initial html viewer now used for Idle Help. Properly indent fixed-pitch text (patch by Mark Roseman). Give code snippet a very Sphinx-like light blueish-gray background. Re-use initial width and height set by users for shell and editor. When the Table of Contents (TOC) menu is used, put the section header at the top of the screen.
- bpo-25225: Condense and rewrite Idle doc section on text colors.
- bpo-21995: Explain some differences between IDLE and console Python.
- bpo-22820: Explain need for *print* when running file from Idle editor.
- bpo-25224: Doc: augment Idle feature list and no-subprocess section.
- bpo-25219: Update doc for Idle command line options. Some were missing and notes were not correct.
- bpo-24861: Most of idlelib is private and subject to change. Use idleib.idle.* to start Idle. See idlelib.__init__.__doc__.
- bpo-25199: Idle: add synchronization comments for future maintainers.
- bpo-16893: Replace help.txt with help.html for Idle doc display. The new idlelib/help.html is rstripped Doc/build/html/library/idle.html. It looks better than help.txt and will better document Idle as released. The tkinter html viewer that works for this file was written by Rose Roseman. The now unused EditorWindow.HelpDialog class and helt.txt file are deprecated.
- bpo-24199: Deprecate unused idlelib.idlever with possible removal in 3.6.
- bpo-24790: Remove extraneous code (which also create 2 & 3 conflicts).

### DocumentationÂ¶

- bpo-26736: Used HTTPS for external links in the documentation if possible.
- bpo-6953: Rework the Readline module documentation to group related functions together, and add more details such as what underlying Readline functions and variables are accessed.
- bpo-23606: Adds note to ctypes documentation regarding cdll.msvcrt.
- bpo-24952: Clarify the default size argument of stack_size() in the âthreadingâ and â_threadâ modules. Patch from Mattip.
- bpo-26014: Update 3.x packaging documentation: * âSee alsoâ links to the new docs are now provided in the legacy pages * links to setuptools documentation have been updated

### TestsÂ¶

- bpo-21916: Added tests for the turtle module. Patch by ingrid, Gregory Loyse and Jelle Zijlstra.
- bpo-26295: When using âpython3 -m test âtestdir=TESTDIRâ, regrtest doesnât add âtest.â prefix to test module names.
- bpo-26523: The multiprocessing thread pool (multiprocessing.dummy.Pool) was untested.
- bpo-26015: Added new tests for pickling iterators of mutable sequences.
- bpo-26325: Added test.support.check_no_resource_warning() to check that no ResourceWarning is emitted.
- bpo-25940: Changed test_ssl to use its internal local server more. This avoids relying on svn.python.org, which recently changed root certificate.
- bpo-25616: Tests for OrderedDict are extracted from test_collections into separate file test_ordered_dict.
- bpo-25449: Added tests for OrderedDict subclasses.
- bpo-25188: Add -P/âpgo to test.regrtest to suppress error output when running the test suite for the purposes of a PGO build. Initial patch by Alecsandru Patrascu.
- bpo-22806: Add `python -m test --list-tests` command to list tests.
- bpo-18174: `python -m test --huntrleaks ...` now also checks for leak of
file descriptors. Patch written by Richard Oudkerk.
- bpo-25260: Fix `python -m test --coverage` on Windows. Remove the list
of ignored directories.
- `PCbuild\rt.bat` now accepts an unlimited number of arguments to pass
along to regrtest.py.  Previously there was a limit of 9.
- bpo-26583: Skip test_timestamp_overflow in test_import if bytecode files cannot be written.

### BuildÂ¶

- bpo-21277: Donât try to link _ctypes with a ffi_convenience library.
- bpo-26884: Fix linking extension modules for cross builds. Patch by Xavier de Gaye.
- bpo-26932: Fixed support of RTLD_* constants defined as enum values, not via macros (in particular on Android). Patch by Chi Hsuan Yen.
- bpo-22359: Disable the rules for running _freeze_importlib and pgen when cross-compiling. The output of these programs is normally saved with the source code anyway, and is still regenerated when doing a native build. Patch by Xavier de Gaye.
- bpo-21668: Link audioop, _datetime, _ctypes_test modules to libm, except on Mac OS X. Patch written by Chi Hsuan Yen.
- bpo-25702: A âwith-lto configure option has been added that will enable link time optimizations at build time during a make profile-opt. Some compilers and toolchains are known to not produce stable code when using LTO, be sure to test things thoroughly before relying on it. It can provide a few % speed up over profile-opt alone.
- bpo-26624: Adds validation of ucrtbase[d].dll version with warning for old versions.
- bpo-17603: Avoid error about nonexistent fileblocks.o file by using a lower-level check for st_blocks in struct stat.
- bpo-26079: Fixing the build output folder for tix-8.4.3.6. Patch by Bjoern Thiel.
- bpo-26465: Update Windows builds to use OpenSSL 1.0.2g.
- bpo-25348: Added `--pgo` and`--pgo-job` arguments to`PCbuild\build.bat` for building with Profile-Guided Optimization.  The
old`PCbuild\build_pgo.bat` script is removed.
- bpo-25827: Add support for building with ICC to `configure` , including a
new`--with-icc` flag.
- bpo-25696: Fix installation of Python on UNIX with make -j9.
- bpo-24986: It is now possible to build Python on Windows without errors when external libraries are not available.
- bpo-24421: Compile Modules/_math.c once, before building extensions. Previously it could fail to compile properly if the math and cmath builds were concurrent.
- bpo-26465: Update OS X 10.5+ 32-bit-only installer to build and link with OpenSSL 1.0.2g.
- bpo-26268: Update Windows builds to use OpenSSL 1.0.2f.
- bpo-25136: Support Apple Xcode 7âs new textual SDK stub libraries.
- bpo-24324: Do not enable unreachable code warnings when using gcc as the option does not work correctly in older versions of gcc and has been silently removed as of gcc-4.5.

### WindowsÂ¶

- bpo-27053: Updates make_zip.py to correctly generate library ZIP file.
- bpo-26268: Update the prepare_ssl.py script to handle OpenSSL releases that donât include the contents of the include directory (that is, 1.0.2e and later).
- bpo-26071: bdist_wininst created binaries fail to start and find 32bit Python
- bpo-26073: Update the list of magic numbers in launcher
- bpo-26065: Excludes venv from library when generating embeddable distro.
- bpo-25022: Removed very outdated PC/example_nt/ directory.

### Tools/DemosÂ¶

- bpo-26799: Fix python-gdb.py: donât get C types once when the Python code is loaded, but get C types on demand. The C types can change if python-gdb.py is loaded before the Python executable. Patch written by Thomas Ilsche.
- bpo-26271: Fix the Freeze tool to properly use flags passed through configure. Patch by Daniel Shaulov.
- bpo-26489: Add dictionary unpacking support to Tools/parser/unparse.py. Patch by Guo Ci Teo.
- bpo-26316: Fix variable name typo in Argument Clinic.
- bpo-25440: Fix output of python-config âextension-suffix.
- bpo-25154: The pyvenv script has been deprecated in favour of ```
python3 -m
venv
```
.

### C APIÂ¶

- bpo-26312: SystemError is now raised in all programming bugs with using PyArg_ParseTupleAndKeywords(). RuntimeError did raised before in some programming bugs.
- bpo-26198: ValueError is now raised instead of TypeError on buffer overflow in parsing âes#â and âet#â format units. SystemError is now raised instead of TypeError on programmatical error in parsing format string.

## Python 3.5.5 finalÂ¶

*Release date: 2018-02-04*

There were no new changes in version 3.5.5.

## Python 3.5.5 release candidate 1Â¶

*Release date: 2018-01-23*

### SecurityÂ¶

- bpo-32551: The `sys.path[0]` initialization change for bpo-29139 caused
a regression by revealing an inconsistency in how sys.path is initialized
when executing`__main__` from a zipfile, directory, or other import
location. This is considered a potential security issue, as it may lead to
privileged processes unexpectedly loading code from user controlled
directories in situations where that was not previously the case. The
interpreter now consistently avoids ever adding the import locationâs
parent directory to`sys.path` , and ensures no other`sys.path` entries are inadvertently modified when inserting the import location
named on the command line. (Originally reported as bpo-29723 against
Python 3.6rc1, but it was missed at the time that the then upcoming Python
3.5.4 release would also be affected)
- bpo-30657: Fixed possible integer overflow in PyBytes_DecodeEscape, **CVE 2017-1000158** . Original patch by Jay Bosamiya; rebased to Python 3
by Miro HronÄok.
- bpo-30947: Upgrade libexpat embedded copy from version 2.2.1 to 2.2.3 to get security fixes.

### Core and BuiltinsÂ¶

- bpo-31095: Fix potential crash during GC caused by `tp_dealloc` which
doesnât call`PyObject_GC_UnTrack()` .

### LibraryÂ¶

- bpo-32072: Fixed issues with binary plists: Fixed saving bytearrays. Identical objects will be saved only once. Equal references will be load as identical objects. Added support for saving and loading recursive data structures.
- bpo-31170: expat: Update libexpat from 2.2.3 to 2.2.4. Fix copying of partial characters for UTF-8 input (libexpat bug 115): https://github.com/libexpat/libexpat/issues/115

## Python 3.5.4 finalÂ¶

*Release date: 2017-08-07*

### LibraryÂ¶

- bpo-30119: ftplib.FTP.putline() now throws ValueError on commands that contains CR or LF. Patch by Donghee Na.

## Python 3.5.4 release candidate 1Â¶

*Release date: 2017-07-23*

### SecurityÂ¶

- bpo-30730: Prevent environment variables injection in subprocess on Windows. Prevent passing other environment variables and command arguments.
- bpo-30694: Upgrade expat copy from 2.2.0 to 2.2.1 to get fixes of multiple security vulnerabilities including: **CVE 2017-9233** (External entity
infinite loop DoS),**CVE 2016-9063** (Integer overflow, re-fix),**CVE 2016-0718** (Fix regression bugs from 2.2.0âs fix to**CVE 2016-0718** ) and**CVE 2012-0876** (Counter hash flooding with
SipHash). Note: the**CVE 2016-5300** (Use os-specific entropy sources like
getrandom) doesnât impact Python, since Python already gets entropy from
the OS to set the expat secret using`XML_SetHashSalt()` .
- bpo-30500: Fix urllib.parse.splithost() to correctly parse fragments. For example, `splithost('//127.0.0.1#@evil.com/')` now correctly returns the`127.0.0.1` host, instead of treating`@evil.com` as the host in an
authentication (`login@host` ).
- bpo-29591: Update expat copy from 2.1.1 to 2.2.0 to get fixes of **CVE 2016-0718** and**CVE 2016-4472** . See
https://sourceforge.net/p/expat/bugs/537/ for more information.

### Core and BuiltinsÂ¶

- bpo-30876: Relative import from unloaded package now reimports the package instead of failing with SystemError. Relative import from non-package now fails with ImportError rather than SystemError.
- bpo-30765: Avoid blocking in pthread_mutex_lock() when PyThread_acquire_lock() is asked not to block.
- bpo-27945: Fixed various segfaults with dict when input collections are mutated during searching, inserting or comparing. Based on patches by Duane Griffin and Tim Mitchell.
- bpo-25794: Fixed type.__setattr__() and type.__delattr__() for non-interned attribute names. Based on patch by Eryk Sun.
- bpo-29935: Fixed error messages in the index() method of tuple, list and deque when pass indices of wrong type.
- bpo-28876: `bool(range)` works even if`len(range)` raises`OverflowError` .
- bpo-29600: Fix wrapping coroutine return values in StopIteration.
- bpo-29537: Restore runtime compatibility with bytecode files generated by CPython 3.5.0 to 3.5.2, and adjust the eval loop to avoid the problems that could be caused by the malformed variant of the BUILD_MAP_UNPACK_WITH_CALL opcode that they may contain. Patch by Petr Viktorin, Serhiy Storchaka, and Nick Coghlan.
- bpo-28598: Support __rmod__ for subclasses of str being called before str.__mod__. Patch by Martijn Pieters.
- bpo-29602: Fix incorrect handling of signed zeros in complex constructor for complex subclasses and for inputs having a __complex__ method. Patch by Serhiy Storchaka.
- bpo-29347: Fixed possibly dereferencing undefined pointers when creating weakref objects.
- bpo-29438: Fixed use-after-free problem in key sharing dict.
- bpo-29319: Prevent RunMainFromImporter overwriting sys.path[0].
- bpo-29337: Fixed possible BytesWarning when compare the code objects. Warnings could be emitted at compile time.
- bpo-29478: If max_line_length=None is specified while using the Compat32 policy, it is no longer ignored. Patch by Mircea Cosbuc.

### LibraryÂ¶

- bpo-29403: Fix `unittest.mock` âs autospec to not fail on method-bound
builtin functions.  Patch by Aaron Gallagher.
- bpo-30961: Fix decrementing a borrowed reference in tracemalloc.
- bpo-30886: Fix multiprocessing.Queue.join_thread(): it now waits until the thread completes, even if the thread was started by the same process which created the queue.
- bpo-29854: Fix segfault in readline when using readlineâs history-size option. Patch by Nir Soffer.
- bpo-30807: signal.setitimer() may disable the timer when passed a tiny value. Tiny values (such as 1e-6) are valid non-zero values for setitimer(), which is specified as taking microsecond-resolution intervals. However, on some platform, our conversion routine could convert 1e-6 into a zero interval, therefore disabling the timer instead of (re-)scheduling it.
- bpo-30441: Fix bug when modifying os.environ while iterating over it
- bpo-30532: Fix email header value parser dropping folding white space in certain cases.
- bpo-29169: Update zlib to 1.2.11.
- bpo-30879: os.listdir() and os.scandir() now emit bytes names when called with bytes-like argument.
- bpo-30746: Prohibited the â=â character in environment variable names in `os.putenv()` and`os.spawn*()` .
- bpo-29755: Fixed the lgettext() family of functions in the gettext module. They now always return bytes.
- bpo-30645: Fix path calculation in imp.load_package(), fixing it for cases when a package is only shipped with bytecodes. Patch by Alexandru Ardelean.
- bpo-23890: unittest.TestCase.assertRaises() now manually breaks a reference cycle to not keep objects alive longer than expected.
- bpo-30149: inspect.signature() now supports callables with variable-argument parameters wrapped with partialmethod. Patch by Donghee Na.
- bpo-29931: Fixed comparison check for ipaddress.ip_interface objects. Patch by Sanjay Sundaresan.
- bpo-24484: Avoid race condition in multiprocessing cleanup.
- bpo-28994: The traceback no longer displayed for SystemExit raised in a callback registered by atexit.
- bpo-30508: Donât log exceptions if Task/Future âcancel()â method was called.
- bpo-28556: Updates to typing module: Add generic AsyncContextManager, add support for ContextManager on all versions. Original PRs by Jelle Zijlstra and Ivan Levkivskyi
- bpo-29870: Fix ssl sockets leaks when connection is aborted in asyncio/ssl implementation. Patch by MichaÃ«l SghaÃ¯er.
- bpo-29743: Closing transport during handshake process leaks open socket. Patch by Nikolay Kim
- bpo-27585: Fix waiter cancellation in asyncio.Lock. Patch by Mathieu Sornay.
- bpo-30418: On Windows, subprocess.Popen.communicate() now also ignore EINVAL on stdin.write() if the child process is still running but closed the pipe.
- bpo-30378: Fix the problem that logging.handlers.SysLogHandler cannot handle IPv6 addresses.
- bpo-29960: Preserve generator state when _random.Random.setstate() raises an exception. Patch by Bryan Olson.
- bpo-30414: multiprocessing.Queue._feed background running thread do not break from main loop on exception.
- bpo-30003: Fix handling escape characters in HZ codec. Based on patch by Ma Lin.
- bpo-30301: Fix AttributeError when using SimpleQueue.empty() under *spawn* and*forkserver* start methods.
- bpo-30329: imaplib and poplib now catch the Windows socket WSAEINVAL error (code 10022) on shutdown(SHUT_RDWR): An invalid operation was attempted. This error occurs sometimes on SSL connections.
- bpo-30375: Warnings emitted when compile a regular expression now always point to the line in the user code. Previously they could point into inners of the re module if emitted from inside of groups or conditionals.
- bpo-30048: Fixed `Task.cancel()` can be ignored when the task is running
coroutine and the coroutine returned without any more`await` .
- bpo-29990: Fix range checking in GB18030 decoder. Original patch by Ma Lin.
- bpo-26293: Change resulted because of zipfile breakage. (See also: bpo-29094)
- bpo-30243: Removed the __init__ methods of _jsonâs scanner and encoder. Misusing them could cause memory leaks or crashes. Now scanner and encoder objects are completely initialized in the __new__ methods.
- bpo-30185: Avoid KeyboardInterrupt tracebacks in forkserver helper process when Ctrl-C is received.
- bpo-28556: Various updates to typing module: add typing.NoReturn type, use WrapperDescriptorType, minor bug-fixes. Original PRs by Jim Fasarakis-Hilliard and Ivan Levkivskyi.
- bpo-30205: Fix getsockname() for unbound AF_UNIX sockets on Linux.
- bpo-30070: Fixed leaks and crashes in errors handling in the parser module.
- bpo-30061: Fixed crashes in IOBase methods __next__() and readlines() when readline() or __next__() respectively return non-sizeable object. Fixed possible other errors caused by not checking results of PyObject_Size(), PySequence_Size(), or PyMapping_Size().
- bpo-30068: _io._IOBase.readlines will check if itâs closed first when hint is present.
- bpo-29694: Fixed race condition in pathlib mkdir with flags parents=True. Patch by Armin Rigo.
- bpo-29692: Fixed arbitrary unchaining of RuntimeError exceptions in contextlib.contextmanager. Patch by Siddharth Velankar.
- bpo-29998: Pickling and copying ImportError now preserves name and path attributes.
- bpo-29942: Fix a crash in itertools.chain.from_iterable when encountering long runs of empty iterables.
- bpo-27863: Fixed multiple crashes in ElementTree caused by race conditions and wrong types.
- bpo-28699: Fixed a bug in pools in multiprocessing.pool that raising an exception at the very first of an iterable may swallow the exception or make the program hang. Patch by Davin Potts and Xiang Zhang.
- bpo-25803: Avoid incorrect errors raised by Path.mkdir(exist_ok=True) when the OS gives priority to errors such as EACCES over EEXIST.
- bpo-29861: Release references to tasks, their arguments and their results as soon as they are finished in multiprocessing.Pool.
- bpo-29884: faulthandler: Restore the old sigaltstack during teardown. Patch by Christophe Zeitouny.
- bpo-25455: Fixed crashes in repr of recursive buffered file-like objects.
- bpo-29800: Fix crashes in partial.__repr__ if the keys of partial.keywords are not strings. Patch by Michael Seifert.
- bpo-29742: get_extra_info() raises exception if get called on closed ssl transport. Patch by Nikolay Kim.
- bpo-8256: Fixed possible failing or crashing input() if attributes âencodingâ or âerrorsâ of sys.stdin or sys.stdout are not set or are not strings.
- bpo-28298: Fix a bug that prevented array âQâ, âLâ and âIâ from accepting big intables (objects that have __int__) as elements. Patch by Oren Milman.
- bpo-29615: SimpleXMLRPCDispatcher no longer chains KeyError (or any other exception) to exception(s) raised in the dispatched methods. Patch by Petr Motejlek.
- bpo-29704: asyncio.subprocess.SubprocessStreamProtocol no longer closes before all pipes are closed.
- bpo-29703: Fix asyncio to support instantiation of new event loops in child processes.
- bpo-29376: Fix assertion error in threading._DummyThread.is_alive().
- bpo-29110: Fix file object leak in aifc.open() when file is given as a filesystem path and is not in valid AIFF format. Patch by Anthony Zhang.
- bpo-28961: Fix unittest.mock._Call helper: donât ignore the name parameter anymore. Patch written by Jiajun Huang.
- bpo-29532: Altering a kwarg dictionary passed to functools.partial() no longer affects a partial object after creation.
- bpo-28556: Various updates to typing module: typing.Counter, typing.ChainMap, improved ABC caching, etc. Original PRs by Jelle Zijlstra, Ivan Levkivskyi, Manuel Krebber, and Åukasz Langa.
- bpo-29100: Fix datetime.fromtimestamp() regression introduced in Python 3.6.0: check minimum and maximum years.
- bpo-29519: Fix weakref spewing exceptions during interpreter shutdown when used with a rare combination of multiprocessing and custom codecs.
- bpo-29416: Prevent infinite loop in pathlib.Path.mkdir
- bpo-29444: Fixed out-of-bounds buffer access in the group() method of the match object. Based on patch by WGH.
- bpo-29335: Fix subprocess.Popen.wait() when the child process has exited to a stopped instead of terminated state (ex: when under ptrace).
- bpo-29290: Fix a regression in argparse that help messages would wrap at non-breaking spaces.
- bpo-28735: Fixed the comparison of mock.MagickMock with mock.ANY.
- bpo-29011: Fix an important omission by adding Deque to the typing module.
- bpo-29219: Fixed infinite recursion in the repr of uninitialized ctypes.CDLL instances.
- bpo-28969: Fixed race condition in C implementation of functools.lru_cache. KeyError could be raised when cached function with full cache was simultaneously called from different threads with the same uncached arguments.
- bpo-29142: In urllib.request, suffixes in no_proxy environment variable with leading dots could match related hostnames again (e.g. .b.c matches a.b.c). Patch by Milan Oberkirch.

### DocumentationÂ¶

- bpo-30176: Add missing attribute related constants in curses documentation.
- bpo-26985: Add missing info of code object in inspect documentation.
- bpo-28929: Link the documentation to its source file on GitHub.
- bpo-25008: Document smtpd.py as effectively deprecated and add a pointer to aiosmtpd, a third-party asyncio-based replacement.
- bpo-26355: Add canonical header link on each page to corresponding major version of the documentation. Patch by Matthias Bussonnier.
- bpo-29349: Fix Python 2 syntax in code for building the documentation.

### TestsÂ¶

- bpo-30822: Fix regrtest command line parser to allow passing -u extralargefile to run test_zipfile64.
- bpo-30383: regrtest: Enhance regrtest and backport features from the master branch. Add options: âcoverage, âtestdir, âlist-tests (list test files, donât run them), âlist-cases (list test identifiers, donât run them, bpo-30523), âmatchfile (load a list of test filters from a text file, bpo-30540), âslowest (alias to âslow). Enhance output: add timestamp, test result, currently running tests, âTests result: xxxâ summary with total duration, etc. Fix reference leak hunting in regrtest, âhuntrleaks: regrtest now warms up caches, create explicitly all internal singletons which are created on demand to prevent false positives when checking for reference leaks. (bpo-30675).
- bpo-30357: test_thread: setUp() now uses support.threading_setup() and support.threading_cleanup() to wait until threads complete to avoid random side effects on following tests. Initial patch written by Grzegorz Grzywacz.
- bpo-28087: Skip test_asyncore and test_eintr poll failures on macOS. Skip some tests of select.poll when running on macOS due to unresolved issues with the underlying system poll function on some macOS versions.
- bpo-30197: Enhanced functions swap_attr() and swap_item() in the test.support module. They now work when delete replaced attribute or item inside the with statement. The old value of the attribute or item (or None if it doesnât exist) now will be assigned to the target of the âasâ clause, if there is one.
- bpo-29571: to match the behaviour of the `re.LOCALE` flag,
test_re.test_locale_flag now uses`locale.getpreferredencoding(False)` to determine the candidate encoding for the test regex (allowing it to
correctly skip the test when the default locale encoding is a multi-byte
encoding)

### BuildÂ¶

- bpo-29243: Prevent unnecessary rebuilding of Python during `make test` ,`make install` and some other make targets when configured with`--enable-optimizations` .
- bpo-23404: Donât regenerate generated files based on file modification time anymore: the action is now explicit. Replace `make touch` with`make regen-all` .
- bpo-29643: Fix `--enable-optimization` didnât work.

### WindowsÂ¶

### C APIÂ¶

- bpo-27867: Function PySlice_GetIndicesEx() is replaced with a macro if Py_LIMITED_API is set to the value between 0x03050400 and 0x03060000 (not including) or 0x03060100 or higher.
- bpo-29083: Fixed the declaration of some public API functions. PyArg_VaParse() and PyArg_VaParseTupleAndKeywords() were not available in limited API. PyArg_ValidateKeywordArguments(), PyArg_UnpackTuple() and Py_BuildValue() were not available in limited API of version < 3.3 when PY_SSIZE_T_CLEAN is defined.

## Python 3.5.3 finalÂ¶

*Release date: 2017-01-17*

There were no code changes between 3.5.3rc1 and 3.5.3 final.

## Python 3.5.3 release candidate 1Â¶

*Release date: 2017-01-02*

### SecurityÂ¶

### Core and BuiltinsÂ¶

- bpo-29073: bytearray formatting no longer truncates on first null byte.
- bpo-28932: Do not include <sys/random.h> if it does not exist.
- bpo-28147: Fix a memory leak in split-table dictionaries: setattr() must not convert combined table into split table.
- bpo-25677: Correct the positioning of the syntax error caret for indented blocks. Based on patch by Michael Layzell.
- bpo-29000: Fixed bytes formatting of octals with zero padding in alternate form.
- bpo-28512: Fixed setting the offset attribute of SyntaxError by PyErr_SyntaxLocationEx() and PyErr_SyntaxLocationObject().
- bpo-28991: functools.lru_cache() was susceptible to an obscure reentrancy bug caused by a monkey-patched len() function.
- bpo-28648: Fixed crash in Py_DecodeLocale() in debug build on Mac OS X when decode astral characters. Patch by Xiang Zhang.
- bpo-19398: Extra slash no longer added to sys.path components in case of empty compile-time PYTHONPATH components.
- bpo-28426: Fixed potential crash in PyUnicode_AsDecodedObject() in debug build.
- bpo-23782: Fixed possible memory leak in _PyTraceback_Add() and exception loss in PyTraceBack_Here().
- bpo-28379: Added sanity checks and tests for PyUnicode_CopyCharacters(). Patch by Xiang Zhang.
- bpo-28376: The type of long range iterator is now registered as Iterator. Patch by Oren Milman.
- bpo-28376: The constructor of range_iterator now checks that step is not 0. Patch by Oren Milman.
- bpo-26906: Resolving special methods of uninitialized type now causes implicit initialization of the type instead of a fail.
- bpo-18287: PyType_Ready() now checks that tp_name is not NULL. Original patch by Niklas Koep.
- bpo-24098: Fixed possible crash when AST is changed in process of compiling it.
- bpo-28350: String constants with null character no longer interned.
- bpo-26617: Fix crash when GC runs during weakref callbacks.
- bpo-27942: String constants now interned recursively in tuples and frozensets.
- bpo-21578: Fixed misleading error message when ImportError called with invalid keyword args.
- bpo-28203: Fix incorrect type in error message from ```
complex(1.0,
{2:3})
```
. Patch by Soumya Sharma.
- bpo-27955: Fallback on reading /dev/urandom device when the getrandom() syscall fails with EPERM, for example when blocked by SECCOMP.
- bpo-28131: Fix a regression in zipimportâs compile_source(). zipimport should use the same optimization level as the interpreter.
- bpo-25221: Fix corrupted result from PyLong_FromLong(0) when Python is compiled with NSMALLPOSINTS = 0.
- bpo-25758: Prevents zipimport from unnecessarily encoding a filename (patch by Eryk Sun)
- bpo-28189: dictitems_contains no longer swallows compare errors. (Patch by Xiang Zhang)
- bpo-27812: Properly clear out a generatorâs frameâs backreference to the generator to prevent crashes in frame.clear().
- bpo-27811: Fix a crash when a coroutine that has not been awaited is finalized with warnings-as-errors enabled.
- bpo-27587: Fix another issue found by PVS-Studio: Null pointer check after use of âdefâ in _PyState_AddModule(). Initial patch by Christian Heimes.
- bpo-26020: set literal evaluation order did not match documented behaviour.
- bpo-27782: Multi-phase extension module import now correctly allows the `m_methods` field to be used to add module level functions to instances
of non-module types returned from`Py_create_mod` . Patch by Xiang Zhang.
- bpo-27936: The round() function accepted a second None argument for some types but not for others. Fixed the inconsistency by accepting None for all numeric types.
- bpo-27487: Warn if a submodule argument to âpython -mâ or runpy.run_module() is found in sys.modules after parent packages are imported, but before the submodule is executed.
- bpo-27558: Fix a SystemError in the implementation of âraiseâ statement. In a brand new thread, raise a RuntimeError since there is no active exception to reraise. Patch written by Xiang Zhang.
- bpo-27419: Standard __import__() no longer look up â__import__â in globals or builtins for importing submodules or âfrom importâ. Fixed handling an error of non-string package name.
- bpo-27083: Respect the PYTHONCASEOK environment variable under Windows.
- bpo-27514: Make having too many statically nested blocks a SyntaxError instead of SystemError.
- bpo-27473: Fixed possible integer overflow in bytes and bytearray concatenations. Patch by Xiang Zhang.
- bpo-27507: Add integer overflow check in bytearray.extend(). Patch by Xiang Zhang.
- bpo-27581: Donât rely on wrapping for overflow check in PySequence_Tuple(). Patch by Xiang Zhang.
- bpo-27443: __length_hint__() of bytearray iterators no longer return a negative integer for a resized bytearray.
- bpo-27942: Fix memory leak in codeobject.c

### LibraryÂ¶

- bpo-15812: inspect.getframeinfo() now correctly shows the first line of a context. Patch by Sam Breese.
- bpo-29094: Offsets in a ZIP file created with extern file object and modes âwâ and âxâ now are relative to the start of the file.
- bpo-13051: Fixed recursion errors in large or resized curses.textpad.Textbox. Based on patch by Tycho Andersen.
- bpo-29119: Fix weakrefs in the pure python version of collections.OrderedDict move_to_end() method. Contributed by Andra Bogildea.
- bpo-9770: curses.ascii predicates now work correctly with negative integers.
- bpo-28427: old keys should not remove new values from WeakValueDictionary when collecting from another thread.
- bpo-28923: Remove editor artifacts from Tix.py.
- bpo-28871: Fixed a crash when deallocate deep ElementTree.
- bpo-19542: Fix bugs in WeakValueDictionary.setdefault() and WeakValueDictionary.pop() when a GC collection happens in another thread.
- bpo-20191: Fixed a crash in resource.prlimit() when pass a sequence that doesnât own its elements as limits.
- bpo-28779: multiprocessing.set_forkserver_preload() would crash the forkserver process if a preloaded module instantiated some multiprocessing objects such as locks.
- bpo-28847: dbm.dumb now supports reading read-only files and no longer writes the index file when it is not changed.
- bpo-25659: In ctypes, prevent a crash calling the from_buffer() and from_buffer_copy() methods on abstract classes like Array.
- bpo-28732: Fix crash in os.spawnv() with no elements in args
- bpo-28485: Always raise ValueError for negative compileall.compile_dir(workers=â¦) parameter, even when multithreading is unavailable.
- bpo-28387: Fixed possible crash in _io.TextIOWrapper deallocator when the garbage collector is invoked in other thread. Based on patch by Sebastian Cufre.
- bpo-27517: LZMA compressor and decompressor no longer raise exceptions if given empty data twice. Patch by Benjamin Fogle.
- bpo-28549: Fixed segfault in cursesâs addch() with ncurses6.
- bpo-28449: tarfile.open() with mode ârâ or âr:â now tries to open a tar file with compression before trying to open it without compression. Otherwise it had 50% chance failed with ignore_zeros=True.
- bpo-23262: The webbrowser module now supports Firefox 36+ and derived browsers. Based on patch by Oleg Broytman.
- bpo-27939: Fixed bugs in tkinter.ttk.LabeledScale and tkinter.Scale caused by representing the scale as float value internally in Tk. tkinter.IntVar now works if float value is set to underlying Tk variable.
- bpo-28255: calendar.TextCalendar().prmonth() no longer prints a space at the start of new line after printing a monthâs calendar. Patch by Xiang Zhang.
- bpo-20491: The textwrap.TextWrapper class now honors non-breaking spaces. Based on patch by Kaarle Ritvanen.
- bpo-28353: os.fwalk() no longer fails on broken links.
- bpo-25464: Fixed HList.header_exists() in tkinter.tix module by addin a workaround to Tix library bug.
- bpo-28488: shutil.make_archive() no longer add entry â./â to ZIP archive.
- bpo-24452: Make webbrowser support Chrome on Mac OS X.
- bpo-20766: Fix references leaked by pdb in the handling of SIGINT handlers.
- bpo-26293: Fixed writing ZIP files that starts not from the start of the file. Offsets in ZIP file now are relative to the start of the archive in conforming to the specification.
- bpo-28321: Fixed writing non-BMP characters with binary format in plistlib.
- bpo-28322: Fixed possible crashes when unpickle itertools objects from incorrect pickle data. Based on patch by John Leitch.
- Fix possible integer overflows and crashes in the mmap module with unusual usage patterns.
- bpo-1703178: Fix the ability to pass the âlink-objects option to the distutils build_ext command.
- bpo-28253: Fixed calendar functions for extreme months: 0001-01 and 9999-12. Methods itermonthdays() and itermonthdays2() are reimplemented so that they donât call itermonthdates() which can cause datetime.date under/overflow.
- bpo-28275: Fixed possible use after free in the decompress() methods of the LZMADecompressor and BZ2Decompressor classes. Original patch by John Leitch.
- bpo-27897: Fixed possible crash in sqlite3.Connection.create_collation() if pass invalid string-like object as a name. Patch by Xiang Zhang.
- bpo-18893: Fix invalid exception handling in Lib/ctypes/macholib/dyld.py. Patch by Madison May.
- bpo-27611: Fixed support of default root window in the tkinter.tix module.
- bpo-27348: In the traceback module, restore the formatting of exception messages like âException: Noneâ. This fixes a regression introduced in 3.5a2.
- bpo-25651: Allow false values to be used for msg parameter of subTest().
- bpo-27932: Prevent memory leak in win32_ver().
- Fix UnboundLocalError in socket._sendfile_use_sendfile.
- bpo-28075: Check for ERROR_ACCESS_DENIED in Windows implementation of os.stat(). Patch by Eryk Sun.
- bpo-25270: Prevent codecs.escape_encode() from raising SystemError when an empty bytestring is passed.
- bpo-28181: Get antigravity over HTTPS. Patch by Kaartic Sivaraam.
- bpo-25895: Enable WebSocket URL schemes in urllib.parse.urljoin. Patch by Gergely Imreh and Markus Holtermann.
- bpo-27599: Fixed buffer overrun in binascii.b2a_qp() and binascii.a2b_qp().
- bpo-19003: m email.generator now replaces only `\r` and/or`\n` line
endings, per the RFC, instead of all unicode line endings.
- bpo-28019: itertools.count() no longer rounds non-integer step in range between 1.0 and 2.0 to 1.
- bpo-25969: Update the lib2to3 grammar to handle the unpacking generalizations added in 3.5.
- bpo-14977: mailcap now respects the order of the lines in the mailcap files (âfirst matchâ), as required by RFC 1542. Patch by Michael Lazar.
- bpo-24594: Validates persist parameter when opening MSI database
- bpo-17582: xml.etree.ElementTree nows preserves whitespaces in attributes (Patch by Duane Griffin. Reviewed and approved by Stefan Behnel.)
- bpo-28047: Fixed calculation of line length used for the base64 CTE in the new email policies.
- bpo-27445: Donât pass str(_charset) to MIMEText.set_payload(). Patch by Claude Paroz.
- bpo-22450: urllib now includes an `Accept: */*` header among the default
headers. This makes the results of REST API requests more consistent and
predictable especially when proxy servers are involved.
- lib2to3.pgen3.driver.load_grammar() now creates a stable cache file between runs given the same Grammar.txt input regardless of the hash randomization setting.
- bpo-27570: Avoid zero-length memcpy() etc calls with null source pointers in the âctypesâ and âarrayâ modules.
- bpo-22233: Break email header lines *only* on the RFC specified CR and LF
characters, not on arbitrary unicode line breaks.  This also fixes a bug
in HTTP header parsing.
- bpo-27988: Fix email iter_attachments incorrect mutation of payload list.
- bpo-27691: Fix ssl moduleâs parsing of GEN_RID subject alternative name fields in X.509 certs.
- bpo-27850: Remove 3DES from ssl moduleâs default cipher list to counter measure sweet32 attack ( **CVE 2016-2183** ).
- bpo-27766: Add ChaCha20 Poly1305 to ssl moduleâs default cipher list. (Required OpenSSL 1.1.0 or LibreSSL).
- bpo-26470: Port ssl and hashlib module to OpenSSL 1.1.0.
- Remove support for passing a file descriptor to os.access. It never worked but previously didnât raise.
- bpo-12885: Fix error when distutils encounters symlink.
- bpo-27881: Fixed possible bugs when setting sqlite3.Connection.isolation_level. Based on patch by Xiang Zhang.
- bpo-27861: Fixed a crash in sqlite3.Connection.cursor() when a factory creates not a cursor. Patch by Xiang Zhang.
- bpo-19884: Avoid spurious output on OS X with Gnu Readline.
- bpo-27706: Restore deterministic behavior of random.Random().seed() for string seeds using seeding version 1. Allows sequences of calls to random() to exactly match those obtained in Python 2. Patch by Nofar Schnider.
- bpo-10513: Fix a regression in Connection.commit(). Statements should not be reset after a commit.
- A new version of typing.py from https://github.com/python/typing: Collection (only for 3.6) (bpo-27598). Add FrozenSet to __all__ (upstream #261). Fix crash in _get_type_vars() (upstream #259). Remove the dict constraint in ForwardRef._eval_type (upstream #252).
- bpo-27539: Fix unnormalised `Fraction.__pow__` result in the case of
negative exponent and negative base.
- bpo-21718: cursor.description is now available for queries using CTEs.
- bpo-2466: posixpath.ismount now correctly recognizes mount points which the user does not have permission to access.
- bpo-27773: Correct some memory management errors server_hostname in _ssl.wrap_socket().
- bpo-26750: unittest.mock.create_autospec() now works properly for subclasses of property() and other data descriptors.
- In the curses module, raise an error if window.getstr() or window.instr() is passed a negative value.
- bpo-27783: Fix possible usage of uninitialized memory in operator.methodcaller.
- bpo-27774: Fix possible Py_DECREF on unowned object in _sre.
- bpo-27760: Fix possible integer overflow in binascii.b2a_qp.
- bpo-27758: Fix possible integer overflow in the _csv module for large record lengths.
- bpo-27568: Prevent HTTPoxy attack ( **CVE 2016-1000110** ). Ignore the
HTTP_PROXY variable when REQUEST_METHOD environment is set, which
indicates that the script is in CGI mode.
- bpo-27656: Do not assume sched.h defines any SCHED_* constants.
- bpo-27130: In the âzlibâ module, fix handling of large buffers (typically 4 GiB) when compressing and decompressing. Previously, inputs were limited to 4 GiB, and compression and decompression operations did not properly handle results of 4 GiB.
- bpo-27533: Release GIL in nt._isdir
- bpo-17711: Fixed unpickling by the persistent ID with protocol 0. Original patch by Alexandre Vassalotti.
- bpo-27522: Avoid an unintentional reference cycle in email.feedparser.
- bpo-26844: Fix error message for imp.find_module() to refer to âpathâ instead of ânameâ. Patch by Lev Maximov.
- bpo-23804: Fix SSL zero-length recv() calls to not block and not raise an error about unclean EOF.
- bpo-27466: Change time format returned by http.cookie.time2netscape, confirming the netscape cookie format and making it consistent with documentation.
- bpo-26664: Fix activate.fish by removing mis-use of `$` .
- bpo-22115: Fixed tracing Tkinter variables: trace_vdelete() with wrong mode no longer break tracing, trace_vinfo() now always returns a list of pairs of strings, tracing in the âuâ mode now works.
- Fix a scoping issue in importlib.util.LazyLoader which triggered an UnboundLocalError when lazy-loading a module that was already put into sys.modules.
- bpo-27079: Fixed curses.ascii functions isblank(), iscntrl() and ispunct().
- bpo-26754: Some functions (compile() etc) accepted a filename argument encoded as an iterable of integers. Now only strings and byte-like objects are accepted.
- bpo-27048: Prevents distutils failing on Windows when environment variables contain non-ASCII characters
- bpo-27330: Fixed possible leaks in the ctypes module.
- bpo-27238: Got rid of bare excepts in the turtle module. Original patch by Jelle Zijlstra.
- bpo-27122: When an exception is raised within the context being managed by a contextlib.ExitStack() and one of the exit stack generators catches and raises it in a chain, do not re-raise the original exception when exiting, let the new chained one through. This avoids the **PEP 479** bug described
in issue25782.
- bpo-26386: Fixed ttk.TreeView selection operations with item idâs containing spaces.
- bpo-16182: Fix various functions in the âreadlineâ module to use the locale encoding, and fix get_begidx() and get_endidx() to return code point indexes.
- bpo-27392: Add loop.connect_accepted_socket(). Patch by Jim Fulton.
- bpo-27930: Improved behaviour of logging.handlers.QueueListener. Thanks to Paulo Andrade and Petr Viktorin for the analysis and patch.
- bpo-21201: Improves readability of multiprocessing error message. Thanks to Wojciech Walczak for patch.
- bpo-27456: asyncio: Set TCP_NODELAY by default.
- bpo-27906: Fix socket accept exhaustion during high TCP traffic. Patch by Kevin Conway.
- bpo-28174: Handle when SO_REUSEPORT isnât properly supported. Patch by Seth Michael Larson.
- bpo-26654: Inspect functools.partial in asyncio.Handle.__repr__. Patch by iceboy.
- bpo-26909: Fix slow pipes IO in asyncio. Patch by INADA Naoki.
- bpo-28176: Fix callbacks race in asyncio.SelectorLoop.sock_connect.
- bpo-27759: Fix selectors incorrectly retain invalid file descriptors. Patch by Mark Williams.
- bpo-28368: Refuse monitoring processes if the child watcher has no loop attached. Patch by Vincent Michel.
- bpo-28369: Raise RuntimeError when transportâs FD is used with add_reader, add_writer, etc.
- bpo-28370: Speedup asyncio.StreamReader.readexactly. Patch by ÐÐ¾ÑÐµÐ½Ð±ÐµÑÐ³ ÐÐ°ÑÐº.
- bpo-28371: Deprecate passing asyncio.Handles to run_in_executor.
- bpo-28372: Fix asyncio to support formatting of non-python coroutines.
- bpo-28399: Remove UNIX socket from FS before binding. Patch by ÐÐ¾ÑÐµÐ½Ð±ÐµÑÐ³ ÐÐ°ÑÐº.
- bpo-27972: Prohibit Tasks to await on themselves.
- bpo-26923: Fix asyncio.Gather to refuse being cancelled once all children are done. Patch by Johannes Ebke.
- bpo-26796: Donât configure the number of workers for default threadpool executor. Initial patch by Hans Lawrenz.
- bpo-28600: Optimize loop.call_soon().
- bpo-28613: Fix get_event_loop() return the current loop if called from coroutines/callbacks.
- bpo-28639: Fix inspect.isawaitable to always return bool Patch by Justin Mayfield.
- bpo-28652: Make loop methods reject socket kinds they do not support.
- bpo-28653: Fix a refleak in functools.lru_cache.
- bpo-28703: Fix asyncio.iscoroutinefunction to handle Mock objects.
- bpo-24142: Reading a corrupt config file left the parser in an invalid state. Original patch by Florian HÃ¶ch.
- bpo-28990: Fix SSL hanging if connection is closed before handshake completed. (Patch by HoHo-Ho)

### IDLEÂ¶

- bpo-15308: Add âinterrupt executionâ (^C) to Shell menu. Patch by Roger Serwy, updated by Bayard Randel.
- bpo-27922: Stop IDLE tests from âflashingâ gui widgets on the screen.
- Add version to title of IDLE help window.
- bpo-25564: In section on IDLE â console differences, mention that using exec means that __builtins__ is defined for each statement.
- bpo-27714: text_textview and test_autocomplete now pass when re-run in the same process. This occurs when test_idle fails when run with the -w option but without -jn. Fix warning from test_config.
- bpo-25507: IDLE no longer runs buggy code because of its tkinter imports. Users must include the same imports required to run directly in Python.
- bpo-27452: add line counter and crc to IDLE configHandler test dump.
- bpo-27365: Allow non-ascii chars in IDLE NEWS.txt, for contributor names.
- bpo-27245: IDLE: Cleanly delete custom themes and key bindings. Previously, when IDLE was started from a console or by import, a cascade of warnings was emitted. Patch by Serhiy Storchaka.

### C APIÂ¶

### DocumentationÂ¶

- bpo-28513: Documented command-line interface of zipfile.

### TestsÂ¶

- bpo-28950: Disallow -j0 to be combined with -T/-l/-M in regrtest command line arguments.
- bpo-28666: Now test.support.rmtree is able to remove unwritable or unreadable directories.
- bpo-23839: Various caches now are cleared before running every test file.
- bpo-28409: regrtest: fix the parser of command line arguments.
- bpo-27787: Call gc.collect() before checking each test for âdangling threadsâ, since the dangling threads are weak references.
- bpo-27369: In test_pyexpat, avoid testing an error message detail that changed in Expat 2.2.0.

### Tools/DemosÂ¶

- bpo-27952: Get Tools/scripts/fixcid.py working with Python 3 and the current âreâ module, avoid invalid Python backslash escapes, and fix a bug parsing escaped C quote signs.
- bpo-27332: Fixed the type of the first argument of module-level functions generated by Argument Clinic. Patch by Petr Viktorin.
- bpo-27418: Fixed Tools/importbench/importbench.py.

### WindowsÂ¶

- bpo-28251: Improvements to help manuals on Windows.
- bpo-28110: launcher.msi has different product codes between 32-bit and 64-bit
- bpo-25144: Ensures TargetDir is set before continuing with custom install.
- bpo-27469: Adds a shell extension to the launcher so that drag and drop works correctly.
- bpo-27309: Enabled proper Windows styles in python[w].exe manifest.

### BuildÂ¶

- bpo-29080: Removes hard dependency on hg.exe from PCBuild/build.bat
- bpo-23903: Added missed names to PC/python3.def.
- bpo-10656: Fix out-of-tree building on AIX. Patch by Tristan Carel and Michael Haubenwallner.
- bpo-26359: Rename âwith-optimiations to âenable-optimizations.
- bpo-28444: Fix missing extensions modules when cross compiling.
- bpo-28248: Update Windows build and OS X installers to use OpenSSL 1.0.2j.
- bpo-28258: Fixed build with Estonian locale (python-config and distclean targets in Makefile). Patch by Arfrever Frehtes Taifersar Arahesis.
- bpo-26661: setup.py now detects system libffi with multiarch wrapper.
- bpo-28066: Fix the logic that searches build directories for generated include files when building outside the source tree.
- bpo-15819: Remove redundant include search directory option for building outside the source tree.
- bpo-27566: Fix clean target in freeze makefile (patch by Lisa Roach)
- bpo-27705: Update message in validate_ucrtbase.py
- bpo-27983: Cause lack of llvm-profdata tool when using clang as required for PGO linking to be a configure time error rather than make time when âwith-optimizations is enabled. Also improve our ability to find the llvm-profdata tool on MacOS and some Linuxes.
- bpo-26307: The profile-opt build now applies PGO to the built-in modules.
- bpo-26359: Add the âwith-optimizations configure flag.
- bpo-27713: Suppress spurious build warnings when updating importlibâs bootstrap files. Patch by Xiang Zhang
- bpo-25825: Correct the references to Modules/python.exp and ld_so_aix, which are required on AIX. This updates references to an installation path that was changed in 3.2a4, and undoes changed references to the build tree that were made in 3.5.0a1.
- bpo-27453: CPP invocation in configure must use CPPFLAGS. Patch by Chi Hsuan Yen.
- bpo-27641: The configure script now inserts comments into the makefile to prevent the pgen and _freeze_importlib executables from being cross-compiled.
- bpo-26662: Set PYTHON_FOR_GEN in configure as the Python program to be used for file generation during the build.
- bpo-10910: Avoid C++ compilation errors on FreeBSD and OS X. Also update FreedBSD version checks for the original ctype UTF-8 workaround.
- bpo-28676: Prevent missing âgetentropyâ declaration warning on macOS. Patch by Gareth Rees.

## Python 3.5.2 finalÂ¶

*Release date: 2016-06-26*

### Core and BuiltinsÂ¶

- bpo-26930: Update Windows builds to use OpenSSL 1.0.2h.

### TestsÂ¶

- bpo-26867: Ubuntuâs openssl OP_NO_SSLv3 is forced on by default; fix test.

### IDLEÂ¶

- bpo-27365: Allow non-ascii in idlelib/NEWS.txt - minimal part for 3.5.2.

## Python 3.5.2 release candidate 1Â¶

*Release date: 2016-06-12*

### SecurityÂ¶

- bpo-26556: Update expat to 2.1.1, fixes **CVE 2015-1283** .
- Fix TLS stripping vulnerability in smtplib, **CVE 2016-0772** . Reported by
Team Oststrom.
- bpo-26839: On Linux, `os.urandom()` now calls`getrandom()` with`GRND_NONBLOCK` to fall back on reading`/dev/urandom` if the urandom
entropy pool is not initialized yet. Patch written by Colm Buckley.
- bpo-26657: Fix directory traversal vulnerability with http.server on Windows. This fixes a regression that was introduced in 3.3.4rc1 and 3.4.0rc1. Based on patch by Philipp Hagemeister.
- bpo-26313: ssl.py _load_windows_store_certs fails if windows cert store is empty. Patch by Baji.
- bpo-25939: On Windows open the cert store readonly in ssl.enum_certificates.

### Core and BuiltinsÂ¶

- bpo-27066: Fixed SystemError if a custom opener (for open()) returns a negative number without setting an exception.
- bpo-20041: Fixed TypeError when frame.f_trace is set to None. Patch by Xavier de Gaye.
- bpo-26168: Fixed possible refleaks in failing Py_BuildValue() with the âNâ format unit.
- bpo-26991: Fix possible refleak when creating a function with annotations.
- bpo-27039: Fixed bytearray.remove() for values greater than 127. Patch by Joe Jevnik.
- bpo-23640: int.from_bytes() no longer bypasses constructors for subclasses.
- bpo-26811: gc.get_objects() no longer contains a broken tuple with NULL pointer.
- bpo-20120: Use RawConfigParser for .pypirc parsing, removing support for interpolation unintentionally added with move to Python 3. Behavior no longer does any interpolation in .pypirc files, matching behavior in Python 2.7 and Setuptools 19.0.
- bpo-26659: Make the builtin slice type support cycle collection.
- bpo-26718: super.__init__ no longer leaks memory if called multiple times. NOTE: A direct call of super.__init__ is not endorsed!
- bpo-25339: PYTHONIOENCODING now has priority over locale in setting the error handler for stdin and stdout.
- bpo-26494: Fixed crash on iterating exhausting iterators. Affected classes are generic sequence iterators, iterators of str, bytes, bytearray, list, tuple, set, frozenset, dict, OrderedDict, corresponding views and os.scandir() iterator.
- bpo-26581: If coding cookie is specified multiple times on a line in Python source code file, only the first one is taken to account.
- bpo-26464: Fix str.translate() when string is ASCII and first replacements removes character, but next replacement uses a non-ASCII character or a string longer than 1 character. Regression introduced in Python 3.5.0.
- bpo-22836: Ensure exception reports from PyErr_Display() and PyErr_WriteUnraisable() are sensible even when formatting them produces secondary errors. This affects the reports produced by sys.__excepthook__() and when __del__() raises an exception.
- bpo-26302: Correct behavior to reject comma as a legal character for cookie names.
- bpo-4806: Avoid masking the original TypeError exception when using star ( `*` ) unpacking in function calls.  Based on patch by Hagen FÃ¼rstenau
and Daniel Urban.
- bpo-27138: Fix the doc comment for FileFinder.find_spec().
- bpo-26154: Add a new private _PyThreadState_UncheckedGet() function to get the current Python thread state, but donât issue a fatal error if it is NULL. This new function must be used instead of accessing directly the _PyThreadState_Current variable. The variable is no more exposed since Python 3.5.1 to hide the exact implementation of atomic C types, to avoid compiler issues.
- bpo-26194: Deque.insert() gave odd results for bounded deques that had reached their maximum size. Now an IndexError will be raised when attempting to insert into a full deque.
- bpo-25843: When compiling code, donât merge constants if they are equal but have a different types. For example, ```
f1, f2 = lambda: 1, lambda:
1.0
```
is now correctly compiled to two different functions:`f1()` returns`1` (`int` ) and`f2()` returns`1.0` (`int` ), even if`1` and`1.0` are equal.
- bpo-22995: [UPDATE] Comment out the one of the pickleability tests in _PyObject_GetState() due to regressions observed in Cython-based projects.
- bpo-25961: Disallowed null characters in the type name.
- bpo-25973: Fix segfault when an invalid nonlocal statement binds a name starting with two underscores.
- bpo-22995: Instances of extension types with a state that arenât subclasses of list or dict and havenât implemented any pickle-related methods (__reduce__, __reduce_ex__, __getnewargs__, __getnewargs_ex__, or __getstate__), can no longer be pickled. Including memoryview.
- bpo-20440: Massive replacing unsafe attribute setting code with special macro Py_SETREF.
- bpo-25766: Special method __bytes__() now works in str subclasses.
- bpo-25421: __sizeof__ methods of builtin types now use dynamic basic size. This allows sys.getsize() to work correctly with their subclasses with __slots__ defined.
- bpo-25709: Fixed problem with in-place string concatenation and utf-8 cache.
- bpo-27147: Mention **PEP 420** in the importlib docs.
- bpo-24097: Fixed crash in object.__reduce__() if slot name is freed inside __getattr__.
- bpo-24731: Fixed crash on converting objects with special methods __bytes__, __trunc__, and __float__ returning instances of subclasses of bytes, int, and float to subclasses of bytes, int, and float correspondingly.
- bpo-26478: Fix semantic bugs when using binary operators with dictionary views and tuples.
- bpo-26171: Fix possible integer overflow and heap corruption in zipimporter.get_data().
- bpo-25660: Fix TAB key behaviour in REPL with readline.
- bpo-25887: Raise a RuntimeError when a coroutine object is awaited more than once.
- bpo-27243: Update the __aiter__ protocol: instead of returning an awaitable that resolves to an asynchronous iterator, the asynchronous iterator should be returned directly. Doing the former will trigger a PendingDeprecationWarning.

### LibraryÂ¶

- bpo-21386: Implement missing IPv4Address.is_global property. It was documented since 07a5610bae9d. Initial patch by Roger Luethi.
- bpo-20900: distutils register command now decodes HTTP responses correctly. Initial patch by ingrid.
- A new version of typing.py provides several new classes and features: @overload outside stubs, Reversible, DefaultDict, Text, ContextManager, Type[], NewType(), TYPE_CHECKING, and numerous bug fixes (note that some of the new features are not yet implemented in mypy or other static analyzers). Also classes for **PEP 492** (Awaitable, AsyncIterable,
AsyncIterator) have been added (in fact they made it into 3.5.1 but were
never mentioned).
- bpo-25738: Stop http.server.BaseHTTPRequestHandler.send_error() from sending a message body for 205 Reset Content. Also, donât send Content header fields in responses that donât have a body. Patch by Susumu Koshiba.
- bpo-21313: Fix the âplatformâ module to tolerate when sys.version contains truncated build information.
- bpo-27164: In the zlib module, allow decompressing raw Deflate streams with a predefined zdict. Based on patch by Xiang Zhang.
- bpo-24291: Fix wsgiref.simple_server.WSGIRequestHandler to completely write data to the client. Previously it could do partial writes and truncate data. Also, wsgiref.handler.ServerHandler can now handle stdout doing partial writes, but this is deprecated.
- bpo-26809: Add `__all__` to`string` .  Patch by Emanuel Barry.
- bpo-26373: subprocess.Popen.communicate now correctly ignores BrokenPipeError when the child process dies before .communicate() is called in more/all circumstances.
- bpo-21776: distutils.upload now correctly handles HTTPError. Initial patch by Claudiu Popa.
- bpo-27114: Fix SSLContext._load_windows_store_certs fails with PermissionError
- bpo-18383: Avoid creating duplicate filters when using filterwarnings and simplefilter. Based on patch by Alex Shkop.
- bpo-27057: Fix os.set_inheritable() on Android, ioctl() is blocked by SELinux and fails with EACCESS. The function now falls back to fcntl(). Patch written by MichaÅ Bednarski.
- bpo-27014: Fix infinite recursion using typing.py. Thanks to Kalle Tuure!
- bpo-14132: Fix urllib.request redirect handling when the target only has a query string. Original fix by JÃ¡n Janech.
- bpo-17214: The âurllib.requestâ module now percent-encodes non-ASCII bytes found in redirect target URLs. Some servers send Location header fields with non-ASCII bytes, but âhttp.clientâ requires the request target to be ASCII-encodable, otherwise a UnicodeEncodeError is raised. Based on patch by Christian Heimes.
- bpo-26892: Honor debuglevel flag in urllib.request.HTTPHandler. Patch contributed by Chi Hsuan Yen.
- bpo-22274: In the subprocess module, allow stderr to be redirected to stdout even when stdout is not redirected. Patch by Akira Li.
- bpo-26807: mock_open âfilesâ no longer error on readline at end of file. Patch from Yolanda Robla.
- bpo-25745: Fixed leaking a userptr in curses panel destructor.
- bpo-26977: Removed unnecessary, and ignored, call to sum of squares helper in statistics.pvariance.
- bpo-26881: The modulefinder module now supports extended opcode arguments.
- bpo-23815: Fixed crashes related to directly created instances of types in _tkinter and curses.panel modules.
- bpo-17765: weakref.ref() no longer silently ignores keyword arguments. Patch by Georg Brandl.
