---
id: collect-261121-veille-ia/docs-techniques/changelog-2-20
title: "Changelog"
domain: docs-techniques
role: reference
task: article
actors: []
dates: ["2015-12-06"]
keywords: ["patch", "xiang", "zhang", "fixed", "bytes", "readline", "instead", "return"]
source: https://docs.python.org/release/3.15.1/whatsnew/changelog.html
source_anchor: ""
source_lines: [28501, 30000]
section: "Docs_Techniques"
---
- bpo-26873: xmlrpc now raises ResponseError on unsupported type tags instead of silently return incorrect result.
- bpo-26711: Fixed the comparison of plistlib.Data with other types.
- bpo-24114: Fix an uninitialized variable in `ctypes.util` . The bug only
occurs on SunOS when the ctypes implementation searches for the`crle` program. Patch by Xiang Zhang.  Tested on SunOS by Kees Bos.
- bpo-26864: In urllib.request, change the proxy bypass host checking against no_proxy to be case-insensitive, and to not match unrelated host names that happen to have a bypassed hostname as a suffix. Patch by Xiang Zhang.
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
- bpo-26735: Fix `os.urandom()` on Solaris 11.3 and newer when reading
more than 1,024 bytes: call`getrandom()` multiple times with a limit of
1024 bytes per call.
- bpo-16329: Add .webm to mimetypes.types_map. Patch by Giampaolo Rodolaâ.
- bpo-13952: Add .csv to mimetypes.types_map. Patch by Geoff Wilson.
- bpo-26709: Fixed Y2038 problem in loading binary PLists.
- bpo-23735: Handle terminal resizing with Readline 6.3+ by installing our own SIGWINCH handler. Patch by Eric Price.
- bpo-26586: In http.server, respond with â413 Request header fields too largeâ if there are too many header fields to parse, rather than killing the connection and raising an unhandled exception. Patch by Xiang Zhang.
- bpo-22854: Change BufferedReader.writable() and BufferedWriter.readable() to always return False.
- bpo-25195: Fix a regression in mock.MagicMock. _Call is a subclass of tuple (changeset 3603bae63c13 only works for classes) so we need to implement __ne__ ourselves. Patch by Andrew Plummer.
- bpo-26644: Raise ValueError rather than SystemError when a negative length is passed to SSLSocket.recv() or read().
- bpo-23804: Fix SSL recv(0) and read(0) methods to return zero bytes instead of up to 1024.
- bpo-26616: Fixed a bug in datetime.astimezone() method.
- bpo-21925: `warnings.formatwarning()` now catches exceptions on`linecache.getline(...)` to be able to log`ResourceWarning` emitted late during the Python shutdown process.
- bpo-24266: Ctrl+C during Readline history search now cancels the search mode when compiled with Readline 7.
- bpo-26560: Avoid potential ValueError in BaseHandler.start_response. Initial patch by Peter Inglesby.
- bpo-26569: Fix `pyclbr.readmodule()` and`pyclbr.readmodule_ex()` to support importing packages.
- bpo-26499: Account for remaining Content-Length in HTTPResponse.readline() and read1(). Based on patch by Silent Ghost. Also document that HTTPResponse now supports these methods.
- bpo-25320: Handle sockets in directories unittest discovery is scanning. Patch from Victor van den Elzen.
- bpo-16181: cookiejar.http2time() now returns None if year is higher than datetime.MAXYEAR.
- bpo-26513: Fixes platform module detection of Windows Server
- bpo-23718: Fixed parsing time in week 0 before Jan 1. Original patch by TamÃ¡s Bence Gedai.
- bpo-20589: Invoking Path.owner() and Path.group() on Windows now raise NotImplementedError instead of ImportError.
- bpo-26177: Fixed the keys() method for Canvas and Scrollbar widgets.
- bpo-15068: Got rid of excessive buffering in the fileinput module. The bufsize parameter is no longer used.
- bpo-2202: Fix UnboundLocalError in AbstractDigestAuthHandler.get_algorithm_impls. Initial patch by Mathieu Dupuy.
- bpo-25718: Fixed pickling and copying the accumulate() iterator with total is None.
- bpo-26475: Fixed debugging output for regular expressions with the (?x) flag.
- bpo-26457: Fixed the subnets() methods in IP network classes for the case when resulting prefix length is equal to maximal prefix length. Based on patch by Xiang Zhang.
- bpo-26385: Remove the file if the internal open() call in NamedTemporaryFile() fails. Patch by Silent Ghost.
- bpo-26402: Fix XML-RPC client to retry when the server shuts down a persistent connection. This was a regression related to the new http.client.RemoteDisconnected exception in 3.5.0a4.
- bpo-25913: Leading `<~` is optional now in base64.a85decode() with
adobe=True.  Patch by Swati Jaiswal.
- bpo-26186: Remove an invalid type check in importlib.util.LazyLoader.
- bpo-26367: importlib.__import__() raises SystemError like builtins.__import__() when `level` is specified but without an
accompanying package specified.
- bpo-26309: In the âsocketserverâ module, shut down the request (closing the connected socket) when verify_request() returns false. Patch by Aviv Palivoda.
- bpo-25995: os.walk() no longer uses FDs proportional to the tree depth.
- bpo-26117: The os.scandir() iterator now closes file descriptor not only when the iteration is finished, but when it was failed with error.
- bpo-25911: Restored support of bytes paths in os.walk() on Windows.
- bpo-26045: Add UTF-8 suggestion to error message when posting a non-Latin-1 string with http.client.
- bpo-12923: Reset FancyURLopenerâs redirect counter even if there is an exception. Based on patches by Brian Brazil and Daniel Rocco.
- bpo-25945: Fixed a crash when unpickle the functools.partial object with wrong state. Fixed a leak in failed functools.partial constructor. âargsâ and âkeywordsâ attributes of functools.partial have now always types tuple and dict correspondingly.
- bpo-26202: copy.deepcopy() now correctly copies range() objects with non-atomic attributes.
- bpo-23076: Path.glob() now raises a ValueError if itâs called with an invalid pattern. Patch by Thomas Nyberg.
- bpo-19883: Fixed possible integer overflows in zipimport.
- bpo-26227: On Windows, getnameinfo(), gethostbyaddr() and gethostbyname_ex() functions of the socket module now decode the hostname from the ANSI code page rather than UTF-8.
- bpo-26147: xmlrpc now works with strings not encodable with used non-UTF-8 encoding.
- bpo-25935: Garbage collector now breaks reference loops with OrderedDict.
- bpo-16620: Fixed AttributeError in msilib.Directory.glob().
- bpo-26013: Added compatibility with broken protocol 2 pickles created in old Python 3 versions (3.4.3 and lower).
- bpo-25850: Use cross-compilation by default for 64-bit Windows.
- bpo-17633: Improve zipimportâs support for namespace packages.
- bpo-24705: Fix sysconfig._parse_makefile not expanding ${} vars appearing before $() vars.
- bpo-22138: Fix mock.patch behavior when patching descriptors. Restore original values after patching. Patch contributed by Sean McCully.
- bpo-25672: In the ssl module, enable the SSL_MODE_RELEASE_BUFFERS mode option if it is safe to do so.
- bpo-26012: Donât traverse into symlinks for `**` pattern in
pathlib.Path.[r]glob().
- bpo-24120: Ignore PermissionError when traversing a tree with pathlib.Path.[r]glob(). Patch by Ulrich Petri.
- bpo-25447: fileinput now uses sys.stdin as-is if it does not have a buffer attribute (restores backward compatibility).
- bpo-25447: Copying the lru_cache() wrapper object now always works, independently from the type of the wrapped object (by returning the original object unchanged).
- bpo-24103: Fixed possible use after free in ElementTree.XMLPullParser.
- bpo-25860: os.fwalk() no longer skips remaining directories when error occurs. Original patch by Samson Lee.
- bpo-25914: Fixed and simplified OrderedDict.__sizeof__.
- bpo-25902: Fixed various refcount issues in ElementTree iteration.
- bpo-25717: Restore the previous behaviour of tolerating most fstat() errors when opening files. This was a regression in 3.5a1, and stopped anonymous temporary files from working in special cases.
- bpo-24903: Fix regression in number of arguments compileall accepts when â-dâ is specified. The check on the number of arguments has been dropped completely as it never worked correctly anyway.
- bpo-25764: In the subprocess module, preserve any exception caused by fork() failure when preexec_fn is used.
- bpo-6478: _strptimeâs regexp cache now is reset after changing timezone with time.tzset().
- bpo-14285: When executing a package with the âpython -m packageâ option, and package initialization fails, a proper traceback is now reported. The ârunpyâ module now lets exceptions from package initialization pass back to the caller, rather than raising ImportError.
- bpo-19771: Also in runpy and the â-mâ option, omit the irrelevant message ââ¦ is a package and cannot be directly executedâ if the package could not even be initialized (e.g. due to a bad `*.pyc` file).
- bpo-25177: Fixed problem with the mean of very small and very large numbers. As a side effect, statistics.mean and statistics.variance should be significantly faster.
- bpo-25718: Fixed copying object with state with boolean value is false.
- bpo-10131: Fixed deep copying of minidom documents. Based on patch by Marian Ganisin.
- bpo-25725: Fixed a reference leak in pickle.loads() when unpickling invalid data including tuple instructions.
- bpo-25663: In the Readline completer, avoid listing duplicate global names, and search the global namespace before searching builtins.
- bpo-25688: Fixed file leak in ElementTree.iterparse() raising an error.
- bpo-23914: Fixed SystemError raised by unpickler on broken pickle data.
- bpo-25691: Fixed crash on deleting ElementTree.Element attributes.
- bpo-25624: ZipFile now always writes a ZIP_STORED header for directory entries. Patch by Dingyuan Wang.
- Skip getaddrinfo if host is already resolved. Patch by A. Jesse Jiryu Davis.
- bpo-26050: Add asyncio.StreamReader.readuntil() method. Patch by ÐÐ°ÑÐº ÐÐ¾ÑÐµÐ½Ð±ÐµÑÐ³.
- bpo-25924: Avoid unnecessary serialization of getaddrinfo(3) calls on OS X versions 10.5 or higher. Original patch by A. Jesse Jiryu Davis.
- bpo-26406: Avoid unnecessary serialization of getaddrinfo(3) calls on current versions of OpenBSD and NetBSD. Patch by A. Jesse Jiryu Davis.
- bpo-26848: Fix asyncio/subprocess.communicate() to handle empty input. Patch by Jack OâConnor.
- bpo-27040: Add loop.get_exception_handler method
- bpo-27041: asyncio: Add loop.create_future method
- bpo-27223: asyncio: Fix _read_ready and _write_ready to respect _conn_lost. Patch by Åukasz Langa.
- bpo-22970: asyncio: Fix inconsistency cancelling Condition.wait. Patch by David Coles.

### IDLEÂ¶

- bpo-5124: Paste with text selected now replaces the selection on X11. This matches how paste works on Windows, Mac, most modern Linux apps, and ttk widgets. Original patch by Serhiy Storchaka.
- bpo-24759: Make clear in idlelib.idle_test.__init__ that the directory is a private implementation of test.test_idle and tool for maintainers.
- bpo-27196: Stop âThemeChangedâ warnings when running IDLE tests. These persisted after other warnings were suppressed in #20567. Apply Serhiy Storchakaâs update_idletasks solution to four test files. Record this additional advice in idle_test/README.txt
- bpo-20567: Revise idle_test/README.txt with advice about avoiding tk warning messages from tests. Apply advice to several IDLE tests.
- bpo-27117: Make colorizer htest and turtledemo work with dark themes. Move code for configuring text widget colors to a new function.
- bpo-26673: When tk reports font size as 0, change to size 10. Such fonts on Linux prevented the configuration dialog from opening.
- bpo-21939: Add test for IDLEâs percolator. Original patch by Saimadhav Heblikar.
- bpo-21676: Add test for IDLEâs replace dialog. Original patch by Saimadhav Heblikar.
- bpo-18410: Add test for IDLEâs search dialog. Original patch by Westley MartÃnez.
- bpo-21703: Add test for IDLEâs undo delegator. Original patch by Saimadhav Heblikar .
- bpo-27044: Add ConfigDialog.remove_var_callbacks to stop memory leaks.
- bpo-23977: Add more asserts to test_delegator.
- bpo-20640: Add tests for idlelib.configHelpSourceEdit. Patch by Saimadhav Heblikar.
- In the âIDLE-console differencesâ section of the IDLE doc, clarify how running with IDLE affects sys.modules and the standard streams.
- bpo-25507: fix incorrect change in IOBinding that prevented printing. Augment IOBinding htest to include all major IOBinding functions.
- bpo-25905: Revert unwanted conversion of â to â RIGHT SINGLE QUOTATION MARK in README.txt and open this and NEWS.txt with âasciiâ. Re-encode CREDITS.txt to utf-8 and open it with âutf-8â.

### DocumentationÂ¶

- bpo-19489: Moved the search box from the sidebar to the header and footer of each page. Patch by Ammar Askar.
- bpo-24136: Document the new **PEP 448** unpacking syntax of 3.5.
- bpo-26736: Used HTTPS for external links in the documentation if possible.
- bpo-6953: Rework the Readline module documentation to group related functions together, and add more details such as what underlying Readline functions and variables are accessed.
- bpo-23606: Adds note to ctypes documentation regarding cdll.msvcrt.
- bpo-25500: Fix documentation to not claim that __import__ is searched for in the global scope.
- bpo-26014: Update 3.x packaging documentation: * âSee alsoâ links to the new docs are now provided in the legacy pages * links to setuptools documentation have been updated

### TestsÂ¶

- bpo-21916: Added tests for the turtle module. Patch by ingrid, Gregory Loyse and Jelle Zijlstra.
- bpo-26523: The multiprocessing thread pool (multiprocessing.dummy.Pool) was untested.
- bpo-26015: Added new tests for pickling iterators of mutable sequences.
- bpo-26325: Added test.support.check_no_resource_warning() to check that no ResourceWarning is emitted.
- bpo-25940: Changed test_ssl to use self-signed.pythontest.net. This avoids relying on svn.python.org, which recently changed root certificate.
- bpo-25616: Tests for OrderedDict are extracted from test_collections into separate file test_ordered_dict.
- bpo-26583: Skip test_timestamp_overflow in test_import if bytecode files cannot be written.

### BuildÂ¶

- bpo-26884: Fix linking extension modules for cross builds. Patch by Xavier de Gaye.
- bpo-22359: Disable the rules for running _freeze_importlib and pgen when cross-compiling. The output of these programs is normally saved with the source code anyway, and is still regenerated when doing a native build. Patch by Xavier de Gaye.
- bpo-27229: Fix the cross-compiling pgen rule for in-tree builds. Patch by Xavier de Gaye.
- bpo-21668: Link audioop, _datetime, _ctypes_test modules to libm, except on Mac OS X. Patch written by Xavier de Gaye.
- bpo-25702: A âwith-lto configure option has been added that will enable link time optimizations at build time during a make profile-opt. Some compilers and toolchains are known to not produce stable code when using LTO, be sure to test things thoroughly before relying on it. It can provide a few % speed up over profile-opt alone.
- bpo-26624: Adds validation of ucrtbase[d].dll version with warning for old versions.
- bpo-17603: Avoid error about nonexistent fileblocks.o file by using a lower-level check for st_blocks in struct stat.
- bpo-26079: Fixing the build output folder for tix-8.4.3.6. Patch by Bjoern Thiel.
- bpo-26465: Update Windows builds to use OpenSSL 1.0.2g.
- bpo-24421: Compile Modules/_math.c once, before building extensions. Previously it could fail to compile properly if the math and cmath builds were concurrent.
- bpo-25348: Added `--pgo` and`--pgo-job` arguments to`PCbuild\build.bat` for building with Profile-Guided Optimization.  The
old`PCbuild\build_pgo.bat` script is now deprecated, and simply calls`PCbuild\build.bat --pgo %*` .
- bpo-25827: Add support for building with ICC to `configure` , including a
new`--with-icc` flag.
- bpo-25696: Fix installation of Python on UNIX with make -j9.
- bpo-26930: Update OS X 10.5+ 32-bit-only installer to build and link with OpenSSL 1.0.2h.
- bpo-26268: Update Windows builds to use OpenSSL 1.0.2f.
- bpo-25136: Support Apple Xcode 7âs new textual SDK stub libraries.
- bpo-24324: Do not enable unreachable code warnings when using gcc as the option does not work correctly in older versions of gcc and has been silently removed as of gcc-4.5.

### WindowsÂ¶

- bpo-27053: Updates make_zip.py to correctly generate library ZIP file.
- bpo-26268: Update the prepare_ssl.py script to handle OpenSSL releases that donât include the contents of the include directory (that is, 1.0.2e and later).
- bpo-26071: bdist_wininst created binaries fail to start and find 32bit Python
- bpo-26073: Update the list of magic numbers in launcher
- bpo-26065: Excludes venv from library when generating embeddable distro.
- bpo-17500: Remove unused and outdated icons. (See also: https://github.com/python/pythondotorg/issues/945)

### Tools/DemosÂ¶

- bpo-26799: Fix python-gdb.py: donât get C types once when the Python code is loaded, but get C types on demand. The C types can change if python-gdb.py is loaded before the Python executable. Patch written by Thomas Ilsche.
- bpo-26271: Fix the Freeze tool to properly use flags passed through configure. Patch by Daniel Shaulov.
- bpo-26489: Add dictionary unpacking support to Tools/parser/unparse.py. Patch by Guo Ci Teo.
- bpo-26316: Fix variable name typo in Argument Clinic.

## Python 3.5.1 finalÂ¶

*Release date: 2015-12-06*

### Core and BuiltinsÂ¶

- bpo-25709: Fixed problem with in-place string concatenation and utf-8 cache.

### WindowsÂ¶

- bpo-25715: Python 3.5.1 installer shows wrong upgrade path and incorrect logic for launcher detection.

## Python 3.5.1 release candidate 1Â¶

*Release date: 2015-11-22*

### Core and BuiltinsÂ¶

- bpo-25630: Fix a possible segfault during argument parsing in functions that accept filesystem paths.
- bpo-23564: Fixed a partially broken sanity check in the _posixsubprocess internals regarding how fds_to_pass were passed to the child. The bug had no actual impact as subprocess.py already avoided it.
- bpo-25388: Fixed tokenizer crash when processing undecodable source code with a null byte.
- bpo-25462: The hash of the key now is calculated only once in most operations in C implementation of OrderedDict.
- bpo-22995: Default implementation of __reduce__ and __reduce_ex__ now rejects builtin types with not defined __new__.
- bpo-25555: Fix parser and AST: fill lineno and col_offset of âargâ node when compiling AST from Python objects.
- bpo-24802: Avoid buffer overreads when int(), float(), compile(), exec() and eval() are passed bytes-like objects. These objects are not necessarily terminated by a null byte, but the functions assumed they were.
- bpo-24726: Fixed a crash and leaking NULL in repr() of OrderedDict that was mutated by direct calls of dict methods.
- bpo-25449: Iterating OrderedDict with keys with unstable hash now raises KeyError in C implementations as well as in Python implementation.
- bpo-25395: Fixed crash when highly nested OrderedDict structures were garbage collected.
- bpo-25274: sys.setrecursionlimit() now raises a RecursionError if the new recursion limit is too low depending at the current recursion depth. Modify also the âlower-water markâ formula to make it monotonic. This mark is used to decide when the overflowed flag of the thread state is reset.
- bpo-24402: Fix input() to prompt to the redirected stdout when sys.stdout.fileno() fails.
- bpo-24806: Prevent builtin types that are not allowed to be subclassed from being subclassed through multiple inheritance.
- bpo-24848: Fixed a number of bugs in UTF-7 decoding of misformed data.
- bpo-25280: Import trace messages emitted in verbose (-v) mode are no longer formatted twice.
- bpo-25003: On Solaris 11.3 or newer, os.urandom() now uses the getrandom() function instead of the getentropy() function. The getentropy() function is blocking to generate very good quality entropy, os.urandom() doesnât need such high-quality entropy.
- bpo-25182: The stdprinter (used as sys.stderr before the io module is imported at startup) now uses the backslashreplace error handler.
- bpo-25131: Make the line number and column offset of set/dict literals and comprehensions correspond to the opening brace.
- bpo-25150: Hide the private `_Py_atomic_`*xxx*
symbols from the
public Python.h header to fix a compilation error with OpenMP.
PyThreadState_GET() becomes an alias to PyThreadState_Get() to avoid ABI
incompatibilities.

### LibraryÂ¶

- bpo-25626: Change three zlib functions to accept sizes that fit in Py_ssize_t, but internally cap those sizes to UINT_MAX. This resolves a regression in 3.5 where GzipFile.read() failed to read chunks larger than 2 or 4 GiB. The change affects the zlib.Decompress.decompress() max_length parameter, the zlib.decompress() bufsize parameter, and the zlib.Decompress.flush() length parameter.
- bpo-25583: Avoid incorrect errors raised by os.makedirs(exist_ok=True) when the OS gives priority to errors such as EACCES over EEXIST.
- bpo-25593: Change semantics of EventLoop.stop() in asyncio.
- bpo-6973: When we know a subprocess.Popen process has died, do not allow the send_signal(), terminate(), or kill() methods to do anything as they could potentially signal a different process.
- bpo-25590: In the Readline completer, only call getattr() once per attribute.
- bpo-25498: Fix a crash when garbage-collecting ctypes objects created by wrapping a memoryview. This was a regression made in 3.5a1. Based on patch by Eryksun.
- bpo-25584: Added âescapeâ to the __all__ list in the glob module.
- bpo-25584: Fixed recursive glob() with patterns starting with `**` .
- bpo-25446: Fix regression in smtplibâs AUTH LOGIN support.
- bpo-18010: Fix the pydoc web serverâs module search function to handle exceptions from importing packages.
- bpo-25554: Got rid of circular references in regular expression parsing.
- bpo-25510: fileinput.FileInput.readline() now returns bââ instead of ââ at the end if the FileInput was opened with binary mode. Patch by Ryosuke Ito.
- bpo-25503: Fixed inspect.getdoc() for inherited docstrings of properties. Original patch by John Mark Vandenberg.
- bpo-25515: Always use os.urandom as a source of randomness in uuid.uuid4.
- bpo-21827: Fixed textwrap.dedent() for the case when largest common whitespace is a substring of smallest leading whitespace. Based on patch by Robert Li.
- bpo-25447: The lru_cache() wrapper objects now can be copied and pickled (by returning the original object unchanged).
- bpo-25390: typing: Donât crash on Union[str, Pattern].
- bpo-25441: asyncio: Raise error from drain() when socket is closed.
- bpo-25410: Cleaned up and fixed minor bugs in C implementation of OrderedDict.
- bpo-25411: Improved Unicode support in SMTPHandler through better use of the email package. Thanks to user simon04 for the patch.
- bpo-25407: Remove mentions of the formatter module being removed in Python 3.6.
- bpo-25406: Fixed a bug in C implementation of OrderedDict.move_to_end() that caused segmentation fault or hang in iterating after moving several items to the start of ordered dict.
- bpo-25364: zipfile now works in threads disabled builds.
- bpo-25328: smtpdâs SMTPChannel now correctly raises a ValueError if both decode_data and enable_SMTPUTF8 are set to true.
- bpo-25316: distutils raises OSError instead of DistutilsPlatformError when MSVC is not installed.
- bpo-25380: Fixed protocol for the STACK_GLOBAL opcode in pickletools.opcodes.
- bpo-23972: Updates asyncio datagram create method allowing reuseport and reuseaddr socket options to be set prior to binding the socket. Mirroring the existing asyncio create_server method the reuseaddr option for datagram sockets defaults to True if the O/S is âposixâ (except if the platform is Cygwin). Patch by Chris Laws.
- bpo-25304: Add asyncio.run_coroutine_threadsafe(). This lets you submit a coroutine to a loop from another thread, returning a concurrent.futures.Future. By Vincent Michel.
- bpo-25232: Fix CGIRequestHandler to split the query from the URL at the first question mark (?) rather than the last. Patch from Xiang Zhang.
- bpo-24657: Prevent CGIRequestHandler from collapsing slashes in the query part of the URL as if it were a path. Patch from Xiang Zhang.
- bpo-24483: C implementation of functools.lru_cache() now calculates keyâs hash only once.
- bpo-22958: Constructor and update method of weakref.WeakValueDictionary now accept the self and the dict keyword arguments.
- bpo-22609: Constructor of collections.UserDict now accepts the self keyword argument.
- bpo-25111: Fixed comparison of traceback.FrameSummary.
- bpo-25262: Added support for BINBYTES8 opcode in Python implementation of unpickler. Highest 32 bits of 64-bit size for BINUNICODE8 and BINBYTES8 opcodes no longer silently ignored on 32-bit platforms in C implementation.
- bpo-25034: Fix string.Formatter problem with auto-numbering and nested format_specs. Patch by Anthon van der Neut.
- bpo-25233: Rewrite the guts of asyncio.Queue and asyncio.Semaphore to be more understandable and correct.
- bpo-25203: Failed readline.set_completer_delims() no longer left the module in inconsistent state.
- bpo-23600: Default implementation of tzinfo.fromutc() was returning wrong results in some cases.
- bpo-23329: Allow the ssl module to be built with older versions of LibreSSL.
- Prevent overflow in _Unpickler_Read.
- bpo-25047: The XML encoding declaration written by Element Tree now respects the letter case given by the user. This restores the ability to write encoding names in uppercase like âUTF-8â, which worked in Python 2.
- bpo-25135: Make deque_clear() safer by emptying the deque before clearing. This helps avoid possible reentrancy issues.
- bpo-19143: platform module now reads Windows version from kernel32.dll to avoid compatibility shims.
- bpo-25092: Fix datetime.strftime() failure when errno was already set to EINVAL.
- bpo-23517: Fix rounding in fromtimestamp() and utcfromtimestamp() methods of datetime.datetime: microseconds are now rounded to nearest with ties going to nearest even integer (ROUND_HALF_EVEN), instead of being rounding towards minus infinity (ROUND_FLOOR). Itâs important that these methods use the same rounding mode than datetime.timedelta to keep the property: (datetime(1970,1,1) + timedelta(seconds=t)) == datetime.utcfromtimestamp(t). It also the rounding mode used by round(float) for example.
- bpo-25155: Fix datetime.datetime.now() and datetime.datetime.utcnow() on Windows to support date after year 2038. It was a regression introduced in Python 3.5.0.
- bpo-25108: Omitted internal frames in traceback functions print_stack(), format_stack(), and extract_stack() called without arguments.
- bpo-25118: Fix a regression of Python 3.5.0 in os.waitpid() on Windows.
- bpo-24684: socket.socket.getaddrinfo() now calls PyUnicode_AsEncodedString() instead of calling the encode() method of the host, to handle correctly custom string with an encode() method which doesnât return a byte string. The encoder of the IDNA codec is now called directly instead of calling the encode() method of the string.
- bpo-25060: Correctly compute stack usage of the BUILD_MAP opcode.
- bpo-24857: Comparing call_args to a long sequence now correctly returns a boolean result instead of raising an exception. Patch by A Kaptur.
- bpo-23144: Make sure that HTMLParser.feed() returns all the data, even when convert_charrefs is True.
- bpo-24982: shutil.make_archive() with the âzipâ format now adds entries for directories (including empty directories) in ZIP file.
- bpo-25019: Fixed a crash caused by setting non-string key of expat parser. Based on patch by John Leitch.
- bpo-16180: Exit pdb if file has syntax error, instead of trapping user in an infinite loop. Patch by Xavier de Gaye.
- bpo-24891: Fix a race condition at Python startup if the file descriptor of stdin (0), stdout (1) or stderr (2) is closed while Python is creating sys.stdin, sys.stdout and sys.stderr objects. These attributes are now set to None if the creation of the object failed, instead of raising an OSError exception. Initial patch written by Marco Paolini.
- bpo-24992: Fix error handling and a race condition (related to garbage collection) in collections.OrderedDict constructor.
- bpo-24881: Fixed setting binary mode in Python implementation of FileIO on Windows and Cygwin. Patch from Akira Li.
- bpo-25578: Fix (another) memory leak in SSLSocket.getpeercer().
- bpo-25530: Disable the vulnerable SSLv3 protocol by default when creating ssl.SSLContext.
- bpo-25569: Fix memory leak in SSLSocket.getpeercert().
- bpo-25471: Sockets returned from accept() shouldnât appear to be nonblocking.
- bpo-25319: When threading.Event is reinitialized, the underlying condition should use a regular lock rather than a recursive lock.
- bpo-21112: Fix regression in unittest.expectedFailure on subclasses. Patch from Berker Peksag.
- bpo-24764: cgi.FieldStorage.read_multi() now ignores the Content-Length header in part headers. Patch written by Peter Landry and reviewed by Pierre Quentel.
- bpo-24913: Fix overrun error in deque.index(). Found by John Leitch and Bryce Darling.
- bpo-24774: Fix docstring in http.server.test. Patch from Chiu-Hsiang Hsu.
- bpo-21159: Improve message in configparser.InterpolationMissingOptionError. Patch from Åukasz Langa.
- bpo-20362: Honour TestCase.longMessage correctly in assertRegex. Patch from Ilia Kurenkov.
- bpo-23572: Fixed functools.singledispatch on classes with false metaclasses. Patch by Ethan Furman.
- asyncio: ensure_future() now accepts awaitable objects.

### IDLEÂ¶

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
- bpo-16893: Replace help.txt with help.html for Idle doc display. The new idlelib/help.html is rstripped Doc/build/html/library/idle.html. It looks better than help.txt and will better document Idle as released. The tkinter html viewer that works for this file was written by Mark Roseman. The now unused EditorWindow.HelpDialog class and helt.txt file are deprecated.
- bpo-24199: Deprecate unused idlelib.idlever with possible removal in 3.6.
- bpo-24790: Remove extraneous code (which also create 2 & 3 conflicts).

### DocumentationÂ¶

- bpo-22558: Add remaining doc links to source code for Python-coded modules. Patch by Yoni Lavi.
- bpo-12067: Rewrite Comparisons section in the Expressions chapter of the language reference. Some of the details of comparing mixed types were incorrect or ambiguous. NotImplemented is only relevant at a lower level than the Expressions chapter. Added details of comparing range() objects, and default behaviour and consistency suggestions for user-defined classes. Patch from Andy Maier.
- bpo-24952: Clarify the default size argument of stack_size() in the âthreadingâ and â_threadâ modules. Patch from Mattip.
- bpo-23725: Overhaul tempfile docs. Note deprecated status of mktemp. Patch from Zbigniew JÄdrzejewski-Szmek.
- bpo-24808: Update the types of some PyTypeObject fields. Patch by Joseph Weston.
- bpo-22812: Fix unittest discovery examples. Patch from Pam McAâNulty.

### TestsÂ¶

- bpo-25449: Added tests for OrderedDict subclasses.
- bpo-25099: Make test_compileall not fail when an entry on sys.path cannot be written to (commonly seen in administrative installs on Windows).
- bpo-23919: Prevents assert dialogs appearing in the test suite.
- `PCbuild\rt.bat` now accepts an unlimited number of arguments to pass
along to regrtest.py.  Previously there was a limit of 9.

### BuildÂ¶

- bpo-24915: Add LLVM support for PGO builds and use the test suite to generate the profile data. Initial patch by Alecsandru Patrascu of Intel.
- bpo-24910: Windows MSIs now have unique display names.
- bpo-24986: It is now possible to build Python on Windows without errors when external libraries are not available.

### WindowsÂ¶

- bpo-25450: Updates shortcuts to start Python in installation directory.
- bpo-25164: Changes default all-users install directory to match per-user directory.
- bpo-25143: Improves installer error messages for unsupported platforms.
- bpo-25163: Display correct directory in installer when using non-default settings.
- bpo-25361: Disables use of SSE2 instructions in Windows 32-bit build
- bpo-25089: Adds logging to installer for case where launcher is not selected on upgrade.
- bpo-25165: Windows uninstallation should not remove launcher if other versions remain
- bpo-25112: py.exe launcher is missing icons
- bpo-25102: Windows installer does not precompile for -O or -OO.
- bpo-25081: Makes Back button in installer go back to upgrade page when upgrading.
- bpo-25091: Increases font size of the installer.
- bpo-25126: Clarifies that the non-web installer will download some components.
- bpo-25213: Restores requestedExecutionLevel to manifest to disable UAC virtualization.
- bpo-25022: Removed very outdated PC/example_nt/ directory.

### Tools/DemosÂ¶

- bpo-25440: Fix output of python-config âextension-suffix.

## Python 3.5.0 finalÂ¶

*Release date: 2015-09-13*

### BuildÂ¶

- bpo-25071: Windows installer should not require TargetDir parameter when installing quietly.

## Python 3.5.0 release candidate 4Â¶

*Release date: 2015-09-09*

### LibraryÂ¶

- bpo-25029: Fixes MemoryError in test_strptime.

### BuildÂ¶

- bpo-25027: Reverts partial-static build options and adds vcruntime140.dll to Windows installation.

## Python 3.5.0 release candidate 3Â¶

*Release date: 2015-09-07*

### Core and BuiltinsÂ¶

### LibraryÂ¶

- bpo-24917: time_strftime() buffer over-read.
- bpo-24748: To resolve a compatibility problem found with py2exe and pywin32, imp.load_dynamic() once again ignores previously loaded modules to support Python modules replacing themselves with extension modules. Patch by Petr Viktorin.
- bpo-24635: Fixed a bug in typing.py where isinstance([], typing.Iterable) would return True once, then False on subsequent calls.
- bpo-24989: Fixed buffer overread in BytesIO.readline() if a position is set beyond size. Based on patch by John Leitch.
- bpo-24913: Fix overrun error in deque.index(). Found by John Leitch and Bryce Darling.

## Python 3.5.0 release candidate 2Â¶

*Release date: 2015-08-25*

### Core and BuiltinsÂ¶

- bpo-24769: Interpreter now starts properly when dynamic loading is disabled. Patch by Petr Viktorin.
- bpo-21167: NAN operations are now handled correctly when python is compiled with ICC even if -fp-model strict is not specified.
- bpo-24492: A âpackageâ lacking a __name__ attribute when trying to perform a `from .. import ...` statement will trigger an ImportError instead of
an AttributeError.

### LibraryÂ¶

## Python 3.5.0 release candidate 1Â¶

*Release date: 2015-08-09*

### Core and BuiltinsÂ¶

- bpo-24667: Resize odict in all cases that the underlying dict resizes.

### LibraryÂ¶

- bpo-24824: Signatures of codecs.encode() and codecs.decode() now are compatible with pydoc.
- bpo-24634: Importing uuid should not try to load libc on Windows
- bpo-24798: _msvccompiler.py doesnât properly support manifests
- bpo-4395: Better testing and documentation of binary operators. Patch by Martin Panter.
- bpo-23973: Update typing.py from GitHub repo.
- bpo-23004: mock_open() now reads binary data correctly when the type of read_data is bytes. Initial patch by Aaron Hill.
- bpo-23888: Handle fractional time in cookie expiry. Patch by ssh.
- bpo-23652: Make it possible to compile the select module against the libc headers from the Linux Standard Base, which do not include some EPOLL macros. Patch by Matt Frank.
- bpo-22932: Fix timezones in email.utils.formatdate. Patch from Dmitry Shachnev.
- bpo-23779: imaplib raises TypeError if authenticator tries to abort. Patch from Craig Holmquist.
- bpo-23319: Fix ctypes.BigEndianStructure, swap correctly bytes. Patch written by Matthieu Gautier.
- bpo-23254: Document how to close the TCPServer listening socket. Patch from Martin Panter.
- bpo-19450: Update Windows and OS X installer builds to use SQLite 3.8.11.
- bpo-17527: Add PATCH to wsgiref.validator. Patch from Luca Sbardella.
- bpo-24791: Fix grammar regression for call syntax: âg(*a or b)â.

### IDLEÂ¶

- bpo-23672: Allow Idle to edit and run files with astral chars in name. Patch by Mohd Sanad Zaki Rizvi.
- bpo-24745: Idle editor default font. Switch from Courier to platform-sensitive TkFixedFont. This should not affect current customized font selections. If there is a problem, edit $HOME/.idlerc/config-main.cfg and remove â `font`*xxx*
â entries from
[Editor Window].  Patch by Mark Roseman.
- bpo-21192: Idle editor. When a file is run, put its name in the restart bar. Do not print false prompts. Original patch by Adnan Umer.
- bpo-13884: Idle menus. Remove tearoff lines. Patch by Roger Serwy.

### DocumentationÂ¶

- bpo-24129: Clarify the reference documentation for name resolution. This includes removing the assumption that readers will be familiar with the name resolution scheme Python used prior to the introduction of lexical scoping for function namespaces. Patch by Ivan Levkivskyi.
- bpo-20769: Improve reload() docs. Patch by Dorian Pula.
- bpo-23589: Remove duplicate sentence from the FAQ. Patch by Yongzhi Pan.
- bpo-24729: Correct IO tutorial to match implementation regarding encoding parameter to open function.

### TestsÂ¶

- bpo-24751: When running regrtest with the `-w` command line option, a
test run is no longer marked as a failure if all tests succeed when
re-run.

## Python 3.5.0 beta 4Â¶

*Release date: 2015-07-26*

### Core and BuiltinsÂ¶

- bpo-23573: Restored optimization of bytes.rfind() and bytearray.rfind() for single-byte argument on Linux.
- bpo-24569: Make **PEP 448** dictionary evaluation more consistent.
- bpo-24583: Fix crash when set is mutated while being updated.
- bpo-24407: Fix crash when dict is mutated while being updated.
- bpo-24619: New approach for tokenizing async/await. As a consequence, it is now possible to have one-line âasync def foo(): await ..â functions.
- bpo-24687: Plug refleak on SyntaxError in function parameters annotations.
- bpo-15944: memoryview: Allow arbitrary formats when casting to bytes. Patch by Martin Panter.

### LibraryÂ¶

- bpo-23441: rcompleter now prints a tab character instead of displaying possible completions for an empty word. Initial patch by Martin Sekera.
- bpo-24683: Fixed crashes in _json functions called with arguments of inappropriate type.
- bpo-21697: shutil.copytree() now correctly handles symbolic links that point to directories. Patch by Eduardo Seabra and Thomas Kluyver.
- bpo-14373: Fixed segmentation fault when gc.collect() is called during constructing lru_cache (C implementation).
- bpo-24695: Fix a regression in traceback.print_exception(). If exc_traceback is None we shouldnât print a traceback header like described in the documentation.
- bpo-24620: Random.setstate() now validates the value of state last element.
- bpo-22485: Fixed an issue that caused `inspect.getsource` to return
incorrect results on nested functions.
- bpo-22153: Improve unittest docs. Patch from Martin Panter and evilzero.
- bpo-24580: Symbolic group references to open group in re patterns now are explicitly forbidden as well as numeric group references.
- bpo-24206: Fixed __eq__ and __ne__ methods of inspect classes.
- bpo-24631: Fixed regression in the timeit module with multiline setup.
- bpo-18622: unittest.mock.mock_open().reset_mock would recurse infinitely. Patch from Nicola Palumbo and Laurent De Buyst.
- bpo-23661: unittest.mock side_effects can now be exceptions again. This was a regression vs Python 3.4. Patch from Ignacio Rossi
- bpo-24608: chunk.Chunk.read() now always returns bytes, not str.
- bpo-18684: Fixed reading out of the buffer in the re module.
- bpo-24259: tarfile now raises a ReadError if an archive is truncated inside a data segment.
- bpo-15014: SMTP.auth() and SMTP.login() now support RFC 4954âs optional initial-response argument to the SMTP AUTH command.
- bpo-24669: Fix inspect.getsource() for âasync defâ functions. Patch by Kai Groner.
- bpo-24688: ast.get_docstring() for âasync defâ functions.

### BuildÂ¶

- bpo-24603: Update Windows builds and OS X 10.5 installer to use OpenSSL 1.0.2d.

## Python 3.5.0 beta 3Â¶

*Release date: 2015-07-05*

### Core and BuiltinsÂ¶

- bpo-24467: Fixed possible buffer over-read in bytearray. The bytearray object now always allocates place for trailing null byte and itâs buffer now is always null-terminated.
- Upgrade to Unicode 8.0.0.
- bpo-24345: Add Py_tp_finalize slot for the stable ABI.
- bpo-24400: Introduce a distinct type for **PEP 492** coroutines; add
types.CoroutineType, inspect.getcoroutinestate,
inspect.getcoroutinelocals; coroutines no longer use CO_GENERATOR flag;
sys.set_coroutine_wrapper works only for âasync defâ coroutines;
inspect.iscoroutine no longer uses collections.abc.Coroutine, itâs
intended to test for pure âasync defâ coroutines only; add new opcode:
GET_YIELD_FROM_ITER; fix generators wrapper used in types.coroutine to be
instance of collections.abc.Generator; collections.abc.Awaitable and
collections.abc.Coroutine can no longer be used to detect generator-based
coroutinesâuse inspect.isawaitable instead.
- bpo-24450: Add gi_yieldfrom to generators and cr_await to coroutines. Contributed by Benno Leslie and Yury Selivanov.
- bpo-19235: Add new RecursionError exception. Patch by Georg Brandl.

### LibraryÂ¶

- bpo-21750: mock_open.read_data can now be read from each instance, as it could in Python 3.3.
- bpo-24552: Fix use after free in an error case of the _pickle module.
- bpo-24514: tarfile now tolerates number fields consisting of only whitespace.
- bpo-19176: Fixed doctype() related bugs in C implementation of ElementTree. A deprecation warning no longer issued by XMLParser subclass with default doctype() method. Direct call of doctype() now issues a warning. Parserâs doctype() now is not called if targetâs doctype() is called. Based on patch by Martin Panter.
- bpo-20387: Restore semantic round-trip correctness in tokenize/untokenize for tab-indented blocks.
- bpo-24456: Fixed possible buffer over-read in adpcm2lin() and lin2adpcm() functions of the audioop module.
- bpo-24336: The contextmanager decorator now works with functions with keyword arguments called âfuncâ and âselfâ. Patch by Martin Panter.
- bpo-24522: Fix possible integer overflow in json accelerator module.
- bpo-24489: ensure a previously set C errno doesnât disturb cmath.polar().
- bpo-24408: Fixed AttributeError in measure() and metrics() methods of tkinter.Font.
- bpo-14373: C implementation of functools.lru_cache() now can be used with methods.
- bpo-24347: Set KeyError if PyDict_GetItemWithError returns NULL.
- bpo-24348: Drop superfluous incref/decref.
- bpo-24359: Check for changed OrderedDict size during iteration.
- bpo-24368: Support keyword arguments in OrderedDict methods.
- bpo-24362: Simplify the C OrderedDict fast nodes resize logic.
- bpo-24377: Fix a ref leak in OrderedDict.__repr__.
- bpo-24369: Defend against key-changes during iteration.

### TestsÂ¶

### DocumentationÂ¶

### BuildÂ¶

- bpo-24432: Update Windows builds and OS X 10.5 installer to use OpenSSL 1.0.2c.

## Python 3.5.0 beta 2Â¶

*Release date: 2015-05-31*

### Core and BuiltinsÂ¶

- bpo-24284: The startswith and endswith methods of the str class no longer return True when finding the empty string and the indexes are completely out of range.
- bpo-24115: Update uses of PyObject_IsTrue(), PyObject_Not(), PyObject_IsInstance(), PyObject_RichCompareBool() and _PyDict_Contains() to check for and handle errors correctly.
- bpo-24328: Fix importing one character extension modules.
- bpo-11205: In dictionary displays, evaluate the key before the value.
- bpo-24285: Fixed regression that prevented importing extension modules from inside packages. Patch by Petr Viktorin.

### LibraryÂ¶

- bpo-23247: Fix a crash in the StreamWriter.reset() of CJK codecs.
- bpo-24270: Add math.isclose() and cmath.isclose() functions as per **PEP 485** . Contributed by Chris Barker and Tal Einat.
- bpo-5633: Fixed timeit when the statement is a string and the setup is not.
- bpo-24326: Fixed audioop.ratecv() with non-default weightB argument. Original patch by David Moore.
- bpo-16991: Add a C implementation of OrderedDict.
- bpo-23934: Fix inspect.signature to fail correctly for builtin types lacking signature information. Initial patch by James Powell.

## Python 3.5.0 beta 1Â¶

*Release date: 2015-05-24*

### Core and BuiltinsÂ¶

- bpo-24276: Fixed optimization of property descriptor getter.
- bpo-24268: PEP 489: Multi-phase extension module initialization. Patch by Petr Viktorin.
- bpo-23955: Add pyvenv.cfg option to suppress registry/environment lookup for generating sys.path on Windows.
- bpo-24257: Fixed system error in the comparison of faked types.SimpleNamespace.
- bpo-22939: Fixed integer overflow in iterator object. Patch by Clement Rouault.
- bpo-23985: Fix a possible buffer overrun when deleting a slice from the front of a bytearray and then appending some other bytes data.
- bpo-24102: Fixed exception type checking in standard error handlers.
- bpo-15027: The UTF-32 encoder is now 3x to 7x faster.
- bpo-23290: Optimize set_merge() for cases where the target is empty. (Contributed by Serhiy Storchaka.)
- bpo-2292: PEP 448: Additional Unpacking Generalizations.
- bpo-24096: Make warnings.warn_explicit more robust against mutation of the warnings.filters list.
- bpo-23996: Avoid a crash when a delegated generator raises an unnormalized StopIteration exception. Patch by Stefan Behnel.
- bpo-23910: Optimize property() getter calls. Patch by Joe Jevnik.
- bpo-23911: Move path-based importlib bootstrap code to a separate frozen module.
- bpo-24192: Fix namespace package imports.
- bpo-24022: Fix tokenizer crash when processing undecodable source code.
- bpo-9951: Added a hex() method to bytes, bytearray, and memoryview.
- bpo-22906: PEP 479: Change StopIteration handling inside generators.
- bpo-24017: PEP 492: Coroutines with async and await syntax.

### LibraryÂ¶

- bpo-14373: Added C implementation of functools.lru_cache(). Based on patches by Matt Joiner and Alexey Kachayev.
- bpo-24230: The tempfile module now accepts bytes for prefix, suffix and dir parameters and returns bytes in such situations (matching the os module APIs).
- bpo-22189: collections.UserString now supports __getnewargs__(), __rmod__(), casefold(), format_map(), isprintable(), and maketrans(). Patch by Joe Jevnik.
- bpo-24244: Prevents termination when an invalid format string is encountered on Windows in strftime.
- bpo-23973: PEP 484: Add the typing module.
- bpo-23086: The collections.abc.Sequence() abstract base class added *start* and*stop* parameters to the index() mixin. Patch by Devin
Jeanpierre.
- bpo-20035: Replaced the `tkinter._fix` module used for setting up the
Tcl/Tk environment on Windows with a private function in the`_tkinter` module that makes no permanent changes to the environment.
- bpo-24257: Fixed segmentation fault in sqlite3.Row constructor with faked cursor type.
- bpo-15836: assertRaises(), assertRaisesRegex(), assertWarns() and assertWarnsRegex() assertments now check the type of the first argument to prevent possible user error. Based on patch by Daniel Wagner-Hall.
- bpo-9858: Add missing method stubs to _io.RawIOBase. Patch by Laura Rupprecht.
- bpo-22955: attrgetter, itemgetter and methodcaller objects in the operator module now support pickling. Added readable and evaluable repr for these objects. Based on patch by Josh Rosenberg.
- bpo-22107: tempfile.gettempdir() and tempfile.mkdtemp() now try again when a directory with the chosen name already exists on Windows as well as on Unix. tempfile.mkstemp() now fails early if parent directory is not valid (not exists or is a file) on Windows.
- bpo-23780: Improved error message in os.path.join() with single argument.
- bpo-6598: Increased time precision and random number range in email.utils.make_msgid() to strengthen the uniqueness of the message ID.
- bpo-24091: Fixed various crashes in corner cases in C implementation of ElementTree.
- bpo-21931: msilib.FCICreate() now raises TypeError in the case of a bad argument instead of a ValueError with a bogus FCI error number. Patch by Jeffrey Armstrong.
- bpo-13866: *quote_via* argument added to urllib.parse.urlencode.
- bpo-20098: New mangle_from policy option for email, default True for compat32, but False for all other policies.
- bpo-24211: The email library now supports RFC 6532: it can generate headers using utf-8 instead of encoded words.
- bpo-16314: Added support for the LZMA compression in distutils.
- bpo-21804: poplib now supports RFC 6856 (UTF8).
- bpo-18682: Optimized pprint functions for builtin scalar types.
- bpo-22027: smtplib now supports RFC 6531 (SMTPUTF8).
- bpo-23488: Random generator objects now consume 2x less memory on 64-bit.
- bpo-1322: platform.dist() and platform.linux_distribution() functions are now deprecated. Initial patch by Vajrasky Kok.
- bpo-22486: Added the math.gcd() function. The fractions.gcd() function now is deprecated. Based on patch by Mark Dickinson.
- bpo-24064: Property() docstrings are now writeable. (Patch by Berker Peksag.)
- bpo-22681: Added support for the koi8_t encoding.
- bpo-22682: Added support for the kz1048 encoding.
- bpo-23796: peek and read1 methods of BufferedReader now raise ValueError if they called on a closed object. Patch by John Hergenroeder.
- bpo-21795: smtpd now supports the 8BITMIME extension whenever the new *decode_data* constructor argument is set to False.
- bpo-24155: optimize heapq.heapify() for better cache performance when heapifying large lists.
- bpo-21800: imaplib now supports RFC 5161 (enable), RFC 6855 (utf8/internationalized email) and automatically encodes non-ASCII usernames and passwords to UTF8.
- bpo-20274: When calling a _sqlite.Connection, it now complains if passed any keyword arguments. Previously it silently ignored them.
- bpo-20274: Remove ignored and erroneous âkwargsâ parameters from three METH_VARARGS methods on _sqlite.Connection.
- bpo-24134: assertRaises(), assertRaisesRegex(), assertWarns() and assertWarnsRegex() checks now emits a deprecation warning when callable is None or keyword arguments except msg is passed in the context manager mode.
- bpo-24018: Add a collections.abc.Generator abstract base class. Contributed by Stefan Behnel.
- bpo-23880: Tkinterâs getint() and getdouble() now support Tcl_Obj. Tkinterâs getdouble() now supports any numbers (in particular int).
- bpo-22619: Added negative limit support in the traceback module. Based on patch by Dmitry Kazakov.
- bpo-24094: Fix possible crash in json.encode with poorly behaved dict subclasses.
- bpo-9246: On POSIX, os.getcwd() now supports paths longer than 1025 bytes. Patch written by William Orr.
- bpo-17445: add difflib.diff_bytes() to support comparison of byte strings (fixes a regression from Python 2).
- bpo-23917: Fall back to sequential compilation when ProcessPoolExecutor doesnât exist. Patch by Claudiu Popa.
- bpo-23008: Fixed resolving attributes with boolean value is False in pydoc.
- Fix asyncio issue 235: LifoQueue and PriorityQueueâs put didnât increment unfinished tasks (this bug was introduced when JoinableQueue was merged with Queue).
- bpo-23908: os functions now reject paths with embedded null character on Windows instead of silently truncating them.
- bpo-23728: binascii.crc_hqx() could return an integer outside of the range 0-0xffff for empty data.
- bpo-23887: urllib.error.HTTPError now has a proper repr() representation. Patch by Berker Peksag.
- asyncio: New event loop APIs: set_task_factory() and get_task_factory().
- asyncio: async() function is deprecated in favour of ensure_future().
- bpo-24178: asyncio.Lock, Condition, Semaphore, and BoundedSemaphore support new âasync withâ syntax. Contributed by Yury Selivanov.
- bpo-24179: Support âasync forâ for asyncio.StreamReader. Contributed by Yury Selivanov.
- bpo-24184: Add AsyncIterator and AsyncIterable ABCs to collections.abc. Contributed by Yury Selivanov.
- bpo-22547: Implement informative __repr__ for inspect.BoundArguments. Contributed by Yury Selivanov.
- bpo-24190: Implement inspect.BoundArgument.apply_defaults() method. Contributed by Yury Selivanov.
- bpo-20691: Add âfollow_wrappedâ argument to inspect.Signature.from_callable() and inspect.signature(). Contributed by Yury Selivanov.
- bpo-24248: Deprecate inspect.Signature.from_function() and inspect.Signature.from_builtin().
- bpo-23898: Fix inspect.classify_class_attrs() to support attributes with overloaded __eq__ and __bool__. Patch by Mike Bayer.
- bpo-24298: Fix inspect.signature() to correctly unwrap wrappers around bound methods.

### IDLEÂ¶

- bpo-23184: remove unused names and imports in idlelib. Initial patch by Al Sweigart.

### TestsÂ¶

### DocumentationÂ¶

### Tools/DemosÂ¶

- bpo-24000: Improved Argument Clinicâs mapping of converters to legacy âformat unitsâ. Updated the documentation to match.
- bpo-24001: Argument Clinic converters now use accept={type} instead of types={âtypeâ} to specify the types the converter accepts.
- bpo-23330: h2py now supports arbitrary filenames in #include.
- bpo-24031: make patchcheck now supports git checkouts, too.

## Python 3.5.0 alpha 4Â¶

*Release date: 2015-04-19*

### Core and BuiltinsÂ¶

- bpo-22980: Under Linux, GNU/KFreeBSD and the Hurd, C extensions now include the architecture triplet in the extension name, to make it easy to test builds for different ABIs in the same working tree. Under OS X, the extension name now includes **PEP 3149** -style information.
- bpo-22631: Added Linux-specific socket constant CAN_RAW_FD_FRAMES. Patch courtesy of Joe Jevnik.
- bpo-23731: Implement **PEP 488** : removal of .pyo files.
- bpo-23726: Donât enable GC for user subclasses of non-GC types that donât add any new fields. Patch by Eugene Toder.
- bpo-23309: Avoid a deadlock at shutdown if a daemon thread is aborted while it is holding a lock to a buffered I/O object, and the main thread tries to use the same I/O object (typically stdout or stderr). A fatal error is emitted instead.
- bpo-22977: Fixed formatting Windows error messages on Wine. Patch by Martin Panter.
- bpo-23466: %c, %o, %x, and %X in bytes formatting now raise TypeError on non-integer input.
- bpo-24044: Fix possible null pointer dereference in list.sort in out of memory conditions.
- bpo-21354: PyCFunction_New function is exposed by python DLL again.

### LibraryÂ¶

- bpo-23840: tokenize.open() now closes the temporary binary file on error to fix a resource warning.
- bpo-16914: new debuglevel 2 in smtplib adds timestamps to debug output.
- bpo-7159: urllib.request now supports sending auth credentials automatically after the first 401. This enhancement is a superset of the enhancement from bpo-19494 and supersedes that change.
- bpo-23703: Fix a regression in urljoin() introduced in 901e4e52b20a. Patch by Demian Brecht.
- bpo-4254: Adds _curses.update_lines_cols(). Patch by Arnon Yaari
- bpo-19933: Provide default argument for ndigits in round. Patch by Vajrasky Kok.
- bpo-23193: Add a numeric_owner parameter to tarfile.TarFile.extract and tarfile.TarFile.extractall. Patch by Michael Vogt and Eric Smith.
- bpo-23342: Add a subprocess.run() function than returns a CalledProcess instance for a more consistent API than the existing call* functions.
- bpo-21217: inspect.getsourcelines() now tries to compute the start and end lines from the code object, fixing an issue when a lambda function is used as decorator argument. Patch by Thomas Ballinger and Allison Kaptur.
- bpo-24521: Fix possible integer overflows in the pickle module.
- bpo-22931: Allow â[â and â]â in cookie values.
- The keywords attribute of functools.partial is now always a dictionary.
- bpo-23811: Add missing newline to the PyCompileError error message. Patch by Alex Shkop.
- bpo-21116: Avoid blowing memory when allocating a multiprocessing shared array thatâs larger than 50% of the available RAM. Patch by MÃ©dÃ©ric Boquien.
- bpo-22982: Improve BOM handling when seeking to multiple positions of a writable text file.
- bpo-23464: Removed deprecated asyncio JoinableQueue.
- bpo-23529: Limit the size of decompressed data when reading from GzipFile, BZ2File or LZMAFile. This defeats denial of service attacks using compressed bombs (i.e. compressed payloads which decompress to a huge size). Patch by Martin Panter and Nikolaus Rath.
- bpo-21859: Added Python implementation of io.FileIO.
- bpo-23865: close() methods in multiple modules now are idempotent and more robust at shutdown. If they need to release multiple resources, all are released even if errors occur.
- bpo-23400: Raise same exception on both Python 2 and 3 if sem_open is not available. Patch by Davin Potts.
- bpo-10838: The subprocess now module includes SubprocessError and TimeoutError in its list of exported names for the users wild enough to use `from subprocess import *` .
- bpo-23411: Added DefragResult, ParseResult, SplitResult, DefragResultBytes, ParseResultBytes, and SplitResultBytes to urllib.parse.__all__. Patch by Martin Panter.
- bpo-23881: urllib.request.ftpwrapper constructor now closes the socket if the FTP connection failed to fix a ResourceWarning.
- bpo-23853: `socket.socket.sendall()` does no more reset the socket
timeout each time data is sent successfully. The socket timeout is now the
maximum total duration to send all data.
- bpo-22721: An order of multiline pprint output of set or dict containing orderable and non-orderable elements no longer depends on iteration order of set or dict.
- bpo-15133: _tkinter.tkapp.getboolean() now supports Tcl_Obj and always returns bool. tkinter.BooleanVar now validates input values (accepted bool, int, str, and Tcl_Obj). tkinter.BooleanVar.get() now always returns bool.
- bpo-10590: xml.sax.parseString() now supports string argument.
- bpo-23338: Fixed formatting ctypes error messages on Cygwin. Patch by Makoto Kato.
- bpo-15582: inspect.getdoc() now follows inheritance chains.
- bpo-2175: SAX parsers now support a character stream of InputSource object.
- bpo-16840: Tkinter now supports 64-bit integers added in Tcl 8.4 and arbitrary precision integers added in Tcl 8.5.
- bpo-23834: Fix socket.sendto(), use the C Py_ssize_t type to store the result of sendto() instead of the C int type.
- bpo-23618: `socket.socket.connect()` now waits until the connection
completes instead of raising`InterruptedError` if the connection is
interrupted by signals, signal handlers donât raise an exception and the
socket is blocking or has a timeout.`socket.socket.connect()` still
raise`InterruptedError` for non-blocking sockets.
- bpo-21526: Tkinter now supports new boolean type in Tcl 8.5.
- bpo-23836: Fix the faulthandler module to handle reentrant calls to its signal handlers.
- bpo-23838: linecache now clears the cache and returns an empty result on MemoryError.
- bpo-10395: Added os.path.commonpath(). Implemented in posixpath and ntpath. Based on patch by Rafik Draoui.
- bpo-23611: Serializing more âlookupableâ objects (such as unbound methods or nested classes) now are supported with pickle protocols < 4.
- bpo-13583: sqlite3.Row now supports slice indexing.
- bpo-18473: Fixed 2to3 and 3to2 compatible pickle mappings. Fixed ambiguous reverse mappings. Added many new mappings. Import mapping is no longer applied to modules already mapped with full name mapping.
- bpo-23485: select.select() is now retried automatically with the recomputed timeout when interrupted by a signal, except if the signal handler raises an exception. This change is part of the **PEP 475** .
- bpo-23752: When built from an existing file descriptor, io.FileIO() now only calls fstat() once. Before fstat() was called twice, which was not necessary.
- bpo-23704: collections.deque() objects now support __add__, __mul__, and __imul__().
- bpo-23171: csv.Writer.writerow() now supports arbitrary iterables.
- bpo-23745: The new email header parser now handles duplicate MIME parameter names without error, similar to how get_param behaves.
- bpo-22117: Fix os.utime(), it now rounds the timestamp towards minus infinity (-inf) instead of rounding towards zero.
- bpo-23310: Fix MagicMockâs initializer to work with __methods__, just like configure_mock(). Patch by Kasia Jachim.

### BuildÂ¶

### TestsÂ¶

### Tools/DemosÂ¶

- bpo-18128: pygettext now uses standard +NNNN format in the POT-Creation-Date header.
- bpo-23935: Argument Clinicâs understanding of format units accepting bytes, bytearrays, and buffers is now consistent with both the documentation and the implementation.
- bpo-23944: Argument Clinic now wraps long impl prototypes at column 78.
- bpo-20586: Argument Clinic now ensures that functions without docstrings have signatures.
- bpo-23492: Argument Clinic now generates argument parsing code with PyArg_Parse instead of PyArg_ParseTuple if possible.
- bpo-23500: Argument Clinic is now smarter about generating the â#ifndefâ (empty) definition of the methoddef macro: itâs only generated once, even if Argument Clinic processes the same symbol multiple times, and itâs emitted at the end of all processing rather than immediately after the first use.

### C APIÂ¶

- bpo-23998: PyImport_ReInitLock() now checks for lock allocation error

## Python 3.5.0 alpha 3Â¶

*Release date: 2015-03-28*

### Core and BuiltinsÂ¶

- bpo-23573: Increased performance of string search operations (str.find, str.index, str.count, the in operator, str.split, str.partition) with arguments of different kinds (UCS1, UCS2, UCS4).
- bpo-23753: Python doesnât support anymore platforms without stat() or fstat(), these functions are always required.
- bpo-23681: The -b option now affects comparisons of bytes with int.
- bpo-23632: Memoryviews now allow tuple indexing (including for multi-dimensional memoryviews).
- bpo-23192: Fixed generator lambdas. Patch by Bruno Cauet.
- bpo-23629: Fix the default __sizeof__ implementation for variable-sized objects.

### LibraryÂ¶

- bpo-14260: The groupindex attribute of regular expression pattern object now is non-modifiable mapping.
- bpo-23792: Ignore KeyboardInterrupt when the pydoc pager is active. This mimics the behavior of the standard unix pagers, and prevents pipepager from shutting down while the pager itself is still running.
- bpo-23775: pprint() of OrderedDict now outputs the same representation as repr().
- bpo-23765: Removed IsBadStringPtr calls in ctypes
- bpo-22364: Improved some re error messages using regex for hints.
- bpo-23742: ntpath.expandvars() no longer loses unbalanced single quotes.
- bpo-21717: The zipfile.ZipFile.open function now supports âxâ (exclusive creation) mode.
- bpo-21802: The reader in BufferedRWPair now is closed even when closing writer failed in BufferedRWPair.close().
- bpo-23622: Unknown escapes in regular expressions that consist of `'\'` and ASCII letter now raise a deprecation warning and will be forbidden in
Python 3.6.
- bpo-23671: string.Template now allows specifying the âselfâ parameter as a keyword argument. string.Formatter now allows specifying the âselfâ and the âformat_stringâ parameters as keyword arguments.
- bpo-23502: The pprint module now supports mapping proxies.
- bpo-17530: pprint now wraps long bytes objects and bytearrays.
- bpo-22687: Fixed some corner cases in breaking words in tetxtwrap. Got rid of quadratic complexity in breaking long words.
- bpo-4727: The copy module now uses pickle protocol 4 (PEP 3154) and supports copying of instances of classes whose __new__ method takes keyword-only arguments.
- bpo-23491: Added a zipapp module to support creating executable zip file archives of Python code. Registered â.pyzâ and â.pyzwâ extensions on Windows for these archives (PEP 441).
- bpo-23657: Avoid explicit checks for str in zipapp, adding support for pathlib.Path objects as arguments.
- bpo-23688: Added support of arbitrary bytes-like objects and avoided unnecessary copying of memoryview in gzip.GzipFile.write(). Original patch by Wolfgang Maier.
- bpo-23252: Added support for writing ZIP files to unseekable streams.
- bpo-23647: Increase imaplibâs MAXLINE to accommodate modern mailbox sizes.
- bpo-23539: If body is None, http.client.HTTPConnection.request now sets Content-Length to 0 for PUT, POST, and PATCH headers to avoid 411 errors from some web servers.
- bpo-22351: The nntplib.NNTP constructor no longer leaves the connection and socket open until the garbage collector cleans them up. Patch by Martin Panter.
- bpo-23704: collections.deque() objects now support methods for index(), insert(), and copy(). This allows deques to be registered as a MutableSequence and it improves their substitutability for lists.
- bpo-23715: `signal.sigwaitinfo()` and`signal.sigtimedwait()` are
now retried when interrupted by a signal not in the*sigset* parameter, if
the signal handler does not raise an exception. signal.sigtimedwait()
recomputes the timeout with a monotonic clock when it is retried.
- bpo-23001: Few functions in modules mmap, ossaudiodev, socket, ssl, and codecs, that accepted only read-only bytes-like object now accept writable bytes-like object too.
- bpo-23646: If time.sleep() is interrupted by a signal, the sleep is now retried with the recomputed delay, except if the signal handler raises an exception (PEP 475).
- bpo-23136: _strptime now uniformly handles all days in week 0, including Dec 30 of previous year. Based on patch by Jim Carroll.
- bpo-23700: Iterator of NamedTemporaryFile now keeps a reference to NamedTemporaryFile instance. Patch by Bohuslav Kabrda.
- bpo-22903: The fake test case created by unittest.loader when it fails importing a test module is now picklable.
- bpo-22181: On Linux, os.urandom() now uses the new getrandom() syscall if available, syscall introduced in the Linux kernel 3.17. It is more reliable and more secure, because it avoids the need of a file descriptor and waits until the kernel has enough entropy.
- bpo-2211: Updated the implementation of the http.cookies.Morsel class. Setting attributes key, value and coded_value directly now is deprecated. update() and setdefault() now transform and check keys. Comparing for equality now takes into account attributes key, value and coded_value. copy() now returns a Morsel, not a dict. repr() now contains all attributes. Optimized checking keys and quoting values. Added new tests. Original patch by Demian Brecht.
- bpo-18983: Allow selection of output units in timeit. Patch by Julian Gindi.
- bpo-23631: Fix traceback.format_list when a traceback has been mutated.
- bpo-23568: Add rdivmod support to MagicMock() objects. Patch by HÃ¥kan LÃ¶vdahl.
- bpo-2052: Add charset parameter to HtmlDiff.make_file().
- bpo-23668: Support os.truncate and os.ftruncate on Windows.
- bpo-23138: Fixed parsing cookies with absent keys or values in cookiejar. Patch by Demian Brecht.
- bpo-23051: multiprocessing.Pool methods imap() and imap_unordered() now handle exceptions raised by an iterator. Patch by Alon Diamant and Davin Potts.
- bpo-23581: Add matmul support to MagicMock. Patch by HÃ¥kan LÃ¶vdahl.
- bpo-23566: enable(), register(), dump_traceback() and dump_traceback_later() functions of faulthandler now accept file descriptors. Patch by Wei Wu.
- bpo-22928: Disabled HTTP header injections in http.client. Original patch by Demian Brecht.
- bpo-23615: Modules bz2, tarfile and tokenize now can be reloaded with imp.reload(). Patch by Thomas Kluyver.
- bpo-23605: os.walk() now calls os.scandir() instead of os.listdir(). The usage of os.scandir() reduces the number of calls to os.stat(). Initial patch written by Ben Hoyt.

### BuildÂ¶

- bpo-23585: make patchcheck will ensure the interpreter is built.

### TestsÂ¶

### Tools/DemosÂ¶

- bpo-22826: The result of open() in Tools/freeze/bkfile.py is now better compatible with regular files (in particular it now supports the context management protocol).

## Python 3.5.0 alpha 2Â¶

*Release date: 2015-03-09*

### Core and BuiltinsÂ¶

- bpo-23571: PyObject_Call() and PyCFunction_Call() now raise a SystemError if a function returns a result and raises an exception. The SystemError is chained to the previous exception.

### LibraryÂ¶

- bpo-22524: New os.scandir() function, part of the **PEP 471** :
âos.scandir() function â a better and faster directory iteratorâ. Patch
written by Ben Hoyt.
- bpo-23103: Reduced the memory consumption of IPv4Address and IPv6Address.
- bpo-21793: BaseHTTPRequestHandler again logs response code as numeric, not as stringified enum. Patch by Demian Brecht.
- bpo-23476: In the ssl module, enable OpenSSLâs X509_V_FLAG_TRUSTED_FIRST flag on certificate stores when it is available.
- bpo-23576: Avoid stalling in SSL reads when EOF has been reached in the SSL layer but the underlying connection hasnât been closed.
- bpo-23504: Added an __all__ to the types module.
- bpo-23563: Optimized utility functions in urllib.parse.
- bpo-7830: Flatten nested functools.partial.
- bpo-20204: Added the __module__ attribute to _tkinter classes.
- bpo-19980: Improved help() for non-recognized strings. help(ââ) now shows the help on str. help(âhelpâ) now shows the help on help(). Original patch by Mark Lawrence.
- bpo-23521: Corrected pure python implementation of timedelta division. Eliminated OverflowError from `timedelta * float` for some floats;
Corrected rounding in timedelta true division.
- bpo-21619: Popen objects no longer leave a zombie after exit in the with statement if the pipe was broken. Patch by Martin Panter.
- bpo-22936: Make it possible to show local variables in tracebacks for both the traceback module and unittest.
- bpo-15955: Add an option to limit the output size in bz2.decompress(). Patch by Nikolaus Rath.
- bpo-6639: Module-level turtle functions no longer raise TclError after closing the window.
- bpo-814253: Group references and conditional group references now work in lookbehind assertions in regular expressions. (See also: bpo-9179)
- bpo-23215: Multibyte codecs with custom error handlers that ignores errors consumed too much memory and raised SystemError or MemoryError. Original patch by Aleksi Torhamo.
- bpo-5700: io.FileIO() called flush() after closing the file. flush() was not called in close() if closefd=False.
- bpo-23374: Fixed pydoc failure with non-ASCII files when stdout encoding differs from file system encoding (e.g. on Mac OS).
- bpo-23481: Remove RC4 from the SSL moduleâs default cipher list.
- bpo-21548: Fix pydoc.synopsis() and pydoc.apropos() on modules with empty docstrings.
- bpo-22885: Fixed arbitrary code execution vulnerability in the dbm.dumb module. Original patch by Claudiu Popa.
- bpo-23239: ssl.match_hostname() now supports matching of IP addresses.
- bpo-23146: Fix mishandling of absolute Windows paths with forward slashes in pathlib.
- bpo-23096: Pickle representation of floats with protocol 0 now is the same for both Python and C implementations.
- bpo-19105: pprint now more efficiently uses free space at the right.
- bpo-14910: Add allow_abbrev parameter to argparse.ArgumentParser. Patch by Jonathan Paugh, Steven Bethard, paul j3 and Daniel Eriksson.
- bpo-21717: tarfile.open() now supports âxâ (exclusive creation) mode.
- bpo-23344: marshal.dumps() is now 20-25% faster on average.
- bpo-20416: marshal.dumps() with protocols 3 and 4 is now 40-50% faster on average.
- bpo-23421: Fixed compression in tarfile CLI. Patch by wdv4758h.
- bpo-23367: Fix possible overflows in the unicodedata module.
- bpo-23361: Fix possible overflow in Windows subprocess creation code.
- logging.handlers.QueueListener now takes a respect_handler_level keyword argument which, if set to True, will pass messages to handlers taking handler levels into account.
- bpo-19705: turtledemo now has a visual sorting algorithm demo. Original patch from Jason Yeo.
- bpo-23801: Fix issue where cgi.FieldStorage did not always ignore the entire preamble to a multipart body.

### BuildÂ¶

### C APIÂ¶

- bpo-20204: Deprecation warning is now raised for builtin types without the __module__ attribute.

### WindowsÂ¶

## Python 3.5.0 alpha 1Â¶

*Release date: 2015-02-08*

### Core and BuiltinsÂ¶

- bpo-23285: PEP 475 - EINTR handling.
- bpo-22735: Fix many edge cases (including crashes) involving custom mro() implementations.
- bpo-22896: Avoid using PyObject_AsCharBuffer(), PyObject_AsReadBuffer() and PyObject_AsWriteBuffer().
- bpo-21295: Revert some changes (bpo-16795) to AST line numbers and column offsets that constituted a regression.
- bpo-22986: Allow changing an objectâs __class__ between a dynamic type and static type in some cases.
- bpo-15859: PyUnicode_EncodeFSDefault(), PyUnicode_EncodeMBCS() and PyUnicode_EncodeCodePage() now raise an exception if the object is not a Unicode object. For PyUnicode_EncodeFSDefault(), it was already the case on platforms other than Windows. Patch written by Campbell Barton.
- bpo-21408: The default __ne__() now returns NotImplemented if __eq__() returned NotImplemented. Original patch by Martin Panter.
- bpo-23321: Fixed a crash in str.decode() when error handler returned replacement string longer than malformed input data.
- bpo-22286: The âbackslashreplaceâ error handlers now works with decoding and translating.
- bpo-23253: Delay-load ShellExecute[AW] in os.startfile for reduced startup overhead on Windows.
- bpo-22038: pyatomic.h now uses stdatomic.h or GCC built-in functions for atomic memory access if available. Patch written by Vitor de Lima and Gustavo Temple.
- bpo-20284: %-interpolation (aka printf) formatting added for bytes and bytearray.
- bpo-23048: Fix jumping out of an infinite while loop in the pdb.
- bpo-20335: bytes constructor now raises TypeError when encoding or errors is specified with non-string argument. Based on patch by Renaud Blanch.
- bpo-22834: If the current working directory ends up being set to a non-existent directory then import will no longer raise FileNotFoundError.
- bpo-22869: Move the interpreter startup & shutdown code to a new dedicated pylifecycle.c module
- bpo-22847: Improve method cache efficiency.
- bpo-22335: Fix crash when trying to enlarge a bytearray to 0x7fffffff bytes on a 32-bit platform.
- bpo-22653: Fix an assertion failure in debug mode when doing a reentrant dict insertion in debug mode.
- bpo-22643: Fix integer overflow in Unicode case operations (upper, lower, title, swapcase, casefold).
- bpo-17636: Circular imports involving relative imports are now supported.
- bpo-22604: Fix assertion error in debug mode when dividing a complex number by (nan+0j).
- bpo-21052: Do not raise ImportWarning when sys.path_hooks or sys.meta_path are set to None.
- bpo-16518: Use âbytes-like object requiredâ in error messages that previously used the far more cryptic ââxâ does not support the buffer protocol.
- bpo-22470: Fixed integer overflow issues in âbackslashreplaceâ, âxmlcharrefreplaceâ, and âsurrogatepassâ error handlers.
- bpo-22540: speed up `PyObject_IsInstance` and`PyObject_IsSubclass` in
the common case that the second argument has metaclass`type` .
- bpo-18711: Add a new `PyErr_FormatV` function, similar to`PyErr_Format` but accepting a`va_list` argument.
- bpo-22520: Fix overflow checking when generating the repr of a unicode object.
- bpo-22519: Fix overflow checking in PyBytes_Repr.
- bpo-22518: Fix integer overflow issues in latin-1 encoding.
- bpo-16324: _charset parameter of MIMEText now also accepts email.charset.Charset instances. Initial patch by Claude Paroz.
- bpo-1764286: Fix inspect.getsource() to support decorated functions. Patch by Claudiu Popa.
- bpo-18554: os.__all__ includes posix functions.
- bpo-21391: Use os.path.abspath in the shutil module.
- bpo-11471: avoid generating a JUMP_FORWARD instruction at the end of an if-block if there is no else-clause. Original patch by Eugene Toder.
- bpo-22215: Now ValueError is raised instead of TypeError when str or bytes argument contains not permitted null character or byte.
- bpo-22258: Fix the internal function set_inheritable() on Illumos. This platform exposes the function `ioctl(FIOCLEX)` , but calling it fails
with errno is ENOTTY: âInappropriate ioctl for deviceâ. set_inheritable()
now falls back to the slower`fcntl()` (`F_GETFD` and then`F_SETFD` ).
- bpo-21389: Displaying the __qualname__ of the underlying function in the repr of a bound method.
- bpo-22206: Using pthread, PyThread_create_key() now sets errno to ENOMEM and returns -1 (error) on integer overflow.
- bpo-20184: Argument Clinic based signature introspection added for 30 of the builtin functions.
- bpo-22116: C functions and methods (of the âbuiltin_function_or_methodâ type) can now be weakrefâed. Patch by Wei Wu.
- bpo-22077: Improve index error messages for bytearrays, bytes, lists, and tuples by adding âor slicesâ. Added â, not <typename>â for bytearrays. Original patch by Claudiu Popa.
- bpo-20179: Apply Argument Clinic to bytes and bytearray. Patch by Tal Einat.
- bpo-22082: Clear interned strings in slotdefs.
- Upgrade Unicode database to Unicode 7.0.0.
- bpo-21897: Fix a crash with the f_locals attribute with closure variables when frame.clear() has been called.
- bpo-21205: Add a new `__qualname__` attribute to generator, the
qualified name, and use it in the representation of a generator
(`repr(gen)` ). The default name of the generator (`__name__` attribute) is now get from the function instead of the code. Use`gen.gi_code.co_name` to get the name of the code.
- bpo-21669: With the aid of heuristics in SyntaxError.__init__, the parser now attempts to generate more meaningful (or at least more search engine friendly) error messages when âexecâ and âprintâ are used as statements.
- bpo-21642: In the conditional if-else expression, allow an integer written with no space between itself and the `else` keyword (e.g.```
True if
42else False
```
) to be valid syntax.
- bpo-21523: Fix over-pessimistic computation of the stack effect of some opcodes in the compiler. This also fixes a quadratic compilation time issue noticeable when compiling code with a large number of âandâ and âorâ operators.
- bpo-21418: Fix a crash in the builtin function super() when called without argument and without current frame (ex: embedded Python).
- bpo-21425: Fix flushing of standard streams in the interactive interpreter.
- bpo-21435: In rare cases, when running finalizers on objects in cyclic trash a bad pointer dereference could occur due to a subtle flaw in internal iteration logic.
- bpo-21377: PyBytes_Concat() now tries to concatenate in-place when the first argument has a reference count of 1. Patch by Nikolaus Rath.
- bpo-20355: -W command line options now have higher priority than the PYTHONWARNINGS environment variable. Patch by Arfrever.
- bpo-21274: Define PATH_MAX for GNU/Hurd in Python/pythonrun.c.
- bpo-20904: Support setting FPU precision on m68k.
- bpo-21209: Fix sending tuples to custom generator objects with the yield from syntax.
- bpo-21193: pow(a, b, c) now raises ValueError rather than TypeError when b is negative. Patch by Josh Rosenberg.
- bpo-21176: PEP 465: Add the â@â operator for matrix multiplication.
- bpo-21134: Fix segfault when str is called on an uninitialized UnicodeEncodeError, UnicodeDecodeError, or UnicodeTranslateError object.
- bpo-19537: Fix PyUnicode_DATA() alignment under m68k. Patch by Andreas Schwab.
- bpo-20929: Add a type cast to avoid shifting a negative number.
- bpo-20731: Properly position in source code files even if they are opened in text mode. Patch by Serhiy Storchaka.
- bpo-20637: Key-sharing now also works for instance dictionaries of subclasses. Patch by Peter Ingebretson.
- bpo-8297: Attributes missing from modules now include the module name in the error text. Original patch by ysj.ray.
- bpo-19995: %c, %o, %x, and %X now raise TypeError on non-integer input.
- bpo-19655: The ASDL parser - used by the build process to generate code for managing the Python AST in C - was rewritten. The new parser is self contained and does not require to carry long the spark.py parser-generator library; spark.py was removed from the source base.
- bpo-12546: Allow `\x00` to be used as a fill character when using str,
int, float, and complex __format__ methods.
- bpo-20480: Add ipaddress.reverse_pointer. Patch by Leon Weber.
- bpo-13598: Modify string.Formatter to support auto-numbering of replacement fields. It now matches the behavior of str.format() in this regard. Patches by Phil Elson and Ramchandra Apte.
- bpo-8931: Make alternate formatting (â#â) for type âcâ raise an exception. In versions prior to 3.5, â#â with âcâ had no effect. Now specifying it is an error. Patch by Torsten Landschoff.
- bpo-23165: Perform overflow checks before allocating memory in the _Py_char2wchar function.

### LibraryÂ¶

- bpo-23399: pyvenv creates relative symlinks where possible.
- bpo-20289: cgi.FieldStorage() now supports the context management protocol.
- bpo-13128: Print response headers for CONNECT requests when debuglevel > 0. Patch by Demian Brecht.
- bpo-15381: Optimized io.BytesIO to make less allocations and copyings.
- bpo-22818: Splitting on a pattern that could match an empty string now raises a warning. Patterns that can only match empty strings are now rejected.
- bpo-23099: Closing io.BytesIO with exported buffer is rejected now to prevent corrupting exported buffer.
- bpo-23326: Removed __ne__ implementations. Since fixing default __ne__ implementation in bpo-21408 they are redundant.
- bpo-23363: Fix possible overflow in itertools.permutations.
- bpo-23364: Fix possible overflow in itertools.product.
- bpo-23366: Fixed possible integer overflow in itertools.combinations.
- bpo-23369: Fixed possible integer overflow in _json.encode_basestring_ascii.
- bpo-23353: Fix the exception handling of generators in PyEval_EvalFrameEx(). At entry, save or swap the exception state even if PyEval_EvalFrameEx() is called with throwflag=0. At exit, the exception state is now always restored or swapped, not only if why is WHY_YIELD or WHY_RETURN. Patch co-written with Antoine Pitrou.
- bpo-14099: Restored support of writing ZIP files to tellable but non-seekable streams.
- bpo-14099: Writing to ZipFile and reading multiple ZipExtFiles is threadsafe now.
- bpo-19361: JSON decoder now raises JSONDecodeError instead of ValueError.
- bpo-18518: timeit now rejects statements which canât be compiled outside a function or a loop (e.g. âreturnâ or âbreakâ).
- bpo-23094: Fixed readline with frames in Python implementation of pickle.
- bpo-23268: Fixed bugs in the comparison of ipaddress classes.
- bpo-21408: Removed incorrect implementations of __ne__() which didnât returned NotImplemented if __eq__() returned NotImplemented. The default __ne__() now works correctly.
- bpo-19996: `email.feedparser.FeedParser` now handles (malformed)
headers with no key rather than assuming the body has started.
- bpo-20188: Support Application-Layer Protocol Negotiation (ALPN) in the ssl module.
- bpo-23133: Pickling of ipaddress objects now produces more compact and portable representation.
- bpo-23248: Update ssl error codes from latest OpenSSL git master.
- bpo-23266: Much faster implementation of ipaddress.collapse_addresses() when there are many non-consecutive addresses.
- bpo-23098: 64-bit dev_t is now supported in the os module.
- bpo-21817: When an exception is raised in a task submitted to a ProcessPoolExecutor, the remote traceback is now displayed in the parent process. Patch by Claudiu Popa.
- bpo-15955: Add an option to limit output size when decompressing LZMA data. Patch by Nikolaus Rath and Martin Panter.
- bpo-23250: In the http.cookies module, capitalize âHttpOnlyâ and âSecureâ as they are written in the standard.
- bpo-23063: In the distutilsâ check command, fix parsing of reST with code or code-block directives.
- bpo-23209: selectors.BaseSelector.get_key() now raises a RuntimeError if the selector is closed. And selectors.BaseSelector.close() now clears its internal reference to the selector mapping to break a reference cycle. Initial patch written by Martin Richard. (See also: bpo-23225)
- bpo-17911: Provide a way to seed the linecache for a PEP-302 module without actually loading the code.
- bpo-17911: Provide a new object API for traceback, including the ability to not lookup lines at all until the traceback is actually rendered, without any trace of the original objects being kept alive.
- bpo-19777: Provide a home() classmethod on Path objects. Contributed by Victor Salgado and Mayank Tripathi.
- bpo-23206: Make `json.dumps(..., ensure_ascii=False)` as fast as the
default case of`ensure_ascii=True` .  Patch by Naoki Inada.
- bpo-23185: Add math.inf and math.nan constants.
- bpo-23186: Add ssl.SSLObject.shared_ciphers() and ssl.SSLSocket.shared_ciphers() to fetch the clientâs list ciphers sent at handshake.
- bpo-23143: Remove compatibility with OpenSSLs older than 0.9.8.
- bpo-23132: Improve performance and introspection support of comparison methods created by functool.total_ordering.
- bpo-19776: Add an expanduser() method on Path objects.
- bpo-23112: Fix SimpleHTTPServer to correctly carry the query string and fragment when it redirects to add a trailing slash.
- bpo-21793: Added http.HTTPStatus enums (i.e. HTTPStatus.OK, HTTPStatus.NOT_FOUND). Patch by Demian Brecht.
- bpo-23093: In the io, module allow more operations to work on detached streams.
- bpo-23111: In the ftplib, make ssl.PROTOCOL_SSLv23 the default protocol version.
- bpo-22585: On OpenBSD 5.6 and newer, os.urandom() now calls getentropy(), instead of reading /dev/urandom, to get pseudo-random bytes.
- bpo-19104: pprint now produces evaluable output for wrapped strings.
- bpo-23071: Added missing names to codecs.__all__. Patch by Martin Panter.
- bpo-22783: Pickling now uses the NEWOBJ opcode instead of the NEWOBJ_EX opcode if possible.
- bpo-15513: Added a __sizeof__ implementation for pickle classes.
- bpo-19858: pickletools.optimize() now aware of the MEMOIZE opcode, can produce more compact result and no longer produces invalid output if input data contains MEMOIZE opcodes together with PUT or BINPUT opcodes.
- bpo-22095: Fixed HTTPConnection.set_tunnel with default port. The port value in the host header was set to âNoneâ. Patch by Demian Brecht.
- bpo-23016: A warning no longer produces an AttributeError when the program is run with pythonw.exe.
- bpo-21775: shutil.copytree(): fix crash when copying to VFAT. An exception handler assumed that OSError objects always have a âwinerrorâ attribute. That is not the case, so the exception handler itself raised AttributeError when run on Linux (and, presumably, any other non-Windows OS). Patch by Greg Ward.
- bpo-1218234: Fix inspect.getsource() to load updated source of reloaded module. Initial patch by Berker Peksag.
- bpo-21740: Support wrapped callables in doctest. Patch by Claudiu Popa.
- bpo-23009: Make sure selectors.EpollSelector.select() works when no FD is registered.
- bpo-22959: In the constructor of http.client.HTTPSConnection, prefer the contextâs check_hostname attribute over the *check_hostname* parameter.
- bpo-22696: Add function `sys.is_finalizing()` to know about
interpreter shutdown.
- bpo-16043: Add a default limit for the amount of data xmlrpclib.gzip_decode will return. This resolves **CVE 2013-1753** .
- bpo-14099: ZipFile.open() no longer reopen the underlying file. Objects returned by ZipFile.open() can now operate independently of the ZipFile even if the ZipFile was created by passing in a file-like object as the first argument to the constructor.
- bpo-22966: Fix __pycache__ pyc file name clobber when pyc_compile is asked to compile a source file containing multiple dots in the source file name.
- bpo-21971: Update turtledemo doc and add module to the index.
- bpo-21032: Fixed socket leak if HTTPConnection.getresponse() fails. Original patch by Martin Panter.
- bpo-22407: Deprecated the use of re.LOCALE flag with str patterns or re.ASCII. It was newer worked.
- bpo-22902: The âipâ command is now used on Linux to determine MAC address in uuid.getnode(). Patch by Bruno Cauet.
- bpo-22960: Add a context argument to xmlrpclib.ServerProxy constructor.
- bpo-22389: Add contextlib.redirect_stderr().
- bpo-21356: Make ssl.RAND_egd() optional to support LibreSSL. The availability of the function is checked during the compilation. Patch written by Bernard Spil.
- bpo-22915: SAX parser now supports files opened with file descriptor or bytes path.
- bpo-22609: Constructors and update methods of mapping classes in the collections module now accept the self keyword argument.
- bpo-22940: Add readline.append_history_file.
- bpo-19676: Added the ânamereplaceâ error handler.
- bpo-22788: Add *context* parameter to logging.handlers.HTTPHandler.
- bpo-22921: Allow SSLContext to take the *hostname* parameter even if
OpenSSL doesnât support SNI.
- bpo-22894: TestCase.subTest() would cause the test suite to be stopped when in failfast mode, even in the absence of failures.
- bpo-22796: HTTP cookie parsing is now stricter, in order to protect against potential injection attacks.
- bpo-22370: Windows detection in pathlib is now more robust.
- bpo-22841: Reject coroutines in asyncio add_signal_handler(). Patch by Ludovic.Gasc.
- bpo-19494: Added urllib.request.HTTPBasicPriorAuthHandler. Patch by Matej Cepl.
- bpo-22578: Added attributes to the re.error class.
- bpo-22849: Fix possible double free in the io.TextIOWrapper constructor.
- bpo-12728: Different Unicode characters having the same uppercase but different lowercase are now matched in case-insensitive regular expressions.
- bpo-22821: Fixed fcntl() with integer argument on 64-bit big-endian platforms.
- bpo-21650: Add an `--sort-keys` option to`json.tool` CLI.
- bpo-22824: Updated reprlib output format for sets to use set literals. Patch contributed by Berker Peksag.
- bpo-22824: Updated reprlib output format for arrays to display empty arrays without an unnecessary empty list. Suggested by Serhiy Storchaka.
- bpo-22406: Fixed the uu_codec codec incorrectly ported to 3.x. Based on patch by Martin Panter.
- bpo-17293: uuid.getnode() now determines MAC address on AIX using netstat. Based on patch by Aivars KalvÄns.
- bpo-22769: Fixed ttk.Treeview.tag_has() when called without arguments.
- bpo-22417: Verify certificates by default in httplib (PEP 476).
- bpo-22775: Fixed unpickling of http.cookies.SimpleCookie with protocol 2 and above. Patch by Tim Graham.
- bpo-22776: Brought excluded code into the scope of a try block in SysLogHandler.emit().
- bpo-22665: Add missing get_terminal_size and SameFileError to shutil.__all__.
- bpo-6623: Remove deprecated Netrc class in the ftplib module. Patch by Matt Chaput.
- bpo-17381: Fixed handling of case-insensitive ranges in regular expressions.
- bpo-22410: Module level functions in the re module now cache compiled locale-dependent regular expressions taking into account the locale.
- bpo-22759: Query methods on pathlib.Path() (exists(), is_dir(), etc.) now return False when the underlying stat call raises NotADirectoryError.
- bpo-8876: distutils now falls back to copying files when hard linking doesnât work. This allows use with special filesystems such as VirtualBox shared folders.
- bpo-22217: Implemented reprs of classes in the zipfile module.
- bpo-22457: Honour load_tests in the start_dir of discovery.
- bpo-18216: gettext now raises an error when a .mo file has an unsupported major version number. Patch by Aaron Hill.
- bpo-13918: Provide a locale.delocalize() function which can remove locale-specific number formatting from a string representing a number, without then converting it to a specific type. Patch by CÃ©dric Krier.
- bpo-22676: Make the pickling of global objects which donât have a __module__ attribute less slow.
- bpo-18853: Fixed ResourceWarning in shlex.__nain__.
- bpo-9351: Defaults set with set_defaults on an argparse subparser are no longer ignored when also set on the parent parser.
- bpo-7559: unittest test loading ImportErrors are reported as import errors with their import exception rather than as attribute errors after the import has already failed.
- bpo-19746: Make it possible to examine the errors from unittest discovery without executing the test suite. The new `errors` attribute on`TestLoader` exposes these non-fatal errors encountered during
discovery.
- bpo-21991: Make email.headerregistryâs header âparamsâ attributes be read-only (MappingProxyType). Previously the dictionary was modifiable but a new one was created on each access of the attribute.
- bpo-22638: SSLv3 is now disabled throughout the standard library. It can still be enabled by instantiating a SSLContext manually.
- bpo-22641: In asyncio, the default SSL context for client connections is now created using ssl.create_default_context(), for stronger security.
- bpo-17401: Include closefd in io.FileIO repr.
- bpo-21338: Add silent mode for compileall. quiet parameters of compile_{dir, file, path} functions now have a multilevel value. Also, -q option of the CLI now have a multilevel value. Patch by Thomas Kluyver.
- bpo-20152: Convert the array and cmath modules to Argument Clinic.
- bpo-18643: Add socket.socketpair() on Windows.
- bpo-22435: Fix a file descriptor leak when socketserver bind fails.
- bpo-13096: Fixed segfault in CTypes POINTER handling of large values.
- bpo-11694: Raise ConversionError in xdrlib as documented. Patch by Filip GruszczyÅski and Claudiu Popa.
- bpo-19380: Optimized parsing of regular expressions.
- bpo-1519638: Now unmatched groups are replaced with empty strings in re.sub() and re.subn().
- bpo-18615: sndhdr.what/whathdr now return a namedtuple.
- bpo-22462: Fix pyexpatâs creation of a dummy frame to make it appear in exception tracebacks.
- bpo-21965: Add support for in-memory SSL to the ssl module. Patch by Geert Jansen.
- bpo-21173: Fix len() on a WeakKeyDictionary when .clear() was called with an iterator alive.
- bpo-11866: Eliminated race condition in the computation of names for new threads.
- bpo-21905: Avoid RuntimeError in pickle.whichmodule() when sys.modules is mutated while iterating. Patch by Olivier Grisel.
- bpo-11271: concurrent.futures.Executor.map() now takes a *chunksize* argument to allow batching of tasks in child processes and improve
performance of ProcessPoolExecutor.  Patch by Dan OâReilly.
- bpo-21883: os.path.join() and os.path.relpath() now raise a TypeError with more helpful error message for unsupported or mismatched types of arguments.
- bpo-22219: The zipfile module CLI now adds entries for directories (including empty directories) in ZIP file.
- bpo-22449: In the ssl.SSLContext.load_default_certs, consult the environmental variables SSL_CERT_DIR and SSL_CERT_FILE on Windows.
- bpo-22508: The email.__version__ variable has been removed; the email code is no longer shipped separately from the stdlib, and __version__ hasnât been updated in several releases.
- bpo-20076: Added non derived UTF-8 aliases to locale aliases table.
- bpo-20079: Added locales supported in glibc 2.18 to locale alias table.
- bpo-20218: Added convenience methods read_text/write_text and read_bytes/ write_bytes to pathlib.Path objects.
- bpo-22396: On 32-bit AIX platform, donât expose os.posix_fadvise() nor os.posix_fallocate() because their prototypes in system headers are wrong.
- bpo-22517: When an io.BufferedRWPair object is deallocated, clear its weakrefs.
- bpo-22437: Number of capturing groups in regular expression is no longer limited by 100.
- bpo-17442: InteractiveInterpreter now displays the full chained traceback in its showtraceback method, to match the built in interactive interpreter.
- bpo-23392: Added tests for marshal C API that works with FILE*.
- bpo-10510: distutils register and upload methods now use HTML standards compliant CRLF line endings.
- bpo-9850: Fixed macpath.join() for empty first component. Patch by Oleg Oshmyan.
- bpo-5309: distutilsâ build and build_ext commands now accept a `-j` option to enable parallel building of extension modules.
- bpo-22448: Improve canceled timer handles cleanup to prevent unbound memory usage. Patch by Joshua Moore-Oliva.
- bpo-22427: TemporaryDirectory no longer attempts to clean up twice when used in the with statement in generator.
- bpo-22362: Forbidden ambiguous octal escapes out of range 0-0o377 in regular expressions.
- bpo-20912: Now directories added to ZIP file have correct Unix and MS-DOS directory attributes.
- bpo-21866: ZipFile.close() no longer writes ZIP64 central directory records if allowZip64 is false.
- bpo-22278: Fix urljoin problem with relative urls, a regression observed after changes to issue22118 were submitted.
- bpo-22415: Fixed debugging output of the GROUPREF_EXISTS opcode in the re module. Removed trailing spaces in debugging output.
- bpo-22423: Unhandled exception in thread no longer causes unhandled AttributeError when sys.stderr is None.
- bpo-21332: Ensure that `bufsize=1` in subprocess.Popen() selects line
buffering, rather than block buffering.  Patch by Akira Li.
- bpo-21091: Fix API bug: email.message.EmailMessage.is_attachment is now a method.
- bpo-21079: Fix email.message.EmailMessage.is_attachment to return the correct result when the header has parameters as well as a value.
- bpo-22247: Add NNTPError to nntplib.__all__.
- bpo-22366: urllib.request.urlopen will accept a context object (SSLContext) as an argument which will then be used for HTTPS connection. Patch by Alex Gaynor.
- bpo-4180: The warnings registries are now reset when the filters are modified.
- bpo-22419: Limit the length of incoming HTTP request in wsgiref server to 65536 bytes and send a 414 error code for higher lengths. Patch contributed by Devin Cook.
- Lax cookie parsing in http.cookies could be a security issue when combined with non-standard cookie handling in some web browsers. Reported by Sergey Bobrov.
- bpo-20537: logging methods now accept an exception instance as well as a Boolean value or exception tuple. Thanks to Yury Selivanov for the patch.
- bpo-22384: An exception in Tkinter callback no longer crashes the program when it is run with pythonw.exe.
- bpo-22168: Prevent turtle AttributeError with non-default Canvas on OS X.
- bpo-21147: sqlite3 now raises an exception if the request contains a null character instead of truncating it. Based on patch by Victor Stinner.
- bpo-13968: The glob module now supports recursive search in subdirectories using the `**` pattern.
- bpo-21951: Fixed a crash in Tkinter on AIX when called Tcl command with empty string or tuple argument.
- bpo-21951: Tkinter now most likely raises MemoryError instead of crash if the memory allocation fails.
- bpo-22338: Fix a crash in the json module on memory allocation failure.
- bpo-12410: imaplib.IMAP4 now supports the context management protocol. Original patch by Tarek ZiadÃ©.
- bpo-21270: We now override tuple methods in mock.call objects so that they can be used as normal call attributes.
- bpo-16662: `load_tests()` is now unconditionally run when it is present
in a packageâs`__init__.py` .`TestLoader.loadTestsFromModule()` still accepts use_load_tests, but it is deprecated and ignored.  A new
keyword-only attribute`pattern` is added and documented.  Patch given
by Robert Collins, tweaked by Barry Warsaw.
- bpo-22226: First letter no longer is stripped from the âstatusâ key in the result of Treeview.heading().
- bpo-19524: Fixed resource leak in the HTTP connection when an invalid response is received. Patch by Martin Panter.
- bpo-20421: Add a .version() method to SSL sockets exposing the actual protocol version in use.
- bpo-19546: configparser exceptions no longer expose implementation details. Chained KeyErrors are removed, which leads to cleaner tracebacks. Patch by Claudiu Popa.
- bpo-22051: turtledemo no longer reloads examples to re-run them. Initialization of variables and gui setup should be done in main(), which is called each time a demo is run, but not on import.
- bpo-21933: Turtledemo users can change the code font size with a menu selection or control(command) â-â or â+â or control-mousewheel. Original patch by Lita Cho.
- bpo-21597: The separator between the turtledemo text pane and the drawing canvas can now be grabbed and dragged with a mouse. The code text pane can be widened to easily view or copy the full width of the text. The canvas can be widened on small screens. Original patches by Jan Kanis and Lita Cho.
- bpo-18132: Turtledemo buttons no longer disappear when the window is shrunk. Original patches by Jan Kanis and Lita Cho.
- bpo-22043: time.monotonic() is now always available. `threading.Lock.acquire()` ,`threading.RLock.acquire()` and socket
operations now use a monotonic clock, instead of the system clock, when a
timeout is used.
- bpo-21527: Add a default number of workers to ThreadPoolExecutor equal to 5 times the number of CPUs. Patch by Claudiu Popa.
- bpo-22216: smtplib now resets its state more completely after a quit. The most obvious consequence of the previous behavior was a STARTTLS failure during a connect/starttls/quit/connect/starttls sequence.
- bpo-22098: ctypesâ BigEndianStructure and LittleEndianStructure now define an empty __slots__ so that subclasses donât always get an instance dict. Patch by Claudiu Popa.
- bpo-22185: Fix an occasional RuntimeError in threading.Condition.wait() caused by mutation of the waiters queue without holding the lock. Patch by Doug Zongker.
- bpo-22287: On UNIX, _PyTime_gettimeofday() now uses clock_gettime(CLOCK_REALTIME) if available. As a side effect, Python now depends on the librt library on Solaris and on Linux (only with glibc older than 2.17).
- bpo-22182: Use e.args to unpack exceptions correctly in distutils.file_util.move_file. Patch by Claudiu Popa.
- The webbrowser module now uses subprocessâs start_new_session=True rather than a potentially risky preexec_fn=os.setsid call.
- bpo-22042: signal.set_wakeup_fd(fd) now raises an exception if the file descriptor is in blocking mode.
- bpo-16808: inspect.stack() now returns a named tuple instead of a tuple. Patch by Daniel Shahaf.
- bpo-22236: Fixed Tkinter images copying operations in NoDefaultRoot mode.
- bpo-2527: Add a *globals* argument to timeit functions, in order to
override the globals namespace in which the timed code is executed. Patch
by Ben Roberts.
- bpo-22118: Switch urllib.parse to use RFC 3986 semantics for the resolution of relative URLs, rather than RFCs 1808 and 2396. Patch by Demian Brecht.
- bpo-21549: Added the âmembersâ parameter to TarFile.list().
- bpo-19628: Allow compileall recursion depth to be specified with a -r option.
- bpo-15696: Add a __sizeof__ implementation for mmap objects on Windows.
- bpo-22068: Avoided reference loops with Variables and Fonts in Tkinter.
- bpo-22165: SimpleHTTPRequestHandler now supports undecodable file names.
- bpo-15381: Optimized line reading in io.BytesIO.
- bpo-8797: Raise HTTPError on failed Basic Authentication immediately. Initial patch by Sam Bull.
- bpo-20729: Restored the use of lazy iterkeys()/itervalues()/iteritems() in the mailbox module.
- bpo-21448: Changed FeedParser feed() to avoid *O* (*n*<sup>2</sup> )
behavior when parsing long line. Original patch by Raymond Hettinger.
- bpo-22184: The functools LRU Cache decorator factory now gives an earlier and clearer error message when the user forgets the required parameters.
- bpo-17923: glob() patterns ending with a slash no longer match non-dirs on AIX. Based on patch by Delhallt.
- bpo-21725: Added support for RFC 6531 (SMTPUTF8) in smtpd.
- bpo-22176: Update the ctypes moduleâs libffi to v3.1. This release adds support for the Linux AArch64 and POWERPC ELF ABIv2 little endian architectures.
- bpo-5411: Added support for the âxztarâ format in the shutil module.
- bpo-21121: Donât force 3rd party C extensions to be built with -Werror=declaration-after-statement.
- bpo-21975: Fixed crash when using uninitialized sqlite3.Row (in particular when unpickling pickled sqlite3.Row). sqlite3.Row is now initialized in the __new__() method.
- bpo-20170: Convert posixmodule to use Argument Clinic.
- bpo-21539: Add an *exists_ok* argument to`Pathlib.mkdir()` to mimic`mkdir -p` and`os.makedirs()` functionality.  When true, ignore`FileExistsErrors` . Patch by Berker Peksag.
- bpo-22127: Bypass IDNA for pure-ASCII host names in the socket module (in particular for numeric IPs).
- bpo-21047: set the default value for the *convert_charrefs* argument of
HTMLParser to True.  Patch by Berker Peksag.
- Add an __all__ to html.entities.
- bpo-15114: the strict mode and argument of HTMLParser, HTMLParser.error, and the HTMLParserError exception have been removed.
- bpo-22085: Dropped support of Tk 8.3 in Tkinter.
- bpo-21580: Now Tkinter correctly handles bytes arguments passed to Tk. In particular this allows initializing images from binary data.
- bpo-22003: When initialized from a bytes object, io.BytesIO() now defers making a copy until it is mutated, improving performance and memory use on some use cases. Patch by David Wilson.
- bpo-22018: On Windows, signal.set_wakeup_fd() now also supports sockets. A side effect is that Python depends to the WinSock library.
- bpo-22054: Add os.get_blocking() and os.set_blocking() functions to get and set the blocking mode of a file descriptor (False if the O_NONBLOCK flag is set, True otherwise). These functions are not available on Windows.
- bpo-17172: Make turtledemo start as active on OS X even when run with subprocess. Patch by Lita Cho.
- bpo-21704: Fix build error for _multiprocessing when semaphores are not available. Patch by Arfrever Frehtes Taifersar Arahesis.
- bpo-20173: Convert sha1, sha256, sha512 and md5 to ArgumentClinic. Patch by Vajrasky Kok.
- Fix repr(_socket.socket) on Windows 64-bit: donât fail with OverflowError on closed socket. repr(socket.socket) already works fine.
- bpo-22033: Reprs of most Python implemented classes now contain actual class name instead of hardcoded one.
- bpo-21947: The dis module can now disassemble generator-iterator objects based on their gi_code attribute. Patch by Clement Rouault.
- bpo-16133: The asynchat.async_chat.handle_read() method now ignores BlockingIOError exceptions.
- bpo-22044: Fixed premature DECREF in call_tzinfo_method. Patch by Tom Flanagan.
- bpo-19884: readline: Disable the meta modifier key if stdout is not a terminal to not write the ANSI sequence `"\033[1034h"` into stdout. This
sequence is used on some terminal (ex: TERM=xterm-256colorâ) to enable
support of 8 bit characters.
- bpo-4350: Removed a number of out-of-dated and non-working for a long time Tkinter methods.
- bpo-6167: Scrollbar.activate() now returns the name of active element if the argument is not specified. Scrollbar.set() now always accepts only 2 arguments.
- bpo-15275: Clean up and speed up the ntpath module.
- bpo-21888: plistlibâs load() and loads() now work if the fmt parameter is specified.
- bpo-22032: __qualname__ instead of __name__ is now always used to format fully qualified class names of Python implemented classes.
- bpo-22031: Reprs now always use hexadecimal format with the â0xâ prefix when contain an id in form â at 0xâ¦â.
- bpo-22018: signal.set_wakeup_fd() now raises an OSError instead of a ValueError on `fstat()` failure.
- bpo-21044: tarfile.open() now handles fileobj with an integer ânameâ attribute. Based on patch by Antoine Pietri.
- bpo-21966: Respect -q command-line option when code module is ran.
- bpo-19076: Donât pass the redundant âfileâ argument to self.error().
- bpo-16382: Improve exception message of warnings.warn() for bad category. Initial patch by Phil Elson.
- bpo-21932: os.read() now uses a `Py_ssize_t()` type instead of
int for the size to support reading more than 2 GB at once. On
Windows, the size is truncated to INT_MAX. As any call to os.read(), the
OS may read less bytes than the number of requested bytes.
- bpo-21942: Fixed source file viewing in pydocâs server mode on Windows.
- bpo-11259: asynchat.async_chat().set_terminator() now raises a ValueError if the number of received bytes is negative.
- bpo-12523: asynchat.async_chat.push() now raises a TypeError if it doesnât get a bytes string
- bpo-21707: Add missing kwonlyargcount argument to ModuleFinder.replace_paths_in_code().
- bpo-20639: calling Path.with_suffix(ââ) allows removing the suffix again. Patch by July Tikhonov.
- bpo-21714: Disallow the construction of invalid paths using Path.with_name(). Original patch by Antony Lee.
- bpo-15014: Added âauthâ method to smtplib to make implementing auth mechanisms simpler, and used it internally in the login method.
- bpo-21151: Fixed a segfault in the winreg module when `None` is passed
as a`REG_BINARY` value to SetValueEx.  Patch by John Ehresman.
- bpo-21090: io.FileIO.readall() does not ignore I/O errors anymore. Before, it ignored I/O errors if at least the first C call read() succeed.
- bpo-5800: headers parameter of wsgiref.headers.Headers is now optional. Initial patch by Pablo Torres Navarrete and SilentGhost.
- bpo-21781: ssl.RAND_add() now supports strings longer than 2 GB.
- bpo-21679: Prevent extraneous fstat() calls during open(). Patch by Bohuslav Kabrda.
- bpo-21863: cProfile now displays the module name of C extension functions, in addition to their own name.
- bpo-11453: asyncore: emit a ResourceWarning when an unclosed file_wrapper object is destroyed. The destructor now closes the file if needed. The close() method can now be called twice: the second call does nothing.
- bpo-21858: Better handling of Python exceptions in the sqlite3 module.
- bpo-21476: Make sure the email.parser.BytesParser TextIOWrapper is discarded after parsing, so the input file isnât unexpectedly closed.
- bpo-20295: imghdr now recognizes OpenEXR format images.
- bpo-21729: Used the âwithâ statement in the dbm.dumb module to ensure files closing. Patch by Claudiu Popa.
- bpo-21491: socketserver: Fix a race condition in child processes reaping.
- bpo-21719: Added the `st_file_attributes` field to os.stat_result on
Windows.
- bpo-21832: Require named tuple inputs to be exact strings.
- bpo-21722: The distutils âuploadâ command now exits with a non-zero return code when uploading fails. Patch by Martin Dengler.
- bpo-21723: asyncio.Queue: support any type of number (ex: float) for the maximum size. Patch written by Vajrasky Kok.
- bpo-21711: support for âsite-pythonâ directories has now been removed from the site module (it was deprecated in 3.4).
- bpo-17552: new socket.sendfile() method allowing a file to be sent over a socket by using high-performance os.sendfile() on UNIX. Patch by Giampaolo Rodolaâ.
- bpo-18039: dbm.dump.open() now always creates a new database when the flag has the value ânâ. Patch by Claudiu Popa.
- bpo-21326: Add a new is_closed() method to asyncio.BaseEventLoop. run_forever() and run_until_complete() methods of asyncio.BaseEventLoop now raise an exception if the event loop was closed.
- bpo-21766: Prevent a security hole in CGIHTTPServer by URL unquoting paths before checking for a CGI script at that path.
- bpo-21310: Fixed possible resource leak in failed open().
- bpo-21256: Printout of keyword args should be in deterministic order in a mock function call. This will help to write better doctests.
- bpo-21677: Fixed chaining nonnormalized exceptions in io close() methods.
- bpo-11709: Fix the pydoc.help function to not fail when sys.stdin is not a valid file.
- bpo-21515: tempfile.TemporaryFile now uses os.O_TMPFILE flag is available.
- bpo-13223: Fix pydoc.writedoc so that the HTML documentation for methods that use âselfâ in the example code is generated correctly.
- bpo-21463: In urllib.request, fix pruning of the FTP cache.
- bpo-21618: The subprocess module could fail to close open fds that were inherited by the calling process and already higher than POSIX resource limits would otherwise allow. On systems with a functioning /proc/self/fd or /dev/fd interface the max is now ignored and all fds are closed.
- bpo-20383: Introduce importlib.util.module_from_spec() as the preferred way to create a new module.
- bpo-21552: Fixed possible integer overflow of too long string lengths in the tkinter module on 64-bit platforms.
- bpo-14315: The zipfile module now ignores extra fields in the central directory that are too short to be parsed instead of letting a struct.unpack error bubble up as this âbad dataâ appears in many real world zip files in the wild and is ignored by other zip tools.
- bpo-13742: Added âkeyâ and âreverseâ parameters to heapq.merge(). (First draft of patch contributed by Simon Sapin.)
- bpo-21402: tkinter.ttk now works when default root window is not set.
- bpo-3015: `_tkinter.create()` now creates`tkapp` object with`wantobjects=1` by default.
- bpo-10203: sqlite3.Row now truly supports sequence protocol. In particular it supports reverse() and negative indices. Original patch by Claudiu Popa.
- bpo-18807: If copying (no symlinks) specified for a venv, then the python interpreter aliases (python, python3) are now created by copying rather than symlinking.
- bpo-20197: Added support for the WebP image type in the imghdr module. Patch by Fabrice Aneche and Claudiu Popa.
- bpo-21513: Speedup some properties of IP addresses (IPv4Address, IPv6Address) such as .is_private or .is_multicast.
- bpo-21137: Improve the repr for threading.Lock() and its variants by showing the âlockedâ or âunlockedâ status. Patch by Berker Peksag.
- bpo-21538: The plistlib module now supports loading of binary plist files when reference or offset size is not a power of two.
- bpo-21455: Add a default backlog to socket.listen().
- bpo-21525: Most Tkinter methods which accepted tuples now accept lists too.
- bpo-22166: With the assistance of a new internal _codecs._forget_codec helping function, test_codecs now clears the encoding caches to avoid the appearance of a reference leak
- bpo-22236: Tkinter tests now donât reuse default root window. New root window is created for every test class.
- bpo-10744: Fix **PEP 3118** format strings on ctypes objects with a
nontrivial shape.
- bpo-20826: Optimize ipaddress.collapse_addresses().
- bpo-21487: Optimize ipaddress.summarize_address_range() and ipaddress.{IPv4Network,IPv6Network}.subnets().
- bpo-21486: Optimize parsing of netmasks in ipaddress.IPv4Network and ipaddress.IPv6Network.
- bpo-13916: Disallowed the surrogatepass error handler for non UTF-* encodings.
- bpo-20998: Fixed re.fullmatch() of repeated single character pattern with ignore case. Original patch by Matthew Barnett.
- bpo-21075: fileinput.FileInput now reads bytes from standard stream if binary mode is specified. Patch by Sam Kimbrel.
- bpo-19775: Add a samefile() method to pathlib Path objects. Initial patch by Vajrasky Kok.
- bpo-21226: Set up modules properly in PyImport_ExecCodeModuleObject (and friends).
- bpo-21398: Fix a unicode error in the pydoc pager when the documentation contains characters not encodable to the stdout encoding.
- bpo-16531: ipaddress.IPv4Network and ipaddress.IPv6Network now accept an (address, netmask) tuple argument, so as to easily construct network objects from existing addresses.
- bpo-21156: importlib.abc.InspectLoader.source_to_code() is now a staticmethod.
- bpo-21424: Simplified and optimized heaqp.nlargest() and nmsmallest() to make fewer tuple comparisons.
- bpo-21396: Fix TextIOWrapper(â¦, write_through=True) to not force a flush() on the underlying binary stream. Patch by akira.
- bpo-18314: Unlink now removes junctions on Windows. Patch by Kim GrÃ¤sman
- bpo-21088: Bugfix for curses.window.addch() regression in 3.4.0. In porting to Argument Clinic, the first two arguments were reversed.
- bpo-21407: _decimal: The module now supports function signatures.
- bpo-10650: Remove the non-standard âwatchexpâ parameter from the Decimal.quantize() method in the Python version. It had never been present in the C version.
- bpo-21469: Reduced the risk of false positives in robotparser by checking to make sure that robots.txt has been read or does not exist prior to returning True in can_fetch().
- bpo-19414: Have the OrderedDict mark deleted links as unusable. This gives an early failure if the link is deleted during iteration.
- bpo-21421: Add __slots__ to the MappingViews ABC. Patch by Josh Rosenberg.
- bpo-21101: Eliminate double hashing in the C speed-up code for collections.Counter().
- bpo-21321: itertools.islice() now releases the reference to the source iterator when the slice is exhausted. Patch by Anton Afanasyev.
- bpo-21057: TextIOWrapper now allows the underlying binary streamâs read() or read1() method to return an arbitrary bytes-like object (such as a memoryview). Patch by Nikolaus Rath.
- bpo-20951: SSLSocket.send() now raises either SSLWantReadError or SSLWantWriteError on a non-blocking socket if the operation would block. Previously, it would return 0. Patch by Nikolaus Rath.
- bpo-13248: removed previously deprecated asyncore.dispatcher __getattr__ cheap inheritance hack.
- bpo-9815: assertRaises now tries to clear references to local variables in the exceptionâs traceback.
- bpo-19940: ssl.cert_time_to_seconds() now interprets the given time string in the UTC timezone (as specified in RFC 5280), not the local timezone.
- bpo-13204: Calling sys.flags.__new__ would crash the interpreter, now it raises a TypeError.
- bpo-19385: Make operations on a closed dbm.dumb database always raise the same exception.
- bpo-21207: Detect when the os.urandom cached fd has been closed or replaced, and open it anew.
- bpo-21291: subprocessâs Popen.wait() is now thread safe so that multiple threads may be calling wait() or poll() on a Popen instance at the same time without losing the Popen.returncode value.
- bpo-21127: Path objects can now be instantiated from str subclass instances (such as `numpy.str_` ).
- bpo-15002: urllib.response object to use _TemporaryFileWrapper (and _TemporaryFileCloser) facility. Provides a better way to handle file descriptor close. Patch contributed by Christian Theune.
- bpo-12220: mindom now raises a custom ValueError indicating it doesnât support spaces in URIs instead of letting a âsplitâ ValueError bubble up.
- bpo-21068: The ssl.PROTOCOL* constants are now enum members.
- bpo-21276: posixmodule: Donât define USE_XATTRS on KFreeBSD and the Hurd.
- bpo-21262: New method assert_not_called for Mock. It raises AssertionError if the mock has been called.
- bpo-21238: New keyword argument `unsafe` to Mock. It raises`AttributeError` in case of an attribute startswith assert or assret.
- bpo-20896: ssl.get_server_certificate() now uses PROTOCOL_SSLv23, not PROTOCOL_SSLv3, for maximum compatibility.
- bpo-21239: patch.stopall() didnât work deterministically when the same name was patched more than once.
- bpo-21203: Updated fileConfig and dictConfig to remove inconsistencies. Thanks to Jure Koren for the patch.
- bpo-21222: Passing name keyword argument to mock.create_autospec now works.
- bpo-21197: Add lib64 -> lib symlink in venvs on 64-bit non-OS X POSIX.
- bpo-17498: Some SMTP servers disconnect after certain errors, violating strict RFC conformance. Instead of losing the error code when we issue the subsequent RSET, smtplib now returns the error code and defers raising the SMTPServerDisconnected error until the next command is issued.
- bpo-17826: setting an iterable side_effect on a mock function created by create_autospec now works. Patch by Kushal Das.
- bpo-7776: Fix `Host:` header and reconnection when using
http.client.HTTPConnection.set_tunnel(). Patch by Nikolaus Rath.
- bpo-20968: unittest.mock.MagicMock now supports division. Patch by Johannes Baiter.
- bpo-21529: Fix arbitrary memory access in JSONDecoder.raw_decode with a negative second parameter. Bug reported by Guido Vranken. (See also: **CVE 2014-4616** )
- bpo-21169: getpass now handles non-ascii characters that the input stream encoding cannot encode by re-encoding using the replace error handler.
- bpo-21171: Fixed undocumented filter API of the rot13 codec. Patch by Berker Peksag.
- bpo-20539: Improved math.factorial error message for large positive inputs and changed exception type (OverflowError -> ValueError) for large negative inputs.
- bpo-21172: isinstance check relaxed from dict to collections.Mapping.
- bpo-21155: asyncio.EventLoop.create_unix_server() now raises a ValueError if path and sock are specified at the same time.
- bpo-21136: Avoid unnecessary normalization of Fractions resulting from power and other operations. Patch by Raymond Hettinger.
- bpo-17621: Introduce importlib.util.LazyLoader.
- bpo-21076: signal module constants were turned into enums. Patch by Giampaolo Rodolaâ.
- bpo-20636: Improved the repr of Tkinter widgets.
- bpo-19505: The items, keys, and values views of OrderedDict now support reverse iteration using reversed().
- bpo-21149: Improved thread-safety in logging cleanup during interpreter shutdown. Thanks to Devin Jeanpierre for the patch.
- bpo-21058: Fix a leak of file descriptor in `tempfile.NamedTemporaryFile()` , close the file descriptor if`io.open()` fails
- bpo-21200: Return None from pkgutil.get_loader() when __spec__ is missing.
- bpo-21013: Enhance ssl.create_default_context() when used for server side sockets to provide better security by default.
- bpo-20145: `assertRaisesRegex` and`assertWarnsRegex` now raise a`TypeError` if the second argument is not a string or compiled regex.
- bpo-20633: Replace relative import by absolute import.
- bpo-20980: Stop wrapping exception when using ThreadPool.
- bpo-21082: In os.makedirs, do not set the process-wide umask. Note this changes behavior of makedirs when exist_ok=True.
- bpo-20990: Fix issues found by pyflakes for multiprocessing.
- bpo-21015: SSL contexts will now automatically select an elliptic curve for ECDH key exchange on OpenSSL 1.0.2 and later, and otherwise default to âprime256v1â.
- bpo-21000: Improve the command-line interface of json.tool.
- bpo-20995: Enhance default ciphers used by the ssl module to enable better security and prioritize perfect forward secrecy.
- bpo-20884: Donât assume that __file__ is defined on importlib.__init__.
- bpo-21499: Ignore __builtins__ in several test_importlib.test_api tests.
- bpo-20627: xmlrpc.client.ServerProxy is now a context manager.
- bpo-19165: The formatter module now raises DeprecationWarning instead of PendingDeprecationWarning.
- bpo-13936: Remove the ability of datetime.time instances to be considered false in boolean contexts.
- bpo-18931: selectors module now supports /dev/poll on Solaris. Patch by Giampaolo Rodolaâ.
- bpo-19977: When the `LC_TYPE` locale is the POSIX locale (`C` locale),`sys.stdin` and`sys.stdout` are now using the`surrogateescape` error handler, instead of the`strict` error
handler.
- bpo-20574: Implement incremental decoder for cp65001 code (Windows code page 65001, Microsoft UTF-8).
- bpo-20879: Delay the initialization of encoding and decoding tables for base32, ascii85 and base85 codecs in the base64 module, and delay the initialization of the unquote_to_bytes() table of the urllib.parse module, to not waste memory if these modules are not used.
- bpo-19157: Include the broadcast address in the usable hosts for IPv6 in ipaddress.
- bpo-11599: When an external command (e.g. compiler) fails, distutils now prints out the whole command line (instead of just the command name) if the environment variable DISTUTILS_DEBUG is set.
- bpo-4931: distutils should not produce unhelpful âerror: Noneâ messages anymore. distutils.util.grok_environment_error is kept but doc-deprecated.
- bpo-20875: Prevent possible gzip ââreadâ is not definedâ NameError. Patch by Claudiu Popa.
- bpo-11558: `email.message.Message.attach` now returns a more useful
error message if`attach` is called on a message for which`is_multipart` is False.
- bpo-20283: RE pattern methods now accept the string keyword parameters as documented. The pattern and source keyword parameters are left as deprecated aliases.
- bpo-20778: Fix modulefinder to work with bytecode-only modules.
- bpo-20791: copy.copy() now doesnât make a copy when the input is a bytes object. Initial patch by Peter Otten.
- bpo-19748: On AIX, time.mktime() now raises an OverflowError for year outsize range [1902; 2037].
- bpo-19573: inspect.signature: Use enum for parameter kind constants.
- bpo-20726: inspect.signature: Make Signature and Parameter picklable.
- bpo-17373: Add inspect.Signature.from_callable method.
- bpo-20378: Improve repr of inspect.Signature and inspect.Parameter.
- bpo-20816: Fix inspect.getcallargs() to raise correct TypeError for missing keyword-only arguments. Patch by Jeremiah Lowin.
- bpo-20817: Fix inspect.getcallargs() to fail correctly if more than 3 arguments are missing. Patch by Jeremiah Lowin.
- bpo-6676: Ensure a meaningful exception is raised when attempting to parse more than one XML document per pyexpat xmlparser instance. (Original patches by Hirokazu Yamamoto and Amaury Forgeot dâArc, with suggested wording by David Gutteridge)
- bpo-21117: Fix inspect.signature to better support functools.partial. Due to the specifics of functools.partial implementation, positional-or-keyword arguments passed as keyword arguments become keyword-only.
- bpo-20334: inspect.Signature and inspect.Parameter are now hashable. Thanks to Antony Lee for bug reports and suggestions.
- bpo-15916: doctest.DocTestSuite returns an empty unittest.TestSuite instead of raising ValueError if it finds no tests
- bpo-21209: Fix asyncio.tasks.CoroWrapper to workaround a bug in yield-from implementation in CPythons prior to 3.4.1.
- asyncio: Add gi_{frame,running,code} properties to CoroWrapper (upstream bpo-163).
- bpo-21311: Avoid exception in _osx_support with non-standard compiler configurations. Patch by John Szakmeister.
- bpo-11571: Ensure that the turtle window becomes the topmost window when launched on OS X.
- bpo-21801: Validate that __signature__ is None or an instance of Signature.
- bpo-21923: Prevent AttributeError in distutils.sysconfig.customize_compiler due to possible uninitialized _config_vars.
- bpo-21323: Fix http.server to again handle scripts in CGI subdirectories, broken by the fix for security bpo-19435. Patch by Zach Byrne.
- bpo-22733: Fix ffi_prep_args not zero-extending argument values correctly on 64-bit Windows.
- bpo-23302: Default to TCP_NODELAY=1 upon establishing an HTTPConnection. Removed use of hard-coded MSS as itâs an optimization thatâs no longer needed with Nagle disabled.

### IDLEÂ¶

- bpo-20577: Configuration of the max line length for the FormatParagraph extension has been moved from the General tab of the Idle preferences dialog to the FormatParagraph tab of the Config Extensions dialog. Patch by Tal Einat.
- bpo-16893: Update Idle doc chapter to match current Idle and add new information.
- bpo-3068: Add Idle extension configuration dialog to Options menu. Changes are written to HOME/.idlerc/config-extensions.cfg. Original patch by Tal Einat.
- bpo-16233: A module browser (File : Class Browser, Alt+C) requires an editor window with a filename. When Class Browser is requested otherwise, from a shell, output window, or âUntitledâ editor, Idle no longer displays an error box. It now pops up an Open Module box (Alt+M). If a valid name is entered and a module is opened, a corresponding browser is also opened.
- bpo-4832: Save As to type Python files automatically adds .py to the name you enter (even if your system does not display it). Some systems automatically add .txt when type is Text files.
- bpo-21986: Code objects are not normally pickled by the pickle module. To match this, they are no longer pickled when running under Idle.
- bpo-17390: Adjust Editor window title; remove âPythonâ, move version to end.
- bpo-14105: Idle debugger breakpoints no longer disappear when inserting or deleting lines.
- bpo-17172: Turtledemo can now be run from Idle. Currently, the entry is on the Help menu, but it may move to Run. Patch by Ramchandra Apt and Lita Cho.
- bpo-21765: Add support for non-ascii identifiers to HyperParser.
- bpo-21940: Add unittest for WidgetRedirector. Initial patch by Saimadhav Heblikar.
- bpo-18592: Add unittest for SearchDialogBase. Patch by Phil Webster.
- bpo-21694: Add unittest for ParenMatch. Patch by Saimadhav Heblikar.
- bpo-21686: add unittest for HyperParser. Original patch by Saimadhav Heblikar.
- bpo-12387: Add missing upper(lower)case versions of default Windows key bindings for Idle so Caps Lock does not disable them. Patch by Roger Serwy.
- bpo-21695: Closing a Find-in-files output window while the search is still in progress no longer closes Idle.
- bpo-18910: Add unittest for textView. Patch by Phil Webster.
- bpo-18292: Add unittest for AutoExpand. Patch by Saihadhav Heblikar.
- bpo-18409: Add unittest for AutoComplete. Patch by Phil Webster.
- bpo-21477: htest.py - Improve framework, complete set of tests. Patches by Saimadhav Heblikar
- bpo-18104: Add idlelib/idle_test/htest.py with a few sample tests to begin consolidating and improving human-validated tests of Idle. Change other files as needed to work with htest. Running the module as __main__ runs all tests.
- bpo-21139: Change default paragraph width to 72, the **PEP 8** recommendation.
- bpo-21284: Paragraph reformat test passes after user changes reformat width.
- bpo-17654: Ensure IDLE menus are customized properly on OS X for non-framework builds and for all variants of Tk.
- bpo-23180: Rename IDLE âWindowsâ menu item to âWindowâ. Patch by Al Sweigart.

### BuildÂ¶

- bpo-15506: Use standard PKG_PROG_PKG_CONFIG autoconf macro in the configure script.
- bpo-22935: Allow the ssl module to be compiled if openssl doesnât support SSL 3.
- bpo-22592: Drop support of the Borland C compiler to build Python. The distutils module still supports it to build extensions.
- bpo-22591: Drop support of MS-DOS, especially of the DJGPP compiler (MS-DOS port of GCC).
- bpo-16537: Check whether self.extensions is empty in setup.py. Patch by Jonathan Hosmer.
- bpo-22359: Remove incorrect uses of recursive make. Patch by Jonas Wagner.
- bpo-21958: Define HAVE_ROUND when building with Visual Studio 2013 and above. Patch by Zachary Turner.
- bpo-18093: the programs that embed the CPython runtime are now in a separate âProgramsâ directory, rather than being kept in the Modules directory.
- bpo-15759: âmake suspiciousâ, âmake linkcheckâ and âmake doctestâ in Doc/ now display special message when and only when there are failures.
- bpo-21141: The Windows build process no longer attempts to find Perl, instead relying on OpenSSL source being configured and ready to build. The `PCbuild\build_ssl.py` script has been re-written and re-named to`PCbuild\prepare_ssl.py` , and takes care of configuring OpenSSL source
for both 32 and 64 bit platforms.  OpenSSL sources obtained from
svn.python.org will always be pre-configured and ready to build.
- bpo-21037: Add a build option to enable AddressSanitizer support.
- bpo-19962: The Windows build process now creates âpython.batâ in the root of the source tree, which passes all arguments through to the most recently built interpreter.
- bpo-21285: Refactor and fix curses configure check to always search in a ncursesw directory.
- bpo-15234: For BerkeleyDB and Sqlite, only add the found library and include directories if they arenât already being searched. This avoids an explicit runtime library dependency.
- bpo-17861: Tools/scripts/generate_opcode_h.py automatically regenerates Include/opcode.h from Lib/opcode.py if the latter gets any change.
- bpo-20644: OS X installer build support for documentation build changes in 3.4.1: assume externally supplied sphinx-build is available in /usr/bin.
- bpo-20022: Eliminate use of deprecated bundlebuilder in OS X builds.
- bpo-15968: Incorporated Tcl, Tk, and Tix builds into the Windows build solution.
- bpo-17095: Fix Modules/Setup *shared* support.
- bpo-21811: Anticipated fixes to support OS X versions > 10.9.
- bpo-21166: Prevent possible segfaults and other random failures of python âgenerate-posix-vars in pybuilddir.txt build target.
- bpo-18096: Fix library order returned by python-config.
- bpo-17219: Add library build dir for Python extension cross-builds.
- bpo-22919: Windows build updated to support VC 14.0 (Visual Studio 2015), which will be used for the official release.
- bpo-21236: Build _msi.pyd with cabinet.lib instead of fci.lib
- bpo-17128: Use private version of OpenSSL for OS X 10.5+ installer.

### C APIÂ¶

- bpo-14203: Remove obsolete support for view==NULL in PyBuffer_FillInfo(), bytearray_getbuffer(), bytesiobuf_getbuffer() and array_buffer_getbuf(). All functions now raise BufferError in that case.
- bpo-22445: PyBuffer_IsContiguous() now implements precise contiguity tests, compatible with NumPyâs NPY_RELAXED_STRIDES_CHECKING compilation flag. Previously the function reported false negatives for corner cases.
- bpo-22079: PyType_Ready() now checks that statically allocated type has no dynamically allocated bases.
- bpo-22453: Removed non-documented macro PyObject_REPR().
- bpo-18395: Rename `_Py_char2wchar()` to`Py_DecodeLocale()` ,
rename`_Py_wchar2char()` to`Py_EncodeLocale()` , and document
these functions.
- bpo-21233: Add new C functions: PyMem_RawCalloc(), PyMem_Calloc(), PyObject_Calloc(), _PyObject_GC_Calloc(). bytes(int) is now using `calloc()` instead of`malloc()` for large objects which is faster and
use less memory.
- bpo-20942: PyImport_ImportFrozenModuleObject() no longer sets __file__ to match what importlib does; this affects _frozen_importlib as well as any module loaded using imp.init_frozen().

### DocumentationÂ¶

- bpo-19548: Update the codecs module documentation to better cover the distinction between text encodings and other codecs, together with other clarifications. Patch by Martin Panter.
- bpo-22394: Doc/Makefile now supports `make venv PYTHON=../python` to
create a venv for generating the documentation, e.g.,```
make html
PYTHON=venv/bin/python3
```
