---
id: collect-261121-veille-ia/docs-techniques/changelog-2-1
title: "Changelog"
domain: docs-techniques
role: reference
task: article
actors: []
dates: ["2026-10-21"]
keywords: ["crash", "gh-159739", "avoid", "patch", "during", "thread", "bytes", "fails"]
source: https://docs.python.org/release/3.15.1/whatsnew/changelog.html
source_anchor: ""
source_lines: [1, 1500]
section: "Docs_Techniques"
---
# ChangelogÂ¶

## Python 3.15.1 finalÂ¶

*Release date: 2026-10-21*

### SecurityÂ¶

### Core and BuiltinsÂ¶

- gh-159739: Avoid a crash if allocation fails while creating a `string.templatelib.Template` iterator.
- gh-159739: Propagate errors during generic class construction and avoid a crash after failed generic alias substitution.
- gh-159739: Raise `OverflowError` when the absolute value of a dynamic
printf-style formatting width is too large.
- gh-159739: Fix a reference leak in `gc.get_objects()` and`gc.get_referrers()` when an internal allocation fails in the
free-threaded build.
- gh-159739: Fix memory leaks during interpreter startup when a path configuration file is empty or too large, or when an audit hook rejects a command.
- gh-159739: Avoid a crash when an interpreter configuration string cannot be encoded as UTF-8.
- gh-159739: Preserve the original `SyntaxError` if allocating optional
error metadata fails.
- gh-159739: Avoid a crash if formatting an integer with the `c` presentation type runs out of memory.
- gh-159739: Avoid a crash when a module filename cannot be encoded as UTF-8.
- gh-159603: On FreeBSD, call `pthread_attr_get_np()` to get the stack
size (and guard size) of the current thread. Patch by Victor Stinner.
- gh-130279: Fix bug where compiler did not set the right exception handler for some jump instructions.
- gh-159430: Report the correct exception if looking up `__bytes__()` raises an exception while performing
%-formatting of`bytes` .
- gh-159320: Add dynamic detection of stack sizes on Android, allowing safer deep recursion.
- gh-159288: Fix a crash when walking a very long chain of lazy import placeholders, as created by an aliased dotted import such as ```
lazy import
a.b.c as d
```
.`RecursionError` is now raised instead.
- gh-159285: Fix a crash in a debug build when copying an empty `dict` fails with`MemoryError` . Patch by Christian Aurich
Zanettini Martins.
- gh-159041: Fix a use-after-free in `asyncio` task bookkeeping when a
finalizer creates a task during thread shutdown.
- gh-158973: Reset the warnings lock in the child after `os.fork()` ,
before clearing other threadsâ states, to avoid deadlocks when their
finalizers issue warnings. Preserve the lockâs recursion depth if the
forking thread owns it.
- gh-158918: Reduce C stack usage of `_PyModule_IsPossiblyShadowing` ,
which could potentially cause stack overflows during module attribute
access while deeply recursing in agressively optimized builds.
- gh-158803: Fix a crash in `bytes.join()` and`bytearray.join()` in
the free-threaded build when another thread concurrently mutates
the list being joined. Patch by Christian Aurich Zanettini Martins.
- gh-158824: Fix module-level `__getattr__()` being called
before a moduleâs own lazy imports were resolved, which allowed it to
shadow them.
- gh-158820: Fix a crash when a lazy import is declared or resolved by a finalizer running during interpreter shutdown.
- gh-158592: Skip the array reallocation when shrinking a small `list` . Patch by Donghee Na.
- gh-158583: `bytes.fromhex()` and`bytearray.fromhex()` : Fix
uninitialized memory read. Patch by Victor Stinner.
- gh-158197: Fix crashes and debug-build assertion failures in the free-threaded build when iterating over a split dictionary while another thread deletes entries or clears it. Publish insertion-order updates atomically so readers cannot observe an increased size before the corresponding order entry is initialized.
- gh-158592: `del list[index]` now triggers shrinking the listâs internal
array.
- gh-158486: Fix the PowerSort mask calculation in `list.sort()` for
very large lists.
- gh-158121: Fix missing `sys.monitoring``INSTRUCTION` events when a`LINE` callback returns`sys.monitoring.DISABLE` .
- gh-158219: Fix `bytearray` constructor: do not use a bytes object
if its hash value is already computed. Patch by Cody Maloney and Victor
Stinner.
- gh-158072: Fix a JIT crash when a class is instantiated with their default arguments
- gh-157468: Fix the JIT optimizer folding a builtin name to a constant taken from the interpreterâs builtins dictionary even when the running function uses a different `__builtins__` mapping, such as one created
with`vars(builtins).copy()` . The optimizer now only folds when the
functionâs builtins is the interpreterâs, and the folded constant is
guarded at runtime so that another function sharing the same code object
but a different builtins mapping does not use it.
- gh-157242: If `bytearray.resize()` or`bytearray.take_bytes()` fails, leave the`bytearray` unchanged, instead of clearing it.
Patch by Victor Stinner.
- gh-156091: Fix a crash when compiling deeply nested inlined list, set, or dict comprehensions. A `SyntaxError` is now raised when the nesting
exceeds the compilerâs static block limit.
- gh-106045: Fix standard library discovery in virtual environments created from a Python executable symlink located outside its base installation.
- gh-156459: Fix cleanup on error in `compiler_set_qualname` . Previously
it was called in`_PyCompile_EnterScope` , after the scope had been
entered, and this was not reversed in case of an error.
- gh-156371: Fix missing `PyRefTracer_DESTROY` events for trashcan
deferred objects. Patch by Donghee Na.
- gh-156204: Fix a crash in `PyErr_GivenExceptionMatches()` when
evaluating deeply nested exception tuples. The recursion is now bounded,
and exceeding the limit is reported as an unraisable exception.
- gh-155823: Fix a JIT bug where the side exit taken when a virtual iterator is exhausted left a stale value on the stack, so the tier one interpreter resumed with corrupted state and `yield from` raised`AttributeError` instead of finishing normally.
- gh-155504: Fix a crash when thread state creation fails before the thread state is fully initialized.
- gh-155315: Fix `marshal` so that a`frozendict` referenced more
than once in the serialized data round-trips correctly, instead of failing
to load with`ValueError` . Patch by tonghuaroot.
- gh-154937: Fix a data race on thread handle identifiers when `_thread._shutdown()` runs concurrently with the startup of non-daemon
threads in the free-threaded build.
- gh-152075: Reduced lock contention during LOAD_GLOBAL bytecode specialization under free threading.
- gh-153578: Fix an out-of-bounds write in `bytearray.extend()` when the
bytearray is resized while the argumentâs`__buffer__()` is
being acquired, for example by another thread. Patch by tonghuaroot.
- gh-151895: Fixed a crash in `marshal.loads()` when an allocation
failed while loading a reference-tracked dictionary; it now raises`MemoryError` .
- gh-151518: Fix a free-threaded stop-the-world fairness issue that could starve a thread reattaching after being suspended while detached.
- gh-149816: Fix a Use-After-Free race condition in `subtype_getweakref()` during concurrent weakref creation and destruction in the free-threading
build.
- gh-149816: Fix race conditions in `invoke_gc_callback` iterating`gc.callbacks` in free-threading mode.
- gh-69605: Fix REPL completions inserted incorrectly when there was spaces in import statements.

### LibraryÂ¶

- gh-138641: Ignore `pdb.set_trace()` when called directly at the`pdb` prompt.
- gh-159864: Fix a crash in `syslog.openlog()` under the free-threaded
build when`sys.argv` is modified concurrently.
- gh-159811: Fix a crash in `hashlib` when the OpenSSL routine used by`hash.update()` or by hash object constructors
fails on data larger than 2048 bytes. Patch by BÃ©nÃ©dikt Tran.
- gh-159811: Fix a crash in `hmac` when the OpenSSL routine used by`hmac.HMAC.update()` or by`hmac.new()` fails on data larger than
2048 bytes. Patch by BÃ©nÃ©dikt Tran.
- gh-159786: Fix the error handling of the HACL* routine used by `hmac.new()` and`hmac.HMAC.update()` . If the maximum input length
of the underlying hash function is exceeded, an`OverflowError` is
now raised on all builds, instead of crashing on DEBUG builds and being
silently ignored otherwise. Code correctness. Realistically this can only
happen at implausible sizes. Patch by BÃ©nÃ©dikt Tran.
- gh-159739: Fix copying `collections.deque` subclasses with a large
maximum length.
- gh-159739: Allow cyclic garbage collection to reclaim group records returned by `grp` .
- gh-159739: Fix `io.BytesIO` reads into overlapping views of the
same buffer.
- gh-159739: Avoid a crash when memory allocation fails while creating a `collections.OrderedDict` items iterator.
- gh-159739: Avoid reading past the end of an encoding name when the `surrogatepass` error handler receives a truncated UTF-16 or UTF-32
name.
- gh-159739: Fix incorrect `ctypes` scalar values after`ctypes.resize()` enlarges the object.
- gh-159739: Fix a memory leak when Unicode normalization cannot grow its temporary buffer.
- gh-159739: Fix resource leaks in `ssl` when certificate conversion or
key log setup fails.
- gh-149015: Fix `bdb.Bdb.clear_all_file_breaks()` leaving breakpoints
behind when multiple breakpoints are set on the same line.
- gh-159620: Fix `python -m asyncio ps` ,`pstree` and async-aware`profiling.sampling` stacks stopping at asynchronous generators.
Patch by Timofei Ivankov.
- gh-159458: Fix a ThreadSanitizer data race on the static cache in `normalize_century()` of the datetime module under the free-threaded
build.
- gh-159430: Report the correct exception if looking up `__name__` raises
an exception when calling`repr()` on a`weakref.ref` .
- gh-159225: Prevent `input` calls in PyREPL from modifying readline
history.
- gh-159044: Restore the previous behaviour of `http.client.HTTPConnection.set_tunnel()` where header values that are
not strings (such as`None` ,`int` or other objects) are
converted with`str` instead of raising`AttributeError` when
the request is sent.
- gh-159184: Fix `marshal.dumps()` raising`MemoryError` for a set
or frozenset containing several complex NaNs, which also made it
impossible to write a`.pyc` file for source code containing such a
constant.
- gh-159185: Fix a crash in `marshal.dumps()` and`marshal.dump()` when a set is modified while its elements are serialized, for example by
the`__buffer__()` method of one of its elements.`RuntimeError` is now raised.
- gh-159049: Fix reference cycles in `asyncio.loop.create_connection()` and`asyncio.loop.start_tls()` after a failed TLS handshake. Patch by
Timofei Ivankov.
- gh-87720: Fixed error in `email` new policies where header folding
failed to detect the need to re-quote text obtained from encoded words in
certain cases.
- gh-158929: Fix crash when calling `syslog.syslog()` while a
subinterpreter in another thread calls`syslog.openlog()` or`syslog.closelog()` .
- gh-158936: `dataclass()` no longer fails when a string
annotation names a lazy import that cannot be
resolved, matching the behavior for non-string annotations.
- gh-158893: Make `os.strerror()` thread-safe: use the reentrant`strerror_r()` function if available. Patch by Victor Stinner.
- gh-158539: Fix macOS runtime section lookup for remote debugging when virtual memory region sizes differ from Mach-O segment file sizes.
- gh-156545: Fix deep flamegraph exports on platforms with small C stacks by falling back to the Python JSON encoder when the C encoder runs out of stack space.
- gh-158552: Wait for Windows threads to finish suspending before reading target memory in the sampling profilerâs blocking mode. Resume the target if waiting for suspension fails.
- gh-156321: Fix `asyncio.shield()` reporting an exception from the
inner future through the loop exception handler as soon as it completes
after the shield was cancelled, even when the exception is retrieved
afterwards. The exception is now reported only if it is never retrieved,
as with any other future.
- gh-158539: Fix `profiling.sampling` exception mode discarding samples
for code running inside an`except` block in a generator or coroutine.
The remote debugger now follows`tstate->exc_info` and its`previous_item` chain instead of only reading the embedded`exc_state` , matching the exception that`sys.exception()` reports.
- gh-158563: Fix `turtle.undo()` after`turtle.teleport()` and after
an exception in`turtle.circle()` ,`turtle.dot()` or`turtle.write()` . Fix`turtle.stamp()` ,`turtle.clearstamp()` ,`turtle.clear()` and`turtle.reset()` when the undo buffer is
disabled.
- gh-158540: Fix `profiling.sampling` not adding the directory
containing the profiled script to`sys.path` , which prevented the
script from importing modules placed next to it.  The scriptâs directory
is now added, matching the behavior of`python script.py` .
- gh-158522: Fix `profiling.sampling` showing a single frame for a task
whose coroutine recurses. Patch by Timofei Ivankov.
- gh-158169: Fix `zlib.Decompress.unconsumed_tail` duplicating`unused_data` after end of stream when*max_length* was used.
- gh-158004: Fix `asyncio.as_completed()` to continue yielding every
awaitable after a pending iteration step is cancelled.
- gh-153888: Fix `unittest.mock.patch()` with`autospec` ,`spec` , or`spec_set` when used with lazy-imported objects.
- gh-157672: Fix `tkinter` on macOS: creating a`Tk` instance no longer replaces the Python handlers of SIGINT, SIGHUP and
SIGTERM with Tkâs handler which exits the process, so Ctrl-C raises`KeyboardInterrupt` again.
- gh-157621: Fix a crash when a weak-reference callback is attached to a `typing.ParamSpecArgs` or`typing.ParamSpecKwargs` instance.
- gh-69919: `code.InteractiveInterpreter.runsource()` and the REPL now
report any exception raised by compiling the source, such as`MemoryError` or`RecursionError` for too deeply nested source,
instead of propagating it or printing a traceback of the REPL internals.
- gh-157451: Fix `gettext` plural form selection when the`Plural-Forms` expression combines the unary`!` operator with a
binary operator, for example`!n + 1` or`2 * !n` .  The latter raised`SyntaxError` .
- gh-157406: Fix the Python implementation of `xml.etree.ElementTree.XMLParser` : the`doctype()` method of the
target is now called for a document type declaration without an external
identifier, like`<!DOCTYPE html>` , as in the C implementation.
- gh-157364: Fix a use-after-free in `io.TextIOWrapper` when a call
to the underlying buffer reentrantly detaches it. Patched by Shamil
Abdulaev.
- gh-157242: `io.BytesIO` is no longer closed on`write()` and`truncate()` failure (`MemoryError` ). Patch by Victor Stinner.
- gh-157325: Fix an out-of-bounds read in the `hz` incremental decoder
when the input ends with`~` .
- gh-123018: Fix `readline.write_history_file()` and`readline.append_history_file()` producing a history file that`readline.read_history_file()` could not load when a limit was set
with`readline.set_history_length()` and Python was built against`libedit` .  This works around NetBSD PR 60322.
- gh-157146: `linecache` can now read the source of a module that was
imported from a zip archive on`sys.path` when given only the file
name, as`pdb` ,`warnings` and`doctest` do.
- gh-157048: Preserve the position and contents of `io.BytesIO` when`BytesIO.__init__()` fails because a buffer is exported.
- gh-156961: Fix `tkinter.font.nametofont()` and the`tkinter.font.Font` constructor for a font name returned by Tk as a
Tcl object, for example by`ttk.Style.lookup()` .
- gh-156946: Fix a crash in `curses.panel` when the finalizer of a
panelâs user pointer runs while the panel is being deallocated.  The panel
is now taken off the panel stack before its user pointer is dropped.
- gh-156920: Fix `asyncio` on Windows: closing a`DatagramTransport` under`ProactorEventLoop` while datagrams were still queued, or
while an in-flight write failed right as`close()` was draining the
buffer, could strand the queued data and never call`connection_lost()` ,
hanging the close indefinitely.
- gh-156810: Fix a `UnicodeEncodeError` crash in the sampling
profilerâs collapsed-stack export (`--collapsed` ) when a sampled frameâs
function or file name contains non-ASCII or surrogate-escaped characters.
The output file is now written as UTF-8.
- gh-156797: Fix `xml.sax.saxutils.XMLGenerator` : the namespace URI
is now escaped in the namespace declaration.  Previously a URI containing`&` ,`<` or a quote character produced a document which cannot be
parsed.
- gh-83895: `xml.etree.ElementTree` now accepts input larger than 2 GiB
in the C implementation. The data is fed to Expat in chunks, as`xml.parsers.expat` already did, instead of raising`OverflowError` .
- gh-99064: Fix `xml.etree.ElementTree.parse()` with a text file or
other source of`str` data in the C implementation. The encoding
declared in the document was applied to the already decoded text, which
produced mojibake. It is now ignored, as when parsing with`XMLParser.feed()` or`fromstring()` .
- gh-156713: Fix `nturl2path.pathname2url()` and`nturl2path.url2pathname()` : the filesystem encoding and error
handler are now used for percent-encoding and decoding, as in`urllib.request` .  Previously paths containing surrogate characters
raised`UnicodeEncodeError` .
- gh-156698: Fix `asyncio.ProactorEventLoop` UDP transports so that a
write error no longer strands a paused protocol: the write loop is now
rescheduled when data remains buffered, and the protocol is resumed when
the buffer has drained.
- gh-156658: `xml.dom` and`xml.etree.ElementTree` no longer treat
characters which are not white space in XML (such as U+00A0) as white
space. Previously they could be lost in`indent()` ,`canonicalize()` with`strip_text=True` , and
when parsing with the`whitespace-in-element-content` feature turned
off.
- gh-156568: `zipapp.create_archive()` now correctly sets the executable
bit on the target archive when copying a zipapp where the target is a
path-like object.
- gh-156545: Fix the sampling profilerâs flamegraph export so that profiling a deeply recursive program no longer fails with `RecursionError` instead of producing a flamegraph. Patch by tonghuaroot.
- gh-156476: The pure Python implementation of `queue.SimpleQueue.get()` now ignores*timeout* when*block* is false, matching the documented
behavior and the C implementation. It previously raised`ValueError` .
- gh-156444: Fix matching of a negated character set in a bytes pattern compiled with both `re.IGNORECASE` and`re.LOCALE` .  The
case closure is now applied to the members of the set, so that`[^bc]` no longer matches`b'B'` .
- gh-156180: `zlib.adler32_combine()` and`zlib.crc32_combine()` now
raise`ValueError` if the*len2* argument is negative, instead of
returning rubbish or hanging indefinitely, respectively.
- gh-156166: `ssl` : A failed assignment or deletion of the`_msg_callback` attribute of`ssl.SSLContext` no longer removes
the current callback. Deleting it now raises`AttributeError` instead
of`TypeError` .
- gh-156173: Calling `zlib.Decompress.flush()` on invalid compressed
data now raises`zlib.error` instead of being silently ignored.
- gh-152817: `sqlite3` : Disallow removing the`row_factory` attribute
of a cursor to prevent a crash on a query.
- gh-156124: Fix a crash in the free-threaded build when deleting the `contents` attribute of a`ctypes` pointer.
- gh-156101: Fix `sqlite3.Cursor.arraysize` being set to 0 if the
assigned value is too large. The attribute is now left unchanged if the
assignment fails.
- gh-156100: Fix crashes in `sqlite3.Connection` when deleting the`autocommit` attribute or setting it to an
integer which does not fit in C long. Both now raise an
exception.
- gh-156099: Fix a crash when deleting the `keylog_filename` attribute of`ssl.SSLContext` . It now raises`AttributeError` .
- gh-156067: Fix error handling in the `zoneinfo` accelerator module
when a transition index is`-1` or a TZ stringâs`__bool__` raises.
- gh-155997: Fix `concurrent.interpreters.list_all()` .  It failed if an
interpreter was destroyed during the call, in particular by a garbage
collection which finalized the object owning the last reference to it.
- gh-155941: Fix `asyncio.start_server()` when a plain-function*client_connected_cb* raises: the error is now reported like the coroutine
case and the transport is closed, instead of leaving the connection open
forever (which also made`asyncio.Server.wait_closed()` hang).
- gh-155843: `hmac` : ensure that HMAC objects are properly initialized
to prevent rare crashes on allocation failures. Patch by BÃ©nÃ©dikt Tran.
- gh-155835: `hashlib` : Fix data races when accessing`digest_size` and`block_size` on
SHA-3 objects. Patch by BÃ©nÃ©dikt Tran.
- gh-155830: `_blake2` : Ensure that BLAKE-2 objects are correctly
initialized before being tracked by the garbage collector. Patch by
BÃ©nÃ©dikt Tran.
- gh-155782: `ssl` : prevent a crash in`group()` for
session-less objects on OpenSSL 3.2 and later. Patch by BÃ©nÃ©dikt Tran.
- gh-155811: Add a sequence counter to GC statistics to prevent `_remote_debugging` returning inconsistent snapshots caused by
non-atomic reads. Patch by Maurycy PawÅowski-WieroÅski.
- gh-155717: `multiprocessing` âs default start method is now
âspawnâ instead of`"forkserver"` when there is no writable temporary directory, such as on a read-only file
system. Previously starting a process failed with an`OSError` there.
Patch by BÃ©nÃ©dikt Tran.
- gh-155775: Fix a regression in `imaplib` introduced in the fix for
gh-63121: refreshing the capabilities after a successful`login()` or`authenticate()` consumed the`CAPABILITY` response, so it could no longer be read with`response()` .
- gh-113318: Fix crashes when deleting an attribute whose setter is generated by Argument Clinic and is not prepared for deletion, among them `frame.f_trace_opcodes` and the`context` ,`owner` and`session` attributes of`_ssl._SSLSocket` . Deleting such attribute now
raises`AttributeError` .
- gh-150449: Fix `sqlite3.Blob` slices with a negative step. Reading
or writing such a slice raised`SystemError` . It now raises`ValueError` .
- gh-155336: Fix error handling in `socket.gethostbyaddr()` and`socket.gethostbyname_ex()` when hostname resolution fails.
- gh-155426: Make sure that the pure Python fallback of `hmac.digest()` rejects keys that are neither`bytes` or`bytearray` .
Previously, an`AttributeError` was raised instead of`TypeError` . Patch by BÃ©nÃ©dikt Tran.
- gh-102475: Fix `os.path.realpath()` on Windows: the unresolved part of
the path is now appended, not joined, so a file name which looks like a
drive (e.g.`spam:eggs` ) no longer discards the resolved part.  A path
relative to another drive is now resolved against the root directory of
that drive.
- gh-89760: Fix `os.path.realpath()` on Windows: the`\\?\` prefix is
no longer stripped from a volume GUID path, which made the result invalid.
- gh-84419: Fix `os.stat()` on Windows: trailing dots and spaces, which
are ignored by the operating system, are no longer taken into account when
the execute permissions are guessed from the file extension.
- gh-155319: `warnings.warn_explicit()` now displays the source line
taken from the loader of the module whose globals are passed as*module_globals* .  It also no longer raises`IndexError` if*lineno* is out of the range of the module source.
- gh-123011: `warnings.warn_explicit()` no longer emits a spurious`DeprecationWarning` or raises`ImportError` when it is called
with the globals of the`__main__` module.
- gh-69370: **python -m inspect --details** no longer fails with`UnicodeEncodeError` if the path of the module contains characters
unencodable in the encoding of`sys.stdout` .  Such characters are
now escaped with backslashes.
- gh-69371: Fix `pydoc` for modules whose path contains undecodable
bytes.`pydoc.writedoc()` and the pydoc HTTP server no longer fail
with`UnicodeEncodeError` : the file URL is now percent-encoded using
the filesystem encoding, and characters unencodable in the generated HTML
page are escaped with backslashes.
- gh-154855: Fix `curses.window.instr()` returning one character too few
when the`curses` module is built against a curses library that
counts the terminator in the requested length, such as the NetBSD one.
- gh-154594: Fix `copy.deepcopy()` so that an object whose deep copy is`None` is still memoized. Patch by tonghuaroot.
- gh-154523: Fixed data-race when calling `io.TextIOBase.detach()` in
free-threaded build.
- gh-135683: `logging.FileHandler` and`logging.handlers.WatchedFileHandler` now honor`logging.raiseExceptions` for errors that occur while opening the
file, like for other errors during logging.
- gh-82535: `logging.handlers.SysLogHandler` no longer fails if the
address cannot be resolved when the handler is created. The address is
resolved again when a record is emitted.
- gh-153970: Calling `str()` on a`subprocess.CalledProcessError` no longer raises`TypeError` when its`returncode` is not an
integer, such as`None` .
- gh-154194: Fix the sampling profiler dropping entire samples when a non-fatal read fails; frames now keep any readable metadata, and long funcnames and filenames are truncated instead. Patch by Maurycy PawÅowski-WieroÅski.
- gh-153005: `concurrent.interpreters.Queue.get()` and`concurrent.interpreters.Queue.put()` now compute their`timeout` deadline from`time.monotonic()` instead of the wall clock, so
adjusting the system clock during the call no longer makes them over- or
under-wait.
- gh-154062: Stop normalizing differential flamegraph baselines to the duration of the current profile.
- gh-81241: Fix `OSError` when importing`ctypes` from a
statically linked interpreter.
- gh-153838: Skip non-regular files in the `profiling.sampling` heatmap
exporter instead of reading them unconditionally.  Patch by tonghuaroot.
- gh-141276: Fix duplicate `SyntaxWarning` messages when importing
source modules from ZIP archives.`zipimport.zipimporter.get_filename()` , and therefore`find_spec()` , no longer compile source
modules. A`SyntaxError` in a zipped source module is now raised when
the module is executed rather than when it is found, matching the behavior
of filesystem-based loaders.
- gh-153603: Fix a crash in the ISO-2022 decoders when decoding a byte after an unknown charset designation is set via the decoderâs `setstate` method. Patch by tonghuaroot.
- gh-153539: Fix a crash in the C implementation of `io.TextIOWrapper.tell()` when the decoderâs`getstate` method
triggers a reentrant seek, or when another thread seeks the same stream
concurrently. Patch by tonghuaroot.
- gh-152754: Fix a crash when the same `os.scandir()` iterator is used
concurrently from multiple threads.  It no longer releases the directory
handle while another thread is reading from it.  Sharing an iterator
between threads remains subject to race conditions: which entries each
thread receives is unspecified.
- gh-153364: Make frame, coroutine and task-waiter walks iterative and bounded, avoiding potential hangs and stack overflows. Fix asyncio task inspection when awaited-by sets contain removed entries. Patch by Maurycy PawÅowski-WieroÅski.
- gh-149816: `hashlib` : fix a crash arising from a race condition when
looking up an OpenSSL hash algorithm by name the first time. Patch by
BÃ©nÃ©dikt Tran.
- gh-151292: Fix `profiling.sampling --binary` leaving unreadable profile
files when the binary format reaches a size limit. Preserve collected
samples when the writer can still finalize safely. Patch by Maurycy
PawÅowski-WieroÅski.
- gh-152721: Fix quadratic replay time in the `profiling.sampling` binary reader when a profileâs run-length-encoded samples alternate thread
status.
- gh-151126: Fix a crash caused by failing to set `MemoryError` on
allocation failure when passing`ctypes.Structure` or`ctypes.Union` instances by value to ctypes foreign functions.
- gh-152692: Fix `annotationlib.type_repr()` returning a string such as`None.list.append` for bound built-in methods, whose`__module__` is`None` .  It now returns the qualified name, as it already did for other
builtins.
- gh-152490: Fix `TypeError` when formatting a`SyntaxError` raised with`NotImplemented` as its argument, which caused
unexpected REPL exit and IDLE Shell restart.
- gh-152373: Fix the `pdb``until` command not stepping over a list
comprehension when a breakpoint is set on it. Patch by tonghuaroot.
- gh-152204: Fix the pure-Python implementations of `datetime.date.fromisoformat()` ,`datetime.time.fromisoformat()` and`datetime.datetime.fromisoformat()` silently accepting some
malformed ISO 8601 strings, such as non-ASCII digits or a sign in a
fixed-width field (for example`'2020+12'` or`'20201212T0102Ù£Ù¤'` ).
Each field is now required to be exactly*N* ASCII digits, matching the C
implementation.
- gh-133998: Fix `tarfile.open()` in streaming write mode (`w|gz` )
raising`struct.error` when given an*mtime* outside the range of the
gzip headerâs 32-bit timestamp field.  Out-of-range values are now stored
as`0` , matching`gzip` .
- gh-117807: Fix `mimetypes` initialization from MIME map files
containing invalid UTF-8 bytes.
- gh-143008: Fix race conditions when re-initializing a `io.TextIOWrapper` object.
- gh-145856: Fix `plistlib.dumps()` and`plistlib.dump()` so that`skipkeys=True` together with`sort_keys=True` correctly drops
non-string keys when the dictionary contains a mix of string and
non-string keys.  Previously the sort ran before the filter and raised`TypeError` .
- gh-143376: `concurrent.interpreters` : fix a crash when internal
cross-interpreter channel ends have incorrect channel IDs. Patch by
BÃ©nÃ©dikt Tran.
- gh-146065: Fix a crash in asyncio.Future iterator when send() is called after throw() or close().
- gh-143008: Fix crash in `io.TextIOWrapper` when reentrant`io.TextIOBase.detach()` is called reentrantly from the underlying
buffer.
- gh-129605: Improve exception handling when computing suggestions in traceback.py. This fixes a bug that could cause PyREPL to exit unexpectedly.
- gh-102967: A bug in `doctest._SpoofOut.truncate()` was causing None
to be passed to`StringIO.seek()` when no size was given. A simple
fix skips the seek call when no size is given so the buffer can be
truncated from the current position.
- gh-82039: `http.cookiejar.FileCookieJar.load()` now checks the first,
format signature line in a case-insensitive manner. Patch by Ashley
Harvey.
- gh-70758: Creating a new `turtle` screen after the previous one was
closed no longer raises`turtle.Terminator` on the first turtle command.
- bpo-44012: The `exploded` attribute of`ipaddress.IPv6Address` and`ipaddress.IPv6Interface` now supports addresses with a scope
ID (link-local addresses). Previously the former raised`AddressValueError` and the latter omitted the scope ID.
- gh-83869: Fix `tarfile` reading an archive with a GNU sparse 1.0
member whose size is set in the pax extended header. The offset of the
next header was computed from the offset of the data, which is already
past the sparse map, and from the size of the member, which can be the
apparent size of the sparse file. All following members were unreachable.
- gh-61366: `http.cookiejar.MozillaCookieJar` now reads session
cookies written by curl and Wget, which use`0` in the expiration time
field. Contributed by JÃ©rÃ©mie Detrey.

### DocumentationÂ¶

- gh-142083: Document the `'w'` type code in the`array.array` docstring, so that it lists the same type codes as`array.typecodes` and the`array` module documentation.
- gh-107689: Added docstrings to abstract ctypes.

### TestsÂ¶

- gh-159293: Restore the recursion limit after the free-threading recursion-limit race test to avoid failures in subsequent tests run in the same process.
- gh-159211: The iOS testbed no longer gathers simulator diagnostics when a test run fails.
- gh-157628: In regrtest, only consider that a test leaks if all test runs leak at least one file descriptor. For example, ignore â[0, 1, -1] file descriptors, sum=0â deltas, instead of reporting a leak. Patch by Victor Stinner.
- gh-140550: Enable limited C API tests on Free Threading in test_cext and test_cppext: test the `Py_TARGET_ABI3T` macro. Patch by Victor
Stinner.
- gh-75876: A test decorated with `bigmemtest()` now runs
in a subprocess if it really allocates the memory it asks for (that is, if
the`-M` option is used), so that the memory it uses and the address
space it fragments are released when it ends.  A dummy run stays in the
process.  The separate memory watchdog process is no longer needed: the
parent process watches the memory usage of the subprocess.
- gh-155411: Fix `test.support.subTests()` for asynchronous test
methods.  They were wrapped in a synchronous function, which discarded the
coroutine without awaiting it, so the test silently did not run at all.
- gh-132581: The list of tests which altered the execution environment now includes the reasons why the environment was considered altered, for example an unraisable exception or a modified `sys.path` .  The final
result no longer repeats the same state twice (like```
ENV CHANGED then ENV
CHANGED
```
).
- gh-155109: Add `test.support.run_with_limited_c_stack()` and use it in
tests that exhaust the C stack with a fixed number of recursive calls, so
that their outcome no longer depends on the C stack size.

### BuildÂ¶

- gh-143632: Fix the check that disables `PR_SET_VMA_ANON_NAME` on musl
libc: it tested an unset variable, so it never had any effect and the
build could fail with a redefinition of`struct prctl_mm_map` .
- gh-159759: Fix the `libpython.static` path in**PEP 739**`build-details.json` , which pointed to a file that is not installed when
building with`--enable-shared` , and no longer list a static
library for framework builds, which do not install one.
- gh-159440: The version string reported by `sys.version` no longer
carries a spurious`-dirty` suffix when CPython is built out-of-tree
from a clean git checkout: the build stamp now checks the source tree
instead of the build directory.
- gh-158683: Fix the `Modules/Setup` entry for the`_zstd` module by adding missing source files.
- gh-157142: Fix the link of `Programs/_freeze_module` when extension
modules are built in (`MODULE_BUILDTYPE=static` ) and the test modules
are enabled:`Modules/getpath_noop.c` now defines`_Py_Get_Getpath_CodeObject()` , which`_testinternalcapi` uses.
- gh-155911: Fix curses detection when a curses library is already in `LIBS` . Patch by Shamil Abdulaev.

### WindowsÂ¶

- gh-159880: Fix a key handle leak in `winreg.CreateKey()` ,`winreg.CreateKeyEx()` and`winreg.OpenKey()` when the`winreg.OpenKey/result` audit hook fails.
- gh-157906: Fix an incorrect architecture macro check that prevented CPU prefetching in the free-threaded garbage collector on 32-bit Windows builds.
- gh-157789: Use compiler option `-Og` in case of clang-cl debug builds on
Windows to reduce stack usage. Patch by Chris Eibl.
- gh-72353: Reading from the console on Windows now continues if the read was cancelled by Ctrl+C, but the `SIGINT` handler did not
raise an exception.  Previously the read ended as if at end of file.
- gh-86427: Fix the encoding of the standard streams in the legacy Windows stdio mode ( `PYTHONLEGACYWINDOWSSTDIO` ).  It is now the code page
of the console, as in Python 3.7, not the ANSI code page.
- gh-87587: `os.device_encoding()` on Windows now returns the code page
of any console file descriptor, not only 0, 1 and 2, and returns`None` for other character devices like`NUL` .  The UTF-8 code page is now
reported as`"utf-8"` instead of`"cp65001"` .

### macOSÂ¶

- gh-159739: Propagate errors that occur while converting macOS system proxy settings.

### IDLEÂ¶

- gh-159517: Fix the completion list and the call tip grabbing the focus on macOS with Tcl/Tk 9.1.
- gh-56596: Make IDLE keyboard shortcuts with ascii letters work when Caps Lock is on even when only one of the upper and lowercase versions are defined.
- gh-64007: IDLE no longer applies syntax coloring to the text entered for `input()` in the Shell.
- gh-85560: Fix IDLE failing to find the opening parenthesis when a continuation line inside the parentheses starts with `else` of a
conditional expression or with`yield` .
- gh-75487: Fix saving a truncated number when an integer entry in the IDLE Settings dialog is blanked with the Backspace key.
- gh-74112: Ctrl-C in the IDLE Shell now interrupts blocking calls such as `time.sleep()` and`socket.recv` .
- gh-151471: Colorize the soft keyword `type` in IDLE.
- gh-103089: IDLE no longer hangs when opening a file with very long lines. Only the first 2,000 characters of a line are colorized.
- gh-70331: IDLE no longer fails to start with `python -m idlelib` , and
its user process no longer fails to start, when the current directory
contains user files with the same names as standard library modules that
IDLE imports, such as`random.py` or`tkinter.py` .
- gh-93016: Fix parsing of command line arguments in the IDLE âRunâ¦ Customizedâ dialog on Windows: backslashes are no longer treated as escape characters. Fix also display of the previous arguments in the dialog.
- gh-69365: The IDLE search dialogs now show a regular expression error below the entry instead of in a message box.
- gh-105689: Fix calltips, parenthesis matching and auto-indent in the IDLE Shell after output containing unbalanced quotes or parentheses.
- gh-60402: In IDLE, Tab in a string that is not the start of a file name inserts a tab instead of opening the completion list.
- gh-68453: Fix running IDLE with the -s option together with -c, -r or -: the command or script now runs after the startup file, without an error dialog and without restarting the shell.
- gh-69919: IDLE now reports any exception raised by compiling the source, such as `MemoryError` for too deeply nested source, instead of
hanging the Shell.
- gh-91398: Remove the border around the IDLE Shell sidebar, which was drawn in the window background color and stood out with dark themes.
- gh-71136: Fix a hang of the IDLE Shell when code run under the debugger raises `KeyboardInterrupt` .
- gh-154511: Consolidate IDLEâs mouse wheel handling in `idlelib.util` .`wheel_event` moves there from`idlelib.tree` and joins`x11_buttons` , which tells whether Tk reports wheel rotations to a
widget as`<Button-4>` /`<Button-5>` events, and`bind_wheel` , which
binds whichever events Tk sends.
- gh-156896: Stop deleting tkinter submodules when idlelib.run is imported.
- gh-153480: Fix IDLE failing to start when opening a file which does not exist yet. Fix a traceback when a file open in the IDLE editor is deleted by another program; IDLE now asks whether to close the window, save the file elsewhere, or ignore it.
- gh-66172: IDLE no longer fails to start when a user configuration file is corrupt. The unparsable file is renamed with a â.badâ suffix, default settings are used instead, and a warning lists the affected files.
- gh-90304: Prevent IDLE from failing to start when a font reports a zero width for the `'0'` character.  Such a broken font caused a`ZeroDivisionError` ; the editor now falls back to the configured
width.
- gh-135511: Fix display of `NameError` and`AttributeError` with
multi-line message.

### Tools/DemosÂ¶

- gh-158451: `python-gdb.py` is now able to format an invalid Unicode
string: render invalid characters as`\Uhhhhhhhh` instead of raising an
exception. Patch by Victor Stinner.
- gh-156219: Fix Argument Clinic generating code which reads an optional positional argument which was not passed, if the function has a `**kwds` parameter.
- gh-113318: Fix Argument Clinic for `@getter` and`@setter` in a
preprocessor conditional block. It failed with an internal error. Argument
Clinic now also rejects the accessors of the same attribute with different
C basenames, and the same accessor defined twice, which silently generated
invalid or duplicated entries of`PyGetSetDef` .
- gh-155218: Fix Argument Clinic generating the flags of the optional groups in different order on 32-bit and 64-bit platforms.
- gh-155207: Argument Clinic now supports the `--dry-run` and`--diff` options. They list the files which would be changed, or write a unified
diff of the changes to the standard output, without modifying any file.
- gh-64502: Fix Argument Clinic support of parameters with a default value used together with optional groups. Such parameters were always required in the generated parsing code.

### C APIÂ¶

- gh-159739: Fix a crash in `_PyTuple_Resize()` when resizing a
non-empty tuple to a negative or too large size. It now raises`SystemError` or`MemoryError` respectively.
- gh-159739: `PyInitConfig_SetStr()` now releases a previously
configured value and preserves it if conversion of the replacement fails.
- gh-159739: Fix a reference leak when `_PyTuple_Resize()` fails.
- gh-149044: Fix the `PySlot_END` macro for C++: initialize all
members to avoid compiler warnings. Patch by Victor Stinner.
- gh-158810: Fix `Py_ARRAY_LENGTH()` macro on C++: do not use`__builtin_types_compatible_p()` on C++. Patch by Victor Stinner.
- gh-158451: Fix `_PyUnicodeWriter_WriteLatin1String()` when the
writer buffer kind is UCS-2 or UCS-4. Patch by Victor Stinner.
- gh-157649: Fix `Py_CLEAR` and`Py_SETREF` macros on C++:
implement them using`auto` instead of`decltype()` . Using`decltype()` produced invalid code when clearing/setting an array item.
Patch by Victor Stinner.
- gh-142217: Add tests on the undocumented and deprecated `_Py_Identifier` C API. Patch by Victor Stinner.
- gh-157384: `PyImport_CreateModuleFromInitfunc()` now sets the
package context while calling*initfunc* , so that a single-phase init
submodule created with only the last component of its name gets the full
name from the spec.
- gh-157242: Fix `PyBytesWriter_Resize()` to handle properly memory
allocation failure. Leave the writer unchanged on error, instead of
leaving it in an inconsistent state. Patch by Victor Stinner.
- gh-156101: Donât set the result in error in `PyLong_AsInt32()` ,`PyLong_AsUInt32()` ,`PyLong_AsInt64()` and`PyLong_AsUInt64()` . Leave the result unchanged in this case. Patch
by Victor Stinner.

## Python 3.15.0 finalÂ¶

*Release date: 2026-10-09*

There were no new changes in version 3.15.0.

## Python 3.15.0 release candidate 3Â¶

*Release date: 2026-10-02*

### SecurityÂ¶

- gh-158446: Fix a crash or incorrect output that could occur when formatting a `float` or`complex` with a precision close to
the platformâs`INT_MAX` .`PyOS_double_to_string()` now raises`ValueError` for any precision of that magnitude, regardless of
presentation type or value, as the format string parsers already did for
precisions above`INT_MAX` .
- gh-156793: `asyncio` :`loop.start_tls()` and`loop.create_connection()` now validate the*server_hostname* argument if an`ssl.SSLContext` is passed with*check_hostname* set
to`True` .
- gh-156793: `ssl.SSLContext.wrap_bio()` now validates its*server_side* ,*server_hostname* and*session* arguments similar to`ssl.SSLContext.wrap_socket()` .In particular, a context with `check_hostname` enabled and no*server_hostname* passed to`wrap_bio()` now raises`ValueError` instead of completing a handshake that verified the
certificate chain without verifying the peerâs identity, with no
indication that the check had been skipped.
- gh-157265: In `tarfile` , when extracting a link falls back to
extracting a member of the archive, skip the member when the filter
function returns None when called with the extracted memberâs name
replaced with the linkâs.
- gh-157953: Update bundled libexpat to version 2.8.5.
- gh-157579: Fix a race condition in the cleanup of `tempfile.TemporaryDirectory` . When working around file system
permission errors, files are now removed relative to open directory file
descriptors instead of resolving their path again, so that replacing a
directory of the tree with a symbolic link can no longer make the cleanup
delete files outside of the temporary directory. This addresses**CVE 2026-12345** .
- gh-156293: Fix a crash in `ssl` when an`sni_callback` switches a connection to another`SSLContext` and the context that carries the callback is no
longer referenced by the application. Servers that keep their`sni_callback` context alive (the usual case when it wraps the listening
socket or is stored on the server object) were not affected. This
addresses**CVE 2026-19445** .

### Core and BuiltinsÂ¶

- gh-158140: Resolve lazily imported sibling submodules independently, so accessing one does not import another merely because its declaration came last.
- gh-158364: Fix crash when `sys._current_frames()` or`sys._current_exceptions()` is called while another interpreter is
running.
- gh-158254: Reverse iterators over a `dict` and its views now raise`RuntimeError` if the dictionaryâs keys change during iteration, like
forward iterators, instead of yielding entries for the new keys.
- gh-157914: Revert gh-146064. The linked PyMutex change was causing a significant performance regression in some cases.
- gh-157838: Merge biased reference counts on behalf of threads that are detached instead of waiting for them to attach again, in the free-threaded build.
- gh-157778: Include the line number of the opening brace in the `SyntaxError` for an unclosed f-string or t-string replacement field
when a matching quote on a later line is interpreted as the start of an
unterminated string.
- gh-157757: Fix a lazy `import a.b as c` reading`b` off`a` instead
of importing the module`a.b` .  It now binds the submodule, and raises`ModuleNotFoundError` when no module backs the name.
- gh-155695: Resolved names are now removed from `sys.lazy_modules` more consistently: a lazy import of an already loaded module is no longer
recorded, and reifying`lazy from pkg import attr` now discards the`pkg.attr` entry.
- gh-157378: Fix `SyntaxError.offset` and`SyntaxError.end_offset` for
the âNon-UTF-8 code starting with â¦â error when a non-ASCII character
precedes the invalid byte on the same line. Patch by Shamil Abdulaev.
- gh-156762: Fix undefined behaviour in `operator.methodcaller` : its`tp_clear` slot function returned`void` instead
of`int` , so the garbage collector called it through an incompatible
function type. Patched by Shamil Abdulaev.
- gh-157377: Fix `gc.get_count()` on the free-threaded build resetting
the thread-local allocation counter that schedules automatic garbage
collection. A thread that called it while allocating could prevent cyclic
garbage from ever being collected.
- gh-157247: Fix `sys.activate_stack_trampoline()` allowing activation
from a subinterpreter while the JIT is enabled in the main interpreter.
- gh-156995: Fix `bytearray` sharing its buffer with the single-byte`bytes` object of the same value, so that writing to the bytearray
modified that`bytes` object. This happened with`bytearray.take_bytes()` when exactly one byte remained, and on the
free-threaded build when a bytearray was shrunk to one byte from another
thread.
- gh-156910: Fix a deadlock in the free threaded build when setting `__abstractmethods__` on a type while another thread
holds the type lock.
- gh-156894: Fix the position of syntax errors which cover a range if the line contains non-ASCII characters before the error.
- gh-155525: Fix quadratic-time tokenization of modules containing many f-strings or t-strings.
- gh-156689: Fix an out-of-bounds read in `compile()` and`ast.parse()` when an AST object is passed with`mode='func_type'` .
- gh-156114: Fix a crash in the perf trampoline when a code object has a name or filename that cannot be encoded to UTF-8. Patched by Shamil Abdulaev.
- gh-156126: Fix a crash when importing a module whose name contains characters that cannot be encoded to UTF-8 (such as lone surrogates) while `-X importtime` is enabled.
- gh-155526: Fix spurious `OverflowError` for`abs(nanj)` in case`errno` was previously set to`ERANGE` by some library
call. Patch by Sergey B Kirpichev.
- gh-151464: Exclude invalid token `<>` from`tokenize` output.`SyntaxError` for`<>` now suggests`!=` .
- gh-154719: Trailing whitespace in a t-string interpolation expression is now preserved in `string.templatelib.Interpolation.expression` , up
to the closing`}` or the conversion (`!` ), format (`:` ), or debug
(`=` ) delimiter. Explicit line continuations following a debug`=` remain part of the debug text and are excluded from`expression` .
- gh-152708: Fix AST generation asymmetry where unary positive `+` was
implicitly dropped in pattern match expressions, restoring parsing parity
with unary negative`-` .

### LibraryÂ¶

- gh-157788: Fix `python -m asyncio ps` failing on a process that has a
task with a name longer than 255 characters.
- gh-157660: Fix `profiling.sampling` reporting errors or incorrect
line numbers in free-threaded builds when a thread-local bytecode array
grows or gains entries after being cached.
- gh-157639: Fix a crash in `cProfile` when an external timer
re-enters the profiler. Profiling events raised while the external timer
runs are now ignored.
- gh-157335: Fix out-of-bounds write in `mmap.mmap.__setitem__` that could
occur when converting the index or the assigned value (via`__index__()` for a single item, or via the buffer protocol
for a slice) resized or closed the mmap object during the assignment.
- gh-128509: `marshal.load()` and`marshal.loads()` can now get
1-byte string singletons. Patch by Victor Stinner.
- gh-157301: Fix `asyncio.Task` hanging the event loop when an eager
start fails to enter the taskâs context.
- gh-157213: Fix `asyncio.gather()` leaving stale await-graph edges on
children that outlive it.
- gh-156002: `zipfile` again reads members through a third-party
decompressor installed by monkey-patching the private`_get_decompressor()` to return an object that only implements old
BZ2Decompressor API from Python 3.3. Note that decompressors without`needs_input` and two-argument`decompress()` are vulnerable to**CVE 2026-15310** .
- gh-157159: The usage message printed by `python -m tabnanny` when no
arguments are given now suggests`python -m` instead of the source file.
- gh-157058: `asyncio.wait_for()` with a non-positive timeout now
records the waiter in the call graph.
- gh-156933: Fix incorrect integer return values from `ctypes` callbacks on some platforms, such as s390x.
- gh-156988: `asyncio.print_call_graph()` no longer truncates the call
stack at a synchronous generator.
- gh-156970: Set the docstring of `unittest.mock.Mock.return_value` .
- gh-156924: `annotationlib.ForwardRef.evaluate()` now resolves
lazy import proxies found in the namespace. With`FORWARDREF` , a lazy import that fails to
resolve now results in a`ForwardRef` instead of the`lazy import proxy` , and with`VALUE` the underlying exception is
propagated.
- gh-127057: Fix `asyncio.ProactorEventLoop` UDP transports so that a`ConnectionResetError` raised by`WSARecvFrom` no longer stops the
read loop.
- gh-156512: Fix `asyncio` losing a connection twice when`resume_writing()` closes the transport.
- gh-155596: Fix `pprint.pprint()` and`pprint.pformat()` with`expand=True` not honouring*width* for nested values.  The width of the`'key':` prefix was not counted when deciding whether a value fitted on
the current line, so values under long keys could overflow*width* instead
of being expanded.
- gh-155433: Fix `asyncio.TaskGroup` losing outside cancellation
after`cancel()` .
- gh-155418: Fix `asyncio.TaskGroup` hang when a task created by`asyncio.eager_task_factory()` cancels the group before suspending.
- gh-155143: Fix `asyncio.shield()` leaking the calling task via the
await-graph and callbacks when called on a future that never resolves.
- gh-154470: Fixed a spurious `^J` printed at the end of every source line
by the`list` command of`pdb` when the output is colorized (for
example when attaching to a running process). The source line is now
stripped before it is colorized, like the`where` command already did.
- gh-154335: The `profiling.sampling` profiler now respects`NO_COLOR` ,`FORCE_COLOR` , and`PYTHON_COLORS` and avoids colored
output when not writing to a terminal.
- gh-154090: Record the profiling mode in Tachyon binary profiles and reject differential comparisons whose known modes do not match.
- gh-152907: Restore cooked-mode terminal output flags around `PyOS_InputHook` callbacks in the new REPL
(`_pyrepl` ), so that output written by an input hook (for example a
GUI toolkit event loop) is no longer emitted with`OPOST` disabled and
keeps its carriage returns.
- gh-151179: Fix a pidfd leak in `_PidfdChildWatcher` on Linux: the
watcher no longer leaks the process file descriptor when`waitpid()` fails with an error other than`ChildProcessError` .

### DocumentationÂ¶

- gh-57491: Reduce the depth of the table of contents in the EPUB documentation from three levels to two, making it much shorter and easier to navigate in e-book readers.

### TestsÂ¶

- gh-158051: The iOS testbed has been updated for compatibilty with Xcode 27.

### BuildÂ¶

- gh-158010: Update Android and iOS build scripts to use OpenSSL 3.5.9.
- gh-157549: Use `sysconf(_SC_PAGESIZE)` instead of`getpagesize()` in`remote_debug.h` so`_remote_debugging` builds on Darwin with`-Werror=implicit-function-declaration` .
- gh-152023: Update Android builds to SQLite 3.53.4. Additionally, enable the `median()` and`percentile()` functions.

### WindowsÂ¶

- gh-158010: Updated Windows builds to use OpenSSL 3.5.9.
- gh-157368: Fix truncated `sys.version` in the case of clang-cl
versions 22 or newer on Windows. Also fixes the Windows on Arm case, where
the Microsoft compiler was reported instead of clang. Patch by Chris Eibl.
- gh-157212: Fix `_Py_ThreadId()` compilation on Windows ARM64 with MinGW
toolchains, where`__getReg()` is unavailable. The TEB address is now
read directly from`x18` . Patch by Christian Aurich.
- gh-152023: Update Windows builds to SQLite 3.53.4. Additionally, enable the `median()` and`percentile()` functions.

### macOSÂ¶

### IDLEÂ¶

- gh-158067: Fix the IDLE File Open dialog listing text files before Python files when opened from the Shell or an Output window.
- gh-75512: Output from a thread printed in the IDLE Shell while a statement is being entered no longer takes over the prompt of the statement.
- gh-89544: Fix IDLE hanging after Restart Shell while receiving a large output from the user process.
- gh-59568: Fix âSave Asâ and âOpenâ dialogs in IDLE for a file whose name starts with `~` on Windows and X11 with Tcl/Tk older than 9.
- gh-69004: The Debugger item of the IDLE Debug menu is now disabled while user code runs, like Stack Viewer.
- gh-93966: The highlighting of a syntax error in the IDLE Shell is now removed when the next statement is entered, not only when the next syntax error occurs.
- gh-75234: Fix editing help sources and key bindings in the IDLE Settings dialog after selecting them with the keyboard.
- gh-55646: Fix IDLE crash at startup when the user configuration contains an invalid key binding, such as `<Alt-Key-up>` instead of`<Alt-Key-Up>` .  The invalid binding is now ignored with a warning.

### C APIÂ¶

- gh-156939: Fix `PyBytesWriter_FinishWithSize()` to return an empty
bytearray when`_PyBytesWriter_CreateByteArray()` is used, instead
of returning an empty bytes string. Patch by Victor Stinner.
- gh-156939: Fix `PyBytesWriter_Format()` error handling: restore the
writer to its previous state on error. Patch by Victor Stinner.
- gh-140550: Fix the `PyABIInfo_VAR` macro: remove redundant`;` .
Before,`PyABIInfo_VAR(abi_info);` code added two`;;` which is
illegal in C++03. Patch by Victor Stinner.

## Python 3.15.0 release candidate 2Â¶

*Release date: 2026-09-01*

### SecurityÂ¶

- gh-156723: Update bundled libexpat to version 2.8.4.
- gh-156002: Bound the amount of data `zipfile` decompresses per read
for members compressed with bzip2, LZMA, or Zstandard, matching the
existing limit for deflate. A small archive member could previously expand
into an unbounded allocation even when read in small chunks.
- gh-155999: Fix the `tarfile``tar` and`data` extraction filters
creating directories outside the destination for members whose name leaves
the destination and returns to it, such as`../evil/../dest/sub/file` .
The containment check used the resolved path, but intermediate directories
were created from the name as given.
- gh-155292: Change the `stringprep` module and`encodings.idna` codec to not consider Unicode codepoint attributes beyond those defined in**RFC 3454** .
- gh-155694: Fix **CVE 2026-15806** by scoping`HTTPPasswordMgr` credentials to the URL scheme,
preventing credentials stored for an HTTPS URL from being used for a
matching HTTP URL, while URIs without a scheme continue to match any
scheme.

### Core and BuiltinsÂ¶

- gh-155725: `tracemalloc` no longer acquires the GIL nor
creates a temporary thread state to trace memory allocations: traceback
frames now store plain UTF-8 strings instead of Python str objects, and
tracebacks are captured using the Python thread state already associated
with the calling thread, even if it is not attached. Threads without a
Python thread state record the traceback as`<unknown>` .
- gh-148750: `encodings` is no longer frozen, due to issues importing
submodules when using a zipped standard library.
- gh-155254: Avoid warning about missing standard library when using _pth files.
- gh-155978: Fix a memory leak in the free-threaded build when setting or deleting a special method (such as `__repr__` ) on a class that has many
subclasses.
- gh-155752: Fix a crash when a `types.GenericAlias` argument gains a`__typing_subst__` hook after the alias parameters have been cached.
- gh-129752: Donât update adaptive counters in the free-threaded build when thread-local bytecode is disabled ( `-X tlbc=0` ). Patch by Donghee Na.
- gh-155515: Track the internal HAMT iterators, which back iteration over a `contextvars.Context` , with the garbage collector.  A reference
cycle running through such an iterator was never collected, leaking the
whole context it iterated over.
- gh-155400: Fix a deadlock in the free-threaded build between two threads assigning to a special method of the same class. While applying the type slot updates with the world stopped, only the type lock was prevented from being released; the type dict mutex could still be released and re-acquired, in the wrong order, if the thread blocked on the stop-the-world mutex.
- gh-155363: Fix a leak in the free-threaded build when creating a thread state fails after an internal QSBR slot has been reserved for it. The slot could never be reclaimed, so the QSBR array grew without bound across repeated failures.
- gh-154701: Fix an infinite loop in JIT when a `FOR_ITER` side exit links
an executor back to itself.
- gh-154196: Improve `AttributeError` messages from unresolved lazy
imports. Patch by Bartosz SÅawecki.
- gh-151377: Fix races in free-threaded builds when updating type slots for newly created classes and when removing entries from a base typeâs subclasses dictionary.
- gh-148817: Fold large constant list and set literals used as the iterable of a `for` loop or`in` /`not in` test into a constant`tuple` or`frozenset` , restoring an optimization previously
done by the AST optimizer that was lost when constant folding moved to the
CFG.
- gh-85260: `compile()` now raises`ValueError` instead of crashing
on a debug build if an identifier field of an AST node (such as the name
of a function, a class, an imported module or a caught exception) is`"None"` ,`"True"` or`"False"` .

### LibraryÂ¶

- gh-156523: Fix `asyncio.as_completed()` not recording the awaiting
task in the call graph.
- gh-156408: Fix `asyncio.print_call_graph()` raising`AttributeError` when called on a task that has already finished.
- gh-156404: Restore compatibility with `.pth` files generated before
Python 3.15 in the`site` module. Inject the`fullname` variable in
the frame which executes pth code.
- gh-156353: Fix `configparser` parsing when using whitespace in*delimiters* .
- gh-156112: `_ios_support` no longer fails to import when`ctypes.util.find_library()` cannot resolve the ObjC runtime through
the dyld shared cache, as is the case on iOS 13. The runtime is now loaded
by name, which dyld resolves against the cache directly.
- gh-155974: Fix a regression in Python 3.15: `addstr()` ,`addnstr()` ,`insstr()` and`insnstr()` again restore the window attributes when
the write fails, instead of leaving the temporary*attr* applied.
- gh-155952: Fix the signatures of `sqlite3.Connection.execute()` ,`warnings.warn()` and`locale.setlocale()` which rendered`<unrepresentable>` instead of the correct defaults for parameters.
- gh-155888: Fix `asyncio.WriteTransport.writelines()` hanging the
transport when the last data chunk is empty.
- gh-155869: Fix `reorganize()` in`dbm.dumb` failing to persist
updated value offsets, which could cause data loss after reopening the
database.
- gh-155781: Fix a possible crash in free-threaded builds when the `sqlite3` converter registry is modified concurrently while a cursor
builds its row cast map.
- gh-155702: Fix `sqlite3.Blob` slice assignment with a step. It
patched the bytes object read from the blob, which for a single byte is an
immortal singleton, so that the value of that byte was changed in the
whole process.
- gh-155468: Fix `netrc` to distinguish empty quoted tokens from
end-of-file, so malformed files no longer cause the remaining content to
be silently ignored.
- gh-155519: Avoid a data-race in free-threaded builds when reading and writing context variables from different threads.
- gh-155009: Fix `argparse.ArgumentParser` to preserve the program
name from`sys.argv[0]` when a named module is executed as the main
program without replacing`sys.argv[0]` .
- gh-155063: Bump the version of pip bundled in ensurepip to version 26.2.1
- gh-153711: On macOS, add run-time checks around the syscalls *pipe2 (2)* and*dup3 (2)* , in addition to the existing
build-time checks. This means that Python built on macOS 27 (where these
calls are available) can run on macOS 26 (where they arenât).
- gh-154871: Fixed a crash in `asyncio.Task.get_context()` when called
on an uninitialized task.
- gh-154568: Fix `array` unpickling of little-endian float16.
- gh-154566: Fix `array.array.byteswap()` corrupting data for`'Zd'` (complex double) arrays with more than one element: the 16-byte item loop
advanced the buffer pointer by only 8 bytes per iteration, causing items
after the first to be scrambled.
- gh-154467: Fixed `pdb` remote attaching (`python -m pdb -p PID` )
sending an empty prompt to the client instead of`(Pdb)` when the target
process has an interactive terminal.
- gh-153772: `isinstance()` checks against`collections.abc` classes such as`Mapping` no longer raise`AttributeError` when the instance has no`__class__` .  The
abstract base class machinery now falls back to the objectâs type in that
case, matching the behaviour of the built-in`isinstance()` .
- gh-154086: Store per-thread sample counts in Tachyon flamegraphs so filtering a thread updates frame widths and totals.
- gh-154060: Store measured duration and sampling rate in Tachyon binary profiles so they remain available in replayed flamegraphs.
- gh-154085: Prevent differential flamegraphs from duplicating self time across line nodes for the same function.
- gh-154088: Make Tachyon count and render elided stacks consistently in differential flamegraphs.
- gh-154089: Do not show unavailable sampling efficiency statistics in replayed Tachyon flamegraphs.
- gh-153158: Remove the erroneous *width* argument of`calendar.HTMLCalendar.formatmonthpage()` , which could drop the year
from the heading.
- gh-113329: Fix `OSError` being raised when trying to run doctests on
a class objects in the REPL. Patch by Sten Wessel.

### BuildÂ¶

- gh-156369: Update Android and iOS build scripts to use OpenSSL 3.5.8.
- gh-156115: iOS simulator builds are now configured with `-mios-simulator-version-min` instead of the deviceâs`-mios-version-min` . The device flag made the linker reject libraries
resolved from the simulator SDK, so library detection silently failed.

### WindowsÂ¶

- gh-156369: Updated Windows builds to use OpenSSL 3.5.8.

### macOSÂ¶

- gh-156369: Update macOS installer to use OpenSSL 3.5.8.

### Tools/DemosÂ¶

- gh-154059: Fix the time units in Tachyon flame graph tooltips by accounting for the sampling interval when converting samples to milliseconds.

### C APIÂ¶

- gh-131740: On the free-threaded build, `PyUnstable_GC_VisitObjects()` now also visits frozen objects
(objects moved to the permanent generation by`gc.freeze()` ), matching
the behavior of the default build.

## Python 3.15.0 release candidate 1Â¶

*Release date: 2026-08-04*

### SecurityÂ¶

- gh-152674: The `xml.etree.ElementTree.Element` methods`findall()` ,`iterfind()` and`find()` avoid quadratic behavior when
using XPath index predicates (`[1]` ,`[last()]` ,`[last()-N]` ) on
XML documents with many same-tag siblings.

### Core and BuiltinsÂ¶

- gh-154902: Fix a crash when `__conditional_annotations__` is rebound to
a non-set object.
- gh-133931: Fix data races when setting attributes of function objects on the free threaded build.
- gh-154775: When matching a complex literal in `case` statements,
an extraneous`+` sign (for example,`1++1j` or`1-+1j` ) is no
longer allowed.
- gh-154709: Fix an out-of-bounds access in reverse dictionary iterators when the underlying dictionary is cleared and modified after the iterator is created.
- gh-154695: Fix `asyncio.Task` raising`AttributeError` when
created with`eager_start=True` and no explicit*loop* argument.
- gh-153809: Fix interpreter crash while deallocating objects of `asyncio.Task` on free-threaded builds. Contributed by Sergey
Miryanov.
- gh-154275: Fix a crash when getting deeply nested `__parameters__` from
a`types.GenericAlias` objects.
- gh-154014: Fix a JIT assertion during interpreter shutdown by initializing `vm_data` fields for cold executors that bypass`_Py_ExecutorInit()` .
- gh-153932: Fix thread safety issue in the `__reduce__` method of`enumerate` .
- gh-153881: Fix potential data race when calling `__getstate__()` under the free-threaded build.
- gh-153570: Fix a use-after-free in `bytearray.take_bytes()` when the
argumentâs`__index__()` method resizes the bytearray. Patch
by tonghuaroot.
- gh-153419: Fix multiple `bytearray` crashes and reference leaks
caused by skipping`__init__()` and broken state setup code.
- gh-153236: Propagate exceptions raised while importing lazy submodules instead of reporting them as missing attributes.
- gh-148874: Ignore interrupts immediately after calling the `__enter__` method of a context menager in a`with` statement. This ensures that the`__exit__` method is always called in a`with` statement.
- gh-150208: Avoid double-quoting string values from `pyconfig.h` in`sysconfigdata` variables.

### LibraryÂ¶

- gh-155063: Bump the version of pip bundled in ensurepip to version 26.2
- gh-154936: Fix the pure Python `json` decoder to report the correct
position for invalid literal control characters in JSON strings.
- gh-154892: Fix a bug in the C accelerator for `zoneinfo` where`datetime.datetime` subclasses returning`-1` for`hour` ,`minute` , or`second` could incorrectly raise a`SystemError` .
- gh-154848: The `pickle` C accelerator now enforces frame boundaries
when unpickling, as the pure Python implementation already did.  An
argument that straddles a frame boundary, or a frame that begins before
the previous one has ended, now raises`pickle.UnpicklingError` instead of being silently read across the boundary.  This prevents the
loaded data from diverging from the`pickletools` disassembly of the
same pickle.
- gh-154885: `os.get_terminal_size()` now checks`isatty` before
calling`ioctl` , which reduces log noise on Android.
- gh-109638: Fix exponential time in `csv.Sniffer.sniff()` for a sample
which contains many quote characters.  A doubled quote character is now
also detected in a field which contains the delimiter or a line break.
- gh-98820: Fix quadratic time in `csv.Sniffer.sniff()` for a sample
which contains quoted fields, in particular for a single column of quoted
fields.
- gh-154738: Fix `ExternalEntityParserCreate()` not propagating the
reparse-deferral setting to the subparser, which left`GetReparseDeferralEnabled()` returning an uninitialized value.
Patch by tonghuaroot.
- gh-93251: Fix `UnicodeDecodeError` in`socket` functions (such
as`getaddrinfo()` and`gethostbyaddr()` ) when the
localized error message of the C library is not UTF-8: decode it from the
locale encoding.
- gh-148468: Fix a regression in `argparse` where colorized argument
help containing format specifiers did not accept string-like proxy
objects.
- gh-142035: Fix incorrect wrapping of `argparse` help text when color
is enabled.
- gh-135736: Fix : `asyncio.TaskGroup` to not wrap a`GeneratorExit` into a`BaseExceptionGroup` if it was raised by
the body of the task group and none of the tasks in the group raised
exceptions.
- gh-154551: Fix `ctypes.util.find_library()` returning`None` in
non-UTF-8 locales.
- gh-79366: Fixed a race condition in `logging` : if a handler was
removed while a record was being emitted, the following handlers of the
same logger could be skipped.
- gh-154189: Fixed a potential use-after-free when calling `functools.partial()` . Now, when invoking a`partial()` object, the stored function, positional arguments, and keyword arguments
are preserved for the duration of the call in case of reentrancy.
- gh-73458: Fix `logging.config.listen()` : it left the caller waiting
for the`ready` event forever if the server could not be started, for
example if the port was invalid or already in use. It now also binds to an
IPv6 address if the host has no IPv4 address, for example if`localhost` is only aliased to`::1` .
- gh-154460: Fix `time.strftime()` and`datetime.datetime.strftime()` returning a wrong ISO 8601 week number
(`%V` ) on OpenBSD.
- gh-154435: Fix `os.posix_fadvise()` and`os.posix_fallocate()` on
DragonFly BSD: they raised`OSError` with a meaningless error code,
because these functions return -1 and set`errno` there.
- gh-154399: Fix `venv` activation in a non-interactive csh:`activate.csh` no longer fails when the`prompt` variable is not set.
- gh-154389: Fix `uuid.uuid1()` on OpenBSD: it returned a version 4
UUID, because`uuid_create()` generates random UUIDs on this platform.
- gh-154324: Fix `os.sendfile()` on illumos: it no longer reports a
successful transfer when the underlying system call failed without writing
any data.
- gh-154307: Fix `tempfile.TemporaryDirectory.cleanup()` on DragonFly
BSD, where removing a file with the`UF_NOUNLINK` flag failed with`EISDIR` instead of`EPERM` .
- gh-154291: Fix `socket.has_dualstack_ipv6()` to return`False` on
platforms such as DragonFly BSD where setting`IPV6_V6ONLY` to 0
silently has no effect.
- gh-154283: On DragonFly BSD, `threading.get_native_id()` now returns a
value that is unique across processes, matching the other platforms.
- gh-154258: Fix a crash in `mmap.mmap.resize()` on NetBSD when growing
a shared anonymous mapping.`resize()` now raises`ValueError` in this case, as it already did on Linux.
- gh-131565: `ctypes.util.dllist()` now works on NetBSD.  It is
implemented in the`_ctypes` extension module so that`dl_iterate_phdr()` reports all loaded shared libraries: on NetBSD it
only reports the link-map group of the calling object, which excluded them
when called through ctypes.
- gh-154225: Fix `os.openpty()` on Solaris and illumos: it no longer
leaves the pseudo-terminal as the controlling terminal of the calling
process.
- gh-154227: Fix `os.posix_openpt()` on OpenBSD, where it rejected the`O_CLOEXEC` flag.
- gh-145030: Fix `asyncio` write pipe transports for named FIFOs on
macOS and Solaris. Unread data sitting in the FIFO made the transport
misinterpret a poll event as the reader disconnecting, wrongly closing the
transport.
- gh-154176: Fix a crash in `locale.strxfrm()` on DragonFly BSD, whose`wcsxfrm()` does not support the zero size.
- gh-154836: Fix `subprocess.Popen.wait()` raising`TypeError` (macOS and other BSDs) or`OverflowError` (Linux) for very large*timeout* values such as`float('inf')` , a 3.15 regression in the new
event-driven wait. Also fix`select.kqueue.control()` masking`OverflowError` for out-of-range timeouts as`TypeError` .
- gh-154053: Fix compilation of the `ssl` module against LibreSSL,
which does not provide`SSL_CTX_set1_sigalgs_list()` and`SSL_CTX_set1_client_sigalgs_list()` . The`set_server_sigalgs()` and`set_client_sigalgs()` methods of`ssl.SSLContext` now
raise`NotImplementedError` on LibreSSL.
- gh-154001: Fix `random.binomialvariate()` raising`ZeroDivisionError` when`random.random()` returns zero.
- gh-153908: Fix data race when calling `repr()` on`itertools.count` under the free-threaded build.
- gh-153896: Deduplicate unhashable args in `typing.Literal` .
- gh-153864: On a wide `curses` build,`curses.window.insch()` now
inserts a non-ASCII byte as the character it encodes in the windowâs
encoding, consistently with`addch()` , instead of its
code point.
- gh-153862: On a wide `curses` build,`curses.window.inch()` now
returns the locale-encoded byte of a non-ASCII character, matching`instr()` , instead of the low byte of its code point.
- gh-146011: Fix a heap-use-after-free in the C implementation of `decimal` when calling`repr()` after deleting the`Context` .
- gh-153761: Fix cancelling `asyncio.loop.sock_accept()` dropping a
pending connection.
- gh-127049: Fix a race condition in `asyncio` on Unix where`asyncio.subprocess.Process.send_signal()` ,`terminate()` or`kill()` could signal an unrelated process
that was recycled onto the PID of the already-reaped child when
ThreadedChildWatcher is used. Patch by Kumar Aditya.
- gh-153404: `urllib.robotparser.RobotFileParser` now silently
ignores a`Crawl-delay` or`Request-rate` value written with
non-decimal digits (such as`U+00B2 SUPERSCRIPT TWO` ) instead of raising`ValueError` and aborting the parse of the whole`robots.txt` file.
- gh-153291: Fix a data race in `readline.get_completer()` and`readline.get_pre_input_hook()` on the free-threaded build: the getters read the stored hook without the critical
section that the corresponding setters hold.
- gh-153133: Fix a socket leak in `asyncio.loop.create_connection()` when the transport cannot be created.
- gh-152431: Fix `asyncio.StreamWriter.start_tls()` to keep the linked`StreamReader` transport in sync with the upgraded transport.
- gh-151955: Allow more types to be used in the `bound` argument to`typing.ParamSpec` and`typing.TypeVarTuple` .
- gh-119710: Fix `asyncio` subprocess`wait()` hanging when the process has
exited but one of its pipes is kept open by an inherited child process (so
the pipe never reaches EOF).`wait()` now returns as soon as the process
exits, regardless of the pipesâ state.
- gh-151022: Fix `_remote_debugging` stack traces for code objects with
large line tables.
- gh-149319: The `asyncio` REPL now ignores`PYTHONSTARTUP` and`PYTHON_BASIC_REPL` when`-E` or`-I` is used.
Patch by Jonathan Dung.
- gh-148665: socket.shutdown function is now enabled for Emscripten builds. However, if the runtime does not implement the shutdown syscall, it will show âFunction not implementedâ error.
- gh-140326: Fix the `asyncio` REPL namespace so that relative imports
no longer resolve against the`asyncio` package and`__file__` is
no longer set.
- gh-103925: Fix `csv.Sniffer.sniff()` for a sample with`\r\n` line
endings in which a quoted field ends a line: a letter could be detected as
the delimiter.

### DocumentationÂ¶

### TestsÂ¶

- gh-76595: Add C API tests for `PyCapsule_Import()` .
- gh-154211: Add `test.support.skip_if_huge_c_stack()` and use it to skip
tests that exhaust the C stack if the stack limit is very large (e.g. on
DragonFly BSD or with`ulimit -s unlimited` ).
- gh-154167: The test runner (regrtest) now restores the default SIGINT handler if it was inherited as ignored, so the test suite no longer hangs when run as a shell background job.
- gh-154144: Fix building the `_testcapi` module on NetBSD.
- gh-152548: Add the `test.support.isolation.runInSubprocess()` decorator to run a test method or`TestCase` subclass in a fresh
interpreter subprocess, isolated from the rest of the test run.

### BuildÂ¶

- gh-154070: Build the `curses` module against a wide-character capable
ncurses even when it is not named`ncursesw` â for example the pkgsrc
ncurses on NetBSD and illumos, or the system ncurses on macOS.  Such a
library previously produced a narrow build.
- gh-138800: Fix library name in python3.pc on Android.

### WindowsÂ¶

- gh-124111: Updated Windows builds to use Tcl/Tk 9.0.4.

### Tools/DemosÂ¶

- gh-154580: Fix `python-gdb.py` raising`UnicodeEncodeError` when
pretty-printing a non-ASCII`str` in a locale whose host charset
cannot encode it, such as any non-ASCII string in the C locale.
- gh-152384: The CPython Pixi packages are now all accessible at the same `Tools/pixi-packages` subdirectory, rather than at`Tools/pixi-packages/{variant}` as before. Variants are now selected not
via subdirectory but via`flags` ; see
https://pixi.prefix.dev/latest/concepts/package_specifications/#extras-and-flags
for usage instructions. The`tsan-freethreading` variant has been
renamed to`tsan_freethreading` , while the`default` ,`asan` , and`freethreading` variants retain their previous names.

### C APIÂ¶

- gh-122931: Allow importing stable ABI C extensions that include a multiarch tuple in their filename, e.g. `foo.abi3-x86-64-linux-gnu.so` .

## Python 3.15.0 beta 4Â¶

*Release date: 2026-07-18*

### SecurityÂ¶

- gh-153030: Fixed quadratic complexity in incremental parsing of long unterminated constructs (such as tags or comments) in `html.parser.HTMLParser` , which could be exploited for a denial of
service.
- gh-152216: Update bundled libexpat to version 2.8.2.
- gh-151987: The `tarfile.TarFile.extract()` method now applies the
given filter when it extracts a link target from the archive as a
fallback.
- gh-151981: In `tarfile` , seeking a stream now stops when end of the
stream is reached.
- gh-151558: Fixed an vulnerability in the `tarfile``data` and`tar` extraction filters where crafted archives could create a symlink
pointing outside the destination directory. This was a bypass of**CVE 2025-4330** .
- gh-150743: `http.client` now limits the number of chunked-response
trailer lines it will read to`max_response_headers` (100 by default),
and the number of interim (1xx) responses it will skip to 100.  A
malicious or broken server could previously stream trailer lines or```
100
Continue
```
responses forever, hanging the client even when a socket
timeout was in use. Reported by`@YLChen-007` via GHSA-w4q2-g22w-6fr4.
- gh-143927: Normalize all line endings (CR, CRLF, and LF) to LF+TAB when writing multi-line configparser values.

### Core and BuiltinsÂ¶

- gh-153298: Fixes a data race in `types.GenericAlias``__parameters__` initialization on free-threading builds.
- gh-153205: Fix a potential `SystemError` during vector calls when
memory allocation fails. A`MemoryError` is now raised instead.
- gh-152682: Fix NULL pointer dereference in `compile()` when a reserved
name (e.g.`__classdict__` ) is used as a type parameter name and memory
allocation fails while formatting the error message.
- gh-152635: Fix a crash caused when running out of memory creating a `_interpchannels` channel. Now a`MemoryError` is correctly
raised.
- gh-152405: Do not expose the internal mapping of `types.MappingProxyType` when performing rich-compare operations
with non-stdlib types. Previously, it was possible to mutate the internal
mapping of a proxy there. Now we pass not the original`dict` instance, but its copy, when dealing with custom types.
- gh-152492: `collections.OrderedDict``update` method can now
accept`frozendict` as an argument.
- gh-152375: Fix undefined behaviour when a `sys.monitoring` callback
raised an exception while the program was following a branch or loop.
- gh-152235: Defer GC tracking of `set.intersection()` ,`set.difference()` ,`set.symmetric_difference()` ,`set.union()` and`set.__sub__` . Patch by Donghee Na.
- gh-152235: Defer GC tracking of a `set` or`frozenset` to
the end of its construction from iterable. Patch by Donghee Na.
- gh-152228: Fix an assertion failure when python is built in a debug mode that happened in `str.replace()` under a limited memory situation.
- gh-151722: `frozendict.fromkeys()` now only tracks the`frozendict` in the garbage collector once the dictionary is fully
initialized. Patch by Donghee Na and Victor Stinner.
- gh-151763: Fixes possible crash on `types.CodeType` deallocation.
- gh-152020: On the free-threaded build, `asyncio.all_tasks()` no longer
loses eager-started tasks when called from a thread other than the one
running the event loop.
- gh-151763: Fix a potential crash in `compile()` ,`exec()` ,`eval()` and`ast.parse()` when an allocation fails: the parser or
compiler could return without setting an exception.
- gh-151912: Fixed a crash in `type()` when selecting a metaclass whose`tp_new` slot is`NULL` . Such metaclasses are now rejected with`TypeError` instead of causing a NULL pointer dereference.
- gh-151773: Fix a crash in `contextvars.ContextVar.set()` when memory
allocation fails.
- gh-151672: Fix an inconsistency where calling `__lazy_import__` with a
string`fromlist` would return a`types.LazyImportType` that
resolves to the named member, rather than the module being imported.
- gh-151619: Fix an issue where using non-module global or builtin namespaces (such as dictionaries passed to `exec()` ) could cause
cached global loads to produce unresolved lazy imports.
- gh-151126: Fix a crash when sharing `memoryview` objects between
interpreters fails due to running out of memory. It now raises a proper`MemoryError` .
- gh-151644: Fix a data race in `sys.setdlopenflags()` and`sys.getdlopenflags()` when called concurrently in the free-threaded
build. The underlying`_PyImport_GetDLOpenFlags` and`_PyImport_SetDLOpenFlags` functions now use atomic load/store
operations.
- gh-151126: Avoid possible crash in `_winapi.c` where a device has no
memory left. Now it properly raises a`MemoryError` . Patch by Ivy Xu.
- gh-151029: On Linux, fix `sys.remote_exec()` unable to find remote
writable memory when`libpython` replaced on disk.
- gh-150459: Fix `SyntaxError` error message for```
from x lazy import
y
```
. Raise`SyntaxWarning` on`from . lazy import x` (with
whitespace between the dots and a module named`lazy` ).
- gh-150858: Fix a data race while changing `__qualname__` of a type
concurrently on free-threaded builds.
- gh-144774: Fix data race in `BaseException` when an exception is
copied while being mutated.
- gh-150411: Fix a data race in the free-threaded build when `gc.get_count()` reads the young generation allocation count while
another thread updates it.
- gh-149689: Fix missing error propagation in parser action helpers when memory allocation fails. Patch by Thomas Kowalski.
- gh-149162: Fix a potential deadlock in `PyUnicode_InternFromString()` and other interning functions in the
free-threaded build when called from C++ static local
initializers.
- gh-148825: Fix build error if specialization is disabled.

### LibraryÂ¶

- gh-153695: Hashing a `sqlite3.Row` that contains an unhashable
value now raises`TypeError` instead of`SystemError` . Patch by
tonghuaroot.
- gh-153658: Fix `sqlite3.Connection.iterdump()` raising`sqlite3.OperationalError` when a table name contains a single quote.
Patch by tonghuaroot.
- gh-85943: Fix `struct` functions raising`BytesWarning` under
the`-bb` command line option when a`str` format is used after
an equal`bytes` format (or vice versa).  The internal format cache
no longer mixes`str` and`bytes` keys.
- gh-151292: Store the sample count in `profiling.sampling` binary
format, as a 64-bit integer, widening from previously used 32-bit integer.
This breaks the existing recordings. Long or thread-heavy profiling
sessions will no longer fail because of`OverflowError` . Patch by
Maurycy PawÅowski-WieroÅski.
- gh-153417: Error messages from `imaplib.IMAP4.select()` and`imaplib.IMAP4.uid()` no longer raise`BytesWarning` under`-bb` when the mailbox or command argument is`bytes` .
- gh-153406: `email.utils.parsedate_to_datetime()` now raises`ValueError` instead of`OverflowError` when the parsed year or
timezone offset is out of range, matching its documented behavior.
- gh-153333: The `readprofile` method of`tkinter.Tk` now reads the
userâs profile scripts using the encoding declared in the file, instead of
the locale encoding.
- gh-153083: Defer GC tracking of an `array.array` to the end of its
construction. Patch by Donghee Na.
- gh-153292: Fix data race in repr of `threading.RLock` in
free-threading build.
- gh-153293: Fix the live sampling profiler TUI keeping stale aggregated opcode statistics after a stats reset.
- gh-143990: A `tkinter.font.Font` created from a named font,
including by`copy()` , now copies its configured
options rather than the options resolved by Tclâs`font actual` ,
preserving a size specified in pixels (a negative size).
- gh-148286: Fix undefined behavior in `compression.zstd.ZstdDecompressor.unused_data` when a complete
frame was decompressed in a single call.
- gh-153210: Fix crash on `array` import under a memory pressure.
- gh-153200: Fix `math.isqrt()` returning an incorrect result for
arguments not less than 2**64 that are instances of an`int` subclass with an overridden comparison operator.
- gh-153068: Fix `cProfile.Profile.enable()` to no longer overwrite
errors from`sys.monitoring` .
- gh-153062: Fix a crash when concurrently iterating an `itertools.tee()` iterator on the free-threaded build.
- gh-153056: Fix `string.Template` raising a spurious`ValueError` when the*pattern* attribute is a compiled regular
expression object, which the documentation allows.  On the free-threaded
build this also occurred as a data race on the first concurrent use.
- gh-143921: Narrow the control character check in `imaplib` commands:
only NUL, CR and LF are now rejected. Other control characters are valid
in quoted strings and can occur in mailbox names returned by the server,
so they are now accepted and sent quoted.
- gh-153037: Fix `ZstdFile` raising`AttributeError` instead of`io.UnsupportedOperation` when
iterating over a file that is not open for reading.
- gh-135661: Fix `html.parser.HTMLParser` : an abruptly closed empty
comment (`<!-->` or`<!--->` ) no longer extends up to a later`-->` in the same`feed()` call.
- gh-152851: Prevent a crash when allocation fails while copying a `BLAKE-2s/2b` object. Patch by BÃ©nÃ©dikt Tran.
- gh-54930: Error responses of `http.server.BaseHTTPRequestHandler` to malformed request lines now include a status line and headers instead
of being sent in the bare HTTP/0.9 style. Only a valid HTTP/0.9 request (a
two-word`GET` request line) now receives an HTTP/0.9 style response.
- gh-119592: Fix `concurrent.futures.ProcessPoolExecutor` stranding
submitted work forever when a worker process exited upon reaching its*max_tasks_per_child* limit after`shutdown()` was called with`wait=False` : a replacement worker is now spawned and the remaining work
executed as documented. If the executor has instead been garbage collected
without`shutdown()` (gh-152967), or a replacement worker cannot be
started, the remaining futures now fail with`BrokenProcessPool` instead of never
resolving. A worker exit racing`shutdown(wait=False)` can also no
longer crash the executor management thread.
- gh-150579: `concurrent.futures` now uses lazy imports for its
executor submodules instead of a module`__getattr__` hook.
- gh-152951: `collections.deque` prevent rare crash when calling`extend` under high memory pressure conditions.
- gh-150880: Normalize non-extended Windows paths before appending the wildcard used by `os.listdir()` and`os.scandir()` , making paths with
trailing spaces behave consistently with other filesystem APIs.
- gh-152068: Fixes a bug when a line was split (particularly on macOS Terminal.app) in the middle of a colorized keyword, causing the ANSI Color Reset sequence (ESC0m) to not be properly printed, causing the output to be colored when it shouldnât
- gh-152849: Out-of-range float and integer timestamps now raise `OverflowError` with the same message. Patch by tonghuaroot.
- gh-152847: Reject a POSIX TZ transition rule with non-digit characters in the day-of-year field in the pure-Python `zoneinfo` parser. Patch by
tonghuaroot.
- gh-152718: Fix unbounded memory allocation in the `profiling.sampling` binary profile reader when a file declares more
string or frame entries than it contains.
- gh-108280: Connecting `imaplib` to a server that does not send a
valid IMAP4 greeting (for example a POP3 server answering on the IMAP
port) now raises an error reporting the serverâs response instead of`imaplib.IMAP4.error: None` .
- gh-63121: `imaplib` now refreshes the cached capability list after a
successful`login()` or`authenticate()` , using the`CAPABILITY` response
sent by the server or, if none was sent, by querying it, so that
capabilities that become available only after authentication (such as`ENABLE` on Gmail) are recognized.  Capabilities advertised in the
server greeting are now also used, avoiding a redundant`CAPABILITY` command.
- gh-88574: `imaplib` no longer fails when a server sends a spurious
blank line after the counted data of a literal, including after a literal
that terminates a response (such as a mailbox name returned by`LIST` ).
Such blank lines are now skipped without swallowing the following line.
- gh-152502: Detect the `curses` mouse interface
(`getmouse()` , the`BUTTON*` constants, and others) with a
configure capability probe or library macros instead of gating it on
ncurses-specific macros.  It is now also available with other curses
implementations that provide it, such as NetBSD curses and PDCurses (the
latter underpins`windows-curses` ).
- gh-151842: Fix a crash in `_interpreters.capture_exception()` when`MemoryError` happens. Patch by Amrutha Modela.
- gh-40038: `imaplib` now again quotes command arguments when
necessary, for example mailbox names containing a space.  Such quoting was
inadvertently disabled when the module was ported to Python 3, and the
arguments are now quoted according to the**RFC 3501** grammar.  For
backward compatibility, an argument already enclosed in double quotes is
left unchanged, so code that quotes arguments itself keeps working.
- gh-50966: Fix unbounded recursion in `turtle` when a mouse event
handler that moves the turtle is reentered while the screen is being
redrawn, for example with`screen.ondrag(turtle.goto)` .  This could
previously crash the interpreter.
- gh-152569: Fix `asyncio.wait()` leaking waiting tasks via the
await-graph when racing a future that never resolves. The waiting task is
now discarded from every futureâs`awaited_by` set once`wait()` returns, even for pending futures.
- gh-110357: Importing `hashlib` no longer logs an error to stderr when
a normally guaranteed hash algorithm is unavailable in the current runtime
(for example under an OpenSSL FIPS configuration or a build using`--without-builtin-hashlib-hashes` ). Code that actually uses the missing
algorithm still gets a clear`ValueError` .
- gh-152356: Fix a hang in `profiling.sampling run --blocking` on Windows
when the target process exits. The profiler now finalizes binary profiles
instead of continuing to sample the exited process.
- gh-78335: Update the docstrings of `tkinter` and`tkinter.ttk` widget classes to list all supported widget options, including options
added in Tk 9.0 and 9.1.`tkinter.Menubutton` and`tkinter.Message` previously had no option list at all.
- gh-151126: Fix two crashes in `tkinter` and`socket` modules
initialization under a memory pressure. Sets missing`MemoryError` .
- gh-133031: `curses.textpad.Textbox` now enters and reads back the
non-ASCII characters of an 8-bit locale encoding, instead of mangling them
with a 7-bit mask.
- gh-71880: `curses.textpad.Textbox` now lets the lower-right cell of
the window be edited.  Writing it with`addch()` would
move the cursor past the end of the window, raising an error and scrolling
a scrollable window, so it is now written with`insch()` , which keeps the cursor in place.
- gh-83274: Deallocating a `tkinter` application from a thread other
than the one it was created in no longer crashes the interpreter.  The
underlying Tcl interpreter is leaked instead, and a`RuntimeWarning` is reported.
- gh-152305: Fix the pure-Python `datetime.time.strftime()` implementation raising`AttributeError` for the year directives.
Patch by tonghuaroot.
- gh-88758: `tkinter.Misc.focus_get()` ,`focus_displayof()` ,`focus_lastfor()` and`winfo_containing()` now return`None` instead of raising`KeyError` when the widget was not created by`tkinter` (for example a torn-off menu).
- gh-38464: `tkinter.Misc.nametowidget()` now resolves the
auto-generated names of cloned menus (a menu used as a menubar or a
cascade) back to the original widget.
- gh-152248: Make the C and pure-Python `zoneinfo` parsers validate
POSIX TZ abbreviations consistently, rejecting unquoted abbreviations with
non-letter characters and empty quoted abbreviations. Patch by
tonghuaroot.
- gh-80937: Fix a memory leak in `tkinter` when a Tcl command created
with`createcommand` was not explicitly removed before the interpreter
was deleted.  The command no longer keeps the interpreter alive through a
reference cycle.
- gh-152246: Fix the pure-Python `zoneinfo` parser accepting an invalid
POSIX TZ transition rule with a non-period separator. Patch by
tonghuaroot.
- gh-151496: Fixed `profiling.sampling --gecko` with`--async-aware` by
flattening async task stacks before generating Gecko samples.`--binary` now rejects`--async-aware` until the binary format supports async task
data.
- gh-139816: Fix a hang in `tkinter` on interactive Python built
without`readline` .  An exception raised in a callback no longer
causes the event loop to stop and wait for the user to press Enter;
pending callbacks now keep running until input is actually available on
stdin.
- gh-139145: Fix a busy loop in `tkinter` on interactive Python.  When
a Tcl command running its own event loop (such as`vwait` or`wait_variable()` ) was active and input arrived on stdin, the event
loop kept spinning at 100% CPU. The stdin file handler is now removed as
soon as input is available.  Based on a patch by Michiel de Hoon.
- gh-152212: Fix the pure-Python `zoneinfo` parser accepting a POSIX TZ
string with a`std` abbreviation but no offset. This is invalid per
POSIX and now raises`ValueError` , matching the C accelerator. Patch
by tonghuaroot.
- gh-152156: Fix a possible crash in `concurrent.interpreters.create()` under limited memory conditions.
- gh-152157: The C implementations of `fromisoformat()` and`fromisoformat()` now reject a decimal separator that
is not followed by any fractional digit before a timezone designator.
- gh-151763: Fix crash in `_interpqueues.create()` whe`MemoryError` happens on queue creation.
- gh-151669: On Windows, when populating tar archives from filesystem content, to conform to the tar format standard, backslashes in symlink targets are be replaced by slashes.
- gh-105895: Add `match` and`case` to the list of
supported topics by`help()` .
- gh-151378: Fix a bug in the binary collector in Tachyon that caused unbounded memory growth while profiling a thread that stayed asleep. Patch by Maurycy PawÅowski-WieroÅski.
- gh-152079: Fix `datetime.datetime.fromisoformat()` in the C
implementation dropping the sub-second part of a UTC offset whose
whole-second part is zero, matching the pure-Python implementation.
- gh-152052: The `json` C accelerator now correctly reports an
unterminated string for a`\uXXXX` escape at the end of the input.
- gh-152060: Fix `datetime.datetime.fromisoformat()` raising`AssertionError` instead of`ValueError` for some malformed
strings in the pure-Python implementation, matching the C implementation.
- gh-127802: The deprecated `tkinter.Variable` methods`trace_variable()` ,`trace()` ,`trace_vdelete()` and`trace_vinfo()` are now scheduled for removal in Python 3.17.
- gh-126219: Fixed a crash in `tkinter.Tk` when*className* contains
a non-BMP character and tkinter is built against Tcl/Tk 8.x.  Such a name
is now rejected with a`ValueError` .
- gh-86165: Fix `imaplib.Time2Internaldate()` to use the local timezone
offset for`time.struct_time` values with`tm_gmtoff` set to`None` ,
as returned by`datetime.datetime.timetuple()` . Contributed by Xiao
Yuan.
- gh-151814: Fix unbounded memory growth in `io.TextIOWrapper` when
repeatedly writing an empty string.
- gh-151596: Add missing `size` positional argument to the pure-Python
implementation of`io.TextIOBase.readline()` .
- gh-151640: Fix a data race in `io.BytesIO` in free-threaded builds
when whole-buffer reads or peeks, or`getvalue()` , share
the internal buffer with concurrent writes.
- gh-151613: Fix another way the Tachyon profiler frame cache could produce impossible mixed stack traces when `_PyInterpreterFrame` addresses are
reused, by validating cached frame anchors with a sequence counter.
- gh-148660: Fix a crash in `collections.OrderedDict.copy()` when a
keyâs`__eq__` or a subclass method mutates the dict during the copy.
Now raises`RuntimeError` instead, as iteration does.
- gh-151497: Opening a `tarfile` archive no longer attempts to
pre-allocate a huge buffer when a crafted or truncated member claims an
oversized extended header (a GNU long name/link or a pax header).  The
extended header is now read in bounded chunks, so its size field can no
longer trigger memory exhaustion.
- gh-151416: Fix a crash in `os.spawnv()` and`os.spawnve()` when an*argv* itemâs`__fspath__()` method mutates the*argv* list during argument conversion.`os.spawnv()` argument conversion
errors other than`TypeError` , such as the`ValueError` for an
embedded null, are no longer replaced with a generic`TypeError` .
- gh-150994: Make the type annotations in the private `_colorize` module
resolvable.
- gh-150583: Correctly set the default compression level in `compression.zstd` when passing a digested dictionary during
compression.
- gh-150641: Fix bug where `typing.evaluate_forward_ref()` with the`STRING` format could leak internal names used by the annotation
machinery.
- gh-149816: Fix a potential use after free condition in `pickle.dumps()` in free-threaded mode when serializing lists.
- gh-47005: Fix `urllib.request.AbstractHTTPHandler.do_open()` to give
regular headers set via`add_header()` priority over unredirected headers, consistent with`get_header()` and`header_items()` .
- gh-140729: Fix a pickling error in the `cProfile` module when profiling
a script that uses`multiprocessing.Process` with the`spawn` and`forkserver` start methods.
- gh-115634: Fix a deadlock in `concurrent.futures.ProcessPoolExecutor` when using`max_tasks_per_child` , present since the feature was introduced in
Python 3.11. The executor stopped scheduling queued tasks after a worker
process exited upon reaching its task limit. Based on a fix proposed by
Tabrez Mohammed.
- gh-79638: Disallow all access in `urllib.robotparser` if the`robots.txt` file is unreachable due to server or network errors.
- gh-105708: Accept an uppercase V prefix in IPvFuture addresses in `urllib.parse.urlsplit()` .

### TestsÂ¶

- gh-151626: Fix several tests in `test.test_inspect` ,`test.test_import` ,`test.test_importlib` ,`test.test_py_compile` and`test.test_compileall` that failed when the test suite was run with`PYTHONPYCACHEPREFIX` set.  These tests now neutralize the pycache
prefix where they assume the default`__pycache__` bytecode layout.
- gh-151096: Fix `test_embed` failing when CPython is configured with a
split exec prefix (`--exec-prefix` differing from`--prefix` ).

### BuildÂ¶

- gh-126877: Fix the **configure** check for Tcl/Tk which could
wrongly succeed with optimizing compilers when the libraries are missing.
- gh-153438: Update Windows build and installer tooling and documentation to use the current download URL for `nuget.exe` .
- gh-152870: Fix a compilation error in the `decimal` C extension
(`_decimal` ) when it is built with`EXTRA_FUNCTIONALITY` .`Context.apply()` called the internal`_apply` helper using its
pre-Argument-Clinic signature; the call is now made through the generated`_impl` function.
- gh-152769: Enable the perf profiler trampoline on Alpine Linux with the musl C library on `x86_64` and`aarch64` . The
trampoline is architecture-specific and does not depend on the C library,
so the same assembly trampoline used for glibc is reused for musl.
- gh-152502: The `curses` module now detects`set_escdelay()` ,`set_tabsize()` and the`ESCDELAY` and`TABSIZE` variables with**configure** capability probes instead of the ncurses-specific`NCURSES_EXT_FUNCS` macro, so they are exposed when building against
other curses implementations such as NetBSD curses that provide them.

### WindowsÂ¶

### macOSÂ¶

### IDLEÂ¶

- gh-82183: When the shell is busy running code, using âRunâ¦ Customizedâ with âRestart shellâ unchecked now reports that the shell is executing instead of restarting it anyway.
- gh-83653: Blanking an integer entry in IDLEâs Settings dialog, such as âAuto squeeze min linesâ, no longer saves an empty string as an invalid configuration value.
- gh-65339: Saving the IDLE Shell or an Output window now defaults to a `.txt` extension and lists text files before Python files, since their
content is not Python source.
- gh-80504: The âIn files:â field of IDLEâs Find in Files dialog now always contains a full directory path, even for an unsaved editor or the Shell. This shows in the grep output which directory was searched.
- gh-134300: Do not add the `idlelib` directory to the path of the IDLE
user process. User code run in IDLE can no longer import`idlelib` submodules as top-level modules, such as`import help` .
- gh-89360: Fix a rare crash in the IDLE editor when the completion window is closed: deleting a key binding for a sequence that is not bound to the virtual event is now ignored instead of raising a `ValueError` .
- gh-71956: Fix Replace All in the IDLE editorâs Replace dialog when the search direction is âUpâ and âWrap aroundâ is off: it now replaces all matches above the current position instead of only the first one.
- gh-152728: Move functions run.fix_scaling, editor.fixwordbreaks (as fix_word_breaks) and pyshell.fix_x11_paste to idlelib.util.
- gh-66331: Set the `WM_CLASS` window property of IDLEâs windows to`Idle` on X11, so that window managers group and label them correctly
instead of using the default`Toplevel` .
- gh-85320: IDLE now reads and writes its configuration files and the breakpoints file using UTF-8 instead of the locale encoding. This keeps non-ASCII data (such as non-ASCII paths) from being corrupted and makes the files portable between environments.

### C APIÂ¶

- gh-152132: Fix `Py_RunMain()` to return an exit code, rather than
calling`Py_Exit()` , when running a script, a command, or the REPL.
Patch by Victor Stinner.
- gh-145633: `PyFloat_Pack8()` and`PyFloat_Unpack*` functions are
no longer guaranteed to always succeed on CPython.
- gh-153300: `PyConfig_Set()` now also set global configuration
variables. For example,`PyConfig_Set("inspect", value)` now also sets`Py_InspectFlag` . Patch by Victor Stinner.

## Python 3.15.0 beta 3Â¶

*Release date: 2026-06-23*

### SecurityÂ¶

- gh-151544: `Modules/Setup.local` is no longer used as a landmark to
discover whether Python is running in a source tree, as it could
potentially affect actual installs. The`pybuilddir.txt` file is now
the sole indicator of running in a source tree.
- gh-151159: Update macOS installer to use OpenSSL 3.5.7.
- gh-151159: Update Android and iOS installers to use OpenSSL 3.5.7.
- gh-150599: Fix a possible stack buffer overflow in `bz2` when a`bz2.BZ2Decompressor` is reused after a decompression error. The
decompressor now becomes unusable after libbz2 reports an error.
- gh-149835: `shutil.move()` now resolves symlinks via`os.path.realpath()` when checking whether the destination is inside
the source directory, preventing a symlink-based bypass of that guard.

### Core and BuiltinsÂ¶

- gh-151905: Fix OOM error handling in `PyFrame_GetBack()` to
propagate exceptions instead of masking them as None.
- gh-151722: Defer GC tracking of `frozendict` to end of
construction. Patch by Donghee Na.
- gh-151546: Fix the stack limit check if Python is linked to musl (ex: Alpine Linux). Use the stack size set by the linker to compute the stack limits. Patch by Victor Stinner.
- gh-151510: Fix a crash in `__lazy_import__()` when called without an
explicit`globals` argument and without a current Python frame.
- gh-151461: Fix direct execution of files with invalid source encodings to report the underlying codec lookup or decoding error instead of the generic `SyntaxError: encoding problem` message. Patch by Bartosz
SÅawecki.
- gh-151218: `PyConfig_Set()` and`sys.set_int_max_str_digits()` now replace`sys.flags` (create a new object), instead of modifying`sys.flags` in-place. Patch by Victor Stinner.
- gh-151297: Fix an invalid pointer dereference that could occur when calling `PyObject_Realloc()` with a NULL pointer in
free-threaded builds or with`PYTHONMALLOC` set to`mimalloc` .
- gh-151253: If `import encodings` (first import) fails at Python startup,
dump the Python path configuration to help users debugging their
configuration. Patch by Victor Stinner.
- gh-151238: Fix a crash when compiling a concatenated f-string or t-string if an error occurs when processing one of itâs parts.
- gh-151112: Fix a crash in the compiler that could occur when running out of memory.
- gh-151126: Fix a crash, when thereâs no memory left on a device, which happened in: code compilation, `_interpchannels` module,`_winapi.CreateProcess()` function.Now these places raise proper `MemoryError` errors.
- gh-150902: Apply an existing optimization of PyCriticalSection (single mutex) to PyCriticalSection2: avoid acquiring the same locks that the current CS has already acquired.
- gh-151065: Fix memory leak when using the mimalloc memory allocator.
- gh-150988: Fix a reference leak in `OSError` when attributes are set
before`super().__init__()` .
- gh-150723: Fix perf jitdump timestamps on macOS. Events were stamped using `CLOCK_MONOTONIC` , but macOS profilers timestamp their samples with`mach_absolute_time()` . The mismatch prevented the JIT code mappings
from lining up with the samples, so no Python frame could be resolved.
- gh-150723: Fix malformed perf jitdump thread ids on macOS. The `thread_id` field of the`JR_CODE_LOAD` record was written as a 64-bit
value instead of the 32-bit value required by the jitdump format, which
shifted every following field and prevented profilers from resolving
Python frames.
- gh-150700: Fix a `SystemError` when compiling a class-scope
comprehension containing a`lambda` that references`__class__` ,`__classdict__` , or`__conditional_annotations__` . Patch by Bartosz
SÅawecki.
