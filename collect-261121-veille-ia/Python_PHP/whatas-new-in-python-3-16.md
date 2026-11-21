---
id: collect-261121-veille-ia/python-php/whatas-new-in-python-3-16
title: "Whatâs new in Python 3.16"
domain: python-php
role: reference
task: article
actors: []
dates: []
keywords: ["contributed", "python", "parameter", "asyncio", "whatas", "stop", "support", "faster"]
source: https://docs.python.org/3.16/whatsnew/3.16.html
source_anchor: ""
source_lines: [1, 1390]
section: "Python_PHP"
---
# Whatâs new in Python 3.16Â¶

- Editor:
- Savannah Ostrowski

This article explains the new features in Python 3.16, compared to 3.15.

For full details, see the changelog.

Note

Prerelease users should be aware that this document is currently in draft form. It will be updated substantially as Python 3.16 moves towards release, so itâs worth checking back even after reading earlier versions.

## Summary â Release highlightsÂ¶

- Set operations and Unicode properties in regular expressions.
- Encrypted Client Hello support in ssl.
- Faster interpreter startup.
- Faster UUID generation, with UUIDv4 and UUIDv7 about 15 and 30 times faster in microbenchmarks.
- Build support for tvOS, watchOS, and visionOS.
- Improved error messages.

## New featuresÂ¶

### Improved error messagesÂ¶

- `SyntaxError` messages now suggest Python alternatives to some
keywords from other programming languages. For example,`switch x:` suggests`match` ,`delete x` suggests`del` , and`function f():` suggests`def` .
(Contributed by Zang Langyan in gh-151128.)
- Improve `SyntaxError` message for`&&` and`||` operators,
suggesting`and` /`&` and`or` /`|` respectively.
(Contributed by Aniket, Sergey B Kirpichev, and Nikita Sobolev in
gh-146495.)
- Suggest the closest valid feature name in the `SyntaxError` message
when an unknown name is imported from`__future__` .
(Contributed by Nikita Sobolev and Pablo Galindo Salgado in gh-145868.)
- `TypeError` messages for some instance methods called with too many
positional arguments now suggest checking whether the method definition is
missing the`self` parameter. (Contributed by Suren Nihalani in
gh-152315.)
- Add context notes to `TypeError` raised while hashing a`tuple` , identifying the index of the item whose hash failed.
(Contributed by Ivan Zorin in gh-161433.)

## Other language changesÂ¶

- The `iter()` function now accepts the*stop_exception* parameter.
The created iterator stops when the callable raises the specified exception.
The second parameter is now named*stop_value* and can be passed by keyword.`aiter()` now accepts the same*stop_value* and*stop_exception* parameters, calling an asynchronous callable and awaiting the result.
(Contributed by Serhiy Storchaka in gh-64862.)
- `memoryview.cast()` now allows casting a multidimensional
F-contiguous view to a one-dimensional view.
(Contributed by Jaemin Park in gh-91484.)
- `memoryview.cast()` now accepts an*order* parameter.  With`order='F'` it returns a zero-copy Fortran-contiguous (column-major)
view of a flat buffer, which is useful for interfacing with column-major
libraries.
(Contributed by Serhiy Storchaka in gh-78959.)
- Frame objects now support `weak references` .  This allows associating extra data with active frames,
for example in debuggers, without keeping the frames (and everything
they reference) alive indefinitely.
(Contributed by Åukasz Langa in gh-102960.)
- `anext()` and`aiter()` are now implemented in Python instead
of C.  The awaitable returned by`anext(it, default)` and the`__anext__()` of the iterator returned by`aiter(callable, ...)` are
now plain coroutines, so introspection tools such as`asyncio.print_call_graph()` can see through them into the awaited code.
(Contributed by Kumar Aditya in gh-157361.)

## Default interactive shellÂ¶

## New modulesÂ¶

- None yet.

## Improved modulesÂ¶

### argparseÂ¶

- `ArgumentGroup` and`MutuallyExclusiveGroup` , the types returned by`add_argument_group()` and`add_mutually_exclusive_group()` , are now
public. The`_` -prefixed names remain as aliases.
(Contributed by BernÃ¡t GÃ¡bor in gh-144812.)

### asyncioÂ¶

- Add the *mode* parameter to`asyncio.loop.create_unix_server()` and`asyncio.start_unix_server()` to set the permissions of the Unix
socket file created for*path* .
(Contributed by Sam Bull in gh-94984.)
- Add *eager_tasks* parameter to`asyncio.run()` function and`asyncio.Runner` class constructor.
(Contributed by Andrew Svetlov in gh-128289.)

### bdbÂ¶

- `bdb.BdbQuit` is now a subclass of`BaseException` , instead of`Exception` , to allow it to be used for cleanly exiting the debugger without
being accidentally caught by user code.
(Contributed by Tian Gao in gh-149337.)

### bz2Â¶

- Added constants `bzlib_version` and`bzlib_version_info` which provide information about
the version of the bzip2 compression library in use.
(Contributed by Serhiy Storchaka in gh-117404.)

### codecsÂ¶

- On platforms that provide the C libraryâs *iconv(3)* function,
every encoding known to`iconv` for which Python has no built-in codec
is now available (for example`cp1133` ).
Prefixing an encoding name with`iconv:` forces the`iconv` -based codec
even when a built-in codec of the same name exists.
(Contributed by Serhiy Storchaka in gh-152997.)

### compression.zstdÂ¶

- Added constants `ZSTD_VERSION` and`ZSTD_VERSION_INFO` which provide information
about the version of the zstd library that was used for building the module.`zstd_version_info` is now a named tuple.
(Contributed by Serhiy Storchaka in gh-117404.)

### concurrent.futuresÂ¶

- The iterator returned by `concurrent.futures.Executor.map()` is no longer
automatically closed if a function call raises an exception.
Use the`close()` method to explicitly close the iterator.
(Contributed by xzmeng and Serhiy Storchaka in gh-108518.)

### concurrent.interpretersÂ¶

- `Interpreter` objects now support the
context manager protocol. Exiting the`with` block calls`close()` .
(Contributed by Peter Bierma in gh-158370.)

### copyÂ¶

- `copy.replace()` now supports`argparse.Namespace` ,`optparse.Values` ,`decimal.Context` ,`tarfile.TarInfo` , and CSV dialects (instances of`csv.Dialect` subclasses and objects returned by`csv.get_dialect()` ).
(Contributed by Serhiy Storchaka and Stan Ulbrych in gh-155043,
gh-155044, gh-155041, gh-155040, and gh-155033.)

### csvÂ¶

- `csv.Sniffer.sniff()` now deduces the dialect by trial parsing
instead of heuristics based on matching isolated fragments
and on character frequencies,
so the result is consistent with how`csv.reader()` will parse the sample.
It can now detect the*escapechar* parameter,
accepts non-ASCII delimiters in the*delimiters* argument,
no longer misdetects the delimiter
when the sample contains delimiter characters inside quoted fields,
and no longer takes quadratic time on quoted samples.
The results may differ from those of earlier Python versions.
(Contributed by Serhiy Storchaka in gh-83273.)
- `csv.Sniffer.sniff()` now detects the*lineterminator* parameter
by a majority vote among the line endings of the sample,
instead of always returning`'\r\n'` .
(Contributed by Serhiy Storchaka in gh-75008.)

### ctypesÂ¶

- Add `ctypes.util.struct()` for generating`Structure` types
from an annotation-based syntax, similar to how the`dataclasses` module
is used.
(Contributed by Peter Bierma in gh-104533.)
- Add `ctypes.util.wrap_dll_function()` for generating function pointers
through a function signature.
(Contributed by Peter Bierma in gh-153903.)
- Added constants `LIBFFI_VERSION` ,`libffi_version` ,`LIBFFI_VERSION_INFO` and`libffi_version_info` , which provide information
about the version of the libffi library in use.
They are only available with libffi 3.5 or later.
(Contributed by Serhiy Storchaka in gh-157487.)
- Objects with an `_as_parameter_` attribute are now
implicitly converted when passed for a`POINTER()` argument.
(Contributed by Peter Bierma in gh-71990.)
- Callback functions can now return `Structure` and`Union` instances by value.
(Contributed by Russell Keith-Magee in gh-49960.)

### cursesÂ¶

- The `curses` character-cell window methods now accept a full character
cell â a spacing character optionally followed by combining characters â
in addition to a single integer or byte character.  This affects`addch()` ,`bkgd()` ,`bkgdset()` ,`border()` ,`box()` ,`echochar()` ,`hline()` ,`insch()` and`vline()` .
Also add the wide-character read methods`get_wstr()` and`in_wstr()` , the counterparts of`getstr()` and`instr()` that return a`str` rather than`bytes` ,
and the module functions`curses.erasewchar()` ,`curses.killwchar()` and`curses.wunctrl()` , the wide-character counterparts of`curses.erasechar()` ,`curses.killchar()` and`curses.unctrl()` .
On a narrow (non-ncursesw) build the character cell holds a single character
without combining marks, representable as one byte in the windowâs encoding,
and`in_wstr()` returns its decoded text.
(Contributed by Serhiy Storchaka in gh-151757.)
- Add the `curses.complexchar` type, representing a styled
character cell (its text, attributes and color pair), and the window
methods`in_wch()` and`getbkgrnd()` that return one â the counterparts of`inch()` and`getbkgd()` .  The
character-cell methods, such as`addch()` and`border()` , now also accept a`complexchar` .  These work whether or not Python was built
against a wide-character-aware curses library; on a narrow build a cell holds a
single character representable as one byte in the windowâs encoding (so only
8-bit locales are supported).
(Contributed by Serhiy Storchaka in gh-152233.)
- Add the `curses.complexstr` type, an immutable run of styled cells
(the string counterpart of`complexchar` ), and the window
method`in_wchstr()` that returns one.  The string-cell
methods`addstr()` ,`addnstr()` ,`insstr()` and`insnstr()` now also
accept a`complexstr` .  Like`complexchar` , it
works whether or not Python was built against a wide-character-aware curses
library.
(Contributed by Serhiy Storchaka in gh-152233.)
- The wide-character `curses` functions and methods`get_wch()` ,`get_wstr()` ,`curses.unget_wch()` ,`curses.erasewchar()` ,`curses.killwchar()` and`curses.wunctrl()` now also work when Python
is not built against a wide-character-aware curses library, on an 8-bit
locale, where each character is a single byte in the relevant encoding.`curses.ungetch()` now also accepts a one-character string, like`curses.unget_wch()` ; on a wide-character build it can be any character
(previously a multibyte character raised`OverflowError` ).
(Contributed by Serhiy Storchaka in gh-152470.)
- Add support for multiple terminals to the `curses` module:
the new functions`curses.newterm()` ,`curses.set_term()` and`curses.new_prescr()` ,
the corresponding screen object,
and the`window.use()` method.
(Contributed by Serhiy Storchaka in gh-90092.)
- Add the `curses` window methods`attr_get()` ,`attr_set()` ,`attr_on()` ,`attr_off()` and`color_set()` , which
pass the color pair as a separate argument instead of packing it into the
attribute value, and the corresponding`WA_*` attribute constants.
(Contributed by Serhiy Storchaka in gh-152219.)
- Add the `curses.term_attrs()` function, which returns the supported
video attributes as WA_* values, the
counterpart of`curses.termattrs()` .
(Contributed by Serhiy Storchaka in gh-152332.)
- Add the `WACS_*` constants to the`curses` module, the counterparts of
the ACS_* line-drawing codes as`curses.complexchar` cells.
(Contributed by Serhiy Storchaka in gh-155863.)
- Add the `curses` functions`curses.alloc_pair()` ,`curses.find_pair()` ,`curses.free_pair()` and`curses.reset_color_pairs()` for dynamic color-pair management,
available when built against a wide-character ncurses with extended-color
support.
(Contributed by Serhiy Storchaka in gh-151774.)
- Add the `curses` functions`scr_dump()` ,`scr_restore()` ,`scr_init()` and`scr_set()` , which dump the whole screen to a file and restore it.
(Contributed by Serhiy Storchaka in gh-152260.)
- Add the `curses` window method`dupwin()` , which
returns a new window that is an independent duplicate of an existing one.
(Contributed by Serhiy Storchaka in gh-152258.)
- Add the soft-label-key functions to the `curses` module, which manage a
row of labels along the bottom line of the screen:`slk_init()` ,`slk_set()` ,`slk_label()` ,`slk_refresh()` ,`slk_noutrefresh()` ,`slk_clear()` ,`slk_restore()` ,`slk_touch()` , the attribute functions`slk_attron()` ,`slk_attroff()` ,`slk_attrset()` ,`slk_attr()` ,`slk_attr_on()` ,`slk_attr_off()` ,`slk_attr_set()` , and`slk_color()` .
(Contributed by Serhiy Storchaka in gh-152263.)
- Add the `curses` key-management functions`define_key()` ,`key_defined()` and`keyok()` , available when built
against an ncurses with`NCURSES_EXT_FUNCS` .
(Contributed by Serhiy Storchaka in gh-152334.)
- Add the `curses.has_mouse()` function and the`curses.window.mouse_trafo()` method, completing the`curses` mouse interface.
(Contributed by Serhiy Storchaka in gh-152325.)
- Add `curses` functions and window methods that report state which could
previously only be set, such as`curses.window.is_keypad()` ,`curses.window.getparent()` and`curses.is_cbreak()` ,
available when built against an ncurses with`NCURSES_EXT_FUNCS` .
(Contributed by Serhiy Storchaka in gh-151776.)
- Add `curses.nofilter()` , which undoes the effect of`curses.filter()` .
(Contributed by Serhiy Storchaka in gh-151744.)
- `curses.textpad.Textbox` now supports entering and reading back the
full Unicode range, including combining characters, when curses is built with
wide-character support.
(Contributed by Serhiy Storchaka in gh-133031.)

### datetimeÂ¶

- Add the `%Nf` format code (where*N* is from 1 to 6) to`datetime.datetime.strftime()` ,`datetime.time.strftime()` ,`datetime.date.strftime()` ,`datetime.datetime.strptime()` and`datetime.time.strptime()` .
(Contributed by Stan Ulbrych and Karoline Pauls in gh-157290.)
- Add `datetime.timezone.name` , the*name* passed to the constructor,
or`None` if no name was passed.
(Contributed by Stan Ulbrych in gh-131210.)

### dbm.gnuÂ¶

- Added constants `GDBM_VERSION_INFO` ,`gdbm_version_info` and`gdbm_version` ,
which provide information about the version of the GDBM library in use.
(Contributed by Serhiy Storchaka in gh-157478.)

### dbm.ndbmÂ¶

- Added constants which provide information about the version of the underlying library in use: `GDBM_VERSION_INFO` ,`gdbm_version_info` and`gdbm_version` if it is GDBM, or`BDB_VERSION` ,`bdb_version` ,`BDB_VERSION_INFO` and`bdb_version_info` if it is Berkeley DB.`dbm.ndbm.library` is now`'ndbm'` instead of`'GNU gdbm'` for a classic NDBM library.
(Contributed by Serhiy Storchaka in gh-157480.)

### decimalÂ¶

- Added constants `LIBMPDEC_VERSION` ,`libmpdec_version` ,`LIBMPDEC_VERSION_INFO` ,
and`libmpdec_version_info` in the C implementation,
which provide information about the version of the libmpdec library in use.
(Contributed by Serhiy Storchaka in gh-157475.)

### difflibÂ¶

- Expose the optional *autojunk* parameter of`difflib.SequenceMatcher` in public functions and class methods in`difflib` , allowing callers
to control the automatic junk heuristic.
(Contributed by Tomasz Kazimierczak in gh-118150.)

### encodingsÂ¶

- Add the `utf-7-imap` codec, implementing the modified UTF-7 encoding
used for international IMAP4 mailbox names (**RFC 3501** ).
(Contributed by Serhiy Storchaka in gh-66788.)
- `encodings.idna.nameprep()` now takes a*limit* argument that allows
rejecting extremely large input early.
(Contributed by Petr Viktorin in gh-157675.)

### gzipÂ¶

- `gzip.open()` now accepts an optional argument`mtime` which is passed on to the constructor of the`GzipFile` class.
(Contributed by Marin Misur in gh-91372.)

### httpÂ¶

- Add the `QUERY` HTTP method (**RFC 10008** ) to`HTTPMethod` .
(Contributed by Michiel W. Beijen in gh-153309.)

### http.clientÂ¶

- `HTTPConnection` now honors an`Expect: 100-Continue` request header: the request body is sent only after the server responds
with`100 Continue` , or once the new*continue_timeout* of`request()` and`putrequest()` expires.  If the server
responds with a final status instead, the body is not sent.`getresponse()` gains a*support_continue* parameter to return`100 Continue` responses to the caller.
(Contributed by Ashwin Menon, Tim Bartlett and Senthil Kumaran in
gh-42550.)
- Add the `ProxyTunnelError` exception, a subclass of`OSError` raised when a proxy rejects the CONNECT request sent for`set_tunnel()` .  It exposes the status
code, reason and headers of the proxyâs response.
(Contributed by Senthil Kumaran in gh-51540.)

### imaplibÂ¶

- Add the `id()` method,
a wrapper for the`ID` command (**RFC 2971** ).
(Contributed by Serhiy Storchaka in gh-98092.)
- Add the `move()` method,
a wrapper for the`MOVE` command (**RFC 6851** ).
(Contributed by Serhiy Storchaka in gh-77508.)
- Add the `login_plain()` method, which authenticates
using the`PLAIN` SASL mechanism (**RFC 4616** ) and supports non-ASCII
user names and passwords.
(Contributed by PrzemysÅaw Buczkowski and Serhiy Storchaka in gh-89869.)
- Non-ASCII mailbox names are now automatically encoded as modified UTF-7 ( **RFC 3501** , section 5.1.3) in the default mode, so international mailbox
names can be passed as ordinary`str` without enabling`UTF8=ACCEPT` (under which they are sent as UTF-8).
(Contributed by Serhiy Storchaka in gh-49555.)
- The `copy()` ,`move()` ,`fetch()` ,`store()` ,`search()` ,`sort()` ,`thread()` and`expunge()` methods
now accept a keyword-only*uid* argument that selects the corresponding`UID` command, as a more convenient alternative to`uid()` .
(Contributed by Serhiy Storchaka in gh-153502.)
- `search()` ,`sort()` and`thread()` (and the corresponding`uid` commands)
now encode`str` search criteria to the declared*charset* ,
so international search text can be passed as an ordinary`str` .
When*charset* is`None` (as it must be under`UTF8=ACCEPT` ),
the criteria are sent using the connection encoding instead.
(Contributed by Serhiy Storchaka in gh-153494.)
- Command methods now accept structured arguments, so the module takes care of quoting instead of the caller. A *message_set* and lists of flags or other atoms
can be passed as sequences instead of preformatted strings,
and the`search()` ,`fetch()` ,`sort()` ,`thread()` and`uid()` methods accept a*params* keyword argument
that substitutes and quotes`?` placeholders,
in the manner of`sqlite3` parameter substitution.
(Contributed by Serhiy Storchaka in gh-153521.)

### ioÂ¶

- Add `io.BytesIO.peek()` method to read without advancing position.
(Contributed by Marcel Martin in gh-90533.)

### ipaddressÂ¶

- Add `IPv4Network.next_network()` and`IPv6Network.next_network()` methods to find the next nearest network with a specific prefix size.
(Contributed by Faisal Mahmood in gh-87027.)
- Add `IPv4Network.prev_network()` and`IPv6Network.prev_network()` methods to find the previous nearest network with a specific prefix size.
(Contributed by Faisal Mahmood in gh-87027.)

### loggingÂ¶

- `TimedRotatingFileHandler` now uses the creation
time instead of the last modification time of an existing log file as
the basis for the first rotation after handler creation, if supported by
the OS and file system.
This allows it to be used in short-running programs that start and end
before the rotation interval expires.
(Contributed by IvÃ¡n MÃ¡rton and Serhiy Storchaka in gh-84649.)

### lzmaÂ¶

- Add support of new BCJ filters ARM64 and RISC-V via `lzma.FILTER_ARM64` and`lzma.FILTER_RISCV` .  Note that the
new filters will work only if the runtime library supports them. ARM64 filter
requires`lzma` 5.4.0 or newer while RISC-V requires 5.6.0 or newer.
(Contributed by Chien Wong in gh-115988.)
- Added constants `LZMA_VERSION` ,`lzma_version` ,`LZMA_VERSION_INFO` , and`lzma_version_info` ,
which provide information about the version of the lzma library in use.
(Contributed by Serhiy Storchaka in gh-117404.)

### mathÂ¶

- Added trigonometric functions that work in units of half turns, rather than radians. The new functions `math.acospi()` ,`math.asinpi()` ,`math.atanpi()` , and`math.atan2pi()` return half-turn angles. The
new functions`math.cospi()` ,`math.sinpi()` , and`math.tanpi()` take half-turn angle arguments. These functions are recommended by IEEE
754-2019 and standardized in C23.
(Contributed by Jeff Epler in gh-150534.)

### multiprocessingÂ¶

- Add the optional *buffersize* parameter to`multiprocessing.pool.Pool.imap()` and`multiprocessing.pool.Pool.imap_unordered()` to limit the number of
submitted tasks whose results have not yet been yielded. If the buffer is
full, iteration over the*iterable* pauses until a result is yielded from
the buffer. To fully utilize poolâs capacity when using this feature, set*buffersize* at least to the number of processes in pool (to consume*iterable* as you go), or even higher (to prefetch the next`N=buffersize-processes` arguments).
(Contributed by Oleksandr Baltian in gh-136871.)

### osÂ¶

- Add `os.pidfd_getfd()` for duplicating a file descriptor from another
process via a pidfd.  Available on Linux 5.6+.
(Contributed by Maurycy PawÅowski-WieroÅski in gh-149464.)
- The following functions are now available on Android: `os.chroot()` ,`os.initgroups()` ,`os.setegid()` ,`os.seteuid()` ,`os.setgid()` ,`os.setregid()` ,`os.setresgid()` ,`os.setresuid()` ,`os.setreuid()` ,
and`os.setuid()` . Calling them without sufficient privileges
now raises`PermissionError` instead of the functions being unavailable.
(Contributed by Md Arif in gh-152936.)
- `os.readlink()` now accepts a file descriptor referring to a symlink on
Linux, Android, macOS and iOS.
(Contributed by OOTS in gh-157899.)

### pydocÂ¶

- Modernize the HTML output of `pydoc` .  The pages generated by the`pydoc` HTTP server now use semantic HTML5 markup and a new style
sheet based on the python-docs-theme used by docs.python.org, with dark mode support.  Class members
inherited from other classes are collapsed by default.
(Contributed by Serhiy Storchaka in gh-153906.)
- Add help for more special names to the built-in `help()` function.
(Contributed by Adam Hartz in gh-137966.)

### pickleÂ¶

- Weak references ( `weakref.ref` ),`weakref.WeakMethod` ,`weakref.WeakSet` ,`weakref.WeakValueDictionary` and`weakref.WeakKeyDictionary` can now be pickled if the weakly
referenced objects are strongly referenced elsewhere in the same pickle;
otherwise pickling raises`pickle.PicklingError` .`WeakMethod` and`WeakSet` can now also be copied.
(Contributed by Serhiy Storchaka in gh-74876.)

### quopriÂ¶

- `quopri.decode()` ,`quopri.decodestring()` and`binascii.a2b_qp()` gained a*strip_ws* parameter.  When true, trailing
whitespace is stripped from each line while decoding, as required by**RFC 2045** for a quoted-printable body.
(Contributed by Serhiy Storchaka in gh-62222.)

### reÂ¶

- `re` now supports set operations and nested sets in character classes,
as described in Unicode Technical Standard #18: set difference (`[A--B]` ),
intersection (`[A&&B]` ) and union (`[A||B]` ), where an operand may be a
nested set written in square brackets.  For example,`[a-z--[aeiou]]` matches an ASCII lowercase consonant.
(Contributed by Serhiy Storchaka in gh-152100.)
- Regular expressions now support Unicode property escapes `\p{...}` and`\P{...}` , which match a character by a Unicode property â for example`\p{Lu}` (an uppercase letter),`\p{Cased}` or`\p{ASCII}` .  See
the regular expression syntax for the supported
properties.
(Contributed by Serhiy Storchaka in gh-95555.)

### readlineÂ¶

- Added constants `READLINE_VERSION_INFO` ,`readline_version_info` and`readline_version` , which provide information about
the version of the Readline library in use.
(Contributed by Serhiy Storchaka in gh-157485.)

### shlexÂ¶

- Add keyword-only parameter *force* to`shlex.quote()` to force quoting
a string, even if it is already safe for a shell without being quoted.
(Contributed by Jay Berry in gh-148846.)

### shutilÂ¶

- Add keyword-only *copy_dir_stat* parameter to`shutil.copytree()` :
set it to false to skip copying directory metadata with`shutil.copystat()` , e.g. when copying a read-only tree.
(Contributed by Åukasz Langa in gh-89721.)
- `shutil.unpack_archive()` can now unpack wheel files using the`whl` format.
(Contributed by Emma Harper Smith and Stan Ulbrych in gh-84027.)

### socketÂ¶

- The `socket.sethostname()` function is now available on Android.
Calling it without sufficient privileges now raises`PermissionError` instead of the function being unavailable.
(Contributed by Md Arif in gh-152936.)

### sqlite3Â¶

- `sqlite3.Blob` now supports negative-step slices for reading and
writing (for example,`blob[9:0:-2]` ).  Previously, such slices would raise`SystemError` or`ValueError` .
(Contributed by Jiseok CHOI in gh-150449.)
- Added constants `SQLITE_VERSION` and`SQLITE_VERSION_INFO` which provide information
about the version of the SQLite library that was used for building the module.`sqlite_version_info` is now a named tuple.
(Contributed by Serhiy Storchaka in gh-157470.)

### sslÂ¶

- Added support for Encrypted Client Hello (ECH) with OpenSSL 4.0 or later. Clients can supply an ECHConfigList discovered through DNS, for example using a third-party library such as `dnspython` to query HTTPS or SVCB records, as the*ech_config* argument
of`wrap_socket()` and`wrap_bio()` or with`set_ech_config()` .
Servers can use`load_ech_keys()` to load ECH private
keys and their configurations from PEM files.
(Contributed by Peter Bierma and Iain R. Learmonth in gh-89730.)
- `ssl.OPENSSL_VERSION_INFO` is now a named tuple.
Added`OPENSSL_API_VERSION_INFO` which provides information
about the version of the OpenSSL library that was used for building
the module.
(Contributed by Serhiy Storchaka in gh-157513.)
- The `ssl` module now rejects TLS certificate chains that contain one
of a set of compromised eMudhra intermediate CA certificates.  The affected
certificates are identified by their public key, so cross-signed copies that
reuse the same key are blocked as well.  It is a defense-in-depth backstop
that applies even when an application supplies its own root CA bundle, and
regardless of whether the platform trust store has removed trust.
(Contributed by Christian Heimes in gh-160040 for**CVE 2026-115557** .)

### symtableÂ¶

- `symtable.symtable()` now accepts an AST object,
like the built-in`compile()` .
(Contributed by Serhiy Storchaka in gh-153844.)
- Inlined list, set and dict comprehensions ( **PEP 709** ) are now represented
as their own symbol table entries, of type`INLINED_COMPREHENSION` . Each such entry is
a lexical child of the enclosing scope and records the comprehensionâs own
locals, cells, and free names. It does not correspond to a separate
compilation unit. For names loaded only inside an inlined list, set, or
dict comprehension,`symtable.Symbol.is_referenced()` can now return`True` on the enclosing function.
(Contributed by Irit Katriel in gh-124697.)

### tkinterÂ¶

- Added many `tkinter.ttk.Treeview` methods wrapping the enhanced`ttk::treeview` widget commands from Tk 9.1, such as`sort()` ,`search()` ,`expand()` ,`collapse()` ,`hide()` ,`unhide()` , and
methods for cell focus, selection and tagging.  The`expand()` and`collapse()` methods (without recursion) also work on Tk older than 9.1.
(Contributed by Serhiy Storchaka in gh-151910.)
- Added constants `TCL_VERSION_INFO` and`TK_VERSION_INFO` , named tuples with the versions of
the Tcl and Tk libraries that were used for building the module.
(Contributed by Serhiy Storchaka in gh-157514.)
- Added new `tkinter.Text` methods`edit_canundo()` and`edit_canredo()` which return whether an undo or redo
is possible.
(Contributed by Serhiy Storchaka in gh-151674.)
- Added new `tkinter.Text` methods`sync()` and`pendingsync()` which control and report the
synchronization of the displayed view with the underlying text.
(Contributed by Serhiy Storchaka in gh-151675.)
- Added the `ttk.Style.theme_styles` method which returns the list of styles
defined in a theme.
(Contributed by Serhiy Storchaka in gh-151920.)
- Added new `tkinter.Canvas` methods`rchars()` which replaces the text or coordinates of canvas items, and`rotate()` which rotates the coordinates of canvas items.
(Contributed by Serhiy Storchaka in gh-151876.)
- Added a `validate()` method to the`tkinter.Entry` and`tkinter.Spinbox` widgets, which forces an evaluation of the
validation command.
(Contributed by Serhiy Storchaka in gh-151878.)
- Added the `tkinter.Menu.postcascade()` method, and the`tk_scaling()` and`tk_inactive()` methods which respectively query or set the display scaling factor and
report the user idle time.
(Contributed by Serhiy Storchaka in gh-151881.)
- Added the `tk_print()` method to`tkinter.Canvas` and`tkinter.Text` which prints the contents of the widget using the
native print dialog.  It requires Tk 8.7/9.0 or newer.
(Contributed by Serhiy Storchaka in gh-153256.)
- Added new window-management methods `winfo_isdark()` (dark mode detection),`wm_iconbadge()` (application icon
badge) and`wm_stackorder()` (toplevel stacking order).
(Contributed by Serhiy Storchaka in gh-151874.)
- Added the `tk_appname()` ,`tk_useinputmethods()` and`tk_caret()` methods, exposing the application send name, the X Input Methods state and
the input method caret location.
(Contributed by Serhiy Storchaka in gh-151886.)
- Added support for more options in `tkinter.PhotoImage` methods: the*format* parameter of`put()` , the*metadata* parameter of`put()` ,`read()` ,`write()` and`data()` , and
the*withalpha* parameter of`get()` .
(Contributed by Serhiy Storchaka in gh-151890.)
- Added the `redither()` method which recalculates the
dithered image when its data was supplied in pieces.
(Contributed by Serhiy Storchaka in gh-151888.)
- `tkinter.OptionMenu` now accepts arbitrary`tkinter.Menubutton` options as keyword arguments, which can also override its default appearance.
(Contributed by Serhiy Storchaka in gh-101284.)
- The `tkinter.simpledialog` dialogs were modernized to match the look
and feel of the native Tk dialogs.`tkinter.simpledialog.SimpleDialog` and the`askinteger()` ,`askfloat()` and`askstring()` dialogs are now built from the themed`tkinter.ttk` widgets instead of the classic`tkinter` widgets;
the`tkinter.simpledialog.Dialog` base class still defaults to the
classic widgets for compatibility.  Both`Dialog` and`SimpleDialog` gained a*use_ttk* parameter that selects between the
classic Tk widgets and the themed ttk widgets.`SimpleDialog` also
gained*bitmap* and*detail* parameters, draws the standard icons with themed images in the
ttk version, and accepts mappings of button options as*buttons* entries.
(Contributed by Serhiy Storchaka in gh-59396.)
- The `tkinter.filedialog.FileDialog` dialog and its`tkinter.filedialog.LoadFileDialog` and`tkinter.filedialog.SaveFileDialog` subclasses are now built from the
themed`tkinter.ttk` widgets by default instead of the classic`tkinter` widgets, and gained a*use_ttk* parameter to select between
them.
(Contributed by Serhiy Storchaka in gh-59396.)
- Added the `tkinter.systray` module which provides the`SysTrayIcon` class as an interface to the system
tray icon and the`notify()` function which sends a
desktop notification.  They require Tk 8.7/9.0 or newer.
(Contributed by Serhiy Storchaka in gh-153259.)
- `tkinter.scrolledtext.ScrolledText` gained a*use_ttk* parameter to use
the themed`tkinter.ttk` frame and scroll bar instead of the classic`tkinter` widgets.
(Contributed by Serhiy Storchaka in gh-59396.)
- `tkinter.font.Font` can now wrap a font description without creating a
new named font, by passing it as*font* with`exists=True` and no*name* .
This avoids a loss of precision in`actual()` ,`measure()` and`metrics()` .
(Contributed by Serhiy Storchaka in gh-143990.)
- Added the `tkinter.fontchooser` module which provides the`FontChooser` class as an interface to the
native font selection dialog.
(Contributed by Serhiy Storchaka in gh-72880.)
- Values of several Tcl object types returned by `tkinter` are now
converted to the corresponding Python type instead of being wrapped in a`_tkinter.Tcl_Obj` :`index` ,`window` ,`nsName` and`parsedVarName` objects to`str` , and`pixel` screen distances
with no unit suffix to`int` or`float` .
(Contributed by Serhiy Storchaka in gh-153513.)

### turtleÂ¶

- Translations of the `turtle` docstrings are now distributed on PyPI
in the turtle-translations package. Additionally, added the`PYTHON_TURTLE_LANG` environment variable to select the language.
(Contributed by Stan Ulbrych in gh-156360.)

### unicodedataÂ¶

- The Unicode database has been updated to Unicode 18.0.0.

### urllib.requestÂ¶

- `ProxyBasicAuthHandler` and`ProxyDigestAuthHandler` now authenticate with a
proxy that answers the CONNECT request for an`https` URL with`407 Proxy Authentication Required` .  When no credentials are available,`HTTPError` with code 407 is raised instead of`URLError` .
(Contributed by Senthil Kumaran in gh-51540.)

### uuidÂ¶

- `uuid.UUID` ,`uuid.uuid4()` , and`uuid.uuid7()` now have
C implementations, with a pure Python fallback.
In microbenchmarks, UUIDv4 and UUIDv7 generation are approximately 15 and
30 times faster, respectively, than the pure Python implementations.
UUID construction and string conversion are also faster; see
uuid for the benchmark results.
(Contributed by Yury Selivanov in gh-139123.)

### xmlÂ¶

- Add support for multiple multi-byte encodings in the `XML parser` : âcp932â, âcp949â, âcp950â, âBig5â, âEUC-JPâ,
âGB2312â, âGBKâ, âjohabâ, and âShift_JISâ.
Add partial support (only BMP characters) for multi-byte encodings
âBig5-HKSCSâ, âEUC_JIS-2004â, âEUC_JISX0213â, âShift_JIS-2004â,
âShift_JISX0213â, âutf-8-sigâ and non-standard aliases like âUTF8â
(without hyphen).
The parser now raises`ValueError` for known unsupported
multi-byte encodings such as âISO-2022-JPâ or âraw-unicode-escapeâ
instead of failing later, when encountering non-ASCII data.
(Contributed by Serhiy Storchaka in gh-62259.)
- `xml.dom.minidom` now conforms closer to the DOM Level 1 specification.
It checks names passed to the factory methods,
rejects inserting or setting a node created by another document
or making a node a descendant of itself,
rejects a second element or document type child of a document
and a notation child of a document fragment,
reports attributes defaulted in the DTD
and whether an attribute was given in the start tag,
and implements`EntityReference` nodes
and`Document.createEntityReference()` .
(Contributed by Jason Orendorff and Serhiy Storchaka in gh-44871.)
- `xml.dom.minidom` now validates namespaces in the factory methods`createElementNS()` ,`createAttributeNS()` and`setAttributeNS()` .`NamespaceErr` is now raised for a malformed qualified name,
for a prefix with an empty namespace, and for illegal use
of the`xml` and`xmlns` prefixes.
(Contributed by Serhiy Storchaka in gh-156665.)
- `toprettyxml()` in`xml.dom.minidom` and`indent()` in`xml.etree.ElementTree` no longer add whitespace inside an element
which is marked with`xml:space="preserve"` or which contains text.`toprettyxml()` also takes into account
the content model declared in the DTD.
(Contributed by Serhiy Storchaka in gh-81623.)
- Add `GetSpecifiedAttributeCount()` method
to the`XML parser` objects.
It tells how many of the reported attributes were given in the start tag
rather than defaulted from the DTD.
(Contributed by Jason Orendorff and Serhiy Storchaka in gh-44871.)
- The `writexml()` ,`toxml()` and`toprettyxml()` methods
now work for`DocumentFragment` nodes in`xml.dom.minidom` .
(Contributed by Serhiy Storchaka in gh-54092.)
- `XMLPullParser` and`iterparse()` now support the*target* parameter.
The reported object is the value returned by the corresponding method of
the target, so a large document can be parsed incrementally without
building a tree for it.
(Contributed by Serhiy Storchaka in gh-63102.)
- Added constant `VERSION_INFO` in the`XML parser` module, which provides information
about the version of the Expat library that was used for building the module.`version_info` is now a named tuple.
(Contributed by Serhiy Storchaka in gh-157473.)

### zipfileÂ¶

- Add `ZipFile.remove()` to remove a member
from an archiveâs central directory, and`ZipFile.repack()` to reclaim the space used
by the local file entries of removed members.
(Contributed by Danny Lin in gh-51067.)

### zlibÂ¶

- Added constants `ZLIB_VERSION_INFO` ,`zlib_version_info` , and`ZLIBNG_VERSION_INFO` ,
which provide information about the version of the zlib and the zlib-ng
libraries in use.
Added`zlib_version` as an alias of`ZLIB_RUNTIME_VERSION` .
(Contributed by Serhiy Storchaka in gh-117404.)

## OptimizationsÂ¶

- Interpreter startup is faster, and several common operations have been optimized. Improvements include less work during garbage collection and faster construction of `list` ,`dict` ,`bytes` , and`bytearray` objects.
Startup performance is tracked on the Python Speed Center.
(Contributed by Pablo Galindo Salgado, Cody Maloney, and Pieter Eendebak in
gh-159137, gh-160858, gh-160913, and gh-87613.)
- Speed up iteration over asynchronous generators: values yielded to `async for` no longer create and discard a`StopIteration` exception
at every nesting level. The`async_generators` pyperformance benchmark is
about 30% faster.
(Contributed by Shamil Abdulaev in gh-158304.)

### astÂ¶

- AST nodes now store attributes inline with shared keys instead of allocating a separate dictionary for each node. Benchmarks of this change measured about 16% faster `ast.parse()` calls and 52% less
memory used by the resulting ASTs.
(Contributed by Lysandros Nikolaou and Pablo Galindo Salgado
in gh-153568.)

### datetime and timeÂ¶

- `datetime.datetime.strptime()` ,`datetime.date.strptime()` ,`datetime.time.strptime()` , and`time.strptime()` are faster
for common numeric formats such as`%Y-%m-%d` and`%Y-%m-%d %H:%M:%S` .
(Contributed by Paul Ganssle in gh-158252, based on work by
Stan Ulbrych.)

### reÂ¶

- Character class escapes ( `\d` ,`\D` ,`\s` ,`\S` ,`\w` and`\W` )
outside a character set, and character sets containing a single such escape
(such as`[\d]` or`[^\s]` ), are now compiled to a single`CATEGORY` opcode instead of being wrapped in an`IN` block.  This speeds up matching
of patterns such as`\d+` and reduces the size of the compiled byte code.
(Contributed by Serhiy Storchaka in gh-152033 and Pieter Eendebak in
gh-152056.)

### uuidÂ¶

- C implementations accelerate UUID generation, construction, and string conversion. Microbenchmarks comparing the C and pure Python implementations measured the following speedups: Operation Speedup `uuid.uuid4()` generation15.01x `uuid.uuid7()` generation29.64x `uuid.UUID` from a string6.76x `uuid.UUID` from bytes5.16x `str()` conversion of a UUID6.66x (Contributed by Yury Selivanov in gh-139123.)

## RemovedÂ¶

### annotationlibÂ¶

- The `annotationlib.ForwardRef._evaluate()` method
which has been deprecated since Python 3.14.
Use`annotationlib.ForwardRef.evaluate()` or`typing.evaluate_forward_ref()` instead.

### arrayÂ¶

- The `'u'` format code (`wchar_t` ) which has been deprecated in
documentation since Python 3.3 and at runtime since Python 3.13.
Use`'w'` format code instead (`Py_UCS4` , always 4 bytes).

### asyncioÂ¶

- The `asyncio.iscoroutinefunction()` which has been deprecated since Python 3.14.
Use`inspect.iscoroutinefunction()` instead.
- The event loop policy system, including the `asyncio.AbstractEventLoopPolicy` ,`asyncio.DefaultEventLoopPolicy` ,`asyncio.WindowsSelectorEventLoopPolicy` and`asyncio.WindowsProactorEventLoopPolicy` classes and the`asyncio.get_event_loop_policy()` and`asyncio.set_event_loop_policy()` functions,
which have been deprecated since Python 3.14.
Use`asyncio.run()` or`asyncio.Runner` with a custom*loop_factory* instead.

### functoolsÂ¶

- Remove support for calling the Python implementation of `functools.reduce()` with*function* or*sequence* as keyword arguments,
deprecated since Python 3.14.  These parameters are now positional-only.
(Contributed by Nikita Sobolev in gh-149537.)

### loggingÂ¶

- Remove support for custom logging handlers with the *strm* argument,
deprecated since Python 3.14.
Define handlers with the*stream* argument instead.
(Contributed by Nikita Sobolev in gh-149598.)

### mimetypesÂ¶

- Remove support for undotted extensions in `mimetypes.MimeTypes.add_type()` , deprecated since Python 3.14.
Valid extensions must now start with a`'.'` or be empty;
undotted extensions raise a`ValueError` .
(Contributed by Nikita Sobolev in gh-149720.)

### shutilÂ¶

- The `ExecError` exception which has been deprecated since Python 3.14.
It has not been used by any function in`shutil` since Python 3.4.
(Contributed by Stan Ulbrych in gh-149567.)

### symtableÂ¶

- The `symtable.Class.get_methods()` method
which has been deprecated since Python 3.14.

### sysÂ¶

- The `_enablelegacywindowsfsencoding()` function
which has been deprecated since Python 3.13.
Use the`PYTHONLEGACYWINDOWSFSENCODING` environment variable instead.
(Contributed by Stan Ulbrych in gh-149595.)

### sysconfigÂ¶

- The `sysconfig.expand_makefile_vars()` function
which has been deprecated since Python 3.14.
Use the`vars` argument of`sysconfig.get_paths()` instead.
(Contributed by Stan Ulbrych in gh-149499.)

### tarfileÂ¶

- The undocumented and unused `tarfile.TarFile.tarfile` attribute
has been deprecated since Python 3.13.

### xml.etree.ElementTreeÂ¶

- The undocumented `xml.etree.ElementTree.ElementTree.write_c14n()` method
and the support of`method="c14n"` in`write()` and`tostring()` .
They never worked and always raised`ValueError` .
Use`xml.etree.ElementTree.canonicalize()` instead.
(Contributed by Daniel Hillier in gh-118318.)

## DeprecatedÂ¶

### New deprecationsÂ¶

- `abc` :
  - `abc.abstractclassmethod` ,`abc.abstractstaticmethod` ,
and`abc.abstractproperty` , soft-deprecated since Python 3.3,
now raise a`DeprecationWarning` .
These classes will be removed in Python 3.21;
use`abc.abstractmethod()` with`classmethod()` ,`staticmethod()` , and`property` respectively instead.
- `ast` :
  - Classes `slice` ,`Index` ,`ExtSlice` ,`Suite` ,`Param` ,`AugLoad` and`AugStore` , deprecated since Python 3.9, are no longer
imported by`from ast import *` and issue a deprecation warning on
use. The classes are slated for removal in Python 3.21. These types are not
generated by the parser or accepted by the code generator.
  - The `dims` property of`ast.Tuple` objects, deprecated since Python
3.9, now issues a deprecation warning on use. This property is slated for
removal in 3.21. Use`ast.Tuple.elts` instead.
- `__author__` ,`__credits__` ,`__copyright__` and`__date__` :
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
- `functools` :
  - Using a return annotation to infer the dispatch type for `functools.singledispatch()` or`functools.singledispatchmethod` now emits a`DeprecationWarning` and will raise`TypeError` in Python 3.21.
Annotate the dispatch parameter or pass the dispatch type explicitly
instead.
(Contributed by Bartosz SÅawecki in gh-143465.)
- `struct` :
  - The `'F'` and`'D'` type codes, soft-deprecated since Python 3.15,
are now deprecated.  These codes will be removed in Python 3.21.
Use the two-letter forms`'Zf'` and`'Zd'` instead.
(Contributed by Sergey B Kirpichev in gh-121249.)
- `tempfile` :
  - The private `tempfile._TemporaryFileWrapper` name is deprecated
and is slated for removal in Python 3.21.
Use the new public`tempfile.TemporaryFileWrapper` instead,
which is the return type of`tempfile.NamedTemporaryFile()` .
- `tkinter` :
  - `tkinter.filedialog.askopenfiles()` is deprecated and slated for
removal in Python 3.19.  Opening several files at once is error-prone, and
the returned list cannot be used in a`with` statement.  Iterate
over the names returned by`askopenfilenames()` and
open them one by one instead.
(Contributed by Serhiy Storchaka in gh-152638.)
- `typing` :
  - `typing.AnyStr` now raises`DeprecationWarning` when used and is scheduled to be removed in Python 3.21.
Use**PEP 695** syntax instead.
(Contributed by Nikita Sobolev in gh-150190.)
- `urllib.parse` :
  - Accepting objects with false values (like `0` and`[]` ) except empty
strings, byte-like objects and`None` is now deprecated in the URL
parsing functions, and is scheduled to raise`TypeError` in Python
3.18.  This extends the same deprecation made in Python 3.14 for`parse_qsl()` ,`parse_qs()` , and`urlencode()` .
(Contributed by Jacob Walls in gh-63293.)

### Pending removal in Python 3.17Â¶

- `datetime` :
  - `strptime()` calls using a format string containing`%e` (day of month) without a year.
This has been deprecated since Python 3.15.
(Contributed by Stan Ulbrych in gh-70647.)
- `collections.abc` :
  - `collections.abc.ByteString` is scheduled for removal in Python 3.17.Use `isinstance(obj, collections.abc.Buffer)` to test if`obj` implements the buffer protocol at runtime. For use
in type annotations, either use`Buffer` or a union
that explicitly specifies the types your code supports (e.g.,`bytes | bytearray | memoryview` ).`ByteString` was originally intended to be an abstract class that
would serve as a supertype of both`bytes` and`bytearray` .
However, since the ABC never had any methods, knowing that an object was an
instance of`ByteString` never actually told you anything useful
about the object. Other common buffer types such as`memoryview` were also never understood as subtypes of`ByteString` (either at
runtime or by static type checkers).See **PEP 688** for more details.
(Contributed by Shantanu Jain in gh-91896.)
- `encodings` :
  - Passing non-ascii *encoding* names to`encodings.normalize_encoding()` is deprecated and scheduled for removal in Python 3.17.
(Contributed by Stan Ulbrych in gh-136702.)
- `profile` :
  - The `profile` module is deprecated and will be removed in
Python 3.17. Use`profiling.tracing` instead, which provides a
compatible API. See**PEP 799** for details.
(Contributed by Pablo Galindo and LÃ¡szlÃ³ Kiss KollÃ¡r in gh-138122.)
- `webbrowser` :
  - `webbrowser.MacOSXOSAScript` is deprecated in favour of`webbrowser.MacOS` . (gh-137586)
- `typing` :
  - Before Python 3.14, old-style unions were implemented using the private class `typing._UnionGenericAlias` . This class is no longer needed for the implementation,
but it has been retained for backward compatibility, with removal scheduled for Python
3.17. Users should use documented introspection helpers like`typing.get_origin()` and`typing.get_args()` instead of relying on private implementation details.
  - `typing.ByteString` , deprecated since Python 3.9, is scheduled for removal in
Python 3.17.Use `isinstance(obj, collections.abc.Buffer)` to test if`obj` implements the buffer protocol at runtime. For use
in type annotations, either use`Buffer` or a union
that explicitly specifies the types your code supports (e.g.,`bytes | bytearray | memoryview` ).`ByteString` was originally intended to be an abstract class that
would serve as a supertype of both`bytes` and`bytearray` .
However, since the ABC never had any methods, knowing that an object was an
instance of`ByteString` never actually told you anything useful
about the object. Other common buffer types such as`memoryview` were also never understood as subtypes of`ByteString` (either at
runtime or by static type checkers).See **PEP 688** for more details.
(Contributed by Shantanu Jain in gh-91896.)
- `tkinter` :
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

## Porting to Python 3.16Â¶

This section lists previously described changes and other bugfixes that may require changes to your code.

- In `tkinter` , the*name* parameter of the`wait_variable()` ,`setvar()` and`getvar()` methods and the*value* parameter of`setvar()` are now required.  Calling these methods without
them, which formerly defaulted to`'PY_VAR'` and`'1'` , now raises`TypeError` .
(Contributed by Serhiy Storchaka in gh-152587.)
- `urllib.parse.urlsplit()` and`urllib.parse.urlparse()` now raise`ValueError` when the URL is parsed if the port is not a decimal
number or is outside the range 0â65535.  Previously, the error was only
raised when the`port` attribute was read.  This also affects`urljoin()` , and`urldefrag()` for
URLs with a fragment.  Code which never read`port` may now fail
on URLs such as:
  - SCP-style addresses given a scheme, such as `git+ssh://git@github.com:org/repo.git` ;
  - multi-host URLs with ports, such as `mongodb://h1:27017,h2:27017/db` or`postgresql://h1:5432,h2:5433/db` ;
  - URL templates with a placeholder port, such as `http://localhost:{port}/` .
 These are not valid URLs under **RFC 3986** .  To process them, split off
the netloc yourself before calling`urlsplit()` , for
example by replacing it with a placeholder host, or fill in templates
before parsing them.
(Contributed by Miguel Brito and Senthil Kumaran in gh-88037.)
- `uuid.UUID` now consistently raises`TypeError` when the*bytes* or*bytes_le* argument is not a`bytes` object.
Previously, some invalid inputs raised`AssertionError` or`ValueError` .
(Contributed by Yury Selivanov in gh-139123.)
- `xml.dom.minidom` now raises`InvalidCharacterErr` for a name which is not a valid XML name,`WrongDocumentErr` for inserting or setting a node created by another document,`InuseAttributeErr` for setting an attribute node
which belongs to another element,
and`HierarchyRequestErr` for inserting a node into itself
or its descendant.
It also rejects a second element or document type child of a document
and a notation child of a document fragment.
Such operations formerly succeeded
and produced an invalid document or an endless loop.
On the other hand,`Element.removeAttribute()` and`Element.removeAttributeNS()` no longer raise`NotFoundErr` if there is no matching attribute.
Attributes defaulted in the DTD are no longer omitted when parsing.
(Contributed by Jason Orendorff and Serhiy Storchaka in gh-44871.)
- `xml.dom.minidom` now raises`NamespaceErr` for a malformed qualified name, for a prefix with an empty namespace,
and for illegal use of the`xml` and`xmlns` prefixes.
Such operations formerly succeeded and produced an invalid document.
(Contributed by Serhiy Storchaka in gh-156665.)
- `toprettyxml()` in`xml.dom.minidom` and`indent()` in`xml.etree.ElementTree` no longer add whitespace inside an element
which is marked with`xml:space="preserve"` or which contains text,
because this changed the content of the element.`toprettyxml()` also takes into account
the content model declared in the DTD.
(Contributed by Serhiy Storchaka in gh-81623.)
- On Windows, seeking a pipe now fails instead of silently appearing to succeed: `os.lseek()` and`seek()` raise`OSError` ,
and`seekable()` returns`False` .  As a consequence,
opening a pipe in a read-write binary mode (`'r+b'` or`'w+b'` ) now
raises`io.UnsupportedOperation` unless buffering is disabled.
(Contributed by An Long in gh-86768.)

## Build changesÂ¶

- Remove the bundled copy of the libmpdec decimal library from the CPython source tree to simplify maintenance and updates. The `decimal` module
will now unconditionally use the systemâs libmpdec decimal library. Also remove the
now unused`--with-system-libmpdec`**configure** flag.
This change has no impact on binary releases of Python, which have been
built against a separate copy of libmpdec for the past several releases.(Contributed by Sergey B Kirpichev in gh-115119.)
- Add a `--with-build-details-suffix` configure flag to allow
Linux distributions that co-install multiple versions of Python in the
same tree to avoid`build-details.json` clashes.(Contributed by Stefano Rivera in gh-131372.)
- Add the `--with-curses`**configure** option to select the
curses backend for the`curses` and`curses.panel` modules.
In addition to`ncursesw` and`ncurses` , it can now build against the
systemâs native`curses` library (for example on NetBSD or Solaris), with
wide-character support when the library provides it.`--without-curses` excludes the modules from the build.(Contributed by Serhiy Storchaka in gh-136687.)
- Add the `--with-zlib` and`--with-bzip2`**configure** options to select the zlib and bzip2 implementations used to build the`zlib` and`bz2` modules. In addition to the default system
libraries, they can build against zlib-rs and libbzip2-rs, and`--with-zlib=zlib-ng` verifies that the detected zlib is zlib-ng.`--without-zlib` and`--without-bzip2` exclude the modules from the build.(Contributed by Stan Ulbrych in gh-139314.)
- Add the `--with-hwaddress-sanitizer`**configure** option to
build with HWAddressSanitizer.(Contributed by Stan Ulbrych, Florian Mayer and AnnaAr321 in gh-156049.)
- iOS builds can now be configured as free-threaded or debug builds. (Contributed by Russell Keith-Magee in gh-159385.)

### Build support for tvOS, watchOS, and visionOSÂ¶

Added build support for Appleâs tvOS, watchOS, and visionOS.
These platforms are not currently included in the supported
platform tiers defined by **PEP 11**.
See the tvOS,
watchOS, and
visionOS build instructions.

(Contributed by Russell Keith-Magee in gh-131999.)

## C API changesÂ¶

### New featuresÂ¶

- Add a new way to efficiently create a `frozendict` instance by building
a`dict` and âfreezingâ it:`PyDict_AsFrozenDictAndClear()` .
(Contributed by Nikita Sobolev in gh-153413.)
- Add `Py_HashBuffer()` to the limited C API.
(Contributed by Victor Stinner in gh-148233.)
- Add `Py_SETREF` and`Py_XSETREF` macros to the limited C
API.
(Contributed by Nathan Goldbaum and Victor Stinner in gh-158842.)
- Add the PyUnicodeWriter API to the limited C API. (Contributed by Victor Stinner in gh-161172.)
- Add `PyUnstable_InterpreterFrame_GetLocal()` to read a local variable
of an internal interpreter frame by its localsplus index, and`PyUnstable_Code_GetLocalPlusNames()` to get the matching variable
names.
(Contributed by Guilherme Leobas in gh-156133.)

### Porting to Python 3.16Â¶

- `PyType_ClearCache()` is now a no-op as the type cache is now
implemented per-type. It still returns the current version tag.
- Functions reading marshalled data from a FILE*, such as `PyMarshal_ReadObjectFromFile()` ,
now raise`OSError` for I/O errors
and`KeyboardInterrupt` for interrupted reading,
instead of`EOFError` .
(Contributed by Serhiy Storchaka in gh-155907.)
- `PyMarshal_WriteLongToFile()` and`PyMarshal_WriteObjectToFile()` now set the error indicator for I/O errors and interrupted writing,
instead of ignoring them.`PyMarshal_WriteObjectToFile()` now also sets the error indicator
if the value cannot be marshalled.
(Contributed by Serhiy Storchaka in gh-155907.)
- `_Py_c_abs()` no longer sets`errno` to zero on success,
but rather leaves it unchanged.
(Contributed by Sergey B Kirpichev in gh-155526.)

### Deprecated C APIsÂ¶

- `PyGen_New()` ,`PyGen_NewWithQualName()` ,`PyCoro_New()` ,
and`PyAsyncGen_New()` are deprecated.
They are scheduled for removal in 3.18.
- Deprecate `PyInitConfig_GetExitCode()` ,`PyStatus_Exit()` ,
and`PyStatus_IsExit()` functions, and`PyStatus.exitcode` member.`Py_InitializeFromInitConfig()` and`Py_InitializeFromConfig()` can no longer return an exit code.
Instead, if a command line option wants to exit Python, the option is
now processed in`Py_RunMain()` .
(Contributed by Victor Stinner in gh-158080.)
- `PyModule_GetFilename()` is no longer deprecated, but using`PyModule_GetFilenameObject()` instead is still recommended.
(Contributed by Victor Stinner in gh-154757.)
- Soft deprecate functions modifying Unicode strings: `PyUnicode_New()` ,`PyUnicode_CopyCharacters()` ,`PyUnicode_Fill()` ,`PyUnicode_Resize()` ,`PyUnicode_WRITE()` and`PyUnicode_WriteChar()` .
Use the safer PyUnicodeWriter API instead.
(Contributed by Victor Stinner in gh-157710.)

#### Pending removal in Python 3.18Â¶

- The following private functions are deprecated and planned for removal in Python 3.18: 
  - `_PyBytes_Join()` : use`PyBytes_Join()` .
  - `_PyDict_GetItemStringWithError()` : use`PyDict_GetItemStringRef()` .
  - `_PyDict_Pop()` : use`PyDict_Pop()` .
  - `_PyLong_Sign()` : use`PyLong_GetSign()` .
  - `_PyLong_FromDigits()` and`_PyLong_New()` :
use`PyLongWriter_Create()` .
  - `_PyThreadState_UncheckedGet()` : use`PyThreadState_GetUnchecked()` .
  - `_PyUnicode_AsString()` : use`PyUnicode_AsUTF8()` .
  - `_PyUnicodeWriter_Init()` :
replace`_PyUnicodeWriter_Init(&writer)` with`writer = PyUnicodeWriter_Create(0)` .
  - `_PyUnicodeWriter_Finish()` :
replace`_PyUnicodeWriter_Finish(&writer)` with`PyUnicodeWriter_Finish(writer)` .
  - `_PyUnicodeWriter_Dealloc()` :
replace`_PyUnicodeWriter_Dealloc(&writer)` with`PyUnicodeWriter_Discard(writer)` .
  - `_PyUnicodeWriter_WriteChar()` :
replace`_PyUnicodeWriter_WriteChar(&writer, ch)` with`PyUnicodeWriter_WriteChar(writer, ch)` .
  - `_PyUnicodeWriter_WriteStr()` :
replace`_PyUnicodeWriter_WriteStr(&writer, str)` with`PyUnicodeWriter_WriteStr(writer, str)` .
  - `_PyUnicodeWriter_WriteSubstring()` :
replace`_PyUnicodeWriter_WriteSubstring(&writer, str, start, end)` with`PyUnicodeWriter_WriteSubstring(writer, str, start, end)` .
  - `_PyUnicodeWriter_WriteASCIIString()` :
replace`_PyUnicodeWriter_WriteASCIIString(&writer, str, size)` with`PyUnicodeWriter_WriteASCII(writer, str, size)` .
  - `_PyUnicodeWriter_WriteLatin1String()` :
replace`_PyUnicodeWriter_WriteLatin1String(&writer, str, size)` by
decoding the string with`PyUnicode_DecodeLatin1()` , writing the
result with`PyUnicodeWriter_WriteStr()` , and releasing the result
with`Py_DECREF()` .
  - `_PyUnicodeWriter_Prepare()` : (no replacement).
  - `_PyUnicodeWriter_PrepareKind()` : (no replacement).
  - `_Py_HashPointer()` : use`Py_HashPointer()` .
  - `_Py_fopen_obj()` : use`Py_fopen()` .
  - `PyGen_New()` : (no replacement).
  - `PyGen_NewWithQualName()` : (no replacement).
  - `PyCoro_New()` : (no replacement).
  - `PyAsyncGen_New()` : (no replacement).
 The pythoncapi-compat project can be used to get these new public functions on Python 3.13 and older. (Contributed by Victor Stinner in gh-128863.)

#### Pending removal in Python 3.19Â¶

- **PEP 456** embedders support for the string hashing scheme definition.

#### Pending removal in Python 3.20Â¶

- `_PyObject_CallMethodId()` ,`_PyObject_GetAttrId()` and`_PyUnicode_FromId()` are deprecated since 3.15 and will be removed in
3.20. Instead, use`PyUnicode_InternFromString()` and cache the result in
the module state, then call`PyObject_CallMethod()` or`PyObject_GetAttr()` .
(Contributed by Victor Stinner in gh-141049.)
- The `cval` field in`PyComplexObject` (gh-128813).
Use`PyComplex_AsCComplex()` and`PyComplex_FromCComplex()` to convert a Python complex number to/from the C`Py_complex` representation.
- Macros `Py_MATH_PIl` and`Py_MATH_El` .

#### Pending removal in future versionsÂ¶

The following APIs are deprecated and will be removed, although there is currently no date scheduled for their removal.

- `Py_TPFLAGS_HAVE_FINALIZE` :
Unneeded since Python 3.8.
- `PyErr_Fetch()` :
Use`PyErr_GetRaisedException()` instead.
- `PyErr_NormalizeException()` :
Use`PyErr_GetRaisedException()` instead.
- `PyErr_Restore()` :
Use`PyErr_SetRaisedException()` instead.
- `PyOS_AfterFork()` :
Use`PyOS_AfterFork_Child()` instead.
- `PySlice_GetIndicesEx()` :
Use`PySlice_Unpack()` and`PySlice_AdjustIndices()` instead.
- `PyUnicode_READY()` :
Unneeded since Python 3.12
- `PyErr_Display()` :
Use`PyErr_DisplayException()` instead.
- `_PyErr_ChainExceptions()` :
Use`_PyErr_ChainExceptions1()` instead.
- `PyBytesObject.ob_shash` member:
call`PyObject_Hash()` instead.
- Thread Local Storage (TLS) API: 
  - `PyThread_create_key()` :
Use`PyThread_tss_alloc()` instead.
  - `PyThread_delete_key()` :
Use`PyThread_tss_free()` instead.
  - `PyThread_set_key_value()` :
Use`PyThread_tss_set()` instead.
  - `PyThread_get_key_value()` :
Use`PyThread_tss_get()` instead.
  - `PyThread_delete_key_value()` :
Use`PyThread_tss_delete()` instead.
  - `PyThread_ReInitTLS()` :
Unneeded since Python 3.7.

### Removed C APIsÂ¶

- Remove 21 global configuration variables, deprecated in Python 3.12. To initialize Python, the PyInitConfig C API can be used instead. To read these variables at runtime, `PyConfig_Get()` can be used
instead.
Removed variables with their PyInitConfig C API and`PyConfig_Get()` replacement:
  - `Py_DebugFlag` : Use`"parser_debug"` instead.
  - `Py_VerboseFlag` : Use`"verbose"` instead.
  - `Py_QuietFlag` : Use`"quiet"` instead.
  - `Py_InteractiveFlag` : Use`"interactive"` instead.
  - `Py_InspectFlag` : Use`"inspect"` instead.
  - `Py_OptimizeFlag` : Use`"optimization_level"` instead.
  - `Py_NoSiteFlag` : Use`"site_import"` instead (opposite value).
  - `Py_BytesWarningFlag` : Use`"bytes_warning"` instead.
  - `Py_FrozenFlag` : Use`"pathconfig_warnings"` instead (opposite value).
  - `Py_IgnoreEnvironmentFlag` : Use`"use_environment"` instead (opposite value).
  - `Py_DontWriteBytecodeFlag` : Use`"write_bytecode"` instead (opposite value).
  - `Py_NoUserSiteDirectory` : Use`"user_site_directory"` instead (opposite value).
  - `Py_UnbufferedStdioFlag` : Use`"buffered_stdio"` instead (opposite value).
  - `Py_HashRandomizationFlag` : Use`"hash_seed"` instead.
  - `Py_IsolatedFlag` : Use`"isolated"` instead.
  - `Py_LegacyWindowsFSEncodingFlag` : Use`"legacy_windows_fs_encoding"` instead.
  - `Py_LegacyWindowsStdioFlag` : Use`"legacy_windows_stdio"` instead.
  - `Py_FileSystemDefaultEncoding` ,`Py_HasFileSystemDefaultEncoding` : Use`"filesystem_encoding"` instead.
  - `Py_FileSystemDefaultEncodeErrors` : Use`"filesystem_errors"` instead.
  - `Py_UTF8Mode` : Use`"utf8_mode"` instead.
 (Contributed by Victor Stinner in gh-158001.)
- Remove `PyEval_InitThreads()` function which did nothing since Python
3.7 and was deprecated since Python 3.9.
(Contributed by Victor Stinner in gh-154757.)
- Remove `Py_SetProgramName()` and`Py_SetPythonHome()` functions, deprecated since Python 3.11. Instead, use PyInitConfig C
API to set respectively`"program_name"` and`"home"` .
(Contributed by Victor Stinner in gh-158001.)
- Remove `PySys_SetArgv()` and`PySys_SetArgvEx()` functions,
deprecated since Python 3.11. Instead, set directly`sys.argv` , and
maybe also`sys.path` .
(Contributed by Victor Stinner in gh-158001.)
- The `PyUnstable_ExecutableKinds` array, as well as the macros`PyUnstable_EXECUTABLE_KIND_SKIP` ,`PyUnstable_EXECUTABLE_KIND_PY_FUNCTION` ,`PyUnstable_EXECUTABLE_KIND_BUILTIN_FUNCTION` ,`PyUnstable_EXECUTABLE_KIND_METHOD_DESCRIPTOR` and`PyUnstable_EXECUTABLE_KINDS` , were removed.(Contributed by Peters Bierma and Viktorin in gh-152105.)
