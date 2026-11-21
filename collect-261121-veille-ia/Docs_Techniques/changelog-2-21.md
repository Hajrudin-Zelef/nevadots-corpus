---
id: collect-261121-veille-ia/docs-techniques/changelog-2-21
title: "Changelog"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["test", "tests", "patch", "added", "module", "unittest", "package", "discovery"]
source: https://docs.python.org/release/3.15.1/whatsnew/changelog.html
source_anchor: ""
source_lines: [30001, 30084]
section: "Docs_Techniques"
---
.
- bpo-21514: The documentation of the json module now refers to new JSON RFC 7159 instead of obsoleted RFC 4627.
- bpo-21777: The binary sequence methods on bytes and bytearray are now documented explicitly, rather than assuming users will be able to derive the expected behaviour from the behaviour of the corresponding str methods.
- bpo-6916: undocument deprecated asynchat.fifo class.
- bpo-17386: Expanded functionality of the `Doc/make.bat` script to make
it much more comparable to`Doc/Makefile` .
- bpo-21312: Update the thread_foobar.h template file to include newer threading APIs. Patch by Jack McCracken.
- bpo-21043: Remove the recommendation for specific CA organizations and to mention the ability to load the OS certificates.
- bpo-20765: Add missing documentation for PurePath.with_name() and PurePath.with_suffix().
- bpo-19407: New package installation and distribution guides based on the Python Packaging Authority tools. Existing guides have been retained as legacy links from the distutils docs, as they still contain some required reference material for tool developers that isnât recorded anywhere else.
- bpo-19697: Document cases where __main__.__spec__ is None.

### TestsÂ¶

- bpo-18982: Add tests for CLI of the calendar module.
- bpo-19548: Added some additional checks to test_codecs to ensure that statements in the updated documentation remain accurate. Patch by Martin Panter.
- bpo-22838: All test_re tests now work with unittest test discovery.
- bpo-22173: Update lib2to3 tests to use unittest test discovery.
- bpo-16000: Convert test_curses to use unittest.
- bpo-21456: Skip two tests in test_urllib2net.py if _ssl module not present. Patch by Remi Pointel.
- bpo-20746: Fix test_pdb to run in refleak mode (-R). Patch by Xavier de Gaye.
- bpo-22060: test_ctypes has been somewhat cleaned up and simplified; it now uses unittest test discovery to find its tests.
- bpo-22104: regrtest.py no longer holds a reference to the suite of tests loaded from test modules that donât define test_main().
- bpo-22111: Assorted cleanups in test_imaplib. Patch by Milan Oberkirch.
- bpo-22002: Added `load_package_tests` function to test.support and used
it to implement/augment test discovery in test_asyncio, test_email,
test_importlib, test_json, and test_tools.
- bpo-21976: Fix test_ssl to accept LibreSSL version strings. Thanks to William Orr.
- bpo-21918: Converted test_tools from a module to a package containing separate test files for each tested script.
- bpo-9554: Use modern unittest features in test_argparse. Initial patch by Denver Coneybeare and Radu Voicilas.
- bpo-20155: Changed HTTP method names in failing tests in test_httpservers so that packet filtering software (specifically Windows Base Filtering Engine) does not interfere with the transaction semantics expected by the tests.
- bpo-19493: Refactored the ctypes test package to skip tests explicitly rather than silently.
- bpo-18492: All resources are now allowed when tests are not run by regrtest.py.
- bpo-21634: Fix pystone micro-benchmark: use floor division instead of true division to benchmark integers instead of floating-point numbers. Set pystone version to 1.2. Patch written by Lennart Regebro.
- bpo-21605: Added tests for Tkinter images.
- bpo-21493: Added test for ntpath.expanduser(). Original patch by Claudiu Popa.
- bpo-19925: Added tests for the spwd module. Original patch by Vajrasky Kok.
- bpo-21522: Added Tkinter tests for Listbox.itemconfigure(), PanedWindow.paneconfigure(), and Menu.entryconfigure().
- bpo-17756: Fix test_code test when run from the installed location.
- bpo-17752: Fix distutils tests when run from the installed location.
- bpo-18604: Consolidated checks for GUI availability. All platforms now at least check whether Tk can be instantiated when the GUI resource is requested.
- bpo-21275: Fix a socket test on KFreeBSD.
- bpo-21223: Pass test_site/test_startup_imports when some of the extensions are built as builtins.
- bpo-20635: Added tests for Tk geometry managers.
- Add test case for freeze.
- bpo-20743: Fix a reference leak in test_tcl.
- bpo-21097: Move test_namespace_pkgs into test_importlib.
- bpo-21503: Use test_both() consistently in test_importlib.
- bpo-20939: Avoid various network test failures due to new redirect of http://www.python.org/ to https://www.python.org: use http://www.example.com instead.
- bpo-20668: asyncio tests no longer rely on tests.txt file. (Patch by Vajrasky Kok)
- bpo-21093: Prevent failures of ctypes test_macholib on OS X if a copy of libz exists in $HOME/lib or /usr/local/lib.
- bpo-22770: Prevent some Tk segfaults on OS X when running gui tests.
- bpo-23211: Workaround test_logging failure on some OS X 10.6 systems.
- bpo-23345: Prevent test_ssl failures with large OpenSSL patch level values (like 0.9.8zc).

### Tools/DemosÂ¶

- bpo-22314: pydoc now works when the LINES environment variable is set.
- bpo-22615: Argument Clinic now supports the âtypeâ argument for the int converter. This permits using the int converter with enums and typedefs.
- bpo-20076: The makelocalealias.py script no longer ignores UTF-8 mapping.
- bpo-20079: The makelocalealias.py script now can parse the SUPPORTED file from glibc sources and supports command line options for source paths.
- bpo-22201: Command-line interface of the zipfile module now correctly extracts ZIP files with directory entries. Patch by Ryan Wilson.
- bpo-22120: For functions using an unsigned integer return converter, Argument Clinic now generates a cast to that type for the comparison to -1 in the generated code. (This suppresses a compilation warning.)
- bpo-18974: Tools/scripts/diff.py now uses argparse instead of optparse.
- bpo-21906: Make Tools/scripts/md5sum.py work in Python 3. Patch by Zachary Ware.
- bpo-21629: Fix Argument Clinicâs ââconvertersâ feature.
- Add support for `yield from` to 2to3.
- Add support for the **PEP 465** matrix multiplication operator to 2to3.
- bpo-16047: Fix module exception list and __file__ handling in freeze. Patch by Meador Inge.
- bpo-11824: Consider ABI tags in freeze. Patch by Meador Inge.
- bpo-20535: PYTHONWARNING no longer affects the run_tests.py script. Patch by Arfrever Frehtes Taifersar Arahesis.

### WindowsÂ¶

- bpo-23260: Update Windows installer
- The bundled version of Tcl/Tk has been updated to 8.6.3. The most visible result of this change is the addition of new native file dialogs when running on Windows Vista or newer. See Tcl/Tkâs TIP 432 for more information. Also, this version of Tcl/Tk includes support for Windows 10.
- bpo-17896: The Windows build scripts now expect external library sources to be in `PCbuild\..\externals` rather than`PCbuild\..\..` .
- bpo-17717: The Windows build scripts now use a copy of NASM pulled from svn.python.org to build OpenSSL.
- bpo-21907: Improved the batch scripts provided for building Python.
- bpo-22644: The bundled version of OpenSSL has been updated to 1.0.1j.
- bpo-10747: Use versioned labels in the Windows start menu. Patch by Olive Kilburn.
- bpo-22980: .pyd files with a version and platform tag (for example, â.cp35-win32.pydâ) will now be loaded in preference to those without tags.

**(For information about older versions, consult the HISTORY file.)**
