---
id: collect-261121-veille-ia/docs-techniques/changelog-2-15
title: "Changelog"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["call", "system", "module", "faulthandler", "stack", "ctypes", "python", "time"]
source: https://docs.python.org/release/3.15.1/whatsnew/changelog.html
source_anchor: ""
source_lines: [21001, 22500]
section: "Docs_Techniques"
---
to produce a multiline indented output.
- bpo-34410: Fixed a crash in the `tee()` iterator when re-enter it.
RuntimeError is now raised in this case.
- bpo-37140: Fix a ctypes regression of Python 3.8. When a ctypes.Structure is passed by copy to a function, ctypes internals created a temporary object which had the side effect of calling the structure finalizer (__del__) twice. The Python semantics requires a finalizer to be called exactly once. Fix ctypes internals to no longer call the finalizer twice.
- bpo-37587: `_json.scanstring` is now up to 3x faster when there are many
backslash escaped characters in the JSON string.
- bpo-37834: Prevent shutil.rmtree exception when built on non-Windows system without fd system call support, like older versions of macOS.
- bpo-10978: Semaphores and BoundedSemaphores can now release more than one waiting thread at a time.
- bpo-37972: Subscripts to the `unittest.mock.call` objects now receive
the same chaining mechanism as any other custom attributes, so that the
following usage no longer raises a`TypeError` :`call().foo().__getitem__('bar')`Patch by blhsing
- bpo-37965: Fix C compiler warning caused by distutils.ccompiler.CCompiler.has_function.
- bpo-37964: Add `F_GETPATH` command to`fcntl` .
- bpo-37960: `repr()` of buffered and text streams now silences only
expected exceptions when get the value of ânameâ and âmodeâ attributes.
- bpo-37961: Add a `total_nframe` field to the traces collected by the
tracemalloc module. This field indicates the original number of frames
before it was truncated.
- bpo-37951: Most features of the subprocess module now work again in subinterpreters. Only *preexec_fn* is restricted in subinterpreters.
- bpo-36205: Fix the rusage implementation of time.process_time() to correctly report the sum of the system and user CPU time.
- bpo-37950: Fix `ast.dump()` when call with incompletely initialized
node.
- bpo-34679: Restores instantiation of Windows IOCP event loops from the non-main thread.
- bpo-36917: Add default implementation of the `ast.NodeVisitor.visit_Constant()` method which emits a deprecation
warning and calls corresponding methods`visit_Num()` ,`visit_Str()` ,
etc.
- bpo-37798: Update test_statistics.py to verify that the statistics module works well for both C and Python implementations. Patch by Donghee Na
- bpo-26589: Added a new status code to the http module: 451 UNAVAILABLE_FOR_LEGAL_REASONS
- bpo-37915: Fix a segmentation fault that appeared when comparing instances of `datetime.timezone` and`datetime.tzinfo` objects. Patch by Pablo
Galindo.
- bpo-32554: Deprecate having random.seed() call hash on arbitrary types.
- bpo-9938: Add optional keyword argument `exit_on_error` for`ArgumentParser` .
- bpo-37851: The `faulthandler` module no longer allocates its
alternative stack at Python startup. Now the stack is only allocated at
the first faulthandler usage.
- bpo-32793: Fix a duplicated debug message when `smtplib.SMTP.connect()` is called.
- bpo-37885: venv: Donât generate unset variable warning on deactivate.
- bpo-37868: Fix dataclasses.is_dataclass when given an instance that never raises AttributeError in __getattr__. That is, an object that returns something for __dataclass_fields__ even if itâs not a dataclass.
- bpo-37811: Fix `socket` moduleâs`socket.connect(address)` function
being unable to establish connection in case of interrupted system call.
The problem was observed on all OSes which`poll(2)` system call can
take only non-negative integers and -1 as a timeout value.
- bpo-37863: Optimizations for Fraction.__hash__ suggested by Tim Peters.
- bpo-21131: Fix `faulthandler.register(chain=True)` stack. faulthandler
now allocates a dedicated stack of`SIGSTKSZ*2` bytes, instead of just`SIGSTKSZ` bytes. Calling the previous signal handler in faulthandler
signal handler uses more than`SIGSTKSZ` bytes of stack memory on some
platforms.
- bpo-37798: Add C fastpath for statistics.NormalDist.inv_cdf() Patch by Donghee Na
- bpo-37804: Remove the deprecated method `threading.Thread.isAlive()` .
Patch by Donghee Na.
- bpo-37819: Add Fraction.as_integer_ratio() to match the corresponding methods in bool, int, float, and decimal.
- bpo-14465: Add an xml.etree.ElementTree.indent() function for pretty-printing XML trees. Contributed by Stefan Behnel.
- bpo-37810: Fix `difflib``?` hint in diff output when dealing with
tabs.  Patch by Anthony Sottile.
- bpo-37772: In `zipfile.Path` , when adding implicit dirs, ensure that
ancestral directories are added and that duplicates are excluded.
- bpo-18578: Renamed and documented `test.bytecode_helper` as`test.support.bytecode_helper` . Patch by Joannah Nanjekye.
- bpo-37785: Fix xgettext warnings in `argparse` .
- bpo-34488: `writelines()` method of`io.BytesIO` is now
slightly faster when many small lines are passed. Patch by Sergey
Fedoseev.
- bpo-37449: `ensurepip` now uses`importlib.resources.read_binary()` to
read data instead of`pkgutil.get_data()` . Patch by Joannah Nanjekye.
- bpo-28292: Mark calendar.py helper functions as being private. The follows PEP 8 guidance to maintain the style conventions in the module and it addresses a known case of user confusion.
- bpo-18049: Add definition of THREAD_STACK_SIZE for AIX in Python/thread_pthread.h The default thread stacksize caused crashes with the default recursion limit Patch by M Felt
- bpo-37742: The logging.getLogger() API now returns the root logger when passed the name ârootâ, whereas previously it returned a non-root logger named ârootâ. This could affect cases where user code explicitly wants a non-root logger named ârootâ, or instantiates a logger using logging.getLogger(__name__) in some top-level module called âroot.pyâ.
- bpo-37738: Fix the implementation of curses `addch(str, color_pair)` :
pass the color pair to`setcchar()` , instead of always passing 0 as the
color pair.
- bpo-37723: Fix performance regression on regular expression parsing with huge character sets. Patch by Yann Vaginay.
- bpo-35943: The function `PyImport_GetModule()` now ensures any
module it returns is fully initialized. Patch by Joannah Nanjekye.
- bpo-32178: Fix IndexError in `email` package when trying to parse
invalid address fields starting with`:` .
- bpo-37268: The `parser` module is deprecated and will be removed in
future versions of Python.
- bpo-11953: Completing WSA* error codes in `socket` .
- bpo-37685: Fixed comparisons of `datetime.timedelta` and`datetime.timezone` .
- bpo-37697: Synchronize `importlib.metadata` with importlib_metadata
0.19,
improving handling of EGG-INFO files and fixing a crash when entry point
names contained colons.
- bpo-37695: Correct `curses.unget_wch()` error message.  Patch by
Anthony Sottile.
- bpo-37689: Add `is_relative_to()` in`PurePath` to determine
whether or not one path is relative to another.
- bpo-29553: Fixed `argparse.ArgumentParser.format_usage()` for mutually
exclusive groups. Patch by Andrew Nester.
- bpo-37691: Let math.dist() accept coordinates as sequences (or iterables) rather than just tuples.
- bpo-37685: Fixed `__eq__` ,`__lt__` etc implementations in some
classes. They now return`NotImplemented` for unsupported type of
the other operand. This allows the other operand to play role (for example
the equality comparison with`ANY` will return`True` ).
- bpo-37354: Make Activate.ps1 Powershell script static to allow for signing it.
- bpo-37664: Update wheels bundled with ensurepip (pip 19.2.3 and setuptools 41.2.0)
- bpo-37663: Bring consistency to venv shell activation scripts by always using __VENV_PROMPT__.
- bpo-37642: Allowed the pure Python implementation of `datetime.timezone` to represent sub-minute offsets close to
minimum and maximum boundaries, specifically in the ranges (23:59, 24:00)
and (-23:59, 24:00). Patch by Ngalim Siregar
- bpo-36161: In `posix` , use`ttyname_r` instead of`ttyname` for
thread safety.
- bpo-36324: Make internal attributes for statistics.NormalDist() private.
- bpo-37555: Fix `NonCallableMock._call_matcher` returning tuple instead
of`_Call` object when`self._spec_signature` exists. Patch by
Elizabeth Uselton
- bpo-29446: Make `from tkinter import *` import only the expected
objects.
- bpo-16970: Adding a value error when an invalid value in passed to nargs Patch by Robert Leenders
- bpo-34443: Exceptions from `enum` now use the`__qualname` of the
enum class in the exception message instead of the`__name__` .
- bpo-37491: Fix `IndexError` when parsing email headers with unexpectedly
ending bare-quoted string value. Patch by Abhilash Raj.
- bpo-37587: Make json.loads faster for long strings. (Patch by Marco Paolini)
- bpo-18378: Recognize âUTF-8â as a valid value for LC_CTYPE in locale._parse_localename.
- bpo-37579: Return `NotImplemented` in Python implementation of`__eq__` for`timedelta` and`time` when the other object being compared is not of the same type to match C
implementation. Patch by Karthikeyan Singaravelan.
- bpo-21478: Record calls to parent when autospecced object is attached to a mock using `unittest.mock.attach_mock()` . Patch by Karthikeyan
Singaravelan.
- bpo-37531: âpython3 -m test -jN âtimeout=TIMEOUTâ now kills a worker process if it runs longer than *TIMEOUT* seconds.
- bpo-37482: Fix serialization of display name in originator or destination address fields with both encoded words and special chars.
- bpo-36993: Improve error reporting for corrupt zip files with bad zip64 extra data. Patch by Daniel Hillier.
- bpo-37502: pickle.loads() no longer raises TypeError when the buffers argument is set to None
- bpo-37520: Correct behavior for zipfile.Path.parent when the path object identifies a subdirectory.
- bpo-18374: Fix the `.col_offset` attribute of nested`ast.BinOp` instances which had a too large value in some situations.
- bpo-37424: Fixes a possible hang when using a timeout on `subprocess.run()` while capturing output.  If the child process spawned
its own children or otherwise connected its stdout or stderr handles with
another process, we could hang after the timeout was reached and our child
was killed when attempting to read final output from the pipes.
- bpo-37421: Fix `multiprocessing.util.get_temp_dir()` finalizer: clear
also the âtempdirâ configuration of the current process, so next call to`get_temp_dir()` will create a new temporary directory, rather than
reusing the removed temporary directory.
- bpo-37481: The distutils `bdist_wininst` command is deprecated in Python
3.8, use`bdist_wheel` (wheel packages) instead.
- bpo-37479: When `Enum.__str__` is overridden in a derived class, the
override will be used by`Enum.__format__` regardless of whether mixin
classes are present.
- bpo-37440: http.client now enables TLS 1.3 post-handshake authentication for default context or if a cert_file is passed to HTTPSConnection.
- bpo-37437: Update vendorized expat version to 2.2.7.
- bpo-37428: SSLContext.post_handshake_auth = True no longer sets SSL_VERIFY_POST_HANDSHAKE verify flag for client connections. Although the option is documented as ignored for clients, OpenSSL implicitly enables cert chain validation when the flag is set.
- bpo-37420: `os.sched_setaffinity()` now correctly handles errors that
arise during iteration over its`mask` argument. Patch by Brandt Bucher.
- bpo-37412: The `os.getcwdb()` function now uses the UTF-8 encoding on
Windows, rather than the ANSI code page: see**PEP 529** for the rationale.
The function is no longer deprecated on Windows.
- bpo-37406: The sqlite3 module now raises TypeError, rather than ValueError, if operation argument type is not str: execute(), executemany() and calling a connection.
- bpo-29412: Fix IndexError in parsing a header value ending unexpectedly. Patch by Abhilash Raj.
- bpo-36546: The *dist* argument for statistics.quantiles() is now
positional only. The current name doesnât reflect that the argument can be
either a dataset or a distribution.  Marking the parameter as positional
avoids confusion and makes it possible to change the name later.
- bpo-37394: Fix a bug that was causing the `queue` module to fail if
the accelerator module was not available. Patch by Pablo Galindo.
- bpo-37376: `pprint` now has support for`types.SimpleNamespace` . Patch by Carl Bordum Hansen.
- bpo-26967: An `ArgumentParser` with`allow_abbrev=False` no longer disables grouping of short flags, such as`-vv` , but only disables abbreviation of long flags as documented. Patch
by Zac Hatfield-Dodds.
- bpo-37212: `unittest.mock.call()` now preserves the order of keyword
arguments in repr output. Patch by Karthikeyan Singaravelan.
- bpo-37372: Fix error unpickling datetime.time objects from Python 2 with seconds>=24. Patch by Justin Blanchard.
- bpo-37345: Add formal support for UDPLITE sockets. Support was present before, but it is now easier to detect support with ```
hasattr(socket,
'IPPROTO_UDPLITE')
```
and there are constants defined for each of the
values needed:`socket.IPPROTO_UDPLITE` ,`UDPLITE_SEND_CSCOV` , and`UDPLITE_RECV_CSCOV` . Patch by Gabe Appleton.
- bpo-37358: Optimized `functools.partial` by using vectorcall.
- bpo-37347: `sqlite3.Connection.create_aggregate()` ,`sqlite3.Connection.create_function()` ,`sqlite3.Connection.set_authorizer()` ,`sqlite3.Connection.set_progress_handler()``sqlite3.Connection.set_trace_callback()` methods lead to segfaults if
some of these methods are called twice with an equal object but not the
same. Now callbacks are stored more carefully. Patch by Aleksandr Balezin.
- bpo-37163: The *obj* argument of`dataclasses.replace()` is
positional-only now.
- bpo-37085: Add the optional Linux SocketCAN Broadcast Manager constants, used as flags to configure the BCM behaviour, in the socket module. Patch by Karl Ding.
- bpo-37328: `HTMLParser.unescape` is removed.  It was undocumented and
deprecated since Python 3.4.
- bpo-37305: Add .webmanifest -> application/manifest+json to list of recognized file types and content type headers
- bpo-37320: `aifc.openfp()` alias to`aifc.open()` ,`sunau.openfp()` alias to`sunau.open()` , and`wave.openfp()` alias to`wave.open()` have been removed. They were deprecated since Python 3.7.
- bpo-37315: Deprecated accepting floats with integral value (like `5.0` )
in`math.factorial()` .
- bpo-37312: `_dummy_thread` and`dummy_threading` modules have been
removed. These modules were deprecated since Python 3.7 which requires
threading support.
- bpo-33972: Email with single part but content-type set to `multipart/*` doesnât raise AttributeError anymore.
- bpo-37280: Use threadpool for reading from file for sendfile fallback mode.
- bpo-37279: Fix asyncio sendfile support when sendfile sends extra data in fallback mode.
- bpo-19865: `ctypes.create_unicode_buffer()` now also supports non-BMP
characters on platforms with 16-bit`wchar_t` (for example,
Windows and AIX).
- bpo-37266: In a subinterpreter, spawning a daemon thread now raises an exception. Daemon threads were never supported in subinterpreters. Previously, the subinterpreter finalization crashed with a Python fatal error if a daemon thread was still running.
- bpo-37210: Allow pure Python implementation of `pickle` to work even
when the C`_pickle` module is unavailable.
- bpo-21872: Fix `lzma` : module decompresses data incompletely. When
decompressing a FORMAT_ALONE format file, and it doesnât have the end
marker, sometimes the last one to dozens bytes canât be output. Patch by
Ma Lin.
- bpo-35922: Fix `RobotFileParser.crawl_delay()` and`RobotFileParser.request_rate()` to return`None` rather than raise`AttributeError` when no relevant rule is defined in the robots.txt
file.  Patch by RÃ©mi Lapeyre.
- bpo-35766: Change the format of feature_version to be a (major, minor) tuple.
- bpo-36607: Eliminate `RuntimeError` raised by`asyncio.all_tasks()` if internal tasks weak set is changed by another
thread during iteration.
- bpo-18748: `_pyio.IOBase` destructor now does nothing if getting
the`closed` attribute fails to better mimic`_io.IOBase` finalizer.
- bpo-36402: Fix a race condition at Python shutdown when waiting for threads. Wait until the Python thread state of all non-daemon threads get deleted (join all non-daemon threads), rather than just wait until non-daemon Python threads complete.
- bpo-37206: Default values which cannot be represented as Python objects no longer improperly represented as `None` in function signatures.
- bpo-37111: Added `encoding` and`errors` keyword parameters to`logging.basicConfig` .
- bpo-12144: Ensure cookies with `expires` attribute are handled in`CookieJar.make_cookies()` .
- bpo-34886: Fix an unintended ValueError from `subprocess.run()` when
checking for conflicting*input* and*stdin* or*capture_output* and*stdout* or*stderr* args when they were explicitly provided but with`None` values within a passed in`**kwargs` dict rather than as passed
directly by name. Patch contributed by RÃ©mi Lapeyre.
- bpo-37173: The exception message for `inspect.getfile()` now correctly
reports the passed class rather than the builtins module.
- bpo-37178: Give math.perm() a one argument form that means the same as math.factorial().
- bpo-37178: For math.perm(n, k), let k default to n, giving the same result as factorial.
- bpo-37165: Converted _collections._count_elements to use the Argument Clinic.
- bpo-34767: Do not always create a `collections.deque` in`asyncio.Lock` .
- bpo-37158: Speed-up statistics.fmean() by switching from a function to a generator.
- bpo-34282: Remove `Enum._convert` method, deprecated in 3.8.
- bpo-37150: `argparse._ActionsContainer.add_argument` now throws error,
if someone accidentally pass FileType class object instead of instance of
FileType as`type` argument.
- bpo-28724: The socket module now has the `socket.send_fds()` and`socket.recv.fds()` methods. Contributed by Joannah Nanjekye, Shinya
Okano and Victor Stinner.
- bpo-35621: Support running asyncio subprocesses when execution event loop in a thread on UNIX.
- bpo-36520: Lengthy email headers with UTF-8 characters are now properly encoded when they are folded. Patch by Jeffrey Kintscher.
- bpo-30835: Fixed a bug in email parsing where a message with invalid bytes in content-transfer-encoding of a multipart message can cause an AttributeError. Patch by Andrew Donnellan.
- bpo-31163: pathlib.Path instanceâs rename and replace methods now return the new Path instance.
- bpo-25068: `urllib.request.ProxyHandler` now lowercases the keys of
the passed dictionary.
- bpo-26185: Fix `repr()` on empty`ZipInfo` object. Patch by
MickaÃ«l Schoentgen.
- bpo-21315: Email headers containing RFC2047 encoded words are parsed despite the missing whitespace, and a defect registered. Also missing trailing whitespace after encoded words is now registered as a defect.
- bpo-31904: Port test_datetime to VxWorks: skip zoneinfo tests on VxWorks
- bpo-35805: Add parser for Message-ID header and add it to default HeaderRegistry. This should prevent folding of Message-ID using RFC 2048 encoded words.
- bpo-36871: Ensure method signature is used instead of constructor signature of a class while asserting mock object against method calls. Patch by Karthikeyan Singaravelan.
- bpo-35070: posix.getgrouplist() now works correctly when the user belongs to NGROUPS_MAX supplemental groups. Patch by Jeffrey Kintscher.
- bpo-31783: Fix race condition in ThreadPoolExecutor when worker threads are created during interpreter shutdown.
- bpo-36582: Fix `UserString.encode()` to correctly return`bytes` rather than a`UserString` instance.
- bpo-32424: Deprecate xml.etree.ElementTree.Element.copy() in favor of copy.copy(). Patch by Gordon P. Hemsley
- bpo-36564: Fix infinite loop in email header folding logic that would be triggered when an email policyâs max_line_length is not long enough to include the required markup and any values in the message. Patch by Paul Ganssle
- bpo-36543: Removed methods Element.getchildren(), Element.getiterator() and ElementTree.getiterator() and the xml.etree.cElementTree module.
- bpo-36409: Remove the old plistlib API deprecated in Python 3.4
- bpo-36302: distutils sorts source file lists so that Extension .so files build more reproducibly by default
- bpo-36250: Ignore `ValueError` from`signal` with`interaction` in
non-main thread.
- bpo-36046: Added `user` ,`group` and`extra_groups` parameters to
the subprocess.Popen constructor. Patch by Patrick McLean.
- bpo-32627: Fix compile error when `_uuid` headers conflicting included.
- bpo-35800: Deprecate `smtpd.MailmanProxy` ready for future removal.
- bpo-35168: `shlex.shlex.punctuation_chars` is now a read-only
property.
- bpo-8538: Add support for boolean actions like `--foo` and`--no-foo` to argparse. Patch contributed by RÃ©mi Lapeyre.
- bpo-20504: Fixes a bug in `cgi` module when a multipart/form-data
request has no`Content-Length` header.
- bpo-25988: The abstract base classes in `collections.abc` no longer
are exposed in the regular`collections` module.
- bpo-11122: Distutils wonât check for rpmbuild in specified paths only.
- bpo-34775: Division handling of PurePath now returns NotImplemented instead of raising a TypeError when passed something other than an instance of str or PurePath. Patch by Roger Aiudi.
- bpo-34749: `binascii.a2b_base64()` is now up to 2 times faster. Patch
by Sergey Fedoseev.
- bpo-34519: Add additional aliases for HP Roman 8. Patch by Michael Osipov.
- bpo-28009: Fix uuid.getnode() on platforms with â.â as MAC Addr delimiter as well fix for MAC Addr format that omits a leading 0 in MAC Addr values. Currently, AIX is the only know platform with these settings. Patch by Michael Felt.
- bpo-30618: Add `readlink()` . Patch by Girts Folkmanis.
- bpo-32498: Made `urllib.parse.unquote()` accept bytes in addition to
strings. Patch by Stein Karlsen.
- bpo-33348: lib2to3 now recognizes expressions after `*` and`**` like
in`f(*[] or [])` .
- bpo-32689: Update `shutil.move()` function to allow for Path objects
to be used as source argument. Patch by Emily Morehouse and Maxwell
â5.13bâ McKinnon.
- bpo-32820: Added `__format__` to IPv4 and IPv6 classes.  Always outputs
a fully zero-padded string. Supports b/x/n modifiers (bin/hex/native
format). Native format for IPv4 is bin, native format for IPv6 is hex.
Also supports â#â and â_â modifiers.
- bpo-27657: Fix urllib.parse.urlparse() with numeric paths. A string like âpath:80â is no longer parsed as a path but as a scheme (âpathâ) and a path (â80â).
- bpo-4963: Fixed non-deterministic behavior related to mimetypes extension mapping and module reinitialization.

### DocumentationÂ¶

- bpo-21767: Explicitly mention abc support in functools.singledispatch
- bpo-38816: Provides more details about the interaction between `fork()` and CPythonâs runtime, focusing just on the C-API.  This
includes cautions about where`fork()` should and shouldnât be
called.
- bpo-38351: Modernize `email` examples from %-formatting to f-strings.
- bpo-38778: Document the fact that `RuntimeError` is raised if`os.fork()` is called in a subinterpreter.
- bpo-38592: Add Brazilian Portuguese to the language switcher at Python Documentation website.
- bpo-38294: Add list of no-longer-escaped chars to re.escape documentation
- bpo-38053: Modernized the plistlib documentation
- bpo-26868: Fix example usage of `PyModule_AddObject()` to properly
handle errors.
- bpo-36797: Fix a dead link in the distutils API Reference.
- bpo-37977: Warn more strongly and clearly about pickle insecurity
- bpo-37979: Added a link to dateutil.parser.isoparse in the datetime.fromisoformat documentation. Patch by Paul Ganssle
- bpo-12707: Deprecate info(), geturl(), getcode() methods in favor of the headers, url, and status properties, respectively, for HTTPResponse and addinfourl. Also deprecate the code attribute of addinfourl in favor of the status attribute. Patch by Ashwin Ramaswami
- bpo-37937: Mention `frame.f_trace` in`sys.settrace()` docs.
- bpo-37878: Make `PyThreadState_DeleteCurrent()` Internal.
- bpo-37759: Beginning edits to Whatsnew 3.8
- bpo-37726: Stop recommending getopt in the tutorial for command line argument parsing and promote argparse.
- bpo-32910: Remove implementation-specific behaviour of how venvâs Deactivate works.
- bpo-37256: Fix wording of arguments for `Request` in`urllib.request`
- bpo-37284: Add a brief note to indicate that any new `sys.implementation` required attributes must go through the PEP
process.
- bpo-30088: Documented that `mailbox.Maildir` constructor doesnât
attempt to verify the maildir folder layout correctness. Patch by
Sviatoslav Sydorenko.
- bpo-37521: Fix `importlib` examples to insert any newly created modules
via importlib.util.module_from_spec() immediately into sys.modules instead
of after calling loader.exec_module().Thanks to Benjamin Mintz for finding the bug.
- bpo-37456: Slash (â/â) is now part of syntax.
- bpo-37487: Fix PyList_GetItem index description to include 0.
- bpo-37149: Replace the dead link to the Tkinter 8.5 reference by John Shipman, New Mexico Tech, with a link to the archive.org copy.
- bpo-37478: Added possible exceptions to the description of os.chdir().
- bpo-34903: Documented that in `datetime.datetime.strptime()` , the
leading zero in some two-digit formats is optional. Patch by Mike Gleen.
- bpo-36260: Add decompression pitfalls to zipfile module documentation.
- bpo-37004: In the documentation for difflib, a note was added explicitly warning that the results of SequenceMatcherâs ratio method may depend on the order of the input strings.
- bpo-36960: Restructured the `datetime` docs in the interest of making
them more user-friendly and improving readability. Patch by Brad Solomon.
- bpo-36487: Make C-API docs clear about what the âmainâ interpreter is.
- bpo-23460: The documentation for decimal string formatting using the `:g` specifier has been updated to reflect the correct exponential
notation cutoff point. Original patch contributed by Tuomas Suutari.
- bpo-35803: Document and test that `tempfile` functions may accept a
path-like object for the`dir` argument.  Patch by Anthony
Sottile.
- bpo-33944: Added a note about the intended use of code in .pth files.
- bpo-34293: Fix the Doc/Makefile regarding PAPER environment variable and PDF builds
- bpo-25237: Add documentation for tkinter modules

### TestsÂ¶

- bpo-38614: Fix test_communicate() of test_asyncio.test_subprocess: use `support.LONG_TIMEOUT` (5 minutes), instead of just 1 minute.
- bpo-38614: Add timeout constants to `test.support` :`LOOPBACK_TIMEOUT` ,`INTERNET_TIMEOUT` ,`SHORT_TIMEOUT` and`LONG_TIMEOUT` .
- bpo-38502: test.regrtest now uses process groups in the multiprocessing mode (-jN command line option) if process groups are available: if `os.setsid()` and`os.killpg()` functions are available.
- bpo-35998: Fix a race condition in test_asyncio.test_start_tls_server_1(). Previously, there was a race condition between the test main() function which replaces the protocol and the test ServerProto protocol which sends ANSWER once it gets HELLO. Now, only the test main() function is responsible to send data, ServerProto no longer sends data.
- bpo-38470: Fix `test_compileall.test_compile_dir_maxlevels()` on Windows
without long path support: only create 3 subdirectories instead of between
20 and 100 subdirectories.
- bpo-37531: On timeout, regrtest no longer attempts to call `popen.communicate()` again: it can hang until all child processes using
stdout and stderr pipes completes. Kill the worker process and ignores its
output. Change also the faulthandler timeout of the main process from 1
minute to 5 minutes, for Python slowest buildbots.
- bpo-38239: Fix test_gdb for Link Time Optimization (LTO) builds.
- bpo-38275: test_ssl now handles disabled TLS/SSL versions better. OpenSSLâs crypto policy and run-time settings are recognized and tests for disabled versions are skipped. Tests also accept more TLS minimum_versions for platforms that override OpenSSLâs default with strict settings.
- bpo-38271: The private keys for test_ssl were encrypted with 3DES in traditional PKCS#5 format. 3DES and the digest algorithm of PKCS#5 are blocked by some strict crypto policies. Use PKCS#8 format with AES256 encryption instead.
- bpo-38270: test.support now has a helper function to check for availability of a hash digest function. Several tests are refactored avoid MD5 and use SHA256 instead. Other tests are marked to use MD5 and skipped when MD5 is disabled.
- bpo-37123: Multiprocessing test test_mymanager() now also expects -SIGTERM, not only exitcode 0. BaseManager._finalize_manager() sends SIGTERM to the manager process if it takes longer than 1 second to stop, which happens on slow buildbots.
- bpo-38212: Multiprocessing tests: increase test_queue_feeder_donot_stop_onexc() timeout from 1 to 60 seconds.
- bpo-38117: Test with OpenSSL 1.1.1d
- bpo-38018: Increase code coverage for multiprocessing.shared_memory.
- bpo-37805: Add tests for json.dump(â¦, skipkeys=True). Patch by Donghee Na.
- bpo-37531: Enhance regrtest multiprocess timeout: write a message when killing a worker process, catch popen.kill() and popen.wait() exceptions, put a timeout on the second call to popen.communicate().
- bpo-37876: Add tests for ROT-13 codec.
- bpo-36833: Added tests for `PyDateTime_`*xxx* _GET_*xxx* ()
macros of
the C API of the`datetime` module. Patch by Joannah Nanjekye.
- bpo-37558: Fix test_shared_memory_cleaned_after_process_termination name handling
- bpo-37526: Add `test.support.catch_threading_exception()` : context
manager catching`threading.Thread` exception using`threading.excepthook()` .
- bpo-37421: test_concurrent_futures now explicitly stops the ForkServer instance if itâs running.
- bpo-37421: multiprocessing tests now stop the ForkServer instance if itâs running: close the âaliveâ file descriptor to ask the server to stop and then remove its UNIX address.
- bpo-37421: test_distutils.test_build_ext() is now able to remove the temporary directory on Windows: donât import the newly built C extension (âxxâ) in the current process, but test it in a separated process.
- bpo-37421: test_concurrent_futures now cleans up multiprocessing to remove immediately temporary directories created by multiprocessing.util.get_temp_dir().
- bpo-37421: test_winconsoleio doesnât leak a temporary file anymore: use tempfile.TemporaryFile() to remove it when the test completes.
- bpo-37421: multiprocessing tests now explicitly call `_run_finalizers()` to immediately remove temporary directories created by tests.
- bpo-37421: urllib.request tests now call `urlcleanup()` to remove temporary files created by`urlretrieve()` tests and to clear the`_opener` global variable set
by`urlopen()` and functions calling indirectly`urlopen()` .
- bpo-37472: Remove `Lib/test/outstanding_bugs.py` .
- bpo-37199: Fix test failures when IPv6 is unavailable or disabled.
- bpo-19696: Replace deprecated method ârandom.chooseâ with ârandom.choiceâ in âtest_pkg_import.pyâ.
- bpo-37335: Remove no longer necessary code from c locale coercion tests
- bpo-37421: Fix test_shutil to no longer leak temporary files.
- bpo-37411: Fix test_wsgiref.testEnviron() to no longer depend on the environment variables (donât fail if âXâ variable is set).
- bpo-37400: Fix test_os.test_chown(): use os.getgroups() rather than grp.getgrall() to get groups. Rename also the test to test_chown_gid().
- bpo-37359: Add âcleanup option to python3 -m test to remove `test_python_*` directories of previous failed jobs. Add âmake
cleantestâ to run`python3 -m test --cleanup` .
- bpo-37362: test_gdb no longer fails if it gets an âunexpectedâ message on stderr: it now ignores stderr. The purpose of test_gdb is to test that python-gdb.py commands work as expected, not to test gdb.
- bpo-35998: Avoid TimeoutError in test_asyncio: test_start_tls_server_1()
- bpo-37278: Fix test_asyncio ProactorLoopCtrlC: join the thread to prevent leaking a running thread and leaking a reference.
- bpo-37261: Fix `test.support.catch_unraisable_exception()` : its
__exit__() method now ignores unraisable exception raised when clearing
its`unraisable` attribute.
- bpo-37069: regrtest now uses `sys.unraisablehook()` to mark a test as
âenvironment alteredâ (ENV_CHANGED) if it emits an âunraisable exceptionâ.
Moreover, regrtest logs a warning in this case.Use `python3 -m test --fail-env-changed` to catch unraisable exceptions
in tests.
- bpo-37252: Fix assertions in `test_close` and`test_events_mask_overflow` devpoll tests.
- bpo-37169: Rewrite `_PyObject_IsFreed()` unit tests.
- bpo-37153: `test_venv.test_multiprocessing()` now explicitly calls`pool.terminate()` to wait until the pool completes.
- bpo-34001: Make test_ssl pass with LibreSSL. LibreSSL handles minimum and maximum TLS version differently than OpenSSL.
- bpo-36919: Make `test_source_encoding.test_issue2301` implementation
independent. The test will work now for both CPython and IronPython.
- bpo-30202: Update `test.test_importlib.test_abc` to test`find_spec()` .
- bpo-28009: Modify the test_uuid logic to test when a program is available AND can be used to obtain a MACADDR as basis for an UUID. Patch by M. Felt
- bpo-34596: Fallback to a default reason when `unittest.skip()` is
uncalled. Patch by Naitree Zhu.

### BuildÂ¶

- bpo-38809: On Windows, build scripts will now recognize and use python.exe from an active virtual env.
- bpo-38684: Fix _hashlib build when Blake2 is disabled, but OpenSSL supports it.
- bpo-38468: Misc/python-config.in now uses `getvar()` for all still
existing`sysconfig.get_config_var()` calls. Patch by Joannah Nanjekye.
- bpo-37415: Fix stdatomic.h header check for ICC compiler: the ICC implementation lacks atomic_uintptr_t type which is needed by Python.
- bpo-38301: In Solaris family, we must be sure to use `-D_REENTRANT` .
Patch by JesÃºs Cea AviÃ³n.
- bpo-36002: Locate `llvm-profdata` and`llvm-ar` binaries using`AC_PATH_TOOL` rather than`AC_PATH_TARGET_TOOL` .
- bpo-37936: The `.gitignore` file systematically keeps ârootedâ, with
a non-trailing slash, all the rules that are meant to apply to files in a
specific place in the repo.  Previously, when the intended file to ignore
happened to be at the root of the repo, weâd most often accidentally also
ignore files and directories with the same name anywhere in the tree.
- bpo-37760: The `Tools/unicode/makeunicodedata.py` script, which is
used for converting information from the Unicode Character Database into
generated code and data used by the methods of`str` and by the`unicodedata` module, now handles each characterâs data as a`dataclass` with named attributes, rather than a length-18 list of
different fields.
- bpo-37936: The `.gitignore` file no longer applies to any files that
are in fact tracked in the Git repository.  Patch by Greg Price.
- bpo-37725: Change âcleanâ makefile target to also clean the program guided optimization (PGO) data. Previously you would have to use âmake cleanâ and âmake profile-removalâ, or âmake clobberâ.
- bpo-37707: Mark some individual tests to skip when âpgo is used. The tests marked increase the PGO task time significantly and likely donât help improve optimization of the final executable.
- bpo-36044: Reduce the number of unit tests run for the PGO generation task. This speeds up the task by a factor of about 15x. Running the full unit test suite is slow. This change may result in a slightly less optimized build since not as many code branches will be executed. If you are willing to wait for the much slower build, the old behavior can be restored using â./configure [..] PROFILE_TASK=â-m test âpgo-extendedââ. We make no guarantees as to which PGO task set produces a faster build. Users who care should run their own relevant benchmarks as results can depend on the environment, workload, and compiler tool chain.
- bpo-37468: `make install` no longer installs`wininst-*.exe` files
used by distutils bdist_wininst: bdist_wininst only works on Windows.
- bpo-37189: Many `PyRun_`*XXX* ()
functions like`PyRun_String()` were no longer exported in`libpython38.dll` by
mistake. Export them again to fix the ABI compatibility.
- bpo-25361: Enables use of SSE2 instructions in Windows 32-bit build.
- bpo-36210: Update optional extension module detection for AIX. ossaudiodev and spwd are not applicable for AIX, and are no longer reported as missing. 3rd-party packaging of ncurses (with ASIS support) conflicts with officially supported AIX curses library, so configure AIX to use libcurses.a. However, skip trying to build _curses_panel. patch by M Felt

### WindowsÂ¶

- bpo-38589: Fixes HTML Help shortcut when Windows is not installed to C drive
- bpo-38453: Ensure ntpath.realpath() correctly resolves relative paths.
- bpo-38519: Restores the internal C headers that were missing from the nuget.org and Microsoft Store packages.
- bpo-38492: Remove `pythonw.exe` dependency on the Microsoft C++ runtime.
- bpo-38344: Fix error message in activate.bat
- bpo-38359: Ensures `pyw.exe` launcher reads correct registry key.
- bpo-38355: Fixes `ntpath.realpath` failing on`sys.executable` .
- bpo-38117: Update bundled OpenSSL to 1.1.1d
- bpo-38092: Reduce overhead when using multiprocessing in a Windows virtual environment.
- bpo-38133: Allow py.exe launcher to locate installations from the Microsoft Store and improve display of active virtual environments.
- bpo-38114: The `pip.ini` is no longer included in the Nuget package.
- bpo-32592: Set Windows 8 as the minimum required version for API support
- bpo-36634: `os.cpu_count()` now returns active processors rather than
maximum processors.
- bpo-36634: venv activate.bat now works when the existing variables contain double quote characters.
- bpo-38081: Prevent error calling `os.path.realpath()` on`'NUL'` .
- bpo-38087: Fix case sensitivity in test_pathlib and test_ntpath.
- bpo-38088: Fixes distutils not finding vcruntime140.dll with only the v142 toolset installed.
- bpo-37283: Ensure command-line and unattend.xml setting override previously detected states in Windows installer.
- bpo-38030: Fixes `os.stat()` failing for block devices on Windows
- bpo-38020: Fixes potential crash when calling `os.readlink()` (or
indirectly through`realpath()` ) on a file that is not a
supported link.
- bpo-37705: Improve the implementation of `winerror_to_errno()` .
- bpo-37549: `os.dup()` no longer fails for standard streams on Windows
7.
- bpo-1311: The `nul` file on Windows now returns True from`exists()` and a valid result from`os.stat()` with`S_IFCHR` set.
- bpo-9949: Enable support for following symlinks in `os.realpath()` .
- bpo-37834: Treat all name surrogate reparse points on Windows in `os.lstat()` and other reparse points as regular files in`os.stat()` .
- bpo-36266: Add the module name in the formatted error message when DLL load fail happens during module import in `_PyImport_FindSharedFuncptrWindows()` . Patch by Srinivas Nyayapati.
- bpo-25172: Trying to import the `crypt` module on Windows will
result in an`ImportError` with a message explaining that the module
isnât supported on Windows. On other platforms, if the underlying`_crypt` module is not available, the ImportError will include a message
explaining the problem.
- bpo-37778: Fixes the icons used for file associations to the Microsoft Store package.
- bpo-37734: Fix use of registry values to launch Python from Microsoft Store app.
- bpo-37702: Fix memory leak on Windows in creating an SSLContext object or running `urllib.request.urlopen('https://...')` .
- bpo-37672: Switch Windows Store packageâs pip to use bundled `pip.ini` instead of`PIP_USER` variable.
- bpo-10945: Officially drop support for creating bdist_wininst installers on non-Windows systems.
- bpo-37445: Include the `FORMAT_MESSAGE_IGNORE_INSERTS` flag in`FormatMessageW()` calls.
- bpo-37369: Fixes path for `sys.executable` when running from the
Microsoft Store.
- bpo-37380: Donât collect unfinished processes with `subprocess._active` on Windows to cleanup later. Patch by Ruslan Kuprieiev.
- bpo-37351: Removes libpython38.a from standard Windows distribution.
- bpo-35360: Update Windows builds to use SQLite 3.28.0.
- bpo-37267: On Windows, `os.dup()` no longer creates an inheritable fd
when handling a character file.
- bpo-36779: Ensure `time.tzname` is correct on Windows when the active
code page is set to CP_UTF7 or CP_UTF8.
- bpo-32587: Make `winreg.REG_MULTI_SZ` support zero-length strings.
- bpo-28269: Replace use of `strcasecmp()` for the system function`_stricmp()` . Patch by Minmin Gong.
- bpo-36590: Add native Bluetooth RFCOMM support to socket module.

### macOSÂ¶

- bpo-38117: Updated OpenSSL to 1.1.1d in macOS installer.
- bpo-38089: Move Azure Pipelines to latest VM versions and make macOS tests optional
- bpo-18049: Increase the default stack size of threads from 5MB to 16MB on macOS, to match the stack size of the main thread. This avoids crashes on deep recursion in threads.
- bpo-34602: Avoid test suite failures on macOS by no longer calling resource.setrlimit to increase the process stack size limit at runtime. The runtime change is no longer needed since the interpreter is being built with a larger default stack size.
- bpo-35360: Update macOS installer to use SQLite 3.28.0.
- bpo-34631: Updated OpenSSL to 1.1.1c in macOS installer.

### IDLEÂ¶

- bpo-26353: Stop adding newline when saving an IDLE shell window.
- bpo-4630: Add an option to toggle IDLEâs cursor blink for shell, editor, and output windows. See Settings, General, Window Preferences, Cursor Blink. Patch by Zackery Spytz.
- bpo-38598: Do not try to compile IDLE shell or output windows
- bpo-36698: IDLE no longer fails when write non-encodable characters to stderr. It now escapes them with a backslash, as the regular Python interpreter. Added the `errors` field to the standard streams.
- bpo-35379: When exiting IDLE, catch any AttributeError. One happens when EditorWindow.close is called twice. Printing a traceback, when IDLE is run from a terminal, is useless and annoying.
- bpo-38183: To avoid problems, test_idle ignores the user config directory. It no longer tries to create or access .idlerc or any files within. Users must run IDLE to discover problems with saving settings.
- bpo-38077: IDLE no longer adds âargvâ to the user namespace when initializing it. This bug only affected 3.7.4 and 3.8.0b2 to 3.8.0b4.
- bpo-38041: Shell restart lines now fill the window width, always start with â=â, and avoid wrapping unnecessarily. The line will still wrap if the included file name is long relative to the width.
- bpo-35771: To avoid occasional spurious test_idle failures on slower machines, increase the `hover_delay` in test_tooltip.
- bpo-37824: Properly handle user input warnings in IDLE shell. Cease turning SyntaxWarnings into SyntaxErrors.
- bpo-37929: IDLE Settings dialog now closes properly when there is no shell window.
- bpo-37902: Add mousewheel scrolling for IDLE module, path, and stack browsers. Patch by George Zhang.
- bpo-37849: Fixed completions list appearing too high or low when shown above the current line.
- bpo-36419: Refactor IDLE autocomplete and improve testing.
- bpo-37748: Reorder the Run menu. Put the most common choice, Run Module, at the top.
- bpo-37692: Improve highlight config sample with example shell interaction and better labels for shell elements.
- bpo-37628: Settings dialog no longer expands with font size.
- bpo-37627: Initialize the Customize Run dialog with the command line arguments most recently entered before. The user can optionally edit before submitting them.
- bpo-33610: Fix code context not showing the correct context when first toggled on.
- bpo-37530: Optimize code context to reduce unneeded background activity. Font and highlight changes now occur along with text changes instead of after a random delay.
- bpo-27452: Cleanup `config.py` by inlining`RemoveFile` and
simplifying the handling of`file` in`CreateConfigHandlers` .
- bpo-37325: Fix tab focus traversal order for help source and custom run dialogs.
- bpo-37321: Both subprocess connection error messages now refer to the âStartup failureâ section of the IDLE doc.
- bpo-17535: Add optional line numbers for IDLE editor windows. Windows open without line numbers unless set otherwise in the General tab of the configuration dialog.
- bpo-26806: To compensate for stack frames added by IDLE and avoid possible problems with low recursion limits, add 30 to limits in the user code execution process. Subtract 30 when reporting recursion limits to make this addition mostly transparent.
- bpo-37177: Properly âattachâ search dialogs to their main window so that they behave like other dialogs and do not get hidden behind their main window.
- bpo-37039: Adjust âZoom Heightâ to individual screens by momentarily maximizing the window on first use with a particular screen. Changing screen settings may invalidate the saved height. While a window is maximized, âZoom Heightâ has no effect.
- bpo-35763: Make calltip reminder about â/â meaning positional-only less obtrusive by only adding it when there is room on the first line.
- bpo-5680: Add âRunâ¦ Customizedâ to the Run menu to run a module with customized settings. Any âcommand line argumentsâ entered are added to sys.argv. One can suppress the normal Shell main module restart.
- bpo-36390: Gather Format menu functions into format.py. Combine paragraph.py, rstrip.py, and format methods from editor.py.

### Tools/DemosÂ¶

- bpo-38118: Update Valgrind suppression file to ignore a false alarm in `PyUnicode_Decode()` when using GCC builtin strcmp().
- bpo-38347: pathfix.py: Assume all files that end on â.pyâ are Python scripts when working recursively.
- bpo-37803: pdbâs `--help` and`--version` long options now work.
- bpo-37942: Improve ArgumentClinic converter for floats.
- bpo-37704: Remove `Tools/scripts/h2py.py` : use cffi to access a C API in
Python.
- bpo-37675: 2to3 now works when run from a zipped standard library.
- bpo-37034: Argument Clinic now uses the argument name on errors with keyword-only argument instead of their position. Patch contributed by RÃ©mi Lapeyre.
- bpo-37064: Add option -k to pathscript.py script: preserve shebang flags. Add option -a to pathscript.py script: add flags.

### C APIÂ¶

- bpo-37633: Re-export some function compatibility wrappers for macros in `pythonrun.h` .
- bpo-38644: Provide `Py_EnterRecursiveCall()` and`Py_LeaveRecursiveCall()` as regular functions for the limited API.
Previously, there were defined as macros, but these macros didnât work
with the limited API which cannot access`PyThreadState.recursion_depth` field. Remove`_Py_CheckRecursionLimit` from the stable ABI.
- bpo-38650: The global variable `PyStructSequence_UnnamedField` is
now a constant and refers to a constant string.
- bpo-38540: Fixed possible leak in `PyArg_Parse()` and similar
functions for format units`"es#"` and`"et#"` when the macro`PY_SSIZE_T_CLEAN` is not defined.
- bpo-38395: Fix a crash in `weakref.proxy` objects due to incorrect
lifetime management when calling some associated methods that may delete
the last reference to object being referenced by the proxy. Patch by Pablo
Galindo.
- bpo-36389: The `_PyObject_CheckConsistency()` function is now also
available in release mode. For example, it can be used to debug a crash in
the`visit_decref()` function of the GC.
- bpo-38266: Revert the removal of PyThreadState_DeleteCurrent() with documentation.
- bpo-38303: Update audioop extension module to use the stable ABI (PEP-384). Patch by Tyler Kieft.
- bpo-38234: `Py_SetPath()` now sets`sys.executable` to the
program full path (`Py_GetProgramFullPath()` ) rather than to the
program name (`Py_GetProgramName()` ).
- bpo-38234: Python ignored arguments passed to `Py_SetPath()` ,`Py_SetPythonHome()` and`Py_SetProgramName()` : fix Python
initialization to use specified arguments.
- bpo-38205: The `Py_UNREACHABLE()` macro now calls`Py_FatalError()` .
- bpo-38140: Make dict and weakref offsets opaque for C heap types by passing the offsets through PyMemberDef
- bpo-15088: The C function `PyGen_NeedsFinalizing` has been removed. It
was not documented, tested or used anywhere within CPython after the
implementation of**PEP 442** . Patch by Joannah Nanjekye. (Patch by Joannah
Nanjekye)
- bpo-36763: Options added by `PySys_AddXOption()` are now handled the
same way than`PyConfig.xoptions` and command line`-X` options.
- bpo-37926: Fix a crash in `PySys_SetArgvEx(0, NULL, 0)` .
- bpo-37879: Fix subtype_dealloc to suppress the type decref when the base type is a C heap type
- bpo-37645: Add `_PyObject_FunctionStr()` to get a user-friendly
string representation of a function-like object. Patch by Jeroen Demeyer.
- bpo-29548: The functions `PyEval_CallObject` ,`PyEval_CallFunction` ,`PyEval_CallMethod` and`PyEval_CallObjectWithKeywords` are
deprecated. Use`PyObject_Call()` and its variants instead.
- bpo-37151: `PyCFunction_Call` is now a deprecated alias of`PyObject_Call()` .
- bpo-37540: The vectorcall protocol now requires that the caller passes only strings as keyword names.
- bpo-37207: The vectorcall protocol is now enabled for `type` objects:
set`tp_vectorcall` to a vectorcall function to be used instead of`tp_new` and`tp_init` when calling the class itself.
- bpo-21120: Exclude Python-ast.h, ast.h and asdl.h from the limited API.
- bpo-37483: Add new function `_PyObject_CallOneArg` for calling an object
with one positional argument.
- bpo-36763: Add `PyConfig_SetWideStringList()` function.
- bpo-37337: Add fast functions for calling methods: `_PyObject_VectorcallMethod()` ,`_PyObject_CallMethodNoArgs()` and`_PyObject_CallMethodOneArg()` .
- bpo-28805: The `METH_FASTCALL` calling convention has been
documented.
- bpo-37221: The new function `PyCode_NewWithPosOnlyArgs()` allows to
create code objects like`PyCode_New()` , but with an extra*posonlyargcount* parameter for indicating the number of positonal-only
arguments.
- bpo-37215: Fix dtrace issue introduce by bpo-36842
- bpo-37194: Add a new public `PyObject_CallNoArgs()` function to the
C API: call a callable Python object without any arguments. It is the most
efficient way to call a callback without any argument. On x86-64, for
example,`PyObject_CallFunctionObjArgs(func, NULL)` allocates 960 bytes
on the stack per call, whereas`PyObject_CallNoArgs(func)` only
allocates 624 bytes per call.
- bpo-37170: Fix the cast on error in `PyLong_AsUnsignedLongLongMask()` .
- bpo-35381: Convert posixmodule.c statically allocated types `DirEntryType` and`ScandirIteratorType` to heap-allocated types.
- bpo-34331: Use singular/plural noun in error message when instantiating an abstract class with non-overridden abstract method(s).

## Python 3.8.0 beta 1Â¶

*Release date: 2019-06-04*

### SecurityÂ¶

- bpo-35907: **CVE 2019-9948** : Avoid file reading by disallowing`local-file://` and`local_file://` URL schemes in`URLopener().open()` and`URLopener().retrieve()` of`urllib.request` .
- bpo-33529: Prevent fold function used in email header encoding from entering infinite loop when there are too many non-ASCII characters in a header.
- bpo-33164: Updated blake2 implementation which uses secure memset implementation provided by platform.

### Core and BuiltinsÂ¶

- bpo-35814: Allow unpacking in the right hand side of annotated assignments. In particular, `t: Tuple[int, ...] = x, y, *z` is now
allowed.
- bpo-37126: All structseq objects are now tracked by the garbage collector. Patch by Pablo Galindo.
- bpo-37122: Make the *co_argcount* attribute of code objects represent the
total number of positional arguments (including positional-only
arguments). The value of*co_posonlyargcount* can be used to distinguish
which arguments are positional only, and the difference (*co_argcount* -*co_posonlyargcount* ) is the number of positional-or-keyword arguments.
Patch by Pablo Galindo.
- bpo-20092: Constructors of `int` ,`float` and`complex` will now use the`__index__()` special
method, if available and the corresponding method`__int__()` ,`__float__()` or`__complex__()` is not available.
- bpo-37087: Add native thread ID (TID) support to OpenBSD.
- bpo-26219: Implemented per opcode cache mechanism and `LOAD_GLOBAL` instruction use it.`LOAD_GLOBAL` is now about 40% faster. Contributed
by Yury Selivanov, and Inada Naoki.
- bpo-37072: Fix crash in PyAST_FromNodeObject() when flags is NULL.
- bpo-37029: Freeing a great many small objects could take time quadratic in the number of arenas, due to using linear search to keep `obmalloc.c` âs
list of usable arenas sorted by order of number of free memory pools.
This is accomplished without search now, leaving the worst-case time
linear in the number of arenas.  For programs where this quite visibly
matters (typically with more than 100 thousand small objects alive
simultaneously), this can greatly reduce the time needed to release their
memory.
- bpo-26423: Fix possible overflow in `wrap_lenfunc()` when```
sizeof(long)
< sizeof(Py_ssize_t)
```
(e.g., 64-bit Windows).
- bpo-37050: Improve the AST for âdebugâ f-strings, which use â=â to print out the source of the expression being evaluated. Delete expr_text from the FormattedValue node, and instead use a Constant string node (possibly merged with adjacent constant expressions inside the f-string).
- bpo-22385: The `bytes.hex` ,`bytearray.hex` , and`memoryview.hex` methods as well as the`binascii.hexlify` and`b2a_hex` functions now
have the ability to include an optional separator between hex bytes.  This
functionality was inspired by MicroPythonâs hexlify implementation.
- bpo-26836: Add `os.memfd_create()` .
- bpo-37032: Added new `replace()` method to the code type
(`types.CodeType` ).
- bpo-37007: Implement `socket.if_nameindex()` ,`socket.if_nametoindex()` , and`socket.if_indextoname()` on
Windows.
- bpo-36829: `PyErr_WriteUnraisable()` now creates a traceback object
if there is no current traceback. Moreover, call`PyErr_NormalizeException()` and`PyException_SetTraceback()` to normalize the exception value. Ignore any error.
- bpo-36878: Only accept text after `# type: ignore` if the first
character is ASCII. This is to disallow things like`# type: ignoreÃ©` .
- bpo-36878: Store text appearing after a `# type: ignore` comment in the
AST. For example a type ignore like`# type: ignore[E1000]` will have
the string`"[E1000]"` stored in its AST node.
- bpo-2180: Treat line continuation at EOF as a `SyntaxError` by Anthony
Sottile.
- bpo-36907: Fix a crash when calling a C function with a keyword dict ( `f(**kwargs)` ) and changing the dict`kwargs` while that function is
running.
- bpo-36946: Fix possible signed integer overflow when handling slices.
- bpo-36826: Add NamedExpression kind support to ast_unparse.c
- bpo-1875: A `SyntaxError` is now raised if a code blocks that will be
optimized away (e.g. if conditions that are always false) contains syntax
errors. Patch by Pablo Galindo.
- bpo-36027: Allow computation of modular inverses via three-argument `pow` : the second argument is now permitted to be negative in the case
where the first and third arguments are relatively prime.
- bpo-36861: Update the Unicode database to version 12.1.0.
- bpo-28866: Avoid caching attributes of classes which type defines mro() to avoid a hard cache invalidation problem.
- bpo-36851: The `FrameType` stack is now correctly cleaned up if the
execution ends with a return and the stack is not empty.
- bpo-34616: The `compile()` builtin functions now support the`ast.PyCF_ALLOW_TOP_LEVEL_AWAIT` flag,  which allow to compile sources
that  contains top-level`await` ,`async with` or`async for` . This
is useful to evaluate async-code from with an already async functions; for
example in a custom REPL.
- bpo-36842: Implement PEP 578, adding sys.audit, io.open_code and related APIs.
- bpo-27639: Correct return type for UserList slicing operations. Patch by Michael Blahay, Erick Cervantes, and vaultah
- bpo-36737: Move PyRuntimeState.warnings into per-interpreter state (via âmodule stateâ).
- bpo-36793: Removed `__str__` implementations from builtin types`bool` ,`int` ,`float` ,`complex` and few
classes from the standard library. They now inherit`__str__()` from`object` .
- bpo-36817: Add a `=` feature f-strings for debugging. This can precede`!s` ,`!r` , or`!a` . It produces the text of the expression,
followed by an equal sign, followed by the repr of the value of the
expression. So`f'{3*9+15=}'` would be equal to the string`'3*9+15=42'` .  If`=` is specified, the default conversion is set to`!r` , unless a format spec is given, in which case the formatting
behavior is unchanged, and __format__ will be used.
- bpo-24048: Save the live exception during import.câs `remove_module()` .
- bpo-27987: pymalloc returns memory blocks aligned by 16 bytes, instead of 8 bytes, on 64-bit platforms to conform x86-64 ABI. Recent compilers assume this alignment more often. Patch by Inada Naoki.
- bpo-36601: A long-since-meaningless check for `getpid() == main_pid` was
removed from Pythonâs internal C signal handler.
- bpo-36594: Fix incorrect use of `%p` in format strings. Patch by Zackery
Spytz.
- bpo-36045: `builtins.help()` now prefixes`async` for async functions.
- bpo-36084: Add native thread ID (TID) to threading.Thread objects (supported platforms: Windows, FreeBSD, Linux, macOS)
- bpo-36035: Added fix for broken symlinks in combination with pathlib
- bpo-35983: Added new trashcan macros to deal with a double deallocation that could occur when the `tp_dealloc` of a subclass calls the`tp_dealloc` of a base class and that base class uses the trashcan
mechanism. Patch by Jeroen Demeyer.
- bpo-20602: Do not clear `sys.flags` and`sys.float_info` during shutdown. Patch by Zackery Spytz.
- bpo-26826: Expose `copy_file_range()` as a low level API in the`os` module.
- bpo-32388: Remove cross-version binary compatibility requirement in tp_flags.
- bpo-31862: Port binascii to PEP 489 multiphase initialization. Patch by Marcel Plch.

### LibraryÂ¶

- bpo-37128: Added `math.perm()` .
- bpo-37120: Add SSLContext.num_tickets to control the number of TLSv1.3 session tickets.
- bpo-12202: Fix the error handling in `msilib.SummaryInformation.GetProperty()` . Patch by Zackery Spytz.
- bpo-26835: The fcntl module now contains file sealing constants for sealing of memfds.
- bpo-29262: Add `get_origin()` and`get_args()` introspection helpers
to`typing` module.
- bpo-12639: `msilib.Directory.start_component()` no longer fails if*keyfile* is not`None` .
- bpo-36999: Add the `asyncio.Task.get_coro()` method to publicly expose
the tasksâs coroutine object.
- bpo-35246: Make `asyncio.create_subprocess_exec()` accept path-like
arguments.
- bpo-35279: Change default *max_workers* of`ThreadPoolExecutor` from`cpu_count() * 5` to`min(32, cpu_count() + 4)` .  Previous value was
unreasonably large on many cores machines.
- bpo-37076: `_thread.start_new_thread()` now logs uncaught exception
raised by the function using`sys.unraisablehook()` , rather than`sys.excepthook()` , so the hook gets access to the function which
raised the exception.
- bpo-33725: On macOS, the `multiprocessing` module now uses*spawn* start method by default.
- bpo-37054: Fix destructor `_pyio.BytesIO` and`_pyio.TextIOWrapper` : initialize their`_buffer` attribute as
soon as possible (in the class body), because itâs used by`__del__()` which calls`close()` .
- bpo-37058: PEP 544: Add `Protocol` and`@runtime_checkable` to the`typing` module.
- bpo-36933: The functions `sys.set_coroutine_wrapper` and`sys.get_coroutine_wrapper` that were deprecated and marked for removal
in 3.8 have been removed.
- bpo-37047: Handle late binding and attribute access in `unittest.mock.AsyncMock` setup for autospeccing. Document newly
implemented async methods in`unittest.mock.MagicMock` .
- bpo-37049: PEP 589: Add `TypedDict` to the`typing` module.
- bpo-37046: PEP 586: Add `Literal` to the`typing` module.
- bpo-37045: PEP 591: Add `Final` qualifier and`@final` decorator to
the`typing` module.
- bpo-37035: Donât log OSError based exceptions if a fatal error has occurred in asyncio transport. Peer can generate almost any OSError, user cannot avoid these exceptions by fixing own code. Errors are still propagated to user code, itâs just logging them is pointless and pollute asyncio logs.
- bpo-37001: `symtable.symtable()` now accepts the same input types for
source code as the built-in`compile()` function. Patch by Dino
Viehland.
- bpo-37028: Implement asyncio REPL
- bpo-37027: Return safe to use proxy socket object from transport.get_extra_info(âsocketâ)
- bpo-32528: Make asyncio.CancelledError a BaseException. This will address the common mistake many asyncio users make: an âexcept Exceptionâ clause breaking Tasks cancellation. In addition to this change, we stop inheriting asyncio.TimeoutError and asyncio.InvalidStateError from their concurrent.futures.* counterparts. Thereâs no point for these exceptions to share the inheritance chain.
- bpo-1230540: Add a new `threading.excepthook()` function which handles
uncaught`threading.Thread.run()` exception. It can be overridden to
control how uncaught`threading.Thread.run()` exceptions are handled.
- bpo-36996: Handle `unittest.mock.patch()` used as a decorator on async
functions.
- bpo-37008: Add support for calling `next()` with the mock resulting
from`unittest.mock.mock_open()`
- bpo-27737: Allow whitespace only header encoding in `email.header` - by
Batuhan Taskaya
- bpo-36969: PDB command `args` now  display positional only arguments.
Patch contributed by RÃ©mi Lapeyre.
- bpo-36969: PDB command `args` now  display keyword only arguments. Patch
contributed by RÃ©mi Lapeyre.
- bpo-36983: Add missing names to `typing.__all__` :`ChainMap` ,`ForwardRef` ,`OrderedDict` - by Anthony Sottile.
- bpo-36972: Add SupportsIndex protocol to the typing module to allow type checking to detect classes that can be passed to `hex()` ,`oct()` and`bin()` .
- bpo-32972: Implement `unittest.IsolatedAsyncioTestCase` to help testing
asyncio-based code.
- bpo-36952: `fileinput.input()` and`fileinput.FileInput`**bufsize** argument has been removed (was deprecated and ignored since
Python 3.6), and as a result the**mode** and**openhook** arguments have
been made keyword-only.
- bpo-36952: Starting with Python 3.3, importing ABCs from `collections` is deprecated, and import should be done from`collections.abc` . Still being able to import from`collections` was marked for removal in 3.8, but has been delayed to 3.9; documentation
and`DeprecationWarning` clarified.
- bpo-36949: Implement __repr__ for WeakSet objects.
- bpo-36948: Fix `NameError` in`urllib.request.URLopener.retrieve()` . Patch by Karthikeyan
Singaravelan.
- bpo-33524: Fix the folding of email header when the max_line_length is 0 or None and the header contains non-ascii characters. Contributed by Licht Takeuchi (@Licht-T).
- bpo-24564: `shutil.copystat()` now ignores`errno.EINVAL` on`os.setxattr()` which may occur when copying files on filesystems
without extended attributes support.Original patch by Giampaolo Rodola, updated by Ying Wang.
- bpo-36888: Python child processes can now access the status of their parent process using multiprocessing.process.parent_process
- bpo-36921: Deprecate `@coroutine` for sake of`async def` .
- bpo-25652: Fix bug in `__rmod__` of`UserString` - by Batuhan Taskaya.
- bpo-36916: Remove a message about an unhandled exception in a task when writer.write() is used without await and writer.drain() fails with an exception.
- bpo-36889: Introduce `asyncio.Stream` class that merges`asyncio.StreamReader` and`asyncio.StreamWriter` functionality.`asyncio.Stream` can work in readonly, writeonly and
readwrite modes. Provide`asyncio.connect()` ,`asyncio.connect_unix()` ,`asyncio.connect_read_pipe()` and`asyncio.connect_write_pipe()` factories to open`asyncio.Stream` connections. Provide`asyncio.StreamServer` and`UnixStreamServer` to serve servers with asyncio.Stream API.
Modify`asyncio.create_subprocess_shell()` and`asyncio.create_subprocess_exec()` to use`asyncio.Stream` instead of deprecated`StreamReader` and`StreamWriter` .
Deprecate`asyncio.StreamReader` and`asyncio.StreamWriter` .
Deprecate usage of private classes, e.g.`asyncio.FlowControlMixing` and`asyncio.StreamReaderProtocol` outside of asyncio package.
- bpo-36845: Added validation of integer prefixes to the construction of IP networks and interfaces in the ipaddress module.
- bpo-23378: Add an extend action to argparser.
- bpo-36867: Fix a bug making a SharedMemoryManager instance and its parent process use two separate resource_tracker processes.
- bpo-23896: Adds a grammar to lib2to3.pygram that contains exec as a function not as statement.
- bpo-36895: The function `time.clock()` was deprecated in 3.3 in favor of`time.perf_counter()` and marked for removal in 3.8, it has removed.
- bpo-35545: Fix asyncio discarding IPv6 scopes when ensuring hostname resolutions internally
- bpo-36887: Add new function `math.isqrt()` to compute integer square
roots.
- bpo-34632: Introduce the `importlib.metadata` module with (provisional)
support for reading metadata from third-party packages.
- bpo-36878: When using `type_comments=True` in`ast.parse` , treat```
#
type: ignore
```
followed by a non-alphanumeric character and then arbitrary
text as a type ignore, instead of requiring nothing but whitespace or
another comment. This is to permit formations such as```
# type:
ignore[E1000]
```
.
- bpo-36778: `cp65001` encoding (Windows code page 65001) becomes an alias
to`utf_8` encoding.
- bpo-36867: The multiprocessing.resource_tracker replaces the multiprocessing.semaphore_tracker module. Other than semaphores, resource_tracker also tracks shared_memory segments.
- bpo-30262: The `Cache` and`Statement` objects of the`sqlite3` module are not exposed to the user.  Patch by Aviv Palivoda.
- bpo-24538: In `shutil.copystat()` , first copy extended file attributes
and then file permissions, since extended attributes can only be set on
the destination while it is still writeable.
- bpo-36829: Add new `sys.unraisablehook()` function which can be
overridden to control how âunraisable exceptionsâ are handled. It is
called when an exception has occurred but there is no way for Python to
handle it. For example, when a destructor raises an exception or during
garbage collection (`gc.collect()` ).
- bpo-36832: Introducing `zipfile.Path` , a pathlib-compatible wrapper for
traversing zip files.
- bpo-36814: Fix an issue where os.posix_spawnp() would incorrectly raise a TypeError when file_actions is None.
- bpo-33110: Handle exceptions raised by functions added by concurrent.futures add_done_callback correctly when the Future has already completed.
- bpo-26903: Limit `max_workers` in`ProcessPoolExecutor` to 61 to work
around a WaitForMultipleObjects limitation.
- bpo-36813: Fix `QueueListener` to call`queue.task_done()` upon stopping. Patch by Bar Harel.
- bpo-36806: Forbid creation of asyncio stream objects like StreamReader, StreamWriter, Process, and their protocols outside of asyncio package.
- bpo-36802: Provide both sync and async calls for StreamWriter.write() and StreamWriter.close()
- bpo-36801: Properly handle SSL connection closing in asyncio StreamWriter.drain() call.
- bpo-36785: Implement PEP 574 (pickle protocol 5 with out-of-band buffers).
- bpo-36772: functools.lru_cache() can now be used as a straight decorator in addition to its existing usage as a function that returns a decorator.
- bpo-6584: Add a `BadGzipFile` exception to the`gzip` module.
- bpo-36748: Optimized write buffering in C implementation of `TextIOWrapper` . Writing ASCII string to`TextIOWrapper` with ascii,
latin1, or utf-8 encoding is about 20% faster.  Patch by Inada Naoki.
- bpo-8138: Donât mark `wsgiref.simple_server.SimpleServer` as
multi-threaded since`wsgiref.simple_server.WSGIServer` is
single-threaded.
- bpo-22640: `py_compile.compile()` now supports silent mode. Patch by
Joannah Nanjekye
- bpo-29183: Fix double exceptions in `wsgiref.handlers.BaseHandler` by calling its`close()` method only
when no exception is raised.
- bpo-36548: Improved the repr of regular expression flags.
- bpo-36542: The signature of Python functions can now be overridden by specifying the `__text_signature__` attribute.
- bpo-36533: Reinitialize logging.Handler locks in forked child processes instead of attempting to acquire them all in the parent before forking only to be released in the child process. The acquire/release pattern was leading to deadlocks in code that has implemented any form of chained logging handlers that depend upon one another as the lock acquisition order cannot be guaranteed.
- bpo-35252: Throw a TypeError instead of an AssertionError when using an invalid type annotation with singledispatch.
- bpo-35900: Allow reduction methods to return a 6-item tuple where the 6th item specifies a custom state-setting method thatâs called instead of the regular `__setstate__` method.
- bpo-35900: enable custom reduction callback registration for functions and classes in _pickle.c, using the new Picklerâs attribute `reducer_override`
- bpo-36368: Fix a bug crashing SharedMemoryManager instances in interactive sessions after a ctrl-c (KeyboardInterrupt) was sent
- bpo-31904: Fix mmap fail for VxWorks
- bpo-27497: `csv.DictWriter.writeheader()` now returns the return value
of the underlying`csv.Writer.writerow()` method. Patch contributed by
Ashish Nitin Patil.
- bpo-36239: Parsing .mo files now ignores comments starting and ending with #-#-#-#-#.
- bpo-26707: Enable plistlib to read and write binary plist files that were created as a KeyedArchive file. Specifically, this allows the plistlib to process 0x80 tokens as UID objects.
- bpo-31904: Add posix module support for VxWorks.
- bpo-35125: Asyncio: Remove inner callback on outer cancellation in shield
- bpo-35721: Fix `asyncio.SelectorEventLoop.subprocess_exec()` leaks
file descriptors if`Popen` fails and called with`stdin=subprocess.PIPE` . Patch by Niklas Fiekas.
- bpo-31855: `unittest.mock.mock_open()` results now respects the
argument of read([size]). Patch contributed by RÃ©mi Lapeyre.
- bpo-35431: Implement `math.comb()` that returns binomial coefficient,
that computes the number of ways to choose k items from n items without
repetition and without order. Patch by Yash Aggarwal and Keller Fuchs.
- bpo-26660: Fixed permission errors in `TemporaryDirectory` clean up. Previously`TemporaryDirectory.cleanup()` failed when non-writeable or
non-searchable files or directories were created inside a temporary
directory.
- bpo-34271: Add debugging helpers to ssl module. Itâs now possible to dump key material and to trace TLS protocol. The default and stdlib contexts also support SSLKEYLOGFILE env var.
- bpo-26467: Added AsyncMock to support using unittest to mock asyncio coroutines. Patch by Lisa Roach.
- bpo-33569: dataclasses.InitVar: Exposes the type used to create the init var.
- bpo-34424: Fix serialization of messages containing encoded strings when the policy.linesep is set to a multi-character string. Patch by Jens Troeger.
- bpo-34303: Performance of `functools.reduce()` is slightly improved.
Patch by Sergey Fedoseev.
- bpo-33361: Fix a bug in `codecs.StreamRecoder` where seeking might
leave old data in a buffer and break subsequent read calls. Patch by Ammar
Askar.
- bpo-22454: The `shlex` module now exposes`shlex.join()` , the
inverse of`shlex.split()` . Patch by Bo Bayles.
- bpo-31922: `asyncio.AbstractEventLoop.create_datagram_endpoint()` : Do
not connect UDP socket when broadcast is allowed. This allows to receive
replies after a UDP broadcast.
- bpo-24882: Change ThreadPoolExecutor to use existing idle threads before spinning up new ones.
- bpo-31961: Added support for bytes and path-like objects in `subprocess.Popen()` on Windows.  The*args* parameter now accepts a
path-like object if*shell* is`False` and a sequence containing
bytes and path-like objects. The*executable* parameter now accepts a
bytes and path-like object. The*cwd* parameter now accepts a
bytes object. Based on patch by Anders Lorentsen.
- bpo-33123: `pathlib.Path.unlink` now accepts a*missing_ok* parameter to avoid a`FileNotFoundError` from being raised. Patch by
Robert Buchholz.
- bpo-32941: Allow `mmap.mmap` objects to access the madvise() system
call (through`mmap.mmap.madvise()` ).
- bpo-22102: Added support for ZIP files with disks set to 0. Such files are commonly created by builtin tools on Windows when use ZIP64 extension. Patch by Francisco Facioni.
- bpo-32515: trace.py can now run modules via python3 -m trace -t âmodule module_name
- bpo-32299: Changed `unittest.mock.patch.dict()` to return the patched
dictionary when used as context manager. Patch by Vadim Tsander.
- bpo-27141: Added a `__copy__()` to`collections.UserList` and`collections.UserDict` in order to correctly implement shallow copying
of the objects. Patch by Bar Harel.
- bpo-31829: `\r` ,`\0` and`\x1a` (end-of-file on Windows) are now
escaped in protocol 0 pickles of Unicode strings. This allows to load them
without loss from files open in text mode in Python 2.
- bpo-23395: `_thread.interrupt_main()` now avoids setting the Python
error status if the`SIGINT` signal is ignored or not handled by Python.

### DocumentationÂ¶

- bpo-36896: Clarify that some types have unstable constructor signature between Python versions.
- bpo-36686: Improve documentation of the stdin, stdout, and stderr arguments of the `asyncio.subprocess_exec` function to specify which
values are supported. Also mention that decoding as text is not supported.Add a few tests to verify that the various values passed to the std* arguments actually work.
- bpo-36984: Improve version added references in `typing` module - by
Anthony Sottile.
- bpo-36868: Whatâs new now mentions SSLContext.hostname_checks_common_name instead of SSLContext.host_flags.
- bpo-35924: Add a note to the `curses.addstr()` documentation to warn
that multiline strings can cause segfaults because of an ncurses bug.
- bpo-36783: Added C API Documentation for Time_FromTimeAndFold and PyDateTime_FromDateAndTimeAndFold as per PEP 495. Patch by Edison Abahurire.
- bpo-36797: More of the legacy distutils documentation has been either pruned, or else more clearly marked as being retained solely until the setuptools documentation covers it independently.
- bpo-22865: Add detail to the documentation on the `pty.spawn` function.
- bpo-35397: Remove deprecation and document urllib.parse.unwrap(). Patch contributed by RÃ©mi Lapeyre.
- bpo-32995: Added the context variable in glossary.
- bpo-33519: Clarify that `copy()` is not part of the`MutableSequence` ABC.
- bpo-33482: Make `codecs.StreamRecoder.writelines` take a list of bytes.
- bpo-25735: Added documentation for func factorial to indicate that returns integer values
- bpo-20285: Expand object.__doc__ (docstring) to make it clearer. Modify pydoc.py so that help(object) lists object methods (for other classes, help omits methods of the object base class.)

### TestsÂ¶

- bpo-37069: Modify test_coroutines, test_cprofile, test_generators, test_raise, test_ssl and test_yield_from to use `test.support.catch_unraisable_exception()` rather than`test.support.captured_stderr()` .
- bpo-37098: Fix test_memfd_create on older Linux Kernels.
- bpo-37081: Test with OpenSSL 1.1.1c
- bpo-36829: Add `test.support.catch_unraisable_exception()` : context
manager catching unraisable exception using`sys.unraisablehook()` .
- bpo-36915: The main regrtest process now always removes all temporary directories of worker processes even if they crash or if they are killed on KeyboardInterrupt (CTRL+c).
- bpo-36719: âpython3 -m test -jN â¦â now continues the execution of next tests when a worker process crash (CHILD_ERROR state). Previously, the test suite stopped immediately. Use âfailfast to stop at the first error.
- bpo-36816: Update Lib/test/selfsigned_pythontestdotnet.pem to match self-signed.pythontest.netâs new TLS certificate.
- bpo-35925: Skip httplib and nntplib networking tests when they would otherwise fail due to a modern OS or distro with a default OpenSSL policy of rejecting connections to servers with weak certificates.
- bpo-36782: Add tests for several C API functions in the `datetime` module. Patch by Edison Abahurire.
- bpo-36342: Fix test_multiprocessing in test_venv if platform lacks functioning sem_open.

### BuildÂ¶

- bpo-36721: To embed Python into an application, a new `--embed` option
must be passed to`python3-config --libs --embed` to get`-lpython3.8` (link the application to libpython). To support both 3.8 and older, try`python3-config --libs --embed` first and fallback to```
python3-config
--libs
```
(without`--embed` ) if the previous command fails.Add a pkg-config `python-3.8-embed` module to embed Python into an
application:`pkg-config python-3.8-embed --libs` includes`-lpython3.8` . To support both 3.8 and older, try```
pkg-config
python-X.Y-embed --libs
```
first and fallback to```
pkg-config python-X.Y
--libs
```
(without`--embed` ) if the previous command fails (replace`X.Y` with the Python version).On the other hand, `pkg-config python3.8 --libs` no longer contains`-lpython3.8` . C extensions must not be linked to libpython (except on
Android, case handled by the script); this change is backward incompatible
on purpose.
- bpo-36786: âmake installâ now runs compileall in parallel.

### WindowsÂ¶

- bpo-36965: include of STATUS_CONTROL_C_EXIT without depending on MSC compiler
- bpo-35926: Update to OpenSSL 1.1.1b for Windows.
- bpo-29883: Add Windows support for UDP transports for the Proactor Event Loop. Patch by Adam Meily.
- bpo-33407: The `Py_DEPRECATED()` macro has been implemented for
MSVC.

### macOSÂ¶

- bpo-36231: Support building Python on macOS without /usr/include installed. As of macOS 10.14, system header files are only available within an SDK provided by either the Command Line Tools or the Xcode app.

### IDLEÂ¶

- bpo-35610: Replace now redundant .context_use_ps1 with .prompt_last_line. This finishes change started in bpo-31858.
- bpo-37038: Make idlelib.run runnable; add test clause.
- bpo-36958: Print any argument other than None or int passed to SystemExit or sys.exit().
- bpo-36807: When saving a file, call os.fsync() so bits are flushed to e.g. USB drive.
- bpo-32411: In browser.py, remove extraneous sorting by line number since dictionary was created in line number order.

### Tools/DemosÂ¶

- bpo-37053: Handle strings like uâbarâ correctly in Tools/parser/unparse.py. Patch by Chih-Hsuan Yen.

### C APIÂ¶

- bpo-36763: Implement the **PEP 587** âPython Initialization Configurationâ.
- bpo-36379: Fix crashes when attempting to use the *modulo* parameter when`__ipow__` is implemented in C.
- bpo-37107: Update `PyObject_CallMethodObjArgs()` and`_PyObject_CallMethodIdObjArgs` to use`_PyObject_GetMethod` to avoid
creating a bound method object in many cases. Patch by Michael J.
Sullivan.
- bpo-36974: Implement **PEP 590** : Vectorcall: a fast calling protocol for
CPython. This is a new protocol to optimize calls of custom callable
objects.
- bpo-36763: `Py_Main()` now returns the exitcode rather than calling`Py_Exit(exitcode)` when calling`PyErr_Print()` if the current
exception type is`SystemExit` .
- bpo-36922: Add new type flag `Py_TPFLAGS_METHOD_DESCRIPTOR` for objects
behaving like unbound methods. These are objects supporting the
optimization given by the`LOAD_METHOD` /`CALL_METHOD` opcodes. See PEP
590.
- bpo-36728: The `PyEval_ReInitThreads()` function has been removed
from the C API. It should not be called explicitly: use`PyOS_AfterFork_Child()` instead.

## Python 3.8.0 alpha 4Â¶

*Release date: 2019-05-06*

### SecurityÂ¶

- bpo-36742: Fixes mishandling of pre-normalization characters in urlsplit().
- bpo-30458: Address **CVE 2019-9740** by disallowing URL paths with embedded
whitespace or control characters through into the underlying http client
request.  Such potentially malicious header injection URLs now cause an
http.client.InvalidURL exception to be raised.
- bpo-35755: `shutil.which()` now uses`os.confstr("CS_PATH")` if
available and if the`PATH` environment variable is not set.
Remove also the current directory from`posixpath.defpath` . On Unix,`shutil.which()` and the`subprocess` module no longer search the
executable in the current directory if the`PATH` environment
variable is not set.

### Core and BuiltinsÂ¶

- bpo-36722: In debug build, import now also looks for C extensions compiled in release mode and for C extensions compiled in the stable ABI.
- bpo-32849: Fix Python Initialization code on FreeBSD to detect properly when stdin file descriptor (fd 0) is invalid.
- bpo-36623: Remove parser headers and related function declarations that lack implementations after the removal of pgen.
- bpo-20180: `dict.pop()` is now up to 33% faster thanks to Argument
Clinic. Patch by Inada Naoki.
- bpo-36611: Debug memory allocators: disable serialno field by default from debug hooks on Python memory allocators to reduce the memory footprint by 5%. Enable `tracemalloc` to get the traceback where a memory block
has been allocated when a fatal memory error is logged to decide where to
put a breakpoint. Compile Python with`PYMEM_DEBUG_SERIALNO` defined to
get back the field.
- bpo-36588: On AIX, `sys.platform` doesnât contain the major version
anymore. Always return`'aix'` , instead of`'aix3'` ..`'aix7'` .
Since older Python versions include the version number, it is recommended
to always use`sys.platform.startswith('aix')` . Contributed by M. Felt.
- bpo-36549: Change str.capitalize to use titlecase for the first character instead of uppercase.
- bpo-36540: Implement **PEP 570** (Python positional-only parameters). Patch
by Pablo Galindo.
- bpo-36475: `PyEval_AcquireLock()` and`PyEval_AcquireThread()` now terminate the current thread if called
while the interpreter is finalizing, making them consistent with`PyEval_RestoreThread()` ,`Py_END_ALLOW_THREADS()` , and`PyGILState_Ensure()` .
- bpo-36504: Fix signed integer overflow in _ctypes.câs `PyCArrayType_new()` .
- bpo-20844: Fix running script with encoding cookie and LF line ending may fail on Windows.
- bpo-24214: Fixed support of the surrogatepass error handler in the UTF-8 incremental decoder.
- bpo-36452: Changing `dict` keys during iteration of the dict itself,`keys()` ,`values()` , or`items()` will now be detected in certain
corner cases where keys are deleted/added so that the number of keys isnât
changed. A`RuntimeError` will be raised after`len(dict)` iterations.
Contributed by Thomas Perl.
- bpo-36459: Fix a possible double `PyMem_FREE()` due to tokenizer.câs`tok_nextc()` .
- bpo-36433: Fixed TypeError message in classmethoddescr_call.
- bpo-36430: Fix a possible reference leak in `itertools.count()` .
- bpo-36440: Include node names in `ParserError` messages, instead of
numeric IDs. Patch by A. Skrobov.
- bpo-36143: Regenerate `keyword` from the Grammar and Tokens file
using pgen. Patch by Pablo Galindo.
- bpo-18372: Add missing `PyObject_GC_Track()` calls in the`pickle` module. Patch by Zackery Spytz.

### LibraryÂ¶

- bpo-35952: Fix pythoninfo when the compiler is missing.
- bpo-28238: The `.find*()` methods of xml.etree.ElementTree can now
search for wildcards like`{*}tag` and`{ns}*` that match a tag in any
namespace or all tags in a namespace.  Patch by Stefan Behnel.
- bpo-26978: `pathlib.path.link_to()` is now implemented. It creates a
hard link pointing to a path.
- bpo-1613500: `fileinput.FileInput` now uses the input file mode to
correctly set the output file mode (previously it was hardcoded to`'w'` )  when`inplace=True` is passed to its constructor.
- bpo-36734: Fix compilation of `faulthandler.c` on HP-UX. Initialize`stack_t current_stack` to zero using`memset()` .
- bpo-13611: The xml.etree.ElementTree packages gained support for C14N 2.0 serialisation. Patch by Stefan Behnel.
- bpo-36669: Add missing matrix multiplication operator support to weakref.proxy.
- bpo-36676: The XMLParser() in xml.etree.ElementTree provides namespace prefix context to the parser target if it defines the callback methods âstart_ns()â and/or âend_ns()â. Patch by Stefan Behnel.
- bpo-36673: The TreeBuilder and XMLPullParser in xml.etree.ElementTree gained support for parsing comments and processing instructions. Patch by Stefan Behnel.
- bpo-36650: The C version of functools.lru_cache() was treating calls with an empty `**kwargs` dictionary as being distinct from calls with no
keywords at all. This did not result in an incorrect answer, but it did
trigger an unexpected cache miss.
- bpo-28552: Fix `distutils.sysconfig` if`sys.executable` is`None` or an empty string: use`os.getcwd()` to initialize`project_base` .  Fix also the distutils build command: donât use`sys.executable` if it is`None` or an empty string.
- bpo-35755: `shutil.which()` and`distutils.spawn.find_executable` now use`os.confstr("CS_PATH")` if available instead of`os.defpath` , if the`PATH` environment variable is not set.
Moreover, donât use`os.confstr("CS_PATH")` nor`os.defpath` if
the`PATH` environment variable is set to an empty string.
- bpo-25430: improve performance of `IPNetwork.__contains__()`
- bpo-30485: Path expressions in xml.etree.ElementTree can now avoid explicit namespace prefixes for tags (or the â{namespace}tagâ notation) by passing a default namespace with an empty string prefix.
- bpo-36613: Fix `asyncio` wait() not removing callback if exception
- bpo-36598: Fix `isinstance` check for Mock objects with spec when the
code is executed under tracing. Patch by Karthikeyan Singaravelan.
- bpo-18748: In development mode ( `-X``dev` ) and in debug build,
the`io.IOBase` destructor now logs`close()` exceptions. These
exceptions are silent by default in release mode.
- bpo-36575: The `_lsprof` module now uses internal timer same to`time.perf_counter()` by default.`gettimeofday(2)` was used on Unix.
New timer has better resolution on most Unix platforms and timings are no
longer impacted by system clock updates since`perf_counter()` is
monotonic. Patch by Inada Naoki.
- bpo-33461: `json.loads` now emits`DeprecationWarning` when`encoding` option is specified.  Patch by Matthias Bussonnier.
- bpo-36559: The random module now prefers the lean internal _sha512 module over hashlib for seed(version=2) to optimize import time.
- bpo-17561: Set backlog=None as the default for socket.create_server.
- bpo-34373: Fix `time.mktime()` error handling on AIX for year before
1970.
- bpo-36232: Improve error message when trying to open existing DBM database that actually doesnât exist. Patch by Marco Rougeth.
- bpo-36546: Add statistics.quantiles()
- bpo-36050: Optimized `http.client.HTTPResponse.read()` for large
response. Patch by Inada Naoki.
- bpo-36522: If *debuglevel* is set to >0 in`http.client` , print all
values for headers with multiple values for the same header name. Patch by
Matt Houglum.
- bpo-36492: Deprecated passing required arguments like *func* as keyword
arguments in functions which should accept arbitrary keyword arguments and
pass them to other function. Arbitrary keyword arguments (even with names
âselfâ and âfuncâ) can now be passed to these functions if the required
arguments are passed as positional arguments.
- bpo-27181: Add statistics.geometric_mean().
- bpo-30427: `os.path.normcase()` relies on`os.fspath()` to check the
type of its argument. Redundant checks have been removed from its`posixpath.normcase()` and`ntpath.normcase()` implementations. Patch
by Wolfgang Maier.
- bpo-36385: Stop rejecting IPv4 octets for being ambiguously octal. Leading zeros are ignored, and no longer are assumed to specify octal octets. Octets are always decimal numbers. Octets must still be no more than three digits, including leading zeroes.
- bpo-36434: Errors during writing to a ZIP file no longer prevent to properly close it.
- bpo-36407: Fixed wrong indentation writing for CDATA section in xml.dom.minidom. Patch by Vladimir Surjaninov.
- bpo-36326: inspect.getdoc() can now find docstrings for member objects when __slots__ is a dictionary.
- bpo-36366: Calling `stop()` on an unstarted or stopped`unittest.mock.patch()` object will now return`None` instead of
raising`RuntimeError` , making the method idempotent. Patch by
Karthikeyan Singaravelan.
- bpo-36348: The `imap.IMAP4.logout()` method no longer ignores silently
arbitrary exceptions.
- bpo-31904: Add time module support and fix test_time failures for VxWorks.
- bpo-36227: Added support for keyword arguments `default_namespace` and`xml_declaration` in functions`ElementTree.tostring()` and`ElementTree.tostringlist()` .
- bpo-36004: Added new alternate constructors `datetime.date.fromisocalendar()` and`datetime.datetime.fromisocalendar()` , which construct date objects
from ISO year, week number and weekday; these are the inverse of each
classâs`isocalendar` method. Patch by Paul Ganssle.
- bpo-35936: `modulefinder` no longer depends on the deprecated`imp` module, and the initializer for`modulefinder.ModuleFinder` now has immutable default arguments.
Patch by Brandt Bucher.
- bpo-35376: `modulefinder` correctly handles modules that have the
same name as a bad package. Patch by Brandt Bucher.
- bpo-17396: `modulefinder` no longer crashes when encountering syntax
errors in followed imports. Patch by Brandt Bucher.
- bpo-35934: Added `create_server()` and`has_dualstack_ipv6()` convenience functions to automate the
necessary tasks usually involved when creating a server socket, including
accepting both IPv4 and IPv6 connections on the same socket.  (Contributed
by Giampaolo Rodola in bpo-17561.)
- bpo-23078: Add support for `classmethod()` and`staticmethod()` to`unittest.mock.create_autospec()` .  Initial patch by Felipe Ochoa.
- bpo-35416: Fix potential resource warnings in distutils. Patch by MickaÃ«l Schoentgen.
- bpo-25451: Add transparency methods to `tkinter.PhotoImage` .  Patch
by Zackery Spytz.
- bpo-35082: Donât return deleted attributes when calling dir on a `unittest.mock.Mock` .
- bpo-34547: `wsgiref.handlers.BaseHandler` now handles abrupt client
connection terminations gracefully. Patch by Petter Strandmark.
- bpo-31658: `xml.sax.parse()` now supports path-like. Patch by MickaÃ«l Schoentgen.
- bpo-34139: Remove stale unix datagram socket before binding
- bpo-33530: Implemented Happy Eyeballs in `asyncio.create_connection()` .
Added two new arguments,*happy_eyeballs_delay* and*interleave* , to
specify Happy Eyeballs behavior.
- bpo-33291: Do not raise AttributeError when calling the inspect functions isgeneratorfunction, iscoroutinefunction, isasyncgenfunction on a method created from an arbitrary callable. Instead, return False.
- bpo-31310: Fix the multiprocessing.semaphore_tracker so it is reused by child processes
- bpo-31292: Fix `setup.py check --restructuredtext` for files containing`include` directives.

### DocumentationÂ¶

- bpo-36625: Remove obsolete comments from docstrings in fractions.Fraction
- bpo-30840: Document relative imports
- bpo-36523: Add docstring for io.IOBase.writelines().
- bpo-36425: New documentation translation: Simplified Chinese.
- bpo-36345: Avoid the duplication of code from `Tools/scripts/serve.py` in using the`literalinclude` directive for the basic
wsgiref-based web server in the documentation of`wsgiref` .
Contributed by StÃ©phane Wirtel.
- bpo-36345: Using the code of the `Tools/scripts/serve.py` script as an
example in the`wsgiref` documentation.  Contributed by StÃ©phane
Wirtel.
- bpo-36157: Added documentation for PyInterpreterState_Main().
- bpo-33043: Updates the docs.python.org page with the addition of a âContributing to Docsâ link at the end of the page (between âReporting Bugsâ and âAbout Documentationâ). Updates the âFound a Bugâ page with additional links and information in the Documentation Bugs section.
- bpo-35581: @typing.type_check_only now allows type stubs to mark functions and classes not available during runtime.
- bpo-33832: Add glossary entry for âmagic methodâ.
- bpo-32913: Added re.Match.groupdict example to regex HOWTO.

### TestsÂ¶

- bpo-36719: regrtest now always detects uncollectable objects. Previously, the check was only enabled by `--findleaks` . The check now also works
with`-jN/--multiprocess N` .`--findleaks` becomes a deprecated alias
to`--fail-env-changed` .
- bpo-36725: When using multiprocessing mode (-jN), regrtest now better reports errors if a worker process fails, and it exits immediately on a worker thread failure or when interrupted.
- bpo-36454: Change test_time.test_monotonic() to test only the lower bound of elapsed time after a sleep command rather than the upper bound. This prevents unnecessary test failures on slow buildbots. Patch by Victor Stinner.
- bpo-32424: Improve test coverage for xml.etree.ElementTree. Patch by Gordon P. Hemsley.
- bpo-32424: Fix typo in test_cyclic_gc() test for xml.etree.ElementTree. Patch by Gordon P. Hemsley.
- bpo-36635: Add a new `_testinternalcapi` module to test the internal
C API.
- bpo-36629: Fix `test_imap4_host_default_value()` of`test_imaplib` :
catch also`errno.ENETUNREACH` error.
- bpo-36611: Fix `test_sys.test_getallocatedblocks()` when`tracemalloc` is enabled.
- bpo-36560: Fix reference leak hunting in regrtest: compute also deltas (of reference count, allocated memory blocks, file descriptor count) during warmup, to ensure that everything is initialized before starting to hunt reference leaks.
- bpo-36565: Fix reference hunting ( `python3 -m test -R 3:3` ) when Python
has no built-in abc module.
- bpo-31904: Port test_resource to VxWorks: skip tests cases setting RLIMIT_FSIZE and RLIMIT_CPU.
- bpo-31904: Fix test_tabnanny on VxWorks: adjust ENOENT error message.
- bpo-36436: Fix `_testcapi.pymem_buffer_overflow()` : handle memory
allocation failure.
- bpo-31904: Fix test_utf8_mode on VxWorks: Python always use UTF-8 on VxWorks.
- bpo-36341: Fix tests that may fail with PermissionError upon calling bind() on AF_UNIX sockets.

### BuildÂ¶

- bpo-36747: Remove the stale scriptsinstall Makefile target.
- bpo-21536: On Unix, C extensions are no longer linked to libpython except on Android and Cygwin. It is now possible for a statically linked Python to load a C extension built using a shared library Python. When Python is embedded, `libpython` must not be loaded with`RTLD_LOCAL` , but`RTLD_GLOBAL` instead. Previously, using`RTLD_LOCAL` , it was already not possible to load C extensions which
were not linked to`libpython` , such as C extensions of the standard
library built by the`*shared*` section of`Modules/Setup` .distutils, python-config and python-config.py have been modified.
- bpo-36707: `./configure --with-pymalloc` no longer adds the`m` flag
to SOABI (sys.implementation.cache_tag). Enabling or disabling pymalloc
has no impact on the ABI.
- bpo-36635: Change `PyAPI_FUNC(type)` ,`PyAPI_DATA(type)` and`PyMODINIT_FUNC` macros of`pyport.h` when`Py_BUILD_CORE_MODULE` is
defined. The`Py_BUILD_CORE_MODULE` define must be now be used to build
a C extension as a dynamic library accessing Python internals: export the`PyInit_`*xxx* ()
function in DLL exports on Windows.
- bpo-31904: Donât build the `_crypt` extension on VxWorks.
- bpo-36618: Add `-fmax-type-align=8` to CFLAGS when clang compiler is
detected. The pymalloc memory allocator aligns memory on 8 bytes. On
x86-64, clang expects alignment on 16 bytes by default and so uses MOVAPS
instruction which can lead to segmentation fault. Instruct clang that
Python is limited to alignment on 8 bytes to use MOVUPS instruction
instead: slower but donât trigger a SIGSEGV if the memory is not aligned
on 16 bytes. Sadly, the flag must be added to`CFLAGS` and not just`CFLAGS_NODIST` , since third party C extensions can have the same issue.
- bpo-36605: `make tags` and`make TAGS` now also parse`Modules/_io/*.c` and`Modules/_io/*.h` .
- bpo-36465: Release builds and debug builds are now ABI compatible: defining the `Py_DEBUG` macro no longer implies the`Py_TRACE_REFS` macro, which introduces the only ABI incompatibility. The`Py_TRACE_REFS` macro, which adds the`sys.getobjects()` function
and the`PYTHONDUMPREFS` environment variable, can be set using
the new`./configure --with-trace-refs` build option.
- bpo-36577: setup.py now correctly reports missing OpenSSL headers and libraries again.
- bpo-36544: Fix regression introduced in bpo-36146 refactoring setup.py
- bpo-36508: `python-config --ldflags` no longer includes flags of the`LINKFORSHARED` variable. The`LINKFORSHARED` variable must only be
used to build executables.
- bpo-36503: Remove references to âaix3â and âaix4â. Patch by M. Felt.

### WindowsÂ¶

- bpo-35920: Added platform.win32_edition() and platform.win32_is_iot(). Added support for cross-compiling packages for Windows ARM32. Skip tests that are not expected to work on Windows IoT Core ARM32.
- bpo-36649: Remove trailing spaces for registry keys when installed via the Store.
- bpo-34144: Fixed activate.bat to correctly update codepage when chcp.com returns dots in output. Patch by Lorenz Mende.
- bpo-36509: Added preset-iot layout for Windows IoT ARM containers. This layout doesnât contain UI components like tkinter or IDLE. It also doesnât contain files to support on-target builds since Windows ARM32 builds must be cross-compiled when using MSVC.
- bpo-35941: enum_certificates function of the ssl module now returns certificates from all available certificate stores inside windows in a query instead of returning only certificates from the system wide certificate store. This includes certificates from these certificate stores: local machine, local machine enterprise, local machine group policy, current user, current user group policy, services, users. ssl.enum_crls() function is changed in the same way to return all certificate revocation lists inside the windows certificate revocation list stores.
- bpo-36441: Fixes creating a venv when debug binaries are installed.
- bpo-36085: Enable better DLL resolution on Windows by using safe DLL search paths and adding `os.add_dll_directory()` .
- bpo-36010: Add the venv standard library module to the nuget distribution for Windows.
- bpo-29515: Add the following socket module constants on Windows: IPPROTO_AH IPPROTO_CBT IPPROTO_DSTOPTS IPPROTO_EGP IPPROTO_ESP IPPROTO_FRAGMENT IPPROTO_GGP IPPROTO_HOPOPTS IPPROTO_ICLFXBM IPPROTO_ICMPV6 IPPROTO_IDP IPPROTO_IGMP IPPROTO_IGP IPPROTO_IPV4 IPPROTO_IPV6 IPPROTO_L2TP IPPROTO_MAX IPPROTO_ND IPPROTO_NONE IPPROTO_PGM IPPROTO_PIM IPPROTO_PUP IPPROTO_RDP IPPROTO_ROUTING IPPROTO_SCTP IPPROTO_ST
- bpo-35947: Added current version of libffi to cpython-source-deps. Change _ctypes to use current version of libffi on Windows.
- bpo-34060: Report system load when running test suite on Windows. Patch by Ammar Askar. Based on prior work by Jeremy Kloth.
- bpo-31512: With the Windows 10 Creators Update, non-elevated users can now create symlinks as long as the computer has Developer Mode enabled.

### macOSÂ¶

### IDLEÂ¶

- bpo-36429: Fix starting IDLE with pyshell. Add idlelib.pyshell alias at top; remove pyshell alias at bottom. Remove obsolete __name__==â__main__â command.

### Tools/DemosÂ¶

- bpo-14546: Fix the argument handling in Tools/scripts/lll.py.

### C APIÂ¶

- bpo-36763: Fix memory leak in `Py_SetStandardStreamEncoding()` :
release memory if the function is called twice.
- bpo-36641: PyDoc_VAR(name) and PyDoc_STRVAR(name, str) now create `static const char name[]` instead of`static char name[]` .
Patch by Inada Naoki.
- bpo-36389: Change the value of `CLEANBYTE` ,`DEADDYTE` and`FORBIDDENBYTE` internal constants used by debug hooks on Python memory
allocators (`PyMem_SetupDebugHooks()` function). Byte patterns`0xCB` ,`0xDB` and`0xFB` have been replaced with`0xCD` ,`0xDD` and`0xFD` to use the same values than Windows CRT debug`malloc()` and`free()` .
- bpo-36443: Since Python 3.7.0, calling `Py_DecodeLocale()` before`Py_Initialize()` produces mojibake if the`LC_CTYPE` locale is
coerced and/or if the UTF-8 Mode is enabled by the user configuration. The
LC_CTYPE coercion and UTF-8 Mode are now disabled by default to fix the
mojibake issue. They must now be enabled explicitly (opt-in) using the new`_Py_PreInitialize()` API with`_PyPreConfig` .
- bpo-36025: Fixed an accidental change to the datetime C API where the arguments to the `PyDate_FromTimestamp()` function were incorrectly
interpreted as a single timestamp rather than an arguments tuple, which
causes existing code to start raising`TypeError` . The
backwards-incompatible change was only present in alpha releases of Python
3.8. Patch by Paul Ganssle.
- bpo-35810: Modify `PyObject_Init` to correctly increase the refcount of
heap-allocated Type objects. Also fix the refcounts of the heap-allocated
types that were either doing this manually or not decreasing the typeâs
refcount in tp_dealloc

## Python 3.8.0 alpha 3Â¶

*Release date: 2019-03-25*

### SecurityÂ¶

- bpo-36216: Changes urlsplit() to raise ValueError when the URL contains characters that decompose under IDNA encoding (NFKC-normalization) into characters that affect how the URL is parsed.
- bpo-35121: Donât send cookies of domain A without Domain attribute to domain B when domain A is a suffix match of domain B while using a cookiejar with `http.cookiejar.DefaultCookiePolicy` policy. Patch
by Karthikeyan Singaravelan.

### Core and BuiltinsÂ¶

- bpo-36421: Fix a possible double decref in _ctypes.câs `PyCArrayType_new()` .
- bpo-36412: Fix a possible crash when creating a new dictionary.
- bpo-36398: Fix a possible crash in `structseq_repr()` .
- bpo-36256: Fix bug in parsermodule when parsing a state in a DFA that has two or more arcs with labels of the same type. Patch by Pablo Galindo.
- bpo-36365: repr(structseq) is no longer limited to 512 bytes.
- bpo-36374: Fix a possible null pointer dereference in `merge_consts_recursive()` . Patch by Zackery Spytz.
- bpo-36236: At Python initialization, the current directory is no longer prepended to `sys.path` if it has been removed.
- bpo-36352: Python initialization now fails with an error, rather than silently truncating paths, if a path is too long.
- bpo-36301: Python initialization now fails if decoding `pybuilddir.txt` configuration file fails at startup.
- bpo-36333: Fix leak in _PyRuntimeState_Fini. Contributed by StÃ©phane Wirtel.
- bpo-36332: The builtin `compile()` can now handle AST objects that
contain assignment expressions. Patch by Pablo Galindo.
- bpo-36282: Improved error message for too much positional arguments in some builtin functions.
- bpo-30040: New empty dict uses fewer memory for now. It used more memory than empty dict created by `dict.clear()` .  And empty dict creation and
deletion is about 2x faster.  Patch by Inada Naoki.
- bpo-36262: Fix an unlikely memory leak on conversion from string to float in the function `_Py_dg_strtod()` used by`float(str)` ,`complex(str)` ,`pickle.load()` ,`marshal.load()` , etc.
- bpo-36252: Update Unicode databases to version 12.0.0.
- bpo-36218: Fix a segfault occurring when sorting a list of heterogeneous values. Patch contributed by RÃ©mi Lapeyre and Elliot Gorokhovsky.
- bpo-36188: Cleaned up left-over vestiges of Python 2 unbound method handling in method objects and documentation. Patch by Martijn Pieters
- bpo-36124: Add a new interpreter-specific dict and expose it in the C-API via PyInterpreterState_GetDict(). This parallels PyThreadState_GetDict(). However, extension modules should continue using PyModule_GetState() for their own internal per-interpreter state.
- bpo-35975: Add a `feature_version` flag to`ast.parse()` (documented)
and`compile()` (hidden) that allows tweaking the parser to support
older versions of the grammar. In particular, if`feature_version` is 5
or 6, the hacks for the`async` and`await` keyword from PEP 492 are
reinstated. (For 7 or higher, these are unconditionally treated as
keywords, but they are still special tokens rather than`NAME` tokens
that the parser driver recognizes.)
- bpo-31904: Use UTF-8 as the system encoding on VxWorks.
- bpo-36048: The `__index__()` special method will be used
instead of`__int__()` for implicit conversion of Python
numbers to C integers.  Using the`__int__()` method in implicit
conversions has been deprecated.
- bpo-35808: Retire pgen and use a modified version of pgen2 to generate the parser. Patch by Pablo Galindo.

### LibraryÂ¶

- bpo-36401: The class documentation created by pydoc now has a separate section for readonly properties.
- bpo-36320: The typing.NamedTuple() class has deprecated the _field_types attribute in favor of the __annotations__ attribute which carried the same information. Also, both attributes were converted from OrderedDict to a regular dict.
- bpo-34745: Fix `asyncio` ssl memory issues caused by circular
references
- bpo-36324: Add method to statistics.NormalDist for computing the inverse cumulative normal distribution.
- bpo-36321: collections.namedtuple() misspelled the name of an attribute. To be consistent with typing.NamedTuple, the attribute name should have been â_field_defaultsâ instead of â_fields_defaultsâ. For backwards compatibility, both spellings are now created. The misspelled version may be removed in the future.
- bpo-36297: âunicode_internalâ codec is removed. It was deprecated since Python 3.3. Patch by Inada Naoki.
- bpo-36298: Raise ModuleNotFoundError in pyclbr when a module canât be found. Thanks to âmentalâ for the bug report.
- bpo-36268: Switch the default format used for writing tars with `tarfile` to the modern POSIX.1-2001 pax standard, from the
vendor-specific GNU. Contributed by C.A.M. Gerlach.
- bpo-36285: Fix integer overflows in the array module. Patch by Stephan Hohe.
- bpo-31904: Add _signal module support for VxWorks.
- bpo-36272: `logging` does not silently ignore RecursionError anymore.
Patch contributed by RÃ©mi Lapeyre.
- bpo-36280: Add a kind field to ast.Constant. It is âuâ if the literal has a âuâ prefix (i.e. a Python 2 style unicode literal), else None.
- bpo-35931: The `pdb``debug` command now gracefully handles all
exceptions.
- bpo-36251: Fix format strings used for stderrprinter and re.Match reprs. Patch by Stephan Hohe.
- bpo-36235: Fix `CFLAGS` in`customize_compiler()` of`distutils.sysconfig` : when the`CFLAGS` environment variable is
defined, donât override`CFLAGS` variable with the`OPT` variable
anymore. Initial patch written by David Malcolm.
- bpo-35807: Update ensurepip to install pip 19.0.3 and setuptools 40.8.0.
- bpo-36139: Release GIL when closing `mmap` objects.
- bpo-36179: Fix two unlikely reference leaks in _hashopenssl. The leaks only occur in out-of-memory cases.
- bpo-36169: Add overlap() method to statistics.NormalDist. Computes the overlapping coefficient for two normal distributions.
- bpo-36103: Default buffer size used by `shutil.copyfileobj()` is changed
from 16 KiB to 64 KiB on non-Windows platform to reduce system call
overhead. Contributed by Inada Naoki.
- bpo-36130: Fix `pdb` with`skip=...` when stepping into a frame
without a`__name__` global.  Patch by Anthony Sottile.
- bpo-35652: shutil.copytree(copy_function=â¦) erroneously pass DirEntry instead of a path string.
- bpo-35178: Ensure custom `warnings.formatwarning()` function can
receive`line` as positional argument. Based on patch by Tashrif Billah.
- bpo-36106: Resolve potential name clash with libmâs sinpi(). Patch by Dmitrii Pasechnik.
- bpo-36091: Clean up reference to async generator in Lib/types. Patch by Henry Chen.
- bpo-36043: `FileCookieJar` supports path-like object.
Contributed by StÃ©phane Wirtel
- bpo-35899: Enum has been fixed to correctly handle empty strings and strings with non-Latin characters (ie. âÎ±â, â×â) without crashing. Original patch contributed by Maxwell. Assisted by StÃ©phane Wirtel.
- bpo-21269: Add `args` and`kwargs` properties to mock call objects.
Contributed by Kumar Akshay.
- bpo-30670: `pprint.pp` has been added to pretty-print objects with
dictionary keys being sorted with their insertion order by default.
Parameter*sort_dicts* has been added to`pprint.pprint` ,`pprint.pformat` and`pprint.PrettyPrinter` . Contributed by RÃ©mi
Lapeyre.
- bpo-35843: Implement `__getitem__` for`_NamespacePath` .  Patch by
Anthony Sottile.
- bpo-35802: Clean up code which checked presence of `os.stat` /`os.lstat` /`os.chmod` which are always present.  Patch by Anthony
Sottile.
- bpo-35715: Librates the return value of a ProcessPoolExecutor _process_worker after itâs no longer needed to free memory
- bpo-35493: Use `multiprocessing.connection.wait()` instead of polling
each 0.2 seconds for worker updates in`multiprocessing.Pool` .
Patch by Pablo Galindo.
- bpo-35661: Store the venv prompt in pyvenv.cfg.
- bpo-35121: Donât set cookie for a request when the request path is a prefix match of the cookieâs path attribute but doesnât end with â/â. Patch by Karthikeyan Singaravelan.
- bpo-21478: Calls to a child function created with `unittest.mock.create_autospec()` should propagate to the parent.
Patch by Karthikeyan Singaravelan.
- bpo-35198: Fix C++ extension compilation on AIX

### DocumentationÂ¶

- bpo-36329: Declare the path of the Python binary for the usage of `Tools/scripts/serve.py` when executing`make -C Doc/ serve` .
Contributed by StÃ©phane Wirtel
- bpo-36138: Improve documentation about converting datetime.timedelta to scalars.
- bpo-21314: A new entry was added to the Core Language Section of the Programming FAQ, which explaines the usage of slash(/) in the signature of a function. Patch by Lysandros Nikolaou

### TestsÂ¶

- bpo-36234: test_posix.PosixUidGidTests: add tests for invalid uid/gid type (str). Initial patch written by David Malcolm.
- bpo-29571: Fix `test_re.test_locale_flag()` :  use`locale.getpreferredencoding()` rather than`locale.getlocale()` to
get the locale encoding. With some locales,`locale.getlocale()` returns
the wrong encoding.
- bpo-36123: Fix race condition in test_socket.

### BuildÂ¶

- bpo-36356: Fix leaks that led to build failure when configured with address sanitizer.
- bpo-36146: Add `TEST_EXTENSIONS` constant to`setup.py` to allow to
not build test extensions like`_testcapi` .
- bpo-36146: Fix setup.py on macOS: only add `/usr/include/ffi` to include
directories of _ctypes, not for all extensions.
- bpo-31904: Enable build system to cross-build for VxWorks RTOS.

### WindowsÂ¶

- bpo-36312: Fixed decoders for the following code pages: 50220, 50221, 50222, 50225, 50227, 50229, 57002 through 57011, 65000 and 42.
- bpo-36264: Donât honor POSIX `HOME` in`os.path.expanduser` on
windows.  Patch by Anthony Sottile.
- bpo-24643: Fix name collisions due to `#define timezone _timezone` in
PC/pyconfig.h.

### IDLEÂ¶

- bpo-36405: Use dict unpacking in idlelib.
- bpo-36396: Remove fgBg param of idlelib.config.GetHighlight(). This param was only used twice and changed the return type.
- bpo-36176: Fix IDLE autocomplete & calltip popup colors. Prevent conflicts with Linux dark themes (and slightly darken calltip background).
- bpo-23205: For the grep module, add tests for findfiles, refactor findfiles to be a module-level function, and refactor findfiles to use os.walk.
- bpo-23216: Add docstrings to IDLE search modules.
- bpo-36152: Remove colorizer.ColorDelegator.close_when_done and the corresponding argument of .close(). In IDLE, both have always been None or False since 2007.
- bpo-32129: Avoid blurry IDLE application icon on macOS with Tk 8.6. Patch by Kevin Walzer.
- bpo-36096: Refactor class variables to instance variables in colorizer.
- bpo-30348: Increase test coverage of idlelib.autocomplete by 30%. Patch by Louie Lu

### Tools/DemosÂ¶

### C APIÂ¶

## Python 3.8.0 alpha 2Â¶

*Release date: 2019-02-25*

### Core and BuiltinsÂ¶

- bpo-36052: Raise a `SyntaxError` when assigning a value to`__debug__` with the Assignment Operator. Contributed by StÃ©phane Wirtel
and Pablo Galindo.
- bpo-36012: Doubled the speed of class variable writes. When a non-dunder attribute was updated, there was an unnecessary call to update slots.
- bpo-35942: The error message emitted when returning invalid types from `__fspath__` in interfaces that allow passing`PathLike` objects has been improved and now it does explain the origin of the error.
- bpo-36016: `gc.get_objects` can now receive an optional parameter
indicating a generation to get objects from. Patch by Pablo Galindo.
- bpo-1054041: When the main interpreter exits due to an uncaught KeyboardInterrupt, the process now exits in the appropriate manner for its parent process to detect that a SIGINT or ^C terminated the process. This allows shells and batch scripts to understand that the user has asked them to stop.
- bpo-35992: Fix `__class_getitem__()` not being called on a class with a
custom non-subscriptable metaclass.
- bpo-35993: Fix a crash on fork when using subinterpreters. Contributed by StÃ©phane Wirtel
- bpo-35991: Fix a potential double free in Modules/_randommodule.c.
- bpo-35961: Fix a crash in slice_richcompare(): use strong references rather than stolen references for the two temporary internal tuples.
- bpo-35911: Enable the creation of cell objects by adding a `cell.__new__` method, and expose the type`cell` in`Lib/types.py` under the name CellType. Patch by Pierre Glaser.
- bpo-12822: Use monotonic clock for `pthread_cond_timedwait` when`pthread_condattr_setclock` and`CLOCK_MONOTONIC` are available.
- bpo-15248: The compiler emits now syntax warnings in the case when a comma is likely missed before tuple or list.
- bpo-35886: The implementation of PyInterpreterState has been moved into the internal header files (guarded by Py_BUILD_CORE).
- bpo-31506: Clarify the errors reported when `object.__new__` and`object.__init__` receive more than one argument. Contributed by Sanyam
Khurana.
- bpo-35724: Signal-handling is now guaranteed to happen relative to the main interpreter.
- bpo-33608: We added a new internal _Py_AddPendingCall() that operates relative to the provided interpreter. This allows us to use the existing implementation to ask another interpreter to do work that cannot be done in the current interpreter, like decref an object the other interpreter owns. The existing Py_AddPendingCall() only operates relative to the main interpreter.
- bpo-33989: Fix a possible crash in `list.sort()` when sorting objects
with`ob_type->tp_richcompare == NULL` .  Patch by Zackery Spytz.

### LibraryÂ¶

- bpo-35512: `unittest.mock.patch.dict()` used as a decorator with
string target resolves the target during function call instead of during
decorator construction. Patch by Karthikeyan Singaravelan.
- bpo-36018: Add statistics.NormalDist, a tool for creating and manipulating normal distributions of random variable. Features a composite class that treats the mean and standard deviation of measurement data as single entity.
- bpo-35904: Added statistics.fmean() as a faster, floating-point variant of the existing mean() function.
- bpo-35918: Removed broken `has_key` method from
multiprocessing.managers.SyncManager.dict. Contributed by RÃ©mi Lapeyre.
- bpo-18283: Add support for bytes to `shutil.which()` .
- bpo-35960: Fix `dataclasses.field()` throwing away empty mapping
objects passed as metadata.
- bpo-35500: Write expected and actual call parameters on separate lines in `unittest.mock.Mock.assert_called_with()` assertion errors.
Contributed by Susan Su.
- bpo-35931: The `pdb``debug` command now gracefully handles syntax
errors.
- bpo-24209: In http.server script, rely on getaddrinfo to bind to preferred address based on the bind parameter. Now default bind or binding to a name may bind to IPv6 or dual-stack, depending on the environment.
- bpo-35321: Set `__spec__.origin` of`_frozen_importlib` to frozen so
that it matches the behavior of`_frozen_importlib_external` .  Patch by
Nina Zakharenko.
- bpo-35378: Fix a reference issue inside `multiprocessing.Pool` that
caused the pool to remain alive if it was deleted without being closed or
terminated explicitly. A new strong reference is added to the pool
iterators to link the lifetime of the pool to the lifetime of its
iterators so the pool does not get destroyed if a pool iterator is still
alive.
- bpo-34294: re module, fix wrong capturing groups in rare cases. `re.search()` ,`re.findall()` ,`re.sub()` and other functions
that scan through string looking for a match, should reset capturing
groups between two match attempts. Patch by Ma Lin.
- bpo-35615: `weakref` : Fix a RuntimeError when copying a
WeakKeyDictionary or a WeakValueDictionary, due to some keys or values
disappearing while iterating.
- bpo-35606: Implement `math.prod()` as analogous function to`sum()` that returns the product of a âstartâ value (default: 1) times
an iterable of numbers. Patch by Pablo Galindo.
- bpo-32417: Performing arithmetic between `datetime.datetime` subclasses and`datetime.timedelta` now returns an object of the
same type as the`datetime.datetime` subclass. As a result,`datetime.datetime.astimezone()` and alternate constructors like`datetime.datetime.now()` and`datetime.fromtimestamp()` called
with a`tz` argument now*also* retain their subclass.
- bpo-35153: Add *headers* optional keyword-only parameter to`xmlrpc.client.ServerProxy` ,`xmlrpc.client.Transport` and`xmlrpc.client.SafeTransport` .  Patch by CÃ©dric Krier.
- bpo-34572: Fix C implementation of pickle.loads to use importlibâs locking mechanisms, and thereby avoid using partially loaded modules. Patch by Tim Burgess.

### DocumentationÂ¶

### TestsÂ¶

- bpo-27313: Avoid test_ttk_guionly ComboboxTest failure with macOS Cocoa Tk.
- bpo-36019: Add test.support.TEST_HTTP_URL and replace references of http://www.example.com by this new constant. Contributed by StÃ©phane Wirtel.
- bpo-36037: Fix test_ssl for strict OpenSSL configuration like RHEL8 strict crypto policy. Use older TLS version for minimum TLS version of the server SSL context if needed, to test TLS version older than default minimum TLS version.
- bpo-35798: Added `test.support.check_syntax_warning()` .
- bpo-35505: Make test_imap4_host_default_value independent on whether the local IMAP server is running.
- bpo-35917: multiprocessing: provide unit tests for SyncManager and SharedMemoryManager classes + all the shareable types which are supposed to be supported by them. (patch by Giampaolo Rodola)
- bpo-35704: Skip `test_shutil.test_unpack_archive_xztar` to prevent a
MemoryError on 32-bit AIX when MAXDATA setting is less than 0x20000000.Patch by Michael Felt (aixtools)
- bpo-34720: Assert m_state != NULL to mimic GC traversal functions that do not correctly handle module creation when the module state has not been created.

### WindowsÂ¶

- bpo-35976: Added ARM build support to Windows build files in PCBuild.
- bpo-35692: `pathlib` no longer raises when checking file and directory
existence on drives that are not ready
- bpo-35872: Uses the base Python executable when invoking venv in a virtual environment
- bpo-35873: Prevents venv paths being inherited by child processes
- bpo-35299: Fix sysconfig detection of the source directory and distutils handling of pyconfig.h during PGO profiling

### IDLEÂ¶
