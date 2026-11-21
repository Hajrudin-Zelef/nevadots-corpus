---
id: collect-261121-veille-ia/docs-techniques/changelog-2-18
title: "Changelog"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["test", "support", "documentation", "regrtest", "android", "some", "remove", "command"]
source: https://docs.python.org/release/3.15.1/whatsnew/changelog.html
source_anchor: ""
source_lines: [25501, 27000]
section: "Docs_Techniques"
---
- bpo-30052: the link targets for `bytes()` and`bytearray()` are
now their respective type definitions, rather than the corresponding
builtin function entries. Use bytes and
bytearray to reference the latter. In order to
ensure this and future cross-reference updates are applied automatically,
the daily documentation builds now disable the default output caching
features in Sphinx.
- bpo-26985: Add missing info of code object in inspect documentation.
- bpo-19824: Improve the documentation for, and links to, template strings by emphasizing their utility for internationalization, and by clarifying some usage constraints. (See also: bpo-20314, bpo-12518)
- bpo-28929: Link the documentation to its source file on GitHub.
- bpo-25008: Document smtpd.py as effectively deprecated and add a pointer to aiosmtpd, a third-party asyncio-based replacement.
- bpo-26355: Add canonical header link on each page to corresponding major version of the documentation. Patch by Matthias Bussonnier.
- bpo-29349: Fix Python 2 syntax in code for building the documentation.
- bpo-23722: The data model reference and the porting section in the 3.6 Whatâs New guide now cover the additional `__classcell__` handling
needed for custom metaclasses to fully support**PEP 487** and
zero-argument`super()` .
- bpo-28513: Documented command-line interface of zipfile.

### TestsÂ¶

- bpo-29639: test.support.HOST is now âlocalhostâ, a new HOSTv4 constant has been added for your `127.0.0.1` needs, similar to the existing HOSTv6
constant.
- bpo-31320: Silence traceback in test_ssl
- bpo-31346: Prefer PROTOCOL_TLS_CLIENT and PROTOCOL_TLS_SERVER protocols for SSLContext.
- bpo-25674: Remove sha256.tbs-internet.com ssl test
- bpo-30715: Address ALPN callback changes for OpenSSL 1.1.0f. The latest version behaves like OpenSSL 1.0.2 and no longer aborts handshake.
- bpo-30822: regrtest: Exclude tzdata from regrtest âall. When running the test suite using âuse=all / -u all, exclude tzdata since it makes test_datetime too slow (15-20 min on some buildbots) which then times out on some buildbots. Fix also regrtest command line parser to allow passing -u extralargefile to run test_zipfile64.
- bpo-30695: Add the `set_nomemory(start, stop)` and`remove_mem_hooks()` functions to the`_testcapi` module.
- bpo-30357: test_thread: setUp() now uses support.threading_setup() and support.threading_cleanup() to wait until threads complete to avoid random side effects on following tests. Initial patch written by Grzegorz Grzywacz.
- bpo-30197: Enhanced functions swap_attr() and swap_item() in the test.support module. They now work when delete replaced attribute or item inside the with statement. The old value of the attribute or item (or None if it doesnât exist) now will be assigned to the target of the âasâ clause, if there is one.
- bpo-24932: Use proper command line parsing in _testembed
- bpo-28950: Disallow -j0 to be combined with -T/-l in regrtest command line arguments.
- bpo-28683: Fix the tests that bind() a unix socket and raise PermissionError on Android for a non-root user.
- bpo-26936: Fix the test_socket failures on Android - getservbyname(), getservbyport() and getaddrinfo() are broken on some Android API levels.
- bpo-28666: Now test.support.rmtree is able to remove unwritable or unreadable directories.
- bpo-23839: Various caches now are cleared before running every test file.
- bpo-26944: Fix test_posix for Android where âid -Gâ is entirely wrong or missing the effective gid.
- bpo-28409: regrtest: fix the parser of command line arguments.
- bpo-28217: Adds _testconsole module to test console input.
- bpo-26939: Add the support.setswitchinterval() function to fix test_functools hanging on the Android armv7 qemu emulator.

### BuildÂ¶

- bpo-31354: Allow `--with-lto` to be used on all builds, not just```
make
profile-opt
```
.
- bpo-31370: Remove support for building âwithout-threads. This option is not really useful anymore in the 21st century. Removing lots of conditional paths allows us to simplify the code base, including in difficult to maintain low-level internal code.
- bpo-31341: Per **PEP 11** , support for the IRIX operating system was
removed.
- bpo-30854: Fix compile error when compiling âwithout-threads. Patch by Masayuki Yamamoto.
- bpo-30687: Locate msbuild.exe on Windows when building rather than vcvarsall.bat
- bpo-20210: Support the *disabled* marker in Setup files. Extension modules
listed after this marker are not built at all, neither by the Makefile nor
by setup.py.
- bpo-29941: Add `--with-assertions` configure flag to explicitly enable C`assert()` checks. Defaults to off.`--with-pydebug` implies`--with-assertions` .
- bpo-28787: Fix out-of-tree builds of Python when configured with `--with--dtrace` .
- bpo-29243: Prevent unnecessary rebuilding of Python during `make test` ,`make install` and some other make targets when configured with`--enable-optimizations` .
- bpo-23404: Donât regenerate generated files based on file modification time anymore: the action is now explicit. Replace `make touch` with`make regen-all` .
- bpo-29643: Fix `--enable-optimization` didnât work.
- bpo-27593: sys.version and the platform module python_build(), python_branch(), and python_revision() functions now use git information rather than hg when building from a repo.
- bpo-29572: Update Windows build and OS X installers to use OpenSSL 1.0.2k.
- bpo-27659: Prohibit implicit C function declarations: use `-Werror=implicit-function-declaration` when possible (GCC and Clang,
but it depends on the compiler version). Patch written by Chi Hsuan Yen.
- bpo-29384: Remove old Be OS helper scripts.
- bpo-26851: Set Android compilation and link flags.
- bpo-28768: Fix implicit declaration of function _setmode. Patch by Masayuki Yamamoto
- bpo-29080: Removes hard dependency on hg.exe from PCBuild/build.bat
- bpo-23903: Added missed names to PC/python3.def.
- bpo-28762: lockf() is available on Android API level 24, but the F_LOCK macro is not defined in android-ndk-r13.
- bpo-28538: Fix the compilation error that occurs because if_nameindex() is available on Android API level 24, but the if_nameindex structure is not defined.
- bpo-20211: Do not add the directory for installing C header files and the directory for installing object code libraries to the cross compilation search paths. Original patch by Thomas Petazzoni.
- bpo-28849: Do not define sys.implementation._multiarch on Android.
- bpo-10656: Fix out-of-tree building on AIX. Patch by Tristan Carel and Michael Haubenwallner.
- bpo-26359: Rename âwith-optimiations to âenable-optimizations.
- bpo-28444: Fix missing extensions modules when cross compiling.
- bpo-28208: Update Windows build and OS X installers to use SQLite 3.14.2.
- bpo-28248: Update Windows build and OS X installers to use OpenSSL 1.0.2j.
- bpo-21124: Fix building the _struct module on Cygwin by passing `NULL` instead of`&PyType_Type` to PyVarObject_HEAD_INIT.  Patch by Masayuki
Yamamoto.
- bpo-13756: Fix building extensions modules on Cygwin. Patch by Roumen Petrov, based on original patch by Jason Tishler.
- bpo-21085: Add configure check for siginfo_t.si_band, which Cygwin does not provide. Patch by Masayuki Yamamoto with review and rebase by Erik Bray.
- bpo-28258: Fixed build with Estonian locale (python-config and distclean targets in Makefile). Patch by Arfrever Frehtes Taifersar Arahesis.
- bpo-26661: setup.py now detects system libffi with multiarch wrapper.
- bpo-27979: A full copy of libffi is no longer bundled for use when building _ctypes on non-OSX UNIX platforms. An installed copy of libffi is now required when building _ctypes on such platforms.
- bpo-15819: Remove redundant include search directory option for building outside the source tree.
- bpo-28676: Prevent missing âgetentropyâ declaration warning on macOS. Patch by Gareth Rees.

### WindowsÂ¶

- bpo-31392: Update Windows build to use OpenSSL 1.1.0f
- bpo-30389: Adds detection of Visual Studio 2017 to distutils on Windows.
- bpo-31358: zlib is no longer bundled in the CPython source, instead it is downloaded on demand just like bz2, lzma, OpenSSL, Tcl/Tk, and SQLite.
- bpo-31340: Change to building with MSVC v141 (included with Visual Studio 2017)
- bpo-30581: os.cpu_count() now returns the correct number of processors on Windows when the number of logical processors is greater than 64.
- bpo-30916: Pre-build OpenSSL, Tcl and Tk and include the binaries in the build.
- bpo-30731: Add a missing xmlns to python.manifest so that it matches the schema.
- bpo-30291: Allow requiring 64-bit interpreters from py.exe using -64 suffix. Contributed by Steve (Gadget) Barnes.
- bpo-30362: Adds list options (-0, -0p) to py.exe launcher. Contributed by Steve Barnes.
- bpo-23451: Fix socket deprecation warnings in socketmodule.c. Patch by Segev Finer.
- bpo-30450: The build process on Windows no longer depends on Subversion, instead pulling external code from GitHub via a Python script. If Python 3.6 is not found on the system (via `py -3.6` ), NuGet is used to
download a copy of 32-bit Python.
- bpo-29579: Removes readme.txt from the installer.
- bpo-25778: winreg does not truncate string correctly (Patch by Eryk Sun)
- bpo-28896: Deprecate WindowsRegistryFinder and disable it by default
- bpo-28522: Fixes mishandled buffer reallocation in getpathp.c
- bpo-28402: Adds signed catalog files for stdlib on Windows.
- bpo-28333: Enables Unicode for ps1/ps2 and input() prompts. (Patch by Eryk Sun)
- bpo-28251: Improvements to help manuals on Windows.
- bpo-28110: launcher.msi has different product codes between 32-bit and 64-bit
- bpo-28161: Opening CON for write access fails
- bpo-28162: WindowsConsoleIO readall() fails if first line starts with Ctrl+Z
- bpo-28163: WindowsConsoleIO fileno() passes wrong flags to _open_osfhandle
- bpo-28164: _PyIO_get_console_type fails for various paths
- bpo-28137: Renames Windows path file to ._pth
- bpo-28138: Windows ._pth file should allow import site

### IDLEÂ¶

- bpo-31493: IDLE code context â fix code update and font update timers. Canceling timers prevents a warning message when test_idle completes.
- bpo-31488: IDLE - Update non-key options in former extension classes. When applying configdialog changes, call .reload for each feature class. Change ParenMatch so updated options affect existing instances attached to existing editor windows.
- bpo-31477: IDLE - Improve rstrip entry in doc. Strip trailing whitespace strips more than blank spaces. Multiline string literals are not skipped.
- bpo-31480: IDLE - make tests pass with zzdummy extension disabled by default.
- bpo-31421: Document how IDLE runs tkinter programs. IDLE calls tcl/tk update in the background in order to make live interaction and experimentation with tkinter applications much easier.
- bpo-31414: IDLE â fix tk entry box tests by deleting first. Adding to an int entry is not the same as deleting and inserting because int(ââ) will fail.
- bpo-31051: Rearrange IDLE configdialog GenPage into Window, Editor, and Help sections.
- bpo-30617: IDLE - Add docstrings and tests for outwin subclass of editor. Move some data and functions from the class to module level. Patch by Cheryl Sabella.
- bpo-31287: IDLE - Do not modify tkinter.message in test_configdialog.
- bpo-27099: Convert IDLEâs built-in âextensionsâ to regular features. About 10 IDLE features were implemented as supposedly optional extensions. Their different behavior could be confusing or worse for users and not good for maintenance. Hence the conversion. The main difference for users is that user configurable key bindings for builtin features are now handled uniformly. Now, editing a binding in a keyset only affects its value in the keyset. All bindings are defined together in the system-specific default keysets in config-extensions.def. All custom keysets are saved as a whole in config-extension.cfg. All take effect as soon as one clicks Apply or Ok. The affected events are â<<force-open-completions>>â, â<<expand-word>>â, â<<force-open-calltip>>â, â<<flash-paren>>â, â<<format-paragraph>>â, â<<run-module>>â, â<<check-module>>â, and â<<zoom-height>>â. Any (global) customizations made before 3.6.3 will not affect their keyset-specific customization after 3.6.3. and vice versa. Initial patch by Charles Wohlganger.
- bpo-31206: IDLE: Factor HighPage(Frame) class from ConfigDialog. Patch by Cheryl Sabella.
- bpo-31001: Add tests for configdialog highlight tab. Patch by Cheryl Sabella.
- bpo-31205: IDLE: Factor KeysPage(Frame) class from ConfigDialog. The slightly modified tests continue to pass. Patch by Cheryl Sabella.
- bpo-31130: IDLE â stop leaks in test_configdialog. Initial patch by Victor Stinner.
- bpo-31002: Add tests for configdialog keys tab. Patch by Cheryl Sabella.
- bpo-19903: IDLE: Calltips use `inspect.signature` instead of`inspect.getfullargspec` . This improves calltips for builtins converted
to use Argument Clinic. Patch by Louie Lu.
- bpo-31083: IDLE - Add an outline of a TabPage class in configdialog. Update existing classes to match outline. Initial patch by Cheryl Sabella.
- bpo-31050: Factor GenPage(Frame) class from ConfigDialog. The slightly modified tests continue to pass. Patch by Cheryl Sabella.
- bpo-31004: IDLE - Factor FontPage(Frame) class from ConfigDialog. Slightly modified tests continue to pass. Fix General tests. Patch mostly by Cheryl Sabella.
- bpo-30781: IDLE - Use ttk widgets in ConfigDialog. Patches by Terry Jan Reedy and Cheryl Sabella.
- bpo-31060: IDLE - Finish rearranging methods of ConfigDialog Grouping methods pertaining to each tab and the buttons will aid writing tests and improving the tabs and will enable splitting the groups into classes.
- bpo-30853: IDLE â Factor a VarTrace class out of ConfigDialog. Instance tracers manages pairs consisting of a tk variable and a callback function. When tracing is turned on, setting the variable calls the function. Test coverage for the new class is 100%.
- bpo-31003: IDLE: Add more tests for General tab.
- bpo-30993: IDLE - Improve configdialog font page and tests. In configdialog: Document causal pathways in create_font_tab docstring. Simplify some attribute names. Move set_samples calls to var_changed_font (idea from Cheryl Sabella). Move related functions to positions after the create widgets function. In test_configdialog: Fix test_font_set so not order dependent. Fix renamed test_indent_scale so it tests the widget. Adjust tests for movement of set_samples call. Add tests for load functions. Put all font tests in one class and tab indent tests in another. Except for two lines, these tests completely cover the related functions.
- bpo-30981: IDLE â Add more configdialog font page tests.
- bpo-28523: IDLE: replace âcolourâ with âcolorâ in configdialog.
- bpo-30917: Add tests for idlelib.config.IdleConf. Increase coverage from 46% to 96%. Patch by Louie Lu.
- bpo-30934: Document coverage details for idlelib tests. Add section to idlelib/idle-test/README.txt. Include check that branches are taken both ways. Exclude IDLE-specific code that does not run during unit tests.
- bpo-30913: IDLE: Document ConfigDialog tk Vars, methods, and widgets in docstrings This will facilitate improving the dialog and splitting up the class. Original patch by Cheryl Sabella.
- bpo-30899: IDLE: Add tests for ConfigParser subclasses in config. Patch by Louie Lu.
- bpo-30881: IDLE: Add docstrings to browser.py. Patch by Cheryl Sabella.
- bpo-30851: IDLE: Remove unused variables in configdialog. One is a duplicate, one is set but cannot be altered by users. Patch by Cheryl Sabella.
- bpo-30870: IDLE: In Settings dialog, select font with Up, Down keys as well as mouse. Initial patch by Louie Lu.
- bpo-8231: IDLE: call config.IdleConf.GetUserCfgDir only once.
- bpo-30779: IDLE: Factor ConfigChanges class from configdialog, put in config; test. * In config, put dump test code in a function; run it and unittest in âif __name__ == â__main__â. * Add class config.ConfigChanges based on changes_class_v4.py on bpo issue. * Add class test_config.ChangesTest, partly using configdialog_tests_v1.py. * Revise configdialog to use ConfigChanges; see tracker msg297804. * Revise test_configdialog to match configdialog changes. * Remove configdialog functions unused or moved to ConfigChanges. Cheryl Sabella contributed parts of the patch.
- bpo-30777: IDLE: configdialog - Add docstrings and fix comments. Patch by Cheryl Sabella.
- bpo-30495: IDLE: Improve textview with docstrings, PEP8 names, and more tests. Patch by Cheryl Sabella.
- bpo-30723: IDLE: Make several improvements to parenmatch. Add âparensâ style to highlight both opener and closer. Make âdefaultâ style, which is not default, a synonym for âopenerâ. Make time-delay work the same with all styles. Add help for config dialog extensions tab, including help for parenmatch. Add new tests. Original patch by Charles Wohlganger.
- bpo-30674: IDLE: add docstrings to grep module. Patch by Cheryl Sabella
- bpo-21519: IDLEâs basic custom key entry dialog now detects duplicates properly. Original patch by Saimadhav Heblikar.
- bpo-29910: IDLE no longer deletes a character after commenting out a region by a key shortcut. Add `return 'break'` for this and other
potential conflicts between IDLE and default key bindings.
- bpo-30728: Review and change idlelib.configdialog names. Lowercase method and attribute names. Replace âcolourâ with âcolorâ, expand overly cryptic names, delete unneeded underscores. Replace `import *` with specific
imports. Patches by Cheryl Sabella.
- bpo-6739: IDLE: Verify user-entered key sequences by trying to bind them with tk. Add tests for all 3 validation functions. Original patch by G Polo. Tests added by Cheryl Sabella.
- bpo-15786: Fix several problems with IDLEâs autocompletion box. The following should now work: clicking on selection box items; using the scrollbar; selecting an item by hitting Return. Hangs on MacOSX should no longer happen. Patch by Louie Lu.
- bpo-25514: Add doc subsubsection about IDLE failure to start. Popup no-connection message directs users to this section.
- bpo-30642: Fix reference leaks in IDLE tests. Patches by Louie Lu and Terry Jan Reedy.
- bpo-30495: Add docstrings for textview.py and use PEP8 names. Patches by Cheryl Sabella and Terry Jan Reedy.
- bpo-30290: Help-about: use pep8 names and add tests. Increase coverage to 100%. Patches by Louie Lu, Cheryl Sabella, and Terry Jan Reedy.
- bpo-30303: Add _utest option to textview; add new tests. Increase coverage to 100%. Patches by Louie Lu and Terry Jan Reedy.
- bpo-29071: IDLE colors f-string prefixes (but not invalid ur prefixes).
- bpo-28572: Add 10% to coverage of IDLEâs test_configdialog. Update and augment description of the configuration system.

### Tools/DemosÂ¶

- bpo-30983: gdb integration commands (py-bt, etc.) work on optimized shared builds now, too. **PEP 523** introduced _PyEval_EvalFrameDefault which
inlines PyEval_EvalFrameEx on non-debug shared builds.  This broke the
ability to use py-bt, py-up, and a few other Python-specific gdb
integrations. The problem is fixed by only looking for
_PyEval_EvalFrameDefault frames in python-gdb.py.  Original patch by Bruno
âPolacoâ Penteado.
- bpo-29748: Added the slice index converter in Argument Clinic.
- bpo-24037: Argument Clinic now uses the converter `bool(accept={int})` rather than`int` for semantical booleans.  This avoids repeating the
default value for Python and C and will help in converting to`bool` in
future.
- bpo-29367: python-gdb.py now supports also `method-wrapper` (`wrapperobject` ) objects.
- bpo-28023: Fix python-gdb.py didnât support new dict implementation.
- bpo-15369: The pybench and pystone microbenchmark have been removed from Tools. Please use the new Python benchmark suite https://github.com/python/pyperformance which is more reliable and includes a portable version of pybench working on Python 2 and Python 3.
- bpo-28102: The zipfile module CLI now prints usage to stderr. Patch by Stephen J. Turnbull.

### C APIÂ¶

- bpo-31338: Added the `Py_UNREACHABLE()` macro for code paths which are
never expected to be reached.  This and a few other useful macros are now
documented in the C API manual.
- bpo-30832: Remove own implementation for thread-local storage. CPython has provided the own implementation for thread-local storage (TLS) on Python/thread.c, itâs used in the case which a platform has not supplied native TLS. However, currently all supported platforms (Windows and pthreads) have provided native TLS and defined the Py_HAVE_NATIVE_TLS macro with unconditional in any case.
- bpo-30708: PyUnicode_AsWideCharString() now raises a ValueError if the second argument is NULL and the wchar_t* string contains null characters.
- bpo-16500: Deprecate PyOS_AfterFork() and add PyOS_BeforeFork(), PyOS_AfterFork_Parent() and PyOS_AfterFork_Child().
- bpo-6532: The type of results of PyThread_start_new_thread() and PyThread_get_thread_ident(), and the id parameter of PyThreadState_SetAsyncExc() changed from âlongâ to âunsigned longâ.
- bpo-27867: Function PySlice_GetIndicesEx() is deprecated and replaced with a macro if Py_LIMITED_API is not set or set to the value between 0x03050400 and 0x03060000 (not including) or 0x03060100 or higher. Added functions PySlice_Unpack() and PySlice_AdjustIndices().
- bpo-29083: Fixed the declaration of some public API functions. PyArg_VaParse() and PyArg_VaParseTupleAndKeywords() were not available in limited API. PyArg_ValidateKeywordArguments(), PyArg_UnpackTuple() and Py_BuildValue() were not available in limited API of version < 3.3 when PY_SSIZE_T_CLEAN is defined.
- bpo-28769: The result of PyUnicode_AsUTF8AndSize() and PyUnicode_AsUTF8() is now of type `const char *` rather of`char *` .
- bpo-29058: All stable API extensions added after Python 3.2 are now available only when Py_LIMITED_API is set to the PY_VERSION_HEX value of the minimum Python version supporting this API.
- bpo-28822: The index parameters *start* and*end* of PyUnicode_FindChar()
are now adjusted to behave like`str[start:end]` .
- bpo-28808: PyUnicode_CompareWithASCIIString() now never raises exceptions.
- bpo-28761: The fields name and doc of structures PyMemberDef, PyGetSetDef, PyStructSequence_Field, PyStructSequence_Desc, and wrapperbase are now of type `const char *` rather of`char *` .
- bpo-28748: Private variable _Py_PackageContext is now of type ```
const char
*
```
rather of`char *` .
- bpo-19569: Compiler warnings are now emitted if use most of deprecated functions.
- bpo-28426: Deprecated undocumented functions PyUnicode_AsEncodedObject(), PyUnicode_AsDecodedObject(), PyUnicode_AsDecodedUnicode() and PyUnicode_AsEncodedUnicode().

## Python 3.6.6 finalÂ¶

*Release date: 2018-06-27*

There were no new changes in version 3.6.6.

## Python 3.6.6 release candidate 1Â¶

*Release date: 2018-06-11*

### Core and BuiltinsÂ¶

- bpo-33786: Fix asynchronous generators to handle GeneratorExit in athrow() correctly
- bpo-30654: Fixed reset of the SIGINT handler to SIG_DFL on interpreter shutdown even when there was a custom handler set previously. Patch by Philipp Kerling.
- bpo-33622: Fixed a leak when the garbage collector fails to add an object with the `__del__` method or referenced by it into the`gc.garbage` list.`PyGC_Collect()` can now be called when an
exception is set and preserves it.
- bpo-31849: Fix signed/unsigned comparison warning in pyhash.c.
- bpo-33391: Fix a leak in set_symmetric_difference().
- bpo-28055: Fix unaligned accesses in siphash24(). Patch by Rolf Eike Beer.
- bpo-33231: Fix potential memory leak in `normalizestring()` .
- bpo-29922: Improved error messages in âasync withâ when `__aenter__()` or`__aexit__()` return non-awaitable object.
- bpo-33199: Fix `ma_version_tag` in dict implementation is uninitialized
when copying from key-sharing dict.
- bpo-33041: Fixed jumping when the function contains an `async for` loop.
- bpo-32282: Fix an unnecessary ifdef in the include of VersionHelpers.h in socketmodule on Windows.
- bpo-21983: Fix a crash in `ctypes.cast()` in case the type argument is a
ctypes structured data type. Patch by Eryk Sun and Oren Milman.

### LibraryÂ¶

- bpo-30167: Prevent site.main() exception if PYTHONSTARTUP is set. Patch by Steve Weber.
- bpo-33812: Datetime instance d with non-None tzinfo, but with d.tzinfo.utcoffset(d) returning None is now treated as naive by the astimezone() method.
- bpo-30805: Avoid race condition with debug logging
- bpo-33767: The concatenation ( `+` ) and repetition (`*` ) sequence
operations now raise`TypeError` instead of`SystemError` when
performed on`mmap.mmap` objects.  Patch by Zackery Spytz.
- bpo-32684: Fix gather to propagate cancellation of itself even with return_exceptions.
- bpo-33674: Fix a race condition in SSLProtocol.connection_made() of asyncio.sslproto: start immediately the handshake instead of using call_soon(). Previously, data_received() could be called before the handshake started, causing the handshake to hang or fail.
- bpo-31647: Fixed bug where calling write_eof() on a _SelectorSocketTransport after itâs already closed raises AttributeError.
- bpo-33672: Fix Task.__repr__ crash with Cythonâs bogus coroutines
- bpo-33469: Fix RuntimeError after closing loop that used run_in_executor
- bpo-11874: Use a better regex when breaking usage into wrappable parts. Avoids bogus assertion errors from custom metavar strings.
- bpo-30877: Fixed a bug in the Python implementation of the JSON decoder that prevented the cache of parsed strings from clearing after finishing the decoding. Based on patch by c-fos.
- bpo-33548: tempfile._candidate_tempdir_list should consider common TEMP locations
- bpo-33542: Prevent `uuid.get_node` from using a DUID instead of a MAC on
Windows. Patch by Zvi Effron
- bpo-26819: Fix race condition with `ReadTransport.resume_reading` in
Windows proactor event loop.
- bpo-28556: Minor fixes in typing module: add annotations to `NamedTuple.__new__` , pass`*args` and`**kwds` in`Generic.__new__` .  Original PRs by Paulius Å arka and Chad Dombrova.
- bpo-20087: Updated alias mapping with glibc 2.27 supported locales.
- bpo-33422: Fix trailing quotation marks getting deleted when looking up byte/string literals on pydoc. Patch by AndrÃ©s Delfino.
- bpo-33197: Update error message when constructing invalid inspect.Parameters Patch by Donghee Na.
- bpo-33383: Fixed crash in the get() method of the `dbm.ndbm` database
object when it is called with a single argument.
- bpo-33329: Fix multiprocessing regression on newer glibcs
- bpo-991266: Fix quoting of the `Comment` attribute of`http.cookies.SimpleCookie` .
- bpo-33131: Upgrade bundled version of pip to 10.0.1.
- bpo-33308: Fixed a crash in the `parser` module when converting an ST
object to a tree of tuples or lists with`line_info=False` and`col_info=True` .
- bpo-33263: Fix FD leak in `_SelectorSocketTransport` Patch by Vlad
Starostin.
- bpo-33256: Fix display of `<module>` call in the html produced by`cgitb.html()` . Patch by StÃ©phane Blondon.
- bpo-33203: `random.Random.choice()` now raises`IndexError` for empty
sequences consistently even when called from subclasses without a`getrandbits()` implementation.
- bpo-33224: Update difflib.mdiff() for **PEP 479** .  Convert an uncaught
StopIteration in a generator into a return-statement.
- bpo-33209: End framing at the end of C implementation of `pickle.Pickler.dump()` .
- bpo-32861: The urllib.robotparserâs `__str__` representation now
includes wildcard entries and the âCrawl-delayâ and âRequest-rateâ fields.
Patch by Michael Lazar.
- bpo-33096: Allow ttk.Treeview.insert to insert iid that has a false boolean value. Note iid=0 and iid=False would be same. Patch by Garvit Khatri.
- bpo-33127: The ssl module now compiles with LibreSSL 2.7.1.
- bpo-33021: Release the GIL during fstat() calls, avoiding hang of all threads when calling mmap.mmap(), os.urandom(), and random.seed(). Patch by Nir Soffer.
- bpo-27683: Fix a regression in `ipaddress` that result of`hosts()` is empty when the network is constructed by a tuple
containing an integer mask and only 1 bit left for addresses.
- bpo-32844: Fix wrong redirection of a low descriptor (0 or 1) to stderr in subprocess if another low descriptor is closed.
- bpo-31908: Fix output of cover files for `trace` module command-line
tool. Previously emitted cover files only when`--missing` option was
used. Patch by Michael Selik.
- bpo-31457: If nested log adapters are used, the inner `process()` methods are no longer omitted.
- bpo-16865: Support arrays >=2GiB in `ctypes` .  Patch by Segev Finer.
- bpo-31238: pydoc: the stop() method of the private ServerThread class now waits until DocServer.serve_until_quit() completes and then explicitly sets its docserver attribute to None to break a reference cycle.

### DocumentationÂ¶

- bpo-33503: Fix broken pypi link
- bpo-33421: Add missing documentation for `typing.AsyncContextManager` .
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
- bpo-8243: Add a note about curses.addch and curses.addstr exception behavior when writing outside a window, or pad.
- bpo-31432: Clarify meaning of CERT_NONE, CERT_OPTIONAL, and CERT_REQUIRED flags for ssl.SSLContext.verify_mode.

### TestsÂ¶

### BuildÂ¶

- bpo-5755: Move `-Wstrict-prototypes` option to`CFLAGS_NODIST` from`OPT` . This option emitted annoying warnings when building extension
modules written in C++.
- bpo-33614: Ensures module definition files for the stable ABI on Windows are correctly regenerated.
- bpo-33522: Enable CI builds on Visual Studio Team Services at https://python.visualstudio.com/cpython
- bpo-33012: Add `-Wno-cast-function-type` for gcc 8 for silencing
warnings about function casts like casting to PyCFunction in method
definition lists.
- bpo-33394: Enable the verbose build for extension modules, when GNU make is passed macros on the command line.

### WindowsÂ¶

- bpo-33184: Update Windows installer to OpenSSL 1.0.2o.

### macOSÂ¶

- bpo-33184: Update macOS installer build to use OpenSSL 1.0.2o.

### IDLEÂ¶

- bpo-33656: On Windows, add API call saying that tk scales for DPI. On Windows 8.1+ or 10, with DPI compatibility properties of the Python binary unchanged, and a monitor resolution greater than 96 DPI, this should make text and lines sharper. It should otherwise have no effect.
- bpo-33768: Clicking on a context line moves that line to the top of the editor window.
- bpo-33763: IDLE: Use read-only text widget for code context instead of label widget.
- bpo-33664: Scroll IDLE editor text by lines. Previously, the mouse wheel and scrollbar slider moved text by a fixed number of pixels, resulting in partial lines at the top of the editor box. The change also applies to the shell and grep output windows, but not to read-only text views.
- bpo-33679: Enable theme-specific color configuration for Code Context. Use the Highlights tab to see the setting for built-in themes or add settings to custom themes.
- bpo-33642: Display up to maxlines non-blank lines for Code Context. If there is no current context, show a single blank line.
- bpo-33628: IDLE: Cleanup codecontext.py and its test.
- bpo-33564: IDLEâs code context now recognizes async as a block opener.
- bpo-29706: IDLE now colors async and await as keywords in 3.6. They become full keywords in 3.7.
- bpo-21474: Update word/identifier definition from ascii to unicode. In text and entry boxes, this affects selection by double-click, movement left/right by control-left/right, and deletion left/right by control-BACKSPACE/DEL.
- bpo-33204: IDLE: consistently color invalid string prefixes. A âuâ string prefix cannot be paired with either ârâ or âfâ. Consistently color as much of the prefix, starting at the right, as is valid. Revise and extend colorizer test.
- bpo-32831: Add docstrings and tests for codecontext.

### Tools/DemosÂ¶

- bpo-33189: **pygettext.py** now recognizes only literal strings as
docstrings and translatable strings, and rejects bytes literals and
f-string expressions.
- bpo-31920: Fixed handling directories as arguments in the `pygettext` script. Based on patch by Oleg Krasnikov.
- bpo-29673: Fix pystackv and pystack gdbinit macros.
- bpo-32885: Add an `-n` flag for`Tools/scripts/pathfix.py` to disable
automatic backup creation (files with`~` suffix).
- bpo-31583: Fix 2to3 for using with âadd-suffix option but without âoutput-dir option for relative path to files in current directory.

### C APIÂ¶

- bpo-32374: Document that m_traverse for multi-phase initialized modules can be called with m_state=NULL, and add a sanity check

## Python 3.6.5 finalÂ¶

*Release date: 2018-03-28*

### TestsÂ¶

- bpo-32872: Avoid regrtest compatibility issue with namespace packages.

### BuildÂ¶

- bpo-33163: Upgrade pip to 9.0.3 and setuptools to v39.0.1.

## Python 3.6.5 release candidate 1Â¶

*Release date: 2018-03-13*

### SecurityÂ¶

- bpo-33001: Minimal fix to prevent buffer overrun in os.symlink on Windows
- bpo-32981: Regexes in difflib and poplib were vulnerable to catastrophic backtracking. These regexes formed potential DOS vectors (REDOS). They have been refactored. This resolves **CVE 2018-1060** and**CVE 2018-1061** .
Patch by Jamie Davis.

### Core and BuiltinsÂ¶

- bpo-33026: Fixed jumping out of âwithâ block by setting f_lineno.
- bpo-17288: Prevent jumps from âreturnâ and âexceptionâ trace events.
- bpo-32889: Update Valgrind suppression list to account for the rename of `Py_ADDRESS_IN_RANG` to`address_in_range` .
- bpo-32650: Pdb and other debuggers dependent on bdb.py will correctly step over (next command) native coroutines. Patch by Pablo Galindo.
- bpo-32685: Improve suggestion when the Python 2 form of print statement is either present on the same line as the header of a compound statement or else terminated by a semi-colon instead of a newline. Patch by Nitish Chandra.
- bpo-32583: Fix possible crashing in builtin Unicode decoders caused by write out-of-bound errors when using customized decode error handlers.
- bpo-26163: Improved frozenset() hash to create more distinct hash values when faced with datasets containing many similar values.
- bpo-27169: The `__debug__` constant is now optimized out at compile
time. This fixes also bpo-22091.
- bpo-32329: `sys.flags.hash_randomization` is now properly set to 0 when
hash randomization is turned off by`PYTHONHASHSEED=0` .
- bpo-30416: The optimizer is now protected from spending much time doing complex calculations and consuming much memory for creating large constants in constant folding.
- bpo-18533: `repr()` on a dict containing its own`values()` or`items()` no longer raises`RecursionError` ; OrderedDict similarly.
Instead, use`...` , as for other recursive structures.  Patch by Ben
North.
- bpo-32028: Leading whitespace is now correctly ignored when generating suggestions for converting Py2 print statements to Py3 builtin print function calls. Patch by Sanyam Khurana.
- bpo-32137: The repr of deeply nested dict now raises a RecursionError instead of crashing due to a stack overflow.

### LibraryÂ¶

- bpo-33064: lib2to3 now properly supports trailing commas after `*args` and`**kwargs` in function signatures.
- bpo-31804: Avoid failing in multiprocessing.Process if the standard streams are closed or None at exit.
- bpo-33037: Skip sending/receiving data after SSL transport closing.
- bpo-30353: Fix ctypes pass-by-value for structs on 64-bit Cygwin/MinGW.
- bpo-33009: Fix inspect.signature() for single-parameter partialmethods.
- bpo-32969: Expose several missing constants in zlib and fix corresponding documentation.
- bpo-32713: Fixed tarfile.itn handling of out-of-bounds float values. Patch by Joffrey Fuhrer.
- bpo-30622: The ssl module now detects missing NPN support in LibreSSL.
- bpo-32922: dbm.open() now encodes filename with the filesystem encoding rather than default encoding.
- bpo-32859: In `os.dup2` , donât check every call whether the`dup3` syscall exists or not.
- bpo-21060: Rewrite confusing message from setup.py upload from âNo dist file created in earlier commandâ to the more helpful âMust create and upload files in one commandâ.
- bpo-32857: In `tkinter` ,`after_cancel(None)` now raises a`ValueError` instead of canceling the first scheduled function.
Patch by Cheryl Sabella.
- bpo-32852: Make sure sys.argv remains as a list when running trace.
- bpo-32841: Fixed `asyncio.Condition` issue which silently ignored
cancellation after notifying and cancelling a conditional lock. Patch by
Bar Harel.
- bpo-31787: Fixed refleaks of `__init__()` methods in various modules.
(Contributed by Oren Milman)
- bpo-30157: Fixed guessing quote and delimiter in csv.Sniffer.sniff() when only the last field is quoted. Patch by Jake Davis.
- bpo-32394: socket: Remove TCP_FASTOPEN, TCP_KEEPCNT flags on older version Windows during run-time.
- bpo-32777: Fix a rare but potential pre-exec child process deadlock in subprocess on POSIX systems when marking file descriptors inheritable on exec in the child process. This bug appears to have been introduced in 3.4.
- bpo-32647: The ctypes module used to depend on indirect linking for dlopen. The shared extension is now explicitly linked against libdl on platforms with dl.
- bpo-32734: Fixed `asyncio.Lock()` safety issue which allowed acquiring
and locking the same lock multiple times, without it being free. Patch by
Bar Harel.
- bpo-32727: Do not include name field in SMTP envelope from address. Patch by StÃ©phane Wirtel
- bpo-27931: Fix email address header parsing error when the username is an empty quoted string. Patch by Xiang Zhang.
- bpo-32304: distutilsâ upload command no longer corrupts tar files ending with a CR byte, and no longer tries to convert CR to CRLF in any of the upload text fields.
- bpo-32502: uuid.uuid1 no longer raises an exception if a 64-bit hardware address is encountered.
- bpo-31848: Fix the error handling in Aifc_read.initfp() when the SSND chunk is not found. Patch by Zackery Spytz.
- bpo-32555: On FreeBSD and Solaris, os.strerror() now always decode the byte string from the current locale encoding, rather than using ASCII/surrogateescape in some cases.
- bpo-32521: The nis module is now compatible with new libnsl and headers location.
- bpo-32473: Improve ABCMeta._dump_registry() output readability
- bpo-32521: glibc has removed Sun RPC. Use replacement libtirpc headers and library in nis module.
- bpo-32228: Ensure that `truncate()` preserves the file position (as
reported by`tell()` ) after writes longer than the buffer size.
- bpo-26133: Donât unsubscribe signals in asyncio UNIX event loop on interpreter shutdown.
- bpo-32185: The SSL module no longer sends IP addresses in SNI TLS extension on platforms with OpenSSL 1.0.2+ or inet_pton.
- bpo-32323: `urllib.parse.urlsplit()` does not convert zone-id (scope)
to lower case for scoped IPv6 addresses in hostnames now.
- bpo-32302: Fix bdist_wininst of distutils for CRT v142: it binary compatible with CRT v140.
- bpo-32255: A single empty field is now always quoted when written into a CSV file. This allows to distinguish an empty row from a row consisting of a single empty field. Patch by Licht Takeuchi.
- bpo-32277: Raise `NotImplementedError` instead of`SystemError` on
platforms where`chmod(..., follow_symlinks=False)` is not supported.
Patch by Anthony Sottile.
- bpo-32199: The getnode() ip getter now uses âip linkâ instead of âip link listâ.
- bpo-27456: Ensure TCP_NODELAY is set on Linux. Tests by Victor Stinner.
- bpo-31900: The `locale.localeconv()` function now sets temporarily the`LC_CTYPE` locale to the`LC_NUMERIC` locale to decode`decimal_point` and`thousands_sep` byte strings if they are non-ASCII
or longer than 1 byte, and the`LC_NUMERIC` locale is different than the`LC_CTYPE` locale. This temporary change affects other threads. Same
change for the`str.format()` method when formatting a number
(`int` ,`float` ,`float` and subclasses) with the`n` type (ex:`'{:n}'.format(1234)` ).
- bpo-31802: Importing native path module ( `posixpath` ,`ntpath` ) now
works even if the`os` module still is not imported.

### DocumentationÂ¶

- bpo-17232: Clarify docs for -O and -OO. Patch by Terry Reedy.
- bpo-32800: Update link to w3c doc for xml default namespaces.
- bpo-8722: Document `__getattr__()` behavior when property`get()` method raises`AttributeError` .
- bpo-32614: Modify RE examples in documentation to use raw strings to prevent `DeprecationWarning` and add text to REGEX HOWTO to highlight
the deprecation.
- bpo-31972: Improve docstrings for `pathlib.PurePath` subclasses.
- bpo-17799: Explain real behaviour of sys.settrace and sys.setprofile and their C-API counterparts regarding which type of events are received in each function. Patch by Pablo Galindo Salgado.

### TestsÂ¶

- bpo-32517: Fix failing `test_asyncio` on macOS 10.12.2+ due to transport
of`KqueueSelector` loop was not being closed.
- bpo-32721: Fix test_hashlib to not fail if the _md5 module is not built.
- bpo-32252: Fix faulthandler_suppress_crash_report() used to prevent core dump files when testing crashes. getrlimit() returns zero on success.
- bpo-31518: Debian Unstable has disabled TLS 1.0 and 1.1 for SSLv23_METHOD(). Change TLS/SSL protocol of some tests to PROTOCOL_TLS or PROTOCOL_TLSv1_2 to make them pass on Debian.

### BuildÂ¶

- bpo-32635: Fix segfault of the crypt module when libxcrypt is provided instead of libcrypt at the system.

### WindowsÂ¶

- bpo-33016: Fix potential use of uninitialized memory in nt._getfinalpathname
- bpo-32903: Fix a memory leak in os.chdir() on Windows if the current directory is set to a UNC path.
- bpo-31966: Fixed WindowsConsoleIO.write() for writing empty data.
- bpo-32409: Ensures activate.bat can handle Unicode contents.
- bpo-32457: Improves handling of denormalized executable path when launching Python.
- bpo-32370: Use the correct encoding for ipconfig output in the uuid module. Patch by Segev Finer.
- bpo-29248: Fix `os.readlink()` on Windows, which was mistakenly
treating the`PrintNameOffset` field of the reparse data buffer as a
number of characters instead of bytes. Patch by Craig Holmquist and SSE4.
- bpo-32588: Create standalone _distutils_findvs module.

### macOSÂ¶

- bpo-32726: Provide an additional, more modern macOS installer variant that supports macOS 10.9+ systems in 64-bit mode only. Upgrade the supplied third-party libraries to OpenSSL 1.0.2n, XZ 5.2.3, and SQLite 3.22.0. The 10.9+ installer now links with and supplies its own copy of Tcl/Tk 8.6.8.

### IDLEÂ¶

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
- bpo-32765: Update configdialog General tab docstring to add new widgets to the widget list.

### Tools/DemosÂ¶

- bpo-24960: 2to3 and lib2to3 can now read pickled grammar files using pkgutil.get_data() rather than probing the filesystem. This lets 2to3 and lib2to3 work when run from a zipfile.
- bpo-32222: Fix pygettext not extracting docstrings for functions with type annotated arguments. Patch by Toby Harradine.

### C APIÂ¶

- bpo-29084: Undocumented C API for OrderedDict has been excluded from the limited C API. It was added by mistake and actually never worked in the limited C API.

## Python 3.6.4 finalÂ¶

*Release date: 2017-12-18*

There were no new code changes in version 3.6.4 since v3.6.4rc1.

## Python 3.6.4 release candidate 1Â¶

*Release date: 2017-12-05*

### Core and BuiltinsÂ¶

- bpo-32176: co_flags.CO_NOFREE is now always set correctly by the code object constructor based on freevars and cellvars, rather than needing to be set correctly by the caller. This ensures it will be cleared automatically when additional cell references are injected into a modified code object and function.
- bpo-31949: Fixed several issues in printing tracebacks (PyTraceBack_Print()). Setting sys.tracebacklimit to 0 or less now suppresses printing tracebacks. Setting sys.tracebacklimit to None now causes using the default limit. Setting sys.tracebacklimit to an integer larger than LONG_MAX now means using the limit LONG_MAX rather than the default limit. Fixed integer overflows in the case of more than `2**31` traceback items on Windows. Fixed output errors handling.
- bpo-30696: Fix the interactive interpreter looping endlessly when no memory.
- bpo-20047: Bytearray methods partition() and rpartition() now accept only bytes-like objects as separator, as documented. In particular they now raise TypeError rather of returning a bogus result when an integer is passed as a separator.
- bpo-31852: Fix a segmentation fault caused by a combination of the async soft keyword and continuation lines.
- bpo-21720: BytesWarning no longer emitted when the *fromlist* argument of`__import__()` or the`__all__` attribute of the module contain bytes
instances.
- bpo-31825: Fixed OverflowError in the âunicode-escapeâ codec and in codecs.escape_decode() when decode an escaped non-ascii byte.
- bpo-28603: Print the full context/cause chain of exceptions on interpreter exit, even if an exception in the chain is unhashable or compares equal to later ones. Patch by Zane Bitter.
- bpo-31786: Fix timeout rounding in the select module to round correctly negative timeouts between -1.0 and 0.0. The functions now block waiting for events as expected. Previously, the call was incorrectly non-blocking. Patch by Pablo Galindo.
- bpo-31642: Restored blocking âfrom package import moduleâ by setting sys.modules[âpackage.moduleâ] to None.
- bpo-31626: Fixed a bug in debug memory allocator. There was a write to freed memory after shrinking a memory block.
- bpo-31619: Fixed a ValueError when convert a string with large number of underscores to integer with binary base.
- bpo-31592: Fixed an assertion failure in Python parser in case of a bad `unicodedata.normalize()` . Patch by Oren Milman.
- bpo-31588: Raise a `TypeError` with a helpful error message when class
creation fails due to a metaclass with a bad`__prepare__()` method.
Patch by Oren Milman.
- bpo-31566: Fix an assertion failure in `_warnings.warn()` in case of a
bad`__name__` global. Patch by Oren Milman.
- bpo-31505: Fix an assertion failure in `json` , in case`_json.make_encoder()` received a bad`encoder()` argument. Patch by
Oren Milman.
- bpo-31492: Fix assertion failures in case of failing to import from a module with a bad `__name__` attribute, and in case of failing to access
an attribute of such a module. Patch by Oren Milman.
- bpo-31490: Fix an assertion failure in `ctypes` class definition, in
case the class has an attribute whose name is specified in`_anonymous_` but not in`_fields_` . Patch by Oren Milman.
- bpo-31478: Fix an assertion failure in `_random.Random.seed()` in case
the argument has a bad`__abs__()` method. Patch by Oren Milman.
- bpo-31315: Fix an assertion failure in imp.create_dynamic(), when spec.name is not a string. Patch by Oren Milman.
- bpo-31311: Fix a crash in the `__setstate__()` method of`ctypes._CData` , in case of a bad`__dict__` . Patch by Oren Milman.
- bpo-31293: Fix crashes in true division and multiplication of a timedelta object by a float with a bad as_integer_ratio() method. Patch by Oren Milman.
- bpo-31285: Fix an assertion failure in `warnings.warn_explicit` , when
the return value of the received loaderâs`get_source()` has a bad`splitlines()` method. Patch by Oren Milman.
- bpo-30817: `PyErr_PrintEx()` clears now the ignored exception that may
be raised by`_PySys_SetObjectId()` , for example when no memory.

### LibraryÂ¶

- bpo-28556: Two minor fixes for `typing` module: allow shallow copying
instances of generic classes, improve interaction of`__init_subclass__` with generics. Original PRs by Ivan Levkivskyi.
- bpo-27240: The header folding algorithm for the new email policies has been rewritten, which also fixes bpo-30788, bpo-31831, and bpo-32182. In particular, RFC2231 folding is now done correctly.
- bpo-32186: io.FileIO.readall() and io.FileIO.read() now release the GIL when getting the file size. Fixed hang of all threads with inaccessible NFS server. Patch by Nir Soffer.
- bpo-12239: Make `msilib.SummaryInformation.GetProperty()` return`None` when the value of property is`VT_EMPTY` .  Initial patch by
Mark Mc Mahon.
- bpo-31325: Fix wrong usage of `collections.namedtuple()` in the`RobotFileParser.parse()` method. Initial patch by Robin Wellner.
- bpo-12382: `msilib.OpenDatabase()` now raises a better exception
message when it couldnât open or create an MSI file.  Initial patch by
William TisÃ¤ter.
- bpo-32110: `codecs.StreamReader.read(n)` now returns not more than*n* characters/bytes for non-negative*n* . This makes it compatible with`read()` methods of other file-like objects.
- bpo-32072: Fixed issues with binary plists: Fixed saving bytearrays. Identical objects will be saved only once. Equal references will be load as identical objects. Added support for saving and loading recursive data structures.
- bpo-32034: Make asyncio.IncompleteReadError and LimitOverrunError pickleable.
- bpo-32015: Fixed the looping of asyncio in the case of reconnection the socket during waiting async read/write from/to the socket.
- bpo-32011: Restored support of loading marshal files with the TYPE_INT64 code. These files can be produced in Python 2.7.
- bpo-31970: Reduce performance overhead of asyncio debug mode.
- bpo-9678: Fixed determining the MAC address in the uuid module: Using ifconfig on NetBSD and OpenBSD. Using arp on Linux, FreeBSD, NetBSD and OpenBSD. Based on patch by Takayuki Shimizukawa.
- bpo-30057: Fix potential missed signal in signal.signal().
- bpo-31933: Fix Blake2 params leaf_size and node_offset on big endian platforms. Patch by Jack OâConnor.
- bpo-31927: Fixed compilation of the socket module on NetBSD 8. Fixed assertion failure or reading arbitrary data when parse a AF_BLUETOOTH address on NetBSD and DragonFly BSD.
- bpo-27666: Fixed stack corruption in curses.box() and curses.ungetmouse() when the size of types chtype or mmask_t is less than the size of C long. curses.box() now accepts characters as arguments. Based on patch by Steve Fink.
- bpo-31897: plistlib now catches more errors when read binary plists and raises InvalidFileException instead of unexpected exceptions.
- bpo-25720: Fix the method for checking pad state of curses WINDOW. Patch by Masayuki Yamamoto.
- bpo-31893: Fixed the layout of the kqueue_event structure on OpenBSD and NetBSD. Fixed the comparison of the kqueue_event objects.
- bpo-31891: Fixed building the curses module on NetBSD.
- bpo-28416: Instances of pickle.Pickler subclass with the persistent_id() method and pickle.Unpickler subclass with the persistent_load() method no longer create reference cycles.
- bpo-28326: Fix multiprocessing.Process when stdout and/or stderr is closed or None.
- bpo-31457: If nested log adapters are used, the inner `process()` methods are no longer omitted.
- bpo-31457: The `manager` property on LoggerAdapter objects is now
properly settable.
- bpo-31806: Fix timeout rounding in time.sleep(), threading.Lock.acquire() and socket.socket.settimeout() to round correctly negative timeouts between -1.0 and 0.0. The functions now block waiting for events as expected. Previously, the call was incorrectly non-blocking. Patch by Pablo Galindo.
- bpo-28603: traceback: Fix a TypeError that occurred during printing of exception tracebacks when either the current exception or an exception in its context/cause chain is unhashable. Patch by Zane Bitter.
- bpo-30058: Fixed buffer overflow in select.kqueue.control().
- bpo-31770: Prevent a crash when calling the `__init__()` method of a`sqlite3.Cursor` object more than once. Patch by Oren Milman.
- bpo-31672: `idpattern` in`string.Template` matched some non-ASCII
characters. Now it uses`-i` regular expression local flag to avoid
non-ASCII characters.
- bpo-31764: Prevent a crash in `sqlite3.Cursor.close()` in case the`Cursor` object is uninitialized. Patch by Oren Milman.
- bpo-31752: Fix possible crash in timedelta constructor called with custom integers.
- bpo-31701: On Windows, faulthandler.enable() now ignores MSC and COM exceptions.
- bpo-31728: Prevent crashes in `_elementtree` due to unsafe cleanup of`Element.text` and`Element.tail` . Patch by Oren Milman.
- bpo-31620: an empty asyncio.Queue now doesnât leak memory when queue.get pollers timeout
- bpo-31632: Fix method set_protocol() of class _SSLProtocolTransport in asyncio module. This method was previously modifying a wrong reference to the protocol.
- bpo-31675: Fixed memory leaks in Tkinterâs methods splitlist() and split() when pass a string larger than 2 GiB.
- bpo-31673: Fixed typo in the name of Tkinterâs method adderrorinfo().
- bpo-30806: Fix the string representation of a netrc object.
- bpo-15037: Added a workaround for getkey() in curses for ncurses 5.7 and earlier.
- bpo-25351: Avoid venv activate failures with undefined variables
- bpo-25532: inspect.unwrap() will now only try to unwrap an object sys.getrecursionlimit() times, to protect against objects which create a new object on every attribute access.
- bpo-30347: Stop crashes when concurrently iterate over itertools.groupby() iterators.
- bpo-31516: `threading.current_thread()` should not return a dummy thread
at shutdown.
- bpo-31351: python -m ensurepip now exits with non-zero exit code if pip bootstrapping has failed.
- bpo-31482: `random.seed()` now works with bytes in version=1
- bpo-31334: Fix `poll.poll([timeout])` in the`select` module for
arbitrary negative timeouts on all OSes where it can only be a
non-negative integer or -1. Patch by Riccardo Coccioli.
- bpo-31310: multiprocessingâs semaphore tracker should be launched again if crashed.
- bpo-31308: Make multiprocessingâs forkserver process immune to Ctrl-C and other user interruptions. If it crashes, restart it when necessary.

### DocumentationÂ¶

- bpo-32105: Added asyncio.BaseEventLoop.connect_accepted_socket versionadded marker.
- bpo-31537: Fix incorrect usage of `get_history_length` in readline
documentation example code. Patch by Brad Smith.
- bpo-30085: The operator functions without double underscores are preferred for clarity. The one with underscores are only kept for back-compatibility.

### TestsÂ¶

- bpo-31380: Skip test_httpservers test_undecodable_file on macOS: fails on APFS.
- bpo-31705: Skip test_socket.test_sha256() on Linux kernel older than 4.5. The test fails with ENOKEY on kernel 3.10 (on ppc64le). A fix was merged into the kernel 4.5.
- bpo-31174: Fix test_tools.test_unparse: DirectoryTestCase now stores the names sample to always test the same files. It prevents false alarms when hunting reference leaks.
- bpo-30695: Add the `set_nomemory(start, stop)` and`remove_mem_hooks()` functions to the`_testcapi` module.

### BuildÂ¶

- bpo-32059: `detect_modules()` in`setup.py` now also searches the
sysroot paths when cross-compiling.
- bpo-31957: Fixes Windows SDK version detection when building for Windows.
- bpo-31609: Fixes quotes in PCbuild/clean.bat
- bpo-31934: Abort the build when building out of a not clean source tree.
- bpo-31926: Fixed Argument Clinic sometimes causing compilation errors when there was more than one function and/or method in a .c file with the same name.
- bpo-28791: Update Windows builds to use SQLite 3.21.0.
- bpo-28791: Update OS X installer to use SQLite 3.21.0.
- bpo-22140: Prevent double substitution of prefix in python-config.sh.
- bpo-31536: Avoid wholesale rebuild after `make regen-all` if nothing
changed.

### WindowsÂ¶

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
- bpo-31460: Simplify the API of IDLEâs Module Browser. Passing a widget instead of an flist with a root widget opens the option of creating a browser frame that is only part of a window. Passing a full file name instead of pieces assumed to come from a .py file opens the possibility of browsing python files that do not end in .py.
- bpo-31649: IDLE - Make _htest, _utest parameters keyword only.
- bpo-31559: Remove test order dependence in idle_test.test_browser.
- bpo-31459: Rename IDLEâs module browser from Class Browser to Module Browser. The original module-level class and method browser became a module browser, with the addition of module-level functions, years ago. Nested classes and functions were added yesterday. For back-compatibility, the virtual event <<open-class-browser>>, which appears on the Keys tab of the Settings dialog, is not changed. Patch by Cheryl Sabella.
- bpo-31500: Default fonts now are scaled on HiDPI displays.
- bpo-1612262: IDLE module browser now shows nested classes and functions. Original patches for code and tests by Guilherme Polo and Cheryl Sabella, respectively.

### Tools/DemosÂ¶

- bpo-30722: Make redemo work with Python 3.6 and newer versions. Also, remove the `LOCALE` option since it doesnât work with string patterns in
Python 3. Patch by Christoph Sarnowski.

### C APIÂ¶

- bpo-20891: Fix PyGILState_Ensure(). When PyGILState_Ensure() is called in a non-Python thread before PyEval_InitThreads(), only call PyEval_InitThreads() after calling PyThreadState_New() to fix a crash.
- bpo-31532: Fix memory corruption due to allocator mix in getpath.c between Py_GetPath() and Py_SetPath()
- bpo-30697: The `PyExc_RecursionErrorInst` singleton is removed and`PyErr_NormalizeException()` does not use it anymore. This singleton is
persistent and its members being never cleared may cause a segfault during
finalization of the interpreter. See also bpo-22898.

## Python 3.6.3 finalÂ¶

*Release date: 2017-10-03*

### LibraryÂ¶

- bpo-31641: Re-allow arbitrary iterables in `concurrent.futures.as_completed()` . Fixes regression in 3.6.3rc1.

### BuildÂ¶

## Python 3.6.3 release candidate 1Â¶

*Release date: 2017-09-18*

### SecurityÂ¶

### Core and BuiltinsÂ¶

- bpo-31471: Fix an assertion failure in `subprocess.Popen()` on Windows,
in case the env argument has a bad`keys()` method. Patch by Oren
Milman.
- bpo-31418: Fix an assertion failure in `PyErr_WriteUnraisable()` in case
of an exception with a bad`__module__` attribute. Patch by Oren Milman.
- bpo-31416: Fix assertion failures in case of a bad warnings.filters or warnings.defaultaction. Patch by Oren Milman.
- bpo-31411: Raise a TypeError instead of SystemError in case warnings.onceregistry is not a dictionary. Patch by Oren Milman.
- bpo-31373: Fix several possible instances of undefined behavior due to floating-point demotions.
- bpo-30465: Location information ( `lineno` and`col_offset` ) in
f-strings is now (mostly) correct.  This fixes tools like flake8 from
showing warnings on the wrong line (typically the first line of the file).
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
- bpo-30597: `print` now shows expected input in custom error message when
used as a Python 2 statement. Patch by Sanyam Khurana.

### LibraryÂ¶

- bpo-31499: xml.etree: Fix a crash when a parser is part of a reference cycle.
- bpo-28556: typing.get_type_hints now finds the right globalns for classes and modules by default (when no `globalns` was specified by the caller).
- bpo-28556: Speed improvements to the `typing` module.  Original PRs by
Ivan Levkivskyi and Mitar.
- bpo-31544: The C accelerator module of ElementTree ignored exceptions raised when looking up TreeBuilder target methods in XMLParser().
- bpo-31234: socket.create_connection() now fixes manually a reference cycle: clear the variable storing the last exception on success.
- bpo-31457: LoggerAdapter objects can now be nested.
- bpo-31400: Improves SSL error handling to avoid losing error numbers.
- bpo-28958: ssl.SSLContext() now uses OpenSSL error information when a context cannot be instantiated.
- bpo-27340: SSLSocket.sendall() now uses memoryview to create slices of data. This fixes support for all bytes-like object. It is also more efficient and avoids costly copies.
- bpo-31178: Fix string concatenation bug in rare error path in the subprocess module
- bpo-31350: Micro-optimize `asyncio._get_running_loop()` to become up
to 10% faster.
- bpo-31170: expat: Update libexpat from 2.2.3 to 2.2.4. Fix copying of partial characters for UTF-8 input (libexpat bug 115): https://github.com/libexpat/libexpat/issues/115
- bpo-29136: Add TLS 1.3 cipher suites and OP_NO_TLSv1_3.
- bpo-29212: Fix concurrent.futures.thread.ThreadPoolExecutor threads to have a non repr() based thread name by default when no thread_name_prefix is supplied. They will now identify themselves as âThreadPoolExecutor-y_nâ.
- bpo-9146: Fix a segmentation fault in _hashopenssl when standard hash functions such as md5 are not available in the linked OpenSSL library. As in some special FIPS-140 build environments.
- bpo-27144: The `map()` and`as_completed()` iterators in`concurrent.futures` now avoid keeping a reference to yielded objects.
- bpo-10746: Fix ctypes producing wrong **PEP 3118** type codes for integer
types.
- bpo-22536: The subprocess module now sets the filename when FileNotFoundError is raised on POSIX systems due to the executable or cwd not being found.
- bpo-31249: concurrent.futures: WorkItem.run() used by ThreadPoolExecutor now breaks a reference cycle between an exception object and the WorkItem object.
- bpo-31247: xmlrpc.server now explicitly breaks reference cycles when using sys.exc_info() in code handling exceptions.
- bpo-30102: The ssl and hashlib modules now call OPENSSL_add_all_algorithms_noconf() on OpenSSL < 1.1.0. The function detects CPU features and enables optimizations on some CPU architectures such as POWER8. Patch is based on research from Gustavo Serra Scalet.
- bpo-31185: Fixed miscellaneous errors in asyncio speedup module.
- bpo-31135: ttk: fix the destroy() method of LabeledScale and OptionMenu classes. Call the parent destroy() method even if the used attribute doesnât exist. The LabeledScale.destroy() method now also explicitly clears label and scale attributes to help the garbage collector to destroy all widgets.
- bpo-31107: Fix `copyreg._slotnames()` mangled attribute calculation for
classes whose name begins with an underscore. Patch by Shane Harvey.
- bpo-31061: Fixed a crash when using asyncio and threads.
- bpo-30502: Fix handling of long oids in ssl. Based on patch by Christian Heimes.
- bpo-30119: ftplib.FTP.putline() now throws ValueError on commands that contains CR or LF. Patch by Donghee Na.
- bpo-30595: multiprocessing.Queue.get() with a timeout now polls its reader in non-blocking mode if it succeeded to acquire the lock but the acquire took longer than the timeout.
- bpo-29403: Fix `unittest.mock` âs autospec to not fail on method-bound
builtin functions.  Patch by Aaron Gallagher.
- bpo-30961: Fix decrementing a borrowed reference in tracemalloc.
- bpo-25684: Change `ttk.OptionMenu` radiobuttons to be unique across
instances of`OptionMenu` .
- bpo-30886: Fix multiprocessing.Queue.join_thread(): it now waits until the thread completes, even if the thread was started by the same process which created the queue.
- bpo-29854: Fix segfault in readline when using readlineâs history-size option. Patch by Nir Soffer.
- bpo-30319: socket.close() now ignores ECONNRESET error.
- bpo-30828: Fix out of bounds write in `asyncio.CFuture.remove_done_callback()` .
- bpo-30807: signal.setitimer() may disable the timer when passed a tiny value. Tiny values (such as 1e-6) are valid non-zero values for setitimer(), which is specified as taking microsecond-resolution intervals. However, on some platform, our conversion routine could convert 1e-6 into a zero interval, therefore disabling the timer instead of (re-)scheduling it.
- bpo-30441: Fix bug when modifying os.environ while iterating over it
- bpo-30532: Fix email header value parser dropping folding white space in certain cases.
- bpo-30879: os.listdir() and os.scandir() now emit bytes names when called with bytes-like argument.
- bpo-30746: Prohibited the â=â character in environment variable names in `os.putenv()` and`os.spawn*()` .
- bpo-29755: Fixed the lgettext() family of functions in the gettext module. They now always return bytes.

### DocumentationÂ¶

### TestsÂ¶

- bpo-31320: Silence traceback in test_ssl
- bpo-25674: Remove sha256.tbs-internet.com ssl test
- bpo-30715: Address ALPN callback changes for OpenSSL 1.1.0f. The latest version behaves like OpenSSL 1.0.2 and no longer aborts handshake.
- bpo-30822: regrtest: Exclude tzdata from regrtest âall. When running the test suite using âuse=all / -u all, exclude tzdata since it makes test_datetime too slow (15-20 min on some buildbots) which then times out on some buildbots. Fix also regrtest command line parser to allow passing -u extralargefile to run test_zipfile64.

### BuildÂ¶

- bpo-30854: Fix compile error when compiling âwithout-threads. Patch by Masayuki Yamamoto.

### WindowsÂ¶

- bpo-30389: Adds detection of Visual Studio 2017 to distutils on Windows.
- bpo-31340: Change to building with MSVC v141 (included with Visual Studio 2017)
- bpo-30581: os.cpu_count() now returns the correct number of processors on Windows when the number of logical processors is greater than 64.
- bpo-30731: Add a missing xmlns to python.manifest so that it matches the schema.

### IDLEÂ¶

- bpo-31493: IDLE code context â fix code update and font update timers. Canceling timers prevents a warning message when test_idle completes.
- bpo-31488: IDLE - Update non-key options in former extension classes. When applying configdialog changes, call .reload for each feature class. Change ParenMatch so updated options affect existing instances attached to existing editor windows.
- bpo-31477: IDLE - Improve rstrip entry in doc. Strip trailing whitespace strips more than blank spaces. Multiline string literals are not skipped.
- bpo-31480: IDLE - make tests pass with zzdummy extension disabled by default.
- bpo-31421: Document how IDLE runs tkinter programs. IDLE calls tcl/tk update in the background in order to make live interaction and experimentation with tkinter applications much easier.
- bpo-31414: IDLE â fix tk entry box tests by deleting first. Adding to an int entry is not the same as deleting and inserting because int(ââ) will fail.
- bpo-31051: Rearrange IDLE configdialog GenPage into Window, Editor, and Help sections.
- bpo-30617: IDLE - Add docstrings and tests for outwin subclass of editor. Move some data and functions from the class to module level. Patch by Cheryl Sabella.
- bpo-31287: IDLE - Do not modify tkinter.message in test_configdialog.
- bpo-27099: Convert IDLEâs built-in âextensionsâ to regular features. About 10 IDLE features were implemented as supposedly optional extensions. Their different behavior could be confusing or worse for users and not good for maintenance. Hence the conversion. The main difference for users is that user configurable key bindings for builtin features are now handled uniformly. Now, editing a binding in a keyset only affects its value in the keyset. All bindings are defined together in the system-specific default keysets in config-extensions.def. All custom keysets are saved as a whole in config-extension.cfg. All take effect as soon as one clicks Apply or Ok. The affected events are â<<force-open-completions>>â, â<<expand-word>>â, â<<force-open-calltip>>â, â<<flash-paren>>â, â<<format-paragraph>>â, â<<run-module>>â, â<<check-module>>â, and â<<zoom-height>>â. Any (global) customizations made before 3.6.3 will not affect their keyset-specific customization after 3.6.3. and vice versa. Initial patch by Charles Wohlganger.
- bpo-31206: IDLE: Factor HighPage(Frame) class from ConfigDialog. Patch by Cheryl Sabella.
- bpo-31001: Add tests for configdialog highlight tab. Patch by Cheryl Sabella.
- bpo-31205: IDLE: Factor KeysPage(Frame) class from ConfigDialog. The slightly modified tests continue to pass. Patch by Cheryl Sabella.
- bpo-31130: IDLE â stop leaks in test_configdialog. Initial patch by Victor Stinner.
- bpo-31002: Add tests for configdialog keys tab. Patch by Cheryl Sabella.
- bpo-19903: IDLE: Calltips use `inspect.signature` instead of`inspect.getfullargspec` . This improves calltips for builtins converted
to use Argument Clinic. Patch by Louie Lu.
- bpo-31083: IDLE - Add an outline of a TabPage class in configdialog. Update existing classes to match outline. Initial patch by Cheryl Sabella.
- bpo-31050: Factor GenPage(Frame) class from ConfigDialog. The slightly modified tests continue to pass. Patch by Cheryl Sabella.
- bpo-31004: IDLE - Factor FontPage(Frame) class from ConfigDialog. Slightly modified tests continue to pass. Fix General tests. Patch mostly by Cheryl Sabella.
- bpo-30781: IDLE - Use ttk widgets in ConfigDialog. Patches by Terry Jan Reedy and Cheryl Sabella.
- bpo-31060: IDLE - Finish rearranging methods of ConfigDialog Grouping methods pertaining to each tab and the buttons will aid writing tests and improving the tabs and will enable splitting the groups into classes.
- bpo-30853: IDLE â Factor a VarTrace class out of ConfigDialog. Instance tracers manages pairs consisting of a tk variable and a callback function. When tracing is turned on, setting the variable calls the function. Test coverage for the new class is 100%.
- bpo-31003: IDLE: Add more tests for General tab.
- bpo-30993: IDLE - Improve configdialog font page and tests. In configdialog: Document causal pathways in create_font_tab docstring. Simplify some attribute names. Move set_samples calls to var_changed_font (idea from Cheryl Sabella). Move related functions to positions after the create widgets function. In test_configdialog: Fix test_font_set so not order dependent. Fix renamed test_indent_scale so it tests the widget. Adjust tests for movement of set_samples call. Add tests for load functions. Put all font tests in one class and tab indent tests in another. Except for two lines, these tests completely cover the related functions.
- bpo-30981: IDLE â Add more configdialog font page tests.
- bpo-28523: IDLE: replace âcolourâ with âcolorâ in configdialog.
- bpo-30917: Add tests for idlelib.config.IdleConf. Increase coverage from 46% to 96%. Patch by Louie Lu.
- bpo-30934: Document coverage details for idlelib tests. Add section to idlelib/idle-test/README.txt. Include check that branches are taken both ways. Exclude IDLE-specific code that does not run during unit tests.
- bpo-30913: IDLE: Document ConfigDialog tk Vars, methods, and widgets in docstrings This will facilitate improving the dialog and splitting up the class. Original patch by Cheryl Sabella.
- bpo-30899: IDLE: Add tests for ConfigParser subclasses in config. Patch by Louie Lu.
- bpo-30881: IDLE: Add docstrings to browser.py. Patch by Cheryl Sabella.
- bpo-30851: IDLE: Remove unused variables in configdialog. One is a duplicate, one is set but cannot be altered by users. Patch by Cheryl Sabella.
- bpo-30870: IDLE: In Settings dialog, select font with Up, Down keys as well as mouse. Initial patch by Louie Lu.
- bpo-8231: IDLE: call config.IdleConf.GetUserCfgDir only once.
- bpo-30779: IDLE: Factor ConfigChanges class from configdialog, put in config; test. * In config, put dump test code in a function; run it and unittest in âif __name__ == â__main__â. * Add class config.ConfigChanges based on changes_class_v4.py on bpo issue. * Add class test_config.ChangesTest, partly using configdialog_tests_v1.py. * Revise configdialog to use ConfigChanges; see tracker msg297804. * Revise test_configdialog to match configdialog changes. * Remove configdialog functions unused or moved to ConfigChanges. Cheryl Sabella contributed parts of the patch.
- bpo-30777: IDLE: configdialog - Add docstrings and fix comments. Patch by Cheryl Sabella.
- bpo-30495: IDLE: Improve textview with docstrings, PEP8 names, and more tests. Patch by Cheryl Sabella.
- bpo-30723: IDLE: Make several improvements to parenmatch. Add âparensâ style to highlight both opener and closer. Make âdefaultâ style, which is not default, a synonym for âopenerâ. Make time-delay work the same with all styles. Add help for config dialog extensions tab, including help for parenmatch. Add new tests. Original patch by Charles Wohlganger.
- bpo-30674: IDLE: add docstrings to grep module. Patch by Cheryl Sabella
- bpo-21519: IDLEâs basic custom key entry dialog now detects duplicates properly. Original patch by Saimadhav Heblikar.
- bpo-29910: IDLE no longer deletes a character after commenting out a region by a key shortcut. Add `return 'break'` for this and other
potential conflicts between IDLE and default key bindings.
- bpo-30728: Review and change idlelib.configdialog names. Lowercase method and attribute names. Replace âcolourâ with âcolorâ, expand overly cryptic names, delete unneeded underscores. Replace `import *` with specific
imports. Patches by Cheryl Sabella.
- bpo-6739: IDLE: Verify user-entered key sequences by trying to bind them with tk. Add tests for all 3 validation functions. Original patch by G Polo. Tests added by Cheryl Sabella.

### Tools/DemosÂ¶

- bpo-30983: gdb integration commands (py-bt, etc.) work on optimized shared builds now, too. **PEP 523** introduced _PyEval_EvalFrameDefault which
inlines PyEval_EvalFrameEx on non-debug shared builds.  This broke the
ability to use py-bt, py-up, and a few other Python-specific gdb
integrations. The problem is fixed by only looking for
_PyEval_EvalFrameDefault frames in python-gdb.py.  Original patch by Bruno
âPolacoâ Penteado.

## Python 3.6.2 finalÂ¶

*Release date: 2017-07-17*

No changes since release candidate 2

## Python 3.6.2 release candidate 2Â¶

*Release date: 2017-07-07*

### SecurityÂ¶

- bpo-30730: Prevent environment variables injection in subprocess on Windows. Prevent passing other environment variables and command arguments.
- bpo-30694: Upgrade expat copy from 2.2.0 to 2.2.1 to get fixes of multiple security vulnerabilities including: **CVE 2017-9233** (External entity
infinite loop DoS),**CVE 2016-9063** (Integer overflow, re-fix),**CVE 2016-0718** (Fix regression bugs from 2.2.0âs fix to**CVE 2016-0718** ) and**CVE 2012-0876** (Counter hash flooding with
SipHash). Note: the**CVE 2016-5300** (Use os-specific entropy sources like
getrandom) doesnât impact Python, since Python already gets entropy from
the OS to set the expat secret using`XML_SetHashSalt()` .
- bpo-30500: Fix urllib.parse.splithost() to correctly parse fragments. For example, `splithost('//127.0.0.1#@evil.com/')` now correctly returns the`127.0.0.1` host, instead of treating`@evil.com` as the host in an
authentication (`login@host` ).

## Python 3.6.2 release candidate 1Â¶

*Release date: 2017-06-17*

### SecurityÂ¶

- bpo-29591: Update expat copy from 2.1.1 to 2.2.0 to get fixes of **CVE 2016-0718** and**CVE 2016-4472** . See
https://sourceforge.net/p/expat/bugs/537/ for more information.

### Core and BuiltinsÂ¶

- bpo-30682: Removed a too-strict assertion that failed for certain f-strings, such as eval(âfâ\nââ) and eval(âfâ\rââ).
- bpo-30604: Move co_extra_freefuncs to not be per-thread to avoid crashes
- bpo-29104: Fixed parsing backslashes in f-strings.
- bpo-27945: Fixed various segfaults with dict when input collections are mutated during searching, inserting or comparing. Based on patches by Duane Griffin and Tim Mitchell.
- bpo-25794: Fixed type.__setattr__() and type.__delattr__() for non-interned attribute names. Based on patch by Eryk Sun.
- bpo-30039: If a KeyboardInterrupt happens when the interpreter is in the middle of resuming a chain of nested âyield fromâ or âawaitâ calls, itâs now correctly delivered to the innermost frame.
- bpo-12414: sys.getsizeof() on a code object now returns the sizes which includes the code struct and sizes of objects which it references. Patch by Donghee Na.
- bpo-29949: Fix memory usage regression of set and frozenset object.
- bpo-29935: Fixed error messages in the index() method of tuple, list and deque when pass indices of wrong type.
- bpo-29859: Show correct error messages when any of the pthread_* calls in thread_pthread.h fails.
- bpo-28876: `bool(range)` works even if`len(range)` raises`OverflowError` .
- bpo-29600: Fix wrapping coroutine return values in StopIteration.
- bpo-28856: Fix an oversight that %b format for bytes should support objects follow the buffer protocol.
- bpo-29714: Fix a regression that bytes format may fail when containing zero bytes inside.
- bpo-29478: If max_line_length=None is specified while using the Compat32 policy, it is no longer ignored. Patch by Mircea Cosbuc.

### LibraryÂ¶

- bpo-30616: Functional API of enum allows to create empty enums. Patched by Donghee Na
- bpo-30038: Fix race condition between signal delivery and wakeup file descriptor. Patch by Nathaniel Smith.
- bpo-23894: lib2to3 now recognizes `rb'...'` and`f'...'` strings.
- bpo-23890: unittest.TestCase.assertRaises() now manually breaks a reference cycle to not keep objects alive longer than expected.
- bpo-30149: inspect.signature() now supports callables with variable-argument parameters wrapped with partialmethod. Patch by Donghee Na.
- bpo-30645: Fix path calculation in imp.load_package(), fixing it for cases when a package is only shipped with bytecodes. Patch by Alexandru Ardelean.
- bpo-29931: Fixed comparison check for ipaddress.ip_interface objects. Patch by Sanjay Sundaresan.
- bpo-30605: re.compile() no longer raises a BytesWarning when compiling a bytes instance with misplaced inline modifier. Patch by Roy Williams.
- bpo-24484: Avoid race condition in multiprocessing cleanup (#2159)
- bpo-28994: The traceback no longer displayed for SystemExit raised in a callback registered by atexit.
- bpo-30508: Donât log exceptions if Task/Future âcancel()â method was called.
- bpo-28556: Updates to typing module: Add generic AsyncContextManager, add support for ContextManager on all versions. Original PRs by Jelle Zijlstra and Ivan Levkivskyi
- bpo-29870: Fix ssl sockets leaks when connection is aborted in asyncio/ssl implementation. Patch by MichaÃ«l SghaÃ¯er.
- bpo-29743: Closing transport during handshake process leaks open socket. Patch by Nikolay Kim
- bpo-27585: Fix waiter cancellation in asyncio.Lock. Patch by Mathieu Sornay.
- bpo-30418: On Windows, subprocess.Popen.communicate() now also ignore EINVAL on stdin.write() if the child process is still running but closed the pipe.
- bpo-29822: inspect.isabstract() now works during __init_subclass__. Patch by Nate Soares.
- bpo-29581: ABCMeta.__new__ now accepts `**kwargs` , allowing abstract
base classes to use keyword parameters in __init_subclass__. Patch by Nate
Soares.
- bpo-30557: faulthandler now correctly filters and displays exception codes on Windows
- bpo-30378: Fix the problem that logging.handlers.SysLogHandler cannot handle IPv6 addresses.
- bpo-29960: Preserve generator state when _random.Random.setstate() raises an exception. Patch by Bryan Olson.
- bpo-30414: multiprocessing.Queue._feed background running thread do not break from main loop on exception.
- bpo-30003: Fix handling escape characters in HZ codec. Based on patch by Ma Lin.
- bpo-30301: Fix AttributeError when using SimpleQueue.empty() under *spawn* and*forkserver* start methods.
- bpo-30329: imaplib and poplib now catch the Windows socket WSAEINVAL error (code 10022) on shutdown(SHUT_RDWR): An invalid operation was attempted. This error occurs sometimes on SSL connections.
- bpo-30375: Warnings emitted when compile a regular expression now always point to the line in the user code. Previously they could point into inners of the re module if emitted from inside of groups or conditionals.
- bpo-30048: Fixed `Task.cancel()` can be ignored when the task is running
coroutine and the coroutine returned without any more`await` .
- bpo-30266: contextlib.AbstractContextManager now supports anti-registration by setting __enter__ = None or __exit__ = None, following the pattern introduced in bpo-25958. Patch by Jelle Zijlstra.
- bpo-30298: Weaken the condition of deprecation warnings for inline modifiers. Now allowed several subsequential inline modifiers at the start of the pattern (e.g. `'(?i)(?s)...'` ).  In verbose mode whitespaces and
comments now are allowed before and between inline modifiers (e.g.```
'(?x)
(?i) (?s)...'
```
).
- bpo-29990: Fix range checking in GB18030 decoder. Original patch by Ma Lin.
- bpo-26293: Change resulted because of zipfile breakage. (See also: bpo-29094)
- bpo-30243: Removed the __init__ methods of _jsonâs scanner and encoder. Misusing them could cause memory leaks or crashes. Now scanner and encoder objects are completely initialized in the __new__ methods.
- bpo-30185: Avoid KeyboardInterrupt tracebacks in forkserver helper process when Ctrl-C is received.
- bpo-28556: Various updates to typing module: add typing.NoReturn type, use WrapperDescriptorType, minor bug-fixes. Original PRs by Jim Fasarakis-Hilliard and Ivan Levkivskyi.
- bpo-30205: Fix getsockname() for unbound AF_UNIX sockets on Linux.
- bpo-30070: Fixed leaks and crashes in errors handling in the parser module.
- bpo-30061: Fixed crashes in IOBase methods __next__() and readlines() when readline() or __next__() respectively return non-sizeable object. Fixed possible other errors caused by not checking results of PyObject_Size(), PySequence_Size(), or PyMapping_Size().
- bpo-30017: Allowed calling the close() method of the zip entry writer object multiple times. Writing to a closed writer now always produces a ValueError.
- bpo-30068: _io._IOBase.readlines will check if itâs closed first when hint is present.
- bpo-29694: Fixed race condition in pathlib mkdir with flags parents=True. Patch by Armin Rigo.
- bpo-29692: Fixed arbitrary unchaining of RuntimeError exceptions in contextlib.contextmanager. Patch by Siddharth Velankar.
- bpo-29998: Pickling and copying ImportError now preserves name and path attributes.
- bpo-29953: Fixed memory leaks in the replace() method of datetime and time objects when pass out of bound fold argument.
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
- bpo-28231: The zipfile module now accepts path-like objects for external paths.
- bpo-26915: index() and count() methods of collections.abc.Sequence now check identity before checking equality when do comparisons.
- bpo-29615: SimpleXMLRPCDispatcher no longer chains KeyError (or any other exception) to exception(s) raised in the dispatched methods. Patch by Petr Motejlek.
- bpo-30177: path.resolve(strict=False) no longer cuts the path after the first element not present in the filesystem. Patch by Antoine Pietri.

### IDLEÂ¶

- bpo-15786: Fix several problems with IDLEâs autocompletion box. The following should now work: clicking on selection box items; using the scrollbar; selecting an item by hitting Return. Hangs on MacOSX should no longer happen. Patch by Louie Lu.
- bpo-25514: Add doc subsubsection about IDLE failure to start. Popup no-connection message directs users to this section.
- bpo-30642: Fix reference leaks in IDLE tests. Patches by Louie Lu and Terry Jan Reedy.
- bpo-30495: Add docstrings for textview.py and use PEP8 names. Patches by Cheryl Sabella and Terry Jan Reedy.
- bpo-30290: Help-about: use pep8 names and add tests. Increase coverage to 100%. Patches by Louie Lu, Cheryl Sabella, and Terry Jan Reedy.
- bpo-30303: Add _utest option to textview; add new tests. Increase coverage to 100%. Patches by Louie Lu and Terry Jan Reedy.

### C APIÂ¶

- bpo-27867: Function PySlice_GetIndicesEx() no longer replaced with a macro if Py_LIMITED_API is not set.

### BuildÂ¶

- bpo-29941: Add `--with-assertions` configure flag to explicitly enable C`assert()` checks. Defaults to off.`--with-pydebug` implies`--with-assertions` .
- bpo-28787: Fix out-of-tree builds of Python when configured with `--with--dtrace` .
- bpo-29243: Prevent unnecessary rebuilding of Python during `make test` ,`make install` and some other make targets when configured with`--enable-optimizations` .
- bpo-23404: Donât regenerate generated files based on file modification time anymore: the action is now explicit. Replace `make touch` with`make regen-all` .
- bpo-29643: Fix `--enable-optimization` didnât work.

### DocumentationÂ¶

- bpo-30176: Add missing attribute related constants in curses documentation.
- bpo-30052: the link targets for `bytes()` and`bytearray()` are
now their respective type definitions, rather than the corresponding
builtin function entries. Use bytes and
bytearray to reference the latter. In order to
ensure this and future cross-reference updates are applied automatically,
the daily documentation builds now disable the default output caching
features in Sphinx.
- bpo-26985: Add missing info of code object in inspect documentation.

### Tools/DemosÂ¶

- bpo-29367: python-gdb.py now supports also `method-wrapper` (`wrapperobject` ) objects.

### TestsÂ¶

- bpo-30357: test_thread: setUp() now uses support.threading_setup() and support.threading_cleanup() to wait until threads complete to avoid random side effects on following tests. Initial patch written by Grzegorz Grzywacz.
- bpo-30197: Enhanced functions swap_attr() and swap_item() in the test.support module. They now work when delete replaced attribute or item inside the with statement. The old value of the attribute or item (or None if it doesnât exist) now will be assigned to the target of the âasâ clause, if there is one.

### WindowsÂ¶

- bpo-30687: Locate msbuild.exe on Windows when building rather than vcvarsall.bat
- bpo-30450: The build process on Windows no longer depends on Subversion, instead pulling external code from GitHub via a Python script. If Python 3.6 is not found on the system (via `py -3.6` ), NuGet is used to
download a copy of 32-bit Python.

## Python 3.6.1 finalÂ¶

*Release date: 2017-03-21*

### Core and BuiltinsÂ¶

- bpo-29723: The `sys.path[0]` initialization change for bpo-29139 caused
a regression by revealing an inconsistency in how sys.path is initialized
when executing`__main__` from a zipfile, directory, or other import
location. The interpreter now consistently avoids ever adding the import
locationâs parent directory to`sys.path` , and ensures no other`sys.path` entries are inadvertently modified when inserting the import
location named on the command line.

### BuildÂ¶

- bpo-27593: fix format of git information used in sys.version
- Fix incompatible comment in python.h

## Python 3.6.1 release candidate 1Â¶

*Release date: 2017-03-04*

### Core and BuiltinsÂ¶

- bpo-28893: Set correct __cause__ for errors about invalid awaitables returned from __aiter__ and __anext__.
- bpo-29683: Fixes to memory allocation in _PyCode_SetExtra. Patch by Brian Coleman.
- bpo-29684: Fix minor regression of PyEval_CallObjectWithKeywords. It should raise TypeError when kwargs is not a dict. But it might cause segv when args=NULL and kwargs is not a dict.
- bpo-28598: Support __rmod__ for subclasses of str being called before str.__mod__. Patch by Martijn Pieters.
- bpo-29607: Fix stack_effect computation for CALL_FUNCTION_EX. Patch by Matthieu Dartiailh.
- bpo-29602: Fix incorrect handling of signed zeros in complex constructor for complex subclasses and for inputs having a __complex__ method. Patch by Serhiy Storchaka.
- bpo-29347: Fixed possibly dereferencing undefined pointers when creating weakref objects.
- bpo-29438: Fixed use-after-free problem in key sharing dict.
- bpo-29319: Prevent RunMainFromImporter overwriting sys.path[0].
- bpo-29337: Fixed possible BytesWarning when compare the code objects. Warnings could be emitted at compile time.
- bpo-29327: Fixed a crash when pass the iterable keyword argument to sorted().
- bpo-29034: Fix memory leak and use-after-free in os module (path_converter).
- bpo-29159: Fix regression in bytes(x) when x.__index__() raises Exception.
- bpo-28932: Do not include <sys/random.h> if it does not exist.
- bpo-25677: Correct the positioning of the syntax error caret for indented blocks. Based on patch by Michael Layzell.
- bpo-29000: Fixed bytes formatting of octals with zero padding in alternate form.
- bpo-26919: On Android, operating system data is now always encoded/decoded to/from UTF-8, instead of the locale encoding to avoid inconsistencies with os.fsencode() and os.fsdecode() which are already using UTF-8.
- bpo-28991: functools.lru_cache() was susceptible to an obscure reentrancy bug triggerable by a monkey-patched len() function.
- bpo-28739: f-string expressions are no longer accepted as docstrings and by ast.literal_eval() even if they do not include expressions.
- bpo-28512: Fixed setting the offset attribute of SyntaxError by PyErr_SyntaxLocationEx() and PyErr_SyntaxLocationObject().
- bpo-28918: Fix the cross compilation of xxlimited when Python has been built with Py_DEBUG defined.
- bpo-28731: Optimize _PyDict_NewPresized() to create correct size dict. Improve speed of dict literal with constant keys up to 30%.

### LibraryÂ¶

- bpo-29169: Update zlib to 1.2.11.
- bpo-29623: Allow use of path-like object as a single argument in ConfigParser.read(). Patch by David Ellis.
- bpo-28963: Fix out of bound iteration in asyncio.Future.remove_done_callback implemented in C.
- bpo-29704: asyncio.subprocess.SubprocessStreamProtocol no longer closes before all pipes are closed.
- bpo-29271: Fix Task.current_task and Task.all_tasks implemented in C to accept None argument as their pure Python implementation.
- bpo-29703: Fix asyncio to support instantiation of new event loops in child processes.
- bpo-29376: Fix assertion error in threading._DummyThread.is_alive().
- bpo-28624: Add a test that checks that cwd parameter of Popen() accepts PathLike objects. Patch by Sayan Chowdhury.
- bpo-28518: Start a transaction implicitly before a DML statement. Patch by Aviv Palivoda.
- bpo-29532: Altering a kwarg dictionary passed to functools.partial() no longer affects a partial object after creation.
- bpo-29110: Fix file object leak in aifc.open() when file is given as a filesystem path and is not in valid AIFF format. Patch by Anthony Zhang.
- bpo-28556: Various updates to typing module: typing.Counter, typing.ChainMap, improved ABC caching, etc. Original PRs by Jelle Zijlstra, Ivan Levkivskyi, Manuel Krebber, and Åukasz Langa.
- bpo-29100: Fix datetime.fromtimestamp() regression introduced in Python 3.6.0: check minimum and maximum years.
- bpo-29519: Fix weakref spewing exceptions during interpreter shutdown when used with a rare combination of multiprocessing and custom codecs.
- bpo-29416: Prevent infinite loop in pathlib.Path.mkdir
- bpo-29444: Fixed out-of-bounds buffer access in the group() method of the match object. Based on patch by WGH.
- bpo-29335: Fix subprocess.Popen.wait() when the child process has exited to a stopped instead of terminated state (ex: when under ptrace).
- bpo-29290: Fix a regression in argparse that help messages would wrap at non-breaking spaces.
- bpo-28735: Fixed the comparison of mock.MagickMock with mock.ANY.
- bpo-29316: Restore the provisional status of typing module, add corresponding note to documentation. Patch by Ivan L.
- bpo-29219: Fixed infinite recursion in the repr of uninitialized ctypes.CDLL instances.
- bpo-29011: Fix an important omission by adding Deque to the typing module.
- bpo-28969: Fixed race condition in C implementation of functools.lru_cache. KeyError could be raised when cached function with full cache was simultaneously called from different threads with the same uncached arguments.
- bpo-29142: In urllib.request, suffixes in no_proxy environment variable with leading dots could match related hostnames again (e.g. .b.c matches a.b.c). Patch by Milan Oberkirch.
- bpo-28961: Fix unittest.mock._Call helper: donât ignore the name parameter anymore. Patch written by Jiajun Huang.
- bpo-29203: functools.lru_cache() now respects **PEP 468** and preserves the
order of keyword arguments.  f(a=1, b=2) is now cached separately from
f(b=2, a=1) since both calls could potentially give different results.
- bpo-15812: inspect.getframeinfo() now correctly shows the first line of a context. Patch by Sam Breese.
- bpo-29094: Offsets in a ZIP file created with extern file object and modes âwâ and âxâ now are relative to the start of the file.
- bpo-29085: Allow random.Random.seed() to use high quality OS randomness rather than the pid and time.
- bpo-29061: Fixed bug in secrets.randbelow() which would hang when given a negative input. Patch by Brendan Donegan.
- bpo-29079: Prevent infinite loop in pathlib.resolve() on Windows
- bpo-13051: Fixed recursion errors in large or resized curses.textpad.Textbox. Based on patch by Tycho Andersen.
- bpo-29119: Fix weakrefs in the pure python version of collections.OrderedDict move_to_end() method. Contributed by Andra Bogildea.
- bpo-9770: curses.ascii predicates now work correctly with negative integers.
- bpo-28427: old keys should not remove new values from WeakValueDictionary when collecting from another thread.
- bpo-28923: Remove editor artifacts from Tix.py.
- bpo-29055: Neaten-up empty population error on random.choice() by suppressing the upstream exception.
- bpo-28871: Fixed a crash when deallocate deep ElementTree.
- bpo-19542: Fix bugs in WeakValueDictionary.setdefault() and WeakValueDictionary.pop() when a GC collection happens in another thread.
- bpo-20191: Fixed a crash in resource.prlimit() when passing a sequence that doesnât own its elements as limits.
- bpo-28779: multiprocessing.set_forkserver_preload() would crash the forkserver process if a preloaded module instantiated some multiprocessing objects such as locks.
- bpo-28847: dbm.dumb now supports reading read-only files and no longer writes the index file when it is not changed.
- bpo-26937: The chown() method of the tarfile.TarFile class does not fail now when the grp module cannot be imported, as for example on Android platforms.

### IDLEÂ¶

### WindowsÂ¶

- bpo-29579: Removes readme.txt from the installer
- bpo-29326: Ignores blank lines in ._pth files (Patch by Alexey Izbyshev)
- bpo-28164: Correctly handle special console filenames (patch by Eryk Sun)
- bpo-29409: Implement **PEP 529** for io.FileIO (Patch by Eryk Sun)
- bpo-29392: Prevent crash when passing invalid arguments into msvcrt module.
- bpo-25778: winreg does not truncate string correctly (Patch by Eryk Sun)
- bpo-28896: Deprecate WindowsRegistryFinder and disable it by default.

### C APIÂ¶

- bpo-27867: Function PySlice_GetIndicesEx() is replaced with a macro if Py_LIMITED_API is not set or set to the value between 0x03050400 and 0x03060000 (not including) or 0x03060100 or higher.
- bpo-29083: Fixed the declaration of some public API functions. PyArg_VaParse() and PyArg_VaParseTupleAndKeywords() were not available in limited API. PyArg_ValidateKeywordArguments(), PyArg_UnpackTuple() and Py_BuildValue() were not available in limited API of version < 3.3 when PY_SSIZE_T_CLEAN is defined.
- bpo-29058: All stable API extensions added after Python 3.2 are now available only when Py_LIMITED_API is set to the PY_VERSION_HEX value of the minimum Python version supporting this API.

### DocumentationÂ¶

- bpo-28929: Link the documentation to its source file on GitHub.
- bpo-25008: Document smtpd.py as effectively deprecated and add a pointer to aiosmtpd, a third-party asyncio-based replacement.
- bpo-26355: Add canonical header link on each page to corresponding major version of the documentation. Patch by Matthias Bussonnier.
- bpo-29349: Fix Python 2 syntax in code for building the documentation.

### TestsÂ¶

- bpo-28087: Skip test_asyncore and test_eintr poll failures on macOS. Skip some tests of select.poll when running on macOS due to unresolved issues with the underlying system poll function on some macOS versions.
- bpo-29571: to match the behaviour of the `re.LOCALE` flag,
test_re.test_locale_flag now uses`locale.getpreferredencoding(False)` to determine the candidate encoding for the test regex (allowing it to
correctly skip the test when the default locale encoding is a multi-byte
encoding)
- bpo-28950: Disallow -j0 to be combined with -T/-l in regrtest command line arguments.
- bpo-28683: Fix the tests that bind() a unix socket and raise PermissionError on Android for a non-root user.
- bpo-26939: Add the support.setswitchinterval() function to fix test_functools hanging on the Android armv7 qemu emulator.

### BuildÂ¶

- bpo-27593: sys.version and the platform module python_build(), python_branch(), and python_revision() functions now use git information rather than hg when building from a repo.
- bpo-29572: Update Windows build and OS X installers to use OpenSSL 1.0.2k.
- bpo-26851: Set Android compilation and link flags.
- bpo-28768: Fix implicit declaration of function _setmode. Patch by Masayuki Yamamoto
- bpo-29080: Removes hard dependency on hg.exe from PCBuild/build.bat
- bpo-23903: Added missed names to PC/python3.def.
- bpo-28762: lockf() is available on Android API level 24, but the F_LOCK macro is not defined in android-ndk-r13.
- bpo-28538: Fix the compilation error that occurs because if_nameindex() is available on Android API level 24, but the if_nameindex structure is not defined.
- bpo-20211: Do not add the directory for installing C header files and the directory for installing object code libraries to the cross compilation search paths. Original patch by Thomas Petazzoni.
- bpo-28849: Do not define sys.implementation._multiarch on Android.

## Python 3.6.0 finalÂ¶

*Release date: 2016-12-23*

No changes since release candidate 2

## Python 3.6.0 release candidate 2Â¶

*Release date: 2016-12-16*

### Core and BuiltinsÂ¶

### Tools/DemosÂ¶

- bpo-28770: Fix python-gdb.py for fastcalls.

### WindowsÂ¶

- bpo-28896: Deprecate WindowsRegistryFinder.

### BuildÂ¶

- bpo-28898: Prevent gdb build errors due to HAVE_LONG_LONG redefinition.

## Python 3.6.0 release candidate 1Â¶

*Release date: 2016-12-06*

### Core and BuiltinsÂ¶

- bpo-23722: Rather than silently producing a class that doesnât support zero-argument `super()` in methods, failing to pass the new`__classcell__` namespace entry up to`type.__new__` now results in a`DeprecationWarning` and a class that supports zero-argument`super()` .
- bpo-28797: Modifying the class __dict__ inside the __set_name__ method of a descriptor that is used inside that class no longer prevents calling the __set_name__ method of other descriptors.
- bpo-28782: Fix a bug in the implementation `yield from` when checking if
the next instruction is YIELD_FROM. Regression introduced by WORDCODE
(bpo-26647).

### LibraryÂ¶

- bpo-27030: Unknown escapes in re.sub() replacement template are allowed again. But they still are deprecated and will be disabled in 3.7.
- bpo-28835: Fix a regression introduced in warnings.catch_warnings(): call warnings.showwarning() if it was overridden inside the context manager.
- bpo-27172: To assist with upgrades from 2.7, the previously documented deprecation of `inspect.getfullargspec()` has been reversed. This
decision may be revisited again after the Python 2.7 branch is no longer
officially supported.
- bpo-26273: Add new `socket.TCP_CONGESTION` (Linux 2.6.13) and`socket.TCP_USER_TIMEOUT` (Linux 2.6.37) constants. Patch written
by Omar Sandoval.
- bpo-24142: Reading a corrupt config file left configparser in an invalid state. Original patch by Florian HÃ¶ch.
- bpo-28843: Fix asyncio C Task to handle exceptions __traceback__.

### C APIÂ¶

- bpo-28808: PyUnicode_CompareWithASCIIString() now never raises exceptions.

### DocumentationÂ¶

### Tools/DemosÂ¶

- bpo-28023: Fix python-gdb.py didnât support new dict implementation.

## Python 3.6.0 beta 4Â¶

*Release date: 2016-11-21*

### Core and BuiltinsÂ¶

- bpo-28532: Show sys.version when -V option is supplied twice.
- bpo-27100: The with-statement now checks for __enter__ before it checks for __exit__. This gives less confusing error messages when both methods are missing. Patch by Jonathan Ellington.
- bpo-28746: Fix the set_inheritable() file descriptor method on platforms that do not have the ioctl FIOCLEX and FIONCLEX commands.
- bpo-26920: Fix not getting the localeâs charset upon initializing the interpreter, on platforms that do not have langinfo.
- bpo-28648: Fixed crash in Py_DecodeLocale() in debug build on Mac OS X when decode astral characters. Patch by Xiang Zhang.
- bpo-19398: Extra slash no longer added to sys.path components in case of empty compile-time PYTHONPATH components.
- bpo-28665: Improve speed of the STORE_DEREF opcode by 40%.
- bpo-28583: PyDict_SetDefault didnât combine split table when needed. Patch by Xiang Zhang.
- bpo-27243: Change PendingDeprecationWarning -> DeprecationWarning. As it was agreed in the issue, __aiter__ returning an awaitable should result in PendingDeprecationWarning in 3.5 and in DeprecationWarning in 3.6.
- bpo-26182: Fix a refleak in code that raises DeprecationWarning.
- bpo-28721: Fix asynchronous generators aclose() and athrow() to handle StopAsyncIteration propagation properly.

### LibraryÂ¶

- bpo-28752: Restored the __reduce__() methods of datetime objects.
- bpo-28727: Regular expression patterns, _sre.SRE_Pattern objects created by re.compile(), become comparable (only x==y and x!=y operators). This change should fix the bpo-18383: donât duplicate warning filters when the warnings module is reloaded (thing usually only done in unit tests).
- bpo-20572: The subprocess.Popen.wait methodâs undocumented endtime parameter now raises a DeprecationWarning.
- bpo-25659: In ctypes, prevent a crash calling the from_buffer() and from_buffer_copy() methods on abstract classes like Array.
- bpo-19717: Makes Path.resolve() succeed on paths that do not exist. Patch by Vajrasky Kok
- bpo-28563: Fixed possible DoS and arbitrary code execution when handle plural form selections in the gettext module. The expression parser now supports exact syntax supported by GNU gettext.
- bpo-28387: Fixed possible crash in _io.TextIOWrapper deallocator when the garbage collector is invoked in other thread. Based on patch by Sebastian Cufre.
- bpo-28600: Optimize loop.call_soon.
- bpo-28613: Fix get_event_loop() return the current loop if called from coroutines/callbacks.
- bpo-28634: Fix asyncio.isfuture() to support unittest.Mock.
- bpo-26081: Fix refleak in _asyncio.Future.__iter__().throw.
- bpo-28639: Fix inspect.isawaitable to always return bool Patch by Justin Mayfield.
- bpo-28652: Make loop methods reject socket kinds they do not support.
- bpo-28653: Fix a refleak in functools.lru_cache.
- bpo-28703: Fix asyncio.iscoroutinefunction to handle Mock objects.
- bpo-28704: Fix create_unix_server to support Path-like objects (PEP 519).
- bpo-28720: Add collections.abc.AsyncGenerator.

### DocumentationÂ¶

- bpo-28513: Documented command-line interface of zipfile.

### TestsÂ¶

### BuildÂ¶

## Python 3.6.0 beta 3Â¶

*Release date: 2016-10-31*

### Core and BuiltinsÂ¶

- bpo-28128: Deprecation warning for invalid str and byte escape sequences now prints better information about where the error occurs. Patch by Serhiy Storchaka and Eric Smith.
- bpo-28509: dict.update() no longer allocate unnecessary large memory.
- bpo-28426: Fixed potential crash in PyUnicode_AsDecodedObject() in debug build.
- bpo-28517: Fixed of-by-one error in the peephole optimizer that caused keeping unreachable code.
- bpo-28214: Improved exception reporting for problematic __set_name__ attributes.
- bpo-23782: Fixed possible memory leak in _PyTraceback_Add() and exception loss in PyTraceBack_Here().
- bpo-28471: Fix âPython memory allocator called without holding the GILâ crash in socket.setblocking.

### LibraryÂ¶

- bpo-27517: LZMA compressor and decompressor no longer raise exceptions if given empty data twice. Patch by Benjamin Fogle.
- bpo-28549: Fixed segfault in cursesâs addch() with ncurses6.
- bpo-28449: tarfile.open() with mode ârâ or âr:â now tries to open a tar file with compression before trying to open it without compression. Otherwise it had 50% chance failed with ignore_zeros=True.
- bpo-23262: The webbrowser module now supports Firefox 36+ and derived browsers. Based on patch by Oleg Broytman.
- bpo-27939: Fixed bugs in tkinter.ttk.LabeledScale and tkinter.Scale caused by representing the scale as float value internally in Tk. tkinter.IntVar now works if float value is set to underlying Tk variable.
- bpo-18844: The various ways of specifying weights for random.choices() now produce the same result sequences.
- bpo-28255: calendar.TextCalendar().prmonth() no longer prints a space at the start of new line after printing a monthâs calendar. Patch by Xiang Zhang.
- bpo-20491: The textwrap.TextWrapper class now honors non-breaking spaces. Based on patch by Kaarle Ritvanen.
- bpo-28353: os.fwalk() no longer fails on broken links.
- bpo-28430: Fix iterator of C implemented asyncio.Future doesnât accept non-None value is passed to it.send(val).
- bpo-27025: Generated names for Tkinter widgets now start by the â!â prefix for readability.
- bpo-25464: Fixed HList.header_exists() in tkinter.tix module by addin a workaround to Tix library bug.
- bpo-28488: shutil.make_archive() no longer adds entry â./â to ZIP archive.
- bpo-25953: re.sub() now raises an error for invalid numerical group reference in replacement template even if the pattern is not found in the string. Error message for invalid group reference now includes the group index and the position of the reference. Based on patch by SilentGhost.
- bpo-18219: Optimize csv.DictWriter for large number of columns. Patch by Mariatta Wijaya.
- bpo-28448: Fix C implemented asyncio.Future didnât work on Windows.
- bpo-28480: Fix error building socket module when multithreading is disabled.
- bpo-24452: Make webbrowser support Chrome on Mac OS X.
- bpo-20766: Fix references leaked by pdb in the handling of SIGINT handlers.
- bpo-28492: Fix how StopIteration exception is raised in _asyncio.Future.
- bpo-28500: Fix asyncio to handle async gens GC from another thread.
- bpo-26923: Fix asyncio.Gather to refuse being cancelled once all children are done. Patch by Johannes Ebke.
- bpo-26796: Donât configure the number of workers for default threadpool executor. Initial patch by Hans Lawrenz.
- bpo-28544: Implement asyncio.Task in C.

### WindowsÂ¶

- bpo-28522: Fixes mishandled buffer reallocation in getpathp.c

### BuildÂ¶

### TestsÂ¶

## Python 3.6.0 beta 2Â¶

*Release date: 2016-10-10*

### Core and BuiltinsÂ¶

- bpo-28183: Optimize and cleanup dict iteration.
- bpo-26081: Added C implementation of asyncio.Future. Original patch by Yury Selivanov.
- bpo-28379: Added sanity checks and tests for PyUnicode_CopyCharacters(). Patch by Xiang Zhang.
- bpo-28376: The type of long range iterator is now registered as Iterator. Patch by Oren Milman.
- bpo-28376: Creating instances of range_iterator by calling range_iterator type now is deprecated. Patch by Oren Milman.
- bpo-28376: The constructor of range_iterator now checks that step is not 0. Patch by Oren Milman.
- bpo-26906: Resolving special methods of uninitialized type now causes implicit initialization of the type instead of a fail.
- bpo-18287: PyType_Ready() now checks that tp_name is not NULL. Original patch by Niklas Koep.
- bpo-24098: Fixed possible crash when AST is changed in process of compiling it.
- bpo-28201: Dict reduces possibility of 2nd conflict in hash table when hashes have same lower bits.
- bpo-28350: String constants with null character no longer interned.
- bpo-26617: Fix crash when GC runs during weakref callbacks.
- bpo-27942: String constants now interned recursively in tuples and frozensets.
- bpo-21578: Fixed misleading error message when ImportError called with invalid keyword args.
- bpo-28203: Fix incorrect type in complex(1.0, {2:3}) error message. Patch by Soumya Sharma.
- bpo-28086: Single var-positional argument of tuple subtype was passed unscathed to the C-defined function. Now it is converted to exact tuple.
- bpo-28214: Now __set_name__ is looked up on the class instead of the instance.
- bpo-27955: Fallback on reading /dev/urandom device when the getrandom() syscall fails with EPERM, for example when blocked by SECCOMP.
- bpo-28192: Donât import readline in isolated mode.
- Upgrade internal unicode databases to Unicode version 9.0.0.
- bpo-28131: Fix a regression in zipimportâs compile_source(). zipimport should use the same optimization level as the interpreter.
- bpo-28126: Replace Py_MEMCPY with memcpy(). Visual Studio can properly optimize memcpy().
- bpo-28120: Fix dict.pop() for split dictionary when trying to remove a âpending keyâ (Not yet inserted in split-table). Patch by Xiang Zhang.
- bpo-26182: Raise DeprecationWarning when async and await keywords are used as variable/attribute/class/function name.

### LibraryÂ¶

- bpo-27998: Fixed bytes path support in os.scandir() on Windows. Patch by Eryk Sun.
- bpo-28317: The disassembler now decodes FORMAT_VALUE argument.
- bpo-26293: Fixed writing ZIP files that starts not from the start of the file. Offsets in ZIP file now are relative to the start of the archive in conforming to the specification.
- bpo-28380: unittest.mock Mock autospec functions now properly support assert_called, assert_not_called, and assert_called_once.
- bpo-27181: remove statistics.geometric_mean and defer until 3.7.
- bpo-28229: lzma module now supports pathlib.
- bpo-28321: Fixed writing non-BMP characters with binary format in plistlib.
- bpo-28225: bz2 module now supports pathlib. Initial patch by Ethan Furman.
- bpo-28227: gzip now supports pathlib. Patch by Ethan Furman.
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
- bpo-28368: Refuse monitoring processes if the child watcher has no loop attached. Patch by Vincent Michel.
- bpo-28369: Raise RuntimeError when transportâs FD is used with add_reader, add_writer, etc.
- bpo-28370: Speedup asyncio.StreamReader.readexactly. Patch by ÐÐ¾ÑÐµÐ½Ð±ÐµÑÐ³ ÐÐ°ÑÐº.
- bpo-28371: Deprecate passing asyncio.Handles to run_in_executor.
- bpo-28372: Fix asyncio to support formatting of non-python coroutines.
- bpo-28399: Remove UNIX socket from FS before binding. Patch by ÐÐ¾ÑÐµÐ½Ð±ÐµÑÐ³ ÐÐ°ÑÐº.
- bpo-27972: Prohibit Tasks to await on themselves.

### WindowsÂ¶

- bpo-28402: Adds signed catalog files for stdlib on Windows.
- bpo-28333: Enables Unicode for ps1/ps2 and input() prompts. (Patch by Eryk Sun)
- bpo-28251: Improvements to help manuals on Windows.
- bpo-28110: launcher.msi has different product codes between 32-bit and 64-bit
- bpo-28161: Opening CON for write access fails
- bpo-28162: WindowsConsoleIO readall() fails if first line starts with Ctrl+Z
- bpo-28163: WindowsConsoleIO fileno() passes wrong flags to _open_osfhandle
- bpo-28164: _PyIO_get_console_type fails for various paths
- bpo-28137: Renames Windows path file to ._pth
- bpo-28138: Windows ._pth file should allow import site

### C APIÂ¶

- bpo-28426: Deprecated undocumented functions PyUnicode_AsEncodedObject(), PyUnicode_AsDecodedObject(), PyUnicode_AsDecodedUnicode() and PyUnicode_AsEncodedUnicode().

### BuildÂ¶

- bpo-28258: Fixed build with Estonian locale (python-config and distclean targets in Makefile). Patch by Arfrever Frehtes Taifersar Arahesis.
- bpo-26661: setup.py now detects system libffi with multiarch wrapper.
- bpo-15819: Remove redundant include search directory option for building outside the source tree.

### TestsÂ¶

- bpo-28217: Adds _testconsole module to test console input.

## Python 3.6.0 beta 1Â¶

*Release date: 2016-09-12*

### Core and BuiltinsÂ¶

- bpo-23722: The __class__ cell used by zero-argument super() is now initialized from type.__new__ rather than __build_class__, so class methods relying on that will now work correctly when called from metaclass methods during class creation. Patch by Martin Teichmann.
- bpo-25221: Fix corrupted result from PyLong_FromLong(0) when Python is compiled with NSMALLPOSINTS = 0.
- bpo-27080: Implement formatting support for **PEP 515** .  Initial patch by
Chris Angelico.
- bpo-27199: In tarfile, expose copyfileobj bufsize to improve throughput. Patch by Jason Fried.
- bpo-27948: In f-strings, only allow backslashes inside the braces (where the expressions are). This is a breaking change from the 3.6 alpha releases, where backslashes are allowed anywhere in an f-string. Also, require that expressions inside f-strings be enclosed within literal braces, and not escapes like `f'\x7b"hi"\x7d'` .
- bpo-28046: Remove platform-specific directories from sys.path.
- bpo-28071: Add early-out for differencing from an empty set.
- bpo-25758: Prevents zipimport from unnecessarily encoding a filename (patch by Eryk Sun)
- bpo-25856: The __module__ attribute of extension classes and functions now is interned. This leads to more compact pickle data with protocol 4.
- bpo-27213: Rework CALL_FUNCTION* opcodes to produce shorter and more efficient bytecode. Patch by Demur Rumed, design by Serhiy Storchaka, reviewed by Serhiy Storchaka and Victor Stinner.
- bpo-26331: Implement tokenizing support for **PEP 515** . Patch by Georg
Brandl.
- bpo-27999: Make âglobal after useâ a SyntaxError, and ditto for nonlocal. Patch by Ivan Levkivskyi.
- bpo-28003: Implement **PEP 525** â Asynchronous Generators.
- bpo-27985: Implement **PEP 526** â Syntax for Variable Annotations. Patch
by Ivan Levkivskyi.
- bpo-26058: Add a new private version to the builtin dict type, incremented at each dictionary creation and at each dictionary change. Implementation of the PEP 509.
- bpo-27364: A backslash-character pair that is not a valid escape sequence now generates a DeprecationWarning. Patch by Emanuel Barry.
- bpo-27350: `dict` implementation is changed like PyPy. It is more
compact and preserves insertion order. (Concept developed by Raymond
Hettinger and patch by Inada Naoki.)
- bpo-27911: Remove unnecessary error checks in `exec_builtin_or_dynamic()` .
- bpo-27078: Added BUILD_STRING opcode. Optimized f-strings evaluation.
- bpo-17884: Python now requires systems with inttypes.h and stdint.h
- bpo-27961: Require platforms to support `long long` . Python hasnât
compiled without`long long` for years, so this is basically a
formality.
- bpo-27355: Removed support for Windows CE. It was never finished, and Windows CE is no longer a relevant platform for Python.
- Implement **PEP 523** .
- bpo-27870: A left shift of zero by a large integer no longer attempts to allocate large amounts of memory.
- bpo-25402: In int-to-decimal-string conversion, improve the estimate of the intermediate memory required, and remove an unnecessarily strict overflow check. Patch by Serhiy Storchaka.
- bpo-27214: In long_invert, be more careful about modifying object returned by long_add, and remove an unnecessary check for small longs. Thanks Oren Milman for analysis and patch.
- bpo-27506: Support passing the bytes/bytearray.translate() âdeleteâ argument by keyword.
- bpo-27812: Properly clear out a generatorâs frameâs backreference to the generator to prevent crashes in frame.clear().
