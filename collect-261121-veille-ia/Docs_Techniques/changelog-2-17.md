---
id: collect-261121-veille-ia/docs-techniques/changelog-2-17
title: "Changelog"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["update", "modules", "files", "test", "right", "consistently", "called", "option"]
source: https://docs.python.org/release/3.15.1/whatsnew/changelog.html
source_anchor: ""
source_lines: [24001, 25500]
section: "Docs_Techniques"
---
- bpo-33217: Deprecate looking up non-Enum objects in Enum classes and Enum members (will raise `TypeError` in 3.8+).
- bpo-33203: `random.Random.choice()` now raises`IndexError` for empty
sequences consistently even when called from subclasses without a`getrandbits()` implementation.
- bpo-33224: Update difflib.mdiff() for **PEP 479** .  Convert an uncaught
StopIteration in a generator into a return-statement.
- bpo-33209: End framing at the end of C implementation of `pickle.Pickler.dump()` .
- bpo-20104: Improved error handling and fixed a reference leak in `os.posix_spawn()` .
- bpo-33175: In dataclasses, Field.__set_name__ now looks up the __set_name__ special method on the class, not the instance, of the default value.
- bpo-33097: Raise RuntimeError when `executor.submit` is called during
interpreter shutdown.
- bpo-31908: Fix output of cover files for `trace` module command-line
tool. Previously emitted cover files only when`--missing` option was
used. Patch by Michael Selik.

### DocumentationÂ¶

- bpo-33378: Add Korean language switcher for https://docs.python.org/3/
- bpo-33276: Clarify that the `__path__` attribute on modules cannot be
just any value.
- bpo-33201: Modernize documentation for writing C extension types.
- bpo-33195: Deprecate `Py_UNICODE` usage in`c-api/arg` document.`Py_UNICODE` related APIs are deprecated since Python 3.3, but it is
missed in the document.
- bpo-8243: Add a note about curses.addch and curses.addstr exception behavior when writing outside a window, or pad.
- bpo-32337: Update documentation related with `dict` order.

### TestsÂ¶

- bpo-33358: Fix `test_embed.test_pre_initialization_sys_options()` when
the interpreter is built with`--enable-shared` .

### BuildÂ¶

- bpo-33394: Enable the verbose build for extension modules, when GNU make is passed macros on the command line.
- bpo-33393: Update config.guess and config.sub files.
- bpo-33377: Add new triplets for mips r6 and riscv variants (used in extension suffixes).
- bpo-32232: By default, modules configured in `Modules/Setup` are no
longer built with`-DPy_BUILD_CORE` . Instead, modules that specifically
need that preprocessor definition include it in their individual entries.
- bpo-33182: The embedding tests can once again be built with clang 6.0

### WindowsÂ¶

- bpo-33184: Update Windows installer to use OpenSSL 1.1.0h.

### macOSÂ¶

- bpo-33184: Update macOS installer build to use OpenSSL 1.1.0h.

### IDLEÂ¶

- bpo-21474: Update word/identifier definition from ascii to unicode. In text and entry boxes, this affects selection by double-click, movement left/right by control-left/right, and deletion left/right by control-BACKSPACE/DEL.
- bpo-33204: IDLE: consistently color invalid string prefixes. A âuâ string prefix cannot be paired with either ârâ or âfâ. Consistently color as much of the prefix, starting at the right, as is valid. Revise and extend colorizer test.

### Tools/DemosÂ¶

- bpo-33189: **pygettext.py** now recognizes only literal strings as
docstrings and translatable strings, and rejects bytes literals and
f-string expressions.
- bpo-31920: Fixed handling directories as arguments in the `pygettext` script. Based on patch by Oleg Krasnikov.
- bpo-29673: Fix pystackv and pystack gdbinit macros.
- bpo-31583: Fix 2to3 for using with âadd-suffix option but without âoutput-dir option for relative path to files in current directory.

## Python 3.7.0 beta 3Â¶

*Release date: 2018-03-29*

### SecurityÂ¶

- bpo-33136: Harden ssl module against LibreSSL **CVE 2018-8970** .
X509_VERIFY_PARAM_set1_host() is called with an explicit namelen. A new
test ensures that NULL bytes are not allowed.
- bpo-33001: Minimal fix to prevent buffer overrun in os.symlink on Windows
- bpo-32981: Regexes in difflib and poplib were vulnerable to catastrophic backtracking. These regexes formed potential DOS vectors (REDOS). They have been refactored. This resolves **CVE 2018-1060** and**CVE 2018-1061** .
Patch by Jamie Davis.

### Core and BuiltinsÂ¶

- bpo-33053: When using the -m switch, sys.path[0] is now explicitly expanded as the *starting* working directory, rather than being left as
the empty path (which allows imports from the current working directory at
the time of the import)
- bpo-33018: Improve consistency of errors raised by `issubclass()` when
called with a non-class and an abstract base class as the first and second
arguments, respectively. Patch by Josh Bronson.
- bpo-33041: Fixed jumping when the function contains an `async for` loop.
- bpo-33026: Fixed jumping out of âwithâ block by setting f_lineno.
- bpo-33005: Fix a crash on fork when using a custom memory allocator (ex: using PYTHONMALLOC env var). _PyGILState_Reinit() and _PyInterpreterState_Enable() now use the default RAW memory allocator to allocate a new interpreters mutex on fork.
- bpo-17288: Prevent jumps from âreturnâ and âexceptionâ trace events.
- bpo-32836: Donât use temporary variables in cases of list/dict/set comprehensions

### LibraryÂ¶

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
- bpo-33061: Add missing `NoReturn` to`__all__` in typing.py
- bpo-33078: Fix the size handling in multiprocessing.Queue when a pickling error occurs.
- bpo-33064: lib2to3 now properly supports trailing commas after `*args` and`**kwargs` in function signatures.
- bpo-33056: FIX properly close leaking fds in concurrent.futures.ProcessPoolExecutor.
- bpo-33021: Release the GIL during fstat() calls, avoiding hang of all threads when calling mmap.mmap(), os.urandom(), and random.seed(). Patch by Nir Soffer.
- bpo-31804: Avoid failing in multiprocessing.Process if the standard streams are closed or None at exit.
- bpo-33037: Skip sending/receiving data after SSL transport closing.
- bpo-27683: Fix a regression in `ipaddress` that result of`hosts()` is empty when the network is constructed by a tuple
containing an integer mask and only 1 bit left for addresses.
- bpo-32999: Fix C implementation of ```
ABC.__subclasscheck__(cls,
subclass)
```
crashed when`subclass` is not a type object.
- bpo-33009: Fix inspect.signature() for single-parameter partialmethods.
- bpo-32969: Expose several missing constants in zlib and fix corresponding documentation.
- bpo-32056: Improved exceptions raised for invalid number of channels and sample width when read an audio file in modules `aifc` ,`wave` and`sunau` .
- bpo-32844: Fix wrong redirection of a low descriptor (0 or 1) to stderr in subprocess if another low descriptor is closed.
- bpo-32857: In `tkinter` ,`after_cancel(None)` now raises a`ValueError` instead of canceling the first scheduled function.
Patch by Cheryl Sabella.
- bpo-31639: http.server now exposes a ThreadedHTTPServer class and uses it when the module is run with `-m` to cope with web browsers pre-opening
sockets.
- bpo-27645: `sqlite3.Connection` now exposes a`backup` method, if the underlying SQLite
library is at version 3.6.11 or higher.  Patch by Lele Gaifax.

### DocumentationÂ¶

- bpo-33126: Document PyBuffer_ToContiguous().
- bpo-27212: Modify documentation for the `islice()` recipe to consume
initial values up to the start index.
- bpo-28247: Update `zipapp` documentation to describe how to make
standalone applications.
- bpo-18802: Documentation changes for ipaddress. Patch by Jon Foster and Berker Peksag.
- bpo-27428: Update documentation to clarify that `WindowsRegistryFinder` implements`MetaPathFinder` . (Patch by Himanshu Lakhara)

### TestsÂ¶

### BuildÂ¶

- bpo-33163: Upgrade pip to 9.0.3 and setuptools to v39.0.1.

### WindowsÂ¶

### macOSÂ¶

- bpo-32726: Build and link with private copy of Tcl/Tk 8.6 for the macOS 10.6+ installer. The 10.9+ installer variant already does this. This means that the Python 3.7 provided by the python.org macOS installers no longer need or use any external versions of Tcl/Tk, either system-provided or user-installed, such as ActiveTcl.

### IDLEÂ¶

- bpo-32984: Set `__file__` while running a startup file.  Like Python,
IDLE optionally runs one startup file in the Shell window before
presenting the first interactive input prompt.  For IDLE,`-s` runs a
file named in environmental variable`IDLESTARTUP` or`PYTHONSTARTUP` ;`-r file` runs`file` .  Python sets`__file__` to the startup file name before running the file and unsets
it before the first prompt.  IDLE now does the same when run normally,
without the`-n` option.
- bpo-32940: Simplify and rename StringTranslatePseudoMapping in pyparse.

### Tools/DemosÂ¶

- bpo-32885: Add an `-n` flag for`Tools/scripts/pathfix.py` to disable
automatic backup creation (files with`~` suffix).

### C APIÂ¶

## Python 3.7.0 beta 2Â¶

*Release date: 2018-02-27*

### SecurityÂ¶

- bpo-28414: The ssl module now allows users to perform their own IDN en/decoding when using SNI.

### Core and BuiltinsÂ¶

- bpo-32889: Update Valgrind suppression list to account for the rename of `Py_ADDRESS_IN_RANG` to`address_in_range` .
- bpo-31356: Remove the new API added in bpo-31356 (gc.ensure_disabled() context manager).
- bpo-32305: For namespace packages, ensure that both `__file__` and`__spec__.origin` are set to None.
- bpo-32303: Make sure `__spec__.loader` matches`__loader__` for
namespace packages.
- bpo-32711: Fix the warning messages for Python/ast_unparse.c. Patch by StÃ©phane Wirtel
- bpo-32583: Fix possible crashing in builtin Unicode decoders caused by write out-of-bound errors when using customized decode error handlers.

### LibraryÂ¶

- bpo-32960: For dataclasses, disallow inheriting frozen from non-frozen classes, and also disallow inheriting non-frozen from frozen classes. This restriction will be relaxed at a future date.
- bpo-32713: Fixed tarfile.itn handling of out-of-bounds float values. Patch by Joffrey Fuhrer.
- bpo-32951: Direct instantiation of SSLSocket and SSLObject objects is now prohibited. The constructors were never documented, tested, or designed as public constructors. Users were suppose to use ssl.wrap_socket() or SSLContext.
- bpo-32929: Remove the tri-state parameter âhashâ, and add the boolean âunsafe_hashâ. If unsafe_hash is True, add a __hash__ function, but if a __hash__ exists, raise TypeError. If unsafe_hash is False, add a __hash__ based on the values of eq= and frozen=. The unsafe_hash=False behavior is the same as the old hash=None behavior. unsafe_hash=False is the default, just as hash=None used to be.
- bpo-32947: Add OP_ENABLE_MIDDLEBOX_COMPAT and test workaround for TLSv1.3 for future compatibility with OpenSSL 1.1.1.
- bpo-30622: The ssl module now detects missing NPN support in LibreSSL.
- bpo-32922: dbm.open() now encodes filename with the filesystem encoding rather than default encoding.
- bpo-32859: In `os.dup2` , donât check every call whether the`dup3` syscall exists or not.
- bpo-32556: nt._getfinalpathname, nt._getvolumepathname and nt._getdiskusage now correctly convert from bytes.
- bpo-25988: Emit a `DeprecationWarning` when using or importing an ABC
directly from`collections` rather than from`collections.abc` .
- bpo-21060: Rewrite confusing message from setup.py upload from âNo dist file created in earlier commandâ to the more helpful âMust create and upload files in one commandâ.
- bpo-32852: Make sure sys.argv remains as a list when running trace.
- bpo-31333: `_abc` module is added.  It is a speedup module with C
implementations for various functions and methods in`abc` .  Creating an
ABC subclass and calling`isinstance` or`issubclass` with an ABC
subclass are up to 1.5x faster. In addition, this makes Python start-up up
to 10% faster. Note that the new implementation hides internal registry
and caches, previously accessible via private attributes`_abc_registry` ,`_abc_cache` , and`_abc_negative_cache` .  There are
three debugging helper methods that can be used instead`_dump_registry` ,`_abc_registry_clear` , and`_abc_caches_clear` .
- bpo-32841: Fixed `asyncio.Condition` issue which silently ignored
cancellation after notifying and cancelling a conditional lock. Patch by
Bar Harel.
- bpo-32819: ssl.match_hostname() has been simplified and no longer depends on re and ipaddress module for wildcard and IP addresses. Error reporting for invalid wildcards has been improved.
- bpo-32394: socket: Remove TCP_FASTOPEN,TCP_KEEPCNT,TCP_KEEPIDLE,TCP_KEEPINTVL flags on older version Windows during run-time.
- bpo-31787: Fixed refleaks of `__init__()` methods in various modules.
(Contributed by Oren Milman)
- bpo-30157: Fixed guessing quote and delimiter in csv.Sniffer.sniff() when only the last field is quoted. Patch by Jake Davis.
- bpo-32792: collections.ChainMap() preserves the order of the underlying mappings.
- bpo-32775: `fnmatch.translate()` no longer produces patterns which
contain set operations. Sets starting with â[â or containing âââ, â&&â,
â~~â or â||â will be interpreted differently in regular expressions in
future versions. Currently they emit warnings. fnmatch.translate() now
avoids producing patterns containing such sets by accident.
- bpo-32622: Implement native fast sendfile for Windows proactor event loop.
- bpo-32777: Fix a rare but potential pre-exec child process deadlock in subprocess on POSIX systems when marking file descriptors inheritable on exec in the child process. This bug appears to have been introduced in 3.4.
- bpo-32647: The ctypes module used to depend on indirect linking for dlopen. The shared extension is now explicitly linked against libdl on platforms with dl.
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
- bpo-32221: Various functions returning tuple containing IPv6 addresses now omit `%scope` part since the same information is already encoded in*scopeid* tuple item. Especially this speeds up`socket.recvfrom()` when it receives multicast packet since useless resolving of network
interface name is omitted.
- bpo-30693: The TarFile class now recurses directories in a reproducible way.
- bpo-30693: The ZipFile class now recurses directories in a reproducible way.

### DocumentationÂ¶

- bpo-28124: The ssl module function ssl.wrap_socket() has been de-emphasized and deprecated in favor of the more secure and efficient SSLContext.wrap_socket() method.
- bpo-17232: Clarify docs for -O and -OO. Patch by Terry Reedy.
- bpo-32436: Add documentation for the contextvars module (PEP 567).
- bpo-32800: Update link to w3c doc for xml default namespaces.
- bpo-11015: Update `test.support` documentation.
- bpo-8722: Document `__getattr__()` behavior when property`get()` method raises`AttributeError` .
- bpo-32614: Modify RE examples in documentation to use raw strings to prevent `DeprecationWarning` and add text to REGEX HOWTO to highlight
the deprecation.
- bpo-31972: Improve docstrings for `pathlib.PurePath` subclasses.

### TestsÂ¶

- bpo-31809: Add tests to verify connection with secp ECDH curves.

### BuildÂ¶

- bpo-32898: Fix the python debug build when using COUNT_ALLOCS.

### WindowsÂ¶

- bpo-32901: Update Tcl and Tk versions to 8.6.8
- bpo-31966: Fixed WindowsConsoleIO.write() for writing empty data.
- bpo-32409: Ensures activate.bat can handle Unicode contents.
- bpo-32457: Improves handling of denormalized executable path when launching Python.
- bpo-32370: Use the correct encoding for ipconfig output in the uuid module. Patch by Segev Finer.
- bpo-29248: Fix `os.readlink()` on Windows, which was mistakenly
treating the`PrintNameOffset` field of the reparse data buffer as a
number of characters instead of bytes. Patch by Craig Holmquist and SSE4.

### macOSÂ¶

- bpo-32901: Update macOS 10.9+ installer to Tcl/Tk 8.6.8.

### IDLEÂ¶

- bpo-32916: Change `str` to`code` in pyparse.
- bpo-32905: Remove unused code in pyparse module.
- bpo-32874: Add tests for pyparse.
- bpo-32837: Using the system and place-dependent default encoding for open() is a bad idea for IDLEâs system and location-independent files.
- bpo-32826: Add âencoding=utf-8â to open() in IDLEâs test_help_about. GUI test test_file_buttons() only looks at initial ascii-only lines, but failed on systems where open() defaults to âasciiâ because readline() internally reads and decodes far enough ahead to encounter a non-ascii character in CREDITS.txt.
- bpo-32765: Update configdialog General tab docstring to add new widgets to the widget list.

### Tools/DemosÂ¶

- bpo-32222: Fix pygettext not extracting docstrings for functions with type annotated arguments. Patch by Toby Harradine.

## Python 3.7.0 beta 1Â¶

*Release date: 2018-01-30*

### Core and BuiltinsÂ¶

- bpo-32703: Fix coroutineâs ResourceWarning when thereâs an active error set when itâs being finalized.
- bpo-32650: Pdb and other debuggers dependent on bdb.py will correctly step over (next command) native coroutines. Patch by Pablo Galindo.
- bpo-28685: Optimize list.sort() and sorted() by using type specialized comparisons when possible.
- bpo-32685: Improve suggestion when the Python 2 form of print statement is either present on the same line as the header of a compound statement or else terminated by a semi-colon instead of a newline. Patch by Nitish Chandra.
- bpo-32697: Python now explicitly preserves the definition order of keyword-only parameters. Itâs always preserved their order, but this behavior was never guaranteed before; this behavior is now guaranteed and tested.
- bpo-32690: The locals() dictionary now displays in the lexical order that variables were defined. Previously, the order was reversed.
- bpo-32677: Add `.isascii()` method to`str` ,`bytes` and`bytearray` . It can be used to test that string contains only ASCII
characters.
- bpo-32670: Enforce **PEP 479** for all code. This means that manually
raising a StopIteration exception from a generator is prohibited for all
code, regardless of whether âfrom __future__ import generator_stopâ was
used or not.
- bpo-32591: Added built-in support for tracking the origin of coroutine objects; see sys.set_coroutine_origin_tracking_depth and CoroutineType.cr_origin. This replaces the asyncio debug modeâs use of coroutine wrapping for native coroutine objects.
- bpo-31368: Expose preadv and pwritev system calls in the os module. Patch by Pablo Galindo
- bpo-32544: `hasattr(obj, name)` and`getattr(obj, name, default)` are
about 4 times faster than before when`name` is not found and`obj` doesnât override`__getattr__` or`__getattribute__` .
- bpo-26163: Improved frozenset() hash to create more distinct hash values when faced with datasets containing many similar values.
- bpo-32550: Remove the STORE_ANNOTATION bytecode.
- bpo-20104: Expose posix_spawn as a low level API in the os module. (removed before 3.7.0rc1)
- bpo-24340: Fixed estimation of the code stack size.
- bpo-32436: Implement **PEP 567** Context Variables.
- bpo-18533: `repr()` on a dict containing its own`values()` or`items()` no longer raises`RecursionError` ; OrderedDict similarly.
Instead, use`...` , as for other recursive structures.  Patch by Ben
North.
- bpo-20891: Py_Initialize() now creates the GIL. The GIL is no longer created âon demandâ to fix a race condition when PyGILState_Ensure() is called in a non-Python thread.
- bpo-32028: Leading whitespace is now correctly ignored when generating suggestions for converting Py2 print statements to Py3 builtin print function calls. Patch by Sanyam Khurana.
- bpo-31179: Make dict.copy() up to 5.5 times faster.
- bpo-31113: Get rid of recursion in the compiler for normal control flow.

### LibraryÂ¶

- bpo-25988: Deprecate exposing the contents of collections.abc in the regular collections module.
- bpo-31429: The default cipher suite selection of the ssl module now uses a blacklist approach rather than a hard-coded whitelist. Python no longer re-enables ciphers that have been blocked by OpenSSL security update. Default cipher suite selection can be configured on compile time.
- bpo-30306: contextlib.contextmanager now releases the arguments passed to the underlying generator as soon as the context manager is entered. Previously it would keep them alive for as long as the context manager was alive, even when not being used as a function decorator. Patch by Martin Teichmann.
- bpo-21417: Added support for setting the compression level for zipfile.ZipFile.
- bpo-32251: Implement asyncio.BufferedProtocol (provisional API).
- bpo-32513: In dataclasses, allow easier overriding of dunder methods without specifying decorator parameters.
- bpo-32660: `termios` makes available`FIONREAD` ,`FIONCLEX` ,`FIOCLEX` ,`FIOASYNC` and`FIONBIO` also under Solaris/derivatives.
- bpo-27931: Fix email address header parsing error when the username is an empty quoted string. Patch by Xiang Zhang.
- bpo-32659: Under Solaris and derivatives, `os.stat_result` provides
a st_fstype attribute.
- bpo-32662: Implement Server.start_serving(), Server.serve_forever(), and Server.is_serving() methods. Add âstart_servingâ keyword parameter to loop.create_server() and loop.create_unix_server().
- bpo-32391: Implement `asyncio.StreamWriter.wait_closed()` and`asyncio.StreamWriter.is_closing()` methods
- bpo-32643: Make Task._step, Task._wakeup and Future._schedule_callbacks methods private.
- bpo-32630: Refactor decimal module to use contextvars to store decimal context.
- bpo-32622: Add `asyncio.AbstractEventLoop.sendfile()` method.
- bpo-32304: distutilsâ upload command no longer corrupts tar files ending with a CR byte, and no longer tries to convert CR to CRLF in any of the upload text fields.
- bpo-32502: uuid.uuid1 no longer raises an exception if a 64-bit hardware address is encountered.
- bpo-32596: `concurrent.futures` imports`ThreadPoolExecutor` and`ProcessPoolExecutor` lazily (using**PEP 562** ). It makes```
import
asyncio
```
about 15% faster because asyncio uses only`ThreadPoolExecutor` by default.
- bpo-31801: Add `_ignore_` to`Enum` so temporary variables can be used
during class construction without being turned into members.
- bpo-32576: Use queue.SimpleQueue() in places where it can be invoked from a weakref callback.
- bpo-32574: Fix memory leak in asyncio.Queue, when the queue has limited size and it is full, the cancellation of queue.put() can cause a memory leak. Patch by: JosÃ© Melero.
- bpo-32521: The nis module is now compatible with new libnsl and headers location.
- bpo-32467: collections.abc.ValuesView now inherits from collections.abc.Collection.
- bpo-32473: Improve ABCMeta._dump_registry() output readability
- bpo-32102: New argument `capture_output` for subprocess.run
- bpo-32521: glibc has removed Sun RPC. Use replacement libtirpc headers and library in nis module.
- bpo-32493: UUID module fixes build for FreeBSD/OpenBSD
- bpo-32503: Pickling with protocol 4 no longer creates too small frames.
- bpo-29237: Create enum for pstats sorting options
- bpo-32454: Add close(fd) function to the socket module.
- bpo-25942: The subprocess module is now more graceful when handling a Ctrl-C KeyboardInterrupt during subprocess.call, subprocess.run, or a Popen context manager. It now waits a short amount of time for the child (presumed to have also gotten the SIGINT) to exit, before continuing the KeyboardInterrupt exception handling. This still includes a SIGKILL in the call() and run() APIs, but at least the child had a chance first.
- bpo-32433: The hmac module now has hmac.digest(), which provides an optimized HMAC digest.
- bpo-28134: Sockets now auto-detect family, type and protocol from file descriptor by default.
- bpo-32404: Fix bug where `datetime.datetime.fromtimestamp()` did not
call __new__ in`datetime.datetime` subclasses.
- bpo-32403: Improved speed of `datetime.date` and`datetime.datetime` alternate constructors.
- bpo-32228: Ensure that `truncate()` preserves the file position (as
reported by`tell()` ) after writes longer than the buffer size.
- bpo-32410: Implement `loop.sock_sendfile` for asyncio event loop.
- bpo-22908: Added seek and tell to the ZipExtFile class. This only works if the file object used to open the zipfile is seekable.
- bpo-32373: Add socket.getblocking() method.
- bpo-32248: Add `importlib.resources` and`importlib.abc.ResourceReader` as the unified API for reading
resources contained within packages.  Loaders wishing to support resource
reading must implement the`get_resource_reader()` method.  File-based
and zipimport-based loaders both implement these APIs.`importlib.abc.ResourceLoader` is deprecated in favor of these new
APIs.
- bpo-32320: collections.namedtuple() now supports default values.
- bpo-29302: Add contextlib.AsyncExitStack. Patch by Alexander Mohr and Ilya Kulakov.
- bpo-31961: *Removed in Python 3.7.0b2.* The*args* argument of
subprocess.Popen can now be a path-like object. If*args* is given
as a sequence, itâs first element can now be a path-like object as
well.
- bpo-31900: The `locale.localeconv()` function now sets temporarily the`LC_CTYPE` locale to the`LC_NUMERIC` locale to decode`decimal_point` and`thousands_sep` byte strings if they are non-ASCII
or longer than 1 byte, and the`LC_NUMERIC` locale is different than the`LC_CTYPE` locale. This temporary change affects other threads. Same
change for the`str.format()` method when formatting a number
(`int` ,`float` ,`float` and subclasses) with the`n` type (ex:`'{:n}'.format(1234)` ).
- bpo-31853: Use super().method instead of socket.method in SSLSocket. They were there most likely for legacy reasons.
- bpo-31399: The ssl module now uses OpenSSLâs X509_VERIFY_PARAM_set1_host() and X509_VERIFY_PARAM_set1_ip() API to verify hostname and IP addresses. Subject common name fallback can be disabled with SSLContext.hostname_checks_common_name.
- bpo-14976: Add a queue.SimpleQueue class, an unbounded FIFO queue with a reentrant C implementation of put().

### DocumentationÂ¶

- bpo-32724: Add references to some commands in the documentation of Pdb. Patch by StÃ©phane Wirtel
- bpo-32649: Complete the C API documentation, profiling and tracing part with the newly added per-opcode events.
- bpo-17799: Explain real behaviour of sys.settrace and sys.setprofile and their C-API counterparts regarding which type of events are received in each function. Patch by Pablo Galindo Salgado.

### TestsÂ¶

- bpo-32721: Fix test_hashlib to not fail if the _md5 module is not built.
- bpo-28414: Add test cases for IDNA 2003 and 2008 host names. IDNA 2003 internationalized host names are working since bpo-31399 has landed. IDNA 2008 are still broken.
- bpo-32604: Add a new â_xxsubinterpretersâ extension module that exposes the existing subinterpreter C-API and a new cross-interpreter data sharing mechanism. The module is primarily intended for more thorough testing of the existing subinterpreter support. Note that the _xxsubinterpreters module has been removed in 3.7.0rc1.
- bpo-32602: Add test certs and test for ECDSA cert and EC/RSA dual mode.
- bpo-32549: On Travis CI, Python now Compiles and uses a local copy of OpenSSL 1.1.0g for testing.

### BuildÂ¶

- bpo-32635: Fix segfault of the crypt module when libxcrypt is provided instead of libcrypt at the system.
- bpo-32598: Use autoconf to detect OpenSSL libs, headers and supported features. The ax_check_openssl M4 macro uses pkg-config to locate OpenSSL and falls back to manual search.
- bpo-32593: Drop support of FreeBSD 9 and older.
- bpo-29708: If the `SOURCE_DATE_EPOCH` environment variable is set,`py_compile` will always create hash-based`.pyc` files.

### WindowsÂ¶

### macOSÂ¶

- bpo-32726: Provide an additional, more modern macOS installer variant that supports macOS 10.9+ systems in 64-bit mode only. Upgrade the supplied third-party libraries to OpenSSL 1.1.0g and to SQLite 3.22.0. The 10.9+ installer now links with and supplies its own copy of Tcl/Tk 8.6.
- bpo-28440: No longer add /Library/Python/3.x/site-packages to sys.path for macOS framework builds to avoid future conflicts.

### C APIÂ¶

## Python 3.7.0 alpha 4Â¶

*Release date: 2018-01-08*

### Core and BuiltinsÂ¶

- bpo-31975: The default warning filter list now starts with a âdefault::DeprecationWarning:__main__â entry, so deprecation warnings are once again shown by default in single-file scripts and at the interactive prompt.
- bpo-32226: `__class_getitem__` is now an automatic class method.
- bpo-32399: Add AIX uuid library support for RFC4122 using uuid_create() in libc.a
- bpo-32390: Fix the compilation failure on AIX after the f_fsid field has been added to the object returned by os.statvfs() (bpo-32143). Original patch by Michael Felt.
- bpo-32379: Make MRO computation faster when a class inherits from a single base.
- bpo-32259: The error message of a TypeError raised when unpack non-iterable is now more specific.
- bpo-27169: The `__debug__` constant is now optimized out at compile
time. This fixes also bpo-22091.
- bpo-32329: The `-R` option now turns on hash randomization when
the`PYTHONHASHSEED` environment variable is set to`0` .
Previously, the option was ignored. Moreover,`sys.flags.hash_randomization` is now properly set to 0 when hash
randomization is turned off by`PYTHONHASHSEED=0` .
- bpo-30416: The optimizer is now protected from spending much time doing complex calculations and consuming much memory for creating large constants in constant folding. Increased limits for constants that can be produced in constant folding.
- bpo-32282: Fix an unnecessary ifdef in the include of VersionHelpers.h in socketmodule on Windows.
- bpo-30579: Implement TracebackType.__new__ to allow Python-level creation of traceback objects, and make TracebackType.tb_next mutable.
- bpo-32260: Donât byte swap the input keys to the SipHash algorithm on big-endian platforms. This should ensure siphash gives consistent results across platforms.
- bpo-31506: Improve the error message logic for object.__new__ and object.__init__. Patch by Sanyam Khurana.
- bpo-20361: `-b` and`-bb` now inject`'default::BytesWarning'` and`error::BytesWarning` entries into`sys.warnoptions` , ensuring that
they take precedence over any other warning filters configured via the`-W` option or the`PYTHONWARNINGS` environment variable.
- bpo-32230: `-X dev` now injects a`'default'` entry into
sys.warnoptions, ensuring that it behaves identically to actually passing`-Wdefault` at the command line.
- bpo-29240: Add a new UTF-8 mode: implementation of the **PEP 540** .
- bpo-32226: **PEP 560** : Add support for`__mro_entries__` and`__class_getitem__` . Implemented by Ivan Levkivskyi.
- bpo-32225: **PEP 562** : Add support for module`__getattr__` and`__dir__` . Implemented by Ivan Levkivskyi.
- bpo-31901: The `atexit` module now has its callback stored per
interpreter.
- bpo-31650: Implement **PEP 552** (Deterministic pycs). Python now supports
invalidating bytecode cache files bashed on a source content hash rather
than source last-modified time.
- bpo-29469: Move constant folding from bytecode layer to AST layer. Original patch by Eugene Toder.

### LibraryÂ¶

- bpo-32506: Now that dict is defined as keeping insertion order, drop OrderedDict and just use plain dict.
- bpo-32279: Add params to dataclasses.make_dataclasses(): init, repr, eq, order, hash, and frozen. Pass them through to dataclass().
- bpo-32278: Make type information optional on dataclasses.make_dataclass(). If omitted, the string âtyping.Anyâ is used.
- bpo-32499: Add dataclasses.is_dataclass(obj), which returns True if obj is a dataclass or an instance of one.
- bpo-32468: Improve frame repr() to mention filename, code name and current line number.
- bpo-23749: asyncio: Implement loop.start_tls()
- bpo-32441: Return the new file descriptor (i.e., the second argument) from `os.dup2` . Previously,`None` was always returned.
- bpo-32422: `functools.lru_cache` uses less memory (3 words for each
cached key) and takes about 1/3 time for cyclic GC.
- bpo-31721: Prevent Python crash from happening when Future._log_traceback is set to True manually. Now it can only be set to False, or a ValueError is raised.
- bpo-32415: asyncio: Add Task.get_loop() and Future.get_loop()
- bpo-26133: Donât unsubscribe signals in asyncio UNIX event loop on interpreter shutdown.
- bpo-32363: Make asyncio.Task.set_exception() and set_result() raise NotImplementedError. Task._step() and Future.__await__() raise proper exceptions when they are in an invalid state, instead of raising an AssertionError.
- bpo-32357: Optimize asyncio.iscoroutine() and loop.create_task() for non-native coroutines (e.g. async/await compiled with Cython). âloop.create_task(python_coroutine)â used to be 20% faster than âloop.create_task(cython_coroutine)â. Now, the latter is as fast.
- bpo-32356: asyncio.transport.resume_reading() and pause_reading() are now idempotent. New transport.is_reading() method is added.
- bpo-32355: Optimize asyncio.gather(); now up to 15% faster.
- bpo-32351: Use fastpath in asyncio.sleep if delay<0 (2x boost)
- bpo-32348: Optimize asyncio.Future schedule/add/remove callback. The optimization shows 3-6% performance improvements of async/await code.
- bpo-32331: Fix socket.settimeout() and socket.setblocking() to keep socket.type as is. Fix socket.socket() constructor to reset any bit flags applied to socketâs type. This change only affects OSes that have SOCK_NONBLOCK and/or SOCK_CLOEXEC.
- bpo-32248: Add `importlib.abc.ResourceReader` as an ABC for
loaders to provide a unified API for reading resources contained within
packages.  Also add`importlib.resources` as the port of`importlib_resources` .
- bpo-32311: Implement asyncio.create_task(coro) shortcut
- bpo-32327: Convert asyncio functions that were documented as coroutines to coroutines. Affected functions: loop.sock_sendall, loop.sock_recv, loop.sock_accept, loop.getaddrinfo, loop.getnameinfo.
- bpo-32323: `urllib.parse.urlsplit()` does not convert zone-id (scope)
to lower case for scoped IPv6 addresses in hostnames now.
- bpo-32302: Fix bdist_wininst of distutils for CRT v142: it binary compatible with CRT v140.
- bpo-29711: Fix `stop_serving` in asyncio proactor loop kill all
listening servers
- bpo-32308: `re.sub()` now replaces empty matches adjacent to a
previous non-empty match.
- bpo-29970: Abort asyncio SSLProtocol connection if handshake not complete within 10 seconds.
- bpo-32314: Implement asyncio.run().
- bpo-17852: Revert incorrect fix based on misunderstanding of _Py_PyAtExit() semantics.
- bpo-32296: Implement asyncio._get_running_loop() and get_event_loop() in C. This makes them 4x faster.
- bpo-32250: Implement `asyncio.current_task()` and`asyncio.all_tasks()` . Add helpers intended to be used by alternative
task implementations:`asyncio._register_task` ,`asyncio._enter_task` ,`asyncio._leave_task` and`asyncio._unregister_task` . Deprecate`asyncio.Task.current_task()` and`asyncio.Task.all_tasks()` .
- bpo-32255: A single empty field is now always quoted when written into a CSV file. This allows to distinguish an empty row from a row consisting of a single empty field. Patch by Licht Takeuchi.
- bpo-32277: Raise `NotImplementedError` instead of`SystemError` on
platforms where`chmod(..., follow_symlinks=False)` is not supported.
Patch by Anthony Sottile.
- bpo-30050: New argument warn_on_full_buffer to signal.set_wakeup_fd lets you control whether Python prints a warning on stderr when the wakeup fd buffer overflows.
- bpo-29137: The `fpectl` library has been removed. It was never enabled
by default, never worked correctly on x86-64, and it changed the Python
ABI in ways that caused unexpected breakage of C extensions.
- bpo-32273: Move asyncio.test_utils to test.test_asyncio.
- bpo-32272: Remove asyncio.async() function.
- bpo-32269: Add asyncio.get_running_loop() function.
- bpo-32265: All class and static methods of builtin types now are correctly classified by inspect.classify_class_attrs() and grouped in pydoc output. Added types.ClassMethodDescriptorType for unbound class methods of builtin types.
- bpo-32253: Deprecate `yield from lock` ,`await lock` ,```
with (yield
from lock)
```
and`with await lock` for asyncio synchronization
primitives.
- bpo-22589: Changed MIME type of .bmp from âimage/x-ms-bmpâ to âimage/bmpâ
- bpo-32193: Convert asyncio to use *async/await* syntax. Old styled```
yield
from
```
is still supported too.
- bpo-32206: Add support to run modules with pdb
- bpo-32227: `functools.singledispatch` now supports registering
implementations using type annotations.
- bpo-15873: Added new alternate constructors `datetime.datetime.fromisoformat()` ,`datetime.time.fromisoformat()` and`datetime.date.fromisoformat()` as the inverse operation of each
classesâs respective`isoformat` methods.
- bpo-32199: The getnode() ip getter now uses âip linkâ instead of âip link listâ.
- bpo-32143: os.statvfs() includes the f_fsid field from statvfs(2)
- bpo-26439: Fix ctypes.util.find_library() for AIX by implementing ctypes._aix.find_library() Patch by: Michael Felt
- bpo-31993: The pickler now uses less memory when serializing large bytes and str objects into a file. Pickles created with protocol 4 will require less memory for unpickling large bytes and str objects.
- bpo-27456: Ensure TCP_NODELAY is set on Linux. Tests by Victor Stinner.
- bpo-31778: ast.literal_eval() is now more strict. Addition and subtraction of arbitrary numbers no longer allowed.
- bpo-31802: Importing native path module ( `posixpath` ,`ntpath` ) now
works even if the`os` module still is not imported.
- bpo-30241: Add contextlib.AbstractAsyncContextManager. Patch by Jelle Zijlstra.
- bpo-31699: Fix deadlocks in `concurrent.futures.ProcessPoolExecutor` when task arguments or
results cause pickling or unpickling errors. This should make sure that
calls to the`ProcessPoolExecutor` API always eventually return.
- bpo-15216: `TextIOWrapper.reconfigure()` supports changing*encoding* ,*errors* , and*newline* .

### DocumentationÂ¶

- bpo-32418: Add get_loop() method to Server and AbstractServer classes.

### TestsÂ¶

### WindowsÂ¶

- bpo-19764: Implement support for `subprocess.Popen(close_fds=True)` on
Windows. Patch by Segev Finer.

### Tools/DemosÂ¶

- bpo-24960: 2to3 and lib2to3 can now read pickled grammar files using pkgutil.get_data() rather than probing the filesystem. This lets 2to3 and lib2to3 work when run from a zipfile.

### C APIÂ¶

- bpo-32030: Py_Initialize() doesnât reset the memory allocators to default if the `PYTHONMALLOC` environment variable is not set.
- bpo-29084: Undocumented C API for OrderedDict has been excluded from the limited C API. It was added by mistake and actually never worked in the limited C API.
- bpo-32264: Moved the pygetopt.h header into internal/, since it has no public APIs.
- bpo-32241: `Py_SetProgramName()` and`Py_SetPythonHome()` now take the`const wchar *` arguments instead of`wchar *` .

## Python 3.7.0 alpha 3Â¶

*Release date: 2017-12-05*

### Core and BuiltinsÂ¶

- bpo-32176: co_flags.CO_NOFREE is now always set correctly by the code object constructor based on freevars and cellvars, rather than needing to be set correctly by the caller. This ensures it will be cleared automatically when additional cell references are injected into a modified code object and function.
- bpo-10544: Yield expressions are now deprecated in comprehensions and generator expressions. They are still permitted in the definition of the outermost iterable, as that is evaluated directly in the enclosing scope.
- bpo-32137: The repr of deeply nested dict now raises a RecursionError instead of crashing due to a stack overflow.
- bpo-32096: Revert memory allocator changes in the C API: move structures back from _PyRuntime to Objects/obmalloc.c. The memory allocators are once again initialized statically, and so PyMem_RawMalloc() and Py_DecodeLocale() can be called before _PyRuntime_Initialize().
- bpo-32043: Add a new âdeveloper modeâ: new â-X devâ command line option to enable debug checks at runtime.
- bpo-32023: SyntaxError is now correctly raised when a generator expression without parenthesis is used instead of an inheritance list in a class definition. The duplication of the parentheses can be omitted only on calls.
- bpo-32012: SyntaxError is now correctly raised when a generator expression without parenthesis is passed as an argument, but followed by a trailing comma. A generator expression always needs to be directly inside a set of parentheses and cannot have a comma on either side.
- bpo-28180: A new internal `_Py_SetLocaleFromEnv(category)` helper
function has been added in order to improve the consistency of behaviour
across different`libc` implementations (e.g. Android doesnât support
setting the locale from the environment by default).
- bpo-31949: Fixed several issues in printing tracebacks (PyTraceBack_Print()). Setting sys.tracebacklimit to 0 or less now suppresses printing tracebacks. Setting sys.tracebacklimit to None now causes using the default limit. Setting sys.tracebacklimit to an integer larger than LONG_MAX now means using the limit LONG_MAX rather than the default limit. Fixed integer overflows in the case of more than `2**31` traceback items on Windows. Fixed output errors handling.
- bpo-30696: Fix the interactive interpreter looping endlessly when no memory.
- bpo-20047: Bytearray methods partition() and rpartition() now accept only bytes-like objects as separator, as documented. In particular they now raise TypeError rather of returning a bogus result when an integer is passed as a separator.
- bpo-21720: BytesWarning no longer emitted when the *fromlist* argument of`__import__()` or the`__all__` attribute of the module contain bytes
instances.
- bpo-31845: Environment variables are once more read correctly at interpreter startup.
- bpo-28936: Ensure that lexically first syntax error involving a parameter and `global` or`nonlocal` is detected first at a given scope. Patch
by Ivan Levkivskyi.
- bpo-31825: Fixed OverflowError in the âunicode-escapeâ codec and in codecs.escape_decode() when decode an escaped non-ascii byte.
- bpo-31618: The per-frame tracing logic added in 3.7a1 has been altered so that `frame->f_lineno` is updated before either`"line"` or`"opcode"` events are emitted. Previously, opcode events were emitted
first, and therefore would occasionally see stale line numbers on the
frame. The behavior of this feature has changed slightly as a result: when
both`f_trace_lines` and`f_trace_opcodes` are enabled, line events
now occur first.
- bpo-28603: Print the full context/cause chain of exceptions on interpreter exit, even if an exception in the chain is unhashable or compares equal to later ones. Patch by Zane Bitter.
- bpo-31786: Fix timeout rounding in the select module to round correctly negative timeouts between -1.0 and 0.0. The functions now block waiting for events as expected. Previously, the call was incorrectly non-blocking. Patch by Pablo Galindo.
- bpo-31781: Prevent crashes when calling methods of an uninitialized `zipimport.zipimporter` object. Patch by Oren Milman.
- bpo-30399: Standard repr() of BaseException with a single argument no longer contains redundant trailing comma.
- bpo-31626: Fixed a bug in debug memory allocator. There was a write to freed memory after shrinking a memory block.
- bpo-30817: `PyErr_PrintEx()` clears now the ignored exception that may
be raised by`_PySys_SetObjectId()` , for example when no memory.

### LibraryÂ¶

- bpo-28556: Two minor fixes for `typing` module: allow shallow copying
instances of generic classes, improve interaction of`__init_subclass__` with generics. Original PRs by Ivan Levkivskyi.
- bpo-32214: PEP 557, Data Classes. Provides a decorator which adds boilerplate methods to classes which use type annotations so specify fields.
- bpo-27240: The header folding algorithm for the new email policies has been rewritten, which also fixes bpo-30788, bpo-31831, and bpo-32182. In particular, RFC2231 folding is now done correctly.
- bpo-32186: io.FileIO.readall() and io.FileIO.read() now release the GIL when getting the file size. Fixed hang of all threads with inaccessible NFS server. Patch by Nir Soffer.
- bpo-32101: Add `sys.flags.dev_mode` flag
- bpo-32154: The `asyncio.windows_utils.socketpair()` function has been
removed: use directly`socket.socketpair()` which is available on all
platforms since Python 3.5 (before, it wasnât available on Windows).`asyncio.windows_utils.socketpair()` was just an alias to`socket.socketpair` on Python 3.5 and newer.
- bpo-32089: warnings: In development (-X dev) and debug mode (pydebug build), use the âdefaultâ action for ResourceWarning, rather than the âalwaysâ action, in the default warnings filters.
- bpo-32107: `uuid.getnode()` now preferentially returns universally
administered MAC addresses if available, over locally administered MAC
addresses.  This makes a better guarantee for global uniqueness of UUIDs
returned from`uuid.uuid1()` .  If only locally administered MAC
addresses are available, the first such one found is returned.
- bpo-23033: Wildcard is now supported in hostname when it is one and only character in the left most segment of hostname in second argument of `ssl.match_hostname()` .  Patch by Mandeep Singh.
- bpo-12239: Make `msilib.SummaryInformation.GetProperty()` return`None` when the value of property is`VT_EMPTY` .  Initial patch by
Mark Mc Mahon.
- bpo-28334: Use `os.path.expanduser()` to find the`~/.netrc` file in`netrc.netrc` .  If it does not exist,`FileNotFoundError` is
raised.  Patch by Dimitri Merejkowsky.
- bpo-32121: Made `tracemalloc.Traceback` behave more like the traceback
module, sorting the frames from oldest to most recent.`Traceback.format()` now accepts negative*limit* , truncating the result
to the`abs(limit)` oldest frames. To get the old behaviour, one can use
the new*most_recent_first* argument to`Traceback.format()` . (Patch by
Jesse Bakker.)
- bpo-31325: Fix wrong usage of `collections.namedtuple()` in the`RobotFileParser.parse()` method. Initial patch by Robin Wellner.
- bpo-12382: `msilib.OpenDatabase()` now raises a better exception
message when it couldnât open or create an MSI file.  Initial patch by
William TisÃ¤ter.
- bpo-19610: `setup()` now warns about invalid types for some fields. The`distutils.dist.Distribution` class now warns when`classifiers` ,`keywords` and`platforms` fields are not specified as a list or a
string.
- bpo-32071: Added the `-k` command-line option to`python -m unittest` to run only tests that match the given pattern(s).
- bpo-10049: Added *nullcontext* no-op context manager to contextlib. This
provides a simpler and faster alternative to ExitStack() when handling
optional context managers.
- bpo-28684: The new test.support.skip_unless_bind_unix_socket() decorator is used here to skip asyncio tests that fail because the platform lacks a functional bind() function for unix domain sockets (as it is the case for non root users on the recent Android versions that run now SELinux in enforcing mode).
- bpo-32110: `codecs.StreamReader.read(n)` now returns not more than*n* characters/bytes for non-negative*n* . This makes it compatible with`read()` methods of other file-like objects.
- bpo-27535: The warnings module doesnât leak memory anymore in the hidden warnings registry for the âignoreâ action of warnings filters. warn_explicit() function doesnât add the warning key to the registry anymore for the âignoreâ action.
- bpo-32088: warnings: When Python is build is debug mode ( `Py_DEBUG` ),`DeprecationWarning` ,`PendingDeprecationWarning` and`ImportWarning` warnings are now displayed by default.
- bpo-1647489: Fixed searching regular expression patterns that could match an empty string. Non-empty string can now be correctly found after matching an empty string.
- bpo-25054: Added support of splitting on a pattern that could match an empty string.
- bpo-32072: Fixed issues with binary plists: Fixed saving bytearrays. Identical objects will be saved only once. Equal references will be load as identical objects. Added support for saving and loading recursive data structures.
- bpo-32069: Drop legacy SSL transport from asyncio, ssl.MemoryBIO is always used anyway.
- bpo-32066: asyncio: Support pathlib.Path in create_unix_connection; sock arg should be optional
- bpo-32046: Updates 2to3 to convert from operator.isCallable(obj) to callable(obj). Patch by Donghee Na.
- bpo-32018: inspect.signature should follow **PEP 8** , if the parameter has
an annotation and a default value. Patch by Donghee Na.
- bpo-32025: Add time.thread_time() and time.thread_time_ns()
- bpo-32037: Integers that fit in a signed 32-bit integer will be now pickled with protocol 0 using the INT opcode. This will decrease the size of a pickle, speed up pickling and unpickling, and make these integers be unpickled as int instances in Python 2.
- bpo-32034: Make asyncio.IncompleteReadError and LimitOverrunError pickleable.
- bpo-32015: Fixed the looping of asyncio in the case of reconnection the socket during waiting async read/write from/to the socket.
- bpo-32011: Restored support of loading marshal files with the TYPE_INT64 code. These files can be produced in Python 2.7.
- bpo-28369: Enhance add_reader/writer check that socket is not used by some transport. Before, only cases when add_reader/writer were called with an int FD were supported. Now the check is implemented correctly for all file-like objects.
- bpo-31976: Fix race condition when flushing a file is slow, which can cause a segfault if closing the file from another thread.
- bpo-31985: Formally deprecated aifc.openfp, sunau.openfp, and wave.openfp. Since change 7bc817d5ba917528e8bd07ec461c635291e7b06a in 1993, openfp in each of the three modules had been pointing to that moduleâs open function as a matter of backwards compatibility, though it had been both untested and undocumented.
- bpo-21862: cProfile command line now accepts `-m module_name` as an
alternative to script path. Patch by Sanyam Khurana.
- bpo-31970: Reduce performance overhead of asyncio debug mode.
- bpo-31843: *database* argument of sqlite3.connect() now accepts a
path-like object, instead of just a string.
- bpo-31945: Add Configurable *blocksize* to`HTTPConnection` and`HTTPSConnection` for improved upload throughput.  Patch by Nir Soffer.
- bpo-31943: Add a `cancelled()` method to`asyncio.Handle` .  Patch
by Marat Sharafutdinov.
- bpo-9678: Fixed determining the MAC address in the uuid module: Using ifconfig on NetBSD and OpenBSD. Using arp on Linux, FreeBSD, NetBSD and OpenBSD. Based on patch by Takayuki Shimizukawa.
- bpo-30057: Fix potential missed signal in signal.signal().
- bpo-31933: Fix Blake2 params leaf_size and node_offset on big endian platforms. Patch by Jack OâConnor.
- bpo-21423: Add an initializer argument to {Process,Thread}PoolExecutor
- bpo-31927: Fixed compilation of the socket module on NetBSD 8. Fixed assertion failure or reading arbitrary data when parse a AF_BLUETOOTH address on NetBSD and DragonFly BSD.
- bpo-27666: Fixed stack corruption in curses.box() and curses.ungetmouse() when the size of types chtype or mmask_t is less than the size of C long. curses.box() now accepts characters as arguments. Based on patch by Steve Fink.
- bpo-31917: Add 3 new clock identifiers: `time.CLOCK_BOOTTIME` ,`time.CLOCK_PROF` and`time.CLOCK_UPTIME` .
- bpo-31897: plistlib now catches more errors when read binary plists and raises InvalidFileException instead of unexpected exceptions.
- bpo-25720: Fix the method for checking pad state of curses WINDOW. Patch by Masayuki Yamamoto.
- bpo-31893: Fixed the layout of the kqueue_event structure on OpenBSD and NetBSD. Fixed the comparison of the kqueue_event objects.
- bpo-31891: Fixed building the curses module on NetBSD.
- bpo-31884: added required constants to subprocess module for setting priority on windows
- bpo-28281: Remove year (1-9999) limits on the Calendar.weekday() function. Patch by Mark Gollahon.
- bpo-31702: crypt.mksalt() now allows to specify the number of rounds for SHA-256 and SHA-512 hashing.
- bpo-30639: `inspect.getfile()` no longer computes the repr of unknown
objects to display in an error message, to protect against badly behaved
custom reprs.
- bpo-30768: Fix the pthread+semaphore implementation of PyThread_acquire_lock_timed() when called with timeout > 0 and intr_flag=0: recompute the timeout if sem_timedwait() is interrupted by a signal (EINTR). See also the **PEP 475** .
- bpo-31854: Add `mmap.ACCESS_DEFAULT` constant.
- bpo-31834: Use optimized code for BLAKE2 only with SSSE3+. The pure SSE2 implementation is slower than the pure C reference implementation.
- bpo-28292: Calendar.itermonthdates() will now consistently raise an exception when a date falls outside of the 0001-01-01 through 9999-12-31 range. To support applications that cannot tolerate such exceptions, the new methods itermonthdays3() and itermonthdays4() are added. The new methods return tuples and are not restricted by the range supported by datetime.date.
- bpo-28564: The shutil.rmtree() function has been sped up to 20â40%. This was done using the os.scandir() function.
- bpo-28416: Instances of pickle.Pickler subclass with the persistent_id() method and pickle.Unpickler subclass with the persistent_load() method no longer create reference cycles.
- bpo-31653: Donât release the GIL if we can acquire a multiprocessing semaphore immediately.
- bpo-28326: Fix multiprocessing.Process when stdout and/or stderr is closed or None.
- bpo-20825: Add `subnet_of` and`superset_of` containment tests to`ipaddress.IPv6Network` and`ipaddress.IPv4Network` . Patch
by Michel Albert and Cheryl Sabella.
- bpo-31827: Remove the os.stat_float_times() function. It was introduced in Python 2.3 for backward compatibility with Python 2.2, and was deprecated since Python 3.1.
- bpo-31756: Add a `subprocess.Popen(text=False)` keyword argument to`subprocess` functions to be more explicit about when the library should
attempt to decode outputs into text. Patch by Andrew Clegg.
- bpo-31819: Add AbstractEventLoop.sock_recv_into().
- bpo-31457: If nested log adapters are used, the inner `process()` methods are no longer omitted.
- bpo-31457: The `manager` property on LoggerAdapter objects is now
properly settable.
- bpo-31806: Fix timeout rounding in time.sleep(), threading.Lock.acquire() and socket.socket.settimeout() to round correctly negative timeouts between -1.0 and 0.0. The functions now block waiting for events as expected. Previously, the call was incorrectly non-blocking. Patch by Pablo Galindo.
- bpo-31803: time.clock() and time.get_clock_info(âclockâ) now emit a DeprecationWarning warning.
- bpo-31800: Extended support for parsing UTC offsets. strptime â%zâ can now parse the output generated by datetime.isoformat, including seconds and microseconds.
- bpo-28603: traceback: Fix a TypeError that occurred during printing of exception tracebacks when either the current exception or an exception in its context/cause chain is unhashable. Patch by Zane Bitter.
- bpo-30541: Add new function to seal a mock and prevent the automatically creation of child mocks. Patch by Mario Corchero.
- bpo-31784: Implement the **PEP 564** , add new 6 new functions with
nanosecond resolution to the`time` module:`clock_gettime_ns()` ,`clock_settime_ns()` ,`monotonic_ns()` ,`perf_counter_ns()` ,`process_time_ns()` ,`time_ns()` .
- bpo-30143: 2to3 now generates a code that uses abstract collection classes from collections.abc rather than collections.
- bpo-31770: Prevent a crash when calling the `__init__()` method of a`sqlite3.Cursor` object more than once. Patch by Oren Milman.
- bpo-31764: Prevent a crash in `sqlite3.Cursor.close()` in case the`Cursor` object is uninitialized. Patch by Oren Milman.
- bpo-31752: Fix possible crash in timedelta constructor called with custom integers.
- bpo-31620: an empty asyncio.Queue now doesnât leak memory when queue.get pollers timeout
- bpo-31690: Allow the flags re.ASCII, re.LOCALE, and re.UNICODE to be used as group flags for regular expressions.
- bpo-30349: FutureWarning is now emitted if a regular expression contains character set constructs that will change semantically in the future (nested sets and set operations).
- bpo-31664: Added support for the Blowfish hashing in the crypt module.
- bpo-31632: Fix method set_protocol() of class _SSLProtocolTransport in asyncio module. This method was previously modifying a wrong reference to the protocol.
- bpo-15037: Added a workaround for getkey() in curses for ncurses 5.7 and earlier.
- bpo-31307: Allow use of bytes objects for arguments to `configparser.ConfigParser.read()` . Patch by Vincent Michel.
- bpo-31334: Fix `poll.poll([timeout])` in the`select` module for
arbitrary negative timeouts on all OSes where it can only be a
non-negative integer or -1. Patch by Riccardo Coccioli.
- bpo-31310: multiprocessingâs semaphore tracker should be launched again if crashed.
- bpo-31308: Make multiprocessingâs forkserver process immune to Ctrl-C and other user interruptions. If it crashes, restart it when necessary.
- bpo-31245: Added support for AF_UNIX socket in asyncio `create_datagram_endpoint` .
- bpo-30553: Add HTTP/2 status code 421 (Misdirected Request) to `http.HTTPStatus` . Patch by Vitor Pereira.

### DocumentationÂ¶

- bpo-32105: Added asyncio.BaseEventLoop.connect_accepted_socket versionadded marker.

### TestsÂ¶

- bpo-31380: Skip test_httpservers test_undecodable_file on macOS: fails on APFS.
- bpo-31705: Skip test_socket.test_sha256() on Linux kernel older than 4.5. The test fails with ENOKEY on kernel 3.10 (on ppc64le). A fix was merged into the kernel 4.5.
- bpo-32138: Skip on Android test_faulthandler tests that raise SIGSEGV and remove the test.support.requires_android_level decorator.
- bpo-32136: The runtime embedding tests have been split out from `Lib/test/test_capi.py` into a new`Lib/test/test_embed.py` file.
- bpo-28668: test.support.requires_multiprocessing_queue is removed. Skip tests with test.support.import_module(âmultiprocessing.synchronizeâ) instead when the semaphore implementation is broken or missing.
- bpo-32126: Skip test_get_event_loop_new_process in test.test_asyncio.test_events when sem_open() is not functional.
- bpo-31174: Fix test_tools.test_unparse: DirectoryTestCase now stores the names sample to always test the same files. It prevents false alarms when hunting reference leaks.

### BuildÂ¶

- bpo-28538: Revert the previous changes, the if_nameindex structure is defined by Unified Headers.
- bpo-28762: Revert the last commit, the F_LOCK macro is defined by Android Unified Headers.
- bpo-29040: Support building Android with Unified Headers. The first NDK release to support Unified Headers is android-ndk-r14.
- bpo-32059: `detect_modules()` in`setup.py` now also searches the
sysroot paths when cross-compiling.
- bpo-31957: Fixes Windows SDK version detection when building for Windows.
- bpo-31609: Fixes quotes in PCbuild/clean.bat
- bpo-31934: Abort the build when building out of a not clean source tree.
- bpo-31926: Fixed Argument Clinic sometimes causing compilation errors when there was more than one function and/or method in a .c file with the same name.
- bpo-28791: Update Windows builds to use SQLite 3.21.0.
- bpo-28791: Update OS X installer to use SQLite 3.21.0.
- bpo-28643: Record profile-opt build progress with stamp files.
- bpo-31866: Finish removing support for AtheOS.

### WindowsÂ¶

- bpo-1102: Return `None` when`View.Fetch()` returns`ERROR_NO_MORE_ITEMS` instead of raising`MSIError` . Initial patch by
Anthony Tuininga.
- bpo-31944: Fixes Modify button in Apps and Features dialog.
- bpo-20486: Implement the `Database.Close()` method to help closing MSI
database objects.
- bpo-31857: Make the behavior of USE_STACKCHECK deterministic in a multi-threaded environment.

### macOSÂ¶

- bpo-31392: Update macOS installer to use OpenSSL 1.0.2m

### IDLEÂ¶

- bpo-32207: Improve tk event exception tracebacks in IDLE. When tk event handling is driven by IDLEâs run loop, a confusing and distracting queue.EMPTY traceback context is no longer added to tk event exception tracebacks. The traceback is now the same as when event handling is driven by user code. Patch based on a suggestion by Serhiy Storchaka.
- bpo-32164: Delete unused file idlelib/tabbedpages.py. Use of TabbedPageSet in configdialog was replaced by ttk.Notebook.
- bpo-32100: IDLE: Fix old and new bugs in pathbrowser; improve tests. Patch mostly by Cheryl Sabella.
- bpo-31858: IDLE â Restrict shell prompt manipulation to the shell. Editor and output windows only see an empty last prompt line. This simplifies the code and fixes a minor bug when newline is inserted. Sys.ps1, if present, is read on Shell start-up, but is not set or changed.
- bpo-31860: The font sample in the IDLE configuration dialog is now editable. Changes persist while IDLE remains open
- bpo-31836: Test_code_module now passes if run after test_idle, which sets ps1. The code module uses sys.ps1 if present or sets it to â>>> â if not. Test_code_module now properly tests both behaviors. Ditto for ps2.
- bpo-28603: Fix a TypeError that caused a shell restart when printing a traceback that includes an exception that is unhashable. Patch by Zane Bitter.
- bpo-13802: Use non-Latin characters in the IDLEâs Font settings sample. Even if one selects a font that defines a limited subset of the unicode Basic Multilingual Plane, tcl/tk will use other fonts that define a character. The expanded example give users of non-Latin characters a better idea of what they might see in IDLEâs shell and editors. To make room for the expanded sample, frames on the Font tab are re-arranged. The Font/Tabs help explains a bit about the additions.

### Tools/DemosÂ¶

- bpo-32159: Remove CVS and Subversion tools: remove svneol.py and treesync.py scripts. CPython migrated from CVS to Subversion, to Mercurial, and then to Git. CVS and Subversion are no longer used to develop CPython.
- bpo-30722: Make redemo work with Python 3.6 and newer versions. Also, remove the `LOCALE` option since it doesnât work with string patterns in
Python 3. Patch by Christoph Sarnowski.

### C APIÂ¶

- bpo-20891: Fix PyGILState_Ensure(). When PyGILState_Ensure() is called in a non-Python thread before PyEval_InitThreads(), only call PyEval_InitThreads() after calling PyThreadState_New() to fix a crash.
- bpo-32125: The `Py_UseClassExceptionsFlag` flag has been removed. It was
deprecated and wasnât used anymore since Python 2.0.
- bpo-25612: Move the current exception state from the frame object to the co-routine. This simplifies the interpreter and fixes a couple of obscure bugs caused by having swap exception state when entering or exiting a generator.
- bpo-23699: Add Py_RETURN_RICHCOMPARE macro to reduce boilerplate code in rich comparison functions.
- bpo-30697: The `PyExc_RecursionErrorInst` singleton is removed and`PyErr_NormalizeException()` does not use it anymore. This singleton is
persistent and its members being never cleared may cause a segfault during
finalization of the interpreter. See also bpo-22898.

## Python 3.7.0 alpha 2Â¶

*Release date: 2017-10-16*

### Core and BuiltinsÂ¶

- bpo-31558: `gc.freeze()` is a new API that allows for moving all objects
currently tracked by the garbage collector to a permanent generation,
effectively removing them from future collection events. This can be used
to protect those objects from having their PyGC_Head mutated. In effect,
this enables great copy-on-write stability at fork().
- bpo-31642: Restored blocking âfrom package import moduleâ by setting sys.modules[âpackage.moduleâ] to None.
- bpo-31708: Allow use of asynchronous generator expressions in synchronous functions.
- bpo-31709: Drop support of asynchronous __aiter__.
- bpo-30404: The -u option now makes the stdout and stderr streams unbuffered rather than line-buffered.
- bpo-31619: Fixed a ValueError when convert a string with large number of underscores to integer with binary base.
- bpo-31602: Fix an assertion failure in `zipimporter.get_source()` in
case of a bad`zlib.decompress()` . Patch by Oren Milman.
- bpo-31592: Fixed an assertion failure in Python parser in case of a bad `unicodedata.normalize()` . Patch by Oren Milman.
- bpo-31588: Raise a `TypeError` with a helpful error message when class
creation fails due to a metaclass with a bad`__prepare__()` method.
Patch by Oren Milman.
- bpo-31574: Importlib was instrumented with two dtrace probes to profile import timing.
- bpo-31566: Fix an assertion failure in `_warnings.warn()` in case of a
bad`__name__` global. Patch by Oren Milman.
- bpo-31506: Improved the error message logic for `object.__new__` and`object.__init__` .
- bpo-31505: Fix an assertion failure in `json` , in case`_json.make_encoder()` received a bad`encoder()` argument. Patch by
Oren Milman.
- bpo-31492: Fix assertion failures in case of failing to import from a module with a bad `__name__` attribute, and in case of failing to access
an attribute of such a module. Patch by Oren Milman.
- bpo-31478: Fix an assertion failure in `_random.Random.seed()` in case
the argument has a bad`__abs__()` method. Patch by Oren Milman.
- bpo-31336: Speed up class creation by 10-20% by reducing the overhead in the necessary special method lookups. Patch by Stefan Behnel.
- bpo-31415: Add `-X importtime` option to show how long each import
takes. It can be used to optimize applicationâs startup time.  Support the`PYTHONPROFILEIMPORTTIME` as an equivalent way to enable this.
- bpo-31410: Optimized calling wrapper and classmethod descriptors.
- bpo-31353: **PEP 553** - Add a new built-in called`breakpoint()` which
calls`sys.breakpointhook()` .  By default this imports`pdb` and calls`pdb.set_trace()` , but users may override`sys.breakpointhook()` to
call whatever debugger they want.  The original value of the hook is saved
in`sys.__breakpointhook__` .
- bpo-17852: Maintain a list of open buffered files, flush them before exiting the interpreter. Based on a patch from Armin Rigo.
- bpo-31315: Fix an assertion failure in imp.create_dynamic(), when spec.name is not a string. Patch by Oren Milman.
- bpo-31311: Fix a crash in the `__setstate__()` method of`ctypes._CData` , in case of a bad`__dict__` . Patch by Oren Milman.
- bpo-31293: Fix crashes in true division and multiplication of a timedelta object by a float with a bad as_integer_ratio() method. Patch by Oren Milman.
- bpo-31285: Fix an assertion failure in `warnings.warn_explicit` , when
the return value of the received loaderâs`get_source()` has a bad`splitlines()` method. Patch by Oren Milman.
- bpo-30406: Make `async` and`await` proper keywords, as specified in**PEP 492** .

### LibraryÂ¶

- bpo-30058: Fixed buffer overflow in select.kqueue.control().
- bpo-31672: `idpattern` in`string.Template` matched some non-ASCII
characters. Now it uses`-i` regular expression local flag to avoid
non-ASCII characters.
- bpo-31701: On Windows, faulthandler.enable() now ignores MSC and COM exceptions.
- bpo-31728: Prevent crashes in `_elementtree` due to unsafe cleanup of`Element.text` and`Element.tail` . Patch by Oren Milman.
- bpo-31671: Now `re.compile()` converts passed RegexFlag to normal int
object before compiling. bm_regex_compile benchmark shows 14% performance
improvements.
- bpo-30397: The types of compiled regular objects and match objects are now exposed as `re.Pattern` and`re.Match` .  This adds information in
pydoc output for the`re` module.
- bpo-31675: Fixed memory leaks in Tkinterâs methods splitlist() and split() when pass a string larger than 2 GiB.
- bpo-31673: Fixed typo in the name of Tkinterâs method adderrorinfo().
- bpo-31648: Improvements to path predicates in ElementTree: Allow whitespace around predicate parts, i.e. â[a = âtextâ]â instead of requiring the less readable â[a=âtextâ]â. Add support for text comparison of the current node, like â[.=âtextâ]â. Patch by Stefan Behnel.
- bpo-30806: Fix the string representation of a netrc object.
- bpo-31638: Add optional argument `compressed` to`zipapp.create_archive` , and add option`--compress` to the command
line interface of`zipapp` .
- bpo-25351: Avoid venv activate failures with undefined variables
- bpo-20519: Avoid ctypes use (if possible) and improve import time for uuid.
- bpo-28293: The regular expression cache is no longer completely dumped when it is full.
- bpo-31596: Added pthread_getcpuclockid() to the time module
- bpo-27494: Make 2to3 accept a trailing comma in generator expressions. For example, `set(x for x in [],)` is now allowed.
- bpo-30347: Stop crashes when concurrently iterate over itertools.groupby() iterators.
- bpo-30346: An iterator produced by itertools.groupby() iterator now becomes exhausted after advancing the groupby iterator.
- bpo-31556: Cancel asyncio.wait_for future faster if timeout <= 0
- bpo-31540: Allow passing a context object in `concurrent.futures.ProcessPoolExecutor` constructor. Also, free
job resources in`concurrent.futures.ProcessPoolExecutor` earlier
to improve memory usage when a worker waits for new jobs.
- bpo-31516: `threading.current_thread()` should not return a dummy thread
at shutdown.
- bpo-31525: In the sqlite module, require the sqlite3_prepare_v2 API. Thus, the sqlite module now requires sqlite version at least 3.3.9.
- bpo-26510: argparse subparsers are now required by default. This matches behaviour in Python 2. For optional subparsers, use the new parameter `add_subparsers(required=False)` . Patch by Anthony Sottile. (As of
3.7.0rc1, the default was changed to not required as had been the case
since Python 3.3.)
- bpo-27541: Reprs of subclasses of some collection and iterator classes ( `bytearray` ,`array.array` ,`collections.deque` ,`collections.defaultdict` ,`itertools.count` ,`itertools.repeat` )
now contain actual type name instead of hardcoded names of the base class.
- bpo-31351: python -m ensurepip now exits with non-zero exit code if pip bootstrapping has failed.
- bpo-31389: `pdb.set_trace()` now takes an optional keyword-only argument`header` . If given, this is printed to the console just before debugging
begins.

### DocumentationÂ¶

### BuildÂ¶

- bpo-31696: Improve compiler version information in `sys.version` when Python is built with Clang.
- bpo-31625: Stop using ranlib on static libraries. Instead, we assume ar supports the âsâ flag.
- bpo-31624: Remove support for BSD/OS.
- bpo-22140: Prevent double substitution of prefix in python-config.sh.
- bpo-31569: Correct PCBuild/ case to PCbuild/ in build scripts and documentation.
- bpo-31536: Avoid wholesale rebuild after `make regen-all` if nothing
changed.

### IDLEÂ¶

- bpo-31460: Simplify the API of IDLEâs Module Browser. Passing a widget instead of an flist with a root widget opens the option of creating a browser frame that is only part of a window. Passing a full file name instead of pieces assumed to come from a .py file opens the possibility of browsing python files that do not end in .py.
- bpo-31649: IDLE - Make _htest, _utest parameters keyword only.
- bpo-31559: Remove test order dependence in idle_test.test_browser.
- bpo-31459: Rename IDLEâs module browser from Class Browser to Module Browser. The original module-level class and method browser became a module browser, with the addition of module-level functions, years ago. Nested classes and functions were added yesterday. For back-compatibility, the virtual event <<open-class-browser>>, which appears on the Keys tab of the Settings dialog, is not changed. Patch by Cheryl Sabella.
- bpo-31500: Default fonts now are scaled on HiDPI displays.
- bpo-1612262: IDLE module browser now shows nested classes and functions. Original patches for code and tests by Guilherme Polo and Cheryl Sabella, respectively.

### C APIÂ¶

- bpo-28280: Make `PyMapping_Keys()` ,`PyMapping_Values()` and`PyMapping_Items()` always return a`list` (rather than a`list` or
a`tuple` ). Patch by Oren Milman.
- bpo-31532: Fix memory corruption due to allocator mix in getpath.c between Py_GetPath() and Py_SetPath()
- bpo-25658: Implement **PEP 539** for Thread Specific Storage (TSS) API: it
is a new Thread Local Storage (TLS) API to CPython which would supersede
use of the existing TLS API within the CPython interpreter, while
deprecating the existing API. PEP written by Erik M. Bray, patch by
Masayuki Yamamoto.

## Python 3.7.0 alpha 1Â¶

*Release date: 2017-09-19*

### SecurityÂ¶

- bpo-29781: SSLObject.version() now correctly returns None when handshake over BIO has not been performed yet.
- bpo-29505: Add fuzz tests for float(str), int(str), unicode(str); for oss-fuzz.
- bpo-30947: Upgrade libexpat embedded copy from version 2.2.1 to 2.2.3 to get security fixes.
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

- bpo-31490: Fix an assertion failure in `ctypes` class definition, in
case the class has an attribute whose name is specified in`_anonymous_` but not in`_fields_` . Patch by Oren Milman.
- bpo-31471: Fix an assertion failure in `subprocess.Popen()` on Windows,
in case the env argument has a bad`keys()` method. Patch by Oren
Milman.
- bpo-31418: Fix an assertion failure in `PyErr_WriteUnraisable()` in case
of an exception with a bad`__module__` attribute. Patch by Oren Milman.
- bpo-31416: Fix assertion failures in case of a bad warnings.filters or warnings.defaultaction. Patch by Oren Milman.
- bpo-28411: Change direct usage of PyInterpreterState.modules to PyImport_GetModuleDict(). Also introduce more uniformity in other code that deals with sys.modules. This helps reduce complications when working on sys.modules.
- bpo-28411: Switch to the abstract API when dealing with `PyInterpreterState.modules` . This allows later support for all dict
subclasses and other Mapping implementations.  Also add a`PyImport_GetModule()` function to reduce a bunch of duplicated code.
- bpo-31411: Raise a TypeError instead of SystemError in case warnings.onceregistry is not a dictionary. Patch by Oren Milman.
- bpo-31344: For finer control of tracing behaviour when testing the interpreter, two new frame attributes have been added to control the emission of particular trace events: `f_trace_lines` (`True` by
default) to turn off per-line trace events; and`f_trace_opcodes` (`False` by default) to turn on per-opcode trace events.
- bpo-31373: Fix several possible instances of undefined behavior due to floating-point demotions.
- bpo-30465: Location information ( `lineno` and`col_offset` ) in
f-strings is now (mostly) correct.  This fixes tools like flake8 from
showing warnings on the wrong line (typically the first line of the file).
- bpo-30860: Consolidate CPythonâs global runtime state under a single struct. This improves discoverability of the runtime state.
- bpo-31347: Fix possible undefined behavior in _PyObject_FastCall_Prepend.
- bpo-31343: Include sys/sysmacros.h for major(), minor(), and makedev(). GNU C library plans to remove the functions from sys/types.h.
- bpo-31291: Fix an assertion failure in `zipimport.zipimporter.get_data` on Windows, when the return value of`pathname.replace('/','\\')` isnât
a string. Patch by Oren Milman.
- bpo-31271: Fix an assertion failure in the `write()` method of`io.TextIOWrapper` , when the encoder doesnât return a bytes object.
Patch by Oren Milman.
- bpo-31243: Fix a crash in some methods of `io.TextIOWrapper` , when the
decoderâs state is invalid. Patch by Oren Milman.
- bpo-30721: `print` now shows correct usage hint for using Python 2
redirection syntax.  Patch by Sanyam Khurana.
- bpo-31070: Fix a race condition in importlib _get_module_lock().
- bpo-30747: Add a non-dummy implementation of _Py_atomic_store and _Py_atomic_load on MSVC.
- bpo-31095: Fix potential crash during GC caused by `tp_dealloc` which
doesnât call`PyObject_GC_UnTrack()` .
- bpo-31071: Avoid masking original TypeError in call with * unpacking when other arguments are passed.
- bpo-30978: str.format_map() now passes key lookup exceptions through. Previously any exception was replaced with a KeyError exception.
- bpo-30808: Use _Py_atomic API for concurrency-sensitive signal state.
- bpo-30876: Relative import from unloaded package now reimports the package instead of failing with SystemError. Relative import from non-package now fails with ImportError rather than SystemError.
- bpo-30703: Improve signal delivery. Avoid using Py_AddPendingCall from signal handler, to avoid calling signal-unsafe functions. The tests Iâm adding here fail without the rest of the patch, on Linux and OS X. This means our signal delivery logic had defects (some signals could be lost).
- bpo-30765: Avoid blocking in pthread_mutex_lock() when PyThread_acquire_lock() is asked not to block.
- bpo-31161: Make sure the âMissing parenthesesâ syntax error message is only applied to SyntaxError, not to subclasses. Patch by Martijn Pieters.
- bpo-30814: Fixed a race condition when import a submodule from a package.
- bpo-30736: The internal unicodedata database has been upgraded to Unicode 10.0.
- bpo-30604: Move co_extra_freefuncs from per-thread to per-interpreter to avoid crashes.
- bpo-30597: `print` now shows expected input in custom error message when
used as a Python 2 statement. Patch by Sanyam Khurana.
- bpo-30682: Removed a too-strict assertion that failed for certain f-strings, such as eval(âfâ\nââ) and eval(âfâ\rââ).
- bpo-30501: The compiler now produces more optimal code for complex condition expressions in the âifâ, âwhileâ and âassertâ statement, the âifâ expression, and generator expressions and comprehensions.
- bpo-28180: Implement **PEP 538** (legacy C locale coercion). This means
that when a suitable coercion target locale is available, both the core
interpreter and locale-aware C extensions will assume the use of UTF-8 as
the default text encoding, rather than ASCII.
- bpo-30486: Allows setting cell values for __closure__. Patch by Lisa Roach.
- bpo-30537: itertools.islice now accepts integer-like objects (having an __index__ method) as start, stop, and slice arguments
- bpo-25324: Tokens needed for parsing in Python moved to C. `COMMENT` ,`NL` and`ENCODING` . This way the tokens and tok_names in the token
module donât get changed when you import the tokenize module.
- bpo-29104: Fixed parsing backslashes in f-strings.
- bpo-27945: Fixed various segfaults with dict when input collections are mutated during searching, inserting or comparing. Based on patches by Duane Griffin and Tim Mitchell.
- bpo-25794: Fixed type.__setattr__() and type.__delattr__() for non-interned attribute names. Based on patch by Eryk Sun.
- bpo-30039: If a KeyboardInterrupt happens when the interpreter is in the middle of resuming a chain of nested âyield fromâ or âawaitâ calls, itâs now correctly delivered to the innermost frame.
- bpo-28974: `object.__format__(x, '')` is now equivalent to`str(x)` rather than`format(str(self), '')` .
- bpo-30024: Circular imports involving absolute imports with binding a submodule to a name are now supported.
- bpo-12414: sys.getsizeof() on a code object now returns the sizes which includes the code struct and sizes of objects which it references. Patch by Donghee Na.
- bpo-29839: len() now raises ValueError rather than OverflowError if __len__() returned a large negative integer.
- bpo-11913: README.rst is now included in the list of distutils standard READMEs and therefore included in source distributions.
- bpo-29914: Fixed default implementations of __reduce__ and __reduce_ex__(). object.__reduce__() no longer takes arguments, object.__reduce_ex__() now requires one argument.
- bpo-29949: Fix memory usage regression of set and frozenset object.
- bpo-29935: Fixed error messages in the index() method of tuple, list and deque when pass indices of wrong type.
- bpo-29816: Shift operation now has less opportunity to raise OverflowError. ValueError always is raised rather than OverflowError for negative counts. Shifting zero with non-negative count always returns zero.
- bpo-24821: Fixed the slowing down to 25 times in the searching of some unlucky Unicode characters.
- bpo-29102: Add a unique ID to PyInterpreterState. This makes it easier to identify each subinterpreter.
- bpo-29894: The deprecation warning is emitted if __complex__ returns an instance of a strict subclass of complex. In a future versions of Python this can be an error.
- bpo-29859: Show correct error messages when any of the pthread_* calls in thread_pthread.h fails.
- bpo-29849: Fix a memory leak when an ImportError is raised during from import.
- bpo-28856: Fix an oversight that %b format for bytes should support objects follow the buffer protocol.
- bpo-29723: The `sys.path[0]` initialization change for bpo-29139 caused
a regression by revealing an inconsistency in how sys.path is initialized
when executing`__main__` from a zipfile, directory, or other import
location. The interpreter now consistently avoids ever adding the import
locationâs parent directory to`sys.path` , and ensures no other`sys.path` entries are inadvertently modified when inserting the import
location named on the command line.
- bpo-29568: Escaped percent â%%â in the format string for classic string formatting no longer allows any characters between two percents.
- bpo-29714: Fix a regression that bytes format may fail when containing zero bytes inside.
- bpo-29695: bool(), float(), list() and tuple() no longer take keyword arguments. The first argument of int() can now be passes only as positional argument.
- bpo-28893: Set correct __cause__ for errors about invalid awaitables returned from __aiter__ and __anext__.
- bpo-28876: `bool(range)` works even if`len(range)` raises`OverflowError` .
- bpo-29683: Fixes to memory allocation in _PyCode_SetExtra. Patch by Brian Coleman.
- bpo-29684: Fix minor regression of PyEval_CallObjectWithKeywords. It should raise TypeError when kwargs is not a dict. But it might cause segv when args=NULL and kwargs is not a dict.
- bpo-28598: Support __rmod__ for subclasses of str being called before str.__mod__. Patch by Martijn Pieters.
- bpo-29607: Fix stack_effect computation for CALL_FUNCTION_EX. Patch by Matthieu Dartiailh.
- bpo-29602: Fix incorrect handling of signed zeros in complex constructor for complex subclasses and for inputs having a __complex__ method. Patch by Serhiy Storchaka.
- bpo-29347: Fixed possibly dereferencing undefined pointers when creating weakref objects.
- bpo-29463: Add `docstring` field to Module, ClassDef, FunctionDef, and
AsyncFunctionDef ast nodes.  docstring is not first stmt in their body
anymore.  It affects`co_firstlineno` and`co_lnotab` of code object
for module and class. (Reverted in bpo-32911.)
- bpo-29438: Fixed use-after-free problem in key sharing dict.
- bpo-29546: Set the âpathâ and ânameâ attribute on ImportError for ```
from
... import ...
```
.
- bpo-29546: Improve from-import error message with location
- bpo-29478: If max_line_length=None is specified while using the Compat32 policy, it is no longer ignored. Patch by Mircea Cosbuc.
- bpo-29319: Prevent RunMainFromImporter overwriting sys.path[0].
- bpo-29337: Fixed possible BytesWarning when compare the code objects. Warnings could be emitted at compile time.
- bpo-29327: Fixed a crash when pass the iterable keyword argument to sorted().
- bpo-29034: Fix memory leak and use-after-free in os module (path_converter).
- bpo-29159: Fix regression in bytes(x) when x.__index__() raises Exception.
- bpo-29049: Call _PyObject_GC_TRACK() lazily when calling Python function. Calling function is up to 5% faster.
- bpo-28927: bytes.fromhex() and bytearray.fromhex() now ignore all ASCII whitespace, not only spaces. Patch by Robert Xiao.
- bpo-28932: Do not include <sys/random.h> if it does not exist.
- bpo-25677: Correct the positioning of the syntax error caret for indented blocks. Based on patch by Michael Layzell.
- bpo-29000: Fixed bytes formatting of octals with zero padding in alternate form.
- bpo-18896: Python function can now have more than 255 parameters. collections.namedtuple() now supports tuples with more than 255 elements.
- bpo-28596: The preferred encoding is UTF-8 on Android. Patch written by Chi Hsuan Yen.
- bpo-22257: Clean up interpreter startup (see **PEP 432** ).
- bpo-26919: On Android, operating system data is now always encoded/decoded to/from UTF-8, instead of the locale encoding to avoid inconsistencies with os.fsencode() and os.fsdecode() which are already using UTF-8.
- bpo-28991: functools.lru_cache() was susceptible to an obscure reentrancy bug triggerable by a monkey-patched len() function.
- bpo-28147: Fix a memory leak in split-table dictionaries: setattr() must not convert combined table into split table. Patch written by INADA Naoki.
- bpo-28739: f-string expressions are no longer accepted as docstrings and by ast.literal_eval() even if they do not include expressions.
- bpo-28512: Fixed setting the offset attribute of SyntaxError by PyErr_SyntaxLocationEx() and PyErr_SyntaxLocationObject().
- bpo-28918: Fix the cross compilation of xxlimited when Python has been built with Py_DEBUG defined.
- bpo-23722: Rather than silently producing a class that doesnât support zero-argument `super()` in methods, failing to pass the new`__classcell__` namespace entry up to`type.__new__` now results in a`DeprecationWarning` and a class that supports zero-argument`super()` .
- bpo-28797: Modifying the class __dict__ inside the __set_name__ method of a descriptor that is used inside that class no longer prevents calling the __set_name__ method of other descriptors.
- bpo-28799: Remove the `PyEval_GetCallStats()` function and deprecate the
untested and undocumented`sys.callstats()` function. Remove the`CALL_PROFILE` special build: use the`sys.setprofile()` function,`cProfile` or`profile` to profile function calls.
- bpo-12844: More than 255 arguments can now be passed to a function.
- bpo-28782: Fix a bug in the implementation `yield from` when checking if
the next instruction is YIELD_FROM. Regression introduced by WORDCODE
(bpo-26647).
- bpo-28774: Fix error position of the unicode error in ASCII and Latin1 encoders when a string returned by the error handler contains multiple non-encodable characters (non-ASCII for the ASCII codec, characters out of the U+0000-U+00FF range for Latin1).
- bpo-28731: Optimize _PyDict_NewPresized() to create correct size dict. Improve speed of dict literal with constant keys up to 30%.
- bpo-28532: Show sys.version when -V option is supplied twice.
- bpo-27100: The with-statement now checks for __enter__ before it checks for __exit__. This gives less confusing error messages when both methods are missing. Patch by Jonathan Ellington.
- bpo-28746: Fix the set_inheritable() file descriptor method on platforms that do not have the ioctl FIOCLEX and FIONCLEX commands.
- bpo-26920: Fix not getting the localeâs charset upon initializing the interpreter, on platforms that do not have langinfo.
- bpo-28648: Fixed crash in Py_DecodeLocale() in debug build on Mac OS X when decode astral characters. Patch by Xiang Zhang.
- bpo-28665: Improve speed of the STORE_DEREF opcode by 40%.
- bpo-19398: Extra slash no longer added to sys.path components in case of empty compile-time PYTHONPATH components.
- bpo-28621: Sped up converting int to float by reusing faster bits counting implementation. Patch by Adrian Wielgosik.
- bpo-28580: Optimize iterating split table values. Patch by Xiang Zhang.
- bpo-28583: PyDict_SetDefault didnât combine split table when needed. Patch by Xiang Zhang.
- bpo-28128: Deprecation warning for invalid str and byte escape sequences now prints better information about where the error occurs. Patch by Serhiy Storchaka and Eric Smith.
- bpo-28509: dict.update() no longer allocate unnecessary large memory.
- bpo-28426: Fixed potential crash in PyUnicode_AsDecodedObject() in debug build.
- bpo-28517: Fixed of-by-one error in the peephole optimizer that caused keeping unreachable code.
- bpo-28214: Improved exception reporting for problematic __set_name__ attributes.
- bpo-23782: Fixed possible memory leak in _PyTraceback_Add() and exception loss in PyTraceBack_Here().
- bpo-28183: Optimize and cleanup dict iteration.
- bpo-26081: Added C implementation of asyncio.Future. Original patch by Yury Selivanov.
- bpo-28379: Added sanity checks and tests for PyUnicode_CopyCharacters(). Patch by Xiang Zhang.
- bpo-28376: The type of long range iterator is now registered as Iterator. Patch by Oren Milman.
- bpo-28376: Creating instances of range_iterator by calling range_iterator type now is disallowed. Calling iter() on range instance is the only way. Patch by Oren Milman.
- bpo-26906: Resolving special methods of uninitialized type now causes implicit initialization of the type instead of a fail.
- bpo-18287: PyType_Ready() now checks that tp_name is not NULL. Original patch by Niklas Koep.
- bpo-24098: Fixed possible crash when AST is changed in process of compiling it.
- bpo-28201: Dict reduces possibility of 2nd conflict in hash table when hashes have same lower bits.
- bpo-28350: String constants with null character no longer interned.
- bpo-26617: Fix crash when GC runs during weakref callbacks.
- bpo-27942: String constants now interned recursively in tuples and frozensets.
- bpo-28289: ImportError.__init__ now resets not specified attributes.
- bpo-21578: Fixed misleading error message when ImportError called with invalid keyword args.
- bpo-28203: Fix incorrect type in complex(1.0, {2:3}) error message. Patch by Soumya Sharma.
- bpo-28086: Single var-positional argument of tuple subtype was passed unscathed to the C-defined function. Now it is converted to exact tuple.
- bpo-28214: Now __set_name__ is looked up on the class instead of the instance.
- bpo-27955: Fallback on reading /dev/urandom device when the getrandom() syscall fails with EPERM, for example when blocked by SECCOMP.
- bpo-28192: Donât import readline in isolated mode.
- bpo-27441: Remove some redundant assignments to ob_size in longobject.c. Thanks Oren Milman.
- bpo-27222: Clean up redundant code in long_rshift function. Thanks Oren Milman.
- Upgrade internal unicode databases to Unicode version 9.0.0.
- bpo-28131: Fix a regression in zipimportâs compile_source(). zipimport should use the same optimization level as the interpreter.
- bpo-28126: Replace Py_MEMCPY with memcpy(). Visual Studio can properly optimize memcpy().
- bpo-28120: Fix dict.pop() for split dictionary when trying to remove a âpending keyâ (Not yet inserted in split-table). Patch by Xiang Zhang.
- bpo-26182: Raise DeprecationWarning when async and await keywords are used as variable/attribute/class/function name.
- bpo-26182: Fix a refleak in code that raises DeprecationWarning.
- bpo-28721: Fix asynchronous generators aclose() and athrow() to handle StopAsyncIteration propagation properly.
- bpo-26110: Speed-up method calls: add LOAD_METHOD and CALL_METHOD opcodes.

### LibraryÂ¶

- bpo-31499: xml.etree: Fix a crash when a parser is part of a reference cycle.
- bpo-31482: `random.seed()` now works with bytes in version=1
- bpo-28556: typing.get_type_hints now finds the right globalns for classes and modules by default (when no `globalns` was specified by the caller).
- bpo-28556: Speed improvements to the `typing` module.  Original PRs by
Ivan Levkivskyi and Mitar.
- bpo-31544: The C accelerator module of ElementTree ignored exceptions raised when looking up TreeBuilder target methods in XMLParser().
- bpo-31234: socket.create_connection() now fixes manually a reference cycle: clear the variable storing the last exception on success.
- bpo-31457: LoggerAdapter objects can now be nested.
- bpo-31431: SSLContext.check_hostname now automatically sets SSLContext.verify_mode to ssl.CERT_REQUIRED instead of failing with a ValueError.
- bpo-31233: socketserver.ThreadingMixIn now keeps a list of non-daemonic threads to wait until all these threads complete in server_close().
- bpo-28638: Changed the implementation strategy for collections.namedtuple() to substantially reduce the use of exec() in favor of precomputed methods. As a result, the *verbose* parameter and*_source* attribute are no longer supported.  The benefits include 1)
having a smaller memory footprint for applications using multiple named
tuples, 2) faster creation of the named tuple class (approx 4x to 6x
depending on how it is measured), and 3) minor speed-ups for instance
creation using __new__, _make, and _replace.  (The primary patch
contributor is Jelle Zijlstra with further improvements by INADA Naoki,
Serhiy Storchaka, and Raymond Hettinger.)
- bpo-31400: Improves SSL error handling to avoid losing error numbers.
- bpo-27629: Make return types of SSLContext.wrap_bio() and SSLContext.wrap_socket() customizable.
- bpo-28958: ssl.SSLContext() now uses OpenSSL error information when a context cannot be instantiated.
- bpo-28182: The SSL module now raises SSLCertVerificationError when OpenSSL fails to verify the peerâs certificate. The exception contains more information about the error.
- bpo-27340: SSLSocket.sendall() now uses memoryview to create slices of data. This fixes support for all bytes-like object. It is also more efficient and avoids costly copies.
- bpo-14191: A new function `argparse.ArgumentParser.parse_intermixed_args` provides the ability to
parse command lines where there user intermixes options and positional
arguments.
- bpo-31178: Fix string concatenation bug in rare error path in the subprocess module
- bpo-31350: Micro-optimize `asyncio._get_running_loop()` to become up
to 10% faster.
- bpo-31170: expat: Update libexpat from 2.2.3 to 2.2.4. Fix copying of partial characters for UTF-8 input (libexpat bug 115): https://github.com/libexpat/libexpat/issues/115
- bpo-29136: Add TLS 1.3 cipher suites and OP_NO_TLSv1_3.
- bpo-1198569: `string.Template` subclasses can optionally define`braceidpattern` if they want to specify different placeholder patterns
inside and outside the braces.  If None (the default) it falls back to`idpattern` .
- bpo-31326: concurrent.futures.ProcessPoolExecutor.shutdown() now explicitly closes the call queue. Moreover, shutdown(wait=True) now also join the call queue thread, to prevent leaking a dangling thread.
- bpo-27144: The `map()` and`as_completed()` iterators in`concurrent.futures` now avoid keeping a reference to yielded objects.
- bpo-31281: Fix `fileinput.FileInput(files, inplace=True)` when`files` contain`pathlib.Path` objects.
- bpo-10746: Fix ctypes producing wrong **PEP 3118** type codes for integer
types.
- bpo-27584: `AF_VSOCK` has been added to the socket interface which
allows communication between virtual machines and their host.
- bpo-22536: The subprocess module now sets the filename when FileNotFoundError is raised on POSIX systems due to the executable or cwd not being found.
- bpo-29741: Update some methods in the _pyio module to also accept integer types. Patch by Oren Milman.
- bpo-31249: concurrent.futures: WorkItem.run() used by ThreadPoolExecutor now breaks a reference cycle between an exception object and the WorkItem object.
- bpo-31247: xmlrpc.server now explicitly breaks reference cycles when using sys.exc_info() in code handling exceptions.
- bpo-23835: configparser: reading defaults in the `ConfigParser()` constructor is now using`read_dict()` , making its behavior consistent
with the rest of the parser.  Non-string keys and values in the defaults
dictionary are now being implicitly converted to strings.  Patch by James
Tocknell.
- bpo-31238: pydoc: the stop() method of the private ServerThread class now waits until DocServer.serve_until_quit() completes and then explicitly sets its docserver attribute to None to break a reference cycle.
- bpo-5001: Many asserts in `multiprocessing` are now more informative,
and some error types have been changed to more specific ones.
- bpo-31109: Convert zipimport to use Argument Clinic.
- bpo-30102: The ssl and hashlib modules now call OPENSSL_add_all_algorithms_noconf() on OpenSSL < 1.1.0. The function detects CPU features and enables optimizations on some CPU architectures such as POWER8. Patch is based on research from Gustavo Serra Scalet.
- bpo-18966: Non-daemonic threads created by a multiprocessing.Process are now joined on child exit.
- bpo-31183: `dis` now works with asynchronous generator and coroutine
objects. Patch by George Collins based on diagnosis by Luciano Ramalho.
- bpo-5001: There are a number of uninformative asserts in the `multiprocessing` module, as noted in issue 5001. This change fixes two
of the most potentially problematic ones, since they are in
error-reporting code, in the`multiprocessing.managers.convert_to_error` function. (It also makes more informative a ValueError message.) The only
potentially problematic change is that the AssertionError is now a
TypeError; however, this should also help distinguish it from an
AssertionError being*reported* by the function/its caller (such as in
issue 31169). - Patch by Allen W. Smith (drallensmith on github).
- bpo-31185: Fixed miscellaneous errors in asyncio speedup module.
- bpo-31151: socketserver.ForkingMixIn.server_close() now waits until all child processes completed to prevent leaking zombie processes.
- bpo-31072: Add an `include_file` parameter to`zipapp.create_archive()`
- bpo-24700: Optimize array.array comparison. It is now from 10x up to 70x faster when comparing arrays holding values of the same integer type.
- bpo-31135: ttk: fix the destroy() method of LabeledScale and OptionMenu classes. Call the parent destroy() method even if the used attribute doesnât exist. The LabeledScale.destroy() method now also explicitly clears label and scale attributes to help the garbage collector to destroy all widgets.
- bpo-31107: Fix `copyreg._slotnames()` mangled attribute calculation for
classes whose name begins with an underscore. Patch by Shane Harvey.
- bpo-31080: Allow `logging.config.fileConfig` to accept kwargs and/or
args.
- bpo-30897: `pathlib.Path` objects now include an`is_mount()` method
(only implemented on POSIX).  This is similar to`os.path.ismount(p)` .
Patch by Cooper Ry Lees.
- bpo-31061: Fixed a crash when using asyncio and threads.
- bpo-30987: Added support for CAN ISO-TP protocol in the socket module.
- bpo-30522: Added a `setStream` method to`logging.StreamHandler` to
allow the stream to be set after creation.
- bpo-30502: Fix handling of long oids in ssl. Based on patch by Christian Heimes.
- bpo-5288: Support tzinfo objects with sub-minute offsets.
- bpo-30919: Fix shared memory performance regression in multiprocessing in 3.x. Shared memory used anonymous memory mappings in 2.x, while 3.x mmaps actual files. Try to be careful to do as little disk I/O as possible.
- bpo-26732: Fix too many fds in processes started with the âforkserverâ method. A child process would inherit as many fds as the number of still-running children.
- bpo-29403: Fix `unittest.mock` âs autospec to not fail on method-bound
builtin functions.  Patch by Aaron Gallagher.
- bpo-30961: Fix decrementing a borrowed reference in tracemalloc.
- bpo-19896: Fix multiprocessing.sharedctypes to recognize typecodes `'q'` and`'Q'` .
- bpo-30946: Remove obsolete code in readline module for platforms where GNU readline is older than 2.1 or where select() is not available.
- bpo-25684: Change `ttk.OptionMenu` radiobuttons to be unique across
instances of`OptionMenu` .
- bpo-30886: Fix multiprocessing.Queue.join_thread(): it now waits until the thread completes, even if the thread was started by the same process which created the queue.
- bpo-29854: Fix segfault in readline when using readlineâs history-size option. Patch by Nir Soffer.
- bpo-30794: Added multiprocessing.Process.kill method to terminate using the SIGKILL signal on Unix.
- bpo-30319: socket.close() now ignores ECONNRESET error.
- bpo-30828: Fix out of bounds write in `asyncio.CFuture.remove_done_callback()` .
- bpo-30302: Use keywords in the `repr` of`datetime.timedelta` .
- bpo-30807: signal.setitimer() may disable the timer when passed a tiny value. Tiny values (such as 1e-6) are valid non-zero values for setitimer(), which is specified as taking microsecond-resolution intervals. However, on some platform, our conversion routine could convert 1e-6 into a zero interval, therefore disabling the timer instead of (re-)scheduling it.
- bpo-30441: Fix bug when modifying os.environ while iterating over it
- bpo-29585: Avoid importing `sysconfig` from`site` to improve startup
speed. Python startup is about 5% faster on Linux and 30% faster on macOS.
- bpo-29293: Add missing parameter ânâ on multiprocessing.Condition.notify(). The doc claims multiprocessing.Condition behaves like threading.Condition, but its notify() method lacked the optional ânâ argument (to specify the number of sleepers to wake up) that threading.Condition.notify() accepts.
- bpo-30532: Fix email header value parser dropping folding white space in certain cases.
- bpo-30596: Add a `close()` method to`multiprocessing.Process` .
- bpo-9146: Fix a segmentation fault in _hashopenssl when standard hash functions such as md5 are not available in the linked OpenSSL library. As in some special FIPS-140 build environments.
- bpo-29169: Update zlib to 1.2.11.
- bpo-30119: ftplib.FTP.putline() now throws ValueError on commands that contains CR or LF. Patch by Donghee Na.
- bpo-30879: os.listdir() and os.scandir() now emit bytes names when called with bytes-like argument.
- bpo-30746: Prohibited the â=â character in environment variable names in `os.putenv()` and`os.spawn*()` .
- bpo-30664: The description of a unittest subtest now preserves the order of keyword arguments of TestCase.subTest().
- bpo-21071: struct.Struct.format type is now `str` instead of`bytes` .
- bpo-29212: Fix concurrent.futures.thread.ThreadPoolExecutor threads to have a non repr() based thread name by default when no thread_name_prefix is supplied. They will now identify themselves as âThreadPoolExecutor-y_nâ.
- bpo-29755: Fixed the lgettext() family of functions in the gettext module. They now always return bytes.
- bpo-30616: Functional API of enum allows to create empty enums. Patched by Donghee Na
- bpo-30038: Fix race condition between signal delivery and wakeup file descriptor. Patch by Nathaniel Smith.
- bpo-23894: lib2to3 now recognizes `rb'...'` and`f'...'` strings.
- bpo-24744: pkgutil.walk_packages function now raises ValueError if *path* is a string. Patch by Sanyam Khurana.
- bpo-24484: Avoid race condition in multiprocessing cleanup.
- bpo-30589: Fix multiprocessing.Process.exitcode to return the opposite of the signal number when the process is killed by a signal (instead of 255) when using the âforkserverâ method.
- bpo-28994: The traceback no longer displayed for SystemExit raised in a callback registered by atexit.
- bpo-30508: Donât log exceptions if Task/Future âcancel()â method was called.
- bpo-30645: Fix path calculation in `imp.load_package()` , fixing it for
cases when a package is only shipped with bytecodes. Patch by Alexandru
Ardelean.
- bpo-11822: The dis.dis() function now is able to disassemble nested code objects.
- bpo-30624: selectors does not take KeyboardInterrupt and SystemExit into account, leaving a fd in a bad state in case of error. Patch by Giampaolo Rodolaâ.
- bpo-30595: multiprocessing.Queue.get() with a timeout now polls its reader in non-blocking mode if it succeeded to acquire the lock but the acquire took longer than the timeout.
- bpo-28556: Updates to typing module: Add generic AsyncContextManager, add support for ContextManager on all versions. Original PRs by Jelle Zijlstra and Ivan Levkivskyi
- bpo-30605: re.compile() no longer raises a BytesWarning when compiling a bytes instance with misplaced inline modifier. Patch by Roy Williams.
- bpo-29870: Fix ssl sockets leaks when connection is aborted in asyncio/ssl implementation. Patch by MichaÃ«l SghaÃ¯er.
- bpo-29743: Closing transport during handshake process leaks open socket. Patch by Nikolay Kim
- bpo-27585: Fix waiter cancellation in asyncio.Lock. Patch by Mathieu Sornay.
- bpo-30014: modify() method of poll(), epoll() and devpoll() based classes of selectors module is around 10% faster. Patch by Giampaolo Rodolaâ.
- bpo-30418: On Windows, subprocess.Popen.communicate() now also ignore EINVAL on stdin.write() if the child process is still running but closed the pipe.
- bpo-30463: Added empty __slots__ to abc.ABC. This allows subclassers to deny __dict__ and __weakref__ creation. Patch by Aaron Hall.
- bpo-30520: Loggers are now pickleable.
- bpo-30557: faulthandler now correctly filters and displays exception codes on Windows
- bpo-30526: Add TextIOWrapper.reconfigure() and a TextIOWrapper.write_through attribute.
- bpo-30245: Fix possible overflow when organize struct.pack_into error message. Patch by Yuan Liu.
- bpo-30378: Fix the problem that logging.handlers.SysLogHandler cannot handle IPv6 addresses.
- bpo-16500: Allow registering at-fork handlers.
- bpo-30470: Deprecate invalid ctypes call protection on Windows. Patch by Mariatta Wijaya.
- bpo-30414: multiprocessing.Queue._feed background running thread do not break from main loop on exception.
- bpo-30003: Fix handling escape characters in HZ codec. Based on patch by Ma Lin.
- bpo-30149: inspect.signature() now supports callables with variable-argument parameters wrapped with partialmethod. Patch by Donghee Na.
- bpo-30436: importlib.find_spec() raises ModuleNotFoundError instead of AttributeError if the specified parent module is not a package (i.e. lacks a __path__ attribute).
- bpo-30301: Fix AttributeError when using SimpleQueue.empty() under *spawn* and*forkserver* start methods.
- bpo-30375: Warnings emitted when compile a regular expression now always point to the line in the user code. Previously they could point into inners of the re module if emitted from inside of groups or conditionals.
- bpo-30329: imaplib and poplib now catch the Windows socket WSAEINVAL error (code 10022) on shutdown(SHUT_RDWR): An invalid operation was attempted. This error occurs sometimes on SSL connections.
- bpo-29196: Removed previously deprecated in Python 2.4 classes Plist, Dict and _InternalDict in the plistlib module. Dict values in the result of functions readPlist() and readPlistFromBytes() are now normal dicts. You no longer can use attribute access to access items of these dictionaries.
- bpo-9850: The `macpath` is now deprecated and will be removed in
Python 3.8.
- bpo-30299: Compiling regular expression in debug mode on CPython now displays the compiled bytecode in human readable form.
- bpo-30048: Fixed `Task.cancel()` can be ignored when the task is running
coroutine and the coroutine returned without any more`await` .
- bpo-30266: contextlib.AbstractContextManager now supports anti-registration by setting __enter__ = None or __exit__ = None, following the pattern introduced in bpo-25958. Patch by Jelle Zijlstra.
- bpo-30340: Enhanced regular expressions optimization. This increased the performance of matching some patterns up to 25 times.
- bpo-30298: Weaken the condition of deprecation warnings for inline modifiers. Now allowed several subsequential inline modifiers at the start of the pattern (e.g. `'(?i)(?s)...'` ).  In verbose mode whitespaces and
comments now are allowed before and between inline modifiers (e.g.```
'(?x)
(?i) (?s)...'
```
).
- bpo-30285: Optimized case-insensitive matching and searching of regular expressions.
- bpo-29990: Fix range checking in GB18030 decoder. Original patch by Ma Lin.
- bpo-29979: rewrite cgi.parse_multipart, reusing the FieldStorage class and making its results consistent with those of FieldStorage for multipart/form-data requests. Patch by Pierre Quentel.
- bpo-30243: Removed the __init__ methods of _jsonâs scanner and encoder. Misusing them could cause memory leaks or crashes. Now scanner and encoder objects are completely initialized in the __new__ methods.
- bpo-30215: Compiled regular expression objects with the re.LOCALE flag no longer depend on the locale at compile time. Only the locale at matching time affects the result of matching.
- bpo-30185: Avoid KeyboardInterrupt tracebacks in forkserver helper process when Ctrl-C is received.
- bpo-30103: binascii.b2a_uu() and uu.encode() now support using ``'`'`` as
zero instead of space.
- bpo-28556: Various updates to typing module: add typing.NoReturn type, use WrapperDescriptorType, minor bug-fixes. Original PRs by Jim Fasarakis-Hilliard and Ivan Levkivskyi.
- bpo-30205: Fix getsockname() for unbound AF_UNIX sockets on Linux.
- bpo-30228: The seek() and tell() methods of io.FileIO now set the internal seekable attribute to avoid one syscall on open() (in buffered or text mode).
- bpo-30190: unittestâs assertAlmostEqual and assertNotAlmostEqual provide a better message in case of failure which includes the difference between left and right arguments. (patch by Giampaolo Rodolaâ)
- bpo-30101: Add support for curses.A_ITALIC.
- bpo-29822: inspect.isabstract() now works during __init_subclass__. Patch by Nate Soares.
- bpo-29960: Preserve generator state when _random.Random.setstate() raises an exception. Patch by Bryan Olson.
- bpo-30070: Fixed leaks and crashes in errors handling in the parser module.
- bpo-22352: Column widths in the output of dis.dis() are now adjusted for large line numbers and instruction offsets.
- bpo-30061: Fixed crashes in IOBase methods __next__() and readlines() when readline() or __next__() respectively return non-sizeable object. Fixed possible other errors caused by not checking results of PyObject_Size(), PySequence_Size(), or PyMapping_Size().
- bpo-30218: Fix PathLike support for shutil.unpack_archive. Patch by Jelle Zijlstra.
- bpo-10076: Compiled regular expression and match objects in the re module now support copy.copy() and copy.deepcopy() (they are considered atomic).
- bpo-30068: _io._IOBase.readlines will check if itâs closed first when hint is present.
- bpo-29694: Fixed race condition in pathlib mkdir with flags parents=True. Patch by Armin Rigo.
- bpo-29692: Fixed arbitrary unchaining of RuntimeError exceptions in contextlib.contextmanager. Patch by Siddharth Velankar.
- bpo-26187: Test that sqlite3 trace callback is not called multiple times when schema is changing. Indirectly fixed by switching to use sqlite3_prepare_v2() in bpo-9303. Patch by Aviv Palivoda.
- bpo-30017: Allowed calling the close() method of the zip entry writer object multiple times. Writing to a closed writer now always produces a ValueError.
- bpo-29998: Pickling and copying ImportError now preserves name and path attributes.
- bpo-29995: re.escape() now escapes only regex special characters.
- bpo-29962: Add math.remainder operation, implementing remainder as specified in IEEE 754.
- bpo-29649: Improve struct.pack_into() exception messages for problems with the buffer size and offset. Patch by Andrew Nester.
- bpo-29654: Support If-Modified-Since HTTP header (browser cache). Patch by Pierre Quentel.
- bpo-29931: Fixed comparison check for ipaddress.ip_interface objects. Patch by Sanjay Sundaresan.
- bpo-29953: Fixed memory leaks in the replace() method of datetime and time objects when pass out of bound fold argument.
- bpo-29942: Fix a crash in itertools.chain.from_iterable when encountering long runs of empty iterables.
- bpo-10030: Sped up reading encrypted ZIP files by 2 times.
- bpo-29204: Element.getiterator() and the html parameter of XMLParser() were deprecated only in the documentation (since Python 3.2 and 3.4 correspondingly). Now using them emits a deprecation warning.
- bpo-27863: Fixed multiple crashes in ElementTree caused by race conditions and wrong types.
- bpo-25996: Added support of file descriptors in os.scandir() on Unix. os.fwalk() is sped up by 2 times by using os.scandir().
- bpo-28699: Fixed a bug in pools in multiprocessing.pool that raising an exception at the very first of an iterable may swallow the exception or make the program hang. Patch by Davin Potts and Xiang Zhang.
- bpo-23890: unittest.TestCase.assertRaises() now manually breaks a reference cycle to not keep objects alive longer than expected.
- bpo-29901: The zipapp module now supports general path-like objects, not just pathlib.Path.
- bpo-25803: Avoid incorrect errors raised by Path.mkdir(exist_ok=True) when the OS gives priority to errors such as EACCES over EEXIST.
- bpo-29861: Release references to tasks, their arguments and their results as soon as they are finished in multiprocessing.Pool.
- bpo-19930: The mode argument of os.makedirs() no longer affects the file permission bits of newly created intermediate-level directories.
- bpo-29884: faulthandler: Restore the old sigaltstack during teardown. Patch by Christophe Zeitouny.
- bpo-25455: Fixed crashes in repr of recursive buffered file-like objects.
- bpo-29800: Fix crashes in partial.__repr__ if the keys of partial.keywords are not strings. Patch by Michael Seifert.
- bpo-8256: Fixed possible failing or crashing input() if attributes âencodingâ or âerrorsâ of sys.stdin or sys.stdout are not set or are not strings.
- bpo-28692: Using non-integer value for selecting a plural form in gettext is now deprecated.
- bpo-26121: Use C library implementation for math functions erf() and erfc().
- bpo-29619: os.stat() and os.DirEntry.inode() now convert inode (st_ino) using unsigned integers.
- bpo-28298: Fix a bug that prevented array âQâ, âLâ and âIâ from accepting big intables (objects that have __int__) as elements.
- bpo-29645: Speed up importing the webbrowser module. webbrowser.register() is now thread-safe.
- bpo-28231: The zipfile module now accepts path-like objects for external paths.
- bpo-26915: index() and count() methods of collections.abc.Sequence now check identity before checking equality when do comparisons.
- bpo-28682: Added support for bytes paths in os.fwalk().
- bpo-29728: Add new `socket.TCP_NOTSENT_LOWAT` (Linux 3.12)
constant. Patch by Nathaniel J. Smith.
- bpo-29623: Allow use of path-like object as a single argument in ConfigParser.read(). Patch by David Ellis.
- bpo-9303: Migrate sqlite3 module to _v2 API. Patch by Aviv Palivoda.
- bpo-28963: Fix out of bound iteration in asyncio.Future.remove_done_callback implemented in C.
- bpo-29704: asyncio.subprocess.SubprocessStreamProtocol no longer closes before all pipes are closed.
- bpo-29271: Fix Task.current_task and Task.all_tasks implemented in C to accept None argument as their pure Python implementation.
- bpo-29703: Fix asyncio to support instantiation of new event loops in child processes.
- bpo-29615: SimpleXMLRPCDispatcher no longer chains KeyError (or any other exception) to exception(s) raised in the dispatched methods. Patch by Petr Motejlek.
- bpo-7769: Method register_function() of xmlrpc.server.SimpleXMLRPCDispatcher and its subclasses can now be used as a decorator.
- bpo-29376: Fix assertion error in threading._DummyThread.is_alive().
- bpo-28624: Add a test that checks that cwd parameter of Popen() accepts PathLike objects. Patch by Sayan Chowdhury.
- bpo-28518: Start a transaction implicitly before a DML statement. Patch by Aviv Palivoda.
- bpo-29742: get_extra_info() raises exception if get called on closed ssl transport. Patch by Nikolay Kim.
- bpo-16285: urllib.parse.quote is now based on RFC 3986 and hence includes â~â in the set of characters that is not quoted by default. Patch by Christian Theune and Ratnadeep Debnath.
- bpo-29532: Altering a kwarg dictionary passed to functools.partial() no longer affects a partial object after creation.
- bpo-29110: Fix file object leak in aifc.open() when file is given as a filesystem path and is not in valid AIFF format. Patch by Anthony Zhang.
- bpo-22807: Add uuid.SafeUUID and uuid.UUID.is_safe to relay information from the platform about whether generated UUIDs are generated with a multiprocessing safe method.
- bpo-29576: Improve some deprecations in importlib. Some deprecated methods now emit DeprecationWarnings and have better descriptive messages.
- bpo-29534: Fixed different behaviour of Decimal.from_float() for _decimal and _pydecimal. Thanks Andrew Nester.
- bpo-10379: locale.format_string now supports the âmonetaryâ keyword argument, and locale.format is deprecated.
- bpo-29851: importlib.reload() now raises ModuleNotFoundError if the module lacks a spec.
- bpo-28556: Various updates to typing module: typing.Counter, typing.ChainMap, improved ABC caching, etc. Original PRs by Jelle Zijlstra, Ivan Levkivskyi, Manuel Krebber, and Åukasz Langa.
- bpo-29100: Fix datetime.fromtimestamp() regression introduced in Python 3.6.0: check minimum and maximum years.
- bpo-29416: Prevent infinite loop in pathlib.Path.mkdir
- bpo-29444: Fixed out-of-bounds buffer access in the group() method of the match object. Based on patch by WGH.
- bpo-29377: Add WrapperDescriptorType, MethodWrapperType, and MethodDescriptorType built-in types to types module. Original patch by Manuel Krebber.
- bpo-29218: Unused install_misc command is now removed. It has been documented as unused since 2000. Patch by Eric N. Vander Weele.
- bpo-29368: The extend() method is now called instead of the append() method when unpickle collections.deque and other list-like objects. This can speed up unpickling to 2 times.
- bpo-29338: The help of a builtin or extension class now includes the constructor signature if __text_signature__ is provided for the class.
- bpo-29335: Fix subprocess.Popen.wait() when the child process has exited to a stopped instead of terminated state (ex: when under ptrace).
- bpo-29290: Fix a regression in argparse that help messages would wrap at non-breaking spaces.
- bpo-28735: Fixed the comparison of mock.MagickMock with mock.ANY.
- bpo-29197: Removed deprecated function ntpath.splitunc().
- bpo-29210: Removed support of deprecated argument âexcludeâ in tarfile.TarFile.add().
- bpo-29219: Fixed infinite recursion in the repr of uninitialized ctypes.CDLL instances.
- bpo-29192: Removed deprecated features in the http.cookies module.
- bpo-29193: A format string argument for string.Formatter.format() is now positional-only.
- bpo-29195: Removed support of deprecated undocumented keyword arguments in methods of regular expression objects.
- bpo-28969: Fixed race condition in C implementation of functools.lru_cache. KeyError could be raised when cached function with full cache was simultaneously called from different threads with the same uncached arguments.
- bpo-20804: The unittest.mock.sentinel attributes now preserve their identity when they are copied or pickled.
- bpo-29142: In urllib.request, suffixes in no_proxy environment variable with leading dots could match related hostnames again (e.g. .b.c matches a.b.c). Patch by Milan Oberkirch.
- bpo-28961: Fix unittest.mock._Call helper: donât ignore the name parameter anymore. Patch written by Jiajun Huang.
- bpo-15812: inspect.getframeinfo() now correctly shows the first line of a context. Patch by Sam Breese.
- bpo-28985: Update authorizer constants in sqlite3 module. Patch by Dingyuan Wang.
- bpo-29079: Prevent infinite loop in pathlib.resolve() on Windows
- bpo-13051: Fixed recursion errors in large or resized curses.textpad.Textbox. Based on patch by Tycho Andersen.
- bpo-9770: curses.ascii predicates now work correctly with negative integers.
- bpo-28427: old keys should not remove new values from WeakValueDictionary when collecting from another thread.
- bpo-28923: Remove editor artifacts from Tix.py.
- bpo-28871: Fixed a crash when deallocate deep ElementTree.
- bpo-19542: Fix bugs in WeakValueDictionary.setdefault() and WeakValueDictionary.pop() when a GC collection happens in another thread.
- bpo-20191: Fixed a crash in resource.prlimit() when passing a sequence that doesnât own its elements as limits.
- bpo-16255: subprocess.Popen uses /system/bin/sh on Android as the shell, instead of /bin/sh.
- bpo-28779: multiprocessing.set_forkserver_preload() would crash the forkserver process if a preloaded module instantiated some multiprocessing objects such as locks.
- bpo-26937: The chown() method of the tarfile.TarFile class does not fail now when the grp module cannot be imported, as for example on Android platforms.
- bpo-28847: dbm.dumb now supports reading read-only files and no longer writes the index file when it is not changed. A deprecation warning is now emitted if the index file is missed and recreated in the ârâ and âwâ modes (will be an error in future Python releases).
- bpo-27030: Unknown escapes consisting of `'\'` and an ASCII letter in
re.sub() replacement templates regular expressions now are errors.
- bpo-28835: Fix a regression introduced in warnings.catch_warnings(): call warnings.showwarning() if it was overridden inside the context manager.
- bpo-27172: To assist with upgrades from 2.7, the previously documented deprecation of `inspect.getfullargspec()` has been reversed. This
decision may be revisited again after the Python 2.7 branch is no longer
officially supported.
- bpo-28740: Add sys.getandroidapilevel(): return the build time API version of Android as an integer. Function only available on Android.
- bpo-26273: Add new `socket.TCP_CONGESTION` (Linux 2.6.13) and`socket.TCP_USER_TIMEOUT` (Linux 2.6.37) constants. Patch written
by Omar Sandoval.
- bpo-28752: Restored the __reduce__() methods of datetime objects.
- bpo-28727: Regular expression patterns, _sre.SRE_Pattern objects created by re.compile(), become comparable (only x==y and x!=y operators). This change should fix the bpo-18383: donât duplicate warning filters when the warnings module is reloaded (thing usually only done in unit tests).
- bpo-20572: Remove the subprocess.Popen.wait endtime parameter. It was deprecated in 3.4 and undocumented prior to that.
- bpo-25659: In ctypes, prevent a crash calling the from_buffer() and from_buffer_copy() methods on abstract classes like Array.
- bpo-28548: In the âhttp.serverâ module, parse the protocol version if possible, to avoid using HTTP 0.9 in some error responses.
- bpo-19717: Makes Path.resolve() succeed on paths that do not exist. Patch by Vajrasky Kok
- bpo-28563: Fixed possible DoS and arbitrary code execution when handle plural form selections in the gettext module. The expression parser now supports exact syntax supported by GNU gettext.
- bpo-28387: Fixed possible crash in _io.TextIOWrapper deallocator when the garbage collector is invoked in other thread. Based on patch by Sebastian Cufre.
- bpo-27517: LZMA compressor and decompressor no longer raise exceptions if given empty data twice. Patch by Benjamin Fogle.
- bpo-28549: Fixed segfault in cursesâs addch() with ncurses6.
- bpo-28449: tarfile.open() with mode ârâ or âr:â now tries to open a tar file with compression before trying to open it without compression. Otherwise it had 50% chance failed with ignore_zeros=True.
- bpo-23262: The webbrowser module now supports Firefox 36+ and derived browsers. Based on patch by Oleg Broytman.
- bpo-24241: The webbrowser in an X environment now prefers using the default browser directly. Also, the webbrowser register() function now has a documented âpreferredâ argument, to specify browsers to be returned by get() with no arguments. Patch by David Steele
- bpo-27939: Fixed bugs in tkinter.ttk.LabeledScale and tkinter.Scale caused by representing the scale as float value internally in Tk. tkinter.IntVar now works if float value is set to underlying Tk variable.
- bpo-28255: calendar.TextCalendar.prweek() no longer prints a space after a weeksâs calendar. calendar.TextCalendar.pryear() no longer prints redundant newline after a yearâs calendar. Based on patch by Xiang Zhang.
- bpo-28255: calendar.TextCalendar.prmonth() no longer prints a space at the start of new line after printing a monthâs calendar. Patch by Xiang Zhang.
- bpo-20491: The textwrap.TextWrapper class now honors non-breaking spaces. Based on patch by Kaarle Ritvanen.
- bpo-28353: os.fwalk() no longer fails on broken links.
- bpo-28430: Fix iterator of C implemented asyncio.Future doesnât accept non-None value is passed to it.send(val).
- bpo-27025: Generated names for Tkinter widgets now start by the â!â prefix for readability.
- bpo-25464: Fixed HList.header_exists() in tkinter.tix module by addin a workaround to Tix library bug.
- bpo-28488: shutil.make_archive() no longer adds entry â./â to ZIP archive.
- bpo-25953: re.sub() now raises an error for invalid numerical group reference in replacement template even if the pattern is not found in the string. Error message for invalid group reference now includes the group index and the position of the reference. Based on patch by SilentGhost.
- bpo-28469: timeit now uses the sequence 1, 2, 5, 10, 20, 50,â¦ instead of 1, 10, 100,â¦ for autoranging.
- bpo-28115: Command-line interface of the zipfile module now uses argparse. Added support of long options.
- bpo-18219: Optimize csv.DictWriter for large number of columns. Patch by Mariatta Wijaya.
- bpo-28448: Fix C implemented asyncio.Future didnât work on Windows.
- bpo-23214: In the âioâ module, the argument to BufferedReader and BytesIOâs read1() methods is now optional and can be -1, matching the BufferedIOBase specification.
- bpo-28480: Fix error building socket module when multithreading is disabled.
- bpo-28240: timeit: remove `-c/--clock` and`-t/--time` command line
options which were deprecated since Python 3.3.
- bpo-28240: timeit now repeats the benchmarks 5 times instead of only 3 to make benchmarks more reliable.
- bpo-28240: timeit autorange now uses a single loop iteration if the benchmark takes less than 10 seconds, instead of 10 iterations. âpython3 -m timeit -s âimport timeâ âtime.sleep(1)ââ now takes 4 seconds instead of 40 seconds.
- Distutils.sdist now looks for README and setup.py files with case sensitivity. This behavior matches that found in Setuptools 6.0 and later. See setuptools 100 for rationale.
- bpo-24452: Make webbrowser support Chrome on Mac OS X. Patch by Ned Batchelder.
- bpo-20766: Fix references leaked by pdb in the handling of SIGINT handlers.
- bpo-27998: Fixed bytes path support in os.scandir() on Windows. Patch by Eryk Sun.
- bpo-28317: The disassembler now decodes FORMAT_VALUE argument.
- bpo-28380: unittest.mock Mock autospec functions now properly support assert_called, assert_not_called, and assert_called_once.
- bpo-28229: lzma module now supports pathlib.
- bpo-28321: Fixed writing non-BMP characters with binary format in plistlib.
- bpo-28225: bz2 module now supports pathlib. Initial patch by Ethan Furman.
- bpo-28227: gzip now supports pathlib. Patch by Ethan Furman.
- bpo-28332: Deprecated silent truncations in socket.htons and socket.ntohs. Original patch by Oren Milman.
- bpo-27358: Optimized merging var-keyword arguments and improved error message when passing a non-mapping as a var-keyword argument.
- bpo-28257: Improved error message when passing a non-iterable as a var-positional argument. Added opcode BUILD_TUPLE_UNPACK_WITH_CALL.
- bpo-28322: Fixed possible crashes when unpickle itertools objects from incorrect pickle data. Based on patch by John Leitch.
- bpo-28228: imghdr now supports pathlib.
- bpo-28226: compileall now supports pathlib.
- bpo-28314: Fix function declaration (C flags) for the getiterator() method of xml.etree.ElementTree.Element.
- bpo-28148: Stop using localtime() and gmtime() in the time module. Introduced platform independent _PyTime_localtime API that is similar to POSIX localtime_r, but available on all platforms. Patch by Ed Schouten.
- bpo-28253: Fixed calendar functions for extreme months: 0001-01 and 9999-12. Methods itermonthdays() and itermonthdays2() are reimplemented so that they donât call itermonthdates() which can cause datetime.date under/overflow.
- bpo-28275: Fixed possible use after free in the decompress() methods of the LZMADecompressor and BZ2Decompressor classes. Original patch by John Leitch.
- bpo-27897: Fixed possible crash in sqlite3.Connection.create_collation() if pass invalid string-like object as a name. Patch by Xiang Zhang.
- bpo-18844: random.choices() now has k as a keyword-only argument to improve the readability of common cases and come into line with the signature used in other languages.
- bpo-18893: Fix invalid exception handling in Lib/ctypes/macholib/dyld.py. Patch by Madison May.
- bpo-27611: Fixed support of default root window in the tkinter.tix module. Added the master parameter in the DisplayStyle constructor.
- bpo-27348: In the traceback module, restore the formatting of exception messages like âException: Noneâ. This fixes a regression introduced in 3.5a2.
- bpo-25651: Allow false values to be used for msg parameter of subTest().
- bpo-27778: Fix a memory leak in os.getrandom() when the getrandom() is interrupted by a signal and a signal handler raises a Python exception.
- bpo-28200: Fix memory leak on Windows in the os module (fix path_converter() function).
- bpo-25400: RobotFileParser now correctly returns default values for crawl_delay and request_rate. Initial patch by Peter Wirtz.
- bpo-27932: Prevent memory leak in win32_ver().
- Fix UnboundLocalError in socket._sendfile_use_sendfile.
- bpo-28075: Check for ERROR_ACCESS_DENIED in Windows implementation of os.stat(). Patch by Eryk Sun.
- bpo-22493: Warning message emitted by using inline flags in the middle of regular expression now contains a (truncated) regex pattern. Patch by Tim Graham.
- bpo-25270: Prevent codecs.escape_encode() from raising SystemError when an empty bytestring is passed.
- bpo-28181: Get antigravity over HTTPS. Patch by Kaartic Sivaraam.
- bpo-25895: Enable WebSocket URL schemes in urllib.parse.urljoin. Patch by Gergely Imreh and Markus Holtermann.
- bpo-28114: Fix a crash in parse_envlist() when env contains byte strings. Patch by Eryk Sun.
- bpo-27599: Fixed buffer overrun in binascii.b2a_qp() and binascii.a2b_qp().
- bpo-27906: Fix socket accept exhaustion during high TCP traffic. Patch by Kevin Conway.
- bpo-28174: Handle when SO_REUSEPORT isnât properly supported. Patch by Seth Michael Larson.
- bpo-26654: Inspect functools.partial in asyncio.Handle.__repr__. Patch by iceboy.
- bpo-26909: Fix slow pipes IO in asyncio. Patch by INADA Naoki.
- bpo-28176: Fix callbacks race in asyncio.SelectorLoop.sock_connect.
- bpo-27759: Fix selectors incorrectly retain invalid file descriptors. Patch by Mark Williams.
- bpo-28325: Remove vestigial MacOS 9 macurl2path module and its tests.
- bpo-28368: Refuse monitoring processes if the child watcher has no loop attached. Patch by Vincent Michel.
- bpo-28369: Raise RuntimeError when transportâs FD is used with add_reader, add_writer, etc.
- bpo-28370: Speedup asyncio.StreamReader.readexactly. Patch by ÐÐ¾ÑÐµÐ½Ð±ÐµÑÐ³ ÐÐ°ÑÐº.
- bpo-28371: Deprecate passing asyncio.Handles to run_in_executor.
- bpo-28372: Fix asyncio to support formatting of non-python coroutines.
- bpo-28399: Remove UNIX socket from FS before binding. Patch by ÐÐ¾ÑÐµÐ½Ð±ÐµÑÐ³ ÐÐ°ÑÐº.
- bpo-27972: Prohibit Tasks to await on themselves.
- bpo-24142: Reading a corrupt config file left configparser in an invalid state. Original patch by Florian HÃ¶ch.
- bpo-29581: ABCMeta.__new__ now accepts `**kwargs` , allowing abstract
base classes to use keyword parameters in __init_subclass__. Patch by Nate
Soares.
- bpo-25532: inspect.unwrap() will now only try to unwrap an object sys.getrecursionlimit() times, to protect against objects which create a new object on every attribute access.
- bpo-30177: path.resolve(strict=False) no longer cuts the path after the first element not present in the filesystem. Patch by Antoine Pietri.

### DocumentationÂ¶

- bpo-31294: Fix incomplete code snippet in the ZeroMQSocketListener and ZeroMQSocketHandler examples and adapt them to Python 3.
- bpo-21649: Add RFC 7525 and Mozilla server side TLS links to SSL documentation.
- bpo-31128: Allow the pydoc server to bind to arbitrary hostnames.
- bpo-30803: Clarify doc on truth value testing. Original patch by Peter Thomassen.
- bpo-30176: Add missing attribute related constants in curses documentation.
